---
title: "Git as the Shared Brain: My Hybrid Agentic Dev Workflow"
date: 2026-02-18
summary: "Running two AI coding agents across phone and terminal without losing context, and why Git is a better coordination layer than conversation history."
draft: false
---

My dev workflow now involves at least two AI agents and no context cliff. One runs on my phone. One runs at my terminal. They've never had a conversation with each other, and they don't need to.

The standard mental model for multi-agent coordination is wrong. Most people assume agents need shared memory, shared context, or some kind of message-passing infrastructure. They don't. They need shared durable artifacts.

## The context cliff problem

When you work with an AI coding agent long enough, you hit the context cliff. The conversation grows stale. The agent starts to hallucinate, not because it's broken, but because you've pushed past the edge of what it can reliably attend to. It loses track of decisions made an hour ago, why certain files are structured the way they are, what was tried and abandoned.

This isn't just subjective experience. A Stanford study by Nelson Liu et al. (later published in the *Transactions of the Association for Computational Linguistics*) found that language models systematically perform worse when relevant information sits in the middle of a long context, even when that information technically fits. Chroma Research replicated this in 2025 across 18 models, including GPT-4.1, Claude 4, and Gemini 2.5, and found every single one showed measurable performance degradation as context length grew. They named the phenomenon "context rot."

The obvious response is to keep the context window fresh: start new sessions, summarize aggressively, use memory systems. But these are patches. They fix the symptom rather than the architecture.

The harder question is: what is context actually for? When an agent reads a file, why does it need to know *why* the file exists? What decisions shaped it? What alternatives were rejected? Usually that information lives in the conversation history. And conversation history doesn't persist.

But code does. Tests do. Commits do. Documentation does.

## Stateless persistence

Here's the reframe I've landed on: agents don't need shared conversation history if they share durable artifacts.

The repository already carries most of what an agent needs to reconstruct intent. The code itself encodes decisions. Tests encode expected behavior. Commit messages encode why changes were made. A `PLAN.md` encodes where things are headed. An `ARCHITECTURE.md` explains what patterns to follow and why.

I call this *stateless persistence*. The agents themselves are stateless; each session starts fresh, with no memory of the previous one. But persistence lives in the repo. Every commit is a checkpoint. Every diff is runnable context. Git becomes the shared working memory neither agent has on its own.

So instead of asking "how do I keep context alive across sessions," the question becomes "how do I make the repository complete enough that any agent can pick it up cold?"

Anthropic's own engineering team arrived at the same place. In a November 2025 post on long-running agent workflows, they documented a pattern using two agent roles: an initializer that creates a structured `claude-progress.txt` file on first run, and a coding agent that reads that file plus the git history in each subsequent session to reconstruct where things stand. No transcript sharing. No context summarization. Git history plus one plain-text state file.

## The workflow in practice

My setup uses two agents across two surfaces.

On my phone, I use OpenClaw via Telegram. OpenClaw (built by Peter Steinberger, who recently joined OpenAI to lead personal agent development) is an open-source AI agent gateway that connects AI models to messaging platforms. Steinberger made over 6,600 commits using his own agent in January 2026 alone, a single engineer shipping at team-scale throughput via Telegram commands. On mobile, it handles planning, task breakdown, and feature work. It writes tests, runs them, updates documentation, and won't commit until tests pass. When it's ready to push, it asks for my approval. I can review the diff on my phone, approve or reject, and move on.

At my terminal, Claude Code picks up exactly where OpenClaw left off, not because I pasted a transcript or summarized the session, but because the repository holds the state. The tests are there. The commit history is there. The planning doc is there. Claude Code reads the repo and has everything it needs.

What's interesting is that neither agent knows about the other. There's no agent-to-agent communication, no shared memory system, no orchestration layer. The coordination happens through the repository. Both agents read from and write to the same source of truth.

## The distributed systems parallel

This pattern has a name in distributed systems: event sourcing.

In event sourcing, the system's state is never stored directly. Instead, you store an append-only log of events. Any component can reconstruct the current state by replaying the log. Components don't share memory; they share the event log.

Git is an event log. Every commit is an immutable event. The current state of the codebase is always derivable from the commit history. Different agents (or humans) can independently reconstruct state by reading from the same log.

The architectural consequence is that agent handoffs stop being a memory problem and start being a state transition problem. Memory problems scale badly: they require passing ever-larger contexts, summarizing and losing information, or building complex memory retrieval systems. State transition problems scale well; you just make sure each commit leaves the repo in a clean, self-describing state.

This is why I'm skeptical of approaches that try to solve the multi-agent coordination problem with vector databases, context stores, or shared memory layers. These are valuable in some contexts, but they add infrastructure to work around a constraint that a well-structured repository already solves.

## Radical incrementalism as the forcing function

The workflow only works if the repository stays in a state where agents can pick it up cold. That requires discipline I've come to call *radical incrementalism*.

The rules:

- Plan in a plain-text file that lives in the repo
- Implement one small piece at a time
- Write or update tests, and run them before committing
- Update documentation in the same commit as the code
- Keep push behind a human gate

The last rule matters more than it looks. Requiring human approval to push isn't just a safety measure; it's an architectural constraint that forces proper increments. If an agent can push autonomously, it batches work, skips docs, merges untested changes. The approval gate means every push has to be reviewable, which means it has to be small and self-contained.

The commit history is the recap. When I pick up the next session, on phone or terminal, I don't ask the agent to summarize what was done. I read the last few commits. They tell me exactly what changed, why, and what the next logical step is.

The planning doc changes too. It's not a static requirements document; it's a living file that agents update as they work. When a feature is complete, they mark it done. When they deviate from the original plan, they note it. The planning file becomes a running record of intent, which is the most valuable context an agent can have.

## Scaling beyond small projects

The obvious question is whether this works at larger scale. It does, but the repository needs to carry more structure.

On a small project, the code itself is usually readable enough that an agent can infer intent. On a larger project that breaks down. You need clear interfaces: well-defined module boundaries with documented contracts, because agents that can't reason about an interface can't safely modify code behind it. You need architecture notes, a file (I use `ARCHITECTURE.md`) that explains the key patterns and the reasons behind them. Why is this service structured as a pipeline? Why does this module avoid shared state? What's the preferred way to add a new handler? Without this, agents make locally reasonable but globally incoherent decisions. You need CI as the source of truth: automated tests and linting that run on every PR, with no direct pushes to main. If CI passes, the increment is good. If it fails, nothing merges. And you need PR gates instead of direct pushes, so that for larger teams, human or agent, pull requests force explicit review before changes enter the shared state.

At that point, the repository stops being a folder of files and starts functioning as a coordination substrate, not just for the two agents in my personal workflow, but for any combination of agents and humans working on the codebase.

## GitHub's Continuous AI as validation

GitHub's recently previewed Agentic Workflows framework makes the same architectural bet. It embeds AI agents directly into GitHub Actions, running Copilot, Claude, or Codex to triage issues, diagnose CI failures, maintain documentation, and review PRs. The agents operate in sandboxed containers, have read-only access by default, and route any write operations through explicit approval gates.

GitHub calls this "Continuous AI," framing it as the agentic evolution of continuous integration. The framing is accurate. CI works because the repository is the source of truth. Test results, build artifacts, deployment status are all attached to commits, not to individual developers' sessions. Continuous AI works for the same reason: agents don't need persistent context if they have access to a complete, well-structured repository.

It's a different bet than building purpose-built multi-agent memory systems. It assumes the existing infrastructure of version control, CI, and code review is already sufficient for agent coordination, if you're willing to treat the repository as a first-class artifact rather than just a place to store code.

Thomas Dohmke, former GitHub CEO, is building a company called Entire on exactly this thesis. His first product, Checkpoints, records prompts, tool calls, and reasoning traces alongside each git commit, turning the commit history into a complete audit trail of both code and agent intent. The seed round closed at $60M, reportedly the largest in developer tools history. The bet is the same: the repository, not a separate memory layer, is where agent coordination lives.

## What changes

The practical shift isn't about technology. It's about discipline.

Most developers, myself included until recently, treat the repository as an output. A place where finished work lands. The commit message is an afterthought. The docs are separate from the code. The planning doc doesn't exist.

For agentic workflows to work well, the repository has to become an input. It has to be the medium through which intent is communicated, not just to collaborators, but to the agents doing the work.

This means writing commit messages that explain why, not just what. Keeping a planning doc that lives in the repo and gets updated as the work evolves. Writing documentation in the same commit as the feature, not as a separate ticket. Structuring code so that the interfaces are self-explanatory and the architecture is legible.

None of this is new advice. It's what good software engineering has always looked like. The difference is that with agents in the loop, the cost of not doing it is immediate: your agent loses the thread and starts guessing. The discipline that was previously nice-to-have becomes structural. And that turns out to be a decent argument for finally doing it right.

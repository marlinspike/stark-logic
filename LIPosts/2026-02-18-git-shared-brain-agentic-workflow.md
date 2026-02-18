---
title: "Git as the Shared Brain: My Hybrid Agentic Dev Workflow"
date: 2026-02-18
tags: [AI, development, agentic, git, workflow, continuous-ai]
status: draft
---

My dev workflow has at least two agents and no context cliff.

On my phone, I use OpenClaw via Telegram. It handles planning, task management, and most feature work. It writes tests, runs them, updates documentation, and won’t commit until everything passes. When it’s ready, it asks for permission to push.

At my terminal, Claude Code picks up exactly where I left off. Not because I pasted a transcript. Because the repository holds the state.I think of this as 𝘚𝘵𝘢𝘵𝘦𝘭𝘦𝘴𝘴 𝘗𝘦𝘳𝘴𝘪𝘴𝘵𝘦𝘯𝘤𝘦.

Agents don’t need shared conversation history if they share durable artifacts. The repo carries intent, implementation, tests, and docs. Every commit is a checkpoint. Every diff is runnable context. Git becomes the shared working memory neither agent has on its own.

The constraint creates a discipline I’ve come to value, 𝘙𝘢𝘥𝘪𝘤𝘢𝘭 𝘐𝘯𝘤𝘳𝘦𝘮𝘦𝘯𝘵𝘢𝘭𝘪𝘴𝘮. Plan in a plain-text file. Implement one small piece at a time, write or update tests and run them, update docs and then Commit and keep Push behind a human gate. No sprawling sessions, no recapping everything in chat. The history is the recap.

Can you build larger systems this way? Yes, but only if the repository holds more than code, and holds intent, boundaries, and decisions. You want clear interfaces, plenty of planning, architecture notes for patterns and choices. And of course, CI as the source of truth with PR gates instead of direct pushes.

At that point, the repo stops being a folder of files and starts behaving like a coordination substrate. GitHub is calling part of this direction “𝘊𝘰𝘯𝘵𝘪𝘯𝘶𝘰𝘶𝘴 𝘈𝘐,” with agents embedded inside GitHub Actions to triage issues, review PRs, and analyze CI failures. Different surface, same architectural shift. If the repository is treated as a state machine, agent handoffs stop being a memory problem and start being a state transition problem. That scales much further than just chat ever will.

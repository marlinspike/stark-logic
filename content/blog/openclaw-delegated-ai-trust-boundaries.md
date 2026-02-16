---
title: "OpenClaw and the Shift from Chat to Delegation"
date: 2026-02-05
summary: "I installed OpenClaw expecting a weekend experiment. What I found was something closer to delegated execution, and a clear lesson in why trust boundaries matter when your AI agent has shell access."
draft: false
---

I installed OpenClaw expecting a weekend rabbit hole. Tinker with it for a few hours, maybe automate a small task, move on. What actually happened was different. Within the first stretch of working with it, I had it doing real work asynchronously. Not "answer my question" work. Actual tasks I'd otherwise have to shepherd step by step.

That shift, from chat to delegation, is the thing worth paying attention to.

## What changed

Most AI tools I've used follow the same loop. You type a prompt, get a response, refine it, paste output somewhere, run the next step. The human is the orchestrator. You're managing the event loop manually, and if you walk away, nothing happens.

OpenClaw breaks that pattern. You hand it a task and go do something else. It plans, executes, hits errors, adjusts, reports back. The interaction model is closer to assigning work to a junior colleague than chatting with a bot.

The interface choice reinforces this. OpenClaw doesn't live behind its own UI. It plugs into the messaging apps you already use: Telegram, WhatsApp, Signal, Slack. I run mine through Telegram. That means I can message it from my phone while I'm away from my desk. "What did I decide about that architecture last week?" or "Pull together the notes from Tuesday's meeting." Because I take a lot of notes, it can actually work with what I have lying around. Notes stop feeling like a graveyard and start feeling like an asset.

The fact that it lives in a messaging app means the interaction cost drops to near zero. No context switch, no opening a separate tool. It just sits in the flow of how I already communicate.

## Memory is what makes delegation work

The deeper pattern here is memory. Most coding agents have what I'd call session amnesia. You burn through tokens, build up context, hit a wall or a timeout, start a fresh chat, and everything you built up is gone. Continuity becomes a discipline you force on the tool rather than something it provides.

OpenClaw handles this differently. It maintains state across sessions. It picks up habits. It remembers what you told it last week. When it hits a problem it can't solve with its existing capabilities, it can write code to extend itself, what OpenClaw calls "skills." Sometimes that skill is a small script. Sometimes it's a full integration.

This matters because useful delegation requires memory. If you have to re-explain context every time you talk to an agent, you haven't delegated anything. You've just added a middleman. The value comes when the agent accumulates understanding over time, the same way a good colleague does.

The memory architecture underneath is still evolving. Vector databases for semantic retrieval, structured state for facts that need to be authoritative, summarization for keeping context windows from blowing up. Each approach has tradeoffs. Carry too much context forward and the model gets distracted. Carry too little and it loses coherence. Get a bad fact into the summary and it poisons future behavior.

These are real engineering problems, not solved ones. But even now, the difference between an agent with persistent memory and one without is stark. One feels like a tool you use. The other starts to feel like a system you work with.

## The security problem is real

Here's where I stop being optimistic and start being careful.

OpenClaw's own security documentation is blunt about the threat model. If you give an agent the ability to run shell commands, read files, and send messages, you have created a new privileged surface area. This isn't theoretical. It's the actual architecture.

Snyk's security research in February 2026 put concrete numbers on the problem. They audited 3,984 skills from ClawHub (OpenClaw's skill registry) and found 76 confirmed malicious payloads designed for credential theft, backdoor installation, and data exfiltration. 283 skills, about 7% of the registry, contained critical security flaws that expose sensitive credentials. 91% of the malicious skills included prompt injection, a technique that manipulates the AI agent itself into sharing data or running commands it shouldn't.

If you installed a random ClawHub skill in the past month, there was roughly a 1-in-8 chance it contained a serious security flaw.

This is the npm supply chain problem all over again, except worse. A compromised npm package can run code in your build pipeline. A compromised OpenClaw skill can run code *and* read your files *and* send messages on your behalf *and* access whatever credentials you've shared with the agent. The permissions are just wider.

Snyk also documented the first coordinated malware campaign targeting OpenClaw users, with 30+ malicious skills distributed through ClawHub specifically designed to exfiltrate data. This already happened.

## How I actually run it

Given all that, here's my setup. I treat OpenClaw like a powerful machine that I've invited into my house, and I'm careful about which rooms it can access.

It runs on a dedicated Mac Mini, separate from my primary workstation and away from my password manager, crypto wallet, and primary email. A box with limited access and nothing worth stealing.

The credentials it has are minimal. A couple of shared passwords for specific services, access to Claude as its LLM backend, and not much else. If it needs more, that's an explicit decision, usually another API key or access to a specific data source. Each expansion of trust is deliberate.

I run it with sandboxing enabled. OpenClaw supports Docker-based tool sandboxing, where each agent's tools run in an isolated container with no access to the host filesystem by default. The sandbox workspace lives under `~/.openclaw/sandboxes`, separate from anything important.

Network isolation matters too. The Mac Mini sits on its own network segment. If OpenClaw gets compromised, the damage is contained to a machine with nothing valuable on it.

A friend of mine has his OpenClaw checking him in for flights automatically. I'm not that brave yet. The gap between "this is cool" and "I trust this with my airline credentials" is wide for me. Maybe I'll get there. But trust with autonomous agents should be earned incrementally, not granted upfront.

## Where this is going

What I take from all of this is mostly optimistic, but with caveats.

We're finally getting AI tools that feel less like chat and more like delegated execution. The interaction model is shifting from "I talk to a bot" to "I assign work to an agent." That matters. It's closer to having a capable, if sometimes unpredictable, assistant than having a fancy autocomplete.

But the security implications aren't something you bolt on later. Every capability you give an agent is a capability that can be exploited. The ClawHub numbers make this concrete: a 7% rate of critical flaws in a public skill registry is not an edge case. It's a systemic problem that the ecosystem needs to solve before this scales further.

The right mental model is progressive trust. Minimal permissions to start. Isolated hardware. Expand access deliberately, one capability at a time, as you build confidence. Watch what it does. Keep logs.

This applies to all autonomous agents, not just OpenClaw. The pattern will repeat with every agent framework that gains traction. The tools will get more capable. The security surface will get larger. And the people who treat trust boundaries as an afterthought will be the ones reading about their credentials on a security research blog.

I'd rather be the one reading the research than the one featured in it.

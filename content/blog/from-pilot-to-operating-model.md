---
title: "From AI Pilot to Operating Model"
date: 2026-01-15
summary: "88% of AI pilots never reach production. The ones that do share a pattern: they rebuild workflows from scratch instead of automating the old ones, and they treat the cultural shift as the actual work."
draft: false
---

Most organizations are stuck in what I'd call pilot purgatory. They run AI experiments, get promising results in a controlled setting, write up a deck, and then nothing happens. The pilot doesn't scale. The team moves on. Another experiment starts.

IDC found that 88% of AI proofs of concept never make it to production. An MIT report from 2025 put the failure rate for generative AI implementations at 95%. McKinsey says nearly two-thirds of organizations are still stuck in pilot mode. These aren't cherry-picked numbers. They're the baseline.

I've seen this pattern up close. Through Greenhouse, I work directly with leadership teams to move past it. What I've learned is that the gap between a successful pilot and a working operating model is almost never about the technology. It's about how the work gets done, and whether anyone is willing to change that.

## Automate the process or rebuild it

The most common mistake is treating AI as a faster version of the existing workflow. Take whatever people are doing manually, point an LLM at it, and expect speed. This almost never works at scale.

The reason is simple: most existing workflows were designed around human constraints. Humans are slow at certain things (reading 200 documents, cross-referencing data across systems) and good at others (judgment calls, context switching, handling ambiguity). When you automate a process designed for human limitations, you inherit all the bottlenecks and workarounds that were baked in to accommodate those limitations.

What works is rebuilding from first principles. I helped a team build an internal application that assembles complex research binders automatically. We didn't take their existing process and add AI to it. We sat down with the subject-matter experts, understood what the *output* needed to be, and then designed a new workflow where AI and data carried the load from the start.

The difference was dramatic. The old process involved researchers manually pulling sources, reading them, writing summaries, formatting them into binders, and sending them for review. The new process defined the output spec, let AI handle the assembly and summarization, and moved the humans to review and judgment. Same output quality. Fraction of the time. But it only worked because we didn't try to automate the old way of doing it.

MIT's own analysis backs this up. Their research found that 95% of AI investments don't deliver returns, often because organizations layer AI onto outdated workflows instead of rethinking the work itself.

## The role shift that nobody talks about

When AI handles the mechanical work, the human role changes. This sounds obvious in the abstract but it's disorienting in practice.

My own role on the Greenhouse project shifted from writing every line of code to operating at the architect level. I was setting system boundaries. Defining data contracts. Deciding where responsibility should live. Establishing expectations around failure and observability. Reviewing output critically rather than producing it directly.

Andrej Karpathy described this shift well when he said he's "not writing the code directly 99% of the time" and instead "orchestrating agents who do and acting as oversight." That matches my experience. AI is a capable, tireless junior engineer that can translate well-specified intent into implementation at machine speed. But it can't decide what to build, how to decompose the problem, or where the system boundaries should go. That's the human job, and it requires *more* skill than writing the code yourself, not less.

The market is reflecting this. Demand for senior architects has surged 30% while entry-level programming positions have dropped from 30% to 20% of total tech roles. The people who can specify intent clearly and validate output are becoming more valuable. The people whose primary skill was translating specifications into code are watching that skill get automated.

This isn't a comfortable thing to say. But pretending it's not happening doesn't help anyone prepare for it.

## Culture is the actual barrier

BCG has a rule of thumb for AI value creation: 10% comes from the algorithms, 20% from the technology and data infrastructure, 70% from transforming people, organization, and processes. That ratio matches what I've seen.

The technical problems with AI adoption, getting models to work, building pipelines, integrating with existing systems, are hard but tractable. You can hire for them. You can buy solutions. The cultural problems are harder because you can't outsource them.

Here's what I mean by cultural barriers, specifically:

Most engineers were taught that their job is to write code. Telling them their job is now to specify what code should do and review what gets generated feels like a demotion, even when it actually gives them more reach than they had before. Getting comfortable describing intent instead of micromanaging implementation goes against decades of training. It takes time.

Then there's the discipline problem. When AI generates code, the quality depends entirely on how well you specified what you wanted. Loose specifications produce loose code. This means investing in the boring stuff: type systems, interface definitions, acceptance criteria, observability hooks. The teams that skip this step end up with AI-generated code that works in demos and breaks in production.

Instrumentation is another one. When a human writes code, they carry implicit knowledge of how it works and where it might fail. AI-generated code has no such memory. You need explicit instrumentation from the start, or you end up with systems you can't debug. I've watched teams discover this the hard way, usually at 2am when something breaks and nobody understands what the AI actually built.

And then there's the speed adjustment. AI-assisted development compresses the feedback cycle. You go from specification to working code in minutes instead of days. That speed is only useful if you have the judgment to evaluate quickly, and the discipline to slow down when something doesn't look right.

## What actually moves organizations forward

The pattern I've seen work is this: leadership engages, but the conversation moves quickly from strategy into building. Builders and business owners are in the same room, not separated by layers of project management. The work is visible. The results are real.

When leaders see how quickly ideas turn into working systems, the conversation changes. It stops being about whether to adopt AI and starts being about how to reorganize around it. That's the shift from experimentation to operating model.

This is harder than it sounds, because it requires leaders to do something uncomfortable: participate in the technical work closely enough to understand what's actually happening. Not write code, but be in the room when decisions get made about system boundaries, data contracts, and what gets automated versus what stays human.

The organizations that are getting real value from AI aren't the ones with the best models or the biggest budgets. They're the ones where leadership understands that AI transformation is mostly a people problem, and where they're willing to sit with the discomfort of changing how work gets done rather than just adding new tools to old processes.

The technology has been ready for a while now. The question, as usual, is whether the organization is willing to change how it works. Most aren't. But the ones that do tend to move fast.

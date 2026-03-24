---
title: "The Work Between Systems"
date: 2026-03-24
summary: "Most enterprise software captures what happened. It doesn't capture the reasoning that connects the dots. That gap — the in-between work — is what AI can finally start to close."
draft: false
---

There's a description I keep coming back to when I think about how knowledge work actually operates inside large organizations: the human as interface.

Not as decision-maker. Not as analyst. As interface — the person manually bridging the gap between systems that don't talk to each other, pulling a report from one tool, cross-referencing it against something in another, opening a third tab to check context that lives in neither, and then synthesizing all of it in their head before they can do anything useful with it.

This is the daily reality for a lot of knowledge workers. Asana's Anatomy of Work Index (a survey of over 13,000 knowledge workers across eight countries) found that more than 60% of the workday goes to what they call "work about work": chasing status updates, searching for information, switching contexts, sitting in meetings to relay what one system knows to people who use a different one. That's not a productivity problem. The workers aren't slow or disorganized. The underlying systems just weren't built to share context, and someone has to carry it.

The framing that keeps getting used to describe this is "systems of record." We've built excellent systems of record. Salesforce knows your customers. Workday knows your headcount. ServiceNow knows your tickets. But knowing, in each of those systems, is siloed. The reasoning that connects them — the judgment call about what a pattern in the customer data means for a staffing decision, or what a cluster of tickets implies about a product's stability — has always lived outside the software. In the analyst's head. In the email thread. In the meeting where someone who knew both systems happened to be in the room.

## Why software never captured this work

There's a deeper explanation for why this connective tissue never made it into systems. The problem isn't just technical. It's a representability problem.

Michael Polanyi gave the clearest articulation of it in 1966: "We can know more than we can tell." His concept of tacit knowledge — the kind of expertise that can't be fully verbalized, the judgment that a skilled professional exercises without being able to write down the rules — is precisely what traditional software can't encode. To build a system that does something, you have to be able to specify what it should do. For a large class of knowledge work, that specification is impossible, not just difficult. The expert doesn't know the rules they're applying. They know the outcome when they see it.

David Autor called the practical implication of this "Polanyi's Paradox" in a 2014 NBER paper: computers substitute well for workers doing routine, codifiable tasks and amplify the value of workers doing everything else. The tasks requiring "flexibility, judgment, and common sense" proved hardest to automate because you can't reduce them to conditionals.

Robotic Process Automation tried. RPA treats software interfaces the way humans use them — click this button, copy this field, paste it here — and it works for exactly that. Fixed layouts, structured data, deterministic rules. The canonical failure mode: change the vendor's invoice template slightly and the bot breaks. Not because the information changed in any meaningful way, but because the system was mimicking physical steps rather than understanding the task. Industry studies consistently put RPA failure rates above 80% without AI augmentation. The tasks where RPA fails are the ones where judgment is required: unstructured inputs, exception handling, decisions that depend on context the bot doesn't have.

The CSCW research community has studied the invisible version of this problem since the early 1990s under the name "articulation work" — the meta-level coordination labor required to make collaborative work happen, the kind that shows up in no formal workflow. Leigh Star and Anselm Strauss documented it: the informal, in-between activity that holds distributed work systems together but appears in no system of record. It's been there the whole time, unmeasured and unrepresented, because we had no way to represent it.

## What's actually changed

The reason this is worth revisiting now isn't that the problem is new. It's that the tools available to address it are.

What language models introduced, and what agentic systems are starting to make useful, is the ability to operate on unstructured information at scale. Not to execute a fixed sequence of steps, but to reason across disparate sources, synthesize context, and make judgments under ambiguity. That's the capability that was always missing from the automation stack.

Vendors are now calling this a "reasoning layer" — software that sits above systems of record, pulls from multiple sources, and applies inference to connect what they individually know. Salesforce calls it the Atlas Reasoning Engine. ServiceNow built a domain-specific reasoning model with NVIDIA. Glean built it as a knowledge graph that supports multi-step agent queries across the full enterprise SaaS stack. The implementations differ, but the structural insight is the same: the intelligence needs to live somewhere above the data stores, able to see across them, rather than inside any one of them.

This isn't replacing systems of record. It's building what was always missing: the layer that can do the work the human was doing between them.

The distinction from RPA matters. RPA automates tasks — it executes steps. A reasoning layer automates decisions; it handles what to do and why, not just the mechanical sequence of doing it. That's a different category of software, and it's a category that didn't really exist before.

## Where this gets hard: the public sector case

If you want to see the full weight of this problem, look at government.

The GAO's most recent assessment of federal IT found that 79% of the $337 billion annual federal IT budget — roughly $83 billion — goes to operating and maintaining existing systems, not modernizing them. The IRS still runs applications written in COBOL that are over 60 years old. The FAA has 105 of 138 legacy air traffic control systems flagged as unsustainable. These aren't edge cases. They're the norm.

The consequence is that data silos are deeply entrenched. Information lives in systems that don't interoperate, built to different standards, owned by different agencies, sometimes running on infrastructure that lacks modern APIs entirely. The people who need to reason across these systems — epidemiologists tracking outbreak patterns, analysts building situational awareness across multiple data streams — are doing the integration work manually. They're the interface.

This is the environment we've been working in with AI Greenhouse. A recent engagement with the CDC's Public Health Informatics Office, alongside partners at Booz Allen, put this directly in focus. CDC's data landscape involves case reports, laboratory results, demographic data, and signals from emergency departments, spread across systems with different standards, different cadences, and historically limited interoperability. The work of getting from raw data to a useful epidemiological picture has always required human analysts to bridge those gaps.

What made the CDC engagement meaningful wasn't deploying a chat interface or automating a report. It was getting specific about which parts of the analyst's work were the stitching — the connective tissue that existed only in their head — and whether we could encode any of that reasoning computationally. Not fully replace it. Identify the parts that were tedious rather than expert, and take those off the plate so the actual expertise could go further.

Booz Allen's position here is structural: they hold the $1.7 billion CDC Data Modernization Accelerator contract, making them the delivery partner for the infrastructure that PHIO and OPHDST are building toward. When the data infrastructure evolves, the reasoning capabilities that can run on top of it evolve with it. The two have to move together.

## The part that isn't automation

Here's where I'd push back on how this is usually framed.

The dominant narrative around AI in knowledge work is about efficiency: do the same work faster, reduce headcount, improve throughput. In high-stakes domains, that's the wrong goal.

The analysts at the CDC doing outbreak surveillance aren't primarily slow. The bottleneck isn't their typing speed or the time it takes to generate a table. The bottleneck is the 60-70% of their time going to work that isn't analysis — tracking down data that lives in a different system, reconciling formats that don't match, pulling together the context required to even start asking the right question. That's the stitching work. That belongs to the software layer, not the epidemiologist.

When you offload that, you don't get a slightly faster version of what you had before. You get the analyst focused entirely on the part that actually requires an expert: interpreting the signal, seeing the pattern that doesn't fit the model, making the call that determines whether something is a local anomaly or the beginning of something larger. That judgment isn't going away, and in public health contexts, getting more of it applied in the right places has consequences that don't show up in a productivity metric.

This is the framing I keep coming back to when I describe what AI Greenhouse is trying to do. Not automation as a replacement for expertise. Automation as the infrastructure that makes expertise go further, by clearing the path of everything that was consuming it unnecessarily.

## What this requires in practice

There's a practical constraint that gets underestimated. You can't build a reasoning layer on top of data that isn't accessible, isn't trusted, or isn't semantically coherent.

The 60% of work time lost to coordination doesn't exist in a vacuum — it exists because the underlying data infrastructure is fragmented. Before an AI system can reason across disparate sources, those sources need to be connected in some way, even loosely. Before an agent can synthesize context, the context has to be findable. Building a reasoning layer and doing data modernization aren't sequential steps. Every increment of better data infrastructure expands the surface area where reasoning can operate.

In government contexts, where the legacy constraints are most severe, the path forward looks less like deploying a single AI product and more like progressive capability-building: identify where the manual synthesis work is concentrated, connect the specific data sources that feed it, and bring a reasoning layer in on top of that specific slice. Not a grand unified platform. Narrow, high-value interventions that build trust incrementally and demonstrate that the reasoning layer can be held accountable.

The operational discipline around how you do this in high-stakes environments with real legacy constraints is still early. But the framing is right: the goal is encoding the reasoning that connects systems, not just the steps between them.

The work that was never representable in software — the judgment work, the synthesis work, the work between systems — that's what we're finally getting traction on.

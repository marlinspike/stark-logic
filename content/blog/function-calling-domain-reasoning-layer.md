---
title: "Stop Wiring Apps Together. Build a Domain Reasoning Layer Instead."
date: 2026-02-19
summary: "Most teams using AI function calling are optimizing at the wrong level. The wins aren't app-by-app; they're domain-wide. Here's what that actually means, why legacy systems make it hard, and how the infrastructure is catching up."
draft: false
---

The teams I've watched get the most out of AI function calling have something in common. They stopped thinking about which app to connect next and started thinking about what the model needs to reason across an entire domain.

That's a different problem. And solving the right one changes what you build.

## The integration layer is getting a reasoning upgrade

For two decades, the standard answer to "how do we connect our systems" was some flavor of middleware (ESBs, workflow engines, message brokers). These tools moved data between systems by following rules someone wrote in advance. If this condition, call this endpoint. Route this message type to this queue. Logic defined explicitly, maintained by engineers, brittle when the world changed.

Function calling gives the LLM the decision role that used to belong to those routing rules. The model receives a request, interprets intent, decides which tools to invoke, reasons over the results, and determines whether to call more tools or return an answer. That's not rule-based routing. It's contextual reasoning over a set of capabilities.

The practical difference matters. A rule-based router fails when the request is ambiguous or slightly outside the defined cases. An LLM reasons through ambiguity. It can infer that a user asking "what's the status of my order and can I change the delivery address" needs two operations sequenced in a specific order, even if no one programmed that case explicitly.

But this reasoning capability is only as good as the tools it reasons over. Which is where the domain question becomes critical.

## Why app-by-app fails

Most function calling implementations start the same way: pick an application, expose a few endpoints as functions, wire them up. The model can now do things in that app. Ship it.

The problem is that the gains stay local. You've added an AI interface to one system. Users talking to that interface can do things in that system. But most real business tasks don't live inside one system.

A customer service workflow touches a CRM, an order management system, a returns platform, and probably a knowledge base. An analyst running a reporting workflow hits data warehouses, financial systems, and scheduling tools. The interesting work, the work where an AI reasoning layer actually changes what's possible, spans those boundaries.

When you optimize app-by-app, you end up with a collection of isolated AI-augmented apps rather than a coherent reasoning layer. The model has partial context in each interaction. It can't coordinate across systems it doesn't know about. You've made each app slightly better. You haven't changed what the domain can do.

McKinsey's December 2024 analysis of AI-augmented IT modernization puts a number on what changes when you approach this differently: 40-50% faster modernization timelines and a 40% reduction in costs from technology debt. Those numbers come from client work, not a controlled study, so they carry the caveats that always come with consulting firm statistics. But the directional logic holds from what I've seen: once you have a coherent reasoning layer spanning a domain, every workflow in that domain can benefit. The investment in the domain layer isn't paid back once. It compounds.

## What "domain-level" actually means in practice

A domain, in this context, is a bounded area of business activity with its own vocabulary, data, rules, and systems. Order management is a domain. Customer identity is a domain. Financial operations is a domain. Each has its own coherent set of capabilities that belong together.

Building function calling at the domain level means designing your tool library around what the domain can do, not around what each application exposes. The functions represent business operations in the domain's own vocabulary. `get_customer_order_history` rather than `query_orders_db`. `cancel_and_rebook_reservation` rather than separate `cancel` and `book` calls that only make sense together.

LangGraph's documentation makes the same point about agent design: "an agent is more likely to succeed on a focused task than if it has to select from dozens of tools." The practical pattern it recommends is a router that classifies incoming requests and hands them to specialized subagents, each with a focused set of domain-appropriate tools. A billing agent, a logistics agent, a support agent, each with its own toolset, coordinated by a top-level orchestrator that knows which domain owns the problem.

This is where MCP (Model Context Protocol) becomes infrastructure rather than tooling. Anthropic released MCP in November 2024 as an open standard for connecting LLMs to external tools and data sources. By end of 2025 it had 97M+ monthly SDK downloads, 10,000+ active public servers, and adoption from OpenAI, Google, Microsoft, and every major AI platform. In December 2025 Anthropic donated it to the Linux Foundation's Agentic AI Foundation, which is how you signal "this is infrastructure, not product."

What MCP does for domain design is provide a standardized way to register and discover the tools in a domain. Your domain's capabilities become discoverable by any agent that speaks MCP. The domain layer gets an interface contract, which means domain tool libraries can be built once and consumed across multiple agents and applications.

## The legacy problem

Here's where the concept runs into reality. Most enterprise domains aren't running clean microservices with well-defined APIs. They're running systems that have been accumulating logic for decades.

Legacy codebases have functions that do ten things. Services that only work when called in a specific order that exists nowhere in documentation. Side effects embedded so deep in the call stack that nobody's sure what triggers them. APIs designed for a specific UI that assumes context no other caller would have.

Expose that directly through function calling and you'll hit walls fast. The model can't reason reliably over unpredictable behavior. If calling function A sometimes also modifies state B in a way that depends on which other functions ran recently, the model has no basis for reasoning about that. It will make decisions that look correct based on the tool descriptions but fail in execution.

The McKinsey report makes this point directly, in a way that's easy to miss: "converting old code into modern tech languages simply transports your tech debt from a legacy system into a modern one." Wrapping legacy behavior in a function doesn't make that behavior LLM-friendly. The model gets an interface to chaos.

What does work is the harder path. Break large functions into smaller, single-purpose units. A tool named `manage_customer_account` that handles fetching, updating, and deleting is worse than `get_customer_account`, `update_customer_contact_info`, and `close_customer_account`, even though it's more work to write. The model needs to select tools accurately from their descriptions. Atomic tools with unambiguous names make that selection tractable.

Document intent, not just behavior. The description field on a function is how the model decides whether to call it. A description that says "queries the accounts table" is worse than one that says "retrieves a customer's account details by customer ID, including contact information, account status, and associated order history." The model needs to understand what the function *accomplishes*, not just what it does technically.

Test before you expose. You need confidence that changes to one function hold across the domain, not just for the service you just touched. In legacy systems with tangled dependencies, a change to one function often has effects elsewhere. You need test coverage broad enough to catch those ripples before they become production incidents that the model unknowingly triggers.

Roll out incrementally. Don't rewire the entire domain at once. Start with the most stable, cleanest subsystem. Get the design right there. Build the patterns your team knows how to apply. Then expand.

## What makes a function LLM-friendly

A few things matter most when designing tools for LLM consumption.

Single responsibility: one function, one action. If it does more than one thing, split it.

Verb-first naming. Names that describe the action: `search_inventory`, `calculate_refund_amount`, `send_confirmation_email`. Not `inventory_lookup` or `refund_calc`. The verb is how the model maps intent to tool.

Flat, simple parameters. Deeply nested JSON input objects are hard for the model to construct correctly. Flatten where possible. If a parameter requires complex structure, consider whether the tool can handle that complexity internally.

Consistent output schemas. The model needs to reason over tool results and pass them to other tools. If your tools return inconsistent structures (sometimes a list, sometimes a dict, sometimes an error in an unexpected format) the reasoning chain degrades.

Honest error behavior. If the tool fails, return a structured error that tells the model what went wrong and whether it's retryable. The model needs to know whether to try again, try a different tool, or escalate.

## Infrastructure work, not integration work

The teams that approach function calling as an integration project (connect app X to the model) tend to get narrowly useful results. The model helps with things that live in app X.

The teams that approach it as infrastructure work, building a domain reasoning layer that knows how to act across an entire bounded context, end up with something that compounds. Every new workflow in the domain gets the reasoning layer. Every improvement to the tool library benefits every agent that uses it. The investment is in the domain, and the domain is shared.

That's a different kind of project. It takes longer to set up. It requires thinking about your domain's vocabulary and capabilities as a coherent design problem. It almost always surfaces ugly things about your legacy systems that you'd rather not deal with.

But it's the only version of this that changes what your domain can actually do, rather than just adding an AI interface to what it already could.

The question isn't which app to connect next. It's which domain you're willing to build a real reasoning layer for.

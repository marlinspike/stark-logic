---
title: "The Wrong First Question in Azure AI"
date: 2025-09-19
summary: "The Responses API going GA in Azure AI Foundry isn't primarily a model story. It's a runtime architecture decision that most teams make too late."
draft: false
---

Every AI project starts with the same question: which model?

GPT-4o or Llama? o3 or Mistral? Which benchmark wins? Which is cheaper at scale?

It's not a bad question. But it's usually the second or third decision that matters, not the first. When the Responses API reached general availability in Azure AI Foundry in August 2025, most of the coverage focused on what models it supports. That missed the more important story.

The more important question is: what runtime are you building on?

## What the Responses API actually does

The Responses API is Azure OpenAI's stateful inference API. Microsoft describes it as unifying the Chat Completions API and the Assistants API into a single experience, and that framing is useful if you've been working with either of those, but it undersells what changed.

With the older Chat Completions API, you're responsible for conversation state. Every request is stateless. You get a response, and on the next turn you send the full conversation history back: every prior message, every tool call, every output. Your application has to store this, manage it, trim it when it approaches the context window limit, and pass it correctly on each call. It works. It's also a surprising amount of plumbing for what feels like it should be a solved problem.

The Responses API offloads that plumbing. You get back a `response.id`. On the next turn, you pass `previous_response_id` and the service reconstructs the context. You never send the full conversation history yourself. The API stores it, retained for 30 days by default and deletable on demand. Multi-turn conversations become a chain of IDs.

The practical difference is real. A team building a conversational agent with the Chat Completions API typically ends up writing code to:

- Store and retrieve conversation history (a database, a cache, something)
- Serialize and deserialize message arrays correctly
- Handle context window limits (deciding which messages to drop when approaching the limit)
- Manage tool call outputs and thread them back into the next request in the right format
- Track thread state across user sessions

None of this is technically hard. But it's all accidental complexity, work that doesn't differentiate your product. The Responses API eliminates most of it.

Beyond state management, the API has native built-in tools: File Search (with vector store integration), Code Interpreter (sandboxed Python execution), Function Calling, Computer Use, Image Generation, and remote MCP server connections. These all compose within a single API call. You're not gluing together separate services; the orchestration happens inside the API.

## The real architecture decision

Here's where the LinkedIn post I originally wrote undersold the stakes.

Choosing your runtime in Azure AI Foundry isn't just a developer experience choice. It's an architectural constraint that affects what you can change later. The three primary runtimes have meaningfully different properties: Responses API, Foundry Agent Service, and Azure AI Inference.

**Responses API** is the right fit if you're building single-agent, tool-using experiences on OpenAI models and want managed state out of the box. The API is opinionated. It handles the orchestration layer for you, which saves real work for the use cases it fits. The constraint is that it only works with Azure OpenAI deployments, specifically models in the OpenAI family. Mistral, Llama, DeepSeek, Cohere, and other third-party models hosted in Foundry's model catalog don't route through the Responses API.

**Foundry Agent Service** is the managed orchestration layer for more complex scenarios: multi-agent systems, enterprise connectors (SharePoint, Microsoft Fabric, Bing Search), and applications that need Azure's enterprise compliance and RBAC surface. Since November 2025, Agent Service has used Responses API as its default backbone via the Azure AI Projects client library. So if you're on Agent Service, you're already on Responses API under the hood. Agent Service went GA in May 2025, a few months before the Responses API itself.

**Azure AI Inference** is the flexible, multi-provider path. A single endpoint (`https://<resource>.services.ai.azure.com/models`) exposes Mistral, Llama, DeepSeek, Cohere, Phi, and a growing catalog of models using a common inference schema. No managed state. No built-in orchestration. You compose your own stack. The upside is model flexibility and portability: switching models is a config change, not a code change. The downside is that the plumbing the Responses API handles for you, you're writing yourself.

## The model provider split

The split between Responses API and Inference isn't arbitrary. It reflects a real distinction in how Azure hosts and manages these models.

The Responses API is an Azure OpenAI service. It's built specifically around OpenAI's model family, and the stateful infrastructure (response storage, thread management, built-in tools) is tightly coupled to that deployment type. Non-OpenAI models in Foundry's catalog are either hosted by their providers on Azure infrastructure (Mistral, Meta Llama, Cohere) or self-hosted as managed endpoints (open-weight models from Hugging Face, etc.). They access Azure through the AI Inference endpoint, not through Azure OpenAI.

This means the runtime choice and the model choice are coupled, at least in one direction. If you want Mistral Large or Llama 4 Maverick, you're building on Inference, and the orchestration layer is yours to own. If you want the managed state and built-in tools of the Responses API, you're committing to the OpenAI model family on Azure.

This split isn't permanent. The stack is moving, as it always is. But as of today, it's a hard constraint worth understanding before you're several months into a build.

## The migration pressure

There's a timeline forcing decisions that wasn't part of the picture a year ago.

OpenAI announced the Assistants API deprecation in August 2025. The sunset date is August 26, 2026. That's a year-long runway, which sounds comfortable until you consider that most teams don't start migrations until they feel urgency. If you're running production workloads on the Assistants API (persistent threads, managed tool use, file search), you have a concrete deadline to move to either the Responses API or Foundry Agent Service.

The Responses API is the natural migration target. It was explicitly designed to replace the Assistants API, offering the same stateful conversation model and built-in tools with a cleaner interface. Microsoft's own documentation for the Assistants API now recommends Foundry Agent Service as the current GA option, and the v1 API (which the Responses API requires) supersedes the older versioned API scheme.

If you're starting a new project today, the Assistants API shouldn't be on the table. Not because it's broken. It still works. But you'd be building on something with a known end date.

## Making the call

When scoping a new AI workflow, I work through a few questions before touching model benchmarks.

The first is whether you need stateful multi-turn conversations. If yes, and you're on OpenAI models, start with the Responses API or Agent Service. If you need stateful conversations but also need model flexibility, you'll roll your own state layer. Budget for that explicitly — it's real work.

The second is whether you need built-in orchestrated tools. File search, code execution, and computer use are non-trivial to implement correctly. On OpenAI models with the Responses API, you get these out of the box. On Inference, you're integrating them yourself or through a framework like LangChain or LlamaIndex.

The third is model portability. The Responses API locks you to OpenAI models on Azure. The family is broad (GPT-4o, GPT-4.1, o3, the GPT-5 series), so this isn't a narrow constraint. But if your team has a concrete reason to hedge across providers — cost, capability gaps, compliance requirements, or the option to self-host — building on Inference keeps that door open.

If you need SharePoint, Microsoft Fabric, or Bing grounding, those connectors live at the Agent Service level, not at the raw Responses API level. That's the right layer for any serious enterprise integration surface.

And single-agent experiences fit naturally on the Responses API. Multi-agent coordination, where agents call other agents and delegate tasks, is what Agent Service's "Connected Agents" feature is designed for.

## What this means in practice

The model conversation isn't going away. Benchmarks matter. Cost per token matters. Capability gaps between model families are real. But those decisions play out differently depending on the runtime layer you've already committed to.

Teams that answer the runtime question first — and make it deliberately rather than by default — avoid the most expensive rework. Not because the runtime can't change later, but because it's load-bearing. The state management patterns, the tool integration approach, the authentication model, the client SDK: these all flow from the runtime choice. Changing it late means rebuilding the foundation.

The Responses API going GA is a concrete signal that Microsoft considers this layer stable for production. The model catalog in Azure AI Foundry is genuinely broad now. The engineering decision is mostly about what you're trading: stateful and opinionated on OpenAI models, or flexible and composable across the wider catalog.

Both are defensible. Make the call before you're too deep into the build to make it cleanly.

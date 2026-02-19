---
title: "The Compliance Gate Is Open. Now Build Actual Architecture."
date: 2026-02-19
summary: "Azure OpenAI's FedRAMP High P-ATO cleared the biggest compliance bottleneck federal AI teams faced. What comes after that clearance is harder, and most teams aren't building it right."
draft: false
---

When Azure OpenAI received its FedRAMP High Provisional Authorization to Operate from the Joint Authorization Board in August 2023, federal teams split between relief and wait-and-see. A year and a half later, the authorization mattered, but not for the reason most announcements made it sound.

The headline was always compliance. The actual unlock was architecture.

## What the P-ATO actually gives you

The FedRAMP High JAB P-ATO covers the Azure OpenAI platform at the infrastructure level: roughly 410 NIST SP 800-53 controls across 18 control families. When an agency builds on Azure OpenAI, it inherits those controls from Microsoft. For a PaaS service like this, inheritance can cover 60% or more of your total control set.

That's not nothing. A typical agency ATO built from scratch takes 12 to 18 months. Building on a FedRAMP High-authorized platform, with strong inheritance and a well-prepared System Security Plan, can compress that to six to twelve weeks in the best cases. You're not starting from zero on access control, system integrity, contingency planning, or a dozen other families that Microsoft has already had independently assessed by a 3PAO.

But here's what the authorization doesn't give you: a free pass.

Agencies still need their own Authority to Operate. They still need a Customer Responsibility Matrix that explicitly documents which controls are inherited and which are theirs. They still need to address their own data handling policies, application-layer security, and access management. For high-impact AI under OMB M-25-21, the kind that touches benefits determinations, enforcement decisions, or rights-impacting workflows, they need pre-deployment testing, impact assessments, and ongoing monitoring as well.

The P-ATO cuts the runway. It doesn't eliminate it.

## The cross-cloud architecture nobody explains clearly

What Azure Government agencies at IL4 and IL5 actually had to do to use Azure OpenAI is the more technically interesting piece, and it doesn't get enough coverage.

Azure Government and Azure Commercial are physically and network-isolated environments. You cannot ExpressRoute-peer a Government VNet directly into Commercial. They're treated as distinct clouds, by design.

So how does a DoD agency running at IL5 use Azure OpenAI, which until September 2024 only existed in Commercial?

The answer involves Microsoft's global backbone network. The architecture works like this: the agency application and all government data stay in Azure Government. When a request is made to Azure OpenAI, the query travels from Azure Government over Microsoft's private backbone to the Commercial endpoint, never touching the public internet. The response comes back the same way. Microsoft maintains a zero-retention policy for these federal queries: no government data is stored or logged in the Commercial environment after the response is returned.

The queries are just text. The data stays where it lives.

In September 2024, Azure OpenAI (including GPT-4o) received its FedRAMP High authorization for Azure Government directly, plus DoD DISA IL4/IL5 provisional authorization. By January 2025, GPT-4o cleared for Top Secret (IL6) in Microsoft's government cloud. The cross-cloud architecture was a bridge that's now largely been superseded for most use cases; Azure Government agencies can call Azure OpenAI endpoints in their own environment directly. But the underlying pattern is worth understanding: AI inference sitting in a different trust boundary than your data, connected via a private backbone, shows up in other forms as architectures get more complex.

## The acceleration is real

I want to put some numbers behind what "agencies are moving" actually means, because the GAO report that's been cited in most coverage is more interesting than the headline.

Across 11 federal agencies with publicly available use case inventories, generative AI use cases went from 32 in 2023 to 282 in 2024. That's a 9x increase in a single year. Total AI use cases, including traditional ML, predictive analytics, and NLP, nearly doubled from 571 to 1,110. Over 1,700 AI use cases were publicly disclosed across all agencies for 2024.

These aren't pilots. The VA went from 1 generative AI use case in 2023 to 27 in 2024. The IRS has 61 active use cases. Treasury more than doubled its inventory.

What's actually in those use cases matters more than the count. The IRS deployed a synthetic data engine to simulate fraud scenarios and test claims processing. OPM received an $18.3 million Technology Modernization Fund award in December 2024 for a two-year project to modernize its 40-year-old COBOL-based retirement system using AI. SSA is looking at options for its 60 million lines of COBOL that underpin retirement and disability claims processing.

These are not chatbots on top of systems. These are systems-level interventions.

## What "LLMs as middleware" actually means

LLMs moving from the surface layer into the integration layer is happening at exactly the places you'd expect, given the constraints of federal IT.

Federal agencies face a structural problem. Their most important data is locked in legacy systems that predate modern APIs by decades: COBOL on IBM mainframes, FORTRAN-based scientific workloads, legacy databases without REST interfaces. These systems work. They encode decades of business logic. And they're almost impossible to replace wholesale.

Traditional middleware handled this by writing explicit rules: if this message type arrives, transform and route to this endpoint. ESBs, workflow engines, message brokers. The approach requires engineers who understand both sides of the integration and can maintain the routing logic as systems evolve. That's expensive, and the logic gets brittle.

What changes with LLMs in the integration layer: you're replacing rule-based routing with reasoning. The LLM receives structured inputs, understands context, calls APIs on legacy systems, reconciles outputs from multiple sources, and produces a response or takes an action. You're not writing transformation rules. You're defining what the model needs to understand about each system it can call.

For a claims processing workflow at VA or SSA, this might look like a model that queries multiple backend databases (medical records, benefit eligibility systems, payment history), understands a caseworker's question in natural language, retrieves relevant information, flags inconsistencies, and surfaces a recommendation with citation to the source records. The legacy systems don't change. The model mediates between them.

OPM's COBOL modernization project takes a different approach: use AI to translate COBOL directly into Python or Java, then migrate to modern cloud infrastructure. The vendor will refactor and convert legacy retirement benefits and financial systems on IBM Mainframe using AI-assisted code translation, then rearchitect to a modern DBMS in OPM's Azure cloud. Both patterns, AI as mediating layer and AI as translation engine, are valid. They solve different problems, and they carry different risks.

## Where the hard problems actually are

### COBOL isn't just code

The 60 million lines of COBOL running SSA's claims processing systems represent decades of edge cases, regulatory changes, and fix-in-place logic that was never formally documented. An LLM-translated version of that code may silently discard or misinterpret edge cases that affect real benefit claimants in unusual situations. There's no comprehensive test suite to catch those errors, because legacy systems were never built with automated testing in mind.

DOGE's stated plan to "rewrite the SSA codebase in months" misunderstands this as a software problem. It's a knowledge problem. Approximately 30% of SSA's CIO organization is retirement-eligible. The institutional knowledge walking out the door as those people leave is at least as important as the COBOL itself. Rushing LLM-assisted translation before that knowledge is captured and encoded into validation tests is how you build systems that look modern and fail silently.

### The ATO is not the design

FedRAMP 20x, announced by GSA in March 2025, promises to compress authorization from 18+ months to weeks via automation and commercial framework alignment. The first pilot participants achieved authorization in roughly three months. That's a real improvement.

But faster compliance processing doesn't make the underlying architecture decisions easier. Where does the model sit relative to your data? What happens when a model returns an incorrect inference that affects a benefit determination? Who reviews high-impact AI decisions, and what does that review process look like technically? OMB M-25-21 requires pre-deployment testing and impact assessments for high-impact AI, with a compliance deadline of April 2026. Most agencies are still working out what "pre-deployment testing" means in practice for systems this complex.

### Testing the integration layer

The model itself is relatively well-understood. It has benchmark scores, safety evaluations, red team results. The hard part is testing what happens when the model calls a legacy API that returns an unexpected format, or when two source systems return inconsistent data, or when the prompt structure interacts badly with a specific class of inputs. That testing has to happen at the integration layer, and most agencies don't have mature frameworks for it yet.

## What I'm actually watching

FedRAMP 20x prioritization of AI cloud services is worth tracking. GSA has committed to expedited authorization for conversational AI systems with demonstrated agency demand and GSA Multiple Award Schedule presence. With the IL6 authorization going through in early 2025, the compliance ceiling for AI tools in government has effectively cleared for most use cases.

The bottleneck shifted. It's now architecture, integration, and validation, not authorization.

The teams making real progress treat the LLM as a component with defined inputs, outputs, and failure modes rather than a magic box placed at the end of a workflow. They build retrieval layers that connect the model to authoritative data sources. They work out what "human in the loop" means operationally before writing it into a policy document. They write system prompts that encode the specific business logic their domain requires, not generic instructions.

The compliance gate is open. What gets built on the other side of it is the actual work.

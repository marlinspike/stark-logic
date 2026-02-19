---
title: "An 18-Month ATO Doesn't Make Software Secure. Here's What Does."
date: 2025-07-12
summary: "The traditional DoD ATO process was sold as a security control. It was actually a snapshot, useful once and immediately aging. SWIFT changes the fundamental model, and the industry response tells you everything about how ready the field is."
draft: false
---

There's a phrase I've heard repeated in DoD acquisition circles for years: "We can't rush security." It was the implicit justification for an authorization process that could take 18 months and cost more than $3 million before a single line of code ran in production for the warfighter.

Here's the problem with that framing. The traditional Authority to Operate process wasn't security. It was a photograph of security, accurate at the moment it was taken and degrading from that instant forward.

Acting Pentagon CIO Katie Arrington put it more bluntly in 2025, calling the Risk Management Framework "archaic" and "a bunch of paperwork." That's the kind of language you use when you've watched a well-intentioned process calcify into something that protects the process more than the systems it was supposed to secure.

## What the old model actually did

The RMF-based ATO worked like this: a team of security assessors manually reviewed documentation, verified controls, and issued an authorization typically valid for one to three years. During that window, continuous monitoring was supposed to happen, but "continuous" often meant monthly scans, or quarterly ones, or whenever someone had cycles.

The point-in-time problem is structural, not a failure of implementation. A system approved in January reflects the threat landscape, the dependency graph, and the configuration baseline of January. By March, three of your dependencies have published CVEs. By June, your container image is running a kernel version with a known privilege escalation. By the following January, when you're up for reauthorization, you've been flying with an unknown attack surface for most of your authorization window.

The Atlantic Council made exactly this diagnosis in its analysis of DoD's software approval process: one-time ATO reviews "are not useful for tracking a system's long-term security posture" and "leaving a system approved for this long without further review increases its security risk." The security gate at the end of the process was never actually guarding the thing it claimed to guard.

Meanwhile, software sat in what the defense acquisition community calls the "valley of death," the 18 to 24 months between a working prototype and actual authorization for use. Warfighters operated on 30-year-old legacy systems while modern capabilities waited in queue.

That's not a speed problem. That's a risk profile problem. The old model embedded risk by design.

## What SWIFT actually changes

On June 1, 2025, the DoD launched SWIFT (Software Fast Track) via Acting CIO Arrington's "Accelerating Secure Software" memo. The final framework and implementation plan were published at the end of July, following a 90-day sprint that collected more than 500 industry responses across three RFIs. The appetite for change was real.

The shift is architectural, not just procedural.

Instead of a manual checklist review conducted by an assessor team with a scheduling backlog, SWIFT uses AI (specifically large language model-based analysis) to scan source code for anomalies. If the system finds nothing concerning, a provisional ATO can be issued in hours. Arrington described the process directly: "I have AI on the backside — large language modeling — that will determine if there are any anomalies, if there's something in your source code that's bad. If not, you get a provisional ATO."

Compressing from 18 months to hours doesn't cut corners. It moves the security work from a periodic gate to a continuous function.

The SBOM requirement is where things get concrete and technically demanding. SWIFT requires vendors to produce certified Software Bills of Materials for both sandbox and production environments, plus a third-party SBOM. All of it gets uploaded to eMASS (the DoD's Enterprise Mission Assurance Support Service) for automated review. The point is transparency: if I know exactly what components are running in your system at any given moment, I can correlate that against known vulnerabilities in near-real time rather than waiting for the next assessment cycle.

This builds on, and ultimately supersedes, the cATO (Continuous ATO) framework the DoD CIO published guidance on in April 2024. The cATO model established three criteria: continuous monitoring of RMF controls, active cyber defense capability, and use of an approved DevSecOps reference design with a secure software supply chain. SWIFT takes those same principles and extends them further into the acquisition and authorization pipeline.

## Where this was always headed

I co-authored the DoD Enterprise DevSecOps Cloud Reference Design with the DoD CIO and my colleague Tim Meyers. Writing it, the direction was clear even then: security had to live in the pipeline, not at the end of it.

The practical path we described is one teams can build toward right now. Use GitHub.com for IL2 development. Automate SBOM generation, vulnerability scanning, and configuration hardening directly in your CI/CD pipelines, not as a pre-deployment step but as continuous pipeline stages that run on every commit. Promote artifacts through IL2 to IL4 or IL5 environments using infrastructure-as-code and pre-hardened baselines. The authorization reflects your current posture because your pipeline is continuously asserting it.

That's what "security in the pipeline" actually means. Not a scan you run before you submit your ATO package. A set of automated gates that your code passes through every time it moves.

## The hard questions SWIFT hasn't fully answered

I want to be honest about where SWIFT is still incomplete, because the industry feedback was explicit about it.

The biggest open question is attestation standardization. After analyzing more than 400 RFI responses, the SWFT Combined Summary found a consistent theme: vendors have no clear picture of what a valid attestation actually is. What evidence is required? How often does it need to be refreshed? Can vendors self-attest to NIST SP 800-218 compliance, or do they need a third-party assessment organization? The document acknowledged "a strong call for the DoD to define a legitimate attestation, identify what is required to complete an attestation, and to ensure consistency of these standards."

That's a significant gap for a process supposed to compress authorization timelines. If vendors don't know what evidence they need to produce, they can't build pipelines that produce it automatically.

The SBOM accuracy question is real in a different way. Most SBOM tools generate their output from source code or package manifests. Source-code analysis can miss vulnerabilities in compiled artifacts, in the binary that actually runs rather than the source that produced it. Binary code analysis extracts a software inventory directly from compiled code, including firmware, and surfaces risks that manifest-based tools can't see. SWIFT's current framework doesn't fully resolve whether source-code SBOMs satisfy its transparency goals or whether binary analysis is required to validate them.

Third-party assessor qualifications are also still being refined. The standards for who counts as a valid external verification body, what certifications and accreditations are required, were under active development as of mid-2025.

These aren't reasons to dismiss SWIFT. They're the natural friction of replacing a decades-old process with something built for a different threat environment. The trajectory is right. The details need to catch up.

## What this means if you're building for DoD today

SBOMs are no longer optional documentation. They're operational artifacts. Your pipeline should generate them automatically on every build and maintain them for both development and production environments. Third-party validation is likely to become a hard requirement, which means you need to know who your validating organization will be before you're staring at a deadline.

The gap between source-code SBOMs and binary analysis is a real liability. Tools that only scan your package manifests will miss what's actually compiled into your artifacts. Building binary analysis into your pipeline now isn't premature; it's where the requirement is heading.

Continuous monitoring is a development team responsibility built in from the first pipeline stage, not an operations team responsibility bolted on after deployment. If your CI/CD doesn't have automated security gates, you don't have security — you have the absence of a recent violation.

And if you're still treating the ATO as something that happens at the end of a build cycle, you're optimizing for the wrong outcome. The new model measures your current posture, continuously. That's only possible if you've been building for continuous posture from the start.

## Faster is more secure

The phrase "we can't rush security" was always a misunderstanding of what the old ATO process was measuring. It measured documentation quality at a point in time. It was never measuring the security of a running system.

SWIFT measures something different: whether your software, as it actually exists right now, passes automated analysis for known risks and supply chain integrity. That measurement is only useful if it's repeated continuously, and if it's automated, there's no reason it can't be.

The framing I'll stand behind is the same one that informed the DevSecOps reference design: when security is built into the pipeline and validated continuously, faster is more secure. Not despite the speed. Because of it.

The 18-month ATO was slow. It was also insecure. Those two things were not separate problems.

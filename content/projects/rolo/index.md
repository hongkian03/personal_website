---
title: "Rolo"
description: "A four-person startup's AI personal network platform; I led the Clerk authentication migration, instrumented AWS Bedrock with async Langfuse traces and a fail-open circuit breaker, and self-hosted Langfuse on Kubernetes."
date: 2026-03-18
lastmod: 2026-05-01
draft: false
role: "Backend, AI observability, infrastructure, code review"
period: "Mar 2026 – May 2026"
status: "Launched"
featured: true
category: "Software Engineering"
summary: "A four-person startup shipped an AI-assisted personal network management platform to a small group of invited early users. I led the Clerk authentication migration, instrumented AWS Bedrock workflows with async Langfuse traces and a fail-open circuit breaker, and deployed the self-hosted Langfuse stack on Kubernetes."
stack:
  - TypeScript
  - Next.js
  - PostgreSQL
  - Drizzle ORM
  - Clerk
  - AWS Bedrock
  - Langfuse
  - Helm
  - Kubernetes
  - Docker
  - Playwright
links: []
---

## Context

Rolo is a personal network management website for people with large professional networks, including startup founders. The product helps users organize contacts and relationship context so they can manage and leverage their networks more deliberately. I joined a four-person team during the most intensive build phase in March 2026 and continued product networking and feedback work after launch. The core product shipped and was shared with users for feedback in May 2026.

The four-person team included a main founder across product and architecture, a contributor focused on mobile integration, a feature-oriented contributor who relied heavily on AI coding tools, and me as a backend and infrastructure contributor with additional technical code review responsibilities.

## Contribution

- Migrated the custom username and password authentication to Clerk-managed identity.
- Implemented the backend Clerk integration, including the initial built-in UI integration.
- Set up Langfuse tracing of LLM calls.
- Performed the initial self-hosted Langfuse setup using Helm and Kubernetes on a Linux VPS over SSH, with PostgreSQL as the explicitly recalled backing service.
- Connected the deployed Rolo application to Langfuse and verified that traces arrived.
- Used tracing to observe model, token, and cost data for AWS Bedrock-backed features.
- Prototyped backend graph computation, caching, APIs, and a force-directed frontend visualization. The first implementation was merged and then reverted during my involvement. The feature was eventually pushed back and cancelled due to its complexity.
- Reviewed teammate code, especially AI-generated contributions, to protect maintainability and architecture quality.
- Helped market the product and gather integration-focused user feedback in late April and May 2026.

## Features I Worked On (in chronological order)

### Relationship graph prototype

My first graph implementation was merged and then reverted on March 26. Later graph work exists in the repository but should not be described as my shipped feature.

The prototype backend compared each user's active contacts for shared company, location, and education, used a PostgreSQL self-join to find contacts associated with the same events, normalized common company, location, and school names before matching, canonicalized contact pairs to avoid duplicate edges, merged implicit and event-derived connections, and scored unique signal types using additive weights capped at 1.0. Connection sources were stored in PostgreSQL JSONB, and nodes and edges were serialized for the visualization layer. Initial signal weights were 0.50 for a shared event, 0.25 for a shared company, 0.15 for a shared education, and 0.10 for a shared location. Implicit pair comparison was quadratic in the number of contacts, which motivated caching and made the feature sensitive to larger or denser networks.

The cache and API behavior followed a stale-while-revalidate design: one graph cache record was stored per user, contact or calendar changes marked the cache stale, the graph API returned fresh cached data immediately when available, a stale cache was returned immediately while a recomputation ran after the response, and a first request without a cache performed synchronous computation. The client polled every ten seconds while cached data remained stale.

The first frontend used Cytoscape.js with a CoSE force-directed layout. Node repulsion was 4,500, ideal edge length 100, gravity 0.25, and the simulation ran for 1,000 iterations. Edge width and opacity mapped from connection strength, nodes linked to profile pages, and edge tooltips explained shared events, companies, schools, or locations. A later experimental renderer used D3 force-graph physics with an anchored "You" node, adaptive repulsion, weight-dependent link distance, collision forces, and five seconds of simulation. Hover reliability was affected by differences between the visible canvas and the hidden shadow canvas used for hit detection, especially after pan, zoom, and animation. I investigated hitbox updates, edge hit widths, animation padding, and drag-state suppression, but the feature was ultimately de-scoped during my involvement.

### Authentication migration

The prior system used custom username and password authentication backed by PostgreSQL. The migration delegated identity, credentials, and session management to Clerk while retaining PostgreSQL as the application database. The repository-verified work included Clerk dependencies and environment validation, a clerk_id mapping in the users table and associated migrations, Clerk middleware for route protection, webhook handling for user synchronization, rewritten session and authorization guards, updates across the authentication API routes, Clerk identity handling in the user repository, initial Clerk components in the application shell, and onboarding for user name collection. The defensible outcome is a simpler authentication code path with managed credential security and support for third-party sign-in. I do not have incident data to claim a measured reduction in bugs or a quantified security improvement.

### Langfuse and AWS Bedrock observability

The implementation added two tracing paths. The first was completeWithTracking, which emitted rich traces with task type, feature tag, user ID, provider, model, prompt, response, token counts, and latency. The second was a smaller safety-net trace for direct LLMClient.complete calls that did not pass through the centralized wrapper. The centralized wrapper passed skipTracing to prevent duplicate traces. Trace emission ran asynchronously so observability could not delay the user-facing LLM response.

The reliability design was deliberately fail-open. Langfuse configuration was optional, so the application continued when credentials were absent. Trace failures did not fail the underlying LLM operation. A three-state circuit breaker opened after five consecutive trace failures, paused for five minutes, and then attempted a half-open recovery. Successful tracing reset the failure counter and closed the circuit. Sentiment classification was migrated to the centralized tracked LLM wrapper as part of this work.

The deployment side used Helm and Kubernetes on a Linux VPS, reached over SSH and Linux command-line tools. PostgreSQL was the recalled backing service. I connected Rolo to the deployment, verified that traces arrived, and used the UI to inspect model, token, and cost information. I did not configure alerts, sampling, retention policies, or ongoing platform maintenance. While I implemented and deployed Langfuse tracing, the project subsequently explored or migrated the tracked provider integration to LangSmith due to a switch in focus from self-hosting to ease of use and setup and integration with LangChain.

## Reflections

Several months after recruiting me to work on MAGK, my friend let me on again to help with Rolo. This time, there was a longer runway for development but no investor pitch planned. Still, I was on board with the idea and saw its business potential and value as a consumer product, so I agreed to join.

With a development runway of several weeks, and the need to balance my commitment with classes and other stuff (as the semester was ongoing), the experience was a tad more stressful. The graph visualization feature started off as my onboarding project, mostly to familiarize myself with the development workflows of the team, which relied heavily on agentic AI (Jack being the avid supporter that he is of the tech). I'm quite unhappy with how it turned out though. If I had more time to intentionally plan and figure it out, I think I could've done a much better job, but alas the team wanted to move fast.

I shifted my focus to backend and infrastructure, which I actually quite enjoyed more. I particularly enjoyed getting to SSH to a VPS and setting up infrastructure hosting using stuff like Helm and Kubernetes. There's somem oddly satisfying aspect to looking up commands, running them and tuning the system to my preferences, and understanding what's going on behind the scenes. The whole process is way more intuitive to me than coding with and debugging frontend stuff like HTML/CSS and React.

Oddly enough, having tinkered with a number of basic projects myself, this was my first time using authentication as a service (does AaaS exist as a terminology?) with Clerk. Perhaps it's because I've never shipped a product like this before, but it never really occured to me to go beyond setting up a PostgreSQL database with some intermediate encryption and decryption. I enjoyed the convenience of it all, and Clerk might be my preferred means of implementing auth going forward.

Eventually, after Rolo's launch, feedback was received from a small set of early adopters, but everyone else (including me) moved on to other endeavours as thhe summer approached. For instance, Jack himself mainly casually markets Rolo while focusing on his primary mission of finding another great idea. I started my research assistant position, which you can read more about [here](/content/experience/undergraduate-research-assistant/). Overall, it was another great experience working in a team and shipping real features and software. Perhaps Jack will contact me again in about a year.
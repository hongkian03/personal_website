---
title: "MAGK"
description: "Stateless backend services for a workflow automation prototype, with AI data ingestion pipelines."
date: 2025-08-01
lastmod: 2025-08-01
draft: false
role: "Backend services and AI data pipelines"
period: "Aug 2025"
status: "Completed prototype"
featured: false
category: "Software Engineering"
stack:
  - Python
  - FastAPI
  - Playwright
  - Amazon Bedrock
links: []
---

## Context

A friend approached me to help develop a prototype for his startup idea for an upcoming investor pitch. The product was a workflow automation tool for financial data analysts, leveraging web scraping and API calls to Amazon Bedrock for AI-powered features.

## Contribution

Designed and built the backend services. Built service workflows to validate user inputs, orchestrate model calls, and return structured outputs. Integrated Amazon Bedrock with API calls to support AI-driven web scraping and data ingestion pipelines.

## Approach

Kept services stateless so they could be deployed and scaled trivially. Treated each external dependency as a typed boundary so the rest of the system could evolve without breaking.

## Outcome and learning

A minimum viable product was finished for the pitch but eventually no funding was secured and the project was sunsetted.

## Reflections

This was my first time working in a software development team. It was a fairly disorienting and challenging, yet necessary experience for me to grow as a software developer. I learned how to use git effectively beyond just the common push/pull/commit commands, and learning about the backend services I designed and coded while implementing them drastically improved my understanding of system design. Additionally, this project probably solidifed my relatively greater interests in backend engineering and system design in the software development process. My technical skills also improved a lot. That last point is, rather funnily, quite evident to me as I am writing this reflection, as the technical details of this project are noticeably lacking compared to subsequent projects.
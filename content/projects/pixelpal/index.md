---
title: "PixelPal"
description: "AI-powered photo editing workflow tool — built the backend for BostonHacks 2025 (Top 4)."
date: 2025-11-01
lastmod: 2025-11-01
draft: false
role: "Team member — backend logic and AI workflow"
period: "Nov 2025"
status: "Completed"
featured: true
category: "Software Engineering"
stack:
  - Python
  - FastAPI
  - Gemini API
  - Next.js
links: []
---

## Context

I collaborated in a four-person team at BostonHacks 2025 to build PixelPal, a full-stack photo editing tool where users could chain AI-driven processing steps in a modular workflow. The idea came 

## Contribution

I mostly owned the backend logic, coding the FastAPI backend for our frontend to interface with the Google Gemini API. I designed validation and transformation pipelines so that the AI output was consistent and interpretable. I also contributed heavily to the prompt engineering that was the backbone of the modular workflow system that let users chain processing steps for greater control. 

## Approach

Validated inputs at the boundary, orchestrated Gemini calls through a small set of well-named service functions, and returned structured outputs the frontend could render predictably. Treated each step as a pure transformation so the workflow could be re-run or reordered safely.

## Reflections

We sadly did not win any prizes, but we were told by a judge that we finished top 4 in our track and it was close. We did get great feedback from some of the judges, which initially inspired us to try continuing and polishing the project, but everyone moved on to other things. Personally I'd love to revisit this, perhaps as a mobile app project to gain some familiarity with that domain.
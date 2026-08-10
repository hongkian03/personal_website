---
title: "Undergraduate Research Assistant"
description: "Refactored and documented a multi-script Python pipeline for biomolecular NMR resonance assignment in the Marintchev Lab."
date: 2025-03-01
lastmod: 2026-07-30
draft: false
organization: "Marintchev Lab, Boston University"
role: "Undergraduate Research Assistant"
period: "May 2026 – Aug 2026"
location: "Boston, MA"
category: "Work Experience"
highlights:
  - Refactored and documented an inherited multi-script Python pipeline for NMR chemical-shift analysis, establishing a reproducible workflow for a three-researcher team.
  - Replaced hardcoded file paths with CLI-configurable inputs and deterministic output naming using argparse and pathlib, eliminating source-code edits for new datasets.
  - Extended statistical assignment logic with pandas and NumPy and validated ambiguous behavior through manual review with a faculty domain expert.
---

## Context

The Marintchev Lab at Boston University works on biomolecular NMR (nuclear magnetic resonance) — specifically, automating parts of the resonance assignment process that turns raw chemical-shift data into labeled spin systems. I joined as an undergraduate researcher alongside two other students, working under Professor Marintchev on a Python pipeline that previous researchers had left in a difficult state to use.

## Contribution

Took ownership of the engineering quality of the assignment pipeline. Mapped and documented the existing multi-script workflow so the team could reason about it as a system. Replaced hardcoded input filenames with command-line arguments and made output filenames deterministic from inputs, so the pipeline could be re-run on new datasets without touching source code. Began evaluating and refining the existing statistical assignment logic against manual expert review.

## Approach

Worked in two passes. The first pass was almost entirely about making the existing code reproducible — argparse, pathlib, naming conventions, and a clear input/output contract. The second pass was about the assignment logic itself: applying Gaussian chemical-shift distributions and bootstrap estimation to evaluate probabilistic assignments against BMRB reference data, with Professor Marintchev as the manual validator for ambiguous cases.

## Outcome and learning

The team now has a single documented pipeline they can hand to a new undergraduate and run on a new dataset without code edits. The deeper lesson was that "reproducible" and "correct" are not the same problem — making the code reproducible was necessary work before the statistical refinements could even be discussed honestly.

## Reflections

_Coming soon._

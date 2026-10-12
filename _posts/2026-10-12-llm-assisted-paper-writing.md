---
layout: post
title: "Writing Papers with an LLM: Principles and Process"
date: 2026-10-12
description: principles for the introduction and method, a three-phase human–LLM workflow, and advice on experiments and discussion
---

{% include figure.liquid path="assets/img/headpic-llm-assisted-paper-writing.png" class="img-fluid rounded z-depth-1" %}

I've been writing papers alongside LLMs long enough to stop treating them as a magic draft button. What works is a small set of principles and a clear sense of who is in charge at each stage.

## Principles for the Introduction

The strongest intros share a shape: existing methods have a single core limitation, and overcoming it requires solving several technical challenges. State that motivation, and back it with concrete examples of where the limitation bites.

Abstract, introduction, and experiments should carry different information granularity. If the abstract reports the headline result, the introduction can describe research questions and setup instead of repeating numbers. If the abstract stays conceptual, the introduction gives the core data — the gain, the F1 delta — and the experiments section unpacks the full analysis and secondary numbers. The same finding should not be retold at the same resolution three times.

## Principles for the Method

Three rules keep a method section from becoming a list of components.

First, purpose before definition. Explain why a design is needed and what role it plays in the system before naming it.

Second, design followed by implementation. Every design must be immediately followed by how it is realized. Don't write "we extract / retrieve / associate / integrate" and stop — say whether it's done via LLM prompting, a rule-based pipeline, embedding retrieval, a graph query, a localization algorithm, or something else.

Third, organize by data flow. Order the section as input → processing → intermediate result → validation/transformation → output, following how the software actually runs, rather than laying components side by side.

## Three Phases with an LLM

**Fast draft.** The LLM is the writer; you are the reader. The goal is to materialize ideas quickly. Redundancy, roughness, and unstable structure are all fine.

**Polish.** You become the writer; the LLM is the reviewer. You decide what each paragraph should say; the LLM flags logical jumps, ambiguous phrasing, and missing citations, and suggests rewordings.

**Finalize.** You are the sole architect. Read the whole thing from the start and rebuild the global model: why this paper exists, what each section contributes, and which claim each result supports. At this stage, asking the LLM to rewrite large blocks tends to break the structure you've already formed.

## Advice on Experiments and Discussion

A few rules I picked up along the way, worth repeating because they're easy to forget under deadline pressure.

Put the valuable conclusions first. Give the overall trend, then discuss the exceptions. Every conclusion should make clear which table or analysis it comes from — "as shown in Table 3" — so the reader can verify rather than trust. And in the discussion, if you have no new knowledge to add, don't discuss. A discussion section that only restates results is worse than a short one.

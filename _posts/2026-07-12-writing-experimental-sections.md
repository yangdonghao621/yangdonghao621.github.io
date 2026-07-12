---
layout: post
title: "How to Write a Strong Experimental Section In The Era Of AI"
date: 2026-07-12
description: a four-move framework for turning experimental results into a scientific argument — from numbers to meaning, observations to mechanisms, individual findings to a coherent whole, and the study to the broader literature
---

{% include figure.liquid path="assets/img/headpic-writing-experimental-sections.jpg" class="img-fluid rounded z-depth-1" %}

Many experimental sections are technically complete but intellectually weak. They contain tables, figures, and statistical tests, yet after reading them the audience may still wonder: what do these numbers actually mean, why did the results occur, and what does the study contribute beyond this particular experiment?

A strong experimental section moves from observation to explanation, from local findings to a coherent argument, and from the paper itself to the broader literature. I summarize this progression in four moves.

## 1. From Numbers to Meaning: What Happened?

The first task is to explain what the tables and figures show. This sounds obvious, but many papers merely repeat values already visible in a table:

> Method A achieved 28%, while Method B achieved 20%.

A better interpretation:

> Method A increased the success rate from 20% to 28%, a 40% relative improvement. This suggests the additional information is useful for a meaningful subset of tasks, though remaining failures indicate its benefit is conditional rather than universal.

The goal is to transform numerical observations into conclusions. At this stage, clarify the main comparison, the magnitude of the difference, whether it is statistically and practically meaningful, whether it holds across settings, and whether it supports the hypothesis. Do not read the table aloud — explain what the evidence allows the reader to conclude.

## 2. From Observations to Mechanisms: Why Did It Happen?

Aggregate metrics show that a method works, but rarely how. Mechanistic explanations require additional evidence: subgroup analyses, ablation studies, representative cases, execution traces, error categories, or comparisons between successful and unsuccessful examples.

A useful analytical chain: statistical pattern → subgroup difference → representative case → process evidence → mechanism explanation.

Equally important: explain abnormal or negative results. They are not inconveniences to hide — they are often the most valuable evidence for understanding the method's boundary conditions. A method may fail when guidance is too vague, inconsistent with the repository state, difficult to operationalize, or correct in principle but irrelevant to the specific failure. These cases reveal when and why the approach does not work.

However, distinguish among conclusions directly supported by quantitative evidence, explanations supported by repeated qualitative patterns, and plausible hypotheses requiring further validation. One case can illustrate a mechanism; it cannot prove universality.

## 3. From Individual Findings to a Coherent Whole

Research questions are presented separately for clarity, but a paper should not feel like a collection of unrelated mini-studies. Each RQ should play a specific role in the overall argument.

For instance, a study might ask: whether an artifact is reliable, whether it improves downstream performance, under what conditions it is useful, and how it changes agent behavior. These form a potential evidence chain: the artifact is sufficiently reliable → the agent can use it → its use changes the reasoning process → the changed process improves the outcome. This is stronger than four isolated answers.

A good synthesis identifies three relationships: **progressive** (how one result supports the next), **conditional** (under what circumstances the effect appears), and **boundary** (where the evidence stops). A convincing paper rarely concludes a method simply "works"; a more informative conclusion identifies when it is effective and when it is not.

## 4. From the Current Study to the Broader Literature: Why Does It Matter?

Connecting to prior work should be more than adding citations that agree with the results. A strong discussion asks: does the study validate an existing theory in a new setting, reveal the mechanism behind a previously observed effect, identify a condition under which a prior claim holds, expose a boundary ignored by earlier studies, or contradict previous evidence — and if so, why?

Four productive modes of engagement: **validation** (confirming an existing claim in a new domain), **extension** (explaining how or why an effect occurs), **qualification** (showing a broad claim only holds under certain conditions), and **challenge** (producing conflicting evidence and investigating why). The most valuable contribution is often showing how new evidence changes the way we understand the problem — not simply confirming previous work.

## Putting It Together

The four moves form a progression: what happened → why did it happen → what do the findings collectively show → why does it matter. Together they transform an experimental section from a sequence of numerical reports into a scientific argument. The goal is not merely to prove a result exists — it is to help the reader understand it, explain its mechanism, see its place in the paper's argument, and recognize how it advances the field.

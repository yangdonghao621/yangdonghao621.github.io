---
layout: post
title: "A Semi-Automated Research Workflow"
date: 2026-10-12
description: dividing labor between the human, the agents, and the workspace — what actually survives contact with real papers, and the hard problems that remain unsolved
---

{% include figure.liquid path="assets/img/headpic-semi-auto-research-workflow.png" class="img-fluid rounded z-depth-1" %}

A semi-automated research workflow rests on a clean division of labor: the human proposes the questions, judges scientific value, understands the agents, and organizes the paper's logic; the agents execute, explore, generate, and check; and the workspace holds state, constrains the process, and keeps results traceable.

That division is easy to state and hard to keep. Here is what survived contact with real papers, and the problems I still haven't solved.

## Setup and Workspace

Start with a literature review to fix the direction, then build an agentic workspace with enough disk space. The first thing to write is `agents.md` — a document describing the general workflow, principles, and research questions, and especially which folders hold what. It should mandate timely cleanup of stale scripts and regular commits.

In practice agents generate a flood of scripts, and you either commit constantly or never dare to. The fix that worked is to define milestones: when a milestone is done, clean, check, and commit, so each step builds on a correct base. The worst state is a pile of `.md` and script files whose names reveal nothing, whose contents you don't want to read, and that you don't dare delete.

One more trap to define away in advance: with a hundred tasks, not every agentic run succeeds. Some produce empty patches or time out. If you don't agree on conventions up front, the LLM will faithfully report every number — 96, 98, 99 — and readability collapses.

## Before Any Experiment

The agent must finish three things before running anything: an experiment-design markdown, the infrastructure code, and an API smoke test. All three, no exceptions.

Two habits matter for results. Reuse experiment outputs wherever possible. And organize results not by research question — different RQs can draw on the same experiment from different angles. Better to keep RQ folders for scripts and markdown, and point to raw experiment data with symlinks.

## After Each Experiment

Human and agent analyze results together from multiple angles, verifying hypotheses with LLM-assisted annotation. When a hypothesis is confirmed or falsified, resist the urge to rush it into the paper. Demand that the agent re-check, again and again, whether the conclusion itself contains a bug. Agents will make mistakes, and only another agent will find them — the developer has no patience for that audit.

Manage analysis results hierarchically: archive what is useless but might return, and delete only what is confirmed wrong. Prefer archiving over deleting — you never know when something will matter again.

## Writing

The human reads the conclusions and writes the outline; the agent fills in the content. Or the agent drafts a full version first, but the final logic must be human-reviewed. An agent writing the paper alone is never good.

Every claim needs a corresponding reproduction script, and the final pass needs repeated checks: figure-table-text consistency, a writing gate (EASE guidelines plus an academic-writing skill, avoiding AI-sounding prose), and a reproduction package.

## Three Unsolved Problems

First, when memory fills up and context gets compressed, the agent almost inevitably gets dumber — and it is doing exactly the work where mistakes hurt most. A local memory system should help, but today it bloats with trivia and feels expensive and useless.

Second, how to hand a task to another agent to save the main agent's context. One approach is to land an experiment-design markdown and let a second agent read it, understand the repo, and finish the task. In practice I usually let the main agent do everything, and the context pollution buries high-level ideas under engineering detail.

Third, references. I still assemble `.bib` files myself; I don't trust an agent to do that yet.

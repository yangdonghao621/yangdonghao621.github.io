---
layout: post
title: "Rethinking the Research Workflow: Toward an AI-Native Workspace"
date: 2026-07-09
description: why current research toolchains are fragmented and what an AI-native workspace for computational research might look like
---

{% include figure.liquid path="assets/img/headpic-ai-native-research-workspace.png" class="img-fluid rounded z-depth-1" %}

The way we do computational research today is painfully fragmented. A typical project flows through four stages—coding, discussion, writing, and reflection—each using a different tool, each with its own context silo.

In VSCode or Cursor for coding, the LLM knows your source files but has no idea what your paper claims, what experiments you've tried, or why a particular design choice was made. When you switch to ChatGPT or Claude Web for brainstorming, the LLM has lost all awareness of your latest code, experiment results, and failed attempts. Open Overleaf to write, and it knows even less: no experiment details, no figure provenance, no history of decisions. By the time you sit down to reflect, most of that tacit knowledge has already evaporated.

The consequences are threefold. First, LLM context is not shared across stages—every tool switch is a context reset. Second, research artifacts are scattered across disjoint systems, making genuine retrospection nearly impossible. But the most critical pain point, and the one most unique to research, is that **claims in your paper cannot be reliably traced back to the experiments, code, failed attempts, and design discussions that produced them**.

What we need is not yet another IDE. The minimal viable form already exists as a convention layered on top of tools researchers already use: a VSCode/Cursor workspace augmented by MCP tools and Skills, with the project repo itself serving as the unified knowledge base.

Imagine a project organized like this:

```
project/
├── paper/              # main.tex, sections, figures
├── code/               # src, scripts
├── experiments/        # configs, results, logs
├── notes/              # research_log, meeting_notes, failed_ideas
├── skills/             # research_advisor, experiment_analyst, paper_writer, reviewer
└── context/            # claims.yaml, evidence_map.yaml, decisions.md
```

The real capability is not the directory structure—it's what the system can do with it. When you write a claim in your paper, the agent knows which experiment it maps to, which script generated the figure, what configuration produced that result, which alternatives you tried and discarded, and what the discussion looked like when the decision was made. The evidence chain is explicit, searchable, and shareable.

This is not a tool. It is a **workspace convention**—an AI-native way to organize computational research so that paper, code, experiments, logs, and agent history form a single, coherent context, and every claim carries its evidence with it.

---
layout: page
title: DNA Storage via Redundant Homologous Sequences
description: Voting-based error correction for DNA data storage. Encoding-agnostic recovery with 100% accuracy at 15× redundancy.
img: assets/img/Mengma_Figures/Figure_01.png
importance: 1
category: ydh
related_publications: false
---

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/Mengma_Figures/MyTalk.jpg" class="img-fluid rounded z-depth-1" %}
    <figcaption class="figure-caption text-center">Presenting our approach at the competition.</figcaption>
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/Mengma_Figures/OurAward.jpg" class="img-fluid rounded z-depth-1" %}
    <figcaption class="figure-caption text-center"><strong>Team Baseline — 7th place out of 190 teams</strong></figcaption>
  </div>
</div>

---

> 🏆 **7th out of 190 teams** in the national DNA storage competition &nbsp;·&nbsp; [📰 Read news coverage](https://finance.sina.com.cn/stock/med/2023-11-06/doc-imztstpq7354812.shtml)

---

## 1. Project Overview

This project addresses the core challenge in DNA-based data storage: faithful recovery of original information from error-prone DNA sequences introduced during synthesis, amplification, and sequencing. We propose a novel decoding pipeline that leverages homologous sequence redundancy, unsupervised clustering, and a sliding-window voting algorithm to reconstruct error-free sequences without relying on explicit error-correcting codes embedded in the encoding stage.

Our approach is encoding-agnostic, computationally efficient, and achieved 100% reconstruction accuracy on the competition's 15× redundancy benchmark, ranked **7th out of 190 teams**.

## 2. Problem Formulation

The DNA storage pipeline consists of two phases:

| Phase | Input | Output | Evaluation Metric |
|-------|-------|--------|-------------------|
| Encoding | Image (JPG/PNG/WEBP/BMP) | DNA sequence | Information density (20%), synthesis/sequencing compatibility (30%) |
| Decoding | Error-prone DNA sequences | Reconstructed image | Similarity to original (50%) |

Key observation: Errors are sparsely distributed across individual copies. When the same sequence is replicated multiple times (homologous sequences), error positions are unlikely to coincide across copies. This redundancy enables consensus-based reconstruction without prior error models.

## 3. Encoding Strategy

We adopt a two-stage encoding pipeline:

**Image preprocessing:** Compression and downsampling (format-adaptive) to reduce bitstream length while preserving perceptual quality.

**Fountain-code-based mapping:** Each 20-bit block encodes positional information; 208-bit blocks carry payload data. The encoded DNA sequence length is fixed at 130 nucleotides per block.

The encoding produces N identical copies of each DNA sequence—these constitute the homologous sequence family delivered for synthesis. Reference implementation adapted from Chamaeleo.

{% include figure.liquid path="assets/img/Mengma_Figures/Figure_07.png" class="img-fluid rounded z-depth-1" %}

*Figure 1: Encoding workflow. Image bitstreams are partitioned into 20-bit position blocks and 208-bit payload blocks, then mapped to 130-nt DNA sequences via fountain codes.*

## 4. Decoding Pipeline

The decoding phase is the core contribution of this work. It comprises three sequential modules.

### 4.1 Sequence Vectorization

Each DNA sequence (length L = 120) is transformed into a 132-dimensional feature vector: 64-dim for frequency of all 3-mer combinations (4³ = 64), and 68-dim for per-position nucleotide counts over sliding windows of length 7 (⌊120/7⌋ × 4 = 68). This hybrid representation captures both compositional and positional information.

### 4.2 Multi-Level Clustering

We employ a two-stage clustering strategy to group homologous sequences: DBSCAN with k-nearest-neighbor distance as adaptive density threshold, followed by K-means refinement on the resulting clusters. Outlier sequences (with abnormally low local density) are iteratively removed and re-clustered, ensuring that only true homologs are retained for reconstruction.

{% include figure.liquid path="assets/img/Mengma_Figures/Figure_09.png" class="img-fluid rounded z-depth-1" %}

*Figure 2: Multi-level clustering pipeline. DNA sequences are vectorized into 132-dim features, clustered via DBSCAN with adaptive density threshold, then refined by K-means. Outliers are iteratively removed.*

### 4.3 Sliding-Window Voting Reconstruction

This is the algorithmic centerpiece of our decoder. It operates on aligned clusters and reconstructs the consensus sequence through recursive local voting.

**Core Assumptions:** Errors at the same position across multiple homologs are rare. Within any 4-base window, at most one error occurs in the majority of copies.

{% include figure.liquid path="assets/img/Mengma_Figures/Figure_13.png" class="img-fluid rounded z-depth-1" %}

*Figure 3: Core sliding-window voting mechanism. A 3-base window scans aligned homologous sequences; the most frequent pattern determines the consensus base at each position.*

**Voting Mechanism:** A sliding window of length 3 (stride = 1) scans the aligned sequences. The most frequent 3-base pattern is identified as the consensus fragment. The middle base of that window is voted and fixed. The window then shifts right by 1 (standard case) or by 2 (when encountering homopolymer runs, e.g., AAA).

**Handling Insertions/Deletions (Indels):** Indels cause misalignment, breaking simple positional voting. Our algorithm dynamically adjusts stride per sequence based on local consensus.

{% include figure.liquid path="assets/img/Mengma_Figures/Figure_14.png" class="img-fluid rounded z-depth-1" %}

*Figure 4a: Indel handling — Case A (homopolymer). Normal sequences shift by 2; erroneous sequences skip the repeated base and fall back to Case B.*

{% include figure.liquid path="assets/img/Mengma_Figures/Figure_15.png" class="img-fluid rounded z-depth-1" %}

*Figure 4b: Indel handling — Case B (non-homopolymer). Normal sequences shift by 1; indels are detected by cross-comparing 3–5 bases of the candidate against 2–4 bases of the consensus.*

**Ambiguity Resolution:** When both insertion and deletion conditions are met simultaneously, we treat it as a deletion first, shift the window by 5 positions, compare the 2–4 bases of the normal fragment against positions 4–6 of the corrected sequence, and either accept or revert based on the match.


{% include figure.liquid path="assets/img/Mengma_Figures/Figure_19.png" class="img-fluid rounded z-depth-1" %}

*Figure 5: Ambiguity resolution. When both insertion and deletion conditions are met, we first treat it as a deletion, then compare the 2–4 bases of the normal fragment against positions 4–6 of the corrected sequence*

**Recursive Split Strategy:** To mitigate local greedy failures in the middle of long sequences, we recursively split each sequence into two halves (with a 10-base overlap) and recover each half independently—one from the start, the other from the end. The overlapping region is used for consistency checking.

{% include figure.liquid path="assets/img/Mengma_Figures/Figure_21.png" class="img-fluid rounded z-depth-1" %}

*Figure 6: Recursive split strategy. Each sequence is divided into two halves with 10-base overlap; forward recovery on the first half, backward recovery on the second.*

## 5. Experimental Results

### 5.1 Dataset & Setup

10,000 randomly generated sequences (length 159, GC ≈ 50%, max homopolymer ≤ 4), each replicated 7×. Errors injected via the official competition simulator. Hardware: single-core CPU.

### 5.2 Performance Metrics

| Metric | Value |
|--------|-------|
| Per-base accuracy | 99.76% |
| Full-sequence recovery rate | 98.31% |
| Min per-sequence accuracy (worst case) | 57.86% |
| Total runtime (10k sequences) | 233.37 s |

{% include figure.liquid path="assets/img/Mengma_Figures/Figure_24.png" class="img-fluid rounded z-depth-1" %}

*Figure 7: Key performance overview. Per-base accuracy, full-sequence recovery rate, and runtime breakdown.*

### 5.3 Competition Benchmark (15× Redundancy)

| Component | Score |
|-----------|-------|
| Compression | 94.70 |
| Compatibility | 71.26 |
| Recovery (standard) | 87.13 / 89.33 |
| Recovery (trap images) | 45.31 |

Trap images contained fine-grained background details lost during aggressive compression—highlighting a current limitation.

{% include figure.liquid path="assets/img/Mengma_Figures/Figure_25.png" class="img-fluid rounded z-depth-1" %}

*Figure 8: Competition benchmark scores across dimensions. Compression (94.70), compatibility (71.26), and recovery performance with and without trap images.*

## 6. Innovation and Contributions

- **Encoding-agnostic recovery:** Compatible with any DNA encoding scheme—no custom error-correcting codes required.
- **High robustness:** 100% original-sequence recovery at 15× redundancy under diverse error types.
- **Practical redundancy management:** Resilient to partial DNA sample loss—realistic wet-lab scenario.
- **Hybrid statistical–deterministic algorithm:** Density-based clustering + deterministic voting outperforms purely probabilistic or rule-based methods.

{% include figure.liquid path="assets/img/Mengma_Figures/Figure_28.png" class="img-fluid rounded z-depth-1" %}

*Figure 9: Summary of key innovations. Encoding-agnostic design, robustness to multiple error types, practical for partial DNA loss, and extensible with deep-learning components.*

## 7. Limitations and Future Work

Trap image failure indicates a need for content-adaptive encoding that preserves perceptual saliency. Clustering is currently CPU-bound; GPU-accelerated distance computations will reduce runtime significantly. Extremely long homopolymer stretches (>10 bases) may require additional heuristics or trained models.

## 8. Conclusion

We present a complete DNA storage pipeline with a novel error-recovery framework based on homologous sequence redundancy and a sliding-window voting algorithm. The system achieves near-perfect reconstruction accuracy on benchmark datasets and is compatible with diverse encoding strategies, advancing DNA as a long-term digital storage medium.

**Team:** Baseline · **Affiliation:** Beihang University (School of Computer Science), Institute of Automation (CAS), Institute of Biophysics (CAS) · **Date:** Nov 01, 2023

**Contributions:** Donghao Yang (encoding algorithm, presentation & defense) · CAS collaborators (decoding algorithm, slides & visualization)

---

**Further Reading**

- [Robust data storage in DNA by de Bruijn graph-based de novo strand assembly](https://www.nature.com/articles/s41467-022-33046-w) — Song et al., *Nature Communications*, 2022.
- [Alternative approaches to DNA storage](https://zhuanlan.zhihu.com/p/664617027) — technical review (in Chinese).

# Unsupervised Learning on ConceptARC

This repository explores **unsupervised learning techniques** for discovering latent structure in **abstract visual reasoning tasks** using the **ConceptARC** dataset.  
The focus is on understanding *how tasks transform inputs into outputs*, rather than on image appearance or supervised labels.

---

## Motivation

Abstract visual reasoning tasks, such as those in ARC and ConceptARC, require understanding **transformations** between input and output grids.  
However, labeled supervision for such transformations is expensive and often ambiguous.

This project investigates the following question:

> **Can unsupervised models uncover meaningful latent transformation structure in abstract reasoning tasks without using task labels?**

From a practical perspective, this work demonstrates how unsupervised learning can:
- Reduce reliance on manual labeling,
- Reveal reusable task patterns,
- Support dataset diagnostics and task taxonomy analysis.

---

## Dataset

The project uses the **ConceptARC** dataset, a structured variant of the Abstraction and Reasoning Corpus (ARC).

Each task consists of:
- An input grid,
- A corresponding output grid,
- A human-defined conceptual category (used **only for evaluation**, not training).

### Dataset characteristics
- 427 input–output training pairs,
- Variable-sized grids padded to a fixed resolution (32 × 32),
- Discrete color values (0–9) plus background,
- 16 coarse conceptual categories (e.g., *AboveBelow*, *ExtractObjects*, *TopBottom2D*).

---

## Method Overview

The analysis follows three main steps:

### 1. Representation Learning with NMF
- Input and output grids are one-hot encoded into high-dimensional, non-negative vectors.
- **Non-Negative Matrix Factorization (NMF)** is applied to learn part-based latent representations.
- NMF components are interpretable and preserve spatial structure.

### 2. Transformation Modeling
- For each task, a **latent transformation vector** is computed:
  
  Delta = embedding(output) - embedding(input)

- These vectors represent *how* a task changes an image, rather than *what* the image looks like.

### 3. Unsupervised Clustering
- Transformation vectors are clustered using:
  - DBSCAN (cosine distance),
  - K-Means,
  - Hierarchical clustering.
- Cluster quality is evaluated using **Normalized Mutual Information (NMI)** against ConceptARC categories.

---

## Key Results

- Clustering **transformation vectors** is significantly more effective than clustering raw images or joint input–output embeddings.
- DBSCAN with cosine distance achieves the strongest alignment with known categories.
- Several ConceptARC categories overlap in latent space, indicating shared underlying transformations.
- Some tasks do not belong to any dominant cluster, revealing ambiguity or unique structure.

These results suggest that **unsupervised learning can recover meaningful task structure**, even when human-defined categories are coarse or overlapping.

---

## Limitations

- ConceptARC categories are noisy and overlapping by design.
- NMF assumes linear additive structure.
- Clustering results are sensitive to distance metrics and hyperparameters.

---

## Future Work

- Replace NMF with **neural embeddings** (e.g., VAEs or autoencoders),
- Incorporate spatial priors into representations,
- Combine unsupervised clustering with lightweight supervision,
- Evaluate robustness across random seeds and dataset subsets.

---

## Repository Structure

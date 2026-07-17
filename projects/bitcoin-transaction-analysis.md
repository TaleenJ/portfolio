---
layout: default
title: Bitcoin Transaction Analysis
---

<p class="back-link"><a href="{{ '/#projects' | relative_url }}">← Back to projects</a></p>

# Bitcoin Transaction Analysis

<p class="project-meta">Machine learning · ransomware detection · 1M+ transactions · Fall 2025</p>

## Overview

Team project exploring machine learning on Bitcoin transaction data to help identify ransomware-related activity. Models were trained on the open UCI dataset **“Bitcoin Heist Ransomware Address,”** with a focus on a reproducible preprocessing → modeling → evaluation pipeline.

## Pipeline

1. **Data preparation** — clean and structure large transaction tables; handle missing or noisy fields.
2. **Feature engineering & scaling** — select and transform features useful for separating heist-related addresses from ordinary traffic.
3. **Train / test split** — 80/20 holdout for fair comparison across models.
4. **Model training** — run multiple algorithms on the same prepared features.
5. **Evaluation** — compare models with ROC curves, precision–recall curves, and confusion matrices.

## Algorithms tested

- **Random Forest**
- **Support Vector Machine (SVM)**
- **K‑Nearest Neighbors (KNN)**
- **K‑Means clustering** (unsupervised structure / comparison)

The goal was not a single “winner,” but a clear, side-by-side view of how different families of models behave on the same ransomware-address prediction task.

## Technical stack

Python · pandas · NumPy · scikit-learn · Jupyter notebooks

## Results & impact

Built a reproducible experimentation workflow for a high-volume financial dataset, with cross-model comparison artifacts (ROC, PR, confusion matrices) to support analysis of ransomware-threat detection in Bitcoin transaction data.

## Paper & code

- **Repository:** [ML-Modeling-for-Bitcoin-Transactions](https://github.com/TaleenJ/ML-Modeling-for-Bitcoin-Transactions)
- **Paper (PDF):** [Detecting Ransom Threats with Machine Learning Models for Bitcoin Transactions](https://github.com/TaleenJ/ML-Modeling-for-Bitcoin-Transactions/blob/main/Detecting_Ransom_Threats_with_Machine_Learning_Models_for_Bitcoin_Transactions.pdf)

## Images

<div class="image-grid">
  <div class="media-placeholder">Figure / notebook plot placeholder — add later</div>
  <div class="media-placeholder">Figure / notebook plot placeholder — add later</div>
</div>

<p class="projects-more">More projects on my <a href="https://github.com/TaleenJ">GitHub</a>.</p>

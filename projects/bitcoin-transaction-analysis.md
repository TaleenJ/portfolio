---
layout: default
title: Bitcoin Transaction Analysis
---

<p class="back-link"><a href="{{ '/#projects' | relative_url }}">← Back to projects</a></p>

# Bitcoin Transaction Analysis

<p class="project-meta">Machine learning · ransomware detection · 1M+ transactions · Fall 2025 · Group project with Travis Counihan and David Nguyen</p>

## Overview

This was a **group project with Travis Counihan and David Nguyen** — an introduction to approaching and practicing machine learning algorithms with a goal of finding **ransomware attacks in Bitcoin transaction data**. The models were trained from an openly available UCI dataset titled **“Bitcoin Heist Ransomware Address.”**

The work walks through a full experimentation pipeline — preprocessing, feature work, model training, and evaluation — so different algorithms can be compared on the same ransomware-address detection task.

## Pipeline

1. **Data preparation** — clean and structure large transaction tables; handle missing or noisy fields (`DataAfterPreprocessing` notebook).
2. **Feature engineering & scaling** — select and transform features useful for separating heist-related addresses from ordinary traffic.
3. **Train / test split** — 80/20 holdout for fair comparison across models.
4. **Model training** — run multiple algorithms on the same prepared features (`ModelingTechniques` notebook).
5. **Evaluation** — compare models with ROC curves, precision–recall curves, and confusion matrices (`EvaluationMetrics` notebook).

## Algorithms tested

- **Random Forest**
- **Support Vector Machine (SVM)**
- **K‑Nearest Neighbors (KNN)**
- **K‑Means clustering** (unsupervised structure / comparison)

The goal was not a single “winner,” but a clear, side-by-side view of how different families of models behave when the target is ransomware-related Bitcoin activity.

## Technical stack

Python · pandas · NumPy · scikit-learn · Jupyter notebooks

## Results & impact

Built a reproducible ML workflow on a high-volume financial dataset aimed at ransomware-threat detection in Bitcoin transactions, with shared preprocessing and cross-model evaluation artifacts (ROC, precision–recall, confusion matrices).

## Paper & code

- **Repository:** [ML-Modeling-for-Bitcoin-Transactions](https://github.com/TaleenJ/ML-Modeling-for-Bitcoin-Transactions)
- **Paper (PDF):** [Detecting Ransom Threats with Machine Learning Models for Bitcoin Transactions](https://github.com/TaleenJ/ML-Modeling-for-Bitcoin-Transactions/blob/main/Detecting_Ransom_Threats_with_Machine_Learning_Models_for_Bitcoin_Transactions.pdf)

<p class="projects-more">More projects on my <a href="https://github.com/TaleenJ">GitHub</a>.</p>

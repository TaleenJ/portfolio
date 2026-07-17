---
layout: default
title: Titan Trails
---

<p class="back-link"><a href="{{ '/#projects' | relative_url }}">← Back to projects</a></p>

# Titan Trails

<p class="project-meta">Indoor navigation · CSU Fullerton · Team lead · Spring 2026</p>

## Overview

Titan Trails is an indoor navigation app for CSUF’s Engineering Building. Outdoor GPS is unreliable indoors, so the system uses Wi‑Fi RSSI fingerprinting to estimate a user’s live position, then routes them to a chosen floor and room on a mapped floor plan.

Users can create an account to save searches. On Android, the phone scans nearby access points; those signal strengths are sent to a machine learning model that predicts coordinates. From that live location, Dijkstra’s algorithm computes a shortest path across a graph of rooms, hallways, stairs, and elevators.

## Architecture

The app is split into a mobile client and a Python backend:

1. **Client (React Native / Expo)** — sign-in, building list, explore map, room selection, Wi‑Fi scan trigger, and route display on floor plans.
2. **Auth & data (Firebase)** — accounts and saved user info.
3. **API & models (Python / FastAPI)** — serves trained regressors (stored as `.pkl`) that map RSSI vectors to (x, y) coordinates.
4. **Fingerprinting pipeline** — reference points ~1m apart on each floor; router IDs aligned to floor overlays so live scans can be matched to the training schema (missing routers default to −100).

Routing uses a **per-floor adjacency list** of nodes. Bridge nodes (stairs / elevators) connect floors with a higher cost penalty so the path prefers finishing on the current floor before changing levels.

## Technical implementation

- **Positioning models:** Started with **K‑Nearest Neighbors** for speed; moved to **Random Forest** (100 estimators) after KNN underfit. Models regress X/Y from fingerprinted RSSI features.
- **Routing:** **Dijkstra’s algorithm** on the floor graph; uniform edge cost of 1 between hallway nodes; bridge nodes penalized (cost 15) to discourage unnecessary floor changes.
- **Scanning:** Native Android Wi‑Fi scanning with location permissions (iOS does not expose the same multi-AP scan APIs).
- **Scope:** Proof of concept focused on the Engineering Building, with a path to expand campus-wide.

## Technical stack

React Native · TypeScript · Expo · Python · FastAPI · scikit-learn (KNN, Random Forest) · Jupyter · Firebase · Dijkstra graph routing

## Results & impact

- Train/test evaluation (80/20): KNN **R² ≈ 0.60**; Random Forest **R² ≈ 0.79**.
- Cluster mean-squared distance checks suggested roughly **~1.4 m** average error for the Random Forest model (vs ~1.9 m for KNN), using the project’s floor-unit scale.
- Successful first-floor routing demos showed live position updates and path display while walking the building.

Signal noise from walls, hallway clutter, and phone occlusion still limits accuracy — more repeated walks and stable access-point coverage would tighten predictions further.

## Images

<div class="image-grid">
  <div class="media-placeholder">Screenshot / figure placeholder — add later</div>
  <div class="media-placeholder">Screenshot / figure placeholder — add later</div>
  <div class="media-placeholder">Screenshot / figure placeholder — add later</div>
</div>

<p class="projects-more">More projects on my <a href="https://github.com/TaleenJ">GitHub</a>.</p>

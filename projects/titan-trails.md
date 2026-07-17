---
layout: default
title: Titan Trails
permalink: /projects/titan-trails/
---

<p class="back-link"><a href="{{ site.baseurl }}/#projects">← Back to projects</a></p>

# Titan Trails

<p class="project-meta">Indoor navigation · CSU Fullerton · Team lead · Spring 2026</p>

## Overview

Titan Trails is an indoor navigation app for CSUF’s Engineering Building. Outdoor GPS is unreliable indoors, so the system uses Wi‑Fi **received signal strength indicators (RSSI)** to estimate a user’s live position across **all four floors**, then routes them to a chosen floor and room on the mapped floor plans.

Users can create an account to save searches. On Android, the phone scans nearby access points; those signal strengths are sent to a machine learning model that predicts coordinates. From that live location, Dijkstra’s algorithm computes a shortest path across a graph of rooms, hallways, stairs, and elevators.

## Architecture

The app is split into a mobile client and a Python backend:

1. **Client (React Native / Expo)** — sign-in, building list, explore map, room selection, Wi‑Fi scan trigger, and route display on floor plans.
2. **Auth & data (Firebase)** — accounts and saved user info.
3. **API & models (Python / FastAPI)** — serves trained regressors (stored as `.pkl`) that map RSSI vectors to (x, y) coordinates.
4. **Fingerprinting pipeline** — reference points ~1m apart on **each of the four floors**; router IDs aligned to each floor overlay so live scans can be matched to the training schema (missing routers default to −100).

Routing uses a **per-floor adjacency list** of nodes for floors 1–4. Bridge nodes (stairs / elevators) connect floors with a higher cost penalty so the path prefers finishing on the current floor before changing levels.

## Models & prediction

Models were built in Jupyter notebooks and trained on the fingerprinted RSSI maps for **all four floors**. Each model predicts a continuous **(x, y)** location from the user’s live scan on the active floor.

- **K‑Nearest Neighbors (first model)** — chosen for simplicity and fast response. We tuned **K** with an elbow plot, but the model underfit live user scans and left too much overlap between predicted location clusters.
- **Random Forest (second model)** — 100 estimators. Router IDs from the scan are matched to the floor’s known access points; missing routers are filled with −100 so the feature vector stays complete. This model generalized better and became the production predictor.

**What the dots mean:** each colored point is a predicted position on the floor from RSSI measured at the user’s spot. Red markers in the cluster plots show where we were standing (reference / centroid locations) when we collected evaluation scans.

### Before fine-tuning — overlapping clusters

Early predictions mixed nearby hallway regions together. Cluster boundaries overlapped, which made live location less trustworthy for routing.

<figure class="project-figure">
  <img src="{{ '/assets/images/titan-trails/clusters-before.png' | relative_url }}" alt="Scatter plot of predicted floor positions with overlapping orange, green, and blue clusters before fine-tuning">
  <figcaption>Before fine-tuning: predicted position clusters overlap, so nearby spots on the floor are harder to separate.</figcaption>
</figure>

### After fine-tuning — clearer separation

After moving to Random Forest and tightening evaluation around standing points, clusters separate cleanly. The same RSSI → (x, y) idea, but with much less confusion between regions.

<figure class="project-figure">
  <img src="{{ '/assets/images/titan-trails/clusters-after.png' | relative_url }}" alt="Scatter plot of predicted floor positions with three clearly separated clusters after fine-tuning">
  <figcaption>After fine-tuning: three distinct clusters with clearer separation — better live-location predictions for routing.</figcaption>
</figure>

### Live floor predictions

On the floor plan, orange dots show predicted position points along the hallways from RSSI scans — the live user location signal the router uses before Dijkstra draws a path.

<figure class="project-figure project-figure-wide">
  <img src="{{ '/assets/images/titan-trails/floor-predictions.png' | relative_url }}" alt="Engineering building floor plan with orange predicted position points along hallways">
  <figcaption>Floor overlay example: predicted position points from RSSI along corridors — fingerprinting and live location covered all four floors of the Engineering Building.</figcaption>
</figure>

## Technical implementation

- **Routing:** Dijkstra’s algorithm on the floor graph; edge cost 1 between hallway nodes; bridge nodes (stairs / elevators) cost 15 so routes prefer finishing on the current floor first.
- **Scanning:** Native Android Wi‑Fi scanning with location permissions (iOS does not expose the same multi-AP scan APIs).
- **Scope:** Indoor live location and routing for the **full Engineering Building (floors 1–4)**, with a path to expand campus-wide.

## Technical stack

React Native · TypeScript · Expo · Python · FastAPI · scikit-learn (KNN, Random Forest) · Jupyter · Firebase · Dijkstra graph routing

## Results & conclusions

Evaluation used an **80/20** train/test split and standing-point scans inside the building:

- **R²:** KNN ≈ **0.60** → Random Forest ≈ **0.79**
- **Cluster mean-squared distance:** KNN averaged about **~1.9 m** error; Random Forest about **~1.4 m** (using the project’s 25 units ≈ 1 meter scale)
- Live walk tests often placed the marker within roughly **1–2 meters** of where we stood, and UI overlays lined up with doorways from our point of view

**Takeaway:** fine-tuning and swapping to Random Forest clearly improved cluster separation and usable live location. Remaining error still comes from walls, hallway clutter, phone occlusion, and inconsistent access-point visibility — more repeated walks and stable APs would tighten it further.

<p class="projects-more">More projects on my <a href="https://github.com/TaleenJ">GitHub</a>.</p>

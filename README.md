# Spatio-Temporal Crowd Risk Assessment System

A real-time crowd monitoring and early-risk assessment system that combines **point-based crowd detection** with **motion behavior analysis** to identify potentially unsafe crowd conditions from video streams.

This project is designed to move beyond simple crowd counting by incorporating both **spatial information** (how many people are present and where they are located) and **temporal information** (how the crowd is moving over time). The final output is a **continuous crowd risk score** with interpretable safety levels such as **Low, Medium, and High**.

---

## Table of Contents

- [Overview](#overview)
- [Motivation](#motivation)
- [Key Features](#key-features)
- [System Architecture](#system-architecture)
- [How It Works](#how-it-works)
- [Pipeline](#pipeline)
- [Core Modules](#core-modules)
- [Technologies Used](#technologies-used)
- [Project Structure](#project-structure)
- [Datasets](#datasets)
- [Installation](#installation)
- [Usage](#usage)
- [Input and Output](#input-and-output)
- [Risk Modeling](#risk-modeling)
- [Visualization](#visualization)
- [Results and Evaluation](#results-and-evaluation)
- [Applications](#applications)
- [Limitations](#limitations)
- [Future Work](#future-work)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgements](#acknowledgements)

---

## Overview

Crowd safety is a major concern in public spaces such as festivals, stadiums, railway stations, religious gatherings, political rallies, and urban events. Conventional systems often focus only on surveillance or people counting, but they do not fully capture whether the crowd is behaving in a safe or risky way.

This project addresses that gap by building a **spatio-temporal crowd risk assessment framework** that:

- Detects and counts individuals in dense scenes using **P2PNet**
- Analyzes crowd motion using **optical flow**
- Extracts behavioral indicators such as:
  - velocity
  - acceleration
  - entropy / turbulence
  - counterflow
- Fuses these features into a **risk score**
- Provides real-time monitoring through an interactive interface

The goal is to support **early warning and proactive intervention** before dangerous crowd situations escalate.

---

## Motivation

Many crowd analysis systems stop at:
- people counting
- density estimation
- anomaly detection in a narrow sense

However, real-world crowd risk depends on more than just the number of people. A crowd can become unsafe because of:
- sudden directional conflicts
- panic-like acceleration
- high local density
- turbulent movement patterns
- unstable flow behavior

This project was developed to create a more practical safety-oriented system by combining:
1. **Spatial understanding** of the crowd
2. **Temporal behavior analysis**
3. **Risk-aware decision logic**

---

## Key Features

- **Point-Based Crowd Detection**
  - Uses P2PNet for dense crowd localization and counting
  - Avoids limitations of bounding-box-based detection in crowded scenes

- **Motion Behavior Analysis**
  - Uses optical flow to capture how the crowd is moving between frames
  - Extracts meaningful motion features rather than relying only on raw flow

- **Spatio-Temporal Risk Scoring**
  - Combines density and motion indicators into a unified risk value

- **Temporal Smoothing**
  - Applies EMA (Exponential Moving Average) to stabilize noisy frame-wise risk predictions

- **Interactive Visualization**
  - Displays crowd count, motion patterns, density understanding, and risk levels

- **Real-Time Safety Monitoring**
  - Suitable for practical surveillance-style use cases

---

## System Architecture

The system consists of three main stages:

1. **Spatial Analysis**
   - Detect people
   - Estimate crowd count
   - Derive density-related features

2. **Temporal Analysis**
   - Compute motion between frames using optical flow
   - Extract behavior descriptors

3. **Risk Modeling**
   - Fuse spatial and temporal features
   - Compute final risk score
   - Map score to risk levels

---

## How It Works

Given a video stream or uploaded video, the system first extracts frames. Each frame is passed through a **point-based crowd localization model (P2PNet)** to estimate head-point positions and total crowd count. This gives the system a spatial understanding of the scene.

Next, consecutive frames are analyzed using **optical flow** to measure motion patterns in the crowd. From the flow field, the system derives behavior features such as movement speed, directional disorder, counterflow, and acceleration-like changes.

These features are then combined using a **weighted risk model**, producing a risk score for each frame or time segment. To avoid sudden fluctuations caused by noise, the score is smoothed over time using **EMA**. The final output is shown as a risk level such as **Low**, **Medium**, or **High**, along with visual aids.

---

## Pipeline

```text
Video Input
   ↓
Frame Extraction
   ↓
Spatial Analysis (P2PNet)
   ├── Head Point Detection
   ├── Crowd Counting
   └── Density Estimation / Density Features
   ↓
Temporal Analysis (Optical Flow)
   ├── Velocity
   ├── Acceleration
   ├── Entropy / Turbulence
   └── Counterflow
   ↓
Feature Fusion
   ↓
Risk Score Computation
   ↓
EMA Smoothing
   ↓
Risk Level Output (Low / Medium / High)
   ↓
Visualization Dashboard

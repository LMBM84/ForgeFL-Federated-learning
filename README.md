# 🏭 Industrial Cross-Plant Predictive Maintenance

<div align="center">

![Status](https://img.shields.io/badge/Status-Active-success?style=flat-square)
![Domain](https://img.shields.io/badge/Domain-Aerospace_&_Defense-blue?style=flat-square)
![Methodology](https://img.shields.io/badge/AI-Federated_Learning-orange?style=flat-square)
![License](https://img.shields.io/badge/License-Apache_2.0-blue?style=flat-square)

**A decentralized approach to RUL (Remaining Useful Life) prediction across heterogeneous operating fleets.**

[View Demo](#) • [Read the Paper](#) • [Report Bug](#)

</div>

---

## 📖 Overview

![Engine Architecture](https://via.placeholder.com/1000x300?text=C-MAPSS+Turbofan+Engine+Architecture)

This project targets a critical challenge in aerospace and industrial sectors: **Predictive Maintenance (PdM)** for complex aero-propulsion systems.

Specifically, this repository implements a Federated Learning solution for **Commercial Turbofan Engines (90,000 lb thrust class)**. In this realistic scenario, a fleet of engines operates under vastly different flight envelopes, creating a "Cross-Plant" (or Cross-Fleet) data challenge:

**Diverse Operating Conditions:** Altitude (0–42k ft), Mach Number (0–0.84), and Throttle Resolver Angle (20–100).
**Unknown Variations:** Each engine possesses unique, unknown initial wear and manufacturing variations.
* **Privacy Constraints:** Flight data from different operators cannot be centralized due to competitive sensitivity and data sovereignty.

Collaborative learning significantly improves RUL prediction accuracy by aggregating degradation patterns from the entire fleet without exposing raw flight telemetry.

---

## 🚫 The Challenge: Why Centralization Fails

Traditional cloud-centric approaches fail in this setting due to four critical friction points:

| Constraint | Description |
| :--- | :--- |
| **🔒 Data Sovereignty** | Machine telemetry is often legally restricted to remain on-site or within national borders. Centralization introduces regulatory friction that blocks collaboration. |
| **🛡️ Process Confidentiality** | High-frequency sensor signals implicitly encode proprietary operating parameters. Exposing raw data risks leaking **Intellectual Property (IP)** between plants. |
| **💸 Bandwidth Economics** | Continuous upload of multi-sensor time-series data creates high, recurring network costs that scale linearly with the number of machines. |
| **🔌 Operational Resilience** | Factory-floor systems require low latency and must tolerate intermittent connectivity. Cloud dependencies introduce unacceptable **single points of failure**. |

---

## 💡 The Solution: Federated Learning

![Architecture Diagram](https://via.placeholder.com/800x400?text=Federated+Learning+Architecture)

This project investigates a **Federated Learning (FL)** architecture to train predictive maintenance models collaboratively while respecting industrial constraints.

### Key Capabilities
* **Local Data Retention:** Raw sensor data (Temperatures, Pressures, Speeds) never leaves the local environment.
* **Bandwidth Efficiency:** Communication is reduced to compressed model updates rather than raw time-series streams.
* **Heterogeneity Robustness:** The model handles non-IID conditions caused by different fault modes (HPC vs. Fan Degradation) across different clients.

---

## 📊 Evaluation & Datasets

The system is evaluated using the **NASA C-MAPSS (Commercial Modular Aero-Propulsion System Simulation)** benchmark. This high-fidelity physics-based simulation generates run-to-failure trajectories for a fleet of engines.

### Dataset Composition
The project utilizes four distinct sub-datasets to simulate increasing levels of Federated Heterogeneity:

| Dataset | Train/Test Trajectories | Op. Conditions | Fault Modes | Challenge Level |
| :--- | :--- | :--- | :--- | :--- |
| **FD001** | 100 / 100 | 1 (Sea Level) | 1 (HPC Degradation) | **Low** (Baseline) |
| **FD002** | 260 / 259 | 6 (Mixed) | 1 (HPC Degradation) | **Medium** (Concept Drift) |
| **FD003** | 100 / 100 | 1 (Sea Level) | 2 (HPC + Fan) | **Medium** (Label Shift) |
| **FD004** | 248 / 249 | 6 (Mixed) | 2 (HPC + Fan) | **High** (Non-IID) |

### Input Features (21 Sensors)
The model utilizes 21 sensor outputs characterizing the engine's thermodynamic state:

* **Temperatures:** Total temp at Fan Inlet (T2), LPC Outlet (T24), HPC Outlet (T30), LPT Outlet (T50).
* **Pressures:** Pressure at Fan Inlet (P2), Bypass Duct (P15), HPC Outlet (P30).
* **Speeds:** Physical Fan Speed (Nf), Core Speed (Ne), and Corrected Speeds.
* **Ratios:** Bypass Ratio (BPR), Engine Pressure Ratio (EPR).

### Failure Definition
Each engine starts with varying degrees of initial wear (normal)and degrades over time. The **RUL (Remaining Useful Life)** is defined as the number of operational cycles remaining until the system reaches a predefined failure threshold (Health Index = 0).



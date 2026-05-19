
Here is link to the files: https://drive.google.com/drive/folders/15A93PN-F0PT2Glw1rvyK_c7LEyaZv7VF?usp=sharing
# Optimized Flood Sensor Placement for Urban Waterlogging Monitoring — Gurugram

A network-based optimization framework for strategically placing flood sensors in urban drainage systems under budget and uncertainty constraints. This project focuses on Gurugram, India, integrating graph theory, GIS, optimization, and noise-aware flood detection to improve real-time urban flood monitoring.

---

# Overview

Urban flooding and road waterlogging are recurring problems in rapidly urbanizing Indian cities. Instead of placing flood sensors arbitrarily, this project formulates sensor placement as a network optimization problem using:

- Directed drainage network modeling
- Node importance scoring
- Coverage optimization
- Gaussian noise-aware detection modeling
- Binary Integer Programming (BIP)
- Sensitivity analysis under varying sensor noise levels

The framework determines the optimal locations of flood sensors to maximize monitoring effectiveness while minimizing redundancy and operating within budget constraints.

---

# Key Features

- Directed graph representation of urban drainage networks
- Entropy-based node importance weighting
- Upstream observability modeling
- Gaussian sensor noise modeling
- Noise-aware optimization objective
- Binary Integer Programming using PuLP + CBC solver
- Sensitivity analysis for robustness evaluation
- GIS-based spatial visualization of sensor locations
- Scalable framework applicable to other cities

---

# Study Area

## Gurugram, Haryana, India

The study focuses on flood-prone urban regions of Gurugram, particularly:

- NH-48 corridor
- Hero Honda Chowk
- IFFCO Chowk underpass
- Sikanderpur
- Sohna Road
- Metro connectivity zones
- Major underpasses and low-lying regions

The drainage network and road system are modeled as a directed graph using elevation gradients and connectivity.

---

# Methodology

The methodology pipeline includes:

```text
Data Acquisition
        ↓
Directed Network Modeling
        ↓
Node Importance Scoring
        ↓
Coverage Matrix Construction
        ↓
Noise-Aware Detection Modeling
        ↓
Binary Integer Optimization
        ↓
Sensitivity Analysis
```

---

## 1. Network Modeling

The drainage/road network is represented as:

```math
G = (V, E)
```

Where:

- `V` → network nodes (junctions, manholes, drainage points)
- `E` → directed edges representing flow paths

Flow direction is assigned using DEM-derived elevation gradients.

---

## 2. Coverage Matrix

A sensor placed downstream can observe upstream flooding conditions.

```math
c_{ij} =
\begin{cases}
1, & \text{if sensor at } i \text{ observes node } j \\
0, & \text{otherwise}
\end{cases}
```

Constraints include:

- Upstream connectivity
- Maximum network distance (`Dmax = 2000 m`)
- Directional observability

---

## 3. Node Importance Weighting

Each node receives an importance score based on:

- Historical flooding
- Topography
- Infrastructure proximity
- Outfall proximity

Composite importance function:

```math
w_j =
\alpha_1 s_{flood}(j)
+ \alpha_2 s_{topo}(j)
+ \alpha_3 s_{infra}(j)
+ \alpha_4 s_{outfall}(j)
```

Weights used:

- Flood history → 0.4
- Topography → 0.3
- Infrastructure → 0.2
- Outfall proximity → 0.1

---

## 4. Noise-Aware Detection Model

Real flood sensors experience uncertainty and measurement noise.

Gaussian noise model:

```math
\tilde{h} = h + \eta,
\quad
\eta \sim \mathcal{N}(0, \sigma^2)
```

Detection probability:

```math
P(\mathrm{detect}\mid h)
=
1 -
\Phi\left(
\frac{h_{threshold} - h}{\sigma}
\right)
```

Parameters used:

- Noise standard deviation: `σ = 0.05 m`
- Detection threshold: `0.10 m`

---

# Optimization Problem

The placement problem is formulated as a Binary Integer Programming (BIP) problem.

Objective:

```math
\max
\sum_{j \in V}
w_j
\cdot
\min
\left(
1,
\sum_{i \in V_s}
c_{ij} x_i
\right)
\cdot
p_{noise}(j)
```

Subject to:

- Budget constraint
- Sensor separation constraint
- Outfall coverage constraint
- High-importance node coverage
- Binary decision constraints

Solver used:

- PuLP
- CBC Solver

---

# Results

## Optimized Deployment

| Metric | Value |
|---|---|
| Total Sensors | 40 |
| Budget Utilized | INR 1,200,000 |
| Nodes Covered | 2,158 |
| Total Network Nodes | 81,469 |
| Minimum Sensor Separation | 771 m |
| Mean Detection Probability | 2.28% |

---

## Strategic Sensor Locations

Top flood-monitoring locations include:

- Hero Honda Chowk
- IFFCO Chowk underpass
- Sikanderpur Chowk
- Rajiv Chowk
- HUDA City Centre
- Medanta Hospital vicinity
- Fortis Hospital road

---

# Noise Model Analysis

## Detection Probability Curve

The Gaussian noise model produces a sigmoidal detection response:

- 50% detection probability at threshold depth
- Near-certain detection for deeper flooding

## Noise Priority Distribution

The network exhibits a bimodal priority distribution:

- Low-risk regions
- High flood-priority regions

This improves targeted sensor allocation.

---

# Sensitivity Analysis

Noise robustness was tested for:

```text
σ ∈ [0.025 m, 0.20 m]
```

Observed trends:

- Stable coverage across all noise levels
- Increased detection reliability at higher noise
- Gradual reduction in uncertainty
- Robust placement under varying conditions

---

# Technologies Used

- Python
- GeoPandas
- NetworkX
- NumPy
- Pandas
- Matplotlib
- PuLP Optimization
- OpenStreetMap
- SRTM DEM
- Jupyter Notebook

---

# Repository Structure

```text
.
├── data/
│   ├── network/
│   ├── flood_data/
│   ├── dem/
│   └── infrastructure/
│
├── notebooks/
│   └── Gurugram_Flood_Sensor_Placement.ipynb
│
├── outputs/
│   ├── maps/
│   ├── sensitivity_analysis/
│   └── sensor_locations/
│
├── figures/
│   ├── noise_model.png
│   └── sensitivity_analysis.png
│
├── reports/
│   └── project_report.pdf
│
├── requirements.txt
└── README.md
```

---

# Installation

Clone the repository:

```bash
git clone https://github.com/your-username/gurugram-flood-sensor-placement.git
cd gurugram-flood-sensor-placement
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Launch notebook:

```bash
jupyter notebook
```

---

# Future Improvements

- Hydrodynamic coupling with HEC-RAS / SWMM
- Real-time IoT integration
- Dynamic sensor repositioning
- Machine learning flood prediction
- Multi-city deployment framework
- Mobile flood sensor systems

---

# Citation

```bibtex
@project{Srinayak2026FloodSensors,
  author = {Karthik Chandra Srinayak},
  title = {Optimized Placement of Flood Sensors to Monitor Road Waterlogging in Urban Areas: Gurugram},
  institution = {Indian Institute of Technology Gandhinagar},
  year = {2026}
}
```

---

# Acknowledgements

- Prof. Udit Bhatia
- IIT Gandhinagar
- OpenStreetMap contributors
- SRTM DEM mission

# 📡 Modal and Forced Vibration Analysis of a Telescopic Aerial (MDOF System)

This repository presents a finite element implementation for analysing the **free and forced vibrations** of a 3-element telescopic stainless steel aerial using the **Euler-Bernoulli beam theory** in 2D. The system is modelled as an MDOF (Multi-Degree of Freedom) system using beam elements with translational and rotational degrees of freedom.

---

## 🧠 Problem Overview

A **telescopic radio aerial** composed of 3 hollow concentric steel tubes (0.2 m each, total length 0.6 m) is analysed in flexure. Each tube is modelled using a **2D beam element** with two degrees of freedom per node: translational and rotational.

### Given:

| Tube | Outer Dia (mm) | Inner Dia (mm) |
|------|----------------|----------------|
| 1    | 2              | 1              |
| 2    | 3              | 2              |
| 3    | 4              | 3              |

---

## 🛠️ Methodology

### 1. Finite Element Modelling
- Each beam element has **4 DOFs** (2 nodes × [translational + rotational])
- Global matrix size: **8×8**
- Mass matrix includes a **lumped 1g stopper mass** at the aerial tip (translational only)

### 2. Boundary Conditions
- The thick end (element 3) is **fixed (cantilevered)**  
- DOFs corresponding to this fixed node are removed from the global matrices

### 3. Eigenvalue Analysis
- Generalised eigenvalue problem solved using:
```python
from scipy.linalg import eigh
eigenvals, eigenvecs = eigh(K, M)
```
- Frequencies extracted from `sqrt(eigenvalues) / (2 * pi)`
- Mode shapes visualised for translational DOFs

### 4. Rayleigh Damping
To enforce ζ₁ = ζ₃ = 2%, Rayleigh damping coefficients `alpha`, `beta` are computed using:
```
[C] = alpha * [M] + beta * [K]
```
Damping for 2nd mode is estimated using:
```
ζ₂ = 0.5 * (alpha / ω₂ + beta * ω₂)
```

### 5. Forced Vibration Analysis
Using direct frequency response:
```
H(ω) = [-ω² * M + jωC + K]⁻¹
```
- Receptance computed between aerial tip and its neighbour node  
- Frequency sweep across first three modes  
- Results plotted in magnitude (dB) and phase  

---

## 📊 Results

### ✔️ Natural Frequencies:

| Mode | Frequency (Hz) |
|------|----------------|
| 1    | 10.47          |
| 2    | 40.19          |
| 3    | 106.64         |
| 4    | 253.20         |
| 5    | 441.94         |
| 6    | 730.75         |

### ✔️ Mode Shapes

Visualised in 2D using only **translational components** of eigenvectors  
![Mode Shapes](figures/modeshape_plot.png)

### ✔️ Rayleigh Damping Coefficients:

- **alpha (mass):** 2.3961 s⁻¹  
- **beta (stiffness):** 5.4361 × 10⁻⁵ s  
- **Damping ratio for 2nd mode:** ~1.16%

### ✔️ Driving Point Receptance

![Receptance](figures/receptance_plot.png)

- Shows resonance peaks near natural frequencies  
- Damping evident in amplitude reduction at high frequency  

---

## 🧾 Discussion

### 🧩 Observations:
- Two rigid body modes (translational & rotational) expected in the unconstrained model  
- Second mode has nodal point ~0.23 m from fixed end  
- Absence of anti-resonance suggests symmetric modal influence at the tip  

### ⚠️ Model Limitations:
- 2D assumption: real-world system behaves in 3D  
- Boundary assumptions: perfectly fixed in model vs real clamping flexibility  
- Material damping and external factors (e.g., air drag) neglected  

---

## 🚀 Getting Started

### Requirements
- Python 3.x  
- NumPy, SciPy, Matplotlib  

### Run the Script
```bash
python aerial_vibration_analysis.py
```

---

## 📘 References

- Craig, R.R. & Kurdila, A.J. *Fundamentals of Structural Dynamics*, 2nd Ed.  
- Rao, S.S. *Mechanical Vibrations*, 6th Ed.  

---

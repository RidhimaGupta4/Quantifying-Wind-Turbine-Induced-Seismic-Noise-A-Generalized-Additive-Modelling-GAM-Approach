# Quantifying Wind Turbine-Induced Seismic Noise: A Statistical Modelling Approach

[cite_start]This repository contains the analytical framework and R code for my dissertation project, focused on the **Eskdalemuir Seismological Array (EKA)**[cite: 9, 18]. [cite_start]The project develops a robust statistical method to isolate and quantify seismic energy directly attributable to wind turbine activity using **Generalized Additive Models (GAMs)**[cite: 31, 632].

## 📌 Project Overview
[cite_start]The UK’s Net-Zero Strategy requires doubling on-shore wind capacity by 2030[cite: 73, 75]. [cite_start]However, modern turbines near the EKA can emit ground vibrations (0.5–8 Hz) that potentially mask low-magnitude nuclear signals[cite: 125, 126]. [cite_start]This study provides a regulator-ready tool to quantify these effects and ensure compliance with the **0.336 nm RMS displacement** statutory limit[cite: 36, 128].

---

## 🛠 Data Processing Pipeline
The following pipeline was developed to transform high-rate raw seismic traces into a policy-relevant log-energy response variable[cite: 356]:

1.  **High-Rate Traces**: Raw velocity signals sampled at 100 Hz over 10-minute blocks[cite: 318, 332].
2.  **Spectral Transformation**: Conversion of traces into Power Spectral Density (PSD) using Welch’s method[cite: 325, 335].
3.  **Filtering & Weighting**: Trimming to the 0.5–8 Hz band and applying a Frequency-Domain Weighting Function (FDWF)[cite: 336, 368].
4.  **Integration**: Summing weighted PSD values to compute total seismic energy ($E$)[cite: 379, 380].
5.  **Normalization**: Applying a $log_{10}$ transform to stabilize variance for Gaussian modeling[cite: 383, 389].
6.  **Metadata Tagging**: Appending synchronized wind speed, direction, and turbine operational status[cite: 392, 396].

---

## 📈 Model Development & Selection
A forward-selection workflow was used to build the final Generalized Additive Model (GAM). [cite_start]Each iteration was evaluated based on the reduction in **Akaike Information Criterion (AIC)**[cite: 711, 854].

### Model Comparison Table [cite: 712]
| Model ID | Full Equation / Terms Added | $\Delta$ AIC (Relative) |
| :--- | :--- | :--- |
| **$M_1$** | Baseline: Operational Status only | -89.19 |
| **$M_3$** | Added Non-linear Wind Speed Spline $s(WindSpeed)$ | -106.73 |
| **$M_4$** | Added Cyclic Wind Direction Spline $s^{cyc}(WindDir)$ | -107.13 |
| **$M_5$** | Added Daily Random Effects $b_{Date}$ | -147.74 |
| **$M_6$** | Added Hourly Cyclic Smooth $s^{cyc}(Hour)$ | -147.85 |
| **$M_7$ (Full)** | **Added Operational interaction $s(WindSpeed, by=Op)$** | **Best Fit** |

**Key Insight**: The final model achieved an **Adjusted $R^2$ of 0.987**, explaining **99.1% of the deviance** in measurements[cite: 977].

---

## 📊 Key Findings
* **Significant Uplift**: Turbine operation is associated with a statistically significant increase in seismic energy, representing a relative increase of approximately 20%[cite: 22, 994].
* **Regulatory Compliance**: Even at peak operation, turbine-induced ground motion reached only **53% of the allowed limit** (0.1771 nm observed vs. 0.336 nm limit)[cite: 24, 1036].
* **Directional Sensitivity**: South-westerly winds produced higher energy readings due to the physical alignment between turbines and sensors[cite: 28, 534].

## 🚀 Repository Structure
* `/data`: Metadata and engineered block-level datasets[cite: 323, 1127].
* `/scripts`: R implementation using the `mgcv` package for REML-based smoothing[cite: 63, 752].
* `/plots`: Visual diagnostics including Q-Q plots and partial effect curves[cite: 68, 780].

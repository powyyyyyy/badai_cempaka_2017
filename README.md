# 🌧️ WRF Simulation and Validation of Tropical Cyclone Cempaka (2017)

> Numerical Weather Prediction using **WRF-ARW** and rainfall validation against **MSWEP** observations during the Tropical Cyclone Cempaka event (26–29 November 2017).

---

# 📖 Manual

Complete installation guide for **WRF**, **WPS**, **Docker**, and the simulation workflow:

👉 https://github.com/powyyyyyy/wrf_manual

---

# 📌 Project Overview

This project performs a high-resolution numerical weather simulation of **Tropical Cyclone Cempaka (2017)** using the **Weather Research and Forecasting (WRF-ARW)** model.

The simulation is initialized using **ERA5 Reanalysis** datasets and validated against **MSWEP** precipitation observations to evaluate the model's capability in reproducing an extreme rainfall event over southern Java.

The project includes both qualitative (spatial visualization) and quantitative (statistical validation) analyses.

---

# 🎯 Objectives

The objectives of this project are:

- Simulate the atmospheric conditions during Tropical Cyclone Cempaka.
- Generate hourly and accumulated rainfall using WRF.
- Analyze rainfall evolution throughout the event.
- Compare WRF rainfall with MSWEP observations.
- Evaluate model performance using spatial and statistical metrics.

---

# 📍 Case Study

## Tropical Cyclone Cempaka (2017)

Simulation Period

```text
26 November 2017 00:00 UTC
↓
29 November 2017 23:00 UTC
```

Study Area

- Southern West Java
- Central Java
- Special Region of Yogyakarta
- Southern Indian Ocean of Java

---

# 🛰️ Data Sources

## ERA5 Reanalysis

ERA5 is used as the atmospheric forcing for WRF.

Purpose:

- Initial Conditions (IC)
- Boundary Conditions (BC)

Downloaded Variables

Pressure Level

- Geopotential
- Temperature
- Relative Humidity
- Specific Humidity
- U-component Wind
- V-component Wind

Single Level

- Surface Pressure
- Mean Sea Level Pressure
- Total Precipitation
- Skin Temperature
- Sea Surface Temperature
- 2 m Temperature
- 2 m Dew Point
- 10 m U Wind
- 10 m V Wind
- Soil Temperature
- Soil Moisture
- Land-Sea Mask
- Geopotential

---

## MSWEP

MSWEP (Multi-Source Weighted-Ensemble Precipitation)

Used as the rainfall reference dataset for validating WRF simulations.

---

# ⚙️ Model Configuration

## WRF-ARW

Nested Domain Configuration

| Domain | Resolution |
|---------|-----------:|
| d01 | 27 km |
| d02 | 9 km |
| d03 | 3 km |

The highest-resolution domain (**d03**) is used for statistical validation.

---

# 📂 Notebook Workflow

The notebook is organized into several processing stages.

---

# 1️⃣ Load WRF Output

Read all simulation outputs:

```python
wrfout_d01*
wrfout_d02*
wrfout_d03*
```

Variables extracted include:

- Latitude
- Longitude
- Time
- Rainfall
- Atmospheric variables

---

### Output

📷 *(Insert figure here)*

---

# 2️⃣ Compute Total Rainfall

WRF stores accumulated rainfall using:

```text
RAINC
+
RAINNC
```

where

```text
RAINC
```

Convective Rainfall

and

```text
RAINNC
```

Non-Convective Rainfall.

Therefore,

```text
Total Rainfall

=

RAINC + RAINNC
```

Hourly rainfall is calculated by differencing consecutive accumulated outputs:

```text
Rain(t)

=

Accumulated(t)

-

Accumulated(t-1)
```

---

# 📈 Time Series Analysis

Analyze rainfall evolution during the event.

Axis

```text
X = Time

Y = Rainfall (mm/hour)
```

---

## Domain d02

📷 *(Insert figure here)*

---

## Domain d03

📷 *(Insert figure here)*

---

# 🗺️ Spatial Rainfall Distribution

## WRF Hourly Rainfall

Hourly rainfall distribution over the study area.

📷 *(Insert figure here)*

---

## WRF Accumulated Rainfall

Accumulated rainfall during the entire simulation.

📷 *(Insert figure here)*

---

# 🌧️ MSWEP Rainfall

## Hourly Rainfall

📷 *(Insert figure here)*

---

## Accumulated Rainfall

📷 *(Insert figure here)*

---

# 🔄 Interpolation

Before statistical validation,

MSWEP data are interpolated onto the WRF grid.

```text
MSWEP

↓

Interpolation

↓

WRF Grid
```

Purpose:

- Match spatial resolution
- Eliminate grid mismatch
- Ensure fair statistical comparison

---

# 📍 Domain d02 Evaluation

## Structural Similarity Index (SSIM)

SSIM evaluates the similarity of rainfall patterns.

Range

```text
0 → 1
```

Higher values indicate better spatial agreement.

---

### Result

| Metric | Value |
|---------|------:|
| SSIM | **0.336** |

### Interpretation

An SSIM value of **0.336** indicates that the simulated rainfall pattern shares partial structural similarity with MSWEP observations. Although the general rainfall system is reproduced, noticeable differences remain in the spatial distribution of precipitation.

---

## Rainfall Centroid Analysis

The rainfall centroid represents the spatial center of precipitation.

The distance between WRF and MSWEP centroids indicates how accurately WRF locates the main rainfall system.

---

### Result

| Metric | Value |
|---------|------:|
| Centroid Distance | **175.31 km** |

### Interpretation

The rainfall centroid generated by WRF is displaced by approximately **175 km** relative to MSWEP. This displacement suggests that the model successfully produced the rainfall system but shifted its location, contributing to lower statistical agreement.

---

# 📍 Domain d03 Evaluation

The highest-resolution domain (3 km) is used for quantitative validation.

Metrics include:

- RMSE
- MAE
- Bias
- Pearson Correlation
- R² Score

---

### Result Table

| Metric | Value |
|---------|------:|
| RMSE | **5.93 mm/hour** |
| MAE | **4.53 mm/hour** |
| Bias | **-0.85 mm/hour** |
| Pearson Correlation (r) | **0.034** |
| R² Score | **-0.11** |

### Interpretation

- **RMSE (5.93 mm/hour)** indicates that the overall rainfall error remains within a reasonable range for an extreme weather simulation.
- **MAE (4.53 mm/hour)** shows the average absolute deviation between simulated and observed rainfall.
- **Bias (-0.85 mm/hour)** suggests a slight underestimation of rainfall intensity by WRF.
- **Pearson Correlation (0.034)** indicates weak temporal and spatial correspondence with observations.
- **Negative R²** suggests that the simulated rainfall variability does not adequately explain the observed rainfall variability.

---

# 🗺️ Difference Map

Spatial rainfall difference

```text
WRF

-

MSWEP
```

Interpretation

Positive values

→ Overestimation

Negative values

→ Underestimation

📷 *(Insert figure here)*

---

# 📉 Scatter Plot

Relationship between

```text
WRF Rainfall

vs

MSWEP Rainfall
```

📷 *(Insert figure here)*

---

# 📊 Summary

| Evaluation | Result |
|------------|--------:|
| SSIM | **0.336** |
| Centroid Distance | **175.31 km** |
| RMSE | **5.93 mm/hour** |
| MAE | **4.53 mm/hour** |
| Bias | **-0.85 mm/hour** |
| Pearson Correlation | **0.034** |
| R² Score | **-0.11** |

---

# 🔍 Discussion

Several important findings can be drawn from this study:

- WRF successfully simulated the occurrence of heavy rainfall associated with Tropical Cyclone Cempaka.
- The simulated rainfall intensity is generally close to the observations, as indicated by the relatively low RMSE and small Bias.
- The spatial rainfall pattern differs from MSWEP, reflected by an SSIM value of **0.336**.
- The rainfall centroid is displaced by approximately **175 km**, indicating a noticeable spatial shift of the precipitation system.
- This displacement error is likely one of the primary reasons for the low correlation coefficient.
- While WRF captures the general evolution of the event, accurately reproducing the exact location and timing of extreme rainfall remains challenging.

---

# 🏁 Conclusion

This study demonstrates that the **WRF-ARW model** is capable of reproducing the overall characteristics of the rainfall event associated with **Tropical Cyclone Cempaka (26–29 November 2017)**.

Statistical evaluation shows that the simulated rainfall intensity is reasonably close to MSWEP observations, as reflected by the relatively low RMSE (**5.93 mm/hour**) and small Bias (**−0.85 mm/hour**). However, the weak Pearson Correlation (**0.034**) indicates limited agreement in the spatial-temporal distribution of rainfall.

Spatial validation further reveals an **SSIM of 0.336** and a rainfall centroid displacement of approximately **175 km**, suggesting that the primary limitation of the simulation is **displacement error**, where the rainfall system is reproduced but shifted from the observed location.

Overall, the model successfully represents the occurrence of the extreme rainfall event, but additional improvements—such as optimizing physical parameterizations, refining domain configuration, or applying object-based verification metrics—could further enhance simulation accuracy for tropical cyclone events over Indonesia.

---

# 🚀 Future Improvements

Potential developments for future work include:

- Evaluate multiple WRF physics parameterizations.
- Compare alternative nesting configurations.
- Validate against BMKG rain gauge observations.
- Apply categorical verification metrics (POD, FAR, CSI).
- Implement Fractions Skill Score (FSS).
- Perform object-based rainfall verification.
- Investigate displacement error using feature-based verification methods.

---

# 👨‍💻 Author

**Powy Alcuino**

Telkom University

Atmospheric Modeling • WRF • ERA5 • MSWEP • Tropical Cyclone • Numerical Weather Prediction

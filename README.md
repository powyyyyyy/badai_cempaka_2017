# 🌧️ WRF Simulation and Validation of Tropical Cyclone Cempaka (2017)

> Numerical Weather Prediction using WRF-ARW and validation against MSWEP rainfall observations.

---

## 📖 Manual

Untuk panduan instalasi WRF, WPS, Docker, dan workflow lengkap:

👉 https://github.com/powyyyyyy/wrf_manual

---

## 📌 Project Overview

Project ini bertujuan untuk mensimulasikan kejadian hujan ekstrem yang dipicu oleh **Siklon Tropis Cempaka** menggunakan **Weather Research and Forecasting Model (WRF-ARW)**.

Hasil simulasi kemudian dibandingkan dengan data observasi hujan **MSWEP** untuk mengevaluasi kemampuan model dalam mereproduksi pola hujan selama kejadian ekstrem.

---

## 🎯 Objectives

- Simulasi kondisi atmosfer selama Siklon Tropis Cempaka
- Analisis distribusi curah hujan
- Analisis perkembangan hujan terhadap waktu
- Validasi WRF terhadap MSWEP
- Evaluasi performa model menggunakan metrik statistik

---

## 📍 Case Study

### Tropical Cyclone Cempaka

Periode simulasi:

```text
26 November 2017 00:00 UTC
↓
29 November 2017 23:00 UTC
```

Wilayah fokus:

- Jawa Tengah
- DIY Yogyakarta
- Jawa Barat bagian selatan
- Samudra Hindia Selatan Jawa

---

# 🛰️ Data Sources

## ERA5 Reanalysis

Digunakan sebagai:

- Initial Condition
- Boundary Condition

Variabel:

- Temperature
- Relative Humidity
- Specific Humidity
- Geopotential
- U Wind
- V Wind

---

## MSWEP

Digunakan sebagai data referensi hujan.

Resolusi lebih tinggi dibanding data observasi konvensional dan cocok untuk evaluasi curah hujan ekstrem.

---

# ⚙️ Model Configuration

## WRF-ARW

Grid system:

| Domain | Resolution |
|----------|----------|
| d01 | 27 km |
| d02 | 9 km |
| d03 | 3 km |

---

# 📂 Notebook Workflow

Notebook dibagi menjadi beberapa tahapan utama.

---

## 1️⃣ Load WRF Output

Membaca seluruh file:

```python
wrfout_d01*
wrfout_d02*
wrfout_d03*
```

Output:

- Latitude
- Longitude
- Time
- Rainfall

---

### Output

📷 Masukkan gambar jika diperlukan

```md
![Load Data](images/load_data.png)
```

---

## 2️⃣ Compute Total Rainfall

WRF menyimpan hujan dalam bentuk akumulatif:

```text
RAINC
+
RAINNC
```

dengan:

```text
RAINC  = Convective Rainfall
RAINNC = Non-Convective Rainfall
```

Total Rainfall:

```text
RAIN = RAINC + RAINNC
```

---

### Formula

Hourly rainfall dihitung menggunakan:

```text
Rain(t)
=
Accumulated(t)
-
Accumulated(t-1)
```

---

# 📈 Time Series Analysis

Analisis perkembangan hujan terhadap waktu.

Sumbu:

```text
X = Time
Y = Rainfall (mm)
```

---

## Domain d02

📷


<img width="846" height="386" alt="d02_rainrate" src="https://github.com/user-attachments/assets/7a0e7807-1b86-4352-9f0d-abe5a00fa066" />



---

## Domain d03

📷


<img width="855" height="386" alt="d03_rainrate" src="https://github.com/user-attachments/assets/c3b3134c-b82d-4449-bd96-c9b1cf4a68ae" />



---

# 🗺️ Spatial Rainfall Distribution

## WRF Hourly Rainfall

Visualisasi distribusi hujan per jam.

📷


<img width="647" height="548" alt="d03 hourly rainfall" src="https://github.com/user-attachments/assets/5e60fb99-52cc-40b1-b0cd-c8f71d74b709" />



---

## WRF Accumulated Rainfall

Akumulasi hujan selama event.

📷


<img width="655" height="528" alt="d03_accumulated" src="https://github.com/user-attachments/assets/3547ccd9-0c88-4f58-a510-2db81fee7a0f" />



---

# 🌧️ MSWEP Rainfall

## Hourly Rainfall

📷


<img width="655" height="528" alt="mswep_hourly_observ" src="https://github.com/user-attachments/assets/68d38fbd-7743-4ce0-92dc-4fac94e705a0" />



---

## Accumulated Rainfall

📷


<img width="664" height="528" alt="mswep_accumulate_rainobserv" src="https://github.com/user-attachments/assets/2a0520e0-235b-4644-835e-f6d99c797b4f" />



---

# 🔄 Interpolation

Sebelum validasi:

```text
MSWEP
↓
Interpolated
↓
WRF Grid
```

Tujuan:

- Menyamakan grid
- Menghindari bias akibat perbedaan resolusi

---

# 📍 Domain d02 Evaluation

## Structural Similarity Index (SSIM)

Digunakan untuk mengevaluasi kemiripan pola hujan.

Range:

```text
0 → 1
```

Semakin mendekati:

```text
1
```

Semakin baik.

---

### Result

| Metric | Value |
|----------|----------|
| SSIM | XX |

---

## Rainfall Centroid Analysis

Menghitung jarak antara pusat hujan:

```text
WRF
vs
MSWEP
```

---

### Result

| Metric | Value |
|----------|----------|
| Centroid Distance | XX km |

---

### Visualization

📷

```md
![Centroid](images/centroid.png)
```

---

# 📍 Domain d03 Evaluation

Domain resolusi tertinggi digunakan untuk evaluasi statistik.

---

## RMSE

Root Mean Square Error

```text
Semakin kecil semakin baik
```

---

## MAE

Mean Absolute Error

```text
Semakin kecil semakin baik
```

---

## Bias

```text
Bias > 0  → Overestimate

Bias < 0  → Underestimate
```

---

## Pearson Correlation

```text
-1 → 1
```

Semakin mendekati:

```text
1
```

Semakin baik.

---

## R² Score

Koefisien determinasi.

Range:

```text
0 → 1
```

---

### Result Table

| Metric | Value |
|----------|----------|
| RMSE | XX |
| MAE | XX |
| Bias | XX |
| Correlation (r) | XX |
| R² | XX |

---

# 🗺️ Difference Map

Perbedaan:

```text
WRF
-
MSWEP
```

Interpretasi:

```text
Positif  → Overestimate

Negatif → Underestimate
```

---

📷

```md
![Difference Map](images/difference_map.png)
```

---

# 📉 Scatter Plot

Hubungan antara:

```text
WRF Rainfall
vs
MSWEP Rainfall
```

---

📷

```md
![Scatter Plot](images/scatter.png)
```

---

# 📊 Summary

| Evaluation | Result |
|------------|------------|
| SSIM | XX |
| Centroid Distance | XX km |
| RMSE | XX |
| MAE | XX |
| Bias | XX |
| Pearson r | XX |
| R² | XX |

---

# 🔍 Discussion

Hal yang dianalisis:

- Evolusi hujan selama Siklon Tropis Cempaka
- Perbedaan pola hujan antara WRF dan MSWEP
- Lokasi overestimate dan underestimate
- Pengaruh resolusi domain terhadap performa model
- Kemampuan WRF dalam mereproduksi hujan ekstrem

---

# 🏁 Conclusion

Kesimpulan utama terkait:

- Akurasi simulasi WRF
- Kesesuaian pola hujan terhadap observasi
- Pengaruh resolusi model terhadap hasil simulasi
- Kelayakan WRF untuk simulasi kejadian hujan ekstrem di Indonesia

---

## 👨‍💻 Author

Powy Alcuino

Telkom University

Atmospheric Modeling • WRF • ERA5 • MSWEP • Tropical Cyclone Cempaka

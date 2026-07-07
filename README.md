# 🌧️ Simulasi dan Validasi WRF pada Kejadian Siklon Tropis Cempaka (2017)

> Simulasi Numerical Weather Prediction (NWP) menggunakan **WRF-ARW** serta validasi curah hujan terhadap data observasi **MSWEP** pada kejadian Siklon Tropis Cempaka.

---

# 📖 Manual Instalasi

Panduan lengkap instalasi WRF, WPS, Docker, hingga proses running simulasi dapat dilihat pada repository berikut:

👉 https://github.com/powyyyyyy/wrf_manual

---

# 📌 Gambaran Proyek

Project ini bertujuan untuk mensimulasikan kejadian hujan ekstrem yang dipicu oleh **Siklon Tropis Cempaka** menggunakan model **Weather Research and Forecasting (WRF-ARW)**.

Model dijalankan menggunakan data **ERA5 Reanalysis** sebagai **Initial Condition (IC)** dan **Boundary Condition (BC)**. Selanjutnya, hasil simulasi dibandingkan dengan data curah hujan observasi **MSWEP** untuk mengetahui seberapa baik WRF mampu merepresentasikan distribusi hujan selama kejadian tersebut.

Analisis dilakukan secara:

- Visual (peta distribusi hujan)
- Temporal (time series)
- Statistik (RMSE, MAE, Bias, Korelasi, R²)
- Spasial (SSIM dan Rainfall Centroid)

---

# 🎯 Tujuan Penelitian

Penelitian ini bertujuan untuk:

- Mensimulasikan kondisi atmosfer saat Siklon Tropis Cempaka.
- Menghasilkan distribusi curah hujan menggunakan WRF.
- Melihat perkembangan hujan dari waktu ke waktu.
- Membandingkan hasil simulasi dengan data MSWEP.
- Mengevaluasi performa model menggunakan beberapa metrik statistik dan spasial.

---

# 📍 Studi Kasus

## Siklon Tropis Cempaka

Periode simulasi:

```text
26 November 2017 00:00 UTC
↓
29 November 2017 23:00 UTC
```

Wilayah penelitian meliputi:

- Jawa Barat bagian Selatan
- Jawa Tengah
- DI Yogyakarta
- Samudra Hindia Selatan Pulau Jawa

---

# 🛰️ Sumber Data

## ERA5 Reanalysis

Data ERA5 digunakan sebagai:

- Initial Condition (IC)
- Boundary Condition (BC)

Variabel Pressure Level:

- Geopotential
- Temperature
- Relative Humidity
- Specific Humidity
- U Wind
- V Wind

Variabel Single Level:

- Surface Pressure
- Mean Sea Level Pressure
- Total Precipitation
- Sea Surface Temperature
- Skin Temperature
- 2m Temperature
- 2m Dew Point
- 10m U Wind
- 10m V Wind
- Soil Temperature
- Soil Moisture
- Geopotential
- Land-Sea Mask

---

## MSWEP

MSWEP digunakan sebagai data referensi (observasi) curah hujan.

Sebelum dilakukan validasi, data MSWEP diinterpolasi terlebih dahulu agar memiliki resolusi grid yang sama dengan WRF.

---

# ⚙️ Konfigurasi Model

Model yang digunakan:

**WRF-ARW**

Konfigurasi domain:

| Domain | Resolusi |
|---------|----------:|
| d01 | 27 km |
| d02 | 9 km |
| d03 | 3 km |

Domain d03 digunakan sebagai domain utama untuk evaluasi statistik karena memiliki resolusi paling tinggi.

---

# 📂 Alur Notebook

Notebook dibagi menjadi beberapa tahapan utama.

---

# 1️⃣ Membaca Output WRF

Notebook membaca seluruh file:

```python
wrfout_d01*
wrfout_d02*
wrfout_d03*
```

Variabel yang diambil meliputi:

- Latitude
- Longitude
- Time
- Curah Hujan
- Variabel atmosfer lainnya

---

📷

*(Masukkan gambar output jika diperlukan)*

---

# 2️⃣ Menghitung Curah Hujan

WRF menyimpan hujan dalam bentuk akumulasi.

Curah hujan total dihitung menggunakan:

```text
RAIN = RAINC + RAINNC
```

dimana:

- RAINC = Convective Rainfall
- RAINNC = Non-Convective Rainfall

Karena nilai tersebut bersifat akumulatif, maka curah hujan per jam dihitung menggunakan:

```text
Rain(t)

=

Accumulated(t)

-

Accumulated(t-1)
```

---

# 📈 Analisis Time Series

Analisis ini bertujuan melihat perkembangan intensitas hujan selama simulasi.

Sumbu:

```text
X = Waktu

Y = Curah Hujan (mm/jam)
```

## Domain d02

📷

---

## Domain d03

📷

---

# 🗺️ Distribusi Curah Hujan WRF

## Curah Hujan per Jam

📷

---

## Curah Hujan Akumulasi

📷

---

# 🌧️ Data Observasi MSWEP

## Curah Hujan per Jam

📷

---

## Curah Hujan Akumulasi

📷

---

# 🔄 Interpolasi

Sebelum dilakukan validasi statistik, data MSWEP diinterpolasi ke grid WRF.

```text
MSWEP

↓

Interpolasi

↓

Grid WRF
```

Tujuan:

- Menyamakan resolusi grid
- Menghindari bias akibat resolusi berbeda
- Memastikan perbandingan dilakukan pada grid yang sama

---

# 📍 Evaluasi Domain d02

## Structural Similarity Index (SSIM)

SSIM digunakan untuk mengukur kemiripan pola distribusi hujan antara WRF dan MSWEP.

Nilai SSIM berada pada rentang:

```text
0 → 1
```

Semakin mendekati **1**, maka pola hujan semakin mirip.

### Hasil

| Metrik | Nilai |
|---------|-------:|
| SSIM | **0.336** |

### Interpretasi

Nilai SSIM sebesar **0.336** menunjukkan bahwa WRF mampu membentuk pola hujan yang secara umum menyerupai observasi, namun masih terdapat perbedaan distribusi spasial yang cukup besar.

---

## Rainfall Centroid

Rainfall Centroid digunakan untuk mengetahui seberapa jauh posisi pusat hujan hasil simulasi terhadap observasi.

### Hasil

| Metrik | Nilai |
|---------|-------:|
| Centroid Distance | **175.31 km** |

### Interpretasi

Pusat hujan hasil simulasi WRF bergeser sekitar **175 km** terhadap pusat hujan pada data MSWEP. Hal ini menunjukkan bahwa model berhasil membentuk sistem hujan, namun lokasi hujan utama masih mengalami pergeseran (displacement error).

---

# 📍 Evaluasi Domain d03

Domain d03 digunakan sebagai domain utama untuk validasi statistik.

Metrik yang digunakan:

- RMSE
- MAE
- Bias
- Pearson Correlation
- R² Score

### Hasil

| Metrik | Nilai |
|---------|-------:|
| RMSE | **5.93 mm/jam** |
| MAE | **4.53 mm/jam** |
| Bias | **-0.85 mm/jam** |
| Pearson Correlation | **0.034** |
| R² Score | **-0.11** |

### Interpretasi

- RMSE sebesar **5.93 mm/jam** menunjukkan kesalahan prediksi masih berada pada kisaran yang cukup baik untuk simulasi kejadian hujan ekstrem.
- MAE sebesar **4.53 mm/jam** menunjukkan rata-rata selisih absolut antara hasil simulasi dan observasi.
- Bias bernilai **-0.85 mm/jam**, menandakan bahwa WRF sedikit meng-underestimate curah hujan dibandingkan MSWEP.
- Korelasi Pearson yang rendah (**0.034**) menunjukkan hubungan spasial maupun temporal antara simulasi dan observasi masih lemah.
- Nilai R² yang negatif menunjukkan variasi curah hujan hasil simulasi belum mampu menjelaskan variasi curah hujan observasi dengan baik.

---

# 🗺️ Difference Map

Difference Map menunjukkan selisih curah hujan antara WRF dan MSWEP.

```text
WRF

-

MSWEP
```

Interpretasi:

- Positif → Overestimate
- Negatif → Underestimate

📷

---

# 📉 Scatter Plot

Scatter Plot digunakan untuk melihat hubungan antara:

```text
Curah Hujan WRF

vs

Curah Hujan MSWEP
```

📷

---

# 📊 Ringkasan Hasil

| Evaluasi | Hasil |
|-----------|-------:|
| SSIM | **0.336** |
| Rainfall Centroid | **175.31 km** |
| RMSE | **5.93 mm/jam** |
| MAE | **4.53 mm/jam** |
| Bias | **-0.85 mm/jam** |
| Pearson (r) | **0.034** |
| R² | **-0.11** |

---

# 🔍 Pembahasan

Berdasarkan hasil simulasi dan validasi terhadap data MSWEP, diperoleh beberapa temuan penting, yaitu:

- Model WRF berhasil mensimulasikan kejadian hujan ekstrem akibat Siklon Tropis Cempaka selama periode simulasi.
- Intensitas curah hujan yang dihasilkan model relatif mendekati observasi, ditunjukkan oleh nilai RMSE yang cukup rendah dan Bias yang kecil.
- Pola distribusi hujan masih memiliki perbedaan spasial terhadap observasi, yang ditunjukkan oleh nilai SSIM sebesar **0.336**.
- Analisis Rainfall Centroid menunjukkan adanya pergeseran pusat hujan sekitar **175 km**, sehingga lokasi hujan maksimum belum sepenuhnya sesuai dengan observasi.
- Pergeseran spasial tersebut menjadi salah satu penyebab rendahnya nilai korelasi Pearson.
- Secara umum, WRF telah mampu merepresentasikan karakteristik utama kejadian hujan ekstrem, meskipun masih memerlukan peningkatan dalam mereproduksi lokasi dan distribusi hujan secara lebih akurat.

---

# 🏁 Kesimpulan

Model **WRF-ARW** berhasil mensimulasikan kejadian hujan ekstrem yang dipicu oleh **Siklon Tropis Cempaka** pada periode **26–29 November 2017**.

Hasil evaluasi menunjukkan bahwa besaran curah hujan hasil simulasi relatif mendekati data observasi, ditunjukkan oleh nilai **RMSE sebesar 5.93 mm/jam** dan **Bias sebesar -0.85 mm/jam**. Namun demikian, nilai **Pearson Correlation sebesar 0.034** menunjukkan bahwa hubungan spasial maupun temporal antara simulasi dan observasi masih rendah.

Evaluasi spasial melalui **SSIM sebesar 0.336** dan **Rainfall Centroid sejauh 175.31 km** mengindikasikan bahwa kelemahan utama model berada pada **displacement error**, yaitu pergeseran lokasi sistem hujan terhadap observasi.

Secara keseluruhan, konfigurasi WRF yang digunakan telah mampu merepresentasikan karakteristik umum hujan ekstrem selama Siklon Tropis Cempaka. Meskipun demikian, peningkatan konfigurasi model, parameterisasi fisik, maupun metode validasi masih diperlukan agar hasil simulasi semakin mendekati kondisi observasi.

---

# 🚀 Pengembangan Selanjutnya

Beberapa pengembangan yang dapat dilakukan pada penelitian berikutnya:

- Mencoba kombinasi parameterisasi fisik WRF yang berbeda.
- Menguji konfigurasi domain lain.
- Membandingkan hasil simulasi dengan data observasi BMKG.
- Menambahkan evaluasi menggunakan POD, FAR, CSI, dan Fractions Skill Score (FSS).
- Menggunakan metode validasi berbasis objek (Object-Based Verification) untuk menganalisis displacement error.

---

# 👨‍💻 Author

**Powy Alcuino**

Telkom University

Atmospheric Modeling • WRF • ERA5 • MSWEP • Tropical Cyclone Cempaka

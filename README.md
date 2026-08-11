# 🚗 The Luna --- Pesta Data Nasional (PEDAS) 2025

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?logo=pandas)
![NumPy](https://img.shields.io/badge/NumPy-Numerical%20Computing-013243?logo=numpy)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-11557c)
![Award](https://img.shields.io/badge/Award-Bronze%20Award-CD7F32)

Repository ini merupakan dokumentasi proyek kelompok **The Luna** dalam
mengikuti **Babak Penyisihan Kompetisi Nasional APTIKOM -- Pesta Data
Nasional (PEDAS) 2025**.

Proyek ini berfokus pada **analisis dan prediksi penjualan mobil**
menggunakan metode **Fuzzy Time Series**. Data yang digunakan merupakan
data penjualan bulanan dari lima merek mobil, yaitu Daihatsu, Honda,
Mitsubishi, Suzuki, dan Toyota.

------------------------------------------------------------------------

## 🏆 Pencapaian

Kelompok The Luna meraih **Bronze Award** pada **Babak Penyisihan Kompetisi Nasional APTIKOM -- Pesta
Data Nasional (PEDAS) 2025**

------------------------------------------------------------------------

## 👥 Kelompok The Luna

**Universitas Lampung**

  Nama                     NPM          Peran
  ------------------------ ------------ ----------------
  **Dian Fatonah**         2215061115   Ketua Kelompok
  **Dini Canika Sumara**   2215061026   Anggota
  **Safrina Fitriani**     2255061015   Anggota

------------------------------------------------------------------------

## 📌 Tentang Proyek

Proyek ini melakukan pengolahan data penjualan mobil untuk menghasilkan
prediksi penjualan pada **bulan Agustus 2025**.

Lima merek yang dianalisis adalah:

-   🚘 Daihatsu
-   🚘 Honda
-   🚘 Mitsubishi
-   🚘 Suzuki
-   🚘 Toyota

Tahapan utama yang dilakukan dalam notebook meliputi:

1.  Import library
2.  Data gathering
3.  Data assessing
4.  Data cleaning
5.  Exploratory Data Analysis (EDA)
6.  Forecasting menggunakan Fuzzy Time Series
7.  Evaluasi hasil forecasting
8.  Prediksi penjualan bulan Agustus 2025

------------------------------------------------------------------------

## 📊 Dataset

Dataset yang digunakan:

``` text
Dataset/dataCarSale2021-2025.csv
```

Dataset memiliki **54 observasi** dan **6 kolom**:

  Kolom          Keterangan
  -------------- ---------------------------
  `waktu`        Periode waktu penjualan
  `DAIHATSU`     Data penjualan Daihatsu
  `HONDA`        Data penjualan Honda
  `MITSUBISHI`   Data penjualan Mitsubishi
  `SUZUKI`       Data penjualan Suzuki
  `TOYOTA`       Data penjualan Toyota

Contoh data:

  Waktu          Daihatsu    Honda   Mitsubishi   Suzuki   Toyota
  ------------ ---------- -------- ------------ -------- --------
  2021-01-01        8.993    7.231        9.108    6.400   15.474
  2021-02-01        9.412    6.812        7.567    4.600   14.645
  2021-03-01       16.770   11.350       13.088    8.669   26.034

------------------------------------------------------------------------

## 🔎 Data Assessment

Pemeriksaan awal dilakukan untuk mengetahui struktur dan kualitas
dataset.

Hasil assessment:

-   Jumlah observasi: **54**
-   Jumlah kolom: **6**
-   Data duplikat: **0**
-   Seluruh 54 observasi memiliki nilai pada setiap kolom berdasarkan
    `df.info()`.

Tipe data sebelum cleaning:

``` text
waktu         object
DAIHATSU      float64
HONDA         float64
MITSUBISHI    float64
SUZUKI        float64
TOYOTA        float64
```

------------------------------------------------------------------------

## 🧹 Data Cleaning

Tahapan cleaning yang dilakukan:

### 1. Konversi kolom waktu

Kolom `waktu` diubah menjadi tipe `datetime`.

``` python
df['waktu'] = pd.to_datetime(
    df['waktu'],
    format='%Y-%m-%d'
)
```

### 2. Konversi kolom penjualan

Seluruh kolom penjualan diubah dari `float` menjadi `integer`.

``` python
for col in df.columns:
    if col != 'waktu':
        df[col] = df[col].astype(int)
```

Setelah cleaning:

``` text
waktu         datetime64[ns]
DAIHATSU      int64
HONDA         int64
MITSUBISHI    int64
SUZUKI        int64
TOYOTA        int64
```

------------------------------------------------------------------------

# 🔮 Forecasting dengan Fuzzy Time Series

Prediksi dilakukan menggunakan metode **Fuzzy Time Series (FTS)**.

Implementasi pada notebook menggunakan tahapan:

``` text
Data Historis
      ↓
Universe of Discourse
      ↓
Partition
      ↓
Fuzzy Sets
      ↓
Fuzzification
      ↓
Fuzzy Logical Relationships (FLR)
      ↓
Fuzzy Logical Relationship Groups (FLRG)
      ↓
Forecasting
      ↓
Evaluasi
      ↓
Prediksi Agustus 2025
```

### Parameter yang digunakan

-   **Buffer Universe of Discourse:** 10% dari rentang data
-   **Jumlah interval:** 1.400 interval
-   **Fuzzy set:** menggunakan nilai tengah (*midpoint*) setiap interval
-   **Seed:** `404`

### Universe of Discourse

Universe of Discourse ditentukan berdasarkan nilai minimum dan maksimum
setiap merek, kemudian ditambahkan buffer sebesar 10% dari rentang data.

### Partition

Universe of Discourse dibagi menjadi **1.400 interval** dengan panjang
interval yang sama.

### Fuzzification

Setiap nilai penjualan dipetakan ke fuzzy state berdasarkan interval
tempat nilai tersebut berada.

### FLR dan FLRG

Hubungan antara fuzzy state pada periode saat ini dan periode berikutnya
digunakan untuk membentuk:

-   **Fuzzy Logical Relationships (FLR)**
-   **Fuzzy Logical Relationship Groups (FLRG)**

### Forecasting

Nilai prediksi diperoleh berdasarkan fuzzy state dan nilai midpoint dari
state yang berhubungan.

------------------------------------------------------------------------

# 📈 Evaluasi Forecasting

Evaluasi dilakukan menggunakan:

-   **MAE** --- Mean Absolute Error
-   **RMSE** --- Root Mean Square Error
-   **MAPE** --- Mean Absolute Percentage Error
-   **SMAPE** --- Symmetric Mean Absolute Percentage Error
-   **ME** --- Mean Error

Hasil evaluasi pada notebook:

  Brand                MAE        RMSE        MAPE       SMAPE       ME
  ------------ ----------- ----------- ----------- ----------- --------
  Daihatsu           0.30%       1.21%       0.47%       0.24%   -0.00%
  Honda          **0.04%**   **0.05%**   **0.06%**   **0.03%**   -0.00%
  Mitsubishi         0.41%       1.45%       0.57%       0.29%    0.01%
  Suzuki             1.16%       5.66%       1.22%       0.58%    0.00%
  Toyota             0.86%       4.27%       0.80%       0.39%    0.00%

### Hasil evaluasi

Berdasarkan metrik yang dihitung pada notebook, **Honda memiliki nilai
error paling rendah** dibandingkan merek lainnya:

-   MAE: **0.04%**
-   RMSE: **0.05%**
-   MAPE: **0.06%**
-   SMAPE: **0.03%**

------------------------------------------------------------------------

# 🔮 Hasil Prediksi Penjualan Agustus 2025

Hasil forecasting menggunakan Fuzzy Time Series:

    Peringkat Brand          Prediksi Penjualan
  ----------- ------------ --------------------
           🥇 Toyota                  **7.613**
           🥈 Daihatsu                **6.349**
           🥉 Mitsubishi              **6.117**
            4 Honda                   **4.168**
            5 Suzuki                  **2.669**

### Ringkasan

-   **Toyota** memperoleh prediksi tertinggi sebesar **7.613 unit**.
-   **Daihatsu** memperoleh prediksi sebesar **6.349 unit**.
-   **Mitsubishi** memperoleh prediksi sebesar **6.117 unit**.
-   **Honda** memperoleh prediksi sebesar **4.168 unit**.
-   **Suzuki** memperoleh prediksi terendah sebesar **2.669 unit**.

------------------------------------------------------------------------

## 💡 Key Findings

1.  Dataset terdiri dari **54 observasi** penjualan dari lima merek
    mobil.
2.  Tidak ditemukan **baris duplikat** pada dataset.
3.  Data waktu dan data penjualan diproses melalui tahap cleaning
    sebelum forecasting.
4.  Forecasting dilakukan menggunakan **Fuzzy Time Series** dengan
    **1.400 interval**.
5.  Pada evaluasi in-sample yang dihitung pada notebook, **Honda
    memiliki error paling rendah**.
6.  Hasil forecasting untuk Agustus 2025 menunjukkan **Toyota memiliki
    prediksi penjualan tertinggi**, yaitu **7.613 unit**.
7.  **Suzuki memiliki prediksi penjualan terendah**, yaitu **2.669
    unit**.

------------------------------------------------------------------------

## 🛠️ Teknologi yang Digunakan

-   Python
-   Google Colab
-   Pandas
-   NumPy
-   Matplotlib
-   Scikit-learn

Library utama:

``` python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

from sklearn.metrics import mean_absolute_error, mean_squared_error
from google.colab import files
```

------------------------------------------------------------------------

## 📁 Struktur Repository

``` text
TheLuna_PeDaS2025/
│
├── Dataset/
│   └── dataCarSale2021-2025.csv
│
├── The Luna_Universitas Lampung_PeDaS 2025.ipynb
│
├── theluna.csv
│
└── README.md
```

------------------------------------------------------------------------

## 🚀 Cara Menjalankan

### 1. Clone Repository

``` bash
git clone https://github.com/Dianftnh/TheLuna_PeDaS2025.git
```

### 2. Masuk ke Repository

``` bash
cd TheLuna_PeDaS2025
```

### 3. Buka Notebook

Buka:

``` text
The Luna_Universitas Lampung_PeDaS 2025.ipynb
```

Notebook dapat dijalankan menggunakan **Google Colab** atau **Jupyter
Notebook**.

### 4. Jalankan Cell Secara Berurutan

Urutan proses:

``` text
Import Packages
      ↓
Gathering Data
      ↓
Assessing Data
      ↓
Cleaning Data & EDA
      ↓
Fuzzy Time Series
      ↓
Evaluation
      ↓
Forecast Agustus 2025
```

------------------------------------------------------------------------

## 📄 Output

Hasil prediksi dirangkum dalam:

``` text
theluna.csv
```

dengan hasil:

  Brand          Sales
  ------------ -------
  Daihatsu        6349
  Honda           4168
  Mitsubishi      6117
  Suzuki          2669
  Toyota          7613

------------------------------------------------------------------------

## 🏅 Sertifikat & Penghargaan

**Dian Fatonah** memperoleh:

> **Bronze Award**\
> pada Babak Penyisihan Kompetisi Nasional APTIKOM -- Pesta Data
> Nasional (PEDAS)

Pelaksanaan: **31 Agustus s.d. 6 September 2025**\
Tanggal sertifikat: **19 September 2025**

------------------------------------------------------------------------

## 👩‍💻 Kelompok

### The Luna --- Universitas Lampung

-   **Dian Fatonah** --- Ketua Kelompok
-   **Dini Canika Sumara** --- Anggota
-   **Safrina Fitriani** --- Anggota

------------------------------------------------------------------------

## 📌 Catatan

README ini mendokumentasikan proses yang terdapat pada notebook proyek,
mulai dari pengumpulan data, assessment, cleaning, forecasting
menggunakan Fuzzy Time Series, evaluasi, hingga hasil prediksi penjualan
Agustus 2025.

------------------------------------------------------------------------

⭐ **The Luna \| Universitas Lampung \| PEDAS 2025**

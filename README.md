# 📉 Indonesian Inflation Econometric Analysis (1992–2025)
*Multiple Linear Regression, Time-Series Diagnostics & Policy Implications*

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python)
![Minitab](https://img.shields.io/badge/Tools-Minitab%2022-green)
![License](https://img.shields.io/badge/License-MIT-yellow)

## 📌 Ringkasan Proyek
Proyek ini menganalisis faktor-faktor makroekonomi utama yang mempengaruhi tingkat inflasi di Indonesia selama periode **1992–2025** (n=34). Menggunakan kerangka **Regresi Linier Berganda (OLS)**, penelitian ini menguji hubungan antara Inflasi IHK (Y) dengan Pertumbuhan PDB Riil (X_1), Nilai Tukar Rupiah/USD (X_2), dan Harga Minyak Mentah Brent (X_3).

Analisis dilakukan dengan pendekatan ganda menggunakan **Minitab Statistical Software** dan **Python (Google Colab)**, yang mencakup estimasi parameter berbasis aljabar matriks, pengujian asumsi klasik deret waktu, hingga evaluasi kebijakan moneter.

---

## 📊 Hasil Utama & Temuan Model

### 1. Persamaan Regresi Estimasi (OLS)
Y = 29,1645 - 2,5245 X_1 - 0,001143 X_2 + 0,0354 X_3

| Variabel Prediktor | Koefisien (Beta) | Std Error | t-statistic | p-value | Signifikansi (alpha=0.05) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Konstanta (Beta 0)** | 29.1645 | 2.568 | 11.358 | 0.000 | Signifikan |
| **PDB Riil % (X_1)** | -2.5245 | 0.258 | -9.774 | 0.000 | Signifikan |
| **Kurs Rp/USD (X_2)** | -0.001143 | 0.00025 | -4.471 | 0.000 | Signifikan |
| **Harga Minyak (X_3)** | 0.0354 | 0.034 | 1.032 | 0.310 | Tidak Signifikan |

### 2. Kebaikan Model & Diagnostik Asumsi
* **Adjusted R^2:** 77,37% R^2 = 79,42%, menunjukkan kapasitas penjelas yang tinggi terhadap variasi inflasi.
* **Multikolinearitas (VIF):** VIF(X_1)=1.36, VIF(X_2)=1.87, VIF(X_3)=1.82. Nilai VIF < 10 mengonfirmasi **bebas multikolinearitas**.
* **Autokorelasi Residual (Durbin-Watson):** DW = 1.88237. Berada dalam rentang bebas autokorelasi (1.65 < DW < 2.35).

---

## 💡 Implikasi Ekonomi & Kebijakan
1. **Kapasitas Produksi Domestik (X_1):** Koefisien negatif signifikan (Beta_1 = -2.5245) menunjukkan bahwa peningkatan pertumbuhan PDB riil di Indonesia didorong oleh ekspansi penawaran agregat yang meredam tekanan inflasi (*supply-driven growth*).
2. **Efektivitas Inflation Targeting Framework (ITF) (X_2):** Koefisien negatif pada kurs nominal mencerminkan keberhasilan Bank Indonesia pasca-krisis 1998 dalam menstabilkan ekspektasi inflasi domestik melalui rezim kerangka ITF.
3. **Peran Buffer APBN (X_3):** Harga minyak dunia tidak berpengaruh signifikan secara langsung karena kebijakan subsidi energi pemerintah yang menyerap gejolak harga internasional.

---

## 🛠️ Metodologi & Alur Pengolahan Data

### Workflow Minitab
1. Import dataset dari `Data Tugas Proyek MSM 2.xlsx`.
2. Menu: `Stat` > `Regression` > `Regression` > `Fit Regression Model...`.
3. Mengaktifkan statistik **Durbin-Watson** pada opsi diagnostik dan memplot grafik **Four in one** residual.
4. Berkas lengkap tersimpan di `minitab/TUGAS PROYEK MSM 2 FIX.mpx`.

### Workflow Python (Google Colab)
```python
import pandas as pd
import statsmodels.api as sm
from statsmodels.stats.outliers_influence import variance_inflation_factor
from statsmodels.stats.stattools import durbin_watson

# Load data
df = pd.read_excel("data/Data_Inflasi_Indonesia_1992-2025.xlsx", sheet_name="Data Siap Pakai")
X = sm.add_constant(df[['Pertumbuhan PDB Riil % (X1)', 'Kurs Rp/USD (X2)', 'Harga Minyak USD/ Barel (X3)']])
y = df['Inflasi % (Y)']

# OLS Fit & Diagnostics
model = sm.OLS(y, X).fit()
print(model.summary())
print("DW Statistic:", durbin_watson(model.resid))

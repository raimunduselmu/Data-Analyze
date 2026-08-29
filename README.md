# 📈 Analisis Portofolio Saham & Efficient Frontier dengan Simulasi Monte Carlo

Repositori ini berisi proyek analisis kuantitatif dan optimasi portofolio multi-aset menggunakan pendekatan **Simulasi Monte Carlo**. Proyek ini memodelkan ribuan kombinasi bobot portofolio acak untuk memetakan kurva *Efficient Frontier*, mencari alokasi bobot optimal berdasarkan rasio *Risk-to-Return (Sharpe Ratio)*, serta meminimalkan volatilitas risiko investasi.

---

## 📌 Ringkasan Proyek & Tujuan
Analisis ini menggunakan data historis pergerakan 5 aset saham: **ESTC, GAP, SOLS, TRLV, dan WDAY**.

Tujuan utama analisis:
1. **Data Ingestion & Cleaning:** Mengambil data harga saham historis (*OHLCV / Adjusted Close*) melalui API `yfinance`.
2. **Statistik Deskriptif & Distribusi Return:** Menghitung *Daily Returns*, *Monthly Returns*, normalisasi harga dasar, serta visualisasi sebaran data dengan Histogram dan Box Plot.
3. **Analisis Metrik Portofolio:** Menghitung *Annualized Return*, *Annualized Volatility*, dan *Sharpe Ratio* (dengan asumsi *Risk-Free Rate* $R_f = 1\%$) pada bobot setara (*Equal-Weighted Portfolio*).
4. **Simulasi Monte Carlo (10.000 Iterasi):** Menghasilkan 10.000 kombinasi bobot acak untuk mencari portofolio dengan **Sharpe Ratio Maksimum (Optimal Portfolio)** dan **Volatilitas Minimum (Minimum Variance Portfolio)**.
5. **Pemetaan Efficient Frontier:** Memvisualisasikan batas efisiensi investasi menggunakan *Scatter Plot*.

---

## 📐 Landasan Matematis & Metrik

1. **Daily Returns & Cumulative Returns:**
   $$R_t = \frac{P_t - P_{t-1}}{P_{t-1}}$$
   $$\text{Cumulative Return} = \prod (1 + R_t) - 1$$

2. **Annualized Return & Volatility ($252\text{ hari bursa}$):**
   $$E[R_p] = \sum (w_i \cdot \bar{R}_i) \times 252$$
   $$\sigma_p = \sqrt{\mathbf{w}^T (\mathbf{\Sigma} \times 252) \mathbf{w}}$$
   *Keterangan: $\mathbf{w}$ adalah vektor bobot portofolio ($\sum w_i = 1$) dan $\mathbf{\Sigma}$ adalah matriks kovarians return harian[cite: 1].*

3. **Sharpe Ratio ($R_f = 0.01$):**
   $$\text{Sharpe Ratio} = \frac{E[R_p] - R_f}{\sigma_p}$$

---

## 🛠️ Tech Stack & Library
- **Bahasa:** Python 3.11+
- **Data & Komputasi:** `pandas`, `numpy`
- **Market Data:** `yfinance`
- **Visualisasi Statis:** `matplotlib`, `seaborn`
- **Visualisasi Interaktif:** `plotly`

---

## 📂 Struktur Repositori
```text
├── assets/
│  ├──
├── Visualisasi Saham.ipynb          # Notebook analisis lengkap
├── requirements.txt                 # Daftar dependensi library
└── README.md                        # Dokumentasi proyek



## 📊 Hasil Analisis & Visualisasi

### 1. Pergerakan Harga Historis Saham
Analisis dilakukan pada 5 ticker saham (**ESTC, GAP, SOLS, TRLV, WDAY**) dalam rentang waktu **6 April 2026 – 28 Agustus 2026** (101 hari bursa).

| Ticker | Harga Terendah (Min) | Harga Rata-Rata (Mean) | Harga Tertinggi (Max) | Volatilitas Harga (Std Dev) |
| :--- | :---: | :---: | :---: | :---: |
| **ESTC** | $43.30 | $60.19 | $87.34 | 11.33 |
| **GAP** | $18.35 | $21.65 | $27.02 | 2.21 |
| **SOLS** | $55.29 | $73.81 | $88.48 | 11.37 |
| **TRLV** | $6.02 | $8.91 | $13.00 | 1.35 |
| **WDAY** | $112.50 | $141.66 | $206.45 | 25.27 |

![Historical Adjusted Close Prices](assets/historical_prices.png)
> *Gambar 1: Pergerakan harga penutupan (Adjusted Close) 5 saham selama periode observasi.*

---

### 2. Distribusi & Karakteristik Return Harian
Untuk melihat sebaran dan potensi *outlier* pada imbal hasil harian, dilakukan analisis distribusi menggunakan Histogram dan Box Plot:

![Daily Returns Boxplot](assets/returns_distribution.png)
> *Gambar 2: Box Plot sebaran imbal hasil harian (Daily Returns) masing-masing aset.*[cite: 1]

- **Karakteristik Sebaran:** Seluruh aset memiliki rata-rata return harian mendekati nol dengan tingkat dispersi (ekor volatilitas) yang bervariasi[cite: 1].
- **Performa Bulanan Positif:** Saham **ESTC** dan **WDAY** mencatatkan momentum kenaikan tertinggi pada periode Mei 2026 (masing-masing +39.35% dan +19.44%), sementara saham **TRLV** mengalami reli kuat pada Agustus 2026 (+27.51%)[cite: 1].

---

### 3. Kinerja Aset Individual vs Portofolio Bobot Setara (Equal-Weighted)
Dengan asumsi $252\text{ hari perdagangan/tahun}$ dan *Risk-Free Rate* ($R_f$) sebesar $1.0\%$ ($0.01$)[cite: 1]:

| Aset / Portofolio | Return Kumulatif | Return Tahunan (Annual Return) | Volatilitas Tahunan ($\sigma$) | Sharpe Ratio |
| :--- | :---: | :---: | :---: | :---: |
| **ESTC** | +71.68% | +134.20% | 66.84% | 1.9928 |
| **GAP** | -12.43% | -28.91% | 47.98% | -0.6234 |
| **SOLS** | +8.78% | +24.62% | 46.57% | 0.5072 |
| **TRLV** | +84.62% | +169.52% | 79.13% | 2.1296 |
| **WDAY** | +47.53% | +108.57% | 52.41% | 2.0525 |
| **Portofolio Equal Weight (20% Tiap Saham)** | **+38.16%** | **+81.60%** | **42.18%** | **1.9109** |

> 💡 **Insight:** Diversifikasi pada portofolio bobot setara berhasil memangkas risiko volatilitas menjadi **42.18%** (lebih rendah daripada volatilitas aset tunggal manapun), dengan tetap mempertahankan imbal hasil tahunan yang solid sebesar **81.60%**[cite: 1].

---

### 4. Hasil Simulasi Monte Carlo & Efficient Frontier
Simulasi dijalankan sebanyak **10.000 iterasi portofolio acak** untuk mengidentifikasi kombinasi bobot optimal yang memaksimalkan Sharpe Ratio (*Tangency Portfolio*) dan meminimalkan varians risiko (*Minimum Volatility Portfolio*)[cite: 1].

![Efficient Frontier Monte Carlo](assets/efficient_frontier.png)
> *Gambar 3: Kurva Efficient Frontier hasil 10.000 simulasi Monte Carlo dengan penanda Portofolio Optimal (Bintang Merah) dan Portofolio Risiko Minimum (Bintang Hijau).*[cite: 1]

#### 🏆 Perbandingan Portofolio Hasil Optimasi:

| Parameter | Portofolio Optimal (Max Sharpe Ratio) | Portofolio Risiko Minimum (Min Volatility) |
| :--- | :---: | :---: |
| **Annualized Return** | **128.84%** | **31.25%** |
| **Annualized Volatility** | **53.12%** | **37.40%** |
| **Sharpe Ratio** | **2.4066** | **0.8088** |
| **Alokasi Bobot ESTC** | 23.15% | 8.42% |
| **Alokasi Bobot GAP** | 0.21% *(Diabaikan/Minimal)* | 28.14% |
| **Alokasi Bobot SOLS** | 1.84% | 41.50% |
| **Alokasi Bobot TRLV** | 39.42% | 3.65% |
| **Alokasi Bobot WDAY** | 35.38% | 18.29% |

---

### 📌 Kesimpulan & Temuan Utama
1. **Peningkatan Efisiensi Risk-Adjusted Return:** Portofolio optimal hasil simulasi Monte Carlo menghasilkan **Sharpe Ratio sebesar 2.4066**, meningkat signifikan dibandingkan portofolio bobot setara (1.9109) maupun rata-rata aset individual[cite: 1].
2. **Peran Alokasi Bobot:** Model optimasi mengalokasikan bobot terbesar pada **TRLV (~39.4%)**, **WDAY (~35.4%)**, dan **ESTC (~23.2%)** karena kontribusi return yang tinggi terhadap unit risiko yang diambil[cite: 1].
3. **Efek Diversifikasi Risiko:** Portofolio Risiko Minimum mampu menekan volatilitas hingga **37.40%** dengan memperbesar alokasi pada aset berfluktuasi rendah seperti **SOLS (41.5%)** dan **GAP (28.1%)**[cite: 1].
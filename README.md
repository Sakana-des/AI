# 📊 Analisis Data Penjualan E-Commerce — Nusantara Mart

Notebook Jupyter ini berisi analisis eksplorasi data (EDA) terhadap dataset penjualan e-commerce **Nusantara Mart** menggunakan Python.

---

## 📁 Dataset

| File | Deskripsi |
|------|-----------|
| `E-Commerce Sales Analytics.csv` | Dataset utama yang digunakan dalam analisis |

### Kolom Dataset

| Kolom | Tipe Data | Deskripsi |
|-------|-----------|-----------|
| `order_id` | int64 | ID unik setiap transaksi |
| `order_date` | str / datetime | Tanggal transaksi |
| `customer_id` | int64 | ID pelanggan |
| `product_category` | str | Kategori produk (Beauty, Clothing, Electronics, dll.) |
| `region` | str | Wilayah penjualan (North, South, East, West) |
| `quantity` | int64 | Jumlah barang yang dibeli |
| `unit_price` | float64 | Harga satuan produk |
| `discount` | float64 | Besaran diskon (0.00 – 0.35) |
| `payment_method` | str | Metode pembayaran (Wallet, Card, COD) |
| `delivery_days` | int64 | Lama pengiriman (hari) |
| `customer_rating` | float64 | Rating pelanggan (1.0 – 5.0) |
| `revenue` | float64 | Total pendapatan dari transaksi |

> **Jumlah data:** 5.000 baris, tanpa nilai kosong (*non-null*).

---

## 🔧 Library yang Digunakan

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
from datetime import datetime
```

| Library | Versi | Fungsi |
|---------|-------|--------|
| `pandas` | – | Manipulasi dan analisis data tabular |
| `matplotlib` | – | Pembuatan visualisasi dasar |
| `seaborn` | – | Visualisasi statistik yang lebih estetis |
| `numpy` | – | Komputasi numerik |
| `datetime` | bawaan | Pemrosesan data tanggal |

---

## 📒 Struktur Notebook (`main.ipynb`)

### Cell 1 — Import Library
Mengimpor semua library yang dibutuhkan untuk analisis.

---

### Cell 2 — Load Dataset
```python
df = pd.read_csv('E-Commerce Sales Analytics.csv')
```
Membaca dataset dari file CSV ke dalam DataFrame Pandas.

---

### Cell 3 — Eksplorasi Awal (`df.head()`)
Menampilkan 5 baris pertama dataset untuk melihat struktur data secara sekilas.

---

### Cell 4 — Statistik Deskriptif (`df.describe()`)
Menampilkan ringkasan statistik numerik meliputi:
- **Count, Mean, Std, Min, Max**
- **Kuartil (25%, 50%, 75%)**

Contoh hasil:
| Statistik | revenue |
|-----------|---------|
| count | 5000 |
| mean | 1021.96 |
| std | 825.58 |
| min | 11.21 |
| 25% | 354.53 |
| 50% | 796.65 |
| 75% | 1515.69 |
| max | 4119.33 |

---

### Cell 5 — Informasi Tipe Data (`df.info()`)
Menampilkan tipe data setiap kolom dan memastikan tidak ada nilai null.

---

### Cell 6 — Statistik Revenue
```python
print(df['revenue'].mean())    # rata-rata
print(df['revenue'].median())  # median
print(df['revenue'].std())     # standar deviasi
print(df['revenue'].quantile(0.25))  # Q1
print(df['revenue'].quantile(0.75))  # Q3
```
Menghitung dan menampilkan ukuran pemusatan dan penyebaran data revenue secara manual.

---

### Cell 7 — Total Penjualan per Kategori Produk
Membuat **bar chart horizontal** untuk membandingkan total revenue antar kategori produk.

```python
total_kategori = df.groupby('product_category')['revenue'].sum().sort_values(ascending=False)
sns.barplot(x=total_kategori.values, y=total_kategori.index, palette='Blues_r')
```

📌 **Insight:** Kategori produk mana yang memberikan kontribusi revenue tertinggi.

---

### Cell 8 — Tren Penjualan Bulanan
Mengkonversi kolom `order_date` ke format datetime, lalu memvisualisasikan **tren revenue bulanan** menggunakan line chart.

```python
df['order_date'] = pd.to_datetime(df['order_date'])
df['bulan'] = df['order_date'].dt.strftime('%B')
tren_bulanan = df.groupby('bulan')['revenue'].sum()
plt.plot(tren_bulanan.index, tren_bulanan.values, marker='o', color='crimson')
```

📌 **Insight:** Pola musiman atau tren pertumbuhan penjualan sepanjang tahun.

---

### Cell 9 — Hubungan Jumlah Pembelian dengan Revenue
**Scatter plot** untuk melihat korelasi antara jumlah barang yang dibeli (`quantity`) dan total nilai transaksi (`revenue`), dipisahkan berdasarkan kategori produk.

```python
sns.scatterplot(data=df, x='quantity', y='revenue', hue='product_category')
```

📌 **Insight:** Apakah membeli lebih banyak barang berbanding lurus dengan revenue yang lebih tinggi?

---

### Cell 10 — Distribusi Rating & Sebaran Pembelian
Dua visualisasi sekaligus:

1. **Histogram Rating Pelanggan** — distribusi frekuensi rating dari 1.0 hingga 5.0.
   ```python
   sns.histplot(df['customer_rating'], bins=5, color='seagreen')
   ```

2. **Boxplot Jumlah Pembelian per Kategori** — mendeteksi outlier pada jumlah barang yang dibeli di setiap kategori.
   ```python
   sns.boxplot(data=df, x='product_category', y='quantity')
   ```

---

### Cell 11 — Laporan Ringkasan Analisis
Menghasilkan **laporan teks otomatis** yang merangkum hasil analisis, termasuk:

- Total penjualan keseluruhan
- Rata-rata nilai transaksi
- Kategori produk terlaris (berdasarkan kuantitas)
- Wilayah dengan revenue tertinggi
- Rata-rata rating pelanggan

---

## 🚀 Cara Menjalankan

### Prasyarat
Pastikan Python dan library berikut sudah terinstal:
```bash
pip install pandas matplotlib seaborn numpy jupyter
```

### Langkah
1. Clone atau unduh repositori ini.
2. Pastikan file `E-Commerce Sales Analytics.csv` berada di direktori yang sama dengan `main.ipynb`.
3. Jalankan Jupyter Notebook:
   ```bash
   jupyter notebook main.ipynb
   ```
4. Jalankan semua cell secara berurutan (**Kernel → Restart & Run All**).

---

## 📈 Ringkasan Insight Utama

| Metrik | Nilai |
|--------|-------|
| Total Transaksi | 5.000 |
| Rata-rata Revenue | Rp 1.021,96 |
| Median Revenue | Rp 796,65 |
| Rating Rata-rata | ~2.97 |
| Rentang Pengiriman | 1–11 hari |

---

## 🗂️ Struktur File Proyek

```
AI/
├── main.ipynb                      # Notebook analisis utama
├── E-Commerce Sales Analytics.csv  # Dataset penjualan
├── README.md                       # Dokumentasi ini
└── ...
```

---

## 📝 Catatan

- Seluruh komentar dan label visualisasi menggunakan **Bahasa Indonesia**.
- Analisis mencakup aspek statistik deskriptif, tren waktu, perbandingan kategori, dan distribusi data.
- Dataset bersih — tidak ada nilai kosong pada 5.000 baris data.

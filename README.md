# Analisis FnB di Pontianak Berdasarkan Kecamatan dan Foot Traffic (Data Google Maps)

## 📋 Deskripsi Proyek

Proyek ini menganalisis kompetitor bisnis Food & Beverage (FnB) di wilayah Pontianak menggunakan data dari Google Maps. Analisis ini berfokus pada distribusi bisnis FnB berdasarkan kecamatan, kategori, dan metrik performa seperti jumlah ulasan serta rating.

## 🎯 Tujuan Analisis

Proyek ini dirancang untuk menjawab pertanyaan-pertanyaan bisnis strategis berikut:

1. **Bagaimana distribusi bisnis FnB di berbagai kecamatan Pontianak?**
   - Mengidentifikasi area dengan konsentrasi bisnis FnB tertinggi
   - Menentukan kecamatan dengan potensi pasar terbesar

2. **Kategori FnB apa yang paling populer di Pontianak?**
   - Analisis jenis-jenis bisnis FnB yang dominan
   - Preferensi pasar berdasarkan kategori

3. **Bagaimana performa bisnis FnB dilihat dari rating dan ulasan?**
   - Menganalisis tingkat kepuasan pelanggan (rating)
   - Mengukur engagement pelanggan (jumlah ulasan/reviews)
   - Identifikasi bisnis dengan performa terbaik

4. **Apa saja peluang bisnis FnB baru di Pontianak?**
   - Identifikasi area dengan kompetisi rendah
   - Kategori dengan potensi pertumbuhan tinggi
   - Rekomendasi lokasi strategis

## 📊 Dataset

### Sumber Data
- **Platform**: Google Maps API
- **Total Records**: 1,250 bisnis FnB
- **Periode**: Data scapred pada tanggal 2026-09-16 (scraped dari Google Maps)
- **Coverage Area**: Kota Pontianak, Indonesia

### Fitur Utama Dataset
| Fitur | Deskripsi | Tipe Data |
|-------|-----------|-----------|
| `title` | Nama bisnis FnB | Text |
| `categoryName` | Kategori utama bisnis | Text |
| `categories` | Daftar kategori (multi-label) | List |
| `neighborhood` | Lingkungan/kelurahan | Text |
| `address` | Alamat lengkap | Text |
| `location` | Koordinat geografis (lat, lng) | GeoJSON |
| `reviewsCount` | Jumlah ulasan di Google Maps | Numeric |
| `totalScore` | Rating/skor rata-rata (1-5) | Numeric |
| `permanentlyClosed` | Status tutup permanen | Boolean |
| `temporarilyClosed` | Status tutup sementara | Boolean |

### Data Quality
- **Total Records Dianalisis**: 1,250
- **Records Aktif** (tidak tutup): 1,250
- **Completeness**:
  - `categoryName`: 99.68% (1,246/1,250)
  - `neighborhood`: 99.92% (1,249/1,250)
  - `address`: 99.92% (1,249/1,250)
  - `reviewsCount`: 85.76% (1,072/1,250)
  - `totalScore`: 85.76% (1,072/1,250)

## 📦 Dataset & Metodologi Singkat

- **Sumber data:** hasil scraping Google Maps (`allfieldjson.json`), 1.250 baris × 62 kolom mentah.
- **Data cleaning:**
  - Membuang bisnis yang `permanentlyClosed` / `temporarilyClosed`.
  - Mengisi `reviewsCount` & `totalScore` yang kosong dengan 0.
  - Menghapus 1 baris tanpa alamat; mengisi 4 baris tanpa kategori sebagai `Cafe`.
  - Menstandarkan 72 variasi penulisan `neighborhood` (typo, bahasa Inggris/Indonesia campur, dsb.) menjadi **6 kecamatan resmi Pontianak** (Kota, Selatan, Tenggara, Timur, Barat, Utara), termasuk koreksi manual untuk 10 baris yang salah klasifikasi geografis.
  - Membuang 25 kategori non-F&B murni (hotel, mall, SPBU, gym, kantor, dsb.).
- **Hasil akhir:** **1.206 bisnis F&B aktif** siap dianalisis.
- **Kategorisasi:** puluhan `categoryName` mentah disederhanakan menjadi 4 kategori utama — **Cafe, Restaurant & Dining, Street Food, Bakery & Dessert**.
- **Tools:** Python (pandas, matplotlib, seaborn, folium) di Google Colab.

### Data Processing:
1. Data loading dari JSON (Google Maps API output)
2. Data cleaning & missing value handling
3. Feature engineering (geografis, kategori extraction)
4. Outlier detection & handling

---

## 🔍 Hasil Analisis Utama

### 1. Peta persaingan per kecamatan

| Kecamatan | Jumlah Kompetitor | Avg Foot Traffic (ulasan/outlet) | Total Foot Traffic | Avg Rating |
|---|---:|---:|---:|---:|
| Pontianak Selatan | 308 | 367.13 | 113.077 | 4.13 |
| **Pontianak Tenggara** | **88** | **192.76** | 16.963 | **4.13** |
| Pontianak Kota | 351 | 140.02 | 49.146 | 3.98 |
| Pontianak Timur | 81 | 77.35 | 6.265 | 3.77 |
| Pontianak Barat | 184 | 64.02 | 11.780 | 3.30 |
| Pontianak Utara | 194 | 59.21 | 11.486 | 3.37 |

**Insight:** Pontianak Kota dan Pontianak Selatan adalah pusat keramaian dengan kompetitor terbanyak (351 & 308 outlet) — pasar cenderung jenuh. Pontianak Tenggara justru menawarkan kombinasi langka: kompetitor paling sedikit (88 outlet) namun foot traffic rata-rata tertinggi kedua (192,76 ulasan/outlet) dan rating konsumen sama tingginya dengan Pontianak Selatan (4,13). Ini adalah sinyal *demand tinggi, supply rendah*.

### 2. Dominasi kategori: fenomena "1001 warkop"

Dari 1.206 bisnis, **795 (~66%) adalah Cafe/Warkop** — jauh melampaui kategori lain (Restaurant & Dining 385, Street Food 13, Bakery & Dessert 13). Cafe mendominasi hampir di *setiap* kecamatan, mengonfirmasi Pontianak sebagai kota dengan kultur "ngopi" yang sangat jenuh secara jumlah pemain.

| Kategori | Jumlah Venue | Avg Foot Traffic | Avg Rating |
|---|---:|---:|---:|
| Bakery & Dessert | 13 | **429.15** | **4.33** |
| Restaurant & Dining | 385 | 285.02 | 4.12 |
| Street Food | 13 | 199.77 | 4.06 |
| Cafe | 795 | 114.23 | 3.65 |

**Insight kunci:** meskipun Cafe jumlahnya paling banyak, secara *kualitas* performa (rata-rata ulasan per outlet & rating) kategori ini justru **paling lemah**. Restaurant & Dining menarik foot traffic hampir 2,5× lipat dari Cafe (285 vs 114 ulasan/outlet) dengan rating lebih tinggi — indikasi Cafe adalah kategori paling jenuh dan kompetitif, sementara Restaurant & Dining relatif under-supplied dibanding daya tariknya.

### 3. Bakery & Dessert: permata tersembunyi, tapi hati-hati bias outlier

Bakery & Dessert punya foot traffic dan rating tertinggi dari semua kategori, tapi jumlah venue-nya sangat sedikit (hanya 13 di seluruh Pontianak). Saat ditelusuri per outlet, angka ini **didorong kuat oleh 1–2 destinasi legendaris**, terutama **Es Krim Angi** di Pontianak Selatan (4.520 ulasan, rating 4,6) dan **Es Teler Jeranding** di Pontianak Barat (482 ulasan). Di luar dua nama itu, mayoritas venue bakery/dessert lain hanya punya belasan hingga ratusan ulasan. Artinya angka rata-rata kategori ini agak *skewed* — bukan berarti setiap toko dessert baru otomatis akan seramai itu, tapi tetap mengonfirmasi kategori ini kurang tergarap dan punya daya tarik kuat saat eksekusinya tepat.

### 4. Komposisi kategori di Pontianak Tenggara (lokasi rekomendasi utama)

| Kategori | Jumlah Bisnis |
|---|---:|
| Cafe | 45 |
| Restaurant & Dining | 37 |
| Street Food | 5 |
| Bakery & Dessert | 1 |

Di kecamatan dengan peluang terbaik ini, Cafe masih mendominasi jumlah pemain, tapi **Restaurant & Dining baru punya 37 pemain** — jauh lebih sedikit dibanding Cafe, sementara secara nasional kategori ini terbukti menarik foot traffic jauh lebih tinggi. Bakery & Dessert bahkan nyaris kosong (hanya 1 outlet), membuka ruang untuk pemain baru di kategori tersebut.

---

## 📈 Visualisasi & Pola yang Ditemukan

Notebook menghasilkan 4 visualisasi utama:

1. **Dual-axis bar + line chart** — jumlah kompetitor (bar) vs rata-rata foot traffic (line) per kecamatan. Pola yang terlihat: dua kurva ini **tidak berbanding lurus** — Pontianak Kota punya kompetitor terbanyak tapi foot traffic per outlet rendah, sementara Pontianak Tenggara sebaliknya. Pola inilah yang mendasari rekomendasi lokasi.
2. **Grouped bar chart** — distribusi 4 kategori utama di tiap kecamatan. Pola: Cafe selalu jadi bar tertinggi di semua kecamatan tanpa kecuali, memvisualkan kejenuhan kategori Cafe secara nasional.
3. **Heatmap (kecamatan × kategori)** — rata-rata foot traffic tiap kombinasi kecamatan-kategori. Sel dengan warna paling "panas" justru muncul di kategori Restaurant & Dining/Bakery & Dessert di beberapa kecamatan non-pusat kota, bukan di Cafe — menunjukkan *hot spot* peluang tersembunyi di luar kategori yang paling ramai pemainnya.
4. **Peta interaktif (folium + marker cluster)** — sebaran geografis seluruh 1.206 bisnis berdasarkan koordinat lat/lng, dikelompokkan per kecamatan untuk validasi lokasi lapangan.

**Pola besar yang konsisten di semua visualisasi:** *jumlah kompetitor tinggi tidak selalu berarti performa/foot traffic tinggi*, dan kategori dengan jumlah pemain paling banyak (Cafe) justru punya efisiensi foot traffic per outlet paling rendah — klasik tanda pasar jenuh (oversaturated market).

---

## 💡 Rekomendasi Strategis

### 1. Lokasi terbaik: Pontianak Tenggara
Rasio suplai-permintaan paling sehat di Pontianak — kompetitor rendah (88 outlet), foot traffic tinggi (192,76 ulasan/outlet, tertinggi kedua), dan rating pasar tinggi (4,13) yang menandakan ekspektasi kualitas konsumen di area ini tinggi namun tetap bisa dipenuhi.

### 2. Opsi A — Restaurant & Dining modern di Pontianak Tenggara
Buka restoran modern/akses cepat (kuliner lokal modern, Asian food seperti ramen/dimsum, atau ayam/fast-food berkualitas) menyasar keluarga muda, pekerja kantoran, dan mahasiswa. Kategori ini baru punya 37 pemain di Tenggara namun secara historis menarik foot traffic ~2,5× lebih besar dari Cafe biasa.

### 3. Opsi B — Bakery & Dessert premium di Pontianak Selatan/Kota
Buka bakery/dessert modern dan *instagramable* (artisan bakery, gelato, modern dessert house) di sekitar Pontianak Selatan atau Pontianak Kota. Kategori ini adalah *under-served market* dengan foot traffic dan rating tertinggi di Pontianak — meski didorong oleh beberapa nama legendaris, ini membuktikan ada minat besar masyarakat terhadap dessert yang belum banyak digarap secara modern.

### 4. Rekomendasi Aksi Lanjutan
Sebelum eksekusi, lakukan **validasi lokasi langsung** di titik-titik terpilih di Pontianak Tenggara: bandingkan foot traffic aktual di lapangan, kepadatan & jenis kompetitor terdekat, rentang harga, target konsumen, dan jam ramai — karena data Google Maps adalah proxy, bukan pengganti survei lapangan.

### ⚠️ Catatan & Keterbatasan
- Foot traffic di sini adalah **proxy dari jumlah ulasan Google (reviewsCount)**, bukan data kunjungan aktual — bisnis lama cenderung terakumulasi lebih banyak ulasan.
- Kategori **Bakery & Dessert dan Street Food** memiliki sampel sangat kecil (13 venue masing-masing), sehingga rata-ratanya rentan bias oleh outlier — perlakukan sebagai sinyal awal, bukan angka pasti.
- Klasifikasi kecamatan sebagian dilakukan lewat pembersihan teks otomatis + koreksi manual pada data yang ambigu, sehingga tetap ada kemungkinan kecil bias kategorisasi wilayah.
1. **Data Temporal** - Snapshot saat scraping, bukan time-series
2. **Missing Reviews** - 14.24% bisnis tidak memiliki review/rating
3. **Bias** - Bisnis yang lebih mapan lebih likely untuk have reviews
4. **Geographic Accuracy** - Bergantung pada akurasi Google Maps
5. **Category Ambiguity** - Beberapa bisnis memiliki multi-kategori


## 📁 Struktur File

```
├── README.md                          # Dokumentasi proyek
├── Analisis_FnB_di_Pontianak_...ipynb # Jupyter notebook analisis
├── Data/
│   └── allfieldjson.json              # Raw data dari Google Maps
└── Results/                           # Output analisis
    ├── visualisasi_distribusi.png     # Peta distribusi bisnis
    ├── analisis_kategori.csv          # Breakdown kategori
    └── rekomendasi_lokasi.csv         # Rekomendasi area
```

## 🛠️ Tools & Technologies

- **Python 3.x**
- **Pandas** - Data manipulation & analysis
- **Numpy** - Numerical computations
- **Matplotlib & Seaborn** - Data visualization
- **Google Maps API** - Data collection
- **Jupyter Notebook** - Interactive analysis




## 📞 Kontak & Kontribusi

Proyek ini dibuat untuk tujuan analisis pasar dan insights bisnis.

---

**Last Updated**: September 2026  
**Data Collection Date**: September 2026  
**Coverage Area**: Kota Pontianak, West Kalimantan, Indonesia

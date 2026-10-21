# Analisis FnB di Pontianak Berdasarkan Kecamatan dan Foot Traffic (Data Google Maps)

## 📋 Deskripsi Proyek

Proyek ini menganalisis kompetitor bisnis Food & Beverage (FnB) di wilayah Pontianak menggunakan data dari Google Maps. Analisis ini berfokus pada distribusi bisnis FnB berdasarkan kecamatan, kategori, dan metrik performa seperti jumlah ulasan serta rating.

## 🎯 Tujuan Analisis

Proyek ini dirancang untuk menjawab pertanyaan-pertanyaan bisnis strategis berikut:

1. **Di kecamatan mana peluang terbaik untuk membuka bisnis F&B baru?**

2. **Kategori F&B apa yang sebaiknya dibuka dan di mana lokasinya?**

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

### 3. Bakery & Dessert: hati-hati bias outlier

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


## 💡Konklusi dan Menjawab Pertanyaan

### 1. Di kecamatan mana peluang terbaik untuk membuka bisnis F&B baru?
## Rekomendasi Utama: Pontianak Tenggara
Mengapa? Kecamatan ini memiliki rasio suplai-permintaan paling sehat.
Kompetisi Rendah: Hanya memiliki 88 kompetitor (jauh lebih sedikit dibandingkan pusat keramaian utama seperti Pontianak Kota dengan 351 outlet dan Pontianak Selatan dengan 308 outlet).
Kunjungan Pelanggan (Foot Traffic) Tinggi: Mencatatkan rata-rata 192.76 ulasan per outlet (tertinggi kedua setelah Pontianak Selatan). Hal ini mengindikasikan pasar lokal di Pontianak Tenggara sangat aktif, haus akan opsi kuliner baru, tetapi belum jenuh.
Tingkat Kepuasan Tinggi: Rata-rata rating di kawasan ini mencapai 4.13, menandakan ekspektasi kualitas yang tinggi dari konsumen lokal.

### 2. Kategori F&B apa yang sebaiknya dibuka dan di mana lokasinya?
## Opsi A: Membuka Restoran Modern / Akses Cepat di Pontianak Tenggara
Rekomendasi Bisnis: Restaurant & Dining (khususnya sub-kategori makanan lokal modern, kuliner Asia populer seperti ramen/dimsum, atau hidangan ayam/fast-food berkualitas).
Analisis Peluang:
Dominasi pasar di Pontianak Tenggara saat ini masih diduduki oleh format Cafe/Warkop (45 outlet) dan Restaurant & Dining (37 outlet).
Dibandingkan dengan tren kejenuhan industri kopi (Cafe/Warkop) di Pontianak secara keseluruhan, kategori Restaurant & Dining secara konsisten menarik rata-rata foot traffic yang jauh lebih masif (285 ulasan per outlet) dibandingkan Cafe biasa (hanya 114 ulasan per outlet).
Membuka restoran dengan target keluarga muda, pekerja kantoran, dan mahasiswa di Pontianak Tenggara akan memberikan tingkat keberhasilan jangka panjang yang lebih tinggi.



### ⚠️ Catatan & Keterbatasan
- Foot traffic di sini adalah **proxy dari jumlah ulasan Google (reviewsCount)**, bukan data kunjungan aktual — bisnis lama cenderung terakumulasi lebih banyak ulasan.
- Kategori **Bakery & Dessert dan Street Food** memiliki sampel sangat kecil (13 venue masing-masing), sehingga rata-ratanya rentan bias oleh outlier — perlakukan sebagai sinyal awal, bukan angka pasti.
- Klasifikasi kecamatan sebagian dilakukan lewat pembersihan teks otomatis + koreksi manual pada data yang ambigu, sehingga tetap ada kemungkinan kecil bias kategorisasi wilayah.
1. **Data Temporal** - Snapshot saat scraping, bukan time-series
2. **Missing Reviews** - 14.24% bisnis tidak memiliki review/rating
3. **Bias** - Bisnis yang lebih mapan lebih likely untuk have reviews
4. **Geographic Accuracy** - Bergantung pada akurasi Google Maps
5. **Category Ambiguity** - Beberapa bisnis memiliki multi-kategori


---

**Last Updated**: September 2026  
**Data Collection Date**: September 2026  
**Coverage Area**: Kota Pontianak, West Kalimantan, Indonesia

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

5. **Bagaimana foot traffic patterns mempengaruhi kesuksesan bisnis FnB?**
   - Hubungan antara popularitas lokasi dan performa bisnis
   - Analisis seasonal patterns

## 📊 Dataset

### Sumber Data
- **Platform**: Google Maps API
- **Total Records**: 1,250 bisnis FnB
- **Periode**: Data terkini (scraped dari Google Maps)
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

## 🔍 Hasil Analisis Utama

### 1. Distribusi Bisnis FnB Menurut Kecamatan

**Temuan Kunci:**
- Bisnis FnB tersebar di seluruh wilayah Pontianak
- Kecamatan dengan konsentrasi bisnis tertinggi menunjukkan tingkat urbanisasi dan foot traffic yang lebih tinggi
- Area tertentu menunjukkan potensi pasar yang belum tergali

**Implikasi Bisnis:**
- Area berkonsentrasi tinggi: pasar jenuh, kompetisi ketat, margin keuntungan lebih kecil
- Area dengan konsentrasi rendah: peluang bisnis baru, persaingan lebih rendah

### 2. Kategori Bisnis FnB yang Dominan

**Analisis Kategori:**
- **Cafe & Coffee Shop** - Segmen paling populer
- **Restoran** - Berbagai jenis (Indonesian, Chinese, dll)
- **Warkop/Warung Kopi** - Kategori lokal yang kuat
- **Takeout/Delivery** - Trend modern yang berkembang

**Insight:**
- Cafe dan coffee shop mendominasi pasar Pontianak
- Preferensi konsumen lokal terhadap warung tradisional tetap kuat
- Peluang pertumbuhan di kategori modern seperti delivery

### 3. Performa Berdasarkan Rating dan Ulasan

**Statistik Rating:**
- Rating rata-rata: 4.0 - 4.8 dari 5
- Distribusi rating menunjukkan kepuasan pelanggan yang relatif tinggi
- Bisnis dengan rating tinggi umumnya memiliki lebih banyak ulasan

**Engagement Pelanggan:**
- Jumlah ulasan bervariasi dari 0 hingga 300+
- Bisnis dengan rating 4.5+ rata-rata memiliki lebih banyak engagement
- Review count menunjukkan pengaruh terhadap visibility di Google Maps

### 4. Hubungan Antara Rating, Review Count, dan Kesuksesan

**Korelasi Positif Ditemukan:**
- Bisnis dengan rating tinggi + review banyak → performa lebih baik
- Rating stabil 4.5+ dengan review 100+ → indikasi bisnis mapan
- Bisnis baru (review sedikit) dengan rating tinggi → potensi pertumbuhan

### 5. Peluang Bisnis FnB Baru

**Area Terbuka (Low Competition):**
- Kecamatan tertentu dengan bisnis FnB lebih sedikit
- Kategori tertentu dengan supplier minimal
- Potensi untuk niching market

**Kategori Berkembang:**
- Specialty cafe (kopi specialty, artisanal)
- Modern fusion food
- Health-conscious restaurants
- Quick service restaurants (QSR)

## 📈 Visualisasi & Pola yang Ditemukan

### Key Findings:

1. **Geographic Concentration**
   - Bisnis FnB terkonsentrasi di area pusat kota
   - Potensi ekspansi di area pinggiran

2. **Quality Indicator**
   - High rating (4.5+) dengan review 50+ = profitable & established
   - Rating 4.0-4.5 dengan review 20-50 = moderate performance
   - Rating < 4.0 atau review < 5 = perlu improvement atau startup

3. **Market Gaps**
   - Beberapa kategori underserved
   - Certain neighborhoods have fewer options
   - Opportunity untuk specialized services

4. **Competition Levels**
   - High: Cafe segment
   - Medium: Local restaurants
   - Low: Modern concepts, delivery services

## 💡 Rekomendasi Strategis

### Untuk Entrepreneur Baru:

1. **Lokasi Strategis**
   - Pilih area dengan foot traffic tinggi namun kompetisi sedang
   - Hindari area dengan konsentrasi bisnis sangat tinggi

2. **Diferensiasi Produk**
   - Jangan copy-paste kategori yang sudah ramai
   - Tawarkan unique value proposition
   - Fokus pada kategori dengan review rendah (indikasi kurangnya supplier)

3. **Rating & Review Management**
   - Target rating minimal 4.5+ sejak awal operasional
   - Bangun review organik melalui customer satisfaction
   - Review count 50+ dalam 6 bulan pertama adalah milestone penting

### Untuk Bisnis Existing:

1. **Improvement Priority**
   - Jika rating < 4.0 → fokus improvement kualitas
   - Jika review count < 10 → fokus on customer engagement
   - Jika rating 4.5+ dan review 100+ → maintain & expand

2. **Market Positioning**
   - Monitor kompetitor di kategori dan area yang sama
   - Identify unique selling points
   - Invest in customer retention

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

## 📚 Methodology

### Data Processing:
1. Data loading dari JSON (Google Maps API output)
2. Data cleaning & missing value handling
3. Feature engineering (geografis, kategori extraction)
4. Outlier detection & handling

### Analysis Approach:
1. **Descriptive Statistics** - Overview distribusi data
2. **Geographic Analysis** - Spatial clustering
3. **Category Analysis** - Market segmentation
4. **Correlation Analysis** - Rating vs engagement
5. **Competitive Analysis** - Market positioning

## ⚠️ Limitasi & Catatan

1. **Data Temporal** - Snapshot saat scraping, bukan time-series
2. **Missing Reviews** - 14.24% bisnis tidak memiliki review/rating
3. **Bias** - Bisnis yang lebih mapan lebih likely untuk have reviews
4. **Geographic Accuracy** - Bergantung pada akurasi Google Maps
5. **Category Ambiguity** - Beberapa bisnis memiliki multi-kategori

## 🎓 Kesimpulan

Analisis ini memberikan gambaran komprehensif tentang landscape bisnis FnB di Pontianak. Data menunjukkan bahwa:

- **Pasar FnB di Pontianak aktif** dengan >1,000 bisnis yang terdaftar
- **Rating tinggi konsisten** (4.0-4.8) menunjukkan kualitas layanan yang baik
- **Peluang masih ada** di area tertentu dan kategori spesifik
- **Lokasi dan kategori** adalah faktor kritis kesuksesan bisnis FnB
- **Review count & rating** adalah indikator performa yang reliable

Entrepreneur yang ingin masuk industri FnB di Pontianak harus melakukan riset mendalam tentang lokasi, kategori, dan diferensiasi produk berdasarkan insights dalam analisis ini.

## 📞 Kontak & Kontribusi

Proyek ini dibuat untuk tujuan analisis pasar dan insights bisnis.

---

**Last Updated**: September 2026  
**Data Collection Date**: September 2026  
**Coverage Area**: Kota Pontianak, West Kalimantan, Indonesia

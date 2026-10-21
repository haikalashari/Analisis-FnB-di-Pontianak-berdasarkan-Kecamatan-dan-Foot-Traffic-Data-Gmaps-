# Analisis FnB di Pontianak Berdasarkan Kecamatan dan Foot Traffic (Data Google Maps)

## 📋 Deskripsi Proyek

Proyek ini menganalisis kompetitor bisnis Food & Beverage (FnB) di wilayah Pontianak menggunakan data hasil scraping Google Maps. Analisis berfokus pada distribusi bisnis FnB berdasarkan kecamatan dan kategori, lalu dibandingkan dengan metrik performa berupa jumlah ulasan (`reviewsCount`) dan rating (`totalScore`).

> **Penting untuk dibaca dulu:** data ini adalah **sampel hasil pencarian Google Maps** dari 5 kata kunci, bukan sensus seluruh FnB Pontianak. Semua angka di bawah harus dibaca dalam kerangka itu.

## 🎯 Tujuan Analisis

1. **Di kecamatan mana peluang terbaik untuk membuka bisnis F&B baru?**
2. **Kategori F&B apa yang sebaiknya dibuka dan di mana lokasinya?**

Keduanya dijawab di bagian [Konklusi](#-konklusi-dan-menjawab-pertanyaan), masing-masing disertai tingkat keyakinan dan bukti yang mendukungnya.

## 📊 Dataset

### Sumber Data
- **Platform**: Google Maps (hasil scraping, `allfieldjson.json`)
- **Total record**: 1.250 baris × 62 kolom mentah
- **Kata kunci pencarian**: `coffe shop` (380), `warkop` (264), `restoran` (263), `cafe` (182), `kuliner` (161)
- **Periode**: scraping 16 September 2026 (`scrapedAt`)
- **Coverage Area**: Kota Pontianak, West Kalimantan

### Fitur Utama Dataset
| Fitur | Deskripsi | Tipe Data |
|-------|-----------|-----------|
| `title` | Nama bisnis FnB | Text |
| `categoryName` | Kategori utama bisnis | Text |
| `categories` | Daftar kategori (multi-label) | List |
| `neighborhood` | Lingkungan/kelurahan | Text |
| `address` | Alamat lengkap | Text |
| `location` | Koordinat geografis (lat, lng) | GeoJSON |
| `reviewsCount` | Jumlah ulasan Google Maps (**proxy foot traffic**) | Numeric |
| `totalScore` | Rating rata-rata (1–5) | Numeric |
| `permanentlyClosed` / `temporarilyClosed` | Status tutup | Boolean |
| `searchString` / `rank` | Kata kunci & posisi di hasil pencarian | Text / Numeric |

### Kualitas Data
| Metrik | Nilai |
|---|---|
| Total record scraping | 1.250 |
| Record **aktif** (tidak tutup) | **1.218** (0 `permanentlyClosed`, 32 `temporarilyClosed`) |
| Record **dianalisis** | **1.206** (setelah pembuangan non-FnB, 1 baris tanpa alamat, dan 6 venue di luar Pontianak) |
| `categoryName` lengkap | 99,68% (1.246/1.250) |
| `neighborhood` & `address` lengkap | 99,92% (1.249/1.250) |
| `reviewsCount` & `totalScore` terisi | 85,76% (1.072/1.250) — **14,24% kosong** |


## 📦 Metodologi Singkat

- **Data cleaning:**
  - Mengisi `reviewsCount` & `totalScore` yang kosong dengan **0** — konsekuensinya dibahas di [Keterbatasan](#️-catatan--keterbatasan).
  - Menghapus 1 baris tanpa alamat; mengisi 4 baris tanpa kategori sebagai `Cafe`.
  - Menstandarkan 73 variasi penulisan `neighborhood` (typo, bahasa Inggris/Indonesia campur) menjadi **6 kecamatan resmi Pontianak** (Kota, Selatan, Tenggara, Timur, Barat, Utara), termasuk koreksi manual untuk 10 baris yang salah klasifikasi dan pembuangan 6 venue di luar Pontianak.
  - Membuang baris dengan **26 nama kategori non-FnB** (38 baris: hotel, mall, gym, kantor, SPBU, dsb.).
- **Hasil akhir:** **1.206 bisnis F&B** siap dianalisis.
- **Kategorisasi:** puluhan `categoryName` mentah disederhanakan menjadi 4 kategori utama — **Cafe, Restaurant & Dining, Street Food, Bakery & Dessert**.
- **Metrik utama:** *foot traffic* = `reviewsCount` (proxy, bukan data kunjungan), dihitung sebagai **rata-rata (mean)** di notebook; pembacaan yang lebih aman memakai **median** (dihitung ulang di sini karena distribusinya sangat miring).
- **Tools:** Python (pandas, matplotlib, seaborn, folium) di Google Colab.

---

## 🔍 Hasil Analisis Utama

### 1. Peta persaingan per kecamatan

Sumber: output notebook (mean) + hitung ulang median dari `allfieldjson.json` (proses mirip cleaning, ±6 baris).

| Kecamatan | Jumlah Venue | Avg Ulasan/outlet (notebook) | Median Ulasan/outlet | Total Ulasan | Avg Rating (null→0) | Rating (null dibuang) | Venue tanpa ulasan |
|---|---:|---:|---:|---:|---:|---:|---:|
| Pontianak Selatan | 308 | 367,13 | 77 | 113.077 | 4,13 | 4,46 | 7,5% |
| **Pontianak Tenggara** | **88** | **192,76** | **42** | 16.963 | 4,13 | 4,42 | 7,1% |
| Pontianak Kota | 351 | 140,02 | 27 | 49.146 | 3,98 | 4,46 | 10,8% |
| Pontianak Timur | **81** | 77,35 | 20 | 6.265 | 3,76 | 4,49 | 16,2% |
| Pontianak Barat | 184 | 64,02 | 12 | 11.780 | 3,30 | 4,40 | 25,0% |
| Pontianak Utara | 194 | 59,21 | 11 | 11.486 | 3,37 | 4,45 | 24,2% |

**Insight:**
- **Jumlah venue ≠ performa.** Pontianak Kota punya venue terbanyak (351) tetapi median ulasan/outlet hanya 27 — sedangkan Tenggara dengan 88 venue mencapai median 42. Peringkat **mean dan median sama persis** (Selatan → Tenggara → Kota → Timur → Barat → Utara), jadi kesimpulan ini bukan efek outlier.
- **Tenggara bukan yang kompetitornya paling sedikit** — Pontianak Timur lebih sedikit (81). Tenggara adalah *kedua* terendah, dengan permintaan tercatat kedua tertinggi: itu yang menjadikannya kandidat terbaik, bukan klaim "paling sedikit".
- **Perbedaan rating antar kecamatan tidak bermakna.** Kolom "Avg Rating (null→0)" dihitung dengan mengisi 14,24% rating kosong menjadi 0, sehingga Barat/Utara tampak rendah (3,30/3,37). Setelah rating kosong dibuang, semua kecamatan berada di rentang **4,40–4,49** (median 4,5 di mana-mana). Jadi **rating tidak dipakai sebagai dasar rekomendasi** di bawah.
- **Selisih juga mencerminkan usia venue:** venue tanpa ulasan tersebar tidak merata (Barat 25,0% vs Tenggara 7,1%) — kecamatan dengan banyak venue baru/ belum ber-ulasan akan tertekan rata-ratanya.

### 2. Dominasi kategori: fenomena "1001 warkop"

| Kategori | Jumlah Venue | Avg Foot Traffic (notebook) | Median Foot Traffic | Avg Rating (null→0) | Rating (null dibuang) |
|---|---:|---:|---:|---:|---:|
| Bakery & Dessert | 13 | 429,15 | 17 | 4,33 | 4,69 |
| Restaurant & Dining | 385 | 285,02 | 38 | 4,12 | **4,45** |
| Street Food | 13 | 199,77 | 39 | 4,06 | 4,40 |
| Cafe | 795 | 114,23 | 25 | 3,65 | **4,45** |

**Insight:**
- **795 dari 1.206 (≈66%) adalah Cafe/Warkop** — Pontianak memang jauh lebih didominasi format ngopi daripada kategori lain.
- **Cafe memang paling lemah menurut jumlah ulasan, tetapi jaraknya tidak sebesar kesan "2,5×".** Dengan mean: 285 vs 114 (2,5×). Dengan median: 38 vs 25 (**hanya 1,5×**). Angka mean dinaikkan oleh segelintir venue raksasa.
- **Selisih rating Cafe (3,65) vs Restaurant (4,12) adalah artefak, bukan kualitas.** Rating kosong pada Cafe berjumlah 18,2% vs Restaurant 7,1%, dan keduanya diisi 0. Setelah null dibuang: **Cafe 4,45 = Restaurant 4,45** — *tidak ada beda rating sama sekali*.
- Kategori dengan median tertinggi: Street Food (39) dan Restaurant (38), keduanya dengan jumlah pemain jauh di bawah Cafe.

### 3. Bakery & Dessert: sampel sangat kecil, angka didominasi 1 outlier

Bakery & Dessert memang memuncaki rata-rata (429,15 ulasan/outlet; rating 4,33), tetapi:

- Hanya **13 venue** di seluruh Pontianak.
- **Es Krim Angi sendiri menyumbang 4.520 dari total 5.579 ulasan kategori (81%)**; pengikutnya Es Teler Jeranding 482 ulasan. Sebelas venue lainnya berada di kisaran 0–366 ulasan.
- **Tanpa Es Krim Angi, rata-rata kategori ini turun ke ±88–96 ulasan/outlet — di bawah Cafe (114)**, dan mediannya hanya 17.

Kesimpulan yang aman: kategori ini **jarang muncul di hasil pencarian** (13/1.206 = 1,1%), tetapi *bukan* berarti otomatis ramai. Angka 429,15 tidak boleh dipakai sebagai proyeksi permintaan.

### 4. Komposisi kategori di Pontianak Tenggara (kandidat lokasi utama)

| Kategori | Jumlah Venue | Avg Foot Traffic (notebook) | Median Foot Traffic |
|---|---:|---:|---:|
| Cafe | 45 | **218,87** | 160 |
| Restaurant & Dining | 37 | 135,35 | 22 |
| Street Food | 5 | 420,60 | 187 |
| Bakery & Dessert | 1 | 3,00 | 3 |

**Insight penting yang membatasi rekomendasi:**
- Di kecamatan yang paling direkomendasikan justru berlaku **kebalikan dari tren nasional**: Cafe Tenggara (218,87) **lebih kuat** dari Restaurant Tenggara (135,35), dan median-nya 160 vs 22.
- Bahkan **Cafe di Tenggara adalah Cafe terkuat se-Pontianak** (median 160 vs Selatan 59, Kota 25,5, Timur 20, Barat 9, Utara 10).
- Artinya: argumen *"warkop sudah jenuh, buka restoran"* **tidak berlaku di Tenggara** — di sana justru bukti lokal menunjukkan cafe masih kuat.

---

## 💡 Konklusi dan Menjawab Pertanyaan

### 1. Di kecamatan mana peluang terbaik untuk membuka bisnis F&B baru?

**Jawaban: Pontianak Tenggara — keyakinan sedang, dengan catatan.**

| Bukti (dari notebook) | Nilai | Posisi |
|---|---|---|
| Jumlah kompetitor | 88 | ke-2 terendah (Timur 81 lebih rendah) |
| Avg ulasan/outlet | 192,76 | ke-2 tertinggi (Selatan 367,13) |
| Median ulasan/outlet | 42 | ke-2 tertinggi (Selatan 77) |
| Total ulasan | 16.963 | ke-3 (Setelah Selatan 113.077, Kota 49.146) |
| Venue tanpa ulasan | 7,1% | terendah bersama Selatan |
| Rating | 4,42 (null dibuang) | praktis sama dengan kecamatan lain (4,40–4,49) |

**Mengapa Tenggara:** kombinasi *supply rendah + aktivitas tercatat menengah-tinggi* — satu-satunya kecamatan yang berada di kuadran itu. Peringkatnya bertahan baik dengan mean maupun median, sehingga bukan efek 1–2 venue raksasa.

**Mengapa bukan yang lain:**
- **Selatan** punya permintaan tercatat tertinggi (median 77) tetapi sudah diisi 308 venue → ruang tumbuh paling sempit.
- **Kota** venue terbanyak (351) dengan median rendah (27) → paling jenuh menurut metrik ini.
- **Timur** supply paling rendah (81) tetapi median hanya 20 → permintaan tercatat lemah.
- **Barat & Utara** median 12 dan 11, dengan 25% dan 24,2% venue tanpa ulasan → bukti aktivitas paling lemah.

**Catatan yang wajib disampaikan (jangan disembunyikan):**
1. "Foot traffic" = akumulasi jumlah ulasan, **bukan kunjungan** — venue lama/mapan terakumulasi lebih banyak ulasan.
2. Keunggulan Tenggara **sebagian** berasal dari basis venue yang lebih matang (hanya 7,1% tanpa ulasan, vs 25% di Barat) — bukan bukti "pasar haus akan opsi kuliner baru".
3. Rating **tidak** dipakai sebagai alasan: setelah null dibuang, Tenggara (4,42) justru sedikit di bawah Selatan (4,46), Kota (4,46), Timur (4,49).
4. Angka berasal dari 5 kata kunci pencarian; rank Google berkorelasi **ρ −0,37** dengan jumlah ulasan → sampel condong ke venue populer.
5. 32 venue tutup sementara masih termasuk dalam hitungan.

### 2. Kategori F&B apa yang sebaiknya dibuka dan di mana lokasinya?

**Jawaban singkat: secara nasional Restaurant & Dining menunjukkan bukti terkuat (keyakinan B), TETAPI di Pontianak Tenggara bukti lokalnya terbalik — jadi rekomendasi lokasi + kategori harus dipisah dan divalidasi lapangan.**

**a) Bukti nasional (seluruh 1.206 venue)**
- Restaurant & Dining: 385 venue, median 38 ulasan/outlet vs Cafe 795 venue, median 25 → **lebih efisien dengan pemain jauh lebih sedikit**.
- Ini konsisten di 5 dari 6 kecamatan (Kota, Selatan, Timur, Barat, Utara).
- **Kecuali Tenggara** — dan Tenggara adalah lokasi yang direkomendasikan.

**b) Tiga opsi beserta keyakinannya**

| Opsi | Keyakinan | Argumen kuat | Argumen lemah / syarat |
|---|---|---|---|
| **A. Restaurant & Dining di Pontianak Tenggara** | **B** | Nasional terbukti lebih efisien (median 38 vs 25); di Tenggara baru 37 pemain | Di Tenggara sendiri Restaurant (135,35) **di bawah** Cafe (218,87); median 22 vs 160. Klaim "secara konsisten lebih masif" **tidak berlaku di lokasi ini** → wajib survei: apakah demand rendah atau memang belum tergarap |
| **B. Cafe/ Warkop berkualitas di Pontianak Tenggara** | **B** | Bukti lokal terkuat di seluruh data (median 160, tertinggi nasional) | Jumlah pemain sudah 45 → ruang di segmen ini menyempit; harus punya diferensiasi jelas |
| **C. Bakery & Dessert (Pontianak Kota/Selatan)** | **C (rendah)** | Hanya 13 venue (1,1% dari sampel) → jarang muncul | Rata-rata 429,15 **artefak Es Krim Angi (81% ulasan kategori)**; tanpa Angi mean ±88–96 < Cafe. Belum ada bukti permintaan untuk kategori ini |

**Rekomendasi:**
1. **Lokasi: Pontianak Tenggara** (alasan di atas).
2. **Kategori: belum bisa diputuskan dari data ini saja.** Data hanya membuktikan dua hal: (i) Cafe sudah 66% pasar dan paling padat; (ii) Restaurant lebih efisien *secara nasional*. Di Tenggara keduanya justru bertolak belakang.
3. **Sebelum membuka apa pun, lakukan validasi lapangan** di 2–3 titik Tenggara: hitung ulang kompetitor sekitar (radius 500 m), jam ramai, rentang harga, dan target konsumen — karena `popularTimesHistogram` dan `price` pada data ini **kosong semua**.


---

## 📈 Visualisasi di Notebook

1. **Dual-axis bar + line** — jumlah kompetitor vs rata-rata ulasan vs rating per kecamatan.
2. **Grouped bar** — distribusi 4 kategori utama di tiap kecamatan (Cafe bar tertinggi di semua kecamatan).
3. **Heatmap kecamatan × kategori** — rata-rata foot traffic tiap kombinasi (sel terpanas: Street Food Tenggara 420,6; Cafe Tenggara 218,9 — bukan Cafe nasional).
4. **Peta interaktif Folium + marker cluster** — sebaran 1.206 venue berdasarkan lat/lng, dengan bubble per kecamatan (ukuran = rata-rata ulasan, warna = level kompetisi).

---

## ⚠️ Catatan & Keterbatasan

1. **Bias sampel (paling penting)** — 1.250 record berasal dari **5 kata kunci** dengan posisi rank 1–117. Rank berkorelasi **ρ −0,37** dengan jumlah ulasan → Google menampilkan venue populer lebih dulu, sehingga sampel condong ke venue besar. Angka = *"venue yang muncul di hasil pencarian"*, **bukan jumlah bisnis sebenarnya**.
2. **Rating bias** — 14,24% rating kosong **diisi 0** di notebook. Perbandingan rating antar kecamatan/kategori pada tabel atas karena itu tercap rendah tidak proporsional (Cafe null 18,2% vs Restaurant 7,1%; Barat/Utara null 24–25%). Setelah null dibuang seluruh kecamatan berada di 4,40–4,49.
3. **32 venue tutup sementara** masih ikut terhitung (bug filter pada notebook).
4. **Foot traffic = proxy `reviewsCount`**, bukan data kunjungan aktual; bisnis lama/mapan terakumulasi lebih banyak ulasan.
5. **Sampel sangat kecil** pada Bakery & Dessert dan Street Food (13 venue masing-masing) → rentan outlier; Bakery dikuasai 1 venue (81% ulasan).
6. **Data temporal** — snapshot satu tanggal (16 Sep 2026), bukan time-series; tidak bisa menjawab tren/kelangsungan usaha.
7. **Klasifikasi kecamatan** dilakukan lewat pembersihan teks otomatis + koreksi manual → masih ada kemungkinan bias wilayah.
8. **Multi-kategori** — sebagian bisnis punya lebih dari satu kategori; hanya `categoryName` utama yang dipetakan ke 4 kelompok.

---

## 📁 Struktur File

```
├── README.md                          # Dokumentasi proyek
├── Analisis_FnB_di_Pontianak_...ipynb # Notebook analisis (Google Colab)
└── allfieldjson.json                  # Data mentah hasil scraping Google Maps (1.250 × 62)
```

> Catatan: folder `Data/` dan `Results/` yang disebut pada versi README sebelumnya tidak ada di repositori ini — struktur di atas adalah kondisi sebenarnya.

## 🛠️ Tools & Technologies

- **Python 3.x** · **Pandas** · **NumPy**
- **Matplotlib & Seaborn** (visualisasi statis) · **Folium** (peta interaktif)
- **Google Colab** (environment eksekusi)
- **Google Maps** (sumber data scraping)

---

**Last Updated**: Oktober 2026
**Data Collection Date**: 16 September 2026
**Coverage Area**: Kota Pontianak, West Kalimantan, Indonesia

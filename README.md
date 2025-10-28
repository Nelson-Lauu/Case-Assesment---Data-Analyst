# Laporan Case Assesment EmergencyCall - Data Analyst
## Domain Proyek
Bidang yang dianalisis dalam case assessment ini adalah **Kesehatan & Layanan Konseling Digital**, dengan fokus pada performa sistem berbasis AI dan pengalaman pengguna dalam platform **Emergency Call AI Support System**.

## Judul Proyek
**Analisis Performa Sistem AI dan Optimalisasi Layanan Konseling Digital**

### Latar Belakang

Perusahaan sedang mengembangkan **Emergency Call AI Support System**, sebuah platform berbasis kecerdasan buatan yang bertujuan untuk membantu pengguna mendapatkan respons cepat dalam layanan konseling darurat.  

Namun, hasil pemantauan internal menunjukkan beberapa isu operasional yang mulai memengaruhi kinerja sistem dan tingkat kepuasan pengguna, seperti:
- Peningkatan volume pengguna aktif hingga 28%, disertai lonjakan waktu respons AI sebesar 35%.  
- Penurunan kepuasan pengguna pada sesi malam hari, terutama yang melibatkan interaksi AI.  
- Ketidakseimbangan beban kerja antar shift konselor, di mana beberapa shift mengalami overload sementara lainnya idle.  
- Ditemukannya anomali data log berupa *duplicate entries* dan *missing reports* pada sistem pencatatan sesi.

Sebagai Data Analyst, tugas utama adalah **mengidentifikasi akar penyebab permasalahan tersebut melalui analisis data**, serta menyusun **rekomendasi strategis berbasis data** untuk peningkatan performa sistem, keseimbangan sumber daya, dan kepuasan pengguna secara menyeluruh.

## Business Understanding

### Problem Statements
- Bagaimana pengaruh **peningkatan jumlah pengguna** terhadap performa sistem AI (terutama waktu respons dan akurasi)?  
- Faktor apa yang menyebabkan **penurunan kepuasan pengguna pada shift malam**, terutama di sesi berbasis AI?  
- Apakah terdapat **ketidakseimbangan beban kerja antar shift konselor** yang berdampak pada antrean pengguna dan pengalaman layanan?  
- Sejauh mana **kualitas data log sistem (duplikasi dan missing entries)** memengaruhi reliabilitas pelaporan performa dan kepuasan pengguna?  

### Goals
- Mengidentifikasi **hubungan antara traffic pengguna dan performa AI** untuk menentukan potensi *bottleneck* sistem.  
- Menganalisis **tingkat kepuasan pengguna berdasarkan waktu dan jenis sesi (AI vs Non-AI)** guna menemukan akar penyebab penurunan pengalaman pengguna.  
- Mengevaluasi **distribusi tenaga konselor dan panjang antrean antar shift** untuk memahami ketidakseimbangan operasional.  
- Menilai **kualitas data log** untuk memastikan keandalan metrik performa dan kepuasan yang digunakan dalam pengambilan keputusan manajemen.  

### Solution Statements
- Melakukan **Exploratory Data Analysis (EDA)** secara mendalam untuk menemukan pola antara variabel operasional utama: jumlah sesi, waktu respons AI, akurasi AI, antrean konselor, dan skor kepuasan pengguna.  
- Menghitung **korelasi statistik (Pearson)** antar variabel guna memahami hubungan sebab-akibat yang mungkin terjadi dalam performa sistem.  
- Membuat **visualisasi tren dan perbandingan antar shift** untuk mengidentifikasi periode waktu yang paling bermasalah (overload atau idle).  
- Mengidentifikasi dan memvalidasi **anomali data log** seperti duplikasi dan *missing entries* yang dapat memengaruhi hasil analisis.  
- Menyusun **rekomendasi berbasis data** terkait optimalisasi sistem, redistribusi sumber daya, dan perbaikan pipeline data agar performa dan kepuasan pengguna meningkat secara berkelanjutan.

## Data Understanding

Dataset yang digunakan merupakan data operasional dari **Emergency Call AI Support System**, yang berisi catatan interaksi pengguna, performa AI, serta aktivitas konselor manusia pada periode **September 2025**.  

Dataset ini digunakan untuk menganalisis hubungan antara performa sistem, tingkat kepuasan pengguna, dan keseimbangan sumber daya antar shift waktu.  

**Jumlah data:** 2.666 baris  
**Jumlah fitur:** 15 kolom  

### Struktur Dataset
| No | Kolom | Deskripsi | Tipe Data |
|----|--------|------------|------------|
| 1 | `timestamp` | Waktu lengkap terjadinya sesi (tanggal dan jam) | object |
| 2 | `date` | Tanggal sesi dalam format harian | object |
| 3 | `hour` | Jam sesi (format 0–23) | int64 |
| 4 | `session_id` | ID unik setiap sesi pengguna | object |
| 5 | `user_id` | ID unik pengguna | object |
| 6 | `channel` | Jenis kanal yang digunakan (mis. chat, voice, atau AI) | object |
| 7 | `session_length_min` | Durasi sesi dalam menit | int64 |
| 8 | `ai_response_time_ms` | Waktu respons AI dalam milidetik | float64 |
| 9 | `ai_accuracy` | Nilai akurasi hasil respons AI | float64 |
| 10 | `counselors_on_shift` | Jumlah konselor aktif pada waktu tersebut | int64 |
| 11 | `counselor_queue_length` | Panjang antrean pengguna pada shift tersebut | int64 |
| 12 | `counselor_available` | Status ketersediaan konselor (True/False) | bool |
| 13 | `user_satisfaction_score` | Skor kepuasan pengguna (skala 1–5) | float64 |
| 14 | `duplicate_flag` | Penanda apakah data sesi terduplikasi | bool |
| 15 | `missing_report_flag` | Penanda apakah data sesi memiliki laporan tidak lengkap | bool |

---

**Catatan:**
- Kolom `ai_response_time_ms` dan `ai_accuracy` memiliki beberapa nilai *missing* (sekitar 25%), terutama pada data sesi non-AI.  
- Kolom `duplicate_flag` dan `missing_report_flag` digunakan untuk mendeteksi anomali pada sistem logging.  
- Dataset ini merepresentasikan gabungan antara **sesi AI dan Non-AI**, yang akan dibandingkan lebih lanjut dalam tahap *Exploratory Data Analysis (EDA)*.

## Data Cleaning

Tahap *data cleaning* dilakukan untuk memastikan dataset siap dianalisis dengan hasil yang akurat dan konsisten.  
Beberapa langkah utama yang dilakukan meliputi:

### 1. Pemeriksaan Nilai Kosong (*Missing Values*)
Pemeriksaan nilai kosong dilakukan untuk memastikan tidak ada data penting yang hilang sebelum tahap analisis.
Hasil pemeriksaan:

| Kolom | Jumlah Missing Values |
|-------|------------------------|
| timestamp | 0 |
| date | 0 |
| hour | 0 |
| session_id | 0 |
| user_id | 0 |
| channel | 0 |
| session_length_min | 0 |
| ai_response_time_ms | **680** |
| ai_accuracy | **680** |
| counselors_on_shift | 0 |
| counselor_queue_length | 0 |
| counselor_available | 0 |
| user_satisfaction_score | 0 |
| duplicate_flag | 0 |
| missing_report_flag | 0 |

**Interpretasi:**
- Hasil pemeriksaan missing values menunjukkan bahwa dua kolom terkait performa AI (`ai_response_time_ms`, `ai_accuracy`) memiliki tingkat missing sekitar **25%**. Setelah investigasi, missing ini merepresentasikan sesi yang **tidak melibatkan AI (non-AI sessions)** sehingga kami **tidak menghapus** baris-baris tersebut.
- Sebagai gantinya, kami menambahkan kolom `is_ai_session` (`True` jika nilai AI ada) dan melakukan analisis performa AI hanya pada subset `is_ai_session=True`.  
- Untuk baris yang memiliki `missing_report_flag=True`, kami menandai dan **mengecualikannya dari analisis outcome** yang membutuhkan laporan sesi lengkap, namun tetap menghitungnya dalam metrik *traffic* dan *beban shift*.  
- Semua keputusan pembersihan terdokumentasi dan diuji melalui *sensitivity analysis* untuk memastikan hasil analisis tidak bias.

**Kode Implementasi:**

```python
# Kolom 'is_ai_session' → True jika ada nilai pada kolom AI
df['is_ai_session'] = df['ai_response_time_ms'].notnull()
```

### **2. Pemeriksaan Data Duplikat**

Langkah selanjutnya adalah memastikan tidak terdapat duplikasi baris yang dapat mengganggu hasil analisis statistik maupun perhitungan agregasi.

**Kode Implementasi:**

```python
# Cek apakah terdapat baris duplikat pada dataset
df.duplicated().sum()
0
```
**Interpretasi:**
- Berdasarkan hasil pemeriksaan, tidak ada baris data yang identik di seluruh kolom (pada level baris).
Namun, analisis lanjutan di tahap Exploratory Data Analysis (EDA 4) menemukan adanya duplikasi sistemik, yaitu sesi pengguna yang tercatat lebih dari sekali akibat retry event ketika beban server tinggi.

Oleh karena itu:
- Tidak dilakukan penghapusan data pada tahap cleaning.
- Kolom duplicate_flag tetap dipertahankan untuk keperluan analisis anomali log di EDA selanjutnya.
Keputusan ini diambil agar hasil analisis tetap mencerminkan kondisi operasional sistem secara realistis, termasuk potensi logging error yang menjadi bagian penting dari studi performa.

### 3. Analisis Deskriptif Awal (*Statistical Summary*)

Langkah ini bertujuan untuk memahami gambaran umum distribusi data dan mendeteksi pola awal sebelum dilakukan analisis eksploratif lebih lanjut.

**Kode Analisis:**

```python
# Melihat statistik deskriptif dari kolom numerik
df.describe()
```
| Variabel | Count | Mean | Std | Min | 25% | 50% | 75% | Max |
|-----------|--------|------|------|------|------|------|------|------|
| hour | 2666 | 11.65 | 6.94 | 0 | 6 | 12 | 18 | 23 |
| session_length_min | 2666 | 16.61 | 10.26 | 2 | 8 | 16 | 24 | 49 |
| ai_response_time_ms | 1986 | 1154.06 | 376.83 | 354 | 875 | 1096 | 1372.75 | 2758 |
| ai_accuracy | 1986 | 0.785 | 0.087 | 0.42 | 0.73 | 0.80 | 0.85 | 1.00 |
| counselors_on_shift | 2666 | 4.28 | 2.49 | 1 | 2 | 4 | 6 | 16 |
| counselor_queue_length | 2666 | 1.39 | 1.88 | 0 | 0 | 0 | 2 | 10 |
| user_satisfaction_score | 2666 | 3.69 | 0.64 | 1.28 | 3.25 | 3.68 | 4.13 | 5.00 |

**Interpretasi:**

- **Distribusi Waktu Operasional (`hour`)**
Aktivitas pengguna tersebar merata antara pukul 00–23, dengan median di jam ke-12 (sekitar pukul 12 siang).  
Hal ini menunjukkan pengguna aktif hampir sepanjang hari, dengan potensi puncak aktivitas di siang hingga sore hari.

- **Durasi Sesi (`session_length_min`)**
Rata-rata durasi sesi adalah **16,6 menit**, dengan variasi sedang (std ≈ 10,26 menit).  
75% sesi berlangsung kurang dari 24 menit, dan sesi terpanjang mencapai 49 menit.  
Ini menandakan interaksi berlangsung cepat dan efisien — tipikal platform konseling digital berbasis percakapan singkat.

- **Performa AI (`ai_response_time_ms`, `ai_accuracy`)**
Rata-rata waktu respons AI adalah **1.154 ms (~1,15 detik)** — menunjukkan sistem bekerja efisien.  
Nilai maksimum 2.758 ms masih dalam batas wajar, walaupun menunjukkan variasi saat beban sistem tinggi.  
Rata-rata akurasi AI sebesar **0,785 (78,5%)** dengan sebaran sempit (std ≈ 0,086), menandakan performa model cukup stabil.

- **Beban Konselor (`counselors_on_shift`, `counselor_queue_length`)**
Jumlah konselor aktif per shift berkisar antara **1–16**, dengan rata-rata **4,28 konselor**.  
Antrean pengguna relatif rendah (rata-rata **1,39**, median 0), artinya sebagian besar pengguna langsung mendapat layanan.  
Namun, nilai maksimum antrean mencapai **10 pengguna**, mengindikasikan potensi penumpukan di jam tertentu.

- **Kepuasan Pengguna (`user_satisfaction_score`)**
Skor rata-rata kepuasan pengguna adalah **3,68/5**, dengan rentang **1,28–5**.  
75% sesi mendapat skor ≥3,25, menunjukkan tingkat kepuasan yang cukup baik.  
Nilai rendah (sekitar 1,2–2,0) kemungkinan berasal dari sesi pada jam sibuk atau dengan latensi AI tinggi.

---

**Kesimpulan Awal:**
Dataset menunjukkan pola operasional yang stabil dengan sistem AI yang cepat dan akurat, serta tingkat kepuasan pengguna yang relatif tinggi.  
Potensi area perbaikan terletak pada **optimisasi distribusi konselor antar shift** dan **peningkatan efisiensi waktu respons AI** pada jam-jam sibuk.

### 4. Pemeriksaan Outlier dengan Boxplot

Langkah ini dilakukan untuk mendeteksi adanya nilai ekstrem (*outlier*) pada kolom numerik yang dapat memengaruhi analisis statistik.

**Kode Visualisasi:**

```python
# Menampilkan visual Boxplot - Pemeriksaan Outlier Tiap Kolom Numerik
numeric_cols = ['session_length_min', 'ai_response_time_ms', 'ai_accuracy', 'user_satisfaction_score']
fig, axes = plt.subplots(nrows=1, ncols=len(numeric_cols), figsize=(15, 5))

for i, col in enumerate(numeric_cols):
    axes[i].boxplot(df[col].dropna(), vert=True, patch_artist=True, boxprops=dict(facecolor='skyblue'))
    axes[i].set_title(col, fontsize=10)
    axes[i].set_ylabel('Value', fontsize=9)
    axes[i].grid(axis='y', linestyle='--', alpha=0.6)

plt.suptitle('Boxplot — Pemeriksaan Outlier Tiap Kolom Numerik', fontsize=12)
plt.tight_layout(rect=[0, 0, 1, 0.96])
plt.show()
```


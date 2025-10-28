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

**Kesimpulan:**
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
![Boxplot](https://drive.google.com/uc?export=view&id=1LOKghcspur2loLuHbv2uemK073fpvSml)

**Interpretasi**
Visualisasi boxplot di atas memperlihatkan sebaran dan potensi outlier pada setiap variabel numerik utama:
- **Session Length (Menit)**
Sebagian besar durasi sesi berada pada kisaran 8–25 menit, dengan median sekitar 16 menit.
Terdapat sedikit nilai di atas 45 menit yang muncul sebagai mild outlier, namun masih logis untuk sesi konseling yang lebih intens.

- **AI Response Time (ms)**
Median waktu respons sekitar 1.100 ms (1,1 detik), dengan beberapa outlier ringan di atas 2.000–2.500 ms.
Nilai ini masih dapat diterima sebagai variasi performa normal, kemungkinan disebabkan oleh beban sistem atau jaringan pada waktu tertentu.

- **AI Accuracy**
Nilai akurasi mayoritas terkonsentrasi di antara 0,7–0,9 dengan median di sekitar 0,8.
Outlier di bawah 0,5 menunjukkan beberapa sesi di mana model AI menghasilkan rekomendasi yang kurang optimal, namun jumlahnya sangat kecil.

- **User Satisfaction Score**
Skor kepuasan berkisar antara 3–4 dengan median 3,6.
Beberapa outlier rendah di bawah skor 2 menggambarkan sesi dengan tingkat kepuasan rendah — kemungkinan akibat keterlambatan respons atau antrean panjang.
---
**Kesimpulan:**

Secara keseluruhan, outlier yang muncul masih tergolong natural variation dan tidak perlu dihapus.
Visualisasi ini memperkuat kesimpulan sebelumnya bahwa dataset memiliki kualitas tinggi dan dapat langsung digunakan untuk tahap Exploratory Data Analysis (EDA).

### 5.Distribusi Nilai – Histogram
Distribusi Nilai Kolom Numerik Visualisasi histogram digunakan untuk memahami pola sebaran data pada setiap variabel numerik dan mengonfirmasi hasil pemeriksaan outlier sebelumnya.
**Kode Visualisasi**

```python
# Distribusi Nilai - Histogram
df[numeric_cols].hist(figsize=(10,8), bins=20, color='skyblue', edgecolor='black')
plt.suptitle('Distribusi Nilai Kolom Numerik')
plt.show()
```
![Histogram](https://drive.google.com/uc?export=view&id=125nTpy6SZ-6WB9vMyNRTqKSBXs2T7oz4)

**Interpetasi**
- **Session Length (Menit)**
Distribusi cenderung right-skewed (condong ke kanan), dengan mayoritas sesi berdurasi 5–25 menit dan penurunan frekuensi setelah 30 menit. Hal ini menggambarkan bahwa sebagian besar pengguna melakukan sesi singkat hingga sedang, sesuai konteks platform konseling daring yang fleksibel.

- **AI Response Time (ms)**
 Distribusi cenderung *right-skewed* (condong ke kanan), dengan mayoritas sesi berdurasi 5–25 menit dan penurunan frekuensi setelah 30 menit. Hal ini menggambarkan bahwa sebagian besar pengguna melakukan sesi singkat hingga sedang, sesuai konteks platform konseling daring yang fleksibel.

- **AI Accuracy**  
Distribusi sedikit *left-skewed* dengan mayoritas nilai berkisar antara 0.7–0.9. Ini menunjukkan model AI memiliki tingkat akurasi tinggi yang konsisten di seluruh sesi. Tidak ditemukan nilai ekstrem di bawah 0.5, menandakan performa model cukup handal.

- **User Satisfaction Score**  
Distribusi hampir normal, terpusat di sekitar skor 3.5–4.0. Pola ini menggambarkan bahwa kepuasan pengguna berada pada tingkat moderat ke tinggi, tanpa bias ke nilai rendah atau ekstrem.

---
**Kesimpulan:**
Secara keseluruhan, tidak ada distribusi yang menunjukkan anomali signifikan. Data numerik menunjukkan karakteristik alami dari interaksi pengguna dan performa sistem AI yang stabil.

### 6. Pemeriksaan outlier dengan nilai tak logis
Untuk memastikan integritas data numerik, dilakukan pemeriksaan terhadap kemungkinan nilai yang tidak logis (invalid values) pada variabel utama.

**Kode Implementasi:**
```python
num_cols = [
    'session_length_min',
    'ai_response_time_ms',
    'ai_accuracy',
    'user_satisfaction_score',
    'counselor_queue_length',
    'counselors_on_shift'
]

# Deteksi nilai tak logis dengan kondisi khusus
invalid_conditions = {
    'session_length_min_negatif': df[df['session_length_min'] < 0],
    'ai_response_time_ms_negatif': df[df['ai_response_time_ms'] < 0],
    'ai_response_time_ms_tinggi': df[df['ai_response_time_ms'] > 10000],   # lebih dari 10 detik
    'ai_accuracy_di_luar_rentang': df[(df['ai_accuracy'] < 0) | (df['ai_accuracy'] > 1)],
    'user_satisfaction_score_invalid': df[(df['user_satisfaction_score'] < 0) | (df['user_satisfaction_score'] > 5)],
    'queue_negatif': df[df['counselor_queue_length'] < 0],
    'counselor_negatif': df[df['counselors_on_shift'] < 0]
}

print("\n Jumlah baris dengan nilai tak logis per kategori:")
for key, subset in invalid_conditions.items():
    print(f"{key}: {subset.shape[0]}")

for key, subset in invalid_conditions.items():
    if subset.shape[0] > 0:
        print(f"\nContoh baris {key}:")
        display(subset.head(3))


Jumlah baris dengan nilai tak logis per kategori:
session_length_min_negatif: 0
ai_response_time_ms_negatif: 0
ai_response_time_ms_tinggi: 0
ai_accuracy_di_luar_rentang: 0
user_satisfaction_score_invalid: 0
queue_negatif: 0
counselor_negatif: 0
```
**Interpretasi:**

Pemeriksaan mencakup:
1. Nilai negatif pada variabel yang seharusnya positif (mis. durasi, waktu respons, antrean, jumlah konselor).  
2. Nilai di luar skala wajar, seperti:
   - `ai_response_time_ms` > 10.000 ms (10 detik)
   - `ai_accuracy` di luar rentang 0–1
   - `user_satisfaction_score` di luar skala 0–5

Hasil pengecekan menunjukkan **tidak ada baris data yang mengandung nilai tak logis** di seluruh kategori yang diuji.  
Dengan kata lain, seluruh nilai numerik berada dalam rentang yang realistis dan sesuai konteks operasional sistem.
Hal ini menunjukkan bahwa proses pencatatan data (logging) berjalan dengan baik tanpa error sistemik yang menyebabkan nilai anomali ekstrem.

### 7. Konversi format waktu
Kolom `timestamp` dan `date` pada dataset awal masih berupa tipe data `object` (string), sehingga belum dapat digunakan untuk analisis berbasis waktu seperti agregasi per jam, hari, atau shift.  

**Kode Implementasi:**
```python
# Konversi kolom 'timestamp' dan 'date' ke tipe datetime
df['timestamp'] = pd.to_datetime(df['timestamp'], format='%d/%m/%Y %H:%M', errors='coerce')
df['date'] = pd.to_datetime(df['date'], format='%d/%m/%Y', errors='coerce')
print(df[['timestamp', 'date']].head())
```
Dengan konversi ini, sistem dapat membaca dan mengolah waktu secara numerik — misalnya menghitung selisih antar waktu, melakukan grouping per jam, atau menganalisis pola aktivitas pengguna berdasarkan waktu tertentu.
Proses ini menjadi dasar penting untuk analisis tren dan distribusi waktu, terutama karena isu utama dalam studi ini berhubungan dengan penurunan kepuasan pengguna pada malam hari dan perbedaan performa antar shift.

Dengan tipe data waktu yang sudah tepat, analisis temporal menjadi akurat dan dapat digunakan untuk mendukung pengambilan keputusan berbasis waktu operasional.

### 8. Pembuatan Kolom Shift

Untuk menganalisis pola performa sistem dan kepuasan pengguna berdasarkan waktu operasional, dibuat kolom turunan baru bernama shift.

**Kode Implementasi**
```python
def get_shift(hour):
    if 6 <= hour < 14:
        return 'Morning'
    elif 14 <= hour < 22:
        return 'Afternoon'
    else:
        return 'Night'

df['shift'] = df['hour'].apply(get_shift)
```
**Interpretasi:**
- Kolom ini mengelompokkan setiap sesi ke dalam tiga periode waktu utama:
  1. Morning : 06:00 – 13:59
  2. Afternoon : 14:00 – 21:59
  3. Night : 22:00 – 05:59
- Pembagian ini bertujuan untuk mengamati perbedaan performa sistem, beban kerja, dan kepuasan pengguna antar periode waktu.

- Hal ini relevan dengan temuan awal perusahaan yang menunjukkan bahwa:
  1. Kepuasan pengguna menurun 15% pada malam hari, dan
  2. Beberapa shift mengalami overload sementara shift lain idle.
- Dengan adanya kolom shift, analisis dapat dilakukan secara lebih tersegmentasi — misalnya membandingkan AI response time, user satisfaction, dan queue length pada tiap periode waktu, sehingga mendukung evaluasi operasional yang lebih presisi.

## Exploratory Data Analysis
### Arah dan Tujuan EDA
Tahap *Exploratory Data Analysis (EDA)* difokuskan untuk menjawab permasalahan utama yang telah diidentifikasi oleh tim internal.  
Analisis diarahkan untuk memahami pola, tren, dan hubungan antar variabel utama yang memengaruhi performa sistem dan kepuasan pengguna.

### Fokus Analisis

1. **Performa Sistem AI terhadap Lonjakan Pengguna**  
   - Menilai korelasi antara peningkatan jumlah sesi pengguna (*traffic load*) dengan waktu respons dan akurasi sistem AI.  
   - Tujuannya adalah mendeteksi potensi *bottleneck* atau penurunan performa AI saat terjadi lonjakan penggunaan.

2. **Kepuasan Pengguna antar Shift dan Jenis Sesi (AI vs Non-AI)**  
   - Menganalisis perbedaan rata-rata skor kepuasan berdasarkan waktu (morning, afternoon, night) dan tipe sesi.  
   - Hasilnya akan membantu mengidentifikasi penurunan kualitas layanan, khususnya pada shift tertentu atau pada sesi berbasis AI.

3. **Distribusi Beban Kerja dan Kinerja Konselor per Shift**  
   - Mengevaluasi keseimbangan antara jumlah konselor aktif dan antrean pengguna di setiap shift.  
   - Analisis ini digunakan untuk mengetahui apakah terdapat ketidakseimbangan beban kerja yang berdampak pada efisiensi layanan dan kepuasan pengguna.

4. **Kualitas dan Konsistensi Data Log Sistem**  
   - Memeriksa kualitas data log untuk mendeteksi *duplicate entries*, *missing values*, dan anomali lainnya.  
   - Langkah ini bertujuan memastikan reliabilitas dataset sebelum digunakan untuk analisis performa dan kepuasan.

### Output yang Diharapkan
- **Visualisasi interaktif** (grafik tren, distribusi, dan korelasi) untuk setiap fokus analisis.  
- **Matriks korelasi** antara variabel traffic, performa AI, dan kepuasan pengguna.  
- **Insight operasional** terkait efisiensi sistem, keseimbangan beban kerja, dan faktor penurunan kepuasan pengguna.  
- **Laporan kualitas data** yang mencakup tingkat kelengkapan, duplikasi, dan konsistensi antar variabel.

---

### 1. EDA (Performa Sistem AI terhadap Lonjakan Pengguna)
**Visualisasi 1**
![EDA1](https://drive.google.com/uc?export=view&id=1QF-KMYg_UU61HusnxKHc2xqxLGWh0LJQ)

**Visualisasi 2**
![EDA2](https://drive.google.com/uc?export=view&id=1YXiu9OMRmILvPzVbxTI65sE0nibGmpdA)


**Interpretasi:**

Grafik menunjukkan tren paralel antara **jumlah sesi pengguna harian (garis biru)** dan **rata-rata waktu respons AI (garis merah)**.  
Secara umum, terdapat dua fase utama:

1. **Periode awal (1–20 September 2025):**  
   Volume sesi cenderung stabil di kisaran 70–95 sesi per hari dengan waktu respons AI sekitar 900–1100 ms.

2. **Periode akhir (setelah 22 September 2025):**  
   Terjadi lonjakan volume pengguna hingga lebih dari 120 sesi per hari, yang diikuti peningkatan waktu respons AI menjadi 1300–1450 ms.  
   Pola ini menunjukkan adanya *load effect* — performa AI menurun ketika traffic meningkat.

Temuan ini konsisten dengan hasil internal yang mencatat **kenaikan volume pengguna sebesar 28%** disertai **peningkatan response time sebesar 35%**, yang mengindikasikan adanya *load effect* ketika traffic tinggi.

**Visualisasi 3**
![EDA3](https://drive.google.com/uc?export=view&id=1acmKmib39FrKyNS_lHrL_hMgdnq3B2Y2)

## Analisis Korelasi Statistik

Nilai korelasi Pearson antara variabel harian menunjukkan:

| Hubungan | Nilai Korelasi | Interpretasi |
|:--|:--:|:--|
| `total_sessions` ↔ `avg_ai_response` | **0.71** | Korelasi positif kuat — semakin banyak sesi, semakin lambat respons AI |
| `total_sessions` ↔ `avg_ai_accuracy` | **0.30** | Korelasi lemah positif — akurasi AI sedikit naik seiring volume |
| `avg_ai_response` ↔ `avg_ai_accuracy` | **0.09** | Hampir tidak berkorelasi — waktu respons dan akurasi AI berjalan independen |

Korelasi positif (0.71) antara volume dan waktu respons menandakan bahwa peningkatan traffic kemungkinan membebani sistem pemrosesan AI,  menyebabkan latensi yang lebih tinggi. Namun, akurasi AI relatif stabil, yang berarti modelnya masih bekerja baik meskipun sistemnya tertekan.

## Insight Utama

- Peningkatan jumlah pengguna di akhir bulan memperlihatkan dampak nyata terhadap kenaikan *AI response time*.  
- Hal ini menunjukkan potensi **bottleneck pada sisi server atau load balancing AI engine**, bukan pada algoritma prediksi itu sendiri.  
- Akurasi AI tetap konsisten, menandakan sistem masih memberikan hasil yang benar, tetapi dengan waktu tanggap lebih lama.

**Kesimpulan sementara:**  
- Peningkatan aktivitas pengguna memang berdampak pada penurunan efisiensi performa AI (latency meningkat 35%),  
sehingga perbaikan kapasitas sistem atau *scaling mechanism* perlu diprioritaskan dalam rencana kuartal berikutnya.

- Analisis ini memvalidasi temuan bahwa peningkatan jumlah pengguna berdampak langsung pada kenaikan waktu respons AI, namun tidak secara signifikan memengaruhi akurasi model. Artinya, permasalahan yang muncul lebih bersifat teknis (server load) daripada kualitas algoritma.


### 2. EDA (Kepuasan Pengguna antar Shift dan Jenis Sesi (AI vs Non-AI))

**Visualisasi 1**
![EDA1](https://drive.google.com/uc?export=view&id=1xXUzKuurarVaIYkstD30WzN8hE0cDc93)

**Visualisasi 2**
![EDA2](https://drive.google.com/uc?export=view&id=1uw8L_kEP18KeqNxvLN0xUT-suJ1d4lnE)

## Analisis Rata-rata Kepuasan Pengguna per Shift (AI vs Non-AI)

Visualisasi di atas menunjukkan perbandingan skor kepuasan pengguna antar shift waktu dan jenis sesi (AI vs Non-AI).  
Dari hasil pengamatan:

- **Afternoon Shift** memiliki rata-rata kepuasan tertinggi (~4.05 untuk Non-AI, ~3.6 untuk AI).  
- **Morning Shift** berada sedikit di bawahnya, dengan pola serupa.  
- **Night Shift** memperlihatkan penurunan yang paling nyata —  
  skor Non-AI sekitar 3.9, sementara AI hanya **3.4–3.5**.

Dengan demikian, perbedaan antara *AI sessions* dan *Non-AI sessions* menjadi semakin signifikan di shift malam.  
Selisih skor kepuasan pengguna malam terhadap siang hari mencapai sekitar **15%**,  
sesuai dengan temuan internal bahwa kepuasan pengguna malam hari menurun terutama pada sesi yang melibatkan AI.

**Interpretasi:**

- Skor kepuasan pengguna AI selalu lebih rendah dibanding Non-AI di seluruh shift,  
  menandakan bahwa **pengalaman pengguna dengan AI masih di bawah konselor manusia.**
- Gap paling besar terjadi pada **shift malam**, di mana performa sistem AI sebelumnya terbukti melambat.  
- Pola ini memperkuat indikasi bahwa **waktu respons AI yang tinggi** berdampak langsung pada penurunan kepuasan malam hari.

Dengan kata lain, bukan perilaku pengguna atau faktor konselor yang menyebabkan kepuasan menurun,  
melainkan **degradasi performa sistem AI di periode malam (lateness dan response delay).**

### 3. EDA (Distribusi Beban Kerja dan Kinerja Konselor per Shift)

**Visualisasi 1**
![EDA1](https://drive.google.com/uc?export=view&id=1RfMLrQFpNG8Div3x8B5rj9wYs5Lfr7uk)

**Visualisasi 2**
![EDA1](https://drive.google.com/uc?export=view&id=1GlNRDGg026AttJ_RYKKlTlk1vBz8X7em)

## Analisis Distribusi Konselor dan Antrean Pengguna per Shift

Visualisasi menunjukkan perbandingan rata-rata **jumlah konselor aktif** dan **panjang antrean pengguna** pada tiga kategori shift waktu: *Morning*, *Afternoon*, dan *Night*.

Dari hasil pengamatan:

- **Morning Shift** memiliki jumlah konselor aktif tertinggi (sekitar **4 konselor per jam**) dan panjang antrean terendah (**~0.9 antrean**).  
- **Afternoon Shift** menurun sedikit dalam jumlah konselor (sekitar **3 orang**) dengan antrean rata-rata **~1.3 pengguna**.  
- **Night Shift** menunjukkan ketidakseimbangan paling nyata — jumlah konselor aktif menurun menjadi **~2 orang**, sedangkan antrean pengguna meningkat hingga **>2 orang per jam**.  

Pola ini menegaskan adanya **distribusi sumber daya yang tidak merata antar shift**, di mana periode malam mengalami *overload* sementara periode pagi relatif *idle*.


**Visualisasi 3**
![EDA1](https://drive.google.com/uc?export=view&id=1l_Tf6P_4KCnt0c7xfB-fYMBK_XTiqSSi)

## Analisis Korelasi antara Jumlah Konselor, Panjang Antrean, dan Kepuasan Pengguna

Matriks korelasi Pearson menunjukkan hubungan antar variabel operasional utama:

| Hubungan | Nilai Korelasi | Interpretasi |
|-----------|----------------|---------------|
| `counselors_on_shift` ↔ `counselor_queue_length` | **-0.54** | Korelasi negatif sedang — semakin banyak konselor aktif, semakin pendek antrean pengguna. |
| `counselors_on_shift` ↔ `user_satisfaction_score` | **+0.18** | Korelasi lemah positif — kehadiran lebih banyak konselor cenderung meningkatkan kepuasan pengguna. |
| `counselor_queue_length` ↔ `user_satisfaction_score` | **-0.27** | Korelasi negatif — semakin panjang antrean, semakin rendah skor kepuasan pengguna. |

## Interpretasi Analitis

Temuan ini menunjukkan bahwa **performa layanan manusia (counselor system)** juga menjadi faktor penentu pengalaman pengguna.  
- Ketidakseimbangan jumlah konselor antar shift berkontribusi langsung terhadap peningkatan antrean di malam hari.  
- Panjang antrean yang tinggi berdampak negatif pada kepuasan pengguna, meskipun sesi tidak selalu melibatkan AI.  
- Pola korelasi memperkuat bahwa **ketika jumlah konselor aktif menurun, antrean naik, dan kepuasan menurun** — sesuai laporan internal bahwa beberapa shift *overload* sementara lainnya *idle*.

## Insight Utama

Distribusi tenaga konselor belum seimbang antar shift.  
Pada **shift malam**, kekurangan jumlah konselor menyebabkan **panjang antrean meningkat lebih dari dua kali lipat**, yang berdampak pada penurunan kepuasan pengguna.  

Hal ini menunjukkan perlunya:
- **Penjadwalan dinamis** berbasis volume sesi historis,  
- **Redistribusi tenaga konselor** ke shift malam atau jam sibuk, dan  
- **Implementasi sistem auto-allocation** atau *predictive staffing* untuk menyeimbangkan beban kerja antar periode waktu.  

Dengan perbaikan ini, sistem dapat menjaga **stabilitas waktu tanggap layanan** serta meningkatkan **kepuasan pengguna secara konsisten di seluruh shift**.

**Kesimpulan:**  
Analisis ini berhasil membuktikan secara *data-driven* bahwa **ketidakseimbangan tenaga konselor** adalah penyebab utama antrean panjang dan turunnya kepuasan malam hari, bukan hanya faktor AI.

### 4. EDA (Kualitas dan Konsistensi Data Log Sistem)

**Visualisasi 1**
![EDA1](https://drive.google.com/uc?export=view&id=18rOgCR6YGfIXVdS1iqWMWWb4dsR0nDN1)

**Visualisasi 2**
![EDA2](https://drive.google.com/uc?export=view&id=1GA3vgW8GBrME7QWaQxgD5y5X7-foDzBc)

```python
=== Analisis Duplikasi Data ===
Jumlah session_id duplikat: 0
Jumlah baris dengan duplicate_flag=True: 84


=== Analisis Missing Entries ===
Jumlah baris dengan missing_report_flag=True: 106

Jumlah nilai hilang pada data missing_report_flag=True:
ai_response_time_ms    26
ai_accuracy            26

=== RINGKASAN ===
Total baris data: 2666
Duplikasi terdeteksi: 84 baris (3.15%)
Missing entries terdeteksi: 106 baris (3.98%)
Total data bermasalah (duplikasi + missing): 7.13% dari total dataset
```

**Visualisasi 3**
![EDA2](https://drive.google.com/uc?export=view&id=1e_MrT-p7sw9XgcKNyYPQL4VTsAnfex_7)

## Analisis Kualitas Data Log (Duplikasi & Missing Entries)

### Ringkasan Visualisasi
Visualisasi menampilkan distribusi **anomali data log** yang meliputi:
1. Jumlah **duplikasi per tanggal**, dan  
2. Jumlah **missing entries** (*laporan yang tidak lengkap*) per tanggal.

---

## Temuan Utama

### Duplikasi Data
- Terdapat **84 baris duplikat (~3.15%)** dari total 2.666 entri data.  
- Lonjakan duplikasi terjadi pada **minggu keempat September (23–29 September 2025)** — bersamaan dengan peningkatan volume pengguna.  
- Pola ini menandakan bahwa **sistem logging melakukan *retry event*** ketika beban server tinggi, menyebabkan satu sesi tercatat dua kali dengan selisih waktu kecil.

### Missing Entries
- Terdapat **106 baris (~3.98%)** dengan `missing_report_flag=True`.  
- Kolom yang paling sering tidak lengkap: **`ai_response_time_ms`** dan **`ai_accuracy`**.  
- Polanya meningkat di akhir bulan — menunjukkan **gagal simpan log saat sistem padat** sehingga sebagian laporan tidak tercatat penuh.

---

## Korelasi dengan Variabel Operasional

| Variabel | Rata-rata Data Normal | Rata-rata Data Anomali | Perbedaan |
|-----------|-----------------------|------------------------|------------|
| `ai_response_time_ms` | 1153 ms | 1168 ms | +15 ms |
| `ai_accuracy` | 0.79 | 0.78 | -0.01 |

- Korelasi antara `ai_response_time_ms` dan `user_satisfaction_score` = **-0.20** → latensi tinggi menurunkan kepuasan pengguna.  
- Korelasi antara flag duplikasi dan missing = **-0.04** → keduanya **tidak terjadi bersamaan**, melainkan terpisah.

---

## Estimasi Dampak
Sekitar **7.1% data log terdeteksi bermasalah (duplikasi + missing)**.  
Masalah ini **bukan akibat human error**, melainkan **beban sistem tinggi** yang memengaruhi stabilitas proses logging.

---

## Insight Utama
Sistem pencatatan data memerlukan penguatan untuk menjaga **reliabilitas metrik performa**.  
Disarankan langkah berikut:

- **Server-side logging validation** → mencegah duplikasi akibat *retry event*.  
- **Monitoring otomatis** → memicu alert jika >2% data harian bermasalah.  
- **Pipeline data cleansing** → membersihkan duplikasi & missing sebelum analisis KPI.  

Dengan perbaikan tersebut, sistem dapat menjaga:
- Keakuratan insight bisnis 
- Konsistensi evaluasi performa AI & tim konselor 

---

**Kesimpulan:**  
- Sekitar 7.1% data log mengandung anomali yang terutama disebabkan oleh tekanan sistem saat traffic tinggi.  
- Penguatan proses logging dan validasi otomatis diperlukan agar metrik performa tetap akurat dan andal di semua periode operasional.


# Penjelasan Metode, Asumsi, dan Logika Interpretasi

---

## 1. Metode Analisis
Analisis dilakukan menggunakan pendekatan **Exploratory Data Analysis (EDA)** dengan kombinasi metode **deskriptif dan statistik** sebagai berikut:

- **Analisis tren** → menelusuri perubahan volume pengguna dan waktu respons AI per tanggal.  
- **Korelasi Pearson** → mengukur kekuatan hubungan antar variabel utama (jumlah sesi, latency, kepuasan pengguna).  
- **Analisis perbandingan rata-rata** → membandingkan hasil antar *shift* dan jenis sesi (AI vs Non-AI).  
- **Heatmap dan matriks korelasi** → mendeteksi keterkaitan antar faktor operasional seperti antrean, jumlah konselor, dan skor kepuasan.

Pendekatan ini dipilih karena fokus studi bersifat **diagnostik** — mencari **pola penyebab menurunnya performa sistem** secara data-driven.

---

## 2. Logika Asumsi
Agar hasil analisis tetap realistis dan kontekstual dengan lingkungan operasional, beberapa asumsi digunakan:

| Aspek | Asumsi yang Ditetapkan |
|-------|-------------------------|
| **Pembagian Shift** | Morning (06–13), Afternoon (14–19), Night (20–05) |
| **Ambang Latency Tinggi** | > **1200 ms** (mengacu pada SLA internal AI system) |
| **Periode Analisis** | 1–30 September 2025 (representatif untuk tren bulanan) |
| **Flag Anomali Sistem** | `duplicate_flag` & `missing_report_flag` → indikator error sistem, bukan human input |

Asumsi ini disusun untuk menyederhanakan model analisis **tanpa mengubah makna atau integritas data**.

---

## 3. Alasan di Balik Interpretasi Data
Setiap insight disusun dengan mempertimbangkan **kombinasi bukti statistik dan konteks operasional**:

| Pola Data | Interpretasi Analitis |
|------------|-----------------------|
| Korelasi positif kuat (**r = 0.71**) antara jumlah sesi dan waktu respons | Beban sistem meningkat saat traffic tinggi |
| Penurunan kepuasan malam hari beriringan dengan peningkatan latency AI | Performa teknis AI berdampak langsung pada pengalaman pengguna |
| Korelasi negatif antara jumlah konselor dan panjang antrean (**r = –0.54**) | Ketidakseimbangan sumber daya manusia antar shift |
| Lonjakan duplikasi & missing log saat traffic tinggi | Indikasi kegagalan sistem logging saat *server overload* |

Interpretasi ini menghubungkan **indikator teknis (AI, server, konselor)** dengan **dampak bisnis (kepuasan pengguna, reliabilitas data)** secara **objektif, terukur, dan kontekstual**.

---

# Rekomendasi Berbasis Data untuk Optimalisasi Sistem dan Operasional

Berdasarkan hasil eksplorasi data (EDA 1–4), ditemukan beberapa pola signifikan yang memengaruhi **performa sistem**, **kepuasan pengguna**, dan **efisiensi operasional tim konselor**.  
Bagian ini merangkum **insight utama** beserta **rekomendasi strategis** yang disusun berdasarkan tingkat prioritas implementasi.

---

## Ringkasan Temuan Utama

| No | Area Analisis | Insight Utama | Dampak Terhadap Sistem |
|----|----------------|----------------|-------------------------|
| **1** | **Performa Sistem AI (EDA 1)** | Volume pengguna naik **28%** disertai peningkatan waktu respons **+35%**. Korelasi kuat (**r = 0.71**) antara jumlah sesi dan waktu respons menandakan adanya **bottleneck server**. | Latency meningkat signifikan di akhir bulan, menurunkan efisiensi dan pengalaman pengguna. |
| **2** | **Kepuasan Pengguna (EDA 2)** | Kepuasan pengguna **AI sessions** menurun hingga **15% pada shift malam**, sedangkan Non-AI relatif stabil. | Pengguna mengalami penurunan pengalaman saat AI melambat, terutama di jam malam. |
| **3** | **Distribusi Konselor & Antrean (EDA 3)** | **Ketidakseimbangan shift**: malam kekurangan konselor (~2 orang), antrean naik >2 pengguna/jam. Korelasi negatif **r = -0.54** antara jumlah konselor dan panjang antrean. | Antrean tinggi menurunkan kepuasan pengguna dan menyebabkan *overload* di periode malam. |
| **4** | **Kualitas Data Log (EDA 4)** | Sekitar **7.1% data log bermasalah** (duplikasi + missing). Lonjakan terjadi saat traffic tinggi. | Data performa berpotensi bias, mengurangi reliabilitas pelaporan KPI dan evaluasi performa AI. |

---

## Rekomendasi Strategis Berbasis Data

| Prioritas | Rekomendasi | Fokus Perbaikan | Dampak | Horizon Waktu |
|------------|--------------|----------------|--------|----------------|
| **High** | **Optimasi sistem AI response time** melalui *load balancing*, *asynchronous queue*, dan peningkatan kapasitas server. | Proses Teknis | Mengurangi latency hingga **30–40%** saat traffic tinggi. | ≤ 1 bulan |
| **High** | **Redistribusi tenaga konselor berbasis pola traffic historis** (*dynamic shift scheduling*). | Alokasi SDM | Menurunkan panjang antrean malam hari >50%, meningkatkan kepuasan malam hari (+0.3 poin). | ≤ 1 bulan |
| **Medium** | **Implementasi sistem monitoring & alert real-time** untuk AI latency, antrean, dan kepuasan per shift. | Proses Operasional | Deteksi dini terhadap *overload* atau penurunan performa AI/konselor. | 1–2 bulan |
| **Medium** | **Perkuat sistem logging** dengan *server-side validation* & auto-flag duplikasi/missing (>2% trigger alert). | Kualitas Data | Meningkatkan reliabilitas KPI dan evaluasi performa sistem. | 1–2 bulan |
| **Low** | **Evaluasi lanjutan model AI** setelah sistem stabil (*retraining & fine-tuning model*). | Optimalisasi Sistem | Meningkatkan akurasi prediksi jangka panjang tanpa memperburuk waktu respons. | 2–3 bulan |

---

## Fokus Implementasi

### Jangka Pendek (≤ 1 bulan)
- Perkuat infrastruktur server AI untuk mengurangi latency.  
- Atur ulang jadwal konselor agar beban antar shift lebih merata.  

### Jangka Menengah (1–2 bulan)
- Pasang dashboard monitoring performa & logging alert otomatis.  
- Bangun sistem validasi log untuk mencegah duplikasi dan *missing entries*.  

### Jangka Panjang (≥ 3 bulan)
- Lakukan evaluasi performa model AI setelah sistem stabil.  
- Rancang pengembangan prediktif untuk peningkatan kepuasan jangka panjang.  

---

## Kesimpulan

Analisis data menunjukkan bahwa **penurunan performa sistem dan kepuasan pengguna bukan disebabkan oleh algoritma AI yang buruk**,  
melainkan oleh **beban sistem** dan **ketidakseimbangan sumber daya manusia** antar shift.

Dengan implementasi rekomendasi ini, perusahaan dapat mencapai:
- **Stabilitas performa AI di seluruh shift waktu**,  
- **Distribusi kerja konselor yang lebih optimal**, dan  
- **Kualitas data log yang lebih andal untuk evaluasi KPI.**

**Hasil akhir:** Sistem layanan menjadi **lebih responsif, efisien, dan berorientasi pada pengalaman pengguna.**


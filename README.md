# MindGuard UB
**Sistem Cerdas Deteksi Dini dan Dukungan Kesehatan Mental Mahasiswa Berbasis AI**

Dikembangkan untuk AI & Data Innovation Challenge — Technology & Creative Arena (TEKRA)
Fakultas Ilmu Komputer, Universitas Brawijaya 2026

## Tim

| Nama | NIM | Peran |
|------|-----|-------|
| Affan Abyarahman | 245150207111020 | Ketua |
| Athaya Ra'uf Al-Albani | 245150200111033 | Anggota |
| Alif Negifta Wibawaputra | 245150200111066 | Anggota |

---

## Latar Belakang

Lebih dari 60% mahasiswa di berbagai perguruan tinggi dilaporkan mengalami gejala depresi dan kecemasan, namun hanya sebagian kecil yang mencari bantuan profesional. Di Universitas Brawijaya, tantangan ini diperparah oleh rasio konselor yang tidak proporsional, tidak adanya sistem monitoring terintegrasi, dan stigma sosial dalam mencari bantuan psikologis.

MindGuard UB dikembangkan sebagai solusi ekosistem digital terintegrasi yang mendeteksi, memonitor, dan merespons kondisi kesehatan mental mahasiswa secara proaktif berbasis AI dan data.

---

## Konsep Solusi

MindGuard UB terdiri dari empat output utama yang saling melengkapi.

**AI Chatbot MindGuard** merupakan antarmuka percakapan berbasis mobile yang dapat diakses mahasiswa 24/7 untuk check-in emosional harian dan konseling awal. Chatbot dibangun menggunakan IndoBERT fine-tuned untuk memahami Bahasa Indonesia informal, slang, dan code-switching, serta secara otomatis melakukan routing ke konselor manusia ketika risiko Level 2 atau Level 3 terdeteksi.

**Dashboard Smart Campus Mental Health** menyajikan visualisasi interaktif yang menampilkan distribusi kondisi mental per fakultas, tren waktu, faktor risiko dominan, dan efektivitas intervensi. Dashboard ini digunakan oleh konselor dan pimpinan akademik sebagai dasar pengambilan keputusan berbasis data.

**Early Warning System (EWS)** merupakan sistem notifikasi otomatis bertingkat tiga level kepada konselor ketika algoritma mendeteksi mahasiswa dengan skor risiko tinggi.

| Level | Trigger | Respons |
|-------|---------|---------|
| Level 1 — Perhatian | Risk score > 0.60 selama 3 hari berturut-turut | Notifikasi ke konselor |
| Level 2 — Waspada | Risk score > 0.75 atau deteksi ideasi bunuh diri | Alert prioritas tinggi |
| Level 3 — Kritis | Deteksi kata kunci krisis dengan confidence >= 0.90 | Respons darurat instan |

**Laporan Analitik dan Rekomendasi Kebijakan** berupa laporan periodik mingguan dan bulanan yang merangkum insight dari data agregat, tren musiman seperti lonjakan stres saat UAS, serta rekomendasi berbasis data untuk pengembangan program kesehatan mental kampus.

---

## Pendekatan AI / Data

| Komponen | Metode | Fungsi |
|----------|--------|--------|
| Klasifikasi Kondisi Mental | MultiOutputClassifier (Logistic Regression, Random Forest, Naive Bayes, SVM) | Klasifikasi multi-label: Depression, Anxiety, Panic Attack, Seek Treatment |
| Risk Scoring | Rule-based (Low / Medium / High) | Penentuan tingkat risiko berdasarkan jumlah kondisi yang dialami |
| Clustering Profil Risiko | K-Means dan K-Medoids (k=3) | Pengelompokan mahasiswa berdasarkan profil demografis dan kondisi mental |
| NLP Chatbot | IndoBERT fine-tuned | Pemahaman teks Bahasa Indonesia, deteksi sentimen dan risiko dalam percakapan |
| Early Warning System | Rule-based threshold + ML score fusion | Pembangkitan alert bertingkat kepada konselor |

---

## Alur Analisis

### Dataset

Dataset yang digunakan adalah **Student Mental Health Dataset** yang memuat atribut demografis dan kondisi kesehatan mental mahasiswa. Setelah preprocessing, dataset memiliki fitur berikut:

| Fitur | Tipe | Keterangan |
|-------|------|------------|
| Gender | Kategorikal | Jenis kelamin mahasiswa |
| Age | Numerik | Usia mahasiswa |
| Course | Kategorikal | Program studi |
| Year | Ordinal | Tahun studi (Year 1–4) |
| CGPA | Ordinal | Indeks prestasi kumulatif (0–4.00) |
| Marital_Status | Biner | Status pernikahan |
| Depression | Biner (target) | Ya / Tidak |
| Anxiety | Biner (target) | Ya / Tidak |
| Panic_Attack | Biner (target) | Ya / Tidak |
| Seek_Treatment | Biner (target) | Ya / Tidak |

---

### 1. Data Collection & Loading

Data dimuat dari repositori publik GitHub dalam format CSV:

```python
url = 'https://raw.githubusercontent.com/alifnw/data-vault/refs/heads/main/Student%20Mental%20health.csv'
df = pd.read_csv(url)
```

---

### 2. Data Cleaning & Preprocessing

Tahap ini memastikan data bersih dan konsisten sebelum dianalisis:

- **Penghapusan kolom tidak relevan**: Kolom `Timestamp` dihapus karena tidak digunakan dalam pemodelan.
- **Stripping whitespace**: Seluruh kolom bertipe string dibersihkan dari spasi di awal dan akhir nilai.
- **Standarisasi nilai**: Nilai pada kolom `Course` dan `Year` distandarisasi untuk menghilangkan variasi penulisan (misalnya `koe` → `KOE`, `Irkhs` → `KIRKHS`, `engine` → `Engineering`).
- **Imputasi missing value**: Nilai kosong pada kolom `Age` diisi menggunakan nilai median.
- **Penghapusan duplikat**: Data duplikat diidentifikasi dan dievaluasi.

---

### 3. Exploratory Data Analysis (EDA)

EDA dilakukan secara bertahap dalam tiga tingkatan analisis:

#### a. Univariate Analysis
- **Distribusi Usia**: Ditampilkan menggunakan histogram dan boxplot untuk mengetahui sebaran dan outlier usia mahasiswa.
- **Distribusi Gender**: Ditampilkan menggunakan countplot dan pie chart untuk melihat proporsi gender.
- **Distribusi Tahun Studi & CGPA**: Ditampilkan menggunakan countplot untuk melihat komposisi angkatan dan rentang nilai akademik.
- **Prevalensi Kondisi Mental**: Jumlah dan persentase mahasiswa yang mengalami Depression, Anxiety, Panic Attack, dan yang mencari pertolongan (Seek Treatment) divisualisasikan dalam bar chart.
- **10 Jurusan Terbanyak**: Ditampilkan menggunakan horizontal bar chart.
- **Distribusi Label Target**: Keempat label target divisualisasikan untuk memahami ketidakseimbangan kelas (class imbalance), disertai matriks ko-okorensi label dan heatmap korelasi antar label.

#### b. Bivariate Analysis
- **Kondisi Mental berdasarkan Gender**: Countplot dan stacked bar chart persentase untuk Depression, Anxiety, dan Panic Attack per gender.
- **Uji Chi-Square (Gender vs. Kondisi Mental)**: Uji statistik untuk mengetahui ada tidaknya hubungan signifikan antara gender dan masing-masing kondisi mental.
- **Kondisi Mental berdasarkan Tahun Studi**: Bar chart distribusi kondisi mental per angkatan.
- **Kondisi Mental berdasarkan CGPA**: Bar chart distribusi kondisi mental per rentang CGPA.
- **Seek Treatment berdasarkan Kondisi Mental**: Visualisasi hubungan antara kondisi mental yang dialami dengan keputusan mencari pertolongan profesional.

#### c. Multivariate Analysis
- **Heatmap Korelasi**: Korelasi antar keempat kondisi mental (Depression, Anxiety, Panic Attack, Seek Treatment) setelah encoding biner, divisualisasikan menggunakan heatmap.

---

### 4. Feature Engineering

Sebelum pemodelan, dilakukan rekayasa fitur dan transformasi data:

- **Encoding biner**: Kolom Gender dan Marital_Status diubah menjadi nilai numerik (0/1).
- **Encoding ordinal**: Kolom Year (Year 1–4) dan CGPA (0–1.99 s.d. 3.50–4.00) diubah menjadi nilai ordinal menggunakan `OrdinalEncoder`.
- **Normalisasi**: Kolom `Age` dinormalisasi menggunakan `StandardScaler`.
- **Penghapusan fitur**: Kolom `Course` dihapus dari fitur input klasifikasi karena kardinalitas tinggi.
- **Risk Score**: Fitur baru diturunkan secara rule-based berdasarkan jumlah kondisi mental yang dialami:

```
0 kondisi → Low
1 kondisi → Medium
2 atau 3 kondisi → High
```

- **Treatment Gap**: Fitur biner yang menandai mahasiswa berisiko (Medium/High) namun tidak mencari pertolongan (`Seek_Treatment = No`), digunakan sebagai indikator gap layanan kesehatan mental.

---

### 5. Pemodelan

#### a. Multi-label Classification

Model klasifikasi multi-output digunakan untuk memprediksi empat label target sekaligus: Depression, Anxiety, Panic Attack, dan Seek Treatment.

**Train-Test Split**: 90% data untuk training, 10% untuk pengujian (`random_state=42`).

Empat model dilatih dan dievaluasi:

| Model | Konfigurasi |
|-------|-------------|
| Logistic Regression (Baseline) | `class_weight='balanced'`, `max_iter=1000` |
| Random Forest | `n_estimators=100`, `class_weight='balanced'` |
| Naive Bayes | `BernoulliNB` |
| SVM | `kernel='linear'`, `class_weight='balanced'`, `probability=True` |

**Evaluasi Model** menggunakan:
- Hamming Loss
- F1 Score Macro & Weighted
- Classification Report per label

#### b. Clustering Profil Risiko

Clustering dilakukan untuk mengelompokkan mahasiswa berdasarkan profil demografis dan kondisi mental menggunakan dua set fitur:

- **Demografis saja**: Gender, Age, Year, CGPA, Marital_Status
- **Demografis + Kondisi Mental**: Gender, Age, Year, CGPA, Marital_Status, Depression, Anxiety, Panic_Attack

**Penentuan jumlah klaster optimal** menggunakan Elbow Method dan Silhouette Score pada rentang k = 2–7.

Dua algoritma klasterisasi dijalankan:

| Algoritma | Konfigurasi | Keterangan |
|-----------|-------------|------------|
| K-Means | `k=3`, `random_state=42`, `n_init=10` | Klasterisasi berbasis centroid |
| K-Medoids (PAM) | `k=3`, `random_state=42`, `method='pam'` | Lebih robust terhadap outlier |

**Evaluasi stabilitas klaster** dilakukan menggunakan Adjusted Rand Index (ARI) pada 5 inisialisasi berbeda untuk mengukur konsistensi hasil klasterisasi.

Profil masing-masing klaster dianalisis berdasarkan rata-rata nilai fitur, termasuk prevalensi kondisi mental per klaster, untuk mengidentifikasi kelompok mahasiswa berisiko tinggi.

---

### 6. Output & Integrasi

- Dataset hasil preprocessing dan feature engineering disimpan sebagai `mental_health_cleaned.csv` untuk kebutuhan visualisasi dashboard dan inferensi EWS.
- Hasil risk scoring dan treatment gap digunakan sebagai dasar logika Early Warning System.

---

## Arsitektur Pipeline

```
Data Collection → Preprocessing → EDA → Feature Engineering → AI Modeling → Risk Scoring & EWS → Visualization
```

1. **Data Collection** — Load dataset mahasiswa (CSV), integrasi API SIAM UB, survei PHQ-9/GAD-7, log chatbot
2. **Preprocessing** — Cleaning, standarisasi nilai, imputasi missing value, penghapusan duplikat
3. **EDA** — Analisis univariat, bivariat, multivariat untuk memahami pola dan distribusi data
4. **Feature Engineering** — Encoding, normalisasi, derivasi Risk Score dan Treatment Gap
5. **AI Modeling** — Training model klasifikasi multi-label dan klasterisasi profil risiko
6. **Risk Scoring & EWS** — Penghitungan composite risk score dan pembangkitan alert bertingkat
7. **Visualization** — Dashboard interaktif, laporan otomatis, notifikasi konselor

---

## Struktur Repository

```
mindguard-ub/
├── README.md
└── MindGuard_UB.ipynb
```

---

## Referensi

- Malgaroli et al. (2023). Natural language processing for mental health interventions. *Translational Psychiatry*, 13, 309.
- Mawar et al. (2025). An Early Warning Framework for Mental Health Crises. *Antigen: Jurnal Kesehatan Masyarakat*, 3(4), 119–138.
- Pendi & Sulistiani (2026). Klasifikasi Kesehatan Mental Menggunakan SVM. *BITS*, 7(4), 2312–2321.
- Simanjuntak et al. (2025). Dashboard Visualisasi Data Kesehatan Mental. *JIKTI*, 2(2), 97–108.
- Sutrisno et al. (2026). Efektivitas Chatbot AI vs Konselor Manusia. *JITET*, 14(1), 1427–1438.

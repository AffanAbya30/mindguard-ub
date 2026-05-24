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

## Latar Belakang

Lebih dari 60% mahasiswa di berbagai perguruan tinggi dilaporkan mengalami gejala depresi dan kecemasan, namun hanya sebagian kecil yang mencari bantuan profesional. Di Universitas Brawijaya, tantangan ini diperparah oleh rasio konselor yang tidak proporsional, tidak adanya sistem monitoring terintegrasi, dan stigma sosial dalam mencari bantuan psikologis.

MindGuard UB dikembangkan sebagai solusi ekosistem digital terintegrasi yang mendeteksi, memonitor, dan merespons kondisi kesehatan mental mahasiswa secara proaktif berbasis AI dan data.

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

## Pendekatan AI / Data

| Komponen | Metode | Fungsi |
|----------|--------|--------|
| Klasifikasi Kondisi Mental | MultiOutputClassifier (SVM, Random Forest, Logistic Regression, Naive Bayes) | Klasifikasi multi-label: Depression, Anxiety, Panic Attack |
| Risk Scoring | Rule-based (Low / Medium / High) | Penentuan tingkat risiko berdasarkan jumlah kondisi yang dialami |
| Clustering Profil Risiko | K-Means dan DBSCAN | Pengelompokan mahasiswa berdasarkan profil demografis dan kondisi mental (K-Means k=3 dan DBSCAN density-based) |
| NLP Chatbot | IndoBERT fine-tuned | Pemahaman teks Bahasa Indonesia, deteksi sentimen dan risiko dalam percakapan |
| Early Warning System | Rule-based threshold + ML score fusion | Pembangkitan alert bertingkat kepada konselor |
| Forecasting | LSTM, Prophet | Peramalan tren stres dan depresi per semester |
| Recommendation | Collaborative Filtering + Content-Based | Rekomendasi sumber daya personal (artikel, mindfulness, jadwal konseling) |

## Arsitektur Pipeline

```
Data Collection → Preprocessing → Feature Engineering → AI Modeling → Inference & EWS → Visualization
```

1. **Data Collection** — Integrasi API dengan SIAM UB, survei PHQ-9/GAD-7 berkala, log chatbot
2. **Preprocessing** — Cleaning, imputasi missing value, normalisasi, encoding kategorikal
3. **Feature Engineering** — Total Pressure Score, Linguistic Risk Index, Behavioral Change Index
4. **AI Modeling** — Training model klasifikasi, clustering, NLP, dan forecasting
5. **Inference & EWS** — Real-time scoring dan fusi skor menjadi composite risk score
6. **Visualization** — Dashboard interaktif, laporan otomatis, notifikasi konselor

## Struktur Repository

```
mindguard-ub/
├── README.md
└── MindGuard_UB.ipynb
```

*Notebook ini telah selesai dikembangkan dan divalidasi end-to-end dengan pipeline ML bebas kebocoran data serta model clustering K-Means dan DBSCAN.*

## Referensi

- Malgaroli et al. (2023). Natural language processing for mental health interventions. *Translational Psychiatry*, 13, 309.
- Mawar et al. (2025). An Early Warning Framework for Mental Health Crises. *Antigen: Jurnal Kesehatan Masyarakat*, 3(4), 119–138.
- Pendi & Sulistiani (2026). Klasifikasi Kesehatan Mental Menggunakan SVM. *BITS*, 7(4), 2312–2321.
- Simanjuntak et al. (2025). Dashboard Visualisasi Data Kesehatan Mental. *JIKTI*, 2(2), 97–108.
- Sutrisno et al. (2026). Efektivitas Chatbot AI vs Konselor Manusia. *JITET*, 14(1), 1427–1438.

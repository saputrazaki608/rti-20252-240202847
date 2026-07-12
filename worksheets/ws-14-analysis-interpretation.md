# WS-14: Analysis, Interpretation & Failure Analysis

> **Bab 14 — Analisis Data, Interpretasi & Failure Analysis**

---

## Ringkasan Materi

### Data → Knowledge Model

```
Data → Analysis → Interpretation → Explanation → Knowledge
```

Tiga level yang berbeda:
- **Analysis** — "Apa yang terjadi?" (deskriptif + inferensial)
- **Interpretation** — "Apa artinya?" (konteks RQ + literatur)
- **Failure Analysis** — "Mengapa tidak berhasil?" (boundary conditions)

### Beyond p-value

**Statistical significance ≠ practical significance.** Selalu laporkan:
1. p-value (signifikansi statistik)
2. Effect size (besarnya efek)
3. Confidence interval (rentang ketidakpastian)

| Effect Size (Cohen's d) | Interpretasi |
|-------------------------|-------------|
| < 0.2 | Small |
| 0.2 – 0.8 | Medium |
| > 0.8 | Large |

### Pemilihan Uji Statistik

| Kondisi | Uji yang Tepat |
|---------|---------------|
| 2 grup, normal, paired | Paired t-test |
| 2 grup, non-normal | Wilcoxon signed-rank |
| > 2 grup, normal | One-way ANOVA + post-hoc |
| > 2 grup, non-normal | Kruskal-Wallis + post-hoc |
| 2 variabel kontinu | Pearson (normal) / Spearman (rank) |

### Failure Analysis as Contribution

Hipotesis yang ditolak adalah **temuan yang berharga**:

| Dataset | New (F1) | Baseline (F1) | p-value | Cohen's d |
|---------|---------|--------------|---------|-----------|
| DS-1 (small, clean) | 94.2±1.1 | 89.3±1.5 | <0.001 | **3.7** |
| DS-4 (medium, noisy) | 78.3±3.2 | 82.1±2.8 | 0.008 | **-1.3** |
| DS-5 (large, noisy) | 71.6±4.1 | 80.5±3.0 | <0.001 | **-2.5** |

**Insight:** Metode baru unggul di data bersih tapi gagal di data noisy → asumsi Gaussian dilanggar → **boundary condition** ditemukan → hybrid approach direkomendasikan.

**Partial failure + deep analysis = kontribusi lebih kaya daripada full success tanpa analisis.**

### Limitation Types

| Jenis | Contoh |
|-------|--------|
| Internal validity | Confounders yang tidak dikontrol |
| External validity | Generalisasi ke domain lain |
| Construct validity | Metrik mengukur apa yang dimaksud? |
| Statistical limitation | Sample size, asumsi distribusi |

### Jebakan Kognitif

1. "Signifikan statistik = penting secara praktis" → cek effect size
2. "Hipotesis tidak didukung → cari sudut baru" → p-hacking
3. "Kegagalan tidak perlu dilaporkan detail" → missed insight
4. "Limitasi cukup disebutkan, tidak perlu dianalisis" → kedalaman hilang

---

## Template A.14 — Analysis & Interpretation Report

```
ANALYSIS & INTERPRETATION

1. Statistik Deskriptif:
   | Skenario | Mean | Std | Median | Min | Max | n |
   |----------|------|-----|--------|-----|-----|---|
   |          |      |     |        |     |     |   |

2. Uji Hipotesis:
   Uji yang digunakan  : ____________________
   Justifikasi          : ____________________
   Hasil: p = ____, effect size (d/r/η²) = ____
   CI 95%               : [____, ____]

3. Keputusan:
   [ ] H₀ ditolak → H₁ diterima
   [ ] H₀ tidak ditolak

4. Interpretasi:
   Hubungan ke RQ       : ____________________
   Practical significance: ____________________
   Perbandingan literatur: ____________________

5. Limitation:
   | Jenis | Ancaman | Dampak | Mitigasi |
   |-------|---------|--------|----------|
   |       |         |        |          |

6. Failure Analysis (jika H₀ tidak ditolak):
   Penyebab potensial  : ____________________
   Boundary condition   : ____________________
   Insight              : ____________________
```

---

## Latihan 1 — Pemilihan Uji Statistik

Tentukan uji statistik yang tepat untuk eksperimen Anda.

| Pertanyaan | Jawaban |
|-----------|---------|
| Berapa grup yang dibandingkan? | *3 grup (Proposed Lightweight CNN, Baseline Hybrid CNN-LSTM, Baseline Standard 1D-CNN)* |
| Apakah data berpasangan (paired)? | *Tidak (Independent, karena variasi performa dievaluasi dari run acak dengan seed yang independen pada arsitektur terpisah)* |
| Apakah distribusi normal? (uji normalitas) | *Tidak terdistribusi normal (Karakteristik performa metrik dari data log paket jaringan real-time umumnya memiliki sebaran skewed)* |
| **Uji yang dipilih:** | *Kruskal-Wallis Test (Uji non-parametrik untuk >2 grup independen)* |
| **Justifikasi:** | *Eksperimen membandingkan tiga arsitektur model berbeda berskala multivariat di mana asumsi distribusi normal pada populasi skor run tidak terpenuhi (non-parametric)._* |

**Effect size yang akan dilaporkan:** [ ] Cohen's d / [v] Eta-squared / [ ] Lainnya: ____

---

## Latihan 2 — Interpretasi Hasil

Gunakan data berikut (atau data riil Anda) untuk berlatih interpretasi.

**Data:**
| Model | Accuracy (mean ± std) | n |
|-------|----------------------|---|
| A | 89.2 ± 1.5 | 10 |
| B | 87.8 ± 2.1 | 10 |

p = 0.045, Cohen's d = 0.74, CI 95% = [0.03, 2.77]

| Aspek | Interpretasi |
|-------|-------------|
| Signifikansi statistik | *p = 0.045 --> Signifikan secara statistik pada tingkat a = 0.05 karena nilai p < 0.05. Ada perbedaan nyata antara performa akurasi Model A dan Model B.* |
| Effect size | *d = 0.74 --> Medium-to-large effect. Menunjukkan bahwa intervensi perubahan struktur arsitektur memberikan efek kekuatan perbedaan yang substansial pada hasil akhir akurasi.* |
| Practical significance | *Selisih nilai rata-rata akurasi secara riil adalah 1.4% (89.2% vs 87.8%). Dalam konteks deteksi serangan siber skala enterprise dengan jutaan paket log per detik, peningkatan $1.4\%$ ini sangat krusial karena berhasil mereduksi ribuan potensi ancaman lolos (false negatives).* |
| Hubungan ke RQ | *Menjawab pertanyaan penelitian (Research Question) mengenai efektivitas integrasi komponen baru terhadap akurasi model; terbukti bahwa modifikasi berhasil meningkatkan performa klasifikasi secara valid.* |
| Perbandingan literatur | *Hasil akurasi Model A sebesar 89.2% ini sejalan bahkan melampaui beberapa penelitian terdahulu yang rata-rata tertahan di angka $86-88% untuk deteksi berbasis arsitektur lightweight.* |

---

## Latihan 3 — Failure Analysis

Latih kemampuan failure analysis: hipotesis TIDAK didukung. Apa yang bisa dipelajari?

**Skenario:** Tabel Evaluasi Skenario Hipotesis Tidak Didukung (F1 = 83.2%, Baseline = 84.7%, p = 0.12)
| Pertanyaan | Jawaban |
|-----------|---------|
| Apakah ini "gagal"? | *Bukan gagal total — Hasil statistik non-signifikan (p > 0.05) merupakan temuan ilmiah yang valid yang menunjukkan bahwa penambahan kompleksitas modul tidak selalu berkorelasi positif pada performa klasifikasi paket.* |
| Kemungkinan penyebab? | *Penambahan lapisan arsitektur baru memicu terjadinya overfitting pada subset data latih atau menyebabkan hilangnya representasi spasial fitur esensial dari log paket akibat reduksi dimensi yang terlalu agresif.* |
| Boundary condition? | *Metode baru ini kurang optimal jika diimplementasikan pada kondisi dataset yang memiliki ketimpangan kelas (class imbalance) sangat ekstrem antar kategori serangan siber.* |
| Insight yang bisa diambil? | *Diperlukan mekanisme penyeimbangan data (data balancing) seperti SMOTE atau weighted loss function terlebih dahulu sebelum menyalurkan fitur ke dalam arsitektur model usulan.* |
| Apakah layak dilaporkan? Mengapa? | *Ya — Laporan negative result ini penting untuk dipublikasikan guna mencegah komunitas peneliti lain melakukan duplikasi riset struktural yang sama dan memberikan batasan batasan teknis baru.* |

**Limitation terkait:**
| Jenis | Ancaman | Dampak |
|-------|---------|--------|
| *Statistical* | *Jumlah pengulangan simulasi terbatas (n=10)* | *Nilai power test statistik berisiko rendah dalam mendeteksi signifikansi margin tipis.* |
| *External Validity* | *Evaluasi hanya menggunakan satu ekosistem dataset (CICIDS2017)* | *Generalisasi model terhadap pola ancaman siber baru atau variasi arsitektur jaringan lain belum teruji penuh.* |

---

## Refleksi

> Apakah "failure" dalam riset benar-benar gagal, atau justru kontribusi? Bagaimana failure analysis mengubah cara Anda melihat hasil negatif?
Hasil negatif atau tidak terdukungnya sebuah hipotesis bukanlah sebuah kegagalan personal dalam riset, melainkan sebuah kontribusi ilmiah yang objektif. Failure analysis mengubah sudut pandang saya dari yang awalnya menganggap hasil negatif sebagai penanda cacatnya sebuah eksperimen, menjadi sebuah fondasi penting untuk memetakan boundary conditions (batas kemampuan) suatu model. Mengetahui dengan pasti di mana, kapan, dan mengapa sebuah arsitektur deep learning tidak berfungsi memberikan kontribusi pengetahuan yang sama berharganya dengan mengetahui aspek keberhasilannya, sekaligus memberikan peta jalan yang jelas untuk perbaikan iterasi riset berikutnya.

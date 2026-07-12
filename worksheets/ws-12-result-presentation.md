# WS-12: Result Presentation & Visualization

> **Bab 12 — Penyajian Hasil & Visualisasi**

---

## Ringkasan Materi

### Data → Insight Model

```
Validated Data → Structured Presentation → Visualization → Pattern Recognition → Insight
```

Penyajian **mendahului** analisis. Tabel dan grafik membantu peneliti "melihat" data sebelum menghitung. Langsung ke uji statistik tanpa visualisasi berisiko kesimpulan yang secara teknis benar tapi kontekstual salah (Anscombe's Quartet, 1973).

### Tabel = Presisi, Grafik = Pola

Keduanya **saling melengkapi**:
- Tabel: angka presisi, self-contained (dipahami tanpa teks), sortable
- Grafik: pola visual, tren, perbandingan cepat

### Jenis Grafik Berdasarkan Tujuan

| Tujuan | Jenis Grafik |
|--------|-------------|
| Perbandingan antar-skenario | Bar chart (grouped/stacked) |
| Distribusi per-skenario | Box plot / violin plot |
| Tren temporal | Line chart |
| Korelasi dua variabel | Scatter plot |
| Proporsi (total = 100%) | Pie chart (hati-hati!) |

### Contoh Tabel Hasil yang Baik

| Model | Accuracy (%) | F1-Score (%) | Training Time (min) |
|-------|-------------|-------------|---------------------|
| BERT | 88.4 ± 1.2 | 87.1 ± 1.4 | 45.2 ± 3.1 |
| LSTM | 86.1 ± 1.8 | 84.5 ± 2.0 | 12.8 ± 1.2 |
| SVM | 82.3 ± 0.9 | 80.7 ± 1.1 | 0.3 ± 0.1 |

*N=10 per model. Mean ± std. Diurutkan berdasarkan Accuracy.*

### Visualization Bias — Yang Harus Dihindari

| Bias | Deskripsi | Dampak |
|------|----------|--------|
| Truncated axis | Y tidak dari 0 | Memperbesar perbedaan kecil |
| Inconsistent scale | Dua grafik skala beda | Perbandingan menyesatkan |
| Cherry-picked data | Hanya tampilkan yang "menang" | Selektif, tidak jujur |
| 3D effects | Efek 3D tanpa dimensi data ke-3 | Distorsi tanpa informasi |
| Missing error bar | Tidak ada variabilitas | Menyembunyikan ketidakpastian |

### Engineering vs Research Presentation

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Tujuan grafik | Dashboard monitoring | Mendukung argumen ilmiah |
| Informasi wajib | KPI, threshold | Mean, std, CI, N, p-value |
| Bias handling | Less critical | Wajib dihindari (peer-review) |

---

## Template A.12 — Result Presentation Plan

```
RESULT PRESENTATION PLAN

Research Question : ____________________
Metrik Utama      : ____________________

Tabel Hasil:
| Skenario | Metrik 1 (mean ± std) | Metrik 2 (mean ± std) | n |
|----------|----------------------|----------------------|---|
|          |                      |                      |   |

Visualisasi yang Direncanakan:
| # | Jenis Grafik | Pesan Utama | Metrik |
|---|-------------|-------------|--------|
| 1 |             |             |        |
| 2 |             |             |        |

Bias Check:
  [ ] Y-axis mulai dari 0 (atau dijustifikasi)
  [ ] Error bar/CI ditampilkan
  [ ] Semua data disertakan (tidak cherry-picked)
  [ ] Tidak menggunakan 3D tanpa alasan
```

---

## Latihan 1 — Tabel Hasil

Buat tabel hasil eksperimen Anda (boleh dengan data simulasi jika belum punya data riil).

| Skenario | Metrik 1 (mean ± std) | Metrik 2 (mean ± std) | n |
|----------|----------------------|----------------------|---|
| *Proposed: Lightweight CNN + Attention* | *87.25 ± -0.45%* | *1.84 ± 0.08ms* | *3* |
| *Baseline 2: Hybrid CNN-LSTM* | *84.10 ± 0.00% * | * 4.12 ± 0.00ms * | *1* |
| *Baseline 1: Standard 1D-CNN* | * 79.85 ± 0.00% * | * 1.21 ± 0.00ms * | *1* |

**Checklist tabel:**
- [v] Self-contained (judul jelas, satuan ada, N tercantum)
- [v] Mean ± std (bukan single number)
- [v] Diurutkan berdasarkan metrik utama
- [v] Format konsisten di semua baris

---

## Latihan 2 — Rencana Visualisasi

Rencanakan 2-3 grafik untuk menyajikan data dari Latihan 1. Setiap grafik = satu pesan.

| # | Jenis Grafik | Pesan | Data yang Digunakan |
|---|-------------|-------|---------------------|
| 1 | *Bar chart + error bar* | *Menunjukkan keunggulan F1-score model usulan dibandingkan kedua baseline beserta stabilitasnya terhadap variasi seed.* | * {Mean F1-score} ± {std} dari tabel hasil.* |
| 2 | *Line Chart* | *Menampilkan visualisasi kurva penurunan training loss tiap epoch untuk membuktikan kecepatan konvergensi lapisan attention.* | *Nilai loss per epoch dari data log internal run 1–3.* |
| 3 | *Scatter plot* | *Menilai trade-off antara efisiensi komputasi fisik (latency) dengan keandalan deteksi (F1-score) antar ketiga model.* | * Mean F1-score vs Mean Latency* |

---

## Latihan 3 — Bias Detection

Evaluasi visualisasi berikut untuk bias (skenario dari contoh):

**Skenario:** Metode A = 91.2%, Metode B = 90.8%. Bar chart dengan Y-axis mulai dari 90%.

| Pertanyaan | Jawaban |
|-----------|---------|
| Apakah Y-axis menyesatkan? | *Ya — Karena sumbu Y dimulai dari $90\%$, kolom Metode A secara visual tampak dua kali lipat lebih tinggi dari Metode B, padahal selisih aslinya sangat tipis (hanya 0.4%).* |
| Apakah error bar ditampilkan? | *Tidak — Tanpa error bar, pembaca tidak bisa mengetahui apakah perbedaan $0.4\%$ tersebut signifikan atau hanya fluktuasi acak (margin of error).* |
| Apakah semua kondisi ditampilkan? | *Ya — Kedua metode ditampilkan berdampingan, namun pemotongan skala sumbu Y mendistorsi interpretasi performa nyata.* |
| Apa solusinya? | *Mengubah batas bawah sumbu Y agar dimulai dari skala dasar $0\%$ agar representasi visual proporsional, serta menambahkan komponen error bar pada masing-masing kolom grafik.* |

**Evaluasi grafik Anda sendiri dari Latihan 2:**
- [v] Semua bias check lulus
- [ ] Ada yang perlu diperbaiki: -

---

## Refleksi

> Mengapa tabel dan grafik keduanya diperlukan — tidak cukup salah satu saja? Pernahkah Anda membuat grafik yang (tanpa sengaja) menyesatkan?
Keduanya memiliki peran komplementer dalam komunikasi data ilmiah. Tabel diperlukan untuk menyajikan nilai numerik eksak, presisi data, serta deviasi standar secara detail untuk kebutuhan pengujian ulang (reproducibility). Sebaliknya, grafik diperlukan untuk memberikan pemahaman visual yang instan mengenai tren, perbandingan relatif, serta trade-off antar-arsitektur (seperti melihat titik temu optimal antara latency vs accuracy) yang sulit ditangkap dengan cepat jika hanya melihat deretan angka mentah.
> Pernahkah Anda membuat grafik yang (tanpa sengaja) menyesatkan?
Ya, saya pernah membuat grafik garis untuk memantau nilai training loss di mana rentang sumbu datanya disempitkan secara otomatis oleh library visualisasi. Akibatnya, fluktuasi loss yang sebenarnya sangat kecil dan wajar tampak seperti penurunan tajam yang drastis dan tidak stabil, yang bisa membuat pembaca menyimpulkan secara keliru bahwa proses pelatihan model mengalami masalah divergensi.

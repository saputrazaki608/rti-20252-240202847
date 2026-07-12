# WS-07: Experimental Design & Validity

> **Bab 7 — Experimental Design & Validity**

---

## Ringkasan Materi

### Correlation ≠ Causality

Kausalitas membutuhkan 3 syarat:
1. **Covariance** — X dan Y bergerak bersama
2. **Temporal precedence** — X berubah sebelum Y
3. **Elimination of alternatives** — Tidak ada faktor lain yang menjelaskan Y

Controlled experiment adalah satu-satunya metode yang bisa membuktikan kausalitas.

### Empat Jenis Validitas

| Jenis | Pertanyaan | Ancaman Umum |
|-------|-----------|-------------|
| **Internal** | Apakah hubungan IV→DV nyata? | Confounding variable, selection bias |
| **External** | Apakah bisa digeneralisasi? | Dataset terlalu spesifik |
| **Construct** | Apakah mengukur konsep yang benar? | Metrik tidak sesuai |
| **Conclusion** | Apakah kesimpulan statistik valid? | Sample size kecil, uji salah |

Internal dan external validity sering berkonflik: semakin terkontrol (internal kuat) → semakin artificial (external lemah).

### Tiga Tipe Eksperimen dalam Riset TI

| Tipe | Deskripsi | Kapan Digunakan |
|------|----------|----------------|
| **Comparison Study** | Metode A vs B pada kondisi identik | Membandingkan pendekatan berbeda |
| **Ablation Study** | Full system → lepas komponen satu per satu | Mengukur kontribusi tiap komponen |
| **Parameter Study** | Variasikan satu parameter, amati dampak | Uji sensitifitas/robustness |

### Fairness dalam Perbandingan

Perbandingan yang adil = **kondisi identik** untuk semua metode: dataset sama, preprocessing sama, tuning effort sebanding, environment sama, metrik sama.

Contoh tidak adil: Transformer (30 fitur tambahan + Bayesian optimization) vs RF (default params) → hasilnya misleading.

### Threats to Validity = Diidentifikasi Sebelum Eksperimen

Ancaman validitas harus diidentifikasi **sebelum** eksperimen dan mitigasinya dirancang sebagai bagian dari desain — bukan ditulis sebagai boilerplate setelah selesai.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan testing | Memastikan sistem memenuhi requirement | Membuktikan hubungan kausal antar variabel |
| Baseline | Versi sebelumnya (last release) | Metode tervalidasi dari literatur |
| Kegagalan | Bug → fix → release | H₀ tidak ditolak → tetap kontribusi ilmiah |
| Sukses | 100% test pass | Evidence valid — mendukung atau menolak hipotesis |

### Istilah Penting

- **Causality** — Hubungan sebab-akibat (covariance + temporal + elimination)
- **Controlled Experiment** — Ubah satu variabel, kontrol sisanya, amati efek
- **Fairness** — Semua metode diuji pada kondisi yang benar-benar identik
- **Threats to Validity** — Faktor yang bisa melemahkan kesimpulan jika tidak dimitigasi
- **Conclusion Validity** — Validitas statistik: power, sample size, uji yang tepat

---

## Template A.7 — Desain Eksperimen Lengkap

```
EXPERIMENT DESIGN

Research Question : ____________________
Hypothesis        : ____________________
Tipe Eksperimen   : [ ] Comparison  [ ] Ablation  [ ] Parameter

Kondisi Eksperimen:
| Kondisi | Deskripsi | IV Value | CV Settings |
|---------|-----------|----------|-------------|
| Control |           |          |             |
| Treatment |         |          |             |

Fairness Checklist:
  [ ] Dataset identik untuk semua kondisi
  [ ] Preprocessing setara
  [ ] Tuning effort setara
  [ ] Environment identik
  [ ] Metrik evaluasi sama

Threat Analysis:
| Threat Type | Ancaman Spesifik | Mitigasi |
|-------------|-----------------|----------|
| Internal    |                 |          |
| External    |                 |          |
| Construct   |                 |          |
| Conclusion  |                 |          |

Statistical Plan:
  Uji statistik   : ____________________
  Justifikasi      : ____________________
  Alpha            : ____________________
  Effect size min  : ____________________
```

---

## Latihan 1 — Desain Eksperimen

Susun desain eksperimen berdasarkan RQ, variabel, dan sistem dari WS-04 sampai WS-06.

**RQ:** Bagaimana performa arsitektur Lightweight CNN yang diintegrasikan dengan Temporal Attention Mechanism dalam menurunkan inference latency sekaligus mempertahankan nilai F1-score pada kelas serangan minoritas jika dibandingkan dengan arsitektur Standard 1D-CNN dan Hybrid CNN-LSTM menggunakan dataset CICIDS2017?

**Tipe eksperimen:** [v] Comparison / [ ] Ablation / [ ] Parameter

| Kondisi | Deskripsi | IV Value | CV Settings |
|---------|-----------|----------|-------------|
| Control | *Evaluasi model baseline standar industri dan SOTA dari studi literatur terdahulu.* | *Standard 1D-CNN & Hybrid CNN-LSTM* | *Dataset CICIDS2017, 80:20 stratified split, random seed 42, Adam optimizer, learning rate 0.001.* |
| Treatment | *Evaluasi arsitektur usulan yang dikembangkan untuk mengatasi kesenjangan riset.* | *Lightweight CNN + Temporal Attention* | *Dataset CICIDS2017, 80:20 stratified split, random seed 42, Adam optimizer, learning rate 0.001.* |

---

## Latihan 2 — Fairness Checklist

Evaluasi apakah desain eksperimen di Latihan 1 sudah fair.

| Kriteria | Status | Detail |
|----------|--------|--------|
| Dataset identik | *✅ Terpenuhi* | *Seluruh model (Control & Treatment) dilatih dan diuji menggunakan partisi baris data (subset) yang persis sama dari CICIDS2017.* |
| Preprocessing setara | *✅ Terpenuhi*  | *Metode pembersihan data, penanganan nilai hilang, dan penskalaan fitur menggunakan konfigurasi Min-Max Scaler yang seragam.* |
| Tuning effort setara | *✅ Terpenuhi*  | *Jumlah epoch maksimum (misal: 50 epochs), ukuran batch (256), dan arsitektur penghenti dini (Early Stopping dengan kesabaran 5 epochs) disamakan.* |
| Environment identik | *✅ Terpenuhi*  | *Semua proses training dan benchmarking waktu dijalankan di atas mesin kontainer Docker yang sama dengan alokasi hardware resource terisolasi.* |
| Metrik evaluasi sama | *✅ Terpenuhi*  | *Pengukuran efisiensi waktu (Inference Latency) dan keandalan klasifikasi (F1-Score) dihitung lewat fungsi pembantu (utility functions) yang sama.* |

**Ada yang tidak fair?** [ ] Ya / [v] Tidak
> Jika ya, bagaimana cara memperbaikinya? — (Desain eksperimen sudah sepenuhnya adil karena satu-satunya variabel yang berubah hanyalah struktur arsitektur internal model itu sendiri).

---

## Latihan 3 — Threat Analysis

Identifikasi ancaman validitas untuk desain eksperimen ini.

| Threat Type | Ancaman Spesifik | Mitigasi |
|-------------|-----------------|----------|
| Internal | *Adanya bias fluktuasi latensi komputasi akibat background process dari Sistem Operasi host selama pengujian model berlangsung.* | *Melakukan pemanasan awal (warm-up runs) sebanyak 100 iterasi, diikuti pemrosesan uji 30 kali ulangan untuk diambil rata-rata terpotong (trimmed mean).* |
| External | *Karakteristik fitur model yang terlalu spesifik (overfitting) pada struktur data CICIDS2017, sehingga kinerjanya menurun jika diuji pada tipe lalu lintas jaringan lain.* | *Mengunci arsitektur agar murni berbasis ekstraksi fitur dimensi log tanpa hardcoded parameters, serta menyiapkan rencana validasi silang pada dataset sekunder.* |
| Construct | *Penggunaan metrik rata-rata waktu (average latency) yang berpotensi menyembunyikan masalah latensi ekstrem (long-tail latency outliers) pada paket data tertentu.* | *Menyertakan pelaporan metrik distribusi tambahan berupa nilai persentil ke-95 ($P_{95}$) dan ke-99 ($P_{99}$) untuk melengkapi gambaran variabilitas waktu.* |
| Conclusion | *Pengambilan kesimpulan keunggulan model yang terburu-buru murni dari selisih angka desimal metrik tanpa landasan kepastian yang kuat.* | *Menerapkan uji signifikansi statistik secara formal (seperti Paired t-Test atau ANOVA) dengan ambang batas $\alpha = 0.05$ sebelum menarik kesimpulan akhir.* |

**Ancaman mana yang paling sulit dimitigasi?** External Validity
> Karena dataset publik di laboratorium (statis) bagaimanapun tidak akan pernah bisa 100% menduplikasi dinamika, kecepatan volume, dan mutasi tak terduga dari serangan siber baru (zero-day attacks) di jaringan lokal dunia nyata (production environment) yang sangat liar.

---

## Refleksi

> Sebuah paper melaporkan "metode kami mengalahkan semua baseline." Apa 3 pertanyaan pertama yang harus diajukan untuk mengevaluasi klaim ini?

**Jawaban:**
1. Apakah baseline yang digunakan sudah dioptimalkan secara adil (tuned properly)?
sering kali peneliti sengaja membiarkan model baseline berjalan dengan hyperparameter bawaan (default) yang lemah agar model usulan mereka terlihat jauh lebih unggul secara instan.
2. Pada metrik apa dan di bagian subset data yang mana metode tersebut menang?
Perlu dipastikan apakah keunggulan tersebut merata di semua lini, atau jangan-jangan model tersebut hanya unggul tipis di kelas mayoritas yang mudah ditebak, namun sebenarnya hancur dalam mendeteksi variasi data kritis kelas minoritas.
3. Apakah lingkungan pengujian, arsitektur prapemrosesan, dan data latih yang diberikan kepada setiap model benar-benar identik?
Jika model usulan diberikan keuntungan berupa data fitur yang lebih bersih atau prapemrosesan yang lebih kompleks daripada model baseline, maka perbandingan performa tersebut menjadi cacat secara ilmiah (unfair comparison).

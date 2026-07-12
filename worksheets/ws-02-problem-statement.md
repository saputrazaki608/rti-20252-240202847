# WS-02: Problem Statement

> **Bab 2 — Problem Formulation & System Context**

---

## Ringkasan Materi

### Problem Formation Model

Masalah riset melewati 5 tahap transformasi. Melompat langsung dari Reality ke Variable adalah kesalahan paling umum.

```
Reality → Observed Issue (Symptom) → Diagnosed Problem (Root Cause)
→ Researchable Problem (Scoped) → Measurable Variable (Operationalized)
```

### Topic ≠ Problem ≠ Research Problem

| Level | Contoh | Status |
|-------|--------|--------|
| **Topik** | Keamanan IoT | Terlalu luas, tidak bisa diuji |
| **Problem** | MQTT tidak terenkripsi | Spesifik tapi belum riset |
| **Research Problem** | Belum ada studi membandingkan overhead TLS 1.3 vs DTLS pada MQTT di IoT RAM < 64KB | Bisa dirancang eksperimennya |

### Symptom vs Root Cause

Apa yang diamati (gejala) ≠ mengapa terjadi (akar masalah). Gunakan **5 Whys** atau **Fishbone Diagram** untuk menggali.

Contoh: "User meninggalkan checkout" (symptom) → "Waktu loading > 8 detik karena API call sequential" (root cause).

### System Thinking

Setiap masalah riset TI harus terikat pada komponen sistem: **Input → Process → Output → Outcome → Constraints → Stakeholders**.

### Problem Quality Check

Masalah riset yang layak harus memenuhi 5 kriteria:
- **Clarity** — Satu orang membaca akan paham
- **Measurability** — Ada metrik kuantitatif
- **Relevance** — Penting untuk domain
- **Testability** — Bisa gagal (falsifiable)
- **Impact** — Ada kontribusi jika terjawab

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan | Menyelesaikan masalah (*solve*) | Memahami dan membuktikan (*understand & prove*) |
| Masalah | Bug, error, fitur belum ada | Gap dalam pengetahuan |
| Scope | Selesaikan semua yang perlu | Batasi agar bisa dibuktikan |
| Output | Working system | Evidence, paper, replicable findings |

### Istilah Penting

- **Problem Statement** — Formulasi tertulis: konteks sistem + gap + dampak + justifikasi
- **System Context** — Deskripsi lengkap: input, proses, output, outcome, constraints, stakeholders
- **Problem Drift** — Masalah "bermutasi" dari pendahuluan ke metodologi karena statement awal tidak presisi
- **Solution-First Thinking** — Memulai dari solusi tanpa masalah yang jelas — berbahaya dalam riset
- **Operational Definition** — Definisi variabel yang cukup jelas agar peneliti lain bisa mengukur hal yang sama

---

## Template A.2 — Problem Statement Builder

```
PROBLEM STATEMENT BUILDER

Domain & Konteks
  Domain   : ____________________
  Konteks  : ____________________

System Context
  Input       : ____________________
  Process     : ____________________
  Output      : ____________________
  Outcome     : ____________________
  Constraints : ____________________
  Stakeholders: ____________________

Fenomena → Problem
  Fenomena yang diamati             : ____________________
  Gejala (symptom) yang terukur     : ____________________
  Masalah yang didiagnosis          : ____________________
  Masalah riset (researchable)      : ____________________
  Variabel yang terukur             : ____________________

Problem Quality Check
  [ ] Clarity — Apakah satu orang membaca akan paham?
  [ ] Measurability — Apakah ada metrik kuantitatif?
  [ ] Relevance — Apakah penting untuk domain?
  [ ] Testability — Apakah bisa gagal?
  [ ] Impact — Apakah ada kontribusi jika terjawab?

Problem Statement (1 paragraf):
  ____________________
```

---

## Latihan 1 — Dari Topik ke Masalah Riset

Pilih satu topik di bidang TI yang diminati. Transformasikan melalui 5 tahap Problem Formation Model.

**Topik awal:** *Deteksi Anomali Lalu Lintas Jaringan Berbasis Deep Learning untuk Keamanan Siber*

| Tahap | Hasil |
|-------|-------|
| Reality | *Infrastruktur jaringan komputer terus-menerus menghadapi ancaman serangan siber yang menyusup di sela-sela jutaan paket data aktivitas normal sehari-hari.* |
| Observed Issue (Symptom) | *Terjadi peningkatan serangan siber yang lolos dari deteksi (High False Negative Rate) dan tingginya alarm palsu (High False Alarm Rate) saat menggunakan sistem keamanan jaringan (Intrusion Detection System) tradisional.* |
| Diagnosed Problem (Root Cause) | *Metode deteksi tradisional berbasis aturan (rule-based) bersifat statis dan tidak mampu beradaptasi untuk mengenali pola variasi serangan siber modern yang sengaja disamarkan agar menyerupai paket data normal.* |
| Researchable Problem | *Bagaimana merancang arsitektur model klasifikasi berbasis Convolutional Neural Network (CNN) yang mampu mengeksplorasi fitur-fitur spasial dari log paket jaringan secara otomatis tanpa bergantung pada signature serangan yang sudah usang?* |
| Measurable Variable | *Variabel Bebas (Independent): Variasi arsitektur arsitektur CNN (jumlah layer, ukuran filter) dan metode prapemrosesan data. Variabel Terikat (Dependent): Tingkat Akurasi, Precision, Recall, F1-Score, dan False Alarm Rate (FAR) model.* |

**Apakah terjebak solution-first thinking?** [ ] Ya / [ ] Tidak
> Jika ya, kembali ke tahap mana? ________________________

---

## Latihan 2 — System Context Decomposition

Gambarkan konteks sistem dari masalah riset di Latihan 1.

| Komponen | Deskripsi |
|----------|----------|
| Input | *Aliran paket data jaringan mentah (raw network traffic/pcap logs) atau fitur-fitur dataset statistik lalu lintas jaringan (seperti durasi, protokol, jumlah bytes).* |
| Process | *Prapemrosesan data (normalization & feature mapping), ekstraksi fitur spasial secara otomatis oleh lapisan konvolusi model CNN yang telah diefisiensikan, dan klasifikasi kelas data.* |
| Output | *Label prediksi klasifikasi untuk setiap paket data atau koneksi yang masuk, yaitu apakah berstatus Normal atau Anomali/Serangan (beserta skor probabilitasnya).* |
| Outcome | *Terwujudnya sistem deteksi intrusi (IDS) yang responsif, penurunan tingkat kelolosan serangan (False Negative), serta berkurangnya beban komputasi (CPU/RAM overhead) pada gerbang jaringan.* |
| Constraints | *Keterbatasan spesifikasi perangkat keras (hardware resource constraints pada edge gateway), keharusan memproses data dengan latensi sangat rendah (real-time processing limit), dan ketersediaan dataset latih yang seimbang.* |
| Stakeholders | *Network Security Administrator, tim Cyber Security Incident Response, peneliti bidang keamanan siber, dan penyedia infrastruktur jaringan operasional perusahaan.* |

**Komponen mana yang paling relevan dengan masalah riset?** *Process dan Constraints*

---

## Latihan 3 — Problem Quality Check

Evaluasi problem statement yang sudah dibuat menggunakan 5 kriteria.

| Kriteria | Skor (1-5) | Justifikasi |
|----------|-----------|-------------|
| Clarity | *4* | *Rumusan masalah sudah jelas fokus pada aspek efisiensi komputasi model CNN, namun perlu penyebutan domain operasional target secara eksplisit (misal: perangkat edge gateway).* |
| Measurability | *5* | *Sangat terukur karena keberhasilan riset dievaluasi langsung menggunakan metrik kuantitatif baku seperti inference latency (milidetik), jumlah parameter, serta nilai F1-score.* |
| Relevance | *5* | *Sangat relevan dengan tantangan nyata di bidang TI, di mana model deep learning berskala besar sering kali mengalami bottleneck saat harus memproses lalu lintas data jaringan yang padat secara real-time.* |
| Testability | *5* | *Sangat bisa diuji secara empiris melalui eksperimen komparasi di laboratorium dengan membandingkan performa model CNN usulan terhadap arsitektur CNN baseline.* |
| Impact | *4* | *Berdampak besar bagi penguatan infrastruktur keamanan siber industri karena memungkinkan penerapan IDS berbasis AI yang hemat daya, cepat, dan murah secara operasional.* |

**Skor total:** 23 / 25

**Problem statement versi final (1 paragraf):**
> Penerapan model Deep Learning seperti Convolutional Neural Network (CNN) untuk sistem deteksi intrusi jaringan (Intrusion Detection System) terbukti mampu memberikan akurasi tinggi, namun struktur arsitekturnya yang kompleks menimbulkan masalah tingginya latensi komputasi (inference latency) dan konsumsi sumber daya memori yang besar. Karakteristik ini membuatnya tidak efisien dan sulit diimplementasikan secara real-time pada perangkat edge gateway jaringan dengan kapasitas komputasi terbatas. Oleh karena itu, penelitian ini dirancang untuk mengatasi bottleneck tersebut dengan merancang arsitektur model CNN yang ringan (lightweight) guna mereduksi kompleksitas komputasi dan waktu eksekusi deteksi tanpa mengorbankan keandalan tingkat akurasi (F1-score) dalam memitigasi serangan anomali jaringan.

---

## Refleksi

> Bandingkan "masalah" yang biasa ditemui saat coding (bug, error) dengan masalah riset. Apa perbedaan fundamental dalam cara mendefinisikan dan mendekati keduanya?

**Jawaban:**
> Perbedaan fundamental antara "masalah saat coding" (bug/error) dengan "masalah riset" terletak pada sifat solusi dan tujuannya:

Masalah Coding (Bug/Error): Bersifat deterministik dan teknis operasional. Solusinya sudah pasti ada (misalnya memperbaiki kesalahan sintaksis, logic error, atau konfigurasi) dan bertujuan agar program berjalan sesuai spesifikasi rancangan yang sudah diketahui sebelumnya. Cara mendekatinya adalah dengan melakukan debugging lokal menggunakan stack trace atau log error.

Masalah Riset: Bersifat terbuka (open-ended problem), belum memiliki solusi tunggal yang pasti, dan berakar pada kesenjangan pengetahuan (knowledge gap) atau batasan performa sistem yang ada. Cara mendekatinya membutuhkan metode ilmiah yang sistematis, dimulai dari observasi gejala di dunia nyata, tinjauan literatur ilmiah untuk memetakan solusi yang sudah ada, hingga pengujian hipotesis baru secara empiris guna menghasilkan pengetahuan baru (new knowledge) atau artefak yang dioptimalkan.

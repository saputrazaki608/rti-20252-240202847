# WS-09: Implementation & Environment

> **Bab 9 — Implementasi Riset & Kontrol Lingkungan**

---

## Ringkasan Materi

### Implementasi Riset ≠ Coding Biasa

Tujuan implementasi riset bukan membuat software yang berfungsi, melainkan membangun **instrumen pengukuran yang konsisten**. Setiap modul harus di-mapping ke variabel (dari Bab 6), parameter harus config-driven, dan logging aktif dari hari pertama.

> **Mengapa reproducibility penting?** Sains dibangun di atas prinsip verifikasi — temuan harus bisa dikonfirmasi oleh peneliti lain. _Replicability crisis_ yang terjadi di banyak paper riset ML/AI disebabkan oleh environment tidak terdokumentasi: orang lain tidak bisa reproduksi, hasil diragukan, kepercayaan terhadap temuan hilang. Prinsip: **dokumentasi environment = snapshot kredibilitas riset Anda.**

### Reproducible Implementation Model

```
Design → Implementation → Environment Setup → Execution Consistency → Reproducibility → Trustworthy Result
```

Setiap transisi memiliki syarat:
- Design → Implementation: kode sesuai mapping variabel-ke-komponen
- Implementation → Environment: versi, dependency, seed, path, OS eksplisit
- Environment → Consistency: seed terkunci, urutan deterministik
- Consistency → Reproducibility: dokumentasi lengkap
- Reproducibility → Trust: siapa pun ikuti dokumentasi → hasil sama/serupa

### Repeatability vs Reproducibility

| Level | Peneliti | Environment | Hasil |
|-------|---------|-------------|-------|
| **Repeatability** | Sama | Sama | Sama persis |
| **Reproducibility** | Berbeda | Berbeda (ikuti docs) | Sama/serupa |

Capai **repeatability** dulu, baru **reproducibility**.

### Engineering vs Research Perspective

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Tujuan | Sistem berfungsi untuk user | Instrumen pengukuran konsisten |
| Dependency | Update ke terbaru | Lock di versi spesifik |
| Testing | Unit, integration, E2E | Repeatability test (run ulang → sama?) |
| Dokumentasi | User guide, API docs | Environment spec, execution steps, expected output |
| Config | Default masuk akal | Setiap parameter eksplisit & adjustable |

### Jebakan Kognitif

1. Menunda environment setup → bug sulit dilacak
2. Tidak pakai version control → hasil tidak bisa direkonstruksi
3. Menolak Docker/container → "di laptop saya bisa" saat review
   - **Docker** = teknologi container yang "membungkus" aplikasi beserta seluruh dependency-nya dalam satu unit terisolasi. Hasilnya: kode berjalan identik di laptop, server, maupun reviewer lain. Intro singkat: `docker run -v $(pwd):/workspace environment-image python run_experiment.py`
4. 3× hasil sama ≠ repeatable (bisa cache/state tersimpan)

### Dependency Locking

Mengandalkan "install library terbaru" berbahaya: versi berbeda = perilaku berbeda = hasil tidak reproducible. Praktik:
- **Python**: buat `requirements.txt` dengan versi eksplisit: `scikit-learn==1.3.2`, lalu kunci dengan `pip freeze > requirements.txt`
- **Conda**: gunakan `conda env export > environment.yml` untuk snapshot lengkap
- **Node.js/R/Julia**: gunakan `package-lock.json` / `renv.lock` / `Project.toml` — semua fungsi serupa: lock versi + hash

### Istilah Penting

- **Environment Specification** — Deskripsi lengkap: hardware, OS, runtime, library + versi, config, seed
- **Dependency** — Komponen eksternal yang harus di-lock versinya
- **Config-driven** — Parameter dieksternalisasi ke file konfigurasi, bukan hardcode

---

## Template A.9 — Dokumentasi Setup Eksperimen

```
EXPERIMENT SETUP DOCUMENTATION

Hardware:
  CPU     : ____________________
  RAM     : ____________________
  GPU     : ____________________
  Storage : ____________________

Software:
  OS        : ____________________
  Runtime   : ____________________
  Framework : ____________________

Dependencies:
| Library | Version | Sumber | Hash/Checksum |
|---------|---------|--------|---------------|
|         |         |        |               |
|         |         |        |               |

Konfigurasi:
  Config file     : ____________________
  Random seed     : ____________________
  Hyperparameters : ____________________

Reproducibility Check:
  [ ] Dependency terdokumentasi (requirements.txt / lock file)
  [ ] Seed ditetapkan di semua level (Python, NumPy, framework)
  [ ] Config di version control
  [ ] README instruksi reproduksi lengkap
```

---

## Latihan 1 — Environment Specification

Dokumentasikan environment untuk eksperimen Anda (boleh environment saat ini atau yang direncanakan).

| Komponen | Spesifikasi |
|----------|------------|
| CPU | *Intel Core i5-12400F @ 2.50GHz (6 Cores, 12 Threads)* |
| RAM | *16 GB DDR4 Dual-Channel* |
| GPU | *NVIDIA GeForce RTX 3050 8GB VRAM* |
| OS | *Windows 11 Home / Linux Ubuntu 22.04 LTS via WSL2* |
| Runtime | *Python 3.10.12* |
| Framework | *PyTorch 2.1.2 + CUDA 11.8* |
| Random Seed | *42* |

**Dependencies (minimal 5):**

| Library | Version | Alasan Dibutuhkan |
|---------|---------|-------------------|
| *Toreh* | *2.1.2* | *Framework utama untuk membangun arsitektur tensor Lightweight CNN dan lapisan Temporal Attention.* |
| *Torchvision* | *0.16.2* | *Memanfaatkan modul utilitas pengolahan sekuensial tensor dasar.* |
| *Pandas* | *2.1.4* | *Prapemrosesan data, penyaringan baris, dan penanganan ketidakseimbangan kelas log CICIDS2017.* |
| *Scikit-learn* | *1.3.2* | *Melakukan stratified train-test split serta kalkulasi metrik F1-score kelas minoritas.* |
| *Numpy* | *1.26.2* | *Manipulasi array matriks konfusi dan penguncian acak tingkat dasar (vectorized calculations).* |

---

## Latihan 2 — Repeatability Test Plan

Rancang tes repeatability sederhana: jalankan kode yang sama 3× di environment yang sama.

| Run | Seed | Metrik Utama | Hasil Sama? |
|-----|------|-------------|-------------|
| 1 | *42* | *Inference Latency & F1-Score Minoritas* | — |
| 2 | *42* | *Inference Latency & F1-Score Minoritas* | [v] Ya / [ ] Tidak |
| 3 | *42* | *Inference Latency & F1-Score Minoritas* | [v] Ya / [ ] Tidak |

**Jika hasil berbeda, kemungkinan penyebab:**

> Penyebab umum non-repeatability:
> - **Thermal throttling** — CPU/GPU overheating pada run berturut-turut → clock speed turun → waktu eksekusi berubah
> - **Background process** — antivirus scan, update OS, atau cloud sync aktif saat run berlangsung
> - **Cache dari run sebelumnya** — hasil tersimpan di memori/disk sehingga run berikutnya tidak menjalankan komputasi penuh
> - **Random state tidak dikontrol di semua level** — Python seed di-set, tapi NumPy/PyTorch/TensorFlow punya seed independen

___________________________________________________

**Checklist kontrol yang sudah diterapkan:**
- [v] Random seed di-set di semua level
- [v] Tidak ada background process yang mengganggu
- [v] Cache dibersihkan antar-run
- [v] Config file yang sama untuk semua run

---

## Latihan 3 — README Eksperimen

Tulis README minimum untuk eksperimen Anda (6 komponen wajib).

```
# Judul Eksperimen: Lightweight CNN dengan Temporal Attention Mechanism untuk Deteksi Anomali Jaringan Kritis pada Dataset CICIDS2017

## 1. Environment
> CPU: Intel Core i5-12400F (6 Cores)
> RAM: 16 GB DDR4
> GPU: NVIDIA RTX 3050 8GB
> OS: Ubuntu 22.04 LTS (via WSL2)
> Runtime: Python 3.10.12, PyTorch 2.1.2, CUDA 11.8

## 2. Installation
> ```bash
# Klon repositori eksperimen
git clone [https://github.com/saputrazaki608/lightweight-cnn-attention.git](https://github.com/saputrazaki608/lightweight-cnn-attention.git)
cd lightweight-cnn-attention
# Buat virtual environment dan install dependencies
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
## 3. Data
> Dataset Publik CICIDS2017 (Koleksi berkas CSV lalu lintas jaringan).
> Format: Berkas terkompresi .csv hasil ekstraksi fitur log PCAP.
> Ukuran: Subset data bersih berukuran sekitar ~1.2 GB setelah pembersihan baris bernilai NaN atau Infinite.

## 4. Execution
> # Menjalankan prapemrosesan data secara stratified
python src/preprocess.py --data_path ./data/CICIDS2017.csv

# Menjalankan pelatihan dan pengujian performa model
python src/main.py --config config/hyperparams.json --run_benchmark true

## 5. Configuration
> Eksperimen dikendalikan melalui file kontrol config/hyperparams.json dengan parameter kunci:

model_type : lightweight_cnn_attention

batch_size : 256

learning_rate : 0.001

random_seed : 42

num_epochs : 50

## 6. Expected Output
> {
  "experiment_status": "SUCCESS",
  "metrics": {
    "average_inference_latency_ms": 1.84,
    "minority_class_f1_score": 0.8725,
    "total_parameters": 45120
  }
}
```

---

## Refleksi

> Apakah eksperimen Anda saat ini bisa direproduksi oleh orang lain tanpa bantuan Anda? Komponen apa yang masih hilang?
**Apakah eksperimen saat ini bisa direproduksi?** Secara komputasi alur logika data dan bobot model (*F1-score*), eksperimen ini sudah bisa direproduksi secara presisi oleh orang lain karena penentuan berkas dan parameter benih acak telah dikunci rapat di file konfigurasi. Namun, untuk metrik fisik *inference latency*, reproduksibilitas mutlak agak sulit dicapai jika orang lain menggunakan spesifikasi komputer yang berbeda kelas.
**Level saat ini:** [v] Repeatability / [ ] Reproducibility / [ ] Belum keduanya
**Komponen yang belum terdokumentasi:**
> Petunjuk pembuatan berkas kontainerisasi lingkungan (*Docker Image* atau berkas `Dockerfile` dan `docker-compose.yml`) belum disediakan secara formal. Dokumentasi tersebut sangat dibutuhkan agar orang lain dapat melakukan replikasi lingkungan sistem operasi dan *library build* secara instan tanpa perlu melakukan instalasi manual satu per satu di komputer lokal mereka.

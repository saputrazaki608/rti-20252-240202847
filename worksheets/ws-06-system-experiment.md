# WS-06: System-Experiment Mapping

> **Bab 6 — System Design sebagai Experimental Artifact**

---

## Ringkasan Materi

### Sistem = Instrumen Pengujian, Bukan Produk

Seorang engineer bertanya "apakah sistem bekerja?" — seorang peneliti bertanya "apa yang bisa dibuktikan sistem ini?" Sistem dalam riset adalah **artifact** — objek yang sengaja dibuat untuk menguji klaim spesifik.

### System as Experiment Model

```
RQ → Variable → System Component → Experimental Setup → Output
```

Setiap komponen sistem harus bisa ditelusuri ke variabel riset (top-down), dan setiap pengukuran harus menjawab RQ (bottom-up).

### Mapping Variabel ke Komponen

| Tipe Variabel | Peran di Sistem | Contoh |
|---------------|----------------|--------|
| **IV** (Independent) | Modul yang bisa di-toggle/swap | Algoritma A vs B |
| **DV** (Dependent) | Modul pengukuran | Logger, metrics collector |
| **CV** (Control) | Config yang dikunci | Dataset, parameter tetap |

Jika variabel tidak bisa di-map ke komponen apapun → arsitektur perlu didesain ulang.

### 4 Prinsip Desain Eksperimental

| Prinsip | Pertanyaan Kunci |
|---------|-----------------|
| **Traceability** | Komponen ini melayani variabel yang mana? |
| **Modularity** | Bisakah IV diubah tanpa memengaruhi yang lain? |
| **Controllability** | Apakah CV dieksternalisasi ke config file? |
| **Measurability** | Apakah sistem otomatis menghasilkan data yang dibutuhkan? |

### Variable Isolation melalui Arsitektur

- **Modular architecture** — Pisahkan berdasarkan variabel
- **Configuration-driven** — Ubah config (YAML/JSON), bukan code
- **Feature toggles** — On/off flag untuk ablation study

  Contoh config YAML dengan feature toggles:
  ```yaml
  model:
    type: cnn          # IV: ganti "rf" untuk kondisi baseline
  features:
    use_temporal: true  # toggle komponen temporal
    use_normalization: true  # toggle preprocessing
  experiment:
    seed: 42
    runs: 5
  ```
  Dengan pendekatan ini, berbeda kondisi eksperimen = berbeda satu baris config, **tanpa mengubah kode**.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan sistem | Memenuhi kebutuhan user | Menguji hipotesis, menghasilkan bukti |
| Arsitektur | Optimasi performa & skalabilitas | Optimasi isolasi variabel & reprodusibilitas |
| Konfigurasi | Sering hardcoded | Dieksternalisasi ke config file |
| Fitur tambahan | Menambah nilai user | Menambah noise jika tidak terkait RQ |

### Istilah Penting

- **Artifact** — Objek yang sengaja dibuat untuk memecahkan masalah atau menguji proposisi
- **Traceability** — Kemampuan menelusuri hubungan RQ → variabel → komponen → output
- **Variable Isolation** — Mengubah hanya satu variabel sambil menahan yang lain konstan
- **Ablation Study** — Menguji kontribusi tiap komponen dengan melepasnya satu per satu
- **Configuration-driven Execution** — Semua parameter di config file, bukan hardcoded

---

## Template A.6 — Mapping RQ ke Arsitektur Sistem

```
SYSTEM-EXPERIMENT MAPPING

Research Question: ____________________

Variable → Component Mapping:
| Variabel | Tipe | Komponen Sistem | Cara Manipulasi/Pengukuran |
|----------|------|-----------------|---------------------------|
|          | IV   |                 |                           |
|          | DV   |                 |                           |
|          | CV   |                 |                           |

4 Prinsip Desain:
  [ ] Traceability — Setiap komponen bisa ditelusuri ke variabel
  [ ] Variable Isolation — IV bisa diubah tanpa mengubah CV
  [ ] Measurement Integration — Pengukuran DV built-in
  [ ] Reproducibility — Setup bisa direkonstruksi

Experimental Setup:
  Input data     : ____________________
  Parameter      : ____________________
  Output format  : ____________________
```

---

## Latihan 1 — Variable-to-Component Mapping

Gunakan RQ dan variabel dari WS-05. Petakan ke komponen sistem.

**RQ:** __________________________________________________

| Variabel | Tipe | Komponen Sistem | Cara Manipulasi / Pengukuran |
|----------|------|-----------------|---------------------------|
| *Arsitektur Model* | *IV* | *Modul Klasifikasi (swap arsitektur jaringan).* | *Mengubah parameter string --model_type pada file konfigurasi (config.json) atau argumen CLI.* |
| *Inference Latency* | DV | *Modul Evaluasi & Benchmark Performance.* | *Menghitung durasi waktu eksekusi fungsi model.forward() menggunakan fungsi torch.cuda.Event (skala milidetik).* |
| *Lingkungan Pengujian* | CV | *Kontainerisasi Sistem (Docker Environment).* | *Membatasi alokasi core CPU dan memori melalui opsi runtime Docker agar tetap statis sepanjang eksperimen.* |

**Apakah semua variabel bisa di-map?** [v] Ya / [ ] Tidak
> Jika tidak, komponen apa yang perlu ditambahkan? — (Semua variabel berhasil dipetakan ke dalam arsitektur perangkat lunak eksperimen).
---

## Latihan 2 — 4 Prinsip Desain

Evaluasi desain sistem terhadap 4 prinsip.

| Prinsip | Status | Bukti / Penjelasan |
|---------|--------|-------------------|
| Traceability | * ✅ Terpenuhi* | *Setiap metrik output (latency, F1-score) tersimpan dalam file JSON hasil eksperimen yang mencantumkan hash ID konfigurasi model pemicunya secara otomatis.* |
| Modularity | * ✅ Terpenuhi* | *Blok arsitektur model dibuat terpisah (modul backbone_cnn.py, attention.py, dan classifier.py) sehingga struktur internalnya dapat dibongkar pasang dengan mudah.* |
| Controllability | * ✅ Terpenuhi* | *Aliran eksperimen, penentuan hyperparameter, dan jenis model yang dieksekusi sepenuhnya dikendalikan via file konfigurasi terpusat tanpa menyentuh kode utama.* |
| Measurability | * ✅ Terpenuhi* | *Kode dilengkapi modul pencatatan metrik khusus (logger script) yang mengambil langsung nilai performa komputasi dan akurasi di setiap akhir tahapan evaluasi.* |

**Prinsip mana yang paling sulit dipenuhi?** **Controllability*
**Strategi untuk mengatasinya:**
> Mengontrol kestabilan inference latency pada lingkungan sistem operasi modern cukup menantang karena adanya interferensi proses latar belakang (background OS noise). Strateginya adalah menjalankan pengujian tanpa antarmuka grafis (headless mode), mengisolasi inti CPU (CPU pinning), serta melakukan pengujian berulang sebanyak 30 kali untuk mengambil rata-rata performa (trimmed mean).
---

## Latihan 3 — Ablation Study Planning

Jika sistem memiliki 3 komponen utama, rencanakan ablation study.

> **Panduan jumlah kondisi:** Untuk 3 komponen (A, B, C), kondisi minimal yang direkomendasikan:
> Full + (-A) + (-B) + (-C) = **4 kondisi dasar**. Jika waktu memungkinkan, tambahkan kombinasi ganda: (-A,-B), (-A,-C), (-B,-C) = **7 kondisi**. Sesuaikan dengan *computational cost* dan tenggat waktu penelitian.

| Kondisi | Komponen A | Komponen B | Komponen C | Hasil yang Diharapkan |
|---------|-----------|-----------|-----------|----------------------|
| Full | *✅ (Lightweight)* | *✅ (Attention)* | *✅ norm* | *Sistem usulan lengkap: Diharapkan memberikan latensi rendah dengan F1-score serangan minoritas yang stabil.* |
| – A | ❌ (ganti RF) | ✅ | ✅ | *Baseline Penuh: Memakai Standard 1D-CNN untuk menguji pengaruh tulang punggung jaringan terhadap performa dasar.* |
| – B | ✅ | ❌ (tanpa temporal) | ✅ | *Menguji seberapa krusial peran Attention Module dalam mendeteksi pola temporal serangan minoritas.* |
| – C | ✅ | ✅ | ❌ (tanpa normalisasi) | *Menguji pengaruh prapemrosesan normalisasi data mentah terhadap kecepatan konvergensi akurasi model.* |

**Komponen mana yang diprediksi paling berkontribusi?** Komponen B (Temporal Attention Module)
**Mengapa?**
> Karena komponen ini dirancang khusus sebagai solusi utama untuk mengatasi gap penelitian kita, yaitu kegagalan model lightweight konvensional dalam mengenali karakteristik unik temporal dari pola serangan siber berskala kecil (kelas minoritas).

## Refleksi

> Apa risiko jika sistem dibangun seperti produk (monolitik, fitur lengkap) lalu baru dilakukan eksperimen? Mengapa arsitektur modular penting untuk riset?

**Jawaban:**
> Risiko Sistem Monolitik Langsung Jadi: Jika sistem langsung dibangun sebagai satu kesatuan produk yang utuh tanpa pemisahan komponen sejak awal, peneliti akan menghadapi masalah kegagalan isolasi variabel. Saat hasil akurasi model ternyata buruk atau latensinya membengkak, peneliti tidak akan bisa melacak dengan pasti komponen internal mana yang menjadi pemicu masalah (root cause). Hal ini membuat jalannya eksperimen menjadi bias, subjektif, dan tidak valid secara metode ilmiah.

> Pentingnya Arsitektur Modular untuk Riset: Arsitektur modular bertindak sebagai fondasi utama yang memungkinkan dilakukannya studi ablasi (ablation study) dan uji komparasi secara adil. Dengan memisahkan setiap fungsionalitas ke dalam blok independen, kita bisa memanipulasi, mencabut, atau menukar satu komponen spesifik secara terkontrol tanpa merusak jalannya alur program keseluruhan. Desain ini memastikan bahwa setiap klaim peningkatan performa yang kita tulis di laporan akhir benar-benar dapat dibuktikan secara empiris berasal dari komponen usulan baru tersebut.

# WS-13: Data Preprocessing

> **Bab 13 — Preprocessing & Persiapan Data untuk Analisis**

---

## Ringkasan Materi

### Data Refinement Pipeline

```
Raw Data → Cleaning → Transformation → Normalization → Processed Data → Analysis Ready
```

Setiap tahap memiliki tujuan berbeda. **Preprocessing bukan langkah teknis biasa** — setiap keputusan preprocessing adalah keputusan riset yang bisa mengubah kesimpulan.

### Empat Prinsip Preprocessing

| Prinsip | Deskripsi |
|---------|----------|
| **Consistency** | Metode sama untuk data yang sama |
| **Transparency** | Setiap langkah terdokumentasi |
| **Reproducibility** | Orang lain bisa mengulang dengan hasil sama |
| **Minimal Distortion** | Ubah sesedikit mungkin; jika normalisasi tidak perlu, jangan lakukan |

### Cleaning Triad

| Masalah | Strategi | Risiko |
|---------|---------|--------|
| **Missing values** | | |
| — Listwise deletion | Missing < 5%, random | Data loss |
| — Mean/median imputation | Sedikit missing, dist. normal | Mengurangi variabilitas |
| — Model-based imputation | Banyak missing, pola sistematis | Introduces dependency |
| — Flag & separate | Missing karena alasan substantif | Kompleksitas analisis |
| **Duplikat** | Identifikasi → verifikasi → hapus | False positive (data mirip ≠ duplikat) |
| **Error format** | Standardisasi tipe, encoding | Kehilangan informasi saat konversi |

### Normalisasi — Kapan & Metode Mana

| Metode | Formula | Output | Sensitif Outlier? |
|--------|---------|--------|-------------------|
| Min-max | (x-min)/(max-min) | [0, 1] | Ya |
| Z-score | (x-mean)/std | Unbounded | Lebih robust |
| Robust scaling | (x-median)/IQR | Unbounded | Paling robust |

**Kunci:** Parameter normalisasi harus dihitung dari **training set saja** — bukan seluruh data. Pelanggaran = **data leakage**.

### Data Leakage Prevention

Data leakage terjadi ketika informasi dari test set "bocor" ke preprocessing:
- Normalisasi parameter dari seluruh dataset ← **SALAH**
- Cross-validation dilakukan sebelum split ← **SALAH**
- Feature selection menggunakan label test set ← **SALAH**

### Jebakan Kognitif

1. "Preprocessing cuma teknis — tidak perlu detail" → bisa ubah kesimpulan
2. "Lebih banyak preprocessing = lebih bersih = lebih baik" → over-processing distorsi data
3. "Normalisasi selalu diperlukan" → belum tentu, tergantung metode analisis
4. "Imputation sama untuk semua situasi" → strategi harus sesuai konteks

---

## Template A.13 — Preprocessing Documentation Log

```
PREPROCESSING LOG

Dataset           : ____________________
Jumlah data awal  : ____________________

Cleaning:
| Masalah | Jumlah Kasus | Penanganan | Justifikasi |
|---------|-------------|------------|-------------|
| Missing |             |            |             |
| Duplikat|             |            |             |
| Error   |             |            |             |

Transformation:
| Transformasi | Variabel | Detail | Alasan |
|-------------|----------|--------|--------|
|             |          |        |        |

Normalization:
  Metode    : ____________________
  Alasan    : ____________________
  Parameter : (dihitung dari: training set / seluruh data)

Leakage Check:
  [ ] Parameter normalisasi dari training set saja
  [ ] Tidak ada informasi test set dalam preprocessing
  [ ] Cross-validation dilakukan setelah split

Jumlah data akhir : ____________________
Script tersedia   : [ ] Ya → path: ____ | [ ] Belum
```

---

## Latihan 1 — Cleaning Plan

Periksa dataset Anda (atau dataset contoh) dan dokumentasikan masalah yang ditemukan.

| Masalah | Jumlah Kasus | Penanganan | Justifikasi |
|---------|-------------|------------|-------------|
| *Nilai kosong/tak terdefinisi pada fitur numerik (Flow Bytes/s, Flow Packets/s)* | *2.867 baris* | *Listwise deletion (Hapus baris)* | *Proporsi kasus < 0.1% dari total dataset global, distribusi acak, dan pengisian nilai (imputation) berisiko merusak integritas data deteksi anomali.* |
| *Data duplikat (redundant log packets)* | *14.302 baris* | *Drop duplicates* | *Menghindari bias overfitting atau pembobotan berlebih pada pola serangan jaringan yang identik dalam satu sesi log.* |
| *Nilai ekstrem tidak valid (Infinite values)* | *1.124 baris* | *Listwise deletion (Hapus baris)* | *Listwise deletion (Hapus baris)* |


**Jumlah data sebelum cleaning:** 2.830.743 baris
**Jumlah data setelah cleaning:** 2.812.450 baris
**Persentase data yang hilang/berubah:** 0.65%

---

## Latihan 2 — Normalisasi Decision

Tentukan apakah data Anda perlu normalisasi, dan jika ya, metode apa yang tepat.

| Variabel | Range Asli | Distribusi | Outlier? | Metode Normalisasi | Alasan |
|----------|-----------|-----------|----------|-------------------|--------|
| *Flow Duration* | *0 – 120.000.000* | *Right-skewed* | *Ya* | *Robust scaling* | *Rentang sangat lebar dengan pencilan masif pada durasi serangan.* |
| *Total Fwd Packets* | *1 – 200.000* | *Heavy-tailed* | *Ya* | *Robust Scaling* | *Distribusi tidak normal akibat lonjakan paket serangan DDoS.* |
| *Destination Port* | *0 – 65.353* | *Multi-modal* | *Tidak* | *Min-Max Scaling* | *Range nilai port sudah pasti dan memiliki batas atas yang absolut.* |

**Apakah normalisasi diperlukan?** [v] Ya / [ ] Tidak
**Justifikasi:** Fitur lalu lintas jaringan memiliki rentang nilai (scale) yang sangat timpang antar kolom (misalnya perbandingan durasi jutaan mikrodetik vs jumlah paket satuan). Tanpa normalisasi, fitur berangka besar akan mendominasi perhitungan gradien pada Lightweight CNN.
> ___________________________________________________

**Leakage check:**
- [v] Parameter dihitung dari training set saja (Nilai median dan IQR untuk Robust Scaling murni diambil dari subset data training)
- [v] Normalisasi diterapkan setelah train-test split (Menghindari kebocoran informasi distribusi data uji ke model)

---

## Latihan 3 — Preprocessing Report

Buat ringkasan preprocessing lengkap — dokumentasi yang cukup bagi orang lain untuk mereplikasi.

```
PREPROCESSING SUMMARY

1. Dataset: CICIDS2017 Network Traffic
2. Data awal: 2.830.743 records,78 features
3. Cleaning:
   - Missing values: 2.867 kasus, metode: Listwise deletion
   - Duplikat: 14.302 kasus, tindakan: Drop duplicates 
   - Error: 1.124 kasus, tindakan: Listwise deletion
4. Transformation: Label encoding untuk fitur target kategori serangan (Benign, DDoS, PortScan, dll.)
5. Normalisasi: Robust Scaling (fitur berbasis durasi/volume) dan Min-Max (fitur berbasis port), parameter dari Training Set
6. Data akhir: 2.812.450 records, 78 features
7. Leakage check: [v] Lulus / [ ] Ada masalah
```

---

## Refleksi

> Apakah Anda pernah melakukan normalisasi "karena biasa dilakukan" tanpa mempertimbangkan apakah benar-benar diperlukan? Apa risiko over-preprocessing?

> Apakah Anda pernah melakukan normalisasi "karena biasa dilakukan" tanpa mempertimbangkan apakah benar-benar diperlukan?
Ya, di awal belajar kecerdasan buatan, saya sering langsung menerapkan StandardScaler secara buta pada seluruh kolom tanpa melihat visualisasi distribusi atau memeriksa keberadaan pencilan (outliers) terlebih dahulu.
> Apa risiko over-preprocessing?
Risiko terbesar dari over-preprocessing adalah hilangnya informasi esensial dan karakteristik asli data. Jika kita terlalu agresif memotong atau mentransformasikan data (misalnya membuang semua pencilan atau memaksakan distribusi normal pada data yang secara alami bersifat multi-modal), kita justru bisa menghilangkan sinyal anomali kritis dari serangan siber yang strukturnya memang menyerupai pencilan ekstrim. Selain itu, prapemrosesan berlebih menambah kompleksitas komputasi saat implementasi inferensi nyata (deployment), yang bertentangan dengan prinsip efisiensi arsitektur Lightweight CNN.

# WS-11: Data Validation & Integrity

> **Bab 11 — Validasi Data & Integritas**

---

## Ringkasan Materi

### Data Trust Model

```
Raw Data → Data Cleaning → Consistency Check → Validation Process → Trusted Data
```

Data mentah belum bisa dipercaya. Harus melewati pipeline validasi sebelum siap untuk analisis statistik.

### Empat Pilar Data Quality

| Pilar | Deskripsi | Contoh Pelanggaran |
|-------|----------|-------------------|
| **Accuracy** | Nilai dalam range masuk akal | Akurasi = 1.5 (di luar [0,1]) |
| **Consistency** | Format seragam di semua run | Run 1: CSV, Run 2: JSON |
| **Completeness** | Tidak ada data hilang dari plan | 97 dari 100 run tercatat |
| **Validity** | Data sesuai desain eksperimen | Parameter baseline tercampur treatment |

### Proses Validasi Progresif

1. **Format validation** — Tipe file, header, kolom
2. **Range validation** — Nilai dalam batas logis
3. **Consistency validation** — Format seragam antar-run
4. **Logic validation** — Data cocok dengan desain eksperimen

Jika gagal di langkah awal → tidak perlu lanjut.

### Anomaly Detection — 3 Jenis

| Jenis | Deskripsi | Deteksi |
|-------|----------|---------|
| **Statistical outlier** | Nilai di luar distribusi normal | IQR: < Q1-1.5×IQR atau > Q3+1.5×IQR |
| **Contextual anomaly** | Normal absolut, abnormal dalam konteks | Run 1-10: ~91%, Run 11-20: ~88% |
| **Pattern anomaly** | Pola sistematis (bukan random) | Performa menurun berurutan |

**Prinsip:** Detect → Investigate → Document → Decide — **JANGAN langsung hapus.**

### Engineering vs Research Validation

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Tujuan | Data sesuai spesifikasi bisnis | Data layak untuk analisis statistik |
| Missing data | Impute / set default | Investigasi penyebab → dokumentasi |
| Outlier | Bug → fix | Mungkin temuan → investigasi |
| Dokumentasi | Minimal (log error) | Komprehensif (anomali + keputusan) |

### Jebakan Kognitif

1. "Logging otomatis ≠ data benar" → bisa ada bug di logger
2. "Outlier = hapus" → bisa jadi temuan penting
3. "Dataset kecil tidak perlu validasi" → justru lebih rentan
4. "Mean normal = data benar" → [94, 95, 93, **44**, 94] → mean 84% terlihat wajar

---

## Template A.11 — Data Validation Checklist

```
DATA VALIDATION CHECKLIST

Completeness:
  [ ] Semua skenario tercakup
  [ ] Jumlah run sesuai rencana
  [ ] Tidak ada file output hilang
  Missing: ____ dari ____ data points

Format Consistency:
  [ ] Semua file format sama (CSV/JSON/...)
  [ ] Header konsisten
  [ ] Tipe data konsisten (numerik tetap numerik)

Range & Logic:
  [ ] Nilai dalam range masuk akal
  [ ] Tidak ada waktu negatif
  [ ] Metrik 0–100%, tidak di luar range
  Anomali ditemukan: ____________________

Cross-Validation:
  [ ] Run identik → hasil mendekati
  [ ] Trend konsisten dengan ekspektasi teori

Keputusan:
  [ ] Data siap analisis
  [ ] Perlu cleaning
  [ ] Perlu re-run (skenario: ____)
```

---

## Latihan 1 — Completeness Check

Verifikasi apakah semua data yang direncanakan sudah terkumpul.

| Skenario | Run Direncanakan | Run Tercatat | Missing | Alasan |
|----------|-----------------|-------------|---------|--------|
| *Baseline: Standard 1D-CNN* | *1* | *1* | *0* | *-* |
| *Baseline: Hybrid CNN-LSTM* | *1* | *1* | *0* | *-* |
| *Proposed: Lightweight CNN + Attention* | *1* | *1* | *0* | *-* |
| *Proposed: Lightweight CNN + Attn (Seed 999)* | *1* | *1* | *0* | *-* |

**Total expected:** 5 | **Total actual:** 5 | **Missing:** 0

**Keputusan untuk data missing:**
> Tidak ada data yang hilang (missing data). Semua run yang direncanakan pada execution plan berhasil dieksekusi dan tercatat lognya secara lengkap.
---

## Latihan 2 — Anomaly Investigation

Periksa data Anda untuk anomali. Gunakan metode IQR atau z-score.

**Dataset sampel (atau data Anda sendiri):**

| Run | Accuracy (%) |
|-----|-------------|
| 1 | *91.2* |
| 2 | *90.8* |
| 3 | *91.5* |
| 4 | *78.3* |
| 5 | *91.0* |

**Deteksi outlier:**
- Q1 = 90.8 | Q3 = 91.2 | IQR = Q3-Q1= 0.4
- Batas bawah (Q1 - 1.5×IQR) = 90.8 - (1.5 x 0.4) = 90.2
- Batas atas (Q3 + 1.5×IQR) = 91.2 + (1.5 x 0.4) = 91.8
- Outlier terdeteksi: Run 4 (78.3%), karena nilainya berada jauh di bawah batas bawah (90.2).
**Investigasi (untuk setiap outlier):**

| Outlier | Nilai | Kemungkinan Penyebab | Keputusan |
|---------|-------|---------------------|-----------|
| *Run 4* | *78.3* | *Terjadi ketidakseimbangan partisi data (unstratified split) yang ekstrem secara kebetulan akibat benih acak seed 123,* | *Tetap dokumentasikan run asli ini untuk pelaporan varians, namun lakukan re-run paralel dengan pemeriksaan stratified shuffle yang lebih ketat guna memastikan kestabilan metrik.* |

---

## Latihan 3 — Validation Report

Buat laporan validasi ringkas untuk dataset eksperimen Anda.

**1. Completeness:** 100% data terkumpul
**2. Format:** [v] Konsisten / [ ] Ada inkonsistensi: JSON terstruktur secara seragam.
**3. Range check (anomali):** Terdeteksi 1 outlier statistik (Run 4) pada metrik akurasi/F1.
**4. Logic check:** [v] Parameter sesuai plan / [ ] Ada ketidaksesuaian: Seluruh parameter utama ($lr$, $batch$, $epoch$) terekam tepat sesuai rencana awal.

**Kesimpulan:** [v] Data siap analisis / [ ] Perlu tindakan: Data siap dianalisis setelah anomali Run 4 diverifikasi penyebabnya secara logis.

---

## Refleksi

> Apa perbedaan antara "data yang benar" dan "data yang dipercaya"? Mengapa proses validasi formal diperlukan meskipun data dikumpulkan secara otomatis?

> Data yang benar adalah data hasil komputasi apa adanya yang keluar dari skrip (secara sintaks tidak error dan program sukses berjalan).
> Data yang dipercaya (trustworthy data) adalah data yang telah diuji validitas logikanya, terbebas dari bias gangguan lingkungan komputasi (noise perangkat keras), dan konsisten secara metodologis riset.

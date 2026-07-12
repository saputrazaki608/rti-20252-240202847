# WS-03: Literature Mapping & Gap

> **Bab 3 — Literature Review, Research Gap & Baseline**

---

## Ringkasan Materi

### Literature Review = Positioning, Bukan Ringkasan

Literature review bukan merangkum paper satu per satu. Pendekatan yang benar adalah **concept-centric** — organisasi berdasarkan tema, metode, atau variabel. Tujuan: menemukan **pola, kontradiksi, dan gap**.

**Perbandingan pendekatan Author-centric vs Concept-centric:**

| Aspek | Author-centric (Hindari) | Concept-centric (Gunakan) |
|-------|--------------------------|---------------------------|
| Struktur | Per penulis/paper ("Rahman et al. menyatakan...") | Per konsep/metode ("Pendekatan berbasis transformer") |
| Tujuan | Ringkasan isi paper | Perbandingan metode & identifikasi gap |
| Contoh paragraph | "Rahman (2023) pakai CNN. Lee (2022) pakai LSTM. Zhang (2021) pakai RF." | "Tiga pendekatan dominan: CNN digunakan oleh 4 paper untuk representasi fitur visual; LSTM untuk data sekuensial; RF sebagai baseline klasik." |
| Hasil akhir | Daftar paper | Peta pengetahuan + gap yang teridentifikasi |

### Empat Jenis Research Gap

| Jenis Gap | Deskripsi | Contoh |
|-----------|----------|--------|
| **Performance Gap** | Performa belum memadai | Akurasi deteksi hanya 78% pada kasus tertentu |
| **Method Gap** | Pendekatan belum diterapkan | Belum ada yang pakai transformer untuk task ini |
| **Data Gap** | Dataset terbatas/tidak representatif | Semua studi pakai dataset sintetis |
| **Context Gap** | Belum diuji pada konteks berbeda | Belum ada evaluasi di negara berkembang |

Gap terkuat = kombinasi 2+ jenis.

### Systematic Search Strategy

1. **Database utama**: IEEE Xplore, ACM DL, Scopus
   - Akses IEEE/ACM melalui jaringan kampus atau VPN institusi
   - Alternatif bebas biaya: Google Scholar, ResearchGate ([researchgate.net](https://www.researchgate.net)), arXiv ([arxiv.org](https://arxiv.org))
2. **Boolean query** yang terdokumentasi eksplisit
   - Contoh: `("anomaly detection" OR "intrusion detection") AND ("deep learning" OR "neural network") NOT ("medical imaging")`
   - Gunakan tanda kutip untuk frasa eksak; AND/OR/NOT mengontrol scope
3. **Snowballing** — dua arah:
   - **Backward snowballing**: buka daftar referensi di paper kunci → telusuri paper yang dikutip
   - **Forward snowballing**: di Google Scholar, klik "Cited by" di bawah paper kunci → temukan paper yang mengutipnya
   - Ulangi 1–2 tingkat untuk membangun cakupan komprehensif
4. Klaim "belum ada penelitian" harus didukung **bukti pencarian**

### Baseline Selection — 3 Kriteria

| Kriteria | Pertanyaan |
|----------|-----------|
| **Relevan** | Apakah menyelesaikan masalah yang sama? |
| **Representatif** | Apakah mewakili common practice? |
| **State-of-the-Art** | Apakah terbaru/terbaik? |

Membandingkan deep learning 2024 dengan decision tree sederhana tanpa justifikasi = **straw man comparison** (perbandingan tidak jujur).

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan baca literatur | Mencari solusi yang sudah ada | Memahami apa yang belum terjawab |
| Cara membaca paper | Tutorial, how-to | Metode, limitasi, gap |
| Baseline | Framework terpopuler | State-of-the-art yang rigorous |
| Dokumentasi pencarian | Tidak diperlukan | Wajib (reproducible) |

### Istilah Penting

- **Concept-centric** — Organisasi literatur berdasarkan konsep/metode, bukan per penulis
- **Snowballing** — Backward (telusuri referensi) + Forward (cari yang mengutip paper kunci)
- **Research Position** — Pernyataan eksplisit posisi riset terhadap studi sebelumnya
- **Straw man comparison** — Memilih baseline lemah agar metode sendiri terlihat lebih baik

---

## Template A.3 — Literature Mapping & Gap Identification

```
LITERATURE MAPPING

Topik      : ____________________
Database   : ____________________
Query      : ____________________
Tahun      : ____________________
Hasil awal : ____ paper → Screening → ____ paper final

Literature Matrix (concept-centric):

| Study | Tahun | Method | Data | Result | Limitation |
|-------|-------|--------|------|--------|------------|
|       |       |        |      |        |            |

Pola yang ditemukan:
  Metode dominan     : ____________________
  Dataset umum       : ____________________
  Limitasi berulang  : ____________________

GAP IDENTIFICATION

Gap 1: [Jenis: performance / method / data / context]
  Deskripsi    : ____________________
  Bukti        : ____________________
  Signifikansi : ____________________

Gap 2: [Jenis: ____]
  Deskripsi    : ____________________
  Bukti        : ____________________
  Signifikansi : ____________________

Baseline Selection:
| Baseline | Relevansi | Representatif | Source |
|----------|-----------|---------------|--------|
|          |           |               |        |
```

---

## Latihan 1 — Concept-Centric Literature Table

Gunakan topik riset dari WS-02. Cari minimal 5 paper relevan menggunakan database akademik.

> **Panduan pencarian:**
> - Database: IEEE Xplore, ACM DL, Google Scholar, atau ResearchGate
> - Tulis query Boolean yang digunakan: contoh `("object detection" OR "image classification") AND ("edge computing") NOT ("medical")`. Dokumentasikan query secara eksplisit.
> - Akses gratis: buka Google Scholar → cari judul paper → klik [PDF] jika tersedia, atau akses lewat campus VPN

**Topik riset:** *Optimasi Efisiensi Model CNN untuk Deteksi Anomali Lalu Lintas Jaringan*
**Query pencarian:** *("intrusion detection" OR "anomaly detection") AND ("CNN" OR "deep learning") AND ("efficient" OR "lightweight" OR "latency")*
**Database:** *IEEE Xplore / Google Scholar*
| # | Study | Tahun | Method | Dataset | Result | Limitasi |
|---|-------|-------|--------|---------|--------|----------|
| 1 | *Khan et al.* | *2021* | *Efficient Lightweight CNN* | *NSL-KDD, CIDIDS-001* | *Akurasi 98.54%, mereduksi waktu komputasi secara signifikan.* | *Hanya diuji pada fitur ekstraksi statis, belum mencakup zero-day attack dinamis.* |
| 2 | *Imrana et al.* | *2021* | *Hybrid CNN-LSTM* | *NSL-KDD* | *Akurasi 99.2%, unggul dalam mendeteksi serangan temporal.* | *Struktur hybrid meningkatkan memory overhead saat deployment.* |
| 3 | *Binbusayyis et al.* | *2022* | *1D-CNN + Unsupervised IDBS* | *UNSW-NB15, CICIDS2017* | *F1-Score 97.8%, sangat cepat memproses lalu lintas padat.* | *Performa menurun drastis pada data dengan tingkat class imbalance ekstrem.* |
| 4 | *Henry et al.* | *2023* | *Lightweight CNN with Gabor Filter* | *Edge-IIoTset* | *Akurasi 96.3% pada perangkat edge computing.* | *Ada sedikit penurunan akurasi demi mengejar efisiensi waktu deteksi.* |
| 5 | *Jiang et al.* | *2024* | *Ghost-CNN (Model Compression)* | *CICIDS2017, CSE-CIC-IDS2018* | *Parameter terpangkas 45%, Akurasi stabil di 98.1%.* | *Memerlukan fase prapemrosesan data (feature engineering) yang cukup kompleks.* |

**Pola yang terlihat — Metode dominan:**  *Penggunaan arsitektur 1D-CNN (One-Dimensional CNN) atau teknik kompresi model (seperti Ghost Module dan integrasi arsitektur hibrida) menjadi pilihan utama untuk mengeksplorasi fitur lokal log jaringan secara cepat dan efisien.*
**Limitasi yang berulang:** *Sebagian besar studi masih sangat bergantung pada dataset publik statis (seperti NSL-KDD atau CICIDS2017) yang terkadang tidak merepresentasikan kompleksitas variasi serangan siber baru, serta performa model yang rentan menurun ketika dihadapkan pada masalah ketidakseimbangan data (class imbalance) yang parah.*
---

## Latihan 2 — Gap Identification

Berdasarkan tabel di Latihan 1, identifikasi gap.

| Jenis Gap | Ditemukan? | Gap Statement |
|-----------|-----------|---------------|
| Performance Gap | [v] Ya / [ ] Tidak | *Model CNN ringan (lightweight) berhasil memangkas waktu komputasi, namun mengalami penurunan akurasi (F1-score) yang signifikan pada pendeteksian kategori serangan minoritas.* |
| Method Gap | [v] Ya / [ ] Tidak | *Kebanyakan metode reduksi parameter mengandalkan arsitektur 1D-CNN sederhana yang gagal menangkap dependensi temporal jangka panjang dari paket data jaringan.* |
| Data Gap | [v] Ya / [ ] Tidak | *Evaluasi model mayoritas masih bergantung pada dataset statis berbasis lab (seperti NSL-KDD atau CICIDS2017) yang belum mencakup karakteristik serangan siber terbaru.Context Gap[X] Ya / [ ] TidakPengujian performa model lebih banyak dilakukan pada simulasi lingkungan lokal (controlled environment) dan jarang dievaluasi secara langsung pada perangkat keras edge gateway industri yang sesungguhnya.* |
| Context Gap | [v] Ya / [ ] Tidak | *Pengujian performa model lebih banyak dilakukan pada simulasi lingkungan lokal (controlled environment) dan jarang dievaluasi secara langsung pada perangkat keras edge gateway industri yang sesungguhnya.* |

**Gap utama yang dipilih:** *Performance Gap dan Method Gap*
**Mengapa gap ini penting (bukan sekadar "belum ada yang meneliti")?**
> *Dalam ranah keamanan siber, sebuah model IDS berbasis AI tidak boleh mengorbankan tingkat keandalan deteksi demi mengejar kecepatan operasional. Jika model dibuat terlalu ringan dengan mengabaikan pola ekstraksi fitur temporal yang mendalam (Method Gap), maka serangan-serangan berbahaya berskala kecil namun kritikal (kelas minoritas) akan lolos dari penyaringan sistem (Performance Gap). Mengatasi celah ini sangat krusial untuk memastikan infrastruktur jaringan tetap terlindungi secara optimal tanpa membebani kapasitas komputasi perangkat edge.*

---

## Latihan 3 — Baseline Selection

Pilih 2 baseline dari literatur yang sudah dibaca.

| # | Baseline | Mengapa Relevan | Mengapa Representatif | Apakah SOTA? | Sumber |
|---|----------|----------------|----------------------|-------------|--------|
| 1 | *Standard 1D-CNN* | *Task sama: Ekstraksi fitur log paket jaringan untuk deteksi anomali secara otomatis.* | *Merupakan arsitektur paling mendasar yang dipakai oleh mayoritas paper pembanding.* | *Bukan, tapi merupakan common practice / fondasi utama.* | *Khan et al., 2021* |
| 2 | *Hybrid CNN-LSTM* | *Task sama: Menangkap pola spasial sekaligus hubungan temporal paket jaringan.* | *Sering digunakan sebagai pembanding tingkat lanjut untuk menguji akurasi vs efisiensi.* | *Ya, salah satu State-of-the-Art (SOTA) untuk deteksi berbasis deret waktu.* | *Imrana et al., 2021* |

**Apakah pemilihan baseline ini bisa dianggap straw man?** [ ] Ya / [v] Tidak
> Justifikasi: *Pemilihan ini bukan straw man fallacy karena kita tidak memilih model lemah yang sengaja diatur agar mudah dikalahkan. Sebaliknya, kita menyandingkan model usulan kita langsung dengan arsitektur standar industri (Standard 1D-CNN) serta model dengan akurasi SOTA (CNN-LSTM). Ini merupakan uji komparasi yang adil dan valid untuk membuktikan apakah model baru kita benar-benar berhasil memangkas latensi tanpa merusak akurasi secara drastis.*

---

## Refleksi

> Apa perbedaan antara "belum ada yang meneliti ini" (klaim tanpa bukti) dengan research gap yang valid? Bagaimana cara membuktikan bahwa sebuah gap benar-benar ada?

**Jawaban:**
> *Klaim Tanpa Bukti ("Belum ada yang meneliti ini"): Sering kali muncul dari asumsi sepihak karena kurangnya membaca literatur. Pernyataan ini bersifat spekulatif dan rentan salah, karena bisa jadi topik tersebut sebenarnya sudah banyak diteliti namun menggunakan istilah atau kata kunci pencarian yang berbeda.*

>*Research Gap yang Valid: Merupakan kesenjangan yang ditemukan secara objektif setelah memetakan literatur secara kritis. Gap ini menunjukkan batasan performa (performance limit), inkonsistensi hasil eksperimen terdahiva, atau kelemahan metodologi tertentu dari solusi-solusi yang sudah ada.*

*Cara Membuktikan Bahwa Sebuah Gap Benar-Benar Ada:*

*Gap dapat dibuktikan dengan menyusun Tabel Literatur Berorientasi Konsep (Concept-Centric Literature Table) seperti yang dilakukan di latihan sebelumnya. Dari tabel tersebut, kita mengelompokkan temuan-temuan peneliti terdahulu, lalu menunjukkan bukti empiris berulang (misalnya: menunjukkan data bahwa 5 paper SOTA terakhir semuanya mengalami penurunan akurasi pada kelas minoritas demi mengejar kecepatan). Dengan menyajikan kutipan, data metrik, dan keterbatasan dari paper-paper bereputasi, eksistensi gap tersebut menjadi tidak terbantahkan secara ilmiah.*

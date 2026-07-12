# WS-08: Proposal Integration (UTS)

> **Bab 8 — Proposal & Checkpoint**

---

## Ringkasan Materi

### Proposal = Satu Argumen Utuh

Proposal riset bukan kumpulan bab yang independen. Ia adalah **satu argumen** yang mengalir dari masalah ke rencana solusi. Jika satu koneksi putus, seluruh proposal kehilangan koherensi.

### Integration Map — 6 Koneksi Kritis

```
Problem (Bab 2) → Gap (Bab 3) → RQ & H (Bab 4) → Metrik (Bab 5) → Sistem (Bab 6) → Eksperimen (Bab 7)
```

| Koneksi | Pertanyaan Verifikasi |
|---------|----------------------|
| Problem → Gap | Apakah gap muncul dari analisis literatur terhadap masalah? |
| Gap → RQ | Apakah RQ langsung menjawab gap yang teridentifikasi? |
| RQ → Metrik | Apakah setiap variabel di RQ punya metrik terdefinisi? |
| Metrik → Sistem | Apakah setiap metrik bisa diukur oleh komponen sistem? |
| Sistem → Eksperimen | Apakah desain eksperimen menggunakan sistem sebagai instrumen? |

### Koherensi Vertikal + Horizontal

- **Vertikal** — Alur logis atas-ke-bawah (problem → experiment). Setiap section menjawab pertanyaan yang diangkat section sebelumnya dan memunculkan pertanyaan baru.
- **Horizontal** — Konsistensi terminologi (nama variabel di RQ = di hipotesis = di metrik = di desain)

**Operasionalisasi Red Thread** (benang merah):
```
Bab 2 (Problem) → | memperkenalkan masalah X + evidensi |
                          ↓ menimbulkan pertanyaan: "apa akar gap-nya?"
Bab 3 (Gap)     → | menjawab pertanyaan tadi + membuka "lalu apa yang perlu diteliti?" |
                          ↓
Bab 4 (RQ/H)    → | menjawab gap dengan pertanyaan spesifik + prediksi terukur |
                          ↓
Bab 5-7 (Method)→ | menjawab RQ melalui desain eksperimen yang tepat |
```
Jika ada lompatan (section B tidak menjawab pertanyaan section A), red thread putus.

### Jebakan Kognitif

| Jebakan | Deskripsi |
|---------|----------|
| "Selling" Introduction | Menulis promosi, bukan menyajikan data dan gap |
| Copy-paste Methodology | Menyalin deskripsi tekstbook tanpa menyesuaikan ke RQ |
| Optimistic Timeline | Meremehkan waktu implementasi; selalu tambah buffer 30-50% |
| No Possibility of Failure | Mengimplikasikan hasil pasti sukses — proposal jujur mengakui H₀ mungkin tidak ditolak |

### Struktur Proposal

1. **Pendahuluan** — Latar belakang + problem statement (Bab 1-2)
2. **Tinjauan Pustaka** — Literature review + gap + baseline (Bab 3)
3. **RQ / Kontribusi / Hipotesis** — (Bab 4)
4. **Metodologi** — Metrik + sistem + desain eksperimen (Bab 5-7)
5. **Timeline & Output**

### Istilah Penting

- **Integration Map** — Diagram 6 koneksi kritis antar komponen proposal
- **Vertical Coherence** — Alur logis atas-ke-bawah
- **Horizontal Coherence** — Konsistensi terminologi di semua bagian
- **Checkpoint** — Titik self-assessment sebelum transisi dari desain ke eksekusi

---

## Template A.8 — Integration Checklist

```
PROPOSAL INTEGRATION CHECKLIST

Koneksi Vertikal (Flow Atas-Bawah):
  [ ] Problem → Gap: masalah terdokumentasi di literatur
  [ ] Gap → RQ: pertanyaan menjawab gap spesifik
  [ ] RQ → Hypothesis: hipotesis memprediksi jawaban
  [ ] Hypothesis → Metric: metrik mengukur variabel dalam hipotesis
  [ ] Metric → System: komponen sistem menghasilkan/mengukur metrik
  [ ] System → Experiment: desain eksperimen menggunakan sistem

Koneksi Horizontal (Konsistensi):
  [ ] Istilah sama di semua bagian
  [ ] Variabel di RQ = variabel di hipotesis = metrik di desain
  [ ] Scope tidak berubah dari masalah ke eksperimen

Cognitive Trap Checklist:
  [ ] Tidak ada paragraf "promosi" di pendahuluan (hanya data & gap)
  [ ] Metodologi disesuaikan ke RQ, bukan copy-paste textbook
  [ ] Timeline sudah ditambah buffer 30-50% dari estimasi awal
  [ ] Proposal mengakui kemungkinan H0 tidak ditolak (honest uncertainty)
  [ ] Tidak ada klaim "pasti berhasil" atau "meningkatkan signifikan"

Rubrik Self-Assessment:
| Kriteria     | 1 (Lemah)                                        | 2 (Cukup)                                     | 3 (Baik)                                           | Skor |
|------------- |--------------------------------------------------|-----------------------------------------------|----------------------------------------------------|------|
| Koherensi    | >2 koneksi vertikal terputus                     | 1-2 koneksi lemah, argumen masih bisa diikuti | Semua 6 koneksi terhubung, red thread jelas        |      |
| Specificity  | Variabel/metrik masih abstrak, tidak ada angka   | Sebagian metrik terdefinisi numerik           | Semua metrik + threshold + unit pengukuran jelas   |      |
| Feasibility  | Timeline >6 bulan tanpa memperhitungkan sumber   | Timeline 3-6 bulan dengan asumsi tertentu     | Timeline 1-3 bulan realistis dengan rencana detail |      |
| Rigor        | Baseline tidak jelas atau straw man              | 1-2 baseline dengan justifikasi partial       | 2+ baseline SOTA + justifikasi pemilihan lengkap   |      |
```

---

## Latihan 1 — Kompilasi Proposal Mini

Kumpulkan hasil dari WS-02 sampai WS-07 menjadi satu ringkasan proposal.

| Komponen | Sumber | Isi (1-2 kalimat) |
|----------|--------|-------------------|
| Problem Statement | WS-02 | *Contoh: Sistem rekomendasi memiliki akurasi tinggi (RMSE 0.87) tetapi satisfaction score rendah (45/100). Gap antara metrik teknis dan kepuasan pengguna belum diteliti.* |
| Gap | WS-03 | *Model deep learning untuk deteksi anomali jaringan sering kali mengorbankan kecepatan komputasi demi mengejar akurasi tinggi. Akibatnya, model menjadi terlalu lambat untuk mendeteksi ancaman secara real-time di tingkat edge gateway.* |
| RQ | WS-04 | *Belum ada penelitian yang mengintegrasikan pemangkasan struktural Lightweight CNN dengan Temporal Attention Mechanism untuk mempercepat pemrosesan tanpa menurunkan akurasi deteksi kelas serangan minoritas.* |
| Hipotesis | WS-04 | *Bagaimana performa arsitektur Lightweight CNN yang diintegrasikan dengan Temporal Attention Mechanism dalam menurunkan inference latency sekaligus mempertahankan nilai F1-score pada kelas serangan minoritas jika dibandingkan dengan arsitektur Standard 1D-CNN dan Hybrid CNN-LSTM menggunakan dataset CICIDS2017?* |
| Variabel & Metrik | WS-05 | *$H_1$: Arsitektur Lightweight CNN + Temporal Attention mampu menurunkan inference latency hingga $\le 2$ ms per paket dengan tetap mempertahankan nilai F1-score $\ge 0.85$ pada kelas serangan minoritas dibandingkan model baseline.* |
| Sistem | WS-06 | *Sistem dirancang menggunakan arsitektur modular yang memisahkan blok Lightweight CNN backbone, komponen Temporal Attention, modul metrik evaluasi, serta subsistem prapemrosesan data.* |
| Desain Eksperimen | WS-07 | *Eksperimen berbasis komparasi (controlled comparison) formal menggunakan subset dataset CICIDS2017 terstandardisasi dengan pengujian waktu iteratif (30 kali pengulangan) di dalam environment Docker terisolasi.* |

---

## Latihan 2 — Integration Checklist

Verifikasi 6 koneksi kritis. Isi dengan merujuk tabel di Latihan 1.

| Koneksi | Status | Bukti |
|---------|--------|-------|
| Problem → Gap | *✅ Terpenuhin* | *Problem membahas dilema akurasi vs kecepatan, dan Gap langsung menawarkan solusi penyeimbang berupa struktur model ringkas bermekanisme atensi.* |
| Gap → RQ | *✅ Terpenuhin* | *RQ secara eksplisit mempertanyakan dampak dari integrasi komponen yang didefinisikan pada Gap terhadap aspek latensi dan skor F1.* |
| RQ → Hypothesis |  *✅ Terpenuhin* | *Hypothesis ($H_1$) memberikan jawaban terukur (nilai konkret $\le 2$ ms dan F1 $\ge 0.85$) terhadap pertanyaan yang diajukan di dalam RQ.* |
| Hypothesis → Metric |  *✅ Terpenuhin* | *Batasan numerik pada hipotesis diuji menggunakan metrik operasional yang klop, yaitu milidetik (latency) dan nilai presisi matriks konfusi (F1-score).* |
| Metric → System |  *✅ Terpenuhin* | *Sistem memiliki modul pengekstraksi khusus (benchmark performance dan metrics calculator) untuk merekam variabel DV secara langsung saat uji coba dijalankan.* |
| System → Experiment |  *✅ Terpenuhin* | *Sifat sistem yang modular memungkinkan skrip desain eksperimen menukar jenis arsitektur (IV) secara otomatis melalui argumen baris perintah.* |

**Koneksi mana yang paling lemah?** Hypothesis → Metric
**Bagaimana cara memperkuatnya?**
Memastikan bahwa ambang batas keberhasilan hipotesis (seperti $\le 2$ ms) didasarkan pada standar industri nyata untuk pelabelan paket real-time, bukan sekadar tebakan angka acak, serta mendokumentasikan formula matematika perhitungan F1-score kelas minoritas secara ketat di kode pengujian.

**Konsistensi horizontal — apakah istilah dan scope konsisten?** [v] Ya / [ ] Tidak
> Jika tidak, di bagian mana terjadi inkonsistensi? — (Sangat konsisten, fokus tidak bergeser dari korelasi Lightweight CNN, Attention, Latency, dan F1-score kelas serangan minoritas).

---

## Latihan 3 — Rubrik Self-Assessment

Evaluasi proposal mini menggunakan rubrik.

| Kriteria | Skor (1-3) | Justifikasi |
|----------|-----------|-------------|
| Koherensi | *3* | *Alur logika dari penemuan masalah di literatur awal hingga penentuan metode uji di desain eksperimen saling mengunci tanpa ada variabel asing yang tiba-tiba muncul.* |
| Specificity | *3* | *Semua metrik evaluasi performa model didefinisikan secara kuantitatif, numerik, dan objektif lengkap beserta metode ekstraksinya.* |
| Feasibility | *3* | *Dataset tersedia secara publik (CICIDS2017) dan seluruh infrastruktur eksperimen komputasi dapat dieksekusi secara mandiri menggunakan perangkat komputasi laboratorium yang ada.* |
| Rigor | *3* | *Pengujian dilengkapi langkah mitigasi bias (isolasi kontainer), uji pengulangan performa, serta direncanakan menggunakan uji signifikansi statistik formal.* |

**Skor total:**  12/12

**Apakah proposal siap untuk fase eksekusi?** [v] Ya / [ ] Belum
> Jika belum, apa yang perlu diperbaiki? — (Proposal sudah matang, koheren, dan siap ditranslasikan langsung ke dalam implementasi baris kode).

---

## Refleksi

> Dari seluruh proses WS-01 sampai WS-08, bagian mana yang paling mudah dan paling sulit? Mengapa? Apa yang akan dilakukan berbeda jika mengulang dari awal?

**Bagian termudah:** Memetakan arsitektur model dan variabel eksperimen (WS-05 dan WS-06), karena penulisan komponen sistem sangat linear dengan logika pemrograman deep learning objek-terorientasi sehari-hari.
**Bagian tersulit:** Mengisolasi ancaman validitas (threat to validity pada WS-07) dan merumuskan deskripsi masalah yang sempit (narrow gap pada WS-03). Memastikan bahwa riset ini tidak bias dan benar-benar belum dikerjakan orang lain membutuhkan ketelitian tinggi saat membedah literatur.
**Yang akan dilakukan berbeda:**
> Saya akan membangun kontainer Docker lingkungan eksperimen dan fungsi pembersih prapemrosesan dataset (data pipeline) terlebih dahulu di minggu pertama, sebelum melakukan spesifikasi mendalam pada struktur arsitektur modelnya, agar variabilitas data mentah dapat dipahami lebih cepat.

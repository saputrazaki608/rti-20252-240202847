# WS-15: Scientific Writing

> **Bab 15 — Penulisan Ilmiah**

---

## Ringkasan Materi

### Scientific Argument Flow

```
Problem → Gap → RQ → Method → Result → Analysis → Conclusion → Contribution
```

Paper ilmiah adalah **satu argumen utuh** dari masalah ke kontribusi. Setiap node harus terhubung logis ke node sebelum dan sesudahnya.

### Struktur IMRAD

| Section | Peran | Pertanyaan Kunci |
|---------|-------|-----------------|
| **Introduction** | Motivasi + frame | Why is this needed? |
| **Method** | Deskripsi (reproducible) | How was it done? |
| **Results** | Laporan objektif | What was found? |
| **Discussion** | Interpretasi + refleksi | What does it mean? |
| **Conclusion** | Ringkasan + kontribusi | So what? |

### Logical Flow — "Red Thread"

Setiap paragraf menjawab satu pertanyaan dan memicu pertanyaan berikutnya. Alur logis ini harus terasa di tiga level:
1. **Antar-kalimat** dalam paragraf
2. **Antar-paragraf** dalam section
3. **Antar-section** dalam paper

### Internal Consistency

Setiap elemen yang dijanjikan di Introduction harus hadir di Discussion/Conclusion.

**Consistency Matrix:**
```
           Intro  Method  Result  Discuss  Conclude
RQ1          ✓      ✓       ✓       ✓        ✓
RQ2          ✓      ✓       ✓       ✗ ←      ✓
Metrik-X     ✗      ✗       ✓ ←     ✗        ✗
```
**Masalah:** RQ2 dibahas di semua bagian kecuali Discussion. Metrik-X muncul di Result tapi tidak diperkenalkan di Method.

### Writing Quality Triad

| Kualitas | Deskripsi | Contoh Buruk → Baik |
|----------|----------|---------------------|
| **Clarity** | Dipahami sekali baca | "Performa meningkat" → "Accuracy meningkat dari 85.3% ke 89.7%" |
| **Precision** | Istilah eksak, tanpa ambiguitas | "signifikan" → "signifikan secara statistik (p=0.003, d=1.2)" |
| **Conciseness** | Setiap kata menambah informasi | Hapus kalimat redundan, filler words |

### Urutan Penulisan yang Disarankan

1. **Method & Results** — paling stabil, tulis pertama
2. **Discussion** — interpretasi berdasarkan hasil
3. **Introduction** — frame sesuai temuan aktual
4. **Abstract & Conclusion** — terakhir

### Target Jumlah Kata

| Section | Target |
|---------|--------|
| Introduction | 500–700 |
| Related Work | 700–1000 |
| Method | 800–1200 |
| Results | 500–800 |
| Discussion | 600–900 |
| Conclusion | 200–400 |

### Jebakan Kognitif

1. "Lebih panjang = lebih lengkap" → conciseness lebih berharga
2. "Introduction harus ditulis pertama" → justru ditulis terakhir
3. "Jargon teknis = lebih ilmiah" → clarity lebih penting
4. "Discussion = ringkasan Results" → Discussion = interpretasi + konteks

---

## Template A.15 — Paper Structure Checklist

```
PAPER STRUCTURE CHECKLIST

Title   : ____________________
Target  : [ ] Jurnal  [ ] Konferensi  [ ] Laporan

Section Check:
  [ ] Abstract — masalah, metode, hasil utama, kontribusi (max 250 kata)
  [ ] Introduction — konteks → gap → RQ → kontribusi → struktur paper
  [ ] Related Work — concept-centric, gap positioning
  [ ] Method — reproducible: desain, variabel, metrik, setup, prosedur
  [ ] Results — tabel + grafik + observasi (tanpa interpretasi)
  [ ] Discussion — interpretasi, perbandingan, implikasi, limitation
  [ ] Conclusion — jawaban RQ, kontribusi, future work

Consistency Matrix:
  [ ] RQ di Introduction = RQ di Method = RQ di Conclusion
  [ ] Variabel di Method = variabel di Results
  [ ] Klaim di Discussion didukung data di Results
  [ ] Limitasi di Discussion di-address di Conclusion/Future Work

Writing Quality:
  [ ] Clarity — mudah dipahami tanpa re-read
  [ ] Precision — tidak ada istilah ambigu
  [ ] Conciseness — tidak ada kalimat redundan
```

---

## Latihan 1 — Paper Outline

Buat outline paper untuk riset Anda menggunakan struktur IMRAD.

| Section | Konten Utama (2-3 kalimat) | Target Kata |
|---------|---------------------------|------------|
| Abstract | *Model deep learning konvensional untuk IDS sering kali menuntut daya komputasi tinggi sehingga sulit diterapkan pada infrastruktur jaringan edge. Penelitian ini mengusulkan arsitektur Lightweight CNN dengan integrasi Temporal Attention untuk meningkatkan deteksi serangan siber pada dataset CICIDS2017. Hasil pengujian menunjukkan peningkatan F1-score hingga 87.25% dengan inference latency rendah sebesar 1.84ms.* | 200-250 |
| Introduction | *Konteks: Pertumbuhan lalu lintas data meningkatkan risiko intrusi jaringan berbahaya yang memerlukan sistem deteksi real-time. Gap: Model deteksi saat ini terlalu berat (heavyweight) dan sering mengabaikan ketergantungan temporal paket data kecil. RQ: Apakah integrasi mekanisme atensi pada arsitektur CNN ringan mampu menjaga akurasi klasifikasi serangan tanpa membebani latensi komputasi fisik?* | 500-700 |
| Related Work | *Bagian ini mengulas literatur optimasi model deteksi anomali menggunakan arsitektur 1D-CNN tradisional dan hibrida CNN-LSTM. Kami memetakan keterbatasan metode terdahulu dalam menyeimbangkan antara nilai ukuran parameter model dan akurasi deteksi pada kelas minoritas. Fokus diberikan pada bagaimana mekanisme atensi ringan diadopsi pada ranah selain deteksi jaringan untuk menjadi basis solusi kami.* | 700-1000 |
| Method | *Menjelaskan alur prapemrosesan dataset CICIDS2017 mulai dari pembersihan data tak valid hingga teknik Robust Scaling untuk mengatasi outlier masif. Selanjutnya, dijabarkan rincian arsitektur Lightweight 1D-CNN usulan beserta formulasi matematis lapisan temporal attention yang disisipkan. Prosedur evaluasi dibahas dengan menetapkan skenario pengujian replikasi run acak berdasar parameter variasi seed.* | 800-1200 |
| Results | *Menyajikan visualisasi data dan performa numerik komparatif antara model usulan terhadap baseline standard 1D-CNN dan Hybrid CNN-LSTM. Bagian ini memuat tabel metrik evaluasi makro (F1-Score, akurasi) serta metrik fisik berupa inference latency per paket log. Signifikansi perbedaan performa diperkuat secara valid melalui lampiran hasil uji statistik Kruskal-Wallis.* | 500-800 |
| Discussion | *Menginterpretasikan temuan bahwa efisiensi komputasi model berhasil dicapai berkat reduksi parameter arsitektur CNN tanpa mengorbankan sensitivitas deteksi kelas minoritas. Bagian ini juga menganalisis keterbatasan (boundary conditions) model saat berhadapan dengan ketimpangan data kelas ekstrim. Diulas pula korelasi praktis hasil eksperimen ini terhadap potensi implementasi perangkat keras berspesifikasi rendah.* | 600-900 |
| Conclusion | *Penelitian ini berhasil membuktikan bahwa arsitektur Lightweight CNN yang dikombinasikan dengan temporal attention mampu menjadi solusi deteksi anomali jaringan yang cepat dan akurat. Model usulan mencapai performa optimal yang seimbang antara efisiensi latensi dan ketajaman klasifikasi. Pengembangan ke depan disarankan berfokus pada integrasi arsitektur dengan penyeimbang data adaptif langsung di tingkat jaringan lokal.* | 200-400 |

---

## Latihan 2 — Consistency Matrix

Buat consistency matrix untuk memverifikasi internal consistency paper Anda.

|  | Intro | Method | Result | Discussion | Conclusion |
|--|-------|--------|--------|-----------|-----------|
| *Contoh: RQ1* | *✓* | *✓* | *✓* | *✓* | *✓* |
| *Contoh: Metrik-X* | *✗ ←* | *✗ ←* | *✓* | *✗ ←* | *✗ ←* |
| RQ1 | v | v | v | v | v |
| RQ2 | v | v | v | v | v |
| Metrik utama | v | v | v | v | v |
| Variabel IV | v | v | v | v | v |
| Variabel DV | v | v | v | v | v |
| Klaim/kontribusi | v | v | v | v | v |

**Isi setiap sel:** ✓ (ada & konsisten), ✗ (missing), ~ (ada tapi inkonsisten)

**Inkonsistensi yang ditemukan:**
> Tidak ditemukan inkonsistensi structural yang berarti; semua variabel terikat dan metrik performa komputasi yang dijanjikan pada bagian Introduction telah diukur di Method dan dijawab tuntas pada Results hingga Conclusion.

**Tindakan perbaikan:**
> Memastikan penggunaan istilah penamaan komponen arsitektur tetap seragam di seluruh bab (misal: konsisten menggunakan istilah "Lightweight CNN" dan bukan berganti-ganti menjadi "Reduced CNN").

## Latihan 3 — Writing Quality Check

Ambil satu paragraf dari tulisan Anda (atau tulis paragraf baru) dan evaluasi kualitasnya.

**Paragraf asli:**
> Model baru ini menunjukkan performa yang sangat bagus sekali saat dites menggunakan data log jaringan CICIDS2017. Latensinya berkurang drastis sehingga kecepatannya menjadi lebih cepat daripada model baseline lama yang dipasang di komputer server kemarin.

| Kriteria | Evaluasi | Perbaikan |
|----------|---------|-----------|
| Clarity | *Frasa "performa yang sangat bagus sekali" bersifat subjektif dan tidak memberikan gambaran metrik performa apa yang dimaksud (apakah akurasi, precision, atau recall).* | *Ubah menjadi: "menunjukkan peningkatan skor F1-score kelas minoritas sebesar 87.25%"* |
| Precision | *Penggunaan kata "berkurang drastis", "lebih cepat", dan "di komputer server kemarin" tidak ilmiah karena tidak menyertakan angka eksak spesifik serta spesifikasi lingkungan uji fisik.* | *Ubah menjadi: "...mereduksi inference latency sebesar 55.3% (1.84ms) dibandingkan arsitektur Hybrid CNN-LSTM."* |
| Conciseness | *Kalimat terlalu bertele-tele dan tidak baku (bagus sekali, kemarin), mengurangi ketegasan argumentasi teknis riset.* | *Memadatkan kalimat menjadi satu pernyataan padat berbasis data kuantitatif yang efisien.* |

**Paragraf setelah perbaikan:**
> Arsitektur model usulan menghasilkan peningkatan F1-score hingga 87.25% pada dataset CICIDS2017. Selain itu, model ini secara signifikan mampu mereduksi rentang inference latency menjadi 1.84 ± 0.08ms, atau 55.3% lebih efisien dibandingkan arsitektur baseline Hybrid CNN-LSTM.

---

## Refleksi

> Apa perbedaan antara menulis "tentang" riset dan menulis sebagai "argumen" riset? Bagaimana urutan penulisan (Method → Discussion → Introduction) mengubah kualitas tulisan?
Menulis "tentang" riset cenderung hanya bersifat deskriptif dan naratif, seperti sekadar melaporkan kronologi aktivitas atau menceritakan deretan angka yang keluar dari program tanpa arah yang jelas. Sebaliknya, menulis sebagai "argumen" riset berarti memposisikan setiap kalimat, data, dan hasil uji statistik sebagai bukti logis untuk mempertahankan sebuah klaim ilmiah atau menjawab urutan rumusan masalah. Kita tidak hanya memberi tahu pembaca apa hasilnya, melainkan meyakinkan pembaca mengapa hasil tersebut valid, penting, dan apa dampaknya bagi khazanah keilmuan.
> Bagaimana urutan penulisan (Method --> Result --> Discussion --> Introduction) mengubah kualitas tulisan?
Urutan penulisan terbalik ini (paradigma berbasis data terlebih dahulu) meningkatkan kualitas tulisan secara drastis karena mencegah terjadinya inkonsistensi logis atau klaim berlebihan (overclaiming). Dengan menulis Method dan Result terlebih dahulu, kita menapakkan kaki pada realitas data riil yang objektif dari eksperimen yang sudah terjadi. Setelah fondasi angka itu kokoh dan dianalisis dalam Discussion, kita baru merumuskan arah narasi Introduction. Pola ini memastikan bahwa masalah (gap) dan pertanyaan penelitian (RQ) yang kita bangun di awal tulisan benar-benar sinkron, realistis, dan terjawab secara presisi oleh hasil data eksperimen di akhir lembar karya ilmiah.

# Product Requirements Document (PRD)
## Digital Invitation Platform — "Undangan.Digital"

---

## 1. Document Information

| Atribut | Keterangan |
|---|---|
| **Nama Proyek** | Digital Invitation Platform (nama produk: **Undangan.Digital**) |
| **Repository** | `digital-invitation-platform` |
| **Lokasi Dokumen** | `docs/01-planning/01-PRD.md` |
| **Jenis Proyek** | Enterprise SaaS + E-Commerce Platform untuk Undangan Digital |
| **Tech Stack Utama** | Backend: **Laravel 12** (PHP) · Frontend: **Next.js** · Infrastruktur: Docker, Nginx |
| **Versi Dokumen** | 1.1 |
| **Penulis** | Product Management Office — Undangan.Digital |
| **Status** | Draft — Menunggu Review Stakeholder |
| **Terakhir Diperbarui** | 06 Juli 2026 |
| **Klasifikasi Dokumen** | Internal — Confidential |
| **Siklus Review** | Bulanan, atau setiap kali terjadi perubahan besar pada scope produk |
| **Pemilik Dokumen (Document Owner)** | Head of Product |

### 1.1 Tujuan Dokumen

Dokumen ini adalah **master document** di dalam folder `docs/` yang menyelaraskan seluruh dokumentasi teknis proyek Digital Invitation Platform — SRS, SAD, Database Architecture, API Specification, dan UI/UX Guideline. Setiap keputusan arsitektur, desain, maupun engineering yang tertuang di dokumen-dokumen turunan tersebut harus dapat ditelusuri (*traceable*) kembali ke kebutuhan bisnis dan produk yang dijabarkan di sini. Jika terjadi ketidaksesuaian antara dokumen turunan dengan PRD ini, maka PRD ini yang menjadi rujukan utama (*source of truth*) sampai terjadi revisi resmi melalui proses change management.

### 1.2 Peta Dokumentasi (`docs/`)

PRD ini menjadi akar referensi bagi seluruh dokumen berikut di repository yang sama. Tabel ini adalah peta navigasi — isi teknis masing-masing dokumen tetap ditulis terpisah di file-nya sendiri, bukan diduplikasi di sini.

| Folder | Dokumen | Cakupan |
|---|---|---|
| `01-planning/` | `01-PRD.md` (dokumen ini) | Product Requirements Document — master document |
| `01-planning/` | `02-UIUX-Guideline.md` | Design system, tipografi, layout, komponen — turunan dari Bab 17 (Core Features) & Bab 21-22 (User Journey/Flow) dokumen ini |
| `02-srs/` | `01-Introduction.md` s/d `08-Appendix.md` | Software Requirements Specification — penjabaran teknis dari Bab 18 (Functional Requirements) & Bab 19 (Non-Functional Requirements) |
| `03-sad/` | `01-System-Architecture.md` | Arsitektur sistem tingkat tinggi — turunan dari Bab 17 (Core Features) & Bab 6 (Background) |
| `03-sad/` | `02-Frontend-Architecture.md` | Arsitektur Next.js — turunan dari Bab 19 (NFR: Performance, Compatibility) |
| `03-sad/` | `03-Backend-Architecture.md` | Arsitektur Laravel 12 — turunan dari Bab 19 (NFR: Security, Maintainability) & Bab 20 (Business Rules) |
| `03-sad/` | `04-Database-Architecture.md` | Skema data — turunan dari Bab 17 (Core Features) & Bab 20 (Business Rules) |
| `04-api/` | `API-Specification.md` | Kontrak endpoint — turunan dari Bab 18 (Functional Requirements) |

> **Catatan versi dokumen ini (v1.1):** Versi sebelumnya (v1.0) ditulis sebelum keputusan final tech stack backend. Revisi ini menyesuaikan seluruh referensi teknis di PRD dengan struktur repository resmi (`backend/` berbasis **Laravel 12**, `frontend/` berbasis **Next.js**), tanpa mengubah keputusan bisnis maupun fitur yang sudah disepakati pada versi sebelumnya.

### 1.3 Riwayat Revisi

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | — | Draft awal kerangka dokumen | Product Management Office |
| 1.0 | 06 Juli 2026 | Rilis pertama PRD lengkap 32 bab | Product Management Office |
| 1.1 | 06 Juli 2026 | Penyesuaian referensi tech stack (Laravel 12 + Next.js) dan penambahan peta dokumentasi agar konsisten dengan struktur repository `digital-invitation-platform` | Product Management Office |

---

## 2. Executive Summary

Undangan.Digital adalah platform **Software as a Service (SaaS)** sekaligus **marketplace digital** yang memungkinkan individu, event organizer, dan reseller untuk membuat, mengelola, dan mendistribusikan undangan digital secara instan — tanpa perlu keahlian desain maupun teknis. Produk ini menggantikan proses undangan fisik yang lambat, mahal, dan sulit dilacak dengan pengalaman digital yang cepat (undangan siap dalam hitungan menit), terjangkau (mulai dari paket gratis), dan terintegrasi penuh dengan kanal distribusi yang paling relevan bagi pasar Indonesia: WhatsApp.

Berbeda dari kompetitor yang umumnya hanya menjual "jasa pembuatan" undangan (model manual, per-order, dikerjakan tim desain), Undangan.Digital dibangun sebagai **produk self-service yang scalable**: pengguna membangun undangan mereka sendiri lewat builder visual, sementara sistem menangani seluruh kompleksitas di baliknya — rendering yang ringan untuk ribuan tamu, RSVP real-time, pembayaran otomatis, hingga model bisnis reseller white-label yang memungkinkan mitra membangun bisnis mereka sendiri di atas infrastruktur kami.

Secara bisnis, produk ini beroperasi pada tiga lapisan monetisasi yang saling memperkuat: **(1) langganan B2C** untuk pasangan pengantin dan penyelenggara acara individu, **(2) model B2B/reseller** dengan kredit grosir dan white-label untuk vendor pernikahan dan EO, serta **(3) marketplace add-on** (template premium, fitur tambahan, dan kelak AI Copywriter serta filter AR). Kombinasi ini dirancang agar unit economics tetap sehat baik pada skala individu (ribuan transaksi kecil) maupun skala partner (ratusan transaksi bervolume besar).

Dokumen ini menjabarkan alasan bisnis, target pasar, fitur, arsitektur produk tingkat tinggi, model monetisasi, metrik keberhasilan, serta roadmap 12 bulan pertama yang menjadi acuan bagi seluruh tim lintas fungsi — Product, Engineering, Design, QA, dan DevOps — untuk membangun produk yang konsisten dengan visi jangka panjang perusahaan.

---

## 3. Product Vision

> **"Menjadi infrastruktur digital nomor satu di Asia Tenggara untuk setiap momen yang layak dirayakan — dari lamaran, pernikahan, kelahiran, hingga perayaan korporat — dengan menghilangkan seluruh friksi antara niat untuk mengundang dan momen tamu benar-benar hadir."**

Visi ini memiliki tiga pilar utama yang membedakan Undangan.Digital dari sekadar "pembuat undangan online":

1. **Infrastruktur, bukan sekadar produk.** Undangan.Digital dirancang sebagai platform yang bisa ditumpangi bisnis lain (reseller, EO, vendor pernikahan) untuk membangun layanan mereka sendiri — mirip bagaimana Shopify menjadi infrastruktur commerce bagi jutaan toko, bukan hanya satu toko besar.
2. **End-to-end momen, bukan hanya kartu.** Undangan hanyalah titik masuk. Value sesungguhnya ada pada rangkaian pengalaman: RSVP yang akurat, buku tamu yang membekas, hingga amplop digital yang mempermudah tamu memberi kado tanpa harus datang ke ATM.
3. **Regional-first, WhatsApp-native.** Berbeda dengan pemain global (Greenvelope, Joy) yang berbasis email, produk ini dibangun di atas asumsi bahwa kanal komunikasi utama di Asia Tenggara adalah aplikasi pesan instan — sehingga seluruh alur produk (share, notifikasi RSVP, reminder) dioptimalkan untuk WhatsApp terlebih dahulu.

---

## 4. Product Mission

Misi produk menerjemahkan visi di atas menjadi komitmen operasional jangka pendek-menengah:

- **Menghilangkan kebutuhan skill desain.** Siapa pun — tanpa latar belakang desain grafis — harus mampu menghasilkan undangan yang terlihat profesional dalam waktu kurang dari 5 menit.
- **Menjamin keandalan pada momen yang tidak bisa diulang.** Pernikahan hanya terjadi sekali; undangan yang down saat traffic memuncak (H-1 hingga hari-H) adalah kegagalan produk yang tidak bisa ditoleransi. Reliabilitas adalah komitmen produk, bukan hanya target teknis.
- **Membuka jalur penghasilan bagi mitra bisnis.** Reseller dan EO harus bisa membangun lini bisnis baru di atas platform ini tanpa perlu membangun infrastruktur sendiri, dengan margin yang wajar bagi kedua pihak.
- **Menjadi platform yang dipercaya untuk transaksi finansial kecil.** Amplop digital menyentuh uang sungguhan tamu dan pemilik acara — sistem harus dibangun dengan standar keamanan finansial sejak hari pertama, bukan ditambal belakangan.

---

## 5. Background

### 5.1 Konteks Pasar

Indonesia mencatatkan lebih dari 1,8 juta pernikahan tercatat setiap tahun (data historis Kementerian Agama & BPS), belum termasuk acara ulang tahun, khitanan, aqiqah, wisuda, dan acara korporat yang jumlahnya jauh lebih besar. Pandemi COVID-19 (2020–2022) mempercepat adopsi undangan digital dari sekadar "opsi hemat" menjadi **norma baru** — namun momentum ini kemudian diikuti oleh ledakan pemain kecil berbasis jasa manual (freelancer, agensi one-man-show) yang mengerjakan undangan secara custom per-order, bukan produk yang scalable.

### 5.2 Mengapa Sekarang

Tiga tren mendorong urgensi produk ini:

1. **Fragmentasi pasar penyedia jasa.** Ratusan penyedia undangan digital di Indonesia beroperasi sebagai jasa manual (desain per-order via WhatsApp, dikerjakan 1–3 hari), bukan produk self-service. Ini menciptakan pengalaman yang tidak konsisten dan sulit di-scale menjadi bisnis besar.
2. **Perilaku pembayaran digital sudah matang.** QRIS, e-wallet, dan virtual account kini menjadi metode pembayaran default di Indonesia, membuka jalan bagi fitur amplop digital yang dulu terasa asing kini diterima secara luas.
3. **Reseller economy tumbuh pesat.** Fenomena "jual ulang" digital product (template Canva, jasa desain, top-up game) menunjukkan pasar Indonesia sangat responsif terhadap model bisnis reseller/dropship — sebuah pola yang belum banyak dimanfaatkan pemain undangan digital yang ada.

### 5.3 Kondisi Kompetitif Saat Ini

Pemain lokal (Invitanku, Datengdong, Invitin, dan sejenisnya) unggul dari sisi harga dan lokalitas budaya (dukungan bahasa daerah, tema islami, adat), namun lemah dari sisi **produk sebagai platform** — banyak yang masih bergantung pada proses manual berbasis WhatsApp untuk revisi dan pembuatan. Pemain global (Greenvelope, Joy, WedSites, Canva) unggul dari sisi kematangan produk self-service dan kualitas desain, namun tidak dibangun untuk konteks distribusi via WhatsApp maupun kebutuhan lokal (adat, bahasa daerah, sistem pembayaran Indonesia). Detail lengkap ada di Bab 14 (Competitive Analysis).

---

## 6. Problem Statement

| # | Pain Point | Dampak bagi Pengguna |
|---|---|---|
| P1 | Undangan fisik mahal dan lambat — cetak, distribusi, revisi memakan waktu berhari-hari dan biaya signifikan (Rp3.000–15.000/lembar) | Pengantin/penyelenggara acara kehilangan waktu & anggaran yang bisa dialokasikan ke kebutuhan lain |
| P2 | Platform undangan digital yang ada mayoritas berbasis jasa manual, bukan self-service | Waktu tunggu 1–3 hari untuk revisi kecil; pengalaman tidak konsisten antar-vendor |
| P3 | Sulit melacak siapa yang benar-benar akan hadir | Perencanaan katering, kursi, dan souvenir jadi tidak akurat, berisiko pemborosan atau kekurangan |
| P4 | Distribusi undangan ke ratusan tamu dilakukan manual satu per satu | Highly time-consuming, rawan salah nama/typo, dan tidak profesional |
| P5 | Tamu ingin memberi kado/angpau tapi kesulitan tanpa hadir fisik | Kehilangan potensi "amplop" yang seharusnya bisa diterima pemilik acara |
| P6 | Vendor pernikahan/EO ingin menjual jasa undangan digital tapi tidak punya kapasitas engineering untuk membangun sistem sendiri | Vendor kehilangan potensi revenue stream tambahan yang relevan dengan bisnis inti mereka |
| P7 | Traffic undangan sangat musiman (weekend, musim nikah) sehingga infrastruktur sering down di saat paling krusial | Pengalaman buruk bagi tamu tepat saat first impression acara berlangsung |

---

## 7. Business Opportunity

### 7.1 Ukuran Pasar (Estimasi)

| Segmen | Estimasi Volume Tahunan (Indonesia) | Potensi Take Rate Produk |
|---|---|---|
| Pernikahan | ~1,8 juta acara/tahun | Target penetrasi 2–5% dalam 3 tahun |
| Ulang tahun & acara keluarga (khitanan, aqiqah) | Puluhan juta acara/tahun (mayoritas belum tersentuh undangan digital berbayar) | Segmen volume tinggi, ARPU rendah — cocok untuk model freemium |
| Acara korporat & wisuda | Ratusan ribu acara/tahun | Segmen ARPU tinggi, cocok untuk paket Enterprise/Business |
| Reseller/EO/vendor pernikahan | Puluhan ribu pelaku usaha aktif | Segmen B2B, revenue recurring dari kredit bulk |

### 7.2 Mengapa Peluang Ini Bisa Dimenangkan

- **Celah produk yang jelas:** tidak ada satu pun pemain lokal besar yang menggabungkan self-service builder tingkat produk (seperti Joy/Greenvelope) dengan model reseller white-label yang matang dan integrasi WhatsApp asli.
- **Efek jaringan dari reseller.** Setiap reseller yang onboard membawa puluhan hingga ratusan klien mereka sendiri — pertumbuhan tidak hanya bergantung pada akuisisi B2C langsung.
- **Biaya switching rendah bagi kompetitor lama untuk berpindah.** Karena mayoritas kompetitor lokal berbasis jasa manual, mereka justru berpotensi menjadi reseller/mitra platform ini, bukan hanya pesaing.

---

## 8. Product Goals

Tujuan produk (bukan bisnis) yang harus dicapai agar visi dan misi terwujud secara teknis dan pengalaman pengguna:

1. Pengguna baru dapat menyelesaikan pembuatan undangan pertama mereka (dari registrasi hingga publish) dalam **≤ 5 menit** tanpa bantuan customer service.
2. Halaman undangan publik memiliki **waktu muat di bawah 2 detik** pada koneksi 4G, termasuk saat diakses ribuan tamu secara bersamaan.
3. Sistem RSVP mencatat respons tamu secara **real-time** dan akurat, tanpa duplikasi maupun kehilangan data.
4. Reseller dapat mengelola **klien tak terbatas** dari satu dashboard tanpa watermark platform utama (white-label penuh).
5. Seluruh transaksi finansial (langganan maupun amplop digital) berjalan melalui payment gateway resmi dengan **tingkat kegagalan transaksi < 1%**.
6. Platform mendukung ekspansi ke jenis acara baru (korporat, komunitas) tanpa perubahan arsitektur data inti — mengonfirmasi bahwa desain skema `Invitations` sudah cukup generik sejak awal (lihat SAD & Database Architecture Specification).

---

## 9. Business Goals

| Tujuan Bisnis | Horizon | Indikator |
|---|---|---|
| Mencapai *product-market fit* pada segmen pernikahan B2C | 0–6 bulan | Retention bulan-ke-2 pengguna berbayar ≥ 40% |
| Membangun basis reseller aktif sebagai kanal pertumbuhan utama | 3–9 bulan | ≥ 500 reseller aktif dengan minimal 1 transaksi/bulan |
| Mencapai profitabilitas unit ekonomi per transaksi (bukan profitabilitas perusahaan) | 6–12 bulan | Contribution margin per order positif setelah biaya payment gateway & infrastruktur |
| Ekspansi ke minimal 1 negara Asia Tenggara lain (Malaysia/Filipina) | 12–18 bulan | Peluncuran versi bahasa & payment gateway lokal |
| Membangun brand sebagai platform "paling andal saat momen penting" | Berkelanjutan | Uptime undangan publik ≥ 99,9% pada periode peak (Sabtu–Minggu) |

---

## 10. Success Metrics (KPI)

KPI dikelompokkan berdasarkan lapisan produk agar setiap tim (Growth, Product, Engineering) memiliki metrik yang relevan dengan area kerjanya.

### 10.1 Metrik Akuisisi & Engagement

| Metrik | Definisi | Target Tahun Pertama |
|---|---|---|
| MAU (Monthly Active Users) | User unik yang login atau mengelola undangan dalam 30 hari | 50.000 pada bulan ke-12 |
| DAU/MAU Ratio | Indikator stickiness produk | ≥ 15% |
| Conversion Rate (Visitor → Registrasi) | Jumlah registrasi dibagi total pengunjung landing page | ≥ 8% |
| Conversion Rate (Registrasi → Berbayar) | Jumlah user yang membeli paket dibagi total registrasi | ≥ 25% |
| Activation Rate | Persentase user baru yang berhasil publish undangan pertama dalam 24 jam | ≥ 60% |

### 10.2 Metrik Retensi & Kepuasan

| Metrik | Definisi | Target |
|---|---|---|
| Retention Bulan-2 (Reseller) | Reseller yang masih transaksi di bulan ke-2 setelah onboarding | ≥ 50% |
| Customer Satisfaction (CSAT) | Skor survei pasca-transaksi | ≥ 4,5 / 5 |
| Bounce Rate Landing Page | Persentase pengunjung yang keluar tanpa interaksi | ≤ 45% |
| Churn Rate Reseller Bulanan | Reseller yang berhenti bertransaksi | ≤ 8%/bulan |

### 10.3 Metrik Bisnis & Monetisasi

| Metrik | Definisi | Target Tahun Pertama |
|---|---|---|
| Average Order Value (AOV) | Rata-rata nilai transaksi per order | Rp 90.000–150.000 |
| Gross Merchandise Value (GMV) | Total nilai transaksi yang diproses platform | Rp 15 miliar pada tahun pertama |
| Take Rate Marketplace | Persentase komisi dari transaksi reseller/template premium | 15–20% |
| Monthly Recurring Revenue (MRR) growth | Pertumbuhan MRR bulanan | ≥ 15% MoM pada 6 bulan pertama |

### 10.4 Metrik Teknis & Operasional

| Metrik | Definisi | Target |
|---|---|---|
| Page Load Speed (Undangan Publik) | Largest Contentful Paint (LCP) | < 2 detik |
| Availability (Uptime) | Persentase waktu sistem operasional | ≥ 99,9% (termasuk saat peak weekend) |
| API Error Rate | Persentase request API yang gagal (5xx) | < 0,5% |
| Mean Time to Recovery (MTTR) | Waktu rata-rata pemulihan insiden | < 30 menit untuk insiden kritikal |

---

## 11. Stakeholders

| Peran | Kepentingan Utama | Tingkat Keterlibatan |
|---|---|---|
| **Investor/Pemegang Saham** | Pertumbuhan GMV, jalur menuju profitabilitas, potensi ekspansi regional | Laporan bulanan/kuartalan |
| **Product Manager** | Prioritas fitur, kesesuaian dengan visi produk, KPI produk | Harian |
| **Software Architect / Backend Developer** | Kejelasan requirement fungsional & non-fungsional untuk desain sistem | Harian, sejak fase desain |
| **Frontend Developer** | Konsistensi UI/UX Guideline, kejelasan alur pengguna | Harian |
| **UI/UX Designer** | Konsistensi visi produk dengan design system dan riset pengguna | Sejak fase discovery |
| **QA Engineer** | Kejelasan kriteria penerimaan (acceptance criteria) tiap fitur | Sejak fase requirement hingga rilis |
| **DevOps Engineer** | Target availability, skalabilitas musiman, kebutuhan infrastruktur | Sejak fase arsitektur |
| **Customer Support** | Pemahaman fitur untuk membantu pengguna & mitra reseller | Sebelum rilis fitur baru |
| **Reseller/Mitra B2B** | Kejelasan skema komisi, kualitas white-label, dukungan operasional | Melalui program partnership |
| **Pengguna Akhir (Customer, Bride & Groom, Wedding Organizer)** | Kemudahan penggunaan, keandalan, harga terjangkau | Melalui riset pengguna & feedback loop |
| **Compliance/Legal** | Kepatuhan terhadap regulasi data pribadi (UU PDP) dan sistem pembayaran (BI/OJK) | Sejak fase desain fitur pembayaran & data pribadi |

---

## 12. Target Market

### 12.1 Segmentasi Pasar

| Segmen | Karakteristik | Prioritas |
|---|---|---|
| **B2C — Pasangan Pengantin** | Usia 22–35 tahun, urban & sub-urban, melek digital, sensitif harga namun mengutamakan kesan elegan | Prioritas #1 (peluncuran) |
| **B2C — Perayaan Keluarga** (ulang tahun, khitanan, aqiqah, wisuda) | Segmen volume tinggi, ARPU rendah, sangat sensitif terhadap kemudahan pakai | Prioritas #2 |
| **B2B — Wedding Organizer (WO) & Vendor Pernikahan** | Mengelola 5–50 acara per tahun, butuh efisiensi dan branding sendiri | Prioritas #1 (sejajar B2C pengantin) |
| **B2B — Reseller/Kreator Digital** | Pelaku bisnis digital (mirip penjual pulsa/top-up game), mencari produk digital bermarjin tinggi untuk dijual ulang | Prioritas #2 |
| **B2B — Perusahaan/Korporat** | Acara internal (gathering, town hall), butuh white-label dan branding korporat | Prioritas #3 (fase lanjutan) |

### 12.2 Segmentasi Geografis

- **Fase 1 (0–6 bulan):** Indonesia — fokus kota tingkat 1 & 2 (Jabodetabek, Bandung, Surabaya, Medan, Makassar, Yogyakarta) di mana adopsi digital payment dan smartphone tinggi.
- **Fase 2 (6–12 bulan):** Perluasan ke kota tingkat 3 dengan strategi harga dan template yang disesuaikan (tema adat lokal, bahasa daerah).
- **Fase 3 (12–18 bulan):** Ekspansi regional Asia Tenggara (Malaysia, Filipina) yang memiliki pola budaya perayaan dan penetrasi WhatsApp yang serupa.

### 12.3 Segmentasi Perilaku

- **Value-conscious planners**: mencari harga termurah dengan fitur cukup — target paket Basic/Free.
- **Experience-conscious planners**: bersedia membayar lebih untuk kesan premium & fitur lengkap (amplop digital, custom domain) — target paket Pro/Business.
- **Business-minded resellers**: melihat undangan digital sebagai komoditas untuk dijual ulang, mengutamakan margin dan kemudahan operasional — target paket Reseller/Enterprise.

---

## 13. User Persona

### 13.1 Customer (Individu Umum)

> **"Rina, 24 tahun, Staff Marketing"** — Membuat undangan ulang tahun anak pertamanya. Tidak familiar dengan tools desain, mengandalkan HP untuk segala hal, dan ingin sesuatu yang terlihat bagus tanpa harus belajar software baru.

- **Kebutuhan:** Template siap pakai, proses instan, harga terjangkau.
- **Frustrasi:** Takut hasil akhir terlihat "murahan" atau tidak profesional.
- **Ukuran sukses baginya:** Bisa membagikan undangan ke grup keluarga WhatsApp dalam satu sore.

### 13.2 Bride & Groom (Calon Pengantin)

> **"Dimas & Sarah, 27 & 26 tahun"** — Sedang merencanakan pernikahan dengan anggaran terbatas namun ingin kesan mewah. Mengelola ratusan nama tamu dari kedua keluarga besar.

- **Kebutuhan:** RSVP akurat untuk perencanaan katering, kemudahan membagikan link bernama otomatis ke ratusan tamu, fitur amplop digital untuk tamu luar kota.
- **Frustrasi:** Kesulitan melacak siapa yang sudah/belum konfirmasi; takut ada tamu yang terlewat diundang.
- **Ukuran sukses baginya:** Mengetahui jumlah tamu hadir secara akurat H-3 sebelum acara.

### 13.3 Wedding Organizer (WO) / Vendor Pernikahan

> **"Ibu Tania, pemilik WO dengan 8 karyawan"** — Menangani 20–30 pernikahan per tahun, ingin menawarkan undangan digital sebagai bagian dari paket layanannya tanpa harus merekrut desainer digital khusus.

- **Kebutuhan:** Dashboard untuk mengelola banyak klien sekaligus, branding sendiri (white-label), harga grosir.
- **Frustrasi:** Platform yang ada mengharuskan proses manual per klien, tidak efisien untuk skala bisnisnya.
- **Ukuran sukses baginya:** Bisa onboarding klien baru dalam < 15 menit dan menjualnya sebagai bagian dari paket WO.

### 13.4 Reseller / Kreator Digital

> **"Bagas, 22 tahun, mahasiswa & penjual produk digital"** — Sudah menjual pulsa dan top-up game secara online, mencari produk digital baru bermarjin tinggi yang mudah dijual ke jaringan sosialnya.

- **Kebutuhan:** Kredit grosir, komisi jelas, materi promosi siap pakai, dashboard sederhana.
- **Frustrasi:** Produk digital lain marginnya tipis dan kompetisi harga sangat ketat.
- **Ukuran sukses baginya:** Penghasilan tambahan konsisten Rp1–3 juta/bulan dari menjual ulang undangan.

### 13.5 Admin (Internal — Moderasi & Support)

> **"Tim Customer Support & Konten"** — Bertugas memoderasi template baru, menjawab tiket support, dan memverifikasi pembayaran manual pada fase awal.

- **Kebutuhan:** Dashboard moderasi yang efisien, visibilitas penuh atas status transaksi.
- **Ukuran sukses baginya:** Rata-rata waktu respons tiket < 2 jam.

### 13.6 Super Admin (Internal — Manajemen Platform)

> **"Tim Leadership/Operasional Inti"** — Bertanggung jawab atas kesehatan bisnis platform secara keseluruhan: revenue, growth, dan keputusan strategis produk.

- **Kebutuhan:** Visibilitas end-to-end atas metrik bisnis (MRR, churn, GMV), kontrol penuh atas konfigurasi sistem dan integrasi pihak ketiga.
- **Ukuran sukses baginya:** Dashboard analitik real-time yang menjadi rujukan pengambilan keputusan strategis.

---

## 14. Competitive Analysis

Analisis berikut membandingkan Undangan.Digital dengan pemain lokal maupun global yang relevan, berdasarkan riset produk terkini masing-masing platform.

| Kompetitor | Model Bisnis | Kelebihan | Kekurangan | Peluang Diferensiasi Undangan.Digital |
|---|---|---|---|---|
| **Invitanku** | Freemium self-service, ribuan template siap pakai untuk berbagai jenis acara (pernikahan, ulang tahun, baby shower, dsb.) | Katalog template sangat luas, proses self-service yang matang, harga bersaing | Fokus global/generik, dukungan lokal Indonesia (adat, bahasa daerah, pembayaran lokal) tidak menjadi titik jual utama; tidak ada model reseller yang terstruktur | Menawarkan template dengan kedalaman lokal (adat, syar'i, bahasa daerah) plus program reseller yang tidak dimiliki Invitanku |
| **Datengdong** | Harga sangat terjangkau, banyak template lokal unik | Harga murah, dekat dengan selera pasar Indonesia | Performa server kadang melambat saat peak season; sebagian proses masih semi-manual | Investasi pada arsitektur cache & CDN sejak awal (lihat SAD) agar tetap stabil saat peak, dengan harga yang tetap kompetitif |
| **Invitin** | Template premium eksklusif, positioning kelas atas | Kualitas desain sangat tinggi, branding premium kuat | Harga mahal, fitur kustomisasi dikunci di paket bawah, kurang cocok untuk segmen volume tinggi | Kustomisasi fleksibel tersedia mulai paket mid-tier, tidak dikunci eksklusif untuk kelas atas saja |
| **Canva** | Design tool umum (bukan spesifik undangan), model freemium + Canva Pro | Fleksibilitas desain sangat tinggi, brand sangat kuat, ekosistem template luas | Bukan platform undangan end-to-end — tidak ada RSVP native, tidak ada manajemen tamu, tidak ada distribusi WA terintegrasi | Undangan.Digital adalah *purpose-built* end-to-end: desain + RSVP + distribusi + amplop digital dalam satu alur, sementara Canva hanya menyelesaikan sisi desain |
| **WedSites** | One-time payment (bukan langganan), all-in-one wedding planning (website, budget, seating chart, guest list) | Sangat lengkap sebagai *wedding planning tool*, drag-and-drop builder kuat, mendukung multi-bahasa | Berbasis pasar Barat (USD), tidak terintegrasi dengan WhatsApp maupun payment gateway Indonesia; harga one-time ($99–199) relatif mahal untuk pasar Indonesia | Model harga lokal (Rupiah, mulai gratis), distribusi WhatsApp-native, dan amplop digital terintegrasi QRIS — kebutuhan yang tidak relevan bagi WedSites |
| **Joy (withjoy.com)** | Gratis untuk website & RSVP, monetisasi dari registry & stationery premium | Smart RSVP sangat matang (custom questions, grouping keluarga), ekosistem registry kuat, aplikasi mobile pendamping | Berbasis pasar AS, model registry (hadiah fisik via toko) tidak relevan dengan budaya "amplop"/angpau Indonesia; tidak ada integrasi WhatsApp | Amplop digital (bukan registry toko fisik) sebagai pengganti yang lebih relevan secara budaya, plus distribusi WhatsApp-native |
| **Greenvelope** | Model pay-per-guest, positioning premium ramah lingkungan | Pengalaman "amplop animasi" sangat khas, RSVP & seating chart sangat matang, delivery email premium | Model harga per-tamu menjadi mahal untuk acara besar (ratusan tamu), berbasis email (bukan WhatsApp), tidak ada payment lokal | Model harga flat per paket (bukan per-tamu) jauh lebih ekonomis untuk acara besar khas Indonesia; distribusi via WhatsApp yang jauh lebih relevan secara lokal |

### 14.1 Kesimpulan Analisis Kompetitor

Pemain global (Canva, WedSites, Joy, Greenvelope) unggul dalam **kematangan produk self-service** dan kualitas desain, tetapi seluruhnya dibangun di atas asumsi pasar Barat: pembayaran kartu kredit, distribusi email, dan model gift-registry. Pemain lokal (Invitanku, Datengdong, Invitin) memahami kebutuhan lokal namun umumnya belum menggabungkan kematangan produk self-service dengan model bisnis reseller yang scalable. **Celah pasar Undangan.Digital** ada tepat di persimpangan ini: produk self-service sekelas pemain global, dengan asumsi dasar (WhatsApp, QRIS, harga Rupiah, adat lokal) sekelas pemain lokal terbaik — ditambah model reseller yang belum dieksekusi matang oleh siapa pun di pasar ini.

---

## 15. SWOT Analysis

| Kategori | Poin Analisis |
|---|---|
| **Strengths (Kekuatan)** | • Model reseller white-label yang terstruktur (belum dimiliki matang oleh kompetitor lokal)<br>• Distribusi WhatsApp-native sejak arsitektur awal<br>• Arsitektur cache/CDN dirancang sejak hari pertama untuk mengatasi masalah performa yang jadi kelemahan umum kompetitor lokal<br>• Model harga fleksibel (freemium hingga enterprise) menjangkau seluruh segmen pasar |
| **Weaknesses (Kelemahan)** | • Brand baru, belum memiliki basis pengguna maupun kepercayaan pasar seperti pemain lama<br>• Katalog template awal lebih terbatas dibanding pemain established (Invitanku, Canva)<br>• Ketergantungan pada integrasi pihak ketiga (payment gateway, WhatsApp API) yang berada di luar kendali penuh perusahaan |
| **Opportunities (Peluang)** | • Fragmentasi pasar penyedia jasa manual membuka peluang konsolidasi melalui program reseller<br>• Pertumbuhan ekonomi reseller/dropship digital di Indonesia<br>• Potensi ekspansi ke jenis acara non-pernikahan (korporat, komunitas, sekolah) yang belum tergarap maksimal<br>• Potensi ekspansi regional Asia Tenggara dengan pola budaya serupa |
| **Threats (Ancaman)** | • Kompetitor lokal besar bisa meniru model reseller dalam waktu singkat begitu terbukti berhasil<br>• Perubahan kebijakan API pihak ketiga (WhatsApp Business API, payment gateway) dapat mengganggu operasional<br>• Sensitivitas harga pasar Indonesia yang tinggi dapat menekan margin<br>• Risiko regulasi terkait perlindungan data pribadi (UU PDP) yang terus berkembang |

---

## 16. Product Scope

### 16.1 In Scope (Versi 1.0–5.0 / 12 Bulan Pertama)

- Registrasi & autentikasi multi-role (Customer, Reseller, Admin, Super Admin)
- Builder undangan self-service berbasis wizard (step-by-step)
- Katalog template dengan filter kategori, tema, dan warna
- Sistem RSVP, buku tamu, galeri foto/video, love story, hitung mundur, musik latar, Google Maps
- Generate link tamu massal & distribusi via WhatsApp
- Amplop digital (info rekening, e-wallet, QRIS)
- Payment gateway terintegrasi (langganan & marketplace)
- Dashboard Reseller dengan white-label penuh
- Sistem custom domain
- QR Check-in di lokasi acara
- Program affiliate/referral
- AI Assistant (copywriter ucapan) — fase lanjutan
- CMS untuk blog & konten marketing SEO

### 16.2 Out of Scope (Tidak Termasuk pada 12 Bulan Pertama)

- Aplikasi mobile native (iOS/Android) — MVP berbasis web responsif terlebih dahulu
- Fitur live streaming acara di dalam undangan
- Filter AR Instagram terintegrasi (dicadangkan sebagai future enhancement, Bab 31)
- Manajemen vendor pernikahan pihak ketiga (katalog vendor, marketplace vendor) — di luar cakupan produk undangan
- Dukungan multi-bahasa penuh selain Bahasa Indonesia & Inggris pada fase awal
- Sistem manajemen keuangan/akuntansi lengkap bagi reseller (hanya wallet & withdrawal sederhana)

---

## 17. Core Features

Fitur dikelompokkan berdasarkan modul agar setiap tim engineering dapat memetakan tanggung jawab dengan jelas. Setiap fitur disertai *business reasoning* singkat.

| Modul | Fitur | Business Reasoning |
|---|---|---|
| **Authentication** | Registrasi, login, OAuth Google, refresh token, RBAC 4 peran | Fondasi seluruh fitur; RBAC sejak awal mencegah kebocoran akses lintas peran (lihat Bab 20 Business Rules) |
| **Dashboard** | Dashboard Customer, Reseller, Admin, Super Admin (berbeda per peran) | Setiap peran memiliki tujuan berbeda (lihat Bab 13); dashboard generik akan menurunkan efisiensi masing-masing |
| **Invitation Builder** | Wizard step-by-step (Data Mempelai → Acara → Galeri → Love Story → Pengaturan) | Mengubah proses yang biasanya butuh skill desain menjadi proses mengisi formulir — kunci dari Product Goal #1 |
| **Invitation Management** | CRUD undangan, status Draft/Active/Expired, preview real-time | Memberi kontrol penuh kepada pemilik acara atas siklus hidup undangannya |
| **Guest Management** | Import Excel/manual, generate link unik per tamu | Menyelesaikan Pain Point P4 (distribusi manual yang melelahkan) |
| **RSVP** | Konfirmasi kehadiran, jumlah tamu, custom questions | Menyelesaikan Pain Point P3 (akurasi perencanaan) |
| **Guest Book** | Ucapan & doa dari tamu, moderasi ringan | Nilai emosional — kenangan digital pengganti buku tamu fisik |
| **Gallery & Story** | Galeri foto/video, timeline kisah cinta | Personalisasi & storytelling, mendorong engagement tamu |
| **Music** | Musik latar dengan kontrol putar | Sentuhan personal yang menjadi standar di industri (Greenvelope, Joy) |
| **Countdown** | Hitung mundur otomatis ke tanggal acara | Fitur kecil dengan dampak emosional tinggi, biaya implementasi rendah |
| **Maps** | Google Maps embed + direction | Menyelesaikan kebutuhan praktis tamu menuju lokasi |
| **Gift (Amplop Digital)** | Info rekening/e-wallet/QRIS | Menyelesaikan Pain Point P5 — salah satu diferensiator monetisasi utama |
| **Analytics** | Statistik pengunjung, RSVP, funnel undangan dibuka vs. dikonfirmasi | Memberi pemilik acara rasa kontrol dan data untuk keputusan (katering, kursi) |
| **Marketplace** | Template premium, add-on fitur berbayar | Sumber revenue tambahan di luar langganan pokok |
| **Order & Payment** | Checkout paket, histori transaksi, invoice | Fondasi monetisasi B2C |
| **Subscription** | Manajemen masa aktif paket, auto-renewal (fase lanjutan) | Mendukung model recurring revenue |
| **Wallet & Withdrawal** | Saldo komisi reseller/affiliate, penarikan dana | Fondasi model bisnis reseller (Business Goal #2) |
| **Referral/Affiliate** | Kode referral, tracking komisi | Kanal pertumbuhan berbiaya rendah dibanding iklan berbayar |
| **Notification** | Notifikasi in-app & email untuk RSVP baru, transaksi, dsb. | Meningkatkan engagement dan kepercayaan transaksi |
| **Admin** | Moderasi template, tiket support, verifikasi pembayaran manual | Menyelesaikan Pain Point P2 tanpa kehilangan kontrol kualitas |
| **Super Admin** | Kontrol penuh sistem, laporan revenue, manajemen role | Kebutuhan governance internal |
| **Reseller** | Kredit bulk, manajemen klien, white-label, custom domain klien | Menyelesaikan Pain Point P6 — inti dari Business Goal #2 |
| **AI Module** | AI Copywriter untuk ucapan/kalimat pembuka | Diferensiasi produk & pengurang friksi menulis bagi pengguna awam |
| **CMS** | Blog artikel wedding planner untuk SEO | Kanal akuisisi organik jangka panjang, biaya marginal rendah |
| **Settings** | Pengaturan akun, preferensi notifikasi, keamanan | Kebutuhan dasar tata kelola akun pengguna |

---

## 18. Functional Requirements

Ringkasan requirement fungsional inti (detail lengkap tiap endpoint dan skema data merujuk ke API Specification dan Database Architecture Specification).

| ID | Requirement | Prioritas |
|---|---|---|
| FR-01 | Sistem harus memungkinkan registrasi via email/password dan Google OAuth | Must Have |
| FR-02 | Sistem harus menerbitkan JWT access token & refresh token dengan masa berlaku terpisah | Must Have |
| FR-03 | Sistem harus menegakkan Role-Based Access Control untuk 4 peran utama (Super Admin, Admin, Reseller, Customer) | Must Have |
| FR-04 | Pengguna harus dapat memilih template dari katalog dengan filter kategori/tema/warna | Must Have |
| FR-05 | Sistem harus menyediakan wizard builder multi-step dengan auto-save | Must Have |
| FR-06 | Sistem harus dapat men-generate link unik per tamu dari daftar nama (impor Excel atau input manual) | Must Have |
| FR-07 | Tamu harus dapat mengisi RSVP tanpa perlu login/registrasi | Must Have |
| FR-08 | Sistem harus mencatat & menampilkan statistik RSVP secara real-time di dashboard pemilik undangan | Must Have |
| FR-09 | Sistem harus mendukung checkout paket berbayar melalui payment gateway (virtual account, e-wallet, QRIS) | Must Have |
| FR-10 | Reseller harus dapat membeli kredit dalam jumlah besar (bulk) dengan harga diskon bertingkat | Should Have |
| FR-11 | Reseller harus dapat membuat undangan atas nama klien tanpa watermark platform utama | Must Have |
| FR-12 | Sistem harus mendukung permintaan custom domain yang di-mapping otomatis melalui API DNS provider | Should Have |
| FR-13 | Sistem harus menyediakan QR code unik per tamu untuk check-in di lokasi acara | Could Have |
| FR-14 | Sistem harus mencatat setiap transaksi amplop digital dengan jejak audit yang jelas | Must Have |
| FR-15 | Sistem harus mengirim notifikasi (in-app/email) saat ada RSVP baru, ucapan baru, atau transaksi baru | Should Have |
| FR-16 | Super Admin harus memiliki akses ke laporan revenue, MRR, dan churn secara real-time | Must Have |
| FR-17 | Admin harus dapat memoderasi template baru sebelum tayang di katalog publik | Must Have |
| FR-18 | Sistem harus mendukung kode voucher/kupon dengan validasi kuota dan masa berlaku | Should Have |
| FR-19 | Sistem harus mendukung program referral dengan pelacakan komisi otomatis | Could Have |
| FR-20 | AI Assistant harus dapat menghasilkan draf kalimat ucapan berdasarkan input singkat pengguna | Could Have |

*(Skala prioritas MoSCoW: Must Have, Should Have, Could Have, Won't Have — konsisten dengan Bab 21 Prioritas Roadmap.)*

---

## 19. Non-Functional Requirements

| Kategori | Requirement | Target Terukur |
|---|---|---|
| **Performance** | Waktu muat halaman undangan publik | LCP < 2 detik pada koneksi 4G |
| **Availability** | Uptime sistem, termasuk saat traffic peak (Sabtu–Minggu musim nikah) | ≥ 99,9% per bulan |
| **Security** | Enkripsi data in-transit (TLS 1.2+) dan at-rest untuk data sensitif (kredensial, data pembayaran) | Wajib, tanpa pengecualian |
| **Security** | Validasi kepemilikan resource (ownership check) pada setiap endpoint yang mengakses data milik user | Wajib pada 100% endpoint yang relevan |
| **Scalability** | Sistem harus dapat menangani lonjakan traffic 10x lipat pada akhir pekan tanpa degradasi performa signifikan | Auto-scaling horizontal pada layer API & cache |
| **Reliability** | Mean Time Between Failures (MTBF) pada layanan inti | > 720 jam |
| **Accessibility** | Kepatuhan minimum terhadap WCAG 2.1 Level AA untuk elemen kritikal (kontras warna, ukuran target sentuh) | Target ukuran sentuh minimal 44x44px sesuai UI/UX Guideline |
| **SEO** | Landing page dan halaman blog harus dapat di-crawl dan diberi index oleh mesin pencari | Skor Core Web Vitals "Good" pada seluruh halaman publik |
| **Maintainability** | Kode backend mengikuti konvensi Laravel (Routes → Middleware → Controller → Service/Action Class → Eloquent Model) agar mudah diuji & dikembangkan; logika bisnis tidak ditulis langsung di Controller | Code coverage unit/feature test (PestPHP/PHPUnit) ≥ 70% pada modul inti |
| **Observability** | Setiap request API harus dapat ditelusuri melalui logging terstruktur & tracing | Log tersentralisasi dengan retensi minimal 30 hari |
| **Compatibility** | Undangan publik harus tampil baik di 95% kombinasi browser/device pasar Indonesia (Chrome Mobile, Safari iOS) | Pengujian lintas perangkat sebelum tiap rilis mayor |
| **Internationalization** | Dukungan Bahasa Indonesia sebagai default, Bahasa Inggris sebagai opsi tambahan pada undangan | Tersedia toggle bahasa per undangan pada fase lanjutan |

---

## 20. Business Rules

1. **Ownership Rule.** Setiap resource (undangan, tamu, RSVP) hanya dapat diakses dan dimodifikasi oleh `user_id` pemiliknya, kecuali oleh peran Admin/Super Admin untuk keperluan moderasi.
2. **Role Assignment Rule.** Peran Reseller, Admin, dan Super Admin tidak dapat diperoleh melalui registrasi publik — hanya dapat ditetapkan secara manual oleh Super Admin.
3. **Publish Rule.** Undangan hanya dapat berstatus `Active` (dapat diakses publik) setelah pembayaran paket terverifikasi berhasil.
4. **Guest Link Rule.** Setiap link tamu bersifat unik dan tidak dapat digunakan ulang oleh tamu lain untuk mencegah RSVP ganda atas nama yang sama.
5. **White-Label Rule.** Watermark/branding platform utama otomatis disembunyikan untuk seluruh undangan yang dibuat melalui akun Reseller aktif.
6. **Refund Rule.** Pembayaran paket yang sudah diaktifkan (undangan sudah berstatus `Active`) tidak dapat direfund, kecuali terbukti kegagalan sistem di pihak platform.
7. **Domain Expiry Rule.** Custom domain yang tidak diperpanjang otomatis dinonaktifkan dan undangan kembali ke subdomain default platform, tanpa kehilangan data.
8. **Wallet Withdrawal Rule.** Penarikan saldo reseller/affiliate memiliki nilai minimum penarikan dan diproses maksimal dalam 3 hari kerja setelah verifikasi.
9. **Content Moderation Rule.** Template baru dari kreator pihak ketiga wajib melalui moderasi Admin sebelum tayang di katalog publik, untuk menjaga standar kualitas dan mencegah konten tidak pantas.
10. **Data Retention Rule.** Data undangan yang sudah `Expired` disimpan minimal 90 hari sebelum dihapus permanen, memberi kesempatan pemilik acara untuk memperpanjang atau mengekspor data.

---

## 21. User Journey

### 21.1 Perjalanan Bride & Groom (Naratif)

Dimas dan Sarah baru saja menentukan tanggal pernikahan mereka. Mereka mendengar tentang Undangan.Digital dari rekan yang pernah memakainya, lalu membuka landing page dari HP masing-masing di sela waktu istirahat kerja. Melihat harga paket Pro yang jelas dan contoh template yang bisa langsung di-preview dalam frame HP, mereka memutuskan mendaftar tanpa perlu bertanya ke admin terlebih dahulu — sesuatu yang tidak bisa mereka lakukan di platform lain yang mengharuskan chat WhatsApp dulu untuk sekadar melihat harga.

Setelah membayar, mereka mengisi builder step-by-step: data mempelai, detail acara (akad & resepsi dengan lokasi berbeda), mengunggah galeri foto prewedding, dan menulis kisah cinta singkat. Semua berjalan dengan live preview di sisi kanan layar sehingga mereka bisa langsung melihat hasilnya tanpa perlu menunggu revisi dari admin. Dalam waktu kurang dari 30 menit (termasuk waktu memilih-milih foto), undangan mereka siap dipublikasikan.

Mereka lalu mengimpor daftar 350 nama tamu dari file Excel yang sudah disiapkan keluarga masing-masing, dan sistem menghasilkan 350 link unik secara otomatis. Alih-alih menyalin satu per satu, mereka membagikannya lewat fitur "klik untuk kirim ke WhatsApp" yang otomatis mengisi nama tamu di pesan pembuka. Selama dua minggu berikutnya, mereka memantau dashboard RSVP setiap malam — melihat siapa yang sudah membuka undangan, siapa yang sudah konfirmasi hadir, dan menyesuaikan jumlah pesanan katering berdasarkan angka riil, bukan estimasi kasar seperti pernikahan kakak mereka sebelumnya yang masih pakai undangan cetak.

### 21.2 Perjalanan Reseller (Naratif)

Bagas, yang sebelumnya berjualan pulsa dan top-up game, melihat iklan program reseller Undangan.Digital di Instagram. Ia mendaftar dan membeli paket kredit bulk termurah untuk mencoba. Dashboard reseller memberinya subdomain khusus dan opsi branding sendiri — ia menamainya "Undangan Kilat by Bagas". Ia lalu menawarkan jasa ini ke grup WhatsApp alumni kampusnya, mendapatkan tiga klien pertama dalam seminggu.

Karena tidak perlu menyentuh kode maupun desain sama sekali — cukup membantu klien mengisi form builder atau bahkan mengarahkan mereka mengisi sendiri — Bagas bisa melayani klien sambil kuliah. Ia memantau saldo dan komisinya dari dashboard wallet, dan menarik dananya begitu mencapai jumlah minimum penarikan.

---

## 22. User Flow

Alur utama pengguna reguler, dari kunjungan pertama hingga acara selesai:

```
Pengunjung Website
      ↓
Registrasi / Login (Email atau Google OAuth)
      ↓
Pilih Paket (Free / Basic / Pro / Business)
      ↓
Pilih Template dari Katalog (filter kategori, tema, warna)
      ↓
Pembayaran (Virtual Account / E-Wallet / QRIS)
      ↓
Builder Wizard: Data Mempelai → Detail Acara → Galeri/Video → Love Story → Pengaturan
      ↓
Preview Real-time (frame mobile)
      ↓
Publish Undangan (status: Active)
      ↓
Input/Impor Daftar Tamu → Generate Link Unik Massal
      ↓
Bagikan via WhatsApp (klik langsung dari dashboard)
      ↓
Tamu Membuka Undangan → Mengisi RSVP → Menulis di Buku Tamu → (Opsional) Kirim Amplop Digital
      ↓
Pemilik Acara Memantau Statistik RSVP & Amplop di Dashboard
      ↓
QR Check-in di Lokasi Acara (opsional, paket tertentu)
      ↓
Pasca-Acara: Undangan berstatus Expired setelah masa aktif berakhir, data disimpan 90 hari
```

### 22.1 Alur Reseller (Ringkas)

```
Registrasi Reseller (verifikasi manual oleh Super Admin)
      ↓
Beli Kredit Bulk (harga diskon bertingkat)
      ↓
Setup Branding (nama brand, subdomain/custom domain sendiri)
      ↓
Tambah Klien Baru → Buat/Kelola Undangan atas nama Klien
      ↓
Klien Menggunakan Undangan Seperti Alur Customer Biasa (tanpa watermark platform utama)
      ↓
Reseller Memantau Kredit Tersisa & Komisi di Dashboard Wallet
      ↓
Pengajuan Withdrawal Saldo (diproses maks. 3 hari kerja)
```

---

## 23. Monetization Strategy

Undangan.Digital menerapkan strategi monetisasi berlapis agar tidak bergantung pada satu sumber revenue tunggal:

| Lapisan | Sumber Revenue | Penjelasan |
|---|---|---|
| **1. Subscription B2C** | Penjualan paket Basic/Pro/Business per undangan | Sumber revenue utama pada tahun pertama, model transaksi sekali bayar per acara (bukan recurring bulanan, karena sifat acara yang satu kali) |
| **2. Reseller Bulk Credit** | Penjualan kredit grosir dengan harga diskon bertingkat | Revenue bervolume tinggi dengan biaya akuisisi pelanggan (CAC) yang jauh lebih rendah karena reseller yang melakukan akuisisi end-user |
| **3. Marketplace Add-on** | Template premium, fitur tambahan (custom domain, AI Copywriter, filter AR) | Revenue incremental dengan margin tinggi karena biaya marginal produksi mendekati nol |
| **4. Take Rate Amplop Digital** | Komisi kecil dari setiap transaksi amplop digital yang diproses (mengikuti fee payment gateway + margin platform) | Revenue recurring yang tumbuh seiring skala transaksi, namun harus transparan agar tidak dianggap memotong hak pemilik acara |
| **5. Affiliate/Referral** | Bukan sumber revenue langsung, namun kanal akuisisi berbiaya rendah yang menekan CAC keseluruhan | Investasi jangka menengah untuk pertumbuhan organik |

### 23.1 Prinsip Monetisasi

- **Freemium sebagai pintu masuk, bukan produk akhir.** Paket gratis harus cukup menarik untuk menunjukkan nilai produk, namun cukup terbatas (watermark, fitur dasar saja) agar mendorong upgrade.
- **Transparansi biaya amplop digital.** Potongan dari transaksi amplop digital harus dikomunikasikan dengan jelas di awal untuk menjaga kepercayaan — ini menyangkut uang pribadi pengguna, bukan sekadar fitur produk.
- **Reseller tidak boleh berkompetisi harga dengan platform utama.** Struktur harga bulk harus memberi insentif margin yang wajar bagi reseller tanpa membuat mereka "membanting harga" hingga merusak persepsi harga pasar.

---

## 24. Pricing Strategy

Strategi harga menggunakan pendekatan **value-based tiering**: setiap kenaikan paket harus mencerminkan kenaikan nilai yang jelas dirasakan pengguna, bukan sekadar pembatasan artifisial.

| Prinsip | Penjelasan |
|---|---|
| **Harga transparan di muka** | Berbeda dari kompetitor berbasis jasa manual yang mengharuskan chat dulu untuk tahu harga, seluruh harga paket tampil jelas di landing page |
| **Psychological anchoring** | Paket menengah (Pro) diposisikan sebagai pilihan "paling populer" untuk mengarahkan preferensi mayoritas pengguna ke paket dengan margin terbaik |
| **Harga lokal, bukan konversi mata uang asing** | Seluruh harga dalam Rupiah dan disesuaikan daya beli pasar Indonesia, berbeda dari WedSites/Greenvelope yang berbasis USD |
| **Diskon bertingkat untuk volume (reseller)** | Semakin besar kredit yang dibeli reseller di muka, semakin rendah harga per-unit — mendorong komitmen jangka panjang mitra |
| **Tidak ada biaya tersembunyi pada fitur inti** | RSVP, buku tamu, dan hitung mundur tersedia di seluruh paket berbayar tanpa biaya tambahan, untuk menghindari kesan "fitur dasar dikunci" seperti keluhan pada Invitin |

---

## 25. Subscription Plans

| Paket | Target Pengguna | Harga Indikatif | Fitur Utama |
|---|---|---|---|
| **Free** | Percobaan/perayaan sederhana | Rp 0 | 1 template dasar, watermark platform, RSVP & buku tamu terbatas, aktif 30 hari |
| **Basic** | Individu dengan anggaran terbatas | Rp 49.000 | 1 template standar, RSVP & buku tamu penuh, hitung mundur, aktif 3 bulan |
| **Pro** | Pasangan pengantin, perayaan penting | Rp 99.000 | Semua template non-eksklusif, amplop digital, generate link WA massal, custom domain, aktif 6 bulan — **paket paling populer** |
| **Business** | Wedding Organizer & vendor skala kecil-menengah | Rp 299.000 | Semua fitur Pro + multi-undangan dalam satu akun, QR check-in, analitik lanjutan, prioritas support |
| **Reseller** | Mitra bisnis B2B | Mulai Rp 499.000 (kredit bulk) | Kredit undangan grosir, white-label penuh (tanpa watermark), dashboard klien sendiri, custom domain klien |
| **Enterprise** | Korporat & institusi | Harga kustom (by request) | SLA khusus, integrasi API korporat, branding penuh, dedicated support, kontrak tahunan |

*Catatan: Harga bersifat indikatif untuk perencanaan awal dan dapat disesuaikan berdasarkan hasil riset harga (pricing research) sebelum peluncuran resmi.*

---

## 26. Risk Analysis

| Kategori | Risiko | Dampak | Mitigasi |
|---|---|---|---|
| **Bisnis** | Kompetitor besar meniru model reseller dengan cepat | Kehilangan diferensiasi utama | Membangun *network effect* dan loyalitas reseller lebih dulu melalui program insentif jangka panjang (bukan hanya harga) |
| **Bisnis** | Sensitivitas harga pasar menekan margin | Profitabilitas tertunda | Diversifikasi revenue (marketplace, take rate amplop) agar tidak bergantung pada satu sumber |
| **Teknis** | Lonjakan traffic musiman (akhir pekan) menyebabkan downtime | Kegagalan produk tepat saat momen krusial pengguna | Auto-scaling, caching Redis untuk data undangan publik, load testing rutin sebelum musim nikah puncak |
| **Teknis** | Ketergantungan pada API pihak ketiga (WhatsApp, payment gateway) | Gangguan operasional di luar kendali tim | Redundansi penyedia (multi payment gateway), circuit breaker & fallback pada integrasi kritikal |
| **Keamanan** | Kebocoran akses lintas pengguna (ownership bypass) | Kepercayaan pengguna rusak, potensi pelanggaran data pribadi | Ownership middleware wajib pada seluruh endpoint yang relevan (lihat SAD & Business Rule #1) |
| **Keamanan** | Penyalahgunaan sistem RSVP oleh bot (spam) | Data RSVP tidak akurat, mengganggu perencanaan acara pemilik | Rate limiting per-IP dan reCAPTCHA invisible pada form publik |
| **Operasional** | Volume tiket support melonjak saat peak season | Waktu respons memburuk, kepuasan pengguna turun | Basis pengetahuan self-service (FAQ, CMS) dan staffing musiman |
| **Finansial** | Kegagalan transaksi payment gateway | Kehilangan revenue & pengalaman buruk pengguna | Monitoring tingkat kegagalan transaksi real-time, dukungan multi payment gateway (Midtrans & Xendit) |
| **Regulasi** | Perubahan/penambahan ketentuan UU Perlindungan Data Pribadi (PDP) | Risiko denda dan kewajiban kepatuhan baru | Audit kepatuhan data pribadi berkala bersama tim legal, minimalisasi penyimpanan data sensitif yang tidak perlu |

---

## 27. Assumptions

1. Mayoritas pengguna mengakses platform melalui perangkat mobile, bukan desktop.
2. WhatsApp tetap menjadi kanal distribusi dominan di pasar target selama masa roadmap 12 bulan.
3. Payment gateway lokal (Midtrans/Xendit) tetap tersedia dan stabil secara komersial.
4. Pengguna bersedia membayar untuk kenyamanan (kecepatan, keandalan) dibanding memilih opsi gratis yang lebih ribet.
5. Reseller/mitra bisnis memiliki jaringan sosial yang dapat dikonversi menjadi pelanggan tanpa biaya akuisisi besar dari platform.
6. Regulasi terkait undangan digital dan transaksi amplop digital tidak berubah signifikan dalam 12 bulan ke depan.
7. Infrastruktur cloud (AWS/GCP/Cloudflare) yang dipilih tetap kompetitif secara biaya seiring pertumbuhan skala.

---

## 28. Dependencies

| Dependency | Jenis | Dampak Jika Tidak Tersedia |
|---|---|---|
| Payment Gateway (Midtrans/Xendit) | Eksternal — Kritis | Seluruh alur monetisasi berhenti total |
| WhatsApp Business API / penyedia pihak ketiga | Eksternal — Kritis | Fitur distribusi utama tidak berfungsi, mengurangi diferensiasi utama produk |
| Google Maps API | Eksternal — Penting | Fitur lokasi/direction tidak tersedia, menurunkan kelengkapan undangan |
| Cloudinary/AWS S3 | Eksternal — Kritis | Upload & kompresi galeri foto tidak berfungsi |
| Cloudflare (CDN & DNS) | Eksternal — Kritis | Performa & fitur custom domain terganggu |
| OpenAI API | Eksternal — Opsional (fase lanjutan) | Fitur AI Copywriter tertunda, tidak mengganggu fitur inti |
| Tim Legal/Compliance internal | Internal — Penting | Risiko kepatuhan regulasi data pribadi & transaksi finansial |
| Tim Desain Template | Internal — Penting | Katalog template tidak bertumbuh, menurunkan daya tarik produk dari waktu ke waktu |

---

## 29. Product Roadmap (12 Bulan)

| Kuartal | Fokus Utama | Fitur yang Dirilis |
|---|---|---|
| **Q1 (Bulan 1–3)** — Core MVP | Fondasi produk & validasi pasar awal | Auth, dashboard dasar, 5 template default, builder form, RSVP, buku tamu, hitung mundur, maps, galeri, pembayaran manual (transfer bank) |
| **Q2 (Bulan 4–6)** — Automation & Monetization | Otomatisasi transaksi & kanal reseller pertama | Integrasi payment gateway (aktivasi instan), generate & blast link WA otomatis, dashboard Reseller awal, amplop digital (input rekening manual) |
| **Q3 (Bulan 7–9)** — Scale & Feature Rich | Skalabilitas teknis & kelengkapan fitur premium | Custom domain otomatis, QR check-in, 20+ template premium baru, penguatan infrastruktur cache/CDN |
| **Q4 (Bulan 10–12)** — Growth | Kanal pertumbuhan & kematangan produk | Program affiliate/referral, dukungan dwi-bahasa pada undangan, tema builder (kustomisasi warna/font oleh pengguna), AI Assistant tahap awal |

*Detail versi (V1.0–V5.0) selaras dengan Software Architecture Document; roadmap ini adalah pandangan bisnis dari versi teknis yang sama.*

---

## 30. Release Strategy

| Tahap | Definisi | Kriteria Keluar (Exit Criteria) |
|---|---|---|
| **MVP (Minimum Viable Product)** | Rilis internal/terbatas untuk memvalidasi alur inti: registrasi → builder → publish → RSVP | Seluruh Functional Requirement "Must Have" (Bab 18) berfungsi tanpa bug kritikal |
| **Beta (Closed Beta)** | Rilis terbatas ke sekelompok pengguna & reseller undangan (early adopter) yang dipilih | Feedback kualitatif positif, tidak ada insiden keamanan/data, tingkat penyelesaian builder ≥ 50% |
| **Release Candidate (RC)** | Rilis mendekati final dengan seluruh fitur Q1–Q2 roadmap, diuji beban (load testing) untuk skenario peak weekend | Load test berhasil menangani traffic simulasi 10x rata-rata tanpa penurunan performa signifikan |
| **Production (General Availability)** | Rilis publik penuh, terbuka untuk registrasi umum dan pemasaran aktif | Uptime stabil ≥ 99,9% selama masa RC, seluruh channel payment gateway terverifikasi produksi |

Strategi rilis mengikuti pendekatan **incremental rollout**: setiap fitur baru pada Q2–Q4 dirilis terlebih dahulu ke basis pengguna reseller (early feedback loop lebih cepat karena volume transaksi tinggi) sebelum dirilis penuh ke seluruh basis pengguna Customer.

---

## 31. Future Enhancements

Daftar berikut adalah katalog ide pengembangan jangka panjang (di luar roadmap 12 bulan pertama), sebagai bahan diskusi prioritisasi produk pada siklus perencanaan berikutnya.

1. Aplikasi mobile native (iOS & Android) untuk dashboard Customer & Reseller
2. Filter AR Instagram/TikTok terintegrasi untuk promosi acara
3. Fitur live streaming acara langsung dari halaman undangan
4. Integrasi katalog vendor pernikahan (fotografer, katering, dekorasi) sebagai marketplace tambahan
5. AI Assistant untuk rekomendasi template berdasarkan tema acara
6. AI generator galeri (color grading otomatis foto prewedding)
7. Voice invitation — pesan suara singkat dari pengantin untuk tamu VIP
8. Undangan multi-bahasa penuh (bukan hanya Indonesia-Inggris)
9. Widget cuaca otomatis di halaman undangan (prediksi cuaca hari-H)
10. Fitur "wedding countdown" terintegrasi dengan kalender pribadi tamu (Google Calendar add-to-calendar)
11. Sistem seating chart otomatis berbasis data RSVP
12. Kalkulator estimasi anggaran katering berdasarkan data RSVP real-time
13. Modul checklist perencanaan pernikahan (mirip WedSites) sebagai add-on
14. Fitur "tamu virtual" — tombol hadir secara virtual/live untuk yang berhalangan
15. Dukungan livestream dengan komentar/reaksi real-time dari tamu jarak jauh
16. Fitur donasi amal terintegrasi (opsi selain amplop digital pribadi)
17. Sistem loyalitas poin untuk reseller berdasarkan volume transaksi
18. Marketplace jasa freelance (MC, fotografer) yang terhubung langsung dari dashboard
19. Custom font upload untuk kaligrafi nama pengantin
20. Editor drag-and-drop tingkat lanjut (mirip WedSites) untuk pengguna power-user
21. Fitur wedding registry (gift registry) terintegrasi e-commerce lokal
22. Sistem hotel/akomodasi block booking untuk tamu luar kota
23. Sistem ride-sharing/carpooling antar tamu menuju lokasi acara
24. Analitik lanjutan: heatmap interaksi halaman undangan
25. A/B testing template bagi pengguna Business/Enterprise
26. Progressive Web App (PWA) agar undangan dapat "diinstal" ke home screen tamu
27. Fitur cetak undangan fisik on-demand terintegrasi (hybrid digital-fisik)
28. Sistem notifikasi WhatsApp otomatis H-7, H-1 kepada tamu yang belum RSVP
29. Fitur "amplop digital kolektif" — patungan kado dari beberapa tamu sekaligus
30. Dashboard khusus vendor pernikahan untuk mengelola beberapa event organizer sekaligus (multi-level reseller)
31. Sertifikasi/badge "Verified Vendor" bagi reseller berkinerja tinggi
32. API publik bagi pengembang pihak ketiga untuk membangun integrasi kustom
33. Modul CRM ringan bagi Wedding Organizer untuk mengelola leads klien
34. Fitur voting tema/lagu oleh tamu sebelum acara
35. Personalisasi undangan berbasis data (nama tamu otomatis muncul di headline undangan)
36. Dukungan format undangan audio-visual pendek (video 15 detik) sebagai pengganti kartu statis
37. Marketplace preset musik latar berlisensi resmi
38. Sistem afiliasi multi-tier (reseller merekrut sub-reseller)
39. Dashboard "Wedding Day Command Center" — panel real-time hari-H (RSVP, check-in, amplop) dalam satu layar
40. Integrasi kalender vendor untuk menghindari bentrok jadwal venue
41. Fitur souvenir digital (e-souvenir) yang dikirim otomatis pasca-acara
42. Modul ucapan terima kasih otomatis (thank you note digital) ke seluruh tamu yang hadir
43. Personal wedding hashtag generator untuk media sosial
44. Fitur galeri kolaboratif — tamu dapat mengunggah foto momen acara langsung ke galeri undangan
45. Dukungan mata uang & metode pembayaran lokal negara ekspansi (Malaysia, Filipina)
46. White-label penuh untuk enterprise (domain terpisah sepenuhnya dari brand Undangan.Digital)
47. Sistem sertifikat digital kehadiran untuk acara korporat/wisuda
48. Modul survei kepuasan pasca-acara otomatis ke tamu
49. Integrasi dengan asisten virtual (Google Assistant/Siri) untuk pengingat RSVP
50. Machine learning untuk prediksi jumlah kehadiran aktual berdasarkan pola RSVP historis
51. Dukungan aksesibilitas lanjutan (pembaca layar penuh, mode kontras tinggi) untuk tamu difabel
52. Fitur "gift wishlist" sederhana tanpa perlu marketplace pihak ketiga penuh

---

## 32. Appendix

### 32.1 Kesimpulan

Digital Invitation Platform (Undangan.Digital) diposisikan untuk mengisi celah nyata di pasar Asia Tenggara, khususnya Indonesia: menggabungkan kematangan produk self-service kelas dunia dengan pemahaman mendalam atas kebiasaan lokal (distribusi WhatsApp, pembayaran QRIS, budaya amplop/angpau) serta model bisnis reseller yang belum dieksekusi matang oleh kompetitor mana pun di segmen ini. Keberhasilan produk ini bergantung pada tiga hal yang harus dijaga konsisten oleh seluruh tim lintas fungsi: **keandalan teknis saat momen krusial**, **kesederhanaan pengalaman bagi pengguna awam**, dan **kejelasan nilai ekonomi bagi mitra reseller**.

### 32.2 Dokumen Referensi

- `docs/01-planning/02-UIUX-Guideline.md` — UI/UX Guideline
- `docs/02-srs/01-Introduction.md` s/d `08-Appendix.md` — Software Requirements Specification (SRS)
- `docs/03-sad/01-System-Architecture.md` — Software Architecture Document (SAD): Arsitektur Sistem
- `docs/03-sad/02-Frontend-Architecture.md` — SAD: Arsitektur Frontend (Next.js)
- `docs/03-sad/03-Backend-Architecture.md` — SAD: Arsitektur Backend (Laravel 12)
- `docs/03-sad/04-Database-Architecture.md` — SAD: Arsitektur Database
- `docs/04-api/API-Specification.md` — API Specification
- Roadmap & PRD internal — "Platform SaaS Undangan Digital" (dokumen pendahulu, digunakan sebagai basis konsistensi fitur & arsitektur)

> Dokumen-dokumen di atas belum seluruhnya dibuat pada tahap ini. Daftar ini adalah peta rujukan resmi begitu masing-masing file dibuat, agar penomoran dan penamaan sejak awal konsisten dengan struktur repository.

### 32.3 Glosarium Singkat

| Istilah | Definisi |
|---|---|
| RSVP | *Répondez s'il vous plaît* — konfirmasi kehadiran tamu terhadap undangan |
| White-label | Produk yang dijual ulang oleh mitra dengan branding mitra sendiri, tanpa identitas brand platform asli terlihat |
| GMV | Gross Merchandise Value — total nilai transaksi yang diproses melalui platform |
| MRR | Monthly Recurring Revenue — pendapatan berulang bulanan |
| Take Rate | Persentase komisi yang diambil platform dari nilai transaksi yang difasilitasi |
| Ownership Middleware | Lapisan validasi teknis yang memastikan pengguna hanya dapat mengakses data miliknya sendiri |
| QRIS | Quick Response Code Indonesian Standard — standar QR pembayaran nasional Indonesia |

### 32.4 Catatan Tambahan

Dokumen ini akan direvisi setiap kali terjadi perubahan signifikan pada scope produk, model bisnis, atau hasil riset pasar baru. Setiap revisi wajib dicatat pada tabel Riwayat Revisi (Bab 1.2) dan dikomunikasikan ke seluruh stakeholder yang tercantum pada Bab 11.

---

*Dokumen ini bersifat internal dan confidential. Dilarang mendistribusikan ke pihak eksternal tanpa persetujuan Product Management Office.*

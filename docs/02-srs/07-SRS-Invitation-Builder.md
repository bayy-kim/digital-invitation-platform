SOFTWARE REQUIREMENTS SPECIFICATION (SRS)



Proyek: Platform SaaS \& E-Commerce Undangan Digital



Modul: Guest Invitation (Halaman Publik Undangan)



Status: Disetujui untuk Pengembangan



1\. Tujuan Modul



Modul Guest Invitation adalah halaman publik akhir (end-product) yang akan diakses oleh tamu undangan melalui URL unik (misal: domain.com/pernikahan/budi-rara?to=Nama+Tamu). Tujuan utama modul ini adalah menyajikan informasi pernikahan secara interaktif, estetis, dan informatif secara mobile-first.



Mengingat lebih dari 95% tamu mengakses undangan melalui perangkat seluler dengan kondisi jaringan yang bervariasi, modul ini dirancang dengan optimasi performa ekstrem (ringan, asinkron, dan ramah SEO) serta dilengkapi fitur interaktif seperti konfirmasi kehadiran (RSVP), ucapan digital, integrasi penunjuk arah (Google Maps), scan QR Check-in, dan pengiriman hadiah digital.



2\. Functional Requirements (Kebutuhan Fungsional)



Sistem harus memfasilitasi fungsionalitas berikut pada halaman publik undangan:



FR-01 (Loading Screen \& Progressive Asset Loading)



Sebelum seluruh aset termuat sempurna, sistem menampilkan halaman pemuatan (loading screen) bertema minimalis dengan logo pengantin atau inisial nama mempelai yang beranimasi halus (skeleton loading atau spinner elegan).



Sistem memuat skrip fundamental terlebih dahulu, menunda (defer) pemuatan gambar galeri berukuran besar hingga transisi cover selesai terbuka.



FR-02 (Cover Screen \& Dynamic Guest Name)



Halaman pertama yang wajib dilihat tamu adalah Cover depan yang menampilkan foto utama/ilustrasi, nama pasangan mempelai, tanggal acara, serta baris nama tamu dinamis yang diambil dari parameter URL ?to=.



Menyediakan tombol interaktif "Buka Undangan" (Open Invitation) sebagai satu-satunya pemicu (trigger) untuk masuk ke halaman utama undangan.



FR-03 (Autoplay Backsound \& Audio Control)



Musik latar (backsound) bawaan atau MP3 yang diunggah pengguna wajib diputar secara otomatis (autoplay) segera setelah tamu mengeklik tombol "Buka Undangan". Hal ini dilakukan untuk mematuhi kebijakan keamanan browser modern yang melarang autoplay audio sebelum adanya interaksi pengguna.



Menyediakan tombol kontrol audio (floating button) yang menetap di pojok layar untuk menjeda (pause) atau memutar kembali (play) musik kapan saja.



FR-04 (Animasi Masuk \& Scroll Navigation)



Menggunakan efek transisi animasi masuk (fade-in, slide-up, atau parallax) yang halus saat pengguna menggulir layar ke bawah menggunakan library visual (seperti Framer Motion di Next.js).



Menyediakan sticky navigation menu atau tombol cepat (floating menu di bagian bawah pada mobile) untuk melompat langsung ke bagian tertentu (Mempelai, Acara, Galeri, RSVP, Gift).



FR-05 (Hero \& Detail Profil Mempelai)



Menampilkan foto profil premium, nama lengkap, nama panggilan, nama orang tua, serta urutan silsilah keluarga (putra/putri ke-X) dari masing-masing mempelai pria dan wanita.



Menyediakan tombol tautan ikonik langsung ke profil sosial media (Instagram/TikTok) mempelai yang aktif.



FR-06 (Countdown Timer \& Detail Acara)



Menampilkan jam hitung mundur (countdown timer) yang presisi dan real-time menuju waktu akad atau resepsi pernikahan.



Menampilkan informasi terperinci mengenai daftar agenda acara (Akad Nikah, Pemberkatan, Resepsi, Unduh Mantu, dll) lengkap dengan hari, tanggal, jam mulai hingga selesai, nama gedung/tempat, serta alamat lengkap.



FR-07 (Google Maps Integration \& Navigation)



Merender peta lokasi acara secara interaktif (embedded interactive Google Maps iframe).



Menyediakan tombol navigasi "Petunjuk Arah" (Get Directions) yang mengarahkan tamu langsung ke aplikasi Google Maps atau Waze menggunakan koordinat geografis presisi (latitude \& longitude).



FR-08 (Love Story Timeline)



Menyajikan riwayat perjalanan kisah cinta pasangan mempelai (perkenalan, lamaran, keputusan menikah) dalam bentuk linimasa (timeline) kronologis yang dilengkapi teks deskriptif, tanggal, dan foto pendukung.



FR-09 (Gallery Photo \& Video Integration)



Menyajikan album foto prewedding dalam bentuk grid estetis dengan fitur Lightbox (foto membesar saat diklik dan dapat digeser kanan-kiri).



Menyediakan pemutar video (embedded video player) dari tautan YouTube atau Vimeo secara responsif tanpa menurunkan performa muat halaman.



FR-10 (Digital Gift \& QRIS Amplop Digital)



Menampilkan informasi opsi pemberian hadiah secara digital (cashless) berupa:



Nomor rekening bank (disertai tombol pintas "Salin Nomor Rekening").



Akun e-wallet (Dana, GoPay, OVO).



Gambar kode QRIS dinamis/statis untuk transfer instan.



Menampilkan alamat pengiriman fisik milik pengantin bagi tamu yang ingin mengirimkan kado fisik secara langsung.



FR-11 (Dynamic RSVP Form)



Menyediakan form konfirmasi kehadiran interaktif yang meminta tamu menginput:



Nama Tamu (otomatis terisi dari parameter URL ?to=, namun tetap dapat diedit).



Konfirmasi Kehadiran (Hadir, Tidak Hadir, Ragu-ragu).



Jumlah Pax/Tamu yang akan ikut hadir (pilihan dropdown integer: 1 - 5).



Opsi Tambahan (misal: pilihan menu makanan atau sesi jam kehadiran, jika diaktifkan di Builder).



FR-12 (Guest Book \& Wishes Wall)



Menyajikan papan ucapan berisi doa, selamat, dan pesan-pesan dari tamu undangan (Wishes Wall).



Tamu dapat mengirimkan ucapan secara langsung melalui form ucapan. Ucapan baru harus langsung muncul di papan ucapan secara dinamis (real-time update).



FR-13 (Share Invitation \& Copy Link)



Menyediakan tombol berbagi (Share) di bagian akhir atau navigasi untuk mempermudah tamu menyebarkan kembali link undangan tersebut melalui WhatsApp, Facebook, Twitter, atau menyalin tautan langsung ke clipboard.



FR-14 (Watermark Branding Platform)



Menampilkan logo/teks kecil di bagian kaki (footer) undangan: "Powered by \[NamaPlatformSaaS]".



Watermark ini otomatis disembunyikan jika pengguna (mempelai) berlangganan paket Premium/Pro atau reseller mengaktifkan fitur White-Label.



FR-15 (SEO Metadata, Open Graph, \& WhatsApp Preview)



Sistem harus merender tag SEO dinamis secara Server-Side di Next.js:



<title>: "Undangan Pernikahan \[Pria] \& \[Wanita]"



<meta name="description">: Kalimat undangan yang dipersonalisasi.



<meta property="og:image">: Gambar OpenGraph kustom (foto cover pengantin atau gambar terkompresi dengan teks terintegrasi) agar saat tautan dibagikan di WhatsApp/Telegram, muncul preview gambar yang mewah dan representatif.



3\. Non-Functional Requirements (Kebutuhan Non-Fungsional)



A. Performance \& Rendering Speed (Core Web Vitals)



Pemuatan Awal (Page Load Speed): Halaman publik harus bisa diakses penuh di bawah 1.5 detik pada jaringan 4G stabil.



Largest Contentful Paint (LCP): Harus kurang dari 2.0 detik. Gambar cover utama wajib menggunakan tag optimasi Next.js (next/image) dengan prioritas tinggi (priority={true}).



Cumulative Layout Shift (CLS): Harus bernilai 0 atau kurang dari 0.1. Dimensi area maps, galeri, dan avatar mempelai wajib memiliki cadangan ruang (skeleton placeholder) untuk mencegah layout melompat saat aset dimuat.



Server-Side Rendering (SSR) \& Caching: Menggunakan skema Incremental Static Regeneration (ISR) di Next.js dengan siklus revalidasi otomatis (misal: revalidate tiap 60 detik) untuk data ucapan baru, sedangkan halaman HTML statis dilayani langsung dari CDN (Cloudflare) guna memangkas beban database MySQL/PostgreSQL hingga 90%.



B. Mobile-First Responsiveness



Tampilan wajib dioptimasi 100% untuk layar ponsel cerdas Android dan iOS dengan resolusi layar mulai dari 320px (iPhone SE) hingga ukuran tablet/desktop. Layout harus mengalir lancar secara vertikal tanpa adanya kerusakan visual atau teks terpotong (overflow).



C. Image \& Media Optimization



Semua gambar yang diunggah mempelai wajib dilayani melalui CDN (Cloudinary/S3) dalam format modern (WebP atau AVIF).



Kompresi otomatis diterapkan secara agresif sehingga tidak ada gambar yang berukuran di atas 400KB untuk versi mobile.



D. Accessibility



Kontras warna teks dengan latar belakang wajib memenuhi standar WCAG 2.0 (kontras minimal 4.5:1).



Seluruh elemen interaktif (tombol "Buka Undangan", tab, input form) harus ramah sentuhan dengan ukuran tap area minimal 44px x 44px.



4\. User Flow (Alur Pengguna Tamu)



&#x20; \[ Tamu Klik Link WA/Sosmed ]

&#x20;              ↓

&#x20;    \[ Loading Screen (Sistem memuat aset kritis) ]

&#x20;              ↓

&#x20;  \[ Cover Screen (Tampil nama pengantin \& nama tamu dinamis) ]

&#x20;              ↓

&#x20;\[ Tamu Klik "Buka Undangan" ] → \[ Musik Autoplay Aktif ] → \[ Layar Terbuka/Scroll Aktif ]

&#x20;                                                                      ↓

&#x20;                                                           \[ Baca Profil Mempelai ]

&#x20;                                                                      ↓

&#x20;                                                              \[ Lihat Detail Acara ]

&#x20;                                                                      ↓

&#x20;                                                          \[ Cek Peta \& Klik Petunjuk ]

&#x20;                                                                      ↓

&#x20;                                                          \[ Lihat Galeri \& Video ]

&#x20;                                                                      ↓

&#x20;                                                           \[ Kirim Amplop Digital ]

&#x20;                                                                      ↓

&#x20;                                                          \[ Isi Form RSVP \& Wishes ]

&#x20;                                                                      ↓

&#x20;                                                           \[ Klik CTA Share/Selesai ]





5\. Validasi Sistem (Data \& Parameter)



Validasi Query Parameter to:



Sistem menyaring input parameter ?to= untuk menghilangkan karakter HTML/skrip berbahaya. Jika parameter kosong atau tidak ada, tampilan nama tamu digantikan dengan teks standar yang ramah (misal: "Tamu Undangan" atau "Sahabat \& Keluarga").



Validasi Form RSVP:



guest\_name: Wajib diisi, maksimal 50 karakter.



attendance: Harus bernilai enum saja (Hadir, Tidak Hadir, Ragu-Ragu).



total\_persons: Integer positif. Wajib divalidasi antara angka 1 hingga batas maksimal pax yang ditentukan pengantin (default: maksimal 5 per undangan).



Validasi Form Wishes/Ucapan:



message: Wajib diisi, minimal 5 karakter, maksimal 500 karakter. Disaring (sanitized) dari karakter tag HTML.



Validasi Google Maps \& Embed Iframe:



Koordinat latitude dan longitude harus berupa format kordinat desimal standar.



6\. Hak Akses (Role-Based Access Control)



Guest (Tamu Publik - Unauthenticated):



Hak Akses: Hanya memiliki hak baca (read-only) pada seluruh konten undangan, hak tulis (write) untuk mengirimkan data RSVP dan data Buku Tamu (Wishes), serta hak transfer uang/akses QRIS (melalui visualisasi info rekening).



Batasan: Tidak memiliki akses ke panel administrasi, builder, atau melihat data tamu lain selain ucapan yang di-publish.



Customer / Mempelai (Pemilik Undangan):



Memiliki akses untuk menguji tampilan publik. Dapat memoderasi ucapan tamu (menghapus ucapan yang tidak pantas) melalui Dashboard Customer.



Reseller, Admin, \& Super Admin:



Memiliki hak pengawasan global. Dapat menonaktifkan (suspend) halaman publik undangan jika melanggar syarat ketentuan (misal: digunakan untuk penipuan, konten asusila, dll).



7\. Error Handling (Penanganan Kesalahan)



Kasus: Undangan tidak ditemukan (404/Slug Salah).



Solusi: Sistem tidak memunculkan blank page atau sistem eror mentah Laravel/Next.js. Pengguna dialihkan ke halaman 404 estetik bertuliskan: "Maaf, halaman undangan tidak ditemukan atau alamat URL salah. Silakan periksa kembali tautan Anda." Disertai tombol arah ke Beranda platform.



Kasus: Undangan telah Kedaluwarsa (Expired).



Solusi: Jika waktu expired\_at pada tabel undangan telah lewat, sistem merender halaman terkunci bertuliskan: "Masa aktif undangan ini telah berakhir. Terima kasih atas perhatian Anda."



Kasus: Gagal RSVP / Papan Ucapan Error.



Solusi: Jika database sibuk atau koneksi internet tamu terputus saat submit form, tampilkan pesan peringatan halus (Toast Notification): "Gagal mengirim konfirmasi. Silakan periksa koneksi internet Anda dan coba lagi."



Kasus: Browser Memblokir Autoplay Musik.



Solusi: Jika browser tamu memiliki kebijakan ketat yang memblokir audio (terutama Safari iOS lama), sistem tidak boleh crash. Pemutar audio akan masuk ke status paused dan ikon musik di pojok kanan akan beranimasi statis dengan ikon mute, mengundang tamu untuk mengeklik tombol putar manual secara intuitif.



8\. Security (Standar Keamanan)



Public Access Isolation:



Endpoint publik halaman undangan hanya mengekspos data non-sensitif. Data sensitif pengantin seperti email akun pengguna, nomor telepon pribadi (jika disembunyikan), atau log aktivitas sistem tidak boleh dimasukkan ke dalam JSON payload response.



Anti Enumeration \& Obfuscated ID:



Link undangan wajib menggunakan sistem kustom slug berbasis teks (misal: /pernikahan/budi-dan-rara) atau UUID v4, bukan menggunakan ID increment integer (/pernikahan/12) untuk mencegah pihak luar melakukan eksploitasi data tamu dengan menebak urutan ID.



Spam \& Flooding Protection (Rate Limiting):



Mengaktifkan rate limiter pada API endpoint /rsvp dan /guest-book berbasis alamat IP tamu. Maksimal tamu hanya boleh mengirim 3 kali RSVP/ucapan dalam kurun waktu 5 menit untuk mencegah bot spam membanjiri database pengantin.



XSS (Cross-Site Scripting) Protection:



Seluruh teks doa dan ucapan tamu wajib melalui tahap sanitasi ketat di sisi Backend Laravel sebelum disimpan di database. Sebelum dirender di Next.js, pastikan menggunakan parser teks aman, dilarang merender langsung menggunakan dangerouslySetInnerHTML tanpa sanitasi HTML Purifier terlebih dahulu.



9\. Struktur Database yang Dibutuhkan (Entity-Relationship)



Modul ini akan membaca data dari tabel master yang dibuat dari modul Builder dan menulis data baru ke beberapa tabel interaksi tamu berikut:



&#x20;   +-------------------+             +-----------------------+

&#x20;   |    Invitations    | <─────────1 |  Public\_Invitations   |

&#x20;   +-------------------+             +-----------------------+

&#x20;             │ 1                             │ 1

&#x20;             │                               │

&#x20;             ├───────────1:N──────────────┐  ├───────────1:N──────────────┐

&#x20;             ▼                            ▼  ▼                            ▼

&#x20;     +---------------+             +--------------+                +--------------+

&#x20;     |  Guest\_RSVPs  |             | Guest\_Books  |                |  Analytics   |

&#x20;     +---------------+             +--------------+                +--------------+





Penjelasan Tabel Spesifik Modul Tamu:



Public\_Invitations (Virtual View / Sub-Tabel):

Berisi replika data live dari Invitations dan Invitation\_Drafts yang sudah di-publish untuk akses publik tercepat.



Guest\_RSVPs:

Menyimpan konfirmasi kehadiran dari para tamu.



Kolom: id (UUID), invitation\_id (FK), guest\_name, attendance (enum), total\_persons (int), created\_at.



Guest\_Books:

Menyimpan ucapan selamat dan doa restu tamu yang akan ditampilkan di halaman undangan.



Kolom: id (UUID), invitation\_id (FK), guest\_name, message (text), is\_approved (boolean, default: true), created\_at.



Invitation\_Analytics:

Menyimpan metrik performa kunjungan secara anonim untuk statistik di dashboard pengantin.



Kolom: id, invitation\_id (FK), ip\_address\_hash, user\_agent, referrer, viewed\_at.



10\. Daftar API yang Dibutuhkan (Endpoint Definition)



Sistem menggunakan endpoint API Next.js yang terhubung aman ke backend Laravel Headless:



GET /api/v1/invitation/{slug}



Fungsi: Mengambil payload data lengkap konten publik undangan (Profil, Acara, Tema, Musik, Link Galeri) berdasarkan slug URL unik untuk dirender di halaman Next.js.



GET /api/v1/invitation/{id}/gallery



Fungsi: Mengambil array daftar URL foto galeri beresolusi tinggi dengan teknik deferred load.



GET /api/v1/invitation/{id}/guest-book



Fungsi: Mengambil daftar ucapan selamat (Wishes) terbaru dari tamu yang berstatus is\_approved = true dengan sistem paginasi tak terbatas (infinite scroll pagination).



POST /api/v1/invitation/{id}/rsvp



Fungsi: Menyimpan data kehadiran tamu baru ke database. Endpoint ini diamankan dengan invisible reCAPTCHA v3.



POST /api/v1/invitation/{id}/guest-book



Fungsi: Mengirimkan doa/ucapan selamat baru dari tamu ke database. Teks otomatis disanitasi dari potensi tag skrip berbahaya sebelum disimpan.



POST /api/v1/invitation/{id}/analytics



Fungsi: Menerima ping asinkron setiap kali halaman dibuka untuk mencatat metrik kunjungan unik (Unique Views) tanpa memblokir kecepatan interaksi pengguna utama.


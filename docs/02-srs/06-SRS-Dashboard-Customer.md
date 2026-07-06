SOFTWARE REQUIREMENTS SPECIFICATION (SRS)



Proyek: Platform SaaS \& E-Commerce Undangan Digital

Modul: Dashboard Customer (CMS \& Analytics)

Status: Disetujui untuk Pengembangan



1\. Tujuan Modul



Modul Dashboard Customer merupakan ruang kerja utama (workspace) bagi pelanggan (Customer/Reseller) setelah mereka melakukan registrasi dan pembelian. Tujuannya adalah menyediakan antarmuka Content Management System (CMS) mandiri yang intuitif untuk mengedit konten undangan (teks, foto, jadwal acara), mengatur fitur (Guest Book, Amplop Digital), serta memantau performa undangan (Statistik \& RSVP) secara real-time. Modul ini harus memberikan pengalaman seperti menggunakan website builder profesional tanpa memerlukan keahlian coding.



2\. Functional Requirements (Kebutuhan Fungsional)



Sistem harus memfasilitasi fungsionalitas berikut:



FR-01 (Dashboard Overview): Menampilkan ringkasan akun dengan sapaan pengguna, metrik utama (Total Pengunjung, RSVP Hadir, Total Amplop Digital), Quick Action (Buat Undangan Baru, Sebar Link), dan tabel aktivitas terakhir (Notifikasi RSVP masuk).



FR-02 (Manajemen Daftar Undangan): Menampilkan daftar undangan yang dimiliki user beserta statusnya (Draft, Active, Expired). Menyediakan opsi: Edit, Preview, Publish/Unpublish, Duplicate (Duplikasi), dan Hapus Undangan.



FR-03 (Builder - Data Mempelai \& Acara): Form input untuk mengelola profil mempelai (pria/wanita, nama orang tua, sosial media) dan detail acara (jenis acara, tanggal, jam, lokasi lengkap).



FR-04 (Builder - Integrasi Media \& Konten):



Upload Foto/Galeri: Mengelola foto prewedding dengan fitur drag \& drop, hapus, dan urutkan.



Upload Video: Memasukkan link YouTube/Vimeo atau upload file video pendek.



Upload Musik: Memilih musik background dari library sistem atau upload file MP3 pribadi.



Love Story: Form dinamis untuk membuat timeline cerita cinta (tanggal, judul, deskripsi).



FR-05 (Builder - Fitur Pintar \& Peta):



Google Maps: Input link Google Maps yang otomatis dikonversi menjadi iframe/embed map dan peta interaktif.



Countdown: Pengaturan hitung mundur menuju tanggal acara utama.



FR-06 (Builder - Interaksi Tamu):



RSVP \& Buku Tamu: Pengaturan untuk mengaktifkan/menonaktifkan form RSVP. Terdapat antarmuka untuk membaca, menghapus (moderasi), atau membalas ucapan tamu.



Gift / Amplop Digital: Form untuk menambahkan daftar rekening bank, e-wallet, atau mengunggah gambar QRIS.



FR-07 (Distribusi \& QR Code): Fitur untuk men-generate link khusus per nama tamu (Share Link), men-generate QR Code Check-in untuk tamu, serta mengunduh QR Code tersebut dalam format PNG/PDF.



FR-08 (Pengaturan Lanjutan \& SEO): Konfigurasi slug URL, pengaturan Custom Domain (request mapping ke namamempelai.com), dan pengaturan meta SEO (Judul, Deskripsi, Thumbnail WhatsApp).



FR-09 (Statistik \& Riwayat): Halaman analitik khusus per undangan untuk melihat grafik kunjungan, persentase RSVP (Hadir/Tidak/Ragu), dan riwayat pembayaran (untuk mengunduh invoice atau melakukan Perpanjang Masa Aktif).



FR-10 (Auto Save, Draft, \& Versioning): Sistem secara otomatis menyimpan perubahan (Auto Save) setiap kali pengguna mengetik/mengubah nilai. Jika belum dipublish, status tetap Draft. Menyediakan Version History sederhana untuk membatalkan perubahan terakhir.



3\. Non-Functional Requirements (Kebutuhan Non-Fungsional)



Performance (Upload \& Rendering): Menggunakan teknik chunking atau unggah langsung ke Cloud Storage (S3/Cloudinary) dari klien (Presigned URL) untuk menghindari beban server. Proses render antarmuka form tidak boleh lag (berjalan pada 60fps).



Auto Save \& Real-Time Preview: State management (menggunakan Redux/Zustand di Next.js) harus sinkron dengan iframe preview undangan dalam waktu kurang dari 100ms. Panggilan API untuk Auto Save harus di-debounce (misal: 1.5 detik setelah berhenti mengetik) untuk mengurangi beban API.



Scalability \& Availability: Modul harus stabil meski diakses oleh ratusan Reseller yang mengedit undangan secara bersamaan.



Security \& Data Isolation: Memastikan isolasi data secara ketat. Satu pelanggan tidak boleh bisa membaca, mengedit, atau menghapus data (terutama URL aset gambar) milik pelanggan lain.



Responsive \& Accessibility: Dashboard wajib responsif. Pada mobile, area builder memprioritaskan form isian dan menyembunyikan live preview ke dalam sebuah tombol (Floating Action Button).



4\. User Flow (Alur Pengguna)



Login \& Navigasi: Pengguna masuk -> Diarahkan ke Dashboard Utama -> Melihat ringkasan analitik.



Pemilihan Mode: Pengguna menavigasi ke menu "Daftar Undangan" -> Mengklik tombol "Edit" pada salah satu undangan.



Proses Editing (Builder): Pengguna masuk ke Workspace (Form di kiri, Preview di kanan) -> Mengisi Data Mempelai -> Auto Save berjalan di latar belakang -> Memasukkan foto galeri -> Live preview otomatis memperbarui tampilan.



Penerbitan: Pengguna puas dengan hasil -> Klik tombol "Publish" -> Sistem memvalidasi kelengkapan data wajib -> Status berubah menjadi Active.



Distribusi: Pengguna berpindah ke tab "Sebar Undangan" -> Memasukkan nama tamu -> Menggandakan tautan (copy link) -> Bagikan ke WhatsApp.



Monitoring: Pengguna kembali ke Dashboard hari berikutnya -> Masuk ke tab "RSVP" -> Melihat daftar tamu yang mengonfirmasi kehadiran -> (Selesai).



5\. Validasi Sistem



Validasi Upload Media:



Foto: Maksimal 2MB per gambar, format diizinkan: JPG, PNG, WEBP.



Video: Jika upload, maksimal 20MB (MP4). Jika link YouTube, harus format URL YouTube yang valid.



Musik: Maksimal 5MB (MP3).



Validasi Waktu \& Tanggal: Tanggal akhir acara tidak boleh lebih awal dari tanggal mulai. Jika status undangan Active, tanggal acara tidak boleh berada di masa lalu melebihi masa kedaluwarsa sistem.



Validasi Link \& Domain: Memastikan tautan Google Maps menggunakan URL yang valid (maps.google.com atau goo.gl/maps). Pendaftaran Custom Domain divalidasi ketersediaan dan format FQDN-nya (Fully Qualified Domain Name).



Validasi Publish: Mencegah undangan di-publish jika atribut mandatory (Nama Mempelai, Tanggal Acara) masih kosong.



6\. Hak Akses (Role-Based Access Control)



Customer: Memiliki akses penuh (Create, Read, Update, Delete) hanya pada data undangan, foto, RSVP, dan riwayat pesanan yang terkait dengan user\_id mereka sendiri.



Reseller: Sama seperti Customer, tetapi dapat mengatur logo white-label pada dashboard mereka, dan memanajemen data undangan dari klien mereka (sub-users/guests).



Admin: Memiliki akses Read-Only ke dashboard pengguna melalui fitur "Impersonate" (login sebagai user terkait) hanya untuk keperluan Customer Support / Troubleshooting. Tidak bisa mengubah data tanpa log audit.



Super Admin: Akses global. Dapat melakukan force delete atau me-suspend undangan yang melanggar ketentuan (misal: konten ilegal/spam).



7\. Error Handling (Penanganan Kesalahan)



Kasus: Auto Save Gagal (Koneksi Putus). UI merespons dengan badge merah "Koneksi terputus. Data disimpan secara lokal". Menyimpan state di LocalStorage sementara.



Kasus: Upload Gagal / File Terlalu Besar. Menampilkan Toast Alert: "Gagal mengunggah. Ukuran foto maksimal adalah 2MB. Silakan kompres foto Anda terlebih dahulu."



Kasus: Storage/Kuota Penuh. Menampilkan Modal: "Kuota penyimpanan Anda telah habis. Silakan hapus beberapa foto atau tingkatkan (upgrade) paket berlangganan Anda."



Kasus: Domain Gagal Diverifikasi. Status domain berubah menjadi Warning, menampilkan instruksi teknis (DNS CNAME record) dan anjuran menghubungi Support.



8\. Security (Standar Keamanan Sistem)



Ownership Validation (Mutlak): Di backend (Laravel), setiap endpoint API (GET/PUT/DELETE) harus memiliki Middleware yang memvalidasi bahwa invitation\_id yang sedang diakses benar-benar milik user\_id dari token JWT saat ini. (SELECT \* FROM invitations WHERE id = ? AND user\_id = ?)



File Upload Scan \& Sanitization: Ekstensi file divalidasi di sisi server menggunakan Magic Bytes (bukan sekadar nama ekstensi). File .php, .js, atau executable langsung ditolak untuk mencegah celah Remote Code Execution (RCE).



Anti XSS (Cross-Site Scripting): Input berbasis teks (terutama nama tamu, Love Story, dan Guest Book) wajib di-escape atau di-sanitize sebelum dirender di DOM frontend untuk mencegah injeksi script.



Secure File Access: Unduhan QR code massal atau ekspor data RSVP ke Excel dibatasi dengan Rate Limiting untuk mencegah Denial of Wallet / DDoS.



9\. Struktur Database yang Dibutuhkan (Entity-Relationship)



Invitations (Tabel Induk): id, user\_id, template\_id, slug, status (draft/active), expired\_at.



Invitation\_Settings: id, invitation\_id, custom\_domain, seo\_title, seo\_desc, auto\_play\_music.



Bride\_Grooms: id, invitation\_id, type (groom/bride), name, nickname, parents\_name, ig\_handle, photo\_url.



Events: id, invitation\_id, title (Akad/Resepsi), event\_date, start\_time, end\_time, location\_name, address, maps\_url, is\_primary.



Stories: id, invitation\_id, date, title, description, image\_url.



Galleries: id, invitation\_id, file\_url, sort\_order.



Videos \& Musics: Tabel untuk menyimpan URL embed video dan aset MP3 yang menempel ke invitation\_id.



Gifts: id, invitation\_id, bank\_name, account\_number, account\_name, qr\_code\_url.



RSVPs \& Guest\_Books: (Gabungan atau pisah tabel). id, invitation\_id, guest\_name, attendance (yes/no/maybe), total\_persons, message.



Invitation\_Statistics: id, invitation\_id, unique\_views, last\_viewed\_at (Diupdate secara asinkron/Redis).



Activity\_Logs: id, user\_id, action (e.g., "Published Invitation", "Updated Gallery"), ip\_address, created\_at.



10\. Daftar API yang Dibutuhkan (Endpoint Definition)



Backend harus menyediakan REST API berikut untuk antarmuka Next.js Dashboard:



GET /api/v1/dashboard/overview

Fungsi: Menarik ringkasan metrik statistik global (total views, total RSVP, recent activities) untuk akun user tersebut.



GET /api/v1/invitations

Fungsi: Mengambil daftar seluruh undangan milik user dengan parameter pagination dan search.



GET /api/v1/invitations/{id}

Fungsi: Menarik payload data lengkap (mempelai, acara, galeri, settings) untuk dimuat ke dalam state Builder Editor.



PUT /api/v1/invitations/{id}

Fungsi: Endpoint Auto Save. Menerima payload JSON parsial dari aktivitas Builder untuk memperbarui data teks/pengaturan.



POST /api/v1/invitations/{id}/media

Fungsi: Menerima pengiriman file (Multipart/form-data) untuk Foto Galeri, Musik, atau QRIS Gift. Mengembalikan URL CDN untuk di-render oleh klien.



POST /api/v1/invitations/{id}/publish

Fungsi: Mengubah status dari Draft ke Active. Melakukan validasi mandatory fields sebelum menyetujui.



POST /api/v1/invitations/{id}/duplicate

Fungsi: Membuat kloning dari struktur undangan yang ada (deep copy ke baris baru) untuk mempermudah pengerjaan undangan kedua.



GET /api/v1/invitations/{id}/rsvps

Fungsi: Mengambil daftar tamu yang mengisi RSVP \& Buku Tamu untuk di-render ke dalam tabel analitik. Mendukung export CSV.



POST /api/v1/invitations/{id}/share

Fungsi: Generate link dan QR Code dinamis berbasis teks nama tamu untuk disebar via WhatsApp.


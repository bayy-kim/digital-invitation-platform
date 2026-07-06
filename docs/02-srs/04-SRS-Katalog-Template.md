SOFTWARE REQUIREMENTS SPECIFICATION (SRS)



Proyek: Platform SaaS \& E-Commerce Undangan Digital

Modul: Katalog Template

Status: Disetujui untuk Pengembangan



1\. Tujuan Modul



Modul Katalog Template berfungsi sebagai "etalase utama" (storefront) dari platform SaaS ini. Tujuannya adalah untuk memudahkan pengunjung dan pengguna terdaftar dalam menemukan, menyaring, mempratinjau, dan memilih desain undangan digital yang paling sesuai dengan kebutuhan mereka. Modul ini dirancang untuk memberikan pengalaman penemuan produk (product discovery) yang cepat, visual, dan sangat intuitif guna meningkatkan rasio konversi pembelian atau penggunaan template.



2\. Functional Requirements (Kebutuhan Fungsional)



Sistem harus mampu menjalankan fungsi-fungsi berikut:



FR-01 (Menampilkan Daftar Template): Sistem harus menampilkan seluruh template aktif dalam format grid card yang rapi. Tampilan awal memuat template unggulan (Default) dan mendukung navigasi berbasis Pagination atau Infinite Scroll (pemuatan berlanjut saat pengguna menggulir ke bawah).



FR-02 (Filter Katalog Multikriteria): Sistem memungkinkan pengguna menyaring template berdasarkan kombinasi:



Kategori Acara: Pernikahan, Ulang Tahun, Aqiqah, Corporate, dll.



Warna Dominan: Menggunakan color swatch (pilihan warna visual).



Tema/Gaya: Minimalis, Elegan, Rustic, Floral, dll.



Harga: Gratis (Free), Premium (Berbayar), atau berdasarkan tier paket (Basic, Pro).



FR-03 (Pencarian / Search): Sistem menyediakan kolom pencarian berbasis teks untuk mencari template berdasarkan nama, keyword, atau tag spesifik (misal: "Undangan Adat Jawa").



FR-04 (Pengurutan / Sorting): Sistem memungkinkan pengguna mengurutkan hasil katalog berdasarkan: Terbaru (Newest), Terpopuler (Most Popular/Best Seller), Harga Terendah ke Tertinggi, dan Harga Tertinggi ke Terendah.



FR-05 (Sistem Badge / Label Visual): Sistem secara otomatis atau manual menampilkan badge pada card template, meliputi: Promo, Best Seller, New, Premium, atau Recommended.



FR-06 (Quick Preview): Saat pengguna mengarahkan kursor (hover) pada card template di desktop, sistem menampilkan auto-scroll preview (animasi gambar berjalan) atau menampilkan tombol "Lihat Cepat" yang membuka modal popup tanpa berpindah halaman.



FR-07 (Live Preview Interaktif): Sistem menyediakan halaman khusus (Detail Template) yang menampilkan iframe berbentuk mockup smartphone interaktif agar pengguna dapat menyimulasikan penggunaan template secara real-time.



FR-08 (Manajemen Wishlist / Favorit): Pengguna yang sudah login dapat menambahkan template ke daftar keinginan (wishlist) dengan mengklik ikon hati.



FR-09 (Rekomendasi \& Template Terkait): Pada halaman Detail Template, sistem menampilkan bagian "Mungkin Anda Suka" yang berisi template dengan kategori, tema, atau warna yang sejenis.



FR-10 (Bagikan / Share Template): Sistem menyediakan tombol untuk membagikan tautan template ke media sosial (WhatsApp, Facebook, X) atau tombol Copy Link.



3\. Non-Functional Requirements (Kebutuhan Non-Fungsional)



Performance (Loading Speed): Halaman katalog utama harus dimuat dalam waktu kurang dari 2 detik (First Contentful Paint). Pemuatan gambar harus menggunakan teknik Lazy Loading (gambar hanya dimuat saat masuk ke layar).



Image Optimization: Seluruh thumbnail dan aset gambar wajib diubah ukurannya secara otomatis dan disajikan dalam format modern (WebP/AVIF) melalui layanan CDN/Image Optimizer (seperti Cloudinary).



Caching: Hasil kueri pencarian populer dan filter default harus di-cache menggunakan Redis agar tidak membebani pangkalan data relasional pada saat trafik tinggi.



SEO (Search Engine Optimization): URL halaman detail template harus menggunakan format yang ramah SEO (contoh: /template/undangan-pernikahan-rustic-elegan). Halaman harus dirender secara Server-Side (SSR/SSG) agar dapat diindeks oleh mesin pencari beserta meta tags dinamis (Title, Description, OpenGraph Image).



Responsive \& Accessibility: Modul wajib berfungsi sempurna di ukuran layar Desktop, Tablet, dan Mobile. Harus menggunakan tag semantik HTML dan atribut ARIA untuk aksesibilitas Screen Reader.



Scalability \& Availability: Harus mampu menangani lonjakan pencarian dan filtering secara paralel (misal: saat kampanye promo) dengan jaminan uptime 99.9%.



4\. User Flow (Alur Pengguna)



Navigasi Awal: Pengguna berada di Landing Page -> Klik tombol "Katalog" atau "Eksplorasi Template" di Navbar.



Eksplorasi (Katalog Utama): Sistem menampilkan halaman Katalog dengan Grid Template terbaru/unggulan.



Penyaringan (Filter \& Search): Pengguna mengetik di kolom pencarian ATAU mencentang filter (misal: Kategori "Pernikahan" + Tema "Rustic").



Pengurutan (Sorting): Pengguna memilih urutan "Terpopuler" dari dropdown. Sistem memuat ulang grid template sesuai kriteria.



Quick Preview \& Wishlist: Pengguna melihat thumbnail, mengklik ikon hati untuk menyimpannya ke Wishlist (membutuhkan login), atau menekan ikon mata (Quick Preview).



Detail Template (Live Preview): Pengguna mengklik Card template -> Diarahkan ke halaman Detail Template -> Pengguna menggeser layar mockup HP untuk melihat fitur detail.



Keputusan (Call to Action): Setelah yakin, pengguna mengklik tombol "Gunakan Template Ini" atau "Pesan Sekarang" -> Sistem mengarahkan pengguna ke Modul Pembayaran / Modul Dashboard Builder.



5\. Validasi Form \& Input



Untuk menjaga stabilitas sistem, validasi di sisi Klien (Frontend) dan Server (Backend) diberlakukan:



Search Input: Maksimal 100 karakter. Menolak karakter berbahaya (seperti <script>, =, ;) untuk mencegah injeksi XSS. Spasi berlebih akan di-trim.



Filter Parameter: Parameter URL filter (seperti ?category=wedding\&color=red) hanya menerima daftar nilai yang valid (Enums/UUID yang terdaftar). Jika parameter tidak valid, sistem mengabaikannya dan menggunakan nilai bawaan (fallback).



Pagination Input: Nomor halaman (page) dan batas item (limit) harus berupa angka bulat positif (integer). Jika input melebihi jumlah halaman maksimal, sistem akan mengembalikan halaman terakhir atau halaman kosong (tanpa error fatal).



Wishlist Action: ID Template yang dikirim untuk ditambahkan ke wishlist wajib divalidasi keberadaannya di database.



6\. Hak Akses (Role-Based Access Control)



Guest (Pengunjung Belum Login): Dapat melihat seluruh katalog, menggunakan filter, pencarian, melihat live preview, dan membagikan tautan. Tidak dapat menambahkan template ke wishlist (akan diarahkan ke halaman Login).



Customer \& Reseller: Memiliki semua hak Guest, ditambah kemampuan menambahkan/menghapus template dari wishlist pribadi mereka, serta memproses pesanan template.



Admin \& Super Admin (Sisi Publik): Memiliki tampilan dan hak yang sama dengan Customer saat mengakses antarmuka publik, namun memiliki akses tambahan di Modul Dashboard untuk membuat, mengedit, menyembunyikan, atau menghapus data template (di luar cakupan SRS antarmuka pengguna ini).



7\. Error Handling (Penanganan Kesalahan)



Sistem harus memberikan umpan balik visual yang informatif untuk setiap skenario kegagalan:



Kasus: Pencarian/Filter tidak menghasilkan data. Pesan/UI: Menampilkan ilustrasi grafis (misal: ikon kaca pembesar) dengan pesan, "Maaf, template dengan kriteria tersebut belum tersedia. Coba hapus beberapa filter atau ubah kata kunci Anda." Disertai tombol "Reset Filter".



Kasus: Template dinonaktifkan (Draft/Deleted) saat diakses via direct link. Pesan/UI: Diarahkan ke halaman 404 (Not Found) khusus dengan pesan, "Template yang Anda cari sudah tidak tersedia atau telah dihapus." Disertai tombol "Kembali ke Katalog".



Kasus: Gambar gagal dimuat (Jaringan putus/CDN Error). Solusi: Thumbnail otomatis menampilkan gambar placeholder bawaan dengan logo platform secara elegan.



Kasus: Server Timeout / Gagal mengambil data dari API. Pesan/UI: Muncul Toast Alert merah bertuliskan, "Gagal memuat katalog. Periksa koneksi internet Anda atau coba beberapa saat lagi." Disertai tombol Retry pada layar kosong.



8\. Security (Standar Keamanan)



Anti SQL Injection: Semua parameter filter dan pencarian wajib disanitasi oleh backend ORM (Object-Relational Mapping). Dilarang keras merakit kueri SQL secara manual dari parameter URL pengguna.



XSS Protection: Output dari pencarian yang ditampilkan kembali ke layar pengguna (misal: "Menampilkan hasil untuk: 



$$keyword$$



") harus di-encode agar tidak mengeksekusi script berbahaya.



Rate Limiting: Endpoint API pencarian (Search) wajib dibatasi (misal: maksimal 30 request per menit per IP) untuk mencegah scraping data massal atau serangan DDoS (Denial of Service).



Secure Asset Access (Hotlink Protection): Aset gambar premium template disajikan melalui CDN (seperti Cloudflare) yang dikonfigurasi untuk memblokir permintaan dari domain eksternal (CORS policy ketat) demi mencegah kompetitor mencuri bandwidth atau aset desain.



9\. Struktur Database yang Dibutuhkan (Entity-Relationship)



Templates (Tabel Utama): Menyimpan master data undangan. (Kolom esensial: id, nama, slug, deskripsi, harga, tipe 



$$gratis/premium$$



, status 



$$aktif/draft$$



, total\_views, waktu\_dibuat).



Categories: Menyimpan jenis acara. (Kolom: id, nama\_kategori 



$$Wedding, Birthday, dll$$



, slug). Relasi N:M atau 1:N ke Templates.



Themes: Menyimpan gaya desain. (Kolom: id, nama\_tema 



$$Minimalist, dll$$



).



Colors: Menyimpan palet warna untuk filter visual. (Kolom: id, nama\_warna, kode\_hex).



Template\_Images: Menyimpan galeri/thumbnail dari setiap template. (Kolom: id, template\_id, url\_gambar, is\_thumbnail). Relasi N:1 ke Templates.



Template\_Features: Menyimpan daftar fitur yang didukung template tersebut (misal: RSVP, QR Code). Relasi N:1 ke Templates.



Wishlists (atau Favorites): Menyimpan daftar keinginan pengguna. (Kolom: id, user\_id, template\_id). Relasi N:1 ke Users, N:1 ke Templates.



Reviews \& Ratings: Menyimpan ulasan pengguna terhadap suatu desain. (Kolom: id, template\_id, user\_id, rating 



$$1-5$$



, komentar).



10\. Daftar API yang Dibutuhkan (Endpoint Definition)



Untuk mendukung modul frontend, API Backend yang disiapkan adalah:



GET /api/v1/templates

Fungsi: Mengambil daftar semua template yang aktif. Menerima query parameter untuk paginasi (page, limit) dan sorting (sort\_by).



GET /api/v1/templates/filter

Fungsi: Mengambil data template berdasarkan kombinasi parameter (category, theme, color, price). Mengembalikan array data beserta total item.



GET /api/v1/templates/search

Fungsi: Endpoint khusus yang dioptimalkan (dengan indexing teks/Elasticsearch) untuk merespons kueri pencarian kata kunci (?q=keyword).



GET /api/v1/templates/metadata

Fungsi: Mengambil daftar master data kategori, tema, dan warna. Digunakan saat pertama kali memuat form Sidebar Filter di Frontend.



GET /api/v1/templates/{slug}

Fungsi: Mengambil detail lengkap sebuah template tertentu (termasuk fitur, harga, dan gambar galeri) berdasarkan slug URL-nya. Digunakan pada Halaman Detail Template.



GET /api/v1/templates/{slug}/related

Fungsi: Mengambil maksimal 4-6 template lain yang memiliki kategori/tema yang sama dengan template referensi (Rekomendasi).



POST /api/v1/templates/{slug}/view

Fungsi: Endpoint tanpa respons (fire-and-forget) untuk merekam aktivitas bahwa template ini baru saja dilihat. Menambah metrik total\_views secara asinkron.



GET /api/v1/wishlists

Fungsi: (Auth Required) Mengambil seluruh template yang disimpan oleh user yang sedang login.



POST /api/v1/wishlists

Fungsi: (Auth Required) Menambahkan template\_id tertentu ke daftar wishlist pengguna.



DELETE /api/v1/wishlists/{template\_id}

Fungsi: (Auth Required) Menghapus template dari daftar wishlist pengguna.



GET /api/v1/templates/{slug}/reviews

Fungsi: Mengambil daftar ulasan dan rata-rata rating (bintang) untuk halaman detail template.


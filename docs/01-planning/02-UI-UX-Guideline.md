UI/UX DESIGN MASTER GUIDELINE



Proyek: Platform SaaS \& E-Commerce Undangan Digital

Style: Modern, Premium, Minimalis, Clean (Inspirasi: Apple, Stripe, Linear, Framer)



1\. DESIGN SYSTEM \& FUNDAMENTALS



A. Color Palette



Pemilihan warna berfokus pada kesan organik, elegan, namun tetap memberikan nuansa tech-startup yang profesional.



Primary: Sage Green (#78866B) - Memberikan kesan menenangkan, organik, premium, dan sangat cocok dengan tema wedding.



Secondary: Soft Gold (#D4AF37) - Aksen untuk elemen premium, VIP, atau highlight harga.



Neutral (Background \& Surface):



Base BG: Off-White (#FAFAFB) - Menghindari putih murni agar mata tidak cepat lelah (Apple style).



Surface/Card: Pure White (#FFFFFF).



Neutral (Typography):



Heading: Charcoal Black (#18181B).



Body Text: Slate Gray (#52525B).



Semantic (Feedback): Success (#10B981), Error (#EF4444), Warning (#F59E0B).



B. Typography



Heading (Display \& Titles): Plus Jakarta Sans atau Clash Display. Tegas, geometris, modern. Sangat identik dengan vibe SaaS modern (Linear/Stripe).



Body (Paragraphs \& UI Text): Inter. Sangat legible di ukuran kecil, netral, dan profesional.



C. UI Components (Atomic Design)



Button: Corner radius 6px (sedikit membulat, tidak terlalu pill). Varian: Solid (Sage Green), Outline (Border tipis), Ghost (Tanpa background, hover effect abu-abu).



Card: Putih bersih, tanpa border tebal. Menggunakan soft drop-shadow yang sangat tipis (box-shadow: 0 4px 20px rgba(0,0,0,0.03)).



Input \& Form: Background putih, border abu-abu sangat terang (#E4E4E7), berubah menjadi outline Sage Green saat focus.



Badge/Tag: Background transparan dengan tint warna (misal: Sage Green transparan 10% dengan teks Sage Green pekat).



2\. PAGE DESIGNS \& USER EXPERIENCE



A. Landing Page



Tujuan Halaman: Mengedukasi pengunjung, membangun kepercayaan (trust), dan mengonversi (checkout atau registrasi).



Struktur Layout:



Navbar: Logo (Kiri), Links \& Kategori (Tengah), Auth \& Keranjang (Kanan). Berubah sticky dengan efek glassmorphism saat di-scroll.



Hero Section: Headline besar tebal, Sub-headline deskriptif, 2 CTA Button (Primary: "Eksplorasi Template", Secondary: "Cara Kerja"), visual mockup iPhone melayang menampilkan undangan.



Keunggulan (Bento Grid): Layout ala Apple/Framer dengan kotak-kotak asimetris menampilkan ikon dan teks singkat (Kecepatan, RSVP otomatis, dll).



Cara Pemesanan: Timeline vertikal minimalis (Pilih -> Bayar -> Edit -> Sebar).



Template Terbaru \& Terlaris: Slider/Carousel kartu template dengan hover state yang memutar auto-scroll preview.



Harga Paket: 3 Kartu (Basic, Pro, Reseller). Kartu Pro sedikit lebih besar/di- highlight dengan badge Gold.



Testimoni: Layout masonry atau scrolling marquee.



FAQ \& Footer: Akordion minimalis, footer besar dengan multi-kolom navigasi.



Komponen: Bento Grid, Carousel, Pricing Card, Accordion, Sticky Navbar.



Alur Pengguna: Landing -> Terpukau oleh Hero -> Percaya karena Keunggulan \& Testimoni -> Tertarik melihat Harga -> Klik eksplor template.



Alasan Desain: Menggunakan Bento Grid dan banyak white space (Apple style) memecah informasi kompleks menjadi potongan visual yang mudah dicerna.



UX Improvement: Gunakan lazy loading untuk gambar. Animasi fade-up ringan saat scroll (Framer motion style) agar website terasa hidup.



Responsive Behaviour: Desktop (Grid lebar), Tablet (Grid 2 kolom), Mobile (Stack vertikal vertikal, Navbar menjadi Hamburger Menu).



B. Halaman Katalog Template



Tujuan Halaman: Discovery. Membantu user menemukan template yang pas dengan filter yang sangat spesifik.



Struktur Layout:



Top Bar: Search Bar besar bergaya Notion (Shortcut Cmd+K atau klik).



Left Sidebar (Filter): Kategori (Wedding, Birthday), Warna (Color swatches), Harga, Fitur.



Main Content: Sorting dropdown, Grid kartu template (3-4 kolom).



Card Template: Gambar thumbnail, Judul, Harga, Ikon Wishlist (Hati), Tombol Quick Preview muncul saat di-hover.



Pagination: Clean number pagination di bawah.



Komponen: Checkbox list, Color picker circle, Image Card, Hover Overlay, Dropdown.



Alur Pengguna: Buka Katalog -> Ketik di Search atau klik Filter Warna -> Lihat Grid -> Hover kartu -> Klik untuk masuk ke Detail.



Alasan Desain: Mengisolasi filter di kiri menjaga grid katalog tetap rapi. Filter warna visual (lingkaran warna) jauh lebih baik daripada teks.



UX Improvement: Skeleton Loading saat memuat halaman atau ganti filter. Tidak ada page reload saat filter diklik (seamless AJAX/React state).



Responsive Behaviour: Mobile: Filter disembunyikan dalam Bottom Sheet atau Modal yang muncul saat klik tombol "Filter". Grid menjadi 1 atau 2 kolom.



C. Halaman Detail Template



Tujuan Halaman: Memberikan simulasi pengalaman produk sesungguhnya dan mendorong tombol "Beli".



Struktur Layout:



Kiri (Sticky, 50%): Frame mockup smartphone (iPhone) interaktif. Berisi Live Preview undangan yang bisa di-scroll. Terdapat tombol toggle "Mobile / Desktop View" di atas frame.



Kanan (Scrollable, 50%):



Breadcrumb, Judul, Badge Kategori.



Harga \& CTA "Pesan Sekarang" (Sticky di bawah pada mobile).



Tabs: Deskripsi, Fitur (Icon list), Galeri (Thumbnail kecil), Review (Star rating).



Rekomendasi "Mungkin Anda Suka" (Grid 2 kolom).



Komponen: Iframe mobile mockup, Tabs, Icon list, Sticky action bar.



Alur Pengguna: Masuk halaman -> Scroll preview interaktif di frame HP -> Baca fitur -> Klik "Pesan Sekarang".



Alasan Desain: Pendekatan split-screen memastikan preview undangan selalu terlihat saat user membaca deskripsi atau review.



UX Improvement: Tombol "Pesan Sekarang" menggunakan mikro-animasi pulse ringan. Frame iframe menggunakan lazy load agar tidak membebani initial page load.



Responsive Behaviour: Mobile: Layout berubah vertikal. Live preview menjadi kotak biasa (bukan sticky), tombol "Pesan Sekarang" menjadi sticky bottom bar yang menempel di bawah layar ponsel.



D. Halaman Checkout



Tujuan Halaman: Menyelesaikan transaksi dengan minim friksi (Frictionless checkout ala Stripe).



Struktur Layout:



Clean Header: Hanya logo (tanpa navigasi, agar user fokus bayar).



Kiri (Form): Stepper ringkas (1. Akun/Login, 2. Detail Pesanan, 3. Pembayaran). Form input alamat/data awal.



Kanan (Order Summary): Sticky box berisi gambar template kecil, rincian biaya, input kode Voucher dengan tombol "Terapkan", Total Bayar.



Komponen: Stepper, Input Form dengan validasi real-time, Radio button (Pilih Bank/E-Wallet), Toast notification (Voucher berhasil).



Alur Pengguna: Login/Guest -> Konfirmasi rincian -> Input voucher -> Pilih Bank -> Klik "Bayar Sekarang" -> Redirect ke Payment Gateway.



Alasan Desain: Menghilangkan navigasi (Header/Footer) meniru konsep Stripe checkout, mencegah user terdistraksi atau kabur dari halaman.



UX Improvement: Validasi input on-blur (langsung merah jika salah format tanpa menunggu tombol submit). Angka total bayar otomatis update saat voucher diinput.



Responsive Behaviour: Mobile: Order Summary disembunyikan dalam accordion "Lihat Rincian Pesanan", form pembayaran mengambil layar penuh.



E. Dashboard Customer (B2C)



Tujuan Halaman: Ruang kerja pengguna untuk mengedit, memantau, dan mengatur undangannya sendiri.



Struktur Layout:



Sidebar (Kiri): Profil ringkas, Navigasi bergaya Linear (Dashboard, Edit Undangan, Tamu \& RSVP, Amplop \& Gift, Pengaturan), Indikator Masa Aktif.



Main Area (Dashboard Default):



Greeting "Halo, Budi".



Stat Card: 3 kotak metrik (Total Tamu, RSVP Hadir, Uang Masuk).



Quick Actions: Tombol besar (Edit, Salin Link, QR Code).



Recent Activity: Tabel kecil berisi 5 RSVP terakhir.



Main Area (Edit Undangan / Builder Mode):



Layout Split: Kiri Form Editor (Accordion: Data Mempelai, Acara, Galeri, dll), Kanan Live Preview Mobile.



Komponen: Sidebar collapsible, Stat Cards, Data Table, Form Accordion, Drag-and-drop zone (untuk upload galeri).



Alur Pengguna: Login -> Lihat Metrik -> Klik Edit Undangan -> Buka Accordion "Galeri" -> Upload foto -> Lihat preview di kanan langsung berubah -> Publish.



Alasan Desain: Memisahkan mode view stats dengan mode builder. Saat builder, split-screen sangat esensial agar user tidak lelah bolak-balik klik tombol "Preview".



UX Improvement: Auto-save system dengan indikator "Menyimpan..." di pojok atas (ala Google Docs). Area upload mendukung drag-and-drop file ganda.



Responsive Behaviour: Tablet: Sidebar mengecil menjadi ikon saja. Mobile: Mode Builder menyembunyikan live preview, user harus klik Floating Action Button berlogo "Mata" untuk melihat preview full screen.



F. Dashboard Admin



Tujuan Halaman: Manajemen operasional konten, template, dan keluhan pelanggan.



Struktur Layout:



Sidebar: Dashboard, Kelola Template, Pesanan (Status: Pending/Sukses), Manajemen Customer, Voucher, Blog/Artikel, Laporan.



Main Area (Manajemen Template): Tabel data dengan fitur Search dan Filter canggih. Kolom: Thumbnail, Nama, Kategori, Harga, Status (Aktif/Draft), Aksi (Edit/Hapus).



Komponen: Data Table canggih dengan bulk actions (ceklis banyak row), Modal Form, Status Badge (Hijau/Kuning/Merah).



Alur Pengguna: Buka halaman Pesanan -> Filter status "Pending" -> Verifikasi manual atau cek detail -> Ubah status.



Alasan Desain: Fungsionalitas di atas segalanya. Whitespace sedikit dikurangi dibanding UI Customer agar layar bisa memuat lebih banyak baris data (Data-dense layout).



UX Improvement: Fitur Copy to clipboard pada ID Pesanan. Inline editing pada tabel untuk mengubah status dengan cepat tanpa harus membuka halaman baru.



Responsive Behaviour: Tabel dapat di-scroll secara horizontal (overflow-x) pada layar kecil. Sidebar masuk ke dalam off-canvas/drawer.



G. Dashboard Super Admin



Tujuan Halaman: Pemantauan tingkat tinggi, analitik finansial global, dan kontrol konfigurasi sistem SaaS.



Struktur Layout:



Sidebar: Overview Analytics, Semua Tenant/Reseller, Semua Admin, Role \& Permissions, System Logs, Maintenance.



Main Area (Overview):



Grafik garis (Chart) besar menunjukkan Monthly Recurring Revenue (MRR) atau Tren Penjualan.



Tabel Server Health / Log Aktivitas terbaru.



Komponen: Area Line Chart, Bar Chart, Data Table dengan export (CSV/PDF), Toggle Switches (untuk Maintenance mode).



Alur Pengguna: Login -> Cek grafik pendapatan -> Cek Log aktivitas mencurigakan -> Tambah hak akses Admin baru.



Alasan Desain: Menggunakan pendekatan Dashboard Analytics (seperti Datadog/Google Analytics). Fokus pada visualisasi data yang langsung menangkap mata.



UX Improvement: Gunakan tooltip pada grafik (hover di atas garis akan memunculkan nilai spesifik). Peringatan berwarna merah tegas jika backup gagal atau ada isu keamanan.



Responsive Behaviour: Grafik bersifat responsif (resize sesuai container). Namun disarankan pengaksesan via Desktop untuk analisa data.



3\. GLOBAL RESPONSIVE BEHAVIOUR \& INTERACTION



Mobile First vs Desktop: Bagian end-product (Undangan publik) harus didesain Mobile-First (95% pengunjung mengakses dari HP). Namun, bagian Editor (Dashboard Customer) dan Dashboard Admin didesain Desktop-First karena merancang/memasukkan data lebih nyaman di layar besar.



Navigation on Mobile:



Navbar Desktop diubah menjadi Bottom Navigation Bar (seperti aplikasi Instagram) untuk Dashboard Customer di Mobile, memberikan rasa Native App.



Modals vs Drawers: Pada layar Desktop gunakan Modal/Pop-up di tengah layar. Pada layar Mobile, ubah Modal tersebut menjadi Bottom Sheet/Drawer yang ditarik dari bawah (lebih ergonomis untuk ibu jari).



Loading States: Jangan gunakan layar putih kosong saat loading. Gunakan Skeleton Loaders (kotak abu-abu berkedip halus menyerupai bentuk konten) agar website terasa sangat responsif dan cepat, mengurangi rasio bounce.



Micro-interactions (Feedback): Setiap aksi (sukses menyimpan, gagal bayar, URL disalin) WAJIB memunculkan Toast Notification kecil di pojok kanan bawah yang hilang sendiri dalam 3 detik, agar user tidak bingung apakah klik mereka berhasil.


SOFTWARE REQUIREMENTS SPECIFICATION (SRS)



Proyek: Platform SaaS \& E-Commerce Undangan Digital

Modul: Checkout \& Pembayaran

Status: Disetujui untuk Pengembangan



1\. Tujuan Modul



Modul Checkout \& Pembayaran berfungsi sebagai sistem pemrosesan transaksi finansial pada platform SaaS ini. Tujuannya adalah untuk memfasilitasi pengguna (Customer/Reseller) dalam menyelesaikan pembelian template atau paket langganan secara aman, cepat, dan otomatis. Modul ini terintegrasi penuh dengan Payment Gateway (seperti Midtrans atau Xendit) untuk mendukung berbagai metode pembayaran, mencatat riwayat transaksi, menerbitkan invoice, dan mengaktifkan produk secara real-time tanpa intervensi manual.



2\. Functional Requirements (Kebutuhan Fungsional)



Sistem harus mampu menjalankan fungsi-fungsi berikut:



FR-01 (Ringkasan Pesanan / Order Summary): Sistem harus menampilkan rincian pesanan dengan jelas, meliputi nama template/paket, harga dasar, pajak (jika ada), potongan harga, dan total akhir yang harus dibayar.



FR-02 (Manajemen Voucher \& Promo): Sistem memungkinkan pengguna memasukkan kode voucher/kupon. Sistem akan memvalidasi kupon dan menghitung ulang total pesanan secara otomatis (diskon persentase atau nominal tetap).



FR-03 (Pemilihan Metode Pembayaran): Sistem harus menampilkan daftar metode pembayaran yang aktif dan didukung melalui integrasi Payment Gateway, termasuk:



Virtual Account (BCA, Mandiri, BNI, BRI, dll)



QRIS



E-Wallet (GoPay, OVO, ShopeePay, Dana)



Kartu Kredit / Debit



FR-04 (Proses Checkout \& Generate Invoice): Saat pengguna menekan "Bayar", sistem akan membuat Order ID unik, mengunci harga, membuat tagihan (Invoice), dan mengarahkan pengguna ke halaman/UI Payment Gateway (Snap URL atau Checkout URL).



FR-05 (Webhook \& Sinkronisasi Status): Sistem backend harus memiliki endpoint webhook yang secara pasif mendengarkan pembaruan status dari Payment Gateway secara real-time (Status: Pending, Success/Settlement, Expired, Failed).



FR-06 (Aktivasi Otomatis): Segera setelah status pembayaran menjadi Success, sistem otomatis mengaktifkan template/paket ke akun pengguna tanpa jeda.



FR-07 (Notifikasi Multikanal): Sistem memicu pengiriman notifikasi via Email dan WhatsApp (via API pihak ketiga) pada 3 fase transaksi:



Invoice Created (Instruksi pembayaran).



Payment Success (Bukti lunas \& link akses).



Payment Expired (Peringatan batal).



FR-08 (Retry Payment / Ubah Metode): Jika pembayaran Pending atau gagal di tengah proses, pengguna dapat membatalkan pesanan tersebut atau memilih opsi "Ubah Metode Pembayaran" yang akan membuat Payment Request baru untuk Order yang sama.



FR-09 (Riwayat Transaksi): Sistem menyediakan antarmuka bagi pengguna untuk melihat seluruh riwayat pembelian, mengunduh Invoice berformat PDF, dan melihat status transaksi.



FR-10 (Refund \& Pembatalan): Sistem menyediakan fitur bagi Admin untuk melakukan Refund (pengembalian dana) baik secara manual maupun parsial melalui API Payment Gateway (jika didukung), serta mencatat log pembatalan pesanan.



3\. Non-Functional Requirements (Kebutuhan Non-Fungsional)



Performance: Perhitungan total harga (termasuk validasi diskon) di backend harus selesai di bawah 500ms. Redirect ke Payment Gateway tidak boleh melebihi 2 detik.



Security \& PCI Compliance: Sistem tidak menyimpan data sensitif Kartu Kredit di basis data lokal. Seluruh pemrosesan instrumen pembayaran didelegasikan ke Payment Gateway yang sudah tersertifikasi PCI-DSS. Transaksi wajib menggunakan enkripsi TLS 1.2 atau lebih tinggi.



Availability \& Reliability: Harus memiliki ketersediaan 99.9%. Jika Payment Gateway utama (misal: Midtrans) mengalami downtime, sistem secara otomatis memberikan pesan kesalahan yang anggun (graceful degradation) atau beralih ke gateway cadangan (jika dikonfigurasi).



Scalability: Mampu menangani ribuan permintaan checkout serentak saat periode Flash Sale atau promo tanggal kembar tanpa race condition (berebut stok/ID).



4\. User Flow (Alur Pengguna)



Inisiasi: Pengguna mengklik "Pesan Sekarang" dari halaman Detail Template. (Sistem mengecek apakah user sudah login. Jika belum, redirect ke Auth).



Checkout Page: Pengguna diarahkan ke halaman Checkout. Sistem menampilkan Order Summary.



Input Voucher (Opsional): Pengguna mengetik kupon -> Klik "Terapkan" -> Sistem memvalidasi dan memotong harga.



Checkout Action: Pengguna mengklik "Lanjutkan Pembayaran".



Payment Gateway Integration: Sistem merender UI/Modal Payment Gateway (misal: Midtrans Snap). Pengguna memilih metode (contoh: QRIS) dan mendapatkan instruksi bayar.



Status Pending: Pengguna melihat halaman "Menunggu Pembayaran" dengan Countdown Timer batas waktu bayar (misal: 24 jam).



Proses Pembayaran (Eksternal): Pengguna scan QRIS via aplikasi m-Banking mereka.



Webhook Trigger: M-banking sukses -> Payment Gateway mengirim POST Webhook ke Backend SaaS -> Backend memvalidasi Signature -> Mengubah status Order menjadi Paid -> Memberi akses produk.



Konfirmasi \& Redirect: Layar pengguna (Frontend) yang sedang melakukan polling otomatis berubah ke halaman "Pembayaran Berhasil!". Sistem mengirim Invoice ke Email \& WA.



Selesai: Pengguna diarahkan ke Dashboard Customer untuk mulai mengedit template.



5\. Validasi Sistem



Validasi Voucher: Mengecek eksistensi kupon, kuota penggunaan (limit), tanggal berlaku (expired date), syarat minimum transaksi, dan apakah kupon berlaku untuk produk yang sedang di-checkout.



Validasi Nominal (Server-Side Calculation): Frontend dilarang keras mengirimkan nominal Total Bayar ke API Backend untuk diproses. Backend harus menghitung ulang seluruh harga dari database berdasarkan template\_id dan voucher\_code untuk mencegah eksploitasi manipulasi harga dari browser inspector / Postman.



Validasi Idempotency: Mencegah satu Order ID dibayar dua kali. Jika status Order sudah PAID, semua request pembayaran baru untuk Order tersebut akan ditolak.



Validasi Ketersediaan Metode: Memastikan metode pembayaran yang dipilih belum dinonaktifkan oleh Admin/Payment Gateway.



6\. Hak Akses (Role-Based Access Control)



Guest: Tidak memiliki akses. Akan di-redirect ke halaman Login/Register saat mencoba checkout.



Customer: Dapat membuat pesanan, menerapkan voucher publik, membayar, membatalkan pesanan pending, dan melihat riwayat invoice miliknya sendiri.



Reseller: Memiliki hak yang sama dengan Customer, ditambah akses ke voucher/harga khusus (tier B2B) dan pembelian paket kredit (bulk).



Admin: Dapat melihat semua data transaksi, melihat bukti bayar manual (jika ada), mengunduh laporan pesanan, dan melakukan pembatalan pesanan pelanggan jika ada masalah teknis.



Super Admin: Memiliki akses ke metrik pendapatan (Revenue Dashboard), mengatur kunci rahasia API (API Keys) Payment Gateway, dan mengatur pajak/biaya admin platform.



7\. Error Handling (Penanganan Kesalahan)



Sistem merespons kegagalan dengan pesan yang ramah pengguna (UI) dan log yang detail di server:



Kasus: Voucher tidak valid/kadaluarsa. Pesan UI: "Maaf, kode voucher ini tidak ditemukan atau sudah melewati batas waktu penggunaan."



Kasus: API Payment Gateway Timeout/Down. Pesan UI: "Sistem pembayaran sedang mengalami gangguan dari penyedia layanan. Mohon coba beberapa saat lagi."



Kasus: Pembayaran Kadaluarsa (Expired). Pesan UI: "Waktu pembayaran telah habis. Silakan buat pesanan baru untuk melanjutkan." (Status order di DB otomatis menjadi Expired).



Kasus: Pembayaran Gagal (Kartu Ditolak/Saldo Kurang). Pesan UI: "Pembayaran Anda ditolak oleh Bank/Penyedia E-Wallet. Silakan gunakan metode pembayaran lain." Terdapat tombol "Ubah Metode Pembayaran".



8\. Security (Standar Keamanan Transaksi)



Webhook Signature Verification: Setiap request ke endpoint webhook wajib divalidasi menggunakan HMAC (SHA512/SHA256) mencocokkan Signature Key dari Payment Gateway. Request tanpa signature yang valid akan ditolak (HTTP 401/403).



Anti-Fraud \& Replay Attack: Validasi status pembayaran terakhir. Jika webhook mengirim status settlement untuk Order ID yang sudah settlement, sistem harus mengabaikannya (Idempotent operation) untuk mencegah pemberian limit ganda atau aktivitas looping.



CSRF Protection: Form checkout di frontend diamankan menggunakan CSRF Token untuk menghindari pengiriman pesanan sepihak oleh skrip peretas.



Data Sanitization: Input Order Notes (jika ada) dan Voucher Code dibersihkan (sanitized) dari injeksi SQL/XSS sebelum diproses.



9\. Struktur Database yang Dibutuhkan (Entity-Relationship)



Orders (Tabel Utama Transaksi): Menyimpan header transaksi. (Kolom: id \[UUID/Order\_No], user\_id, subtotal, tax\_amount, discount\_amount, grand\_total, status \[Pending, Paid, Failed, Expired], created\_at, expired\_at).



Order\_Items: Rincian barang yang dibeli (mengantisipasi cart massal di masa depan). (Kolom: id, order\_id, item\_type \[Template/Package], item\_id, price, quantity).



Payments: Rekaman interaksi spesifik dengan Payment Gateway. (Kolom: id, order\_id, pg\_transaction\_id, payment\_method \[qris, bca\_va, dll], amount, pg\_status, paid\_at). Relasi N:1 ke Orders.



Payment\_Methods: Master data metode bayar untuk ditampilkan di UI. (Kolom: id, name, code, logo\_url, is\_active, admin\_fee).



Invoices: Data tagihan final untuk keperluan cetak/PDF. (Kolom: id, order\_id, invoice\_number, pdf\_url, issued\_date).



Coupons / Vouchers: Master data promo. (Kolom: id, code, type \[fixed/percent], value, min\_purchase, max\_discount, usage\_limit, used\_count, valid\_from, valid\_until, is\_active).



Payment\_Logs (Transaksi/Audit): Log table (No-SQL/JSON column) menyimpan raw payload dari webhook Payment Gateway untuk keperluan investigasi bug atau sengketa (dispute).



Refunds: Mencatat proses pengembalian dana. (Kolom: id, order\_id, amount, reason, status, processed\_by\_admin\_id).



10\. Daftar API yang Dibutuhkan (Endpoint Definition)



Untuk mendukung modul Checkout Frontend (Next.js), Backend (Laravel) menyediakan API RESTful berikut:



POST /api/v1/checkout/calculate

Fungsi: Endpoint simulasi (Draft). Menerima payload template\_id dan opsional voucher\_code. Mengembalikan rincian perhitungan harga (Subtotal, Diskon, Total Akhir) tanpa menyimpan ke database.



POST /api/v1/checkout

Fungsi: (Auth Required) Endpoint utama eksekusi. Menerima template\_id dan voucher\_code. Backend melakukan kalkulasi harga, menyimpan ke tabel Orders, memanggil API Payment Gateway (Snap API), dan mengembalikan Token/URL pembayaran ke Frontend.



POST /api/v1/payments/webhook

Fungsi: Endpoint publik (tanpa token user, dilindungi Signature) untuk menerima callback/notification asinkron dari Midtrans/Xendit (contoh: status bayar berubah menjadi lunas).



GET /api/v1/orders/{order\_id}/status

Fungsi: (Auth Required) Digunakan oleh Frontend untuk melakukan polling ringan mengecek apakah pembayaran telah berhasil di latar belakang.



POST /api/v1/orders/{order\_id}/cancel

Fungsi: (Auth Required) Membatalkan pesanan yang masih berstatus Pending. Menghubungi PG untuk membatalkan Virtual Account/URL yang aktif.



GET /api/v1/orders/history

Fungsi: (Auth Required) Mengambil daftar riwayat pesanan (Pagination) untuk ditampilkan di Dashboard Customer/Reseller.



GET /api/v1/invoices/{order\_id}

Fungsi: (Auth Required) Mengambil detail lengkap pesanan atau men-generate link unduhan PDF Invoice.



POST /api/v1/admin/refunds

Fungsi: (Admin Required) Mengeksekusi pengembalian dana parsial atau penuh atas suatu order\_id bermasalah. Terintegrasi dengan Refund API Payment Gateway.


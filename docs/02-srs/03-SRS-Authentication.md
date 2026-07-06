SOFTWARE REQUIREMENTS SPECIFICATION (SRS)



Proyek: Platform SaaS \& E-Commerce Undangan Digital

Modul: Authentication (Otentikasi \& Otorisasi Pengguna)

Status: Disetujui untuk Pengembangan



1\. Tujuan Modul



Modul Authentication berfungsi sebagai gerbang utama keamanan platform SaaS. Tujuannya adalah untuk mengelola identitas pengguna, memverifikasi kredensial, mengatur sesi (session), serta memberikan hak akses (otorisasi) yang tepat berdasarkan peran pengguna (Super Admin, Admin, Reseller, atau Customer). Modul ini harus memastikan pengalaman masuk yang mulus (frictionless) tanpa mengorbankan keamanan data tingkat tinggi.



2\. Functional Requirements (Kebutuhan Fungsional)



Sistem harus mampu melakukan fungsi-fungsi berikut:



FR-01 (Registrasi Akun): Sistem memungkinkan pengguna baru (Customer/Reseller) mendaftar menggunakan Email dan Password.



FR-02 (Verifikasi Email): Sistem harus mengirimkan tautan atau OTP (One Time Password) ke email pendaftar untuk memverifikasi kepemilikan email sebelum akun aktif sepenuhnya.



FR-03 (Login Kredensial): Sistem memungkinkan pengguna masuk menggunakan kombinasi Email dan Password yang telah terdaftar dan terverifikasi.



FR-04 (Social Login / SSO): Sistem menyediakan opsi pendaftaran dan login menggunakan akun Google (Google OAuth 2.0).



FR-05 (Lupa Password): Sistem menyediakan alur pemulihan kata sandi melalui pengiriman tautan token aman ke email terdaftar.



FR-06 (Reset Password): Sistem memungkinkan pengguna mengatur ulang kata sandi mereka menggunakan token pemulihan yang valid.



FR-07 (Logout): Sistem memungkinkan pengguna untuk mengakhiri sesi dan mencabut (revoke) token akses mereka secara aman dari semua perangkat.



3\. Non-Functional Requirements (Kebutuhan Non-Fungsional)



Performance: Waktu respons dari klik tombol "Login" hingga dasbor dimuat tidak boleh melebihi 2 detik pada koneksi internet standar (3G/4G).



Availability: Modul harus memiliki uptime 99.9%, karena downtime pada otentikasi berarti seluruh sistem SaaS tidak dapat digunakan.



Usability: Antarmuka form login/register harus mendukung navigasi keyboard (tombol Tab untuk pindah kolom, Enter untuk submit).



Scalability: Harus mampu menangani concurrent login (login bersamaan) dalam jumlah besar, terutama pada akhir pekan ketika trafik wedding memuncak.



4\. User Flow (Alur Pengguna)



Alur Registrasi: Pengunjung klik "Daftar" -> Mengisi Nama, Email, Password -> Submit -> Sistem mengirim Email Verifikasi -> Pengguna klik link di email -> Akun aktif -> Diarahkan ke Dashboard.



Alur Login: Pengunjung klik "Masuk" -> Mengisi Email \& Password -> Submit -> Sistem memvalidasi kredensial -> Sukses -> Sistem menerbitkan Access Token -> Diarahkan ke Dashboard (sesuai Role).



Alur Lupa Password: Di halaman Login, klik "Lupa Password" -> Input Email -> Submit -> Buka Inbox Email \& klik Link Reset -> Diarahkan ke halaman Buat Password Baru -> Submit -> Login ulang dengan password baru.



5\. Validasi Form



Untuk mencegah data kotor dan injeksi, validasi berikut diterapkan di sisi Client (Frontend) dan Server (Backend):



Nama Lengkap: Wajib diisi. Minimal 3 karakter, maksimal 50 karakter. Hanya menerima huruf dan spasi.



Email: Wajib diisi. Harus sesuai dengan format standar alamat email (mengandung @ dan domain yang valid).



Password Baru: Wajib diisi. Minimal 8 karakter. Harus mengandung kombinasi: minimal 1 huruf besar, 1 huruf kecil, dan 1 angka.



Konfirmasi Password: Wajib diisi. Harus 100% identik dengan kolom Password Baru.



6\. Hak Akses (Role-Based Access Control)



Modul otentikasi akan menyematkan Role ID pada token pengguna yang berhasil login.



Super Admin: Memiliki akses ke dasbor analitik global, manajemen modul sistem, dan manajemen pengguna/admin.



Admin: Memiliki akses ke manajemen template, validasi pembayaran, dan moderasi konten.



Reseller: Memiliki akses ke dasbor khusus untuk manajemen klien (pembuatan undangan massal) dan pengaturan white-label/custom domain.



Customer: Memiliki akses ke dasbor standar untuk mengedit undangan milik mereka sendiri dan melihat statistik RSVP.



Tamu (Unauthenticated): Hanya memiliki hak baca (read-only) pada URL publik undangan dan hak tulis pada form RSVP/Buku Tamu. Tidak bisa mengakses API Dashboard.



7\. Error Handling (Penanganan Kesalahan)



Pesan error yang ditampilkan kepada pengguna harus jelas namun tidak membahayakan keamanan (tidak verbose).



Kasus: Email sudah terdaftar. Pesan: "Email ini sudah terdaftar. Silakan gunakan email lain atau masuk ke akun Anda."



Kasus: Kombinasi Email/Password salah. Pesan: "Email atau Password yang Anda masukkan salah." (Penting: Jangan beri tahu secara spesifik apakah emailnya yang salah atau passwordnya yang salah untuk mencegah enumerasi email).



Kasus: Token Reset Password kedaluwarsa. Pesan: "Tautan pemulihan kata sandi sudah tidak berlaku. Silakan minta tautan baru."



Kasus: Terlalu banyak percobaan gagal. Pesan: "Terlalu banyak percobaan masuk yang gagal. Akun Anda dikunci sementara selama 15 menit."



8\. Security (Standar Keamanan)



Hashing: Kata sandi tidak boleh disimpan dalam bentuk teks biasa (plaintext). Wajib di-hash menggunakan algoritma kuat seperti Bcrypt atau Argon2 dengan salt yang di-generate secara acak.



Token Management: Menggunakan standar JWT (JSON Web Token).



Access Token berumur pendek (misal: 15-30 menit) disimpan di memori/state Frontend.



Refresh Token berumur panjang disimpan secara aman dalam bentuk HttpOnly, Secure Cookie untuk mencegah eksploitasi serangan XSS (Cross-Site Scripting).



Brute-Force Protection: Implementasi Rate Limiting pada endpoint login (misal: maksimal 5 kegagalan per IP dalam 15 menit).



Bot Protection: Implementasi reCAPTCHA v3 (mode invisible) pada form registrasi dan login untuk mencegah serangan spam/bot.



9\. Struktur Database yang Dibutuhkan



Tabel Users:

Menyimpan data master pengguna. (Kolom esensial: ID unik, nama lengkap, email, password\_hash, role\_id, is\_email\_verified, google\_id 



$$untuk OAuth$$



, waktu\_dibuat, waktu\_diupdate).



Tabel Roles:

Menyimpan daftar hak akses. (Kolom esensial: ID role, nama\_role 



$$Super Admin, Customer, dll$$



).



Tabel Password\_Resets:

Menyimpan token sementara untuk lupa password. (Kolom esensial: email, token\_hash, waktu\_kedaluwarsa).



Tabel Refresh\_Tokens (Opsional untuk keamanan ketat):

Menyimpan whitelist sesi perangkat yang sedang aktif, memungkinkan fitur "Logout dari semua perangkat".



10\. Daftar API yang Dibutuhkan (Endpoint Definition)



POST /api/auth/register : Mengirim payload pendaftaran, memicu pengiriman email verifikasi.



POST /api/auth/login : Menerima kredensial email/password, mengembalikan Access Token dan men-set HttpOnly Cookie untuk Refresh Token.



POST /api/auth/google : Menerima token dari Google OAuth, mengecek apakah email sudah ada, jika belum otomatis dibuatkan akun, mengembalikan sesi login.



POST /api/auth/verify-email : Menerima token verifikasi dari URL email untuk mengaktifkan akun.



POST /api/auth/forgot-password : Menerima input email, memicu pengiriman tautan reset ke email tersebut.



POST /api/auth/reset-password : Menerima token dari URL dan password baru, mengupdate password\_hash di database.



POST /api/auth/logout : Mencabut sesi token dan membersihkan HttpOnly Cookie di sisi klien.



POST /api/auth/refresh : Memvalidasi Refresh Token di cookie dan menerbitkan Access Token baru jika sesi belum kedaluwarsa.


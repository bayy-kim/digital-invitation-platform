SOFTWARE ARCHITECTURE DOCUMENT (SAD)



Proyek: Platform SaaS \& E-Commerce Undangan Digital



Status: Baseline Arsitektur untuk Pengembangan



Target Pemakai: Software Architect, Tech Lead, DevOps, Backend Developer, Frontend Developer



1\. Arsitektur Sistem



High-Level Architecture (HLA)



Platform ini mengadopsi pola arsitektur API-First (Decoupled Frontend and Backend). Pemisahan ini memisahkan kekhawatiran (separation of concerns) antara manajemen sistem internal (SaaS Dashboard) dengan mesin perender undangan publik (Guest Invitation) yang membutuhkan performa muat ekstra cepat.



&#x20;                              +----------------------------+

&#x20;                              |      Cloudflare Edge       |

&#x20;                              | (CDN, WAF, SSL, caching)   |

&#x20;                              +----------------------------+

&#x20;                                     /              \\

&#x20;                                    /                \\

&#x20;                                   v                  v

&#x20;                  +------------------------+  +--------------------------+

&#x20;                  |   Frontend (Next.js)   |  |   Render Engine (Next.js)|

&#x20;                  |   Dashboard \& Store    |  |    Public Invitation     |

&#x20;                  +------------------------+  +--------------------------+

&#x20;                                   \\                  /

&#x20;                                    \\                /

&#x20;                                     v              v

&#x20;                             +------------------------------+

&#x20;                             |   API Gateway / Reverse Proxy|

&#x20;                             |           (Nginx)            |

&#x20;                             +------------------------------+

&#x20;                                            |

&#x20;                                            v

&#x20;                             +------------------------------+

&#x20;                             |    Backend API (Laravel)     |

&#x20;                             +------------------------------+

&#x20;                                 /          |             \\

&#x20;                                /           |              \\

&#x20;                               v            v               v

&#x20;               +-------------------+ +-------------+ +---------------+

&#x20;               | PostgreSQL (Data) | | Redis (Cache| |  Supervisor    |

&#x20;               |                   | |  \& Session) | | (Queue Worker)|

&#x20;               +-------------------+ +-------------+ +---------------+





Penjelasan Komponen Utama:



Frontend (Next.js App Router): Melayani Landing Page, Katalog Template, Proses Checkout, dan Dashboard Customer/Admin. Menggunakan kombinasi Server Components (RSC) untuk optimasi SEO pada Storefront, dan Client Components untuk keaktifan Dasbor.



Render Engine (Next.js Static/ISR Engine): Sub-aplikasi yang diisolasi khusus untuk merender halaman undangan publik (/pernikahan/\[slug]). Dibuat seringkas mungkin untuk mengejar performa mematikan (minimal bundle size).



Backend Headless (Laravel): Bertindak sebagai core transaction engine dan REST API provider. Menangani autentikasi, manajemen data, webhooks payment gateway, integrasi WhatsApp/Email, dan penjadwalan (scheduling).



Database (PostgreSQL): Penyimpanan relasional utama dengan performa tinggi, mendukung kueri kompleks untuk dasbor admin dan transaksi keuangan, serta optimalisasi tipe data JSONB untuk draft payload builder.



Storage (Object Storage S3/Cloudinary): Semua berkas media (foto, audio, PDF invoice) diletakkan langsung di Object Storage, mengeliminasi beban penyimpanan pada disk lokal server aplikasi.



CDN (Cloudflare): Menyediakan enkripsi SSL/TLS, proteksi DDoS, Web Application Firewall (WAF), caching static assets, serta manajemen DNS otomatis untuk pemetaan Custom Domain.



Cache (Redis): Menyimpan data sesi autentikasi, mem-bypass kueri halaman undangan publik yang sering dibaca, serta mengelola antrean (job queue).



Queue (Laravel Queue + Redis): Menangani proses asinkron berat seperti pengiriman Email OTP, pengiriman notifikasi WhatsApp massal, dan pembuatan dokumen PDF Invoice/export Excel.



Email Service (Mailgun/SendGrid): Gateway SMTP transaksional pihak ketiga yang andal.



WhatsApp Service (Wablas/Unofficial Provider): Gateway pengiriman pesan dinamis massal.



Payment Gateway (Midtrans): Pemrosesan pembayaran otomatis dengan verifikasi instan via secure webhooks.



Domain Service (Cloudflare API): Otomatisasi pendaftaran CNAME dan verifikasi Custom Domain klien secara terprogram dari dashboard customer.



2\. Tech Stack \& Alasan Pemilihan



A. Frontend



Teknologi: Next.js 15+ (React 19), Tailwind CSS, Zustand (State Management), Framer Motion (Animation).



Alasan: Next.js mendukung Static Site Generation (SSG) dan Incremental Static Regeneration (ISR). Fitur ISR sangat krusial bagi halaman undangan karena data pengantin jarang berubah secara drastis, sehingga HTML dapat di-cache di CDN dan dimuat di bawah 1 detik tanpa membebani database backend pada setiap kunjungan tamu. Tailwind digunakan untuk kemudahan pembuatan tema kustom secara dinamis tanpa overhead CSS.



B. Backend



Teknologi: Laravel 11+ (PHP 8.3+), Spatie Media Library, Laravel Sanctum (JWT Auth).



Alasan: Laravel menyediakan ekosistem terintegrasi yang matang, developer velocity yang tinggi, ORM (Eloquent) yang aman dari SQL injection, sistem antrean (Queue) bawaan yang kuat, dan pustaka integrasi pihak ketiga melimpah untuk menunjang kecepatan rilis ke pasar (time-to-market).



C. Database, Caching, \& Queue



Database: PostgreSQL 16



Alasan: PostgreSQL memiliki penanganan tipe data JSONB yang sangat efisien untuk menyimpan Layout Draft Payload pada fitur Builder, serta indeks GIN untuk pencarian katalog cepat.



Cache \& Session: Redis



Alasan: Operasi in-memory berkecepatan tinggi cocok untuk mengelola cache dinamis halaman undangan dan throttling rate limiting.



Queue Driver: Redis Queue dengan Supervisor manager.



D. Cloud Storage, Image Optimization, \& Deployment



Storage: Cloudinary (untuk Media Galeri) \& AWS S3 Compatible (DigitalOcean Spaces, untuk Dokumen/Invoice).



Alasan: Cloudinary melakukan optimasi gambar otomatis on-the-fly (konversi otomatis ke WebP/AVIF, resizing cerdas berdasarkan DPR layar mobile).



Hosting/Server: Vercel (Frontend \& Render Engine) \& VPS / Cloud Server (Ubuntu + Docker untuk Backend \& DB).



Alasan: Vercel menyediakan jaringan CDN global (Edge) terbaik untuk aplikasi Next.js. VPS mandiri memberikan efisiensi biaya komputasi untuk menjalankan API Laravel, Redis, dan PostgreSQL skala besar.



3\. Arsitektur Frontend



Folder Structure (Next.js Monorepo-ish)



/frontend

├── /apps

│   ├── /store-dashboard   # Landing page, store, checkout, \& customer dashboard

│   │   ├── /app           # Next.js App Router (pages \& layouts)

│   │   ├── /components    # Local components (Bento Grid, Pricing, Sidebar)

│   │   └── /services      # Fetch API wrappers

│   └── /render-engine     # Isolate engine for Guest Invitation views

│       ├── /app           # Routing `/pernikahan/\[slug]`

│       └── /themes        # Core themes modules (Rustic, Modern, Classic)

├── /packages

│   ├── /ui                # Shared UI Components library (Buttons, Modals, Badges)

│   ├── /utils             # Shared utility functions (date formatter, currency, sanitizers)

│   └── /types             # Global TypeScript definitions





State Management \& API Layer



Zustand Store (useBuilderStore): Mengelola state draft pengeditan secara lokal di sisi klien selama sesi Builder aktif. Menyimpan mutasi perubahan objek JSON, riwayat undo/redo, dan memicu timer asinkron Auto Save.



API Layer (Axios Wrapper): Dilengkapi interseptor untuk secara otomatis melampirkan JWT token di header Authorization dan menangani pembaharuan token (token refresh) otomatis ketika terjadi respons HTTP 401.



Theme \& Rendering System: Sub-aplikasi render-engine memuat satu file data JSON raksasa yang mewakili seluruh detail undangan pengantin. Setiap tema (RusticTheme, ElegantTheme) adalah komponen React murni yang menerima data JSON tersebut sebagai props.



4\. Arsitektur Backend



Laravel Module-based Structure



Backend diorganisasi menggunakan struktur modular logika untuk menjaga kerapian saat sistem berkembang besar:



/app

├── /Http

│   ├── /Controllers

│   │   ├── /Auth          # Register, Login, Social Auth, Password Reset

│   │   ├── /Customer      # Invitation, RSVP, Media upload

│   │   └── /Public        # Public RSVP post, Guestbook views, Slug fetcher

│   └── /Middleware        # JWT, Ownership check, Rate limiting

├── /Models                # Eloquent Models (User, Invitation, RSVP, Event)

├── /Services              # Domain Logic layer (MidtransService, WhatsAppService, CloudflareAPI)

├── /Repositories          # Data Access abstraction layer

├── /Policies              # Authorization logic (IsOwnerOfInvitation)

├── /Jobs                  # Queue jobs (SendWhatsAppNotification, GenerateInvoicePDF)

├── /Events \& /Listeners   # Event-Driven Hooks (e.g., OrderPaid -> ActivateInvitation)

└── /Notifications         # Transactional Emails \& System Notifications





Pola Aliran Request (Architecture Pattern)



Setiap request yang masuk wajib melewati lapisan pertahanan berikut sebelum mengubah data di Database:



HTTP Request -> Web Server (Nginx) -> Middleware (Auth/Sanitize) -> FormRequest Validation -> Controller -> Service (Business Logic) -> Repository (Data Store) -> Database





5\. Database Architecture



Entity Relationship Model (Sederhana \& Terindeks)



&#x20;  \[Users] 1 --------- N \[Invitations] 1 --------- 1 \[Invitation\_Settings]

&#x20;     |                       |

&#x20;     | 1                     | 1

&#x20;     v                       ├--------- N \[Events]

&#x20;  \[Orders] 1                 ├--------- N \[Stories]

&#x20;     |                       ├--------- N \[Galleries]

&#x20;     | 1                     ├--------- N \[Gifts]

&#x20;     v                       ├--------- N \[Guest\_RSVPs]

&#x20;  \[Payments]                 ├--------- N \[Guest\_Books]

&#x20;                             └--------- N \[Analytics]





Pengoptimalan Performa Skema Database:



Primary Keys: Menggunakan UUIDv4 untuk entitas Users, Invitations, Orders, Guest\_RSVPs, dan Guest\_Books guna mencegah enumerasi data oleh penyerang luar.



Indexes (Strategis):



Indeks B-Tree pada Invitations(slug) dan Invitations(status).



Indeks Komposit pada Guest\_RSVPs(invitation\_id, attendance).



Indeks GIN pada Invitation\_Drafts(draft\_payload) untuk mendukung kueri pencarian properti JSON dinamis secara efisien.



Soft Deletes: Diimplementasikan pada tabel Invitations dan Users agar data yang tidak sengaja dihapus oleh pelanggan dapat dipulihkan oleh tim admin CS.



Audit Logging (activity\_logs): Mencatat setiap perubahan status kritis (misalnya: admin mengubah status paket user, reseller menambah kredit, atau pengantin menarik dana amplop digital).



6\. Storage Architecture



Semua berkas diletakkan di Cloud Storage dengan struktur folder hierarki yang aman:



/uploads/invitations/{id}/gallery/: Foto-foto prewedding (menggunakan optimasi otomatis Cloudinary, melayani berkas dalam WebP/AVIF).



/uploads/invitations/{id}/music/: File audio MP3 latar belakang pengantin.



/uploads/invitations/{id}/qris/: Gambar kode QRIS pembayaran hadiah digital.



/invoices/{year}/{month}/: File PDF tagihan pembelian terlindung (akses menggunakan Presigned S3 URL jangka pendek).



/exports/{user\_id}/: File Excel unduhan data RSVP yang akan kedaluwarsa otomatis dalam 24 jam melalui kebijakan siklus hidup (lifecycle policy) S3.



7\. Security Architecture



Mekanisme Autentikasi Dual-Token (JWT)



Access Token: Disimpan di memori jangka pendek Frontend state (Zustand). Kedaluwarsa dalam 15 menit. Dilampirkan di header Authorization: Bearer <token>.



Refresh Token: Disimpan dalam secure cookie dengan flag: HttpOnly, Secure, SameSite=Strict. Cookie ini hanya dikirim oleh browser ke endpoint /api/auth/refresh untuk menerbitkan Access Token baru. Ini 100% aman dari pencurian token lewat skrip berbahaya (XSS).



Keamanan Sistem Transaksional \& Input



Ownership Validation Middleware (Laravel):

Setiap endpoint yang memodifikasi isi undangan dilindungi oleh Middleware Kepemilikan. Middleware mengekstrak user\_id dari JWT dan membandingkannya dengan kolom user\_id di tabel Invitations berdasarkan ID parameter rute. Jika tidak cocok, sistem langsung mengembalikan respons HTTP 403 Forbidden.



Proteksi XSS pada Buku Tamu (Guest Book):

Sebelum teks doa tamu disimpan di database, Backend Laravel membersihkannya menggunakan pustaka mews/purifier (HTML Purifier) untuk menghilangkan tag <script>, onload, javascript:, atau elemen manipulasi DOM lainnya.



Proteksi DDoS \& Brute-Force (Throttling):



Endpoint /api/auth/login dibatasi maksimal 5 percobaan gagal per IP per 15 menit.



Endpoint /api/v1/invitation/{id}/rsvp dibatasi maksimal 5 request per IP per menit menggunakan Redis Rate Limiter untuk menghindari spam konfirmasi kehadiran palsu.



8\. Builder Architecture



Sistem WYSIWYG Builder beroperasi menggunakan pemisahan status data (Draft \& Live) untuk menghindari kerusakan halaman publik saat pengguna sedang melakukan eksperimen desain:



&#x20;                      \[ Frontend Zustand State ]

&#x20;                                 |

&#x20;                                 | (Pengetikan / Modifikasi Layout)

&#x20;                                 v

&#x20;                      \[ Debounced Auto Save ] (Tunda 1.5 detik)

&#x20;                                 |

&#x20;                                 v  PUT /api/v1/builder/{id}

&#x20;                      \[ Database: Invitation\_Drafts ] (Payload JSONB)

&#x20;                                 |

&#x20;                (Klik "Publish Perubahan" oleh Pengantin)

&#x20;                                 v  POST /api/v1/builder/{id}/publish

&#x20;                      \[ Database: Invitations ] (Update Live Data)

&#x20;                                 |

&#x20;                      \[ Invalidasi Cache Redis ]





Mekanisme Pengurutan Bagian (Section Reordering)



Struktur urutan bagian disimpan dalam bentuk array sederhana di dalam JSON konfigurasi, misalnya: sections\_order: \['cover', 'mempelai', 'acara', 'galeri', 'rsvp', 'footer']. Mengubah urutan menggunakan modul drag-and-drop hanya memanipulasi urutan nilai string di dalam array ini, sehingga sangat ringan dan tidak merusak skema database SQL.



9\. Public Invitation Architecture



Untuk menyajikan performa loading ekstrem (di bawah 1.5 detik) pada perangkat seluler tamu undangan, arsitektur visual dan transmisi data diatur dengan skema berikut:



Strategi Pemuatan Progresif (Next.js Dynamic Imports)



Critical Assets First: HTML dasar, file CSS fundamental, lagu latar belakang, dan gambar cover utama dimuat terlebih dahulu.



Lazy Components Loading: Komponen interaktif berat seperti Google Maps iframe, galeri foto Masonry, pemutar video YouTube, dan Wishes Wall dideklarasikan menggunakan Next.js dynamic() dengan flag ssr: false. Komponen-komponen ini hanya akan mulai di-render dan mengunduh JavaScript-nya ketika pengguna menggulir layar mendekati koordinat komponen tersebut (Intersection Observer).



SEO Pre-rendering (SSR): Saat bot perayap WhatsApp atau Google mengakses /pernikahan/\[slug], Next.js langsung menyajikan tag <meta> Open Graph lengkap dengan gambar thumbnail cover pengantin yang telah dikompresi, sehingga pratinjau tautan (link preview) langsung muncul dengan indah dan instan di ruang obrolan.



10\. Infrastruktur \& DevOps



Lingkungan Server (Environments Layout)



Development: PC lokal pengembang menggunakan Docker Compose untuk menjalankan kontainer Laravel, Next.js, Postgres, dan Redis.



Staging: VPS kelas menengah (2 vCPU, 4GB RAM) yang mereplikasi data produksi secara berkala untuk keperluan pengujian QA internal.



Production: Arsitektur terpisah:



Next.js Frontend di-deploy di infrastruktur global edge Vercel Pro.



Backend Laravel, Postgres, Redis, dan Queue Worker berjalan di Cloud Server (VPS KVM) terdedikasi berkinerja tinggi yang dikelola dengan Docker Swarm atau Kubernetes sederhana.



Skema Container Service (Docker Compose - Production)



services:

&#x20; nginx:

&#x20;   image: nginx:alpine

&#x20;   ports: \["80:80", "443:443"]

&#x20; laravel-api:

&#x20;   build: ./backend

&#x20;   restart: always

&#x20; queue-worker:

&#x20;   build: ./backend

&#x20;   command: php artisan queue:work redis --sleep=3 --tries=3

&#x20; postgres-db:

&#x20;   image: postgres:16-alpine

&#x20;   volumes: \[pgdata:/var/lib/postgresql/data]

&#x20; redis-cache:

&#x20;   image: redis:7-alpine





11\. Deployment Flow (CI/CD Pipeline)



Sistem menggunakan alur otomatisasi berbasis GitHub Actions untuk memastikan rilis kode berjalan aman tanpa adanya waktu henti (Zero-Downtime Deployment).



&#x20;  \[ Developer Push Code ke Branch 'main' ]

&#x20;                      |

&#x20;                      v

&#x20;        \[ GitHub Actions Runner Dipicu ]

&#x20;                      |

&#x20;  ┌───────────────────┴───────────────────┐

&#x20;  ▼ (Linting \& Testing)                   ▼ (Docker Build \& Push)

\[ Run PHPUnit \& Jest ]                  \[ Build Docker Image ]

\[ Linter \& Static Analysis ]            \[ Push ke Private Registry ]

&#x20;  │                                       │

&#x20;  └───────────────────┬───────────────────┘

&#x20;                      v (Lolos Verifikasi)

&#x20;    \[ Deployment Triggered ke Server Utama ]

&#x20;                      |

&#x20;                      v (Zero-Downtime Rolling Update)

&#x20;    \[ Server Menarik Image Baru dari Registry ]

&#x20;                      |

&#x20;    \[ Jalankan Artisan DB Migration (Safe Mode) ]

&#x20;                      |

&#x20;    \[ Switch Traffic ke Container Baru \& Matikan Container Lama ]

&#x20;                      |

&#x20; \[ Sukses! (Jika Gagal, Otomatis Rollback ke Container Tag Sebelumnya) ]





12\. Folder Structure Umum (Workspace Utama)



Untuk koordinasi monorepo atau repositori terintegrasi, struktur folder root diatur sebagai berikut:



/platform-undangan-digital (Root)

├── /frontend               # Seluruh kode visual (Next.js Apps \& Shared Packages)

├── /backend                # API Headless (Laravel, artisan scripts, DB migrations)

├── /documentation          # File Markdown, dokumen panduan API (Swagger/Postman)

├── /docker-configs         # File konfigurasi Nginx, Dockerfiles, Docker Compose

└── /shared-assets          # Logo platform, default template assets, \& SVG master





13\. Coding Standard \& Kebijakan Pengembangan



A. Naming Convention



Backend (PHP/Laravel):



Class \& Controller: PascalCase (contoh: InvitationController).



Method \& Variable: camelCase (contoh: generateQrCode).



Database Table \& Column: snake\_case (contoh: guest\_rsvps, is\_email\_verified).



Frontend (TypeScript/Next.js):



React Components: PascalCase (contoh: WishesWall.tsx).



Hooks \& Utility: camelCase (contoh: useActiveTheme.ts).



B. Commit Message Convention (Angular Style)



Setiap pesan commit wajib mengikuti format berikut untuk mempermudah pelacakan changelog:



feat: <deskripsi> - Untuk fitur fungsional baru (misal: feat: add midtrans qr\_code checkout).



fix: <deskripsi> - Untuk perbaikan bug (misal: fix: resolve image compression failure on safari).



refactor: <deskripsi> - Pengubahan struktur kode tanpa menambah fitur (misal: refactor: optimize database query on analytics).



docs: <deskripsi> - Penulisan dokumen atau manual sistem.



C. Branching Strategy (Git Flow Sederhana)



main: Menyimpan kode stabil yang berjalan di server produksi.



staging: Cabang rilis untuk integrasi dan pengujian sistem QA.



feature/<nama-fitur>: Cabang sementara yang dibuat oleh developer untuk mengerjakan satu fitur tertentu (misal: feature/builder-undo-redo), wajib digabungkan kembali ke staging via Pull Request yang ditinjau (code review).


FRONTEND ARCHITECTURE SPECIFICATION



Proyek: Platform SaaS \& E-Commerce Undangan Digital



Lokasi Berkas: docs/03-sad/02-Frontend-Architecture.md



Status: Approved for Implementation



Target Pemakai: Frontend Architects, Lead Developers, UI/UX Engineers



1\. Frontend Overview



Frontend dari platform SaaS Undangan Digital ini dirancang sebagai sistem yang terdekopel (decoupled frontend architecture) untuk melayani tiga ranah bisnis yang memiliki karakteristik performa, SEO, dan interaktivitas yang berbeda secara radikal:



Storefront \& Landing Page (E-Commerce): Fokus pada daya pikat visual, akuisisi pengguna, SEO on-page yang sempurna, kecepatan muat instan, dan optimasi konversi.



SaaS Dashboard (Customer \& Admin): Panel administratif padat data (data-dense workflows) yang menangani operasi pembuatan draf undangan, manajemen transaksi, analitik RSVP, serta integrasi manajemen klien bagi Reseller.



Guest Invitation (Render Engine): Mesin perender khusus untuk melayani halaman undangan publik yang dikonsumsi oleh jutaan tamu secara simultan di perangkat seluler dengan variasi kestabilan koneksi internet.



Arsitektur frontend ini diatur sebagai Turborepo Monorepo untuk membagi beban aplikasi ke dalam sub-aplikasi yang diisolasi secara proses namun tetap berbagi pustaka komponen visual (design system), utilitas enkripsi, penanganan data, dan skema tipe TypeScript global.



2\. Framework yang Digunakan



Platform ini mengadopsi Next.js 15+ (berjalan di atas React 19) sebagai fondasi utama. Semua kode ditulis secara ketat menggunakan TypeScript 5+ untuk menjamin keamanan tipe (type safety) di seluruh aliran data aplikasi. Kerangka kerja styling visual didukung secara eksklusif oleh Tailwind CSS v4 guna mengeliminasi penumpukan berkas CSS statis dan mempercepat proses rendering visual di tingkat browser.



3\. Alasan Memilih Next.js



Pemilihan Next.js didasarkan pada keputusan taktis untuk mencapai keseimbangan optimal antara efisiensi pengembangan (Developer Experience) dan kecepatan akses pengguna (User Experience):



Incremental Static Regeneration (ISR): Memungkinkan platform merender ratusan ribu halaman undangan publik sebagai berkas HTML statis di CDN Edge, lalu memperbaruinya secara asinkron di latar belakang saat terjadi RSVP baru atau perubahan konten draf. Ini mengurangi Time to First Byte (TTFB) hingga < 100ms.



React Server Components (RSC): Memindahkan porsi rendering komponen non-interaktif dan penarikan data awal ke sisi server. Hal ini secara drastis mengurangi ukuran bundel JavaScript (JS bundle size) yang dikirimkan ke perangkat mobile tamu undangan yang sering kali memiliki spesifikasi rendah.



Built-in Asset Optimization: Komponen <Image> bawaan Next.js mengintegrasikan konversi format gambar (menjadi WebP/AVIF) dan penyesuaian resolusi gambar secara asinkron berdasarkan DPI layar pengakses, menghindarkan platform dari masalah gambar prewedding raksasa yang merusak performa muat.



Edge Middleware Routing: Memungkinkan platform menangani custom domain klien secara langsung di tingkat CDN Edge melalui Cloudflare/Vercel Middleware tanpa membebani server aplikasi utama dengan kueri routing.



4\. Folder Structure (Monorepo Specification)



Struktur proyek frontend diimplementasikan menggunakan arsitektur monorepo berbasis Turborepo untuk menyatukan dependensi, membagi tugas build caching, dan mempertahankan batas modularitas:



/frontend

├── /apps

│   ├── /store-dashboard         # Next.js App - Storefront, Checkout, \& Dashboard (Customer, Reseller, Admin)

│   │   ├── /app                 # Next.js App Router (RSC \& Client Routes)

│   │   ├── /components          # Komponen UI lokal dashboard dan e-commerce

│   │   ├── /features            # Modul fitur mandiri (builder, billing, rsvp)

│   │   ├── /hooks               # Custom hooks fungsional lokal

│   │   ├── /services            # Axios instances \& konfigurasi API Client

│   │   └── /stores              # Zustand global state (auth, checkout)

│   └── /render-engine           # Next.js App - Engine khusus perender undangan publik

│       ├── /app                 # Routing khusus `/\[slug]` \& `/\[slug]/\[guest\_name]`

│       └── /themes              # Engine modul visual tema (Modern, Rustic, Classic)

├── /packages

│   ├── /ui                      # Shared Design System (Atomic Design components)

│   │   ├── /atoms               # Buttons, Inputs, Badges, Typography

│   │   ├── /molecules           # FormFields, CardTemplates, CustomSelects

│   │   └── /organisms           # Tables, Navigation bars, Collapsible sidebars

│   ├── /utils                   # Utilitas bersama (dateFormatter, currencyConverter, sanitizers)

│   └── /types                   # Master Contract - Tipe data TypeScript global

├── package.json                 # Monorepo Workspace settings

└── turbo.json                   # Turborepo build pipeline caching rules





5\. App Router



Aplikasi menggunakan Next.js App Router secara penuh. Setiap rute didefinisikan secara deklaratif menggunakan direktori fisik di dalam folder app/.



Secara default, semua halaman merupakan React Server Components (RSC) untuk meminimalkan beban komputasi klien.



Komponen interaktif (seperti form input, tombol aksi, modal interaktif) dideklarasikan sebagai Client Components dengan menyematkan direktif "use client" di baris paling atas berkas.



6\. Route Groups



Sistem mengimplementasikan Route Groups menggunakan tanda kurung (groupname) untuk mengatur arsitektur folder tanpa memengaruhi skema URL publik. Ini memisahkan tata letak (layout) dan kontrol akses middleware secara rapi:



app/(marketing): Menampung halaman Landing Page, Fitur, Pricing, FAQ, dan Blog. Menggunakan marketing-layout.tsx yang kaya akan elemen animasi dan visual.



app/(auth): Menampung rute Login, Register, Lupa Password, dan Reset Password. Menggunakan auth-layout.tsx yang bersih tanpa navigasi utama.



app/(dashboard): Menampung dasbor pengguna (/customer, /reseller, /admin). Menggunakan dashboard-layout.tsx yang mengintegrasikan sidebar persisten dan sistem otentikasi sesi aktif.



7\. Layout Architecture



Layout dirancang secara hierarkis (Nested Layouts) untuk mereduksi rendering ulang elemen statis (unnecessary re-renders):



app/layout.tsx (Root Layout): Menginisialisasi tag dasar HTML/Body, font sistem (Plus Jakarta Sans dan Inter), serta membungkus aplikasi dengan Providers global (React Query, Zustand Hydrator, UI Toast Container).



app/(dashboard)/layout.tsx: Menyediakan kerangka dasbor dengan Sidebar Navigasi responsif (menggunakan CSS transition untuk meminimalkan layout shift) dan Header Akun.



apps/render-engine/app/layout.tsx: Layout ultra-ringan yang diisolasi khusus tanpa memuat file CSS eksternal platform utama, guna menjamin performa rendering undangan publik di atas rata-rata.



8\. Component Architecture



Sistem menerapkan pemisahan yang ketat antara komponen cerdas yang memproses data (Container/Smart Components) dan komponen visual murni (Presentational/Dumb Components). Aliran data berjalan satu arah (Unidirectional Data Flow):



&#x20;              +-------------------------------------------+

&#x20;              |  Container Component (RSC / Page Router)  |

&#x20;              +-------------------------------------------+

&#x20;                                    │

&#x20;                                    │ (Mengirimkan Data Read-only via Props)

&#x20;                                    ▼

&#x20;              +-------------------------------------------+

&#x20;              | Stateful Client Component (Zustand/Query) |

&#x20;              +-------------------------------------------+

&#x20;                                    │

&#x20;                                    │ (Memicu Event Callback \& State Update)

&#x20;                                    ▼

&#x20;              +-------------------------------------------+

&#x20;              |     Presentational Component (UI Kit)     |

&#x20;              +-------------------------------------------+





9\. Atomic Design



Pustaka komponen bersama pada /packages/ui dirancang menggunakan adaptasi metodologi Atomic Design:



Atoms: Komponen visual terkecil yang tidak dapat dipecah lagi dan tidak memiliki dependensi data eksternal (contoh: <Button>, <Input>, <Badge>, <Typography>).



Molecules: Kombinasi sederhana dari beberapa komponen atom untuk membentuk satu fungsionalitas (contoh: <FormField> yang menyatukan <Label>, <Input>, dan teks validasi eror).



Organisms: Blok antarmuka kompleks yang membentuk area fungsional mandiri di layar (contoh: <SidebarNavigation>, <RSVPTableGrid>, <MediaDropzone>).



10\. Shared Components



Semua komponen yang diletakkan di /packages/ui harus mematuhi standar kegunaan ulang (reusability) yang ketat:



Style-Agnostic with Classes Extensibility: Komponen harus menerima prop className eksternal untuk digabungkan menggunakan pustaka tailwind-merge dan clsx (cn utility).



Strict Type Definitions: Setiap prop wajib dideklarasikan tipenya secara eksplisit (dilarang menggunakan any).



Interactive State Support: Komponen atom input wajib mendukung pemetaan referensi (ref forwarding) menggunakan React.forwardRef agar kompatibel dengan sistem manajemen form.



11\. Feature Modules



Aplikasi store-dashboard menerapkan modularisasi berbasis fitur (Feature-Driven Directory). Setiap folder di dalam /features mengisolasi semua aset, komponen, hooks, dan tipe data yang hanya relevan untuk domain fitur tersebut:



/features/builder: Mengelola semua logika WYSIWYG, panel editor, kontrol tema warna, dan live syncing.



/features/rsvp: Mengelola penarikan tabel tamu, konfirmasi RSVP, moderasi ucapan buku tamu, dan fitur filter data.



/features/billing: Mengelola checkout flow, detail invoice PDF generator, dan visualisasi status pembayaran.



12\. State Management (Zustand)



Manajemen state klien yang berumur panjang dan membutuhkan akses lintas komponen dikelola menggunakan Zustand. Zustand dipilih karena ukurannya yang sangat kecil (< 2KB), tidak memerlukan pembungkusan Context Provider global yang dapat merusak performa rendering React, serta menggunakan pendekatan pemilih (selectors) untuk mereduksi unnecessary re-renders.



Struktur Zustand Store Utama:



useAuthStore: Menyimpan informasi token akses JWT singkat, profil pengguna terenkripsi, dan status verifikasi email.



useBuilderStore: Menyimpan objek JSON draf berukuran besar yang mewakili status pengeditan undangan saat ini. Store ini memantau perubahan draf, memproses tumpukan history state untuk undo/redo, serta memicu auto-save ke API backend secara asinkron.



13\. Server State (TanStack Query)



Untuk data yang berasal dari basis data eksternal (statis maupun dinamis), platform mempercayakan pengelolaannya pada TanStack Query (React Query) v5:



Cache Management: React Query melacak masa segar data (stale time) secara cerdas di latar belakang untuk menghindari panggilan API berulang saat pengguna berpindah navigasi dasbor.



Optimistic Updates: Saat pengguna mengubah status kehadiran tamu atau menghapus pesan di buku tamu, React Query langsung memanipulasi cache lokal untuk merender perubahan tersebut secara instan di layar, lalu menyinkronkannya dengan database di latar belakang. Jika sinkronisasi gagal, state visual akan dibatalkan (rolled back) secara otomatis.



Infinite Scroll Catalog: Mengintegrasikan useInfiniteQuery untuk melayani pemuatan katalog template yang tidak terputus (seamless template discovery).



14\. API Layer



API Layer diabstraksikan melalui berkas Axios client yang dikonfigurasi secara modular (/services/apiClient.ts):



Request Interceptor: Secara otomatis menyuntikkan Access Token JWT dari memori Zustand ke dalam header HTTP Authorization: Bearer <token> pada setiap permintaan API yang membutuhkan otentikasi.



Response Interceptor (Token Rotation): Jika API mengembalikan status HTTP 401 Unauthorized (karena Access Token kedaluwarsa), interseptor akan menahan permintaan asli, memicu pemanggilan asinkron ke endpoint /api/auth/refresh secara terpisah untuk memperbarui token menggunakan HttpOnly Secure Cookie, lalu mengulangi permintaan asli yang tertahan tadi dengan token baru tanpa interupsi visual pada dasbor pengguna.



15\. Authentication Flow



Otentikasi didesain dengan tingkat keamanan maksimal tanpa menyimpan kredensial di media penyimpanan lokal (LocalStorage) yang rentan terhadap serangan XSS:



&#x20;1. Form Login Diisi ──> POST /api/auth/login ──> Backend Memvalidasi

&#x20;                                                         │

&#x20;                               ┌─────────────────────────┴─────────────────────────┐

&#x20;                               ▼ (Response Body)                                   ▼ (Set-Cookie Header)

&#x20;                 Access Token (JWT, valid 15 Menit)                 Refresh Token (HttpOnly, Secure, SameSite, 7 Hari)

&#x20;                               │                                                   │

&#x20;                               ▼                                                   ▼

&#x20;               Disimpan di Memori Zustand State                       Disimpan Aman di Storage Browser





16\. Dashboard Architecture



Arsitektur Dashboard dirancang sebagai lingkungan kerja padat data yang sangat responsif. Tata letak utama menggunakan sistem navigasi Sidebar yang dapat diringkas (collapsible) secara visual. Elemen-elemen visual dasbor, termasuk widget grafik analitik dan tabel manajemen tamu, memprioritaskan performa kueri dengan teknik de-bouncing pada form pencarian, pemrosesan filter langsung di sisi klien (client-side filter caching), serta membatasi rendering elemen grafik yang belum masuk ke area pandang.



17\. Invitation Builder Architecture



Modul Builder menggunakan arsitektur Isolated Split-Screen Workspace:



Panel Form Editor (Sisi Kiri): Diisi form isian terkelompok menggunakan teknik accordion.



Live Preview Panel (Sisi Kanan): Merupakan area pembungkus mockup smartphone yang memuat render-engine dalam elemen <iframe>.



Mekanisme Sinkronisasi Inter-Window: Ketika pengguna mengetik data baru di form, state Zustand diperbarui. Store Zustand mengeksekusi komunikasi antar-jendela browser menggunakan skema HTML5 postMessage untuk mengirimkan payload perubahan JSON secara instan ke dalam <iframe> tanpa memicu pemuatan ulang halaman. Browser di dalam <iframe> menangkap pesan tersebut secara asinkron, meng-update representasi data tema lokalnya, dan memicu render ulang mikro-komponen secara lokal dengan latensi < 50ms.



18\. Public Invitation Architecture



Sub-aplikasi render-engine untuk melayani halaman publik tamu undangan dioptimalkan secara ekstrem:



Zero Main Application Bundle Bloat: Aplikasi ini diisolasi dari bundel visual dasbor utama. Kode-kode visual pustaka grafik dasbor tidak akan pernah ikut diunduh oleh tamu undangan.



Minimal JavaScript Footprint: Memaksimalkan fitur static markup dari React Server Components. JavaScript hanya diunduh untuk kebutuhan interaksi dasar (kontrol audio, transisi visual, form RSVP, dan pengiriman pesan ucapan).



Edge Caching Service: Halaman HTML statis disajikan dari Cloudflare CDN Edge, memotong siklus request ke database backend utama secara signifikan.



19\. Theme System (Dynamic Styling Engine)



Sistem kustomisasi visual undangan tidak menggunakan penulisan file CSS unik per template. Platform mengimplementasikan Tailwind CSS-Variables-Based Styling Engine:



Setiap tema (Rustic Theme, Elegant Theme, Modern Theme) mendefinisikan layout HTML-nya sendiri dengan merujuk pada pemetaan CSS variabel global yang disematkan di root pembungkus tema (seperti --theme-primary, --theme-secondary, --font-theme-heading, --font-theme-body).



Pada saat halaman undangan publik dimuat, sistem membaca preferensi visual dari konfigurasi JSON yang ditarik dari database (misalnya: primary\_color: '#556B2F').



Preferensi ini disuntikkan sebagai inline CSS variables pada elemen root pembungkus: <div style={{ '--theme-primary': themeData.primary\_color }}>.



Tailwind secara dinamis memetakan kelas utility kustom seperti bg-\[var(--theme-primary)] atau text-\[var(--theme-primary)] tanpa perlu melakukan kompilasi CSS ulang, memberikan kebebasan mutlak bagi pengguna untuk mengubah warna dasar tema secara dinamis di Builder.



20\. Responsive Strategy



Platform menetapkan arah perancangan responsif yang sangat terarah berdasarkan target pemakaian:



Dashboard \& Builder Workspace: Didesain dengan prioritas Desktop-First. Pengerjaan pengeditan visual dan pemantauan analitik terperinci paling produktif dilakukan di layar lebar. Pada layar seluler, panel preview disembunyikan dan hanya dapat dipanggil via Floating Action Button (FAB).



Storefront \& Katalog: Menggunakan sistem grid dinamis berbasis breakpoints Tailwind (grid-cols-1 sm:grid-cols-2 lg:grid-cols-4).



Guest Invitation Page: Menggunakan metodologi Mobile-First Mutlak. Skala ukuran teks menggunakan satuan relatif (rem / em) dan proporsi kontainer menggunakan persentase lebar layar (vw) untuk menjamin konsistensi visual pada semua aspek rasio layar ponsel tamu undangan.



21\. Form Management



Semua form input data di dalam Builder dan Dashboard dikelola menggunakan React Hook Form. Pustaka ini dipilih karena mengandalkan pendekatan Uncontrolled Inputs (mengakses nilai elemen input via React refs). Pendekatan ini mencegah terjadinya render ulang global komponen dasbor setiap kali pengguna mengetik karakter baru di dalam form, menjamin performa input tetap berjalan stabil di kecepatan 60fps walaupun form isian data sangat panjang.



22\. Validation



Formulir input divalidasi secara deklaratif menggunakan skema penulisan dari Zod:



Aturan validasi (seperti validasi panjang karakter nama mempelai, format URL Instagram, validasi ekstensi file gambar) dideklarasikan sebagai berkas skema terpusat di /packages/utils/schemas/.



Skema Zod diintegrasikan langsung sebagai resolver di React Hook Form (@hookform/resolvers/zod).



Validasi dipicu secara instan di sisi klien pada event On-Blur (saat pengguna memindahkan fokus kursor dari kolom input), memberikan peringatan instan sebelum form diserahkan ke server. Skema Zod yang sama diimpor langsung oleh tim backend Laravel untuk memastikan integritas data dari kedua sisi.



23\. Error Handling



Sistem menerapkan taktik degradasi fungsional yang halus (Graceful Degradation):



React Error Boundaries: Seluruh area visual krusial (seperti area rendering Iframe Preview, visualisasi diagram analitik, tabel pembayaran) dibungkus secara terpisah menggunakan komponen Error Boundary. Jika terjadi kegagalan rendering pada plugin eksternal, area tersebut akan digantikan dengan tampilan alternatif (Fallback UI) berupa pesan eror yang ramah dan tombol "Coba Lagi" tanpa melumpuhkan sisa aplikasi dasbor lainnya.



Axios Global Interceptor Response Catching: Setiap eror HTTP di luar status sukses (seperti eror 500 dari server atau eror 403) akan secara otomatis ditangkap untuk memicu kemunculan Toast Notification merah berisi deskripsi kesalahan yang mudah dipahami oleh pengguna umum.



24\. Loading Strategy



Sistem melarang keras penggunaan indikator pemuatan satu halaman penuh (Full-screen Blocking Spinners) karena menurunkan impresi kecepatan aplikasi secara psikologis:



Streaming \& Suspense: Next.js Server Components membagi halaman dasbor menjadi komponen terpisah. Tata letak dasar dasbor langsung tampil di detik pertama, sedangkan area yang membutuhkan proses kueri lambat (seperti statistik analitik atau riwayat tagihan) dimuat secara asinkron menggunakan pembungkus <Suspense> dengan visualisasi skeleton loading lokal.



Button Loading States: Setiap tombol kirim (submit) yang melakukan proses penulisan ke database otomatis berubah status menjadi disabled dengan menampilkan mikro-spinner di dalam tombol tersebut, mencegah pengiriman data ganda (double-submit).



25\. Skeleton Loading



Kerangka penampung abu-abu berdenyut (Skeleton Loading) dirancang untuk meniru kontur fisik asli komponen yang sedang memuat data dari API. Hal ini memastikan tidak terjadi pergeseran tata letak visual (Cumulative Layout Shift - CLS) yang mengganggu mata pengguna sesaat setelah data dari API selesai diunduh dan dirender di layar dasbor.



26\. Toast Notification



Feedback visual interaksi asinkron disajikan melalui Toast Notification System:



Toast diletakkan menetap di pojok kanan bawah pada Desktop atau di atas layar pada perangkat Mobile.



Memiliki variasi visual warna semantik: Hijau (Sukses), Merah (Kesalahan), Kuning (Peringatan).



Dikonfigurasi menghilang otomatis dalam waktu 3 detik, namun tetap menyajikan tombol tutup silang (x) untuk dibatalkan manual oleh pengguna.



Sistem antrean toast diatur dengan batas tampilan simultan maksimal 3 baris untuk mencegah penumpukan informasi visual yang mengganggu area kerja dasbor.



27\. Modal System



Modal digunakan eksklusif untuk interaksi terisolasi yang membutuhkan keputusan sadar dan fokus penuh dari pengguna (seperti konfirmasi penghapusan draf, pop-up konfirmasi perpanjangan masa aktif paket, atau konfirmasi keluar akun):



Modal dirender di atas elemen pelindung latar (backdrop overlay gelap #18181B dengan opasitas 40% dan efek filter blur ringan).



Harus mendukung interaksi penutupan instan dengan menekan tombol tombol fisik Esc pada keyboard atau mengklik area luar modal di atas backdrop overlay.



Mengunci fokus navigasi keyboard (Focus Trap) di dalam elemen modal saat modal aktif untuk kepentingan aksesibilitas.



28\. Drawer System



Drawer (panel geser dari tepi samping layar) atau bottom-sheets diimplementasikan untuk navigasi data yang padat namun membutuhkan kedalaman membaca tanpa kehilangan konteks halaman dasbor utama:



Pada Layar Desktop: Digunakan untuk menampilkan bantuan pelanggan transaksional, daftar riwayat notifikasi sistem, atau detail pengisian voucher diskon.



Pada Layar Mobile: Drawer desktop diubah formatnya menjadi Bottom-Sheet (panel ditarik dari bawah layar ke atas) karena jauh lebih ergonomis bagi jangkauan ibu jari pengguna seluler.



29\. Performance Optimization



Target optimasi performa frontend dipatok ketat pada parameter Google Core Web Vitals (Skor minimum > 90 untuk semua metrik di desktop, dan > 85 di mobile):



Tree Shaking \& Bundle Splitting: Mengeliminasi seluruh modul kode dari library pihak ketiga yang tidak benar-benar dieksekusi di aplikasi.



Modern CSS Purging: Mengandalkan sistem kompilasi bawaan Tailwind untuk memangkas utility CSS yang tidak terpakai dari bundel produksi, membatasi ukuran berkas CSS final di bawah 40KB.



DNS Prefetching \& Preconnecting: Menyematkan tag penunjuk optimasi koneksi awal pada server Cloudinary, AWS S3, Google Maps API, dan endpoint Payment Gateway di header HTML guna mempercepat siklus penarikan aset media.



30\. Lazy Loading



Image Lazy Loading: Semua elemen gambar non-kritis menggunakan properti loading="lazy". Properti ini didukung oleh penanganan placeholder blur berbasis WebP resolusi rendah (LQIP - Low Quality Image Placeholders) agar transisi pemuatan gambar prewedding tampak mewah dan tidak kasar.



Viewport Observer: Bagian peta Google Maps interaktif dan video YouTube di halaman undangan publik baru akan ditarik skrip iframe-nya hanya saat elemen kontainer tersebut berjarak kurang dari 200px menuju area pandang aktif pengguna (viewport).



31\. Dynamic Import



Komponen interaktif yang berukuran besar dan jarang dieksekusi pada pemuatan pertama (seperti modul pembaca statistik grafik bar/line yang berat, fungsionalitas pembuat dokumen PDF Invoice, atau QR Code Scanner generator) diimpor secara dinamis menggunakan utilitas next/dynamic dari Next.js:



Modul-modul ini diisolasi dari bundel inisial halaman utama. Kode asinkronnya baru akan diunduh oleh browser klien setelah pemicu interaksi pengguna aktif (seperti menekan tombol "Ekspor Laporan" atau menekan tab "Statistik Detail"), menghemat bandwidth muatan pertama secara dramatis.



32\. SEO Strategy



Optimasi mesin pencari dikelola secara asinkron di tingkat server menggunakan Next.js Metadata API:



Halaman toko katalog template dan halaman undangan publik memanfaatkan fungsi eksternal generateMetadata() di tingkat Next.js Server Components. Fungsi ini memicu pemanggilan cepat data SEO (judul acara, deskripsi singkat, dan URL gambar cover utama pengantin) dari database sebelum HTML dikirimkan ke klien.



Hasil markup HTML sudah mengintegrasikan tag Open Graph (og:title, og:description, og:image) dan JSON-LD Structured Data yang lengkap. Saat link dibagikan di WhatsApp, pratinjau thumbnail cover langsung tampil megah dan instan tanpa latensi penarikan bot WhatsApp.



33\. Accessibility (a11y)



Untuk menjamin keramahan penggunaan bagi semua kalangan tamu undangan (termasuk lansia atau pengguna dengan keterbatasan fisik visual):



Semantic Markups: Memaksimalkan penggunaan tag semantik HTML5 (<header>, <main>, <section>, <article>, <nav>, <footer>) alih-alih menggunakan elemen generik <div> untuk menyusun tata letak.



ARIA Attributes Support: Semua ikon dekoratif yang tidak memiliki representasi teks wajib disisipkan tag aria-hidden="true". Elemen tombol atau form interaktif wajib menyertakan deskripsi visual pembantu (aria-label atau aria-describedby) yang terbaca sempurna oleh perangkat lunak pembaca layar (Screen Reader).



Keyboard Tab Navigation: Seluruh elemen interaktif form, dropdown, tab, dan tombol wajib dapat diakses, disorot, dan dieksekusi menggunakan navigasi tombol Tab dan Enter secara berurutan.



34\. Folder Naming Convention



Penamaan folder di dalam seluruh area frontend monorepo wajib mematuhi standar kebab-case secara kaku (seluruh huruf kecil, kata dipisahkan oleh tanda hubung -):



Contoh penamaan direktori fungsional: store-dashboard, render-engine, shared-components, image-optimizer, rsvp-analytics.



Aturan penamaan folder ini meminimalkan masalah sensitivitas karakter (case-sensitivity) saat melakukan kompilasi bundel pada lingkungan server produksi berbasis sistem operasi Linux/Unix.



35\. Component Naming Convention



Penamaan berkas file komponen visual frontend diatur dengan ketentuan ketat berikut:



React Components File: Wajib ditulis menggunakan format PascalCase dan berekstensi .tsx. Contoh: WishesWall.tsx, Button.tsx, NavbarDashboard.tsx.



Custom Hooks File: Wajib ditulis diawali kata use menggunakan format camelCase dan berekstensi .ts. Contoh: useBuilderStore.ts, useIntersectionObserver.ts.



Utility \& Configuration File: Wajib ditulis menggunakan format camelCase dan berekstensi .ts atau .json. Contoh: apiClient.ts, tailwind.config.ts, themeVariables.json.



36\. Best Practices (Aturan Emas Pengembang Frontend)



Setiap pengembang frontend yang berkontribusi pada platform ini wajib mematuhi 5 aturan emas pengembangan berikut:



Larang Impor Berulang (DRY Principle): Dilarang keras menulis ulang atau menyalin fungsionalitas visual yang identik secara berulang. Komponen fungsional universal wajib diangkat statusnya sebagai pustaka komponen bersama di /packages/ui.



TypeScript Strict Configuration: Penggunaan tipe data penutup dosa any dilarang secara mutlak. Setiap properti data API wajib dideklarasikan bentuk kontrak antarmukanya secara mendalam di direktori /packages/types.



No Business Logic in Atoms Components: Komponen atom dan molekul visual harus bersih dari logika kueri database, instansiasi panggilan Axios, atau manipulasi state global. Komponen tersebut harus murni bertindak sebagai penerima props visual dan penembak event callback ke atas.



Debounce All Search \& Filtering Triggers: Setiap form input pencarian di katalog atau dashboard tamu wajib dibatasi laju pemicunya menggunakan fungsi debounce minimal 300ms guna menghindari penumpukan request visual yang memicu pembekuan antarmuka (UI freezing).



Continuous Bundle Analysis Monitoring: Selalu awasi total ukuran kompilasi bundel kode sebelum merilis fitur baru ke server produksi, guna menjamin performa waktu muat awal tetap terjaga di bawah batas kritis.



37\. Kesimpulan



Dokumen Frontend Architecture Specification ini menyajikan cetak biru fungsional, performa, dan keamanan visual platform e-commerce serta SaaS undangan digital yang sangat modular dan kuat. Melalui pendekatan pemisahan tegas antara ekosistem visual toko dasbor (store-dashboard) dengan mesin perender ultra-cepat berbasis static-generation (render-engine), platform ini siap diimplementasikan oleh tim pengembang Anda menggunakan Next.js, Tailwind CSS, Zustand, dan TanStack Query dengan jaminan kestabilan performa berskala tinggi.


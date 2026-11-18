# PROJECT TeFA Wisesa (Auto Care Tracker)
Sistem informasi manajemen bengkel kendaraan terintegrasi: web admin, aplikasi mobile pelanggan, halaman pelacakan servis publik, backend REST API, dan database SQL Server.
---

## Tim Developer
|    Nama   |                    Peran                     |                     GitHub                     |
|-----------|----------------------------------------------|------------------------------------------------|
| **Dimas** | Project Lead & Full Stack Web Developer      | [GitHub](https://github.com/dimasgood691-byte) |
| **Andra** | Backend Web Developer                        | [GitHub](https://github.com/ridwanaulandra)    |
| **Azzam** | Full Stack Mobile Developer                  | [GitHub](https://github.com/Azzamwahyudin1)    |
| **Bima**  | Frontend Web Developer                       | [GitHub](https://github.com)
| **Catur** | Data Analyst                                 | [GitHub](https://github.com)
| **Asyraf**| UI & UX Desainer                             | [GitHub](https://github.com)

## [VERSI TECH STACK YANG DIGUNAKAN]
   - **.NET** : 10.0.401 [Backend]
   - **NEXT.JS** : 16.4.3 [FrontendWeb]
   - **Flutter** : 3.47.7 [MobileApp]

## [PROGRES PENGERJAAN]
### 1. Membuat Page Login Admin (Next.js)

Dalam pengerjaan halaman login di `page.tsx`, tidak ada library eksternal tambahan yang di-install di luar bawaan proyek. Seluruh fitur dan efek visual dibangun menggunakan tech stack yang sudah terdaftar di `package.json`:

1. **Library Utama (Bawaan Project)**
   - **React (react v19.3.0)**
     - Digunakan untuk hooks bawaan yaitu `useState` dan `useEffect` (mengelola input email/password, deteksi Caps Lock, fitur Ingat Email, serta melacak posisi koordinat kursor x, y untuk efek spotlight).
   - **Next.js (next v16.4.0 — modul next/navigation & next/font/google)**
     - Menggunakan `useRouter` dari `next/navigation` untuk mengarahkan pengguna (redirect) setelah login berhasil.
     - Menggunakan font Geist dan Geist_Mono dari `next/font/google` di `layout.tsx`.
   - **Tailwind CSS v4 (tailwindcss & @tailwindcss/turbopack)**
     - Digunakan untuk seluruh desain responsif (split-screen), efek glassmorphism (backdrop-blur), gradasi warna, shadow, dan transisi interaktif.

2. **Teknik Native (Tanpa Library Tambahan)**
   - **CSS Keyframes Murni (globals.css):** Animasi getar saat error (`animate-shake`), lencana melayang (`animate-float`), dan bola cahaya bergerak (`animate-blob`) ditulis menggunakan `@keyframes` CSS murni tanpa library animasi seperti Framer Motion.
   - **Inline SVG:** Seluruh ikon (logo, email, gembok, mata show/hide password, peringatan) menggunakan kode `<svg>` langsung di JSX, sehingga tidak perlu menginstal library ikon seperti lucide-react atau react-icons.
   - **Browser Web API (fetch & localStorage):** Menggunakan `fetch()` bawaan browser untuk memanggil endpoint REST API .NET (`/api/users/login`) dan `localStorage` untuk menyimpan sesi admin serta preferensi ingat email.

### 2. Arsitektur HTTP-Only Cookie, Server Actions & Middleware (Next.js)
Untuk meningkatkan keamanan sesi autentikasi di sisi *frontend* web (Next.js) dan mencegah serangan XSS (*Cross-Site Scripting*), penyimpanan token JWT kini dipindahkan dari `localStorage` ke **HTTP-Only Cookie**:
1. **Server Actions (`app/actions/auth.tsx`):**
   - `loginAdmin`: Menangani proses *login* dari sisi *server* Next.js ke *endpoint* `/api/users/login` (.NET), memvalidasi bahwa *role* pengguna termasuk staf operasional bengkel (`SuperAdmin`, `Kasir`, atau `Mekanik`), lalu menanamkan token JWT ke dalam *cookie* `admin_token` dan `user_role` dengan atribut `httpOnly: true`, `sameSite: "lax"`, dan masa aktif 3 jam.
   - `logoutAdmin`: Menghapus *cookie* `admin_token` dan `user_role` secara aman dari sisi *server* lalu mengarahkan pengguna kembali ke `/login`.
2. **Route Protection Middleware (`middleware.ts`):**
   - Memproteksi seluruh rute `/dashboard/:path*` dengan memvalidasi keberadaan token, mengekstrak klaim `role` langsung dari *payload* JWT, serta memeriksa masa berlaku token (`exp`).
   - Menerapkan pembatasan akses sub-rute berbasis *role* (misalnya halaman `/dashboard/users` hanya dapat diakses oleh `SuperAdmin`).
   - Mencegah staf yang sudah terautentikasi kembali ke halaman `/login` sebelum melakukan *logout*.


## Langkah Instalasi
#### [1] Clone Repository

```bash
git clone https://github.com/dimasgood691-byte/Auto-Care-Tracker.git
```

### [2] Menjalankan Frontend Web (NEXT.JS)

```bash
cd web_auto_care_tracker
```

Buat file bernama .env.local di dalam folder web_auto_care_tracker, lalu tambahkan baris konfigurasi berikut:

```
NEXT_PUBLIC_API_URL=http://localhost:5083/api
```

install seluruh library atau dependencies web :

```bash
npm install
```

jalankan server NEXT.JS :

```bash
npm run dev #frontend web berjalan di http://localhost:3000
```

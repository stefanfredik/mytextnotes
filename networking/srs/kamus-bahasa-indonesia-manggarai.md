---
icon: toggle-off
---

# Kamus Bahasa Indonesia - Manggarai

## Software Requirements Specification (SRS)

### Kamus Digital Bahasa Manggarai – Bahasa Indonesia

|                   |                                                                                          |
| ----------------- | ---------------------------------------------------------------------------------------- |
| **Versi Dokumen** | 1.1.0                                                                                    |
| **Tanggal**       | 2026-05-27                                                                               |
| **Status**        | Draft                                                                                    |
| **Changelog**     | v1.1.0 — Penambahan fitur Phrase Book & Parallel Corpus untuk fondasi terjemahan kalimat |

***

### Daftar Isi

1. [Pendahuluan](https://claude.ai/chat/54e2fa92-1b10-46c9-bd43-76671ba254b2#1-pendahuluan)
2. [Deskripsi Umum Sistem](https://claude.ai/chat/54e2fa92-1b10-46c9-bd43-76671ba254b2#2-deskripsi-umum-sistem)
3. [Aktor & Role](https://claude.ai/chat/54e2fa92-1b10-46c9-bd43-76671ba254b2#3-aktor--role)
4. [Functional Requirements](https://claude.ai/chat/54e2fa92-1b10-46c9-bd43-76671ba254b2#4-functional-requirements)
5. [Non-Functional Requirements](https://claude.ai/chat/54e2fa92-1b10-46c9-bd43-76671ba254b2#5-non-functional-requirements)
6. [Arsitektur Sistem](https://claude.ai/chat/54e2fa92-1b10-46c9-bd43-76671ba254b2#6-arsitektur-sistem)
7. [Arsitektur Backend (Golang)](https://claude.ai/chat/54e2fa92-1b10-46c9-bd43-76671ba254b2#7-arsitektur-backend-golang)
8. [Arsitektur Frontend (React)](https://claude.ai/chat/54e2fa92-1b10-46c9-bd43-76671ba254b2#8-arsitektur-frontend-react)
9. [Desain Database](https://claude.ai/chat/54e2fa92-1b10-46c9-bd43-76671ba254b2#9-desain-database)
10. [API Specification](https://claude.ai/chat/54e2fa92-1b10-46c9-bd43-76671ba254b2#10-api-specification)
11. [User Flow](https://claude.ai/chat/54e2fa92-1b10-46c9-bd43-76671ba254b2#11-user-flow)
12. [Deployment & Infrastructure](https://claude.ai/chat/54e2fa92-1b10-46c9-bd43-76671ba254b2#12-deployment--infrastructure)
13. [Roadmap Fitur Terjemahan Kalimat](https://claude.ai/chat/54e2fa92-1b10-46c9-bd43-76671ba254b2#13-roadmap-fitur-terjemahan-kalimat)
14. [Glossary](https://claude.ai/chat/54e2fa92-1b10-46c9-bd43-76671ba254b2#14-glossary)

***

### 1. Pendahuluan

#### 1.1 Tujuan Dokumen

Dokumen ini merupakan Software Requirements Specification (SRS) untuk platform **Kamus Digital Bahasa Manggarai – Bahasa Indonesia**. Dokumen ini mendefinisikan kebutuhan fungsional, non-fungsional, arsitektur teknis, desain database, dan panduan implementasi yang menjadi acuan seluruh tim pengembang.

#### 1.2 Ruang Lingkup

Platform ini adalah aplikasi web berbasis kamus dua arah (Bahasa Manggarai ↔ Bahasa Indonesia) yang:

* Mendukung multi-dialek Bahasa Manggarai dengan manajemen variasi kosakata antar dialek.
* Menyediakan akses publik tanpa autentikasi untuk pencarian dan penelusuran kamus.
* Memiliki sistem kontribusi berbasis komunitas dengan alur review dan approval yang terstruktur.
* Dibangun dengan prinsip SOLID, clean code, dan arsitektur yang scalable serta mudah dimaintain.
* Menyediakan fondasi database untuk fitur **Phrase Book** (kalimat statis per kategori) dan **Parallel Corpus** (korpus terjemahan kalimat) yang akan diaktifkan pada versi berikutnya.

#### 1.3 Definisi & Singkatan

| Istilah               | Definisi                                                                                             |
| --------------------- | ---------------------------------------------------------------------------------------------------- |
| **Kosakata**          | Satu entri kata dalam Bahasa Manggarai beserta seluruh atributnya                                    |
| **Dialek**            | Variasi regional dari Bahasa Manggarai                                                               |
| **Entry**             | Representasi kosakata dalam sistem                                                                   |
| **Submission**        | Kosakata baru yang diajukan oleh kontributor, menunggu review                                        |
| **Phrase Book**       | Koleksi kalimat umum yang sudah diterjemahkan, dikelompokkan per kategori                            |
| **Parallel Corpus**   | Kumpulan pasangan kalimat Manggarai–Indonesia sebagai fondasi mesin terjemahan                       |
| **Sentence Category** | Kategori pengelompokan kalimat pada Phrase Book (salam, arah, dll)                                   |
| **SRS**               | Software Requirements Specification                                                                  |
| **SSO**               | Single Sign-On                                                                                       |
| **JWT**               | JSON Web Token                                                                                       |
| **SOLID**             | Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion |
| **DDD**               | Domain-Driven Design                                                                                 |

#### 1.4 Referensi

* Go 1.23+ Documentation: https://go.dev/doc
* React 19+ Documentation: https://react.dev
* PostgreSQL 16 Documentation: https://www.postgresql.org/docs
* Meilisearch Documentation: https://www.meilisearch.com/docs

***

### 2. Deskripsi Umum Sistem

#### 2.1 Perspektif Produk

Platform kamus ini merupakan aplikasi web mandiri (standalone) yang dapat diakses melalui browser. Platform berfungsi sebagai repositori digital pelestarian Bahasa Manggarai yang dikelola secara kolaboratif oleh komunitas.

#### 2.2 Fitur Utama

| No   | Fitur                                                                | Prioritas        |
| ---- | -------------------------------------------------------------------- | ---------------- |
| F-01 | Pencarian kosakata dua arah (Manggarai ↔ Indonesia)                  | Wajib            |
| F-02 | Tampilan detail kosakata dengan variasi dialek                       | Wajib            |
| F-03 | Multi-dialek: satu kata bisa memiliki arti berbeda di dialek berbeda | Wajib            |
| F-04 | Contoh kalimat per kosakata                                          | Wajib            |
| F-05 | Relasi antar kosakata (sinonim, antonim, turunan, berkaitan)         | Wajib            |
| F-06 | Sistem kontribusi dengan alur review & approval                      | Wajib            |
| F-07 | Autentikasi via Google SSO untuk kontributor                         | Wajib            |
| F-08 | Role-based access control (Public, Contributor, Validator, Admin)    | Wajib            |
| F-09 | Dashboard admin untuk manajemen platform                             | Wajib            |
| F-10 | Filter pencarian berdasarkan dialek                                  | Wajib            |
| F-11 | Notifikasi status submission                                         | Wajib            |
| F-12 | Laporan kosakata yang tidak tepat oleh publik                        | Wajib            |
| F-13 | Smart suggestion kata terkait                                        | Akan datang      |
| F-14 | Audio pronunciation                                                  | Akan datang      |
| F-15 | Phrase Book — kamus kalimat umum per kategori & dialek               | Akan datang (v2) |
| F-16 | Parallel Corpus — korpus terjemahan kalimat bebas                    | Akan datang (v3) |

#### 2.3 Batasan Sistem (v1)

* Tidak ada fitur audio pronunciation pada versi pertama.
* Tidak ada fitur offline/PWA pada versi pertama.
* Tidak ada API publik untuk developer eksternal pada versi pertama.
* Jumlah dialek tidak dibatasi secara teknis, dapat dikonfigurasi oleh Admin.

***

### 3. Aktor & Role

#### 3.1 Hierarki Role

```
Admin
  └── Validator  (Contributor yang ditunjuk Admin)
        └── Contributor  (User yang telah login)
              └── Public User  (Tanpa autentikasi)
```

#### 3.2 Deskripsi Setiap Role

**Public User**

* Tidak perlu login.
* Dapat mengakses seluruh konten kamus yang telah dipublish.
* Dapat melaporkan kosakata yang tidak tepat.

**Contributor**

* Mendaftar via Google SSO, akun langsung aktif (open registration).
* Dapat submit kosakata baru; semua submission masuk antrian review.
* Dapat mengedit submission milik sendiri selama masih berstatus `pending`.
* Tidak dapat mereview submission orang lain.

**Validator**

* Contributor yang dipilih dan ditunjuk oleh Admin.
* Submission milik Validator sendiri langsung dipublish tanpa review (trusted contributor).
* Dapat mereview, menyetujui, menolak, atau merevisi langsung submission dari Contributor.
* Tidak dapat mereview submission miliknya sendiri jika ada kasus khusus yang di-eskalasi.

**Admin**

* Full control atas seluruh platform.
* Submission milik Admin langsung dipublish tanpa review.
* Dapat menunjuk atau mencabut status Validator dari Contributor.
* Dapat men-suspend akun Contributor.
* Dapat mengelola data Dialek (tambah, edit, hapus).
* Dapat mengakses analytics dan laporan platform.
* Dapat mereview dan mem-bypass semua submission secara langsung.

#### 3.3 Matriks Permission

| Aksi                                  | Public | Contributor |     Validator     |   Admin   |
| ------------------------------------- | :----: | :---------: | :---------------: | :-------: |
| Browse & search kamus                 |    ✅   |      ✅      |         ✅         |     ✅     |
| Lihat detail kosakata                 |    ✅   |      ✅      |         ✅         |     ✅     |
| Laporkan kosakata                     |    ✅   |      ✅      |         ✅         |     ✅     |
| Submit kosakata baru                  |    ❌   |      ✅      |         ✅         |     ✅     |
| Edit submission sendiri (pending)     |    ❌   |      ✅      |         ✅         |     ✅     |
| Review submission orang lain          |    ❌   |      ❌      |         ✅         |     ✅     |
| Revisi langsung submission orang lain |    ❌   |      ❌      |         ✅         |     ✅     |
| Approve / reject submission           |    ❌   |      ❌      |         ✅         |     ✅     |
| Bypass review (auto-publish)          |    ❌   |      ❌      | ✅ (milik sendiri) | ✅ (semua) |
| Assign / cabut Validator              |    ❌   |      ❌      |         ❌         |     ✅     |
| Suspend akun Contributor              |    ❌   |      ❌      |         ❌         |     ✅     |
| Kelola Dialek                         |    ❌   |      ❌      |         ❌         |     ✅     |
| Akses analytics                       |    ❌   |      ❌      |         ❌         |     ✅     |

***

### 4. Functional Requirements

#### 4.1 FR-01: Pencarian Kosakata

**FR-01.1** Sistem harus menyediakan search bar yang dapat diakses tanpa login pada halaman utama.

**FR-01.2** Pencarian mendukung dua arah yang dapat di-toggle oleh user:

* Mode **Manggarai → Indonesia**: input kata Manggarai, output terjemahan Indonesia.
* Mode **Indonesia → Manggarai**: input kata Indonesia, output padanan Manggarai beserta info dialek.

**FR-01.3** Pencarian bersifat _typo-tolerant_ (fuzzy search) untuk mengakomodasi variasi ejaan.

**FR-01.4** Hasil pencarian menampilkan: kata, arti ringkas, dan badge dialek yang tersedia.

**FR-01.5** Pencarian dapat difilter berdasarkan satu atau lebih dialek secara opsional.

**FR-01.6** Jika tidak ada hasil ditemukan, sistem menampilkan pesan informatif dan saran kata terdekat.

#### 4.2 FR-02: Detail Kosakata

**FR-02.1** Halaman detail menampilkan informasi lengkap sebuah kosakata:

* Kata (ejaan utama dan variasi ejaan per dialek jika ada).
* Kelas kata (nomina, verba, adjektiva, dll).
* Arti dalam Bahasa Indonesia per dialek.
* Catatan penggunaan atau konteks jika ada.
* Contoh kalimat beserta terjemahannya.
* Daftar dialek yang memiliki kata ini.
* Kosakata terkait (relasi).

**FR-02.2** Jika satu kata memiliki arti berbeda di dialek yang berbeda, sistem menampilkan setiap arti secara terpisah dengan label dialek yang jelas.

**FR-02.3** Jika sebuah kata tidak tersedia di dialek tertentu, hal tersebut ditampilkan secara eksplisit dalam antarmuka.

#### 4.3 FR-03: Relasi Kosakata

**FR-03.1** Setiap kosakata dapat memiliki relasi dengan kosakata lain.

**FR-03.2** Tipe relasi yang didukung: `sinonim`, `antonim`, `turunan`, `berkaitan`.

**FR-03.3** Halaman detail menampilkan kosakata terkait sebagai chip/card yang dapat diklik untuk navigasi ke detail kata terkait.

**FR-03.4** Kontributor dan Validator dapat menambahkan usulan relasi saat submit kosakata.

#### 4.4 FR-04: Sistem Kontribusi

**FR-04.1** Contributor yang telah login dapat mengakses form submit kosakata baru.

**FR-04.2** Form submission mencakup:

* Kata (ejaan utama).
* Variasi ejaan per dialek (opsional).
* Pilihan dialek (multi-select, minimal satu).
* Kelas kata.
* Arti dalam Bahasa Indonesia (dapat berbeda per dialek).
* Contoh kalimat (minimal satu, opsional per dialek).
* Relasi kata (opsional).
* Catatan tambahan (opsional).

**FR-04.3** Submission dari Contributor masuk dengan status `pending` dan menunggu review.

**FR-04.4** Submission dari Validator dan Admin langsung berstatus `published` tanpa melewati antrian review.

**FR-04.5** Contributor dapat melihat riwayat seluruh submissionnya beserta status masing-masing di dashboard.

**FR-04.6** Contributor dapat mengedit submission miliknya sendiri hanya selama masih berstatus `pending`.

#### 4.5 FR-05: Alur Review

**FR-05.1** Validator dan Admin dapat melihat daftar semua submission berstatus `pending`.

**FR-05.2** Reviewer dapat melakukan tiga aksi terhadap sebuah submission:

* **Approve**: submission dipublish dan tersedia di kamus publik.
* **Reject**: submission ditolak dengan catatan alasan wajib diisi.
* **Revisi langsung**: Validator/Admin mengedit konten submission secara langsung kemudian mempublishnya.

**FR-05.3** Sistem mengirimkan notifikasi in-app kepada Contributor setelah submissionnya diproses (approved, rejected, atau direvisi).

**FR-05.4** Jika submission direvisi langsung, Contributor menerima notifikasi yang menginformasikan bahwa kata mereka telah diedit sebelum dipublish.

**FR-05.5** Submission yang ditolak dapat dilihat Contributor dengan alasan penolakan, namun tidak dapat diedit ulang (harus submit baru).

#### 4.6 FR-06: Manajemen Admin

**FR-06.1** Admin dapat melihat daftar seluruh pengguna terdaftar beserta role mereka.

**FR-06.2** Admin dapat menunjuk Contributor menjadi Validator dengan toggle pada halaman manajemen user.

**FR-06.3** Admin dapat mencabut status Validator dari seorang user.

**FR-06.4** Admin dapat men-suspend akun Contributor sehingga user tersebut tidak dapat login dan tidak dapat submit.

**FR-06.5** Admin dapat mengelola data Dialek: menambah dialek baru, mengedit nama/deskripsi dialek, dan menonaktifkan dialek.

**FR-06.6** Admin dapat melihat laporan kosakata yang dilaporkan oleh publik dan mengambil tindakan (edit, hapus, atau abaikan laporan).

**FR-06.7** Admin memiliki halaman analytics yang menampilkan:

* Total kosakata published per dialek.
* Jumlah submission pending, approved, dan rejected.
* Aktivitas kontributor (top contributors).
* Pertumbuhan kosakata per bulan.

#### 4.7 FR-07: Autentikasi

**FR-07.1** Autentikasi dilakukan eksklusif via Google OAuth 2.0 (SSO).

**FR-07.2** Tidak ada form registrasi manual; pendaftaran sepenuhnya melalui Google SSO.

**FR-07.3** Saat pertama kali login, sistem otomatis membuat akun baru dengan role `contributor`.

**FR-07.4** Sesi autentikasi menggunakan JWT dengan refresh token untuk keamanan.

**FR-07.5** Admin dibuat secara manual melalui database seeder atau script CLI, tidak melalui UI.

#### 4.8 FR-08: Pelaporan

**FR-08.1** Setiap halaman detail kosakata memiliki tombol "Laporkan" yang dapat digunakan oleh semua user termasuk yang tidak login.

**FR-08.2** Form laporan meminta pengguna memilih alasan (ejaan salah, arti tidak tepat, contoh kalimat salah, konten tidak pantas, lainnya) dan mengisi deskripsi opsional.

**FR-08.3** Laporan masuk ke antrian Admin dan dapat ditindaklanjuti melalui dashboard.

***

### 5. Non-Functional Requirements

#### 5.1 Performance

| Metrik                                            | Target        |
| ------------------------------------------------- | ------------- |
| Waktu respons pencarian (p95)                     | < 300ms       |
| Waktu muat halaman utama (First Contentful Paint) | < 1.5 detik   |
| Throughput API minimal                            | 500 req/detik |
| Waktu respons API umum (p95)                      | < 500ms       |

#### 5.2 Scalability

* Backend didesain stateless untuk mendukung horizontal scaling.
* Database menggunakan connection pooling.
* Search engine (Meilisearch) dapat berjalan terpisah dari service utama.
* Cache layer (Redis) untuk pencarian populer dan session management.

#### 5.3 Security

* Seluruh komunikasi menggunakan HTTPS/TLS.
* JWT access token berumur pendek (15 menit), refresh token berumur 7 hari.
* Refresh token disimpan sebagai HTTP-only cookie.
* Rate limiting pada endpoint publik dan endpoint autentikasi.
* Input validation dan sanitasi pada semua endpoint.
* CORS dikonfigurasi secara ketat, hanya mengizinkan origin frontend resmi.
* Secret management menggunakan environment variables, tidak ada credential di codebase.

#### 5.4 Availability

* Target uptime: 99.5% per bulan.
* Health check endpoint untuk monitoring oleh Docker/orchestrator.
* Graceful shutdown untuk zero-downtime deployment.

#### 5.5 Maintainability

* Test coverage minimal 70% untuk business logic layer.
* Seluruh API terdokumentasi menggunakan OpenAPI 3.0 (Swagger).
* Kode mengikuti konvensi Go (gofmt, golint) dan ESLint untuk TypeScript/React.
* Database migration menggunakan tools versioning (golang-migrate).
* Structured logging (JSON format) untuk kemudahan parsing di log aggregator.

#### 5.6 Accessibility & UX

* Desain mengikuti prinsip WCAG 2.1 Level AA.
* Antarmuka responsif untuk desktop, tablet, dan mobile.
* Mendukung dark mode.
* Tampilan bersih (minimalis) dan mudah digunakan oleh semua kalangan usia.

***

### 6. Arsitektur Sistem

#### 6.1 Gambaran Umum

```
┌─────────────────────────────────────────────────────┐
│                   User / Browser                    │
└────────────────────────┬────────────────────────────┘
                         │ HTTPS
┌────────────────────────▼────────────────────────────┐
│              Nginx (Reverse Proxy)                  │
│         Static files + SSL termination              │
└──────────┬──────────────────────┬───────────────────┘
           │                      │
┌──────────▼──────────┐ ┌─────────▼──────────────────┐
│   Frontend (React)  │ │   Backend API (Golang)       │
│   Port 3000         │ │   Port 8080                  │
└─────────────────────┘ └────┬──────────┬─────────────┘
                             │          │
              ┌──────────────▼──┐  ┌────▼───────────┐
              │  PostgreSQL 16  │  │  Redis 7        │
              │  Port 5432      │  │  Port 6379      │
              └─────────────────┘  └────────────────┘
                                        │
                             ┌──────────▼─────────┐
                             │  Meilisearch        │
                             │  Port 7700          │
                             └────────────────────┘
```

#### 6.2 Stack Teknologi

| Layer                | Teknologi                  | Versi  |
| -------------------- | -------------------------- | ------ |
| Backend Language     | Go                         | 1.23+  |
| Backend Framework    | Fiber                      | v3     |
| ORM / Query Builder  | sqlc + pgx                 | latest |
| Auth                 | golang-jwt + Google OAuth2 | latest |
| Search Engine        | Meilisearch                | v1.x   |
| Cache / Session      | Redis (go-redis)           | v9     |
| Database             | PostgreSQL                 | 16     |
| Migration            | golang-migrate             | latest |
| Frontend Framework   | React + TypeScript         | 19+    |
| Build Tool           | Vite                       | 6+     |
| Styling              | Tailwind CSS               | v4     |
| UI Components        | shadcn/ui                  | latest |
| State Management     | Zustand                    | latest |
| Server State / Cache | TanStack Query             | v5     |
| HTTP Client          | Axios                      | latest |
| Router               | React Router               | v7     |
| Reverse Proxy        | Nginx                      | 1.25+  |
| Containerization     | Docker + Docker Compose    | latest |

***

### 7. Arsitektur Backend (Golang)

#### 7.1 Prinsip Desain

Backend dibangun menggunakan **Clean Architecture** yang dikombinasikan dengan prinsip **SOLID**:

* **Single Responsibility**: Setiap package/struct hanya memiliki satu alasan untuk berubah.
* **Open/Closed**: Menggunakan interface untuk mempermudah penambahan fitur tanpa mengubah kode yang ada.
* **Liskov Substitution**: Implementasi interface dapat saling menggantikan tanpa mengubah perilaku sistem.
* **Interface Segregation**: Interface kecil dan spesifik, tidak memaksa implementasi method yang tidak diperlukan.
* **Dependency Inversion**: Layer tinggi tidak bergantung pada layer rendah; keduanya bergantung pada abstraksi (interface).

#### 7.2 Struktur Direktori

```
backend/
├── cmd/
│   └── api/
│       └── main.go              # Entry point aplikasi
├── config/
│   ├── config.go                # Struct konfigurasi
│   └── loader.go                # Load dari env / file
├── internal/
│   ├── domain/                  # Layer domain (paling dalam, tidak ada dependency eksternal)
│   │   ├── entity/
│   │   │   ├── entry.go         # Entity Entry (kosakata)
│   │   │   ├── dialect.go       # Entity Dialect
│   │   │   ├── definition.go    # Entity Definition
│   │   │   ├── user.go          # Entity User
│   │   │   ├── submission.go    # Entity Submission
│   │   │   └── relation.go      # Entity WordRelation
│   │   ├── repository/          # Interface repository (abstraksi)
│   │   │   ├── entry_repository.go
│   │   │   ├── dialect_repository.go
│   │   │   ├── user_repository.go
│   │   │   └── submission_repository.go
│   │   └── service/             # Interface service (abstraksi use case)
│   │       ├── entry_service.go
│   │       ├── search_service.go
│   │       ├── submission_service.go
│   │       └── auth_service.go
│   ├── usecase/                 # Layer use case (business logic)
│   │   ├── entry_usecase.go
│   │   ├── search_usecase.go
│   │   ├── submission_usecase.go
│   │   ├── review_usecase.go
│   │   ├── auth_usecase.go
│   │   └── admin_usecase.go
│   ├── repository/              # Implementasi repository (infrastructure)
│   │   ├── postgres/
│   │   │   ├── entry_repo.go
│   │   │   ├── dialect_repo.go
│   │   │   ├── user_repo.go
│   │   │   └── submission_repo.go
│   │   └── redis/
│   │       └── cache_repo.go
│   ├── delivery/                # Layer delivery (HTTP handlers)
│   │   ├── http/
│   │   │   ├── handler/
│   │   │   │   ├── entry_handler.go
│   │   │   │   ├── search_handler.go
│   │   │   │   ├── submission_handler.go
│   │   │   │   ├── auth_handler.go
│   │   │   │   └── admin_handler.go
│   │   │   ├── middleware/
│   │   │   │   ├── auth_middleware.go
│   │   │   │   ├── role_middleware.go
│   │   │   │   ├── rate_limiter.go
│   │   │   │   └── cors.go
│   │   │   └── router.go
│   └── infrastructure/
│       ├── database/
│       │   ├── postgres.go      # Koneksi PostgreSQL
│       │   └── redis.go         # Koneksi Redis
│       ├── search/
│       │   └── meilisearch.go   # Koneksi & operasi Meilisearch
│       └── oauth/
│           └── google.go        # Google OAuth2 client
├── pkg/
│   ├── apperror/                # Custom error types
│   │   └── errors.go
│   ├── response/                # HTTP response helper
│   │   └── response.go
│   ├── validator/               # Input validator
│   │   └── validator.go
│   ├── logger/                  # Structured logger (zerolog)
│   │   └── logger.go
│   └── pagination/              # Pagination helper
│       └── pagination.go
├── db/
│   ├── migrations/              # SQL migration files
│   │   ├── 000001_create_users.up.sql
│   │   ├── 000001_create_users.down.sql
│   │   └── ...
│   ├── queries/                 # sqlc query files
│   │   ├── entries.sql
│   │   ├── dialects.sql
│   │   └── ...
│   └── sqlc.yaml                # sqlc configuration
├── docs/
│   └── swagger.yaml             # OpenAPI 3.0 spec
├── scripts/
│   └── seed.go                  # Database seeder
├── .env.example
├── Dockerfile
├── docker-compose.yml
└── go.mod
```

#### 7.3 Dependency Flow

```
Delivery (Handler) → UseCase → Domain (Interface)
                                     ↑
                          Repository (Implementation)
                          Infrastructure (DB, Cache, Search)
```

Setiap layer hanya mengetahui layer di bawahnya melalui interface, tidak pernah secara langsung.

#### 7.4 Contoh Implementasi Interface (Domain Layer)

```go
// internal/domain/repository/entry_repository.go

package repository

import (
    "context"
    "github.com/yourorg/kamus/internal/domain/entity"
)

type EntryRepository interface {
    FindByID(ctx context.Context, id string) (*entity.Entry, error)
    FindBySlug(ctx context.Context, slug string) (*entity.Entry, error)
    FindPublished(ctx context.Context, filter EntryFilter) ([]*entity.Entry, int64, error)
    Create(ctx context.Context, entry *entity.Entry) error
    Update(ctx context.Context, entry *entity.Entry) error
    Delete(ctx context.Context, id string) error
}

type EntryFilter struct {
    DialectIDs []string
    Page       int
    Limit      int
}
```

```go
// internal/domain/service/entry_service.go

package service

import (
    "context"
    "github.com/yourorg/kamus/internal/domain/entity"
)

type EntryService interface {
    GetEntryDetail(ctx context.Context, slug string) (*entity.EntryDetail, error)
    ListEntries(ctx context.Context, filter EntryFilter) (*entity.PaginatedEntries, error)
    CreateEntry(ctx context.Context, input CreateEntryInput, userID string) (*entity.Entry, error)
    UpdateEntry(ctx context.Context, id string, input UpdateEntryInput, userID string) (*entity.Entry, error)
}
```

#### 7.5 Alur Request

```
HTTP Request
    │
    ▼
Middleware (Auth, RateLimit, CORS)
    │
    ▼
Handler (Validation, Parse Request)
    │
    ▼
UseCase (Business Logic, Orchestration)
    │
    ├──▶ Repository (Database Operation via Interface)
    │
    ├──▶ Cache (Redis via Interface)
    │
    └──▶ Search (Meilisearch via Interface)
    │
    ▼
Response (Structured JSON)
```

#### 7.6 Error Handling

Menggunakan custom error type yang membawa HTTP status code dan kode error:

```go
// pkg/apperror/errors.go

type AppError struct {
    Code       string `json:"code"`
    Message    string `json:"message"`
    StatusCode int    `json:"-"`
}

var (
    ErrNotFound      = &AppError{Code: "NOT_FOUND", Message: "Resource not found", StatusCode: 404}
    ErrUnauthorized  = &AppError{Code: "UNAUTHORIZED", Message: "Authentication required", StatusCode: 401}
    ErrForbidden     = &AppError{Code: "FORBIDDEN", Message: "Access denied", StatusCode: 403}
    ErrBadRequest    = &AppError{Code: "BAD_REQUEST", Message: "Invalid request", StatusCode: 400}
    ErrConflict      = &AppError{Code: "CONFLICT", Message: "Resource already exists", StatusCode: 409}
    ErrInternal      = &AppError{Code: "INTERNAL_ERROR", Message: "Internal server error", StatusCode: 500}
)
```

#### 7.7 Submission Workflow Logic

```go
// internal/usecase/submission_usecase.go (pseudocode)

func (u *submissionUseCase) Submit(ctx context.Context, input SubmitInput, user *entity.User) (*entity.Submission, error) {
    submission := entity.NewSubmission(input, user.ID)

    // Validator dan Admin: langsung published, tidak masuk antrian
    if user.Role == entity.RoleValidator || user.Role == entity.RoleAdmin {
        submission.Status = entity.StatusPublished
        entry := submission.ToEntry()
        if err := u.entryRepo.Create(ctx, entry); err != nil {
            return nil, err
        }
        // Sync ke Meilisearch
        u.searchService.IndexEntry(ctx, entry)
        return submission, nil
    }

    // Contributor: masuk antrian pending
    submission.Status = entity.StatusPending
    if err := u.submissionRepo.Create(ctx, submission); err != nil {
        return nil, err
    }
    return submission, nil
}
```

***

### 8. Arsitektur Frontend (React)

#### 8.1 Prinsip Desain

* **Component-driven**: UI dibangun dari komponen kecil yang reusable dan composable.
* **Separation of Concerns**: Logika bisnis, state management, dan UI dipisahkan secara tegas.
* **Feature-based structure**: Kode diorganisir berdasarkan fitur/domain, bukan berdasarkan tipe file.
* **Type-safe**: TypeScript digunakan secara ketat di seluruh codebase.

#### 8.2 Struktur Direktori

```
frontend/
├── public/
│   └── favicon.ico
├── src/
│   ├── app/                     # Konfigurasi app level (router, providers)
│   │   ├── App.tsx
│   │   ├── router.tsx           # Route definitions
│   │   └── providers.tsx        # Context, QueryClient, dll
│   ├── features/                # Fitur-fitur utama (feature-based modules)
│   │   ├── dictionary/          # Fitur kamus (publik)
│   │   │   ├── components/
│   │   │   │   ├── SearchBar.tsx
│   │   │   │   ├── SearchResult.tsx
│   │   │   │   ├── EntryCard.tsx
│   │   │   │   ├── EntryDetail.tsx
│   │   │   │   ├── DialectBadge.tsx
│   │   │   │   ├── RelatedWords.tsx
│   │   │   │   └── ReportButton.tsx
│   │   │   ├── hooks/
│   │   │   │   ├── useSearch.ts
│   │   │   │   └── useEntryDetail.ts
│   │   │   ├── api/
│   │   │   │   └── dictionaryApi.ts
│   │   │   ├── types/
│   │   │   │   └── dictionary.types.ts
│   │   │   └── index.ts         # Public API of this feature
│   │   ├── auth/                # Fitur autentikasi
│   │   │   ├── components/
│   │   │   │   └── LoginButton.tsx
│   │   │   ├── hooks/
│   │   │   │   └── useAuth.ts
│   │   │   ├── store/
│   │   │   │   └── authStore.ts # Zustand store
│   │   │   ├── api/
│   │   │   │   └── authApi.ts
│   │   │   └── index.ts
│   │   ├── contribution/        # Fitur submit kosakata
│   │   │   ├── components/
│   │   │   │   ├── SubmissionForm.tsx
│   │   │   │   ├── SubmissionList.tsx
│   │   │   │   └── SubmissionStatus.tsx
│   │   │   ├── hooks/
│   │   │   │   └── useSubmission.ts
│   │   │   ├── api/
│   │   │   │   └── contributionApi.ts
│   │   │   └── index.ts
│   │   ├── review/              # Fitur review (validator)
│   │   │   ├── components/
│   │   │   │   ├── ReviewQueue.tsx
│   │   │   │   ├── ReviewForm.tsx
│   │   │   │   └── ReviewActions.tsx
│   │   │   ├── hooks/
│   │   │   │   └── useReview.ts
│   │   │   └── index.ts
│   │   └── admin/               # Fitur admin
│   │       ├── components/
│   │       │   ├── UserManagement.tsx
│   │       │   ├── DialectManagement.tsx
│   │       │   ├── AnalyticsDashboard.tsx
│   │       │   └── ReportManagement.tsx
│   │       ├── hooks/
│   │       │   └── useAdmin.ts
│   │       └── index.ts
│   ├── shared/                  # Komponen, hooks, utils yang dipakai banyak fitur
│   │   ├── components/
│   │   │   ├── ui/              # shadcn/ui components (auto-generated)
│   │   │   ├── layout/
│   │   │   │   ├── Header.tsx
│   │   │   │   ├── Footer.tsx
│   │   │   │   └── PageLayout.tsx
│   │   │   ├── ProtectedRoute.tsx
│   │   │   ├── Pagination.tsx
│   │   │   └── EmptyState.tsx
│   │   ├── hooks/
│   │   │   ├── useDebounce.ts
│   │   │   └── useNotification.ts
│   │   └── utils/
│   │       ├── cn.ts            # Tailwind className merger
│   │       └── formatters.ts
│   ├── lib/
│   │   ├── axios.ts             # Axios instance + interceptors
│   │   └── queryClient.ts       # TanStack Query config
│   └── types/
│       └── api.types.ts         # Global API response types
├── index.html
├── vite.config.ts
├── tailwind.config.ts
├── tsconfig.json
└── package.json
```

#### 8.3 State Management Strategy

| Jenis State                  | Tool                        | Contoh                                          |
| ---------------------------- | --------------------------- | ----------------------------------------------- |
| Server state (data dari API) | TanStack Query              | Hasil pencarian, detail kata, daftar submission |
| Client/UI state global       | Zustand                     | Data user yang login, toggle dark mode          |
| Local component state        | React useState / useReducer | Form input, toggle modal                        |

#### 8.4 Routing Structure

```
/                           → Halaman utama (search)
/kata/:slug                 → Detail kosakata
/dialek/:dialect            → Browse per dialek

/auth/callback              → Google OAuth callback

/dashboard                  → Dashboard Contributor (protected)
/dashboard/submit           → Form submit kosakata (protected: contributor+)
/dashboard/submissions      → Riwayat submission (protected: contributor+)

/validator                  → Antrian review (protected: validator+)
/validator/:id              → Detail review submission (protected: validator+)

/admin                      → Admin dashboard (protected: admin)
/admin/users                → Manajemen user (protected: admin)
/admin/dialects             → Manajemen dialek (protected: admin)
/admin/reports              → Laporan kosakata (protected: admin)
/admin/analytics            → Analytics (protected: admin)
```

#### 8.5 UI/UX Guidelines

* **Tipografi**: Font Inter (sistem), hierarki jelas antara heading, body, dan caption.
* **Warna**: Palet netral dengan satu accent color (hijau toska atau biru slate) sebagai identitas.
* **Spacing**: Menggunakan spacing scale Tailwind yang konsisten (4px base unit).
* **Komponen**: Menggunakan shadcn/ui sebagai komponen dasar agar konsisten dan accessible.
* **Search Experience**: Instant search dengan debounce 300ms, hasil muncul di bawah search bar tanpa full page reload.
* **Mobile First**: Semua halaman didesain mobile-first, kemudian di-enhance untuk layar besar.

***

### 9. Desain Database

#### 9.1 Pertimbangan Desain

Database dirancang untuk mengakomodasi kompleksitas utama sistem:

1. **Satu kata bisa ada di banyak dialek** dengan arti yang berbeda per dialek.
2. **Satu kata bisa tidak tersedia** di dialek tertentu.
3. **Pencarian dua arah** (Manggarai → Indonesia dan sebaliknya).
4. **Riwayat submission dan review** harus tersimpan untuk audit trail.
5. **Relasi antar kata** bersifat many-to-many dan memiliki tipe.

#### 9.2 ERD (Entity Relationship)

```
users
  │
  ├── (created_by) ──────────────────────────────────── entries
  │                                                        │
  │                                                        ├── entry_dialects ──── dialects
  │                                                        │        │
  │                                                        │        └── definitions
  │                                                        │                 │
  │                                                        │                 └── example_sentences
  │                                                        │
  │                                                        └── word_relations (self-join)
  │
  └── (submitted_by) ────────────────────────────────── submissions
            │                                                  │
            └── (reviewed_by) ────────────────────────────────┘
```

#### 9.3 Skema Tabel Lengkap

```sql
-- ============================================================
-- USERS
-- ============================================================
CREATE TABLE users (
    id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    google_id     VARCHAR(255) UNIQUE NOT NULL,
    email         VARCHAR(255) UNIQUE NOT NULL,
    name          VARCHAR(255) NOT NULL,
    avatar_url    TEXT,
    role          VARCHAR(20) NOT NULL DEFAULT 'contributor'
                  CHECK (role IN ('admin', 'validator', 'contributor')),
    is_suspended  BOOLEAN NOT NULL DEFAULT FALSE,
    created_at    TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at    TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_google_id ON users(google_id);
CREATE INDEX idx_users_role ON users(role);

-- ============================================================
-- DIALECTS
-- ============================================================
CREATE TABLE dialects (
    id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name        VARCHAR(100) UNIQUE NOT NULL,
    slug        VARCHAR(100) UNIQUE NOT NULL,
    description TEXT,
    region      VARCHAR(255),
    is_active   BOOLEAN NOT NULL DEFAULT TRUE,
    sort_order  INTEGER NOT NULL DEFAULT 0,
    created_at  TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at  TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_dialects_slug ON dialects(slug);
CREATE INDEX idx_dialects_is_active ON dialects(is_active);

-- ============================================================
-- ENTRIES (Kosakata Bahasa Manggarai)
-- ============================================================
CREATE TABLE entries (
    id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    base_form       VARCHAR(255) NOT NULL,       -- Ejaan utama/standar
    slug            VARCHAR(255) UNIQUE NOT NULL, -- URL-friendly identifier
    part_of_speech  VARCHAR(50),                  -- nomina, verba, adjektiva, dll
    notes           TEXT,                          -- Catatan umum
    status          VARCHAR(20) NOT NULL DEFAULT 'published'
                    CHECK (status IN ('published', 'archived')),
    created_by      UUID NOT NULL REFERENCES users(id),
    created_at      TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at      TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_entries_slug ON entries(slug);
CREATE INDEX idx_entries_base_form ON entries(base_form);
CREATE INDEX idx_entries_status ON entries(status);
CREATE INDEX idx_entries_created_by ON entries(created_by);

-- ============================================================
-- ENTRY_DIALECTS (Hubungan Entry dengan Dialek)
-- Satu entry bisa hadir di banyak dialek, dan bisa memiliki
-- ejaan lokal yang berbeda di masing-masing dialek.
-- ============================================================
CREATE TABLE entry_dialects (
    id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    entry_id        UUID NOT NULL REFERENCES entries(id) ON DELETE CASCADE,
    dialect_id      UUID NOT NULL REFERENCES dialects(id),
    local_spelling  VARCHAR(255),   -- Ejaan lokal jika berbeda dari base_form
    is_available    BOOLEAN NOT NULL DEFAULT TRUE, -- FALSE jika kata tidak ada di dialek ini
    created_at      TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at      TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    UNIQUE(entry_id, dialect_id)
);

CREATE INDEX idx_entry_dialects_entry_id ON entry_dialects(entry_id);
CREATE INDEX idx_entry_dialects_dialect_id ON entry_dialects(dialect_id);

-- ============================================================
-- DEFINITIONS (Arti per EntryDialect)
-- Satu entry_dialect bisa memiliki lebih dari satu arti
-- (homonim atau makna ganda).
-- ============================================================
CREATE TABLE definitions (
    id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    entry_dialect_id UUID NOT NULL REFERENCES entry_dialects(id) ON DELETE CASCADE,
    meaning         TEXT NOT NULL,              -- Arti dalam Bahasa Indonesia
    context_notes   TEXT,                       -- Catatan konteks penggunaan
    sort_order      INTEGER NOT NULL DEFAULT 0,
    created_at      TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at      TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_definitions_entry_dialect_id ON definitions(entry_dialect_id);

-- ============================================================
-- EXAMPLE_SENTENCES (Contoh Kalimat)
-- Terhubung ke definition (arti spesifik).
-- ============================================================
CREATE TABLE example_sentences (
    id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    definition_id   UUID NOT NULL REFERENCES definitions(id) ON DELETE CASCADE,
    sentence_source TEXT NOT NULL,  -- Kalimat dalam Bahasa Manggarai
    sentence_translation TEXT NOT NULL, -- Terjemahan dalam Bahasa Indonesia
    created_at      TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at      TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_example_sentences_definition_id ON example_sentences(definition_id);

-- ============================================================
-- WORD_RELATIONS (Relasi Antar Kosakata)
-- Self-referencing many-to-many untuk relasi semantik.
-- ============================================================
CREATE TABLE word_relations (
    id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    from_entry_id   UUID NOT NULL REFERENCES entries(id) ON DELETE CASCADE,
    to_entry_id     UUID NOT NULL REFERENCES entries(id) ON DELETE CASCADE,
    relation_type   VARCHAR(30) NOT NULL
                    CHECK (relation_type IN ('sinonim', 'antonim', 'turunan', 'berkaitan')),
    created_by      UUID NOT NULL REFERENCES users(id),
    created_at      TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    UNIQUE(from_entry_id, to_entry_id, relation_type),
    CHECK (from_entry_id <> to_entry_id) -- tidak boleh relasi dengan dirinya sendiri
);

CREATE INDEX idx_word_relations_from ON word_relations(from_entry_id);
CREATE INDEX idx_word_relations_to ON word_relations(to_entry_id);

-- ============================================================
-- SUBMISSIONS (Antrian Submission Kosakata Baru)
-- Hanya untuk submission dari Contributor.
-- Validator & Admin langsung insert ke entries.
-- ============================================================
CREATE TABLE submissions (
    id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    submitted_by    UUID NOT NULL REFERENCES users(id),
    status          VARCHAR(20) NOT NULL DEFAULT 'pending'
                    CHECK (status IN ('pending', 'approved', 'rejected')),

    -- Payload: data yang disubmit, disimpan sebagai JSONB
    -- agar fleksibel dan tidak perlu tabel submission_dialects terpisah
    payload         JSONB NOT NULL,

    -- Review info
    reviewed_by     UUID REFERENCES users(id),
    reviewed_at     TIMESTAMPTZ,
    review_notes    TEXT,             -- Catatan jika rejected atau direvisi
    was_edited      BOOLEAN NOT NULL DEFAULT FALSE, -- TRUE jika validator merevisi sebelum publish

    -- Jika approved, entry yang dihasilkan
    resulting_entry_id UUID REFERENCES entries(id),

    created_at      TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at      TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_submissions_submitted_by ON submissions(submitted_by);
CREATE INDEX idx_submissions_status ON submissions(status);
CREATE INDEX idx_submissions_reviewed_by ON submissions(reviewed_by);

-- ============================================================
-- REPORTS (Laporan Kosakata oleh User)
-- ============================================================
CREATE TABLE reports (
    id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    entry_id        UUID NOT NULL REFERENCES entries(id) ON DELETE CASCADE,
    reported_by     UUID REFERENCES users(id), -- NULL jika anonymous
    reason          VARCHAR(50) NOT NULL
                    CHECK (reason IN ('ejaan_salah', 'arti_tidak_tepat',
                                      'contoh_salah', 'konten_tidak_pantas', 'lainnya')),
    description     TEXT,
    status          VARCHAR(20) NOT NULL DEFAULT 'open'
                    CHECK (status IN ('open', 'resolved', 'dismissed')),
    resolved_by     UUID REFERENCES users(id),
    resolved_at     TIMESTAMPTZ,
    created_at      TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_reports_entry_id ON reports(entry_id);
CREATE INDEX idx_reports_status ON reports(status);

-- ============================================================
-- REFRESH_TOKENS (JWT Refresh Token Store)
-- ============================================================
CREATE TABLE refresh_tokens (
    id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id     UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    token_hash  VARCHAR(255) UNIQUE NOT NULL, -- SHA-256 hash dari token
    expires_at  TIMESTAMPTZ NOT NULL,
    revoked     BOOLEAN NOT NULL DEFAULT FALSE,
    created_at  TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_refresh_tokens_user_id ON refresh_tokens(user_id);
CREATE INDEX idx_refresh_tokens_token_hash ON refresh_tokens(token_hash);

-- ============================================================
-- NOTIFICATIONS
-- ============================================================
CREATE TABLE notifications (
    id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id     UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    type        VARCHAR(50) NOT NULL,
                -- 'submission_approved', 'submission_rejected',
                -- 'submission_edited_then_published'
    payload     JSONB NOT NULL,  -- Data tambahan (submission id, kata, catatan)
    is_read     BOOLEAN NOT NULL DEFAULT FALSE,
    created_at  TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_notifications_user_id ON notifications(user_id);
CREATE INDEX idx_notifications_is_read ON notifications(is_read);
```

#### 9.4 Contoh Data: Skenario Multi-Dialek

Kata "makan" dalam Bahasa Manggarai dengan dua dialek memiliki kata berbeda:

```
entries
  └── id: "abc-123", base_form: "elong", slug: "elong"

entry_dialects
  ├── entry_id: "abc-123", dialect_id: "dialek-manggarai-tengah", local_spelling: "elong"
  └── entry_id: "abc-123", dialect_id: "dialek-manggarai-timur",  local_spelling: "ngelong"

definitions
  ├── entry_dialect_id: (manggarai-tengah), meaning: "makan"
  └── entry_dialect_id: (manggarai-timur),  meaning: "makan (lebih formal)"
```

#### 9.5 Meilisearch Index Schema

Dokumen yang diindeks ke Meilisearch untuk pencarian cepat:

```json
{
  "id": "abc-123",
  "base_form": "elong",
  "slug": "elong",
  "part_of_speech": "verba",
  "dialects": ["Manggarai Tengah", "Manggarai Timur"],
  "dialect_spellings": ["elong", "ngelong"],
  "meanings": ["makan", "makan (lebih formal)"],
  "example_sentences": ["Aku elong hang nasi", "Hau ngelong wa?"],
  "related_words": ["hang", "minum"],
  "updated_at": "2026-05-27T00:00:00Z"
}
```

Index settings:

* **Searchable attributes**: `base_form`, `dialect_spellings`, `meanings`, `example_sentences`
* **Filterable attributes**: `dialects`, `part_of_speech`
* **Sortable attributes**: `base_form`, `updated_at`
* **Typo tolerance**: enabled (max 2 typos untuk kata > 8 karakter)

***

### 10. API Specification

#### 10.1 Base URL & Format

```
Base URL    : https://api.kamus-manggarai.id/api/v1
Content-Type: application/json
Auth Header : Authorization: Bearer <access_token>
```

#### 10.2 Standar Response

**Success:**

```json
{
  "success": true,
  "data": { ... },
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 150
  }
}
```

**Error:**

```json
{
  "success": false,
  "error": {
    "code": "NOT_FOUND",
    "message": "Kosakata tidak ditemukan"
  }
}
```

#### 10.3 Endpoint List

**Auth**

| Method | Endpoint                | Auth | Deskripsi                     |
| ------ | ----------------------- | ---- | ----------------------------- |
| GET    | `/auth/google`          | -    | Redirect ke Google OAuth      |
| GET    | `/auth/google/callback` | -    | Callback handler Google OAuth |
| POST   | `/auth/refresh`         | -    | Refresh access token          |
| POST   | `/auth/logout`          | ✅    | Revoke refresh token          |
| GET    | `/auth/me`              | ✅    | Get current user info         |

**Dictionary (Public)**

| Method | Endpoint                 | Auth | Deskripsi                       |
| ------ | ------------------------ | ---- | ------------------------------- |
| GET    | `/entries`               | -    | List semua kosakata (paginated) |
| GET    | `/entries/:slug`         | -    | Detail satu kosakata            |
| GET    | `/search`                | -    | Pencarian kosakata              |
| GET    | `/dialects`              | -    | List semua dialek aktif         |
| POST   | `/entries/:slug/reports` | -    | Laporkan kosakata               |

**Query params `/search`:**

* `q` (string, required): kata yang dicari
* `direction` (string): `manggarai_to_indonesia` atau `indonesia_to_manggarai` (default: `manggarai_to_indonesia`)
* `dialect_ids` (array of UUID, optional): filter per dialek
* `page` (int): halaman (default: 1)
* `limit` (int): jumlah per halaman (default: 20, max: 50)

**Contribution (Contributor+)**

| Method | Endpoint                  | Auth           | Deskripsi                              |
| ------ | ------------------------- | -------------- | -------------------------------------- |
| POST   | `/submissions`            | ✅ Contributor+ | Submit kosakata baru                   |
| GET    | `/submissions`            | ✅ Contributor+ | List submission milik sendiri          |
| GET    | `/submissions/:id`        | ✅ Contributor+ | Detail submission                      |
| PUT    | `/submissions/:id`        | ✅ Contributor+ | Edit submission (hanya status pending) |
| GET    | `/notifications`          | ✅ Contributor+ | List notifikasi                        |
| PATCH  | `/notifications/:id/read` | ✅ Contributor+ | Tandai notifikasi sudah dibaca         |

**Review (Validator+)**

| Method | Endpoint                    | Auth         | Deskripsi                        |
| ------ | --------------------------- | ------------ | -------------------------------- |
| GET    | `/review/queue`             | ✅ Validator+ | List semua submission pending    |
| GET    | `/review/queue/:id`         | ✅ Validator+ | Detail submission untuk direview |
| POST   | `/review/queue/:id/approve` | ✅ Validator+ | Approve submission               |
| POST   | `/review/queue/:id/reject`  | ✅ Validator+ | Reject submission                |
| PUT    | `/review/queue/:id/revise`  | ✅ Validator+ | Revisi langsung dan publish      |

**Admin**

| Method | Endpoint                            | Auth    | Deskripsi                             |
| ------ | ----------------------------------- | ------- | ------------------------------------- |
| GET    | `/admin/users`                      | ✅ Admin | List semua user                       |
| PATCH  | `/admin/users/:id/toggle-validator` | ✅ Admin | Assign/cabut role validator           |
| PATCH  | `/admin/users/:id/toggle-suspend`   | ✅ Admin | Suspend/unsuspend user                |
| GET    | `/admin/dialects`                   | ✅ Admin | List semua dialek (termasuk nonaktif) |
| POST   | `/admin/dialects`                   | ✅ Admin | Tambah dialek baru                    |
| PUT    | `/admin/dialects/:id`               | ✅ Admin | Edit dialek                           |
| PATCH  | `/admin/dialects/:id/toggle-active` | ✅ Admin | Aktifkan/nonaktifkan dialek           |
| GET    | `/admin/reports`                    | ✅ Admin | List laporan kosakata                 |
| PATCH  | `/admin/reports/:id`                | ✅ Admin | Tindak lanjut laporan                 |
| GET    | `/admin/analytics`                  | ✅ Admin | Data analytics platform               |

***

### 11. User Flow

#### 11.1 Public User — Pencarian Kosakata

```
Buka website
    └──▶ Halaman utama (search bar aktif, tidak perlu login)
              └──▶ Input kata → hasil muncul instan (debounce 300ms)
                        └──▶ Klik hasil → Halaman detail kosakata
                                  ├── Lihat arti per dialek
                                  ├── Lihat contoh kalimat
                                  ├── Klik kata terkait → navigasi ke detail kata terkait
                                  └── Klik "Laporkan" → Isi form laporan
```

#### 11.2 Contributor — Submit Kosakata

```
Klik "Daftar sebagai Kontributor"
    └──▶ Login via Google SSO → akun otomatis dibuat (role: contributor)
              └──▶ Dashboard Contributor
                        └──▶ Klik "Submit Kata Baru"
                                  └──▶ Isi form submission
                                            └──▶ Submit → status: pending
                                                      ├── Menunggu review Validator/Admin
                                                      ├── Terima notifikasi saat diproses
                                                      │     ├── Approved → kata published
                                                      │     ├── Rejected → lihat alasan
                                                      │     └── Direvisi → notifikasi + kata published
                                                      └── (Jika pending) Bisa edit submission
```

#### 11.3 Validator — Review Submission

```
Login via Google SSO
    └──▶ Dashboard Validator (antrian submission pending)
              └──▶ Buka detail submission
                        └──▶ Pilih aksi:
                                  ├── Approve → kata langsung published
                                  ├── Reject → isi alasan wajib → kirim notifikasi ke contributor
                                  └── Revisi → edit konten submission → publish → notifikasi ke contributor
```

#### 11.4 Validator — Submit Kosakata Sendiri

```
Login via Google SSO
    └──▶ Klik "Submit Kata Baru"
              └──▶ Isi form submission
                        └──▶ Submit → langsung published (auto-approved, trusted contributor)
```

#### 11.5 Admin — Manajemen Platform

```
Login via Google SSO
    └──▶ Admin Dashboard
              ├──▶ Kelola User → assign/cabut validator, suspend akun
              ├──▶ Kelola Dialek → tambah, edit, nonaktifkan
              ├──▶ Review Submission → approve/reject/revisi (bypass semua antrian)
              ├──▶ Kelola Laporan → tindaklanjuti laporan kosakata dari publik
              └──▶ Analytics → statistik platform
```

***

### 12. Deployment & Infrastructure

#### 12.1 Docker Compose Setup

```yaml
# docker-compose.yml
version: '3.9'

services:
  nginx:
    image: nginx:1.25-alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./nginx/ssl:/etc/nginx/ssl:ro
    depends_on:
      - backend
      - frontend
    restart: unless-stopped

  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    expose:
      - "3000"
    restart: unless-stopped

  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    expose:
      - "8080"
    env_file:
      - ./backend/.env
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_healthy
      meilisearch:
        condition: service_started
    restart: unless-stopped

  postgres:
    image: postgres:16-alpine
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: kamus_manggarai
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    expose:
      - "5432"
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER}"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: unless-stopped

  redis:
    image: redis:7-alpine
    command: redis-server --requirepass ${REDIS_PASSWORD}
    volumes:
      - redis_data:/data
    expose:
      - "6379"
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: unless-stopped

  meilisearch:
    image: getmeili/meilisearch:v1.7
    volumes:
      - meilisearch_data:/meili_data
    environment:
      MEILI_MASTER_KEY: ${MEILI_MASTER_KEY}
    expose:
      - "7700"
    restart: unless-stopped

volumes:
  postgres_data:
  redis_data:
  meilisearch_data:
```

#### 12.2 Environment Variables

```env
# backend/.env.example

# App
APP_ENV=production
APP_PORT=8080
APP_URL=https://kamus-manggarai.id
FRONTEND_URL=https://kamus-manggarai.id

# Database
DB_HOST=postgres
DB_PORT=5432
DB_NAME=kamus_manggarai
DB_USER=kamus_user
DB_PASSWORD=secret
DB_MAX_OPEN_CONNS=25
DB_MAX_IDLE_CONNS=10

# Redis
REDIS_HOST=redis
REDIS_PORT=6379
REDIS_PASSWORD=secret

# Meilisearch
MEILI_HOST=http://meilisearch:7700
MEILI_MASTER_KEY=secret

# JWT
JWT_ACCESS_SECRET=secret
JWT_ACCESS_EXPIRY=15m
JWT_REFRESH_EXPIRY=168h

# Google OAuth
GOOGLE_CLIENT_ID=your-client-id
GOOGLE_CLIENT_SECRET=your-client-secret
GOOGLE_REDIRECT_URL=https://api.kamus-manggarai.id/api/v1/auth/google/callback
```

#### 12.3 Dockerfile Backend (Multi-stage)

```dockerfile
# backend/Dockerfile
FROM golang:1.23-alpine AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o /app/bin/api ./cmd/api

FROM alpine:3.20
RUN apk add --no-cache ca-certificates tzdata
WORKDIR /app
COPY --from=builder /app/bin/api .
COPY --from=builder /app/db/migrations ./db/migrations
EXPOSE 8080
HEALTHCHECK --interval=30s --timeout=3s CMD wget -qO- http://localhost:8080/health || exit 1
CMD ["./api"]
```

#### 12.4 Strategi Backup

* Database PostgreSQL: `pg_dump` terjadwal setiap hari, disimpan ke direktori backup lokal atau object storage.
* Volume Docker (`postgres_data`, `redis_data`, `meilisearch_data`): backup reguler menggunakan script cron.
* Meilisearch data dapat di-rebuild dari PostgreSQL kapan saja dengan menjalankan indexer ulang.

***

### 13. Glossary

| Term                     | Penjelasan                                                                                                                           |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| **Base form**            | Ejaan utama/standar dari sebuah kosakata Manggarai                                                                                   |
| **Local spelling**       | Ejaan alternatif yang digunakan di dialek tertentu                                                                                   |
| **Entry**                | Satu entri kosakata Bahasa Manggarai dalam database                                                                                  |
| **Entry dialect**        | Representasi sebuah entry dalam konteks dialek tertentu                                                                              |
| **Definition**           | Satu arti/makna dari sebuah entry\_dialect dalam Bahasa Indonesia                                                                    |
| **Submission**           | Kosakata yang diajukan oleh Contributor, menunggu review                                                                             |
| **Payload (submission)** | Data JSON yang berisi seluruh informasi kata yang disubmit                                                                           |
| **Published**            | Status kosakata yang sudah tersedia di kamus publik                                                                                  |
| **Pending**              | Status submission yang belum direview                                                                                                |
| **Trusted contributor**  | Sebutan informal untuk Validator yang submissionnya auto-published                                                                   |
| **Auto-approved**        | Proses di mana submission langsung published tanpa review manual                                                                     |
| **Typo-tolerant search** | Pencarian yang masih mengembalikan hasil meskipun ada kesalahan ketik                                                                |
| **Slug**                 | Versi URL-friendly dari sebuah kata (contoh: "elong-verba")                                                                          |
| **SOLID**                | Prinsip desain perangkat lunak: Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion |
| **Clean Architecture**   | Pola arsitektur yang memisahkan concern ke dalam lapisan dengan aturan dependency yang ketat                                         |

***

_Dokumen ini bersifat living document dan akan diperbarui seiring perkembangan proyek._

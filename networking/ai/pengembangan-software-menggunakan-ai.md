---
icon: brain
---

# Pengembangan Software Menggunakan AI

## 📘 Panduan Mendalam: Menggunakan AI Agent untuk Pengembangan Software

***

### DAFTAR ISI

1. Filosofi & Mindset Fundamental
2. Persiapan Sebelum Prompting
3. Anatomi Prompt yang Ideal
4. Framework Step-by-Step Eksekusi
5. Pola Prompt untuk Setiap Fase SDLC
6. Strategi Iterasi & Refinement
7. Anti-Pattern & Kesalahan Umum
8. Template Prompt Siap Pakai
9. Studi Kasus End-to-End

***

### 1. Filosofi & Mindset Fundamental

#### 🧠 Prinsip Utama

```
AI Agent BUKAN pengganti developer.
AI Agent ADALAH pair programmer yang sangat cepat tapi TIDAK memiliki konteks
kecuali yang ANDA berikan.
```

#### Tiga Hukum Prompting untuk Software Development

```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│  HUKUM #1: GARBAGE IN = GARBAGE OUT                     │
│  → Kualitas output BERBANDING LURUS dengan              │
│    kualitas instruksi yang diberikan                     │
│                                                         │
│  HUKUM #2: DIVIDE & CONQUER                             │
│  → Pecah masalah besar menjadi unit kecil               │
│    yang bisa dieksekusi satu per satu                    │
│                                                         │
│  HUKUM #3: VERIFY, DON'T TRUST                          │
│  → Selalu validasi output AI sebelum                    │
│    mengintegrasikannya ke codebase                       │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

#### Mental Model: Anda sebagai "Arsitek", AI sebagai "Builder"

```
ANDA bertanggung jawab untuk:          AI bertanggung jawab untuk:
├── Keputusan arsitektur               ├── Menulis boilerplate code
├── Business logic & requirements      ├── Implementasi detail teknis
├── Review & validasi                  ├── Refactoring & optimasi
├── Konteks domain                     ├── Menulis test cases
└── Quality assurance final            └── Dokumentasi & penjelasan
```

***

### 2. Persiapan Sebelum Prompting

#### 📋 Checklist Pra-Prompting

Sebelum menulis satu baris prompt pun, siapkan:

```
FASE PERSIAPAN
══════════════

□ STEP 1: Definisikan Project Scope
  ├── Apa masalah yang ingin diselesaikan?
  ├── Siapa target user?
  ├── Fitur inti (MVP) vs nice-to-have?
  └── Constraint (waktu, budget, teknologi)?

□ STEP 2: Tentukan Tech Stack
  ├── Bahasa pemrograman
  ├── Framework & library
  ├── Database
  ├── Infrastructure/deployment
  └── Third-party services

□ STEP 3: Siapkan Konteks Dokumen
  ├── ERD / Database schema
  ├── API specification
  ├── Wireframe / mockup
  ├── User stories / requirements
  └── Existing codebase (jika ada)

□ STEP 4: Buat Rencana Dekomposisi
  ├── Bagi project menjadi modul
  ├── Bagi modul menjadi fitur
  ├── Bagi fitur menjadi task
  └── Tentukan urutan eksekusi (dependency graph)
```

#### Dokumen Konteks Master (Context Document)

Buat satu dokumen yang akan menjadi **"sumber kebenaran"** untuk semua interaksi dengan AI:

```markdown
# PROJECT CONTEXT DOCUMENT
## Nama Project: [Nama]
## Deskripsi: [1-2 kalimat]

### Tech Stack
- Backend: [contoh: Node.js + Express + TypeScript]
- Frontend: [contoh: React 18 + Vite + TailwindCSS]
- Database: [contoh: PostgreSQL + Prisma ORM]
- Auth: [contoh: JWT + bcrypt]
- Deployment: [contoh: Docker + AWS ECS]

### Coding Standards
- Naming convention: [camelCase / snake_case / PascalCase]
- File structure: [feature-based / layer-based]
- Error handling pattern: [contoh: custom error classes]
- API response format: [contoh: { success, data, error, meta }]

### Arsitektur
- Pattern: [contoh: Clean Architecture / MVC / Hexagonal]
- State management: [contoh: Zustand / Redux Toolkit]
- API style: [REST / GraphQL / tRPC]

### Aturan Bisnis Kritis
- [Aturan 1]
- [Aturan 2]
- [Aturan 3]
```

***

### 3. Anatomi Prompt yang Ideal

#### 🔬 Struktur Prompt 7 Lapis (Seven-Layer Prompt)

```
┌─────────────────────────────────────────────┐
│  LAYER 7: OUTPUT FORMAT                     │
│  → Tentukan format output yang diinginkan   │
├─────────────────────────────────────────────┤
│  LAYER 6: CONSTRAINTS & BOUNDARIES          │
│  → Batasan yang harus dipatuhi              │
├─────────────────────────────────────────────┤
│  LAYER 5: EXAMPLES (Few-Shot)               │
│  → Contoh input/output yang diharapkan      │
├─────────────────────────────────────────────┤
│  LAYER 4: SPECIFIC TASK                     │
│  → Instruksi detail apa yang harus dibuat   │
├─────────────────────────────────────────────┤
│  LAYER 3: TECHNICAL CONTEXT                 │
│  → Stack, arsitektur, existing code         │
├─────────────────────────────────────────────┤
│  LAYER 2: DOMAIN CONTEXT                    │
│  → Bisnis logic, aturan domain             │
├─────────────────────────────────────────────┤
│  LAYER 1: ROLE & PERSONA                    │
│  → Siapa AI dalam interaksi ini             │
└─────────────────────────────────────────────┘
```

#### Contoh Penerapan 7 Lapis:

````markdown
## ❌ PROMPT BURUK:
"Buatkan API login"

## ✅ PROMPT IDEAL:

**[LAYER 1 - ROLE]**
Kamu adalah senior backend engineer yang expert di Node.js, 
Express, dan TypeScript dengan pengalaman 10+ tahun membangun 
sistem autentikasi yang aman.

**[LAYER 2 - DOMAIN CONTEXT]**
Saya sedang membangun platform e-learning dimana user bisa 
berperan sebagai Student atau Instructor. Setiap user harus 
terautentikasi sebelum mengakses course content.

**[LAYER 3 - TECHNICAL CONTEXT]**
Tech stack:
- Runtime: Node.js 20 + TypeScript 5.x
- Framework: Express.js
- ORM: Prisma dengan PostgreSQL
- Auth: JWT (access + refresh token)
- Validasi: Zod
- Arsitektur: Clean Architecture (Controller → Service → Repository)

Berikut schema Prisma untuk User:
\```prisma
model User {
  id          String   @id @default(cuid())
  email       String   @unique
  password    String
  fullName    String
  role        Role     @default(STUDENT)
  isVerified  Boolean  @default(false)
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}

enum Role {
  STUDENT
  INSTRUCTOR
  ADMIN
}
\```

**[LAYER 4 - SPECIFIC TASK]**
Buatkan endpoint POST /api/v1/auth/login dengan ketentuan:
1. Menerima email dan password dari request body
2. Validasi input menggunakan Zod schema
3. Cek apakah user exists dan password valid (bcrypt)
4. Cek apakah user sudah verified (isVerified = true)
5. Generate access token (expire 15 menit) dan refresh token (expire 7 hari)
6. Simpan refresh token di database (tabel RefreshToken)
7. Return access token di response body, refresh token di httpOnly cookie

**[LAYER 5 - EXAMPLES]**
Request:
POST /api/v1/auth/login
Body: { "email": "john@example.com", "password": "SecurePass123!" }

Success Response (200):
{
  "success": true,
  "data": {
    "accessToken": "eyJhbG...",
    "user": {
      "id": "clx...",
      "email": "john@example.com",
      "fullName": "John Doe",
      "role": "STUDENT"
    }
  }
}

Error Response (401):
{
  "success": false,
  "error": {
    "code": "INVALID_CREDENTIALS",
    "message": "Email atau password salah"
  }
}

**[LAYER 6 - CONSTRAINTS]**
- JANGAN expose password hash di response apapun
- JANGAN beri informasi spesifik apakah email atau password yang salah
- Gunakan constant-time comparison untuk password
- Rate limit: maksimal 5 percobaan login per IP per 15 menit
- Semua error harus di-handle, jangan ada unhandled promise rejection
- Ikuti OWASP authentication best practices

**[LAYER 7 - OUTPUT FORMAT]**
Berikan output dalam file-file terpisah:
1. `src/modules/auth/auth.validation.ts` - Zod schemas
2. `src/modules/auth/auth.service.ts` - Business logic
3. `src/modules/auth/auth.controller.ts` - Request handler
4. `src/modules/auth/auth.route.ts` - Route definition
5. Jelaskan setiap keputusan teknis yang kamu buat
````

***

### 4. Framework Step-by-Step Eksekusi

#### 🎯 The CODER Framework

```
C - Clarify     → Klarifikasi requirements & konteks
O - Outline     → Minta AI buat outline/rencana dulu
D - Develop     → Eksekusi development per komponen
E - Evaluate    → Review & test output
R - Refine      → Iterasi sampai sesuai ekspektasi

     ┌──────────┐
     │ CLARIFY  │ ──→ Apakah requirements sudah jelas?
     └────┬─────┘     Tidak → Tanya balik ke stakeholder
          │ Ya
     ┌────▼─────┐
     │ OUTLINE  │ ──→ Minta AI buat rencana implementasi
     └────┬─────┘     Review rencana, setuju?
          │ Ya
     ┌────▼─────┐
     │ DEVELOP  │ ──→ Eksekusi satu per satu komponen
     └────┬─────┘     
          │
     ┌────▼─────┐
     │ EVALUATE │ ──→ Code review, test, validasi
     └────┬─────┘     Ada masalah?
          │ Ya            │ Tidak
     ┌────▼─────┐    ┌───▼────┐
     │  REFINE  │    │  DONE  │
     └────┬─────┘    └────────┘
          │
          └──→ Kembali ke DEVELOP atau OUTLINE
```

***

#### STEP 1: CLARIFY — Klarifikasi dengan AI

**Tujuan:** Gunakan AI untuk membantu Anda memikirkan hal yang belum terpikirkan.

```markdown
## PROMPT CLARIFY:

Saya ingin membangun [deskripsi singkat project].

Sebelum mulai coding, bantu saya mengidentifikasi:

1. **Pertanyaan kritis** yang harus dijawab sebelum mulai development
2. **Asumsi** yang mungkin saya buat tapi perlu dikonfirmasi
3. **Edge cases** yang sering terlewat untuk sistem seperti ini
4. **Keputusan arsitektur** yang harus dibuat di awal
5. **Risiko teknis** yang perlu dimitigasi

Konteks project:
[paste project context document]
```

**Contoh output yang diharapkan:**

```
AI akan mengembalikan daftar pertanyaan seperti:
- "Apakah sistem perlu mendukung multi-tenancy?"
- "Bagaimana strategi handling concurrent transactions?"
- "Apakah ada compliance requirement (GDPR, dll)?"
- dst.
```

**Apa yang Anda lakukan:** Jawab setiap pertanyaan, tambahkan jawaban ke Context Document.

***

#### STEP 2: OUTLINE — Minta Rencana Implementasi

**Tujuan:** Dapatkan blueprint sebelum menulis code.

```markdown
## PROMPT OUTLINE:

Berdasarkan context document berikut: [paste context]

Buatkan rencana implementasi detail untuk [fitur/modul].

Yang saya butuhkan:

1. **Daftar file** yang perlu dibuat beserta tanggung jawabnya
2. **Dependency graph** — file mana yang harus dibuat duluan
3. **Database schema changes** yang diperlukan
4. **API endpoint list** dengan method, path, dan deskripsi singkat
5. **Estimasi kompleksitas** tiap komponen (simple/medium/complex)

Format output:
- Gunakan tree structure untuk file listing
- Gunakan tabel untuk API endpoints
- Berikan urutan implementasi yang direkomendasikan (numbered steps)

JANGAN tulis code dulu. Hanya rencana.
```

**Contoh Output:**

```
📁 Rencana Implementasi: Modul Authentication

Urutan Implementasi:
════════════════════

Step 1: Database Schema (simple)
├── prisma/migrations/add-refresh-token-table
└── prisma/schema.prisma (update)

Step 2: Shared Utilities (simple)
├── src/utils/jwt.ts
├── src/utils/password.ts
└── src/utils/api-response.ts

Step 3: Auth Module (complex)
├── src/modules/auth/auth.validation.ts
├── src/modules/auth/auth.repository.ts
├── src/modules/auth/auth.service.ts
├── src/modules/auth/auth.controller.ts
└── src/modules/auth/auth.route.ts

Step 4: Middleware (medium)
├── src/middleware/authenticate.ts
├── src/middleware/authorize.ts
└── src/middleware/rate-limiter.ts

Step 5: Tests (medium)
├── tests/unit/auth.service.test.ts
└── tests/integration/auth.api.test.ts

API Endpoints:
┌────────┬──────────────────────────┬──────────────────────┐
│ Method │ Path                     │ Description          │
├────────┼──────────────────────────┼──────────────────────┤
│ POST   │ /api/v1/auth/register    │ Register new user    │
│ POST   │ /api/v1/auth/login       │ Login user           │
│ POST   │ /api/v1/auth/logout      │ Logout user          │
│ POST   │ /api/v1/auth/refresh     │ Refresh access token │
│ POST   │ /api/v1/auth/verify-email│ Verify email         │
└────────┴──────────────────────────┴──────────────────────┘
```

**Apa yang Anda lakukan:**

* Review rencana
* Adjust jika ada yang tidak sesuai
* Setujui sebelum lanjut ke DEVELOP

***

#### STEP 3: DEVELOP — Eksekusi Per Komponen

**Tujuan:** Implementasi satu komponen pada satu waktu.

> ⚠️ **ATURAN EMAS: Satu prompt = Satu komponen/file**

````markdown
## PROMPT DEVELOP (Template):

Konteks project: [paste context document ATAU referensi ke conversation sebelumnya]

Kita sedang mengimplementasi Step [X] dari rencana implementasi:
[paste bagian relevan dari outline]

Sekarang, buatkan file: `[nama file lengkap dengan path]`

Requirements:
- [requirement 1]
- [requirement 2]
- [requirement 3]

File yang sudah dibuat sebelumnya (sebagai referensi):

\```typescript
// src/utils/jwt.ts (sudah selesai)
[paste code yang sudah dibuat]
\```

Aturan:
- Gunakan TypeScript strict mode
- Semua function harus ada JSDoc comment
- Handle semua error cases
- Export yang diperlukan saja (principle of least privilege)

Berikan HANYA code untuk file yang diminta.
Setelah code, berikan catatan singkat tentang keputusan yang kamu buat.
````

#### Strategi Eksekusi Bertahap:

```
SESI 1: Foundation Layer
═══════════════════════
Prompt 1.1 → Database schema / migrations
Prompt 1.2 → Utility functions (jwt, hashing, response formatter)
Prompt 1.3 → Base classes / interfaces / types
            ↓
        [REVIEW & TEST]
            ↓
SESI 2: Business Logic Layer
════════════════════════════
Prompt 2.1 → Validation schemas
Prompt 2.2 → Repository layer
Prompt 2.3 → Service layer
            ↓
        [REVIEW & TEST]
            ↓
SESI 3: Interface Layer
═══════════════════════
Prompt 3.1 → Controllers
Prompt 3.2 → Routes
Prompt 3.3 → Middleware
            ↓
        [REVIEW & TEST]
            ↓
SESI 4: Quality Layer
════════════════════
Prompt 4.1 → Unit tests
Prompt 4.2 → Integration tests
Prompt 4.3 → Documentation
            ↓
        [FINAL REVIEW]
```

***

#### STEP 4: EVALUATE — Review Output AI

**Tujuan:** Validasi kualitas code sebelum integrasi.

````markdown
## PROMPT EVALUATE (Code Review):

Review code berikut dari perspektif:

1. **Security** — Apakah ada vulnerability? (injection, XSS, dll)
2. **Performance** — Apakah ada potential bottleneck? (N+1 query, memory leak)
3. **Error Handling** — Apakah semua edge case ter-handle?
4. **Best Practices** — Apakah sesuai dengan [framework] conventions?
5. **Type Safety** — Apakah TypeScript types sudah benar & ketat?
6. **Testability** — Apakah mudah di-test? Ada tight coupling?

Code:
\```typescript
[paste code yang mau di-review]
\```

Format review:
- 🔴 CRITICAL — Harus diperbaiki
- 🟡 WARNING — Sebaiknya diperbaiki
- 🟢 SUGGESTION — Nice to have
- ✅ GOOD — Hal yang sudah benar

Untuk setiap temuan, berikan:
1. Lokasi (baris/function)
2. Masalah
3. Solusi (dengan contoh code)
````

**Checklist Manual yang Tetap Harus Anda Lakukan:**

```
HUMAN REVIEW CHECKLIST
══════════════════════

□ Apakah code bisa berjalan? (build tanpa error)
□ Apakah business logic benar sesuai requirements?
□ Apakah naming sudah konsisten dengan codebase?
□ Apakah tidak ada hardcoded values yang seharusnya configurable?
□ Apakah tidak ada data sensitif yang ter-expose?
□ Apakah error messages user-friendly?
□ Apakah ada console.log / debug code yang harus dihapus?
□ Apakah imports sudah benar?
□ Coba jalankan secara manual — edge case bekerja?
```

***

#### STEP 5: REFINE — Iterasi Perbaikan

**Tujuan:** Perbaiki output berdasarkan hasil evaluasi.

```markdown
## PROMPT REFINE (Template):

Dari code [nama file] yang kamu buat sebelumnya, 
saya menemukan beberapa masalah:

### Masalah 1: [Deskripsi]
- Baris/area: [lokasi]
- Expected behavior: [apa yang seharusnya terjadi]
- Actual behavior: [apa yang terjadi sekarang]
- Contoh kasus: [input → expected output]

### Masalah 2: [Deskripsi]
...

### Tambahan requirement:
- [requirement baru yang teridentifikasi]

Tolong perbaiki code tersebut. 
Berikan HANYA bagian yang berubah, dengan komentar 
// CHANGED: [alasan perubahan] di setiap perubahan.
```

***

### 5. Pola Prompt untuk Setiap Fase SDLC

#### 📐 Fase 1: Requirements Analysis

```markdown
## PROMPT: Requirements Analysis

Saya memiliki deskripsi fitur berikut dari stakeholder:

"[paste deskripsi fitur dalam bahasa natural]"

Bantu saya mengubah ini menjadi:

1. **User Stories** dengan format:
   Sebagai [role], saya ingin [action], agar [benefit]
   
   Dengan acceptance criteria:
   - Given [context], When [action], Then [result]

2. **Functional Requirements** — numbered list (FR-001, FR-002, dst)

3. **Non-Functional Requirements** — performance, security, scalability

4. **Out of Scope** — apa yang BUKAN bagian dari fitur ini

5. **Dependencies** — fitur/sistem lain yang dibutuhkan

6. **Open Questions** — hal yang ambigu dan perlu diklarifikasi
```

***

#### 📐 Fase 2: System Design

```markdown
## PROMPT: Database Design

Berdasarkan requirements berikut:
[paste requirements]

Tech: [database engine] + [ORM]

Buatkan database design:

1. **ERD** dalam format mermaid diagram
2. **Schema definition** dalam format [Prisma/TypeORM/Sequelize/SQL DDL]
3. **Indexes** yang direkomendasikan beserta alasannya
4. **Seed data** untuk development/testing

Pertimbangan:
- Data yang sering di-query bersamaan
- Normalisasi vs denormalisasi (jelaskan tradeoff)
- Soft delete vs hard delete
- Audit trail (created_at, updated_at, deleted_at)
- Scalability (apakah perlu partitioning?)
```

```markdown
## PROMPT: API Design

Berdasarkan requirements dan database schema berikut:
[paste]

Buatkan API design dengan format:

Untuk setiap endpoint:
- Method + Path
- Description
- Authentication required? Role required?
- Request headers
- Request body (dengan TypeScript interface)
- Query parameters (jika ada)  
- Response body untuk setiap status code
- Rate limiting rules
- Caching strategy

Format output: OpenAPI 3.0 YAML atau Markdown table
```

```markdown
## PROMPT: Architecture Decision

Saya perlu memilih antara [opsi A] vs [opsi B] untuk [konteks].

Berikan analisis dalam format:

| Kriteria        | Opsi A    | Opsi B    |
|----------------|-----------|-----------|
| Performance    | ...       | ...       |
| Complexity     | ...       | ...       |
| Scalability    | ...       | ...       |
| Learning Curve | ...       | ...       |
| Community      | ...       | ...       |
| Cost           | ...       | ...       |

Pro & Cons masing-masing.
Rekomendasi final dengan alasan.
Contoh skenario dimana masing-masing opsi lebih unggul.
```

***

#### 📐 Fase 3: Implementation

````markdown
## PROMPT: Implementasi Komponen Baru

**Context:** [paste project context + related code]

**Task:** Implementasikan [nama komponen]

**Spesifikasi:**
- Input: [apa yang diterima]
- Process: [langkah-langkah logic]
- Output: [apa yang dikembalikan]
- Error cases: [kondisi error dan response-nya]

**Dependencies yang sudah ada:**
\```
[paste interface/type/code yang menjadi dependency]
\```

**Coding standards:**
- [standar 1]
- [standar 2]

**Berikan:**
1. Full implementation code
2. Inline comments untuk logic kompleks
3. JSDoc untuk public methods
````

````markdown
## PROMPT: Integrasi dengan Existing Code

Berikut existing code yang perlu dimodifikasi:

\```[language]
[paste existing code]
\```

Saya perlu menambahkan fungsionalitas:
[deskripsi fitur baru]

Aturan:
- Minimize perubahan pada existing code (Open/Closed Principle)
- Jangan break existing tests
- Backward compatible
- Tandai setiap perubahan dengan komentar // NEW: atau // MODIFIED:

Berikan:
1. Modified code (full file, bukan partial)
2. Daftar perubahan yang dilakukan
3. Potensi breaking changes (jika ada)
````

***

#### 📐 Fase 4: Testing

````markdown
## PROMPT: Generate Test Cases

Berikan comprehensive test cases untuk code berikut:

\```[language]
[paste code yang mau di-test]
\```

Testing framework: [Jest/Vitest/Mocha/Pytest/etc]

Buatkan:

1. **Unit Tests**
   - Happy path (semua input valid)
   - Edge cases (boundary values, empty input, null/undefined)
   - Error cases (invalid input, service failure)
   - Setiap branch dalam conditional logic

2. **Test Structure:**
   - describe blocks per function/method
   - it blocks dengan deskripsi behavior-driven
   - Proper setup/teardown (beforeEach/afterEach)
   - Mocking strategy untuk dependencies

3. **Coverage target:** semua branches, semua error paths

Format: siap copy-paste, bisa langsung dijalankan

Contoh test naming convention:
"should [expected behavior] when [condition]"
````

***

#### 📐 Fase 5: Documentation

````markdown
## PROMPT: API Documentation

Berdasarkan source code berikut:
\```
[paste route + controller + validation code]
\```

Buatkan API documentation yang mencakup:

1. **Endpoint Overview** — deskripsi singkat purpose
2. **Authentication** — apakah perlu auth, role apa
3. **Request** — method, URL, headers, body, query params
4. **Response** — untuk setiap status code (200, 400, 401, 403, 404, 500)
5. **Example** — curl command yang bisa langsung dipakai
6. **Error Codes** — daftar custom error codes dan artinya
7. **Rate Limiting** — rules yang berlaku

Format: Markdown (siap untuk README atau docs site)
````

***

### 6. Strategi Iterasi & Refinement

#### 🔄 Conversation Management

```
STRATEGI CONVERSATION MANAGEMENT
═════════════════════════════════

1. SATU CONVERSATION = SATU MODUL/FITUR
   → Jangan campur auth module dengan payment module
   → Mulai conversation baru untuk modul berbeda

2. SUMMARIZE PERIODICALLY
   → Setiap 5-10 prompt, minta AI merangkum apa yang sudah dibuat
   → Ini mencegah context drift

3. RE-INJECT CONTEXT
   → Di awal setiap sesi baru, paste ulang Context Document
   → Tambahkan summary dari sesi sebelumnya

4. KEEP A CHANGELOG
   → Catat di luar AI: file apa saja yang sudah dibuat
   → Status masing-masing: draft / reviewed / final
```

#### Prompt untuk Menjaga Konsistensi Antar Sesi

````markdown
## PROMPT: Context Resume

Saya melanjutkan development dari sesi sebelumnya.

**Project Context:** [paste context document]

**Progress sejauh ini:**
- ✅ Database schema (final)
- ✅ Auth module: validation, repository, service (final)
- 🔄 Auth module: controller (draft, perlu revision)
- ⬜ Auth module: routes, middleware (belum dibuat)

**File yang sudah final:**
\```typescript
// src/modules/auth/auth.service.ts
[paste latest version]
\```

**Issue dari sesi sebelumnya yang perlu diselesaikan:**
1. Error handling di login service belum cover kasus account locked
2. Refresh token rotation belum diimplementasi

**Tugas sekarang:** [tugas spesifik]
````

#### Teknik Iterasi Bertingkat

```
LEVEL 1: Quick Fix
"Di baris 45, ubah X menjadi Y karena [alasan]"

LEVEL 2: Targeted Refactor  
"Refactor function Z agar [tujuan]. Pertahankan interface yang sama."

LEVEL 3: Structural Change
"Ubah approach dari [cara lama] ke [cara baru]. 
 Berikut alasannya: [alasan].
 Yang perlu berubah: [daftar file/komponen].
 Mulai dari [file pertama]."

LEVEL 4: Complete Rewrite
"Implementasi [komponen] ini tidak sesuai. Tulis ulang dari awal
 dengan approach berikut: [deskripsi approach baru].
 Gunakan context yang sama tapi abaikan implementasi sebelumnya."
```

***

### 7. Anti-Pattern & Kesalahan Umum

#### 🚫 Anti-Pattern yang Harus Dihindari

```
┌─────────────────────────────────────────────────────────────┐
│ ANTI-PATTERN #1: "The Mega Prompt"                          │
│                                                             │
│ ❌ "Buatkan aplikasi e-commerce lengkap dengan user          │
│     management, product catalog, cart, checkout,             │
│     payment integration, admin dashboard, reporting,         │
│     dan notification system"                                │
│                                                             │
│ ✅ Pecah menjadi 8+ conversation terpisah, masing-masing    │
│    untuk satu modul, dimulai dari yang paling fundamental   │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ ANTI-PATTERN #2: "The Assumption Bomb"                      │
│                                                             │
│ ❌ "Buatkan REST API untuk users"                            │
│    (AI harus menebak: stack? auth? fields? validasi?)       │
│                                                             │
│ ✅ Sebutkan SETIAP asumsi secara eksplisit                   │
│    Stack, fields, validasi rules, auth, response format     │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ ANTI-PATTERN #3: "Copy-Paste Blindly"                       │
│                                                             │
│ ❌ Copy output AI langsung ke codebase tanpa membaca        │
│                                                             │
│ ✅ Baca setiap baris. Pahami. Test. Baru commit.            │
│    Minimal: jalankan build dan test suite                   │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ ANTI-PATTERN #4: "The Vague Feedback"                       │
│                                                             │
│ ❌ "Ini tidak benar, perbaiki"                               │
│ ❌ "Ada bug, fix dong"                                       │
│                                                             │
│ ✅ "Function calculateDiscount() di baris 23 mengembalikan   │
│    NaN ketika input quantity = 0. Expected: return 0.       │
│    Juga handle kasus quantity negatif (throw error)."       │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ ANTI-PATTERN #5: "Context Amnesia"                          │
│                                                             │
│ ❌ Lanjut prompting di conversation panjang tanpa            │
│    menyadari AI sudah "lupa" konteks awal                   │
│                                                             │
│ ✅ Setiap 10-15 exchanges, mulai conversation baru          │
│    dengan context document + summary progress              │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ ANTI-PATTERN #6: "Framework Frankenstein"                    │
│                                                             │
│ ❌ Tidak specify framework/library version, AI mencampur     │
│    syntax dari berbagai versi                               │
│                                                             │
│ ✅ Selalu cantumkan versi: "React 18", "Next.js 14          │
│    (App Router)", "Express 4.x", "Prisma 5.x"              │
└─────────────────────────────────────────────────────────────┘
```

***

### 8. Template Prompt Siap Pakai

#### 📝 Template A: Inisiasi Project Baru

```markdown
Saya akan memulai project baru. Bantu saya menyiapkan fondasi.

## Project Overview
- **Nama:** [nama project]
- **Tujuan:** [satu paragraf deskripsi]
- **Target User:** [siapa yang akan pakai]
- **MVP Scope:** [fitur minimal yang harus ada]

## Tech Stack (sudah diputuskan)
- [list tech stack]

## Yang saya butuhkan sekarang:
1. Project structure (folder & file tree)
2. Essential configuration files dengan isi lengkap:
   - package.json (dengan semua dependencies)
   - tsconfig.json
   - .env.example
   - docker-compose.yml (jika applicable)
   - ESLint & Prettier config
3. Base setup code:
   - Entry point (index.ts / app.ts)
   - Database connection
   - Basic middleware setup
   - Error handling setup
   - Logger setup
4. README.md dengan setup instructions

Berikan satu file per code block, dengan path sebagai judul.
```

***

#### 📝 Template B: Implementasi CRUD Module

````markdown
## Context
[paste project context]

## Task: Buat CRUD module untuk entity [NamaEntity]

### Entity Definition:
\```
[paste schema/interface/model]
\```

### Business Rules:
1. [aturan bisnis 1]
2. [aturan bisnis 2]

### Endpoints yang dibutuhkan:
| Method | Endpoint               | Description        | Auth  |
|--------|------------------------|--------------------|-------|
| POST   | /api/v1/[entity]       | Create             | Yes   |
| GET    | /api/v1/[entity]       | List (paginated)   | Yes   |
| GET    | /api/v1/[entity]/:id   | Get by ID          | Yes   |
| PUT    | /api/v1/[entity]/:id   | Update             | Yes   |
| DELETE | /api/v1/[entity]/:id   | Soft delete        | Admin |

### Pagination & Filter:
- Default page size: 10, max: 100
- Sortable fields: [field1, field2]
- Filterable fields: [field1, field2]
- Search fields: [field1, field2]

### File yang perlu dibuat:
1. Validation schema (Zod)
2. Repository (data access)
3. Service (business logic)  
4. Controller (request handling)
5. Route definition

Buat file-file tersebut sesuai urutan di atas.
Mulai dari file pertama: validation schema.
````

***

#### 📝 Template C: Bug Fix

````markdown
## Bug Report

**File:** `[path/to/file.ts]`
**Function/Method:** `[nama function]`

**Current code:**
\```[language]
[paste relevant code section]
\```

**Steps to reproduce:**
1. [step 1]
2. [step 2]
3. [step 3]

**Expected behavior:**
[apa yang seharusnya terjadi]

**Actual behavior:**
[apa yang terjadi sekarang]

**Error message (jika ada):**
\```
[paste error]
\```

**Environment:**
- Node.js: [version]
- [Other relevant versions]

**Tolong:**
1. Identifikasi root cause
2. Berikan fix dengan penjelasan
3. Suggest test case untuk prevent regression
````

***

#### 📝 Template D: Code Refactoring

````markdown
## Refactoring Request

**File:** `[path]`

**Current code:**
\```[language]
[paste code]
\```

**Masalah dengan code saat ini:**
- [masalah 1: contoh "function terlalu panjang (>100 lines)"]
- [masalah 2: contoh "mixed concerns: validation + business logic + data access"]
- [masalah 3: contoh "tidak testable karena direct DB call"]

**Tujuan refactoring:**
- [tujuan 1: contoh "pisahkan concerns sesuai Single Responsibility"]
- [tujuan 2: contoh "buat dependency injectable untuk testing"]
- [tujuan 3: contoh "improve readability"]

**Constraint:**
- Interface/contract yang digunakan oleh kode lain HARUS tetap sama
- Behavior harus identical (refactor, bukan rewrite logic)
- [constraint lain]

**Output yang diharapkan:**
- Refactored code (bisa jadi multiple files)
- Penjelasan setiap perubahan structural
- Sebelum/sesudah comparison untuk perubahan signifikan
````

***

#### 📝 Template E: Performance Optimization

````markdown
## Performance Optimization Request

**Context:**
[deskripsi masalah performa: "API endpoint /api/products 
memakan waktu 3-5 detik untuk response, target < 200ms"]

**Current implementation:**
\```[language]
[paste code]
\```

**Database query yang dihasilkan (jika applicable):**
\```sql
[paste SQL query dari query log]
\```

**Data volume:**
- Total records: [angka]
- Average request/second: [angka]
- Current response time: [angka]
- Target response time: [angka]

**Analisis yang sudah dilakukan:**
- [contoh: "profiling menunjukkan 80% waktu di database query"]
- [contoh: "N+1 query terdeteksi di relasi products-categories"]

**Bantu saya:**
1. Identifikasi semua bottleneck
2. Berikan solusi untuk masing-masing (ranked by impact)
3. Implementasi solusi dengan priority tertinggi
4. Estimasi improvement yang diharapkan
````

***

### 9. Studi Kasus End-to-End

#### 🏗️ Kasus: Membangun Task Management API

Berikut simulasi lengkap bagaimana workflow berjalan:

***

**CONVERSATION 1: Klarifikasi & Perencanaan**

**Prompt 1 (Clarify):**

```markdown
Saya ingin membangun Task Management API (seperti Todoist 
yang disederhanakan).

Fitur MVP:
- User registration & login
- CRUD tasks
- Task memiliki: title, description, status, priority, due date
- Task bisa di-assign ke user lain
- Filter & search tasks
- Organisasi tasks dalam projects

Tech stack:
- Node.js + Express + TypeScript
- PostgreSQL + Prisma
- JWT auth
- Zod validation

Sebelum mulai:
1. Apa saja pertanyaan kritis yang perlu dijawab?
2. Apa entity/model yang dibutuhkan?
3. Apa urutan implementasi yang ideal?
```

**Anda merespon jawaban AI, menjawab pertanyaan kritis, lalu lanjut...**

***

**Prompt 2 (Outline):**

```markdown
Berdasarkan diskusi kita, berikut keputusan finalnya:
[paste ringkasan keputusan]

Sekarang buatkan:
1. Complete Prisma schema untuk semua entity
2. Folder structure project
3. Daftar semua API endpoints
4. Dependency graph implementasi (urutan file)

JANGAN tulis implementation code dulu.
```

***

**CONVERSATION 2: Foundation Setup**

**Prompt 3 (Develop - Foundation):**

```markdown
[paste context document]

Kita mulai implementasi. Step 1: Project Setup.

Buatkan file-file berikut dengan konten lengkap:
1. package.json (semua dependencies)
2. tsconfig.json (strict mode)
3. prisma/schema.prisma (dari desain yang sudah kita setujui)
4. src/app.ts (Express setup dengan middleware standar)
5. src/config/index.ts (environment variables with validation)
6. .env.example

Untuk app.ts, setup:
- CORS
- Helmet
- JSON body parser (limit 10mb)
- Request logging (morgan)
- Global error handler
- 404 handler
- Health check endpoint GET /health
```

**Prompt 4 (Develop - Utilities):**

```markdown
Lanjut Step 2: Shared Utilities.

Berikut app.ts yang sudah kita buat:
[paste app.ts]

Buatkan:
1. src/utils/api-response.ts
   - Function: successResponse(data, message?, meta?)
   - Function: errorResponse(error, statusCode?)
   - Konsisten dengan format response yang kita tentukan

2. src/utils/jwt.ts
   - generateAccessToken(payload)
   - generateRefreshToken(payload)
   - verifyToken(token, type)
   - Token payload interface: { userId, email, role }

3. src/utils/password.ts
   - hashPassword(plain)
   - comparePassword(plain, hashed)

4. src/utils/pagination.ts
   - parsePaginationQuery(query) → { page, limit, skip }
   - buildPaginationMeta(total, page, limit) → { ... }

5. src/types/index.ts
   - Semua shared TypeScript types/interfaces
```

***

**CONVERSATION 3: Auth Module**

**Prompt 5 (Develop - Auth):**

```markdown
[paste updated context + utils code summary]

Step 3: Auth Module. Mulai dari validation.

Buatkan src/modules/auth/auth.validation.ts:
- registerSchema: email (valid email), password (min 8, 
  harus ada uppercase, lowercase, number), fullName (min 2)
- loginSchema: email, password
- refreshTokenSchema: refreshToken string
- Gunakan Zod
- Export inferred types dari setiap schema
```

**Prompt 6 → auth.repository.ts** **Prompt 7 → auth.service.ts** **Prompt 8 → auth.controller.ts** **Prompt 9 → auth.route.ts** **Prompt 10 → auth.middleware.ts**

_Setiap prompt mengikuti template DEVELOP, menyertakan code dari langkah sebelumnya sebagai referensi._

***

**CONVERSATION 4: Review & Testing**

**Prompt 11 (Evaluate):**

```markdown
Berikut complete auth module yang sudah kita buat:

[paste semua file auth]

Lakukan comprehensive code review. Fokus pada:
1. Security vulnerabilities
2. Error handling completeness
3. TypeScript type safety
4. Edge cases yang terlewat
5. Compliance dengan OWASP top 10
```

**Prompt 12 (Test):**

```markdown
Berdasarkan auth.service.ts berikut:
[paste code]

Buatkan comprehensive unit tests menggunakan Vitest + 
Prism mocking.

Test cases yang harus ada:
- Register: success, duplicate email, invalid input
- Login: success, wrong password, unverified account, 
  non-existent user
- Refresh token: success, expired token, revoked token, 
  token rotation
- Logout: success, invalid token

Mock semua external dependencies (Prisma, bcrypt, jwt utils)
```

***

**CONVERSATION 5: Task Module (repeat pattern)**

_...dan seterusnya untuk setiap modul..._

***

### 📊 Quick Reference Card

```
╔══════════════════════════════════════════════════════════════╗
║              AI-ASSISTED DEVELOPMENT CHEAT SHEET             ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  BEFORE PROMPTING                                            ║
║  □ Context Document ready?                                   ║
║  □ Task decomposed into small units?                         ║
║  □ Dependencies identified?                                  ║
║  □ Expected output format clear?                             ║
║                                                              ║
║  DURING PROMPTING                                            ║
║  □ Role assigned?                                            ║
║  □ Context provided (domain + technical)?                    ║
║  □ Task specific & unambiguous?                              ║
║  □ Examples included?                                        ║
║  □ Constraints stated?                                       ║
║  □ Output format defined?                                    ║
║                                                              ║
║  AFTER RECEIVING OUTPUT                                      ║
║  □ Code reviewed line by line?                               ║
║  □ Build passes?                                             ║
║  □ Tests written & passing?                                  ║
║  □ Edge cases verified manually?                             ║
║  □ Security check done?                                      ║
║  □ Consistent with existing codebase?                        ║
║                                                              ║
║  CONVERSATION MANAGEMENT                                     ║
║  □ 1 conversation = 1 module (max)                           ║
║  □ Context re-injected after 10-15 exchanges?                ║
║  □ Progress tracked externally?                              ║
║  □ Summary requested periodically?                           ║
║                                                              ║
║  PROMPT SIZE GUIDE                                           ║
║  ├── Simple task: 5-15 lines                                 ║
║  ├── Medium task: 20-50 lines                                ║
║  ├── Complex task: 50-100 lines                              ║
║  └── If >100 lines: DECOMPOSE FURTHER                        ║
║                                                              ║
╚══════════════════════════════════════════════════════════════╝
```

***

### 🎯 Penutup: Prinsip "The 80/20 of AI-Assisted Development"

```
80% keberhasilan ditentukan oleh SEBELUM Anda prompting:
├── Kejelasan requirements
├── Kualitas context document
├── Dekomposisi yang tepat
└── Pemilihan urutan eksekusi

20% sisanya adalah skill prompting itu sendiri:
├── Struktur prompt
├── Specificity
├── Iterasi yang efektif
└── Review yang teliti

INGAT:
══════
• AI tidak tahu apa yang TIDAK Anda sebutkan
• AI tidak bisa membaca pikiran Anda
• AI akan memberikan output SEBAIK konteks yang diberikan
• ANDA tetap the engineer — AI hanyalah tool tercepat
  yang pernah Anda gunakan
```

***

> **Golden Rule:** _Perlakukan AI seperti developer baru yang sangat pintar tapi baru hari pertama join tim Anda — dia bisa menulis code yang excellent, tapi butuh onboarding yang jelas tentang project, standards, dan konteks bisnis Anda._

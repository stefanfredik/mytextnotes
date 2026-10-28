# Arisan

### Struktur

Baik, saya akan membantu Anda merancang desain aplikasi arisan berdasarkan kriteria yang Anda berikan. Berikut adalah panduan langkah demi langkah untuk mendesain aplikasi tersebut, termasuk struktur teknologi, fitur, dan alur kerja umum.1. Teknologi yang Digunakan

* Bahasa Pemrograman: PHP (versi terbaru, saat ini PHP 8.3 per Feb 2025)
* Framework Backend: Laravel (versi terbaru, saat ini Laravel 11.x)
* Framework Frontend: Vue.js (versi terbaru, saat ini Vue 3.x)
* Database: MariaDB (versi terbaru, kompatibel dengan MySQL)
* Frontend Styling: Tailwind CSS
* Komunikasi: REST API (dibangun dengan Laravel untuk backend dan dikonsumsi oleh Vue.js di frontend)

2\. Arsitektur Aplikasi

* Backend: Laravel akan digunakan untuk membangun REST API yang menyediakan endpoint untuk semua fungsi aplikasi (login, CRUD data arisan, upload bukti transfer, dll.).
* Frontend: Vue.js sebagai single-page application (SPA) yang mengkonsumsi API dari backend, dengan Tailwind CSS untuk styling yang responsif dan modern.
* Database: MariaDB untuk menyimpan data pengguna, informasi arisan, anggota, dan bukti pembayaran.
* Struktur Folder:
  * Backend: Mengikuti struktur default Laravel (app, routes/api.php, controllers, dll.).
  * Frontend: Struktur Vue.js (src/components, src/views, src/services untuk API calls).

3\. Desain DatabaseBerikut adalah skema database awal yang dapat digunakan:

* Tabel users (untuk admin dan anggota):
  * id (PK)
  * name (varchar)
  * email (varchar, unique)
  * password (hashed)
  * role (enum: 'admin', 'member')
  * created\_at, updated\_at
* Tabel arisans (data arisan):
  * id (PK)
  * name (varchar)
  * start\_date (datetime)
  * total\_amount (decimal)
  * rules (text)
  * created\_at, updated\_at
* Tabel arisan\_members (relasi anggota ke arisan):
  * id (PK)
  * arisan\_id (FK ke arisans)
  * user\_id (FK ke users)
  * status (enum: 'active', 'pending', 'completed')
* Tabel payments (bukti pembayaran):
  * id (PK)
  * arisan\_id (FK ke arisans)
  * user\_id (FK ke users)
  * proof\_file (varchar, path ke file bukti)
  * status (enum: 'pending', 'validated', 'rejected')
  * created\_at, updated\_at
* Tabel notes (catatan/informasi):
  * id (PK)
  * arisan\_id (FK ke arisans)
  * content (text)
  * created\_by (FK ke users, admin)
  * created\_at, updated\_at

4\. Desain REST APIBerikut adalah contoh endpoint API yang dibutuhkan:

* General:
  * POST /api/login - Login pengguna (mengembalikan token JWT).
* Admin:
  * POST /api/users - Tambah user baru.
  * POST /api/arisans - Buat arisan baru.
  * POST /api/arisans/{id}/members - Tambah anggota ke arisan.
  * PUT /api/arisans/{id} - Edit informasi arisan (waktu, jumlah, aturan).
  * POST /api/payments/{id}/validate - Validasi bukti pembayaran.
  * POST /api/notes - Tambah catatan/informasi.
* User Anggota:
  * GET /api/arisans - Lihat daftar arisan yang diikuti.
  * POST /api/payments - Upload bukti transfer (multipart/form-data).
  * GET /api/notes/{arisan\_id} - Lihat catatan/informasi arisan.

5\. Fitur dan Alur KerjaGeneral:

* Login User:
  * Frontend: Form login di Vue.js (email, password).
  * Backend: Autentikasi dengan Laravel Sanctum/JWT, mengembalikan token untuk akses API.

Admin:

* Menambahkan User:
  * Form di frontend untuk input nama, email, dan role.
  * Backend menyimpan data ke tabel users dengan password default.
* Mengelola Data Arisan:
  * Buat Arisan Baru: Form untuk nama, waktu start, jumlah, dan aturan.
  * Tambah Anggota: Pilih user dari daftar untuk ditambahkan ke arisan.
  * Validasi Bukti Bayar: Tampilkan daftar bukti pembayaran (dengan preview gambar), tombol validasi/tolak.
* Mengelola Data Anggota:
  * CRUD anggota (tambah, edit, hapus) dengan form sederhana.
* Membuat Note:
  * Form teks untuk input catatan, simpan ke tabel notes.

User Anggota:

* Mengikuti Arisan:
  * Tampilan daftar arisan yang diikuti (dari tabel arisan\_members).
* Upload Bukti Transfer:
  * Form upload file (gambar/pdf) di Vue.js, kirim ke API /api/payments.
* Melihat Informasi/Catatan:
  * Daftar catatan dari API /api/notes/{arisan\_id}, ditampilkan dalam komponen Vue.

6\. Implementasi TeknologiBackend (Laravel):

* Gunakan Laravel Sanctum untuk autentikasi API.
* Buat model dan migration untuk tabel di atas.
* Controller untuk setiap fitur (contoh: ArisanController, PaymentController).
* Simpan file bukti transfer di storage Laravel (misalnya: storage/app/public/proofs).

Frontend (Vue.js + Tailwind):

* Gunakan Vue Router untuk navigasi (halaman login, dashboard admin, dashboard anggota).
* Integrasi Axios untuk memanggil API.
* Desain UI dengan Tailwind CSS (contoh: grid untuk daftar arisan, form untuk upload).
* Preview file bukti transfer sebelum upload menggunakan FileReader API.

7\. Langkah Pengembangan

1. Setup Backend:
   * Install Laravel (composer create-project laravel/laravel arisan-backend).
   * Konfigurasi MariaDB di .env.
   * Buat migration dan model.
   * Definisikan rute API di routes/api.php.
2. Setup Frontend:
   * Install Vue.js (npm create vue@latest).
   * Tambahkan Tailwind CSS (npm install -D tailwindcss).
   * Buat komponen untuk setiap fitur.
3. Integrasi:
   * Hubungkan frontend ke backend via Axios.
   * Uji setiap endpoint API dengan Postman sebelum integrasi frontend.
4. Testing:
   * Uji login, CRUD arisan, upload file, dan validasi bukti bayar.

8\. Catatan Tambahan

* Keamanan: Gunakan validasi input di Laravel dan Vue.js, enkripsi password, serta batasi akses API berdasarkan role.
* File Upload: Pastikan server mendukung upload file (konfigurasi php.ini: upload\_max\_filesize).
* Responsivitas: Desain dengan Tailwind agar aplikasi mobile-friendly.

Apakah Anda ingin saya bantu lebih detail untuk salah satu bagian, misalnya kode contoh untuk API atau komponen Vue.js?

### Backend

Baik, saya akan memberikan contoh kode lengkap untuk aplikasi arisan berdasarkan spesifikasi yang Anda berikan. Karena ini adalah proyek yang cukup besar, saya akan fokus pada bagian-bagian utama sebagai fondasi yang bisa Anda kembangkan lebih lanjut. Kode akan mencakup backend (Laravel) dan frontend (Vue.js), dengan komunikasi via REST API.Backend: Laravel1. Setup Awal

*   Install Laravel:bash

    ```bash
    composer create-project laravel/laravel arisan-backend
    cd arisan-backend
    composer require laravel/sanctum
    ```
*   Konfigurasi .env:env

    ```
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=arisan_db
    DB_USERNAME=root
    DB_PASSWORD=
    ```

2\. Migration DatabaseBuat file migration untuk tabel-tabel yang dibutuhkan:

*   users (dari default Laravel, tambahkan role):php

    ```php
    // database/migrations/xxxx_create_users_table.php
    Schema::create('users', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->string('email')->unique();
        $table->string('password');
        $table->enum('role', ['admin', 'member'])->default('member');
        $table->timestamps();
    });
    ```
*   arisans:php

    ```php
    // database/migrations/xxxx_create_arisans_table.php
    Schema::create('arisans', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->dateTime('start_date');
        $table->decimal('total_amount', 15, 2);
        $table->text('rules');
        $table->timestamps();
    });
    ```
*   arisan\_members:php

    ```php
    // database/migrations/xxxx_create_arisan_members_table.php
    Schema::create('arisan_members', function (Blueprint $table) {
        $table->id();
        $table->foreignId('arisan_id')->constrained()->onDelete('cascade');
        $table->foreignId('user_id')->constrained()->onDelete('cascade');
        $table->enum('status', ['active', 'pending', 'completed'])->default('pending');
        $table->timestamps();
    });
    ```
*   payments:php

    ```php
    // database/migrations/xxxx_create_payments_table.php
    Schema::create('payments', function (Blueprint $table) {
        $table->id();
        $table->foreignId('arisan_id')->constrained()->onDelete('cascade');
        $table->foreignId('user_id')->constrained()->onDelete('cascade');
        $table->string('proof_file');
        $table->enum('status', ['pending', 'validated', 'rejected'])->default('pending');
        $table->timestamps();
    });
    ```
*   notes:php

    ```php
    // database/migrations/xxxx_create_notes_table.php
    Schema::create('notes', function (Blueprint $table) {
        $table->id();
        $table->foreignId('arisan_id')->constrained()->onDelete('cascade');
        $table->foreignId('created_by')->constrained('users')->onDelete('cascade');
        $table->text('content');
        $table->timestamps();
    });
    ```

Jalankan migrasi:bash

```bash
php artisan migrate
```

3\. ModelBuat model dengan relasi:

*   User.php:php

    ```php
    namespace App\Models;

    use Illuminate\Foundation\Auth\User as Authenticatable;
    use Laravel\Sanctum\HasApiTokens;

    class User extends Authenticatable
    {
        use HasApiTokens;

        protected $fillable = ['name', 'email', 'password', 'role'];
        protected $hidden = ['password'];

        public function arisans()
        {
            return $this->belongsToMany(Arisan::class, 'arisan_members')->withPivot('status');
        }
    }
    ```
*   Arisan.php:php

    ```php
    namespace App\Models;

    use Illuminate\Database\Eloquent\Model;

    class Arisan extends Model
    {
        protected $fillable = ['name', 'start_date', 'total_amount', 'rules'];

        public function members()
        {
            return $this->belongsToMany(User::class, 'arisan_members')->withPivot('status');
        }

        public function payments()
        {
            return $this->hasMany(Payment::class);
        }

        public function notes()
        {
            return $this->hasMany(Note::class);
        }
    }
    ```
*   ArisanMember.php:php

    ```php
    namespace App\Models;

    use Illuminate\Database\Eloquent\Model;

    class ArisanMember extends Model
    {
        protected $table = 'arisan_members';
        protected $fillable = ['arisan_id', 'user_id', 'status'];
    }
    ```
*   Payment.php:php

    ```php
    namespace App\Models;

    use Illuminate\Database\Eloquent\Model;

    class Payment extends Model
    {
        protected $fillable = ['arisan_id', 'user_id', 'proof_file', 'status'];

        public function user()
        {
            return $this->belongsTo(User::class);
        }

        public function arisan()
        {
            return $this->belongsTo(Arisan::class);
        }
    }
    ```
*   Note.php:php

    ```php
    namespace App\Models;

    use Illuminate\Database\Eloquent\Model;

    class Note extends Model
    {
        protected $fillable = ['arisan_id', 'created_by', 'content'];

        public function arisan()
        {
            return $this->belongsTo(Arisan::class);
        }

        public function creator()
        {
            return $this->belongsTo(User::class, 'created_by');
        }
    }
    ```

4\. Controller dan API Routes

*   AuthController.php:php

    ```php
    namespace App\Http\Controllers;

    use App\Models\User;
    use Illuminate\Http\Request;
    use Illuminate\Support\Facades\Hash;

    class AuthController extends Controller
    {
        public function login(Request $request)
        {
            $request->validate([
                'email' => 'required|email',
                'password' => 'required',
            ]);

            $user = User::where('email', $request->email)->first();
            if (!$user || !Hash::check($request->password, $user->password)) {
                return response()->json(['message' => 'Invalid credentials'], 401);
            }

            $token = $user->createToken('api-token')->plainTextToken;
            return response()->json(['token' => $token, 'user' => $user]);
        }
    }
    ```
*   ArisanController.php (contoh beberapa fungsi):php

    ```php
    namespace App\Http\Controllers;

    use App\Models\Arisan;
    use App\Models\ArisanMember;
    use Illuminate\Http\Request;

    class ArisanController extends Controller
    {
        public function store(Request $request)
        {
            $request->validate([
                'name' => 'required',
                'start_date' => 'required|date',
                'total_amount' => 'required|numeric',
                'rules' => 'required',
            ]);

            $arisan = Arisan::create($request->all());
            return response()->json($arisan, 201);
        }

        public function addMember(Request $request, $id)
        {
            $request->validate([
                'user_id' => 'required|exists:users,id',
            ]);

            $arisanMember = ArisanMember::create([
                'arisan_id' => $id,
                'user_id' => $request->user_id,
                'status' => 'pending',
            ]);

            return response()->json($arisanMember, 201);
        }
    }
    ```
*   PaymentController.php:php

    ```php
    namespace App\Http\Controllers;

    use App\Models\Payment;
    use Illuminate\Http\Request;

    class PaymentController extends Controller
    {
        public function store(Request $request)
        {
            $request->validate([
                'arisan_id' => 'required|exists:arisans,id',
                'proof_file' => 'required|file|mimes:jpg,png,pdf|max:2048',
            ]);

            $file = $request->file('proof_file');
            $path = $file->store('proofs', 'public');

            $payment = Payment::create([
                'arisan_id' => $request->arisan_id,
                'user_id' => auth()->id(),
                'proof_file' => $path,
                'status' => 'pending',
            ]);

            return response()->json($payment, 201);
        }

        public function validatePayment($id)
        {
            $payment = Payment::findOrFail($id);
            $payment->update(['status' => 'validated']);
            return response()->json($payment);
        }
    }
    ```
*   routes/api.php:php

    ```php
    use App\Http\Controllers\AuthController;
    use App\Http\Controllers\ArisanController;
    use App\Http\Controllers\PaymentController;

    Route::post('/login', [AuthController::class, 'login']);

    Route::middleware('auth:sanctum')->group(function () {
        Route::post('/arisans', [ArisanController::class, 'store']);
        Route::post('/arisans/{id}/members', [ArisanController::class, 'addMember']);
        Route::post('/payments', [PaymentController::class, 'store']);
        Route::post('/payments/{id}/validate', [PaymentController::class, 'validatePayment']);
    });
    ```

5\. Middleware untuk RoleTambahkan cek role di middleware:

*   app/Http/Middleware/CheckRole.php:php

    ```php
    namespace App\Http\Middleware;

    use Closure;

    class CheckRole
    {
        public function handle($request, Closure $next, $role)
        {
            if ($request->user()->role !== $role) {
                return response()->json(['message' => 'Unauthorized'], 403);
            }
            return $next($request);
        }
    }
    ```
*   Daftarkan di app/Http/Kernel.php:php

    ```php
    protected $routeMiddleware = [
        'role' => \App\Http\Middleware\CheckRole::class,
    ];
    ```
*   Gunakan di rute:php

    ```php
    Route::middleware(['auth:sanctum', 'role:admin'])->group(function () {
        Route::post('/arisans', [ArisanController::class, 'store']);
    });
    ```



### Front End

Baik, karena Anda menggunakan Vite yang sudah terintegrasi dengan Laravel terbaru, kita akan menyesuaikan pendekatan untuk frontend Vue.js agar memanfaatkan fitur integrasi tersebut. Laravel terbaru (mulai versi 9 dan seterusnya) menggunakan Vite sebagai bundler default, sehingga Anda tidak perlu membuat proyek frontend terpisah. Semua aset frontend akan dikelola di dalam proyek Laravel Anda, dan Vite akan menangani kompilasi serta hot module replacement (HMR).Berikut langkah-langkah untuk melanjutkan proyek arisan Anda dengan Vue.js, Tailwind CSS, dan Vite yang terintegrasi di Laravel:

***

Setup dan Struktur1. Pastikan Vite TerintegrasiKarena Anda sudah menggunakan Laravel terbaru, Vite seharusnya sudah terkonfigurasi. Periksa file berikut untuk memastikan:

*   File vite.config.js di root proyek:javascript

    ```javascript
    import { defineConfig } from 'vite'
    import laravel from 'laravel-vite-plugin'
    import vue from '@vitejs/plugin-vue'

    export default defineConfig({
      plugins: [
        laravel({
          input: ['resources/js/app.js', 'resources/css/app.css'],
          refresh: true,
        }),
        vue({
          template: {
            transformAssetUrls: {
              base: null,
              includeAbsolute: false,
            },
          },
        }),
      ],
    })
    ```
*   File package.json: Pastikan dependensi berikut sudah ada (tambahkan jika belum):json

    ```json
    {
      "devDependencies": {
        "@vitejs/plugin-vue": "^5.0.0",
        "axios": "^1.6.0",
        "laravel-vite-plugin": "^1.0.0",
        "vite": "^5.0.0",
        "vue": "^3.4.0",
        "vue-router": "^4.2.0",
        "tailwindcss": "^3.4.0",
        "postcss": "^8.4.0",
        "autoprefixer": "^10.4.0"
      }
    }
    ```

    Jalankan perintah berikut untuk menginstall dependensi:bash

    ```bash
    npm install
    ```

2\. Konfigurasi Tailwind CSSJika belum ada, buat konfigurasi Tailwind:Buat file tailwind.config.js di root proyek:javascript

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './resources/**/*.blade.php',
    './resources/**/*.js',
    './resources/**/*.vue',
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

Edit file resources/css/app.css:css

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

3\. Struktur DirektoriKarena ini terintegrasi dengan Laravel, kita akan bekerja di dalam folder resources. Berikut struktur yang disarankan:

```
resources/
├── css/
│   └── app.css             # Styling Tailwind
├── js/
│   ├── app.js             # Entry point Vue
│   ├── router/            # Konfigurasi Vue Router
│   │   └── index.js
│   ├── components/        # Komponen Vue
│   │   ├── Login.vue
│   │   ├── Navbar.vue
│   │   ├── ArisanList.vue
│   │   └── PaymentForm.vue
│   └── views/             # Halaman utama
│       ├── Home.vue
│       ├── Dashboard.vue
│       └── ArisanDetail.vue
└── views/
    └── app.blade.php      # Template Blade utama
```

4\. Entry Point Vue (resources/js/app.js)Edit file resources/js/app.js untuk menginisialisasi Vue dan Vue Router:javascript

```javascript
import { createApp } from 'vue'
import App from './views/Home.vue' // Ganti dengan komponen root Anda
import router from './router'
import '../css/app.css'

const app = createApp(App)
app.use(router)
app.mount('#app')
```

5\. Template Blade Utama (resources/views/app.blade.php)Buat file ini untuk menjadi wadah aplikasi Vue Anda:blade

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Arisan App</title>
    @vite(['resources/css/app.css', 'resources/js/app.js'])
</head>
<body>
    <div id="app"></div>
</body>
</html>
```

Tambahkan route di routes/web.php untuk memuat template ini:php

```php
Route::get('/{any?}', function () {
    return view('app');
})->where('any', '.*');
```

6\. Konfigurasi Vue Router (resources/js/router/index.js)javascript

```javascript
import { createRouter, createWebHistory } from 'vue-router'
import Home from '../views/Home.vue'
import Dashboard from '../views/Dashboard.vue'
import ArisanDetail from '../views/ArisanDetail.vue'

const routes = [
  { path: '/', name: 'Home', component: Home },
  { path: '/dashboard', name: 'Dashboard', component: Dashboard, meta: { requiresAuth: true } },
  { path: '/arisan/:id', name: 'ArisanDetail', component: ArisanDetail, meta: { requiresAuth: true } },
]

const router = createRouter({
  history: createWebHistory(),
  routes,
})

router.beforeEach((to, from, next) => {
  const token = localStorage.getItem('token')
  if (to.meta.requiresAuth && !token) {
    next('/')
  } else {
    next()
  }
})

export default router
```

***

Komponen Frontend7. Komponen Home/Login (resources/js/views/Home.vue)vue

```
<template>
  <div class="flex items-center justify-center min-h-screen bg-gray-100">
    <div class="bg-white p-6 rounded-lg shadow-lg w-full max-w-md">
      <h2 class="text-2xl font-bold mb-6 text-center">Login</h2>
      <form @submit.prevent="login">
        <div class="mb-4">
          <label class="block text-gray-700">Email</label>
          <input v-model="form.email" type="email" class="w-full p-2 border rounded" required />
        </div>
        <div class="mb-4">
          <label class="block text-gray-700">Password</label>
          <input v-model="form.password" type="password" class="w-full p-2 border rounded" required />
        </div>
        <button type="submit" class="w-full bg-blue-500 text-white p-2 rounded hover:bg-blue-600">
          Login
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      form: {
        email: '',
        password: '',
      },
    }
  },
  methods: {
    async login() {
      try {
        const response = await axios.post('/api/login', this.form)
        localStorage.setItem('token', response.data.token)
        this.$router.push('/dashboard')
      } catch (error) {
        alert('Login failed: ' + error.response.data.message)
      }
    },
  },
}
</script>
```

8\. Komponen Navbar (resources/js/components/Navbar.vue)vue

```
<template>
  <nav class="bg-blue-500 p-4 text-white">
    <div class="container mx-auto flex justify-between">
      <span class="text-xl font-bold">Arisan App</span>
      <button @click="logout" class="bg-red-500 px-4 py-2 rounded hover:bg-red-600">
        Logout
      </button>
    </div>
  </nav>
</template>

<script>
export default {
  methods: {
    logout() {
      localStorage.removeItem('token')
      this.$router.push('/')
    },
  },
}
</script>
```

9\. Komponen Dashboard (resources/js/views/Dashboard.vue)vue

```
<template>
  <div class="min-h-screen bg-gray-100">
    <Navbar />
    <div class="container mx-auto p-6">
      <h1 class="text-3xl font-bold mb-6">Dashboard</h1>
      <ArisanList />
    </div>
  </div>
</template>

<script>
import Navbar from '../components/Navbar.vue'
import ArisanList from '../components/ArisanList.vue'

export default {
  components: { Navbar, ArisanList },
}
</script>
```

10\. Komponen ArisanList (resources/js/components/ArisanList.vue)vue

```
<template>
  <div class="bg-white p-6 rounded-lg shadow-lg">
    <h2 class="text-xl font-bold mb-4">Daftar Arisan</h2>
    <div v-if="arisans.length" class="space-y-4">
      <div v-for="arisan in arisans" :key="arisan.id" class="border p-4 rounded">
        <h3 class="text-lg font-semibold">{{ arisan.name }}</h3>
        <p>Total: Rp {{ arisan.total_amount }}</p>
        <p>Start: {{ new Date(arisan.start_date).toLocaleDateString() }}</p>
        <router-link :to="'/arisan/' + arisan.id" class="text-blue-500 hover:underline">
          Detail
        </router-link>
      </div>
    </div>
    <p v-else>Tidak ada arisan tersedia.</p>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      arisans: [],
    }
  },
  mounted() {
    this.fetchArisans()
  },
  methods: {
    async fetchArisans() {
      try {
        const response = await axios.get('/api/arisans', {
          headers: { Authorization: `Bearer ${localStorage.getItem('token')}` },
        })
        this.arisans = response.data
      } catch (error) {
        console.error('Error fetching arisans:', error)
      }
    },
  },
}
</script>
```

11\. Komponen ArisanDetail (resources/js/views/ArisanDetail.vue)vue

```
<template>
  <div class="min-h-screen bg-gray-100">
    <Navbar />
    <div class="container mx-auto p-6">
      <h1 class="text-3xl font-bold mb-6">{{ arisan.name }}</h1>
      <div class="bg-white p-6 rounded-lg shadow-lg">
        <p>Total: Rp {{ arisan.total_amount }}</p>
        <p>Rules: {{ arisan.rules }}</p>
        <PaymentForm :arisan-id="arisan.id" />
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from '../components/Navbar.vue'
import PaymentForm from '../components/PaymentForm.vue'
import axios from 'axios'

export default {
  components: { Navbar, PaymentForm },
  data() {
    return {
      arisan: {},
    }
  },
  mounted() {
    this.fetchArisan()
  },
  methods: {
    async fetchArisan() {
      try {
        const response = await axios.get(`/api/arisans/${this.$route.params.id}`, {
          headers: { Authorization: `Bearer ${localStorage.getItem('token')}` },
        })
        this.arisan = response.data
      } catch (error) {
        console.error('Error fetching arisan:', error)
      }
    },
  },
}
</script>
```

12\. Komponen PaymentForm (resources/js/components/PaymentForm.vue)vue

```
<template>
  <div class="mt-6">
    <h3 class="text-lg font-semibold mb-4">Upload Bukti Pembayaran</h3>
    <form @submit.prevent="submitPayment">
      <input type="file" @change="onFileChange" class="mb-4" accept=".jpg,.png,.pdf" />
      <button type="submit" class="bg-green-500 text-white p-2 rounded hover:bg-green-600">
        Submit
      </button>
    </form>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  props: ['arisanId'],
  data() {
    return {
      file: null,
    }
  },
  methods: {
    onFileChange(event) {
      this.file = event.target.files[0]
    },
    async submitPayment() {
      const formData = new FormData()
      formData.append('arisan_id', this.arisanId)
      formData.append('proof_file', this.file)

      try {
        await axios.post('/api/payments', formData, {
          headers: {
            Authorization: `Bearer ${localStorage.getItem('token')}`,
            'Content-Type': 'multipart/form-data',
          },
        })
        alert('Pembayaran berhasil diupload!')
      } catch (error) {
        alert('Error: ' + error.response.data.message)
      }
    },
  },
}
</script>
```

***

Menjalankan Proyek

1.  Jalankan server Laravel:bash

    ```bash
    php artisan serve
    ```
2.  Jalankan Vite untuk frontend:bash

    ```bash
    npm run dev
    ```

Sekarang, buka browser di http://localhost:8000 (atau port yang sesuai dengan server Laravel Anda), dan aplikasi Anda akan berjalan dengan Vue.js sebagai SPA yang terintegrasi dengan backend Laravel.

***

Catatan Penting

* API Routes: Pastikan rute API di routes/api.php sudah sesuai dengan yang digunakan di frontend (misalnya, /api/arisans, /api/payments).
* CSRF Token: Jika Anda mengalami masalah dengan POST request (CSRF), pastikan menggunakan Laravel Sanctum dan token autentikasi sesuai dengan backend Anda.
* Penyesuaian: Kode di atas mengasumsikan endpoint API sudah sesuai dengan backend Anda. Sesuaikan jika ada perbedaan (misalnya, struktur response atau nama field).

Jika ada bagian tertentu yang ingin diperdalam (misalnya, menambah fitur atau debugging), beri tahu saya!

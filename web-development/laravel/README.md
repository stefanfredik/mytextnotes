# Laravel

Okay, the user wants a comprehensive tutorial based on that YouTube video. Let me start by recalling what the video covers. From the previous interaction, I know it's about building a full-stack app with Laravel, Vue.js, image uploads, and deployment. The user probably wants a step-by-step guide similar to the video content.

First, I need to outline the main sections. The original summary had Project Setup, Frontend, Backend, Axios, Deployment, etc. But the user wants a full tutorial, so I should break it down into detailed steps. Maybe start with prerequisites, then setup both frontend and backend, integrate them, handle authentication, image uploads, and finally deployment.

Wait, the user mentioned deployment on shared hosting using GitHub Actions. I should include specific steps for that. Also, the video uses Laravel Breeze for authentication, so I need to explain how to set that up with Vue.

I should check if there are any specific commands or configurations from the video that are crucial. For example, using Axios interceptors for CSRF tokens, setting up CORS in Laravel, and GitHub Actions workflows.

I need to make sure each step is clear and follows the video's structure. Maybe divide the tutorial into sections like Project Setup, Frontend Development, Backend Development, Integration, Deployment. Each section with subsections.

Also, include code snippets where necessary, like the Axios configuration, Laravel routes, Vue components. But since I can't write too much code, maybe highlight the key parts and explain them.

The user might be a developer familiar with basics but needs guidance on integrating Vue and Laravel. So, the tutorial should be detailed but not too basic. Include steps like installing dependencies, setting up routes, creating components, handling forms, and API calls.

For deployment, explain setting up GitHub Actions, configuring the workflow file, and deploying to Hostinger. Mention environment variables and server requirements for Laravel.

I should also mention common pitfalls, like CORS issues, CSRF token handling, and ensuring the frontend and backend are properly connected. Maybe add notes or tips based on the video's content.

Finally, wrap it up with a conclusion and encourage engagement, like asking questions or subscribing. Make sure the tutorial flows logically from setup to deployment, mirroring the video's progression.

Berikut tutorial lengkap berdasarkan video [**Fullstack Laravel + Vue 3 (Authentication, Upload Gambar, Deploy Shared Hosting)**](https://www.youtube.com/watch?v=qVi3vv9K8Fk\&t=819s):

***

### **Tutorial Lengkap: Fullstack Laravel + Vue 3**

#### **Persiapan Awal**

1. **Instalasi Tools**:
   * PHP ≥8.1, Composer, Node.js, npm.
   * Laravel CLI: `composer global require laravel/installer`.
   * Code Editor (VS Code, PHPStorm, dll).
2.  **Buat Folder Proyek**:

    ```bash
    mkdir fullstack-project
    cd fullstack-project
    ```

***

#### **Backend (Laravel) Setup**

**1. Inisialisasi Proyek Laravel**

```bash
composer create-project laravel/laravel backend
cd backend
```

**2. Install Laravel Breeze (Authentication)**

```bash
composer require laravel/breeze --dev
php artisan breeze:install
php artisan migrate
```

**3. Konfigurasi Database**

*   Buka `.env` dan sesuaikan dengan database lokal:

    ```env
    DB_DATABASE=your_db_name
    DB_USERNAME=root
    DB_PASSWORD=
    ```

**4. Buat Model & Migration untuk Upload Gambar**

```bash
php artisan make:model Image -m
```

*   Edit file migration `create_images_table`:

    ```php
    public function up() {
        Schema::create('images', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->string('path');
            $table->foreignId('user_id')->constrained();
            $table->timestamps();
        });
    }
    ```
*   Jalankan migrasi:

    ```bash
    php artisan migrate
    ```

**5. Buat API Routes**

*   Tambahkan di `routes/api.php`:

    ```php
    use App\Http\Controllers\ImageController;

    Route::middleware('auth:sanctum')->group(function () {
        Route::post('/upload-image', [ImageController::class, 'store']);
        Route::get('/images', [ImageController::class, 'index']);
    });
    ```

**6. Buat Controller untuk Image**

```bash
php artisan make:controller ImageController
```

*   Isi `ImageController.php`:

    ```php
    public function store(Request $request) {
        $request->validate(['image' => 'required|image|max:2048']);
        $path = $request->file('image')->store('public/images');
        $image = auth()->user()->images()->create([
            'title' => $request->title,
            'path' => $path
        ]);
        return response()->json($image, 201);
    }

    public function index() {
        return auth()->user()->images;
    }
    ```

***

#### **Frontend (Vue 3) Setup**

**1. Inisialisasi Proyek Vue 3**

```bash
npm create vite@latest frontend -- --template vue
cd frontend
npm install
```

**2. Install Dependencies**

```bash
npm install axios vue-router@4 vuex@next @tailwindcss/forms
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

**3. Konfigurasi Tailwind CSS**

*   Tambahkan di `tailwind.config.js`:

    ```js
    module.exports = {
      content: ["./index.html", "./src/**/*.{vue,js,ts,jsx,tsx}"],
      theme: { extend: {} },
      plugins: [require('@tailwindcss/forms')],
    }
    ```

**4. Setup Router (Vue Router)**

*   Buat `src/router/index.js`:

    ```js
    import { createRouter, createWebHistory } from 'vue-router';
    import Login from '../views/Login.vue';
    import Register from '../views/Register.vue';
    import Dashboard from '../views/Dashboard.vue';

    const routes = [
      { path: '/', component: Dashboard, meta: { requiresAuth: true } },
      { path: '/login', component: Login },
      { path: '/register', component: Register },
    ];

    const router = createRouter({ history: createWebHistory(), routes });
    export default router;
    ```

**5. Buat Komponen Halaman**

*   Contoh `src/views/Dashboard.vue`:

    ```vue
    <template>
      <div class="container mx-auto p-4">
        <h1 class="text-2xl font-bold mb-4">Dashboard</h1>
        <input type="file" @change="handleUpload" />
      </div>
    </template>

    <script setup>
    import { ref } from 'vue';
    import axios from 'axios';

    const handleUpload = async (e) => {
      const file = e.target.files[0];
      const formData = new FormData();
      formData.append('image', file);
      await axios.post('/api/upload-image', formData);
    };
    </script>
    ```

***

#### **Integrasi Laravel + Vue**

**1. Konfigurasi CORS di Laravel**

*   Install package CORS:

    ```bash
    composer require fruitcake/laravel-cors
    ```
*   Tambahkan middleware di `app/Http/Kernel.php`:

    ```php
    protected $middleware = [
        \Fruitcake\Cors\HandleCors::class,
        // ...
    ];
    ```

**2. Setup Axios untuk API**

*   Buat `src/utils/axios.js`:

    ```js
    import axios from 'axios';

    const api = axios.create({
      baseURL: 'http://localhost:8000/api',
      withCredentials: true,
    });

    export default api;
    ```

***

#### **Deploy ke Shared Hosting (Hostinger)**

**1. Persiapan Server**

* Upload file Laravel dan Vue ke folder terpisah (misal: `public_html/api` dan `public_html/app`).

**2. Konfigurasi GitHub Actions**

*   Buat file `.github/workflows/deploy.yml`:

    ```yaml
    name: Deploy to Hostinger
    on: [push]
    jobs:
      deploy:
        runs-on: ubuntu-latest
        steps:
          - name: Deploy via FTP
            uses: SamKirkland/FTP-Deploy-Action@4.3.0
            with:
              server: ${{ secrets.FTP_SERVER }}
              username: ${{ secrets.FTP_USERNAME }}
              password: ${{ secrets.FTP_PASSWORD }}
              local-dir: ./backend/
              server-dir: /api/
    ```

**3. Setup Environment di Hosting**

* Atur environment variables untuk Laravel di panel hosting.
* Jalankan `composer install` dan `php artisan migrate` via SSH.

***

#### **Catatan Penting**

1. **CSRF Protection**:
   * Pastikan request dari Vue menyertakan header `X-CSRF-TOKEN` (gunakan `axios.defaults.withCredentials = true`).
2. **Authentication**:
   * Gunakan `Sanctum` untuk autentikasi API. Pastikan session cookie terkirim.
3. **Upload File**:
   *   Simpan file di folder `storage/app/public` dan buat symbolic link:

       ```bash
       php artisan storage:link
       ```

***

#### **Sumber Daya**

* **Source Code**: [GitHub Repository](https://github.com/your-repo)
* **Dokumentasi**:
  * [Laravel Sanctum](https://laravel.com/docs/sanctum)
  * [Vue Router](https://router.vuejs.org/)

Jika ada pertanyaan, tulis di komentar! Jangan lupa **subscribe** untuk tutorial lainnya 😊.

# Modul

## Modul Belajar Laravel 12

### Daftar Isi

1. [Persiapan Lingkungan Pengembangan](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#persiapan-lingkungan-pengembangan)
2. [Pendahuluan](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#pendahuluan)
3. [Konsep Dasar Laravel](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#konsep-dasar-laravel)
4. [Routing](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#routing)
5. [Controller](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#controller)
6. [Blade Template Engine](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#blade-template-engine)
7. [Database dan Migration](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#database-dan-migration)
8. [Eloquent ORM](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#eloquent-orm)
9. [Validasi Form](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#validasi-form)
10. [Authentication](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#authentication)
11. [Authorization](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#authorization)
12. [Middleware](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#middleware)
13. [Request dan Response](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#request-dan-response)
14. [Laravel Livewire](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#laravel-livewire)
15. [API Development](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#api-development)
16. [Testing](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#testing)
17. [Deployment](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#deployment)
18. [Best Practices](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#best-practices)
19. [Fitur Baru Laravel 12](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#fitur-baru-laravel-12)
20. [Proyek Praktis](https://claude.ai/chat/f56b36e0-67c3-408a-b08b-5b558efd10f1#proyek-praktis)

### Pendahuluan

#### Apa itu Laravel?

Laravel adalah framework PHP yang elegan dan ekspresif, dirancang untuk membuat pengembangan aplikasi web menjadi lebih menyenangkan dan produktif. Framework ini menawarkan struktur yang jelas dan fitur-fitur kuat yang memudahkan developer dalam membangun aplikasi dari yang sederhana hingga kompleks.

#### Sejarah dan Evolusi Laravel

Laravel pertama kali dirilis pada tahun 2011 oleh Taylor Otwell. Sejak saat itu, Laravel terus berkembang dan mengalami peningkatan yang signifikan hingga versi terbaru, Laravel 12. Versi ini dirilis dengan berbagai fitur baru dan peningkatan performa yang membuat pengembangan aplikasi menjadi lebih efisien.

#### Filosofi Laravel

Laravel dibangun dengan filosofi "Developer Happiness" atau kebahagiaan pengembang. Ini berarti Laravel dirancang untuk memberikan pengalaman pengembangan yang menyenangkan tanpa mengorbankan fungsionalitas. Laravel juga menekankan pada konvensi dibandingkan konfigurasi, yang memungkinkan developer untuk fokus pada logika bisnis daripada pengaturan.

#### Keunggulan Laravel

* **Sintaks Elegan**: Kode Laravel mudah dibaca dan dipahami.
* **Ekosistem Lengkap**: Memiliki berbagai package dan tool yang mendukung pengembangan.
* **Komunitas Aktif**: Dukungan komunitas yang besar dan aktif.
* **Dokumentasi Lengkap**: Memiliki dokumentasi yang komprehensif dan mudah dipahami.
* **Performa Tinggi**: Dioptimasi untuk kecepatan dan efisiensi.

### Persiapan Lingkungan Pengembangan

#### Kebutuhan Sistem

* PHP 8.2 atau lebih tinggi
* Composer (Package Manager untuk PHP)
* Database (MySQL, PostgreSQL, SQLite, atau SQL Server)
* Web Server (Apache, Nginx)
* Git (opsional, tapi direkomendasikan)

#### Instalasi PHP dan Composer

```bash
# Instalasi PHP di Ubuntu
sudo apt update
sudo apt install php8.2 php8.2-cli php8.2-common php8.2-mbstring php8.2-xml php8.2-zip php8.2-mysql

# Instalasi Composer
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php composer-setup.php
php -r "unlink('composer-setup.php');"
sudo mv composer.phar /usr/local/bin/composer
```

#### Instalasi Laravel

Ada beberapa cara untuk instalasi Laravel:

**Menggunakan Laravel Installer**

```bash
composer global require laravel/installer
laravel new nama-proyek
```

**Menggunakan Composer Create-Project**

```bash
composer create-project laravel/laravel nama-proyek
```

**Menggunakan GitHub**

```bash
git clone https://github.com/laravel/laravel.git nama-proyek
cd nama-proyek
composer install
```

#### Konfigurasi Awal

Setelah instalasi, Anda perlu melakukan konfigurasi awal:

1.  Salin file `.env.example` menjadi `.env`

    ```bash
    cp .env.example .env
    ```
2.  Generate application key

    ```bash
    php artisan key:generate
    ```
3.  Konfigurasi database di file `.env`

    ```
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=nama_database
    DB_USERNAME=username
    DB_PASSWORD=password
    ```
4.  Jalankan server development

    ```bash
    php artisan serve
    ```

### Konsep Dasar Laravel

#### Struktur Direktori Laravel

Laravel memiliki struktur direktori yang terorganisir dengan baik:

* **app/**: Berisi kode inti aplikasi
  * **Console/**: Berisi perintah Artisan kustom
  * **Exceptions/**: Berisi handler exception
  * **Http/**: Berisi controllers, middleware, dan requests
  * **Models/**: Berisi model Eloquent
  * **Providers/**: Berisi service providers
* **bootstrap/**: Berisi file bootstrap aplikasi
* **config/**: Berisi file konfigurasi
* **database/**: Berisi migrations, seeds, dan factories
* **public/**: Berisi file yang bisa diakses publik
* **resources/**: Berisi views, assets, dan language files
* **routes/**: Berisi definisi route
* **storage/**: Berisi file yang dihasilkan aplikasi
* **tests/**: Berisi file test
* **vendor/**: Berisi dependencies (dikelola oleh Composer)

#### Siklus Hidup Request

Siklus hidup request dalam Laravel:

1. Request diterima oleh server
2. Diproses oleh public/index.php
3. Middleware dijalankan
4. Request diteruskan ke router
5. Router mengarahkan ke controller
6. Controller melakukan logika bisnis
7. View di-render
8. Response dikirim ke client

#### Service Container

Service Container adalah alat yang kuat untuk manage dependencies dan dependency injection dalam Laravel. Dengan Service Container, Anda dapat:

* Mendefinisikan binding
* Menyelesaikan dependencies
* Membuat instance dari class

```php
// Mendefinisikan binding
$this->app->bind('HelpSpot\API', function ($app) {
    return new HelpSpot\API($app->make('HttpClient'));
});

// Menyelesaikan dependencies
$api = $this->app->make('HelpSpot\API');
```

#### Service Providers

Service Providers adalah pusat dari proses bootstrap Laravel. Mereka bertanggung jawab untuk:

* Mendaftarkan binding ke Service Container
* Mendaftarkan event listeners
* Melakukan konfigurasi awal

```php
class AppServiceProvider extends ServiceProvider
{
    public function register()
    {
        $this->app->singleton('translator', function ($app) {
            return new Translator;
        });
    }

    public function boot()
    {
        // Kode inisialisasi
    }
}
```

### Routing

#### Dasar Routing

Routing adalah proses menentukan bagaimana aplikasi merespons permintaan client untuk URL tertentu.

```php
// routes/web.php
Route::get('/', function () {
    return view('welcome');
});

Route::get('/about', function () {
    return view('about');
});
```

#### HTTP Methods

Laravel mendukung berbagai HTTP method:

```php
Route::get('/users', [UserController::class, 'index']);
Route::post('/users', [UserController::class, 'store']);
Route::put('/users/{id}', [UserController::class, 'update']);
Route::delete('/users/{id}', [UserController::class, 'destroy']);
```

#### Route Parameters

Anda dapat menentukan parameter dalam route:

```php
Route::get('/users/{id}', function ($id) {
    return "User dengan ID: " . $id;
});

// Parameter opsional
Route::get('/users/{name?}', function ($name = 'Guest') {
    return "Halo, " . $name;
});
```

#### Route Naming

Memberi nama pada route untuk memudahkan referensi:

```php
Route::get('/profile', [ProfileController::class, 'show'])->name('profile.show');

// Menggunakan nama route
$url = route('profile.show');
```

#### Route Groups

Mengelompokkan route untuk menerapkan atribut yang sama:

```php
Route::middleware(['auth'])->group(function () {
    Route::get('/dashboard', [DashboardController::class, 'index']);
    Route::get('/profile', [ProfileController::class, 'show']);
});

Route::prefix('admin')->group(function () {
    Route::get('/users', [AdminController::class, 'users']);
    Route::get('/reports', [AdminController::class, 'reports']);
});
```

#### Route Caching

Untuk meningkatkan performa, Anda dapat meng-cache route:

```bash
php artisan route:cache
```

### Controller

#### Membuat Controller

Controller bertugas menangani logika aplikasi yang berkaitan dengan permintaan HTTP.

```bash
php artisan make:controller UserController
```

#### Controller Methods

```php
namespace App\Http\Controllers;

use App\Models\User;
use Illuminate\Http\Request;

class UserController extends Controller
{
    // Menampilkan daftar resource
    public function index()
    {
        $users = User::all();
        return view('users.index', compact('users'));
    }

    // Menampilkan form untuk membuat resource baru
    public function create()
    {
        return view('users.create');
    }

    // Menyimpan resource baru
    public function store(Request $request)
    {
        $validatedData = $request->validate([
            'name' => 'required|max:255',
            'email' => 'required|email|unique:users',
            'password' => 'required|min:8',
        ]);

        User::create($validatedData);

        return redirect()->route('users.index')->with('success', 'User berhasil ditambahkan');
    }

    // Menampilkan resource tertentu
    public function show(User $user)
    {
        return view('users.show', compact('user'));
    }

    // Menampilkan form untuk mengedit resource
    public function edit(User $user)
    {
        return view('users.edit', compact('user'));
    }

    // Memperbarui resource
    public function update(Request $request, User $user)
    {
        $validatedData = $request->validate([
            'name' => 'required|max:255',
            'email' => 'required|email|unique:users,email,' . $user->id,
        ]);

        $user->update($validatedData);

        return redirect()->route('users.index')->with('success', 'User berhasil diperbarui');
    }

    // Menghapus resource
    public function destroy(User $user)
    {
        $user->delete();

        return redirect()->route('users.index')->with('success', 'User berhasil dihapus');
    }
}
```

#### Resource Controllers

Laravel memiliki fitur resource controllers untuk operasi CRUD:

```bash
php artisan make:controller UserController --resource
```

Lalu, daftarkan dalam routes:

```php
Route::resource('users', UserController::class);
```

#### Dependency Injection

Anda dapat menggunakan dependency injection dalam controller:

```php
public function show(User $user, UserService $userService)
{
    $stats = $userService->getStats($user);
    return view('users.show', compact('user', 'stats'));
}
```

#### Single Action Controllers

Untuk action yang kompleks, Anda dapat membuat controller khusus:

```php
namespace App\Http\Controllers;

class ProcessPaymentController extends Controller
{
    public function __invoke(Request $request)
    {
        // Proses pembayaran
        return redirect()->route('dashboard');
    }
}
```

Dan route-nya:

```php
Route::post('/payment', ProcessPaymentController::class);
```

### Blade Template Engine

#### Sintaks Dasar

Blade adalah template engine yang kuat dan intuitif dari Laravel:

```php
// Menampilkan variabel
{{ $name }}

// Menampilkan variabel dengan escape HTML
{!! $html !!}

// Komentar
{{-- Komentar ini tidak akan terlihat di HTML --}}
```

#### Direktif Control Structures

```php
// If statement
@if ($user->isAdmin)
    Admin
@elseif ($user->isManager)
    Manager
@else
    User
@endif

// Unless statement
@unless (Auth::check())
    Silakan login
@endunless

// Loops
@for ($i = 0; $i < 10; $i++)
    {{ $i }}
@endfor

@foreach ($users as $user)
    {{ $user->name }}
@endforeach

@forelse ($users as $user)
    {{ $user->name }}
@empty
    Tidak ada user
@endforelse

@while ($condition)
    Loop content
@endwhile
```

#### Layout dengan Blade

```php
// resources/views/layouts/app.blade.php
<!DOCTYPE html>
<html>
<head>
    <title>@yield('title')</title>
</head>
<body>
    <header>
        @include('partials.header')
    </header>
    
    <main>
        @yield('content')
    </main>
    
    <footer>
        @include('partials.footer')
    </footer>
</body>
</html>

// resources/views/home.blade.php
@extends('layouts.app')

@section('title', 'Home Page')

@section('content')
    <h1>Welcome to our website</h1>
    <p>This is the home page.</p>
@endsection
```

#### Components

Blade Components memungkinkan Anda membuat komponen yang dapat digunakan kembali:

```php
// Membuat component
php artisan make:component Alert

// resources/views/components/alert.blade.php
<div class="alert alert-{{ $type }}">
    {{ $slot }}
</div>

// Menggunakan component
<x-alert type="danger">
    <strong>Error!</strong> Something went wrong.
</x-alert>
```

#### Blade Directives

Blade menyediakan banyak direktif bawaan:

```php
@auth
    // Pengguna sudah login
@endauth

@guest
    // Pengguna belum login
@endguest

@csrf
// Menghasilkan token CSRF

@method('PUT')
// Menambahkan method spoofing

@php
    $counter = 1;
@endphp
```

### Database dan Migration

#### Konfigurasi Database

Konfigurasi database dilakukan di file `.env`:

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
```

#### Membuat Migration

Migration adalah cara untuk mengelola struktur database:

```bash
php artisan make:migration create_posts_table
```

File migration akan dibuat di `database/migrations`:

```php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreatePostsTable extends Migration
{
    public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->text('content');
            $table->foreignId('user_id')->constrained()->onDelete('cascade');
            $table->boolean('published')->default(false);
            $table->timestamp('published_at')->nullable();
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('posts');
    }
}
```

#### Menjalankan Migration

```bash
php artisan migrate
```

#### Rollback Migration

```bash
php artisan migrate:rollback
```

#### Schema Builder

Schema Builder memungkinkan Anda mengelola struktur database:

```php
// Menambah kolom
Schema::table('users', function (Blueprint $table) {
    $table->string('phone')->nullable()->after('email');
});

// Mengubah kolom
Schema::table('posts', function (Blueprint $table) {
    $table->string('title', 100)->change();
});

// Menghapus kolom
Schema::table('users', function (Blueprint $table) {
    $table->dropColumn('phone');
});
```

#### Indexes

```php
Schema::table('users', function (Blueprint $table) {
    $table->index('email');
    $table->unique(['name', 'email']);
    $table->fullText('description');
});
```

#### Foreign Keys

```php
Schema::table('posts', function (Blueprint $table) {
    $table->foreignId('category_id')->constrained();
    $table->foreignId('user_id')
          ->constrained()
          ->onUpdate('cascade')
          ->onDelete('cascade');
});
```

### Eloquent ORM

#### Membuat Model

```bash
php artisan make:model Post
```

Model dasar:

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    use HasFactory;

    // Tentukan tabel jika berbeda dari konvensi
    protected $table = 'posts';

    // Tentukan primary key jika bukan 'id'
    protected $primaryKey = 'post_id';

    // Tentukan apakah menggunakan timestamps
    public $timestamps = true;

    // Tentukan kolom yang dapat diisi (fillable)
    protected $fillable = [
        'title',
        'content',
        'user_id',
        'published',
        'published_at',
    ];

    // Tentukan kolom yang tidak dapat diisi (guarded)
    protected $guarded = [];

    // Tentukan casting tipe data kolom
    protected $casts = [
        'published' => 'boolean',
        'published_at' => 'datetime',
    ];
}
```

#### Operasi CRUD dengan Eloquent

**Create**

```php
// Cara 1
$post = new Post;
$post->title = 'Judul Post';
$post->content = 'Konten post...';
$post->user_id = 1;
$post->save();

// Cara 2
$post = Post::create([
    'title' => 'Judul Post',
    'content' => 'Konten post...',
    'user_id' => 1,
]);
```

**Read**

```php
// Mengambil semua data
$posts = Post::all();

// Mengambil data dengan kondisi
$publishedPosts = Post::where('published', true)->get();

// Mengambil data dengan id tertentu
$post = Post::find(1);

// Mengambil data pertama
$latestPost = Post::latest()->first();

// Mengambil data dengan kondisi atau throw exception
$post = Post::findOrFail(1);
```

**Update**

```php
// Cara 1
$post = Post::find(1);
$post->title = 'Judul Baru';
$post->save();

// Cara 2
Post::where('id', 1)->update(['title' => 'Judul Baru']);
```

**Delete**

```php
// Cara 1
$post = Post::find(1);
$post->delete();

// Cara 2
Post::destroy(1);
Post::destroy([1, 2, 3]);
```

#### Relasi Eloquent

**One To One**

```php
// User.php
public function profile()
{
    return $this->hasOne(Profile::class);
}

// Profile.php
public function user()
{
    return $this->belongsTo(User::class);
}
```

**One To Many**

```php
// User.php
public function posts()
{
    return $this->hasMany(Post::class);
}

// Post.php
public function user()
{
    return $this->belongsTo(User::class);
}
```

**Many To Many**

```php
// User.php
public function roles()
{
    return $this->belongsToMany(Role::class);
}

// Role.php
public function users()
{
    return $this->belongsToMany(User::class);
}
```

**Has Many Through**

```php
// Country.php
public function posts()
{
    return $this->hasManyThrough(Post::class, User::class);
}
```

**Polymorphic Relations**

```php
// Comment.php
public function commentable()
{
    return $this->morphTo();
}

// Post.php
public function comments()
{
    return $this->morphMany(Comment::class, 'commentable');
}
```

#### Eager Loading

Untuk menghindari N+1 query problem:

```php
// Tanpa eager loading (N+1 problem)
$posts = Post::all();
foreach ($posts as $post) {
    echo $post->user->name;
}

// Dengan eager loading
$posts = Post::with('user')->get();
foreach ($posts as $post) {
    echo $post->user->name;
}

// Eager loading multiple relationships
$posts = Post::with(['user', 'comments', 'tags'])->get();
```

#### Attribute Casting

```php
protected $casts = [
    'published' => 'boolean',
    'options' => 'array',
    'price' => 'decimal:2',
    'published_at' => 'datetime',
];
```

#### Scopes

```php
// Model
public function scopePublished($query)
{
    return $query->where('published', true);
}

public function scopeOfType($query, $type)
{
    return $query->where('type', $type);
}

// Penggunaan
$posts = Post::published()->ofType('tutorial')->get();
```

### Validasi Form

#### Validasi Dasar

```php
public function store(Request $request)
{
    $validatedData = $request->validate([
        'title' => 'required|max:255',
        'content' => 'required',
        'email' => 'required|email|unique:users',
        'password' => 'required|min:8|confirmed',
    ]);

    // Proses data
}
```

#### Aturan Validasi

Laravel memiliki banyak aturan validasi:

```php
$rules = [
    'name' => 'required|string|max:255',
    'email' => 'required|email|unique:users,email',
    'age' => 'nullable|integer|min:18',
    'website' => 'nullable|url',
    'image' => 'nullable|image|max:2048',
    'status' => 'in:active,inactive',
    'skills' => 'array|min:1',
    'skills.*' => 'string|distinct',
];
```

#### Pesan Error Kustom

```php
$validatedData = $request->validate([
    'email' => 'required|email',
    'password' => 'required|min:8',
], [
    'email.required' => 'Alamat email harus diisi.',
    'email.email' => 'Format email tidak valid.',
    'password.required' => 'Password harus diisi.',
    'password.min' => 'Password minimal 8 karakter.',
]);
```

#### Form Request Validation

```bash
php artisan make:request StorePostRequest
```

```php
namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class StorePostRequest extends FormRequest
{
    public function authorize()
    {
        return true;
    }

    public function rules()
    {
        return [
            'title' => 'required|max:255',
            'content' => 'required',
            'category_id' => 'required|exists:categories,id',
        ];
    }

    public function messages()
    {
        return [
            'title.required' => 'Judul harus diisi.',
            'title.max' => 'Judul maksimal 255 karakter.',
            'content.required' => 'Konten harus diisi.',
            'category_id.required' => 'Kategori harus dipilih.',
            'category_id.exists' => 'Kategori tidak valid.',
        ];
    }
}
```

Penggunaan dalam controller:

```php
public function store(StorePostRequest $request)
{
    $validatedData = $request->validated();
    
    // Proses data
}
```

#### Validasi Manual

```php
use Illuminate\Support\Facades\Validator;

$validator = Validator::make($request->all(), [
    'title' => 'required|max:255',
    'content' => 'required',
]);

if ($validator->fails()) {
    return redirect()->back()
        ->withErrors($validator)
        ->withInput();
}
```

#### Menampilkan Error di Blade

```php
<input type="text" name="title" value="{{ old('title') }}">
@error('title')
    <div class="alert alert-danger">{{ $message }}</div>
@enderror
```

### Authentication

#### Konfigurasi Authentication

Laravel menyediakan scaffolding untuk authentication:

```bash
composer require laravel/ui
php artisan ui bootstrap --auth
```

Atau menggunakan Laravel Breeze:

```bash
composer require laravel/breeze
php artisan breeze:install
```

#### Authentication Controller

Laravel menyediakan controller untuk authentication:

* `App\Http\Controllers\Auth\LoginController`
* `App\Http\Controllers\Auth\RegisterController`
* `App\Http\Controllers\Auth\ForgotPasswordController`
* `App\Http\Controllers\Auth\ResetPasswordController`
* `App\Http\Controllers\Auth\ConfirmPasswordController`
* `App\Http\Controllers\Auth\VerificationController`

#### Authentication Route

Routes untuk authentication didefinisikan dengan satu baris:

```php
Route::auth();
```

Atau jika Anda ingin kustomisasi:

```php
Route::get('/login', [LoginController::class, 'showLoginForm'])->name('login');
Route::post('/login', [LoginController::class, 'login']);
Route::post('/logout', [LoginController::class, 'logout'])->name('logout');

Route::get('/register', [RegisterController::class, 'showRegistrationForm'])->name('register');
Route::post('/register', [RegisterController::class, 'register']);
```

#### Guard

Laravel menggunakan konsep guard untuk menentukan bagaimana user diautentikasi:

```php
// Mendapatkan user yang sedang login
$user = Auth::user();

// Cek jika user sudah login
if (Auth::check()) {
    // User sudah login
}

// Login user
Auth::login($user);

// Login user dengan "remember me"
Auth::login($user, true);

// Logout user
Auth::logout();
```

#### Middleware Authentication

Middleware `auth` digunakan untuk melindungi route:

```php
Route::middleware('auth')->group(function () {
    Route::get('/dashboard', [DashboardController::class, 'index']);
});
```

#### API Authentication

Untuk API, gunakan sanctum:

```bash
composer require laravel/sanctum
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
php artisan migrate
```

Tambahkan middleware di `app/Http/Kernel.php`:

```php
'api' => [
    \Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful::class,
    'throttle:api',
    \Illuminate\Routing\Middleware\SubstituteBindings::class,
],
```

Penggunaan:

```php
Route::post('/login', function (Request $request) {
    $credentials = $request->validate([
        'email' => 'required|email',
        'password' => 'required',
    ]);

    if (Auth::attempt($credentials)) {
        $user = Auth::user();
        $token = $user->createToken('api-token')->plainTextToken;
        return response()->json(['token' => $token]);
    }

    return response()->json(['message' => 'Unauthorized'], 401);
});

Route::middleware('auth:sanctum')->get('/user', function (Request $request) {
    return $request->user();
});
```

### Authorization

#### Mendefinisikan Kebijakan (Policy)

Policy adalah kelas yang digunakan untuk mengatur otorisasi:

```bash
php artisan make:policy PostPolicy --model=Post
```

Implementasi policy:

```php
namespace App\Policies;

use App\Models\Post;
use App\Models\User;

class PostPolicy
{
    public function viewAny(User $user)
    {
        return true;
    }

    public function view(User $user, Post $post)
    {
        return true;
    }

    public function create(User $user)
    {
        return true;
    }

    public function update(User $user, Post $post)
    {
        return $user->id === $post->user_id;
    }

    public function delete(User $user, Post $post)
    {
        return $user->id === $post->user_id;
    }
}
```

#### Mendaftarkan Policy

Policy didaftarkan di `app/Providers/AuthServiceProvider.php`:

```php
protected $policies = [
    Post::class => PostPolicy::class,
];
```

#### Menggunakan Policy

```php
// Dalam controller
public function update(Request $request, Post $post)
{
    $this->authorize('update', $post);
    
    // Proses update
}

public function index()
{
    if (auth()->user()->can('viewAny', Post::class)) {
        // ...
    }
}
```

Dalam Blade:

```php
@can('update', $post)
    <a href="{{ route('posts.edit', $post) }}">Edit</a>
@endcan
```

#### Gate

Gate adalah closure yang digunakan untuk authorization:

```php
// Dalam AuthServiceProvider
public function boot()
{
    $this->registerPolicies();

    Gate::define('update-post', function (User $user, Post $post) {
        return $user->id === $post->user_id;
    });
}
```

Penggunaan:

```php
if (Gate::allows('update-post', $post)) {
    // User dapat mengupdate post
}

if (Gate::denies('update-post', $post)) {
    // User tidak dapat mengupdate post
}

// Atau dengan helper
if (auth()->user()->can('update-post', $post)) {
    // User dapat mengupdate post
}
```

### Middleware

#### Apa itu Middleware?

Middleware adalah mekanisme filtering HTTP request yang masuk ke aplikasi.

#### Membuat Middleware

```bash
php artisan make:middleware CheckAge

```

## Lanjutan

## Modul Belajar Laravel 12 (lanjutan)

### Middleware (lanjutan)

#### Implementasi Middleware

Contoh implementasi middleware:

```php
namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;

class CheckAge
{
    public function handle(Request $request, Closure $next)
    {
        if ($request->age < 18) {
            return redirect('home');
        }

        return $next($request);
    }
}
```

#### Mendaftarkan Middleware

Middleware didaftarkan di `app/Http/Kernel.php`:

```php
// Global middleware
protected $middleware = [
    \App\Http\Middleware\TrustProxies::class,
    \Illuminate\Http\Middleware\HandleCors::class,
    \App\Http\Middleware\PreventRequestsDuringMaintenance::class,
    \Illuminate\Foundation\Http\Middleware\ValidatePostSize::class,
    \App\Http\Middleware\TrimStrings::class,
    \Illuminate\Foundation\Http\Middleware\ConvertEmptyStringsToNull::class,
];

// Route middleware
protected $routeMiddleware = [
    'auth' => \App\Http\Middleware\Authenticate::class,
    'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
    'cache.headers' => \Illuminate\Http\Middleware\SetCacheHeaders::class,
    'can' => \Illuminate\Auth\Middleware\Authorize::class,
    'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
    'password.confirm' => \Illuminate\Auth\Middleware\RequirePassword::class,
    'signed' => \Illuminate\Routing\Middleware\ValidateSignature::class,
    'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
    'verified' => \Illuminate\Auth\Middleware\EnsureEmailIsVerified::class,
    'age' => \App\Http\Middleware\CheckAge::class,
];
```

#### Menggunakan Middleware

```php
// Dalam route
Route::get('/profile', function () {
    // ...
})->middleware('auth');

// Middleware dengan parameter
Route::get('/admin', function () {
    // ...
})->middleware('role:admin');

// Multiple middleware
Route::get('/admin/profile', function () {
    // ...
})->middleware(['auth', 'role:admin']);

// Middleware dalam grup
Route::middleware(['auth'])->group(function () {
    Route::get('/dashboard', function () {
        // ...
    });
    
    Route::get('/profile', function () {
        // ...
    });
});
```

#### Middleware Before & After

Middleware dapat dijalankan sebelum atau sesudah request:

```php
// Before middleware
public function handle($request, Closure $next)
{
    // Kode sebelum request diproses
    
    return $next($request);
}

// After middleware
public function handle($request, Closure $next)
{
    $response = $next($request);
    
    // Kode setelah request diproses
    
    return $response;
}
```

### Request dan Response

#### Request

Laravel menyediakan kelas `Illuminate\Http\Request` untuk mengakses data request:

```php
public function store(Request $request)
{
    // Mengakses input
    $name = $request->input('name');
    
    // Mengakses input dengan default value
    $name = $request->input('name', 'John');
    
    // Mengakses input dengan dot notation
    $name = $request->input('user.name');
    
    // Mengakses semua input
    $input = $request->all();
    
    // Mengakses beberapa input
    $input = $request->only(['name', 'email']);
    $input = $request->except(['password']);
    
    // Mengakses query string
    $name = $request->query('name');
    
    // Cek keberadaan input
    if ($request->has('name')) {
        // ...
    }
    
    // Cek jika input ada dan tidak kosong
    if ($request->filled('name')) {
        // ...
    }
    
    // Mengakses file
    $file = $request->file('photo');
    
    // Cek jika file diunggah
    if ($request->hasFile('photo')) {
        // ...
    }
}
```

#### Response

Laravel menyediakan berbagai cara untuk membuat response:

```php
// String response
return 'Hello World';

// View response
return view('welcome');

// JSON response
return response()->json([
    'name' => 'John',
    'email' => 'john@example.com',
]);

// Download response
return response()->download($pathToFile);

// File response
return response()->file($pathToFile);

// Redirect response
return redirect('/home');
return redirect()->route('home');
return redirect()->back();
return redirect()->with('status', 'Profile updated!');

// Response dengan status code
return response('Not Found', 404);

// Response dengan header
return response('Hello', 200)->header('Content-Type', 'text/plain');
```

#### Macro Response

Anda dapat membuat response macro untuk digunakan kembali:

```php
// Dalam AppServiceProvider
public function boot()
{
    Response::macro('caps', function ($value) {
        return Response::make(strtoupper($value));
    });
}

// Penggunaan
return response()->caps('hello world');
```

### Laravel Livewire

#### Pengenalan Livewire

Livewire adalah library full-stack untuk Laravel yang membuat aplikasi dinamis tanpa perlu menulis JavaScript.

#### Instalasi Livewire

```bash
composer require livewire/livewire
```

#### Membuat Komponen Livewire

```bash
php artisan make:livewire Counter
```

Ini akan membuat dua file:

* `app/Http/Livewire/Counter.php`
* `resources/views/livewire/counter.blade.php`

#### Implementasi Komponen

```php
// app/Http/Livewire/Counter.php
namespace App\Http\Livewire;

use Livewire\Component;

class Counter extends Component
{
    public $count = 0;

    public function increment()
    {
        $this->count++;
    }

    public function decrement()
    {
        $this->count--;
    }

    public function render()
    {
        return view('livewire.counter');
    }
}
```

```php
<!-- resources/views/livewire/counter.blade.php -->
<div>
    <h1>{{ $count }}</h1>
    <button wire:click="increment">+</button>
    <button wire:click="decrement">-</button>
</div>
```

#### Mounting Komponen

```php
// Dalam Blade
@livewire('counter')
```

Atau dalam route:

```php
Route::get('/counter', \App\Http\Livewire\Counter::class);
```

#### Properti Livewire

```php
// Properti
public $name = 'John';

// Validasi
public function updated($propertyName)
{
    $this->validateOnly($propertyName, [
        'name' => 'required|min:6',
        'email' => 'required|email',
    ]);
}

// Form submit
public function submit()
{
    $this->validate([
        'name' => 'required|min:6',
        'email' => 'required|email',
    ]);
    
    // Proses form
}
```

#### Livewire Actions

```php
// Dalam Blade
<button wire:click="save">Save</button>
<button wire:click="delete({{ $item->id }})">Delete</button>
<input wire:model="name" type="text">
<input wire:model.debounce.500ms="search" type="text">
<form wire:submit.prevent="submit">
    <!-- Form fields -->
</form>
```

#### Livewire Lifecycle Hooks

```php
public function mount($id)
{
    $this->post = Post::find($id);
}

public function hydrate()
{
    // Dijalankan setiap kali komponen di-hydrate
}

public function dehydrate()
{
    // Dijalankan setiap kali komponen di-dehydrate
}

public function updating($name, $value)
{
    // Dijalankan sebelum properti diupdate
}

public function updated($name, $value)
{
    // Dijalankan setelah properti diupdate
}
```

### API Development

#### Membuat API Controller

```bash
php artisan make:controller API/PostController --api
```

#### API Routes

```php
// routes/api.php
Route::middleware('auth:sanctum')->group(function () {
    Route::apiResource('posts', PostController::class);
});
```

#### API Resource

```bash
php artisan make:resource PostResource
```

```php
namespace App\Http\Resources;

use Illuminate\Http\Resources\Json\JsonResource;

class PostResource extends JsonResource
{
    public function toArray($request)
    {
        return [
            'id' => $this->id,
            'title' => $this->title,
            'content' => $this->content,
            'created_at' => $this->created_at->format('Y-m-d H:i:s'),
            'updated_at' => $this->updated_at->format('Y-m-d H:i:s'),
            'user' => new UserResource($this->whenLoaded('user')),
            'comments' => CommentResource::collection($this->whenLoaded('comments')),
        ];
    }
}
```

#### API Collection

```bash
php artisan make:resource PostCollection
```

```php
namespace App\Http\Resources;

use Illuminate\Http\Resources\Json\ResourceCollection;

class PostCollection extends ResourceCollection
{
    public function toArray($request)
    {
        return [
            'data' => $this->collection,
            'links' => [
                'self' => 'link-value',
            ],
        ];
    }
}
```

#### Menggunakan API Resource

```php
// Single resource
return new PostResource($post);

// Collection
return PostResource::collection(Post::all());

// Dengan pagination
return PostResource::collection(Post::paginate());
```

#### API Versioning

```php
// routes/api.php
Route::prefix('v1')->group(function () {
    Route::apiResource('posts', V1\PostController::class);
});

Route::prefix('v2')->group(function () {
    Route::apiResource('posts', V2\PostController::class);
});
```

#### API Throttling

```php
Route::middleware('throttle:60,1')->group(function () {
    Route::apiResource('posts', PostController::class);
});
```

### Testing

#### Tipe-tipe Testing

Laravel mendukung berbagai jenis testing:

* Unit Testing
* Feature Testing
* HTTP Testing
* Browser Testing (dengan Laravel Dusk)

#### Membuat Test

```bash
php artisan make:test PostTest
php artisan make:test PostTest --unit
```

#### Struktur Test

```php
namespace Tests\Feature;

use Illuminate\Foundation\Testing\RefreshDatabase;
use Tests\TestCase;

class PostTest extends TestCase
{
    use RefreshDatabase;

    public function test_a_user_can_view_posts()
    {
        $response = $this->get('/posts');

        $response->assertStatus(200);
    }

    public function test_a_user_can_create_a_post()
    {
        $response = $this->post('/posts', [
            'title' => 'Test Post',
            'content' => 'Test Content',
        ]);

        $response->assertRedirect('/posts');
        $this->assertDatabaseHas('posts', [
            'title' => 'Test Post',
            'content' => 'Test Content',
        ]);
    }
}
```

#### Running Test

```bash
php artisan test
vendor/bin/phpunit
```

#### HTTP Testing

```php
public function test_a_user_can_view_posts()
{
    $user = User::factory()->create();
    $post = Post::factory()->create(['user_id' => $user->id]);

    $response = $this->actingAs($user)->get('/posts');

    $response->assertStatus(200);
    $response->assertSee($post->title);
}
```

#### Database Testing

```php
public function test_a_post_can_be_created()
{
    $user = User::factory()->create();

    $response = $this->actingAs($user)->post('/posts', [
        'title' => 'Test Post',
        'content' => 'Test Content',
    ]);

    $this->assertDatabaseHas('posts', [
        'title' => 'Test Post',
        'content' => 'Test Content',
        'user_id' => $user->id,
    ]);
}
```

#### Mocking

```php
public function test_post_can_be_published()
{
    $this->mock(PostRepository::class, function ($mock) {
        $mock->shouldReceive('publish')->once()->andReturn(true);
    });

    $post = Post::factory()->create();
    $service = $this->app->make(PostService::class);

    $result = $service->publish($post);

    $this->assertTrue($result);
}
```

#### Factory

```php
namespace Database\Factories;

use App\Models\Post;
use App\Models\User;
use Illuminate\Database\Eloquent\Factories\Factory;

class PostFactory extends Factory
{
    protected $model = Post::class;

    public function definition()
    {
        return [
            'title' => $this->faker->sentence,
            'content' => $this->faker->paragraphs(3, true),
            'user_id' => User::factory(),
            'published' => false,
        ];
    }

    public function published()
    {
        return $this->state(function (array $attributes) {
            return [
                'published' => true,
                'published_at' => now(),
            ];
        });
    }
}
```

### Deployment

#### Persiapan Deployment

Sebelum deploy, lakukan hal berikut:

1.  Optimasi autoloader

    ```bash
    composer install --optimize-autoloader --no-dev
    ```
2.  Optimasi konfigurasi

    ```bash
    php artisan config:cache
    ```
3.  Optimasi route

    ```bash
    php artisan route:cache
    ```
4.  Optimasi view

    ```bash
    php artisan view:cache
    ```
5.  Compile assets

    ```bash
    npm run build
    ```

#### Deployment Server

Beberapa hal yang perlu diperhatikan:

1. Kebutuhan server
   * PHP 8.2 atau lebih tinggi
   * Ekstensi PHP: BCMath, Ctype, Fileinfo, JSON, Mbstring, OpenSSL, PDO, Tokenizer, XML
   * Database: MySQL, PostgreSQL, SQLite, SQL Server
2. Directory Permissions
   * Direktori `storage` dan `bootstrap/cache` harus writable
3. Environment File
   * Salin `.env.example` menjadi `.env`
   * Atur konfigurasi yang sesuai
   *   Generate app key

       ```bash
       php artisan key:generate
       ```

#### Deployment dengan Forge

Laravel Forge adalah layanan untuk deploy Laravel dengan mudah:

1. Connect ke server (DigitalOcean, AWS, dll)
2. Buat site baru
3. Connect ke repository
4. Deploy script

#### Deployment dengan Envoyer

Envoyer adalah layanan untuk zero-downtime deployment:

1. Connect ke server
2. Connect ke repository
3. Setup deployment hooks
4. Deploy

#### Deployment dengan Docker

```dockerfile
FROM php:8.2-fpm

# Install dependencies
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    unzip \
    git

# Install PHP extensions
RUN docker-php-ext-install pdo pdo_mysql

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Set working directory
WORKDIR /var/www

# Copy application
COPY . /var/www

# Install dependencies
RUN composer install --optimize-autoloader --no-dev

# Set permissions
RUN chown -R www-data:www-data /var/www
```

#### Deployment dengan GitHub Actions

```yaml
name: Deploy

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Setup PHP
        uses: shivammathur/setup-php@v2
        with:
          php-version: '8.2'
      
      - name: Install dependencies
        run: composer install --prefer-dist --no-progress --no-suggest
      
      - name: Deploy
        uses: SamKirkland/FTP-Deploy-Action@4.1.0
        with:
          server: ${{ secrets.FTP_SERVER }}
          username: ${{ secrets.FTP_USERNAME }}
          password: ${{ secrets.FTP_PASSWORD }}
          server-dir: /public_html/
```

### Best Practices

#### Kode Terorganisasi

* Ikuti PSR-12 coding standard
* Gunakan namespace yang jelas
* Gunakan autoloading
* Buat kelas dan method yang fokus
* Gunakan DI (Dependency Injection)

#### SOLID Principles

* **S**ingle Responsibility Principle
* **O**pen-Closed Principle
* **L**iskov Substitution Principle
* **I**nterface Segregation Principle
* **D**ependency Inversion Principle

#### Repository Pattern

```php
// Interface
interface PostRepositoryInterface
{
    public function all();
    public function find($id);
    public function create(array $data);
    public function update($id, array $data);
    public function delete($id);
}

// Implementasi
class PostRepository implements PostRepositoryInterface
{
    protected $model;

    public function __construct(Post $post)
    {
        $this->model = $post;
    }

    public function all()
    {
        return $this->model->all();
    }

    public function find($id)
    {
        return $this->model->find($id);
    }

    public function create(array $data)
    {
        return $this->model->create($data);
    }

    public function update($id, array $data)
    {
        $post = $this->model->find($id);
        $post->update($data);
        return $post;
    }

    public function delete($id)
    {
        return $this->model->destroy($id);
    }
}
```

#### Service Pattern

```php
class PostService
{
    protected $repository;

    public function __construct(PostRepositoryInterface $repository)
    {
        $this->repository = $repository;
    }

    public function getAllPosts()
    {
        return $this->repository->all();
    }

    public function createPost(array $data)
    {
        // Validasi, logika bisnis, dll
        return $this->repository->create($data);
    }

    public function updatePost($id, array $data)
    {
        // Validasi, logika bisnis, dll
        return $this->repository->update($id, $data);
    }

    public function deletePost($id)
    {
        // Validasi, logika bisnis, dll
        return $this->repository->delete($id);
    }
}
```

#### Caching

```php
// Menggunakan cache
$posts = Cache::remember('posts', 60, function () {
    return Post::all();
});

// Menggunakan cache dengan tag
Cache::tags(['posts', 'api'])->put('posts', $posts, 60);

// Menghapus cache
Cache::forget('posts');
Cache::tags(['posts'])->flush();
```

#### Security

* Gunakan HTTPS
* Hindari SQL Injection dengan menggunakan Query Builder atau Eloquent
* Validasi input
* Gunakan CSRF protection
* Hindari Mass Assignment
* Gunakan Authentication dan Authorization
* Gunakan Password Hashing

#### Performance

* Menggunakan Lazy Loading dan Eager Loading
* Menggunakan Query Builder daripada Collection
* Menggunakan Pagination
* Menggunakan Caching
* Menggunakan Queue untuk proses yang berat
* Menggunakan Database Indexing

### Fitur Baru Laravel 12

#### Migrasi Berbasis Kelas

Laravel 12 memperkenalkan migrasi berbasis kelas, yang memungkinkan Anda membuat migrasi yang lebih terstruktur:

```php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateUsersTable extends Migration
{
    public function up(): void
    {
        Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('email')->unique();
            $table->timestamp('email_verified_at')->nullable();
            $table->string('password');
            $table->rememberToken();
            $table->timestamps();
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('users');
    }
}
```

#### Improved Validation

Laravel 12 memperkenalkan peningkatan pada validasi, termasuk:

```php
// Nested validation
$validator = Validator::make($request->all(), [
    'users.*.name' => 'required|string|max:255',
    'users.*.email' => 'required|email|unique:users,email',
]);

// Improved error messages
$messages = [
    'users.*.name.required' => 'The name field is required for all users.',
    'users.*.email.unique' => 'The email has already been taken.',
];
```

#### Enhanced Eloquent Relationships

Laravel 12 menyediakan peningkatan pada relasi Eloquent:

```php
// Has-Many-Through dengan intermediate model
class Country extends Model
{
    public function posts()
    {
        return $this->hasManyThrough(
            Post::class,
            User::class,
            'country_id', // Foreign key on User table
            'user_id',    // Foreign key on Post table
            'id',         // Local key on Country table
            'id'          // Local key on User table
        );
    }
}
```

#### Verbatim String Syntax

Laravel 12 memperkenalkan sintaks string verbatim untuk Blade:

```php
@verbatim
    <div class="container">
        {{ hello }}
    </div>
@endverbatim
```

#### Improved API Resources

Laravel 12 menyediakan peningkatan pada API Resources:

```php
public function toArray($request)
{
    return [
        'id' => $this->id,
        'name' => $this->name,
        'email' => $this->email,
        'created_at' => $this->created_at,
        'updated_at' => $this->updated_at,
        'posts' => PostResource::collection($this->whenLoaded('posts')),
        'post_count' => $this->when($request->has('include_post_count'), function () {
            return $this->posts()->count();
        }),
    ];
}
```

#### Improved Queue System

Laravel 12 memperkenalkan peningkatan pada sistem antrian:

```php
// Unique jobs
SomeJob::dispatch($user)->unique('user-'.$user->id);

// Throttled jobs
SomeJob::dispatch($user)->throttle(60);
```

#### Laravel Precognition

Laravel 12 memperkenalkan Precognition, sebuah fitur untuk validasi asinkron:

```php
// Client-side setup
<script>
    const form = document.querySelector('form');
    form.addEventListener('input', async (e) => {
        const field = e.target;
        const response = await fetch('/validate', {
            method: 'POST',
            headers: {
                'X-Inertia-Precognition': true,
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                [field.name]: field.value
            })
        });
        const data = await response.json();
        // Handle validation errors
    });
</script>

// Server-side setup
Route::post('/validate', function (Request $request) {
    $request->validate([
        'email' => 'required|email|unique:users',
        'password' => 'required|min:8',
    ]);
    
    // Proses form
});
```

#### Improved Testing Tools

Laravel 12 menyediakan peningkatan pada tools testing:

```php
// Mocking Queue
$this->mock(Queue::class, function ($mock) {
    $mock->shouldReceive('push')->once()->with(Mockery::type(SendWelcomeEmail::class));
});

// Mocking Event
$this->mock(Dispatcher::class, function ($mock) {
    $mock->shouldReceive('dispatch')->once()->with(Mockery::type(UserRegistered::class));
});
```

#### Improved Laravel Horizon

Laravel 12 memperkenalkan peningkatan pada Laravel Horizon:

```php
// In config/horizon.php
'environments' => [
    'production' => [
        'supervisor-1' => [
            'connection' => 'redis',
            'queue' => ['default'],
            'balance' => 'simple',
            'processes' => 10,
            'tries' => 3,
            'timeout' => 60,
        ],
    ],
    'local' => [
        'supervisor-1' => [
            'connection' => 'redis',
            'queue' => ['default'],
            'balance' => 'simple',
            'processes' => 3,
            'tries' => 3,
        ],
    ],
],
```

### Proyek Praktis

#### Membuat Blog Sederhana

**Langkah 1: Membuat Project**

```bash
composer create-project laravel/laravel blog
cd blog
```

**Langkah 2: Membuat Model dan Migration**

```bash
php artisan make:model Post -m
```

```php
// database/migrations/xxxx_xx_xx_create_posts_table.php
public function up()
{
    Schema::create('posts', function (Blueprint $table) {
        $table->id();
        $table->string('title');
        $table->string('slug')->unique();
        $table->text('content');
        $table->foreignId('user_id')->constrained()->onDelete('cascade');
        $table->boolean('published')->default(false);
        $table->timestamp('published_at')->nullable();
        $table->timestamps();
    });
}
```

```bash
php artisan make:model Comment -m
```

```php
// database/migrations/xxxx_xx_xx_create_comments_table.php
public function up()
{
    Schema::create('comments', function (Blueprint $table) {
        $table->id();
        $table->text('content');
        $table->foreignId('post_id')->constrained()->onDelete('cascade');
        $table->foreignId('user_id')->constrained()->onDelete('cascade');
        $table->timestamps();
    });
}
```

**Langkah 3: Membuat Controller**

```bash
php artisan make:controller PostController --resource
php artisan make:controller CommentController --resource
```

**Langkah 4: Menambahkan Relasi**

```php
// app/Models/User.php
public function posts()
{
    return $this->hasMany(Post::class);
}

public function comments()
{
    return $this->hasMany(Comment::class);
}

// app/Models/Post.php
public function user()
{
    return $this->belongsTo(User::class);
}

public function comments()
{
    return $this->hasMany(Comment::class);
}

// app/Models/Comment.php
public function user()
{
    return $this->belongsTo(User::class);
}

public function post()
{
    return $this->belongsTo(Post::class);
}
```

**Langkah 5: Membuat Route**

```php
// routes/web.php
Route::middleware(['auth'])->group(function () {
    Route::resource('posts', PostController::class);
    Route::resource('posts.comments', CommentController::class)->shallow();
});
```

**Langkah 6: Membuat View**

```php
// resources/views/posts/index.blade.php
@extends('layouts.app')

@section('content')
    <div class="container">
        <h1>Blog Posts</h1>
        <a href="{{ route('posts.create') }}" class="btn btn-primary mb-3">Create Post</a>
        
        @foreach ($posts as $post)
            <div class="card mb-3">
                <div class="card-body">
                    <h2 class="card-title">{{ $post->title }}</h2>
                    <p class="card-text">{{ Str::limit($post->content, 200) }}</p>
                    <p class="card-text"><small class="text-muted">By {{ $post->user->name }} on {{ $post->created_at->format('M d, Y') }}</small></p>
                    <a href="{{ route('posts.show', $post) }}" class="btn btn-primary">Read More</a>
                </div>
            </div>
        @endforeach
        
        {{ $posts->links() }}
    </div>
@endsection
```

**Langkah 7: Implementasi Controller**

```php
// app/Http/Controllers/PostController.php
public function index()
{
    $posts = Post::with('user')
        ->where('published', true)
        ->latest('published_at')
        ->paginate(10);
    
    return view('posts.index', compact('posts'));
}

public function show(Post $post)
{
    $post->load(['user', 'comments.user']);
    
    return view('posts.show', compact('post'));
}
```

**Langkah 8: Membuat Form Request**

```bash
php artisan make:request StorePostRequest
php artisan make:request UpdatePostRequest
```

```php
// app/Http/Requests/StorePostRequest.php
public function rules()
{
    return [
        'title' => 'required|max:255',
        'content' => 'required',
        'published' => 'boolean',
    ];
}

// Implementasi di controller
public function store(StorePostRequest $request)
{
    $post = new Post($request->validated());
    $post->user_id = auth()->id();
    $post->slug = Str::slug($request->title);
    
    if ($request->published) {
        $post->published_at = now();
    }
    
    $post->save();
    
    return redirect()->route('posts.show', $post);
}
```

**Langkah 9: Menambahkan Policy**

```bash
php artisan make:policy PostPolicy --model=Post
```

```php
// app/Policies/PostPolicy.php
public function update(User $user, Post $post)
{
    return $user->id === $post->user_id;
}

public function delete(User $user, Post $post)
{
    return $user->id === $post->user_id;
}
```

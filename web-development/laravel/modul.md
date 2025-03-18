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

## Sistem Manajemen Inventaris Toko Online dengan Laravel 12

### Deskripsi Proyek

Proyek ini adalah sistem manajemen inventaris untuk toko online yang memungkinkan pengguna melakukan pengelolaan produk, kategori, stok, pesanan, dan laporan. Sistem ini dibangun menggunakan Laravel 12 dengan fitur terbaru.

### Prasyarat

* PHP 8.2+
* Composer
* Node.js dan NPM
* Database MySQL/PostgreSQL
* Git

### Langkah 1: Instalasi Laravel 12

```bash
composer create-project laravel/laravel inventory-management
cd inventory-management
```

### Langkah 2: Konfigurasi Database

Edit file `.env` untuk mengkonfigurasi koneksi database:

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=inventory_db
DB_USERNAME=root
DB_PASSWORD=password
```

### Langkah 3: Instalasi Laravel Breeze untuk Autentikasi

```bash
composer require laravel/breeze --dev
php artisan breeze:install blade
npm install
npm run dev
```

### Langkah 4: Membuat Model dan Migrasi

#### Model Kategori

```bash
php artisan make:model Category -m
```

Edit file migrasi `create_categories_table.php`:

```php
public function up(): void
{
    Schema::create('categories', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->string('slug')->unique();
        $table->text('description')->nullable();
        $table->timestamps();
    });
}
```

#### Model Produk

```bash
php artisan make:model Product -m
```

Edit file migrasi `create_products_table.php`:

```php
public function up(): void
{
    Schema::create('products', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->string('slug')->unique();
        $table->text('description')->nullable();
        $table->decimal('price', 10, 2);
        $table->integer('stock_quantity');
        $table->string('sku')->unique();
        $table->foreignId('category_id')->constrained()->onDelete('cascade');
        $table->string('image_path')->nullable();
        $table->boolean('is_active')->default(true);
        $table->timestamps();
    });
}
```

#### Model Supplier

```bash
php artisan make:model Supplier -m
```

Edit file migrasi `create_suppliers_table.php`:

```php
public function up(): void
{
    Schema::create('suppliers', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->string('email')->unique();
        $table->string('phone')->nullable();
        $table->string('address')->nullable();
        $table->text('notes')->nullable();
        $table->timestamps();
    });
}
```

#### Model Penerimaan Stok

```bash
php artisan make:model StockReceipt -m
```

Edit file migrasi `create_stock_receipts_table.php`:

```php
public function up(): void
{
    Schema::create('stock_receipts', function (Blueprint $table) {
        $table->id();
        $table->string('receipt_number')->unique();
        $table->foreignId('supplier_id')->constrained();
        $table->date('receipt_date');
        $table->decimal('total_amount', 12, 2);
        $table->text('notes')->nullable();
        $table->timestamps();
    });
}
```

#### Model Detail Penerimaan Stok

```bash
php artisan make:model StockReceiptItem -m
```

Edit file migrasi `create_stock_receipt_items_table.php`:

```php
public function up(): void
{
    Schema::create('stock_receipt_items', function (Blueprint $table) {
        $table->id();
        $table->foreignId('stock_receipt_id')->constrained()->onDelete('cascade');
        $table->foreignId('product_id')->constrained();
        $table->integer('quantity');
        $table->decimal('unit_cost', 10, 2);
        $table->timestamps();
    });
}
```

#### Model Pesanan

```bash
php artisan make:model Order -m
```

Edit file migrasi `create_orders_table.php`:

```php
public function up(): void
{
    Schema::create('orders', function (Blueprint $table) {
        $table->id();
        $table->string('order_number')->unique();
        $table->foreignId('user_id')->constrained();
        $table->date('order_date');
        $table->decimal('total_amount', 12, 2);
        $table->enum('status', ['pending', 'processing', 'completed', 'cancelled'])->default('pending');
        $table->string('shipping_address');
        $table->string('payment_method');
        $table->text('notes')->nullable();
        $table->timestamps();
    });
}
```

#### Model Detail Pesanan

```bash
php artisan make:model OrderItem -m
```

Edit file migrasi `create_order_items_table.php`:

```php
public function up(): void
{
    Schema::create('order_items', function (Blueprint $table) {
        $table->id();
        $table->foreignId('order_id')->constrained()->onDelete('cascade');
        $table->foreignId('product_id')->constrained();
        $table->integer('quantity');
        $table->decimal('unit_price', 10, 2);
        $table->timestamps();
    });
}
```

### Langkah 5: Migrasi Database

```bash
php artisan migrate
```

### Langkah 6: Membuat Factory dan Seeder untuk Data Testing

#### CategoryFactory

```bash
php artisan make:factory CategoryFactory
```

Edit `CategoryFactory.php`:

```php
public function definition(): array
{
    return [
        'name' => fake()->unique()->word(),
        'slug' => fn ($attributes) => Str::slug($attributes['name']),
        'description' => fake()->paragraph(),
    ];
}
```

#### ProductFactory

```bash
php artisan make:factory ProductFactory
```

Edit `ProductFactory.php`:

```php
public function definition(): array
{
    return [
        'name' => fake()->unique()->words(3, true),
        'slug' => fn ($attributes) => Str::slug($attributes['name']),
        'description' => fake()->paragraph(),
        'price' => fake()->randomFloat(2, 10, 1000),
        'stock_quantity' => fake()->numberBetween(5, 100),
        'sku' => 'SKU-' . fake()->unique()->randomNumber(6),
        'category_id' => Category::factory(),
        'image_path' => null,
        'is_active' => true,
    ];
}
```

#### DatabaseSeeder

Edit `database/seeders/DatabaseSeeder.php`:

```php
public function run(): void
{
    // Create admin user
    \App\Models\User::factory()->create([
        'name' => 'Admin User',
        'email' => 'admin@example.com',
        'password' => Hash::make('password'),
    ]);

    // Create categories and products
    \App\Models\Category::factory(5)
        ->has(\App\Models\Product::factory()->count(5))
        ->create();
        
    // Create suppliers
    \App\Models\Supplier::factory(10)->create();
}
```

### Langkah 7: Menjalankan Seeder

```bash
php artisan db:seed
```

### Langkah 8: Membuat Relasi antar Model

#### Category Model

```php
// app/Models/Category.php
public function products()
{
    return $this->hasMany(Product::class);
}
```

#### Product Model

```php
// app/Models/Product.php
public function category()
{
    return $this->belongsTo(Category::class);
}

public function stockReceiptItems()
{
    return $this->hasMany(StockReceiptItem::class);
}

public function orderItems()
{
    return $this->hasMany(OrderItem::class);
}
```

#### Supplier Model

```php
// app/Models/Supplier.php
public function stockReceipts()
{
    return $this->hasMany(StockReceipt::class);
}
```

#### StockReceipt Model

```php
// app/Models/StockReceipt.php
public function supplier()
{
    return $this->belongsTo(Supplier::class);
}

public function items()
{
    return $this->hasMany(StockReceiptItem::class);
}
```

#### StockReceiptItem Model

```php
// app/Models/StockReceiptItem.php
public function stockReceipt()
{
    return $this->belongsTo(StockReceipt::class);
}

public function product()
{
    return $this->belongsTo(Product::class);
}
```

#### Order Model

```php
// app/Models/Order.php
public function user()
{
    return $this->belongsTo(User::class);
}

public function items()
{
    return $this->hasMany(OrderItem::class);
}
```

#### OrderItem Model

```php
// app/Models/OrderItem.php
public function order()
{
    return $this->belongsTo(Order::class);
}

public function product()
{
    return $this->belongsTo(Product::class);
}
```

### Langkah 9: Membuat Controller

#### CategoryController

```bash
php artisan make:controller CategoryController --model=Category --resource
```

Edit `app/Http/Controllers/CategoryController.php`:

```php
<?php

namespace App\Http\Controllers;

use App\Models\Category;
use Illuminate\Http\Request;
use Illuminate\Support\Str;

class CategoryController extends Controller
{
    public function index()
    {
        $categories = Category::paginate(10);
        return view('categories.index', compact('categories'));
    }

    public function create()
    {
        return view('categories.create');
    }

    public function store(Request $request)
    {
        $validated = $request->validate([
            'name' => 'required|string|max:255|unique:categories',
            'description' => 'nullable|string',
        ]);

        $validated['slug'] = Str::slug($validated['name']);
        
        Category::create($validated);
        
        return redirect()->route('categories.index')
            ->with('success', 'Kategori berhasil dibuat.');
    }

    public function show(Category $category)
    {
        return view('categories.show', compact('category'));
    }

    public function edit(Category $category)
    {
        return view('categories.edit', compact('category'));
    }

    public function update(Request $request, Category $category)
    {
        $validated = $request->validate([
            'name' => 'required|string|max:255|unique:categories,name,' . $category->id,
            'description' => 'nullable|string',
        ]);

        $validated['slug'] = Str::slug($validated['name']);
        
        $category->update($validated);
        
        return redirect()->route('categories.index')
            ->with('success', 'Kategori berhasil diperbarui.');
    }

    public function destroy(Category $category)
    {
        // Check if category has products
        if ($category->products()->exists()) {
            return redirect()->route('categories.index')
                ->with('error', 'Tidak dapat menghapus kategori yang memiliki produk.');
        }
        
        $category->delete();
        
        return redirect()->route('categories.index')
            ->with('success', 'Kategori berhasil dihapus.');
    }
}
```

#### ProductController

```bash
php artisan make:controller ProductController --model=Product --resource
```

Edit `app/Http/Controllers/ProductController.php` dengan pendekatan yang sama.

### Langkah 10: Membuat Route

Edit `routes/web.php`:

```php
<?php

use App\Http\Controllers\ProfileController;
use App\Http\Controllers\CategoryController;
use App\Http\Controllers\ProductController;
use App\Http\Controllers\SupplierController;
use App\Http\Controllers\StockReceiptController;
use App\Http\Controllers\OrderController;
use App\Http\Controllers\DashboardController;
use Illuminate\Support\Facades\Route;

/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
*/

Route::get('/', function () {
    return view('welcome');
});

Route::middleware(['auth', 'verified'])->group(function () {
    Route::get('/dashboard', [DashboardController::class, 'index'])->name('dashboard');
    
    Route::resources([
        'categories' => CategoryController::class,
        'products' => ProductController::class,
        'suppliers' => SupplierController::class,
        'stock-receipts' => StockReceiptController::class,
        'orders' => OrderController::class,
    ]);
});

Route::middleware('auth')->group(function () {
    Route::get('/profile', [ProfileController::class, 'edit'])->name('profile.edit');
    Route::patch('/profile', [ProfileController::class, 'update'])->name('profile.update');
    Route::delete('/profile', [ProfileController::class, 'destroy'])->name('profile.destroy');
});

require __DIR__.'/auth.php';
```

### Langkah 11: Membuat Views

#### Layout Utama (Sudah ada dari Laravel Breeze)

#### Dashboard

Buat file `resources/views/dashboard.blade.php`:

```blade
<x-app-layout>
    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            {{ __('Dashboard') }}
        </h2>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
                <!-- Card Statistik Produk -->
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg p-6">
                    <div class="text-gray-900 text-3xl font-bold">{{ $productCount }}</div>
                    <div class="text-gray-500">Total Produk</div>
                </div>
                
                <!-- Card Kategori -->
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg p-6">
                    <div class="text-gray-900 text-3xl font-bold">{{ $categoryCount }}</div>
                    <div class="text-gray-500">Total Kategori</div>
                </div>
                
                <!-- Card Stok Rendah -->
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg p-6">
                    <div class="text-gray-900 text-3xl font-bold">{{ $lowStockCount }}</div>
                    <div class="text-gray-500">Produk Stok Rendah</div>
                </div>
                
                <!-- Card Pesanan -->
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg p-6">
                    <div class="text-gray-900 text-3xl font-bold">{{ $pendingOrderCount }}</div>
                    <div class="text-gray-500">Pesanan Pending</div>
                </div>
            </div>
            
            <!-- Grafik Penjualan Terbaru -->
            <div class="mt-6 bg-white overflow-hidden shadow-sm sm:rounded-lg p-6">
                <h3 class="text-lg font-medium text-gray-900 mb-4">Penjualan Terbaru</h3>
                <canvas id="salesChart" height="100"></canvas>
            </div>
            
            <!-- Daftar Pesanan Terbaru -->
            <div class="mt-6 bg-white overflow-hidden shadow-sm sm:rounded-lg p-6">
                <h3 class="text-lg font-medium text-gray-900 mb-4">Pesanan Terbaru</h3>
                <div class="overflow-x-auto">
                    <table class="min-w-full divide-y divide-gray-200">
                        <thead>
                            <tr>
                                <th class="px-6 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">No. Pesanan</th>
                                <th class="px-6 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Pelanggan</th>
                                <th class="px-6 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Tanggal</th>
                                <th class="px-6 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Total</th>
                                <th class="px-6 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Status</th>
                            </tr>
                        </thead>
                        <tbody class="bg-white divide-y divide-gray-200">
                            @foreach($recentOrders as $order)
                            <tr>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">{{ $order->order_number }}</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">{{ $order->user->name }}</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">{{ $order->order_date->format('d/m/Y') }}</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">Rp {{ number_format($order->total_amount, 0, ',', '.') }}</td>
                                <td class="px-6 py-4 whitespace-nowrap">
                                    <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full 
                                        @if($order->status == 'completed') bg-green-100 text-green-800 
                                        @elseif($order->status == 'processing') bg-blue-100 text-blue-800 
                                        @elseif($order->status == 'cancelled') bg-red-100 text-red-800 
                                        @else bg-yellow-100 text-yellow-800 @endif">
                                        {{ ucfirst($order->status) }}
                                    </span>
                                </td>
                            </tr>
                            @endforeach
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
    
    @push('scripts')
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const ctx = document.getElementById('salesChart').getContext('2d');
            const salesChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: {!! json_encode($chartData['labels']) !!},
                    datasets: [{
                        label: 'Penjualan',
                        data: {!! json_encode($chartData['data']) !!},
                        backgroundColor: 'rgba(59, 130, 246, 0.2)',
                        borderColor: 'rgba(59, 130, 246, 1)',
                        borderWidth: 1
                    }]
                },
                options: {
                    scales: {
                        y: {
                            beginAtZero: true
                        }
                    }
                }
            });
        });
    </script>
    @endpush
</x-app-layout>
```

#### Kategori

Buat file `resources/views/categories/index.blade.php`:

```blade
<x-app-layout>
    <x-slot name="header">
        <div class="flex justify-between items-center">
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                {{ __('Kategori') }}
            </h2>
            <a href="{{ route('categories.create') }}" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
                Tambah Kategori
            </a>
        </div>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            @if (session('success'))
                <div class="bg-green-100 border border-green-400 text-green-700 px-4 py-3 rounded relative mb-4" role="alert">
                    <span class="block sm:inline">{{ session('success') }}</span>
                </div>
            @endif

            @if (session('error'))
                <div class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative mb-4" role="alert">
                    <span class="block sm:inline">{{ session('error') }}</span>
                </div>
            @endif

            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6 text-gray-900">
                    <div class="overflow-x-auto">
                        <table class="min-w-full divide-y divide-gray-200">
                            <thead>
                                <tr>
                                    <th class="px-6 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">ID</th>
                                    <th class="px-6 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Nama</th>
                                    <th class="px-6 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Slug</th>
                                    <th class="px-6 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Jumlah Produk</th>
                                    <th class="px-6 py-3 bg-gray-50 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Aksi</th>
                                </tr>
                            </thead>
                            <tbody class="bg-white divide-y divide-gray-200">
                                @foreach ($categories as $category)
                                    <tr>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">{{ $category->id }}</td>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">{{ $category->name }}</td>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">{{ $category->slug }}</td>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">{{ $category->products->count() }}</td>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium">
                                            <a href="{{ route('categories.show', $category) }}" class="text-blue-600 hover:text-blue-900 mr-3">Lihat</a>

                                            <a href="{{ route('categories.edit', $category) }}" class="text-indigo-600 hover:text-indigo-900 mr-3">Edit</a>

                                            <form class="inline-block" action="{{ route('categories.destroy', $category) }}" method="POST" onsubmit="return confirm('Apakah Anda yakin ingin menghapus kategori ini?');">
                                                @csrf
                                                @method('DELETE')
                                                <button type="submit" class="text-red-600 hover:text-red-900">Hapus</button>
                                            </form>
                                        </td>
                                    </tr>
                                @endforeach
                            </tbody>
                        </table>
                    </div>
                    
                    <div class="mt-4">
                        {{ $categories->links() }}
                    </div>
                </div>
            </div>
        </div>
    </div>
</x-app-layout>
```

Dan buat file untuk `create.blade.php`, `edit.blade.php`, dan `show.blade.php` dengan pendekatan yang sama.

### Langkah 12: Menambahkan DashboardController

```bash
php artisan make:controller DashboardController
```

Edit `app/Http/Controllers/DashboardController.php`:

```php
<?php

namespace App\Http\Controllers;

use App\Models\Category;
use App\Models\Order;
use App\Models\Product;
use Carbon\Carbon;
use Illuminate\Http\Request;

class DashboardController extends Controller
{
    public function index()
    {
        // Card statistics
        $productCount = Product::count();
        $categoryCount = Category::count();
        $lowStockCount = Product::where('stock_quantity', '<', 10)->count();
        $pendingOrderCount = Order::where('status', 'pending')->count();

        // Recent orders
        $recentOrders = Order::with('user')
            ->orderBy('created_at', 'desc')
            ->limit(5)
            ->get();

        // Chart data - sales for last 7 days
        $startDate = Carbon::now()->subDays(6)->startOfDay();
        $endDate = Carbon::now()->endOfDay();

        $salesData = Order::where('status', '!=', 'cancelled')
            ->where('created_at', '>=', $startDate)
            ->where('created_at', '<=', $endDate)
            ->selectRaw('DATE(created_at) as date, SUM(total_amount) as total')
            ->groupBy('date')
            ->orderBy('date')
            ->get();

        $chartData = [
            'labels' => [],
            'data' => []
        ];

        for ($i = 0; $i < 7; $i++) {
            $date = Carbon::now()->subDays(6 - $i)->format('Y-m-d');
            $chartData['labels'][] = Carbon::parse($date)->format('d/m');
            
            $total = $salesData->firstWhere('date', $date);
            $chartData['data'][] = $total ? $total->total : 0;
        }

        return view('dashboard', compact(
            'productCount',
            'categoryCount',
            'lowStockCount',
            'pendingOrderCount',
            'recentOrders',
            'chartData'
        ));
    }
}
```

### Langkah 13: Menambahkan Validasi Request

```bash
php artisan make:request ProductRequest
```

Edit `app/Http/Requests/ProductRequest.php`:

```php
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class ProductRequest extends FormRequest
{
    public function authorize(): bool
    {
        return true;
    }

    public function rules(): array
    {
        $rules = [
            'name' => 'required|string|max:255',
            'category_id' => 'required|exists:categories,id',
            'description' => 'nullable|string',
            'price' => 'required|numeric|min:0',
            'stock_quantity' => 'required|integer|min:0',
            'sku' => 'required|string|max:50',
            'is_active' => 'sometimes|boolean',
            'image' => 'nullable|image|max:2048',
        ];

        // If updating, make unique check except the current product
        if ($this->isMethod('patch') || $this->isMethod('put')) {
            $product = $this->route('product');
            $rules['name'] .= '|unique:products,name,' . $product->id;
            $rules['sku'] .= '|unique:products,sku,' . $product->id;
        } else {
            $rules['name'] .= '|unique:products,name';
            $rules['sku'] .= '|unique:products,sku';
        }

        return $rules;
    }
}
```

### Langkah 14: Menambahkan Middleware dan Role

```bash
php artisan make:middleware CheckRole
```

Edit `app/Http/Middleware/CheckRole.php`:

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;
use Symfony\Component\HttpFoundation\Response;

class CheckRole
{
    public function handle(Request $request, Closure $next, string $role): Response
    {
        if (! $request->user() || ! $request->user()->hasRole($role)) {
            abort(403, 'Unauthorized action.');
        }

        return $next($request);
    }
}
```

Tambahkan middleware ke `app/Http/Kernel.php`:

```php
protected $routeMiddleware = [
    // Other middleware...
    'role' => \App\Http\Middleware\CheckRole::class,
];
```

### Langkah 15: Membuat Laporan dengan Laravel Excel

```bash
composer require maatwebsite/excel
```

Buat file `app/Exports/ProductsExport.php`:

```php
<?php

namespace App\Exports;

use App\Models\Product;
use Maatwebsite\Excel\Concerns\FromCollection;
use Maatwebsite\Excel\Concerns\WithHeadings;
use Maatwebsite\Excel\Concerns\WithMapping;

class ProductsExport implements FromCollection, WithHeadings, WithMapping
{
    public function collection()
    {
        return Product::with('category')->get();
    }

    public function headings(): array
    {
        return [
            'ID',
            'Nama',
            'SKU',
            'Kategori',
            'Harga',
            'Stok',
            'Status',
        ];
    }

    public function map($product): array
    {
        return [
            $product->id,
            $product->name,
            $product->sku,
            $product->category->name,
            $product->price,
            $product->stock_quantity,
            $product->is_active ? 'Aktif' : 'Tidak Aktif',
        ];
    }
}
```

Tambahkan method ke `ProductController`:

```php
public function export()
{
    return Excel::download(new ProductsExport, 'products.xlsx');
}
```

### Langkah 16: Menambahkan API untuk Mobile App

```bash
php artisan make:controller API/ProductController
```

Saya akan melanjutkan pembuatan modul Sistem Manajemen Inventaris Toko Online dengan Laravel 12 berdasarkan dokumen yang Anda berikan. Mari kita lanjutkan dengan langkah-langkah berikutnya.

### Langkah 16: Menambahkan API untuk Mobile App

Pertama, mari kita lengkapi controller API untuk produk:

```php
<?php

namespace App\Http\Controllers\API;

use App\Http\Controllers\Controller;
use App\Models\Product;
use Illuminate\Http\Request;
use App\Http\Resources\ProductResource;
use App\Http\Resources\ProductCollection;

class ProductController extends Controller
{
    public function index()
    {
        $products = Product::with('category')->paginate(10);
        return new ProductCollection($products);
    }

    public function show(Product $product)
    {
        return new ProductResource($product->load('category'));
    }

    public function store(Request $request)
    {
        $validated = $request->validate([
            'name' => 'required|string|max:255|unique:products',
            'category_id' => 'required|exists:categories,id',
            'description' => 'nullable|string',
            'price' => 'required|numeric|min:0',
            'stock_quantity' => 'required|integer|min:0',
            'sku' => 'required|string|max:50|unique:products',
            'is_active' => 'sometimes|boolean',
        ]);

        $product = Product::create($validated);
        return new ProductResource($product);
    }

    public function update(Request $request, Product $product)
    {
        $validated = $request->validate([
            'name' => 'required|string|max:255|unique:products,name,' . $product->id,
            'category_id' => 'required|exists:categories,id',
            'description' => 'nullable|string',
            'price' => 'required|numeric|min:0',
            'stock_quantity' => 'required|integer|min:0',
            'sku' => 'required|string|max:50|unique:products,sku,' . $product->id,
            'is_active' => 'sometimes|boolean',
        ]);

        $product->update($validated);
        return new ProductResource($product);
    }

    public function destroy(Product $product)
    {
        $product->delete();
        return response()->json(['message' => 'Produk berhasil dihapus']);
    }
}
```

### Langkah 17: Membuat Resource untuk API

```bash
php artisan make:resource ProductResource
php artisan make:resource ProductCollection
```

Edit `app/Http/Resources/ProductResource.php`:

```php
<?php

namespace App\Http\Resources;

use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\JsonResource;

class ProductResource extends JsonResource
{
    public function toArray(Request $request): array
    {
        return [
            'id' => $this->id,
            'name' => $this->name,
            'slug' => $this->slug,
            'description' => $this->description,
            'price' => $this->price,
            'stock_quantity' => $this->stock_quantity,
            'sku' => $this->sku,
            'is_active' => $this->is_active,
            'image_path' => $this->image_path ? asset('storage/' . $this->image_path) : null,
            'category' => [
                'id' => $this->category->id,
                'name' => $this->category->name,
            ],
            'created_at' => $this->created_at->format('Y-m-d H:i:s'),
            'updated_at' => $this->updated_at->format('Y-m-d H:i:s'),
        ];
    }
}
```

Edit `app/Http/Resources/ProductCollection.php`:

```php
<?php

namespace App\Http\Resources;

use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\ResourceCollection;

class ProductCollection extends ResourceCollection
{
    public function toArray(Request $request): array
    {
        return [
            'data' => $this->collection,
            'links' => [
                'self' => url()->current(),
            ],
            'meta' => [
                'total' => $this->total(),
                'count' => $this->count(),
                'per_page' => $this->perPage(),
                'current_page' => $this->currentPage(),
                'total_pages' => $this->lastPage(),
            ],
        ];
    }
}
```

### Langkah 18: Menambahkan API Routes

Edit `routes/api.php`:

```php
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\API\ProductController;
use App\Http\Controllers\API\CategoryController;
use App\Http\Controllers\API\OrderController;
use App\Http\Controllers\API\AuthController;

/*
|--------------------------------------------------------------------------
| API Routes
|--------------------------------------------------------------------------
*/

Route::middleware('auth:sanctum')->get('/user', function (Request $request) {
    return $request->user();
});

// Auth Routes
Route::post('/login', [AuthController::class, 'login']);
Route::post('/register', [AuthController::class, 'register']);

// Public Routes
Route::get('/products', [ProductController::class, 'index']);
Route::get('/products/{product}', [ProductController::class, 'show']);
Route::get('/categories', [CategoryController::class, 'index']);
Route::get('/categories/{category}', [CategoryController::class, 'show']);

// Protected Routes
Route::middleware('auth:sanctum')->group(function () {
    Route::post('/logout', [AuthController::class, 'logout']);
    
    // Product Routes
    Route::post('/products', [ProductController::class, 'store']);
    Route::put('/products/{product}', [ProductController::class, 'update']);
    Route::delete('/products/{product}', [ProductController::class, 'destroy']);
    
    // Category Routes
    Route::post('/categories', [CategoryController::class, 'store']);
    Route::put('/categories/{category}', [CategoryController::class, 'update']);
    Route::delete('/categories/{category}', [CategoryController::class, 'destroy']);
    
    // Order Routes
    Route::get('/orders', [OrderController::class, 'index']);
    Route::get('/orders/{order}', [OrderController::class, 'show']);
    Route::post('/orders', [OrderController::class, 'store']);
    Route::put('/orders/{order}', [OrderController::class, 'update']);
});
```

### Langkah 19: Menambahkan AuthController untuk API

```bash
php artisan make:controller API/AuthController
```

Edit `app/Http/Controllers/API/AuthController.php`:

```php
<?php

namespace App\Http\Controllers\API;

use App\Http\Controllers\Controller;
use App\Models\User;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Hash;
use Illuminate\Validation\ValidationException;

class AuthController extends Controller
{
    public function register(Request $request)
    {
        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'email' => 'required|string|email|max:255|unique:users',
            'password' => 'required|string|min:8|confirmed',
        ]);

        $user = User::create([
            'name' => $validated['name'],
            'email' => $validated['email'],
            'password' => Hash::make($validated['password']),
        ]);

        $token = $user->createToken('auth_token')->plainTextToken;

        return response()->json([
            'user' => $user,
            'token' => $token,
            'token_type' => 'Bearer',
        ], 201);
    }

    public function login(Request $request)
    {
        $request->validate([
            'email' => 'required|email',
            'password' => 'required',
        ]);

        $user = User::where('email', $request->email)->first();

        if (! $user || ! Hash::check($request->password, $user->password)) {
            throw ValidationException::withMessages([
                'email' => ['Email atau password salah.'],
            ]);
        }

        $token = $user->createToken('auth_token')->plainTextToken;

        return response()->json([
            'user' => $user,
            'token' => $token,
            'token_type' => 'Bearer',
        ]);
    }

    public function logout(Request $request)
    {
        $request->user()->currentAccessToken()->delete();

        return response()->json(['message' => 'Berhasil logout']);
    }
}
```

### Langkah 20: Menambahkan Fitur Upload Gambar

Edit `app/Http/Controllers/ProductController.php` untuk menambahkan fitur upload gambar:

```php
public function store(ProductRequest $request)
{
    $validated = $request->validated();

    // Handle image upload
    if ($request->hasFile('image')) {
        $path = $request->file('image')->store('products', 'public');
        $validated['image_path'] = $path;
    }

    $validated['slug'] = Str::slug($validated['name']);
    
    $product = Product::create($validated);
    
    return redirect()->route('products.index')
        ->with('success', 'Produk berhasil dibuat.');
}

public function update(ProductRequest $request, Product $product)
{
    $validated = $request->validated();

    // Handle image upload
    if ($request->hasFile('image')) {
        // Delete old image if exists
        if ($product->image_path && Storage::disk('public')->exists($product->image_path)) {
            Storage::disk('public')->delete($product->image_path);
        }
        
        $path = $request->file('image')->store('products', 'public');
        $validated['image_path'] = $path;
    }

    $validated['slug'] = Str::slug($validated['name']);
    
    $product->update($validated);
    
    return redirect()->route('products.index')
        ->with('success', 'Produk berhasil diperbarui.');
}
```

Tambahkan namespace di bagian atas file:

```php
use Illuminate\Support\Facades\Storage;
use Illuminate\Support\Str;
```

### Langkah 21: Menambahkan Fitur Pencarian

Edit `app/Http/Controllers/ProductController.php`:

```php
public function index(Request $request)
{
    $query = Product::with('category');

    // Search functionality
    if ($request->has('search')) {
        $search = $request->search;
        $query->where(function($q) use ($search) {
            $q->where('name', 'like', "%{$search}%")
              ->orWhere('sku', 'like', "%{$search}%")
              ->orWhereHas('category', function($q) use ($search) {
                  $q->where('name', 'like', "%{$search}%");
              });
        });
    }

    // Category filter
    if ($request->has('category') && $request->category != '') {
        $query->where('category_id', $request->category);
    }

    // Stock filter
    if ($request->has('stock_status')) {
        if ($request->stock_status == 'low') {
            $query->where('stock_quantity', '<', 10);
        } elseif ($request->stock_status == 'out') {
            $query->where('stock_quantity', 0);
        }
    }

    $products = $query->paginate(10);
    $categories = Category::all();

    return view('products.index', compact('products', 'categories'));
}
```

Edit `resources/views/products/index.blade.php` untuk menambahkan form pencarian:

```blade
<div class="mb-4">
    <form action="{{ route('products.index') }}" method="GET" class="flex flex-col sm:flex-row gap-4">
        <div class="flex-grow">
            <input type="text" name="search" value="{{ request('search') }}" placeholder="Cari produk..." class="w-full px-4 py-2 border rounded">
        </div>
        <div>
            <select name="category" class="w-full px-4 py-2 border rounded">
                <option value="">Semua Kategori</option>
                @foreach($categories as $category)
                    <option value="{{ $category->id }}" {{ request('category') == $category->id ? 'selected' : '' }}>
                        {{ $category->name }}
                    </option>
                @endforeach
            </select>
        </div>
        <div>
            <select name="stock_status" class="w-full px-4 py-2 border rounded">
                <option value="">Semua Stok</option>
                <option value="low" {{ request('stock_status') == 'low' ? 'selected' : '' }}>Stok Rendah (<10)</option>
                <option value="out" {{ request('stock_status') == 'out' ? 'selected' : '' }}>Habis</option>
            </select>
        </div>
        <div>
            <button type="submit" class="w-full px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">Cari</button>
        </div>
    </form>
</div>
```

### Langkah 22: Menambahkan Fitur Notifikasi

Buat model dan migrasi untuk notifikasi:

```bash
php artisan make:model Notification -m
```

Edit migrasi `create_notifications_table.php`:

```php
public function up(): void
{
    Schema::create('notifications', function (Blueprint $table) {
        $table->id();
        $table->foreignId('user_id')->constrained();
        $table->string('type');
        $table->string('title');
        $table->text('message');
        $table->string('link')->nullable();
        $table->boolean('read')->default(false);
        $table->timestamps();
    });
}
```

Buat class helper untuk mengirim notifikasi:

```php
<?php

namespace App\Helpers;

use App\Models\Notification;
use App\Models\User;

class NotificationHelper
{
    public static function send($userId, $type, $title, $message, $link = null)
    {
        return Notification::create([
            'user_id' => $userId,
            'type' => $type,
            'title' => $title,
            'message' => $message,
            'link' => $link,
            'read' => false,
        ]);
    }

    public static function lowStockAlert($productId, $productName)
    {
        // Send to all admin users
        $adminUsers = User::where('is_admin', true)->get();
        
        foreach ($adminUsers as $user) {
            self::send(
                $user->id,
                'low_stock',
                'Stok Produk Rendah',
                "Produk '{$productName}' memiliki stok yang rendah dan perlu diisi ulang.",
                route('products.edit', $productId)
            );
        }
    }
}
```

### Langkah 23: Menambahkan Fitur Laporan

Buat controller untuk laporan:

```bash
php artisan make:controller ReportController
```

Edit `app/Http/Controllers/ReportController.php`:

```php
<?php

namespace App\Http\Controllers;

use App\Models\Order;
use App\Models\Product;
use App\Models\StockReceipt;
use Carbon\Carbon;
use Illuminate\Http\Request;
use App\Exports\ProductsExport;
use App\Exports\SalesExport;
use Maatwebsite\Excel\Facades\Excel;

class ReportController extends Controller
{
    public function index()
    {
        return view('reports.index');
    }

    public function inventory()
    {
        $products = Product::with('category')->get();
        $totalValue = $products->sum(function($product) {
            return $product->price * $product->stock_quantity;
        });
        
        return view('reports.inventory', compact('products', 'totalValue'));
    }

    public function sales(Request $request)
    {
        $startDate = $request->input('start_date') ? Carbon::parse($request->input('start_date')) : Carbon::now()->startOfMonth();
        $endDate = $request->input('end_date') ? Carbon::parse($request->input('end_date')) : Carbon::now();

        $orders = Order::whereBetween('order_date', [$startDate, $endDate])
            ->where('status', 'completed')
            ->with(['items.product'])
            ->get();

        $totalSales = $orders->sum('total_amount');
        $totalOrders = $orders->count();
        
        // Group by product
        $salesByProduct = [];
        foreach ($orders as $order) {
            foreach ($order->items as $item) {
                $productId = $item->product_id;
                if (!isset($salesByProduct[$productId])) {
                    $salesByProduct[$productId] = [
                        'name' => $item->product->name,
                        'quantity' => 0,
                        'revenue' => 0
                    ];
                }
                $salesByProduct[$productId]['quantity'] += $item->quantity;
                $salesByProduct[$productId]['revenue'] += $item->quantity * $item->unit_price;
            }
        }
        
        return view('reports.sales', compact('orders', 'totalSales', 'totalOrders', 'salesByProduct', 'startDate', 'endDate'));
    }

    public function exportInventory()
    {
        return Excel::download(new ProductsExport, 'inventory-report.xlsx');
    }

    public function exportSales(Request $request)
    {
        $startDate = $request->input('start_date') ? Carbon::parse($request->input('start_date')) : Carbon::now()->startOfMonth();
        $endDate = $request->input('end_date') ? Carbon::parse($request->input('end_date')) : Carbon::now();
        
        return Excel::download(new SalesExport($startDate, $endDate), 'sales-report.xlsx');
    }
}
```

### Langkah 24: Menambahkan Fitur Pengaturan Aplikasi

Buat model dan migrasi untuk pengaturan:

```bash
php artisan make:model Setting -m
```

Edit migrasi `create_settings_table.php`:

```php
public function up(): void
{
    Schema::create('settings', function (Blueprint $table) {
        $table->id();
        $table->string('key')->unique();
        $table->text('value')->nullable();
        $table->timestamps();
    });
}
```

Buat controller untuk pengaturan:

```bash
php artisan make:controller SettingController
```

Edit `app/Http/Controllers/SettingController.php`:

```php
<?php

namespace App\Http\Controllers;

use App\Models\Setting;
use Illuminate\Http\Request;

class SettingController extends Controller
{
    public function index()
    {
        $settings = Setting::all()->keyBy('key');
        return view('settings.index', compact('settings'));
    }

    public function update(Request $request)
    {
        $validated = $request->validate([
            'store_name' => 'required|string|max:255',
            'store_email' => 'required|email|max:255',
            'store_phone' => 'required|string|max:20',
            'store_address' => 'required|string',
            'currency' => 'required|string|max:10',
            'low_stock_threshold' => 'required|integer|min:1',
            'allow_backorder' => 'sometimes|boolean',
            'tax_rate' => 'required|numeric|min:0|max:100',
        ]);

        foreach ($validated as $key => $value) {
            Setting::updateOrCreate(
                ['key' => $key],
                ['value' => $value]
            );
        }

        // Handle store logo upload
        if ($request->hasFile('store_logo')) {
            $path = $request->file('store_logo')->store('settings', 'public');
            Setting::updateOrCreate(
                ['key' => 'store_logo'],
                ['value' => $path]
            );
        }

        return redirect()->route('settings.index')
            ->with('success', 'Pengaturan berhasil disimpan.');
    }
}
```

### Langkah 25: Menambahkan Fitur Ekspor/Impor Data

Buat class untuk impor data:

```php
<?php

namespace App\Imports;

use App\Models\Product;
use App\Models\Category;
use Maatwebsite\Excel\Concerns\ToModel;
use Maatwebsite\Excel\Concerns\WithHeadingRow;
use Maatwebsite\Excel\Concerns\WithValidation;
use Illuminate\Support\Str;

class ProductsImport implements ToModel, WithHeadingRow, WithValidation
{
    public function model(array $row)
    {
        // Find or create category
        $category = Category::firstOrCreate(
            ['name' => $row['category']],
            [
                'slug' => Str::slug($row['category']),
                'description' => null,
            ]
        );

        return new Product([
            'name' => $row['name'],
            'slug' => Str::slug($row['name']),
            'description' => $row['description'] ?? null,
            'price' => $row['price'],
            'stock_quantity' => $row['stock_quantity'],
            'sku' => $row['sku'],
            'category_id' => $category->id,
            'is_active' => $row['is_active'] ?? true,
        ]);
    }

    public function rules(): array
    {
        return [
            'name' => 'required|string|max:255',
            'category' => 'required|string|max:255',
            'price' => 'required|numeric|min:0',
            'stock_quantity' => 'required|integer|min:0',
            'sku' => 'required|string|max:50|unique:products,sku',
            'is_active' => 'nullable|boolean',
        ];
    }
}
```

Tambahkan method impor di `ProductController`:

```php
public function import(Request $request)
{
    $request->validate([
        'file' => 'required|mimes:xlsx,xls,csv',
    ]);

    try {
        Excel::import(new ProductsImport, $request->file('file'));
        return redirect()->route('products.index')
            ->with('success', 'Produk berhasil diimpor.');
    } catch (\Maatwebsite\Excel\Validators\ValidationException $e) {
        $failures = $e->failures();
        $errors = [];

        foreach ($failures as $failure) {
            $errors[] = 'Baris ' . $failure->row() . ': ' . implode(', ', $failure->errors());
        }

        return redirect()->back()
            ->withErrors(['import_errors' => $errors]);
    }
}
```

### Langkah 26: Menambahkan Fitur Barcode/QR Code

Install package untuk barcode:

```bash
composer require simplesoftwareio/simple-qrcode
```

Tambahkan method di `ProductController`:

```php
public function generateBarcode(Product $product)
{
    $qrcode = QrCode::size(300)
        ->format('png')
        ->generate(route('products.show', $product));

    return response($qrcode)
        ->header('Content-Type', 'image/png');
}
```

Tambahkan tombol untuk generate barcode di view product:

```blade
<a href="{{ route('products.barcode', $product) }}" class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded" target="_blank">
    Generate QR Code
</a>
```

### Langkah 27: Menambahkan Fitur Cetak Invoice

Buat file `resources/views/orders/invoice.blade.php`:

```blade
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Invoice #{{ $order->order_number }}</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            font-size: 14px;
        }
        .invoice-container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        }
        .invoice-header {
            text-align: center;
            margin-bottom: 20px;
        }
        .invoice-title {
            font-size: 24px;
            font-weight: bold;
        }
        .invoice-details {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
        }
        .invoice-details-left, .invoice-details-right {
            width: 48%;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: left;
        }
        th {
            background-color: #f2f2f2;
        }
        .total-row {
            font-weight: bold;
        }
        .footer {
            text-align: center;
            margin-top: 40px;
            font-size: 12px;
        }
        @media print {
            .no-print {
                display: none;
            }
        }
    </style>
</head>
<body>
    <div class="invoice-container">
        <div class="invoice-header">
            <div class="invoice-title">INVOICE</div>
            <div>{{ config('app.name') }}</div>
        </div>
        
        <div class="invoice-details">
            <div class="invoice-details-left">
                <h3>Ditagihkan Kepada:</h3>
                <p>
                    {{ $order->user->name }}<br>
                    {{ $order->shipping_address }}
                </p>
            </div>
            <div class="invoice-details-right">
                <h3>Detail Invoice:</h3>
                <p>
                    <strong>Invoice:</strong> #{{ $order->order_number }}<br>
                    <strong>Tanggal:</strong> {{ $order->order_date->format('d/m/Y') }}<br>
                    <strong>Status:</strong> {{ ucfirst($order->status) }}<br>
                    <strong>Metode Pembayaran:</strong> {{ $order->payment_method }}
                </p>
            </div>
        </div>
        
        <table>
            <thead>
                <tr>
                    <th>No</th>
                    <th>Produk</th>
                    <th>Harga</th>
                    <th>Jumlah</th>
                    <th>Subtotal</th>
                </tr>
            </thead>
            <tbody>
                @foreach($order->items as $index => $item)
                <tr>
                    <td>{{ $index + 1 }}</td>
                    <td>{{ $item->product->name }}</td>
                    <td>Rp {{ number_format($item->unit_price, 0, ',', '.') }}</td>
                    <td>{{ $item->quantity }}</td>
                    <td>Rp {{ number_format($item->unit_price * $item->quantity, 0, ',', '.') }}</td>
                </tr>
                @endforeach
            </tbody>
            <tfoot>
                <tr class="total-row">
                    <td colspan="4" style="text-align: right">Total:</td>
                    <td>Rp {{ number_format($order->total_amount, 0, ',', '.') }}</td>
                </tr>
            </tfoot>
        </table>
        
        @if($order->notes)
        <div>
            <h3>Catatan:</h3>
            <p>{{ $order->notes }}</p>
        </div>
        @endif
        
        <div class="footer">
            Terima kasih atas pesanan Anda!<br>
            {{ config('app.name') }}
        </div>
        
        <div class="no-print" style="margin-top: 20px; text-align: center;">
            <button onclick="window.print();" style="padding: 10px 20px; background-color: #4CAF50; color: white; border: none; cursor: pointer;">
                Cetak Invoice
            </button>
            <a href="{{ route('orders.show', $order) }}" style="padding: 10px 20px; background-color: #2196F3; color: white; text-decoration: none; margin-left: 10px;">
                Kembali
            </a>
        </div>
    </div>
</body>
</html>
```

Tambahkan route dan method controller:

```php
// Route
Route::get('/orders/{order}/invoice', [OrderController::class, 'invoice'])->name('orders.invoice');

// Controller Method
public function invoice(Order $order)
{
    return view('orders.invoice', compact('order'));
}
```

### Langkah 28: Proteksi CSRF dan Keamanan

Tambahkan middleware CSRF di `app/Http/Kernel.php`:

```php
protected $middlewareGroups = [
    'web' => [
        \App\Http\Middleware\EncryptCookies::class,
        \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
        \Illuminate\Session\Middleware\StartSession::class,
        \Illuminate\View\Middleware\ShareErrorsFromSession::class,
        \App\Http\Middleware\VerifyCsrfToken::class,
        \Illuminate\Routing\Middleware\SubstituteBindings::class,
    ],
];
```

Tambahkan validasi untuk mencegah XSS di `ProductRequest`:

```php
public function rules(): array
{
    $rules = [
        'name' => 'required|string|max:255',
        'category_id' => 'required|exists:categories,id',
        'description' => 'nullable|string',
        'price' => 'required|numeric|min:0',
        'stock_quantity' => 'required|integer|min:0',
        'sku' => 'required|string|max:50',
        'is_active' => 'sometimes|boolean',
        'image' => 'nullable|image|max:2048',
    ];

    // If updating, make unique check except the current product
    if ($this->isMethod('patch') || $this->isMethod('put')) {
        $product = $this->route('product');
        $rules['name'] .= '|unique:products,name,' . $product->id;
        $rules['sku'] .= '|unique:products,sku,' . $product->id;
    } else {
        $rules['name'] .= '|unique:products,name';
        $rules['sku'] .= '
```

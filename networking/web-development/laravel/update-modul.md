# Update Modul

## Sistem Manajemen Inventaris Toko Online dengan Laravel 12

### Daftar Isi

1. Pengenalan
2. Persiapan Lingkungan Pengembangan
3. Instalasi Laravel 12
4. Konfigurasi Database
5. Migrasi Database
6. Model Relasi
7. Authentication & Authorization
8. Fitur Manajemen Produk
9. Fitur Manajemen Kategori
10. Fitur Manajemen Stok
11. Fitur Manajemen Supplier
12. Fitur Manajemen Pesanan
13. Integrasi Payment Gateway
14. Laporan dan Analitik
15. API Development
16. Frontend dengan Livewire
17. Testing
18. Deployment
19. Optimasi dan Best Practices

### Pengenalan

Sistem Manajemen Inventaris Toko Online adalah aplikasi berbasis web yang dikembangkan menggunakan framework Laravel 12. Aplikasi ini dirancang untuk membantu pemilik toko online dalam mengelola inventaris produk, pesanan, pelanggan, dan supplier secara efisien. Fitur-fitur utama yang akan dikembangkan meliputi:

* Manajemen produk dan kategori
* Manajemen stok dan persediaan
* Manajemen supplier
* Manajemen pesanan dan transaksi
* Integrasi payment gateway
* Laporan dan analitik penjualan
* REST API untuk integrasi dengan platform lain

### Persiapan Lingkungan Pengembangan

Sebelum memulai pengembangan, pastikan Anda telah menyiapkan lingkungan pengembangan dengan persyaratan berikut:

#### Persyaratan Sistem

* PHP 8.2 atau yang lebih baru
* Composer
* MySQL/MariaDB
* Node.js dan NPM
* Git

#### Tool Rekomendasi

* Visual Studio Code atau PHPStorm sebagai IDE
* Docker (opsional, untuk pengembangan dengan container)
* Postman atau Insomnia (untuk testing API)
* TablePlus atau DBeaver (untuk manajemen database)

### Instalasi Laravel 12

#### Langkah 1: Instalasi Laravel via Composer

```bash
composer create-project laravel/laravel inventory-system
cd inventory-system
```

#### Langkah 2: Konfigurasi File .env

Salin file `.env.example` menjadi `.env` dan sesuaikan konfigurasi database:

```bash
cp .env.example .env
php artisan key:generate
```

Edit file `.env` untuk konfigurasi database:

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=inventory_system
DB_USERNAME=root
DB_PASSWORD=
```

#### Langkah 3: Instalasi Dependensi Frontend

```bash
npm install
```

### Konfigurasi Database

#### Membuat Database

Buat database baru sesuai dengan konfigurasi pada file `.env`:

```sql
CREATE DATABASE inventory_system CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### Migrasi Database

#### Membuat Tabel Migrasi

Kita akan membuat beberapa tabel utama untuk sistem manajemen inventaris:

**Tabel Categories**

```bash
php artisan make:migration create_categories_table
```

Edit file migrasi untuk tabel categories:

```php
public function up(): void
{
    Schema::create('categories', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->string('slug')->unique();
        $table->text('description')->nullable();
        $table->string('image')->nullable();
        $table->boolean('is_active')->default(true);
        $table->timestamps();
        $table->softDeletes();
    });
}
```

**Tabel Products**

```bash
php artisan make:migration create_products_table
```

Edit file migrasi untuk tabel products:

```php
public function up(): void
{
    Schema::create('products', function (Blueprint $table) {
        $table->id();
        $table->foreignId('category_id')->constrained()->onDelete('cascade');
        $table->string('name');
        $table->string('slug')->unique();
        $table->string('sku')->unique();
        $table->text('description')->nullable();
        $table->decimal('price', 10, 2);
        $table->decimal('cost_price', 10, 2)->nullable();
        $table->integer('stock_quantity')->default(0);
        $table->integer('low_stock_threshold')->default(10);
        $table->boolean('is_active')->default(true);
        $table->json('attributes')->nullable();
        $table->string('image')->nullable();
        $table->timestamps();
        $table->softDeletes();
    });
}
```

**Tabel Suppliers**

```bash
php artisan make:migration create_suppliers_table
```

Edit file migrasi untuk tabel suppliers:

```php
public function up(): void
{
    Schema::create('suppliers', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->string('email')->unique();
        $table->string('phone')->nullable();
        $table->text('address')->nullable();
        $table->string('contact_person')->nullable();
        $table->boolean('is_active')->default(true);
        $table->text('notes')->nullable();
        $table->timestamps();
        $table->softDeletes();
    });
}
```

**Tabel Stock Movements**

```bash
php artisan make:migration create_stock_movements_table
```

Edit file migrasi untuk tabel stock\_movements:

```php
public function up(): void
{
    Schema::create('stock_movements', function (Blueprint $table) {
        $table->id();
        $table->foreignId('product_id')->constrained();
        $table->foreignId('user_id')->constrained();
        $table->string('reference_no');
        $table->enum('type', ['in', 'out', 'adjustment']);
        $table->integer('quantity');
        $table->text('notes')->nullable();
        $table->timestamps();
    });
}
```

**Tabel Orders**

```bash
php artisan make:migration create_orders_table
```

Edit file migrasi untuk tabel orders:

```php
public function up(): void
{
    Schema::create('orders', function (Blueprint $table) {
        $table->id();
        $table->foreignId('user_id')->constrained();
        $table->string('order_number')->unique();
        $table->enum('status', ['pending', 'processing', 'completed', 'cancelled'])->default('pending');
        $table->decimal('total_amount', 10, 2);
        $table->decimal('tax_amount', 10, 2)->default(0);
        $table->decimal('shipping_amount', 10, 2)->default(0);
        $table->decimal('discount_amount', 10, 2)->default(0);
        $table->text('shipping_address')->nullable();
        $table->text('billing_address')->nullable();
        $table->string('payment_method')->nullable();
        $table->string('payment_status')->default('pending');
        $table->string('tracking_number')->nullable();
        $table->json('notes')->nullable();
        $table->timestamps();
        $table->softDeletes();
    });
}
```

**Tabel Order Items**

```bash
php artisan make:migration create_order_items_table
```

Edit file migrasi untuk tabel order\_items:

```php
public function up(): void
{
    Schema::create('order_items', function (Blueprint $table) {
        $table->id();
        $table->foreignId('order_id')->constrained()->onDelete('cascade');
        $table->foreignId('product_id')->constrained();
        $table->integer('quantity');
        $table->decimal('price', 10, 2);
        $table->decimal('discount', 10, 2)->default(0);
        $table->json('options')->nullable();
        $table->timestamps();
    });
}
```

**Menjalankan Migrasi**

```bash
php artisan migrate
```

### Model Relasi

#### Model Category

```bash
php artisan make:model Category
```

Edit file `app/Models/Category.php`:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\SoftDeletes;

class Category extends Model
{
    use HasFactory, SoftDeletes;

    protected $fillable = [
        'name', 'slug', 'description', 'image', 'is_active'
    ];

    public function products()
    {
        return $this->hasMany(Product::class);
    }
}
```

#### Model Product

```bash
php artisan make:model Product
```

Edit file `app/Models/Product.php`:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\SoftDeletes;

class Product extends Model
{
    use HasFactory, SoftDeletes;

    protected $fillable = [
        'category_id', 'name', 'slug', 'sku', 'description', 'price', 
        'cost_price', 'stock_quantity', 'low_stock_threshold', 
        'is_active', 'attributes', 'image'
    ];

    protected $casts = [
        'attributes' => 'array',
        'is_active' => 'boolean',
        'price' => 'decimal:2',
        'cost_price' => 'decimal:2',
    ];

    public function category()
    {
        return $this->belongsTo(Category::class);
    }

    public function stockMovements()
    {
        return $this->hasMany(StockMovement::class);
    }

    public function orderItems()
    {
        return $this->hasMany(OrderItem::class);
    }

    public function isLowStock()
    {
        return $this->stock_quantity <= $this->low_stock_threshold;
    }
}
```

#### Model Supplier

```bash
php artisan make:model Supplier
```

Edit file `app/Models/Supplier.php`:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\SoftDeletes;

class Supplier extends Model
{
    use HasFactory, SoftDeletes;

    protected $fillable = [
        'name', 'email', 'phone', 'address', 'contact_person', 
        'is_active', 'notes'
    ];

    protected $casts = [
        'is_active' => 'boolean',
    ];
}
```

#### Model StockMovement

```bash
php artisan make:model StockMovement
```

Edit file `app/Models/StockMovement.php`:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class StockMovement extends Model
{
    use HasFactory;

    protected $fillable = [
        'product_id', 'user_id', 'reference_no', 'type', 
        'quantity', 'notes'
    ];

    public function product()
    {
        return $this->belongsTo(Product::class);
    }

    public function user()
    {
        return $this->belongsTo(User::class);
    }
}
```

#### Model Order

```bash
php artisan make:model Order
```

Edit file `app/Models/Order.php`:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\SoftDeletes;

class Order extends Model
{
    use HasFactory, SoftDeletes;

    protected $fillable = [
        'user_id', 'order_number', 'status', 'total_amount',
        'tax_amount', 'shipping_amount', 'discount_amount',
        'shipping_address', 'billing_address', 'payment_method',
        'payment_status', 'tracking_number', 'notes'
    ];

    protected $casts = [
        'total_amount' => 'decimal:2',
        'tax_amount' => 'decimal:2',
        'shipping_amount' => 'decimal:2',
        'discount_amount' => 'decimal:2',
        'notes' => 'array',
    ];

    public function user()
    {
        return $this->belongsTo(User::class);
    }

    public function items()
    {
        return $this->hasMany(OrderItem::class);
    }
}
```

#### Model OrderItem

```bash
php artisan make:model OrderItem
```

Edit file `app/Models/OrderItem.php`:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class OrderItem extends Model
{
    use HasFactory;

    protected $fillable = [
        'order_id', 'product_id', 'quantity', 'price', 
        'discount', 'options'
    ];

    protected $casts = [
        'price' => 'decimal:2',
        'discount' => 'decimal:2',
        'options' => 'array',
    ];

    public function order()
    {
        return $this->belongsTo(Order::class);
    }

    public function product()
    {
        return $this->belongsTo(Product::class);
    }
}
```

### Authentication & Authorization

Laravel 12 memiliki beberapa pilihan untuk autentikasi. Kita akan menggunakan Laravel Breeze yang menyediakan implementasi sederhana namun lengkap.

#### Instalasi Laravel Breeze

```bash
composer require laravel/breeze
php artisan breeze:install blade
php artisan migrate
```

#### Peran dan Izin

Kita akan membuat sistem peran dan izin sederhana:

**Membuat Migrasi untuk Roles dan Permissions**

```bash
php artisan make:migration create_roles_table
```

Edit file migrasi roles:

```php
public function up(): void
{
    Schema::create('roles', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->string('slug')->unique();
        $table->string('description')->nullable();
        $table->timestamps();
    });

    Schema::create('permissions', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->string('slug')->unique();
        $table->string('description')->nullable();
        $table->timestamps();
    });

    Schema::create('role_user', function (Blueprint $table) {
        $table->id();
        $table->foreignId('role_id')->constrained()->onDelete('cascade');
        $table->foreignId('user_id')->constrained()->onDelete('cascade');
        $table->timestamps();

        $table->unique(['role_id', 'user_id']);
    });

    Schema::create('permission_role', function (Blueprint $table) {
        $table->id();
        $table->foreignId('permission_id')->constrained()->onDelete('cascade');
        $table->foreignId('role_id')->constrained()->onDelete('cascade');
        $table->timestamps();

        $table->unique(['permission_id', 'role_id']);
    });
}
```

**Model Role dan Permission**

```bash
php artisan make:model Role
php artisan make:model Permission
```

Edit file `app/Models/Role.php`:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Role extends Model
{
    use HasFactory;

    protected $fillable = [
        'name', 'slug', 'description'
    ];

    public function permissions()
    {
        return $this->belongsToMany(Permission::class);
    }

    public function users()
    {
        return $this->belongsToMany(User::class);
    }
}
```

Edit file `app/Models/Permission.php`:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Permission extends Model
{
    use HasFactory;

    protected $fillable = [
        'name', 'slug', 'description'
    ];

    public function roles()
    {
        return $this->belongsToMany(Role::class);
    }
}
```

**Modifikasi Model User**

Edit file `app/Models/User.php` untuk menambahkan relasi dengan roles:

```php
public function roles()
{
    return $this->belongsToMany(Role::class);
}

public function hasRole($role)
{
    if (is_string($role)) {
        return $this->roles->contains('slug', $role);
    }
    
    return !! $role->intersect($this->roles)->count();
}

public function hasPermission($permission)
{
    return $this->roles->map->permissions->flatten()->contains('slug', $permission);
}
```

**Membuat Middleware untuk Authorization**

```bash
php artisan make:middleware CheckRole
```

Edit file `app/Http/Middleware/CheckRole.php`:

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;
use Symfony\Component\HttpFoundation\Response;

class CheckRole
{
    public function handle(Request $request, Closure $next, $role): Response
    {
        if (!$request->user() || !$request->user()->hasRole($role)) {
            abort(403, 'Unauthorized action.');
        }
        
        return $next($request);
    }
}
```

Daftarkan middleware di `app/Http/Kernel.php`:

```php
protected $routeMiddleware = [
    // Middleware lainnya...
    'role' => \App\Http\Middleware\CheckRole::class,
];
```

**Database Seeder untuk Roles dan Permissions**

```bash
php artisan make:seeder RolesAndPermissionsSeeder
```

Edit file `database/seeders/RolesAndPermissionsSeeder.php`:

```php
<?php

namespace Database\Seeders;

use App\Models\Permission;
use App\Models\Role;
use App\Models\User;
use Illuminate\Database\Seeder;
use Illuminate\Support\Facades\Hash;

class RolesAndPermissionsSeeder extends Seeder
{
    public function run(): void
    {
        // Membuat peran
        $adminRole = Role::create([
            'name' => 'Administrator',
            'slug' => 'admin',
            'description' => 'Administrator memiliki akses penuh ke sistem'
        ]);

        $managerRole = Role::create([
            'name' => 'Manager',
            'slug' => 'manager',
            'description' => 'Manager memiliki akses ke manajemen persediaan dan laporan'
        ]);

        $staffRole = Role::create([
            'name' => 'Staff',
            'slug' => 'staff',
            'description' => 'Staff memiliki akses terbatas'
        ]);

        // Membuat izin
        $permissions = [
            ['name' => 'View Dashboard', 'slug' => 'view-dashboard'],
            ['name' => 'Manage Users', 'slug' => 'manage-users'],
            ['name' => 'Create Products', 'slug' => 'create-products'],
            ['name' => 'Edit Products', 'slug' => 'edit-products'],
            ['name' => 'Delete Products', 'slug' => 'delete-products'],
            ['name' => 'View Products', 'slug' => 'view-products'],
            ['name' => 'Manage Categories', 'slug' => 'manage-categories'],
            ['name' => 'Manage Orders', 'slug' => 'manage-orders'],
            ['name' => 'View Reports', 'slug' => 'view-reports'],
            ['name' => 'Manage Settings', 'slug' => 'manage-settings'],
        ];

        foreach ($permissions as $permission) {
            Permission::create($permission);
        }

        // Menetapkan izin ke peran
        $adminRole->permissions()->attach(Permission::all());
        
        $managerRole->permissions()->attach(
            Permission::whereIn('slug', [
                'view-dashboard', 'create-products', 'edit-products', 
                'view-products', 'manage-categories', 'manage-orders', 'view-reports'
            ])->get()
        );
        
        $staffRole->permissions()->attach(
            Permission::whereIn('slug', [
                'view-dashboard', 'view-products', 'view-reports'
            ])->get()
        );

        // Membuat user admin
        $admin = User::create([
            'name' => 'Administrator',
            'email' => 'admin@example.com',
            'password' => Hash::make('password'),
        ]);

        $admin->roles()->attach($adminRole);
    }
}
```

Jalankan seeder:

```bash
php artisan db:seed --class=RolesAndPermissionsSeeder
```

### Fitur Manajemen Produk

#### Controller Produk

```bash
php artisan make:controller ProductController --resource
```

Edit file `app/Http/Controllers/ProductController.php`:

```php
<?php

namespace App\Http\Controllers;

use App\Models\Category;
use App\Models\Product;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Storage;
use Illuminate\Support\Str;

class ProductController extends Controller
{
    public function __construct()
    {
        $this->middleware('role:admin,manager')->except(['index', 'show']);
    }

    public function index()
    {
        $products = Product::with('category')->paginate(10);
        return view('products.index', compact('products'));
    }

    public function create()
    {
        $categories = Category::where('is_active', true)->get();
        return view('products.create', compact('categories'));
    }

    public function store(Request $request)
    {
        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'category_id' => 'required|exists:categories,id',
            'description' => 'nullable|string',
            'price' => 'required|numeric|min:0',
            'cost_price' => 'nullable|numeric|min:0',
            'stock_quantity' => 'required|integer|min:0',
            'low_stock_threshold' => 'nullable|integer|min:0',
            'is_active' => 'boolean',
            'image' => 'nullable|image|max:2048',
        ]);

        if ($request->hasFile('image')) {
            $imagePath = $request->file('image')->store('products', 'public');
            $validated['image'] = $imagePath;
        }

        $validated['slug'] = Str::slug($request->name);
        $validated['sku'] = 'PRD-' . strtoupper(Str::random(8));

        $product = Product::create($validated);

        return redirect()->route('products.index')
            ->with('success', 'Produk berhasil ditambahkan.');
    }

    public function show(Product $product)
    {
        return view('products.show', compact('product'));
    }

    public function edit(Product $product)
    {
        $categories = Category::where('is_active', true)->get();
        return view('products.edit', compact('product', 'categories'));
    }

    public function update(Request $request, Product $product)
    {
        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'category_id' => 'required|exists:categories,id',
            'description' => 'nullable|string',
            'price' => 'required|numeric|min:0',
            'cost_price' => 'nullable|numeric|min:0',
            'stock_quantity' => 'required|integer|min:0',
            'low_stock_threshold' => 'nullable|integer|min:0',
            'is_active' => 'boolean',
            'image' => 'nullable|image|max:2048',
        ]);

        if ($request->hasFile('image')) {
            // Hapus gambar lama jika ada
            if ($product->image) {
                Storage::disk('public')->delete($product->image);
            }
            
            $imagePath = $request->file('image')->store('products', 'public');
            $validated['image'] = $imagePath;
        }

        $validated['slug'] = Str::slug($request->name);

        $product->update($validated);

        return redirect()->route('products.index')
            ->with('success', 'Produk berhasil diperbarui.');
    }

    public function destroy(Product $product)
    {
        // Hapus gambar jika ada
        if ($product->image) {
            Storage::disk('public')->delete($product->image);
        }
        
        $product->delete();

        return redirect()->route('products.index')
            ->with('success', 'Produk berhasil dihapus.');
    }
}
```

#### Views untuk Produk

**products/index.blade.php**

```php
<x-app-layout>
    <x-slot name="header">
        <div class="flex justify-between items-center">
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                {{ __('Produk') }}
            </h2>
            @if(auth()->user()->hasRole('admin') || auth()->user()->hasRole('manager'))
            <a href="{{ route('products.create') }}" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
                Tambah Produk
            </a>
            @endif
        </div>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6 bg-white border-b border-gray-200">
                    @if(session('success'))
                        <div class="bg-green-100 border border-green-400 text-green-700 px-4 py-3 rounded relative mb-4" role="alert">
                            <span class="block sm:inline">{{ session('success') }}</span>
                        </div>
                    @endif

                    <div class="mb-4">
                        <form action="{{ route('products.index') }}" method="GET" class="flex items-center">
                            <input type="text" name="search" placeholder="Cari produk..." class="rounded-md shadow-sm border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50 flex-1 mr-2" value="{{ request('search') }}">
                            <button type="submit" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Cari</button>
                        </form>
                    </div>

                    <div class="overflow-x-auto">
                        <table class="min-w-full divide-y divide-gray-200">
                            <thead class="bg-gray-50">
                                <tr>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Gambar
                                    </th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Nama
                                    </th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Kategori
                                    </th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        SKU
                                    </th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Harga
                                    </th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Stok
                                    </th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Status
                                    </th>
                                    <th scope="col" class="px-6 py-3 text-right text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Aksi
                                    </th>
                                </tr>
                            </thead>
                            <tbody class="bg-white divide-y divide-gray-200">
                                @forelse($products as $product)
                                <tr>
                                    <td class="px-6 py-4 whitespace-nowrap">
                                        @if($product->image)
                                            <img src="{{ asset('storage/' . $product->image) }}" alt="{{ $product->name }}" class="h-10 w-10 object-cover rounded">
                                        @else
                                            <div class="h-10 w-10 bg-gray-200 rounded flex items-center justify-center">
                                                <span class="text-gray-500 text-xs">No img</span>
                                            </div>
                                        @endif
                                    </td>
                                    <td class="px-6 py-4 whitespace-nowrap">
                                        <div class="text-sm font-medium text-gray-900">
                                            {{ $product->name }}
                                        </div>
                                    </td>
                                    <td class="px-6 py-4 whitespace-nowrap">
                                        <div class="text-sm text-gray-900">{{ $product->category->name }}</div>
                                    </td>
                                  <td class="px-6 py-4 whitespace-nowrap">
    <div class="text-sm text-gray-900">{{ $product->sku }}</div>
</td>
<td class="px-6 py-4 whitespace-nowrap">
    <div class="text-sm text-gray-900">Rp {{ number_format($product->price, 0, ',', '.') }}</div>
</td>
<td class="px-6 py-4 whitespace-nowrap">
    <div class="text-sm {{ $product->isLowStock() ? 'text-red-600 font-bold' : 'text-gray-900' }}">
        {{ $product->stock_quantity }}
    </div>
</td>
<td class="px-6 py-4 whitespace-nowrap">
    <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full {{ $product->is_active ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800' }}">
        {{ $product->is_active ? 'Aktif' : 'Nonaktif' }}
    </span>
</td>
<td class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium">
    <a href="{{ route('products.show', $product) }}" class="text-indigo-600 hover:text-indigo-900 mr-2">Detail</a>
    @if(auth()->user()->hasRole('admin') || auth()->user()->hasRole('manager'))
    <a href="{{ route('products.edit', $product) }}" class="text-yellow-600 hover:text-yellow-900 mr-2">Edit</a>
    <form action="{{ route('products.destroy', $product) }}" method="POST" class="inline">
        @csrf
        @method('DELETE')
        <button type="submit" class="text-red-600 hover:text-red-900" onclick="return confirm('Apakah Anda yakin ingin menghapus produk ini?')">Hapus</button>
    </form>
    @endif
</td>
</tr>
@empty
<tr>
    <td colspan="8" class="px-6 py-4 whitespace-nowrap text-center text-gray-500">
        Tidak ada produk yang ditemukan.
    </td>
</tr>
@endforelse
</tbody>
</table>
</div>
<div class="mt-4">
    {{ $products->links() }}
</div>
</div>
</div>
</div>
</div>
</x-app-layout>
```



#### products/index.blade.php&#x20;

```php
<td class="px-6 py-4 whitespace-nowrap">
    <div class="text-sm text-gray-900">{{ $product->sku }}</div>
</td>
<td class="px-6 py-4 whitespace-nowrap">
    <div class="text-sm text-gray-900">Rp {{ number_format($product->price, 0, ',', '.') }}</div>
</td>
<td class="px-6 py-4 whitespace-nowrap">
    <div class="text-sm {{ $product->isLowStock() ? 'text-red-600 font-bold' : 'text-gray-900' }}">
        {{ $product->stock_quantity }}
    </div>
</td>
<td class="px-6 py-4 whitespace-nowrap">
    <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full {{ $product->is_active ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800' }}">
        {{ $product->is_active ? 'Aktif' : 'Nonaktif' }}
    </span>
</td>
<td class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium">
    <a href="{{ route('products.show', $product) }}" class="text-indigo-600 hover:text-indigo-900 mr-2">Detail</a>
    @if(auth()->user()->hasRole('admin') || auth()->user()->hasRole('manager'))
    <a href="{{ route('products.edit', $product) }}" class="text-yellow-600 hover:text-yellow-900 mr-2">Edit</a>
    <form action="{{ route('products.destroy', $product) }}" method="POST" class="inline">
        @csrf
        @method('DELETE')
        <button type="submit" class="text-red-600 hover:text-red-900" onclick="return confirm('Apakah Anda yakin ingin menghapus produk ini?')">Hapus</button>
    </form>
    @endif
</td>
</tr>
@empty
<tr>
    <td colspan="8" class="px-6 py-4 whitespace-nowrap text-center text-gray-500">
        Tidak ada produk yang ditemukan.
    </td>
</tr>
@endforelse
</tbody>
</table>
</div>
<div class="mt-4">
    {{ $products->links() }}
</div>
</div>
</div>
</div>
</div>
</x-app-layout>
```

#### products/create.blade.php

```php
<x-app-layout>
    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            {{ __('Tambah Produk Baru') }}
        </h2>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6 bg-white border-b border-gray-200">
                    <form method="POST" action="{{ route('products.store') }}" enctype="multipart/form-data">
                        @csrf

                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <!-- Nama Produk -->
                            <div>
                                <x-label for="name" :value="__('Nama Produk')" />
                                <x-input id="name" class="block mt-1 w-full" type="text" name="name" :value="old('name')" required autofocus />
                                @error('name')
                                    <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                                @enderror
                            </div>

                            <!-- Kategori -->
                            <div>
                                <x-label for="category_id" :value="__('Kategori')" />
                                <select id="category_id" name="category_id" class="block mt-1 w-full rounded-md shadow-sm border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50">
                                    <option value="">Pilih Kategori</option>
                                    @foreach($categories as $category)
                                        <option value="{{ $category->id }}" {{ old('category_id') == $category->id ? 'selected' : '' }}>
                                            {{ $category->name }}
                                        </option>
                                    @endforeach
                                </select>
                                @error('category_id')
                                    <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                                @enderror
                            </div>

                            <!-- Harga -->
                            <div>
                                <x-label for="price" :value="__('Harga Jual')" />
                                <x-input id="price" class="block mt-1 w-full" type="number" name="price" :value="old('price')" step="0.01" required />
                                @error('price')
                                    <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                                @enderror
                            </div>

                            <!-- Harga Modal -->
                            <div>
                                <x-label for="cost_price" :value="__('Harga Modal')" />
                                <x-input id="cost_price" class="block mt-1 w-full" type="number" name="cost_price" :value="old('cost_price')" step="0.01" />
                                @error('cost_price')
                                    <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                                @enderror
                            </div>

                            <!-- Jumlah Stok -->
                            <div>
                                <x-label for="stock_quantity" :value="__('Jumlah Stok')" />
                                <x-input id="stock_quantity" class="block mt-1 w-full" type="number" name="stock_quantity" :value="old('stock_quantity', 0)" required />
                                @error('stock_quantity')
                                    <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                                @enderror
                            </div>

                            <!-- Batas Stok Rendah -->
                            <div>
                                <x-label for="low_stock_threshold" :value="__('Batas Stok Rendah')" />
                                <x-input id="low_stock_threshold" class="block mt-1 w-full" type="number" name="low_stock_threshold" :value="old('low_stock_threshold', 10)" />
                                @error('low_stock_threshold')
                                    <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                                @enderror
                            </div>

                            <!-- Status -->
                            <div>
                                <label for="is_active" class="flex items-center">
                                    <input id="is_active" type="checkbox" class="rounded border-gray-300 text-indigo-600 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50" name="is_active" value="1" {{ old('is_active', true) ? 'checked' : '' }}>
                                    <span class="ml-2 text-sm text-gray-600">{{ __('Aktif') }}</span>
                                </label>
                            </div>

                            <!-- Gambar -->
                            <div>
                                <x-label for="image" :value="__('Gambar Produk')" />
                                <input id="image" type="file" name="image" class="block mt-1 w-full">
                                @error('image')
                                    <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                                @enderror
                            </div>
                        </div>

                        <!-- Deskripsi -->
                        <div class="mt-4">
                            <x-label for="description" :value="__('Deskripsi')" />
                            <textarea id="description" name="description" rows="4" class="block mt-1 w-full rounded-md shadow-sm border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50">{{ old('description') }}</textarea>
                            @error('description')
                                <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                            @enderror
                        </div>

                        <div class="flex items-center justify-end mt-4">
                            <a href="{{ route('products.index') }}" class="mr-2 bg-gray-500 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded">
                                Batal
                            </a>
                            <x-button>
                                {{ __('Simpan') }}
                            </x-button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</x-app-layout>
```

#### products/edit.blade.php

```php
<x-app-layout>
    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            {{ __('Edit Produk') }}: {{ $product->name }}
        </h2>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6 bg-white border-b border-gray-200">
                    <form method="POST" action="{{ route('products.update', $product) }}" enctype="multipart/form-data">
                        @csrf
                        @method('PUT')

                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <!-- Nama Produk -->
                            <div>
                                <x-label for="name" :value="__('Nama Produk')" />
                                <x-input id="name" class="block mt-1 w-full" type="text" name="name" :value="old('name', $product->name)" required autofocus />
                                @error('name')
                                    <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                                @enderror
                            </div>

                            <!-- Kategori -->
                            <div>
                                <x-label for="category_id" :value="__('Kategori')" />
                                <select id="category_id" name="category_id" class="block mt-1 w-full rounded-md shadow-sm border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50">
                                    <option value="">Pilih Kategori</option>
                                    @foreach($categories as $category)
                                        <option value="{{ $category->id }}" {{ old('category_id', $product->category_id) == $category->id ? 'selected' : '' }}>
                                            {{ $category->name }}
                                        </option>
                                    @endforeach
                                </select>
                                @error('category_id')
                                    <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                                @enderror
                            </div>

                            <!-- Harga -->
                            <div>
                                <x-label for="price" :value="__('Harga Jual')" />
                                <x-input id="price" class="block mt-1 w-full" type="number" name="price" :value="old('price', $product->price)" step="0.01" required />
                                @error('price')
                                    <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                                @enderror
                            </div>

                            <!-- Harga Modal -->
                            <div>
                                <x-label for="cost_price" :value="__('Harga Modal')" />
                                <x-input id="cost_price" class="block mt-1 w-full" type="number" name="cost_price" :value="old('cost_price', $product->cost_price)" step="0.01" />
                                @error('cost_price')
                                    <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                                @enderror
                            </div>

                            <!-- Jumlah Stok -->
                            <div>
                                <x-label for="stock_quantity" :value="__('Jumlah Stok')" />
                                <x-input id="stock_quantity" class="block mt-1 w-full" type="number" name="stock_quantity" :value="old('stock_quantity', $product->stock_quantity)" required />
                                @error('stock_quantity')
                                    <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                                @enderror
                            </div>

                            <!-- Batas Stok Rendah -->
                            <div>
                                <x-label for="low_stock_threshold" :value="__('Batas Stok Rendah')" />
                                <x-input id="low_stock_threshold" class="block mt-1 w-full" type="number" name="low_stock_threshold" :value="old('low_stock_threshold', $product->low_stock_threshold)" />
                                @error('low_stock_threshold')
                                    <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                                @enderror
                            </div>

                            <!-- Status -->
                            <div>
                                <label for="is_active" class="flex items-center">
                                    <input id="is_active" type="checkbox" class="rounded border-gray-300 text-indigo-600 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50" name="is_active" value="1" {{ old('is_active', $product->is_active) ? 'checked' : '' }}>
                                    <span class="ml-2 text-sm text-gray-600">{{ __('Aktif') }}</span>
                                </label>
                            </div>

                            <!-- Gambar -->
                            <div>
                                <x-label for="image" :value="__('Gambar Produk')" />
                                @if($product->image)
                                    <div class="mt-2 mb-2">
                                        <img src="{{ asset('storage/' . $product->image) }}" alt="{{ $product->name }}" class="h-20 w-20 object-cover rounded">
                                    </div>
                                @endif
                                <input id="image" type="file" name="image" class="block mt-1 w-full">
                                <p class="text-xs text-gray-500 mt-1">Biarkan kosong jika tidak ingin mengubah gambar.</p>
                                @error('image')
                                    <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                                @enderror
                            </div>
                        </div>

                        <!-- Deskripsi -->
                        <div class="mt-4">
                            <x-label for="description" :value="__('Deskripsi')" />
                            <textarea id="description" name="description" rows="4" class="block mt-1 w-full rounded-md shadow-sm border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50">{{ old('description', $product->description) }}</textarea>
                            @error('description')
                                <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                            @enderror
                        </div>

                        <div class="flex items-center justify-end mt-4">
                            <a href="{{ route('products.index') }}" class="mr-2 bg-gray-500 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded">
                                Batal
                            </a>
                            <x-button>
                                {{ __('Simpan Perubahan') }}
                            </x-button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</x-app-layout>
```

#### products/show.blade.php

```php
<x-app-layout>
    <x-slot name="header">
        <div class="flex justify-between items-center">
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                {{ __('Detail Produk') }}
            </h2>
            <div>
                <a href="{{ route('products.index') }}" class="bg-gray-500 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded">
                    Kembali
                </a>
                @if(auth()->user()->hasRole('admin') || auth()->user()->hasRole('manager'))
                <a href="{{ route('products.edit', $product) }}" class="bg-yellow-500 hover:bg-yellow-700 text-white font-bold py-2 px-4 rounded ml-2">
                    Edit
                </a>
                @endif
            </div>
        </div>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6 bg-white border-b border-gray-200">
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                        <div class="md:col-span-1">
                            @if($product->image)
                                <img src="{{ asset('storage/' . $product->image) }}" alt="{{ $product->name }}" class="w-full h-auto rounded-lg shadow">
                            @else
                                <div class="w-full h-64 bg-gray-200 rounded-lg flex items-center justify-center">
                                    <span class="text-gray-500">Tidak ada gambar</span>
                                </div>
                            @endif
                        </div>

                        <div class="md:col-span-2">
                            <h1 class="text-2xl font-bold text-gray-800">{{ $product->name }}</h1>
                            <div class="mt-2 flex items-center">
                                <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full {{ $product->is_active ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800' }}">
                                    {{ $product->is_active ? 'Aktif' : 'Nonaktif' }}
                                </span>
                                <span class="ml-2 text-sm text-gray-500">SKU: {{ $product->sku }}</span>
                            </div>

                            <div class="mt-4">
                                <h2 class="text-lg font-semibold text-gray-700">Harga</h2>
                                <div class="flex items-baseline">
                                    <span class="text-2xl font-bold text-gray-900">Rp {{ number_format($product->price, 0, ',', '.') }}</span>
                                    @if($product->cost_price)
                                        <span class="ml-4 text-sm text-gray-500">Harga Modal: Rp {{ number_format($product->cost_price, 0, ',', '.') }}</span>
                                    @endif
                                </div>
                            </div>

                            <div class="mt-4">
                                <h2 class="text-lg font-semibold text-gray-700">Kategori</h2>
                                <p class="text-gray-600">{{ $product->category->name }}</p>
                            </div>

                            <div class="mt-4">
                                <h2 class="text-lg font-semibold text-gray-700">Stok</h2>
                                <p class="text-gray-600 {{ $product->isLowStock() ? 'text-red-600 font-bold' : '' }}">
                                    {{ $product->stock_quantity }} unit
                                    @if($product->isLowStock())
                                        <span class="text-red-600">(Stok Rendah)</span>
                                    @endif
                                </p>
                                <p class="text-sm text-gray-500">Batas Stok Rendah: {{ $product->low_stock_threshold }} unit</p>
                            </div>

                            @if($product->description)
                                <div class="mt-4">
                                    <h2 class="text-lg font-semibold text-gray-700">Deskripsi</h2>
                                    <div class="prose max-w-none mt-2">
                                        {{ $product->description }}
                                    </div>
                                </div>
                            @endif

                            <div class="mt-6">
                                <h2 class="text-lg font-semibold text-gray-700">Informasi Tambahan</h2>
                                <div class="mt-2 grid grid-cols-2 gap-4">
                                    <div>
                                        <p class="text-sm text-gray-500">Dibuat pada</p>
                                        <p class="text-gray-600">{{ $product->created_at->format('d M Y H:i') }}</p>
                                    </div>
                                    <div>
                                        <p class="text-sm text-gray-500">Terakhir diperbarui</p>
                                        <p class="text-gray-600">{{ $product->updated_at->format('d M Y H:i') }}</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</x-app-layout>
```

### Fitur Manajemen Kategori

#### Controller Kategori

```bash
php artisan make:controller CategoryController --resource
```

Edit file `app/Http/Controllers/CategoryController.php`:

```php
<?php

namespace App\Http\Controllers;

use App\Models\Category;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Storage;
use Illuminate\Support\Str;

class CategoryController extends Controller
{
    public function __construct()
    {
        $this->middleware('role:admin,manager')->except(['index', 'show']);
    }

    public function index()
    {
        $categories = Category::withCount('products')
            ->orderBy('name')
            ->paginate(10);
        return view('categories.index', compact('categories'));
    }

    public function create()
    {
        return view('categories.create');
    }

    public function store(Request $request)
    {
        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'description' => 'nullable|string',
            'is_active' => 'boolean',
            'image' => 'nullable|image|max:2048',
        ]);

        if ($request->hasFile('image')) {
            $imagePath = $request->file('image')->store('categories', 'public');
            $validated['image'] = $imagePath;
        }

        $validated['slug'] = Str::slug($request->name);

        Category::create($validated);

        return redirect()->route('categories.index')
            ->with('success', 'Kategori berhasil ditambahkan.');
    }

    public function show(Category $category)
    {
        $products = $category->products()->paginate(10);
        return view('categories.show', compact('category', 'products'));
    }

    public function edit(Category $category)
    {
        return view('categories.edit', compact('category'));
    }

    public function update(Request $request, Category $category)
    {
        $validated = $request->validate([
            'name' => 'required|string|max:255',
            'description' => 'nullable|string',
            'is_active' => 'boolean',
            'image' => 'nullable|image|max:2048',
        ]);

        if ($request->hasFile('image')) {
            // Hapus gambar lama jika ada
            if ($category->image) {
                Storage::disk('public')->delete($category->image);
            }
            
            $imagePath = $request->file('image')->store('categories', 'public');
            $validated['image'] = $imagePath;
        }

        $validated['slug'] = Str::slug($request->name);

        $category->update($validated);

        return redirect()->route('categories.index')
            ->with('success', 'Kategori berhasil diperbarui.');
    }

    public function destroy(Category $category)
    {
        // Periksa apakah kategori memiliki produk
        if ($category->products()->count() > 0) {
            return redirect()->route('categories.index')
                ->with('error', 'Kategori tidak dapat dihapus karena masih memiliki produk terkait.');
        }

        // Hapus gambar jika ada
        if ($category->image) {
            Storage::disk('public')->delete($category->image);
        }
        
        $category->delete();

        return redirect()->route('categories.index')
            ->with('success', 'Kategori berhasil dihapus.');
    }
}
```

#### Views untuk Kategori

**categories/index.blade.php**

```php
<x-app-layout>
    <x-slot name="header">
        <div class="flex justify-between items-center">
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                {{ __('Kategori Produk') }}
            </h2>
            @if(auth()->user()->hasRole('admin') || auth()->user()->hasRole('manager'))
            <a href="{{ route('categories.create') }}" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
                Tambah Kategori
            </a>
            @endif
        </div>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6 bg-white border-b border-gray-200">
                    @if(session('success'))
                        <div class="bg-green-100 border border-green-400 text-green-700 px-4 py-3 rounded relative mb-4" role="alert">
                            <span class="block sm:inline">{{ session('success') }}</span>
                        </div>
                    @endif

                    @if(session('error'))
                        <div class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative mb-4" role="alert">
                            <span class="block sm:inline">{{ session('error') }}</span>
                        </div>
                    @endif

                    <div class="overflow-x-auto">
                        <table class="min-w-full divide-y divide-gray-200">
                            <thead class="bg-gray-50">
                                <tr>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Gambar
                                    </th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Nama
                                    </th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Jumlah Produk
                                    </th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Status
                                    </th>
                                    <th scope="col" class="px-6 py-3 text-right text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Aksi
                                    </th>
                                </tr>
                            </thead>
                            <tbody class="bg-white divide-y divide-gray-200">
                                @forelse($categories as $category)
                                <tr>
                                    <td class="px-6 py-4 whitespace-nowrap">
                                        @if($category->image)
                                            <img src="{{ asset('storage/' . $category->image) }}" alt="{{ $category->name }}" class="h-10 w-10 object-cover rounded">
                                        @else
                                            <div class="h-10 w-10 bg-gray-200 rounded flex items-center justify-center">
                                                <span class="text-gray-500 text-xs">No img</span>
                                            </div>
                                        @endif
                                    </td>
                                    <td class="px-6 py-4 whitespace-nowrap">
                                        <div class="text-sm font-medium text-gray-900">
                                            {{ $category->name }}
                                        </div>
                                    </td>
                                    <td class="px-6 py-4 whitespace-nowrap">
                                        <div class="text-sm text-gray-900">{{ $category->products_count }}</div>
                                    </td>
                                    <td class="px-6 py-4 whitespace-nowrap">
                                        <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full {{ $category->is_active ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800' }}">
                                            {{ $category->is_active ? 'Aktif' : 'Nonaktif' }}
                                        </span>
                                    </td>
                                    <!-- categories/index.blade.php (lanjutan) -->
                                    <td class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium">
                                        <a href="{{ route('categories.show', $category) }}" class="text-indigo-600 hover:text-indigo-900 mr-2">Detail</a>
                                        @if(auth()->user()->hasRole('admin') || auth()->user()->hasRole('manager'))
                                        <a href="{{ route('categories.edit', $category) }}" class="text-yellow-600 hover:text-yellow-900 mr-2">Edit</a>
                                        <form action="{{ route('categories.destroy', $category) }}" method="POST" class="inline">
                                            @csrf
                                            @method('DELETE')
                                            <button type="submit" class="text-red-600 hover:text-red-900" onclick="return confirm('Apakah Anda yakin ingin menghapus kategori ini?')">Hapus</button>
                                        </form>
                                        @endif
                                    </td>
                                </tr>
                                @empty
                                <tr>
                                    <td colspan="5" class="px-6 py-4 whitespace-nowrap text-center text-gray-500">
                                        Tidak ada kategori yang ditemukan.
                                    </td>
                                </tr>
                                @endforelse
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

```php
<!-- categories/index.blade.php (lanjutan) -->
                                    <td class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium">
                                        <a href="{{ route('categories.show', $category) }}" class="text-indigo-600 hover:text-indigo-900 mr-2">Detail</a>
                                        @if(auth()->user()->hasRole('admin') || auth()->user()->hasRole('manager'))
                                        <a href="{{ route('categories.edit', $category) }}" class="text-yellow-600 hover:text-yellow-900 mr-2">Edit</a>
                                        <form action="{{ route('categories.destroy', $category) }}" method="POST" class="inline">
                                            @csrf
                                            @method('DELETE')
                                            <button type="submit" class="text-red-600 hover:text-red-900" onclick="return confirm('Apakah Anda yakin ingin menghapus kategori ini?')">Hapus</button>
                                        </form>
                                        @endif
                                    </td>
                                </tr>
                                @empty
                                <tr>
                                    <td colspan="5" class="px-6 py-4 whitespace-nowrap text-center text-gray-500">
                                        Tidak ada kategori yang ditemukan.
                                    </td>
                                </tr>
                                @endforelse
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

Mari saya lanjutkan dengan view lainnya untuk manajemen kategori dan fitur-fitur berikutnya:

### Fitur Manajemen Kategori (lanjutan)

#### Views untuk Kategori (lanjutan)

**categories/create.blade.php**

```php
<x-app-layout>
    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            {{ __('Tambah Kategori Baru') }}
        </h2>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6 bg-white border-b border-gray-200">
                    <form method="POST" action="{{ route('categories.store') }}" enctype="multipart/form-data">
                        @csrf

                        <!-- Nama Kategori -->
                        <div class="mb-4">
                            <x-label for="name" :value="__('Nama Kategori')" />
                            <x-input id="name" class="block mt-1 w-full" type="text" name="name" :value="old('name')" required autofocus />
                            @error('name')
                                <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                            @enderror
                        </div>

                        <!-- Status -->
                        <div class="mb-4">
                            <label for="is_active" class="flex items-center">
                                <input id="is_active" type="checkbox" class="rounded border-gray-300 text-indigo-600 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50" name="is_active" value="1" {{ old('is_active', true) ? 'checked' : '' }}>
                                <span class="ml-2 text-sm text-gray-600">{{ __('Aktif') }}</span>
                            </label>
                        </div>

                        <!-- Gambar -->
                        <div class="mb-4">
                            <x-label for="image" :value="__('Gambar Kategori')" />
                            <input id="image" type="file" name="image" class="block mt-1 w-full">
                            @error('image')
                                <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                            @enderror
                        </div>

                        <!-- Deskripsi -->
                        <div class="mb-4">
                            <x-label for="description" :value="__('Deskripsi')" />
                            <textarea id="description" name="description" rows="4" class="block mt-1 w-full rounded-md shadow-sm border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50">{{ old('description') }}</textarea>
                            @error('description')
                                <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                            @enderror
                        </div>

                        <div class="flex items-center justify-end mt-4">
                            <a href="{{ route('categories.index') }}" class="mr-2 bg-gray-500 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded">
                                Batal
                            </a>
                            <x-button>
                                {{ __('Simpan') }}
                            </x-button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</x-app-layout>
```

**categories/edit.blade.php**

```php
<x-app-layout>
    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            {{ __('Edit Kategori') }}: {{ $category->name }}
        </h2>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6 bg-white border-b border-gray-200">
                    <form method="POST" action="{{ route('categories.update', $category) }}" enctype="multipart/form-data">
                        @csrf
                        @method('PUT')

                        <!-- Nama Kategori -->
                        <div class="mb-4">
                            <x-label for="name" :value="__('Nama Kategori')" />
                            <x-input id="name" class="block mt-1 w-full" type="text" name="name" :value="old('name', $category->name)" required autofocus />
                            @error('name')
                                <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                            @enderror
                        </div>

                        <!-- Status -->
                        <div class="mb-4">
                            <label for="is_active" class="flex items-center">
                                <input id="is_active" type="checkbox" class="rounded border-gray-300 text-indigo-600 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50" name="is_active" value="1" {{ old('is_active', $category->is_active) ? 'checked' : '' }}>
                                <span class="ml-2 text-sm text-gray-600">{{ __('Aktif') }}</span>
                            </label>
                        </div>

                        <!-- Gambar -->
                        <div class="mb-4">
                            <x-label for="image" :value="__('Gambar Kategori')" />
                            @if($category->image)
                                <div class="mt-2 mb-2">
                                    <img src="{{ asset('storage/' . $category->image) }}" alt="{{ $category->name }}" class="h-20 w-20 object-cover rounded">
                                </div>
                            @endif
                            <input id="image" type="file" name="image" class="block mt-1 w-full">
                            <p class="text-xs text-gray-500 mt-1">Biarkan kosong jika tidak ingin mengubah gambar.</p>
                            @error('image')
                                <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                            @enderror
                        </div>

                        <!-- Deskripsi -->
                        <div class="mb-4">
                            <x-label for="description" :value="__('Deskripsi')" />
                            <textarea id="description" name="description" rows="4" class="block mt-1 w-full rounded-md shadow-sm border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50">{{ old('description', $category->description) }}</textarea>
                            @error('description')
                                <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                            @enderror
                        </div>

                        <div class="flex items-center justify-end mt-4">
                            <a href="{{ route('categories.index') }}" class="mr-2 bg-gray-500 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded">
                                Batal
                            </a>
                            <x-button>
                                {{ __('Simpan Perubahan') }}
                            </x-button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</x-app-layout>
```

**categories/show.blade.php**

```php
<x-app-layout>
    <x-slot name="header">
        <div class="flex justify-between items-center">
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                {{ __('Detail Kategori') }}: {{ $category->name }}
            </h2>
            <div>
                <a href="{{ route('categories.index') }}" class="bg-gray-500 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded">
                    Kembali
                </a>
                @if(auth()->user()->hasRole('admin') || auth()->user()->hasRole('manager'))
                <a href="{{ route('categories.edit', $category) }}" class="bg-yellow-500 hover:bg-yellow-700 text-white font-bold py-2 px-4 rounded ml-2">
                    Edit
                </a>
                @endif
            </div>
        </div>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6 bg-white border-b border-gray-200">
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                        <div class="md:col-span-1">
                            @if($category->image)
                                <img src="{{ asset('storage/' . $category->image) }}" alt="{{ $category->name }}" class="w-full h-auto rounded-lg shadow">
                            @else
                                <div class="w-full h-64 bg-gray-200 rounded-lg flex items-center justify-center">
                                    <span class="text-gray-500">Tidak ada gambar</span>
                                </div>
                            @endif
                        </div>

                        <div class="md:col-span-2">
                            <h1 class="text-2xl font-bold text-gray-800">{{ $category->name }}</h1>
                            <div class="mt-2 flex items-center">
                                <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full {{ $category->is_active ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800' }}">
                                    {{ $category->is_active ? 'Aktif' : 'Nonaktif' }}
                                </span>
                            </div>

                            @if($category->description)
                                <div class="mt-4">
                                    <h2 class="text-lg font-semibold text-gray-700">Deskripsi</h2>
                                    <div class="prose max-w-none mt-2">
                                        {{ $category->description }}
                                    </div>
                                </div>
                            @endif

                            <div class="mt-6">
                                <h2 class="text-lg font-semibold text-gray-700">Informasi Tambahan</h2>
                                <div class="mt-2 grid grid-cols-2 gap-4">
                                    <div>
                                        <p class="text-sm text-gray-500">Jumlah Produk</p>
                                        <p class="text-gray-600">{{ $category->products->count() }}</p>
                                    </div>
                                    <div>
                                        <p class="text-sm text-gray-500">Dibuat pada</p>
                                        <p class="text-gray-600">{{ $category->created_at->format('d M Y H:i') }}</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Daftar Produk -->
                    <div class="mt-8">
                        <h2 class="text-xl font-semibold text-gray-800 mb-4">Produk dalam Kategori Ini</h2>
                        
                        @if($products->count() > 0)
                            <div class="overflow-x-auto">
                                <table class="min-w-full divide-y divide-gray-200">
                                    <thead class="bg-gray-50">
                                        <tr>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                                Gambar
                                            </th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                                Nama
                                            </th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                                SKU
                                            </th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                                Harga
                                            </th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                                Stok
                                            </th>
                                            <th scope="col" class="px-6 py-3 text-right text-xs font-medium text-gray-500 uppercase tracking-wider">
                                                Aksi
                                            </th>
                                        </tr>
                                    </thead>
                                    <tbody class="bg-white divide-y divide-gray-200">
                                        @foreach($products as $product)
                                        <tr>
                                            <td class="px-6 py-4 whitespace-nowrap">
                                                @if($product->image)
                                                    <img src="{{ asset('storage/' . $product->image) }}" alt="{{ $product->name }}" class="h-10 w-10 object-cover rounded">
                                                @else
                                                    <div class="h-10 w-10 bg-gray-200 rounded flex items-center justify-center">
                                                        <span class="text-gray-500 text-xs">No img</span>
                                                    </div>
                                                @endif
                                            </td>
                                            <td class="px-6 py-4 whitespace-nowrap">
                                                <div class="text-sm font-medium text-gray-900">
                                                    {{ $product->name }}
                                                </div>
                                            </td>
                                            <td class="px-6 py-4 whitespace-nowrap">
                                                <div class="text-sm text-gray-900">{{ $product->sku }}</div>
                                            </td>
                                            <td class="px-6 py-4 whitespace-nowrap">
                                                <div class="text-sm text-gray-900">Rp {{ number_format($product->price, 0, ',', '.') }}</div>
                                            </td>
                                            <td class="px-6 py-4 whitespace-nowrap">
                                                <div class="text-sm {{ $product->isLowStock() ? 'text-red-600 font-bold' : 'text-gray-900' }}">
                                                    {{ $product->stock_quantity }}
                                                </div>
                                            </td>
                                            <td class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium">
                                                <a href="{{ route('products.show', $product) }}" class="text-indigo-600 hover:text-indigo-900">Detail</a>
                                            </td>
                                        </tr>
                                        @endforeach
                                    </tbody>
                                </table>
                            </div>
                            <div class="mt-4">
                                {{ $products->links() }}
                            </div>
                        @else
                            <div class="bg-gray-100 p-4 rounded text-gray-700 text-center">
                                Tidak ada produk dalam kategori ini.
                            </div>
                        @endif
                    </div>
                </div>
            </div>
        </div>
    </div>
</x-app-layout>
```

### Fitur Manajemen Stok

#### Controller Stok

```bash
php artisan make:controller StockMovementController
```

Edit file `app/Http/Controllers/StockMovementController.php`:

```php
<?php

namespace App\Http\Controllers;

use App\Models\Product;
use App\Models\StockMovement;
use Illuminate\Http\Request;
use Illuminate\Support\Str;

class StockMovementController extends Controller
{
    public function __construct()
    {
        $this->middleware('role:admin,manager');
    }

    public function index()
    {
        $stockMovements = StockMovement::with(['product', 'user'])
            ->orderBy('created_at', 'desc')
            ->paginate(15);
        
        return view('stock.index', compact('stockMovements'));
    }

    public function create()
    {
        $products = Product::where('is_active', true)->orderBy('name')->get();
        return view('stock.create', compact('products'));
    }

    public function store(Request $request)
    {
        $validated = $request->validate([
            'product_id' => 'required|exists:products,id',
            'type' => 'required|in:in,out,adjustment',
            'quantity' => 'required|integer|min:1',
            'notes' => 'nullable|string',
        ]);

        $product = Product::findOrFail($request->product_id);
        $validated['user_id'] = auth()->id();
        $validated['reference_no'] = 'STK-' . strtoupper(Str::random(8));

        // Update stok produk berdasarkan tipe pergerakan stok
        switch ($request->type) {
            case 'in':
                $product->stock_quantity += $request->quantity;
                break;
            case 'out':
                if ($product->stock_quantity < $request->quantity) {
                    return redirect()->back()
                        ->withInput()
                        ->withErrors(['quantity' => 'Jumlah stok tidak mencukupi untuk pengeluaran']);
                }
                $product->stock_quantity -= $request->quantity;
                break;
            case 'adjustment':
                // Adjustment langsung mengubah kuantitas stok ke nilai baru
                $validated['quantity'] = $request->quantity - $product->stock_quantity;
                $product->stock_quantity = $request->quantity;
                break;
        }

        $product->save();
        StockMovement::create($validated);

        return redirect()->route('stock.index')
            ->with('success', 'Pergerakan stok berhasil dicatat.');
    }

    public function show(StockMovement $stockMovement)
    {
        return view('stock.show', compact('stockMovement'));
    }

    public function productHistory($productId)
    {
        $product = Product::findOrFail($productId);
        $stockMovements = StockMovement::with('user')
            ->where('product_id', $productId)
            ->orderBy('created_at', 'desc')
            ->paginate(15);

        return view('stock.product-history', compact('product', 'stockMovements'));
    }

    public function lowStock()
    {
        $lowStockProducts = Product::where('is_active', true)
            ->whereRaw('stock_quantity <= low_stock_threshold')
            ->orderBy('stock_quantity')
            ->paginate(15);

        return view('stock.low-stock', compact('lowStockProducts'));
    }
}
```

#### Views untuk Manajemen Stok

**stock/index.blade.php**

```php
<x-app-layout>
    <x-slot name="header">
        <div class="flex justify-between items-center">
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                {{ __('Riwayat Pergerakan Stok') }}
            </h2>
            <div>
                <a href="{{ route('stock.create') }}" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
                    Tambah Pergerakan Stok
                </a>
                <a href="{{ route('stock.low-stock') }}" class="ml-2 bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded">
                    Produk Stok Rendah
                </a>
            </div>
        </div>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6 bg-white border-b border-gray-200">
                    @if(session('success'))
                        <div class="bg-green-100 border border-green-400 text-green-700 px-4 py-3 rounded relative mb-4" role="alert">
                            <span class="block sm:inline">{{ session('success') }}</span>
                        </div>
                    @endif

                    <div class="overflow-x-auto">
                        <table class="min-w-full divide-y divide-gray-200">
                            <thead class="bg-gray-50">
                                <tr>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Referensi
                                    </th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Produk
                                    </th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Tipe
                                    </th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Kuantitas
                                    </th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        User
                                    </th>
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Tanggal
                                    </th>
                                    <th scope="col" class="px-6 py-3 text-right text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Aksi
                                    </th>
                                </tr>
                            </thead>
                            <tbody class="bg-white divide-y divide-gray-200">
                                @forelse($stockMovements as $movement)
                                <tr>
                                    <td class="px-6 py-4 whitespace-nowrap">
                                        <div class="text-sm font-medium text-gray-900">
                                            {{ $movement->reference_no }}
                                        </div>
                                    </td>
                                    <td class="px-6 py-4 whitespace-nowrap">
                                        <div class="text-sm text-gray-900">{{ $movement->product->name }}</div>
                                    </td>
                                    <td class="px-6 py-4 whitespace-nowrap">
                                        <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full 
                                            {{ $movement->type == 'in' ? 'bg-green-100 text-green-800' : 
                                               ($movement->type == 'out' ? 'bg-red-100 text-red-800' : 'bg-yellow-100 text-yellow-800') }}">
                                            {{ $movement->type == 'in' ? 'Masuk' : ($movement->type == 'out' ? 'Keluar' : 'Penyesuaian') }}
                                        </span>
                                    </td>
                                    <td class="px-6 py-4 whitespace-nowrap">
                                        <div class="text-sm 
                                            {{ $movement->type == 'in' ? 'text-green-600' : 
                                               ($movement->type == 'out' ? 'text-red-600' : 'text-yellow-600') }}">
                                            {{ $movement->type == 'out' ? '-' : '+' }}{{ abs($movement->quantity) }}
                                        </div>
                                    </td>
                                    <td class="px-6 py-4 whitespace-nowrap">
                                        <div class="text-sm text-gray-900">{{ $movement->user->name }}</div>
                                    </td>
                                    <td class="px-6 py-4 whitespace-nowrap">
                                        <div class="text-sm text-gray-900">{{ $movement->created_at->format('d M Y H:i') }}</div>
                                    </td>
                                    <td class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium">
                                        <a href="{{ route('stock.show', $movement) }}" class="text-indigo-600 hover:text-indigo-900">Detail</a>
                                    </td>
                                </tr>
                                @empty
                                <tr>
                                    <td colspan="7" class="px-6 py-4 whitespace-nowrap text-center text-gray-500">
                                        Tidak ada data pergerakan stok.
                                    </td>
                                </tr>
                                @endforelse
                            </tbody>
                        </table>
                    </div>
                    <div class="mt-4">
                        {{ $stockMovements->links() }}
                    </div>
                </div>
            </div>
        </div>
    </div>
</x-app-layout>
```

**stock/create.blade.php**

```php
<x-app-layout>
    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            {{ __('Tambah Pergerakan Stok') }}
        </h2>
    </x-slot>

    <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
            <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                <div class="p-6 bg-white border-b border-gray-200">
                    <form method="POST" action="{{ route('stock.store') }}">
                        @csrf

                        <!-- Produk -->
                        <div class="mb-4">
                            <x-label for="product_id" :value="__('Produk')" />
                            <select id="product_id" name="product_id" class="block mt-1 w-full rounded-md shadow-sm border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50">
                                <option value="">Pilih Produk</option>
                                @foreach($products as $product)
                                    <option value="{{ $product->id }}" data-stock="{{ $product->stock_quantity }}" {{ old('product_id') == $product->id ? 'selected' : '' }}>
                                        {{ $product->name }} (Stok: {{ $product->stock_quantity }})
                                    </option>
                                @endforeach
                            </select>
                            @error('product_id')
                                <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                            @enderror
                        </div>

                        <!-- Tipe Pergerakan -->
                        <div class="mb-4">
                            <x-label for="type" :value="__('Tipe Pergerakan')" />
                            <select id="type" name="type" class="block mt-1 w-full rounded-md shadow-sm border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50">
                                <option value="in" {{ old('type') == 'in' ? 'selected' : '' }}>Stok Masuk</option>
                                <option value="out" {{ old('type') == 'out' ? 'selected' : '' }}>Stok Keluar</option>
                                <option value="adjustment" {{ old('type') == 'adjustment' ? 'selected' : '' }}>Penyesuaian Stok</option>
                            </select>
                            @error('type')
                                <p class="text-red-500 text-xs mt-1">{{ $message }}</p>
                            @enderror
                        </div>

                        <!-- Kuantitas -->
                        <div class="mb-4">
                            <x-label for="quantity" :value="__('Kuantitas')" />
                            <x-input id="quantity" class="block mt-1 w-full" type="
```

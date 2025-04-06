# Latihan Project

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

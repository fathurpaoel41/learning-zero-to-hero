# PERTEMUAN 9 — Laravel Intro

## 📚 Materi

### 1. Pengenalan Laravel
Laravel adalah framework PHP yang powerful dan elegan untuk pengembangan aplikasi web modern.

**Keuntungan Laravel:**
- Syntax yang clean dan mudah dibaca
- Built-in authentication dan authorization
- Eloquent ORM untuk database
- Blade templating engine
- Routing yang powerful
- Artisan CLI untuk otomasi
- Community yang besar

### 2. Instalasi Laravel

**Persyaratan:**
- PHP >= 8.1
- Composer (dependency manager untuk PHP)
- MySQL/MariaDB

**Instalasi via Composer:**
```bash
composer create-project laravel/laravel nama-project
```

**Contoh:**
```bash
composer create-project laravel/laravel belajar-laravel
cd belajar-laravel
```

**Menjalankan Development Server:**
```bash
php artisan serve
```
Server akan berjalan di `http://localhost:8000`

### 3. Folder Structure Laravel

```
belajar-laravel/
├── app/                    # Core application code
│   ├── Http/
│   │   └── Controllers/   # Controllers
│   └── Models/            # Models
├── bootstrap/             # Bootstrap files
├── config/                # Configuration files
├── database/
│   ├── migrations/        # Database migrations
│   └── seeders/           # Database seeders
├── public/                # Public assets (index.php)
├── resources/
│   ├── views/             # Blade templates
│   └── js/                # JavaScript files
├── routes/
│   ├── web.php            # Web routes
│   └── api.php            # API routes
├── storage/               # Storage files
├── tests/                 # Test files
├── vendor/                # Composer dependencies
├── .env                   # Environment configuration
├── artisan                # Artisan CLI
└── composer.json          # Composer dependencies
```

### 4. File .env
File `.env` menyimpan konfigurasi environment seperti database, mail, dll.

**Contoh .env:**
```env
APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:...
APP_DEBUG=true
APP_URL=http://localhost

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=belajar_laravel
DB_USERNAME=root
DB_PASSWORD=
```

**⚠️ PENTING:** Jangan commit file `.env` ke git! File `.env.example` adalah template.

### 5. Route Dasar

**File: `routes/web.php`**

**Route Sederhana:**
```php
<?php

use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return 'Hello World';
});
```

**Route dengan Parameter:**
```php
Route::get('/user/{id}', function ($id) {
    return 'User ID: ' . $id;
});
```

**Route dengan Optional Parameter:**
```php
Route::get('/user/{id?}', function ($id = null) {
    return 'User ID: ' . ($id ?? 'Tidak ada');
});
```

**Route dengan Named Route:**
```php
Route::get('/hello', function () {
    return 'Hello Laravel';
})->name('hello');
```

**Mengakses Named Route:**
```php
// Di Blade
<a href="{{ route('hello') }}">Hello</a>

// Di Controller
return redirect()->route('hello');
```

### 6. Route Methods

```php
// GET - Untuk mengambil data
Route::get('/users', function () {
    return 'List Users';
});

// POST - Untuk mengirim data
Route::post('/users', function () {
    return 'Create User';
});

// PUT/PATCH - Untuk update data
Route::put('/users/{id}', function ($id) {
    return 'Update User ' . $id;
});

// DELETE - Untuk hapus data
Route::delete('/users/{id}', function ($id) {
    return 'Delete User ' . $id;
});
```

### 7. Route dengan Multiple Methods

```php
Route::match(['get', 'post'], '/users', function () {
    return 'Users';
});

Route::any('/users', function () {
    return 'Users';
});
```

### 8. Artisan Commands

Artisan adalah command-line interface Laravel.

**Command Umum:**
```bash
# Menjalankan server
php artisan serve

# Membuat controller
php artisan make:controller UserController

# Membuat model
php artisan make:model User

# Membuat migration
php artisan make:migration create_users_table

# Menjalankan migration
php artisan migrate

# List semua routes
php artisan route:list

# Clear cache
php artisan cache:clear
php artisan config:clear
php artisan view:clear
```

### 9. Contoh Lengkap: Route Hello

**File: `routes/web.php`**
```php
<?php

use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return view('welcome');
});

Route::get('/hello', function () {
    return 'Hello, Welcome to Laravel!';
});

Route::get('/hello/{name}', function ($name) {
    return 'Hello, ' . $name . '!';
});
```

**Test di Browser:**
- `http://localhost:8000/` → Welcome page
- `http://localhost:8000/hello` → "Hello, Welcome to Laravel!"
- `http://localhost:8000/hello/Budi` → "Hello, Budi!"

---

## 📝 Pretest (3 Soal Ringkas)

1. **Apa itu Laravel?**
   - Jawab: Laravel adalah framework PHP yang powerful dan elegan untuk pengembangan aplikasi web modern. Laravel menyediakan banyak fitur built-in seperti routing, ORM, authentication, dan templating engine.

2. **Folder apa yang menyimpan controller?**
   - Jawab: Controller disimpan di folder `app/Http/Controllers/`. Controller digunakan untuk menangani logika aplikasi dan request dari user.

3. **Apa fungsi file .env?**
   - Jawab: File `.env` menyimpan konfigurasi environment seperti database connection, app name, app URL, dan konfigurasi lainnya. File ini tidak boleh di-commit ke git karena berisi informasi sensitif.

---

## 🏠 PR (Pekerjaan Rumah)

### Tugas 1: Setup Project Laravel
Setup project Laravel baru dengan nama `belajar-laravel`.

**Langkah:**
```bash
# 1. Install Laravel
composer create-project laravel/laravel belajar-laravel

# 2. Masuk ke folder project
cd belajar-laravel

# 3. Copy .env.example ke .env
cp .env.example .env

# 4. Generate app key
php artisan key:generate

# 5. Konfigurasi database di .env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=belajar_laravel
DB_USERNAME=root
DB_PASSWORD=

# 6. Jalankan server
php artisan serve
```

**Verifikasi:**
- Buka browser: `http://localhost:8000`
- Harus muncul halaman welcome Laravel

### Tugas 2: Buat Route /hello
Buat satu route `/hello` yang return string.

**File: `routes/web.php`**
```php
<?php

use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return view('welcome');
});

Route::get('/hello', function () {
    return 'Hello, Selamat Belajar Laravel!';
});
```

**Test:**
- Buka browser: `http://localhost:8000/hello`
- Harus muncul: "Hello, Selamat Belajar Laravel!"

**Bonus:**
- Buat route `/hello/{name}` yang menerima parameter nama
- Buat route `/about` yang return informasi tentang aplikasi

---

## 💡 Tips

- Selalu gunakan `php artisan serve` untuk development
- File `.env` jangan di-commit ke git
- Gunakan `php artisan route:list` untuk melihat semua routes
- Route di `web.php` otomatis memiliki middleware web (session, CSRF, dll)
- Named route memudahkan maintenance jika URL berubah
- Gunakan Artisan commands untuk generate file (controller, model, migration)

---

**Selamat belajar! 🚀**


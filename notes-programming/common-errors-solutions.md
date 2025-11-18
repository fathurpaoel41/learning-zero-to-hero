# Common Errors & Solutions

## Error yang Sering Terjadi dan Solusinya

### PHP Errors

#### 1. Parse Error: syntax error, unexpected
**Error:**
```
Parse error: syntax error, unexpected '$variable' (T_VARIABLE)
```

**Penyebab:**
- Lupa titik koma (`;`)
- Kurung tidak seimbang
- String tidak ditutup

**Solusi:**
```php
// ❌ Salah
$name = "Budi"
echo $name;

// ✅ Benar
$name = "Budi";
echo $name;
```

#### 2. Undefined Variable
**Error:**
```
Notice: Undefined variable: $name
```

**Penyebab:**
- Variabel belum didefinisikan
- Typo pada nama variabel

**Solusi:**
```php
// ✅ Cek dulu dengan isset()
if (isset($name)) {
    echo $name;
}

// ✅ Atau gunakan null coalescing
echo $name ?? 'Default';
```

#### 3. Call to undefined function
**Error:**
```
Fatal error: Call to undefined function mysqli_connect()
```

**Penyebab:**
- Extension PHP tidak aktif
- Function tidak ada

**Solusi:**
- Aktifkan extension di `php.ini`
- Untuk MySQLi: `extension=mysqli`

#### 4. Cannot modify header information
**Error:**
```
Warning: Cannot modify header information - headers already sent
```

**Penyebab:**
- Ada output sebelum `header()` atau `redirect()`
- Spasi sebelum `<?php`

**Solusi:**
```php
// ✅ Pastikan tidak ada output sebelum header
// ❌ Jangan ada echo/print sebelum redirect
header("Location: index.php");
exit();

// ✅ Atau gunakan output buffering
ob_start();
// ... kode ...
ob_end_flush();
```

### Laravel Errors

#### 1. 404 Not Found
**Error:**
```
404 | Not Found
```

**Penyebab:**
- Route tidak terdaftar
- URL salah
- Controller tidak ada

**Solusi:**
```bash
# Cek route yang terdaftar
php artisan route:list

# Pastikan route ada di routes/web.php
Route::get('/users', [UserController::class, 'index']);
```

#### 2. Class not found
**Error:**
```
Class 'App\Http\Controllers\UserController' not found
```

**Penyebab:**
- Namespace salah
- File tidak ada
- Autoload belum di-update

**Solusi:**
```bash
# Update autoload
composer dump-autoload

# Pastikan namespace benar
namespace App\Http\Controllers;
```

#### 3. 419 Page Expired (CSRF)
**Error:**
```
419 | Page Expired
```

**Penyebab:**
- Form tidak ada `@csrf`
- Session expired
- CSRF token tidak match

**Solusi:**
```blade
{{-- ✅ Pastikan ada @csrf di form --}}
<form method="POST">
    @csrf
    ...
</form>
```

#### 4. SQLSTATE[HY000] [2002] Connection refused
**Error:**
```
SQLSTATE[HY000] [2002] Connection refused
```

**Penyebab:**
- Database tidak running
- Kredensial salah di `.env`
- Host/port salah

**Solusi:**
```env
# Cek .env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=belajar
DB_USERNAME=root
DB_PASSWORD=

# Pastikan MySQL running
# Cek koneksi dengan phpMyAdmin atau MySQL client
```

#### 5. Target class [Controller] does not exist
**Error:**
```
Target class [UserController] does not exist
```

**Penyebab:**
- Controller belum dibuat
- Nama controller salah

**Solusi:**
```bash
# Buat controller
php artisan make:controller UserController

# Pastikan nama file dan class sama
# File: UserController.php
# Class: class UserController
```

### JavaScript Errors

#### 1. Cannot read property of undefined
**Error:**
```
Uncaught TypeError: Cannot read property 'name' of undefined
```

**Penyebab:**
- Object belum ada
- Property tidak ada

**Solusi:**
```javascript
// ✅ Cek dulu
if (user && user.name) {
    console.log(user.name);
}

// ✅ Atau optional chaining (ES2020)
console.log(user?.name);
```

#### 2. Uncaught ReferenceError: variable is not defined
**Error:**
```
Uncaught ReferenceError: name is not defined
```

**Penyebab:**
- Variabel belum dideklarasikan
- Scope variabel salah

**Solusi:**
```javascript
// ✅ Deklarasikan dulu
let name = "Budi";
console.log(name);
```

#### 3. addEventListener is not a function
**Error:**
```
Uncaught TypeError: element.addEventListener is not a function
```

**Penyebab:**
- Element tidak ditemukan (null)
- Bukan DOM element

**Solusi:**
```javascript
// ✅ Cek dulu apakah element ada
let element = document.querySelector('.my-class');
if (element) {
    element.addEventListener('click', handler);
}
```

### MySQL Errors

#### 1. Table doesn't exist
**Error:**
```
Table 'database.table' doesn't exist
```

**Penyebab:**
- Tabel belum dibuat
- Nama tabel salah
- Database salah

**Solusi:**
```sql
-- Cek tabel yang ada
SHOW TABLES;

-- Buat tabel jika belum ada
CREATE TABLE users (...);
```

#### 2. Duplicate entry for key
**Error:**
```
Duplicate entry 'email@example.com' for key 'email'
```

**Penyebab:**
- Data duplikat untuk unique key
- Primary key duplikat

**Solusi:**
```sql
-- Cek data yang sudah ada
SELECT * FROM users WHERE email = 'email@example.com';

-- Hapus atau update data lama
DELETE FROM users WHERE email = 'email@example.com';
```

#### 3. Unknown column in field list
**Error:**
```
Unknown column 'name' in 'field list'
```

**Penyebab:**
- Kolom tidak ada di tabel
- Typo pada nama kolom

**Solusi:**
```sql
-- Cek struktur tabel
DESCRIBE users;

-- Pastikan nama kolom benar
SELECT name FROM users; -- bukan nama
```

### General Tips

1. **Baca Error Message dengan Teliti**
   - Error message biasanya memberitahu lokasi masalah
   - Line number menunjukkan baris yang error

2. **Check Console/Log**
   - Browser console untuk JavaScript
   - Laravel log: `storage/logs/laravel.log`
   - PHP error log

3. **Debug dengan var_dump/dd/console.log**
   ```php
   var_dump($variable);
   dd($variable);
   ```
   ```javascript
   console.log(variable);
   ```

4. **Cek Dokumentasi**
   - Laravel: laravel.com/docs
   - PHP: php.net/manual
   - JavaScript: MDN Web Docs

5. **Google Error Message**
   - Copy paste error message ke Google
   - Biasanya sudah ada solusi di Stack Overflow


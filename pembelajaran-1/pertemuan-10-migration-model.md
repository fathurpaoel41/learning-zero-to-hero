# PERTEMUAN 10 — Migration & Model

## 📚 Materi

### 1. Migration
Migration adalah version control untuk database schema. Migration memungkinkan kita untuk membuat, mengubah, dan menghapus tabel dengan cara yang terstruktur.

**Keuntungan Migration:**
- Version control untuk database
- Mudah di-share dengan tim
- Dapat di-rollback jika ada kesalahan
- Konsisten di semua environment

### 2. Membuat Migration

**Perintah:**
```bash
php artisan make:migration create_nama_tabel_table
```

**Contoh:**
```bash
php artisan make:migration create_todos_table
```

**File akan dibuat di: `database/migrations/YYYY_MM_DD_HHMMSS_create_todos_table.php`**

### 3. Struktur Migration

**File Migration:**
```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('todos', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('todos');
    }
};
```

**Penjelasan:**
- `up()` - Dijalankan saat `php artisan migrate`
- `down()` - Dijalankan saat `php artisan migrate:rollback`
- `$table->id()` - Membuat kolom `id` dengan AUTO_INCREMENT PRIMARY KEY
- `$table->timestamps()` - Membuat kolom `created_at` dan `updated_at`

### 4. Tipe Data di Migration

**String:**
```php
$table->string('name');              // VARCHAR(255)
$table->string('email', 100);       // VARCHAR(100)
$table->text('description');        // TEXT
$table->longText('content');        // LONGTEXT
```

**Numeric:**
```php
$table->integer('age');             // INT
$table->bigInteger('count');        // BIGINT
$table->float('price');             // FLOAT
$table->decimal('amount', 10, 2);   // DECIMAL(10,2)
```

**Date/Time:**
```php
$table->date('birth_date');         // DATE
$table->time('start_time');         // TIME
$table->dateTime('published_at');   // DATETIME
$table->timestamp('deleted_at');    // TIMESTAMP
```

**Boolean:**
```php
$table->boolean('is_active');       // TINYINT(1)
```

**Lainnya:**
```php
$table->json('metadata');           // JSON
$table->enum('status', ['active', 'inactive']); // ENUM
```

### 5. Constraints di Migration

```php
$table->string('email')->unique();           // UNIQUE
$table->string('name')->nullable();          // NULL
$table->string('title')->default('Untitled'); // DEFAULT
$table->string('slug')->index();             // INDEX
$table->foreignId('user_id')->constrained(); // FOREIGN KEY
```

### 6. Contoh Migration Lengkap

**File: `database/migrations/YYYY_MM_DD_HHMMSS_create_todos_table.php`**
```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('todos', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->text('description')->nullable();
            $table->boolean('completed')->default(false);
            $table->timestamps();
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('todos');
    }
};
```

### 7. Menjalankan Migration

**Menjalankan semua migration:**
```bash
php artisan migrate
```

**Rollback migration terakhir:**
```bash
php artisan migrate:rollback
```

**Rollback semua migration:**
```bash
php artisan migrate:reset
```

**Refresh (rollback + migrate):**
```bash
php artisan migrate:refresh
```

**Fresh (drop semua tabel + migrate):**
```bash
php artisan migrate:fresh
```

### 8. Model
Model adalah representasi dari tabel database. Model menggunakan Eloquent ORM.

**Membuat Model:**
```bash
php artisan make:model Todo
```

**File akan dibuat di: `app/Models/Todo.php`**

### 9. Struktur Model Dasar

**File: `app/Models/Todo.php`**
```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Todo extends Model
{
    use HasFactory;
    
    // Nama tabel (opsional, default: plural dari nama model)
    protected $table = 'todos';
    
    // Primary key (opsional, default: 'id')
    protected $primaryKey = 'id';
    
    // Timestamps (opsional, default: true)
    public $timestamps = true;
    
    // Fillable (kolom yang bisa di-mass assign)
    protected $fillable = [
        'title',
        'description',
        'completed'
    ];
    
    // Guarded (kolom yang tidak bisa di-mass assign)
    // protected $guarded = ['id'];
}
```

### 10. Eloquent Dasar

**Eloquent** adalah ORM (Object-Relational Mapping) Laravel yang memudahkan interaksi dengan database.

**Create:**
```php
// Cara 1: Mass assignment
$todo = Todo::create([
    'title' => 'Belajar Laravel',
    'description' => 'Mempelajari migration dan model',
    'completed' => false
]);

// Cara 2: Manual
$todo = new Todo();
$todo->title = 'Belajar Laravel';
$todo->description = 'Mempelajari migration dan model';
$todo->completed = false;
$todo->save();
```

**Read:**
```php
// Ambil semua data
$todos = Todo::all();

// Ambil berdasarkan ID
$todo = Todo::find(1);

// Ambil dengan kondisi
$todos = Todo::where('completed', false)->get();

// Ambil data pertama
$todo = Todo::where('completed', false)->first();
```

**Update:**
```php
// Cara 1
$todo = Todo::find(1);
$todo->title = 'Updated Title';
$todo->save();

// Cara 2: Mass update
Todo::where('id', 1)->update([
    'title' => 'Updated Title',
    'completed' => true
]);
```

**Delete:**
```php
// Cara 1
$todo = Todo::find(1);
$todo->delete();

// Cara 2
Todo::destroy(1);

// Cara 3: Mass delete
Todo::where('completed', true)->delete();
```

### 11. Seeder
Seeder digunakan untuk mengisi database dengan data dummy/testing.

**Membuat Seeder:**
```bash
php artisan make:seeder TodoSeeder
```

**File: `database/seeders/TodoSeeder.php`**
```php
<?php

namespace Database\Seeders;

use Illuminate\Database\Seeder;
use App\Models\Todo;

class TodoSeeder extends Seeder
{
    public function run(): void
    {
        Todo::create([
            'title' => 'Belajar Laravel',
            'description' => 'Mempelajari migration dan model',
            'completed' => false
        ]);
        
        Todo::create([
            'title' => 'Membuat CRUD',
            'description' => 'Membuat aplikasi CRUD dengan Laravel',
            'completed' => false
        ]);
    }
}
```

**Menjalankan Seeder:**
```bash
php artisan db:seed --class=TodoSeeder
```

**Atau di DatabaseSeeder:**
```php
public function run(): void
{
    $this->call([
        TodoSeeder::class,
    ]);
}
```

---

## 📝 Pretest (3 Soal Ringkas)

1. **Perintah membuat migration?**
   - Jawab: `php artisan make:migration create_nama_tabel_table`. Contoh: `php artisan make:migration create_todos_table`

2. **Apa itu model di Laravel?**
   - Jawab: Model adalah representasi dari tabel database. Model menggunakan Eloquent ORM untuk berinteraksi dengan database. Model biasanya disimpan di `app/Models/`.

3. **Apa itu Eloquent?**
   - Jawab: Eloquent adalah ORM (Object-Relational Mapping) Laravel yang memudahkan interaksi dengan database menggunakan syntax yang mirip dengan object-oriented programming. Contoh: `Todo::all()`, `Todo::find(1)`, `Todo::create([...])`.

---

## 🏠 PR (Pekerjaan Rumah)

### Tugas 1: Buat Migration untuk Tabel todos
Buat migration untuk tabel `todos` dengan kolom:
- `id` - Primary key, auto increment
- `title` - String, required
- `description` - Text, nullable
- `completed` - Boolean, default false
- `timestamps` - created_at dan updated_at

**Langkah:**
```bash
php artisan make:migration create_todos_table
```

**Edit file migration:**
```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('todos', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->text('description')->nullable();
            $table->boolean('completed')->default(false);
            $table->timestamps();
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('todos');
    }
};
```

**Jalankan migration:**
```bash
php artisan migrate
```

**Verifikasi:**
- Cek di database, tabel `todos` harus sudah dibuat
- Atau gunakan `php artisan migrate:status`

### Tugas 2: Buat Model Todo
Buat model `Todo` dengan fillable properties.

**Langkah:**
```bash
php artisan make:model Todo
```

**Edit file `app/Models/Todo.php`:**
```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Todo extends Model
{
    use HasFactory;
    
    protected $fillable = [
        'title',
        'description',
        'completed'
    ];
}
```

**Test Model (bisa di route atau tinker):**
```bash
php artisan tinker
```

**Di tinker:**
```php
// Create
$todo = Todo::create([
    'title' => 'Test Todo',
    'description' => 'Ini adalah test',
    'completed' => false
]);

// Read
Todo::all();

// Update
$todo = Todo::find(1);
$todo->completed = true;
$todo->save();

// Delete
$todo->delete();
```

---

## 💡 Tips

- Nama migration harus deskriptif: `create_todos_table`, `add_status_to_todos_table`
- Selalu test migration dengan `migrate:rollback` sebelum commit
- Gunakan `$fillable` atau `$guarded` untuk mass assignment protection
- Model name singular, tabel name plural (Todo → todos)
- Gunakan `php artisan tinker` untuk test model dengan cepat
- Timestamps otomatis di-update saat create/update

---

**Selamat belajar! 🚀**


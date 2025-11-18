# PERTEMUAN 11 — Laravel CRUD

## 📚 Materi

### 1. Controller
Controller menangani request dari user dan mengembalikan response.

**Membuat Controller:**
```bash
php artisan make:controller TodoController
```

**Resource Controller (untuk CRUD):**
```bash
php artisan make:controller TodoController --resource
```

**File: `app/Http/Controllers/TodoController.php`**
```php
<?php

namespace App\Http\Controllers;

use App\Models\Todo;
use Illuminate\Http\Request;

class TodoController extends Controller
{
    /**
     * Display a listing of the resource.
     */
    public function index()
    {
        $todos = Todo::all();
        return view('todos.index', compact('todos'));
    }

    /**
     * Show the form for creating a new resource.
     */
    public function create()
    {
        return view('todos.create');
    }

    /**
     * Store a newly created resource in storage.
     */
    public function store(Request $request)
    {
        $validated = $request->validate([
            'title' => 'required|max:255',
            'description' => 'nullable',
        ]);

        Todo::create([
            'title' => $validated['title'],
            'description' => $validated['description'],
            'completed' => false
        ]);

        return redirect()->route('todos.index')
            ->with('success', 'Todo berhasil ditambahkan');
    }

    /**
     * Display the specified resource.
     */
    public function show(Todo $todo)
    {
        return view('todos.show', compact('todo'));
    }

    /**
     * Show the form for editing the specified resource.
     */
    public function edit(Todo $todo)
    {
        return view('todos.edit', compact('todo'));
    }

    /**
     * Update the specified resource in storage.
     */
    public function update(Request $request, Todo $todo)
    {
        $validated = $request->validate([
            'title' => 'required|max:255',
            'description' => 'nullable',
        ]);

        $todo->update($validated);

        return redirect()->route('todos.index')
            ->with('success', 'Todo berhasil diupdate');
    }

    /**
     * Remove the specified resource from storage.
     */
    public function destroy(Todo $todo)
    {
        $todo->delete();

        return redirect()->route('todos.index')
            ->with('success', 'Todo berhasil dihapus');
    }
}
```

### 2. Route Resource
Route resource otomatis membuat semua route untuk CRUD.

**File: `routes/web.php`**
```php
use App\Http\Controllers\TodoController;

Route::resource('todos', TodoController::class);
```

**Route yang dibuat otomatis:**
```
GET    /todos           → index()   (list)
GET    /todos/create    → create()  (form create)
POST   /todos           → store()   (proses create)
GET    /todos/{id}      → show()    (detail)
GET    /todos/{id}/edit → edit()    (form edit)
PUT    /todos/{id}      → update()  (proses update)
DELETE /todos/{id}      → destroy() (proses delete)
```

**Lihat semua routes:**
```bash
php artisan route:list
```

### 3. View (Blade Template)

**File: `resources/views/todos/index.blade.php`**
```blade
<!DOCTYPE html>
<html>
<head>
    <title>Todo List</title>
    <style>
        body { font-family: Arial; padding: 20px; }
        table { border-collapse: collapse; width: 100%; }
        th, td { border: 1px solid #ddd; padding: 12px; text-align: left; }
        th { background-color: #4CAF50; color: white; }
        .btn { padding: 8px 16px; text-decoration: none; border-radius: 4px; }
        .btn-primary { background: #2196F3; color: white; }
        .btn-success { background: #4CAF50; color: white; }
        .btn-danger { background: #f44336; color: white; }
        .success { background: #d4edda; color: #155724; padding: 10px; border-radius: 5px; margin-bottom: 20px; }
    </style>
</head>
<body>
    <h1>Todo List</h1>

    @if(session('success'))
        <div class="success">{{ session('success') }}</div>
    @endif

    <a href="{{ route('todos.create') }}" class="btn btn-primary">Tambah Todo</a>

    <table>
        <thead>
            <tr>
                <th>ID</th>
                <th>Title</th>
                <th>Description</th>
                <th>Status</th>
                <th>Aksi</th>
            </tr>
        </thead>
        <tbody>
            @forelse($todos as $todo)
                <tr>
                    <td>{{ $todo->id }}</td>
                    <td>{{ $todo->title }}</td>
                    <td>{{ $todo->description ?? '-' }}</td>
                    <td>{{ $todo->completed ? 'Selesai' : 'Belum Selesai' }}</td>
                    <td>
                        <a href="{{ route('todos.edit', $todo) }}" class="btn btn-success">Edit</a>
                        <form action="{{ route('todos.destroy', $todo) }}" method="POST" style="display: inline;">
                            @csrf
                            @method('DELETE')
                            <button type="submit" class="btn btn-danger" onclick="return confirm('Yakin ingin menghapus?')">Hapus</button>
                        </form>
                    </td>
                </tr>
            @empty
                <tr>
                    <td colspan="5" style="text-align: center;">Tidak ada data</td>
                </tr>
            @endforelse
        </tbody>
    </table>
</body>
</html>
```

**File: `resources/views/todos/create.blade.php`**
```blade
<!DOCTYPE html>
<html>
<head>
    <title>Tambah Todo</title>
    <style>
        body { font-family: Arial; padding: 20px; }
        form { max-width: 500px; }
        input, textarea { width: 100%; padding: 8px; margin: 5px 0; }
        button { padding: 10px 20px; background: #4CAF50; color: white; border: none; cursor: pointer; }
        .error { color: red; font-size: 14px; }
    </style>
</head>
<body>
    <h1>Tambah Todo</h1>

    @if($errors->any())
        <div style="background: #f8d7da; color: #721c24; padding: 10px; border-radius: 5px; margin-bottom: 20px;">
            <ul>
                @foreach($errors->all() as $error)
                    <li>{{ $error }}</li>
                @endforeach
            </ul>
        </div>
    @endif

    <form action="{{ route('todos.store') }}" method="POST">
        @csrf
        
        <label>Title:</label>
        <input type="text" name="title" value="{{ old('title') }}" required>
        @error('title')
            <div class="error">{{ $message }}</div>
        @enderror

        <label>Description:</label>
        <textarea name="description" rows="4">{{ old('description') }}</textarea>
        @error('description')
            <div class="error">{{ $message }}</div>
        @enderror

        <button type="submit">Simpan</button>
        <a href="{{ route('todos.index') }}">Batal</a>
    </form>
</body>
</html>
```

**File: `resources/views/todos/edit.blade.php`**
```blade
<!DOCTYPE html>
<html>
<head>
    <title>Edit Todo</title>
    <style>
        body { font-family: Arial; padding: 20px; }
        form { max-width: 500px; }
        input, textarea { width: 100%; padding: 8px; margin: 5px 0; }
        button { padding: 10px 20px; background: #2196F3; color: white; border: none; cursor: pointer; }
        .error { color: red; font-size: 14px; }
    </style>
</head>
<body>
    <h1>Edit Todo</h1>

    @if($errors->any())
        <div style="background: #f8d7da; color: #721c24; padding: 10px; border-radius: 5px; margin-bottom: 20px;">
            <ul>
                @foreach($errors->all() as $error)
                    <li>{{ $error }}</li>
                @endforeach
            </ul>
        </div>
    @endif

    <form action="{{ route('todos.update', $todo) }}" method="POST">
        @csrf
        @method('PUT')
        
        <label>Title:</label>
        <input type="text" name="title" value="{{ old('title', $todo->title) }}" required>
        @error('title')
            <div class="error">{{ $message }}</div>
        @enderror

        <label>Description:</label>
        <textarea name="description" rows="4">{{ old('description', $todo->description) }}</textarea>
        @error('description')
            <div class="error">{{ $message }}</div>
        @enderror

        <button type="submit">Update</button>
        <a href="{{ route('todos.index') }}">Batal</a>
    </form>
</body>
</html>
```

### 4. Validation
Laravel menyediakan validation yang powerful.

**Di Controller:**
```php
$validated = $request->validate([
    'title' => 'required|max:255',
    'description' => 'nullable|string',
    'email' => 'required|email|unique:users',
    'age' => 'required|integer|min:18|max:100',
]);
```

**Rules Umum:**
- `required` - Wajib diisi
- `nullable` - Boleh kosong
- `string` - Harus string
- `integer` - Harus integer
- `email` - Harus format email
- `max:255` - Maksimal 255 karakter
- `min:18` - Minimal 18
- `unique:table,column` - Harus unik di tabel

**Menampilkan Error di View:**
```blade
@if($errors->any())
    <ul>
        @foreach($errors->all() as $error)
            <li>{{ $error }}</li>
        @endforeach
    </ul>
@endif

@error('title')
    <div class="error">{{ $message }}</div>
@enderror
```

### 5. Redirect dengan Flash Message
```php
return redirect()->route('todos.index')
    ->with('success', 'Todo berhasil ditambahkan');
```

**Di View:**
```blade
@if(session('success'))
    <div class="success">{{ session('success') }}</div>
@endif
```

---

## 📝 Pretest (3 Soal Ringkas)

1. **Apa perintah membuat controller resource?**
   - Jawab: `php artisan make:controller NamaController --resource`. Controller resource otomatis membuat method index, create, store, show, edit, update, dan destroy.

2. **Apa fungsi $request->validate()?**
   - Jawab: `$request->validate()` digunakan untuk memvalidasi data yang dikirim dari form. Jika validasi gagal, Laravel otomatis redirect kembali dengan error messages. Contoh: `$request->validate(['title' => 'required|max:255'])`.

3. **Apa itu route resource?**
   - Jawab: Route resource adalah cara untuk mendefinisikan semua route CRUD sekaligus dengan satu baris: `Route::resource('todos', TodoController::class)`. Route resource otomatis membuat 7 route untuk index, create, store, show, edit, update, dan destroy.

---

## 🏠 PR (Pekerjaan Rumah)

### Tugas: Buat CRUD To-Do List
Buat CRUD To-Do List lengkap dengan Laravel (basic saja tanpa JavaScript).

**Langkah:**

1. **Pastikan migration dan model sudah dibuat** (dari pertemuan 10)

2. **Buat Controller:**
```bash
php artisan make:controller TodoController --resource
```

3. **Buat Route:**
```php
// routes/web.php
use App\Http\Controllers\TodoController;

Route::get('/', function () {
    return redirect()->route('todos.index');
});

Route::resource('todos', TodoController::class);
```

4. **Implementasi Controller** (gunakan contoh di materi)

5. **Buat Views:**
   - `resources/views/todos/index.blade.php` - List semua todos
   - `resources/views/todos/create.blade.php` - Form tambah todo
   - `resources/views/todos/edit.blade.php` - Form edit todo

6. **Fitur yang harus ada:**
   - ✅ Tampilkan semua todos
   - ✅ Form tambah todo (title, description)
   - ✅ Form edit todo
   - ✅ Hapus todo (dengan konfirmasi)
   - ✅ Validasi form (title required)
   - ✅ Flash message setelah create/update/delete
   - ✅ Error handling dan display

**Kriteria Penilaian:**
- ✅ Semua operasi CRUD berfungsi
- ✅ Menggunakan route resource
- ✅ Ada validasi form
- ✅ Ada flash message
- ✅ Error ditampilkan dengan baik
- ✅ UI rapi dan mudah digunakan

---

## 💡 Tips

- Gunakan `--resource` flag saat membuat controller untuk CRUD
- Route resource otomatis membuat semua route yang dibutuhkan
- Gunakan `@csrf` di form untuk keamanan
- Gunakan `@method('PUT')` untuk update dan `@method('DELETE')` untuk delete
- `old()` helper untuk mempertahankan input setelah validation error
- `compact()` untuk pass data ke view
- Gunakan `@forelse` untuk handle empty data

---

**Selamat belajar! 🚀**


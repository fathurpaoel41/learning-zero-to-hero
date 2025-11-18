# PERTEMUAN 16 — Laravel To-Do List Project

## 📚 Materi

### 1. Review Konsep CRUD + DOM Interaktif
Pada pertemuan ini, kita akan menggabungkan semua yang sudah dipelajari:
- Laravel CRUD (Create, Read, Update, Delete)
- JavaScript DOM Manipulation
- Form Validation
- AJAX (opsional)

### 2. Struktur Project

```
todo-laravel/
├── app/
│   ├── Http/Controllers/
│   │   └── TodoController.php
│   └── Models/
│       └── Todo.php
├── database/
│   └── migrations/
│       └── YYYY_MM_DD_create_todos_table.php
├── resources/
│   ├── views/
│   │   ├── layouts/
│   │   │   └── app.blade.php
│   │   └── todos/
│   │       ├── index.blade.php
│   │       ├── create.blade.php
│   │       └── edit.blade.php
│   └── js/
│       └── todos.js
├── routes/
│   └── web.php
└── public/
    └── js/
        └── todos.js (compiled)
```

### 3. Model dan Migration

**Migration: `database/migrations/YYYY_MM_DD_create_todos_table.php`**
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

**Model: `app/Models/Todo.php`**
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

    protected $casts = [
        'completed' => 'boolean',
    ];
}
```

### 4. Controller Lengkap

**Controller: `app/Http/Controllers/TodoController.php`**
```php
<?php

namespace App\Http\Controllers;

use App\Models\Todo;
use Illuminate\Http\Request;

class TodoController extends Controller
{
    public function index()
    {
        $todos = Todo::orderBy('created_at', 'desc')->get();
        return view('todos.index', compact('todos'));
    }

    public function create()
    {
        return view('todos.create');
    }

    public function store(Request $request)
    {
        $validated = $request->validate([
            'title' => 'required|max:255',
            'description' => 'nullable|string',
        ]);

        Todo::create([
            'title' => $validated['title'],
            'description' => $validated['description'] ?? null,
            'completed' => false
        ]);

        return redirect()->route('todos.index')
            ->with('success', 'Todo berhasil ditambahkan!');
    }

    public function edit(Todo $todo)
    {
        return view('todos.edit', compact('todo'));
    }

    public function update(Request $request, Todo $todo)
    {
        $validated = $request->validate([
            'title' => 'required|max:255',
            'description' => 'nullable|string',
        ]);

        $todo->update($validated);

        return redirect()->route('todos.index')
            ->with('success', 'Todo berhasil diupdate!');
    }

    public function destroy(Todo $todo)
    {
        $todo->delete();

        return redirect()->route('todos.index')
            ->with('success', 'Todo berhasil dihapus!');
    }

    // Toggle status (opsional, untuk AJAX)
    public function toggle(Request $request, Todo $todo)
    {
        $todo->update([
            'completed' => !$todo->completed
        ]);

        return response()->json([
            'success' => true,
            'completed' => $todo->completed
        ]);
    }
}
```

### 5. Routes

**Routes: `routes/web.php`**
```php
<?php

use App\Http\Controllers\TodoController;
use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return redirect()->route('todos.index');
});

Route::resource('todos', TodoController::class);

// Route untuk toggle status (opsional)
Route::post('/todos/{todo}/toggle', [TodoController::class, 'toggle'])
    ->name('todos.toggle');
```

### 6. Layout

**Layout: `resources/views/layouts/app.blade.php`**
```blade
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>@yield('title', 'Todo List')</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: Arial, sans-serif; background: #f5f5f5; }
        .container { max-width: 800px; margin: 0 auto; padding: 20px; }
        .header { background: #4CAF50; color: white; padding: 20px; border-radius: 5px; margin-bottom: 20px; }
        .header h1 { margin-bottom: 10px; }
        .nav a { color: white; text-decoration: none; margin-right: 15px; }
        .nav a:hover { text-decoration: underline; }
        .success { background: #d4edda; color: #155724; padding: 10px; border-radius: 5px; margin-bottom: 20px; }
        .error { color: red; font-size: 14px; margin-top: 5px; }
        table { width: 100%; border-collapse: collapse; background: white; }
        th, td { padding: 12px; text-align: left; border-bottom: 1px solid #ddd; }
        th { background: #4CAF50; color: white; }
        tr:hover { background: #f5f5f5; }
        .btn { padding: 8px 16px; text-decoration: none; border-radius: 4px; border: none; cursor: pointer; }
        .btn-primary { background: #2196F3; color: white; }
        .btn-success { background: #4CAF50; color: white; }
        .btn-danger { background: #f44336; color: white; }
        .btn-small { padding: 5px 10px; font-size: 12px; }
        .status { padding: 5px 10px; border-radius: 3px; font-size: 12px; }
        .status.completed { background: #4CAF50; color: white; }
        .status.pending { background: #ff9800; color: white; }
        form { background: white; padding: 20px; border-radius: 5px; }
        input, textarea { width: 100%; padding: 8px; margin: 5px 0; border: 1px solid #ddd; border-radius: 4px; }
        button[type="submit"] { margin-top: 10px; }
    </style>
    @stack('styles')
</head>
<body>
    <div class="container">
        <header class="header">
            <h1>📝 Todo List App</h1>
            <nav class="nav">
                <a href="{{ route('todos.index') }}">Home</a>
                <a href="{{ route('todos.create') }}">Tambah Todo</a>
            </nav>
        </header>

        @if(session('success'))
            <div class="success">{{ session('success') }}</div>
        @endif

        <main>
            @yield('content')
        </main>
    </div>

    @stack('scripts')
</body>
</html>
```

### 7. Views

**Index: `resources/views/todos/index.blade.php`**
```blade
@extends('layouts.app')

@section('title', 'Todo List')

@section('content')
    <h2>Daftar Todo</h2>
    
    <a href="{{ route('todos.create') }}" class="btn btn-primary">Tambah Todo Baru</a>

    <table style="margin-top: 20px;">
        <thead>
            <tr>
                <th>ID</th>
                <th>Title</th>
                <th>Description</th>
                <th>Status</th>
                <th>Aksi</th>
            </tr>
        </thead>
        <tbody id="todo-tbody">
            @forelse($todos as $todo)
                <tr data-id="{{ $todo->id }}">
                    <td>{{ $todo->id }}</td>
                    <td class="todo-title">{{ $todo->title }}</td>
                    <td>{{ $todo->description ?? '-' }}</td>
                    <td>
                        <span class="status {{ $todo->completed ? 'completed' : 'pending' }}" id="status-{{ $todo->id }}">
                            {{ $todo->completed ? 'Selesai' : 'Belum Selesai' }}
                        </span>
                    </td>
                    <td>
                        <button class="btn btn-success btn-small btn-toggle" data-id="{{ $todo->id }}">
                            Toggle
                        </button>
                        <a href="{{ route('todos.edit', $todo) }}" class="btn btn-primary btn-small">Edit</a>
                        <form action="{{ route('todos.destroy', $todo) }}" method="POST" class="delete-form" style="display: inline;">
                            @csrf
                            @method('DELETE')
                            <button type="submit" class="btn btn-danger btn-small">Hapus</button>
                        </form>
                    </td>
                </tr>
            @empty
                <tr>
                    <td colspan="5" style="text-align: center; padding: 20px;">Tidak ada todo</td>
                </tr>
            @endforelse
        </tbody>
    </table>
@endsection

@push('scripts')
    <script src="{{ asset('js/todos.js') }}"></script>
@endpush
```

**Create: `resources/views/todos/create.blade.php`**
```blade
@extends('layouts.app')

@section('title', 'Tambah Todo')

@section('content')
    <h2>Tambah Todo Baru</h2>

    @if($errors->any())
        <div style="background: #f8d7da; color: #721c24; padding: 10px; border-radius: 5px; margin-bottom: 20px;">
            <ul>
                @foreach($errors->all() as $error)
                    <li>{{ $error }}</li>
                @endforeach
            </ul>
        </div>
    @endif

    <form id="todo-form" action="{{ route('todos.store') }}" method="POST">
        @csrf
        
        <div>
            <label>Title:</label>
            <input type="text" name="title" id="title" value="{{ old('title') }}" required>
            <span id="title-error" class="error"></span>
        </div>

        <div>
            <label>Description:</label>
            <textarea name="description" id="description" rows="4">{{ old('description') }}</textarea>
            <span id="description-error" class="error"></span>
        </div>

        <button type="submit" class="btn btn-primary">Simpan</button>
        <a href="{{ route('todos.index') }}" class="btn" style="background: #ccc;">Batal</a>
    </form>
@endsection

@push('scripts')
    <script>
        // Validasi form
        document.querySelector('#todo-form').addEventListener('submit', function(event) {
            let title = document.querySelector('#title').value.trim();
            let titleError = document.querySelector('#title-error');
            
            titleError.textContent = '';
            
            if (title === '') {
                event.preventDefault();
                titleError.textContent = 'Title harus diisi!';
            } else if (title.length < 3) {
                event.preventDefault();
                titleError.textContent = 'Title minimal 3 karakter!';
            }
        });
    </script>
@endpush
```

**Edit: `resources/views/todos/edit.blade.php`**
```blade
@extends('layouts.app')

@section('title', 'Edit Todo')

@section('content')
    <h2>Edit Todo</h2>

    @if($errors->any())
        <div style="background: #f8d7da; color: #721c24; padding: 10px; border-radius: 5px; margin-bottom: 20px;">
            <ul>
                @foreach($errors->all() as $error)
                    <li>{{ $error }}</li>
                @endforeach
            </ul>
        </div>
    @endif

    <form id="todo-form" action="{{ route('todos.update', $todo) }}" method="POST">
        @csrf
        @method('PUT')
        
        <div>
            <label>Title:</label>
            <input type="text" name="title" id="title" value="{{ old('title', $todo->title) }}" required>
            <span id="title-error" class="error"></span>
        </div>

        <div>
            <label>Description:</label>
            <textarea name="description" id="description" rows="4">{{ old('description', $todo->description) }}</textarea>
            <span id="description-error" class="error"></span>
        </div>

        <button type="submit" class="btn btn-primary">Update</button>
        <a href="{{ route('todos.index') }}" class="btn" style="background: #ccc;">Batal</a>
    </form>
@endsection

@push('scripts')
    <script>
        // Validasi form
        document.querySelector('#todo-form').addEventListener('submit', function(event) {
            let title = document.querySelector('#title').value.trim();
            let titleError = document.querySelector('#title-error');
            
            titleError.textContent = '';
            
            if (title === '') {
                event.preventDefault();
                titleError.textContent = 'Title harus diisi!';
            } else if (title.length < 3) {
                event.preventDefault();
                titleError.textContent = 'Title minimal 3 karakter!';
            }
        });
    </script>
@endpush
```

### 8. JavaScript untuk Interaktivitas

**JavaScript: `public/js/todos.js`**
```javascript
// Konfirmasi delete
document.querySelectorAll('.delete-form').forEach(form => {
    form.addEventListener('submit', function(event) {
        event.preventDefault();
        
        if (confirm('Yakin ingin menghapus todo ini?')) {
            this.submit();
        }
    });
});

// Toggle status tanpa reload (AJAX)
document.querySelectorAll('.btn-toggle').forEach(button => {
    button.addEventListener('click', function() {
        let todoId = this.dataset.id;
        let statusSpan = document.querySelector(`#status-${todoId}`);
        
        // Update UI langsung (optimistic update)
        let isCompleted = statusSpan.classList.contains('completed');
        
        if (isCompleted) {
            statusSpan.classList.remove('completed');
            statusSpan.classList.add('pending');
            statusSpan.textContent = 'Belum Selesai';
        } else {
            statusSpan.classList.remove('pending');
            statusSpan.classList.add('completed');
            statusSpan.textContent = 'Selesai';
        }
        
        // Kirim request ke server (opsional)
        fetch(`/todos/${todoId}/toggle`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'X-CSRF-TOKEN': document.querySelector('meta[name="csrf-token"]').content
            }
        })
        .then(response => response.json())
        .then(data => {
            if (!data.success) {
                // Rollback jika gagal
                if (isCompleted) {
                    statusSpan.classList.remove('pending');
                    statusSpan.classList.add('completed');
                    statusSpan.textContent = 'Selesai';
                } else {
                    statusSpan.classList.remove('completed');
                    statusSpan.classList.add('pending');
                    statusSpan.textContent = 'Belum Selesai';
                }
                alert('Gagal update status!');
            }
        })
        .catch(error => {
            console.error('Error:', error);
            // Rollback
            if (isCompleted) {
                statusSpan.classList.remove('pending');
                statusSpan.classList.add('completed');
                statusSpan.textContent = 'Selesai';
            } else {
                statusSpan.classList.remove('completed');
                statusSpan.classList.add('pending');
                statusSpan.textContent = 'Belum Selesai';
            }
        });
    });
});
```

---

## 📝 Pretest (3 Soal Ringkas)

1. **Apa itu MVC di Laravel?**
   - Jawab: MVC (Model-View-Controller) adalah pola arsitektur yang memisahkan aplikasi menjadi 3 bagian:
     - **Model** - Menangani data dan logika database (Todo.php)
     - **View** - Menangani tampilan/user interface (Blade templates)
     - **Controller** - Menangani request dan koordinasi antara Model dan View (TodoController.php)

2. **Apa fungsi model dalam CRUD?**
   - Jawab: Model dalam CRUD berfungsi untuk:
     - Berinteraksi dengan database menggunakan Eloquent
     - Mendefinisikan fillable/guarded untuk mass assignment
     - Menyediakan method untuk query data (create, read, update, delete)
     - Menjadi representasi dari tabel database

3. **Apa hubungan JS DOM dengan Laravel?**
   - Jawab: JavaScript DOM digunakan untuk membuat aplikasi Laravel lebih interaktif:
     - Validasi form di client-side sebelum submit
     - Update UI tanpa reload halaman (AJAX)
     - Konfirmasi sebelum delete
     - Manipulasi elemen HTML secara dinamis
     - Laravel menyediakan data melalui Blade, JavaScript memanipulasi tampilan

---

## 🏠 PR (Pekerjaan Rumah)

### Tugas: Selesaikan To-Do List Interaktif
Selesaikan aplikasi To-Do List interaktif dengan fitur lengkap.

**Fitur Wajib:**
- ✅ CRUD lengkap (Create, Read, Update, Delete)
- ✅ Layout yang konsisten di semua halaman
- ✅ Validasi form (client-side dan server-side)
- ✅ Flash message setelah operasi
- ✅ Konfirmasi sebelum delete
- ✅ Toggle status tanpa reload halaman (menggunakan JavaScript)

**Fitur Bonus:**
- ✅ Search/filter todo
- ✅ Sort by status/date
- ✅ Pagination
- ✅ Mark all as completed
- ✅ Delete all completed

**Struktur yang Harus Ada:**
1. Migration dan Model Todo
2. Controller dengan semua method CRUD
3. Routes (resource + toggle)
4. Layout dengan navigation
5. Views untuk index, create, edit
6. JavaScript untuk interaktivitas

**Kriteria Penilaian:**
- ✅ Semua operasi CRUD berfungsi dengan baik
- ✅ JavaScript untuk toggle status bekerja
- ✅ Validasi form bekerja (client dan server)
- ✅ UI rapi dan user-friendly
- ✅ Code terorganisir dan mudah dibaca
- ✅ Menggunakan best practices Laravel

---

## 💡 Tips

- Gunakan route resource untuk CRUD standar
- Pisahkan JavaScript ke file terpisah untuk maintainability
- Gunakan optimistic update untuk UX yang lebih baik
- Selalu validasi di client (JS) dan server (Laravel)
- Gunakan CSRF token untuk keamanan
- Test semua fitur sebelum submit
- Gunakan console.log() untuk debugging JavaScript

---

**Selamat belajar! 🚀**


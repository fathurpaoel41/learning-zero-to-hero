# PERTEMUAN 12 — Laravel Blade Advanced

## 📚 Materi

### 1. Pengenalan Blade
Blade adalah templating engine Laravel yang powerful dan mudah digunakan.

**Keuntungan Blade:**
- Syntax yang clean dan mudah dibaca
- Inheritance (extends, yield, section)
- Components dan includes
- Directives (@if, @foreach, dll)
- Auto-escaping untuk keamanan

### 2. Layout (Master Template)

**File: `resources/views/layouts/app.blade.php`**
```blade
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@yield('title', 'Laravel App')</title>
    <style>
        body { font-family: Arial; margin: 0; padding: 0; }
        .header { background: #4CAF50; color: white; padding: 20px; }
        .container { max-width: 1200px; margin: 0 auto; padding: 20px; }
        .footer { background: #333; color: white; padding: 20px; text-align: center; margin-top: 50px; }
    </style>
    @stack('styles')
</head>
<body>
    <header class="header">
        <h1>My Laravel App</h1>
        <nav>
            <a href="{{ route('todos.index') }}" style="color: white; margin-right: 20px;">Todos</a>
            <a href="{{ route('todos.create') }}" style="color: white;">Tambah Todo</a>
        </nav>
    </header>

    <main class="container">
        @if(session('success'))
            <div style="background: #d4edda; color: #155724; padding: 10px; border-radius: 5px; margin-bottom: 20px;">
                {{ session('success') }}
            </div>
        @endif

        @yield('content')
    </main>

    <footer class="footer">
        <p>&copy; 2024 My Laravel App. All rights reserved.</p>
    </footer>

    @stack('scripts')
</body>
</html>
```

### 3. Menggunakan Layout

**File: `resources/views/todos/index.blade.php`**
```blade
@extends('layouts.app')

@section('title', 'Todo List')

@section('content')
    <h2>Todo List</h2>

    <a href="{{ route('todos.create') }}" style="padding: 10px 20px; background: #2196F3; color: white; text-decoration: none; border-radius: 4px;">
        Tambah Todo
    </a>

    <table style="width: 100%; border-collapse: collapse; margin-top: 20px;">
        <thead>
            <tr style="background: #4CAF50; color: white;">
                <th style="padding: 12px; border: 1px solid #ddd;">ID</th>
                <th style="padding: 12px; border: 1px solid #ddd;">Title</th>
                <th style="padding: 12px; border: 1px solid #ddd;">Description</th>
                <th style="padding: 12px; border: 1px solid #ddd;">Status</th>
                <th style="padding: 12px; border: 1px solid #ddd;">Aksi</th>
            </tr>
        </thead>
        <tbody>
            @forelse($todos as $todo)
                <tr>
                    <td style="padding: 12px; border: 1px solid #ddd;">{{ $todo->id }}</td>
                    <td style="padding: 12px; border: 1px solid #ddd;">{{ $todo->title }}</td>
                    <td style="padding: 12px; border: 1px solid #ddd;">{{ $todo->description ?? '-' }}</td>
                    <td style="padding: 12px; border: 1px solid #ddd;">
                        {{ $todo->completed ? 'Selesai' : 'Belum Selesai' }}
                    </td>
                    <td style="padding: 12px; border: 1px solid #ddd;">
                        <a href="{{ route('todos.edit', $todo) }}" style="padding: 5px 10px; background: #4CAF50; color: white; text-decoration: none; border-radius: 4px;">Edit</a>
                        <form action="{{ route('todos.destroy', $todo) }}" method="POST" style="display: inline;">
                            @csrf
                            @method('DELETE')
                            <button type="submit" style="padding: 5px 10px; background: #f44336; color: white; border: none; border-radius: 4px; cursor: pointer;" onclick="return confirm('Yakin ingin menghapus?')">Hapus</button>
                        </form>
                    </td>
                </tr>
            @empty
                <tr>
                    <td colspan="5" style="text-align: center; padding: 20px;">Tidak ada data</td>
                </tr>
            @endforelse
        </tbody>
    </table>
@endsection
```

### 4. Blade Directives

**@if, @elseif, @else, @endif:**
```blade
@if($todo->completed)
    <span>Selesai</span>
@else
    <span>Belum Selesai</span>
@endif

@if($count > 10)
    <p>Banyak data</p>
@elseif($count > 5)
    <p>Sedang</p>
@else
    <p>Sedikit</p>
@endif
```

**@foreach, @forelse:**
```blade
@foreach($todos as $todo)
    <p>{{ $todo->title }}</p>
@endforeach

@forelse($todos as $todo)
    <p>{{ $todo->title }}</p>
@empty
    <p>Tidak ada data</p>
@endforelse
```

**@for, @while:**
```blade
@for($i = 0; $i < 10; $i++)
    <p>Iterasi {{ $i }}</p>
@endfor

@while($condition)
    <p>Looping</p>
@endwhile
```

**@switch:**
```blade
@switch($status)
    @case('active')
        <span>Aktif</span>
        @break
    @case('inactive')
        <span>Tidak Aktif</span>
        @break
    @default
        <span>Unknown</span>
@endswitch
```

### 5. Components

**Membuat Component:**
```bash
php artisan make:component Alert
```

**File: `app/View/Components/Alert.php`**
```php
<?php

namespace App\View\Components;

use Illuminate\View\Component;

class Alert extends Component
{
    public $type;
    public $message;

    public function __construct($type = 'info', $message = '')
    {
        $this->type = $type;
        $this->message = $message;
    }

    public function render()
    {
        return view('components.alert');
    }
}
```

**File: `resources/views/components/alert.blade.php`**
```blade
@props(['type' => 'info', 'message' => ''])

<div class="alert alert-{{ $type }}">
    {{ $message }}
</div>
```

**Menggunakan Component:**
```blade
<x-alert type="success" message="Data berhasil disimpan" />
<x-alert type="error" message="Terjadi kesalahan" />
```

### 6. Include (Partial Views)

**File: `resources/views/partials/navbar.blade.php`**
```blade
<nav style="background: #4CAF50; padding: 20px;">
    <a href="{{ route('todos.index') }}" style="color: white; margin-right: 20px;">Todos</a>
    <a href="{{ route('todos.create') }}" style="color: white;">Tambah Todo</a>
</nav>
```

**Menggunakan Include:**
```blade
@include('partials.navbar')

@include('partials.navbar', ['active' => 'todos'])
```

**Include dengan Kondisi:**
```blade
@includeIf('partials.navbar')

@includeWhen($user, 'partials.user-info')
```

### 7. Stack dan Push

**Di Layout:**
```blade
@stack('styles')
@stack('scripts')
```

**Di View:**
```blade
@push('styles')
    <style>
        .custom-style { color: red; }
    </style>
@endpush

@push('scripts')
    <script>
        console.log('Hello');
    </script>
@endpush
```

### 8. Blade Helpers

**Echo Data:**
```blade
{{ $variable }}              <!-- Auto-escape -->
{!! $html !!}                <!-- No escape (hati-hati!) -->
{{ $variable ?? 'Default' }}  <!-- Null coalescing -->
```

**URL dan Route:**
```blade
{{ url('/todos') }}
{{ route('todos.index') }}
{{ asset('css/style.css') }}
```

**Old Input:**
```blade
<input value="{{ old('title', $todo->title ?? '') }}">
```

**CSRF Token:**
```blade
@csrf
```

**Method Spoofing:**
```blade
@method('PUT')
@method('DELETE')
```

### 9. Contoh Lengkap dengan Layout

**Layout: `resources/views/layouts/app.blade.php`**
```blade
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@yield('title', 'Laravel App')</title>
    <link rel="stylesheet" href="{{ asset('css/app.css') }}">
    @stack('styles')
</head>
<body>
    @include('partials.navbar')

    <main class="container">
        @yield('content')
    </main>

    @include('partials.footer')

    @stack('scripts')
</body>
</html>
```

**View: `resources/views/todos/create.blade.php`**
```blade
@extends('layouts.app')

@section('title', 'Tambah Todo')

@section('content')
    <h2>Tambah Todo</h2>

    @if($errors->any())
        <div class="alert alert-danger">
            <ul>
                @foreach($errors->all() as $error)
                    <li>{{ $error }}</li>
                @endforeach
            </ul>
        </div>
    @endif

    <form action="{{ route('todos.store') }}" method="POST">
        @csrf
        
        <div>
            <label>Title:</label>
            <input type="text" name="title" value="{{ old('title') }}" required>
            @error('title')
                <div class="error">{{ $message }}</div>
            @enderror
        </div>

        <div>
            <label>Description:</label>
            <textarea name="description">{{ old('description') }}</textarea>
        </div>

        <button type="submit">Simpan</button>
        <a href="{{ route('todos.index') }}">Batal</a>
    </form>
@endsection
```

---

## 📝 Pretest (3 Soal Ringkas)

1. **Apa itu Blade?**
   - Jawab: Blade adalah templating engine Laravel yang powerful dan mudah digunakan. Blade menyediakan syntax yang clean, inheritance, components, dan directives untuk membuat view yang dinamis.

2. **Apa fungsi @yield?**
   - Jawab: `@yield` digunakan di layout untuk menampilkan konten dari section yang didefinisikan di view child. Contoh: `@yield('content')` akan menampilkan konten dari `@section('content')` di view.

3. **Apa fungsi @foreach di Blade?**
   - Jawab: `@foreach` digunakan untuk melakukan perulangan melalui array atau collection. Contoh: `@foreach($todos as $todo) ... @endforeach`. Ada juga `@forelse` yang menangani kasus ketika data kosong.

---

## 🏠 PR (Pekerjaan Rumah)

### Tugas 1: Buat Layout Utama
Buat layout utama yang bisa digunakan di semua halaman.

**File: `resources/views/layouts/app.blade.php`**
- Header dengan navigation
- Main content area dengan `@yield('content')`
- Footer
- Title dinamis dengan `@yield('title')`
- Support untuk `@stack('styles')` dan `@stack('scripts')`

### Tugas 2: Gunakan Layout di 3 Halaman CRUD
Gunakan layout yang sudah dibuat di 3 halaman CRUD:
- `todos/index.blade.php` - List todos
- `todos/create.blade.php` - Form tambah
- `todos/edit.blade.php` - Form edit

**Contoh struktur:**
```blade
@extends('layouts.app')

@section('title', 'Todo List')

@section('content')
    <!-- Konten halaman -->
@endsection
```

**Bonus:**
- Buat partial untuk navbar (`partials/navbar.blade.php`)
- Buat partial untuk footer (`partials/footer.blade.php`)
- Gunakan `@include` untuk navbar dan footer di layout

---

## 💡 Tips

- Gunakan layout untuk menghindari duplikasi code
- `@yield` untuk konten utama, `@stack` untuk CSS/JS tambahan
- `@include` untuk partial views yang digunakan berulang
- `@forelse` lebih baik dari `@foreach` karena handle empty data
- Selalu gunakan `{{ }}` untuk echo (auto-escape), `{!! !!}` hanya jika benar-benar perlu
- Components berguna untuk reusable UI elements
- Gunakan `old()` untuk mempertahankan input setelah validation error

---

**Selamat belajar! 🚀**


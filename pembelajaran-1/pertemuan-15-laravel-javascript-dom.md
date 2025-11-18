# PERTEMUAN 15 — Laravel + JavaScript DOM

## 📚 Materi

### 1. Mengintegrasikan JavaScript dengan Laravel Blade

**Cara 1: Inline Script di Blade**
```blade
@extends('layouts.app')

@section('content')
    <div id="todo-list">
        <!-- Konten -->
    </div>

    <script>
        // JavaScript di sini
        console.log('Hello from Blade!');
    </script>
@endsection
```

**Cara 2: External JavaScript File**
```blade
@extends('layouts.app')

@section('content')
    <div id="todo-list">
        <!-- Konten -->
    </div>
@endsection

@push('scripts')
    <script src="{{ asset('js/todos.js') }}"></script>
@endpush
```

**Cara 3: Menggunakan @stack di Layout**
```blade
<!-- layouts/app.blade.php -->
<body>
    @yield('content')
    
    @stack('scripts')
</body>
```

### 2. Mengakses Data Laravel di JavaScript

**Menggunakan JSON:**
```blade
<script>
    // Data dari controller
    let todos = @json($todos);
    
    // Atau
    let todos = {!! json_encode($todos) !!};
    
    console.log(todos);
</script>
```

**Menggunakan Data Attribute:**
```blade
<div id="todo-container" 
     data-todos='@json($todos)'
     data-user-id="{{ auth()->id() }}">
</div>

<script>
    let container = document.querySelector('#todo-container');
    let todos = JSON.parse(container.dataset.todos);
    let userId = container.dataset.userId;
</script>
```

**Menggunakan Route di JavaScript:**
```blade
<script>
    // Route helper
    let deleteUrl = "{{ route('todos.destroy', ':id') }}";
    
    // Replace :id dengan actual ID
    function getDeleteUrl(id) {
        return deleteUrl.replace(':id', id);
    }
</script>
```

### 3. Dynamic Table dengan JavaScript

**HTML/Blade:**
```blade
<table id="todo-table">
    <thead>
        <tr>
            <th>ID</th>
            <th>Title</th>
            <th>Status</th>
            <th>Aksi</th>
        </tr>
    </thead>
    <tbody id="todo-tbody">
        @foreach($todos as $todo)
            <tr data-id="{{ $todo->id }}">
                <td>{{ $todo->id }}</td>
                <td class="todo-title">{{ $todo->title }}</td>
                <td class="todo-status">
                    <span class="status-badge {{ $todo->completed ? 'completed' : 'pending' }}">
                        {{ $todo->completed ? 'Selesai' : 'Belum Selesai' }}
                    </span>
                </td>
                <td>
                    <button class="btn-toggle" data-id="{{ $todo->id }}">
                        Toggle Status
                    </button>
                </td>
            </tr>
        @endforeach
    </tbody>
</table>
```

**JavaScript:**
```javascript
// Toggle status tanpa reload
document.querySelectorAll('.btn-toggle').forEach(button => {
    button.addEventListener('click', function() {
        let todoId = this.dataset.id;
        let row = this.closest('tr');
        let statusBadge = row.querySelector('.status-badge');
        
        // Update UI langsung (optimistic update)
        if (statusBadge.classList.contains('completed')) {
            statusBadge.classList.remove('completed');
            statusBadge.classList.add('pending');
            statusBadge.textContent = 'Belum Selesai';
        } else {
            statusBadge.classList.remove('pending');
            statusBadge.classList.add('completed');
            statusBadge.textContent = 'Selesai';
        }
        
        // Kirim request ke server (optional)
        // fetch(`/todos/${todoId}/toggle`, { method: 'POST' })
    });
});
```

### 4. Delete dengan Konfirmasi JavaScript

**Blade:**
```blade
<form action="{{ route('todos.destroy', $todo) }}" 
      method="POST" 
      class="delete-form"
      data-id="{{ $todo->id }}">
    @csrf
    @method('DELETE')
    <button type="submit" class="btn-delete">Hapus</button>
</form>
```

**JavaScript:**
```javascript
// Konfirmasi sebelum delete
document.querySelectorAll('.delete-form').forEach(form => {
    form.addEventListener('submit', function(event) {
        event.preventDefault();
        
        let todoId = this.dataset.id;
        let confirmed = confirm('Yakin ingin menghapus todo ini?');
        
        if (confirmed) {
            // Submit form
            this.submit();
        }
    });
});
```

**Atau dengan Sweet Alert (jika menggunakan library):**
```javascript
document.querySelectorAll('.delete-form').forEach(form => {
    form.addEventListener('submit', function(event) {
        event.preventDefault();
        
        Swal.fire({
            title: 'Yakin ingin menghapus?',
            text: "Data tidak bisa dikembalikan!",
            icon: 'warning',
            showCancelButton: true,
            confirmButtonColor: '#d33',
            cancelButtonColor: '#3085d6',
            confirmButtonText: 'Ya, hapus!'
        }).then((result) => {
            if (result.isConfirmed) {
                this.submit();
            }
        });
    });
});
```

### 5. Validasi Form dengan JavaScript

**Blade:**
```blade
<form id="todo-form" action="{{ route('todos.store') }}" method="POST">
    @csrf
    
    <div>
        <label>Title:</label>
        <input type="text" name="title" id="title" value="{{ old('title') }}">
        <span id="title-error" class="error"></span>
    </div>
    
    <div>
        <label>Description:</label>
        <textarea name="description" id="description">{{ old('description') }}</textarea>
        <span id="description-error" class="error"></span>
    </div>
    
    <button type="submit">Simpan</button>
</form>
```

**JavaScript:**
```javascript
let form = document.querySelector('#todo-form');
let titleInput = document.querySelector('#title');
let descriptionInput = document.querySelector('#description');
let titleError = document.querySelector('#title-error');
let descriptionError = document.querySelector('#description-error');

form.addEventListener('submit', function(event) {
    event.preventDefault();
    
    // Reset errors
    titleError.textContent = '';
    descriptionError.textContent = '';
    
    let isValid = true;
    
    // Validasi title
    if (titleInput.value.trim() === '') {
        titleError.textContent = 'Title harus diisi!';
        isValid = false;
    } else if (titleInput.value.length < 3) {
        titleError.textContent = 'Title minimal 3 karakter!';
        isValid = false;
    }
    
    // Validasi description (optional)
    if (descriptionInput.value.length > 500) {
        descriptionError.textContent = 'Description maksimal 500 karakter!';
        isValid = false;
    }
    
    if (isValid) {
        // Submit form
        this.submit();
    }
});

// Real-time validation
titleInput.addEventListener('input', function() {
    if (this.value.trim() !== '') {
        titleError.textContent = '';
    }
});
```

### 6. AJAX dengan Fetch API

**Update Status tanpa Reload:**
```javascript
function toggleTodoStatus(todoId) {
    fetch(`/todos/${todoId}/toggle`, {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            'X-CSRF-TOKEN': document.querySelector('meta[name="csrf-token"]').content
        },
        body: JSON.stringify({ id: todoId })
    })
    .then(response => response.json())
    .then(data => {
        if (data.success) {
            // Update UI
            let statusBadge = document.querySelector(`[data-id="${todoId}"] .status-badge`);
            statusBadge.textContent = data.completed ? 'Selesai' : 'Belum Selesai';
            statusBadge.className = `status-badge ${data.completed ? 'completed' : 'pending'}`;
        }
    })
    .catch(error => {
        console.error('Error:', error);
        alert('Terjadi kesalahan!');
    });
}
```

**Di Layout (tambahkan CSRF token):**
```blade
<head>
    <meta name="csrf-token" content="{{ csrf_token() }}">
</head>
```

### 7. Contoh Lengkap: Todo List Interaktif

**Controller:**
```php
public function index()
{
    $todos = Todo::all();
    return view('todos.index', compact('todos'));
}
```

**Blade: `todos/index.blade.php`**
```blade
@extends('layouts.app')

@section('content')
    <h2>Todo List</h2>
    
    <div id="todo-container" data-todos='@json($todos)'>
        <table>
            <thead>
                <tr>
                    <th>Title</th>
                    <th>Status</th>
                    <th>Aksi</th>
                </tr>
            </thead>
            <tbody id="todo-tbody">
                @foreach($todos as $todo)
                    <tr data-id="{{ $todo->id }}">
                        <td>{{ $todo->title }}</td>
                        <td>
                            <span class="status {{ $todo->completed ? 'completed' : 'pending' }}">
                                {{ $todo->completed ? 'Selesai' : 'Belum' }}
                            </span>
                        </td>
                        <td>
                            <button class="btn-toggle" data-id="{{ $todo->id }}">Toggle</button>
                            <form action="{{ route('todos.destroy', $todo) }}" method="POST" class="delete-form" style="display: inline;">
                                @csrf
                                @method('DELETE')
                                <button type="submit" class="btn-delete">Hapus</button>
                            </form>
                        </td>
                    </tr>
                @endforeach
            </tbody>
        </table>
    </div>
@endsection

@push('scripts')
    <script src="{{ asset('js/todos.js') }}"></script>
@endpush
```

**JavaScript: `public/js/todos.js`**
```javascript
// Konfirmasi delete
document.querySelectorAll('.delete-form').forEach(form => {
    form.addEventListener('submit', function(event) {
        event.preventDefault();
        
        if (confirm('Yakin ingin menghapus?')) {
            this.submit();
        }
    });
});

// Toggle status (UI only)
document.querySelectorAll('.btn-toggle').forEach(button => {
    button.addEventListener('click', function() {
        let row = this.closest('tr');
        let statusSpan = row.querySelector('.status');
        
        if (statusSpan.classList.contains('completed')) {
            statusSpan.classList.remove('completed');
            statusSpan.classList.add('pending');
            statusSpan.textContent = 'Belum';
        } else {
            statusSpan.classList.remove('pending');
            statusSpan.classList.add('completed');
            statusSpan.textContent = 'Selesai';
        }
    });
});
```

---

## 📝 Pretest (3 Soal Ringkas)

1. **Apa fungsi preventDefault()?**
   - Jawab: `preventDefault()` digunakan untuk mencegah behavior default dari event. Contoh: mencegah form submit default, mencegah link navigation, dll. Biasanya digunakan sebelum melakukan validasi atau AJAX request.

2. **Bagaimana cara menambah elemen ke halaman pakai JS?**
   - Jawab: 
   ```javascript
   // Buat element
   let newElement = document.createElement('div');
   newElement.textContent = 'Konten baru';
   
   // Tambahkan ke DOM
   let container = document.querySelector('#container');
   container.appendChild(newElement);
   ```

3. **Bagaimana menghubungkan Blade + JS?**
   - Jawab: 
     - Menggunakan inline script di Blade: `<script>...</script>`
     - Menggunakan external file dengan `@push('scripts')` dan `@stack('scripts')` di layout
     - Menggunakan `@json()` untuk pass data PHP ke JavaScript
     - Menggunakan data attributes untuk pass data ke JavaScript

---

## 🏠 PR (Pekerjaan Rumah)

### Tugas 1: Tambahkan JS untuk Konfirmasi Delete
Tambahkan JavaScript untuk konfirmasi delete di To-Do List.

**Fitur:**
- Saat klik tombol "Hapus", muncul konfirmasi
- Jika "OK", form di-submit
- Jika "Cancel", form tidak di-submit

**File:** Tambahkan ke file JavaScript yang sudah ada atau buat baru.

### Tugas 2: Tambahkan Validasi Form menggunakan JS
Tambahkan validasi form menggunakan JavaScript di halaman create/edit todo.

**Fitur:**
- Validasi title tidak boleh kosong
- Validasi title minimal 3 karakter
- Tampilkan error message di bawah input
- Mencegah form submit jika ada error
- Error hilang saat user mulai mengetik

**File:** Tambahkan ke file JavaScript atau buat file baru.

---

## 💡 Tips

- Gunakan `@json()` untuk pass data array/object dari PHP ke JavaScript
- Selalu validasi di client (JS) dan server (Laravel)
- `preventDefault()` untuk mencegah behavior default
- Gunakan `@push('scripts')` untuk organize JavaScript
- Data attributes (`data-*`) berguna untuk pass data ke JavaScript
- Fetch API untuk AJAX request (modern, built-in)
- Jangan lupa CSRF token untuk POST/PUT/DELETE request

---

**Selamat belajar! 🚀**


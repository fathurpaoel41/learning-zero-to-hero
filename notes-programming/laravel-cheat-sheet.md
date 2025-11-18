# Laravel Cheat Sheet

## Quick Reference untuk Laravel

### Artisan Commands
```bash
php artisan serve                    # Jalankan server
php artisan make:controller Name     # Buat controller
php artisan make:model Name          # Buat model
php artisan make:migration name      # Buat migration
php artisan migrate                  # Jalankan migration
php artisan migrate:rollback         # Rollback migration
php artisan route:list               # List semua route
php artisan tinker                   # Laravel REPL
```

### Routes
```php
// Basic
Route::get('/users', function() {
    return 'Users';
});

// Controller
Route::get('/users', [UserController::class, 'index']);

// Resource (CRUD)
Route::resource('users', UserController::class);

// Named route
Route::get('/hello', function() {
    return 'Hello';
})->name('hello');

// Redirect
return redirect()->route('users.index');
```

### Controller
```php
// Get request data
$request->input('name');
$request->get('name');
$request->all();

// Validation
$validated = $request->validate([
    'name' => 'required|max:255',
    'email' => 'required|email',
]);

// Return view
return view('users.index', compact('users'));
return view('users.index', ['users' => $users]);

// Return JSON
return response()->json($data);
```

### Model & Eloquent
```php
// Create
User::create(['name' => 'Budi']);
$user = new User();
$user->name = 'Budi';
$user->save();

// Read
User::all();
User::find(1);
User::where('age', '>', 18)->get();
User::first();
User::count();

// Update
$user = User::find(1);
$user->update(['name' => 'Siti']);
$user->name = 'Siti';
$user->save();

// Delete
$user->delete();
User::destroy(1);
User::where('age', '<', 18)->delete();
```

### Migration
```php
// Create table
Schema::create('users', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->timestamps();
});

// Modify table
Schema::table('users', function (Blueprint $table) {
    $table->string('email')->after('name');
});

// Drop column
$table->dropColumn('email');
```

### Blade Syntax
```blade
{{ $variable }}              {{-- Echo --}}
{!! $html !!}                {{-- Raw HTML --}}
@if($condition) ... @endif   {{-- If --}}
@foreach($items as $item) ... @endforeach
@forelse($items as $item) ... @empty ... @endforelse
@include('partials.nav')     {{-- Include --}}
@extends('layouts.app')      {{-- Extend --}}
@section('content') ... @endsection
@csrf                        {{-- CSRF token --}}
@method('PUT')               {{-- Method spoofing --}}
```

### Validation Rules
```php
'required'           // Wajib diisi
'nullable'           // Boleh kosong
'string'             // Harus string
'integer'            // Harus integer
'email'              // Format email
'max:255'            // Maksimal karakter
'min:18'             // Minimal nilai
'unique:users,email' // Harus unik
'confirmed'          // Harus sama dengan field_confirmation
```

### Common Helpers
```php
route('users.index');           // Generate URL route
url('/users');                  // Generate URL
asset('css/style.css');         // Asset URL
old('name', 'default');         // Old input
csrf_token();                   // CSRF token
dd($var);                       // Dump and die
dump($var);                     // Dump
```

### Database Query Builder
```php
DB::table('users')->get();
DB::table('users')->where('age', '>', 18)->get();
DB::table('users')->insert(['name' => 'Budi']);
DB::table('users')->where('id', 1)->update(['name' => 'Siti']);
DB::table('users')->where('id', 1)->delete();
```


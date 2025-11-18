# Contoh Project Sederhana

## Project untuk Latihan Setelah Menyelesaikan 18 Pertemuan

### 1. Aplikasi Catatan Harian

**Deskripsi:**
Aplikasi sederhana untuk mencatat kegiatan harian dengan kategori.

**Fitur:**
- CRUD catatan harian
- Kategori (kerja, personal, olahraga, dll)
- Filter berdasarkan kategori
- Pencarian catatan
- Tanggal otomatis

**Tech Stack:**
- Laravel
- MySQL
- JavaScript (DOM)
- Bootstrap (opsional)

**Database:**
```sql
categories (id, name, color)
notes (id, title, content, category_id, date, created_at, updated_at)
```

**Learning Points:**
- Relasi one-to-many
- Filter dan search
- Date handling

---

### 2. Sistem Manajemen Buku Perpustakaan

**Deskripsi:**
Aplikasi untuk mengelola data buku di perpustakaan sederhana.

**Fitur:**
- CRUD buku
- CRUD anggota
- Peminjaman buku
- Pengembalian buku
- Status buku (tersedia/dipinjam)

**Tech Stack:**
- Laravel
- MySQL
- JavaScript

**Database:**
```sql
books (id, title, author, isbn, status)
members (id, name, email, phone)
borrowings (id, book_id, member_id, borrow_date, return_date, status)
```

**Learning Points:**
- Relasi many-to-many
- Status management
- Date calculation

---

### 3. Aplikasi To-Do List dengan Prioritas

**Deskripsi:**
To-Do List yang lebih advanced dengan fitur prioritas dan due date.

**Fitur:**
- CRUD todo
- Prioritas (high, medium, low)
- Due date
- Status (pending, in progress, completed)
- Filter dan sort
- Reminder (opsional)

**Tech Stack:**
- Laravel
- MySQL
- JavaScript
- Date picker library

**Database:**
```sql
todos (id, title, description, priority, due_date, status, created_at, updated_at)
```

**Learning Points:**
- Enum/status handling
- Date comparison
- Sorting multiple columns

---

### 4. Sistem Absensi Sederhana

**Deskripsi:**
Aplikasi untuk mencatat absensi siswa/karyawan.

**Fitur:**
- CRUD siswa/karyawan
- Check in/out
- Laporan kehadiran
- Statistik kehadiran per bulan
- Export laporan (opsional)

**Tech Stack:**
- Laravel
- MySQL
- JavaScript
- Chart library (opsional)

**Database:**
```sql
students (id, name, email, phone)
attendances (id, student_id, date, check_in, check_out, status)
```

**Learning Points:**
- Time handling
- Report generation
- Statistics calculation

---

### 5. Aplikasi Manajemen Keuangan Pribadi

**Deskripsi:**
Aplikasi untuk mencatat pemasukan dan pengeluaran pribadi.

**Fitur:**
- CRUD transaksi
- Kategori (pemasukan/pengeluaran)
- Sub-kategori (makanan, transport, dll)
- Laporan bulanan
- Grafik pengeluaran (opsional)
- Budget limit per kategori (opsional)

**Tech Stack:**
- Laravel
- MySQL
- JavaScript
- Chart.js (untuk grafik)

**Database:**
```sql
categories (id, name, type, parent_id)
transactions (id, category_id, amount, description, date, type)
```

**Learning Points:**
- Financial calculation
- Chart integration
- Category hierarchy

---

### 6. Blog Sederhana

**Deskripsi:**
Aplikasi blog dengan fitur dasar.

**Fitur:**
- CRUD artikel
- Kategori artikel
- Tag (opsional)
- Comment (opsional)
- Search artikel
- Pagination

**Tech Stack:**
- Laravel
- MySQL
- JavaScript
- Rich text editor (opsional)

**Database:**
```sql
categories (id, name, slug)
posts (id, title, content, category_id, published_at, created_at)
tags (id, name)
post_tag (post_id, tag_id) -- many-to-many
```

**Learning Points:**
- Content management
- Many-to-many relationship
- Pagination
- Search functionality

---

### 7. Aplikasi Kontak (Phonebook)

**Deskripsi:**
Aplikasi untuk menyimpan kontak teman/keluarga.

**Fitur:**
- CRUD kontak
- Grup kontak (keluarga, teman, kerja)
- Pencarian kontak
- Export kontak (opsional)
- Import kontak (opsional)

**Tech Stack:**
- Laravel
- MySQL
- JavaScript

**Database:**
```sql
groups (id, name)
contacts (id, name, phone, email, address, group_id, created_at)
```

**Learning Points:**
- Simple CRUD
- Grouping data
- Search functionality

---

## Tips Memilih Project

1. **Mulai dari yang Paling Menarik**
   - Pilih project yang sesuai minat
   - Akan lebih termotivasi

2. **Mulai dari yang Sederhana**
   - Jangan langsung project kompleks
   - Tambah fitur secara bertahap

3. **Fokus pada Learning**
   - Setiap project harus ada hal baru
   - Challenge diri sendiri

4. **Buat Portfolio**
   - Project bisa jadi portfolio
   - Tunjukkan ke orang lain
   - Deploy ke internet (opsional)

## Workflow Membuat Project

1. **Planning (1-2 hari)**
   - Tentukan fitur
   - Design database
   - Buat wireframe sederhana

2. **Setup (1 hari)**
   - Install Laravel
   - Setup database
   - Buat migration

3. **Development (1-2 minggu)**
   - Buat CRUD dasar
   - Tambah fitur satu per satu
   - Test setiap fitur

4. **Polish (3-5 hari)**
   - Fix bugs
   - Improve UI/UX
   - Add validation
   - Add error handling

5. **Deploy (opsional)**
   - Deploy ke hosting
   - Test di production
   - Share ke orang lain

---

**Selamat membuat project! 🚀**


# PERTEMUAN 18 — Review & Ujian Mini

## 📚 Materi

### 1. Review Alur CRUD

**CRUD (Create, Read, Update, Delete)** adalah operasi dasar untuk mengelola data.

**Alur CRUD di Laravel:**

1. **Create (Membuat Data Baru)**
   - User mengisi form → Submit
   - Request masuk ke Controller `store()`
   - Validasi data
   - Simpan ke database via Model
   - Redirect dengan flash message

2. **Read (Membaca Data)**
   - Request ke Controller `index()` atau `show()`
   - Ambil data dari database via Model
   - Tampilkan di View

3. **Update (Mengubah Data)**
   - User klik Edit → Form terisi data lama
   - User ubah data → Submit
   - Request masuk ke Controller `update()`
   - Validasi data
   - Update database via Model
   - Redirect dengan flash message

4. **Delete (Menghapus Data)**
   - User klik Delete → Konfirmasi
   - Request masuk ke Controller `destroy()`
   - Hapus dari database via Model
   - Redirect dengan flash message

**Diagram Alur:**
```
User → Route → Controller → Model → Database
                ↓
              View ← Data
```

### 2. Best Practices Development

**1. Code Organization**
- Pisahkan kode sesuai struktur MVC
- Gunakan namespace yang jelas
- Buat folder untuk mengorganisir file

**2. Security**
- Selalu validasi input (client & server)
- Gunakan CSRF token untuk form
- Gunakan prepared statements (Eloquent sudah handle)
- Sanitize input sebelum disimpan
- Jangan expose sensitive data di frontend

**3. Database**
- Gunakan migration untuk version control
- Gunakan foreign key untuk relasi
- Index kolom yang sering di-query
- Normalisasi database (hindari duplikasi)

**4. Error Handling**
- Tampilkan error yang user-friendly
- Log error untuk debugging
- Validasi dengan pesan yang jelas
- Handle exception dengan try-catch

**5. Performance**
- Gunakan eager loading untuk relasi (with())
- Cache query yang sering digunakan
- Optimize gambar dan assets
- Minimize database query

**6. Code Quality**
- Gunakan naming convention yang konsisten
- Comment kode yang kompleks
- Refactor code yang duplikat
- Follow PSR standards (PHP)

**7. Testing**
- Test semua fitur sebelum deploy
- Test edge cases
- Test validasi form
- Test error handling

### 3. Debugging Dasar

**Laravel Debugging:**
```php
// dd() - Dump and Die
dd($variable);

// dump() - Dump tanpa stop
dump($variable);

// Log
\Log::info('Pesan log');
\Log::error('Error message');

// Debug di Blade
{{ dd($variable) }}
```

**JavaScript Debugging:**
```javascript
// console.log()
console.log('Debug message', variable);

// console.error()
console.error('Error message');

// console.table() untuk array/object
console.table(data);

// debugger (breakpoint)
debugger; // Akan pause di browser DevTools
```

**Database Debugging:**
```php
// Enable query log
DB::enableQueryLog();
// ... kode query ...
dd(DB::getQueryLog());

// Atau di .env
DB_LOG_QUERIES=true
```

### 4. Common Issues & Solutions

**Issue 1: Route tidak ditemukan**
- **Solusi:** Cek `php artisan route:list`, pastikan route sudah terdaftar

**Issue 2: Class not found**
- **Solusi:** Run `composer dump-autoload`, cek namespace

**Issue 3: Database connection error**
- **Solusi:** Cek `.env` file, pastikan kredensial benar

**Issue 4: 419 Page Expired (CSRF)**
- **Solusi:** Pastikan form ada `@csrf`, cek session config

**Issue 5: JavaScript tidak jalan**
- **Solusi:** Cek console browser untuk error, pastikan file JS ter-load

**Issue 6: Data tidak muncul**
- **Solusi:** Cek query di database, cek variable di view, gunakan `dd()`

### 5. Tips untuk Final Project

**Planning:**
1. Tentukan fitur yang akan dibuat
2. Buat database schema
3. Buat migration
4. Buat model dan relasi
5. Buat controller dan route
6. Buat view dengan layout
7. Tambahkan JavaScript untuk interaktivitas
8. Test semua fitur
9. Fix bugs
10. Deploy (opsional)

**Checklist:**
- ✅ Database migration berjalan
- ✅ Model dengan fillable/guarded
- ✅ Controller dengan validasi
- ✅ Route terdaftar
- ✅ View dengan layout
- ✅ Form validation (client & server)
- ✅ Flash message
- ✅ Error handling
- ✅ JavaScript untuk UX
- ✅ Responsive design (opsional)

---

## 📝 Pretest (3 Soal Ringkas)

1. **Jelaskan alur CRUD secara singkat.**
   - Jawab: 
     - **Create:** Form input → Controller store() → Validasi → Model create() → Database → Redirect
     - **Read:** Route → Controller index()/show() → Model query → View tampilkan
     - **Update:** Form edit → Controller update() → Validasi → Model update() → Database → Redirect
     - **Delete:** Konfirmasi → Controller destroy() → Model delete() → Database → Redirect

2. **Sebutkan 3 best practice development.**
   - Jawab: 
     1. **Security** - Validasi input, CSRF token, sanitize data
     2. **Code Organization** - Struktur MVC, namespace jelas, folder terorganisir
     3. **Error Handling** - Tampilkan error user-friendly, log error, handle exception
     (Lainnya: Database normalization, Performance optimization, Code quality, Testing)

3. **Apa bagian paling sulit menurut kamu dari 17 pertemuan sebelumnya?**
   - Jawab: (Jawaban personal, contoh)
     - Relasi database dan JOIN
     - JavaScript DOM manipulation
     - Integrasi Laravel + JavaScript
     - Validasi form yang kompleks
     - Debugging error

---

## 🏠 PR (Final Project)

### Tugas: Bangun Aplikasi Sederhana Laravel

Pilih salah satu aplikasi berikut dan buat dengan fitur lengkap:

#### Pilihan 1: To-Do List Advanced
**Fitur Wajib:**
- ✅ CRUD lengkap untuk todo
- ✅ Kategori todo (relasi)
- ✅ Status (pending, in progress, completed)
- ✅ Validasi form
- ✅ JavaScript DOM untuk toggle status tanpa reload
- ✅ Layout yang rapi
- ✅ Search/filter todo

**Fitur Bonus:**
- Due date dengan reminder
- Priority (high, medium, low)
- Mark all as completed
- Export to PDF/Excel

#### Pilihan 2: Aplikasi Catatan Pengeluaran
**Fitur Wajib:**
- ✅ CRUD untuk catatan pengeluaran
- ✅ Kategori pengeluaran (makanan, transport, dll)
- ✅ Tanggal dan jumlah
- ✅ Validasi form
- ✅ JavaScript untuk kalkulasi total
- ✅ Layout yang rapi
- ✅ Filter by kategori/tanggal

**Fitur Bonus:**
- Grafik pengeluaran per bulan
- Budget limit per kategori
- Export laporan
- Dashboard dengan statistik

#### Pilihan 3: Sistem Absensi Sederhana
**Fitur Wajib:**
- ✅ CRUD untuk data siswa/karyawan
- ✅ CRUD untuk absensi (check in/out)
- ✅ Relasi antara siswa dan absensi
- ✅ Validasi form
- ✅ JavaScript untuk validasi waktu
- ✅ Layout yang rapi
- ✅ Filter by tanggal/nama

**Fitur Bonus:**
- Laporan kehadiran per bulan
- Statistik kehadiran
- Export laporan
- QR Code untuk check in (advanced)

### Kriteria Penilaian

**Wajib (60%):**
- ✅ CRUD lengkap dan berfungsi
- ✅ Relasi database (minimal 2 tabel)
- ✅ Validasi form (client & server)
- ✅ JavaScript DOM untuk interaktivitas
- ✅ Layout konsisten dan rapi
- ✅ Flash message dan error handling

**Kualitas Code (20%):**
- ✅ Struktur code terorganisir
- ✅ Mengikuti best practices
- ✅ Code mudah dibaca dan dipahami
- ✅ Comment untuk bagian kompleks

**Fitur Tambahan (20%):**
- ✅ Fitur bonus yang berfungsi
- ✅ UX yang baik
- ✅ Design yang menarik
- ✅ Responsive (opsional)

### Deliverables

1. **Source Code**
   - Semua file Laravel project
   - Database migration
   - JavaScript files

2. **Database**
   - SQL dump atau migration
   - Seeder untuk data contoh (opsional)

3. **Dokumentasi Singkat**
   - Cara install dan setup
   - Fitur yang dibuat
   - Screenshot aplikasi (opsional)

### Timeline

- **Minggu 1:** Planning, database design, migration
- **Minggu 2:** Model, Controller, basic CRUD
- **Minggu 3:** View, layout, JavaScript
- **Minggu 4:** Testing, bug fixing, dokumentasi

---

## 💡 Tips Final Project

1. **Mulai dari yang sederhana** - Buat CRUD dasar dulu, baru tambah fitur
2. **Test setiap fitur** - Jangan tunggu sampai selesai semua
3. **Gunakan Git** - Commit setiap progress penting
4. **Jangan takut error** - Error adalah bagian dari belajar
5. **Referensi** - Gunakan dokumentasi Laravel dan Stack Overflow
6. **Ask for help** - Jika stuck, tanya mentor atau teman
7. **Documentation** - Tulis dokumentasi untuk diri sendiri
8. **Have fun!** - Nikmati proses belajar dan membuat project

---

## 🎓 Kesimpulan 18 Pertemuan

Selamat! Anda telah menyelesaikan 18 pertemuan pembelajaran pemrograman:

1. ✅ Fundamental Pemrograman (variabel, tipe data, operator)
2. ✅ Function & Control Flow
3. ✅ PHP Native Dasar
4. ✅ PHP OOP
5. ✅ MySQL Basic
6. ✅ SQL CRUD
7. ✅ PHP Native + MySQL
8. ✅ Mini Project PHP Native CRUD
9. ✅ Laravel Intro
10. ✅ Migration & Model
11. ✅ Laravel CRUD
12. ✅ Laravel Blade Advanced
13. ✅ JavaScript Fundamental
14. ✅ JavaScript DOM
15. ✅ Laravel + JavaScript DOM
16. ✅ Laravel To-Do List Project
17. ✅ MySQL Advanced Basic
18. ✅ Review & Ujian Mini

**Skill yang Dikuasai:**
- ✅ Pemrograman dasar (PHP, JavaScript)
- ✅ Database (MySQL, SQL)
- ✅ Framework Laravel
- ✅ Frontend (HTML, CSS, JavaScript, Blade)
- ✅ CRUD Application
- ✅ Best Practices Development

**Langkah Selanjutnya:**
- Terus berlatih dengan membuat project sendiri
- Pelajari fitur Laravel lebih lanjut (Authentication, API, dll)
- Pelajari JavaScript framework (Vue.js, React)
- Pelajari deployment (hosting, server)
- Join komunitas developer
- Terus belajar dan berkembang!

---

**Selamat! Anda telah menyelesaikan kursus ini! 🎉🚀**


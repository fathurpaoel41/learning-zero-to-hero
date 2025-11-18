# Git Cheat Sheet

## Quick Reference untuk Git & GitHub

### Konfigurasi Awal
Setup awal Git di komputer Anda.

```bash
# Set nama pengguna global
git config --global user.name "Nama Anda"

# Set email global
git config --global user.email "email@anda.com"
```
**Deskripsi:** Konfigurasi ini digunakan untuk mengidentifikasi siapa yang membuat commit. Cukup dilakukan sekali per instalasi Git.

---

### Inisialisasi & Kloning
Memulai repository baru atau mengambil yang sudah ada.

```bash
# Inisialisasi repository baru di folder yang ada
git init

# Kloning (download) repository yang sudah ada
git clone [url_repository]
```
**Deskripsi:**
- `git init`: Membuat repository Git baru di direktori saat ini.
- `git clone`: Membuat salinan lokal dari repository remote (misalnya dari GitHub).

---

### Commit (Menyimpan Perubahan)
Merekam perubahan ke dalam riwayat repository.

```bash
# Menambahkan file ke staging area
git add [nama_file]
git add . # Menambahkan semua file yang berubah

# Melihat status perubahan
git status

# Merekam perubahan yang ada di staging area (commit)
git commit -m "Pesan commit yang deskriptif"

# Menggabungkan add dan commit untuk file yang sudah di-track
git commit -am "Pesan commit"
```
**Deskripsi:**
- **Staging Area:** Tempat persiapan file sebelum di-commit.
- `git add`: Memindahkan perubahan dari working directory ke staging area.
- `git commit`: Menyimpan snapshot dari staging area ke dalam riwayat commit.

---

### Remote (Repository Online)
Mengelola koneksi ke repository remote (seperti GitHub).

```bash
# Melihat daftar remote
git remote -v

# Menambahkan remote baru
git remote add [nama_remote] [url_repository]
# Contoh: git remote add origin git@github.com:user/repo.git

# Menghapus remote
git remote remove [nama_remote]

# Mengganti URL remote
git remote set-url [nama_remote] [url_baru]
```
**Deskripsi:** Remote adalah versi repository Anda yang di-hosting di internet atau jaringan lain. `origin` adalah nama default yang umum digunakan untuk remote utama.

---

### Push (Mengirim Perubahan)
Mengirim commit dari repository lokal ke repository remote.

```bash
# Push ke remote
git push [nama_remote] [nama_branch]
# Contoh: git push origin master

# Push dan set branch lokal untuk melacak branch remote
git push -u origin master
```
**Deskripsi:** `git push` digunakan untuk mengunggah konten repository lokal ke remote. Ini adalah cara Anda berbagi perubahan dengan orang lain.

---

### Pull (Menarik Perubahan)
Mengambil dan menggabungkan perubahan dari repository remote ke repository lokal.

```bash
# Pull dari remote
git pull [nama_remote] [nama_branch]
# Contoh: git pull origin master
```
**Deskripsi:** `git pull` adalah gabungan dari dua perintah: `git fetch` (mengambil perubahan dari remote) dan `git merge` (menggabungkan perubahan tersebut ke branch lokal Anda).

---

### Merge (Menggabungkan Branch)
Menggabungkan riwayat dari branch yang berbeda ke dalam branch saat ini.

```bash
# Pindah ke branch tujuan (misal: master)
git checkout master

# Gabungkan branch lain (misal: feature) ke branch saat ini
git merge [nama_branch_sumber]
# Contoh: git merge feature-login
```
**Deskripsi:** `git merge` digunakan untuk mengintegrasikan perubahan dari satu branch ke branch lain. Ini sering digunakan setelah menyelesaikan pekerjaan di sebuah *feature branch*.

---

### Reset (Membatalkan Perubahan)
Mengembalikan kondisi repository ke commit tertentu. **Hati-hati, bisa menghapus riwayat!**

```bash
# Reset ke commit sebelumnya, menjaga perubahan di working directory
git reset --soft HEAD~1

# Reset ke commit sebelumnya, menjaga perubahan di staging
git reset --mixed HEAD~1 # Ini adalah default

# Reset ke commit sebelumnya, MENGHAPUS semua perubahan
git reset --hard HEAD~1

# Membatalkan 'git add' (unstage)
git reset [nama_file]
```
**Deskripsi:**
- `--soft`: Membatalkan commit, tapi file tetap di staging.
- `--mixed`: Membatalkan commit dan staging, tapi file tetap di working directory.
- `--hard`: Menghapus commit, staging, dan perubahan di working directory. Sangat destruktif.

---

### Undo Last Commit (Membatalkan Commit Terakhir)
Cara aman untuk membatalkan commit terakhir.

```bash
# Jika belum di-push ke remote (paling aman)
# Membatalkan commit tapi menyimpan perubahan
git reset HEAD~1

# Jika sudah di-push (membuat commit baru yang membatalkan)
git revert HEAD
```
**Deskripsi:**
- `git reset HEAD~1`: (Lihat di atas) Cocok jika commit hanya ada di lokal.
- `git revert`: Membuat commit baru yang isinya adalah kebalikan dari commit yang ingin dibatalkan. Ini lebih aman untuk riwayat yang sudah di-share karena tidak mengubah history yang ada.

---

### Stash (Menyimpan Sementara)
Menyimpan perubahan yang belum di-commit secara sementara.

```bash
# Menyimpan perubahan saat ini ke stash
git stash

# Melihat daftar stash
git stash list

# Mengembalikan stash terakhir dan menghapusnya dari daftar
git stash pop

# Mengembalikan stash terakhir tanpa menghapusnya
git stash apply

# Menghapus semua stash
git stash clear
```
**Deskripsi:** `git stash` sangat berguna ketika Anda sedang mengerjakan sesuatu, tetapi harus pindah branch untuk memperbaiki bug mendadak. Anda bisa men-"stash" perubahan Anda, pindah branch, dan kembali lagi nanti untuk mengambil stash tersebut.

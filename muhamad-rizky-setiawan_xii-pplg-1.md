# Studi Kasus: Penerapan Fitur Baru (Feature Branch Workflow)

**Nama** : Muhamad Rizky Setiawan
**Kelas** : XII PPLG 1
**Proyek** : Website UMKM, fitur *Autentikasi Multi-Faktor (MFA)*

---

## 1. Keuntungan Utama Pembatasan Branch `main`

Branch `main` adalah kode yang berjalan di lingkungan produksi. Membatasi commit langsung ke `main` memberikan keuntungan berikut:

1. **Menjaga stabilitas produksi.** Kode yang belum diuji tidak langsung masuk ke sistem yang dipakai pengguna, sehingga risiko *production crash* berkurang drastis.
2. **Adanya Code Review (Quality Gate).** Setiap perubahan harus melalui Pull Request dan ditinjau anggota lain, sehingga bug, celah keamanan, dan kode yang tidak sesuai standar bisa ditemukan lebih awal.
3. **Isolasi pekerjaan.** Fitur yang masih setengah jadi (misalnya MFA) dikerjakan di branch terpisah dan tidak mengganggu fitur lain atau pekerjaan anggota tim yang lain.
4. **Kolaborasi paralel yang aman.** Banyak anggota bisa mengerjakan fitur berbeda pada waktu bersamaan tanpa saling menimpa kode.
5. **Riwayat perubahan yang rapi dan mudah ditelusuri.** Setiap fitur memiliki branch dan PR sendiri, sehingga mudah dilacak siapa mengubah apa dan mengapa.
6. **Rollback lebih mudah.** Jika sebuah fitur bermasalah, cukup *revert* PR terkait tanpa merusak bagian lain.
7. **Mendukung otomatisasi (CI/CD).** Tes otomatis dapat dijalankan pada setiap PR sebelum kode digabung ke `main`.

---

## 2. Perintah Git: Feature Branch Workflow (Fitur MFA)

### Langkah 0: Fork & Clone (alur pengumpulan tugas)

```bash
# Fork repository guru lewat tombol "Fork" di GitHub, lalu clone hasil fork
git clone https://github.com/Japar-sodik/sts_version_control.git
cd sts_version_control

# (Opsional) Tambahkan repository guru sebagai upstream agar tetap sinkron
git remote add upstream https://github.com/Japar-sodik/sts_version_control.git
git remote -v
```

### Langkah 1: Sinkronisasi `main` terbaru

```bash
git checkout main
git pull upstream main
```

### Langkah 2: Membuat branch fitur terpisah


# Untuk pengerjaan fitur MFA di proyek UMKM
git checkout -b feature/mfa-authentication

# Untuk pengumpulan tugas STS (format: jawaban-[nama-siswa_kelas])
git checkout -b jawaban-nama-siswa_kelas

# Cek branch aktif
git branch

> Catatan: perintah modern yang setara adalah `git switch -c feature/mfa-authentication`.

### Langkah 3: Mengerjakan fitur dan menyimpan progres lokal

```bash
# Cek file yang berubah
git status

# Tambahkan file ke staging area
git add .
# atau per file:
git add auth/mfa.js routes/auth.js

# Commit bertahap dengan pesan terstruktur (Conventional Commits)
git commit -m "feat(auth): tambahkan generate secret dan QR code TOTP untuk MFA"
git commit -m "feat(auth): tambahkan verifikasi kode OTP saat login"
git commit -m "test(auth): tambahkan unit test verifikasi OTP"
git commit -m "docs(auth): tambahkan dokumentasi penggunaan MFA"

# Lihat riwayat commit
git log --oneline
```

### Langkah 4: Sinkronisasi sebelum push (menghindari konflik)

```bash
git fetch upstream
git rebase upstream/main
# jika ada konflik: perbaiki file, lalu
# git add <file>
# git rebase --continue
```

### Langkah 5: Push branch ke GitHub

```bash
git push -u origin feature/mfa-authentication
# untuk tugas STS:
git push -u origin jawaban-nama-siswa_kelas
```

### Langkah 6: Membuat Pull Request

1. Buka repository di GitHub, klik **Compare & pull request**.
2. Base repository: repository guru, branch `main`. Compare: branch fitur/jawaban Anda.
3. Isi judul dan deskripsi PR, lalu klik **Create pull request**.
4. Kirim link PR sebagai bukti pengumpulan.

---

## 3. Commit Pesan Terstruktur untuk Pengumpulan Tugas

```bash
git add nama_kelas.md
git commit -m "docs: tambahkan jawaban studi kasus feature branch workflow" \
           -m "- Jelaskan keuntungan pembatasan branch main
- Tuliskan perintah Git dari pembuatan branch hingga push
- Siapkan Pull Request untuk ditinjau guru" \
           -m "Refs: STS Version Control"

git push -u origin jawaban-nama-siswa_kelas
```

**Format pesan commit:**

```
<type>(<scope>): <deskripsi singkat>

<body: penjelasan apa dan mengapa>

<footer: referensi/issue>
```

| Type | Kegunaan |
|------|----------|
| `feat` | Menambah fitur baru |
| `fix` | Memperbaiki bug |
| `docs` | Perubahan dokumentasi |
| `test` | Menambah atau mengubah tes |
| `refactor` | Merapikan kode tanpa mengubah fungsi |
| `chore` | Tugas pemeliharaan (konfigurasi, dependensi) |

---

## 4. Template Deskripsi Pull Request

```markdown
## Deskripsi
Menambahkan fitur Autentikasi Multi-Faktor (MFA) berbasis TOTP.

## Perubahan
- Generate secret dan QR code
- Verifikasi kode OTP saat login
- Unit test dan dokumentasi

## Cara Menguji
1. Login dengan akun uji
2. Scan QR code dengan aplikasi authenticator
3. Masukkan kode OTP dan pastikan login berhasil

## Checklist
- [x] Kode sudah diuji secara lokal
- [x] Tidak ada commit langsung ke main
- [x] Siap ditinjau (review)
```

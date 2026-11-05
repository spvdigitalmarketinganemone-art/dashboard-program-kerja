# Panduan Setup Dashboard di GitHub

Akun GitHub: akun tim DM (spv.digitalmarketinganemone@gmail.com). Nama menu di GitHub dan Cloudflare bisa sedikit berbeda dari panduan ini kalau tampilannya diperbarui.

## 1. Buat repo
1. Login ke GitHub dengan akun tim DM.
2. Buat repository baru, misalnya `dashboard-program-kerja`. Pilih **Private** (ubah ke Public hanya kalau memakai Opsi B).
3. Upload semua isi folder ini (ekstrak dulu file zip-nya), termasuk folder `data/` dan `docs/`, lalu commit ke branch `main`.

## 2. Hosting dashboard
Pilih salah satu.

**Yang dipakai sekarang (sejak 5 Nov 2026): Opsi B, GitHub Pages.** Repo `spvdigitalmarketinganemone-art/dashboard-program-kerja` sudah **Public**, dan dashboard ada di https://reporting.anemoneindonesia.com. Opsi A belum bisa dipakai karena paket Zero Trust Free butuh kartu untuk verifikasi.

### Opsi A (lebih aman, belum dipakai): Cloudflare Workers + Access — hanya tim yang bisa membuka
Gratis, dan dashboard hanya bisa dibuka oleh email yang diizinkan. Paket Zero Trust Free tetap meminta kartu untuk verifikasi.

**Kalau dibuat (Cloudflare sekarang membuatnya sebagai Worker, bukan Pages):**
1. Buat akun Cloudflare (gratis).
2. Di menu **Workers & Pages**, buat project baru dan sambungkan ke repo GitHub `dashboard-program-kerja`.
3. Pengaturan build: build command **dikosongkan**, deploy command `npx wrangler deploy`, root directory `/`.
4. Setelah deploy, dashboard punya alamat `dashboard-program-kerja.<nama-akun>.workers.dev`.
5. Di Worker tersebut buka **Settings → Domains & Routes**, lalu klik **Enable Access** di baris **Production** dan **Preview**. Keduanya harus aktif. Kalau masih tertulis "Anyone with this URL can visit", dashboard dan file data masih terbuka untuk publik.
6. Di **Zero Trust → Access → Applications**, atur policy aplikasinya: **Allow**, hanya email anggota tim dan management, login dengan **One-time PIN** (kode yang dikirim ke email). Paket gratis cukup untuk sampai 50 orang.
7. Tes di jendela incognito: halaman login Cloudflare harus muncul sebelum dashboard, dan email yang tidak terdaftar harus ditolak.
8. Setiap kali bot menyimpan perubahan di GitHub, Cloudflare otomatis memperbarui dashboard dalam 1–2 menit.

### Opsi B (dipakai sekarang): GitHub Pages — siapa pun yang punya link bisa membuka
Lebih sederhana, tapi **datanya terbuka**. Di paket GitHub gratis, GitHub Pages hanya bisa dipakai kalau repo-nya **Public**, artinya file data (budget, CPMQL), dokumen, dan seluruh riwayat commit juga bisa dibaca siapa saja.
1. Ubah repo menjadi Public (Settings → General).
2. Settings → **Pages** → Source: **Deploy from a branch**, branch `main`, folder `/ (root)`.
3. Alamat dashboard: https://reporting.anemoneindonesia.com (alamat bawaan `https://spvdigitalmarketinganemone-art.github.io/dashboard-program-kerja/` otomatis dialihkan ke sini).
   - Custom domain diatur di Settings → Pages → Custom domain = `reporting.anemoneindonesia.com`, centang **Enforce HTTPS**.
   - Di DNS anemoneindonesia.com (panel serverorange): record **CNAME** `reporting` → `spvdigitalmarketinganemone-art.github.io`.
   - Kalau dashboard sudah tidak dipakai atau repo dihapus, **hapus juga record CNAME itu** supaya subdomain tidak bisa diklaim orang lain di GitHub Pages.
4. Setiap kali bot menyimpan perubahan di GitHub, GitHub Pages memperbarui dashboard dalam 1–2 menit.
5. Worker Cloudflare lama (`dashboard-program-kerja.spv-digitalmarketinganemone.workers.dev`) tidak dipakai lagi. Hapus atau matikan Worker URL-nya di Cloudflare supaya tidak ada dua alamat dashboard.

## 3. Akses GitHub untuk bot
1. Di akun GitHub tim DM: Settings → Developer settings → **Personal access tokens → Fine-grained tokens** → Generate new token.
2. **Repository access:** pilih *Only select repositories* → `dashboard-program-kerja` saja.
3. **Permissions:** *Contents* = **Read and write**. Biarkan yang lain tanpa akses.
4. Beri masa berlaku (misalnya 90 hari) dan catat kapan harus diperpanjang.
5. Pasang token itu langsung di pengaturan bot. **Jangan kirim token lewat chat** atau simpan di repo.

## 4. Sambungkan bot
1. Berikan isi `INSTRUKSI-BOT.md` sebagai instruksi bot (misalnya di bot Report Dashboard atau di MausBot).
2. Pastikan bot punya kemampuan membaca dan menulis file di repo GitHub memakai token di atas.
3. Uji dengan report kecil, misalnya: "Minggu 1: rapat program outlet & franchise organik sudah jalan 5 Nov." Cek di tab **Commits** bahwa hanya `data/todo_nov.json` dan `data/meta.json` yang berubah, lalu cek dashboard.

## 5. Bagikan
- Tulis link dashboard di `README.md`.
- Opsi A: tambahkan email anggota tim dan management di aturan Access.
- Repo GitHub cukup diakses SPV (dan bot). Tim dan management hanya perlu link dashboard.

## Kalau dashboard tampil kosong atau "Failed to load"
Biasanya ada file JSON yang formatnya rusak. Buka tab **Commits**, lihat perubahan terakhir di folder `data/`, lalu kembalikan (revert) commit itu atau minta bot memperbaikinya.

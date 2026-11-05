# Dashboard Program Kerja Digital Marketing — Anemone Indonesia

Dashboard interaktif program kerja bulanan tim Digital Marketing: target & to-do mingguan, Ads Franchise (Paid Ads), Organik (SMS), dan rekap target vs realisasi per bulan.

- **Dashboard:** https://reporting.anemoneindonesia.com
- **Hosting:** GitHub Pages dari branch `main` (repo **Public**), dengan custom domain `reporting.anemoneindonesia.com` (record CNAME di DNS serverorange → `spvdigitalmarketinganemone-art.github.io`). File `CNAME` di repo jangan dihapus. Dashboard dan semua isi repo bisa dibuka siapa saja yang punya link. Otomatis diperbarui setiap ada commit ke `main`.
- **Program kerja November 2026 (teks):** [docs/program-kerja-dm-november-2026.md](docs/program-kerja-dm-november-2026.md)

## Cara update
Update dilakukan **lewat bot**, bukan diedit manual:
1. SPV mengirim report ke bot.
2. Bot mengubah file di folder `data/` sesuai aturan di [INSTRUKSI-BOT.md](INSTRUKSI-BOT.md).
3. Dashboard otomatis menampilkan angka terbaru 1–2 menit kemudian.

Riwayat semua perubahan bisa dilihat di tab **Commits** repo ini.

## Isi repo
| Path | Isi |
|---|---|
| `index.html` | Halaman dashboard (jangan diubah bot) |
| `data/kpi_mingguan.json` | Target & aktual KPI per minggu |
| `data/todo_nov.json` | To-do November: target, status, hasil aktual |
| `data/pacing_nov.json` | Pacing Ads Franchise per minggu |
| `data/views_checkpoint.json` | Checkpoint views organik |
| `data/rekap_bulanan.json` | Target vs realisasi per bulan |
| `data/funnel_bulanan.json`, `okt_mingguan.json`, `creative_meta_okt.json`, `organik_akun.json`, `target_nov.json` | Data acuan Oktober & target November |
| `data/meta.json` | Waktu update terakhir |
| `INSTRUKSI-BOT.md` | Aturan untuk bot yang meng-update data |
| `PANDUAN-SETUP.md` | Cara hosting dan menyambungkan bot |

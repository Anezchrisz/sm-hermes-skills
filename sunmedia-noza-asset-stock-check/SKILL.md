---
name: sunmedia-noza-asset-stock-check
description: Menilai stok aset mentah klien (foto/video existing) vs kebutuhan syuting baru, sebelum menulis naskah video yang bergantung pada footage tertentu. Jalankan setelah sunmedia-noza-task-brief-intake, sebelum sunmedia-noza-video-script-brief.
---

# Cek Stok Aset Klien

Prosedur ini menjawab satu pertanyaan per naskah video: **apakah footage yang dibutuhkan sudah ada, atau perlu syuting baru?** Jangan menulis naskah yang mengasumsikan footage tertentu tanpa melewati pengecekan ini dulu.

## 1. Sumber informasi

Field `asset_notes` di `Task_Brief (2A)` (lihat `sunmedia-noza-task-brief-intake`). Ini biasanya diisi dari Post-Deal Onboarding Form klien (lihat `hermes-agent-workflow-v2.md` bagian 6: "ketersediaan aset mentah" adalah salah satu input onboarding yang dikalibrasi Noya). Noza TIDAK menghubungi klien langsung untuk menanyakan aset — itu di luar wewenangnya; semua info aset datang lewat Task_Brief.

## 2. Tiga status hasil

Untuk SETIAP ide naskah video (sebelum ditulis lengkap), tentukan satu dari tiga status:

| Status | Kapan dipakai |
|---|---|
| `ASSET_READY` | `asset_notes` secara eksplisit menyebut footage/foto yang cocok dengan ide naskah ini (mis. "video produk tersedia di Google Drive klien", "foto outlet lengkap 20 lokasi"). |
| `NEEDS_NEW_SHOOT` | `asset_notes` ADA isinya, tapi tidak mencakup apa yang dibutuhkan naskah ini (mis. aset yang ada hanya foto produk, padahal naskah butuh rekaman proses/testimoni pelanggan). |
| `NEEDS_ASSET_CONFIRMATION` | `asset_notes` kosong atau tidak disebutkan sama sekali di Task_Brief — Noza tidak tahu apakah aset ada atau tidak, dan TIDAK BOLEH mengasumsikan salah satu arah. |

## 3. Aturan inti — jangan dilanggar

- **Default-nya adalah `NEEDS_ASSET_CONFIRMATION`, bukan `ASSET_READY`.** Kalau ragu antara "mungkin ada" dan "tidak jelas", pilih `NEEDS_ASSET_CONFIRMATION`. Asumsi optimis di sini berisiko naskah selesai lengkap tapi tidak bisa diproduksi karena aset ternyata tidak ada (konsekuensinya ditanggung tim produksi/kreatif, bukan Noza).
- **Pilih ide naskah yang menyesuaikan aset yang TERSEDIA dulu**, baru isi sisa kuota 12 naskah dengan ide yang butuh syuting baru/konfirmasi — supaya paket konten tetap bisa mulai dieksekusi sebagian meski sebagian lain masih menunggu aset.
- **Satu status per naskah, dicatat eksplisit**, bukan disimpulkan dari isi naskah. Status ini nanti masuk ke bagian "Flag Aset" di `content-package-active.md` (lihat `sunmedia-noza-content-package-schema`).

## 4. Output dari skill ini

Satu tabel kerja (internal, sebelum ditulis jadi naskah lengkap):

```
| # | Ide naskah (judul kerja) | Status aset | Catatan |
|---|---|---|---|
| 1 | ... | ASSET_READY | footage tersedia: ... |
| 2 | ... | NEEDS_NEW_SHOOT | butuh rekaman proses produksi, aset existing cuma foto produk |
| 3 | ... | NEEDS_ASSET_CONFIRMATION | Task_Brief tidak menyebut aset sama sekali |
```

Tabel ini jadi dasar bagian "Flag Aset" di paket konten akhir — jangan ditulis ulang dari ingatan, salin persis dari sini.

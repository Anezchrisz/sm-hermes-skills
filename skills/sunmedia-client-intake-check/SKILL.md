---
name: sunmedia-client-intake-check
description: Validasi form intake klien Sun Media sebelum riset dimulai. Gunakan setiap kali ada klien/calon klien baru, form intake baru dikirim, atau PM memberi data bisnis klien lewat Telegram/Discord.
version: 1.0.0
metadata:
  hermes:
    tags: [sunmedia, intake, fase-1, nara]
    category: sunmedia
---

# Client Intake Check

## When to Use
Langkah pertama untuk setiap klien. Jalankan sebelum audit, bedah kompetitor, atau ICP.

## Form intake (4 blok)
1. **Bisnis**: produk/jasa, model bisnis, lokasi, URL website/IG/marketplace, USP yang diklaim klien.
2. **Budget**: range budget iklan/bulan (hanya dicatat; keputusan alokasi milik Noya).
3. **Kompetitor**: daftar kompetitor dari klien (boleh kosong).
4. **Target**: target pasar yang disasar, tujuan bisnis (leads/penjualan/awareness), mode (pitch atau full).

## Procedure
1. Cocokkan input PM dengan 4 blok di atas. Buat `client_id` (slug huruf kecil, tanpa spasi, mis. `bali-villa-co`).
2. Tandai tiap field: terisi / kosong / ambigu.
3. Kosong tapi tidak material -> catat sebagai **asumsi** (butuh validasi PM), lanjutkan.
4. Kosong atau ambigu dan **mengubah hasil secara material** (mis. tidak ada URL sama sekali, target pasar tidak jelas) -> kirim satu pesan ke PM berisi maksimal 3 pertanyaan spesifik, lalu tunggu.
5. Daftar kompetitor tidak lengkap (kurang dari 3) -> catat bahwa Nara yang akan mencari kandidat di langkah teardown.
6. Keluarkan ringkasan intake: client_id, mode, field terisi, daftar asumsi.

## Pitfalls
- Jangan menolak mulai hanya karena data kurang; asumsi ditandai lebih baik daripada berhenti.
- Jangan menyimpulkan budget atau channel dari form; itu bukan tugas Nara.

## Verification
- Semua 4 blok punya status. Setiap asumsi tercatat dengan kalimat "asumsi, perlu validasi PM".

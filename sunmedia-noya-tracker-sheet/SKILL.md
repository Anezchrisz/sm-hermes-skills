---
name: sunmedia-noya-tracker-sheet
description: Mencatat dan memperbarui status dokumen Master_Brief serta Performance_Review_Brief ke tab Noya_Tracker di Google Sheet tracker utama Sun Media. Selalu dipanggil setiap kali Noya menerbitkan atau memperbarui dokumen di Google Drive.
---

# Prosedur Pencatatan Google Sheet (`Noya_Tracker`)

## 1. Tujuan & Lokasi
Menjaga visibilitas *real-time* bagi PM, Orca, dan Yohanes tanpa harus membuka satu per satu folder klien di Google Drive.
- **Target Spreadsheet:** `SUNMEDIA_TRACKER_SHEET_ID` (dari `.env`)
- **Target Tab:** `Noya_Tracker` (terpisah dari tab milik Nara agar tidak terjadi konflik penulisan baris).

## 2. Struktur Kolom Baku Tab `Noya_Tracker`
Pastikan baris yang ditulis selalu mengikuti urutan kolom berikut:

| Kolom | Nama Field | Format / Aturan Isi |
|---|---|---|
| A | `timestamp_wita` | `YYYY-MM-DD HH:MM` (zona waktu WITA / Bali) |
| B | `client_slug` | Identifier unik klien (konsisten dengan nama folder Drive) |
| C | `doc_type` | `MASTER_BRIEF` / `WEEKLY_REVIEW` / `DELTA_SPRINT` |
| D | `version_or_week` | `v1.0`, `v1.1`, atau `2026-W47` |
| E | `gate_status` | `DRAFT_GATE_1` / `PROVISIONAL_PITCH` / `PENDING_CLIENT_RECONFIRMATION` / `READY_GATE_1_5` / `LOCKED_EXECUTION` / `LEVEL_1` / `LEVEL_2_SPRINT` / `LEVEL_3_PIVOT` |
| F | `max_deviation_pct` | Angka deviasi tertinggi (`0%` saat pitch awal; diisi `%` saat kalibrasi/review) |
| G | `drive_url` | Link langsung ke file aktif di Google Drive |
| H | `one_line_summary` | Ringkasan maksimal 1 kalimat (angka kunci + tindakan yang dibutuhkan dari PM/Orca) |

## 3. Aturan Pembaruan
1. **Jangan menimpa histori lama:** Setiap penerbitan baru atau perubahan status Gate ditambahkan sebagai **baris baru (append)** di bawah baris terakhir sehingga jejak audit waktu tercatat lengkap.
2. **Validasi Link:** Pastikan `drive_url` mengarah ke file yang benar (`master-brief-active.md` atau `weekly-review-YYYY-Www.md`) dan dapat diakses oleh akun tim Sun Media sebelum menulis ke Sheet.

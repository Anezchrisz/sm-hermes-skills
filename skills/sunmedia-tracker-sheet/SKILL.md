---
name: sunmedia-tracker-sheet
description: Catat output Nara (status brief, funnel audit, kompetitor, ICP, asumsi, learning log) ke Google Sheet tracker Sun Media memakai skill google-workspace. Gunakan setiap kali brief selesai atau status klien berubah.
version: 1.0.0
metadata:
  hermes:
    tags: [sunmedia, google-sheets, tracker, nara]
    category: sunmedia
    requires_tools: [terminal]
---

# Tracker Sheet (Google Sheets)

## When to Use
Setelah `sunmedia-client-intelligence-brief` selesai, saat PM mengubah status, atau saat learning log dicatat. Gunakan skill bawaan `google-workspace` (Sheets) untuk operasi baca/tulis; skill ini hanya mendefinisikan skema dan aturan.

## Sheet ID
Spreadsheet ID tracker disimpan di memory Nara (PM memberitahu: "Simpan Sheet ID tracker: ..."). Jika belum ada, minta PM sekali. Jangan membuat spreadsheet baru sendiri.

## Tab dan kolom (baris 1 = header, jangan diubah)
- `Brief_Tracker`: client_id, client, mode, brief_version, date, status, assumptions_count, brief_path, pm, notes
  - status: `draft` | `menunggu_review_PM` | `diserahkan_ke_Noya` | `revisi`
- `Funnel_Audit`: client_id, brief_version, stage, finding, urgency, evidence
- `Competitors`: client_id, brief_version, name, url, selection_reason, positioning, channels, pricing_offer, strengths, weaknesses, sources
- `ICP`: client_id, brief_version, summary, motivations, objections, channels, conversion_triggers, white_space, confidence
- `Assumptions`: client_id, brief_version, item, needs_validation_by, status, pm_answer
  - status: `open` | `validated` | `rejected`
- `Learning_Log`: date, client_id, industry, assumption, correction_or_actual, lesson, status
  - status: `auto_applied` | `needs_review`

## Procedure
1. Baca header tiap tab terlebih dahulu; jika berbeda dari skema, berhenti dan lapor PM.
2. **Append** baris baru (jangan menimpa). Satu baris per temuan/kompetitor/asumsi. Daftar dalam satu sel dipisah `; `.
3. Untuk `Brief_Tracker`: satu baris per `client_id` + `brief_version`. Update status di baris yang ada, bukan menambah baris baru.
4. Potong teks sel panjang (di bawah 2000 karakter); isi lengkap tetap di `brief.md`/`brief.json`.
5. Setelah menulis, baca ulang baris terakhir untuk verifikasi, lalu kirim tautan Sheet ke PM.

## Pitfalls
- Jangan menghapus baris atau mengubah header.
- Jangan menulis data klien ke spreadsheet selain tracker yang disepakati.
- Jika autentikasi Google gagal (NOT_AUTHENTICATED / REFRESH_FAILED), lapor ke PM; jangan mencoba jalur lain.

## Verification
- Baris terbaru terbaca kembali dan cocok dengan brief.json.

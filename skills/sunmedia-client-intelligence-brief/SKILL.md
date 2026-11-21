---
name: sunmedia-client-intelligence-brief
description: Rakit Client Intelligence Brief Sun Media (Markdown + JSON) dari hasil intake, funnel audit, teardown kompetitor, dan ICP, lalu serahkan utuh ke Noya. Gunakan di akhir riset Fase 1A.
version: 1.0.0
metadata:
  hermes:
    tags: [sunmedia, brief, fase-1, nara]
    category: sunmedia
---

# Client Intelligence Brief

## When to Use
Setelah intake check, funnel audit, competitor teardown, dan ICP selesai.

## Output: dua file per klien
Simpan di `/opt/data/shared/briefs/<client_id>/v<N>/` (buat folder jika belum ada):
- `brief.md` (dibaca manusia)
- `brief.json` (dibaca Noya dan Orca; JSON valid, tanpa komentar)

## Struktur brief.md
1. Ringkasan Eksekutif (3-5 insight actionable)
2. Funnel Audit (temuan + urgensi)
3. Competitive Landscape (tabel 3 kompetitor)
4. ICP Definitif
5. Peluang & Rekomendasi Arah (untuk Noya; bukan keputusan)
6. Asumsi yang perlu validasi PM
7. Daftar sumber

## Skema brief.json
```json
{
  "client": "", "client_id": "", "mode": "pitch | full",
  "brief_version": "", "date": "",
  "executive_summary": [""],
  "funnel_audit": [{"stage": "", "finding": "", "urgency": "quick_fix | structural", "evidence": ""}],
  "competitors": [{"name": "", "url": "", "selection_reason": "", "positioning": "",
    "channels": [{"channel": "", "intensity": ""}], "pricing_offer": "",
    "strengths": [""], "weaknesses": [""], "sources": [""]}],
  "icp": {"summary": "", "motivations": [""], "objections": [""], "channels": [""],
    "conversion_triggers": [""], "white_space": "", "confidence": ""},
  "opportunities": [""],
  "assumptions": [{"item": "", "needs_validation_by": "PM"}],
  "sources": [""]
}
```

## Procedure
1. Gabungkan hasil semua skill sebelumnya; jangan menambah klaim baru tanpa sumber.
2. Tulis `brief.md` dan `brief.json`. Validasi JSON (mis. `python -m json.tool`).
3. Jalankan checklist kualitas di bawah.
4. Panggil `sunmedia-tracker-sheet` untuk mencatat ke Google Sheet.
5. Kirim ke PM (channel tempat perintah datang): ringkasan eksekutif 3-5 poin, jumlah asumsi, tautan Sheet, dan lokasi file. Serahkan brief **utuh** ke Noya, bukan ringkasan.

## Checklist kualitas
- Semua klaim kompetitor punya sumber; asumsi ditandai.
- Tidak ada keputusan budget, channel, KPI, CAC/ROAS.
- Tidak ada angka tanpa sumber.
- JSON valid dan dapat dibaca Noya tanpa penyuntingan.
- Brief yang sudah locked oleh PM tidak diubah; revisi = versi baru (v2, v3).

## Pitfalls
- Jangan meringkas berlebihan saat serah ke Noya; detail dibutuhkan.
- Jangan menimpa versi lama; naikkan nomor versi.

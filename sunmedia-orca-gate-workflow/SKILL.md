---
name: sunmedia-orca-gate-workflow
description: Mengelola penomoran versi artefak (minor/mayor), jalur penolakan Gate (maksimal 2 putaran revisi lalu eskalasi Yohanes), dan routing Gate 2 async per agent. Dipanggil setiap kali PM memutuskan approve/reject di Gate 1, Gate 1.5, atau Gate 2.
---

# Gate & Versi — Orca

Skill ini administratif murni: Orca tidak menilai isi keputusan PM, hanya memproses dan
merutekan keputusan yang SUDAH dibuat PM.

## Penomoran versi

- **Revisi minor** (perbaikan kecil, tidak ganti status Gate): naikkan angka desimal —
  `v1.0` → `v1.1`.
- **Re-lock / perubahan status Gate** (`PROVISIONAL_PITCH` → `LOCKED_EXECUTION`, atau
  re-lock setelah Level 3 Major Pivot): naikkan angka mayor — `v1.x` → `v2.0`.
- Setiap artefak (`Master_Brief`, `Task_Brief`, SOP) membawa nomor versi dan tanggal di
  bagian "Riwayat Versi" isi dokumennya, bukan hanya di nama file archive.

## Jalur penolakan Gate (maksimal 2 putaran revisi)

1. PM menolak di Gate manapun → Orca mengembalikan dokumen ke agent pemiliknya beserta
   komentar PM, menaikkan counter revisi untuk dokumen itu (mulai dari 0).
2. Agent merevisi → dokumen diajukan ulang ke Gate yang sama.
3. Jika counter revisi mencapai 2 dan PM MASIH menolak → Orca eskalasi ke Yohanes,
   lampirkan riwayat lengkap (dokumen awal + komentar PM tiap putaran + revisi).
4. Reset counter ke 0 begitu dokumen itu disetujui PM, atau begitu Yohanes memberi
   keputusan final (lanjut dengan revisi tertentu, atau batalkan jalur ini).

## Gate 2 async per agent

- Gate 2 TIDAK menunggu keempat Task_Brief (Noza/Arka/Weba/Yuka) selesai bersamaan.
- Begitu PM menyetujui Task_Brief satu agent, Orca langsung mengubah status tracker
  agent itu jadi `DISETUJUI_GATE2` dan mengizinkan eksekusi dimulai untuk agent itu saja —
  TIDAK menunggu agent lain.
- Catat status per-agent secara independen di tracker, jangan membuat satu status
  gabungan "Gate 2 selesai" yang menunggu semua agent.

## Status deal yang relevan untuk routing (konteks dari `sunmedia-orca-deal-followup`)

- `STALLED` bisa dibuka kembali PM kapan saja menjadi `IN_NEGOTIATION` atau `WON` — bila
  ini terjadi setelah Gate 1 terlanjur kedaluwarsa (`PROVISIONAL_PITCH` berlaku 30 hari),
  beri tahu Noya untuk menyegarkan benchmark sebelum Gate 1.5, jangan langsung lanjut
  dengan angka lama.

## Yang TIDAK boleh dilakukan
- Mengunci status Gate sendiri — hanya mencatat keputusan PM/Yohanes yang sudah ada.
- Menilai apakah revisi agent "cukup baik" — itu wewenang PM di Gate, Orca hanya
  meneruskan dan menghitung counter.
- Membiarkan counter revisi berjalan tanpa batas — begitu mencapai 2, WAJIB eskalasi,
  tidak menunggu putaran ke-3 atau ke-4.

## Checklist
- [ ] Nomor versi naik sesuai jenis perubahan (minor vs mayor).
- [ ] Counter revisi per dokumen tercatat dan di-reset dengan benar.
- [ ] Eskalasi ke Yohanes terjadi tepat di putaran ke-3, dengan riwayat lengkap terlampir.
- [ ] Status Gate 2 per agent independen, tidak saling menunggu.

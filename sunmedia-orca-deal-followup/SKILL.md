---
name: sunmedia-orca-deal-followup
description: State machine follow-up status deal pasca pitch (H+2 tanya, H+5 ingat, H+10 tandai STALLED/LOST + minta alasan ke Loss Log). Dipicu scheduler relatif ke tanggal deck berstatus SENT, atau manual /followup_deal.
---

# Deal Follow-up — Orca

## Pemicu
Hitungan hari (H) dimulai dari tanggal tracker PD menandai deck berstatus `SENT` (lolos
Pitch Review). Orca membaca tanggal ini dari tab tracker, bukan dari tanggal hari ini
dikurangi perkiraan.

## Jadwal

| Hari | Aksi |
|---|---|
| H+2 | Tanya PM via Telegram/Discord: status deal — `WON` / `IN_NEGOTIATION` / `LOST`? |
| H+5 | Bila belum ada jawaban atau masih `IN_NEGOTIATION`, kirim pengingat. |
| H+10 | Bila masih belum ada keputusan final, tandai tracker `STALLED` (bila sebelumnya `IN_NEGOTIATION`) atau `LOST` (bila PM konfirmasi kalah) secara otomatis, DAN minta PM mengisi satu baris alasan ke Loss Log. |

## Transisi status

- `WON` → berhenti follow-up, lanjut ke Jalur B (Onboarding & Gate 1.5) — Orca tidak
  mengulang analisis, cukup catat transisi dan beri tahu Noya untuk mode kalibrasi.
- `LOST` → catat alasan di Loss Log (satu kalimat dari PM), tutup follow-up.
- `STALLED` → tutup siklus H+2/H+5/H+10 TAPI tetap bisa dibuka kembali PM kapan saja
  menjadi `IN_NEGOTIATION` atau `WON` — bila dibuka kembali, Orca mengecek apakah
  `PROVISIONAL_PITCH` masih berlaku (≤30 hari dari Gate 1); bila sudah lewat, beri tahu
  PM bahwa Noya perlu menyegarkan benchmark dulu sebelum lanjut ke Gate 1.5.

## Format Loss Log (satu baris per entri)

| timestamp | client_slug | status_akhir | alasan_satu_kalimat | hari_sejak_SENT |
|---|---|---|---|---|

## Yang TIDAK boleh dilakukan
- Menyimpulkan alasan kalah sendiri — selalu minta PM mengisi, jangan menebak dari
  konteks chat.
- Menandai `LOST` tanpa konfirmasi PM pada H+10 bila status terakhir masih
  `IN_NEGOTIATION` tanpa respons — dalam kasus ini tandai `STALLED`, bukan `LOST`
  (`LOST` hanya bila PM eksplisit konfirmasi kalah).
- Melanjutkan follow-up setelah `WON` — begitu `WON` tercatat, siklus H+2/5/10 ditutup
  permanen untuk klien itu.

## Checklist
- [ ] Tanggal H dihitung dari tanggal `SENT` di tracker, bukan perkiraan.
- [ ] Pengingat H+5 hanya terkirim bila H+2 belum memberi jawaban final.
- [ ] H+10 otomatis menulis status + meminta alasan, tidak dibiarkan menggantung.
- [ ] `STALLED` yang dibuka kembali dicek dulu usia `PROVISIONAL_PITCH`-nya.

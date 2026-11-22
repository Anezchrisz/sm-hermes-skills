---
name: sunmedia-orca-weekly-trigger
description: Memicu siklus /report_weekly [client_id] setiap Jumat sore/Senin pagi, mengisi tab Raw_Weekly_Data, mengingatkan bila data belum lengkap Senin siang, dan merutekan Task_Brief DELTA_SPRINT dari Noya ke Noza/Weba dengan SLA Gate 2 cepat.
---

# Weekly Trigger — Orca

Analisis isi data tetap wewenang Noya (`sunmedia-noya-weekly-performance-review`). Skill
ini HANYA memicu siklus, mengisi tab mentah, dan menjaga kelengkapan data — sesuai
larangan Orca menganalisis strategi.

## Jadwal pemicu
Jumat sore ATAU Senin pagi (WITA) — lihat `.env` `SCHEDULER_MODE` di setup guide untuk
mekanisme pemicu waktu (native Hermes vs cron eksternal).

## Langkah

1. Untuk setiap klien aktif (status `READY_TO_LAUNCH` atau sedang eksekusi), picu
   `/report_weekly [client_id]`.
2. Kumpulkan data mentah ke tab `Raw_Weekly_Data`:
   - Media Buyer: spend, reach/CTR, link clicks, CPL/CPA, ROAS.
   - Tim CS/AE: chat per `Ref_Code`, kualitas Cold/Warm/Hot, closing, nilai transaksi,
     alasan gagal terbanyak.
   - Kreatif & SEO: engagement konten teratas, visibilitas lokal/organik.
3. **Senin siang**, cek kelengkapan tab `Raw_Weekly_Data`. Bila ada kolom kosong/divisi
   belum mengisi → kirim pengingat ke divisi terkait, DAN beri tahu Noya dengan flag
   `INCOMPLETE_DATA` (Noya tetap menganalisis metrik yang tersedia dengan flag ini,
   Orca tidak menunda atau membatalkan analisis — hanya menandai).
4. Setelah Noya menghasilkan `Performance_Review_Brief` dengan klasifikasi Level 1/2/3:
   - **Level 1 (Minor):** tidak ada aksi Orca selain mencatat versi minor baru di tracker.
   - **Level 2 (Tactical Sprint / `DELTA_SPRINT`):** terbitkan `Task_Brief` mode
     `DELTA_SPRINT` ke Noza dan/atau Weba (sesuai arahan Noya), dengan flag SLA Gate 2
     cepat — rute lewat `sunmedia-orca-gate-workflow` tapi dengan prioritas tinggi.
   - **Level 3 (Major Pivot):** beri tahu PM/Yohanes bahwa klien ini wajib kembali ke
     Fase 1 (Nara & Noya mengulang analisis), DAN wajib mengulang Gate 1 + Gate 1.5 —
     Orca tidak memulai ulang Fase 1 sendiri, hanya memberi sinyal dan menghentikan
     siklus eksekusi Fase 2 untuk klien itu sampai ada Master_Brief baru.

## Yang TIDAK boleh dilakukan
- Menganalisis root cause deviasi sendiri — itu wewenang Noya, Orca hanya memicu dan
  mengisi data mentah.
- Melanjutkan analisis seolah data lengkap padahal ada kolom yang kosong tanpa flag
  `INCOMPLETE_DATA`.
- Menerbitkan `DELTA_SPRINT` tanpa arahan eksplisit dari hasil `Performance_Review_Brief`
  Noya.

## Checklist
- [ ] Siklus terpicu tepat waktu (Jumat sore/Senin pagi) untuk semua klien aktif.
- [ ] `INCOMPLETE_DATA` ditandai dengan benar bila ada kolom kosong Senin siang.
- [ ] `DELTA_SPRINT` hanya terbit mengikuti arahan Level 2 dari Noya, bukan inisiatif Orca.
- [ ] Level 3 memicu penghentian siklus eksekusi dan notifikasi kembali ke Fase 1.

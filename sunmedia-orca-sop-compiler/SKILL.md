---
name: sunmedia-orca-sop-compiler
description: Menyusun SOP 3 lapis (Context & Goal, Ready Assets, Tech & Attribution Checklist) dari Task_Brief yang sudah lolos Gate 2, untuk handoff ke tim eksekusi. Dipanggil /compile_sop setelah seluruh Gate 2 Fase 2 selesai untuk satu klien.
---

# SOP Compiler — Orca

## Prasyarat
Semua Task_Brief (Noza/Arka/Weba/Yuka) untuk klien terkait sudah `DISETUJUI_GATE2`
(lihat `sunmedia-orca-gate-workflow`). Bila ada yang belum, SOP tetap bisa disusun
sebagian — tandai bagian yang terkait agent belum-disetujui sebagai `PENDING_GATE2`,
jangan mengisi dengan data yang belum final.

## Struktur SOP 3 lapis

### Lapis 1 — Context & Goal
Ringkasan satu halaman: `core_value_proposition`, `primary_offer`, target KPI utama
(dari Master_Brief), dan segmen bahasa (`language_matrix`) — disalin dari field
wajib-identik yang sama dipakai di semua Task_Brief (lihat `sunmedia-orca-context-slicing`).

### Lapis 2 — Ready Assets
Checklist aset yang harus tersedia sebelum eksekusi dimulai, per divisi:
- Tim Kreatif: 12 video script/carousel/copy dari Noza — status `READY`/`BELUM`.
- Media Buyer: struktur campaign dan budget dari Arka — status `READY`/`BELUM`.
- Web Developer/Weba: landing page dan WA pre-filled dengan `Ref_Code` — status
  `READY`/`BELUM`.
- Spesialis SEO/GEO/GBP: paket audit dan optimasi dari Yuka — status `READY`/`BELUM`.

### Lapis 3 — Tech & Attribution Checklist
- [ ] Pixel/CAPI terpasang dan terverifikasi (test event masuk).
- [ ] Konvensi UTM konsisten dengan yang didefinisikan Arka di Task_Brief.
- [ ] `Ref_Code` per kanal/CS sudah aktif di WA pre-filled Weba.
- [ ] Kecepatan Custom Web (landing page) sudah dites, ambang yang disepakati terpenuhi.

## Output
Simpan sebagai dokumen SOP di folder klien (ikuti pola folder standar — ajukan nomor
subfolder baru lewat Change Request bila belum dialokasikan; jangan menebak nomor
sendiri). Sertakan versi dan tanggal.

## Pemicu `READY_TO_LAUNCH`
Lihat `sunmedia-orca-launch-guard-tracker` — SOP Lapis 3 yang tercentang penuh adalah
salah satu dari dua syarat `READY_TO_LAUNCH` (syarat lain: semua dependensi tracker
`READY`).

## Yang TIDAK boleh dilakukan
- Mengisi status "READY" pada Ready Assets tanpa konfirmasi nyata dari agent/divisi
  terkait — selalu verifikasi ke tracker, jangan mengasumsikan selesai.
- Menyusun SOP penuh untuk klien yang Task_Brief-nya belum ada sama sekali (beda dari
  "sebagian belum Gate 2" — ini kasus data belum ada).

## Checklist
- [ ] Lapis 1 memuat field wajib-identik yang sama dengan Task_Brief.
- [ ] Lapis 2 mencantumkan status nyata per divisi, bukan asumsi.
- [ ] Lapis 3 checklist teknis lengkap sebelum SOP ditandai final.
- [ ] Versi dan tanggal tercantum di dokumen.

---
name: sunmedia-orca-launch-guard-tracker
description: Baca/tulis Google Sheet Tracker per divisi, menerapkan Launch Guard (HOLD_WAITING_LP pada baris Arka sampai Weba & Web Developer READY; hold yang sama bila Pixel/CAPI belum terverifikasi), dan memicu READY_TO_LAUNCH. Dipanggil /update_tracker setiap kali ada perubahan status dari divisi manapun.
---

# Tracker & Launch Guard — Orca

Muat dulu `sunmedia-client-folder-standard` bagian 4 untuk kolom tracker baku — skill ini
hanya menambah kolom `dependency_status` khusus Orca, tidak mendefinisikan ulang kolom lain.

## Tab tracker per divisi
`Tracker_Tim_Kreatif`, `Tracker_Media_Buyer`, `Tracker_CS_AE`, `Tracker_Web_Developer`,
`Tracker_SEO_GEO_GBP` — satu tab per divisi agar tidak konflik tulis baris bersamaan.

## Logika Launch Guard

- Baris **Arka (Media Buyer)** berstatus `HOLD_WAITING_LP` sampai DUA kondisi terpenuhi:
  1. Baris Weba (landing page/WA funnel copy) `READY`.
  2. Baris Web Developer (build teknis landing page) `READY`.
- Guard yang SAMA (hold) berlaku bila **Pixel/CAPI belum terverifikasi**, terlepas dari
  status Weba/Web Developer — cek checklist lapis 3 SOP (`sunmedia-orca-sop-compiler`).
- **Noza (Tim Kreatif) dan Yuka (SEO/GEO/GBP) TIDAK tertahan** oleh guard ini — pekerjaan
  mereka tidak bergantung pada landing page jadi.
- Begitu semua kondisi di atas `READY`, ubah status Arka dari `HOLD_WAITING_LP` ke
  `READY` secara otomatis — jangan menunggu input manual PM untuk transisi ini (ini
  murni mekanis, bukan keputusan strategis).

## Pemicu `READY_TO_LAUNCH`

Status klien berubah jadi `READY_TO_LAUNCH` HANYA bila KEDUA syarat ini terpenuhi
bersamaan:
1. Semua baris tracker (kelima divisi) berstatus `READY` (tidak ada yang `HOLD_*` atau
   `BELUM`).
2. Checklist Lapis 3 Tech & Attribution di SOP (`sunmedia-orca-sop-compiler`) tercentang
   penuh.

Begitu `READY_TO_LAUNCH` tercapai, kirim notifikasi ke PM dan ke seluruh agent Fase 2
terkait — ini sinyal eksekusi boleh dimulai penuh.

## Update status "sedang dalam eksekusi"
Begitu output dari suatu divisi masuk (misal Noza menyerahkan 12 script), ubah status
baris itu otomatis jadi "sedang dalam eksekusi" di tracker — tidak perlu PM memicu
manual untuk transisi administratif ini.

## Yang TIDAK boleh dilakukan
- Melepas `HOLD_WAITING_LP` sebelum Weba DAN Web Developer benar-benar `READY` — jangan
  melepas hold berdasarkan perkiraan "sebentar lagi selesai".
- Menahan Noza atau Yuka dengan Launch Guard — guard ini HANYA untuk Arka terhadap
  Weba/Web Developer/Pixel-CAPI.
- Mengubah `READY_TO_LAUNCH` jadi tercapai padahal salah satu dari dua syarat belum
  terpenuhi.

## Checklist
- [ ] Kolom `dependency_status` terisi konsisten di tab Media Buyer.
- [ ] Transisi `HOLD_WAITING_LP` → `READY` otomatis begitu kedua kondisi terpenuhi.
- [ ] `READY_TO_LAUNCH` hanya terpicu bila tracker DAN checklist Lapis 3 lengkap.
- [ ] Notifikasi terkirim ke PM dan agent terkait saat `READY_TO_LAUNCH` tercapai.

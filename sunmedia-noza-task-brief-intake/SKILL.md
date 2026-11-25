---
name: sunmedia-noza-task-brief-intake
description: Validasi Task_Brief (2A) dari Orca sebelum mulai menulis naskah video, carousel, atau copy konten. Jalankan ini di awal SETIAP siklus kerja Noza, termasuk siklus DELTA_SPRINT.
---

# Intake & Validasi Task_Brief (2A)

Ini langkah PERTAMA di setiap siklus kerja Noza. Jangan mulai menulis naskah, carousel, atau copy apa pun sebelum Task_Brief lolos checklist ini.

## 1. Lokasi file

Baca `03_Task_Briefs_Orca/noza/task-brief-active.md` (dan `.json` bila ada) di folder klien — ditulis Orca, BUKAN Noza. Ikuti aturan folder/slug klien di skill `sunmedia-client-folder-standard`. Jangan menebak lokasi lain atau memakai versi di `archive/`.

## 2. Field wajib — cek satu per satu

| Field | Wajib ada? | Kalau kosong/ambigu |
|---|---|---|
| `core_value_proposition` | Ya | STOP. Tanpa ini, pesan konten bisa tidak sinkron dengan Arka/Weba (message-match). Laporkan ke PM lewat Orca, jangan menebak atau menulis versi sendiri. |
| `primary_offer` | Ya | STOP, sama seperti di atas. |
| `language_matrix` (daftar segmen: `Local_ID`, `Expat_Nomad`/`Tourist`, atau kombinasi) | Ya | STOP. Tanpa ini Noza tidak tahu berapa versi bahasa yang harus ditulis. |
| `channel_mix` (daftar platform target: TikTok, Instagram Reels, YouTube Shorts, dll.) | Ya | STOP. Tanpa ini Noza tidak tahu format/durasi/pacing yang dipakai. |
| Catatan aset klien (`asset_notes`: aset existing apa saja yang tersedia, atau pernyataan eksplisit "tidak ada data") | Tidak wajib ada isinya, TAPI field-nya harus ada | Kalau field ini SAMA SEKALI tidak disebut di Task_Brief (bukan sekadar kosong, tapi tidak ada sama sekali), tandai SEMUA naskah di siklus ini dengan `NEEDS_ASSET_CONFIRMATION` sejak awal — jangan tunggu sampai skill `sunmedia-noza-asset-stock-check` baru menyadarinya satu per satu. |
| `mode` (`NORMAL` atau `DELTA_SPRINT`) | Ya | Kalau tidak ada, asumsikan `NORMAL` DAN catat asumsi ini di ringkasan paket — jangan diam-diam. |
| `delta_sprint_focus` (hanya ada bila `mode = DELTA_SPRINT`, mis. "4-6 hook baru, fokus creative fatigue produk X") | Wajib ada bila `mode = DELTA_SPRINT` | STOP bila `mode = DELTA_SPRINT` tapi field ini kosong — tanpa fokus yang jelas, Noza tidak tahu akar masalah mana yang harus diperbaiki, risikonya menulis ulang 12 naskah penuh padahal yang diminta cuma beberapa hook (lihat `hermes-agent-workflow-v2.md` bagian 9, Level 2 Tactical Sprint). |

## 3. Larangan saat intake

- **Jangan mengarang nilai field yang hilang.** Kalau `core_value_proposition` kosong, JANGAN menulis proposisi versimu sendiri dari hasil tebakan membaca nama klien.
- **Jangan melanjutkan ke penulisan naskah kalau salah satu field "STOP" di atas gagal.** Laporkan ke PM/Orca lewat channel yang biasa dipakai, sebutkan field mana yang hilang, lalu berhenti sampai Task_Brief direvisi.
- **Jangan mengubah isi Task_Brief sendiri** — itu file milik Orca (lihat `sunmedia-client-folder-standard` bagian 2: "Agent HANYA menulis ke folder nomornya sendiri").

## 4. Checklist sebelum lanjut ke skill berikutnya

- [ ] `core_value_proposition`, `primary_offer`, `language_matrix`, `channel_mix` semua ada dan jelas
- [ ] `mode` diketahui (`NORMAL`/`DELTA_SPRINT`); bila `DELTA_SPRINT`, `delta_sprint_focus` jelas
- [ ] Status aset diketahui: ada datanya, ATAU sudah ditandai `NEEDS_ASSET_CONFIRMATION` di seluruh paket
- [ ] Ringkasan satu paragraf ditulis di awal kerja: field apa yang diasumsikan (kalau ada), field apa yang ditandai perlu konfirmasi

Setelah checklist ini lolos, lanjut ke `sunmedia-noza-asset-stock-check` (mode `NORMAL`) atau langsung ke `sunmedia-noza-video-script-brief` dengan fokus sempit (mode `DELTA_SPRINT`).

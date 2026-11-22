---
name: sunmedia-client-folder-standard
description: Standar tunggal struktur folder Google Drive, penamaan slug klien, dan pola file-aktif/archive untuk semua output agent Sun Media. Dimuat oleh Nara, Noya, Orca, dan setiap agent baru yang menyimpan file ke folder klien.
---

# Standar Folder Klien Sun Media

Skill ini TIDAK berisi prosedur riset atau kalkulasi — itu ada di skill masing-masing agent. Skill ini hanya menjawab satu pertanyaan: **di mana file disimpan dan dengan nama apa**, supaya semua agent konsisten tanpa saling menyalin aturan.

## 1. Root dan slug klien

Root: `SunMedia_Clients/` di Google Drive (akun kerja Sun Media).

Setiap klien punya satu folder dengan nama persis:

```
<YYYY-MM>_<ClientSlug>/
```

- `YYYY-MM` = bulan dan tahun saat klien PERTAMA KALI masuk pipeline (bulan Nara menerima intake pertama). Tidak berubah walau prosesnya lanjut ke bulan berikutnya.
- `ClientSlug` = nama bisnis dari field "Bisnis" di Client Intake Form, diubah ke PascalCase tanpa spasi dan tanpa karakter khusus. Contoh: "Kopi Kenangan Denpasar" → `KopiKenanganDenpasar`.
- Agent manapun yang membuat slug WAJIB mengecek dulu apakah folder dengan `ClientSlug` itu (abaikan prefix `YYYY-MM`) sudah ada di `SunMedia_Clients/` sebelum membuat baru — supaya klien yang sama tidak punya dua folder karena beda bulan pencatatan.
- Folder klien dibuat oleh agent PERTAMA yang menulis output untuk klien itu. Untuk klien baru, itu selalu Nara. Agent berikutnya (Noya, Orca, dst.) hanya menambah subfolder bernomor di dalamnya, tidak membuat folder klien baru. Orca secara khusus WAJIB berhenti dan lapor PM bila folder klien belum ada — ia tidak boleh membuatnya sendiri.

## 2. Subfolder bernomor di dalam folder klien

```
<YYYY-MM>_<ClientSlug>/
├── 01_Research_Nara/
├── 02_Master_Brief_Noya/
├── 03_Task_Briefs_Orca/        ← AKTIF — dipasang oleh Orca
├── 04_Weekly_Reviews_Noya/
└── 05_Deck_PD/                 ← reserved, belum aktif sampai PD dibangun
```

Nomor sudah dialokasikan tetap per agent/fase, bukan urutan pembuatan. Saat agent baru dibangun (PD, Noza, Arka, Weba, Yuka), tambahkan baris baru di tabel ini dulu — lewat Change Request skill seperti aturan governance di `hermes-agent-workflow-v2.md` bagian 10 — baru pasang skill agent itu. Jangan biarkan agent baru menebak nomor sendiri.

| No. | Folder | Pemilik tulis | Pembaca |
|---|---|---|---|
| 01 | `Research_Nara` | Nara | Noya, Orca |
| 02 | `Master_Brief_Noya` | Noya | Orca, PD, dan semua agent Fase 2 |
| 03 | `Task_Briefs_Orca` | Orca | Noza, Arka, Weba, Yuka |
| 04 | `Weekly_Reviews_Noya` | Noya | PM, Yohanes |
| 05 | `Deck_PD` | PD | PM, Yohanes, klien |

Agent HANYA menulis ke folder nomornya sendiri. Membaca folder agent lain boleh; menulis atau mengubah isi folder agent lain tidak boleh, bahkan untuk memperbaiki typo — minta agent pemilik yang mengulang. Ini berlaku ketat untuk Orca: ia membaca `01_Research_Nara/` dan `02_Master_Brief_Noya/` untuk keperluan context slicing dan QA, tapi hanya menulis ke `03_Task_Briefs_Orca/`.

## 3. Pola file aktif + archive (wajib sama di semua folder output)

Di setiap subfolder bernomor:

```
02_Master_Brief_Noya/
├── master-brief-active.md      ← SATU-SATUNYA file yang dibaca agent lain
├── master-brief-active.json    ← kalau skill agent itu juga menghasilkan JSON
└── archive/
    ├── master-brief-provisional-v1.0.md
    └── master-brief-locked-v1.0.md
```

Aturan:
1. File yang dibaca downstream agent SELALU bernama `<jenis>-active.md` (dan `.json` bila ada) — tanpa nomor versi di nama file. Ini yang membuat Noya, Orca, dst. tidak perlu mencari "versi tertinggi"; tinggal baca satu nama file yang pasti. Untuk Orca: setiap Task_Brief per agent disimpan sebagai `03_Task_Briefs_Orca/<agent-slug>/task-brief-active.md` (dan `.json`), subfolder per agent (`noza/`, `arka/`, `weba/`, `yuka/`) di dalam `03_Task_Briefs_Orca/`.
2. Sebelum menimpa `*-active.md` dengan versi baru, pindahkan isi versi SEBELUMNYA ke `archive/` dengan nama bertanggal/berversi (`<jenis>-v{N}.md`, atau tag status seperti `master-brief-provisional-v1.0.md` untuk Noya).
3. Riwayat revisi tetap ditulis di dalam isi file aktif (bagian "Riwayat Revisi"/"Riwayat Versi"), bukan hanya disimpulkan dari nama file di archive.
4. Tracker Sheet mencatat STATUS dan LINK ke file aktif, bukan ke file archive.

## 4. Kolom tracker standar (acuan, boleh beda tab per agent)

Setiap agent punya tab tracker sendiri (Nara: `Brief_Tracker`, Noya: `Noya_Tracker`, Orca: satu tab per divisi — `Tracker_Tim_Kreatif`, `Tracker_Media_Buyer`, `Tracker_CS_AE`, `Tracker_Web_Developer`, `Tracker_SEO_GEO_GBP`) agar tidak konflik penulisan baris bersamaan, tapi kolomnya mengikuti bentuk yang sama supaya PM membaca satu format:

| Kolom | Isi |
|---|---|
| timestamp | Waktu WITA |
| client_slug | `<YYYY-MM>_<ClientSlug>` persis sama dengan nama folder |
| doc_type | nama subfolder bernomor, misal `01_Research_Nara` |
| version_or_week | versi (`v2`) atau minggu (`2026-W47`) |
| status | sesuai status khusus agent itu (`menunggu_review_PM`, `disetujui`, `PENDING_CLIENT_RECONFIRMATION`, `HOLD_WAITING_LP`, dst.) |
| ringkasan | satu kalimat |
| drive_url | link ke file `*-active.*`, bukan ke archive |

Tab tracker milik Orca menambah satu kolom khusus: `dependency_status` (nilai: `READY`, `HOLD_WAITING_LP`, `HOLD_PIXEL_CAPI`, `READY_TO_LAUNCH`) untuk mendukung logika Launch Guard — kolom ini tidak dipakai tab Nara/Noya.

## 5. Checklist sebelum agent manapun menyimpan file

- [ ] Folder klien `<YYYY-MM>_<ClientSlug>/` sudah ada? Kalau belum dan saya bukan Nara, STOP — jangan buat folder klien, laporkan ke PM (kemungkinan intake belum lewat Nara).
- [ ] Subfolder bernomor milik saya sudah ada di dalamnya? Kalau belum, buat sesuai tabel di atas, jangan pakai nomor lain.
- [ ] File lama di `*-active.*` sudah dipindah ke `archive/` dengan nama versi sebelum saya tulis file aktif baru?
- [ ] Baris tracker tab saya sudah diisi dengan `client_slug` yang PERSIS sama dengan nama folder (termasuk prefix `YYYY-MM`)?
- [ ] Link yang saya kirim ke PM mengarah ke `*-active.*`, bukan ke archive?

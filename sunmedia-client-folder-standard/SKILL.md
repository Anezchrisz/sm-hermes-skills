---
name: sunmedia-client-folder-standard
description: Standar tunggal struktur folder Google Drive, penamaan slug klien, pola file-aktif/archive, dan tracker pusat + tracker lokal per klien untuk semua output agent Sun Media. Dimuat oleh Nara, Noya, Orca, Noza, dan setiap agent baru yang menyimpan file ke folder klien.
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
- Nara juga yang membuat **Tracker lokal per klien** (bagian 2.5) di saat yang sama dengan membuat folder klien — satu paket, satu langkah.

## 2. Subfolder bernomor di dalam folder klien

```
<YYYY-MM>_<ClientSlug>/
├── Tracker_<ClientSlug>         ← Google Sheet, dibuat Nara — lihat bagian 2.5
├── 01_Research_Nara/
├── 02_Master_Brief_Noya/
├── 03_Task_Briefs_Orca/        ← AKTIF — dipasang oleh Orca
├── 04_Weekly_Reviews_Noya/
├── 05_Deck_PD/                 ← reserved, belum aktif sampai PD dibangun
└── 06_Content_Noza/            ← AKTIF — dipasang oleh Noza
```

`Tracker_<ClientSlug>` sengaja di ROOT folder klien, bukan subfolder bernomor — ia bukan output satu agent, tapi ringkasan lintas-agent.

Nomor sudah dialokasikan tetap per agent/fase, bukan urutan pembuatan. Saat agent baru dibangun (Arka, Weba, Yuka setelah Noza), tambahkan baris baru di tabel ini dulu lewat Change Request — jangan biarkan agent baru menebak nomor sendiri.

| No. | Folder | Pemilik tulis | Pembaca |
|---|---|---|---|
| — | `Tracker_<ClientSlug>` (Sheet, root) | Semua agent (append baris masing-masing) | PM, Yohanes |
| 01 | `Research_Nara` | Nara | Noya, Orca |
| 02 | `Master_Brief_Noya` | Noya | Orca, PD, dan semua agent Fase 2 |
| 03 | `Task_Briefs_Orca` | Orca | Noza, Arka, Weba, Yuka |
| 04 | `Weekly_Reviews_Noya` | Noya | PM, Yohanes |
| 05 | `Deck_PD` | PD | PM, Yohanes, klien |
| 06 | `Content_Noza` | Noza | PM, Orca (QA), Arka (rujukan kreatif) |

Agent HANYA menulis ke folder nomornya sendiri. Membaca folder agent lain boleh; menulis atau mengubah isi folder agent lain tidak boleh, bahkan untuk memperbaiki typo — minta agent pemilik yang mengulang. **Pengecualian satu-satunya** adalah `Tracker_<ClientSlug>`: semua agent boleh menambah baris (append) di situ, tapi HANYA baris miliknya sendiri — tidak pernah mengubah/menghapus baris agent lain.

## 2.5 Tracker lokal per klien (salinan status, terhubung ke tracker pusat)

**Masalah yang diselesaikan:** tracker pusat (`SunMedia - Fase 1 Intelligence Tracker`, satu spreadsheet dengan banyak tab — `Brief_Tracker`, `Noya_Tracker`, `Tracker_Tim_Kreatif`, dst.) berisi baris SEMUA klien bercampur. PM yang mau cek satu klien harus mencari/filter di banyak tab. Output konten/brief sendiri TIDAK pernah ditumpuk di tracker — itu sudah berlaku sejak desain awal (tracker cuma pegang STATUS dan `drive_url`, isi lengkap selalu file terpisah di folder bernomor). Yang baru di sini murni soal kemudahan cek status per klien.

**Desain: salinan, bukan sumber kebenaran baru.**
- Tracker pusat tetap SATU-SATUNYA tempat agent *membaca* status lintas-klien (mis. Orca baca status Weba untuk Launch Guard). Tidak berubah.
- Setiap klien punya satu file tambahan: `Tracker_<ClientSlug>` (Google Sheet, satu tab bernama `Status`), disimpan di root folder klien. Nara membuatnya sekali, dari template kosong (kolom sama seperti bagian 4 + satu kolom `agent`), bersamaan dengan membuat folder klien baru.
- **Setiap kali** agent manapun menulis satu baris ke tab miliknya di tracker pusat, ia WAJIB menulis baris yang SAMA PERSIS (kolom sama + kolom `agent` diisi nama agent) ke tab `Status` di `Tracker_<ClientSlug>` milik klien itu — dua langkah tulis berurutan, bukan sinkronisasi dua arah, bukan formula `IMPORTRANGE`. Alasan memilih double-write dibanding `IMPORTRANGE`: `IMPORTRANGE` antar-spreadsheet perlu klik "Izinkan akses" manual satu kali oleh manusia untuk SETIAP spreadsheet baru (setiap klien baru akan trigger ini lagi) — rapuh untuk sistem yang jalan tanpa diawasi. Double-write tidak butuh persetujuan manual apa pun, cukup satu panggilan tulis tambahan ke skill `google-workspace` yang sudah dipakai tiap agent.
- Kalau tulisan ke `Tracker_<ClientSlug>` gagal (mis. Sheet API error) TAPI tulisan ke tracker pusat berhasil: agent tetap lanjut (tracker pusat yang jadi acuan), catat satu baris warning di log. JANGAN retry berulang-ulang atau blok pekerjaan karena ini.

**Isi tab `Status` di `Tracker_<ClientSlug>`:**

| Kolom | Isi |
|---|---|
| timestamp | Waktu WITA — sama dengan yang ditulis ke tracker pusat |
| agent | `Nara` / `Noya` / `Orca` / `Noza` / dst. — kolom BARU, tidak ada di tab tracker pusat karena di sana tiap agent sudah punya tab sendiri |
| doc_type | sama seperti bagian 4 |
| version_or_week | sama seperti bagian 4 |
| status | sama seperti bagian 4 |
| ringkasan | sama seperti bagian 4 |
| drive_url | sama seperti bagian 4 |

`client_slug` TIDAK perlu jadi kolom di sini (tiap baris di Sheet ini sudah pasti satu klien yang sama — nama Sheet-nya sendiri).

**Siapa menulis apa:**
- Nara: baris `01_Research_Nara` (dari `Brief_Tracker`)
- Noya: baris `02_Master_Brief_Noya` dan `04_Weekly_Reviews_Noya` (dari `Noya_Tracker`)
- Orca: baris `03_Task_Briefs_Orca` per sub-agent, dan baris-baris tracker divisi Fase 3 (dari `Tracker_Tim_Kreatif`, `Tracker_Media_Buyer`, `Tracker_CS_AE`, `Tracker_Web_Developer`, `Tracker_SEO_GEO_GBP`)
- Noza: baris `06_Content_Noza` (dari baris miliknya di `Tracker_Tim_Kreatif`)
- PD, Arka, Weba, Yuka: pola sama begitu masing-masing dibangun

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
1. File yang dibaca downstream agent SELALU bernama `<jenis>-active.md` (dan `.json` bila ada) — tanpa nomor versi di nama file. Untuk Orca: setiap Task_Brief per agent disimpan sebagai `03_Task_Briefs_Orca/<agent-slug>/task-brief-active.md` (dan `.json`), subfolder per agent (`noza/`, `arka/`, `weba/`, `yuka/`). Untuk Noza: paket konten disimpan sebagai `06_Content_Noza/content-package-active.md` (dan `.json`).
2. Sebelum menimpa `*-active.md` dengan versi baru, pindahkan isi versi SEBELUMNYA ke `archive/` dengan nama bertanggal/berversi (`<jenis>-v{N}.md`, atau tag status seperti `master-brief-provisional-v1.0.md` untuk Noya).
3. Riwayat revisi tetap ditulis di dalam isi file aktif (bagian "Riwayat Revisi"/"Riwayat Versi"), bukan hanya disimpulkan dari nama file di archive.
4. Tracker Sheet (pusat maupun lokal per klien) mencatat STATUS dan LINK ke file aktif, bukan ke file archive, dan tidak pernah isi lengkap dokumennya.

## 4. Kolom tracker standar (acuan, boleh beda tab per agent)

Setiap agent punya tab tracker sendiri di tracker PUSAT (Nara: `Brief_Tracker`, Noya: `Noya_Tracker`, Orca: satu tab per divisi — `Tracker_Tim_Kreatif`, `Tracker_Media_Buyer`, `Tracker_CS_AE`, `Tracker_Web_Developer`, `Tracker_SEO_GEO_GBP`; Noza menulis ke `Tracker_Tim_Kreatif` yang sama dengan Orca pakai) agar tidak konflik penulisan baris bersamaan, tapi kolomnya mengikuti bentuk yang sama supaya PM membaca satu format:

| Kolom | Isi |
|---|---|
| timestamp | Waktu WITA |
| client_slug | `<YYYY-MM>_<ClientSlug>` persis sama dengan nama folder |
| doc_type | nama subfolder bernomor, misal `01_Research_Nara` |
| version_or_week | versi (`v2`) atau minggu (`2026-W47`) |
| status | sesuai status khusus agent itu (`menunggu_review_PM`, `disetujui`, `PENDING_CLIENT_RECONFIRMATION`, `HOLD_WAITING_LP`, dst.) |
| ringkasan | satu kalimat |
| drive_url | link ke file `*-active.*`, bukan ke archive |

Tab tracker milik Orca menambah satu kolom khusus: `dependency_status` (nilai: `READY`, `HOLD_WAITING_LP`, `HOLD_PIXEL_CAPI`, `READY_TO_LAUNCH`) untuk mendukung logika Launch Guard — kolom ini tidak dipakai tab Nara/Noya. Noza menulis baris statusnya sendiri ke `Tracker_Tim_Kreatif`; kolom `dependency_status` untuk baris Noza dibiarkan `READY` begitu paket konten selesai (Noza sendiri tidak pernah `HOLD`).

Setiap baris di atas, SETELAH ditulis ke tab pusat, disalin juga ke `Tracker_<ClientSlug>` milik klien terkait (lihat bagian 2.5) — kolom `client_slug` dan `dependency_status` boleh ditinggalkan di salinan lokal.

## 5. Checklist sebelum agent manapun menyimpan file

- [ ] Folder klien `<YYYY-MM>_<ClientSlug>/` sudah ada? Kalau belum dan saya bukan Nara, STOP — jangan buat folder klien, laporkan ke PM.
- [ ] Subfolder bernomor milik saya sudah ada di dalamnya? Kalau belum, buat sesuai tabel di atas, jangan pakai nomor lain.
- [ ] File lama di `*-active.*` sudah dipindah ke `archive/` dengan nama versi sebelum saya tulis file aktif baru?
- [ ] Baris tracker tab saya di tracker PUSAT sudah diisi dengan `client_slug` yang PERSIS sama dengan nama folder (termasuk prefix `YYYY-MM`)?
- [ ] Baris yang sama sudah disalin ke `Tracker_<ClientSlug>` (tab `Status`, kolom `agent` diisi) di root folder klien? Kalau gagal, saya tetap lanjut, tapi catat warning.
- [ ] Link yang saya kirim ke PM mengarah ke `*-active.*`, bukan ke archive?

---
name: sunmedia-noza-content-package-schema
description: Cara menggabungkan naskah video, carousel, dan copy konten jadi satu paket output, dan cara menyimpan/melaporkan statusnya. Jalankan ini TERAKHIR di setiap siklus, setelah ketiga skill konten selesai. Memuat skill sunmedia-client-folder-standard untuk aturan folder dan tracker.
---

# Skema Paket Output & Penyimpanan

Skill ini TIDAK menulis konten kreatif (itu tugas `sunmedia-noza-video-script-brief`, `sunmedia-noza-carousel-content`, `sunmedia-noza-copy-konten`) — skill ini hanya menjawab: bagaimana ketiganya digabung, disimpan, dan dilaporkan statusnya.

Memuat skill `sunmedia-client-folder-standard` untuk aturan folder, slug klien, dan pola file aktif/archive — jangan definisikan ulang di sini.

## 1. Lokasi dan nama file

Ikuti `sunmedia-client-folder-standard`:
```
<YYYY-MM>_<ClientSlug>/
└── 06_Content_Noza/
    ├── content-package-active.md
    ├── content-package-active.json
    └── archive/
        └── content-package-v{N}.md
```
Sebelum menulis `content-package-active.md` baru, pindahkan versi SEBELUMNYA ke `archive/content-package-v{N}.md` terlebih dahulu (N = nomor versi sebelumnya).

## 2. Struktur Markdown baku (`content-package-active.md`)

1. **Ringkasan Paket** — jumlah naskah video, jumlah set carousel, jumlah item copy, daftar singkat flag `NEEDS_ASSET_CONFIRMATION`/`NEEDS_NEW_SHOOT` (jumlah + judul kerja yang terdampak).
2. **Naskah Video** — satu entri per naskah (lihat field wajib di `sunmedia-noza-video-script-brief`).
3. **Carousel** — satu entri per set (lihat field wajib di `sunmedia-noza-carousel-content`).
4. **Copy Konten Pendukung** — satu entri per item (lihat field wajib di `sunmedia-noza-copy-konten`).
5. **Flag Aset** — tabel lengkap dari `sunmedia-noza-asset-stock-check`, termasuk status final tiap naskah/slide.
6. **Riwayat Versi** — tanggal, nomor versi, ringkasan perubahan dari versi sebelumnya (kosong/"v1, paket awal" untuk versi pertama).

## 3. Skema JSON (`content-package-active.json`)

```json
{
  "client_slug": "",
  "package_version": "",
  "date": "",
  "mode": "NORMAL | DELTA_SPRINT",
  "summary": {
    "video_count": 0,
    "carousel_set_count": 0,
    "copy_item_count": 0,
    "needs_asset_confirmation_count": 0,
    "needs_new_shoot_count": 0
  },
  "video_scripts": [
    {
      "id": "",
      "working_title": "",
      "platform": "",
      "duration_seconds": "",
      "language_segment": "Local_ID | Expat_Nomad",
      "asset_status": "ASSET_READY | NEEDS_NEW_SHOOT | NEEDS_ASSET_CONFIRMATION",
      "hook_chosen": "",
      "hook_alternatives": [""],
      "hook_reason": "",
      "body_beats": [""],
      "visual_audio_direction": [""],
      "cta": ""
    }
  ],
  "carousel": [
    {
      "id": "",
      "language_segment": "",
      "slides": [
        {"slide_no": 1, "copy": "", "visual_direction": "", "asset_status": ""}
      ]
    }
  ],
  "copy": [
    {
      "linked_item_id": "",
      "platform": "",
      "caption": "",
      "cta_copy": "",
      "hashtags": [""]
    }
  ],
  "asset_flags": [
    {"item_id": "", "status": "", "note": ""}
  ]
}
```

Field `client_slug` harus PERSIS sama dengan nama folder klien (lihat `sunmedia-client-folder-standard` bagian 1).

## 4. Melaporkan status (tracker pusat + tracker lokal per klien)

Setelah `content-package-active.{md,json}` tersimpan:

1. Tulis satu baris ke tab `Tracker_Tim_Kreatif` di tracker pusat (`SunMedia - Fase 1 Intelligence Tracker`) — kolom standar: `timestamp`, `client_slug`, `doc_type` (`06_Content_Noza`), `version_or_week`, `status`, `ringkasan` (satu kalimat), `drive_url` (link ke `content-package-active.md`, BUKAN ke archive). Isi `dependency_status` = `READY` (Noza tidak pernah `HOLD`, lihat `hermes-agent-workflow-v2.md` bagian 8).
2. **Salin baris yang SAMA** ke tab `Status` di Sheet `Tracker_<ClientSlug>` (root folder klien) — kolom sama, ditambah kolom `agent` = `Noza`. Ini DUA tulis Sheet terpisah, bukan satu — lihat `sunmedia-client-folder-standard` bagian 2.5 untuk desain lengkap.
3. Jangan pernah menyalin ISI paket konten (naskah/carousel/copy) ke Sheet manapun — Sheet hanya pegang status + link, isi lengkap selalu di `content-package-active.md/.json`.
4. Kalau langkah 2 gagal (error Sheet API) tapi langkah 1 berhasil: tetap lanjut, catat satu baris warning di log kerja. Jangan retry berulang atau memblokir pekerjaan karena ini.

## 5. Checklist akhir sebelum paket dianggap selesai

- [ ] Versi sebelumnya (bila ada) sudah dipindah ke `archive/` sebelum file aktif baru ditulis
- [ ] `content-package-active.md` punya 6 bagian lengkap
- [ ] `content-package-active.json` valid dan field `client_slug` cocok nama folder
- [ ] Baris tracker ditulis ke `Tracker_Tim_Kreatif` (pusat) DAN disalin ke `Tracker_<ClientSlug>` (lokal)
- [ ] `drive_url` di kedua tempat mengarah ke `content-package-active.md`, bukan archive

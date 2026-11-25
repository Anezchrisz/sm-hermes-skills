---
name: sunmedia-noza-learning-log
description: Mencatat hook/format yang terbukti performa tinggi atau rendah per industri dan segmen, ke Knowledge_Base, setelah hasil Fase 4 (weekly performance review) tersedia. Format sama dengan sunmedia-nara-learning-log dan sunmedia-noya-learning-log.
---

# Learning Log Noza

Dijalankan SETELAH data Fase 4 (`Performance_Review_Brief` milik Noya, lihat `hermes-agent-workflow-v2.md` bagian 9) tersedia untuk klien terkait — bukan setiap kali paket konten selesai, karena performa konten baru diketahui setelah berjalan di lapangan.

## 1. Kapan mencatat

- Setelah siklus mingguan Fase 4 menunjukkan data "engagement konten teratas" untuk klien tersebut.
- Setelah `DELTA_SPRINT` (Level 2) selesai dan hasilnya terlihat di siklus berikutnya — apakah hook baru memperbaiki creative fatigue atau tidak.

## 2. Field entri (maksimal 150 kata per entri, konsisten dengan pola Noya)

```
- date: 
  client_industry: 
  market_segment: Local_ID | Expat_Nomad
  platform: 
  hook_type: (arketipe hook, mis. "pertanyaan provokatif", "before-after")
  result: tinggi | rendah (ringkas, bukan angka mentah — angka performa tetap milik Noya/Performance_Review_Brief)
  note: (maks. ~100 kata, pola apa yang terlihat, bukan rekap angka)
```

## 3. Aturan sanitasi (sama dengan Noya/Nara)

- **Tidak menyimpan kredensial** (token, API key, password) dalam bentuk apa pun.
- **Tidak menyimpan data pribadi pelanggan akhir klien** (nama, nomor WA, isi chat pelanggan) — itu wilayah tracker CS/AE, bukan learning log kreatif.
- Angka performa mentah (reach, engagement rate, dsb.) TIDAK disalin ke sini — itu tetap di `Performance_Review_Brief` Noya. Learning log Noza mencatat POLA (hook/format apa yang berhasil/gagal), bukan angka itu sendiri.

## 4. Dua pola yang WAJIB dipantau

1. **Hook/format per industri dan segmen** — mana yang konsisten tinggi/rendah retensi, dipakai sebagai referensi (bukan aturan kaku) saat menulis `sunmedia-noza-video-script-brief` untuk klien baru di industri serupa.
2. **Pola `NEEDS_NEW_SHOOT` berulang per klien** — kalau satu klien berulang kali butuh syuting baru tiap siklus, ini indikasi klien perlu jadwal syuting rutin terjadwal, bukan ad-hoc per Task_Brief. Catat sebagai entri terpisah, tag `client_slug`, bukan tag industri.
3. (Tambahan) **Platform yang paling sering memicu revisi PM di Gate 2** — kalau satu platform konsisten direvisi, ini sinyal untuk ditinjau polanya (format/pacing/tone), bukan untuk diam-diam diubah sendiri oleh Noza.

## 5. Retrieval sebelum kerja baru

Sebelum mulai siklus baru untuk klien/industri tertentu, baca 5 entri terbaru yang cocok dengan tag industri/segmen klien ini (pola sama dengan Nara/Noya, lihat `hermes-agent-workflow-v2.md` bagian 10 butir 2) — gunakan sebagai referensi arah hook, BUKAN sebagai kepastian bahwa pola lama akan berulang.

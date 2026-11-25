---
name: sunmedia-noza-video-script-brief
description: Struktur baku satu naskah video pendek (hook, body, arahan visual/audio, CTA). Dipakai untuk menulis 12 naskah per siklus NORMAL, atau beberapa hook tambahan per siklus DELTA_SPRINT.
---

# Struktur Naskah Video Pendek

Satu naskah = satu unit lengkap siap diserahkan ke tim produksi. Jangan menyerahkan naskah yang hanya berisi ide kasar.

## 1. Field wajib per naskah

1. **Judul kerja** — nama singkat internal untuk mengenali naskah (bukan judul video final).
2. **Platform target** — salah satu dari `channel_mix` di Task_Brief (TikTok / Instagram Reels / YouTube Shorts / dll). Satu naskah bisa ditandai cocok untuk lebih dari satu platform HANYA bila pacing dan durasinya memang sama; kalau beda, buat versi terpisah.
3. **Durasi target** — dalam detik, disesuaikan kebiasaan platform (umumnya 15–60 detik untuk short-form; sebutkan rentang, bukan angka tunggal yang kaku).
4. **Segmen bahasa** — `Local_ID` atau `Expat_Nomad`/`Tourist`, diambil dari `language_matrix`. Kalau Task_Brief minta dua segmen, buat dua naskah terpisah (bukan satu naskah bilingual dicampur).
5. **Status aset** — salin dari hasil `sunmedia-noza-asset-stock-check` (`ASSET_READY` / `NEEDS_NEW_SHOOT` / `NEEDS_ASSET_CONFIRMATION`).
6. **Hook** — lihat bagian 2.
7. **Body** — alur isi setelah hook, dipecah per beat/adegan singkat (bukan paragraf panjang).
8. **Arahan visual/audio** — apa yang terlihat dan terdengar di tiap beat (shot type, teks di layar, musik/SFX, bukan hanya narasi lisan).
9. **CTA** — satu tindakan jelas di akhir (sesuai `primary_offer`), bukan beberapa CTA bersaing.

## 2. Hook — prioritas nomor satu

- Tulis **2–3 alternatif hook** (3 detik pertama) untuk tiap naskah sebelum memilih satu.
- Tiap alternatif beri label arketipe singkat (mis. "pertanyaan provokatif", "klaim mengejutkan", "potongan hasil/before-after", "POV/relatable moment").
- Setelah memilih satu, tulis SATU kalimat alasan kenapa itu yang dipilih (mis. "paling spesifik ke objection utama ICP", "paling cocok dengan nada `core_value_proposition`").
- Hook TIDAK BOLEH menyebut brand/nama produk di detik pertama kecuali itu sendiri yang jadi kejutan (nama besar/dikenal) — buka dengan masalah/rasa penasaran dulu, baru masuk ke penawaran.

## 3. Segmen bahasa — bedakan nada, bukan cuma terjemahan

- **`Local_ID`**: Bahasa Indonesia sehari-hari, nada dekat/informal, sering mengarah ke CTA "chat WA sekarang", bukti sosial (testimoni, jumlah pembeli) dan harga/promo biasanya disebut eksplisit.
- **`Expat_Nomad`/`Tourist`**: Bahasa Inggris, nada lebih halus dan berbasis kepercayaan (review, kredensial, keamanan transaksi), ekspektasi produksi visual sedikit lebih rapi, CTA biasanya mengarah ke link/booking form, bukan selalu WA.
- Jangan menerjemahkan hook `Local_ID` kata demi kata ke Inggris untuk segmen kedua — tulis ulang dengan logika budaya yang sesuai, tapi `primary_offer` dan `core_value_proposition` yang mendasarinya harus tetap identik secara substansi (message-match).

## 4. Mode DELTA_SPRINT (lihat `hermes-agent-workflow-v2.md` bagian 9, Level 2)

Saat Task_Brief bermode `DELTA_SPRINT` dengan `delta_sprint_focus` (mis. "4-6 hook baru, creative fatigue"):
- Hanya tulis **hook tambahan terfokus** pada akar masalah yang disebut — JANGAN menulis ulang paket 12 naskah penuh dari nol.
- Tetap sertakan body/arahan visual/CTA ringkas untuk tiap hook baru (bukan hook tanpa konteks), tapi body boleh lebih singkat karena biasanya dipasangkan ke footage/format yang sudah ada.

## 5. Checklist sebelum naskah dianggap selesai

- [ ] Platform, durasi, segmen bahasa, status aset semua terisi
- [ ] ≥2 alternatif hook ditulis, satu dipilih dengan alasan
- [ ] Body dipecah per beat, bukan paragraf naratif panjang
- [ ] Arahan visual/audio eksplisit per beat, bukan hanya narasi
- [ ] Satu CTA jelas, sesuai `primary_offer` dari Task_Brief
- [ ] Tidak ada angka KPI/proyeksi performa yang dikarang di dalam naskah

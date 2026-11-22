---
name: sunmedia-orca-context-slicing
description: Memotong Master_Brief [LOCKED_EXECUTION] milik Noya menjadi Task_Brief terpisah untuk Noza (2A), Arka (2B), Weba (2C), dan Yuka (2D), dengan field core_value_proposition/primary_offer/language_matrix yang identik di keempatnya (prinsip message-match). Jalankan saat dipanggil /slice_brief, atau segera setelah PM mengunci Gate 1.5 (LOCKED_EXECUTION).
---

# Context Slicing — Orca

Skill ini TIDAK membuat strategi atau angka baru. Tugasnya murni memotong satu dokumen
(`Master_Brief [LOCKED_EXECUTION]`) menjadi empat dokumen lebih kecil, satu per agent
Fase 2, dengan field inti yang WAJIB identik di keempatnya.

Muat dulu skill `sunmedia-client-folder-standard` untuk aturan lokasi/penamaan file —
skill ini tidak mendefinisikan ulang aturan folder.

## Kapan dijalankan
- Dipicu otomatis segera setelah PM mengunci Gate 1.5 (status `Master_Brief` berubah ke
  `LOCKED_EXECUTION`).
- Dipicu manual via command `/slice_brief <client_slug>`.
- Dipicu ulang (re-slice) bila `Master_Brief` naik versi mayor (re-lock) setelah Level 3
  Major Pivot di Fase 4.

## Langkah

1. **Baca sumber.** Baca `02_Master_Brief_Noya/master-brief-active.md` dan `.json` untuk
   `client_slug` terkait. Jika status BUKAN `LOCKED_EXECUTION`, STOP — jangan memotong
   `PROVISIONAL_PITCH`, laporkan ke PM bahwa brief belum terkunci untuk eksekusi.
2. **Validasi skema sumber dulu.** Jalankan `sunmedia-orca-deterministic-qa` pada
   `master-brief-active.json` sebelum memotong — jangan memotong brief yang skemanya
   sendiri tidak valid; itu hanya akan menyebar kesalahan ke empat Task_Brief.
3. **Ambil field wajib-identik** dari Master_Brief JSON:
   - `core_value_proposition`
   - `primary_offer`
   - `language_matrix` (array per segmen — Local_ID / Expat_Nomad / Tourist)
   Field ini disalin APA ADANYA ke keempat Task_Brief, tanpa diringkas atau ditafsirkan
   ulang. Bila salah satu field ini kosong di Master_Brief, STOP dan lapor ke PM — jangan
   mengisi sendiri.
4. **Potong field khusus per agent** (referensi, sesuaikan dengan skema `Master_Brief`
   final Noya):
   - **Noza (2A — Content):** `channel_mix` (ringkas nama kanal saja), `funnel_tracking.funnel_model`, kebutuhan jumlah/jenis konten dari `Master_Brief` bagian channel mix, `target_segment` per kanal.
   - **Arka (2B — Paid Ads):** `channel_mix` penuh (budget per kanal, target_segment), `unit_economics` (Net Ad Spend, CPL/CAC target, Break-Even ROAS), `funnel_tracking` penuh.
   - **Weba (2C — CRO/LP/WA):** `funnel_tracking` (`funnel_model`, `ref_code_prefix`), `unit_economics.assumed_cs_close_rate_pct`, `language_matrix` (penekanan ekstra karena menulis copy LP).
   - **Yuka (2D — SEO/GEO):** `icp`/ICP terkait (bila Master_Brief menyimpan rujukan ke `Client_Intelligence_Brief` Nara), `target_segment` geografis (Local_ID vs Expat_Nomad menentukan bahasa konten SEO/GBP).
5. **Tulis output** ke `03_Task_Briefs_Orca/<agent-slug>/task-brief-active.md` dan `.json`
   (agent-slug: `noza`, `arka`, `weba`, `yuka`), sesuai pola folder standar. Pindahkan
   versi lama ke `archive/` dulu bila ada.
6. **Validasi hasil.** Jalankan `sunmedia-orca-deterministic-qa` pada keempat Task_Brief
   yang baru ditulis sebelum mengumumkan selesai — pastikan field wajib-identik benar-benar
   identik karakter demi karakter di keempatnya (bukan hanya "mirip").
7. **Update tracker & notifikasi.** Catat baris baru di tab tracker Orca untuk tiap agent
   (status awal: `MENUNGGU_GATE_2`), kirim notifikasi ke PM dan ke masing-masing profile
   agent Fase 2 dengan link Task_Brief masing-masing.

## Yang TIDAK boleh dilakukan
- Mengubah, membulatkan, atau "memperbaiki" angka dari Master_Brief.
- Menulis Task_Brief untuk agent yang belum ada foldernya di `03_Task_Briefs_Orca/` tanpa
  membuat subfolder itu dulu sesuai pola standar.
- Memotong brief yang statusnya masih `PROVISIONAL_PITCH`.
- Menyimpulkan field yang kosong di Master_Brief — selalu lapor ke PM, jangan menebak.

## Checklist sebelum selesai
- [ ] Status sumber `LOCKED_EXECUTION`, bukan draft/provisional.
- [ ] `core_value_proposition`, `primary_offer`, `language_matrix` identik di 4 Task_Brief.
- [ ] Field khusus per agent hanya berisi yang relevan untuk peran itu (bukan seluruh Master_Brief disalin mentah).
- [ ] File aktif lama (bila ada) sudah dipindah ke `archive/`.
- [ ] Baris tracker dan notifikasi terkirim ke agent + PM.

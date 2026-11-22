---
name: sunmedia-noya-onboarding-calibration
description: Mengkalibrasi Draft Master_Brief (PROVISIONAL_PITCH) menggunakan data riil dari Post-Deal Onboarding Form setelah klien deal WON untuk menghasilkan Master_Brief versi LOCKED_EXECUTION. Gunakan saat trigger /calibrate_brief dipanggil di Jalur B (Gate 1.5).
---

# Prosedur Kalibrasi Onboarding (`/calibrate_brief`)

## 1. Input Wajib
1. `master-brief-active.json` & `.md` (status `PROVISIONAL_PITCH`) dari folder `02_Master_Brief_Noya/`.
2. `Post-Deal Onboarding Form` yang telah diisi klien dan diverifikasi PM.

## 2. Audit Data Onboarding & Tracking Readiness
Periksa empat kelompok data internal klien:
1. **Unit Economics Riil:** `AOV` aktual, `HPP / COGS`, `gross_margin_pct` aktual, promo/diskon aktif (`primary_offer`), serta metode pembayaran (`QRIS/Transfer/CC/Payment Gateway`).
2. **Kapasitas Operasional & CS:**
   - Jumlah CS aktif, jam operasional balas chat WA, dan histori `cs_close_rate_pct` aktual (jika sudah pernah beriklan/jualan online).
   - Kapasitas layanan maksimal per hari/bulan (`max_monthly_service_capacity` — misal jumlah slot kamar/villa, kapasitas produksi botol/hari, atau slot pasien klinik).
3. **Kesiapan Teknis Tracking (rujuk skill `analytics`):**
   - Status akses Meta Business Manager, Pixel + Conversions API (CAPI), Google Tag/GA4, TikTok Pixel, serta format `ref_code_prefix` untuk pelacakan closing WA manual oleh CS.
4. **Budget Terkonfirmasi:** Nilai `monthly_net_ad_spend` yang disepakati di kontrak (setelah dipisahkan dari PPN 11% dan Agency Fee).

**Aturan Ambiguitas (Threshold 10%):** Jika ada isian form onboarding yang kosong/kontradiktif dan berpotensi menggeser CAC/ROAS/kapasitas **>10%**, tanyakan langsung ke PM sebelum menghitung final.

## 3. Rekalkulasi & Pengecekan Kapasitas
1. Hitung ulang `break_even_roas` riil berdasarkan `gross_margin_pct` aktual klien (`[SRC:klien]`).
2. Hitung ulang proyeksi `cpl_estimate`, `cac_estimate`, dan `roas_estimate` per kanal dengan mengganti asumsi pitch menjadi data klien.
3. **Cap Kapasitas:** Pastikan total proyeksi jumlah customer/lead tidak melebihi `max_monthly_service_capacity`. Jika budget klien berpotensi menghasilkan demand di atas kapasitas operasional, turunkan proyeksi volume atau rekomendasikan pergeseran alokasi budget ke produk/layanan dengan margin lebih tinggi.

## 4. Kalkulasi Deviasi & Aturan Ambang 20%
Bandingkan seluruh angka hasil kalibrasi terhadap versi `PROVISIONAL_PITCH`:
- `cac_deviation_pct = ((cac_calibrated - cac_pitch) / cac_pitch) × 100`
- `roas_deviation_pct = ((roas_calibrated - roas_pitch) / roas_pitch) × 100`
- `capacity_deviation_pct = ((cap_calibrated - cap_pitch) / cap_pitch) × 100`
- `max_abs_deviation_pct = max(|cac_deviation_pct|, |roas_deviation_pct|, |capacity_deviation_pct|)`

**Aturan Wajib (`>20%` Threshold):**
- Jika `max_abs_deviation_pct > 20%` (misal: HPP klien ternyata lebih tinggi sehingga ROAS terkalibrasi turun >20% dari janji di pitch deck, atau kapasitas CS terlalu kecil):
  1. Set `"requires_client_reconfirmation": true` pada JSON.
  2. Tahan dokumen di status `PENDING_CLIENT_RECONFIRMATION` (jangan ajukan lock Gate 1.5 dulu).
  3. Kirim laporan perbandingan angka Pitch vs Kalibrasi + akar penyebab ke **PM** untuk dikonfirmasi ulang ke klien.
  4. **SLA Eskalasi:** Jika PM belum merespons dalam **1×24 jam**, kirim eskalasi otomatis ke **Yohanes** (`YOHANES_ESCALATION_CHAT_ID`).
- Jika `max_abs_deviation_pct ≤ 20%`:
  1. Set `"requires_client_reconfirmation": false`.
  2. Tandai dokumen siap dikunci PM di Gate 1.5.

## 5. Penyimpanan & Notifikasi Gate 1.5
1. Validasi schema via `noya-master-brief-schema`.
2. Simpan `master-brief-active.md`, `master-brief-active.json`, dan arsip `archive/master-brief-locked-v1.0.md` (setelah PM memberi konfirmasi lock Gate 1.5).
3. Update tab `Noya_Tracker` via `noya-tracker-sheet`.
4. Beri notifikasi ke **PM** dan **Orca** bahwa `Master_Brief` siap/telah terkunci untuk distribusi *context slicing* ke Fase 2 (Noza, Arka, Weba, Yuka).

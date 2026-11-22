---
name: sunmedia-noya-pitch-estimation
description: Mengonversi Client_Intelligence_Brief dari Nara dan benchmark industri menjadi Draft Master_Brief (mode pitch) lengkap dengan perhitungan unit economics, channel mix, dan proyeksi funnel untuk pasar Indonesia & Bali. Gunakan saat trigger /pitch_brief dipanggil atau saat PM/PD meminta estimasi angka untuk proposal klien baru.
---

# Prosedur Estimasi Pitch (`/pitch_brief`)

## 1. Input Wajib & Pengecekan Awal
1. Baca `Client_Intelligence_Brief` milik klien dari Google Drive (`01_Research_Nara/`) atau path yang diberikan.
2. Periksa riwayat `Knowledge_Base` (via `noya-learning-log`) untuk industri, segmen pasar (`Local_ID` vs `Expat_Nomad` / `Tourist`), dan model funnel yang serupa.
3. Identifikasi parameter dasar dari brief Nara:
   - Model bisnis & produk/layanan utama
   - Estimasi harga jual rata-rata (`AOV` dalam IDR)
   - Target segmen & geografi (misal: Canggu/Seminyak/Ubud untuk Expat/Tourist; Denpasar/Nasional untuk Local_ID)
   - Tujuan bisnis / target omzet / indikasi budget awal dari prospek (jika ada)

## 2. Aturan Ambiguitas (Threshold 10%)
- Jika informasi harga jual (`AOV`), target volume, atau batasan budget dari brief prospek tidak jelas dan berpotensi mengubah proyeksi CAC/ROAS/budget **>10%**, hentikan pembuatan draft dan ajukan pertanyaan terfokus ke PM sebelum lanjut.
- Jika dampaknya **≤10%**, gunakan asumsi konservatif dan wajib beri tag `[ASUMSI — PERLU VALIDASI PM]`.

## 3. Pengumpulan Benchmark & Tag Sumber
1. Cek data historis internal Sun Media di `Knowledge_Base` terlebih dahulu (`[SRC:internal-KB]`).
2. Jika data internal belum tersedia untuk industri/segmen tersebut, lakukan pencarian web untuk benchmark CTR, CPC, CPL, dan CVR pasar Indonesia/SEA (`[SRC:benchmark | Nama Sumber & Tahun | Geografi]`).
3. Terapkan **faktor koreksi konservatif** pada benchmark publik (minimal +15% hingga +25% pada estimasi CPL/CAC, atau -15% pada CVR) agar proyeksi pitch tidak over-promise.

## 4. Kalkulasi Unit Economics & Funnel Mundur (Backward Math)
Pisahkan seluruh komponen biaya dalam **IDR**:
1. **Struktur Budget:**
   - `monthly_net_ad_spend`: Saldo iklan murni yang masuk ke platform (Meta/Google/TikTok).
   - `estimated_tax_vat_pct`: Default `11`% (PPN iklan digital Indonesia).
   - `monthly_gross_ad_spend`: `monthly_net_ad_spend × 1.11`.
   - `agency_fee`: Dicatat terpisah dari Ad Spend (jangan pernah mencampur agency fee ke dalam pembagi ROAS iklan kecuali diminta eksplisit oleh PM).
2. **Break-Even Guardrail:**
   - Jika estimasi `gross_margin_pct` tersedia di brief Nara, hitung `break_even_roas = 1 / (gross_margin_pct / 100)`.
   - Jika belum tersedia saat pitch, gunakan estimasi margin konservatif industri dan tandai `[ASUMSI — PERLU VALIDASI PM]`.
3. **Alur Funnel (sesuai `funnel_model`):**
   - **CTWA (Click-to-WhatsApp — mayoritas bisnis Bali/ID):**
     `Net Ad Spend` → `Impresi` (via CPM) → `Klik/Chat WA` (via CTR & CPC) → `Qualified Leads` (`target_cpl`) → `Closing` (via `assumed_cs_close_rate_pct`, default konservatif 10%–20% tergantung tiket harga) → `CAC` (`Net Ad Spend / Total Closing`) → `ROAS` (`(Total Closing × AOV) / Net Ad Spend`).
   - **LP_LEAD / DIRECT_CHECKOUT:**
     Ganti tahap `cs_close_rate` dengan `lp_conversion_rate_pct` dan `checkout_completion_rate_pct`.
4. **Simulasi Python (Opsional / Multi-Channel):**
   - Jalankan `scripts/marketing_budget_modeler.py` dan `scripts/growth_model_simulator.py` via terminal untuk memodelkan pembagian budget antar kanal dan payback period.
   - Konversi seluruh parameter input ke satuan numerik penuh (IDR tanpa pemisah titik/koma) dan validasi hasil script terhadap formula funnel di atas.

## 5. Penentuan Channel Mix & Language Matrix
- Alokasikan budget maksimal pada 2–3 kanal utama agar tidak memecah fase *learning* algoritma iklan:
  - **Meta Ads (IG/FB):** Utama untuk CTWA, visual lifestyle, retargeting, dan geo-fencing Bali.
  - **TikTok Ads / Spark Ads:** Utama untuk awareness cepat, F&B, wellness, dan massa `Local_ID`.
  - **Google Search / Maps / PMax:** Utama untuk *high-intent search* (klinik, sewa villa, jasa B2B, ekspat pencari layanan spesifik).
- Isi `language_matrix` sebagai **array** terpisah per segmen (`Local_ID` → `id`, `Expat_Nomad`/`Tourist` → `en`) beserta porsi persentase budget masing-masing (total harus `100%`).

## 6. Finalisasi Output & Handover
1. Validasi struktur dokumen menggunakan prosedur di skill `noya-master-brief-schema` dengan status `PROVISIONAL_PITCH` dan versi `v1.0`.
2. Simpan `master-brief-active.md`, `master-brief-active.json`, dan salinan arsip `archive/master-brief-provisional-v1.0.md` ke folder Google Drive `02_Master_Brief_Noya/`.
3. Catat entri baru via skill `noya-tracker-sheet`.
4. Kirim ringkasan eksekutif (angka kunci, asumsi yang butuh validasi, dan link Drive) ke **PM** untuk persetujuan Gate 1 dan **PD** sebagai bahan pitch deck.

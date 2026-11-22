---
name: sunmedia-noya-learning-log
description: Menyimpan pembelajaran akurasi estimasi vs aktual serta pola CAC/ROAS/CPL per industri, segmen pasar (Local_ID vs Expat_Nomad), kanal, dan tipe funnel ke Knowledge_Base Noya setiap akhir siklus evaluasi Fase 4 atau setelah kalibrasi onboarding.
---

# Prosedur Pembaruan Pengetahuan (`Knowledge_Base` Learning Log)

## 1. Kapan Dijalankan
1. **Setelah Kalibrasi Gate 1.5 selesai:** Mencatat seberapa akurat estimasi pitch Noya dibanding data internal riil klien (`pitch_vs_calibrated_variance`).
2. **Setiap akhir siklus bulanan (4 minggu) di Fase 4:** Mencatat performa aktual kampanye dibanding target `LOCKED_EXECUTION` (`calibrated_vs_actual_variance`).

## 2. Struktur Metadata Wajib
Setiap entri yang disimpan ke `memories/` dan `Knowledge_Base` wajib memiliki tag metadata standar agar mudah dicari saat `/pitch_brief` klien berikutnya:

```yaml
---
entry_id: "KB-NOYA-YYYYMMDD-<ClientSlug>"
industry: "<misal: F&B_Wellness | Hospitality_Villa | Aesthetic_Clinic | Property_Sales | Education>"
market_segment: "<Local_ID | Expat_Nomad | Hybrid | Tourist>"
geo_focus: "<misal: Canggu_Seminyak | Ubud | Denpasar | Nasional_ID>"
channel: "<Meta_Ads | Google_Maps | Google_Search | TikTok_Ads>"
funnel_type: "<CTWA | DIRECT_CHECKOUT | LP_LEAD>"
ticket_size_tier: "<LOW_LT_250K | MID_250K_2JT | HIGH_GT_2JT>"
pitch_vs_actual_variance_pct: 0
delta_sprint_triggered_count: 0
---
```

## 3. Isi Ringkasan Pembelajaran (Maksimal 150 Kata per Entri)
Catat tiga poin padat:
1. **Angka Aktual Terverifikasi:** Rentang CPM, CTR, CPL, `cs_close_rate`, CAC, dan ROAS aktual yang berhasil dicapai (dalam IDR).
2. **Pelajaran Kalibrasi (Bias yang Ditemukan):** Apa yang terlalu optimistis atau terlalu pesimistis pada estimasi awal (misal: *"Pada klinik estetika di Denpasar dengan funnel CTWA, CPL Meta cukup murah Rp22.000, namun cs_close_rate hanya 12% karena pasien butuh konsultasi dokter dulu — estimasi awal 20% terlalu tinggi"*).
3. **Aturan Baru untuk Pitch Berikutnya:** Penyesuaian parameter yang harus diterapkan Noya saat menangani klien baru di kategori serupa.

## 4. Sanitasi Data Klien
Jangan menyimpan kredensial akun, API key, atau data pribadi pelanggan akhir (nama/nomor HP leads) ke dalam `Knowledge_Base`—hanya simpan metrik agregat dan pola performa.

---
name: sunmedia-noya-weekly-performance-review
description: Mengevaluasi performa kampanye mingguan di Fase 4 dari tab Raw_Weekly_Data, menyusun Performance_Review_Brief 4 bagian, mendeteksi data tidak lengkap, dan menentukan eskalasi Level 1, Level 2 (DELTA_SPRINT), atau Level 3 (Major Pivot). Gunakan saat trigger /report_weekly dijalankan.
---

# Prosedur Review Performa Mingguan (`/report_weekly`)

## 1. Input Wajib
1. Tab `Raw_Weekly_Data` pada Google Sheet klien/tracker untuk periode minggu evaluasi (`YYYY-Www`).
2. `master-brief-active.json` (status `LOCKED_EXECUTION`) dari folder `02_Master_Brief_Noya/`.

## 2. Langkah 1: Audit Integritas & Kelengkapan Data (Rujuk Skill `analytics`)
Sebelum menghitung KPI, periksa integritas baris data minggu berjalan:
- **Cek Anomali Tracking:** Apakah ada spend berjalan tanpa impresi/klik, atau klik tinggi (>100) dengan 0 event konversi akibat pixel/UTM/Ref_Code putus? Jika ya, tandai `TRACKING_ANOMALY` dan eskalasi teknis ke Arka/Weba via PM.
- **Protokol `INCOMPLETE_DATA`:**
  - Jika data platform iklan (`spend`, `impressions`, `clicks`, `leads_wa`) sudah masuk, namun data `closing_count` dan `actual_revenue` per `Ref_Code` dari tim CS klien belum lengkap:
    1. **Jangan mengarang atau mengestimasi angka closing/CAC/ROAS.**
    2. Hitung dan laporkan seluruh metrik *upper/mid-funnel* yang valid (CPM, CTR, CPC, CPL).
    3. Isi kolom CAC & ROAS aktual dengan `PENDING (INCOMPLETE_DATA)` dan cantumkan daftar kolom yang belum diisi CS pada bagian paling atas laporan.

## 3. Langkah 2: Diagnosis Root Cause per Lapisan Funnel (Rujuk Skill `attribution`)
Bandingkan aktual mingguan vs target mingguan prorata di `master-brief-active.json`, lalu isolasi masalah pada titik putus yang tepat:
- **CPM melonjak >25% & CTR turun >25%:** *Creative fatigue* atau audiens jenuh → Masalah di **Hook/Visual/Angle (Noza)** (ambang CTR disamakan dengan ambang resmi Level 2 di bagian 4).
- **CTR sehat, tetapi CVR klik-ke-lead/LP rendah (>20% di bawah target):** Ketidaksesuaian janji iklan dengan halaman tujuan atau kecepatan muat LP → Masalah di **Landing Page / Offer Framing (Weba)**.
- **CPL sesuai target, tetapi `cs_close_rate` aktual jauh di bawah `assumed_cs_close_rate_pct`:** Kualitas lead kurang tepat atau respon/script follow-up CS klien lambat → Masalah di **Targeting Kualitas Lead (Arka) atau Script Bantahan CS (Weba/PM)**.
- **Multi-channel overlap:** Gunakan prinsip incrementality/atribusi dari skill `attribution` agar tidak mematikan kanal TOF (misal TikTok Awareness) yang ternyata mendorong pencarian Branded Search di Google.

## 4. Langkah 3: Klasifikasi Level Keputusan
Tentukan tepat **satu** level keputusan berdasarkan ambang batas berikut:

| Level | Kondisi Pemicu | Tindakan Noya & Alur |
|---|---|---|
| **LEVEL_1_MINOR** | Deviasi metrik utama ≤15%, tidak ada kelelahan kreatif berat, atau hanya butuh realokasi budget antar ad-set/kanal **≤20%** untuk memaksimalkan ROAS. | Noya menyusun rekomendasi optimasi bidding/budget, memperbarui `master-brief-active` ke versi minor berikutnya (`v1.x`) jika ada geser budget, dan mencatat di tracker. |
| **LEVEL_2_DELTA_SPRINT** | CTR turun **>25%** (creative fatigue, sesuai `hermes-agent-workflow-v2.md` bagian 9), CVR LP turun >20%, atau `cs_close_rate` meleset signifikan akibat kelemahan angle/bantahan, sementara unit economics dasar masih sehat. | Noya **dilarang** mengubah struktur besar sendiri. Wajib menjalankan skill `noya-delta-sprint-brief` untuk menerbitkan `Task_Brief` mode `DELTA_SPRINT` bagi Noza dan/atau Weba lewat **Orca**. |
| **LEVEL_3_MAJOR_PIVOT** | CAC aktual **>2x batas maksimal** selama **2–3 minggu berturut-turut** (sesuai `hermes-agent-workflow-v2.md` bagian 9 — bukan sekadar >20% di atas batas aman), atau penawaran ditolak pasar, atau kapasitas layanan klien kolaps. | Noya merekomendasikan penghentian/pemangkasan spend darurat dan mengeskalasi ke **PM & Yohanes** untuk mengembalikan proyek ke Fase 1 (reset strategi/offer). Wajib Gate 1 dan Gate 1.5 ulang. |

## 5. Format Output `Performance_Review_Brief` (4 Bagian Tetap)
Simpan ke `04_Weekly_Reviews_Noya/weekly-review-YYYY-Www.md`:
1. **Ringkasan Deviasi:** Tabel perbandingan `Target` vs `Aktual` vs `Deviasi %` (Spend Net, CTR, CPL, Leads/WA, Closing per Ref_Code, CAC, ROAS, Status Data).
2. **Analisis Root Cause:** Diagnosis berbasis data per kanal dan per segmen (`Local_ID` vs `Expat_Nomad`).
3. **Rekomendasi Optimasi:** Daftar aksi konkret (apa yang diubah, di kanal mana, dan estimasi dampak angkanya).
4. **Flag Keputusan:** Status (`LEVEL_1_MINOR` / `LEVEL_2_DELTA_SPRINT` / `LEVEL_3_MAJOR_PIVOT`), justifikasi numerik, serta lampiran `Task_Brief` jika Level 2.

## 6. Post-Review Actions
1. Update tab `Noya_Tracker` via `noya-tracker-sheet`.
2. Jika siklus evaluasi bulanan/4-mingguan selesai, jalankan `noya-learning-log` untuk menyimpan pembelajaran ke `Knowledge_Base`.

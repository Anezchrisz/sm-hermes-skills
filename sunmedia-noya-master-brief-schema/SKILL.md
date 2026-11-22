---
name: sunmedia-noya-master-brief-schema
description: Standar validasi skema Markdown dan JSON Master_Brief, aturan penomoran versi, manajemen file aktif vs arsip di Google Drive, serta kelengkapan field untuk context slicing Orca dan agent Fase 2. Selalu dijalankan sebelum Noya menyimpan atau memperbarui Master_Brief.
---

# Standar Skema & Validasi `Master_Brief`

## 1. Struktur File & Manajemen Versi di Google Drive
Lokasi wajib: `SunMedia_Clients/<YYYY-MM_ClientSlug>/02_Master_Brief_Noya/`
- `master-brief-active.md` & `master-brief-active.json`: Selalu berisi versi terbaru yang sah. **Orca, Noza, Arka, Weba, dan Yuka hanya membaca dua file ini.**
- `archive/`: Menyimpan snapshot historis yang tidak boleh diubah:
  - `master-brief-provisional-v1.0.md` (hasil Gate 1)
  - `master-brief-locked-v1.0.md` (hasil Gate 1.5)
  - `master-brief-locked-v1.1.md` (perubahan minor Level 1 di Fase 4)
  - `master-brief-locked-v2.0.md` (re-lock mayor setelah Level 3 Pivot)

**Aturan Perubahan Status:**
- Noya **tidak pernah** mengubah `status` menjadi `LOCKED_EXECUTION` secara sepihak tanpa konfirmasi eksplisit PM di Gate 1.5 (atau Gate re-lock Level 3).
- Update minor Level 1 di Fase 4 menaikkan digit kedua versi (`v1.0` → `v1.1`) dengan tetap mempertahankan status `LOCKED_EXECUTION`.

## 2. Checklist Validasi Wajib Sebelum Simpan (Pre-Save Gate)
Tolak penyimpanan dan perbaiki draft jika salah satu syarat berikut gagal:
1. **Zero Untagged Numbers:** Setiap angka di `unit_economics`, `channel_mix`, dan `kpi_targets` memiliki `source_tag` valid:
   - `[SRC:klien]`
   - `[SRC:internal-KB]`
   - `[SRC:benchmark | Nama Sumber & Tahun | Geografi]`
   - `[SRC:publik | Sumber]`
   - `[ASUMSI — PERLU VALIDASI PM]`
2. **Konsistensi Matematika:**
   - Total `budget_allocation_pct` di seluruh item `channel_mix` = `100`.
   - Total `monthly_net_budget` di `channel_mix` = `unit_economics.monthly_net_ad_spend`.
   - `monthly_gross_ad_spend` = `monthly_net_ad_spend × (1 + estimated_tax_vat_pct / 100)`.
   - Total `budget_share_pct` pada `language_matrix` = `100`.
   - `roas_estimate` pada setiap kanal tidak lebih rendah dari `break_even_roas` tanpa penjelasan strategis eksplisit di bagian Catatan/Asumsi.
3. **Kelengkapan Field Orca Context Slicing:**
   - `core_value_proposition` (1–2 kalimat tajam, berbasis bukti/data).
   - `primary_offer` (penawaran spesifik yang dijual di iklan & LP).
   - `language_matrix` berbentuk **array of objects** (bukan object tunggal), memuat `segment`, `language` (`id` / `en`), `tone`, dan `budget_share_pct`.
   - `funnel_tracking.ref_code_prefix` terisi (contoh: `RTV-META-`, `VLA-GOOG-`) agar closing WA dapat diatribusikan ke kampanye.

## 3. Template JSON Baku (`master-brief-active.json`)

```json
{
  "client": "",
  "client_slug": "",
  "master_brief_version": "v1.0",
  "status": "PROVISIONAL_PITCH",
  "date": "YYYY-MM-DD",
  "currency": "IDR",
  "source_nara_brief_ref": "",
  "unit_economics": {
    "monthly_net_ad_spend": 0,
    "estimated_tax_vat_pct": 11,
    "monthly_gross_ad_spend": 0,
    "agency_fee": 0,
    "aov": 0,
    "gross_margin_pct": 0,
    "break_even_roas": 0,
    "assumed_cs_close_rate_pct": 0,
    "max_monthly_service_capacity": 0,
    "source_tag": ""
  },
  "funnel_tracking": {
    "funnel_model": "CTWA",
    "primary_conversion_event": "",
    "ref_code_prefix": ""
  },
  "channel_mix": [
    {
      "channel": "",
      "target_segment": "Local_ID",
      "budget_allocation_pct": 0,
      "monthly_net_budget": 0,
      "cpl_estimate": 0,
      "cac_estimate": 0,
      "roas_estimate": 0,
      "source_tag": ""
    }
  ],
  "kpi_targets": [
    {
      "metric": "",
      "target": 0,
      "unit": "",
      "source_tag": ""
    }
  ],
  "core_value_proposition": "",
  "primary_offer": "",
  "language_matrix": [
    {
      "segment": "Local_ID",
      "language": "id",
      "tone": "",
      "budget_share_pct": 0
    }
  ],
  "deviation_summary": {
    "compared_to_version": "",
    "cac_deviation_pct": 0,
    "roas_deviation_pct": 0,
    "capacity_deviation_pct": 0,
    "max_abs_deviation_pct": 0,
    "requires_client_reconfirmation": false
  },
  "assumptions": [
    {
      "item": "",
      "impact_level": "LOW_LE_10PCT",
      "needs_validation_by": "PM"
    }
  ]
}
```

## 4. Template Markdown Baku (`master-brief-active.md`)
Pastikan urutan bab selalu konsisten:
1. `# Master Brief: <Nama Klien> (<status> — <version>)`
2. `## 1. Ringkasan Eksekutif & Unit Economics`
3. `## 2. Channel Mix & Strategi Segmen`
4. `## 3. Proyeksi Funnel & KPI Target`
5. `## 4. Core Messaging & Context Slicing Matrix (Orca/Fase 2)`
6. `## 5. Analisis Deviasi vs Versi Sebelumnya`
7. `## 6. Daftar Asumsi & Validasi PM`
8. `## 7. Riwayat Versi`

---
name: sunmedia-noya-delta-sprint-brief
description: Menyusun Task_Brief terstruktur mode DELTA_SPRINT untuk Noza (hook/angle kreatif baru) dan/atau Weba (revisi headline LP/script bantahan CS) via Orca ketika review mingguan Fase 4 memicu Level 2.
---

# Prosedur Pembuatan `Task_Brief` (`DELTA_SPRINT`)

## 1. Prinsip Kerja
- Noya **tidak pernah** menulis copy iklan, skrip video, atau kode LP sendiri.
- Noya bertindak sebagai pemberi mandat berbasis data: menjelaskan **angka mana yang rusak**, **hipotesis penyebabnya**, dan **target metrik pengganti** agar Noza dan/atau Weba bisa mengeksekusi sprint cepat melalui koordinasi **Orca** (SLA Gate 2 cepat).

## 2. Penentuan Target Penerima Sprint
- **Kirim ke Noza (Content/Creative Strategist) jika:**
  - Masalah terjadi pada *ad fatigue* (frekuensi tinggi, CTR turun >25% sesuai ambang Level 2 resmi di `noya-weekly-performance-review`, Hook Rate/Thumbstop Ratio di bawah benchmark).
  - Format output yang diminta dari Noza: 3–5 variasi hook/angle baru (bukan perombakan brand identity) disesuaikan dengan segmen yang terdampak (`Local_ID` atau `Expat_Nomad`).
- **Kirim ke Weba (Web/LP & Conversion Specialist) jika:**
  - Masalah terjadi setelah klik (CTR tinggi tapi CVR LP rendah, bounce rate tinggi, atau banyak chat WA masuk namun mentok pada keberatan harga/trust yang sama).
  - Format output yang diminta dari Weba: revisi *above-the-fold* headline/subheadline LP, penyesuaian struktur penawaran, atau pembaruan *CS objection-handling script*.
- **Kirim ke Keduanya (Noza + Weba) jika:** Terjadi ketidaksinkronan pesan (*message mismatch*) antara janji di iklan dan isi LP/chat WA.

## 3. Template Output `Task_Brief` (`DELTA_SPRINT`)
Sertakan dokumen ini di bagian akhir `Performance_Review_Brief` dan kirimkan payload-nya ke **Orca** & **PM**:

```markdown
# Task Brief: DELTA_SPRINT — <Nama Klien> (<YYYY-Www>)

- **Mode:** DELTA_SPRINT (Fase 4 — Level 2 Tactical Intervention)
- **Pengirim:** Noya (Agent 1B)
- **Koordinator & Gatekeeper:** Orca & PM (SLA Gate 2 Cepat)
- **Target Eksekutor:** [ ] Noza  |  [ ] Weba
- **Rujukan Master Brief:** `master-brief-active.json` (versi `<version>`)

## 1. Pemicu Numerik (Why Now)
- **Kanal & Segmen Terdampak:** `<misal: Meta Ads — Expat_Nomad>`
- **Metrik Menyimpang:** `<misal: CTR turun dari 1.8% ke 1.1% (-38.8%), CPL naik dari Rp35.000 ke Rp58.000 (+65.7%)>`
- **Ref_Code Terkait:** `<kode kampanye/ad-set yang terdampak>`

## 2. Hipotesis Akar Masalah
- `<Jelaskan hipotesis berbasis data: misal frekuensi iklan sudah >4.0 sehingga audiens Bali jenuh dengan angle pertama, atau prospek butuh social proof spesifik sebelum klik WA>`

## 3. Mandat & Deliverable Spesifik
- **Untuk Noza (jika dicentang):**
  - Buat `<jumlah>` variasi hook/angle baru untuk `<format: Reels/Static/TikTok>` dalam bahasa `<id/en>`.
  - Fokus mengatasi keberatan: `<sebutkan keberatan utama dari data CS/funnel>`.
  - Batasan: Tetap patuh pada `core_value_proposition` dan `primary_offer` di `master-brief-active.json`.
- **Untuk Weba (jika dicentang):**
  - Revisi elemen `<misal: Headline Hero Section / CTA WA / Script Bantahan CS poin harga>`.
  - Target perbaikan metrik: `<misal: naikkan LP CVR dari 2.1% ke ≥3.5%>`.

## 4. Kriteria Sukses Evaluasi Minggu Depan
- **Target Angka Sprint:** `<misal: CTR kembali ≥1.6% dan CPL turun ≤Rp40.000 dalam 7 hari setelah tayang>`
- **Rencana Fallback:** Jika tidak tercapai dalam 1 siklus sprint, eskalasi ke Level 3 atau realokasi budget ke `<kanal cadangan>`.
```

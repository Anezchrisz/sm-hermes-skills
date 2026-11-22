---
name: sunmedia-orca-skill-change-request
description: Memindai Knowledge_Base dan log kegagalan QA mingguan untuk pola berulang (minimal 3 kejadian serupa), menyusun draft Skill Change Request berversi/bertanggal. TIDAK pernah menulis langsung ke SKILL.md manapun; aktif hanya setelah approval manual PM/Yohanes.
---

# Skill Governance — Orca

## Prinsip
`SKILL.md` bersifat read-only bagi semua agent, termasuk Orca sendiri. Pembelajaran
lapangan hanya boleh masuk lewat Change Request berversi/bertanggal yang di-approve
manual PM/Yohanes — tidak ada auto-update.

## Sumber data
- Log kegagalan QA dari `sunmedia-orca-deterministic-qa` (field apa yang paling sering
  FAIL, agent mana, kriteria apa).
- Koreksi PM di Gate 1/1.5/2 (dari `sunmedia-orca-gate-workflow` — alasan penolakan yang
  berulang).
- `Knowledge_Base` entri dari Nara/Noya (pola industri/ICP/akurasi — bukan wewenang Orca
  untuk menilai isinya, hanya memindai apakah ada pola operasional di baliknya).

## Ambang: minimal 3 kejadian serupa

1. Kumpulkan kejadian (kegagalan QA / koreksi PM) selama satu minggu.
2. Kelompokkan berdasarkan kemiripan pola (field yang sama, jenis kesalahan yang sama,
   bisa lintas klien).
3. Hitung jumlah kejadian per kelompok. **Kelompok dengan <3 kejadian TIDAK diajukan**
   sebagai Change Request — catat saja di memory untuk dipantau minggu berikutnya.
4. Kelompok dengan ≥3 kejadian → susun draft Change Request.

## Format draft Skill Change Request

```
SKILL CHANGE REQUEST — <nomor> — <tanggal>
Skill target: <nama skill, misal sunmedia-noya-pitch-estimation>
Pola yang ditemukan: <deskripsi pola, jumlah kejadian, klien-klien terkait>
Usulan perubahan: <perubahan konkret yang diusulkan ke SKILL.md terkait>
Status: MENUNGGU_APPROVAL_PM
```

Simpan sebagai dokumen bertanggal/berversi, terpisah dari `Knowledge_Base` sampai
disetujui.

## Setelah approval
PM/Yohanes men-approve manual → **Yohanes atau PM sendiri (bukan Orca) yang mengedit**
`SKILL.md` terkait dan commit ke repo `sunmedia-hermes-skills`. Orca hanya mengubah
status draft menjadi `DISETUJUI` dan mengarsipkannya — tidak melakukan commit atau edit
`SKILL.md` apa pun, bahkan setelah disetujui.

## Yang TIDAK boleh dilakukan
- Menulis atau mengedit `SKILL.md` manapun, kapan pun, dengan alasan apa pun.
- Mengajukan Change Request untuk pola dengan <3 kejadian.
- Menggabungkan beberapa pola yang tidak benar-benar serupa hanya untuk mencapai ambang 3.

## Checklist
- [ ] Ambang ≥3 kejadian serupa benar-benar terpenuhi sebelum draft diajukan.
- [ ] Draft menyebut skill target yang spesifik, bukan "perbaiki secara umum".
- [ ] Tidak ada perubahan langsung ke `SKILL.md` mana pun oleh Orca.
- [ ] Status draft terupdate (`MENUNGGU_APPROVAL_PM` → `DISETUJUI`/`DITOLAK`) sesuai keputusan PM/Yohanes.

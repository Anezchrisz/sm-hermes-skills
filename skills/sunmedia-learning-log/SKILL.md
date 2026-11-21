---
name: sunmedia-learning-log
description: Catat pembelajaran Nara dari koreksi PM, revisi asumsi, atau hasil aktual (Fase 4) ke learning log, dan usulkan pembaruan skill dengan penandaan jelas vs perlu review. Gunakan saat PM mengoreksi brief atau memberi hasil aktual.
version: 1.0.0
metadata:
  hermes:
    tags: [sunmedia, learning, nara]
    category: sunmedia
---

# Learning Log

## When to Use
Saat: (a) PM mengoreksi atau menolak bagian brief di Approval Gate 1, (b) komunikasi intake mengubah asumsi, (c) hasil aktual Fase 4 berbeda dari asumsi brief, (d) alasan menang/kalah pitching diberikan.

## Procedure
1. Tulis entri ringkas: `date`, `client_id`, `industry`, `assumption` (apa yang Nara asumsikan), `correction_or_actual`, `lesson` (satu kalimat yang bisa dipakai ulang).
2. Klasifikasi:
   - Jelas dan tidak kontroversial (mis. sumber data tertentu ternyata tidak akurat) -> `auto_applied`: perbarui skill/memory terkait, beri tanggal di catatan perubahan, dan beri tahu PM apa yang baru dipelajari.
   - Ambigu atau berdampak besar (mis. bertentangan dengan skill yang ada) -> `needs_review`: **jangan** ubah skill; tandai untuk review PM.
3. Catat ke tab `Learning_Log` lewat `sunmedia-tracker-sheet`. Tag industri selalu diisi agar berguna lintas klien.
4. Setiap perubahan skill harus mencantumkan tanggal perubahan dan alasannya.

## Pitfalls
- Jangan menyimpulkan pola dari satu klien sebagai aturan umum; tandai "1 kasus".
- Jangan menyimpan data pribadi atau kredensial di learning log.

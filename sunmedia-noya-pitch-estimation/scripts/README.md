# Scripts Folder (`noya-pitch-estimation`)

Skill `cmo-advisor` **sudah terpasang dan terverifikasi** (hash SHA-256 dicocokkan byte-per-byte terhadap upstream `alirezarezvani/claude-skills`) di lokasi:
`/opt/data/profiles/noya/skills/marketing/cmo-advisor/scripts/`

Salin **kedua file itu apa adanya** (jangan diekstrak ulang dari GitHub, jangan diedit) ke folder ini:
1. `marketing_budget_modeler.py`
2. `growth_model_simulator.py`

Setelah disalin, verifikasi ulang kecocokan dengan cmo-advisor menggunakan `sha256sum` pada kedua lokasi — hash harus identik. Ingat: nilai default di kedua script adalah data sampel upstream, bukan data Sun Media — selalu edit bagian `INPUTS` dengan angka Master_Brief sebelum dijalankan (lihat `SKILL.md` bagian 4.4).

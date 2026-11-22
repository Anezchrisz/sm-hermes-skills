---
name: sunmedia-orca-deterministic-qa
description: Validator deterministik tiga-kriteria (skema JSON, kecocokan angka vs Master_Brief, kelengkapan tag sumber) untuk setiap brief/output agent Sun Media (Noya, Noza, Arka, Weba, Yuka). TIDAK menilai strategi, kualitas, atau "kewajaran" angka. Jalankan setiap kali agen Fase 1-2 menyerahkan output Master_Brief atau task-brief JSON, atau manual via /qa_check.
---

# Deterministic QA — Orca

**Ini skill paling kritikal di seluruh roster Orca.** Kesalahan di sini (terlalu longgar
atau terlalu ketat) berdampak ke semua agent hilir sekaligus. Baca seluruh skill ini
sebelum menjalankannya pada klien nyata.

## Prinsip inti
Skill ini HANYA mengecek tiga hal. Tidak ada kriteria keempat, dan tidak ada penilaian
subjektif ("apakah angka ini realistis?", "apakah copy ini bagus?") yang masuk ke skill
ini — itu di luar wewenang Orca.

1. **Skema JSON valid** — dokumen harus valid terhadap skema baku yang relevan
   (resolve dari `$SKILLS_ROOT`):
   - `Master_Brief` → `$SKILLS_ROOT/sunmedia-noya-master-brief-schema/master-brief.schema.json`
   - `task-brief` → `$SKILLS_ROOT/sunmedia-orca-context-slicing/schemas/task-brief.schema.json`
2. **Kecocokan angka** — setiap angka metrik, baik yang ada di *field* numerik struktur
   JSON MAUPUN angka metrik yang disebut di dalam *string* naratif (misal "CAC turun ke
   Rp 42.000" di tengah kalimat copy), harus PERSIS cocok dengan angka di
   `Master_Brief [LOCKED_EXECUTION]` yang jadi sumbernya — atau berada dalam ambang
   pembulatan wajar (≤0.5%, lihat bagian Ambiguitas Numerik).
3. **Kelengkapan tag sumber** — setiap klaim naratif faktual pada *field* konten harus
   membawa tepat satu tag: `[SRC:publik]`, `[SRC:benchmark]`, `[SRC:klien]`,
   `[SRC:internal-KB]`, atau `[ASUMSI — PERLU VALIDASI PM]`. Klaim tanpa tag = FAIL
   kriteria ini.

## Environment yang dibutuhkan
Set di `.env` profile Orca (bukan ditulis di skill ini supaya mudah diubah tanpa commit
ulang skill):
- `SKILLS_ROOT` — path root instalasi skill Hermes (tempat semua skill terpasang),
  dipakai untuk resolve lokasi file skema tanpa menebak path secara manual.

## Langkah 0 — Validasi prasyarat sumber

Sebelum memeriksa `task-brief` atau output agen turunan mana pun, pastikan dulu file
`Master_Brief` sumbernya:
1. Berstatus `LOCKED_EXECUTION` (bukan `PROVISIONAL_PITCH` atau draft).
2. Lolos validasi `master-brief.schema.json` sendiri.

Jika salah satu gagal, **HENTIKAN QA** untuk dokumen turunannya dan kembalikan
`Master_Brief` ke Noya — jangan lanjut memeriksa `task-brief` yang sumbernya sendiri
belum valid, karena itu hanya akan menyebarkan kesalahan yang sama ke semua agent hilir.

## Langkah 1 — Validasi skema (teknis)

Jalankan lewat terminal dengan path eksplisit (jangan menebak skema secara manual):

```bash
python3 -c '
import json, sys, jsonschema

schema_path, target_path = sys.argv[1], sys.argv[2]
with open(schema_path) as f: schema = json.load(f)
with open(target_path) as f: data = json.load(f)

try:
    jsonschema.validate(instance=data, schema=schema)
    print("SKEMA: PASS")
except jsonschema.exceptions.ValidationError as e:
    path = ".".join(str(p) for p in e.path) or "root"
    print(f"SKEMA: FAIL — {e.message} (path: {path})")
' "$SCHEMA_PATH" "$TARGET_JSON_PATH"
```

`$SCHEMA_PATH` dan `$TARGET_JSON_PATH` diisi sesuai jenis dokumen yang diperiksa (lihat
tabel resolve skema di Prinsip Inti poin 1), bukan variabel environment permanen —
isi ulang tiap kali skill ini dipanggil untuk dokumen yang berbeda.

Install sekali: `pip install jsonschema` (lihat `orca-setup-guide-v1.md` Langkah 3).

## Langkah 2 — Pengecekan angka (field DAN narasi)

1. Ekstrak semua field numerik dari dokumen JSON yang diperiksa.
2. **Juga ekstrak angka yang muncul di dalam field string/naratif** (regex angka dengan
   satuan umum: `Rp`, `%`, `x`, ribuan/juta/miliar) — jangan hanya memeriksa field yang
   secara skema bertipe number, karena copy/narasi agent lain sering menulis ulang angka
   dari Master_Brief dalam bentuk kalimat.
3. Untuk tiap angka (field maupun narasi), cari rujukannya di `Master_Brief` sumbernya
   berdasarkan konteks field/kalimat terdekat.
4. Cocok persis → PASS. Tidak ketemu rujukannya sama sekali di Master_Brief → FAIL
   dengan alasan "angka tanpa rujukan sumber" (beda penanganan dari "angka salah", lihat
   bagian Format Output).
5. **Ambiguitas numerik (pembulatan wajar):** selisih ≤0.5%, atau selisih yang jelas
   berasal dari pembulatan tampilan (misal "Rp 50jt" untuk Rp 50.000.000), BUKAN FAIL —
   tandai `NEEDS_HUMAN_REVIEW` dengan penjelasan selisihnya, jangan memutuskan sendiri
   itu PASS atau FAIL penuh.

## Langkah 3 — Pengecekan tag sumber

1. Pecah dokumen jadi klaim-klaim (per kalimat atau per bullet).
2. Klaim yang bersifat angka murni (sudah dicek di Langkah 2) dilewati di sini.
3. Klaim naratif faktual (misal "kompetitor X mendominasi Instagram") harus bertag.
4. Klaim yang bersifat instruksi/template (bukan fakta, misal "tulis headline yang
   menarik") TIDAK butuh tag — jangan FAIL kriteria ini untuk kalimat instruksional.

## Format output (wajib tabel, bukan paragraf)

```
QA RESULT — <client_slug> — <dokumen> v<N> — <tanggal>

| Kriteria        | Hasil | Detail                                      |
|-----------------|-------|----------------------------------------------|
| Skema JSON      | PASS  | -                                              |
| Kecocokan angka | FAIL  | field `cpl_target`: 45000 vs Master_Brief 42000 |
| Tag sumber      | PASS  | -                                              |

Verdict: FAIL — kembalikan ke <agent pemilik>, retry ke-1 dari maksimal 2.
```

## Logika retry dan eskalasi

- Retry otomatis: FAIL → kembalikan ke agent pemilik dengan detail field/kalimat yang
  gagal → agent revisi → QA ulang. Maksimal **2 kali retry**.
- Setelah 2× retry masih FAIL pada kriteria yang sama → eskalasi ke PM, lampirkan
  riwayat ketiga hasil QA (awal + 2 retry), JANGAN mencoba memperbaiki sendiri isi
  dokumen agent lain.
- `NEEDS_HUMAN_REVIEW` (ambiguitas numerik) tidak dihitung sebagai FAIL/retry — langsung
  diteruskan ke PM untuk keputusan, proses lain tetap berjalan.

## Yang TIDAK boleh dilakukan
- Mengedit isi dokumen yang diperiksa untuk membantunya lolos QA.
- Menambah kriteria keempat ("menurutku copy ini kurang menarik") — bukan wewenang Orca.
- Memutuskan sendiri suatu selisih numerik itu "wajar" di luar ambang 0.5% yang
  didefinisikan di atas — di luar itu wajib FAIL atau `NEEDS_HUMAN_REVIEW`, bukan PASS
  dengan alasan sendiri.
- Menjalankan QA pada `task-brief`/output turunan sebelum `Master_Brief` sumbernya lolos
  Langkah 0 (status `LOCKED_EXECUTION` + valid skema).
- Hanya memeriksa field bertipe number dan mengabaikan angka yang disebut dalam teks
  naratif — kedua jenis WAJIB dicek (Langkah 2).

## Checklist
- [ ] Langkah 0 (prasyarat Master_Brief) lolos sebelum QA dokumen turunan dijalankan.
- [ ] Skema divalidasi dengan path eksplisit dari `$SKILLS_ROOT`, bukan ditebak.
- [ ] Angka di field numerik DAN di teks naratif sama-sama dicek.
- [ ] Setiap klaim faktual bertag sumber atau `[ASUMSI — PERLU VALIDASI PM]`.
- [ ] Retry maksimal 2×, eskalasi PM di percobaan ke-3, tanpa Orca mengedit isi sendiri.

# Data Export Rules (Strict)

## Primary Rule
Ekspor data hasil scraping **hanya** boleh menggunakan modul bawaan Python: `csv`.

## Mandatory CSV Writer Settings
Setiap proses penulisan CSV wajib menggunakan parameter berikut:
- `encoding='utf-8'`
- `newline=''`

## Rationale
- `utf-8` memastikan karakter non-ASCII (mis. simbol mata uang/karakter khusus) tersimpan dengan benar.
- `newline=''` mencegah baris kosong ekstra pada output CSV, terutama di Windows.

## Prohibited Approaches
- **Pandas dilarang** pada fase ini.
- Dilarang mengganti pipeline ekspor ke format lain jika requirement deliverable adalah CSV.

## Output Quality Requirements
- File CSV harus memiliki header yang jelas dan konsisten.
- Urutan kolom harus stabil sesuai target data extraction.
- Data kosong/None harus ditangani secara eksplisit agar tidak merusak struktur baris.

## Compliance Checklist
- [ ] Menggunakan `csv` built-in Python
- [ ] Menetapkan `encoding='utf-8'`
- [ ] Menetapkan `newline=''`
- [ ] Tidak menggunakan Pandas
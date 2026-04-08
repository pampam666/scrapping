# Technical Context (Strict Stack & Constraints)

## Mandatory Technology Stack
Proyek ini **harus** mengikuti stack berikut sesuai PRD:

1. **Environment**: Jupyter Notebook (`.ipynb`)
2. **Language**: Python 3.8+
3. **Core Scraping Library**: `scrapling` (D4Vinci/Scrapling)
4. **Browser Automation Layer**: `playwright` (dependensi penting untuk browser control)
5. **Data Export Module**: Python built-in `csv`

## Hard Requirements (Non-Negotiable)
- Browser wajib berjalan di mode visual menggunakan `StealthyFetcher` dengan `headless=False`.
- Alur autentikasi bersifat semi-manual: script harus pause memakai `input()` agar user login sendiri.
- Setelah user melanjutkan, disarankan jeda render DOM (`time.sleep(3)` sampai `5`) sebelum parsing.
- Strategi ekstraksi wajib tahan terhadap perubahan class dinamis (gunakan XPath/CSS berbasis struktur/teks).
- Error handling wajib memakai `try-except` agar script tidak langsung crash ketika struktur target berubah sebagian.

## Data Export Constraints
- Hanya boleh menggunakan modul bawaan `csv`.
- Penulisan file CSV harus menyertakan:
  - `encoding='utf-8'`
  - `newline=''`
- Tujuan: kompatibilitas karakter khusus (UTF-8) dan mencegah baris kosong ekstra (khususnya di Windows).

## Explicit Prohibitions
- **Dilarang menggunakan Pandas** dalam iterasi proyek ini.

## Runtime/Execution Notes
- Karena target cenderung high-security (mis. Semrush), flow harus mengakomodasi login manual dan potensi captcha.
- State browser/session perlu dijaga agar tetap aktif lintas cell notebook hingga proses ekstraksi & ekspor selesai.
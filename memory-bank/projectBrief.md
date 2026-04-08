# Project Brief: Authenticated Web Scraper to CSV via Jupyter Notebook

## Project Goal
Membangun workspace untuk proyek scraping interaktif berbasis **Jupyter Notebook** yang mampu mengambil data dari situs dengan proteksi autentikasi/anti-bot tinggi (contoh: Semrush), lalu mengekspor hasilnya ke file **CSV**.

Pendekatan yang diwajibkan adalah **semi-automated**:
- Browser dibuka dalam mode visual (bukan headless).
- Pengguna melakukan login manual dan menangani captcha bila diperlukan.
- Notebook menunggu konfirmasi pengguna melalui `input()` sebelum lanjut ke ekstraksi data.

## Architectural Direction (PRD-Aligned)
Arsitektur notebook harus modular per cell agar alur mudah dikontrol:
1. **Environment Setup** – instalasi dependency (`scrapling`, `playwright`).
2. **Imports & Initialization** – inisialisasi modul/fungsi bantu.
3. **Browser Launch & Authentication** – jalankan browser visual dengan `StealthyFetcher` dan pause untuk login manual.
4. **Extraction Phase** – parsing halaman setelah login, fokus ke selector robust.
5. **CSV Export & Cleanup** – simpan data via built-in `csv`, lalu tutup resource browser.

## Key Technical Principles
- Wajib menggunakan `StealthyFetcher` dengan `headless=False`.
- Ekstraksi elemen harus memprioritaskan **XPath/CSS berbasis struktur/teks**, bukan class dinamis acak.
- Wajib ada `try-except` untuk graceful degradation saat struktur DOM berubah.
- Export data hanya menggunakan modul built-in `csv` dengan:
  - `encoding='utf-8'`
  - `newline=''`

## Explicit Constraints
- **Tidak boleh menggunakan Pandas** pada iterasi ini.
- Setelah user menekan ENTER pasca login, perlu jeda render DOM (`time.sleep(3)` hingga `5` detik) sebelum ekstraksi.
- Sesi browser perlu dikelola agar tidak terputus antar cell notebook.

## Definition of Done (High-Level)
- Notebook berjalan lokal tanpa error.
- Browser visual muncul (non-headless).
- Login manual berhasil dilakukan tanpa gangguan script.
- Setelah ENTER, data dari halaman authenticated berhasil diekstrak.
- File CSV hasil scraping terbentuk rapi, UTF-8, tanpa baris kosong berlebih.

## Final Workspace Structure (Approved)
- `/.agents/` — AI Skills
- `/.clinerules/` — Project Rules
- `/memory-bank/` — AI Context
- `/data/` — CSV Outputs (`hasil_scraping.csv` target location)
- `semrush_scraper.ipynb` — Main script notebook (starter scaffold)
- `requirements.txt` — Python dependencies (`jupyter`, `scrapling`, `playwright`)
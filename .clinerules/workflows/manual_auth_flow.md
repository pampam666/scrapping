# Workflow: Manual Authentication Flow (Semi-Automated)

## Scope
Workflow wajib untuk target scraping dengan proteksi autentikasi/anti-bot tinggi, sesuai PRD proyek Jupyter + Scrapling + Playwright.

## Exact Execution Workflow
1. **Launch visual browser session**
   - Inisialisasi sesi menggunakan `StealthyFetcher`.
   - Pastikan browser dijalankan dengan `headless=False`.

2. **Navigate to target entry URL**
   - Buka halaman login/entry point target (mis. Semrush).

3. **Pause script for manual auth**
   - Cetak instruksi jelas di notebook:
     - User harus login manual pada browser yang terbuka.
     - Selesaikan captcha/2FA jika muncul.
     - Tunggu sampai halaman target (dashboard/data table) benar-benar tampil.
   - Hentikan eksekusi dengan `input()`.

4. **User confirmation gate**
   - Instruksi eksplisit: **"Tekan ENTER di notebook setelah login berhasil dan halaman data termuat."**

5. **Post-auth stabilization**
   - Setelah ENTER, tunggu 3–5 detik untuk memastikan DOM final telah dirender.

6. **Proceed to extraction phase**
   - Lanjutkan parsing menggunakan selector robust berbasis struktur/teks.
   - Terapkan `try-except` untuk menjaga workflow tetap resilient.

## Compliance Notes
- Flow ini **tidak boleh** diganti menjadi login fully-automated.
- Session browser harus dipertahankan selama notebook berjalan antar-cell hingga ekstraksi dan ekspor selesai.
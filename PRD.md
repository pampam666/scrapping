# Product Requirements Document (PRD)
**Project Name:** Authenticated Web Scraper to CSV via Jupyter Notebook
**Target Platform:** Jupyter Notebook (.ipynb)
**Primary Target Website:** Semrush (or similar high-security websites)

---

## 1. Project Overview
Proyek ini bertujuan untuk mengembangkan skrip web scraping interaktif berbasis Jupyter Notebook yang mampu mengekstrak data dari halaman web yang dilindungi oleh sistem autentikasi dan *anti-bot* yang ketat. Hasil akhir dari ekstraksi data harus disimpan ke dalam format file `.csv`. 

Karena ketatnya keamanan target, pendekatan *fully-automated login* tidak digunakan. Sebagai gantinya, sistem akan menggunakan pendekatan *semi-automated* di mana skrip akan membuka browser, menunda eksekusi untuk memungkinkan *user* melakukan login manual dan menyelesaikan Captcha, sebelum akhirnya melanjutkan proses ekstraksi DOM.

## 2. Technology Stack
Agen AI (Cline.bot) diwajibkan menggunakan tumpukan teknologi berikut:
* **Environment:** Jupyter Notebook (`.ipynb`)
* **Language:** Python 3.8+
* **Core Scraping Library:** `scrapling` (GitHub: D4Vinci/Scrapling)
* **Browser Automation:** `playwright` (sebagai dependensi dari Scrapling)
* **Data Export:** `csv` (Python built-in module)

## 3. Functional Requirements

### 3.1. Notebook Structure (Cell Layout)
Jupyter Notebook harus dibagi menjadi beberapa sel (*cells*) yang logis agar pengguna dapat mengontrol alur eksekusi:
* **Cell 1: Environment Setup.** Menginstal dependensi (`!pip install scrapling`, `!playwright install`).
* **Cell 2: Module Imports & Initialization.** Mengimpor pustaka dan mendefinisikan fungsi utilitas.
* **Cell 3: Browser Launch & Authentication Phase.** Membuka browser dalam mode visual dan menahan eksekusi menunggu input pengguna.
* **Cell 4: Data Extraction Phase.** Melakukan *parsing* HTML (menggunakan XPath/CSS) setelah pengguna mengonfirmasi status login.
* **Cell 5: CSV Export & Cleanup.** Menyimpan data ke disk dan menutup *instance* browser.

### 3.2. Browser & Stealth Configuration
* Wajib menggunakan kelas `StealthyFetcher` dari pustaka `scrapling`.
* Parameter browser **wajib** disetel ke mode visual: `headless=False`. Hal ini krusial agar pengguna dapat melihat antarmuka web dan melakukan login.

### 3.3. Manual Authentication Handling
* Sistem harus melakukan navigasi ke URL target awal.
* Gunakan fungsi `input()` pada Python untuk menghentikan sementara (pause) eksekusi sel. Skrip harus mencetak instruksi yang jelas ke layar yang meminta pengguna untuk login secara manual di browser yang terbuka, dan menekan `ENTER` di notebook setelah halaman data (dashboard) termuat sepenuhnya.

### 3.4. Data Extraction & Resilience
* Sistem harus menangani *dynamic class names* (misal: CSS *modules* / *styled-components* yang menghasilkan *class* acak seperti `sc-abc kXyZ`). 
* Diutamakan menggunakan **XPath** yang mencari elemen berdasarkan struktur atau teks (contoh: `//div[contains(text(), 'Target')]`), bukan bergantung pada nama *class* yang bisa berubah sewaktu-waktu.
* Sistem harus menyertakan blok `try-except` untuk mencegah skrip *crash* (*graceful degradation*) jika struktur tabel web berubah sebagian.

### 3.5. Data Export (CSV)
* Data hasil ekstrak harus disimpan menggunakan modul `csv` bawaan Python.
* Pastikan file diatur dengan parameter `encoding='utf-8'` dan `newline=''` untuk menghindari karakter aneh (misalnya pada simbol mata uang atau *intent keyword*) dan mencegah baris kosong berlebih di sistem operasi Windows.
* Struktur Data (Contoh Target Data Tabel):
    * Kolom 1: Keyword
    * Kolom 2: Position
    * Kolom 3: Volume
    * Kolom 4: KD (Keyword Difficulty)

## 4. Non-Functional Requirements
* **State Management:** Pastikan *instance* `StealthyFetcher` dideklarasikan secara global atau dikelola sedemikian rupa agar sesi browser tidak tertutup saat berpindah antar-sel (*cell*) di Jupyter Notebook.
* **Rate Limiting:** Jika skrip dirancang untuk mengunjungi banyak halaman (paginasi), wajib menyertakan jeda waktu acak (`time.sleep()`) antar *request* untuk menghindari blokir dari *anti-bot*.

## 5. Acceptance Criteria (Definition of Done)
1.  Jupyter Notebook berhasil dijalankan tanpa error di lingkungan lokal.
2.  Sel eksekusi berhasil memunculkan Chromium browser (bukan headless).
3.  Pengguna berhasil melakukan login manual tanpa diganggu oleh skrip.
4.  Setelah pengguna menekan ENTER, skrip berhasil mengekstrak *node* HTML dari halaman yang dilindungi *auth*.
5.  File `hasil_scraping.csv` terbuat di direktori lokal dengan format *header* dan baris data yang rapi dan benar (mendukung karakter UTF-8).

## 6. Developer Notes for Cline.bot
* *Do not use Pandas for this iteration.* Keep dependencies lightweight by using the built-in `csv` module.
* Target web uses React.js. Ensure you add `time.sleep(3)` to `5` seconds after the user presses ENTER to allow the DOM to fully render the final state before extracting the page source with Scrapling.
* Assume the user is targeting a table component. Provide a robust, generic XPath selector example in Cell 4 that the user can easily swap out with their specific target.

***
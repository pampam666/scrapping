# Scraping Rules (Resilience-First)

## Selector Strategy (Mandatory)
Untuk website dengan class dinamis (React/CSS modules/styled-components), selector harus memprioritaskan:
1. XPath/CSS berbasis **struktur DOM**
2. XPath/CSS berbasis **teks/label stabil**

### Strictly Discouraged
- Jangan mengandalkan class name acak/dinamis sebagai selector utama.
- Hindari selector rapuh yang bergantung pada urutan elemen yang mudah berubah.

## Error Handling Requirement
Setiap tahap ekstraksi penting wajib dibungkus `try-except` agar:
- script tidak crash total saat sebagian elemen gagal ditemukan,
- proses tetap menghasilkan output parsial yang dapat dipakai,
- kegagalan dapat dilaporkan secara jelas (graceful degradation).

## Manual Auth Compatibility
- Scraping baru boleh berjalan setelah gerbang autentikasi manual (`input()`) dilewati.
- Setelah ENTER dari user, wajib beri waktu rendering 3–5 detik sebelum parsing halaman.

## Robustness Principles
- Gunakan fallback selector jika selector utama gagal.
- Validasi keberadaan node sebelum akses atribut/teks.
- Logging status ekstraksi per bagian untuk memudahkan debugging saat layout berubah.

## Compliance Checklist
- [ ] Selector berbasis struktur/teks digunakan
- [ ] Tidak bergantung pada class dinamis sebagai strategi utama
- [ ] Blok `try-except` diterapkan pada titik rawan gagal
- [ ] Flow kompatibel dengan autentikasi manual + jeda render DOM
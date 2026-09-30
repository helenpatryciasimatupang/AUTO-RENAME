# FAT–Pole Photo Automation

Web statis untuk mengotomatisasi pencocokan FAT pada file KMZ dengan foto pole, memilih foto full view pertama, mengganti nama foto berdasarkan `FAT ID Network`, lalu mengunduh seluruh hasil sebagai `HASIL_POLE.zip`.

## Cara pakai

1. Buka website.
2. Klik **Pilih KMZ** dan pilih file `.kmz` yang memuat field `FAT ID Network` dan `Pole ID (New)`.
3. Masukkan foto dengan salah satu cara:
   - **Upload ZIP Foto**, atau
   - **Pilih Folder Foto** (Chrome/Edge yang mendukung pemilihan folder).
4. Setelah KMZ dan foto selesai dibaca, klik **PROSES SEMUA FOTO**.
5. Periksa tabel hasil. Foto dipasangkan berdasarkan token Pole ID dalam tanda kurung, misalnya `(2A)`.
6. Jika satu Pole ID memiliki beberapa foto, kandidat diurutkan secara natural berdasarkan nama file dan foto pertama dipilih sebagai full view.
7. Klik **Download Hasil ZIP**. Hasil berupa:

```text
HASIL_POLE.zip
└── POLE/
    ├── POLE FCJK13D02S01A01.jpg
    ├── POLE FCJK13D02S01A02.jpg
    └── ...
```

8. Klik **Bersihkan Data** untuk menghapus referensi file dari sesi browser.

## Privasi

Semua KMZ, ZIP, foto, mapping, dan hasil diproses lokal di browser. Aplikasi tidak memiliki backend, database, upload API, Firebase, Supabase, cloud storage, atau penyimpanan file pengguna melalui `localStorage`/IndexedDB.

GitHub Pages hanya menyajikan file aplikasi statis. Jangan commit KMZ/foto asli pengguna ke repository.

## Aturan pencocokan

- FAT authoritative: `FAT ID Network`.
- Pole authoritative: `Pole ID (New)`.
- `2A` hanya cocok dengan token seperti `(2A)` atau `( 2a )`.
- `(12A)` dan `_2A` tidak dianggap cocok untuk Pole ID `2A`.
- Foto pertama = kandidat pertama setelah natural ascending filename sort; jika nama sama, `inputIndex` paling awal dipilih.
- Format gambar: JPG, JPEG, PNG, WEBP.
- Exact duplicate mapping FAT/Pole diringkas.
- FAT yang memiliki lebih dari satu Pole ID non-kosong diberi status `CONFLICT` dan tidak diekspor otomatis.

## Menjalankan lokal

Tidak ada build step.

```bash
python -m http.server 8000
```

Buka `http://localhost:8000/`.

## Publish ke GitHub Pages

1. Buat repository GitHub, misalnya `fat-pole-photo-automation`.
2. Upload/push isi folder project ini ke branch yang akan dipublish.
3. Di GitHub buka **Settings → Pages**.
4. Pada **Build and deployment**, pilih **Deploy from a branch**.
5. Pilih branch repository dan folder `/ (root)`.
6. Simpan. Website akan tersedia pada pola URL:

```text
https://<username>.github.io/fat-pole-photo-automation/
```

Semua asset menggunakan URL relatif agar aman ketika website berada di subpath GitHub Pages.

## Browser

Target utama: Google Chrome/Chromium terbaru pada Windows. Microsoft Edge terbaru juga didukung. Jika directory picker tidak tersedia, gunakan upload ZIP foto.

## Struktur project

```text
index.html
css/style.css
js/app.js
js/kmz-parser.js
js/photo-loader.js
js/photo-matcher.js
js/exporter.js
js/utils.js
lib/jszip.min.js
```

`lib/jszip.min.js` adalah JSZip 3.10.1 yang disimpan lokal sehingga runtime tidak memerlukan CDN.

## Pengujian

Automated unit/controller tests:

```bash
npm test
```

Synthetic browser acceptance test tersedia di `tests/browser-e2e.html` dan menggunakan data sintetis saja.

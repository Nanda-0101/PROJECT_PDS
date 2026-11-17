# Prakiraan Kunjungan Wisatawan Mancanegara ke Indonesia

**Exponential Smoothing (SES, Holt, Holt-Winters) dan Regresi Linear**

UAS Pemodelan dan Simulasi, Program Studi Informatika, FMIPA, Universitas Udayana (2026).

Proyek ini memprakirakan jumlah kunjungan wisatawan mancanegara (wisman) bulanan ke Indonesia, lalu membandingkan akurasi lima model: SES, Holt, Holt-Winters multiplikatif, Holt-Winters aditif, dan regresi linear.

## Ringkasan Hasil

Data: Total kunjungan bulanan, Januari 2023 – September 2026 (45 bulan). Data latih 36 bulan (2023–2025), data uji 9 bulan (Januari–September 2026).

| Metode | MAPE (%) | RMSE | MAE |
| --- | --- | --- | --- |
| **Holt-Winters Multiplikatif** | **3,08** | 49.776 | 40.800 |
| Holt-Winters Aditif | 3,73 | 57.303 | 47.511 |
| SES | 11,32 | 173.397 | 141.625 |
| Regresi Linear (tren) | 12,77 | 184.005 | 156.474 |
| Holt (tren) | 13,00 | 187.513 | 159.246 |

- Kunjungan memiliki **tren naik** (sekitar 14.406 kunjungan per bulan menurut regresi linear, R² = 0,63) dan **pola musiman** yang jelas: puncak Juli–Agustus dan Desember, terendah Januari–Maret.
- **Holt-Winters multiplikatif** adalah model terbaik. Model tanpa komponen musiman (SES, Holt, regresi) meleset pada bulan puncak dan bulan rendah.
- Prakiraan Oktober 2026 – September 2027 menempatkan puncak pada Desember 2026 dan Juli–Agustus 2027. Total 2026 diprakirakan sekitar 16,2 juta kunjungan, naik 5,5% dari 2025.

Keterbatasan: hanya tiga siklus musiman penuh untuk pelatihan, tidak ada variabel eksternal (kebijakan visa, harga tiket, bencana, dll.), dan prakiraan berupa titik tunggal tanpa selang kepercayaan. Pembahasan lengkap ada di laporan.

## Struktur Repositori

```
.
├── Pemodelan_Simulasi_Wisman.ipynb      # notebook utama (interaktif)
├── data/                                # 4 berkas CSV BPS (2023–2026)
│   └── *Wisatawan_Mancanegara*.csv
└── README.md
```


## Data

Sumber: Badan Pusat Statistik (BPS), *Jumlah Kunjungan Wisatawan Mancanegara per Bulan ke Indonesia Menurut Pintu Masuk*, empat berkas CSV untuk tahun 2023, 2024, 2025, dan 2026.

- Setiap berkas memuat kunjungan per bulan untuk pintu udara, laut, dan darat beserta rinciannya.
- Data 2026 tersedia sampai September; bulan yang bertanda `-` diabaikan.
- Variabel yang dimodelkan secara default adalah baris **Total**. Pintu masuk lain (mis. Ngurah Rai, Soekarno-Hatta, Batam) bisa dipilih lewat widget.

## Cara Menjalankan

### Opsi 1: Google Colab

1. Unggah `Pemodelan_Simulasi_Wisman.ipynb` ke Colab (atau buka langsung dari GitHub).
2. Jalankan **Runtime → Run all**.
3. Jika berkas CSV belum ada di folder kerja, Colab akan meminta Anda mengunggah keempat CSV (2023–2026).

### Opsi 2: Lokal (Jupyter)

```bash
git clone https://github.com/[username]/[repositori].git
cd [repositori]
pip install numpy pandas scipy matplotlib ipywidgets jupyter
jupyter notebook Pemodelan_Simulasi_Wisman.ipynb
```

Letakkan keempat CSV di folder yang sama dengan notebook, lalu jalankan semua sel dari atas ke bawah.


## Isi Notebook

| Bagian | Keterangan |
| --- | --- |
| 1. Memuat Data | Membaca keempat CSV menjadi tabel panjang (pintu, tahun, bulan, jumlah) |
| 2. Fungsi Model | SES, Holt, Holt-Winters (multiplikatif/aditif), regresi linear, MAPE/RMSE/MAE, pencarian parameter α, β, γ (meminimalkan SSE data latih) |
| 3. Mesin Simulasi | `jalankan_model`: latih, uji pada *N* bulan terakhir, latih ulang pada seluruh data, lalu prakiraan *h* bulan |
| 4. Visualisasi | Fungsi grafik |
| 5. Perbandingan Seluruh Metode | Menghasilkan Gambar 1–5 laporan, grafik MAPE, tabel akurasi dan prakiraan |
| 6. Kesimpulan Singkat | Ringkasan temuan |
| 7. Simulasi Interaktif | Widget input pengguna (sel paling bawah) |

## Fitur Interaktif

Pada bagian *Simulasi Interaktif* pengguna dapat memilih:

- **Sumber data**: dataset BPS atau **input manual** (deret bulanan dipisah koma, mis. `120,130,125,...`, dengan bulan awal `YYYY-MM`).
- **Pintu masuk** dan **tahun mulai** data.
- Satu atau beberapa **metode**.
- Jumlah **data uji** (bulan) dan **horizon** prakiraan (1–24 bulan).
- **Parameter α, β, γ**: dioptimasi otomatis atau diatur manual.

Hasil berupa grafik dan tabel muncul setelah tombol **Jalankan** ditekan. Holt-Winters membutuhkan minimal 24 data latih (2 × periode musiman 12); notebook akan menampilkan pesan jika data tidak cukup.

## Metode Singkat

- **SES**: ℓₜ = α·yₜ + (1 − α)·ℓₜ₋₁
- **Holt**: menambah komponen tren bₜ dengan konstanta β
- **Holt-Winters** (periode m = 12): menambah indeks musiman sₜ dengan konstanta γ, dalam bentuk multiplikatif dan aditif
- **Regresi linear**: ŷ = a + b·t dengan metode kuadrat terkecil
- **Akurasi**: MAPE, RMSE, dan MAE pada data uji

Penurunan rumus dan contoh perhitungan manual Holt-Winters ada di laporan (bab *Perhitungan Matematika*).

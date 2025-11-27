# Asisten Keuangan AI (n8n Workflow)

Repository ini berisi workflow otomatisasi untuk **n8n** yang mengubah bot Telegram menjadi asisten pencatat keuangan pribadi. Workflow ini menggunakan **Google Gemini AI** untuk memproses pesan teks maupun foto struk belanja, lalu menyimpannya secara rapi ke **Google Sheets**.

## Fitur Utama

* **Pencatatan Teks Natural:** Cukup ketik "Beli kopi 25rb pakai OVO", bot akan mengekstrak kategori, nominal, dan sumber dana secara otomatis.
* **Analisis Gambar (OCR):** Kirim foto struk belanja, AI akan membaca tanggal, total belanja, dan nama toko.
* **Auto-ID & Kategorisasi:**
    * Membuat ID unik otomatis (Format: `PMSK-001` untuk Pemasukan, `PGLR-001` untuk Pengeluaran).
    * Mendeteksi kategori (Makanan, Transportasi, dll) dan jenis transaksi (Pemasukan/Pengeluaran).
* **Integrasi Penuh:** Menghubungkan Telegram, Google Gemini (AI), dan Google Sheets.

## Prasyarat

Sebelum mengimpor workflow ini, pastikan Anda memiliki:

1.  **n8n:** Terinstal (Self-hosted atau Cloud).
2.  **Akun Google Cloud Platform:**
    * Aktifkan **Google Sheets API**.
    * Aktifkan **Generative Language API** (untuk Gemini).
3.  **Bot Telegram:** Buat melalui @BotFather dan dapatkan API Token.
4.  **Google Spreadsheet:** Siapkan satu file spreadsheet kosong.

## Persiapan Google Sheets

Buat sheet baru dan pastikan baris pertama (Header) memiliki nama kolom persis seperti ini (perhatikan huruf besar/kecil):

| Kolom | Deskripsi |
| :--- | :--- |
| `ID` | ID Transaksi (Otomatis) |
| `Tanggal ` | **Perhatikan ada spasi di akhir kata** |
| `Keterangan` | Detail transaksi |
| `Nominal` | Jumlah uang |
| `Kategori` | Jenis pengeluaran (Makanan, dll) |
| `Catatan` | Jenis transaksi (Pemasukan/Pengeluaran) |
| `Sumber` | Asal dana (Cash, BCA, Gopay, dll) |

## Cara Instalasi

1.  **Import Workflow:**
    * Download file `AssistenKeuangan.json` dari repo ini.
    * Buka dashboard n8n Anda.
    * Klik menu **Workflows** > **Import from File** > Pilih `AssistenKeuangan.json`.

2.  **Setup Credentials di n8n:**
    Anda perlu membuat credentials baru di n8n untuk node-node berikut:
    * **Telegram Trigger & Telegram Node:** Masukkan API Token dari BotFather.
    * **Google Gemini Chat Model:** Masukkan API Key Google Gemini (PaLM).
    * **Google Sheets Tool:** Lakukan otentikasi OAuth2 dengan akun Google Anda.

3.  **Konfigurasi Node:**
    * Buka node **"Append or update row in sheet..."**.
    * Pilih file Spreadsheet dan Sheet yang sudah Anda siapkan di bagian *Document* dan *Sheet*.

4.  **Aktifkan:**
    * Klik **Save**.
    * Geser tombol **Active** ke posisi ON.

## Cara Penggunaan

1.  Buka bot Telegram Anda.
2.  **Kirim Teks:** "Gajian bulan ini 5.000.000 masuk ke BCA".
3.  **Kirim Gambar:** Upload foto struk belanjaan Indomaret/Alfamart.
4.  Bot akan membalas konfirmasi dan data otomatis masuk ke Google Sheets.

## Struktur Workflow

* **Telegram Trigger:** Menerima pesan masuk.
* **Router (If Node):** Memisahkan pesan teks dan pesan gambar.
* **AI Agent (LangChain):** Otak utama yang memproses data menggunakan prompt sistem.
* **Gemini Vision:** Menganalisis gambar jika input berupa foto.
* **Google Sheets Node:** Menyimpan data hasil olahan ke spreadsheet.

---
**Catatan:** Pastikan n8n Anda memiliki akses internet publik (atau menggunakan Tunnel) agar Webhook Telegram berfungsi.

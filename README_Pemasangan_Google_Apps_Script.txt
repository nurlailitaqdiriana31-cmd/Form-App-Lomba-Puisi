# Aplikasi Pendaftaran Lomba Membaca Puisi Beregu
## Paket Google Apps Script + Google Sheet

### Isi paket
- `Index.html` — antarmuka pendaftaran, pratinjau cetak/PDF, WhatsApp, rekap admin.
- `Code.gs` — backend Google Apps Script untuk penyimpanan Google Sheet, login admin, validasi dan kuota.
- `README.txt` — panduan pemasangan.

### Cara memasang
1. Buat Google Sheet baru untuk data pendaftaran.
2. Dari Google Sheet, buka **Ekstensi → Apps Script**.
3. Di editor Apps Script, buat/ganti file `Code.gs` dengan isi file Code.gs pada paket ini.
4. Tambahkan file HTML bernama tepat `Index`, lalu salin isi `Index.html`.
5. Simpan proyek. Jalankan fungsi `setupInitialAdmin` satu kali dari editor:
   - Buka menu pemilih fungsi, pilih `setupInitialAdmin`.
   - Klik Run. Saat pertama kali, Apps Script tidak menampilkan dialog argumen; cara termudah adalah ubah sementara fungsi menjadi `function setupInitialAdmin(){ return setupInitialAdmin('admin','GANTI-DENGAN-KATA-SANDI-12-KARAKTER'); }` dengan mengganti nama fungsi helper sementara, atau gunakan petunjuk di bawah.
   - **PENTING:** jangan deploy sebelum akun admin disiapkan. Kata sandi minimal 12 karakter.
6. Berikan izin yang diminta Google kepada proyek.
7. Pilih **Deploy → New deployment → Web app**.
   - Execute as: **Me**
   - Who has access: pilih sesuai kebijakan sekolah/organisasi. Jika pendaftaran harus publik, pilih akses yang mengizinkan calon pendaftar membuka aplikasi.
8. Deploy, lalu buka URL Web app. Setiap kali mengubah kode, buat **Deploy → Manage deployments → Edit → New version**.

### Menyiapkan admin (cara aman dan praktis)
Karena Apps Script tidak menyediakan formulir argumen saat menjalankan fungsi, tambahkan sementara fungsi berikut di bagian bawah `Code.gs`, lalu jalankan sekali dari editor dan hapus fungsi sementara setelah berhasil:

```javascript
function initializeAdminOnce() {
  return setupInitialAdmin('admin', 'GANTI-KATA-SANDI-INi-12-KARAKTER-ATAU-LEBIH');
}
```

Gunakan kata sandi unik yang panjang. Jangan membagikan kata sandi ke peserta. Setelah berhasil, hapus `initializeAdminOnce` dari kode dan simpan ulang. Untuk mengganti kata sandi, tambahkan fungsi sementara yang memanggil `changeAdminPassword('KATA-SANDI-BARU-12-KARAKTER-ATAU-LEBIH')`, jalankan, lalu hapus fungsi sementara.

### Fitur
- Pendaftaran publik dengan validasi kolom wajib dan nomor HP.
- Google Sheet sebagai basis data utama.
- Login admin untuk rekap, pencarian, filter, ekspor CSV/JSON, pengaturan kuota, dan hapus data.
- Pemeriksaan kuota di sisi server agar tidak hanya bergantung pada tampilan.
- Pencegahan pendaftaran baru dari sekolah dengan nama yang sama.
- Cetak formulir / **Simpan sebagai PDF** melalui dialog cetak browser.
- Tautan konfirmasi WhatsApp setelah penyimpanan.
- Tema terang/gelap.

### Catatan
- PDF dihasilkan melalui fitur cetak browser (pilih **Save as PDF**), bukan file PDF otomatis di Google Drive.
- WhatsApp membuka aplikasi/web WhatsApp dengan teks yang telah diisi; pengguna tetap menekan tombol Kirim sendiri.
- Token login disimpan sementara di sesi browser dan kedaluwarsa maksimal enam jam. Sesi admin dilindungi di server menggunakan cache.
- Jangan menyimpan data sensitif yang tidak diperlukan. Batasi akses web app sesuai kebutuhan sekolah.
- Kuota awal adalah 100 regu. Admin dapat mengubahnya dari halaman rekap.
- Untuk perubahan struktur data atau integrasi baru, uji dulu di salinan spreadsheet.

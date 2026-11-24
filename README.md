# QRIS Dynamic Generator

HTML tunggal untuk mengubah **QRIS statis** menjadi **QRIS dinamis** dengan nominal + kode unik 3 digit.

## Fitur
- Parsing payload QRIS (TLV) + tabel tag
- Validasi CRC payload asli (CRC-16/CCITT-FALSE)
- Ubah tag `01` menjadi `12` (Point of Initiation Method = dinamis)
- Nominal = nominal dasar + kode unik 3 digit, ditulis di tag `54`
- Hitung ulang CRC tag `63`
- Render QR code + upload gambar QR sebagai input
- Deteksi QR bertingkat: `BarcodeDetector` native → jsQR (rotasi 0/90/180/270°, skala 1x/2x, inversi warna) → ZXing (`TRY_HARDER`, 8 sudut)
- **Crop manual**: geser mouse di gambar untuk memilih area QR jika deteksi otomatis gagal (berguna saat QR dikelilingi logo merchant, teks bank, dan dekorasi lain)
- QRIS merchant bisa ditanam permanen lewat `CONFIG.MERCHANT_QRIS`

## Cara pakai
1. Buka `index.html` di browser (atau buka halaman GitHub Pages).
2. Tempel payload QRIS statis / unggah gambar QR. Gambar dengan logo/teks di sekitar QR didukung; jika gagal, geser mouse untuk memilih area QR lalu klik **Decode Area Terpilih**.
3. Isi nominal dasar, atur kode unik (atau klik **Acak**).
4. Klik **Buat QRIS Dinamis**, lalu salin payload / gunakan QR-nya.

Contoh: nominal `25000` + kode `735` → tag `54` = `25735`.

## Menanam QRIS merchant
Edit bagian atas blok `<script>` di `index.html`:

```js
const CONFIG = {
  MERCHANT_QRIS: '0002010102...',   // payload QRIS statis merchant
  MERCHANT_NAME: 'Nama Toko'
};
```

## Catatan
- Library QR (qrcode.js) dan pembaca QR (jsQR) dimuat dari CDN, jadi memerlukan internet.
- Alat ini hanya memformat payload sesuai spesifikasi QRIS. Penerimaan QR dinamis tetap bergantung pada acquirer / sistem kasir merchant. Uji dulu sebelum produksi.

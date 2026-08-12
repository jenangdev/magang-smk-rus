# TES SELEKSI MAGANG SMK RADEN UMAR SAID

**Ruang lingkup:** POS (Point of Sale / Kasir) & Inventori (Gudang/Stok)

## Petunjuk Umum

Tes ini terbagi 4 tipe: **ALGORITMA**, **TESTING/PENGUJIAN**, **DESAIN DATABASE**, dan **DESAIN FRONT END**. Tiap soal berisi **Konteks** (situasi umum) dan **Deskripsi** (aturan bisnis spesifik yang jadi bahan soal).

Bisa menggunakan pseudocode (potongan kode yang dijelaskan) / contoh SQL sederhana untuk membantu menjelaskan jawaban, atau menggunakan poin-poin / diagram flowchart.

> Tes ini tidak mempresentasikan sistem POS dan Inventori Showroom secara utuh.

---

## Algoritma

### Soal 1 — Hak Akses User

**Konteks:**
Aplikasi POS/Inventori punya pengaturan hak akses yang berbeda-beda untuk tiap jenis user, supaya user hanya bisa melakukan hal yang memang jadi tugasnya.

**Deskripsi:**
User dibagi menjadi 3 role: Gudang, Pembelian, dan Kasir.

- **Role Gudang**: boleh melihat stok semua produk, boleh melihat daftar produk yang mendekati/sudah lewat tanggal kedaluwarsa (expired), tidak boleh mengubah harga jual maupun memproses transaksi kasir.
- **Role Pembelian**: boleh mencatat pembelian barang dari supplier (otomatis menambah stok), boleh melihat riwayat harga beli, tidak boleh melihat laporan penjualan atau memproses transaksi kasir.
- **Role Kasir**: boleh memproses transaksi penjualan & mencetak struk, boleh melihat stok (read-only) untuk cek ketersediaan barang saat melayani pembeli, tidak boleh mengubah data stok secara manual maupun melihat harga beli/riwayat pembelian.

**Pertanyaan:**
Dengan konteks dan deskripsi yang ada, jika seorang user dengan role Kasir mencoba membuka menu "Riwayat Pembelian & Harga Beli" (baik lewat tombol menu maupun langsung ketik URL pada browser), bagaimana alur/algoritma pengecekan yang seharusnya terjadi di sistem dari user itu login sampai permintaan itu ditolak atau diterima? Gambarkan langkah-langkahnya (boleh menggunakan flowchart atau dengan alur proses deskriptif).

### Soal 2 — Perhitungan Diskon Berjenjang

**Konteks:**
Toko sering memberi diskon berjenjang berdasarkan total belanja per transaksi, untuk mendorong pembeli belanja lebih banyak.

**Deskripsi:**
Aturan diskon toko:

- Total belanja < Rp 100.000 → tidak ada diskon
- Total belanja Rp 100.000–299.999 → diskon 5%
- Total belanja Rp 300.000–499.999 → diskon 10%
- Total belanja >= Rp 500.000 → diskon 15%

Diskon dihitung dari total belanja sebelum pajak/ongkos lain, dan berlaku untuk transaksi tunai/non-tunai biasa (tidak berlaku kalau pembeli juga pakai voucher/kupon terpisah).

**Pertanyaan:**
Dengan konteks dan deskripsi yang ada, jika ada transaksi dengan total belanja Rp 480.000 dan pembeli juga menggunakan 1 voucher potongan Rp 30.000, bagaimana algoritma/urutan langkah yang harus dijalankan sistem untuk menghitung total akhir yang harus dibayar pembeli? Jelaskan urutan pengecekannya beserta hasil akhirnya.

### Soal 3 — Nomor Struk/Invoice Berurutan Otomatis

**Konteks:**
Tiap transaksi penjualan harus punya nomor struk/invoice sendiri yang unik (tidak boleh sama dengan transaksi lain), supaya mudah dilacak dan tidak tertukar saat direkap.

**Deskripsi:**
Format nomor invoice yang dipakai toko: `INV-<tanggal>-<urut>`, contoh `INV-20260812-0001`. Nomor urut ini harus reset ke `0001` lagi setiap kali berganti hari (jadi transaksi pertama besok juga mulai dari `0001` lagi, bukan lanjut dari nomor terakhir hari sebelumnya).

**Pertanyaan:**
Dengan konteks dan deskripsi yang ada, jelaskan algoritma sistem untuk menentukan nomor invoice sebuah transaksi baru. Jawab 2 hal berikut:

- (a) Langkah apa saja yang dilakukan sistem, dari transaksi baru mulai diproses sampai nomor invoice-nya jadi (misal `INV-20260812-0007`)? Jelaskan urutannya langkah demi langkah.
- (b) Bagaimana caranya sistem tahu kapan harus mulai lagi dari `0001` (pas ganti hari) dan kapan harus lanjut dari nomor terakhir (masih di hari yang sama)?

### Soal 4 — Pembayaran Gabungan Beberapa Metode (Split Payment)

**Konteks:**
Tidak semua pembeli bayar dengan 1 metode saja — kadang pembeli ingin bayar sebagian pakai uang tunai dan sisanya pakai kartu/transfer dalam satu transaksi yang sama.

**Deskripsi:**
Sistem kasir mengizinkan 1 transaksi dibayar dengan lebih dari 1 metode pembayaran sekaligus (tunai, kartu, transfer, dst), asal total semua metode yang diinput sama dengan total tagihan. Kasir menginput nominal untuk tiap metode satu per satu, dan sistem baru boleh menganggap transaksi "lunas" kalau jumlah semua nominal yang diinput sudah menutupi total tagihan.

**Pertanyaan:**
Dengan konteks dan deskripsi yang ada, jika total tagihan sebuah transaksi adalah Rp 250.000, dan kasir menginput pembayaran secara bertahap: tunai Rp 100.000, lalu kartu Rp 100.000, lalu tunai lagi Rp 60.000, bagaimana algoritma yang mengecek status "lunas/belum" setiap kali kasir menambahkan 1 metode pembayaran baru, dan bagaimana caranya sistem tahu ada kelebihan bayar Rp 10.000 di langkah terakhir serta harus dijadikan uang kembalian (bukan tambahan ke metode manapun)? Jelaskan alurnya langkah demi langkah.

### Soal 5 — Pembulatan Uang Kembalian Tunai

**Konteks:**
Sejak uang koin/recehan kecil makin jarang dipakai, banyak toko membulatkan nominal transaksi tunai supaya kembaliannya tidak perlu pecahan di bawah Rp 100 atau Rp 500.

**Deskripsi:**
Aturan pembulatan toko: total tagihan (setelah diskon, sebelum kembalian dihitung) dibulatkan ke kelipatan Rp 100 terdekat — kalau sisa di bawah Rp 50 dibulatkan ke bawah, kalau Rp 50 ke atas dibulatkan ke atas. Pembulatan ini hanya berlaku untuk pembayaran tunai; pembayaran non-tunai (kartu/QRIS) tidak dibulatkan sama sekali, ditagih sesuai nominal asli.

**Pertanyaan:**
Dengan konteks dan deskripsi yang ada, seorang pembeli punya total tagihan Rp 47.630 dan membayar tunai dengan uang Rp 50.000. Jelaskan algoritma sistemnya untuk menjawab 3 hal berikut:

- (a) Berapa total tagihan pembeli itu setelah dibulatkan?
- (b) Berapa uang kembalian yang harus diberikan ke pembeli?
- (c) Seandainya pembeli yang sama membayar pakai QRIS/non-tunai, apa yang berbeda dari jawaban (a) dan (b) di atas?

Jelaskan urutan langkah perhitungannya untuk tiap poin.

---

## Testing / Pengujian

### Soal 1 — Validasi Input Qty Transaksi

**Konteks:**
Form input transaksi penjualan di kasir punya kolom Qty (jumlah barang) yang diisi manual oleh kasir untuk barang-barang tertentu yang tidak pakai barcode scan.

**Deskripsi:**
Seharusnya kolom Qty hanya menerima angka bulat positif (minimal 1), tidak boleh 0, tidak boleh negatif, dan tidak boleh melebihi stok yang tersedia.

**Pertanyaan:**
Dengan konteks dan deskripsi yang ada, jika kamu diminta menguji kolom Qty ini, buatkan minimal 6 skenario tes (test case) yang mencakup kondisi normal maupun kondisi "nakal"/tidak wajar (contoh: angka 0, negatif, huruf, angka desimal, melebihi stok, dst), lengkap dengan input yang dicoba dan hasil yang diharapkan.

### Soal 2 — Pengujian Alur Retur Barang

**Konteks:**
Toko punya fitur retur untuk pembeli yang mengembalikan barang cacat/rusak.

**Deskripsi:**
Urutan proses retur di sistem:

1. Kasir memilih transaksi pembeli (struk pelanggan).
2. Kasir memilih barang yang mau diretur beserta jumlahnya — jumlah retur tidak boleh lebih dari jumlah yang pernah dibeli di transaksi itu.
3. Sistem mengembalikan uang ke pembeli sesuai harga beli barang itu di transaksi asal.
4. Stok gudang bertambah sesuai jumlah retur — kecuali barang ditandai "rusak, tidak layak jual ulang": kalau ditandai rusak, stok tidak bertambah, tapi tetap dicatat sebagai kerugian toko.

**Pertanyaan:**
Dengan konteks dan deskripsi yang ada, buatkan skenario tes (test case) untuk masing-masing kondisi berikut, lengkap dengan hasil yang diharapkan:

- (a) Retur normal — barang yang dikembalikan masih bagus/layak jual ulang. Cek: uang kembali ke pembeli & stok gudang harus bertambah.
- (b) Retur barang ditandai rusak — barang tidak layak dijual lagi. Cek: uang tetap kembali ke pembeli, tapi stok gudang tidak boleh bertambah.
- (c) Retur dengan jumlah melebihi yang pernah dibeli — misal pembeli beli 2 pcs tapi minta retur 5 pcs. Cek: sistem harus menolak.

Untuk tiap skenario, tulis: data contoh yang dipakai (nama barang, qty beli, qty retur), langkah pengujian, serta hasil yang diharapkan.

### Soal 3 — Rekonsiliasi Laporan Kas Harian

**Konteks:**
Setiap kasir tutup shift, dan sistem menghasilkan laporan total kas, berdasarkan seluruh transaksi tunai selama shift itu berlangsung.

**Deskripsi:**
Rumus yang dipakai sistem:

```
Total Kas = Modal Awal + Total Penjualan Tunai − Total Kembalian − Total Retur Tunai
```

Aturan tiap jenis transaksi terhadap rumus di atas:

- **Penjualan TUNAI**: ikut menambah "Total Penjualan Tunai".
- **Penjualan NON-TUNAI** (kartu/QRIS): tidak dihitung sama sekali, uangnya masuk ke bank.
- **Retur TUNAI**: ikut mengurangi lewat "Total Retur Tunai" — uang balik ke pembeli, diambil dari laci.
- **Transaksi DIBATALKAN**: tidak ikut dihitung.

**Pertanyaan:**
Dengan konteks dan deskripsi yang ada, buat skenario tes untuk SATU shift kasir yang berisi campuran transaksi berikut:

- 3 transaksi penjualan TUNAI
- 2 transaksi penjualan NON-TUNAI
- 1 retur TUNAI
- 1 transaksi yang DIBATALKAN

Langkah yang perlu kamu kerjakan:

1. Tentukan sendiri modal awal shift & nominal tiap transaksi di atas.
2. Hitung berapa "Total Kas" yang seharusnya muncul di laporan sistem, pakai rumus di atas.
3. Jelaskan transaksi mana saja yang ikut dan mana yang tidak ikut dihitung, supaya jelas kenapa hasil akhirnya seperti itu.

### Soal 4 — Pengujian Promo Berbasis Periode Waktu

**Konteks:**
Toko menjalankan promo "Diskon 20%" untuk kategori produk minuman, yang berlaku hanya di tanggal 1–7 tiap bulan (misal promo awal bulan). Di luar tanggal itu, harga kembali normal tanpa diskon.

**Deskripsi:**
Sistem menentukan promo aktif/tidak dengan membandingkan tanggal transaksi terhadap tanggal mulai & tanggal selesai promo yang disimpan di data promo. Promo harus otomatis berhenti berlaku begitu lewat tanggal selesai, tanpa perlu ada yang manual mematikan promo itu.

**Pertanyaan:**
Dengan konteks dan deskripsi yang ada, buatkan skenario tes untuk memastikan promo ini aktif/tidaknya mengikuti tanggal. Buat 1 skenario tes untuk masing-masing kondisi tanggal berikut beserta hasil yang diharapkan (diskon 20% harus muncul atau tidak):

- (a) Transaksi di tengah periode promo (misal tanggal 4) → Expected: diskon HARUS berlaku.
- (b) Transaksi tepat di hari terakhir promo (tanggal 7) → Expected: diskon HARUS tetap berlaku (hari terakhir masih termasuk periode promo).
- (c) Transaksi sehari setelah promo berakhir (tanggal 8) → Expected: diskon TIDAK boleh berlaku lagi.
- (d) Transaksi sehari sebelum promo dimulai (tanggal terakhir bulan sebelumnya, misal tanggal 30/31) → Expected: diskon TIDAK boleh berlaku.

Untuk tiap skenario, tulis: tanggal transaksi yang dipakai, langkah pengujian, dan hasil yang diharapkan sesuai catatan di atas.

### Soal 5 — Pengujian Fitur Cari Produk

**Konteks:**
Kasir/admin butuh cara cepat menemukan produk dari daftar ribuan produk yang ada di toko, tanpa harus scroll satu-satu. Karena itu sistem biasanya menyediakan kolom pencarian (search box) di halaman daftar produk.

**Deskripsi:**
Kolom pencarian produk seharusnya bisa mencari berdasarkan nama produk (walau hurufnya kecil/besar campur, dan walau yang diketik cuma sebagian kata, tidak harus nama lengkap), maupun berdasarkan kode/barcode produk. Kalau kata kunci yang diketik tidak cocok dengan produk manapun, halaman harus menampilkan pesan "data tidak ditemukan", bukan halaman kosong tanpa keterangan atau malah error.

**Pertanyaan:**
Dengan konteks dan deskripsi yang ada, buatkan minimal 5 skenario tes (test case) untuk kolom pencarian produk ini, mencakup:

- (a) Pencarian dengan kata kunci yang PASTI ada (misalnya sebagian nama produk yang kamu tahu ada di daftar).
- (b) Pencarian dengan huruf besar/kecil dicampur (misal "KeCaP" untuk mencari produk "Kecap Manis").
- (c) Pencarian pakai kode/barcode produk, bukan nama.
- (d) Pencarian dengan kata kunci yang PASTI tidak ada di produk manapun (misal ketik "xxzzxx").
- (e) Pencarian dengan kolom dikosongkan/tidak diisi sama sekali.

Untuk tiap skenario, tulis: kata kunci yang dipakai dan hasil yang diharapkan.

---

## Desain Database

### Soal 1 — Skema Hak Akses (Role & Permission)

**Konteks:**
Sistem butuh cara menyimpan data siapa boleh mengakses fitur apa, dan pengaturan ini harus bisa diubah-ubah oleh admin tanpa perlu ubah kode program (misal suatu saat ada role baru, atau role lama ditambah 1 izin akses baru).

**Deskripsi:**
Merujuk ke 3 role di soal Algoritma nomor 1 (Gudang, Pembelian, Kasir Utama) dengan izin akses masing-masing yang berbeda-beda per fitur (lihat stok, lihat expired, catat pembelian, lihat harga beli, transaksi kasir, ubah harga jual, dst).

**Pertanyaan:**
Dengan konteks dan deskripsi yang ada, jika dibuatkan skema database-nya, tabel apa saja yang dibutuhkan (nama tabel, field-field serta tipe datanya) dan bagaimana relasi antar tabelnya, supaya 1 role bisa punya banyak izin akses (permission), dan 1 user bisa cek aksesnya apa saja saat berhasil login?

### Soal 2 — Skema Transaksi Penjualan & Pergerakan Stok

**Konteks:**
Satu transaksi penjualan biasanya berisi banyak barang berbeda sekaligus, dan tiap transaksi harus bisa ditelusuri ulang barang apa saja yang terjual, berapa harganya saat itu, dan bagaimana pengaruhnya ke stok.

**Deskripsi:**
Kebutuhan minimal: nomor invoice/struk, tanggal & waktu transaksi, kasir yang melayani, daftar barang yang dibeli beserta qty & harga satuan saat transaksi terjadi (harga ini harus tetap sama walau harga produk di master berubah di kemudian hari), total & metode pembayaran, dan status transaksi.

**Pertanyaan:**
Dengan konteks dan deskripsi yang ada, jika dibuatkan skema database-nya, tabel apa saja yang dibutuhkan beserta fieldnya, dan bagaimana relasinya supaya harga barang yang tersimpan di transaksi lama tidak ikut berubah walau harga produk di master diubah kemudian hari?

### Soal 3 — Skema Stock Opname (Pencocokan Stok Fisik)

**Konteks:**
Setiap akhir bulan, gudang melakukan penghitungan stok fisik (stock opname) untuk semua produk, lalu dicocokkan dengan angka stok yang tercatat di sistem. Kalau ada selisih (barang hilang, rusak tidak tercatat, salah input sebelumnya, dll), stok di sistem perlu disesuaikan supaya sama dengan kondisi fisik yang sebenarnya.

**Deskripsi:**
Kebutuhan minimal: tiap sesi stock opname harus tercatat kapan dilakukan, siapa yang melakukan, dan gudang mana yang dihitung. Untuk tiap produk yang dihitung, sistem perlu menyimpan: berapa stok menurut sistem sebelum stock opname, berapa hasil hitung fisik sebenarnya, dan berapa selisihnya (bisa lebih, bisa kurang, bisa juga pas/tidak ada selisih). Setelah sesi opname disetujui, stok produk di gudang itu harus otomatis disesuaikan mengikuti hasil hitung fisik.

**Pertanyaan:**
Dengan konteks dan deskripsi yang ada, jika dibuatkan skema database-nya, jawab per poin berikut:

1. **Riwayat** — tabel apa saja yang dibutuhkan beserta fieldnya, supaya riwayat tiap sesi stock opname bisa dilihat ulang kapan saja, termasuk detail produk mana saja yang selisih dan berapa besar selisihnya per produk? Serta bagaimana relasi antar tabelnya.
2. **Update Stok** — bagaimana caranya supaya stok produk di gudang otomatis ter-update mengikuti hasil opname, begitu sesi opname itu disetujui? Jelaskan tabel/field mana yang terhubung ke tabel stok gudang yang sudah ada.

---

## Desain Front End

### Soal 1 — Rancangan Layar Kasir

**Konteks:**
Kasir bekerja cepat untuk melayani banyak pembeli berurutan — tampilan yang berantakan atau terlalu banyak klik akan memperlambat antrean dan bikin kasir gampang salah input.

**Deskripsi:**
Halaman input transaksi kasir minimal butuh area-area berikut: tempat mencari/scan barang, daftar barang yang sudah ditambahkan ke keranjang beserta qty & subtotal per baris, total keseluruhan yang harus dibayar, area input pembayaran (termasuk kemungkinan lebih dari 1 metode bayar), dan tombol aksi utama (bayar, batal, cetak ulang). Kasir sering pakai keyboard (barcode scanner terhubung sebagai input keyboard) dan jarang pakai mouse supaya cepat.

**Pertanyaan:**
Dengan konteks dan deskripsi yang ada, buatkan rancangan/wireframe sederhana untuk halaman ini (boleh sketsa manual difoto, boleh digambar di drawio/Figma/tools sejenis). Dengan penjelasan:

- (a) Di mana posisi tiap area (pencarian barang, keranjang, total, pembayaran, tombol aksi/simpan) kamu letakkan, dan kenapa disusun seperti itu.
- (b) Elemen apa yang paling ditonjolkan/dibesarkan di layar itu, dan kenapa elemen itu yang paling penting bagi kasir.
- (c) Bagaimana rancanganmu mengakomodasi kasir yang jarang pakai mouse (misal: urutan fokus antar kolom pas ditekan Enter/Tab, shortcut keyboard, dll).

---

## Pengumpulan Jawaban

Semua jawaban dapat di-push ke repository GitHub pribadi, lalu link repository-nya diinformasikan ke nomor **+62 853-2799-6377** (Idin).

---

**Referensi:** dalam pembuatan flowchart / ERD / wireframe bisa menggunakan <https://www.drawio.com/> atau tools sejenis.

**Diperbarui:** 12 Agustus 2026

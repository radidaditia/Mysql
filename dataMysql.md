1. Membuat Database Baru:
   CREATE DATABASE hotel_management;

2. Menggunakan Database:
   USE hotel_management;

3. Membuat Tabel untuk Kamar:
   CREATE TABLE kamar (
   id INT PRIMARY KEY AUTO_INCREMENT,
   nomor_kamar INT NOT NULL,
   tipe_kamar VARCHAR(50) NOT NULL,
   harga DECIMAL(10,2) NOT NULL,
   status ENUM('tersedia', 'dipesan', 'terisi') NOT NULL DEFAULT 'tersedia'
   );

4. Membuat Tabel untuk Tamu:
   CREATE TABLE tamu (
   id INT PRIMARY KEY AUTO_INCREMENT,
   nama VARCHAR(100) NOT NULL,
   email VARCHAR(50),
   telepon VARCHAR(15),
   alamat TEXT
   );

5. Membuat Tabel untuk Reservasi:
   CREATE TABLE reservasi (
   id INT PRIMARY KEY AUTO_INCREMENT,
   id_tamu INT,
   id_kamar INT,
   tanggal_checkin DATE NOT NULL,
   tanggal_checkout DATE NOT NULL,
   FOREIGN KEY (id_tamu) REFERENCES tamu(id) ON DELETE CASCADE,
   FOREIGN KEY (id_kamar) REFERENCES kamar(id) ON DELETE CASCADE
   );

6. Membuat Tabel untuk Layanan Tambahan:
   CREATE TABLE layanan_tambahan (
   id INT PRIMARY KEY AUTO_INCREMENT,
   id_reservasi INT,
   jenis_layanan VARCHAR(50) NOT NULL,
   harga DECIMAL(10,2) NOT NULL,
   FOREIGN KEY (id_reservasi) REFERENCES reservasi(id) ON DELETE CASCADE
   );

7. Membuat Tabel untuk Pembayaran:
   CREATE TABLE pembayaran (
   id INT PRIMARY KEY AUTO_INCREMENT,
   id_reservasi INT,
   total_pembayaran DECIMAL(10,2) NOT NULL,
   metode_pembayaran VARCHAR(50) NOT NULL,
   tanggal_pembayaran DATE NOT NULL,
   FOREIGN KEY (id_reservasi) REFERENCES reservasi(id) ON DELETE CASCADE
   );

8. Menambahkan Data :

## menambahkan data kamar baru;

INSERT INTO tamu (nama, email, telepon, alamat) VALUES
('Budi Santoso', 'budi.santoso@example.com', '987654321', '456 Oak St'),
('Dewi Sulastri', 'dewi.sulastri@example.com', '555123456', '789 Elm St'),
('Anwar Prabowo', 'anwar.prabowo@example.com', '111222333', '321 Maple St'),
('Ratna Wijaya', 'ratna.wijaya@example.com', '777888999', '654 Pine St'),
('Agus Suryanto', 'agus.suryanto@example.com', '444555666', '987 Birch St');

## Menambahkan Data kamar baru:

INSERT INTO kamar (nomor_kamar, tipe_kamar, harga, status) VALUES
(102, 'Deluxe', 120.00, 'tersedia'),
(103, 'Standard', 80.00, 'tersedia'),
(104, 'Suite', 150.00, 'tersedia'),
(105, 'Standard', 80.00, 'tersedia'),
(106, 'Deluxe', 120.00, 'tersedia');

## Menambahkan Data reservasi Baru:

INSERT INTO reservasi (id_tamu, id_kamar, tanggal_checkin, tanggal_checkout) VALUES
(2, 2, '2024-02-18', '2024-02-25'),
(3, 3, '2024-02-20', '2024-02-22'),
(4, 4, '2024-02-22', '2024-02-25'),
(5, 5, '2024-02-25', '2024-02-28'),
(1, 1, '2024-02-28', '2024-03-05');

## Menambahkan Data layanan_tambahan:

INSERT INTO layanan_tambahan (id_reservasi, jenis_layanan, harga) VALUES
(2, 'Spa', 30.00),
(3, 'Wifi', 10.00),
(4, 'Sarapan', 20.00),
(5, 'Wifi', 10.00),
(1, 'Spa', 30.00);

## Menambahkan Data pembayaran:

INSERT INTO pembayaran (id_reservasi, total_pembayaran, metode_pembayaran, tanggal_pembayaran) VALUES
(2, 180.00, 'Kartu Kredit', '2024-02-25'),
(3, 110.00, 'Tunai', '2024-02-22'),
(4, 200.00, 'Kartu Debit', '2024-02-25'),
(5, 90.00, 'Kartu Kredit', '2024-02-28'),
(1, 180.00, 'Tunai', '2024-03-05');

9. Melihat Data Tamu:
   SELECT \* FROM tamu;

10. Mencari Reservasi Berdasarkan ID Tamu:
    SELECT \* FROM reservasi WHERE id_tamu = 1;

11. Menggunakan ORDER BY dan JOIN untuk Melihat Detail Reservasi:
    SELECT reservasi.id, tamu.nama AS nama_tamu, kamar.nomor_kamar, reservasi.tanggal_checkin, reservasi.tanggal_checkout
    FROM reservasi
    JOIN tamu ON reservasi.id_tamu = tamu.id
    JOIN kamar ON reservasi.id_kamar = kamar.id
    ORDER BY reservasi.id DESC;

12. Menggunakan LIMIT:
    SELECT \* FROM kamar LIMIT 5;

13. Menggunakan Like untuk Pencarian Data:
    SELECT \* FROM tamu WHERE nama LIKE "%John%";

14. Menggunakan Aliases:
    SELECT nomor_kamar AS "Nomor Kamar", tipe_kamar AS "Tipe Kamar" FROM kamar;

15. Menggunakan IN:
    SELECT \* FROM kamar WHERE id IN (1, 2, 3);

##

Query Join:

SELECT
reservasi.id,
tamu.nama AS nama_tamu,
kamar.nomor_kamar,
reservasi.tanggal_checkin,
reservasi.tanggal_checkout,
layanan_tambahan.jenis_layanan,
pembayaran.total_pembayaran
FROM reservasi
JOIN tamu ON reservasi.id_tamu = tamu.id
JOIN kamar ON reservasi.id_kamar = kamar.id
LEFT JOIN layanan_tambahan ON reservasi.id = layanan_tambahan.id_reservasi
LEFT JOIN pembayaran ON reservasi.id = pembayaran.id_reservasi;

##

LEFT JOIN untuk menggabungkan data dari tabel kamar, tamu, dan reservasi:

SELECT
kamar.nomor_kamar,
tamu.nama AS nama_tamu,
reservasi.tanggal_checkin,
reservasi.tanggal_checkout
FROM
kamar
LEFT JOIN reservasi ON kamar.id = reservasi.id_kamar
LEFT JOIN tamu ON reservasi.id_tamu = tamu.id;

##

RIGHT JOIN untuk menggabungkan data dari tabel tamu, layanan_tambahan, dan pembayaran:

SELECT
tamu.nama,
layanan_tambahan.jenis_layanan,
pembayaran.total_pembayaran
FROM
tamu
RIGHT JOIN reservasi ON tamu.id = reservasi.id_tamu
RIGHT JOIN layanan_tambahan ON reservasi.id = layanan_tambahan.id_reservasi
RIGHT JOIN pembayaran ON reservasi.id = pembayaran.id_reservasi;

##

INNER JOIN untuk menggabungkan data dari tabel kamar, tamu, reservasi, layanan_tambahan, dan pembayaran:

SELECT
kamar.nomor_kamar,
tamu.nama AS nama_tamu,
reservasi.tanggal_checkin,
reservasi.tanggal_checkout,
layanan_tambahan.jenis_layanan,
pembayaran.total_pembayaran
FROM
kamar
JOIN reservasi ON kamar.id = reservasi.id_kamar
JOIN tamu ON reservasi.id_tamu = tamu.id
JOIN layanan_tambahan ON reservasi.id = layanan_tambahan.id_reservasi
JOIN pembayaran ON reservasi.id = pembayaran.id_reservasi;

##

## Menghapus data dari tabel pembayaran yang terkait dengan reservasi tamu

DELETE FROM pembayaran WHERE id_reservasi IN (SELECT id FROM reservasi WHERE id_tamu = 1);

## Menghapus data dari tabel layanan_tambahan yang terkait dengan reservasi tamu

DELETE FROM layanan_tambahan WHERE id_reservasi IN (SELECT id FROM reservasi WHERE id_tamu = 1);

## Menghapus data dari tabel reservasi yang terkait dengan tamu

DELETE FROM reservasi WHERE id_tamu = 1;

## Menghapus data tamu

DELETE FROM tamu WHERE id = 1;

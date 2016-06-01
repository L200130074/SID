# SID
Nama  :Dian Suryanto
Nim   :L200130074
# Sistem Informasi Terdistribudi
TUGAS (Soal UTS)

1. Menyiapkan database yang akan digunakan Sebelum menulis kode program untuk aplikasi server, terlebih dahulu Anda siapkan database yang akan digunakan, yaitu data mengenai mahasiswa.
mysql> CREATE DATABASE Mhs; mysql> CREATE TABLE Mahasiswa(nama VARCHAR(20) PRIMARY KEY, nim VARCHAR(30) NOT NULL, alamat VARCHAR(255) NOT NULL, ipk VARCHAR(4) NOT NULL );

Menulis kode program untuk aplikasi server
Setelah Anda membuat database ‘Mhs’, langkah selanjutnya yaitu membuat kode program untuk aplikasi server dari Web Service.

<? require_once('nusoap.php'); $ns = "http://localhost:libnusoap/"; $server = new soap_server; $server->configureWSDL('Mahasiswa', $ns); $server->wsdl->schemaTargetNamespace = $ns; $server->register('Mahasiswa', array('nama’ => 'xsd:string'), array('return'=>'xsd:string'), $ns); function RamalanZodiak($nama) { if (!$nama) { return new soap_fault('Client', '', 'Harus ada nilainya!', ''); } 9 if ($conn = mysql_connect("host", "user", "password")) { if ($db = mysql_select_db("Mhs")) { $result = mysql_query("SELECT * FROM Mahasiswa WHERE nama = '$nama "); while ($row = mysql_fetch_array($result)) { $nama = $row["nama "]; $nim = $row["nim"]; $alamat = $row["alamat"]; $ipk = $row["ipk"]; } } else { return new soap_fault('Database Server', '', 'Koneksi ke database gagal!', ''); } } else { return new soap_fault('Database Server', '', 'Koneksi ke database gagal!', ''); } return "nama: $nama
nim: $nim
alamat: $alamat
ipk: $ipk
; } $server->service($HTTP_RAW_POST_DATA); exit(); ?> Untuk memastikan apakah aplikasi server yang telah dibangun dapat berjalan dengan baik atau tidak, ada baiknya kalau Anda melakukan pengetesan terlebih dahulu sebelum dnda menulis kode program untuk aplikasi client dari Web Service Jika pengetesan yang Anda lakukan berhasil, maka pada browser Anda akan tampil seperti gambar di atas. Anda dapat melihat deskripsi dari Web Service yang Anda bangun dengan memilih menu WSDL pada bagian kiri atas.

Menulis kode program untuk aplikasi client
Langkah berikutnya adalah menulis kode program untuk aplikasi client. Aplikasi client akan melakukan permintaan layanan pada server Web Service, dan akan menerima nilai yang dikembalikan oleh server Web Service.

<?php require_once "lib/nusoap.php";

$wsdl = "http://localhost/sid/soapjsonserver.php?wsdl"; $client = new nusoap_client($wsdl,'wsdl');

$error = $client->getError(); if ($error) { echo "

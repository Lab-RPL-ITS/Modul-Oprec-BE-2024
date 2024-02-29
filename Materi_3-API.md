# API

## Daftar Isi

- [Daftar Isi](#daftar-isi)
- [HTTP/HTTPS](#http/https)
- [Definisi & Fungsi](#definisi--fungsi)
  - [Definisi](#definisi)
  - [Fungsi](#fungsi)
- [Macam Arsitektur](#macam-arsitektur)
  - [SOAP](#soap)
  - [GraphQL](#graphql)
  - [RPC](#rpc)
  - [Rest API](#rest-api)
- [Contoh Tech Stack](#contoh-tech-stack)
- [Terkait Database](#terkait-database)
  - [Integrasi](#integrasi)
  - [Migration](#migration)
  - [Seeder & Factory](#seeder--factory)
- [ORM](#orm)
  - [GORM](#gorm)
  - [Kelebihan dan Kekurangan GORM](#kelebihan-dan-kekurangan-gorm)
  - [Implementasi GORM](#implementasi-gorm)
- [Authentication](#authentication)
- [Middleware](#middleware)
- [Authentication Implementation](#authentication-implementation)
- [Encryption & Hash](#encryption--hash)
  - [Encryption](#encryption)
  - [Hash](#hash)
- [Authorization](#authorization)
- [File Handling](#file-handling)
  - [Database](#database)
  - [Server Storage](#server-storage)
  - [Cloud Storage](#cloud-storage)
- [Pemilihan](#pemilihan)

## HTTP/HTTPS
<img src="image/Materi_3-API/httpvshttps.jpg">
HTTP (*Hypertext Transfer Protocol*) adalah protokol yang digunakan untuk mengirim dokumen *hypertext* antara server dan client di *World Wide Web*. Protokol ini mendefinisikan bagaimana pesan diformat dan ditransmisikan, serta apa tindakan yang harus diambil oleh *web server* dan *browser* dalam berbagai tahapan komunikasi. <br>

#### Apa sih protokol itu?
Protokol disini adalah aturan atau standar yang menentukan bagaimana data dikirimkan, diterima, dan diinterpretasikan antar perangkat komputer atau entitas dalam jaringan. Contoh protokol yang umum digunakan ada TCP/IP, UDP, FTP, SMTP, DNS, HTTP, dan HTTPS.

Oke balik ke topik awal lagi. Sedangkan HTTPS (Hypertext Transfer Protocol Secure) merupakan versi aman dari HTTP, protokol dasar untuk komunikasi di World Wide Web. HTTPS menambahkan lapisan enkripsi untuk meningkatkan keamanan data yang ditransfer antara web browser pengguna dan server web. Ini dilakukan melalui penggunaan SSL/TLS (Secure Sockets Layer/Transport Layer Security), yang menyediakan enkripsi, integritas data, dan autentikasi.

### Cara Kerja Protokol HTTP

<img src="image/Materi_3-API/flow_http.png">
Protokol HTTP (Hypertext Transfer Protocol) adalah fondasi dari komunikasi data di World Wide Web, memungkinkan web browser dan server web untuk berkomunikasi dengan satu sama lain. Berikut adalah cara kerja dasar dari protokol HTTP:

#### Pembukaan Koneksi
Koneksi TCP dibuka oleh Client melalui Web browser ke server yang dituju pada port tertentu, biasanya port 80 adalah default port untuk HTTP. Koneksi ini memungkinkan pengiriman dan penerimaan data antara Klien dengan server

#### HTTP Request
Setelah koneksi terbuka, Klien mengirimkan request HTTP ke server. Request ini bisa berupa berbagai metode HTTP, seperti:
- GET = untuk meminta atau mengambil data yang ada di server
- POST = untuk mengirim data menuju server
- PUT = untuk mengganti data dengan yang sudah diperbarui
- PATCH = mirip dengan PUT, tetapi bisa memilih properti mana aja yang ingin diperbarui
- DELETE = untuk menghapus data dari database

HTTP request ini meliputi:
- Request Line = Menyatakan metode HTTP yang digunakan, URL resource yang diminta, dan versi protokol HTTP yang digunakan
- Headers = Berisi metadata tentang request, misalnya jenis konten, cookie, user agent, dan lain-lain
- Message Body = Digunakan untuk mengirimkan data ke server, seperti formulir data dan file upload

#### HTTP Response
Server memproses permintaan dan mengirimkan respons kembali ke Klien berupa:
- Status Line = Menyatakan versi protokol HTTP, kode status, dan teks status
- Headers = Berisi metadata tentang response, misalnya jenis konten, panjang konten, cookie yang telah di atur oleh server, dan lain-lain
- Message Body = Berisi konten yang sebenarnya dari response, seperti HTML, data JSON, atau jenis file lainnya

#### Penutupan Koneksi
Setelah transaksi request-response selesai, koneksi TCP biasanya akan tertutup kecuali jika header ‘Connection: keep-alive’ digunakan. Header ini akan membuat koneksi tetap terbuka untuk request berikutnya, sehingga akan mengurangi overhead pembukaan koneksi yang baru

#### Stateless
HTTP adalah protokol yang stateless, artinya setiap permintaan dari klien akan dianggap independen dan tidak bergantung pada permintaan sebelumnya sehingga server tidak akan menyimpan informasi dari Klien

### Cara Kerja Protokol HTTPS
Alur kerja HTTP dan HTTPS memiliki beberapa perbedaan kunci, terutama berkaitan dengan keamanan komunikasi antara klien (seperti browser web) dan server. Perbedaan utama antara HTTP dan HTTPS terletak pada penggunaan lapisan keamanan tambahan pada HTTPS melalui SSL/TLS, yang mengamankan data yang ditransfer melalui enkripsi dan autentikasi. Sementara HTTP lebih cepat dan sederhana dalam implementasi, kurangnya keamanan membuatnya tidak cocok untuk transaksi sensitif atau pertukaran data pribadi. Berikut adalah alur kerja dari HTTPS:

#### Pembukaan Koneksi
Klien membuat permintaan HTTPS ke server dengan memasukkan URL yang menggunakan skema ```https://``` di web browser mereka. Koneksi TCP antara klien dan server biasanya dilakukan melalui port 443

#### Penyajian Sertifikat
Server akan merespons dengan mengirimkan sertifikat digitalnya ke klien. Sertifikat ini dikeluarkan oleh Certificate Authority (CA) dan berisi informasi publik server, termasuk kunci publiknya, nama serve, masa berlaku dari sertifikat, dan tandatangan digital dari CA

#### Verifikasi Sertifikat
Klien akan memverifikasi sertifikat tersebut untuk memastikan keaslian server. Proses ini termasuk proses memeriksa apakah sertifikat yang digunakan belum kadaluarsa, diterbitkan oleh CA yang terpercaya, dan digunakan oleh website yang sebenarnya diminta. Jika sertifikat tidak dapat diverifikasi, klien akan menampilkan peringatan keamanan website

#### Pertukaran Kunci
Setelah sertifikat diverifikasi, klien dan server melakukan pertukaran kunci untuk menyetujui kunci enkripsi simetris yang akan digunakan untuk mengamankan sesi komunikasi

#### Enkripsi Komunikasi
Semua data yang ditransfer termasuk HTTP request, HTTP response, data formulir, cookie, dll, antara klien dan server akan dienkripsi menggunakan kunci simetris yang sudah disepakati untuk melindungi data dari penyadapan dan manipulasi

#### Penutupan Koneksi
Setelah komunikasi selesai, sesi enkripsi akan ditutup. Kunci simetris yang digunakan di sesi tersebut tidak akan digunakan lagi, sehingga proses negosiasi kunci akan diulang untuk membuat kunci simetris baru pada sesi berikutnya

## Definisi & Fungsi
Setelah memahami konsep HTTP dan HTTPS yang berperan sebagai perisai digital ketika berkelana di dunia maya, kita perlu memahami “transportasi” apa yang cocok digunakan ketika berkelana. <br><br>

Bayangkan jika internet adalah sebuah kota besar yang ramai dan penuh dengan destinasi menarik. Di kota ini, HTTP dan HTTPS berperan seperti sistem jalan raya yang menghubungkan berbagai lokasi. HTTP adalah jalan raya biasa, di mana informasi bergerak bebas tapi terbuka untuk dilihat siapa saja. Sementara itu, HTTPS seperti jalan raya yang dilengkapi dengan terowongan aman; informasi yang bergerak di dalamnya terlindungi, jadi hanya yang berhak saja yang bisa melihatnya. <br><br>

Nah, di sinilah API masuk sebagai sistem transportasi umum seperti bus, kereta, atau taksi yang ada di kota tersebut. API memudahkan kita untuk 'berpindah' dari satu layanan ke layanan lain, seperti dari media sosial ke layanan cuaca, atau dari toko online ke layanan pengiriman paket, tanpa perlu berjalan kaki atau mengemudi sendiri. Kita cukup memberitahu API tujuan dan ia akan mengantarkan permintaan kita ke tujuan yang tepat dan kembali dengan informasi atau layanan yang kita butuhkan. <br><br>

Dengan kombinasi jalan raya digital HTTPS yang aman dan sistem transportasi umum API yang efisien, menjelajahi kota internet menjadi lebih mudah, aman, dan menyenangkan. Tapi sebenarnya API ini apa sih?

<img src="image/Materi_3-API/rest-api.png">

### Definisi
API atau Application Programming Interface adalah mekanisme yang memungkinkan dua komponen perangkat lunak untuk saling berkomunikasi menggunakan serangkaian definisi dan protokol. Sama seperti sistem transportasi umum tadi, API dapat memudahkan pertukaran data dan layanan antara berbagai sistem dan aplikasi. Misalnya, sistem perangkat lunak badan meteorologi, klimatologi, dan geofisika (BMKG) berisi data cuaca harian. Aplikasi cuaca di ponsel akan “berkomunikasi” dengan sistem ini melalui API dan menampilkan pembaruan cuaca harian.

### Fungsi
Dari contoh diatas, bisa disimpulkan bahwa Fungsi API ini sangat beragam dan tergantung pada kebutuhan dan tujuan implementasi. Berikut adalah beberapa fungsi umum dari API:

1. Penghubung (Connector) = Seperti yang sudah dijelaskan sebelumnya, API berfungsi sebagai penghubung antara dua aplikasi atau sistem yang berbeda. Ini memungkinkan pertukaran data dan komunikasi antar aplikasi atau sistem. Dengan API, tugas-tugas yang biasanya membutuhkan intervensi manual bisa diotomatisasi, mempercepat proses dan meningkatkan efisiensi.

2. Abstraksi Kompleksitas = API dapat menyembunyikan kompleksitas implementasi internal suatu sistem, dan memberikan antarmuka yang mudah digunakan untuk pengguna.

3. Integrasi Aplikasi = API memungkinkan aplikasi berbeda untuk saling berintegrasi dan berbagi fungsionalitas, sehingga API dapat memperluas dan meningkatkan kemampuan aplikasi tersebut.

4. Pemberian Layanan Web (Web Services) = Banyak API yang disediakan sebagai layanan web, memfasilitasi pertukaran data melalui protokol HTTP atau HTTPS.

5. Optimasi Kinerja = Dengan menggunakan API, kinerja aplikasi dapat dioptimalkan dengan memanfaatkan fungsionalitas yang sudah ada tanpa perlu menulis ulang kode dari awal.

6. Mendukung Pengembangaan Kolaboratif = API memungkinkan tim developer yang berbeda untuk bekerja secara terpisah pada komponen-komponen yang berbeda dan tetap mengintegrasikan hasil pekerjaan mereka melalui API.

## Macam Arsitektur
Setelah kita mengerti mengenai ``definisi`` dan ``fungsi`` mengenai API (*Application Programming Interface*). Saatnya kita mengenal istilah baru yaitu ``arsitektur`` pada API. **Arsitektur** pada API terdapat beberapa macam yang umum digunakan, diantaranya adalah:

- SOAP (Simple Object Access Protocol)
- GraphQL 
- RPC
  - JSON-RPC
  - XML-RPC
- REST (Representational State Transfer)

Apa sajakah itu? Mari kita mengenal lebih dalam terkait arsitektur tersebut

#### SOAP 
*Simple Obkject Access Protocol* atau biasa dikenal dengan SOAP adalah protokol komunikasi yang menggunakan ``XML`` sebagai format untuk pertukaran pesan di lingkungan jaringan terdistribusi.

SOAP menggunakan protokol *Transport* seperti HTTP, SMTP, dan sebgainya. SOAP mendefinisikan serangkaian aturan dan standar untuk komunikasi seperti WSDL (Web Services Description Language) dan UDDI (Universal Description, Discovery, and Integration).

Contoh dengan bahasa pemrograman Python
```py
from zeep import Client

client = Client('https://www.example.com/exampleapi')
result = client.service.GetUser(123) # request user with ID 123

name = result['Username']
```

Dalam contoh tersebut, library ``zeep`` merupakan salah satu yang dapat digunakan untuk mengimplementasikan SOAP. Dimulai dengan inisialisasi *client* berdasarkan SOAP *endpoint*. Kemudian dipanggil layanan ``GetUser`` dengan parameter ID_Pengguna. Lalu kita melakukan *assign* terhadap hasil dari client tersebut pada variabel ``name``

Mari kita lihat bagaimana ``SOAP`` ini disusun:

```xml
<?xml version="1.0"?>
<soap:Envelope xmlns:soap="https://www.w3.org/2003/05/soap-envelope">
  <soap:Header>
  </soap:Header>
  <soap:Body>
    <m:GetUser>
      <m:UserId>123</m:UserId>
    </m:GetUser>
  </soap:Body>
</soap:Envelope>
```

dan mungkin response nya adalah sebagai berikut

```xml
<?xml version="1.0"?>

<soap:Envelope
xmlns:soap="https://www.w3.org/2003/05/soap-envelope/"
soap:encodingStyle="https://www.w3.org/2003/05/soap-encoding">

<soap:Body>
  <m:GetUserResponse>
    <m:Username>RPL</m:Username>
  </m:GetUserResponse>
</soap:Body>

</soap:Envelope>
```

#### GraphQL

<img src="image/Materi_3-API/3-2.png">

GraphQL adalah bahasa query yang fleksibel untuk API dan dikembangkan oleh *Facebook*. GraphQL memungkinkan klien untuk menentukan struktur data yang diinginkan dan mengembalikan data yang sesuai dengan permintaan.

Untuk pertukaran data umumnya menggunakan ``JSON``. GraphQL menggunakan satu endpoint untuk semua permintaan dan klien dapat menentukan sendiri data yang diinginkan.

Untuk mencoba melakukan demo pada GraphQL dapat diakses pada url berikut https://graphql-demo.mead.io/

Agar lebih paham mengenai graphQL dapat dilihat pada url [Youtube](https://youtu.be/eIQh02xuVw4?si=8ueu4lo_x-Lf29SD) tersebut.

#### RPC
*Remote Procedure Call* atau biasa dikenal dengan istilah RPC adalah mekanisme yang memungkinkan sebuah program untuk menjalankan prosedur (fungsi atau metode) pada komputer yang berbeda dalam jaringan, seolah-olah prosedur tersebut dijalankan di komputer lokal. Ini memungkinkan pemanggilan fungsi di sistem yang terdistribusi, dengan pemanggilan yang dilakukan melalui jaringan. 

Bagaimana RPC bekerja?

- Pemanggil (*Client*): Program yang memulai pemanggilan prosedur di komputer yang berbeda.
- Pelayan (*Server*): Program yang menyediakan prosedur yang dapat dipanggil di komputer target
- Stub (*Penghubung*): Berfungsi sebagai perantara antara pemanggil dan pelayan. Ada dua jenis stub: stub klien di sisi pemanggil dan stub server di sisi pelayan.

**Contoh RPC**
Misalkan kamu memiliki 2 komputer yang terhubung dalam jaringan. Pada satu komputer, kamu memiliki aplikasi klien, dan di komputer lain sebagai server. Aplikasi server menyediakan fungsi tertentu yang dapat dipanggil oleh aplikasi klien sebagai berikut:

1. Definisi Fungsi
  - Aplikasi Server: Menyediakan fungsi ``Tambah(a, b)`` untuk pengumlahan dua angka
  - Aplikasi Client: Memiliki kode yang memanggil ``Tambah(a, b)`` pada aplikasi server
2. Panggilan Proses
  - Aplikasi klien memanggil fungsi ``Tambah(a, b)`` melalui pemanggilan prosedur jarak jauh,
  - Pemanggilan ini dikemas dan dikirim ke aplikasi Server melalui jaringan.
3. Pelaksanaan Proses
  - Aplikasi server menerima pemanggilan, membuka paket data, dan mengekstrak parameter
  - Fungsi ``Tambah(a, b)`` dijalankan pada sisi server dengan paraeter yang diterima
4. Pengembalian Hasil
  - Hasil dari fungsi di server dikembalikan melalui jaringan kepada aplikasi klien melalui pemanggilan prosedur jarak jauh
  - aplikais klien menerima hasil dan melanjutkan eksekusi

**Tambahan**
- Implementasi RPC dapat menggunakan protokol khusus seperti JSON-RPC atau XML-RPC, atau protokol umum seperti HTTP sebagai transport layer.
- Sebuah contoh populer dari RPC adalah gRPC (*Google Remote Procedure Call*), yang menggunakan HTTP/2 sebagai protokol transport dan Protocol Buffers sebagai format pertukaran data.

#### REST API

<img src="image/Materi_3-API/3-1.png">

*Representational State Transfer API* atau biasa dikenal sebagai Rest-API ini merupakan antarmuka yang digunakan oleh dua sistem ``komputer`` untuk bertukar informasi secara aman melalui internet. REST memberlakukan syarat mengenai cara **API** bekerja. REST pada awlanbya dibuat sebagai panduan untuk mengelola komunikasi pada jaringan kompleks seperti internet. Anda dapat menggunakan ``arsitektur`` berbasis REST untuk mendukung komunikasi pada jaringan seperti internet.

Biasnya, Developer API dapat merancang API menggunakan beberapa arsitektur yang berbeda. API yang mengikut gaya arsitektur **REST** ini disebut sebagai API REST. Layanan web yang menerapkan arsitektur ini disebut sebai layanan web RESTful. 

**REST** menggunakan metode HTTP seperti GET, POST, PUT, PATCH, dan DELETE untuk berinteraksi dengan sumber daya. Berikut adalah penjelasannya:

- **GET**: Mendapatkan informasi tentang sumber daya / response yang telah dibuat
- **POST**: Membuat sumber daya baru / menambahkan data 
- **PUT**: Memperbarui sumber daya atau membuat sumber daya baru jika tidak ada
- **DELETE**: Menghapus sumber daya

REST services sendiri haruslah *stateless*, artinya setiap permintaan dari klien ke server harus mencakup semua informasi yang diperlukan untuk memahami dan memproses permintaan tersebut. Tidak ada informasi status yang disimpan di server antara dua permintaan. Dengan kata lain, server tidak menyimpan status klien di antara permintaan. Setiap permintaan dianggap mandiri dan berdiri sendiri.

Contoh stateless pada REST:

**Authentication**
- Saat klien melakukan permintaan, informasi autentikasi (seperti token) disertakan di header permintaan
- Contoh:

```sh
GET /api/data
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

**Session Management** 
- Tidak ada sesi (session) yang disimpan di server antara dua permintaan. Setiap permintaan harus membawa informasi yang diperlukan untuk mengidentifikasi klien.
- Contoh: 
```sh
GET /api/user/profile
Cookie: sessionid=abc123
```

**Caching**
- Informasi *caching* disertakan dalam header HTTP untuk setiap permintaan. Server tidak mengingat apa yang dikirimkan sebelumnya oleh klien.
- Contoh: 
```sh
GET /api/data
Cache-Control: max-age=3600
```
Keuntungan dari pendekatan *stateless* adalah meningkatkan skalabilitas dan kinerja server, karena tidak perlu menyimpan atau mengelola status sesi untuk setiap klien. Setiap permintaan dapat diproses independen dan tanpa ketergantungan pada permintaan sebelumnya. Dengan demikian, *stateless* sangat sesuai dengan karakteristik skala web dan aplikasi yang dapat diterapkan di berbagai server yang mendukung permintaan tanpa status.

## Contoh Tech Stack

## Terkait Database

### Integrasi
Untuk melakukan integrasi dengan **database** diperlukan library dari masing-masing bahasa pemrograman. Di setiap bahasa pemrograman pasti memiliki *driver* untuk terkoneksi dengan *database*, entah melalui terminal(local), server, maupun *User Interface* seperti MySQL Workbench, pgAdmin, DBeaver, dan sebagainya.

Bahasa pemrogaman **Golang** merupakan salah satu bahasa yang memiliki driver library yang sangat terkenal yaitu ``postgres driver`` yang ada pada **GORM** library. Berikut adalah cara untuk mengintegrasikan dengan database menggunakan bahasa pemrograman Golang

Pertama-tama, kalian bisa menginstall package berikut

```go
go get -u "gorm.io/gorm"
```

Setelah itu, kita membutuhkan *Data Source Name* atau biasa dikenal dengan DSN. DSN digunakan untuk menyediakan informasi koneksi ke sumber data, seperti database. DSN menyimpan detail konfigurasi koneksi, seperti jenis database, alamat server, nama pengguna, dan kata sandi.

```go
dbUser := "root"
dbPass := "test123"
dbHost := "localhost"
dbName := "latihan_integrasi"
dbPort := 5432

dsn := fmt.Sprintf("host=%v user=%v password=%v dbname=%v port=%v TimeZone=Asia/Jakarta", dbHost, dbUser, dbPass, dbName, dbPort)
```

Setelah itu kita bisa mekonfigurasikan dengan **GORM** library tadi sebagai berikut

```go
db, err := gorm.Open(postgres.New(postgres.Config{
  DSN: dsn,
  PreferSimpleProtocol: true,
}), &gorm.Config{})
if err != nil {
  panic(err)
}
```

Jika sudah melakukan hal tersebut, selamat anda sudah melakukan integrasi dengan database.

### Migration
Dalam konteks Pengembangan Perangkat Lunak (PPL) dan Database, *Migration* merujuk pada proses mengelola perubahan skema ``database``. Skema database mencakup struktur tabel, hubungan antar tabel, indeks, dan aturan lainnya yang mendefinisikan cara data disimpan dan diakses dalam database. Migrasi digunakan untuk memudahkan pembaruan dan perubahan struktur *database* tanpa kehilangan data yang sudah ada. Proses ini terutama berguna ketika kalian mengembangkan aplikasi secara berkelanjutan dan perlu melakukan perubahan terhadap struktur database yang sudah ada.

Pada umumnya, migrasi melibatkan beberapa langkah, seperti:

- **Membuat Migrasi**: Membuat skrip atau file migrasi yang menggambarkan perubahan yang diinginkan pada skema database. Skrip ini biasanya mencakup perintah SQL untuk menambahkan, mengubah, atau menghapus tabel, kolom, atau indeks.
- **Menjalankan Migrasi**: Menggunakan alat migrasi atau perintah tertentu yang disediakan oleh kerangka kerja atau sistem manajemen basis data (DBMS) untuk menjalankan migrasi. Ini menerapkan perubahan yang dijelaskan dalam skrip migrasi ke basis data.
- **Reversibility**: Migrasi sering menyediakan cara untuk mundur (*rollback*) atau membatalkan perubahan yang telah dijalankan. Hal ini memudahkan dalam pengembangan dan memastikan bahwa jika terjadi kesalahan, perubahan dapat dikembalikan ke keadaan sebelumnya.

Kerangka kerja pengembangan web dan ORM (*Object-Relational Mapping*) seringkali menyertakan alat atau fitur migrasi. Contohnya, Django (Python web framework) memiliki fitur migrasi yang dikenal sebagai ``Django Migrations``, sementara Ruby on Rails (Ruby web framework) memiliki alat migrasi yang disebut ``Rails Migrations``, lalu Golang dengan ``GORM`` nya dan laravel dengan migrations nya. Alat ini membantu pengembang mengelola struktur database dengan lebih terstruktur dan aman.

Pada bahasa pemrograman ``Golang`` untuk melakukan migrations, kalian tinggal menggunakan kode sebagai berikut

```go
func Migrate(db *gorm.DB) error {
	if err := db.AutoMigrate(
		// Entity yang ingin kalian migrate
	); err != nil {
		return err
	}

	return nil
}
```

Berikut merupakan ``implementasi`` dari **migration**

https://github.com/Caknoooo/go-gin-clean-architecture/assets/92671053/90c6c6a9-6a5b-46e4-a068-33f0de693154


### Seeder & Factory
Seeder dan factory adalah konsep yang umumnya digunakan dalam pengembangan perangkat lunak, terutama dalam pengembangan aplikasi web atau sistem manajemen basis data. Kedua konsep ini biasanya berkaitan dengan pengisian data atau pembuatan data palsu (*dummy data*) untuk melakukan pengujian

**Seeder**
- **Definisi**: Seeder adalah bagian dari sistem pengembangan yang bertanggung jawab untuk memasukkan data awal atau data uji ke dalam basis data (*Database*)
- **Fungsi Utama**: Sebuah seeder digunakan untuk mengisi basis data dengan data statis atau data uji yang diperlukan untuk menjalankan dan menguji aplikasi.
- **Contoh Penggunaan**: Dalam pengembangan web dengan kerangka kerja tertentu, seperti Laravel (PHP), Rails (Ruby), atau Django (Python), Anda dapat menggunakan seeder untuk memasukkan data contoh, seperti pengguna awal, peran, atau entitas lainnya ke dalam basis data.

**Factory**
- **Definisi**: Factory adalah pola desain yang digunakan untuk membuat objek secara dinamis. Dalam konteks pengembangan perangkat lunak, factory seringkali digunakan untuk membuat data palsu.
- **Fungsi Utama**: Factory digunakan untuk menghasilkan objek dengan data yang dihasilkan secara dinamis. Dalam konteks basis data, factory dapat digunakan untuk membuat rekaman palsu atau data uji dengan mudah.
- **Contoh Penggunaan**: Dalam kerangka kerja pengembangan web Laravel, misalnya, Anda dapat menggunakan fitur ``"Eloquent Factory"`` untuk mendefinisikan cara membuat data palsu. Ini berguna untuk mengisi basis data dengan jumlah data yang dibutuhkan untuk menguji fungsionalitas aplikasi.

Contoh penggunaan seeder dan factory biasanya terkait dengan inisialisasi dan pengisian basis data selama fase pengembangan dan pengujian. *Seeder* membantu mengisi data statis, sementara *factory* membantu membuat data palsu secara dinamis. Kombinasi keduanya mempermudah pengembang untuk mengelola dan memasukkan data yang dibutuhkan untuk pengembangan dan pengujian aplikasi.

Contoh pada bahasa pemrograman ``Golang`` sebagai berikut:

**data.json**
```json
[
  {
    "name": "admin",
    "telp_number": "08123456789",
    "email": "admin1234@gmail.com",
    "password": "admin1234",
    "role": "admin",
    "is_verified": true
  },
  {
    "name": "user",
    "telp_number": "08123456789",
    "email": "user1234@gmail.com",
    "password": "user1234",
    "role": "user",
    "is_verified": true
  }
]
```

**list_user.go**
```go
func ListUserSeeder(db *gorm.DB) error {
	jsonFile, err := os.Open("data.json")
	if err != nil {
		return err
	}

	jsonData, _ := io.ReadAll(jsonFile)

	var listUser []entity.User
	if err := json.Unmarshal(jsonData, &listUser); err != nil {
		return err
	}

	hasTable := db.Migrator().HasTable(&entity.User{})
	if !hasTable {
		if err := db.Migrator().CreateTable(&entity.User{}); err != nil {
			return err
		}
	}

	for _, data := range listUser {
		var user entity.User
		err := db.Where(&entity.User{Email: data.Email}).First(&user).Error
		if err != nil && !errors.Is(err, gorm.ErrRecordNotFound) {
			return err
		}

		isData := db.Find(&user, "email = ?", data.Email).RowsAffected
		if isData == 0 {
			if err := db.Create(&data).Error; err != nil {
				return err
			}
		}
	}

	return nil
}
```

Berikut merupakan ``implementasi`` dari **seeder**

https://github.com/Caknoooo/go-gin-clean-architecture/assets/92671053/b869e62c-6e22-4069-bc71-e616ee2dc97e

## ORM
ORM merupakan singkatan dari *Object Relational Mapping*. ORM sendiri merupakan teknik yang merubah suatu tabel menjadi sebuah object yang nantinya mudah untuk digunakan.

<img src="image/Materi_3-API/3-3.png">

Object yang dibuat memiliki property yang sama dengan field-field yang ada pada suatu tabel. ORM memungkinakan kita melakukan query dan memanipulasi data di database menggunakan **Object oritented**. 

**Kenapa sih kok pakai ORM?**
Alasan utama menggunakan ORM adalah untuk mengurangi kesalahan ketika menuliskan query ke database kita. Karena memang terdapat aturan tersendiri dalam penulisan syntaxnya. Hal ini membuat para *developer* kesulitan dan bahkan memerlukan banyak waktu untuk melakukan query. Maka diperlukan ORM untuk mempermudah mengakses database tanpa melakukan query sama sekali.

**Kalau nggak pakai ORM bisa gak sih?**
Bisa kok, cuma kita diwajibkan untuk memahami seluruh query database dan harus benar-benar paham betul agar dapat membuat query yang oke dan bisa menyajikan data yang diperlukan. 

**Gimana cara kerja ORM?**
Karena kita udah tau penting atau tidaknya menggunakan ORM, cuss sekarang kita bakal bahas gimana **ORM** bekerja. Jadi awalnya, kita perlu mendefinisikan suatu *object*. Lalu kita buat field-field pada *object* sesuai dengan yang ada pada table di database Anda. Misalkan kita akan membuat *object* bernama makanan. FIeld yang dimiliki adalah id, nama, harga. Selanjutnya *object* tersebut dapat kita gunakan untuk melakukan CRUD tanpa menggunakan query. 

Contoh: 
```go
type Food struct {
   ID uint `gorm:”primaryKey”`
   Nama string 
   Price float64
}
```

**Dengan menggunakan ORM**
```go
var db *gorm.DB

// Create
food := Food(Name: “J one”, Price: 10.500)
db.Create(&food)

// Read
var retrievedFood Food
db.First(&retrievedFood, 1) // Mencari makanan dengan ID 1

// Update
db.Model(&retrievedFood).Update(“Price”, 12.00)

// Delete
db.Delete(&retrievedFood)
```

**Tanpa menggunakan ORM**
```go
db, err := sql.Open(<your database url>)

// Create
insertFoodQuery := “INSERT INTO foods(name, price) VALUES(?, ?)”
_, err = db.Exec(insertFoodQuery, “J one”, 10.500)
```

Pada bahasa pemrograman Golang, framework **ORM** yang umum digunakan adalah **GORM**

#### GORM

<img src="image/Materi_3-API/3-4.png">

**GORM** memiliki *built-in* database/sql packages. Memiliki fungsi auto migration, logging, context, association, dan masih banyak lainnya. Menggunakan pendekatan code-first, pembuatan model dapat menggunakan struct untuk berinteraksi dengan database seperti MySQL, SQLite, hingga PostgreSQL.

#### Kelebihan dan Kekurangan GORM

Kelebihan | Kekurangan 
:------ | :-------
Pendekatan Code-First | Penggunaan ORM secara garis besar dapat memperlambat aplikasi
Dapat support beberapa SQL seperti MySQL, SQLite, PostgreSQL dan masih banyak lainnya. | Penggunaannya pada query yang kompleks tidak mudah
Developer Friendly | Terdapat banyak pendeklarasian sehingga bisa sedikit membingungkan
Banyak Pengguna 

#### Implementasi GORM
Setelah kita memahami ORM yang bisa dibilang cukup membingungkan, sekarang saatnya kita kembali ke Visual Studio Code. Pertama kita perlu menginstall GORM dan driver database menggunakan command berikut

```sh
go get -u gorm.io/gorm
go get -u gorm.io/driver/postgres
```

Selanjutnya kita bisa menggunakan **GORM** dengan mengimport **GORM** serta driver dari database yang digunakan, dalam kasus ini akan menggunakan *postgres*

```go
import (
  "gorm.io/gorm"
  "gorm.io/driver/postgres"
)
```

Berikut adalah full kode nya pada file **main.go**
```go
import (
  "gorm.io/gorm"
  "gorm.io/driver/postgres"
)

type Product struct {
  gorm.Model
  Nama string
  Harga uint
}
 
func main() {
  dsn := "host=localhost user=postgres password=root dbname=oprec port=5432 TimeZone=Asia/Jakarta"
  db, err := gorm.Open(postgres.Open(dsn), &gorm.Config{})
  if err != nil {
    panic("failed to connect database")
  }

  // Migrate the schema
  err = db.AutoMigrate(&Product{})
  if err != nil {
    panic("failed to migrate schema")
  }

  // Create
  db.Create(&Product{Nama: "Lontong", Harga: 1000})

  // Read
  var product Product

  db.First(&product, 1) // find product with integer primary key

  db.First(&product, "nama = ?", "Lontong") // find product with nama D42

  // Update - update product's harga to 200
  db.Model(&product).Update("Harga", 2000)

  // Update - update multiple fields
  db.Model(&product).Updates(Product{Harga: 4000, Nama: "Cilok"}) // non-zero fields
  db.Model(&product).Updates(map[string]interface{}{"Harga": 5000, "Nama": "Cireng"})

  // Delete - delete product
  db.Delete(&product, 1)

  dbSQL, err  := db.DB()
  if err !=  nil {
    panic("failed to get db")
  }
 
  dbSQL.Close()
}
```

## Authentication
**Autentikasi** adalah pemastian bahwa user tersebut adalah user yang melakukan login saat itu juga. Autentikasi berbeda dengan autorisasi, autorisasi adalah pemastian bahwa seseorang dapat mengakses atau memiliki akses terhadap suatu endpoint. Terdapat banyak jenis autentikasi yang dapat diterapkan pada perangkat lunak, antara lain:

- **Basic Authentication**
*Basic Authentication* adalah metode autentikasi paling sederhana. Client akan mengirimkan username / email dan password mereka tanpa dienkripsi kepada server. Lalu server akan memastikan kebenaran kredensial tersebut. Username dan password tersebut akan disimpan di header dan diperlukan pada tiap request ke server.

- **Digest Authentication**
*Digest Authentication* serupa dengan Basic Authentication tetapi data seperti username dan password akan di enkripsi agar data lebih aman.

- **Session Based Authentication**
*Session Based Authentication* adalah autentikasi yang menyimpan state user di server, setelah user melakukan login, server akan memeriksa kredensial dan menghasilkan sebuah session. Session tersebut akan disimpan di dalam database dan juga browser (dalam bentuk sessionID di cookie). Metode ini lebih aman karena hanya mengirimkan id dari session. Akan tetapi, setiap user memerlukan autorisasi dan autentikasi. Server juga harus memeriksa session yang berkaitan di dalam database.

- **Token Based Authentiaction**
*Token Based Authentication* adalah sebuah autentikasi dengan menggunakan token dan yang paling umum digunakan saat ini. Salah satu jenis token yang umum digunakan adalah JSON Web Token (JWT). JWT menyimpan data header, payload, dan signature di dalanya untuk memastikan kredensial dari user. Data pada JWT diencode dengan menggunakan standar base64

- **One Time Password**
*One Time Password* atau biasa disebut OTP ini adalah metode yang cukup umum digunakan. OTP merupakan kode yang dibuat secara acak dan biasanya memiliki waktu kadaluwarsa (Time Based OTP). OTP biasanya digunakan pada sistem yang bersifat sensitif.

- **OAuth dan OpenID**
*OAuth* adalah metode yang memperbolehkan user login menggunakan single-sign-on (SSO) baik dari media sosial pihak ketiga lainnya. 

#### Middleware

<img src="image/Materi_3-API/3-5.png">

**Middleware** adalah konsep yang umumnya digunakan dalam pengembangan perangkat lunak, terutama dalam aplikasi web. Middleware adalah perangkat lunak yang berada di antara aplikasi atau sistem dan berbagai komponen seperti server atau database. Ini berfungsi untuk memproses atau memodifikasi permintaan dan tanggapan sebelum atau setelah mencapai tujuan akhirnya.

Dalam konteks aplikasi web, middleware digunakan untuk menangani berbagai tugas, seperti autentikasi, otorisasi, logging, caching, atau transformasi data. Middleware beroperasi pada tingkat perantara antara permintaan yang diterima dari klien dan tanggapan yang dikirimkan oleh server.

Berikut adalah beberapa konsep umum terkait middleware dalam pengembangan web:

1. **Autentikasi dan otorisasi**
Middleware dapat digunakan untuk memeriksa (autentikasi) dan hak akses (otorisasi) sebelum membiarkan akses ke bagian tertentu dari aplikasi.
2. **Logging**
Middleware logging dapat mencatat informasi tentang setiap permintaan yang masuk atau tanggapan yang keluar. Ini membantu dalam pemantauan dan pemecahan masalah.
3. **Caching**
Middleware dapat memberikan mekanisme caching untuk mempercepat kinerja dengan menyimpan sementara hasil permintaan yang sering digunakan.

Dalam banyak kerangka kerja web, *middleware* sering kali dapat dikonfigurasi dan digunakan dengan cara yang fleksibel. Pengembang dapat menyusun rantai middleware, di mana setiap lapisan middleware berkontribusi pada pemrosesan permintaan dan tanggapan secara berurutan.

Contoh kerangka kerja web yang menggunakan konsep middleware termasuk Express.js untuk Node.js, Django untuk Python, dan Laravel untuk PHP.


#### Authentication Implementation
Kita akan menggunakan **Token Based Authenticatin** menggunakan token JWT. Pada bahasa pemrograman Golang, kita akan menggunakan library https://github.com/golang-jwt. Untuk menginstall package tersebut anda hanya perlu membuka terminal anda dan menjalankan perintah berikut 

```sh
go get -u https://github.com/golang-jwt
```

Kode autentikasi akan dijadikan sebagai *middleware* yang menengahi antara router controller. Karena kita menggunakan framework Gin, maka return value dari fungsi middleware kita akan bertipe ``gin.HandlerFunc`` yang berupa sebuah fungsi yang menerima gin.Context. Token akan dibuat setiap user melakukan login. **Token** biasanya disimpan di dalam browser seperti di dalam (localstorage, cookies, dan sebagainya) dan memiliki prefix string “Bearer “ di depannya karena berupa bearer token. Sehingga saat menerima token, token tersebut memiliki format berikut:

```json
Bearer <token>
```

Oleh karena itu, kita harus memastikan bahwa string token sesuai dengan ketentuan. Berikut merupakan implementasi ``middleware`` autentikasi JWT

**authentication.go**

```go
package middleware

import (
  "net/http"
  "strings"

  "rpl-simple-backend/utils"
  "github.com/gin-gonic/gin"
)

func Authenticate(jwtService service.JWTService) gin.HandlerFunc {
  return func(ctx *gin.Context) {
    authHeader := ctx.GetHeader("Authorization")
    if authHeader == "" {
      response := utils.BuildResponseFailed("request proses failed", "token not found", nil)
      ctx.AbortWithStatusJSON(http.StatusUnauthorized, response)
      return
    }
    if !strings.Contains(authHeader, "Bearer ") {
      response := utils.BuildResponseFailed("request proses failed", "token not valid", nil)
      ctx.AbortWithStatusJSON(http.StatusUnauthorized, response)
      return
    }
    authHeader = strings.Replace(authHeader, "Bearer ", "", -1)
    token, err := jwtService.ValidateToken(authHeader)
    if err != nil {
      response := utils.BuildResponseFailed("request proses failed", "token not validate", nil)
      ctx.AbortWithStatusJSON(http.StatusUnauthorized, response)
      return
    }
    if !token.Valid {
      response := utils.BuildResponseFailed("request proses failed", "failed denied access", nil)
      ctx.AbortWithStatusJSON(http.StatusUnauthorized, response)
      return
    }
    ctx.Set("token", authHeader)
    ctx.Next()
  }
}
```

**jwtService.go**
```go
type JWTService interface {
  GenerateToken(userId string, role string) string
  ValidateToken(token string) (*jwt.Token, error)
  GetUserIDByToken(token string) (string, error)
}

type jwtCustomClaim struct {
  UserID string `json:"user_id"`
  Role   string `json:"role"`
  jwt.RegisteredClaims
}

type jwtService struct {
  secretKey string
  issuer    string
}

func NewJWTService() JWTService {
  return &jwtService{
    secretKey: getSecretKey(),
    issuer:    "Template",
  }
}

func getSecretKey() string {
  secretKey := os.Getenv("JWT_SECRET")
  if secretKey == "" {
    secretKey = "Template"
  }
  return secretKey
}

func (j *jwtService) GenerateToken(userId string, role string) string {
  claims := jwtCustomClaim{
    userId,
    role,
    jwt.RegisteredClaims{
      ExpiresAt: jwt.NewNumericDate(time.Now().Add(time.Minute * 120)),
      Issuer:    j.issuer,
      IssuedAt:  jwt.NewNumericDate(time.Now()),
    },
  }

  token := jwt.NewWithClaims(jwt.SigningMethodHS256, claims)
  tx, err := token.SignedString([]byte(j.secretKey))
  if err != nil {
    log.Println(err)
  }
  return tx
}

func (j *jwtService) parseToken(t_ *jwt.Token) (any, error) {
  if _, ok := t_.Method.(*jwt.SigningMethodHMAC); !ok {
    return nil, fmt.Errorf("unexpected signing method %v", t_.Header["alg"])
  }
  return []byte(j.secretKey), nil
}

func (j *jwtService) ValidateToken(token string) (*jwt.Token, error) {
  return jwt.Parse(token, j.parseToken)
}

func (j *jwtService) GetUserIDByToken(token string) (string, error) {
  t_Token, err := j.ValidateToken(token)
  if err != nil {
    return "", err
  }
 
  claims := t_Token.Claims.(jwt.MapClaims)
  id := fmt.Sprintf("%v", claims["user_id"])
  return id, nil
}
```

## Encryption & Hash

![Encryption vs Hash](image/Materi_3-API/1709020625039.png)

### Encryption

Encryption / Enkripsi adalah suatu hal yang digunakan untuk menyembunyikan sebuah data, bersifat sangat penting untuk menjamin keamanan dan melindungi dari serangan-serangan siber pihak tidak bertanggung jawab. Hal ini dilakukan tidak hanya pada data diam tetapi juga pada traffic data, bahkan sepenting itu hingga dibuatkan protokol web HTTPs yang merupakan protokol HTTP versi aman karena segala traffic data dari client dan server dienkripsi.

Enkripsi pada dasarnya bekerja dengan menggunakan suatu key untuk mengubah data awal atau yang biasa disebut plaintext menjadi sebuah encrypted text yang hanya dapat dikembalikan ke semula (decrypt) menggunakan suatu key khusus.

Terdapat 2 jenis enkripsi, yang pertama adalah Asymmetric Encryption yang key untuk enkripsi dan dekripsinya berbeda (Public Key & Private Key) sehingga lebih aman dan memungkinkan beberapa use case khusus dimana diperlukan 2 key berbeda, contoh algoritmanya adalah RSA. Kemudian yang kedua adalah Symmetric Encryption yang menggunakan key yang sama untuk kedua proses sehingga lebih cepat dan efisien, contoh algoritmanya adalah AES dan DES. Berbagai algoritma enkripsi pun memiliki keunggulan masing-masing dan harus dipilih yang paling sesuai berdasarkan use case yang diperlukan dan tipe data yang ingin kalian enkripsi (teks, file, dll).

Sebenarnya, saat ini menjadi sebuah best practice untuk melakukan enkripsi terhadap segala data pribadi atau sensitif pada database yang daftarnya bisa dilihat pada GDPR (EU) atau UU PDP sebagai berikut,

- GDPR (EU) : https://gdpr.eu/eu-gdpr-personal-data/
- UU PDP : http://www.wantiknas.go.id/id/berita/jenis-jenis-data-pribadi-menurut-uu-pdp-ini-rinciannya

Tetapi kebanyakan API saat ini, terlebih yang dibuat untuk skala kecil atau mengutamakan performa di atas segalanya belum menerapkan enkripsi pada data-data pribadi pengguna. Hal ini dikarenakan meskipun dampak positif enkripsi cukup banyak dan besar, tetapi menerapkan enkripsi pada banyak data di database juga mempunyai dampak negatif seperti penalti terhadap performa karena harus memakan waktu untuk melakukan dekripsi dan enkripsi setiap kali penambahan dan membaca data serta menyulitkan proses backup apabila terjadi bencana yang tidak diinginkan pada database.

Untuk contoh API dengan Gin (Golang) yang mengimplementasikan enkripsi pada data-data pribadi dapat dilihat <a href="https://github.com/Caknoooo/information-security-be">disini</a>.

### Hash

Bayangkan jika kalian mendaftarkan akun kalian ke suatu website, tetapi website tersebut menyimpan password yang kalian masukkan begitu saja sesuai apa yang kalian masukkan atau yang biasa disebut dengan plaintext. Tentunya kalian akan merasa tidak aman karena bukan hanya pemegang website yang dapat melihat password kalian, tetapi hacker dan juga orang-orang lain yang tidak bertanggung jawab pun dapat mendapatkan password kalian dengan mudah. Terlebih lagi kebanyakan orang saat ini menggunakan password yang sama untuk beberapa akun yang berbeda.

Karena hal tersebut, saat ini sudah menjadi suatu keharusan bagi penyimpanan password di database agar tidak menyimpan dalam bentuk plaintext melainkan menggunakan sesuatu yang disebut dengan hashing untuk menyembunyikan password aslinya. Mengapa tidak menggunakan encryption saja? Berbeda dengan encryption, hashing adalah fungsi satu arah (one-way function) sehingga tidak memungkinkan bagi kita untuk melakukan "decrypt" terhadap hasil dari hash. Hal ini sangat sesuai dengan kebutuhan kita dalam menyimpan password karena kita tidak pernah membutuhkan bahkan tidak boleh mengetahui nilai aslinya. Yang kita perlukan hanyalah untuk melakukan komparasi antara nilai hasil hash yang kita simpan dengan password yang dimasukkan pengguna ketika ingin melakukan login, dimana fungsi tersebut dapat dilakukan dengan hashing. Meskipun bukan tidak mungkin untuk melakukan crack terhadap password yang dihash karena dapat dilakukan penyerangan dengan metode brute force, tetapi itu tetaplah jauh lebih sulit dibandingkan dengan melakukan dekripsi terhadap password yang dienkripsi dengan suatu key.

Untuk lebih meningkatkan keamanan dari hashing, maka biasanya dilakukan penambahan menggunakan sesuatu yang disebut dengan salt. Salt sendiri adalah karakter-karakter tambahan yang secara spesifik dibuat untuk ditambahkan ke password sebelum dilakukan hashing.

Terdapat banyak algoritma untuk hashing, ada yang cepat dan juga yang lambat. Contoh algoritma yang cepat seperti SHA1 dan MD5. Tetapi dalam hal penyimpanan password, algoritma hash yang cepat akan mempermudah hacker untuk melakukan crack terhadap password kita dengan serangan brute force tadi karena tidak perlu waktu lama untuk melakukannya. Maka dari itu, saat ini algoritma yang sering digunakan untuk melakukan hashing terhadap password yang akan disimpan pada database adalah algoritma yang lebih lambat seperti bcrypt, Argon2, dan Scrypt.

Kali ini kita akan bahas salah satu algoritma hashing yang cukup sering digunakan yaitu bcrypt.

#### bcrypt

bcrypt merupakan algoritma hash adaptif bersifat round-based yang dirancang oleh 2 peneliti keamanan komputer terkemuka Niels Provos dan David Mazières berdasarkan cipher Blowfish yang menjadi asal mula huruf b digunakan. Algoritma ini mengembangkan teknik salt dengan menambahkan random salt pada setiap password yang membuatnya menjadi lebih aman.

![bcrypt Hash](image/Materi_3-API/1709021554900.png)

Alg adalah versi dari algoritma bcrypt yang digunakan. Cost atau jumlah round default dari bcrypt adalah 10 yang dinilai cukup aman ketika algoritma ini dibuat. Tetapi karena hardware kini semakin canggih dan cepat, untuk meningkatkan keamanan jumlah round juga dapat dinaikkan meskipun tentu akan memperlama proses hash. Pemilihan jumlah round juga perlu diperhatikan karena pada bcrypt semakin tinggi costnya akan semakin lama waktu yang diperlukan, bisa dilihat pada grafik berikut,

![Grafik bcrypt](image/Materi_3-API/1709021894010.png)

Berikut adalah contoh implementasi library bcrypt pada bahasa Go:

```go
import (
	"golang.org/x/crypto/bcrypt"
)

// Hash
func HashAndSalt(p string) (string, error) {
	pwd := []byte(p)
	hash, err := bcrypt.GenerateFromPassword(pwd, bcrypt.MinCost)
	if err != nil {
		return "", err
	}
	return string(hash), nil
}

// Validate
func ComparePassword(hashedPwd string, plainPassword []byte) (bool, error) {
	byteHash := []byte(hashedPwd)
	err := bcrypt.CompareHashAndPassword(byteHash, plainPassword)
	if err != nil {
		return false, err
	}
	return true, nil
}
```

Untuk perbandingan, berikut kami sertakan juga contoh implementasi pada JavaScript:

```js
const bcrypt = require("bcrypt");

// Hash
bcrypt
  .hash(plainTextPassword1, saltRounds)
  .then((hash) => {
    console.log(`Hash: ${hash}`);
  })
  .catch((err) => console.error(err.message));

// Validate
bcrypt
  .compare(plainTextPassword1, hash)
  .then((res) => {
    console.log(res);
  })
  .catch((err) => console.error(err.message));
```

## Authorization

Bila Authentication / Autentikasi adalah tentang memastikan seorang pengguna dengan kredensial yang sesuai dapat masuk dan menggunakan service yang tersedia, Authorization / Otorisasi adalah menentukan bagian yang dapat atau tidak dapat diakses oleh seorang pengguna yang telah terverifikasi. Maka, bila membicarakan tentang authorization kurang lebih sama dengan membicarakan tentang role karena bisanya authorization diterapkan dalam bentuk role atau yang disebut dengan Role-based Authorization atau Role-based Access Control (RBAC).

![RBAC](image/Materi_3-API/1709027733788.png)

Bentuk Authorization paling simple dan sering ditemui ialah adanya role User dan Admin yang dapat digambarkan dengan atribut `is_admin` pada entitas User. Tentu bila bernilai 1 maka menandakan pengguna tersebut adalah Admin dan berlaku sebaliknya. Penerapan dari atribut ini sendiri ada pada admin yang akan diarahkan ke halaman admin dashboard segera setelah masuk dan mempunyai hak serta izin untuk melakukan aksi-aksi admin seperti menampilkan seluruh pengguna, menghapus pengguna, dan lain-lain bergantung dari aplikasinya. Di sisi lain, User biasa akan diarahkan ke halaman khusus pengguna dengan izin yang terbatas hanya bagi pengguna sehingga tidak dapat menggunakan fitur admin yang bila dipaksakan akan mengembalikan HTTP status 401 (Unauthorized).

Tentunya tidak hanya terbatas disana, role yang disediakan bisa sangatlah banyak bergantung dari kebutuhan aplikasi sehingga atribut `is_admin` haruslah diganti menjadi atribut `role` atau bahkan tabel `role` tersendiri yang dapat mengakomodasi banyak peran. Sebagai contoh, untuk sebuah organisasi besar yang terdiri dari beberapa divisi seperti Marketing, Bisnis, Teknologi, Pengembangan Produk, dan lain-lain. Tentunya akses serta fitur yang diperlukan oleh masing-masing divisi akan berbeda bergantung dari jobdesc mereka masing-masing sehingga bagi yang tidak berkebutuhan dilarang untuk mengakses bagian tertentu.

Misalkan terdapat beberapa role yang ingin dibuat untuk mempunyai akses di suatu fitur yang sama sehingga terjadi irisan, kita juga bisa membuat tabel `permission` yang kemudian terhubung dengan tabel `role` melalui suatu tabel semacam `role has permission`. Untuk penerapannya sendiri dapat digunakan middleware ataupun tidak, sebagai contoh pada aplikasi yang dibuat dengan Laravel (PHP) akan berbentuk kurang lebih sebagai berikut,

```php
// Melalui Middleware
Route::group(['middleware' => ['role:admin']], function () {});

Route::group(['middleware' => ['permission:create post']], function () {});

// Tanpa Middleware
if ($user->hasRole('admin')) {}

if ($user->hasPermissionTo('create post')) {}
```

Untuk penerapan middleware pada Authorization menggunakan Gin (Golang) sebagai berikut,

```go
routes := route.Group("/api/url_shortener")
{
  routes.POST("", urlShortenerController.Create)

  routes.GET("/:id", middleware.Authenticate(jwtService, userServices, roleHasPermissionService, permissionService, "user_url_shortener.index"), urlShortenerController.GetUrlShortenerPrivate)

  routes.GET("/public/:id", urlShortenerController.GetUrlShortenerPublic)

  routes.DELETE("/:id", middleware.Authenticate(jwtService, userServices, roleHasPermissionService, permissionService, "user_url_shortener.delete"), urlShortenerController.DeleteUrlShortener)

  routes.PATCH("/:id", middleware.Authenticate(jwtService, userServices, roleHasPermissionService, permissionService, "user_url_shortener.update"), urlShortenerController.UpdateUrlShortener)
}
```

## File Handling

Kebanyakan aplikasi saat ini memerlukan penyimpanan file untuk menjalankan fitur-fiturnya, termasuk dengan menampilkan file yang disimpan tadi. File-file yang diperlukan pun bisa beragam jenisnya, bisa berupa pdf, gambar, video, dan lain-lain. Maka dari itu, kita sebagai Backend Developer pun harus tau cara melakukannya.

Untuk dapat mengupload file ke API, content type yang harus dikirim dari pengguna atau sisi Frontend pada header request HTTP akan berbeda dari biasanya, yakni umumnya menggunakan tipe `multipart/form-data`. Untuk menerima request dalam bentuk file tersebut juga harus menggunakan tipe data tertentu, seperti misalnya dalam Golang dapat digunakan `*multipart.FileHeader`.

Secara umum, terdapat 4 pilihan metode yang dapat kita gunakan untuk menyimpan dan menampilkan file melalui API yang akan dibahas pada bagian ini.

### Database

Kita dapat menyimpan file dalam database, hal ini karena pada dasarnya sebuah file baik itu pdf, gambar, maupun video terdiri dari banyak karakter-karakter sehingga dapat disimpan dalam atribut bertipe TEXT. Bahkan database juga ada yang mempunyai tipe data khusus untuk menyimpan file seperti VARBINARY atau FILESTREAM pada SQL Server.

Tetapi cara ini sangat jarang digunakan, mengapa? Tentunya karena cara ini mempunyai dampak negatif walaupun juga terdapat dampak positifnya. Salah satunya adalah file memakan jumlah penyimpanan yang besar sehingga sangat tidak direkomendasikan untuk menggunakan database yang jauh lebih mahal dibanding filesystem. Kemudian, dari segi performa, read dan write terhadap database lebih lambat dibandingkan filesystem sehingga tentu akan berakibat buruk bagi aplikasi. Meskipun begitu, ada juga dampak positif dari penyimpanan di database seperti properti ACID dan keamanan yang terjamin, penyimpanan lebih tersentralisasi, backup lebih mudah, dan lain-lain.

Karena dampak negatifnya yang cukup berat, maka database biasanya dipergunakan untuk menyimpan path atau id dari file yang disimpan di tempat lain saja.

### Server Storage

Nah, opsi yang lebih baik dan lebih sering digunakan adalah menyimpan file pada Server Storage atau Filesystem dari Server itu sendiri. Apabila menggunakan cara ini, maka database akan kita gunakan untuk menyimpan file path tempat suatu file spesifik disimpan pada filesystem server kita.

Dengan cara ini, API kita tinggal mengupload file ke server dan menyimpan file path tempat file berada pada database. Kemudian bila nanti file perlu untuk ditampilkan, API kita tinggal melakukan read terhadap file berdasarkan file path yang disimpan di database.

Kelebihan dari cara ini adalah read / write yang lebih cepat dan harga yang lebih murah dari database. Tetapi untuk cara ini, kita harus menyiapkan server yang cukup besar dari segi storage agar bisa menampung file yang cukup banyak dan harus selalu melakukan maintenance, menjaga keamanan, dan mengurus skalabilitas dari server kita sendiri.

Berikut adalah contoh implementasi upload file dan juga read file di filesystem server pada Golang:

```go
// Upload File
err = os.WriteFile(filePath, fileData, 0666)
if err != nil {
  return err
}

// Read File
_, err := os.Stat(filePath)
if err != nil {
  return err
}
```

### Cloud Storage

Opsi terakhir yang juga sering digunakan adalah menggunakan Cloud Storage untuk menyimpan file-file kita. Contoh service yang tersedia dan sering digunakan seperti Amazon S3, Google Buckets, dan Cloudinary. Nantinya, yang kita simpan pada database kita bisa berupa id dari file pada cloud storage atau langsung berupa link yang dapat digunakan untuk mengakses suatu file spesifik pada cloud storage kita.

Dengan cara ini, API kita akan melakukan upload ke Cloud Storage yang kita telah siapkan dan menyimpan ID / Linknya untuk disimpan pada database. Untuk menampilkan file, kita bisa mendapatkan file melalui Cloud Storage berdasarkan yang kita simpan pada database tadi.

Kelebihan dari cara ini adalah harga yang cenderung lebih murah daripada membeli suatu physical server dengan size fix karena cloud storage biasanya menyediakan harga yang sesuai dengan pemakaian terkini, sehingga cara ini cocok apabila kita ingin menggunakan server yang tidak terlalu besar dalam segi storage. Selain itu, untuk masalah keamanan, skalabilitas, dan maintenance juga akan dihandle sepenuhnya oleh pihak mereka sehingga kita tidak perlu pusing mengurusnya.

Akan tetapi, kelebihan yang kedua justru dapat menjadi fatal apabila kita salah memilih service karena sebenarnya bergantung pada third-party tidaklah baik karena ketika mereka mengalami down, sistem kita juga tidak akan bisa berfungsi dengan baik dan kita pun tidak bisa berbuat apa-apa selain menunggu mereka memperbaiki masalah di sisi mereka. Maka dari itu, kita harus benar-benar memilih third-party service client yang terpercaya dan memiliki nama yang baik.

Contoh implementasi upload dan download file dari cloud storage yang dalam hal ini adalah Google Bucket dari Google Cloud Platform (GCP) dengan menggunakan bahasa yang sama untuk komparasi yakni Go sebagai berikut,

```go
// Upload File
func UploadToCloud(file *multipart.FileHeader, path string) error {
	storageClient, err := storage.NewClient(context.TODO(), option.WithCredentialsFile("keys.json"))
	if err != nil {
		return err
	}
	defer storageClient.Close()

	storageWriter := storageClient.Bucket(bucketName).Object(path).NewWriter(context.TODO())
	defer storageWriter.Close()

	openedFile, err := file.Open()
	if err != nil {
		return err
	}

	if _, err := io.Copy(storageWriter, openedFile); err != nil {
		return err
	}

	return nil

}

// Download File
func DownloadFromCloud(srcPath string, dstPath string) error {
	fmt.Println(srcPath)
	f, err := os.Create(dstPath)
	if err != nil {
		return err
	}

	storageClient, err := storage.NewClient(context.TODO(), option.WithCredentialsFile("keys.json"))
	if err != nil {
		return err
	}

	storageReader, err := storageClient.Bucket(bucketName).Object(srcPath).NewReader(context.TODO())
	if err != nil {
		return err
	}
	defer storageReader.Close()

	if _, err := io.Copy(f, storageReader); err != nil {
		return err
	}

	if err = f.Close(); err != nil {
		return err
	}

	return nil
}
```

## Pemilihan

Nah, sekarang kita masuk ke pertanyaan yang juga cukup membuat bingung para developer yakni bagaimana cara memilih tech stack terbaik untuk digunakan dalam membuat API. Jawabannya tidak berbeda jauh dari pemilihan database di materi yang lalu, yakni dengan melakukan analisis terhadap kebutuhan aplikasi kalian terlebih dahulu. Untuk mengetahuinya, coba jawab beberapa pertanyaan berikut,

1. Apakah aplikasi berskala besar dan pembangunannya membutuhkan bantuan banyak library?
2. Apakah aplikasi memerlukan struktur OOP yang kompleks?
3. Seberapa banyak kira-kira jumlah pengguna konkuren maksimal yang akan menggunakan aplikasi ini?
4. Seberapa baik performa yang dibutuhkan pada aplikasi ini?
5. Apakah aplikasi perlu untuk diintegrasikan dengan sesuatu yang lain?

Skala aplikasi dan kebutuhan library akan mempengaruhi pemilihan tech stack karena beberapa framework seperti Laravel memiliki banyak built-in library yang sangat memudahkan developer untuk membangun aplikasi, begitu juga dengan bahasa-bahasa seperti Python dan JavaScript yang juga memiliki banyak library eksternal sehingga sangat memudahkan. Hal ini berbeda dengan bahasa seperti C dan Golang yang cukup mengharuskan developer untuk menulis sendiri kode yang diperlukan karena tidak semudah itu mencari library yang sesuai. Perlu diingat dalam menggunakan library harus dipastikan asal, pembuat, serta status maintenancenya karena penggunaan library yang kurang dimaintain oleh penggunanya akan buruk bagi aplikasi di masa depan.

Pilihan bahasa yang dapat digunakan untuk membangun sebuah API juga terbagi menjadi beberapa jenis sehingga perlu dijadikan bahan pertimbangan pula. Misalkan aplikasi memerlukan struktur OOP yang kompleks, maka sebaiknya menggunakan bahasa bertipe Object-Oriented seperti PHP atau Python daripada bahasa bertipe Procedural seperti Golang atau C. Ada juga jenis lain yang perlu diperhatikan yakni pemilihan bahasa tipe interpreted dan compiled yang bisa dicari kelebihan serta kekurangannya melalui internet seperti pada <a href="https://forum.upbatam.ac.id/index.php/cbis/article/view/6583">artikel ini</a>.

Jumlah pengguna konkuren dan performa yang dibutuhkan juga merupakan faktor penting dalam memilih tech stack karena setiap tech stack memiliki performa dan kemampuan menghandle concurrency yang berbeda. Untuk mengetahui mana yang terbaik, kalian bisa mencari informasi melalui internet, misalkan melalui komparasi performa benchmarking seperti yang dapat dilihat <a href="https://medium.com/deno-the-complete-reference/list-of-all-of-my-real-world-performance-comparisons-a8e9182ac50">disini</a>.

Kemudian, hal-hal yang perlu diintegrasikan pada aplikasi juga berpengaruh, seperti misalnya untuk aplikasi mobile kebanyakan menggunakan Firebase karena kesesuaian dan kemudahan dalam proses pembangunan aplikasi. Selain itu, misalkan aplikasi memerlukan integrasi dengan ML atau AI, maka seringkali menggunakan framework dari bahasa Python seperti FastAPI untuk mempermudah pekerjaan.

Satu hal lagi yang tidak kalah penting adalah penyesuaian tech stack kemampuan dan pengalaman tim developernya, karena tentu diperlukan kemampuan yang mumpuni serta pengalaman yang cukup untuk bisa membangun aplikasi yang baik, terampil, dan terjamin efektivitasnya. Tetapi, seorang developer yang terikat ke 1 tech stack saja tentunya kurang baik karena kurang dalam segi fleksibilitas, sehingga diperlukan pemahaman secara fundamental dan bukan hanya pemahaman pada tech stack tertentu untuk menjadi seorang developer yang handal dan mampu beradaptasi dengan tech stack apapun.

Untuk detail lebih lanjutnya dalam mencari tech stack yang terbaik tentu bergantung pada kebutuhan masing-masing sehingga harus dilakukan riset mendalam sebelum fiksasi tech stack dengan membaca-baca pendapat profesional, artikel, dokumen, maupun internet terkait kelebihan dan kekurangan masing-masing tech stack seperti yang ada pada <a href="https://techwings.com/blog/backend-frameworks-ultimate-comparison">web ini</a>.

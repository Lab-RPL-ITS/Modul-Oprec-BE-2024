# API

## Daftar Isi

- [Daftar Isi](#daftar-isi)
- [Pengantar](#pengantar)
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
- [Hash & Encryption](#hash--encryption)
- [Authorization](#authorization)
- [Store & View File](#store--view-file)
- [Pemilihan](#pemilihan)

## Pengantar

## Definisi & Fungsi

### Definisi

### Fungsi

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

**Autentikasi**
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

## Hash & Encryption

## Authorization

## Store & View File

## Pemilihan

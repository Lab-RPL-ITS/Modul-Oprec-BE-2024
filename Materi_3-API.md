# API

## Daftar Isi

- [Daftar Isi](#daftar-isi)
- [Pengantar](#pengantar)
- [Definisi & Fungsi](#definisi--fungsi)
  - [Definisi](#definisi)
  - [Fungsi](#fungsi)
- [Macam Arsitektur](#macam-arsitektur)
- [Contoh Tech Stack](#contoh-tech-stack)
- [Terkait Database](#terkait-database)
  - [Integrasi](#integrasi)
  - [Migration](#migration)
  - [Seeder & Factory](#seeder--factory)
- [ORM](#orm)
- [Authentication](#authentication)
- [Encryption & Hash](#encryption--hash)
  - [Encryption](#encryption)
  - [Hash](#hash)
- [Authorization](#authorization)
- [File Handling](#file-handling)
  - [Database](#database)
  - [Server Storage](#server-storage)
  - [Cloud Storage](#cloud-storage)
- [Pemilihan](#pemilihan)

## Pengantar

## Definisi & Fungsi

### Definisi

### Fungsi

## Macam Arsitektur

## Contoh Tech Stack

## Terkait Database

### Integrasi

### Migration

### Seeder & Factory

## ORM

## Authentication

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

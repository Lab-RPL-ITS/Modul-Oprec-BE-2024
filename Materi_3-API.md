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
- [Store & View File](#store--view-file)
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

## Store & View File

## Pemilihan

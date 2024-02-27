# Extras

## Daftar Isi

- [Daftar Isi](#daftar-isi)
- [Arsitektur Codebase](#arsitektur-codebase)
- [Automation Test](#automation-test)
  - [Unit Test](#unit-test)
  - [Integration / Feature Test](#integration--feature-test)
  - [Functional / Acceptance Test](#functional--acceptance-test)
- [Dokumentasi](#dokumentasi)

## Arsitektur Codebase

## Automation Test

Dalam proses pembangunan aplikasi, khususnya ketika penulisan kode, bagaimana cara memastikan bahwa kode yang baru kamu tulis berjalan dengan lancar tanpa merusak satupun fungsional dari kode lain yang sudah ada sebelumnya? Mungkin bila jumlah fitur dan use case yang tersedia masih sedikit, cara percobaan secara manual di local akan bekerja dengan baik. Tetapi bagaimana bila fitur dan use case yang tersedia sudah sangat banyak? Tentu testing terhadap keseluruhan aplikasi akan menjadi hal yang paling menghabiskan waktu dari penambahan suatu fitur atau perbaikan bug.

Untuk mencegah hal tersebut, maka kita harus memahami dan mengaplikasikan automation testing ke dalam keseluruhan fase pengembangan aplikasi kita. Automation Testing adalah pengujian secara otomatis yang dilakukan oleh komputer untuk menentukan apakah sesuatu yang dalam hal ini adalah program, sudah berjalan dengan baik sesuai harapan atau tidak dengan melakukan simulasi terhadap kode yang telah kita tulis.

![Test Hierarchy](image/Materi_4-Extras/1709046894897.png)

Suatu kultur dalam pengembangan aplikasi yang mengaplikasikan automation test pada keseluruhan codebase disebut TDD (Test Driven Development). Sesuai dengan gambar di atas, Automation Test terdiri dari 3 jenis test yang akan dibahas pada bagian ini.

### Unit Test

Unit Test adalah testing yang fokusnya hanya pada 1 unit saja, sehingga tidak perlu memperhatikan hasil kolaborasi dengan kode lain karena ada bagiannya sendiri nanti. Pada test ini, kita cukup fokus secara spesifik pada 1 bagian sehingga bila terjadi kegagalan kita langsung mengetahui bahwa masalahnya ada pada bagian tersebut.

Berikut adalah contoh implementasi Unit Test pada API dengan tech stack Laravel (PHP) pada fitur sinkronisasi dari tabel `medical_test_statistics` yang menyimpan statistik berkaitan dengan isi dari entitas MedicalTest,

```php
class MedicalTestSyncTest extends SQLiteTestCase
{
  use MigrateAnalyticsDatabase;

  protected Collection $cities;

  protected Model $subdistrict;

  protected Collection $users;

  public function setUp(): void
  {
    parent::setUp();

    $this->subdistrict = Subdistrict::factory()->create();
    $this->users = User::factory(5)->create([
      'subdistrict_id' => $this->subdistrict->id,
    ]);
    $status = MedicalTest::getStatesFor('status')->toArray();

    $this->users->each(function ($user, $index) use ($status) {
      MedicalTest::factory()->create([
          'user_id' => $user->id,
          'status' => $status[$index % 3],
      ]);
    });
  }

  /* @test */
  public function test_medical_test_sync(): void
  {
    (new MedicalTestAnalyticService())->sync();
    $this->assertDatabaseHas('medical_test_statistics', [
      'city_id' => $this->subdistrict->city->id,
      'city_name' => $this->subdistrict->city->name,
      'total_students' => $this->users->count(),
      'total_unverified' => 2,
      'total_invalid' => 2,
      'total_valid' => 1,
    ]);
  }
}
```

Sebagai perbandingan, berikut adalah contoh Unit Test untuk fitur penambahan / registrasi pengguna pada API dengan tech stack HapiJS (JavaScript) menggunakan Jest,

```js
describe("AddUserUseCase", () => {
  it("should orchestrating the add user action correctly", async () => {
    // Arrange
    const useCasePayload = {
      username: "uname",
      password: "secret",
      fullname: "Full Name",
    };

    const expectedRegisteredUser = new RegisteredUser({
      id: "user-123",
      username: useCasePayload.username,
      fullname: useCasePayload.fullname,
    });

    const mockUserRepository = new UserRepository();
    const mockPasswordHash = new PasswordHash();

    mockUserRepository.verifyAvailableUsername = jest.fn(() => Promise.resolve());
    mockPasswordHash.hash = jest.fn(() => Promise.resolve("encrypted_password"));
    mockUserRepository.addUser = jest.fn(() =>
      Promise.resolve(
        new RegisteredUser({
          id: "user-123",
          username: useCasePayload.username,
          fullname: useCasePayload.fullname,
        })
      )
    );

    const getUserUseCase = new AddUserUseCase({
      userRepository: mockUserRepository,
      passwordHash: mockPasswordHash,
    });

    // Action
    const registeredUser = await getUserUseCase.execute(useCasePayload);

    // Assert
    expect(registeredUser).toStrictEqual(expectedRegisteredUser);
    expect(mockUserRepository.verifyAvailableUsername).toBeCalledWith(useCasePayload.username);
    expect(mockPasswordHash.hash).toBeCalledWith(useCasePayload.password);
    expect(mockUserRepository.addUser).toBeCalledWith(
      new UserRegister({
        username: useCasePayload.username,
        password: "encrypted_password",
        fullname: useCasePayload.fullname,
      })
    );
  });
});
```

### Integration / Feature Test

Integration / Feature Test adalah testing yang fokusnya terhadap suatu fitur lengkap yang merupakan bentuk atau hasil kolaborasi antar beberapa unit berbeda. Tujuannya untuk memastikan keharmonisan antara setiap unit yang menjadi satu set penyusun suatu fitur.

Berikut adalah contoh implementasi Integration / Feature Test pada API dengan tech stack Laravel (PHP), masih pada fitur sinkronisasi dari tabel `medical_test_statistics` yang menyimpan statistik berkaitan dengan isi dari entitas MedicalTest,

```php
class AnalyticMedicalTestTest extends BaseBackOfficeTestCase
{
    use MigrateAnalyticsDatabase;

    protected function getResourcePath(): string
    {
        return parent::getResourcePath().'/analytic/medical-tests';
    }

    public function setUp(): void
    {
        parent::setUp();

        $this->model = MedicalTestStatistic::factory()->create(); // @phpstan-ignore-line
    }

    /* @test */
    public function test_index_analytic_medical_tests(): void
    {
        $this->response = $this->withToken($this->token)->getJson($this->getEndpoint());
        $this->assertApiCollection($this->model->toArray());
    }
}
```

Sebagai perbandingan juga, berikut adalah contoh Integration / Feature Test untuk fitur penambahan / registrasi pengguna pada API dengan tech stack HapiJS (JavaScript) menggunakan Jest,

```js
describe("Users endpoints", () => {
  afterAll(async () => {
    await pool.end();
  });

  afterEach(async () => {
    await UsersTableTestHelper.cleanTable();
    await AuthenticationsTableTestHelper.cleanTable();
  });

  describe("when POST /authentications", () => {
    it("should respond with status code 201 and new authentication", async () => {
      // Arrange
      const requestPayload = {
        username: "uname",
        password: "secret",
      };
      const server = await createServer(container);
      await server.inject({
        method: "POST",
        url: "/users",
        payload: {
          username: "uname",
          password: "secret",
          fullname: "Full Name",
        },
      });

      // Action
      const response = await server.inject({
        method: "POST",
        url: "/authentications",
        payload: requestPayload,
      });

      // Assert
      const responseJson = JSON.parse(response.payload);
      expect(response.statusCode).toEqual(201);
      expect(responseJson.status).toEqual("success");
      expect(responseJson.data.accessToken).toBeDefined();
      expect(responseJson.data.refreshToken).toBeDefined();
    });
  });
});
```

### Functional / Acceptance Test

Functional / Acceptance Test atau yang biasa juga dikenal sebagai End-to-End Test (E2E Test) merupakan testing dengan tingkat tertinggi yang berfokus pada keseluruhan aplikasi, tujuannya memastikan aplikasi secara keseluruhan berjalan dengan baik. Biasanya dijalankan dalam bentuk simulasi pada platform yang sama dengan yang digunakan oleh pengguna nantinya.

Contohnya bila mengembangkan aplikasi web, maka Functional / Acceptance Test harus dijalankan pada browser. Jika Anda mengembangkan aplikasi Mobile, Functional / Acceptance Test harus dijalankan pada device Mobile Phone. Dalam pembuatan API sendiri, Functional / Acceptance Test dapat dijalankan menggunakan fitur testing dari Postman.

![Postman Test](image/Materi_4-Extras/1709054638749.png)

Apa tuh Postman? Nah, pas banget, akan kita bahas di materi berikutnya.

### Contoh Implementasi

Untuk lebih jelasnya terkait TDD (Test Driven Development), contoh penerapannya dalam sebuah codebase API yang menerapkan Clean Architecture dan dibangun dengan tech stack HapiJS (JavaScript) serta dilengkapi automation testing menggunakan Jest dapat dilihat <a href="https://github.com/zetsux/garuda-forum-api">disini</a>.

## Dokumentasi

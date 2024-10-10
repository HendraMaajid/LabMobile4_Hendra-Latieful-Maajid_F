# tokokita

Nama : Hendra Latieful Maajid

NIM : H1D022018

Shift KRS: D

Shift Baru: F

Tugas pertemuan 4 dan 5 (Tugas pertemuan 5 ada dibawah tugas pertemuan 4)
## Screenshot TUGAS PERTEMUAN 4
<img src="login.png" alt="Login Screenshot" width="200"/><img src="registrasi.png" alt="Registrasi Screenshot" width="200"/>
<img src="list_produk.png" alt="List Produk Screenshot" width="200"/><img src="tambah.png" alt="Tambah Produk Screenshot" width="200"/>
<img src="edit.png" alt="Edit Produk Screenshot" width="200"/><img src="delete.png" alt="Delete Produk Screenshot" width="200"/>
<img src="logout.png" alt="Logout Screenshot" width="200"/>


# TUGAS PERTEMUAN 5

Terdapat folder yang namanya helpers yang dimana folder ini berisi kelas-kelas dan fungsi-fungsi utilitas yang digunakan di seluruh proyek Flutter untuk membantu tugas-tugas umum seperti komunikasi API, penanganan pengecualian, dan manajemen informasi pengguna.

## Isi

1. [Api.dart](#apidart)
2. [ApiUrl.dart](#apiurldart)
3. [app_exception.dart](#app_exceptiondart)
4. [user_info.dart](#user_infodart)

### Api.dart

File ini berisi kelas `Api` yang menyediakan metode-metode untuk melakukan permintaan HTTP ke API backend. Termasuk:

- Metode POST, GET, PUT, dan DELETE
- Penyertaan token otomatis dalam header permintaan
- Penanganan kesalahan untuk masalah jaringan
- Penguraian respons dan pemetaan kesalahan

### ApiUrl.dart

Kelas `ApiUrl` dalam file ini mendefinisikan konstanta dan metode untuk membangun URL endpoint API. Termasuk:

- Konfigurasi URL dasar
- Definisi endpoint untuk berbagai rute API (registrasi, login, operasi produk)
- Metode untuk menghasilkan URL dengan parameter dinamis (misalnya, ID produk)

### app_exception.dart

File ini mendefinisikan kelas-kelas pengecualian kustom untuk menangani berbagai jenis kesalahan yang mungkin terjadi selama komunikasi API:

- `AppException`: Kelas pengecualian dasar
- `FetchDataException`: Untuk kesalahan komunikasi umum
- `BadRequestException`: Untuk permintaan yang tidak valid (kode status 400)
- `UnauthorisedException`: Untuk kesalahan autentikasi/otorisasi (kode status 401/403)
- `UnprocessableEntityException`: Untuk kesalahan validasi (kode status 422)
- `InvalidInputException`: Untuk data input yang tidak valid

### user_info.dart

Kelas `UserInfo` dalam file ini menyediakan metode untuk mengelola informasi terkait pengguna menggunakan SharedPreferences:

- Mengatur dan mendapatkan token autentikasi pengguna
- Mengatur dan mendapatkan ID pengguna
- Keluar (menghapus data tersimpan)

# A. Penjelasan Proses Registrasi 

## 1. Proses Submit
<img src="registrasi1.png" width="300"/>

Ketika tombol "Registrasi" ditekan, method `_submit()` dipanggil:

```dart
void _submit() {
  _formKey.currentState!.save();
  setState(() {
    _isLoading = true;
  });
  RegistrasiBloc.registrasi(
    nama: _namaTextboxController.text,
    email: _emailTextboxController.text,
    password: _passwordTextboxController.text
  ).then((value) {
    showDialog(
      context: context,
      barrierDismissible: false,
      builder: (BuildContext context) => SuccessDialog(
        description: "Registrasi berhasil, silahkan login",
        okClick: () {
          Navigator.pop(context);
        },
      )
    );
  }, onError: (error) {
    showDialog(
      context: context,
      barrierDismissible: false,
      builder: (BuildContext context) => const WarningDialog(
        description: "Registrasi gagal, silahkan coba lagi",
      )
    );
  });
  setState(() {
    _isLoading = false;
  });
}
```


## 2. Blok Registrasi (RegistrasiBloc)

`RegistrasiBloc` menangani logika bisnis untuk proses registrasi:

```dart
class RegistrasiBloc {
  static Future<Registrasi> registrasi({String? nama, String? email, String? password}) async {
    String apiUrl = ApiUrl.registrasi;
    var body = {"nama": nama, "email": email, "password": password};
    var response = await Api().post(apiUrl, body);
    var jsonObj = json.decode(response.body);
    return Registrasi.fromJson(jsonObj);
  }
}
```

1. Method `registrasi()` dipanggil dengan parameter nama, email, dan password.
2. URL API untuk registrasi diambil dari `ApiUrl.registrasi`.
3. Data registrasi disiapkan dalam bentuk map.
4. Permintaan POST dikirim ke API menggunakan `Api().post()`.
5. Respons dari API di-decode dari JSON.
6. Objek `Registrasi` dibuat dari data JSON yang diterima.


## 3. Model Registrasi (Registrasi)

Model `Registrasi` merepresentasikan struktur data respons dari API:

```dart
class Registrasi {
  int? code;
  bool? status;
  String? data;

  Registrasi({this.code, this.status, this.data});

  factory Registrasi.fromJson(Map<String, dynamic> obj) {
     return Registrasi(
     code: obj['code'],
     status: obj['status'],
     data: obj['data']);
  }
}
```

## 4. Alur Proses Registrasi

1. User mengisi form registrasi di `RegistrasiPage`.
2. Ketika tombol registrasi ditekan, validasi form dilakukan.
3. Jika valid, `_submit()` dipanggil, yang kemudian memanggil `RegistrasiBloc.registrasi()`.
4. `RegistrasiBloc` mengirim data ke API menggunakan `Api().post()`.
5. API memproses data dan mengirim respons.
6. Respons dikonversi menjadi objek `Registrasi`.
7. Berdasarkan respons, dialog sukses atau peringatan ditampilkan di `RegistrasiPage`.


## Penanganan Error
- Jika terjadi error, dialog peringatan akan ditampilkan.
- jika tidak maka akan muncul dialog sukses.
<br>
<img src="registrasi2.png" width="300"/>



# B. Penjelasan Proses Login

## 1. Proses Login

```dart
Widget _buttonLogin() {
  return ElevatedButton(
    child: const Text("Login"),
    onPressed: () {
      var validate = _formKey.currentState!.validate();
      if (validate) {
        if (!_isLoading) _submit();
      }
    },
  );
}

void _submit() {
  _formKey.currentState!.save();
  setState(() {
    _isLoading = true;
  });
  LoginBloc.login(
    email: _emailTextboxController.text,
    password: _passwordTextboxController.text
  ).then((value) async {
    print("Login successful: $value");
    if (value.userID != null) {
      await UserInfo().setToken(value.token ?? "");
      await UserInfo().setUserID(value.userID!);
      Navigator.pushReplacement(context,
          MaterialPageRoute(builder: (context) => const ProdukPage()));
    } else {
      throw Exception("UserID is null");
    }
  }).catchError((error) {
    print("Login error: $error");
    showDialog(
      context: context,
      barrierDismissible: false,
      builder: (BuildContext context) => WarningDialog(
        description: "Login gagal: ${error.toString()}",
      )
    );
  }).whenComplete(() {
    setState(() {
      _isLoading = false;
    });
  });
}
```

Penjelasan:
- `_buttonLogin()` memvalidasi form sebelum memanggil `_submit()`.
- `_submit()` melakukan proses login:
    1. Mengatur `_isLoading` menjadi true.
    2. Memanggil `LoginBloc.login()` dengan email dan password.
    3. Jika berhasil, menyimpan token dan userID, lalu navigasi ke `ProdukPage`.
    4. Jika gagal, menampilkan dialog peringatan.
    5. Setelah selesai, mengatur `_isLoading` kembali menjadi false.

## 2. Model Login

```dart
class Login {
  int? code;
  bool? status;
  String? token;
  int? userID;
  String? userEmail;

  Login({this.code, this.status, this.token, this.userID, this.userEmail});

  factory Login.fromJson(Map<String, dynamic> obj) {
    print(obj);  // Untuk debugging
    return Login(
        code: obj['code'],
        status: obj['status'],
        token: obj['data']['token'],
        userID: int.tryParse(obj['data']['user']['id'].toString()),
        userEmail: obj['data']['user']['email']
    );
  }
}
```

Penjelasan:
- Model `Login` merepresentasikan respons dari API login.
- `Login.fromJson()` mengkonversi data JSON menjadi objek `Login`.
- `int.tryParse()` digunakan untuk mengonversi ID user ke tipe int.

## 3. Login Bloc

```dart
import 'dart:convert';
import 'package:tokokita/helpers/api.dart';
import 'package:tokokita/helpers/api_url.dart';
import 'package:tokokita/model/login.dart';

class LoginBloc {
  static Future<Login> login({String? email, String? password}) async {
    String apiUrl = ApiUrl.login;
    var body = {"email": email, "password": password};
    var response = await Api().post(apiUrl, body);
    var jsonObj = json.decode(response.body);
    return Login.fromJson(jsonObj);
  }
}
```

Penjelasan:
- `LoginBloc` menangani logika bisnis untuk proses login.
- `login()` method mengirim request POST ke API dengan email dan password.
- Respons API di-decode dari JSON dan dikonversi menjadi objek `Login`.

## 4. Alur Proses Login

1. User mengisi form login di `LoginPage`.
2. Saat tombol login ditekan, validasi form dilakukan.
3. Jika valid, `_submit()` dipanggil, yang memanggil `LoginBloc.login()`.
4. `LoginBloc` mengirim data ke API menggunakan `Api().post()`.
5. API memproses data dan mengirim respons.
6. Respons dikonversi menjadi objek `Login`.
7. Jika login berhasil, token dan userID disimpan, dan user diarahkan ke `ProdukPage`.
8. Jika gagal, dialog peringatan ditampilkan.


# C. Penjelasan Proses Tampil Data



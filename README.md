# tokokita

Nama : Hendra Latieful Maajid

NIM : H1D022018

Shift KRS: D

Shift Baru: F

Tugas pertemuan 4 dan 5 (Tugas pertemuan 5 ada dibawah tugas pertemuan 4)
## Screenshot TUGAS PERTEMUAN 4
![Screenshot](login.png)
![Screenshot](registrasi.png)
![Screenshot](list_produk.png)
![Screenshot](tambah.png)
![Screenshot](edit.png)
![Screenshot](delete.png)
![Screenshot](logout.png)

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


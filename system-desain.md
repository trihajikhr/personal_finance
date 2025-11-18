# System Desain

Berisi rancangan awal dan sistem dari aplikasi yang akan dibuat. Mengambil beberapa contoh dari banyak aplikasi personal finance lainya yang sudah ada, maka dibuat rancangan sistem desain sebagai berikut:

## Identifikasi Entitas

1. User
2. Mata uang
3. Kategori
4. Pemasukan
5. Pengeluaran
6. Tabungan
7. Pengeluaran tabungan
8. Pemasukan tabungan

## Identifikasi Tipe data

SQLite memiliki beberapa perbedaan syntax dengan query SQL lain, sehingga akan digunakan penulisan tipe data yang sesuai dengan ketentuan SQLite yang ada.

### User

Aplikasi akan dirancang untuk bisa menangai lebih dari satu akun pengguna, yaitu dengan dibuat memiliki lebih dari akun. Setiap akun akan memiliki data keuangan yang berbeda-beda. Aplikasi personal finance modern kebanyakan mengikuti pendekatna ini.

| Atribut       | Tipe data | Keterangan                    |
|---------------|-----------|-------------------------------|
| id_user       | int       | PK (autoincrement, not null)) |
| email         | varchar   | unique                        |
| nama          | varchar   |                               |
| tanggal regis | datetime  |                               |
| mata uang     | id        | FK                            |
| saldo         | int       |                               |

### Mata Uang

Ketika user pertama kali masuk dan membuat akun, user akan diminta untuk memberi tahu aplikasi jenis mata uang apa yang dia gunakan. Namun jika ditengah-tengah pemakaian yser ingin berganti mata uang, maka aplikasi harus bisa menghandle hal tersebut. 

Kasus ini mungkin untuk terjadi, jika semisal user berpindah negara, dan ingin mempertahankan value mata uang yang dimilikinya di aplikasi.

| Atribut         | Tipe data | Keterangan |
|-----------------|-----------|------------|
| id_matauang     | int       | PK         |
| Kode iso        | varchar   | unique     |
| nama_matauang   | varchar   |            |
| simbol matauang | varchar   |            |
| presisi_desimal | int       |            |
| nama_singkat    | varchar   |            |

### Kategori

Kategori akan menyimpan pengelompokan pemasukan dan pengeluaran yang dikelompokan berdasarkan kategori. User bisa memantau total pengeluaran dan pemasukan berdasarkan kategori tertentu, dengan menyediakan opsi yang menyediakan tampilakan riwayat transaksi berdasar kategori.


| Atribut       | Tipe data | Keterangan               |
|---------------|-----------|--------------------------|
| id_kategori   | int       | PK (*autu increment*)    |
| jenis         | enum      | (pemasukan, pengeluaran) |
| nama_kategori | varchar   |                          |

### Pemasukan

Pemasukan mencaatat uang masuk yang diterima oleh sistem, dan kemudian akan menambah total saldo yang ada. Aplikasi harus bisa menyimpan log pemasukan, mulai dari tanggal, value, dan jenis pemasukan yang diinputkan oleh user.

| Atribut      | Tipe data | Keterangan            |
|--------------|-----------|-----------------------|
| id_pemasukan | int       | PK (*auto increment*) |
| tanggal      | datetime  |                       |
| id_kategori  | int       | FK                    |
| jumlah       | int       |                       |

### Pengeluaran

Pengeluaran mencatat uang yang keluar, atau dispent oleh user. Fungsinya mendukung pencatatan riwayat transaksi, yang tidak berbeda jauh dengan pencatatn pemasukan.

| Atribut        | Tipe data  | Keterangan            |
| -------------- |------------|-----------------------|
| id_pengeluaran | int        | PK (*auto increment*) |
| tanggal        | datetime   |                       |
| id_kategori    | varchar    | FK                    |
| jumlah         | int        |                       |

### Tabungan:

Beberapa aplikasi menawarkan opsi untuk membuat pengelompokan, dimana user membuat rencana finansial untuk kepentingan tertentu, dan mengharuskan menambung uangnya. Aplikasi akan dirancang untuk menyediakan opsi tersebut, sehingga pencatatn transaksi akan dipisahkan dengan pencatatn transaksi utama, namun dengan beberapa fitur yang lebih disederhanakan.

Ini artinya, tabungan akan memiliki entitas pemasukan dan pengeluaranya sendri, namun dengan kategori, user, dan mata uang utama.

| Atribut       | Tipe data   | Keterangan            |
| ------------- |-------------| --------------------- |
| id_tabungan   | int         | PK (*auto increment*) |
| tujuan        | varchar     |                       |
| jumlah        | int         |                       |
| target_jumlah | int         |                       |
| date_start    | date        |                       |
| date_end      |        date |                       |

- Pengeluaran tabungan

| Atribut        | Tipe     | Keterangan                  |
| -------------- |----------| --------------------------- |
| id_tabungan    | int      | FK (tabungan . id_tabungan) |
| nominal_keluar |    int   |                             |
| tanggal        | datetime |                             |

- Pemasukan tabungan

| Atribut       | Tipe            | Keterangan                 |
| ------------- |-----------------| -------------------------- |
| id_tabungan   | int             | FK(tabungan . id_tabungan) |
| nominal_masuk | int             |                            |
| tanggal       |       datetimee |                            |

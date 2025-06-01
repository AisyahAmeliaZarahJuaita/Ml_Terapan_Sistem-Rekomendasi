# Laporan Proyek Machine Learning - Aisyah Amelia Zarah Juaita

# Project Overview

## Latar Belakang

Indonesia merupakan negara yang kaya akan destinasi wisata. Namun, banyak wisatawan kesulitan menemukan destinasi yang sesuai dengan preferensi mereka. Untuk itu, sistem rekomendasi tempat wisata sangat dibutuhkan untuk membantu pengguna dalam menentukan tujuan wisata berdasarkan histori dan minat mereka.

## Pentingnya Proyek

Pentingnya proyek ini terletak pada kemampuannya untuk meningkatkan pengalaman pengguna dalam memilih destinasi wisata, mempercepat proses pencarian tempat wisata yang relevan, serta mendorong sektor pariwisata secara keseluruhan.

## Referensi

- Ricci, Rokach, & Shapira (2015). Recommender Systems Handbook.

- Dataset: Kaggle Travel Recommendation System

# Business Understanding

## Problem Statements

1. Bagaimana sistem dapat merekomendasikan tempat wisata yang sesuai dengan preferensi pengguna?

2. Bagaimana cara meningkatkan relevansi rekomendasi menggunakan pendekatan yang berbeda?
   
3. Pendekatan mana yang lebih efektif: content-based atau collaborative filtering?

## Goals

1. Mengembangkan sistem rekomendasi tempat wisata.

2. Menyajikan dua pendekatan berbeda: content-based filtering dan collaborative filtering.

3. Memberikan output rekomendasi Top-N destinasi untuk setiap pengguna.

## Solution Statements

1. Content-Based Filtering
   Menggunakan informasi atribut dari tempat wisata (seperti kategori, deskripsi, lokasi) untuk 
   menemukan kesamaan antar tempat, kemudian merekomendasikan tempat yang mirip dengan yang 
   disukai user sebelumnya.

2. Collaborative Filtering
   Menggunakan interaksi antar pengguna dan tempat wisata untuk menemukan pola kesukaan. 
   Pendekatan ini memanfaatkan teknik matrix factorization (seperti SVD) untuk merekomendasikan 
   tempat yang disukai pengguna serupa.

# Data Understanding

Proyek ini menggunakan dataset Travel Recommendation System yang tersedia di Kaggle:

🔗 https://www.kaggle.com/code/datawithshaurya/travel-recommendation-system

## Informasi Dataset

1. tourism_rating.csv: memiliki 10.000 baris dan memiliki 3 kolom
2. tourism_with_id.csv: memiliki 437 baris dan memiliki 13 kolom
3. user.csv: memiliki 300 baris dan memiliki 3 kolom

## Exploratory Data Analysis

- Tourism Rating Variabel
  
  "tourism_rating.head()"

Pada bagian ini Dataset tourism_rating berisi data interaksi antara pengguna dan tempat wisata dalam bentuk rating. Setiap baris merepresentasikan satu penilaian yang diberikan oleh pengguna (User_Id) terhadap suatu destinasi wisata (Place_Id) dengan skor tertentu pada kolom Place_Ratings.

  "tourism_rating.info()"
  
Hasil dari tourism_rating.info() menunjukkan bahwa dataset terdiri dari 10.000 baris dan 3 kolom: User_Id, Place_Id, dan Place_Ratings. Semua kolom bertipe data int64 dan tidak memiliki missing value (nilai kosong). Ini berarti data bersih dan siap digunakan untuk analisis atau pelatihan model rekomendasi tanpa perlu proses imputasi atau pembersihan tambahan.

- Tourism Variabel

  "tourism.head()"

Dataset `tourism` berisi informasi detail tentang destinasi wisata. Setiap baris merepresentasikan satu tempat wisata, dengan kolom-kolom seperti `Place_Id` (ID tempat), `Place_Name` (nama tempat), `Description` (deskripsi), `Category` (jenis wisata), `City` (kota), `Price` (harga tiket), `Rating` (nilai rating umum), `Time_Minutes` (perkiraan durasi kunjungan), serta koordinat geografis (`Lat` dan `Long`). Terdapat juga kolom `Coordinate` dalam format dictionary, dan dua kolom tambahan (`Unnamed: 11` dan `Unnamed: 12`) yang tampaknya tidak terpakai atau hasil dari proses ekspor. Data ini penting untuk sistem rekomendasi berbasis konten karena menyediakan fitur-fitur yang bisa digunakan untuk mengukur kemiripan antar tempat wisata.

  "tourism.info()"

Hasil `tourism.info()` menunjukkan bahwa dataset memiliki 437 entri dan 13 kolom. Sebagian besar kolom terisi penuh, kecuali `Time_Minutes` yang hanya memiliki 205 nilai (mengandung banyak missing value) dan `Unnamed: 11` yang seluruhnya kosong. Kolom seperti `Place_Id`, `Place_Name`, `Description`, `Category`, dan `City` menyimpan informasi utama tentang tempat wisata, sedangkan `Price`, `Rating`, `Lat`, dan `Long` memberikan data numerik terkait lokasi dan preferensi. Kolom `Unnamed: 11` sebaiknya dihapus karena tidak mengandung informasi. Dataset ini cocok digunakan untuk analisis konten destinasi wisata dan visualisasi lokasi.

- User Variabel

  "user.head()"

Dataset `user` berisi informasi tentang pengguna yang memberikan rating pada tempat wisata. Setiap baris mencakup `User_Id` (ID unik pengguna), `Location` (asal kota dan provinsi), serta `Age` (usia pengguna). Data ini berguna untuk analisis demografis dan dapat dimanfaatkan dalam sistem rekomendasi berbasis pengguna (collaborative filtering) atau untuk personalisasi rekomendasi berdasarkan lokasi dan usia.

  "user.info()"

Hasil `user.info()` menunjukkan bahwa dataset berisi 300 entri dengan 3 kolom: `User_Id`, `Location`, dan `Age`. Semua kolom memiliki data lengkap (tidak ada missing value). `User_Id` dan `Age` bertipe numerik (`int64`), sementara `Location` bertipe teks (`object`). Dataset ini bersih dan siap digunakan untuk analisis atau pemodelan, terutama dalam sistem rekomendasi berbasis pengguna atau segmentasi berdasarkan lokasi dan usia.

- Melihat Statistik Unik

Terdapat output yang dihasilkan: 

- Jumlah user unik: 300
- Jumlah tempat wisata unik: 437

## Variabel-Variabel pada Dataset

1. tourism_rating:

    1. `User_Id` - ID unik yang merepresentasikan pengguna yang memberi rating.
    2. `Place_Id``- ID unik untuk setiap tempat wisata yang diberi rating.
    3. `Place_Ratings` - Nilai rating (penilaian) yang diberikan user terhadap tempat wisata 
        tersebut (biasanya skala 1-5).

2. tourism:
    1. `Place_Id` - ID unik tempat wisata (digunakan sebagai kunci relasi).
    2. `Place_Name` - Nama tempat wisata.
    3. `Description` - Deskripsi singkat tempat wisata.
    4. `Category` - Kategori tempat wisata.
    5. `City` - Kota tempat wisata berada.
    6. `Price` - Estimasi harga tiket masuk atau biaya kunjungan.
    7. `Rating` - Rating umum atau popularitas tempat.
    8. `Time_Minutes` - Estimasi waktu kunjungan dalam menit (durasi).
    9. `Coordinate` - Lokasi geografis gabungan (lat-long dalam satu string).
    10. `Lat` - Latitude tempat wisata (lintang).
    11. `Long` - Longitude tempat wisata (bujur).
    12. `Unnamed: 11` - Kolom kosong.
    13. `Unnamed: 12` - Kolom kosong.

3. user:

    1. `User_Id` - ID unik pengguna.
    2. `Location` - Lokasi pengguna (bisa berupa kota, provinsi, atau nama tempat tinggal).
    3. `Age` - 	Umur pengguna.

# Data Preparation

## Pembersihan dan Penggabungan Data

- Menggabungkan Semua Tempat (placeID Unik)

Menghasilkan output: Jumlah seluruh tempat wisata unik berdasarkan Place_Id: 437, yang dimana Tahap ini digunakan untuk memperoleh seluruh ID tempat wisata unik dari dataset tourism. Pertama, Place_Id diambil dan diubah menjadi array unik menggunakan np.unique(), lalu diurutkan dengan np.sort(). Hasil akhirnya menunjukkan bahwa terdapat 437 tempat wisata unik berdasarkan kolom Place_Id, yang sesuai dengan jumlah tempat wisata dalam dataset.

- Menggabungkan Semua User

Menghasilkan output: Jumlah seluruh user unik: 300, yang dimana tahap ini menggabungkan seluruh ID pengguna (User_Id) dari dua dataset: tourism_rating dan user. Setelah digabung, kode menggunakan np.unique() untuk menghapus duplikat, lalu mengurutkan hasilnya. Hasil akhirnya menunjukkan bahwa terdapat 300 pengguna unik secara keseluruhan.

- Melihat Jumlah Rating

Menghasilkan output: 

Jumlah data rating: 10000

Cek missing value:
User_Id          0
Place_Id         0
Place_Ratings    0
dtype: int64

Yang dimana pada tahap ini menampilkan jumlah total data rating dalam dataset `tourism_rating`, yaitu 10.000 baris. Selain itu, pengecekan missing value menunjukkan bahwa tidak ada nilai yang hilang pada kolom `User_Id`, `Place_Id`, maupun `Place_Ratings`, sehingga data lengkap dan siap digunakan untuk analisis atau pemodelan.

- Gabungkan Rating dengan Nama Tempat

Pada tahap ini melakukan beberapa langkah penting untuk memudahkan analisis data. Pertama, kolom `Place_Id` dan `Place_Name` pada dataset `tourism` diganti namanya menjadi `placeID` dan `name` agar konsisten dengan dataset `tourism_rating` yang juga diubah kolom `Place_Id` dan `User_Id` menjadi `placeID` dan `userID`. Selanjutnya, kedua dataset tersebut digabung (`merge`) berdasarkan kolom `placeID` untuk menggabungkan data rating dengan informasi nama tempat wisata dan kategorinya. Hasilnya adalah tabel yang berisi ID pengguna, ID tempat, nilai rating, nama tempat wisata, dan kategori, sehingga memudahkan analisis dan rekomendasi berbasis konten dan rating.

- Gabungkan Data dengan Profil User

Pada tahap ini menggabungkan data pengguna (user) dengan data rating dan informasi tempat wisata (all_place_name) berdasarkan kolom userID. Sebelumnya, kolom User_Id pada dataset user diubah menjadi userID agar konsisten. Hasil penggabungan (all_data) berisi informasi lengkap yang mencakup ID pengguna, ID tempat, rating, nama tempat, kategori, serta data demografis pengguna seperti lokasi dan usia. Data ini siap untuk analisis lebih lanjut, misalnya untuk personalisasi rekomendasi berdasarkan profil pengguna.

- Cek Data Akhir & Missing Value

Tahapan ini memeriksa apakah terdapat nilai yang hilang (missing values) dalam dataset gabungan `all_data`. Hasilnya menunjukkan bahwa tidak ada missing value di semua kolom, sehingga data lengkap dan siap digunakan untuk analisis atau pemodelan tanpa perlu penanganan data kosong lebih lanjut.

## Content Development Content Based Filtering

1. TF-IDF Vectorizer
   
   - Pada tahap ini menggunakan TfidfVectorizer untuk mengubah data kategori tempat wisata 
     menjadi representasi numerik berdasarkan frekuensi dan pentingnya kata (TF-IDF). Setelah 
     dilakukan fitting pada kolom category, dihasilkan daftar fitur unik berupa kata-kata yang 
     membentuk kategori, seperti 'budaya', 'taman', 'hiburan', dll. Fitur ini bisa digunakan 
     untuk analisis teks atau pemodelan lebih lanjut.

   - Mengubah data kategori wisata dari tourism_new['category'] menjadi matriks TF-IDF 
     menggunakan fit_transform. Matriks ini merepresentasikan 437 tempat wisata (baris) dan 10 
     kata fitur unik (kolom) yang dihasilkan sebelumnya. Jadi, ukuran matriks (437, 10) 
     menunjukkan ada 437 dokumen (tempat wisata) dan 10 fitur kata unik kategori yang dipakai 
     untuk analisis lebih lanjut.

   - Pada tahapan ini yaitu mengubah matriks TF-IDF yang sebelumnya dalam format sparse matrix 
     menjadi matriks dense (penuh) menggunakan .todense(). Outputnya adalah matriks numerik 
     lengkap yang menampilkan bobot TF-IDF setiap fitur kategori untuk setiap tempat wisata. 
     Nilai-nilai di dalam matriks menunjukkan seberapa penting suatu kata (kategori) dalam 
     deskripsi tiap tempat wisata, dengan angka 0 artinya kata tersebut tidak muncul pada 
     kategori tersebut.

   - Untuk tahapan ini  sebuah DataFrame dari matriks TF-IDF yang telah dihitung sebelumnya, di 
     mana kolom-kolomnya merepresentasikan kategori wisata (fitur TF-IDF) dan baris-barisnya 
     adalah nama-nama tempat wisata. Kemudian, kode tersebut mengambil sampel acak sebanyak 22 
     kolom dan 10 baris untuk menampilkan sebagian data secara acak, sehingga memudahkan dalam 
     melihat representasi bobot kategori pada beberapa tempat wisata secara ringkas.

2. Cosine Similarity

   



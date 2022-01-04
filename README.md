# Laporan Proyek Machine Learning 
#
#### Nama   : Ramdani Tarjianto
#### Email  : ramdani.tarjianto83@gmail.com
#
#
#
#
#


# Judul
##### MEMBUAT SISTEM REKOMENDASI FILM DENGAN CONTENT BASED FILLTERING DAN COLABORATIVE FILLTERING
#
#
#
## 1.DOMAIN PROYEK

###### 1.1 LATAR BELAKANG
Dengan adanya internet, informasi lebih mudah didapat,Karena kita bisa mengakses internet kapan saja, dimana saja. Namun informasi Juga lebih cepat menyebar agar informasinya terus berkembang, sehingga Pengguna kesulitan mendapatkan informasi yang sesuai dengan informasi yang mereka cari, dan Sulit untuk memilih informasi yang sesuai dengan selera pengguna. Misalnya, ada berbagai situs web untuk informasi tentang film Berikan informasi tentang film, seperti database film Internet (IMDb.com), MovieLens (movielens.org), dll. Menurut informasi Bisa didapatkan di website IMDb, saat ini IMDb memiliki total 4.449.001 (IMDb.com, 2017) dan akan terus bertambah. dari Jumlah data pasti akan menyulitkan pengguna untuk menemukan judul Lihatlah informasi tentang film yang mungkin dia sukai Film di situs web. 
Dari situlah muncul tantangan untuk membuat sebuah sistem yang dapat memberikan saran atau rekomendasi kepada pengguna sehingga dapat memperoleh item atau informasi yang sesuai dengan selera mereka, dan sistem tersebut adalah sistem rekomendasi. Sistem rekomendasi dapat membantu pengguna dengan memberikan saran untuk proyek berdasarkan preferensi mereka. Saat ini, sistem rekomendasi banyak digunakan di berbagai bidang seperti musik, buku, berita, film, dll. Metode yang digunakan dalam penelitian ini adalah Content Based Filltering dan Collaborative Filtering. Prinsip kerja Content Based Filltering adalah merekomendasikan item yang mirip dengan item yang disukai pengguna di masa lalu. Sedangkan Prinsip kerja Collaborative Filltering adalah memberikan rekomendasi kepada pengguna aktif berdasarkan kesamaan selera pengguna lain terhadap suatu item

###### 1.2 MASALAH DALAM DOMAIN
Cara membuat rekomendasi film dengan menggunakan content based filltering dan collaborative filltering agar dapat membantu pengguna dengan memberikan saran untuk proyek berdasarkan preferensi mereka.
#
#
#
#
#
## 2.BUSINESS UNDERSTANDING
###### 2.1 PROBLEM STATEMENTS
Membuat sistem rekomendasi film dengan content based filltering dan collaborative filltering
###### 2.2 TUJUAN & Manfaat
- Mengimplemtasikan metode Content Based Filltering dan Collaborative Filtering    sebagai metode dalam sistem rekomendasi film.
- Mengukur tingkat akurasi sistem rekomendasi dengan menggunakan
metode Content Based Filltering dan Collaborative Filtering.
- Merekomendasikan film berdasarkan prefensi pengguna.

## 3. DATA UNDERSTANDING
Dataset yang digunakan pada penelitian ini adalah data yang berasal dari situs kaggle, yaitu [movies-recomendation-system](https://www.kaggle.com/kanametov/movies-recomendation-system). Dataset ini menjelaskan peringkat bintang 5 dan aktivitas penandaan teks bebas dari [MovieLens](http://movielens.org), layanan rekomendasi film. Dataset ini berisi 22884377 data ratings dan 586994 data  tag di 34208 film. Data ini dibuat oleh 247753 pengguna antara 09 Januari 1995 dan 29 Januari 2016. Dataset ini dibuat pada 29 Januari 2016.

Variabel-variabel pada dataset movies-recomendation-system adalah sebagai berikut:
- movies = berisi tentang nama movie dan Geners
- ratings = berisi tentang user yang memberikan rating mulai dari 1.0 sampai 5.0 setiap movies
- tags = berisi tag dari setiap movies
- links = berisi imdbid dan tmbdid pada setiap movies

### 3.1 DATA VISUALIZATION
![Visualisasi](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/1.PNG)
berdasarkan output di atas, kita dapat mengetahui bahwa file movies.csv memiliki 34208 entri dan 3 column yang terdiri dari movieId, title dan genres. movieId merupakan ID dari setiap movie, title merupakan judul dari setiap movie, sedangkan genres merupakan genre dari setiap movie.

![Visualisasi](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/2.PNG)
kita dapat mengetahui bahwa file ratings.csv memiliki 22884377 entri dan memiliki 4 column, yang terdiri dari userId, movieId, rating, timestamp. userId merupakan id dari setiap user, movieId merupakan ID dari setiap movie, rating merupakan penilaian dari setiap movie dan timestamp merupakan Stempel waktu dari setiap movie dalam satuan detik.

![Menggabungkan Data](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/gabungin%20data.PNG)
Kemudian penulis menggabungkan dataframe pada file movie.csv dan rating.csv berdasarkan movieId. Tujuannya, agar penulis dapat mengetahui judul film, genres dan rating dari setiap pengguna.

#
#
#
#
#
## 4. DATA PREPARATION
#### 4.1.1 Content Based Filltering
Setelah proses penggabungan, penulis mencoba untuk memeriksa missing value pada data tersebut. 
![Missing_Value](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/dp1.PNG)
dapat terlihat bahwa pada data tersebut tidak ada missing value, yang berarti dapat melanjutkan ketahap selanjutnya.

Sebelum masuk tahap pemodelan, penulis perlu menyamakan genres. terkadang nama movie sama tetapi memiliki genres yang berbeda. Jika dibiarkan, hal ini bisa menyebabkan bias pada data.
![Mengurutkan_Dataframe](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/4.mengurutkan%20movie.PNG)
Ternyata di column genres terdapat (no genres listed) yang artinya pada film itu tidak terdefinisi dengan genres apapun.

![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/5.mengecek%20genres.PNG)
Kemudian penulis memeriksa semua film yang tidak terdefinisi dengan genres apapun (no genres listed). dan terdapat 2454 yang tidak terdefinisi.

![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/6.%20memeriksa%20movie%20dengan%20no.PNG)
Selanjutnya penulis memeriksa salah satu film dengan judul "Scorpio Rising (1964)" dan memeriksa apakah dalam film tersebut terdapat genre yang terdefinisi, dan ternyata tidak ditemukan genre yang terdefinisi pada film tersebut. ini berlaku terhadap semua film yang tidak memiliki genre yang terdefinisi. Tentunya ini dapat mempengaruhi hasil rekomendasi pada model nantinya.

![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/7.%20fix.PNG)
Langkah selanjutnya agar tidak mempengaruhi hasil rekomendasi, film yang tidak terdefinisi dengan genres apapun akan dihapus. Pada tahap ini data sudah bersih dari film yang tidak terdifinisi dengan genres apapun dan missing values.



#### 4.1.2 Collaborative Filltering
![membaca](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/dp_cl0.PNG)
Load data dengan membaca file ratings.csv dan mengambil sebanyak 5000 data, kemudian memeriksa apakah ada missing values pada dataset, dan tidak ditemukan missinga values maka bisa lanjut ke langkah selanjutnya.

![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/dp_cl1.PNG)
penulis mencoba (encode) fitur ‘user’ dan ‘movie’ ke dalam indeks integer. kemudian memetakan userId dan movieId ke dataframe yang berkaitan. menampilkan jumlah user, jumlah movies, dan mengubah nilai rating menjadi float.

![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/dp_cl2.PNG)
Pada tahap ini penulis melakukan pembagian data menjadi data training dan validasi. Selanjutnya, penulis membagi data train dan validasi dengan komposisi 90:10. Namun sebelumnya, kemudian memetakan (mapping) data user dan movies menjadi satu value. Kemudian penulis menormalisasi dalam skala 0 sampai 1 agar mudah dalam melakukan proses training.

#
#
#
#
#
## 5. MODELING
#### 5.1.1 Content Based Filltering
![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/md%201.PNG)
Tahap selanjutnya penulis hanya mengambil 5000 dari 22884377 data agar tidak memamakan resource komputasi yang besar saat menghitung dengan menggunakan cosine_similarity.

![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/md%202.PNG)
Pada tahap ini, penulis membangun sistem rekomendasi sederhana berdasarkan jenis genres pada setiap film. Penulis menggunakan TF-IDF Vectorizer Teknik tersebut juga akan digunakan pada sistem rekomendasi untuk menemukan representasi fitur penting dari setiap genres pada film.

![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/md%203.PNG)
Selanjutnya penulis melakukan fit dan transformasi ke dalam bentuk matriks. matriks yang penulis miliki berukuran (5000, 21). Nilai 5000 merupakan ukuran data dan 21 merupakan matrik genres.  

![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/md%204.PNG)
Selanjutnya, penulis menghitung derajat kesamaan (similarity degree) antar film dengan teknik cosine similarity. Pada tahapan ini, penulis menghitung cosine similarity dataframe tfidf_matrix yang kita peroleh pada tahapan sebelumnya.

![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/md%205.PNG)
Langkah selanjutnya penulis membuat fungsi untuk menampilkan hasil rekomendasi.
- Nama_movies : Nama movie (index kemiripan dataframe).
- Similarity_data : Dataframe mengenai similarity yang telah kita definisikan sebelumnya.
- Items : Nama dan fitur yang digunakan untuk mendefinisikan kemiripan, dalam  hal ini adalah ‘title’ dan ‘genres’.
- k : Banyak rekomendasi yang ingin diberikan.

![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/md%206.PNG)



#### 5.1.2 Collaborative Filltering
![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/md_cl1.PNG)
Selanjutnya adalah tahapan compile model menggunakan loss Binary Crossentropy, Optimizer menggunakan adam dengan learing rate = 0.001, dan metrics accuracy menggunakan Root Mean Squared Error (RMSE). Kemudian melakukan training dengan epoch 100 dan batch size 64.

![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/md_cl2.PNG)
ini merupakan hasil training dari model


#
#
#
#
#
## 6. EVALUATION
#### 6.1.1 Content Based Filltering
![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/md%206.PNG)
Dari hasil rekomendasi diatas, diketahui bahwa "Postman, The (Postino, Il) (1994)" termasuk ke dalam genres Comedy|Drama|Romance. Dari 10 item yang direkomendasikan, 10 item memiliki kategori Comedy|Drama|Romance (similar). Artinya, precision sistem kita sebesar 10/10 atau 100%.

#### 6.1.2 Collaborative Filltering
![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/md_cl4.PNG)
Pada proses training model cukup smooth dan model konvergen pada epochs sekitar 100. Dari proses ini, kita memperoleh nilai root_mean_squred_error akhir sebesar sekitar 0.15 dan error pada data validasi sebesar 0.23.

![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/md_cl3.PNG)
hasil di atas adalah rekomendasi untuk user dengan id 23. Dari output tersebut, membandingkan antara Movies with high ratings from user dan Top 10 movie recommendation untuk user. beberapa movies rekomendasi memiliki genres yang sesuai dengan rating user

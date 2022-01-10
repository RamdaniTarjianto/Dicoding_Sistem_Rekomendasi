# Laporan Proyek Machine Learning 
#
#### Nama   : Ramdani Tarjianto
#### Email  : ramdani.tarjianto83@gmail.com
#
# Judul
##### MEMBUAT SISTEM REKOMENDASI FILM DENGAN CONTENT BASED FILTERING DAN COLLABORATIVE FILTERING
#
#
#
## 1.DOMAIN PROYEK

###### 1.1 LATAR BELAKANG
Dengan adanya internet, informasi lebih mudah didapat, karena kita bisa mengakses internet kapan saja, dimana saja. Namun informasi juga semakin cepat bertambah, sehingga pengguna kesulitan mendapatkan informasi yang sesuai dengan informasi yang sedang mereka cari, dan sulit untuk memilih informasi yang sesuai dengan selera mereka. Misalnya, ada berbagai situs web untuk informasi tentang film, seperti database film Internet (IMDb.com), MovieLens (movielens.org), dll. Diperkirakan data film akan terus bertambah setiap tahunnya. Dari data film yang terus bertambah setiap tahunnya pasti akan menyulitkan pengguna untuk menemukan judul film yang mungkin akan mereka sukai.

Dari situlah muncul tantangan untuk membuat sebuah sistem yang dapat memberikan saran atau rekomendasi kepada pengguna sehingga dapat memperoleh item atau informasi yang sesuai dengan selera mereka, dan sistem tersebut adalah sistem rekomendasi. Sistem rekomendasi dapat membantu pengguna dengan memberikan rekomendasi berdasarkan preferensi mereka. Saat ini, sistem rekomendasi banyak digunakan di berbagai bidang seperti musik, buku, berita, film, dll. Metode yang digunakan dalam penelitian ini adalah Content Based Filtering dan Collaborative Filtering. Prinsip kerja Content Based Filtering adalah merekomendasikan item yang mirip dengan item yang disukai pengguna di masa lalu. Sedangkan Prinsip kerja Collaborative Filtering adalah memberikan rekomendasi kepada pengguna aktif berdasarkan kesamaan selera pengguna lain terhadap suatu item

###### 1.2 MASALAH DALAM DOMAIN
Cara membuat rekomendasi film dengan menggunakan content based filtering dan collaborative filtering agar dapat membantu pengguna dengan memberikan rekomendasi berdasarkan preferensi mereka.
#
#
#
## 2.BUSINESS UNDERSTANDING
###### 2.1 PROBLEM STATEMENTS
- Bagaimana cara membuat sistem rekomendasi film menggunakan teknik Content Based Filtering?
- Bagaimana cara membuat sistem rekomendasi yang dapat memberikan rekomendasi berdasarkan preferensi pengguna?

###### 2.2 GOALS
- Membuat sistem rekomendasi film menggunakan teknik Content Based filtering.
- Mengetahui cara mendapatkan rekomendasi dari film yang mungkin akan disukai dan belum pernah diakses/ditonton oleh pengguna menggunakan teknik Collaborative Filtering.

 
###### 2.3 SOLUTION STATEMENT
- Kelebihan dari Content Based Filtering adalah merekomendasikan item yang mirip dengan item yang disukai pengguna di masa lalu, sehingga Content Based Filtering dapat memberikan rekomendasi item yang subjektif dan tepat kepada pengguna. Kekurangan Content Based Filtering adalah Sistem hanya akan menunjukkan item yang nilainya tinggi untuk dicocokkan dengan profil pengguna, maka pengguna akan selalu menemukan item serupa dengan yang sudah direkomendasikan sebelumnya.

- Kelebihan dari Collaborative Filtering adalah pengguna dapat menjelajahi item atau konten di luar preferensi pengguna. Pengguna juga bisa mendapatkan rekomendasi berdasarkan tren publik yang dianalisis melalui penilaian pengguna lain. Kelemahan dari Collaborative Filtering adalah pengguna tidak akan mendapatkan rekomendasi berdasarkan preferensi pribadi. Konten-Konten yang disediakan oleh sistem rekomendasi lebih banyak berasal dari preferensi publik daripada preferensi pribadi.

## 3. DATA UNDERSTANDING
Dataset yang digunakan pada penelitian ini adalah data yang berasal dari situs kaggle, yaitu [movies-recomendation-system](https://www.kaggle.com/kanametov/movies-recomendation-system). Dataset ini berisi 22884377 data ratings dan 586994 data  tag di 34208 film. Dataset ini dibuat oleh 247753 pengguna, antara 09 Januari 1995 dan 29 Januari 2016. User dipilih secara acak, semua pengguna yang dipilih telah memberi peringkat setidaknya 1 film. Tidak ada informasi demografis yang disertakan. Setiap user diwakili oleh id, dan tidak ada informasi lain yang disediakan. Data tersebut terdapat dalam empat file, `links.csv`, `movies.csv`, `ratings.csv` dan `tags.csv`. 

Variabel-variabel pada dataset movies-recomendation-system adalah sebagai berikut:
- movies = berisi tentang nama movie dan Genres
- ratings = berisi tentang user yang memberikan rating mulai dari 1.0 sampai 5.0 setiap movies
- tags = berisi tag dari setiap movies
- links = berisi imdbid dan tmbdid pada setiap movies


Setelah proses penggabungan, penulis mencoba untuk memeriksa missing value pada data tersebut. 
#
![Missing_Value](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/dp1.PNG)
#
dapat terlihat bahwa pada data tersebut tidak ada missing value, yang berarti dapat melanjutkan ke tahap selanjutnya.

### 3.1 DATA VISUALIZATION
![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/visualisasi%201s.PNG) 
#
Pada bar plot diatas dapat dilihat bahwa dari 1000 data yang diambil dari files ratings.csv, Ternyata banyak user yang memberikan rating 4 dan 3 pada film yang pernah mereka tonton.

![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/visualisasi%202s.PNG)
#
Pada bar plot kedua dapat dilihat bahwa dari 100 data yang diambil dari files movies.scv, Ternyata banyak film yang bergenres Drama
#
#
#
#
#
## 4. DATA PREPARATION
#### 4.1.1 Content Based Filtering
- Pada tahap data preparation content based filtering langkah pertama adalah menggabungkan dataframe ratings dan dataframe movie berdasarkan movieId kemudian memeriksa apakah ada missing value atau tidak menggunakan fungsi .isnull().sum(). Jika terdapat nilai yang hilang atau null, maka kita atasi menggunakan fungsi .dropna()

- Mengurutkan genres untuk memeriksa apakah ada genres yang tidak sesuai. Jika dibiarkan, hal ini bisa menyebabkan bias pada data. Ternyata di column genres terdapat (no genres listed) yang artinya pada film itu tidak terdefinisi dengan genres apapun.

- Kemudian penulis memeriksa semua film yang tidak terdefinisi dengan genres apapun (no genres listed) dan terdapat 2454 row yang tidak terdefinisi.
Selanjutnya penulis memeriksa salah satu film dengan judul "Scorpio Rising (1964)" dan memeriksa apakah dalam film tersebut terdapat genre yang terdefinisi, dan ternyata tidak ditemukan genre yang terdefinisi pada film tersebut. Ini berlaku terhadap semua film yang tidak memiliki genre yang terdefinisi. Tentunya ini dapat mempengaruhi hasil rekomendasi pada model nantinya.

- Langkah selanjutnya agar tidak mempengaruhi hasil rekomendasi, film yang tidak terdefinisi dengan genres apapun akan dihapus. Pada tahap ini data sudah bersih dari film yang tidak terdefinisi dengan genres apapun dan missing values.

- Kemudian mengkonversi data series menjadi list menggunakan fungsi tolist(). Kita akan konversi variabel 'movieId', 'title', dan 'genres' menjadi list.

- Selanjutnya membuat sebuah dictionary. Untuk data 'movieId', 'title', dan 'genres', ini berfungsi untuk menentukan pasangan key-value.


#### 4.1.2 Collaborative Filtering
- Tahap pertama Load data dengan membaca file ratings.csv dan mengambil sebanyak 5000 data agar tidak memakan resource komputasi yang besar

- Penulis mencoba (encode) fitur ‘user’ dan ‘movie’ ke dalam indeks integer. Kemudian memetakan userId dan movieId ke dataframe yang berkaitan. Menampilkan jumlah user, jumlah movies, dan mengubah nilai rating menjadi float. 

- Tahap selanjutnya penulis melakukan normalisasi pada kolom rating dengan skala 0 dan 1. Kemudian membagi data menjadi data training dan data validasi. Dengan komposisi 90% untuk data training dan 10% untuk data validasi.

#
#
#
#
#
## 5. MODELING
#### 5.1.1 Content Based Filtering
Setelah membuat sebuah dictionary. Untuk data 'movieId', 'title', dan 'genres' yang disimpan pada variabel movies_new. Tahap selanjutnya penulis hanya mengambil 5000 dari 22884377 data agar tidak memakan resource komputasi yang besar saat menghitung dengan menggunakan cosine_similarity.

![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/md%202.PNG)
#
Pada tahap ini, penulis membangun sistem rekomendasi sederhana berdasarkan jenis genres pada setiap film. Penulis menggunakan TF-IDF Vectorizer Teknik tersebut juga akan digunakan pada sistem rekomendasi untuk menemukan representasi fitur penting dari setiap genres pada film. Selanjutnya penulis melakukan fit dan transformasi ke dalam bentuk matriks. Matriks yang penulis miliki berukuran (5000, 21). Nilai 5000 merupakan ukuran data dan 21 merupakan matrik genres. Selanjutnya, penulis menghitung derajat kesamaan (similarity degree) antar film, dengan teknik cosine similarity. Pada tahapan ini, penulis menghitung cosine similarity dataframe tfidf_matrix yang kita peroleh pada tahapan sebelumnya.

Langkah selanjutnya penulis membuat fungsi yang bernama movies_recommendations untuk menampilkan hasil rekomendasi.
- Nama_movies : Nama movie (index kemiripan dataframe).
- Similarity_data : Dataframe mengenai similarity yang telah kita definisikan sebelumnya.
- Items : Nama dan fitur yang digunakan untuk mendefinisikan kemiripan, dalam  hal ini adalah ‘title’ dan ‘genres’.
- k : Banyak rekomendasi yang ingin diberikan.

![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/md%206.PNG)
#
Setelah kita menjalankan kode di atas, kita akan mendapatkan 10 rekomendasi film dengan genres yang sama. 


#### 5.1.2 Collaborative Filtering
![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/md_cl1.PNG)
#
Selanjutnya adalah tahapan compile model menggunakan loss Binary Crossentropy, Optimizer menggunakan adam dengan learning rate = 0.001, dan metrics accuracy menggunakan Root Mean Squared Error (RMSE). Kemudian melakukan training dengan epoch 100 dan batch size 64.

![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/md_cl3.PNG)
Hasil di atas adalah rekomendasi untuk user dengan id 23. Dari output tersebut, membandingkan antara Movies with high ratings from user dan Top 10 movie recommendation untuk user. Beberapa movies rekomendasi memiliki genres yang sesuai dengan rating user


#
#
#
#
#
## 6. EVALUATION
#### 6.1.1 Content Based Filtering
Dari hasil rekomendasi Content Based Filtering, diketahui bahwa "Postman, The (Postino, Il) (1994)" termasuk ke dalam genres Comedy|Drama|Romance. Dari 10 item yang direkomendasikan, 10 item memiliki kategori Comedy|Drama|Romance (similar). Artinya, precision sistem kita sebesar 10/10 atau 100%.
#
![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/precision.PNG)

#### 6.1.2 Collaborative Filtering
kita mengevaluasi metrik Root Squared Mean Error (RMSE). Root Squared Mean Error adalah metrik yang umum digunakan untuk mengukur perbedaan antara nilai (nilai sampel atau populasi) yang diprediksi oleh model dan nilai yang diamati. Dibandingkan dengan MSE, RMSE merupakan hasil root MSE sehingga nilai RMSE lebih kecil dari MSE.
![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/rumus.jpg)
#
- At = Nilai data Aktual 
- Ft = Nilai hasil peramalan 
- N= banyaknya data 
- ∑ = Summation (Jumlahkan keseluruhan nilai)

Nilai RMSE lebih kecil dari pada MSE, dan nilai yang kecil ini dapat menjadi kelebihan dan kekurangan tersendiri. Keuntungan dari nilai kecil adalah kita tidak perlu takut, karena nilai kesalahannya kecil, kita bisa langsung ke tahap berikutnya, tetapi nilai kesalahannya kecil, kita bisa terlalu percaya diri pada model untuk melihat. kemungkinan overfitting dan underfitting.

![](https://raw.githubusercontent.com/RamdaniTarjianto/Dicoding_Sistem_Rekomendasi/main/gambar/md_cl4.PNG)
#
Pada proses training model cukup smooth dan model konvergen pada epochs sekitar 100. Dari proses ini, kita memperoleh nilai root_mean_squred_error akhir sebesar sekitar 0.15 dan error pada data validasi sebesar 0.23.



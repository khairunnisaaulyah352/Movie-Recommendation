# Movie Recommendation
oleh : Khairunnisa Aulyah

## Domain Proyek
Dalam sejarahnya, film menjadi media yang sangat berpengaruh jika dibandingkan  dengan  media-media yang lain karena unsur-unsur film dalam bentuk audio dan visual dapat memberikan  kekuatan  universal komunikasi. Pesan-pesan komunikasi terwujud dalam cerita dan misi yang dibawa dalam bentuk drama, action, komedi, dan horor.

Dikarenakan  dengan  semakin  ramainya  penggemar  film. Terkadang  para  penggemar  film  mengalami  kebingungan  saat  mencari  informasi film di internet. Penggemar film terkadang mencari informasi film di beberapa situs  untuk  dibandingkan, tak jarang situs-situs tersebut mempunyai informasi yang berbeda-beda.

Berdasarkan permasalahan tersebut, penelitian ini dilakukan untuk menciptakan sebuah rekomendasi dengan teknik Content Based Filtering. Dengan menggunakan teknik ini sistem akan merekomendasikan item yang mirip dengan item yang disukai pengguna di masa lalu dan juga sistem akan merekomendasikan item yang sama dengan yang telah Anda lihat.

## Business Understanding
###Problem Statement
- Berdasarkan data mengenai pengguna, bagaimana membuat sistem rekomendasi yang menyarankan item serupa yang pernah disukai di masa lalu atau sedang dilihat di masa kini kepada pengguna?

###Goals
- Membuat model development yang dapat menghasilkan sejumlah rekomendasi movie yang dipersonalisasi untuk pengguna.

###Solution Statement
- Menggunakan teknik content-based filtering yang akan merekomendasikan sejumlah movie berdasarkan item serupa yang pernah disukai  di masa lalu atau sedang dilihat di masa kini kepada pengguna.

## Data Understanding
penelitian ini menggunakan dataset rekomendasi film yang dapat diunduh di: [Movie Recommenadtion](https://www.kaggle.com/datasets/rohan4050/movie-recommendation-data/download?datasetVersionNumber=1)

informasi dataset: 
- Dataset yang digunakan memiliki 4 berkas dengan format file csv
- Terdapat missing value dalam dataset ini

variabel yang digunakan dalam datset ini yaitu: 
- link : merupakan link dari movie
- movies : merupakan movie yang tersedia
- tag : merupakan tanda dari movie tersebut
- rating : merupakan nilai dari movie tersebut

tahapan Data Understanding yang dilakukan dalam penelitian ini yaitu:
1. .info() : menampilkan informasi dari variabel yang digunakan. Dalam penelitian ini kita menggunakan 4 variabek

## Data Preprocessing
Tahapan yang dilakukan paada saat data preprocessing yaitu:

#### Menggabungkan Movie
 Pertama kita akan Mengindentifikasikan jumlah seluruh movie pada dataset menggunakan library numpy dan fungsi concatenate untuk menggabungkan beberapa file dengan movieId sebagai acuan. output:
Jumlah seluruh data movie berdasarkan movieID:  9742

### Menggabungkan Seluruh User
Menggunakan  fungsi concatenate dari library numpy untuk menggabungkan seluruh data pada kategori variabel User. 
output: 
Jumlah seluruh user:  610
 
### Mengetahui Jumlah Rating
Menggabungkan file link, tag, movies ke dalam dataframe movie_info kemudian menggabungkan dataframe rating dengan movie_info berdasarkan nilai movieId.Setelah itu menggunakan fungsi.groupby().sum() unutuk mendapatkan hasil jumlahnya
output: 

|        |          |        |               |            |          |          |              |
|-------:|---------:|-------:|--------------:|-----------:|---------:|---------:|-------------:|
|        | userId_x | rating |   timestamp_x |     imdbId |   tmdbId | userId_y |  timestamp_y |
|      1 |   329520 | 4215.0 | 1214572277395 | 24662435.0 | 185330.0 | 296055.0 | 8.173308e+11 |
|      2 |   217506 | 2265.0 |  749631499932 | 12484670.0 | 972840.0 |  72600.0 | 6.296298e+11 |
|      3 |    58988 |  678.0 |  209062937544 |  5887856.0 | 811304.0 |  30056.0 | 1.189162e+11 |
|      4 |     3078 |   33.0 |   12580104096 |   804195.0 | 219499.0 |      0.0 | 0.000000e+00 |
|      5 |    58716 |  602.0 |  194562210376 |  5539009.0 | 581238.0 |  46452.0 | 1.114626e+11 |
|    ... |      ... |    ... |           ... |        ... |      ... |      ... |          ... |
| 193581 |      368 |    8.0 |    3074218164 |  5476944.0 | 432131.0 |      0.0 | 0.000000e+00 |
| 193583 |      368 |    7.0 |    3074219090 |  5914996.0 | 445030.0 |      0.0 | 0.000000e+00 |
| 193585 |      368 |    7.0 |    3074219610 |  6397426.0 | 479308.0 |      0.0 | 0.000000e+00 |
| 193587 |      368 |    7.0 |    3074220042 |  8391976.0 | 483455.0 |      0.0 | 0.000000e+00 |
| 193609 |      362 |    8.0 |    3074315212 |   101726.0 |  37891.0 |      0.0 | 0.000000e+00 |

### Menggabungkan Data dengan Fitur Nama Movie
Pertama definisikan variabel all_movie_rate dengan variabel rating. Selanjutnya, untuk mengetahui nama movie dengan movieId tertentu, kita gabungkan data movies yang berisikan movieId, title dan genres berdasarkan movieId dan assign ke variabel all_movie_name dengan fungsi merge dari library pandas. 
output: 

|   | userId | movieId | rating | timestamp |
|--:|-------:|--------:|-------:|----------:|
| 0 |      1 |       1 |    4.0 | 964982703 |
| 1 |      1 |       3 |    4.0 | 964981247 |
| 2 |      1 |       6 |    4.0 | 964982224 |
| 3 |      1 |      47 |    5.0 | 964983815 |
| 4 | 1      | 50      | 5.0    | 964982931 |

### Menggabungkan Data dengan Fitur Tag
Menggabungkan variabel all_MOVE_name yang kita peroleh dari tahapan sebelumnya dengan fitur tag. Tujuannya untuk mengetahui tag setiap movie yang tersedia.
output: 
|   | userId | movieId | rating | timestamp |                       title |                                          genres |
|--:|-------:|--------:|-------:|----------:|----------------------------:|------------------------------------------------:|
| 0 |      1 |       1 |    4.0 | 964982703 |            Toy Story (1995) | Adventure\|Animation\|Children\|Comedy\|Fantasy |
| 1 |      1 |       3 |    4.0 | 964981247 |     Grumpier Old Men (1995) |                                 Comedy\|Romance |
| 2 |      1 |       6 |    4.0 | 964982224 |                 Heat (1995) |                         Action\|Crime\|Thriller |
| 3 |      1 |      47 |    5.0 | 964983815 | Seven (a.k.a. Se7en) (1995) |                               Mystery\|Thriller |
| 4 |      1 |      50 |    5.0 | 964982931 |  Usual Suspects, The (1995) |                        Crime\|Mystery\|Thriller |

## Data Preparation
tahapan yang dilakukan: 
### Mengatasi Misising Value
pada tahap ini kita akan mengecek missing value menggunakan fungsi .isnull().sum()
output:
<p>
 <img src="https://user-images.githubusercontent.com/71605581/208914642-e32ce397-b265-4177-a00c-a0e8f842ad6c.png" width="180"  height="200"> </p>
 <p> Gambar 1. output pencarian missing value </p>

seperti pada gambar diatas kita bahwa terdapat missing value pada fitur tag sebanyak 52549 sampel.Sehingga kita harus drop missing value ini menggunakan fungsi .dropna()
output:
<p> <img src="https://user-images.githubusercontent.com/71605581/208919124-58853544-4ee4-455a-9504-19fcb02aee10.png" width="180"  height="200"> </p>
 <p> Gambar 2. output drop missing value </p>

### Menyamakan Jenis movie
Terkadang, ada beberapa movie yang memeiliki nama sama tatpi kategori berbeda. Jika dibiarkan, hal ini bisa menyebabkan bias pada data. maka kita akan menghapus data yang duplikat dengan fungsi drop_duplicates(). Selanjutnya melakukan konversi data series menjadi list enggunakan fungsi tolist() dari library numpy.
output: 

- movie_id : 1554
- movie_name : 1554
- movie_genre : 1554

Tahap berikutnya, Membuat dictionary untuk menentukan pasangan key-value pada data movie_id, movie_name, movie_genre.
output:

|   | id |              movie_name |                                           genre |
|--:|---:|------------------------:|------------------------------------------------:|
| 0 |  1 |        Toy Story (1995) | Adventure\|Animation\|Children\|Comedy\|Fantasy |
| 1 |  2 |          Jumanji (1995) |                    Adventure\|Children\|Fantasy |
| 2 |  3 | Grumpier Old Men (1995) |                                 Comedy\|Romance |

## Model Development dengan Content Based Filtering

### TF-IDF Vectorizer
pertama kita akan Melakukan perhitungan idf pada data genre dan Mapping array dari fitur index integer ke fitur nama. kemudian Melakukan fit lalu ditransformasikan ke bentuk matrix. 
output:

(1554, 24)

kemudian mengubah vektor tf-idf dalam bentuk matriks dengan fungsi todense() dan membuat dataframe untuk melihat tf-idf matrix.output yang dihasilkan akan menunjukkan nama movie dengan nilai matriks pada setiap kategori 0.0 atau >0.0. hal ini berarti setiap movie_name masuk dalam kategori yang berbeda-beda

### Cosone similarity
Menghitung derajat kesamaan (similarity degree) antar movie dengan teknik cosine similarity pada matrix tf-idf.output menghasilkan keluaran berupa matriks kesamaan dalam bentuk array.

## Modeling and Result
Menggunakan sistem rekomendasi *Content Based Filtering* yaitu menyarankan item serupa yang pernah disukai di masa lalu atau sedang dilihat di masa kini kepada pengguna. Rekomendasi Movie dibawah bekerja berdasarkan kemiripan dataframe.
Parameter yang digunakan:
- nama_movie :  Nama movie (index kemiripan dataframe)
- similarity_data : Kesamaan dataframe, simetrik, dengan resto sebagai indeks dan kolom
- items : Mengandung kedua nama dan fitur lainnya yang digunakan untuk mendefinisikan kemiripan
- k : Banyaknya jumlah rekomendasi yang diberikan. 
Pada index ini, kita mengambil k dengan nilai similarity terbesar pada index matrix yang diberikan (i).
Movie yang pernah diliat pengguna adalah Hancock(2008)

|      |    id |     movie_name |                                     genre |
|-----:|------:|---------------:|------------------------------------------:|
| 1387 | 60074 | Hancock (2008) | Action\|Adventure\|Comedy\|Crime\|Fantasy |

Berdasarkan Movie tersebut terdapat 5 rekomendasi movie yang mirip dengan movie Hancock.
output:
|   |                                        movie_name |                              genre |
|--:|--------------------------------------------------:|-----------------------------------:|
| 0 |                     Forbidden Kingdom, The (2008) | Action\|Adventure\|Comedy\|Fantasy |
| 1 | Pirates of the Caribbean: The Curse of the Bla... | Action\|Adventure\|Comedy\|Fantasy |
| 2 | Kingsman: The Secret Service (2015)               | Action\|Adventure\|Comedy\|Crime   |
| 3 | Batman Forever (1995)                             | Action\|Adventure\|Comedy\|Crime   |
| 4 | Tomb Raider (2018)                                |         Action\|Adventure\|Fantasy |


## Evaluation
Sistem rekomendasi yang digunakan adalah *Content Based Filtering* yang mempelajari profil minat pengguna baru berdasarkan data dari objek yang telah dinilai pengguna. sistem ini bekerja dengan menyarankan item serupa yang pernah disukai di masa lalu atau sedang dilihat di masa kini kepada pengguna. Semakin banyak informasi yang diberikan pengguna, semakin baik akurasi sistem rekomendasi. 
Teknik *Content Based Filtering* akan Mengambil data dengan menggunakan argpartition untuk melakukan partisi secara tidak langsung sepanjang sumbu yang diberikan. kemudian akan mengambil data dengan similarity terbesar dari index yang ada dan drop nama_movie agar nama movie yang dicari tidak muncul dalam daftar rekomendasi.

- Movie Hancock (2008)

|      |    id |     movie_name |                                     genre |
|-----:|------:|---------------:|------------------------------------------:|
| 1387 | 60074 | Hancock (2008) | Action\|Adventure\|Comedy\|Crime\|Fantasy |

- 5 rekomendasi movie yang mirip dengan movie diatas.

|   |                                        movie_name |                              genre |
|--:|--------------------------------------------------:|-----------------------------------:|
| 0 |                     Forbidden Kingdom, The (2008) | Action\|Adventure\|Comedy\|Fantasy |
| 1 | Pirates of the Caribbean: The Curse of the Bla... | Action\|Adventure\|Comedy\|Fantasy |
| 2 | Kingsman: The Secret Service (2015)               | Action\|Adventure\|Comedy\|Crime   |
| 3 | Batman Forever (1995)                             | Action\|Adventure\|Comedy\|Crime   |
| 4 | Tomb Raider (2018)                                |         Action\|Adventure\|Fantasy |

Dilihat dari hasil rekomendasi diatas bahwa masing-masing genre memiliki kesamaan terhadap genre dari movie Hancock(2008). Sehingga rekomendasi yang dihasilkan berhasil.

Referensi : [1] [MaylidaIzattulWardah, S. D. (2022). Implementasi Machine Learning Untuk Rekomendasi Film Di Imdb Menggunakan Collaborative Filtering Berdasarkan Analisa Sentimen IMDB. Jurnal Manajemen Informatika Jayakarta.](https://journal.stmikjayakarta.ac.id/index.php/JMIJayakarta/article/view/868/552)

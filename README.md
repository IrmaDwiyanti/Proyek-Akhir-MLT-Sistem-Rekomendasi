# Laporan Proyek Machine Learning - Irma Dwiyanti

## Project Overview

### Latar Belakang

Di era digital saat ini, jumlah buku dan konten bacaan yang tersedia secara online sangatlah besar. Hal ini menyebabkan pengguna mengalami kesulitan dalam menemukan buku yang sesuai dengan minat dan preferensi mereka, sebuah fenomena yang dikenal sebagai *information overload* (Bawden & Robinson, 2009). Sistem rekomendasi menjadi solusi penting untuk membantu mengatasi masalah ini dengan memberikan saran buku yang relevan berdasarkan perilaku dan preferensi pengguna sebelumnya.

**Collaborative filtering** merupakan metode yang banyak digunakan dalam sistem rekomendasi karena kemampuannya untuk memanfaatkan data interaksi pengguna tanpa perlu informasi mendalam tentang isi produk (Ricci et al., 2015). Namun, metode ini masih memiliki tantangan dalam hal skalabilitas dan cold start pada pengguna atau item baru. Pendekatan model-based dengan embedding menggunakan neural network menawarkan solusi dengan merepresentasikan pengguna dan item dalam ruang fitur yang dapat dipelajari secara efisien, sehingga meningkatkan kualitas rekomendasi (He et al., 2017).

### Alasan Pemecahan Masalah

Mengembangkan sistem rekomendasi buku yang efektif sangat penting untuk meningkatkan pengalaman pengguna, mendorong literasi, dan membantu penulis atau penerbit menjangkau audiens yang tepat. Dengan menggunakan model neural network berbasis embedding, sistem diharapkan mampu memberikan rekomendasi yang lebih personal dan akurat dibandingkan teknik tradisional.

### Referensi
- Bawden, D., & Robinson, L. (2009). The dark side of information: Overload, anxiety and other paradoxes and pathologies. *Journal of Information Science*, 35(2), 180-191. https://doi.org/10.1177/0165551508095781
- Ricci, F., Rokach, L., & Shapira, B. (2015). Recommender systems handbook. Springer.
- He, X., Liao, L., Zhang, H., Nie, L., Hu, X., & Chua, T. (2017). Neural collaborative filtering. *Proceedings of the 26th International Conference on World Wide Web (WWW '17)*, 173-182. https://doi.org/10.1145/3038912.3052569

## Business Understanding

Pada bagian ini, dilakukan klarifikasi terkait masalah utama yang hendak diselesaikan dalam proyek sistem rekomendasi buku ini. Pemahaman mendalam tentang permasalahan yang dihadapi sangat penting agar solusi yang dihasilkan tepat sasaran dan efektif.

### Problem Statements

- Bagaimana cara memberikan rekomendasi buku yang relevan kepada pengguna berdasarkan data rating yang telah mereka berikan sebelumnya?

### Goals

- Membangun sistem rekomendasi buku yang mampu memberikan saran buku yang personal dan relevan berdasarkan pola rating pengguna.

### Solution Statements

Untuk mencapai tujuan tersebut, proyek ini akan mengeksplorasi beberapa pendekatan solusi sebagai berikut:

1. **Collaborative Filtering berbasis Matrix Factorization**  
   Metode ini menggunakan data rating untuk mempelajari representasi laten dari pengguna dan buku. Dengan memfaktorkan matriks rating, sistem dapat memprediksi rating untuk buku yang belum pernah diberi rating oleh pengguna, sehingga memberikan rekomendasi yang sesuai.

2. **Content-Based Filtering (sebagai alternatif)**  
   Menggunakan fitur buku (misal genre, pengarang, sinopsis) untuk merekomendasikan buku yang mirip dengan buku yang sudah disukai pengguna. Metode ini berguna untuk mengatasi masalah cold-start item.

3. **Hybrid Approach**  
   Menggabungkan collaborative filtering dan content-based filtering untuk memanfaatkan kelebihan keduanya, meningkatkan akurasi rekomendasi, dan mengurangi kelemahan masing-masing metode.

Pendekatan utama yang digunakan dalam proyek ini adalah collaborative filtering dengan embedding neural network karena mampu menangkap pola preferensi pengguna secara efektif tanpa memerlukan fitur domain secara eksplisit.

## Data Understanding

Dataset dapat diunduh di: [Kaggle Book Dataset](https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset)

Dataset yang digunakan adalah dataset buku dari Kaggle yang berisi informasi buku, pengguna, dan rating.  
Dataset ini terdiri dari:  
### Books.csv

Beikut adalah informasi terkait daataset Books.csv :

| Kolom               | Deskripsi                       |
|---------------------|---------------------------------|
| `ISBN`              | Kode unik ISBN buku             |
| `Book-Title`        | Judul buku                     |
| `Book-Author`       | Nama penulis buku              |
| `Year-Of-Publication` | Tahun terbit buku             |
| `Publisher`         | Nama penerbit                  |
| `Image-URL-S`       | URL gambar buku ukuran kecil   |
| `Image-URL-M`       | URL gambar buku ukuran sedang  |
| `Image-URL-L`       | URL gambar buku ukuran besar   |

Ringkasan informasi dataset :

![image](https://github.com/user-attachments/assets/2973c793-6fe6-4317-a641-c7724d280205)

**Berikut adalah insight dari gambar di atas :**

- Ukuran Data Besar: 271.360 baris dan 8 kolom.
- Tipe Data Teks: Semua kolom (ISBN, judul, penulis, dll.) berupa teks (object).
- Nilai Hilang: Ada nilai kosong pada kolom Book-Author, Publisher, dan Image-URL-L.
- Memori: Menggunakan sekitar 16.6 MB memori.

### Ratings.csv

Beikut adalah informasi terkait daataset Ratings.csv :

| Kolom         | Deskripsi                                           |
|---------------|-----------------------------------------------------|
| `User-ID`     | ID unik pengguna (angka besar)                      |
| `ISBN`        | Kode ISBN buku (string gabungan huruf dan angka)   |
| `Book-Rating` | Nilai rating dari 0 hingga 10                        |

Berikut isi dataset rating:

![image](https://github.com/user-attachments/assets/bae94ffb-fb45-485c-a610-f95769f15da8)

**Jumlah baris:** 1.149.780

*Catatan:*  
`User-ID` dan `ISBN` akan dilakukan encoding untuk optimasi sistem rekomendasi berbasis Collaborative Filtering. Data rating ini merupakan data utama dalam pembangunan model rekomendasi.

### Users.csv

Beikut adalah informasi terkait daataset Ratings.csv :

| Kolom     | Deskripsi         |
|-----------|-------------------|
| `User-ID` | ID unik pengguna  |
| `Location`| Lokasi pengguna   |
| `Age`     | Usia pengguna     |

**Jumlah baris:** 278.858

![image](https://github.com/user-attachments/assets/13f1bee7-2077-4289-8450-fb3045d5f663)

Dari hasil visualisasi, dapat dilihat bahwa lebih dari 700 ribu pengguna memberikan rating 0 terhadap buku, yang mengindikasikan adanya ketidakseimbangan distribusi data (imbalanced data). Oleh karena itu, perlu dilakukan penyesuaian pada data untuk menciptakan proporsi yang lebih seimbang, sehingga sistem rekomendasi yang dibangun dapat bekerja secara lebih efektif.

## Data Preparation

Tahapan penyiapan data dilakukan untuk memastikan bahwa data yang digunakan bersih, terstruktur, dan optimal bagi proses pelatihan model. Berikut adalah teknik-teknik yang diterapkan pada proyek ini:

1. ⚖️ Penanganan Data Tidak Seimbang (Handling Imbalanced Data)
Analisis awal menunjukkan bahwa mayoritas pengguna memberikan rating 0 terhadap buku, menciptakan ketidakseimbangan data (imbalanced). Ketidakseimbangan ini dapat mengganggu performa model. Oleh karena itu, semua entri dengan rating 0 dihapus (drop) dari dataset. Meskipun jumlah data berkurang secara signifikan, distribusi rating menjadi lebih proporsional, yang diharapkan mampu meningkatkan akurasi model rekomendasi.

2. 🔢 Encoding
Kolom User-ID dan ISBN berisi data numerik besar dan string acak yang tidak berurutan. Agar dapat diproses oleh algoritma machine learning, kedua fitur ini dikodekan ulang (encoded) menjadi indeks bilangan bulat (integer index). Proses ini juga mengoptimalkan efisiensi memori selama pelatihan.

3. 🔀 Pengacakan Dataset (Randomizing Dataset)
Dataset diacak agar distribusi data menjadi acak dan representatif. Langkah ini penting untuk mengurangi risiko overfitting serta memastikan bahwa model mendapatkan paparan data yang bervariasi saat pelatihan dan validasi.

4. 📏 Standarisasi Data (Data Standardization)
Rating buku berada dalam rentang 0–10. Untuk menyelaraskan skala nilai dan mempermudah proses pelatihan, nilai rating dinormalisasi ke rentang 0–1. Ini dilakukan agar setiap fitur memiliki kontribusi yang setara dalam pembelajaran model serta menghindari bias yang mungkin muncul dari perbedaan skala.

5. ✂️ Pemisahan Data (Data Splitting)
Dataset dibagi menjadi dua bagian:

Training set: 80% dari data, digunakan untuk melatih model

Validation set: 20% dari data, digunakan untuk mengevaluasi kinerja model

Pembagian ini bertujuan untuk mengevaluasi generalisasi model secara obyektif terhadap data yang belum pernah dilihat sebelumnya.

## Modeling

Pada tahap ini, dibangun sebuah model rekomendasi yang mempelajari hubungan antara pengguna dan buku melalui pendekatan embedding. Model ini bertujuan untuk menghitung skor kecocokan antara masing-masing user dan buku.

🧠 Arsitektur Model RecommenderNet
Model dibangun dengan menggunakan kelas khusus bernama RecommenderNet. Beberapa parameter utama yang digunakan dalam proses ini antara lain:

- num_users: Jumlah total pengguna unik
- num_isbn: Jumlah total buku berdasarkan ISBN unik
- embedding_size: Ukuran dimensi dari vektor embedding yang akan digunakan untuk representasi user dan buku

🔗 Proses Embedding
Model melakukan embedding terhadap ID pengguna dan ISBN buku menjadi vektor berdimensi tetap (embedding_size). Tujuannya adalah untuk menangkap fitur laten dari interaksi user dan buku dalam ruang vektor berdimensi rendah.

Semakin besar nilai embedding_size, semakin banyak informasi yang bisa ditangkap. Namun, jika terlalu besar, model bisa mengalami overfitting. Oleh karena itu, pada proyek ini digunakan Optuna untuk melakukan hyperparameter tuning dan menemukan nilai embedding yang paling optimal.

⚙️ Mekanisme Skoring
Setelah representasi embedding terbentuk:

Vektor user dan buku dikalikan menggunakan operasi dot product untuk menghasilkan skor kecocokan.

Tambahan bias juga dapat diberikan baik pada sisi user maupun buku untuk meningkatkan akurasi.

Output skor kecocokan kemudian dinormalisasi ke dalam rentang [0, 1] menggunakan fungsi aktivasi sigmoid.

⚗️ Kompilasi Model
Model dikompilasi dengan konfigurasi berikut:

Loss Function: binary_crossentropy, digunakan karena model menilai pasangan user-buku sebagai pasangan cocok atau tidak cocok

Optimizer: Adam dengan learning rate sebesar 0.001

📚 Hasil Rekomendasi

![image](https://github.com/user-attachments/assets/c6864bb0-325b-489f-883c-50ce502b993e)

Setelah pelatihan selesai, model mampu memberikan Top-10 rekomendasi buku untuk setiap pengguna berdasarkan skor kecocokan yang dihitung. Rekomendasi ini diharapkan mampu merefleksikan preferensi pengguna berdasarkan pola rating historis

## Evaluation

Untuk mengukur kinerja model rekomendasi yang dibangun, proyek ini menggunakan metrik Root Mean Square Error (RMSE). RMSE merupakan salah satu metode evaluasi standar yang digunakan dalam model regresi untuk mengetahui seberapa besar perbedaan antara nilai yang diprediksi dan nilai sebenarnya.

📐 Apa itu RMSE?
RMSE mengukur rata-rata akar dari kuadrat selisih antara nilai prediksi (ŷ) dan nilai aktual (y). Rumusnya adalah sebagai berikut:

![image](https://github.com/user-attachments/assets/46700c68-0ada-4793-accc-d4a1a58ecbcc)

*Keterangan:

- y: Nilai aktual (observasi)
- ŷ: Nilai prediksi
- 𝑖: Indeks data
- 𝑛: Jumlah total data

Semakin kecil nilai RMSE, maka semakin baik model dalam memprediksi karena hasil prediksinya mendekati data aktual. Nilai RMSE yang rendah menandakan tingkat kesalahan prediksi yang kecil, yang berarti model bekerja secara lebih akurat.

📈 Hasil Evaluasi
Berdasarkan hasil pelatihan model, diperoleh nilai RMSE sebesar 0.1860. Nilai ini mengindikasikan bahwa model memiliki performa yang cukup baik dalam mempelajari pola interaksi antara pengguna dan buku. Meskipun hasilnya sudah tergolong bagus, nilai RMSE ini masih dapat diminimalkan lebih lanjut melalui optimasi dan tuning parameter tambahan di masa mendatang.

Visualisasi performa RMSE selama proses pelatihan juga ditampilkan pada grafik berikut untuk memberikan gambaran perkembangan model secara lebih mendalam.

![image](https://github.com/user-attachments/assets/b41ca685-225e-4d6d-a619-5958f4ec7d0b)

## Penutup
Proyek ini berhasil membangun sistem rekomendasi buku menggunakan model-based collaborative filtering dengan neural network embedding. Model ini mampu mempelajari representasi laten dari pengguna dan buku berdasarkan data rating, sehingga dapat memprediksi preferensi pengguna dengan cukup baik.

Pendekatan ini terbukti efektif dan efisien untuk skala besar karena prediksi dilakukan melalui inferensi model, bukan pencocokan manual. Ke depan, sistem ini masih bisa ditingkatkan dengan menambahkan fitur konten (content-based), regulasi model, dan evaluasi dengan metrik seperti RMSE.

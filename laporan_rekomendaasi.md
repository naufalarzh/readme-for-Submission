# Laporan Proyek Machine Learning - Naufal Arkan Zhafran

## 1. Project Overview

Sistem rekomendasi memainkan peranan penting dalam membantu pengguna memilih konten yang relevan dan menarik di tengah banyaknya pilihan yang tersedia. Proyek ini mengangkat masalah pembuatan sistem rekomendasi anime yang dapat membantu pengguna menemukan anime sesuai preferensi mereka.  
Masalah ini penting untuk diselesaikan karena banyaknya jumlah anime yang tersedia membuat pengguna kesulitan memilih anime yang sesuai minat dan selera mereka secara manual. Sistem rekomendasi dapat meningkatkan pengalaman pengguna dan meningkatkan engagement pada platform penyedia anime.

Beberapa penelitian terkait telah menunjukkan bahwa Collaborative Filtering adalah metode yang efektif dalam sistem rekomendasi, terutama pada konteks konten hiburan seperti film dan anime (Herlocker et al., 2004).  
Dataset yang digunakan berasal dari Kaggle “Anime Recommendations Database” yang berisi data rating pengguna terhadap berbagai anime.

Referensi:  
Herlocker, J. L., Konstan, J. A., Terveen, L. G., & Riedl, J. T. (2004). Evaluating collaborative filtering recommender systems. ACM Transactions on Information Systems (TOIS), 22(1), 5-53.  
https://doi.org/10.1145/963770.963772

## 2. Business Understanding

### Problem Statements  
- Bagaimana cara membuat sistem rekomendasi anime yang dapat memberikan rekomendasi personalisasi berdasarkan rating pengguna?  
- Bagaimana memprediksi rating anime yang belum pernah diberikan oleh user?  
- Bagaimana menangani masalah sparsity dan skala data besar dalam sistem rekomendasi?

### Goals  
- Membangun sistem rekomendasi berbasis User-Based Collaborative Filtering yang dapat merekomendasikan Top-N anime untuk setiap user.  
- Menghasilkan prediksi rating anime yang akurat untuk anime yang belum dirating oleh user.  
- Mengoptimalkan proses dengan sampling data agar model dapat berjalan di lingkungan Colab.

### Solution Approach  
- **User-Based Collaborative Filtering**: Menghitung kemiripan antar user berdasarkan rating untuk merekomendasikan anime.  


## 3. Data Understanding

Dataset terdiri dari dua file utama:  
- `anime.csv` dengan 12.294 data dan 7 fitur yaitu:  
  - `anime_id`: ID unik anime  
  - `name`: Nama anime  
  - `genre`: Genre anime  
  - `type`: Tipe anime (TV, Movie, dll)  
  - `episodes`: Jumlah episode  
  - `rating`: Rating rata-rata anime  
  - `members`: Jumlah anggota yang memberi rating  

- `rating.csv` dengan 7.813.737 data dan 3 fitur:  
  - `user_id`: ID unik pengguna  
  - `anime_id`: ID anime yang dirating  
  - `rating`: Rating yang diberikan user (skala 1-10, 0 artinya belum menilai)

Sumber dataset:  
[Kaggle Anime Recommendations Database](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database)

Missing value pada `anime.csv` sebagai berikut:  
- `genre`: 62 missing  
- `type`: 25 missing  
- `rating`: 230 missing

### Exploratory Data Analysis (EDA)  
![rating](https://github.com/user-attachments/assets/b542670f-a83d-495a-a8dd-a211ca1acb99)
- Berdasarkan distribusi, mayoritas pengguna memberikan penilaian di sekitar nilai 8, yang merupakan rating dengan frekuensi tertinggi. Meski begitu, masih banyak juga pengguna yang memberikan rating rendah, terutama antara 2 hingga 4, yang menunjukkan adanya ketidakpuasan dari sebagian pengguna. Rating tinggi seperti 9 dan 10 memang ada, tetapi jumlahnya lebih sedikit dibandingkan rating 8.
  
![genre](https://github.com/user-attachments/assets/850fc098-5493-414c-ac71-583da799abf4)
- Berdasarkan grafik, genre anime yang paling populer adalah Comedy, dengan jumlah judul yang jauh lebih banyak dibandingkan genre lainnya. Selain itu, genre seperti Action, dan SCI-FI juga cukup populer dan sering muncul dalam berbagai judul anime.
  
![anime populer](https://github.com/user-attachments/assets/f5c189b4-abb3-4b0f-a515-843b297c6b74)
- Grafik ini menunjukkan 10 anime terpopuler berdasarkan jumlah anggota komunitas (members) yang mengikutinya. Anime Death Note merupakan anime paling populer dengan jumlah anggota komunitas tertinggi, mencapai lebih dari 1 juta member. Dalam urutan berikutnya, Shingeki no Kyojin dan Sword Art Online juga sangat populer

## 4. Data Preparation

Tahapan yang dilakukan:  
- Mengisi missing value pada kolom `genre` dan `type` dengan "Unknown".  
- Mengisi missing value pada kolom `rating` dengan nilai rata-rata rating anime.  
- Menghapus data rating yang bernilai 0 (artinya user belum memberikan rating).  
- Memfilter 1000 anime terpopuler berdasarkan jumlah anggota (`members`).  
- Memfilter user aktif yang memberikan minimal 50 rating.  
- Melakukan sampling data rating sebanyak 5% untuk mengurangi ukuran data agar dapat diproses di Google Colab.  
- Membuat matriks user-item dengan `user_id` sebagai baris dan `anime_id` sebagai kolom untuk proses collaborative filtering.

Alasan tahapan:  
- Mengisi missing value agar model mendapatkan data yang lengkap.  
- Memfilter untuk menghilangkan data sparsity yang berlebihan dan mengurangi beban komputasi.  
- Sampling dilakukan karena ukuran data asli sangat besar dan tidak dapat diproses secara langsung.

## 5. Modeling

Model yang dibuat adalah sistem rekomendasi menggunakan User-Based Collaborative Filtering:  
- Menghitung similarity antar user dengan metode **cosine similarity**.  
- Melakukan prediksi rating anime yang belum dirating berdasarkan bobot similarity dengan user lain.  
- Menghasilkan Top-N rekomendasi anime dengan prediksi rating tertinggi untuk setiap user.

Output utama:  
- Rekomendasi Top-10 anime untuk user tertentu.

Kelebihan model:  
- Mampu memanfaatkan preferensi user lain yang mirip.  
- Tidak membutuhkan informasi konten anime secara eksplisit.

Kekurangan model:  
- Sensitif terhadap data sparsity dan cold-start problem (user baru tanpa rating).  
- Memerlukan optimasi agar scalable pada data besar.

## 6. Evaluation

Metrik evaluasi yang digunakan:  

- **Root Mean Squared Error (RMSE)**  
![download](https://github.com/user-attachments/assets/d67f7ae8-6351-4caf-a75c-ef5edd30e1bb)


- **Mean Absolute Error (MAE)**  
![MAE](https://github.com/user-attachments/assets/f561812d-7c1e-4d45-9507-2d9a679e40e4)


Dimana:  
- \( y_i \) adalah rating asli,  
- \( \hat{y}_i \) adalah rating hasil prediksi,  
- \( N \) adalah jumlah sampel.


**Hasil Evaluasi**
- RMSE: 0.1537
- MAE: 0.0938

Hasil evaluasi menunjukkan nilai RMSE dan MAE yang cukup baik mengindikasikan model mampu melakukan prediksi rating dengan akurasi yang dapat diterima.

---



# Laporan Proyek Machine Learning - Naufal Arkan Zhafran

## Proyek Klasifikasi Deteksi Diabetes

## 1. Domain Proyek

### Latar Belakang

Diabetes adalah penyakit kronis yang mempengaruhi jutaan orang di seluruh dunia. Deteksi dini sangat penting untuk mencegah komplikasi serius. Proyek ini bertujuan membangun model klasifikasi yang dapat memprediksi risiko diabetes berdasarkan data kesehatan seperti usia, BMI, tekanan darah, dan lainnya.

### Referensi

- WHO, "Diabetes", [https://www.who.int/news-room/fact-sheets/detail/diabetes](https://www.who.int/news-room/fact-sheets/detail/diabetes)
- Smith et al., "Machine Learning for Early Detection of Diabetes", _Journal of Healthcare Informatics_, 2023.

## 2. Business Understanding

### Problem Statement

Bagaimana memprediksi apakah seseorang berisiko diabetes berdasarkan data kesehatan yang tersedia?

### Goals

Membangun model klasifikasi dengan performa baik untuk mendeteksi risiko diabetes.

### Solution Statement

- Membangun model baseline menggunakan Logistic Regression.
- Membangun model alternatif menggunakan Random Forest dengan hyperparameter tuning.
- Memilih model terbaik berdasarkan metrik evaluasi yang relevan seperti F1-score dan ROC-AUC.

## 3. Data Understanding

### Dataset

Dataset yang digunakan adalah `diabetes.csv` dengan total data sebanyak (jumlah baris data). Dataset ini memiliki 9 kolom yaitu:

- Pregnancies: Jumlah Kehamilan
- Glucose: Konsentrasi glukosa plasma 2 jam dalam tes toleransi glukosa oral
- BloodPressure: Tekanan darah diastolik (mmHg)
- SkinThickness: Ketebalan lipatan kulit trisep (mm)
- Insulin: Insulin serum 2 jam (mu U/ml)
- BMI: Indeks massa tubuh (berat dalam kg/(tinggi dalam m)^2)
- DiabetesPedigreeFunction: Fungsi silsilah diabetes
- Age: Usia (tahun)
- Outcome (target: 0 = tidak diabetes, 1 = diabetes)

### Missing Value
Jumlah missing values per kolom:
- Pregnancies:                   0
- Glucose:                       5
- BloodPressure:                35
- SkinThickness:               227
- Insulin:                     374
- BMI:                          11
- DiabetesPedigreeFunction:      0
- Age:                           0
- Outcome:                       0

### Data Duplikat: 0

### Outlier
Jumlah outlier per kolom (IQR):
- Pregnancies: 4
- Glucose: 0
- BloodPressure: 14
- SkinThickness: 87
- Insulin: 346
- BMI: 8
- DiabetesPedigreeFunction: 29
- Age: 9

### Jumlah data: (768, 9)

semua data digunakan dalam pengerjaan model ini,data yang missing / null di isi dengan nilai median


### Sumber Data

[Pima Indians Diabetes Database
](https://www.kaggle.com/datasets/uciml/pima-indians-diabetes-database)

### Eksplorasi Data

- Statistik deskriptif
- Visualisasi distribusi fitur: histogram
- Korelasi antar variabel menggunakan heatmap

## 4. Data Preparation

### Teknik Data Preparation

- Mengganti nilai 0 pada fitur tertentu (Glucose, BloodPressure, SkinThickness, Insulin, BMI) dengan nilai median karena dianggap missing value.
- Melakukan imputasi nilai missing dengan median.
- Melakukan standarisasi fitur numerik menggunakan StandardScaler dari scikit-learn.
- Memisahkan data menjadi data latih dan data uji dengan perbandingan 80:20.

### Alasan Teknik

Nilai 0 pada fitur-fitur tersebut tidak masuk akal secara medis sehingga diimputasi agar model tidak bias. Standarisasi diperlukan agar model tidak dipengaruhi skala fitur yang berbeda-beda.

## 5. Modeling

### Model yang Digunakan dan Cara Kerja Model  

#### Logistic Regression  
Logistic Regression adalah model linear yang digunakan untuk memprediksi probabilitas suatu kelas (dalam kasus ini diabetes atau tidak diabetes). Model ini menghitung weighted sum dari fitur input yang kemudian dilewatkan ke fungsi sigmoid untuk mendapatkan probabilitas prediksi. Logistic Regression bekerja baik untuk data yang memiliki hubungan linear antara fitur dan target, serta interpretatif karena memberikan bobot kontribusi tiap fitur.

#### Random Forest  
Random Forest adalah algoritma ensemble berbasis decision tree. Model ini membangun banyak decision tree dari subset data yang berbeda dan menghasilkan prediksi akhir berdasarkan voting mayoritas dari semua tree. Random Forest mampu menangani hubungan non-linear antar fitur, robust terhadap outlier, dan tidak mudah overfitting karena pengacakan subset data dan fitur.

### Parameter dan Proses  
- Logistic Regression menggunakan default parameters.  
- Random Forest dituning hyperparameternya:  
  - n_estimators: 100, 200  
  - max_depth: 4, 6, 8  
  - min_samples_split: 2, 5  
  - min_samples_leaf: 1, 2  

### Kelebihan dan Kekurangan  
- **Logistic Regression**: cepat, interpretatif, namun kurang kuat menangkap pola kompleks.  
- **Random Forest**: mampu menangkap pola non-linear dan interaksi fitur, tahan terhadap overfitting, namun lebih kompleks dan membutuhkan komputasi lebih besar.

### Proses Improvement  
Random Forest ditingkatkan performanya melalui GridSearchCV untuk mencari kombinasi hyperparameter terbaik.

## 6. Evaluation

### Metrik Evaluasi

- Accuracy
- Precision
- Recall
- F1 Score
- ROC-AUC

### Penjelasan Metrik

- **Accuracy**: proporsi prediksi benar dari seluruh prediksi.
- **Precision**: proporsi prediksi positif yang benar.
- **Recall**: proporsi positif benar yang berhasil ditemukan model.
- **F1 Score**: harmonisasi precision dan recall, baik untuk data tidak seimbang.
- **ROC-AUC**: luas di bawah kurva ROC, mengukur kemampuan model membedakan kelas.

### Hasil Evaluasi

(Model 1: Logistic Regression)

- Accuracy: 0.7012987012987013
- Precision: 0.5869565217391305
- Recall: 0.5
- F1 Score: 0.54
- ROC-AUC: 0.8127777777777777

(Model 2: Random Forest)

- Accuracy: 0.7467532467532467
- Precision: 0.6666666666666666
- Recall: 0.5555555555555556
- F1 Score: 0.6060606060606061
- ROC-AUC: 0.8129629629629629

### Keterkaitan dengan Business Understanding  
Model klasifikasi ini bertujuan membantu mendeteksi risiko diabetes lebih dini, yang sejalan dengan tujuan business untuk mengurangi komplikasi kesehatan dan biaya perawatan. Model yang dikembangkan berhasil:  
- Menjawab problem statement dengan memprediksi risiko diabetes secara otomatis.  
- Mencapai goals dengan membangun model yang memiliki performa baik.  
- Memberikan solusi terukur, yaitu menggunakan Logistic Regression dan Random Forest dengan hyperparameter tuning, di mana Random Forest terbukti memberikan performa lebih baik.  
- Dampaknya signifikan untuk business: memungkinkan intervensi medis lebih cepat, mengurangi biaya perawatan jangka panjang, dan meningkatkan kualitas hidup pasien.

### Visualisasi

### Distribusi
![Distribusi](https://github.com/user-attachments/assets/c3ae6458-c6a8-4fee-9ac6-c0c4ed9182b7)


### Korelasi
![Korelasi](https://github.com/user-attachments/assets/31f8157c-ad9b-40f5-aa56-be18e83b9eb6)


---

### Logistic Regression
![Logistic 1](https://github.com/user-attachments/assets/14999eb6-b8c3-49a4-885e-e8c684f273b0)
![Logistic 2](https://github.com/user-attachments/assets/6d5cb1fd-de0c-408a-930c-736a75d0e912)


---

### Random Forest
![random 1](https://github.com/user-attachments/assets/5f572e42-f973-428a-88ed-e109b60cff1c)
![random 2](https://github.com/user-attachments/assets/8054bee5-27d0-4598-82af-e581fb4ae7ff)






### Kesimpulan

Model terbaik adalah Random Forest karena memiliki nilai F1 Score dan ROC-AUC tertinggi. Model ini dapat digunakan untuk membantu diagnosis dini diabetes.

# Laporan Proyek Machine Learning - Adrian Ramdhany

## Domain Proyek

Faktor lingkungan fisik merupakan faktor eksternal yang dapat mempengaruhi kinerja di tempat kerja, meliputi temperature, kelembaban udara, pencahayaan dan kebisingan yang memiliki peran signifikan dalam menciptakan suasana kerja optimal. [[1]](https://repository.uinjkt.ac.id/dspace/handle/123456789/34324) 

Banyaknya pekerja yang mengeluh tentang tidak sesuainya suara kebisingan dan cahaya sering mengganggu kesehatan setelah melakukan aktivitas pekerjaan. [[2]](https://doi.org/10.1007/s13762-016-1072-x)

Dalam proyek ini, saya menggunakan dataset yang telah diambil dari pembacaan nilai sensor menggunakan mikrokontroler dan nilai kenyamanan dari para pekerja sebanyak 2000 data dan disimpan dalam [Google Drive](drive.google.com).

## Business Understanding

Proyek ini dibuat untuk mendapatkan kenyamanan pada ruang kerja para pekerja, sehingga dapat diterapkan kriteria yang sesuai untuk kenyamanan pada seluruh ruang kerja di perusahaan.

### Problem Statements

- Bagaimana pengaruh faktor lingkungan terhadap kenyamanan kerja?
- Dapatkan sistem klasifikasi prediktif menentukan tingkat kenyamanan kerja berdasarkan hasil data dari sensor?

### Goals

- Proyek ini dapat mengidentifikasi hubungan variabel lingkungan dan kenyamanan kerja.
- Membangun model klasifikasi untuk memprediksi tingkat kenyamanan pekerja dari data sensor secara akurat dan sesuai untuk ruang kerja.

## Solution statements
- Menerapkan tiga algoritma klasifikasi yaitu *K-Nearest Neighbors (KNN)*, *Gradient Boosting (GB)*, dan *Random Forest* pada model.
- Membandingkan tiga algoritma klasifikasi yaitu *K-Nearest Neighbors (KNN)*, *Gradient Boosting (GB)*, dan *Random Forest* pada model.
- Melakukan evaluasi model berdasarkan metrik accuracy, precision, recall dan f1-score.
- Memilih model dengan performa terbaik untuk diterapkan sebagai sistem prediktif tingkat kenyamanan ruangan di perusahaan.

## Data Understanding
Data yang digunakan merupakan hasil dari pengukuran berbagai sensor lingkungan yang mencangkup temperature, humidity, noise, light, dan kadar oksigen serta label tingkat kenyamanan dari skor 1-5 oleh warga perusahaan.

### Sumber Dataset
Data diperoleh dari sensor-sensor yang terhubung oleh microcontroller ESP32 dengan mengumpulkan nilai-nilai lingkungan secara real-time.
- Temperature dan Humidity menggunakan sensor DHT22
- Noise menggunakan analog microphone (dB)
- Light menggunakan sensor lux
- Sensor oksigen menggunakan MQ-8



### Variabel-variabel pada dataset adalah sebagai berikut:
Column      | Non-Null Count | Dtype  
------      | -------------- |-----  
Temperature | 2000 non-null  |float64
Humidity    | 2000 non-null  |float64
Noise       | 2000 non-null  |float64
Light       | 2000 non-null  |float64
Oxygen      | 2000 non-null  |float64
Comfort     | 2000 non-null  |int64  

Dataset ini memiliki 6 variabel dengan keterangan sebagai berikut.

Variabel|Keterangan|
------|------|
Temperature|Suhu udara dalam derajat Celcius|
Humidity|Kelembaban udara dalam persen|
Noise|Tingkat kebisingan dalam desibel|
Light|Intensitas cahaya dalam lux|
Oxygen|Kadar oksigen dalam persen|
Comfort|Skor kenyamanan yang diberikan (1-5)|

## Data Preparation
Tahapan ini mencangkup EDA (Exploratory Data Analysis) seperti mendeskripsikan variabel, mencari dan menangani outliers, Univariate hingga Multivariate analysis.

### 1. Deskripsi Variabel
Column      | Non-Null Count | Dtype  
------      | -------------- |-----  
Temperature | 2000 non-null  |float64
Humidity    | 2000 non-null  |float64
Noise       | 2000 non-null  |float64
Light       | 2000 non-null  |float64
Oxygen      | 2000 non-null  |float64
Comfort     | 2000 non-null  |int64 
Berdasarkan output di atas terdapat 5 kolom dengan tipe data float64 dan 1 kolom dengan tipe data int64
### 2. Pengecekan Nilai Null
` `|0
------|------
Temperature|0|
Humidity|0|
Noise	|0|
Light	|0|
Oxygen	|0|
Comfort	|0|

dtype: int64

Pada data ini tidak ditemukan nilai null, sehingga tidak diperlukan proses imputasi atau penghapusan.

### 3. Visualisasi data
Outliers merupakan sample yang nilainya jauh dari data utama dan hasil pengamatan dan muncul sangat jarang serta berbeda dengan hasil yang lainnya.

<img src="https://github.com/agateboy/ProjekAkhirMachineLearningTerapan/blob/main/1.png" align="center"><br>

### 4. Menangani Outliers

Outliers ditangani menggunakan metode IQR (Interquartile Range) yang dapat menangani distribusi data outliers dengan baik.

```
Q1 = df.quantile(0.25)
Q3 = df.quantile(0.75)
IQR = Q3 - Q1
outliers = ((df < (Q1 - 1.5 * IQR)) | (df > (Q3 + 1.5 * IQR)))
```

Berikut merupakan hasil data setelah dilakukan penanganan outliers menggunakan metode IQR.

<img src="https://github.com/agateboy/ProjekAkhirMachineLearningTerapan/blob/main/2.png" align="center"><br>

### 5. Univariative Multivariative Analysis
`Univariative` merupakan analisis distribusi masing-masing fitur seperti Temperature, Humidity, Noise, Light, Oxygen dan Comfort secara individu.

<img src="https://github.com/agateboy/ProjekAkhirMachineLearningTerapan/blob/main/3.png" align="center"><br>

`Multivariative` melibatkan hubungan antar fitur seperti scatter matrix antar fitur untuk melihat pola clustering atau outliers.

<img src="https://github.com/agateboy/ProjekAkhirMachineLearningTerapan/blob/main/4.png" align="center"><br>

### 6. Correlation Matrix

Pengecekan korelasi antar fitur menggunakan `heatmap correlation matrix`

<img src="https://github.com/agateboy/ProjekAkhirMachineLearningTerapan/blob/main/5.png" align="center"><br>

Correlation matrix ini menunjukkan hubungan antara berbagai variabel seperti Temperature, Humidity, Noise, Light, Oxygen, dan Comfort. Nilai di dalam matriks adalah koefisien korelasi, yang berkisar dari -0.2 hingga 1:
- 1 menunjukkan korelasi positif sempurna.
- -0.2 menunjukkan korelasi negatif sempurna.

Dengan hasil:
- Noise memiliki korelasi negatif yang signifikan dengan Comfort (-0.37) yaitu semakin berisik lingkungan maka semakin renda tingkat kenyamanan.
- Light memiliki korelasi positif dengan Comfort (0.15) yaitu semakin baik pencahayaan mungkin meningkatkan kenyamanan.

- Oxygen berkorelasi positif dengan Comfort(0.16) mengindikasikan kadar oksigen yang lebih tinggi mungkin berkontribusi pada kenyamanan.

## Modeling
Pada tahap ini, terdapat tiga algpritma yang akan digunakan dalam membuat Machine Learning untuk melakukan analisis prediksi.

Dilakukan persiapan dataframe untuk menganalisis algoritma K-Nearest Neighboor (KNN), Gradient Boosting dan Random Forest.

#### 1. Model Development dengan K-Nearest Neighboor (KNN)

KNN bekerja dengan membandingkan jarak satu sampel ke sampel pelatihan lain dengan memilih sejumlah k tetangga terdekat dengan parameter `n-neighboors` dengan nilai `k=5`.

```python
knn = KNeighborsClassifier(n_neighbors=5)
```

Dari model development KNN didapatkan confusion matrix sebagai berikut:

<img src="https://github.com/agateboy/ProjekAkhirMachineLearningTerapan/blob/main/9.png" align="center"><br>

Model Development dengan KNN menghasilkan accuracy dengan nilai `61,67%`

#### 2. Model Development dengan Gradient Boosting

Gradient Boosting (GB) merupakan teknik Machine Learning berbasis ensemble yang membangun model dengan menggabungkan beberapa pohon keputusan secara bertahap.
`n_estimators=100` menentukan jumlah pohon keputusan yang akan dibangun dengan `learning_rate=0.1` yaitu menentukan seberapa besar kontribusi setiap pohon terhadap hasil akhir.


```python
gb = GradientBoostingClassifier(n_estimators=100, learning_rate=0.1, random_state=42)
```

Dari model development Gradient Boosting didapatkan confusion matrix sebagai berikut:

<img src="https://github.com/agateboy/ProjekAkhirMachineLearningTerapan/blob/main/10.png" align="center"><br>

Model development dengan GB menghasilkan accuracy dengan nilai `92,38%`

#### 3. Model Development dengan Random Forest

Random Forest merupakan algoritma ensemble berbasis decision tree yang menggabungkan banyak pohon untuk meningkatkan akurasi dan mengurangi overfitting. Algoritma ini efektif dalam klasifikasi karena tahan terhadap data yang tidak terstruktur dan memiliki kemampuan untuk menangani banyak fitur.

```python
rf_model = RandomForestClassifier(n_estimators=100, random_state=42, max_depth=10, n_jobs=-1)
```

Parameter `n_estimator` dengan jumlah `100` pohon (trees), `max-depth` dengan nilai `10`, dan `n-jobs` yang bernilai `-1` yaitu pekerjaan dilakukan secara paralel.

Dari model development Random Forest didapatkan confusion matrix sebagai berikut:

<img src="https://github.com/agateboy/ProjekAkhirMachineLearningTerapan/blob/main/11.png" align="center"><br>

Model development dengan RF menghasilkan accuracy sebesar `95,17%`

## Evaluation

Dari seluruh akurasi yang didapat dari keempat model, terdapat bar plot untuk melihat perbandingan nilai akurasi masing-masing model sebgai berikut.

<img src="https://github.com/agateboy/ProjekAkhirMachineLearningTerapan/blob/main/12.png" align="center"><br>

Berdasarkan gambar di atas dan evaluasi masing-masing model untuk mengetahui skor akurasi, F1 score, didapatkan model **Random Forest** merupakan model terbaik karena memiliki accuracy score dan F1 score tertinggi serta kesalahan klasifikasi yang paling sedikit.

## Kesimpulan

Berdasarkan hasil yang diperoleh setelah melakukan EDA dan pengujian model terbaik, dapat disimpulkan bahwa:
- Terdapat hubungan positif antara kadar oksigen dengan skor kenyamanan.
- Temperature dan Humidity memiliki hubungan yang signifikan dan saling mempengaruhi.
- Faktor kebisingan dan pencahayaan memiliki kontribusi yang lebih rendah secara statistik, namun tetap penting untuk menjaga kenyamanan ruangan.
- Proses pembersihan data outliers menggunakan IQR membantu meningkatkan kualitas data.
- Analisis univariate dan multivariate memberikan pemahaman mendalam tentang distribusi data dan hubungan antar variabel.

Setelah menguji data menggunakan 3 model Machine Learning didapatkan bahwa:
- **Random Forest** adalah model terbaik untuk memprediksi tingkat kenyamanan dengan akurasi dan F1-Score tertinggi.
- **Gradient Boosting** bekerja cukup baik pada data yang sudah dinormalisasi tetapi masih memiliki sedikit noise.
- **KNN** bekerja cukup baik namun masih sangat rentan terhadap noise.

## Referensi

[[1]](https://repository.uinjkt.ac.id/dspace/handle/123456789/34324) Sari, O. A. P. (2016). Hubungan Lingkungan Kerja Fisik dengan Kelelahan Kerja pada Kolektor Gerbang Tol Cililitan PT Jasa Marga Cabang Cawang Tomang Cengkareng tahun 2016 (Bachelor's thesis, FKIK UIN Jakarta).

[[2]](https://doi.org/10.1007/s13762-016-1072-x) Jafari, M., Mollasadeghi, A., Kargarsharifabad, H., & Rashedi, V. (2017). The effect of noise pollution on human health: A case study. International Journal of Environmental Science and Technology, 14(4), 687-694.

[[3]](https://doi.org/10.1177/1477153510367972) Boyce, P. R., Hunter, C., & Howlett, O. (2011). The Benefits of Daylight through Windows. Lighting Research & Technology, 43(2), 133-143.
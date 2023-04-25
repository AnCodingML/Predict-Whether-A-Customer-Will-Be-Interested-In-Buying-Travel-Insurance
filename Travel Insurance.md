# **Travel Insurance**
---
## Dataset
<p>Perusahaan tour & travels menawarkan paket asuransi perjalanan kepada pelanggannya yang sudah tercover dengan perlindungan covid. Perusahaan perlu mengetahui pelanggan yang akan tertarik untuk membelinya berdasarkan sejarah databasenya.<p>
<p>Asuransi ditawarkan kepada beberapa pelanggan pada tahun 2019 dan data yang diberikan diambil dari kinerja/penjualan paket selama periode itu. Data disediakan untuk hampir 2000 pelanggan sebelumnya dan diminta untuk membuat model Machine Learning yang dapat memprediksi apakah pelanggan akan tertarik untuk membeli paket asuransi perjalanan.</p>
    
## Fitur
**Age** - Usia Pelanggan <br>
**Employment Type** - Sektor Tempat Pelanggan Dipekerjakan <br>
**GraduateOrNot** - Apakah Pelanggan Lulusan Perguruan Tinggi Atau Tidak <br>
**AnnualIncome** - Pendapatan Tahunan Pelanggan Dalam Rupee India [Dibulatkan Hingga 50 Ribu Rupee Terdekat]<br>
**FamilyMembers** - Jumlah Anggota Dalam Keluarga Pelanggan<br>
**ChronicDisease** - Apakah Pelanggan Menderita Penyakit Utama Atau Kondisi Seperti Diabetes / BP Tinggi atau Asthama, dll. <br>
**FrequentFlyer** - Data Yang Diturunkan Berdasarkan Riwayat Pemesanan Tiket Pesawat Pelanggan Pada Setidaknya 4 Instansi Berbeda Dalam 2 Tahun Terakhir[2017-2019]. <br>
**EverTravelledAbroad** - Apakah Pelanggan Pernah Bepergian Ke Negara Asing<br>
**TravelInsurance** - Apakah Pelanggan Membeli Paket Asuransi Perjalanan Selama Penawaran Perkenalan Yang Diadakan Di Tahun 2019.

## Konsep Model
* Goals<br>
Meningkatkan Buy rate sebesar 20%
* Objectives<br>
Membuat Machine Learning yang dapat memprediksi *potensial customer* dan menentukan segmentasi pelanggan berdasarkan fitur yang paling berpengaruh
* BUSINESS METRICS <br>
Buy Rate
---
## EXPLORATORY DATA ANALYSIS (EDA)

![Kondisi Dataset](image/df.info().png)<br>
Berdasarkan perintah `df.info()` diketahui bahwa dataset memiliki 1987 data dengan pembagian fitur
- Numeric = Age, AnnualIncome, FamilyMembers
- Categorical = Employment Type, GraduateOrNot, ChronicDisease, FrequentFlyer, EverTravelledAbroad, TravelInsurance

![Outlier](image/outlier.png)<br>
berdasarkan grafik boxplot tidak ditemukan outlier sehingga tidak diperlukan melakukan handling outlier<br>

![Distribusi Data](image/distribusi_data.png)<br>
berdasarkan grafik diatas terlihat bahwa data numerik pada dataset memiliki sebaran normal<br>

![Numerical terhadap Target](image/numerical-target.png)<br>
pada grafik Age terhadap target terlihat orang yang memiliki umur 26 hingga 32 cenderung tidak membeli Travel Insurance dan umur 33 hingga 35 cukup seimbang antara yang membeli dan tidak membeli. Pada grafik AnnualIncome terhadap Target didapatkan bahwa orang yang memiliki AnnualIncome 1.4jt rupee cenderung membeli Travel Insurance. sedangkan pada grafik FamilyMembers jumlah anggota keluarga 6 kebawah cenderung tidak membeli Travel Insurance.<br>

![Categorical terhadap target](image/Categorical-target.png)<br>
Terlihat dari grafik yang cukup timpang perbandingannya adalah employment Type terhadap target dimana bidang Private Sector/Self Employment lebih besar perbandingan membeli travel insurance dibandingkan dengan Goverment Sector hal ini berkolerasi dengan nominal pendapatan dan jumlah customer yang dijelaskan pada grafik dibawah

![Employment Type terhadapa AnnualIncome](image/Employmen_Type-Target.png)<br>
Terlihat bahwa customer dengan Private Sectpr/ Self Employment cenderung lebih banyak melakukan perjalanan dan memiliki pendapatan lebih besar dibandingkan dengan goverment sector.<br>

![Correlation Fitur](image/correlation.png)<br>
Terlihat pada grafik korelasi diatas tidak ditemukan nilai redundant pada dataset<br>

---

## Data Pre-Processing

Kolom Unnamed: 0 di drop karena tidak memiliki makna yang jelas `df=df.drop(['Unnamed: 0'], axis=1)`<br>
![Drop Unnamed: 0](image/drop_unnamed.png)<br>
dan didapatkan duplikat data sebanyak **738** kemudian dilakukan penghapusan data duplikat `df = df.drop_duplicates(keep='first')`<br>

Dilakukan Label encoder pada fitur categorical agar dapat dilakukan pemodelan <br>
`object_cols = df.select_dtypes(include=['object']).columns`<br>
`for col in object_cols:`<br>
`   encoder = LabelEncoder()`<br>
`   df[col] = encoder.fit_transform(df[col])`<br>

Dataset yang dimiliki dilakukan splitting data dengan pembagian data train 70% dan data test 30% <br>
`X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)`

---
## Data Modeling Evaluation <br>

![Model Evaluation](image/evaluation.png)<br>
Berdasarkan hasil evaluasi didapatkan model machine learning yang paling baik adalah Gradient Boosting Classifier dan untuk evaluasi yang digunakan adalah **Precision** dikarenakan untuk mengurangi nilai false positif dengan harapan menghindari biaya marketing yang tidak seharusnya dikeluarkan kepada customer yang memang dia tidak mengambil atau kecil peluang dalam mengambil Travel Insurance.<br>
![Feature Important](image/important.png)<br>
Berdasarkan grafik feature important kita dapat melihat fitur yang paling berdampak terhadap model machine learning adalah<br>
* AnnualIncome
* FamilyMembers
* Age <br>

Dan dengan menggunakan shap value kita dapat melihat sebaran data pada fitur terhadap target<br>
![Model Evaluation](image/Shap_value.png)<br>
Terlihat pada fitur AnnualIncome, Age, dan FamilyMembers memiliki sebaran hampir sama dengan semakin besar nilainya maka ia akan semakin mempengaruhi terhadap hasil pemodelan dan pada fitur lainnya memiliki sebaran data kurang jelas sehingga sulit mengambil insight di dalamnya.<br>

---

## Business Recomendation<br>

Berdasarkan hasil EDA dan modeling kita dapat mengambil beberapa insight dan kesimpulan diantaranya:
* Disarankan memprioritaskan customers dengan AnnualIncome yang tinggi
* Memberikan penawaran spesial kepada customer yang belum pernah mengambil Travel Insurance
* Menambah beberapa fitur yang diharapkan dapat menambah kualitas Machine Learning diantaranya:<br>
**Gender<br>
Destination (Foreign/Domestic)<br>
Travel Duration<br>
Travel Time (Holiday Season/Not)<br>**

---
## Prediction Result<br>

Berdasarkan hasil rekomendasi bisnis maka dibuat dataset baru berdasarkan dataset lama dengan modifikasi bagian AnnualIncome dengan rentan 1,4jt rupee hingga 1,8jt rupee<br>

sebelum diterapkan machine learning didapatkan hasil seperti pada grafik dibawah:
![Before Modeling](image/before.png)<br>
Diperoleh data dengan total customer mengambil travel insurance sebesar 35.7% dengan jumlah 710 customers dari 1987 customers<br>

Kemudian diterapkan Machine Learning dengan dataset yang sudah dimodifikasi sebelumnya maka diperoleh hasil:
![After Modeling](image/after.png)<br>
Didapatkan hasil peningkatan pelanggan yang mengambil Travel Insurance sebesar 71.7% dengan jumlah customers 1425 dari 1987 customers.<br>

Hal ini menandakan berdasarkan simulasi, bisnis rekomendasi yang dibuat dan modeling yang dilakukan dapat meningkatkan buy rate sebesar 36%. Diharapkan hasil pemodelan ini menjadi gambaran untuk bagian lain dalam melakukan langkah selanjutnya.
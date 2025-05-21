# **E-Comerce Customer Churn Predict**

# **Business Problem Understanding**

**Context**

Perusahaan ini adalah sebuah platform e-commerce yang melayani berbagai kategori produk, termasuk fashion, elektronik, dan kebutuhan sehari-hari. Perusahaan telah beroperasi cukup lama dan memiliki basis pelanggan yang cukup besar, sebagaimana tercermin dari data pelanggan yang menunjukkan variasi preferensi kategori belanja, serta riwayat penggunaan layanan (tenure). Dalam operasionalnya, perusahaan ini juga memiliki kebijakan cashback yang bertujuan untuk meningkatkan loyalitas pelanggan. Selain itu, data menunjukkan adanya fitur pelayanan pelanggan, seperti penanganan komplain, yang mencerminkan upaya perusahaan dalam menjaga kualitas layanan.

Namun, di tengah kompetisi industri e-commerce yang sangat ketat, perusahaan mulai menghadapi tantangan dalam mempertahankan pelanggannya. Fenomena customer churn atau berpindahnya pelanggan ke platform lain menjadi perhatian utama, karena berdampak langsung terhadap pendapatan dan keberlanjutan bisnis.

Terdapat dua cara untuk mengatasi fenomena customer churn, agar perusahaan dapat mempertahankan keuntungan. Pertama yaitu mempertahankan customer lama agar menetap (customer retentions) dan cara kedua yaitu mencari customer baru (customer acquisition). Berdasarkan statistik dari berbagai industri bisnis, hasil riset menemukan bahwa **customer acquisition memiliki biaya 5 kali lipat lebih dari customer retention.** ([*source*](https://www.huify.com/blog/acquisition-vs-retention-customer-lifetime-value))

**Customer retention list :**

- Biaya campaign loyalitas

- Diskon khusus pelanggan

- Cashback

- Biaya email/pesan promosi

- Program loyalti

Kita asumsikan perusahaan per tahunnya menggelontorkan dana $100.000 untuk memaintain 1000 customer lama, maka :

- Customer Retention Cost :
$$
\text{Retention Cost per Customer} = \frac{100.000}{1000} = \$ 100\text{ per customer}
$$

- Customer Acquisition Cost:

$$
\text{Acquisition Cost per Customer} = \text{Retention Cost per Customer} \times 5 = \$ 500 \text{ per customer}
$$
([*Source*](https://www.clv-calculator.com/customer-costs/retention-costs-clv/retention-cost-formula/))

Dengan demikian diasumsikan untuk melakukan Customer Retention perusahaan menggunakan anggaran sebesar $100 per customer, sedangkan jika melakukan Customer Acquisition perusahaan mengeluarkan anggaran 5 kali lipat dari Customer Retention yaitu $500. 



**Problem Statement**

Perusahaan tidak memiliki sistem yang dapat mengidentifikasi pelanggan yang berisiko churn secara proaktif. Jika semua pelanggan diperlakukan sama tanpa segmentasi, maka strategi retensi menjadi tidak efisien dari segi biaya, waktu, dan sumber daya. Oleh karena itu, perusahaan perlu memahami karakteristik pelanggan yang berpotensi churn agar dapat memberikan perhatian khusus dan personalisasi layanan kepada mereka sebelum mereka benar-benar pergi.

**Goal**

Perusahaan ingin memiliki kemampuan untuk memprediksi kemungkinan seorang pelanggan akan churn, sehingga divisi pemasaran dapat mengambil tindakan preventif seperti memberikan penawaran khusus, diskon, atau kampanye personalisasi.

**Analytic Approach**

Langkah yang akan dilakukan antara lain:

- Melakukan analisis eksploratif pada data untuk memahami data leih dalam.

- Melakukan feature engineering untuk meningkatkan kualitas input ke dalam model.

- Membangun model klasifikasi yang dapat memprediksi kemungkinan pelanggan churn.

**Metric Evaluation**

- **Target:**

    - 0 : Tidak churn / masih menjadi pelanggan aktif

    - 1 : Churn / sudah tidak menggunakan layanan


- **False Positive (FP):**

    - Model memprediksi pelanggan akan churn, padahal sebenarnya tidak.

    - **Risiko bisnis:** Perusahaan akan melakukan Customer Retention (seperti diskon, promo, atau campaign retensi) untuk pelanggan yang sebenarnya tetap loyal.

    - **Estimasi kerugian:** $100 per pelanggan.


- **False Negative (FN):**

    - Model memprediksi pelanggan akan tetap, padahal sebenarnya churn.

    - **Risiko bisnis:** Perusahaan gagal mencegah churn, beresiko kehilangan pelanggan bernilai tinggi tanpa upaya retensi. Sehingga perusahaan akan melakukan Customer Acquisition

    - **Estimasi kerugian:** $500 per pelanggan.

**Scoring**

- Berdasarkan kerugiannya False Negative (FN) lebih berisiko, serta dapat beresiko mengakibatkan hilangnya pelanggan loyal yang sebenarnya masih bisa dipertahankan. Namun, False Positive (FP) juga perlu dikendalikan, mengingat Customer Retention yang sebenarnya tidak perlu bisa cukup besar, dan waktu yang digunakan untuk upaya tersebut seharusnya dapat dialokasikan untuk aktivitas bisnis lainnya yang lebih efektif.

- Oleh karena itu, metrik evaluasi yang akan digunakan adalah **F2-score**, yang memberikan bobot lebih besar pada recall (mengurangi FN), tetapi tetap mempertimbangkan precision untuk mengontrol FP.

# **Columns Descriptions**

| Kolom                   | Deskripsi                                                                 |
|------------------------|---------------------------------------------------------------------------|
| Tenure                 | Lama waktu (dalam bulan) pelanggan telah terdaftar.                       |
| WarehouseToHome        | Jarak (kemungkinan dalam km) antara gudang dan rumah pelanggan.          |
| NumberOfDeviceRegistered | Jumlah perangkat yang terdaftar di akun pelanggan.                     |
| PreferedOrderCat       | Kategori produk yang paling sering dipesan pelanggan.                    |
| SatisfactionScore      | Skor kepuasan pelanggan (kemungkinan skala 1–5).                         |
| MaritalStatus          | Status pernikahan pelanggan (Single, Married, Divorced).                |
| NumberOfAddress        | Jumlah alamat yang terdaftar di akun pelanggan.                         |
| Complain               | Apakah pelanggan pernah komplain (0 = tidak, 1 = ya).                    |
| DaySinceLastOrder      | Jumlah hari sejak pemesanan terakhir oleh pelanggan.                    |
| CashbackAmount         | Jumlah cashback yang diterima oleh pelanggan.                           |
| Churn                  | Apakah pelanggan berhenti menggunakan layanan (0 = tidak, 1 = ya).       |

# **Tools & Library**

 **📊 Data Manipulation & Visualization**
| Fungsi                | Library                        |
| --------------------- | ------------------------------ |
| Manipulasi data       | `pandas`, `numpy`              |
| Visualisasi data      | `seaborn`, `matplotlib.pyplot` |
| Uji distribusi normal | `scipy.stats.normaltest`       |

**⚙️ Preprocessing**
| Fungsi                | Library                                                                     |
| --------------------- | --------------------------------------------------------------------------- |
| Split data            | `train_test_split` dari `sklearn.model_selection`                           |
| Imputasi nilai hilang | `IterativeImputer`,                           |
| Scaling fitur         | `RobustScaler`                            |
| Encoding kategorikal  | `OneHotEncoder` |
| Transformasi kolom    | `ColumnTransformer`                                                         |
| Pipeline              | `Pipeline`                                                                  |

**🧪 Modeling & Evaluasi**
| Fungsi                      | Library                                                                                                           |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| Model Klasifikasi           | `DecisionTreeClassifier`, `RandomForestClassifier`                  |
| Ensemble Learning           |  `AdaBoostClassifier`, `GradientBoostingClassifier` |
| Model Lanjutan              | `XGBClassifier` (XGBoost), `LGBMClassifier` (LightGBM)                                                            |
| Validasi & Cross-Validation | `cross_val_score`, `cross_validate`                                                   |
| Hyperparameter Tuning       | `GridSearchCV`, `RandomizedSearchCV`                                                                              |
| Evaluasi Model              | `fbeta_score`, `make_scorer`, `confusion_matrix`, `classification_report`, `ConfusionMatrixDisplay`               |

**🚀 Deployment**
| Fungsi                 | Library  |
| ---------------------- | -------- |
| Menyimpan/Memuat model | `pickle` |

**🔕 Miscellaneous**
| Fungsi                | Library                             |
| --------------------- | ----------------------------------- |
| Menonaktifkan warning | `warnings.filterwarnings('ignore')` |


# **Best Model : XGBoostClalssifer**

**Best Parameter**
| **Parameter**       | **Nilai**   | 
| ------------------- | ----------- | 
| `random_state`      | `0`         | 
| `use_label_encoder` | `False`     | 
| `eval_metric`       | `'logloss'` | 
| `learning_rate`     | `0.1`       | 
| `booster`           | `'gbtree'`  | 
| `max_depth`         | `3`         | 
| `n_estimators`      | `150`       | 
| `scale_pos_weight`  | `4`         | 

# **Kesimpulan**

Berdasarkan hasil metric evaluation Machine Learning berhasil memperoleh F2 Score sebesar 80% untuk memprediksi pelanggan Churn atau Not Churn dari dari data test. F2 Score adalah metrik evaluasi yang mengutamakan recall lebih tinggi daripada precision (karena recall diberi bobot lebih besar). Data test sendiri terdiri dari 653 data dimana secara metric Machine Learning salah memprediksi 16 Data FN, Serta 48 Data FN. 

Apa bila dihitung berikut merupakan ilustrasi kerugian perusahaan sebelum dan sesudah menggunakan Machine Learning

**Tanpa menggunakan ML**

Sebelum menggunakan ML, perusahaan tidak mengetahui siapa saja customer yg akan churn, sehingga melakukan tindakan custumer retention untuk mencegah kehilangan pelanggan.

Kita asumsikan ketika seorang customer diberikan promosi, maka customer tersebut tidak akan churn.

|                 | Predicted (0) | Predicted (1) |
| :-------------: | :-----------: | :-----------: |
| Actual (0)      | 0             | 546          |
| Actual (1)      | 0             | 107           |

- Pengeluaran perusahaan untuk promosi (TP+FP+TN+FN): $100 x 653 = $65,300
- Promosi yang tepat sasaran pada orang yang churn (TP+FN): $100 x 107 = $10,700
- Artinya perusahaan mengeluarkan biaya promosi sia-sia pada customer loyal: $65,300 - $10,700 = $54,600

**Dengan menggunakan ML**

Setelah menggunakan ML, perusahaan jadi bisa memprediksi siapa saja customer yg akan churn, sehingga bisa mengeluarkan cost untuk promosi lebih tepat sasaran.

|                 | Predicted (0) | Predicted (1) |
| :-------------: | :-----------: | :-----------: |
| Actual (0)      | 500         | 48           |
| Actual (1)      | 16          | 91           |

- Pengeluaran perusahaan untuk salah promosi ke customer loyal (FP): $100 x 46 = $4,800
- Perusahaan kehilangan customer karena tidak terprediksi akan churn (FN): $500 x 16 = $8000
- Artinya perusahaan mengalami kerugian: $4,800 + $8000 = $12,800

**Kerugian menurun setelah pakai ML**

- Kerugian sebelum pakai ML: $54,600
- Kerugian setelah pakai ML:  $12,800
- ML berhasil menurunkan kerugian perusahaan sebesar 75% --> ($54,600 - $12,600) / $159,300

# **Rekomendasi**
- Terdapat beberapa missing value pada kolom `Tenure`, `WarehouseToHome` dan `DaySinceLastOrder` yang kemudian diisi dengan imputer, untuk kedepannya alangkah lebih baik data yang diambil dengan lebih teliti lagi agar tidak terdapat missing value serta membuat prediksi akan lebih akurat dengan data real
- Terdapat feature-feature yang kurang berpengaruh signifikan terhadap hasil prediksi Machine Learning, dengan demikian dapat ditambahkan feature-feature yang lain mungkin seperti jumlah order barang, umur, dll
- Menambahkan parameter yang lebih banyak pada bagian Hyperparameter Tunning, kebetulan perangkat yang saat ini saya gunakan akan memakan waktu yang sangat lama untuk menambah parameter lain. jadi alangkah baiknya dapat mencoba berbagai parameter lain.

# **Streamlit**
https://appchurnpredict-euhmtmpjtuvnita8ip77hv.streamlit.app


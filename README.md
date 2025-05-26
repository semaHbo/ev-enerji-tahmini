# 📊 Akıllı Ortam Koşullarına Göre Ev Enerji Tüketimi Tahmin Sistemi

Bu proje, bir evin farklı saatlerdeki enerji tüketimini çeşitli iç/dış ortam faktörlerine göre tahmin etmeyi amaçlamaktadır. Çalışma, **UCI Appliances Energy Prediction Dataset** verisi üzerine inşa edilmiştir ve veri mühendisliği, makine öğrenimi ve değerlendirme süreçlerini kapsamlı biçimde içerir.

---

## Proje Klasör Yapısı

.
├── Feature_Engineering/
│ ├── Feature_Engineering1.ipynb
│ ├── Feature_Engineering2.ipynb
│ └── Feature_EngineeringPlus.ipynb
├── Model_Training/
│ ├── Linear_Regression.ipynb
│ ├── Linear_Regression_FeaturePlus.ipynb
│ ├── Ridge_Regression.ipynb
│ ├── Ridge_Regression_FeaturePlus.ipynb
│ ├── Random_Forest.ipynb
│ ├── Random_Forest_FeaturePlus.ipynb
│ ├── SVR.ipynb
│ ├── SVR_FeaturePlus.ipynb
│ └── README.md
├── EDA_Enerji_Tuketimi.ipynb
└── README.md

---


## 🎯 Proje Amacı

Evlerdeki **enerji tüketiminin**; saat, gün, mevsim, sıcaklık, nem, basınç, rüzgar gibi ortam koşullarına bağlı olarak tahmin edilebilmesi. Amaç, çeşitli regresyon modelleri ve öznitelik mühendisliği teknikleriyle bu tahmini mümkün olduğunca doğru hale getirmektir.

---

## 🧪 1. EDA (Exploratory Data Analysis)

- Eksik veri analizi
- Zaman temelli dağılım grafikleri
- Korelasyon matrisi
- Outlier kontrolü (özellikle hedef değişkende)
- Hedef değişkenin dağılımı gözlendi ve aşırı sağa çarpık (right-skewed) olduğu tespit edildi.

🧩 `EDA_Enerji_Tuketimi.ipynb` dosyasında detaylar mevcuttur.

---

## 🧱 2. Feature Engineering

Yaratılan bazı yeni öznitelikler:

- `Sicaklik_IcOrtalama`, `Nem_IcOrtalama`: Sensörlerden alınan verilerin ortalaması
- `Sicaklik_Farki`, `Nem_Farki`: İç - dış farklar
- `is_weekend`, `IsHaftaSonuVeSaat`: Zaman temelli özellikler
- Kategorik sütunlar (`Mevsim`, `HaftaGunu_Adi`, `Ay_Adi`) için One-Hot Encoding uygulandı.
- `Feature_EngineeringPlus.ipynb` dosyasında en güncel ve gelişmiş veri seti oluşturuldu.
- 🔄 Veriler tarih sırasına göre ayrıldı:
  - `Train`: İlk 70%
  - `Validation`: Orta 15%
  - `Test`: Son 15%

---

### 🔬 Kullanılan Metrikler:
- MAE (Ortalama mutlak hata)
- MSE / RMSE
- R² (Açıklanan varyans oranı)

---

## 🤖 3. Modelleme Süreci

| Model                              | Veri Seti                | MAE   | RMSE   | R²      |
|-----------------------------------|--------------------------|--------|--------|----------|
| Linear Regression                 | train_scaled             | 53.52  | 94.78  | 0.1081   |
| Ridge Regression                  | train_scaled             | 53.47  | 95.08  | 0.1023   |
| Random Forest                     | train_scaled             | 58.32  | 98.05  | 0.0454   |
| Random Forest (Log Target)        | train_scaled             | 45.27  | 96.31  | 0.0790   |
| Random Forest (FeaturePlus)       | train_plus_scaled        | 58.32  | 98.05  | 0.0454   |
| Random Forest (Tuned + Log)       | train_plus_scaled        | 45.46  | 98.73  | 0.0322   |
| SVR                               | train_scaled             | 44.49  | 98.61  | 0.0345   |
| SVR (GridSearch)                  | train_scaled             | 43.65  | 100.47 | -0.0023  |
| SVR (FeaturePlus - TimeSplit)     | train_plus_scaled        | 39.54  | 93.46  | -0.0267  |

📊 En iyi **MAE değeri**: `SVR (FeaturePlus, zaman temelli bölme)` modeli ile `39.54`

📉 En düşük **RMSE**: `SVR FeaturePlus` modeli ile `93.46`


🔧 Tuning işlemleri:
- `RandomForest`: GridSearchCV ile `max_depth`, `n_estimators`, `min_samples_split`, `min_samples_leaf`
- `SVR`: GridSearchCV ile `C`, `epsilon`, `kernel`

---

## 📈 4. Değerlendirme Metotları

- MAE (Mean Absolute Error)
- MSE (Mean Squared Error)
- RMSE (Root Mean Squared Error)
- R² (Determination Coefficient)
- Gerçek vs Tahmin görselleştirmeleri (line plot)
- Özellik önemleri (Random Forest feature importance grafiği)

---

## 🧠 5. Sonuçlar ve Yorum

- `SVR (FeaturePlus)` modeli zaman bazlı bölme ile en düşük **MAE**’yi vermiştir: `39.54`
- `R²` skorları genel olarak düşüktür. Bu durum:
  - Verideki varyansın çok yüksek olması
  - Hedef değişkenin outlier’lara sahip olması
  - Enerji tüketiminin ani sıçramalar içermesiyle açıklanabilir.
- Model tuning ve feature engineering işlemleri performans iyileştirmiştir ancak sınırlıdır.
- En önemli değişkenler: `hour`, `Basinc`, `Sicaklik_IcOrtalama`, `Aydinlatma_Tuketimi`

---

## 📌 6. Öneriler

- Daha güçlü modeller (XGBoost, LightGBM, LSTM)
- Dönüşümler (Box-Cox, log1p, clipping)
- Farklı hata fonksiyonları (Huber, Quantile Loss)
- Daha dengeli target dağılımı için SMOGN, Oversampling gibi yöntemler
- Neural Network mimarisiyle test (özellikle LSTM/CNN)

---

## 🛠️ Kullanılan Teknolojiler

- Python
- Pandas, NumPy
- Scikit-learn
- Matplotlib, Seaborn
- Google Colab + GitHub versiyon kontrolü

---

## 👨‍💻 Geliştirici

📌 GitHub: [semaHbo](https://github.com/semaHbo)  
🧠 Proje kapsamında mentor yönlendirmeleriyle her adım kayıt altına alınmış ve sistematik olarak uygulanmıştır.

---

## 🗂️ Kaynaklar

- UCI Machine Learning Repository
- scikit-learn documentation
- Veri bilimi toplulukları ve makaleler

---

> Proje, bilimsel yaklaşımı ve sistematik uygulama adımları ile ileri düzeyde modelleme pratiği kazandırmayı hedeflemiştir.

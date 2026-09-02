# 🏠 Konut Fiyat Tahmini — Ridge, Lasso & ElasticNet

Bu projede, konut özelliklerinden yararlanarak **konut fiyatlarının tahmin edilmesi** amaçlanmıştır.

Çalışmada klasik doğrusal regresyona alternatif olarak **Ridge Regression, Lasso Regression ve ElasticNet Regression** modelleri uygulanmış ve performansları karşılaştırılmıştır.

## 📌 Proje Hakkında

Veri setinde toplam **545 konut** ve **13 değişken** bulunmaktadır. Hedef değişken `price` olup, diğer konut özellikleri kullanılarak fiyat tahmini gerçekleştirilmiştir. Veri setinde eksik değer bulunmamaktadır. 
### Kullanılan değişkenler

**Sayısal değişkenler:**
- `area` — Ev alanı
- `bedrooms` — Yatak odası sayısı
- `bathrooms` — Banyo sayısı
- `stories` — Kat sayısı
- `parking` — Otopark sayısı

**Kategorik değişkenler:**
- `mainroad`
- `guestroom`
- `basement`
- `hotwaterheating`
- `airconditioning`
- `prefarea`
- `furnishingstatus`

**Hedef değişken:**
- `price` — Konut fiyatı

## 🛠️ Kullanılan Teknolojiler

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Jupyter Notebook / Google Colab

## 🔎 Uygulanan Yöntem

Proje aşağıdaki adımlardan oluşmaktadır:

1. Veri setinin yüklenmesi
2. Veri setinin genel yapısının incelenmesi
3. Eksik değer kontrolü
4. Sayısal ve kategorik değişkenlerin ayrılması
5. Eğitim ve test verilerinin oluşturulması
6. Sayısal değişkenlerin `StandardScaler` ile ölçeklendirilmesi
7. Kategorik değişkenlerin `OneHotEncoder` ile dönüştürülmesi
8. `ColumnTransformer` ve `Pipeline` kullanılarak modelleme
9. Ridge, Lasso ve ElasticNet modellerinin eğitilmesi
10. Modellerin MSE, MAE ve R² metrikleriyle karşılaştırılması

Veri, `%80` eğitim ve `%20` test olacak şekilde ayrılmıştır; sonuçta 436 eğitim ve 109 test gözlemi kullanılmıştır.

## 🤖 Kullanılan Modeller

### Ridge Regression

Ridge Regression, model katsayılarını küçültmek amacıyla **L2 regularizasyonu** kullanır.

Projede ayrıca farklı `alpha` değerlerini değerlendirmek amacıyla `RidgeCV` ve 5-fold cross-validation kullanılmıştır.

### Lasso Regression

Lasso Regression, **L1 regularizasyonu** kullanarak bazı katsayıları sıfıra çekebilir. Bu özelliği sayesinde değişken seçimi açısından da kullanılabilir.

Projede `LassoCV` ile farklı `alpha` değerleri 5-fold cross-validation kullanılarak değerlendirilmiştir.

### ElasticNet Regression

ElasticNet, **L1 ve L2 regularizasyonlarının birleşimini** kullanır.

Projede:

```text
alpha = 1
l1_ratio = 0.5
max_iter = 1000
```

parametreleriyle uygulanmıştır.

## 📊 Model Sonuçları

Modeller test verisi üzerinde **MSE, MAE ve R²** metrikleri kullanılarak karşılaştırılmıştır.

| Model | MSE | MAE | R² |
|---|---:|---:|---:|
| 🥇 Lasso | 1.754 × 10¹² | 970,044 | **0.6529** |
| 🥈 Ridge | 1.792 × 10¹² | 979,664 | **0.6455** |
| 🥉 ElasticNet | 2.274 × 10¹² | 1,110,583 | **0.5500** |

Sonuçlara göre bu çalışmadaki test setinde **Lasso Regression en yüksek R² değerini** elde etmiştir. Aynı zamanda en düşük MSE ve MAE değerlerine de sahiptir.

## 📈 Değerlendirme Metrikleri

### R² — Determination Coefficient
Modelin hedef değişkendeki varyansı ne ölçüde açıkladığını gösterir. Değer 1'e yaklaştıkça modelin açıklama gücü artar.

### MAE — Mean Absolute Error
Tahminlerin gerçek değerlerden ortalama mutlak sapmasını gösterir. Daha düşük olması daha iyidir.

### MSE — Mean Squared Error
Tahmin hatalarının karesinin ortalamasıdır. Büyük hataları daha fazla cezalandırır.

## 📊 Veri Analizi

Projede sayısal değişkenler arasındaki ilişkileri incelemek için **korelasyon matrisi** ve ısı haritası kullanılmıştır.

## 📁 Proje Yapısı

```text
├── ridge_lasso_elesticnet_regresyon.ipynb
├── Housing.csv
└── README.md
```

## 🎯 Projenin Amacı

Bu çalışmanın temel amacı, regularizasyon yöntemlerinin konut fiyat tahminindeki performansını incelemek ve **Ridge, Lasso ve ElasticNet modellerinin aynı veri seti üzerindeki sonuçlarını karşılaştırmaktır.**

Çalışma sonucunda kullanılan veri ve test bölünmesi kapsamında **Lasso Regression en başarılı model** olarak öne çıkmıştır.

---

## 👩‍💻 Geliştirici

**Sakine Cansu TOPCİ**

Python • Machine Learning • Data Analysis

> Bu proje, makine öğrenmesi ve regresyon algoritmaları üzerine yapılan uygulamalı bir çalışma kapsamında hazırlanmıştır.
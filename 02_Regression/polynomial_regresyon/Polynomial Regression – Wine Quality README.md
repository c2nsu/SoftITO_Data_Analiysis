# 🍷 Polynomial Regression — Wine Quality Prediction

Kırmızı şarapların fizikokimyasal özelliklerinden **kalite skorunu tahmin etmek** amacıyla geliştirilen, polinom regresyon yöntemini adım adım inceleyen bir Machine Learning projesidir.

Bu projede özellikle **alkol oranı (`alcohol`) ile şarap kalitesi (`quality`) arasındaki ilişki** incelenmiş ve farklı polinom derecelerine sahip modeller karşılaştırılmıştır.

---

## 🎯 Projenin Amacı

Lineer regresyon, değişkenler arasındaki ilişkiyi düz bir doğru ile açıklamaya çalışır. Ancak gerçek dünyadaki ilişkiler her zaman doğrusal değildir.

Bu projede şu soruya cevap aranmıştır:

> **Şarapların alkol oranı ile kalite skoru arasındaki ilişki doğrusal mıdır, yoksa polinom regresyon daha iyi bir sonuç verebilir mi?**

Bunun için farklı polinom dereceleri test edilmiş ve modeller;

- Train R²
- Test R²
- Train RMSE
- Test RMSE
- Overfitting riski
- Cross-Validation

üzerinden değerlendirilmiştir.

---

## 📊 Kullanılan Veri Seti

Projede **Wine Quality – Red Wine** veri seti kullanılmıştır.

| Özellik | Değer |
|---|---:|
| Gözlem sayısı | 1.599 |
| Sütun sayısı | 12 |
| Hedef değişken | `quality` |
| Kalite aralığı | 3 – 8 |
| Eksik değer | 0 |

Veri setinde kırmızı şarapların fizikokimyasal özellikleri ile kalite skorları bulunmaktadır.

### Kullanılan temel değişkenler

- `alcohol` — Alkol oranı
- `volatile acidity` — Uçucu asitlik
- `citric acid` — Sitrik asit
- `sulphates` — Sülfatlar
- `quality` — Şarap kalite skoru

Notebook'taki ana modelleme deneyinde **`alcohol → quality`** ilişkisi üzerinden polinom regresyon uygulanmıştır.

---

## 🧠 Kullanılan Yöntem

### Polynomial Regression

Polinom regresyon, bağımsız değişken ile hedef değişken arasındaki doğrusal olmayan ilişkileri modellemek için kullanılır.

Örneğin:

```text
Derece 1 → x
Derece 2 → x + x²
Derece 3 → x + x² + x³
```

Projede `PolynomialFeatures` kullanılarak özellikler polinom derecelerine dönüştürülmüş ve ardından `LinearRegression` ile model oluşturulmuştur.

Kullanılan pipeline yapısı:

```text
PolynomialFeatures
        ↓
LinearRegression
        ↓
Prediction
```

---

## 🔬 Denenen Polinom Dereceleri

Model aşağıdaki dereceler için test edilmiştir:

```text
1
2
3
4
5
7
10
```

Her model için eğitim ve test performansı ayrı ayrı hesaplanmış, Train R² ile Test R² arasındaki fark kullanılarak overfitting riski de incelenmiştir.

---

## 📈 Model Sonuçları

Elde edilen sonuçlardan bazıları:

| Polinom Derecesi | Train R² | Test R² | Overfitting |
|---:|---:|---:|---|
| 1 | 0.2234 | 0.2356 | ✅ |
| 2 | 0.2257 | 0.2316 | ✅ |
| **3** | **0.2318** | **0.2386** | **✅ En iyi** |
| 4 | 0.2341 | 0.2187 | ⚠️ |
| 5 | 0.2346 | 0.2087 | ⚠️ |
| 7 | 0.2347 | 0.2092 | ⚠️ |
| 10 | 0.2347 | 0.2128 | ⚠️ |

Derece yükseldikçe eğitim performansı bir miktar artmasına rağmen test performansının düşmeye başlaması, yüksek derecelerde **overfitting riskinin arttığını** göstermektedir.

---

## 🏆 En İyi Model

Deneyler sonucunda en başarılı model:

### Polynomial Regression — Degree 3

**Test R²:**

```text
0.2386
```

**Test RMSE:**

```text
0.7054
```

Yani notebook'taki karşılaştırmaya göre **3. derece polinom**, test setinde en yüksek R² değerini vermiştir.

---

## 🔁 Cross-Validation

Model seçiminin yalnızca tek bir train/test bölünmesine bağlı kalmaması için **K-Fold Cross-Validation** da uygulanmıştır.

Cross-validation sonuçlarında da:

> 🏆 **En iyi polinom derecesi: 3**

olarak bulunmuştur.

Ayrıca yüksek derecelerde cross-validation standart sapmasının yükselmesi, modellerin farklı veri bölümlerinde daha dengesiz sonuçlar verdiğini göstermektedir. Özellikle derece 7 ve 10 modellerinde bu durum daha belirgindir.

---

## 📊 Projede Yapılan Analizler

Notebook yalnızca model oluşturmakla sınırlı değildir. Baştan sona bir Machine Learning çalışma akışı oluşturulmuştur.

### 1. Veri yükleme

`winequality-red.csv` dosyası Pandas ile okunmuştur.

### 2. Veri inceleme

- Veri boyutu
- Veri tipleri
- İlk gözlemler
- Kalite dağılımı
- İstatistiksel özet

incelenmiştir.

### 3. Eksik veri analizi

Veri setinde **eksik değer bulunmadığı** görülmüştür.

### 4. Aykırı değer analizi

IQR yöntemi kullanılarak;

- `alcohol`
- `volatile acidity`
- `citric acid`
- `sulphates`

değişkenleri incelenmiştir.

### 5. Exploratory Data Analysis

Değişkenlerin dağılımları ve değişkenler arasındaki ilişkiler görselleştirilmiştir.

### 6. Polynomial Feature Engineering

Farklı polinom dereceleri oluşturulmuştur.

### 7. Model eğitimi

`LinearRegression` ile farklı derecelerde modeller eğitilmiştir.

### 8. Model karşılaştırması

R², MSE ve RMSE metrikleri kullanılmıştır.

### 9. Overfitting analizi

Train R² ve Test R² arasındaki fark incelenmiştir.

### 10. Cross-Validation

Modellerin farklı veri bölümlerindeki kararlılığı değerlendirilmiştir.

---

## 🛠️ Kullanılan Teknolojiler

```text
Python
│
├── NumPy
├── Pandas
├── Matplotlib
├── Seaborn
│
└── Scikit-learn
    ├── PolynomialFeatures
    ├── LinearRegression
    ├── Pipeline
    ├── train_test_split
    ├── KFold
    ├── cross_val_score
    ├── R²
    └── Mean Squared Error
```

---

## 📁 Proje Yapısı

```text
polynomial-regression/
│
├── polynomial_regression.ipynb
├── winequality-red.csv
└── README.md
```

> `winequality-red.csv` dosyasının notebook ile aynı klasörde bulunması gerekir.

---

## 🚀 Çalıştırma

Projeyi çalıştırmak için:

### 1. Repoyu klonla

```bash
git clone https://github.com/USERNAME/polynomial-regression.git
```

### 2. Proje klasörüne gir

```bash
cd polynomial-regression
```

### 3. Gerekli kütüphaneleri yükle

```bash
pip install numpy pandas matplotlib seaborn scikit-learn jupyter
```

### 4. Notebook'u çalıştır

```bash
jupyter notebook polynomial_regression.ipynb
```

veya VS Code / JupyterLab üzerinden notebook'u açabilirsiniz.

---

## 💡 Projeden Çıkarılan Sonuç

Bu çalışmanın önemli sonuçlarından biri, **daha yüksek polinom derecesinin otomatik olarak daha iyi model anlamına gelmemesidir.**

Derece yükseldikçe model eğitim verisine daha fazla uyum sağlayabilir. Ancak test performansı düşebilir ve modelin genelleme yeteneği zayıflayabilir.

Bu projede:

```text
Derece 1  → Test R² = 0.2356
Derece 3  → Test R² = 0.2386  🏆
Derece 10 → Test R² = 0.2128
```

sonuçları bu durumu açıkça göstermektedir.

Dolayısıyla model seçiminde yalnızca eğitim performansına değil, **test performansına ve cross-validation sonuçlarına da bakmak gerekir.**

---

## 📌 Key Takeaways

- Polynomial Regression, doğrusal olmayan ilişkileri modellemek için kullanılabilir.
- Polinom derecesinin artırılması her zaman performansı artırmaz.
- Yüksek dereceler overfitting riskini artırabilir.
- Train ve Test performansının birlikte değerlendirilmesi gerekir.
- Cross-validation model seçiminde daha güvenilir bir değerlendirme sağlar.
- Bu çalışmada **3. derece polinom model en başarılı model** olarak belirlenmiştir.

---

## 👩‍💻 Author

**Sakine Cansu TOPCİ**

Machine Learning & Data Science çalışmalarım kapsamında geliştirilmiştir.

---

⭐ Eğer proje faydalı olduysa repository'ye yıldız bırakmayı unutmayın!
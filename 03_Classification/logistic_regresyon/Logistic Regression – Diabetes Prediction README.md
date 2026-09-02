# 🩺 Logistic Regression — Diabetes Prediction

Bu proje, bireylerin sağlık ve yaşam tarzı özelliklerini kullanarak **diyabet durumunu sınıflandırmak** amacıyla geliştirilen bir **Lojistik Regresyon (Logistic Regression)** çalışmasıdır.

Çalışmada gerçek dünya sağlık verileri kullanılarak veri setinin incelenmesi, özelliklerin standardizasyonu, lojistik regresyon modelinin eğitilmesi ve model katsayılarının yorumlanması gerçekleştirilmiştir.

---

## 🎯 Projenin Amacı

Bu çalışmanın temel amacı:

> **Bireylerin sağlık göstergeleri ve yaşam tarzı özelliklerinden yararlanarak diyabet sınıfını tahmin etmek.**

Model, bireyin;

- tansiyon durumu,
- kolesterol durumu,
- BMI,
- sigara kullanımı,
- fiziksel aktivitesi,
- genel sağlık durumu,
- yaş,
- eğitim,
- gelir

gibi özelliklerini kullanarak `Diabetes_012` hedef değişkenini tahmin etmektedir.

---

## 📊 Veri Seti

Projede **BRFSS 2015 Health Indicators** veri seti kullanılmıştır.

Veri seti:

| Özellik | Değer |
|---|---:|
| Gözlem sayısı | **253.680** |
| Toplam sütun | **22** |
| Bağımsız değişken | **21** |
| Hedef değişken | `Diabetes_012` |
| Eksik değer | **0** |
| Sınıf sayısı | **3** |

Notebook'taki veri kontrolünde 253.680 satır ve 22 sütun bulunduğu, ayrıca veri setinde eksik değer olmadığı görülmektedir.

---

## 🏷️ Hedef Değişken

Modelin tahmin etmeye çalıştığı değişken:

```text
Diabetes_012
```

Sınıflar:

```text
0 → Diyabet yok
1 → Prediyabet
2 → Diyabet
```

Veri setindeki sınıf dağılımı:

| Sınıf | Gözlem |
|---:|---:|
| 0.0 | 213.703 |
| 1.0 | 4.631 |
| 2.0 | 35.346 |

Bu dağılım, özellikle **1. sınıfın oldukça az temsil edildiğini** göstermektedir.

---

## 🧬 Kullanılan Özellikler

Modelde toplam **21 bağımsız değişken** kullanılmıştır:

```text
HighBP
HighChol
CholCheck
BMI
Smoker
Stroke
HeartDiseaseorAttack
PhysActivity
Fruits
Veggies
HvyAlcoholConsump
AnyHealthcare
NoDocbcCost
GenHlth
MentHlth
PhysHlth
DiffWalk
Sex
Age
Education
Income
```

Bu değişkenler bireylerin sağlık durumu, davranışları, yaşam tarzı ve sosyoekonomik özelliklerini temsil etmektedir.

---

## 🔄 Proje Akışı

```text
Veri Seti
    ↓
Veri Keşfi
    ↓
Eksik Veri Kontrolü
    ↓
X / y Ayrımı
    ↓
Train / Test Split
    ↓
StandardScaler
    ↓
Logistic Regression
    ↓
Tahmin
    ↓
Model Değerlendirme
    ↓
Katsayı Analizi
```

---

## 🧹 Veri Ön İşleme

Öncelikle hedef değişken bağımsız değişkenlerden ayrılmıştır:

```python
X = df.drop('Diabetes_012', axis=1)
y = df['Diabetes_012']
```

Ardından veri:

- %80 eğitim
- %20 test

olarak ayrılmıştır.

Ayrıştırma sırasında `stratify=y` kullanılarak sınıf dağılımının eğitim ve test setlerinde korunması sağlanmıştır.

### Veri Bölünmesi

```text
Eğitim seti → 202.944 örnek
Test seti   → 50.736 örnek
```

---

## ⚖️ Standardizasyon

Lojistik regresyon öncesinde özellikler `StandardScaler` ile standardize edilmiştir.

```python
scaler = StandardScaler()

X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

Böylece özellikler yaklaşık olarak:

```text
Mean = 0
Std  = 1
```

ölçeğine getirilmiştir.

---

## 🤖 Kullanılan Model

Projede `scikit-learn` içerisindeki **LogisticRegression** modeli kullanılmıştır.

Model ayarları:

```python
LogisticRegression(
    penalty='l2',
    C=1.0,
    solver='lbfgs',
    max_iter=1000,
    random_state=42
)
```

### Kullanılan parametreler

| Parametre | Değer |
|---|---|
| `penalty` | L2 |
| `C` | 1.0 |
| `solver` | lbfgs |
| `max_iter` | 1000 |
| `random_state` | 42 |

Model eğitim seti üzerinde standardize edilmiş verilerle eğitilmiştir.

---

## 📈 Model Katsayıları

Modelin yorumlanabilirliğini artırmak amacıyla lojistik regresyon katsayıları da incelenmiştir.

En yüksek mutlak katsayıya sahip özellikler:

| Özellik | Katsayı |
|---|---:|
| `GenHlth` | -0.309657 |
| `Age` | -0.253105 |
| `BMI` | -0.252392 |
| `HighChol` | -0.193894 |
| `HighBP` | -0.187704 |
| `CholCheck` | -0.129912 |
| `Income` | +0.084474 |
| `HvyAlcoholConsump` | +0.072863 |

Notebook'taki katsayı analizinde `GenHlth`, `Age` ve `BMI` değişkenlerinin mutlak katsayı açısından öne çıktığı görülmektedir.

> **Not:** Katsayıların işareti modelin tahmin ettiği sınıf olasılıklarıyla ilişkili yönü gösterir; tek başına nedensellik anlamına gelmez.

---

## 📊 Görselleştirme

Projede lojistik regresyon katsayılarının daha kolay yorumlanabilmesi için yatay bar grafik oluşturulmuştur.

Grafikte:

- Pozitif katsayılar
- Negatif katsayılar
- Özelliklerin katsayı büyüklükleri

karşılaştırılmaktadır.

---

## 📏 Kullanılan Model Değerlendirme Metrikleri

Notebook'ta model değerlendirmesi için aşağıdaki metrikler kullanılmaktadır:

- **Accuracy**
- **Precision**
- **Recall**
- **F1-Score**
- **Confusion Matrix**
- **Classification Report**
- **ROC Curve**
- **ROC-AUC**

Bu metrikler özellikle sınıflandırma problemlerinde modelin farklı açılardan değerlendirilmesini sağlar.

---

## ⚠️ Sınıf Dengesizliği

Veri setindeki sınıflar eşit dağılmamıştır:

```text
Sınıf 0 → 213.703
Sınıf 1 →   4.631
Sınıf 2 →  35.346
```

Özellikle **Prediyabet (1.0)** sınıfının diğer sınıflara göre oldukça az olması önemli bir noktadır.

Bu nedenle yalnızca accuracy değerine bakmak yerine:

```text
Precision
Recall
F1-Score
Confusion Matrix
ROC-AUC
```

gibi metriklerin birlikte değerlendirilmesi önemlidir.

---

## 🛠️ Kullanılan Teknolojiler

```text
Python
│
├── NumPy
├── Pandas
├── Matplotlib
│
└── Scikit-learn
    ├── train_test_split
    ├── StandardScaler
    ├── LogisticRegression
    ├── accuracy_score
    ├── precision_score
    ├── recall_score
    ├── f1_score
    ├── confusion_matrix
    ├── classification_report
    ├── roc_curve
    └── roc_auc_score
```

---

## 📁 Proje Yapısı

```text
logistic-regression-diabetes/
│
├── lojistik_regresyon_calismasi.ipynb
├── diabetes_012_health_indicators_BRFSS2015.csv
└── README.md
```

> Notebook'un çalışabilmesi için veri setinin notebook içerisinde kullanılan dosya adıyla aynı klasörde bulunması gerekir.

---

## 🚀 Çalıştırma

### 1. Repository'yi klonla

```bash
git clone https://github.com/USERNAME/logistic-regression-diabetes.git
```

### 2. Proje klasörüne gir

```bash
cd logistic-regression-diabetes
```

### 3. Gerekli kütüphaneleri yükle

```bash
pip install numpy pandas matplotlib scikit-learn jupyter
```

### 4. Notebook'u çalıştır

```bash
jupyter notebook lojistik_regresyon_calismasi.ipynb
```

Alternatif olarak notebook **Google Colab** veya **VS Code** üzerinden de çalıştırılabilir.

---

## 💡 Projeden Çıkarılan Sonuçlar

Bu çalışma ile:

- Lojistik regresyonun çok sınıflı sınıflandırma probleminde kullanımı,
- Veri setinin keşfedilmesi,
- Eksik veri kontrolü,
- Eğitim/test ayrımı,
- Standardizasyon,
- Model eğitimi,
- Katsayıların yorumlanması,
- Sınıflandırma metrikleri,
- ROC-AUC ve confusion matrix kullanımı

uygulamalı olarak incelenmiştir.

Özellikle model katsayılarının incelenmesi, lojistik regresyonun yalnızca tahmin yapan değil, aynı zamanda **özelliklerin model üzerindeki etkisini yorumlamaya yardımcı olan** bir yöntem olduğunu göstermektedir.

---

## 📌 Key Takeaways

> **İyi bir sınıflandırma modeli yalnızca yüksek accuracy üretmek değildir.**

Özellikle sınıf dağılımının dengesiz olduğu veri setlerinde precision, recall, F1-score, confusion matrix ve ROC-AUC gibi metriklerin birlikte değerlendirilmesi gerekir.

Bu projede ayrıca standardizasyonun ve model katsayılarının incelenmesiyle lojistik regresyonun hem **tahmin** hem de **yorumlanabilirlik** açısından nasıl kullanılabileceği gösterilmiştir.

---

## 👩‍💻 Author

**Sakine Cansu TOPCİ**

Machine Learning & Data Science çalışmalarım kapsamında geliştirilmiştir.

---

⭐ Projeyi faydalı bulduysanız repository'ye yıldız bırakabilirsiniz.
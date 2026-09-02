<div align="center">

# 🚢 Titanic Survival Classification
## AdaBoost vs XGBoost

**Titanic verisi üzerinde iki güçlü Ensemble Learning algoritmasının performans karşılaştırması**

`Python` · `Scikit-learn` · `XGBoost` · `AdaBoost` · `Machine Learning`

</div>

---

## 📌 Proje Hakkında

Bu projede **AdaBoost** ve **XGBoost** algoritmalarının aynı sınıflandırma problemi üzerindeki performansları karşılaştırılmıştır.

Çalışmanın temel amacı yalnızca hangi modelin daha yüksek doğruluk verdiğini görmek değil;

- Accuracy
- AUC-ROC
- Precision
- Recall
- F1-Score
- Cross Validation
- Eğitim süresi

gibi farklı ölçütleri birlikte değerlendirerek iki boosting algoritmasının davranışlarını incelemektir.

---

## 🎯 Problem

Titanic veri setindeki yolcular için:

> **Yolcu hayatta kaldı mı?**

sorusuna cevap veren bir binary classification problemi ele alınmıştır.

Hedef değişken:

```text
Survived
```

| Değer | Sınıf |
|---|---|
| `0` | Not Survived |
| `1` | Survived |

---

## 📊 Veri Seti

Projede Titanic yarışmasına ait:

```text
gender_submission.csv
```

dosyası kullanılmıştır.

### Veri Seti Özeti

| Özellik | Değer |
|---|---:|
| Toplam gözlem | 418 |
| Toplam sütun | 2 |
| Bağımsız değişken | PassengerId |
| Hedef değişken | Survived |
| Eksik değer | 0 |

### Sınıf Dağılımı

| Sınıf | Gözlem |
|---|---:|
| Not Survived (`0`) | 266 |
| Survived (`1`) | 152 |

Veri seti `%80 eğitim` ve `%20 test` olacak şekilde ayrılmıştır.

```text
Training Set : 334
Test Set     : 84
```

Sınıf oranlarını korumak amacıyla `stratify=y` kullanılmıştır.

---

## ⚠️ Önemli Veri Seti Notu

Bu çalışma klasik Titanic `train.csv` dosyası yerine `gender_submission.csv` kullanılarak gerçekleştirilmiştir.

Bu nedenle modelde bağımsız değişken olarak yalnızca:

```python
X = df[['PassengerId']]
```

kullanılmaktadır.

Dolayısıyla bu proje özellikle **AdaBoost ve XGBoost algoritmalarının karşılaştırılmasına yönelik deneysel bir çalışma** olarak değerlendirilmelidir.

Age, Sex, Pclass, Fare, Embarked gibi klasik Titanic özellikleri bu modelde bulunmamaktadır.

---

# 🤖 Kullanılan Modeller

## 🔴 AdaBoost

AdaBoost, zayıf öğrenicileri ardışık olarak eğiterek güçlü bir sınıflandırıcı oluşturan bir ensemble learning algoritmasıdır.

Projede temel öğrenici olarak:

```python
DecisionTreeClassifier(max_depth=1)
```

kullanılmıştır.

### Parametreler

```python
AdaBoostClassifier(
    estimator=DecisionTreeClassifier(max_depth=1),
    n_estimators=100,
    learning_rate=0.1,
    random_state=42
)
```

---

## 🟢 XGBoost

XGBoost, Gradient Boosting yaklaşımını optimize eden ve özellikle yapılandırılmış verilerde yaygın olarak kullanılan güçlü bir boosting algoritmasıdır.

### Parametreler

```python
XGBClassifier(
    n_estimators=100,
    max_depth=3,
    learning_rate=0.1,
    subsample=0.8,
    colsample_bytree=0.8,
    reg_alpha=0.1,
    reg_lambda=1.0,
    eval_metric='logloss',
    random_state=42
)
```

Notebook çalıştırılırken kullanılan XGBoost sürümü:

```text
3.4.1
```

---

# 📈 Model Sonuçları

## 🏆 AdaBoost vs XGBoost

| Metrik | 🔴 AdaBoost | 🟢 XGBoost | Daha İyi |
|---|---:|---:|---|
| Accuracy | **0.6310** | 0.5119 | 🔴 AdaBoost |
| AUC-ROC | **0.4997** | 0.3990 | 🔴 AdaBoost |
| F1 Score | 0.0000 | **0.0889** | 🟢 XGBoost |
| Precision | 0.0000 | **0.1429** | 🟢 XGBoost |
| Recall | 0.0000 | **0.0645** | 🟢 XGBoost |
| CV Ortalama | **0.5262** | 0.4833 | 🔴 AdaBoost |
| CV Std | 0.1315 | **0.1104** | 🟢 XGBoost |
| Eğitim Süresi | 0.1780 sn | **0.0331 sn** | 🟢 XGBoost |

### Genel Karşılaştırma

```text
🟢 XGBoost : 5 / 8 kategoride daha iyi
🔴 AdaBoost: 3 / 8 kategoride daha iyi
```

---

# 🔴 AdaBoost Sonuçları

```text
Accuracy  : 63.10%
AUC-ROC   : 0.4997
F1 Score  : 0.0000
Precision : 0.0000
Recall    : 0.0000
```

AdaBoost test setinde daha yüksek genel doğruluk elde etmiştir.

Bununla birlikte modelin `Survived` sınıfı için:

```text
Precision = 0
Recall    = 0
F1        = 0
```

sonuçlarını üretmesi, yalnızca accuracy değerine bakmanın neden yeterli olmadığını açık biçimde göstermektedir.

---

# 🟢 XGBoost Sonuçları

```text
Accuracy  : 51.19%
AUC-ROC   : 0.3990
F1 Score  : 0.0889
Precision : 0.1429
Recall    : 0.0645
```

XGBoost'un accuracy değeri AdaBoost'tan daha düşük olmasına rağmen pozitif sınıf için tahmin üretebildiği görülmektedir.

Ayrıca:

```text
AdaBoost eğitim süresi : 0.1780 sn
XGBoost eğitim süresi  : 0.0331 sn
```

sonucuyla XGBoost bu deneyde daha kısa eğitim süresi göstermiştir.

---

# 🔄 Cross Validation

Modeller yalnızca tek bir train-test ayrımı üzerinden değerlendirilmemiştir.

Aynı zamanda:

```python
cross_val_score(..., cv=5)
```

kullanılarak **5-Fold Cross Validation** uygulanmıştır.

### Sonuçlar

| Model | Ortalama Accuracy | Standart Sapma |
|---|---:|---:|
| 🔴 AdaBoost | **0.5262** | 0.1315 |
| 🟢 XGBoost | 0.4833 | **0.1104** |

AdaBoost daha yüksek ortalama CV accuracy elde ederken XGBoost'un sonuçlarında daha düşük standart sapma gözlenmiştir.

---

# 📉 Kullanılan Görselleştirmeler

Notebook içerisinde modelleri daha ayrıntılı incelemek amacıyla çeşitli grafikler oluşturulmuştur.

### Confusion Matrix

AdaBoost ve XGBoost'un doğru ve yanlış sınıflandırmaları yan yana incelenmiştir.

### ROC Curve

Her iki model için ROC eğrileri oluşturularak sınıfları ayırma performansları karşılaştırılmıştır.

### Model Metrics Comparison

Aşağıdaki metrikler bar grafik üzerinde karşılaştırılmıştır:

```text
Accuracy
AUC-ROC
F1 Score
Precision
Recall
```

### AdaBoost n_estimators Analizi

AdaBoost için farklı `n_estimators` değerlerinde:

```text
Training Accuracy
Test Accuracy
```

değişimleri incelenmiştir.

### XGBoost Feature Importance

XGBoost modelinin kullandığı özelliğin önem skoru incelenmiştir.

### XGBoost Training Curve

Training ve validation setleri için:

```text
Log Loss
```

değerlerinin iterasyonlara göre değişimi analiz edilmiştir.

---

# 🧠 Kullanılan Değerlendirme Metrikleri

### Accuracy

Toplam tahminlerin ne kadarının doğru olduğunu gösterir.

```text
Accuracy = Doğru Tahmin / Tüm Tahminler
```

### Precision

Modelin pozitif dediği örneklerin ne kadarının gerçekten pozitif olduğunu gösterir.

### Recall

Gerçek pozitif örneklerin ne kadarının model tarafından bulunabildiğini gösterir.

### F1 Score

Precision ve Recall arasındaki dengeyi ölçer.

```text
F1 = 2 × (Precision × Recall) / (Precision + Recall)
```

### ROC-AUC

Modelin iki sınıfı birbirinden ayırma gücünü değerlendirmek için kullanılır.

---

# 🛠️ Kullanılan Teknolojiler

| Teknoloji | Amaç |
|---|---|
| 🐍 Python | Programlama |
| 🐼 Pandas | Veri işleme |
| 🔢 NumPy | Sayısal işlemler |
| 🤖 Scikit-learn | Makine öğrenmesi |
| ⚡ XGBoost | Boosting modeli |
| 📊 Matplotlib | Görselleştirme |
| 📈 Seaborn | İstatistiksel grafikler |
| 📓 Jupyter Notebook | Geliştirme ortamı |

---

# 📂 Proje Yapısı

```text
Titanic-AdaBoost-XGBoost/
│
├── 📓 titanic_adaboost_xgboost.ipynb
├── 📊 gender_submission.csv
│
└── 📄 README.md
```

---

# 🚀 Projeyi Çalıştırma

## 1. Repository'yi Klonlayın

```bash
git clone <repository-url>
```

```bash
cd Titanic-AdaBoost-XGBoost
```

## 2. Gerekli Kütüphaneleri Kurun

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost jupyter
```

## 3. Jupyter Notebook'u Başlatın

```bash
jupyter notebook
```

Ardından:

```text
titanic_adaboost_xgboost.ipynb
```

dosyasını açarak hücreleri sırayla çalıştırabilirsiniz.

---

# 🔬 Proje Akışı

```text
                 Titanic Dataset
                        │
                        ▼
               Exploratory Analysis
                        │
                        ▼
               Missing Value Check
                        │
                        ▼
                  X / y Split
                        │
                        ▼
               Train / Test Split
                        │
              ┌─────────┴─────────┐
              ▼                   ▼
         🔴 AdaBoost          🟢 XGBoost
              │                   │
              └─────────┬─────────┘
                        ▼
                 Model Evaluation
                        │
       ┌────────────────┼────────────────┐
       ▼                ▼                ▼
    Accuracy        Precision         Recall
       │                │                │
       └────────────────┼────────────────┘
                        ▼
                F1 Score / ROC-AUC
                        │
                        ▼
              5-Fold Cross Validation
                        │
                        ▼
               Model Comparison
```

---

# 💡 Çalışmadan Çıkarılan Sonuçlar

Bu çalışma önemli bir noktayı ortaya koymaktadır:

> **Bir modelin yüksek accuracy değerine sahip olması tek başına iyi bir model olduğu anlamına gelmez.**

AdaBoost `%63.10` accuracy ile XGBoost'tan daha yüksek doğruluk elde etmesine rağmen `Survived` sınıfı için Precision, Recall ve F1 değerleri `0` olmuştur.

XGBoost ise daha düşük genel accuracy değerine rağmen pozitif sınıfta tahmin üretebilmiştir.

Bu nedenle sınıflandırma modelleri değerlendirilirken yalnızca **Accuracy** değil;

**Precision, Recall, F1-Score, ROC-AUC ve Confusion Matrix** gibi metriklerin birlikte değerlendirilmesi önemlidir.

---

# ⚠️ Limitations

Bu projedeki en önemli sınırlama kullanılan veri setidir.

`gender_submission.csv` yalnızca:

```text
PassengerId
Survived
```

değişkenlerini içermektedir.

Bu nedenle model gerçek anlamda yolcuların yaş, cinsiyet, sınıf veya ekonomik özelliklerinden hayatta kalma örüntüleri öğrenmemektedir.

Daha kapsamlı bir model için Titanic `train.csv` veri setindeki:

```text
Pclass
Sex
Age
SibSp
Parch
Fare
Embarked
```

gibi özelliklerin kullanılması uygun bir sonraki geliştirme adımıdır.

---

# 🔮 Gelecek Geliştirmeler

Proje aşağıdaki çalışmalarla geliştirilebilir:

- Titanic `train.csv` veri setine geçiş
- Missing value preprocessing
- Categorical encoding
- Feature engineering
- Hyperparameter tuning
- GridSearchCV / RandomizedSearchCV
- SHAP ile model açıklanabilirliği
- Feature importance analizi
- Random Forest karşılaştırması
- Logistic Regression baseline modeli
- LightGBM / CatBoost karşılaştırması

---

# 👩‍💻 Geliştirici

**Sakine Cansu Topçi**

Yönetim Bilişim Sistemleri  
Machine Learning · Python · Data Science

---

<div align="center">

### ⭐ Bu çalışma Ensemble Learning algoritmalarını uygulamalı olarak öğrenmek amacıyla geliştirilmiştir.

**AdaBoost · XGBoost · Classification · Ensemble Learning · Python**

</div>
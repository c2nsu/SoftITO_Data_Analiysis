# 📊 Telco Customer Churn Prediction

Bu proje, telekomünikasyon müşterilerinin **aboneliklerini sonlandırma (churn)** ihtimalini makine öğrenmesi algoritmaları kullanarak tahmin etmeyi amaçlamaktadır.

Çalışmada **Decision Tree** ve **Random Forest** modelleri kullanılarak müşteri davranışları üzerinden churn tahmini yapılmış ve modeller performans metrikleri açısından karşılaştırılmıştır.

## 🎯 Projenin Amacı

Müşteri kaybı, telekomünikasyon şirketleri için önemli bir problemdir. Bir müşterinin aboneliğini sonlandırma eğilimini önceden tahmin etmek, şirketlerin müşteriyi elde tutmaya yönelik stratejiler geliştirmesine yardımcı olabilir.

Bu projede:

- Müşteri verileri incelenmiştir.
- Eksik değerler kontrol edilmiştir.
- Kategorik değişkenler sayısal forma dönüştürülmüştür.
- Veri eğitim ve test kümelerine ayrılmıştır.
- Decision Tree ve Random Forest modelleri eğitilmiştir.
- Modeller çeşitli performans metrikleriyle karşılaştırılmıştır.
- Confusion Matrix ve ROC eğrileri oluşturulmuştur.
- Random Forest kullanılarak önemli özellikler incelenmiştir.

## 📁 Veri Seti

Projede **Telco Customer Churn** veri seti kullanılmıştır.

Veri setinde:

- **7.043 gözlem**
- **21 değişken**
- Hedef değişken: `Churn`

bulunmaktadır.

Hedef değişken iki sınıftan oluşmaktadır:

- `No` → Müşteri aboneliğini sonlandırmamış
- `Yes` → Müşteri aboneliğini sonlandırmış

Sınıf dağılımı:

| Churn | Oran |
|---|---:|
| No | %73,5 |
| Yes | %26,5 |

## 🛠️ Kullanılan Teknolojiler

- Python
- NumPy
- Pandas
- Matplotlib
- Seaborn
- Scikit-learn
- Jupyter Notebook

## 🔄 Veri Ön İşleme

Modelleme öncesinde veri seti üzerinde temel veri ön işleme adımları uygulanmıştır.

### Eksik Değer Kontrolü

Veri setinde toplam eksik değer sayısı:

```text
0
```

Eksik değer bulunması durumunda ise sayısal değişkenlerde medyan, kategorik değişkenlerde mod kullanılarak doldurma işlemi uygulanacak şekilde yapı oluşturulmuştur.

### Kategorik Değişkenlerin Dönüştürülmesi

Kategorik değişkenler `pd.get_dummies()` kullanılarak sayısal forma dönüştürülmüştür.

Hedef değişken olan `Churn` ise `LabelEncoder` kullanılarak sayısallaştırılmıştır.

### Eğitim ve Test Verisi

Veri:

- %75 eğitim
- %25 test

olacak şekilde ayrılmıştır.

Ayrıca `stratify` kullanılarak sınıf dağılımının eğitim ve test setlerinde korunması sağlanmıştır.

## 🤖 Kullanılan Modeller

### Decision Tree

Karar ağacı modeli aşağıdaki parametrelerle oluşturulmuştur:

```python
DecisionTreeClassifier(
    max_depth=6,
    min_samples_leaf=5,
    class_weight="balanced",
    random_state=42
)
```

### Random Forest

Random Forest modeli ise:

```python
RandomForestClassifier(
    n_estimators=300,
    min_samples_leaf=2,
    class_weight="balanced",
    n_jobs=-1,
    random_state=42
)
```

parametreleriyle oluşturulmuştur.

Her iki modelde de `class_weight="balanced"` kullanılarak sınıf dengesizliğinin etkisinin azaltılması hedeflenmiştir.

## 📈 Model Sonuçları

Test setinde elde edilen sonuçlar:

| Model | Accuracy | F1 Score |
|---|---:|---:|
| Decision Tree | **%75,0** | **0,762** |
| Random Forest | %59,9 | 0,615 |

5 katlı çapraz doğrulama sonuçları:

| Model | CV Ortalama | CV Std |
|---|---:|---:|
| Decision Tree | **0,723** | 0,015 |
| Random Forest | 0,688 | 0,018 |

Bu çalışmadaki sonuçlara göre **Decision Tree**, hem test doğruluğu hem de F1 skoru açısından Random Forest modelinden daha başarılı performans göstermiştir.

## 📊 Kullanılan Değerlendirme Metrikleri

Modellerin performansını değerlendirmek için:

- Accuracy
- Precision
- Recall
- F1 Score
- ROC-AUC
- 5-Fold Cross Validation

metriklerinden yararlanılmıştır.

Ayrıca modellerin tahmin sonuçlarını incelemek amacıyla **Confusion Matrix** ve **ROC Curve** görselleştirmeleri oluşturulmuştur.

## 🔍 Random Forest Özellik Önemleri

Random Forest modelinin `feature_importances_` özelliği kullanılarak model açısından en önemli 10 değişken incelenmiştir.

Bu analiz, hangi müşteri özelliklerinin churn tahmininde model tarafından daha fazla kullanıldığını görmeyi sağlamaktadır.

## 📂 Proje Yapısı

```text
Telco-Customer-Churn/
│
├── rf_df_analiz_.ipynb
├── Telco-Customer-Churn.csv
├── model_karsilastirma.csv
└── README.md
```

## 🚀 Çalıştırma

Projeyi çalıştırmak için:

```bash
git clone <repository-url>
cd Telco-Customer-Churn
```

Gerekli kütüphaneleri yükleyin:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn jupyter
```

Ardından Jupyter Notebook'u başlatın:

```bash
jupyter notebook
```

ve

```text
rf_df_analiz_.ipynb
```

dosyasını çalıştırın.

> **Not:** Notebook'un çalışabilmesi için `Telco-Customer-Churn.csv` dosyasının notebook ile aynı klasörde bulunması gerekir.

## 🧠 Çıkarım

Bu çalışmada iki farklı ağaç tabanlı sınıflandırma algoritması karşılaştırılmıştır. Elde edilen sonuçlarda **Decision Tree modeli**, kullanılan test seti ve değerlendirme kriterleri kapsamında daha yüksek genel performans göstermiştir.

Proje, müşteri kaybı tahmini üzerinden **veri ön işleme → modelleme → değerlendirme → model karşılaştırması → özellik önem analizi** sürecinin uygulanmasını göstermektedir.

---

### 👩‍💻 Geliştirici

**Sakine Cansu Topci**

Veri Analitiği & Yapay Zeka / Makine Öğrenmesi
# 🧬 Breast Cancer Classification with SVM

Bu proje, hücre çekirdeği ölçümlerinden yararlanarak meme tümörlerinin **Malign (M)** veya **Benign (B)** olarak sınıflandırılmasını amaçlayan bir **Support Vector Machine (SVM)** çalışmasıdır.

Projede farklı SVM kernel fonksiyonları karşılaştırılmış ve **GridSearchCV** kullanılarak en uygun hiperparametreler belirlenmiştir.

## 🎯 Projenin Amacı

Meme kanseri teşhisinde kullanılan hücre çekirdeği ölçülerini kullanarak tümörlerin sınıflandırılması hedeflenmiştir.

Çalışmada:

- Veri seti incelenmiştir.
- Bağımsız ve hedef değişkenler belirlenmiştir.
- Eksik değer kontrolü yapılmıştır.
- Veriler `StandardScaler` ile ölçeklendirilmiştir.
- Linear, RBF ve Polynomial SVM modelleri oluşturulmuştur.
- Modeller karşılaştırılmıştır.
- GridSearchCV ile RBF SVM optimize edilmiştir.
- Confusion Matrix oluşturulmuştur.
- Destek vektörleri analiz edilmiştir.
- Linear SVM katsayıları üzerinden özelliklerin etkisi incelenmiştir.
- Test verisi üzerinde örnek tahminler gerçekleştirilmiştir.

## 📊 Veri Seti

Projede `breast-cancer.csv` veri seti kullanılmıştır.

- **569 hasta kaydı**
- **32 sütun**
- Hedef değişken: `diagnosis`

Hedef değişken:

| Değer | Anlam |
|---|---|
| `M` | Malign (kötü huylu) |
| `B` | Benign (iyi huylu) |

Veri setindeki temel hücre çekirdeği ölçümleri:

- Radius
- Texture
- Perimeter
- Area
- Smoothness
- Compactness
- Concavity
- Concave Points
- Symmetry
- Fractal Dimension

Bu özelliklerin `mean`, `SE` ve `worst` değerleri kullanılmıştır.

## 🧹 Veri Ön İşleme

SVM mesafe tabanlı bir algoritma olduğu için veri ön işleme aşamasında özelliklerin ölçeklendirilmesine önem verilmiştir.

Uygulanan işlemler:

1. `id` sütunu modelden çıkarılmıştır.
2. `diagnosis` hedef değişken olarak belirlenmiştir.
3. Hedef değişken `LabelEncoder` ile sayısallaştırılmıştır.
4. Eksik değer kontrolü yapılmıştır.
5. Veri %80 eğitim ve %20 test olarak ayrılmıştır.
6. `stratify` kullanılarak sınıf dağılımı korunmuştur.
7. Bağımsız değişkenler `StandardScaler` ile ölçeklendirilmiştir.

```python
scaler = StandardScaler()

X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

## 🤖 SVM Modelleri

Projede üç farklı kernel fonksiyonu kullanılmıştır.

### Linear SVM

Düzlemsel ve doğrusal bir karar sınırı oluşturur.

```python
SVC(kernel='linear')
```

### RBF SVM

Doğrusal olmayan ilişkileri yakalamak için radial basis function kernel kullanılmıştır.

```python
SVC(kernel='rbf')
```

### Polynomial SVM

Polinom tabanlı karar sınırı oluşturmak için kullanılmıştır.

```python
SVC(kernel='poly', degree=3)
```

## ⚙️ Hiperparametre Optimizasyonu

RBF SVM için `GridSearchCV` kullanılarak farklı `C` ve `gamma` değerleri denenmiştir.

```python
param_grid = {
    'C': [0.1, 1, 10, 100],
    'gamma': ['scale', 'auto', 0.1, 0.01],
    'kernel': ['rbf']
}
```

5-fold cross-validation kullanılarak toplam **16 farklı parametre kombinasyonu** değerlendirilmiştir.

### En iyi parametreler

```text
C      = 1
gamma  = scale
kernel = rbf
```

Cross-validation skoru:

**%97,6**

## 📈 Model Sonuçları

Modellerin test setindeki doğruluk sonuçları:

| Model | Accuracy |
|---|---:|
| Linear SVM | **%96,5** |
| RBF SVM | **%97,4** |
| Polynomial SVM | %88,6 |
| **Optimize RBF SVM** | **%97,4** |

RBF kernel, Linear SVM'e göre küçük bir performans artışı sağlamıştır.

Polynomial SVM ise bu veri setinde diğer iki modele göre daha düşük performans göstermiştir.

## 🔍 Destek Vektör Analizi

Optimize edilmiş RBF SVM modelinin destek vektörleri ayrıca incelenmiştir.

Destek vektörleri, SVM'in karar sınırını belirleyen ve hyperplane'e en yakın eğitim örnekleridir.

Bu analiz ile:

- Toplam destek vektörü sayısı
- Her sınıfa ait destek vektörü sayısı
- Destek vektörü oranı

hesaplanmıştır.

Bu oran, sınıfların karar sınırı açısından ne kadar net ayrıldığını değerlendirmek için kullanılmaktadır.

## 🧠 Özellik Analizi

Linear SVM modelinin katsayıları kullanılarak özelliklerin sınıflandırmaya katkısı incelenmiştir.

Mutlak katsayı değeri daha yüksek olan özellikler, modelin karar sınırında daha fazla etkiye sahiptir.

Analizde özellikle hücre çekirdeğinin:

- Radius
- Concave Points
- Perimeter
- Area
- Concavity

gibi ölçümlerinin sınıflandırmada öne çıktığı görülmüştür.

## 📊 Değerlendirme

Model performansını incelemek için:

- Accuracy
- Precision
- Recall
- F1 Score
- Confusion Matrix
- Cross Validation

kullanılmıştır.

Özellikle **Malign (M) sınıfının Recall değeri** önemlidir. Çünkü kötü huylu bir vakayı benign olarak tahmin etmek, yanlış pozitif bir tahminden daha kritik bir sonuç doğurabilir.

> Bu proje eğitim amaçlı bir makine öğrenmesi çalışmasıdır ve gerçek klinik teşhis amacıyla kullanılmamalıdır.

## 🗂️ Proje Yapısı

```text
SVM/
│
├── svm_analizi.ipynb
├── breast-cancer.csv
└── README.md
```

## 🛠️ Kullanılan Teknolojiler

- Python
- Pandas
- NumPy
- Scikit-learn
- Jupyter Notebook

## 🚀 Çalıştırma

Repoyu klonlayın:

```bash
git clone <repository-url>
cd SVM
```

Gerekli kütüphaneleri yükleyin:

```bash
pip install pandas numpy scikit-learn jupyter
```

Jupyter Notebook'u başlatın:

```bash
jupyter notebook
```

Ardından:

```text
svm_analizi.ipynb
```

dosyasını açarak hücreleri sırayla çalıştırabilirsiniz.

## 📌 Sonuç

Bu çalışmada meme kanseri sınıflandırması için üç farklı SVM kernel'i karşılaştırılmıştır.

Sonuçlar, **RBF SVM'in %97,4 test doğruluğu** ile en başarılı modellerden biri olduğunu göstermiştir. GridSearchCV sonucunda `C=1` ve `gamma=scale` parametreleriyle optimize edilen RBF modeli, **%97,6 cross-validation skoru** elde etmiştir.

Çalışma aynı zamanda SVM'in yalnızca tahmin performansını değil, **kernel seçimi, hiperparametre optimizasyonu, destek vektörleri ve özellik katsayıları** açısından da incelenmesini sağlamaktadır.

---

### 👩‍💻 Author

**Sakine Cansu Topci**

Data Analytics & Artificial Intelligence
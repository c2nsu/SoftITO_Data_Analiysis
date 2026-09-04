# 👕 Fashion-MNIST Image Classification with CNN

Bu projede **Fashion-MNIST** veri seti kullanılarak görüntü sınıflandırma problemi çözülmüştür.

Temel olarak özel bir **Convolutional Neural Network (CNN)** modeli geliştirilmiş, ardından **MobileNetV2 Transfer Learning** yaklaşımı uygulanarak iki farklı derin öğrenme yaklaşımının performanslarının karşılaştırılması amaçlanmıştır.

---

## 🎯 Projenin Amacı

Bu çalışmanın amacı Fashion-MNIST veri setindeki kıyafet görüntülerini doğru sınıflara ayırabilen bir derin öğrenme modeli geliştirmektir.

Proje kapsamında:

- Görüntü verilerinin hazırlanması
- Normalizasyon
- Data Augmentation
- CNN mimarisi oluşturma
- Model eğitimi
- Early Stopping
- Learning Rate optimizasyonu
- Test verisi üzerinde değerlendirme
- Confusion Matrix
- Classification Report
- Transfer Learning
- CNN ve MobileNetV2 karşılaştırması

uygulanmıştır.

---

# 📊 Veri Seti

Projede TensorFlow/Keras içerisinde bulunan **Fashion-MNIST** veri seti kullanılmıştır.

Fashion-MNIST, 28×28 piksel boyutunda gri tonlamalı kıyafet görüntülerinden oluşmaktadır.

Veri setinde toplam **10 farklı sınıf** bulunmaktadır.

| Label | Sınıf |
|---:|---|
| 0 | T-shirt/top |
| 1 | Trouser |
| 2 | Pullover |
| 3 | Dress |
| 4 | Coat |
| 5 | Sandal |
| 6 | Shirt |
| 7 | Sneaker |
| 8 | Bag |
| 9 | Ankle boot |

---

# 🔍 Veri Keşfi

Veri seti TensorFlow Keras üzerinden yüklenmiştir:

```python
from tensorflow.keras.datasets import fashion_mnist

(x_train, y_train), (x_test, y_test) = fashion_mnist.load_data()
```

Eğitim ve test görüntülerinden rastgele örnekler seçilerek sınıflar görselleştirilmiştir.

---

# ⚙️ Veri Ön İşleme

CNN modelinin daha stabil ve hızlı öğrenebilmesi için görüntü piksel değerleri normalize edilmiştir.

Orijinal piksel değerleri:

```text
0 - 255
```

Normalize edildikten sonra:

```text
0 - 1
```

aralığına dönüştürülmüştür.

```python
x_train_normalized = x_train.astype("float32") / 255.0
x_test_normalized = x_test.astype("float32") / 255.0
```

Etiketler ise **One-Hot Encoding** yöntemiyle dönüştürülmüştür.

---

# 🖼️ Data Augmentation

Modelin eğitim verisini ezberlemesini azaltmak ve farklı görüntü varyasyonlarına karşı daha dayanıklı hale gelmesini sağlamak amacıyla `ImageDataGenerator` kullanılmıştır.

Uygulanan augmentation işlemleri:

```python
ImageDataGenerator(
    rotation_range=10,
    width_shift_range=0.1,
    height_shift_range=0.1,
    zoom_range=0.1,
    fill_mode="nearest"
)
```

Bu işlemler sayesinde görüntüler üzerinde küçük döndürme, kaydırma ve yakınlaştırma değişiklikleri oluşturulabilmektedir.

---

# 🧠 CNN Modeli

Projede özel bir CNN mimarisi oluşturulmuştur.

Model genel olarak üç convolutional blok ve ardından fully connected katmanlardan oluşmaktadır.

```text
Input
 │
 ▼
Conv2D 32
 │
Batch Normalization
 │
Conv2D 32
 │
Batch Normalization
 │
MaxPooling
 │
Dropout
 │
 ▼
Conv2D 64
 │
Batch Normalization
 │
Conv2D 64
 │
Batch Normalization
 │
MaxPooling
 │
Dropout
 │
 ▼
Conv2D 128
 │
Batch Normalization
 │
MaxPooling
 │
Dropout
 │
 ▼
Flatten
 │
Dense 256
 │
Batch Normalization
 │
Dropout
 │
 ▼
Dense 10 – Softmax
```

CNN katmanlarında **ReLU**, çıkış katmanında ise çok sınıflı sınıflandırma için **Softmax** aktivasyon fonksiyonu kullanılmıştır.

---

# 🛠️ Model Derleme

Model aşağıdaki yapılandırmayla derlenmiştir:

```python
model_cnn.compile(
    optimizer="adam",
    loss="categorical_crossentropy",
    metrics=["accuracy"]
)
```

### Optimizer

```text
Adam
```

### Loss Function

```text
Categorical Crossentropy
```

### Evaluation Metric

```text
Accuracy
```

---

# 🚀 Model Eğitimi

CNN modeli maksimum **30 epoch** boyunca eğitilecek şekilde yapılandırılmıştır.

```python
history = model_cnn.fit(
    x_train_processed,
    y_train_categorical,
    batch_size=128,
    epochs=30,
    validation_split=0.2,
    callbacks=[early_stopping, reduce_lr]
)
```

Eğitim verisinin `%20`'si validation seti olarak kullanılmıştır.

---

# 🛑 Early Stopping

Modelin gereksiz yere eğitilmesini ve overfitting oluşmasını azaltmak amacıyla **EarlyStopping** kullanılmıştır.

```python
EarlyStopping(
    monitor="val_loss",
    patience=5,
    restore_best_weights=True
)
```

Validation loss 5 epoch boyunca iyileşmezse eğitim durdurulmakta ve en iyi model ağırlıkları geri yüklenmektedir.

---

# 📉 Learning Rate Optimization

Modelin öğrenme sürecini iyileştirmek amacıyla **ReduceLROnPlateau** callback'i kullanılmıştır.

```python
ReduceLROnPlateau(
    monitor="val_loss",
    factor=0.5,
    patience=3,
    min_lr=1e-7
)
```

Validation loss gelişmediğinde learning rate otomatik olarak azaltılmaktadır.

---

# 📈 Eğitim Sonuçlarının Görselleştirilmesi

Model eğitiminden sonra aşağıdaki değerler grafiklerle incelenmiştir:

- Training Accuracy
- Validation Accuracy
- Training Loss
- Validation Loss

Bu grafikler modelin öğrenme davranışının ve olası overfitting durumunun gözlemlenmesini sağlamaktadır.

---

# 🧪 Test Seti ile Değerlendirme

Eğitilen CNN modeli daha önce görmediği test görüntüleri üzerinde değerlendirilmiştir.

```python
test_loss, test_accuracy = model_cnn.evaluate(
    x_test_processed,
    y_test_categorical
)
```

Test Accuracy ve Test Loss değerleri kullanılarak modelin genelleme performansı değerlendirilmiştir.

---

# 🔮 Model Tahminleri

Model test veri setindeki görüntüler üzerinde tahmin üretmektedir.

Her görüntü için:

- Tahmin edilen sınıf
- Gerçek sınıf
- Model güven skoru

gösterilmektedir.

Doğru tahminler ve yanlış tahminler farklı renkler kullanılarak görselleştirilmiştir.

---

# 📊 Confusion Matrix

Modelin hangi sınıfları doğru veya yanlış tahmin ettiğini detaylı olarak incelemek için **Confusion Matrix** kullanılmıştır.

Confusion Matrix sayesinde özellikle birbirine benzeyen kıyafet sınıflarının model tarafından ne kadar karıştırıldığı analiz edilebilmektedir.

---

# 📋 Classification Report

Model performansı yalnızca accuracy kullanılarak değerlendirilmemiştir.

Scikit-learn `classification_report` kullanılarak her sınıf için:

- Precision
- Recall
- F1-Score
- Support

değerleri hesaplanmaktadır.

```python
classification_report(
    y_test,
    y_pred,
    target_names=class_names
)
```

---

# 🔄 Transfer Learning – MobileNetV2

Projede özel CNN modeline ek olarak **Transfer Learning** yöntemi de uygulanmıştır.

Bunun için ImageNet üzerinde önceden eğitilmiş:

```text
MobileNetV2
```

modeli kullanılmıştır.

```python
base_model = MobileNetV2(
    input_shape=(32, 32, 3),
    include_top=False,
    weights="imagenet"
)
```

Önceden öğrenilmiş ağırlıkların korunması için MobileNetV2 katmanları dondurulmuştur:

```python
base_model.trainable = False
```

---

# 🖼️ Fashion-MNIST → MobileNetV2 Dönüşümü

Fashion-MNIST görüntüleri:

```text
28 × 28 × 1
```

boyutundadır.

MobileNetV2 ise RGB görüntü beklediğinden görüntüler:

```text
32 × 32 × 3
```

boyutuna dönüştürülmüştür.

Bu işlem için TensorFlow kullanılmıştır:

```python
x_train_for_transfer = tf.image.resize(
    x_train_processed,
    [32, 32]
)

x_train_for_transfer = tf.image.grayscale_to_rgb(
    x_train_for_transfer
)
```

---

# 🧠 Transfer Learning Mimarisi

Kullanılan mimari:

```text
MobileNetV2
      │
      ▼
GlobalAveragePooling2D
      │
      ▼
Dense 256 – ReLU
      │
      ▼
Dropout 0.5
      │
      ▼
Dense 10 – Softmax
```

Transfer Learning modeli maksimum **15 epoch** boyunca eğitilmektedir.

---

# ⚖️ Model Karşılaştırması

Projenin sonunda iki farklı yaklaşım karşılaştırılmıştır:

| Model | Yaklaşım |
|---|---|
| Custom CNN | Sıfırdan oluşturulan CNN |
| MobileNetV2 | Transfer Learning |

Her iki model test veri seti üzerinde değerlendirilerek **Test Accuracy** değerleri karşılaştırılmaktadır.

Bu karşılaştırma sayesinde Fashion-MNIST gibi küçük gri tonlamalı görüntüler üzerinde sıfırdan oluşturulan CNN ile önceden eğitilmiş bir model kullanmanın etkisi incelenmektedir.

---

# 🛠️ Kullanılan Teknolojiler

Projede kullanılan başlıca teknolojiler:

- Python
- TensorFlow
- Keras
- Scikit-learn
- NumPy
- Pandas
- Matplotlib
- OpenCV
- Jupyter Notebook

---

# 📦 Kullanılan Kütüphaneler

```python
numpy
pandas
matplotlib
opencv-python
scikit-learn
tensorflow
keras
```

Gerekli paketleri yüklemek için:

```bash
pip install numpy pandas matplotlib opencv-python scikit-learn tensorflow
```

---

# 📁 Proje Yapısı

```text
Fashion-MNIST-CNN-Classification/
│
├── CNN_mnist_veri_seti_sınıflandırma.ipynb
│
└── README.md
```

Fashion-MNIST veri seti Keras üzerinden otomatik olarak indirildiği için ayrıca veri seti dosyasının projeye eklenmesi gerekmemektedir.

---

# ▶️ Projeyi Çalıştırma

Repository'yi klonlayın:

```bash
git clone https://github.com/kullanici-adiniz/Fashion-MNIST-CNN-Classification.git
```

Proje klasörüne gidin:

```bash
cd Fashion-MNIST-CNN-Classification
```

Gerekli kütüphaneleri yükleyin:

```bash
pip install numpy pandas matplotlib opencv-python scikit-learn tensorflow jupyter
```

Jupyter Notebook'u başlatın:

```bash
jupyter notebook
```

Ardından:

```text
CNN_mnist_veri_seti_sınıflandırma.ipynb
```

dosyasını açıp hücreleri sırasıyla çalıştırabilirsiniz.

---

# 💡 Projede Öğrenilenler

Bu proje kapsamında aşağıdaki konular üzerinde uygulamalı olarak çalışılmıştır:

- Görüntü sınıflandırma
- Convolutional Neural Networks
- Conv2D
- MaxPooling
- Batch Normalization
- Dropout
- Softmax
- Data Normalization
- One-Hot Encoding
- Data Augmentation
- Early Stopping
- ReduceLROnPlateau
- Confusion Matrix
- Precision
- Recall
- F1-Score
- Transfer Learning
- MobileNetV2
- Model performans karşılaştırması

---

# 📌 Sonuç

Bu projede Fashion-MNIST veri seti üzerinde bir **CNN tabanlı görüntü sınıflandırma sistemi** geliştirilmiştir.

Özel CNN modelinde convolution, pooling, batch normalization ve dropout katmanları kullanılarak modelin görüntülerden özellik çıkarması sağlanmıştır.

Model performansı test verileri, Confusion Matrix ve Classification Report ile değerlendirilmiştir.

Buna ek olarak ImageNet üzerinde önceden eğitilmiş **MobileNetV2** kullanılarak Transfer Learning uygulanmış ve sıfırdan oluşturulan CNN modeliyle karşılaştırılmıştır.

Bu çalışma, görüntü sınıflandırma problemlerinde hem **CNN mimarisinin sıfırdan geliştirilmesini** hem de **Transfer Learning yaklaşımının uygulanmasını** gösteren bir derin öğrenme projesidir.

---

## 👩‍💻 Author

**Sakine Cansu TOPCİ**

Yönetim Bilişim Sistemleri  
Python • Machine Learning • Deep Learning

---

⭐ Projeyi faydalı bulduysanız repository'yi yıldızlayabilirsiniz.
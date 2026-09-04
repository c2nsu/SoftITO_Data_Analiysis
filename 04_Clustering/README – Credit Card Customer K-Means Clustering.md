# 💳 Credit Card Customer Segmentation – K-Means Clustering

Bu projede kredi kartı müşterilerinin finansal ve davranışsal özellikleri kullanılarak **denetimsiz öğrenme (Unsupervised Learning)** yöntemleriyle müşteri segmentasyonu gerçekleştirilmiştir.

Çalışmanın temel amacı, benzer davranışlara sahip müşterileri kümelere ayırarak farklı müşteri profillerini ortaya çıkarmaktır.

## 🎯 Projenin Amacı

Müşterilerin;

- Ortalama kredi limiti
- Sahip oldukları kredi kartı sayısı
- Banka ziyaret sayısı
- Online ziyaret sayısı
- Çağrı merkezi kullanım sayısı

gibi özellikleri kullanılarak müşteri gruplarının belirlenmesi amaçlanmıştır.

Bu kapsamda farklı kümeleme algoritmaları uygulanarak sonuçları karşılaştırılmıştır.

---

## 📊 Veri Seti

Projede **Credit Card Customer Data** veri seti kullanılmıştır.

Veri setinde:

- **660 müşteri**
- **7 sütun**
- Eksik veri bulunmuyor
- Tekrarlanan satır bulunmuyor

bulunmaktadır.

### Kullanılan Değişkenler

| Değişken | Açıklama |
|---|---|
| `Sl_No` | Müşteri sıra numarası |
| `Customer Key` | Müşteri kimlik anahtarı |
| `Avg_Credit_Limit` | Ortalama kredi limiti |
| `Total_Credit_Cards` | Toplam kredi kartı sayısı |
| `Total_visits_bank` | Banka ziyaret sayısı |
| `Total_visits_online` | Online ziyaret sayısı |
| `Total_calls_made` | Çağrı merkezi kullanım sayısı |

Kümeleme sırasında müşteri kimliği niteliğindeki değişkenler yerine müşteri davranışını temsil eden sayısal değişkenlere odaklanılmıştır.

---

## 🧠 Kullanılan Yöntemler

Projede aşağıdaki yöntemler kullanılmıştır:

### 1. K-Means Clustering

K-Means algoritması ile müşteriler farklı gruplara ayrılmıştır.

Optimal küme sayısını belirlemek için:

- **Elbow Method**
- **Silhouette Score**

kullanılmıştır.

K=2 ile K=10 arasındaki değerler incelenmiş ve en yüksek Silhouette Score **K=3** için elde edilmiştir.

**K=3 → Silhouette Score: 0.5157**

### 2. Hierarchical Clustering

Müşteriler arasındaki benzerlikleri hiyerarşik olarak incelemek amacıyla **Agglomerative Clustering** uygulanmıştır.

Sonuçlar PCA kullanılarak iki boyutlu şekilde görselleştirilmiştir.

### 3. DBSCAN

Yoğunluk tabanlı kümeleme yaklaşımını incelemek için DBSCAN uygulanmıştır.

Kullanılan parametreler:

```text
eps = 1.0
min_samples = 5
```

Model sonucunda:

- **2 küme**
- **18 gürültü / aykırı değer**
- **%2.73 gürültü oranı**
- **0.5212 Silhouette Score**

elde edilmiştir.

---

## ⚙️ Veri Ön İşleme

Kümeleme algoritmalarından önce veriler incelenmiş ve gerekli ön işleme adımları uygulanmıştır.

Kullanılan işlemler:

1. Veri setinin yüklenmesi
2. Veri yapısının incelenmesi
3. Eksik değer kontrolü
4. Duplicate kontrolü
5. Sayısal değişkenlerin incelenmesi
6. Standardizasyon
7. Kümeleme modellerinin uygulanması

Özelliklerin farklı ölçeklerde olması nedeniyle **StandardScaler** kullanılarak veriler standartlaştırılmıştır.

---

## 📏 Model Değerlendirme

Kümeleme sonuçlarını değerlendirmek için üç farklı metrik kullanılmıştır:

### Silhouette Score

Kümelerin birbirinden ne kadar iyi ayrıldığını ölçmek için kullanılmıştır.

**K-Means:**

```text
Silhouette Score = 0.5157
```

### Davies-Bouldin Index

Kümelerin birbirine olan benzerliğini ve küme içi dağılımı değerlendirmek için kullanılmıştır.

```text
Davies-Bouldin Index = 0.6797
```

### Calinski-Harabasz Score

Kümeler arası ayrışmayı ve küme içi yoğunluğu değerlendirmek için kullanılmıştır.

```text
Calinski-Harabasz Score = 833.3426
```

K-Means modeli için optimal küme sayısı **3** olarak belirlenmiştir.

---

## 👥 K-Means Müşteri Profilleri

K=3 sonucunda müşteriler üç farklı profile ayrılmıştır.

Küme ortalamalarına göre genel profiller:

| Küme | Ortalama Kredi Limiti | Kredi Kartı | Genel Profil |
|---|---:|---:|---|
| Cluster 0 | ~33,782 | ~5.52 | Orta düzey müşteriler |
| Cluster 1 | ~141,040 | ~8.74 | Yüksek limitli müşteriler |
| Cluster 2 | ~12,174 | ~2.41 | Düşük limitli müşteriler |

Bu profiller, müşterilerin finansal kapasite ve bankacılık davranışlarına göre farklı segmentlere ayrılabileceğini göstermektedir.

---

## 📉 PCA ile Görselleştirme

Kümeleme sonuçlarının iki boyutta görselleştirilebilmesi için **Principal Component Analysis (PCA)** kullanılmıştır.

PCA sayesinde yüksek boyutlu müşteri verileri iki ana bileşene indirgenerek kümelerin görsel olarak incelenmesi sağlanmıştır.

---

## 🛠️ Kullanılan Teknolojiler

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- SciPy
- Jupyter Notebook

### Kullanılan Scikit-learn Modülleri

```python
StandardScaler
KMeans
AgglomerativeClustering
DBSCAN
PCA
silhouette_score
davies_bouldin_score
calinski_harabasz_score
```

---

## 📁 Proje Yapısı

```text
KMeans-Customer-Segmentation/
│
├── KMeans.ipynb
├── Credit_Card_Customer_Data.csv
└── README.md
```

---

## 🚀 Çalıştırma

Projeyi bilgisayarınıza klonlayın:

```bash
git clone https://github.com/kullanici-adiniz/KMeans-Customer-Segmentation.git
```

Proje klasörüne girin:

```bash
cd KMeans-Customer-Segmentation
```

Gerekli kütüphaneleri yükleyin:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn scipy jupyter
```

Jupyter Notebook'u başlatın:

```bash
jupyter notebook
```

Ardından:

```text
KMeans.ipynb
```

dosyasını açarak hücreleri sırasıyla çalıştırabilirsiniz.

---

## 📌 Projede Öğrenilenler

Bu çalışma kapsamında;

- K-Means algoritmasının uygulanması
- Optimal K değerinin belirlenmesi
- Elbow Method
- Silhouette Score
- Davies-Bouldin Index
- Calinski-Harabasz Score
- Hierarchical Clustering
- DBSCAN
- PCA ile boyut indirgeme
- Müşteri segmentasyonu
- Küme profillerinin yorumlanması

konuları üzerinde çalışılmıştır.

---

## 📈 Sonuç

Çalışmada farklı kümeleme yöntemleri kullanılarak kredi kartı müşterilerinin davranışsal ve finansal özelliklerine göre segmentasyonu gerçekleştirilmiştir.

K-Means analizinde **3 kümenin** en uygun yapı olduğu belirlenmiş ve müşteriler genel olarak düşük, orta ve yüksek kredi limitine sahip profiller şeklinde ayrışmıştır.

Ayrıca DBSCAN gibi farklı bir yaklaşım kullanılarak küme yapısının ve aykırı gözlemlerin nasıl değiştiği incelenmiştir.

Bu çalışma, müşteri segmentasyonu gibi problemlerde **unsupervised learning** yöntemlerinin nasıl kullanılabileceğine yönelik bir uygulama örneğidir.

---

## 👩‍💻 Author

**Sakine Cansu TOPCİ**

Yönetim Bilişim Sistemleri | Python | Machine Learning | Data Science

---

⭐ Eğer proje faydalı olduysa repository'yi **star**lamayı unutmayın!
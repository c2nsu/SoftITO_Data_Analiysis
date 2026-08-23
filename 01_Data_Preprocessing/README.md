# 🧹 Superstore Veri Temizleme & Analiz

> **Pandas ile gerçek satış verisi üzerinde uçtan uca veri temizleme, veri kalitesi kontrolü, özellik türetme ve aykırı değer analizi.**

Bu proje, `Superstore.csv` veri setinin **Pandas** kullanılarak incelenmesi ve analize daha uygun, daha düzenli bir yapıya dönüştürülmesi amacıyla hazırlanmıştır.

Çalışma yalnızca veriyi temizlemekle sınırlı değildir; tarih alanlarının dönüştürülmesi, yeni değişkenlerin oluşturulması, veri kalitesi kontrolleri, IQR yöntemiyle aykırı değerlerin belirlenmesi, metin temizliği, filtreleme ve yeniden kullanılabilir bir temizleme fonksiyonu da uygulanmıştır.

---

## 📊 Veri Setine Genel Bakış

İlk durumda veri seti:

| Özellik | Değer |
|---|---:|
| Satır sayısı | **9.994** |
| Sütun sayısı | **21** |
| Sayısal sütun | 6 |
| Metin sütunu | 15 |
| Eksik değer | **0** |
| Tamamen tekrar eden satır | **0** |

Veri setinde müşteri, sipariş, ürün, kategori, bölge, satış, indirim ve kâr gibi bilgiler bulunmaktadır.

### 🗂️ Temel Veri Alanları

| Grup | Örnek sütunlar |
|---|---|
| 🧾 Sipariş | `Order ID`, `Order Date`, `Ship Date`, `Ship Mode` |
| 👤 Müşteri | `Customer ID`, `Customer Name`, `Segment` |
| 🌎 Konum | `Country`, `City`, `State`, `Postal Code`, `Region` |
| 📦 Ürün | `Product ID`, `Product Name`, `Category`, `Sub-Category` |
| 💰 Finansal | `Sales`, `Discount`, `Profit` |
| 🔢 Miktar | `Quantity` |

---

## 🔄 Uygulanan Veri Süreci

```mermaid
flowchart LR
    A["📥 Superstore.csv"] --> B["🔎 İlk Veri İncelemesi"]
    B --> C["🩺 Veri Sağlığı Kontrolü"]
    C --> D["📅 Tarih Dönüşümü"]
    D --> E["🧩 Yeni Değişkenler"]
    E --> F["📈 Aykırı Değer Analizi"]
    F --> G["🏷️ Aykırı Değer Bayrakları"]
    G --> H["🧹 Metin Temizliği"]
    H --> I["🔢 İndeks Düzenleme"]
    I --> J["💾 Superstore_temiz.csv"]
```

---

# 🔎 1. Veriyi Yükleme ve İlk İnceleme

Veri `pandas.read_csv()` ile içeri alınmış ve ilk olarak veri setinin boyutu ve örnek satırları incelenmiştir.

```python
df_ham = pd.read_csv("Superstore.csv", encoding="latin1")

print("Boyut:", df_ham.shape)
df_ham.head()
```

İlk boyut:

**9.994 satır × 21 sütun**

Ardından `.info()` kullanılarak sütunların veri tipleri ve doluluk durumları kontrol edilmiştir.

---

# 🩺 2. Veri Sağlığı Kontrolü

Veri setinde iki temel kalite kontrolü yapılmıştır:

- Eksik değer kontrolü
- Tamamen tekrar eden satır kontrolü

```python
df_ham.isnull().sum()
df_ham.duplicated().sum()
```

### Sonuç

- **Eksik değer bulunmadı.**
- **Tamamen tekrar eden satır bulunmadı.**

Bu nedenle eksik değer doldurma veya doğrudan duplicate satır silme işlemi uygulanmamıştır.

---

# 📅 3. Tarih Sütunlarının Düzenlenmesi

`Order Date` ve `Ship Date` başlangıçta `object` tipindeyken tarih formatına dönüştürülmüştür.

```python
df_ham["Order Date"] = pd.to_datetime(df_ham["Order Date"])
df_ham["Ship Date"] = pd.to_datetime(df_ham["Ship Date"])
```

Bu dönüşüm sayesinde tarihler üzerinde yıl, ay ve tarih farkı gibi işlemler yapılabilir hale gelmiştir.

### Türetilen değişkenler

```python
df_ham["Order Year"] = df_ham["Order Date"].dt.year
df_ham["Order Month"] = df_ham["Order Date"].dt.month
df_ham["Shipping Duration"] = (
    df_ham["Ship Date"] - df_ham["Order Date"]
)
```

Böylece veri setine:

- `Order Year`
- `Order Month`
- `Shipping Duration`

olmak üzere **3 yeni değişken** eklenmiştir.

---

# 📈 4. Sayısal Değişkenlerin İncelenmesi

`Sales`, `Quantity`, `Discount` ve `Profit` değişkenleri `.describe()` ile incelenmiştir.

| Değişken | Ortalama | Minimum | Maksimum |
|---|---:|---:|---:|
| Sales | 229.858 | 0.444 | 22,638.480 |
| Quantity | 3.790 | 1 | 14 |
| Discount | 0.156 | 0 | 0.8 |
| Profit | 28.657 | -6,599.978 | 8,399.976 |

Ayrıca iş kurallarına dayalı kontroller yapılmıştır:

- Negatif `Sales`
- Sıfır veya negatif `Quantity`
- 0'dan küçük `Discount`
- 1'den büyük `Discount`

Bu kontrollerin tamamında **0 hatalı kayıt** bulunmuştur.

---

# 📊 5. Aykırı Değer Analizi — IQR

`Sales` ve `Profit` değişkenlerindeki aykırı değerleri belirlemek için **IQR (Interquartile Range)** yöntemi kullanılmıştır.

Formül:

```text
IQR = Q3 - Q1

Alt Sınır = Q1 - 1.5 × IQR
Üst Sınır = Q3 + 1.5 × IQR
```

### Hesaplanan sınırlar

| Değişken | Alt Sınır | Üst Sınır |
|---|---:|---:|
| Sales | -271.710 | 498.930 |
| Profit | -39.724 | 70.817 |

### Tespit edilen aykırı değerler

- `Sales`: **1.167 kayıt**
- `Profit`: **1.881 kayıt**

Burada önemli bir tercih yapılmıştır:

> Aykırı değerler otomatik olarak silinmemiş, **bayraklanmıştır**.

Çünkü yüksek satış veya yüksek kâr değerleri mutlaka veri hatası anlamına gelmez. Örneğin veri setinde yüksek satış değerine sahip teknoloji ürünleri bulunmaktadır.

---

# 🏷️ 6. Aykırı Değerleri Bayraklama

Aykırı kayıtları kaybetmemek için iki Boolean sütun oluşturulmuştur:

```python
df_ham["Sales_Aykiri"] = (
    (df_ham["Sales"] < alt_sinir["Sales"]) |
    (df_ham["Sales"] > ust_sinir["Sales"])
)

df_ham["Profit_Aykiri"] = (
    (df_ham["Profit"] < alt_sinir["Profit"]) |
    (df_ham["Profit"] > ust_sinir["Profit"])
)
```

Böylece her kayıt için:

- `Sales_Aykiri = True / False`
- `Profit_Aykiri = True / False`

şeklinde kontrol edilebilir bir yapı oluşturulmuştur.

---

# 🔁 7. Kriter Bazlı Tekrar Kontrolü

Tamamen aynı satırların dışında, daha anlamlı bir iş kuralı üzerinden de tekrar kontrolü yapılmıştır.

Kontrol kriteri:

```text
Customer ID + Order Date + Product ID
```

Bu kriter altında **8 tekrar eden kayıt** tespit edilmiştir.

Bu kayıtlar otomatik olarak silinmemiştir; çünkü aynı müşterinin aynı tarihte aynı ürünü birden fazla kez sipariş etmesi iş açısından mümkün olabilir.

---

# 🏷️ 8. Kategori Tutarlılığı

Kategorik değişkenlerin benzersiz değerleri incelenmiştir.

Kontrol edilen alanlar:

- `Category`
- `Sub-Category`
- `Region`
- `Segment`
- `Ship Mode`

Örneğin `Category` içerisinde:

```text
Furniture
Office Supplies
Technology
```

değerleri bulunmaktadır.

Bu kontrol, yazım veya kategori tutarsızlıklarının fark edilmesini sağlamak amacıyla yapılmıştır.

---

# 🧹 9. Metin Temizliği

Metin sütunlarında başta veya sonda gereksiz boşluk olup olmadığı kontrol edilmiştir.

Kontrol sonucunda:

> `Product Name` sütununda **16 satırda** başta/sonda boşluk problemi bulunmuştur.

Düzeltme:

```python
df_ham["Product Name"] = df_ham["Product Name"].str.strip()
```

Düzeltme sonrasında kalan problem:

**0 kayıt**

---

# 🔢 10. İndeks Düzenleme

Temizleme ve dönüşüm işlemlerinin ardından DataFrame indeksleri yeniden düzenlenmiştir.

```python
df_ham = df_ham.reset_index(drop=True)
```

Son indeks aralığı:

```text
0 - 9993
```

---

# 💾 11. Temizlenmiş Verinin Kaydedilmesi

İşlemler tamamlandıktan sonra veri:

```python
df_ham.to_csv("Superstore_temiz.csv", index=False)
```

komutu ile kaydedilmiştir.

### Son Veri Yapısı

| Özellik | İlk Durum | Son Durum |
|---|---:|---:|
| Satır | 9.994 | **9.994** |
| Sütun | 21 | **26** |
| Eksik değer | 0 | **0** |
| Tam duplicate | 0 | **0** |
| Tarih tipi | `object` | `datetime64[ns]` |
| Yeni değişken | — | **5** |

Son veri setinde:

- `Order Year`
- `Order Month`
- `Shipping Duration`
- `Sales_Aykiri`
- `Profit_Aykiri`

olmak üzere **5 yeni sütun** bulunmaktadır.

---

# ⚙️ 12. Uçtan Uca Temizleme Fonksiyonu

Çalışmanın tekrar kullanılabilir olması için işlemler tek bir fonksiyonda birleştirilmiştir:

```python
def veriyi_temizle(df_ham_girdi):
    df = df_ham_girdi.copy()

    df["Order Date"] = pd.to_datetime(df["Order Date"])
    df["Ship Date"] = pd.to_datetime(df["Ship Date"])

    df["Order Year"] = df["Order Date"].dt.year
    df["Order Month"] = df["Order Date"].dt.month
    df["Shipping Duration"] = (
        df["Ship Date"] - df["Order Date"]
    )

    df["Product Name"] = df["Product Name"].str.strip()

    Q1 = df[["Sales", "Profit"]].quantile(0.25)
    Q3 = df[["Sales", "Profit"]].quantile(0.75)

    IQR = Q3 - Q1

    alt = Q1 - 1.5 * IQR
    ust = Q3 + 1.5 * IQR

    df["Sales_Aykiri"] = (
        (df["Sales"] < alt["Sales"]) |
        (df["Sales"] > ust["Sales"])
    )

    df["Profit_Aykiri"] = (
        (df["Profit"] < alt["Profit"]) |
        (df["Profit"] > ust["Profit"])
    )

    df = df.reset_index(drop=True)

    return df
```

Fonksiyon test edildiğinde çıktı:

**9.994 satır × 26 sütun**

olmuştur.

---

# 🔍 13. Veri Seçme ve Filtreleme

Çalışmanın son bölümünde Pandas ile veri seçme ve filtreleme örnekleri uygulanmıştır.

### Belirli sütunları seçme

```python
df_ham[["Customer Name", "Category", "Sales", "Profit"]]
```

### `.loc` ile seçim

```python
df_ham.loc[
    0,
    ["Customer Name", "Category", "Sales", "Profit"]
]
```

### Kategoriye göre filtreleme

```python
df_ham[df_ham["Category"] == "Furniture"]
```

### Birden fazla koşulla filtreleme

Kârı negatif olan ve `Furniture` kategorisinde bulunan ürünler:

```python
df_ham[
    (df_ham["Profit"] < 0) &
    (df_ham["Category"] == "Furniture")
][
    ["Product Name", "Sales", "Discount", "Profit"]
]
```

Bu bölüm, temizlenmiş verinin analiz için nasıl kullanılabileceğini göstermektedir.

---

# 🧰 Kullanılan Teknolojiler

| Teknoloji | Kullanım |
|---|---|
| 🐍 **Python** | Veri işleme |
| 🐼 **Pandas** | Veri temizleme ve analiz |
| 🔢 **NumPy** | Sayısal veri işlemleri |
| ☁️ **Google Colab** | Notebook geliştirme ortamı |
| 📄 **CSV** | Veri saklama formatı |
| 📓 **Jupyter Notebook** | Çalışma formatı |

---

# 📁 Proje Yapısı

```text
📦 superstore-data-cleaning
│
├── 📓 pandas_veri_temizleme.ipynb
├── 📄 Superstore.csv
├── 📄 Superstore_temiz.csv
└── 📖 README.md
```

---

# 🎯 Projenin Kazanımları

Bu çalışma sonucunda:

- Pandas ile CSV veri okuma
- Veri setinin yapısını inceleme
- Eksik veri ve duplicate kontrolü
- Veri tipi dönüşümü
- Tarih verileriyle çalışma
- Yeni değişken türetme
- IQR ile aykırı değer tespiti
- Aykırı değerleri silmeden bayraklama
- Kategorik veri tutarlılığı kontrolü
- Metin temizleme
- DataFrame indeks yönetimi
- Filtreleme ve veri seçme
- Temizlenmiş veriyi CSV olarak dışa aktarma
- Tekrarlanabilir veri temizleme fonksiyonu oluşturma

uygulanmıştır.

---

## 📌 Özet

Bu proje, ham bir satış veri setinin doğrudan analiz edilmesi yerine önce **veri kalitesinin kontrol edilmesi ve analize hazırlanması** yaklaşımını göstermektedir.

Özellikle aykırı değerlerin doğrudan silinmesi yerine `Sales_Aykiri` ve `Profit_Aykiri` sütunlarıyla **izlenebilir hale getirilmesi**, veri kaybını önleyen ve sonraki analizlerde esneklik sağlayan bir yaklaşım olarak kullanılmıştır.

> **Ham Veri → Veri Kalitesi → Temizleme → Özellik Türetme → Aykırı Değer Analizi → Filtreleme → Temiz Veri**

---

### 👩‍💻 Proje

**Superstore Veri Temizleme ve Analiz**

Python · Pandas · NumPy · Google Colab


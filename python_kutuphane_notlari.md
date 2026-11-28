# NumPy Çalışma Notlarım

## 1 Temel Tanım ve Kullanım Amacı

**NumPy Nedir**
NumPy benim Python tarafında sayısal işlemler ve bilimsel hesaplamalar yaparken elim ayağım olan en temel kütüphane adı Numerical Python kısaltmasından geliyor arka planda saf Python yerine C dili ile yazıldığı için inanılmaz hızlı çalışıyor veri bilimi yapay zeka ya da otonom sistemler üzerine çalışırken öğrenilen ve her yerde karşıma çıkan ilk yapı

**Hangi Problemleri Çözer**
Normalde Python listeleri esnek ama işin içine milyonlarca veri girince acayip yavaş kalıyor çünkü listedeki her bir elemanı bellekte ayrı bir nesne olarak tutuyor bir de iki listeyi toplamak ya da çarpmak istediğimde mecbur for döngüsü kurmak gerekiyor büyük verilerde gereksiz iş yükü oluyor NumPy ise verileri bellekte yan yana bloklar halinde ve tek bir veri tipinde tutarak bu yavaşlığı tamamen çözüyor döngü kurmadan tek satırda matris ve vektör hesaplamaları yapabiliyoruz

**Neden Öğrenmem Gerekiyor**
Otonom araçlar görüntü işleme ya da makine öğrenmesi tarafında uğraşırken elimdeki her şey aslında sayılardan oluşan matrisler mesela aracın kamerasından gelen bir görüntü bile piksellerden oluşan üç boyutlu bir sayı dizisi Pandas OpenCV veya PyTorch gibi kütüphanelerin hepsi arka planda NumPy kullanıyor yani bunun mantığını oturtmadan diğerlerini anlamak da zor 

---

## 2 Temel Fonksiyonlar ve Yapılar

### ndarray Oluşturma
NumPy olayının kalbi ndarray dediğimiz çok boyutlu dizi yapısı normal listelerden farkı içindeki tüm elemanların aynı tipte olması gerekiyor yani aynı dizinin içine hem tam sayı hem metin koyamıyorum

```python
import numpy as np

# tek boyutlu dizi yani vektör oluşturalım
dizi_1d = np.array([10, 20, 30, 40])

# iki boyutlu dizi yani bildiğimiz matris
dizi_2d = np.array([[1, 2, 3], [4, 5, 6]])

# baştan sıfırlarla veya birlerle dolu hazır matrisler
sifirlar = np.zeros((3, 4))
birler = np.ones((2, 3))
```

### reshape ndim size dtype
Elimdeki dizinin neye benzediğini anlamak ve şeklini değiştirmek için en sık kullandığım araçlar bunlar

* **ndim** dizinin kaç boyutlu olduğunu söylüyor düz listeyse 1 matrisse 2 çıktısı veriyor
* **size** içinde toplam kaç tane eleman olduğunu gösteriyor
* **dtype** dizideki sayıların tipini int64 float32 gibi yazdırıyor
* **reshape** eleman sayısını bozmadan dizinin satır ve sütun sayısını değiştiriyor örneğin 2x4 lük matrisi 4x2 yapabiliyorum

```python
arr = np.array([[1, 2, 3, 4], [5, 6, 7, 8]])

print(arr.ndim)   # 2 verir
print(arr.size)   # 8 verir
print(arr.dtype)  # int64 verir

# toplam 8 eleman var o yüzden 4x2 yapabilirim
yeni_arr = arr.reshape(4, 2)
```

### arange ve linspace
Elimle tek tek sayı yazmak yerine belirli kurallara göre otomatik dizi üretmek için bu ikisini kullanıyorum

* **arange** normal Python içindeki range mantığıyla aynı başlangıç bitiş ve artış miktarını veriyorum ama bitiş sayısı diziye dahil olmuyor
* **linspace** başlangıç ve bitiş arasını kaç parçaya bölmek istiyorsam onu yazıyorum burada bitiş değeri de diziye dahil ediliyor özellikle grafik çizerken x ekseni oluşturmak için çok pratik oluyor

```python
# 0 dan başla 10 a kadar ikişer artarak git (10 dahil değil)
a = np.arange(0, 10, 2)  # [0 2 4 6 8]

# 0 ile 1 arasını 5 eşit parçaya böl (1 dahil)
b = np.linspace(0, 1, 5) # [0.   0.25 0.5  0.75 1.  ]
```

### Matematiksel İşlemler
NumPy dizilerinde dört işlem yaparken döngüye hiç gerek kalmıyor iki diziyi topladığımda karşılıklı olarak aynı sıradaki elemanlar işleme giriyor buna vektörizasyon deniyor kod yazarken bana çok zaman kazandırıyor

```python
x = np.array([10, 20, 30])
y = np.array([2, 4, 5])

toplam = x + y    # [12 24 35]
fark = x - y      # [ 8 16 25]
carpim = x * y    # [ 20  80 150]
bolum = x / y     # [5. 5. 6.]
```

### Matrix İşlemleri
Lineer cebirdeki gerçek matris çarpımını yapmak istersem normal yıldız işareti kullanmıyorum çünkü o elemanları tek tek çarpıyor onun yerine dot fonksiyonunu ya da at işaretini kullanıyorum

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

# gerçek matris çarpımı
matris_carpimi = np.dot(A, B) # veya kısaca A @ B

# matrisin devriğini yani transpozunu alma
transpoz = A.T
```

---

## 3 Otonom Araçlarda Kullanım Örnekleri

Otonom araçlar etrafını görmek ve anlamak için kamera LiDAR radar gibi sensörlerden sürekli ham veri çekiyor bu verilerin anlık işlenmesinde NumPy doğrudan devrede oluyor[cite: 2]

* **Kamera Görüntüsü İşleme** aracın ön kamerasından gelen her kare aslında yükseklik genişlik ve RGB renk kanallarından oluşan üç boyutlu bir NumPy dizisi şerit çizgilerini bulurken veya trafik ışıklarını tespit ederken görüntüyü siyah beyaza çevirmek ya da belli renk aralığındaki pikselleri filtrelemek için doğrudan matris işlemleri yapıyorum
* **LiDAR Nokta Bulutu** LiDAR sensörü etrafa lazer atarak x y z koordinatlarından oluşan devasa bir nokta bulutu çıkarıyor bunlar kocaman NumPy matrisleri olarak tutuluyor aracın önündeki engele kaç metre kaldığını bulmak veya koordinat eksenlerini çevirmek için matris çarpımları kullanılıyor
* **Kontrol ve Kinematik Hesaplamalar** aracın o anki hızı direksiyon açısı ve ivmesi gibi değerler vektör olarak saklanıyor aracın bir saniye sonra nerede olacağını hesaplayan fizik modellerinde NumPy dizileriyle çok hızlı lineer cebir çözümleri yapılıyor[cite: 2]

***

# Pandas Çalışma Notlarım

## 1 Temel Tanım ve Kullanım Amacı

**Pandas Nedir**
Pandas benim Python tarafında veri analizi ve veri düzenleme işleri için kullandığım en pratik kütüphane ismi Panel Data teriminden geliyor mantığı tamamen Excel tablolarına ya da SQL veritabanlarına benziyor satırlar ve sütunlar üzerinden verileri çok rahat yönetmemi sağlıyor

**Hangi Problemleri Çözer**
Gerçek hayatta sensörlerden veya dışarıdan aldığım veriler hiçbir zaman tertemiz gelmiyor mutlaka içinde boş hücreler hatalı kayıtlar ya da gereksiz sütunlar oluyor Pandas bu dağınık verileri dosyadan okuyup temizlememi eksik yerleri doldurmamı ve istediğim şartlara göre filtrelememi sağlıyor binlerce satırlık veriyi tek satır kodla gruplayıp özetini çıkarabiliyorum

**Neden Öğrenmem Gerekiyor**
Bir yapay zeka modelini eğitmeden ya da bir sistemin performansını ölçmeden önce elimdeki veriyi adam etmem lazım projelerde vaktimin büyük kısmı zaten bu veri temizleme ve hazırlama işine gidiyor Pandas bu angarya işleri aşırı hızlandırdığı için kesin hakim olmam gerekiyor

---

## 2 Temel Fonksiyonlar ve Yapılar

### DataFrame ve Series Farkı
Pandas kullanırken karşıma çıkan iki ana yapı var bunları birbirine karıştırmamam önemli

* **Series** tek boyutlu ve etiketli bir liste gibi düşünebilirim yani Excel tablosundaki tek bir sütuna denk geliyor hem verileri hem de indeks numaralarını tutuyor
* **DataFrame** iki boyutlu bildiğim satır ve sütunlardan oluşan ana tablo birden fazla Series yan yana gelince DataFrame oluşuyor her sütunda farklı tipte veri tutabiliyorum mesela bir sütun yazı diğeri ondalıklı sayı olabiliyor

```python
import pandas as pd

# tek sütunluk Series örneği
hizlar = pd.Series([50, 65, 80, 45], name="Hiz")

# satır ve sütunlardan oluşan DataFrame örneği
veri = {
    "Arac_ID": [101, 102, 103],
    "Hiz": [60, 75, 40],
    "Durum": ["Normal", "Hizli", "Yavas"]
}
df = pd.DataFrame(veri)
```

### CSV ve Excel Okuma
Bilgisayarımdaki veri dosyalarını Python içine tablo olarak çekmek için read_csv ve read_excel fonksiyonlarını kullanıyorum

```python
# csv dosyasını okuyup tablo yapma
df_csv = pd.read_csv("surus_kayitlari.csv")

# excel dosyasından istediğim sayfayı çekme
df_excel = pd.read_excel("sensor_verileri.xlsx", sheet_name="Sayfa1")
```

### Filtreleme loc ve iloc
Tablonun içinden sadece işime yarayan satır ve sütunları kesip almak için iki farklı yöntem kullanıyorum başlarda biraz karıştırıyordum ama mantığı çok basit

* **loc** isim veya etiket bazlı çalışıyor yani sütunların gerçek adlarını yazarak seçim yapıyorum
* **iloc** ise tamamen sıra numarası yani indeks bazlı çalışıyor sıfırdan başlayan satır ve sütun numaralarını veriyorum

```python
# loc ile sütun isimlerini vererek seçme
secim1 = df.loc[0:2, ["Arac_ID", "Hiz"]]

# iloc ile ilk iki satır ve ilk iki sütunu alma
secim2 = df.iloc[0:2, 0:2]

# hızı 50 den büyük olan araçları filtreleme
hizli_araclar = df[df["Hiz"] > 50]
```

### Kayıp Veri Yönetimi dropna ve fillna
Sensör kopmaları veya kayıt hataları yüzünden tabloda NaN yazan boşluklar çıkıyor bunları temizlemek için iki temel çözümüm var

* **dropna** içinde eksik veri olan satırları hiç uğraşmadan tablodan komple siliyor
* **fillna** boş kalan hücreleri benim verdiğim bir değerle mesela sıfırla ya da o sütunun ortalamasıyla dolduruyor

```python
# boş değer içeren satırları uçurma
temiz_df = df.dropna()

# boş yerleri o sütunun ortalamasıyla doldurma
df["Hiz"] = df["Hiz"].fillna(df["Hiz"].mean())
```

### groupby describe ve sort_values
Veriyi inceleyip anlamlı sonuçlar çıkarmak için en çok kullandığım üç analiz metodu bunlar

* **describe** tablodaki sayısal sütunların ortalamasını minimum maksimum değerlerini ve standart sapmasını tek seferde önüme döküyor
* **groupby** verileri belli bir kategoriye göre gruplayıp her grubun kendi içinde ortalamasını ya da toplamını alıyor
* **sort_values** tabloyu seçtiğim bir sütuna göre küçükten büyüğe ya da büyükten küçüğe sıralıyor

```python
# hızlı istatistiksel özet alma
ozet = df.describe()

# duruma göre gruplayıp ortalama hızları bulma
grup_ort = df.groupby("Durum")["Hiz"].mean()

# hıza göre büyükten küçüğe sıralama
sirali_df = df.sort_values(by="Hiz", ascending=False)
```

---

## 3 Otonom Araçlarda Kullanım Örnekleri

Otonom araçlarla yapılan test sürüşlerinde gigabaytlarca log dosyası birikiyor bu verileri ayıklayıp analiz etmek için tamamen Pandas kullanılıyor[cite: 2]

* **Test Sürüşü Log Analizi** araç test sürüşündeyken her milisaniyedeki hız direksiyon açısı fren basıncı gibi veriler CSV olarak kaydediliyor bu dosyaları Pandas ile açıp aracın nerede ani fren yaptığını veya hangi saniyelerde sensör hatası verdiğini filtreleyerek rahatça bulabiliyorum
* **Sensör Veri Senkronizasyonu ve Temizliği** GPS verisi saniyede bir gelirken kamera saniyede otuz kare çekiyor bu verileri zaman damgalarına göre birleştirirken arada boş kalan satırları fillna ile doldurup veri kaybını önlüyoruz
* **Senaryo Sınıflandırma** aracın yağmurlu havada gece sürüşünde veya yoğun trafikte nasıl performans gösterdiğini kıyaslamak için log kayıtlarını hava durumuna göre groupby ile gruplayıp sürücü müdahale sayılarını karşılaştırabiliyorum[cite: 2]

***

# Matplotlib Çalışma Notlarım

## 1 Temel Tanım ve Kullanım Amacı

**Matplotlib Nedir**
Matplotlib Python tarafında elimdeki sayısal verileri grafiğe dökmek için kullandığım en temel görselleştirme kütüphanesi çizgi grafikleri dağılım grafikleri sütun grafikleri ve histogramlar gibi birçok iki boyutlu grafiği bununla çiziyorum

**Hangi Problemleri Çözer**
Binlerce satırlık sayı yığınlarına bakarak sistemde bir terslik olup olmadığını ya da verilerin nasıl değiştiğini anlamak imkansız gibi bir şey Matplotlib bu karmaşık sayıları gözle görülür şekillere çevirerek verideki trendleri ve hataları tek bakışta fark etmemi sağlıyor

**Neden Öğrenmem Gerekiyor**
Yazdığım algoritmanın gerçekten doğru çalışıp çalışmadığını test etmek ya da yaptığım analizi rapora koyup başkalarına anlatmak için grafik çizmek şart ayrıca Seaborn gibi daha süslü görselleştirme kütüphaneleri de altyapı olarak Matplotlib kullanıyor o yüzden bunun mantığını kapmak her yerde işimi görüyor

---

## 2 Temel Fonksiyonlar ve Yapılar

### Temel Grafik Türleri plot scatter bar hist
Elimdeki verinin tipine ve neyi görmek istediğime göre dört ana grafik fonksiyonu kullanıyorum

* **plot** noktaları düz çizgiyle birleştiriyor özellikle zamana bağlı değişimleri görmek için birebir
* **scatter** verileri çizgi çekmeden sadece noktalar halinde gösteriyor iki değişken arasında bir ilişki veya kümelenme var mı diye bakarken kullanıyorum
* **bar** kategorik verileri birbiriyle kıyaslamak için dikey veya yatay sütunlar çiziyor
* **hist** tek bir sayısal verinin hangi aralıklarda daha çok yoğunlaştığını yani dağılımını gösteriyor

```python
import matplotlib.pyplot as plt

zaman = [1, 2, 3, 4, 5]
hiz = [10, 25, 40, 35, 50]

# klasik çizgi grafiği
plt.plot(zaman, hiz)
plt.show()

# noktalı dağılım grafiği
plt.scatter(zaman, hiz)
plt.show()

# karşılaştırma için sütun grafiği
kategoriler = ["Sehir Ici", "Otoyol"]
ort_hiz = [45, 110]
plt.bar(kategoriler, ort_hiz)
plt.show()

# yoğunluk görmek için histogram
olcumler = [22, 25, 24, 30, 35, 22, 28, 29, 31]
plt.hist(olcumler, bins=5)
plt.show()
```

### Figure ve Axes Mantığı
Daha profesyonel ve düzenli grafikler çizerken nesne yönelimli yapı olan Figure ve Axes ikilisini kullanıyorum başta biraz kafa karıştırıcı gelse de kontrol tamamen bende oluyor

* **Figure** üzerine çizim yaptığım boş tuvalin veya pencerenin tamamı içinde birden fazla grafik barındırabiliyor
* **Axes** ise o tuvalin içindeki asıl grafik alanı x ve y eksenleri çizgiler başlıklar hepsi bu Axes nesnesinin üzerine çiziliyor

```python
# 1 adet tuval ve 1 adet grafik alanı oluşturalım
fig, ax = plt.subplots(figsize=(8, 4))

# çizimi doğrudan ax üzerine yapıyorum
ax.plot(zaman, hiz, color="blue", linestyle="--")
```

### Grafik Başlığı Label ve Grid Ekleme
Çizdiğim grafiğe bakan kişinin ne olduğunu anlaması için eksen isimlerini başlığı ve arka plan ızgaralarını mutlaka eklemem gerekiyor

```python
fig, ax = plt.subplots()
ax.plot(zaman, hiz, label="Arac Hizi")

# grafiğin ana başlığı
ax.set_title("Zamana Bagli Hiz Degisimi")

# x ve y eksenlerinin ne olduğu
ax.set_xlabel("Zaman (saniye)")
ax.set_ylabel("Hiz (km/saat)")

# okumayı kolaylaştırmak için arka plana ızgara ekleme
ax.grid(True)

# çizginin neyi temsil ettiğini gösteren kutucuk
ax.legend()
plt.show()
```

### Çoklu Grafik Mantığı subplot
Farklı verileri aynı ekranda yan yana kıyaslamak istediğimde tek bir Figure penceresini satır ve sütunlara bölüp içine birden fazla grafik yerleştiriyorum

```python
# 1 satır 2 sütundan oluşan yan yana iki grafik alanı
fig, axes = plt.subplots(1, 2, figsize=(10, 4))

# soldaki birinci grafik
axes[0].plot(zaman, hiz)
axes[0].set_title("Hiz Grafigi")

# sağdaki ikinci grafik
ivme = [0, 15, 15, -5, 15]
axes[1].bar(zaman, ivme)
axes[1].set_title("Ivme Grafigi")

# yazılar birbirine girmesin diye otomatik hizalama
plt.tight_layout()
plt.show()
```

---

## 3 Otonom Araçlarda Kullanım Örnekleri

Otonom araç geliştirirken yazdığım algoritmaların sahada nasıl davrandığını görmek ve hataları ayıklamak için Matplotlib sürekli elimin altında duruyor[cite: 2]

* **Rota ve Konum Takibi** aracın haritada gitmesi gereken ideal çizgi ile GPS üzerinden gelen gerçek konum noktalarını aynı grafikte üst üste çizdiriyorum böylece aracın keskin virajlarda şeritten ne kadar taştığını gözle net bir şekilde görebiliyorum
* **PID Kontrolcü Ayarları** aracın belirlediğim hedef hıza kaç saniyede çıktığını ve o hızda sabit kalıp kalamadığını görmek için hedef hız ve anlık hızı aynı grafiğe döküyorum araç çok sallanıyorsa bu grafiğe bakıp kontrol parametrelerine ince ayar yapıyorum
* **Sensör Gürültü Analizi** radar veya mesafe sensörlerinden gelen ölçümlerin dağılımını histogram ile çizdirerek sensörün ne kadar sapma yaptığını ve uyguladığım filtre algoritmasının bu gürültüyü ne kadar temizlediğini kıyaslıyorum[cite: 2]


# LiDAR ve Otonom Araçlar Notlarım

## 1. LiDAR Nedir Nasıl Çalışır

* Açılımı Light Detection and Ranging (Işık Tespiti ve Mesafe Tayini) kısaca radarın radyo dalgalarıyla yaptığını görünmez lazer ışınlarıyla yapıyor
* Yarasaların ses dalgalarıyla yönünü bulması gibi düşün ama ses yerine ışık hızında lazer kullanıyor
* **Çalışma mantığı (ToF - Time of Flight)**
  * Sensör çevreye saniyede yüz binlerce hatta milyonlarca kızılötesi lazer darbesi gönderiyor (905 nm veya 1550 nm dalga boyunda yani insan gözüne zarar vermiyor)
  * Gönderilen bu ışınlar etraftaki arabalara yayalara binalara ya da asfalta çarpıp geri dönüyor
  * Işık hızı ($c$) sabit olduğu için lazerin gidiş geliş süresinden ($t$) hedef noktanın mesafesi santimetre hassasiyetinde hesaplanıyor ($\text{Mesafe} = \frac{c \times t}{2}$)
  * Toplanan milyonlarca mesafe verisi birleşince aracın etrafının gerçek zamanlı **3 boyutlu (3D) haritası** ortaya çıkıyor

---

## 2. Otonom Araçta Ne Görev Yapar

Aracın kendi kendine güvenle gidebilmesi için etrafında ne olup bittiğini tam anlaması lazım LiDAR aracın **3 boyutlu gören gözü** gibi çalışıyor ve şu görevleri üstleniyor

* **Haritalama ve Konumlandırma (SLAM)** Araç hareket ederken anlık 3D çevre haritası çıkarıyor ve bunu sistemdeki yüksek çözünürlüklü (HD) haritalarla eşleştiriyor tünele girince ya da yüksek binaların arasında GPS sinyali kesilse bile aracın şeritteki tam yerini santimi santimine buluyor
* **Nesne Tanıma ve Sınıflandırma** Sadece önde bir engel var deyip geçmiyor nokta yoğunluğu sayesinde cismin 3D silüetini çıkarıyor böylece öndekinin yaya mı bisikletli mi otomobil mi kamyon mu yoksa yola düşmüş bir duba mı olduğunu net ayırt ediyor
* **Hız ve Rota Hesaplama** Saniyenin binde biri hızında arka arkaya aldığı taramaları kıyaslıyor etraftaki araçların ve yayaların hangi yöne kaç km/s hızla gittiğini ve olası çarpışma rotalarını hesaplıyor
* **Yol Sınırları ve Zemin Analizi** Kaldırım taşlarını şerit eğimlerini hız kesici kasisleri çukurları ve yoldaki küçük engelleri yükseklik farklarından yakalıyor
* **Zifiri Karanlıkta Bile Görme** Kameralar gece sürüşünde ya da tünel çıkışındaki ani güneş parlamasında zorlanırken LiDAR kendi ışık kaynağını (lazerini) ürettiği için gece gündüz fark etmeksizin aynı netlikte çalışıyor *(Not yoğun sis şiddetli yağmur ve karda su damlaları lazer ışığını saçabildiği için performansı düşüyor o noktada devreye radar giriyor)*

---

## 3. Hangi Tip Veriyi Kullanır

* Kameradan gelen görüntüler düzenli piksel karelerinden oluşur ama LiDAR bize **3D Nokta Bulutu (Point Cloud)** denen uzayda serbest duran milyonlarca noktadan oluşan yapılandırılmamış bir veri üretiyor
* Otonom sürüş yazılımlarında (örneğin ROS) bu veriler genelde `.pcd` `.bin` veya `.las` formatlarında işleniyor
* **Her bir noktanın içinde 4 temel bilgi var $(X, Y, Z, I)$**
  * **$X, Y, Z$ (Uzamsal Koordinatlar)** Sensörü merkez $(0,0,0)$ kabul ederek noktanın araca göre sağ sol enlem ön arka boylam ve yerden yükseklik değerlerini tutar
  * **$I$ (Intensity / Yansıma Yoğunluğu)** Lazer ışınının çarptığı yüzeyden ne kadar parlak geri döndüğünü gösterir asfalt ışığı emer ve düşük değer verir ama beyaz şerit çizgileri trafik levhaları ve araç plakaları ışığı çok güçlü yansıttığı için yüksek değer verir yani sistem şerit çizgilerini buradan bile okuyabilir
  * Ek olarak her noktada hangi milisaniyede alındığı (timestamp) ve sensördeki hangi lazer kanalından geldiği bilgisi de tutulur

---

## 4. LiDAR ile Engel Tespiti Nasıl Yapılır (5 Adım)

Yazılım saniyede 10-20 kez (10-20 Hz) gelen bu milyonlarca dağınık noktayı şu sırayla işleyip engelleri buluyor

1. **Ön İşleme ve Filtreleme (Preprocessing)**
   Havadaki tozdan yağmur damlalarından kaynaklanan sahte noktalar menzil dışı gürültüler ve aracın kendi kaportasına çarpan gereksiz veriler temizleniyor işlem hızını artırmak için nokta sayısı optimize ediliyor (voxel downsampling)
2. **Zemin Ayrıştırma (Ground Segmentation)**
   Engelleri bulmanın en hızlı yolu önce engel olmayan yolu haritadan çıkarmak algoritma (örneğin RANSAC) düzlem uydurma yöntemiyle dümdüz olan asfalt zemini tespit edip haritadan siliyor zemin gidince geriye sadece yolun üstünde duran engeller arabalar yayalar dubalar ve bariyerler kalıyor
3. **Kümeleme (Clustering)**
   Zemin silindikten sonra havada asılı kalan bu noktalar birbirine yakınlıklarına göre gruplanıyor (DBSCAN veya Euclidean Clustering algoritmalarıyla) yan yana belirli bir mesafeden yakın duran noktalar birleştirilip burada tek bir cisim adayı var deniyor
4. **Sınıflandırma ve 3D Kutulama (3D Bounding Box)**
   Devreye nokta bulutunu doğrudan işleyen derin öğrenme modelleri giriyor (**PointNet** **PointPillars** **VoxelNet**) yapay zeka nokta grubunun geometrisine ve yoğunluğuna bakıp bu yaya bu otomobil bu bisiklet diye sınıflandırıyor ve etrafına yönelim açısını da gösteren **3 boyutlu bir kutu** çiziyor
5. **Nesne Takibi (Object Tracking)**
   Oluşan 3D kutular bir önceki karedeki yerleriyle eşleştiriliyor (Kalman Filtresi kullanılıyor) böylece engelin sadece nerede olduğu değil hangi yöne ne hızla hareket ettiği ve çarpışma ihtimali hesaplanıp fren ya da direksiyon kararı veriliyor

---

## Aklımda Kalacak Özet (Sensör Füzyonu)

Hiçbir sensör tek başına kusursuz değil bu yüzden otonom araçlar üçünü birleştirip kullanıyor (**Sensör Füzyonu**)

* **Kamera** Renk görür trafik ışığı ve tabela okur ama 3D derinlik tahmini zayıftır karanlıkta kör olur
* **Radar** Kötü havada (yağmur sis kar) harikadır hızı çok iyi ölçer ama cismin şeklinin ne olduğunu net çizemez
* **LiDAR** 3D şekil mesafe ölçümü ve gece görüşünde rakipsizdir tek eksiği yoğun yağışta ışığın saçılması ve maliyetinin yüksek olmasıdır (yeni nesil Solid-State LiDAR ile maliyeti düşüyor)

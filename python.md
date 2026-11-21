# 1. Hafta: Python Giriş Notları


## 2. Python Temelleri

### 2.1 Değişkenler ve Veri Tipleri
- **`int`:** Tam sayılar 
- **`float`:** Ondalıklı sayılar
- **`string` (`str`):** Yazılar (tırnak içinde yazılır)
- **`bool`:** Sadece `True` veya `False` değerini alır ve true false baş harfi büyük olmalı 
- **`type()`:** İçine yazılan şeyin hangi veri tipinde olduğunu söyler
  - `input()`: Kullanıcıdan klavyeden bilgi alır ama ne yazarsak yazalım metin (`str`) olarak alıyor sayı lazımsa `int(input())` diye çevirmek gerekiyor

### 2. Operatörler
- **Matematik:** `+`, `-`, `*`, `/` bildiğimiz işlemler. `%` ise bölümden kalanı verir (tek-çift sayı bulurken işe yarıyor)
- **Karşılaştırma:** `==` eşit mi diye bakar (tek = atama yapar). `!=` eşit değil mi demek
- **Mantıksal:** `and` (ikisi de doğruysa) `or` (biri doğruysa yeter) `not` (tersine çevirir)

### 3. Koşul Yapıları (if - elif - else)
- Programa karar verdirmek için kullanılır İki nokta `:` koyup alt satırda Tab ile içeriden yazılır
- if: ilk şart doğruysa çalışır
- eilf: İlk şart yanlışsa başka şart kontrolü
- else: hepsi yanlışsa burası çalışır
- iç içe koşul(nested if): bir şart doğruysa ikincisi kontrol edilir (ana kapıyı açtıktan sonra oda kapısı açmak gibi)

### 4. Fonksiyonlar (`def`)
Aynı kodu tekrar tekrar yazmamak için kendi komutumuzu oluştururuz. Parametre ile dışarıdan bilgi alıyor `return` ile sonucu geri veriyor. Kodu parçalara böldüğü için okuması kolay oluyor
```python
 def on_fazlası(sayi):
     sonuc=sayi+10
     return sonuc
 cevap=on_fazlası(25)
 print(cevap)
```
 ekranda 35 görünür

### 5. Döngüler (for - while)
- `for`: Elimdeki listedeki her eleman için sırayla döner (sayı belli olmalı)
- `while`: Şart doğru olduğu sürece durmadan döner
- `break`: Döngüyü direkt kırıp bitirir
- `continue`: O turu pas geçer sonrakinden devam eder


### 6. Liste, Tuple, Dict, Set
- **List (`[]`):** Sıralı liste sonradan `.append()` ile eleman ekleyip `.remove()` ile silebiliyorum
- **Tuple (`()`):** Liste gibi ama sonradan içindekiler değiştirilemiyor
- **Dict (`{}`):** Sözlük gibi `anahtar: değer` şeklinde tutuyor (`{"ad": "Elif", "no": 1}`)
- **Set (`{}`):** Sırasız küme aynı elemandan iki tane koymaya izin vermiyor

### 7. Sınıflar (`class`) ve OOP
Gerçek hayattaki şeyleri (mesela banka hesabı veya kullanıcı) koda dökme mantığı
- `class` ile şablonu çiziyorum.
- `__init__` nesne ilk oluştuğunda otomatik çalışan kurucu fonksiyon.
- `self` ise o nesnenin kendisini temsil ediyor.

- (class: şablon kimlik kartı)
- ( __init__ : kimliği dolduran makine)
- (self: kişiselleştirilmiş kart)



 ### .AGENTS KLASÖRÜ
 Antigravity IDE içinde çalışan yapay zeka ajanlarının ayarlarının bulunduğu klasöre diyoruz. Kuralları sabitler. Bir kuralı bi kere söylersek hep uygulanmasını sağlar. Ayrıca projede kod yazan ajanların kurallarını belirler.
 NOT: Başında "." olması ana kod dosyalarıyla karışmayacak gizli ve ayar için kullanılan bir klasör olduğunu gösterir
 

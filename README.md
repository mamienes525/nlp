# nlp
#  Project Gutenberg - Chapter Similarity Analysis

Bu proje, **Project Gutenberg** kitaplarını kullanarak kitapların genel açıklamaları ile bölümleri arasındaki içerik benzerliğini analiz eder. Amaç, her bölümün kitap konusuyla ne kadar örtüştüğünü ölçmektir. Proje dört temel aşamadan oluşur:

1. Veri Toplama ve Temizleme
2. Ön İşleme (Preprocessing)
3. Zipf Kanunu Görselleştirmesi
4. Vektörleştirme (TF-IDF ve Word2Vec)

---

##  Proje Dosya Yapısı ve Açıklamaları

### 1. `veri_hazirlik.py`

**Amaç:**  
Project Gutenberg'den indirilen kitapları işler, chapter bazında bölerek `gutenberg_parsed_v1.json` dosyasını oluşturur.

**Yapılan İşlemler:**
- Kitap isimlerini ilk satırdan otomatik olarak tanımlar ve yeniden adlandırır.
- Project Gutenberg'in sonundaki lisans/metinleri temizler.
- Chapter başlıklarına göre metni parçalara ayırır.
- Sahte chapter'ları eleyerek yalnızca anlamlı bölümleri saklar.
- Yalnızca İngilizce kitaplar işlenir.
- Eğer `gutenberg_parsed_v1.json` dosyası mevcutsa işlem atlanır.




---

### 2. `on_islem_ve_zipf.py`

**Amaç:**  
Kitapların bölümlerini içeren JSON dosyasından verileri alır, ön işlem uygular (lemmatization + stemming), ardından sonuçları CSV dosyasına yazar ve Zipf yasasına göre grafik oluşturur.

**Yapılan İşlemler:**
- Küçük harfe dönüştürme
- Noktalama ve özel karakter temizliği
- Stop word (gereksiz sözcük) kaldırma
- Lemmatization (kelime kökünü bulma)
- Stemming (türevleri indirgeme)
- Zipf kanunu grafikleri üretimi



> Not: CSV dosyaları mevcutsa veriler yeniden işlenmez, ancak **Zipf grafikleri her durumda** oluşturulur.

---

### 3. `vektorlestirme.py`

**Amaç:**  
Temizlenmiş CSV dosyaları üzerinden TF-IDF ve Word2Vec modelleri üretir. Belgeler arası benzerlik ölçümleri ve kelime uzaylarında ilişki analizleri yapılabilir.

#### A. TF-IDF

- `lemmatized_data.csv` ve `stemmed_data.csv` ayrı ayrı vektörleştirilir.
- Eğer `.csv` dosyası mevcutsa yeniden hesaplama yapılmaz.


#### B. Word2Vec

- Gensim ile toplam **16 model** eğitilir:
  - 2 tür veri (lemmatized + stemmed)
  - 2 model tipi (CBOW + Skip-gram)
  - 2 pencere boyutu (2 + 4)
  - 2 vektör boyutu (100 + 300)
- Eğitim tamamlandığında her model `.model` olarak kaydedilir.
- Her model için `"time"` kelimesine en benzer 5 kelime yazdırılır.


> Model zaten mevcutsa yeniden eğitilmez.

---

##  Çalıştırma Sırası

1. **`veri_hazirlik.py`**  
   Kitapları işler ve bölümlere ayırır.

2. **`on_islem_ve_zipf.py`**  
   Bölümleri temizler ve Zipf görselleştirmeleri üretir.

3. **`vektorlestirme.py`**  
   TF-IDF + Word2Vec modellerini oluşturur.

---

## 📌 Ek Bilgiler

- Zipf yasasına göre, kelimelerin sıklığı ile sıralamaları log-log düzlemde doğrusal bir yapı sergiler. Bu grafikler, metnin doğal dil özelliklerini görselleştirmek için kullanılır.
- TF-IDF vektörleri, her bölümün içeriğini anlamaya ve konu bütünlüğünü analiz etmeye yardımcı olur.
- Word2Vec ile elde edilen kelime uzayı sayesinde, kitap bölümleri ve anahtar kelimeler arasındaki semantik benzerlikler analiz edilebilir.

---

##  Proje Amacı

> Her bir kitap bölümünün, kitabın genel açıklamasıyla ne kadar örtüştüğünü otomatik olarak analiz etmek .

---

Hazırlayan: **[Muhammet Enes OCAK]**
Gümüşhane Üniversitesi, Yazılım Mühendisliği
No:2107231060

Proje: Doğal Dil İşleme Ödevi  
Konu: Kitap Konusu ile Bölüm Eşleşmesi  
Kaynak: [Project Gutenberg](https://www.gutenberg.org/)
aa
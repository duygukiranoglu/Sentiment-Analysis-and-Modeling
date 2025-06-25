# 🧠 Amazon Ürün Yorumları Üzerinden Duygu Analizi ve Modelleme Çalışması
![wordcloud](https://github.com/user-attachments/assets/c05537ac-9953-4316-8147-bbab5c9476f9/e.png)

Bu projede, Amazon platformunda yer alan bir ürüne (**ASIN: B007WTAJTO**) ait kullanıcı yorumları temel alınarak **duygu analizi (sentiment analysis)** gerçekleştirilmiştir.  
Amaç, kullanıcı yorumlarının **olumlu (positive)** veya **olumsuz (negative)** olarak **otomatik şekilde sınıflandırılmasıdır**.

---

## 📌 Proje Akışı

Analiz süreci, **iki aşamalı bir makine öğrenmesi yaklaşımı** ile yürütülmüştür:

---

### 🔹 1. Aşama – Unsupervised Duygu Etiketleme (VADER)

İlk aşamada, elimizde etiketli veri olmadan **sözlük temelli duygu analiz aracı olan VADER** kullanılmıştır.

- Her bir yorum için **compound skorları** hesaplanmıştır.
- Skorlar 0.05'ten büyükse **"Positive"**, diğer durumlarda **"Negative"** olarak etiketlenmiştir.
- Böylece, elimizde **pseudo-label (sözde etiket)** oluşturulmuş ve veri, denetimli öğrenmeye uygun hale getirilmiştir.

---

### 🔹 2. Aşama – Supervised Makine Öğrenmesi ile Sınıflandırma

İkinci aşamada, ilk adımda oluşturulan bu sözde etiketler kullanılarak **denetimli makine öğrenmesi** yöntemleri uygulanmıştır.

#### 🔧 1. Veri Ön İşleme (Preprocessing)
Yorumlar doğal dil işleme teknikleriyle temizlenmiştir:
- Noktalama işaretleri ve rakamlar kaldırıldı  
- Küçük harfe dönüştürüldü  
- Tokenizasyon (kelimelere ayırma)  
- Stopword (önemsiz kelime) temizliği  
- Lemmatization (kelimeleri köküne indirgeme)

#### 🔍 2. Metni Sayısal Vektörlere Dönüştürme
Yorumlar, makine öğrenmesi algoritmalarının anlayabileceği **sayısal temsillere** dönüştürülmüştür:

- **CountVectorizer:** Kelime sıklığına dayalı temsil
- **TF-IDF:**  
   - Kelime düzeyinde (unigram)  
   - N-gram düzeyinde (bigram ve trigram)

#### 🤖 3. Modelleme ve Performans Karşılaştırması
- Her bir sayısal temsil için **Random Forest** modeli kurulmuştur.
- 5 katlı **çapraz doğrulama (cross-validation)** ile ortalama başarı skorları hesaplanmıştır.
- Hedef: **En yüksek performansı sağlayan vektörleştirme yöntemini belirlemek** ve final modellemede onu kullanmak.

---

## ✅ Sonuçlar ve Değerlendirme

- **En yüksek ortalama F1 skoru**, **CountVectorizer** yöntemi ile elde edilmiştir.
- Final model, bu nedenle **CountVectorizer** temsili ile eğitilmiştir.

### 📈 Model Performansı Özeti:
| Sınıf         | Precision | Recall | F1-Score |
|---------------|-----------|--------|----------|
| Pozitif (1)   | 0.85      | 0.97   | **0.90** |
| Negatif (0)   | 0.73      | 0.33   | 0.46     |

- Model, **pozitif yorumları başarılı şekilde sınıflandırmakta** (%90 F1).
- Ancak, **negatif yorumlarda recall değeri düşüktür** (%33).
  
### 🔄 Öneriler:
- **Veri dengesizliği** için:
  - **SMOTE gibi veri dengeleme teknikleri**
  - **Class weight ayarıyla eğitim**
  - **Daha fazla negatif örnek toplama**
- Alternatif modeller ve vektörleştirme stratejileri denenebilir.

---

## 🔍 Kullanılan Veri Seti

- **Kaynak:** [amazon_review.csv](https://www.kaggle.com/datasets/uurdndr/amazon-rating-review)  
- **İncelenen Ürün:** `ASIN: B007WTAJTO`  

### 📄 Veri Seti Özellikleri:

| Değişken Adı      | Açıklama |
|-------------------|----------|
| `reviewerID`      | Kullanıcı ID’si |
| `reviewerName`    | Kullanıcı adı |
| `asin`            | Ürün ID’si |
| `reviewText`      | Değerlendirme metni (ana analiz verisi) |
| `summary`         | Değerlendirme özeti |
| `overall`         | Ürüne verilen puan (rating) |
| `reviewTime`      | Değerlendirme zamanı (ham tarih formatı) |
| `unixReviewTime`  | Değerlendirme zamanı (Unix timestamp formatı) |
| `helpful`         | Faydalı değerlendirme oranı (örneğin `[3,2]`) |
| `helpful_yes`     | Değerlendirmenin faydalı bulunma sayısı |
| `total_vote`      | Değerlendirmeye verilen toplam oy sayısı |
| `day_diff`        | Yorumu yazan kişinin ürünü aldıktan sonra geçen gün sayısı |

> 📌 Bu projede analiz için özellikle `reviewText` sütunu temel alınmıştır.


---

## 🛠️ Kullanılan Araçlar ve Kütüphaneler

- `Python`
- `Pandas`, `NumPy`, `Matplotlib`
- `NLTK`, `TextBlob`, `WordCloud`
- `scikit-learn`: CountVectorizer, TfidfVectorizer, RandomForestClassifier
- `VADER` (SentimentIntensityAnalyzer)

---

Proje ile ilgili geri bildirimleriniz ve önerileriniz için iletişime geçebilirsiniz.  
Teşekkürler! 🙌


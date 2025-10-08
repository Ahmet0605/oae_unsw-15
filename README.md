**ÖZET**

1. Bu çalışmada, ağ güvenliğinde artan saldırı karmaşıklığına karşı Unsupervised ML Algorithms ve Ensemble temelli bir ağ anomali tespit yöntemi önerilmiştir.

2. Model, yalnızca normal ağ trafiği kullanılarak eğitilen bir autoencoder üzerine kuruludur.

3. Anomali tespiti, her katmandaki reconstruction loss ve Mahalanobis uzaklıklarının ağırlıklı birleştirilmesiyle elde edilen bir anomali skoru üzerinden gerçekleştirilmiştir.

4. Model, UNSW-NB15 veri kümesi üzerinde test edilmiş ve yalnızca reconstruction loss veya ham veriye Mahalanobis uzaklığı uygulayan modellere kıyasla daha yüksek performans göstermiştir.

5. Performans değerlendirme metrikleri: Precision, AUROC, AUPRC ve F1-score metrikleridir.

6. Sonuçlar, katman bazlı Mahalanobis uzaklıklarının entegrasyonunun ağ anomalilerini daha verimli biçimde tespit etmeyi sağladığını göstermektedir.

7. Gelecek çalışmalarda, önerilen yöntemin CICIDS-2017 vb. farklı veri kümeleri üzerinde test edilmesi ve gerçek zamanlı saldırı tespit sistemlerine entegrasyonu planlanmaktadır.


**1. Training Stage**

Bu aşama tamamen normal ağ trafiği (yani saldırı içermeyen veri) kullanılarak gerçekleştirilir.

* Adım 1: Autoencoder eğit

"Train an autoencoder using only normal data."
Normal verilerle bir autoencoder (kendini kodlayan ağ) eğitilir. Bu model, normal trafiğin yapısını öğrenir.

* Adım 2: Katman özelliklerini çıkar

"Extract features of each layer on the training samples."
Eğitilmiş autoencoder'a eğitim verisi tekrar verilir ve her katmanın ürettiği ara özellikler (feature) çıkarılır.

* Adım 3: Ortalama ve kovaryans hesapla

"Calculate mean μₗ and covariance matrix Σₗ of extracted features."
Her katman için çıkarılan özelliklerin ortalaması (μ) ve kovaryans matrisi (Σ) hesaplanır. Bu, Mahalanobis uzaklığı için gereklidir.

**2. Testing Stage**

Bu aşamada normal + anormal (saldırı) veriler kullanılır.

* Adım 4: Reconstruction Loss (RL(x)) hesapla

"Calculate reconstruction loss on test samples."
Autoencoder test verisini yeniden oluşturmaya çalışır.
Orijinal veri ile çıktısı arasındaki fark (hata) hesaplanır. Bu hataya reconstruction loss denir.

Normal veri → düşük hata

Anormal veri → yüksek hata

* Adım 5: Test örneklerinden katman özellikleri çıkar

Test verisi modele verilir ve katmanlardaki özellikler alınır.

* Adım 6: Mahalanobis uzaklığını hesapla

"For each layer, calculate Mahalanobis distances Mₗ(x)."
Her katman için, test özellikleri ile eğitim sırasında öğrenilmiş dağılım arasındaki uzaklık ölçülür.

Normal veri → düşük uzaklık

Anormal veri → yüksek uzaklık

* Adım 7: RL(x) + Mₗ(x) değerlerini birleştir

"Ensemble RL(x) and Mₗ(x) through the weighted sum."
Hem reconstruction loss hem de Mahalanobis uzaklıkları ağırlıklı şekilde toplanarak bir anomali skoru üretilir.

* Adım 8: Eşik uygulanarak anomaliler tespit edilir

"Detect anomaly data by applying a threshold."
Anomali skoru belirli bir eşik değerini geçerse veri saldırı olarak işaretlenir.



* ma_scores → Mahalanobis Distance Score
* if_scores → Isolation Forest Score
* ae_scores → Autoencoder Reconstruction Error Score
* efe_scores → Encoder Feature Embedding Scores
* dfe_scores → Decoder Feature Embedding Scores
  

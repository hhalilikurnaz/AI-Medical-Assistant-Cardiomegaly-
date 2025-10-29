# 🩺 AI Medical Assistant Core: Kardiyomegali Teşhisi (v1.0 - End-to-End Benchmark)

Bu proje, bir Sanal Teşhis Asistanının (AI Diagnostic Assistant) kalbi olan Kardiyomegali (Kalp Büyümesi) tespit modelinin geliştirilmesini, uçtan uca optimizasyon ve titiz bir benchmarking süreciyle belgelemektedir.

## 🏆 Proje Özeti ve Nihai Başarı

Tüm hiperparametre optimizasyonları ve mimari denemeler sonucunda, modelin genelleme yeteneği en üst düzeye çıkarılmış ve Validasyon skorundan bile **daha yüksek** bir skorla proje kapatılmıştır.

### Nihai Performans Metrikleri (Test Seti Üzerinde)

| Metrik | Validasyon (En İyi) | NİHAİ TEST SKORU | Yorum |
| :--- | :--- | :--- | :--- |
| **ROC-AUC** | 0.8639 | **0.8711** | Model, Validasyon skorundan yüksek Test skoru alarak Overfitting yapmadığını kanıtladı. |
| **Accuracy** | ~0.78 | **0.7962** | Genel doğruluk hedefine çok yaklaşıldı. |
| **Recall (Hassasiyet)** | N/A | **0.7882** | Hasta tespiti (%78.8) başarılı ve güçlü bir klinik temel sunar. |

## 🔬 Teknik Mimari ve Zorluk Yönetimi

### 1. Veri İşleme ve Ön Hazırlık

* **Veri Seti:** Kaggle Kardiyomegali Görüntüleri (4438 Görüntü, İkili Sınıflandırma).
* **Bölme Yöntemi:** **Stratified Shuffle Split** ile Train/Valid/Test setlerinde etiket dağılımının (%50 True/%50 False) korunması sağlandı.
* **Veri Yükleme:** `cv2` ile hızlı okuma ve **Albumentations** ile GPU tabanlı ön işleme zinciri oluşturuldu. Özellikle X-Ray kontrastını iyileştiren **CLAHE** transformasyonu kullanıldı.

### 2. İleri Düzey Optimizasyon ve Düzenlileştirme

Bu projede, ezberlemeyi (Overfitting) engellemek ve genellemeyi artırmak için üç temel teknik bir arada kullanılmıştır:

| Teknik | Tipi | Amaç ve Fayda |
| :--- | :--- | :--- |
| **Full Fine-Tuning** | Adaptasyon | Önceden eğitilmiş ağırlıkların (4.01M parametre) X-Ray verisine tam adaptasyonu sağlandı. |
| **MixUp & CutMix** | Düzenlileştirme | Sınıf sınırlarını yumuşatmak ve gürültüyü ezberlemeyi önlemek için veriler dinamik olarak karıştırıldı. |
| **Cosine Annealing LR**| Öğrenme Hızı | Öğrenme hızını agresif dalgalandırarak, modelin kayıp fonksiyonunda en derin ve genelleme yeteneği yüksek olan minimumu bulması hedeflendi. |

### 3. Model Benchmarking ve Stratejik Kararlar (Deneme Yanılma Kaydı)

Bu süreç, basitçe skor almak yerine, **stratejik karar verme yeteneğimizi** göstermektedir.

| Deneme No | Mimari | Öğrenme Hızı | En İyi Valid AUC | Stratejik Çıkarım |
| :--- | :--- | :--- | :--- | :--- |
| **D1** | **EfficientNet-B0** | ReduceLROnPlateau | **0.8639** | En iyi başlangıç skoru ve model temelini oluşturdu. |
| **D2** | EfficientNet-B0 | Cosine Annealing | 0.8639 | LR çizelgesi değişikliği B0'ın potansiyelini artırmadı. |
| **D3** | EfficientNet-B1 | Cosine Annealing | 0.8487 | **Başarısız Deneme:** Model kapasitesini artırmak (6.52M parametre), küçük veri setinde ezberlemeye yol açtı (skor düşüşü). B0'a geri dönüldü. |
| **D4** | EfficientNet-B0 | TTA (Test-Time) | 0.8638 | TTA, zaten emin olan modelde ek bir skor artışı sağlamadı. |
| **Final**| **EfficientNet-B0** | D1 Ağırlıkları | **0.8711 (Test)** | En iyi ağırlıkların Test setinde Validasyon skorundan yüksek performans gösterdiği doğrulandı. |

## ⏭️ Projenin Geleceği (Sanal Asistan Entegrasyonu)

Bu model, Sanal Doktor Asistanının görüntü analiz çekirdeği olarak hizmet verecektir. Bundan sonraki aşama:

1.  **Kapsam Genişletme:** 14 hastalıklı NIH Chest X-Ray veri setine geçiş ve Çoklu Etiket Sınıflandırması.
2.  **Veri Füzyonu:** Görüntü modeli çıktısı ile harici 3 Tahlil Analiz Modelinin çıktılarını birleştirmek.
3.  **LLM Raporlama:** Tüm bu sonuçları yapılandırılmış bir prompt ile bir Büyük Dil Modeline (LLM) besleyerek nihai, klinik olarak destekleyici bir rapor oluşturmak.

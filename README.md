# 🩺 AI Medical Assistant Core: Kardiyomegali Tespiti 

Bu not defteri, Sanal Doktor Asistanı sistemimizin ilk ve en önemli bileşenini oluşturur: Yüksek doğrulukta Kardiyomegali (Kalp Büyümesi) tespiti yapan bir model.

## 🏆 Nihai Başarı ve Skorlar

Tüm optimizasyon denemeleri sonucunda elde edilen en iyi model (EfficientNet-B0), test setinde aşağıdaki performansı göstermiştir:

* **NİHAİ ROC-AUC Skoru:** **0.8711** (Ayırma Gücü)
* **Accuracy Skoru:** 0.7962 (Genel Doğruluk)

Bu skorlar, modelin Kardiyomegali tespiti için klinik açıdan güvenilir bir temel oluşturduğunu kanıtlamaktadır.

## 🔬 Kullanılan Strateji ve Teknikler

Projede, modelin genelleme yeteneğini en üst düzeye çıkarmak için aşağıdaki ileri düzey teknikler kullanılmıştır:

1.  **Çekirdek Mimari:** EfficientNet-B0
2.  **Öğrenme Tipi:** Full Fine-Tuning (Önceden Eğitilmiş Ağırlıkların Tamamen Eğitilmesi)
3.  **Düzenlileştirme (Regularization):**
    * MixUp (Görüntüleri Lineer Karıştırma)
    * CutMix (Görüntü Bölgelerini Kesip Yapıştırma)
4.  **Öğrenme Hızı Çizelgesi:** Cosine Annealing LR

## ⏭️ Projenin Devamı (Sonraki Adım)

Bu başarılı model, artık asistanın görüntü analiz bileşeni olarak kullanılacaktır. Bundan sonraki aşama (başka bir not defterinde):

1.  **14 Hastalıklı NIH Veri Seti** üzerinde çoklu etiket sınıflandırması yaparak asistanın kapsamını genişletmek.
2.  Görüntü modeli çıktısı ve tahlil sonuçlarını birleştirip, nihai raporlama için bir **LLM (Büyük Dil Modeli)** entegrasyonu yapmak.

<div align="center">



\# Derin Öğrenme Tabanlı Görsel Açıklama Üretimi



\### Flickr30k veri seti üzerinde ResNet50 + LSTM tabanlı Image Captioning Projesi



Bu projede, görsellerden otomatik metin açıklaması üretmek amacıyla derin öğrenme tabanlı bir image captioning modeli geliştirilmiştir. Görsel özellik çıkarımı için ResNet50, açıklama üretimi için LSTM tabanlı bir model kullanılmıştır.



</div>



\---



\## Proje Özeti



Bu çalışmada bir görüntünün içeriğini analiz ederek doğal dilde açıklama üretebilen bir model geliştirilmiştir. Model, Flickr30k veri setindeki görseller ve bu görsellere ait açıklama cümleleri kullanılarak eğitilmiştir.



Projede temel olarak:



| Aşama | Kullanılan Yöntem |

|---|---|

| Görsel özellik çıkarımı | ResNet50 |

| Metin ön işleme | Tokenizer, startseq/endseq etiketleri |

| Model mimarisi | CNN + LSTM |

| Tahmin üretimi | Beam Search |

| Değerlendirme | BLEU-1, BLEU-2, BLEU-3, BLEU-4 |

| Model kaydı | ModelCheckpoint |



\---



\## Kullanılan Teknolojiler



| Teknoloji | Amaç |

|---|---|

| Python | Proje geliştirme dili |

| TensorFlow / Keras | Derin öğrenme modeli |

| ResNet50 | Görsel özellik çıkarımı |

| LSTM | Metin dizisi üretimi |

| Flickr30k | Görsel açıklama veri seti |

| BLEU Score | Model başarım değerlendirmesi |

| Git LFS | Büyük model dosyalarının GitHub’a yüklenmesi |



\---



\## Model Akışı



```mermaid

flowchart LR

&#x20;   A\[Görsel] --> B\[ResNet50]

&#x20;   B --> C\[2048 Boyutlu Görsel Özellik]

&#x20;   D\[Caption Metni] --> E\[Metin Ön İşleme]

&#x20;   E --> F\[Tokenizer]

&#x20;   C --> G\[CNN + LSTM Modeli]

&#x20;   F --> G

&#x20;   G --> H\[Caption Tahmini]

&#x20;   H --> I\[Beam Search]

&#x20;   I --> J\[BLEU Değerlendirmesi]

```



\---



\## Notebook Akışı



Notebook içerisinde proje aşağıdaki sırayla ilerlemektedir:



1\. Kütüphaneler ve Google Drive bağlantısı

2\. Flickr30k veri setinin Kaggle üzerinden indirilmesi

3\. Dizin ve dosya yollarının ayarlanması

4\. ResNet50 ile görsel özellik çıkarımı

5\. Caption metinlerinin temizlenmesi

6\. Tokenizer oluşturulması

7\. Data generator yapısının hazırlanması

8\. CNN + LSTM model mimarisinin oluşturulması

9\. Modelin eğitilmesi

10\. En iyi modelin `best\_model.keras` olarak kaydedilmesi

11\. Modelin tekrar yüklenmesi

12\. Beam Search ile açıklama üretimi

13\. BLEU skorları ile değerlendirme

14\. Örnek görseller üzerinde test



\---



\## Model Mimarisi



Model iki farklı girdiyi birlikte kullanmaktadır:



| Girdi | Açıklama |

|---|---|

| Görsel özellik girdisi | ResNet50 ile çıkarılan 2048 boyutlu vektör |

| Metin girdisi | Tokenizer ile sayısallaştırılmış caption dizileri |



Model, görsel özellikleri ve daha önce üretilmiş kelimeleri birlikte değerlendirerek sıradaki kelimeyi tahmin etmektedir.



\---



\## Eğitim Bilgileri



| Özellik | Değer |

|---|---:|

| Eğitim görsel sayısı | 25427 |

| Test görsel sayısı | 6357 |

| Sözlük boyutu | 19740 |

| Maksimum cümle uzunluğu | 80 |

| Epoch sayısı | 20 |

| Batch size | 32 |

| Toplam parametre sayısı | 12,558,876 |



Eğitim sırasında `ModelCheckpoint` kullanılmıştır. Böylece doğrulama kaybı en düşük olan model otomatik olarak kaydedilmiştir.



\---



\## Kaydedilen Proje Çıktıları



Model eğitimi sonucunda oluşan dosyalar `outputs/` klasörü altında tutulmaktadır:



```text

outputs/

├── best\_model.keras

├── resnet\_features.pkl

└── tokenizer.pkl

```



| Dosya | Açıklama |

|---|---|

| `best\_model.keras` | Eğitim sırasında seçilen en iyi model |

| `resnet\_features.pkl` | ResNet50 ile çıkarılmış görsel özellikler |

| `tokenizer.pkl` | Caption metinleri üzerinden oluşturulan tokenizer |



Bu dosyalar büyük boyutlu olduğu için GitHub’a \*\*Git LFS\*\* kullanılarak yüklenmiştir.



\---



\## Değerlendirme Sonuçları



Model başarımı 1000 test görseli üzerinde BLEU metrikleri ile değerlendirilmiştir.



| Metrik | Skor |

|---|---:|

| BLEU-1 | 0.561179 |

| BLEU-2 | 0.361876 |

| BLEU-3 | 0.232480 |

| BLEU-4 | 0.147978 |



BLEU-1 skorunun daha yüksek olması, modelin tekil kelime düzeyinde daha başarılı olduğunu göstermektedir. BLEU-3 ve BLEU-4 değerlerinin daha düşük olması ise modelin daha uzun ve bağlamsal cümle yapılarında zorlandığını göstermektedir.



\---



\## Örnek Çıktılar



Model bazı basit ve belirgin sahnelerde genel bağlamı yakalayabilmektedir. Ancak karmaşık sahnelerde nesneler arası ilişkiyi veya eylemi yanlış yorumlayabilmektedir.



Örnek çıktı görselleri `results/` klasörü altında yer almaktadır.



```text

results/

└── örnek model tahminleri

```



\---



\## Proje Dosya Yapısı



```text

deep-learning-image-captioning/

│

├── image\_captioning\_derin\_ogrenme.ipynb

├── requirements.txt

├── .gitignore

├── .gitattributes

│

├── outputs/

│   ├── best\_model.keras

│   ├── resnet\_features.pkl

│   └── tokenizer.pkl

│

└── results/

&#x20;   └── örnek model çıktıları

```



\---



\## Kurulum



Gerekli kütüphaneleri yüklemek için:



```bash

pip install -r requirements.txt

```



\---



\## Çalıştırma



Notebook Google Colab üzerinde çalıştırılacak şekilde hazırlanmıştır.



Projeyi baştan çalıştırmak için:



```text

1\. Notebook dosyasını Colab üzerinde açın.

2\. Google Drive bağlantısını çalıştırın.

3\. Kaggle API anahtarını yükleyin.

4\. Flickr30k veri setini indirme hücresini çalıştırın.

5\. ResNet50 ile özellik çıkarımı hücresini çalıştırın.

6\. Metin ön işleme ve tokenizer hücrelerini çalıştırın.

7\. Model mimarisini oluşturun.

8\. Eğitim hücresini çalıştırın.

9\. Beam Search ve BLEU değerlendirme hücrelerini çalıştırın.

```



Modeli yeniden eğitmeden test yapmak için kayıtlı çıktı dosyaları kullanılabilir:



```text

outputs/best\_model.keras

outputs/resnet\_features.pkl

outputs/tokenizer.pkl

```



\---



\## Sonuç



Bu proje kapsamında Flickr30k veri seti üzerinde derin öğrenme tabanlı bir görsel açıklama üretimi modeli geliştirilmiştir. Model, basit ve belirgin nesneler içeren görsellerde daha anlamlı açıklamalar üretirken, karmaşık sahnelerde ve detaylı nesne ilişkilerinde daha sınırlı performans göstermiştir.



Elde edilen sonuçlar, image captioning probleminin yalnızca nesne tanıma değil; aynı zamanda sahne bağlamı, eylem ilişkisi ve dil üretimi gerektiren daha karmaşık bir görev olduğunu göstermektedir.



\---



<div align="center">



\*\*Derin Öğrenme Dersi Proje Çalışması\*\*



</div>


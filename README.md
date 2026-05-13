\# Derin Öğrenme Tabanlı Görsel Açıklama Üretimi



Bu proje, Flickr30k veri seti üzerinde derin öğrenme yöntemleri kullanılarak görsellerden otomatik açıklama üretimi yapılmasını amaçlamaktadır. Projede görsellerden özellik çıkarmak için ResNet50 modeli, açıklama üretimi için ise LSTM tabanlı bir image captioning mimarisi kullanılmıştır.



Model, bir görsele ait sayısal özellikleri ve daha önce üretilmiş kelime dizisini birlikte kullanarak sıradaki kelimeyi tahmin edecek şekilde eğitilmiştir. Eğitim sonucunda en iyi model dosyası kaydedilmiş, daha sonra bu model Beam Search yöntemiyle açıklama üretmek ve BLEU metrikleriyle değerlendirme yapmak için kullanılmıştır.



\## Projenin Amacı



Bu çalışmanın amacı, bir görseli modele girdi olarak verip bu görsele uygun doğal dilde açıklama üretebilen bir sistem geliştirmektir. Bu kapsamda:



\- Flickr30k veri seti kullanılmıştır.

\- Görsellerden ResNet50 ile özellik çıkarılmıştır.

\- Açıklama metinleri temizlenmiş ve tokenize edilmiştir.

\- CNN + LSTM tabanlı bir model mimarisi kurulmuştur.

\- Eğitim sırasında en iyi model otomatik olarak kaydedilmiştir.

\- Beam Search ile açıklama üretimi yapılmıştır.

\- Model başarımı BLEU skorları ile değerlendirilmiştir.



\## Kullanılan Veri Seti



Projede Flickr30k veri seti kullanılmıştır. Veri seti, görüntüler ve bu görüntülere ait insan tarafından yazılmış açıklamalardan oluşmaktadır.



Veri seti büyük boyutlu olduğu için repoya dahil edilmemiştir. Notebook içerisinde veri setinin Kaggle üzerinden indirilmesine yönelik kodlar bulunmaktadır.



\## Proje Akışı



Notebook genel olarak aşağıdaki adımlardan oluşmaktadır:



1\. Gerekli kütüphanelerin yüklenmesi ve Google Drive bağlantısının yapılması

2\. Flickr30k veri setinin Kaggle üzerinden indirilmesi

3\. Proje klasörlerinin ve çıktı dosyalarının tanımlanması

4\. Görsellerden ResNet50 ile özellik çıkarılması

5\. Caption metinlerinin temizlenmesi

6\. Tokenizer oluşturulması ve kaydedilmesi

7\. Eğitim ve test verilerinin ayrılması

8\. Data generator yapısının hazırlanması

9\. CNN + LSTM tabanlı model mimarisinin oluşturulması

10\. Modelin eğitilmesi

11\. Doğrulama kaybına göre en iyi modelin kaydedilmesi

12\. Kaydedilmiş modelin tekrar yüklenmesi

13\. Beam Search yöntemi ile açıklama üretilmesi

14\. BLEU skorları ile model başarımının değerlendirilmesi

15\. Örnek görseller üzerinde model tahminlerinin incelenmesi



\## Notebook Bölümleri



Notebook içerisinde kullanılan ana bölümler şunlardır:



\- `Kütüphaneler ve Drive Bağlantısı`

\- `Kaggle'dan Veri Setini (Flickr30k) İndirme`

\- `Dizinler ve Ayarlar`

\- `ResNet50 ile Akıllı Özellik Çıkarımı`

\- `Metin Ön İşleme ve Tokenizer`

\- `Data Generator`

\- `Mimari`

\- `Eğitimi Aşaması`

\- `Modeli Tekrar Başlatma`

\- `Beam Search Sonuçları`

\- `Görseller ile Test`



\## Kullanılan Yöntem



\### Görsel Özellik Çıkarımı



Görsellerden özellik çıkarmak için ImageNet üzerinde önceden eğitilmiş ResNet50 modeli kullanılmıştır. ResNet50 modelinin sınıflandırma katmanı çıkarılmış ve `pooling='avg'` kullanılarak her görsel için 2048 boyutlu özellik vektörü elde edilmiştir.



Çıkarılan görsel özellikler tekrar tekrar hesaplanmaması için `resnet\_features.pkl` dosyasına kaydedilmiştir.



\### Metin Ön İşleme



Caption metinleri küçük harfe çevrilmiş, noktalama işaretlerinden temizlenmiş ve modele uygun hale getirilmiştir. Her açıklamanın başına `startseq`, sonuna ise `endseq` etiketi eklenmiştir.



Tokenizer kullanılarak kelimeler sayısal dizilere dönüştürülmüştür. Oluşturulan tokenizer daha sonra tekrar kullanılabilmesi için `tokenizer.pkl` dosyasına kaydedilmiştir.



\### Model Mimarisi



Model iki temel girdiden oluşmaktadır:



\- Görsel özellik girdisi

\- Metin dizisi girdisi



Görsel tarafta ResNet50 ile çıkarılmış 2048 boyutlu özellik vektörü kullanılmaktadır. Metin tarafında ise tokenizer ile sayısallaştırılmış caption dizileri LSTM katmanına verilmektedir.



Modelde görsel ve metinsel bilgiler birleştirilerek sıradaki kelime tahmin edilmektedir. Ayrıca modelde görsel temsilin korunmasına yardımcı olmak için ikinci bir çıktı yapısı da kullanılmıştır.



Modelin ana yapısı:



\- ResNet50 tabanlı görsel özellik çıkarımı

\- Embedding katmanı

\- LSTM katmanı

\- Concatenate katmanı

\- Dense katmanları

\- Dropout ve Batch Normalization

\- `word\_output` için categorical crossentropy kaybı

\- `visual\_output` için MSE kaybı



\## Eğitim Bilgileri



Notebook içerisinde veri seti eğitim ve test olarak ayrılmıştır.



| Özellik | Değer |

|---|---:|

| Eğitim görsel sayısı | 25427 |

| Test görsel sayısı | 6357 |

| Sözlük boyutu | 19740 |

| Maksimum cümle uzunluğu | 80 |

| Epoch sayısı | 20 |

| Batch size | 32 |

| Model parametre sayısı | 12,558,876 |



Eğitim sırasında `ModelCheckpoint` kullanılmıştır. Bu sayede doğrulama kaybı en düşük olan model otomatik olarak `best\_model.keras` dosyasına kaydedilmiştir.



\## Kaydedilen Çıktılar



Eğitim ve ön işleme sonucunda oluşan dosyalar `outputs/` klasörü altında tutulmaktadır:



```text

outputs/

├── best\_model.keras

├── resnet\_features.pkl

└── tokenizer.pkl

```



Bu dosyaların açıklamaları:



| Dosya | Açıklama |

|---|---|

| `best\_model.keras` | Eğitim sırasında doğrulama kaybına göre kaydedilen en iyi model |

| `resnet\_features.pkl` | ResNet50 ile çıkarılmış görsel özellikler |

| `tokenizer.pkl` | Caption metinleri üzerinden oluşturulan tokenizer |



Bu dosyalar büyük boyutlu olduğu için GitHub reposuna Git LFS kullanılarak eklenmiştir.



\## Modeli Tekrar Kullanma



Notebook içerisinde modeli yeniden eğitmeden kullanmak için ayrı bir bölüm bulunmaktadır. Bu bölümde daha önce kaydedilmiş olan:



\- `best\_model.keras`

\- `resnet\_features.pkl`

\- `tokenizer.pkl`



dosyaları tekrar yüklenmektedir. Böylece model yeniden eğitilmeden test ve değerlendirme aşamaları çalıştırılabilmektedir.



\## Değerlendirme



Model başarımı BLEU metrikleri ile değerlendirilmiştir. BLEU metriği, modelin ürettiği açıklamalar ile insan tarafından yazılmış referans açıklamalar arasındaki n-gram benzerliğini ölçmektedir.



Projede 1000 test görseli üzerinde Beam Search kullanılarak açıklamalar üretilmiş ve aşağıdaki BLEU skorları elde edilmiştir:



| Metrik | Skor |

|---|---:|

| BLEU-1 | 0.561179 |

| BLEU-2 | 0.361876 |

| BLEU-3 | 0.232480 |

| BLEU-4 | 0.147978 |



BLEU-1 skorunun diğer BLEU skorlarına göre daha yüksek olması, modelin tekil kelime düzeyinde daha başarılı olduğunu göstermektedir. BLEU-3 ve BLEU-4 skorlarının daha düşük olması ise modelin daha uzun kelime dizilerini ve cümle bütünlüğünü üretmede zorlandığını göstermektedir.



\## Örnek Model Çıktıları



Model bazı basit ve belirgin sahnelerde genel anlamda doğru açıklamalar üretebilmiştir. Örneğin kalabalık, yürüyen insanlar veya sokak sahneleri gibi görsellerde genel bağlamı yakalayabilmektedir.



Ancak bazı karmaşık görsellerde modelin nesne ayrımı ve sahne ilişkilerini doğru kurmakta zorlandığı görülmüştür. Örneğin su sporu içeren bir görsel için skateboard ifadesi üretmesi, modelin bazı nesne ve eylemleri karıştırabildiğini göstermektedir.



Bu durum, image captioning probleminin yalnızca nesne tanıma değil, aynı zamanda nesneler arası ilişki, eylem ve sahne bağlamını anlama gerektiren daha karmaşık bir problem olduğunu göstermektedir.



\## Dosya Yapısı



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



\## Kurulum



Gerekli Python kütüphanelerini yüklemek için:



```bash

pip install -r requirements.txt

```



\## Çalıştırma



Notebook Google Colab üzerinde çalıştırılacak şekilde hazırlanmıştır.



Projeyi baştan çalıştırmak için:



1\. Notebook dosyasını Colab üzerinde açın.

2\. Google Drive bağlantısını çalıştırın.

3\. Kaggle API anahtarını yükleyin.

4\. Flickr30k veri setini indirme hücresini çalıştırın.

5\. ResNet50 ile özellik çıkarımı hücresini çalıştırın.

6\. Metin ön işleme ve tokenizer hücrelerini çalıştırın.

7\. Model mimarisini oluşturun.

8\. Eğitim hücresini çalıştırın.

9\. Beam Search ve BLEU değerlendirme hücrelerini çalıştırın.



Eğer model yeniden eğitilmek istenmiyorsa, `Modeli Tekrar Başlatma` bölümü çalıştırılarak kayıtlı model ve çıktı dosyaları üzerinden test yapılabilir.



\## Sonuç



Bu proje kapsamında Flickr30k veri seti üzerinde derin öğrenme tabanlı bir görsel açıklama üretimi modeli geliştirilmiştir. Model, basit ve belirgin nesneler içeren görsellerde genel anlamda tutarlı açıklamalar üretirken, karmaşık sahnelerde ve detaylı nesne ilişkilerinde daha sınırlı performans göstermiştir.



Elde edilen sonuçlar, veri seti miktarının, model mimarisinin, eğitim süresinin ve kullanılan açıklama üretim yönteminin image captioning başarımı üzerinde doğrudan etkili olduğunu göstermektedir. Daha büyük veri setleri, daha güçlü görsel-dil modelleri ve gelişmiş attention tabanlı mimariler kullanılarak model başarımı artırılabilir.


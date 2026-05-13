<div align="center">



\# Derin Öğrenme Tabanlı Görsel Açıklama Üretimi



\*\*Flickr30K veri seti üzerinde ResNet50 özellik çıkarımı ve LSTM tabanlı Image Captioning modeli\*\*



`ResNet50` · `LSTM` · `Beam Search` · `BLEU Değerlendirmesi`



Bu proje kapsamında, görsellerden otomatik açıklama üretebilen derin öğrenme tabanlı bir model geliştirilmiştir.



</div>



<br>



\---



\## Proje Hakkında



Bu çalışmada amaç, bir görselin içeriğini analiz ederek bu görsele uygun doğal dilde açıklama üretebilen bir image captioning modeli geliştirmektir.

Model, Flickr30k veri setindeki görseller ve bu görsellere ait açıklama cümleleri kullanılarak eğitilmiştir. Görsel tarafta ResNet50 ile özellik çıkarımı yapılmış, metin tarafında ise caption verileri temizlenerek tokenizer ile sayısal dizilere dönüştürülmüştür.



\---



\## Kullanılan Yöntemler



| Aşama | Açıklama |

|---|---|

| Veri Seti | Flickr30k |

| Görsel Özellik Çıkarımı | ResNet50 |

| Metin Ön İşleme | Temizleme, tokenization, startseq/endseq |

| Model Mimarisi | CNN + LSTM |

| Tahmin Üretimi | Beam Search |

| Değerlendirme | BLEU metrikleri |

| Model Kaydı | ModelCheckpoint ile en iyi model seçimi |



\---



\## Proje Akışı



<table>

<tr>

<td align="center"><b>1</b><br>Veri Seti</td>

<td align="center">➡️</td>

<td align="center"><b>2</b><br>ResNet50 Özellik Çıkarımı</td>

<td align="center">➡️</td>

<td align="center"><b>3</b><br>Metin Ön İşleme</td>

</tr>

<tr>

<td align="center"><b>4</b><br>Tokenizer</td>

<td align="center">➡️</td>

<td align="center"><b>5</b><br>CNN + LSTM Modeli</td>

<td align="center">➡️</td>

<td align="center"><b>6</b><br>Beam Search + BLEU</td>

</tr>

</table>



\---



\## Notebook Bölümleri



Notebook içerisinde proje aşağıdaki ana bölümlerden oluşmaktadır:



| Bölüm | İçerik |

|---|---|

| Kütüphaneler ve Drive Bağlantısı | Gerekli kütüphaneler ve Google Drive bağlantısı |

| Kaggle'dan Veri Setini İndirme | Flickr30k veri setinin hazırlanması |

| Dizinler ve Ayarlar | Proje klasörleri ve çıktı yolları |

| ResNet50 ile Akıllı Özellik Çıkarımı | Görsellerden 2048 boyutlu özellik çıkarımı |

| Metin Ön İşleme ve Tokenizer | Caption temizleme ve tokenizer oluşturma |

| Data Generator | Eğitim için veri üretici yapı |

| Mimari | CNN + LSTM model yapısı |

| Eğitimi Aşaması | Model eğitimi ve en iyi modelin kaydı |

| Modeli Tekrar Başlatma | Kayıtlı model ve çıktıların tekrar yüklenmesi |

| Beam Search Sonuçları | Açıklama üretimi ve BLEU değerlendirmesi |

| Görseller ile Test | Örnek görseller üzerinde tahmin üretimi |



\---



\## Model Mimarisi



Model iki temel girdiyi birlikte kullanmaktadır:



| Girdi | Açıklama |

|---|---|

| Görsel Girdisi | ResNet50 ile çıkarılmış 2048 boyutlu özellik vektörü |

| Metin Girdisi | Tokenizer ile sayısal hale getirilmiş caption dizisi |



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



Eğitim sırasında ModelCheckpoint kullanılmıştır. Böylece doğrulama kaybı en düşük olan model otomatik olarak best\_model.keras dosyasına kaydedilmiştir.



\---



\## Kaydedilen Çıktılar



Proje çıktıları outputs klasörü altında tutulmaktadır:



```text

outputs/

├── best\_model.keras

├── resnet\_features.pkl

└── tokenizer.pkl

| Dosya | Açıklama |

|---|---|

| best\_model.keras | Eğitim sırasında seçilen en iyi model |

| resnet\_features.pkl | ResNet50 ile çıkarılmış görsel özellikler |

| tokenizer.pkl | Caption metinleri üzerinden oluşturulan tokenizer |



Bu dosyalar büyük boyutlu olduğu için GitHub reposuna Git LFS kullanılarak eklenmiştir.



\---



\## Değerlendirme Sonuçları



Model başarımı 1000 test görseli üzerinde BLEU metrikleri ile değerlendirilmiştir.



| Metrik | Skor |

|---|---:|

| BLEU-1 | 0.561179 |

| BLEU-2 | 0.361876 |

| BLEU-3 | 0.232480 |

| BLEU-4 | 0.147978 |



BLEU-1 değerinin diğer metriklere göre daha yüksek olması, modelin kelime düzeyinde daha başarılı olduğunu göstermektedir. BLEU-3 ve BLEU-4 skorlarının daha düşük olması ise modelin daha uzun ve bağlamsal cümle yapılarında zorlandığını göstermektedir.



\---



\## Örnek Çıktılar



Model bazı basit ve belirgin sahnelerde genel bağlamı yakalayabilmektedir. Ancak karmaşık sahnelerde nesneler arası ilişkiyi veya eylemi yanlış yorumlayabilmektedir.



Örnek model tahminleri results klasörü altında yer almaktadır.



\---



\## Proje Dosya Yapısı



<pre>

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

</pre>



\---



\## Kurulum



Gerekli kütüphaneleri yüklemek için:



<pre>

pip install -r requirements.txt

</pre>



\---



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



Modeli yeniden eğitmeden test yapmak için kayıtlı çıktı dosyaları kullanılabilir:



<pre>

outputs/best\_model.keras

outputs/resnet\_features.pkl

outputs/tokenizer.pkl

</pre>



\---



\## Sonuç



Bu proje kapsamında Flickr30k veri seti üzerinde derin öğrenme tabanlı bir görsel açıklama üretimi modeli geliştirilmiştir. Model, basit ve belirgin nesneler içeren görsellerde daha anlamlı açıklamalar üretirken, karmaşık sahnelerde ve detaylı nesne ilişkilerinde daha sınırlı performans göstermiştir.



Elde edilen sonuçlar, image captioning probleminin yalnızca nesne tanıma değil; aynı zamanda sahne bağlamı, eylem ilişkisi ve dil üretimi gerektiren daha karmaşık bir görev olduğunu göstermektedir.



\---



<div align="center">



<b>Derin Öğrenme Dersi Proje Çalışması</b>



</div>


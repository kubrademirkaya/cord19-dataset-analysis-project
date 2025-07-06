# cord19-dataset-analysis-project
# CORD-19 Literatür Analizi: Salgın Dönemi Araştırma Eğilimleri

## Proje Tanımı ve Amaç

Bu proje, Beyaz Saray ve önde gelen araştırma grupları tarafından oluşturulan **COVID-19 Açık Kaynak Veri Seti (CORD-19 Research Challenge)** üzerinde gerçekleştirilen bir veri analizi ve görselleştirme çalışmasıdır. 20 GB'ı aşkın boyutuyla CORD-19 veri seti, koronavirüs literatürünün bugüne kadarki en kapsamlı koleksiyonunu temsil etmektedir.

Koronavirüs literatüründeki hızlı değişimler, bilim insanlarının en güncel bilgilere ayak uydurmasını zorlaştırmaktadır. Bu projenin temel amacı, bu devasa literatür denizindeki **ana eğilimleri, yayın örüntülerini ve işbirliği dinamiklerini** ortaya koyarak tıp camiasının yüksek öncelikli bilimsel sorulara yanıtlar geliştirmesine ve araştırmalara hızlıca adapte olmasına yardımcı olabilecek **veri analizi ve görselleştirme araçları** geliştirmektir.

## Veri Seti

* **Kaynak:** Kaggle - [CORD-19 Research Challenge](https://www.kaggle.com/allen-institute-for-ai/CORD-19-research-challenge)
* **İçerik:** COVID-19, SARS, MERS ve diğer koronavirüslerle ilgili bilimsel makaleler, ön baskılar (preprints) ve diğer araştırma belgeleri. Veri seti, makale başlıkları, özetler, yazarlar, yayın tarihleri ve dergi bilgileri gibi meta verileri içermektedir.

## Kullanılan Araçlar ve Kütüphaneler

* **Programlama Dili:** Python 
* **Çalışma Ortamı:** Kaggle Notebooks (büyük veri setleri ve GPU erişimi avantajları nedeniyle tercih edilmiştir)
* **Kütüphaneler:**
    * `pandas`: Veri manipülasyonu ve analizi için.
    * `numpy`: Sayısal işlemler için.
    * `matplotlib`: Temel görselleştirmeler için.
    * `seaborn`: İstatistiksel görselleştirmeler için.
    * `wordcloud`: Kelime bulutu görselleştirmesi için.

## Proje Adımları ve Metodoloji

Proje, kısıtlı zaman çerçevesinde en anlamlı içgörüleri elde etmek üzere aşağıdaki adımları içermektedir:

1.  **Veri Yükleme ve İlk Keşif:**
    * `metadata.csv` dosyası, tüm makale meta verilerini içerdiği için ana veri kaynağı olarak belirlenmiştir.
    * Veri setinin ilk satırları (`.head()`), genel bilgileri (`.info()`) ve boyutları (`.shape()`) incelenerek veri yapısı anlaşılmıştır.
    * `publish_time` sütunu `datetime` formatına dönüştürülmüş ve yıl, ay-yıl gibi türetilmiş zaman özellikleri eklenmiştir.
    * `authors` sütunundan makale başına yazar sayısı (`num_authors`) ve ilk yazar (`first_author`) bilgileri çıkarılmıştır.
    * `journal` sütunundaki eksik değerler 'Unknown Journal' olarak doldurulmuştur.
    * **COVID-19 dönemi verilerine odaklanmak için, yayın tarihi `2019-12-01` ve sonrası olan makaleler filtrelenmiştir.** Bu, grafiklerde 1800'lerden başlayan eski yayınların elenmesini sağlamıştır.

2.  **Veri Temizleme ve Hazırlık:**
    * Görselleştirme için gerekli `cord_uid`, `title`, `abstract`, `publish_time`, `authors`, `journal`, `source_x` sütunları seçilmiştir.
    * Anahtar sütunlardaki (`publish_time`, `journal`) eksik değerler uygun şekilde işlenmiştir (örn. kaldırılmış veya doldurulmuştur).

3.  **Temel Veri Analizi ve Görselleştirme:**
    * **Zamana Dayalı Eğilimler:** Yıllık ve aylık yayın sayıları incelenerek COVID-19 araştırmalarındaki artış hızı ve dönemsel dalgalanmalar görselleştirilmiştir.
    * **Yayın Platformları:** En çok makale yayınlayan dergiler ve makalelerin geldiği kaynak veritabanları analiz edilmiştir.
    * **Yazar Katkısı:** Makale başına yazar sayısı dağılımı incelenerek işbirliği dinamikleri ve en üretken ilk yazarlar belirlenmiştir.
    * **Anahtar Kelime Özeti (İsteğe Bağlı):** Başlıklardan oluşturulan bir kelime bulutu ile literatürdeki en sık geçen terimler hızlıca özetlenmiştir.

## Bulgular ve Sonuçlar

Yapılan analizler ve görselleştirmeler sonucunda elde edilen temel bulgular aşağıdadır:

### 1. Zamana Göre Yayın Eğilimi

* **Yıllara Göre Makale Sayısı:**
    * Grafik: Number of COVID-19 Articles Published Per Year
    * 2019 yılında COVID-19 ile ilgili yayınlar çok azken, **2020 yılında olağanüstü bir artış** yaşanmıştır.
    * **2021 yılı, en yüksek yayın sayısına ulaşarak zirve yapmıştır.**
    * **2022 yılı itibarıyla yayın sayılarında belirgin bir düşüş gözlemlenmektedir.** Bu düşüş, büyük olasılıkla COVID-19 pandemisinin küresel etkisinin azalması ve CORD-19 veri setinin bu dönemden sonra kapsamlı güncellemelerinin durdurulmasıyla ilişkilidir. Veri seti, salgının en yoğun araştırma dönemini kapsamaktadır.

* **Aylık Yayın Eğilimi:**
    * Grafik: Monthly Trend of COVID-19 Article Publications
    * Aylık grafik, 2020'nin başından itibaren hızlı ve sürekli bir yayın akışını göstermektedir. 2020-2021 yılları boyunca araştırma çıktısının yüksek ve dalgalı seyrettiği görülmektedir.
    * **2022'nin başından itibaren gözlenen ani düşüş**, yıllık grafikteki çıkarımı desteklemektedir ve veri setinin güncelliğiyle ilgili aynı sınırlılığı işaret etmektedir.

### 2. Dergi ve Kaynak Dağılımı

* **En Çok Yayın Yapan Dergiler:**
    * Grafik: Top 15 Journals Publishing COVID-19 Articles
    * En çok yayın yapan dergiler arasında **"Unknown Journal"** başı çekmektedir. Bu durum, `journal` bilgisinin bazı makaleler için eksik olduğunu veya tutarsız kaydedildiğini göstermektedir.
    * `bioRxiv` gibi **preprint sunucularının** yüksek sıralaması, COVID-19 pandemisi sırasında araştırmacıların bulgularını hakemli yayın sürecini beklemeden hızla paylaşma ihtiyacını ve preprint'lerin önemini vurgulamaktadır.

* **Makale Kaynak Dağılımı:**
    * Grafik: Distribution of Articles by Source
    * Makalelerin büyük çoğunluğu **Medline ve PMC** (PubMed Central) veritabanlarından gelmektedir. "Medline; PMC" (%35.5) ve "Medline" (%21.3) en büyük katkıyı sağlamaktadır.
    * Bu durum, CORD-19 veri setinin biyomedikal literatürün ana akım kaynaklarına güçlü bir şekilde dayandığını göstermektedir.

### 3. Yazar Katkısı Analizi

* **Makale Başına Yazar Sayısı Dağılımı:**
    * Grafik: Distribution of Number of Authors Per Article
    * Makalelerin büyük çoğunluğunun **3 ila 6 yazar arasında** olduğu gözlemlenmiştir.
    * Tek yazarlı makale sayısı oldukça düşüktür. Bu, COVID-19 araştırmalarının bireysel çalışmalardan ziyade **disiplinlerarası ve ekip çalışmasına dayalı** bir yapıya sahip olduğunu açıkça ortaya koymaktadır.

### 4. Anahtar Kelime Özeti (Başlıklardan Kelime Bulutu)

* Grafik: Most Frequent Words in COVID-19 Article Titles
* Kelime bulutu, "COVID", "SARS-CoV", "pandemic", "patient", "virus", "infection", "vaccine", "disease", "treatment" gibi beklenen temel terimlerin yanı sıra, "respiratory", "clinical", "impact", "analysis", "health" gibi kavramların da literatürde baskın olduğunu göstermektedir. Bu, araştırmanın odak alanları hakkında hızlı bir görsel özet sunar.

## Sonuç

Bu proje, CORD-19 veri setindeki meta verileri kullanarak COVID-19 ile ilgili bilimsel yayınların **nicel eğilimlerini ve yapısal özelliklerini** görselleştirmeyi başarmıştır. Elde edilen bulgular, şu konularda hızlıca içgörü edinilmesine yardımcı olabilir:

* **Araştırma Hızının Anlaşılması:** Salgın boyunca araştırmaların ne kadar hızlı arttığını ve hangi dönemlerde zirve yaptığını görmek.
* **Etkili Yayın Kanalları:** COVID-19 araştırmalarında en aktif dergileri ve preprint sunucularını belirlemek, yeni araştırmaları takip etmek veya kendi çalışmalarını yayınlamak için yol göstermek.
* **İşbirliği Trendleri:** COVID-19 araştırmalarının genellikle çok yazarlı ve işbirlikçi doğasını anlamak, potansiyel ortaklıklar için fikir vermek.
* **Veri Kaynaklarının Rolü:** CORD-19 veri setinin ana kaynaklarını bilmek, araştırmacıların literatür taramalarını optimize etmelerine yardımcı olabilir.

## Projenin Sınırlılıkları ve Gelecek Çalışmalar

* Bu analiz, CORD-19 veri setinin **meta verileriyle sınırlıdır** ve makalelerin tam metin içeriklerinin derinlemesine metin madenciliğini kapsamamaktadır.
* Gözlemlenen **2022 sonrası veri düşüşü**, veri setinin güncelliğiyle ilgili önemli bir sınırlılıktır. Bu analizden elde edilen zaman bazlı çıkarımlar, ağırlıklı olarak 2020-2021 dönemi için geçerlidir. Gelecekteki çalışmalar, daha güncel COVID-19 literatürünü içeren farklı veri setleriyle desteklenebilir.
* `authors` sütunundaki karmaşık formatlar nedeniyle, yazar analizi basit metriklerle (yazar sayısı, ilk yazar) sınırlı kalmıştır. Gelecekte, yazarların ağ analizleri veya kurumsal işbirlikleri daha detaylı incelenebilir.







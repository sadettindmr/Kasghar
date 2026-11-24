# Kasghar kullanım kılavuzu

Bu kılavuz Kasghar'ı ilk kez kullananlar ve günlük kullanımda bir işlemin nasıl yapıldığını arayanlar için yazıldı. Bölümler uygulamadaki menülerin sırasını izler; soldaki içindekilerden istediğiniz bölüme geçebilirsiniz.

## Kasghar nedir?

Kasghar, literatür kaynaklarında arama yapıp sonuçları kendi bilgisayarınızdaki bir veritabanında biriktiren, düzenleyen ve dışa aktaran bir uygulamadır. Tarayıcıda çalışır ama veriler yalnızca sizin bilgisayarınızda (Docker içinde) tutulur; giriş (login) yoktur.

Başlıca işler:

- **Literatür taraması:** Crossref, PubMed, OpenAlex, Europe PMC, Scopus, Semantic Scholar, CORE, ClinicalTrials.gov ve DOAJ'da aynı sorguyla aynı anda arama yapıp sonuçları veritabanına kaydetmek.
- **Kütüphane:** Kaydedilen kayıtları görüntülemek, süzmek, düzeltmek ve araştırma kümelerine ayırmak.
- **Kayıt yönetimi:** Filtrelemek, etiketlemek, düzeltmek, mükerrer kayıtları birleştirmek, yayınlar arası ilişkileri (ön baskı, düzeltme, geri çekme) izlemek.
- **Tam metinler:** Açık erişim makalelerin tam metinlerini indirmek, PDF eklemek, tam metinde arama yapmak.
- **Dışa aktarma:** Zotero, Mendeley ve EndNote'a aktarılabilir dosyalar (BibTeX, RIS, CSL JSON, EndNote) ve biçimlendirilmiş kaynakça (APA, Vancouver…).
- **Araştırmalar:** Konudan makale taslağına adım adım ilerleyen, PRISMA kaydını tutan araştırma sihirbazı.
- **Atıf ağı ve Analiz:** Bir makaleden başlayan atıf taraması; arama analizleri, PRISMA tabloları ve PROSPERO metinleri.

## Kurulum ve ilk açılış

Kasghar hazır bir Docker imajıyla kurulur. Bilgisayarınızda **Docker Desktop** kurulu ve çalışır durumda olmalıdır.

1. Bir terminal açıp şu komutu çalıştırın (ilk seferde imaj indirilir; birkaç dakika sürebilir):

   `docker run -d --name kasghar --restart unless-stopped --stop-timeout 60 -p 127.0.0.1:8787:8000 -v kasghar-data:/data ghcr.io/sadettindmr/kasghar:latest`

2. Tarayıcıda **http://localhost:8787** adresini açın.

Veritabanı imajın içindedir; ayrıca bir şey kurmanız gerekmez. Bütün veriler `kasghar-data` volume'ünde durur.

- **Durdurma / başlatma:** `docker stop kasghar` ve `docker start kasghar`. Verileriniz silinmez.
- **Güncelleme:** Önce **Admin > Yedekleme**'den yedek alın. Sonra `docker pull ghcr.io/sadettindmr/kasghar:latest`, `docker rm -f kasghar` ve yukarıdaki `docker run` komutu. Veriler volume'de kaldığı için korunur.
- **Verileri silen tek komut** `docker volume rm kasghar-data`'dır; kullanmayın.
- **Kayıt klasörü (isteğe bağlı):** dosyaların bilgisayardaki bir klasöre kaydedilmesi için komuta `-v ~/Kasghar:/exports -e EXPORT_DIR_CONTAINER=/exports -e EXPORT_DIR_HOST=~/Kasghar` ekleyin.
- **Bellek:** Docker'a en az 4 GB bellek ayırmanız önerilir.

## Ekran düzeni

Her sayfanın üstünde aynı menü çubuğu vardır:

| Menü | Ne işe yarar |
|---|---|
| **Literatür taraması** | Açılış ekranı. Aynı sorguyla birden çok kaynakta eş zamanlı arama; sonuçlar veritabanına kaydedilir |
| **Kütüphane** | Kaydedilen kayıtları süzme, görüntüleme, düzeltme, dışa aktarma ve araştırma kümelerine ekleme |
| **Araştırmalar** | Araştırma sihirbazı: konudan makale taslağına adım adım iş akışı |
| **Atıf ağı** | Bir makaleden kaynakçasına ve atıf alanlara doğru genişleyen ağ |
| **Analiz** | Arama analizleri (PRISMA tabloları, PROSPERO metinleri, Excel raporu) ve yayın türleri |
| **Admin** | Ayarlar, aramalar, yedekleme, bakım ve günlük |

- **Kasghar logosu:** Hakkında penceresini açar. Arka planda süren işler (indirmeler, taramalar, içe aktarmalar) burada ilerleme çubuğuyla listelenir. Logonun köşesindeki sarı nokta, arka planda bir iş sürdüğünü gösterir.
- **? (Yardım):** Bu kullanım kılavuzunu açar.
- **TR / EN:** Arayüz dilini değiştirir. Kayıtların kendisi (başlık, özet) çevrilmez.
- **"i" düğmesi:** Sayfa başlığının yanındaki bu düğme, sayfanın açıklamasını açar.
- **Bildirimler:** Biten işlemler sağ altta kısa bir bildirimle onaylanır.
- **Kısayollar:** Alt (Mac'te ⌥) + 1…6 ile menüler arasında geçilir.
- **Telefonda** üst menü ☰ düğmesiyle açılan bir listeye dönüşür.

## İlk ayarlar

Kasghar ayar yapmadan da çalışır; ancak birkaç ayar arama hızını ve kotaları artırır. Ayarlar **Admin > Ayarlar** sayfasındadır.

### Genel

- **İletişim e-postası:** Crossref ve PubMed'e kim olduğunuzu bildirir; Crossref'te hız saniyede 1 istekten 3'e çıkar. Adres yalnızca ilgili kaynağın API'sine gönderilir.
- **İndirilen dosyalar nereye kaydedilsin:** Tarayıcıyla indir, bilgisayardaki kayıt klasörüne kaydet ya da ikisi birden.
- **Varsayılan arama kaynağı:** Kütüphane sayfası açıldığında seçili gelen kaynak.

Aynı bölümün altında **Tam metin deposu** ayarları (depo kapasitesi, açık erişim kaynakları) vardır.

### Performans

Ağır işlerin (mükerrer kayıt taraması, literatür taramasındaki eleme) ne kadar paralel ve ne kadar bellekle çalışacağı: **Mükerrer taramada paralel bağlantı** (boşsa çekirdek sayısına göre), **Ağır sorgularda sorgu belleği** ve **Sorgu başına paralel işçi**. Kaydedince hemen uygulanır. Altındaki **Sunucu** kartı Docker'ın kullanabildiği çekirdek sayısını ve veritabanının sunucu ayarlarını gösterir; bunlar yeniden başlatma gerektirir ve kurulum klasöründe ortam değişkenleriyle değiştirilir (ör. `PG_SHARED_BUFFERS=2GB PG_CACHE=6GB docker compose up -d`).

### Kaynaklar

**Kaynaklar** başlığına tıklayınca her kaynağın sınırları ve kotaları görünür: tek aramada en fazla kaç kayıt indirilebileceği, hız sınırı, e-posta ve API anahtarının girilip girilmediği, OpenAlex'te günlük bütçe ve kalan miktar, Scopus'ta haftalık kota. **Kotaları yenile** her kaynağa tek bir ucuz istek gönderip bu bilgileri günceller. Bir kaynağın günlük bütçesi veya kotası dolduysa **Kaynak bekletiyor** satırı beklemenin bittiği zamanı gösterir; bu süre kaynağın bildirdiğidir, veritabanını sıfırlamak onu kaldırmaz (yeni bir API anahtarı kaydedilirse kalkar).

Başlığın altındaki alt bölümlerde kaynakların kendi ayarları vardır:

| Kaynak | Ayar | Gerekli mi? |
|---|---|---|
| Crossref | E-posta (yoksa Genel'deki kullanılır), Metadata Plus anahtarı | Hayır |
| PubMed | E-posta, NCBI API anahtarı (saniyede 3 → 10 istek) | Hayır |
| OpenAlex | Ücretsiz API anahtarı (günlük bütçe 0,10 $ → 1 $) | Hayır, önerilir |
| Europe PMC | — | Gerekmez |
| Scopus | Elsevier API anahtarı, isteğe bağlı kurum belirteci (Insttoken) | **Evet**, kurum aboneliği de gerekir |
| Semantic Scholar | API anahtarı | Hayır, önerilir (anahtarsız istekler yoğunlukta reddedilir). **Bağlantıyı test et** anahtarı 3 kez dener ve reddedilen deneme sayısını söyler; reddedilen istekler aramada anahtarsız tekrarlanır |
| CORE | API anahtarı (core.ac.uk'ta ücretsiz kayıt) | **Evet** (anahtarsız istekler hemen sınıra takılır) |
| ClinicalTrials.gov | — | Gerekmez (klinik çalışma kayıtları; makale değil) |
| DOAJ | — | Gerekmez (açık erişim dergi makaleleri) |
| OpenCitations | Erişim belirteci | Hayır (yalnızca veri tamamlamada kullanılır; arama kaynağı değildir) |
| DataCite | — | Gerekmez (yalnızca veri tamamlamada: arXiv, Zenodo, depo ve veri seti DOI'leri) |

API anahtarları kaydedildikten sonra bir daha gösterilmez; yalnızca girilip girilmediği görünür. **Bağlantıyı test et** düğmesi ayarların çalışıp çalışmadığını dener.

### Yapay zekâ

**Admin > Ayarlar > Yapay zekâ** yapay zekâ görevlerinin hangi modelle çalışacağını belirler. Bugün çalışan görevler: araştırmalarda **anahtar kelime önerisi** (e.) ve **başlık-özet tarama önerileri** (p.); dil tespiti, ilişki ve mükerrer sayfalarında **dil kararı**, **ilişki** ve **mükerrer önerilerinde karar önerisi**; konu benzerliği, tam metin tarama önerisi, veri çıkarımı ve çeviri sırayla gelecek. Birden çok sağlayıcı ekleyebilirsiniz:

- **Hazır bulut API'leri:** Google Gemini (ücretsiz katman), OpenAI, Anthropic, OpenRouter, Groq, Mistral. Adres ve istek biçimi doludur; **API anahtarı al ↗** bağlantısından anahtarınızı alıp girin.
- **Yeni API:** listede olmayan bir servis için ad, adres, istek biçimi (çoğu servis OpenAI uyumludur), anahtar ve model.
- **Bu bilgisayarda:** Ollama ya da LM Studio (Mac'te GPU ile çalışır; veri bilgisayardan çıkmaz).

Sağlayıcı eklemek için soldaki menüde **Yapay zekâ**'nın altındaki **Sağlayıcı ekle** bölümünü açın (ya da sağlayıcı listesindeki **+ Sağlayıcı ekle** düğmesine basın); türü seçin, gerekirse anahtarı girin, **Modelleri getir** ile sağlayıcının model listesini alın; **Model** kutusu seçim listesine dönüşür, modeli listeden seçin (listede olmayan bir model için **Listede yok, elle yaz…**; kutu boş gelir, önerilen model kutunun içinde soluk yazılıdır), **Bağlantıyı test et** ile deneyin (kısa bir istek gönderilir; kayıt verisi gönderilmez) ve **Kaydet**. **Dakikalık / Günlük istek sınırı** uygulama tarafından uygulanır: sınıra gelince istek gönderilmez (Gemini ücretsiz katmanında model seçilince temkinli varsayılanlar dolar; güncel değeri **Güncel ücretsiz sınırlar ↗** bağlantısında görüp yazın). Listedeki her sağlayıcıda bugünkü istek sayısı görünür; **Düzenle**, **Varsayılan yap**, **Sil**. **Görev yönlendirme** her görevin hangi sağlayıcıyla çalışacağını belirler (seçilmeyen görev varsayılanı kullanır); **çalışıyor** işaretli görevler kullanılabilir, **yakında** işaretliler sonraki sürümlerde gelecek.

**Bağlam penceresi (belirteç):** modelin bir istekte okuyabildiği metin miktarıdır (gönderilen metin + yanıt). Ollama'da modele bu değer gönderilir (boş bırakılırsa 8.192); büyüdükçe bellek kullanımı ve süre artar. 16 GB bellekli bir Mac'te tarama ve anahtar kelime için 8.192 yeterlidir. LM Studio'da bağlam uzunluğu model yüklenirken LM Studio'da ayarlanır; buraya aynı değeri yazın. Bulut modellerinde boş bırakabilirsiniz. Uygulama, gönderdiği metni (ör. uzun özetleri) bu değere göre kısaltır. Aynı modeli farklı bağlam penceresiyle kullanmak isterseniz sağlayıcıyı ikinci kez ekleyip görevleri ayrı ayrı yönlendirin.

**Görevlere göre model önerisi (ücretsiz, internetsiz; 16 GB bellekli Mac):**

| Görev | Önerilen model | Bağlam |
|---|---|---|
| Anahtar kelime ve arama sorgusu önerisi | qwen3.5:9b (Ollama); istenirse Gemini Flash-Lite (az istek, kayıt verisi gönderilmez) | 8.192 |
| Dil kararı (Araştırmalar > l. Dil tespiti) | qwen3.5:9b (bir istekte 20 kayıt; doğruluk ölçümünde gemma3:4b'den belirgin iyi) | 8.192 |
| İlişki ve mükerrer önerilerinde karar önerisi | qwen3.5:9b, bir istekte 1 öğe (~8 sn) | 8.192 |
| Konu benzerliği (yakında) | Gömme modeli: bge-m3 (Ollama, çok dilli); daha hızlısı nomic-embed-text | 8.192 |
| Başlık-özet tarama önerisi | qwen3.5:9b, bir istekte 1 eser; hızlı deneme için gemma3:4b | 8.192 |
| Tam metin tarama önerisi (yakında) | qwen3.5:9b | 32.768 |
| CHARMS veri çıkarımı (yakında) | qwen3.5:9b; sonuçlar mutlaka elle denetlenir | 32.768 |
| Özet çevirisi EN → TR (yakında) | gemma3:4b (hızlı) ya da qwen3.5:9b (daha isabetli) | 8.192 |
| Özet ve metin üretimi (yakında) | qwen3.5:9b | 16.384–32.768 |

Aynı modeli iki bağlam değeriyle kullanmak için Ollama'yı iki kez ekleyin (ör. “Ollama 8K” ve “Ollama 32K”) ve görevleri **Görev yönlendirme**'den ayrı ayrı yönlendirin.

**Yük sınırı (bu bilgisayardaki sağlayıcılarda):** toplu görevler bilgisayarı saatlerce tam yükte çalıştırır; 16 GB'lık Mac'te bellek dolarsa bilgisayar kilitlenip yeniden başlayabilir. Sağlayıcıyı düzenlerken:

- **İstekler arası bekleme (sn):** her istekten sonra beklenir (varsayılan 2; öneri 2–5).
- **İşlemci çekirdeği sınırı:** Ollama'nın kullanacağı çekirdek sayısı (öneri 4; boş: hepsi).
- **Çalışma saatleri:** ör. `23-07`; toplu görevler yalnızca bu saatlerde istek gönderir, pencere dışında bekler ve pencere açılınca kendiliğinden sürer. Görevi gündüz başlatıp geceye bırakabilirsiniz.

Ayrıca uygulama yerel modele aynı anda tek istek gönderir, Ollama'da yalnızca bir model (tek bağlam ayarıyla) yüklü tutar, bir istek olağanın çok üstünde sürerse (bellek dolup bilgisayar yavaşladığında) modeli bellekten boşaltıp 3 dakika dinlenir; uygulama ya da bilgisayar yeniden başlarsa yapay zekâ görevlerini kendiliğinden sürdürmez. **En büyük kazanç Docker'ın belleğindedir:** Docker Desktop > Settings > Resources > Memory 8 GB ise 5 GB'a indirin (uygulama ve veritabanı için yeterli); qwen3.5:9b çalışırken Chrome gibi bellek kullanan uygulamaları kapatın.

**Ücretsiz ve internetsiz kullanım için öneri:** Ollama'yı Mac'e kurup bir model indirin (Terminal'de `ollama pull qwen3.5:9b`), sonra **Sağlayıcı ekle > Ollama (bu bilgisayarda)**, **Modelleri getir**, modeli seçin, **Kaydet**. 16 GB bellekte **qwen3.5:9b** en isabetli seçenektir (eser başına birkaç saniye); **gemma3:4b** daha hızlı ama daha az isabetlidir.

Gizlilik: e-posta adresiniz hiçbir sağlayıcıya gönderilmez. Kayıt verisi (başlık, özet, tam metin) bir bulut sağlayıcıya yalnızca **Kayıt verisi gönderilebilir** işaretliyse gönderilir. Gemini'nin ücretsiz katmanında Google gönderilen içeriği ürünlerini geliştirmek için kullanabilir; yayımlanmamış ya da gizli metin göndermeyin. Her çağrı **Günlük**'e içeriği olmadan yazılır.

### Yayın türleri

Her kaynak yayın türlerini kendi adlarıyla bildirir. Kasghar bunları kendi ortak türlerine (Dergi makalesi, Derleme, Olgu sunumu…) eşler. **Admin > Ayarlar > Yayın türleri**'nde bu eşlemeyi görebilir ve bir terimin hangi türe gideceğini elle değiştirebilirsiniz. Değişiklik o kaynağın o terimli kayıtlarına hemen uygulanır.

## Kütüphane (kayıtları görüntüleme)

Kütüphane sayfası veritabanındaki kayıtları görüntülemek ve süzmek içindir; kaynaklarda yeni arama **Literatür taraması** sayfasından yapılır. Soldaki **Filtreler** bölmesi ortadaki listeyi süzer (« düğmesiyle daraltılabilir):

- **Kayıtlarda ara:** Başlık, yazar, dergi, DOI ve PMID'de arar; sorgu dili için kutunun yanındaki **?** işaretine ve aşağıdaki tabloya bakın.
- **Arama:** Yalnızca seçilen aramayla gelen kayıtlar.
- **Kaynak:** Kayıtları olan kaynaklar onay kutusuyla listelenir; birini veya birkaçını işaretleyin, yalnızca o kaynaklardan gelen kayıtlar listelenir. Liste sayım beklemeden hemen gelir.
- **Yıl:** Başlangıç ve bitiş yılı (dahil).
- **Yayın:** Tür, dil ve ilişki (ör. geri çekilmiş, ön baskısı olan).
- **İçerik:** **Özeti olanlar**, **Ön baskıları hariç tut**, **Geri çekilmişleri hariç tut**, **En az atıf**, **Dil tespiti** (seçilen seçeneğin açıklaması süzgecin altında yazar; bkz. Araçlar > Dil tespiti ve doğrulaması):
  - **Kaynak dili doğrulananlar:** kaynağın bildirdiği dil, başlık ve özetten tespit edilen dille aynı. Örneğin **Dil: İngilizce** ile birlikte seçilince dili kesin İngilizce olan eserler doğrudan bir araştırma kümesine eklenebilir.
  - **Kaynak diliyle uyuşmayanlar:** kaynağın dili tespit edilenden farklı; kaynağın dili korundu, karar bekliyor (Dil tespiti sayfası'nde elle düzeltilir).
  - **Dili tespitle tamamlananlar:** kaynakta dil yoktu, tespit edilen dil yazıldı.
  - **Dili düzeltilenler:** kaynağın dili otomatik düzeltildi ya da elle seçildi.
  - **Dili tespit edilemeyenler:** başlık / özet çok kısa ya da tahmin zayıf.
  - **Henüz bakılmamış:** dil tespiti bu kayıtlarda henüz çalışmadı. Literatür taramasındaki eski uygunluk ölçütleri artık burada, kayıtlar veritabanına alındıktan sonra uygulanır; hiçbir kayıt silinmez.
- **Etiketler:** Yazmaya başlayınca mevcut etiketler önerilir; ok tuşları ve Enter ile ya da tıklayarak seçin, birden çok etiket seçilebilir (seçilenlerden herhangi birini taşıyan eserler). Altında en çok kullanılan etiketler hazır düğme olarak durur; etiket çoksa **Tüm etiketler** aranabilir bir liste açar. Seçili etiketi kaldırmak için çipin × işaretine tıklayın.
- **Filtreleri temizle** hepsini kaldırır.

Filtre sayıları (tür, dil, ilişki, etiket) bir kez hesaplanıp saklanır; veri değişmedikçe hemen gelir.

Filtreler dışa aktarma, kaynakça, açık erişim tam metin ve eksik metadata işlemlerinde de geçerlidir (kayıt seçmediyseniz).

## Literatür taraması

Aynı sorguyu birden çok kaynakta **eş zamanlı** çalıştırır; sistematik derleme gibi çalışmalarda literatürü tek seferde toplamak için kullanılır. Kaynaklarda arama yalnızca buradan yapılır. Tüm sonuçlar veritabanına alınır; dil, yayın türü, özet, atıf gibi ayıklamalar sonra **Arama** sayfasındaki filtrelerle yapılır.

1. **Sorgu:** Ortak sorguyu yazın (AND / OR / NOT ve "ifade" kullanılabilir) ve aranan alanı (başlık ve özet, yalnızca başlık, tüm alanlar) ile yıl aralığını seçin.
2. **Kaynaklar:** Aranacak kaynakları işaretleyin. Kaynağın simgesinin (ör. **PM**, **EP**) üzerine gelince kaynağın açıklaması, sınırları, hangi alanlarda aradığı ve varsa uyarıları açılır. E-postası veya API anahtarı girilmemiş kaynaklar soluk görünür ve seçilemez; yanlarındaki **Ayarla →** bağlantısı ilgili ayara götürür. Europe PMC ayar gerektirmez.
3. **Çalışma etiketi (isteğe bağlı):** Bu aramayla gelen eserlere eklenir. Kütüphane sayfasında **Etiketler** filtresiyle ayrılır.
4. **Sonuç sayılarını al:** Seçili her kaynaktan sonuç sayısı, uyarılar ve kota bilgisi aynı anda istenir. Sayı, kaynağın kutusunda "sınır yok" yazısının altında görünür (yanıt bekleyenlerde "sayılıyor…"; kaynak sınırı varsa "ilk N alınır"), toplam düğmelerin üstündedir. Kaynağın simgesinin üzerine gelince açılan pencerede kaynağa özel düzenlenebilir sorgu, kaynağa gönderilen sorgu, kaynakta ve sonradan uygulanacak ölçütler, notlar ve kota da yer alır. Sayım kısa sürede yanıt alamayan kaynakta uzun süre beklemez, hatayı bildirir; arama başlatıldıktan sonraki indirmede ise geçici hatalar uzun aralıklarla yeniden denenir. Bir kaynak hata verirse kutusunda "hata" yazar (nedeni simgenin penceresinde); yanındaki **Yeniden say** yalnızca o kaynağı yeniden sorgular. Bir kaynağın sorgusunu düzenlerseniz o kutuda **Yeniden say**, sayımdan sonra yeni bir kaynak işaretlerseniz **Say** bağlantısı çıkar. Düğme sayımdan sonra **Yeniden say** olur (tüm seçili kaynakları sayar) ve sayım bitene kadar pasiftir. Ortak sorguyu, aranan alanı ya da yılları değiştirirseniz sayılar silinir, yeniden saymanız gerekir.
5. **Veritabanına kaydet:** Seçili kaynakların hepsi sayılınca etkinleşir (pasifken üzerine gelince nedeni yazar). Sayımda hata veren kaynaklar alınmaz; onay penceresinde adları yazar. Onaylayınca arama başlar. Her taramada PRISMA 2020 uyumlu kayıt tutulur: her kaynağa gönderilen sorgu, arayüz, sınırlamalar, arama tarihi ve sonuç sayısı (PRISMA-S arama stratejisi) değişmez biçimde saklanır; akış şeması ve rapor üretilir. Rapor tarama bitince özetten açılır ve daha sonra **Arama analizi** ya da **Araştırmalar** sayfasından istenildiği zaman yeniden alınabilir.

İpuçları:

- **Kesilen indirmeler** kaldığı yerden sürdürülür. Uygulama yeniden başlarsa (bilgisayar uykudan çıkınca Docker'ın yeniden başlaması dahil) yarım kalan iş, veritabanı açılınca kendiliğinden devam eder; kendiliğinden sürdürülemeyenler için **Sürdür** düğmesi çıkar.
- **Kaynak sınırları:** PubMed tek sorguda en fazla 9.999 kayıt verir, ama sonuç daha fazlaysa Kasghar aramayı PMID aralıklarına bölüp tamamını indirir; diğer kaynaklarda sınır yoktur (OpenAlex'te günlük bütçe, Scopus'ta haftalık kota geçerlidir). Scopus kurum ağında (ya da Insttoken ile) çok daha hızlıdır: istek başına 200 kayıt alınır (kurum dışında ve COMPLETE görünümde 25) ve yıl aralığı verilmiş büyük aramalar yıllara bölünüp aynı anda indirilir; haftalık kota da sekiz kat daha geç dolar. Scopus'ta tüm sonuçları almak kurum ağından (ya da Insttoken ile) bağlanmayı gerektirir: kurum ağı dışında en fazla 5.000 kayıt alınır ve arama **yarım kaldı** olarak biter; kurum ağına bağlandığınızda **Sürdür** kalanını kaldığı yerden getirir.

> **Crossref** mantıksal işleçleri (AND / OR / NOT), tırnakları ve parantezleri desteklemez; sorgu puanlı serbest metin olarak aranır ve kelimelerden yalnızca biri geçen kayıtlar da gelir. Sonuç sayısı bu yüzden diğer kaynaklardan çok daha fazla olabilir; sayım sonucunda bu uyarı gösterilir.

> **PubMed** tek sorguda en fazla 9.999 kayıt verir; daha fazla sonuç gelirse arama PMID aralıklarına (her biri 9.999'un altında, çakışmasız) bölünüp arka planda tamamı indirilir. Kesilirse kaldığı aralıktan sürer.

Sayfa üç sütundur: solda **İşlem adımları** ve altında **Geçmiş literatür taramaları**, ortada arama formu ve sonuç sayıları, sağda **Özet**. Dar ekranda özet formun altına iner. Geçmiş taramalardan birine basınca adımlar, sorgu, o taramanın başlatıldığı andaki sonuç sayıları ve özet o taramaya geçer; **Yeni tarama** ile kendi formunuza dönersiniz. Bir tarama sürerken de geçmiş taramaları inceleyebilirsiniz.

**İşlem adımları** sol panelde sırayla izlenir:

1. Kaynaklar sorgulanıyor (kitap animasyonu)
2. Kaynaklardan alınıp veritabanına yazılıyor (buluttan veritabanına kablo üzerinden akan veri animasyonu; kaynak başına ilerleme çubukları ve kalan süre)
3. Eserler eşitleniyor (aynı DOI / PMID'li kayıtlar tek esere bağlanır)
4. Mükerrer kayıt taraması (benzer başlıklar)
5. Ölçütlere göre eleme (eser düzeyinde; dışlananlar işaretlenir)
6. Etiketleme (etiket verildiyse)
7. Özet

Her adımın işareti durumunu gösterir: ✓ tamamlandı, kırmızı **✗** hata verdi (hata veren kaynaklar ve hata metni adımın altında listelenir). Veritabanına yazma adımında her kaynağın ilerleme çubuğunda tahmini **kalan süre**, panelin altında tüm adımlar için toplam **geçen süre** (ve sürüyorsa kalan süre) gösterilir. Geçici hatayla (ör. kaynağın sunucusu yanıt vermedi) yarım kalan kaynak artan aralıklarla (1, 5 ve 15 dakika sonra) en fazla üç kez otomatik sürdürülür. "Çok fazla istek" hatasında kaynak bir bekleme süresi bildirdiyse o süre sonunda (en fazla 2 saat) bir kez sürdürülür, bildirmediyse otomatik deneme yapılmaz; bekleme süresi kaynağın çubuğunda gösterilir. Günlük bütçesi veya kotası dolan kaynak (ör. OpenAlex) "yarım kaldı" olarak işaretlenir; **Sürdür** düğmesi kaynağın yeniden istek kabul edeceği saate kadar kapalıdır. Kaynak bu saati bildirdiyse ve saat bir günden yakınsa tarama mükerrer kayıt taramasına eksik kayıtla geçmez: kaynağın çubuğunda "kota / bütçe doldu, … içinde kendiliğinden sürdürülecek" yazar ve o saatte kaynak kaldığı yerden sürdürülür (OpenAlex bütçesi her gün 03:00'te, Türkiye saatiyle, yenilenir). Beklemek istemezseniz **Durdur**'a basıp daha sonra **Sürdür** ile devam edebilirsiniz. Semantic Scholar uzun süre "çok fazla istek" derse kaynak 5 dakika sonra kendiliğinden sürdürülür. Kaynak çubuğundaki sayı kaynaktan **alınan / alınacak** kayıttır. Yeniden indirilen bir kaynakta aramaya bağlı kayıt bundan fazla olabilir: önceki indirmede gelip kaynağın bugünkü sonuçlarında bulunmayan kayıtlar silinmez; fark çubuğun altında, özette ve PRISMA raporunda yazar. **Geçen süre** taramanın çalıştığı süredir; uygulamanın kapalı olduğu ya da taramanın çalışmadığı günler sayılmaz.

**Özetteki kaynak tablosu:** Her kaynak için **Bulunan** (kaynaktaki sonuç), **Yazılan** (veritabanına yazılan), **Yeni** (veritabanında önceden olmayan), **Mükerrer** (elenen) ve **Kalan** (mükerrer elemesinden sonra kalan eser; araştırmanın başlangıç havuzu bunların toplamıdır) sayıları. Yalnızca bir kaynakta bulunan eser sayıları Arama analizindedir. Kaynağın adına basınca kayıtları Kütüphane'de açılır. Gönderilen sorgular, yayın yılı aralığı ve arama zamanları (birkaç güne yayıldıysa başlangıç ve bitiş) tablonun altındaki **Sorgular ve arama zamanları** bölümündedir. Kaynaklar tablodaki sırayla değerlendirilir: bir yayın ilk bulunduğu kaynağa sayılır, sonraki kaynaklarda tekrar bulunan kayıtları mükerrer sayılır. Bu yüzden **Kalan** toplamı tekil eser sayısına, **Mükerrer** toplamı PRISMA'daki mükerrer sayısına eşittir. Ayrıntılı tablo PRISMA raporunda, Analiz > Arama analizlerinin PRISMA kartında ve Excel raporunun **PRISMA_Sources** sayfasındadır.

**Taramaya kaynak ekleme:** Tarama sürerken (kaynaklardan indirme adımında) ya da bittikten sonra, adım panelinin altındaki **Kaynak ekle / yeniden indir** bölümünden taramada olmayan bir kaynağı seçip **Kaynak ekle**'ye basın. Kaynak aynı sorgu, yıl aralığı ve ölçütlerle aranır. İndirme sürüyorsa hemen başlar ve tarama onu da bekler; tarama bittiyse yalnızca yeni kaynak indirilir, ardından eserlerin eşitlenmesi, mükerrer kayıt taraması, eleme ve özet yenilenir (diğer kaynaklar yeniden aranmaz). PRISMA-S arama stratejisinde kaynağın eklenme zamanı yazar.

**Kaynağı yeniden indirme:** Bir kaynağın araması tamamlandığı hâlde sonuçlar eksik kaldıysa (ör. kaynak kesintiden sonra sonuçların sırasını değiştirdi), tarama bittikten sonra adım panelinin altındaki **Kaynak ekle / yeniden indir** bölümünden kaynağı seçip **Yeniden indir**'e basın. Kaynağın araması aynı sorguyla baştan indirilir: var olan kayıtların üzerine yazılır, eksik kalanlar eklenir, hiçbir kayıt silinmez; ardından eşitleme, mükerrer kayıt taraması, eleme ve özet yenilenir. Kaynağın kotası veya bütçesi (ör. OpenAlex) yeniden harcanır. O sırada başka bir literatür taraması sürüyorsa istek sıraya alınır: geçmiş listesinde "sırada" yazar ve süren tarama bitince kendiliğinden başlar; vazgeçerseniz **Sıradan çıkar**'a basın. PRISMA raporunda kaynağın ilk arama tarihi ile yeniden indirildiği tarih birlikte yazar.

**Yarım kalan kaynakları sürdürme:** Bir kaynak yarım kaldıysa, hata verdiyse veya arama durdurulduysa özetin üstünde **Yarım kalan kaynaklar** kutusu çıkar. Kaynağın yanındaki **Sürdür** (birden çoksa **Tümünü sürdür**) yalnızca o kaynağın aramasını kaldığı yerden devam ettirir. Literatür taraması sürerken yarım kalan bir kaynak (ör. çok fazla istek hatası) da işlem adımlarında, ilerleme çubuğunun altında hata metni ve **Sürdür** düğmesiyle görünür; arama indirme adımındaysa o kaynak arama durmadan sürdürülür; diğer kaynaklar yeniden aranmaz. Ardından eşitleme, mükerrer kayıt taraması, eleme ve özet yenilenir. Geçmiş literatür taramaları listesinden eski bir aramayı açıp da sürdürebilirsiniz.

Mevcut kayıtlar yeniden eklenmez: veritabanında zaten bulunan bir yayın gelirse kaydı güncellenir, bu aramaya bağlanır ve etiketi eklenir (bir kaydın kimliği ancak kaynaktan alındığında anlaşıldığı için indirmeden atlanamaz). Mükerrer kayıt taraması yalnızca bu aramayla yeni eklenen kayıtları tüm veritabanıyla karşılaştırır.

Hakkında penceresinde literatür taraması tek bir iş olarak, kaynak aramaları onun altında alt işlem olarak görünür. İndirme sürerken üst işin kalan süresi en yavaş kaynağın kalan süresidir.

Aynı anda yalnızca bir literatür taraması çalışır. Biri sürerken sayfa açıldığında form o aramanın sorgusu, ölçütleri ve başlatıldığı andaki sonuç sayılarıyla doldurulur; form arama bitene kadar kilitlidir.

Sayfadan ayrılabilirsiniz; arama arka planda sürer ve uygulama yeniden başlarsa kaldığı yerden devam eder. **Durdur** ile kesilebilir; o ana kadar alınan kayıtlar veritabanında kalır.

**Özet**, PRISMA 2020 akışının ilk adımlarını verir: veritabanlarında belirlenen kayıtlar, mükerrer olduğu için çıkarılanlar, otomasyonla uygun değil olarak işaretlenenler (nedenleriyle) ve taramaya alınacak eser sayısı. **PRISMA raporunu aç** ve **Raporu indir (.html)** düğmeleri akış şemasını, arama stratejisini (PRISMA-S), uygunluk ölçütlerini içeren yazdırılabilir raporu verir (tarayıcıda PDF olarak kaydedilebilir).

**Otomatik dışlanan eserler** listesi özetin altındadır: neden, başlık, yıl, dil ve tür gösterilir. Yanlış dışlanan eserleri işaretleyip **Seçilenleri dahil et** ile dahil edebilirsiniz; etiketleri ve sayılar güncellenir, karar sonraki sürdürmelerde korunur. Listenin tamamı **CSV indir** ile alınır. Etiket düğmesi Kütüphane'yi o etikete göre süzer.

## Kayıt listesi

Ortadaki bölmede veritabanındaki kayıtlar listelenir. Aynı yayının farklı kaynaklardan gelen kayıtları tek bir **eser** olarak gösterilir.

### Görünüm, arama ve filtreler

- **Kayıtlar / Tam metinler:** Tam metinler sekmesi yalnızca tam metni olan eserleri gösterir ve tam metinde de arar.
- **Eser ve kayıt sayısı:** Liste eser başınadır: aynı yayının farklı kaynaklardan veya aramalardan gelen kayıtları tek satırda (tek eser) gösterilir. Sayı satırı "589.180 eserden 1–20 gösteriliyor (972.114 kayıt)" biçimindedir; parantez içindeki tek tek kayıtların sayısıdır. Mükerrer kayıt taraması da tek tek kayıtlar üzerinde çalıştığı için orada bu büyük sayı görünür.
- **Kart / Tablo:** Tablo görünümünde kolonlar seçilebilir, genişlikleri sürüklenerek ayarlanır.
- **⤷ İç içe:** İlişkili yayınlar ana yayının altında girintili gösterilir: yayımlanmış makalenin altında ön baskısı, yeni sürümün altında eski sürüm, makalenin altında düzeltme, geri çekme ve endişe bildirimleri. Alt satıra tıklayınca o kayıt açılır; veritabanında olmayan alt yayın DOI / PMID bağlantısıyla gösterilir. Kayıt sayısının yanındaki düğmeyle kapatılabilir (deneme özelliği).
- **Sıralama:** Listenin üstündeki seçimle (eklenme, yıl, başlık, yazar, atıf).
- **Arama kutusu:** Başlık, yazar, dergi, DOI ve PMID'de arar. Sorgu dili:

| Yazım | Anlamı |
|---|---|
| `sepsis neonatal` | iki kelime birlikte |
| `sepsis OR septicemia` | kelimelerden biri |
| `sepsis NOT covid` veya `sepsis -covid` | bu kelime hariç |
| `"septic shock"` | ifade aynen |
| `child*` | önek araması |
| `title:` `author:` `journal:` `abstract:` `doi:` `tag:` | belirli alanda ara |
| `year:2016` `year:2016-2018` `year:>2017` | yıl |

- **Filtreler:** Soldaki **Filtreler** bölmesindedir (bkz. Kütüphane sayfası).
- **Eşleşme yeri:** Arama yapıldığında her sonucun altında kelimenin nerede bulunduğu (başlık, özet, tam metin…) ve işaretli parçalar görünür.

### Seçim ve silme

Kayıtların yanındaki kutularla seçim yapılır. **Sayfadakileri seç** görünen sayfayı, **Tümünü seç** filtreye uyan bütün kayıtları seçer. **Seçilenleri sil** eserleri (bütün kaynak kayıtları ve tam metinleriyle) siler; silinenler 30 gün boyunca **Admin > Bakım > Çöp kutusu**'ndan geri yüklenebilir.

### Kayıt ayrıntısı

Bir kayda tıklayınca sağda ayrıntısı açılır. Üstte başlık, tür, dil ve kaynak etiketleri; altında dört sekme vardır:

- **Bilgiler:** Yazarlar, dergi, tarih, DOI, PMID, konular, etiketler, atıf sayısı ve özet. **Etiketler** satırına yazıp Enter'a basarak kendi etiketlerinizi eklersiniz.
- **Tam metin:** Ekli dosyalar (aç, indir, kaldır) ve **PDF ekle**.
- **İlişkiler:** Ön baskı, düzeltme ve geri çekme bağları ile aynı eserin diğer kaynaklardaki kayıtları.
- **Aramalar:** Kaydın hangi aramalarla geldiği; tıklayınca liste o aramaya göre süzülür.

Başlığın yanındaki düğmeler:

- **✎ Düzelt:** Kaynaktan yanlış gelen alanları (başlık, yazar, yıl, dergi…) elle düzeltir. Düzeltmeler kaynak güncellense de korunur; kaynaktaki değer sonradan değişirse "kaynakta değişti" işareti çıkar.
- **⌘ Atıf ağı:** Bu makaleden başlayan bir atıf ağı kurar (bkz. Atıf ağı).

### Başlarken

Veritabanı boşken listede üç adımlı bir **Başlarken** kartı çıkar: kaynakları ayarlamak, ilk aramayı yapmak ve kayıtlarla çalışmak.

## İşlemler menüsü

Kayıt listesinin üstündeki **☰ İşlemler** menüsünün üç sekmesi vardır; menü **Araçlar** sekmesiyle açılır. İşlemler, kayıt seçtiyseniz seçilenlere, seçmediyseniz listedeki (filtreye uyan) kayıtlara uygulanır.

### Dışa aktar

| Biçim | Kullanım |
|---|---|
| BibTeX (`.bib`) | Zotero, JabRef, LaTeX |
| Zotero / RIS (`.ris`) | Zotero, Mendeley, EndNote |
| CSV (`.csv`) | Excel, Google E-Tablolar |
| CSL JSON (`.json`) | Zotero, Pandoc |
| EndNote XML (`.xml`), EndNote (`.enw`) | EndNote, RefWorks |
| Tam metinler (PDF, `.zip`) | Kayıtların ekli dosyaları |

Kendi etiketleriniz anahtar kelime olarak aktarılır; Zotero'da etiket olarak görünür.

**Kaynakça oluştur** APA 7, Vancouver, AMA, Harvard, IEEE ve Chicago stillerinde biçimlendirilmiş kaynakça hazırlar (Word ve EndNote'ta açılan RTF, HTML veya düz metin).

### İçe aktar

- **Metaveri dosyası:** Başka uygulamalardan dışa aktarılmış kayıtlar (BibTeX, RIS, PubMed/MEDLINE, PubMed XML, EndNote, Web of Science, Scopus CSV, CSV, CSL JSON). Aynı DOI veya PMID'li kayıtlar mevcut eserlere bağlanır.
- **PDF dosyaları:** Birden fazla PDF seçilebilir; içindeki DOI okunarak ilgili kayda bağlanır. Eşleşmeyenler listelenir.
- **Açık erişim tam metinlerini indir:** Tam metni olmayan eserler için açık erişim tam metin aranıp kayıtlara eklenir. Önce ücretsiz kaynaklar denenir: PMC, Europe PMC, veritabanındaki OpenAlex bağlantıları (istek gerektirmez), Unpaywall (e-posta girildiyse), CORE (anahtar girildiyse) ve (isteğe bağlı) Semantic Scholar. OpenAlex'e DOI ile sorgu, günlük bütçeden harcadığı için en sona kalır. İş arka planda sürer; kesilirse kaldığı yerden devam eder.

### Araçlar

- **Seçilenleri toplu düzelt:** Seçili kayıtların bir alanını tek seferde değiştirir (değeri yaz, bul–değiştir, listeye ekle, boşalt, kaynaktakine dön). Etiket eklemek için de kullanılır.
- **Eksik metadata ve özetleri tamamla:** Özeti, yazarı, yılı, dergisi veya DOI'si eksik kayıtların karşılıkları DOI/PMID ile diğer kaynaklarda aranır ve eksikler tamamlanır. Kaynaklar ücretsizden pahalıya sırayla denenir: PubMed, Crossref, Europe PMC, DOAJ, DataCite, OpenCitations, CORE (anahtar girildiyse), en son OpenAlex (günlük bütçe). ClinicalTrials.gov kayıtları makale olmadığı ve DOI / PMID ile tanımlanmadığı için veri tamamlamada kullanılmaz. Her adımdan önce eserin hâlâ eksiği olup olmadığına bakılır; ücretsiz kaynaklar tamamladıysa pahalı kaynağa o eser için istek gönderilmez.
- **Kimlik ve kurum bilgilerini tamamla:** Kayıt eklemeden eserlere bilgi ekler; adımlar seçilebilir:
  - **Eksik DOI / PMID / PMCID (NCBI PMC ID Converter):** Yalnızca PMC'deki makaleleri tanır. Eklenen kimlik, aynı kimliği taşıyan başka bir eser varsa onunla otomatik birleştirilir; böylece farklı kaynaklardan gelip ortak kimliği eksik olan aynı yayının kayıtları (ör. yalnızca PMCID'li bir Europe PMC kaydı ile yalnızca DOI'li bir Crossref kaydı) tek esere iner.
  - **Yazar ORCID'leri:** Yayını ORCID kaydına eklemiş yazarların ORCID iD'leri (DOI'si olan eserler).
  - **Kurumlar (ROR):** Kayıtlardaki kurum adresleri ROR kurum kimliklerine bağlanır; OpenAlex kayıtlarındaki ROR kimlikleri doğrudan alınır.
  Sonuçlar kaydın ayrıntısında **ORCID** ve **Kurumlar (ROR)** satırlarında, eklenen kimlikler "PMC ID Converter" notuyla görünür. İş arka planda çalışır (logo / Hakkında'da izlenir), kesilirse kaldığı yerden sürer.
- **Dil tespiti ve doğrulaması:** Kayıtların dili başlık ve özetten, uygulamanın içindeki dil modeliyle (fastText, 176 dil; internete bağlanmaz, veri dışarı gönderilmez) tespit edilir ve kaynağın bildirdiği dille karşılaştırılır. Kapsam işaretli eserler ya da listedekilerdir (eserlerin tüm kayıtları).
  - Kaynakta dil yoksa tespit edilen dil yazılır.
  - PubMed ve Europe PMC'nin bildirdiği yayın dili korunur; başlık ve özet başka dildeyse (ör. İngilizce başlık ve özetli Almanca makale) yalnızca **uyuşmuyor** olarak işaretlenir.
  - Diğer kaynaklarda (ör. dili kendi tahmin eden OpenAlex) metin yeterince uzun ve tahmin açıksa dil düzeltilir. İngilizce dışında bildirilmiş bir dil İngilizceye hiçbir zaman otomatik düzeltilmez (İngilizce başlık ya da özet çoğu zaman çeviridir); böyle kayıtlar yalnızca **uyuşmuyor** olarak işaretlenir.
  - **Çok dilli kayıtlar:** özet cümle cümle incelenir. Özette İngilizcenin yanında başka bir dil de belirgin yer tutuyorsa (ör. önce İngilizce, sonra Almanca özet) ya da başlık açıkça başka dilde, özet yalnızca İngilizceyse makale İngilizce sayılmaz; dili diğer dildir. Tek bir yabancı proje adı ya da yayıncı satırı çok dilli saymaz.
  - Sonuç kalıcıdır: kayıt kaynaktan yeniden gelse, eksik metadata tamamlansa ya da kayıtlar yeniden üretilse de tespit edilen dil sıfırlanmaz.
  - Dil tespiti yapılmış kayıtlara bir daha bakılmaz (hız için); her çalıştırmada yalnızca yeni gelen ve henüz bakılmamış kayıtlar işlenir. Tespit kuralları iyileştirildiğinde  eski kuralla değerlendirilmiş kayıtlar bir sonraki çalıştırmada bir kez yeniden değerlendirilir; elle seçilen diller değişmez.
  Sonuç kaydın ayrıntısında **Dil tespiti** satırında (tahmin edilen dil, olasılık, kaynağın dili, çok dilliyse dillerin payı) görünür; **Filtreler > İçerik > Dil tespiti** ile uyuşmayan, düzeltilen, tamamlanan, elle seçilen, tespit edilemeyen ya da henüz bakılmamış kayıtlar süzülür. Rapor ve elle düzeltme **Dil tespiti sayfası** sekmesindedir.
- **Kayıtlı aramaları güncelle:** Tamamlanmış aramaları yeniden çalıştırarak yeni yayınları ekler.
- **Aramaları yönet:** Admin > Aramalar'a götürür.
- **Taramalar ve atıf ağları:** İlgili sayfalara kısayollar.

## Araştırmalar

**Araştırmalar** menüsündeki araştırma sihirbazı, bir araştırmayı konudan makale taslağına kadar adım adım yürütür. 

- **Ekran düzeni:** Solda araştırma listesi ve adımlar, ortada seçili adım, sağda **Araştırmanın durumu**: kapsamda kalan eser sayısı, kaynaklar ve PRISMA aşamalarına göre sayılar (Tanımlama: mükerrerler; Tarama: geri çekilmiş, otomatik eleme, konu taraması, başlık-özet; Uygunluk: tam metin; Dahil etme) ve bekleyen işler (mükerrer öneri grupları, güncellenmeli adımlar). Sayılar adım tamamlandıkça ve karar verdikçe yenilenir; **↻ Yenile** ile elle de yenilenebilir.
- **Araştırma kümeleri:** Kütüphane'de sol bölmedeki **→ Araştırma kümesine ekle…** düğmesi, listede işaretlediğiniz (işaret yoksa filtreye uyan) eserleri adlandırılmış bir kümeye ekler ya da kümeden çıkarır. Birden çok küme oluşturabilirsiniz; kümeler genel etiketlerden ayrıdır ve Araştırmalar'da **Yayın kaynağı** olarak seçilir. Sol bölmedeki **Araştırma kümesi** süzgeci kümenin eserlerini gösterir.
- **Dil:** Araştırma metinleri (konu, başlık, soru, PICOTS, ölçütler) **İngilizce** yazılır; menüler Türkçe olabilir. Adım tamamlanırken dil denetlenir, İngilizce görünmeyen alan varsa adım tamamlanmaz (taslak kaydedilebilir).
- **Yeni araştırma:** **+ Yeni araştırma** ile bir ad verin; araştırma soldaki listeye eklenir ve sıradaki adımı gösterir. Üst menüden **Araştırmalar**'a gelince son açtığınız araştırma açılır.
- **Adımlar:** Sol tarafta aşamalara göre sıralıdır. Bir adım tamamlanmadan sonrakine geçilmez (🔒). Tamamlanan adımda ✓, taslak olarak kaydedilende ✎ görünür. Henüz hazır olmayan adımlarda "yakında" yazar; araştırma türünüzde uygulanmayan adımların üstü çizilidir.
- **Taslak olarak kaydet / Tamamla ve devam et →:** Taslak, adımı tamamlanmış saymaz. **Tamamla ve devam et** zorunlu alanları denetler ve sıradaki adımı açar.
- **Geri dönmek:** Tamamlanmış bir adıma tıklayıp değiştirebilirsiniz (**Değişiklikleri onayla**). Karar değişirse sonraki tamamlanmış adımlar **güncellenmeli** (!) olarak işaretlenir ve siz onları gözden geçirip yeniden onaylayana kadar akış orada durur. Kararı değiştirmeden yeniden onaylamak sonraki adımları etkilemez.
- **Yayın ayrıntısı:** Adımlardaki bir yayının başlığına tıklayınca Kütüphane'deki ayrıntısı sayfadan çıkmadan açılır: geniş ekranda adımın hemen sağındaki bölmede (sağdaki durum bölmesinin yerine), dar ekranda açılır pencerede. Başka bir başlığa tıklamak bölmeyi o yayına geçirir; **✕** ya da **Esc** kapatır, **Kütüphane'de aç ↗** yayını Kütüphane'de yeni sekmede açar. Ctrl (Mac'te ⌘) ile tıklarsanız yayın doğrudan yeni sekmede açılır.
- **Protokol özeti** protokol kararlarını, bağlı aramaları ve araştırmanın eser sayısını tek tabloda gösterir; **Karar geçmişi** hangi adımın ne zaman tamamlandığını ve değiştiğini listeler (yöntem bölümü ve PRISMA için). İkisi de her adımda sağ bölmede, **Araştırmanın durumu**'nun altındadır (tıklayınca açılır).
- **Yapay zekâ sonuçlarında güven:** sonuç tablolarında sayının yanındaki parantez, güveni %90 ve üstü olan önerilerin sayısıdır. Sayıya tıklayınca o kayıtların hepsi, parantezdeki sayıya tıklayınca yalnızca güveni %90 ve üstü olanlar açılır pencerede listelenir; pencerenin üstündeki süzgeçle **güven ≥ %90**, **güven < %90** ya da tüm güven düzeylerini (mükerrer bantlarında ayrıca öneri türünü: Aynı eser / Mükerrer değil / Kararsız / Önerisi yok) seçebilirsiniz. Toplu uygulamada da güven düzeyi üç seçeneklidir: güveni %90'ın altında kalanlara ayrı karar verebilirsiniz.

Hazır adımlar:

| Adım | İçerik |
|---|---|
| **a. Araştırma türü** | Derleme (sistematik derleme / meta-analiz) ya da özgün çalışma (prospektif / retrospektif). Tür, hangi adımların uygulanacağını belirler: başlık-özet ve tam metin taraması, PRISMA ve veri çıkarımı yalnızca derlemede |
| **b. Konu ve başlık** | Araştırma konusu, çalışma başlığı ve isteğe bağlı amaç |
| **c. Araştırma sorusu ve kapsam (PICOTS)** | Araştırma sorusu; Popülasyon, Müdahale / maruziyet, Karşılaştırma, Sonuç, Zaman, Ortam. **Dahil etme ve hariç tutma ölçütleri:** yıl aralığı, yayın dili (yalnızca **İngilizce**; seçenek yok: kaynağın bildirdiği dili İngilizce olmayanlar i. Otomatik elemede, kaynakta dili olmayanlar l. Dil tespitinde ayrılır), **yayın türü** (biçim: dergi makalesi, editöryal, bildiri, ön baskı…) ve **çalışma türü** (tasarım: anlatı / sistematik derleme, meta-analiz, RCT, kohort, vaka-kontrol, olgu sunumu…) için ayrı ayrı dahil et / hariç tut, geri çekilmiş yayınlar, yalnızca ön baskısı olanlar, özeti olmayanlar ve serbest metinli diğer ölçütler. En az soru, popülasyon (P) ve sonuç (O) zorunludur |
| **d. Yayın kaynağı seçimi** | Araştırmanın eserlerinin nereden geleceği: **Yeni literatür taraması**, **Mevcut aramalar** (daha önce birçok kaynakta tek seferde yapılan literatür taramaları, tek kaynak aramaları, dosyadan içe aktarmalar) ya da **Araştırma kümeleri** (Kütüphane'de işaretlediğiniz eser grupları). Birden fazlası seçilebilir. Yeni tarama seçilmezse anahtar kelime adımı atlanır |
| **e. Anahtar kelimeler ve arama sorgusu** | Yeni literatür taraması seçildiyse zorunlu; yalnızca mevcut kaynaklar seçildiyse **isteğe bağlı** (sonraki adımlar beklemez; terimler taramada vurgulanır, yapay zekâ önerisi alınabilir). Her kavram için eş anlamlı terimler ayrı satırlara yazılır: bir grubun terimleri OR, gruplar AND ile birleşir, birden çok kelimeli terim tırnağa alınır. Oluşan sorgu altta görünür (en fazla 500 karakter); **Sorguyu elle düzenle** ile değiştirilebilir. Aranacak alan: başlık ve özet, yalnızca başlık ya da tüm alanlar. Terimi olmayan adım açılınca kavram grupları önceki adımlardaki bilgilerden (konu, araştırma sorusu, PICOTS) yapay zekâyla kendiliğinden doldurulur; terimleri düzenleyip adımı kaydedin, **Geri al** önceki hâline döndürür. **Yapay zekâ ile öner** düğmesi yeni önerileri mevcut gruplarla birleştirir; MeSH başlıkları altta gösterilir (kayıt verisi gönderilmez) |
| **f. Aramalar** | **Yeni literatür taraması** bölümü kaynak başına kapsamı gösterir (kapsandı, eksik yıllar, bağlı arama yok); **Literatür taraması başlat →** formu araştırmanın sorgusu ve yıllarıyla, yalnızca kapsanmayan kaynaklar seçili açar, tarama araştırmaya kendiliğinden bağlanır (bu taramada eleme, etiket ve PRISMA kaydı yapılmaz; araştırmanın adımlarında yapılır). **Mevcut kümeleri bağla** bölümünde kaynak seçiminde işaretlediğiniz türler görünür: türü seçin, listeden işaretleyin, **Seçilenleri bağla**. Aramalar araştırmanın sorgusuna yakınlığa göre sıralıdır; bağlanan aramalar yeniden yapılmaz. Süren bir arama bitmeden adım tamamlanamaz. Bir literatür taramasını bağladıysanız içindeki tek aramaları ayrıca bağlamayın |
| **g. Aramaların birleştirilmesi** | Bağlı her kümeye bir rol verin: **Birleşim (dahil)** (eserleri araştırmaya girer), **Kesişim** (yalnızca bu kümede de bulunan eserler kalır), **Fark (çıkar)** (bu kümedeki eserler çıkarılır). Sonuç tablosu küme başına kayıt, eser, yalnızca o kümede olan ve sonuca giren eserleri; ayrıca veritabanına göre tanımlanan kayıtları, çıkarılan mükerrer kayıtları ve araştırmanın eser sayısını gösterir. Aynı yayının farklı kaynaklardaki kayıtları tek eser sayılır. Tamamlayınca sayılar PRISMA için saklanır ve **Araştırmanın … eserini Kütüphane'de göster →** bağlantısı açılır. Aramalar adımında bir değişiklik olursa bu adım **güncellenmeli** olur |
| **h. Veri tamamlama** | Üç alt adım yalnızca araştırmanın eserleri üzerinde ve sırayla çalışır: **Eksik metadata tamamlama**, **Kimlik tamamlama (PMC ID Converter)**, **Retraction Watch**. **▶ Sırayla çalıştır** tamamlanmamış ve atlanmamış alt adımları çalıştırır; kartlardaki **Çalıştır** tek alt adımı çalıştırır, **Atla** gerekmeyeni geçer. Tekrar kuralları: metadata 24 saat (hiçbir kaynakta bulunamayan eserler 30 gün), kimlik 7 gün içinde denenen eserleri yeniden sormaz; Retraction Watch dökümü günde bir indirilir. Hepsi tamamlanınca düğme **▶ Tümünü yeniden çalıştır** olur. Araştırmanın veri durumu (eksik özet / DOI / dil, dil tespiti yapılmamış kayıtlar, geri çekilmişler…) sağ bölmede **Veri durumu** başlığında. **Geri çekilmiş yayınlar:** Retraction Watch çalıştıktan sonra havuzdaki geri çekilmiş eser sayısı yazar; **Geri çekilmiş N eseri havuzdan dışla** ile onaylayınca dışlanır (PRISMA'da "geri çekilmiş yayınlar"; sonraki tamamlanmış adımlar güncellenmeli olur) |
| **i. Otomatik eleme** | Yalnızca derlemede. Veri tamamlamanın hemen ardından çalışır: havuz erken küçülür, mükerrer, ilişki ve dil adımları ile yapay zekâ işleri yalnızca kalan eserlerle çalışır. Protokoldeki ölçütler sırayla uygulanır: yıl aralığı → yayın dili → yayın türü → çalışma türü → yalnızca ön baskı → özet şartı. **Yayın dili:** kaynağın bildirdiği (ya da elle düzeltilmiş) dil İngilizce değilse eser **onay beklemeden** elenir; kaynakta dili olmayan eserler dışlanmaz, l. Dil tespiti adımında önce fastText, sonra yapay zekâyla değerlendirilir (burada elenenler o taramalara girmez). Ölçüt kutusu kaldırılırsa kaynağa göre dışlama l. adımında yapılır. **Yayın türü ve çalışma türü yalnızca kesin / güvenilir bilinen** eserlere uygulanır; türü belirsiz olanlar havuzda kalır; diğerleri dışlanmaz, "Elle karar gerekiyor" listesine düşer. Şelale grafiği her basamakta çıkarılan ve kalan eser sayısını gösterir (PRISMA için saklanır). Adım tamamlanınca **Eser kararlarını aç** düğmesi dışlanan, elle karar gerektiren ve dahil edilen eserleri açılır pencerede listeler; kararlar orada **Dahil et** / **Dışla** / **Otomatik karara dön** ile değiştirilebilir. Bu adımda not alanı yoktur. **Ölçütler bu adımda değiştirilebilir:** her ölçütün kutusu uygulanıp uygulanmayacağını, altındaki alanlar değerini belirler (yıl aralığı, yayın dili, yayın / çalışma türü dahil et / hariç tut tablosu, yalnızca ön baskılar, özeti olmayanlar). Protokol (c) değişmez; protokolden farklı olan ölçüt **protokolden farklı** etiketiyle işaretlenir ve **protokoldeki değere dön** ile geri alınır. Anahtar kelime süzgeci ve derlemelerin ayrılması bir sonraki adımda (m) |
| **j. Mükerrer kayıtlar** | Otomatik elemeden sonra. **Taramayı başlat** (tek tarama düğmesi) araştırmanın eserleri arasında mükerrer taraması yapar (kütüphane daha önce taranmışsa yalnızca yeni kayıtlar). Kesin eşleşenler (aynı başlık, yıl ve ilk yazar; dergi ve kimlik çelişmiyor) otomatik aynı esere bağlanır. Sonuç tablosu bağlanan grup sayısını ve kalan önerilerin benzerlik dağılımını gösterir. **Yapay zekâ önerileri** paneli kesin olmayan önerileri (yalnızca otomatik elemeden geçen eserlerin çiftleri) yapay zekâya sorar: iki kaydın başlığı, yılı, dergisi, cilt / sayı / sayfası, yayın türü, yazarları, DOI / PMID / PMCID'si, geldiği veritabanı ve özeti karşılaştırılır; öneri **Aynı eser**, **Mükerrer değil** ya da **Kararsız** olur. Sonuçlar başlık benzerliğine göre üç bantta sayılır: **%100**, **> %95** ve **≤ %95**. Her bant için ayrı karar verirsiniz: bandın satırında öneri türünü (Aynı eser / Mükerrer değil) ve güven düzeyini seçip **Uygula**; **Aynı eser** kayıtları tek esere bağlar ve mükerrer havuzdan çıkar, **Mükerrer değil** öneriyi kapatır. Bandın adına tıklayınca çiftler açılır pencerede listelenir (önerisi olmayanlar dahil). Kararsız ve önerisi olmayan çiftler uygulanmaz; uygulanmayan öneriler olduğu gibi kalır, adım karar beklemez. Bağ Mükerrer kayıtlar sayfasının Birleştirilmiş sekmesinden çözülebilir |
| **k. İlişkili kayıtlar** | **Taramayı başlat** yayın ilişkilerini tarar. Kesin ilişkiler (çok yüksek benzerlik; ön baskıda aynı ilk yazar) otomatik onaylanır, diğerleri olduğu gibi kalır. Protokole göre **geri çekilmiş yayınlar** ve **yayımlanmış sürümü olan ön baskılar** havuzdan otomatik çıkarılır (PRISMA'da ayrı kalemlerde sayılır). **Yapay zekâ önerileri** paneli kalan (kesin olmayan) ilişki önerilerini sorar; yalnızca otomatik elemeden geçen eserlere dokunan öneriler sorulur. **Yapay zekânın kullandığı ölçütler** başlığı altında ölçütler yazar: karar çalışmanın kendisine göre verilir (araştırma sorusu, tasarım, popülasyon, ortam, örneklem büyüklüğü, yöntem, sonuçlar; yazar listesinin ve başlığın değişmesi tek başına ret nedeni değil); ilişki türüne göre ayrıca: ön baskı → yayımlanmış sürümde önceki kayıt ön baskı, sonraki dergi yayını, aynı çalışma ve sonraki kayıt öncekinden eski değil; düzeltme / geri çekme / endişe bildiriminde sonraki kayıt tam olarak bu makaleyi anan bir bildirim. Öneri **Onayla** / **Reddet** (güven ve gerekçeyle); öneri türü, dayanak ve güvene göre toplu uygularsınız. Uygulanınca onaylanan ilişkilere göre havuz yeniden hesaplanır (ör. yayımlanmış sürümü bulunan ön baskı çıkarılır); havuz değişirse sonraki tamamlanmış adımlar güncellenmeli olur |
| **l. Dil tespiti** | Yalnızca İngilizce yayınlar kullanılır. Bu adıma yalnızca otomatik elemeden geçen ve **kaynakta dili olmayan** eserler girer (kaynağın bildirdiği dili İngilizce olmayanlar i. adımında onaysız elendi). **Taramayı başlat** bu eserlerin kayıtlarının dilini fastText ile başlık ve özetten tespit eder (Atla ile geçilebilir); ardından **Yapay zekâ önerileri** paneli bu kayıtları sorar: başlık İngilizce değilse dışlama, başlık İngilizce ama özet İngilizce değilse yine dışlama, ikisi de İngilizceyse tutma önerilir. **Sonuçlar ve sınıflara göre uygula** tablosunda her sınıfı (fastText'in sonucuyla birlikte) ayrı onaylarsınız: önerilen eylem (**Dışla** / **Tut**) seçili gelir, güven düzeyini seçip **Uygula** (Dil tespiti sayfası'ndeki kurallarla aynı). Önizleme kaynağa göre İngilizce olanları, kararı verilen (tutulan / dışlanan) ve karar bekleyen eserleri sayar; dışlananlar adım tamamlanınca PRISMA'da Dil kalemine yazılır, karar verilmeyen eser havuzda kalır. **Desteklenmeyen dil** bölümünde dışladığınız kayıtlar görünür, **Tümünü geri al** ile geri alınır. Dışlanan kayıt silinmez |
| **m. Konu özelinde tarama** | Yalnızca derlemede. Birleştirilen havuzun içinde konuya özgü çalışmaları ayırır — örneğin sepsis üzerine toplanan makaleler içinde sepsiste yapay zekâ çalışmaları. İlk kullanımda **Arama dizinini hazırla** ile aday eserlerin kayıtları bir kez dizine alınır (büyük araştırmada birkaç dakika; sonra **Dizini güncelle** yalnızca değişen kayıtları işler). Dizine ve aramaya yalnızca i.–l. adımlarından geçen eserler girer; kutuda aday eser ve kayıt sayısı ayrı görünür (bir eserin PubMed, Europe PMC, OpenAlex… gibi farklı kaynaklardan gelen kayıtlarının hepsi aranır, bu yüzden kayıt sayısı daha büyüktür). **Eşleşmesi gerekenler** kutusuna konu sorgusunu yazın (ör. `"artificial intelligence" OR "machine learning" OR "deep learning" OR "neural network*" OR AI`); isterseniz **e. adımdaki arama sorgusunu kopyala**. Sorgu her eserin **başlığında, anahtar kelimelerinde / MeSH ve konu terimlerinde, özetinde, dergi adında ve eklenmiş tam metninde** aranır (**Aranacak alanlar** ile daraltılabilir); eserin bütün kayıtları birlikte değerlendirilir. Kurallar: AND / OR / NOT, `"ifade"`, `*` ile kelime başı (`predict*`), alan önekleri (`title:`, `abstract:`, `keyword:`, `journal:`, `fulltext:`, `author:`, `year:`); kelimeler İngilizce kökleriyle eşleşir (network = networks), "AI" gibi kısaltmalar kelime olarak aranır ("pain" eşleşmez). **Eşleşirse dışla** ile konu dışı terimler (ör. `animal* OR mice`) çıkarılır. Özeti ve anahtar kelimesi olmayan eserler yalnızca başlıkla aranabildiği için ayrı sayılır; isterseniz dışlanmaz, elle karara bırakılır (o adımında listelenir). **Derlemeleri dışla** konu sorgusuna uyan derlemeleri (anlatı, sistematik, kapsam derlemesi, meta-analiz, bibliyometrik analiz…) ayırır; işaretlerden biri yeter: **kaynağın bildirdiği tür** (PubMed / Europe PMC yayın türü, OpenAlex, Scopus, Semantic Scholar), **başlıkta derleme ifadesi** (review, meta-analysis, scoping, umbrella, overview…; "chart review" gibi özgün çalışmalar sayılmaz), **derleme dergisi** (Cochrane Database of Systematic Reviews, "… Reviews") ve **özette derleme yöntemi** (PRISMA, "we searched…", "this review"). İlk ikisi varsayılan; dergi ve özet daha çok derleme bulur ama yanlış pozitif riski daha yüksektir (işaret başına sayılar sonuçta görünür). Sorguyu ve seçenekleri belirledikten sonra **Aramayı başlat**'a basın (Ctrl / ⌘ + Enter); sonra bir şeyi değiştirirseniz sonuç soluklaşır ve yeniden başlatmanız istenir. Sonuç bölümü sorguya uyan eser sayısını, şelaleyi, derleme sayılarını, eşleşmenin geçtiği yeri (başlık / anahtar kelime / özet / dergi / tam metin) ve örnek eşleşen / dışlanan eserleri gösterir. Dışlananlar PRISMA'da "otomatik araçla dışlanan" kalemine girer; kararlar **Dahil et** / **Dışla** / **Otomatik karara dön** ile değiştirilebilir |
| **n. Konu benzerliği (yapay zekâ)** | Yakında: İngilizce özetlerden araştırma sorusuna konu benzerliği (gömme modeli, GPU gerekmez) |
| **o. Son düzeltmeler** | Taramadan önceki son adım. Eksik bilgisi olan eserler sekmelerde (**Özeti yok**, **Yılı yok**, **Dili yok**, **DOI'si yok**); değeri yazıp **Kaydet**. **Elle karar gerekiyor** listesinde türü kesin olmayan eserleri (ve konu taramasında elle karara bırakılanları) isterseniz dahil edin ya da dışlayın (karar vermezseniz başlık-özet taramasında değerlendirilir). Kütüphane'de toplu düzeltme için **Tüm eserleri Kütüphane'de aç** |
| **p. Başlık ve özet taraması** | Yalnızca derlemede. Kalan eserler kart olarak tek tek gelir: başlık, yazarlar, dergi, DOI / PubMed bağlantısı ve özet (aramadaki ve konu sorgusundaki terimler vurgulu). **Dahil (D)**, **Belirsiz (B)** (tam metinde karar verilecek) ya da **Hariç (H)** — hariç için neden seçin (hazır nedenler ve protokoldeki hariç tutma ölçütleri). **Toplu karar** karar bekleyen tüm eserlere aynı kararı verir. Çok büyük kümelerde (5.000 üstü) uyarı gösterilir. **Yapay zekâ önerileri** bölümü: **Önerileri hazırla** (önce 10 eserle deneyin) karar bekleyen eserlere protokole göre Dahil / Belirsiz / Hariç önerisi, güven ve kısa gerekçe üretir (başlatmadan önce tahmini süre gösterilir; arka planda çalışır, durdurulup **Kaldığı yerden devam et** ile sürdürülür, uygulama yeniden başlarsa kendiliğinden devam eder, günlük sınıra gelince durur). Öneri kartta görünür; **Öneriyi uygula (A)** ile ya da **Önerileri toplu uygula** (ör. güveni %90 ve üstü Hariç önerileri) ile alınır. Uygulanan öneri sizin kararınız olarak, notunda model adıyla kaydedilir; yapay zekâ kendi başına karar vermez. Yapay zekâ önerisi her eserin **çalışma türünü** de söyler (kartta görünür); protokolde dahil / hariç çalışma türleri seçildiyse model buna göre Hariç (çalışma tasarımı) önerebilir |
| **r. Tam metinlerin bulunması** | Başlık-özette dahil ya da belirsiz kalan eserlerin tam metinleri (taramadan önce binlerce eserin tam metnini indirmek saatler ve onlarca GB sürdüğü için bu adım başlık-özetten sonradır). Bağlanan tam metinler Kütüphane'de, tam metin taramasında ve sonraki analiz adımlarında kullanılır. **1. Açık erişim:** **Açık erişim tam metinleri indir** PMC, Europe PMC ve OpenAlex'i (Ayarlar > Tam metin'de açıksa Unpaywall, Semantic Scholar) sırayla dener; büyük kümelerde saatler sürebilir, iş arka planda sürer. Her PDF tam metin deposunda yer kaplar (ortalama 1–3 MB). **2. Dosyaları eşleştir:** Erişemediğiniz makalelerin PDF'lerini bir klasörde toplayın; **Klasör seç…** (klasördeki bütün PDF'ler), **Dosya seç…** ya da sürükle-bırak ile gönderin. İsterseniz dosyaları bilgisayarınızdaki kayıt klasörünün altındaki `tam-metinler/arastirma-<no>` klasörüne koyup **Kayıt klasörünü tara**'ya basın (oradaki dosyalar silinmez). Her dosya DOI, PMID / PMCID, ilk sayfadaki başlık, PDF'teki başlık bilgisi ve dosya adıyla bu araştırmanın eserleriyle eşleştirilir. **Kesin eşleşmeleri hemen bağla** işaretliyse kimlik ve ilk sayfadaki başlık eşleşmeleri hemen bağlanır; dosya adı ve benzer başlık eşleşmeleri **Önerilen eşleşmeler** sekmesinde **Bağla** ile (ya da **Önerilen … eşleşmeyi bağla** ile hepsi birden) onaylanır. Yanlışsa **Başka kayıt seç** ile başlıkta arayıp doğru eseri seçin; **Eşleşmeyen dosyalar** için **Kayıt seç**. **Listeden çıkar** dosyayı listeden kaldırır. **3. Eserler:** **Eksik** / **Tam metni olan** / **Erişilemedi** sekmeleri, başlık / DOI araması, satırda **PDF yükle** ya da **Erişilemedi** (geri almak için **Geri al**) ve **Tam metni olmayanların listesini indir (CSV)** (kütüphaneden ya da yazarlardan istemek için). Adımı tamamlamak için her eserin tam metni ya da "erişilemedi" işareti olmalı ve önerisi bağlanmamış dosya kalmamalı |
| **s. Tam metin taraması** | Yalnızca derlemede. Tam metni olan eserler aynı kartla; **Tam metni aç**, **Dahil** ya da **Hariç** (nedenle). Konuyla ilgili derlemeler "Konuyla ilgili derleme" nedeniyle ayrılır. **Çalışma türü son kontrolü:** kartta başlık-özette yapay zekânın bulduğu tür ve kaynağın güvenilir türü gösterilir; protokolün çalışma türü ölçütüne uymuyorsa uyarı çıkar |
| **t. Analizler ve PRISMA akış şeması** | PRISMA 2020 akış şeması önceki adımların sayılarından, dört aşamada: **Tanımlama** — veritabanlarına göre tanımlanan kayıtlar ve taramadan önce kalem kalem çıkarılanlar: mükerrer kayıtlar (g. birleştirmedeki kesin eşleşmeler ve j. adımındaki kararlar), geri çekilmiş yayınlar (h / k), dil (İngilizce dışı: kaynağın bildirdiği dile göre i. otomatik elemede, kaynakta dili olmayanlar l. dil tespitinde), ön baskılar (yalnızca ön baskı i'de, yayımlanmış sürümü olan k'de), tarih (yayın yılı aralığı dışında), çalışma türü ve yayın türü (i. otomatik eleme, güvenilir üst veriyle), özeti olmayanlar ve arama kümesi işlemleri. **Tarama** — kalan kayıtlar başlık ve özetle elenir: m. konu özelinde tarama (ikincil arama, derlemeler dahil, nedenleriyle) ve p. başlık-özet taramasında dışlananlar (nedenleriyle). **Uygunluk** — erişimi istenen / erişilemeyen raporlar, tam metni değerlendirilen / nedenleriyle dışlanan raporlar. **Dahil etme** — derlemeye dahil edilen çalışmalar. Şemanın yanındaki **ⓘ** simgesinin üzerine gelince (ya da klavyeyle odaklanınca) aşamaların kuralları ve hangi adımın hangi aşamaya yazıldığı listelenir. **SVG indir**, **Sayıları CSV indir** (aynı aşamalarla) | Aynı şema her adımda sağ bölmedeki **PRISMA akış şeması** düğmesiyle açılır ve adımlar ilerledikçe dolar (sayısı henüz belli olmayan kutular kesik çizgili, "n = …"); şema metinleri İngilizcedir. Sağ bölmedeki sayılar da aynı dört aşamaya göre gruplanır.

Sonraki adımlar (veri çıkarımı, referans havuzu ve yazım) sırayla eklenecek.

### Dil tespiti sayfası

Araştırmanın **Dil tespiti** adımındaki **Kayıtları tek tek incelemek için Dil tespiti sayfası ↗** bağlantısıyla açılır; dil tespitinin raporunu ve elle düzeltmeyi içerir. Tespitin kendisi araştırmanın adımında ya da Kütüphane > İşlemler > Araçlar'dan çalıştırılır. Rapordaki sayılar eser sayısıdır; altında kayıt sayısı yazar:

- **Kaynak dili ile tespit edilen dil farklı:** kaynağın bildirdiği dil ile başlık ve özetten tespit edilen dilin farklı olduğu eserler (kaynakta dili boş olanlar hariç). Bunlardan kaynak dili korunanlar **karar bekleyen** olarak ayrıca sayılır.
- **Dil tespit edilemedi:** başlık / özet çok kısa ya da tahmin zayıf.
- **Düzeltme yapıldı:** dili otomatik düzeltilen ve elle seçilen eserler.
- **Dili tamamlandı:** kaynakta dili olmayıp tespit edilen dilin yazıldığı eserler.
- **Çok dilli:** özeti ya da başlığı birden çok dilde olan eserler (İngilizce kısım çeviri sayılır).
- **Dil tespiti bekleyen kayıt:** henüz bakılmamış kayıtlar ve tespit kuralları iyileştirildikten sonra yeniden değerlendirilecek kayıtlar (dil tespitini yeniden çalıştırınca güncellenir).

Bir sayıya tıklayınca aşağıdaki liste o kayıtlara geçer. Liste sekmeleri: **Karar bekleyen farklılar**, **Tüm farklılar**, **Tespit edilemeyenler**, **Otomatik düzeltilenler**, **Elle seçilenler**, **Dili tamamlananlar**, **Çok dilli**; başlıkta arama yapılabilir. Her satırda kaynağın dili, tespit edilen dil (olasılıkla), kaydın şu anki dili ve durumu görünür; başlığa tıklayınca kayıt Kütüphane'de açılır.

**Elle düzeltme:** satırdaki **Kaynak dili** ya da **Tespit edilen** ile, veya işaretlenen kayıtlar için üstteki **Kaynak dilini kullan**, **Tespit edileni kullan** ya da dil kodu yazıp **Uygula** (ör. `tr`, `de`, `fra`) ile kaydın dili seçilir. Elle seçilen dil kalıcıdır: kaynaktan güncelleme, yeniden üretim, metadata tamamlama ve dil tespiti onu değiştirmez. **Otomatik sonuca dön** elle seçimi geri alır.

**Desteklenmeyen dil:** yalnızca İngilizce yayınlar desteklenir; Türkçe dahil diğer diller desteklenmez. Raporun altındaki bölüm **kaynağın bildirdiği dili İngilizce olmayan** kayıtları dile göre sayar; **Dışla** ile tek onayla uygularsınız (araştırmalarda bu, i. Otomatik elemede onay beklemeden yapılır).

Dışlanan kayıt silinmez: "desteklenmeyen dil" olarak işaretlenir, **Kütüphane**'de varsayılan olarak gizlenir (**Filtreler > İçerik > Desteklenmeyen dildekileri gizle**; yalnızca onları görmek için **İlişki** süzgecinde **Desteklenmeyen dil (dışlanan)**), araştırmalarda l. Dil tespiti aşamasında elenir. Açıklamaya tıklayınca kayıtlar açılır pencerede listelenir (başlık Kütüphane'de açılır); **Listede gör** ile kayıtlar aşağıdaki listede açılır (sekmeler **Dışlama adayları**, **Dışlananlar**); dışlananlar listesinde satırdaki **Geri al** ya da bölümdeki **Tümünü geri al** işareti kaldırır.

**Yapay zekâ önerileri:** **Önerileri hazırla** yalnızca **kaynakta dili olmayan** kayıtlar (fastText'in dilini tamamladığı ya da tespit edemediği) için yapay zekâya **başlığın ve özetin dilini ayrı ayrı** sordurur (fastText'in tahmini söylenmez). Kaynağın bildirdiği dil varsa karar ona göredir, yapay zekâya sorulmaz. Kurallar:

- Başlık İngilizce değil → **Dışla** önerisi.
- Başlık İngilizce, özet İngilizce değil → **Dışla** önerisi.
- Başlık ve özet (varsa) İngilizce → **Tut** önerisi.

Öneriler fastText'in sonucuyla birlikte sınıflanır (ör. "başlık İngilizce değil, fastText de İngilizce dışı diyor", "başlık ve özet İngilizce, fastText tespit edemedi") ve karar dayanağına göre (**Yalnızca başlıktan** / **Başlık ve özetten**) sayılır. **Sonuçlar ve sınıflara göre uygula** tablosunda her sınıfın satırında önerilen eylem (**Dışla** / **Tut**) seçili gelir; isterseniz değiştirip güven düzeyini (güven ≥ %90 ya da tüm güven düzeyleri) seçer ve **Uygula**'ya basarsınız. **Tut** kaydın dilini İngilizce olarak kaydeder (elle dil kararı gibi), **Dışla** desteklenmeyen dil olarak işaretler. Yapay zekânın dili belirleyemediği kayıtlarda eylemi siz seçersiniz. Listede her kaydı tek tek de tutabilir ya da dışlayabilirsiniz. Panelde yapay zekâya gönderilmeyen kayıtların sayısı (kaynak dili İngilizce / İngilizce dışı, elle seçilen, dışlanan) yazar; yarım kalan görev **Kaldığı yerden devam et** ile sürer. Sorulma sırası: fastText'in tespit edemedikleri, sonra dilini tamamladıkları (düşük olasılıklılar önce); **Öncelikliler** ilk grubu çalıştırır.

### Mükerrer kayıtlar sayfası

Araştırmanın **Mükerrer kayıtlar** adımından (yalnızca araştırmanın önerileri), literatür taramasının özetindeki **İncele →** bağlantısından ya da Kütüphane > İşlemler > Araçlar'dan açılır.

Aynı yayın farklı kaynaklardan (veya aynı kaynaktan farklı kimlikle) gelebilir.

- **Otomatik bağlama:** DOI, PMID veya PMCID'si aynı olan kayıtlar kaydedilirken kendiliğinden tek esere bağlanır.
- **Mükerrer kayıtları tara:** Başlığı, yılı ve yazarı benzeyen kayıtları bulup **Önerilen** sekmesinde sunar. Tarama yalnızca bu düğmeyle çalışır. Büyük veritabanında tarama uzun sürebilir (yaklaşık bir milyon kayıtta 50 dakika kadar); ilerleme çubuğunda kalan süre gösterilir. Uygulama bu sırada yeniden başlarsa (güncelleme, bilgisayarın uykuya geçmesi) tarama baştan değil, kaldığı yerden sürer. Tarama sürerken eklenen kayıtlar bir sonraki taramaya kalır. **Durdur** ile kesilen taramada önceki öneriler korunur.
- **İlişkili yayınlar önerilmez:** Aralarında bir ilişki bulunan yayınlar (ön baskı ile yayımlanmış sürümü, bir makale ile düzeltme, geri çekme veya endişe bildirimi, aynı yayının sürümleri) başlıkları benzese de ayrı yayınlardır; **İlişkiler** sayfasında gösterilir, mükerrer olarak önerilmez. İlişki **İlişkiler** sayfasında reddedilirse çift yeniden mükerrer önerisi olabilir.
- **Kimliği çelişen ve yaygın başlıklı çiftler önerilmez:** İki eserin de DOI'si var ve farklıysa (ya da PMID'leri farklıysa) bunlar ayrı yayınlardır (ör. bir derginin aynı başlıklı farklı olgu bildirimleri, İngilizce ve İspanyolca baskıları); önerilmez. Başlığı çok sayıda farklı eserde geçen kayıtlar (yer tutucu başlıklar, "Editorial" gibi) yalnızca yıl ve ilk yazar da aynıysa önerilir. Önerilmeyenlerin sayısı listenin üstünde yazar.
- **Kesin eşleşme:** Başlığı (en az 40 karakter), yılı ve ilk yazarı birebir aynı, dergisi ve sayfası çelişmeyen gruplar **✓ Kesin eşleşme** etiketi taşır. **Yalnızca kesin eşleşenler** ile süzülür, **✓ Kesin eşleşenleri bağla** ile tek onayla topluca bağlanır (arka planda; yanlış bir bağ Birleştirilmiş sekmesinden çözülebilir).
- **Aynı anda tek tarama:** Mükerrer kayıt taraması, ilişki taraması ve toplu bağlama / onaylama işleri birbirinin sonucunu kullandığı için aynı anda çalışmaz. Biri sürerken diğeri başlatılırsa uyarı verilir; kendiliğinden başlayanlar (ör. literatür taramasının mükerrer adımı) sırasını bekler.
- Her öneri kartında eserler yan yana gösterilir; **Aynı eser olarak bağla** ya da **Mükerrer değil** seçilir. DOI'leri veya dergileri farklı gruplar ⚠ ile uyarılır.
- **Toplu işlem:** Kartlardaki **Seç** kutuları, **Sayfadakileri seç**, **Seçilenleri bağla** ve **Seçilenler mükerrer değil**.
- **Birleştirilmiş** sekmesinde bağlar tek tek çözülebilir; kayıtlar hiçbir zaman silinmez.
- **Yapay zekâ önerileri (Önerilen sekmesi):** yalnızca kesin olmayan öneriler (gri bölge) için yapay zekâ iki kaydı (başlık, yazarlar, dergi, yıl, DOI, özet) karşılaştırıp **Aynı eser** ya da **Mükerrer değil** önerir; öneri kartın üstünde güven ve gerekçeyle görünür. Kesin eşleşenler sorulmaz. Araştırmadan açılan sayfada (Araştırmalar > j. Mükerrer kayıtlar) yalnızca o araştırmanın otomatik elemeden geçen eserleri sorulur. Öneri karar değildir: satırdaki **Öneriyi uygula** ile tek tek ya da **Önerileri toplu uygula** ile (öneri türü ve güven düzeyine göre, ör. güveni %90 ve üstü olanlar) uygulanır. Başlatmadan önce tahmini süre gösterilir (öğe başına süre bu bilgisayarda ölçülür). Görev arka planda çalışır ve kaldığı yerden sürer: **Durdur** ile durdurup sonra **Kaldığı yerden devam et** ile sürdürebilirsiniz; uygulama ya da bilgisayar yeniden başlarsa görev yarıda kalır ve kendiliğinden sürmez (yerel model açılışta kendiliğinden yüklenip bilgisayarı zorlamasın; panelde uyarı çıkar, **Kaldığı yerden devam et** ile sürdürülür), Ollama kapalıysa 1 saate kadar bekler, günlük istek sınırına gelince durur. Gece ya da bilgisayarı kullanmadığınız saatlerde çalıştırmanız önerilir; önce 20 öğeyle deneyin. Sonuçlar karar ve karar dayanağına (yalnızca başlıktan / başlık ve özetten) göre tabloda sayılır; toplu uygulama başlık benzerliği bantlarına göredir (%100, > %95, ≤ %95; her bant ayrı karar). Sağlayıcı **Admin > Ayarlar > Yapay zekâ > Görev yönlendirme**'de seçilir. **Aynı eser** önerilerini toplu uygulamadan önce birkaç kartı denetleyin (bağ Birleştirilmiş sekmesinden çözülebilir).

### İlişkiler sayfası

Kütüphane > İşlemler > Araçlar'daki **İlişkileri tara** kısayolundan ya da kaydın ayrıntısındaki ilişkiler bölümünden açılır.

Ön baskı ↔ yayımlanmış sürüm, düzeltme (erratum), geri çekme ve endişe bildirimleri gibi yayınlar arası bağlar burada izlenir. Mükerrer olarak aynı esere bağlanmış kayıtlar arasındaki ilişki burada (sayılarda, kayıt listesinin **İlişki** süzgecinde ve işaretlerde) gösterilmez: bunlar aynı yayındır. Bağ **Mükerrer kayıtlar > Birleştirilmiş** sekmesinde çözülürse ilişki geri gelir. Bir yayın geri çekildiyse, bildirimiyle aynı esere bağlansa da **geri çekilmiş** olarak işaretli kalır.

- Kaynakların bildirdiği bağlar doğrudan **İlişkiler** sekmesine gelir; **İlişkileri tara** başlık benzerliğiyle olası bağları **Öneriler** sekmesine ekler. Tarama artımlıdır: yalnızca son taramadan sonra eklenen ya da değişen kayıtlar taranır. Tüm kayıtları baştan taramak için **Tüm kayıtları yeniden tara** kutusunu işaretleyin. İlişki grupları en eski yayının yılına göre (yeniden eskiye) sıralanır.
- Öneriler tek tek ya da toplu (**Sayfadakileri seç**, **Seçilenleri onayla / reddet**) onaylanır.
- **Yapay zekâ önerileri (Öneriler sekmesi):** yapay zekâ iki kaydı (başlık, yıl, dergi, yayın türü ve ön baskı işareti, yazarlar, cilt / sayı / sayfa, kimlikler, özet ve başlık benzerliği) ilişki türüne özgü ölçütlerle (panelde **Yapay zekânın kullandığı ölçütler**) karşılaştırıp ilişkinin doğru olup olmadığını (**Onayla** / **Reddet**) önerir; öneri ilişkinin yanında güven ve gerekçeyle görünür. Ön baskı ile yayımlanmış sürümde yazar listesinin değişmesi olağandır; karar daha çok çalışmanın kendisine (soru, tasarım, örneklem, sonuçlar) göre verilir. Öneri karar değildir: satırdaki **Öneriyi uygula** ile tek tek ya da **Önerileri toplu uygula** ile (öneri türü ve güven düzeyine göre, ör. güveni %90 ve üstü olanlar) uygulanır. Başlatmadan önce tahmini süre gösterilir (öğe başına süre bu bilgisayarda ölçülür). Görev arka planda çalışır ve kaldığı yerden sürer: **Durdur** ile durdurup sonra **Kaldığı yerden devam et** ile sürdürebilirsiniz; uygulama ya da bilgisayar yeniden başlarsa görev yarıda kalır ve kendiliğinden sürmez (yerel model açılışta kendiliğinden yüklenip bilgisayarı zorlamasın; panelde uyarı çıkar, **Kaldığı yerden devam et** ile sürdürülür), Ollama kapalıysa 1 saate kadar bekler, günlük istek sınırına gelince durur. Gece ya da bilgisayarı kullanmadığınız saatlerde çalıştırmanız önerilir; önce 20 öğeyle deneyin. Sonuçlar karar ve karar dayanağına (yalnızca başlıktan / başlık ve özetten) göre tabloda sayılır; toplu uygulama başlık benzerliği bantlarına göredir (%100, > %95, ≤ %95; her bant ayrı karar). Sağlayıcı **Admin > Ayarlar > Yapay zekâ > Görev yönlendirme**'de seçilir.
- Geri çekilmiş yayınlar kayıt listesinde kırmızı zemin ve **Retracted** etiketiyle işaretlenir; ayrıntıda uyarı çıkar.
- **Retraction Watch'u güncelle:** Crossref'in açık yayımladığı Retraction Watch veritabanını (geri çekilen, düzeltilen ve hakkında endişe bildirimi yayımlanan yayınlar; yaklaşık 70 MB) indirir ve veritabanındaki kayıtlarla DOI / PMID ile eşleştirir. Eşleşenler kaynakları ne olursa olsun (PubMed, OpenAlex, Scopus, içe aktarma…) **İlişkiler** sekmesine "Retraction Watch" kaynağıyla eklenir, geri çekilenler işaretlenir. Veri indirildikten sonra her yeni aramanın kayıtları kendiliğinden eşleştirilir; literatür taramasındaki **Geri çekilmiş yayınları dışla** ölçütü de bu bilgiyi kullanır. Veri günlük güncellenir; ara sıra (ör. ayda bir) güncellemek yeterlidir. Döküm son 24 saatte indirildiyse yeniden indirilmez, yalnızca kayıtlarla eşleştirilir; yeniden indirmek için **Dökümü yeniden indir** kutusunu işaretleyin.

## Atıf ağı

Atıf ağı, bir makaleyi merkeze alıp kaynakçasındaki (geriye) ve ona atıf yapan (ileriye) makalelere doğru genişler. Sistematik derlemelerdeki atıf taraması (citation searching / snowballing) yöntemine uygundur.

1. Kayıt listesinde makalenin ayrıntısındaki **⌘ Atıf ağı** düğmesine basın ve yönü seçin.
2. **Atıf ağı** sayfasında merkezdeki makale ve bağlı makaleler dairesel bir ağda ve tabloda gösterilir. Bir makaleye tıklayınca ona geçilir; **Kaynakçasını getir** / **Atıf alanları getir** ile bir alt düzey eklenir.
3. Tablodan makaleler için **Dahil / Belki / Hariç** kararı verilir; seçilenler **Veritabanına ekle** ile kayıt listesine alınır.
4. **Prosedür kaydı (CSV)** her adımı (tarih, yön, bulunan, yeni) içerir; PRISMA 2020 ve PRISMA-S raporlaması için kullanılır.

Veriler OpenAlex'ten alınır; her istek günlük bütçeden çok küçük bir pay düşer.

## Analiz

### Arama analizleri

Sistematik derleme aramalarınızın kaydını tutar ve PROSPERO / PRISMA için gereken metinleri, sayıları, tabloları ve grafikleri üretir. Sayfaya girildiğinde analiz kendiliğinden başlamaz: önce neyin raporlanacağını seçip **Analizi başlat**'a basarsınız. İki çalışma biçimi vardır:

- **Analiz edilecek aramalar** listesinde literatür taramaları ve aramalar birlikte, en yenisi üstte, türü (LİTERATÜR TARAMASI / ARAMA), kayıt sayısı ve tarihiyle listelenir; arama kutusunun altındaki **Göster** seçim kutularıyla türlerden biri gizlenebilir. Bir ya da **birden çok** arama ve literatür taraması işaretleyin; seçilenler **tek analizde** birleştirilir (aynı yayının kayıtları bir kez sayılır). Liste **Aramalarda süz** kutusuyla daraltılır, **Seçimi temizle** ile sıfırlanır; seçim tarayıcıda hatırlanır.
  - **Aynı sorgunun diğer aramalarını da kat** işaretliyse seçilen her aramanın aynı sorgudaki diğer kaynak ve yıllardaki aramaları (ör. yıl yıl yapılan aramalar) ve aynı kaynak ve yılda onu VE ile daraltan sorgular (ör. `sepsis AND "machine learning"`) da rapora girer.
  - Literatür taramasında uygunluk ölçütleriyle dışlanan eserler PRISMA sayılarında ayrıca gösterilir.
  - Literatür taraması bitince özetin altındaki **Arama analizi** bağlantısı o aramayı seçip analizi doğrudan başlatır.
Seçilen aramalar için sayfada sırasıyla şunlar görünür:

- **PROSPERO: arama stratejisi metinleri:** PROSPERO kayıt formunun "Searching and screening" alanlarına uygun İngilizce metinler (kopyalanabilir).
- **Özet kartları** ve **Kaynaklara göre sorgu ilerlemesi:** temel sorgu, çalıştırılan, hata veren, sınıra ulaşan ve kesilen sorgular, alınan kayıt.
- **PRISMA belirleme sayıları:** belirlenen kayıt, mükerrer, tekilleştirmeden sonra, ölçütle dışlanan, başlık/özet taramasına kalan, kesilen sorgular.
- **Kaynaklara göre arama ve mükerrer eleme** ve **Kaynaklar arası örtüşme** tablolarının altındaki **Kolon açıklamaları** her sayının ne anlama geldiğini anlatır (aynı açıklamalar Excel raporunda; ilk tablonunkiler PRISMA raporunda da vardır). **Kalan** ile **Yalnızca bu aramada** farklıdır: kalan, tabloda daha önceki aramalarda bulunmayan eserlerdir ve toplamı havuzu verir; yalnızca bu aramada, diğer aramaların hiçbirinde bulunmayan eserlerdir.
- **Grafikler**, **Sorgu durumu** ısı haritası, **Kaynaklar arası örtüşme** ve **Sorgu günlüğü** (süzülebilir; **CSV indir**).

**Rapor (Excel)**, **Sorgu günlüğü (CSV)**, **PROSPERO metinleri (.txt)** ve **Yazdır / PDF** ile rapor dışa aktarılır.

### Yayın türleri analizi

**Analiz > Yayın türleri** veritabanındaki eserleri Kasghar'ın tür gruplarına göre sayar ve her türün hangi kaynaklardan geldiğini gösterir.

## Admin

Admin sayfasının tüm bölümleri soldaki tek menüdedir. Menü dört başlık altında toplanır: **Ayarlar** (Genel, Performans, Kaynaklar, Yayın türleri, Yapay zekâ), **Aramalar**, **Yedekleme** ve **Bakım** (Tablo bakımı, Çöp kutusu, Tam metin deposu, Testler, Fabrika ayarlarına dön). **Kaynaklar** ve **Yapay zekâ**nın alt bölümleri (her kaynağın ayarı, Sağlayıcı ekle) bu başlıklara basınca açılır. En alttaki **Günlük →** ayrı Günlük sayfasını açar; Günlük sayfasından **Admin** bağlantısıyla geri dönülür. Telefonda menü, seçili bölümün adını gösteren düğmeyle (☰) açılır.

### Aramalar

Arama geçmişi gruplar hâlinde listelenir:

- **Literatür taramaları:** her tarama (ör. #2 “septic”) tek satırda: kaynak sayısı, toplam kayıt, başlangıç zamanı, durum ve **Analiz →** bağlantısı. Satıra basınca taramanın kaynak aramaları açılır.
- **Tek kaynaklı aramalar:** bir literatür taramasına bağlı olmayan aramalar.
- **Eksik metadata tamamlama aramaları:** veri tamamlamanın diğer kaynaklardan aldığı kayıtlar. Her çalıştırma kaynak başına bir iç arama açar ve çalıştırma zamanıyla gruplanır.

Bir arama silinirken varsayılan olarak **yalnızca o aramayla gelen kayıtlar** da silinir; başka aramalarla da gelen kayıtlar korunur. Silinenler çöp kutusuna gider.

### Yedekleme

- **Yedek al:** Veritabanının tamamı ve isteğe bağlı olarak tam metin dosyaları ile ayarlar tek bir `.zip` dosyasına yazılır. Dosyayı uygulamanın dışında güvenli bir yere kopyalayın. Araştırmalardaki konu taramasının arama dizini yedeğe girmez (geri yüklemeden sonra **Dizini güncelle** ile yeniden hazırlanır).
- **Geri yükle:** Seçilen yedek mevcut verinin yerine geçer. Önce mevcut durumun güvenlik yedeği otomatik alınır.
- **Tam metin dosyaları yedeği:** Yalnızca depodaki dosyalar ve hangi kayda ait olduklarını gösteren bir liste.

Yeni bir bilgisayara taşımak için: Kasghar'ı orada kurun, Admin > Yedekleme > Geri yükle ile `.zip` dosyasını seçin.

### Bakım

- **Tablo bakımı:** Silinen kayıtların bıraktığı boşluğu temizler ve aramaların hızlı kalmasını sağlar. Büyük silme ve içe aktarmalardan sonra kendiliğinden çalışır.
- **Çöp kutusu:** Silinen eserler, aramalar ve atıf ağları 30 gün saklanır; **Geri yükle** ile eski hâline döner.
- **Tam metin deposu:** Depoyu denetle (kayıp ve bağlantısız dosyalar) ve temizlik.
- **Testler:** Uygulamanın kendi kendini denetlemesi; veriyi değiştirmez. Bir güncellemeden sonra veya bir sorun fark ettiğinizde çalıştırın.
- **Fabrika ayarlarına dön:** **DB verilerini sil** indirilmiş kayıtları siler, ayarları korur; **Tüm verileri sıfırla** ayarlar dahil her şeyi siler. Her ikisinden önce tam güvenlik yedeği alınır; onay için `SIFIRLA` yazılır.

### Günlük

Silinen kayıtlar, önemli işlemler ve hatalar, indirme hataları ve taramalar burada tarih sırasıyla tutulur.

## Sık sorulan sorular

**Arama "çok fazla istek" (429) hatası veriyor.**
Kaynağın hız sınırına takıldınız. Kaynak ne zaman yeniden deneneceğini bildirmediyse Kasghar sınırı zorlamamak için tekrar denemez: arama **yarım kaldı** olarak durur, alınan kayıtlar korunur; bir süre sonra **Sürdür** ile devam edin. Kaynak bir bekleme süresi bildirdiyse arama o zaman gelince kendiliğinden kaldığı yerden sürer (süre ekranda geri sayımla görünür). Bildirilen süre 2 saatten uzunsa (ör. günlük bütçe doldu) otomatik deneme yapılmaz; **Sürdür** düğmesi o saate kadar kapalıdır ve yanında hangi saatte etkin olacağı yazar. Semantic Scholar bekleme süresi bildirmez; Kasghar orada artan aralıklarla (5, 15, 30, 60 sn) birkaç kez yeniden dener, sonra durur. Anahtarsız istekler yoğunlukta sık reddedilir; ücretsiz bir API anahtarı girin. Semantic Scholar etkin bir anahtarı da zaman zaman reddedebiliyor (429): reddedilen istek beklemeden anahtarsız (ortak havuz) tekrarlanır, sonraki istek yine anahtarla denenir; anahtar art arda 3 kez reddedilirse 10 dakika anahtarsız sürülür ve kaynağın kota bilgisinde "Anahtar durumu" satırı görünür.

**Bir kaynak "şu anda yanıt vermiyor" diyor.**
Kaynağın kendi sunucusunda geçici bir arıza var (ör. PubMed'in arama sunucusu); Kasghar'dan ya da sorgunuzdan kaynaklanmaz. İndirme sırasında Kasghar birkaç kez yeniden dener. Arıza sürerse arama yarım kalır ve o ana kadar alınan kayıtlar korunur: literatür taramasında otomatik olarak (1, 5 ve 15 dakika sonra) ya da kaynağın yanındaki sürdürme düğmesiyle kaldığı yerden sürer.

**Hata mesajlarının teknik ayrıntısı nerede?**
Ekranda tüm kaynaklar için aynı, sade mesajlar gösterilir (kaynak yanıt vermiyor, bağlanılamadı, çok fazla istek, erişim reddedildi, sorgu kabul edilmedi). Kaynağın döndürdüğü ham yanıt **Admin > Günlük > İndirme hataları**'nda kaydın ayrıntısında (`technical_detail`) saklanır.

**Scopus "erişim reddedildi" diyor.**
Scopus için kurum aboneliği gerekir. Kurum ağından bağlanın ya da kütüphanenizden Insttoken isteyip Admin > Ayarlar > Kaynaklar > Scopus'a girin.

**Scopus 5.000 kayıtta durdu.**
Scopus kurumunuzu bağlandığınız ağın adresinden (ya da Insttoken'dan) tanır; tanımazsa derin sayfalamaya izin vermez. Kurum erişiminiz olup olmadığını **Admin > Ayarlar > Kaynaklar > Scopus > Bağlantıyı test et** gösterir. Kurum ağı dışında ilk 5.000 kayıt alınır ve kaynak ⚠ ile **yarım kaldı** olarak işaretlenir (otomatik yeniden denenmez, çünkü beklemek sonucu değiştirmez). Üniversite ağına (veya kurumun VPN'ine) bağlanın ya da Insttoken girin, sonra kaynağın yanındaki **Sürdür**'e basın: Kasghar alınmış kayıtları yeniden yazmadan kaldığı yerden devam eder (Scopus imleci ileri sarılamadığı için önce ilk 5.000 kaydın sayfaları hızlıca geçilir; haftalık kotadan yaklaşık 200 istek harcar). Sürdürürken Kasghar geçilen kayıtlardan rastgele seçtiklerini daha önce alınanlarla karşılaştırır; Scopus'ta sonuçların sırası bu arada değişmişse arama baştan alınır (kayıtlar ikinci kez eklenmez, güncellenir). Hâlâ kurum ağı dışındaysanız aynı açıklamayla durur.

**OpenAlex bütçesi doldu.**
Anahtarsız kullanımda günlük bütçe 0,10 $'dır. Ücretsiz bir API anahtarıyla 1 $'a çıkar; bütçe her gün yenilenir.

**Bir kaydı yanlışlıkla sildim.**
Admin > Bakım > Çöp kutusu'ndan 30 gün içinde geri yükleyebilirsiniz.

**Güncellemeden sonra ekran eski görünüyor.**
Sayfayı yenileyin (Ctrl+R / ⌘R). Kasghar sayfaları önbellekten değil sunucudan güncel hâliyle yükler; sorun sürerse tarayıcının önbelleğini temizleyin.

**Verilerim nerede?**
Bilgisayarınızda, `kasghar-data` adlı Docker volume'ünde (veritabanı, tam metinler ve ayarlar). Düzenli yedek almanız önerilir.

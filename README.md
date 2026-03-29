# FORCER-OpenClaw-TR V1.0

---

## Sürüm Durumu

**Yayınlanan sürüm:** `V1.0`  
**Durum:** Operasyonel / kişisel kullanımda aktif
**Güncel sürüm:** 'V1.2'

---

## Kişisel Akademik Operasyon Sistemi ve AI Ders Koçu

FORCER, bir mühendislik öğrencisinin akademik yaşamını deterministik veri modelleri ve LLM destekli karar mekanizmalarıyla optimize etmek için geliştirilmiş, VPS üzerinde çalışan otonom bir ajan sistemidir. Bu sürümde ana model olarak **Gemini 2.5 Flash** kullanılmaktadır.

Sistem, özellikle ders çalışmaya başlamakta zorlanan, sürdürülebilirlik ve istikrar problemi yaşayan öğrenciler için davranış odaklı bir çalışma altyapısı sunar. FORCER, yalnızca görev takibi yapan klasik bir üretkenlik aracı değil; kullanıcının akademik ritmini izleyen, riskleri önden tespit eden ve gerektiğinde müdahale eden operasyonel bir çalışma sistemi olarak tasarlanmıştır.

Bu versiyon, kişisel kullanım senaryosu için geliştirildiği için mevcut veri yapıları ve bazı karar mekanizmaları kullanıcıya özel optimize edilmiştir.

---

## Projenin Evrimi: V0.1'den V1.0'a

FORCER, ilk aşamada basit bir çalışma zamanlayıcısı olarak başladı; zaman içinde çok katmanlı, veri odaklı ve durum temelli bir akademik yönetim sistemine dönüştü.

### V0.1
- Temel oturum akışı
- Streak (istikrar) takibi

### V0.5
- Notion entegrasyonu
- Otomatik raporlama katmanının eklenmesi

### V1.0
- Pattern Mining (örüntü tespiti)
- Risk Motoru
- Kriz Yönetimi protokolleri

---

## Temel Amaç

FORCER'ın temel amacı, akademik performansı yalnızca plan üretimi üzerinden değil; davranış, tekrar eden örüntüler, risk seviyesi ve günlük çalışma ritmi üzerinden yönetmektir.

Bu yaklaşım sayesinde sistem:

- Kullanıcının çalışma davranışlarını izler
- Günlük ve haftalık örüntüleri analiz eder
- Yaklaşan sınavlar ve mevcut tempo arasında ilişki kurar
- Düşüş, kopma veya kriz anlarını önceden tespit etmeye çalışır
- Gerektiğinde otonom şekilde kullanıcıyı yeniden sürece çeker

---

## Teknik Mimari

FORCER'ın çekirdeği, **OpenClaw Agent Framework** üzerine kuruludur. Sistem; LLM tabanlı karar alma, deterministik hafıza yönetimi, veri analitiği ve zamanlanmış otomasyon katmanlarının birlikte çalışmasıyla yapılandırılmıştır.

### Mimari Bileşenler

**Ajan Çatısı:**  
OpenClaw Agent Framework

**Karar Motoru / Beyin:**  
Gemini 2.5 Flash API

**Hafıza Yönetimi:**  
`MEMORY.md` üzerinden yürütülen, `sed` tabanlı deterministik hafıza manipülasyonu

**Veri Analitiği Hattı:**  
Notion API -> Google Sheets -> Looker Studio Dashboard

**Otomasyon Katmanı:**  
Linux Cron Jobs ve Bash scriptleri

---

## Öne Çıkan Özellikler

### Deterministik Hafıza
Sistem, geçici sohbet bağlamına bağımlı kalmaz. Bunun yerine fiziksel dosyaları esas alan bir **Single Source of Truth** yaklaşımı benimser. Bu sayede hafıza yönetimi daha izlenebilir, denetlenebilir ve tutarlı hale gelir.

### Adaptif Planlayıcı
FORCER, önceki günün verimlilik etiketlerini ve çalışma akışını dikkate alarak dinamik sabah brifingleri oluşturur. Böylece her gün aynı yapıyı tekrar eden statik planlar yerine, bağlama duyarlı yönlendirmeler sunar.

### Risk Motoru
Sistem; vize tarihleri, çalışma sıklığı ve son dönem performans trendlerini birlikte değerlendirerek proaktif uyarılar üretir. Bu katman, özellikle gecikmiş hazırlık ve yaklaşan akademik baskı dönemlerinde erken müdahale amacı taşır.

### Ghost Mode
Kullanıcının pasifleştiği veya sistemden koptuğu anlarda devreye giren otonom dürtme mekanizmasıdır. Bu mod, klasik hatırlatıcılardan farklı olarak bağlama göre yeniden etkileşim kurmayı hedefler.

### Pattern Mining
Tekrarlayan davranış örüntülerini tespit ederek sistemin yalnızca anlık durumlara değil, zaman içinde oluşan alışkanlıklara da tepki vermesini sağlar.

### Kriz Yönetimi Protokolleri
Kullanıcının akademik ritminde belirgin bir düşüş, yoğun sınav baskısı veya çalışma döngüsünde kopma tespit edildiğinde devreye giren müdahale katmanıdır.

---

## Dokümantasyon

Sistemin gelişim süreci, teknik mimarisi ve operasyonel kullanımıyla ilgili detaylar için aşağıdaki dokümanlara göz atabilirsiniz:

- [**Teknik Detaylar**](https://github.com/bberksacti/FORCER-OpenClaw-TR/blob/main/docs/TEKNIK_DETAYLAR.md)  
  Sistem mimarisi, komut yapıları ve çekirdek çalışma mantığı

- [**Gelişim Yolculuğu**](https://github.com/bberksacti/FORCER-OpenClaw-TR/blob/main/docs/GELISIM_RAPORU.md)  
  V0.1'den V1.0'a kadar tüm iterasyonlar ve tasarım kararları

- [**FORCER V1.0 Kullanım Kılavuzu**](https://github.com/bberksacti/FORCER-OpenClaw-TR/blob/main/docs/FORCER_V1_KULLANIM_KILAVUZU.pdf)  
  Sistemin diğer LLM'lere (Claude, GPT vb.) tanıtılması ve operasyonel kullanım detayları için başvuru kaynağı

---

## Notlar

Bu proje, kişisel akademik operasyon ihtiyacına göre şekillendirilmiş bir sistemdir. Dolayısıyla mevcut sürümdeki bazı veri modelleri, eşikler, etiketleme mantıkları ve davranışsal karar akışları kullanıcıya özel optimize edilmiştir.

Buna rağmen FORCER, kişisel kullanımın ötesinde şu alanlar için de güçlü bir temel sunar:

- akademik koçluk sistemleri
- davranış odaklı çalışma asistanları
- kişisel üretkenlik ajanları
- LLM destekli operasyonel takip altyapıları

---

# Updates

### V1.1
- `bitir` komutu davranışı güncellendi  
  `IDENTITY.md` Bölüm 1'e şu kural eklendi:  
  **`bitir = bitti ile aynı`** — yalnızca seansı kapatır, mikro sorgu başlatmaz ve günü kapatmaz.
- Mikro sorgu sayısı **4'ten 3'e** indirildi
- Çakışan sorular birleştirildi
- Kullanıcı tarafından verilen skor artık otomatik olarak **iyi / orta / kötü** etiketlerine dönüştürülüyor
- Günlük verim skoru artık **Notion** tarafına da gönderiliyor
- Notion'da yeni bir **Verim** kolonu eklendi; bu veri artık **Looker Studio** üzerinde trend grafikleri üretmek için kullanılabiliyor
- Streak takibi otomatik hale getirildi; manuel müdahale ihtiyacı kaldırıldı

### V1.2
- Çalışma Süresi Hesaplama Mantığı Güncellendi: Durum Makinesi (State Machine) tabanlı akümülasyon hatası giderildi; artık her mola ve bitiş komutunda aktif çalışma süresi kümülatif olarak hatasız bir şekilde hesaplanıyor.
- Mola Süresi Takibi (Time Tracking): Sisteme toplam mola süresi takibi eklendi; bugun_toplam_mola_sure alanı üzerinden günlük toplam kayıp zaman metrikleri tutulmaya başlandı.
- Odak Puanı Formülü V2: Odak puanı hesaplamasına "mola cezası" parametresi eklendi; mola sürelerinin aktif çalışma süresine oranına göre dinamik ve daha gerçekçi bir verim skoru üretilmesi sağlandı.
- Gün Notu Eşik Değerleri (Thresholds) Optimize Edildi: Kullanıcı geri bildirimleri doğrultusunda gün sonu skor etiketleri (1-4: Kötü, 5-7: Orta, 8-10: İyi) olarak yeniden yapılandırıldı.
- Operasyonel Esneklik Komutları (hgb & ara): Kısa ihtiyaçlar için hgb (hemen geliyorum) ve dışsal uzun kesintiler için ara komutları eklendi; bu komutlar mola sayısını artırmadan süreyi dondurma imkanı tanıyor.
- Raporlama ve Sıfırlama Döngüsü Güncellendi: Haftalık raporlama ve sistem sıfırlama saati, kullanıcı yaşam döngüsüne uyum sağlaması amacıyla Pazartesi 03:00’a çekildi.
- Veri Yapısı Güncellemesi: MEMORY.md dosya yapısı, yeni mola metriklerini ve operasyonel verileri destekleyecek şekilde genişletildi.

## V2 Güncellemesi — Forcer’ın Fiziksele Taşınması

Forcer V2 ile sistem yalnızca Telegram üzerinden çalışan bir ders koçu olmaktan çıkarılıp fiziksel bir kontrol katmanına taşındı. Bu sürümde kullanıcı artık bilgisayara veya telefona dokunmadan, masadaki fiziksel butonlarla seansını yönetebiliyor; LED’lerden anlık durumu görebiliyor ve buzzer üzerinden sesli geri bildirim alabiliyor. Mimari; fiziksel kontrol, ESP32, VPS sunucusu, Telethon tabanlı kullanıcı hesabı entegrasyonu ve Forcer botu arasında katmanlı bir akışla çalışıyor. Butona basıldığında sinyal ESP32 tarafından okunuyor, HTTP isteği Flask sunucusuna gidiyor, Telethon kullanıcı hesabından Telegram’a mesaj gönderiyor ve Forcer komutu sanki kullanıcı yazmış gibi işliyor. ESP32 de düzenli olarak VPS’ten durum çekerek LED’leri güncelliyor.

İşte donanım için teknik detaylar: 
[**Forcer devre kurulumu 1**](https://github.com/bberksacti/FORCER-OpenClaw-TR/blob/main/docs/FORCER_FIZIKSEL_DEVRE_RAPORU.pdf)
[**Forcer devre 2 Tamamlanmış versiyon**](https://github.com/bberksacti/FORCER-OpenClaw-TR/blob/main/docs/FORCER%20DEVRE%20V2.pdf)

![WhatsApp Video 2026-03-29 at 8 27 29 PM (1) (online-video-cutter com) (1) (6)](https://github.com/user-attachments/assets/e0f2a57f-1608-4829-9f2e-b3e310c9c6cc)

- Donanım Tarafı
Donanım tarafında sistem ESP32 DevKit C V4 üzerine kuruldu. Fiziksel arayüz; 3 adet panel tipi anlık buton, 3 adet durum LED’i, aktif buzzer, 220Ω LED dirençleri ve 10kΩ pull-down dirençlerinden oluşuyor. Butonlar mola, devam ve bitir komutlarını fiziksel olarak tetikliyor. LED’ler ise Forcer’ın durumunu görsel olarak yansıtıyor: kırmızı idle/day_closed, sarı on_break, yeşil in_session. Ayrıca Wi-Fi kopması durumunda üç LED birden yanarak hata geri bildirimi veriyor.
Pin tarafında tasarım, gerçek donanım sorunları görülerek rafine edildi. Boot-sensitive pinler ve fiziksel arızalar nedeniyle bazı GPIO’lar değiştirildi; final eşleşme mola=GPIO12, devam=GPIO19, bitir=GPIO34, red=GPIO13, yellow=GPIO25, green=GPIO26, buzzer=GPIO32 olarak oturdu. Özellikle GPIO14’ün reboot sorunu, GPIO27’nin fiziksel hasarı ve GPIO33’ün pull-down desteği vermemesi gerçek donanım üzerinde tespit edilip çözüldü. Bu sayede V2 sadece teorik değil, sahada denenmiş bir fiziksel sürüm haline geldi.
Buton devresi pull-down mantığıyla çalışıyor: buton basılı değilken pin 0, basılıyken 1 okuyor. LED’ler seri dirençle sürülüyor. Fiziksel devrenin önemli tarafı, sistemin tamamen masabaşı davranışı için optimize edilmiş olması: butona bastığında LED ve buzzer anında reaksiyon veriyor; ağ gecikmesi olsa bile kullanıcı fiziksel geri bildirim alıyor. Bu da sistemi “yazılım komut arayüzü” olmaktan çıkarıp gerçek bir çalışma aracı haline getiriyor.
- ESP32 + Sunucu + Telegram Akışı
ESP32 üzerinde MicroPython çalışıyor. Kod; Wi-Fi bağlantısını yönetiyor, butonları 50ms döngüyle okuyor, HTTP isteklerini ayrı thread’de göndererek ana döngüyü bloke etmiyor ve her 5 saniyede bir VPS’ten session durumunu sorgulayıp LED’leri güncelliyor. Basit ama kritik tasarım kararları alındı: LED kilit mekanizması eklendi, 1 saniyelik çift tetikleme koruması kondu ve hata durumlarında üç kısa beep gibi ayırt edilebilir sesli geri bildirimler tanımlandı.
VPS tarafında Flask tabanlı iki ana endpoint kullanılıyor: /status, MEMORY.md içinden session durumunu okuyup JSON döndürüyor; /command?cmd=X, gelen komutu Telethon daemon’a iletiyor. Buradaki en kritik karar OpenClaw CLI yerine Telethon userbot kullanılması oldu. Çünkü bot üzerinden gönderilen mesajlar Forcer tarafından kendi kendine yazılmış gibi algılanıyordu; Telethon ile kullanıcının gerçek hesabından gönderilen mesajlar ise Forcer tarafından doğru şekilde “kullanıcı komutu” olarak işlendi. Bu değişiklik aynı zamanda gecikmeyi de ciddi biçimde düşürdü.
- V1.2 Sonrası Yazılım Geliştirmeleri
Yazılım tarafında V1.2’den V2’ye geçerken en önemli iyileştirmelerden biri gorev_sirasi mantığı oldu. Sabah verilen görev listesi artık sadece özet olarak saklanmıyor; sıralı bir görev kuyruğu olarak tutuluyor. Böylece başladım komutu otomatik olarak listedeki ilk görevi seçebiliyor, bitti komutu da o görevi listeden düşürüp bir sonraki göreve geçiş hazırlığı yapabiliyor. Bu, Forcer’ın sadece not alan bir sistem değil, görev akışı yöneten bir ajan olmasını güçlendirdi.
Bir diğer önemli düzeltme ara komutundaydı. Önceki sürümde ara verildiğinde aktif oturum başlangıç saati doğru güncellenmediği için ara süresi aktif süreye eklenebiliyordu. Bu hata düzeltilerek hem ara hem de hgb akışında zaman sayımı daha doğru hale getirildi. Aynı dönemde mola, hgb, ara, devam ve bitti bloklarına eksik olan session_durumu güncellemeleri de açık şekilde eklendi. Böylece fiziksel butonlar ve Telegram mesajları aynı durum makinesine daha tutarlı şekilde bağlandı.
Model tarafında da bilinçli bir optimizasyon yapıldı: Gemini 2.5 Flash yerine Gemini 2.5 Flash Lite’a geçildi. Gerekçe performans değil, maliyet ve rate limit yönetimiydi. Fiziksel devre testi sırasında hızlı ardışık istekler nedeniyle RPM sınırlarına çarpıldığı için, kural tabanlı ve yapılandırılmış bu sistemde daha ekonomik model seçimi tercih edildi. Bu değişiklik, Forcer’ın sadece akıllı değil aynı zamanda sürdürülebilir ve maliyet kontrollü bir yapıda kalmasına yardımcı oldu.
- V2 Üzerine Ek İyileştirmeler
V2 sonrası teknik iyileştirmelerde halka LED ve ses sistemi tarafında da ciddi ilerleme sağlandı. WS2812B halka LED’in session senkronizasyonu çözüldü; in_session, on_break, idle, day_closed, hgb ve ara için farklı ışık davranışları tanımlandı. Özellikle ara durumu için 4 sarı + 4 kırmızı segmentli görünüm, hgb için sarı blink ve in_session için ayrı yeşil parlaklık profili eklendi. Böylece sistemin fiziksel görsel dili daha zengin ve daha anlamlı hale geldi.
Ses sistemi tarafında DFPlayer Mini için thread çakışması, otomatik sıradaki sesi çalma, SD kart indeksleme ve alarm tetikleme sorunları analiz edilip büyük ölçüde çözüldü. play_token mantığı ile eski thread’lerin yanlış sesi çalması engellendi, ses dosyalarının SD kart sırasına göre davranması dikkate alınarak kurulum prosedürü netleştirildi ve alarm sistemi geçici olarak devre dışı bırakıldı. Bu sürüm, sadece “çalışıyor” değil, donanım davranışları tek tek test edilerek rafine edilmiş bir versiyon oldu.

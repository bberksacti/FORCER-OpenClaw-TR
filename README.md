# FORCER-OpenClaw-TR V1.0

---

## Sürüm Durumu

**Yayınlanan sürüm:** `V1.0`  
**Durum:** Operasyonel / kişisel kullanımda aktif
**Güncel sürüm:** 'V2.00'

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

Looker Studio arayüzü garfikleri (Test verilerinde elde edilmiştir, değerler gerçek değerler değildir):
<img width="507" height="1074" alt="image" src="https://github.com/user-attachments/assets/de9fcee7-d262-442d-a80c-7ab7afc7c871" />

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

## V2 Güncellemesi - Forcer Fiziksel Kontrol Katmanı

Forcer V2 ile sistem sadece Telegram üzerinden çalışan bir bot olmaktan çıkıp, tamamen fiziksel bir kontrol arayüzüne taşındı. Bu sürümün temel amacı, kullanıcının telefon veya bilgisayar bağımlılığını azaltarak masadaki fiziksel butonlarla tüm süreci yönetmesini sağlamaktır.

### Sistem Çalışma Mantığı ve Akış
Sistem; fiziksel donanım, ESP32, VPS sunucusu ve Telethon tabanlı bir daemon arasında çok katmanlı bir yapıyla çalışıyor:

- Kullanıcı fiziksel butona bastığında sinyal ESP32 tarafından yakalanır.
- ESP32, VPS üzerindeki Flask sunucusuna ilgili komut için bir HTTP isteği gönderir.
- Flask sunucusu, Telethon kütüphanesi ile kullanıcının kendi Telegram hesabı üzerinden Forcer botuna mesaj gönderir. 
- Bu sayede bot, mesajı gerçekten kullanıcı yazmış gibi algılar ve süreci başlatır.
- ESP32 her 5 saniyede bir VPS'ten güncel durumu sorgulayarak LED renklerini günceller.

### Donanım Detayları ve Pin Şeması
Donanım tarafı ESP32 DevKit C V4 üzerine kurulu olup; 3 adet panel tipi buton, durum LED'leri ve sesli geri bildirim için buzzer/DFPlayer içermektedir. Donanım kurulumu sırasında boot-sensitive pinler ve donanım hataları nedeniyle rafine edilen final pin şeması şu şekildedir:

| Fonksiyon | GPIO Pin | Açıklama |
| :--- | :--- | :--- |
| Mola Butonu | GPIO 12 | Pull-down |
| Devam Butonu | GPIO 19 | Pull-down |
| Bitir Butonu | GPIO 34 | Pull-down |
| Kırmızı LED (Idle) | GPIO 13 | 220 Ohm dirençli |
| Sarı LED (Mola) | GPIO 25 | 220 Ohm dirençli |
| Yeşil LED (Seans) | GPIO 26 | 220 Ohm dirençli |
| Buzzer | GPIO 32 | Aktif sesli uyarı |

*Not: GPIO 14 (reboot sorunu), GPIO 27 (fiziksel arıza) ve GPIO 33 (pull-down sorunu) nedeniyle bu pinlerden vazgeçilmiştir.*

![WhatsApp Video 2026-03-29 at 8 27 29 PM (1) (online-video-cutter com) (1) (6)](https://github.com/user-attachments/assets/e0f2a57f-1608-4829-9f2e-b3e310c9c6cc)

### Yazılım ve Model Güncellemeleri
V1.2 sonrasında yapılan kritik iyileştirmeler:

- gorev_sirasi Mantığı: Görev listesi artık basit bir metin değil, sıralı bir kuyruk (queue) olarak tutuluyor. "başladım" komutu ilk görevi seçerken, "bitti" komutu otomatik olarak sıradaki göreve geçiş sağlıyor.
- Model Değişikliği: Gemini 2.5 Flash yerine Gemini 2.5 Flash Lite modeline geçildi. Bu tercih hızdan ziyade, fiziksel cihaz üzerinden yapılan hızlı ve ardışık isteklerde RPM (dakikadaki istek) sınırlarına takılmamak ve maliyeti optimize etmek amacıyla yapıldı.
- MicroPython Optimizasyonu: HTTP istekleri ayrı bir thread üzerinden gönderilerek ana döngünün buton okuma sırasında donması engellendi.

### Geri Bildirim ve Senkronizasyon
- LED Halka (WS2812B): in_session, on_break ve day_closed durumları için farklı ışık profilleri tanımlandı. Mola durumunda 4 sarı + 4 kırmızı segmentli özel bir gösterim eklendi.
- Ses Sistemi: DFPlayer Mini üzerindeki thread çakışmaları "play_token" yapısıyla çözüldü. SD kart üzerindeki ses dosyaları indekslenerek alarm ve onay sesleri stabilize edildi.

[Forcer Devre Raporu 1](https://github.com/bberksacti/FORCER-OpenClaw-TR/blob/main/docs/FORCER_FIZIKSEL_DEVRE_RAPORU.pdf)
[Forcer V2 Final Devre Şeması](https://github.com/bberksacti/FORCER-OpenClaw-TR/blob/main/docs/FORCER%20DEVRE%20V2.pdf)

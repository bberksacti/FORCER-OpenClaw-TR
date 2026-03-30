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

## V2 Guncellemesi - Forcer Fiziksel Kontrol Katmani

Forcer V2 ile sistem sadece Telegram uzerinden calisan bir bot olmaktan cikip, tamamen fiziksel bir kontrol arayuzune tasindi. Bu surumun temel amaci, kullanicinin telefon veya bilgisayar bagimliligini azaltarak masadaki fiziksel butonlarla tum sureci yonetmesini saglamaktir.

### Sistem Calisma Mantigi ve Akis
Sistem; fiziksel donanim, ESP32, VPS sunucusu ve Telethon tabanli bir daemon arasinda cok katmanli bir yapiyla calisiyor:

- Kullanici fiziksel butona bastiginda sinyal ESP32 tarafindan yakalanir.
- ESP32, VPS uzerindeki Flask sunucusuna ilgili komut icin bir HTTP istegi gonderir.
- Flask sunucusu, Telethon kutuphanesi ile kullanicinin kendi Telegram hesabi uzerinden Forcer botuna mesaj gonderir. 
- Bu sayede bot, mesaji gercekten kullanici yazmis gibi algilar ve sureci baslatir.
- ESP32 her 5 saniyede bir VPS'ten guncel durumu sorgulayarak LED renklerini gunceller.

### Donanim Detaylari ve Pin Semasi
Donanim tarafı ESP32 DevKit C V4 uzerine kurulu olup; 3 adet panel tipi buton, durum LED'leri ve sesli geri bildirim icin buzzer/DFPlayer icermektedir. Donanim kurulumu sirasinda boot-sensitive pinler ve donanim hatalari nedeniyle rafine edilen final pin semasi su sekildedir:

| Fonksiyon | GPIO Pin | Aciklama |
| :--- | :--- | :--- |
| Mola Butonu | GPIO 12 | Pull-down |
| Devam Butonu | GPIO 19 | Pull-down |
| Bitir Butonu | GPIO 34 | Pull-down |
| Kirmizi LED (Idle) | GPIO 13 | 220 Ohm direncli |
| Sari LED (Mola) | GPIO 25 | 220 Ohm direncli |
| Yesil LED (Seans) | GPIO 26 | 220 Ohm direncli |
| Buzzer | GPIO 32 | Aktif sesli uyari |

*Not: GPIO 14 (reboot sorunu), GPIO 27 (fiziksel ariza) ve GPIO 33 (pull-down sorunu) nedeniyle bu pinlerden vazgecilmistir.*

![WhatsApp Video 2026-03-29 at 8 27 29 PM (1) (online-video-cutter com) (1) (6)](https://github.com/user-attachments/assets/e0f2a57f-1608-4829-9f2e-b3e310c9c6cc)

### Yazilim ve Model Guncellemeleri
V1.2 sonrasinda yapilan kritik iyilestirmeler:

- gorev_sirasi Mantigi: Gorev listesi artik basit bir metin degil, sirali bir kuyruk (queue) olarak tutuluyor. "basladim" komutu ilk gorevi secerken, "bitti" komutu otomatik olarak siradaki goreve gecis sagliyor.
- Model Degisikligi: Gemini 2.5 Flash yerine Gemini 2.5 Flash Lite modeline gecildi. Bu tercih hizdan ziyade, fiziksel cihaz uzerinden yapilan hizli ve ardisik isteklerde RPM (dakikadaki istek) sinirlarina takilmamak ve maliyeti optimize etmek amaciyla yapildi.
- MicroPython Optimizasyonu: HTTP istekleri ayri bir thread uzerinden gonderilerek ana dongunun buton okuma sirasinda donmasi engellendi.

### Geri Bildirim ve Senkronizasyon
- LED Halka (WS2812B): in_session, on_break ve day_closed durumlari icin farkli isik profilleri tanimlandi. Mola durumunda 4 sari + 4 kirmizi segmentli ozel bir gosterim eklendi.
- Ses Sistemi: DFPlayer Mini uzerindeki thread cakismalari "play_token" yapisiyla cozuldu. SD kart uzerindeki ses dosyalari indekslenerek alarm ve onay sesleri stabilize edildi.

[Forcer Devre Raporu 1](https://github.com/bberksacti/FORCER-OpenClaw-TR/blob/main/docs/FORCER_FIZIKSEL_DEVRE_RAPORU.pdf)
[Forcer V2 Final Devre Semasi](https://github.com/bberksacti/FORCER-OpenClaw-TR/blob/main/docs/FORCER%20DEVRE%20V2.pdf)

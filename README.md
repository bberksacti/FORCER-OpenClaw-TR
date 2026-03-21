# FORCER-OpenClaw-TR V1.0

---

## Sürüm Durumu

**Yayınlanan sürüm:** `V1.0`  
**Durum:** Operasyonel / kişisel kullanımda aktif
**Güncel sürüm:** 'V1.1'

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

## Updates

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

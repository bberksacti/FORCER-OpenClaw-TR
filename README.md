FORCER-OpenClaw-TR V1.0 
Kişisel Akademik Operasyon Sistemi ve AI Ders Koçu

FORCER, bir mühendislik öğrencisinin akademik hayatını deterministik veri modelleri ve LLM (Gemini 2.5 Flash) gücüyle optimize eden, VPS üzerinde çalışan otonom bir ajan sistemidir.

FORCER, ders çalışmaya başlamakta zorlanan ve istikrar problemi yaşayan öğrenciler için tasarlanmış davranış odaklı bir AI çalışma sistemidir.
(bu versiyon kendime özel tasarlandığı için şuan veriler kişisel optimize edildi)
*** Evrim: V0.1'den V1.0'a
Proje, basit bir zamanlayıcıdan karmaşık bir durum makinesine (State Machine) dönüşmüştür:

V0.1: Temel oturum akışı ve Streak (istikrar) takibi.

V0.5: Notion entegrasyonu ve otomatik raporlama katmanının eklenmesi.

V1.0: Pattern Mining (Örüntü tespiti), Risk Motoru ve Kriz Yönetimi protokolleri.

*** Teknik Mimari
Çekirdek: OpenClaw Agent Framework

Beyin: Gemini 2.5 Flash API

Hafıza Yönetimi: sed tabanlı deterministik hafıza manipülasyonu (MEMORY.md)

Veri Analitiği: Notion API -> Google Sheets -> Looker Studio Dashboard

Otomasyon: Linux Cron Jobs & Bash Scripts

*** Öne Çıkan Özellikler
Deterministik Hafıza: Chat geçmişine değil, fiziksel dosyalara dayalı "Tek Gerçeklik Kaynağı" prensibi.

Adaptif Planlayıcı: Önceki günün verimlilik etiketlerine göre dinamik sabah brifingi hazırlama.

Risk Motoru: Vize tarihleri ve çalışma trendlerini kıyaslayarak proaktif uyarılar üretme.

Ghost Mode: Kullanıcı pasifleştiğinde devreye giren otonom dürtme (nudging) sistemi.

*** Dokümantasyon
Sistemin tüm gelişim süreci, kullanım detayları ve teknik mimarisi için aşağıdaki kaynaklara göz atabilirsiniz:
- [Teknik Detaylar](docs/TEKNIK_DETAYLAR.md): Sistem mimarisi ve komut yapilari.
- [Gelisim Yolculugu](docs/GELISIM_RAPORU.md): V0.1'den V1.0'a tum iterasyonlar. 
- [FORCER V1.0 Kullanim Kilavuzu](docs/FORCER_V1_KULLANIM_KILAVUZU.pdf): Sistemin diger LLM'lere (Claude, GPT) tanitilmasi ve operasyonel detaylar i‡in basvuru kaynagi. 

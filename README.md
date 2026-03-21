# FORCER-OpenClaw V1.0 
> **Kişisel Akademik Operasyon Sistemi ve AI Ders Koçu**

FORCER, bir öğrencinin akademik hayatını deterministik veri modelleri ve LLM (Gemini 2.5 Flash) gücüyle optimize eden, VPS üzerinde çalışan otonom bir ajan sistemidir.

## Evrim: V0.1'den V1.0'a
Proje, basit bir zamanlayıcıdan karmaşık bir durum makinesine (State Machine) dönüşmüştür:
- **V0.1:** Temel oturum akışı ve Streak takibi.
- **V0.5:** Notion entegrasyonu ve otomatik raporlama katmanı.
- **V1.0:** Pattern Mining (Örüntü tespiti), Risk Motoru ve Kriz Yönetimi protokolleri.

## Teknik Mimari
- **Çekirdek:** OpenClaw Agent Framework
- **Beyin:** Gemini 2.5 Flash API
- **Hafıza Yönetimi:** `sed` tabanlı deterministik hafıza manipülasyonu (MEMORY.md)
- **Veri Analitiği:** Notion API -> Google Sheets -> Looker Studio Dashboard
- **Otomasyon:** Linux Cron Jobs & Bash Scripts

## Öne Çıkan Özellikler
- **Deterministik Hafıza:** Chat geçmişine değil, fiziksel dosyalara dayalı "Tek Gerçeklik Kaynağı" prensibi.
- **Adaptif Planlayıcı:** Önceki günün verimlilik etiketlerine göre sabah brifingi hazırlama.
- **Risk Motoru:** Vize tarihleri ve çalışma trendlerini kıyaslayarak proaktif uyarılar üretme.
- **Ghost Mode:** Kullanıcı pasifleştiğinde devreye giren otonom dürtme sistemi.

## Dokümantasyon
Sistemin tüm gelişim süreci ve teknik detayları için `docs/` klasörüne göz atabilirsiniz:
- [Teknik Detaylar](docs/TEKNIK_DETAYLAR.md): Sistem mimarisi ve komut yapıları.
- [Gelişim Yolculuğu](docs/GELISIM_RAPORU.md): V0.1'den V1.0'a tüm iterasyonlar.
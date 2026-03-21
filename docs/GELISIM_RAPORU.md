# Forcer Sistemi — V0.1'den V1.0'a Gelişim Raporu
**Hazırlayan:** Claude (Anthropic)  
**Tarih:** 2026-03-20  
**Amaç:** Yeni Claude oturumlarında sistemi sıfırdan tanıtmak, V1.0 sonrası güncellemeleri bilinçli yapmak.

---

## SİSTEM NEDİR?

Forcer, Berk'in kişisel ders koçu ve akademik operasyon sistemidir. OpenClaw altyapısı üzerinde çalışır, Gemini 2.5 Flash modelini kullanır, VPS sunucusuna bağlı Telegram botu üzerinden iletişim kurar.

**Temel bileşenler:**
- `IDENTITY.md` — Forcer'ın tüm davranış kuralları, protokoller, sistem mantığı
- `MEMORY.md` — Kalıcı ve geçici durum bilgisi (streak, dersler, günlük veriler)
- Notion — Günlük/haftalık/aylık raporların yazıldığı veritabanı
- Google Sheets + Looker Studio — Notion verilerinden beslenen görsel dashboard

**Dosya konumları:**
- `/home/openclaw/.openclaw/workspace-koc/IDENTITY.md`
- `/home/openclaw/.openclaw/workspace-koc/MEMORY.md`

---

## V0.1 — BAŞLANGIÇ HALİ

İlk sistemde şunlar vardı:
- Temel seans akışı: `başladım / mola / devam / bitti / günü bitirdim`
- Streak takibi
- Ghost mode
- Risk motoru
- Günlük/haftalık/aylık rapor
- Kriz modu, düşüş günü protokolü
- Notion entegrasyonu (5 parametreli `notion-rapor`)
- `bugun_pasif_sure` alanı (aktif kullanımda)
- `MEMORY.md` yazma kuralı: `sed` komutları

**V0.1'deki kritik eksikler:**
- Odak metriği yoktu
- Mola sıklığı takibi yoktu
- `bugun_aktif_sure` "0 saat" formatında tutuluyordu (metin)
- `bitti` komutunda MEMORY.md güncelleme `sed` komutları yoktu — Forcer sözlü söylüyor ama yazmıyordu
- Günlük rapor OZET formatı tanımsızdı — Forcer serbest üretiyordu
- Mikro sorgu `bitti` komutunda da tetikleniyordu (sadece `günü bitirdim`'de olmalıydı)
- `bu_hafta_sure` güncellenmiyordu
- `bugun_aktif_dersler` tek derslik — çok dersli günlerde son ders eziyordu

---

## V0.2-V0.5 — ODAK METRİĞİ EKLEMESİ

### Problem
Berk'in temel sorunu: kısa çalışma + çok mola kombinasyonu. Bunu ölçmek istedi.

### Tartışılan formüller
İlk önerilen basit formülden sonra V3 formülüne karar verildi:

```
ortalama_blok = aktif_dakika / (mola_sayisi + 1)
blok_skoru = min(100, (ortalama_blok / 45) * 100)
hacim_skoru = min(100, (aktif_dakika / 150) * 100)
odak_yuzdesi = round((blok_skoru * 0.75) + (hacim_skoru * 0.25))
```

**Mantık:** Aynı aktif süreyi daha az molayla yaptıysan puan yükselir. Aktif süren düşük ve mola sayın çoksa puan düşer.

### MEMORY.md'ye eklenen alanlar
```
- bugun_mola_sayisi: 0
- bugun_odak_yuzdesi: -
```

### Kaldırılan alan
```
- bugun_pasif_sure  ← tamamen kaldırıldı
```

**Neden kaldırıldı:** Pasif görevlerde (otobüste not okuma, kelime tekrarı vb.) süre tutulmaz. Sadece tamamlandı/tamamlanmadı takibi yapılır. `bugun_pasif_aktiviteler` alanı korundu.

---

## V0.6 — KRİTİK SORUN TESPİTİ VE ÇÖZÜMÜ

### Tespit edilen sorunlar
1. **Toplam süre Notion'a "-" gidiyordu** — `bugun_aktif_sure` "0 saat" formatındaydı, Forcer parse edemiyordu
2. **Odak Notion'a yazılmıyordu** — OZET formatı iki yerde tanımlıydı (Bölüm 14 ve Bölüm 20), çelişki vardı
3. **Mikro sorgu `bitti`'de tetikleniyordu** — sadece `günü bitirdim`'de olmalıydı
4. **Session geçmişi eski davranışı taşıyordu** — dosya değişse bile eski konuşma bağlamı baskın geliyordu

### Yapılan düzeltmeler
- `bugun_aktif_sure: 0 saat` → `bugun_aktif_sure: 0` (saf sayı)
- Bölüm 20'deki tekrar eden OZET format tanımı silindi
- Bölüm 3 Mikro Sorgu başına: `"bitti" komutunda bu soruları SORMA` eklendi
- Session `.jsonl` dosyası silindi, servis restart edildi

---

## V0.7 — MEMORY.md YAZMA SORUNLARI

### Tespit
`bitti` komutunda Forcer süreyi sözlü söylüyor ama `MEMORY.md`'ye yazmıyordu. `sed` komutları IDENTITY.md'de yoktu.

### Eklenen sed komutları — `bitti` akışı
```bash
sed -i 's/- session_durumu: .*/- session_durumu: idle/' ~/.openclaw/workspace-koc/MEMORY.md
sed -i "s/- bugun_aktif_sure: .*/- bugun_aktif_sure: YENİ_TOPLAM_DAKİKA/" ~/.openclaw/workspace-koc/MEMORY.md
sed -i "s/- bugun_aktif_dersler: .*/- bugun_aktif_dersler: DERS_ADI/" ~/.openclaw/workspace-koc/MEMORY.md
sed -i "s/- DERS_KISA_AD: son_calisma=.*/- DERS_KISA_AD: son_calisma=BUGUN_TARIH/" ~/.openclaw/workspace-koc/MEMORY.md
sed -i "s/- bu_hafta_sure: .*/- bu_hafta_sure: YENİ_TOPLAM/" ~/.openclaw/workspace-koc/MEMORY.md
```

### Eklenen sed komutları — `mola` akışı
```bash
sed -i "s/- bugun_mola_sayisi: .*/- bugun_mola_sayisi: YENİ_SAYI/" ~/.openclaw/workspace-koc/MEMORY.md
```

### Eklenen sed komutları — `devam` akışı
```bash
sed -i 's/- mola_baslangic: .*/- mola_baslangic: -/' ~/.openclaw/workspace-koc/MEMORY.md
```

---

## V0.8 — RAPOR SİSTEMİ DÜZELTMELERİ

### OZET formatı sabitlendi
```
"Pasif: [bugun_pasif_aktiviteler] | Enerji: [enerji_modu] | Sebep: [sebep_etiketi] | Odak: %[bugun_odak_yuzdesi] | Not: [bugun_notu]"
```

### Notion komutu güncellendi (8 parametreli)
```bash
notion-rapor "daily" "TARIH" "AKTIF_SURE" "DERSLER" "OZET" "PASIF_AKTIVITELER" "ENERJI_MODU" "SEBEP_ETIKETI"
```

### Mikro sorgu 4 soruya çıkarıldı
```
1. Odak nasıldı, verimli geçti mi?
2. Odak 10 üzerinden kaçtı?
3. Bugün neden düşük/yüksek verimdi? (sebep_etiketi)
4. Yarın planını yaptın mı?
```

**Cevap→Alan eşleşmesi:**
- 1. soru → `bugun_notu`
- 2. soru → `son_mikro_skor`
- 3. soru → `sebep_etiketi`
- 4. soru → `yarin_plan`

### Reset sed bloğu eklendi (Bölüm 14 adım 12)
Tüm TODAY alanları, `aktif_oturum_baslangic`, `aktif_oturum_ders`, `mola_baslangic` sed ile sıfırlanıyor.

---

## V0.9 — ANALİTİK VE STABİLİTE DÜZELTMELERİ

### `bu_hafta_sure` birim sorunu
**Önceki hal:** `bu_hafta_sure: 0 saat` — metin formatı, matematik hatası riski  
**Yeni hal:** `bu_hafta_sure: 0` — saf tam sayı (dakika)  
**`haftalik_hedef` de güncellendi:** `2400 dk` → `2400`

### Çok dersli gün desteği
`bitti` akışında `bugun_aktif_dersler` güncelleme mantığı:
- Alan "-" ise → DERS_ADI yaz
- Alan doluysa → mevcut değerin sonuna ", DERS_ADI (SURE dk)" ekle

### `notion-ders` komutu kaldırıldı
**Neden:** Her `bitti` komutunda Notion ders takibi sayfasına yeni satır açıyordu. Aynı ders için haftada onlarca satır birikiyor, veri dağınıklığı yaratıyordu. Günlük rapordaki "çalışılan konular" kolonu zaten ders bilgisini tutuyor.

### Sınav modu kendiliğinden açılma sorunu çözüldü
Bölüm 16'ya eklendi: `"Sadece aktif_mod alanını değiştirme. Mod değişikliği sadece Berk '/mod sinav' yazınca olur."`

### Haftalık sıfırlamaya sed eklendi
Bölüm 19'a:
```bash
sed -i 's/- bu_hafta_sure: .*/- bu_hafta_sure: 0/' ~/.openclaw/workspace-koc/MEMORY.md
```

### WEEK_BUFFER yazma kuralı netleştirildi
```bash
TARIH=$(date +%Y-%m-%d)
OZET="- $TARIH: $bugun_aktif_dersler. Odak: $bugun_notu, Skor: $son_mikro_skor, Sebep: $sebep_etiketi."
sed -i "/^# WEEK_BUFFER/a $OZET" ~/.openclaw/workspace-koc/MEMORY.md
```

---

## V1.0 — FINAL HAL

### Eklenen son özellikler

**Tek Gerçeklik Kaynağı kuralı (IDENTITY.md başı):**
```
Chat geçmişine değil, sadece MEMORY.md ve IDENTITY.md içeriğine güven.
Çelişki varsa dosyayı baz al, geçmişi görmezden gel.
Her kritik durum değişikliğinde derhal MEMORY.md'ye işle.
```

**PATTERNS tetikleyici (Bölüm 14 adım 10.5):**
Aynı `sebep_etiketi` üst üste 3 gün gelirse `tetikleyici_engeller` alanına işle, 4. günün sabahında Berk'e raporla.

**Bölüm 4 ton belirleme:**
```
- Dünkü odak_yuzdesi < 40 → sakin ve destekleyici ton
- mevcut_streak > 7 → sert ve yüksek beklenti tonu
- Haftalık hedefe kalan dakikayı hesapla, net söyle
```

**`yarin_plan` alanı eklendi (MEMORY.md TODAY):**
Mikro sorgunun 4. sorusunun cevabı artık `bugun_notu`'na değil `yarin_plan`'a yazılıyor.

**`bugun_baslangic_saati` eklendi (MEMORY.md TODAY):**
Günün ilk `başladım` komutunda saat kaydediliyor. Ghost mode bu alana bakarak "zaten başlamış" tespiti yapabiliyor.

**Isınma Turu güncellendi (Bölüm 8):**
Sabit 3 adım yerine `MEMORY.md → ise_yarayan_acilislar` alanından kişisel öneri:
```
ise_yarayan_acilislar: mantik_devreleri (devre_kurcalama), yazilim (proje_yazma), vya (algoritma_cozme)
```

**Session temizliği — VPS cron:**
```bash
# crontab -l
0 3 * * 0 rm -f /home/openclaw/.openclaw/agents/koc/sessions/*.jsonl && sudo systemctl restart openclaw
```
Her Pazar 03:00'da otomatik çalışır. Forcer'a bağımlı değil.

---

## V1.0 — MEMORY.md YAPISI

```markdown
# STATE
- aktif_mod: varsayilan
- session_durumu: idle
- mevcut_streak: 0
- en_uzun_streak: 0
- son_calisma_tarihi: -
- bu_hafta_sure: 0          ← saf tam sayı (dakika)
- haftalik_hedef: 2400      ← saf tam sayı (dakika)
- aktif_oturum_ders: -
- aktif_oturum_baslangic: -
- mola_baslangic: -
- ghost_mode_bugun: 0
- session_ozeti: -

# TODAY
- bugun_baslangic_saati: -  ← YENİ: ilk başladım saati
- bugun_aktif_sure: 0       ← saf tam sayı (dakika)
- bugun_mola_sayisi: 0      ← YENİ: odak metriği için
- bugun_odak_yuzdesi: -     ← YENİ: V3 formülü
- bugun_aktif_dersler: -
- bugun_pasif_aktiviteler: -
- tamamlanan_p1: -
- kalan_p1: -
- enerji_modu: normal
- son_mikro_skor: -
- bugun_notu: -             ← 1. mikro soru (iyi/orta/kötü)
- sebep_etiketi: -          ← 3. mikro soru
- sebep_notu: -
- yarin_plan: -             ← YENİ: 4. mikro soru (evet/hayır)

# PATTERNS
- basari_oruntuleri: -
- tetikleyici_engeller: -
- ise_yarayan_acilislar: mantik_devreleri (devre_kurcalama), yazilim (proje_yazma), vya (algoritma_cozme)
- kacinilacak_durumlar: -

# COURSES
- Adli Bilisim: son_calisma=-
- VYS: son_calisma=-
- VYA: son_calisma=-
- Sayisal Analiz: son_calisma=-
- Mantik Devreleri: son_calisma=-
- Matematik 2: son_calisma=-
- Algoritma ve Programlama: son_calisma=-
- IELTS: son_calisma=-

# WEEK_BUFFER
- (Son 7 gunun kisa ozetleri buraya yazilir, haftalik rapor sonrasi temizlenir)

# LONG_TERM
- italya_hedefi: IELTS, transkript, motivasyon_mektubu, universite_arastir, basvuru_tarihleri
- ielts_hedef_skor: -
- kritik_tarihler: vize_haftasi=13-17_Nisan_2026
- berk_profili: DEHB, ya-hep-ya-hic duzeni, aktif+alttan dersler+IELTS
- italya_son_hatirlatma: -
```

---

## V1.0 — TEMEL KULLANIM

```
Sabah:     Plan listesi at (P1, P2, P3, Pasif)
Çalışma:   başladım → ders → mola → devam → bitti
Gün sonu:  günü bitirdim → 4 soruyu cevapla → rapor otomatik gider
```

---

## BİLİNEN KISITLAR VE İZLENECEK NOKTALAR

1. **Aynı ders toplama:** `bugun_aktif_dersler` append mantığıyla çalışıyor, gerçek merge yok. Aynı dersi aynı gün 3 kez çalışırsan "VYS (30 dk), VYS (25 dk), VYS (20 dk)" gibi görünür.

2. **PATTERNS henüz boş:** Tetikleyici mantığı kuruldu ama dolması için birkaç haftalık gerçek kullanım gerekiyor.

3. **`bugun_notu` çakışma riski:** 1. mikro soru cevabı `bugun_notu`'na yazılıyor. WEEK_BUFFER'da da bu alan kullanılıyor. Tutarlı etiketler kullan (iyi/orta/kötü).

4. **Session temizliği:** Her Pazar 03:00'da otomatik. Temizlik sonrası Forcer MEMORY.md'den her şeyi okur, kayıp olmaz.

5. **Maliyet:** Gerçek kullanımda günlük 2-5 TL, aylık 60-150 TL civarı bekleniyor.

---

## V1.0 SONRASI GÜNCELLEME YAPILACAKSA

**Önce sor:**
- Bu ekleme mevcut hangi protokollerle çakışır?
- MEMORY.md'ye yeni alan gerekiyor mu?
- Reset bloğuna yeni sed satırı eklemek gerekiyor mu?
- Bölüm 14 rapor zincirini etkiliyor mu?

**Temel kural:** Her değişiklik küçük, tek adım, test edilebilir olmalı. Büyük refactor yapma.

**Dosyaları değiştirdikten sonra mutlaka:**
```bash
sudo systemctl restart openclaw
```

**Session temizliği gerekirse:**
```bash
rm ~/.openclaw/agents/koc/sessions/*.jsonl
sudo systemctl restart openclaw
```

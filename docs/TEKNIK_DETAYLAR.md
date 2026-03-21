# FORCER V1.0 — TAM TEKNİK RAPOR
**Tarih:** 2026-03-21
**Hazırlayan:** Claude (Anthropic)
**Amaç:** V1.0 sisteminin tüm teknik ve pratik detaylarını eksiksiz belgelemek.

---

## 1. SİSTEM MİMARİSİ

### 1.1 Bileşenler
- OpenClaw: Ajan çalıştırma altyapısı
- Gemini 2.5 Flash: Forcer'ın kullandığı LLM modeli
- VPS (Ubuntu 24): Tüm sistemin çalıştığı sunucu
- Telegram: Berk ile iletişim kanalı
- Notion: Günlük/haftalık/aylık raporların veritabanı
- Google Sheets + Looker Studio: Notion verilerinden dashboard

### 1.2 Dosya Yapısı
```
/home/openclaw/.openclaw/workspace-koc/
├── IDENTITY.md     ← Forcer'ın tüm davranış kuralları
├── MEMORY.md       ← Kalıcı ve geçici durum bilgisi
├── IDENTITY.md.bak ← Yedek
├── MEMORY.md.bak   ← Yedek
├── AGENTS.md, SOUL.md, TOOLS.md, USER.md, HEARTBEAT.md, BOOTSTRAP.md
└── memory/2026-03-18.md

/home/openclaw/.openclaw/agents/koc/sessions/
├── sessions.json   ← Session metadata
└── *.jsonl         ← Konuşma geçmişleri (Pazar 03:00 silinir)
```

### 1.3 Servis Yönetimi
```bash
sudo systemctl restart openclaw
sudo systemctl status openclaw --no-pager

# Manuel session temizliği
rm ~/.openclaw/agents/koc/sessions/*.jsonl
sudo systemctl restart openclaw

# VPS cron (otomatik, her Pazar 03:00)
0 3 * * 0 rm -f /home/openclaw/.openclaw/agents/koc/sessions/*.jsonl && sudo systemctl restart openclaw
```
**ÖNEMLİ:** IDENTITY.md veya MEMORY.md değiştirdikten sonra MUTLAKA restart yap.

---

## 2. MEMORY.md — V1.0 TAM YAPI

### 2.1 Sıfır Şablonu
```
# STATE
- aktif_mod: varsayilan
- session_durumu: idle
- mevcut_streak: 0
- en_uzun_streak: 0
- son_calisma_tarihi: -
- bu_hafta_sure: 0
- haftalik_hedef: 2400
- aktif_oturum_ders: -
- aktif_oturum_baslangic: -
- mola_baslangic: -
- ghost_mode_bugun: 0
- session_ozeti: -

# TODAY
- bugun_baslangic_saati: -
- bugun_aktif_sure: 0
- bugun_mola_sayisi: 0
- bugun_odak_yuzdesi: -
- bugun_aktif_dersler: -
- bugun_pasif_aktiviteler: -
- tamamlanan_p1: -
- kalan_p1: -
- enerji_modu: normal
- son_mikro_skor: -
- bugun_notu: -
- sebep_etiketi: -
- sebep_notu: -
- yarin_plan: -

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

### 2.2 Alan Referansı

STATE (yarı kalıcı):
- aktif_mod: varsayilan/komutan/sakin/kriz/sinav/minimal
- session_durumu: idle/in_session/on_break/day_closed
- bu_hafta_sure: SAF TAM SAYI (dakika) — "dk" veya "saat" metni YAZMA
- haftalik_hedef: SAF TAM SAYI (2400 = 40 saat)

TODAY (her gün sıfırlanır) — Mikro soru eşleşmesi:
- bugun_baslangic_saati: İlk başladım saati (HH:MM)
- bugun_aktif_sure: SAF TAM SAYI (dakika)
- bugun_mola_sayisi: int (odak metriği için)
- bugun_odak_yuzdesi: int 0-100 (V3 formülü)
- son_mikro_skor: 2. mikro soru cevabı (sayı)
- bugun_notu: 1. mikro soru cevabı (iyi/orta/kötü)
- sebep_etiketi: 3. mikro soru cevabı
- yarin_plan: 4. mikro soru cevabı (evet/hayır)

PATTERNS (kalıcı, Forcer doldurur):
- tetikleyici_engeller: 3 gün üst üste aynı sebep_etiketi gelince yazılır
- ise_yarayan_acilislar: Isınma turu için kişisel tercihler

### 2.3 Kritik Format Kuralı
bu_hafta_sure, haftalik_hedef, bugun_aktif_sure, bugun_odak_yuzdesi:
SADECE SAF TAM SAYI. Hiçbir metin eki yok.

---

## 3. IDENTITY.md — PROTOKOLLER

### 3.1 Tek Gerçeklik Kaynağı (Dosya Başı)
Chat geçmişine değil, sadece MEMORY.md ve IDENTITY.md içeriğine güven.
Çelişki varsa dosyayı baz al, geçmişi görmezden gel.
Her kritik değişiklikte MEMORY.md'ye derhal yaz.

### 3.2 Bölüm 1: Seans Akışı — Komut → sed

"başladım":
  session_durumu=in_session, saati kaydet, dersi sor
  sed -i 's/- bugun_baslangic_saati: .*/- bugun_baslangic_saati: HH:MM/' MEMORY.md
  (Sadece günün ilk başladım'ında. Üzerine yazma.)

"mola":
  session_durumu=on_break, mola_baslangic kaydet
  sed -i "s/- bugun_mola_sayisi: .*/- bugun_mola_sayisi: YENİ_SAYI/" MEMORY.md
  YENİ_SAYI = mevcut + 1

"devam" / "döndüm":
  mola süresini hesapla, 45 dk+ ise uyar, session_durumu=in_session
  sed -i 's/- mola_baslangic: .*/- mola_baslangic: -/' MEMORY.md

"bitti":
  session_durumu=idle, süre hesapla
  sed -i 's/- session_durumu: .*/- session_durumu: idle/' MEMORY.md
  sed -i "s/- bugun_aktif_sure: .*/- bugun_aktif_sure: YENİ_TOPLAM/" MEMORY.md
    YENİ_TOPLAM = mevcut + seans süresi (tam sayı, metin YOK)
  sed -i "s/- DERS_KISA_AD: son_calisma=.*/- DERS_KISA_AD: son_calisma=TARIH/" MEMORY.md
    DERS_KISA_AD = VYS/VYA/IELTS vb.
  bugun_aktif_dersler güncelle:
    "-" ise DERS_ADI yaz
    Doluysa mevcut + ", DERS_ADI (SURE dk)" ekle
  bu_hafta_sure güncelle:
  sed -i "s/- bu_hafta_sure: .*/- bu_hafta_sure: YENİ_TOPLAM/" MEMORY.md
    YENİ_TOPLAM = mevcut + seans süresi (tam sayı, metin YOK)

"günü bitirdim" / "yatıyorum" / "kapanıyorum":
  session_durumu=day_closed, Günlük Rapor Protokolü tetikle

### 3.3 Bölüm 3: Mikro Sorgu (4 Soru)
Tetikleme: SADECE "günü bitirdim/yatıyorum/kapanıyorum"
"bitti" komutunda ASLA sorma.

Sorular ve alanlar:
1. Odak nasıldı? → bugun_notu (iyi/orta/kötü)
2. 10 üzerinden kaç? → son_mikro_skor (sayı)
3. Neden düşük/yüksek? → sebep_etiketi
   Etiketler: uyku, telefon, kaygi, belirsizlik, zor_ders_kacinmasi, plansizlik, fiziksel_yorgunluk, dis_etken
4. Yarın planın var mı? → yarin_plan (evet/hayır)

### 3.4 Bölüm 4: Adaptif Günlük Planlayıcı

Ton belirleme (mod değiştirme değil, sadece ton):
- Dünkü odak_yuzdesi < 40 → sakin ve destekleyici ton
- mevcut_streak > 7 → sert ve yüksek beklenti tonu
- İkisi de yoksa → varsayılan ton

İçerik:
- Önceki gün sebep_etiketi'ne göre yön ver
- Streak durumu
- (haftalik_hedef - bu_hafta_sure) / kalan gün = bugün hedef dakika
- Vizeye kalan gün
- Bir önceki günden sarkan kalan_p1

### 3.5 Bölüm 8: Isınma Turu
Tetik: "başlayamıyorum / zorlanıyorum"
Motivasyon konuşması YAPMA.
MEMORY.md ise_yarayan_acilislar oku, kişisel öneri ver.
Sonra: Devam mı, mikro mola mı, minimumla mı kapatalım?

### 3.6 Bölüm 14: Günlük Rapor Zinciri (KRİTİK SIRA)

Adım 1-6: Verileri çek, özetle
Adım 7: Odak yüzdesi hesapla (V3):
  aktif_dakika = bugun_aktif_sure
  ortalama_blok = aktif_dakika / (bugun_mola_sayisi + 1)
  blok_skoru = min(100, (ortalama_blok / 45) * 100)
  hacim_skoru = min(100, (aktif_dakika / 150) * 100)
  bugun_odak_yuzdesi = round((blok_skoru * 0.75) + (hacim_skoru * 0.25))
  sed -i "s/- bugun_odak_yuzdesi: .*/- bugun_odak_yuzdesi: DEGER/" MEMORY.md

Adım 8: date +%Y-%m-%d

Adım 9: Notion'a kaydet:
  OZET = "Pasif: [bugun_pasif_aktiviteler] | Enerji: [enerji_modu] | Sebep: [sebep_etiketi] | Odak: %[bugun_odak_yuzdesi] | Not: [bugun_notu]"
  notion-rapor "daily" "TARIH" "AKTIF_SURE" "DERSLER" "OZET" "PASIF_AKTIVITELER" "ENERJI_MODU" "SEBEP_ETIKETI"
  NOT: Mikro sorgu cevapları sadece MEMORY.md'ye yazılır.
       Notion OZET'i SADECE yukarıdaki sabit formattan üretilir.

Adım 10: WEEK_BUFFER'a ekle:
  TARIH=$(date +%Y-%m-%d)
  OZET="- $TARIH: $bugun_aktif_dersler. Odak: $bugun_notu, Skor: $son_mikro_skor, Sebep: $sebep_etiketi."
  sed -i "/^# WEEK_BUFFER/a $OZET" MEMORY.md

Adım 10.5: PATTERNS'a sebep_etiketi işle:
  Son 3 günün sebep_etiketi aynıysa tetikleyici_engeller güncelle:
  sed -i "s/- tetikleyici_engeller: .*/- tetikleyici_engeller: ETIKET (ust_uste_N_gun)/" MEMORY.md
  4. gün sabahında Berk'e raporla.

Adım 11: TODAY bloğunu sıfırla:
  sed -i 's/- bugun_baslangic_saati: .*/- bugun_baslangic_saati: -/' MEMORY.md
  sed -i 's/- bugun_aktif_sure: .*/- bugun_aktif_sure: 0/' MEMORY.md
  sed -i 's/- bugun_mola_sayisi: .*/- bugun_mola_sayisi: 0/' MEMORY.md
  sed -i 's/- bugun_odak_yuzdesi: .*/- bugun_odak_yuzdesi: -/' MEMORY.md
  sed -i 's/- bugun_aktif_dersler: .*/- bugun_aktif_dersler: -/' MEMORY.md
  sed -i 's/- bugun_pasif_aktiviteler: .*/- bugun_pasif_aktiviteler: -/' MEMORY.md
  sed -i 's/- tamamlanan_p1: .*/- tamamlanan_p1: -/' MEMORY.md
  sed -i 's/- kalan_p1: .*/- kalan_p1: -/' MEMORY.md
  sed -i 's/- enerji_modu: .*/- enerji_modu: normal/' MEMORY.md
  sed -i 's/- son_mikro_skor: .*/- son_mikro_skor: -/' MEMORY.md
  sed -i 's/- bugun_notu: .*/- bugun_notu: -/' MEMORY.md
  sed -i 's/- sebep_etiketi: .*/- sebep_etiketi: -/' MEMORY.md
  sed -i 's/- sebep_notu: .*/- sebep_notu: -/' MEMORY.md
  sed -i 's/- yarin_plan: .*/- yarin_plan: -/' MEMORY.md
  sed -i 's/- ghost_mode_bugun: .*/- ghost_mode_bugun: 0/' MEMORY.md
  sed -i 's/- aktif_oturum_baslangic: .*/- aktif_oturum_baslangic: -/' MEMORY.md
  sed -i 's/- aktif_oturum_ders: .*/- aktif_oturum_ders: -/' MEMORY.md
  sed -i 's/- mola_baslangic: .*/- mola_baslangic: -/' MEMORY.md

Adım 12: Berk'e kısa özet gönder

### 3.7 Bölüm 16: Sınav Öncesi Mod
Vize haftasına 5 gün kaldığında devreye gir.
aktif_mod ALANINA DOKUNMA. Sadece Berk "/mod sinav" yazınca değişir.
- Mola 45 dk+ ise uyar
- Günlük hedef: 6-8 saat
- Gece sınav hazırlık değerlendirmesi

### 3.8 Bölüm 19: Haftalık Sıfırlama
Her Pazar haftalık rapor sonrası:
1. WEEK_BUFFER temizle
2. sed -i 's/- bu_hafta_sure: .*/- bu_hafta_sure: 0/' MEMORY.md
3. COURSES son_calisma bilgilerini koru

---

## 4. ODAK METRİĞİ — V3 FORMÜLÜ

Hesaplama:
  aktif_dakika = bugun_aktif_sure
  ortalama_blok = aktif_dakika / (bugun_mola_sayisi + 1)
  blok_skoru = min(100, (ortalama_blok / 45) * 100)
  hacim_skoru = min(100, (aktif_dakika / 150) * 100)
  odak_yuzdesi = round((blok_skoru * 0.75) + (hacim_skoru * 0.25))

Mantık: Az mola + uzun blok = yüksek odak. Çok mola + kısa blok = düşük odak.

Örnekler:
  90 dk, 1 mola  → ~78%
  60 dk, 3 mola  → ~33%
  120 dk, 2 mola → ~89%
  150 dk, 0 mola → ~100%

Notion Odak kolonu formülü:
  if(test(prop("Notlar"), "Odak: %\\d+"),
     toNumber(replaceAll(prop("Notlar"), ".*Odak: %(\\d+).*", "$1")) / 100, 0)

---

## 5. NOTION ENTEGRASYONu

Günlük rapor (8 parametre):
  notion-rapor "daily" "TARIH" "AKTIF_SURE" "DERSLER" "OZET" "PASIF_AKTIVITELER" "ENERJI_MODU" "SEBEP_ETIKETI"

OZET sabit format:
  "Pasif: X | Enerji: X | Sebep: X | Odak: %X | Not: X"

Örnek:
  notion-rapor "daily" "2026-03-20" "90" "Veritabanı Yönetim Sistemleri" "Pasif: - | Enerji: normal | Sebep: uyku | Odak: %67 | Not: iyi" "-" "normal" "uyku"

Haftalık: notion-rapor "weekly" "TARIH" "SURE" "OZET" "NOTLAR"
Aylık:    notion-rapor "monthly" "TARIH" "SURE" "OZET" "NOTLAR"

Süre her zaman TAM SAYI DAKIKA. "saat" veya "dk" metni yazma.

Canonical ders adları:
  VYS = Veritabanı Yönetim Sistemleri
  VYA = Veri Yapıları ve Algoritmalar
  IELTS = IELTS Hazırlık

notion-ders komutu V1.0'da KALDIRILDI. Kullanma.

---

## 6. CRON YÖNETİMİ

VPS crontab:
  0 12 * * * /home/openclaw/.openclaw/workspace/check_disk_usage.sh
  0 3 * * 0 rm -f /home/openclaw/.openclaw/agents/koc/sessions/*.jsonl && sudo systemctl restart openclaw

OpenClaw cron:
  openclaw cron list
  openclaw cron add --name "forcer-TIMESTAMP" --agent koc --at "+25m" --message "MESAJ" --channel telegram --account koc --to "telegram:6288779679" --announce
  openclaw cron rm ID

Cron sınırı: SADECE forcer- ile başlayanlara dokun.
ASLA dokunma: ilac-, randevu-, vize-, vps-, openclaw-

---

## 7. GÜNLÜK KULLANIM

Sabah:
  Plan listesi gönder (P1, P2, P3, Pasif)
  Forcer: sebep etiketine, streak'e, haftalık hedefe göre yön verir

Ders:
  başladım → [ders adı] → mola → devam → bitti

Gün sonu:
  günü bitirdim → 4 soru cevapla → rapor otomatik gider

Acil komutlar:
  /mod [isim]           → Mod değiştir
  kaydım bugün          → Dinamik yeniden planlama
  başlayamıyorum        → Isınma turu
  enerjim yok           → Düşüş günü protokolü

Aktif/Pasif ayrımı:
  Aktif: süre takibi, başladım/mola/devam/bitti akışı
  Pasif: süre YOK, sadece tamamlandı/tamamlanmadı

---

## 8. SORUN GİDERME

Değişiklik yansımadı:
  sudo systemctl restart openclaw
  rm ~/.openclaw/agents/koc/sessions/*.jsonl && sudo systemctl restart openclaw

MEMORY.md yanlış değer:
  cat ~/.openclaw/workspace-koc/MEMORY.md
  sed -i 's/- ALAN: .*/- ALAN: DEGER/' ~/.openclaw/workspace-koc/MEMORY.md

Sınav modu kendiliğinden açıldı:
  sed -i 's/- aktif_mod: .*/- aktif_mod: varsayilan/' ~/.openclaw/workspace-koc/MEMORY.md
  sudo systemctl restart openclaw

WEEK_BUFFER kirli:
  sed -i '/^- 2026-03-20/d' ~/.openclaw/workspace-koc/MEMORY.md

---

## 9. BİLİNEN KISITLAR

1. Aynı ders append çalışıyor, merge yok: "VYS (30 dk), VYS (25 dk)"
2. PATTERNS boş — dolması için haftalarca gerçek kullanım gerekiyor
3. Reset bazen eksik kalabilir — kritik değil ama izle
4. bu_hafta_sure gerçekten artıyor mu? — İlk hafta takip et

---

## 10. CLAUDE İÇİN KURALLAR

1. MEMORY.md tek gerçeklik kaynağı. Chat geçmişiyle çelişirse MEMORY.md kazanır.
2. sed kullan, write_file/edit KULLANMA.
3. bu_hafta_sure ve haftalik_hedef SAF TAM SAYI. Metin ekleme.
4. Mod Forcer tarafından değiştirilemez. Sadece Berk /mod yazınca.
5. Mikro sorgu sadece "günü bitirdim"de. "bitti"de SORMA.
6. Notion OZET sabit format. Mikro sorgu metnini OZET olarak gönderme.
7. Restart zorunlu — her değişiklik sonrası.
8. Session temizliği her Pazar 03:00 — MEMORY.md silinmez.
9. notion-ders komutu KALDIRILDI. Kullanma.
10. Bölüm 14 adım sırası kritik: 7→8→9→10→10.5→11→12

Her değişiklik: küçük, tek adım, test edilebilir. Büyük refactor yapma.

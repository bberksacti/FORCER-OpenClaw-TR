# FORCER V3 - OpenClaw Academic Operations Agent

FORCER, OpenClaw üzerinde çalışan kişisel bir akademik operasyon ajanıdır. Temel amacı ders çalışma sürecini yalnızca “plan yapma” seviyesinde bırakmadan; oturumları, molaları, ara durumlarını, günlük akademik ritmi ve haftalık ilerlemeyi takip edilebilir bir sisteme dönüştürmektir.

V3 sürümü, projenin en önemli yeniden yapılandırma adımıdır. Önceki sürümlerde biriken karmaşık prompt yapıları, eski cron akışları, kırılgan state güncellemeleri ve test verileri temizlenmiş; bunun yerine daha sade, güvenilir ve uzun süre kullanılabilir bir **hibrit ajan mimarisi** kurulmuştur.

Bu versiyonda FORCER artık iki katmanlı çalışır:

- **Deterministik çekirdek:** Kısa komutları yakalar, süreleri hesaplar, state dosyasını güvenli şekilde günceller.
- **LLM koçluk katmanı:** Raporları yorumlar, akademik durumu değerlendirir ve kullanıcıya gerçekçi bir sonraki hamle önerir.

Bu sayede FORCER yalnızca bir zamanlayıcı veya Telegram botu değil; akademik ritmi korumaya çalışan, veriye dayalı ama kişisel tonu olan bir çalışma ajanıdır.

---

## V3 ile Gelen Ana Değişim

FORCER V3, eski yapının üzerine küçük yamalar eklemek yerine çekirdek mantığı yeniden düzenler.

Önceki sürümlerde bazı işlemler LLM cevabına, uzun promptlara veya kırılgan dosya güncellemelerine bağlıydı. V3 ile birlikte kritik işlemler artık LLM’e bırakılmaz. Ders oturumu başlatma, bitirme, mola, devam, hgb, ara, kısa rapor ve gün kapatma gibi işlemler `forcer-router` üzerinden deterministik olarak yönetilir.

LLM tamamen devreden çıkarılmamıştır. Tam tersine, daha doğru yerde kullanılır: yorum, değerlendirme, koçluk ve akademik içgörü üretimi.

Kısaca:

```text
Mekanik ve kritik state işlemleri -> deterministic router
Yorum, analiz ve akademik koçluk -> LLM
```

Bu ayrım V3’ün temel mimari fikridir.

---

## Temel Özellikler

### Deterministik Oturum Takibi

FORCER, ders çalışma oturumlarını kısa komutlarla takip eder:

```text
başladım VYS
mola
devam
hgb
ara
bitir
```

Sistem aktif dersi, başlangıç saatini, aktif çalışma süresini, mola sayısını ve haftalık toplamı `MEMORY.md` üzerinden takip eder. Böylece sohbet bağlamı kaybolsa bile akademik durum korunur.

### Akıllı Ama Güvenli Komut Çekirdeği

V3’te temel komutlar LLM’e gitmeden çalışır. Bu, hem hız hem de güvenilirlik sağlar.

Desteklenen deterministik komutlar:

```text
başladım / basladim / başla / basla <ders>
mola
devam / döndüm / dondum / geldim
hgb / hemen geliyorum
ara
bitir / bitti / bitirdim
durum / akademik durum
kısa rapor / kisa rapor
GUNU_BITIR
```

Komutlar Türkçe karakter, büyük/küçük harf ve fazla boşluk farklarına karşı normalize edilir. Örneğin aşağıdaki komutlar aynı mantıkla algılanır:

```text
BAŞLADIM vys
KISA   RAPOR
DÖNDÜM
BİTİR
```

### LLM Destekli Raporlama

Kısa ve mekanik özetler deterministik üretilir. Daha esnek raporlar ise LLM katmanına bırakılır.

Örneğin:

```text
kısa rapor
```

komutu deterministik günlük özet verir.

```text
rapor
bugün nasıl gidiyorum
ne yapmalıyım
```

gibi ifadeler ise LLM tarafından yorumlanır. FORCER burada yalnızca verileri tekrar etmez; çalışma hacmi, ders dağılımı, haftalık hedef ve risk sinyallerinden yola çıkarak kısa bir akademik değerlendirme ve tek net sonraki hamle üretir.

---

## V3 Komutları

| Komut | Açıklama |
| :--- | :--- |
| `başladım <ders>` | Yeni ders oturumu başlatır |
| `mola` | Aktif çalışma bloğunu kapatır ve normal mola başlatır |
| `devam` | Moladan dönüp yeni aktif bloğu başlatır |
| `hgb` | Kısa mecburi ara başlatır; mola sayısını artırmaz |
| `ara` | Uzun mecburi ara başlatır; mola sayısını artırmaz |
| `bitir` | Aktif oturumu kapatır ve süreyi kaydeder |
| `durum` | Anlık akademik state özetini verir |
| `kısa rapor` | Günlük kısa sayısal rapor ve odak yüzdesi üretir |
| `rapor` | LLM destekli yorumlu akademik değerlendirme üretir |
| `GUNU_BITIR` | Günlük kapanış, rapor ve C-LOPAI bridge üretimi yapar |

---

## Ders Takibi

V3 çekirdeğinde dersler kanonik etiketlerle takip edilir. Kullanıcı Türkçe karakterli veya küçük harfli yazsa bile sistem bunları normalize ederek doğru derse eşler.

Örnek dersler:

```text
VYS
VYA
IELTS
Adli Bilisim
Sayisal Analiz
Mantik Devreleri
Matematik 2
Algoritma ve Programlama
```

Örnek kullanım:

```text
başladım sayısal analiz
bitir
```

Sistem bunu kanonik ders ismine eşleyerek state dosyasına işler.

---

## Gün Kapatma ve C-LOPAI Köprüsü

FORCER V3’ün en önemli parçalarından biri C-LOPAI ile kurulan günlük akademik köprüdür.

FORCER kendi başına bağımsız bir gün kapatma cron’u çalıştırmaz. Bunun yerine ana ajan olan **C-LOPAI**, merkezi gün kapatma akışında FORCER’a şu komutu gönderir:

```text
GUNU_BITIR
```

Bu komut çalıştığında FORCER:

- günlük akademik rapor dosyası üretir,
- C-LOPAI’nin okuyabileceği bridge dosyasını günceller,
- haftalık tampon dosyası olan `WEEK_BUFFER` satırını günceller,
- günlük sayaçları sıfırlar,
- `session_durumu` alanını `day_closed` yapar.

Üretilen ana bridge dosyası:

```text
workspace-koc/bridge/FORCER_DAILY_BRIDGE.md
```

Günlük rapor yolu:

```text
workspace-koc/reports/daily/YYYY-MM-DD.md
```

FORCER, 04:00 civarı yapılan otomatik kapanışlar için “akademik gün” mantığını destekler:

```text
00:00 - 04:59 -> bir önceki günün akademik kapanışı
05:00 ve sonrası -> mevcut günün kapanışı
```

Bu sayede gece geç saatlerde veya sabaha karşı yapılan otomatik kapanışlarda rapor tarihi doğru akademik güne yazılır.

---

## close_blocked Davranışı

C-LOPAI `GUNU_BITIR` komutunu gönderdiğinde FORCER aktif bir oturumdaysa, sistem oturumu otomatik kapatmaz. Bunun yerine güvenli şekilde reddeder ve C-LOPAI için bridge dosyasına durum sinyali yazar.

Örnek:

```text
Gun durumu: close_blocked
Sebep: active_session
Session: in_session
Aktif ders: VYS
```

Bu tasarımın amacı veri kaybını önlemek ve ana ajana açık bir sinyal vermektir. Böylece C-LOPAI, FORCER’ın neden günü kapatmadığını anlayabilir.

---

## Teknik Mimari

FORCER V3 şu ana bileşenlerden oluşur:

### OpenClaw Agent Framework

Ajan altyapısı OpenClaw üzerinde çalışır. FORCER, `koc` agent kimliğiyle yapılandırılmıştır.

### Gemini 2.5 Flash

V3’te FORCER için ana LLM modeli olarak **Gemini 2.5 Flash** kullanılır. Daha önceki timeout ve yanıt gecikmesi sorunları nedeniyle FORCER özelinde model override uygulanmıştır.

### forcer-router

Kısa komutların deterministik olarak yakalandığı router katmanıdır.

Ana dosya:

```text
extensions/forcer-router/index.js
```

Bu katman:

- Telegram mesajlarını yakalar,
- komutları normalize eder,
- `MEMORY.md` dosyasını güvenli şekilde günceller,
- günlük rapor ve bridge dosyalarını üretir,
- LLM’e gitmesi gereken mesajlara karışmaz.

### MEMORY.md

FORCER’ın canlı state dosyasıdır. Anlık oturum durumu, günlük süreler, mola verileri, haftalık hedef, ders son çalışma tarihleri ve uzun vadeli hedefler burada tutulur.

### Bridge ve rapor dosyaları

C-LOPAI ve diğer üst seviye ajanların okuyabileceği temiz çıktı dosyalarıdır.

```text
workspace-koc/bridge/FORCER_DAILY_BRIDGE.md
workspace-koc/reports/daily/
workspace-koc/WEEK_BUFFER
```

---

## Fiziksel Kontrol Katmanı: V2’den Kalan Donanım Deneyi

FORCER’ın gelişim sürecinde V2 sürümü, sistemi fiziksel kontrol katmanına taşıyan önemli bir deneydi. ESP32 tabanlı butonlar, LED durum göstergeleri ve VPS üzerinden Telegram’a komut ileten bir ara katman ile FORCER, yalnızca yazılımsal bir bot olmaktan çıkıp masa üstünde fiziksel olarak kontrol edilebilen bir çalışma sistemine dönüştü.

Bu katman V3’ün ana çekirdeği değildir; ancak projenin evriminde önemli bir aşamadır ve ileride tekrar sadeleştirilmiş şekilde entegre edilebilir.

Devre raporları ve görseller:

- [Forcer Devre Raporu 1](https://github.com/bberksacti/FORCER-OpenClaw-TR/blob/main/docs/FORCER_FIZIKSEL_DEVRE_RAPORU.pdf)
- [Forcer V2 Final Devre Şeması](https://github.com/bberksacti/FORCER-OpenClaw-TR/blob/main/docs/FORCER%20DEVRE%20V2.pdf)

V2 fiziksel kontrol demosu:

![Forcer V2 Physical Control Demo](https://github.com/user-attachments/assets/e0f2a57f-1608-4829-9f2e-b3e310c9c6cc)

Looker Studio test görseli:

<img width="507" height="1074" alt="FORCER Looker Studio Test Dashboard" src="https://github.com/user-attachments/assets/de9fcee7-d262-442d-a80c-7ab7afc7c871" />

---

## Projenin Evrimi

FORCER ilk aşamada basit bir çalışma zamanlayıcısı olarak başladı. Zaman içinde Notion entegrasyonu, streak takibi, fiziksel buton kontrolü, dashboard denemeleri, risk motoru fikirleri ve otomatik raporlama katmanları eklendi.

V3 ile birlikte proje daha olgun bir noktaya taşındı:

```text
V0.x -> temel oturum takibi
V1.x -> Notion, skor, streak, rapor denemeleri
V2.x -> fiziksel ESP32 kontrol katmanı
V3.x -> deterministic router + LLM koçluk + C-LOPAI bridge
```

Bu sürüm, geçmişteki deneylerden öğrenilenleri daha sade ve güvenilir bir çekirdeğe indirger.

---

## Mevcut Durum

FORCER V3-MVP itibariyle:

- deterministik komut çekirdeği çalışıyor,
- gün kapatma sistemi çalışıyor,
- C-LOPAI bridge dosyası üretiliyor,
- kısa rapor ve durum komutları hızlı çalışıyor,
- LLM rapor katmanı korunuyor,
- eski Forcer cron’ları temizlendi,
- test verileri temizlenerek gerçek kullanıma hazır state oluşturuldu.

Bu haliyle sistem kişisel kullanım için operasyonel bir V3-MVP seviyesindedir.

---

## Not

FORCER kişisel akademik operasyon ihtiyacından doğmuş bir sistemdir. Bu nedenle bazı veri modelleri, ders isimleri, hedef süreler ve karar eşikleri kişisel kullanım senaryosuna göre şekillenmiştir.

Buna rağmen proje; OpenClaw üzerinde çalışan hibrit ajan mimarisi, deterministik state yönetimi, LLM destekli koçluk yaklaşımı ve çok ajanlı C-LOPAI köprüsüyle daha genel akademik takip ve kişisel üretkenlik sistemleri için güçlü bir temel sunar.

FORCER V3’ün ana fikri basittir:

```text
Az komut.
Temiz state.
Güvenilir takip.
Akıllı ama kontrollü yorum.
```

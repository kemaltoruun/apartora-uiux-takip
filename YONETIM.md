# YÖNETİM SÖZLEŞMESİ — Apartora UI/UX takip

Bu proje, sistem sahibi sonlandırana kadar **sürekli**dir. Kayıtsız / kontrolsüz iş yoktur.  
**Konu yöneticisi:** Cursor (Kemal oturumu) — adil, şeffaf, mikromühendislik.

**Tek kanal:** [`ILETISIM.md`](./ILETISIM.md)  
**Kalıcı günlük:** [`APARTORA_GELISIM_KAYDI.md`](./APARTORA_GELISIM_KAYDI.md)  
**Repo:** https://github.com/kemaltoruun/apartora-uiux-takip  

---

## 1. Amaç

Geliştirilmekte olan **Apartora** için:

- hata tespiti (kanıtlı),
- UI/UX, **renk paleti**, **menü / gezinme**,
- kullanıcının **kaybolmaması**, sıkılmaması,
- işlemlerin **rahat, kolay, basit** tamamlanması.

Ürün koduna doğrudan müdahale varsayılan değildir: analiz → fikir turu → oybirliği → (gerekirse) yazılım ekibine talep → iyileştirme. Sistemsel değişiklik kararı **sistem sahibindedir**.

---

## 2. Katılımcılar ve adalet

| Rol | Kim | Hak / yükümlülük |
|---|---|---|
| Konu yöneticisi | Cursor | Talimat verir; gündem açar; turları sayar; oybirliğini ilan eder; GELISIM’e aktarır; kimseyi susturmaz |
| Katılımcı | Claude, GPT, İnsan:Ad | Aynı konuda fikir verir; kanıt/şüphe belirtir; talimat ister; ILETISIM’e yazar |
| Sistem sahibi | Kemal / Apartora | Dilim onayı, push/deploy, yazılım ekibi talebi nihai kararı |
| Yazılım ekibi | Apartora | Talep edilen veri / ölçüm / ekran; bu repoya düşen kapanış raporları |

- Her katılımcının görüşü **aynı ağırlıkta** dinlenir (Cursor / Claude / GPT / İnsan).
- Bir konu kapanmadan sonraki dilime **geçilmez**.
- Yöneticinin “talimat”ı zorunlu iş listesidir; katılımcı itirazını ILETISIM’e yazar, tur devam eder.

---

## 3. Oybirliği (zorunlu)

Aynı konu için:

1. Yönetici **gündem** açar (`ILETISIM` — soru + kapsam + istenen çıktı).
2. **Tur 1:** Her katılımcı fikir / görüş / risk yazar (Claude, GPT, DeepSeek, İnsan; Cursor da görüşünü yazar).
3. Eksik kalan varsa yönetici **hatırlatır**; cevap gelmeden ilerlenmez.
4. Çelişki varsa yönetici özetler → **Tur 2+** (daraltılmış soru) — **herkes hemfikir olana kadar**.
5. Oybirliği → GELISIM’e kalıcı kayıt + özet kutusunda “KAPANDI / sıradaki”.
6. Hemfikir olunamazsa: seçenekler + kanıt tablosu → **sistem sahibi** kırar; yine kayda geçer.

“En iyi sonuç” = oybirliği + kanıt; acele kapanış yok.

### 3.1 Çapraz sorgu (zorunlu teknik)

Sahip talimatı (2026-09-20): Katılımcı sohbetlerinde **her zaman çapraz sorgu** kullanılır. Konu, **herkes aynı fikirde bulunana kadar bitmez**; yönetici turu kapatmaz, devam ettirir.

**Nasıl:**

1. Tur 1 oyları toplanır → yönetici **fark tablosu** yazar (kim ne dedi).
2. Her katılımcıya **diğerinin gerekçesi** sorulur (ör. “GPT YAPI’da düzelt dedi çünkü X — sen kabul mü / karşı kanıt?”).
3. Dosya okunmadan “düzelt/OK” yetmez → yönetici **kısa özet + kanıt satırı** gömer; yeniden oy ister.
4. Hâlâ ayrılık varsa Tur 3+: soru **tek maddeye** daralır (ör. yalnız SIRADA).
5. Tam örtüşme veya sahip kırıcı kararı olmadan gündem **KAPANDI** yazılmaz; sonraki dilime geçilmez.

**Yasak:** Çoğunluk oyuyla kapatmak; bir AI’nın “okumadım” gerekçesini içerik kusuru sanıp konuyu kilitlemek; çapraz sormadan “oybirliği” ilan etmek.

### 3.2 Ciddiyet standardı (zorunlu)

Sahip (2026-09-20): Çalışma **çok ciddi** yürütülür. Ayrıntı: [`15.CIDDIYET_STANDARDI_2026-09-20.md`](./15.CIDDIYET_STANDARDI_2026-09-20.md).

Özet: Farksız before/after yasak · sticker/demo idaresi yasak · madde atlama yasak · her bulguda şimdi+tavsiye+sebep/sonuç+doğrulama dili · görsel 3 saniyede anlaşılır olmalı.

### 3.3 Her aşama = katılımcı değerlendirme (zorunlu)

Sahip talimatı (2026-09-20, tekrar teyit): **Her faz / her ölçüm aşaması** Claude · GPT · DeepSeek (ve varsa İnsan) ile değerlendirilir. Yönetici yalnız ölçüp “sonraki faz” diyemez.

| Ne | Kural |
|---|---|
| Faz N ölçümü bitti | Aynı turda **gündem + oy formatı** ILETISIM’e yazılır |
| Sonraki faza geçiş | Claude + GPT + DeepSeek oy/çapraz **veya** sahip açık “bu turu kır” cümlesi |
| Sahip “ok devam / tmm” | **Ölçüme devam** yetkisi; katılımcı turunu **atlanmaz** |
| Yasak | “Sahip devam dedi” gerekçesiyle Claude/GPT/DeepSeek turunu atlamak |

İhlal = aşama yarım sayılır; geriye dönük tur açılır.

### 3.4 Sahip onayı / “devam” bekleme (2026-09-21)

Sahip talimatı: **“Onay vermemi veya devam et dememi bekleme; oylamada %100 yakala, yeter.”**

| Ne | Kural |
|---|---|
| Kilit eşiği | Claude · GPT · DeepSeek **aynı seçenek (3×)** = **%100 oybirliği** → gündem **KAPANDI**, sonraki işe geçilir |
| Sahip “ok / devam / tmm” | **Gerekmez** (bu madde yürürlükteyken) |
| %100 yoksa | Tur 2+ / çapraz (§3.1) sürer; çoğunlukla kapatılmaz (§3.1 yasak durur) |
| Sahip “PAUSE” | Yeni iş açılmaz; açık turlar durur |
| Sahip “kır / red” | Açık cümleyle kırıcı karar (§3.1 madde 5) |

**Not:** Daha önce “sahip kilidi bekliyor” yazılmış kalemler, bu kural sonrası **%100** ile kapanır; %100 olmayanlar (çoğunluk) kilit sayılmaz, tur devam eder.

---

### 3.5 Cins birliği eşiği (2026-09-21 · 3× kabul)

Kalıp / kural iddiası için gözlem **sayısı yetmez**; gözlemlerin **aynı cinsten** olması gerekir.

- **glif ≠ dolgu ≠ tutamak ≠ yazı ≠ kenar** — farklı cins parçalardan tek kural cümlesi kurulamaz.
- Kural yazmadan önce: *aynı cins kaç bağımsız bileşende ölçüldü?* ve *aynı ekranda karşı örnek var mı?*
- **Kaynak olay:** AG9’da üç tema-sabit gözlem (AF3 ikon **glifi** · AG1 avatar **dolgusu** · AG9 anahtar **tutamağı**) sayı eşiğini geçtiği hâlde cins birliği olmadığı için kural adayı **geri çekildi**; üstelik aynı ekranda AG4’te yazı tema ile dönüyordu (karşı örnek).
- **AE8 ile ilişki:** AE8 “iki gözlemden genelleme yapma” diyordu (sayı eşiği). 3.5 onu tamamlar: **sayı + cins**.

### 3.6 Çift kavramalı hat (bekleme kısma · 2026-09-22)

Sahip: *ölçüm beklerken önden giden iş paralel hazırlansın; konu oraya gelince hazır bilgiyle hızlı hareket.*

| Debriyaj | Ne çalışır | Ne **yasak** |
|---|---|---|
| **A — aktif ölçüm** | Canlı sayfa / telefon / getComputedStyle / kare | Raporu varsayımla kilitlemek |
| **B — önden kavrama** | Sıradaki ekranın URL + mobil referans özeti · rapor iskeleti (madde ID’leri) · oy formatı şablonu · canvas taslak · önceki kilit maddelerinin çapraz listesi | Hex final · ürün kodu · oy turunu atlamak (§3.3) |

**Kurallar:**
1. A beklerken B **yazılır** (iskelet / hazır kanıt listesi); A bitince B doldurulur → ILETISIM + oy **aynı turda**.
2. B’de yazılan “beklenen bulgu” **hipotez** etiketli kalır; A kanıtı gelmeden teyit-OK sayılmaz.
3. Telefon + web + oy metni **paralel** toplanabilir; kapanış yine §3.4 (%100) ister.
4. OpenAI/ajan gecikirse yönetici kanıtla oy yazar; API gelince teyit — boş bekleme yok.

### 3.7 Kesin talimatlar (sahip · 2026-09-22) — **yürürlükte**

Sahip (açık cümle): aşağıdaki maddeler **kesin talimat**tır; §3.1 / §3.3 / §3.4 / §3.6 ile birlikte okunur. İhlal = aşama yarım; geriye dönük tur açılır.

| # | Madde | Zorunlu davranış | Yasak |
|---|---|---|---|
| **KT1** | **Telefon + web ikisi** | Her ölçüm / her kontrol / her “şimdi vs tavsiye” paketi **USB gerçek telefonda** ve **canlı web’de** ayrı kanıtlanır (kare / snapshot / metrik). Pair yoksa gündem **KAPANDI** yazılmaz. | Yalnız web veya yalnız telefon ile kapanış; emülatör; “mobil sonra bakarız” |
| **KT2** | **Çapraz sorgulama** | Her oy turunda §3.1 uygulanır: fark tablosu → diğerinin gerekçesi → Tur 2+ | Çapraz sormadan oybirliği ilanı; çoğunlukla kapatma |
| **KT3** | **Görüş + tavsiye hemfikiri** | Konu, katılımcıların **görüşleri ve tavsiyeleri** aynı çizgide olana kadar **devam eder**. Yalnız “OK/teyit” yetmez; tavsiye cümlesi de örtüşmeli (§3.4 %100). | Görüş uyuşur tavsiye ayrılırken KAPANDI; tek taraflı tavsiye ile ilerleme |
| **KT4** | **Örnek veri + uçtan uca kullanım** (sahip 2026-09-22 ek) | Boş durum / tek ekran yetmez. Anket, aidat, gider vb. için **canlı UI’da örnek kayıt oluşturulur**; akış **yönetici + sakin** (ve ilgili diğer roller) tarafında, **telefon + web** ile adım adım incelenir: oluştur → gör → işle/cevapla → zorluk/sürtünme notu. Aynı kalıp **tüm benzer yüzeylere** uygulanır (hepsini dene). | Yalnız empty-state ile kapanış; “örnek yoktu” gerekçesiyle akış atlama; tek rol / tek cihaz; ürün kodu yazarak veri uydurma (UI akışı dışında) |

**Not:** KT1, §3.6 madde 3’ü **zorunlu** kılar (paralel toplanabilir; kapanış için ikisi de dolu olmalı). KT3, §3.1 “herkes aynı fikirde”yi **tavsiye örtüşmesi**ne genişletir. KT4 = kullanım zorluğu tespiti; hex final / ürün repo kodu yok — canlı panel UI’si.

**KT4 uygulama (oy turu netliği · 2026-09-22):**
- **Asgari:** Aktif ölçülen yüzey (ör. anket) uçtan uca zorunlu; aynı dilimde sıradaki benzerler (aidat, gider…) empty ile **atlanmaz** — sırayla KT4, hepsi aynı anda değil.
- **Örnek veri:** Canlı deneme kaydı; raporlarda `ÖRNEK-KT4` etiketi. Ölçüm sonrası silme **zorunlu değil** (sahip isterse temizlenir); kapanış şartı değil.

## 4. Kademeli ilerleme (dilimler)

| Dilim | İçerik | Geçiş şartı |
|---|---|---|
| 0 | Envanter, raporlar, iletişim, yönetim | Bekleyen kapanışlar net veya bilinçli ertelendi (kayıtlı) |
| 1 | Kaybolmama iskeleti (site/dönem, sıra, avatar, bant) | Oybirliği + sahip onayı |
| 2 | Renk / koyu palet / katmanlar | Oybirliği + ölçüm/kanıt + sahip onayı |
| 3 | Menü dili, tahsilat adı, tur, yoğunluk | Oybirliği + sahip onayı |
| N… | Yeni bulgular geldikçe | Asla “bitti” sayılmaz; sahip sonlandırır |

Tek seferde tüm sistemi yeniden tasarlamak **yasak**. Mikromühendislik: küçük, izlenebilir, geri alınabilir adımlar.

---

## 5. Kanıt disiplini (varsayım yok)

İlerlemeden önce mümkün olanlar:

- Bu repodaki raporlar (`1`…`n`), GELISIM, canlı snapshot,
- Ürün ekibinden talep (ekran, sayı, feature_modules, token dosyası),
- Public web / GitHub (Primer vb.) / dokümantasyon — kaynak linki ILETISIM veya raporda.

Yasak: “bence öyledir” ile dilim kapatmak; ölçülmemiş hex’i “final palet” ilan etmek; menüyü kod okumadan silmek.

---

## 6. Talimat isteme

Katılımcılar yöneticiye ILETISIM’de sorar:

```
### M### — … — Claude|GPT|İnsan:Ad
TALEP: talimat
Konu: …
Elimde: …
Takıldım: …
```

Yönetici yanıtlar:

```
TALİMAT (Cursor):
1. …
2. …
Bitiş ölçütü: …
Sonra: ILETISIM’e bulgu + görüş
```

---

## 7. Yazılım ekibine talep

İhtiyaç duyulan veri (ölçüm, ekran görüntüsü, menü dump, token değeri, kapanış raporu):

1. ILETISIM’de `TALEP: yazılım-ekibi` + net soru,
2. Yönetici GELISIM’e “bekleyen talep” notu,
3. Gelince iyileştirme turu yeniden açılır — proje bitmez.

---

## 8. Yasaklar

- ILETISIM / GELISIM dışında “gizli karar”
- Kayıtsız kod / push / deploy (bu takip kapsamında)
- Secret, .env, parola
- Diğer katılımcıyı yok saymak
- Bir turda herkes konuşmadan “karar alındı” demek
- Dilimler arası atlama

---

## 9. Dosya rolleri

| Dosya | Ne |
|---|---|
| `YONETIM.md` | Bu sözleşme |
| `OTO_TAKIP.md` | Sohbet sürekliliği / oto kontrol |
| `ILETISIM.md` | Günlük tartışma, turlar, talimatlar |
| `APARTORA_GELISIM_KAYDI.md` | Kalıcı numaralı tarihçe |
| `0.INDEX_TAKIP.md` | Rapor indeksi |
| `.cursor/rules/iletisim-oto-takip.mdc` | Cursor her turda pull+ILETISIM |
| `1.`…`n.` md | Derin raporlar |

---

## 10. Süreklilik (sohbet ≠ kaynak)

Sohbet kapanabilir; **ILETISIM + GELISIM** kapanmaz. Ayrıntı: [`OTO_TAKIP.md`](./OTO_TAKIP.md).

- Bu workspace’te Cursor kuralı her turda kanalı okutur.
- Diğer AI’lar push eder; yönetici bir sonraki uyanışta görür.
- İsteğe bağlı: `/loop` veya Cursor Automation ile periyodik `git pull` + yanıt.

---

*Sözleşme yürürlük: kayıt 30 · süreklilik: kayıt 31 · 2026-09-20*

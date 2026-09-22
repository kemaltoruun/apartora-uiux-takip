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
| Katılımcı | Claude, GPT, DeepSeek, Gemini, İnsan:Ad | Aynı konuda fikir verir; kanıt/şüphe belirtir; talimat ister; ILETISIM’e yazar |
| Sistem sahibi | Kemal / Apartora | Dilim onayı, push/deploy, yazılım ekibi talebi nihai kararı |
| Yazılım ekibi | Apartora | Talep edilen veri / ölçüm / ekran; bu repoya düşen kapanış raporları |

- **Oy sırası (zorunlu · 2026-09-22):** Claude → GPT → DeepSeek → **Gemini** (4. katılımcı). Gündem / fark tablosu / KAPANDI sayımında **dördü de** yazılır; Gemini atlanırsa tur yarım.
- Her katılımcının görüşü **aynı ağırlıkta** dinlenir (Cursor / Claude / GPT / DeepSeek / Gemini / İnsan).
- Bir konu kapanmadan sonraki dilime **geçilmez**.
- Yöneticinin “talimat”ı zorunlu iş listesidir; katılımcı itirazını ILETISIM’e yazar, tur devam eder.

---

## 3. Oybirliği (zorunlu)

Aynı konu için:

1. Yönetici **gündem** açar (`ILETISIM` — soru + kapsam + istenen çıktı).
2. **Tur 1:** Her katılımcı fikir / görüş / risk yazar (Claude, GPT, DeepSeek, Gemini, İnsan; Cursor da görüşünü yazar).
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

Sahip talimatı (2026-09-20, tekrar teyit; Gemini eklendi 2026-09-22): **Her faz / her ölçüm aşaması** Claude · GPT · DeepSeek · Gemini (ve varsa İnsan) ile değerlendirilir. Yönetici yalnız ölçüp “sonraki faz” diyemez.

| Ne | Kural |
|---|---|
| Faz N ölçümü bitti | Aynı turda **gündem + oy formatı** ILETISIM’e yazılır |
| Sonraki faza geçiş | Claude + GPT + DeepSeek + Gemini oy/çapraz **veya** sahip açık “bu turu kır” cümlesi |
| Sahip “ok devam / tmm” | **Ölçüme devam** yetkisi; katılımcı turunu **atlanmaz** |
| Yasak | “Sahip devam dedi” gerekçesiyle Claude/GPT/DeepSeek/Gemini turunu atlamak |

İhlal = aşama yarım sayılır; geriye dönük tur açılır.

### 3.4 Sahip onayı / “devam” bekleme (2026-09-21)

Sahip talimatı: **“Onay vermemi veya devam et dememi bekleme; oylamada %100 yakala, yeter.”**

| Ne | Kural |
|---|---|
| Kilit eşiği | Claude · GPT · DeepSeek · Gemini **aynı seçenek (4×)** = **%100 oybirliği** → gündem **KAPANDI**, sonraki işe geçilir |
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
4. OpenAI/Gemini/ajan gecikirse veya kota dolarsa → **§3.9 BEKLEME-TOKEN** (katılımcı bekler; oto B devam; `YONETICI-GECICI` mümkün; 4× gelmeden KAPANDI yok).
5. **ANTI-DURAK (sahip · 2026-09-22):** Subagent/shell/oy bildirimi veya `git push` **tur sonu değildir**. Gündem KAPANDI / sahip `PAUSE` değilse yönetici aynı uyanışta `SIRADAKI:` adımını yürütür. “devam?” sormak yasak. Ayrıntı: `.cursor/rules/iletisim-oto-takip.mdc` · `OTO_TAKIP.md`.

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

### 3.8 UX referans seti REF-01…20 (sahip · 2026-09-22) — **yürürlükte**

Kaynak envanter: [`82.UX_REFERANS_TALIMAT_ENVANTERI_2026-09-22.md`](./82.UX_REFERANS_TALIMAT_ENVANTERI_2026-09-22.md).  
Oy: GELISIM **145/147** (01…16) · **152** (17…20 REF-EK %100).

| Ne | Kural |
|---|---|
| Zorunluluk | Her mobil+web UI/UX / görsel / menü / renk / akış ölçümünde REF kontrol listesi uygulanır |
| Bulgu formatı | Birincil `REF-xx` · kanıt · şimdi · tavsiye · kabul ölçütü (1 cümle). Aynı bulguya **tek birincil** REF |
| Üstünlük | **K4 (§3.7 KT1–KT4 + ciddiyet + §3.5) kazanır** — REF ile çelişirse K4 |
| İşaretçiler | REF-14=KT1 · REF-15=KT4 · REF-16=ciddiyet (çift birincil yok) |
| Sınırlar (E-rev) | 09=yanıt gecikmesi · 10=süregelen durum · 11=eylem-sonrası doğrulama · 06=öncelik · 07=grup/aynı-rol |
| **REF-EK (17…20)** | 17=mikro+reduced-motion (≠09/13) · 18/19 **koşullu** (N/A+gerekçe) · 20=Security+Clean saha (offline sahte başarı yasak) · estetik maskeleme=06/16 not |
| Yasak | REF’siz “genel UX kötü” iddiası; telif uzun alıntı; hex final |

**Uygunluk (yönetici özeti):** Set Apartora takip için **uygun** — denetçi kontrol listesi. 01…13 içerik; 14–16 kapı köprüsü; 17–20 ek (18/19 koşullu, 20 portal sınırlı).

### 3.9 Token / kota BEKLEME + OTO-KATILIM-YENILE + YEDEK-SLOT (sahip · 2026-09-22) — **yürürlükte**

**Değerlendirme (yönetici):** Evet — **dolan katılımcı** bekletilir; hayır — **tüm oto hattı** bekletilmez. **Süre / kota dönünce oto yeniden katılım zorunlu**. **Katılımcı sayısı her turda 4× dolu tutulur** (sahip · 2026-09-22): kota dolanı beklemeye alınca **boştaki ajan/model varsa hemen yedek** — slot boş bırakılmaz.

| Durum | Zorunlu | Yasak |
|---|---|---|
| API 429 / kota / token / Composer-dolu | O kimlik **BEKLEME-TOKEN** + özet: kim · neden · `yeniden=` | Sessiz atlama; 3× KAPANDI |
| **YEDEK-SLOT (4× DOLU)** | Aynı turda **boşta ajan varsa** o slotu doldur: etiket `YEDEK: Asıl→YedekModel` (ör. `YEDEK: GPT→Grok`). Oy ILETISIM’de **asıl kimlik satırında** yazılır, NOT’ta yedek belirtilir. **Hedef her zaman 4 oy** | Boş slot ile turu ilerletmek; “3 yeter”; yedek varken beklemek |
| Yedek havuz (öncelik) | Kota/context’i **açık** olan: Grok · Claude Task · GPT Task · OpenAI MCP · diğer boş model — **Composer doluysa Composer’a yeni tur verme** | Aynı dolu modele tekrar basmak |
| Diğer oy hazır | Gündem sürer; BEKLEME + YEDEK paralel | “Çoğunluk yeter” |
| Oto (beklemede) | §3.6 **B** devam | Idle “token bekleniyor” |
| Geçici oy | `YONETICI-GECICI` mümkün; asıl/yedek gelince **TEYIT/DUZELT** | Geçiciyi nihai saymak |
| **OTO-KATILIM-YENILE** | `yeniden=` / kota reset → **asıl kimliğe** otomatik yeniden sor; asıl oy gelince yedek **TEYIT** veya **DUZELT** ile kapanır | Unutmak; süre dolunca sormadan KAPANDI |

**Oto bağ (zorunlu sıra):**
1. Kota düştü → `BEKLEME-TOKEN: Kim · neden · yeniden=…`
2. **Hemen** boş ajan tara → varsa `YEDEK-SLOT` ile 4. oy iste (sahip “devam” yok).
3. Ölçüm/B işi sürer (hattı dondurma).
4. `yeniden=` doldu → **asıl** kimliğe otomatik yeniden sor.
5. 4× (asıl veya yedek dolu) → çapraz → KAPANDI veya Tur 2+.

**§3.6 madde 4 ile ilişki:** `YONETICI-GECICI` acil kaçış; §3.9 = BEKLEME + YEDEK-SLOT + OTO-KATILIM-YENILE. Kilit yine §3.4 **4×**.

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
### M### — … — Claude|GPT|DeepSeek|Gemini|İnsan:Ad
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
| `GEMINI_KOPRU.md` | Cursor ↔ Gemini API köprüsü |
| `82.UX_REFERANS_…` | REF-01…20 zorunlu inceleme seti (§3.8) |
| `84.APARTORA_TASARIM_SIRLARI_…` | Tasarım sırları inceleme + REF-EK kaynağı |
| `15.CIDDIYET_…` | Ciddiyet standardı |
| `1.`…`n.` md | Derin raporlar |

---

## 10. Süreklilik (sohbet ≠ kaynak)

Sohbet kapanabilir; **ILETISIM + GELISIM** kapanmaz. Ayrıntı: [`OTO_TAKIP.md`](./OTO_TAKIP.md).

- Bu workspace’te Cursor kuralı her turda kanalı okutur.
- Diğer AI’lar push eder; yönetici bir sonraki uyanışta görür.
- İsteğe bağlı: `/loop` veya Cursor Automation ile periyodik `git pull` + yanıt.

---

*Sözleşme yürürlük: kayıt 30 · süreklilik: kayıt 31 · Gemini 4. katılımcı: kayıt 143 · 2026-09-22*

# APARTORA — Gelişim Kaydı

Her yenilik yeni bir numarayla sona eklenir. Eski kayıtlar değiştirilmez; bir kaydın durumu değişirse yeni bir kayıt açılır ve eskisine atıf yapılır.

**Rolümüz:** Analiz ve tavsiye. Sistemsel değişiklik kararı sistem sahibindedir.
**Takip kuralı:** Her kalıcı değişiklik (karar, rapor dosyası, dilim onayı, kapanış) yeni numarayla **sona** eklenir. Eski kayıt metni değiştirilmez; durum değişince yeni kayıt + eski numaraya atıf.
**Klasör:** `C:\Users\Kemal\Desktop\APARTORA` — numaralı md raporlar burada; bu dosya tek gelişim günlüğüdür.
**Son güncelleme:** 20 Eylül 2026 · Son kayıt no: 53

---

## Kayıtlar

### 1 — Mevcut durum envanteri alındı (19.09.2026)
Claude Code'un çıkardığı `APARTORA_MEVCUT_DURUM_ENVANTERI.md` incelendi (mobil yönetici Özet ve Aidat ekranları, iOS'ta doğrulanmış). Dört tutarsızlık belirlendi: paralel oturumda yarım değişiklik, aynı adı taşıyan iki "Toplu Tahsilat" akışı, rozet ile sekmenin farklı kurallardan beslenmesi, tanıtım turunun dokunuşları yutması.

### 2 — Çalışma sırası belirlendi (19.09.2026)
Önce tutarsızlıklar (1: yarım değişiklik, 2: Toplu Tahsilat, 3: durum rozeti, 4: tanıtım turu), sonra UI/UX (menü, başlık, alt başlık).

### 3 — Adım 1 prompt'u gönderildi: yarım değişiklik
`dues-list-screen.tsx` içindeki commit'lenmemiş değişikliğin güvenle sonuçlandırılması.
**Durum:** Kapanış raporu bekleniyor. UI/UX raporu değişikliğin `d6cca8148` ile commit'lendiğini gösteriyor; teyit edilmedi.

### 4 — Proje kuralları incelendi (19.09.2026)
`CLAUDE.md` ve `.claude/rules` (api, coding, deployment, testing, ui-components, validation) okundu. Prompt'lara yansıyan önemli kurallar:
- `npm run test:mobile` Expo testi değil, web Playwright'tır; mobil test `cd apps/mobile && npm test`.
- Web testleri gerçek `.env.local` yükler; mock'lanmamış test üretime yazabilir.
- API'de `ctx.supabase` service-role'dür; RLS korumaz, site kapsamı sorguda doğrulanmalı.
- Bug fix'te önce 2–3 yaklaşım, onay, sonra kod.

### 5 — Adım 2 prompt'u gönderildi: iki Toplu Tahsilat (salt-okuma)
Kalem bazlı modal ile daire bazlı FIFO akışı karşılaştırılıyor; mükerrer tahsilat, pending_approval, not ezilmesi, site kapsamı ve giriş noktaları doğrulanıyor.
**Durum:** Rapor bekleniyor. Ürün kararı ertelendi.

### 6 — Adım 3 tamamlandı: durum rozeti (19.09.2026)
Rozet artık sekmeyle aynı kuraldan (`toManagerDuesAxes`) çiziliyor. Web aidat detay diyaloğu da düzeltildi (4.572 kalem tabloda "Bekliyor", diyalogda "Ödenmedi" görünüyordu).
- Commit'ler: `829717439` (mobil), `f68a88de1` (web)
- **Push yapılmadı.** Yerel `main`'de başka oturumlara ait 3 commit var (`be2778844`, `7f8f4dbe9`, `91997e91e`); push öncesi netleşmeli.
- Açık kalanlar: `update_overdue_dues_status` UTC gününe bakıyor (İstanbul'da her gece 00:00–03:30 ham statü yanlış); `pending_approval` üç mobil kancada düşüyor (mükerrer tahsilat riski, Adım 2 ile birlikte ele alınmalı); web ay detayı Arapça'da çevrilmemiş; web "Onay Bekliyor" filtresi hiçbir satırla eşleşmiyor.

### 7 — Adım 4 prompt'u gönderildi: tanıtım turu
Kök neden kanıtlanmadan çözüm seçilmeyecek.
**Durum:** Rapor bekleniyor.

### 8 — UI/UX incelemesi ve tasarım sistemi denetimi alındı (19.09.2026)
Menü/başlık yerleşimi incelemesi (K, Ö, A maddeleri) ve tasarım sistemi denetimi okundu. Not: NativeWind 1rem = 14 olduğu için mobil düğme ve ikonlar hedeflenen boyuttan küçük (dokunma hedefi sınırın altında).

### 9 — UI/UX ilk paket prompt'u gönderildi: K6, Ö4, A1
Başlık erişilebilirlik rolü, hızlı işlem düğmesi etiketi, Aidat başlığına dönem + site (A1 koşullu).
**Durum:** Rapor bekleniyor.

### 10 — Gezinme paketi prompt'u hazırlandı: K7, K3(b), Ö5
Site adı her sekmede, avatar her yerde aynı yeri açsın, Özet'te mali durum yukarı.
**Durum:** Hazırlandı; gönderimi teyit edilmedi.

### 11 — Primer (GitHub tasarım sistemi) incelemesi
Temeller (tipografi, renk, yerleşim, içerik, ikonlar, duyarlı tasarım), Figma kütüphaneleri, arayüz desenleri ve senaryo desenleri incelendi; Next.js ve React Native'e uyarlandı. Bazı sayfalar yalnız arama özetlerinden okundu. Çıkan tavsiyeler aşağıdaki listede.

### 12 — Rol netleşti (20.09.2026)
Analiz ve tavsiye veriyoruz; uygulama prompt'u üretmek yerine sonuç raporu hazırlanıyor.

### 13 — Görsel format kuralı (20.09.2026)
Her ekran: yan yana eski/yeni mobil görüntü, numaralı değişiklikler, "ne değişti / neden / faydası" tablosu. Her görselin başlığında hangi portal olduğu (yönetici / sakin) yazılır.

### 14 — Ekran görselleri hazırlandı
- Ekran 1: Özet (yönetici)
- Ekran 2: Aidat listesi (yönetici)
- Ekran 7a–7b: Sakin ana sayfası ve ödeme ekranı (temsili; sakin portalının envanteri yok)

### 15 — Araç değerlendirmesi
- **Impeccable:** En uygun; "ürün modu" var, mevcut tasarım sistemine göre çalışıyor. Web odaklı, React Native desteği belirsiz. Önce web'de salt-okuma `audit` / `critique` ile pilot önerildi.
- **Taste Skill:** Uygulama ekranları için uygun değil (kendisi panelleri ve ürün arayüzlerini kapsam dışı sayıyor); yalnız tanıtım sitesi için düşünülebilir.
- **Figma MCP:** Şu an gerek yok; kod doğruluk kaynağı olarak kalmalı.
- **Playwright:** Zaten repoda; React Native uygulamasını test etmez.

### 16 — İngiltere pazarı kısa araştırması
Tek hâkim sistem yok (MRI, Blockman, Fixflo, Landlord Vision, Tilt, Inox). Ortak öne çıkanlar: otomatik banka mutabakatı, sakinin kendi hesabını görebildiği mobil portal.

### 17 — Renk sistemi sonucu (20.09.2026)
- Renkler ekranlara doğrudan değil, roller üzerinden (tek token dosyası, `packages/shared`).
- Zemin/metin: gündüz fildişi #F5F2EC + mürekkep #16181D; gece #111214 + fildişi #EFEBE3.
- **Düğme renkleri eyleme göre ve iki temada birebir aynı:** onay yeşil #2F7A57, bildirim mavi #2F6DB0, uyarılı onay kehribar #C98A1B, yıkıcı bordo #B23A2E, oluşturma lacivert #1E3A5F, nötr çerçeveli.
- Durum renkleri düğmelerle aynı aileden (Tahsil et yeşil → Ödendi yeşil).
- Kurallar: ekranda tek dolgulu ana düğme; kırmızı yalnız küçük durum etiketinde; renk tek başına anlam taşımaz; her renk çifti uygulamadan önce kontrast ölçümünden geçer.
- Renk psikolojisi makalesi (Görkem Yıldız) karşılaştırıldı; tek ciddi fark marka vurgusunun rengi.

### 18 — Son renk sistemiyle ekranlar (20.09.2026)
Özet ve Aidat listesi (yönetici), gündüz ve gece, eski/yeni yan yana hazırlandı. Düğmelerin iki temada aynı kaldığı gösterildi.

### 19 — Canlı yönetici panosu bakıldı (19–20.09.2026)
`https://apartora.com/manager/dashboard` (Dneme1234, Site Yöneticisi) açık oturumda incelendi. Snapshot: `live-dashboard-snapshot.yml`. Gözlemler: ≤3 uyarı/bant üst üste; Dikkat’te boş daire; KPI ile Mali Durum sırası; Tahsilat Durumu ile Aidat Özeti rakam çelişkisi riski; sidebar’da sakin rotalarının karışması.

### 20 — Renk katmanları + menü dizilim şeması dosyalandı (20.09.2026)
Ayrı şema eksikti; `4.RENK_KATMANLARI_VE_MENU_DIZILIM_SEMASI_2026-09-20.md` yazıldı (L0–L5, canlı sidebar ağacı, mobil 5 sekme). Kod düzenlemesi yok.

### 21 — Klasördeki kural dosyaları okundu; olması gerekenler raporu (20.09.2026)
`CLAUDE.md`, `coding-patterns`, `ui-components`, `api/testing/deployment/validation-rules` okundu. Ürün kodu düzenlenmedi. `5.OLMASI_GEREKENLER_RAPORU_2026-09-20.md` + `0.INDEX_TAKIP.md` + `6.DILIM_PLANI_VE_OTURUM_OZETI_2026-09-20.md` kaydedildi. Sıra: Dilim 0 → 1 (kaybolmama) → 2 (koyu token paketi) → 3 (dil/tahsilat/tur). Canlı SaaS, staged yayın.

### 22 — Gelişim kaydı resmi takip aracı seçildi (20.09.2026)
`APARTORA_GELISIM_KAYDI.md` kalıcı değişiklik günlüğü olarak kullanılacak. Her kalıcı adımda bu dosya güncellenir (yeni numara, sona ekleme). Numaralı rapor md’leri klasörde kalır; indeks `0.INDEX_TAKIP.md` bu kayda da işaret eder.

### 23 — Ortak özel GitHub deposu kararı (20.09.2026)
3–4 farklı kaynağın aynı yeri görmesi için takip dosyaları **Apartora ürün reposundan bağımsız**, **private** bir GitHub deposunda tutulacak. Aday ad: `apartora-uiux-takip`. Yerel klasör: `C:\Users\Kemal\Desktop\APARTORA`. Mevcut fine-grained PAT ile repo **oluşturma** 403 verdi; depo sahibi GitHub’da private repo’yu elle oluşturup collaborator ekleyecek, ardından içerik push edilecek. Ürün kodu bu depoya konmaz.

### 24 — Private depo açıldı ve bağlandı (20.09.2026)
Tarayıcı oturumuyla `https://github.com/kemaltoruun/apartora-uiux-takip` **Private** oluşturuldu; yerel `main` push edildi. Tek doğru günlük: `APARTORA_GELISIM_KAYDI.md`. Diğer kaynaklar: Settings → Collaborators ile davet → kabul → clone/pull. Ürün reposundan bağımsız.

### 25 — Çoklu kaynak: GPT + Claude (+ Cursor) (20.09.2026)
Takip deposunu görecek AI kaynakları: **GPT**, **Claude**, **Cursor** (ve insan sahibi). Bunlar GitHub’da “davet edilecek kullanıcı adı” değil; her biri aynı private repoyu okur/yazar.

**Ortak URL:** https://github.com/kemaltoruun/apartora-uiux-takip  
**Tek doğru dosya:** `APARTORA_GELISIM_KAYDI.md`

| Kaynak | Nasıl bağlanır |
|---|---|
| Cursor (bu makine) | Klasör zaten `origin`’e bağlı; kalıcı değişiklik → kayda ekle → commit → `git push` |
| Claude (Claude Code) | Bu private repoyu clone et veya aynı klasörü aç; işe başlamadan `git pull`; bitince kayda yaz → push. GitHub girişi / PAT o ortamda tanımlı olmalı |
| GPT (ChatGPT / Codex vb.) | Aynı: repo’yu GitHub’da bağlı hesapla aç veya clone; private olduğu için o hesabın collaborator veya sahip olması gerekir. Sohbete dosya yapıştırmak yerine her zaman bu repodan oku |
| İnsan | Collaborators’a ek GitHub kullanıcıları davet edilebilir (yazma yetkisi) |

**Kural:** Her kaynak işe başlamadan günlüğü `git pull` ile alır; kalıcı adımda yeni kayıt numarası ekler; çakışmayı önlemek için kısa kayıt + hemen push.

### 26 — Erişim modeli netleşti: insanlar GPT/Claude kullanır (20.09.2026)
Private repoya davet edilecek olan **GPT veya Claude değil, onları kullanan insanlar**. Her kişi kendi GitHub hesabıyla Collaborators’a eklenir (Write). Sonra o kişi kendi makinesinde Claude Code / GPT / Cursor ile bu repoyu açar; AI aynı `APARTORA_GELISIM_KAYDI.md` dosyasını görür.

**Bekleyen:** Davet listesi (GitHub kullanıcı adı veya e-posta) — sahip verecek; verilince davetler gönderilecek.

### 27 — Takip deposu Public yapıldı (20.09.2026)
GPT/Claude’un GitHub hesabı olmadığı için davet yolu çalışmaz. Karar: `apartora-uiux-takip` **Public** (içinde ürün kaynağı / .env / anahtar yok). Apartora ürün kodu ayrı private kalır. Public öncesi `CLAUDE.md` içinden yedek parola yolları, admin e-posta ve Supabase project_id çıkarıldı. AI’ler repoyu davetsiz okuyabilir; kalıcı yazım için hâlâ GitHub hesabı + push gerekir.

### 28 — Tek iletişim dosyası; konu yöneticisi Cursor (20.09.2026)
İletişim **yalnız** `ILETISIM.md` üzerinden. Herkes oraya yazar, herkes oradan okur. Konu yöneticisi: **Cursor** (sıra, özet kutusu, kalıcı kararları `APARTORA_GELISIM_KAYDI.md`’ye aktarma). Tartışma ILETISIM’de; numaralı gelişim kaydı GELISIM’de. Mesaj formatı: `### M### — tarih — Kim` (Cursor | Claude | GPT | İnsan:Ad).

### 29 — Claude kanala girdi; indeks Private hatası kapatıldı (20.09.2026)
M002 (Claude): okudu, push token yok, indeks “Private” çelişkisi bildirdi, bekleyen liste sorusu. M003 (Cursor): liste **3,5,7,9,10 hâlâ güncel**; indeks Public’e düzeltildi; okuma public (tokensiz), yazma insan/PAT. Dilim 0 devam.

### 30 — Yönetim sözleşmesi + oybirliği / kademeli disiplin (20.09.2026)
`YONETIM.md` yürürlükte. Amaç: kaybolmama, ferah/kolay UI/UX, menü+renk; mikromühendislik; varsayımsız kanıt; adil turlar; **herkes hemfikir** olmadan dilim ilerlemez; proje sahip bitirene kadar sürer; yazılım ekibinden veri talebi kayıtlı. M004 + **Gündem-A Tur 1** (seçenek A/B/C) açıldı. Konu yöneticisi Cursor talimat verir, görüş toplar.

### 31 — Sohbet sürekliliği / oto takip (20.09.2026)
Kaynak gerçeklik sohbet değil: GitHub’daki ILETISIM + GELISIM. `OTO_TAKIP.md` + `.cursor/rules/iletisim-oto-takip.mdc` (alwaysApply): her turda `git pull` + kanal okuma. YONETIM §10. İsteğe bağlı `/loop` veya Cursor Automation. M005.

### 32 — Ön test + katılımcı sohbet açılışı (20.09.2026)
Kanal ön testleri geçti (yerel dosyalar, GitHub API, public raw YONETIM, M001–M005 zinciri). M006: Claude / GPT / İnsan:Kemal davet + yapıştır metinleri; Gündem-A Tur 1 oyları bekleniyor. Kod yok.

### 33 — Son kontrol + iletişim testi (20.09.2026)
M011: git/local/GitHub/raw/kural OK; GPT+Claude oyları kanalda; Claude masaüstü açık ama proje klasörü doğrulanmadı; uzaktan oto uyandırma yok; İnsan:Kemal oyu eksik. Altyapı hazır, Gündem-A kapanışı değil.

### 34 — Claude Code köprü çalışıyor (20.09.2026)
Claude Code kuruldu + `auth login` OK. Cursor `claude -p` ile ILETISIM okuttu; Claude `CLAUDE_KOPRU.md`’ye M012 yazdı. İki yönlü haberleşme (Cursor↔Claude Code) doğrulandı. M013. Gündem-A için İnsan:Kemal oyu hâlâ eksik.

### 35 — Tanıtım / amaç iletişim turu (20.09.2026)
M014 açıldı. Claude M015 ile tanıdı + amaç (YONETIM §1) teyit; köprü Claude Code. GPT OpenAI API 429. İnsan:Kemal tanıtımı bekleniyor. Gündem-A oyları duruyor.

### 36 — Codex CLI kuruldu (20.09.2026)
GPT köprüsü için Codex CLI 0.155.1 (winget + npm). API key ile login denendi; platform kredisi yok. ChatGPT abonelik login bekleniyor (`codex login`). M018.

### 37 — GPT Codex köprü çalışıyor (20.09.2026)
ChatGPT login OK. `codex exec` ile M016 tanıtım/amaç alındı (`GPT_KOPRU.md` + ILETISIM). Cursor↔Claude ve Cursor↔GPT haberleşme kuruldu. İnsan:Kemal tanıtımı/Gündem-A oyu bekleniyor. M019.

### 38 — DeepSeek Deep Code CLI kuruldu (20.09.2026)
`@vegamo/deepcode-cli` 0.4.1 + `~/.deepcode/settings.json` + vault şablonu `deepseek-api.env`. API key henüz yok. `DEEPSEEK_KOPRU.md`. M020.

### 39 — DeepSeek köprü çalışıyor (20.09.2026)
API key kasaya alındı; `deepcode -x -p` ile M021 tanıtım/amaç. Cursor↔DeepSeek OK. Sohbete yapıştırılan key için rotate tavsiyesi. M022.

### 40 — Dörtlü SELAM turu tamam (20.09.2026)
M024–M028: Cursor · Claude · GPT · DeepSeek selam + HAZIR=evet. Üç köprü canlı doğrulandı. Gündem-A için İnsan:Kemal oyu bekleniyor.

### 41 — Gündem-A KAPANDI: oybirliği C + Claude koşulları (20.09.2026)
İnsan:Kemal M029 GÖRÜŞ:C (koşullar kabul). Oybirliği: Cursor+GPT+Claude+Kemal = C. Referans = YAPI (menü ağacı, L0–L5, dosya 5); hex dışı; kod talebi değil; Dilim 1 ayrı onay. Ekip kapanış 3/5/7/9/10 talep son tarihi **2026-09-27**; gelmeyen bilinçli ertelendi. Rozet+3 commit netleşmeden Dilim 0 kapanışı sayılmaz. M030. Kod yok.

### 42 — Gündem-B: zorunlu döküman okuma tamam (20.09.2026)
M031–M035. Claude/GPT/DeepSeek YONETIM + M030 + kayıt 41 + OTO + dosya4/5 okudu; ANLADIM + HAZIR=evet. Dilim ilerlemeden önce okuma şartı sağlandı. Kod yok.

### 43 — Gündem-C KAPANDI: ekip talep + YAPI özeti (20.09.2026)
AI ekip-talep-OK + YAPI-ÖZET OK; sahip DEVAM = onay. Ekip 3/5/7/9/10 son 2026-09-27. YAPI kilitli. M041. Kod yok.

### 44 — Gündem-D: Dilim 1 değerlendirme başladı (20.09.2026)
`7.DEGELENDIRME_DILIM1_BASLANGIC_2026-09-20.md`: D1.1–D1.4 öncelik (site/dönem → avatar → mali blok → bant≤2). Kod yok; ürün src bu klasörde yok. M042. Canvas güncellenir.

### 45 — Kapsam: kod yok — yapı/tasarım/görsel yerleşim (20.09.2026)
Sahip: koda gerek yok; kontroller yapı + tasarım + görsel yerleşim. `8.GORSEL_YAPI_YERLESIM_KONTROL_2026-09-20.md` (V1–V6). Kanıt PWA/web + canvas. M049.

### 46 — Plan: görsel/yapı/yerleşim fazları (20.09.2026)
`9.PLAN_GORSEL_YAPI_YERLESIM_2026-09-20.md`: Faz 0✅ → 1 (V1/V2 Aidat+Menü) → 2 (V5 bant) → 3 (V3/V4) → 4 (V6 menü) → 5 (sakin) → 6 (sentez). Kod yok; plana uyarak ilerlenir. M050.

### 47 — Faz 1 kapandı: V1/V2 Aidat + Site Yönetimi (20.09.2026)
Canlı: Aidat V1 EKSİK + site adı çelişkisi (IŞIK SİTESİ ≠ Dneme1234); V2 RISK. Site Yönetimi V1 EKSİK. Özet V1 OK. Sıradaki Faz 2 (V5). M051 · dosya 8/9.

### 48 — Şimdi vs tavsiye: sayfa yapısı + sebep/sonuç (20.09.2026)
`10.SIMDI_VS_TAVSIYE_SAYFA_YAPISI_2026-09-20.md`: Özet/Aidat/Site Yönetimi iskelet karşılaştırması; sebep→sonuç→kullanıcı faydası; bağlam şeridi tavsiyesi. Canvas `simdi-vs-tavsiye-yapi`. Kod yok. M052.

### 49 — Gündem-E açıldı: paket değerlendirme (20.09.2026)
Dosya 10–13 + Primer olgunluk disiplini katılımcılara. Oylar: YAPI / UK / DOKÜM / PRIMER / SIRADA. M053. Kod yok.

### 50 — Çapraz sorgu + tek-tek envanter (20.09.2026)
YONETIM §3.1 çapraz sorgu zorunlu. `14.TEK_TEK_KONU_DEGERLENDIRME`: Blok A–G tüm sahip paylaşımları (Claude bilgilendirme alt maddeleri, Primer, UK üreticiler, Faz1 mikro). Gündem-E Tur 2 açık; Claude M054 OK/Faz2; GPT M055 düzelt*/sahip-onay; KAPANDI değil. M057.

### 51 — Ciddiyet standardı yürürlükte (20.09.2026)
Sahip: çalışmayı çok ciddiye alın. `15.CIDDIYET_STANDARDI` + YONETIM §3.2. Farksız before/after yasak; 3 sn test; NET Aidat kanıtı `aidat-NET-simdi-vs-tavsiye.png`. M066.

### 52 — Faz 2 V5 bant ölçümü + NET Özet (20.09.2026)
Sahip: “ilk önce UI/UX çalışması”. Canlı Dneme1234: Özet **RISK** (deneme bandı + Deneme butonu + Dikkat); Aidat/Site **OK*** (sistem bandı yok). Dosya `16` + `kanit-gorsel/ozet-V5-NET-simdi-vs-tavsiye.png`. Kod yok. M067.

### 53 — Sahip oturumu: Özet 6 yüzey (20.09.2026)
Sahip Chrome giriş. Evaluate: deneme×2 + PWA bildirim + PWA kur + Dikkat + favori = **6**. V5 RISK güçlendi. `ozet-SAHIP-OTURUM-bant-sayim.md`. M068. Kod yok.

---

## Ortak depo
| Alan | Değer |
|---|---|
| URL | https://github.com/kemaltoruun/apartora-uiux-takip |
| Görünürlük | Public |
| Yönetim | `YONETIM.md` |
| Süreklilik | `OTO_TAKIP.md` |
| İletişim | `ILETISIM.md` |
| Gelişim günlüğü | `APARTORA_GELISIM_KAYDI.md` |

## Bekleyen raporlar
| Kayıt | Konu |
|---|---|
| 3 | Adım 1 kapanış raporu |
| 5 | Adım 2 Toplu Tahsilat karşılaştırması |
| 7 | Adım 4 tanıtım turu kök nedeni |
| 9 | UI/UX ilk paket (K6, Ö4, A1) |
| 10 | Gezinme paketi (gönderim teyidi dahil) |

## Karar bekleyen konular (sistem sahibi)
1. İki Toplu Tahsilat: tek akış mı, ayrı mı? (öneri: Tahsilat Merkezi + FIFO)
2. Rozet commit'lerinin push'u ve başka oturumlara ait 3 commit
3. Gece statü hatası (UTC günü) için ayrı düzeltme
4. Marka vurgusu: lacivert mi mürdüm mü? (tavsiye: lacivert, güven için)
5. Renk paleti yönü: kayıt 17 premium fildişi/mürekkep mi, yoksa denetim L0–L3 açma paketi (#12151C…) mi — ikisi hizalanmalı
6. Bildirim Kanalları kartının yeri
7. Tanıtım turu kalsın mı, kontrol listesine mi dönsün? (Adım 4 sonucuna bağlı)
8. Sakin portalı için ayrı envanter çıkarılması
9. Dilim 1’e geçiş onayı (kaybolmama iskeleti; kod henüz yok)
10. Sidebar’da yönetici menüsüne karışan sakin yolları (feature_modules / ürün)

## Tavsiye listesi
1. Terim sözlüğü (her kavramın tek adı; web ve mobil ortak)
2. Aidat durumu → renk rolü eşlemesi
3. Bağlam satırı (site adı) bilgi verir, değiştirme yapmaz
4. Sistem yazı boyutu desteği ve testi
5. Azaltılmış hareket desteği
6. Dekoratif ikonların ekran okuyucudan gizlenmesi
7. En dar test genişliği 320
8. Tipografi rol ölçeği (mobilde piksel, 12'nin altı yok)
9. Tek token dosyası (web + mobil)
10. Ekran şablonları (sekme kökü, liste, detay, form)
11. Pasif düğme yerine açıklama (mobilde pasifse yanında neden yazılı)
12. Para işlemlerinde onay + geri alma
13. Düğme metni fiil + nesne + tutar ("3 aidatı tahsil et")
14. Aynı anda en fazla 2 uyarı/duyuru
15. Para işlemi sonuçları bağlam içinde, toast'ta değil
16. Kademeli yükleme, toplu işlemde ilerleme göstergesi
17. Her boş durumda tek birincil eylem
18. Aidat Trendi erişilebilirliği
19. Her işe tek ana yol
20. Sakin ödemesinde IBAN ve hazır açıklama kopyalama
21. Toplu düzenlemede etki önizlemesi + geri alma
22. Silme onayında nesne adı ve sayı
23. Sayaç ve toplu işlem kapsamı tek süzme kuralından
24. Detay ekranlarının paylaşılabilir bağlantısı
25. Mobilde alt sayfa kapanınca ekran okuyucu odağının geri dönmesi
26. Düğme renk standardı (eyleme göre, temadan bağımsız)
27. Premium palet (fildişi / mürekkep)
28. Kırmızının yalnız durum etiketinde kullanılması

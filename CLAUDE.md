# APARTORA MANAGER - PROJE TALİMATLARI

## TEMEL KURALLAR
- Türkçe yanıt ver, commit mesajları Türkçe
- **İki OS**: Geliştirme hem **macOS** hem **Windows**'ta yapılıyor — platformu çalışma anında doğrula, tek-OS'a özel komut/config yazma
  - Windows'ta terminal komutlarında `&&` yerine ayrı komutlar; macOS'ta `&&` sorunsuz, `timeout` yok (`gtimeout`)
  - OS'a bağlı config git'e tek form olarak konmaz → `.example` + kurulum `.md` (örnek: `.mcp.json` / `docs/MCP_KURULUM.md`)
- DRY prensibi uygula, kod tekrarından kaçın:
  - Yeni hook/component/API yazarken → benzer mevcut kodu ara, varsa genişlet veya ortak util çıkar
  - Aynı fetch/CRUD pattern'i 2+ yerde → generic helper/factory oluştur
  - Aynı dialog/form yapısı → mevcut shared component kullan veya oluştur
  - Aynı error handling bloğu 2+ yerde → utility'ye taşı
- **ÜÇ HEDEF kuralı (web + mobil Android + mobil iOS)**: Kod yazmadan ÖNCE değişikliği üç hedefe göre kapsamla — "şimdilik sadece web" ikinci kopyayı doğuran iştir. Hedeflerden birini bilinçli atladıysan gerekçesiyle raporla, sessizce bırakma
  - **Sözleşme / tip / Zod şeması / enum / sabit / saf hesap → `packages/shared` (`@apartora/shared`)** — varsayılan ev; alias her iki tsconfig'de hazır. Yeni yardımcı yazmadan ÖNCE `packages/shared/src/` içinde ara. Zod tek tanım, TS tipi `z.infer`'den türer (**giriş = Zod, çıkış = interface**)
  - **Sunum katmanı paylaşılMAZ** (web RSC+Tailwind vs mobil RN+NativeWind) — paylaşılan şey kontrat ve karardır, JSX değil
  - **Native ayrımı `Platform.OS` ile DEĞİL, dosya ikiziyle**: `x.android.ts` + taban `x.ts`. `import` paketleme zamanında çözülür → `Platform.OS` koruması iOS'ta **açılış çökmesini ENGELLEMEZ**, Android'de hiçbir belirti vermez. Mandal: `npm run test:invariants` → `platform-native-isolation`
  - **Native yapılandırma** `app.json` + `apps/mobile/plugins/*.js` config plugin'lerinden geçer — `apps/mobile/ios` ve `apps/mobile/android` **gitignore'da** (prebuild üretir), oraya elle yazılan silinir
- Responsive tasarım, Shadcn/ui + Tailwind CSS kullan
- **İş akışı (önemli ayrım):**
  - **Küçük/net iş** (1-2 dosya, i18n ekleme, küçük UI fix, tek fonksiyon) → tamamlanana kadar durma, direkt kodla
  - **Büyük/belirsiz iş** (3+ dosya, mimari karar, bug fix, DB değişikliği, yeni özellik) → ÖNCE 2-3 yaklaşım öner, onay al, SONRA kodla (detay: KOD BOZULMASINI ENGELLEME bölümü)
- Mevcut kod yapısını bozmadan değişiklik yap
- Yorum satırları ekle, anlamlı isimlendirme kullan
- **`console.log` KULLANMA → `logger` kullan** (`@/lib/logger`)
- **Hardcoded Türkçe metin KULLANMA → `t()` kullan** (i18n)
- **`any` tipi KULLANMA** — TypeScript strict typing uygula, gerekçesiz `any` yasak
- **Yeni yazmadan önce mevcut olanı ara** — Utility, component, hook, endpoint zaten varsa onu kullan, sıfırdan yazma

## KOD BOZULMASINI ENGELLEME (ZORUNLU)

> **"Önce oku, sonra planla, en son kodla. Asla okumadan düzenleme yapma."**

- **IMPORTANT: Değiştireceğin dosyayı ÖNCE oku** - Mevcut kodu anlamadan asla düzenleme yapma
- Küçük diff'ler yap - Tek seferde birden fazla bağımsız değişiklik yapma
- İstenmeyen refactoring YAPMA - Sadece istenen değişikliği yap
- Mevcut import'ları koru (açıkça istenmedikçe)
- Mevcut pattern'leri takip et
- Kontrol: 1) Dosyaları oku 2) Import/bağımlılık kontrol 3) Pattern incele 4) 3+ dosya → Plan Mode 5) Lint çalıştır
- YAPMA: Okumadığın dosyada değişiklik, istenmedikçe refactor, ilgisiz kod değişikliği, fonksiyon imzası/export/type daraltma, aynı hatayı 2+ kez deneme
- **Önce yaklaşım öner kuralı**: TEMEL KURALLAR > İş akışı bölümünde tanımlı (3+ dosya / mimari / bug fix / DB → 2-3 yaklaşım öner, onay al, SONRA kodla)
- **Yeni özellikte ürün önerisi sun**: Yeni özellik geliştirirken gerçek kullanım senaryolarını düşünerek önerilerde bulun (onay al, direkt kodlama):
  - **Ek özellikler**: "Bu modüle şu da eklenebilir" (ör: aidat → gecikme faizi, hatırlatma maili)
  - **Gerçek kullanım**: "Kullanıcı bunu şu şekilde kullanır, şu durum sorun olabilir" (ör: 500 kayıt gelirse, yarıda bırakırsa, yanlışlıkla silerse)
  - **Olması gerekenler**: "Bu özellik varsa şu da olmalı" (ör: duyuru varsa → okundu bilgisi, bildirim varsa → sessiz saat ayarı)
  - **Mevcut sistemle çakışma + doğru yaklaşım**: "Bu değişiklik şu yapıyı etkiler, mühendislik açısından doğru yaklaşım şudur" (ör: yeni rol → RLS policy etkilenir, toplu bildirim → email provider limiti, ödeme değişikliği → webhook'lar — sadece uyarma, çözüm de öner)
- **Teknik kontrolleri bildir**: Yaklaşım önerirken hangi teknik gereksinimleri düşündüğünü kısaca belirt (loading/empty/error state, i18n, dark mode, mobil, validation, rate limit, audit log vb.) — ayrı onay isteme, bilgi ver ve devam et
- **Basit/temiz çözümü tercih et**: Over-engineering YAPMA, node_modules patch'leme YAPMA, postinstall script YAPMA
- **İş bitince kontrol et**: Değiştirdiğin dosyalarda eksik kalan var mı? (i18n, error logging, mobil uyumluluk, lint) — tamamla, yarım bırakma
- **Dokunduğun dosyayı iyileştir**: Bir dosyada değişiklik yaparken, o dosyadaki CLAUDE.md + rules kurallarına uymayan tüm ihlalleri düzelt — sadece dokunduğun dosyada, ilgisiz dosyalara DOKUNMA
- **Refactor fırsatı gördüğünde öner**: Bir dosyada çalışırken belirgin code smell, tekrar eden kod, karmaşık logic veya refactor fırsatı fark edersen → işini bitirdikten sonra kısaca öner (ne, neden, nasıl). Direkt uygulamadan önce onay al. Projede benzer pattern'leri de tara — aynı sorun başka dosyalarda da varsa DRY refactor öner (ör: "Bu pattern 5 dosyada tekrarlanıyor, ortak hook/helper çıkarılabilir: [dosya listesi]")

## TEKNOLOJİ
> Versiyonlar `package.json`'dan okunur — bu listede yazılmaz, eskimemesi için.
- **Frontend**: Next.js, React, Shadcn/ui, Tailwind CSS, TanStack Query, React Hook Form + Zod
- **Backend**: Supabase (PostgreSQL, Auth, Realtime, Storage), Next.js API Routes
- **Supabase MCP project_id**: `nlxrubesberdycjeaaxq`
- **Next.js 16 özellikleri**: `use()` hook, async `cookies()`/`headers()`/`params`/`searchParams`, Turbopack, fetch cache `no-store`

## PROJE YAPISI & ROLLER
- **src/**: app/ (api, superadmin, manager, resident, security) | components/ (ui, dashboard, common) | lib/ | hooks/ | types/
- **Roller**: SUPERADMIN → SEMIADMIN → MANAGER → SEMIMANAGER → SECURITY → RESIDENT

## KOMUTLAR
```bash
npm run dev              # Development (3000)
npm run dev:clean        # Cache temizle + dev (ÖNERİLEN)
npm run build            # Production build
npm run lint             # ESLint
npm run db:sync          # DB tip güncelle + typecheck (DB DEĞİŞİKLİĞİNDE ZORUNLU)
npm run typecheck:fast   # Hızlı tam typecheck (TypeScript 7 native, ~26sn vs ~148sn) — günlük kullanım
npm run typecheck        # typecheck:fast ile AYNI (TS7 + kilit). Eski TS5 yolu: typecheck:full
npm run typecheck:check  # Web TS hata mandalı — baseline'a göre YENİ hata var mı? (push öncesi)
npm run typecheck:baseline # Web TS baseline'ını güncelle (hata azaltınca/bilinçli kabul edince)
npm run typecheck:mobile:check    # Mobil (apps/mobile) TS mandalı — baseline=0, yeni hata fail (mobil dosya dokununca push öncesi)
npm run typecheck:mobile:baseline # Mobil baseline'ını güncelle (tsbaseline.mobile.json — hata eritince küçült)
npm run typecheck:shared:check    # Ortak paket PLATFORM mandalı — DOM'suz + Node'suz derler, baseline=0; pre-push her push'ta koşar
npm run typecheck:shared:baseline # Ortak paket baseline'ını güncelle (tsbaseline.shared.json — hedef 0 kalsın)
npm run audit:bundle     # Bundle boyut analizi + budget kontrolü (build sonrası)
npm run audit:ux-envanter # UX envanteri: rol × menü × rota × mobil karşılık → referans/UX_ENVANTER.md (kırık menü, yetim sayfa)
npm run migrate:rollback # Migration rollback SQL çıkar
```

## VERİTABANI DEĞİŞİKLİK KURALI (ZORUNLU)

> **"DB'ye dokunduğunda, tiplere de dokun."**

- Workflow: 1) Supabase'de değişiklik 2) `npm run db:sync` 3) Typecheck hatalarını düzelt 4) Kod yaz
- `db:sync` ZORUNLU: Yeni tablo, kolon ekleme/silme/değiştirme/yeniden adlandırma
- `db:sync` GEREKSİZ: RLS policy değişikliği, sadece veri ekleme/güncelleme
- Pre-commit hook tam typecheck ÇALIŞTIRMAZ (lint-staged no-op; tsc ~12dk+OOM riski). Push öncesi `npm run typecheck:check` (mandal) ile YENİ TS hatası kontrol et — mevcut borç dondurulmuş, sadece yeni hata fail eder
- **TÜM typecheck yolları TypeScript 7 (native) ile koşar** (2026-09-02; `typecheck`, `typecheck:fast`, `typecheck:check`, mobil eşlenikleri — TS5 yalnız `typecheck:full` kaçış kapısında kaldı). Ölçüldü → TS5 **~148sn + 16GB heap bayrağı zorunlu** (bayraksız OOM/exit 134), TS7 **web ~26sn / tepe ~8,4GB, mobil ~6sn / ~2,1GB, heap bayrağı gerekmiyor**, ikisi de **0 hata — çıktı birebir aynı** (iki tarafta da kasıtlı hata sınamasıyla doğrulandı). Ortak TS7 mantığı `scripts/lib/ts7.mjs`'de. `typescript` bağımlılığını 7'ye ÇEVİRME: TS7 paketinde `tsserver` YOK (editör IntelliSense ölür) ve derleyici API'si yok (`require('typescript')` sadece `version`+`versionMajorMinor` döner; `createProgram` undefined) → `typescript-eslint` çöker, `npm run lint` ölür. Sabitlenmiş `npx` ile izole çalışır; **bin adı `tsc` olduğu için repoya kurulursa `node_modules/.bin/tsc`'yi EZER**. TS7 `baseUrl`'ü kaldırdı; ana `tsconfig.json`'a DOKUNULMADI — `scripts/lib/ts7.mjs` çalışma anında ondan türetilmiş geçici config üretir (tek doğruluk kaynağı korunur, sapma olmaz)
- **Mobil TS mandalı ayrı**: `apps/mobile` kendi tsconfig'i + `tsbaseline.mobile.json` (**baseline=0, mobil TS temiz — 2026-07-18**). Mobil dosya değiştirdiysen push öncesi `npm run typecheck:mobile:check` (yeni hata fail). Yeni borç girerse `typecheck:mobile:baseline` ile güncelle ama hedef 0 kalsın. Web ratchet (`tsbaseline.json`) mobil'i KAPSAMAZ (ayrı tsconfig). NOT: eski "113 Supabase `.eq()` filtre-inference borcu" ASLINDA `LargeSecureStore`→`SupportedStorage` tip çakışmasının cascade'iydi (`supabase` client tipi bozulunca ~60 dosyada `.eq()` çıkarımı çöküyordu); `secure-store.ts` düzeltilince hepsi çözüldü — Supabase `.eq()` toolchain borcu diye bir şey YOK
- **Raw SQL ile auth/user kaydı oluşturma YAPMA** → Mevcut API endpoint veya service function kullan
- **Migration'a DOWN bölümü ekle**: Her yeni migration'a `/*-- DOWN START ... -- DOWN END*/` bloğu ekle (şablon: `supabase/migration-template.sql`)
- Rollback helper: `npm run migrate:rollback <dosya-adı>` ile DOWN SQL'ini çıkar
- **Yeni tabloda GRANT ZORUNLU** (Supabase Data API değişikliği — 30 Ekim 2026 sonrası): Her yeni `public` tablo için `GRANT SELECT, INSERT, UPDATE, DELETE ON public.X TO authenticated; GRANT ALL ON public.X TO service_role;` (gerekirse `anon SELECT`) eklenmeli. Yoksa supabase-js çağrısı `42501` döner. Şablonda hazır blok var

## API KURALLARI
- **Doğrudan Supabase client KULLANMA → API Route kullan** (mutation işlemleri için)
- **Raw `fetch('/api/...')` KULLANMA → `apiFetch` kullan** (`@/lib/utils/api-fetch`)
- **Error Response**: `forbidden()`, `notFound()`, `apiError()` kullan — manuel `NextResponse.json` YAPMA
- **Auth**: `authenticateAdmin(request, ['role1', 'role2'])` | **Handlers**: `managerHandler`, `superAdminHandler`
- **Validation**: `withValidation()` + `apiSchemas` — kuralları varsayma, uygulamadan önce gereksinimleri onayla (hangi alanlar zorunlu, min uzunluklar, özel kurallar)

## GIT
Prefix: `özellik:` (yeni) | `düzeltme:` (bug fix) | `güncelleme:` (update) | `güvenlik:` (güvenlik düzeltmesi) | `belge:` (docs) | `bozucu:` (MAJOR)
- **Listede olmayan önek SESSİZCE düşer**: `version:bump` onu CHANGELOG'a almaz *ve* bump hesabına katmaz — yalnız o önekten commit varsa sürüm hiç yükselmez. Tek doğruluk kaynağı `scripts/version-bump.mjs → PREFIX_MAP`; yeni önek eklerken oraya da ekle. Script artık tanınmayan öneki uyarı olarak basar (2026-09-02'de `güvenlik:` bu yüzden v3.131.0 notlarından düşmüştü)

## SÜRÜMLEME (ZORUNLU — kullanıcı ayrıca istemez)
> Sürümleme işi bitirmenin standart parçasıdır; "sürümle çalışalım" demeye gerek yok, otomatik uygula.
- **Yayınlanabilir iş tamamlanınca bump** (SemVer + CHANGELOG otomatik): web → `npm run version:bump` | mobil → `npm run version:bump:mobile`. Prefix'ler bump'ı belirler (`özellik:`→MINOR, `düzeltme:`/`güncelleme:`→PATCH, `bozucu:`→MAJOR). Detay: `memory/reference_versioning_policy.md`
- **Ne zaman bump ETME**: tek tek küçük commit'lerde değil — bir özellik/mantıksal grup **tamamlandığında** tek bump. Yarım işte bump yapma.
- **Mobil OTA nüansı (kritik)**: `runtimeVersion=appVersion` → **JS/UI-only değişikliği bump ETME**, sadece `npm run ota:publish` (bump runtimeVersion'ı değiştirir, mevcut kullanıcılar OTA'yı alamaz/orphan). **Native değişiklik** (kütüphane/ikon/izin/native sürüm) → `version:bump:mobile` + yeni APK.
- **versionCode elle tutma**: `expo.version`'dan otomatik türetilir (`plugins/with-version-code.js`, prebuild'de). Elle versionCode set etme.

## PARALEL OTURUM, DOĞRULAMA VE YAYIN DİSİPLİNİ
> Kaynak: 13.08–13.09.2026 kullanım raporu (212 oturum). Mesajların **%46'sı** zamanca çakışan oturumlarda yazıldı; en az dört oturum başkasının işi yüzünden engellendi ya da işi mükerrer yaptı; ayrıntılı incelenen 88 oturumun **60'ı** kod bittiği hâlde push/deploy/OTA beklediği için yarım kaldı. Maddeler hafızadaki olay kayıtlarının özetidir — ayrıntı bağlantıda.

**Paralel oturumlar** — bu repoda aynı anda başka Claude oturumları çalışır; indeks ve çalışma ağacı PAYLAŞILIR
- İşe başlarken `git status` + `git log --oneline -10`: istenen iş zaten commit'lenmiş mi, ağaçta kimin yarım işi duruyor?
- Başkasının değişikliğini `git stash`'leme, commit'leme, `checkout --` ile geri alma (`memory/reference_git_stash_parallel_session_trap.md`)
- Kendi dosyanı sahneleyince bekletmeden commit'le; sonra `--stat`'a değil İÇERİĞE bak: `git show HEAD:<dosya> | grep -c '<yeni-sembol>'` (`memory/reference_staged_changes_swept_by_parallel_session.md`)
- Pre-push kapısı commit'i değil ÇALIŞMA AĞACINI tarar: kırmızının senin commit'inde olup olmadığını yalıtılmış worktree'de kanıtla (`memory/feedback_isolated_worktree_proves_gate_ownership.md`). `--no-verify` gerekirse doğruladığın SHA'yı pinle: `git push origin <sha>:refs/heads/main` (`memory/feedback_push_explicit_sha_with_parallel_session.md`)
- **Worktree'yi ELLE kurma → `node scripts/izole-worktree.mjs ac <dizin>`** (sonra `push <dizin>`, `kapat <dizin>`). Elle `git worktree add` ile kurulan dizinde husky kancası YOKTUR (`core.hooksPath` göreli, `.husky/_` git'e girmez) ve git push'u kapısız, tek satır çıktıyla gönderir — üç kez yaşandı (09-11, 09-13, 09-15; sonuncusu mimari ratchet'ini geriletti). Betik kancayı, upstream'i, `node_modules` bağlantılarını ve üretilmiş artefaktları kurar; `push` kanca yoksa DURUR, SHA'ya pinli iter ve uzaktaki SHA'yı doğrular

**Başarıyı etkisinden doğrula**
- Push → `git fetch` sonrası `git rev-parse origin/main` pinlediğin SHA'ya eşit mi. Çıkış kodu tek başına kanıt değil
- Deploy → Coolify paneline değil `https://apartora.com/api/version` yanıtındaki `gitSha`'ya bak (panel iki yönde de yanılıyor)

**Yayın sırası (web + mobil)**
- Push canlıya ÇIKARMAZ; Coolify deploy'u ayrıca tetiklenir
- Mobil JS yeni ya da değişen bir web API'sine bağlıysa sıra: push → web deploy → `/api/version` doğrulaması → OTA. Web inmeden çıkan OTA, sunucuda henüz olmayan uca istek atar
- "OTA yayınla" = ÜÇ kanal, `apps/mobile` içinden: `ota:publish` (production) · `ota:publish:preview` · `ota:publish:play` (gerçek mağaza kullanıcıları). Önce her kanalın manifest'ini ölç, yalnız bayat olanı yayınla (`memory/feedback_ota_trigger_both_channels.md`)
- `ota-publish.mjs` kirli ağaç, env ve native kayma kapılarını kendisi uygular. Başkasının yarım işi için `--kirli-agac-kabul` ile atlatma — ağaç kirliyse yayını izole worktree'den yap (`node scripts/izole-worktree.mjs ac <dizin>`). `eoas`'ın kendi temiz ağaç kontrolü o bayrakla da atlanmaz; `EXPO_PUBLIC_*` git'teki `apps/mobile/release.env`'den geldiği için worktree'de de eksiksizdir (`memory/reference_ios_ota_pipeline_android_only.md`)

**Çalışma alışkanlıkları**
- **Oturum kapanışı**: commit ürettiğin her oturumu yayın durumu tablosuyla bitir — commit · push · prod deploy · her OTA kanalı. Yayınlanmayan her satıra engeli ve bitirecek tek komutu yaz; deploy'u bilerek kullanıcıya bırakıyorsan tabloda öyle belirt
- **Çıkmaz yol bütçesi**: hata ayıklamaya başlamadan yeniden üretim yollarını sinyale ulaşma süresine göre sırala (birim test · log/DB sorgusu · emülatör · gerçek cihaz · web araması). ~15 dk somut sinyal yoksa dur, neyi elediğini raporla, sıradakine geç
- **Önce ölç, sonra refactor**: 3+ dosyalık refactor/yeniden adlandırmadan önce dosya:satır listesi çıkar (yorumlar ayıklanmış; web/mobil/shared ayrı toplam). Tahmini sayıyla plan kurma — "54 çağrı noktası" ölçülünce 172 çıktı
- **Sözdizimi kancası**: düzenlenen JS/TS dosyası anında `scripts/hooks/sozdizimi-kontrol.mjs` PostToolUse kancasından geçer (yalnız sözdizimi, tip değil). Kanca hata dönerse başka işe geçmeden düzelt. `.claude/settings.json` gitignore'da → kurulum her makinede bir kez, parça betiğin başındaki KURULUM notunda

## TEST KOMUTLARI & PROJE-ÖZEL KURALLAR
> Genel test felsefesi (yaz+çalıştır+doğrula+raporla, bug fix RED test, ne zaman gereksiz) global `~/.claude/CLAUDE.md → Test Disiplini` bölümünde — burada SADECE Apartora'ya özel komutlar ve kurallar.

- **API route (unit)** → `npm run test:unit:api` (vitest) | **API route (E2E)** → `npm run test:api` (`tests/api`)
- **Component/hook** → `npm run test:unit` | **Lib/util** → `npm run test:unit:lib` | **Hooks** → `npm run test:unit:hooks`
- **Sayfa akışı (E2E)** → `npm run test:e2e` | Rol bazlı: `test:manager` / `test:owner` / `test:tenant` / `test:staff`
- **Mobil** → `npm run test:mobile` | Portal: `test:mobile:security` / `test:mobile:cleaning` | Tablet: `test:tablet`
- **RLS (yeni tablo + policy)** → `npm run test:rls` (vitest.rls.config.ts) — `src/lib/__tests__/rls/*.test.ts`
- **Toplu unit** → `npm run test:all` | **E2E debug** → `test:e2e:headed` / `test:e2e:debug` / `test:e2e:ui`
- **Proje-özel zorunluluklar**: Yeni API endpoint → unit + **kontrat testi** | Yeni tablo + RLS → RLS testi | Shared util/hook (`src/lib/`, `src/hooks/`) → unit test
- **Katman kontratı (Apartora pattern)**: API response shape frontend beklentisiyle uyuşmalı — sadece status değil body shape assert et (detay: `memory/feedback_test_coverage_by_layer.md`)
- Mevcut testleri KIRMA, değişiklik sonrası ilgili komutu çalıştır

## MOBİL & DARK MODE
- Kullanıcıların çoğu **mobil + dark mode** kullanıyor — bu iki alan öncelikli
- Detaylı kurallar ve pattern'ler → `.claude/rules/coding-patterns.md` (component/hook/sayfa düzenlerken otomatik yüklenir)

## SORUN GİDERME
- **typecheck bekliyor ("Başka bir typecheck koşuyor")**: normal — paralel oturumlar `scripts/typecheck-lock.mjs` ile serileştirilir (aynı anda tek tsc). Ağaç değişmediyse bekleyen koşu sonucu paylaşır, tsc hiç çalışmaz. Acil atlatma: `TYPECHECK_LOCK=0 npm run typecheck`. Takılı kilit: `rm -rf node_modules/.cache/typecheck-lock`
- **ChunkLoadError**: `npm run dev:clean` veya tarayıcı cache temizle

## DEPLOYMENT
- **Platform**: Coolify + Docker (Vercel KULLANILMIYOR) | **URL**: https://apartora.com
- Günlük: `npm run dev:clean` | Deploy öncesi: `docker compose up --build`

## YEDEKLEME (DR) — ✅ ÇALIŞIYOR (2026-05-04)
- **Mekanizma**: `pg_dump` → `gzip` → `GPG (AES-256)` → R2 `apartora-db` bucket. Cronicle (Alpine container) script'i koşturur
- **Sıklık**: Günlük 23:00 UTC (7 gün) + Haftalık Pazar (4 hafta) + Aylık ayın 1'i (12 ay) + Aylık restore drill (4:00 UTC)
- **Cronicle event ID'leri**: kod sabit `src/lib/backup/r2-backup.ts → CRONICLE_EVENTS`
- **Script kaynağı**: `scripts/cronicle/{backup-supabase.sh,restore-drill.sh}` (Cronicle event'leri inline kopya — UI'dan değişiklik repo'ya yansıtılmalı)
- **Şifreleme parolası**: `C:\Users\furka\OneDrive\Belgeler\apartora-backup-parolasi.txt` + `C:\Users\furka\Documents\apartora-backup-parolasi.txt`. Cronicle event env'inde de var. Env'e (Apartora) KOYMA
- **Restore**: UI'dan engelli (CLI prosedürü `scripts/cronicle/README.md`)
- **Audit retention**: pg_cron `cleanup-all-logs` (03:00 UTC, ~180 gün). Backup 23:00'a alındı ki cleanup öncesi son kayıtlar yedeğe girsin
- **Coolify env eklenmeli (production)**: `CRONICLE_API_KEY`, `R2_BACKUP_BUCKET=apartora-db`, `SUPABASE_DB_URL` (opsiyonel, app kullanmıyor)
- **Detay**: `memory/project_supabase_backup.md`

## ÖNEMLİ NOTLAR
- **Lint seçici çalıştır** (detay: `memory/feedback_lint_run_selectively.md`) — sadece push öncesi / 3+ dosya / yeni component-hook-API
- **`/simplify` seçici çalıştır** (lint ile aynı eşik) — 3+ dosya / yeni component-hook-API / push öncesi. Tek satır fix, i18n ekleme, CSS, yorum değişikliğinde ÇALIŞTIRMA (bulacak bir şey yok, "istenmeyen refactoring" kuralını çiğner). Kalite denetler ve **düzeltir**; hata avı değil → o `/code-review`
- DB değişikliklerinde: `db_change.md` güncelle
- Major değişiklik/deploy öncesi: `docker compose up --build` ile test

## ÖDEME TEST & ADMİN
- **Primary provider**: PayTR (test kartları: `memory/reference_paytr_test_cards.md` — Visa/MC/Troy, CVV=000)
- **iyzico (legacy)**: `5892830000000000` | SKT: `12/28` | CVV: `703`
- **Admin email**: `apartora@apartora.com`

## SKILLS
`/mobile-review` `/i18n-check` `/pdf` `/xlsx` `/docx` `/pptx` `/frontend-design` `/webapp-testing`

## PATH-SCOPED RULES
Detaylı kurallar `.claude/rules/` altında, sadece ilgili dosyalarda çalışırken yüklenir:
- `api-rules.md` → `src/app/api/**` — auth, client types, rate limit, error handling, validation
- `testing-rules.md` → `tests/**, **/__tests__/**` — komutlar, yazma kuralları, dosya konumları
- `deployment-rules.md` → `Dockerfile, docker-compose.*` — Docker test kuralları
- `validation-rules.md` → `src/lib/validation/**, src/components/**, src/app/api/**` — Zod schema, form validation, preprocessFormSchema, TR validatorlar
- `coding-patterns.md` → `src/components/**, src/hooks/**, src/lib/**` — i18n, feature flags, logging, hooks, error handling
- `ui-components.md` → `src/components/**, src/app/**` — kanonik UI bileşen tablosu (diyalog, toast, durum ekranları, tablo, rozet, token, mobil karşılıklar) + `ui-component-choice.guard`

---
*Proje: Apartora Manager — stack versiyonları `package.json`'da*

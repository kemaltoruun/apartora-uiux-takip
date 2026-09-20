# Kodlama Desenleri — `src/components/**`, `src/hooks/**`, `src/lib/**`

> Bu dosya **koddan türetildi** (ölçüm: 2026-08-21), tasarlanmadı. Sayılar o günkü gerçek
> kullanım sayılarıdır — bir deseni değiştirmeden önce sayıyı yeniden ölç.
> Genel kurallar `CLAUDE.md`'de; burada yalnız bu üç dizine ait olanlar var.

---

## 1. i18n — `t()` nereden gelir

**Canlı desen (2090 çağrı yeri / 1666 dosya):**

```ts
import { useI18n } from '@/components/providers/I18nProvider'
const { t } = useI18n()
```

- Import yolu **`@/components/providers/I18nProvider`** (1166 dosya). `@/hooks/i18n` barrel'ı yalnız 2 dosyada — yeni kodda onu kullanma.
- `locale` de gerekiyorsa: `const { t, locale } = useI18n()` (54 dosya).
- **Diller: tr / en / ar / ru** — `messages/*.json`. Bir anahtar eklerken **dördüne birden** ekle.
  `ar` **RTL**'dir. Yön yönetimi **global**: `I18nProvider` locale değişiminde
  `document.documentElement.dir`'i kendisi ayarlar — bileşende elle `dir` set etme.
  Bileşen içinde gerekiyorsa `const { isRTL, direction } = useI18n()`.
  (`useRTL()` hook'u `@/hooks/i18n/rtl`'de duruyor ama **fiilen kullanılmıyor** — 1 dosya.)

**Bilinçli borç — bilerek uyma, tek başına migrate etme:**
`useI18n()` kaynak dosyasında `@deprecated` işaretli; performans için `useI18nState()` /
`useI18nActions()` ayrımı var (`t` + `setLocale`, state değişiminde **re-render olmaz**).
Ama benimseme **2'şer dosya** — yani depo geneli hâlâ `useI18n()`. Yeni dosyada split hook
kullanmak serbest; **mevcut 1666 dosyayı toplu çevirmek ayrı bir iş**, izinsiz başlatma.

**`@/hooks/i18n/deprecated.ts`** — `useCommonTranslation`, `useAuthTranslation` gibi 17 alias.
Hepsi `@deprecated`. Yeni kodda **kullanma**; dokunduğun dosyada görürsen `useI18n()`'e çevir.

**Public sayfa tuzağı (dosyada yazılı, ölçülmüş):** `@/hooks/i18n` barrel'ı dashboard
hook'larını **re-export ETMEZ**. Ederse zincir `public sayfa → barrel → dashboard/*` kurulur
ve **53 KB dashboard kataloğu public bundle'a girer**. Dashboard hook'u lazımsa
`@/hooks/i18n/dashboard`'dan **doğrudan** al.

**Dinamik anahtar YASAK:** ``t(`ns.${x}Title`)`` — bulamadığı anahtarın kendisini döndürür,
ekranda ham `ns.fooTitle` yazar. tsc/lint temiz kalır, parite mandalları da yakalamaz
(anahtar hiçbir dilde yoksa "tutarlı" görünür). Mandal: `dynamic-i18n-key.guard`, tavan 0.

---

## 2. Logging — `console.log` YASAK

```ts
import { logger } from '@/lib/logger'   // 1820 dosya — tek doğru biçim
```

API: `logger.debug(msg, data?)` · `logger.info(msg, data?)` · `logger.warn(msg, data?)` ·
`logger.error(msg, error?, data?)`. Hata **ikinci** parametredir, data üçüncü.

- **Yakalanan hatayı data'ya GÖMME** — `logger.error(msg, { hata })` yasak, `logger.error(msg, hata, {...})` doğru.
  `Error` enumerable alan taşımaz: `JSON.stringify({ error: new Error('x') })` → `{"error":{}}`.
  Data `metadata.data` altında JSONB'ye yazıldığı için gömülen hatadan geriye **`{}`** kalır —
  stack değil, **mesajın kendisi bile** kaybolur. İkinci parametredeki `Error` ise Sentry'de
  `captureException` ile gruplanır (aksi hâlde synthetic `LoggedError` üretilir ve her çağrı yeri
  `unified-logger.ts` satırına toplanır), `error_logs.stack`'e gerçek yığını yazar.
  Mandal: `logger-error-arg-position.guard`, ham gömme tavanı **0**.
  **İstisna:** PostgREST hatası (`const { data, error } = await supabase...`) `Error` örneği DEĞİL,
  düz nesnedir — `{code, message, details, hint}` olarak kayıpsız serileşir, data'da durabilir.
- `captureError` da aynı barrel'dan gelir (`@/lib/logger`, 28 dosya).
- **`console.*` borcu KAPANDI (ölçüm 2026-09-08).** Üretim kodunda toplam **15 çağrı** var ve
  hepsi meşrudur: 11'i `unified-logger.ts`'in kendisi (logger console'a yazar), 2'si
  `instrumentation.ts` (logger henüz hazır değil), 1'i `layout.tsx`'teki inline tema script'i
  (`dangerouslySetInnerHTML`, modül yok), 1'i `use-mobile.tsx` (dev-only + eslint-disable).
  Gerçek ihlal 0.
  ⚠️ Bu satır 2026-09-08 öncesinde **"62 dosyada `console.log` var"** diyordu ve YANLIŞTI:
  o sayı yorum ayıklamadan yapılmış naif bir `grep`'ti — 313 eşleşmenin neredeyse tamamı
  JSDoc **örneklerinin içindeydi** (`* console.log(info.browser); // "Chrome"`). Yanlış sayı
  yanlış öncelik üretir; ölçerken yorumları soy.
- **`logger.warn` ve `logger.info` `error_logs`'a ASLA yazmaz.** `unified-logger.ts:69`
  → `DB_LOG_THRESHOLD = LogLevel.ERROR`, koşul `level >= DB_LOG_THRESHOLD`; `WARN=2 < ERROR=3`.
  İkisi de yalnız console'a (container stdout) gider, orada da toplayıcı yok.
  Kalıcı iz istiyorsan **`logger.error`** ya da konuya özel bir tabloya yaz.
  ⚠️ Bu satır 2026-08-28 öncesi `warn`'ı da "kalıcı" sayıyordu ve YANLIŞTI:
  `/api/security/csp-report` ucu ihlalleri `logger.warn` ile "topluyordu",
  sayfa başına 7 rapor geliyordu ve veritabanında **4 ay boyunca 0 satır** vardı.
  Eşiği düşürmek çözüm DEĞİL — depoda **1147 `logger.warn` çağrı yeri** var,
  `error_logs` boğulur. Bkz. hafıza `reference_logger_warn_never_persists`.
- **`after()` içinde `logger.error` `error_logs`'a ULAŞMAZ** → `finally { await logger.flush() }` şart.
- Hassas veri (TC, IBAN, parola, token) **asla** log'a girmez — `data` nesnesini yollamadan önce ayıkla.

---

## 3. Hata yönetimi

- Catch bloğunda hatayı **yutma**: ya `logger.error` ile logla ya yukarı fırlat.
- Kullanıcıya giden mesaj **Türkçe ve anlaşılır**; stack trace kullanıcıya **gitmez**.
- **PostgREST hatası `Error` örneği DEĞİL** → `err instanceof Error` **false** döner ve
  `error_logs`'a boş stack yazar. Ortak çözüm `describeError()` — bkz. hafıza
  `reference_postgrest_error_not_error_instance`.
- `.single()` 0 satırda **hata döndürür**. Dönüşü okumadan geçme, yoksa sessizce yutulur.
- TanStack sorgusunda **`isError` dalını yazmayı unutma**: hata hâlinde `data` `undefined` kalır,
  `?? []` / `?? 0` ile karşılarsan **başarısızlık gerçek veri gibi çizilir** ("0 mesaj kaldı",
  "Bakiye bilgisi yok"). `query-error-branch.guard` **artık web'de de var** (2026-09-12,
  `src/lib/__tests__`; çift yönlü defter, 232 kayıt donduruldu, defterde para ekranı yok) ve
  `apps/mobile` ikizi yerinde duruyor. Kardeşi `error-swallowed-as-data.guard` yalnız `queryFn`
  **içinde** yutulan hatayı yakalar ve `null`u bilerek dürüst sinyal sayar; `null`u `?? 0`a çeviren
  TÜKETİCİ yalnız yeni mandalda görünür. Kanca yazıyorsan `isError`ı **dışarı ver** — daraltılmış
  dönüş (`{ total: q.data?.total ?? 0, isLoading }`) tüketiciyi dallanmaktan alıkoyar. Durum ekranı bileşenleri → `ui-components.md` §3.

---

## 4. Veri çekme — `apiFetch`

```ts
import { apiFetch } from '@/lib/utils/api-fetch'   // 808 dosya
```

Ham `fetch('/api/...')` **kullanma** — depoda 25 dosya kalmış, hedef 0. Mutation için
doğrudan Supabase client'ı da kullanma; **API Route** üzerinden git (`CLAUDE.md → API KURALLARI`).

---

## 5. Feature flag / modül erişimi

`@/lib/features/access-control`:
- `isFeatureEnabled(...)` / `checkFeatureAccess(...)` — asenkron, önbellekli
- `invalidateFeatureFlagCache(featureKey?)` — flag değiştirince **çağrılmalı**
- `decideFeatureAccess(state, entitled)` — saf karar fonksiyonu, test edilebilir

**Yönetici menüsü koddan gelmez, `feature_modules` TABLOSUNDAN gelir.** Menüde bir şey
eksikse **önce tabloyu sorgula**, kodu arama. Canlı bileşen `sidebar/unified/UnifiedSidebar.tsx`.
Bkz. hafıza `reference_sidebar_feature_modules_traps`.

---

## 6. Hook yazarken

- **Global kaynak/seçim sahiplenen hook tek depoya bağlanmalı.** `useSession` 38 abonelikte
  60+ özdeş sorgu üretmişti; site/org seçimi `useState`'te tutulunca dağıldı.
  Mandal: `global-context-store-ownership`.
- **Memoize edilmemiş hook NESNESİ deps dizisinde = React #185** (prodda iki kez).
  ESLint `exhaustive-deps` bu hatanın **kendisini önerir** — lint'e körü körüne uyma.
- Org/site kapsamlı veri çeken hook'ta **`queryKey`'e org/site id KOY**. Taşımazsa
  `switchOrg` sonrası önceki şirketin verisi ekranda kalır (çapraz-kiracı sızıntısı).

---

## 7. Mobil + dark mode (öncelikli alan)

Kullanıcıların çoğu mobil + dark mode. Bu iki alanda "sonra bakarız" yok.

**Dark mode (web):** `darkMode: ["class"]` (`tailwind.config.ts`). 604 bileşen `dark:` sınıfı
kullanıyor, `next-themes` 23 dosyada. Yeni bileşende **her renk için `dark:` karşılığı yaz** —
zemini `bg-background`, metni `text-foreground` gibi semantik token'lardan seç; ham `bg-white`
/ `text-black` dark'ta kırılır.

**Responsive:** 376 bileşen `md:`/`lg:` kullanıyor. Varsayılan mobil, breakpoint'ler yukarı doğru.

**ÜÇ HEDEF hatırlatması** (`CLAUDE.md`): bu dizinlerde yazdığın bir kural/hesap/tip mobilde de
gerekiyorsa yeri **`packages/shared`**'dır. Sunum katmanı paylaşılmaz, **kontrat paylaşılır**.

**Design token BOŞLUĞU (açık borç):** web `tailwind.config.ts` (400 satır) ile mobil
`apps/mobile/tailwind.config.js` (73 satır) **iki ayrı palet**. Ortak token dosyası YOK →
dark mode iki hedefte **ayrı ayrı** bozulabilir. Renk eklerken ikisini de gözden geçir.
Bkz. hafıza `architecture_web_mobile_sync_strategy` B2.

---

## 8. Tip güvenliği

- `any` **yasak**. Gerekiyorsa `unknown` + daraltma, son çare `as unknown as T`.
- API yanıtı için tip **zorunlu** — tercihen `@apartora/shared`'daki Zod şemasından `z.infer`.
- Fonksiyon imzası / export / tip **daraltma** yapma (istenmediyse) — çağıranları kırar.

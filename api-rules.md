# API Kuralları — `src/app/api/**`

> Koddan türetildi (ölçüm 2026-08-21): **1095 route dosyası**. Sayılar o günkü gerçek
> kullanım sayılarıdır. Bir kuralı gevşetmeden önce sayıyı yeniden ölç.

---

## 1. Handler seç — çıplak `export async function GET` yazma

`@/lib/api/api-handler` içindeki wrapper'lar auth + hata + yanıt standardını **tek yerde** tutar.

| Handler | Kullanım | Ne zaman |
|---|---|---|
| `apiHandler` | 627 dosya | Genel; `{ requireAuth, roles }` seçenekleriyle |
| `superAdminHandler` | 338 dosya | superadmin/semiadmin uçları |
| `managerHandler` | 155 dosya | Yönetici uçları (site kapsamı otomatik) |
| `residentHandler` · `siteMemberHandler` · `securityHandler` | — | Rol kabuğuna göre |
| `permissionHandler` · `multiPermissionHandler` | — | İzin bazlı kapı |
| `publicHandler` | — | Auth gerektirmeyen |
| `cronHandler` | — | `CRON_SECRET` ile doğrulanan job'lar |

```ts
export const POST = apiHandler(
  async (request, { user, profile, supabase }) => { ... },
  { requireAuth: true, roles: ['superadmin', 'manager'] }
)
```

**⚠️ `ctx.supabase` HER YERDE service-role'dür — RLS SENİ KORUMAZ.**
`apiHandler` ailesi `baseAuthenticate` üzerinden geçer ve orada client
`createClient(URL, SUPABASE_SERVICE_ROLE_KEY)` ile kurulur
(`src/lib/middleware/auth.ts`, yorumu: "Service Role - RLS bypass").
Bu satır 2026-09-06 öncesinde "cronHandler tek istisnadır, diğerlerinde normal
server client vardır" diyordu ve **YANLIŞTI**; yanlış varsayım yüzünden beş uçta
istemciden gelen `siteId` hiç doğrulanmadan sorguya konmuştu (çapraz-site okuma
ve BAŞKA sitenin cüzdanına kredi yazma). Mandal: `client-site-id-verification.guard`.

`cronHandler` yalnız şu bakımdan ayrıdır: auth session taşımaz, bu yüzden
service-role kullanımı zorunludur (anon client SECURITY DEFINER RPC'lerde
"permission denied" alır).

**Service-role kullandığın her yerde site/org kapsamı SORGUDA olmak zorundadır** — RLS seni
korumaz. Bkz. hafıza `reference_service_role_ctx_supabase_no_rls`.

---

## 2. Hata yanıtı — `NextResponse.json` ile elle yazma

`@/lib/api/error-response` hazır yardımcıları döndürür (hepsi tipli `TypedApiErrorResponse`):

`unauthorized()` · `forbidden()` · `insufficientRole()` · `notFound()` · `resourceDeleted()` ·
`validationError()` · `invalidInput()` · `missingParameter()` · `rateLimited(retryAfter?)` ·
`featureDisabled()` · `noCredits()` · `subscriptionRequired()` · `siteAccessDenied()` ·
`orgAccessDenied()` · `internalError()` · `serverError()` · `databaseError()` ·
`externalServiceError()` · `apiError(...)`

Gerçek kullanım: `validationError` 408 · `notFound` 371 · `forbidden` 321 · `apiError` 16.

**Açık borç:** 1095 route'un **211'i hâlâ ham `NextResponse.json` kullanıyor** (%19).
Hedef 0. Dokunduğun route'ta gördüğünü çevir; ilgisiz route'a dokunma.

**Eksik parametre `badRequest` ile DÖNMEZ** (2026-09-12): zorunlu bir değer İSTEKTE
(query/body/form) hiç gelmediyse `ApiResponse.missingParameter('<param>', '<mesaj>')`
kullan — çoklu ad için dizi ver. `badRequest`in kodu sabit `BAD_REQUEST`tir ve bu kod
iş kuralı reddinde de kullanılıyor; istemci kuralı (`@apartora/shared →
beklenenApiReddiMi`) ikisini gövdeden ayırt edemediği için **istemci hatasını sessizce
`warn`a düşürüyordu** — süperadmin "Ek Giderler" sekmesi `site_id` göndermiyordu, uç
30 günde 6 kez 400 döndü, hiçbir alarm çıkmadı. Kullanıcıya görünen Türkçe mesaj
değişmez, ayrım yalnız `code` alanında kazanılır; ucun kendi `details` bilgisi
(ör. `errorCode`) korunur. `ApiResponse.missingParameter` gövdeyi
`error-response.ts → missingParameter`a devreder (tek uygulama, iki giriş).
Mandal: `eksik-parametre-kodu.guard`, defter dışı tavan **0**, defter iki yönlü.

Kapsam dışı (gerekçeleri mandal defterinde yazılı): oturum/profil durumundan çözülen
değerler (`getSiteIdForUser`, `activeSiteId` — istemci parametreyle düzeltemez),
herkese açık uçların alan/captcha kapıları (bot trafiği alarm üretmemeli), saklı
verinin eksikliği ve `[id]` route'ları (`extractRouteUuid` hem eksik hem BOZUK
segmentte null döner → orada sorun "geçersiz kimlik"tir; 143 çağrı / 72 dosya).

### 2.1 Bozuk JSON gövdesi — sarmalayıcı zaten 400 döner

`await request.json()` bozuk/boş gövdede `SyntaxError` fırlatır. **Bunu elle sarmak
ZORUNLU DEĞİLDİR:** `withErrorHandler` (tüm `apiHandler` ailesinin içinde) hatayı
`isInvalidJsonBody()` ile doğrulayıp `validationError('Geçersiz JSON formatı')`
döndürür — ampirik ölçüldü, **400** (`api-handler.ts`; commit `421af3282`).

Zarar veren şey **çağrıyı geniş bir `try`ın içinde okuyup `catch`te 5xx dönmektir**:

```ts
try {
  const body = await request.json();      // ← hata BURADA yakalanır,
  ...                                      //   sarmalayıcıya HİÇ ULAŞMAZ
} catch {
  return internalError('Beklenmeyen bir hata oluştu');   // kullanıcı hatası → 500
}
```

Kanonik biçim (geniş `try`a dokunmaz, tipi korur):

```ts
const body = await request.json().catch(() => null);
if (body === null) return ApiResponse.validationError('Geçersiz JSON formatı');
```

⚠️ Bu satır 2026-09-06 öncesinde yoktu ve mandalın kendisi ters varsayım taşıyordu:
"korumasız çağrı 500 döner" denip 247 nokta borç sayılmıştı; **179'u zaten 400
döndüren doğru koddu**. Gerçek ihlal 49'du, kapatıldı. Mandal:
`unguarded-request-json.guard`, tavan **0** — ve kendi dayanağını (sarmalayıcının
400 eşlemesi) ampirik sınar.

---

## 3. Doğrulama — `withValidation`

`@/lib/api/with-validation` (320 dosya):

- `withValidation(schema, handler)` — request body
- `withQueryValidation(schema, handler)` — query string
- `withFullValidation(...)` — body + query birlikte
- `withPublicValidation(...)` — auth'suz uçlar
- `commonSchemas` · `createListQuerySchema()` · `createSiteQuerySchema()` — hazır parçalar

Şema kurallarını **varsayma** — hangi alan zorunlu, min uzunluk ne, özel kural var mı:
uygulamadan önce doğrula. Detay: `.claude/rules/validation-rules.md`.

Yanıt gövdesi mobil tarafından parse ediliyorsa şema **`packages/shared`**'da olmalı
(`z.infer` ile tip türet) — web API route'larının **81'i** zaten `@apartora/shared` import ediyor.

---

## 4. Rate limit — hangi handler GERÇEKTEN limitliyor

**Kullanımda olanlar:** `rateLimitedPublicHandler` (78) · `registerRateLimitedHandler` (11) ·
`checkAvailabilityRateLimitedHandler` (10) · `passwordResetRateLimitedHandler` (3) ·
elle `checkRateLimit` (48).

**DOĞRUDAN ÇAĞRILMAYAN EXPORT'LAR (0 route), ama İKİSİ AYNI ŞEY DEĞİL:**
`managerApiRateLimitedHandler` → 0 route **ve hiçbir yerden çağrılmıyor**
`superAdminApiRateLimitedHandler` → 0 route ama **`superAdminHandler` ile
`semiAdminHandler` onu İÇERİDEN çağırıyor** (`api-handler.ts:1956` ve `:1987`,
`enableRateLimit` varsayılanı `true`).

Yani:
- **`superAdminHandler` / `semiAdminHandler` LİMİT UYGULAR** (varsayılan açık;
  kapatmak için üçüncü argüman `false` verilir).
- **`managerHandler` limit UYGULAMAZ** — kaynakta gerekçesi yazılı: rate limiting
  `useSiteRole` ile uyumsuz, TODO olarak duruyor (`api-handler.ts:2053`).

⚠️ Bu satır 2026-09-06 öncesinde "düz `managerHandler`/`superAdminHandler` limit
UYGULAMAZ" diyordu ve süperadmin yarısı YANLIŞTI. Yanlış varsayımla ikinci bir
limit katmanı eklemek çift sayım üretir. Yeni bir MANAGER ucu limit istiyorsa
açıkça eklenmeli. Bkz. hafıza `reference_rate_limit_real_coverage`.

---

## 5. Auth notları

- `authenticateAdmin(request, ['role1','role2'])` — 15 dosya; wrapper kullanamadığın yerler için.
- Multi-role: `useSiteRole: true` ise etkin rol **`site_users`**'tan gelir, `profiles.role`'dan DEĞİL.
  Kararı `ctx.effectiveRole` üzerinden ver.
- `ctx.externalScope` yalnız `effectiveRole === 'external'` iken `scoped: true` olur;
  kullanımı `applyScopeFilter(query, ctx.externalScope, 'apartment_id')`.
- **`authenticated` rolü bir SINIR DEĞİLDİR** — SECURITY DEFINER RPC'ler RLS'i atlar ve
  her oturumlu kullanıcıya açıktır. Yeni SECDEF RPC'de EXECUTE'u açıkça daralt.

---

## 6. Yeni endpoint kontrol listesi

1. Doğru handler seçildi mi (rol/izin kapısı)?
2. Hata yolları `error-response` yardımcılarıyla mı dönüyor?
3. `withValidation` bağlandı mı, şema doğru yerde mi (`packages/shared` gerekiyor mu)?
4. Rate limit gerekiyor mu — gerekiyorsa **açıkça** eklendi mi?
5. Service-role kullanıldıysa site/org filtresi sorguda mı?
6. Unit test + **kontrat testi** yazıldı mı (body shape, sadece status değil)?
7. Yeni tablo eklendiyse `GRANT` verildi mi (yoksa supabase-js `42501`)?

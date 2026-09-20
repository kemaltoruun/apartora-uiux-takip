# Doğrulama Kuralları — `src/lib/validation/**`, `src/components/**`, `src/app/api/**`

> Koddan türetildi (ölçüm 2026-08-21). Zod kullanılıyor; hazır TR validatorlar
> `src/lib/validation/helpers.ts` içinde — **yenisini yazmadan önce buraya bak**.

---

## 1. Hazır validatorlar — sıfırdan yazma

`@/lib/validation/helpers` (hepsi Zod şeması döndürür, mesajlar Türkçe):

**Kimlik / kurum**
- `tcKimlikNo()` · `optionalTcKimlikNo()` — algoritma doğrulaması dahil (`validateTcKimlikAlgorithm`)
- `taxNumber()` · `optionalTaxNumber()` — VKN
- `iban()` — IBAN

**İletişim**
- `phoneNumber(defaultCountry = 'TR')` · `turkishPhoneNumber()` · `optionalPhoneNumber()`
- `email()` · `optionalEmail()` · `optionalUrl()`

**Kimlik/anahtar**
- `uuidSchema(message?)` · `optionalUuid()` · `isValidUuid(value)` · `UUID_REGEX`
- `permissionKey()` · `optionalPermissionKey()` · `validatePermissionKey()` · `validatePermissionKeys()`

**Genel**
- `fullName()` · `isValidFullName()` · `optionalString(maxLength?)`
- `positiveNumber(fieldName?)` · `positiveAmount(fieldName?)` · `dateString()`
- `password()` · `confirmPassword()`
- `emptyStringToNull(schema)` · `emptyStringToUndefined(schema)` — boş form alanı dönüşümü

---

## 2. `.uuid()` TUZAĞI — Zod v4 tohum ID'leri reddeder

Zod v4 `.uuid()` **RFC 4122 sürüm+varyant nibble'ı** zorlar; Postgres zorlamaz.
Sonuç: **DB'de geçerli olan ID API'de 400 döner.** Üretimde ölçüldü — `sites` 463 kaydın 4'ü,
`profiles` 447 kaydın 14'ü RFC dışı. Destek Talepleri ekranı **bu yüzden hiç açılmıyordu**
(izin/abonelik/veri hepsi doğruydu).

**Üç kez görünmez:** tip geçer, lint geçer, **testler de geçer** (fixture'lar gerçek v4 ID
kullanır) — yalnız tohum/legacy veriyle çalışan kullanıcı görür.

**Kanonik kullanım — ham `z.string().uuid()` YAZMA:**
- API şeması → `uuidSchema` / `siteIdSchema` (`src/lib/validation/api-schemas/base.ts`,
  `z.string().regex(UUID_REGEX, ...)`)
- Web form/util → `uuidSchema()` / `UUID_REGEX` (`@/lib/validation/helpers`)
- Web+mobil ortak → `packages/shared/src/uuid.ts` (`UUID_REGEX`, `uuidSchema()`)

Mandal bir **test**tir: `src/lib/__tests__/zod-uuid-strictness.guard.test.ts` — üretim kodunda
`z.string().uuid()` kullanımını yakalar, **tavan 0**. `npm run test:invariants` onu korur.

---

## 3. Form doğrulama

- Resolver: **`formResolver()`** (`@/lib/validation/form-resolver`) — React Hook Form + Zod köprüsü.
- **`preprocessFormSchema(schema)`** (`src/components/security-portal/staff/staff-utils.ts`,
  32 kullanım) — form alanlarını şemaya vermeden önce normalize eder (boş string → undefined vb.).
  Yeni bir formda ham `zodResolver` yerine önce bu ikisine bak.
- Boş bırakılabilen alanlarda `emptyStringToNull` / `emptyStringToUndefined` sarmalayıcısını kullan;
  yoksa `""` değeri sayı/tarih şemasını patlatır.

---

## 4. `maxLength` — doğrulanan kimlik alanına KOYMA

Birden fazla geçerli uzunluk varsa `maxLength` **sessizce bozar**. Gerçek vaka:
VKN(10)/TCKN(11) ortak alanında `maxLength={11}` 12 haneli girişi kırpıp **geçerli görünen
yanlış bir TCKN** üretiyordu. Uzunluğu şema doğrulasın, input kırpmasın.

---

## 5. Şema nerede yaşamalı (ÜÇ HEDEF)

- Yanıt gövdesini **mobil de** tüketiyorsa şema **`packages/shared`**'da olur; TS tipi
  `z.infer`'den türer. **Giriş = Zod, çıkış = interface.**
- Yalnız web formuna ait şema `src/lib/validation/` altında kalabilir.
- `src/lib/validation/api-schemas/` → API route şemaları; `withValidation` ile bağlanır.

**Şifre alt sınırı — KAPANDI (2026-09-11):** `PASSWORD_MIN_LENGTH` artık sunucu
(`register-resident`, sıfırlama şeması, `helpers.password()`), web ve mobil tarafından tüketiliyor.
Public sayfalar barrel yerine alt yoldan alır: `@apartora/shared/validation-rules`. Mandal:
`packages/shared/src/__tests__/sifre-alt-siniri.test.ts` (şifre uzunluğu literalle yazılamaz, tavan 0).

**Açık borç (telefon):** `TR_PHONE_REGEX` / `isValidTrPhone` için "web tüketmiyor" notu
2026-08-21 ölçümüdür, yeniden ölçülmedi. Telefon kuralına dokunduğunda ortak tanıma geçir.

---

## 6. Kural uydurma

Hangi alan zorunlu, min uzunluk kaç, hangi format kabul edilir — **varsayma**.
Mevcut ekranın alanlarını + DB kolonlarını oku, gerekiyorsa sor. Şifre minimumu bu depoda
bir kez web'in **kendi içinde** çelişkiliydi (register 8, api-schemas/auth 6, mobil login 6).
Karar: yeni şifre = 8, login = yalnız "zorunlu" (eski kullanıcı kilitlenmesin). Kullanıcı
2026-09-11'de yeniden onayladı: mevcut 6 haneli şifreler girişte çalışmaya devam eder, yalnız
yeni belirlenen şifre (kayıt, sıfırlama, değiştirme) 8+ ister.

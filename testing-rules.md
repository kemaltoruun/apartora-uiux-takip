# Test Kuralları — `tests/**`, `**/__tests__/**`

> Koddan türetildi (ölçüm 2026-08-21): **707 `__tests__` dizini**.
> Genel test felsefesi (yaz + çalıştır + doğrula + raporla, bug fix'te RED test) global
> `~/.claude/CLAUDE.md → Test Disiplini`'nde. Burada yalnız Apartora'ya özel olanlar.

---

## 1. `test:mobile` MOBİL UYGULAMAYI TEST ETMEZ — en sık yapılan hata

| Komut | GERÇEKTE ne koşar |
|---|---|
| `npm run test:mobile` (kök) | **Playwright, web'i mobil viewport'ta** (`Mobile Chrome`/`Mobile Safari`) |
| `test:mobile:security` / `test:mobile:cleaning` / `test:tablet` | Yine **web** — Playwright proje adları |
| `cd apps/mobile && npm test` | **Expo uygulaması** (jest) ← mobil app testi BUDUR |

Playwright config `apps/mobile`'ı **hiç görmez**. "Mobil testleri çalıştırdım" derken hangisini
koşturduğunu ayırt et; yanlışını koşup yeşil görmek mümkündür.

---

## 2. Katman → komut

| Ne değişti | Komut |
|---|---|
| API route (unit) | `npm run test:unit:api` |
| API route (E2E) | `npm run test:api` (`tests/api`, chromium, workers=1) |
| Component | `npm run test:unit:components` |
| Hook | `npm run test:unit:hooks` |
| Lib / util | `npm run test:unit:lib` |
| Yeni tablo + RLS policy | `npm run test:rls` (`vitest.rls.config.ts`) |
| Sayfa akışı | `npm run test:e2e` · rol bazlı `test:manager` / `test:owner` / `test:tenant` / `test:staff` |
| Toplu unit | `npm run test:all` (lib → api → hooks → components) |
| Expo uygulaması | `cd apps/mobile && npm test` |

E2E hata ayıklama: `test:e2e:headed` / `test:e2e:debug` / `test:e2e:ui` · rapor: `test:report`.

---

## 3. Dosya nereye

- Birim testler kaynağın yanında: `src/**/__tests__/*.test.ts(x)` (707 dizin).
- RLS testleri: `src/lib/__tests__/rls/*.test.ts`.
- Playwright E2E: `tests/e2e/**` · API E2E: `tests/api/**` · yardımcılar `tests/helpers`, `tests/fixtures`.
- Mobil: `apps/mobile/src/**/__tests__/`.

---

## 4. Proje-özel zorunluluklar

- **Yeni API endpoint** → unit test **+ kontrat testi**.
- **Yeni tablo + RLS** → RLS testi.
- **Shared util/hook** (`src/lib/`, `src/hooks/`) → unit test.

**Katman kontratı (Apartora deseni):** API yanıt şekli frontend beklentisiyle uyuşmalı.
Sadece `status` assert etme — **body shape** assert et. Aksi hâlde "200 döndü" diye geçen test,
alan adı değişince sessizce yanlış ekran üretir.

---

## 5. Testler PROD'a yazar — dikkat

- **RLS testlerinin ayrı veritabanı YOK**; üretime yazarlar.
- Unit testler service-role client üzerinden **prod'a yazabiliyor** — bir turda 85 satır yazdığı
  ölçüldü (`typed-client` mock'luydu, `service-client` DEĞİLDİ). Mock'ladığını **ampirik doğrula**,
  varsayma.
- Test verisi temizliği: `@apartora.local` hesaplarına **DOKUNMA** (dokunulmaz sayılır).

---

## 6. Mandal / kapı sistemi

- `npm run test:invariants` → `scripts/invariant-tests.mjs`. Depo genelinde sınıf-bazlı
  değişmezleri kilitler (`platform-native-isolation`, `ios-config`, `query-error-branch.guard`,
  `dynamic-i18n-key.guard`, `zod-uuid-strictness.guard`, `row-a11y-label.guard`, …).
- **`pre-push` kancası** `test:invariants` koşar; `apps/mobile` değiştiyse ayrıca
  `scripts/mobile-jest-gate.mjs` koşar.
- Mobil jest kapısı **worker çökmesini gerçek test hatasından ayırt eder**: çökmede o paketleri
  `--runInBand` yeniden koşar, **gerçek test hatasında ASLA yeniden denemez**. Bu ayrımı bozma —
  rastgele kırmızı kapı insanları `--no-verify`'a alıştırır ve kapı tamamen işlevsizleşir.

**Mandal yazarken:** ÖRNEK imzası değil **SINIF** kodla. Test ölçütü şudur —
*"bildiğim ama henüz düzeltmediğim örnekleri yakalar mıydı?"* Kapsamı tek dosya adına
sabitleyen mandal ertesi gün aynı sınıftan 12 vakayı kaçırdı.

---

## 7. Test yazılmayan durumlar

Yalnız: i18n çeviri eklemesi (string-only), saf CSS/styling, yorum/dokümantasyon.
**Bunların dışında her kod değişikliği test ister.** Atlanacaksa gerekçe yazılı sunulur.

# Deployment Kuralları — `Dockerfile`, `docker-compose.*`

> Koddan türetildi (ölçüm 2026-08-21). Platform **Coolify + Docker**. Vercel KULLANILMIYOR.
> Üretim: https://apartora.com

---

## 1. Docker yapısı

İki aşamalı build (`Dockerfile`):
- `node:22-alpine AS builder` → Next.js **standalone** çıktı
- `node:22-alpine AS runner` → `CMD ["node", "server.js"]`, `EXPOSE 3000`, `HEALTHCHECK` var

**Runner ile builder AYNI Node major olmalı** (dosyada yazılı) — standalone çıktı derlendiği
Node'a bağlıdır. Birini yükseltiyorsan **ikisini birden** yükselt.

`docker-compose.yml` servisleri: `app` (apartora-app) · `notification-worker`
(apartora-notification-worker, `Dockerfile.worker`) · `redis` (redis:7-alpine) ·
`redis-commander`. Ağ: `apartora-network`, kalıcı hacim: `redis_data`.

Port ve URL'leri **talimat vermeden önce `docker-compose.yml`'den doğrula** — tahmin etme.

---

## 2. `NEXT_PUBLIC_*` build-time'dır

Builder aşaması bu `ARG`'ları alır: `NEXT_PUBLIC_APP_URL`, `NEXT_PUBLIC_SUPABASE_URL`,
`NEXT_PUBLIC_SUPABASE_ANON_KEY`, `NEXT_PUBLIC_TURNSTILE_SITE_KEY`,
`NEXT_PUBLIC_TURNSTILE_TEST_MODE`, `NEXT_PUBLIC_VAPID_PUBLIC_KEY`,
`NEXT_PUBLIC_GA_MEASUREMENT_ID`, `NEXT_PUBLIC_GOOGLE_SITE_VERIFICATION`,
`NEXT_PUBLIC_CRONICLE_URL`, `NEXT_PUBLIC_SHOW_TEST_USERS`, `NEXT_PUBLIC_APP_VERSION`,
`NEXT_PUBLIC_GIT_SHA`, `SOURCE_COMMIT`.

Yeni bir `NEXT_PUBLIC_*` eklersen **Dockerfile'a ARG + Coolify'a env** eklemeden
üretimde `undefined` olur — yerelde çalışır, prod'da sessizce boş gelir.

---

## 3. Deploy öncesi

- **Push kapısı prod build'ini GARANTİ ETMEZ**: kancada `npm run build` YOK. Hata yalnız
  ~56 dakikalık uzak build'de çıkar. Büyük değişiklikten önce yerelde
  `docker compose up --build` ile dene.
- Push öncesi: `npm run lint` + `npm run typecheck:check` (web mandalı) temiz olmalı;
  `apps/mobile` dosyası değiştiysen ayrıca `npm run typecheck:mobile:check`.

---

## 4. Coolify işletim notları (koddan türetilemez — hafızadan)

- Uygulama uuid ≠ server uuid. Coolify API'de `restart` = **TAM REBUILD** (~17 dk).
- **Deploy sonrası 502 çoğu zaman PROXY sorunudur, rebuild değil**:
  `POST /servers/{server_uuid}/proxy/restart` → ~15 sn.
- Panel `healthy` gösterirken üretim **502** verebilir; panele değil siteye bak.
- **Prod build koşarken site Cloudflare 522 verebilir** (sistem geneli kaynak açlığı).
  Kendiliğinden düzelir — **REBOOT ETME**.
- BuildKit cache şişer (89 GB ölçüldü). `docker image prune` **cache'e dokunmaz**;
  `--max-used-space` ile budanır. `daemon.json` `builder.gc` ayarı bir kez **prod'u düşürdü**.
- Deploy'un büyük kısmı statik üretimde boşa gidiyor (1561 route'un 1558'i dinamik) —
  bilinen durum, ölçüldü, müdahale **reddedildi**.

Detaylar hafızada: `reference_coolify_502_proxy_restart`, `reference_docker_build_cache_bloat`,
`reference_coolify_build_starves_vps`, `reference_build_static_generation_is_wasted`.

---

## 5. Docker dosyalarına dokunurken

- İki OS'ta da çalışmalı (macOS + Windows) — OS'a özel komut gömme.
- Yeni servis eklersen `docker-compose.yml`'e **container_name + network** ver, yoksa
  Coolify tarafında adlandırma çakışır.
- Değişiklikten sonra **`docker compose up --build` ile yerelde doğrula**, doğrudan push etme.

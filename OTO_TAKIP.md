# Oto takip — sohbet sürekliliği

Apartora UI/UX takip çalışması sohbet kutusuna bağlı değildir.  
**Gerçeklik kaynağı:** GitHub `apartora-uiux-takip` → `ILETISIM.md` + `YONETIM.md` + `APARTORA_GELISIM_KAYDI.md`.

---

## Problem

Cursor sohbeti kapanınca / yeni sohbet açılınca önceki mesajlar agent’ın aklında kalmayabilir.  
Claude / GPT başka yerde yazarsa buradaki agent **otomatik uyanmaz** (GitHub webhook’u yoksa).

### Duraklama (sahip şikâyeti · 2026-09-22) — kök neden

Takip kuralı “oto / §3.6 B devam” diyor. Cursor agent runtime ise arka plan **subagent/shell bitince** çoğu zaman *kısa durum özeti + tur sonu* üretir (“follow-up yoksa gerekmez”). Sonuç: oy yazıldı / push oldu sandığı anda sürüş kesilir; sahip “devam?” demek zorunda kalır.

**Bu takip kuralı kusuru değil + platform çatışmasıdır.** Çözüm: `.cursor/rules/iletisim-oto-takip.mdc` → **ANTI-DURAK** (bildirim ≠ tur sonu; her yanıtta `SIRADAKI:`; push ara kayıttır).

---

## Çözüm — 3 katman

### A) Workspace kuralı (her mesajda) — KURULDU

`.cursor/rules/iletisim-oto-takip.mdc` → `alwaysApply: true`  
Bu klasör açıkken agent her turda: `git pull` → `ILETISIM` oku → gerekirse yönetici yanıtı.

### B) İnsan / diğer AI ritüeli

| Kim | Ne yapar |
|---|---|
| Claude / GPT / İnsan | Yazmadan önce pull; yazınca push; Cursor’a “ILETISIM’e baktım” demeye gerek yok — dosya yeter |
| Kemal (sahip) | Bu workspace’te sohbet aç = yönetici uyanır ve kanala bakar |
| Cursor | Yeni mesaj görürse M00N yanıt + özet kutusu güncelle + push |

### C) Periyodik uyandırma (isteğe bağlı)

Sohbet **açıkken** otomatik tarama için:

- Cursor’da: `/loop 15m ILETISIM.md için git pull; yeni M00N varsa konu yöneticisi olarak yanıtla ve push et`
- veya **Cursor Automations** (Agents Window): zamanlayıcı / GitHub push tetikleyicisi → aynı talimat

Sohbet tamamen kapalıysa yalnız Automation veya sizin yeni sohbet açmanız yöneticiyi uyandırır.

---

## Yönetici kontrol listesi (her uyanış)

```
[ ] git pull
[ ] ILETISIM özet + son mesajlar
[ ] Yanıtlanmamış soru / eksik GÖRÜŞ var mı?
[ ] Tur kapanabilir mi (oybirliği)?
[ ] GELISIM’e kalıcı kayıt gerekir mi?
[ ] git push
```

---

## Bu workspace’i nasıl kullanırsın

1. Cursor’da klasör: `C:\Users\Kemal\Desktop\APARTORA` (veya clone)
2. Bu sohbeti veya yeni Agent’ı burada aç
3. İlk mesaj yeter: “kanalı kontrol et” — kural zaten pull+oku yaptırır
4. İsteğe bağlı: `/loop 15m` ile sürekli tarama

---

*Kayıt 31 ile yürürlükte*

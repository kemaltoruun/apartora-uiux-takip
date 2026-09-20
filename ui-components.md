# UI Bileşen Seçimi — `src/components/**`, `src/app/**`

> **Koddan türetildi** (ölçüm 2026-09-11). Sayılar, modülü **import eden üretim dosyası**
> sayısıdır (test ve story hariç); JSX kullanımı ölçülen yerde ayrıca belirtilir.
> Bir kararı değiştirmeden önce sayıyı yeniden ölç.
> Mandal: `src/components/ui/__tests__/ui-component-choice.guard.test.ts` — aşağıdaki
> **KULLANMA** satırlarından kodla kanıtlanan üçünü iki yönlü borç defteriyle kilitler.

**Etiketler:** **KANONİK** = yeni kodda bunu kullan · **KABUL** = mevcut kullanım yerinde,
yeni kodda gerekçeyle · **KULLANMA** = yeni bağlantı yasak · **ÖLÜ** = hiç import edilmiyor

Web ile mobil **ayrı kütüphanedir**; paylaşılan şey kontrattır, JSX değil (bkz. §8).

---

## 1. Diyalog ve katmanlar

| İhtiyaç | Kullan | Etiket | Sayı | Not |
|---|---|---|---|---|
| Form / düzenleme | `ResponsiveDialog*` — `@/components/ui/responsive-dialog` | KANONİK | 337 | Mobilde Vaul drawer (kaydırarak kapanır), masaüstünde Radix Dialog. API `Dialog` ile birebir → geçiş import değişimidir |
| Uzun form + sabit Kaydet | `DialogScrollContainer.Body` / `.Footer` | KANONİK | 56 | Mobilde footer `sticky`, klavye ve safe-area uyumlu |
| Silme onayı | `ConfirmDeleteDialog` — `@/components/common/ConfirmDeleteDialog` | KANONİK | 50 | AlertDialog + haptic + `isLoading` kilidi; `children` ile sebep alanı |
| Yıkıcı olmayan onay | `AlertDialog*` | KANONİK | 77 | |
| Tablo satırı → zengin detay (okuma) | `DetailBottomSheet` | KANONİK | 24 | Mobilde alt sayfa, masaüstünde yan diyalog. Düzenleme formu için DEĞİL |
| Yan panel (filtre, ayar) | `Sheet` | KABUL | 20 | |
| Masaüstü ağırlıklı diyalog | `Dialog` | KABUL | 65 | Mobilde drawer olmaz, tam ekrana yakın açılır. Yeni formda `ResponsiveDialog` |
| Hazır kabuk | `ResponsiveStickyDialogShell` | KABUL | 1 | Yayılmamış; örnek `superadmin/edit-user-dialog.tsx` |
| Doğrudan drawer | `Drawer` | KABUL | 3 | `ResponsiveDialog` zaten içeride kullanır; yalnız çok snap noktalı özel durum |
| — | `Modal`, `ConfirmDialog` — `@/components/ui/modal` | **KULLANMA** | 5 (borç) | Radix değil, elle `div`: odak hapsi yok, portal yok, mobilde drawer'a dönmez. `ConfirmDialog` açıklamayı **iki kez** basar (başlık altında + gövdede) |

**Mobil kaydırma tuzağı:** `noScrollWrap` + öneksiz `overflow-hidden` + `DialogScrollContainer`
birleşimi gövdeyi mobilde kaydırılamaz yapar. Mandal: `modal-mobile-scroll.guard`.

**Birleştirme adayı (mandal yok):** `ConfirmDeleteDialog` varken 4 yerel `DeleteConfirmDialog` tanımı var —
`dashboard/superadmin/logs/SuperAdminLogsDialogs.tsx`, `manager/dues/components/DuesTableDialogs.tsx`,
`manager/site-management/dialogs/DeleteConfirmDialog.tsx`, `manager/inventory/dialogs/DeleteConfirmDialog.tsx`.

---

## 2. Geçici bildirim (toast)

| Kullan | Etiket | Sayı | Not |
|---|---|---|---|
| `toast` — `@/lib/toast/sonner-adapter` | KANONİK | 639 | Her çağrı `toast-manager`dan geçer: aynı mesaj 3 sn içinde tekrar gösterilmez, hız sınırı uygulanır, başarı/hata/uyarı haptic'i verilir. Üyeler: `success` `error` `warning` `info` `loading` `promise` `custom` `message` `dismiss` `update` |
| `useToastNotification()` — `@/hooks/use-toast-notification` | KABUL | 6 | Aynı adapter; tipli `TOAST_MESSAGES` sabitleriyle |
| `from 'sonner'` (çalışma anı) | **KULLANMA** | 16 (borç) | Tekrar ve hız korumasını, haptic'i atlar. Altyapı dosyaları hariç: `ui/sonner`, `ui/toaster`, `lib/toast-manager`, adapter. Borçtaki dosyaların hepsi yalnız `success/error/warning/info` çağırıyor → borç ödemesi = import değişimi |
| `@/hooks/use-toast`, `@/components/ui/use-toast` | **KULLANMA** | 12 (borç) | Kaynakta `@deprecated` |
| `@/components/ui/toast` (Radix Toast) | **KULLANMA** · ÖLÜ | 0 | İkinci bir toast sistemi; `Toaster` sonner tabanlı |

Toast geçicidir. Sayfada **kalması** gereken uyarı → `Alert` (142). Kullanıcıdan **karar** → `AlertDialog`.

---

## 3. Durum ekranları

| Durum | Kullan | Etiket | Sayı | Not |
|---|---|---|---|---|
| Yükleniyor (sayfa/kart) | `Skeleton` — `@/components/ui/skeleton` | KANONİK | 190 | |
| Yükleniyor (satır/buton içi) | `InlineLoading` — `@/components/feedback` | KANONİK | 7 JSX | |
| Boş liste | `EmptyStateCard` — `@/components/feedback` | KANONİK | 98 JSX | Arama/filtre sonucu boşsa `SearchEmptyState` (4) / `FilterEmptyState` (1) |
| Boş (zengin, onboarding) | `EmptyState` — `@/components/ui/empty-state` | KABUL | 6 | |
| Tablo boş | `DataTable`'ın kendisi | KANONİK | — | Masaüstünde `table.noResults`, mobil kartta `table.mobileCard.noData` basar; ayrı bileşen gerekmez |
| Sorgu hatası (`useAuthorizedQuery`) | `QueryErrorHandler` — `@/components/feedback` | KANONİK | 22 JSX | `errorReason`'a göre kart seçer: `forbidden` → `PermissionRequiredCard` / `PermissionDeniedCard` · `feature_disabled`, `no_credits` → `FeatureDisabledCard` · `not_found` → `ResourceNotFoundCard` · `rate_limited` → `RateLimitedCard` · `server_error`, `network_error`, `validation` → `GenericErrorCard` |
| Genel hata (500, ağ) | `GenericErrorCard` | KANONİK | 32 JSX | `onRetry` ile tekrar dene |
| Bölüm hatası (ekranın bir kısmı) | `InlineQueryError` — `@/components/feedback` | KANONİK | 6 JSX | Ekranın geri kalanı kullanılabilirken tek bir bölümün sorgusu çöktüğünde: kısa mesaj + `onRetry`. Boş durumdan ÖNCE dallanır (`yükleniyor ? … : hata ? … : boş ? …`). Mobil karşılığı aynı ad ve sözleşme (`ui/query-error-state`). İlk tüketici banka eşleştirme diyaloğu (2026-09-11) |

`TableEmptyState` adıyla **iki ayrı** bileşen var (`components/feedback/empty-states.tsx` ve
`lib/table/components/TableEmptyState.tsx`) — importu yolundan seç.

**Hata veri gibi çizilmesin:** TanStack `isError` dalını yaz; `?? []` / `?? 0` başarısızlığı gerçek
veri gibi gösterir. Web mandalı `error-swallowed-as-data.guard` yalnız `queryFn` **içinde** yutulan
hatayı yakalar. Tüketicinin `isError` dalını arayan `query-error-branch.guard` artık **web'de de var**
(2026-09-12; `src/lib/__tests__`, çift yönlü defter, 232 kayıt donduruldu — defterde para ekranı YOK).
İkisi aynı sınıfın iki katmanıdır: üretici hatayı sinyal olarak bırakmalı, tüketici o sinyali çizmeli.

---

## 4. Tablo ve liste

| İhtiyaç | Kullan | Etiket | Sayı | Not |
|---|---|---|---|---|
| Veri tablosu | `DataTable` — `@/components/ui/data-table` | KANONİK | 168 | Mobilde otomatik kart görünümü (`useIsMobile`); `renderMobileCard` ile özelleştir, `disableMobileCards` yalnız gerekçeyle |
| Satır eylemleri | `RowActionsMenu` | KANONİK | 118 | Yetki (`hidden`), yıkıcı onay ve i18n tek yerde. Ham `DropdownMenu` + `MoreHorizontal` yazma |
| Sütun yardımcıları | `@/lib/table` (`getActionsColumn`, `getCurrencyColumn`, `useDataTable` …) | KABUL | 26 | |
| Basit statik tablo | `Table*` — `@/components/ui/table` | KABUL | 62 | `ui/table/` dizini bu dosyanın iç uygulamasıdır, doğrudan import etme |
| — | `ResponsiveTable` | ÖLÜ | 0 | |

---

## 5. Durum rozeti

| Kullan | Etiket | Sayı | Not |
|---|---|---|---|
| `StatusBadge` — `@/components/ui/status-badge` | KANONİK | 7 | `variant`: `dues` · `payment` · `user` · `generic` |
| `Badge` + varyant | KABUL | 835 | Durum dışı etiket. Varyantlar: `default` `secondary` `destructive` `success` `warning` `danger` `info` `outline` |

**Birleştirme adayı:** 3 yerel `StatusBadge` tanımı — `app/superadmin/balance-management/_components/format-helpers.tsx`,
`app/superadmin/crm/_components/CrmBadges.tsx`, `components/security-portal/common/StatusBadge.tsx`.
Durum değerleri DB CHECK kısıtından gelir; eşlemede eksik kalan durum satırı sessizce düşürebilir
(hafıza `reference_dues_status_map_silent_drop`).

**ÖLÜ:** `components/manager/dues/shared/StatusBadge.tsx` hiçbir yerden import edilmiyor (ölçüm 2026-09-11) ve
`pending_approval`'ı tanımıyor. Aidat ay detayı onu bilerek kullanmıyor: etiketleri Türkçe sabit, sayfa i18n'li;
yerine `@apartora/shared → duesGorunumDurumu` ile kendi rozetini çiziyor. Aidat rozeti gerekirse ondan başla.

---

## 6. Temel parçalar

- **Button** (1184): `variant` default · destructive · outline · secondary · ghost · link — `size` default · sm · lg · icon (**44 px dokunma hedefi**). `ui-custom/AnimatedButton` yalnız landing.
- **Card** (806) · **Input** (499) · **Select** (402) · **Textarea** (240) · **Checkbox** (142) · **Switch** (114).
- **Metin kutusu = `Input` / `Textarea`, ham `<input value onChange>` DEĞİL** (2026-09-18): ikisi JS yüklenmeden
  yazılan değeri state'e devralır (`@/hooks/use-pre-hydration-value`). React 19 hydration'da DOM değerine dokunmaz ve
  kaçan `onChange`'i yeniden oynatmaz → ham controlled kutuda state `""` kalır, bağlı buton kilitlenir ya da ilk yeniden
  çizim yazılanı siler (canlı vaka: `/odeme` "Borçları Görüntüle" hiç aktifleşmedi). İstemci tarafı `render` testleri
  bunu göremez — hydration testi `renderToString` + `hydrateRoot` ister. Mandal `ham-controlled-metin-kutusu.guard`
  (iki yönlü defter, 14 dosya / 19 kutu).
- **Form** (85): `Form` + `formResolver` + `preprocessFormSchema` → `validation-rules.md`. Özel girişler: `PhoneInput` (37) · `PasswordInput` (9) · `SearchableCombobox` (11) · `DatePicker` (8) / `Calendar` (34).
- **Sekmeler**: `Tabs` (147); taşan sekme şeridi `ResponsiveTabs` (70) / `ScrollableTabsList` (28). Sayfa sekmesi URL'e bağlanır (`useTabWithUrl`) — mandal `tab-url-binding.guard`.
- **Yetki kapısı**: yazma eylemi `<Can deny="disable">` (mandal `can-write-action-deny.guard`) · sayfa `ui/page-guard` (25).
- **Sayfa başlığı**: `PageHeader` yalnız 8 dosyada — ortak başlık deseni **yerleşmemiş**; ölçüldü, karar verilmedi.

---

## 7. Token'lar

- **Renk yalnız semantik** (`tailwind.config.ts`; değerler `src/styles/globals.css` → `:root` + `.dark`):
  `background` / `foreground` · `card` · `popover` · `primary` · `secondary` · `muted` · `accent` · `destructive` ·
  `border` · `input` · `ring` · `sidebar-*`. Durum renkleri `success` · `warning` · `error` · `info`, her biri
  `-foreground` / `-light` / `-border` ile. Ham `bg-white` / `text-black` yok (`coding-patterns.md` §7).
- **Opaklık** yalnız 5'in katları (`bg-primary/50`); ölçek dışı değer utility üretmez — mandal `tailwind-opacity-scale.guard`.
- **Font**: `font-sans` (Plus Jakarta Sans) · `font-heading` (DM Sans) · `font-arabic` (RTL).
- **Radius**: `rounded-lg` = `--radius` (0.5rem), `md` / `sm` ondan türer. **Ek breakpoint**: `xs` = 480px.

---

## 8. Mobil uygulama karşılıkları (`apps/mobile`, NativeWind)

| İhtiyaç | Web kanonik | Mobil kanonik | Mobil sayı |
|---|---|---|---|
| Form / detay modalı | `ResponsiveDialog` | `SheetScaffold` (tam ekran iskelet) · `AppModal` (ham RN `Modal` yerine tek giriş noktası) | 87 · 96 |
| Alt sayfa | `DetailBottomSheet` | `AppBottomSheet` | 6 |
| Aksiyon menüsü | `RowActionsMenu` | `ActionSheet` | 16 |
| Onay / uyarı | `ConfirmDeleteDialog` · `AlertDialog` | `useConfirm` / `useAlert` (`confirm-dialog`) | 105 |
| Toast | `sonner-adapter` | `useToast` (`ui/toast`) | 162 |
| Boş | `EmptyStateCard` | `EmptyState` | 189 |
| Hata | `QueryErrorHandler` · `GenericErrorCard` · bölüm: `InlineQueryError` | `QueryErrorState` / `ForbiddenState` · bölüm: `InlineQueryError` | 146 |
| Yükleniyor | `Skeleton` | `ScreenState` · `Skeleton` | 81 · 62 |
| Liste | `DataTable` | `QueryListView` (FlatList sanallaştırma) | 108 |
| Başlık | `PageHeader` (8) | `ScreenHeader` (native stack başlığı) | 128 |
| Form kaydet çubuğu | `DialogScrollContainer.Footer` | `StickySaveBar` | 5 |

Mobil kütüphane bu konuda web'den disiplinli: toast tek kaynaktan, `Alert.alert` **0**. Web'deki dağınıklık
(iki borçlu toast yolu, 4 yerel onay diyaloğu, 4 yerel `StatusBadge`) web'e özgüdür.

---

## 9. Hiç import edilmeyen `ui` modülleri (2026-09-11)

`aspect-ratio` · `carousel` · `color-picker` · `context-switcher` · `data-table-row-expand.index` ·
`enhanced-sidebar` · `file-upload` · `hover-card` · `input-otp` · `lazy-image` · `menubar` · `navigation-menu` ·
`pagination` · `permission-guard` · `resizable` · `responsive-container` · `responsive-form` · `responsive-table` ·
`signature-pad` · `tag-input` · `test-mode-indicator` · `toast` · `use-toast` · `virtual-list`

Bir kısmı hazır shadcn parçasıdır. Bağlanmadan önce §1–6'daki kanonik karşılığa bak. **Silme ayrı iştir, onay ister.**

---

## 10. Borç ödeme ve güncelleme

- Borç ödendiğinde mandaldaki defterden **dosyayı sil** — defter iki yönlüdür, ödenmiş borcu bırakmak da kırmızı verir.
- Yeni bir **KULLANMA** kararı mandala ancak gerekçesi koddan kanıtlanınca girer (bu dosyadaki gibi); zevk tercihi mandal olmaz.
- Sayılar değişince bu dosyanın başındaki ölçüm tarihini de güncelle.

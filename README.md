# INADINA TV — Canlı Kanal Oynatıcı

Tarayıcı tabanlı, TV kumandasıyla uyumlu bir canlı yayın oynatıcı. Kanallar ve
kategoriler artık **harici `channels.json`** dosyasından yönetilir; player HLS
(hls.js), MPEG-TS (mpegts.js) ve doğal (native) oynatmayı destekler.

> ⚠️ **Yasal not:** Bu proje yalnızca **resmi/ücretsiz ve yetkili** yayın
> kaynakları (ör. TRT'nin şifresiz kamu yayınları) veya kendi lisanslı
> akışlarınız için tasarlanmıştır. Telifli içeriği izinsiz dağıtan korsan
> sitelerin taranması, domain takibi veya link çıkarımı bu proje kapsamında
> **desteklenmez**.

## Özellikler

- **JSON odaklı içerik** — kanallar, kategoriler ve ayarlar `channels.json`'da.
- **İki katmanlı kategori (yelpaze)** — üst sekmeler (Spor, Haber, Ulusal,
  Çocuk, Müzik, Belgesel…) + her sekme içinde alt gruplar.
- **Gelişmiş player** — kalite seçici (HLS seviyeleri), oynatma hızı
  (0.5x–2x), ses/dil seçimi, PiP, tam ekran, canlıya dön, otomatik yeniden
  bağlanma ve çok-stratejili format denemesi (HLS ↔ TS ↔ MP4 ↔ native).
- **Proxy & User-Agent desteği** — yetkili kaynaklar için isteğe bağlı proxy
  yönlendirmesi ve özel User-Agent/Referer başlıkları (üst çubuktaki
  "Bağlantı Ayarları" menüsünden veya kanal bazında JSON'dan).
- **M3U kaynak birleştirme** — JSON'daki kanalların yanına harici `.m3u`
  listelerini de içe aktarabilir.
- **TV kumanda uyumu**, tema seçenekleri, arama ve kart/liste düzenleri.

## `channels.json` yapısı

```jsonc
{
  "version": 3,
  "settings": {
    "defaultProxyUrl": "https://.../proxy?url=",   // opsiyonel proxy
    "defaultUserAgent": "Mozilla/5.0 ...",          // varsayılan UA
    "userAgentPresets": { "Chrome (Android)": "..." },
    "m3uSources": [                                  // opsiyonel M3U listeleri
      { "name": "...", "url": "https://.../liste.m3u", "useProxy": true, "mainCategory": "belgesel" }
    ]
  },
  "mainCategories": [
    { "id": "spor", "label": "Spor", "icon": "futbol" }
  ],
  "channels": [
    {
      "name": "TRT Spor HD",
      "logo": "https://.../logo.png",
      "url": "https://tv-trtspor1.medya.trt.com.tr/master.m3u8",
      "type": "hls",              // hls | ts | mp4 | auto
      "mainCategory": "spor",     // üst sekme id'si
      "group": "TRT Spor",        // alt kategori (chip)
      "legal": true,              // "RESMİ" rozeti gösterir
      "useProxy": false,          // bu kanal proxy üzerinden mi geçsin
      "userAgent": "",            // (ops.) kanala özel User-Agent
      "referer": ""               // (ops.) kanala özel Referer
    }
  ]
}
```

### Yeni kanal eklemek

`channels.json` içindeki `channels` dizisine bir nesne ekleyin. `mainCategory`
mevcut bir üst kategorinin `id`'si olmalı; yeni bir üst kategori istiyorsanız
`mainCategories`'e ekleyin (`icon` bir Font Awesome ikon adıdır, ör. `futbol`,
`newspaper`, `tv`).

### Proxy / User-Agent

Bazı yetkili kaynaklar özel `User-Agent` veya `Referer` bekleyebilir.
Tarayıcılar bu başlıkları doğrudan `fetch`/XHR ile çoğu zaman engeller; bu
yüzden istekler yapılandırılabilir bir proxy üzerinden yönlendirilebilir.
Proxy, iletilen `ua`, `referer`, `origin` sorgu parametrelerini gerçek isteğe
uygular. Üst çubuktaki **Bağlantı Ayarları** menüsünden global proxy zorlaması,
proxy URL'si ve User-Agent seçilebilir (tercihler `localStorage`'a kaydedilir).

## Çalıştırma

Statik dosyalardan ibarettir; herhangi bir HTTP sunucusuyla servis edin:

```bash
python3 -m http.server 8000
# http://localhost:8000
```

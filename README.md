# Nuvio Türkçe Eklentileri

CVNFLIX Streams sağlayıcılarının Nuvio'ya uyarlanmış hali. Eklentiler kullanıcının cihazında çalışır; sunucu ve proxy yoktur.

## Kurulum

Nuvio'da **Ayarlar > Eklentiler > Depo Ekle** bölümüne şu adresi girin:

```
https://raw.githubusercontent.com/onrcvndev/nuvio-plugin-tr/main/manifest.json
```

Yerel deneme için `npm start` çalıştırıp uygulamanın geliştirici sürümündeki **Plugin Tester**'a `http://<bilgisayar-ip>:3000/manifest.json` adresini girin.

## Geliştirme

```bash
npm install
npm run build        # src/<id>/index.js + manifest.json üretir, küçültülmüş paketleri derler
npm run build:dev    # küçültmesiz, hata ayıklama için
npm test             # URL ve crypto uyum katmanı testleri (çevrimdışı)
npm run test:live    # eklentileri QuickJS içinde canlı sitelere karşı dener
node tools/live-test.js dizipal ddizi   # yalnız seçilen eklentiler
```

`IMPIT_PATH=<impit modülü>` verilirse canlı test Chrome TLS parmak iziyle çıkar. `DEBUG_LOGS=1` eklenti loglarını yazdırır.

Dosya düzeni:

| Yol | İçerik |
| --- | --- |
| `providers.config.js` | Eklenti listesinin tek kaynağı |
| `src/<id>/index.js` | Üretilen giriş dosyaları, elle düzenlenmez |
| `src/_core/providers`, `resolvers` | Stremio eklentisinden taşınan kazıyıcılar ve oynatıcı çözücüleri |
| `src/_core/lib/http.js`, `lib/proxy.js`, `compat/` | Çalışma zamanı uyum katmanı |
| `src/_core/plugin.js`, `services/tmdb.js` | TMDB kimliğinden akış listesine giden `getStreams` akışı |
| `providers/` | Derlenmiş eklentiler, uygulama bunları indirir |

Yeni eklenti için sağlayıcıyı `src/_core/providers/` altına ekleyin, kimlik önekini `lib/ids.js` içindeki `WIO_ID_SPECS` tablosuna ve kaydı `providers.config.js` dosyasına yazın, sonra `npm run build` çalıştırın.

## Çalışma zamanı notları

[Resmi doküman](https://github.com/yoruix/nuvio-providers/blob/template/DOCUMENTATION.md) Hermes'i anlatır; güncel Nuvio (`NuvioMedia/NuvioMobile`, `cmp-rewrite`) QuickJS kullanır. Kod iki ortama da uygun derlenir. QuickJS'te dikkat edilen kısıtlar:

- Manifest üst seviyede `{ name, version, scrapers: [] }` olmalıdır; düz dizi kabul edilmez.
- Eklenti 60 saniye sonra kesilir; `getStreams` 50 saniyede o ana kadar biten sonuçları döner.
- Host `fetch`'i `AbortSignal`'i yok sayar, ağ hatasını `status: 0` ile bildirir ve gövdeyi belleğe alır.
- Host `URL`'si geçersiz adreste hata fırlatmaz ve `searchParams` değişikliğini `href`'e yansıtmaz; bu yüzden `URL`, `cheerio`, `crypto-js` ve `Buffer` pakete gömülüdür.
- Node ortamında `globalThis.crypto` bulunduğu için bazı hatalar yalnız QuickJS'te görünür; değişikliklerden sonra canlı testi çalıştırın.

## Eklenmeyen kaynaklar

| Kaynak | Neden |
| --- | --- |
| YabancıDizi | `/ajax/service` ucu her istemciye 520 döndürüyor |
| SezonlukDizi, AnimeciX, TRanimaci | reCAPTCHA/Turnstile doğrulaması gerekiyor |
| Anizium | Hesap girişi zorunlu |
| RecTV | Uygulamaya gömülü anahtar gerekiyor |
| DiziMom | DDizi ile aynı platform |
| DramaDizilerim, DonghuaWorld | Kaldırıldı: içerikleri TMDB başlıklarıyla çoğunlukla eşleşmiyor, sonuç bulamadan uzun süre bekletiyor |
| SelçukSports, ZirveTV, BeyazElma | Canlı yayınlar TMDB tabanlı `getStreams` modeline uymuyor |

## Lisans

GPL-3.0. Derleme betiği ve yerel sunucu [nuvio-providers](https://github.com/yoruix/nuvio-providers/tree/template) şablonundan uyarlanmıştır.

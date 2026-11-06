# Nuvio Türkçe Eklentileri

CVNFLIX Streams sağlayıcılarının Nuvio'ya uyarlanmış hali. Eklentiler kullanıcının cihazında çalışır; sunucu ve proxy yoktur.

## Destek

Eklentileri beğendiyseniz [Kreosus üzerinden destek olabilirsiniz](https://kreosus.com/onrcvndev?utm_source=kreosus_embed&utm_medium=readme&utm_campaign=onrcvndev).

Destek bağlantısı 3 Kasım 2026 12:15'e (TSİ) kadar akış listelerinin sonunda da görünür; kalan süre satırda yazar ve süre dolunca kendiliğinden kalkar. Nuvio eklenti sonucunda dış bağlantı açamadığı için satıra basmak oynatıcıyı açar ve video başlamaz; adresi tarayıcıya yazarak açın.

## Kurulum

Nuvio'da **Ayarlar > Eklentiler > Depo Ekle** bölümüne şu adresi girin:

```
https://raw.githubusercontent.com/onrcvndev/nuvio-plugin-tr/main/manifest.json
```

## Çalışma zamanı notları

[Resmi doküman](https://github.com/yoruix/nuvio-providers/blob/template/DOCUMENTATION.md) Hermes'i anlatır; güncel Nuvio (`NuvioMedia/NuvioMobile`, `cmp-rewrite`) QuickJS kullanır. Kod iki ortama da uygun derlenir. QuickJS'te dikkat edilen kısıtlar:

- Manifest üst seviyede `{ name, version, scrapers: [] }` olmalıdır; düz dizi kabul edilmez.
- Eklenti 60 saniye sonra kesilir; `getStreams` 50 saniyede o ana kadar biten sonuçları döner.
- Host `fetch`'i `AbortSignal`'i yok sayar, ağ hatasını `status: 0` ile bildirir ve gövdeyi belleğe alır.
- Host `URL`'si geçersiz adreste hata fırlatmaz ve `searchParams` değişikliğini `href`'e yansıtmaz; bu yüzden `URL`, `cheerio`, `crypto-js` ve `Buffer` pakete gömülüdür.

## Lisans

GPL-3.0. Derleme betiği ve yerel sunucu [nuvio-providers](https://github.com/yoruix/nuvio-providers/tree/template) şablonundan uyarlanmıştır.

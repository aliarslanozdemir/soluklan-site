# soluklan.app

Sitenin kaynağı. Düz HTML, yapı adımı yok — GitHub Pages klasörü olduğu gibi yayınlıyor.

**`site/` altındaki HTML dosyalarının hiçbiri elle düzenlenmez.** Hepsi üretiliyor:

```
content/anasayfa.html      → site/index.html
content/makaleler/*.md     → site/rehber/*.html
                           → site/rehber/konu/*.html (kategori sayfaları)
                           → site/rehber/index.html (kategori başına tek satır)
content/sss.md             → site/rehber/sik-sorulanlar.html
docs/gizlilik.md           → site/gizlilik.html
docs/destek.md             → site/destek.html
                           → site/sitemap.xml
                           → build/site-onizleme.html (tek dosya, paylaşmak için)

site/assets/soluklan.css   ← ortak tasarım katmanı (elle yazılır)
site/assets/favicon.svg    ← logo işareti, **simgelerin tek kaynağı**
site/CNAME                 ← soluklan.app
```

`scripts/site_build.py` ayrıca şunları üretiyor: `site/robots.txt`, `site/site.webmanifest`,
`site/llms.txt` ve `site/.nojekyll`.

Simgeler ayrı bir betikle, `favicon.svg`den üretiliyor — logo değişirse tek komut yeter:

```
python3 scripts/site_ikonlar.py   → favicon.ico (kökte), favicon-16/32/48.png,
                                    icon-180/192/512.png, assets/og.png
```

Üretmek için:

```
python3 scripts/site_build.py
```

Üst ve alt bölüm (logo, menü, alt bilgi) tek yerde, `scripts/site_build.py` içinde. Menüye bir
şey eklenince bütün sayfalara birden geliyor.

## Makale yazmak

`content/makaleler/` altına markdown dosyası koy. Başında künye bloğu olmalı:

```
---
baslik: Koherent Nefes
ozet: Bir cümlelik özet, dizin sayfasında ve arama sonuçlarında görünür.
kategori: Teknikler
sira: 1
okuma: 6
---
```

`kategori` üçünden biri: **Nasıl kullanılır**, **Teknikler**, **Perde arkası**. `sira` kategori içindeki
sırayı belirliyor. Dosya adı adresi belirliyor (`koherent-nefes.md` → `/rehber/koherent-nefes.html`).

Sık sorulan sorular `content/sss.md` içinde. Her soru `### ` ile başlıyor, altında cevap
paragrafları ve isteğe bağlı olarak tek satırlık bir bağlantı: `[Ayrıntısı: Başlık](/rehber/slug.html)`.

Gövde markdown: `##` ve `###` başlıklar, `- ` listeler, `**kalın**`, `[bağlantı](adres)`.
Aynı kategorideki iki makale yazının altında kendiliğinden öneriliyor.

**Metinler uygulamadan ya da mağaza açıklamasından kopyalanmıyor.** Site metni ayrı yazılıyor:
daha uzun, kaynaklı ve kanıtın nerede zayıf olduğunu açıkça söyleyen bir dille.

## Gizlilik ve destek metinleri

Kaynak `docs/gizlilik.md` ve `docs/destek.md`. Sebep: aynı metin hem uygulamanın içinde hem sitede geçiyor. İki yerde ayrı yazılırsa er geç
ayrışır ve App Store'a verdiğimiz gizlilik metni uygulamadakinden farklı olur. Metni değiştirmek
için `docs/` altındaki markdown dosyasını düzenle, sonra betiği çalıştır.

Aynı betik `build/site-onizleme.html` de üretiyor: üç sayfa tek dosyada, CSS gömülü — paylaşmak
ve göz atmak için.

## Denetim

```
python3 scripts/site_denet.py
```

Bağlantıları, simge dosyalarını, `canonical`/`og:image`/hreflang etiketlerini, yapısal
verinin geçerli JSON olduğunu ve site haritasının sayfalarla örtüştüğünü sınar. Bunların
hiçbiri gözle görülmüyor: `canonical` düşse ya da `/favicon.ico` 404 verse sayfa normal
açılıyor, hata yalnız arama sonucunda haftalar sonra çıkıyor.

## Yayına alma

Site şu an `aliarslanozdemir.github.io/soluklan-site` deposunda duruyor. Yeni alan adı için:

1. Bu klasörün içeriği o depoya kopyalanır (`CNAME` dosyası dahil).
2. GitHub → Settings → Pages → Custom domain: `soluklan.app`, "Enforce HTTPS" açılır.
3. Alan adının DNS'inde dört A kaydı (`185.199.108–111.153`) ve `www` için CNAME
   (`aliarslanozdemir.github.io`) tanımlanır.

**Dosya adları değişmemeli.** App Store Connect'te gizlilik ve destek adresleri
`.../gizlilik.html` ve `.../destek.html` olarak kayıtlı. Aynı adlar korunursa eski GitHub Pages
adresi yeni alan adına kendiliğinden yönleniyor, inceleme sırasında bağlantı kırılmıyor. Alan adı
çalışmaya başlayınca ASC'deki üç adres (gizlilik, destek, pazarlama) `soluklan.app`'e çevrilir.

## Tasarım

Renkler ve yazı ailesi uygulamanın kendi paletinden geliyor (`Solunum/App/Theme.swift`). Açık ve
koyu tema, `prefers-color-scheme` ile kendiliğinden. Dışarıdan font ya da betik indirilmiyor: sayfa
tek istekte açılıyor, izleme yok — gizlilik metninde "üçüncü taraf yok" derken site de öyle olmalı.

Giriş bölümündeki halka **5,5 saniyede bir** nefes alıp veriyor: uygulamanın Koherent ritmi.
`prefers-reduced-motion` açıksa duruyor.

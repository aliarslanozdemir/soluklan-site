# Mağaza rozetleri

Apple'ın ve Google'ın resmî rozetleri, olduğu gibi. Üzerlerinde renk, yazı ya da boy oranı
değişikliği yapılmaz; yenisi çıkarsa aynı yerden yeniden indirilir.

| Dosya | Kaynak |
|---|---|
| `app-store-<dil>.svg` | Apple Marketing Tools, siyah "Download on the App Store" rozeti: `https://toolbox.marketingtools.apple.com/api/v2/badges/download-on-the-app-store/black/<yerel>` — yereller `tr-tr`, `en-us`, `de-de`, `fr-fr`, `es-es`, `it-it`, `ar-ar`, `zh-cn`, `ru-ru`, `ja-jp` (10 Ekim 2026; sitede ve uygulamada). Portekizce ve Endonezce rozet indirilmedi: o dillerde sayfa İngilizce rozeti İngilizce alt metinle gösteriyor (`site_build.py` → `rozet_dosyasi`), `app-store-pt.svg` / `app-store-id.svg` konursa kendiliğinden kullanılır |
| `google-play-<dil>.svg` | Google Partner Marketing Hub → Google Play → Lockups, icons & badges → Get it on Google Play Badges → Digital → svg: `GetItOnGooglePlay_Badge_Web_color_<Dil>.svg` (Turkish, English, German, French, Spanish, Italian, Arabic-Saudi-Arabia, Chinese-China, Russian) |

İndirme tarihi: 8 Ekim 2026. İki markanın kuralları:

- Apple: <https://developer.apple.com/app-store/marketing/guidelines/> — ekranda en az 40 px boy,
  çevresinde rozet boyunun dörtte biri kadar boşluk, başka rozetlerle birlikteyken siyah rozet,
  rozetin olduğu yerde Apple marka cümlesi.
- Google: <https://partnermarketinghub.withgoogle.com/brands/google-play/google-play/lockups-icons-badges/#badges>
  — en az 28 px boy, çevresinde boyunun dörtte biri kadar boşluk, öteki rozetlerden küçük değil,
  sayfada "Google Play and the Google Play logo are trademarks of Google LLC." cümlesi.

Google Play rozeti `scripts/site_build.py`deki `ANDROID_YAYINDA` açılınca sayfaya giriyor.

Uygulamadaki paylaşma kartı da bu çizimleri kullanıyor (10 Ekim 2026): `rsvg-convert -h 120` ile
120 piksel boyda PNG'ye çevrildi, iOS'ta `Solunum/Resources/Assets.xcassets/Rozetler/` (3x),
Android'de `android/app/src/main/res/drawable-nodpi/rozet_*.png`. Rozet değişirse bunlar da
yeniden üretilir. Kartta Google Play rozeti `PaylasimKarti.androidYayinda` (iOS) /
`PaylasimKarti.ANDROID_YAYINDA` (Android) açılınca çıkıyor.

# SHADOW FIST — iPhone ve Android Paketi

Bu klasör, oyunu hem **yerel uygulama** (App Store / Google Play / APK) hem de
**kurulabilir web uygulaması (PWA)** olarak dağıtmak için hazırdır.

```
shadow-fist/
├── www/                  ← oyunun kendisi (index.html + ikonlar + manifest + sw.js)
├── android/              ← Android Studio projesi (Capacitor 8)
├── ios/                  ← Xcode projesi (Capacitor 8, Swift Package Manager)
├── assets/               ← kaynak ikon ve açılış ekranı görselleri
├── capacitor.config.json ← uygulama kimliği: com.shadowfist.game
└── .github/workflows/build.yml ← otomatik APK derleme + web yayını
```

Uygulamaya eklenenler: yatay ekran kilidi, tam ekran (durum/navigasyon çubuğu gizli),
oyun sırasında ekranın kapanmaması, özel ikonlar ve açılış ekranı.

---

## En kolay yol: GitHub ile otomatik (bilgisayara bir şey kurmadan)

1. GitHub'da yeni bir depo aç (ör. `shadow-fist`) ve bu klasörün içeriğini yükle.
2. Depo → **Settings → Pages → Source: "GitHub Actions"** seç.
3. **Actions** sekmesinde iş akışı kendiliğinden çalışır (~5 dk). Sonunda:
   - **Android:** iş akışı sayfasının altındaki `shadow-fist-android` dosyasını indir,
     içindeki `app-debug.apk`'yı telefona at ve kur
     (telefonda "bilinmeyen kaynaklardan yükleme" iznini vermen gerekir).
   - **iPhone + Android (web uygulaması):** oyun `https://KULLANICI.github.io/shadow-fist/`
     adresinde yayınlanır.
     - iPhone: Safari ile aç → Paylaş → **Ana Ekrana Ekle**
     - Android: Chrome ile aç → ⋮ menü → **Uygulamayı yükle**
     Ana ekrandan açınca tam ekran çalışır, ilk açılıştan sonra internetsiz de oynanır.

> Apple, Mac ve geliştirici hesabı olmadan iPhone'a yerel uygulama kurulmasına izin vermiyor.
> Bu yüzden iPhone'da en pratik yol "Ana Ekrana Ekle".

---

## Elle derleme

Önce bir kez: `npm install`
Oyunda (`www/index.html`) değişiklik yaptıktan sonra her seferinde: `npx cap sync`

### Android (Windows / Mac / Linux)
1. [Android Studio](https://developer.android.com/studio) kur.
2. `npx cap open android` (ya da Android Studio'da `android/` klasörünü aç).
3. Telefonu USB ile bağla (geliştirici seçenekleri → USB hata ayıklama açık) → ▶ Run.
4. **Google Play için:** Build → Generate Signed Bundle / APK → **Android App Bundle**
   → yeni bir anahtar deposu (keystore) oluştur → `.aab` dosyasını Play Console'a yükle.
   ⚠️ Keystore dosyasını ve şifresini kaybetme; güncellemeler için aynısı gerekir.

### iPhone (yalnızca Mac)
1. Mac'e Xcode kur (App Store'dan).
2. `npx cap open ios`
3. Xcode → App hedefi → **Signing & Capabilities** → Team olarak Apple kimliğini seç.
   - Ücretsiz Apple kimliği: kendi iPhone'una kurabilirsin (7 günde bir yenilemen gerekir).
   - Apple Developer Program (yıllık 99 $): TestFlight ve App Store.
4. iPhone'u bağla → ▶ Run. App Store için: Product → Archive → Distribute App.

---

## Yayından önce değiştirmek isteyebileceklerin
- **Uygulama kimliği** `com.shadowfist.game` — mağazalarda benzersiz olmalı.
  Ör. `com.seninadin.shadowfist` yap: `capacitor.config.json` içindeki `appId`'yi değiştir,
  ardından Android'de `android/app/build.gradle` → `applicationId` ve `namespace`,
  iOS'ta Xcode → Bundle Identifier.
- **Sürüm:** Android `android/app/build.gradle` → `versionCode` / `versionName`,
  iOS Xcode → Version / Build.
- **İkonları yenilemek:** `assets/` içindeki görselleri değiştir → `npm run assets` → `npx cap sync`.

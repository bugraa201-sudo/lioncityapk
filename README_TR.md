# Lion City Android – Android Studio projesi

Bu klasör, mevcut Lion City Python/aiohttp uygulamasını Android WebView içinde açan başlangıç projesidir. Python uygulaması APK içine taşınmaz; ayrı bir sunucuda çalışır.

## 1) Android APK oluşturma
1. Android Studio'yu bilgisayarınıza kurun.
2. Android Studio > **Open** ile bu klasörü (`LionCityAndroid`) seçin.
3. Gradle eşitlemesinin tamamlanmasını bekleyin; Android SDK Platform 35 kurulu olmalı.
4. Telefonda USB hata ayıklamayı açıp Run ile test edin.
5. APK için **Build > Build Bundle(s) / APK(s) > Build APK(s)** seçin.
6. Oluşan APK genellikle `app/build/outputs/apk/debug/app-debug.apk` konumundadır.

## 2) Şu anki ngrok bağlantısı
Uygulama `MainActivity.java` içinde şu adrese bağlanır:
`https://steadily-related-quagga.ngrok-free.app/`

Bu adres yalnızca ngrok tüneli ve bilgisayarınızdaki Python sunucusu çalışırken erişilebilir. Bilgisayar kapanırsa, Python durursa veya ngrok adresi değişirse APK açılmaz. Üretim kullanımı için VPS + alan adı + HTTPS önerilir. Adres değişirse `APP_URL` ve `ALLOWED_HOST` değerlerini güncelleyin.

## 3) Python sunucusu
`backend/server2.py` dosyası mevcut sürümün kopyasıdır. Kendi proje klasörünüzde `drivers.txt`, `admin_password.txt` ve uygulamanın oluşturduğu veri dosyalarını da koruyun. Sunucu bilgisayarda şu şekilde çalışır:

```bash
python -m pip install -r requirements.txt
python server2.py
```

Aynı bilgisayarda ngrok:

```bash
ngrok http 8080
```

Admin şifresi `admin_password.txt` içinde tutulur; gerçek şifrenizi APK içine koymayın ve dosyayı herkese açık depoya yüklemeyin.

## 4) Önemli GPS sınırı
Bu ilk APK sürümü uygulama açıkken WebView konum izni ister ve web uygulamasının `watchPosition` akışını kullanır. Uygulama arka plana alındığında veya ekran kapandığında kesintisiz sürücü takibi garanti edilmez. Arka planda sürekli GPS gerekiyorsa ayrıca Android Foreground Location Service, kalıcı bildirim ve uygun Android izinleri geliştirilmelidir.

## 5) Yayına çıkmadan önce
- Ngrok yerine kalıcı HTTPS alan adı/VPS kullanın.
- Yönetici şifresini güçlü ve benzersiz yapın.
- Sürücü/yolcu verileri ve canlı konum için gizlilik metni ve açık rıza akışını ekleyin.
- Gerçek cihazda yolcu, sürücü, WebSocket, GPS, SOS, duyuru ve yönetici panelini ayrı ayrı test edin.

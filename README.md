# 7style-map-v2

# 16 eylül 2023 - Cumartesi

Task ı aldığımda hemen google maps nasıl işliyor bir araştırmaya başladım.

> https://developers.google.com/maps/documentation/javascript/overview?hl=tr

Bu kaynak üzerinden maps lat lng ile veriyi çekebileceğimi anladım hemen aklımda bir basic proje tasarlamaya başladım. Tabi anlamlandırmam bir kaç saati aldı. Tek düşüncem markerları stabil bırakmaya çalışmaktı.Öncelikle bir live location buttonı ekledim çerçeve oluşturduktan sonra. live location a tıkladığımda konum iznimi isteyip bana konumumu göstersin. Tabi bu sırada yapamıyorum çünkü izinleri açmayı unutmuşum... API hataları için sıklıkla stackoverflow a girdim.

Temel bilgilendirme için şu video serilerine baktım.

> https://www.youtube.com/playlist?list=PL4cUxeGkcC9hYYGbV60Vq3IXYNfDk8At1

> https://www.youtube.com/playlist?list=PL3VM-unCzF8jX-GoazLPcbi7M0wJux8F-

Yaklaşık 10 saatte temel bazda eklemeler yaptım ve çerçevem hazırdı. Semantial ui da öğrenmek için semantial ui ekledim...

# 17 eylül 2023 - Pazar

Live location hazırdı ancak diğer adresleri autocomplete olmadan tam olarak yazmak zorundaydım. Search kısmı bir adım daha iyi çalışabilirdi. Autocomplete için bir çözüm aradım.

> https://medium.com/geekculture/building-a-localised-places-autocomplete-with-vue-js-tailwind-and-google-places-api-part-1-14cb83cdc659

> https://developers.google.com/maps/documentation/javascript/place-autocomplete?hl=tr

Arada API dan kaynaklı hatalar alıyorum. Çözüm basit:

> https://developers.google.com/maps/documentation/javascript/error-messages?hl=tr#invalid-key-map-error

Tasarımda cidden zorlandım çünkü maps kısmını ekranın yüzde 75 kadarını kaplamasını diğer işlemler için de sol tarafta yüzde 25 lik alanı kullanmalıydım. CSS kısmındaki karışıklıktan hatalarımı gözle göremiyordum. 3 saat uğraş sonrası kötü de olsa bir çerçevem vs. vardı.

> https://developer.mozilla.org/en-US/docs/Web/CSS/position

> https://www.geeksforgeeks.org/semantic-ui-varying-grid-centering-content/

css ile uğraşırken localStorage'a live location ve search ile aradığım konumları eklemeyi başardım. Eksiklik hata vardı. Page açıldığında kordinatlar 0 a 0 ve marker yok. Marker generate etmek için de live location' a basmalıyız. Live location a bastığımızda 2 adet eleman ekliyor live locationda bulunan arrayimize. Baya bir değişiklik yaptım ancak çözüm bulamadım...

Toplamda 7 saatlik bir çalışma geçirdim. Sıklıkla stackoverFlow üzerinden araştırmalar yaptım ancak linklerini tam bulamadım gir çık yaptığım linklerdi...

# 18 eylül 2023 - Pazartesi

Mailden anladığım üzere projede artık çıkmaza girmiştim. Kodlar iç içe girdiği için bir component mantığı bulunmuyordu. Sıfırdan başlamaya karar verdim. Google Maps'in kendi dökümantasyonuyla bir araştırmaya başladım.

> https://developers.google.com/maps/documentation/javascript/overview

Bahsettiğin vue2-google-maps e baktım. Kaynaklardan inceledim.

> https://beginnersoftwaredeveloper.com/how-to-use-google-map-vuejs-google-maps-examples/

Düşündüm ki koordinat değiştirmek için latitude ve longitude değerlerini girsek localStorage da bu değerleri değiştirsek.

> https://support.google.com/maps/answer/18539?hl=en&co=GENIE.Platform%3DDesktop

Map kısmını tamamlarken rota oluşturmak nasıl bir rota oluşturacağım ona bakmalıydım.

> https://developers.google.com/maps/documentation/javascript/examples/directions-travel-modes

GoogleMap.vue üzerinde bir yorum satırı olarak da ekledim center ı kendim manuel eklediğimde right click ile yeni bir marker ekleyemiyorum. Get All Markers and Refresh e bastığımızda bize ilk localdeki bilgileri getiriyor. update yada remove ile de arrayden eleman seçer gibi(tabikide 0 dan indexten başlıyor). Update yada remove işlemi yaptıktan sonra tekrar refresh etmeliyiz. Sayfayı refresh ettiğimde lokasyonlar yine istanbul izmir ankara oluyor.

Rota kısmında şu anlık a noktasından b noktasına mantığı ile sıradan bir rota işlemi var. Dediğimiz gibi 3 adet konum seçerek bana rota göstermesini ekleyeceğim. Bunu düşünmekteyim.

Toplamda bugün harcadığım süre 10-11 saat arası olabilir. Framework e entegre olmaya çalışırken uzun sürebiliyor...

# 19 eylül 2023 - Salı

Paketlerdeki terslikleri vs manuel temizlemek yerine temiz bir start ile başlamaya karar vermiştim. Uygulamayı yeni bir cli üzerinde kurdum. Macos in sudo problemleriyle uğraştım... Yaklaşık 2 saat sürdü.

## Project setup

```
npm install
```

### Compiles and hot-reloads for development

```
npm run serve
```

### Compiles and minifies for production

```
npm run build
```

### Lints and fixes files

```
npm run lint
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).

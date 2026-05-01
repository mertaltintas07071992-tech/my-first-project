# Black Smoke Jazz — 1 Saatlik Seamless Loop Video Üretim Reçetesi

Bu doküman, Flow / Gemini / Nano Banana ile ürettiğimiz 26+ noir jazz görselini **1 saatlik kesintisiz döngü (seamless loop) YouTube videolarına** dönüştürmek için ücretsiz, tarayıcı tabanlı ve yerel iş akışlarını içerir. Hiçbir kredi tükenmez, kayıt zorunluluğu minimumdur.

## 1. Strateji Özeti

YouTube'da 1 saatlik jazz/lo-fi kanal videosu üretmenin üç ana yolu vardır. Üçü de elimizdeki noir görsellerle uyumludur ve tamamı ücretsiz araçlarla yapılabilir.

**Yöntem A — Cinemagraph (Tek Görsel + Hareket Maskeleme):** Tek bir görseldeki belirli bölgeleri (ör. yağmur, duman, neon bokeh) hafifçe canlandırıp diğer bölgeleri tamamen statik bırakırız. Sonuç 6-12 saniyelik kusursuz bir döngüdür ve 1 saate uzatılır.

**Yöntem B — Ken Burns Slideshow:** Birden fazla görselde yavaş zoom/pan uygulayıp her görseli 30-45 saniye gösteririz. 60-100 görselden 1 saatlik tek dosyaya birleştiririz.

**Yöntem C — Image-to-Video AI Loop:** Ücretsiz AI servisleri kullanarak görseli 4-8 saniyelik kısa videoya çevirip 450 kez tekrarlatırız.

## 2. Ücretsiz Tarayıcı Tabanlı Image-to-Video Araçları

Aşağıdaki listede her aracın özellikleri, ücretsiz limiti, çıkış formatı ve loop için uygunluğu yer alır.

**Pika Labs (pika.art)** — Ücretsiz katmanda günlük belirli sayıda video üretimi. Image-to-video desteği var, "loop" parametresi promptlarda kullanılabilir. 3-4 saniye, 720p. Kayıt: e-posta veya Google.

**Runway ML (runwayml.com) — Gen-3 Free Tier:** Ücretsiz hesap açıldığında 125 kredi verir. Image-to-video Gen-2 ile yaklaşık 25 kredilik 4 saniyelik klipler üretir, yani 5 klip üretimi mümkün. Çıkış 1280x768.

**Kaiber (kaiber.ai)** — Image-to-video mode, ücretsiz deneme krediler. Atmosferik, müzik videosu temalı sonuçlarda güçlü.

**LeiaPix Converter (convert.leiapix.com)** — 2D fotoğrafı 3D parallax animasyona çevirir. Tamamen ücretsiz, kayıt gerektirir. Noir görsellerde brilliant — yağmurlu pencere, abajur derinlik kazanır.

**Immersity AI (immersity.ai)** — LeiaPix'in halefi. Ücretsiz parallax animasyon, 4-6 saniyelik MP4 çıkış. Loop için ideal: kamera yavaşça ileri gidip geri döner.

**CapCut Web (capcut.com)** — Ücretsiz video editor (kayıt gerekir). Görseller arasında "auto cinemagraph" filtresi ve Ken Burns animasyonu hazır şablonlarla bulunur. 1 saatlik export ücretsiz.

**Canva (canva.com) — Free Tier:** Görsele "Ken Burns" zoom ekleyip 60 saniyelik klipler üretebilir. Ücretsiz hesapta 1 saatlik export sınırlıdır ama 5-6 dakikalık parçalar halinde dışa aktarılır.

**Hailuo AI (hailuoai.video)** — MiniMax tarafından geliştirilen. Ücretsiz katmanda günlük image-to-video kotası verir. 6 saniye, 720p.

**Kling AI (klingai.com)** — Kuaishou'nun Sora rakibi. Ücretsiz hesapta günlük 6 ücretsiz video kredisi. Image-to-video desteği güçlü.

## 3. Önerilen İş Akışı (En Hızlı Sonuç İçin)

**Adım 1 — Tek bir master loop klip üret:** Immersity AI veya LeiaPix kullanarak elimizdeki en güçlü noir görseli (ör. Midnight Rain, Nighthawks Reimagined, Bordo Velvet Lounge) parallax loop videoya çevir. Çıkış genelde 4-6 saniyedir. MP4 olarak indir.

**Adım 2 — FFmpeg ile 1 saate uzat:** Yerel bilgisayarda FFmpeg ile aynı klibi 600+ kez ardışık olarak yapıştırıp 60 dakikalık tek dosyaya birleştir.

**Adım 3 — Müzik ekle:** Telifsiz jazz/lo-fi müziği YouTube Audio Library veya Pixabay Music'ten indir, ses kanalını üzerine bindir.

**Adım 4 — YouTube'a yükle.**

## 4. FFmpeg Reçeteleri

Aşağıdaki komutlar yerel makinede çalıştırılır. FFmpeg ücretsiz ve açık kaynaktır.

### 4.1 Tek Klipli Seamless Loop Uzatma (Yöntem C)

8 saniyelik bir loop.mp4 dosyasını 1 saate uzatmak için:

```
ffmpeg -stream_loop 449 -i loop.mp4 -c copy output_1hour.mp4
```

Burada 449 sayısı (450 toplam tekrar = 8 sn x 450 = 3600 sn = 60 dk) klibin kaç defa daha tekrar edeceğini belirler. `-c copy` parametresi yeniden kodlama yapmaz, anında biter.

### 4.2 Tek Görselden Sıfırdan Cinemagraph (Yöntem A)

Statik bir görseli yağmur damlası overlay'i ile birleştirerek pseudo-cinemagraph üretmek. Önce yağmur overlay videosunu Pixabay'den indir (ücretsiz), sonra:

```
ffmpeg -loop 1 -i image.jpg -i rain_overlay.mp4 -filter_complex \
"[0:v]scale=1920:1080,setsar=1[bg]; \
 [1:v]scale=1920:1080,colorkey=black:0.3:0.1[rain]; \
 [bg][rain]overlay=shortest=1[v]" \
-map "[v]" -t 8 -c:v libx264 -pix_fmt yuv420p cinemagraph.mp4
```

Bu komut 8 saniyelik bir klip üretir. Sonra 4.1'deki komutla 1 saate uzatılır.

### 4.3 Ken Burns Slideshow (Yöntem B)

60 görselden her biri 60 saniye gösterilen 1 saatlik slideshow:

```
ffmpeg -framerate 1/60 -i img%03d.jpg -c:v libx264 -r 30 \
  -vf "scale=1920:1080,zoompan=z='min(zoom+0.0005,1.2)':d=1800:s=1920x1080" \
  -pix_fmt yuv420p slideshow_1hour.mp4
```

Görseller img001.jpg, img002.jpg ... img060.jpg formatında olmalı. `zoompan` filtresi her görsele yavaş zoom-in efekti uygular (Ken Burns).

### 4.4 Müzik Bindirme

```
ffmpeg -i video_1hour.mp4 -i jazz_music.mp3 -c:v copy -c:a aac -shortest final.mp4
```

### 4.5 Crossfade ile Daha Yumuşak Loop

İki klibi crossfade ile birbirine bağlamak (loop noktası belirsizleşir):

```
ffmpeg -i loop.mp4 -i loop.mp4 -filter_complex \
  "[0:v][1:v]xfade=transition=fade:duration=1:offset=7" \
  -c:v libx264 smooth_loop.mp4
```

## 5. Telifsiz Jazz Müzik Kaynakları

YouTube videolarında ContentID/copyright stripe yememek için sadece bu kaynakları kullan.

**YouTube Audio Library** (studio.youtube.com → Audio Library) — Kanalın kendi panelinde, Genre filtresinde "Jazz & Blues" var. Tamamen ücretsiz, attribution gerektirmez.

**Pixabay Music** (pixabay.com/music) — "lo-fi jazz", "noir jazz", "smooth jazz" araması güçlü sonuçlar verir. CC0 lisans, attribution gerekmez.

**Free Music Archive** (freemusicarchive.org) — Genre: Jazz. Lisansı kontrol et (CC-BY çoğunluk).

**Uppbeat** (uppbeat.io) — Ücretsiz hesapta aylık 10 indirme. Cinematic jazz koleksiyonu zengin.

**Incompetech** (incompetech.com) — Kevin MacLeod arşivi, attribution gerekir ama kalitelidir.

## 6. Önerilen Pipeline (Pratik Reçete)

Aşağıdaki sıra, sıfırdan başlayan biri için en kısa yoldur.

İlk olarak immersity.ai açılır, "Nighthawks Reimagined" görseli yüklenir, "infinite zoom" veya "subtle parallax" preset'i seçilir, MP4 indirilir. Bu 5 saniyelik klip elimizdeki ilk varlık olur.

Sonra YouTube Audio Library'den 60 dakikalık jazz playlist yapılır (her biri 3-5 dakikalık 12-15 parça birbirine FFmpeg concat ile eklenir). Müzik dosyası hazırdır.

Yerel terminalde FFmpeg ile 5 saniyelik klip 720 kez tekrarlatılır:

```
ffmpeg -stream_loop 719 -i parallax.mp4 -c copy video_loop.mp4
```

Müzik bindirme komutu çalıştırılır:

```
ffmpeg -i video_loop.mp4 -i music_60min.mp3 -c:v copy -c:a aac -shortest BSJ_DeepNightJazz_1Hour.mp4
```

Sonuç: 1 saatlik kanal videosu, hem görsel açıdan döngülü hem de müzik olarak tutarlı.

## 7. YouTube Yükleme Kontrol Listesi

Başlık formatı: "Deep Night Jazz | 1 Hour Noir Lounge Ambience | Black Smoke Jazz" gibi anahtar kelime + süre + kanal markasını içerir.

Açıklama: Track listesi, müzik kaynağı atıfları, kanal sosyal medya linkleri.

Etiketler: deep night jazz, noir jazz, late night jazz, jazz lounge, 1 hour jazz, lo-fi jazz, black smoke jazz, rainy night jazz, smooth jazz ambience.

Thumbnail: Üretilen 8 video thumbnailinden ilgili olanı (1280x720 PNG) seç.

Kategori: Music. Dil: English (broader reach). Made for kids: No.

## 8. Olası Sorunlar ve Çözümler

**Loop noktası belli oluyor:** xfade ile crossfade uygula (4.5 numaralı reçete) veya başlangıç ve bitiş frame'lerini birebir eşitle.

**Video çok büyük:** `-crf 23` (varsayılan) yerine `-crf 28` kullanarak boyutu yarıya indir, kalite YouTube için yeterli.

**Audio loop sınırı duyuluyor:** Müzik dosyasını 65 dakika yap, video 60 dakika; `-shortest` flag'i fazla audio'yu keser, son ses kesilmesi olmaz.

**FFmpeg yok:** Online alternatifi olarak clideo.com/looper veya ezgif.com/loop-video kullanılabilir, fakat 100 MB altı dosya sınırı vardır.

## 9. Sonraki Adımlar

İlk teste şu görsellerden biriyle başla: Midnight Rain & Jazz, Nighthawks Reimagined, Noir Jazz Lounge, Late Night Saxophone, Lonely Night Jazz. Bu beşi en güçlü loop adayları çünkü hem derinlikleri hem statik kompozisyonları parallax animasyona uygundur.

İlk video yüklendikten sonra bu dosyadaki reçeteyi diğer 25+ görsele uygulayarak haftalık seri üretebilirsiniz.

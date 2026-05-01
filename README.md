# Easy LFI Lab (Baslangic Seviye)

Bu proje, PHP'de Local File Inclusion (LFI) zafiyetini temel seviyede gostermek icin hazirlanmis kasitli olarak zafiyetli bir egitim laboratuvaridir.

## Nasil calisir?

Uygulamanin giris noktasi `index.php` dosyasidir. Burada su mantik vardir:

```php
$page = $_GET['page'] ?? 'pages/home.php';
include($page);
```

- `page` GET parametresinden alinir.
- Herhangi bir filtreleme/dogrulama yapilmadan dogrudan `include()` icine verilir.
- Bu da kullanicinin hangi dosyanin include edilecegini kontrol etmesine imkan verir.

## Lab dosya yapisi

- `index.php` -> Zafiyetli include noktasi
- `pages/home.php` -> Anasayfa icerigi
- `pages/about.php` -> Hakkimizda icerigi + gizli ipucu
- `pages/contact.php` -> Iletisim icerigi
- `flag.txt` -> Hedef dosya (root dizinde)
- `assets/style.css` -> Tema

## Kurulum (Docker)

```bash
docker compose up --build -d
```

Tarayicidan ac:

`http://localhost:8080`

## LFI testi (sadece bu lab ortami icin)

Asagidaki URL'ler ile parametre davranisini gozlemleyebilirsin:

- Normal sayfa:
  - `http://localhost:8080/?page=pages/home.php`
  - `http://localhost:8080/?page=pages/about.php`

- Hedef dosyayi okuma:
  - `http://localhost:8080/?page=flag.txt`

- Dizin gecisi (ornek davranis testi):
  - `http://localhost:8080/?page=../../../../etc/passwd`

Not: Son URL, host ortamina gore farkli sonuc verebilir. Ama amac, filtrelenmeyen include nedeniyle path manipule edilebildigini gostermektir.

## Neden zafiyet?

Cunku uygulama:

- Kullanici girdisini dogrulamiyor
- Sabit bir allowlist kullanmiyor
- `include()` oncesi guvenlik kontrolu yapmiyor

## Guvenli yaklasim (kisa not)

Gercek uygulamalarda:

- Sadece izinli sayfa anahtarlarini kullan (allowlist)
- Dosya yolu kullanici girdisinden dogrudan uretilmesin
- `include` edilecek path sabit map'ten secilsin

---

Bu repo yalnizca yasal, kontrollu ve egitim amacli kullanim icindir.

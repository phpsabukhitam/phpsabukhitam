# Composer

[Composer](https://getcomposer.org) adalah sebuah *tool* untuk me-manage *dependency* di PHP.
Bagi sebagian pemula di PHP, Composer terdengar menyeramkan. Tapi, sekali terbiasa menggunakan, mereka tak akan bisa coding tanpanya.

Dalam membangun sebuah software, sering kali kita membutuhkan *library* yang disediakan pihak lain.
Misalnya untuk membuat file PDF, kita dapat menggunakan library [fpdf](http://www.fpdf.org/) atau [ZendPdf](https://github.com/zendframework/ZendPdf).
Untuk mengirim email, kita dapat menggunakan [swiftmailer](http://swiftmailer.org/) atau [Zend\Mail](http://framework.zend.com/manual/current/en/modules/zend.mail.introduction.html).
Untuk memanipulasi email, dapat menggunakan [Imagine](https://github.com/avalanche123/Imagine) atau [Intervention Image](http://image.intervention.io/).

Dengan composer, programmer cukup menuliskan library yang dia butuhkan dan Composer akan meng-*install*nya.

## Dependency Management

Composer me-manage *dependency* per *project*.
Artinya, library tidak terinstal secara global, namun pada masing-masing project.
Sebuah project bisa memiliki dependency library yang berbeda dengan project lainnya.

Masalah yang diselesaikan oleh Composer yaitu:

- Install library
- Bila ternyata library yang dibutuhkan membutuhkan library lainnya
- Deklarasi kebutuhan library dan versinya secara eksplisit

## Mendeklarasikan Dependency

Misal project kita membutuhkan library [Sastrawi](https://github.com/sastrawi/sastrawi), maka cukup buat file `composer.json` di project directory.

```json
{
    "require": {
        "sastrawi/sastrawi": "1.*"
    }
}
```

Berarti kita telah mendeklarasikan bahwa project tersebut membutuhkan library sastrawi/sastrawi versi 1 dengan segala versi minor.

## Install Composer

Cara [download](https://getcomposer.org/download/) / install composer cukup mudah:

### Cara pertama

```bash
    php -r "readfile('https://getcomposer.org/installer');" | php
```

Jika php tidak ditemukan, berarti harus disesuaikan ke instalasi php.
Jika instalasi berhasil, akan muncul file `composer.phar` di directory tersebut.

### Cara kedua

Atau jika menggunakan windows, dapat download dan instal menggunakan : [https://getcomposer.org/Composer-Setup.exe](https://getcomposer.org/Composer-Setup.exe)
Jika instalasi dengan cara di atas berhasil, maka perintah `composer` akan tersedia di command line.

## Cara Menggunakan Composer

Setelah membuat file `composer.json` berisi library yang dibutuhkan, composer dapat meng-installnya melalui *command line* dengan cara:

- Jika install composer dengan cara pertama: `php composer.phar install`
- Jika install composer dengan cara kedua: `composer install`

Setelah perintah sukses dijalankan, akan muncul directory `vendor/sastrawi/sastrawi`.

## Version

Pada contoh sebelumnya, sastrawi dituliskan dengan versi `1.*`, artinya sastrawi versi `1.0`, `1.1`, `1.2` akan memenuhi.
Cara menuliskan versi dapat dilakukan dengan berbagai cara:

| Nama           | Contoh    | Penjelasan | 
|----------------|-----------|---------------|
| Exact Version  | 1.0.5     | versi 1.0.5 bukan yang lain. |
| Range          | >=1.0.5   | versi 1.0.5 atau ke atas. |
| Hyphen Range   | 1.0 - 2.0 | versi 1.0 sampai versi 2.0 (*inclusive*). |
| Wildcard       | 1.0.*     | versi >= 1.0 <1.1. |
| Tilde Operator | ~1.2      | >=1.2 <2.0, sangat berguna pada library yang mengikuti *semantic versioning* |
| Caret Operator | ^1.2.3    | >=1.2.3 <2.0, sangat berguna pada library yang mengikuti *semantic versioning* |

Penjelasan lebih lengkap dapat diikuti di dokumentasi Composer: [https://getcomposer.org/doc/01-basic-usage.md#package-versions](https://getcomposer.org/doc/01-basic-usage.md#package-versions)

## Semantic Versioning

Pada software skala besar, semakin banyak *library* yang diintegrasi ke dalam sistem, akan membuat software tersebut semakin tergantung pada library itu.
Telah dialami sejak lama, sebuah software mengalami *version lock*, yaitu software terkunci pada versi library tertentu.
Ketika library tersebut merilis versi baru, software tidak dapat mengupgrade ke versi tersebut karena masalah *compatibility*.
Versi baru tersebut tidak compatible dengan versi sebelumnya, yang telah digunakan oleh software tersebut.

Oleh sebab itu, sangat penting bagi pengembang *library* untuk bertanggungjawab merilis sebuah versi karena library tersebut digunakan oleh banyak orang.
Hal penting yang perlu dikomunikasikan adalah tentang compatibility.

Semantic Versioning adalah sebuah spesifikasi atau aturan dalam merilis sebuah versi software.
Dengan mengikuti semantic versioning, pengembang *library* dapat mengkomunikasikan *compatibility* dari rilis tersebut.
Pengguna library akan sangat mudah menentukan, apakah versi tersebut dapat diintegrasi dengan software mereka atau tidak.

### MAJOR.MINOR.PATCH

Format utama semantic versioning adalah **MAJOR.MINOR.PATCH**.
Aturan untuk memberi versinya yaitu:

1. Naikkan versi MAJOR jika ada perubahan API yang **tidak *compatible*** dengan versi sebelumnya.
2. Naikkan versi MINOR jika ada perubahan yang **masih *compatible*** dengan versi sebelumnya.
3. Naikkan versi PATCH jika melakukan pembetulan *bug* atau *error*.

Dengan aturan di atas, pengguna library dijamin bahwa selama versi MAJOR masih sama, library tersebut *compatible* dengan sebelumnya.
Misalnya library *Symfony Console* merilis versi 2.7, maka dijamin bahwa versi tersebut compatible dengan versi 2.6, 2.5, 2.4 dan seterusnya ke bawah.
Jadi bila seorang programmer sedang menggunakan versi 2.6, dia dapat dengan tenang mengupgrade ke versi 2.7.

T> Dengan composer, gunakan versi semantic dengan tilde operator atau caret operator.
T> Menuliskan versi ~1, berarti composer akan mencari versi yang *compatible* yaitu >=1.0 <2.0

I> Spesifikasi lengkap Semantic Versioning dapat dilihat di [http://semver.org/](http://semver.org/)

## Composer.lock

Setelah meng-install *dependency*, Composer akan membuat sebuah file `composer.lock` yang berguna untuk mengunci versi library yang digunakan secara presisi.
Seperti yang telah dibahas sebelumnya bahwa pada `composer.json`, dapat dituliskan versi secara fleksibel seperti >=1.2.
Composer akan membaca file `composer.lock` untuk meng-install library dengan versi yang sama persis ketika file `composer.lock` dibuat.
Hal ini bagus karena antar satu programmer dengan programmer lainnya, dapat menggunakan library yang sama persis versinya.

Jika file `composer.lock` ada, Composer tidak membaca konfigurasi dari `composer.json`, melainkan dari `composer.lock`.
Jika file `composer.lock` tidak ada, Composer akan membaca dari `composer.json`, lalu menuliskan versi yang diambil ke `composer.lock`.

Hal ini berarti, ketika ada file `composer.lock`, Composer tidak akan mengambil versi yang lebih baru.
Untuk mengambil versi yang lebih baru, file `composer.lock` harus diupdate. Untuk mengupdate versi baru, sekaligus mengupdate file `composer.lock`, gunakan perintah:

```bash
    php composer.phar update
```

atau

```bash
    composer update
```

## Autoloading

Setelah berhasil install library menggunakan Composer, akan muncul directory `vendor`.
Di situlah letak library terinstall.
Untuk memanggil library yang telah terinstall, include file `vendor/autoload.php` di dalam project.

```php
    <?php

    require_once 'vendor/autoload.php';
```

Setelah itu, semua library siap dipakai.



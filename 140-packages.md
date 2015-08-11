# Packages

Ketika kita membangun software pada skala besar, sering kali kita kewalahan menghadapi kompleksitas software tersebut.
Salah satu cara untuk memanage kompleksitas adalah dengan membaginya ke bagian yang lebih kecil sehingga lebih mudah dimanage.
Sama halnya pada kehidupan, cara menyelesaikan masalah besar adalah dengan membaginya ke masalah kecil-kecil lalu menyelesaikannya satu per satu.

Demikian pula pada software.
Kompleksitas dapat diselesaikan dengan membaginya ke bagian yang lebih kecil berupa komponen, modul, atau package.

Setiap baris kode yang kita tulis mewakili sebuah perilaku pada sistem. Baris kode tersebut harus dipelihara agar tidak menimbulkan masalah di kemudian hari.

***DRY: Don't Repeat Yourself***, sebuah prinsip penting yang telah kita bahas di Bab *Prinsip Software Fleksibel*.
Duplikasi kode menjadikan *source code* menjadi besar -- dan tidak sehat.
Ketika sebuah *logika* yang terduplikasi itu harus berubah, maka perubahan tersebut harus diterapkan juga di tempat-tempat duplikasinya.

Selain itu, mengulang adalah pemborosan waktu, asset yang paling berharga.
Jarum jam terus berdetak dan tak pernah berbalik arah.
Mari kita manfaatkan waktu dan sisa usia dengan bijak.

Di PHP sudah tersedia banyak sekali package berkualitas yang dapat kita manfaatkan (*reuse*).
Dengan begitu kita tidak perlu menyelesaikan masalah yang sebetulnya sudah diselesaikan orang lain -- bahkan dengan cara yang lebih baik.
Bab ini akan membahas package-package yang tersedia di PHP yang dapat digunakan untuk mempercepat development.


## Validation dengan Zend Validator

Hampir setiap software mengambil input data dari luar.
Setiap input data tersebut harus divalidasi terlebih dahulu sebelum diproses.
Proses validasi adalah hal yang umum ditemui sehingga kita perlu menggunakan solusi yang reusable supaya produktif.

### Dahulu Kala

```php
<?php

$errors = array();

if (empty($_POST['username'])) {
    $errors[] = 'Username harus diisi';
}

if (empty($_POST['email'])) {
    $errors[] = 'Email harus diisi';
}

// tampilkan pesan error
```


Kita pasti pernah melihat (atau menulis) kode validasi seperti di atas.
Meskipun cara di atas dapat berjalan, namun cara tersebut kurang terstruktur dan tidak reusable.

Misal kita menambahkan validasi bahwa username hanya boleh berupa karakter alfanumeric, dan email harus valid, maka kode sedikit demi sedikit akan melakukan banyak pengulangan.

```php
<?php

$errors = array();

if (empty($_POST['username'])) {
    $errors[] = 'Username harus diisi';
} else if (!preg_match('/^[A-Za-z0-9]+$/', $_POST['username'])) {
    $errors[] = 'Username hanya boleh mengandung karakter alfanumeric.';
} else if (strlen($_POST['username']) < 7) {
    $errors[] = 'Username minimal 6 karakter';
}

if (empty($_POST['email'])) {
    $errors[] = 'Email harus diisi';
}

// tampilkan pesan error
```


### Zend Validator

Pada era modern seperti sekarang, sudah banyak tersedia library PHP untuk melakukan validasi.
Bila menggunakan framework, biasanya komponen validasi telah disediakan oleh framework tersebut.
Namun pada buku ini, sebagai contoh akan dijelaskan salah satu library validasi yaitu Zend Validator.

[Zend validator](http://framework.zend.com/manual/current/en/modules/zend.validator.html) memeriksa input dan dibandingkan dengan rules yang diberikan. Jika sesuai akan menghasilkan boolean true. Namun jika input tidak sesuai dengan rules, akan menghasilkan boolean false.
Selain itu, Validator juga akan memberikan informasi bagian mana yang tidak sesuai dengan rules.

### Install Zend Validator

Zend Validator dapat diinstal sebagai komponen library menggunakan Composer.

    php composer.phar require zendframework/zend-validator

### Penggunaan

Contoh kita akan mem-validasi username harus diisi:

```php
<?php

require_once __DIR__ . '/vendor/autoload.php';

$username  = ''; 
$validator = new Zend\Validator\NotEmpty();

if ($validator->isValid($username)) {
    // valid, lanjutkan proses
} else {
    // tidak valid, tampilkan pesan error
    foreach ($validator->getMessages() as $messageId => $message) {
        echo "$messageId: $message\n";
    }
}
```

Terlihat sedikit panjang di awal, tapi lebih terstruktur.
Karena sifatnya yang fleksibel, kita bisa menggunakan validator tambahan dari package yang lain.
Mari kita lihat jika validasi ditambah minimal 6 karakter dan berupa karakter alfanumeric.

Install komponen Zend Internationalization untuk memanfaatkan validasi tambahan yang tersedia di sana.

    php composer.phar require zendframework/zend-i18n
    php composer.phar require zendframework/zend-filter

```php
<?php

require_once __DIR__ . '/vendor/autoload.php';

$username  = '12345';
$usernameValidator = new Zend\Validator\ValidatorChain();
$usernameValidator->addValidator(new Zend\Validator\NotEmpty());
$usernameValidator->addValidator(new Zend\I18n\Validator\Alnum());
$usernameValidator->addValidator(new Zend\Validator\StringLength(array('min' => 6)));

if ($usernameValidator->isValid($username)) {
    // valid, lanjutkan proses
} else {
    // tidak valid, tampilkan pesan error
    foreach ($usernameValidator->getMessages() as $messageId => $message) {
        echo "$messageId: $message\n";
    }
}
```

Seperti kode di atas, kita tidak mengulang menuliskan logic validasi.
Dengan demikian, kode dapat reusable di tempat lain pada project itu, atau bahkan validasi dapat digunakan lagi di project lain.


### Validator yang Tersedia di Zend

| Class                               | Keterangan                                   |
|-------------------------------------|----------------------------------------------|
| Zend\\I18n\\Validator\\Alnum        | Validasi karakter alfa numeric ([a-zA-Z0-9]) |
| Zend\\I18n\\Validator\\Alpha        | Validasi karakter alfabet ([a-zA-Z])         |
| Zend\\Validator\\Barcode            | Validasi apakah sebuah string merupakan barcode yang valid | 
| Zend\\Validator\\Between            | Validasi apakah sebuah nilai berada di antara 2 nilai yang ditentukan |
| Zend\\Validator\\Callback           | Validasi menggunakan fungsi callback |
| Zend\\Validator\\CreditCard         | Validasi kartu kredit |
| Zend\\Validator\\Date               | Validasi tanggal |
| Zend\\Validator\\Db\\RecordExists   | Validasi record berada di database |
| Zend\\Validator\\Db\\NoRecordExists | Validasi record tidak ada di database |
| Zend\\Validator\\Digits             | Validasi digit (0-9) |
| Zend\\Validator\\EmailAddress       | Validasi alamat email |
| Zend\\Validator\\GreaterThan        | Validasi sebuah nilai harus lebih besar dari nilai yang ditentukan |
| Zend\\Validator\\Hex                | Validasi hexadecimal |
| Zend\\Validator\\Hostname           | Validasi hostname seperti nama domain dan ip address |
| Zend\\Validator\\Iban               | Validasi IBAN (International Bank Account Number) |
| Zend\\Validator\\Identical          | Validasi sebuah nilai harus sama identik dengan nilai tertentu |
| Zend\\Validator\\InArray            | Validasi sebuah nilai terdapat pada array tertentu |
| Zend\\Validator\\Ip                 | Validasi IP Address |
| Zend\\Validator\\Isbn               | Validasi ISBN |
| Zend\\Validator\\LessThan           | Validasi sebuah nilai kurang dari nilai tertentu |
| Zend\\Validator\\NotEmpty           | Validasi sebuah nilai tidak boleh kosong |
| Zend\\I18n\\Validator\\PostCode     | Validasi kode pos |
| Zend\\Validator\\Regex              | Validasi sebuah nilai sesuai dengan pola regex |
| Zend\\Validator\\Sitemap\\Lastmod   | Validasi Sitemap XML Protocol |
| Zend\\Validator\\StringLength       | Validasi panjang string |

Daftar lengkap validator yang tersedia dapat diakses di:

- [http://framework.zend.com/manual/current/en/modules/zend.validator.set.html](http://framework.zend.com/manual/current/en/modules/zend.validator.set.html)


### Custom message

### Membuat validator sendiri

### Library Validator lainnya:

- [Respect Validate](https://github.com/Respect/Validation)
- [Valitron](https://github.com/vlucas/valitron)
- [Symfony Validator](https://github.com/symfony/Validator)


## Filtering dengan Zend Filter

Berbeda dengan validasi, filter tidak mengembalikan boolean true atau false, melainkan mengembalikan input yang telah dimodifikasi.
Misalnya dari sebuah string `"Rp 2000,-"` difilter ke numeric menjadi `2000`.

Lebih jauh lagi, definisi filter dapat diperluas menjadi transformasi, atau perubahan pada sebuah input.
Contoh transformasi adalah dari string `"sabuk hitam"` menjadi `"SABUK HITAM"`.

### Install Zend Filter

    php composer.phar require zendframework/zend-filter

### Penggunaan

Untuk melakukan filter dari string `"Rp 2000,-"`, kita gunakan filter digit:

```php
<?php

require __DIR__ . '/vendor/autoload.php';

$filter = new Zend\Filter\Digits();
echo $filter->filter('Rp 2000,-'); // 2000
```

Filter juga dapat digunakan secara berantai:

```php
<?php

require __DIR__ . '/vendor/autoload.php';

$filter = new Zend\Filter\FilterChain();
$filter->attach(new Zend\Filter\StringToUpper());
$filter->attach(new Zend\Filter\StringTrim());

echo $filter->filter(' php sabuk hitam '); // PHP SABUK HITAM
```

### Filter yang Tersedia di Zend

| Class                               | Keterangan                                   |
|-------------------------------------|----------------------------------------------|
| Zend\\I18n\\Filter\\Alnum           | Filter alfa numeric |
| Zend\\I18n\\Filter\\Alfa            | Filter alfa |
| Zend\\Filter\\BaseName              | Filter base name |
| Zend\\Filter\\Boolean               | Convert nilai ke boolean | 
| Zend\\Filter\\Callback              | Filter menggunakan callback |
| Zend\\Filter\\Compress              | Compress sebuah string, file, atau directory |
| Zend\\Filter\\Decompress            | Decompress sebuah archive kembali ke string atau file |
| Zend\\Filter\\Digits                | Filter digit atau numeric |
| Zend\\Filter\\Dir                   | Mendapatkan directory dari sebuah path |
| Zend\\Filter\\Encrypt               | Enkripsi sebuah nilai |
| Zend\\Filter\\Decrypt               | Dekripsi sebuah nilai |
| Zend\\Filter\\HtmlEntities          | Convert karakter ke HTML entities |
| Zend\\Filter\\Int                   | Convert sebuah nilai ke integer |
| Zend\\Filter\\Null                  | Convert semua input menjadi null |
| Zend\\I18n\\Filter\\NumberFormat    | Format angka |
| Zend\\Filter\\PregReplace           | Replace string menggunakan regex |
| Zend\\Filter\\RealPath              | Convert sebuah path menjadi path absolute |
| Zend\\Filter\\StringToLower         | Mengubah sebuah string menjadi huruf kecil |
| Zend\\Filter\\StringToUpper         | Mengubah sebuah string menjadi huruf besar / kapital |
| Zend\\Filter\\StringTrim            | Menghapus whitespace di depan dan belakang sebuah string |
| Zend\\Filter\\StripNewlines         | Menghapus karakter ganti baris pada sebuah string |
| Zend\\Filter\\StripTags             | Menghapus tag html pada sebuah string |
| Zend\\Filter\\UriNormalize          | Menormalisasi uri |

Daftar lengkap Filter yang tersedia dapat dilihat di:

- [http://framework.zend.com/manual/current/en/modules/zend.filter.set.html](http://framework.zend.com/manual/current/en/modules/zend.filter.set.html)


### Membuat Filter Sendiri


## Email dengan Zend Mail

Mengirim email juga merupakan kebutuhan yang sering ditemui oleh programmer web.
Dengan Zend\Mail, pengiriman email menjadi mudah dan lebih terstruktur karena menggunakan object.

### Install Zend Mail

    php composer.phar require zendframework/zend-mail

### Penggunaan Sederhana

Contoh penggunaan sederhana untuk mengirim email dari akun gmail.
Pengiriman dari SMTP lain dapat dilakukan dengan cara yang sama, cukup mengganti konfigurasi connection saja.

```php
<?php

require_once __DIR__ . '/vendor/autoload.php';

$mail = new Zend\Mail\Message();
$mail->setBody('Isi email.');
$mail->setFrom('isi-dengan-email-gmail@gmail.com', 'Nama Pengirim');
$mail->addTo('emailpenerima@contoh.com', 'Nama Penerima');
$mail->setSubject('PHP Sabuk Hitam');

// Contoh seting SMTP untuk mengirim email menggunakan gmail
$transport = new Zend\Mail\Transport\Smtp();
$options   = new Zend\Mail\Transport\SmtpOptions(
    array(
        'name'              => 'smtp.googlemail.com',
        'host'              => 'smtp.googlemail.com',
        'port'              => 587,
        'connection_class'  => 'login',
        'connection_config' => array(
            'username' => 'isi-dengan-email-gmail',
            'password' => 'isi-dengan-password-gmail',
            'ssl'      => 'tls',
        ),  
    )
);
$transport->setOptions($options);
$transport->send($mail);
```

Pertama kali, Gmail mungkin akan menolak pengiriman email melalui SMTP karena konfigurasi security yang cukup ketat.
Kemudian, gmail akan memberi notifikasi untuk opsi menonaktifkan konfigurasi keamanan yang ketat.

### Mengirim email dengan attachment

### Lebih lanjut tentang Zend Mail

- [http://framework.zend.com/manual/current/en/modules/zend.mail.introduction.html](http://framework.zend.com/manual/current/en/modules/zend.mail.introduction.html)


## Manipulasi gambar dengan Imagine

[Imagine](http://imagine.readthedocs.org/en/latest/) adalah library untuk memanipulasi gambar.
Library ini berorientasi object dan memiliki kualitas yang bagus.

### Install Imagine

```bash
$ php composer.phar require imagine/imagine
```

### Membuat thumbnail menggunakan Imagine

```php
<?php
require 'vendor/autoload.php';

$imagine = new \Imagine\Gd\Imagine();
$imagine->open('cover.jpg')
        ->thumbnail(
            new \Imagine\Image\Box(400, 400),
            \Imagine\Image\ImageInterface::THUMBNAIL_OUTBOUND
        )
        ->save('cover_thumb.jpg');
```

![Hasil Thumbnail dari Imagine](images/packages/imagine/cover_thumb.jpg)

### Resize gambar

```php
<?php
require 'vendor/autoload.php';

$imagine = new \Imagine\Gd\Imagine();
$imagine->open('cover.png')
        ->resize(new \Imagine\Image\Box(255, 330))
        ->save('cover_small.jpg');
```

![Hasil resize](images/packages/imagine/cover_small.jpg)

### Memberi watermark pada gambar

```php
<?php
require 'vendor/autoload.php';

$imagine   = new \Imagine\Gd\Imagine();

$watermark = $imagine->open('watermark.png');
$cover     = $imagine->open('cover.png')
                     ->resize(new \Imagine\Image\Box(510, 660));

$coverSize     = $cover->getSize();
$watermarkSize = $watermark->getSize();

$bottomLeft = new \Imagine\Image\Point(
    20, 
    $coverSize->getHeight() - $watermarkSize->getHeight()
);

$cover->paste($watermark, $bottomLeft)
      ->save('cover_watermark.jpg');
```


![Gambar dengan watermark](images/packages/imagine/cover_watermarked.jpg)

### Memberi effect pada gambar

```php
<?php
require 'vendor/autoload.php';

$imagine = new \Imagine\Gd\Imagine();
$cover   = $imagine->open('cover.png');

$blue = $cover->palette()->color('#2345F3');

$cover->effects()->colorize($blue);

$cover->resize(new \Imagine\Image\Box(357, 462));
$cover->save('cover_effect.jpg');
```
![Gambar setelah diberi effect](images/packages/imagine/cover_effect.jpg)

### Lebih lanjut tentang Imagine

Lebih lanjut tentang Imagine dapat dipelajari di dokumentasi Imagine:

- [http://imagine.readthedocs.org/en/latest/index.html](http://imagine.readthedocs.org/en/latest/index.html)


{pagebreak}

## Http request dengan Guzzle

[Guzzle](http://guzzle.readthedocs.org/en/latest/) adalah library [HTTP](http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) client.
Sebelumnya, untuk melakukan request HTTP di PHP, kita dapat menggunakan curl, stream wrapper, juga socket.
Setiap alternatif tersebut memiliki API yang sangat berbeda, sehingga susah untuk berpindah dari satu ke yang lain.

Guzzle memberikan solusi berupa library dengan API yang rapi yang menggambarkan HTTP request dan response menggunakan OOP.
Fitur-fitur Guzzle antara lain:

- manage *persistent connections*, *query strings*, dan banyak hal lain
- mempermudah pengiriman request POST dengan upload files
- *asynchronous request*
- dapat memilih http transport handler seperti curl, socket, stream, atau React.

### Install Guzzle

```bash
php composer.phar require guzzlehttp/guzzle
```

### Request HTTP GET

### Request Object

### Response Object

### Request HTTP POST

### Query Strings

### HTTP Header

### Redirects



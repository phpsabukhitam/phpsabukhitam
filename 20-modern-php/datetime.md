## DateTime - Manipulasi Tanggal dan Waktu

Sejak PHP 5.2, manipulasi tanggal dan waktu dapat dilakukan dengan berorientasikan object.
Beberapa class telah disediakan untuk urusan tanggal dan waktu.
Bab ini menjelaskan mengapa harus move on ke object dan bagaimana caranya.

### Kesulitan yang Dialami Pada Cara Prosedural

Sering kali, kita menemui kebutuhan untuk memanipulasi tanggal sebelum menampilkannya dalam format tertentu.
Meski *simpel* dan praktis dalam menampilkan tanggal, cara prosedural memiliki kesulitan dalam memanipulasinya.

```php
<?php

// Cara menampilkan sebuah tanggal dalam berbagai format dengan cara prosedural

$tgl = mktime(23, 59, 59, 1, 1, 2015);
var_dump($tgl); // int(1420131599)

$tglStr1 = date('Y-m-d H:i:s', $tgl);
var_dump($tglStr1); // string(19) "2015-01-01 23:59:59"

$tglStr2 = date('d-m-Y H:i:s', $tgl);
var_dump($tglStr2); // string(19) "01-01-2015 23:59:59"
```
    
Perhatikan kode di atas.
Pada cara prosedural, `date()` mengembalikan string tanggal yang telah terformat, dari parameter berupa integer.

Tidak ada yang salah dengan cara di atas.
Hanya saja, ketika kebutuhan manipulasi tanggal menjadi semakin kompleks, dibutuhkan data yang lebih terstruktur.
Contohnya, dari `$tgl` tersebut, semisal kita ingin menguranginya 3 hari.
Bagaimanakah cara melakukannya?
Apakah menggunakan operasi matematika, mencari tanggalnya lalu dikurangi 3?

### Solusi Dengan Object DateTime

```php
<?php

// membuat object date time menggunakan constructor
// tanpa parameter di constructor berarti tanggal sekarang
$date1 = new DateTime(); // tanggal sekarang
$date2 = new DateTime('2015-02-01 13:10:15');

// menampilkan tanggal dengan format yang diinginkan
var_dump($date1->format('Y-m-d H:i:s')); // string(19) "2015-02-07 20:08:07"
                                         // tanggal sekarang akan mengikuti

var_dump($date2->format('Y-m-d H:i:s')); // string(19) "2015-02-01 13:10:15"

// mengurangi 3 hari cara pertama
$date2->sub(new DateInterval('P3D'));
var_dump($date2->format('Y-m-d H:i:s')); // string(19) "2015-01-29 13:10:15"
                                         // sudah berkurang 3 hari

// mengurangi 3 hari cara kedua
$date2->modify('-3 days');
var_dump($date2->format('Y-m-d H:i:s')); // string(19) "2015-01-26 13:10:15"
                                         // berkurang 3 hari lagi
```


Dengan menggunakan `DateTime`, operasi tanggal dan waktu menjadi lebih *clean*, lebih mudah dibaca.
Selain itu, karena telah berbentuk object, segala kelebihan *object oriented programming* juga dapat dimanfaatkan, seperti yang akan dijelaskan kemudian.


### Membandingkan DateTime

Membandingkan 2 tanggal menggunakan `DateTime` cukup mudah dan enak dibaca.

```php
<?php

$date1 = new DateTime('2 Jan 2015');
$date2 = new DateTime('1 Feb 2015');

var_dump($date1 == $date2); // bool(false)
var_dump($date1 < $date2);  // bool(true)

if ($date2 > $date1) {
    echo $date2->format('Y-m-d H:i:s') 
        . ' lebih besar dari ' . $date1->format('Y-m-d H:i:s') . "\n";
    // 2015-02-01 00:00:00 lebih besar dari 2015-01-02 00:00:00
}
```


### Selisih Waktu

Untuk mencari selisih antara 2 tanggal juga sangat mudah.

```php
<?php

$date1 = new DateTime('30 Jan 2015 15:00:00');
$date2 = new DateTime('2 Feb 2015 23:30:00');

$diff  = $date1->diff($date2);

echo "Selisih: {$diff->d} hari, {$diff->h} jam, {$diff->i} menit\n";
// Selisih: 3 hari, 8 jam, 30 menit

var_dump($diff);
// menampilkan properties lengkap dari $diff
```

    `DateTime::diff()` mengembalikan object [DateInterval](http://php.net/manual/en/class.dateinterval.php) yang akan dibahas kemudian.


### Menambah dan Mengurangi DateTime

Untuk penambahan dan pengurangan `DateTime`, dapat ditempuh dengan banyak cara.
Yang pertama ialah dengan [`DateTime::add()`](http://php.net/manual/en/datetime.add.php) dan [`DateTime::sub()`](http://php.net/manual/en/datetime.sub.php).

```php
<?php

$date1 = new DateTime('2015-02-28');
var_dump($date1->format('Y-m-d H:i:s')); // string(19) "2015-02-28 00:00:00"

$date1->add(new DateInterval('P1D')); // tambah 1 hari
var_dump($date1->format('Y-m-d H:i:s')); // string(19) "2015-03-01 00:00:00"

$date1->sub(new DateInterval('P1M')); // kurangi 1 bulan
var_dump($date1->format('Y-m-d H:i:s')); // string(19) "2015-02-01 00:00:00"

$date1->add(new DateInterval('PT2H30M')); // tambah 2 jam 30 menit
var_dump($date1->format('Y-m-d H:i:s')); // string(19) "2015-02-01 2:30:00"

$date1->sub(new DateInterval('PT4H30S')); // kurangi 4 jam 30 detik
var_dump($date1->format('Y-m-d H:i:s')); // string(19) "2015-01-31 22:29:30"
```

Selain cara di atas, bisa juga menggunakan [`DateTime::modify()`](http://php.net/manual/en/datetime.modify.php).

```
<?php

$date1 = new DateTime('2015-02-28');
var_dump($date1->format('Y-m-d H:i:s')); // string(19) "2015-02-28 00:00:00"

$date1->modify('+1 day'); // tambah 1 hari
var_dump($date1->format('Y-m-d H:i:s')); // string(19) "2015-03-01 00:00:00"

$date1->modify('-1 month'); // kurangi 1 bulan
var_dump($date1->format('Y-m-d H:i:s')); // string(19) "2015-02-01 00:00:00"

$date1->modify('+ 2 hours 30 minutes'); // tambah 2 jam 30 menit
var_dump($date1->format('Y-m-d H:i:s')); // string(19) "2015-02-01 2:30:00"

$date1->modify('-4 hours -30 seconds'); // kurangi 4 jam 30 detik
var_dump($date1->format('Y-m-d H:i:s')); // string(19) "2015-01-31 22:29:30"
```

### Timezone

Mengapa website besar seperti facebook, google, menyediakan layanannya dalam berbagai bahasa?
Hal itu dikarenakan dengan menyediakan layanan di internet (website), maka penggunanya tidak berbatas tempat.
Tidak berbatas waktu.

Website dapat diakses dari belahan dunia manapun, dengan beragam budaya, bahasa, mata uang, dan zona waktu.
Pengunjung sebuah website lebih nyaman menggunakan budaya mereka sehari-hari.
Oleh sebab itu, penting bagi penyedia layanan internet untuk menyediakan kenyamanan itu.

Adaptasi sebuah produk, layanan, atau dokumen terhadap sebuah budaya atau bahasa (*locale*) disebut dengan [Localization](http://www.w3.org/International/questions/qa-i18n.en)
Localization tidak hanya terbatas pada bahasa, namun juga mencakup banyak hal seperti:
 
- Angka dan penomoran
- Penanggalan dan waktu
- Mata uang
- Zona waktu
- Keyboard (*character set*)

Mungkin sebagian akan bertanya, "Tapi *content* website saya kan memang di-khususkan untuk yang berbahasa Indonesia?"
Baiklah, perlu diingat bahwa Indonesia memiliki lebih dari 250 juta penduduk, lebih dari 700 bahasa daerah (saya tak bermaksud menyarankan untuk menyediakan terjemah website ke 700 bahasa daerah), dan **3 zona waktu**.

Pengguna setia yang berasal dari Bali atau dari Makassar akan lebih nyaman jika menggunakan zona waktu mereka sehari-hari.
Menyediakan personalisasi zona waktu seperti itu di website, akan membuat pengunjung nyaman dan terkesan.


#### Zona waktu di PHP

Untuk membuat object `DateTime` dengan zona waktu yang diinginkan:

```php
<?php

$date1 = new DateTime('2015-01-01 09:00:00', new DateTimeZone('Asia/Jakarta'));

var_dump($date1->format('Y-m-d H:i:s e'));
// string(32) "2015-01-01 09:00:00 Asia/Jakarta"

$date1->setTimezone(new DateTimeZone('Asia/Makassar'));

var_dump($date1->format('Y-m-d H:i:s e'));
// string(33) "2015-01-01 10:00:00 Asia/Makassar"
```

Ketika membuat object `DateTime`, parameter kedua, yaitu zona waktu, dapat dikosongi.
Bila zona waktu tidak diisi, berarti zona waktu yang digunakan adalah zona waktu default.
Zona waktu default dapat diisi melalui [date_default_timezone_set()](http://php.net/manual/en/function.date-default-timezone-set.php) atau melalui `php.ini` pada direktif [`date.timezone`](http://php.net/manual/en/datetime.configuration.php#ini.date.timezone).
Untuk mendapatkan timezone default yang sedang digunakan, gunakan [`date_default_timezone_get`](http://php.net/manual/en/function.date-default-timezone-get.php).

Daftar zona waktu di PHP dapat dilihat di : [http://php.net/manual/en/timezones.php](http://php.net/manual/en/timezones.php)


### DateTimeImmutable

`Class` `DateTimeImmutable` berperilaku sama dengan `DateTime` hanya saja dia *immutable*, tidak bisa diubah.
`DateTimeImmutable` tidak dapat berubah `value` nya. Operasi penambahan dan pengurangan akan menghasilkan `object` baru.

`Class` `DateTimeImmutable` tersedia di PHP >= 5.5.0

```php
<?php

$date1 = new DateTimeImmutable('2015-01-01');
$date2 = $date1->modify('+1 day');

var_dump($date1->format('Y-m-d H:i:s'));
// $date1 immutable (tidak berubah).
// string(19) "2015-01-01 00:00:00"

var_dump($date2->format('Y-m-d H:i:s'));
// modify menghasilkan object baru $date2
// string(19) "2015-01-02 00:00:00"
```

Sebisa mungkin, gunakan `DateTimeImmutable` sebagai ganti dari `DateTime`. (Butuh penjelasan lebih lanjut)


### DatePeriod

[`DatePeriod`](http://php.net/manual/en/class.dateperiod.php) dapat digunakan untuk melakukan iterasi dari sebuah tanggal ke tanggal lain pada interval tertentu.

#### Contoh 1 : Menampilkan tanggal antara 27-Dec-2014 sampai 2-Jan-2015

```php
<?php

$date1    = new DateTimeImmutable('27-Dec-2014');
$date2    = new DateTimeImmutable('3-Jan-2015');

// iterasi tiap satu hari
$interval = new DateInterval('P1D');

$period   = new DatePeriod($date1, $interval, $date2);

foreach ($period as $tgl) {
    echo $tgl->format('Y-m-d') . "\n";
}

/*
2014-12-27
2014-12-28
2014-12-29
2014-12-30
2014-12-31
2015-01-01
2015-01-02
tanggal 3-jan-2015 tidak termasuk (excluded)
*/
```

#### Contoh 2 : Menampilkan jam antara 21:00 sampai 02:00

```php
<?php

$date1    = new DateTime();
$date1->setTime(21, 0, 0);

$date2    = new DateTime();
$date2->setTime(3, 0, 0);
$date2->modify('+1 day');

// iterasi tiap satu jam
$interval = new DateInterval('PT1H');

$period   = new DatePeriod($date1, $interval, $date2);

foreach ($period as $tgl) {
    echo $tgl->format('H') . "\n";
}

/*
21
22
23
00
01
02
jam 3 tidak termasuk (excluded)
*/
```

### Extending DateTime

Seperti yang telah dijelaskan sebelumnya, dengan menggunakan `DateTime` berarti dapat memanfaatkan fitur-fitur Object pada PHP.
Salah satu fitur itu ialah *inheritance*. Kita dapat membuat turunan *class* `DateTime`.

```php
<?php

class DateTimeSabukHitam extends DateTime
{
    /**
     * Default timezone ke Asia/Jakarta
     */
    public function __construct($time = 'now', DateTimeZone $timezone = null)
    {
        $timezone = is_null($timezone) 
            ? new DateTimeZone('Asia/Jakarta') : $timezone;

        parent::__construct($time, $timezone);
    }
}

$date1 = new DateTimeSabukHitam();
var_dump($date1->format('Y-m-d H:i:s e'));
// string(32) "2015-02-13 15:11:40 Asia/Jakarta"
```
Contoh turunan *class* `DateTime` yang tersedia di jagat maya:

- [Drupal DateTime](https://api.drupal.org/api/drupal/core!lib!Drupal!Component!Datetime!DateTimePlus.php/class/DateTimePlus/8)
- [https://github.com/briannesbitt/Carbon](https://github.com/briannesbitt/Carbon)
- [https://github.com/jasonlewis/expressive-date](https://github.com/jasonlewis/expressive-date)

### DateTime sebagai Boundary

%% tambahi pengantar

Bayangkan ada kebutuhan untuk membuat fungsi yang menghitung jumlah hari minggu di antara 2 tanggal.

```php
<?php

function hitungHariMinggu($tanggalAwal, $tanggalAkhir)
{
    // hitung hari minggu sejak $tanggalAwal sampai $tanggalAkhir
}
```

Perhatikan parameter `$tanggalAwal` dan `$tanggalAkhir`.
Fungsi di atas belum jelas meminta parameter tersebut dalam tipe apa.
Apakah yang diingikan adalah `$tanggalAwal` berupa string? Ataukah berupa integer? Ataukah berupa `DateTime`?

*Function* yang baik memiliki perilaku yang jelas, presisi, dan tidak ambigu (tidak berstandard ganda).
*Function* yang baik harus presisi tentang beberapa hal seperti:

- parameter seperti apa yang diterima
- parameter seperti apa yang tidak diterima
- apa yang dilakukan oleh `function` tersebut
- seperti apa *return value*-nya
- *exception* apa yang akan muncul jika *guarantee*-nya tidak berhasil dipenuhi

Mari kita terapkan aturan di atas supaya desain *function* `hitungHariMinggu()` menjadi lebih baik.

#### Pengecekan parameter (cara yang kurang tepat)

```php
<?php

function hitungHariMinggu($tanggalAwal, $tanggalAkhir)
{
    // yang diinginkan adalah string
    if (is_string($tanggalAwal)) {
        $tanggalAwal = strtotime($tanggalAwal);
    } else {
        throw new InvalidArgumentException(
            'tanggalAwal harus berupa string berisi tanggal yang valid.'
        );
    }
    // hitung hari minggu sejak $tanggalAwal sampai $tanggalAkhir
}
```

Setelah kode menjadi seperti di atas, sudah mulai jelas parameter yang diterima dan yang tidak.
Namun pengecekan tersebut masih bermasalah, bagaimana bila $tanggalAwal diisi dengan string yang bukan tanggal, katakanlah "abcdef".
Maka fungsi di atas tidak dapat menyelesaikan *guarantee*-nya.
Seharusnya ada pengecekan tanggal harus valid.
Semakin *ribet* ya.

Sebetulnya tidak, justru tidak tepat bila meletakkan pengecekan tanggal di sini.
Cara yang lebih baik yaitu menggunakan [*Type Hint*](http://php.net/manual/en/language.oop5.typehinting.php).

*Type Hinting* akan dijelaskan di Bab Object.

#### Menggunakan Type Hint (cara yang lebih tepat)

```php
<?php

function hitungHariMinggu(DateTime $tanggalAwal, DateTime $tanggalAkhir)
{
    // yang diinginkan adalah bertipe DateTime

    // hitung hari minggu sejak $tanggalAwal sampai $tanggalAkhir
}
```

Deklarasi seperti di atas sudah sangat jelas dan presisi bahwa yang diinginkan adalah bertipe DateTime.
Jika fungsi di atas dipanggil dengan parameter selain DateTime, maka error akan muncul di sisi pemanggil fungsi.
Beda halnya dengan pengecekan parameter tadi (tanpa type hint), error nya akan muncul dari dalam fungsi.

#### Menggunakan Type Hint (cara yang sangat tepat)

```php
<?php

function hitungHariMinggu(
    DateTimeInterface $tanggalAwal,
    DateTimeInterface $tanggalAkhir
) {
    // yang diinginkan adalah bertipe DateTimeInterface
    // object bertipe DateTime maupun DateTimeImmutable sama-sama bisa diterima

    // hitung hari minggu sejak $tanggalAwal sampai $tanggalAkhir
}
```

Untuk menjadi fleksibel, *Type Hint* yang tepat adalah *Type Hint* ke *abstraction* (*interface*).
Dengan demikian class `DateTime` maupun `DateTimeImmutable` dapat digunakan.
Bahkan class yang dibangun sendiri, asal *implements* `DateTimeInteface` juga dapat digunakan di fungsi tersebut.

Hal ini akan dibahas lebih lanjut pada Bab Prinsip Software Fleksibel.

### Further Reading

Lebih lanjut tentang DateTime dapat dibaca di:

- [http://php.net/manual/en/book.datetime.php](http://php.net/manual/en/book.datetime.php)
- [http://php.net/manual/en/class.datetime.php](http://php.net/manual/en/class.datetime.php)
- [http://www.hashbangcode.com/blog/how-i-learned-stop-using-strtotime-and-love-php-datetime](http://www.hashbangcode.com/blog/how-i-learned-stop-using-strtotime-and-love-php-datetime)
- [https://philsturgeon.uk/blog/2012/08/why-php-datetime-rocks/](https://philsturgeon.uk/blog/2012/08/why-php-datetime-rocks/)
- [http://code.tutsplus.com/tutorials/dates-and-time-the-oop-way--net-35395](http://code.tutsplus.com/tutorials/dates-and-time-the-oop-way--net-35395)
- [http://www.sitepoint.com/working-with-dates-and-times/](http://www.sitepoint.com/working-with-dates-and-times/)


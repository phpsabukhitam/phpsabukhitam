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




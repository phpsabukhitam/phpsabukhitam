Syntax
===

PHP sebagai scripting language yang bisa disisipkan kedalam kode HTML harus diawali dengan `<?php` dan diakhiri dengan `?>`.
Dan setiap *statement* harus diakhiri dengan `titik koma` (;).

Berikut contoh kode PHP yang dapat disisipkan kedalam kode HTML

```php
<!DOCTYPE html>
<html>
    <head>
        <title>PHP Sabuk Hitam</title>
    </head>
    <body>
        <?php echo '<p>Halo dunia</p>'; ?>
    </body>
</html>
```

Meskipun awalan *syntax* PHP dapat dipersingkat menjadi `<?` namun penulis tidak menganjurkan untuk menggunakan alternatif awalan *syntax* PHP selain `<?php`.

Jika kode PHP tidak disispkan kedalam kode HTML lebih baik kode PHP tidak perlu diberi penenutup `?>` karena jika ada whitespace setelah `?>` maka akan terjadi efek yang idak diharapkan seperti waning `Warning: Cannot modify header information - headers already sent` karena fungsi `header()` harus dilakukan sebelum PHP melakukan *output buffering* sedangkan jika terdapat whitespace seteleah `?>` pada file yang murni berisi kode PHP maka PHP akan memulai *output buffering* sebelum fungsi `header()` dijalankan.
Berikut contoh jika menulis kode yang murni PHP.

```php
<?php

echo 'hello world';

// End Of File
```

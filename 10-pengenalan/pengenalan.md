Pengenalan Bahasa Pemrograman PHP
===

Bahasa pemrogram PHP (PHP: Hypertext Prepocessor) adalah sebuah *scripting langauge* yang dibangun untuk mempermudah proses pengembangan aplikasi berbasis website.
PHP pertama kali dikembangakan pada tahun 1995 oleh *Rasmus Lerdorf*. Pada awalnya *Rasmus* menciptakan tool sederhana yang bernama *PHP Tool* (Personal Home Page Tool) yang berfungsi untuk mengelola website pribadinya.
Fitur awal PHP Tool meliputi *Form handling* dan kemampuan untuk menyisipkan kode kedalam *HTML*. Menurut *Rasmus* versi awal PHP Tool tidak dia rencanakan untuk menjadi bahasa pemrograman baru, namun karena kepopuleran saat itu memaksa PHP Tool untuk didevelop menjadi bahasa pemrogramman dan menghasilkan sebuah bahasa yang banyak memiliki inkonsistensi dalam penamaan *method*,

PHP 3 & 4
---
Pada tahun 1997 *Andi Gutman* dan *Zeev Suraski* menulis ulang parser yang digunakan PHP Tool dan mengubah akronim PHP menjadi yang saat ini kita telah ketahui dan merilis PHP versi 3.
2 tahun kemudian *Andi Gutman* dan *Zeev Surasi* menulis ulang PHP Core dan menamainya dengan *Zend Engine*. Mereka berdua juga membuat perusahaan yang bernama *Zend Technology*.
Pada tanggal 22 Mei 2000 mereka merilis PHP versi 4 yang dibangun diatas *Zend Engine*

PHP 5
---
PHP versi 5 dirilis pada tahun 2004 yang dibangun diatas *Zend Engine* versi 2. PHP 5 telah mendukung *Object Oriented Programming* dan *PHP Data Object extension*.
Meskipun PHP 5 telah dirilis pada tahun 2004 namun popularitasnya kalah jauh dibanding dengan versi 4. Oleh karena itu pada tahun 2008 Konsorsium PHP mengadakan gearakan *GoPHP5* yang 'memakasa' project *opensource* yang terkenal untuk mengadopsi PHP 5.
PHP 5 terus berkembang dan terus menambah fitur-fitur yang berkaitan dengan Object Oriented Programming sepeteri *Trait*, *Static Late Binding*, *Reflection*, dan lain lain yang akan dibahas lebih mendalam pada bab berikutnya.
Selain itu PHP juga memperkaya fitur-fitur yang berkaitan dengan *Fuctional Programming* seperti *Closure* atau *annonymous function*.

PHP 6
---
PHP mendapat beberapa review negatif karena tidak mendukung *unicode* atau yang lebih dikenal saat ini dengan *emoji*.
Oleh karena itu pada tahun 2005, *Andrei Zmievsk* mengawali experiment untuk mengimplementasikan *unicode* kedalam core PHP.
Namun karena kekurangan developer yang mengerti untuk memodifikasi kode agar support *unicode* dan masalah performa konversi dari atau pada *UTF-16* maka project ini pun akhirnya ditunda.
Fitur *unicode* tidak pernah dirilis sampai PHP 7 muncul.

PHP 7
---
Pada tahun 2014 sampai 2015, versi baru PHP mulai develop dan yang mengejutkan PHP akan menggunakan versi 7 mengingat versi 6 belum dirilis.
Padahal banyak buku yang memakai PHP 6 telah muncul dipasaran, hal ini mengakibatkan perdebatan dan akhirnya komunitas sekapat melalui voting untuk menggakan versi 7 untuk rilis berikutnya.
PHP 7 dikembangankan dari *branch* PHP yang bernama PHP Next Generation (phpng). PHPNG sendiri dikembangan oleh *Dmitry Stogov*, *Xinchen Hui* and *Nikita Popov* dan tujuan utama mereka adalah untuk meningkatkan performa PHP dengan *merefactor* Zend Engine dengan mempertahankan sebagaikan besar kompatibilitas *Zend Engine* versi sebelumnya.
Perubahan yang sangat signifikan pada PHPNG akan memudahkan untuk menningkatkan performa di kemudian hari dengan mengimplemtasi *Just In Time* (JIT) *compiler*.
 

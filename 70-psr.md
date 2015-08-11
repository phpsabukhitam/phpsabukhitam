# PSR (PHP Standard Recommendation)

## PHP-FIG (Framework Interop Group)

[PHP-FIG](http://www.php-fig.org/) (*PHP Framework Interop Group*) adalah sebuah grup yang berisi perwakilan project-project open source di PHP.
Grup tersebut membahas tentang kesamaan pada project-project mereka. Tujuannya adalah agar ditemukan solusi yang standard yang dapat digunakan
pada tiap-tiap project.

Salah satu contoh kesamaan yang ditemui adalah fitur logging.
Banyak framework maupun library di PHP memiliki fitur pencatatan log, namun implementasinya berbeda.
Ada Zend Framework dengan komponen [`Zend\Log`](http://framework.zend.com/manual/current/en/modules/zend.log.overview.html), ada library [`Monolog`](https://github.com/Seldaek/monolog), ada pula [`Log4php`](http://logging.apache.org/log4php/).

Dengan banyaknya interface yang berbeda dan tidak compatible satu sama lain, justru akan membuat hal itu menjadi kelemahan pada ekosistem PHP secara keseluruhan.

PSR (*PHP Standard Recommendation*) adalah spesifikasi yang dihasilkan dari PHP-FIG melalui [workflow](https://github.com/php-fig/fig-standards/blob/master/bylaws/004-psr-workflow.md) yang cukup panjang: Pre-Draft, Entrance Vote, Draft, Review, Accepted.

Berdasarkan [*Index of PHP Standard Recommendations*](http://www.php-fig.org/psr/), sampai saat ini, ada 5 PSR accepted, 1 PSR sedang direview, 4 PSR dalam draft.

{width="95%"}
| Status   | PSR Num | Title                  | Editor                  | Coordinator   |
|----------|---------|------------------------|-------------------------|---------------|
| Accepted | PSR-0   | Autoloading Standard   | N/A                     | N/A           |
| Accepted | PSR-1   | Basic Coding Standard  | N/A                     | N/A           |
| Accepted | PSR-2   | Coding Style Guide     | N/A                     | N/A           |
| Accepted | PSR-3   | Logger Interface       | Jordi Boggiano          | N/A           |
| Accepted | PSR-4   | Autoloading Standard   | Paul M. Jones           | Phil Sturgeon |
| Draft    | PSR-5   | PHPDoc Standard        | Mike van Riel           | Phil Sturgeon |
| Review   | PSR-6   | Caching Interface      | Larry Garfield          | Pádraic Brady |
| Draft    | PSR-7   | HTTP Message Interface | Matthew Weier O'Phinney | Phil Sturgeon |
| Draft    | PSR-8   | Huggable Interface     | Larry Garfield          | Cal Evans     |
| Draft    | PSR-9   | Security Disclosure    | Lukas Kahwe Smith       | Korvin Szanto | 

{pagebreak}

## PSR-0: Autoloading Standard

Mulanya, untuk dapat menggunakan class yang dideklarasikan di file lain, kita menggunakan `require` atau `require_once`.
Lambat laun, cara tersebut mulai menjadi tidak efektif dikarenakan:

- Pada jumlah class yang cukup banyak, menuliskan require_once satu per satu adalah pekerjaan yang membosankan.
- Tidak fleksibel. Ketika letak file diubah, require_once juga harus berubah.

PHP 5 memberi solusi yaitu `autoload`.
Fungsi autoload akan mencari letak file yang mengandung deklarasi class yang dicari, lalu melakukan `require` padanya.
Dengan demikian kita tidak perlu menuliskan require satu per satu.

## Autoload di PHP diterapkan dengan fungsi berikut:

- [`spl_autoload()`](http://php.net/manual/en/function.spl-autoload.php)
- [`spl_autoload_register()`](http://php.net/manual/en/function.spl-autoload-register.php)

Tentu saja, agar dapat menerapkan autoload dengan baik, penamaan class dan peletakan file harus memiliki pola dan aturan.
Misalnya class:
- `Zend_Controller_Action` berada pada `Zend/Controller/Action.php`, pola yang digunakan di sini adalah _ (*underscore*) pada nama class menandakan letak file nya berada pada directory tersebut.
- `Doctrine\Common\Util\Debug` berada pada `Doctrine/Common/Util/Debug.php`
- `SabukHitam\BlogPost` berada pada `SabukHitam/BlogPost.php`

Begitulah, dengan aturan penamaan class dan peletakan file, selain dapat diload secara otomatis, juga mempermudah development dan debugging.
Jika muncul error / exception pada sebuah class, sangat mudah mencari letak file nya.
Coba bayangkan jika tanpa aturan yang jelas, misalnya muncul error di class `BlogPostDatabase`, lalu class tersebut tidak diketahui dideklarasikan di file mana.
Pasti pusing kepala dibuatnya.

PSR-0 adalah standardisasi autoloading.
Aturan-aturan PSR-0 adalah sebagai berikut:

- *Fully-qualified namespace* dan *class* harus mengikuti susunan sebagai berikut: \<Vendor Name>\(<Namespace>\)*<Class Name>
- Setiap *namespace* harus memiliki *top-level namespace* ("Vendor Name").
- Setiap *namespace* boleh memiliki *sub-namespaces* sebanyak apapun.
- Setiap *namespace separator* diconvert ke DIRECTORY_SEPARATOR ketika akan diload dari file system.
- Setiap _ character pada CLASS NAME diconvert ke DIRECTORY_SEPARATOR. Underscore _ character tidak memiliki makna apapun pada namespace.
- *Fully-qualified namespace* dan *class* diakhiri dengan .php saat akan diload dari file system.
- Karakter alfabet pada *vendor names*, *namespaces*, dan *class names* boleh berupa huruf kecil maupun huruf besar.

Contoh namespace dan class sesuai PSR-0:

- `\Doctrine\Common\IsolatedClassLoader` => `/vendor/Doctrine/Common/IsolatedClassLoader.php`
- `\Symfony\Core\Request` => `/path/to/project/lib/vendor/Symfony/Core/Request.php`
- `\Zend\Acl` => `/path/to/project/lib/vendor/Zend/Acl.php`
- `\Zend\Mail\Message` => `/path/to/project/lib/vendor/Zend/Mail/Message.php`
- `\Sastrawi\Stemmer\Stemmer` => `/path/to/project/vendor/sastrawi/Sastrawi/Stemmer/Stemmer.php`



### Lebih jauh tentang PSR-0

- [http://www.php-fig.org/psr/psr-0/](http://www.php-fig.org/psr/psr-0/)
- [http://www.sitepoint.com/autoloading-and-the-psr-0-standard/](http://www.sitepoint.com/autoloading-and-the-psr-0-standard/)

{pagebreak}

## PSR-1: Basic Coding Standard

PSR-1 bertujuan memberikan standardisasi element-element kode PHP secara garis besar.
Tujuannya adalah supaya kode-kode yang ditulis dengan PHP, memiliki kesamaan struktur.

### Files

#### PHP Tags

File PHP harus menggunakan tag pembuka `<?php` dan `<?=`. Tidak diperbolehkan menggunakan variasi tag lainnya.

PHP memiliki [variasi tag](http://php.net/manual/en/language.basic-syntax.phpmode.php) yang beragam yaitu: `<?php`, `<?=`, `<?`, `<script language=PHP>`, `<%`.
 
Tag `<% %>` hanya tersedia jika diaktifkan melalui `asp_tags` di `php.ini`.
Tag `<? ?>` hanya tersedia jika diaktifkan melalui `short_open_tag` di `php.ini`.

#### Character Encoding

File PHP harus disimpan dalam [UTF-8](http://en.wikipedia.org/wiki/UTF-8) tanpa [BOM (Byte Order Marker)](http://en.wikipedia.org/wiki/Byte_order_mark).

![Contoh cara setting UTF-8 tanpa BOM di notepad++.](images/psr/utf8-without-bom.png)


#### Side Effects

Sebuah file hanya boleh melakukan salah satu dari 2 hal berikut:
1. Mendeklarasikan `class`, `function`, `constant`, atau deklarasi lainnya.
2. Melakukan eksekusi selain deklarasi. Istilah yang digunakan di PSR yaitu *side effects*. *Side effect* seperti menampilkan *output*, menulis file, mengubah setting ini, mengubah *global variable*, dan lain-lain.

Berikut adalah contoh kode yang dilarang, yaitu yang mengandung side effects dan mengandung deklarasi secara bersamaan.

```php
<?php
// side effect: change ini settings
ini_set('error_reporting', E_ALL);

// side effect: loads a file
include "file.php";

// side effect: generates output
echo "<html>\n";

// declaration
function foo()
{
	// function body
}
```

Agar sesuai PSR-1, kode di atas harus dipisah menjadi 2 file:

File pertama hanya berisi deklarasi:

```php
<?php

// declaration
function foo()
{
    // function body
}
```

dan file kedua berisi eksekusi / *side effects*:

```php
<?php

// side effect: change ini settings
ini_set('error_reporting', E_ALL);

// side effect: loads a file
include "file.php";
	
// side effect: generates output
echo "<html>\n";
```


#### Namespace dan Nama Class

Namespace dan nama class harus mengikuti autoloading standard PSR: PSR-0 atau PSR-4
Artinya, sebuah file hanya boleh mendeklarasikan satu Class saja.
Sebuah class harus berada dalam sebuah namespace (minimal satu namspace, yaitu *top-level vendor*).
Nama class harus ditulis dalam format `StudlyCaps` (huruf kapital di awal suku kata, misal `BukuTamu`).
Kode PHP yang ditulis versi 5.3 ke atas harus menggunakan namespace formal (\Vendor\SubNamespace\Class).

Contoh kode:

```php
<?php
// PHP 5.3 ke atas:
namespace Vendor\Model;

class Foo
{
}
```

Kode untuk PHP 5.2 ke bawah, menggunakan namespace *boongan* menggunakan karakter _:

```php
<?php
// PHP 5.2.x ke bawah:
class Vendor_Model_Foo
{
}
```

#### Class Constants, Properties, dan Methods

**Constants**

*Class* mewakili semua *Class*, *Abstract Class*, *Interface*, dan *Trait*.

Constant harus dideklarasikan dengan huruf kapital dengan underscore sebagai pemisah suku kata.
Contoh:

```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

**Properties**

Penamaan properties harus konsisten pada scope yang wajar, seperti pada sebuah *vendor*, *package*, *class*, atau *method*.

**Methods**

Nama method harus berformat `camelCase()`.

Contoh yang benar:

```php
<?php
namespace Vendor\Model;

class Foo
{
    public function getDateApproved()
    {
    }
}
```

Contoh yang tidak sesuai PSR-1:

```php
    <?php
    namespace Vendor\Model;
    
    class Foo
    {
        public function get_date_approved()
        {
        }
    }
```


### Lebih lanjut tentang PSR-1

- [http://www.php-fig.org/psr/psr-1/](http://www.php-fig.org/psr/psr-1/)



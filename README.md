# 220302045 - Safinka Nurin Sabrina
# PBF-Codelgniter4

## Selamat Datang Di Codelgniter4
Codelgniter adalah kerangka kerja atau framework pengembangan aplikasi web berbasis PHP yang bersifat open-source.
  # Server Requirements
    1. PHP dan Ekstensi yang Diperlukan
        • intl
        • mbstring
        • json
    2. Ekstensi PHP Opsional
        Ekstensi PHP berikut yang harus diaktifkan di server anda :
          • mysqlnd (jika menggunakan MySQL)
          • curl (jika menggunakan CURLRequest)
          • imagick (jika menggunakan Image Class dengan ImageMagickHandler)
          • gd (jika menggunakan Image class dengan GDHandler)
          • simplexml (jika memformat XML)

          Ekstensi PHP berikut diperlukan saat menggunakan server Cache :
            • memcache (jika menggunakan Cache class dengan MemcachedHandler menggunakan Memcache)
            • memcached (jika menggunakan Cache class dengan MemcachedHandler menggunakan Memcached)
            • redis (jika menggunakan Cache class dengan RedisHandler)

          Ekstensi PHP berikut diperlukan saat menggunakan PHPUnit :
            • dom (jika menggunakan TestResponse class)
            • libxml (jika menggunakan TestResponse class)
            • xdebug (jika menggunakan CIUnitTestCase::assertHeaderEmitted())
    3. Basis Data yang Didukung
      1) MYSQL melalui driver MySQLi
      2) PostgreSQL melalui driver Postgre
      3) SQLite3 melalui driver SQLite3
      4) Microsoft SQL Server melalui driver SQLSRV
      5) Oracle database melalui driver OCI8

  # Credit
    Codelgniter awalnya dikembangkan oleh EllisLab. Pada tahun 2014, Codelgniter diakuisisi oleh British Columbia Institute of Technology dan kemudian diumumkan secara resmi sebagai proyek yang dipelihara oleh komunitas. Pada tahun 2019, yayasan Codelgniter dibentuk untuk menyediakan kelompok pengelolaan yang berkelanjutan yang berpisah dari entitas lain untuk membantu memastikan masa depan framework ini.

  # PSR Compliance
    PSR (PHP Standards Recommendations) didirikan pada tahun 2009 untuk membantu membuat kode antara framework dengan meratifikasi antarmuka, paduan gaya, dan lain-lain. 
    1) PSR-1:Basic Coding Standard
    2) PSR-12:Extended Coding Style
    3) PSR-3:Logger Interface
    4) PSR-4:Autoloading Standard
    5) PSR-6:Caching Interfaces PSR-16:SimpleCache Interface
    6) PSR-7:HTTP Message Interface
  # License Agreement
    • Hak Cipta (c) 2014-2019 Institut Teknologi British Columbia
    • Hak Cipta (c) 2019-2023 Yayasan Codelgniter 

## Installasi
  # Composer Installation
    Instalasi CI :
    ```shell
    composer create-project codelgniter4/appstarter (project-root/nama folder)
    ```
  # Running Your App
    Menjalankan CI :
    ```shell
    php spark serve
    ```
  # Troubleshooting
    Buka menggunakan browser dengan alamat IP :
    ```shell
    http://localhost:8080
    ```
## Build Your First Application
  # Static Pages
    1) Setting Routing Rules
      File located at app/Config/Routes.php
      ```shell
      <?php

use CodeIgniter\Router\RouteCollection;

/**
 * @var RouteCollection $routes
 */
$routes->get('/', 'Home::index');
```
    2) Create Pages Controller
      File at app/Controllers/Pages.php
      ```shell
      <?php

namespace App\Controllers;

class Pages extends BaseController
{
    public function index()
    {
        return view('welcome_message');
    }

    public function view($page = 'home')
    {
        // ...
    }
}
```
    3) Create Views
      Create the header at app/Views/templates/header.php
      ```shell
      <!doctype html>
<html>
<head>
    <title>CodeIgniter Tutorial</title>
</head>
<body>

    <h1><?= esc($title) ?></h1>
    ```
    4) Complete Pages::view() Method
      File at app\Controllers\Pages.php
      ```shell
      <?php

namespace App\Controllers;

use CodeIgniter\Exceptions\PageNotFoundException; // Add this line

class Pages extends BaseController
{
    // ...

    public function view($page = 'home')
    {
        if (! is_file(APPPATH . 'Views/pages/' . $page . '.php')) {
            // Whoops, we don't have a page for that!
            throw new PageNotFoundException($page);
        }

        $data['title'] = ucfirst($page); // Capitalize the first letter

        return view('templates/header', $data)
            . view('pages/' . $page)
            . view('templates/footer');
    }
}
```
    5) Running the App
      ```shell
      php spark serve
      ```
  # News Section
    1) Buat database di MySQL database
        ```shell
        CREATE TABLE news (
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    title VARCHAR(128) NOT NULL,
    slug VARCHAR(128) NOT NULL,
    body TEXT NOT NULL,
    PRIMARY KEY (id),
    UNIQUE slug (slug)
);
```
    2) Koneksi Database di file .env
        Konfigurasi file .env
        ```shell
        database.default.hostname = localhost
database.default.database = ci4tutorial
database.default.username = root
database.default.password = root
database.default.DBDriver = MySQLi
```
    3) Buat NewsModel dari app/Models/NewsModel.php
        ```shell
        <?php

namespace App\Models;

use CodeIgniter\Model;

class NewsModel extends Model
{
    protected $table = 'news';
}
```
    4) Add NewsModel::getNews() Method
        ```shell
        public function getNews($slug = false)
    {
        if ($slug === false) {
            return $this->findAll();
        }

        return $this->where(['slug' => $slug])->first();
    }
    ```
    5) Menambahkan Aturan Perutean
        Ubah file app/Config/Routes.php
        ```shell
        <?php

// ...

use App\Controllers\News; // Add this line
use App\Controllers\Pages;

$routes->get('news', [News::class, 'index']);           // Add this line
$routes->get('news/(:segment)', [News::class, 'show']); // Add this line

$routes->get('pages', [Pages::class, 'index']);
$routes->get('(:segment)', [Pages::class, 'view']);
```
    6) Buat News Controll
        Buat di app/Controllers/News.php
        ```shell
        <?php

namespace App\Controllers;

use App\Models\NewsModel;

class News extends BaseController
{
    public function index()
    {
        $model = model(NewsModel::class);

        $data['news'] = $model->getNews();
    }

    public function show($slug = null)
    {
        $model = model(NewsModel::class);

        $data['news'] = $model->getNews($slug);
    }
}
```
    7) Buat News::index() Method
        Buat di file app/Controllers/News/News.php
        ```shell
        <?php

namespace App\Controllers;

use App\Models\NewsModel;

class News extends BaseController
{
    public function index()
    {
        $model = model(NewsModel::class);

        $data = [
            'news'  => $model->getNews(),
            'title' => 'News archive',
        ];

        return view('templates/header', $data)
            . view('news/index')
            . view('templates/footer');
    }

    // ...
}
```
    8) Buat newws/index View File
        Buat di app/Views/news/index.php
        ```shell
        <h2><?= esc($title) ?></h2>

<?php if (! empty($news) && is_array($news)): ?>

    <?php foreach ($news as $news_item): ?>

        <h3><?= esc($news_item['title']) ?></h3>

        <div class="main">
            <?= esc($news_item['body']) ?>
        </div>
        <p><a href="/news/<?= esc($news_item['slug'], 'url') ?>">View article</a></p>

    <?php endforeach ?>

<?php else: ?>

    <h3>No News</h3>

    <p>Unable to find any news for you.</p>

<?php endif ?>
```
    9) Lengkapi News::show() Method
        ```shell
        <?php

namespace App\Controllers;

use App\Models\NewsModel;
use CodeIgniter\Exceptions\PageNotFoundException;

class News extends BaseController
{
    // ...

    public function show($slug = null)
    {
        $model = model(NewsModel::class);

        $data['news'] = $model->getNews($slug);

        if (empty($data['news'])) {
            throw new PageNotFoundException('Cannot find the news item: ' . $slug);
        }

        $data['title'] = $data['news']['title'];

        return view('templates/header', $data)
            . view('news/view')
            . view('templates/footer');
    }
}
```
    10) Buat news/view View File
        Melalui app/Views/news/view.php
        ```shell
        <h2><?= esc($news['title']) ?></h2>
<p><?= esc($news['body']) ?></p>
```
## Codeigniter4 Overview
  # Models, Views, and Controllers
    1) Models, mengelola data aplikasi dan membantu menegakkan aturan bisnis khusus yang mungkin diperlukan aplikasi.
    2) Views, file sederhana dengan sedikit atau tanpa logika, yang menampilkan informasi kepada pengguna.
    3) Controllers, bertindak sebagai kode perekat, menyusun data bolak-balik antara tampilan (atau pengguna yang melihatnya) dan penyimpanan data.
  # Autoloading Files
    1) Namespaces
      ```shell
      <?php

namespace Config;

use CodeIgniter\Config\AutoloadConfig;

class Autoload extends AutoloadConfig
{
    // ...
    public $psr4 = [
        APP_NAMESPACE => APPPATH, // For custom app namespace
        'Config'      => APPPATH . 'Config',
    ];

    // ...
}
```
    2) Confirming Namespace
      ```shell
      php spark namespaces
```
## General Topics
  # Handling Multiple Applications
    1) The Environment Constant
    ```shell
    CI_ENVIRONMENT = development
    ```
## Controllers and Routing
  # Controller
    Controller adalah file kelas yang menangani permintaan HTTP. 
  # Constructor
    Controller CodeIgniter memiliki konstruktor khusus 'InitController()'.
## Building Responses
  # View
  
## Working with Databases
## Modeling Data
## Managing Databases

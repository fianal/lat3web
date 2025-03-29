# lab7_php_ci

| Data diri| 
|-----------------|
| Nama : Alfian Nur Rizki  | 
| Kelas : TI 23.A6 | 
| NIM : 312310665 | 

<h1> Praktikum 3: View Layout dan View Cell </h1>

## Membuat Layout Utama

<p>Buat folder layout di dalam app/Views/ </p>
<p>buat file main.php di dalam folder layout dengan kode berikut:</p>

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title><?= $title ?? 'My Website' ?></title>
    <link rel="stylesheet" href="<?= base_url('/style.css'); ?>">
</head>
<body>
    <div id="container">
        <header>
            <h1>Layout Sederhana</h1>
        </header>
        
        <nav>
            <a href="<?= base_url('/'); ?>" class="<?= (url_is('/')) ? 'active' : ''; ?>">Home</a>
            <a href="<?= base_url('/artikel'); ?>" class="<?= (url_is('artikel')) ? 'active' : ''; ?>">Artikel</a>
            <a href="<?= base_url('/about'); ?>" class="<?= (url_is('about')) ? 'active' : ''; ?>">About</a>
            <a href="<?= base_url('/contact'); ?>" class="<?= (url_is('contact')) ? 'active' : ''; ?>">Kontak</a>
        </nav>

        <section id="wrapper">
            <section id="main">
                <?= $this->renderSection('content') ?>
            </section>

            <aside id="sidebar">
                <?= view_cell('App\\Cells\\ArtikelTerkini::render') ?>

                <div class="widget-box">
                    <h3 class="title">Widget Header</h3>
                    <ul>
                        <li><a href="#">Widget Link</a></li>
                        <li><a href="#">Widget Link</a></li>
                    </ul>
                </div>

                <div class="widget-box">
                    <h3 class="title">Widget Text</h3>
                    <p>
                        Vestibulum lorem elit, iaculis in nisl volutpat, 
                        malesuada tincidunt arcu. Proin in leo fringilla, 
                        vestibulum mi porta, faucibus felis. Integer pharetra 
                        est nunc, nec pretium nunc pretium ac.
                    </p>
                </div>
            </aside>
        </section>

        <footer>
            <p>&copy; 2021 - Universitas Pelita Bangsa</p>
        </footer>
    </div>
</body>
</html>
```

## Modifikasi File View

<p>Ubah app/Views/home.php agar sesuai dengan layout baru:</p>

```
<?= $this->extend('layout/main') ?>

<?= $this->section('content') ?>

<h1><?= $title; ?></h1>
<hr>
<p><?= $content; ?></p>

<?= $this->endSection() ?>
```

<p>Sesuaikan juga untuk halaman lainnya yang ingin menggunakan format layout yang baru.</p>

## Menampilkan Data Dinamis dengan View Cell

<p>View Cell adalah fitur yang memungkinkan pemanggilan tampilan dalam bentuk komponen yang dapat digunakan ulang. Cocok digunakan untuk elemen-elemen yang sering muncul di berbagai halaman seperti sidebar, widget, atau menu navigasi.</p>

## Membuat Class View Cell

<p>Buat folder Cells di dalam app/ </p>
<p> Buat file ArtikelTerkini.php di dalam app/Cells/ dengan kode berikut:</p>

```
<?php

namespace App\Cells;

use CodeIgniter\View\Cells\Cell;
use App\Models\ArtikelModel;

class ArtikelTerkini extends Cell
{
    public function render(): string
    {
        $artikelModel = new ArtikelModel();
        $data['artikel'] = $artikelModel->orderBy('id', 'DESC')->limit(5)->findAll();

        return view('components/artikel_terkini', $data);
    }

}

```

## Membuat View untuk View Cell

<p>Buat folder components di dalam app/Views/ </p>
<p>Buat file artikel_terkini.php di dalam app/Views/components/ dengan kode berikut:</p>

```
<h3>Artikel Terkini</h3>
<?php if (!empty($artikel) && is_array($artikel)): ?>
<ul>
    <?php foreach ($artikel as $row): ?>
        <li><a href="<?= base_url('/artikel/' . esc($row['slug'])) ?>"><?= esc($row['judul']) ?></a></li>
    <?php endforeach; ?>
</ul>
<?php else: ?>
    <p>Tidak ada artikel terbaru.</p>
<?php endif; ?>
```

## Tugas 

• Apa manfaat utama dari penggunaan View Layout dalam pengembangan aplikasi?

• Jelaskan perbedaan antara View Cell dan View biasa.

• Ubah View Cell agar hanya menampilkan post dengan kategori tertentu.

## Jawaban 

• Apa manfaat utama dari penggunaan View Layout dalam pengembangan aplikasi?

<p>- Memudahkan Pengelolaan Tampilan</p>

<p>- Memisahkan struktur utama dan konten dinamis</p>

• Jelaskan perbedaan antara View Cell dan View biasa.

<p>- View biasa digunakan untuk menampilkan halaman utama secara langsung dengan view, sedangkan View Cell view_cell digunakan untuk memuat komponen kecil yang bersifat reusable dan dapat menjalankan logika sebelum ditampilkan.</p>

• Ubah View Cell agar hanya menampilkan post dengan kategori tertentu.

![img](https://github.com/fianal/lat3web/blob/main/img_lat3web/Homepage.png)


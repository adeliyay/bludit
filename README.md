<h1 align="center"><img src="https://raw.githubusercontent.com/adeliyay/bludit/master/Screenshot/logo.png"></h1>

[Sekilas Tentang](#sekilas-tentang) | [Instalasi](#instalasi) | [Konfigurasi](#konfigurasi) | [Cara Pemakaian](#cara-pemakaian) | [Pembahasan](#pembahasan) | [Referensi](#referensi)
:---:|:---:|:---:|:---:|:---:|:---:

# Aplikasi Web "Bludit"

## Sekilas Tentang
[`^ kembali ke atas ^`](#)

Bludit merupakan sebuah CMS (Content Management System) yang digunakan untuk membuat blog pribadi dalam waktu singkat, bebas biaya dan open source. Bludit menggunakan flat-files untuk menyimpan kiriman dan halaman, tanpa perlu menginstall atau konfigurasi basisdata. Bludit mendukung kode Markdown dan HTML untuk konten kiriman dan halaman yang akan diposting. 

## Instalasi 
[`^ kembali ke atas ^`](#)

**Kebutuhan Sistem :**
-   Ubuntu Server 18.04
-   Web Serverhttps://cdn.worldvectorlogo.com/logos/bludit.svg Apache version 2.4 atau lebih
-   PHP 7.2
-   php7.2-common, php7.2-mbstring, php7.2-xmlrpc, php7.2-soap, php7.2-gd, php7.2-xml, php7.2-cli, php7.2-curl
-   Virtual Box
-   Non-root user dengan pengaturan sudo privileges
-   IP address statik

**Proses Instalasi :**
1. Install Virtual Machine (Virtual Box) dan Ubuntu Server di laptop.
2. Buka Virtual Machine dan Klik New pada virtual machine lalu tambahkan ubuntu server.
   Klik Start dan jalankan Ubuntu server lalu add instalasi ubuntu server.
   Klik Setting >Tampilan *dashboard* admin Network > Advanced > Port Forwarding lalu atur port yang digunakan seperti gambar dibawah ini :
   
3. Login kedalam server menggunakan SSH. Untuk pengguna windows 10 bisa menggunakan aplikasi bash on ubuntu 

  ```
  $ ssh putri@localhost -p 2222
  ```
 4. Install Apache2 HTTP pada Ubuntu server
  ```
  $ sudo apt update
  $ sudo apt install apache2
  ```
   Setelah install Apache2, perintah dibawah ini dapat digunakan untuk memberhentikan, memulai dan mengaktifkan Apache2 pada server
  ```
  $ sudo systemctl stop apache2.service
  $ sudo systemctl start apache2.service
  $ sudo systemctl enable apache2.service
  ```
   Untuk memastikan Apache2 telah berhasil di install, bukalah http://localhost:8888 . JIka berhasil akan menampilkan seperti gambar dibawah ini
   <img src="https://raw.githubusercontent.com/adeliyay/bludit/master/Screenshot/39.PNG"></img>
   
5. Install PHP 7.2 dan modul lain yang diperlukan
  ```Tampilan *dashboard* admin
  $ sudo apt-get install software-properties-commonhttps://cdn.worldvectorlogo.com/logos/bludit.svg
  $ sudo add-apt-repository ppa:ondrej/php
  $ sudo apt update
  $ sudo apt install php7.2 libapache2-mod-php7.2 php7.2-common php7.2-mbstring php7.2-xmlrpc php7.2-soap php7.2-gd php7.2-xml php7.2-cli php7.2-curl php7.2-zip
  $ sudo apt-get install nano wget unzip git curl
  ```
   Setelah menginstall PHP 7.2, bukalah file konfigurasi php untuk Apache2.
  ```
  $ sudo nano /etc/php/7.2/apache2/php.ini
  ```
   Ubah pengaturan seperti dibawah ini dan simpan
  ```
  file_uploads = On
  allow_url_fopen = On
  memory_limit = 256M
  upload_max_filesize = 100M
  max_execution_time = 360
  date.timezone = America/Chicago
```
   Untuk memastikan PHP 7.2 telah terkoneksi dengan Apache2, buatlah  
  ```
  $sudo nano /var/www/html/phpinfo.php
  ```
   Lalu tuliskan seperti dibawah ini, lalu simpan
  ```
  <?php phpinfo( ); ?>
  ```
6. Lakukan update dan restart Apache2
  ```
  $ sudo apt-get update -y
  $ sudo apt-get upgrade -y
  $ sudo systemctl restart apache2.service
  ```
   Kemudian bukalah http://localhost:8888/phpinfo.php maka akan muncul gambar seperti dibawah ini :
   <img src="https://raw.githubusercontent.com/adeliyay/bludit/master/Screenshot/40.PNG"></img>
  
7. Download dan ekstrak instalasi Bludit dan pindahkan ke root directory
  ```
  $ cd /tmp && wget https://df6m0u2ovo2fu.cloudfront.net/builds/bludit-2-3-3.zip
  $ unzip bludit-2-3-3.zip
  $ sudo mv bludit-2-3-3 /var/www/html/bludit
  ```
8. Lalu ubah permission untuk Bludit agar dapat diakses
  ```
  $ sudo chown -R www-data:www-data /var/www/html/bludit/
  $ sudo chmod -R 755 /var/www/html/bludit/ 
  ```
9. Ubah konfigurasi Apache2 agar terkoneksi dengan Bludit dan simpan file
  ```
  $ sudo nano /etc/apache2/sites-available/bludit.conf
  ```
  ```
  <VirtualHost *:80>
     ServerAdmin localhost:8888
     DocumentRoot /var/www/html/bludit/
     ServerName localhost:8888
     ServerAlias localhost:8888

     <Directory /var/www/html/bludit/>
          Options FollowSymlinks
          AllowOverride All
          Require all grantedhttps://cdn.worldvectorlogo.com/logos/bludit.svg
     </Directory>

     ErrorLog ${APACHE_LOG_DIR}/error.log
     CustomLog ${APACHE_LOG_DIR}/access.log combined
    
     <Directory /var/www/html/bludit/>
            RewriteEngine on
            RewriteBase /
            RewriteCond %{REQUEST_FILENAME} !-f
            RewriteRule ^(.*) index.php [PT,L]
     </Directory>
  </VirtualHost>
  ```
10. Aktifkan Bludit Site dan Rewrite Module
  ```
  $ sudo a2ensite bludit.conf
  $ sudo a2enmod rewrite
  $ sudo a2enmod proxy proxy_fcgi rewrite
  $ sudo systemctl restart apache2.service
  ```
11. Buka http://localhost:8888 untuk menginstall bludit, kemudian akan diarahkan ke halaman instalasi Bludit seperti gambar dibawah. Pilih Bahasa yang diinginkan.
<img src="https://raw.githubusercontent.com/adeliyay/bludit/master/Screenshot/41.PNG"></img>

12. Kemudian buat *password* untuk admin.
<img src="https://raw.githubusercontent.com/adeliyay/bludit/master/Screenshot/42.PNG"></img>

13. Instalasi selesai


## Konfigurasi
[`^ kembali ke atas ^`](#)

- Konfigurasi Apache2
`$ sudo nano /etc/apache2/sites-available/bludit.conf`
	
	```
	<VirtualHost *:80>
     		ServerAdmin localhost:8888
     		DocumentRoot /var/www/html/bludit/
     		ServerName localhost:8888
     		ServerAlias localhost:8888

     		<Directory /var/www/html/bludit/>
          		Options FollowSymlinks
          		AllowOverride All
          		Require all granted
     		</Directory>

     		ErrorLog ${APACHE_LOG_DIR}/error.log
     		CustomLog ${APACHE_LOG_DIR}/access.log combined
    
     		<Directory /var/www/html/bludit/>
            		RewriteEngine on
            		RewriteBase /
            		RewriteCond %{REQUEST_FILENAME} !-f
            		RewriteRule ^(.*) index.php [PT,L]
    		</Directory>

	</VirtualHost>
	```

	```
	$ sudo a2ensite bludit.conf
	$ sudo a2enmod rewrite
	$ sudo a2enmod proxy proxy_fcgi rewrite
	$ sudo systemctl restart apache2.service
	```
## Cara Pemakaian
[`^ kembali ke atas ^`](#)

1. Akses Bludit melalui localhost anda dengan masuk ke localhost:8888
<img src="https://raw.githubusercontent.com/adeliyay/bludit/master/Screenshot/43.PNG"></img>

2. Untuk membuat konten baru, masuk ke halaman *dashboard* dengan cara klik `admin panel` dibagian `Create your own content` kemudian login dengan akun anda.
<img src="https://raw.githubusercontent.com/adeliyay/bludit/master/Screenshot/44.PNG"></img>

3. Tampilan *dashboard* admin
<img src="https://raw.githubusercontent.com/adeliyay/bludit/master/Screenshot/45.PNG"></img>

4. Untuk membuat post baru pilih menu `New content` pada bagian `PUBLISH`
<img src="https://raw.githubusercontent.com/adeliyay/bludit/master/Screenshot/27.PNG"></img>

5. Pilih `Save` untuk mempublish post
<img src="https://raw.githubusercontent.com/adeliyay/bludit/master/Screenshot/46.PNG"></img>

6. Untuk *manage posts* pilih menu `Content` pada bagian `MANAGE`
<img src="https://raw.githubusercontent.com/adeliyay/bludit/master/Screenshot/28.PNG"></img>

## Pembahasan
[`^ kembali ke atas ^`](#)

**Kelebihan**
- Tidak perlu melakukan  melakukan install maupun konfigurasi database,
- Menggunakan *flat-files* untuk penyimpanannya juga menggunakan format file JSON,
- Ada fitur CLI Mode (Command Line Interface Mode) dimana pengguna dapat membuat, mengubah atau menghapus *pages* atau *posts* tanpa harus melalui tampilan web,
- Web gratis dan *Open Source*,
- Terdapat dokumentasi pemakaian yang lengkap,
- Kostumisasi tema di blog/website 
- Bisa menambah berbagai macam plugin
- SEO friendly
- Support Markdown
- menjaga privacy dan keamanan dari data pengguna. Bluidit tidak mengambil data library eksternal, frameworks, dan resources lain

**Kekurangan**
- UI kurang menarik karena memakai warna dan ikon yang monoton,
- UI tidak dapat dapat membuat sendiri.

**Aplikasi yang sama**

**Grav**
Grav merupakan salah satu bentuk flat file CMS seperti bludit

Fitur:
1.	Markdown support
2.	Mudah dalam menginstall
3.	Grav mengubah konten menjadi caches sehingga mudah dalam pemrosesan
4.	Tipe Konten dinamis. Grav memudahkan pengguna untuk kostumisasi halaman, termasuk konten modular. 
5.	Multi-Language Support
6.	Simple Backups dan Restore
7.	Support image media processing seperti resize, crop, resample, dan menambahkan efek.
8.	Bisa kostumisasi tema

**Pico**
	Pico merupakan flat file CMS yang cepat dan digunakan oleh para developer. Dalam menginstal pico, dibutuhkan technical skill lebih banyak dari sekedar mengupload file.

Pico sangat simple. Dan bias digunakan dengan menggunakan file teks. Metadata yang digunakan adalah YAML.
Fitur: 
1.	Pico tidak menggunakan database, sehingga pemrosesan lebih cepat
2.	Menggunakan Twig untuk formatting
3.	Open source
4.	Dapat mengedit website hanya dengan menggunakan teks file
5.	Support markdown

## Referensi
[`^ kembali ke atas ^`](#)

[Install Bludit CMS On Ubuntu 18.04](https://websiteforstudents.com/install-bludit-cms-on-ubuntu-16-04-17-10-18-04-with-apache2-mariadb-php-7-2-and-lets-encrypt-ssl-tls/)

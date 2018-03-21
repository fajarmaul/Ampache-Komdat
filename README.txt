<h1 align="center"><img src="https://pbs.twimg.com/profile_images/472092114583433216/2czfO0xa_400x400.png" ><br>Ampache </h1>


[Sekilas Tentang](#sekilas-tentang) | [Instalasi](#instalasi) | [Konfigurasi](#konfigurasi) | [Otomatisasi](#otomatisasi) | [Cara Pemakaian](#cara-pemakaian) | [Pembahasan](#pembahasan) | [Referensi](#referensi)
:---:|:---:|:---:|:---:|:---:|:---:|:---:



# Sekilas Tentang
[`^ kembali ke atas ^`](#)

**Ampache** adalah sebuah aplikasi *streaming* audio/video yang berbasis web dan manajer file yang membuat penggunanya dapat mengakses musik dan video mereka dari mana saja. **Ampache** tersedia  hampir di seluruh perangkat yang mendukung jaringan internet.

Kegunaan **Ampache** terletak pada kemampuannya untuk melakukan ekstraksi *metadata* yang benar yang terdapat pada *tags* yang tersimpan di dalam file anda. **Ampache** bukanlah sebuah *media organiser* , namun merupakan sebuah alat untuk menyajikan kumpulan media yang telah terorganisir ke dalam bentuk yang lebih berguna bagi para penggunanya. 

`Version` : `v.8.7.0`

    acv

# Instalasi
[`^ kembali ke atas ^`](#)

#### Kebutuhan Sistem :
- Unix, Linux atau Windows.
- Web-Server :
	- Apache (Versi paling sering digunakan pada lebih banyak pada fase *testing*)
	- lighttpd
	- nginx
	- IIS
- PHP 5.4 atau versi lebih baru.
- Modul PHP :
	- PDO
	- PDO_MYSQL
	- hash
	- session
	- JSON
	- simplexml (opsional)
	- curl (opsional)
- MySQL 5.x


#### Proses Instalasi :
1. Dengan asumsi SSH sudah terpasang di server, lakukan login kedalam server menggunakan SSH. Untuk pengguna windows bisa menggunakan aplikasi [PuTTY](http://www.putty.org/) atau dengan menggunakan variasi dari linux shell yang tersedia untuk windows ( Contoh : [Git Bash](https://git-scm.com/) ). 

	Jika ssh belum terpasang 
	```
	sudo apt install ssh
	```
	Jika ssh sudah terpasang, lanjut ke proses di bawah ini
    ```
    $ ssh [server-username]@[server-ip-address] -p [port-number]
    
    Contoh :
	
	$ ssh student@127.0.0.1 -p 2222
    ```

2. Pastikan seluruh paket sistem kita *up-to-date*, dan install seluruh kebutuhan dasar sistem seperti `Apache`, `PHP`, dan `MySQL`. Kemudian install tools bantuan lainnya, yaitu `git` dan `composer` yang berguna dalam proses instalasi **Ampache**.
    ```
    $ sudo apt-get update
    $ sudo apt-get install apache2
    $ sudo apt-get install mysql-server
    $ sudo apt-get install php
    $ sudo apt-get install libapache2-mod-php
    $ sudo apt-get install php-mysql
    $ sudo apt-get install php-gd php-mcrypt php-mbstring php-xml php-ssh2 php-curl php-zip php-intl
    $ sudo apt install git
    $ sudo apt install composer
    $ sudo service apache2 restart
    ```
3. Clone **Ampache** dengan Git ke dalam direktori `var/ww/html/` .
    ```
    $ sudo git clone https://github.com/ampache/ampache.git
    ```

4.  Masuk ke dalam direktori `var/www/html/ampache`yang merupakan hasil dari  clone, lalu ubah permission dari folder `config` menjadi 777 (semua *user* dapat mengakses, menulis dan menjalankan) agar composer dan ampache dapat mengakses serta merubah isi file yang terdapat di dalam folder tersebut
	```
	$ sudo chmod 777 -R config 
	```
    

    
 5. Kembali ke root folder ampache kemudian lakukan instalasi *dependency* yang diperlukan oleh  **ampache** agar dapat berjalan di server dengan bantuan composer
	```
	$ sudo composer install
	```

5. Ubah otorisasi kepemilikan ke webserver yang berjalan (Apache2).
    ```
    //lakukan pada direktori /var/www/http untuk mengurangi resiko error
    $ sudo chown -R www-data:www-data ampache
    ```

    

6. Konfigurasi web server Apache agar **Ampache** dapat berjalan .
    ```
    $ sudo a2enmod rewrite
    $ sudo nano /etc/apache2/apache2.conf
	```
	Pada file `/etc/apache2/apache2.conf`, ubah `AllowOverride none` menjadi `AllowOverrie All`  pada bagian 
	```
	<Directory /var/www/>
	        Options Indexes FollowSymLinks
	        AllowOverride None
	        Require all granted
	</Directory>
    ```
    Menjadi
    ```
	<Directory /var/www/>
	        Options Indexes FollowSymLinks
	        AllowOverride All
	        Require all granted
	</Directory>
    ```
	Kemudian lakukan restart Apache
	```
	sudo service apache2 restart
	```

7. Mengubah maksimal ukuran file yang bisa di upload oleh apache.
  Edit file `etc/php/7.0/apache2/php.ini` dan ubah baris kode yang bersesuaian menjadi baris kode berikut :
    ```
    upload_max_filesize = 300M
    post_max_size = 300M                    
    ```

9. Restart kembali Apache web server.
    ```
    $ sudo service apache2 restart
    ```


10. Buka halaman IP web server yang kita gunakan di browser dan akses folder dimana kita melakukan instalasi **Ampache** .
	(Misal jika menggunakan IP localhost) : ``127.0.0.1:8888/ampache`` 
    - Pilihlah bahasa yang akan digunakan selama instalasi
   

      ![1](https://s14.postimg.org/veumveiup/setup_1.png)

	- Kemudian akan muncul halaman diama akan dilakukan pengecekan apakah seluruh *requirements* sudah terpenuhi

      ![2](https://i.pinimg.com/originals/7c/a8/e8/7ca8e8ccb01f01a561241e8e09f16eed.png)

    - Jika Semua kebutuhan sudah terpenuhi, maka seluruh status akan mengembalikan Nilai **OK**

      ![3](https://i.pinimg.com/originals/ce/64/91/ce64917e2617e8623d6176943a4f65dc.png)

    - Konfigurasi database yang akan digunakan oleh **Ampache**

      ![4](https://i.pinimg.com/originals/65/94/91/659491e8b7cca85cb44eff7cb46910c4.png)

    - Konfigurasi pembuatan *config file*

      ![5](https://i.pinimg.com/originals/94/e1/56/94e156de32e99c67bc92657649d3c577.png)

![6](https://i.pinimg.com/originals/9e/58/67/9e58676b4fbf301012a569c167de99ea.png)

![7](https://i.pinimg.com/originals/37/91/7d/37917dffa04b701c2a6c2d37c47e69bc.png)

Pada tabs *file insight*, dapat dilihat error-error seperti di gambar di bawah. Jika semua instalasi telah dilakukan dengan benar, ketika tombol *create config* diklik, maka file konfigurasi akan langsung tersimpan dan semua error tersebut akan teratasi.
![8](https://i.pinimg.com/originals/89/96/9e/89969eee870f60fe4206f19125feda23.png)

		 
  11. Membuat *user account* untuk *administrator*

   ![9](https://i.pinimg.com/originals/f0/f2/6a/f0f26a01c4efdb156f52fa6874e7cf52.png)

12. Setelah proses instalasi selesai, akan tampil halaman informasi versi dari Ampache, dan informasi update. Apabila ampache yang terinstall adalah versi terbaru dari github, maka tidak perlu dilakukan update.
    ![10](https://i.pinimg.com/originals/d3/f5/19/d3f519f844e2e1b444f742c7e883154b.png)
	Tombol update now hanya akan menampilkan bahwa ampache telah berada dalam versi yang terbaru.
13. Login ke dalam **Ampache** dengan menggunakan akun administrator yang telah dibuat sebelumnya.
	![11](https://i.pinimg.com/originals/93/59/f9/9359f99e0d8b61054df058761485aa02.png)

	**Ampache** telah berhasil terinstall 
	![12](https://i.pinimg.com/originals/d0/a1/09/d0a109b5572d4715fb93b4de9d299934.png)
	

# Konfigurasi
[`^ kembali ke atas ^`](#)

Pengaturan konfigurasi di **Ampache** terdapat pada tombol preferences (berbentuk seperti ikon *setting* pada umumnya) untuk pengguna, dan tombol admin untuk pengaturan server secara keseluruhan. 
1. Konfigurasi Pengguna
	- Menu yang dapat dikonfigurasi adalah interface (tampilan), Options (Pengaturan), Playlist, Streaming, serta Account.
    ![adv](https://i.pinimg.com/originals/a6/94/49/a69449e24d9d3156eeb2c62b83bd45a7.png)
    ![Konfigurasi](https://i.pinimg.com/originals/76/c4/08/76c408f29bb2484d1f73889e59a6a9d7.png)
	- Pada menu **Interface**, kita dapat mengatur tampilan pengguna dari **Ampache** seperti, mengatur untuk menampilkan *album art* , dan lain sebagainya.
	- Menu **Options** memiliki pengaturan tentang hal yang dapat user lakukan seperti share, streaming, video, dan lainnya.
	- Menu **Playlist** memiliki pengaturan untuk mengatur playlist yang akan digunakan di **Ampache**
	- Menu **Streaming** mengatur pengaturan soal *streaming*.
	- Menu **Account** untuk mengatur akun pengguna, seperti ubah password, email, dan lainnya.

2. Konfigurasi Server
	Untuk memilih konfigurasi server, klik tombol seperti server yang ada di sebelah tombol exit pada bagian pojok kiri layar **ampache**
	![Server-Config](https://i.pinimg.com/originals/46/ea/34/46ea3454a0c3dd1ff584678555ffbd2e.png)
	- Lalu pada tab **Server Config** di bagian pojok kiri bawah layar, terdapat pengaturan server yang dapat di pilih oleh pengguna atau administrator. Seperti website name, pemilihan bahasa, dan lain sebagainya yang terdapat pada menu interface.
	
	- Pengaturan sistem juga terdapat pada tab yang sama, yaitu pada bagian **System**. Pada menu ini dapat diatur  hal-hal yang berhubungan dengan server dari **Ampache** itu sendiri, seperti pengaturan *auto-update*, pengaturan metadata, lokasi penyimpanan catalog, dan lainnya.
	 ![System-server](https://i.pinimg.com/originals/29/68/f5/2968f52612be8fd1f3054c7dfbebf5a9.png)

3. Penambahan Modul
- **Ampache** didukung oleh banyak modul untuk menambah kenyamanan pengguna dalam menggunakan **Ampache**.   Pengaturan modul dan plugin dapat dilihat jika ikon seperti kepala kabel listrik di klik.
![Modules](https://i.pinimg.com/originals/13/9b/9d/139b9d66f479a1d1bb8156b3e5bdd9ca.png)


# Maintenance
[`^ kembali ke atas ^`](#)
Ada saatnya dimana **Ampache** perlu dilakukan *upgrade* untuk menjaga aplikasi yang terpasang tetap up-to-date dengan pengembangannya. Saat akan melakukan upgrade, ada baiknya server dimasukkan ke dalam mode *maintenance*, untuk menghindari pengguna lain melakukan kesalahan ketika **Ampache** sedang di *upgade*
Langkah-langkah membuat **Ampache** masuk kedalam mode *maintenance*
1. Buatlah file `.maintenance` pada root folder tempat instalasi **Ampache**. Contoh isi dari file tersebut terdapat di dalam file `.maintenance.example` pada root folder instalasi.
2. Jika ingin menambahkan pesan sendiri, harus ditambahkan *semicolon* `;` di akhir file.
![
](https://i.pinimg.com/originals/2b/f2/32/2bf232e14fbd46592549b1ba137bc4f9.png)

    



# Otomatisasi
[`^ kembali ke atas ^`](#)






# Cara Pemakaian
[`^ kembali ke atas ^`](#)

Cara pemakaian **CMS Prestashop** ini sangat mudah, karena aplikasi ini telah menyediakan *interface* yang mudah dimengerti. Berikut untuk lebih jelasnya :
1. Sebelum menggunakan prestashop, kita perlu login pada halaman admin toko kita.

    ![login](https://4.bp.blogspot.com/-rmIdzrb4t4E/WOGbktO4p8I/AAAAAAAAGlc/z1NraShhrpUt-4Z0MVfXofZ5IV9hR_XbwCLcB/s1600/presta1.png)

2. Setelah login, kita akan masuk ke halaman *Dashboard*. Disini kita dapat melihat laporan penjualan website kita baik harian, mingguan, bulanan, bahkan tahunan.

    ![mainpage](https://3.bp.blogspot.com/-AjB_6mJZCxc/WOGbk3XPniI/AAAAAAAAGlg/NGZ_vOSF1s4jvw9Yx9jh7odGt8B4qHSuwCLcB/s1600/presta2.png)

3. Pada bagian samping kiri, terdapat berbagai menu yang dapat kita gunakan. Menu **Order** berguna untuk mengetahui informasi lebih detail tentang penjualan pada website kita. Disini kita dapat mencetak *invoices, credit slips, delivery slips*, dan lain-lain.

    ![order](https://1.bp.blogspot.com/-0-MhQjYMGvQ/WOGbkmpYSXI/AAAAAAAAGlY/JSwla_Py4ckrxc839Im9JKtyJjsmye_hQCLcB/s1600/presta3.png)

4. Menu **Catalog** berguna untuk mengetahui informasi lebih detail tentang barang apa saja yang dijual pada website kita, kategori apa saja yang ada, mengawasi stok barang yang tersisa, merek apa saja yang ada, melihat daftar diskon, dan lain-lain.

    ![catalog](https://4.bp.blogspot.com/-PQTBdZzcMBY/WOGblDw9tYI/AAAAAAAAGlk/GDC7cEzfWmo9LZ8bqcqwnv7iilAYlXNhwCLcB/s1600/presta4.png)

5. Menu **Customers** berguna untuk melihat informasi lebih detail tentang daftar pelanggan kita dan alamatnya.

    ![customer](https://2.bp.blogspot.com/-Mtss0c3rblo/WOGblcDU_bI/AAAAAAAAGlo/u1Hv6LWzwtAEuk9_NPQHnedI6PnotkEMwCLcB/s1600/presta5.png)

6. Menu **Customer Service** berguna untuk mengatur hubungan dengan pelanggan, seperti menerima keluhan, mengirim pesan kepada pelanggan, pengembalian barang, dan lain-lain.

    ![cs](https://3.bp.blogspot.com/-ZlT_WQ0bpyk/WOGblYW-yPI/AAAAAAAAGls/gcZb8k36rEUn8dDFnGXl35R_Uhy_lNMeACLcB/s1600/presta6.png)

7. Menu **Stat** berguna untuk melihat informasi lebih detail dari website kita, seperti jumlah pengunjung setiap harinya, lokasi pelanggan terbanyak, barang apa yang populer, dan lain-lain.

    ![stat](https://3.bp.blogspot.com/-kvZ9MMH6Fxc/WOGblsk5jpI/AAAAAAAAGlw/p8LhO5LIvMU4WnDEF5NKg7--tKy8mWa-wCLcB/s1600/presta7.png)

8. Selain menu-menu yang berhubungan dengan penjualan, **Prestashop** juga menyediakan menu untuk meningkatkan performa website kita seperti menu untuk menginstal modul atau plugin, menu untuk memperindah tampilan website, menu untuk mengatur pengiriman dan pembayaran barang, bahkan menu lokalisasi untuk meningkatkan layanan pada region tertentu.

    ![improve](https://3.bp.blogspot.com/-d_DufFiAblk/WOGbls1KYxI/AAAAAAAAGl0/AsyvLt1zp1IP1BG1PbDaG91PJKV-xDkhACLcB/s1600/presta8.png)

9. Selain itu, terdapat juga menu untuk mempermudah konfigurasi website kita baik itu konfigurasi umum maupun konfigurasi lanjut.

    ![configure](https://4.bp.blogspot.com/-58ke7QyDUwQ/WOGbmCZvHFI/AAAAAAAAGl4/LuQRv6uYmywWjbzmJy82UwNu5qCg8fp1gCLcB/s1600/presta9.png)



# Pembahasan
[`^ kembali ke atas ^`](#)

**Prestashop** ditulis dalam bahasa pemrograman `PHP` yang support untuk penggunaan MySQL. Sebagai salah satu CMS yang paling banyak digunakan di dunia, aplikasi ini menawarkan berbagai kelebihan, diantaranya :
- Aplikasi memiliki panel administrasinya mudah digunakan dan fleksibel, sehingga dapat disesuaikan dengan kebutuhan.
- Mendukung berbagai layanan pembayaran utama, seperti `PayPal`, `VISA`, `MasterCard`, dan `Maestro`.
- Diterjemahkan dalam banyak bahasa, termasuk Bahasa Indonesia.
- Memiliki desain yang *responsive*, sehingga dapat dibuka menggunakan *device* apapun.
- Memiliki lebih dari tiga ratus fitur untuk memudahkan pengguna.
- Banyak pengguna yang berkontribusi pada *discussion boards* dan sejenisnya, sehingga masalah yang dihadapi pengguna dapat cepat terselesaikan.

Tentu saja, sebuah aplikasi pasti memiliki kekurangan. Kekurangan yang dimiliki **Prestashop** antara lain :
- Penggunaan fitur atau modul yang lengkap menyebakan proses loading dari aplikasi ini menjadi sangat lambat
- Penggunaan *resource* memory aplikasi ini cukup besar, terutama ketika menggunakan fitur atau modul yang lengkap.
- Sebagian besar modul dan tema yang tersedia tidak gratis.

Jika dibandingkan dengan CMS sejenisnya seperti **Microweber**, CMS ini memiliki beberapa keunggulan dan kelemahan. Berikut adalah beberapa perbandingan antara kedua CMS ini :
- **Microweber** menyediakan proses design yang fleksibel dengan fitur *Drag and Drop* tanpa batasan, sehingga pengguna bebas mengkreasikan tampilan websitenya. Sedangkan **Prestashop** hanya menyediakan fitur design berupa penggantian template dan logo, adapun template yang tersedia tidak gratis.
- Modul atau plugin yang tersedia pada **Prestashop** jauh lebih banyak dibandingkan pada **Microweber**.
- **Prestashop** memiliki pengguna yang jauh lebih banyak daripada **Microweber** yang aktif pada forum-forum diskusi untuk membantu pengguna pemula.
- **Microweber** lebih ringan daripada **Prestashop** karena modulnya yang sedikit.
- Proses instalasi **Prestashop** lebih mudah karena berbasis PHP saja, sedangkan **Microweber** menggunakan framework laravel sehingga proses instalasi lebih sulit, terutama dalam hal *dependency*.



# Referensi
[`^ kembali ke atas ^`](#)

1. [Github Official Installation Guide](https://github.com/ampache/ampache/wiki/Installation) - Ampache
2. [How to Log Into a VPS with PuTTY on Windows](https://www.digitalocean.com/community/tutorials/how-to-log-into-a-vps-with-putty-windows-users) - DigitalOcean
3. [Change The Maximum File Upload Size on PHP](https://stackoverflow.com/questions/2184513/change-the-maximum-upload-file-size) - Stack Overflow
4. [Empty](https://google.com) - PrestaShop
5. [Ampache Maintenance Mode](https://github.com/ampache/ampache/wiki/Upgrade) - GitHub
6. [Apache Set AllowOverride](https://stackoverflow.com/questions/18740419/how-to-set-allowoverride-all) - Stack Overflow

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzMDIwNDI3NTgsNTIyOTM4ODA1LDEyOT
U3NTAwMDYsLTgyMTMwNTc5Nl19
-->
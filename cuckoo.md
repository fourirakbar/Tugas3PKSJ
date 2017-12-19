# Tahap Pertama (Instalasi)

## Langkah - langkah menginstal Cuckoo

1. Dalam tugas ini, kelompok kami menggunakan OS ubuntu 16.04 untuk menginstall cuckoo. Pertama tama install beberapa kebutuhan untuk menginstall cuckoo dengan mengetikkan ```sudo apt-get install python python-pip python-dev libffi-dev libssl-dev```
![1](/cuckoo/1.png)
<br>

2. Lalu install ```sudo apt-get install libxml2-dev libxsli-dev```
![1](/cuckoo/2.png)
<br>

3. Selagi menunggu instalasi, download cuckoo-current pada ```https://downloads.cuckoosandbox.org```. Pilih yg cuckoo-current.tar.gz
![1](/cuckoo/3.png)
<br>

4. Setelah selesai mendownload cuckoo-current, maka ekstrak hasil downloadan Anda dengan mengetikkan ```tar-xvzf cuckoo-current.tar.gz```
![1](/cuckoo/4.png)
<br>

5. Download virtualbox di halaman resmi virtualbox (disini kelompok kami menggunakan virtualbox5-1), dan buat sebuah os virtual. Kelompok kami menggunakan windows7 untuk os virtual
![1](/cuckoo/5.png)
![1](/cuckoo/6.png)
<br>

6. Lalu atur besar memory, tipe harddisk, dan storagenya
![1](/cuckoo/7.png)
![1](/cuckoo/8.png)
![1](/cuckoo/9.png)
![1](/cuckoo/10.png)
<br>

7. Setelah itu masukkan .iso dari windows7, dengan cara klik setting > storage
![1](/cuckoo/11.png)
<br>

8. Setelah selesai, jalankan os virtual yang barusan Anda buat. Setelah itu install windows7 seperti biasa.
![1](/cuckoo/12.png)
<br>

9. Install mongodb, tcpdump, libcap2-bin pada host Anda
![1](/cuckoo/14.png)
![1](/cuckoo/15.png)
![1](/cuckoo/16.png)
<br>

7. Konfigurasi Tcpdump
![1](/cuckoo/17.png)
<br>

8. Clone volatility dari repo, dan juga install volatility. Volatility merupakan salah satu tools forensic untuk menganalisa hasil dari memory dumps, bisa dijalankan di windows, linux, mac, maupun di android. Volatility juga support untuk 32-bit dan juga 64.0bit.
![1](/cuckoo/18.png)
![1](/cuckoo/19.png)
<br>

9. Download distrom dari reponya. Disini kelompok kami menggunakan distrom terbaru, versi 3.3.4
![1](/cuckoo/20.png)
<br>

10. Setelah selesai download, ekstrak file distrom tersebut. Setelah selesai ekstrak, maka install
![1](/cuckoo/21.png)
![1](/cuckoo/22.png)
<br>

11. Download Yara dari reponya. Disini kelompk kami menggunakan yara terbaru, versi 3.7.0
![1](/cuckoo/26.png)
<br>

12. Setelah itu, ekstrak yara yang barusan Anda download
![1](/cuckoo/27.png)
<br>

13. Masuk ke direktori yara, lalu install dengan command seperti pada gambar dibawah
![1](/cuckoo/28.png)
![1](/cuckoo/29.png)
<br>

14. Download pycrypto pada web resminya
![1](/cuckoo/30.png)
<br>

15. Ekstrak, lalu masuk ke direktori pycrpyto. Setelah itu lakukan build dan install.
![1](/cuckoo/31.png)
![1](/cuckoo/32.png)
<br>

16. Setelah itu download openpyxl, ujson, jupyter dengan menggunakan python pip
![1](/cuckoo/33.png)
![1](/cuckoo/34.png)
![1](/cuckoo/36.png)
<br>

17. Setelah itu install mitmproxy
![1](/cuckoo/38.png)
<br>

18. Pindah ke direktori mitmproxy dengan mengetikkan ```cd ~/.mitmproxy```, lalu copy file mitmproxy-ca-cert.p12
![1](/cuckoo/39.png)
<br>

19. Coba Anda ketikkan mitmdump = /usr/local/bin/mitdump, maka akan muncul seperti pada gambar dibawah
![1](/cuckoo/40.png)
<br>

# Tahap Kedua (Konfigurasi)

## Langkah - langkah konfigurasi Cuckoo

1.  Pindah ke direktori cuckoo/conf yg sudah Anda download.
<br>

2. Edit file cuckoo.conf dengan mengetikkan ```nano cuckoo.conf```. Edit seperti pada gambar dibawah
![1](/cuckoo/41.png)
Ubah memory_dump = on
![1](/cuckoo/42.png)
Ubah IP menjadi IP host Anda
<br>

3. Edit file auxilary.conf dengan mengetikkan ```nano auxilary.conf```. Edit seperti pada gambar dibawah
![1](/cuckoo/43.png)
Pada [mitm], rubah enabled = yes
<br>

4. Edit file virtualbox.conf dengan mengetikkan ```nano virtualbox.conf```. Edit seperti pada gambar dibawah
![1](/cuckoo/44.png)
<br>

5. Edit file processing.conf dengan mengetikkan ```nano processing.conf```. Edit seperti pada gambar dibawah
![1](/cuckoo/45.png)
Pada [memory] dirubah enabled = yes
<br>

6. Edit file memory.conf dengan mengetikkan ```nano memory.conf```. Edit seperti pada gambar dibawah
![1](/cuckoo/46.png)
![1](/cuckoo/47.png)
<br>

7. Konfigurasi pada cuckoo telah selesai, setelah itu pindah ke direktori cuckoo/web, dan jalankan perintah ```sudo ./manage.py runserver```.
![1](/cuckoo/48.png)
<br>

8. Pada poin no 7 terlihat ketika Anda akan menjalankan web dari cuckoo akan terdapat error, itu disebabkan karena ada requirements yang belum terinstall. Ketikkan ```sudo pip install --upgrade html5lib==1.0b8```.
![1](/cuckoo/49.png)
<br>

9. Setelah itu coba jalankan kembali command di no 7.
![1](/cuckoo/50.png)
<br>

10. Coba buka 127.0.0.1:8000 pada browser Anda. Maka akan muncul seperti pada gambar dibawah.
![1](/cuckoo/51.png)
<br>

# Tahap Ketiga (Menyiapkan Guest OS)

## Langkah - langkah menyiapkan guest OS

Kelompok kami menggunakan windows 7 yang di install di virtualbox untuk guest OS. Windows 7 yang akan digunakan sebagai guest OS sudah di install pada tahap pertama.

1. Jalankan OS windows 7 yang sudah Anda buat. Setelah itu matikan firewall dengan mengetikkan ```firewall``` di start menu. Lalu akan muncul seperti gambar dibawah, dan pilih ```Turn off Windows Firewall```.
![1](/cuckoo/52.png)
<br>

2. Buka kembali direktori cuckoo yang sudah Anda download pada tahap pertama. Di dalam direktori cuckoo akan terdapat direktori agent. Buat shared folder dengan cara klik kanan > propertis > local network share. Setelah itu akan ada peringantan untuk menginstall samba terlebih dahulu, klik install.
![1](/cuckoo/54.png)
<br>

3. Buka setting host os virtual windows 7 yang sudah Anda buat, lalu klik shared folders. Setealh itu pilih machine folders, dan arahkan folder path ke folder cuckoo/agent pada host Anda. Pilih auto-mount dan make permanent juga.
![1](/cuckoo/55.png)
![1](/cuckoo/56.png)
<br>

4. Pada os virtual windows 7, download python2.7 pada web resmi python, dan install.
![1](/cuckoo/57.png)
![1](/cuckoo/58.png)
<br>

5. Download adobe pada web resminya, dan install.
![1](/cuckoo/59.png)
<br>


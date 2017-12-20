# Snort

* Instalasi Snort pada Ubuntu
* Konfigurasi Snort
* Uji Coba Snort

-----------------

#### Instalasi Snort pada Ubuntu 
-----------------

##### **Mempersiapkan Server**
1. Sebelum menginstal Snort, instal file - file yang dibutuhkan menggunakan command di bawah ini
    ```
    sudo apt install -y gcc libprec3-dev zlib1g-dev libpcap-dev openssl libssl-dev libnghttp2-dev libdumbnet-dev bison flex libdnet
    ```
##### **Instal Snort**
1. Buat folder download sementara yang menuju ke direktori home, lalu pindah ke direktori tersebut.
    ```
    mkdir ~/snort_src && cd ~/snort_src
    ```

2. Unduh Data Aquisition (DAQ) source package yang berguna untuk memanggil library packet capture dari website Snort menggunakan command wget.
    ```
    wget https://www.snort.org/downloads/snort/daq-2.0.6.tar.gz
    ```

3. Apabila download sudah selesai, ekstrak source code tersebut dan pindah ke direktori baru dengan command berikut:
    ```
    tar -xvzf daq-2.0.6.tar.gz
    cd daq-2.0.6
    ```

4. Jalankan skrip konfigurasi, kemudian compile program menggunakan command <b>make</b>  lalu install DAQ
    ```
    ./configure && make && sudo make install
    ```

5. Setelah menjalankan langkah 5, kembali lagi ke folder tempat Snort diunduh
    ```
    cd ~/snort_src
    ```

6. Selanjutnya, unduk Snort menggunakan wget seperti command di bawah ini.
    ```
    wget https://www.snort.org/downloads/snort/snort-2.9.11.tar.gz
    ```

7. Setelah selesai, extract paket menggunakan tar, diikuti pidnah ke direktori baru
    ```
    tar -xvzf snort-2.9.11.tar.gz
    cd snort-2.9.11
    ```

8. Lakukan konfigurasi instalasi dengan mengaktifkan sourcefire, lalu jalankan <b>make & make install</b>
    ```
    ./configure --enable-sourcefire && make && sudo make install
    ```

9. Langkah yang harus dilakukan selanjutnya adalah melakukan konfigurasi Snort. Update libraries menggunakan command di bawah ini
    ```
    sudo ldconfig
    ```

10. Snort pada Ubuntu akan terinstall menuju direktori **/usr/local/bin/snort**, tetapi sebagai latihan, buat <i>symbolic link</i> menuju direktori **/usr/sbin/snort**
    ```
    sudo ln -s /usr/local/bin/snort /usr/sbin/snort
    ```
##### **Setting Username dan Struktur Folder**

1. Untuk menjalankan Snort pada Ubuntu dengan aman tanpa akses root, gunakan command dibawah ini
    ```
    sudo groupadd snort
    sudo useradd snort -r -s /sbin/nologin -c SNORT_IDS -g snort
    ```

2. Kemudian buat struktur folder menuju tempat dimana konfigurasi Snort berada. Ikuti command di bawah ini:
    ```
    sudo mkdir -p /etc/snort/rules
    sudo mkdir /var/log/snort
    sudo mkdir/usr/local/lib/snort_dynamicrules
    ```

3. Ubah permission untuk direktori baru secara urut seperti yang tertera di bawah ini:
    ```
    sudo chmod -R 5775 /etc/snort
    sudo chmod -R 5775 /var/log/snort
    sudo chmod -R 5775 /usr/local/lib/snort_dynamicrules
    sudo chown -R snort:snort /etc/snort
    sudo chown -R snort:snort /var/log/snort
    sudo chown -R snort:snort /usr/local/lib/snort_dynamicrules
    ```

4. Buat file baru untuk rules pada Snort
    ```
    sudo touch /etc/snort/rules/white_list.rules
    sudo touch /etc/snort/rules/black_list.rules
    sudo touch /etc/snort/rules/local.rules
    ```

5. Salin file konfigurasi dari folder unduhan Snort
    ```
    sudo cp ~/snort_src/snort-2.9.11/etc/*.conf* /etc/snort
    sudo cp ~/snort_src/snort-2.9.11/etc/*.map /etc/snort
    ```
6. Cek apakah instalasi sudah benar, apabila sudah benar tampilan akan seperti dibawah ini
    ```
    snort -V
    ```
    ![6](/snort/snort1.png)

-----------------

#### Konfigurasi Snort
-----------------
##### **Menggunakan Community Rules**
1. Ambil Community Rules menggunakan wget seperti command di bawah ini
    ```
    wget https://www.snort.org/rules/community -O ~/community.tar.gz
    ```

2. Ekstrak rules dan copy menuju folder konfigurasi
    ```
    sudo tar -xvf ~/community.tar.gz -C ~/
    sudo cp ~/community-rules/* /etc/snort/rules
    ```

3. Comment beberapa baris yang tidak terlalu dibutuhkan menggunakan sed dengan cara di bawah ini:
    ```
    sudo sed -i 's/include \$RULE\_PATH/#include \$RULE\_PATH/' /etc/snort/snort.conf
    ```
##### **Konfigurasi Jaringan dan Rules**


1. Edit file snort.conf untuk memodifikasi beberapa parameter. Buka file tersebut dengan file editor menggunakan command dibawah ini
    ```
    sudo nano /etc/snort/snort.conf
    ```
    ![1](/snort/snort2.png)<br>


2. Cari section dibawah ini untuk mengubah beberapa parameter 

    ```
    # Setup the network addresses you are protecting
    ipvar HOME_NET <server public IP>/32
    ```
   

    ```
    # Set up the external network addresses. Leave as "any" in most situations
    ipvar EXTERNAL_NET !$HOME_NET
    ```
     ![1](/snort/snort3.png)<br>

    ```
    # Path to your rules files (this can be a relative path)
    var RULE_PATH /etc/snort/rules
    var SO_RULE_PATH /etc/snort/so_rules
    var PREPROC_RULE_PATH /etc/snort/preproc_rules
    ```

    ```
    # Set the absolute path appropriately
    var WHITE_LIST_PATH /etc/snort/rules
    var BLACK_LIST_PATH /etc/snort/rules
    ```
     ![1](/snort/snort5.png)<br>

3. Masih pada file snort.conf, cari section 6 dan set output untuk unified2 menuju snort.log  seperti dibawah ini
    ```
    # unified2
    # Recommended for most installs
    output unified2: filename snort.log, limit 128
    ```
     ![1](/snort/snort6.png)<br>

4. Uncomment local.rules untuk memperbolehkan adanya perubahan rules pada Snort 
    ```
    include $RULE_PATH/local.rules
    ```
     ![1](/snort/snort7.png)<br>

5. Terakhir, apabila menggunakan Comunity Rules, tambahkan baris dibawah ini. Jika sudah, simpan perubahan konfigurasi.
    ```
    include $RULE_PATH/community.rules
    ```
    ![1](/snort/snort8.png)<br>

##### **Validasi Konfigurasi**
1. Cek konfigurasi menggunakan parameter -T untuk mengaktifkan mode test.
    ```
    sudo snort -T -c /etc/snort/snort.conf
    ```
    Apabila konfigurasi berhasil maka akan muncul tampilan seperti di bawah ini:
    ![1](/snort/snort9.png)
    
##### **Tes Konfigurasi**
1. Untuk mengecek apakah Snort mencatat peringatan, tambah rule custom apabila ada ping (koneksi ICMP) yang mengarah ke file local.rules
    ```
    sudo nano /etc/snort/rules/local.rules
    ```

2. Tambahkan baris berikut ke dalam file local.rules
    ```
    alert icmp any any -> $HOME_NET any (msg:"ICMP test"; sid:10000001; rev:001;)
    ```

3. Jalankan Snort dengan -A untuk mencetak hasil alert ke stdout. 
    ```
    sudo snort -A console -i eth0 -u snort -g snort -c /etc/snort/snort.conf
    ```
    ![1](/snort/snort10.png)

4. Coba ping server Anda dari server lain. 
    ![1](/snort/snort11.png)

5. Apabila konfigurasi berhasil maka akan muncul alert pada server Anda
    ![1](/snort/snort12.png)

-----------------

#### Ujicoba Snort
-----------------

1. Download file pcap dari situs http://www.secrepo.com/
    ![1](/snort/snort13.png)

2. Jalankan command ini untuk membaca file pcap yang sudah diunduh
    ```
    sudo snort -c /etc/snort/[nama_rules] -r [file.pcap] -b -l 
    ```
    ![1](/snort/snort14.png)

3. Hasil akan muncul di file bernama alert
    ![1](/snort/snort15.png)

Resources: https://www.upcloud.com/support/installing-snort-on-ubuntu/#community-rules

https://www.youtube.com/watch?v=qGATEsSO4QY

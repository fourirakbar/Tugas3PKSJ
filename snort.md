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
##### **Konfigurasi Snort (NIDS Mode)**

1. Langkah yang harus dilakukan selanjutnya adalah melakukan konfigurasi Snort. Update libraries menggunakan command di bawah ini
    ```
    sudo ldconfig
    ```

2. Snort pada Ubuntu akan terinstall menuju direktori **/usr/local/bin/snort**, tetapi sebagai latihan, buat <i>symbolic link</i> menuju direktori **/usr/sbin/snort**
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
    ![1](/snort/snort1.png)

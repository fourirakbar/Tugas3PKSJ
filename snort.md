# Snort

* Instalasi Snort pada Ubuntu
* Konfigurasi Snort
* Uji Coba Snort

-----------------

#### Instalasi Snort pada Ubuntu 

1. Sebelum menginstal Snort, instal file - file yang dibutuhkan menggunakan command di bawah ini
    ```
    sudo apt install -y gcc libprec3-dev zlib1g-dev libpcap-dev openssl libssl-dev libnghttp2-dev libdumbnet-dev bison flex libdnet
    ```

2. Buat folder download sementara yang menuju ke direktori home, lalu pindah ke direktori tersebut.
    ```
    mkdir ~/snort_src && cd ~/snort_src
    ```

3. Unduh Data Aquisition (DAQ) source package yang berguna untuk memanggil library packet capture dari website Snort menggunakan command wget.
    ```
    wget https://www.snort.org/downloads/snort/daq-2.0.6.tar.gz
    ```

4. Apabila download sudah selesai, ekstrak source code tersebut dan pindah ke direktori baru dengan command berikut:
    ```
    tar -xvzf daq-2.0.6.tar.gz
    cd daq-2.0.6
    ```

5. Jalankan skrip konfigurasi, kemudian compile program menggunakan command <b>make</b>  lalu install DAQ
    ```
    ./configure && make && sudo make install
    ```

6. Setelah menjalankan langkah 5, kembali lagi ke folder tempat Snort diunduh
    ```
    cd ~/snort_src
    ```

7. Selanjutnya, unduk Snort menggunakan wget seperti command di bawah ini.
    ```
    wget https://www.snort.org/downloads/snort/snort-2.9.11.tar.gz
    ```

8. Setelah selesai, extract paket menggunakan tar, diikuti pidnah ke direktori baru
    ```
    tar -xvzf snort-2.9.9.0.tar.gz
    cd snort-2.9.9.0
    ```

9. Lakukan konfigurasi instalasi dengan mengaktifkan sourcefire, lalu jalankan <b>make & make install</b>
    ```
    ./configure --enable-sourcefire && make && sudo make install
    ```
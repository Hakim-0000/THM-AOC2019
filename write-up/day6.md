download data yang dibutuhkan untuk menyelesaikan task.

<br>Q1:What data was exfiltrated via DNS?
<br>Q2:What did Little Timmy want to be for Christmas?
<br>Q3:What was hidden within the file?

<br>A1:
<br>buka file .pcap yang telah didownload menggunakan wireshark dengan command
```shell
wireshark file.pcap
```
selanjutnya, pada bagian filter masukkan dns. lalu akan terlihat pada tab source dan 
destination bahwa IP 1.1.1.1 mengirimkan data ke IP 192.168.1.107, ketika di follow,
data tersebut berupa `<hex>.holidaythief.com`, karena informasi tersebut ter-encrypt
menggunakan hex, maka kita dapat melakukan decrypt hex tersebut, disini saya menggunakan
tools [cyberchef](cyberchef.org). pada bagian recipe masukkan HEX, dan pada bagian input
masukkan encrypted data tersebut, maka akan keluar hasil decrypted data berupa kalimat.

<br>A2:
<br>selanjutnya kita dapat extract object yang ada pada file .pcap ini. dengan cara
`File>Export Object>HTTP` akan ada 2 file menarik yang ter ekstrak. yaitu 
`christmaslist.zip` dan `TryHackMe.jpg`. ternyata file `christmaslist.zip` membutuhkan
password untuk ekstrak. kita dapat menggunakan tool `zip2john` untuk mengambil informasi
hash, dengan command
```shell
zip2john christmaslist.zip > hash
```
lalu cek file hash dengan `cat` apakah sudah berisi informasi hash. jika sudah, kita dapat
langsung bruteforce passwordnya dengan menggunakan tools `john` atau john the ripper,
dengan command
```shell
john hash
```
kita dapatkan password untuk zip tersebut. kita dapat gunakan `unzip` untuk extrak isinya
```shell
unzip christmaslist.zip
	password: De...
```
selanjutnya, karena pertanyaannya adalah apa harapan Timmy, maka kita `cat` file Timmy
```shell
cat christmaslisttimmy.txt
```

<br>A3:
<br>untuk melihat informasi tersembunyi dari file `TryHackMe.jpg` kita dapat menggunakan
tool `steghide`. dengan cara
```shell
steghide extract -sf ./TryHackMe.jpg
```
tanpa passphrase.

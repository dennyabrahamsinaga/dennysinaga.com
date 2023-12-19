---
title: "[CTF - Lokal] CTF SecureTheSystem 2023"
date: 2023-12-19T14:28:17+07:00
draft: false
---

# CTF Secur{i}e The System 2023

This post will be written in Bahasa Indonesia.

## Forensic

### keylogger

> So, here's the thing. I just came to the Internet caffee (warnet) and after several minutes using the computer, I found a strange process running in the backround. Then i found out it was a keylogger. Luckily I can stop the process and don't let the attacker get this keylogger file. Because i just logged in to my server before.
>
> flag = STS23{`<my server password>`}

Pada challenge ini, kita diberikan sebuah `log.pcapng` file.
Untuk menyelesaikan challenge ini, saya menggunakan [video](https://www.youtube.com/watch?v=EnOgRyio_9Q) ini sebagai referensi karena _case_ yang dibahas kurang lebih sama.

Merujuk pada video tersebut, terdapat sebuah [notes](https://github.com/carlospolop-forks/ctf-usb-keyboard-parser) yang dapat kita gunakan. 
Pertama, kita perlu memfilter (filtering) USB Keyboard Packets pada Wireshark dengan menggunakan filter berikut ini:

`((usb.transfer_type == 0x01) && (frame.len == 35)) && !(usb.capdata == 00:00:00:00:00:00:00:00)`

Kedua, kita pilih semua paket `(Ctrl+A)` yang ditampilkan berdasarkan filter di atas. 
Lalu, klik File > Export Specified Packets dan Save.

Kemudian, pada [notes](https://github.com/carlospolop-forks/ctf-usb-keyboard-parser) yang sebelumnya disebutkan kita dapat menggunakan command di bawah ini:

`tshark -r rill.pcapng -Y 'usb.capdata && usb.data_len == 8' -T fields -e usb.capdata | sed 's/../:&/g2' > new_data.txt`

Keterangan:
- `rill.pcapng` adalah file pcapng yang sudah kita filter sebelumnya.
- `usb.capdata && usb.data_len == 8`, filter ini akan menampilkan _USB packets_ dimana terdapat `usb.capdata`, dan panjang dari _USB data_ adalah 8 bytes.
- `sed 's/../:&/g2' > new_data.txt`, command ini akan menyisipkan tanda titik dua atau `(:)` di antara setiap dua karakter dalam _USB data field_ dan mengarahkan output akhir ke file bernama `new_data/txt`. 

Hasil dari command di atas kira-kira sebagai berikut:

```
00:00:10:00:00:00:00:00
00:00:10:12:00:00:00:00
00:00:12:00:00:00:00:00
00:00:00:00:00:00:00:00
00:00:11:00:00:00:00:00
00:00:11:0e:00:00:00:00
00:00:0e:00:00:00:00:00
00:00:00:00:00:00:00:00
00:00:08:00:00:00:00:00
00:00:00:00:00:00:00:00
00:00:17:00:00:00:00:00
00:00:00:00:00:00:00:00
00:00:2a:00:00:00:00:00
00:00:00:00:00:00:00:00

....

00:00:00:00:00:00:00:00
```

Isi file tersebut tidak ditampilkan sepenuhnya karena hasilnya cukup panjang.

Selanjutnya, kita tinggal menggunakan tools yang disediakan pada repo tersebut. 
Kita dapat menggunakan command berikut:
`python3 usbkeyboard.py out.txt`

Hasilnya adalah sebagai berikut:

```
monket⌫ytype.com
point muslate all pu⌫pss
this at will man llke ely here call want must word come houseting sa⌫till man sinc shoold come first mean become fro that
on fisr⌫⌫
last with also of⌫⌫if problem such off kow possble wy p







down d
she fact us cil









my enter butti⌫on suddently not working <<>⌫>⌫⌫⌫⌫⌫⌫⌫⌫⌫⌫⌫⌫⌫⌫⌫⌫⌫⌫⌫wat the ****



hus⌫ft ...


yey



yo now ⌫⌫⌫⌫⌫kow what⌫?/?????



frfr



i also doo⌫n't really kow :v

bro, lets play brawlhalla?
what ⌫> ⌫⌫?
o o ... gws

i might s⌫gonna try t deploy my we lmao

ssh kyruuu⌫⌫⌫⌫⌫⌫⌫⌫⌫

ssh kyruuu@mydomain.id

th111⌫⌫s




ssh kyruuu@mydomain.id
th1smys3cre⌫etp@sswwo⌫0rd
th1smys3cretpa⌫@ssw0⌫o⌫0rd






ls -la
cat .ssh/auth


cd /var/www/html/⌫/
vi index.html

i <p> Hello, world!</p>
␛:q⌫wq!⌫
cler


cd




exit

monkeytype
⌫⌫⌫mydomain.id
monkeytype.com
well kow to about sme t⌫so then nati like last befor grop about during end back call leave o unde becomeetissu
st



















both fel
mus
own work into firs time gee
incr ely
w s
see ca
one right only however goodnew mos snd from
wher call find numbeleaddjus these for write justeye
for throogh leave grate
than take one feel in during ting rn to justarond how int
during there pen be jusand wy what by not help work day howe ⌫⌫ ye anddmay real d⌫want life leave the such
```

Berdasarkan hasil di atas, ditemukan dua kandidat password yang kita cari:
- th1smys3cre⌫etp@sswwo⌫0rd
- th1smys3cretpa⌫@ssw0⌫o⌫0rd

Setelah menyesuaikan password dengan benar, maka final flagnya adalah: 
**STS23{th1smys3cretp@ssw0rd}**

## Web

### Notes Manager

> You are a penetration tester hired by a small company who got their website hacked recently. They said the hacker somehow got administrative privilege to the website, but there were no logs indicated that an our main admin account was used in other IP Address else than our administrator IP Address. Can you help this company to find the vulnerabilities so that they can patch it ASAP?

Pada challenge ini kita diberikan sebuah website dengan fitur _register_ dan _login_.

![notes-manager](https://gcdnb.pbrd.co/images/egBC6LCyTKgH.png?o=1)

Pertama-tama, saya mendaftar ke platform ini dan kemudian _login_ menggunakan kredensial yang digunakan saat registrasi.
Setelah _login_, kita menemukan bahwa terdapat beberapa menu seperti:
- Create Notes
- Notes  
- Settings

![dashboard](https://gcdnb.pbrd.co/images/BK0Yyuum63lD.png?o=1)

Notes saat `role` adalah `user`:

![notes-before-admin](https://gcdnb.pbrd.co/images/x0x5UjoSSx0X.png?o=1)

Pada menu Settings, kita dapat melakukan Update Profile.
Ketika mengupdate profil, saya menemukan ternyata terdapat parameter `role` yang memiliki nilai `user` pada response.

![response-body](https://gcdnb.pbrd.co/images/pxWe9T88tU9I.png?o=1)

Kemudian saya mencoba mengubah `role` yang kita miliki menjadi admin dengan menambahkan parameter `role` dengan nilai `admin`. 
Untuk melakukan ini, saya menggunakan curl:

```
curl 'http://178.128.113.198:1234/setting/update-profile' \
  -H 'Accept: */*' \
  -H 'Accept-Language: en-US,en;q=0.9' \
  -H 'Connection: keep-alive' \
  -H 'Content-Type: application/x-www-form-urlencoded; charset=UTF-8' \
  -H 'Cookie: session=eyJ1aWQiOjEyMH0.ZYFdNw.yQeaVm4tVecNn0Yjx90atJW--4c' \
  -H 'Origin: http://178.128.113.198:1234' \
  -H 'Referer: http://178.128.113.198:1234/setting' \
  -H 'User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36' \
  -H 'X-Requested-With: XMLHttpRequest' \
  --data-raw 'name=rill&gender=female&role=admin' \
  --compressed \
  --insecure
```

Setelah profil berhasil diupdate, kita dapat melihat daftar _notes_ pada menu Notes.

![notes-after-admin](https://gcdnb.pbrd.co/images/QIKhZDMJw2ot.png?o=1)

Terdapat sebuah notes dengan judul FLAG yang diproteksi dengan sebuah password.

Notes di atas ditulis oleh author dari challenge ini, yaitu aimardcr. 
Karena notes tersebut ditulis oleh author challenge ini saya berasumsi bahwa terdapat informasi berupa _hints_ yang dapat kita gunakan untuk mendapatkan flag.

Pada notes dengan judul `TODO`, kita dapat melihat isi notes tersebut yang menurut saya adalah _hints_ berisi _vulnerabilities_ pada challenge ini:

![notes-todo](https://gcdnb.pbrd.co/images/lECJS7FUl38F.png?o=1) 

Dengan hint berupa IDOR yang disebutkan dalam notes tersebut, kita dapat mengganti UUID notes lain dengan UUID notes FLAG. 
Untuk mengetahui UUID notes FLAG, kita dapat menggunakan Inspect Element untuk melihat UUID-nya:

![inspect-flag-uuid](https://gcdnb.pbrd.co/images/zctg1hcRXiXI.png?o=1)

Saya mengganti UUID notes TODO dengan UUID notes FLAG:
- TODO Notes: `http://178.128.113.198:1234/notes/bb06631b-9b72-424a-b8ec-fb815eb24a75`
- FLAG Notes: `http://178.128.113.198:1234/notes/a6faca0d-7e51-4b02-834f-c7b11370056f`

Akhirnya, kita mendapatkan flag.

![final-flag](https://i.ibb.co/mqVKFvJ/image.png)

**STS23{Bl4ckb0x_Ch4ll_F0r_An0th3r_D4y}**


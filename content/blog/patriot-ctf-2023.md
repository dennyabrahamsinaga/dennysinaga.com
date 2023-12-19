---
title: "[CTF] PatriotCTF 2023"
date: 2023-12-19T09:45:04+07:00
draft: false
---

# PatriotCTF - 2023

URL: [CTFtime.org](https://ctftime.org/event/2030)

## Crypto

### 1. Multi-numeral

![multi-numeral](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Crypto/Multi-numeral/image.png)

You are given a text file containing binary strings. You can use CyberChef to decode the strings:

![cyberchef](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Crypto/Multi-numeral/image-1.png)


flag: PCTF{w0w_s0_m4ny_number5}

### 2. My phone!

![my-phone](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Crypto/My%20phone!/image.png)

We were provided with an image file that appeared to contain an unusual cipher. To decrypt the message within the image, I utilized the [Gravity Falls Bill Cipher tool](https://www.dcode.fr/gravity-falls-bill-cipher).

By decoding the message from the image, I obtained a result like this:

![results](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Crypto/My%20phone!/image-1.png)

At first, I had no clue about this decoded message. However, upon re-reading the challenge and noting the flag format 'PCTF{city_name}', I realized that the decoded message appeared to be a discombobulation. After converting the message, it resembled '46.768, -92.124'.

Indeed, these coordinates appeared to represent a location. I entered them into [Google Maps]((https://www.google.com/maps/search/46.768,+-92.124+maps?sa=X&ved=2ahUKEwiXk6L08aCBAxWdxjgGHbkVBREQ8gF6BAgaEAA&ved=2ahUKEwiXk6L08aCBAxWdxjgGHbkVBREQ8gF6BAgbEAI)) and discovered the following result:
![gmaps-result](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Crypto/My%20phone!/image-2.png)

Since the flag format specified a city name, I determined that the flag was Duluth.

flag: PCTF{Duluth}

---
title: "[CTF] PatriotCTF 2023"
date: 2023-12-19T09:45:04+07:00
draft: false
---

# PatriotCTF - 2023

Link: [CTFtime.org](https://ctftime.org/event/2030)

## Crypto

### Multi-numeral

![multi-numeral](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Crypto/Multi-numeral/image.png)

You are given a text file containing binary strings. You can use CyberChef to decode the strings:

![cyberchef](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Crypto/Multi-numeral/image-1.png)


flag: PCTF{w0w_s0_m4ny_number5}

### My phone!

![my-phone](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Crypto/My%20phone!/image.png)

We were provided with an image file that appeared to contain an unusual cipher. To decrypt the message within the image, I utilized the [Gravity Falls Bill Cipher tool](https://www.dcode.fr/gravity-falls-bill-cipher).

By decoding the message from the image, I obtained a result like this:

![results](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Crypto/My%20phone!/image-1.png)

At first, I had no clue about this decoded message. However, upon re-reading the challenge and noting the flag format 'PCTF{city_name}', I realized that the decoded message appeared to be a discombobulation. After converting the message, it resembled '46.768, -92.124'.

Indeed, these coordinates appeared to represent a location. I entered them into [Google Maps]((https://www.google.com/maps/search/46.768,+-92.124+maps?sa=X&ved=2ahUKEwiXk6L08aCBAxWdxjgGHbkVBREQ8gF6BAgaEAA&ved=2ahUKEwiXk6L08aCBAxWdxjgGHbkVBREQ8gF6BAgbEAI)) and discovered the following result:
![gmaps-result](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Crypto/My%20phone!/image-2.png)

Since the flag format specified a city name, I determined that the flag was Duluth.

flag: PCTF{Duluth}


### ReReCaptcha

![rerecaptcha](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Crypto/ReReCaptcha/image.png)

We were given a ZIP file for this challenge. Extract it and you will get 4 images:
- CT.png
- E.png
- P.png
- Q.png

If we open the file we will see a bunch of numbers. The challenge seems to be related to RSA when looking for the names of the files. Given the C, the E, the P and the Q, we have to find the N by multiplying the P and the Q. Once we have the N, we can do the decryption of the message. I use this [SITE](https://www.dcode.fr/rsa-cipher) to solve it. I use this [SITE](https://www.editpad.org/tool/extract-text-from-image) to extract the text from these images.

![results](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Crypto/ReReCaptcha/image-1.png)

flag: PCTF{I_H0P3_U_U53D_0CR!}

## Forensics 

### Capybara

![caypbara](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Forensics/Capybara/image.png)

We were given an image file to work with. Using exiftool, strings and the binwalk command, I found an embedded file within the image file. I extract it using binwalk and open the audio file (WAV). The audio file sounds like Morse.
Then use this [website](https://morsecode.world/international/decoder/audio-decoder-adaptive.html) to decode the Morse code.

After decoding the audio, I obtained the following result: 5 0 4 3 5 4 4 6 7 B 6 4 3 0 5 F 7 9 3 0 5 5 5 F 6 B 4 E 3 0 5 7 5 F 6 8 3 0 5 7 5 F 7 4 3 0 5 F 5 2 3 3 3 4 4 4 5 F 6 D 3 0 7 2 3 5 3 3 5 F 4 3 3 0 6 4 3 3 3 F 7 D

Converting this decoded Morse code to ASCII, I obtained the flag as shown in the provided image:

![results-capybara](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Forensics/Capybara/image-1.png)

flag: PCTF{d0_y0U_kN0W_h0W_t0_R34D_m0r53_C0d3?}

## Misc

### Welcome

![welcome](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Misc/Welcome/image.png)

To obtain the flag, simply navigate to the rules page as indicated in the provided image.

![rules-welcome](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Misc/Welcome/image-1.png)

flag: PCTF{I_h4v3_r34d_th3_rul35!}

### WPA

![wpa-chall](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Misc/WPA/image.png)

To solve this challenge, we need to analyze the packet capture file named 'savedcap.cap' to obtain the WPA passphrase (password) for the Wi-Fi network. To do this, I attempted to crack the WPA passphrase using the aircrack-ng tool.

`aircrack-ng -w /usr/share/wordlists/rockyou.txt savedcap.cap`

Execute this command, and you will obtain the password as shown in the image below:

![wpa-result](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Misc/WPA/image-1.png)

flag: PCTF{qazwsxedc}

### Uh Oh!

![uh-oh](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Misc/Uh%20Oh!/image.png)

We were provided with a wordlist file and tasked with finding its corresponding phone number based on the North American Numbering Plan (NANP). Our goal was to check each line in the wordlist for a format matching a North American phone number, specifically: '(NPA) XXX-XXXX,' where 'NPA' represents the area code, and 'X' represents any digit.

If you encounter a line that matches the specified phone number format, your task is to extract the phone number and take note of the line number where it was found. Once you've identified the line containing the phone number, create the flag as follows: Replace 'linenumber' with the line number where you found the phone number, and replace 'phonenumonlynumbers' with the digits of the phone number, excluding any special characters.

To automate this process, I created a Python script as follows:

```
import re

# Open the rockyou.txt file for reading
with open("rockyou.txt", "r") as file:
    # Initialize line number
    line_number = 0

    # Loop through each line in the file
    for line in file:
        line_number += 1

        # Use regular expression to match the phone number format
        match = re.search(r'\((\d{3})\)\s(\d{3}-\d{4})', line)

        # If a match is found, extract the phone number
        if match:
            area_code = match.group(1)
            phone_digits = match.group(2)

            # Create the flag
            flag = f"PCTF{{{line_number}_{area_code}{phone_digits}}}"

            # Print the flag
            print("Found phone number on line", line_number)
            print("Flag:", flag)
```

flag: PCTF{7731484_4043037283}

### Twins

![twins](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Misc/Twins/image.png)

Given two files containing a story, you can use any text comparison website. In this challenge, I used [this](https://text-compare.com/) to compare both files. After examining the files, you can construct the flag:

flag: PCTF{4wes0M3_sTories_man}

### Survey

![survey](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Misc/Survey/image.png)

Complete the survey on each page of the form to receive the flag.

flag: PCTF{Th4nk_Y0U_4_pLay1ng_PCTF_Th15_Y34r!}

## OSINT

### Bad Documentation

![bad-documentation](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/OSINT/Bad%20Documentation/image.png)

In this challenge, we were tasked with finding the password of a security researcher on their GitHub repository. My initial examination revealed that there were a total of 8 commits in the repository.

![commits-img](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/OSINT/Bad%20Documentation/image-3.png)

Upon inspecting all the commits, I came across one that seemed to provide a hint:

![hint-img](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/OSINT/Bad%20Documentation/image-1.png)

![hint-img-burprequest](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/OSINT/Bad%20Documentation/image-2.png)

In this specific commit, the researcher was submitting a form to change their password to 'easy' and confirming it by entering 'easy' in both the 'password' and 'confirm_pass' fields. Naturally, I attempted to use 'easy' as the flag, but unfortunately, it turned out to be incorrect.

I also tried using the values of {id} from the first line of the request and the Referer header, but this didn't yield the correct answer either.

Then, I focused on the Authorization header values. It was only after analyzing the Authorization header values that I discovered it was encoded in Base64.

`YWRtaW46UENURntOMF9jMEQzJ3NfMlZlUnlfUjNhTGxZX0cwbjN9`

I decoded it using the following command:

`echo "YWRtaW46UENURntOMF9jMEQzJ3NfMlZlUnlfUjNhTGxZX0cwbjN9" | base64 -d`

The decoded string revealed the flag:

![final-result](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/OSINT/Bad%20Documentation/image-4.png)

The output from the decoded string is 'username:password'."

flag: PCTF{N0_c0D3's_2VeRy_R3aLlY_G0n3}

### Rouge Access Point

![rouge-access-point](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/OSINT/Rouge%20Access%20Point/image.png)

To obtain the SSID using the BSSID of the Wi-Fi network, follow these steps:

1. Note the BSSID of the Wi-Fi network.
2. Access the 'Advanced Search' feature in Wiggle.
3. Insert the BSSID into the search field.
4. After completing the search, you will retrieve the SSID as shown in the image below:

![rap-results](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/OSINT/Rouge%20Access%20Point/image-1.png)

flag: PCTF{@Red's Table Free Wifi} 

## Reverse

### Coffee Shop

![coffee-shop](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Rev/Coffee%20Shop/image.png)

We were provided with a ZIP file that, upon extraction, a JAR file named 'CoffeeShop.jar.' When executed, this JAR file prompts the user to input a name and then checks if the input meets certain conditions outlined in the code.

Upon further investigation using JADX, we obtained the 'CoffeeShop.class' file, which appears as shown in the provided image.

![jadx-img](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Rev/Coffee%20Shop/image-1.png)

The Java code inside 'CoffeeShop.class' contains conditions that encode the user's input to Base64 and check if it meets specific criteria. If the conditions are not met, it executes the 'failure()' function; otherwise, it executes the 'success()' function. I initially struggled to understand how to craft a name that would satisfy these conditions. However, I eventually realized that I could directly decode the required Base64 string:

`R2FsZUJvZXR0aWNoZXI=`

Decoding this string resulted in the name:
> GaleBoetticher

According to the challenge description, we need to submit the flag in the format 'CACI{FirstnameLastname}'

flag: CACI{GaleBoetticher}

### Python XOR

![python-xor](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Rev/Python%20XOR/image.png)

In this challenge, we are given a Python file named XOR.py. The file itself looks like this.

```
from string import punctuation

alphabet = list(punctuation)
data = "bHEC_T]PLKJ{MW{AdW]Y"
def main():
#   For loop goes here
    key = ('')
    decrypted = ''.join([chr(ord(x) ^ ord(key)) for x in data])
    print(decrypted)
main()
```

This code is attempting to decrypt a message using XOR encryption, but it won't work as intended due to the use of an empty key. To decrypt the message correctly, you need to supply a valid key for the XOR operation. We can iterate through the 'alphabet' variable to find the correct XOR key. By performing the XOR operation between the encrypted data and the key, we can reveal the decrypted flag.

I created a solver program as follows:

```
from string import punctuation

alphabet = list(punctuation)
data = "bHEC_T]PLKJ{MW{AdW]Y"

def main():
    for key in alphabet:
        decrypted = ''.join([chr(ord(x) ^ ord(key)) for x in data])
        if "Flag{" in decrypted:
            print(decrypted)
            break

if __name__ == "__main__":
    main()
```

flag: Flag{python_is_e@sy}

## Trivia

### 1972 Schism

![1972-schism](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Trivia/1972%20Schism/image.png)

In order to gather historical context for this challenge, I referenced the official George Mason University website. This valuable source provided insights into GMU's early history, confirming that in 1972, it was a satellite school of another esteemed commonwealth college.

You can read more about it [here.](https://www.gmu.edu/about/legacy/history)

"In 1957, Mason was started as a branch campus of the University of Virginia. The school consisted of a single building and 17 students. Fifteen years later, on April 7, 1972, then-Governor A. Linwood Holton signed legislation to separate George Mason College from the University of Virginia."

flag: PCTF{UVA}

### Mascot Conundrum

![mascot-conundrum](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Trivia/Mascot%20Conundrum/image.png)

To successfully tackle this challenge, I began by thoroughly analyzing the information thoughtfully provided within the challenge description. It was evident from the details offered that the present-day mascot is known as "The Patriot."

My next step was to delve deeper into the history of GMU's mascots. I ventured onto the GMU website, where I discovered a comprehensive list of past mascots. The source I consulted for this was: [GMU Mascot Archives.](https://www.gmu.edu/news/2022-03/archives-history-mason-mascots)

The list of mascots I found on the website included:
- The Patriot
- Mason Maniak
- Mason Gorila
- The Green Mask
- Gunston

However, the challenge hinted at a particular mascot that held a special place in the hearts of both students and faculty. To pinpoint this beloved mascot, I expanded my search to other sources, including:
- A YouTube video: [GMU Mascot History Video](https://www.youtube.com/watch?v=cVlJadZE3nk)
- Comments on GMU's Facebook fanpage: [GMU Facebook Comments](https://www.facebook.com/georgemason/photos/a.47040579996/10151513578979997/?type=3 )

Upon analyzing the comments on the GMU Facebook fanpage, it became evident that Gunston was the mascot that resonated most deeply with the GMU community. Many individuals expressed their fondness for Gunston. Armed with this insight, I confidently submitted "Gunston" as the FLAG, and the submission proved to be successful.

flag: PCTF{Gunston}

### Mason ID OSINT 

![mason-id-osint](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Trivia/Mason%20ID%20OSINT/image.png)

In the Mason ID Card there is a building. The building is Johnson Center.

![mason-idcard](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Trivia/Mason%20ID%20OSINT/image-1.png)

References:
- https://studentcenters.gmu.edu/our-buildingsold/johnson-center/
- https://www.gmu.edu/news/2022-07/retro-mason-johnson-center-dedication-1996

flag: PCTF{George,Johnson}

## Web

### One-for-all

![one-for-all](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Web/One-for-all/image.png)

In this challenge, we need to find four pieces of the flag. To do this, I examine the various functionalities of this website. I also search for cookies and parameters. The default cookie is set as 'kiran.' When I tried to change its value to 'admin,' I obtained the first piece of the flag:

- PCTF{Hang_

Next, there is a profile menu on the website with a URL structure like this: http://chal.pctf.competitivecyber.club:9090/user?id=1. Based on the URL pattern, I attempted to change the user's ID to other numbers ranging from 1 to 50, but unfortunately, I found nothing. However, when I modified the starting point to 0, I managed to obtain the last flag using the collected pieces.

- ev3rYtH1nG}

![last_flag](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Web/One-for-all/image-2.png)

The website also offers a search functionality allowing players to search for any users. In my initial attempt, I tried to perform SQL Injection using sqlmap. I used the following command:

`sqlmap -u "http://chal.pctf.competitivecyber.club:9090/" --data "username=kiran" --method POST --level 2`

Upon running sqlmap, I discovered that the database management system being used was SQLite. I then modified the command to find the tables:

`sqlmap -u "http://chal.pctf.competitivecyber.club:9090/" --data "username=kiran" --method POST --level 2 --tables`

![tables-sql](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Web/One-for-all/image-3.png)

Based on the image above, the table name is accounts. Using this table name, I proceeded to dump its contents:

`sqlmap -u "http://chal.pctf.competitivecyber.club:9090/" --data "username=kiran" --method POST --level 2 -T accounts --dump`

![dump-results](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Web/One-for-all/image-4.png)

As you can see, there is another piece of the flag:

- and_Adm1t_

Furthermore, there is another hint for finding another flag in the fourth row of the password column, indicating that we should navigate to the /secretsforyou path. When we visited the path, we received the following result:

![secretforyou](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Web/One-for-all/image-5.png)

Based on the image above, it appears to be related to a Path Traversal vulnerability. I attempted some basic exploitation using this [site](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Directory%20Traversal/README.md) and managed to bypass it with a semicolon ; until it looked like this:

http://chal.pctf.competitivecyber.club:9090/secretsforyou/..;/

Then, I obtained another piece of the flag:

- l00s3_

After several attempts in this challenge, we have collected all the pieces:

- PCTF{Hang_ [1]
- ev3rYtH1nG} [4]
- and_Adm1t_ [3]
- l00s3_ [2]

Finally, we can assemble the correct flag by putting all the pieces together in the correct order.

flag: PCTF{Hang_l00s3_and_Adm1t_ev3rYtH1nG}

### Scavenger Hunt

![scavenger-hunt](https://raw.githubusercontent.com/dennyabrahamsinaga/ctf-writeup/main/PatriotCTF2023/Web/Scavenger%20Hunt/image.png)

Given the challenge site: http://chal.pctf.competitivecyber.club:5999/

Upon visiting the site, we immediately obtain the first flag: Flag 1/5 - PCTF{Hunt

To uncover the other flags, we need to delve into the source code. The second flag is cleverly hidden within HTML comments:

`<!-- Flag 2/5 - 3r5_4n -->`

Additionally, there are two Javascript files to inspect:
1. http://chal.pctf.competitivecyber.club:5999/script1.js
2. http://chal.pctf.competitivecyber.club:5999/script2.js

The first script reveals the fourth flag:

`console.log("Flag 4/5 - R5_e49");`

While the second script holds the fifth flag:

`document.cookie = "Flag 5/5=e4a541}";`

However, the third flag is still missing. In line with the nature of this "scavenger" challenge, I tried common paths often used in CTF challenges, such as robots.txt and sitemap.xml. It was when I explored /robots.txt that I uncovered the third flag:

```
# Flag 3/5 - D_g4tH3
User-agent: *
Disallow: /
```

flag: PCTF{Hunt3r5_4nD_g4tH3R5_e49e4a541}

---
title: "[CTF] Pointer Overflow CTF 2023"
date: 2023-12-19T19:50:12+07:00
draft: false
---

# UWSP Pointer Overflow CTF

## Crack

### The Gentle Rocking of the Sun

> Here's a password protected archive. Problem is that I seem to have forgotten das Passwort. All I have is this post-it note on my monitor that says "crack2 = 4bd939ed2e01ed1e8540ed137763d73cd8590323"

In this challenge, we were provided with a `crack2.7z` file. You can download it [here](https://uwspedu-my.sharepoint.com/personal/cjohnson_uwsp_edu/_layouts/15/onedrive.aspx?id=%2Fpersonal%2Fcjohnson_uwsp_edu%2FDocuments%2FPOCTF%2FPO%20CTF%202023%2FCrack%202%20-%20The%20Gentle%20Rocking%20of%20the%20Sun%2Fcrack2%2E7z&parent=%2Fpersonal%2Fcjohnson_uwsp_edu%2FDocuments%2FPOCTF%2FPO%20CTF%202023%2FCrack%202%20-%20The%20Gentle%20Rocking%20of%20the%20Sun&ga=1).

To solve this challenge, we should find the password of this archive file.
In the description, it was said `"crack2 = 4bd939ed2e01ed1e8540ed137763d73cd8590323"`.

So, we need to analyze this string. We can use online tools or cli-tools. 
In this case, I use [Haiti](https://noraj.github.io/haiti/#/) with this command:

`haiti 4bd939ed2e01ed1e8540ed137763d73cd8590323`

The output of the above command is:

![result-haiti](https://i.ibb.co/v4wdSmy/image.png)

As you can see, the hash was recognised as SHA-1. Then, using this [online tools](https://md5decrypt.net/en/Sha1/#answerHash), I got the password: zwischen

Now we can extract this file using this command:

`7z x crack2.7z -pzwischen`

The above command extracts the file with the password: zwischen.

After extracting the file, we find 25 directories that seem to be part of the flag. You can see the image below:

![tree-2023](https://i.ibb.co/fqRZrDG/image.png)

The flag format is `poctf{}`, so the final flag is: poctf{uwsp_c411f02n14_d234m1n9}

## Crypto

### Unquestioned and Unrestrained

> First crypto challenge so we have to keep it easy. Here's the flag, but it's encoded. All you have to do is figure out which method was used. Luckily, it's a common one.
>
> cG9jdGZ7dXdzcF80MTFfeTB1Ml84NDUzXzQyM184MzEwbjlfNzBfdTV9

It's just a simple base64 encoded string. You can decode it with this command:

`echo "cG9jdGZ7dXdzcF80MTFfeTB1Ml84NDUzXzQyM184MzEwbjlfNzBfdTV9" | base64 -d`

poctf{uwsp_411_y0u2_8453_423_8310n9_70_u5}

Well, you can identify this string using both these two tools:
- Cipher Identifier by [dcode.fr](https://www.dcode.fr/cipher-identifier)
- [CyberChef](https://gchq.github.io/CyberChef)

If you know of any other great tools, please let met know <3

### Missing and Missed

> A little cerebral fornication to round out the crypto challenges.
> 
> ++++++++++[>+>+++>+++++++>++++++++++<<<<-]>>>>++++++++++++.-.------------.+++++++++++++++++.--------------.+++++++++++++++++++++.------.++.----.---.-----------------.<<++++++++++++++++++++.-.++++++++.>>+++++++++.<<--.>>---------.++++++++++++++++++++++++.<<-----.--.>>---------.<<+++++++++.>>---------------.<<---------.++.>>.+++++++.<<--.++.+++++++.---------.+++++++..----.>>++++++++.+++++++++++++++.

We have been given a set of symbols or probably a set of instructions.
To solve this challenge, we should analyse what it is. 
Since it's a cryptography challenge, it must be related to a cipher.
We can use [this](https://www.dcode.fr/cipher-identifier) website to identify the cipher.

Paste it and then click Analyze.

![cipher-identifier-suggestion](https://i.ibb.co/mHLKm2f/image.png)

It asked us to investigate the Brainfuck. Click on Brainfuck, paste it, and click on Execute.
 
In the results section you will see the flag.

![flag-missingandmissed](https://i.ibb.co/FJfvX93/image.png)

poctf{uwsp_219h7_w20n9_02_f0290773n}

Well, I found out that there was a clue in the description of this challenge. 
It was "cerebral", so I thought it was related or associated with a "brain" cipher. 
Upon further investigation, I discovered that it was related to Brainfuck, a specific type of cipher.
> A little __cerebral__ fornication to round out the crypto challenges.

## Forensics

### If You Don't, Remember Me

> Here is a PDF file that seems to have some problems. I'm not sure what it used to be, but that's not important. I know it contains the flag, but I'm sure you can find it and drag it out of the file somehow. This is a two-step flag as you will find it partially encoded.

We were provided with a `DF1.pdf` file. You can download it [here](https://uwspedu-my.sharepoint.com/personal/cjohnson_uwsp_edu/_layouts/15/onedrive.aspx?ga=1&id=%2Fpersonal%2Fcjohnson_uwsp_edu%2FDocuments%2FPOCTF%2FPO%20CTF%202023%2FDF%201%20-%20If%20You%20Don%27t%20Remember%20Me).

To solve this challenge, I use the strings command as shown below:

`strings DF1.pdf`

If you scroll down, you will find a string like this:
poctf(uwsp_77333163306D335F37305F3768335F39346D33}

Based on the description, it was stated "This is a two-step flag as you will find it partially encoded.", so we could use CyberChef to find the partially encoded string which is `77333163306D335F37305F3768335F39346D33`. 
It turns out it's encoded in hex, and the decoded string is `w31c0m3_70_7h3_94m3`.
The final flag is poctf(uwsp_w31c0m3_70_7h3_94m3}
 
### A Petty Wage in Regret

> Here is a very interesting image. The flag has been broken up into several parts and embedded within it, so it will take a variety of skills to assemble it.

We were given a `DF2.jpg` file.
You can download the image file [here](https://uwspedu-my.sharepoint.com/personal/cjohnson_uwsp_edu/_layouts/15/onedrive.aspx?ga=1&id=%2Fpersonal%2Fcjohnson_uwsp_edu%2FDocuments%2FPOCTF%2FPO%20CTF%202023%2FDF%202%20-%20A%20Petty%20Wage%20in%20Regret).

To solve this challenge, I use exiftool and strings command.

`exiftool DF2.jpg`

Here is the output:
```
ExifTool Version Number         : 12.40
File Name                       : DF2.jpg
Directory                       : .
File Size                       : 350 KiB
File Modification Date/Time     : 2023:12:19 21:03:46+07:00
File Access Date/Time           : 2023:12:19 21:03:50+07:00
File Inode Change Date/Time     : 2023:12:19 21:03:46+07:00
File Permissions                : -rwxr-xr-x
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Exif Byte Order                 : Little-endian (Intel, II)
Image Description               : Blue Plate Special
Make                            : Daikon
Camera Model Name               : R4D15H
Orientation                     : Horizontal (normal)
X Resolution                    : 72
Y Resolution                    : 72
Resolution Unit                 : inches
Software                        : GIMP 2.10.34
Modify Date                     : 2023:06:05 09:46:44
Artist                          : Jeff Lee Johnson
Copyright                       : 2023
User Comment                    : 3A3A50312F323A3A20706F6374667B757773705F3768335F7730726C645F683464
Compression                     : JPEG (old-style)
Thumbnail Offset                : 418
Thumbnail Length                : 7000
XMP Toolkit                     : XMP Core 4.4.0-Exiv2
Document ID                     : gimp:docid:gimp:7d170a57-c0fc-4ebe-a03e-9741fb3ad8af
Instance ID                     : xmp.iid:9d8ffe0d-635a-463e-9119-dbb176f04f70
Original Document ID            : xmp.did:35be7b58-a9d1-422f-a8da-ac6c90492f89
Format                          : image/jpeg
Api                             : 2.0
Platform                        : Windows
Time Stamp                      : 1685976405819215
Version                         : 2.10.34
Creator Tool                    : GIMP 2.10
Metadata Date                   : 2023:06:05T09:46:44:05:00
History Action                  : saved
History Changed                 : /
History Instance ID             : xmp.iid:ff7cac82-2257-4ea5-b4ad-c9aefb4f83e5
History Software Agent          : Gimp 2.10 (Windows)
History When                    : 2023:06:05 09:46:45
Contributor                     : image/jpeg
Profile CMM Type                : Linotronic
Profile Version                 : 2.1.0
Profile Class                   : Display Device Profile
Color Space Data                : RGB
Profile Connection Space        : XYZ
Profile Date Time               : 1998:02:09 06:49:00
Profile File Signature          : acsp
Primary Platform                : Microsoft Corporation
CMM Flags                       : Not Embedded, Independent
Device Manufacturer             : Hewlett-Packard
Device Model                    : sRGB
Device Attributes               : Reflective, Glossy, Positive, Color
Rendering Intent                : Perceptual
Connection Space Illuminant     : 0.9642 1 0.82491
Profile Creator                 : Hewlett-Packard
Profile ID                      : 0
Profile Copyright               : Copyright (c) 1998 Hewlett-Packard Company
Profile Description             : sRGB IEC61966-2.1
Media White Point               : 0.95045 1 1.08905
Media Black Point               : 0 0 0
Red Matrix Column               : 0.43607 0.22249 0.01392
Green Matrix Column             : 0.38515 0.71687 0.09708
Blue Matrix Column              : 0.14307 0.06061 0.7141
Device Mfg Desc                 : IEC http://www.iec.ch
Device Model Desc               : IEC 61966-2.1 Default RGB colour space - sRGB
Viewing Cond Desc               : Reference Viewing Condition in IEC61966-2.1
Viewing Cond Illuminant         : 19.6445 20.3718 16.8089
Viewing Cond Surround           : 3.92889 4.07439 3.36179
Viewing Cond Illuminant Type    : D50
Luminance                       : 76.03647 80 87.12462
Measurement Observer            : CIE 1931
Measurement Backing             : 0 0 0
Measurement Geometry            : Unknown
Measurement Flare               : 0.999%
Measurement Illuminant          : D65
Technology                      : Cathode Ray Tube Display
Red Tone Reproduction Curve     : (Binary data 2060 bytes, use -b option to extract)
Green Tone Reproduction Curve   : (Binary data 2060 bytes, use -b option to extract)
Blue Tone Reproduction Curve    : (Binary data 2060 bytes, use -b option to extract)
Image Width                     : 1000
Image Height                    : 1000
Encoding Process                : Progressive DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:4:4 (1 1)
Image Size                      : 1000x1000
Megapixels                      : 1.0
Thumbnail Image                 : (Binary data 7000 bytes, use -b option to extract)
```

In the output provided, there is an encoded string within the `User Comment: 3A3A50312F323A3A20706F6374667B757773705F3768335F7730726C645F683464`. 
To decode this string, I use CyberChef and we get the output as `::P1/2:: poctf{uwsp_7h3_w0rld_h4d`

This is the first part of our flag. 
The challenge description said: "The flag has been broken up into several parts and embedded within it,...". 
So, we can use binwalk to check if there are any files embedded in this image file. 

`binwalk -e DF2.jpg`

![binwalk-res](https://i.ibb.co/t843Hms/image.png)

It turns out that there are embedded files in it, and it's another JPEG file.

Upon further investigation, I found that there's text inside an embedded image file. 
Using Photopea, this is the result:

![photopea-result](https://i.ibb.co/4pZcZ5r/image.png)

In the image above, it appears the second part of the flag appears as: `::P2/2:17_f1257`

After collecting all the parts, we can now put the flag together. The final flag is poctf{uwsp_7h3_w0rld_h4d_17_f1257}

## Misc

### Here You See A Passer By

> Simple task - solve the maze and find the flag. The password is poctf2023

We were provided with a `misc2.pdf` file. 

![misc2-img](https://i.ibb.co/qrc6bKb/image.png)

To solve this challenge, I use only Paint to draw the path.

![paint-path](https://i.ibb.co/R2rQWJG/image.png)

The final flag is poctf{uwsp_pr377y_bu7_p377y_bu7_pr377y}

## OSINT 

### A Great Interior Desert

> The flag is known to only one person, who is a trusted confidant of your first subject. It' s unknown how or why the two know each other, but your task will be to find the flag by finding subject 2 through subject 1. All we know about subject 1 is this information: @free_jack_marigold@mastodon.social

Using the information on description of this challenge, we can begin to analyse the first subject.
The information provided was @free_jack_marigold@mastodon.social, which prompted us to locate a user with the username 'free_jack' on mastodon.social.

![mastodon-result](https://i.ibb.co/vxfzjpQ/image.png)

There is only one user who has his username as `hijackbrickabrak`. The details for this user are as follows:

![mastodon-detals](https://i.ibb.co/KwLQQpQ/image.png)

As you can see in the image above, we collect a lot of information such as:
- He joined on Jun 01, 2023.
- He has social media with username as `@jock_bronson`.
- He has 2 posts.
- He has 9 following.

When I clicked on the Posts and Following text, it turned out to say "No posts here" and "This user doesn't follow anyone yet.". 
Then, with additional information, we can search for any details related to this user, `@jock_bronson`.

We discovered something interesting: there is a Twitter user with that this username.

![twitter-jock](https://i.ibb.co/58zmQnz/image.png)

When visiting his profile, we collect some information:
- He has the name big_ole_ramen.
- He has 25 followings.
- He has 6 followers.
- He is a student at UWSP where he studies Cybersecurity.

When I searched his tweets, I found nothing. Then, I started looking at his followers.

![list-of-followers](https://i.ibb.co/MGJJsGn/image.png)

In the list of his followers, there is one user called `Definitely NOT Montezuma Cloverfield` who caught my eye because his bio says "I am secret". 
So, I clicked on his profile and we got some information:
- His username is @MontezumaClove1.
- He joined in November 2020.
- He has 2 followings.
- He has 2 followers.

I found a tweet that looks interesting.

![interesting-tweets](https://i.ibb.co/SrvX753/image.png)

It indicated that his former was jock_bronson. 
So, I am now pretty sure that @MontezumaClove1 and jock_bronson's user are the same person. 

This means that we are stuck on the first subject when we should be looking for the flag through the second subject. 
So, I started looking at his followers and found a user called `Senor Spacecakes`. 

![montezuma-followers](https://i.ibb.co/Bf3ybCd/image.png)

Next, I clicked on his profile and gathered some information:
- His username is @SenorSpacecakes.
- He has 32 followings.
- He has 2 followers.
- He has an instagram account with @senorspacecakes.
- He has no posts that look interesting (for flag).

After searching for his tweets, we didn't find anything interesting except for his Instagram profile. 
While visiting his profile, we discovered the flag in one of his [posts](https://www.instagram.com/senorspacecakes/p/Cs92QmqrkTt/).

You can see it below, or simply use incognito mode to avoid logging in and still see his posts.

![insta-post](https://i.ibb.co/Pjd0DZ0/image.png)

To read the flag, you have to flip the image. I use Paint to flip the image, and you can see the result here:

![reverse-flag](https://i.ibb.co/yVgjDcy/image.png)

The final flag is poctf{uwsp_7h3_2357_15_45h}
 
## Reversing

### Easy as it Gets

> It doesn't get much easier than this when it comes to reverse engineering. Here we have a "secure" PowerShell script. All you need to do is figure out the super secret passphrase to decrypt the flag.

We were provided with a `RE1.ps1` file and its contents are as follows:

```
[Reflection.Assembly]::LoadWithPartialName("System.Security")  

function Encrypt-String($String, $Passphrase, $salt="SaltCrypto", $init="IV_Password", [switch]$arrayOutput)  
{  
    $r = new-Object System.Security.Cryptography.RijndaelManaged  
    $pass = [Text.Encoding]::UTF8.GetBytes($Passphrase)  
    $salt = [Text.Encoding]::UTF8.GetBytes($salt)  
    $r.Key = (new-Object Security.Cryptography.PasswordDeriveBytes $pass, $salt, "SHA1", 5).GetBytes(32) #256/8  
    $r.IV = (new-Object Security.Cryptography.SHA1Managed).ComputeHash( [Text.Encoding]::UTF8.GetBytes($init) )[0..15]  
    $c = $r.CreateEncryptor()  
    $ms = new-Object IO.MemoryStream  
    $cs = new-Object Security.Cryptography.CryptoStream $ms,$c,"Write"  
    $sw = new-Object IO.StreamWriter $cs  
    $sw.Write($String)  
    $sw.Close()  
    $cs.Close()  
    $ms.Close()  
    $r.Clear()  
    [byte[]]$result = $ms.ToArray()  
    return [Convert]::ToBase64String($result)  
}  
  
function Decrypt-String($Encrypted, $Passphrase, $salt="SaltCrypto", $init="IV_Password")  
{  
    if($Encrypted -is [string]){  
        $Encrypted = [Convert]::FromBase64String($Encrypted)  
    }  

    $r = new-Object System.Security.Cryptography.RijndaelManaged  
    $pass = [Text.Encoding]::UTF8.GetBytes($Passphrase)  
    $salt = [Text.Encoding]::UTF8.GetBytes($salt)  
    $r.Key = (new-Object Security.Cryptography.PasswordDeriveBytes $pass, $salt, "SHA1", 5).GetBytes(32) #256/8  
    $r.IV = (new-Object Security.Cryptography.SHA1Managed).ComputeHash( [Text.Encoding]::UTF8.GetBytes($init) )[0..15]  
    $d = $r.CreateDecryptor()  
    $ms = new-Object IO.MemoryStream @(,$Encrypted)  
    $cs = new-Object Security.Cryptography.CryptoStream $ms,$d,"Read"  
    $sr = new-Object IO.StreamReader $cs  

    Write-Output $sr.ReadToEnd()  

    $sr.Close()  
    $cs.Close()  
    $ms.Close()  
    $r.Clear()  
}  

cls  

#### 
# TODO: use strong password
# Canadian_Soap_Opera
### 

$pwd = read-host "(Case Sensitive) Please Enter User Password"  

$pcrypted = "TTpgx3Ve2kkHaFNfixbAJfwLqTGQdk9dkmWJ6/t0UCBH2pGyJP/XDrXpFlejfw9d"  

write-host "Encrypted Password is: $pcrypted"  
write-host ""  
write-host "Testing Decryption of Username / Password..."  
write-host ""      

$pdecrypted = Decrypt-String $pcrypted $pwd 

write-host "Decrypted Password is: $pdecrypted"  
```

The provided PowerShell script appears to encrypt and decrypt a password using the Rijndael (AES) algorithm. 
While analysing this script, I discovered a hardcoded password in the TODO comment:

```
#### 
# TODO: use strong password
# Canadian_Soap_Opera
### 
```

To solve this challenge, instead of reading the user input, we can assign the password to the variable. 
So, it will change from:
`$pwd = read-host "(Case Sensitive) Please Enter User Password"`

to

`$pwd = "Canadian_Soap_Opera"`

To simplify the solution, we only need the Decrypt function, which results in the following final code:

```
function Decrypt-String($Encrypted, $Passphrase, $salt="SaltCrypto", $init="IV_Password")  
{  
    if($Encrypted -is [string]){  
        $Encrypted = [Convert]::FromBase64String($Encrypted)  
    }  

    $r = new-Object System.Security.Cryptography.RijndaelManaged  
    $pass = [Text.Encoding]::UTF8.GetBytes($Passphrase)  
    $salt = [Text.Encoding]::UTF8.GetBytes($salt)  
    $r.Key = (new-Object Security.Cryptography.PasswordDeriveBytes $pass, $salt, "SHA1", 5).GetBytes(32) #256/8  
    $r.IV = (new-Object Security.Cryptography.SHA1Managed).ComputeHash( [Text.Encoding]::UTF8.GetBytes($init) )[0..15]  
    $d = $r.CreateDecryptor()  
    $ms = new-Object IO.MemoryStream @(,$Encrypted)  
    $cs = new-Object Security.Cryptography.CryptoStream $ms,$d,"Read"  
    $sr = new-Object IO.StreamReader $cs  

    Write-Output $sr.ReadToEnd()  

    $sr.Close()  
    $cs.Close()  
    $ms.Close()  
    $r.Clear()  
}  

cls  

$pwd = "Canadian_Soap_Opera"  

$pcrypted = "TTpgx3Ve2kkHaFNfixbAJfwLqTGQdk9dkmWJ6/t0UCBH2pGyJP/XDrXpFlejfw9d"  

write-host "Encrypted Password is: $pcrypted"  
write-host ""  
write-host "Testing Decryption of Username / Password..."  
write-host ""      

$pdecrypted = Decrypt-String $pcrypted $pwd 

write-host "Decrypted Password is: $pdecrypted"  
```

Save the following code to a file, and run the script in PowerShell by using the command:

`.\new.ps1`

Here's what we get:

![img-flag](https://i.ibb.co/C0ZMwGC/image.png)

The final flag is poctf{uwsp_4d_v1c70r14m_w4573l4nd3r}

## Stego

### Absence Makes Hearts Go Yonder

> Sometimes the oldest and simplest tricks can be the most fun. Here's an old stego tactic that requires no special software - just a little knowledge and maybe a keen eye.
>
> ![GIF](https://pointeroverflowctf.com/stego1.gif)

In this challenge, we were given with a `stego1.gif` file.
Given the nature of steganography challenges, I assume it involves hiding data within the file.

I used binwalk to solve this challenge.

`binwalk -e stego1.gif`

```
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             GIF image data, version "89a", 210 x 138
561650        0x891F2         Zip archive data, at least v2.0 to extract, compressed size: 37, uncompressed size: 37, name: flag.txt
561779        0x89273         End of Zip archive, footer length: 22
```

This revealed a zip file embedded in the gif file. 
We discovered a flag.txt file when we extracted the data using binwalk. 
Open it to reveal the flag.

poctf{uwsp_h342d_y0u_7h3_f1257_71m3}

Alternatively, we can use the strings command:

`strings stego1.gif | tail` 

At the end of the output we can find the flag.

![end-output](https://i.ibb.co/6Y0QmgS/image.png)

## Web

### We Rest Upon a Single Hope

> I am Vinz, Vinz Clortho, Keymaster of Gozer. Volguus Zildrohar, Lord of the Sebouillia. Are you the Gatekeeper?
>
> [Are you the Keymaster?](https://nvstgt.com/Hope2/index.html)

When you visit the Web Challenge, the page will look like this:

![web-challenge](https://gcdnb.pbrd.co/images/Moq8oiFijCeK.png?o=1)

To begin our investigation, we can analyse the source code by looking at the page source. 
Inside the script tag we find a function that looks like this:

```
<SCRIPT>
	function Gozer(key) {
		var hash = 0, i, chr;
		for (i = 0; i < key.length; i++) {
			chr   = key.charCodeAt(i);
			hash  = ((hash << 5) - hash) + chr;
			hash |= 0;
		}
		return hash;
	}
	function conv(s)	{
		var a = [];
		for (var n = 0, l = s.length; n < l; n ++) {
			var hex = Number(s.charCodeAt(n)).toString(16);
			a.push(hex);
		}
		return a.join('');
	}
	function Zuul(key) {
		if (key == v) {
			var Gatekeeper = [];
			var y = [];
			var z = [];
			Gatekeeper[0] = "706f6374667b75777370";
			Gatekeeper[1] = "formal";
			Gatekeeper[2] = "88410";
			for (var i = 0, l = Gatekeeper[0].length; i < l; i ++) {
				if (i == 0 || i % 2 == 0) {
					y += String.fromCharCode(parseInt((Gatekeeper[0].substring(i, i+2)), 16));
				}
			}
			z[0] = y;
			z[1] = Gatekeeper[2][3];
			z[2] = Gatekeeper[2][2] + Gatekeeper[1][3];
			z[3] = z[2][0] + Gatekeeper[1][5] + Gatekeeper[1][5];
			z[4] = (Gatekeeper[2]/12630) + "h" + z[2][0] + (Gatekeeper[2][0]-1);
			z[5] = z[4][0] + z[4][1] + '3' + Gatekeeper[1][2] + '3';
			z[6] = (Gatekeeper[2]/Gatekeeper[2]) + '5';
			z[7] = (Gatekeeper[2]*0) + Gatekeeper[1][0];
			z[8] = (Gatekeeper[2]/12630) + "h" + '3';
			z[9] = Gatekeeper[1][3] + (Gatekeeper[2]*0) + '5' + (Gatekeeper[2][0]-1);
			z[10] = 'r' + '3' + z[2][0] + Gatekeeper[1][5] + '}';
			console.log(z.join("_"));
		} else {
			console.log("Gozer the Traveler. He will come in one of the pre-chosen forms. During the rectification of the Vuldrini, the traveler came as a large and moving Torg! Then, during the third reconciliation of the last of the McKetrick supplicants, they chose a new form for him: that of a giant Slor! Many Shuvs and Zuuls knew what it was to be roasted in the depths of the Slor that day, I can tell you!");
		}
	}
	var p = navigator.mimeTypes+navigator.doNotTrack;
	var o = navigator.deviceMemory;
	var c = navigator.vendor+navigator.userAgent;
	var t = navigator.product+p;
	var f = o+c+p;
	var v = Gozer(p/((o+c)*t)+f);
</SCRIPT>
```

The source code provided has an interesting function. 
It's the Zuul function, which takes a parameter key and compares it to the value of the v variable. 
If key is equal to v, it performs a series of operations to construct an array z, and then logs the elements of z that have been concatenated, separated by underscores. So it means that if key==v, then the flag will appear.

We can print the value with:

`console.log(v);`

![console-img](https://i.ibb.co/m9Lrs3j/image.png)

If we execute it, it will give us the value.
We can copy the number and paste it. When we click Submit, there was a flag in the Console tab, but it was very fast so it kind of went away. 

![gif-console](https://i.ibb.co/5BhwxWd/Animation.gif)

In order to deal with this, instead of using the Submit button, we can take an additional step.
We can call the Zuul function by passing the value of v (key). In the console, we can run the following command:

`Zuul(206204233);`

The result is:

![result-zuul](https://i.ibb.co/2jTNRTP/image.png)

The final flag is: poctf{uwsp_1_4m_4ll_7h47_7h3r3_15_0f_7h3_m057_r34l}

There's also an alternative way to get the flag. Just copy and run this Javascript code:

```
var Gatekeeper = [];
var y = [];
var z = [];
Gatekeeper[0] = "706f6374667b75777370";
Gatekeeper[1] = "formal";
Gatekeeper[2] = "88410";
for (var i = 0, l = Gatekeeper[0].length; i < l; i ++) {
	if (i == 0 || i % 2 == 0) {
		y += String.fromCharCode(parseInt((Gatekeeper[0].substring(i, i+2)), 16));
	}
}
z[0] = y;
z[1] = Gatekeeper[2][3];
z[2] = Gatekeeper[2][2] + Gatekeeper[1][3];
z[3] = z[2][0] + Gatekeeper[1][5] + Gatekeeper[1][5];
z[4] = (Gatekeeper[2]/12630) + "h" + z[2][0] + (Gatekeeper[2][0]-1);
z[5] = z[4][0] + z[4][1] + '3' + Gatekeeper[1][2] + '3';
z[6] = (Gatekeeper[2]/Gatekeeper[2]) + '5';
z[7] = (Gatekeeper[2]*0) + Gatekeeper[1][0];
z[8] = (Gatekeeper[2]/12630) + "h" + '3';
z[9] = Gatekeeper[1][3] + (Gatekeeper[2]*0) + '5' + (Gatekeeper[2][0]-1);
z[10] = 'r' + '3' + z[2][0] + Gatekeeper[1][5] + '}';
console.log(z.join("_"));
```

This code is part of the Zuul function and essentially converts a given hexadecimal string (Gatekeeper[0]) to ASCII characters, performs some mathematical and string operations, and then prints the result in a specific format.

Here's what we get:

![js-result](https://i.ibb.co/5Kbgv5t/image.png)

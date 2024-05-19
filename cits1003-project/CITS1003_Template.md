<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 100vh;">
  
  <h2>CITS1003 Project Report</h2>
  
  <p>Student ID: [24122137]</p>
  <p>Student Name: [Ziyue Xu]</p>

</div>

# Part 1 - Linux and Networking
## Emu Hack #1 - Backdoored
### Step 1
According to the information we can get that there is a backdoor on the server located on a port between 61000 to 61500 and the challenge serve is hosted at `34.116.68.59`. Using the `nmap` command to scan this IP address.

#### Command:
```bash
nmap -p 61000-61500 -Pn 34.116.68.59
```
#### Explanation:
- `-p`: Specifies the ports or port ranges for `nmap` to scan.
- `-Pn`: Tells `nmap` not to perform host discovery.

#### Result:
```bash
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-11 09:13 EDT
Nmap scan report for 59.68.116.34.bc.googleusercontent.com (34.116.68.59)
Host is up (0.051s latency).
Not shown: 500 filtered tcp ports (no-response)
PORT      STATE SERVICE
61337/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 14.56 seconds

```

### Step 2
To send a TCP message we should enter `nc` command. Send the message `EMU`.

#### Command:
```bash
nc 34.116.68.59 61337
EMU
```
#### Explanation:
- `nc`: Uses for writing to network connections using TCP or UDP.

#### Result:
```bash
 ______        ___   _ _____ ____    _                  
|  _ \ \      / / \ | | ____|  _ \  | |__  _   _        
| |_) \ \ /\ / /|  \| |  _| | | | | | '_ \| | | |       
|  __/ \ V  V / | |\  | |___| |_| | | |_) | |_| |       
|_|     \_/\_/  |_| \_|_____|____/  |_.__/ \__, |       
    _                             _____    |___/        
   / \   _ __   __ _ _ __ _   _  | ____|_ __ ___  _   _ 
  / _ \ | '_ \ / _` | '__| | | | |  _| | '_ ` _ \| | | |
 / ___ \| | | | (_| | |  | |_| | | |___| | | | | | |_| |
/_/   \_\_| |_|\__, |_|   \__, | |_____|_| |_| |_|\__,_|
               |___/      |___/                         

Did you really think you would find our backdoor so easily? :P

Good effort though, here's a flag for your attempt: UWA{4dvanC3d_p0r7_5sc4nN1nG?1!?1}
```
The flag is in the end of this message.

#### Flag Found
```bash
UWA{4dvanC3d_p0r7_5sc4nN1nG?1!?1}
```

## Emu Hack #2 - Git Gud
### Step 1
According to the information we can know that we should use the `git` command to clone the repository in the IP address `34.116.68.59` which called `emu`. The port is `8000`. First switch the current directory to the desktop.

#### Command:
```bash
cd Desktop
git clone http://34.116.68.59:8000/emu
```
#### Explanation:
- `clone`: Creates a copy of an existing repository by downloading the project to the local machine.

#### Result:
```bash
Cloning into 'emu'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
```

### Step 2
From the previous step we can get the folder `emu` located in desktop, check what files are in this folder.

#### Command:
```bash
cd /home/kali/Desktop/emu/
ls -la
```

#### Result:
```bash
drwxr-xr-x 3 kali kali 4096 May 11 13:00 .
drwxr-xr-x 3 kali kali 4096 May 11 13:00 ..
drwxr-xr-x 8 kali kali 4096 May 11 09:38 .git
-rw-r--r-- 1 kali kali  449 May 11 09:38 README.txt
```

### Step 3
Open the file `README.txt`.

#### Command:
```bash
cat README.txt
```

#### Result:
```bash
UWA{N()w_y0U_kN0W_40w_2_u53_g17!1!!}
--------------------------------------------------

To Angry Emu hacker,

As per our agreement, I have set up some SSH credentials for you to access our server:

username: emu001
password: feathers4life24

To make sure only us birbs can get in I set a login shell to stop pesky hoomans getting in. Just do that SSH trick I taught you about to get in.

Delete this message after you read it!

Best regards,
Mr. X
```
The Flag is in the begining of this document.

The provided credentials for SSH access are:

**Username**: `emu001`
**Password**: `feathers4life24`

These credentials could indicate a potential pathway for further exploration or analysis.

#### Flag Found
```bash
UWA{N()w_y0U_kN0W_40w_2_u53_g17!1!!}
```

## Emu Hack #3 - SSH Tricks
### Step 1
According the information and the file `README.txt` we can know that we should use `SSH` command to log in. We also can know that username is `emu001` and password is `feathers4life24`. And we need to check the image file in `/home/emu001`. So we need to use `SSH` command to build connection.

#### Command:
```bash
ssh -p 2022 emu001@34.116.68.59 ls /home/emu001
emu001@34.116.68.59's password: feathers4life24
```
#### Explanation:
- `-p`: Creates a copy of an existing repository by downloading the project to the local machine.

#### Result:
```bash
note_to_angry_emu_hacker.txt
top_secret.png
```

### Step 2
Obviously the image file we need to check is named `top_secret.png`. use `SCP` command to get it.
#### Command:
```bash
scp -P 2022 emu001@34.116.68.59:/home/emu001/top_secret.png ./
emu001@34.116.68.59's password: feathers4life24
```
#### Explanation:
- `-P`: Specifies the port number to connect to on the remote host for the `SCP` command. In normal time it will override the default SSH port `22`, but in this question the SSH port is `2022` by default.

#### Result:
```bash
top_secret.png                             100%  475KB 932.7KB/s   00:00
```

### Step 3
We can find the image `top_secret.png` generated in the folder `emu`. Open it and the flag is on the image.

#### Flag Found
```bash
UWA{how_did_u_get_past_that_login_shell?????}
```

## Emu Hack #4 - Git Gud or GTFO Bin
### Step 1
A clear, and detailed description. 

### Step 2
### Step X

#### Flag Found
```bash
UWA{xxxxxxxxxx}
```

# Part 2 - Cryptography
## Advanced Emu Standard
### Step 1
According to the question, we can get the command to use the `AES-128` mode in `ECB`. At first we need to open the challenge website `http://34.87.251.234:3001`.

### Step 2
Divide the command of length 32 into two parts. And each of them is of length 16.

#### Enter the command in command encryptor:
```bash
deactivate_speci
al_procedure_123
```
#### Explanation:
`AES-128` can only handle 16 bytes at a time. When encrypting data with `AES-128` in `ECB` mode, the data must be split into 16-byte blocks.

#### Result in command encryptor:
```bash
3155433d53ed30c89aef89b2e7273924
4127efafc809cc1209376d039e0001f1
```
Combine the resulting ciphertexts.

### Step 3
Enter ciphertext in encrypted command. Find the flag.

#### Enter:
```bash
3155433d53ed30c89aef89b2e72739244127efafc809cc1209376d039e0001f1
```
#### Result:
```bash
Self-destruct protocol successfully deactivated!

UWA{3CB_i5_bL0cK_Ind3peNd3nt!}
```

#### Flag Found
```bash
UWA{3CB_i5_bL0cK_Ind3peNd3nt!}
```

## Emu Cook Book
### Step 1
Open the CyberChef website `https://cyberchef.io/`.

### Step 2
Search for `From Base64` and `To Hex`, then drag the operation to the Recipe window. And Type Cook Book into the input window. 

#### Enter:
```bash
H4sIAEwyx2UA/7VWX/LyIAw8Td+tpaCP33gS/97/CHY3hYQVne/l5ziVFEKW3SR42D/TEd9lxSNvj6GJ0ZKaidFy7GfhYeZiW07lsmKGjwyXBJfMrbfJ6fDPEMwVAxxDeA/AeOfO5Ibutsy9icWGAeaKrRLepUddvC0JGGx1PFAWNB6PZvmMF2bbVoYB292BA5uscKXDWqcrCvq1EzqjooZEpOlohR34Wpj0rGrQmS7kY40YUswIGxHw6VeAMEs0Lh1NLN4zYvspWGyBMeLibUnAYMTE8Nn1HaRmmBXmGte9GuXaEhMjMlOYMwFF7mpDTih8i1Zuku6ejp0Jnp0aUBe4ZFIRMZSKoT+hFl9fuEKHFi5mDQNGVpQsVFLBQp0jhtOHGoEMOR8pFXAevsfqamA7euVbzS0iiijOlYlRLxK+vTgldwZaWRjrTyQPLswP0yViuFYMHmDUKH+rMZg1DNjUctLV2EcBw+1TDalFjyd9TPpGXzmuBpsU3rNn0YFVElHcu04lAJxgzjqevhpHdbV3KjYHTCcEZmVavUQMj4qhvyiCvv9xb7hvU9Iw8MpABoR7A+82hQKGZ6cG40lufO2MgrUJ36uRmBWtTxJRfokary/3htSGlC5NueVGt7ix0Dp34Y2qt/jc/klIvjk7MEn/n3ztpme45TGVyxt9eh+f4AgAAA==
```

#### Result:
```bash
1f 8b 08 00 4c 32 c7 65 00 ff b5 56 5f f2 f2 20 0c 3c 4d df ad a5 a0 8f df 78 12 ff de ff 08 76 37 85 84 15 9d ef e5 e7 38 95 14 42 96 dd 24 78 d8 3f d3 11 df 65 c5 23 6f 8f a1 89 d1 92 9a 89 d1 72 ec 67 e1 61 e6 62 5b 4e e5 b2 62 86 8f 0c 97 04 97 cc ad b7 c9 e9 f0 cf 10 cc 15 03 1c 43 78 0f c0 78 e7 ce e4 86 ee b6 cc bd 89 c5 86 01 e6 8a ad 12 de a5 47 5d bc 2d 09 18 6c 75 3c 50 16 34 1e 8f 66 f9 8c 17 66 db 56 86 01 db dd 81 03 9b ac 70 a5 c3 5a a7 2b 0a fa b5 13 3a a3 a2 86 44 a4 e9 68 85 1d f8 5a 98 f4 ac 6a d0 99 2e e4 63 8d 18 52 cc 08 1b 11 f0 e9 57 80 30 4b 34 2e 1d 4d 2c de 33 62 fb 29 58 6c 81 31 e2 e2 6d 49 c0 60 c4 c4 f0 d9 f5 1d a4 66 98 15 e6 1a d7 bd 1a e5 da 12 13 23 32 53 98 33 01 45 ee 6a 43 4e 28 7c 8b 56 6e 92 ee 9e 8e 9d 09 9e 9d 1a 50 17 b8 64 52 11 31 94 8a a1 3f a1 16 5f 5f b8 42 87 16 2e 66 0d 03 46 56 94 2c 54 52 c1 42 9d 23 86 d3 87 1a 81 0c 39 1f 29 15 70 1e be c7 ea 6a 60 3b 7a e5 5b cd 2d 22 8a 28 ce 95 89 51 2f 12 be bd 38 25 77 06 5a 59 18 eb 4f 24 0f 2e cc 0f d3 25 62 b8 56 0c 1e 60 d4 28 7f ab 31 98 35 0c d8 d4 72 d2 d5 d8 47 01 c3 ed 53 0d a9 45 8f 27 7d 4c fa 46 5f 39 ae 06 9b 14 de b3 67 d1 81 55 12 51 dc bb 4e 25 00 9c 60 ce 3a 9e be 1a 47 75 b5 77 2a 36 07 4c 27 04 66 65 5a bd 44 0c 8f 8a a1 bf 28 82 be ff 71 6f b8 6f 53 d2 30 f0 ca 40 06 84 7b 03 ef 36 85 02 86 67 a7 06 e3 49 6e 7c ed 8c 82 b5 09 df ab 91 98 15 ad 4f 12 51 7e 89 1a af 2f f7 86 d4 86 94 2e 4d b9 e5 46 b7 b8 b1 d0 3a 77 e1 8d aa b7 f8 dc fe 49 48 be 39 3b 30 49 ff 9f 7c ed a6 67 b8 e5 31 95 cb 1b 7d 7a 1f 9f e0 08 00 00
```
#### Explanation:
The result `1f 8b` when decoded to hex typically indicates the beginning of a `GZIP` file.

### Step 3
So Search for `From Base64`, `Gunzip`, then drag the operation to the Recipe window. And Type Cook Book into the input window.

#### Result:
```bash
00000000%20%2035%2036%2020%2035%2036%2020%2036%2034%2020%2034%2032%2020%2036%2035%2020%2033%20%20%7C56%2056%2064%2042%2065%203%7C%0A00000010%20%2033%2020%2035%2032%2020%2034%2039%2020%2034%2064%2020%2033%2031%2020%2033%2039%20%20%7C3%2052%2049%204d%2031%2039%7C%0A00000020%20%2020%2036%2063%2020%2035%2034%2020%2035%2037%2020%2033%2039%2020%2035%2030%2020%20%20%7C%206c%2054%2057%2039%2050%20%7C%0A00000030%20%2034%2065%2020%2035%2036%2020%2033%2039%2020%2033%2033%2020%2034%2064%2020%2035%20%20%7C4e%2056%2039%2033%204d%205%7C%0A00000040%20%2035%2020%2037%2038%2020%2034%2064%2020%2035%2038%2020%2033%2032%2020%2033%2034%20%20%7C5%2078%204d%2058%2032%2034%7C%0A00000050%20%2020%2037%2061%2020%2035%2036%2020%2036%2061%2020%2034%2065%2020%2037%2039%2020%20%20%7C%207a%2056%206a%204e%2079%20%7C%0A00000060%20%2035%2038%2020%2033%2033%2020%2034%2065%2020%2035%2035%2020%2036%2032%2020%2033%20%20%7C58%2033%204e%2055%2062%203%7C%0A00000070%20%2031%2020%2034%2032%2020%2036%2036%2020%2035%2061%2020%2034%2034%2020%2034%2031%20%20%7C1%2042%2066%205a%2044%2041%7C%0A00000080%20%2020%2037%2038%2020%2036%2032%2020%2036%2062%2020%2036%2034%2020%2036%2036%2020%20%20%7C%2078%2062%206b%2064%2066%20%7C%0A00000090%20%2036%2034%2020%2034%2035%2020%2036%2037%2020%2037%2061%2020%2034%2065%2020%2035%20%20%7C64%2045%2067%207a%204e%205%7C%0A000000a0%20%2037%2020%2035%2036%2020%2036%2036%2020%2035%2061%2020%2034%2036%2020%2035%2061%20%20%7C7%2056%2066%205a%2046%205a%7C%0A000000b0%20%2020%2037%2034%2020%2035%2031%2020%2036%2063%2020%2033%2039%2020%2036%2061%2020%20%20%7C%2074%2051%206c%2039%206a%20%7C%0A000000c0%20%2034%2064%2020%2035%2035%2020%2034%2061%2020%2037%2039%2020%2035%2038%2020%2033%20%20%7C4d%2055%204a%2079%2058%203%7C%0A000000d0%20%2032%2020%2034%2065%2020%2034%2039%2020%2034%2064%2020%2033%2032%2020%2035%2036%20%20%7C2%204e%2049%204d%2032%2056%7C%0A000000e0%20%2020%2034%2037%2020%2035%2038%2020%2033%2032%2020%2034%2065%2020%2036%2066%2020%20%20%7C%2047%2058%2032%204e%206f%20%7C%0A000000f0%20%2034%2065%2020%2034%2035%2020%2037%2038%2020%2037%2033%2020%2035%2035%2020%2033%20%20%7C4e%2045%2078%2073%2055%203%7C%0A00000100%20%2033%2020%2033%2030%2020%2033%2064%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7C3%2030%203d%7C
```
#### Explanation:
According to the output, such as "%20", it's a typical URL encoding, which means blank space.

### Step 4
Drag `URL Decode` to the Recipe window.

#### Result:
```bash
00000000  35 36 20 35 36 20 36 34 20 34 32 20 36 35 20 33  |56 56 64 42 65 3|
00000010  33 20 35 32 20 34 39 20 34 64 20 33 31 20 33 39  |3 52 49 4d 31 39|
00000020  20 36 63 20 35 34 20 35 37 20 33 39 20 35 30 20  | 6c 54 57 39 50 |
00000030  34 65 20 35 36 20 33 39 20 33 33 20 34 64 20 35  |4e 56 39 33 4d 5|
00000040  35 20 37 38 20 34 64 20 35 38 20 33 32 20 33 34  |5 78 4d 58 32 34|
00000050  20 37 61 20 35 36 20 36 61 20 34 65 20 37 39 20  | 7a 56 6a 4e 79 |
00000060  35 38 20 33 33 20 34 65 20 35 35 20 36 32 20 33  |58 33 4e 55 62 3|
00000070  31 20 34 32 20 36 36 20 35 61 20 34 34 20 34 31  |1 42 66 5a 44 41|
00000080  20 37 38 20 36 32 20 36 62 20 36 34 20 36 36 20  | 78 62 6b 64 66 |
00000090  36 34 20 34 35 20 36 37 20 37 61 20 34 65 20 35  |64 45 67 7a 4e 5|
000000a0  37 20 35 36 20 36 36 20 35 61 20 34 36 20 35 61  |7 56 66 5a 46 5a|
000000b0  20 37 34 20 35 31 20 36 63 20 33 39 20 36 61 20  | 74 51 6c 39 6a |
000000c0  34 64 20 35 35 20 34 61 20 37 39 20 35 38 20 33  |4d 55 4a 79 58 3|
000000d0  32 20 34 65 20 34 39 20 34 64 20 33 32 20 35 36  |2 4e 49 4d 32 56|
000000e0  20 34 37 20 35 38 20 33 32 20 34 65 20 36 66 20  | 47 58 32 4e 6f |
000000f0  34 65 20 34 35 20 37 38 20 37 33 20 35 35 20 33  |4e 45 78 73 55 3|
00000100  33 20 33 30 20 33 64                             |3 30 3d|
```
#### Explanation:
According to the output, each hexadecimal value is separated by `%`, and is represented by ASCII character `|`. It's a typical hexadecimal dump.

### Step 5
Drag `From Hexdump`, `From Hex` and `From Base64` to the Recipe window. Then we can get the flag.

#### Flag Found
```bash
UWA{tH3_eMoO5_w1LL_n3V3r_sToP_d01nG_tH35e_dVmB_c1Br_cH3eF_ch4LlS}
```

## Emu Casino
### Step 1
Open the challenge website `http://34.87.251.234:3000`.

### Step 2
Check the cookies of this website and decode it.

#### Enter:
```bash
eyJjcmVkaXRzIjoxMCwicm91bmQiOjEsInNlc3Npb25faWQiOiI3ZDZiMjYyOTc5ZjA5N2I5MjFlYWIyYzlkZGM4M2MwOSJ9.ZknQuw.ZyLk6kwJb6zl6CgkdSfnAiDgUWk
```

#### Header Result:
```bash
{
  "credits": 10,
  "round": 1,
  "session_id": "7d6b262979f097b921eab2c9ddc83c09"
}
```
#### Explanation:
This cookies is a `JWT`, which consists of three parts, `Header`, `Payload` and `Signature`. We can get the cookies id by decoding it.

### Step 3
Open the file `solution_template`, change the session_id as `7d6b262979f097b921eab2c9ddc83c09` and change the round value as `1`. The output is `heads`.

### Step 4
Change the value of round. Then after `10` rounds, wins the balance to `10240` E-Bucks. And get the flag.

#### Flag Found
```bash
UWA{R0LLl111Llli1iNg_1N_C4$$$$h!11!}
```

## EWT
### Step 1
A clear, and detailed description.  

### Step 2
### Step X

#### Flag Found
```bash
UWA{xxxxxxxxxx}
```

# Part 3 - Forensics
## Caffeinated Emus
### Step 1
Download the images `loganpaul-emus.png`.

### Step 2
Open the website which can check the metadata of images. For example, `https://exifinfo.org/?trk=public_post-text`.

### Step 3
Upload the image, and get the GPS Position `31 deg 27' 59.19" S, 119 deg 29' 0.70" E`.

### Step 4
Open the Map website which can enter the GPS position. For example, `https://gps-coordinates.org/`

#### Enter:
```bash
31 deg 27' 59.19" S, 119 deg 29' 0.70" E
```

#### Result:
```bash
Marvel Loch WA 6426, Australia
```
The flag is the town name.

#### Flag Found
```bash
UWA{Marvel Loch}
```

## Flightless Data
### Step 1
Download the file `email.html` and open it. We can get the password:
```bash
theEMusWiLLRoOld3w0oRlD
```

### Step 2
Saving the image as `image.jpeg`, then putting the image into the desktop folder of kali linux. Check if there is the `image.jpeg` in the desktop folder.

#### Command:
```bash
cd Desktop
ls -la
```

#### Result:
```bash
drwxr-xr-x  3 kali kali    4096 May 11 13:33 .
drwx------ 18 kali kali    4096 May 11 14:41 ..
drwxr-xr-x  3 kali kali    4096 May 11 13:35 emu
-rw-rw-rw-  1 kali kali 258781 May 12 04:24 image.jpeg
-rw-rw-rw-  1 kali kali 270266 May 11 08:51 topsecret_corrupted.pdf
```

### Step 3
We need to use command `steghide` to extract data from the image. The file name is `image.jpeg` and the password is `theEMusWiLLRoOld3w0oRlD`.

#### Command:
```bash
steghide extract -p theEMusWiLLRoOld3w0oRlD -sf image.jpeg
```
#### Explanation:
- `-p`: Makes `steghide` specifies the password used to extract hidden data from a file.

#### Result:
```bash
wrote extracted data to "secret.txt".
```


### Step 4
Open `secret.txt` and get the flag.

#### Command:
```bash
cat secret.txt
```

#### Result:
```bash
Hello my fellow Emu.

Fortunately the hoomans arent big brain like use and would not look for this secret message in the least significant bits of an image!

UWA{fLigHtL3sS_d4Ta_uNd3r_tH3_r4dAr}
```
The flag is in the end of this document.

#### Flag Found
```bash
UWA{fLigHtL3sS_d4Ta_uNd3r_tH3_r4dAr}
```

## Ruffled Feathers
### Step 1
Download the file `topsecret_corrupted.pdf`. Then putting the file into the desktop folder of kali linux. 

### Step 2
Install `GHex` in kali linux.

#### Command:
```bash
sudo apt install ghex
```

### Step 3
Open `GHex` and open the file `topsecret_corrupted.pdf`. In the right side of the file `topsecret_corrupted.pdf` shows:

```bash
%PDF-1.5
%\D0\D4\C5\D8
6 0 obj
<<
/Corrupt272
```
Comparing the normal pdf file we can find that we should change `Corrupt` into `Length `.


## Step 4
Saving the file then open the pdf. The flag is on the image.

#### Flag Found
```bash
UWA{uNrUffLed_pDeF}
```

## Emu in the Shell
### Step 1
A clear, and detailed description.  

### Step 2
### Step X

#### Flag Found
```bash
UWA{xxxxxxxxxx}
```

# Part 4 - Vulnerabilities
## Feathered Forum - Part 1
### Step 1
A clear, and detailed description.  

### Step 2
### Step X

#### Flag Found
```bash
UWA{xxxxxxxxxx}
```

## Feathered Forum - Part 2
### Step 1
A clear, and detailed description.  

### Step 2
### Step X

#### Flag Found
```bash
UWA{xxxxxxxxxx}
```

## Feathered Forum - Part 3
### Step 1
A clear, and detailed description.  

### Step 2
### Step X

#### Flag Found
```bash
UWA{xxxxxxxxxx}
```

## Emu Apothecary
### Step 1
A clear, and detailed description.  

### Step 2
### Step X

#### Flag Found
```bash
UWA{xxxxxxxxxx}
```

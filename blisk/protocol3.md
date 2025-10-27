# Protocol 3: Protect the Hacker

Start: 
* Run script
* Get ready to rumble

Emum Steps: 
* Nmap: 

`nmap -sC -sV -iL 0.0.0.0`
* Emum4Linux:

`enum4linux -v -A 0.0.0.0`
* SMB Host Enumeration:

`nxc smb 0.0.0.0`
* SMB Share Enumeration:

`nxc smb 0.0.0.0 -u 'a' -p '' --shares`
* Find DC IP:

`nslookup -type=SRV _ldap._tcp.dc._msdcs.<domain>`
* Emumerate users through rpc:

` net rpc group members 'Domain Users' -W 'domain.local' -I 0.0.0.0 -U % `

` rpcclient -U '' -N $IP enumdomusers querydispinfo`

* Emumerate LDAP:

Full: `ldapsearch -H ldap://0.0.0.0 -x -b 'domainName'`

Authenticated: `ldap://dc.domain.htb -Y GSSAPI -b "cn=users,dc=absolute,dc=htb" "user" "description"`




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

# Bloodhound API: 
1. Run Script (pickle short blueprint long)
2. sudo bloodhound-setup
3. Login to neo4j with default creds
4. Change password
5. sudo nano /etc/bhapi/bhapi.json change password
6. ` bloodhound`
7. Login with default creds and reset pass

# Bloodhound Listener:
1. Data needed:

* Domain Name (-d)
* Username (-u)
* Password (-p)	
* Name Server/DC IP (-ns)

` bloodhound-python -c All -d [DOMAIN.FQDN] -u [USERNAME] -p '[PASSWORD]' -ns [DC_IP_ADDRESS] `





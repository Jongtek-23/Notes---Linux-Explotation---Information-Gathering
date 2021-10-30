# Notes---Linux-Explotation-

### Information Gathering - Commands

##### Enumerating NFS (Network File System)
```bash
nmap --script nfs-ls,nfs-showmount,nfs-statfs <IP target>
showmount -e <IP target>

mkdir -p /mnt/home/bob
mount -t nfs <NFS Server IP>:/home/bob /mnt/home/bob -o nolock
```

##### Enumerating Portmapper (rpcbind) ->  port 111/TCP
```bash
nmap --script rpc-grind,rpcinfo <IP target> -p 111
rpcinfo -p <IP target>
```

##### SAMBA -> SMB/CIFS 
- It provides print and file sharing services for windows clients within an environment
- SAMBA can be found listening on the usual "NetBIOS" ports
```bash
netbios-ns      137/tcp     # NETBIOS Name Service
netbios-ns      137/udp
netbios-dgm     138/tcp     # NETBIOS Datagram Service
netbios-dgm     138/udp
netbios-ssn     139/tcp     # NETBIOS Session Service
netbios-ssn     139/udp
microsoft-ds    445/tcp     # Microsoft Naked CIFS
```

- Using nmap:
`nmap -sT -sU -sV <IP target> -p135,137,138,139,445 --open`

- Enemarate SMB:
```bash
nmap --script smb-enum-shares <IP target>

smbclient -L <IP target>
smbmap -H <IP target>
```
- SMBMAP allows interact with the remote file system, searching file contents for specific strings, and even execiting commands.

- Connect SMB shares:
```bash
smbclient \\<IP target>\<name share smb>
```
- To mount SMB shares:
```bash
apt install cifs-utils

mkdir -p /mnt/www
mount -t cifs \\<IP target>\www /mnt/www
cd /mnt/www
ls -las
```
##### SAMBA - SMB Users -> enumarate users over the SMB protocol

































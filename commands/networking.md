# Networking
## Determining ip adress
- `ip addr`
- `ip address show`
- `ip a`
- `ip a s`
- `ifconfig`
## Displaying the hostname
- `hostname` > myhost
- `uname -n`
- `hostname -f`: display the fully qualified domain name (FQDN).
## Setting the hostname
- `hostname myhost`
- echo 'myhost' > /etc/hostname
- vi /etc/network
    HOSTNAME=WEBPROD01
## Resolving DNS names
- `host myhost/ip` > ip/host
- `dig myhost`
- `etc/hosts`: file with ip/host, can add an alias. 
    127.0.0.1 localhost alias
    127.0.1.1 myhost 
- `etc/nsswitch.con`: name servise switch (nss), control the search order for resolution.
- `etc/services/`: maps port names to port numbers
## Dynamic Host Configuration Protocol (DHCP)
- Each IP is "leased" from the pool of IP addresses the DHCP server manages.
## Testing Connectivity
- `ping HOST/IP`: test the connectivity with some host sending packages until stop.
- `ping -c COUNT HOST`: send COUNT packages.
- `traceroute -n HOST`: trace the complete route (not only the endpoint). 
- `netstat`: network statistics.
    - `-n`: Display numerical addresses and ports.
    - `-i`: Displays a list of network interfaces.
    - `-r`: Displays the route table. (netstat -rn)
    - `-p`: Display the PID and program used.
    - `-l`: Display listening sockets. (netstat -nlp)
    - `-t`: Limit the output to TCP (netstat -ntlp)
    - `-u`: Limit the output to UDP (netstat -nulp)
- `tcpdump`: Packet sniffing.
    - `-n`: Display numerical addresses and ports.
    - `-A`: Display ASCII (text) output.
    - `-v`: Verbose mode. Produce more output.
    - `-vvv`: Even more verbose output.
- `telnet HOST/IP PORT_NUMBER`: telnet google.com 80.


# SSH
## 1. Simple connextion

  1. ennable connextion to ssh client:  
  	- `ssh [IP/Host Name]`
  	- `ssh username@hostname`
  2. close connection:
  	- `exit`

## 2. Connect to VM through ssh
On VM do:
Setting > Network > Advanced > Port Forwarding, then set the SSH parameter:
  - Name: SSH
  - Protocol: TCP
  - Host IP: 127.0.0.1
  - Host Port: 2222
  - IP Guest: Empty
  - Port Guest: 22

Run:
  - `ssh adminuser@127.0.0.1 -p 2222`
  - `ssh adminuser@localhost -p 2222`

  https://medium.com/@pierangelo1982/setting-ssh-connection-to-ubuntu-on-virtualbox-af243f737b8b

### Connect to VM over the network

- Use bridged networking
- Ensure sshd is installed and running
- `/sbin/ip addr`: print address.

## 3. Transferring and Copying Files
- `scp source destination`: copy source to destination.
  - local to remote: `scp file.txt adminuser@localhost:/path`.
  - remote to local: `ssh adminuser@127.0.0.1 -p 2222 > ssh scp file.txt dgarrido@192.168.100.13:~/`
- `sftp host`: start a secure file transfer session with host.
  - `sftp -P 2222 adminuser@localhost`
  - `l[command]`: run command in local device.
    - `lls`: `ls` in local device.
  - `put source destination`: upload local source in remote destination.
- `ftp host`: start a file transfer session with host.




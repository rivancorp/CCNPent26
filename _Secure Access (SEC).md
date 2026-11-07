
<!--   Your Monitor Number  =  #$34T#  -->

## Review on IPv4 Essentials

## CIDR

| CIDR | NETMASK     | RIVAN Format | WILDCARD    |
| ---  | ---         | ---          | ---         |
| /20  |             | (Octet, i)   |             |
| /27  |             | (Octet, i)   |             |
| /14  |             | (Octet, i)   |             |


<br>


### Find the Network:

| Dept       | Network             | Infected Hosts      |
| ---        | ---                 | ---                 |
| ADMIN      | 192.168.192.0       | 192.168.200.54  /18 |
| HR         | 192.168.128.0       | 192.168.130.115 /20 | 
| FINANCE    | 192.168.126.96      | 192.168.126.100 /27 |
| SOC        | 192.168.120.0       | 192.168.122.187 /22 |
| NOC        | 192.168.126.136     | 192.168.126.140 /29 |
| MARKETING  | 192.168.0.0         | 192.168.27.216  /19 |
|            |                     | 192.168.141.216 /20 |


<br>
<br>

---
&nbsp;


# Lab Setup

__WINDOWS__ = Server 2022  
__LINUX__   = Rocky  
__CISCO__   = CSR1000V 


<br>
<br>


### 1. Deploy

Deploy the following VMs   
- CSR1000v  
- YVM
- NetOps  

| VM        | NetAdapter | NetAdapter 2 | NetAdapter 3 | NetAdapter 4  |
| ---       | ---        | ---          | ---          | ---           |
| UTM-PH    | NAT        | VMNet2       | VMNet3       | Bridged (Rep) |
|           |            |              |              |               |
| UTM-JP    | NAT        | VMNet2       | VMNet4       |               |
|           |            |              |              |               |
| NetOps-PH | VMNet1     | VMNet2       | VMNet3       | Bridged (Rep) |
|           |            |              |              |               |
| BLDG-PH   | VMNet3     | VMNet2       |              |               |
|           |            |              |              |               |
| BLDG-JP-1 | VMNet4     |              |              |               |
|           |            |              |              |               |
| BLDG-JP-2 | VMNet4     |              |              |               |
|           |            |              |              |               |


<br>


### 2. Bootstrap

~~~
!@UTM-PH
conf t
 hostname UTM-PH
 enable secret pass
 service password-encryption
 no logging cons
 no ip domain lookup
 line vty 0 14
  transport input all
  password pass
  login local
  exec-timeout 0 0
 int g1
  ip add 208.8.8.11 255.255.255.0
  no shut
 int g2
  ip add 192.168.102.11 255.255.255.0
  no shut
 int g3
  ip add 10.11.11.113 255.255.255.224
  no shut
  exit
 int g4
  ip add 10.#$34T#.1.11 255.255.255.0
  no shut
  exit
 !
 ip route 0.0.0.0 0.0.0.0 208.8.8.2
 !
 ip domain lookup
 ip name-server 8.8.8.8 1.1.1.1
 !
 username admin privilege 15 secret pass
 ip http server
 ip http secure-server
 ip http authentication local
 ip http timeout-policy idle 600 life 86400 requests 10000
 end
wr
!
~~~

<br>

~~~
!@UTM-JP
conf t
 hostname UTM-JP
 enable secret pass
 service password-encryption
 no logging cons
 no ip domain lookup
 line vty 0 14
  transport input all
  password pass
  login local
  exec-timeout 0 0
 int g1
  ip add 208.8.8.12 255.255.255.0
  no shut
 int g2
  ip add 192.168.102.12 255.255.255.0
  no shut
 int g3
  ip add 10.21.21.213 255.255.255.240
  ip add 10.22.22.223 255.255.255.192 secondary
  no shut
  exit
 !
 ip route 0.0.0.0 0.0.0.0 208.8.8.2
 ip domain lookup
 ip name-server 8.8.8.8 1.1.1.1
 !
 username admin privilege 15 secret pass
 ip http server
 ip http secure-server
 ip http authentication local
 ip http timeout-policy idle 600 life 86400 requests 10000
 end
wr
!
~~~

<br>

~~~
!@BLDG-PH
sudo su
hostname BLDG-PH
ifconfig eth0 10.11.11.111 netmask 255.255.255.224 up
route add default gw 10.11.11.113
ping 10.11.11.113
~~~

Create a user account:
~~~
!@BLDG-PH-1
adduser admin

> pass
> pass
~~~

<br>

~~~
!@BLDG-JP-1
sudo su
hostname BLDG-JP-1
ifconfig eth0 10.21.21.211 netmask 255.255.255.240 up
route add default gw 10.21.21.213
ping 10.21.21.213
~~~

<br>

~~~
!@BLDG-JP-2
sudo su
hostname BLDG-JP-2
ifconfig eth0 10.22.22.221 netmask 255.255.255.192 up
route add default gw 10.22.22.223
ping 10.22.22.223
~~~


<br>
<br>


## NetOps-PH Setup
> Login: root
> Pass: C1sc0123

<br>

### 1. Get the MAC Address for the Bridge connection
VMWare > NetOps-PH Settings > NetAdapter (2, 3, & 4) > Advance > MAC Address

| NetAdapter   | MAC Address      | VM Interface |           |
| ---          | ---              | ---          | ---       |
| NetAdapter 2 | ___.___.___.___  | ens___       |  ens192   |
| NetAdapter 3 | ___.___.___.___  | ens___       |  ens224   |
| NetAdapter 4 | ___.___.___.___  | ens___       |  ens256   |


<br>


### 2. Get Network-VM Mapping

~~~
!@NetOps-PH
ip -br link
~~~


<br>


### 3. Modify Interface IP

__Using Network Management CLI for persistent IP.__

~~~
!@NetOps-PH
nmcli connection add \
type ethernet \
con-name VMNET2 \
ifname ens192 \
ipv4.method manual \
ipv4.addresses 192.168.102.6/24 \
autoconnect yes

nmcli connection up VMNET2


nmcli connection add \
type ethernet \
con-name VMNET3 \
ifname ens224 \
ipv4.method manual \
ipv4.addresses 10.11.11.100/27 \
autoconnect yes

nmcli connection up VMNET3


nmcli connection add \
type ethernet \
con-name BRIDGED \
ifname ens256 \
ipv4.method manual \
ipv4.addresses 10.#$34T#.1.6/24 \
autoconnect yes

nmcli connection up BRIDGED


ip route add 10.0.0.0/8 via 10.#$34T#.1.4 dev ens256
ip route add 200.0.0.0/24 via 10.#$34T#.1.4 dev ens256
ip route add 0.0.0.0/0 via 10.11.11.113 dev ens224
~~~



<br>
<br>

---
&nbsp; 


### Jobs of a firewall

1. &nbsp; 
2. &nbsp; 
3. &nbsp; 
4. &nbsp; 
5. &nbsp; 


<br>
<br>


__Make Sure EDGE-#$34T# is Configured__

~~~
!@EDGE-#$34T#
conf t
 hostname EDGE-#$34T#
 enable secret pass
 service password-encryption
 no logging console
 no ip domain-lookup
 line cons 0
  password pass
  login
  exec-timeout 0 0
 line vty 0 14
  password pass
  login
  exec-timeout 0 0
 int gi 0/0/0
  no shut
  ip add 10.#$34T#.#$34T#.1 255.255.255.0
  desc INSIDE
 int gi 0/0/1
  no shut
  ip add 200.0.0.#$34T# 255.255.255.0
  desc OUTSIDE
 int loopback 0
  ip add #$34T#.0.0.1 255.255.255.255
  desc VIRTUALIP
  exit
  
!@ospf routing edge
 no router ospf 1
 router ospf 1
  router-id #$34T#.0.0.1
  network 200.0.0.0 0.0.0.255 area 0
  network 10.#$34T#.#$34T#.0 0.0.0.255 area 0
  network #$34T#.0.0.1 0.0.0.0 area 0
 int gi 0/0/0
  ip ospf network point-to-point
 end
~~~

<br>

~~~
!@BABA-#$34T#
conf t
 hostname coreBaba-#$34T#
 enable secret pass
 service password-encryption
 no logging console
 no ip domain-lookup
 line cons 0
  password pass
  login
  exec-timeout 0 0
 line vty 0 14
  password pass
  login
  exec-timeout 0 0
 int gi 0/1
  no shut
  no switchport
  ip add 10.#$34T#.#$34T#.4 255.255.255.0
 int vlan 1
  no shut
  ip add 10.#$34T#.1.4 255.255.255.0
  desc DEFAULT-VLAN
 int vlan 10
  no shut
  ip add 10.#$34T#.10.4 255.255.255.0
  desc WIFI-VLAN
 int vlan 50
  no shut
  ip add 10.#$34T#.50.4 255.255.255.0
  desc CCTV-VLAN
 int vlan 100
  no shut
  ip add 10.#$34T#.100.4 255.255.255.0
  desc VOICE-VLAN
  exit
 !
 vlan 10
  name WIFIVLAN
 vlan 50
  name CCTVVLAN
 vlan 100
  name VOICEVLAN
 int fa 0/2
  switchport mode access
  switchport access vlan 10
 int fa 0/4
  switchport mode access
  switchport access vlan 10
 int fa 0/6
  switchport mode access
  switchport access vlan 50
 int fa 0/8
  switchport mode access
  switchport access vlan 50
 int fa 0/3
  switchport mode access
  switchport access vlan 100
 int fa 0/5
  switchport mode access
  switchport voice vlan 100
  switchport access vlan 1
  mls qos trust device cisco-phone
 int fa 0/7
  switchport mode access
  switchport voice vlan 100
  switchport access vlan 1
  mls qos trust device cisco-phone
  exit
 !
 ip routing
 no router ospf 1
 router ospf 1
  router-id 10.#$34T#.#$34T#.4
  network 10.#$34T#.0.0 0.0.255.255 area 0
 int gi 0/1
  ip ospf network point-to-point
  end
~~~

<br>

~~~
!@CUCM-#$34T#
conf t
 hostname CUCM-#$34T#
 enable secret pass
 service password-encryption
 no logging console
 no ip domain-lookup
 line cons 0
  password pass
  login
  exec-timeout 0 0
 line vty 0 14
  password pass
  login
  exec-timeout 0 0
 int fa 0/0
  no shut
  ip add 10.#$34T#.100.8 255.255.255.0
  exit
 ip routing
 no router ospf 1
 router ospf 1
  router-id 10.#$34T#.100.8
  network 10.#$34T#.100.0 0.0.0.255 area 0
  end
~~~


<br>
<br>

---
&nbsp;


# Access Control

~~~
!@UTM-PH
conf t
 ip route 10.0.0.0 255.0.0.0 10.#$34T#.1.4
 ip route 200.0.0.0 255.255.255.0 10.#$34T#.1.4
 end
~~~


<br>
<br>


### Task 1: Prevent traffic only from VLAN 100 (CUCM) to reach UTM-PH (10.#$34T#.1.11)
~~~
!@UTM-PH
config t
 no ip access-list extended FWP1
 ip access-list extended FWP1
  deny ip  __.__.__.__    __.__.__.__    __.__.__.__    __.__.__.__  log
  
  permit ip any any
  exit
 !
 int gi 4
  ip access-group FWP1 in
  end
show ip access-list int g4
~~~


<br>


Verify:
~~~
!@CUCM
ping 10.#$34T#.1.11

telnet 10.#$34T#.1.11
~~~


<br>


Remove the ACL
~~~
!@UTM-PH
conf t
 int g4
  no ip access-group FWP1 in
  end
~~~


<br>
<br>

---
&nbsp;


### Task 2: Prevent traffic coming from the PC (10.#$34T#.1.10) and the EDGE Router from reaching UTM-PH (10.#$34T#.1.11)
~~~
!@UTM-PH
config t
 no ip access-list extended FWP2
 ip access-list extended FWP2
  deny ip  __.__.__.__    __.__.__.__    __.__.__.__    __.__.__.__  log
  
  permit ip any any
  exit
 !
 int gi 4
  ip access-group FWP2 in
  end
show ip access-list int g4
~~~


<br>


Remove the ACL
~~~
!@UTM-PH
conf t
 int g4
  no ip access-group FWP2 in
  end
~~~


<br>
<br>

---
&nbsp;


### Task 3: Allow the PC (10.#$34T#.1.10) to access HTTP, SSH, and ICMP of UTM-PH (10.#$34T#.1.11)

` www.fbi.gov  vs  neu.edu.ph `


<br>


Make the UTM Router Vulnerable
~~~
!@UTM-PH
config t
 ip host www.bet11.com 10.#$34T#.1.11
 service finger
 service tcp-small-servers
 service udp-small-servers
 ip dns server
 ip http server
 ip http secure-server
 telephony-service
  no auto-reg-ephone
  max-ephones 5
  max-dn 20
  ip source-address 10.#$34T#.1.11 port 2000
  exit
 voice service voip
  allow-connections h323 to sip
          
  allow-connections sip to h323
  allow-connections sip to sip
  supplementary-service h450.12
 sip
   bind control source-interface g4
   bind media source-interface g4
   registrar server expires max 600 min 60
 voice register global
  mode cme
  source-address 10.#$34T#.1.11 port 5060
  max-dn 12
  max-pool 12
  authenticate register
  create profile sync syncinfo.xml
  end
~~~


<br>
<br>

~~~
!@UTM-PH
config t
 no ip access-list extended FWP3
 ip access-list extended FWP3
  permit  __  host  __.__.__.__  host  __.__.__.__   eq  __  log
  
  exit
 !
 int gi 4
  ip access-group FWP3 in
  end
show ip access-list int g4
~~~


<br>


Remove the ACL
~~~
!@UTM-PH
conf t
 int g4
  no ip access-group FWP3 in
  end
~~~


<br>
<br>

---
&nbsp;


### Task 4: Create an extended ACL for `www.bet11.com` that will open the following ports
SIP, SSH, HTTPS, DNS, SUBMISSION, IMAPS, POP3S, SMTPS, SNMP


| Port       | No.  | 
| ---        | ---  |
| SIP        |      |
| SSH        |      |
| DNS        |      |
| SUBMISSION |      |
| HTTPS      |      |
| SMTPS      |      |
| IMAPS      |      |
| POP3S      |      |
| SNMP       |      |


<br>


~~~
!@UTM-PH
config t
 no ip access-list extended FWP4
 ip access-list extended FWP4
  permit  tcp  any  host  www.bet11.com  eq  22  log
  permit  ___  any  host  www.bet11.com  eq  __  log
  
  exit
 !
 int gi 4
  ip access-group FWP4 in
  end
show ip access-list int g4
~~~


<br>


Remove the ACL
~~~
!@UTM-PH
conf t
 int g4
  no ip access-group FWP4 in
  end
~~~


<br>
<br>

---
&nbsp;


### [Activity] On the EDGE Router create an extended ACL named FWP4 with the following:
- Allow Pings destined for your PC (10.#$34T#.1.10)  
- Allow Telnet access to your CoreBABA's VLAN 1 SVI,  
- but make sure to block everything else  

<br>

~~~
!@EDGE-#$34T#
config t
 ip access-list extended FWP5
  permit  icmp  any  host 10.#$34T#.1.10             log 
  permit  tcp   any  host  ___.___.___.___  eq  ____  log
  
  
  exit
 !
 int g0/0/1
  ip access-group FWP5 in
  end
show ip access-list int g0/0/1
~~~

<br>


Remove the ACL
~~~
!@EDGE-#$34T#
conf t
 int gi 0/0/1
  no ip access-group FWP5 in
  end
~~~


<br>
<br>

---
&nbsp;


## Exam Question:

### 1. The security team has been asked to only enable host A (10.2.2.7) and host B (10.3.9.9)
to the new isolated network segment (10.9.8.14) that provides access to legacy devices.
Access from all other hosts should be blocked. Which of the following entries would
need to be added on the firewall?

  - [ ] __A.__ 
~~~
Permit 10.2.2.0/24 to 10.9.8.14/27
Permit 10.3.9.0/24 to 10.9.8.14/27
Deny 0.0.0.0/0 to 10.9.8.14/27
~~~

  - [ ] __B.__ 
~~~
  Deny 0.0.0.0/0 to 10.9.8.14/27
  Permit 10.2.2.0/24 to 10.9.8.14/27
  Permit 10.3.9.0/24 to 10.9.8.14/27
~~~

  - [ ] __C.__
~~~  
  Permit 10.2.2.7/32 to 10.9.8.14/27
  Permit 10.3.9.9/32 to 10.9.8.14/27
  Deny 0.0.0.0/0 to 10.9.8.14/27
~~~

  - [ ] __D.__
~~~
  Permit 10.2.2.7/32 to 10.9.8.14/27
  Permit 10.3.9.0/24 to 10.9.8.14/27
  Deny 10.9.8.14/27 to 0.0.0.0/0
~~~


&nbsp;
---
&nbsp;


### 2. An enterprise is trying to limit outbound DNS traffic originating from its internal network. Outbound DNS requests
will only be allowed from one device with the IP address 10.50.10.25. Which of the following firewall ACLs will
accomplish this goal?

  - [ ] __A.__ 
~~~
  Access list outbound permit 0.0.0.0/0 0.0.0.0/0 port 53
  Access list outbound deny 10.50.10.25/32 0.0.0.0/0 port 53
~~~

  - [ ] __B.__ 
~~~
  Access list outbound permit 0.0.0.0/0 10.50.10.25/32 port 53
  Access list outbound deny 0.0.0.0/0 0.0.0.0/0 port 53
~~~

  - [ ] __C.__
~~~ 
  Access list outbound permit 0.0.0.0/0 0.0.0.0/0 port 53
  Access list outbound deny 0.0.0.0/0 10.50.10.25/32 port 53
~~~

  - [ ] __D.__
~~~
  Access list outbound permit 10.50.10.25/32 0.0.0.0/0 port 53
  Access list outbound deny 0.0.0.0/0 0.0.0.0/0 port 53
~~~


&nbsp;
---
&nbsp;


| Port   | No.  | Port   | No.   |
| ---    | ---  | ---    | ---   |
| Telnet |      | SMTPS  |       |
| SSH    |      | IMAPS  |       |
| DNS    |      | POP3S  |       |
| HTTP   |      | MYSQL  |       |
| HTTPS  |      | SCCP   |       |
| SMTP   |      | SIP    |       |
| IMAP   |      | FTP    |       |
| POP3   |      | TFTP   |       |


<br>
<br>

---
&nbsp;


## Reverse Proxy

~~~
!@UTM-PH
conf t
 int g1
  ip nat outside
 int g3
  ip nat inside
 !
 ip access-list extended NAT
  deny ip 10.11.11.96 0.0.0.31  10.21.21.208 0.0.0.15
  deny ip 10.11.11.96 0.0.0.31  10.22.22.192 0.0.0.63
  exit
 !
 ip nat inside source list NAT int g1 overload
 !
 ip nat inside source static tcp  10.11.11.111  80  208.8.8.100  8080
 !
 no ip route 10.0.0.0 255.0.0.0 10.#$34T#.1.4
 no ip route 200.0.0.0 255.255.255.0 10.#$34T#.1.4
 end
~~~


<br>
<br>


Create a user account on __BLDG-PH-1__
~~~
!@BLDG-PH-1
sudo su
deluser admin
adduser admin

> pass
> pass
~~~ 


### Task 5: Create Port Forwarding Rule for the following:

| SERVER        | INSIDE PORT  | OUTSIDE IP  | OUTSIDE PORT |
| ---           | ---          | ---         | ---          |
| BLDG-PH       | 443          | 208.8.8.100 | 8443         |
| BLDG-PH       | 22           | 208.8.8.100 | 2222         |


<br>
<br>

---
&nbsp;


### Task 6: Configure NAT on the EDGE Router, then set the following port forwarding rule

| SERVER      | INSIDE PORT | OUTSIDE IP     | OUTSIDE PORT |
| ---         | ---         | ---            | ---          |
| CoreTAAS    | 23          | 200.0.0.#$34T# | 2023         |
| CoreBABA    | 23          | 200.0.0.#$34T# | 4023         |
| CUCM        | 23          | 200.0.0.#$34T# | 8020         |


<br>

~~~
!@EDGE-#$34T#
conf t
 int g0/0/0
  ip nat inside
  exit
 int g0/0/1
  ip nat outside
  exit
 !
 !
 ip access-list extended NAT-POLICY
  permit ip 10.#$34T#.0.0  0.0.255.255  any
  exit
 !
 !
 ip nat inside source list NAT-POLICY int g0/0/1 overload
 ip route 0.0.0.0 0.0.0.0 200.0.0.1
 end
~~~


<br>


~~~
!@EDGE-#$34T#
conf t
 no router ospf 1
 router ospf 1
  router-id #$34T#.0.0.1
  network 10.#$34T#.#$34T#.0 0.0.0.255 area 0
  network #$34T#.0.0.1       0.0.0.0   area 0
  default-information originate always
  end
~~~


<br>


~~~
!@EDGE-#$34T#
conf t
 ip nat inside source static tcp  10.#$34T#.1.4  23  200.0.0.#$34T#  4023
 !
 ip nat inside source static tcp  10.#$34T#.1.4  __  __.__.__.__     __
 ip nat inside source static tcp  10.#$34T#.1.4  __  __.__.__.__     __
 end
~~~


<br>
<br>

---
&nbsp;


## Honeypot

### STEP 1. Create a python file for the honeypot operation.
~~~
!@NetOps
sudo nano /usr/local/bin/tcp-6969-honeypot.py
~~~


<br>


Then paste the following contents to the nano shell.


<br>


~~~
#!/usr/bin/env python3
import asyncio
import datetime
import os
import argparse
import binascii
import pathlib

### LOG FILE LOCATION
BASE_LOG = '/var/log/tcp-6969-honeypot'
os.makedirs(BASE_LOG, exist_ok=True)


### CONVERT RAW BYTES TO HUMAN READABLE DATA
def hexdump(data: bytes) -> str:

  ### CONVERT RAW BYTES TO HEX STRINGS
  hexs = binascii.hexlify(data).decode('ascii')
  
  ### LOOP 32 CHAR CHUNKS TO BE A HUMAN READABLE DATA
  lines = []
  for i in range(0, len(hexs), 32):
    chunk = hexs[i:i+32]
    b = bytes.fromhex(chunk)
    printable = ''.join((chr(x) if 32 <= x < 127 else '.') for x in b)
    lines.append(f'{i//2:08x} {chunk} {printable}')
  return '\n'.join(lines)


### LOG INFORMATION ABOUT THE ATTACKER
async def handle(reader: asyncio.StreamReader, writer: asyncio.StreamWriter):
  
  ### IDENTIFY ATTACKER IP
  peer = writer.get_extra_info('peername')
  if peer is None:
    peer = ('unknown', 0)
  ip, port = peer[0], peer[1]
  
  
  ### SESSION LOGS - Year-Month-Day Hour-Minutes-Seconds
  start = datetime.datetime.utcnow().strftime("%Y%m%dT%H%M%SZ")
  sess_name = f"{start}_{ip.replace(':','_')}_{port}"
  sess_dir = pathlib.Path(BASE_LOG) / sess_name
  sess_dir.mkdir(parents=True, exist_ok=True)
  meta_file = sess_dir / "meta.txt"
  
  ### WRITE SESSION LOGS
  with meta_file.open("w") as mf:
    mf.write(f"start: {start}\npeer: {ip}:{port}\n")
  print(f"[+] connection from {ip}:{port} -> {sess_dir}")


  ### SEND MESSAGE TO THE ATTACKER
  try:
    writer.write(b'Welcome to Rivan, you Hacker!!! \r\n')
    await writer.drain()
  except Exception:
    pass


  ### DUMP RAW AND HEX DATA
  raw_file = sess_dir / "raw.bin"
  hexd_file = sess_dir / "hexdump.txt"
  try:
    with raw_file.open("ab") as rb, hexd_file.open("a") as hf:
      while True:
        data = await asyncio.wait_for(reader.read(4096), timeout=300.0)
        if not data:
          break
        ts = datetime.datetime.utcnow().isoformat() + "Z"
        rb.write(data)
        hf.write(f"\n-- {ts} --\n")
        hf.write(hexdump(data) + "\n")
        
        ### RECORD READABLE COPY
        printable = ''.join((chr(x) if 32 <= x < 127 else '.') for x in data)
        with (sess_dir / "printable.log").open("a") as pf:
          pf.write(f"{ts} {printable}\n")
        
        ### SEND TARPITTED RESPONSE
        try:
          writer.write(b"OK\r\n")
          await writer.drain()
        except Exception:
          break
  except asyncio.TimeoutError:
    print(f"[-] connection timed out {ip}:{port}")
  except Exception as e:
    print(f"[-] session error {e}")
  finally:
    try:
      writer.close()
      await writer.wait_closed()
    except Exception:
      pass
    end = datetime.datetime.utcnow().strftime("%Y%m%dT%H%M%SZ")
    with meta_file.open("a") as mf:
      mf.write(f"end: {end}\n")
    print(f"[+] closed {ip}:{port} -> {sess_dir}")


### TCP HANDLER
async def main(host, port):
  server = await asyncio.start_server(handle, host, port)
  addrs = ", ".join(str(sock.getsockname()) for sock in server.sockets)
  print(f"Listening on {addrs}")
  async with server:
    await server.serve_forever()
      
### CLI ENTRYPOINT
if __name__ == "__main__":
  parser = argparse.ArgumentParser()
  parser.add_argument("--host", default="0.0.0.0")
  parser.add_argument("--port", type=int, default=6969)
  args = parser.parse_args()
  try:
    asyncio.run(main(args.host, args.port))
  except KeyboardInterrupt:
    pass
~~~


<br>


> [!NOTE]
> Imports
> - asyncio: event loop + async IO (handles many connections efficiently).
> - datetime: timestamps.
> - os, pathlib: filesystem operations.
> - argparse: parse CLI arguments (--host, --port).
> - binascii: binary ⇄ hex conversion.


<br>
<br>

---
&nbsp;


### STEP 2. Create the directory for the log files.
~~~
!@NetOps
sudo mkdir /var/log/tcp-6969-honeypot
~~~


<br>


Make the file excecutable


<br>


~~~
!@NetOps
sudo chmod +x /usr/local/bin/tcp-6969-honeypot.py
~~~


<br>
<br>

---
&nbsp;


### STEP 3. Prevent the honeypot server from being coMpronised by assigning a nologin account to it.

~~~
!@NetOps
sudo useradd -r -s /sbin/nologin honeypot69 || true
sudo chown -R honeypot69:honeypot69 /var/log/tcp-6969-honeypot
~~~


<br>
<br>

---
&nbsp;


### STEP 4. Create a Systemd Service unit file

~~~
!@NetOps
nano /etc/systemd/system/tcp-6969-honeypot.service
~~~


<br>


Then paste the following


<br>


~~~
[Unit]
Description=A TCP Honeypot for port 6969
After=network.target

[Service]
User=honeypot69
Group=honeypot69
ExecStart=/usr/local/bin/tcp-6969-honeypot.py --host 0.0.0.0 --port 6969
Restart=on-failure
RestartSec=5
TimeoutStopSec=10
ProtectSystem=full
ProtectHome=yes
NoNewPrivileges=yes
PrivateTmp=yes
PrivateNetwork=no
ReadOnlyPaths=/usr
AmbientCapabilities=
SystemCallFilter=~@clock @cpu-emulation

[Install]
WantedBy=multi-user.target
~~~


<br>
<br>

---
&nbsp;


### STEP 5. Then start the service
~~~
!@NetOps
sudo systemctl daemon-reload
sudo systemctl start tcp-6969-honeypot.service
sudo systemctl status tcp-6969-honeypot.service --no-pager
~~~


<br>
<br>

---
&nbsp;


### STEP 6. OPTIONAL
If binding to ports below 1024 use the following systemd setup
~~~
NoNewPrivileges=No
AmbientCapabilities=CAP_NET_BIND_SERVICE
~~~


<br>
<br>

---
&nbsp;


### STEP 7. Set a Port Forwarding Rule for the Honeypot Server

~~~
!@EDGE-#$34T#
conf t
 ip nat inside source static tcp  10.#$34T#.1.11  6969  200.0.0.#$34T#  3306
 end
~~~


<br>
<br>

---
&nbsp;


## ZBF Security Zones

~~~
!@UTM-PH
clear ip nat trans *
clear ip nat trans *
clear ip nat trans *
conf t
 no ip nat inside source list NAT interface GigabitEthernet1 overload
 no ip nat inside source static tcp 10.11.11.100 22 208.8.8.100 2202 extendable
 no ip nat inside source static tcp 10.11.11.111 22 208.8.8.100 2222 extendable
 no ip nat inside source static tcp 10.11.11.111 80 208.8.8.100 8080 extendable
 no ip nat inside source static tcp 10.11.11.111 443 208.8.8.100 8443 extendable
 end
~~~


<br>


~~~
!@BLDG-JP-1
sudo su
ifconfig eth0 208.8.8.211 netmask 255.255.255.0 up
route add default gw 208.8.8.11
ping 208.8.8.11
~~~


<br>


Create a user account on __BLDG-JP-1__
~~~
!@BLDG-JP-1
sudo su
deluser admin
adduser admin

> pass
> pass
~~~ 


### Establish Zero Trust  

| INTERFACE | ZONE    |
| G1        | OUTSIDE |
| G2        | INSIDE  |
| G3        | INSIDE  |  
| G4        |         |


<br>


### Task 7: Modify SSH Port on Linux
*Prevent easy brute force attacks*

~~~
!@NetOps
nano /etc/ssh/sshd_config
~~~


<br>


Modify SSH Port:  
> Set Port to 2202


<br>


~~~
!@NetOps
sudo semanage port -a -t ssh_port_t -p tcp 2202
systemctl restart sshd
~~~


<br>


__SELinux__
SELinux defines access controls for the applications, processes, and files on a system. 
It uses security policies, which are a set of rules that tell SELinux what can or can’t be accessed, 
to enforce the access allowed by a policy. 


<br>
<br>

---
&nbsp;


# Site to Site Connectivity

~~~
!@EDGE-#$34T#
conf t
 no router ospf 1
 router ospf 1
  router-id #$34T#.0.0.1
  network 10.#$34T#.#$34T#.0 0.0.0.255 area 0
  default-information originate always
  end
~~~


<br>


~~~
!@EDGE-#$34T#
conf t
 no ip access-list extended NAT-POLICY
 ip access-list extended NAT-POLICY
  deny ip 10.#$34T#.0.0 0.0.255.255 10.11.0.0 0.0.255.255
  deny ip 10.#$34T#.0.0 0.0.255.255 10.12.0.0 0.0.255.255
  deny ip 10.#$34T#.0.0 0.0.255.255 10.21.0.0 0.0.255.255
  deny ip 10.#$34T#.0.0 0.0.255.255 10.22.0.0 0.0.255.255
  deny ip 10.#$34T#.0.0 0.0.255.255 10.31.0.0 0.0.255.255
  deny ip 10.#$34T#.0.0 0.0.255.255 10.32.0.0 0.0.255.255
  deny ip 10.#$34T#.0.0 0.0.255.255 10.41.0.0 0.0.255.255
  deny ip 10.#$34T#.0.0 0.0.255.255 10.42.0.0 0.0.255.255
  deny ip 10.#$34T#.0.0 0.0.255.255 10.51.0.0 0.0.255.255
  deny ip 10.#$34T#.0.0 0.0.255.255 10.52.0.0 0.0.255.255
  deny ip 10.#$34T#.0.0 0.0.255.255 10.61.0.0 0.0.255.255
  deny ip 10.#$34T#.0.0 0.0.255.255 10.62.0.0 0.0.255.255
  deny ip 10.#$34T#.0.0 0.0.255.255 10.71.0.0 0.0.255.255
  deny ip 10.#$34T#.0.0 0.0.255.255 10.72.0.0 0.0.255.255
  deny ip 10.#$34T#.0.0 0.0.255.255 10.81.0.0 0.0.255.255
  deny ip 10.#$34T#.0.0 0.0.255.255 10.82.0.0 0.0.255.255
  deny ip 10.#$34T#.0.0 0.0.255.255 10.91.0.0 0.0.255.255
  deny ip 10.#$34T#.0.0 0.0.255.255 10.92.0.0 0.0.255.255
  no deny ip 10.#$34T#.0.0 0.0.255.255 10.#$34T#.0.0 0.0.255.255
  permit ip any any
  end
~~~


<br>


~~~
!@EDGE-#$34T#
conf t
 int tun1
  ip add 172.16.1.#$34T# 255.255.255.0
  tunnel source g0/0/1
  tunnel mode gre multipoint
  no shut
  tun key 123
  ip nhrp authentication C1sc0123
  ip nhrp map multicast dynamic
  ip nhrp network-id 1337
  ip nhrp map 172.16.1.11 200.0.0.11
  ip nhrp map 172.16.1.12 200.0.0.12
  ip nhrp map 172.16.1.21 200.0.0.21
  ip nhrp map 172.16.1.22 200.0.0.22
  ip nhrp map 172.16.1.31 200.0.0.31
  ip nhrp map 172.16.1.32 200.0.0.32
  ip nhrp map 172.16.1.41 200.0.0.41
  ip nhrp map 172.16.1.42 200.0.0.42
  ip nhrp map 172.16.1.51 200.0.0.51
  ip nhrp map 172.16.1.52 200.0.0.52
  ip nhrp map 172.16.1.61 200.0.0.61
  ip nhrp map 172.16.1.62 200.0.0.62
  ip nhrp map 172.16.1.71 200.0.0.71
  ip nhrp map 172.16.1.72 200.0.0.72
  ip nhrp map 172.16.1.81 200.0.0.81
  ip nhrp map 172.16.1.82 200.0.0.82
  ip nhrp map 172.16.1.91 200.0.0.91
  ip nhrp map 172.16.1.92 200.0.0.92
  no ip nhrp map 172.16.1.#$34T# 200.0.0.#$34T#
  exit
 !
 !
 ip route 10.11.0.0 255.255.0.0 172.16.1.11 252
 ip route 10.12.0.0 255.255.0.0 172.16.1.12 252
 ip route 10.21.0.0 255.255.0.0 172.16.1.21 252
 ip route 10.22.0.0 255.255.0.0 172.16.1.22 252
 ip route 10.31.0.0 255.255.0.0 172.16.1.31 252
 ip route 10.32.0.0 255.255.0.0 172.16.1.32 252
 ip route 10.41.0.0 255.255.0.0 172.16.1.41 252
 ip route 10.42.0.0 255.255.0.0 172.16.1.42 252
 ip route 10.51.0.0 255.255.0.0 172.16.1.51 252
 ip route 10.52.0.0 255.255.0.0 172.16.1.52 252
 ip route 10.61.0.0 255.255.0.0 172.16.1.61 252
 ip route 10.62.0.0 255.255.0.0 172.16.1.62 252
 ip route 10.71.0.0 255.255.0.0 172.16.1.71 252
 ip route 10.72.0.0 255.255.0.0 172.16.1.72 252
 ip route 10.81.0.0 255.255.0.0 172.16.1.81 252
 ip route 10.82.0.0 255.255.0.0 172.16.1.82 252
 ip route 10.91.0.0 255.255.0.0 172.16.1.91 252
 ip route 10.92.0.0 255.255.0.0 172.16.1.92 252
 !
 no ip route 10.#$34T#.0.0 255.255.0.0 172.16.1.#$34T# 252
 end
~~~


<br>
<br>

---
&nbsp;


# Certificates

## CA Hierarchy

1. ROOT CA  
   - X509v3  
   - Basic Constraints [Critical]  
       - CA  
   - Key Usage [Critical]  
       - Certificate Sign  
       - CRL Sign  

<br>

2. SUB CA
   - X509v3
   - Basic Constraints [Critical]
       - CA
	   - Path Len: 0
   - Key Usage [Critical]
       - Certificate Sign
       - CRL Sign

<br>

3. LEAF CA
   - X509v3
   - Basic Constraints [Critical]
       - END-ENTITY
   - Extensions
       - Subject Alt Names
   - Key Usage [Critical]
       - Digital Signature
	   - Key Encipherment
	   - Data Encipherment
	   - Key Agreement
   - Extended Key Usage
       - TLS Web Server Authentication
       - TLS Web Client Authentication	   
	   - E-mail Protection
	   - IPSec End System
	   - IPSec Tunnel
	   - IPSec User
	   - IP Security end entity


&nbsp; 
---
&nbsp; 


## Certificates via Web Enrollment
Set Routes for trustpoints:
~~~
!@cmd
route add 208.8.8.11 mask 255.255.255.255 192.168.102.11
route add 208.8.8.12 mask 255.255.255.255 192.168.102.12
~~~


__TRUSTPOINTS__
~~~
!@UTM-PH
conf t
 crypto key generate rsa modulus 2048 label CERTKEY
 !
 crypto pki trustpoint SECSTORE
  enrollment url http://192.168.102.8/certsrv/mscep/mscep.dll
  serial-number
  fqdn utmph.sec#$34T#.com
  ip-address 208.8.8.11
  subject-name CN=UTM-PH,OU=NOC,O=RIVANCORP,L=MAKATI,ST=NCR,C=PH
  subject-alt-name utmph.sec#$34T#.com
  revocation-check none
  source interface GigabitEthernet2
  rsakeypair CERTKEY
  end
~~~

~~~
!@UTM-JP
conf t
 crypto key generate rsa modulus 2048 label CERTKEY
 !
 crypto pki trustpoint SECSTORE
  enrollment url http://192.168.102.8/certsrv/mscep/mscep.dll
  serial-number
  fqdn utmjp.sec#$34T#.com
  ip-address 208.8.8.12
  subject-name CN=UTM-JP,OU=NOC,O=RIVANCORP,L=TOKYO,ST=KANTO,C=JP
  subject-alt-name utmjp.sec#$34T#.com
  revocation-check none
  source interface GigabitEthernet2
  rsakeypair CERTKEY
  end
~~~

~~~
!@UTM-PH,UTM-JP
conf t
 crypto pki enroll SECSTORE
 end
~~~


__Phase 1 (IKEv2) & Phase 2 (IPSec)__
- Encryption
- Integrity
- DH

<br>

__Tunnel Properties__
- IP
- Source Interface
- Destination Peer IP
- Remote Subnets


&nbsp;
---
&nbsp;


### STEP 1 - Phase 1 (IKEv2)
~~~
!@UTM-PH
conf t
 crypto ikev2 proposal IKEV2-PROP
  encryption ______
  integrity ______
  group ______
 !
 crypto ikev2 policy IKEV2-POL
  proposal IKEV2-PROP
 !
 crypto ikev2 profile IKEV2-PROF
  match identity remote address __.__.__.__
  authentication remote rsa-sig
  authentication local rsa-sig
  pki trustpoint SECSTORE
  end
~~~

<br>

~~~
!@UTM-JP
conf t
 crypto ikev2 proposal IKEV2-PROP
  encryption ______
  integrity ______
  group ______
 !
 crypto ikev2 policy IKEV2-POL
  proposal IKEV2-PROP
 !
 crypto ikev2 profile IKEV2-PROF
  match identity remote address __.__.__.__
  authentication remote rsa-sig
  authentication local rsa-sig
  pki trustpoint SECSTORE
  end
~~~


&nbsp;
---
&nbsp;


### STEP 2 - Phase 2 (IPSEC)
~~~
!@UTM-PH, UTM-JP
conf t
 crypto ipsec transform-set TSET _____  _____
  mode ____
 !
 crypto ipsec profile VPN-IPSEC-PROF
  set transform-set TSET
  set ikev2-profile IKEV2-PROF
  end
~~~


&nbsp;
---
&nbsp;


### STEP 3 - Tunnel Properties

~~~
!@UTM-PH
conf t
 int tun1
  ip add __.__.__.__  __.__.__.__
  tunnel source ___
  tunnel destination __.__.__.__
  tunnel mode ipsec ipv4
  tunnel protection ipsec profile VPN-IPSEC-PROF
  end
~~~

<br>

~~~
!@UTM-JP
conf t
 int tun1
  ip add __.__.__.__  __.__.__.__
  tunnel source ___
  tunnel destination __.__.__.__
  tunnel mode ipsec ipv4
  tunnel protection ipsec profile VPN-IPSEC-PROF
  end
~~~


&nbsp;
---
&nbsp;


### STEP 4 - Remote Subnets / Interesting Traffic
~~~
!@UTM-PH
conf t
 ip route __.__.__.__  __.__.__.__  __.__.__.__
 end
~~~

<br>

~~~
!@UTM-JP
conf t
 ip route __.__.__.__  __.__.__.__  __.__.__.__
 end
~~~


<br>
<br>

---
&nbsp; 


## Certificates via OPENSSL

__IF USING TINYCORE FOR CA GENERATION__
~~~
!@BLDG-PH
mkdir certs; cd certs
~~~

<br>

~~~
!@BLDG-PH
vi /etc/resolve.conf


nameserver 8.8.8.8
~~~

<br>

EXIT OUT OF SUDO
~~~
!@BLDG-PH
tce-load -wi nano
echo nano.tcz >> /mnt/sda1/tce/onboot.lst
~~~

<br>
<br>


### STEP 1 - Creating Private Keys

__ROOT CA__
~~~
!@Linux
openssl genrsa -aes256 -out ca.key 2048
~~~

<br>

__INTERMEDIATE CA__
~~~
!@Linux
openssl genrsa -aes256 -out subca.key 2048
~~~


<br>
<br>

---
&nbsp;

### STEP 2 - Create an OPENSSL Configuration File for both the __ROOT CA__ & __INTERMEDIATE CA__

__ROOT CA__
~~~
!@Linux
nano ca.cnf
~~~

<br>

~~~
[ req ]
default_bits       = 2048
default_md         = sha256
prompt             = no
distinguished_name = dn
x509_extensions    = v3_ca


[ dn ]
C  = PH
ST = NCR
L  = Manila
O  = Rivancorp
OU = HQ
CN = Rivancorp Root CA


[ v3_ca ]
subjectKeyIdentifier   = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints       = critical, CA:TRUE
keyUsage               = critical, keyCertSign, cRLSign
~~~


<br>
<br>


__INTERMEDIATE CA__
~~~
!@Linux
nano subca.cnf
~~~

<br>

~~~
[ req ]
default_bits       = 2048
default_md         = sha256
prompt             = no
distinguished_name = dn
x509_extensions    = v3_subca


[ dn ]
C  = PH
ST = NCR
L  = Makati
O  = Rivancorp
OU = Makati Branch
CN = Rivancorp Intermediate CA


[ v3_subca ]
subjectKeyIdentifier   = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints       = critical, CA:TRUE, pathlen:0
keyUsage               = critical, keyCertSign, cRLSign
~~~


<br>
<br>

---
&nbsp;

### STEP 3 - Output the __ROOT CA__

__ROOT CA__
~~~
!@Linux
openssl req \
  -new -x509 \
  -key ca.key \
  -out ca.crt \
  -days 3650 \
  -extensions v3_ca \
  -config ca.cnf
~~~


<br>
<br>

---
&nbsp;

### STEP 4 - Generate CSR for the __INTERMEDIATE CA__

__INTERMEDIATE CA__
~~~
!@Linux
openssl req \
  -new \
  -key subca.key \
  -out subca.csr
~~~

<br>

~~~
Country Name (2 letter code) [XX]:                             PH
State or Province Name (full name) []:                         NCR
Locality Name (eg, city) [Default City]:                       Makati
Organization Name (eg, company) [Default Company Ltd]:         Rivancorp
Organizational Unit Name (eg, section) []:                     Makati Branch
Common Name (eg, your name or your server's hostname) []:      Rivancorp Intermediate CA


A challenge password []:                                       pass
An optional company name []:                                   
~~~


<br>
<br>

---
&nbsp;

### STEP 5 - Sign the CSR using the __ROOT CA__

~~~
!@Linux
openssl x509 \
  -req \
  -in subca.csr \
  -CA ca.crt \
  -CAkey ca.key \
  -CAcreateserial \
  -out subca.crt \
  -days 365 \
  -extensions v3_subca \
  -extfile subca.cnf
~~~


<br>
<br>

---
&nbsp;

### STEP 6 - Install the __ROOT CA__ & __INTERMEDIATE CA__ on Devices

__LINUX__
~~~
!@Linux
cp  ca.crt    /etc/pki/ca-trust/source/anchors/
cp  subca.crt    /etc/pki/ca-trust/source/
~~~

<br>

~~~
!@Linux
update-ca-trust enable
update-ca-trust
~~~

<br>


Verify
~~~
!@Linux
trust list | grep -i "Rivancorp"

openssl x509 -in utmph.crt -noout -issuer -subject
~~~


<br>
<br>


__WINDOWS__
~~~
!@Run
certlm.msc
~~~

<br>

Certificates 
  > Trusted Root Certification Authorities (Right-Click) 
    > All Tasks 
	  > Import

<br>

Certificates 
  > Intermediate Certification Authorities (Right-Click) 
    > All Tasks 
	  > Import


<br>
<br>


__CISCO__
~~~
!@Cisco
conf t
 crypto pki trustpoint RIVAN-CA
  enrollment terminal
  revocation-check crl none
  exit
 !
 crypto pki authenticate RIVAN-CA

 > Paste the CA
~~~


<br>
<br>

---
&nbsp;


### STEP 7 - Generate __LEAF CERTS__ or __END ENTITY CERTS__

__LINUX__  
~~~
!@Linux
openssl genrsa -aes256 -out utmph.key 2048
~~~

<br>

~~~
!@Linux
nano utmph.cnf 
~~~

<br>

~~~
[ req ]
default_bits       = 2048
default_md         = sha256
distinguished_name = dn
req_extensions     = v3_leaf_req
prompt             = no

[ dn ]
C  = PH
ST = NCR
L  = Makati
O  = Rivancorp
OU = Makati Branch
CN = ph.rivancorp.com

[ v3_leaf_req ]
basicConstraints    = critical, CA:false
keyUsage            = critical, digitalSignature, keyEncipherment
extendedKeyUsage    = serverAuth, clientAuth, ipsecEndSystem, ipsecTunnel, ipsecUser, ipsecIKE
subjectAltName      = @alt_names

[ alt_names ]
DNS.1   = utmph.rivancorp.com
IP.1    = 208.8.8.11
~~~

<br>

~~~
!@Linux
openssl req \
  -new \
  -key utmph.key \
  -out utmph.csr \
  -config utmph.cnf
~~~

<br>

~~~
!@NetOps
openssl x509 \
  -req \
  -in utmph.csr \
  -CA subca.crt \
  -CAkey subca.key \
  -CAcreateserial \
  -out utmph.crt \
  -days 30 \
  -extensions v3_leaf_req \
  -extfile utmph.cnf
~~~


<br>
<br>

---
&nbsp;


__WINDOWS__
~~~
!@Run
certlm.msc
~~~

<br>

Certificates   
  > Personal (Right-Click)   
    > All Tasks   
	  > Advance Operations   
	    > Create Custom Request  


<br>
<br>

---
&nbsp;


__CISCO__

~~~
!@UTM-PH
conf t
 crypto key generate rsa modulus 2048 label RIVANPH-KEY exportable
 end
~~~

<br>

~~~
!@UTM-PH
conf t
 crypto pki trustpoint RIVAN-PH
  enrollment terminal
  revocation-check crl none
  rsakeypair RIVANPH-KEY
  exit
 !
 
 
 crypto pki authenticate RIVAN-PH
 
 
 crypto pki enroll RIVAN-PH
 
~~~

<br>


-----BEGIN CERTIFICATE REQUEST-----

-----END CERTIFICATE REQUEST-----


<br>


~~~
!@NetOps
openssl x509 \
  -req \
  -in utmph.csr \
  -CA subca.crt \
  -CAkey subca.key \
  -CAcreateserial \
  -out utmph.crt \
  -days 30 \
  -extensions v3_leaf_req \
  -extfile utmph.cnf
~~~


<br>


~~~
!@UTM-PH
conf t
 crypto pki import RIVAN-PH certificate
 
~~~


<br>
<br>

---
&nbsp;


### ACTIVITY - Issue A Certificate for RIVAN-JP

~~~
!@Linux
nano utmjp.cnf 
~~~

<br>

~~~
[ req ]
default_bits       = 2048
default_md         = sha256
distinguished_name = dn
req_extensions     = v3_leaf_req
prompt             = no

[ dn ]
C  = JP
ST = Kanto
L  = Tokyo
O  = Rivancorp
OU = Tokyo Branch
CN = jp.rivancorp.com

[ v3_leaf_req ]
basicConstraints    = critical, CA:false
keyUsage            = critical, digitalSignature, keyEncipherment
extendedKeyUsage    = serverAuth, clientAuth, ipsecEndSystem, ipsecTunnel, ipsecUser, ipsecIKE
subjectAltName      = @alt_names

[ alt_names ]
DNS.1   = utmjp.rivancorp.com
IP.1    = 208.8.8.12
~~~


~~~
!@UTM-JP
conf t
 crypto key generate rsa modulus 2048 label RIVANJP-KEY exportable
 end
~~~

<br>

~~~
!@UTM-JP
conf t
 crypto pki trustpoint RIVAN-JP
  enrollment terminal
  revocation-check crl none
  rsakeypair RIVANJP-KEY
  exit
 !
 
 
 crypto pki authenticate RIVAN-JP
 
 
 crypto pki enroll RIVAN-JP
 
~~~

<br>


-----BEGIN CERTIFICATE REQUEST-----

-----END CERTIFICATE REQUEST-----


<br>


~~~
!@NetOps
openssl x509 \
  -req \
  -in utmjp.csr \
  -CA subca.crt \
  -CAkey subca.key \
  -CAcreateserial \
  -out utmjp.crt \
  -days 30 \
  -extensions v3_leaf_req \
  -extfile utmjp.cnf
~~~


<br>


~~~
!@UTM-JP
conf t
 crypto pki import RIVAN-JP certificate
 
~~~


<br>
<br>

---
&nbsp;


## Site to Site VPN (via RSA-SIG Authentication)

### STEP 1 - PHASE 1 (IKEv2)

~~~
!@UTM-PH
conf t
 crypto ikev2 proposal IKEV2-PROP
  encryption __-__-__
  integrity __
  group __
 !
 crypto ikev2 policy IKEV2-POL
  proposal IKEV2-PROP
 !
 ! crypto ikev2 keyring VPN-KEYRING
  ! peer RIVANJP-PEER
  !  address __.__.__.__
  !  pre-shared-key _________
 !
 crypto ikev2 profile IKEV2-PROF
  match identity remote address __.__.__.__  __.__.__.__
  authentication remote rsa-sig
  authentication local rsa-sig
  pki trustpoint RIVAN-PH
  ! keyring local VPN-KEYRING
   end
~~~


<br>


~~~
!@UTM-JP
conf t
 crypto ikev2 proposal IKEV2-PROP
  encryption __-__-__
  integrity __
  group __
 !
 crypto ikev2 policy IKEV2-POL
  proposal IKEV2-PROP
 !
 ! crypto ikev2 keyring VPN-KEYRING
  ! peer RIVANPH-PEER
  !  address __.__.__.__
  !  pre-shared-key _________
 !
 crypto ikev2 profile IKEV2-PROF
  match identity remote address __.__.__.__  __.__.__.__
  authentication remote rsa-sig
  authentication local rsa-sig
  pki trustpoint RIVAN-JP
  ! keyring local VPN-KEYRING
   end
~~~


<br>
<br>

---
&nbsp;


### STEP 2 - PHASE 2 (IPSEC)

~~~
!@UTM-PH, UTM-JP
conf t
 crypto ipsec transform-set TSET __-__  ___  __-__-__
  mode transport
 !
 crypto ipsec profile VPN-IPSEC-PROF
  set transform-set TSET
  set ikev2-profile IKEV2-PROF
  end
~~~


<br>
<br>

---
&nbsp;


### STEP 3 - TUNNEL PROPERTIES

~~~
!@UTM-PH
conf t
 int tun1
  ip add __.__.__.__  __.__.__.__
  tunnel source __
  tunnel destination __.__.__.__
  tunnel mode ipsec ipv4
  tunnel protection ipsec profile VPN-IPSEC-PROF
  end
~~~

<br>

~~~
!@UTM-JP
conf t
 int tun1
  ip add __.__.__.__  __.__.__.__
  tunnel source __
  tunnel destination __.__.__.__
  tunnel mode ipsec ipv4
  tunnel protection ipsec profile VPN-IPSEC-PROF
  end
~~~


<br>
<br>

---
&nbsp;


### STEP 4 - Remote Subnets / Interesting Traffic

~~~
!@UTM-PH
conf t
 ip route __.__.__.__   __.__.__.__   __.__.__.__
 ip route __.__.__.__   __.__.__.__   __.__.__.__
 end
~~~

<br>

~~~
!@UTM-JP
conf t
 ip route __.__.__.__   __.__.__.__   __.__.__.__
 end
~~~


<br>
<br>

---
&nbsp;


## Secure Protocols (TLS)

- Website
- File Transfer
- Mail

### FTPS

### STEP 1 - Transfer __ROOT CA__ & __INTERMEDIATE CA__ to Windows

__WINDOWS__
Internet Information Services
  > Create FTP Site
    > Require SSL  


<br>

### Convert to  `.pfx`

~~~
!@Linux
openssl pkcs12 -export \
-in endpoint.crt \
-inkey endpoint.key \
-out endpoint.pfx
~~~


<br>


~~~
!@Linux
cd /certs

ftp 192.168.102.1
put ca.crt
put subca.crt
put endpoint.pfx
~~~

<br>

~~~
!@Linux
cd /certs
cat ca.crt subca.crt linux.crt > fullchain.pem
lftp -u administrator 192.168.102.1
set ssl:ca-file fullchain.pem
set ftp:ssl-force true
set ftp:ssl-protect-data true
~~~


<br>
<br>

---
&nbsp;


## PKCS Types

### 1. PKCS#1 – RSA Cryptography Standard
- Defines the format for RSA public and private keys and the algorithms for RSA encryption and signature.
- Generating RSA keys.


<br>
<br>


### 2. PKCS#3 – Diffie-Hellman Key Agreement Standard
- Specifies how to perform the Diffie-Hellman key exchange for secure symmetric keys.
- Establishing shared secret keys in a secure channel.


<br>
<br>


### 3. PKCS#5 – Password-Based Encryption (PBE)
- Defines how to derive cryptographic keys from passwords and encrypt data using them.
- Protecting private keys with a password.
- password protection / encryption


<br>
<br>


### 4. PKCS#7 – Cryptographic Message Syntax Standard
- Standard for signing and encrypting messages and certificates.
- Import/export certificate chains in Windows/Java.
- certificates (no key), often for chains


<br>
<br>


### 5. PKCS#8 – Private-Key Information Syntax
- Standard format for storing private keys, can include encryption.
- PKCS#1 is only RSA keys; PKCS#8 supports multiple algorithms (RSA, DSA, EC).
- private keys


<br>
<br>


### 6. PKCS#10 – Certificate Signing Request (CSR)
- Standard for requesting a certificate from a Certificate Authority (CA).
- Public key, identity information (Common Name, Org), optional attributes.
- certificate requests


<br>
<br>


### 7. PKCS#12 – Personal Information Exchange
- Securely store and transport private keys + certificates.
- Always password-protected.
- Import/export keys and certificates across platforms (Windows, IIS, Java keystores).
- secure bundle of key + certificate



<br>
<br>

---
&nbsp;


# Forward Proxy

~~~
!@UTM-PH
conf t
 int g1
  ip nat outside
 int g3
  ip nat inside
 int tun1
  ip nat inside
 !
 no  ip access-list extended NAT
 ip access-list extended NAT
  deny ip 11.11.11.0 0.0.0.31  10.21.21.208 0.0.0.15
  deny ip 11.11.11.0 0.0.0.31  10.22.22.192 0.0.0.63
  permit ip any any
 !
 ip nat inside source list NAT int g1
 ip route 0.0.0.0 0.0.0.0 208.8.8.2
 !
 !
 no  ip access-list extended NETOPS-PBR
 ip access-list extended NETOPS-PBR
  deny ip 11.11.11.0 0.0.0.31  10.21.21.208 0.0.0.15
  deny ip 11.11.11.0 0.0.0.31  10.22.22.192 0.0.0.63
  permit ip host 10.11.11.100 any
  exit
 route-map PBR-TO-JP permit 10
  match ip address NETOPS-PBR
  set ip next-hop 172.16.1.2
  exit
 !
 int g3
  ip policy route-map PBR-TO-JP
 end
~~~


<br>


~~~
!@UTM-JP
conf t
 int g1
  ip nat outside
 int g3
  ip nat inside
 int tun1
  ip nat inside
 !
 ip access-list extended NAT
  deny ip 10.21.21.208 0.0.0.15  10.11.11.96 0.0.0.31
  deny ip 10.22.22.192 0.0.0.63  10.11.11.96 0.0.0.31
  permit ip any any
 !
 ip nat inside source list NAT int g1
 ip route 0.0.0.0 0.0.0.0 208.8.8.2
 end
~~~


<br>
<br>


~~~
!@NetOps
nano /etc/sockd.conf
~~~


<br> 


~~~
logoutput: syslog
internal: ens192 port = 1080
external: eth224

method: none

client pass {
  from: 0.0.0.0/0 to: 0.0.0.0/0
}

pass {
  from: 0.0.0.0/0 to: 0.0.0.0/0
  protocol: tcp udp
}
~~~


<br>


~~~
!@NetOps-PH
systemctl start sockd
~~~


<br>


Access Firefox Proxy Settings  
- Manual Proxy  
- SOCKS5 host IP of NetOps port 1080  
- Proxy DNS  


<br>
<br>

---
&nbsp;


# Key Management

~~~
!@NetOps
mkdir /keys;cd /keys
ssh-keygen -t rsa -b 2048 -f rivan
fold -b -w 72 rivan.pub
~~~


<br>


~~~
!@UTM-PH
conf t
 username rivan privilege 15 secret pass
 ip ssh pubkey-chain
  username rivan
   key-string
   
~~~


<br>


~~~
!@UTM-PH
conf t
 ip ssh server algorithm authentication publickey
 no ip ssh server algorithm authentication password
 no ip ssh server algorithm authentication keyboard
 end
~~~



<br>
<br>

---
&nbsp;


## Exam Question:

### 1. Which of the following would most likely be deployed to obtain and analyze attacker
activity and techniques?
- [ ] __A.__ Firewall
- [ ] __B.__ IDS
- [ ] __C.__ Honeypot
- [ ] __D.__ Layer 3 switch


&nbsp;
---
&nbsp;


### 2. Employees located off-site must have access to company resources in order to complete
their assigned tasks. These employees utilize a solution that allows remote access
without interception concerns. Which of the following best describes this solution?
- [ ] __A.__ Proxy server
- [ ] __B.__ NGFW
- [ ] __C.__ VPN
- [ ] __D.__ Security zone


&nbsp;
---
&nbsp;


### 3. A company wants to ensure that a mission-critical database can only be accessed from
specific internal IP addresses. Which of the following should the company deploy to
meet this requirement?
- [ ] __A.__ Web application firewall
- [ ] __B.__ Network tap
- [ ] __C.__ Intrusion prevention system
- [ ] __D.__ Jump server


&nbsp;
---
&nbsp;


### 4. An administrator is creating a secure method for a contractor to access a test
environment. Which of the following would provide the contractor with the best access to
the test environment?
- [ ] __A.__ Application server
- [ ] __B.__ Jump server
- [ ] __C.__ RDP server
- [ ] __D.__ Proxy server

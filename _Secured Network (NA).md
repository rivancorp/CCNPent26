
<!-- Your monitor number = #$34T# -->


## ⛅ Warm Up for Day 4.

### Setup your Day 1 BUT DO NOT configure any device.

<br>

## 🎯 Review
### 1. Which two encoding methods are supported by REST APIs? (Choose Two)
	A. SGML
	B. YAML
	C. XML
	D. JSON
	E. EBCDIC

<br>

### 2. Which output displays a JSON data representation?
A.
~~~
	{
	  "response":{
	  "taskld":{};
	  "url": "string"
	  };
	  "version": "string"
	}
~~~

<br>

B.
~~~
	{
	  "response"-{
	  "taskld"-{},
	  "url"- "string"
	  },
	  "version"- "string"
	}
~~~

<br>

C.
~~~
	{
	  "response":{
	  "taskld":{},
	  "url": "string"
	  },
	  "version": "string"
	}
~~~

<br>

D.
~~~
	{
	  "response",{
	  "taskld",{};
	  "url", "string"
	  };
	  "version", "string"

	}
~~~

&nbsp;
---
&nbsp;

### 3. OSPF Configuration
Refer to the Topology.   
All routers are configured with IP addressing EXCEPT for the link between R1 & R3.  
Finish the configuration by completing the ff:  

<br>

Task 1.  
	Using the 172.16.160.0/21 network,   
	- assign the 1st VALID ip address on R1's e0/1 interface.
	- assign the Last VALID ip address on R3's e0/1 interface.

<br>

Task 2.  
	Configure single area OSPF on all routers with the ff:  
	- All routers belong to AREA 2
	- Use OSPF Process 5
	- Assign each router's LOOPBACK 0 as their router ID.
	- The link between R2 & R3 must be point-to-point.
	- R1 must have the lowest priority on its e0/0 & e0/1 interfaces.
	- Advertise all Connected routes.

<br>

Task 3.  
	Configure static routes.  
	- Configure a static host route on R1 destined for R3's LOOPBACK 
	using R3 as the next hop.
	- Configure a floating static route on R1 destined for R3's LOOPBACK 
	using R2 as the next hop with an Administrative Distance equal 
	to EXTERNAL EIGRP.

<br>
<br>

ANSWER HERE  
Task 1.  
~~~
@R1
conf t
 interface ___
  ip add ___.___.___.___  ___.___.___.___
  no shut
  end

@R3
conf t
 interface ___
  ip add ___.___.___.___  ___.___.___.___
  no shut
  end
~~~

<br>

Task 2.
~~~
@R1
conf t
 interface ___
  ip ospf ___ ___
 interface ___
  ip ospf ___ ___
 router ospf ___
  router-id ___.___.___.___
  network ___.___.___.___  ___.___.___.___ area ___
  network ___.___.___.___  ___.___.___.___ area ___
  network ___.___.___.___  ___.___.___.___ area ___
  end

@R2
conf t
 interface ___
  ip ospf ___ ___
  exit
 router ospf ___
  router-id ___ 
  network ___.___.___.___  ___.___.___.___ area ___
  network ___.___.___.___  ___.___.___.___ area ___
  network ___.___.___.___  ___.___.___.___ area ___
  end
  
@R3
conf t
 interface ___
  ip ospf ___ ___
  exit
 router ospf 5
  router-id ___ 
  network ___.___.___.___  ___.___.___.___ area ___
  network ___.___.___.___  ___.___.___.___ area ___
  network ___.___.___.___  ___.___.___.___ area ___
  end
~~~  

<br>

Task 3.
~~~
@R1
conf t
 ip route 192.168.5.33 255.255.255.___  ___.___.___.___
 ip route 192.168.5.33 255.255.255.___  ___.___.___.___  ___
 end
~~~

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

<details>
<summary>Show Answer</summary>

ANSWERS:  
Task 1.  
~~~
@R1
conf t
 interface e0/1
  ip add 172.16.160.1 255.255.248.0
  no shut
  end

@R3
conf t
 interface e0/1
  ip add 172.16.167.254 255.255.248.0
  no shut
  end
~~~

<br>

Task 2.
~~~
@R1
conf t
 interface e0/0
  ip ospf priority 0
 interface e0/1
  ip ospf priority 0
 router ospf 5
  router-id 172.16.97.111
  network 172.16.97.111 0.0.0.0 area 2
  network 10.0.15.64 0.0.0.31 area 2
  network 172.16.160.0 0.0.7.255 area 2
  end

@R2
conf t
 interface e0/1
  ip ospf network point-to-point
  exit
 router ospf 5
  router-id 10.46.187.22 
  network 10.46.187.22 0.0.0.0 area 2
  network 10.0.15.64 0.0.0.31 area 2
  network 192.168.0.152 0.0.0.3 area 2
  end
  
@R3
conf t
 interface e0/0
  ip ospf network point-to-point
  exit
 router ospf 5
  router-id 192.168.5.33
  network 192.168.5.33 0.0.0.0 area 2
  network 192.168.0.152 0.0.0.3 area 2
  network 172.16.160.0 0.0.7.255 area 2
  end
~~~

<br>

Task 3.
~~~
@R1
conf t
 ip route 192.168.5.33 255.255.255.255 172.16.167.254
 ip route 192.168.5.33 255.255.255.255 10.0.15.90 170
 end
~~~

</details>


<br>
<br>

---
&nbsp;


## OSI Layer
PDU (Protocol Data Unit)

<br>

### 7. Application Layer 
"Underlying service that supports applications"
- SMTP
- Resource sharing
- Service advertisement
- Gramaphone

<br>

### 6. Presentation Layer - File extension
- __Data__
- Anything "-tion" (encryption, )
- .wav .jpg .au .exe .psd

<br>

### 5. Session Layer - Session established
- __Data Stream__
- Session Established, Session Terminated
- Commands: netstat -s -p tcp, telnet, ssh

<br>

__SPAN__
~~~
!@CoreBABA
conf t
 monitor session 1 source interface fa0/3,fa0/5,fa0/7
 monitor session 1 destination interface fa0/1,fa0/9
 end
~~~

<br>

Real-time Transport Protocol

<br>

~~~
!@cmd
nmap -sP 10.k.1.0/24
nmap -v 10.k.1.10
~~~

<br>

What ports are revealed?  
139? 445?  

<br>

That's a vulnerable system. No Firewall. Attack it.
~~~
!@cmd
net use \\10.k.1.10\ipc$     Privilege Escalation
net use x: /delete
net use x: \\10.k.1.10\c$
~~~

<br>

### 4. Transport Layer - TCP/UDP
- __Segments__
- Three way handshake
- TCP Sliding Window
- Commands: `nmap -v 10.#$34T#.1.10`

<br>

- __Well-known ports__ 0 - 1023
- __Registered ports__ 1024 - 49151
- __Ephemeral/Dynamic__ ports 49152 - 65535

<br>

### 3. Network Layer - IP addresses
- __Packets__
- Routing protocols
- Forwarding packets
- Commands: `show ip int br` `sh ip route`

<br>

### 2. Data-link Layer - MAC Addresses
- __Frames__
- FCS
- Preamble

<br>

- Commands: `show vlan brief`

<br>

### 1. Physical Layer - "Things you touch"
- __Bits__
- Speed = b 
- Data = B

<br>

- Commands: `show cdp neigh`


<br>
<br>

---
&nbsp;


## Lab Setup

### 1. Deploy

Devices:
- 2x CSR1000v
- 1x NetOps
- 3x TinyCore (yvm.ova)

CSR1000v:
  Name: UTM-PH
  
  | NetAdapter   |        |
  | ---          | ---    |
  | NetAdapter   | NAT    |
  | NetAdapter 2 | VMNet2 |
  | NetAdapter 3 | VMNet3 |
  

CSR1000v:
  Name: UTM-JP
  
  | NetAdapter   |        |
  | ---          | ---    |
  | NetAdapter   | NAT    |
  | NetAdapter 2 | VMNet2 |
  | NetAdapter 3 | VMNet4 |

NetOps:
  Name: NetOps-PH
  
  | NetAdapter   |                    |
  | ---          | ---                |
  | NetAdapter   | VMNet1             |
  | NetAdapter 2 | VMNet2             |
  | NetAdapter 3 | VMNet3             |
  | NetAdapter 4 | Bridge (Replicate) |

TinyCore (yvm.ova):
  Name: BLDG-PH
  
  | NetAdapter   |                    |
  | ---          | ---                |
  | NetAdapter   | VMNet3             |
  
TinyCore (yvm.ova):
  Name: BLDG-JP-1
  
  | NetAdapter   |                    |
  | ---          | ---                |
  | NetAdapter   | VMNet4             |

TinyCore (yvm.ova):
  Name: BLDG-JP-2
  
  | NetAdapter   |                    |
  | ---          | ---                |
  | NetAdapter   | VMNet4             |


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
 !
 username admin privilege 15 secret pass
 ip http server
 ip http secure-server
 ip http authentication local
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
 !
 username admin privilege 15 secret pass
 ip http server
 ip http secure-server
 ip http authentication local
 end
wr
!
~~~

<br>

~~~
!@BLDG-PH
sudo su
hostname BLDG-PH
ifconfig eth0 10.11.11.101 netmask 255.255.255.224 up
route add default gw 10.11.11.113
ping 10.11.11.113
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


### NetOps-PH Setup
> Login: root
> Pass: C1sc0123

1. Get the MAC Address for the Bridge connection
VMWare > NetOps-PH Settings > NetAdapter (2, 3, & 4) > Advance > MAC Address

| NetAdapter   | MAC Address      | VM Interface |
| NetAdapter 2 | ___.___.___.___  | ens___       |  ens192
| NetAdapter 3 | ___.___.___.___  | ens___       |  ens224
| NetAdapter 4 | ___.___.___.___  | ens___       |  ens256

<br>

2. Get Network-VM Mapping
~~~
!@NetOps-PH
ip -br link
~~~

<br>

3. Modify Interface IP
VMNet2:  192.168.102.6/24
VMNet3:  10.11.11.100/27
Bridged: 10.#$34T#.1.6/24

<br>

~~~
!@NetOps-PH
ifconfig ens192 192.168.102.6 netmask 255.255.255.0 up
ifconfig ens224 10.11.11.100 netmask 255.255.255.224 up
ifconfig ens256 10.#$34T#.1.6 netmask 255.255.255.0 up
~~~

<br>

Verify:
~~~
!@NetOps-PH
ip -4 addr

nmcli connection show
netstat -rn
~~~

<br>

or  

<br>

__Using Network Management CLI for persistent IP.__

<br>

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


### Remote Access
Connect to Management Interfaces of Devices

NetOps-PH: 192.168.102.6
UTM-PH: 192.168.102.11
UTM-JP: 192.168.102.12


<br>
<br>

---
&nbsp;

## Site-to-Site VPN & Cryptography

### 1. Create a Key Pair for both UTM Routers

~~~
!@UTM-PH & UTM-JP
conf t
 ip domain name sec.plus
 !
 crypto key generate rsa modulus 2048 label key exportable
 ip ssh version 2
 ip ssh rsa keypair-name key
 end
show ip ssh
~~~

<br>


View Private & Public Keys
~~~
!@UTM-PH & UTM-JP
conf t
 crypto key export rsa key pem terminal aes C1sc0123
 end
~~~

<br>

### Diffie-Hellman Key Exchange 
Ref: https://sec.cloudapps.cisco.com/security/center/resources/next_generation_cryptography

<br>

| DH GROUP | MODP                      | EC  |
| ---      | ---                       | --- |
| 1 	   | 768                       |     |
| 2 	   | 1024                      |     |
| 5 	   | 1536                      |     |
| 14 	   | 2048                      |     |
| 15 	   | 3072                      |     |
| 16 	   | 4096                      |     |
| 17 	   | 6144                      |     |
| 18 	   | 8192                      |     |
| 19       |                           | 256 |
| 20       |                           | 384 |
| 21       |                           | 521 |
| 24       | 2048 Prime order 256 bits |     |

<br>

~~~
!@NetOps-PH
mkdir crypto; cd crypto
openssl dhparam -out dhgroup.pem 1024
~~~

<br>

~~~
openssl genpkey -paramfile dhgroup.pem -out ph_priv.key
openssl genpkey -paramfile dhgroup.pem -out jp_priv.key
~~~

<br>

~~~
openssl pkey -in ph_priv.key -pubout -out ph_pub.key
openssl pkey -in jp_priv.key -pubout -out jp_pub.key
~~~

<br>

~~~
openssl pkeyutl -derive -inkey ph_priv.key -peerkey jp_pub.key -out ph-shared.secret
openssl pkeyutl -derive -inkey jp_priv.key -peerkey ph_pub.key -out jp-shared.secret
~~~

<br>

View Private key contents
~~~
!@NetOps
openssl pkey -in rsa_priv.key -text -noout
~~~

<br>

### 2. Access the GUI of Both UTM Routers

UTM-PH: https://192.168.102.11
UTM-JP: https://192.168.102.12

<br>

Wireshark: ssh vs telnet vs esp





































VPN Cli Configuration:
~~~
!@UTM-PH
conf t
 crypto ikev2 proposal WEBUI-PROPOSAL-Tunnel1 
  encryption aes-cbc-256
  integrity sha512
  group 14
  exit
 crypto ikev2 policy WEBUI-POLICY 
  match fvrf any
  proposal WEBUI-PROPOSAL-Tunnel1
  exit
 crypto ikev2 keyring WEBUI-KEYS
  peer WEBUI-PEER-208.8.8.12
   description KEY-PEER-208.8.8.12 
   address 208.8.8.12 255.255.255.0
   pre-shared-key C1sc0123
   exit
  exit
 crypto ikev2 profile WEBUI-IKEV2-PROFILE
  match fvrf any
  match identity remote address 208.8.8.12 255.255.255.255 
  authentication remote pre-share
  authentication local pre-share
  keyring local WEBUI-KEYS
  exit
 crypto ipsec transform-set WEBUI-TS-Tunnel1 esp-aes 256 esp-sha512-hmac 
  mode tunnel
  exit
 crypto ipsec profile WEBUI-IPSEC-PROFILE-Tunnel1
  set transform-set WEBUI-TS-Tunnel1 
  set ikev2-profile WEBUI-IKEV2-PROFILE
  exit
 interface Tunnel1
  description From PH to JP 
  ip address 172.16.1.1 255.255.255.252
  tunnel source GigabitEthernet1
  tunnel mode ipsec ipv4
  tunnel destination 208.8.8.12
  tunnel protection ipsec profile WEBUI-IPSEC-PROFILE-Tunnel1
  exit
 ip route 10.22.22.192 255.255.255.192 Tunnel1
 ip route 10.21.21.208 255.255.255.240 Tunnel1
 end
~~~

<br>

~~~
!@UTM-JP
conf t
 crypto ikev2 proposal WEBUI-PROPOSAL-Tunnel1 
  encryption aes-cbc-256
  integrity sha512
  group 14
  exit
 crypto ikev2 policy WEBUI-POLICY 
  match fvrf any
  proposal WEBUI-PROPOSAL-Tunnel1
  exit
 crypto ikev2 keyring WEBUI-KEYS
  peer WEBUI-PEER-208.8.8.11
   description KEY-PEER-208.8.8.11 
   address 208.8.8.11 255.255.255.0
   pre-shared-key C1sc0123
   exit
  exit
 crypto ikev2 profile WEBUI-IKEV2-PROFILE
  match fvrf any
  match identity remote address 208.8.8.11 255.255.255.255 
  authentication remote pre-share
  authentication local pre-share
  keyring local WEBUI-KEYS
  exit
 crypto ipsec transform-set WEBUI-TS-Tunnel1 esp-aes 256 esp-sha512-hmac 
  mode tunnel
  exit
 crypto ipsec profile WEBUI-IPSEC-PROFILE-Tunnel1
  set transform-set WEBUI-TS-Tunnel1 
  set ikev2-profile WEBUI-IKEV2-PROFILE
  exit
 interface Tunnel1
  description From JP to PH 
  ip address 172.16.1.2 255.255.255.252
  tunnel source GigabitEthernet1
  tunnel mode ipsec ipv4
  tunnel destination 208.8.8.11
  tunnel protection ipsec profile WEBUI-IPSEC-PROFILE-Tunnel1
  exit
 ip route 10.11.11.96 255.255.255.224 Tunnel1
 end
~~~

&nbsp;
---
&nbsp;

~~~
!@UTM-PH
conf t
 ip route 10.22.22.192 255.255.255.192 208.8.8.12
 ip route 10.21.21.208 255.255.255.240 208.8.8.12
 end
~~~

<br>

~~~
!@UTM-JP
conf t
 ip route 10.11.11.96 255.255.255.224 208.8.8.11
 end
~~~

&nbsp;
---
&nbsp;

## Internet Connectivity
1. Specify INSIDE and OUTSIDE interfaces
~~~
!@edge
conf t
 int g0/0/0 
  ip nat inside
  exit
 int g0/0/1
  ip nat outside
  end
~~~

<br>

2. Match Traffic
~~~
!@edge
conf t
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

3. Apply INSIDE NAT/PAT
~~~
!@edge
conf t
 ip nat inside source list NAT-POLICY int g0/0/1 overload
 ip route 0.0.0.0 0.0.0.0 200.0.0.1
 end
~~~

<br>

4. Set Default Gateway and DNS
~~~
!@edge
conf t
 no router ospf 1
 router ospf 1
  router-id #$34T#.0.0.1
  network 10.#$34T#.#$34T#.0 0.0.0.255 area 0
  default-information originate always
  exit
 ip name-server 10.#$34T#.1.10   !8.8.8.8 8.8.4.4
 ip domain lookup
 ip domain lookup source-int g0/0/0
 end
~~~

<br>

### mGRE Tunnel (Full Mesh)
~~~
!@edge
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
  end
~~~

<br>

~~~
!@edge
conf t
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

## Forward Proxy & Proxy Chaining

NAT:
1. Specify INSIDE and OUTSIDE interfaces
~~~
!@UTM-PH & UTM-JP
conf t
 int g1
  ip nat outside
 int g3
  ip nat inside
  end
~~~

<br>

2. Match Traffic
~~~
!@UTM-PH
conf t
 ip access-list extended NAT
  deny ip 10.11.11.96 0.0.0.31 10.21.21.208 0.0.0.15
  deny ip 10.11.11.96 0.0.0.31 10.22.22.192 0.0.0.63
  permit ip any any
  end
~~~

<br>

~~~
!@UTM-JP
conf t
 ip access-list extended NAT
  deny ip 10.21.21.208 0.0.0.15 10.11.11.96 0.0.0.31 
  deny ip 10.22.22.192 0.0.0.63 10.11.11.96 0.0.0.31 
  permit ip any any
  end
~~~

<br>

3. Apply INSIDE NAT/PAT
~~~
!@UTM-PH & UTM-JP
conf t
 ip nat inside source list NAT int g1 overload
 end
~~~

<br>

4. Set Default Gateway and DNS
~~~
!@UTM-PH & UTM-JP
conf t
 ip route 0.0.0.0 0.0.0.0 208.8.8.2
 ip domain lookup
 ip domain lookup source-int g2
 ip name-server 8.8.8.8 1.1.1.1
 end
~~~

<br>

~~~
!@NetOps-PH & BLDG-JP-1 & BLDG-JP-2
vi /etc/resolv.conf
~~~

<br>

Forward Proxy
~~~
!@BLDG-JP-1
ssh -l root 10.11.11.100

> (yes/no) yes
~~~

<br>
<br>

---
&nbsp;

## Syslogs

~~~
!@UTM-PH
conf t
 service timestamps log datetime msec
 service sequence-numbers
 !
 archive
  log config
   notify syslog
   exit
  exit
 ip nat log translations syslog bind-only 
 !
 logging host 192.168.102.133 transport udp port 9001
 logging source-interface g2
 logging on
end
~~~


<br>
<br>

---
&nbsp;


### Log Severity Levels

*log.level : "informational"  or log.level : "notification" or log.level : "warning" or log.level : "error" or log.level : "critical" or log.level : "alert" or log.level : "emergency"*


~~~
!@R4
conf t
 service timestamps log datetime msec
 service sequence-numbers
 logging host 208.8.8.133
 logging source-interface e3/3
 logging on
 logging facility local7
 !
 logging trap 6
end
~~~

<br>

__Track Logins__
~~~
!@R4
conf t
 username admin priv 15 secret pass
 ip domain name SYSLOG.COM
 crypto key generate rsa modulus 2048 
 ip ssh version 2
 !
 line vty 0 4
  transport input all
  login local
  login on-success log
  login on-failure log
  end
~~~


<br>
<br>

---
&nbsp;


### NetFlow

~~~
!@Cisco CSR1000v
conf t
 no flow record FLOW_RECORD
 flow record FLOW_RECORD
  match ipv4 source address
  match ipv4 destination address
  match transport source-port
  match transport destination-port
  match ipv4 protocol
  match ipv4 version
  match interface input
  match interface output
  collect counter bytes
  collect counter packets
  collect transport tcp flags
  collect flow direction
  collect timestamp absolute first
  collect timestamp absolute last
  !
  collect ipv4 tos
  collect ipv4 ttl
  collect routing next-hop address ipv4
  collect application name
  collect routing source as
  collect routing destination as
 !
 flow exporter FLOW_EXPORTER
  destination 192.168.102.133
  transport udp 2055
  template data timeout 60
  option interface-table timeout 60
  option exporter-stats timeout 60
 !
 flow monitor FLOW_MONITOR
  exporter FLOW_EXPORTER
  record FLOW_RECORD
 !
 interface GigabitEthernet1
  ip flow monitor FLOW_MONITOR input
  ip flow monitor FLOW_MONITOR output
  end
~~~


<br>
<br>

---
&nbsp;


## Packet Filtering (L3 ACL)
~~~
!@NetOps-PH
youporn.com
pornhub.com
redtube.com
faketaxi.com
bangbros.com
bangbus.com
pinayflix.com
xhamster.com
iyottube.com
~~~

<br>

Protect your most important asset: The brain

<br>

### Create a standard ACL named FWP1
~~~
!@UTM-PH
config t
 no ip access-list standard FWP1
 ip access-list standard FWP1
  deny __.__.__.__  __.__.__.__
  deny __.__.__.__  __.__.__.__
  deny __.__.__.__  __.__.__.__
  permit any
 int gi 1
  ip access-group FWP1 in
  end
show ip access-list int g1
~~~

<br>

Remove the access-list
~~~
!@UTM-PH
config t
 int gi 1
  no ip access-group FWP1 in
  end
~~~

<br>

Alternative: use standard ACL 1-99
~~~
!@UTM-PH
config t
 no access-list 69
 access-list 69 deny 66.254.0.0 0.0.255.255
 access-list 69 deny 104.21.0.0 0.0.255.255
 access-list 69 deny 68.235.0.0 0.0.255.255
 access-list 69 deny 104.17.0.0 0.0.255.255
 access-list 69 deny 88.208.0.0 0.0.255.255
 access-list 69 deny 208.77.0.0 0.0.255.255
 access-list 69 deny 172.67.0.0 0.0.255.255
 access-list 69 permit any
 int gi 1
  ip access-group 69 in
  end
~~~

<br>

### Block schools - FWP2
~~~
!@NetOps-PH
ping www.dlsu.edu.ph 
ping www.ccp.edu.ph      
ping www.feu.edu.ph 
ping  www.ue.edu.ph 
ping  www.yourschool.edu.ph = _________
~~~

<br>

~~~
!@UTM-PH
config t
 no ip access-list standard FWP2
 ip access-list standard FWP2
  deny __.__.__.__  __.__.__.__
  deny __.__.__.__  __.__.__.__
  deny __.__.__.__  __.__.__.__
  permit any
 int gi 1
  ip access-group FWP2 in
  end
wr
show ip access-list int g1
~~~

<br>

### Block torrents
~~~
www.thepiratebay.org
www.limetorrents.com
www.freeanimeonline.com
www.torlock2.com
www.hentaisites.com
www.iptorrents.com
~~~

<br>

~~~
@UTM-PH
config t
 no ip access-list standard FWP3
 ip access-list standard FWP3
  deny __.__.__.__  __.__.__.__
  deny __.__.__.__  __.__.__.__
  deny __.__.__.__  __.__.__.__
  permit any
 int gi 1
  ip __________ FWP3 in
  end
wr
show ip access-list int g1
~~~

<br>
<br>

---
&nbsp;

### L4 Firewall Rules
`www.rivanit.com` vs `neu.edu.ph` vs 'sti.edu.ph'

Make Your UTM Router Vulnerable
~~~
!@UTM-PH
config t
 int g1
  ip add 208.8.8.100 255.255.255.0 secondary
  no shut
  exit
 ip host www.bet100.com 208.8.8.100
 ip access-list extended NAT 
  25 deny ip 208.8.8.0 0.0.0.255 208.8.8.0 0.0.0.255
  exit
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
  ip source-address 208.8.8.11 port 2000
  exit
 voice service voip
  allow-connections h323 to sip
          
  allow-connections sip to h323
  allow-connections sip to sip
  supplementary-service h450.12
 sip
   bind control source-interface g1
   bind media source-interface g1
   registrar server expires max 600 min 60
 voice register global
  mode cme
  source-address 208.8.8.11 port 5060
  max-dn 12
  max-pool 12
  authenticate register
  create profile sync syncinfo.xml
  end
~~~

<br>

Review: Create a DNS A record for www.ccna#$34T#.com
208.8.8.100 www.bet100.com

<br>

or

<br>

Modify the hosts file: c:\Windows\system32\drivers\etc\hosts

<br>

Scan the sites:
~~~
@cmd
nmap -v www.bet100.com
~~~

&nbsp;
---
&nbsp;

### Protect you GAMBLING SITES
Create an extended ACL named FWP4
Open only the following ports:
  http, https, ping, ssh, dns

Formula:
|  PROTOCOL       |  HACKER  |  VICTIM  |  PORT         |
| ---             | ---      | ---      | ---           |
| TCP/UDP/ICMP/IP |   ANY    |          |  Dest.Port eq |

<br>

__SYSLOG__
~~~
!@UTM-PH
conf t
 service timestamps log datetime
 service timestamps debug datetime
 logging 10.#$34T#.1.10
 logging trap 5
 end
wr
~~~

<br>

~~~
!@UTM-PH
conf t
 no ip access-list extended FWP4
 ip access-list extended FWP4
 permit __ Any host www.____.com eq __ log
 permit __ Any host www.____.com eq __ log
 permit __ Any host www.____.com log
 permit __ Any host www.____.com eq __ log
 permit __ Any host www.____.com eq __ log
 Int gi 3
  ip access-group FWP4 in
  end
~~~

<br>

Remove the Firewall:
~~~
@UTM-PH
config t
 int gi 3
  no ip access-group FWP4 in
end
~~~

&nbsp;
---
&nbsp;

### Learn by doing the wrong things
Create an access-list named FWP5 to open the following ports:  
www.bet100.com open only telnet, sql, domain, ftp, ping  

<br>

~~~
!@UTM-PH
conf t
 no ip access-list extended FWP5
 ip access-list extended FWP5
  permit __ Any host www.____.com eq __ log
  exit
 int gi 3
  ip access-group FWP5 in
  end
~~~

&nbsp;
---
&nbsp;

### Remember Ports
Create an access-list named FWP6 to open the following ports:  
www.bet100.com open only sccp, sip, imap, smtp, pop3, ftp, tftp, ntp

~~~
!@UTM-PH
conf t
 no ip access-list extended FWP6
 ip access-list extended FWP6
 permit __ Any host www.____.com eq __ log
 permit __ Any host www.____.com eq __ log
 permit __ Any host www.____.com eq __ log
 permit __ Any host www.____.com eq __ log
 permit __ Any host www.____.com eq __ log
 permit __ Any host www.____.com eq __ log
 permit __ Any host www.____.com eq __ log
 Int gi 3
  ip access-group FWP6 in
  end
~~~

<br>

__Make sure to remove firewall for next activity:__

<br>
<br>

---
&nbsp;

## Zone Based Firewall
~~~
!@UTM-JP
config t
 int g1
  ip add 208.8.8.200 255.255.255.0 secondary
  no shut
  exit
 ip host www.bet200.com 208.8.8.200
 ip access-list extended NAT 
  25 deny ip 208.8.8.0 0.0.0.255 208.8.8.0 0.0.0.255
  exit
  end
~~~

<br>

REMOVE THE TUNNEL
~~~
!@UTM-PH
conf t
 ip route 10.22.22.192 255.255.255.192 208.8.8.12
 ip route 10.21.21.208 255.255.255.240 208.8.8.12
 end
~~~

<br>

~~~
!@UTM-JP
conf t
 ip route 10.11.11.96 255.255.255.224 208.8.8.11
 end
~~~

<br>

Stateless vs Stateful

<br>

Establish Zero Trust
~~~
!@UTM-JP

G1: OUTSIDE
G2: INSIDE
G3: DMZ
Tun1: VPN

Policy
INBOUND:  OUT-IN      DROP
OUTBOUND: IN-OUT      INSPECT
DOMAIN:   OUT-DMZ     INSPECT (RESTRICT)
~~~

<br>

1. Define Zones and Interfaces
~~~
!@UTM-JP
conf t
 zone security OUTSIDE
  description PUBLIC_INTERFACE
 zone security INSIDE
  description LOCAL_LAN
 zone security DMZ
  description SECURE_PUB_SERVERS
 !
 int g1
  zone-member security OUTSIDE
 int g2
  zone-member security INSIDE
 int g3
  zone-member security DMZ
  end
~~~

<br>

2. Create an ACL to match traffic rules
~~~
!@FW
conf t
 ip access-list extended INSIDE-TO-OUTSIDE
  10 permit ip any any
 ip access-list extended OUTSIDE-TO-INSIDE
  10 deny ip any any
  end
~~~

<br>

3. Group traffic using Class Maps
~~~
!@FW
conf t
 class-map type inspect match-any CM-IN-TO-OUT
  match access-group name INSIDE-TO-OUTSIDE
 class-map type inspect match-any CM-OUT-TO-IN
  match access-group name OUTSIDE-TO-INSIDE
  end
~~~

<br>

4. Define Policy Maps to Act on the traffic (ex. inspect, drop, pass, log)
~~~
!@FW
conf t
 policy-map type inspect INSIDE-OUTSIDE-POLICY
  class class-default
   drop log
 policy-map type inspect OUTSIDE-INSIDE-POLICY
  class class-default
   drop log
   end
~~~

<br>

5. Define Zone-Pairs to link source and destination zone to policies in a specific direction.
~~~
!@FW
conf t
 zone-pair security INSIDE-OUTSIDE source INSIDE destination OUTSIDE
  service-policy type inspect INSIDE-OUTSIDE-POLICY
 zone-pair security OUTSIDE-INSIDE source OUTSIDE destination INSIDE
  service-policy type inspect OUTSIDE-INSIDE-POLICY
  end
~~~

<br>

### Exercise: On UTM-JP
- Allow traffic coming from UTM-PH (208.8.8.11) except Ping and Telnet
- Allow traffic to ports http and https

<br>

Verify:
~~~
!@cmd
nmap -v 208.8.8.12
~~~

<br>
<br>

---
&nbsp;

## Reverse Proxy/Port Forwarding
`www.fbi.gov` vs `www.cia.gov`

~~~
!@UTM-JP
config t
 int g1
  ip add 208.8.8.200 255.255.255.0 secondary
  no shut
  exit
 ip host www.bet200.com 208.8.8.200
 ip access-list extended NAT 
  25 deny ip 208.8.8.0 0.0.0.255 208.8.8.0 0.0.0.255
  exit
 !
 ip nat inside source static tcp 10.21.21.211 80 208.8.8.12 8080
  end
~~~

<br>

Access the Web: http://208.8.8.200:8080

<br>

### Exercise: Set Portforwarding Rules for:
- 208.8.8.12 8443 > 10.22.22.221 443
- 208.8.8.12 2222 > 10.21.21.211 22

<br>

Well-Known Ports    Registered Ports     Dynamic/Ephemeral Ports
	0-1023            1024-49151            49152-65535


<br>
<br>

---
&nbsp;

## Configure Cisco Network Services (NTP)
~~~
@cmd
ping time.google.com
~~~

<br>

~~~
@UTM-PH
config t
ntp server 216.239.35.8
end
show ntp associations
~~~

<br>

Configure Master Time Server
~~~
@UTM-PH
config t
ntp master 1
ntp source 208.8.8.11
ntp update-calendar
end
show ntp associations
~~~

<br>
<br>

---
&nbsp;

## Honeypot
~~~
!@cmd
nmap -v www.dpwh.gov.ph
~~~

<br>

### 1. Create a python file for the honeypot operation.
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

&nbsp;
---
&nbsp;

### 2. Create the directory for the log files.
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

&nbsp;
---
&nbsp;

### 3. Prevent the honeypot server from being compronised by assigning a nologin account to it.
~~~
!@NetOps
sudo useradd -r -s /sbin/nologin honeypot69 || true
sudo chown -R honeypot69:honeypot69 /var/log/tcp-6969-honeypot
~~~

&nbsp;
---
&nbsp;

### 4. Create a Systemd Service unit file
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

&nbsp;
---
&nbsp;

### 5. Then start the service
~~~
!@NetOps
sudo systemctl daemon-reload
sudo systemctl start tcp-6969-honeypot.service
sudo systemctl status tcp-6969-honeypot.service --no-pager
~~~

<br>

### 6. OPTIONAL
If binding to ports below 1024 use the following systemd setup
~~~
NoNewPrivileges=No
AmbientCapabilities=CAP_NET_BIND_SERVICE
~~~

<br>

Exercise: Set a port forwarding rule for the honeypot and modify the firewall to allow any access to the honeypot port.

<br>
<br>

---
&nbsp;

## Secure Authentication Methods & Privilege Access Management Solutions
Generate SSH Keys:

CISCO:
~~~
!@UTM-PH
conf t
 ip domain name sec.plus
 !
 crypto key generate rsa modulus 2048 label key exportable
 ip ssh version 2
 ip ssh rsa keypair-name key
 end
show ip ssh
~~~

<br>

LINUX:
~~~
!@NetOps-PH
ssh-keygen -t rsa -b 1024 -f rsa.key
fold -w 46 rsa.key.pub
~~~

<br>

__RSA__ vs __ECDSA__ vs __ED25519__

<br>

RSA (Rivest Shamir Adleman)
- Algo: Integer factorization
= Key Size: 2048-4096 bits
  
ECDSA (Elliptical Curve Digital Signature)
- Algo: Elliptic-curve (NIST P-256/384/521)
- Key Size: 256, 384, 521 bits

ED25519 (Edwards Curve over the 2^255-19 prime field)
- Algo: Elliptic-curve (Curve25519)
- Key-Size: 256 bits

&nbsp;
---
&nbsp;

### ECDSA Key Pair
LINUX:
~~~
!@NetOps-PH
ssh-keygen -t ecdsa -b 521 -f ec.key
fold -w 46 ec.key.pub
~~~

<br>

CISCO:
~~~
!@UTM-PH
conf t
 crypto key generate ec keysize 256 label key exportable
 end
~~~

&nbsp;
---
&nbsp;

### ED25519 Key Pair
LINUX:
~~~
!@NetOps-PH
ssh-keygen -t ed25519 -a 256 -f ed.key
fold -w 46 ed.key.pub
~~~

<br>

CISCO:
~~~
!@UTM-PH
conf t
 crypto key generate ed25519 keysize 256 label key exportable
 end
~~~

<br>

Exercise: Create accounts on both Cisco and Linux servers then attach the public key to accounts.
~~~
!@UTM-PH
conf t
 username admin priv 15 secret pass
 username rivanph priv 15 secret pass
 username ______  priv 15 secret pass
 end
~~~

<br>

~~~
!@NetOps-PH
adduser rivanph
passwd rivanph
~~~

<br>

CISCO:
~~~
!@UTM-PH
conf t
 ip ssh pubkey-chain
  username rivanph
   key-string
    <PASTE PUB KEY>
	exit
   end
~~~

<br>

Disable Password authentication.
~~~
!@UTM-PH
conf t
 ip ssh server algorithm authentication publickey
 no ip ssh server algorithm authentication password
 no ip ssh server algorithm authentication keyboard
 end
~~~

<br>

LINUX:
~~~
!@NetOps-PH
nano /etc/ssh/ssh_config.d
cat key.pub >> /.ssh/authorized_keys

Paste Public Keys
~~~











http://192.168.102.6

<br>

Register Devices in the JumpServer








TShoot Possible Domain Errors
~~~
!@NetOps-PH
nano /opt/jumpserver/config/config.txt
~~~

<br>

`DOMAINS=set.domain.name`

~~~
!@NetOps-PH
jmsctl.sh restart
~~~























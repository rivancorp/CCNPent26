
<!-- Your monitor number = #$34T# -->


## ⛅ Warm Up for Day 4.

### 🔧 Configure the following:
  - Switch (__CoreTAAS__ & __CoreBABA__)
  - Voice Gateway/Call Manager (__CUCM - Cisco Unified Call Manager__)
  - Router (__EDGE__)

<br>

Verify:

~~~cmd
@cmd
ping 10.#$34T#.1.10             PC Network Adapter
ping 10.#$34T#.1.2		    CoreTAAS
ping 10.#$34T#.1.4		    CoreBABA
ping 10.#$34T#.100.8		CUCM
ping 10.#$34T#.#$34T#.1		EDGE - INSIDE
ping 200.0.0.#$34T#		    EDGE - OUTSIDE

ping 200.0.0.k		        Klassmate's EDGE	       k = klassmate's Monitor Number
ping 10.k.100.8		        Klassmate's CUCM
ping 10.k.1.4		        Klassmate's CoreBABA
ping 10.k.1.2		        Klassmate's CoreTAAS
ping 10.k.1.10		        Klassmate's PC
~~~

Your Branch must be able to call other klassmates  

<br>

View your cameras:
  - http://10.#$34T#.50.6  
  - http://10.#$34T#.50.8  

<br>
<br>

---
&nbsp;

## OSI Model 
*Interpret and Explain the OSI Model*

PDU (Protocol Data Unit)

### 7. Application Layer 
"Underlying service that supports applications"  
SMTP  
Resource sharing  
Service advertisement  
Gramaphone  

<br>

### 6. Presentation Layer - File extension
Data  
encryption  
-tion  
.wav .jpg .au .exe .psd  

<br>

### 5. Session Layer - Session established
Data Stream  
stateful  
  
Commands: netstat -s -p tcp, telnet, ssh  

<br>

### 4. Transport Layer - TCP/UDP
Segments  
Three way handshake  
Sliding Window  
  
Commands: nmap -v 10.#$34T#.1.10    
  
Well-known ports 0 - 1023  
Registered ports 1024 - 49151  
Ephemeral/Dynamic ports 49152 - 65535  

<br>

### 3. Network Layer - IP addresses
Packets  
Routing protocols  
Forwarding packets  
  
Commands: show ip int br, sh ip route  

<br>

### 2. Data-link Layer - MAC Addresses
Frames  
FCS  
Preamble  
  
Commands: show vlan brief  

<br>

### 1. Physical Layer - "Things you touch"
Speed = b   
Data = B  
  
Commands: show cdp neigh  

<br>
<br>

---
&nbsp;

## Routing
*How do network devices forward IP packets? *
~~~
!@CoreTAAS
conf t
 ip routing
 int fa0/9
  no switchport
  ip add 10.#$34T#.69.69 255.255.255.248
  no shut
  end
~~~

<br>

~~~
!@CoreBABA
conf t
 ip routing
 int fa0/9
  no switchport
  ip add 10.#$34T#.69.73 255.255.255.248
  no shut
  end
~~~

<br>

Will they ping?
~~~
!@CoreTAAS
ping 10.#$34T#.69.73
~~~

&nbsp;
---
&nbsp;

### Network Engineer: Job Interview Questions for L1/L2 NOC-MSP postions.
*What is a Routing Table?*
*Interpret the components of routing table?*

A routing table is a database, aka a FIB (Forward Information Base), that is used by network devices to efficiently make L3 forwarding decisions.

Palo Alto
| Destination | Next Hop | Metric | Weight | Flags | Age | Interface   |
| ---         | ---      | ---    | ---    | ---   | --- | ---         |
| 0.0.0.0/0   | 0.0.0.0  | 1      |        | A S   |     | ethernet1/1 |

<br>

Fortinet
| Type   | Network  | Gateway IP | Interfaces  | Distance | Metric | Up Since     |
| ---    | ---      | ---        | ---         | ---      | ---    | ---          |
| Static | 0.0.0.0  | 10.0.0.1   | ethernet1/1 | 1        | 0      | 11 hours ago |

<br>

Juniper
| Destination | Type | RtRef | Next hop | Type Index | NhRef | Netif |
| ---         | ---  | ---   | ---      | ---        | ---   | ---   |
| 0.0.0.0/32  | perm | 0     |          | dscd       | 34    | 1     |

<br>

Cisco
~~~
!@CoreTAAS
show ip route
~~~

<br>

Windows
~~~
!@cmd
route print
~~~

<br>

Linux & Windows
~~~
!@terminal
netstat -rn
~~~

## Static Routing
Remove routing to learn
~~~
!@CoreBABA, CUCM, EDGE   (via Serial Connection)
conf t
 no ip routing
 ip routing
 end
~~~

<br>

Syntax:
`ip route [Destination IP]  [Network/Host Mask]  [Exit Interface / Nex-hop]`

<br>

What is a next-hop?

<br>

~~~
!@cmd
tracert 8.8.8.8

Tracing route to 8.8.8.8 over a maximum of 30 hops
  1     2 ms     1 ms     1 ms  192.168.1.1
  2     5 ms     6 ms     5 ms  100.83.0.1
  3     6 ms    11 ms     7 ms  122.2.203.6
  4    12 ms     6 ms     7 ms  210.213.130.15
  5    66 ms    62 ms    49 ms  142.250.175.196
  6     6 ms     6 ms     6 ms  142.251.251.47
  7     8 ms     5 ms     7 ms  142.250.58.243
  8     6 ms    10 ms    10 ms  8.8.8.8
~~~

<br>

Next-hop = 

&nbsp;
---
&nbsp;

### Task 01: Configure static routing on EDGE
To CoreBABA:VLAN 10
~~~
!@EDGE
conf t
 ip route 10.#$34T#.10.4 255.255.255.255 10.#$34T#.#$34T#.4
 end
~~~

<br>

To CoreBABA:VLAN 100
~~~
!@EDGE
conf t
 ip route 10.#$34T#.100.4  255.255.255.255  ___.___.___.___
 end
~~~

<br>

To CUCM
~~~
!@EDGE
conf t
 ip route 10.#$34T#.100.___  255.255.255.255 ___.___.___.___
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
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

&nbsp;
---
&nbsp;

### ANSWER

<details>
<summary>Show Answer</summary>
  
Do not forget the return traffic.  
From CUCM to EDGE
~~~
!@CUCM
conf t
 ip route 10.#$34T#.#$34T#.1  255.255.255.255 10.#$34T#.100.4
 end
~~~

</details>

&nbsp;
---
&nbsp;

### Host Route vs Network Route
### 🎯 Exercise 01: Review. Find the network of the following IP addresses:
| Network          | Host IP           |
| ---              | ---               |
|                  | 10.1.100.79 /18   |
|                  | 172.16.145.18 /20 |
|                  | 192.168.1.205 /30 | 

<br>
<br>

&nbsp;
---
&nbsp;

### ANSWER

<details>
<summary>Show Answer</summary>
	
| Network           | Host IP           |
| ---               | ---               |
| 10.1.64.0 /18     | 10.1.100.79 /18   |
| 172.16.144.0 /20  | 172.16.145.18 /20 |
| 192.168.1.204 /30 | 192.168.1.205 /30 | 
 
</details>

&nbsp;
---
&nbsp;

### Task 02: Configure a static network route on EDGE destined for all VLANs
~~~
!@EDGE
conf t
 ip route 10.#$34T#.0.0  255.255.0.0  10.#$34T#.#$34T#.4
~~~

<br>

Static route on windows
~~~
!@cmd
route add 10.0.0.0 mask 255.0.0.0 10.#$34T#.1.4 -p
route add 200.0.0.0 mask 255.255.255.0 10.#$34T#.1.4 -p
~~~

<br>
<br>

---
&nbsp;

### Exercise 01: Establish connectivity between the loopback 1 IP of EDGE and the loopback 25 IP of CoreTAAS
- EDGE loop1 (#$34T#.0.0.1/32)
- CoreTAAS loop25 (10.#$34T#.25.25/32)
~~~
!@CoreTAAS
conf t
 ip routing
 int loopback 25
  ip add 10.#$34T#.25.25 255.255.255.255
  end
~~~

<br>

~~~
!@EDGE
conf t
 ip routing
 int loopback 1
  ip add #$34T#.0.0.1 255.255.255.255
  end
~~~

<br>
<br>

Verify - Make sure both CoreTAAS & EDGE can ping each other's loopback interface.
~~~
!@CoreTAAS
ping #$34T#.0.0.1
~~~

<br>

~~~
!@EDGE
ping 10.#$34T#.25.25
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
<br>
<br>
<br>
<br>

&nbsp;
---
&nbsp;

### ANSWER

<details>
<summary>Show Answer</summary>

~~~
!@CoreTAAS
conf t
 ip route #$34T#.0.0.1 255.255.255.255 10.#$34T#.1.4
 end
~~~

<br>

~~~
!@EDGE
conf t
 ip route 10.#$34T#.25.25 255.255.255.255 10.#$34T#.#$34T#.4
 end
~~~

<br>

Every device on path must know how to reach both destinations.
~~~
!@CoreBABA
conf t
 ip route 10.#$34T#.25.25 255.255.255.255 10.#$34T#.1.2
 ip route #$34T#.0.0.1 255.255.255.255 10.#$34T#.#$34T#.1
 end
~~~

<br>

Allow EDGE & CoreTAAS to ping directly:

~~~
!@CoreTAAS
conf t
 ip route 10.#$34T#.#$34T#.1 255.255.255.255 10.#$34T#.1.4
 end
~~~

<br>

~~~
!@EDGE
conf t
 ip route 10.#$34T#.1.2 255.255.255.255 10.#$34T#.#$34T#.4
 end
~~~

</details>

<br>
<br>

---
&nbsp;

## 🔀 Dynamic Routing
*What are the different dynamic routing protocols?*

1. IGP (Interior Gateway Protocol)
  - Link-state - __OSPF__ & __ISIS__
  - Distance Vector - __EIGRP__ & __RIP__

2. EGP (Exterior Gateway Protocol)
  - __BGP__ (Border Gateway Protocol)

<br>
<br>

## 🔀 EIGRP
Steps in configuring EIGRP
1. Decide on an ASN
2. Determine Connected Networks
3. Advertise

~~~
!@CoreBABA
conf t
 int loopback 90
  ip add 10.#$34T#.90.4 255.255.255.240
  exit
 router eigrp 100
  network 10.#$34T#.90.0 0.0.0.15
  network 10.#$34T#.1.0 0.0.0.255
  network 10.#$34T#.10.0 0.0.0.255
  network 10.#$34T#.50.0 0.0.0.255
  network 10.#$34T#.100.0 0.0.0.255
  end
~~~

<br>

~~~
!@CUCM
conf t
 int loopback 90
  ip add 10.#$34T#.90.90 255.255.255.240
  exit
 router eigrp 100
  network 10.#$34T#.90.80 0.0.0.15
  network 10.#$34T#.100.0 0.0.0.255
  end
~~~

&nbsp;
---
&nbsp;

### EIGRP Database
1. __`show ip eigrp neighbors`__ : Neighbor Table
2. __`show ip eigrp interfaces`__ : Interface Table
3. __`show ip eigrp topology`__ : Topology Table

<br>
<br>

## Path Selection Process : Admin Distance & Metric Cost
1. __Longest Prefix Match (LPM)__  
2. __Administrative Distance__
3. __Metric Cost__

~~~
!@CUCM
show ip route
~~~

<br>

| Legend | Routing Protocol | Administrative Distance | Metric |
| ---    | ---              | ---                     | ---    |
| C      | Connected        |                         |        |
| S      | Static           |                         |        |
| D      | EIGRP            |                         |        |

<br>
<br>

What makes __EIGRP__ a __Distance Vector Protocol__?
~~~
!@CUCM
show ip protocols
show ip eigrp topology
~~~

<br>
<br>

### EIGRP Metric
*EIGRP Metric Formula*

<br>

1. Reported Distance (__RD__)  
2. Feasible Distance (__FD__)  
3. Successor (Primary : Lowest Cost)  
4. Feasible Successor (Backup)  

<br>

__Feasability Condition__ - An EIGRP route is a feasible successor route if the RD from the neighbor is less than the FD of the successor route.

<br>
<br>

---
&nbsp;

## 🔀 OSPF
*What device do you need to filter data entering your network? Firewall*

<br>

Which is the best firewall?
1. Palo Alto
2. Fortinet
3. Juniper
4. Cisco Firepower

<br>

What Routing protocol to use if __Multi-Vendor__? __OSPF__

&nbsp;
---
&nbsp;

### Single Area OSPF
~~~
!@CoreBABA
conf t
 router ospf 1
  router-id 10.#$34T#.#$34T#.4
  network 10.#$34T#.1.0 0.0.0.255 area 0
  network 10.#$34T#.#$34T#.0 0.0.0.255 area 0
  end
~~~

<br>

~~~
!@EDGE
conf t
 router ospf 1
  router-id #$34T#.0.0.1
  network 10.#$34T#.#$34T#.0 0.0.0.255 area 0
  network 200.0.0.0 0.0.0.255 area 0
  network #$34T#.0.0.1 0.0.0.0 area 0
  end
~~~

&nbsp;
---
&nbsp;

### OSPF Database
1. __`show ip ospf neighbors`__ : Neighbor Table
2. __`show ip ospf interface brief`__ : Interface Table
3. __`show ip ospf topology-info`__ : Topology Table

&nbsp;
---
&nbsp;

## Job Interview Question.
### What are the contents of an OSPF Hello Packet?

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
<br>
<br>
<br>
<br>
<br>

<details>
<summary>Show Answer</summary>
	
- Router ID/Source OSPF Router
- Area ID
- Subnet Mask
- Authentication Type

- Hello Timer: 10
- Dead Timer: 40
- Designated Router
- Backup Designated Router

</details>

&nbsp;
---
&nbsp;

### Link-State Routing Protocol & Multi-Area OSPF
__LSU__ - Link-State Updates  
__LSA__ - Link-State Advertisement  

<br>

- Type 1 LSA - (__O__) Router LSA : Router info & Advertisements
- Type 2 LSA - (__O__) Network LSA : via DR/BDR Network Routes
- Type 3 LSA - (__O IA__) Summary LSA
- Type 5 LSA - (__O E2__) AS-External LSA

<br>

Summary LSA
~~~
!@EDGE
conf t
 int loopback 110
  ip add 10.#$34T#.110.110 255.255.255.255
  ip ospf 1 area #$34T#
  end
clear ip ospf process
yes
~~~

&nbsp;
---
&nbsp;

### OSPF States
D: __Down__ - The initial state where no hello packets have been received from a neighbor  
I: __Init__ - A hello packet has been received from a neighbor, but that neighbor's Router ID was not included in the hello packet, indicating a lack of bidirectional communication  
T: __Two-way__ - Bidirectional communication is established, as both routers have received each other's Router IDs in hello packets. DR/BDR Election occurs.  
E: __Exstart__ - Routers determine who will be the master and who will be the slave for the upcoming database exchange. The router with the higher Router ID becomes the master and begins the exchange of Database Description (DBD) packets, synchronizing sequence numbers.  
E: __Exchange__ - Routers exchange DBD packets to describe their Link State Databases (LSDBs). They compare the contents and identify any missing link-state information  
L: __Loading__ - Routers request the missing link-state information by sending Link-State Requests (LSRs). They then receive Link-State Updates (LSUs) and acknowledge them with Link-State Acknowledgements (LSAs)  
F: __Full__ - the LSDBs are fully synchronized and identical between the adjacent routers. The routers have all the necessary information to form a complete network map and are ready to share routing data  

<br>

Attempt to view OSPF States

~~~
!@R2 & R1
clear ip ospf process
yes
show ip ospf neighbor
~~~

<br>
<br>

---
&nbsp;

## Route Redistribution
Will the ping:
~~~
!@CUCM
ping 200.0.0.#$34T#
~~~

<br>

~~~
!@CoreBABA
conf t
 router eigrp 100
  redistribute ospf 1 metric 10000 100 255 1 1500
  exit
 router ospf 1
  redistribute eigrp 100 subnets
end
~~~

&nbsp;
---
&nbsp;

## Path Selection Process : Admin Distance & Metric Cost
1. __Longest Prefix Match (LPM)__  
2. __Administrative Distance__
3. __Metric Cost__

<br>

| Legend | Routing Protocol | Administrative Distance | Metric |
| ---    | ---              | ---                     | ---    |
| C      | Connected        |                         |        |
| S      | Static           |                         |        |
| D      | EIGRP            |                         |        |
| D EX   | External EIGRP   |                         |        |
| O, OIA | OSPF             |                         |        |
| O E2   | External T5 OSPF |                         |        |

<br>
<br>

---
&nbsp;

## Tunneling and IP Security
*INET vs Private Line*

~~~
!@EDGE
conf t
 int gi 0/0/0
  no shut
  ip add 10.#$34T#.#$34T#.1 255.255.255.0
  ip nat inside
  desc INSIDE
 int gi 0/0/1
  no shut
  ip add 200.0.0.#$34T# 255.255.255.0
  ip nat outside
  desc OUTSIDE
 int loopback 0
  ip add #$34T#.0.0.1 255.255.255.255
  desc VIRTUALIP
 !
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
  exit
 !
 ip nat inside source list NAT-POLICY int g0/0/1 overload
 ip route 0.0.0.0 0.0.0.0 200.0.0.1
 !
 router ospf 1
  router-id #$34T#.0.0.1
  network 10.#$34T#.#$34T#.0 0.0.0.255 area 0
  !
  no network 200.0.0.0 0.0.0.255 area 0
  default-information originate always
  exit
  !
 ip name-server 8.8.8.8 8.8.4.4
 ip domain lookup
 ip domain lookup source-int g0/0/0
 end
ping 8.8.8.8
ping google.com
~~~

<br>

### Tunneling (Unsecure)
~~~
!@EDGE
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

## S2S VPN Lab Setup
### 1. Deploy
Devices:  
- 2x CSR1000v
- 1x NetOps
- 2x TinyCore (yvm.ova)

CSR1000v:  
  Name: UTM-PH  
  
  | NetAdapter   |        |
  | ---          | ---    |
  | NetAdapter   | NAT    |
  | NetAdapter 2 | VMNet2 |
  | NetAdapter 3 | VMNet3 |
  
<br>

CSR1000v:  
  Name: UTM-JP  
  
  | NetAdapter   |        |
  | ---          | ---    |
  | NetAdapter   | NAT    |
  | NetAdapter 2 | VMNet2 |
  | NetAdapter 3 | VMNet4 |

<br> 

NetOps:    
  Name: NetOps-PH  
  
  | NetAdapter   |                    |
  | ---          | ---                |
  | NetAdapter   | VMNet1             |
  | NetAdapter 2 | VMNet2             |
  | NetAdapter 3 | VMNet3             |
  | NetAdapter 4 | Bridge (Replicate) |

<br>

TinyCore (yvm.ova):  
  Name: BLDG-JP-1  
  
  | NetAdapter   |                    |
  | ---          | ---                |
  | NetAdapter   | VMNet4             |

<br>

TinyCore (yvm.ova):  
  Name: BLDG-JP-2  
  
  | NetAdapter   |                    |
  | ---          | ---                |
  | NetAdapter   | VMNet4             |

&nbsp;
---
&nbsp;

### 2. Bootstrap
~~~
!@UTM-PH
conf t
 hostname UTM-PH
 enable secret pass
 service password-encryption
 no logging cons
 no 
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
  ip add 11.11.11.113 255.255.255.224
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
  ip add 21.21.21.213 255.255.255.240
  ip add 22.22.22.223 255.255.255.192 secondary
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
!@BLDG-JP-1
sudo su
ifconfig eth0 21.21.21.211 netmask 255.255.255.240 up
route add default gw 21.21.21.213
ping 21.21.21.213
~~~

<br>

~~~
!@BLDG-JP-2
sudo su
ifconfig eth0 22.22.22.221 netmask 255.255.255.192 up
route add default gw 22.22.22.223
ping 22.22.22.223
~~~

<br>

&nbsp;
---
&nbsp;

### NetOps-PH Setup
> Login: root  
> Pass: C1sc0123  

1. Get the MAC Address for the Bridge connection  
`VMWare > NetOps-PH Settings > NetAdapter (2, 3, & 4) > Advance > MAC Address`

| NetAdapter   | MAC Address        | VM Interface |
| ---          | ---                | ---          |
| NetAdapter 2 | `___.___.___.___`  | ens___       |  ens192
| NetAdapter 3 | `___.___.___.___`  | ens___       |  ens224
| NetAdapter 4 | `___.___.___.___`  | ens___       |  ens256

<br>

2. Get Network-VM Mapping
~~~
!@NetOps-PH
ip -br link
~~~

<br>

3. Modify Interface IP
VMNet2:  192.168.102.6/24  
VMNet3:  11.11.11.100/27  
Bridged: 10.#$34T#.1.6/24  

<br>

~~~
!@NetOps-PH
ifconfig ens192 192.168.102.6 netmask 255.255.255.0 up
ifconfig ens224 11.11.11.100 netmask 255.255.255.224 up
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
~~~

<br>

Verify:
~~~
!@NetOps-PH
ip -4 addr

nmcli connection show
netstat -rn
~~~

&nbsp;
---
&nbsp;

### Provide Connections for VMNet3 & and Bridge Connections
VMNet3:
~~~
!@NetOps-PH
nmcli connection add \
type ethernet \
con-name VMNET3 \
ifname ens224 \
ipv4.method manual \
ipv4.addresses 11.11.11.100/27 \
autoconnect yes

nmcli connection up VMNET3
~~~

<br>

Bridged:
~~~
!@NetOps-PH
nmcli connection add \
type ethernet \
con-name BRIDGED \
ifname ens256 \
ipv4.method manual \
ipv4.addresses 10.#$34T#.1.6/24 \
autoconnect yes

nmcli connection up BRIDGED
~~~

<br>

4. Routing
~~~
!@NetOps-PH
ip route add 10.0.0.0/8 via 10.#$34T#.1.4 dev ens256
ip route add 200.0.0.0/24 via 10.#$34T#.1.4 dev ens256
ip route add 0.0.0.0/0 via 11.11.11.113 dev ens224
~~~

&nbsp;
---
&nbsp;

### Remote Access
Connect to Management Interfaces of Devices

NetOps-PH: 192.168.102.6
UTM-PH: 192.168.102.11
UTM-JP: 192.168.102.12

<br>
<br>

---
&nbsp;

### Site-to-Site VPN & Cryptography
1. Create a Key Pair for both UTM Routers

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

&nbsp;
---
&nbsp;

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

&nbsp;
---
&nbsp;

### 2. Access the GUI of Both UTM Routers

UTM-PH: https://192.168.102.11
UTM-JP: https://192.168.102.12

<br>

What traffic does the ISP see if you have a VPN?  

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
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

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
 ip route 22.22.22.192 255.255.255.192 Tunnel1
 ip route 21.21.21.208 255.255.255.240 Tunnel1
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
 ip route 11.11.11.96 255.255.255.224 Tunnel1
 end
~~~

<br>

~~~
!@UTM-PH
conf t
 ip route 22.22.22.192 255.255.255.192 208.8.8.12
 ip route 21.21.21.208 255.255.255.240 208.8.8.12
 end
~~~

<br>

~~~
!@UTM-JP
conf t
 ip route 11.11.11.96 255.255.255.224 208.8.8.11
 end
~~~

&nbsp;
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
 int g2
  ip nat inside
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
  deny ip 11.11.11.96 0.0.0.31 21.21.21.208 0.0.0.15
  deny ip 11.11.11.96 0.0.0.31 22.22.22.192 0.0.0.63
  permit ip any any
  end
~~~

<br>

~~~
!@UTM-JP
conf t
 ip access-list extended NAT
  deny ip 21.21.21.208 0.0.0.15 11.11.11.96 0.0.0.31 
  deny ip 22.22.22.192 0.0.0.63 11.11.11.96 0.0.0.31 
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
ssh -l root 11.11.11.100

> (yes/no) yes
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
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

&nbsp;
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
Create a standard ACL named FWP1
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

### Exercise: Block Torrents - Create a standard ACL named FWP2
~~~
!@UTM-PH
www.thepiratebay.org 
www.limetorrents.net
www.freeanimeonline.com
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
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

&nbsp;
---
&nbsp;

### L4 Firewall Rules
`www.rivanit.com` vs `neu.edu.ph` vs 'sti.edu.ph'

<br>

### Make Your UTM Router Vulnerable
~~~
!@UTM-PH
config t
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

### Protect you Network
Create an extended ACL named FWP3
Open only the following ports:
  http, https, ping, ssh, dns

<br>

Formula:
|  PROTOCOL    |  HACKER  |  VICTIM  |  PORT         |
| ---          | ---      | ---      | ---           |
| TCP/UDP/ICMP |   ANY    |          |  Dest.Port eq |

~~~
!@UTM-PH
conf t
 no ip access-list extended FWP3
 ip access-list extended FWP3
 permit __ Any host www.____.com eq __ log
 permit __ Any host www.____.com eq __ log
 permit __ Any host www.____.com log
 permit __ Any host www.____.com eq __ log
 permit __ Any host www.____.com eq __ log
 permit __ Any host www.____.com eq __ log
 Int gi 3
  ip access-group FWP3 in
  end
~~~

<br>

### Exercise: Learn by doing the wrong things
Create an extended ACL named FWP4
Open only the following ports:
  telnet, sql, sip, sccp, ftp, imap, pop, smtp

<br>
<br>

---
&nbsp;

### Zone Based Firewall
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
<br>

---
&nbsp;

## Reverse Proxy/Port Forwarding
`www.fbi.gov` vs `www.cia.gov`

<br>

~~~
!@UTM-JP
conf t
 ip access-list extended NAT 
  25 deny ip 208.8.8.0 0.0.0.255 208.8.8.0 0.0.0.255
  exit
 ip nat inside source static tcp 21.21.21.211 80 208.8.8.12 8080
 end
show ip nat trans
~~~

<br>

Access the Web: http://208.8.8.12:8080

<br>

### Exercise: Set Portforwarding Rules for:
- 208.8.8.12 8443 > 22.22.22.221 443
- 208.8.8.12 2222 > 21.21.21.211 22


Well-Known Ports    Registered Ports     Dynamic/Ephemeral Ports
	0-1023            1024-49151            49152-65535

<br>
<br>

---
&nbsp;

## Honeypot

~~~
!@cmd
nmap -v www.dpwh.gov.ph
~~~

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

RSA vs ECDSA vs ED25519

RSA (Rivest Shamir Adleman)
  Algo: Integer factorization
  Key Size: 2048-4096 bits
  
ECDSA (Elliptical Curve Digital Signature)
  Algo: Elliptic-curve (NIST P-256/384/521)
  Key Size: 256, 384, 521 bits

ED25519 (Edwards Curve over the 2^255-19 prime field)
  Algo: Elliptic-curve (Curve25519)
  Key-Size: 256 bits

<br>

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







<!-- Your monitor number = #$34T# -->


## ⛅ Warm Up for Day 3.

<br>

### 🔧 Physically Connect the following:
| Device   | Port       |  -  | Port   | Switch   |
| ---      | ---        | --- | ---    | ---      |
| PC       | TunayNaLAN |  -  | fa0/1  | CoreBABA |
| WLC      | PoE        |  -  | fa0/2  | CoreBABA |
| AP       | PoE        |  -  | fa0/4  | CoreBABA |
| CUCM     | fe0/0      |  -  | fa0/3  | CoreBABA |
| ePhone 1 | Network    |  -  | fa0/5  | CoreBABA |
| ePhone 2 | Network    |  -  | fa0/7  | CoreBABA |
| Cam6     | Eth        |  -  | fa0/6  | CoreBABA |
| Cam8     | Eth        |  -  | fa0/8  | CoreBABA |
| CoreTAAS | fa0/10     |  -  | fa0/10 | CoreBABA |
| CoreTAAS | fa0/11     |  -  | fa0/11 | CoreBABA |
| CoreTAAS | fa0/12     |  -  | fa0/12 | CoreBABA |


<br>
<br>

---
&nbsp;


### Exercise: Find the Network and the Wildcard of their given CIDR

1. 10.180.49.221 /9
2. 172.16.45.12  /20
3. 192.168.112.130 /27

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

---
&nbsp;


### Exercise: Configure C1 using EIGRP, OSPF, and BGP


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

---
&nbsp;


### Dynamic Routing Protocols

Internal Gateway Protocols
1. Link-state - __OSPF__ & __ISIS__  
2. Distance Vector - __EIGRP__ & __RIP__

<br>

Exterior Gateway Protocols
1. Path Vector - __BGP__


<br>
<br>

---
&nbsp;


# EIGRP (Enhanced Interior Gateway Protocol)

### Classic EIGRP
~~~
!@R4
conf t
 router eigrp 100
  no auto-summary
  eigrp router-id 10.1.4.9
  network 10.1.4.8 0.0.0.3
  network 10.1.4.4 0.0.0.3
  end
~~~

~~~
!@C1
config t
 router eigrp 100
  no auto-summary
  eigrp router-id 192.168.1.129
  network 10.1.4.4 0.0.0.3
  network 10.2.1.0 0.0.0.255
  network 10.2.2.0 0.0.0.255
  network 192.168.1.128 0.0.0.31
  end
~~~

~~~
!@C2
conf t
 router eigrp 100
  no auto-summary
  eigrp router-id 192.168.1.130
  network 10.1.4.8 0.0.0.3
  network 10.2.1.0 0.0.0.255
  network 10.2.2.0 0.0.0.255
  network 192.168.1.128 0.0.0.31
  end
~~~


&nbsp;
---
&nbsp;


### EIGRP Hello

| Network Type | Hello | Hold |
| ---          | ---   | ---  |
| Ethernet     |       |      |
| WAN          |       |      |

<br>

### 1. __Hello__

__SPAN__
~~~
!@C1
conf t
 int e3/3
  no shut
  no switchport
  exit
 monitor session 1 source interface e1/1 both
 monitor session 1 destination interface e3/3
 end
~~~


&nbsp;
---
&nbsp;


### 2. __Update__
~~~
!@R4
conf t
 router eigrp 100
  no network 10.1.4.4 0.0.0.3
  network 10.1.4.4 0.0.0.3
  end
~~~


&nbsp;
---
&nbsp;


### 3. __Query__

__RSPAN__
~~~
!@C2
conf t
 vlan 10
  remote-span
  exit
 monitor session 1 source interface e1/1 both
 monitor session 1 destination remote vlan 10
 end
~~~

~~~
!@C1
conf t
 vlan 10
  remote-span
  exit
 no monitor session 1 source interface e1/1 both
 monitor session 1 source remote vlan 10
 end
~~~


&nbsp;
---
&nbsp;


### 4. __Reply__

~~~
!@C1
conf t
 int vlan 20
  shut
  ! no shut
  end
~~~


&nbsp;
---
&nbsp;


### 5. __ACK__


<br>
<br>

---
&nbsp;


## Classic EIGRP Metric Calculation

$$
256 \times \left( \frac{10^7}{\text{Min.\ BW}} + \frac{\text{Total Delay}}{10} \right)
$$

<br>

~~~
!@R4
show ip route eigrp
~~~

<br>

From __R4__ to 10.2.1.0/24  

<br>

| Attribute | Value  |
| ---       | ---    |
| Min. BW   |        |
| Total DLY |        |


<br>
<br>

---
&nbsp;


### 🎯 Exercise 01: Compute for Classic EIGRP metric for the network 10.2.1.0/24 & 10.1.4.4/30 on R3

~~~
!@R4,R3
conf t
 router eigrp 100
  network 10.1.1.8 0.0.0.3
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

---
&nbsp;


### Named EIGRP or Multi Address-Family EIGRP
~~~
!@R3,R4,C1,C2
conf t
 no router eigrp 100
 end
~~~


<br>
<br>

---
&nbsp;


### 🎯 Exercise 02: Advertise all connected routes on C1, C2, and R4 using Named EIGRP.

~~~
!@C2,C1,R4
conf t
 router eigrp ____
  address-family ________________________
   network __.__.__.__   __.__.__.__

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


&nbsp;
---
&nbsp;


## Named EIGRP Metric Calculation
  
__Classic Metric__

$$
256 \times \left( \frac{10^7}{\text{Min.\ BW}} + \frac{\text{Total Delay}}{10} \right)
$$

  
<br>


__Wide Metric__

$$
\left(
\frac{10^7 \cdot 65536}{\text{Min.\ BW}}
+
\frac{\text{Total Delay (psec)} \cdot 65536}{10^6}
\right)
$$


<br>
<br>


*Why Wide Metric?*  

1. __GigabitEthernet__:
   - Scaled bandwidth: 10_000_000 / 1_000_000 = 10
   - Scaled delay: 10 / 10 = 1
   - Composite metric: 10 + 1 * 256 = 2816

2. __10 GigabitEthernet__:
   - Scaled bandwidth: 10_000_000 / 10_000_000 = 1
   - Scaled delay: 10 / 10 = 1
   - Composite metric: 1 + 1 * 256 = 512

3. __20 GigabitEthernet__:
   - Scaled bandwidth: 10_000_000 / 20_000_000 = 0.5 (rounded to 0)
   - Scaled delay: 10 / 10 = 1
   - Composite metric: 0 + 1 * 256 = 256

4. __40 GigabitEthernet__:
   - Scaled bandwidth: 10_000_000 / 40_000_000 = 0.25 (rounded to 0)
   - Scaled delay: 10 / 10 = 1
   - Composite metric: 0 + 1 * 256 = 256


&nbsp;
---
&nbsp;


### Calculate From R4 to 10.2.1.0/24

| Attribute | Value  |
| ---       | ---    |
| Min. BW   |        |
| Total DLY |        |

<br>

~~~
!@R4
show ip route eigrp
show ip protocols
~~~


<br>
<br>

---
&nbsp;


### 🎯 Exercise 03: Configure 10.1.49.0/24 network on A1 and advertise it via EIGRP. 
Determine the composite metric from R4 to A1's 10.1.49.0/24 network.

~~~
!@A1
conf t
 vlan 49
  name FINANCE
  exit
 int vlan 49
  no shut
  ip add 10.1.49.1 255.255.255.0
  bandwidth 10000000
  delay 1000
  exit
 !
 router eigrp CCNPLEVEL
  add ipv4 uni a 100
   network 192.168.1.128 0.0.0.31
   network 10.1.49.0 0.0.0.255
   end
~~~

<br>


* Which Delay will be used, VLAN or INT?   Answer: VLAN *


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

---
&nbsp;


### 🎯 Exercise 04: Configure 10.1.50.0/24 network on A2 and advertise it via EIGRP. 
Determine the composite metric from R4 to A2's 10.1.50.0/24 network.

~~~
!@A2
conf t
 vlan 50
  name SOC
  exit
 int vlan 50
  no shut
  ip add 10.1.50.1 255.255.255.0
  bandwidth 1000
  delay 500
  exit
 !
 router eigrp CCNPLEVEL
  add ipv4 uni a 100
   network 192.168.1.128 0.0.0.31
   network 10.1.50.0 0.0.0.255
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

---
&nbsp;


## EIGRP Path Selection
~~~
!@R4
show ip eigrp topology

P 10.2.2.0/24, 1 successors, FD is 131727360
        via 10.1.4.10 (131727360/1310720), Ethernet1/1
        via 10.1.4.6 (132382720/1966080), Ethernet1/0
~~~

<br>

| Reported Distance (Neighbor Cost) | Feasible Distance |
| ---                               | ---               |
| 1_310_720                         | 131_727_360       |
| 1_966_080                         | 132_382_720       |

<br>

1. Successor - Best Path
2. Feasible Successor - RD of Route < FD of Successor


<br>
<br>

---
&nbsp;


### 🎯 Exercise 05: Determine Successor and Feasible Successor from R4 to 10.1.51.0/24

~~~
!@A1
conf t
 vlan 51
  exit
 int vlan 51
  no shut
  ip add 10.1.51.3 255.255.255.0
  bandwidth 1000
  delay 100
  exit
 router eigrp CCNPLEVEL
  add ipv4 uni a 100
   network 10.1.51.0 0.0.0.255
   end
~~~

~~~
!@A2
conf t
 vlan 51
  exit
 int vlan 51
  no shut
  ip add 10.1.51.4 255.255.255.0
  bandwidth 10000
  delay 10
  exit
 router eigrp CCNPLEVEL
  add ipv4 uni a 100
   network 10.1.51.0 0.0.0.255
   end
~~~

~~~
!@C1
conf t
 vlan 51
  exit
 int vlan 51
  no shut
  ip add 10.1.51.1 255.255.255.0
  bandwidth 100
  delay 1000
  exit
 router eigrp CCNPLEVEL
  add ipv4 uni a 100
   network 10.1.51.0 0.0.0.255
   end
~~~

~~~
!@C2
conf t
 vlan 51
  exit
 int vlan 51
  no shut
  ip add 10.1.51.2 255.255.255.0
  bandwidth 1000
  delay 1000
  exit
 router eigrp CCNPLEVEL
  add ipv4 uni a 100
   network 10.1.51.0 0.0.0.255
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

---
&nbsp;


## EIGRP Unequal Cost Loadbalancing

~~~
!@C1
conf t
 int e2/0
  no sw
  no shut
  ip add 10.1.8.6 255.255.255.252
 !
 router eigrp CCNPLEVEL
  address-family ipv4 unicast a 100
   network 10.1.8.4 0.0.0.3
   end
~~~

<br>

~~~
!@C2
conf t
 int e2/1
  no sw
  no shut
  ip add 10.1.8.10 255.255.255.252
 !
 router eigrp CCNPLEVEL
  address-family ipv4 unicast a 100
   network 10.1.8.8 0.0.0.3
   end
~~~

<br>

~~~
!@R4
conf t
 int e2/0
  no shut
  ip add 10.1.8.5 255.255.255.252
  delay 800
 int e2/1
  no shut
  ip add 10.1.8.9 255.255.255.252
  delay 800
 !
 router eigrp CCNPLEVEL
  address-family ipv4 unicast a 100
   network 10.1.8.4 0.0.0.3
   network 10.1.8.8 0.0.0.3
   end
~~~


<br>
<br>


~~~
!@R4
conf t
 router eigrp CCNPLEVEL
  address-family ipv4 unicast a 100
   topology base
    variance 5   ! 1 default equal cost only
	end
~~~

<br>


$$
\text{FD}_{\text{candidate}} \le \text{FD}_{\text{successor}} \times \text{Variance}
$$



<br>
<br>

---
&nbsp;


## EIGRP Neighborship vs Adjacency

1. ASN Mismatch
~~~
!@R4
conf t
 router eigrp 200
  network 10.1.4.4 0.0.0.3
  end
~~~

<br>

~~~
!@C1
conf t
 router eigrp 100
  network 10.1.4.4 0.0.0.3
  end
~~~


<br>


2. Metric Weights

~~~
!@R4
conf t
 router eigrp 100
  metric weights 0 1 0 1 0 0  
  end
~~~


<br>


3. Wrong Network Advertisement
~~~
!@R4
conf t
 router eigrp 100
  network 10.1.4.0 0.0.0.31
  end
~~~

<br>

~~~
!@C1
conf t
 router eigrp 100
  network 10.1.4.0 0.0.0.15
  end
~~~


<br>
<br>

---
&nbsp;


# Open Shortest Path First (Multi-Area OSPF)

What Routing protocol to use if __Multi-Vendor__? __OSPF__

<br>
<br>

~~~
!@R3
conf t
 router ospf 1
  router-id 3.3.3.3
  network 3.3.3.3 0.0.0.0 area 0
  network 10.1.1.8 0.0.0.3 area 34
  network 10.1.1.4 0.0.0.3 area 0
  network 10.1.2.4 0.0.0.3 area 0
  network 10.1.2.8 0.0.0.3 area 352
  end
~~~

<br>

~~~
!@R4
conf t
 router ospf 1
  router-id 4.4.4.4
  network 4.4.4.4 0.0.0.0 area 34
  network 10.1.1.8 0.0.0.3 area 34
  end
~~~

<br>

~~~
!@R2
conf t
 router ospf 1
  router-id 2.2.2.2
  exit
 int range lo2,e0/2,e1/1
  ip ospf 1 area 0
  exit
 int e1/2
  ip ospf 1 area 12
  exit
 int e1/0
  ip ospf 1 area 352
  exit
 int e1/3
  ip ospf 1 area 26
  end
~~~

<br>

~~~
!@R1
conf t
 router ospf 1
  router-id 1.1.1.1
  exit
 int lo1
  ip ospf 1 area 12
  exit
 int e1/0
  ip ospf 1 area 12
  end
~~~


<br>
<br>

---
&nbsp;


### 🎯 Exercise: Configure OSPF on R5 and R6. Advertise all connected routes.

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

---
&nbsp;


## OSPF Network Types and Priorities.

~~~
!@R4,R3
conf t
 int e1/2
  ip ospf network point-to-point
  end
~~~

<br>

~~~
!@R2
conf t
 int e1/2
  ip ospf priority 255
  end
~~~

<br>

~~~
!@R1
conf t
 int e1/0
  ip ospf priority 0
  end
~~~


<br>
<br>

---
&nbsp;


## OSPF Hello Packet
__SPAN__
~~~
!@CoreBABA
conf t
 ip routing
 int vlan 1
  no shut
  ip add  10.#$34T#.1.4 255.255.255.0
 int g0/1
  no shut
  no sw
  ip add 10.#$34T#.#$34T#.4 255.255.255.0
  ip ospf network point-to-point
 !
 router ospf 1
  router-id 10.#$34T#.#$34T#.4
  network 10.#$34T#.0.0 0.0.255.255 area 0
 !
 monitor session 1 source interface g0/1
 monitor session 1 destination interface fa0/1
 end
~~~

<br>

~~~
!@EDGE
conf t
 int lo0
  ip add #$34T#.0.0.1 255.255.255.255
 int g0/0/0
  no shut
  ip add 10.#$34T#.#$34T#.1 255.255.255.0
  ip ospf network point-to-point
 int g0/0/1
  no shut
  ip add 200.0.0.#$34T# 255.255.255.0
 !
 router ospf 1
  router-id #$34T#.0.0.1
  network 200.0.0.0 0.0.0.255 area 0
  network 10.#$34T#.#$34T#.0 0.0.0.255 area 0
  network #$34T#.0.0.1  0.0.0.0  area 0
  end
~~~

<br>

~~~
!@R4
conf t
 int e3/3
  no shut
  ip add 208.8.8.69 255.255.255.0
  ip ospf 1 area 34
  exit
 end
~~~


<br>


| OSPF Hello          | Value |
| ---                 | ---   |
| Source/Router ID    |       |
| Area ID             |       |
| Subnet/Net Mask     |       |
| Auth Type           |       |
|                     |       |
| Hello Interval      |       |
| Dead Interval       |       |
| Desig Router        |       |
| Backup Desig Router |       |


<br>
<br>

---
&nbsp;


### OSPF States
D: __Down__ - The initial state where no hello packets have been received from a neighbor  

<br>

I: __Init__ - A hello packet has been received from a neighbor, but that neighbor's Router ID was not included in the hello packet, indicating a lack of bidirectional communication  

<br>

T: __Two-way__ - Bidirectional communication is established, as both routers have received each other's Router IDs in hello packets. DR/BDR Election occurs.  

<br>

E: __Exstart__ - Routers determine who will be the master and who will be the slave for the upcoming database exchange. The router with the higher Router ID becomes the master and begins the exchange of Database Description (DBD) packets, synchronizing sequence numbers.  

<br>

E: __Exchange__ - Routers exchange DBD packets to describe their Link State Databases (LSDBs). They compare the contents and identify any missing link-state information  

<br>

L: __Loading__ - Routers request the missing link-state information by sending Link-State Requests (LSRs). They then receive Link-State Updates (LSUs) and acknowledge them with Link-State Acknowledgements (LSAs)  

<br>

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


### Neighborship vs Adjacency
*Practice Doing the wrong things*

<br>

1. Mismatched Network Type
~~~
!@R2
conf t
 int e1/1
  ip ospf network point-to-point
  end
show ip ospf neighbor
clear ip ospf process
~~~

<br>

~~~
!@R1
show ip route ospf
~~~

<br>

__FIX__
~~~
!@R2
conf t
 int e1/1
  no ip ospf network point-to-point
  end
show ip ospf neighbor
clear ip ospf process
~~~


&nbsp;
---
&nbsp;


2. Mismatched Hello & Dead Timers
~~~
!@R2
conf t
 int e1/1
  ip ospf hello-interval 20
  end
show ip ospf neighbor
clear ip ospf process
~~~

<br>

__FIX__
~~~
!@R2
conf t
 int e1/1
  ip ospf hello-interval 10
  end
show ip ospf neighbor
~~~


&nbsp;
---
&nbsp;


3. Mismatch Authentication
~~~
!@R2
conf t
 int e1/1
  ip ospf authentication message-digest
  ip ospf message-digest-key 1 md5 C1sc0123
  end
show ip ospf neighbor
clear ip ospf process
~~~

<br>

__FIX__
~~~
!@R2
conf t
 int e1/1
  no ip ospf authentication message-digest
  no ip ospf message-digest-key 1 md5 C1sc0123
  end
show ip ospf neighbor
~~~


&nbsp;
---
&nbsp;


4. Mismatch Stub Flags
~~~
!@R2
conf t
 router ospf 1
  area 12 stub
  end
show ip ospf neighbor
clear ip ospf process
~~~

<br>

__FIX__
~~~
!@R2
conf t
 router ospf 1
  no area 12 stub
  end
show ip ospf neighbor
~~~


<br>
<br>

---
&nbsp;


## OSPF Metric Calculation  

$$
\text{Cost (Per eggress)} = \frac{\text{Reference Bandwidth}}{\text{Interface Bandwidth (mbps) }}
$$

<br>

~~~
!@R3
show ip ospf | inc band
~~~


<br>
<br>

---
&nbsp;


### 🎯 Exercise 06: From R3, determine OSPF Cost to reach 10.1.1.0/30 and 1.1.1.1/32

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

---
&nbsp;


### OSPF & EIGRP Redistribution

~~~
!@R4
config T
 router eigrp 100
  redistribute ospf 1
  exit
 router ospf 1
  redistribute eigrp 100 subnets
end
~~~

<br>

or

<br>

~~~
!@R4
conf t
 router eigrp CCNPLEVEL
  address-family ipv4 unicast autonomous-system 100
   topology base
    redistribute ospf 1 metric 10000 100 255 1 1500
	exit
   exit
  exit
 router ospf 1
  redistribute eigrp 100 subnets
    end
~~~


<br>
<br>

---
&nbsp;


## Path Selection Process 

### Ex. 1: Destination 10.5.20.37
~~~
S     10.5.20.0/24 
D     10.5.20.32/27
R     10.5.20.36/30          
O     10.5.20.0/25 
B     10.5.20.0/30
~~~


<br>
<br>


### Ex. 2: Destination 10.5.20.37
~~~
S     10.5.20.0/25            
D     10.5.20.0/25
R     10.5.20.0/25
O     10.5.20.0/25
~~~


<br>
<br>


### Ex. 3: Destination 10.5.20.37
~~~
O     10.5.20.0/25  [110/30]
O     10.5.20.0/25  [110/60]
O     10.5.20.0/25  [110/10] 
O     10.5.20.0/25  [110/20]
~~~


<br>


| Legend | Routing Protocol | Administrative Distance | Metric |
| ---    | ---              | ---                     | ---    |
| C      | Connected        |                         |        |
| S      | Static           |                         |        |
| D      | EIGRP            |                         |        |
| D EX   | External EIGRP   |                         |        |
| O      | OSPF             |                         |        |
| O E2   | External T5 OSPF |                         |        |

~~~
!@R4, C1, R3
show ip route
~~~


<br>
<br>

---
&nbsp;


## BGP

~~~
!@R1
conf t
 router bgp 1
  bgp log-neighbor-changes
   neighbor 208.8.8.4 remote-as 34
   neighbor 207.7.7.2 remote-as 25
   neighbor 209.9.9.3 remote-as 34
   address-family ipv4
	neighbor 208.8.8.4 activate
    neighbor 207.7.7.2 activate
    neighbor 209.9.9.3 activate
    network 207.7.7.0 mask 255.255.255.0
    network 208.8.8.0 mask 255.255.255.0
    network 209.9.9.0 mask 255.255.255.0
    end
~~~

<br>

~~~
!@I1 - CONVERGE
conf t
 router bgp 34
  bgp log-neighbor-changes
  neighbor 24.2.4.2 remote-as 25
  neighbor 45.4.5.5 remote-as 25
  neighbor 208.8.8.1 remote-as 1
  address-family ipv4
   neighbor 24.2.4.2 activate
   neighbor 45.4.5.5 activate
   neighbor 208.8.8.1 activate
   network 208.8.8.0 mask 255.255.255.0
   network 24.2.4.0 mask 255.255.255.0
   network 44.44.44.44 mask 255.255.255.255
   network 45.4.5.0 mask 255.255.255.0
   end
~~~

<br>

~~~
!@I3 - GLOBE
conf t
 router bgp 34
  bgp log-neighbor-changes
  neighbor 32.3.2.2 remote-as 25
  neighbor 35.3.5.5 remote-as 25
  neighbor 209.9.9.1 remote-as 1
  address-family ipv4
   network 209.9.9.0 mask 255.255.255.0
   network 32.3.2.0 mask 255.255.255.0
   network 33.33.33.33 mask 255.255.255.255
   network 35.3.5.0 mask 255.255.255.0
   end
~~~

<br>

~~~
!@I4 - GOOGLE
config t
 router bgp 25
  bgp log-neighbor-changes
  neighbor 25.2.5.2 remote-as 25
  neighbor 35.3.5.3 remote-as 34
  neighbor 45.4.5.4 remote-as 34
  address-family ipv4
   network 8.8.8.8 mask 255.255.255.255
   network 55.55.55.55 mask 255.255.255.255
   network 25.2.5.0 mask 255.255.255.0
   network 35.3.5.0 mask 255.255.255.0
   network 45.4.5.0 mask 255.255.255.0
end
~~~


<br>
<br>

---
&nbsp;


### 🎯 Exercise 07: Configure BGP on I2(PLDT). Set the ASN number to 25

~~~
!@I2 - PLDT
config t
 router bgp 25
 bgp log-neighbor-changes
 neighbor 25.2.5.5 remote-as 25
 neighbor 24.2.4.4 remote-as 34
 neighbor 32.3.2.3 remote-as 34
 neighbor 207.7.7.1 remote-as 1
 address-family ipv4
  network 25.2.5.0 mask 255.255.255.0
  network 24.2.4.0 mask 255.255.255.0
  network 32.3.2.0 mask 255.255.255.0
  network 207.7.7.0 mask 255.255.255.0
  network 22.22.22.22 mask 255.255.255.255
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

---
&nbsp;


### BGP  ~~Hello~~ Messages  

~~~
!@C1
conf t
 int e3/3
  no shut
  no sw
  exit
 int e1/0
  no sw
  no shut
  ip add 100.0.0.1 255.255.255.0
  exit
 no router bgp 65531
 router bgp 65531
  bgp log-neighbor-changes
  neighbor 100.0.0.2 remote-as 65532
  address-family ipv4
   neighbor 100.0.0.2 activate
   network 100.0.0.0 mask 255.255.255.0
   !
 monitor session 1 source int e1/0 both
 monitor session 1 dest int e3/3
 end
~~~

<br>

~~~
!@S1
conf t
 int lo1
  ip add 100.100.100.100 255.255.255.255
  exit
 int e1/0
  no shut
  ip add 100.0.0.2 255.255.255.0
  exit
 no router bgp 65532
 router bgp 65532
  bgp log-neighbor-changes
  neighbor 100.0.0.1 remote-as 65531
  address-family ipv4
   neighbor 100.0.0.1 activate
   network 100.0.0.0 mask 255.255.255.0
   network 100.100.100.100 mask 255.255.255.255
   end
~~~


&nbsp;
---
&nbsp;


### 1. Notification
~~~
!@S1
conf t
 router bgp 65532
  no neighbor 100.0.0.1 remote-as 65531
  end
~~~


&nbsp;
---
&nbsp;


### 2. Open Message
~~~
!@S1
conf t
 router bgp 65532
  neighbor 100.0.0.1 remote-as 65531
  end
~~~


&nbsp;
---
&nbsp;


### 3. __Keepalive__


&nbsp;
---
&nbsp;


### 4. __Update__

~~~
!@S1
conf t
 int lo1
  ip add 11.11.11.11 255.255.255.255
  exit
 router bgp 65532
  address-family ipv4
   network 11.11.11.11 mask 255.255.255.255
   end
~~~


<br>
<br>

__Remove BGP__
~~~
!@C1
conf t
 no router bgp 65531
 int e1/0
  switchport
  end
~~~

<br>

~~~
!@S1
conf t
 no router bgp 65532
 no int lo1
 int e1/0
  no ip add
  end
~~~


<br>
<br>

---
&nbsp;


### IBGP vs EBGP

1. __Next-Hop Route Advertisements__

~~~
!@R1
conf t
 int lo0
  ip add 100.1.1.1 255.255.255.255
 router bgp 1
  address-family ipv4
   network 100.1.1.1 mask 255.255.255.255
   end
~~~

<br>

~~~
!@I2
conf t
 router bgp 25
  address-family ipv4
   no network 207.7.7.0 mask 255.255.255.0
   end
~~~

<br>

`Next-Hop-Self`
Forces the router to replace the next-hop IP of advertised routes with its own IP address.

~~~
!@I2 - PLDT
conf t
 router bgp 25
  address-family ipv4
   neighbor 25.2.5.5 next-hop-self
  end
~~~

<br>

~~~
!@Google
clear ip bgp * soft
~~~


&nbsp;
---
&nbsp;


2. __Drop BGP Route Advertisement from the same AS_PATH__

__ALLOWAS-IN__
~~~
!@I3 - GLOBE
conf t
 router bgp 34
  address-family ipv4
   neighbor 32.3.2.2 allowas-in
  end
~~~

~~~
!@I1 - CONVERGE
conf t
 router bgp 34
  address-family ipv4
   neighbor 24.2.4.2 allowas-in
  end
~~~


<br>


__AS-OVERRIDE__
~~~
!@I2
conf t
 router bgp 25
  address-family ipv4
   neighbor 32.3.2.3 as-override
   neighbor 24.2.4.4 as-override
  end
~~~


<br>
<br>

---
&nbsp;


### Candidate Default Route Propagation
~~~
!@R1
conf t
 ip route 0.0.0.0  0.0.0.0  207.7.7.2  7
 ip route 0.0.0.0  0.0.0.0  208.8.8.4  8
 ip route 0.0.0.0  0.0.0.0  209.9.9.3  9
 router ospf 1
  default-information originate
  end
~~~


<br>
<br>

---
&nbsp;


## NAT & PAT

~~~
!@R1
conf t
 access-list 1 permit 10.0.0.0 0.255.255.255
 access-list 1 permit 172.16.0.0 0.15.255.255
 access-list 1 permit 192.168.0.0 0.0.255.255
 int range e1/1-3
  ip nat outside
  exit
 int e1/0
  ip nat inside
  exit
 !
 ip nat inside source list 1 interface ethernet 1/1 overload
 end
~~~

<br>

__Route Map__
~~~
!@R1
conf t
 access-list 1 permit 10.0.0.0 0.255.255.255
 access-list 1 permit 172.16.0.0 0.15.255.255
 access-list 1 permit 192.168.0.0 0.0.255.255
 int range e1/1-3
  ip nat outside
  exit
 int e1/0
  ip nat inside
  exit
 !
 route-map PBR_CONVERGE permit 10
  match ip address 1
  match interface e1/1
 route-map PBR_PLDT permit 10
  match ip address 1
  match interface e1/2
 route-map PBR_GLOBE permit 10
  match ip address 1
  match interface e1/3
 !
 ip nat pool POOL7 207.7.7.10 207.7.7.254 netmask 255.255.255.0  
 ip nat pool POOL8 208.8.8.10 208.8.8.254 netmask 255.255.255.0
 ip nat pool POOL9 209.9.9.10 209.9.9.254 netmask 255.255.255.0
 !
 ip nat inside source route-map PBR_CONVERGE pool POOL8 overload
 ip nat inside source route-map PBR_PLDT pool POOL7 overload
 ip nat inside source route-map PBR_GLOBE pool POOL9 overload
 end
~~~


<br>
<br>

---
&nbsp;


## BGP ATTRIBUTES 

The BGP path attributes are the characteristics for all BGP advertised routes. Additionally to the necessary informations to perform basic routing , the path attributes allow BGP to set routing policies and advertise them. Each attribute is a member of one of the following categories:

1. __Well-known mandatory__ – This attribute has to be known and implemented by all BGP implementations

2. __Well-known discretionary__ – This attribute has to be known by all BGP implementations, but does not have to be sent in an update message.

3. __Optional transitive__ – This attribute does not have to be known by all BGP implementations but the BGP prozess has to accept the path within the attribute is found and forward it to other BGP peers, even though the BGP process doesnt know the attribute.

4. __Optional non-transitive__ – An update with an attribute from this category can be ignored. The router does not have to advertise this route nor the path to its peers.


<br>
<br>

~~~
!@R1
traceroute 8.8.8.8
show bgp
~~~


<br>
<br>


UPDATE contains the following:
- Weight          
- LocPrf          
- Self Originated
- AS Path         
- Origin via      
- Metric          
- Ex/Internal    
- Router ID       
- Neighbor IP     

<br>
<br>

1. Highest Weight - Cisco-specific, locally significant only
2. Highest Local Preference - Preferred exit point within the local AS
3. Locally Originated Route - Network statement, redistribution, or aggregation created by the local router
4. Shortest AS Path - Fewest number of Autonomous Systems traversed
5. Lowest Origin Code - IGP < EGP < Incomplete
6. Lowest MED (Multi-Exit Discriminator) - Preferred entry point into a neighboring AS
7. eBGP over iBGP - External BGP paths are preferred over Internal BGP paths
8. Lowest IGP Metric to Next-Hop - Closest interior gateway protocol cost to reach the next-hop
9. Oldest Path - For eBGP routes, prefer the path that has been known the longest
10. Lowest Router ID - RID of the BGP router that advertised the route
11. (Optional) Shortest Cluster List Length - For Route Reflectors, if present
12. Lowest Neighbor IP Address - The IP address of the peer the route was received from


<br>
<br>

---
&nbsp;


## WEIGHT - Locally assigned
### 🎯 Exercise 08: Make R1 Prefer 208.8.8.4 as the next-hop
~~~
!@R1
conf t
 router bgp 1
  neighbor 208.8.8.4 weight 200
  end
clear ip bgp * soft
show bgp 8.8.8.8
sh bgp
~~~

<br>

~~~
!@R1
conf t
 router bgp 1
  no neighbor 208.8.8.4 weight 200
  end
clear ip bgp * soft
show bgp 8.8.8.8
sh bgp
~~~


&nbsp;
---
&nbsp;


### MANIPULATE BGP METRICS : ROUTE MAPS

__A.__ - Access List
__M.__ - Match
__S.__ - Set


~~~
!@GoogleDNS
config t
 interface Loopback1
  ip address 51.51.51.51 255.255.255.255
 interface Loopback2
  ip address 52.52.52.52 255.255.255.255
 interface Loopback3
  ip address 53.53.53.53 255.255.255.255
 interface Loopback4
  ip address 54.54.54.54 255.255.255.255
 router bgp 25
  network 51.51.51.51 mask 255.255.255.255
  network 52.52.52.52 mask 255.255.255.255
  network 53.53.53.53 mask 255.255.255.255
  network 54.54.54.54 mask 255.255.255.255
  end
~~~


<br>
<br>

---
&nbsp;


### 🎯 Exercise 09: Manipulate BGP Paths using Weight Attribute via Route Maps

- If we want to go to 51.51.51.51 we use ISP1:CONVERGE(208.8.8.4)
- If we want to go to 52.52.52.52 we use ISP2:PLDT(207.7.7.2)
- If we want to go to 53.53.53.53 we use ISP3:GLOBE(209.9.9.3)


<br>
<br>

---
&nbsp;


### 🎯 Exercise 10: Manipulate BGP Paths using Weight Attribute via Route Maps

- If we want to go to 54.54.54.54 we use ISP1:CONVERGE(208.8.8.4)
- If we want to go to 55.55.55.55 we use ISP3:GLOBE(209.9.9.3)


~~~
!@R1
config t
 access-list
 !
 route-map ISPfor
  match 
  set 
 !
 router bgp 1
  neighbor 208.8.8.4
  neighbor 209.9.9.3
  end
clear ip bgp * soft
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

---
&nbsp;


## LOCAL PREFERENCE 
- Keyword: Local
- Req: Must have same AS number
- NOTE: Local Pref >= 100: IDAAN MO SA AKIN
- NOTE: Local Pref < 100: WAG MO IDAAN SA AKIN
- Keyword sa Local Pref: AS Number


<br>
<br>

---
&nbsp;


### 🎯 Exercise 11: Manipulate Local Pref for AS 25
- 207.7.7.1 local pref of 99
- 208.8.8.1 local pref of 108
- 209.9.9.1 local pref of 109

<br>

~~~
!@I2
config t
 access-list 7 permit 207.7.7.0  0.0.0.255
 access-list 8 permit 208.8.8.0  0.0.0.255
 access-list 9 permit 209.9.9.0  0.0.0.255
 !
 route-map LP1 permit 10
  match ip address 7
  set local-pref 99
 route-map LP1 permit 20
  match ip address 8
  set local-pref 200
 route-map LP1 Permit 30
  match ip address 9
  set local-pref 200
 route-map LP1 Permit 40
  !
 router bgp 25
  neighbor 25.2.5.5 route-map LP1 out
  end
clear ip bgp * soft
~~~


<br>
<br>

---
&nbsp;


### 🎯 Exercise 12: Manipulate Local Pref for AS 25
- 54.54.54.54 local pref of 545
- 55.55.55.55 local pref of 555
- 8.8.8.8     local pref of 888  : BLOCK



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

---
&nbsp;


## AS-PATH

~~~
!@ISP1 - Converge
conf t
 route-map ASPATH1 permit 10
  set as-path prepend 34 34 34 34 34
  exit
 router bgp 34
  neighbor 208.8.8.1 route-map ASPATH1 OUT
  end
clear ip bgp * soft
~~~

<br>

~~~
!@ISP2 - PLDT
config t
 route-map ASPATH2 permit 10
  set as-path prepend 25 25 25 25 25
 !
 router bgp 25
  neighbor 207.7.7.1 route-map ASPATH2 OUT
  end
clear ip bgp * soft
~~~


<br>
<br>


Back to normal
~~~
!@ISP1 - Converge
conf t
 router bgp 34
  no neighbor 208.8.8.1 route-map ASPATH1 OUT
  end
clear ip bgp * soft
~~~

<br>

~~~
!@ISP2 - PLDT
config t
 router bgp 25
  no neighbor 207.7.7.1 route-map ASPATH2 OUT
  end
clear ip bgp * soft
~~~


<br>
<br>

---
&nbsp;


## MED - Multi Exit Discriminator
Local Pref - Influences same AS  
MED        - Influence different AS/ ebgp  

Specify an entrypoint to the AS


<br>
<br>

---
&nbsp;


## Network Optimization

### Passive Interface
~~~
!@A1
conf t
 router eigrp CCNPLEVEL
  address-family ipv4 unicast auto 100
   af-interface e0/0
    passive-interface
	end
~~~

<br>

~~~
!@A2,C1,C2
conf t
 router eigrp CCNPLEVEL
  address-family ipv4 unicast auto 100
   af-interface e1/0
    passive-interface
	end
~~~

<br>

~~~
!@R1
conf t
 router ospf 1
  passive-interface lo1
  end
~~~

<br>

~~~
!@R2
conf t
 router ospf 1
  passive-interface lo2
  end
~~~

<br>

~~~
!@R3
conf t
 router ospf 1
  passive-interface lo3
  end
~~~

<br>

~~~
!@R4
conf t
 router ospf 1
  passive-interface lo4
  end
~~~


<br>
<br>

---
&nbsp;


### Route Summarization

Summarize Routes:
1. OCTET
1. RANGE : Determine the lowest and highest decimal value
2. iTANCHA : choose an i that absorbs lowest to highest


<br>
<br>

---
&nbsp;


### 🎯 Exercise: Summarize the ff:
- 10.1.1.0/30
- 10.1.1.4/30
- 10.1.1.8/30
10.1.1.0 /28



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

---
&nbsp;


### 🎯 Exercise: Summarize the ff:
- 10.1.20.8/29
- 10.1.20.16/28
- 10.1.20.32/30
- 10.1.20.64/26



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

---
&nbsp;


### 🎯 Exercise: Summarize the ff:
- 10.144.0.0/13
- 10.157.0.0/16
- 10.171.128.0/17
- 10.183.64.0/18
- 10.190.240.0/20



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

---
&nbsp;


~~~
!@R6
conf t
 int lo8
  ip add 10.10.8.1 255.255.255.0
  ip ospf 1 area 26
 int lo9
  ip add 10.10.9.1 255.255.255.0
  ip ospf 1 area 26
 int lo10
  ip add 10.10.10.1 255.255.255.0
  ip ospf 1 area 26
 int lo11
  ip add 10.10.11.1 255.255.255.0
  ip ospf 1 area 26
 int lo33
  ip add 10.10.33.1 255.255.255.128
  ip ospf 1 area 26
 int lo34
  ip add 10.10.33.129 255.255.255.128
  ip ospf 1 area 26
 int lo35
  ip add 10.10.34.1 255.255.255.128
  ip ospf 1 area 26
 int lo36
  ip add 10.10.34.129 255.255.255.128
  ip ospf 1 area 26
  exit
 router ospf 1
  passive-interface lo8
  passive-interface lo9
  passive-interface lo10
  passive-interface lo11
  passive-interface lo33
  passive-interface lo34
  passive-interface lo35
  passive-interface lo36
  end
~~~


<br>
<br>


### 🎯 Exercise: Summarize the ff:
- 10.10.8.0 /24
- 10.10.9.0 /24
- 10.10.10.0 /24
- 10.10.11.0 /24


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

---
&nbsp;


### 🎯 Excercise 13: Summarize routes from loopback 33 - loopback 36
- 10.10.33.0   /25
- 10.10.33.128 /25
- 10.10.34.0   /25
- 10.10.34.128 /25



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

---
&nbsp;


### Route Filtering

Prefix List : `ip prefix-list [NAME] seq [10] [permit/deny] [x.x.x.x/x] [ge/le] [expected /mask]`


<br>
<br>

---
&nbsp;


## Advance OSPF Concepts

### OSPF External Type 1 and Type 2

1. __E T1__ - The total metric includes the external route's cost plus the internal OSPF cost to reach the ASBR.  
Total Cost = External Cost + OSPF Internal Cost  

<br>

2. __E T2__ The metric is primarily the external cost. The internal OSPF cost to reach the ASBR is not added to the advertised external metric.  
Total Metric = External Cost  


<br>

__Manually Set a Metric__
~~~
!@R4
conf t
 router ospf 1
  no redistribute eigrp 100
  redistribute eigrp 100 subnets metric 40000
  end
~~~

<br>

__Force T1 Routes__
~~~
!@R4
conf t
 router ospf 1
  no redistribute eigrp 100
  redistribute eigrp 100 subnets metric-type 1 metric 40000
  end
~~~


<br>
<br>

---
&nbsp;


### Link-State Routing Protocol
*What makes OSPF Link-State?* 

It builds a topology of the network.

<br>

~~~
!@R1
show ip ospf rib
show ip ospf route
~~~


<br>

__LSU__ - Link-State Updates  
__LSA__ - Link-State Advertisement  

<br>

~~~
ospf.lsa == 1  
ospf.lsa == 2  
ospf.lsa == 3  
~~~

<br>

- Type 1 LSA - (__O__) Router LSA : Router info
- Type 2 LSA - (__O__) Network LSA : via DR/BDR Network Routes
- Type 3 LSA - (__O IA__) Summary LSA
- Type 4 LSA - (__O E2__) Summary ASBR LSA
- Type 5 LSA - (__O E1 / E2__) AS-External LSA
- Type 6 LSA - OSPF Group Membership LSA
- Type 7 LSA - (__O N1 / N2__) NSSA LSA
- Type 8 LSA - OSPFv2 External Attributes & Link Local LSA OSPFv3

<br>

~~~
!@EDGE
conf t
 int lo#$34T#
  ip add #$34T#.#$34T#.#$34T#.#$34T# 255.255.255.255
  ip ospf 1 area #$34T#
  end
clear ip ospf process
yes
~~~

~~~
!@R3
show ip ospf database
show ip ospf database network
~~~


<br>
<br>

---
&nbsp;


### 1. OSPF LSA & Area Types (LSU > LSA)
ospf.lsa.type == 1  ROUTER  
ospf.lsa.type == 2  NETWORK  
ospf.lsa.type == 3  SUMMARY  
ospf.lsa.type == 4  ASBR         (How to Reach T5)  
ospf.lsa.type == 5  AS EXTERNAL  (Advert ROUTE)  
ospf.lsa.type == 7  ASBR (ASBR Converts T5 LSA to T7 only contained within NSSA)

<br>


| Area Type     | LSA       |
| ---           | ---       |
| Normal        | 1,2,3,4,5 |
| Stub          | 1,2,3     |
| Totally Stub  | 1,2       | Inject OSPF Default Route |
| Not So Stubby | 1,2,3,7   |
| Totally NSSA  | 1,2,5,7   |


<br>
<br>


### 2. STUBBY AREA & TOTALLY STUBBY AREA
~~~
!@R3,R2,R5
conf t
 router ospf 1
  area 352 stub
  end
~~~


<br>
<br>


__TOTALLY STUBBY AREA__
~~~
!@R3,R2
conf t
 router ospf 1
  area 352 stub no-summary
  end
~~~


&nbsp;
---
&nbsp;


### 3. NSSA & TOTALLY NSSA
~~~
!@R2,R1
conf t
 router ospf 1
  area 12 nssa
  end
show ip ospf route
~~~

<br>

__Make R1 Inject Default Route (T7 LSA)__
~~~
!@R1
conf t
 router ospf 1
  area 12 nssa default-information-originate
  end
~~~


<br>

~~~
!@R3,R4
conf t
 router ospf 1
  area 34 nssa
  end
~~~

<br>

__AREA 34 MUST BE A TOTALLY NSSA  ||  R4 T7 LSA > R3 T5 LSA > OSPF__
~~~
!@R3
conf t
 router ospf 1
  area 34 nssa no-summary
  end
~~~


<br>

__R1 Cannot Belong to NSSA__
~~~
!@R1
ping 10.2.1.1
~~~

<br>

~~~
!@R2,R1
conf t
 router ospf 1
  no area 12 nssa
  no area 12 nssa default-information-originate
  end
~~~


&nbsp;
---
&nbsp;


### 4. EIGRP Stub
~~~
!@A1,A2
conf t
 router eigrp CCNPLEVEL
  address-family ipv4 unicast auto 100
   eigrp stub connected summary
   end
~~~


&nbsp;
---
&nbsp;


### 5. EIGRP to OSPF Route Map

~~~
!@R4
conf t
 ip prefix-list DEFAULT-ONLY seq 5 permit 0.0.0.0/0
 !
 route-map OSPF-TO-EIGRP permit 10
  match ip address prefix-list DEFAULT-ONLY
 !
 router eigrp CCNPLEVEL
  address-family ipv4 unicast auto 100
   topology base
    redistribute ospf 1 metric 10000 100 255 1 1500 route-map OSPF-TO-EIGRP
    end
~~~


<br>
<br>

---
&nbsp;


### OSPF & EIGRP Authentication
__EIGRP/w KeyChain (CLASSIC EIGRP)__
~~~
!@C1,C2
conf t
 key chain EIGRP
  key 1
   key-string C1sc0123
 int e1/1
  ip authentication key-chain eigrp 100 EIGRP
  ip authentication mode eigrp 100 md5
  end
show ip ospf neighbor
~~~

<br>

~~~
!@R4
conf t
 key chain EIGRP
  key 1
   key-string C1sc0123
 int range e1/0-1
  ip authentication key-chain eigrp 100 EIGRP
  ip authentication mode eigrp 100 md5
  end
show ip ospf neighbor
~~~

<br>

__(NAMED EIGRP ver)__
~~~
!@C1,C2
conf t
 key chain EIGRP
  key 1
   key-string C1sc0123
 router eigrp CCNPLEVEL
  address-family ipv4 unicast autonomous-system 100
   af-interface Ethernet1/1
    authentication mode md5
    authentication key-chain EIGRP
    end
~~~

<br>

~~~
!@R4
conf t
 key chain EIGRP
  key 1
   key-string C1sc0123
 router eigrp CCNPLEVEL
  address-family ipv4 unicast autonomous-system 100
   af-interface Ethernet1/1
    authentication mode md5
    authentication key-chain EIGRP
   af-interface Ethernet1/0
    authentication mode md5
    authentication key-chain EIGRP
    end
~~~


<br>
<br>


__OSPF/w Hash__
~~~
!@R2,R3
conf t
 int e1/0
  ip ospf message-digest-key 1 md5 C1sc0123
  ip ospf authentication message-digest
  end
~~~

~~~
!@R5
conf t
 int range e1/2-3
  ip ospf message-digest-key 1 md5 C1sc0123
  ip ospf authentication message-digest
  end
~~~


<br>
<br>

---
&nbsp;


# IPv6 Routing

## IPv6 BGP

~~~
!@ISP4 - GOOGLE
conf t
 router bgp 25
  neigh b:3:4:b::3 remote-as 34
  neigh b:2:4:b::2 remote-as 25
  neigh b:1:4:b::4 remote-as 34
  address-family ipv6
   neigh b:3:4:b::3 activate
   neigh b:2:4:b::2 activate
   neigh b:1:4:b::4 activate
   network b55::1/128
   network 8:8:8:8::8888/128
   network 2001:4860:4860::8844/128
   network b:3:4:b::/64
   network b:2:4:b::/64
   network b:1:4:b::/64
   end
~~~

<br>

~~~
!@ISP3 - GLOBE
conf t
 router bgp 34
  neigh b:3:4:b::5 remote-as 25
  neigh b:1:2:b::2 remote-as 25
  neigh b:1:33:b::1 remote-as 1
  address-family ipv6
   neigh b:3:4:b::5 activate
   neigh b:1:2:b::2 activate
   neigh b:1:33:b::1 activate
   network b:3:4:b::/64
   network b:1:2:b::/64
   network b:1:33:b::/64
   network b33::1/128
   end
~~~

<br>

~~~				
!@ISP2 - PLDT
conf t
router bgp 25
 neigh b:2:1:b::4 remote-as 34
 neigh b:2:4:b::5 remote-as 25
 neigh b:1:2:b::3 remote-as 34
 neigh b:1:22:b::1 remote-as 1
 address-family ipv6
  neigh b:2:4:b::5 activate
  neigh b:2:1:b::4 activate
  neigh b:1:2:b::3 activate
  neigh b:1:22:b::1 activate
  network b:1:2:b::/64
  network b:2:1:b::/64
  network b:1:22:b::/64
  network b22::1/128
  end
~~~

<br>

~~~
!@ISP1 - Converge
conf t
 router bgp 34
  neigh b:1:4:b::5 remote-as 25
  neigh b:2:1:b::2 remote-as 25
  neigh b:1:11:b::1 remote-as 1
  address-family ipv6
   neigh b:1:4:b::5 activate
   neigh b:2:1:b::2 activate
   neigh b:1:11:b::1 activate
   network b:1:4:b::/64
   network b:2:1:b::/64
   network b:1:11:b::/64
   network b44::1/128
   end
~~~

<br>

~~~
!@R1
conf t
 router bgp 1
  neigh b:1:33:b::3 remote-as 34
  neigh b:1:22:b::2 remote-as 25
  neigh b:1:11:b::4 remote-as 34
  address-family ipv6
   neigh b:1:33:b::3 activate
   neigh b:1:22:b::2 activate
   neigh b:1:11:b::4 activate
   network FEC0:1::/122
   network b:1:33:b::/64
   network b:1:22:b::/64
   network b:1:11:b::/64
   end
~~~


<br>
<br>


__AS-OVERRIDE/ALLOWAS-IN__


<br>
<br>

---
&nbsp;


## Tunnel v6 to v4

~~~
!@R3
conf t
 interface Tunnel34
  no ip address
  ipv6 address 2026::34:1/122
  tunnel source lo3
  tunnel destination 4.4.4.4
  tunnel mode ipv6ip
  end
~~~

<br>

~~~
!@R4
conf t
 interface Tunnel34
  no ip address
  ipv6 address 2026::34:2/122
  tunnel source lo4
  tunnel destination 3.3.3.3
  tunnel mode ipv6ip
 end
~~~


&nbsp;
---
&nbsp;


## OSPFv3

~~~
!@R1
conf t
 ipv6 router ospf 6
  router-id 1.1.1.1
 int lo1
  ipv6 ospf 6 area 12
 int e1/0
  ipv6 ospf 6 area 12
  end
~~~

<br>

~~~
!@R2
conf t
 ipv6 router ospf 6
  router-id 2.2.2.2
 int lo2
  ipv6 ospf 6 area 0
 int e1/1
  ipv6 ospf 6 area 0
 int e1/2
  ipv6 ospf 6 area 12
  end
~~~

<br>

~~~
!@R3
conf t
 ipv6 router ospf 6
  router-id 3.3.3.3
 int lo3
  ipv6 ospf 6 area 0
 interface Tunnel34
  ipv6 ospf 6 area 34
 int e1/1
  ipv6 ospf 6 area 0
  end
~~~

<br>

~~~
!@R4
conf t
 ipv6 router ospf 6
  router-id 4.4.4.4
 int lo4
  ipv6 ospf 6 area 34
 interface Tunnel34
  ipv6 ospf 6 area 34
  end
~~~


<br>
<br>

---
&nbsp;


### OSPFv6 & BGP Redistribution
~~~
!@R1
config t
 ipv6 router ospf 6
  default-information originate always
  redistribute bgp 1 metric 69
 router bgp 1
  address-family ipv6 unicast
   redistribute ospf 6 match internal external 1 external 2 include-connected
   end
~~~


<br>
<br>

---
&nbsp;


## EIGRP IPv6

~~~
!@R4
conf t
 ipv6 router eigrp 10
  eigrp router-id 4.4.4.4
  no shut
 int e1/0
  ipv6 eigrp 10
 int e1/1
  ipv6 eigrp 10
  end
~~~

<br>

~~~
!@C1
conf t
 ipv6 router eigrp 10
  eigrp router-id 10.2.1.1
  no shut
 int lo1
  ipv6 eigrp 10
 int e1/1
  ipv6 eigrp 10
 int vlan 10
  ipv6 eigrp 10
 int vlan 20
  ipv6 eigrp 10
 int vlan 100
  ipv6 eigrp 10
  end
~~~

<br>

~~~	
!@C2
conf t
 ipv6 router eigrp 10
  eigrp router-id 10.2.1.2
  no shut
 int lo1
  ipv6 eigrp 10
 int e1/1
  ipv6 eigrp 10
 int vlan 10
  ipv6 eigrp 10
 int vlan 20
  ipv6 eigrp 10
 int vlan 200
  ipv6 eigrp 10
  end
~~~

<br>


## EIGRP IPv6 & OSPFv3 Redistribution

~~~
!@R4
conf t
 ipv6 router ospf 6
  redistribute eigrp 10 include-connected 
 ipv6 router eigrp 10
  redistribute ospf 6 metric 10000 100 255 1 1500 include-connected
  end
~~~


<br>
<br>

---
&nbsp;


## IPv6 Address Management
- DHCPv4 & DHCPv6
- Autoconfig
- EUI-64

<br>

~~~
!@A1
conf t
 int e0/0
  switchport mode access
  switchport access vlan 10
  end
~~~

<br>

~~~
!@A2
conf t
 int e1/0
  switchport mode access
  switchport access vlan 10
  end
~~~

<br>

~~~
!@C1
conf t
 vlan 10
  no remote-span
 no monitor session 1
 !
 ip dhcp excluded-address 10.2.1.1 10.2.1.100
 ip dhcp pool CCNPv4
  network 10.2.1.0 255.255.255.0
  default-router 10.2.1.1
  domain-name CCNPv4.COM
  dns-server 10.2.1.1
 !
 ipv6 unicast-routing
 !
 ipv6 dhcp pool CCNPv6
  address prefix 10:2:1:12::0/64
  domain-name CCNPv6.COM
  exit
 !
 int vlan 10
  no shut
  ipv6 address 10:2:1:12::1/64
  ipv6 dhcp server CCNPv6
  ipv6 nd managed-config-flag
  ipv6 nd other-config-flag
  end
sh ipv6 dhcp pool 
~~~

<br>

~~~
!@P1
conf t
 ipv6 unicast-routing
 int e0/0
  ipv6 add 10:2:1:12::B00B/64
  no shut
end


conf t
 no ipv6 unicast-routing
 int e0/0
  no shut
  no ipv6 add
  ipv6 enable
  ipv6 add dhcp
  ip add dhcp
  end
~~~

<br>

~~~
!@P2
config t
 ipv6 unicast-routing
 ipv6 route ::/0 10:2:1:12::2
 int e1/0
  ip add dhcp
  ipv6 address 10:2:1:12::/64 eui-64
  no shut
  end
~~~

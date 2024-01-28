# Net challenge 2023 vòng 2
PDF version: [Here](./pdf_edition.pdf).
![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled.png)

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%201.png)

# 1. Router on a stick

## a/

## b/

### SW1:

! Create VLAN 12, 13

SW1(config-vlan)#vlan 12
SW1(config-vlan)#name VLAN12
SW1(config-vlan)#vlan 13
SW1(config-vlan)#name VLAN13
! Configure trunk for port to R1 
SW1(config-if)# switchport trunk encapsulation dot1q
SW1(config-if)#switchport mode trunk

! Assign int to VLAN 12, mode access

SW1(config)#int e0/1
SW1(config-if)#switchport mode access
SW1(config-if)#switchport access vlan 12
SW1(config-if)#no shut

! Assign int to VLAN 13, mode access

SW1(config-if)#int e0/2
SW1(config-if)#switchport access vlan 13
SW1(config-if)#switchport mode access

SW1#sh vlan

VLAN Name                             Status    Ports

---

1    default                          active    Et0/3, Et1/0, Et1/1, Et1/2, Et1/3
12   VLAN12                           active    Et0/1
13   VLAN13                           active    Et0/2
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2

---

1    enet  100001     1500  -      -      -        -    -        0      0

12   enet  100012     1500  -      -      -        -    -        0      0

13   enet  100013     1500  -      -      -        -    -        0      0

1002 fddi  101002     1500  -      -      -        -    -        0      0

1003 tr    101003     1500  -      -      -        -    -        0      0

1004 fdnet 101004     1500  -      -      -        ieee -        0      0

1005 trnet 101005     1500  -      -      -        ibm  -        0      0

Primary Secondary Type              Ports

---

SW1#sh int trunk

Port        Mode             Encapsulation  Status        Native vlan
Et0/0       on               802.1q         trunking      1

Port        Vlans allowed on trunk
Et0/0       1-4094

Port        Vlans allowed and active in management domain
Et0/0       1,12-13

Port        Vlans in spanning tree forwarding state and not pruned
Et0/0       1,12-13

## c/

### R1:

R1(config)#int e0/1
R1(config-if)#no shut
R1(config-if)#
*Nov  7 18:00:26.934: %LINK-3-UPDOWN: Interface Ethernet0/1, changed state to up
*Nov  7 18:00:27.938: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0/1, changed state to up
R1(config-if)#int e0/1.12
R1(config-subif)#encapsulation dot1Q 12

R1(config-subif)#ip add 192.168.12.1 255.255.255.252
R1(config-subif)#no shut

R1(config)#int e0/1.13

R1(config-subif)#encapsulation dot1Q 13

R1(config-subif)#ip add 192.168.13.1 255.255.255.252
R1(config-subif)#no shut

## d/

### R2:

R2(config-if)#int e0/0
R2(config-if)#no shut
R2(config-if)#ip addr 192.168.12.2 255.255.255.252

R2#ping 192.168.12.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.12.1, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 1/2/4 ms

### R3:

R3(config)#int e0/0
R3(config-if)#no shut
R3(config-if)#ip add 192.168.13.2 255.255.255.252
R3#ping 192.168.13.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.13.1, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 1/1/2 ms

## e/

### R3:

R3(config-if)#int e0/1.50
R3(config-subif)#no shut

R3(config-subif)#encapsulation dot1Q 50
R3(config-subif)#ip addr 192.168.50.1 255.255.255.224
R3(config-subif)#int e0/1.51

R3(config-subif)#encapsulation dot1Q 51

R3(config-subif)#ip addr 192.168.51.1 255.255.255.240
R3(config-subif)#no shut

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%202.png)

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%203.png)

### SW2

SW2(config)#vlan 50
SW2(config-vlan)#name VLAN50
SW2(config-vlan)#vlan 51
SW2(config-vlan)#name VLAN51

SW2(config)#int e0/0

SW2(config-if)#switchport trunk encapsulation dot1q
SW2(config-if)#switchport mode trunk

SW2(config)#int e0/1
SW2(config-if)#switchport mode access

SW2(config-if)#switchport access vlan 50
SW2(config-if)#int e0/2

SW2(config-if)#switchport mode access

SW2(config-if)#switchport access vlan 51

## f/

### SW3 (cấu hình sai, đã no vlan)

iguration commands, one per line.  End with CNTL/Z.
SW3(config)#int e0/0
SW3(config-if)#no shut
SW3(config-if)#swi
SW3(config-if)#switchport e

SW3(config-if)#switchport e?
% Unrecognized command
SW3(config-if)#switchport trunk encapsulation dot1q
SW3(config-if)#swit
SW3(config-if)#switchport mode trunk
SW3(config-if)#
*Nov  7 18:34:37.878: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0/0, changed state to down
SW3(config-if)#
*Nov  7 18:34:40.893: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0/0, changed state to up
SW3(config-if)#int e0/1
SW3(config-if)#swi
SW3(config-if)#vlan 10

SW3(config-vlan)#name VLAN10
SW3(config-vlan)#vlan 20
SW3(config-vlan)#name VLAN20
SW3(config-vlan)#vlan 30
SW3(config-vlan)#name VLAN30
SW3(config-vlan)#

### R6:

R6(config)#int e0/1
R6(config-if)#no shut
R6(config-if)#int e0/1.60
R6(config-subif)#no shut
R6(config-subif)#encapsulation dot1Q 60
R6(config-subif)#ip addr 192.168.60.0 255.255.255.0
Bad mask /24 for address 192.168.60.0
R6(config-subif)#ip addr 192.168.60.1 255.255.255.0
R6(config-subif)#int e0/1.61

R6(config-subif)#encapsulation dot1Q 61

R6(config-subif)#no shut
R6(config-subif)#ip addr 192.168.61.1 255.255.255.0

### SW7:

SW7(config)#vlan 60
SW7(config-vlan)#name VLAN60
SW7(config-vlan)#vlan 61
SW7(config-vlan)#name VLAN61

SW7(config-if)#int e0/1
SW7(config-if)#no shut
SW7(config-if)#switchport access vlan 60
SW7(config-if)#int e0/2
SW7(config-if)#no shut
SW7(config-if)#switchport access vlan 61
SW7(config-if)#int e0/0
SW7(config-if)#no shut
SW7(config-if)#switchport trunk encapsulation dot1q
SW7(config-if)#switchport mode trunk

### VPC4

VPCS> ip 192.168.60.100 24 192.168.60.1

VPCS> save

### VPC5

VPCS> ip 192.168.61.100 25 192.168.61.1

VPCS> save

# 2. VTP

## a/

### SW4:

! Configure trunk
SW4(config)#int e0/1
SW4(config-if)#no shut

SW4(config-if)#switchport trunk encapsulation dot1q
SW4(config-if)#switchport mode trunk
SW4(config-if)#int e0/0
SW4(config-if)#switchport trunk encapsulation dot1q

SW4(config-if)#switchport mode trunk
SW4(config-if)#no shut

SW4#sh int trunk

Port        Mode             Encapsulation  Status        Native vlan
Et0/0       on               802.1q         trunking      1
Et0/1       on               802.1q         trunking      1

Port        Vlans allowed on trunk
Et0/0       1-4094
Et0/1       1-4094

Port        Vlans allowed and active in management domain
Et0/0       1
Et0/1       1

Port        Vlans in spanning tree forwarding state and not pruned
Et0/0       1
Et0/1       none

### SW3:

! Configure trunk
SW3(config)#int e0/1
SW3(config-if)#no shut
SW3(config-if)#switchport trunk encapsulation dot1q
SW3(config-if)#switchport mode trunk
SW3(config-if)#int e0/2
SW3(config-if)#no shut
SW3(config-if)#switchport trunk encapsulation dot1q
SW3(config-if)#switchport mode trunk
SW3(config-if)#do copy run start

### SW5:

! Configure trunk
SW5(config)#int e0/2
SW5(config-if)#no shut
SW5(config-if)#switchport trunk encapsulation dot1q
SW5(config-if)#switchport mode trunk
SW5(config-if)#int e0/1
SW5(config-if)#no shut
SW5(config-if)#switchport trunk encapsulation dot1q
SW5(config-if)#switchport mode trunk
SW5(config-if)#do wr

SW5#sh int trunk

Port        Mode             Encapsulation  Status        Native vlan
Et0/1       on               802.1q         trunking      1
Et0/2       on               802.1q         trunking      1

Port        Vlans allowed on trunk
Et0/1       1-4094
Et0/2       1-4094

Port        Vlans allowed and active in management domain
Et0/1       1
Et0/2       1

Port        Vlans in spanning tree forwarding state and not pruned
Et0/1       1
Et0/2       1

## b/ Lưu ý cấu hình VTP server trước (default mode là server)

### SW3:

SW3(config)#vtp domain NetChallenge
Changing VTP domain name from NULL to NetChallenge
SW3(config)#vtp password NetChallenge

### SW4:

SW4(config)#vtp mode client
SW4(config)#vtp domain NetChallenge
Changing VTP domain name from NULL to NetChallenge
SW4(config)#vtp password NetChallenge

SW4(config)#do wr

### SW5:

như SW4

## c/

### SW3:

SW3(config)#vlan 10
SW3(config-vlan)#name NC-Staff
SW3(config-vlan)#vlan 20
SW3(config-vlan)#name NC-Student
SW3(config-vlan)#vlan 30
SW3(config-vlan)#name NC-Guest

### SW4: (check vtp)

SW4#sh vlan

VLAN Name                             Status    Ports

---

1    default                          active    Et0/2, Et0/3, Et1/0, Et1/1
Et1/2, Et1/3
10   NC-Staff                         active

20   NC-Student                       active

30   NC-Guest                         active

1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2

---

1    enet  100001     1500  -      -      -        -    -        0      0

10   enet  100010     1500  -      -      -        -    -        0      0

20   enet  100020     1500  -      -      -        -    -        0      0

30   enet  100030     1500  -      -      -        -    -        0      0

1002 fddi  101002     1500  -      -      -        -    -        0      0

1003 tr    101003     1500  -      -      -        -    srb      0      0

1004 fdnet 101004     1500  -      -      -        ieee -        0      0

1005 trnet 101005     1500  -      -      -        ibm  -        0      0

Primary Secondary Type              Ports

---

### SW5: (check vtp)

SW5#sh vlan

VLAN Name                             Status    Ports

---

1    default                          active    Et0/0, Et0/3, Et1/0, Et1/1
Et1/2, Et1/3
10   NC-Staff                         active

20   NC-Student                       active

30   NC-Guest                         active

1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2

---

1    enet  100001     1500  -      -      -        -    -        0      0

10   enet  100010     1500  -      -      -        -    -        0      0

20   enet  100020     1500  -      -      -        -    -        0      0

30   enet  100030     1500  -      -      -        -    -        0      0

1002 fddi  101002     1500  -      -      -        -    -        0      0

1003 tr    101003     1500  -      -      -        -    srb      0      0

1004 fdnet 101004     1500  -      -      -        ieee -        0      0

1005 trnet 101005     1500  -      -      -        ibm  -        0      0

Primary Secondary Type              Ports

---

### d/

### VPC1:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%204.png)

### VPC2:

Checking for duplicate address...
VPCS : 192.168.20.100 255.255.252.0 gateway 192.168.20.1

### VPC3:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%205.png)

### SW4:

SW4(config)#int e0/2
SW4(config-if)#switchport mode access
SW4(config-if)#switchport access vlan 10
SW4(config-if)#int e0/3
SW4(config-if)#switchport mode access
SW4(config-if)#switchport access vlan 20

### SW5:

! na ná

### SW2:

### SW7:

### Kiểm tra:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%206.png)

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%207.png)

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%208.png)

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%209.png)

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2010.png)

VPC4 → VPC5 ⇒ R6, SW7 hoạt động VLAN 60, 61

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2011.png)

VPC1 → SW1 (int e0/2 ip .2):

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2012.png)

SW5 (int e0/3 ip.2)→ VPC2 :

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2013.png)

FileServer → WebServer:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2014.png)

PC1 → R2:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2015.png)

PC1 → interface VLan30 của R2:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2016.png)

PC1 →  interface VLan20 của R2:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2017.png)

PC1 → PC3:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2018.png)

PC1 → PC2:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2019.png)

# 3. STP

## a

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2020.png)

### SW3:

spanning-tree vlan 10 priority 4096

! Due to the default configuration, there is no more to configure to match flow SW4 → SW3

## Check:

### SW4:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2021.png)

### SW5:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2022.png)

### SW3:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2023.png)

## b/ VPC2 → SW4 → SW5 → SW3 ⇒ vlan 20

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2024.png)

### SW3:

(conf-t): spanning-tree vlan 20 priority 4096

### SW4:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2025.png)

### SW5:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2026.png)

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2027.png)

### Kiểm tra:

### SW3:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2028.png)

### SW4:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2029.png)

### SW5:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2030.png)

## c/ VPC3 → SW5 → SW3 → R2

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2031.png)

### SW3:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2032.png)

! The default configuration matches the requirement: VPC3 → SW5 → SW3 → R2

## Kiểm tra:

### SW3:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2033.png)

PC1 → PC3:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2034.png)

PC1 → R2 interface vlan 30:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2035.png)

PC2 → R2 interface vlan 20:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2036.png)

# 4. Routing

## a/

### R1:

router ospf 100
network 192.168.12.1 0.0.0.0 area 0
network 192.168.13.1 0.0.0.0 area 0

### R2:

router ospf 100
network 192.168.10.1 0.0.0.0 area 0
network 192.168.12.2 0.0.0.0 area 0
network 192.168.20.1 0.0.0.0 area 0
network 192.168.32.1 0.0.0.0 area 0

### R3:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2037.png)

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2038.png)

## Check:

### R1:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2039.png)

### R2:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2040.png)

### R3:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2041.png)

## b/

### R6:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2042.png)

## c/

! R1 has a static replicated route, perhaps received from DHCP. 

! New knowledge note:

redistribute <routing process> (static, EIGRP, BGP, RIP, etc) will redistribute only classful subnets (like /8, /16, /24). If want to redistribute classless subnets, please append keyword “subnets” into the command.

redistribute static will not include the default static route. Refer to [Here](https://community.cisco.com/t5/switching/default-information-originate-and-redistribute-static-ospf/td-p/1822504?fbclid=IwAR327PYvrVLcaXNShjmf9C1wHew5R_PcPo3a2HSNoDGdsxuGvJuFTzAzvyQ).

### R1:

router ospf 100
redistribute static subnets
network 192.168.12.1 0.0.0.0 area 0
network 192.168.13.1 0.0.0.0 area 0
default-information originate

### Check:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2043.png)

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2044.png)

R3 → interface e0/0 of R1:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2045.png)

R2 → interface e0/0 of R1:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2046.png)

VPC 3 → Web Server:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2047.png)

# 5. NAT

## a.1/ NAT for R1

### R1:

R1(config-if)#int e0/0
R1(config-if)#ip nat outside
R1(config)#int e0/1.12
R1(config-subif)#ip nat inside
R1(config-subif)#int e0/1.13
R1(config-subif)#ip nat inside

R1(config)#access-list 1 permit 192.168.10.100
R1(config)#access-list 1 permit 192.168.20.100
R1(config)#access-list 1 permit 192.168.32.100
R1(config)#access-list 1 permit 192.168.50.10
R1(config)#access-list 1 permit 192.168.51.10

R1(config)#ip nat inside source list 1 interface e0/0 overload

R1#sh access-lists 1

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2048.png)

## Check:

VPC1 → 8.8.8.8:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2049.png)

VPC3 → 8.8.8.8:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2050.png)

File Server → 8.8.8.8:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2051.png)

Web Server → 8.8.8.8:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2052.png)

## a.2/

### R3:

ip nat inside source static 192.168.50.10 100.100.100.100

## Check: (vpc6 → 100.100.100.100)

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2053.png)

! I don’t know how to configure IP in VPC6 until check the cloud “Internet”
! Turn out the cloud is a router, with configuration is:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2054.png)

### VPC6:

VPCS> ip 111.111.111.120/24 111.111.111.111

ping 100.100.100.100

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2055.png)

# 6. ACL

## a/

! advice: “extended ACLs approved as close to the src as possible” ⇒ R1 interface e0/1 direction in

### R1:

access-list 100 permit tcp 192.168.20.0 0.0.3.255 host 192.168.51.10 eq ftp
access-list 100 permit ip192.168.10.0 0.0.0.255 host 192.168.51.10
access-list 100 deny   ip any host 192.168.51.10
access-list 100 permit ip any any
R1(config)#int e0/1.12
R1(config-subif)#ip access-group 100 in

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2056.png)

### Check:

VPC 1, VPC2 → File server:

![Untitled](Net%20challenge%202023%20vo%CC%80ng%202%206a2237e0d07446bda32c524a89fc878f/Untitled%2057.png)

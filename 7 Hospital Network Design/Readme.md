Hetauda Health Services is a well-established health provider in Nepal, which offers health solutions and services to its clients. The institution operates in two locations within the same city, having the hospital headquarters 80km away from the branch hospital. Therefore, it has the following departments within its main headquarters: 
- Medical Lead Operation & Consultancy Services (MLOCS)
- Medical Emergency and Reporting (MER)
- Medical Records Management (MRM)
- Information Technology (IT)
- Customer Service (CS)
- Guest/Waiting Area (GWA)

The branch hospital was designed to share the workloads with the headquarters. Hence, it contains the following departments: 
- Nurses & Surgery Operations (NSO)
- Hospital Labs (HL)
- Human Resources (HR)
- Marketing (MK)
- Finance (FIN)
- Guest/Waiting Area (GWA)

So far, the network was using third-party services to maintain its IT services. The senior management has decided to own their network infrastructure, including Local Area Network (LAN), Wide Area Network (WAN), and a server-side site that is expected to be located separately at the headquarters and connected to the HQ router with an access switch. The server-side site will host the DHCP server, DNS Server, Web Server, and Email Server.

The network is expected to be cost-effective and observes the information security rule of the CIA (Confidentiality, Integrity, and Availability). The network is expected to have a hierarchical model with two already purchased core routers (one at HQ and one branch) each connecting to two subscribed ISPs. Due to security requirements, it has been decided that all branches be interconnected with the headquarters through secure VPN tunnels.

You have been hired as a network security engineer to design the network according to the requirements set by the senior management. You will consult an appropriate robust network design model to meet the design requirements. You will also implement Access Control Lists and Virtual Private Network (VPN) to enable secure communication considering security and network performance factors paramount to safeguarding Confidentiality, Integrity, and Availability of data and communication. The network security policy will comprehensively dictate the user's access to each site using Access Control List (ACL).

- [Requirements](#network-design-and-implementation-requirements)
- [Config Steps](#config-steps)
- [Commands](#commands)
   - [Basic Settings](#1-basic-settings-to-all-devices-plus-ssh-on-the-routerscore-layer-router--and-l3-switchesdistrubtion-layer-switchl3)
   - [Vlan Acess - Trunk port ](#2-vlans-assignment-plus-all-access-and-trunk-ports-on-l2-and-l3-switches)
   - [Switchport security](#3-switchport-security-to-finance-department)
   - [Ip addressing | subnetting](#4-subnetting-and-ip-addressing)
   - [OSPF | Router | Switches](#5-ospf-on-the-routers-and-l3-switches)
   - [Static Ip](#6-static-ip-address-to-serverroom-devices)
   - [DHCP configuration](#7-dhcp-server--configurations)
   - [Inter-Vlan Routing | dhcp helper](#8-inter-vlan-routing-on-the-l3-switches-plus-ip-dhcp-helper-addresses)
   - [Wireless Configuration](#9-wireless-network-configurations)
   - [Site-to-site ipsec vpn](#10-site-to-site-ipsec-vpn)
   - [Static Route](#11-default-static-route)
   - [Access Control list](#12-pat--access-control-list)





# Network Design and Implementation Requirements

1. Use Cisco Packet Tracer to design and implement the network solution.
2. Use a hierarchical model providing redundancy in the network.
3. Both HQ and Branch routers are expected to be connected using a serial connection.
4. As mentioned earlier, for network cost-effectiveness, each site is expected to have one core router, two multilayer switches, and several access switches connecting each department.
5. Each department is required to have a wireless network for the users.
6. Every department in HQ is estimated to have around 60 users while in Branch is estimated to be 30 users.
7. Each department should be in a different VLAN and a different subnetwork.
8. Provided a base network of 192.168.100.0, and carry out subnetting to allocate the correct number of IP addresses to each department.
9. The company network is connected to the static, public IP addresses (Internet Protocol) 195.136.17.0/30, 195.136.17.4/30, 195.136.17.8/30, and 195.136.17.12/30 connected to the two Internet providers.
10. Configure basic device settings such as hostnames, console password, enable password, banner messages, and disable IP domain lookup.
11. Devices in all the departments are required to communicate with each other with the respective multilayer switch configured for inter-VLAN routing.
12. The Multilayer switches are expected to carry out both routing and switching functionalities and thus will be assigned IP addresses.
13. All devices in the network are expected to obtain an IP address dynamically from the dedicated DHCP servers located in the server room.
14. Devices in the server room are to be allocated IP addresses statically.
15. Use OSPF as the routing protocol to advertise routes both on the routers and multilayer switches.
16. Configure default static routing to enable routers and multilayer switches to forward any traffic that does not match routing table entries. Use next-hop IP addresses.
17. Configure SSH in all the routers and layer three switches for remote login.
18. Configure port-security for the server site department switch to allow only one device to connect to a switch port, use sticky method to obtain mac-address and violation mode shutdown.
19. Configure the extended ACL rule together with site-to-site VPN (IPSec VPN) to create a tunnel and encrypt communication between HQ and the Branch network.
20. Configure PAT to use the respective outbound router interface IPv4 address, and implement the necessary ACL rule.
21. Test Communication, ensure everything configured is working as expected.


# config steps

0. Netwok Design and beautification.
1. Basic settings to all devices plus ssh on the routers and l3 switches.
2. VLANs assignment plus all access and trunk ports on l2 and l3 switches.
3. Switchport security to finance department.
4. Subnetting and IP addressing
5. OSPF on the routers and l3 switches.
6. Static IP address to serverRoom devices.
7. DHCP server device configurations.
8. Inter-VLAN routing on the l3 switches plus ip dhcp helper addresses.
9. Wireless network configurations.
10. Site-to-site IPSec VPN
11. Default static route
12. PAT + Access Control List
14. Verifying and testing configurations.



# commands 
#### 1. Basic settings to all devices plus **ssh** on the **routers**```Core layer Router  ```and **l3 switches**```Distrubtion layer Switch(l3)```


```
en
conf t
hostname ISP-Fibernet-Router
banner motd  # This is Branch-Router#
```
```
line console 0
password cisco
login
exit
```
```
ip domain-name hackthacker.com
username hackthacker password hackthacker
crypto key generate rsa
1024
```
```
line vty 0 15
login local
transport input ssh
exit
```
```
no ip domain-lookup 
enable password cisco
service password-encryption
do wr
```


##### Basic settings to **switches**```Access layer Switch(l2)```
```
en
conf t
hostname Server-management-sw
banner motd  # This is Server-management switch#
```
```
line console 0
password cisco
login
exit
```
```
line vty 0 15
password cisco
login
exit
```
```
no ip domain-lookup 
enable password cisco
service password-encryption

do wr
```
---
#### 2. VLANs assignment plus all **access** and **trunk** ports on **l2 and l3 switches**.

##### Acess layer (l2 switch )
**Trunk**
```
en
conf t
int range fa0/1-2
switchport mode trunk
ex
```

**Access**
```
vlan 100
name MLOCS
exit
int range fa0/3-24
switchport mode access 
switchport access vlan 100
do wr
```


##### Distrubtion layer (l3 switch )
```
conf t
vlan 200
vlan 210
vlan 220
vlan 230
vlan 240
vlan 250
ex
```
**Trunk**
```
int range gig1/0/1-7
switchport mode trunk
ex
do wr
```

---
#### 3. Switchport security to finance department.
```
int range fa0/2-24
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation shutdown
do wr
end
```


---


#### 4. Subnetting and IP addressing


### IP Addressing
**Base Network: 192.168.100.0**

#### HQ Hospital

| Department | Network Address | Subnet Mask | Host Address Range | Broadcast Address |
|------------|------------------|-------------|--------------------|-------------------|
| MLOCS      | 192.168.100.0    | 255.255.255.192/26 | 192.168.100.1 to 192.168.100.62 | 192.168.100.63 |
| MER        | 192.168.100.64   | 255.255.255.192/26 | 192.168.100.64 to 192.168.100.126 | 192.168.100.127 |
| MRM        | 192.168.100.128  | 255.255.255.192/26 | 192.168.100.129 to 192.168.100.190 | 192.168.100.191 |
| IT         | 192.168.100.192  | 255.255.255.192/26 | 192.168.100.193 to 192.168.100.254 | 192.168.100.255 |
| CS         | 192.168.101.0    | 255.255.255.192/26 | 192.168.101.1 to 192.168.101.62 | 192.168.101.63 |
| GWA        | 192.168.101.64   | 255.255.255.192/26 | 192.168.101.64 to 192.168.101.126 | 192.168.101.127 |



##### Branch Hospital

| Department | Network Address    | Subnet Mask         | Host Address Range                          | Broadcast Address    |
|------------|--------------------|---------------------|---------------------------------------------|----------------------|
| NSO        | 192.168.101.128    | 255.255.255.224/27  | 192.168.101.129 to 192.168.101.158          | 192.168.101.159      |
| HL         | 192.168.101.160    | 255.255.255.224/27  | 192.168.101.161 to 192.168.101.190          | 192.168.101.191      |
| HR         | 192.168.101.192    | 255.255.255.224/27  | 192.168.101.193 to 192.168.101.222          | 192.168.101.223      |
| MK         | 192.168.101.224    | 255.255.255.224/27  | 192.168.101.225 to 192.168.101.254          | 192.168.101.255      |
| FIN        | 192.168.102.0      | 255.255.255.224/27  | 192.168.102.1 to 192.168.102.30             | 192.168.102.31       |
| GWA        | 192.168.102.32     | 255.255.255.224/27  | 192.168.102.33 to 192.168.102.62            | 192.168.102.63       |



##### Server-side Site

| Department | Network Address    | Subnet Mask         | Host Address Range                         | Broadcast Address    |
|------------|--------------------|---------------------|--------------------------------------------|----------------------|
| SSS        | 192.168.102.64     | 255.255.255.240/28  | 192.168.102.65 to 192.168.102.78           | 192.168.102.79       |


##### Between the Routers and Layer-3 Switches

| No.              | Network Address       |
|------------------|-----------------------|
| HQR1 - HQMLSW1   | 192.168.102.80/30     |
| HQR1 - HQMLSW2   | 192.168.102.84/30     |
| BRR1 - BRMLSW1   | 192.168.102.88/30     |
| BRR1 - BRMLSW1   | 192.168.102.92/30     |
| HQR1 - BRR1      | 192.168.102.96/30     |

##### Between the Routers and ISPs

Public IP addresses 195.136.17.0/30, 195.136.17.4/30, 195.136.17.8/30, and 195.136.17.12/30



**Assigning ip address to multi layer switch (l3switch)** 
```
en
conf t
int gig1/0/8
no switchport
ip address 192.168.102.81 255.255.255.252 
ex
do wr
```

**Assigning ip address to Router**
```
interface Serial0/0/0
ip address 195.136.17.1 255.255.255.252
clock rate 64000
no shutdown
ex
do wr
```

---
#### 5. OSPF on the routers and l3 switches.

**l3 Switch OSPF**
```
conf t
ip routing 
router ospf 10
network 192.168.101.128 255.255.255.224 area 0
network 192.168.101.160 255.255.255.224 area 0
network 192.168.101.192 255.255.255.224 area 0
network 192.168.101.224 0.0.0.31 area 0
network 192.168.102.0 0.0.0.31 area 0
network 192.168.102.32 0.0.0.31 area 0

network 192.168.102.92 0.0.0.3 area 0


do wr
```

**Router OSPF**
```
conf t
router ospf 10
network 192.168.102.80 0.0.0.3 area 0
network 192.168.102.84 0.0.0.3 area 0
network 192.168.102.64 0.0.0.15 area 0
network 195.136.17.4 0.0.0.3 area 0
network 195.136.17.0 0.0.0.3 area 0

ex
do wr
```

**ISP ROuter OSPF**
```
conf t
router ospf 10
network 195.136.17.12 0.0.0.3 area 0
network 195.136.17.4 0.0.0.3 area 0
do wr
```

#### 6. Static IP address to serverRoom devices.
You have do manually on this **ServerRoom Devices**

---

#### 7. DHCP server  configurations. 

you have do manually on this **DHCP server**

---

#### 8. Inter-VLAN routing on the l3 switches plus ip dhcp helper addresses.


inter-vlan routing on server-site from **Router**
```
int gig0/2
exit
int gig0/2.300
encapsulation dot1Q 300
ip address 192.168.102.65 255.255.255.240
ex
do wr
```


inter-vlan routing on **L3 switch**
```
conf t
interface Vlan100
ip address 192.168.100.1 255.255.255.192
ip helper-address 192.168.102.67
no shutdown
```
```
interface Vlan200
ip address 192.168.101.129 255.255.255.224
ip helper-address 192.168.102.67
no shutdown
```
---

#### 9. Wireless network configurations.

Do manually on  your **Access-point**

---
#### 10. Site-to-site IPSec VPN

```
license boot module c2900 technology-package securityk9 
do reload
```
```
access-list 110 permit ip 192.168.100.0 0.0.0.255 192.168.101.128 0.0.0.255
access-list 110 permit ip 192.168.101.0 0.0.0.127 192.168.101.128 0.0.0.255
do wr
```
```
crypto isakmp policy 10
encryption aes 256
authentication pre-share 
group 5
ex
```
```
crypto isakmp key hackthacker address 192.168.102.98
crypto ipsec transform-set vpn-set esp-aes esp-sha-hmac 
```
```
crypto map vpn-map 10 ipsec-isakmp
description This is vpn connects to Branch=Hosptital-Network
set peer 192.168.102.98
set transform-set vpn-set
match address 110
```

```
int ser0/0/1
crypto map vpn-map
ex
do wr
do sh crypto ipse sa
```


### HQ Route Aggregation


192.168.100.0/26
192.168.100.64/26
192.168.100.128/26
192.168.100.192/26

-- Summarised as 192.168.100.0/24



192.168.101.0/26
192.168.101.64/26

-- Summarised as 192.168.101.0/25


### BR Route Aggregation


192.168.101.128/27
192.168.101.160/27
192.168.101.192/27
192.168.101.224/27
192.168.102.0/27
192.168.102.32/27

-- Summarised as 192.168.101.128/24

---

#### 11. Default static route
**Router**
```
ip route 0.0.0.0 0.0.0.0 195.136.17.2
ip route 0.0.0.0 0.0.0.0 195.136.17.6 70
```


**l3 switch**

---
#### 12. PAT + Access Control List
```
int ser0/0/0
ip nat outside 
int ser0/1/0
ip nat outside 
ex
```
```
int range gig0/0-1
ip nat inside 
do wr
```
```
ip nat inside source list 1 interface ser0/0/1 overload 
ip nat inside source list 1 interface ser0/1/0 overload
do wr
```

```
access-list 1 permit 192.168.100.0 0.0.0.63     
access-list 1 permit 192.168.100.64 0.0.0.63    
access-list 1 permit 192.168.100.128 0.0.0.63   
access-list 1 permit 192.168.100.192 0.0.0.63  
do wr
do sh ip nat translation 
```
---
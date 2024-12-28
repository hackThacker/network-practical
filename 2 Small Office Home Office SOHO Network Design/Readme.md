# Design a Network in Cisco Packet Tracer


- [Task](#task)
- [solutions of subnet](#subnetting-for-xyz-company-network-design)
- [solutions of commands](#solutions-of-commands)
  - [switch commands](#switch-commands)
  - [switch command to router port](#switch-command-to-router-port)
  - [Router commands](#router-commands)
  - [Router DHCP commands](#router-dhcp-commands)




---

## Task:

# XYZ Company Network Design for Bonalbo Branch

XYZ company is a fast-growing company in Eastern Australia with more than 2 million customers globally. The company deals with selling and buying of food items, which are basically operated from the headquarters. The company is intending to open a branch near the local village Bonalbo. Thus, the company requires young IT graduates to design the network for the branch. The network is intended to operate separately from the HQ network.  
Being a small network, the company has the following requirements during implementation:

a) One router and one switch to be used (all CISCO products).  
b) 3 departments (Admin/IT, Finance/HR and Customer service/Reception).  
c) Each department is required to be in different VLANS.  
d) Each department is required to have wireless network for the users.  
e) Host devices in the network are required to obtain IPv4 address automatically.  
f) Devices in all the departments are required to communicate with each other.

Assume the ISP gave out a base network of 192.168.1.0, you as the young network engineer who has been hired, design and implement a network considering the above requirements.


---

# Subnetting for XYZ Company Network Design

### Base Network: 192.168.1.0

- **No. of Subnets** = 3
- **Formula for Subnets** = 2^n
- **2^n = 3** → n = 2

### Class C Network Details
- Default Subnet Mask: `255.255.255.0` (Binary: `11111111.11111111.11111111.00000000`)

### Subnetting Process
- After borrowing 2 bits, the new subnet mask becomes:
  - New Subnet Mask: `255.255.255.192` (Binary: `11111111.11111111.11111111.11000000`)
  - Block Size: 64

### Subnet Details

#### 1st Subnet
- **Network ID**: 192.168.1.0  
- **Broadcast ID**: 192.168.1.63  
- **Host Range**: 192.168.1.1 - 192.168.1.62  

#### 2nd Subnet
- **Network ID**: 192.168.1.64  
- **Broadcast ID**: 192.168.1.127  
- **Host Range**: 192.168.1.65 - 192.168.1.126  

#### 3rd Subnet
- **Network ID**: 192.168.1.128  
- **Broadcast ID**: 192.168.1.191  
- **Host Range**: 192.168.1.129 - 192.168.1.190


---
### solutions of Commands

#### Switch commands 
 ```
en
Conf t
int range fa0/1-4 or single int fa0/1
switchport mode access
switchport access vlan 10
do wr
exit
do sh start ( to show interface details and vlan)
 ```
  same as other also 

---

#### Switch command to router port
 ```
int gig0/1
switchport mode trunk
do wr
 ```
  same as other also 

---

#### Router commands 
 ```
en
conf t
int gig0/0
no shutdown
exit
int gig0/0.10
encapsulation dot1Q 10 (enter the vlan number)
ip address 192.168.1.1 255.255.255.192 
do wr
exit
do sh start
 ```
 same as other also 

---

####  Router dhcp commands 
 ```
service dhcp
ip dhcp pool Admin-pool
network 192.168.1.0 255.255.255.192
default-router 192.168.1.1
dns-server 192.168.1.1
domain-name Admin.com
do wr
exit
 ```
same as other also 

---



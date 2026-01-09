# IP Telephony (VoIP) and Dial-Peering Networking Project

HackThacker Consultancy Limited specialized in delivering IT infrastructure solutions to medium-sized organizations worldwide. With the expansion of the company, a newly acquired branch needs a network. Your manager is faced with the demands of business and a plethora of technology challenges.

You have been recently hired as a Network Engineer and assigned the task of designing and implementing a VoIP network that is based on the requirements and specifications outlined by your manager.

Each group has been assigned the task of designing, and implementing a network infrastructure for HackThacker Consultancy Limited by inter-networking three departments which are as follows:

| Finance: | HR: |
| --- | --- |
| 20 Phones + 20 PCs & 1 printer | 20 Phones + 20 PCs & 1 printer |

| Cybersecurity: | ICT: |
| --- | --- |
| 20 Phones + 20 PCs & 1 printer | 20 Phones + 20 PCs & 1 printer |


- All desktops have an associated telephone set (each PC is connecting directly to a Phone, not a switch).

The network consists of four servers located at the server side site and is fully configured for the operations, and all servers are shared between all users.

|          |          |
|----------|----------|
| • HTTP   | • Email  |
| • DNS    | • DHCP   |

### Requirements

The IT Manager emphasized scalability and availability, and hence you are required to provide a complete network infrastructure design and implementation. Turtle Consultancy Limited will be using the following IP address: 192.168.100.0/24 for Data, 172.16.100.0/24 for Voice, and 10.10.10.0/24 between the routers.

1. **Design** a networked system to meet the given specifications. **Use packet tracer software to design your network.**

2. **Routers** - Each department is to have VoIP enabled router with server-side LAN attached to the ICT department router. Note: use Cisco 2811 router.

3. **Switches** - Each department has an access layer switch. Note: use Cisco 2960 switch.

4. **Connections** - Use serial connections between a router and a router, then a straight-through cable between the router to switch, switch to hosts, phones to PCs.

5. **Subnets** - Each department will be accessing two subnetworks, for example, data and voice subnets. Note: carry out appropriate subnetting.

6. **Basic settings** - Configure basic device settings such as hostnames, console passwords, enable passwords, banner messages, encrypt all passwords, and disable IP domain lookup.

7. **DHCP Server** - For voice (VoIP), use the respective router as the DHCP server while for Data use the DHCP server device at the server-side site.

8. **VLANs** - Each department will be in two VLANs. One for data and another for voice. Note: All IP phones in the network should be in VLAN 100.

9. **Inter-VLAN Routing** - Use router-on-a-stick to enable inter-VLAN routing on the network. Note: create subinterfaces for both data and voice VLANs.
10. **IP Addressing** - All devices in the network are expected to obtain an IP address dynamically from the respective DHCP servers while the devices in the server room are to be allocated IP addresses statically.
11. **Routing protocol** - Use OSPF as the routing protocol to advertise routes on the routers.
12. **Remote Access** - Configure SSH in all the routers for remote login.
13. **Telephony service** - Configure VoIP on the routers and allocate dial numbers in this format for the departments, Finance(1..), HR (2..), Sales (3..), and ICT (4..) (where 1.. can be 101 to 199) and so on.
14. **Routing for VoIP**  - Configure dial-peering on the routers to allow IP phones from different routers to communicate.
15. **Finalize**  - Test Communication, ensure everything configured is working as expected.
  

#### CONFIG STEPS 

0. Network Design and beautification.
1. Basic settings to all devices plus ssh on the routers.
2. VLANs assignment plus all access and trunk ports on the switches.
3. Subnetting and IP addressing.
4. Static IP address to serverRoom devices.
5. DHCP server device configurations.
6. Configure DHCP for Voice.
7. Inter-VLAN routing on the Routers plus ip dhcp helper addresses.
8. OSPF on the routers.
9. Configure VoIP configuration in all routers.
10. Dial peering configuration in all routers.
11. Verifying and testing configurations.

---

# Commands

#### Basic Settings


######  ROUTER
1. Basic settings to **Router** plus **ssh** on the routers.
```
en
conf t
hostname Finance-Router
banner motd  # This is  Finance-Router#
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

 Basic settings to **L2** **switch**.
 ###### Switch
```
en
conf t
hostname Finance-sw
banner motd  # This is Finance switch#
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

#### VLANs assignment

2. VLANs assignment plus all access and trunk ports on the switches.
```
en
conf t
int range fa0/1
switchport mode trunk
ex
do wr
```
```
conf t
vlan 20 
name DATA
vlan 100
name VOICE
ex
int range fa0/2-24
switchport mode access 
switchport access vlan 10
switchport voice vlan 100
ex
do wr
```
---
#### Subnetting and IP addressing.


**PCs + Printers**

**Base Network: 192.168.100.0**

| Department | Network Address | Devices PCs + Printers | Subnet Mask       | Host Address Range                  | Broadcast Address  |
|------------|------------------|------------------------|-------------------|-------------------------------------|--------------------|
| Finance    | 192.168.100.0    | 21                     | 255.255.255.224/27| 192.168.100.1 to 192.168.100.30     | 192.168.100.31     |
| HR         | 192.168.100.32   | 21                     | 255.255.255.224/27| 192.168.100.33 to 192.168.100.62    | 192.168.100.63     |
| Sales      | 192.168.100.64   | 21                     | 255.255.255.224/27| 192.168.100.65 to 192.168.100.94    | 192.168.100.95     |
| ICT        | 192.168.100.96   | 21                     | 255.255.255.224/27| 192.168.100.97 to 192.168.100.126   | 192.168.100.127    |
| ServerSide | 192.168.100.128  | 4                      | 255.255.255.248/29| 192.168.100.129 to 192.168.100.134  | 192.168.100.135    |


### IP Phones

**Base Network: 172.16.100.0**

| Department | Network Address | Phones | Subnet Mask         | Host Address Range                | Broadcast Address |
|------------|------------------|--------|---------------------|-----------------------------------|-------------------|
| Finance    | 172.16.100.0     | 20     | 255.255.255.224/27  | 172.16.100.1 to 172.16.100.30     | 172.16.100.31     |
| HR         | 172.16.100.32    | 20     | 255.255.255.224/27  | 172.16.100.33 to 172.16.100.62    | 172.16.100.63     |
| Sales      | 172.16.100.64    | 20     | 255.255.255.224/27  | 172.16.100.65 to 172.16.100.94    | 172.16.100.95     |
| ICT        | 172.16.100.96    | 20     | 255.255.255.224/27  | 172.16.100.97 to 172.16.100.126   | 172.16.100.127    |


### Between the Routers

| No.              | Network Address  |
|------------------|------------------|
| Finance to HR    | 10.10.10.0/30    |
| Finance to ICT   | 10.10.10.4/30    |
| Cybersecurity to HR      | 10.10.10.8/30    |
| Cybersecurity to ICT     | 10.10.10.12/30   |


---

#### DHCP for Voice
6. Configure DHCP for Voice
```
service dhcp 
ip dhcp excluded-address 172.16.100.97
ip dhcp pool ICTVOICE
network 172.16.100.96 255.255.255.224
default-router 172.16.100.97 
option 150 ip 172.16.100.97
ex
do wr
```
---

#### Inter-VLAN routing
7. Inter-VLAN routing on the **Routers** plus ip dhcp helper addresses.

```
int fa0/0.10
encapsulation dot1Q 10
ip address 192.168.100.1 255.255.255.224
ip helper-address 192.168.100.130
ex
```
```
int fa0/0.100
encapsulation dot1Q 100
ip address 192.168.100.1 255.255.255.224
ex
do wr
```

#### OSPF
8. OSPF on the routers.
```
conf t
router ospf 10
network 10.10.10.4  0.0.0.3  area 0
network 10.10.10.12  0.0.0.3 area 0
network 192.168.100.96 0.0.0.31 area 0
network 172.16.100.96 0.0.0.31 area 0
network 192.168.100.128  0.0.0.7 area 0
ex
do wr
```
---

#### VoIP configuration
9. Configure VoIP configuration in all routers.
```


```

1hrs:32 min
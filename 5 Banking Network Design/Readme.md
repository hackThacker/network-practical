# Banking Network Design


Nebula Financial Services is a UK-owned company that deals with Banking and Insurance. The company is intending to expand its services across the Asian continent, having the first branch located in Hetauda, Nepal. The company has secured a four-story building to operate within the kathmandu capital city. Therefore, the company would like to allow sourcing the knowledge from a group of final-year students from the local university to design and implement their company network. Assume you are among the students to take over this role. Carefully read down the requirements, then model the design and implement the network based on the company's needs. Each floor has departments as provided in the table below.

- [Task](#requirements)
- [Configuration ](#config-steps)
- [Calculating Subnetting Tables ](#subnetting-calculation)
- [IP Table Addressing](#ip-addressing)
  1. [First Floor](#first-floor-ip)
  2. [Second Floor](#second-floor-ip)
  3. [Third Floor](#third-floor-ip)
  4. [Fourth Floor](#fourth-floor-ip)
- [Router & l3 Switch](#between-the-router-and-layer-3-switch)
- [Commands](#commands)
  - [Basic Configuration of the Devices](#basic-configuration-of-the-devices)
    1. [Access Layer (Switch Commands)](#access-layer-switch-commands)
    2. [Distribution Layer 3 (Switch Commands)](#distribution-layer-3-switch-commands)
  - [Core Layer (Router Commands)](#core-layer-router-commands)
  - [VLAN Access and Trunk, Switchport Security](#vlan-access-and-trunk--switchport-security)
    - [Trunk Port](#trunk-port)
    - [Access Port](#access-port)
    - [Switch Port Security](#switch-port-security)
    - [L3 Switch Port Trunk](#l3-switch-port-trunk)
  - [Assigning IP to L3 Switch](#assigning-ip-to-l3-switch)
  - [Router IP Assigning](#router-ip-assigning)
  - [OSPF on the Routers and L3 Switches](#ospf-on-the-routers-and-l3-switches)
  - [OSPF L3 Switch](#ospf-l3-switch)
  - [Inter-VLAN Routing on the L3 Switches plus IP DHCP Helper Addresses](#inter-vlan-routing-on-the-l3-switches-plus-ip-dhcp-helper-addresses)
  - [Server DHCP Commands](#server-dhcp-commands)




### First Floor
| **No.** | **Department**   | **No. of PCs** | **No. of Printers** |
|---------|-------------------|----------------|---------------------|
| 1       | Management        | 20             | 4                   |
| 2       | Research          | 20             | 4                   |
| 3       | Human Resource    | 20             | 4                   |

### Second Floor
| **No.** | **Department**   | **No. of PCs** | **No. of Printers** |
|---------|-------------------|----------------|---------------------|
| 1       | Marketing         | 20             | 4                   |
| 2       | Accounting        | 20             | 4                   |
| 3       | Finance           | 20             | 4                   |


### Third Floor
| **No.** | **Department**         | **No. of PCs** | **No. of Printers** |
|---------|-------------------------|----------------|---------------------|
| 1       | Logistics and Store     | 20             | 4                   |
| 2       | Customer Care           | 20             | 4                   |
| 3       | Guest Area              | 40             | 2                   |

### Fourth Floor
| **No.** | **Department**          | **No. of PCs** | **No. of Printers** | **No. of Servers**         |
|---------|--------------------------|----------------|---------------------|----------------------------|
| 1       | Administration           | 20             | 2                   | -                          |
| 2       | Cybersecurity                      | 20             | 2                   | -                          |
| 3       | Server Room              | 2 (Admin PCs)  | -                   | 3 (DHCP, HTTP, and Email) |



### Requirements:

1. Use a software modeling tool to visualize the network topology (consider requirement 3)
   - Software Modelling Tools: MS Visio, Visual Paradigm, or Draw.io for modeling network design.

2. Use any of the following network simulation software to implement the above topology:
   - Simulation software: Cisco Packet tracer or GNS3 for design and implementation.
   - There should be one router on each floor. The router should be connecting switches on that floor.
   - Use OSPF as the routing protocol to advertise routes.
   - Each department is required to have a wireless network for the users.
   - Each department except the server room will be anticipated to have around 60 users both wired and wireless users.
   - Host devices in the network are required to obtain IPv4 addresses automatically.
   - Devices in all the departments are required to communicate with each other.
   - All devices in the network are expected to obtain an IP address dynamically from the dedicated DHCP servers located at the server room.
   - Create HTTP, and E-mail servers
   - Configure SSH in all the routers for remote login.

3. Use hierarchical network design with redundancy included:
   - Having core, distribution, and access layers.

4. Configure the basic configuration of the devices:
   - Hostnames
   - Line Console and VTY passwords
   - Banner messages
   - Disable domain IP lookup

5. Each department should be in a different VLAN
   - Create VLANs in every department
   - VLANs you will use in your case, including VLAN1 also e.g. 10, 20, 30... etc.
   - Each VLAN should be a different subnetwork.

6. Planning of IP Addresses:
   - You have been given 192.168.10.0 as the base address for this network.
   - Do subnetting based on the number of hosts in every department as provided above.
   - Identify subnet mask, useable IP address range, and broadcast address for each subnet.

7. End Device Configurations:
   - Configure all the end devices in the network with the appropriate IP address based on the calculations above.

8. Configure port-security:
   - Use sticky command to obtain MAC Address.
   - Violation mode of the shutdown.

9. Test Communication:
   - Do devices in the same VLAN communicate
   - Do the devices in different VLANs communicate

10. Document the project design and implementation


### CONFIG STEPS 

1. Basic settings to all devices plus ssh on the routers and l3 switches.
2. VLANs assignment plus all access and trunk ports.
3. Switchport security to all l2 switches.
4. Subnetting and IP addressing
5. OSPF on the routers and l3 switches.
6. Static IP address to serverRoom devices.
7. DHCP server device configuratiuons.
8. Inter-VLAN routing on the l3 switches plus ip dhcp helper addresses.
9. Wireless network configurations.
10. Verifying and testing configurations.


---

# Subnetting calculation



Let us walk through the subnetting process for one department step by step. I will take the Management department as an example.

### Step 1: New Subnet Mask

The subnet mask is `255.255.255.255`, but it is actually a **host-only mask** (for a network with only one IP address). If you're subnetting, you'll normally employ a subnet mask that allows numerous hosts on the network. 

For example, suppose the department (Management) need a subnet with many IP addresses. We can use a subnet mask like `255.255.255.0` (which has up to 254 valid IP addresses).


So, **let's use a subnet mask of `255.255.255.0` (or `/24` in CIDR notation)** for this example.


The subnet mask `255.255.255.0` in binary is:

- `255.255.255.0` = **11111111.11111111.11111111.00000000**

- **new subnet mask** - `255.255.255.192` = **11111111.11111111.11111111.11000000**

The first 24 bits (the "1"s) are for the network part, and the last 8 bits (the "0"s) are for the host part of the address.

  
- **(The "1"s)** represent `on bits`, which are used to identify the **network** or **subnet** portion of the address.
- **(The "0"s)** represent `off bits`, which are used to identify the **host** portion of the address.

This subnet mask allows for only one network address 
- 1 Network Address (the first address)
- 1 Broadcast Address (the last address in the range)
- 254 Usable IP addresses in between.


### Step 2: Number of Networks / Subnets
For a /26 subnet mask, the first 26 bits are used for the network portion. Therefore, the subnet mask is:

```
11111111.11111111.11111111.11000000
```

$(2^n)$ (on Bit)
The number of subnets created by borrowing 2 bits for the subnet is:

\[
2^2 = 4 \text{ subnets.}
\]

### Step 3: Number of Valid Hosts per Network
For each /26 subnet, there are 6 bits left for the host portion. The number of valid hosts per subnet is:
$2^h - 2$ (off-bit)
\[
(2^6) - 2 = 64 - 2 = 62
\]

So, there are 62 valid hosts per network.


### Step 4: Jumping Values

**Total Network - New Subnet Mask**

\[
256 - 192 = 64 \text{ subnets created.}
\]

The block size, or the jump value, for a /26 subnet is 64 (256 - 192 = 64).

### Step 5: Subnetting Table
Let's create the subnetting table for the Management department:


| Subnet Number | Network Address | Subnet Mask | First IP          | Last IP           | Broadcast Address |
|---------------|------------------|-------------|-------------------|-------------------|-------------------|
| Subnet 0      | 192.168.10.0     | 255.255.255.192 (/26) | 192.168.10.1      | 192.168.10.62     | 192.168.10.63     |
| Subnet 1      | 192.168.10.64    | 255.255.255.192 (/26) | 192.168.10.65     | 192.168.10.126    | 192.168.10.127    |
| Subnet 2      | 192.168.10.128   | 255.255.255.192 (/26) | 192.168.10.129    | 192.168.10.190    | 192.168.10.191    |
| Subnet 3      | 192.168.10.192   | 255.255.255.192 (/26) | 192.168.10.193    | 192.168.10.254    | 192.168.10.255    |


I hope this helps! your learning 😊  
If you need any further assistance, feel free to ask. I'm here to help! ✨

---

## IP Addressing

**Base Network: 192.168.10.0**


#### First Floor Ip

| Department | Network Address | Subnet Mask | Host Address Range | Broadcast Address |
|------------|------------------|-------------|--------------------|-------------------|
| Management | 192.168.10.0     | 255.255.255.192/26 | 192.168.10.1 to 192.168.10.62 | 192.168.10.63 |
| Research   | 192.168.10.64    | 255.255.255.192/26 | 192.168.10.65 to 192.168.10.126 | 192.168.10.127 |
| Human Resource  | 192.168.10.128   | 255.255.255.192/26 | 192.168.10.129 to 192.168.10.190 | 192.168.10.191 |

#### Second Floor Ip

| Department | Network Address | Subnet Mask | Host Address Range | Broadcast Address |
|------------|------------------|-------------|--------------------|-------------------|
| Marketing  | 192.168.10.192   | 255.255.255.192/26 | 192.168.10.193 to 192.168.10.254 | 192.168.10.255 |
| Accounts   | 192.168.11.0     | 255.255.255.192/26 | 192.168.11.1 to 192.168.11.62 | 192.168.11.63 |
| Finance    | 192.168.11.64    | 255.255.255.192/26 | 192.168.11.65 to 192.168.11.126 | 192.168.11.127 |



### Third Floor IP

| Department | Network Address | Subnet Mask       | Host Address Range                | Broadcast Address  |
|------------|-----------------|-------------------|-----------------------------------|--------------------|
| Logistics  | 192.168.11.128  | 255.255.255.192/26 | 192.168.11.129 to 192.168.11.190  | 192.168.11.191     |
| Customer   | 192.168.11.192  | 255.255.255.192/26 | 192.168.11.193 to 192.168.11.254  | 192.168.11.255     |
| Guest      | 192.168.12.0    | 255.255.255.192/26 | 192.168.12.1 to 192.168.12.62     | 192.168.12.63      |

### Fourth Floor Ip

| Department | Network Address | Subnet Mask       | Host Address Range                | Broadcast Address  |
|------------|-----------------|-------------------|-----------------------------------|--------------------|
| Admin      | 192.168.12.64   | 255.255.255.192/26 | 192.168.12.65 to 192.168.12.126   | 192.168.12.127     |
| ICT        | 192.168.12.128  | 255.255.255.192/26 | 192.168.12.129 to 192.168.12.190  | 192.168.12.191     |
| ServerRoom | 192.168.12.192  | 255.255.255.192/26 | 192.168.12.193 to 192.168.12.254  | 192.168.12.255     |

---

## Between the Router and Layer 3 Switch


##### Base Network Address: 10.10.10.00`

##### Base Subnet Address: 255.255.255.252 /300`



| No. | Network Address | Subnet Mask      | Host Address Range       | Broadcast Address |
|-----|-----------------|------------------|--------------------------|-------------------|
| 1   | 10.10.10.0      | 255.255.255.252  | 10.10.10.33 to 10.10.10.34 | 10.10.10.35       |
| 2   | 10.10.10.4      | 255.255.255.252  | 10.10.10.37 to 10.10.10.38 | 10.10.10.39       |
| 3   | 10.10.10.8      | 255.255.255.252  | 10.10.10.41 to 10.10.10.42 | 10.10.10.43       |
| 4   | 10.10.10.12     | 255.255.255.252  | 10.10.10.45 to 10.10.10.46 | 10.10.10.47       |
| 5   | 10.10.10.16     | 255.255.255.252  | 10.10.10.49 to 10.10.10.50 | 10.10.10.51       |
| 6   | 10.10.10.20     | 255.255.255.252  | 10.10.10.53 to 10.10.10.54 | 10.10.10.55       |
| 7   | 10.10.10.24     | 255.255.255.252  | 10.10.10.33 to 10.10.10.34 | 10.10.10.35       |
| 8   | 10.10.10.28     | 255.255.255.252  | 10.10.10.37 to 10.10.10.38 | 10.10.10.39       |
| 9   | 10.10.10.32     | 255.255.255.252  | 10.10.10.41 to 10.10.10.42 | 10.10.10.43       |
| 10  | 10.10.10.36     | 255.255.255.252  | 10.10.10.45 to 10.10.10.46 | 10.10.10.47       |
| 11  | 10.10.10.40     | 255.255.255.252  | 10.10.10.49 to 10.10.10.50 | 10.10.10.51       |
| 12  | 10.10.10.44     | 255.255.255.252  | 10.10.10.53 to 10.10.10.54 | 10.10.10.55       |
| 13  | 10.10.10.48     | 255.255.255.252  | 10.10.10.33 to 10.10.10.34 | 10.10.10.35       |
| 14  | 10.10.10.52     | 255.255.255.252  | 10.10.10.37 to 10.10.10.38 | 10.10.10.39       |


---

# commands


#### Configure the basic configuration of the devices 

##### Access layer  switch commands
```
en
conf t
hostname floor1-magement-sw
banner motd  # This is floor 1 management switch#
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
##### distribution layer 3 switch commands
```
en
conf t
hostname floor2-l3-sw
banner motd  # This is floor 2 layer 3 switch#
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

#### Core  Router commands
```
en
conf t
hostname Router1-floor1
banner motd  # This is Router1 floor 1#
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

#### vlan access and trunk , switchport security

##### trunk port
```
en
conf t
int range fa0/1-2
switchport mode trunk
ex
```
##### access port
```
vlan 100
name management
exit
```
```
int range fa0/3-24
switchport mode access 
switchport access vlan 100
```
##### switch port security
```
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation shutdown
do wr
exit
```
##### l3 switch port trunk
```
int range gigabitEthernet 1/0/3-8
switchport mode trunk
ex
```

#### assigning ip to l3 switch
```
int range gigabitEthernet 1/0/1-2
no switchport
exit
```
```
int gig1/0/1
ip address 10.10.10.1 255.255.255.252

int gig1/0/2
ip address 10.10.10.9 255.255.255.252
do wr
```

#### Router Ip assigning 
```
int serial 0/1/0
ip address 10.10.10.33 255.255.255.252
```

#### OSPF on the routers and l3 switches

##### floor1 router 
```
conf t
router ospf 10
network 10.10.10.0 0.0.0.3 area 0
network 10.10.10.4  0.0.0.3  area 0
network 10.10.10.16  0.0.0.3 area 0
network 10.10.10.28  0.0.0.3 area 0
network 10.10.10.32  0.0.0.3 area 0
ex
do wr
```

##### floor 3 router
```
conf t
router ospf 10
network 10.10.10.32  0.0.0.3  area 0
network 10.10.10.40  0.0.0.3  area 0
network 10.10.10.20  0.0.0.3 area 0
network 10.10.10.48  0.0.0.3 area 0
network 10.10.10.36  0.0.0.3 area 0
ex
do wr
```

##### floor 2 router 
```
conf t
router ospf 10
network 10.10.10.16  0.0.0.3  area 0
network 10.10.10.8  0.0.0.3  area 0
network 10.10.10.24  0.0.0.3 area 0
network 10.10.10.12  0.0.0.3 area 0
network 10.10.10.20  0.0.0.3 area 0
ex
do wr
```


##### floor 4 router
```
conf t
router ospf 10
network 10.10.10.24  0.0.0.3  area 0
network 10.10.10.28  0.0.0.3  area 0
network 10.10.10.36  0.0.0.3 area 0
network 10.10.10.44  0.0.0.3 area 0
network 10.10.10.52  0.0.0.3 area 0
ex
do wr
```

#### OSPF L3 switch
```
conf t
ip routing
router ospf 10
network 10.10.10.0  0.0.0.3  area 0
network 10.10.10.8  0.0.0.3  area 0
```
```
network 192.168.10.0  0.0.0.63  area 0
network 192.168.10.64  0.0.0.63  area 0
network 192.168.10.128  0.0.0.63  area 0
network 192.168.10.192  0.0.0.63  area 0
network 192.168.11.0  0.0.0.63  area 0
network 192.168.11.64  0.0.0.63  area 0
do wr
```


#### Inter-VLAN routing on the l3 switches + ip dhcp helper addresses

##### l3 switch commands
```
conf t
vlan 300
vlan 310
vlan 320
vlan 400
vlan 410
vlan 420
```
```
int vlan 300
no shutdown
ip add 192.168.11.129 255.255.255.192
ip helper-address 192.168.12.196
exit


int vlan 310
no shutdown
ip add 192.168.11.193 255.255.255.192
ip helper-address 192.168.12.196
exit

int vlan 320
no shutdown
ip add 192.168.12.1 255.255.255.192
ip helper-address 192.168.12.196
exit


int vlan 400
no shutdown
ip add 192.168.12.65 255.255.255.192
ip helper-address 192.168.12.196
exit

int vlan 410
no shutdown
ip add 192.168.12.129 255.255.255.192
ip helper-address 192.168.12.196
exit

int vlan 420
no shutdown
ip add 192.168.12.193 255.255.255.192
exit
do wr
```


#### server DHCP commands
```
enable
configure terminal
ip dhcp pool MyPool
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
dns-server 8.8.8.8
address 192.168.1.10 192.168.1.50
max-lease 50
exit
show ip dhcp pool
```
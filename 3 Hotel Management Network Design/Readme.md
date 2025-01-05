# Modern Hotel Network Design

- [Task](#task)
- [solutions of commands](#solutions-of-commands)
  - [Router commands](#router-commands)
      - [Router VLAN configuration](#router-vlan-configuration)
     - [Router IP configuration ](#ip-configuration-on-router)
    - [Router DHCP Configurations](#router-dhcp-commands)
    - [Router OSPF Configurations](#router-ospf-configuration)
  - [switch command ](#switch-commands)
  - [Ssh command ](#ssh-configuration-on-router)
     - [switch security ](#ssh-security-on-switchport)





## Task:

As a part of your end year networking project, you are required to design and implement Vic Modern Hotel network. The hotel has three floors; in the first floor there three departments (Reception, store and Logistics), in the second floor there are three departments (Finance, HR and Sales/Marketing), while the third floor hosts the IT and Admin. Therefore, the following are part of the considerations during the design and implementation.

1. There should be three routers connecting each floor (all placed in the server room in IT department).
2. All routers should be connected to each other using serial DCE cable.
3. The network between the routers should be 10.10.10.0/30, 10.10.10.4/30, 10.10.10.8/30.
4. Each floor is expected to have one switch (placed in the respective floor).
5. Each floor is expected to have WIFI networks connected to laptops and phones.
6. Each department is expected to have a printer.
7. Each department is expected to be in different VLAN with the following details:

---
### 1st Floor;
- Reception - VLAN 80, Network of 192.168.8.0/24
- Store - VLAN 70, Network of 192.168.7.0/24
- Logistics - VLAN 60, Network of 192.168.6.0/24

### 2nd Floor;
- Finance - VLAN 50, Network of 192.168.5.0/24
- HR - VLAN 40, Network of 192.168.4.0/24
- Sales - VLAN 30, Network of 192.168.3.0/24

### 3rd Floor;
- Admin - VLAN 20, Network of 192.168.2.0/24
- IT - VLAN 10, Network of 192.168.1.0/24
- CyberSecurity - VLAN 90, Network of 192.168.10.0/24

---
8. Use OSPF as the routing protocol to advertise routes.
9. All devices in the network are expected to obtain IP address dynamically with their respective router configured as the DHCP server.
10. All the devices in the network are expected to communicate with each other.
11. Configure SSH in all the routers for remote login.
12. In IT department, add PC called Test-PC to port fa0/1 and use it to test remote login.
13. Configure port security to IT-dept switch to allow only Test-PC to access port fa0/1 (use sticky method to obtain mac-address with violation mode of shutdown.)

---


### solutions of commands





### Router Commands

```plaintext
en
conf t
interface serial 0/0/1
no shutdown
int se0/0/0
no shutdown
int gig0/0
no shutdown
int se0/0/1
clock rate 64000
int se0/0/0
clock rate 64000
do wr
```

---

### Switch Commands

```plaintext
en
conf t
int range fa0/1-2
no shutdown
switchport mode access
switchport access vlan 80
do wr

int gig0/1
switchport mode trunk
do wr

show vlan brief
```

---

### IP Configuration on Router

#### Floor 1

```plaintext
int se0/0/1
ip address 10.10.10.5 255.255.255.252

int se0/0/0
ip address 10.10.10.9 255.255.255.252
do wr
```

#### Floor 2

```plaintext
int se0/0/0
ip address 10.10.10.1 255.255.255.252

int se0/0/1
ip address 10.10.10.10 255.255.255.252
do wr
```

#### Floor 3

```plaintext
int se0/0/1
ip address 10.10.10.6 255.255.255.252

int se0/0/0
ip address 10.10.10.2 255.255.255.252
do wr
```

---

### Router VLAN Configuration

```plaintext
int gigabitEthernet 0/0.80
encapsulation dot1Q 80
ip address 102.168.8.1 255.255.255.0
ex
```

---

### Router DHCP Configuration

```plaintext
service dhcp
ip dhcp pool Finance
network 192.168.5.0 255.255.255.0
default-router 192.168.5.1
dns-server 192.168.5.1
ex
```

---

### Router OSPF Configuration

#### First Router

```plaintext
router ospf 10
network 10.10.10.4 255.255.255.252 area 0
network 10.10.10.8 255.255.255.252 area 0
network 192.168.8.0 255.255.255.0 area 0
network 192.168.7.0 255.255.255.0 area 0
network 192.168.6.0 255.255.255.0 area 0
do wr
```

#### Second Router

```plaintext
router ospf 10
network 10.10.10.0 255.255.255.252 area 0
network 10.10.10.8 255.255.255.252 area 0
network 192.168.3.0 255.255.255.0 area 0
network 192.168.4.0 255.255.255.0 area 0
network 192.168.5.0 255.255.255.0 area 0
do wr
```

#### Third Router

```plaintext
router ospf 10
network 10.10.10.0 255.255.255.252 area 0
network 10.10.10.4 255.255.255.252 area 0
network 192.168.2.0 255.255.255.0 area 0
network 192.168.1.0 255.255.255.0 area 0
network 192.168.10.0 255.255.255.0 area 0
do wr
```

---

### SSH Configuration on Router

```plaintext
en
conf t
hostname f1-router
ip domain-name f1
username floor1 password floor1@#123
crypto key generate rsa
1024

line vty 0 15
login local
transport input ssh
do wr
```

---

### SSH Security on Switchport

```plaintext
en
conf t
int fa0/3
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation shutdown
do wr
```

---

### Show Startup Configuration

```plaintext
do sh start
```

--- 
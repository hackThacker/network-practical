# Campus Network Design

# Albion University Network Design

Albion University is a large university which has two campuses situated 20 miles apart. The university's students and staff are distributed in 4 faculties; these include the faculties of Health and Sciences, Business, CyberSecurity  Engineering/Computing, and Art/Design. Each member of staff has a PC, and students have access to PCs in the labs.



- [Task](#requirements)
- [Solutions of Commands](#solutions-of-commands)
    - [Main Router Configuration](#main-campus-router-commands)
        - [Router Interface Configuration](#interface-configurations)
        - [Router DHCP Configurations](#dhcp-pool-configuration)
        - [Router RIP Configurations](#router-rip-configuration)
- [Switch Commands](#main-campus-switch-commands)
    - [Multi-Switch Campus Commands](#switch-interface-configurations)
    - [Building Switch Commands](#building-bulk-interface-configuration)


## Requirements:

1. **Create a network topology with the main components to support the following:**

#### Main campus:

- **Building A**: Administrative staff in the departments of management, HR, and finance. The admin staff PCs are distributed in the building offices, and it is expected that they will share some networking equipment (Hint: use of VLANs is expected here). The Faculty of Business is also situated in this building.

- **Building B**: Faculty of Engineering and Computing and Faculty of Art and Design.

- **Building C**: Faculty of Cybersecurity 

- **Building D**: Students’ labs and IT department. The IT department hosts the University Web server and other servers.

- There is also an email server hosted externally on the cloud.

#### Smaller campus:

- **Faculty of Health and Sciences** (staff and students’ labs are situated on separate floors).

2.  **You will be expected to configure the core devices and few end devices to provide end-to-end connectivity and access to the internal servers and the external server.**

- Each department/faculty is expected to be on its own separate IP network.
- The switches should be configured with appropriate VLANs and security settings.
- **RIPv2** will be used to provide routing for the routers in the internal network and static routing for the external server.
- The devices in building A will be expected to acquire dynamic IP addresses from a router-based DHCP server.


#### Tasks:

#### Task 1:
Your task is to plan, design, and prototype the network topology for Albion University’s network using Cisco Packet Tracer. Formative feedback will be given on this task in week 6.

#### Task 2:
Configure in Packet Tracer the network with appropriate settings to achieve the connectivity and functionalities specified in the requirements.

#### Task 3:
Produce a report (max 1500 words) including evaluation of your proposed network design and critical appraisal of your work. Your evaluation should include performance, scalability, reliability, and security of your proposed network.


### solutions of commands


# Main campus Router Commands

---
### Interface Configurations

```plaintext

interface Serial0/0/0
ip address 10.10.10.5 255.255.255.252
clock rate 64000

interface Serial0/0/1
ip address 10.10.10.1 255.255.255.252
clock rate 64000
no shutdown
do wr
en
```
```
interface GigabitEthernet0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0


interface GigabitEthernet0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
do wr
en
```



# DHCP Pool Configuration

```plaintext
ip dhcp pool Admin
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 192.168.10.1
do wr
```



### Router RIP Configuration

```plaintext
router rip
version 2
network 10.0.0.0
network 192.168.10.0
do wr
```
---


# Main Campus Switch Commands

### Switch Interface Configurations

```plaintext
interface GigabitEthernet1/0/1
switchport trunk allowed vlan 10,20,30,40,50,60,70,80,90
switchport mode trunk

interface GigabitEthernet1/0/2
switchport access vlan 10
switchport mode access

 do wr
 en

```


###  Building Bulk Interface Configuration 

```plaintext
interface range fastEthernet 0/1-24
switchport access vlan 10
switchport mode access
do wr
en
```

---

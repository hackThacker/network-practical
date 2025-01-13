# Company Network Design


A trading floor Support centre employs **600 staff**. They have recently expanded and as a result, need to move to a new building. A building has been identified but has no network. This means that before they can make the move out, a new network service needs to be designed and implemented in the new building. The existing network comprises the following elements:

The new building is expected to have three floors with two departments on each, for example:
1. **First floor** (Sales and Marketing Department - 120 users expected, Human Resource and Logistics Department - 120 users expected)
2. **Second floor** (Finance and Accounts Department - 120 users expected, Administrator and Public Relations Department - 120 users expected)
3. **Third floor** (Cybersecurity - 120 users expected, Server Room - 12 devices expected)

Therefore, as a key member of the Networks Team, you have been tasked with designing a network for the new building. At this stage, a logical design is required, which shows the measures you would put in place to ensure that the new network meets the current business needs and is future-proofed.
<!-- TOC -->

- [Company Network Design](#company-network-design)
- [Requirements](#requirements)
- [config steps](#config-steps)
    - [IP Addressing](#ip-addressing)
            - [First Floor](#first-floor)
            - [Second Floor](#second-floor)
            - [Third Floor](#third-floor)
            - [Between the Routers and Layer-3 Switches](#between-the-routers-and-layer-3-switches)
            - [Between the Routers and Layer-3 Switches](#between-the-routers-and-layer-3-switches)
- [commands](#commands)
            - [nat](#nat)
- [acces list](#acces-list)

<!-- /TOC -->


# Requirements

1. Use Cisco Packet Tracer to design and implement the network solution.
2. Use hierarchical model providing redundancy at every layer i.e. two routers and two multilayer switches are expected to be used to provide redundancy.
3. The network is also expected to connect to at least two ISPs to provide redundancy and each router to be connected to the two ISPs.
4. Each department is required to have a wireless network for the users.
5. Each department should be in a different VLAN and in different subnetwork.
6. Provided a base network of 172.16.1.0, carry out subnetting to allocate the correct number of IP addresses to each department.
7. The company network is connected to the static, public IP addresses (Internet Protocol) 195.136.17.0/30, 195.136.17.4/30, 195.136.17.8/30 and 195.136.17.12/30 connected to the two Internet providers.
8. Configure basic device settings such as hostnames, console password, enable password, banner messages, disable IP domain lookup.
9. Devices in all the departments are required to communicate with each other with the respective multilayer switch configured for inter-VLAN routing.
10. The Multilayer switches are expected to carry out both routing and switching functionalities thus will be assigned IP addresses.
11. All devices in the network are expected to obtain an IP address dynamically from the dedicated DHCP servers located at the server room.
12. Devices in the server room are to be allocated IP addresses statically.
13. **Use OSPF Routing:** Use OSPF as the routing protocol to advertise routes both on the routers and multilayer switches.
14. **SSH Configuration:** Configure SSH on all routers and layer three switches for remote login.
15. **Port Security:** Configure port-security for the Finance and Accounts department to allow only one device to connect to a switchport, use the sticky method to obtain the mac-address, and set violation mode to shutdown.
16. **PAT Configuration:** Configure PAT to use the respective outbound router interface IPv4 address and implement the necessary ACL rule.
17. **Test Communication:** Ensure that everything configured is working as expected.


# config steps
1. Basic settings to all devices plus ssh on the routers and l3 switch. 
2. VLANs assignment plus all access and trunk ports on l2 and l3 switch. 
3. Switchport security to finance department. 
4. Subnetting and IP addressing 
5. OSPF on the routers and l3 switches. 
6. Static IP address to serverRoom devices. 
7. DHCP server device configuratiuons. 
8. Inter-VLAN routing on the l3 switches plus ip dhcp helper addresses. 
9. Wireless network configurations. 
10. PAT + Access Control List 
11. Verifying and testing configurations. 



## IP Addressing
**Base Network: 172.16.1.0**

#### First Floor
| Department         | Network Address | Subnet Mask         | Host Address Range            | Broadcast Address |
|--------------------|-----------------|---------------------|-------------------------------|-------------------|
| Sales & Marketing  | 172.16.1.0      | 255.255.255.128/25  | 172.16.1.1 to 172.16.1.126    | 172.16.1.127      |
| HR and Logistics   | 172.16.1.128    | 255.255.255.128/25  | 172.16.1.129 to 172.16.1.254  | 172.16.1.255      |

#### Second Floor
| Department                | Network Address | Subnet Mask         | Host Address Range            | Broadcast Address |
|---------------------------|-----------------|---------------------|-------------------------------|-------------------|
| Finance & Accounts        | 172.16.2.0      | 255.255.255.128/25  | 172.16.2.1 to 172.16.2.126    | 172.16.2.127      |
| Admin & Public Relations  | 172.16.2.128    | 255.255.255.128/25  | 172.16.2.129 to 172.16.2.254  | 172.16.2.255      |


#### Third Floor
| Department  | Network Address | Subnet Mask         | Host Address Range            | Broadcast Address |
|-------------|-----------------|---------------------|-------------------------------|-------------------|
| ICT         | 172.16.3.0      | 255.255.255.128/25  | 172.16.3.1 to 172.16.3.126    | 172.16.3.127      |
| Server Room | 172.16.3.128    | 255.255.255.240/28  | 172.16.3.129 to 172.16.1.142  | 172.16.3.143      |

#### Between the Routers and Layer-3 Switches
| No.         | Network Address | Subnet Mask         | Host Address Range            | Broadcast Address |
|-------------|-----------------|---------------------|-------------------------------|-------------------|
| R1- MLSW1   | 172.16.3.144    | 255.255.255.252     | 172.16.3.145 to 172.16.3.146  | 172.16.3.147      |
| R1- MLSW2   | 172.16.3.148    | 255.255.255.252     | 172.16.3.149 to 172.16.3.150  | 172.16.3.151      |
| R2- MLSW1   | 172.16.3.152    | 255.255.255.252     | 172.16.3.153 to 172.16.3.154  | 172.16.3.155      |
| R2- MLSW2   | 172.16.3.156    | 255.255.255.252     | 172.16.3.157 to 172.16.3.158  | 172.16.3.159      |

#### Between the Routers and Layer-3 Switches
Public IP addresses 195.136.17.0/30, 195.136.17.4/30, 195.136.17.8/30 and 195.136.17.12/30


# commands

other commands same as Banking Network design
#### nat

```
ip nat inside source list 1 int ser 0/0/0 overload
```

# acces list
```
access-list 1 permit 172.16.1.0 0.0.0.127 
access-list 1 permit 172.16.1.128 0.0.0.127 
access-list 1 permit 172.16.2.0 0.0.0.127 
access-list 1 permit 172.16.2.128 0.0.0.127 
access-list 1 permit 172.16.3.0 0.0.0.127 
access-list 1 permit 172.16.3.128 0.0.0.15
```
```
int range gig0/0-1
ip nat inside 
```
```
int ser0/0/0
ip nat outside 
int ser0/0/1
ip nat outside
```
```
ip route 0.0.0.0 0.0.0.0 ser0/0/0
ip route 0.0.0.0 0.0.0.0 ser0/0/1 70 
do wr

ip route 0.0.0.0 0.0.0.0 gig1/0/1
ip route 0.0.0.0 0.0.0.0 gig1/0/2 70
do wr
```
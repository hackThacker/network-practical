# Banking Network Design


Nebula Financial Services is a UK-owned company that deals with Banking and Insurance. The company is intending to expand its services across the Asian continent, having the first branch located in Hetauda, Nepal. The company has secured a four-story building to operate within the kathmandu capital city. Therefore, the company would like to allow sourcing the knowledge from a group of final-year students from the local university to design and implement their company network. Assume you are among the students to take over this role. Carefully read down the requirements, then model the design and implement the network based on the company's needs. Each floor has departments as provided in the table below.

- [Task](#requirements)
- [Calculating Subnetting Tables ](#subnetting-calculation)
- [IP Table Addressing](#ip-addressing)
  - [First Floor](#first-floor-ip)
  - [Second Floor](#second-floor-ip)
  - [Third Floor](#third-floor-ip)
  - [Fourth Floor](#fourth-floor-ip)
- [Router & l3 Switch](#between-the-router-and-layer-3-switch)


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


##### Base Network Address: `10.10.10.0`
##### Base Subnet Address: `255.255.255.252 /30`



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




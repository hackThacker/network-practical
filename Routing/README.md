### Network Topology and Configuration Guide

This document provides a detailed explanation of the network topology represented in the image, along with configuration steps for each device. This guide is well structured to help you understand the network setup and the configurations involved.

---

## 1. **Overview**

The network topology consists of three main routers, multiple subnets, and several connected devices (laptops). The routers are configured using RIP (Routing Information Protocol) to manage routing between different networks. The network is divided into several subnets, each identified by a specific network ID.

### **Network Components:**
- **Routers:** Three routers (ISR4331, Router1, Router2) with different interface configurations.
- **Switches:** Two switches to manage connections between laptops and routers.
- **Laptops:** Four laptops connected to different subnets.

---

## 2. **Router Configurations**

### **Router 1 Configuration (Huawei Router 1 - Sohan Pvt Ltd)**
This router connects to two networks and has the following configurations:
- **Interface Se0/1/1:**
  - IP Address: `192.168.6.1`
  - Subnet Mask: `255.255.255.0`
- **Gig0/0/1:**
  - Connected to the network `192.168.1.0/24`
- **Routing Protocol:**
  - RIP version 2 is enabled, and it advertises the following networks:
    - `192.168.1.0`
    - `192.168.2.0`
    - `192.168.6.0`

### **Router 2 Configuration (Huawei Router 2 - Anuj Pvt Ltd)**
This router is similarly configured to manage multiple networks:
- **Interface Se0/1/0:**
  - IP Address: `192.168.5.2`
  - Subnet Mask: `255.255.255.0`
- **Gig0/0/1:**
  - Connected to the network `192.168.0.0/24`
- **Routing Protocol:**
  - RIP version 2 is enabled, and it advertises the following networks:
    - `192.168.0.0`
    - `192.168.3.0`
    - `192.168.5.0`

### **Router 3 Configuration (ISR4331)**
This router serves as a backbone for connecting different subnets:
- **Interfaces:**
  - **Se0/1/0:**
    - IP Address: `192.168.2.2`
  - **Se0/1/1:**
    - IP Address: `192.168.3.1`
  - **Gig0/0/0:**
    - IP Address: `192.168.4.1`
- **Routing Protocol:**
  - RIP version 2 is used to manage routing between connected subnets.

---

## 3. **IP Addressing Scheme**

The network is divided into different subnets as follows:
- **Subnet 1:** `192.168.1.0/24`
  - Connected Devices: Laptops with IP addresses `192.168.1.2` and `192.168.1.3`
- **Subnet 2:** `192.168.2.0/24`
  - Connected to Router 1 (Se0/1/0)
- **Subnet 3:** `192.168.3.0/24`
  - Connected to Router 2 (Se0/1/1)
- **Subnet 4:** `192.168.4.0/24`
  - Backbone connection managed by Router 3
- **Subnet 5:** `192.168.5.0/24`
  - Connected to Router 2 (Se0/1/0)
- **Subnet 6:** `192.168.6.0/24`
  - Connected to Router 1 (Se0/1/1)

---

## 4. **Routing Protocol: RIP Configuration**

RIP is configured on all routers to ensure proper routing between the subnets. The main steps to configure RIP on each router are as follows:

### **General RIP Configuration Steps:**
1. **Enable RIP:** Enter the router's configuration mode and enable RIP.
2. **Specify RIP Version:** Use version 2 to support subnet masks.
3. **Advertise Networks:** Specify the networks connected to each router that should be advertised using RIP.
4. **Disable Auto-Summary:** Ensure accurate subnet routing by disabling auto-summarization.
5. **Save Configuration:** Write the configuration to memory.

---

## 5. **Step-by-Step Configuration Guide**

### **Router 1 Configuration:**
```plaintext
enable
configure terminal
interface se0/1/1
ip address 192.168.6.1 255.255.255.0
no shutdown
exit
interface gig0/0/1
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
router rip
version 2
network 192.168.1.0
network 192.168.2.0
network 192.168.6.0
no auto-summary
exit
write
```

### **Router 2 Configuration:**
```plaintext
enable
configure terminal
interface se0/1/0
ip address 192.168.5.2 255.255.255.0
no shutdown
exit
interface gig0/0/1
ip address 192.168.0.1 255.255.255.0
no shutdown
exit
router rip
version 2
network 192.168.0.0
network 192.168.3.0
network 192.168.5.0
no auto-summary
exit
write
```

### **Router 3 Configuration:**
```plaintext
enable
configure terminal
interface se0/1/0
ip address 192.168.2.2 255.255.255.0
no shutdown
exit
interface se0/1/1
ip address 192.168.3.1 255.255.255.0
no shutdown
exit
interface gig0/0/0
ip address 192.168.4.1 255.255.255.0
no shutdown
exit
router rip
version 2
network 192.168.2.0
network 192.168.3.0
network 192.168.4.0
no auto-summary
exit
write
```

---

## 6. **Testing and Verification**

### **Verification Commands:**
- **`show ip route rip`** - To display the current RIP routing table.
- **`ping`** - Test connectivity between different subnets.
- **`traceroute`** - To trace the path packets take to a remote destination.

Ensure all devices can communicate with each other across the subnets. Verify the RIP routing tables on each router to ensure proper propagation of routes.

---

## 7. **Conclusion**

This setup demonstrates a basic implementation of routing using RIP across multiple routers and subnets. The configuration is simple yet effective for learning purposes, and the use of RIP helps in understanding dynamic routing protocols. This  provides a foundational understanding, and users can build upon this by exploring more advanced topics like OSPF or BGP for larger network environments.

---

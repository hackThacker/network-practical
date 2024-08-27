### Network Topology with Dual ISPs and Corporate Networks Using RIP

This document provides detailed explanation of the network topology represented in the image, along with configuration steps for each device. This guide is well structured to help you understand the network setup and the configurations involved.

A comprehensive guide to configuring a network topology involving two Internet Service Providers (ISPs) and two separate companies, each connected to its own network. The topology uses the Routing Information Protocol (RIP) to manage routing between different subnets. The guide is structured in a beginner-friendly manner, including detailed configuration commands.

---

## 1. **Overview**

The network consists of two companies, each connected to its own router, and two ISPs that provide external connectivity. The companies are connected to the ISPs through separate routers. The network is configured to use RIP version 2 for routing between different subnets.

### **Network Components:**
- **Companies:** 
  - **Sohan Pvt Ltd:** Connected via Huawei Router 1.
  - **Anuj Pvt Ltd:** Connected via Huawei Router 2.
- **ISPs:** 
  - **ISP 1:** World Link, connected via Router 3 (ISR4331).
  - **ISP 2:** Vianet, also connected via Router 3 (ISR4331).
- **Devices:**
  - **Laptops:** Connected to the company networks for internal use.
  - **Switches:** Used to connect multiple devices within the company networks.

---
# Network Configuration for Laptop Connectivity

This document provides an overview of network configuration for connecting laptops to Router 1 and outlines the importance of setting the default gateway for seamless communication between devices on different routers.

## Network Setup

### Router 1 Configuration

- **Network ID**: 192.168.1.0/24
- **Router IP Address**: 192.168.1.1

### Laptop Configuration For Router 1 and Router 2

Ensure that each laptop connected to Router 1 has a static IP address assigned as follows:

- **Laptop 0**
  - **IP Address**: 192.168.1.2
  - **Subnet Mask**: 255.255.255.0
  - **Default Gateway**: 192.168.1.1

- **Laptop 1**
  - **IP Address**: 192.168.1.3
  - **Subnet Mask**: 255.255.255.0
  - **Default Gateway**: 192.168.1.1

### Importance of Default Gateway

The default gateway is a critical component in network configurations. It serves as the path through which network traffic is routed when communicating with devices that are not on the same local network. 

In our setup:
- **Without a Default Gateway**: If the default gateway is not configured, laptops will not be able to communicate with devices connected to other routers (e.g., Router 2). This is because the laptops will lack the necessary routing information to send requests beyond Router 1.

- **With a Default Gateway**: Setting the default gateway to Router 1’s IP address (192.168.1.1) ensures that all outgoing traffic destined for networks outside the local subnet (192.168.1.0/24) is properly routed through Router 1. This allows seamless communication between laptops on Router 1 and devices on Router 2 or any other network.

## Summary

1. **Assign static IP addresses and subnet masks** to each laptop connected to Router 1.
2. **Configure the default gateway** on each laptop to Router 1’s IP address (192.168.1.1) to enable communication with devices on other routers.
3. **Verify network connectivity** to ensure all devices can communicate as expected.

By following these configurations, you can ensure proper network connectivity and communication between devices across different routers.

## 2. **Company Network Configurations**

### **2.1 Sohan Pvt Ltd (Huawei Router 1)**
This company’s network is connected to both its internal subnet and ISP 1 via Huawei Router 1.

#### **Interface and Network Configuration:**
- **Se0/1/1 (To ISP 1):**
  - IP Address: `192.168.6.1`
  - Subnet Mask: `255.255.255.0`
- **Gig0/0/1 (Internal Network):**
  - IP Address: `192.168.1.1`
  - Network ID: `192.168.1.0/24`

#### **RIP Configuration:**
- **Networks Advertised:**
  - `192.168.1.0/24`
  - `192.168.2.0/24`
  - `192.168.6.0/24`
  
#### **Configuration Commands:**
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

### **2.2 Anuj Pvt Ltd (Huawei Router 2)**
This company’s network is connected to its internal subnet and ISP 2 via Huawei Router 2.

#### **Interface and Network Configuration:**
- **Se0/1/0 (To ISP 2):**
  - IP Address: `192.168.5.2`
  - Subnet Mask: `255.255.255.0`
- **Gig0/0/1 (Internal Network):**
  - IP Address: `192.168.0.1`
  - Network ID: `192.168.0.0/24`

#### **RIP Configuration:**
- **Networks Advertised:**
  - `192.168.0.0/24`
  - `192.168.3.0/24`
  - `192.168.5.0/24`
  
#### **Configuration Commands:**
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

---

## 3. **ISP Configurations**

### **3.1 ISP 1 (World Link) - Router 3 (ISR4331)**
ISP 1 provides connectivity for Sohan Pvt Ltd. Router 3 is configured to handle this connection and also routes traffic to ISP 2.

#### **Interface Configuration:**
- **Se0/1/0 (To Router 1 - Sohan Pvt Ltd):**
  - IP Address: `192.168.2.2`
  - Subnet Mask: `255.255.255.0`
- **Gig0/0/0 (To ISP Backbone):**
  - IP Address: `192.168.4.1`
  - Subnet Mask: `255.255.255.0`

#### **RIP Configuration:**
- **Networks Advertised:**
  - `192.168.2.0/24`
  - `192.168.4.0/24`
  - `192.168.3.0/24`

#### **Configuration Commands:**
```plaintext
enable
configure terminal
interface se0/1/0
ip address 192.168.2.2 255.255.255.0
no shutdown
exit
interface gig0/0/0
ip address 192.168.4.1 255.255.255.0
no shutdown
exit
router rip
version 2
network 192.168.2.0
network 192.168.4.0
network 192.168.3.0
no auto-summary
exit
write
```

### **3.2 ISP 2 (Vianet) - Router 3 (ISR4331)**
ISP 2 provides connectivity for Anuj Pvt Ltd. Router 3 handles the routing between ISP 1 and ISP 2.

#### **Interface Configuration:**
- **Se0/1/1 (To Router 2 - Anuj Pvt Ltd):**
  - IP Address: `192.168.3.1`
  - Subnet Mask: `255.255.255.0`
- **Gig0/0/0 (To ISP Backbone):**
  - IP Address: `192.168.4.1`
  - Subnet Mask: `255.255.255.0`

#### **RIP Configuration:**
- **Networks Advertised:**
  - `192.168.3.0/24`
  - `192.168.4.0/24`
  - `192.168.5.0/24`

#### **Configuration Commands:**
```plaintext
enable
configure terminal
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
network 192.168.3.0
network 192.168.4.0
network 192.168.5.0
no auto-summary
exit
write
```

## 4. **IP Addressing Scheme**

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
---

## 5. **Testing and Verification**

After configuring the network, it’s essential to verify the connectivity and routing across the network.

### **Verification Commands:**
- **`show ip route rip`**: Displays the current RIP routing table on each router.
- **`ping <IP_Address>`**: Tests connectivity between different routers and devices.
- **`traceroute <IP_Address>`**: Traces the route taken by packets across the network.

Use these commands to confirm that the routing is set up correctly and that all devices can communicate as intended.

---

## 6. **Router 0 (ISP 1 - World Link) Configuration:**
```bash
router rip
 version 2
 network 192.168.2.0
 network 192.168.3.0
 network 192.168.4.0
```

### **Huawei Router 1 (Sohan Pvt Ltd) Configuration:**
```bash
router rip
 version 2
 network 192.168.1.0
 network 192.168.2.0
 network 192.168.6.0
```

### **Huawei Router 2 (Anuj Pvt Ltd) Configuration:**
```bash
router rip
 version 2
 network 192.168.3.0
 network 192.168.5.0
 network 192.168.6.0
```

### **Router 3 (ISP 2 - Vianet) Configuration:**
```bash
router rip
 version 2
 network 192.168.4.0
 network 192.168.5.0
 network 192.168.6.0
```

### **Explanation:**

1. **Router 0 (ISP 1 - World Link)**:
   - This router connects to the `192.168.2.0`, `192.168.3.0`, and `192.168.4.0` networks.
   - These networks are advertised via RIP.

2. **Huawei Router 1 (Sohan Pvt Ltd)**:
   - This router connects to the `192.168.1.0`, `192.168.2.0`, and `192.168.6.0` networks.
   - These networks are advertised via RIP.

3. **Huawei Router 2 (Anuj Pvt Ltd)**:
   - This router connects to the `192.168.3.0`, `192.168.5.0`, and `192.168.6.0` networks.
   - These networks are advertised via RIP.

4. **Router 3 (ISP 2 - Vianet)**:
   - This router connects to the `192.168.4.0`, `192.168.5.0`, and `192.168.6.0` networks.
   - These networks are advertised via RIP.

### **Steps to Configure RIP on Each Router:**

1. **Enter Global Configuration Mode:**
   - `enable`
   - `configure terminal`

2. **Enter RIP Configuration Mode:**
   - `router rip`

3. **Set RIP Version 2:**
   - `version 2`

4. **Advertise the Networks:**
   - Use the `network [network-ID]` command for each network directly connected to the router.

5. **Exit Configuration Mode:**
   - `exit`

### **Verification:**

- Use `show ip route` to verify that RIP routes are being advertised and learned correctly on each router.
- Ping across the network to ensure connectivity is functioning as expected.

If you apply these configurations and follow the troubleshooting steps from earlier, your ICMP (ping) between Huawei Router 1 and Huawei Router 2 should work, assuming there are no other underlying issues.
## 7. **Conclusion**

This configuration provides a reliable and scalable setup for a network involving two companies and dual ISPs using RIP. The network is configured for dynamic routing, which simplifies the management and scalability of the network. This guide is designed to be beginner-friendly, ensuring that even those new to networking can successfully configure and manage this network topology.

Ensure that you follow each step carefully and verify your configurations using the provided commands. This will help you to gain a solid understanding of network configuration and the RIP protocol.

---

This provides all the necessary information to replicate the network setup shown in the diagram.
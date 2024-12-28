---

# Network Topology and Configuration Guide on EIGRP

This document provides a detailed overview and configuration guide for the network topology depicted in the provided image. The network is configured using multiple routers, switches, and end devices, with routing enabled via the Enhanced Interior Gateway Routing Protocol (EIGRP).

## Overview

The network is composed of:
- **Three routers** (ISR, ISR4331, and Huawei routers as name)
- **Multiple laptops** and **switches**
- **Various interfaces** connecting different subnets.

The network setup enables communication between various subnets, with routing handled by EIGRP.

## Router Configurations

### Huawei Router 1

Huawei Router 1 is a crucial part of the network, connected to various subnets and interfaces. Below are the steps to configure this router:

#### Basic Router Setup
1. **Enable the router on privilage mode #:**
   ```bash
   enable or en
   ```

2. **Enter global configuration mode:**
   ```bash
   configure terminal or conf t
   ```

#### Interface Configuration
Configure the serial interface `Se0/1/1` to connect to the network.

3. **Configure the interface:**
   ```bash
   interface se0/1/1
   ip address 192.168.6.1 255.255.255.0
   no shutdown
   ```

4. **Exit configuration mode:**
   ```bash
   exit (from router config)
   exit (from router)
   ```

5. **Save the configuration:**
   ```bash
   write or wr
   ```

### EIGRP Configuration on Huawei Router 1

To enable routing across different networks, configure EIGRP on Huawei Router 1.

1. **Enable the router for EIGRP configuration:**
   ```bash
   enable
   ```

2. **Enter global configuration mode:**
   ```bash
   configure terminal
   ```

3. **Start EIGRP configuration:**
   ```bash
   router eigrp 1
   ```
   - Here, `1` is the Autonomous System (AS) number for all router .

4. **Set the EIGRP router ID:**
   ```bash
   eigrp router-id 1.1.1.1
   ```
   - Here, `1.1.1.1` is the router id different ip number formate for all router .

5. **Advertise the connected networks:**
   ```bash
   network 192.168.1.0
   network 192.168.2.0
   network 192.168.6.0
   ```

6. **Disable auto-summarization:**
   ```bash
   no auto-summary
   ```

7. **Exit EIGRP configuration mode and save:**
   ```bash
   end
   wr
   ```

8. **Verify the configuration:**
   - To verify EIGRP protocols:
     ```bash
     show ip protocols
     ```
   - To check the interface status:
     ```bash
     show ip interface brief or sh ip int bri
     ```

### Additional Router Configurations

Repeat similar steps for other routers  with their respective interface IPs and network IDs as shown in the topology.

## Conclusion

This README.md provides a clear and concise guide to configure the routers and set up EIGRP routing within the network. Follow these steps carefully to ensure the network is configured correctly for seamless communication between devices across different subnets.

---

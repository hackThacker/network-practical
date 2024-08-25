---

# DHCP Server and Client Lab with Router Configuration

This repository contains a practical lab exercise that demonstrates how to set up a DHCP server, configure clients to obtain IP addresses automatically, and configure a router in a network environment. This lab is ideal for those preparing for the CompTIA Network+ certification or anyone looking to gain hands-on experience with networking fundamentals.

## Lab Overview

### Network Setup:
- **Network ID**: `192.168.1.0/24`
- **Subnet Mask**: `255.255.255.0`
- **Default Gateway**: `192.168.1.1`
- **DNS Server**: `8.8.8.8`
- **DHCP Server IP**: `192.168.1.2` (Static IP)

### Devices Used:
- **Router**: ISR4331 (Router0)
- **Switch**: 2960 (Switch0)
- **Server**: Server-PT (Server0)
- **Clients**: Laptop-PT (Laptop2, Laptop5, Laptop6)

## Step-by-Step Instructions

### 1. Configuring the Router

First, we need to set up the router with the appropriate IP address.

**Commands:**
```bash
enable
configure terminal
interface Gig0/0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
end
write

**Don’t be remembered notes you take, but by the labs you practice and the work you do. True mastery comes from hands-on experience.**
```



### 2. Configuring the DHCP Server

On the server, configure the DHCP service to provide IP addresses to the clients.

1. **Access DHCP Configuration**:
   - Go to `Services` tab.
   - Select `DHCP`.
   
2. **Set the DHCP Parameters**:
   - **Default Gateway**: `192.168.1.1`
   - **DNS Server**: `8.8.8.8`
   - **IP Address Range**: From `192.168.1.3` to `192.168.1.254`

3. **Apply Settings**:
   - Ensure the DHCP server is turned on.
   - Save the settings.

### 3. Configuring the Clients

Each client device needs to be set to obtain an IP address automatically from the DHCP server.

**Steps:**
- Open the `Desktop` tab on each Laptop.
- Select `IP Configuration`.
- Choose `DHCP`.

After the setup, each client will receive an IP address from the DHCP server within the range specified.

### 4. Verification

To verify that everything is configured correctly:
1. **Ping the Router** from any client to check connectivity:
   ```bash
   ping 192.168.1.1
   ```
2. **Check IP Configuration** on clients:
   - Ensure the IP address, default gateway, and DNS server are correctly assigned.

## Key Takeaways

- **Router Configuration**: Understanding how to set up a router as a gateway for the network.
- **DHCP Server Configuration**: Automating IP address assignment to clients, reducing manual configuration errors.
- **Client Configuration**: Setting up devices to automatically receive network settings, ensuring proper connectivity.

This lab reinforces fundamental networking concepts, making it an essential practice for those pursuing networking certifications or improving their network management skills.

---
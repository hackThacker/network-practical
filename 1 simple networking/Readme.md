# Design a Network in Cisco Packet Tracer



- [Task](#task)
- [solutions of subnet](#solutions-of-subnetting)
- [solutions of commands](#solutions-of-commands )

## Task:
1. **Design a network in Cisco Packet Tracer to connect IT and HR departments through the following:**
   - Each department should contain at least 2 PCs and printer .
   - Appropriate number of switches and routers should be used in the network.
   - Using the given network address `192.168.40.0`, all interfaces should be configured with appropriate IP addresses, subnet mask, and gateways.
   - All devices in the network should be connected using appropriate cables.
   - Test the connectivity between IT department and HR department. 
     - PCs in the IT department should be able to ping the PCs in the HR department.




---
### solutions of subnetting




### Network Address: 192.168.40.0  
**No. of Subnets:** 2

#### 2^n Calculation for Subnets
- 2^n = 2  
- n = 1 (1 bit borrowed for subnetting)

#### Subnet Mask
- **New Subnet Mask**: 255.255.255.128  
- In binary: `11111111.11111111.11111111.10000000`  
- /25 subnet

---

### 1st Subnet
- **Subnet Mask**: 255.255.255.128  
- **Network ID**: 192.168.40.0  
- **Range of Valid Hosts**: 192.168.40.1 – 192.168.40.126  
- **Broadcast Address**: 192.168.40.127  
- **CIDR Notation**: /25

---

### 2nd Subnet
- **Subnet Mask**: 255.255.255.128  
- **Network ID**: 192.168.40.128  
- **Range of Valid Hosts**: 192.168.40.129 – 192.168.40.254  
- **Broadcast Address**: 192.168.40.255
- **CIDR Notation**: /25



---
### solutions of Commands


### Router


### 1. **Access Router:**
   - **Enter privileged EXEC mode** (to run advanced commands):
     ```
     enable
     ```

   - **Enter global configuration mode** (to make configuration changes):
     ```
     configure terminal
     ```



### 3. **Configure Interface IP Address:**
   - **Enter interface configuration mode** (replace `GigabitEthernet0/0` with your interface):
     ```
     interface GigabitEthernet0/0,  interface gig/0/0
     ```

   - **Set IP address and subnet mask**:
     ```
     ip address 192.168.1.1 255.255.255.0
     ```

   - **Enable the interface** (if it’s administratively down):
     ```
     no shutdown
     ```


### 4. **Save Configuration:**
   - **Save the current configuration to startup-config** (to retain changes after reboot):
     ```
     write 
     ```
     Or:
     ```
     copy running-config startup-config
     ```

### 5. **Exit Router Mode:**
   - **Exit to previous mode**:
     ```
     exit
     ```



### 6. **Ping Command (for testing network connectivity):**
   - **Ping an IP address**:
     ```
     ping 192.168.1.1
     ```

for the pc and printer you have to input the static ip address 


# VLAN Configuration Lab Part 1
## 🏗️ Overview
This repository contains a **VLAN configuration lab** for Cisco Packet Tracer, based on the CCNA 200-301 course. It covers **VLAN configuration** on a Layer 2 switch and **Inter-VLAN Routing** using a router.

### 🔧 Lab Objectives
1. **Configure IP addresses and subnet masks** on each PC.
2. **Set up VLANs** on a Layer 2 switch (SW1).
3. **Configure Inter-VLAN Routing** on a router (R1).
4. **Test network connectivity** using ICMP (ping).

## 📜 Topology
- **Router (R1):** Cisco 2911
- **Switch (SW1):** Cisco Switch-PT
- **PCs:** 6 devices, grouped into 3 VLANs
- ![image](https://github.com/user-attachments/assets/a14924d2-343b-44e6-991b-db2ca997a6a6)


| VLAN  | Network | Gateway | Devices |
|--------|------------|----------------|------------------|
| VLAN 10 (Engineering) | 10.0.0.0/26 | **10.0.0.62** | PC1, PC2 |
| VLAN 20 (HR) | 10.0.0.64/26 | **10.0.0.126** | PC3, PC4 |
| VLAN 30 (Sales) | 10.0.0.128/26 | **10.0.0.190** | PC5, PC6 |

---

## ⚙️ Configuration Commands

### **Router (R1) Configuration**
```bash
enable
configure terminal

interface GigabitEthernet0/0
 ip address 10.0.0.62 255.255.255.192
 no shutdown

interface GigabitEthernet0/1
 ip address 10.0.0.126 255.255.255.192
 no shutdown

interface GigabitEthernet0/2
 ip address 10.0.0.190 255.255.255.192
 no shutdown

end
write memory
```

### **Switch (SW1) VLAN Configuration**
```bash
enable
configure terminal

vlan 10
 name Engineering

vlan 20
 name HR

vlan 30
 name Sales

interface range GigabitEthernet0/1, FastEthernet3/1, FastEthernet4/1
 switchport mode access
 switchport access vlan 10

interface range GigabitEthernet1/1, FastEthernet5/1, FastEthernet6/1
 switchport mode access
 switchport access vlan 20

interface range GigabitEthernet2/1, FastEthernet7/1, FastEthernet8/1
 switchport mode access
 switchport access vlan 30

end
write memory
```

---

## ✅ **Ping Test Results**

### **Working Connectivity:**
- ✅ PC1 ↔ PC2 (Same VLAN)
- ✅ PC3 ↔ PC4 (Same VLAN)
- ✅ PC5 ↔ PC6 (Same VLAN)
- ✅ PC1 ↔ PC5 (Inter-VLAN via R1)
- ✅ PC2 ↔ PC6 (Inter-VLAN via R1)

### **Failed Connectivity:**
- ❌ PC1 → 10.0.0.120 (Incorrect IP/subnet issue)
- ❌ PC5 → 10.0.0.2 (Connectivity issue)

---

## 🛠️ **Troubleshooting Steps**
1. **Check VLAN Assignments**
   ```bash
   show vlan brief
   ```
2. **Verify Trunk Configuration**
   ```bash
   show interfaces trunk
   ```
3. **Check Router Interfaces**
   ```bash
   show ip interface brief
   ```
4. **Ensure PCs Have Correct Gateway**

# **🔥 Part 2: VLAN Trunking & Router-on-a-Stick**

In **Part 2**, we focus on **trunking VLANs** between multiple switches and setting up **Router-on-a-Stick** for VLAN routing.

### **🛠️ Lab Objectives (Part 2)**
✔️ Configure **trunk links** between **SW1** and **SW2**  
✔️ Enable **802.1Q encapsulation** for VLAN routing on the router  
✔️ Set up **subinterfaces** on the router  
✔️ Allow **communication between VLANs via Inter-VLAN Routing**  

---

### **📡 Network Topology (Part 2)**
![image](https://github.com/user-attachments/assets/af6c6a4b-d2b8-40e3-bf13-97286357f53e)

- **SW2** connects VLANs **10, 20, 30** to **SW1** via **trunk ports**  
- **Router-on-a-stick** is configured on **R1** to allow VLAN routing  

---

### **⚙️ Switch 2 (SW2) - Trunk Configuration**
```sh
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
 switchport trunk native vlan 1001
```

---

### **⚙️ Router (R1) - Router-on-a-Stick Configuration**
```sh
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 10.0.0.62 255.255.255.192

interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 10.0.0.126 255.255.255.192

interface GigabitEthernet0/0.30
 encapsulation dot1Q 30
 ip address 10.0.0.190 255.255.255.192
```

---

### **✅ Testing (Part 2)**
- **Check VLANs on switches:**  
  ```sh
  show vlan brief
  ```
- **Verify trunk links:**  
  ```sh
  show interfaces trunk
  ```
- **Ensure router subinterfaces are active:**  
  ```sh
  show ip interface brief
  ```
- **Ping across VLANs:**  
  ```sh
  ping 10.0.0.126
  ping 10.0.0.190
  ```

## 🚀 **How to Use**
1. **Open Cisco Packet Tracer** and load the `.pkt` file.
2. Verify configurations on the router and switch.
3. Test connectivity with `ping` from different PCs.
4. Debug any connectivity issues with `show` commands.

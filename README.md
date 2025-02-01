
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


---

## 🚀 **How to Use**
1. **Open Cisco Packet Tracer** and load the `.pkt` file.
2. Verify configurations on the router and switch.
3. Test connectivity with `ping` from different PCs.
4. Debug any connectivity issues with `show` commands.

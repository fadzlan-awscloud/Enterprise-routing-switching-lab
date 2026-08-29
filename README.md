# Enterprise Switching Lab 01 — VLANs, Access Ports & Trunking

## Objective

Build and configure a basic enterprise Layer 2 switching environment using Cisco Packet Tracer.

The lab demonstrates:

- VLAN creation
- VLAN naming
- Access-port configuration
- 802.1Q trunk configuration
- VLAN verification
- Basic switch connectivity verification

---

## Skills Demonstrated

| Skill | Description |
|---|---|
| VLAN Creation | Create and name VLANs for different departments |
| Access Ports | Assign switch ports to specific VLANs |
| Trunking | Configure trunk links between switches |
| Verification | Use Cisco IOS `show` commands to verify configuration |

---

## Vlan Design
| VLAN | Department  |
| ---: | ----------- |
|   10 | IT          |
|   20 | HR          |
|   30 | Finance     |
|   40 | Engineering |

---

## Configuration
vlan 10
name IT
exit

vlan 20
name HR
exit

vlan 30
name Finance
exit

vlan 40
name Engineering
exit

---

## Configure access Port

### Department IT
interface fa0/1
switchport mode access
switchport access vlan 10
description Link-to-PC0-IT
no shutdown
exit

### Department HR
interface fa0/2
switchport mode access
switchport access vlan 20
description Link-to-PC1-HR
no shutdown
exit

### Department Finance
interface fa0/1
switchport mode access
switchport access vlan 30
description Link-to-PC2-Finance
no shutdown
exit

### Department Engineering
interface fa0/2
switchport mode access
switchport access vlan 40
description Link-to-PC3-Engineering
no shutdown
exit

---

## Configure Trunk Port
interface fa0/3
switchport mode trunk
no shutdown
exit

---

## The trunk carries multiple VLANs between switches
VLAN 10 ─┐
VLAN 20 ─┤
VLAN 30 ─┼── TRUNK ──> Another Switch
VLAN 40 ─┘

---

## Verification
### Check Vlan
do Show vlan brief

### check Trunk
do show interface trunk

### check port status
do show interfaces fa0/3 status

---

## Troubleshooting
show vlan brief
show interfaces status
show interfaces trunk
show cdp neighbors
show running-config

---
## Tools
Cisco Packet Tracer
Cisco IOS CLI

---
## Screenshoots
![Packet Tracer Enterprise Vlan Lab](screenshots/rslab.PNG)
![Packet Tracer Enterprise Vlan Lab](screenshots/sw1rslab-vlan-IT-HR.PNG)
![Packet Tracer Enterprise Vlan Lab](screenshots/sw1rslab-vlan-Fin-Eng.PNG)
![Packet Tracer Enterprise Vlan Lab](screenshots/sw2rslab-config-interface.PNG)

---
## Network Topology

```text
                    +----------------+
                    |    CORE-L3     |
                    |   Cisco 3560   |
                    +-------+--------+
                            |
                          TRUNK
                            |
              +-------------+-------------+
              |                           |
       +------+-------+            +------+-------+
       | ACCESS-SW1   |            | ACCESS-SW2   |
       | Cisco 2960  |            | Cisco 2960  |
       +------+-------+            +------+-------+
              |                           |
          +---+---+                   +---+---+
          |       |                   |       |
         PC0     PC1                 PC2     PC3
         IT      HR                 Finance Engineering
       VLAN 10  VLAN 20             VLAN 30  VLAN 40

       

       




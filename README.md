# Enterprise Routing & Switching Lab

A hands-on Cisco Packet Tracer lab designed to demonstrate enterprise Routing & Switching skills through a structured **Design → Implement → Configure → Verify → Troubleshoot** workflow.

## Objective

This project simulates a small enterprise network with:

- VLAN segmentation
- Access-port configuration
- 802.1Q trunking
- Multilayer switching using a Cisco 3560
- Inter-VLAN routing using SVIs
- Finance and Engineering VLANs
- Verification using Cisco IOS show commands
- Deliberate troubleshooting scenarios (planned for later stages)

The lab is being developed progressively to reflect practical enterprise network administration and troubleshooting tasks.

---

## Skill Areas

| # | Skill Area | Coverage | Status |
|---|---|---|---|
| 1 | VLANs & Access Switching | VLAN creation, access ports, trunking | In progress |
| 2 | Inter-VLAN Routing | 3560 Layer-3 switch, SVIs, `ip routing` | Next |
| 3 | Spanning Tree (STP) | Loop prevention and root bridge concepts | Planned |
| 4 | EtherChannel / Link Aggregation | LACP, redundancy and bandwidth | Planned |
| 5 | Routing Protocols | Static routing and OSPF | Planned |
| 6 | ACLs | Traffic restrictions between VLANs | Planned |
| 7 | NAT/PAT | Internet-facing edge routing | Planned |
| 8 | Troubleshooting | Deliberately broken configurations and incident diagnosis | Planned |

---

## Current VLAN Plan

| VLAN | Name | Purpose | Example Network |
|---:|---|---|---|
| 10 | IT | IT department | 192.168.10.0/24 |
| 20 | HR | HR department | 192.168.20.0/24 |
| 30 | Finance | Finance department | 192.168.30.0/24 |
| 40 | Engineering | Engineering department | 192.168.40.0/24 |

> The IP addressing plan will be implemented during the Inter-VLAN Routing stage.

---

## Topology

Current design uses a Cisco 3560 multilayer switch as the Layer-3 core/distribution device and two Cisco 2960 access switches.

```text
                         +----------------------+
                         |      CORE-L3         |
                         |   Cisco 3560 L3      |
                         |                      |
                         |  VLAN 10 SVI         |
                         |  VLAN 20 SVI         |
                         |  VLAN 30 SVI         |
                         |  VLAN 40 SVI         |
                         +----------+-----------+
                                    |
                         +----------+----------+
                         |                     |
                       TRUNK                 TRUNK
                         |                     |
                +--------+-------+     +-------+--------+
                |  ACCESS-SW1   |     |   ACCESS-SW2   |
                |    Cisco 2960 |     |    Cisco 2960  |
                +------+---+----+     +----+---+-------+
                       |   |                |   |
                      PC0 PC1              PC2 PC3
                      IT  HR              FIN ENG
```

### Current known physical links

From CDP verification:

```text
CORE-L3 Fa0/4  <-->  ACCESS-SW1 Fa0/3
CORE-L3 Fa0/1  <-->  ACCESS-SW2 Fa0/3
```

ACCESS-SW2 verification confirms:

```text
Fa0/3 = trunk
Fa0/1 = Finance access port (VLAN 30)
Fa0/2 = Engineering access port (VLAN 40)
```

---

## Current Configuration Summary

### CORE-L3

Configured:

- Hostname: `CORE-L3`
- VLAN 10: IT
- VLAN 20: HR
- VLAN 30: Finance
- VLAN 40: Engineering (planned/added to the design)
- Layer-3 routing: `ip routing`
- SVI gateway addresses:
  - VLAN 10 → `192.168.10.1/24`
  - VLAN 20 → `192.168.20.1/24`
  - VLAN 30 → `192.168.30.1/24`
- VLAN 40 gateway → `192.168.40.1/24` to be configured/verified
- Trunk toward ACCESS-SW1: `Fa0/4`
- Trunk toward ACCESS-SW2: `Fa0/1`

### ACCESS-SW2

Configured:

```text
VLAN 30 = Finance
VLAN 40 = Engineering

Fa0/1 = access VLAN 30
Fa0/2 = access VLAN 40
Fa0/3 = trunk
```

Verified with:

```text
show vlan brief
show interfaces fastEthernet 0/3 status
show interfaces trunk
```

---

## Verification Commands

Useful commands for this lab:

```text
show vlan brief
show interfaces status
show interfaces trunk
show interfaces fa0/3 status
show cdp neighbors
show ip interface brief
show ip route
show running-config
show spanning-tree
```

### Verification methodology

For each change:

1. Configure
2. Verify
3. Test connectivity
4. Record the evidence
5. Only then continue

This prevents configuration changes from becoming difficult to troubleshoot later.

---

## Troubleshooting Method

The project deliberately follows a structured troubleshooting flow.

### Example: PC cannot communicate

```text
PC
 |
 +--> Check IP address
 |
 +--> Check subnet mask
 |
 +--> Check default gateway
 |
 +--> Check physical link
 |
 +--> Check access-port VLAN
 |
 +--> Check trunk
 |
 +--> Check VLAN exists
 |
 +--> Check SVI
 |
 +--> Check Layer-3 routing
 |
 +--> Test again
```

### Example: VLAN missing across switches

```text
VLAN exists locally?
        |
        v
Access port assigned correctly?
        |
        v
Trunk operational?
        |
        v
VLAN allowed on trunk?
        |
        v
STP forwarding?
        |
        v
Test connectivity
```

---

## Planned Enterprise Features

### Stage 1 — VLANs & Access Switching

- Create VLANs
- Assign access ports
- Configure trunks
- Verify VLAN propagation

### Stage 2 — Inter-VLAN Routing

- Configure SVIs
- Configure default gateways
- Enable `ip routing`
- Test communication between VLANs

### Stage 3 — STP

- Create redundant Layer-2 paths
- Observe STP behavior
- Identify root bridge
- Troubleshoot blocked/forwarding ports

### Stage 4 — EtherChannel

- Configure LACP
- Bundle redundant links
- Verify the Port-Channel
- Introduce a failed member link

### Stage 5 — Routing

- Static routes
- Default routes
- OSPF
- Neighbor troubleshooting
- Route verification

### Stage 6 — ACLs

Example policy:

```text
IT          -> HR          ALLOW
IT          -> Engineering ALLOW
HR          -> Engineering DENY
Engineering -> Finance     DENY
```

The exact policy will be implemented and tested later.

### Stage 7 — NAT/PAT

Add an edge router and simulate Internet access:

```text
Internal VLANs
      |
      v
CORE-L3
      |
      v
EDGE-ROUTER
      |
      v
   INTERNET
```

### Stage 8 — Deliberate Failures

Examples:

- Wrong VLAN
- Wrong access port
- Trunk removed
- VLAN missing
- Incorrect SVI address
- Incorrect subnet mask
- Wrong default gateway
- `ip routing` disabled
- STP issue
- EtherChannel mismatch
- OSPF neighbor failure
- ACL blocking required traffic
- NAT misconfiguration

---

## Project Structure

```text
enterprise-rs-lab/
├── README.md
├── .gitignore
├── configs/
│   ├── access-sw1-config.txt
│   └── access-sw2-config.txt
├── verification/
│   ├── sw1-show-vlan-brief.txt
│   ├── sw1-show-interfaces-trunk.txt
│   ├── sw2-show-vlan-brief.txt
│   └── sw2-show-interfaces-trunk.txt
└── topology.pkt
```

If a `.pkt` file is not available, export/save a topology screenshot as:

```text
topology.png
```

---

## Evidence & Documentation

Configuration files contain the relevant Cisco IOS configuration used in the lab.

Verification files contain command output captured during implementation.

This creates an audit trail showing:

```text
Configuration
     ↓
Verification
     ↓
Evidence
     ↓
Troubleshooting
```

---

## Relevant Skills

This project is intended to demonstrate practical experience with:

- Cisco IOS
- VLAN design and segmentation
- Access and trunk ports
- 802.1Q trunking
- Cisco Catalyst 2960
- Cisco Catalyst 3560 multilayer switching
- Inter-VLAN routing
- SVI configuration
- Network verification
- CDP
- Structured network troubleshooting
- Enterprise network design principles
- STP
- EtherChannel/LACP
- Static routing
- OSPF
- ACLs
- NAT/PAT

> Do not claim a feature as implemented until it has actually been configured, tested, and documented in the lab.

---

## Lab Philosophy

The goal is not simply to memorize Cisco commands.

The lab follows the operational cycle used in real network support:

**Design → Implement → Configure → Verify → Monitor → Troubleshoot → Fix → Verify → Document**

Every major feature will eventually include a deliberately broken scenario so the ability to diagnose the root cause can be demonstrated.


# Enterprise Switching and Routing

## 1. Overview

This document covers the switching and routing part of my enterprise network lab, built in EVE-NG.

The lab uses a three-tier design (Core, Distribution, Access) with VLAN segmentation, OSPF routing, HSRP gateway configuration and redundant links between layers.

## 2. Status Summary

| Feature | Status |
|---|---|
| VLANs and 802.1Q trunking | Configured and checked |
| Inter-VLAN routing (SVIs) | Configured |
| OSPF between Core and Distribution | Configured, neighbors reached FULL, routes learned |
| Default route (static on Core, learned via OSPF on Distribution) | Configured |
| HSRP (version 2) | Configured |
| Redundant uplinks and Spanning Tree | Configured |
| Failover testing | Not done yet |

## 3. Network Architecture

| Layer | Devices | Role |
|---|---|---|
| Core | Core-A, Core-B | Layer 3 routing and connectivity toward the firewalls |
| Distribution | Distribution-A, Distribution-B | VLAN gateways (SVIs), HSRP, OSPF |
| Access | Four access switches | End-device connectivity and VLAN assignment |

### Core Layer
- Layer 3 switching with routed interfaces
- OSPF routing
- Static default route toward the firewalls
- Redundant links to both distribution switches

### Distribution Layer
- VLANs and SVIs for inter-VLAN routing
- HSRP configuration
- OSPF routing
- 802.1Q trunks to the access switches
- Uplinks to both core switches

### Access Layer
- Connects user devices
- Assigns ports to VLANs
- Trunks VLANs up to the distribution layer

## 4. VLANs

| VLAN ID | Name | Purpose |
|---|---|---|
| 10 | Users | Corporate users |
| 30 | Wired-Guest | Wired guest network |
| 100 | Management | Network management |
| 112 | Guest-Wireless | Wireless guest network |

Extra VLANs/interfaces are used for the links between the core and distribution switches.

## 5. Inter-VLAN Routing

Inter-VLAN routing is done with SVIs on the distribution switches.

| VLAN | Distribution-A SVI | Distribution-B SVI | HSRP Virtual IP |
|---|---|---|---|
| 10 | 10.50.10.2 | 10.50.10.3 | 10.50.10.1 |
| 30 | 10.30.30.2 | 10.30.30.3 | 10.30.30.1 |
| 100 | 10.20.100.2 | 10.20.100.3 | 10.20.100.1 |
| 112 | 192.168.112.2 | 192.168.112.3 | 192.168.112.1 |

The HSRP virtual IP is meant to be the default gateway for each VLAN.

## 6. HSRP Gateway Redundancy

HSRP is configured to give each VLAN a redundant first-hop gateway.

**Configuration**
- Version 2
- VLANs 10, 30, 100 and 112
- Preemption enabled, 60 second delay

**Priority design**

| VLAN | Distribution-A | Distribution-B | Preferred Active |
|---|---:|---:|---|
| 10 | 110 | 90 | Distribution-A |
| 30 | 90 | 110 | Distribution-B |
| 100 | 90 | 110 | Distribution-B |
| 112 | 110 | 90 | Distribution-A |

The priorities are meant to split the active role between the two switches.



## 7. OSPF Dynamic Routing

OSPF exchanges routes between the core and distribution layers.

**Configured**
- OSPF process and network advertisements
- Default route propagation

**Observed**
- Neighbor adjacencies reached the FULL state (seen in logs)
- Routing tables showed OSPF-learned routes between core and distribution

The two core switches use different local OSPF process IDs. This is fine because the process ID is only locally significant and does not need to match between neighbors.

## 8. Default Routing

- Static default routes on the core switches toward the firewalls
- Distribution switches learn the default route through OSPF

## 9. Switching and Trunking

802.1Q trunks carry the VLANs between switches:
- Core to distribution
- Distribution to access
- Native VLAN and allowed VLAN lists configured
- Spanning Tree forwarding states checked

## 10. Redundancy Design

The design includes:
- Two core switches
- Two distribution switches
- Multiple uplinks between core and distribution
- HSRP (configured, working)
- OSPF (configured, working)
- Spanning Tree for Layer 2 loop prevention


## 11. Verification Commands

```cisco
show vlan brief
show ip interface brief
show interfaces status
show interfaces trunk
show ip route
show ip ospf neighbor
show standby brief
show standby
show spanning-tree
show spanning-tree summary
```

## 12. Topics Covered

- Cisco IOS on EVE-NG
- VLANs and 802.1Q trunking
- Inter-VLAN routing with SVIs
- OSPF
- Static and default routing
- Spanning Tree Protocol
- HSRP version 2 (configured, troubleshooting in progress)
- Network troubleshooting

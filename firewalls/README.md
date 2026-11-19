# Enterprise Firewall Infrastructure

## 1. Overview

This document covers the firewall part of my enterprise network lab, built in EVE-NG.

The lab includes:
- Two Palo Alto firewalls configured as an HA pair
- One FortiGate firewall
- Connections to the internal network, the DMZ and the ISP

## 2. Status Summary

| Feature | Status |
|---|---|
| Palo Alto zones, interfaces and security policies | Configured |
| Palo Alto HA pair | Configured, failover not tested yet |
| FortiGate interfaces, policies and routing | Configured |
| DNAT (publishing a DMZ server) | Configured and tested, works |
| SNAT (outbound internet access) | Configured, not tested yet |

## 3. Firewall Architecture

| Device | Role |
|---|---|
| Palo Alto 1 and Palo Alto 2 | HA pair connecting the core network, the DMZ and the ISP |
| FortiGate | Firewall connected to the internal servers network |

## 4. Palo Alto Firewalls

Configured areas:
- Security zones and Layer 3 interfaces
- Security policies
- Source NAT and destination NAT
- Routing
- HA between the two firewalls
- Traffic logs for checking and troubleshooting

### Security Policy Design

Policies control which traffic is allowed between zones. The main traffic flows are:

| Source Zone | Destination Zone | Purpose |
|---|---|---|
| Internal | DMZ | Controlled access to DMZ servers |
| Internal | External | Outbound internet access |
| External | DMZ | Access to the published DMZ server (DNAT) |

## 5. FortiGate Firewall

Configured areas:
- Interfaces and address objects
- Firewall policies (source, destination, service, action)
- Static routing
- NAT and virtual IPs
- Traffic logs

## 6. Network Segmentation

The firewalls separate these networks:
- Internal user and server networks
- DMZ
- ISP / external network
- Management network

## 7. NAT

| Type | Purpose | Status |
|---|---|---|
| DNAT | Publish a DMZ server to the external network | Tested, works |
| SNAT | Outbound internet access for internal networks | Configured, not tested yet |

## 8. DMZ

The DMZ is a separate segment for the lab servers (web, mail, app and cloud) behind the firewalls. Only the traffic allowed by the security policies can reach it.

## 9. Verification

**Palo Alto:** interface status, routing table, security policy and NAT policy, traffic logs, HA status.

**FortiGate:** system status, interface status, routing table, firewall policies, virtual IPs, traffic logs.


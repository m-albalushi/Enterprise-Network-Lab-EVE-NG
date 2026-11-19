# Demilitarized Zone (DMZ)

## 1. Overview

This document covers the DMZ in my enterprise network lab, built in EVE-NG. The DMZ is a separate network segment for servers that need controlled access from internal and external networks. It is connected through a DMZ switch and sits behind the firewalls.

## 2. DMZ Servers

| Server | Role |
|---|---|
| WEB01 | Web server |
| MAIL01 | Mail server |
| APP01 | Application server |
| CLOUD01 | Cloud-related server |

## 3. Status Summary

| Item | Status |
|---|---|
| DMZ segment behind the firewalls | Built |
| DNAT publishing of a DMZ server | Configured and tested, works |
| Internal to DMZ access policies | Configured, not documented in detail yet |

## 4. Network Design

The DMZ is separate from the internal user and management networks. The firewalls control traffic between:
- External network and DMZ
- Internal network and DMZ
- DMZ and the other internal segments

## 5. Security Design

- External users should only reach services that are published on purpose.
- Internal users reach DMZ services only as the firewall policies allow.
- DMZ servers should not have unrestricted access to internal networks.
- Traffic logs are used for monitoring and troubleshooting.

## 6. Traffic Flows

**External to DMZ (tested with DNAT)**
1. An external client sends a request to the published address.
2. The firewall translates the destination address (DNAT).
3. The security policy checks the traffic.
4. Allowed traffic reaches the DMZ server.

**Internal to DMZ (design)**
1. An internal client connects to a DMZ service.
2. The firewall checks the security policy.
3. Allowed traffic reaches the DMZ server.

## 7. Verification

- DMZ switch VLAN and trunk configuration
- Server IP connectivity
- Firewall interface status
- Security and NAT policies
- Firewall traffic logs
- External access to the published server

## 8. Scope

This document covers the DMZ network design and how it connects to the firewalls. The server operating systems and applications are not configured or documented here.

## 9. Note

This is a personal learning lab. The addresses are lab addresses and are not from any production network.

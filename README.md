# Enterprise Network Lab (EVE-NG)

A personal lab I built in EVE-NG to practise enterprise network design and firewall configuration. I use it to try setups and changes safely before working with similar ones at work.

## Topology

![Topology](topology.png)

## What I built

- **Three-tier design:** Core, Distribution and Access layers with redundant links between them.
- **VLANs and inter-VLAN routing:** user VLANs 10, 30 and 112.
- **HSRP** for gateway redundancy.
- **Palo Alto firewalls** in an HA pair.
- **FortiGate firewall** connected to the internal servers network.
- **DMZ** with web, mail, app and cloud servers, plus an internal servers network.
- **ISP link** and a separate **management network**.
- **NAT on the firewalls:**
  - DNAT to publish a DMZ server. Tested and working.
  - SNAT for outbound internet access. Configured, not tested yet.

## Tools

EVE-NG, Cisco IOS switches, Palo Alto VM-Series, FortiGate VM.

## Status

This is a work in progress. Next steps:
- Test SNAT
- Test HA failover
- Add sanitized configuration examples and my troubleshooting notes

## Note

This is a personal learning lab. It is not my employer's network, and any configs I share are sanitized.

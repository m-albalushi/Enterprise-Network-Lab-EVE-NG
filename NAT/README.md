# Network Address Translation (NAT)

## 1. Overview

This document covers the NAT setup in my enterprise network lab, built in EVE-NG. I configured both Destination NAT (DNAT) and Source NAT (SNAT) on the firewalls.

## 2. Status Summary

| NAT Type | Purpose | Status |
|---|---|---|
| DNAT | Publish a DMZ server to the external network | Configured and tested, works |
| SNAT | Outbound internet access for internal networks | Configured, not tested yet |

## 3. Destination NAT (DNAT)

DNAT translates an external destination address to the address of a server in the DMZ.

**Traffic flow**
1. An external client sends a request to the published address.
2. The firewall receives it and translates the destination address to the DMZ server.
3. The security policy decides whether the traffic is allowed.
4. Allowed traffic is forwarded to the DMZ server, which replies.

**Result:** tested successfully. The published server was reachable from the external side.

## 4. Source NAT (SNAT)

SNAT translates the source address of internal traffic going toward the external network, so outside networks see the translated address instead of the internal one.

**Traffic flow**
1. An internal client starts a connection to the external network.
2. The firewall translates the source address.
3. Traffic is forwarded toward the ISP.
4. Return traffic is translated back and sent to the original client.


## 5. NAT and Security Policies

NAT only changes addresses. A security policy is still needed to allow the traffic. Both must be correct for a flow to work.

## 6. How I Test

**DNAT**
- Access the published server using its external address
- Check that the request reaches the DMZ server
- Check the firewall traffic logs

**SNAT**
- Start outbound traffic from an internal client
- Check the translated source address
- Check the firewall session/NAT logs
- Confirm return traffic reaches the client



---
title: Appendix B - NetX Duo DHCPv6 server status codes
description: This chapter contains a description of all NetX Duo DHCPv6 server status codes.
---

# Appendix B - NetX Duo DHCPv6 server status codes

| Name              | Code            | Description |
| ------------------- | ------------------- | --------------- |
| Success | 0 | Success |
| Unspecified Failure | 1 | Failure, reason unspecified; this status code is set by the Server to indicate a general failure in granting the Client request not matching the other codes |
| NoAddress Available | 2 | Server has no addresses available to assign to the Client |
| NoBinding | 3 | Client IA address (binding) is not available e.g. the requested IP address is not available for the Server to lease or assigned to another Client. |
| NotOnLink | 4 | The prefix for the address indicates the IP address is not an on link address |
| UseMulticast | 5 | Sent by a Server in response to receiving a Client message using the Server's unicast address instead of the *All_DHCP_Relay_Agents_and_Servers* multicast address |
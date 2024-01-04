---
title: Appendix A – NetX Duo DHCPv6 option codes
description: This chapter contains a description of all NetX Duo DHCPv6 option codes.
---

# Appendix A – NetX Duo DHCPv6 option codes

| Option              | Code            | Description |
| ------------------- | ------------------- | --------------- |
| Client Identifier DUID | 1 | Uniquely identifies a Client host on the network |
| Server Identifier (DUID) | 2 | Uniquely identifies the DHCPv6Server host on the network |
| Identity Association for Non Temporary Addresses (IANA) | 3 | Parameters for a non temporary IP address assignment |
| Identity Association for Temporary Addresses (IATA) | 4 | Parameters for a temporary IP address assignment |
| IA Address | 5 | Actual IPv6 address and IPv6 address lifetimes to be assigned to the Client |
| Option Request | 6 | A list of information requests to obtain network information such as DNS server and other network configuration parameters. |
| Preference | 7 | Included in server Advertise message to client to influence the Client's choice of servers. The Client must choose a server with higher the preference value over other servers. 255 is the maximum value, while zero indicates the client can choose any server replying back to them |
| Elapsed Time | 8 | Contains the time (in 0.01 seconds) when the Client initiates the DHCPv6 exchange with the server. Used by secondary server(s) to determine if the primary server responds in time to the Client request. |
| Relay Message | 9 | Contains the original message in Relay message | 
| Authentication | 11 | Contains information to authenticate the identity and content of DHCPv6 messages |
| Server Unicast | 12 | Server sends this option to let the Client know that the server will accept unicast messages directly from the Client instead of multicast. |

IATA, Relay Message, Authentication and Server Unicast options are not supported in this release of NetX Duo DHCPv6 Server. The current DHCPv6 protocol option code 10 is left undefined in RFC 3315.
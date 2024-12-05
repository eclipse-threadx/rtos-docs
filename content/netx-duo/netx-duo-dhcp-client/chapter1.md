---
title: Chapter 1 - Introduction to the NetX Duo DHCP Client
description: In NetX Duo DHCP Client, the application's IP address is one of the supplied parameters to the *nx_ip_create* service call.
---


In NetX Duo DHCP Client, the application's IP address is one of the supplied parameters to the *nx_ip_create* service call. Supplying the IP address poses no problem if the IP address is known to the application, either statically or through user configuration. However, there are some instances where the application doesn't know or care what its IP address is. In such situations, a zero IP address should be supplied to the *nx_ip_create* function and the DHCP Client protocol should be used to dynamically obtain an IP address.

## Dynamic IP Address Assignment

The basic service used to obtain a dynamic IP address from the network is the Reverse Address Resolution Protocol (RARP). This protocol is similar to ARP, except it is designed to obtain an IP address for itself instead of finding the MAC address for another network node. The low-level RARP message is broadcast on the local network and it is the responsibility of a server on the network to respond with an RARP response, which contains a dynamically allocated IP address.

Although RARP provides a service for dynamic allocation of IP addresses, it has several shortcomings. The most glaring deficiency is that RARP only provides dynamic allocation of the IP address. In most situations, more information is necessary in order for a device to properly participate on a network. In addition to an IP address, most devices need the network mask and the gateway IP address. The IP address of a DNS server and other network information may also be needed. RARP does not have the ability to provide this information.

## RARP Alternatives

In order to overcome the deficiencies of RARP, researchers developed a more comprehensive IP address allocation mechanism called the boot strap Protocol (BOOTP). This protocol has the ability to dynamically allocate an IP address and also provide additional important network information. However, BOOTP has the drawback of being designed for static network configurations. It does not allow for quick or automated address assignment.

This is where the Dynamic Host Configuration Protocol (DHCP) is extremely useful. DHCP is designed to extend the basic functionality of BOOTP to include completely automated IP server allocation and completely dynamic IP address allocation through "leasing" an IP address to a client for a specified period of time. DHCP can also be configured to allocate IP addresses in a static manner like BOOTP.

## DHCP Messages

Although DHCP greatly enhances the functionality of BOOTP, DHCP uses the same message format as BOOTP and supports the same vendor options as BOOTP. In order to perform its function, DHCP introduces seven new DHCP-specific options, as follows:

- DISCOVER (1) (sent by DHCP Client)
- OFFER (2) (sent by DHCP Server)
- REQUEST (3) (sent by DHCP Client)
- DECLINE (4) (sent by DHCP Client)
- ACK (5) (sent by DHCP Server)
- NACK (6) (sent by DHCP Server)
- RELEASE (7) (sent by DHCP Client)
- INFORM (8) (sent by DHCP Client)
- FORCERENEW (9) (sent by DHCP Client)

## DHCP Communication

DHCP utilizes the UDP protocol to send requests and field responses. Prior to having an IP address, UDP messages carrying the DHCP information are sent and received by utilizing the IP broadcast address of 255.255.255.255.

## DHCP Client State Machine

The DHCP Client is implemented as a state machine. The state machine is processed by an internal DHCP thread that is created during *nx_dhcp_create* processing. The main states of DHCP Client are as follows:

- **NX_DHCP_STATE_BOOT** Starting with a previous IP address
- **NX_DHCP_STATE_INIT** Starting with no previous IP address value
- **NX_DHCP_STATE_SELECTING** Waiting for a response from any DHCP server
- **NX_DHCP_STATE_REQUESTING** DHCP Server identified, IP address request sent
- **NX_DHCP_STATE_BOUND** DHCP IP Address lease established
- **NX_DHCP_STATE_RENEWING** DHCP IP Address lease renewal time elapsed, renewal requested
- **NX_DHCP_STATE_REBINDING** DHCP IP Address lease rebind time elapsed, renewal requested
- **NX_DHCP_STATE_FORCERENEW** DHCP IP Address lease established, force renewal by server (currently not supported) or by the application calling nx_dhcp_force_renew
- **NX_DHCP_STATE_ADDRESS_PROBING** DHCP IP Address probing, send the ARP probe to detect IP address conflict.

## DHCP Client Multiple Interface Support

The DHCP Client was previously implemented to run on only a single network interface. The default behavior was (and still is) for the DHCP Client to run on the primary interface. By calling *nx_dhcp_set_interface_index*, the application could (and still can) run DHCP on a secondary network interface instead of the primary interface.

It now supports DHCP running on multiple interfaces in parallel. See **DHCP Client on Multiple Interfaces Simultaneously** in Chapter Two for specific details how to run DHCP Client on more than one physical interface simultaneously.

## DHCP User Request

Once the DHCP server grants an IP address, the DHCP client processing can request additional parameters — one at a time — by using the *nx_dhcp_user_option_request* service.

## DHCP Client Socket Queue 

The DHCP Client automatically clears broadcast packets from DHCP Servers intended for other DHCP Clients from its socket receive queue while waiting for Server to respond to itself. In a busy network, not doing so could cause packets intended for the Client to be dropped.

## DHCP RFCs

NetX Duo DHCP Client is compliant with RFC2132, RFC2131, and related RFCs.
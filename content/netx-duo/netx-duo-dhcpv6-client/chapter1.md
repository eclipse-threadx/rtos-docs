---
title: Chapter 1 - Introduction to NetX Duo DHCPv6 Client
description: "In IPv6 networks, DHCPv6 replaces DHCP for dynamic global IP address assignment from a DHCPv6 Server, and offers most of the same features as well as many enhancements."
---


In IPv6 networks, DHCPv6 replaces DHCP for dynamic global IP address assignment from a DHCPv6 Server, and offers most of the same features as well as many enhancements. This document will explain in detail how the NetX Duo DHCPv6 Client API is used to obtain IPv6 addresses.

## DHCPv6 Communication

The DHCPv6 protocol uses UDP. The Client uses port 546 and the Server uses port 547 to exchange DHCPv6 messages. The Client uses its link local address for a source address to initiate the DHCPv6 requests to the DHCPv6 server(s) available. When the Client sends messages intended for all DHCPv6 servers on the network it uses the *All_DHCP_Relay_Agents_and_Servers* multicast address *FF02::01:02*. This is a reserved, link-scoped multicast address.

## DHCPv6 Process of Requesting an IPv6 Address

To begin the process of requesting a global IPv6 address assignment, a Client first sends a SOLICIT message using the *nx_dhcpv6_send_solicit* service:

**UINT nx_dhcpv6_request_solicit(NX_DHCPV6 \*dhcpv6_ptr)**

This message is sent to all servers using the *All_DHCP_Relay_Agents_and_Servers* address. In the SOLICIT request, the Client may request the assignment of specific IPv6 address(es) as a hint to the Server. It can also request other network configuration information from the Server such as DNS server, NTP server and other options in the Option Request in the SOLICIT message.

A DHCPv6 Server that can service a Client request responds with an ADVERTISE message containing the IPv6 address(es) it can assign to the Client, the IPv6 address lease time and any additional information requested by the Client. The DHCPv6 Client protocol requires the Client to wait for a period of time to receive ADVERTISE messages from all DHCPv6 Servers on the network. It pre-processes each ADVERTISE message to be a valid message, and scans the option data for various DHCPv6 parameters. It also checks the Preference value in the Preference Option, if supplied by the Server. If more than one ADVERTISE message is received, the NetX Duo DHCPv6 Client chooses the Server with the highest preference value received by the end of the wait period.

The exception is if the Client receives an ADVERTISE message with a preference value of 255. It accepts that message immediately and discards all subsequent ADVERTISE messages.

The wait period is defined as the retransmission period that the DHCPv6 Client waits before sending another SOLICIT message if it has not received a response from any Server. The initial retransmission timeout in the SOLICIT state is defined by to the DHCPv6 protocol described in RFC 3315 to be 1 second. Subsequent retransmission intervals, if the DHCPv6 Client fails to receive a valid Server response, are doubled up to a maximum of 120 seconds.

Having chosen the Server, the Client extracts data from the ADVERTISE message and sends a REQUEST message back to the Server to accept the assigned address information and lease times. The Server responds with a REPLY message to confirm the Client IPv6 address(es) are registered with the Server as assigned to the Client.

The DHCPv6 Client registers the assigned IPv6 address(es) with the IP instance (e.g NetX Duo). If configured for the Duplicate Address Detection (DAD) protocol (enabled by default), NetX Duo will automatically send Neighbor Solicit messages to verify the assigned address(es) are unique on the network. If so, it notifies the DHCPv6 Client when the each assigned address has been promoted from TENTATIVE to VALID. The DHCPv6 Client is promoted to the BOUND state and the device may use that IPv6 address to send and transmit messages. If the DAD protocol fails, NetX Duo notifies the DHCPv6 Client and the DHCPv6 Client sends a DECLINE message for the IPv6 address(es) assigned back to the Server and resets the DHCPv6 Client state to the INIT state.

### Notification of Successful Address Assignment and Validation

The application can determine the result of the DHCPv6 Client address solicitation from the state changes if the DHCPv6 Client is configured with the state change callback in the *nx_dhcpv6_client_create* service. If it receives no response from the Server the state change observed is from SOLICIT to INIT. If it received a response from the Server but the Server is unable to assign the address, the application will be notified by the DHCPv6 Client server error callback if configured with one (also in *nx_dhcpv6_client_create).* If the Client achieved the BOUND state but then failed the DAD check, it will see a state change from BOUND to INIT. Note that an application that is enabled for DAD must allow time for the DAD check after starting the request process. Typically this is about 400-500 ticks (4-5 seconds in most cases).

### Relinquishing an IPv6 Address

If and when the Client needs to release an assigned IPv6 address, it informs the DHCPv6 server by calling the *nx_dhcpv6_request_release* service:

### UINT nx_dhcpv6_request_release(NX_DHCPV6 \*dhcpv6_ptr)

The DHCPv6 Client sends a unicast RELEASE message containing the assigned addresses to the Server who assigned the address and waits for a REPLY confirming the Server received the message.

> **Note:** There is a different process for the Client that is powering down but plans to continue using the assigned IPv6 addresses on reboot. It does not send the RELEASE message on powering down unless it plans to request a new address on power up. See "Non Volatile Memory Requirements" for explanation of this situation.

### DHCPv6 Lease Timeouts

The IPv6 lease assigned by the Server contains two timeout parameters, T1 and T2 in each Identity Association – Non Temporary Addresses (IANA) block. An IANA is described in elsewhere in this User Guide. If the elapsed time from when the DHCPv6 Client was bound to the assigned IPv6 address equals T1, the DHCPv6 Client automatically starts renewing the IPv6 address by sending a RENEW message. If the elapsed time equals T2, DHCPv6 Client automatically sends a REBIND message if it received no responses to its RENEW requests.

Two other IPv6 lease parameters, preferred and valid lifetime, are assigned with each Identity Association (IA) block contained in the IANA block. The preferred and valid lifetimes are when the assigned IPv6 address is deprecated or invalid, respectively. T1 must be less than the preferred lifetime. T2 must be less than the valid lifetime.

### IP Thread Task Requirements

The NetX Duo DHCPv6 Client requires creation of a NetX Duo IP instance previous to creating the DHCPv6 Client to use DHCPv6 Client services.

UDP, IPv6, and ICMPv6 must be enabled on the IP instance prior to the using DHCPv6 Client.

  - *nx_udp_enable*

  - *nxd_ipv6_enable*

  - *nxd_icmp_enable*

### Packet Pool Requirements

NetX Duo DHCPv6 Client creation requires a previously created packet pool for sending DHCPv6 messages. The size of the packet pool in terms of packet payload and number of packets available is user configurable, and depends on the anticipated volume of DHCPv6 messages and other transmissions the application will be sending.

A typical DHCPv6 message is about 200 bytes depending on the number of IA addresses and DHCPv6 options requested by the Client.

### Network Requirements

The NetX Duo DHCPv6 Client requires the creation of UDP socket bound to port 546. The socket is created when the DHCPv6 Client task is created.

### Non Volatile Memory Requirements

If a DHCPv6 Client releases its IPv6 lease with the DHCPv6 Server when powering down, and requests new IPv6 address(es) on reboot, then non volatile memory storage is not required. If a Client wishes to continue using its assigned lease, it must store certain information about the DHCPv6 Client to non volatile memory across reboots.

Non volatile memory requirements and the DHCPv6 Client API are discussed further in **Using the NetX Duo DHCPv6 Client** in Chapter Two.

### NetX Duo DHCPv6 Client Limitations

The current release of the NetX Duo DHCPv6 Client has the following limitations:

  - NetX Duo DHCPv6 Client does not support the Server Unicast option for sending unicast DHCPv6 messages to the DHCPv6 Server even if the Server indicates this is permitted.

  - NetX Duo DHCPv6 Client does not support the Reconfigure request in which a Server initiates IPv6 address changes to the Clients on the network.

  - NetX Duo DHCPv6 Client does not support the Enterprise format for the DHCPv6 Unique Identifier control block. It only supports Link Layer and Link Layer Plus Time format.

  - NetX Duo DHCPv6 Client does not support Temporary Association (TA) address requests, but does support Non Temporary (IANA) option requests.

## Multihome and Multiple Address Support

The DHCPv6 Client supports multiple interfaces and multiple addresses per interface. The DHCPv6 Client service, *nx_dhcpv6_client_set_interface* enables the Client application to set the network interface on which the application will be communicating with the DHCPv6 Server. The DHCPv6 Client defaults to the primary interface (index zero).

For multiple addresses per interface, the DHCPv6 Client keeps an internal list of addresses starting at index 0. Note that the same address registered with the DHCPv6 Client may not necessarily be located at the same index in the IP table of interface addresses.

For DHCPv6 Client services that retrieve information about the Client IPv6 address lease, some require an address index to be specified. An example for obtaining the preferred and valid lifetimes is shown below:

```C
UINT nx_dhcpv6_get_valid_ip_address_lease_time(NX_DHCPV6 *dhcpv6_ptr, 
											  UINT address_index, 
											  NXD_ADDRESS *ip_address,
                                              ULONG preferred_lifetime, 
											  ULONG *valid_lifetime)
```

The Client application can also retrieve the number of valid IPv6 addresses assigned from the *nx_dhcpv6_get_valid_ip_address_count* service:

```C
UINT nx_dhcpv6_get_valid_ip_address_count(NX_DHCPV6 *dhcpv6_ptr, 
										  UINT *address_count)
```

Legacy DHCPv6 Client services which were created before multiple addresses were supported in NetX Duo do not take an address index. Therefore, with these services, the data requested is taken from the primary global IA address, regardless how many IA addresses are assigned to the Client. An example is shown below:

```C
UINT nx_dhcpv6_get_IP_address(NX_DHCPV6 *dhcpv6_ptr, 
                              NXD_ADDRESS *ip_address)
```

## NetX Duo DHCPv6 Client Callback Functions

*nx_dhcpv6_state_change_callback*

When the DHCPv6 Client changes to a new DHCPv6 state as a result of processing a DHCPv6 request, it notifies the application with this callback function.

*nx_dhcpv6_server_error_handler*

When the DHCPv6 Client receives a Server reply containing a *Status* option with a non-zero (non successful) status, it notifies the application with this callback which includes the Server error status code.

> **Note:** Since these callback functions are called from the DHCPv6 Client thread task, the Client application must NOT call any NetX Duo DHCPv6 Client services that require mutex control of the DHCPv6 Client such as *nx_dhcpv6_start, nx_dhcpv6_stop,* and any of the API that send messages directly from the callback e.g. *nx_dhcpv6_request_release*.

## DHCPv6 RFCs

NetX Duo DHCP is compliant with RFC3315, RFC3646, and related RFCs.
---
title: Chapter 3 - NetX Duo DHCPv6 server configuration options
description: This chapter contains a description of the NetX Duo DHCPv6 server configuration options.
---

# Chapter 3 - NetX Duo DHCPv6 server configuration options

There are several configuration options for building a NetX Duo DHCPv6 Server application. The following list describes each in detail:
  
- **NX_DISABLE_ERROR_CHECKING** This option removes DHCPv6 error checking. It typically enabled when the application is debugged.  
  
- **NX_DHCPV6_SERVER_THREAD_STACK_SIZE** This defines the size of the DHCPv6 thread stack. By default, the size is 4096 bytes which is more than enough for most NetX Duo applications.

- **NX_DHCPV6_SERVER_THREAD_PRIORITY** This defines the DHCPv6Server thread priority. This should be lower than the DHCPv6 Server's IP thread task priority. The default value is 2.

- **NX_DHCPV6_IP_LEASE_TIMER_INTERVAL** Timer interval in seconds when the lease timer entry function is called by the ThreadX scheduler. The entry function sets a flag for the DHCPv6 Server to increment all Clients' accrued time on their lease by the timer interval. By default, this value is 60.

- **NX_DHCPV6_SESSION_TIMER_INTERVAL** Timer interval in seconds when the session timer entry function is called by the ThreadX scheduler. The entry function sets a flag for the DHCPv6 Server to increment all active Client session time accrued by the timer interval. By default, this value is 3.

The following defines apply to the status option message type and the user configurable message. The status option indicates the outcome of a Client request:

- **NX_DHCPV6_STATUS_MESSAGE_SUCCESS** *"IA OPTION GRANTED"*

- **NX_DHCPV6_STATUS_NO_ADDRS_AVAILABLE** *"IA OPTION NOT GRANTED-NO ADDRESSES AVAILABLE"*

- **NX_DHCPV6_STATUS_MESSAGE_NO_BINDING** *"IA OPTION NOT GRANTED-INVALID CLIENT REQUEST"*

- **NX_DHCPV6_STATUS_MESSAGE_NOT_ON_LINK** *"IA OPTION NOT GRANTED-CLIENT NOT ON LINK"*

- **NX_DHCPV6_STATUS_MESSAGE_USE_MULTICAST** *"IA OPTION NOT GRANTED-CLIENT MUST USE MULTICAST"*

- **NX_DHCPV6_STATUS_MESSAGE_NO_ADDRS_AVAILABLE** *IA OPTION NOT GRANTED-NO ADDRESSES AVAILABLE*

- **NX_DHCPV6_SERVER_DUID_VENDOR_ASSIGNED_ID** Create a Server DUID with a vendor assigned ID. Note the DUID type must be set NX_DHCPV6_DUID_TYPE_VENDOR_ASSIGNED.

- **NX_DHCPV6_SERVER_DUID_VENDOR_ASSIGNED_LENGTH** Sets the upper limit on the Vendor assigned ID. The default value is 48.

- **NX_DHCPV6_SERVER_DUID_VENDOR_PRIVATE_ID** Sets the enterprise type of the DUID to private vendor type.

- **NX_DHCPV6_PACKET_WAIT_OPTION** This defines the wait option for the Server *nx_udp_socket_receive* call. This is perfunctory since the socket has a receive notify callback from NetX Duo, so the packet is already enqueued when the DHCPv6 server calls the receive function. The default value is 1 second (1 * NX_IP_PERIODIC_RATE).

- **NX_DHCPV6_SERVER_DUID_TYPE** This defines the Server DUID type which the Server includes in all messages to Clients. The default value is link layer plus time (NX_DHCPV6_SERVER_DUID_TYPE_LINK_TIME).

- **NX_DHCPV6_SERVER_HW_TYPE** This defines the hardware type in the DUID link layer and link layer plus time options. The default value is NX_DHCPV6_SERVER_HARDWARE_TYPE_ETHERNET.

- **NX_DHCPV6_PREFERENCE_VALUE** This defines the preference option value between 0 and 255, where the higher the value the higher the preference, in the DHCPv6 option of the same name. This tells the Client what preference to place on this Server's offer where multiple DHCPv6 Servers are available to assign IP addresses. A value of 255 instructs the Client to choose this server. A value of zero indicates the Client is free to choose. The default value is zero.

- **NX_DHCPV6_MAX_OPTION_REQUEST_OPTIONS** This defines the maximum number of option requests in a Client request that can be saved to a Client record. The default value is 6.

- **NX_DHCPV6_DEFAULT_T1_TIME** The time in seconds assigned by the Server on a Client address lease for when the Client should begin renewing its IP address. The default value is 2000 seconds.

- **NX_DHCPV6_DEFAULT_T2_TIME** The time in seconds assigned by the Server on a Client address lease for when the Client should begin rebinding its IP address assuming its attempt to renew failed. The default value is 3000 seconds.

- **NX_DHCPV6_DEFAULT_PREFERRED_TIME** This defines the time in seconds assigned by the Server for when an assigned Client IP address lease is deprecated. The default value is 2 NX_DHCPV6_DEFAULT_T1_TIME.

- **NX_DHCPV6_DEFAULT_VALID_TIME** This defines the time expiration in seconds assigned by the Server on an assigned Client IP address lease. After this time expires, the Client IP address is invalid. The default value is 2 NX_DHCPV6_DEFAULT_PREFERRED_TIME.

- **NX_DHCPV6_STATUS_MESSAGE_MAX** Defines the maximum size of the Server message in status option message field . The default value is 100 bytes.

- **NX_DHCPV6_MAX_LEASES** Defines the size of the Server's IP lease table (e.g. the max number of IPv6 address available to lease that can be stored). By default, this value is 100.

- **NX_DHCPV6_MAX_CLIENTS** Defines the size of the Server's Client record table (e.g. max number of Clients that can be stored).This value should be less than or equal to the value NX_DHCPV6_MAX_LEASES. By default, this value is 120.

- **NX_DHCPV6_PACKET_TIME_OUT** Defines the wait option in timer ticks for the DHCPv6 Server wait on packet allocations. The default value is 3 * NX_DHCPV6_SERVER_TICKS_PER_SECOND.

- **NX_DHCPV6_PACKET_RECEIVE_WAIT** Defines the wait option in packet allocate calls on the Server packet pool. The default value is (3 * NX_DHCPV6_SERVER_TICKS_PER_SECOND) , or 3 seconds.

- **NX_DHCPV6_PACKET_SIZE** This defines the packet payload of the Server packet pool packets. The default value is 500 bytes.

- **NX_DHCPV6_PACKET_POOL_SIZE** Defines the Server packet pool size for packets the Server will allocate to send DHCPv6 messages out. The default value is (10 * NX_DHCPV6_PACKET_SIZE).

- **NX_DHCPV6_TYPE_OF_SERVICE** This defines the type of service for UDP packet transmission. By
default, this value is NX_IP_NORMAL.

- **NX_DHCPV6_FRAGMENT_OPTION** This defines the Server socket fragmentation option. The default value is NX_DON'T_FRAGMENT

- **NX_DHCPV6_TIME_TO_LIVE** Specifies the number of routers DHCPv6 packets from the Server
may 'hop' pass before packets are discarded. The default value is set to 0x80.

- **NX_DHCPV6_QUEUE_DEPTH** Specifies the number of packets to keep in the Server UDP socket receive queue before NetX Duo discards packets.
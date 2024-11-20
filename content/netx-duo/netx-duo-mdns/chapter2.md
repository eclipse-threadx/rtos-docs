---
title: Chapter 2 - Installation and use of mDNS
description: This chapter contains a description of various issues related to installation, set up, and usage of the NetX Duo mDNS module.
---


This chapter contains a description of various issues related to installation, set up, and usage of the NetX Duo mDNS module.

## Product Distribution

NetX Duo mDNS is available at [https://github.com/eclipse-threadx/netxduo](https://github.com/eclipse-threadx/netxduo). The package includes two source files and a PDF file that contains this document, as follows:

- **nxd_mdns.h** Header file for NetX Duo mDNS module
- **nxd_mdns.c** C Source file for NetX Duo mDNS module
- **nxd_mdns.pdf** PDF description of mDNS for NetX Duo
- **demo_netxduo_mdns.c** A simple demonstration program that illustrates the services provided by mDNS.

## NetX Duo mDNS Installation

In order to use the NetX Duo mDNS APIs, the entire distribution mentioned previously should be copied to the same directory where NetX Duo is installed. For example, if NetX Duo is installed in the directory "*c:\netxduo*" then the *nxd_mdns.h* and *nxd_mdns.c* should be copied into this directory.

## Using NetX Duo mDNS

Using the NetX Duo mDNS module is easy. Basically, the application code must include *nxd_mdns.h* after it includes *tx_api.h and* *nx_api.h*, in order to use ThreadX, and NetX Duo, respectively. The build project must include the mDNS source code and the application file, and of course the ThreadX and NetX library files. This is all that is required to use NetX Duo mDNS.

## Configuration Options

There are several configuration options for building NetX Duo mDNS module. The default values are listed, but each define can be set by the application prior to inclusion of the specified NetX Duo mDNS header file. The following list describes each in detail:

- **NX_MDNS_DISABLE_SERVER** Disables the mDNS/DNS-SD server functionality. Without the server functionality, the mDNS/DNS-SD module does not announce services provided by local host, nor does it respond to mDNS enquiries. This symbol is not defined by default.
- **NX_MDNS_DISABLE_CLIENT** Disables the mDNS/DNS-SD client functionality. Without the client functionality, mDNS/DNS-SD does not send queries, nor does it maintain mDNS query responses received over the network.
- **NX_MDNS_ENABLE_ADDRESS_CHECK** Defined, the mDNS module performs validates addresses (source address, destination address, and port numbers) from the received mDNS messages. This symbol is defined by default.
- **NX_MDNS_ENABLE_CLIENT_POOF** Defined, the mDNS module performs Passive Observation Of Failures, mDNS /DNS_SD client (querier) observes the multicast queries issued by the other hosts on the network. This symbol is defined by default.
- **NX_MDNS_ENABLE_SERVER_NEGATIVE_RESPONSES** Defined, the mDNS /DNS-SD server (responder) generates negative responses to queries for which it has legitimate ownership. This symbol is defined by default.
- **NX_MDNS_ENABLE_IPV6** Defined, the mDNS/DNS-SD send/process mDNS message over IPv6 address. This symbol is not defined by default.
- **NX_MDNS_IPV6_ADDRESS_COUNT** Maximum IPv6 addresses count of host. The default value is 2.
- **NX_MDNS_HOST_NAME_MAX** Maximum string size for host name. The default value is 64. Does not include the NULL terminator.
- **NX_MDNS_SERVICE_NAME_MAX** Maximum string size for service name. The default value is 64. Does not include the NULL terminator.
- **NX_MDNS_DOMAIN_NAME_MAX** Maximum string size for domain name. The default value is 16. Does not include the NULL terminator.
- **NX_MDNS_CONFLICT_COUNT** Maximum conflict count for host name or service name. The default value is 8.
- **NX_MDNS_RR_TTL_HOST** TTL value for resource records with host name, in second. The default value is 120 for 120s.
- **NX_MDNS_RR_TTL_OTHER** TTL value for other resource records, in second. The default value is 4500 for 75 minutes.
- **NX_MDNS_PROBING_TIMER_COUNT** The time interval, in ticks, between mDNS probing messages. The default value is 25 ticks for 250ms.
- **NX_MDNS_ANNOUNCING_TIMER_COUNT** The time interval, in ticks, between mDNS announcement messages. The default value is 25 ticks for 250ms.
- **NX_MDNS_GOODBYE_TIMER_COUNT** The time interval, in ticks, between repeated "goodbye" messages. The default value is 25 ticks for 250ms.
- **NX_MDNS_QUERY_MIN_TIMER_COUNT** The minimum time interval, in ticks, between two queries. The default value is 100 ticks for 1 second.
- **NX_MDNS_QUERY_MAX_TIMER_COUNT** The maximum time interval, in ticks, between two queries. The default value is 360000 for 60 seconds.
- **NX_MDNS_QUERY_DELAY_MIN** The minimum delay for sending first query, in ticks. The default value is 2 ticks for 20ms.
- **NX_MDNS_QUERY_DELAY_RANGE** The delay range for sending first query, in ticks. The default value is 10 ticks for 100ms.
- **NX_MDNS_RESPONSE_INTERVAL** The time interval, in ticks, in responding to a query to ensure an interval of at least 1s since the last time the record was multicast. The default value is 100 ticks.
- **NX_MDNS_RESPONSE_PROBING_TIMER_OUT** The time interval, in ticks, in responding to a probe queries to ensure an interval of at least 250ms since the last time the record was multicast. The default value is 25 ticks.
- **NX_MDNS_RESPONSE_UNIQUE_DELAY** The delay, in ticks, in responding to a query to a service that is unique to the local network. The default value is 1 tick for 10ms.
- **NX_MDNS_RESPONSE_SHARED_DELAY_MIN** The minimum delay, in ticks, in responding to a query to a shared resource. The default value is 2 ticks for 20ms.
- **NX_MDNS_RESPONSE_SHARED_DELAY_RANGE** The delay range, in ticks, in responding to a query to a shared resource. The default value is 10 ticks for 100ms.
- **NX_MDNS_RESPONSE_TC_DELAY_MIN** The minimum delay, in ticks, in responding to a query with TC bit. The default value is 40 ticks for 400ms.
- **NX_MDNS_RESPONSE_TC_DELAY_RANGE** The delay range, in ticks, in responding to a query with TC bit. The default value is 10 ticks for 100ms.
- **NX_MDNS_TIMER_COUNT_RANGE** When sending out mDNS responses, the packet contains responses that otherwise would be sent within this timer counter range. The timer count range is expressed in ticks. The default value is 12 for 120ms. This value allows a response to include messages that would be sent within the next 120ms range if each tick is 10ms.
- **NX_MDNS_PROBING_RETRANSMIT_COUNT** The number of retransmitted probing messages. The default value is 3.
- **NX_MDNS_GOODBYE_RETRANSMIT_COUNT** The number of retransmitted "goodbye" messages. The default value is 1.
- **NX_MDNS_POOF_MIN_COUNT** The number of queries that no multicast response, then the host may take this as an indication that the record may no longer be valid. The default value is 2.
- **NX_MDNS_POOF_TIME_COUNT** The time interval, in ticks, in deleting the record from the cache after seeing two or more of these queries, and seeing no multicast response containing the expected answer. The default value is 1000 ticks for 10 seconds.
- **NX_MDNS_RR_DELETE_DELAY_TIMER_COUNT** The delay for deleting a resource record when the TTL of this record is zero, in ticks, The default value is 100 ticks for 1 second.

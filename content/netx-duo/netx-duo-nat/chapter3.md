---
title: Chapter 3 - NAT configuration options
description: The following list includes all NAT configuration options and their functions described in detail.
---


Configurable options for the NetX Duo NAT API can be found in *nx_nat.h* with the exception of the first one, **NX_DISABLE_ERROR_CHECKING** which is found in *nx_nat.c*. The following list includes all options and their function described in detail:

- **NX_NAT_DISABLE_ERROR_CHECKING** This option if defined removes the basic NAT error checking. It is typically used after the application has been debugged. The default NetX Duo NAT status is defined (enabled).
- **NX_NAT_ENABLE_REPLACEMENT** This option if defined enables automatic replacement when NAT cache is full.
  > **Note:** Only replace the oldest non-TCP session.
- **NX_NAT_MIN_ENTRY_COUNT** This option sets the minimum count for translation entry. The default count is 3.
- **NX_NAT_TCP_SESSION_TIMEOUT** This option sets the timeout for translation entry for TCP Sessions. The default timeout is 24 hours.
- **NX_NAT_NON_TCP_SESSION_TIMEOUT** This option sets the timeout for translation entry for non-TCP Sessions. The default timeout is 240 seconds.
- **NX_NAT_START_TCP_PORT** This option sets the starting value for finding an unused TCP port to assign an outbound TCP packet. The default value is 20000.
- **NX_NAT_END_TCP_PORT** This option sets the upperlimit of TCP port to assign an outbound TCP packet. The default value is 30000.
- **NX_NAT_START_UDP_PORT** This option sets the starting value for finding an unused UDP port to assign an outbound UDP packet. The default value is 20000.
- **NX_NAT_END_UDP_PORT** This option sets the upperlimit of UDP port to assign an outbound UDP packet. The default value is 30000.
- **NX_NAT_START_ICMP_QUERY_ID** This option sets the starting value for finding an unused query ID to assign an outbound ICMP query packet. The default value is 20000.
- **NX_NAT_END_ICMP_QUERY_ID** This option sets the upperlimit of query IDs to assign an outbound ICMP query packet. The default value is 30000.

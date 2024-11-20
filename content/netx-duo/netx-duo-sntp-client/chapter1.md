---
title: Chapter 1 - Introduction to NetX Duo SNTP
description: The NetX Duo Simple Network Time Protocol (SNTP) is a protocol designed for synchronizing clocks over the Internet.
---


The NetX Duo Simple Network Time Protocol (SNTP) is a protocol designed for synchronizing clocks over the Internet. SNTP Version 4 is a simplified protocol based on the Network Time Protocol (NTP). It utilizes User Datagram Protocol (UDP) services to perform time updates in a simple, stateless protocol. Though not as complex as NTP, SNTP is highly reliable and accurate. In most places of the Internet of today, SNTP provides accuracies of 1-50 milliseconds, depending on the characteristics of the synchronization source and network paths. SNTP has many options to provide reliability of receiving time updates. Ability to switch to alternative servers, applying back off polling algorithms and automatic time server discovery are just a few of the means for an SNTP client to handle a variable Internet time service environment. What it lacks in precision it makes up for in simplicity and ease of implementation. SNTP is intended primarily for providing comprehensive mechanisms to access national time and frequency dissemination (e.g. NTP server) services.

## NetX Duo SNTP Client Requirements

The NetX Duo SNTP Client requires that an IP instance has already been created. In addition, UDP must be enabled on that same IP instance and should have access to the *well-known* port 123 for sending time data to an SNTP Server, although alternative ports will work as well. Broadcast clients should bind the UDP port their broadcast server is sending on, usually 123. The NetX Duo SNTP Client application must have one or more IP SNTP Server addresses.

## NetX Duo SNTP Client Limitations 

Precision in local time representation in NTP time updates handled by the SNTP Client API is limited to millisecond resolution.

The SNTP Client only holds a single SNTP Server address at any time. If that Server appears to be no longer valid, the application must stop the SNTP Client task, and reinitialize it with another SNTP server address, using either broadcast or unicast SNTP communication.

The SNTP Client does not support manycast.

NetX Duo SNTP Client does not support authentication mechanisms for verifying received packet data.

## NetX Duo SNTP Client Operation

RFC 4330 recommends that SNTP clients should operate only at the highest stratum of their local network and preferably in configurations where no NTP or SNTP client is dependent them for synchronization. Stratum level reflects the host position in the NTP time hierarchy where stratum 1 is the highest level (a root time server) and 15 is the lowest allowed level (e.g. Client). The SNTP Client default minimum stratum is 2.

The NetX Duo SNTP Client can operate in one of two basic modes, unicast or broadcast, to obtain time over the Internet. In unicast mode, the Client polls its SNTP Server on regular intervals and waits to receive a reply from that Server. When one is received, the Client verifies that the reply contains a valid time update by applying a set of 'sanity checks' recommended by RFC 4330. The Client then applies the time difference, if any, with the Server clock to its local clock. In broadcast mode, the Client merely listens for time update broadcasts and maintains its local clock after applying a similar set of sanity checks to verify the update time data. Sanity checks are described in detail in the **SNTP Sanity Checks** section below.

Before the Client can run in either mode, it must establish its operating parameters. This is done by calling either *nx_sntp_client_initialize_unicast* or *nx_sntp_client_initialize_broadcast* for unicast or broadcast modes, respectively. These serves set the time outs for maximum time lapse without a valid update, the limit on consecutive invalid updates received, a polling interval for unicast mode, operation mode e.g. unicast vs. broadcast, and SNTP Server.

If the maximum time lapse or maximum invalid updates received is exceeded, the SNTP Client continues to run but sets the current SNTP Server status to invalid. The application can poll the SNTP Client using the *nx_sntp_client_receiving_updates* service to verify the SNTP Server is still sending valid updates. If not, it should stop the SNTP Client thread using the *nx_sntp_client_stop* service and call either of the two initialize services to set another SNTP Server address. To restart the SNTP Client, the application calls *nx_sntp_client_run_broadcast* or *nx_sntp_client_run_unicast*. Note that the application can change SNTP Client operating mode in the initialize call to switch to unicast or broadcast as desired.

### Local Clock Operation

The SNTP time based on the number of seconds on the master NTP clock, or number of seconds elapsed in the first NTP epoch e.g. from Jan 1 **1900 00:00:00 to** Jan 1 **1999 00:00:00**. The significance of 01-01-1999 was when the last leap second occurred. This value is defined as follows:

\#define NTP_SECONDS_AT_01011999 0xBA368E80

Before the SNTP Client runs, the application can optionally initialize the SNTP Client local time for the Client to use as a baseline time. To do so, it must use the *nx_sntp_client_set_local_time* service. This takes the time in NTP format, seconds and fraction, where fraction is the milliseconds in the NTP condensed time. Ideally the application can obtain an SNTP time from an independent source. There is no API for converting year, month, date and time to an NTP time in the NetX Duo SNTP Client. For a description of NTP time format, refer to *RFC4330 "Simple Network Time Protocol (SNTP) Version 4 for IPv4, IPv6 and OSI".*

If no base local time is supplied when the SNTP Client starts up, the SNTP Client will accept the SNTP updates without comparing to its local time on the first update. Thereafter it will apply the maximum and minimum time update values to determine if it will modify its local time.

To obtain the SNTP Client local time, the application can use the *nx_sntp_client_get_local_time_extended* service.

### SNTP Sanity Checks 

The Client examines the incoming packet for the following criteria:

  - Source IP address must match the current server IP address.

  - Sender source port must match with the current server source port.

  - Packet length must be the minimum length to hold an SNTP time message.

Next, the time data is extracted from the packet buffer to which the Client then applies a set of specific 'sanity checks':

  - The Leap Indicator set to 3 indicates the Server is not synchronized. The Client should attempt to find an alternative server.

  - A stratum field set to zero is known as a Kiss of Death (KOD) packet. The SMTP Client KOD handler for this situation is a user defined callback. The small example demo file contains a simple KOD handler for this situation. The Reference ID field optionally contains a code indicating the reason for the KOD reply. At any rate, the KOD handler must indicate how to handle receiving a kiss of death from the SNTP Server. Typically it will want to reinitialize the SNTP Client with another SNTP Server.

  - The Server SNTP version, stratum and mode of operation must be matched to the Client service.

  - If the Client is configured with a server clock dispersion maximum, the Client checks the server clock dispersion on the first update received only, and if it exceeds the Client maximum, the Client rejects the Server.

  - The Server time stamp fields must also pass specific checks. For the unicast Server, all time fields must be filled in and cannot be NULL. The Origination time stamp must equal the Transmit time stamp in the Client's SNTP time message request. This protects the Client from malicious intruders and rogue Server behavior. The broadcast Server need only fill in the Transmit time stamp. Since it does not receive anything from the Client it has no Receive or Origination fields to fill in.

    A failed sanity check brands a time update as an invalid time update. The SNTP Client sanity check service tracks the number of consecutive invalid time updates received from the same Server.

When SNTP Client thread task checks the validity of an SNTP packet for applying to the local SNTP Client time, it increments the count of the SNTP Client `nx_sntp_client_invalid_time_updates.` It also returns an error status to the caller but this is all internal processing so it is not immediately visible to the application. The way to detect failed time updates is to query the value of the SNTP Client `nx_sntp_client_invalid_time_updates` after receiving SNTP Server time updates.

If the Server time update passes the sanity checks, the Client then attempts to process the time data to its local time. If the Client is configured for round trip calculation, e.g. the time from sending an update request to the time one is received, the round trip time is calculated. This value is halved and then added to the Server's time.

Next, if this is the first update received from the current SNTP Server, the SNTP Client determines if it should ignore the difference between the Server and Client local time. Thereafter all updates from the SNTP Server are evaluated for the difference with the Client local time.

The difference between Client and Server time is compared with `NX_SNTP_CLIENT_MAX_TIME_ADJUSTMENT`. If it exceeds this value, the data is thrown out. If the difference is less than the `NX_SNTP_CLIENT_MIN_TIME_ADJUSTMENT` the difference is considered too small to require adjustment.

Passing all these checks, the time update is then applied to the SNTP Client with some corrections for delays in internal SNTP Client processing.

### SNTP Asynchronous Unicast Requests 

The SNTP Client allows the host application to asynchronously send a unicast request for the current time from the NTP server.

```C
UINT _nx_sntp_client_request_unicast_time(
NX_SNTP_CLIENT *client_ptr, 
UINT wait_option)
```

The wait option is the expiration to wait for a response.

If the NTP Server responds, the packet is subjected to the same processing and sanity checks as described in the previous section before updating the SNTP Client local time.

If the call returns successful completion, the application can call *nx_sntp_client_utility_display_date_time* or *nx_sntp_client_get_local_time_extended* for the updated local time.

These unicast requests do not interfere with the normal SNTP Client schedule for sending the next unicast request, or if in broadcast mode, when to expect the next NTP broadcast.

### Periodic Local Time Updates

The maximum adjustment to the local time is set in the NX_SNTP_CLIENT_MAX_TIME_ADJUSTMENT option (in milliseconds). The polling update interval for unicast SNTP Client operations is set in the NX_SNTP_CLIENT_UNICAST_POLL_INTERVAL option (in seconds). If the polling interval is greater than the maximum adjustment, then subsequent server updates after the first server update will be rejected. To prevent this, the SNTP Client will update the local time periodically defined as NX_SNTP_UPDATE_TIMEOUT_INTERVAL. 

If there is a difference in time between the on board RTC and the server time (which the SNTP Client local time should be set to), the RTC should be synched to the SNTP Client time (we do not demonstrate that in this User Guide).

Since SNTP server updates should not occur more often than once per hour, it is not useful to poll the SNTP Client for server updates or server status more often than that. However, the SNTP Client should update its local clock often enough not to fall further than the maximum time adjustment parameter NX_SNTP_CLIENT_MAX_TIME_LAPSE.

Alternatively, the maximum adjustment NX_SNTP_CLIENT_MAX_TIME_LAPSE can be set to greater than the unicast polling update (or anticipated broadcast intervals). The latter eliminates the need for an independent real time clock. However, the intention of SNTP protocol is to avoid total reliance on either local RTC or network time updates. Further, the SNTP Server updates are intended to prevent drift in the local time clock.

## Multiple Network Interfaces

NetX Duo SNTP Client can be configured to run on secondary networks as long as those networks are registered with the IP instance. See the NetX Duo User Guide for more information on how to register secondary networks.

In the *nx_sntp_client_create* call, set the third input, iface_index, to the index of the network for the SNTP Client to receive time updates on. The primary interface is always at index 0. NetX Duo SNTP Client cannot support time updates simultaneously on multiple network interface.

## SNTP and NTP RFCs

NetX Duo SNTP client is compliant with RFC4330 "Simple Network Time Protocol (SNTP) Version 4 for IPv4, IPv6 and OSI" and related RFCs.
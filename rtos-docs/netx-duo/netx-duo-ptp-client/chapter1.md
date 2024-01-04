---
title: Chapter 1 - Introduction to NetX Duo PTP Client
description: This chapter contains an introduction to the NetX Duo PTP Client.
---

# Chapter 1 - Introduction to NetX Duo PTP Client

The NetX Duo PTP implements the client part of the Precision Time Protocol (PTP) version 2, IEEE 1588-2008. It is used to synchronize clocks among MCUs on the local network by communicating each other via IPv4 or IPv6 multicast packets.

## NetX Duo PTP Client Setup

In order to function properly, the PTP client package requires that a NetX Duo IP instance has already been created.

By default, the PTP client joins IPv4 multicast group. In order to run the PTP client on an IPv6 network, `NX_ENABLE_IPV6_MULTICAST` must be defined when building NetX Duo library.

When creating the PTP client, the application must provide a callback function to handle timestamps of incoming and outgoing packets. To achieve high resolution, we recommend applications to generate timestamps using a high resolution timer. To run the PTP on simulator, a software-based implementation `nx_ptp_client_soft_clock_callback` is provided.

The PTP client requires a packet pool for transmitting PTP messages. The payload size of packet pool must be no less than `NX_UDP_PACKET + NX_PTP_CLIENT_PACKET_DATA_SIZE`, which is 108 bytes for IPv4, and 128 bytes if IPv6 is enabled.

After creating the PTP Client, the application can start the PTP client. The synchronization is done in the PTP helper thread. An event callback function can be specified. It will be invoked when any one of the following events happen.
* A master is selected; 
* The time is calibrated.
* A master times out.

## NetX Duo PTP Client Messages

The NetX Duo PTP client implements the delay request-response mechanism only. The PTP client  opens two UDP ports. *319* for **event** message and *320* for **general** message. There are five types of message for this mechanism.

* **Announce**. This is an event message. It is used for master clock selection.
* **Sync**. This is an event message. It is used to send origin timestamp and calculate the path delay from master to client.
* **Follow_Up**. This is a general message. It is optional and used to correct the origin timestamp in related Sync message. It is sent only when the two step bit in Sync flag is set.
* **Delay_Req**. This is an event message. It is sent from PTP client to calculate the path delay from client to master, on receiving Delay_Resp.
* **Delay_Resp**. This is an event message. It is sent from master to client to calculate the path delay from client to master.

*Note, "best master clock" algorithm is not implemented. Only the first master clock is accepted when the PTP client is in listening state.*

## NetX Duo PTP Client Clock Callback
To synchronize clock from master, PTP client needs a local clock. A clock callback function is passed into PTP client during creation. The format of the clock callback routine is  defined below.
```C
UINT ptp_clock_callback(
    NX_PTP_CLIENT *client_ptr, 
    UINT operation,
    NX_PTP_TIME *time_ptr, 
    NX_PACKET *packet_ptr,
    VOID *callback_data);
```
The input parameters are defined as follows:
* *client_ptr* points to PTP client.
* *operation* specifies the callback operation, valid values are defined as shown in the list below.
  * **NX_PTP_CLIENT_CLOCK_INIT** Initialize clock.
  * **NX_PTP_CLIENT_CLOCK_SET** Set current timestamp specified by `time_ptr`.
  * **NX_PTP_CLIENT_CLOCK_GET** Return current timestamp to `time_ptr`.
  * **NX_PTP_CLIENT_CLOCK_PACKET_TS_EXTRACT** Extract timestamp from `packet_ptr` to `time_ptr`.
  * **NX_PTP_CLIENT_CLOCK_ADJUST** Adjust current timestamp less than *1* second.
  * **NX_PTP_CLIENT_CLOCK_PACKET_TS_PREPARE** Mark the `packet_ptr` which requires to notify PTP client the timestamp when it is transmitted.
  * **NX_PTP_CLIENT_CLOCK_SOFT_TIMER_UPDATE** Update soft timer. It can be ignored by hardware clock.
* *time_ptr* points to timestamp.
* *packet_ptr* points to packet.
* *callback_data* points to opaque callback data.

The NX_PTP_TIME data type is defined as follows.
```C
typedef struct NX_PTP_TIME_STRUCT
{
    /* The MSB of the number of seconds */
    LONG  second_high;

    /* The LSB of the number of seconds */
    ULONG second_low;

    /* The number of nanoseconds */
    LONG  nanosecond;
} NX_PTP_TIME;
```

## NetX Duo PTP Client Event Callback
The event callback function can be setup to notify application the state of PTP client. The format of the event callback routine is defined as shown below.
```C
UINT event_callback(
    NX_PTP_CLIENT *client_ptr, 
    UINT event, 
    VOID *event_data, 
    VOID *callback_data);
```
The input parameters are.
* *client_ptr* points to PTP client.
* *event* specifies the callback event, valid values are defined as:
  * **NX_PTP_CLIENT_EVENT_MASTER** A master clock is selected.
  * **NX_PTP_CLIENT_EVENT_SYNC** PTP client is calibrated.
  * **NX_PTP_CLIENT_EVENT_TIMEOUT** Master clock is timeout.
* *event_data* specifies the data related to event. When event is **NX_PTP_CLIENT_EVENT_MASTER**, event_data points to `NX_PTP_CLIENT_MASTER` instance. When event is **NX_PTP_CLIENT_EVENT_SYNC**, event data points to `NX_PTP_CLIENT_SYNC` instance.

## NetX Duo PTP Client Communication
As mentioned previously, NetX Duo PTP client only supports delay request-response mechanism. This mechanism measures the mean path delay between the client and the master clock as below:
1. Client receives *Announce* message from master and select it as best master.
1. Client receives *Sync* message from master. The timestamp *t1* is the origin timestamp in this message. The timestamp *t2* is read from local clock when this message is received.
1. Client receives *Follow_Up* message from master. This message is optional and valid only when two step in *Sync* flag is set. Then the timestamp *t1* is updated to the origin timestamp in this message.
1. Client sends *Delay_Req* message to master. The timestamp *t3* is read from local clock when the message is transmitted.
1. Client receives *Delay_Resp* message from master. The timestamp *t4* is the receive timestamp in this message.

The mean path delay is computed as shown below.
```C
<meanPathDelay>=[(t2-t1)+(t4-t3)]/2
```
The offset from master is computed as shown below.
```C
<offsetFromMaster>=client_clock-master_clock
                  =(t2-t1)-<meanPathDelay>
```

> **Note:** *The ***correctionField*** is ignored during the calculation.*
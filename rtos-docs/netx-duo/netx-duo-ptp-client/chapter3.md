---
title: Chapter 3 - Description of NetX Duo PTP Client Services
description: This chapter contains a description of all NetX Duo PTP client services in alphabetical order.
---

# Chapter 3 - Description of NetX Duo PTP Client Services

This chapter contains a description of all NetX Duo PTP client services (listed below) in alphabetical order.

> **Note:** *In the **Return Values** section in the following API function descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.*

## nx_ptp_client_create

Create a PTP client instance.

### Prototype

```C
UINT nx_ptp_client_create(
    NX_PTP_CLIENT *client_ptr, 
    NX_IP *ip_ptr, 
    UINT interface_index,
    NX_PACKET_POOL *packet_pool_ptr, 
    UINT thread_priority, 
    UCHAR *thread_stack, 
    UINT stack_size,
    NX_PTP_CLIENT_CLOCK_CALLBACK clock_callback, 
    VOID *clock_callback_data);
```

### Description

This service creates an instance of the PTP client.

Note that the  application must first create an IP instance and a packet pool for the PTP client to transmit packets. For the packet pool, application may use the same packet pool in the IP instance; or it can create a dedicated packet pool for PTP client.  The dedicated packet pool approach has the advantage of using small packets (128 bytes packets if IPv6 is used, or 108 bytes for IPv4-only).

### Input Parameters
- *client_ptr*: Pointer to PTP client to create
- *ip_ptr*: Pointer to IP instance
- *interface_index*: Index of PTP network interface
- *packet_pool_ptr*: Pointer to client packet pool
- *thread_priority*:  Priority of PTP thread
- *thread_stack*: Pointer to thread stack
- *stack_size*: Size of thread stack
- *clock_callback*: PTP clock callback
- *clock_callback_data*: Data for the clock callback

### Return Values
* **NX_SUCCESS** (0x00) Client successfully created
* **NX_PTP_CLIENT_INSUFFICIENT_PACKET_PAYLOAD** (0xD04) Packet payload too small
* **NX_PTP_CLIENT_CLOCK_CALLBACK_FAILURE** (0xD05) Failure on clock callback
* **status** Status completion of NetX Duo and ThreadX service calls
* NX_PTR_ERROR (0x07) Invalid input pointer parameter
* NX_INVALID_INTERFACE (0x4C) Invalid interface

### Allowed From
Threads

### Example
```C
/* Create the PTP client instance */
status = nx_ptp_client_create(&ptp_client, &ip_0, 0, &pool_0, 
                              PTP_THREAD_PRIORITY, (UCHAR *)ptp_stack, sizeof(ptp_stack),
                              clock_callback, NX_NULL);

/* If the client was successfully created, status = NX_SUCCESS. */
```

## nx_ptp_client_delete

Deletes a PTP client instance.

### Prototype

```C
UINT nx_ptp_client_delete(NX_PTP_CLIENT *client_ptr);
```

### Description

This service deletes an instance of the PTP client.

### Input Parameters
- *client_ptr*: Pointer to PTP client to delete

### Return Values
* **NX_SUCCESS** (0x00) Client successfully deleted
* NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From
Threads

### Example
```C
/* Delete the PTP client instance */
status = nx_ptp_client_delete(&ptp_client);

/* If the client was successfully deleted, status = NX_SUCCESS. */
```

## nx_ptp_client_master_info_get

Get master clock information.

### Prototype

```C
UINT nx_ptp_client_master_info_get(
    NX_PTP_CLIENT_MASTER *master_ptr, 
    NXD_ADDRESS *address, 
    UCHAR **port_identity, 
    UINT *port_identity_length, 
    UCHAR *priority1, 
    UCHAR *priority2, 
    UCHAR *clock_class,
    UCHAR *clock_accuracy, 
    USHORT *clock_variance, 
    UCHAR **grandmaster_identity,
    UINT *grandmaster_identity_length, 
    USHORT *steps_removed, 
    UCHAR *time_source);
```

### Description
This service gets information of master clock. The master control block is passed to user application through event callback function.

### Input Parameters
- *master_ptr*: Pointer to PTP master clock
- *address*: Address of master clock
- *port_identity*: PTP master port and identity
- *port_identity_length*: Length of PTP master port and identity
- *priority1*: Priority1 of PTP master clock
- *priority2*: Priority2 of PTP master clock
- *clock_class*: Class of PTP master clock
- *clock_accuracy*: Accuracy of PTP master clock
- *clock_variance*: Variance of PTP master clock
- *grandmaster_identity*: Identity of grandmaster clock
- *grandmaster_identity_length*: Length of grandmaster Identity
- *steps_removed*: Steps removed from PTP header
- *time_source*: The source of timer used by grandmaster clock

### Return Values
* **NX_SUCCESS** (0x00) Get master clock information successfully
* NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From
Threads

### Example
```C
static UINT ptp_event_callback(NX_PTP_CLIENT *ptp_client_ptr, UINT event, VOID *event_data, VOID *callback_data)
{
NXD_ADDRESS address;
UCHAR *port_identity;
UINT port_identity_length;
UCHAR priority1, priority2;
UCHAR clock_class, clock_accuracy;
USHORT clock_variance;
UCHAR *grandmaster_identity;
UINT grandmaster_identity_length;
USHORT steps_removed;
UCHAR time_source;

    switch (event)
    {
        case NX_PTP_CLIENT_EVENT_MASTER:
        {
            status = nx_ptp_client_master_info_get((NX_PTP_CLIENT_MASTER *)event_data, 
                                                   &address, &port_identity,
                                                   &port_identity_length, &priority1, 
                                                   &priority2, &clock_class,
                                                   &clock_accuracy, &clock_variance, 
                                                   &grandmaster_identity,
                                                   &grandmaster_identity_length, 
                                                   &steps_removed, &time_source);

            /* If the master clock information was successfully get, status = NX_SUCCESS. */
            break;
        }

        /* Other event process. */
    }
}
```

## nx_ptp_client_packet_timestamp_notify

Notify PTP client the timestamp of the packet.

### Prototype

```C
VOID nx_ptp_client_packet_timestamp_notify(
    NX_PTP_CLIENT *client_ptr, 
    NX_PACKET *packet_ptr, 
    NX_PTP_TIME *timestamp_ptr);
```

### Description
This service notifies the PTP client that packet is transmitted with timestamp. This service is designed for network driver and invoked when the packet is transmitted. The timestamp is usually generated by hardware.

### Input Parameters
- *client_ptr*: Pointer to PTP client to create
- *packet_ptr*: Pointer to PTP packet that is transmitted
- *timestamp_ptr*: Pointer to timestamp of PTP packet

### Return Values
None

### Allowed From
Threads

### Example
```C
/* Call notification callback */
nx_ptp_client_packet_timestamp_notify(client_ptr, packet_ptr, &ts);
```

## nx_ptp_client_soft_clock_callback

Software implementation of a PTP clock.

### Prototype

```C
UINT nx_ptp_client_soft_clock_callback(
    NX_PTP_CLIENT *client_ptr, 
    UINT operation,
    NX_PTP_TIME *time_ptr, 
    NX_PACKET *packet_ptr,
    VOID *callback_data);
```

### Description
This callback function serves as a simulated low resolution clock source for PTP. This routine is provided as a reference and cannot be used for production.

### Input Parameters
- *client_ptr*: Pointer to PTP client to create
- *operation*: Callback operation, valid values are defined as:
  * **NX_PTP_CLIENT_CLOCK_INIT** Initialize clock.
  * **NX_PTP_CLIENT_CLOCK_SET** Set current timestamp specified by `time_ptr`.
  * **NX_PTP_CLIENT_CLOCK_GET** Return current timestamp to `time_ptr`.
  * **NX_PTP_CLIENT_CLOCK_PACKET_TS_EXTRACT** Extract timestamp from `packet_ptr` to `time_ptr`.
  * **NX_PTP_CLIENT_CLOCK_ADJUST** Adjust current timestamp less than *1* second.
  * **NX_PTP_CLIENT_CLOCK_PACKET_TS_PREPARE** Mark the `packet_ptr` which requires to notify PTP client the timestamp when it is transmitted.
  * **NX_PTP_CLIENT_CLOCK_SOFT_TIMER_UPDATE** Update soft timer. It can be ignored by hardware clock.
- *time_ptr*: Pointer to timestamp.
- *packet_ptr*: Pointer to packet.
- *callback_data*: Pointer to opaque callback data.

### Return Values
* **NX_SUCCESS** (0x00) Operation successfully
* **NX_PTP_PARAM_ERROR** (0xD03) Invalid parameter

### Allowed From
PTP internal

### Example
```C/* Create the PTP client instance */
status = nx_ptp_client_create(&ptp_client, &ip_0, 0, &pool_0, 
                              PTP_THREAD_PRIORITY, (UCHAR *)ptp_stack, sizeof(ptp_stack),
                              nx_ptp_client_soft_clock_callback, NX_NULL);

/* If the client was successfully created, status = NX_SUCCESS. */
```

## nx_ptp_client_start

Start PTP client.

### Prototype

```C
UINT nx_ptp_client_start(
    NX_PTP_CLIENT *client_ptr, 
    UCHAR *client_port_identity_ptr, 
    UINT client_port_identity_length,
    UINT domain, 
    UINT transport_specific, 
    NX_PTP_CLIENT_EVENT_CALLBACK event_callback,
    VOID *event_callback_data)
```

### Description
This service starts a previously created PTP client instance.

### Input Parameters
- *client_ptr*: Pointer to PTP client to create
- *client_port_identity_ptr*: Pointer to client port and identity, it can be NULL
- *client_port_identity_length*: Length of client port and identity. It must be 0 if client_port_identity_ptr is NULL or NX_PTP_CLOCK_PORT_IDENTITY_SIZE (10)
- *domain*: PTP clock domain
- *transport_specific*: 4 bits of transport specific
- *event_callback*: Callback function invoked on event
- *event_callback_data*: Data for the event callback

### Return Values
* **NX_SUCCESS** (0x00) Client successfully started
* **NX_PTP_CLIENT_ALREADY_STARTED** (0xD02) PTP client already started
* **status** Status completion of NetX Duo and ThreadX service calls
* NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From
Threads

### Example
```C
status = nx_ptp_client_start(&ptp_client, NX_NULL, 0, 0, 0, ptp_event_callback, NX_NULL);

/* If the client was successfully started, status = NX_SUCCESS. */
```

## nx_ptp_client_stop

Stop PTP client.  After the PTP client is stopped, it does not process PTP packets, nor does it transmit PTP packets.

### Prototype

```C
UINT nx_ptp_client_stop(NX_PTP_CLIENT *client_ptr);
```

### Description
This service stops a previously created and started PTP client instance.

### Input Parameters
- *client_ptr*: Pointer to PTP client to stop

### Return Values
* **NX_SUCCESS** (0x00) Client successfully stopped
* **NX_PTP_CLIENT_NOT_STARTED** (0xD01) Client not started
* NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From
Threads

### Example
```C
status = nx_ptp_client_stop(&ptp_client);

/* If the client was successfully stopped, status = NX_SUCCESS. */
```

## nx_ptp_client_sync_info_get

Get Sync information.

### Prototype

```C
UINT nx_ptp_client_sync_info_get(
    NX_PTP_CLIENT_SYNC *sync_ptr, 
    USHORT *flags, 
    SHORT *utc_offset);
```

### Description
This service gets information of Sync message. The Sync control block is passed to user application through event callback function.

### Input Parameters
- *client_ptr*: Pointer to PTP client to create
- *flags*: Flags in Sync message
- *utc_offset*: Offset between TAI and UTC

### Return Values
* **NX_SUCCESS** (0x00) Get Sync information successfully
* NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From
Threads

### Example
```C
static UINT ptp_event_callback(NX_PTP_CLIENT *ptp_client_ptr, UINT event, VOID *event_data, VOID *callback_data)
{
USHORT utc_offset;

    switch (event)
    {
        case NX_PTP_CLIENT_EVENT_SYNC:
        {
            nx_ptp_client_sync_info_get((NX_PTP_CLIENT_SYNC *)event_data, NX_NULL, &utc_offset);

            /* If the Sync information was successfully get, status = NX_SUCCESS. */
            break;
        }

        /* Other event process. */
    }
}
```

## nx_ptp_client_time_get

Get current time.

### Prototype

```C
UINT nx_ptp_client_time_get(
    NX_PTP_CLIENT *client_ptr, 
    NX_PTP_TIME *time_ptr);
```

### Description
This service gets the current value of the PTP clock. It is available no matter PTP client is synchronized with master clock or not.

### Input Parameters
- *client_ptr*: Pointer to PTP client to create
- *time_ptr*: Pointer to PTP time

### Return Values
* **NX_SUCCESS** (0x00) Client successfully created
* NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From
Threads

### Example
```C
/* Get the PTP clock */
nx_ptp_client_time_get(&ptp_client, &tm);
```

## nx_ptp_client_time_set

Set current time.

### Prototype

```C
UINT nx_ptp_client_time_set(
    NX_PTP_CLIENT *client_ptr, 
    NX_PTP_TIME *time_ptr);
```

### Description
This service sets the current value of the PTP clock. It must be invoked before PTP client starts.

### Input Parameters
- *client_ptr*: Pointer to PTP client to create
- *time_ptr*: Pointer to PTP time

### Return Values
* **NX_SUCCESS** (0x00) Client successfully created
* **NX_PTP_CLIENT_ALREADY_STARTED** (0xD02) PTP client already started
* NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From
Threads

### Example
```C
/* Set current time before PTP client started.  */
status = nx_ptp_client_time_set(&ptp_client, &tm);
```

## nx_ptp_client_utility_convert_time_to_date

Convert PTP time to a UTC date and time.

### Prototype

```C
UINT nx_ptp_client_utility_convert_time_to_date(
    NX_PTP_TIME *time_ptr, 
    LONG offset, 
    NX_PTP_DATE_TIME *date_time_ptr);
```

### Description
This service converts PTP time to a UTC date and time.

### Input Parameters
- *time_ptr*: Pointer to PTP time
- *offset*: Signed second offset to add the PTP time
- *date_time_ptr*: Pointer to resulting date

### Return Values
* **NX_SUCCESS** (0x00) Client successfully created
* **Pointer to resulting date** (0xD03) Invalid input parameter
* NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From
Threads

### Example
```C
/* convert PTP time to UTC date and time */
status = nx_ptp_client_utility_convert_time_to_date(&tm, -ptp_utc_offset, &date);

/* If the time was successfully converted, status = NX_SUCCESS. */
```

## nx_ptp_client_utility_time_diff

Diff two PTP times.

### Prototype

```C
UINT nx_ptp_client_utility_time_diff(
    NX_PTP_TIME *time1_ptr, 
    NX_PTP_TIME *time2_ptr, 
    NX_PTP_TIME *result_ptr);
```

### Description
This service computes the difference between two PTP times.

### Input Parameters
- *time1_ptr*: Pointer to first PTP time
- *time2_ptr*: Pointer to second PTP time
- *result_ptr*: Pointer to result time1-time2

### Return Values
* **NX_SUCCESS** (0x00) Client successfully created
* NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From
Threads

### Example
```C
/* Diff time.  */
status = nx_ptp_client_utility_time_diff(&time1, &time2, &result);

/* If the calculation was successfully, status = NX_SUCCESS. */
```

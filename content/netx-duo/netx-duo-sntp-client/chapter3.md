---
title: Chapter 3 - Description of NetX Duo SNTP Client Services
description: This chapter contains a description of all NetX Duo SNTP Client services (listed below) in alphabetic order.
---


This chapter contains a description of all NetX Duo SNTP Client services (listed below) in alphabetic order.

In the **Return Values** section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## nx_sntp_client_create

Create an SNTP Client

### Prototype

```C
UINT nx_sntp_client_create(
    NX_SNTP_CLIENT *client_ptr, 
    NX_IP *ip_ptr,  
    UINT iface_index, 
    NX_PACKET_POOL *packet_pool_ptr, 
    UINT (*leap_second_handler)(
        NX_SNTP_CLIENT *client_ptr, 
        UINT indicator), 
    UINT (*kiss_of_death_handler)(
        NX_SNTP_CLIENT *client_ptr, 
        NX_SNTP_TIME_MESSAGE *server_time_msg),
    VOID (random_number_generator)(
        struct NX_SNTP_CLIENT_STRUCT *client_ptr, 
        ULONG *rand));
    
```

### Description

This service creates an SNTP Client instance.

### Input Parameters

- *client_ptr*: Pointer to SNTP Client control block
- *ip_ptr*: Pointer to Client IP instance
- *iface_index*: Index to SNTP network interface
- *packet_pool_ptr*: Pointer to Client packet pool
- *leap_second_handler*: Callback for application response to impending leap second
- *kiss_of_death_handler*: Callback for application response to receiving Kiss of Death packet
- *random_number_generator*: Callback to random number generator service

### Return Values

- **NX_SUCCESS** (0x00) Successful Client creation

- **NX_SNTP_INSUFFICIENT_PACKET_PAYLOAD** (0xD2A) Invalid non pointer input

- NX_PTR_ERROR (0x07) Invalid pointer input

- NX_INVALID_INTERFACE (0x4C) Invalid network interface

### Allowed From

Initialization, Threads

### Example

```C
/* Create the SNTP Client on the primary interface. */
UINT iface_index = 0;
status =  nx_sntp_client_create(&demo_client, 
                     iface_index, &client_ip, 
&client_packet_pool, leap_second_handler, 
                    kiss_of_death_handler, 
NULL /* no random_number_generator callback */);

/* If status is NX_SUCCESS an SNTP Client instance was successfully
   created.  */

```

## nx_sntp_client_delete

Delete an SNTP Client

### Prototype

```C
UINT nx_sntp_client_delete(NX_SNTP_CLIENT *client_ptr);
```

### Description

This service deletes an SNTP Client instance.

### Input Parameters

- *client_ptr*: Pointer to SNTP Client control block

### Return Values

- **NX_SUCCESS** (0x00) Successful Client creation

- NX_PTR_ERROR (0x07) Invalid pointer input

- NX_CALLER_ERROR (0x11) Invalid caller of service

### Allowed From

Threads

### Example

```C
/* Delete the SNTP Client. */
status =  nx_sntp_client_delete(&demo_client);

/* If status is NX_SUCCESS an SNTP Client 
   instance was successfully deleted.  */

```

## nx_sntp_client_get_local_time

Get the SNTP Client local time

### Prototype

```C
UINT nx_sntp_client_get_local_time(
    NX_SNTP_CLIENT *client_ptr , 
    ULONG *seconds, 
    ULONG *fraction, 
    CHAR *buffer);

```

### Description

This service gets the SNTP Client local time with an option buffer pointer input to receive the data in string message format.

This service is deprecated. Developers are encouraged to migrate to *nx_sntp_client_get_local_time_extended*().

### Input Parameters

- *client_ptr*: Pointer to SNTP Client control block

- *seconds*: Pointer to local time seconds

- *fraction*: Local time fraction component

- *buffer*: Pointer to buffer to write time data

### Return Values

- **NX_SUCCESS** (0x00) Successful Client creation

- NX_PTR_ERROR (0x07) Invalid pointer input

- NX_CALLER_ERROR (0x11) Invalid caller of service

### Allowed From

Threads

### Example

```C
/* Get the SNTP Client local time without the 
   string message option. */

ULONG base_seconds;
ULONG base_fraction;

status =  nx_sntp_client_get_local_time(&demo_client, 
                                       &base_seconds, 
              				           &base_fraction, 
                                       NX_NULL);
/* If status is NX_SUCCESS an SNTP Client time was successfully
   retrieved.  */

```

## nx_sntp_client_get_local_time_extended

Get the extended SNTP Client local time

### Prototype

```C
UINT nx_sntp_client_get_local_time_extended(
    NX_SNTP_CLIENT *client_ptr,
    ULONG *seconds, 
    ULONG *fraction, 
    CHAR *buffer
    UINT buffer_size);

```

### Description

This service gets the extended SNTP Client local time with an option buffer pointer input to receive the data in string message format.

### Input Parameters

- *client_ptr*: Pointer to SNTP Client control block

- *seconds*: Pointer to local time seconds

- *fraction*: Pointer to fraction component

- *buffer*: Pointer to buffer to write time data

- *buffer_size*: Length of buffer

### Return Values

- **NX_SUCCESS** (0x00) Successful Client creation

- NX_PTR_ERROR (0x07) Invalid pointer input

- NX_CALLER_ERROR (0x11) Invalid caller of service

- NX_SIZE_ERROR (0x09) Check buffer_size fail

### Allowed From

Threads

### Example

```C
/* Get the SNTP Client local time without the 
   string message option. */

#define BUFSIZE 50

ULONG seconds;
ULONG fraction;
CHAR  buffer[BUFSIZE];

status =  nx_sntp_client_get_local_time_extended(&demo_client, 
                                                &seconds, 
              				                    &fraction, 
                                                buffer, 
                                                BUFSIZE);

/* If status is NX_SUCCESS an SNTP Client 
   time was successfully retrieved.  */

```

## nx_sntp_client_initialize_broadcast

Initialize the Client for broadcast operation

### Prototype

```C
UINT nx_sntp_client_initialize_broadcast(
    NX_SNTP_CLIENT *client_ptr,
    ULONG multicast_server_address, 
    ULONG broadcast_time_servers);


```

### Description

This service initializes the Client for broadcast operation by setting the the SNTP Server IP address and initializing SNTP startup parameters and timeouts. If both multicast and broadcast addresses are non-null, the multicast address is selected. If both addresses are null an error is returned. Note this supports IPv4 server addresses only.

### Input Parameters

- *client_ptr*: Pointer to SNTP Client control block

- *multicast_server_address*: SNTP multicast address

- *broadcast_time_server*: SNTP server broadcast address

### Return Values

- **NX_SUCCESS** (0x00) Successful Client Creation

- **NX_INVALID_PARAMETERS** (0x4D) Invalid non pointer input

- NX_PTR_ERROR (0x07) Invalid pointer input

- NX_CALLER_ERROR (0x11) Invalid caller of service

### Allowed From

Initialization, Threads

### Example

```C
/* Initialize the client for broadcast operation.  */
status =  nx_sntp_client_initialize_broadcast(client_ptr,0x0,
                            NX_NULL, IP_ADDRESS(192,2,2,255);

/* If status is NX_SUCCESS the Client 
   was successfully initialized.  */

```

## nxd_sntp_client_initialize_broadcast

Initialize the Client for IPv4 or IPv6 broadcast operation

### Prototype

```C
UINT nxd_sntp_client_initialize_broadcast(
    NX_SNTP_CLIENT *client_ptr,
    NXD_ADDRESS *multicast_server_address, 
    NXD_ADDRESS *broadcast_server_address);

```

### Description

This service initializes the Client for broadcast operation by setting up the SNTP Server IP address and initializing SNTP startup parameters and timeouts. If both broadcast and multicast address pointers are non null, the multicast address is selected. If both address pointers are null, an error is returned. This supports both IPv4 and IPv6 address types. Note that IPv6 does not support broadcast, so the broadcast address pointer is set to IPv6, an error is returned.

### Input Parameters

- *client_ptr*: Pointer to SNTP Client control block

- *multicast_server_address*: SNTP server multicast address

- *broadcast_server_address*: SNTP server broadcast address

### Return Values

- **NX_SUCCESS** (0x00) Client successfully initialized

- NX_SNTP_PARAM_ERROR (0xD0D) Invalid non pointer input

- NX_PTR_ERROR (0x07) Invalid pointer input

- NX_CALLER_ERROR (0x11) Invalid caller of service

### Allowed From

Initialization, Threads

### Example

```C
/* Initialize the client for broadcast operation.  */
NXD_ADDRESS broadcast_server;

Broadcast_server.nxd_ip_address = NX_IP_VERSION_V6;
Broadcast_server.nxd_ip_address.v6[0] = 0x20010db1;
Broadcast_server.nxd_ip_address.v6[1] = 0x0f101;
Broadcast_server.nxd_ip_address.v6[2] = 0x0;
Broadcast_server.nxd_ip_address.v6[3] = 0x101;

status =  nxd_sntp_client_initialize_broadcast(client_ptr,0x0,
                                  NX_NULL, &broadcast_server)


/* If status is NX_SUCCESS the Client 
   was successfully initialized.  */

```

## nx_sntp_client_initialize_unicast

Set up the SNTP Client to run in unicast

### Prototype

```C
UINT nx_sntp_client_initialize_unicast(
    NX_SNTP_CLIENT * client_ptr, 
    ULONG unicast_time_server);

```
### Description

This service initializes the Client for unicast operation by setting the SNTP Server IP address and initializing SNTP startup parameters and timeouts. Note this supports IPv4 server addresses only.

### Input Parameters

- *client_ptr*: Pointer to SNTP Client control block

- *unicast_time_server*: SNTP server IP address

### Return Values

- **NX_SUCCESS** (0x00) Client successfully initialized

- NX_INVALID_PARAMETERS (0x4D) Invalid non pointer input

- NX_PTR_ERROR (0x07) Invalid pointer input

- NX_CALLER_ERROR (0x11) Invalid caller of service

### Allowed From

Initialization, Threads

### Example

```C
/* Initialize the Client for unicast operation.  */
status =  nx_sntp_client_initialize_unicast(&client_ptr, 
                                 IP_ADDRESS(192,2,2,1));


/* If status is NX_SUCCESS the Client 
   is initialized for unicast operation.  */

```


## nxd_sntp_client_initialize_unicast

Set up the SNTP Client to run in IPv4 or IPv6 unicast

### Prototype

```C
UINT nxd_sntp_client_initialize_unicast(
    NX_SNTP_CLIENT * client_ptr, 
    NXD_ADDRESS *unicast_time_server);

```

### Description

This service initializes the Client for unicast operation by setting up the SNTP Server IP address and initializing SNTP startup parameters and timeouts. This supports both IPv4 and IPv6 address types.

### Input Parameters

- *client_ptr*: Pointer to SNTP Client control block

- *unicast_time_server*: SNTP server IP address

### Return Values

- **NX_SUCCESS** (0x00) Client successfully initialized

- NX_INVALID_PARAMETERS (0x4D) Invalid non pointer input

- NX_PTR_ERROR (0x07) Invalid pointer input

- NX_CALLER_ERROR (0x11) Invalid caller of service

### Allowed From

Initialization, Threads

### Example

```C
/* Initialize the Client for unicast operation.  */
NXD_ADDRESS unicast_server;

unicast _server.nxd_ip_address = NX_IP_VERSION_V6;
unicast _server.nxd_ip_address.v6[0] = 0x20010db1;
unicast _server.nxd_ip_address.v6[1] = 0x0f101;
unicast _server.nxd_ip_address.v6[2] = 0x0;
unicast _server.nxd_ip_address.v6[3] = 0x101;

status =  nxd_sntp_client_initialize_unicast(&client_ptr, 
                                        *unicast_server); 


/* If status is NX_SUCCESS the Client 
   is initialized for unicast operation.  */

```

## nx_sntp_client_receiving_updates

Indicate if Client is receiving valid updates

### Prototype

```C
UINT nx_sntp_client_receiving_updates(
    NX_SNTP_CLIENT *client_ptr, 
    UINT *receive_status);

```

### Description

This service indicates if the Client is receiving valid SNTP updates. If the maximum time lapse without a valid update or limit on consecutive invalid updates is exceeded, the receive status is returned as false. Note that the SNTP Client is still running and if the application wishes to restart the SNTP Client with another unicast or broadcast/multicast server it must stop the SNTP Client using the *nx_sntp_client_stop* service, reinitialize the Client using one of the initialize services with another server.

### Input Parameters

- *client_ptr*: Pointer to SNTP Client control block.

- *receive_status*: Pointer to indicator if Client is receiving valid updates.

### Return Values

- **NX_SUCCESS** (0x00) Client successfully received update status

- NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Initialization, Threads

### Example

```C
/* Determine if the SNTP Client is receiving valid updates.  */
UINT receive_status;

status =  nx_sntp_client_receiving_updates(client_ptr, 
                                     &receive_status);

/* If status is NX_SUCCESS and receive_status is NX_TRUE, 
   the client is currently receiving valid updates.  */

```

## nx_sntp_client_request_unicast_time

Send a unicast request directly to the NTP Server


### Prototype

```C
UINT nx_sntp_client_request_unicast_time(
    NX_SNTP_CLIENT *client_ptr, 
    UINT wait_option);
```

### Description

This service allows the application to directly send a unicast request to the NTP server asynchronously from the SNTP Client thread task. The wait option specifies how long to wait for a response. If successful, the application can use other SNTP Client services to obtain the latest time. See section **SNTP Asynchronous Unicast Requests** for more details.

### Input Parameters

- *client_ptr*: Pointer to SNTP Client control block.

- *Wait_option*: Wait option for NTP response in timer ticks.

### Return Values

- **NX_SUCCESS** (0x00) Client successfully sends and
receives unicast update

- **NX_SNTP_CLIENT_NOT_STARTED** (0xD0B) Client thread not started

- NX_PTR_ERROR (0x07) Invalid pointer input

- NX_CALLER_ERROR (0x11) Invalid caller of service

### Allowed From

Threads

### Example

```C
/* Determine if the SNTP Client is receiving valid updates.  */
UINT receive_status;

status =  nx_sntp_client_request_unicast_time(client_ptr, 400);

/* If status is NX_SUCCESS and receive_status is NX_TRUE, 
   the client is received a valid response to the unicast request.  */

```

## nx_sntp_client_run_broadcast

Run the Client in broadcast mode

### Prototype

```C
UINT nx_sntp_client_run_broadcast(NX_SNTP_CLIENT *client_ptr);
```

### Description

This service starts the Client in broadcast mode where it will wait to receive broadcasts from the SNTP server. If a valid broadcast SNTP message is received, the SNTP client timeout for maximum lapse without an update and count of consecutive invalid messages received are reset. If the either of these limits are exceeded, the SNTP Client sets the server status to invalid although it will still wait to receive updates. The application can poll the SNTP Client task for server status, and if invalid stop the SNTP Client and reinitialize it with another SNTP broadcast address. It can also switch to a unicast SNTP server.

### Input Parameters

- *client_ptr*: Pointer to SNTP Client control block.

### Return Values

- **status** Actual completion status

- **NX_SNTP_CLIENT_ALREADY_STARTED** (0xD0C) Client already started

- **NX_SNTP_CLIENT_NOT_INITIALIZED** (0xD01) Client not initialized

- NX_PTR_ERROR (0x07) Invalid pointer input

- NX_CALLER_ERROR (0x11) Invalid caller of service

### Allowed From

Threads

### Example

```C
/* Start Client running in broadcast mode.  */
status =  nx_sntp_client_run_broadcast(client_ptr);

/* If status is NX_SUCCESS, the client is successfully started.  */

```

## nx_sntp_client_run_unicast

Run the Client in unicast mode

### Prototype

```C
UINT nx_sntp_client_run_unicast(NX_SNTP_CLIENT *client_ptr);
```

### Description

This service starts the Client in unicast mode where it periodically sends a unicast request to its SNTP Server for a time update. If a valid SNTP message is received, the SNTP client timeout for maximum lapse without an update, initial polling interval and count of consecutive invalid messages received are reset. If the either of these limits are exceeded, the SNTP Client sets the Server status to invalid although it will still poll and wait to receive updates. The application can poll the SNTP Client task for server status, and if invalid stop the SNTP Client and reinitialize it with another SNTP unicast address. It can also switch to a broadcast SNTP server.

### Input Parameters

- *client_ptr*: Pointer to SNTP Client control block.

### Return Values

- **NX_SUCCESS** (0x00) Successfully started Client in
unicast mode

- **NX_SNTP_CLIENT_ALREADY_STARTED** (0xD0C) Client already started

- **NX_SNTP_CLIENT_NOT_INITIALIZED** (0xD01) Client not initialized

- NX_PTR_ERROR (0x07) Invalid pointer input

- NX_CALLER_ERROR (0x11) Invalid caller of service


### Allowed From

Threads

### Example

```C
/* Start the Client in unicast mode. */
status =  nx_sntp_client_run_unicast(client_ptr);

/* If status = NX_SUCCESS, the Client was successfully started.  */

```

## nx_sntp_client_set_local_time

Set the SNTP Client local time

### Prototype

```C
UINT nx_sntp_client_set_local_time(
    NX_SNTP_CLIENT *client_ptr , 
    ULONG seconds, ULONG fraction);

```

### Description

This service sets the SNTP Client local time with the input time, in SNTP format e.g. seconds and 'fraction' which is the format for putting fractions of a second in hexadecimal format. It is intended for updating the SNTP Client local time from an independent time keeper, e.g. a real time clock. The SNTP protocol is intended for SNTP time updates to keep local clock time from 'drifting'. SNTP server time updates can be, but are not intended to be the sole input to the SNTP Client local time if there is no independent time keeper on the application device.

This API can also be used to give the SNTP Client a base time before starting the SNTP Client thread. The SNTP Client local time is compared to received updates for valid time data. For the first time update received, there might be a very large discrepancy. Therefore there is an option for the SNTP Client to ignore the discrepancy on the first update. In this manner, the SNTP Client can start without a base time. Input time can be obtained from known epoch times (usually available on the Internet) and are computed as the number of seconds since January 1, 1900 (until 2036 when a new 'epoch' will be started.

### Input Parameters

- *client_ptr*: Pointer to SNTP Client control block

- *seconds*: Seconds component of the time input

- *fraction*: Subseconds component in the SNTP
fraction format

### Return Values

- **NX_SUCCESS** (0x00) Successfully set local time

- NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Initialization

### Example

```C
/* Set the SNTP Client local time. */
base_seconds =  0xd2c50b71;
base_fraction = 0xa132db1e;

status =  nx_sntp_client_set_local_time(&demo_client, 
                                        base_seconds, 
                                        base_fraction);

/* If status is NX_SUCCESS an SNTP Client time 
   was successfully set.  */

```

## nx_sntp_client_set_time_update_notify

Set the SNTP update callback

### Prototype

```C
UINT nx_sntp_client_set_time_update_notify(
    NX_SNTP_CLIENT *client_ptr, 
    VOID (time_update_cb)(
        NX_SNTP_TIME_MESSAGE *time_update_ptr,
        NX_SNTP_TIME *local_time)));

```

### Description

This service sets callback to notify the application when the SNTP Client receives a valid time update. It supplies the actual SNTP message and the SNTP Client's local time (usually the same) in NTP format. The application can use the NTP data directly or call the *nx_sntp_client_utility_display_date_time service* to convert the time to human readable format.

### Input Parameters

- *client_ptr*: Pointer to SNTP Client control block

- *time_update_cb*: Pointer to callback function

### Return Values

- **NX_SUCCESS** (0x00) Successfully set callback

- NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Initialization

### Example

```C
/* Set the SNTP Client time update callback. */
VOID client_time_update_notify(NX_SNTP_TIME_MESSAGE *time_update_ptr, 
                                           NX_SNTP_TIME *local time);

NX_SNTP_CLIENT demo_client;


status = nx_sntp_client_set_time_update_notify(&demo_client, 
                                            time_update_cb);

/* If status is NX_SUCCESS an SNTP Client 
   time update callback was successfully set.  */

```

## nx_sntp_client_stop

Stop the SNTP Client thread

### Prototype

```C
UINT nx_sntp_client_stop(NX_SNTP_CLIENT *client_ptr);
```

### Description

This service stops the SNTP Client thread. The SNTP Client thread tasks, which runs in an infinite loop, pauses on every iteration to release control of the SNTP Client state and allow applications to make API calls on the SNTP Client.

### Input Parameters

- *client_ptr*: Pointer to SNTP Client control block

### Return Values

- **NX_SUCCESS** (0x00) Successful stopped Client thread

- **NX_SNTP_CLIENT_NOT_STARTED** (0xDB) SNTP Client thread not
started

- NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Initialization, Threads

### Example

```C
/* Stop the SNTP Client. */
status =  nx_sntp_client_stop(&demo_client);

/* If status is NX_SUCCESS an SNTP 
   Client instance was successfully stopped.  */

```

## nx_sntp_client_utility_display_date_time

Convert an NTP Time to Date and Time string

### Prototype

```C
UINT nx_sntp_client_utility_display_date_time (
    NX_SNTP_CLIENT *client_ptr,
    CHAR *buffer,
    UINT length);

```

### Description

This service converts the SNTP Client local time to a year month date format and returns the date in the supplied buffer. The NX_SNTP_CURRENT_YEAR need not be the same year as the current Client time but it must be defined.

### Input Parameters

- *client_ptr Pointer to SNTP Client**

- *buffer*: Pointer to buffer to store date

- *length*: Size of input buffer

### Return Values

- **NX_SUCCESS** (0x00) Successful conversion

- **NX_SNTP_ERROR_CONVERTING_DATETIME** (0xD08) NX_SNTP_CURRENT_YEAR not defined or no local client time
established

- **NX_SNTP_INVALID_DATETIME_BUFFER** (0xD07) Insufficient buffer length


### Allowed From

Initialization, Threads

### Example

```C
/* Convert and display the Client's local time. */

status =  nx_sntp_client_utility_display_date_time(client_ptr , 
                                        buffer, sizeof(buffer));

/* If status is NX_SUCCESS, 
   date was successfully written to buffer.  */

```

## nx_sntp_client_utility_msecs_to_fraction

Convert milliseconds to an NTP fraction component

### Prototype

```C
UINT nx_sntp_client_utility_msecs_to_fraction (
    ULONG milliseconds,   
    ULONG *fraction);

```

### Description

This service converts the input milliseconds to the NTP fraction component. It is intended for use with applications that have a starting base time for the SNTP Client but not in NTP seconds/fraction format. The number of milliseconds must be less than 1000 to make a valid fraction.

### Input Parameters

- *milliseconds*: Milliseconds to convert

- *fraction*: Pointer to milliseconds converted to fraction

### Return Values

- **NX_SUCCESS** (0x00) Successful conversion

- **NX_SNTP_OVERFLOW_ERROR** (0xD32) Error converting time to a date

- NX_SNTP_INVALID_TIME (0xD30) Invalid SNTP data input

### Allowed From

Initialization, Threads

### Example

```C
/* Convert the milliseconds to a fraction. */


status =  nx_sntp_client_utility_msecs_to_fraction(milliseconds, 
                                                     &fraction);

/* If status is NX_SUCCESS, 
   data was successfully converted.  */

```

## nx_sntp_client_utility_usecs_to_fraction

Convert microseconds to an NTP fraction component

### Prototype

```C
UINT nx_sntp_client_utility_usecs_to_fraction (
    ULONG microseconds,   
    ULONG *fraction);

```
### Description

This service converts the input microseconds to the NTP fraction component. It is intended for use with applications that have a starting base time for the SNTP Client but not in NTP seconds/fraction format. The number of microseconds must be less than 1000000 to make a valid fraction.

### Input Parameters

- *microseconds*: Microseconds to convert

- *fraction*: Pointer to microseconds converted to fraction

### Return Values

- **NX_SUCCESS** (0x00) Successful conversion

- **NX_SNTP_OVERFLOW_ERROR** (0xD32) Error converting time to a date

- NX_SNTP_INVALID_TIME (0xD30) Invalid SNTP data input

### Allowed From

Initialization, Threads

### Example

```C
/* Convert the microseconds to a fraction. */


status =  nx_sntp_client_utility_msecs_to_fraction(microseconds, 
                                                     &fraction);

/* If status is NX_SUCCESS, data was successfully converted.  */

```

## nx_sntp_client_utility_fraction_to_usecs

Convert an NTP fraction component to microseconds

### Prototype

```C
UINT nx_sntp_client_utility_fraction_to_usecs(
    ULONG fraction,   
    ULONG *microseconds);

```

### Description

This service converts the input NTP fraction component to microseconds.

### Input Parameters

- *fraction*: Fraction to convert

- *microseconds*: Pointer to fraction converted to microseconds

### Return Values

- **NX_SUCCESS** (0x00) Successful conversion

- NX_SNTP_INVALID_TIME (0xD30) Invalid SNTP data input

### Allowed From

Initialization, Threads

### Example

```C
/* Convert the fraction to microseconds. */


status =  nx_sntp_client_utility_fraction_to_msecs(fraction, 
                                             &microseconds);

/* If status is NX_SUCCESS, data was successfully converted.  */
```

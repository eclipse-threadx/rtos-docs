---
title: Chapter 3 - Description of NetX Duo Telnet services
description: This chapter contains a description of all NetX Duo Telnet Services (listed below) in alphabetic order.
---

# Chapter 3 - Description of NetX Duo Telnet services

This chapter contains a description of all NetX Duo Telnet Services (listed below) in alphabetic order.

In the "Return Values" section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## nx_telnet_client_connect

Connect a Telnet Client with IPv4 address

### Prototype

```c
UINT nx_telnet_client_connect(
    NX_TELNET_CLIENT *client_ptr, 
    ULONG server_ip,
    UINT server_port, 
    ULONG wait_option);
```

### Description

This service attempts to connect the previously created Telnet Client instance to the Server at the specified IP and port using an IPv4 address for the Telnet Server. This service actually inserts the ULONG server IP address in an NXD_ADDRESS control block and sets the IP version to 4 before calling the *nxd_telnet_client_connect* service described below.

### Input Parameters

- *client_ptr*: Pointer to Telnet Client control block.
- *server_ip*: IPv4 Address of the Telnet Server.
- *server_port*: TCP Port of Server (Telnet Server is port 23).
- *wait_option*: Defines how long the service will wait for the Telnet Client connect. The wait options are defined as follows:
    - **timeout value**: (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the Telnet Server response.
    - **TX_WAIT_FOREVER**: (0xFFFFFFFF)Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the Telnet Server responds to the request.

### Return Values

- **NX_SUCCESS**: (0x00) Successful Client connect.
- **NX_TELNET_NOT_DISCONNECTED**: (0xF4) Client already connected.
- NX_PTR_ERROR: (0x07) Invalid Client pointer.
- NX_IP_ADDRESS_ERROR: (0x21) Invalid IP address.
- NX_CALLER_ERROR: (0x11) Invalid caller of service.

### Allowed From

Threads

### Example

```c
/* Connect the Telnet Client instance "my_client" to the Server at 
   IP address 1.2.3.4 and port 23.  */
status =  nx_telnet_client_connect(&my_client, IP_ADDRESS(1,2,3,4), 23, 100);

/* If status is NX_SUCCESS the Telnet Client instance was successfully
   connected to the Telnet Server.  */
```

## nxd_telnet_client_connect

Connect a Telnet Client with IPv6 or IPv4 address

### Prototype

```c
UINT nxd_telnet_client_connect(
    NX_TELNET_CLIENT *client_ptr, 
    NXD_ADDRESS *server_ip_address, 
    UINT server_port, 
    ULONG wait_option);
```

### Description

This service attempts to connect the previously created Telnet Client instance to the Server at the specified IP and port using the Telnet Server's IPv6 address. This service can take an IPv4 or an IPv6 address but must be contained in the NXD_ADDRESS variable *server_ip_address.*

### Input Parameters

- *client_ptr*: Pointer to Telnet Client control block.
- *server_ip_address*: IP Address of Server.
- *server_port*: TCP Port of Server (Telnet Server is port 23).
- *wait_option*: Defines how long the service will wait for the Telnet Client connect. The wait options are defined as follows:
    - **timeout value**: (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the Telnet Server response.
    - **TX_WAIT_FOREVER**: (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the Telnet Server responds to the request.

### Return Values

- **NX_SUCCESS**: (0x00) Successful Client connect.
- **NX_TELNET_ERROR**: (0xF0) Client connect error.
- **NX_TELNET_NOT_DISCONNECTED**: (0xF4) Client already connected.
- NX_PTR_ERROR: (0x07) Invalid Client pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of service.
- NX_TELNET_INVALID_PARAMETER: (0xF5) Invalid non pointer input

### Allowed From

Threads

### Example

```c
/* Connect the Telnet Client instance "my_client" to the Server at 
   IPv6 address 20010db1:0:f101::101 and port 23.  */
status =  nxd_telnet_client_connect(&my_client, &server_ip_address, 23, 100);

/* If status is NX_SUCCESS the Telnet Client instance was successfully
   connected to the Telnet Server.  */
```

## nx_telnet_client_create

Create a Telnet Client

### Prototype

```c
UINT nx_telnet_client_create(
    NX_TELNET_CLIENT *client_ptr, 
    CHAR *client_name, NX_IP *ip_ptr, 
    ULONG window_size);
```

### Description

This service creates a Telnet Client instance.

### Input Parameters

- *client_ptr*: Pointer to Telnet Client control block.
- *client_name*: Name of Client instance.
- *ip_ptr*: Pointer to IP instance.
- *window_size*: Size of TCP receive window for this Client.

### Return Values

- **NX_SUCCESS**: (0x00) Successful Client create.
- **NX_TELNET_ERROR**: (0xF0) Socket create error.
- NX_PTR_ERROR: (0x07) Invalid Client or IP pointer.

### Allowed From

Initialization, Threads

### Example

```c
/* Create the Telnet Client instance "my_client" on the IP instance "ip_0".  */
status =  nx_telnet_client_create(&my_client, "My Telnet Client", &ip_0, 2048);

/* If status is NX_SUCCESS the Telnet Client instance was successfully
   created.  */
```
## nx_telnet_client_delete

Delete a Telnet Client

### Prototype

```c
UINT nx_telnet_client_delete(NX_TELNET_CLIENT *client_ptr);
```

### Description

This service deletes a previously created Telnet Client instance.

### Input Parameters

- *client_ptr*: Pointer to Telnet Client control block.

### Return Values

- **NX_SUCCESS**: (0x00) Successful Client delete.
- **NX_TELNET_NOT_DISCONNECTED**: (0xF4) Client still connected.
- NX_PTR_ERROR: (0x07) Invalid Client pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c
/* Delete the Telnet Client instance "my_client".  */
status =  nx_telnet_client_delete(&my_client);

/* If status is NX_SUCCESS the Telnet Client instance was successfully
   deleted.  */
```

## nx_telnet_client_disconnect

Disconnect a Telnet Client

### Prototype

```c
UINT nx_telnet_client_disconnect(
    NX_TELNET_CLIENT *client_ptr,
    ULONG wait_option);
```

### Description

This service disconnects a previously connected Telnet Client instance.

### Input Parameters

- *client_ptr*: Pointer to Telnet Client control block.
- *wait_option*: Defines how long the service will wait for the Telnet Client disconnect. The wait options are defined as follows:
    - **timeout value**: (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the Telnet Server response.
    - **TX_WAIT_FOREVER**: (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the Telnet Server responds to the request.

### Return Values

- **NX_SUCCESS**: (0x00) Successful Client disconnect.
- **NX_TELNET_NOT_CONNECTED**: (0xF3) Client not connected.
- NX_PTR_ERROR: (0x07) Invalid Client pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.


### Allowed From

Threads

### Example

```c

/* Disconnect the Telnet Client instance "my_client".  */
status =  nx_telnet_client_disconnect(&my_client, 100);

/* If status is NX_SUCCESS the Telnet Client instance was successfully
   disconnected.  */
```

## nx_telnet_client_packet_receive

Receive packet via Telnet Client

### Prototype

```c
UINT nx_telnet_client_packet_receive(
    NX_TELNET_CLIENT *client_ptr, 
    NX_PACKET **packet_ptr, 
    ULONG wait_option);
```

### Description

This service receives a packet from the previously connected Telnet Client instance.

### Input Parameters

- *client_ptr*: Pointer to Telnet Client control block.
- *packet_ptr*: Pointer to the destination for the received packet.
- *wait_option*: Defines how long the service will wait for the Telnet Client packet receive. The wait options are defined as follows:
    - **timeout value**: (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the Telnet Server response.
    - **TX_WAIT_FOREVER**: (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the Telnet Server responds to the request.

### Return Values

- **NX_SUCCESS**: (0x00) Successful Client packet receive.
- NX_PTR_ERROR: (0x07) Invalid pointer input
- NX_CALLER_ERROR: (0x11) Invalid caller of service.

### Allowed From

Threads

### Example

```c

/* Receive a packet from the Telnet Client instance "my_client".  */
status =  nx_telnet_client_packet_receive(&my_client, &my_packet, 100);

/* If status is NX_SUCCESS the "my_packet" pointer contains data received from
   the Telnet Client connection.  */
```
## nx_telnet_client_packet_send

Send packet via Telnet Client

### Prototype

```c
UINT nx_telnet_client_packet_send(
    NX_TELNET_CLIENT *client_ptr, 
    NX_PACKET *packet_ptr, 
    ULONG wait_option);
```

### Description

This service sends a packet through the previously connected Telnet Client instance.

### Input Parameters

- *client_ptr*: Pointer to Telnet Client control block.
- *packet_ptr*: Pointer to the packet to send.
- *wait_option*: Defines how long the service will wait for the Telnet Client packet send. The wait options are defined as follows:
    - **timeout value**: (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the Telnet Server response.
    - **TX_WAIT_FOREVER**: (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the Telnet Server responds to the request.

### Return Values

- **NX_SUCCESS**: (0x00) Successful Client packet send.
- **NX_TELNET_ERROR**: (0xF0) Send packet failed – caller is responsible for releasing the packet.
- NX_PTR_ERROR: (0x07) Invalid pointer input
- NX_CALLER_ERROR: (0x11) Invalid caller of service.

### Allowed From

Threads

### Example

```c
/* Send a packet via the Telnet Client instance "my_client".  */
status =  nx_telnet_client_packet_send(&my_client, my_packet, 100);
/* If status is NX_SUCCESS the packet was successfully sent.  */

```

## nx_telnet_server_create

Create a Telnet Server

### Prototype

```c
UINT nx_telnet_server_create(
    NX_TELNET_SERVER *server_ptr,
    CHAR *server_name, 
    NX_IP *ip_ptr, 
    VOID *stack_ptr,
    ULONG stack_size, 
    void (*new_connection)(
        struct NX_TELNET_SERVER_STRUCT*telnet_server_ptr, 
        UINT logical_connection), 
    void (*receive_data)(
        struct NX_TELNET_SERVER_STRUCT *telnet_server_ptr, 
        UINT logical_connection,
        NX_PACKET *packet_ptr), 
    void (*connection_end)(
        struct NX_TELNET_SERVER_STRUCT *telnet_server_ptr, 
        UINT logical_connection));

```

### Description

This service creates a Telnet Server instance on the specified IP instance.

### Input Parameters

- *server_ptr*: Pointer to Telnet Server control block.
- *server_name*: Name of Telnet Server instance.
- *ip_ptr*: Pointer to associated IP instance.
- *stack_ptr*: Pointer to stack for the internal Server thread.
- *sack_size*: Size of the stack, in bytes.
- *new_connection*: Application callback routine function pointer. This routine is called whenever a new Telnet Client connection request is detected by the Server.
- *receive_data*: Application callback routine function pointer. This routine is called whenever a new Telnet Client data is present on the connection. This routine is responsible for releasing the packet.
- *end_connection*: Application callback routine function pointer. This routine is called whenever a Telnet Client connection is disconnected by the Client or the Client connection times out ("activity timeout" expires). The Server can also disconnect via the *nx_telnet_server_disconnect* service described below.

### Return Values

- **NX_SUCCESS**: (0x00) Successful Server create.
- NX_PTR_ERROR: (0x07) Invalid Server, IP, stack, or application callback pointers.

### Allowed From

Initialization, Threads

### Example

```c
/* Create a Telnet Server instance "my_server".  */
status =  nx_telnet_server_create(&my_server, "Telnet Server", &ip_0, 
                                   pointer, 2048, telnet_new_connection, 
                                   telnet_receive_data, telnet_connection_end);


/* If status is NX_SUCCESS the Telnet Server was successfully created.  */
```
## nx_telnet_server_delete

Delete a Telnet Server

### Prototype

```c
UINT nx_telnet_server_delete(NX_TELNET_SERVER *server_ptr);
```

### Description

This service deletes a previously created Telnet Server instance.

### Input Parameters

- *server_ptr*: Pointer to Telnet Server control block.

### Return Values

- **NX_SUCCESS**: (0x00) Successful Server delete.
- NX_PTR_ERROR: (0x07) Invalid Server pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c
/* Delete the Telnet Server instance "my_server".  */
status =  nx_telnet_server_delete(&my_server);

/* If status is NX_SUCCESS the Telnet Server was successfully deleted.  */
```

## nx_telnet_server_disconnect

Disconnect a Telnet Client

### Prototype

```c
UINT nx_telnet_server_disconnect(
    NX_TELNET_SERVER *server_ptr,
    UINT logical_connection);
```

### Description

This service disconnects a previously connected Client on this Telnet Server instance. This routine is typically called from the application's receive data callback function in response to a condition detected in the data received.

### Input Parameters

- *server_ptr*: Pointer to Telnet Server control block.
- *logical_connection*: Logical connection corresponding the Client connection on this Server. Valid value range from 0 through NX_TELNET_MAX_CLIENTS.

### Return Values

- **NX_SUCCESS**: (0x00) Successful Server disconnect.
- **NX_TELNET_ERROR**: (0xF0) Server disconnect failed.
- NX_OPTION_ERROR: (0x0A) Invalid logical connection.
- NX_PTR_ERROR: (0x07) Invalid Server pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c

/* Disconnect the Telnet Client associated with logical connection 2 on 
   the Telnet Server instance "my_server".  */
status =  nx_telnet_server_disconnect(&my_server, 2);

/* If status is NX_SUCCESS the Client on logical connection 2 was 
   disconnected.  */
```

## nx_telnet_server_get_open_connection_count

Return number of currently open connections

### Prototype

```c
UINT nx_telnet_server_get_open_connection_count(
    NX_TELNET_SERVER *server_ptr,
    UINT *connection_count);
```

### Description

This service returns the number of currently connected Telnet Clients.

### Input Parameters

- *server_ptr*: Pointer to Telnet Server control block.
- *Connection_count*: Pointer to memory to store connection count

### Return Values

- **NX_SUCCESS**: (0x00) Successful completion.
- NX_PTR_ERROR: (0x07) Invalid Server pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c
/* Get the number of Telnet Clients connected to the Server. */

status =  nx_telnet_server_get_open_connection_count(&my_server, &conn_count);

/* If status is NX_SUCCESS the conn_count holds the number of open connections.  */

```

## nx_telnet_server_packet_send

Send packet through Client connection

### Prototype

```c
UINT nx_telnet_server_packet_send(
    NX_TELNET_SERVER *server_ptr, 
    UINT logical_connection, 
    NX_PACKET *packet_ptr, 
    ULONG wait_option);
```

### Description

This service sends a packet to the Client connection on this Telnet Server instance. This routine is typically called from the application's receive data callback function in response to a condition detected in the data received.

### Input Parameters

- *server_ptr*: Pointer to Telnet Server control block.
- *logical_connection*: Logical connection corresponding the Client connection on this Server. Valid value range from 0 through NX_TELNET_MAX_CLIENTS.
- *packet_ptr*: Pointer to the received packet.
- *wait_option*: Defines how long the service will wait for the Telnet Server packet send. The wait options are defined as follows:
    - **timeout value*: (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the Telnet Server response.
    - **TX_WAIT_FOREVER*: (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the Telnet Server responds to the request. 

### Return Values

- **NX_SUCCESS**: (0x00) Successful packet send.
- **NX_TELNET_FAILED**: (0xF2) TCP socket send failed.
- NX_OPTION_ERROR: (0x0A) Invalid logical connection.
- NX_PTR_ERROR: (0x07) Invalid Server pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of service.

### Allowed From

Threads

### Example

```c
/* Send a packet to the Telnet Client associated with logical connection 2 on 
   the Telnet Server instance "my_server".  */
status =  nx_telnet_server_packet_send(&my_server, 2, my_packet, 100);

/* If status is NX_SUCCESS the packet was sent to the Client on logical 
   connection 2.  */
```

## nx_telnet_server_packet_pool_set

Set previously created packet pool as Telnet Server pool

### Prototype

```c
UINT nx_telnet_server_packet_pool_set(
    NX_TELNET_SERVER *server_ptr,
    NX_PACKET_POOL *packet_pool_ptr);
```

### Description

This service sets a previously created packet pool as the Telnet Server packet pool if NX_TELNET_SERVER_USER_CREATE_PACKET_POOL is defined. It also requires that NX_TELNET_SERVER_OPTION_DISABLE not be defined such that the Telnet Server needs a packet pool to transmit Telnet options to Telnet clients.

This permits applications to create the packet pool in different memory e.g. no cache memory, than the Telnet Server stack. Note that if this function does not check if the Telnet Server packet pool is already set. If it is called on a non null Telnet Server packet pool pointer, it will overwrite it and replace the existing packet pool with packet pool pointed to by the input pointer.

### Input Parameters

- *server_ptr*: Pointer to Telnet Server control block
- *packet_pool_ptr*: Pointer to previously created packet pool

### Return Values

- **NX_SUCCESS**: (0x00) Successfully set pool.
- NX_PTR_ERROR: (0x07) Invalid Server pointer.

### Allowed From

Init, Threads

### Example

```c
status =  nx_packet_pool_create(&telnet_server_packet_pool, 
                                "Telnet Server Packet Pool", 
                                telnet_server_pool_area, 600*10);

/* Set the packet pool as the Telnet Server packet pool.   */
status =  nx_telnet_server_packet_pool_set(&my_server, &telnet_server_packet_pool);

/* If status is NX_SUCCESS the packet pool is set as Telnet Server pool.  */
```
## nx_telnet_server_start

Start a Telnet Server

### Prototype

```c
UINT nx_telnet_server_start(NX_TELNET_SERVER *server_ptr);

```

### Description

This service starts a previously created Telnet Server instance.

### Input Parameters

- *server_ptr*: Pointer to Telnet Server control block.

### Return Values

- **NX_SUCCESS**: (0x00) Successfully started.
- **NX_TELNET_NO_PACKET_POOL**: (0xF6) No packet pool set
- NX_PTR_ERROR: (0x07) Invalid Server pointer.

### Allowed From

Initialization, Threads

### Example

```c
/* Start the Telnet Server instance "my_server".  */
status =  nx_telnet_server_start(&my_server);

/* If status is NX_SUCCESS the Server was started.  */
```

## nx_telnet_server_stop

Stop a Telnet Server

### Prototype

```c
UINT nx_telnet_server_stop(NX_TELNET_SERVER *server_ptr);
```

### Description

This service stops a previously created and started Telnet Server instance.

### Input Parameters

- *server_ptr*: Pointer to Telnet Server control block.

### Return Values

- **NX_SUCCESS**: (0x00) Successfully stopped
- NX_PTR_ERROR: (0x07) Invalid Server pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of service

### Allowed From

Threads

### Example

```c
/* Stop the Telnet Server instance "my_server".  */
status =  nx_telnet_server_stop(&my_server);

/* If status is NX_SUCCESS the Server was stopped.  */
```

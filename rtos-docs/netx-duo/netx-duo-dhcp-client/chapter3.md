---
title: Chapter 3 - Description of NetX Duo DHCP Client services
description: This chapter contains a description of all NetX Duo DHCP Client services (listed below) in alphabetic order.
---

# Chapter 3 - Description of NetX Duo DHCP Client services

This chapter contains a description of all NetX Duo DHCP Client services (listed below) in alphabetic order.

In the **Return Values** section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## nx_dhcp_create

Create a DHCP instance

### Prototype

```c
UINT nx_dhcp_create(
    NX_DHCP *dhcp_ptr, 
    NX_IP *ip_ptr,
    CHAR *name_ptr);
```

### Description

This service creates a DHCP instance for the previously created IP instance. By default the primary interface is enabled for running DHCP. The name input, while not used in the NetX Duo implementation of DHCP Client, must follow RFC 1035 criteria for host names. The total length must not exceed 255 characters, the labels separate by dots must begin with a letter, and end with a letter or number, and may contain hyphens but no other non-alphanumeric character.

If the application would like to run DHCP another interface registered with the IP instance, (using *nx_ip_interface_attach*), the application can call ***nx_dhcp_set_interface_index*** to run DHCP on just that interface, or *nx_dhcp_interface_enable* to run DHCP on that interface as well. See description of these services for more details.

> **Note:** The application must make sure the DHCP Client packet pool payload can support the minimum DHCP message size specified by the RFC 2131 Section 2 (548 bytes of DHCP message data plus UDP, IP and physical network frame headers).

### Input Parameters

- *dhcp_ptr*: Pointer to DHCP control block.
- *ip_ptr*: Pointer to previously created IP instance.  
- *name_ptr*: Pointer to host name for DHCP instance.

### Return Values

- **NX_SUCCESS**: (0x00) Successful DHCP create
- **NX_DHCP_INVALID_NAME**: (0xA8) Invalid host name
- **NX_DHCP_INVALID_PAYLOAD**: (0x9C) Payload too small for DHCP message
- NX_PTR_ERROR: (0x16) Invalid IP or DHCP pointer

### Allowed From

Threads, Initialization

### Example

```c
/* Create a DHCP instance.  */
status =  nx_dhcp_create(&my_dhcp, &my_ip, "My-DHCP");

/* If status is NX_SUCCESS a DHCP instance was successfully created.  */
```

## nx_dhcp_interface_enable

Enable the specified interface to run DHCP 

### Prototype

```c
UINT nx_dhcp_interface_enable(
    NX_DHCP *dhcp_ptr,
    UINT interface_index);
```

### Description

This service enables the specified interface for running DHCP. By default the primary interface is enabled for DHCP Client. At this point, DHCP can be started on this interface either by calling ***nx_dhcp_interface_start*** or to start DHCP on all enabled interfaces ***nx_dhcp_start***.

> **Note:** The application must first register this interface with the IP instance, using *nx_ip_interface_attach.*

Further, there must be an available DHCP Client interface 'record' to add this interface to the list of enabled interfaces. By default NX_DHCP_CLIENT_MAX_RECORDS is defined to 1. Set this option to the maximum number of interfaces expected to run DHCP Client simultaneously. Typically NX_DHCP_CLIENT_MAX_RECORDS will equal NX_MAX_PHYSICAL_INTERFACES; however, if a device has more physical interfaces than it expects to run DHCP Client, it can save memory by setting NX_DHCP_CLIENT_MAX_RECORDS to less than that number. There is not a one to one mapping of physical interfaces with DHCP Client interface records.

The difference between this service and ***nx_dhcp_set_interface_index*** is the latter sets only a single interface to run DHCP whereas this service simply adds the specified interface to the list of Client interfaces enabled for DHCP.

To disable an interface for DHCP, the application can call the ***nx_dhcp_interface_disable*** service.

### Input Parameters

- *dhcp_ptr*: Pointer to DHCP control block.  
- *interface_index*: Index of interface to enable DHCP on

### Return Values

- **NX_SUCCESS**: (0x00) Successful DHCP enable
- **NX_DHCP_NO_RECORDS_AVAILABLE**: (0xA7) No record available for another Interface to be enabled for DHCP
- **NX_DHCP_INTERFACE_ALREADY_ENABLED**: (0xA3) Interface enabled for DHCP
- NX_PTR_ERROR: (0x16) Invalid IP or DHCP pointer
- NX_INVALID_INTERFACE: (0x4C) Invalid network interface

### Allowed From

Threads, Initialization

### Example

```c
/* Enable DHCP on a secondary interface. It is already enabled on the primary 
   interface.  NX_DHCP_CLIENT_MAX_RECORDS is set to 2. */

status =  nx_dhcp_interface_enable(&my_dhcp, 1);
/* If status is NX_SUCCESS the interface was successfully enabled.  */


status = nx_dhcp_start(&my_dhcp);
/* If status is NX_SUCCESS DHCP is running on interface 0 and 1.  */
```

## nx_dhcp_interface_disable

Disable the specified interface to run DHCP 

### Prototype

```c

UINT nx_dhcp_interface_disable(
    NX_DHCP *dhcp_ptr, 
    UINT interface_index);
```

### Description

This service disables the specified interface for running DHCP. It reinitializes the DHCP Client on this interface.

To restart the DHCP Client the application must re-enable the interface using *nx_dhcp_interface_enable* and restart DHCP by calling *nx_dhcp_interface_start*.

### Input Parameters

- *dhcp_ptr*: Pointer to DHCP control block.  
- *interface_index*: Index of interface to disable DHCP on

### Return Values

- **NX_SUCCESS**: (0x00) Successful DHCP create
- **NX_DHCP_INTERFACE_NOT_ENABLED**: (0xA4) Interface not enabled for DHCP
- NX_PTR_ERROR: (0x16) Invalid IP or DHCP pointer
- NX_CALLER_ERROR: (0x11) Invalid caller of this service
- NX_INVALID_INTERFACE: (0x4C) Invalid network interface

### Allowed From

Threads

### Example

```c
/* Disable DHCP on a secondary interface. */

status =  nx_dhcp_interface_disable(&my_dhcp, 1);
/* If status is NX_SUCCESS the interface is successfully disabled.  */
```
## nx_dhcp_clear_broadcast_flag

Set the DHCP broadcast flag

### Prototype

```c
UINT nx_dhcp_clear_broadcast_flag(NX_DHCP *dhcp_ptr, UINT clear_flag);
```

### Description

This service sets or clears the broadcast flag the DHCP message header for all interfaces enabled for DHCP. For some DHCP messages (e.g. DISCOVER) the broadcast flag is set to broadcast because the Client does not have an IP address.

clear_flag values:

- **NX_TRUE**: broadcast flag is cleared (request unicast response)
- **NX_FALSE**: broadcast flag is set (request broadcast response)

This service is intended for DHCP Clients that must go through a router to get to the DHCP Server, where the router rejects forwarding broadcast messages.

### Input Parameters

- *dhcp_ptr*: Pointer to DHCP control block  
- *clear_flag*: Value to set the broadcast flag to

### Return Values

- **NX_SUCCESS**: (0x00) Successfully set flag
- NX_PTR_ERROR: (0x16) Invalid IP or DHCP pointer

### Allowed From

Threads, Initialization

### Example

```c
/* Send DHCP Client messages with the broadcast flag cleared (e.g. request a  
    unicast response).  */
status =  nx_dhcp_clear_broadcast_flag(&my_dhcp, NX_TRUE);

/* If status is NX_SUCCESS the DHCP Client broadcast flag is updated.  */
```

## nx_dhcp_interface_clear_broadcast_flag

Set or clear the broadcast flag on the specified interface

### Prototype

```c
UINT nx_dhcp_interface_clear_broadcast_flag(
    NX_DHCP *dhcp_ptr, 
    UINT interface_index, 
    UINT clear_flag);
```

### Description

This service enables the DHCP Client host application to set or clear the broadcast flag in DHCP Client messages to the DHCP Server on the specified interface. For more details see ***nx_dhcp_clear_broadcast_flag***.

### Input Parameters

- *dhcp_ptr*: Pointer to DHCP control block
- *interface_index*: Index of interface to set the broadcast flag
- *clear_flag*: Value to set the broadcast flag to

### Return Values

- **NX_SUCCESS**: (0x00) Successfully set flag
- **NX_DHCP_INTERFACE_NOT_ENABLED**: (0xA4) Interface not enabled for DHCP
- NX_PTR_ERROR: (0x16) Invalid IP or DHCP pointer
- NX_INVALID_INTERFACE: (0x4C) Invalid network interface

### Allowed From

Threads, Initialization

### Example

```c
/* Send DHCP Client messages with the broadcast flag cleared (e.g. request a 
   unicast response) on a previously attached secondary interface.  */

iface_index = 1;

status =  nx_dhcp_interface_clear_broadcast_flag(&my_dhcp, iface_index, NX_TRUE);

/* If status is NX_SUCCESS the DHCP Client broadcast flag is updated.  */
```

## nx_dhcp_delete

Delete a DHCP instance

### Prototype

```c
UINT nx_dhcp_delete(NX_DHCP *dhcp_ptr);
```

### Description

This service deletes a previously created DHCP instance.

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance.

### Return Values

- **NX_SUCCESS**: (0x00) Successful DHCP delete.
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c
/* Delete a DHCP instance.  */
status =  nx_dhcp_delete(&my_dhcp);

/* If status is NX_SUCCESS the DHCP instance was successfully deleted.  */
```

## nx_dhcp_force_renew

Send a force renew message 

### Prototype

```c
UINT nx_dhcp_force_renew(NX_DHCP *dhcp_ptr);
```

### Description

This service enables the host application to send a force renew message on all interfaces enabled for DHCP. The DHCP Client must be in a BOUND state. This function sets the state to RENEW such that the DHCP Client will try to renew before the T1 timeout expires.

To send a force renew on a specific interface when multiple interfaces are DHCP-enabled, use ***nx_dhcp_interface_force_renew***.

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance.

### Return Values

- **NX_SUCCESS**: (0x00) Successfully sent force renew.
- **NX_DHCP_NOT_BOUND**: (0x94) Client IP address not bound.  
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c
/* Send a force renew message from the Client.  */
status =  nx_dhcp_force_renew(&my_dhcp);

/* If status is NX_SUCCESS the DHCP client state is the RENEWING state and the    
   DHCP Client thread task will begin renewing before T1 is expired.  */
```

## nx_dhcp_interface_force_renew

Send a force renew message on the specified interface

### Prototype

```c
UINT nx_dhcp_interface_force_renew(
    NX_DHCP *dhcp_ptr, 
    UINT interface_index);
```

### Description

This service enables the host application to send a force renew message on the input interface as long as that interface has been enabled for DHCP (see ***nx_dhcp_interface_enable***). The DHCP Client on the specified interface must be in a BOUND state. This function sets the state to RENEW such that the DHCP Client will try to renew before the T1 timeout expires.

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance.

### Return Values

- **NX_SUCCESS**: (0x00) Successfully sent force renew.  
- **NX_DHCP_INTERFACE_NOT_ENABLED**: (0xA4) Interface not enabled for DHCP
- NX_PTR_ERROR: (0x16) Invalid IP or DHCP pointer
- NX_INVALID_INTERFACE: (0x4C) Invalid network interface

### Allowed From

Threads

### Example

```c
/* Send a force renew message to the server on interface 1.  */
status =  nx_dhcp_interface_force_renew(&my_dhcp, 1);


/* If status is NX_SUCCESS the DHCP client state is the RENEWING state and the    
   DHCP Client thread task will begin renewing before T1 is expired.  */
```

## nx_dhcp_packet_pool_set

Set the DHCP Client packet pool

### Prototype

```c
UINT nx_dhcp_packet_pool_set(
    NX_DHCP *dhcp_ptr,
    NX_PACKET_POOL *packet_pool_ptr);
```

### Description

This service allows the application to create the DHCP Client packet pool by passing in a pointer to a previously created packet pool in this service call. To use this feature, the host application must define **NX_DHCP_CLIENT_USER_CREATE_PACKET_POOL**. When defined, the *nx_dhcp_create* service will not create the Client's packet pool. 

> **Note:** The application is recommended to use the default values for the DHCP client packet pool payload, defined as NX_DHCP_PACKET_PAYLOAD in *nxd_dhcp_client.h* when creating the packet pool.

### Input Parameters

- *dhcp_ptr*: Pointer to DHCP control block.  
- *packet_pool_ptr*: Pointer to previously created packet pool

### Return Values

- **NX_SUCCESS**: (0x00) DHCP Client packet pool is set
- **NX_NOT_ENABLED**: (0x14) Service is not enabled
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer
- NX_DHCP_INVALID_PAYLOAD: (0x9C) Payload is too small

### Allowed From

Application code

### Example

```c
/* Create the packet pool. */
status =  nx_packet_pool_create(&dhcp_pool, "DHCP Client Packet Pool", 
                                 NX_DHCP_PACKET_PAYLOAD, pointer, 
                                 (15 * NX_DHCP_PACKET_PAYLOAD));

/* Create the DHCP Client. */
status =  nx_dhcp_create(&dhcp_0, &ip_0, "janetsdhcp1");

/* Set the DHCP Client packet pool.  */
status =  nx_dhcp_packet_pool_set(&my_dhcp, packet_pool_ptr);
/* If status is NX_SUCCESS packet pool was successfully set.  */

```

## nx_dhcp_request_client_ip

Set requested IP address for DHCP instance

### Prototype

```c
UINT nx_dhcp_request_client_ip(
    NX_DHCP *dhcp_ptr, 
    ULONG client_ip_address, 
    UINT skip_discover_message);
```

### Description

This service sets the IP address for the DHCP Client to request from the DHCP Server on the first interface enabled for DHCP in the DHCP Client record. If the *skip_discover_message* flag is set, the DHCP Client skips the Discover message and sends a Request message.

To set the request for a specific IP for DHCP messages on a specific interface, use the *nx_dhcp_interface_request_client_ip* service.

### Input Parameters

- *dhcp_ptr*: Pointer to DHCP control block.  
- *client_ip_address*: IP address to request from DHCP server
- *skip_discover_message**:
    - If true, DHCP Client sends Request message
    - If false, it sends the Discover message.

### Return Values

- **NX_SUCCESS**: (0x00) Requested IP address is set.
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer
- NX_DHCP_INVALID_IP_REQUEST: (0x9D) NULL IP address requested

### Allowed From

Threads

### Example

```c
/* Set the DHCP Client requested IP address and skip the discover message.  */

status =  nx_dhcp_request_client_ip(&my_dhcp, IP(192,168,0,6), NX_TRUE);

/* If status is NX_SUCCESS requested IP address was successfully set.  */

```

## nx_dhcp_interface_request_client_ip

Set requested IP address for DHCP instance on specified interface

### Prototype

```c
UINT nx_dhcp_interface_request_client_ip(
    NX_DHCP *dhcp_ptr,
    UINT  interface_index, 
    ULONG client_ip_address,
    UINT skip_discover_message);
```

### Description

This service sets the IP address for the DHCP Client to request from the DHCP Server on the specified interface, if that interface is enabled for DHCP (see *nx_dhcp_interface_enable*). If the ***skip_discover_message*** flag is set, the DHCP Client skips the Discover message and sends a Request message.

### Input Parameters

- *dhcp_ptr*: Pointer to DHCP control block.
- *Interface_index*: Index of interface to request IP address on
- *client_ip_address*: IP address to request from DHCP server
- *skip_discover_message*: If true, DHCP Client sends Request message; else it sends the Discover message.

### Return Values

- **NX_SUCCESS**: (0x00) Requested IP address is set.
- **NX_DHCP_INTERFACE_NOT_ENABLED**: (0xA4) Interface not enabled for DHCP
- NX_PTR_ERROR: (0x16) Invalid IP or DHCP pointer
- NX_INVALID_INTERFACE: (0x4C) Invalid network interface

### Allowed From

Threads

### Example

```c
/* Set the DHCP Client requested IP address and skip the discover message on  
   interface 0.  */
status =  nx_dhcp_interface_request_client_ip(&my_dhcp, 0, IP(192,168,0,6), NX_TRUE);

/* If status is NX_SUCCESS requested IP address was successfully set.  */
```

## nx_dhcp_reinitialize

Clear the DHCP client network parameters

### Prototype

```c
UINT nx_dhcp_reinitialize(NX_DHCP *dhcp_ptr);
```

### Description

This service clears the host application network parameters (IP address, network address and network mask), and clears the DHCP Client state on all interfaces enabled for DHCP. It is used in combination with *nx_dhcp_stop* and ***nx_dhcp_start*** to 'restart' the DHCP state machine:

```c
nx_dhcp_stop(&my_dhcp);
nx_dhcp_reinitialize(&my_dhcp);
nx_dhcp_start(&my_dhcp);
```

To reinitialize the DHCP Client on a specific interface when multiple interfaces are enabled for DHCP, use the ***nx_dhcp_interface_reinitialize*** service.

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance.

### Return Values

- **NX_SUCCESS**: (0x00) DHCP successfully reinitialized 
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer

### Allowed From

Threads

### Example

```c
/* Reinitialize the previously started DHCP client.  */
status =  nx_dhcp_reinitialize(&my_dhcp);

/* If status is NX_SUCCESS the host application successfully reinitialized its network parameters and DHCP client state. */
```

## nx_dhcp_interface_reinitialize

Clear the DHCP client network parameters on the specified interface 

### Prototype

```c
UINT nx_dhcp_interface_reinitialize(
    NX_DHCP *dhcp_ptr, 
    UINT interface_index);
```

### Description

This service clears the network parameters (IP address, network address and network mask) on the specified interface if that interface is enabled for DHCP (see ***nx_dhcp_interface_enable***). See ***nx_dhcp_reinitialize*** for more details.

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance
- *interface_index*: Index of interface to reinitialize

### Return Values

- **NX_SUCCESS**: (0x00) Interface successfully reinitialized
- **NX_DHCP_INTERFACE_NOT_ENABLED**: (0xA4) Interface not enabled for DHCP
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.
- NX_INVALID_INTERFACE: (0x4C) Invalid network interface

### Allowed From

Threads

### Example

```c
/* Reinitialize the previously started DHCP client on interface 1.  */
status =  nx_dhcp_interface_reinitialize(&my_dhcp, 1);

/* If status is NX_SUCCESS the host application successfully reinitialized its network parameters and DHCP client state. */
```

## nx_dhcp_release

Release Leased IP address

### Prototype

```c
UINT nx_dhcp_release(NX_DHCP *dhcp_ptr);
```

### Description

This service releases the IP address obtained from a DHCP server by sending the RELEASE message to that server. It then reinitializes the DHCP Client. This service is applied to all interfaces enabled for DHCP.

The application can restart the DHCP Client by calling ***nx_dhcp_start***.

To release an address back to the DHCP server on a specific interface, use the *nx_dhcp_interface_release* service

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance.

### Return Values

- **NX_SUCCESS**: (0x00) Successful DHCP release.  
- **NX_DHCP_NOT_BOUND**: (0x94) The IP address has not been leased so it can't be released.
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c
/* Release the previously leased IP address.  */
status =  nx_dhcp_release(&my_dhcp);

/* If status is NX_SUCCESS the previous IP lease was successfully released.  */
```

## nx_dhcp_interface_release

Release IP address on the specified interface

### Prototype

```c
UINT nx_dhcp_interface_release(NX_DHCP *dhcp_ptr, UINT interface_index);
```

### Description

This service releases the IP address obtained from a DHCP server on the specified interface and reinitializes the DHCP Client. The DHCP Client can be restarted by calling ***nx_dhcp_start***.

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance.

### Return Values

- **NX_SUCCESS**: (0x00) Successful DHCP release.
- **NX_DHCP_INTERFACE_NOT_ENABLED**: (0xA4) Interface not enabled for DHCP
- **NX_DHCP_NOT_BOUND**: (0x94) The IP address has not been leased so it can't be released.
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.
- NX_INVALID_INTERFACE: (0x4C) Invalid network interface

### Allowed From

Threads

### Example

```c
/* Release the previously leased IP address on interface 1.  */
status =  nx_dhcp_interface_release(&my_dhcp, 1);

/* If status is NX_SUCCESS the previous IP lease was successfully released.  */
```

## nx_dhcp_decline

Decline IP address from DHCP Server

### Prototype

```c
UINT nx_dhcp_decline(NX_DHCP *dhcp_ptr);
```

### Description

This service declines an IP address leased from the DHCP server on all interfaces enabled for DHCP. If **NX_DHCP_CLIENT_SEND_ ARP_PROBE** is defined, the DHCP Client will send a DECLINE message if it detects that the IP address is already in use. See **ARP Probes** in Chapter One for more information on ARP probe configuration in the NetX Duo DHCP Client.

The application can use this service to decline its IP address if it discovers the address is in use by other means.

This service reinitializes the DHCP Client to that it can be restarted by calling ***nx_dhcp_start***.

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance.

### Return Values

- **NX_SUCCESS**: (0x00) Decline successfully sent  
- **NX_DHCP_NOT_BOUND**: (0x94) DHCP Client not bound
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer
- NX_CALLER_ERROR: (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```c

/* Decline the IP address offered by the DHCP server.  */
status =  nx_dhcp_decline(&my_dhcp);

/* If status is NX_SUCCESS the previous IP address decline message was successfully transmitted.  */
```

## nx_dhcp_interface_decline

Decline IP address from DHCP Server on the specified interface

### Prototype

```c
UINT nx_dhcp_interface_decline(
    NX_DHCP *dhcp_ptr, 
    UINT interface_index);
```

### Description

This service sends the DECLINE message to the server to decline an IP address assigned by the DHCP server. It also reinitializes the DHCP Client. See ***nx_dhcp_decline*** for more details.

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance.
- *Interface_index*: Index of interface to decline IP address

### Return Values

- **NX_SUCCESS**: (0x00) DHCP decline message sent  
- **NX_DHCP_NOT_BOUND**: (0x94) DHCP Client not bound
- **NX_DHCP_INTERFACE_NOT_ENABLED**: (0xA4) Interface not enabled for DHCP
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.
- NX_INVALID_INTERFACE: (0x4C) Invalid network interface

### Allowed From

Threads

### Example

```c
/* Decline the IP address offered by the DHCP server on interface 2.  */
status =  nx_dhcp_interface_decline(&my_dhcp, 2);

/* If status is NX_SUCCESS the previous IP address decline message was successfully transmitted.  */
```
## nx_dhcp_send_request

Send DHCP message to Server

### Prototype

```c
UINT nx_dhcp_send_request(
    NX_DHCP *dhcp_ptr, 
    UINT dhcp_message_type);

```

### Description

This service sends the specified DHCP message to the DHCP server on the first interface enabled for DHCP found in the DHCP Client record. To send a RELEASE or DECLINE message, the application must use the ***nx_dhcp[_interface]_release*** or ***nx_dhcp_interface_decline*** services respectively.

The DHCP Client must be started to use this service except for sending the INFORM_REQUEST message type.

> **Note:** This service is not intended for the host application to 'drive' the DHCP Client state machine.

### Input Parameters

- *dhcp_ptr*: Pointer to DHCP control block.  
- *dhcp_message_type*: Message request (defined in *nxd_dhcp_client.h*)

### Return Values

- **NX_SUCCESS**: (0x00) DHCP message sent  
- **NX_DHCP_NOT_STARTED**: (0x96) Invalid interface index
- **NX_DHCP_INVALID_MESSAGE**: (0x9B) Invalid message type to send
- NX_PTR_ERROR: (0x16) Invalid pointer input

### Allowed From

Threads

### Example

```c
/* Send the DHCP INFORM REQUEST message to the server.  */

status =  nx_dhcp_send_request(&my_dhcp, NX_DHCP_TYPE_DHCPINFORM);
/* If status is NX_SUCCESS a DHCP message was successfully sent.  */
```

## nx_dhcp_interface_send_request

Send DHCP message to Server on a specific interface

### Prototype

```c
UINT nx_dhcp_interface_send_request(
    NX_DHCP *dhcp_ptr, 
    UINT interface_index, 
    UINT dhcp_message_type);
```

### Description

This service sends a message to the DHCP server on the specified interface if that interface is enabled for DHCP. To send a RELEASE or DECLINE message, the application must use the ***nx_dhcp[_interface]_release*** or ***nx_dhcp_interface_decline*** services respectively.

The DHCP Client must be started to use this service except for sending the DHCP INFORM REQUEST message type.

This service is not intended for the host application to 'drive' the DHCP Client state machine.

### Input Parameters

- *dhcp_ptr*: Pointer to DHCP control block.
- *Interface_index*: Index of interface to send message on
- *dhcp_message_type*: Message request (defined in nxd_dhcp_client.h)

### Return Values

- **NX_SUCCESS**: (0x00) DHCP message sent  
- **NX_DHCP_NOT_STARTED**: (0x96) Invalid interface index
- **NX_DHCP_INVALID_MESSAGE**: (0x9B) Invalid message type to send
- **NX_DHCP_INTERFACE_NOT_ENABLED**: (0xA4) Interface not enabled for DHCP
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.
- NX_INVALID_INTERFACE: (0x4C) Invalid network interface

### Allowed From

Threads

### Example

```c
/* Send the INFORM REQUEST message to the server on the primary interface.  */

status =  nx_dhcp_interface_send_request(&my_dhcp, 0, NX_DHCP_TYPE_DHCPINFORM);
/* If status is NX_SUCCESS a DHCP message was successfully sent.  */
```

## nx_dhcp_server_address_get

Get the DHCP Client's DHCP server IP address

### Prototype

```c
UINT nx_dhcp_server_address_get(
    NX_DHCP *dhcp_ptr, 
    ULONG server_address);
```

### Description

This service retrieves the DHCP Client DHCP server IP address on the first interface enabled for DHCP found in the DHCP Client record. The caller can only use this service after the DHCP Client is bound to an IP address assigned by the DHCP Server. The host application can use the ***nx_ip_status_check*** service to verify IP address is set, or it can use the ***nx_dhcp_state_change_notify*** and query the DHCP Client state is **NX_DHCP_STATE_BOUND**. See *nx_dhcp_state_change_notify* for more details about setting the state change callback function.

To find the DHCP server on a specific interface when multiple interfaces are enabled for DHCP Client, use the *nx_dhcp_interface_server_address_get* service

### Input Parameters

- *dhcp_ptr*: Pointer to DHCP control block.
- *server_address*: Pointer to server IP address

### Return Values

- **NX_SUCCESS**: (0x00) DHCP server address returned
- **NX_DHCP_NO_INTERFACES_ENABLED**: (0xA5) No interfaces enabled for DHCP
- NX_PTR_ERROR: (0x16) Invalid input pointer
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c
/* Use the state change notify service to determine the Client transition to the bound state and get its DHCP server IP address.*/

void dhcp_state_change(NX_DHCP *dhcp_ptr, UCHAR new_state)
{
ULONG server_address;
UINT  status;

/* Increment state changes counter.  */
state_changes++;

if (dhcp_0.nx_dhcp_state == NX_DHCP_STATE_BOUND)
    	{
            status = nx_dhcp_server_address_get(&dhcp_0, &server_address);
    	}

}
```

## nx_dhcp_interface_server_address_get

Get the DHCP Client's DHCP server IP address on the specified interface

### Prototype

```c
UINT nx_dhcp_interface_server_address_get(
    NX_DHCP *dhcp_ptr, 
    UINT interface_index,
    ULONG server_address);
```

### Description

This service retrieves the DHCP Client DHCP server IP address on the specified interface if that interface is enabled for DHCP. The DHCP Client must be in the Bound state. After starting the DHCP Client on that interface, the host application can either use the ***nx_ip_status_check*** service to verify the IP address is set, or it can use the DHCP Client state change callback and query the DHCP Client state is NX_DHCP_STATE_BOUND. See ***nx_dhcp_state_change_notify*** for more details about setting the state change callback function.

### Input Parameters

- *dhcp_ptr*: Pointer to DHCP control block.
- *Interface_index*: Index of interface to obtain IP address
- *server_address*: Pointer to server IP address

### Return Values

- **NX_SUCCESS**: (0x00) DHCP server address returned
- **NX_DHCP_NO_INTERFACES_ENABLED**: (0xA5) No interfaces enabled for DHCP
- **NX_DHCP_NOT_BOUND**: (0x94) DHCP Client not bound
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.
- NX_INVALID_INTERFACE: (0x4C) Invalid network interface

### Allowed From

Threads

### Example

```c
/* Use the state change notify service to determine the Client transition to the 
bound state and get its DHCP server IP address. */

void dhcp_state_change(NX_DHCP *dhcp_ptr, UCHAR new_state)
{

ULONG server_address;
UINT  status;

/* Increment state changes counter.  */
state_changes++;

/* Get the DHCP server IP address on interface 1 */
if (dhcp_0.nx_dhcp_state == NX_DHCP_STATE_BOUND)
    	{
         status = nx_dhcp_interface_server_address_get(&dhcp_0, 1, 
                                                       &server_address);
    	}
}
```

## nx_dhcp_set_interface_index

Set network interface for DHCP instance

### Prototype

```c
UINT nx_dhcp_set_interface_index(
    NX_DHCP *dhcp_ptr,
    UINT index);
```

### Description

This service sets the network interface for the DHCP instance to connect to the DHCP Server on when running DHCP Client configured for a single network interface.

By default the DHCP Client runs on the primary interface. To run DHCP on a secondary service, use this service to set the secondary interface as the DHCP Client interface. The application must previously register the specified interface to the IP instance using the ***nx_ip_interface_attach*** service.

> **Note:** This service is intended for applications that intend to run the DHCP Client on only one interface. To run DHCP on multiple interfaces see ***nx_dhcp_interface_enable*** for more details.

### Input Parameters

- *dhcp_ptr*: Pointer to DHCP control block.  
- *index*: Index of device network interface

### Return Values

- **NX_SUCCESS**: (0x00) Interface is successfully set.
- **NX_INVALID_INTERFACE**: (0x4C) Invalid network interface
- **NX_DHCP_INTERFACE_ALREADY_ENABLED**: (0xA3) Interface enabled for DHCP
- **NX_DHCP_NO_RECORDS_AVAILABLE**: (0xA7) No record available for another 
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer

### Allowed From

Threads

### Example

```c
/* Set the DHCP Client interface to the secondary interface (index 1).  */
status =  nx_dhcp_set_interface_index(&my_dhcp, 1);
/* If status is NX_SUCCESS a DHCP interface was successfully set.  */
```

## nx_dhcp_start

Start DHCP processing

### Prototype

```c
UINT nx_dhcp_start(NX_DHCP *dhcp_ptr);
```

### Description

This service starts DHCP processing on all interfaces enabled for DHCP. By default the primary interface is enabled for DHCP when the application calls ***nx_dhcp_create***.

To verify when the IP instance is bound to an IP address on the DHCP Client interface, use *nx_ip_status_check* to see confirm the IP address is valid.

If there are other interfaces already running DHCP, this service will not affect them.

To start DHCP on a specific interface when multiple interfaces are enabled, use the ***nx_dhcp_interface_start*** service.

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance.

### Return Values

- **NX_SUCCESS**: (0x00) Successful DHCP start.  
- **NX_DHCP_ALREADY_STARTED**: (0x93) DHCP already started.
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of service.

### Allowed From

Threads

### Example

```c
/* Start the DHCP processing for this IP instance.  */
status =  nx_dhcp_start(&my_dhcp);

/* If status is NX_SUCCESS the DHCP was successfully started.  */
```

## nx_dhcp_interface_start

Start DHCP processing on the specified interface

### Prototype

```c
UINT nx_dhcp_interface_start(
    NX_DHCP *dhcp_ptr, 
    UINT interface_index);
```

### Description

This service starts DHCP processing on the specified interface if that interface is enabled for DHCP. See ***nx_dhcp_interface_enable*** for more details about enabling an interface for DHCP. By default the primary interface is enabled for DHCP when the application calls ***nx_dhcp_create**.*

If there are no other interfaces running DHCP Client this service will start/resume the DHCP Client thread and (re)activate the DHCP Client timer.  
  
The application should use ***nx_ip_status_check*** to verify if an IP address is obtained.

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance.
- *Interface_index*: Index on which to start the DHCP Client

### Return Values

- **NX_SUCCESS**: (0x00) Successful DHCP start. 
- **NX_DHCP_ALREADY_STARTED**: (0x93) DHCP already started.
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of service.
- NX_INVALID_INTERFACE: (0x4C) Invalid network interface

### Allowed From

Threads

### Example

```c
/* Start the DHCP processing for this IP instance on interface 1.  */
status =  nx_dhcp_interface_start(&my_dhcp, 1);

/* If status is NX_SUCCESS the DHCP was successfully started.  */
```

## nx_dhcp_state_change_notify

Set DHCP state change callback function

### Prototype

```c
UINT nx_dhcp_state_change_notify(
    NX_DHCP *dhcp_ptr, 
    VOID (*dhcp_state_change_notify)(
        NX_DHCP *dhcp_ptr,
        UCHAR new_state));
```

### Description

This service registers the specified callback function dhcp_state_change_notify for notifying an application of DHCP state changes. The callback function supplies the state the DHCP Client has transitioned into.

Following are values associated with the various DHCP states:

- **NX_DHCP_STATE_BOOT**: 1
- **NX_DHCP_STATE_INIT**: 2
- **NX_DHCP_STATE_SELECTING**: 3
- **NX_DHCP_STATE_REQUESTING**: 4
- **NX_DHCP_STATE_BOUND**: 5
- **NX_DHCP_STATE_RENEWING**: 6
- **NX_DHCP_STATE_REBINDING**: 7
- **NX_DHCP_STATE_FORCERENEW**: 8
- **NX_DHCP_STATE_ADDRESS_PROBING**: 9

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance.
- *dhcp_state_change_notify*: State change callback function pointer

### Return Values

- **NX_SUCCESS**: (0x00) Successful callback set.  
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of service.

### Allowed From

Threads, Initialization

### Example

```c
/* Register the "my_state_change" function to be called on any DHCP state change, assuming DHCP has already been created.  */
status =  nx_dhcp_state_change_notify(&my_dhcp, my_state_change);

/* If status is NX_SUCCESS the callback function was successfully
   registered.  */
```

## nx_dhcp_interface_state_change_notify

Set DHCP state change callback function on the specified interface

### Prototype

```c
UINT nx_dhcp_interface_state_change_notify(
    NX_DHCP *dhcp_ptr,
    UINT interface_index,
    VOID (*dhcp_state_change_notify)(
        NX_DHCP *dhcp_ptr, 
        UINT interface_index,
        UCHAR new_state));
```

### Description

This service registers the specified callback function for notifying an application of DHCP state changes. The callback function input arguments are the interface index and the state the DHCP Client has transitioned to on that interface.

For more information about state change functions, see *nx_dhcp_state_change_notify*().

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance.
- *dhcp_interface_state_change_notify*: Application callback function pointer

### Return Values

- **NX_SUCCESS**: (0x00) Successful callback set.  
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer.

### Allowed From

Threads, Initialization

### Example

```c
/* Register the "my_state_change" function to be called on any DHCP state change,   
   assuming DHCP has already been created.  */

void dhcp_interstate_state_change(NX_DHCP *dhcp_ptr, UINT iface_index, 
                                  UCHAR new_state);


status =  nx_dhcp_interstate_state_change_notify(&my_dhcp,  
                                                 dhcp_interstate_state_change);

/* If status is NX_SUCCESS the callback function was successfully
   registered.  */
```

## nx_dhcp_stop

Stops DHCP processing

### Prototype

```c
UINT nx_dhcp_stop(NX_DHCP *dhcp_ptr);
```

### Description

This service stops DHCP processing on all interfaces that have started DHCP processing. If there are no interfaces processing DHCP, this service will suspend the DHCP Client thread, and inactivate the DHCP Client timer.

To stop DHCP on a specific interface if multiple interfaces are enabled for DHCP, use the *nx_dhcp_interface_stop* service.

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance.

### Return Values

- **NX_SUCCESS**: (0x00) Successful DHCP stop
- **NX_DHCP_NOT_STARTED**: (0x96) The DHCP instance not started.
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of service.

### Allowed From

Threads

### Example

```c
/* Stop the DHCP processing for this IP instance.  */
status =  nx_dhcp_stop(&my_dhcp);

/* If status is NX_SUCCESS the DHCP was successfully stopped.  */
```

## nx_dhcp_interface_stop

Stop DHCP processing on the specified interface

### Prototype

```c
UINT nx_dhcp_interface_stop(
    NX_DHCP *dhcp_ptr,
    UINT interface_index);
```

### Description

This service stops DHCP processing on the specified interface if DHCP is already started. If there are no other interfaces running DHCP, the DHCP thread and timer are suspended.

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance.
- *Interface_index*: Interface on which to stop DHCP processing

### Return Values

- **NX_SUCCESS**: (0x00) Successful DHCP stop
- **NX_DHCP_NOT_STARTED**: (0x96) DHCP not started.
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of service.
- NX_INVALID_INTERFACE: (0x4C) Invalid network interface

### Allowed From

Threads

### Example

```c
/* Stop DHCP processing for this IP instance on interface 1.  */
status =  nx_dhcp_interface_stop(&my_dhcp, 1);

/* If status is NX_SUCCESS the DHCP was successfully stopped.  */
```

## nx_dhcp_user_option_retrieve

Retrieve a DHCP option from last server response

### Prototype

```c
UINT nx_dhcp_user_option_retrieve(
    NX_DHCP *dhcp_ptr,
    UINT request_option, 
    UCHAR *destination_ptr,
    UINT *destination_size);
```

### Description

This service retrieves the specified DHCP option from the DHCP options buffer on the first interface enabled for DHCP found on the DHCP Client record. If successful, the option data is copied into the specified buffer.

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance.  
- *request_option*: DHCP option, as specified by the RFCs. See the NX_DHCP_OPTION option in *nxd_dhcp_client.h*.
- *destination_ptr*: Pointer to the destination for the response string.  
- *destination_size*: Pointer to the size of the destination and on return, the destination to place the number of bytes returned.

### Return Values

- **NX_SUCCESS**: (0x00) Successful option retrieval.  
- **NX_DHCP_NOT_BOUND**: (0x94) DHCP Client not bound.
- **NX_DHCP_NO_INTERFACES_ENABLED**: (0xA5) No interfaces enabled for DHCP
- **NX_DHCP_DEST_TO_SMALL**: (0x95) Destination is too small to hold response.
- **NX_DHCP_PARSE_ERROR**: (0x97) Option not found in Server response.
- NX_PTR_ERROR: (0x16) Invalid input pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c
UCHAR  dns_ip_string[4];
ULONG  size;

/* Obtain the IP address of the DNS server.  */
size =    sizeof(dnx_ip_string);
status =  nx_dhcp_user_option_retrieve(&my_dhcp, NX_DHCP_OPTION_DNS_SVR,
                                        dns_ip_string, &size);

/* If status is NX_SUCCESS the DNS IP address is in dns_ip_string.  */
```

## nx_dhcp_interface_user_option_retrieve

Retrieve a DHCP option from last server response on the specified interface

### Prototype

```c
UINT nx_dhcp_interface_user_option_retrieve(
    NX_DHCP *dhcp_ptr,
    UINT interface_index,
    UINT request_option, UCHAR *destination_ptr,
    UINT *destination_size);
```

### Description

This service retrieves the specified DHCP option from the DHCP options buffer on the specified interface, if that interface is enabled for DHCP. If successful, the option data is copied into the specified buffer.

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance.
- *Interface_index*: Index on which to retrieve the specified option
- *request_option*: DHCP option, as specified by the RFCs. See the NX_DHCP_OPTION option: in *nxd_dhcp_client.h*.  
- *destination_ptr*: Pointer to the destination for the response string.  
- *destination_size*: Pointer to the size of the destination and on return, the destination to place the number of bytes returned.

### Return Values

- **NX_SUCCESS**: (0x00) Successful option retrieval.  
- **NX_DHCP_NOT_BOUND**: (0x94) IP address not assigned
- **NX_DHCP_DEST_TO_SMALL**: (0x95) Buffer is too small
- **NX_DHCP_PARSE_ERROR**: (0x97) DHCP Option not found in Server response.
- NX_PTR_ERROR: (0x16) Invalid DHCP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of service.
- NX_INVALID_INTERFACE: (0x4C) Invalid network interface

### Allowed From

Threads

### Example

```c
UCHAR  dns_ip_string[4];
ULONG  size;

/* Obtain the IP address of the DNS server on the primary interface.  */
size =    sizeof(dnx_ip_string);
status =  nx_dhcp_interface_user_option_retrieve(&my_dhcp, 0, NX_DHCP_OPTION_DNS_SVR,
                                                  dns_ip_string, &size);

/* If status is NX_SUCCESS the DNS IP address is in dns_ip_string.  */
```

## nx_dhcp_user_option_convert

Convert four bytes to ULONG

### Prototype

```c
ULONG nx_dhcp_user_option_convert(UCHAR *option_string_ptr);
```

### Description

This service converts the four characters pointed to by *"option_string_ptr"* into an unsigned long value. It is especially useful when IP addresses are  
present.

### Input Parameters

- *option_string_ptr*: Pointer to previously retrieved option string.

### Return Values

- **Value**: Value of first four bytes.

### Allowed From

Threads

### Example

```c
UCHAR  dns_ip_string[4];
ULONG  dns_ip;

/* Convert the first four bytes of "dns_ip_string" to an actual IP
address in "dns_ip."  */
dns_ip=  nx_dhcp_user_option_convert(dns_ip_string);

/* If status is NX_SUCCESS the DNS IP address is in "dns_ip."  */
```

## nx_dhcp_user_option_add_callback_set

Set callback function for adding user supplied options

### Prototype

```c
ULONG nx_dhcp_user_option_add_callback_set(
    NX_DHCP *dhcp_ptr, 
    UINT (*dhcp_user_option_add)(
        NX_DHCP *dhcp_ptr, 
        UINT iface_index, 
        UINT message_type, 
        UCHAR *user_option_ptr, 
        UINT *user_option_length));
```

### Description

This service registers the specified callback function for adding user supplied options.

If the callback function specified, the applications can add user supplied options into the packet by iface_index and message_type.

> **Note:** In user's routine. Applications must follow the DHCP options format when add user supplied options. The total size of user options must be less or equal to user_option_length, and update the user_option_length as real options length. Return NX_TRUE if add options successfully, else return NX_FALSE.

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance.
- *dhcp_user_option_add*: Pointer to user option add function.

### Return Values

- **NX_SUCCESS**: (0x00) Successful callback set.
- NX_PTR_ERROR: (0x16) Invalid pointer.

### Allowed From

Threads

### Example

```c
/* Register the "my_dhcp_user_option_add" function to be called when add DHCP
options, assuming DHCP has already been created.  */

status =  nx_dhcp_user_option_add_callback_set(&my_dhcp, my_dhcp_user_option_add);

/* If status is NX_SUCCESS the callback function was successfully registered.  */

```

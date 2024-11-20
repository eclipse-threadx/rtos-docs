---
title: Chapter 3 - Description of NetX Duo DHCP server services
description: This chapter contains a description of all NetX Duo DHCP Server services (listed below) in alphabetic order.
---


This chapter contains a description of all NetX Duo DHCP Server services (listed below) in alphabetic order.

> **Note:** In the **Return Values** section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## nx_dhcp_server create

Create a DHCP Server instance

### Prototype

```C
UINT nx_dhcp_server_create(
    NX_DHCP_SERVER *dhcp_ptr,
    NX_IP *ip_ptr,
    VOID *stack_ptr,
    ULONG stack_size,
    CHAR *input_address_list,
    CHAR *name_ptr,
    NX_PACKET_POOL *packet_pool_ptr);
```

### Description

This service creates a DHCP Server instance with a previously created IP instance.

> **Important:** The application must make sure the packet pool created for the IP create service has a minimum 548 byte payload, not including the UDP, IP and Ethernet headers.

### Input Parameters

- *dhcp_ptr*: Pointer to DHCP Server control block.
- *ip_ptr*: Pointer to DHCP Server IP instance.
- *stack_ptr*: Pointer DHCP Server stack location.
- *stack_size*: Size of DHCP Server stack input_address_list Pointer to Server's list of IP addresses
- *name_ptr*: Pointer to DHCP Server name packet_pool_ptr Pointer to DHCP Server packet pool

### Return Values

- **NX_SUCCESS** (0x00) Successful DHCP Server create.
- **NX_DHCP_INADEQUATE_PACKET_POOL_PAYLOAD** (0xA9) Packet payload too small error
- **NX_DHCP_NO_SERVER_OPTION_LIST** (0x96) Server option list is empty
- NX_DHCP_PARAMETER_ERROR (0x92) Invalid non pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of service.
- NX_PTR_ERROR (0x16) Invalid pointer input.

### Allowed From

Application

### Example

```C
/* Create a DHCP Server instance. */

status = nx_dhcp_server_create(&dhcp_server, &server_ip, pointer,
    DEMO_SERVER_STACK_SIZE, SERVER_IP_ADDRESS_LIST, "DHCP server", &server_pool);

/* If status is NX_SUCCESS a DHCP Server instance was successfully created. */
```

## nx_dhcp_create_server_ip_address_list

Create a IP address pool

### Prototype

```C
UINT nx_dhcp_create_server_ip_address_list(
    NX_DHCP_SERVER *dhcp_ptr,
    UINT iface_index,
    ULONG start_ip_address,
    ULONG end_ip_address,
    UINT *addresses_added);
```

### Description

This service creates a network interface specific pool of available IP addresses for the specified DHCP server to assign. The start and end IP addresses must match the specified network interface. The actual number of IP addresses added may be less than the total addresses if the IP address list is not large enough (which is set in the user configurable **NX_DHCP_IP_ADDRESS_MAX_LIST_SIZE** parameter).

### Input Parameters

- *dhcp_ptr*: Pointer to DHCP Server control block.
- *iface_index*: Index corresponding to network interface
- *start_ip_address*: First available IP address
- *end_ip_address*: Last of the available IP address
- *addresses_added*: Number of IP addresses added to list

### Return Values

- **NX_SUCCESS** (0x00) Successful DHCP Server create.
- **NX_DHCP_SERVER_BAD_INTERFACE_INDEX** (0xA1) Index does not match addresses
- **NX_DHCP_INVALID_IP_ADDRESS_LIST** (0x99) Invalid address input
- NX_DHCP_INVALID_IP_ADDRESS (0x9B) Illogical start/end addresses
- NX_PTR_ERROR (0x16) Invalid pointer input.

### Allowed From

Application

### Example

```C
/* Create a pool of available IP addresses to assign. */

status = nx_dhcp_create_server_ip_list(&dhcp_server, iface_index,
    START_IP_ADDRESS_LIST, END_IP_ADDRESS_LIST, &addresses_added);

/* If status is NX_SUCCESS a IP address list was successfully created.
    addresses_added indicates how many IP addresses were actually added to the
    list. */
```

## nx_dhcp_clear_client_record

Remove Client record from Server database

### Prototype

```C
UINT nx_dhcp_clear_client_record(
    NX_DHCP_SERVER *dhcp_ptr,
    NX_DHCP_CLIENT *dhcp_client_ptr);
```

### Description

This service clears the Client record from the Server database.

### Input Parameters

- *dhcp_ptr*: Pointer to DHCP Server control block.
- *dhcp_client_ptr*: Pointer to DHCP Client to remove

### Return Values

- **NX_SUCCESS** (0x00) Successful DHCP Server create.
- NX_PTR_ERROR (0x16) Invalid pointer input.
- NX_CALLER_ERROR (0x11) Non thread caller of service

### Allowed From

Application

### Example

```C
/* Remove Client record from the server database. */
status = nx_dhcp_clear_client_record(&dhcp_server, &dhcp_client_ptr);

/* If status is NX_SUCCESS the specified Client was removed from the database. */
```

## nx_dhcp_set_interface_network_parameters

Set network parameters for DHCP options

### Prototype

```C
UINT nx_dhcp_set_interface_network_parameters(
    NX_DHCP_SERVER *dhcp_ptr,
    UINT iface_index, 
    ULONG subnet_mask,
    ULONG default_gateway_address,
    ULONG dns_server_address);
```

### Description

This service sets default values for network critical parameters for the specified interface. The DHCP server will include these options in its OFFER and ACK replies to the DHCP Client. If the host set interface parameters on which a DHCP server is running, the parameters will defaulted as follows: the router set to the primary interface gateway for the DHCP server itself, the DNS server address to the DHCP server itself, and the subnet mask to the same as the DHCP server interface is configured with.

### Input Parameters

- *dhcp_ptr*: Pointer to DHCP Server control block.
- *iface_index*: Index corresponding to network interface
- *subnet_mask*: Subnet mask for Client network
- *default_gateway_address*: Client's router IP address
- *dns_server_address*: DNS server for Client's network

### Return Values

- **NX_SUCCESS** (0x00) Successful DHCP Server create.
- **NX_DHCP_SERVER_BAD_INTERFACE_INDEX** (0xA1) Index does not match addresses
- **NX_DHCP_INVALID_NETWORK_PARAMETERS** (0xA3) Invalid network parameters
- NX_PTR_ERROR (0x16) Invalid pointer input.

### Allowed From

Application

### Example

```C
/* Set network parameters for a specific interface. */

status = nx_dhcp_set_interface_network_parameters(&dhcp_server, iface_index,
    NX_DHCP_SUBNET_MASK, NX_DHCP_DEFAULT_GATEWAY,
    NX_DHCP_DNS_SERVER);

/* If status is NX_SUCCESS network parameters were successfully set. */
```

## nx_dhcp_server_delete

Delete a DHCP Server instance

### Prototype

```C
UINT nx_dhcp_server_delete(NX_DHCP_SERVER *dhcp_ptr);
```

### Description

This service deletes a previously created DHCP Server instance.

### Input Parameters

- *dhcp_ptr*: Pointer to a DHCP Server instance.

### Return Values

- **NX_SUCCESS** (0x00) Successful DHCP Server delete.
- NX_PTR_ERROR (0x16) Invalid pointer input.
- NX_DHCP_PARAMETER_ERROR (0x92) Invalid non pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of service.

### Allowed From

Threads

### Example

```C
/* Delete a DHCP Server instance. */

status = nx_dhcp_server_delete(&dhcp_server);

/* If status is NX_SUCCESS the DHCP Server instance was successfully deleted. */
```

## nx_dhcp_server_start

Start DHCP Server processing

### Prototype

```C
UINT nx_dhcp_server_start(NX_DHCP_SERVER *dhcp_ptr);
```

### Description

This service starts DHCP Server processing, which includes creating a server UDP socket, binding the DHCP port and waiting to receive Client DHCP requests.

### Input Parameters

- *dhcp_ptr*: Pointer to previously created DHCP instance.

### Return Values

- **NX_SUCCESS** (0x00) Successful Server start.
- **NX_DHCP_ALREADY_STARTED** (0x93) DHCP already started.
- NX_PTR_ERROR (0x16) Invalid pointer input.
- NX_CALLER_ERROR (0x11) Invalid caller of service.
- NX_DHCP_PARAMETER_ERROR (0x92) Invalid non pointer input

### Allowed From

Threads

### Example

```C
/* Start the DHCP Server processing for this IP instance. */

status = nx_dhcp_server_start(&dhcp_server);

/* If status is NX_SUCCESS the DHCP Server was successfully started. */
```

## nx_dhcp_server_stop

Stops DHCP Server processing

### Prototype

```C
UINT nx_dhcp_server_stop(NX_DHCP_SERVER *dhcp_ptr);
```

### Description

This service stops DHCP Server processing, which includes of receiving DHCP Client requests.

### Input Parameters

- *dhcp_ptr*: Pointer to DHCP Server instance.

### Return Values

- **NX_SUCCESS** (0x00) Successful DHCP stop.
- **NX_DHCP_ALREADY_STARTED** (0x93) DHCP already started.
- NX_PTR_ERROR (0x16) Invalid pointer input.
- NX_CALLER_ERROR (0x11) Invalid caller of service.
- NX_DHCP_PARAMETER_ERROR (0x92) Invalid non pointer input

### Allowed From

Threads

### Example

```C
/* Stop the DHCP Server processing for this IP instance. */

status = nx_dhcp_server_stop(&dhcp_server);

/* If status is NX_SUCCESS the DHCP Server was successfully stopped. */
```

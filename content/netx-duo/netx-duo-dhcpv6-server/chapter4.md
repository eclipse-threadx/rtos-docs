---
title: Chapter 4 - NetX Duo DHCPv6 server services
description: This chapter contains a description of all NetX Duo DHCPv6Server services.
---


This chapter contains a description of all NetX Duo DHCPv6Server services (listed below).

In the **Return Values** section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## nx_dhcpv6_create_dns_address

Set the network DNS server

### Prototype

```
UINT nx_dhcpv6_create_dns_address(
     NX_DHCPV6_SERVER *dhcpv6_server_ptr, 
     NXD_ADDRESS *dns_ipv6_address);
```

### Description

This service loads the DHCPv6 Server with the DNS server address for the Server DHCPv6 network interface.

### Input Parameters

- *dhcpv6_server_ptr*: Pointer to DHCPv6 Server
- *dns_ipv6_address*: Pointer to the DNS server

### Return Values

- **NX_SUCCESS** (0x00) DNS Server saved to DHCPv6 Server instance
- **NX_DHCPV6_INVALID_INTERFACE_IP_ADDRESS** (0xE95) An invalid address is supplied
- NX_PTR_ERROR (0x16) Invalid pointer input

### Allowed From

Application Code

### Example

```
/* Set the network DNS server with the input address for the Server DHCPv6interface. */
status = nx_dhcpv6_create__dns_address(&dhcp_server_0, &dns_ipv6_address);
/* If this service returns NX_SUCCESS the DNS server data was accepted. */
```

## nx_dhcpv6_create_ip_address_range

Create the Server IP address list

### Prototype

```
UINT _nx_dhcpv6_create_ip_address_range(
     NX_DHCPV6_SERVER *dhcpv6_server_ptr, 
     NXD_ADDRESS *start_ipv6_address, NXD_ADDRESS *end_ipv6_address, 
     UINT *addresses_added)
```

### Description

This service creates the IP address list specified by the start and end addresses of the Server's assignable address range. The start and end addresses must match the Server interface address prefix (must be on the same link as the Server DHCPv6 interface). The number of addresses actually added is returned.

### Input Parameters

- *dhcpv6_server_ptr*: Pointer to DHCPv6 Server
- *start_ipv6_address*: Start of addresses to add
- *end_ipv6_address*: End of addresses to add
- *addresses_added*: Output of addresses added

### Return Values

- **NX_SUCCESS** (0x00) IP address list successfully created
- **NX_DHCPV6_INVALID_INTERFACE_IP_ADDRESS** (0xE95) An invalid address is supplied
- NX_PTR_ERROR (0x16) Invalid pointer input

### Allowed From

Application Code

### Example

```
/* Create the Server IP address list for the server DHCPv6 interface. */
status = nx_dhcpv6_create_ip_address_range(&dhcp_server_0,
         &start_ipv6_address, &end_ipv6_address, &addresses_reserved);
/* If status is NX_SUCCESS one or more addresses were successfully added. */
```

## nx_dhcpv6_reserve_ip_address_range

Reserve specified range of IP addresses

### Prototype

```
UINT _nx_dhcpv6_reserve_ip_address_range(
     NX_DHCPV6_SERVER *dhcpv6_server_ptr, 
     NXD_ADDRESS *start_ipv6_address, NXD_ADDRESS *end_ipv6_address, 
     UINT *addresses_reserved)
```

### Description

This service reserves the IP address range specified by the start and end addresses. These addresses must be within in the previously created server IP address range. These addresses will not be assigned to any Clients by the DHCPv6 Server. The start and end addresses must match the Server interface address prefix (must be on the same link as the Server DHCPv6 network interface). The number of addresses actually reserved is returned.

### Input Parameters

- *dhcpv6_server_ptr*: Pointer to DHCPv6 Server
- *start_ipv6_address*: Start of addresses to reserve
- *end_ipv6_address*: End of addresses to reserve
- **addresses_reserved*: Number of addresses reserved

### Return Values

- **NX_SUCCESS** (0x00) RELEASE message successfully created and processed
- **NX_DHCPV6_INVALID_INTERFACE_IP_ADDRESS** (0xE95) An invalid address is supplied
- **NX_DHCPV6_INVALID_IP_ADDRESS** (0xED1) Starting address not found in Server address list.
- NX_PTR_ERROR (0x16) Invalid pointer input

### Allowed From

Application Code

### Example

```
/* Reserve a range of ip addresses in the Server address table for the server DHCPv6 
network interface. */

status = nx_dhcpv6_reserve_ip_address_range(&dhcp_server_0,
         &start_ipv6_address, &end_ipv6_address, &addresses_reserved);

/* If status is NX_SUCCESS one or more addresses were successfully reserved. */
```

## nx_dhcpv6_server_create

Create the DHCPv6 Server instance

### Prototype

```
UINT nx_dhcpv6_server_create(NX_DHCPV6_SERVER *dhcpv6_server_ptr, 
        NX_IP *ip_ptr, CHAR *name_ptr, 
        NX_PACKET_POOL *packet_pool_ptr, 
        VOID *stack_ptr,ULONG stack_size,
    VOID (*dhcpv6_address_declined_handler)(struct 
        NX_DHCPV6_SERVER_STRUCT *dhcpv6_server_ptr, 
        NX_DHCPV6_CLIENT *dhcpv6_client_ptr, 
        UINT message),
    VOID (*dhcpv6_option_request_handler)(
        struct NX_DHCPV6_SERVER_STRUCT *dhcpv6_server_ptr, 
        UINT option_request, UCHAR *buffer_ptr, UINT *index));
```

### Description

This service creates the DHCPv6 Server task with the specified input. The callback handlers are optional input. The stack pointer, IP instance and packet pool input are required. The IP instance and packet pool must already be created.

User is encouraged to call nx_dhcpv6_server_option_request_handler_set to set the option request handler.

### Input Parameters

- *dhcpv6_server_ptr*: Pointer to DHCPv6 Server
- *ip_ptr*: Pointer to the IP instance
- *name_str*: Pointer to Server name
- *packet_pool_ptr*: Pointer to Server packet pool
- *stack_ptr*: Pointer to Server stack memory
- *stack_size*: Size of Server stack memory
- *dhcpv6_address_declined_handler*: Pointer to Client Decline or Release message handler
- *dhcpv6_option_request_handler*: Pointer to options request option handler

### Return Values

- **NX_SUCCESS** (0x00) Server successfully resumed
- NX_PTR_ERROR (0x16) Invalid pointer input
- NX_DHCPV6_PARAM_ERROR Invalid non pointer input

### Allowed From

Application Code

### Example

```
/* Create the DHCPv6 Server. */
status = nx_dhcpv6_server_create(&dhcp_server_0, &ip_0, "DHCPv6 Server",
         &pool_0, stack_pointer,2048, dhcpv6_decline_handler,
         dhcpv6_get_time_handler);
/* If status is NX_SUCCESS the Server successfully created. */
```

## nx_dhcpv6_server_delete

Delete the DHCPv6 Server

### Prototype

```
UINT _nx_dhcpv6_server_delete(NX_DHCPV6_SERVER *dhcpv6_server_ptr)
```

### Description

This service deletes the DHCPv6 Server task and any request that the Server was processing.

### Input Parameters

- *dhcpv6_server_ptr*: Pointer to DHCPv6 Server

### Return Values

- **NX_SUCCESS** (0x00) Server successfully deleted
- NX_PTR_ERROR (0x16) Invalid pointer input

### Allowed From

Threads

### Example

```
/* Delete the DHCPv6 Serve. */
status = nx_dhcpv6_server_delete(&dhcp_server_0);
/* If status is NX_SUCCESS the Server successfully deleted. */
```

## nx_dhcpv6_server_resume

Resume DHCPv6 Server task

### Prototype

```
UINT _nx_dhcpv6_server_resume(NX_DHCPV6_SERVER *dhcpv6_server_ptr)
```

### Description

This service resumes the DHCPv6 Server task and any request that the Server was processing.

### Input Parameters

- *dhcpv6_server_ptr*: Pointer to DHCPv6 Server

### Return Values

- **NX_SUCCESS** (0x00) Server successfully resumed
- **NX_DHCPV6_ALREADY_STARTED** (0xE91) Server is running already
- **status** (variable) ThreadX and NetX Duo error status
- NX_PTR_ERROR (0x16) Invalid pointer input

### Allowed From

Threads

### Example

```
/* Resume the DHCPv6 Server task. */
status = nx_dhcpv6_server_resume(&dhcp_server_0);
/* If status is NX_SUCCESS the Server successfully resumed. */
```

## nx_dhcpv6_server_suspend

Suspend DHCPv6 Server task

### Prototype

```
UINT _nx_dhcpv6_server_suspend(NX_DHCPV6_SERVER *dhcpv6_server_ptr)
```

### Description

This service suspends the DHCPv6 Server task and any request that the Server was processing.

### Input Parameters

- *dhcpv6_server_ptr*: Pointer to DHCPv6 Server

### Return Values

- **NX_SUCCESS** (0x00) Server successfully resumed
- **NX_DHCPV6_NOT_STARTED** (0xE92) Server is not started 
- **Status** (variable) ThreadX and NetX Duo error status
- NX_PTR_ERROR (0x16) Invalid pointer input

### Allowed From

Threads

### Example

```
/* Suspend the DHCPv6 Server task. */
status = nx_dhcpv6_server_suspend(&dhcp_server_0);

/* If status is NX_SUCCESS the Server successfully suspended. */
```

## nx_dhcpv6_server_start

Start the DHCPv6 Server task

### Prototype

```
UINT _nx_dhcpv6_server_start(NX_DHCPV6_SERVER *dhcpv6_server_ptr)
```

### Description

This service starts the DHCPv6 Server task and readies the Server to process application requests for receiving DHCPv6 Client messages. It verifies the Server instance has sufficient information (Server DUID), creates and binds the UDP socket for sending and receiving DHCPv6 messages, and activates timers for keeping track of session time and IP lease expiration.

> **Note:** Before the DHCPv6 Server can run, the host application is responsible for creating the IP address range from which the Server can assign IP addresses. It is also responsible for setting the Server DUID and DHCPv6 interface (see *nx_dhcpv6_server_duid_set* and *nx_dhcpv6_server_interface_set* respectively.

### Input Parameters

- *dhcpv6_server_ptr*: Pointer to DHCPv6 Server

### Return Values

- **NX_SUCCESS** (0x00) Server successfully started
- **NX_DHCPV6_ALREADY_STARTED** (0xE91) Server is running already
- **NX_DHCPV6_NO_ASSIGNABLE_ADDRESSES** (0xEA7) Server has no assignable addresses to lease
- **NX_DHCPV6_INVALID_GLOBAL_INDEX** (0xE97) Global address index not set
- **NX_DHCPV6_NO_SERVER_DUID** (0xE92) No Server DUID created 
- **status** (variable) ThreadX and NetX Duo error status
- NX_PTR_ERROR (0x16) Invalid pointer input

### Allowed From

Threads

### Example

```
/* Start the DHCPv6 Server task. */
status = nx_dhcpv6_server_start(&dhcp_server_0);
/* If status is NX_SUCCESS the Server successfully started. */
```

## nx_dhcpv6_retrieve_ip_address_lease

Get an IP address lease from the Server table

### Prototype

```
UINT _nx_dhcpv6_retrieve_ip_address_lease(
     NX_DHCPV6_SERVER *dhcpv6_server_ptr, UINT table_index, 
     NXD_ADDRESS *lease_IP_address, ULONG *T1, ULONG *T2, 
     ULONG *valid_lifetime, ULONG *preferred_lifetime)
```

### Description

This service retrieves an IP address lease record from the Server table at the specified table index location. This can be done before or after retrieving Client record data.

The capability of storing and retrieving data between the DHCPv6 Server and non volatile memory is a requirement of the DHCPv6 protocol. It makes no difference in what order IP lease data and Client record data is saved to nonvolatile memory.

> **Note:** It is not recommended to copy data to or from Server tables without stopping or suspending the DHCPv6 Server first.

### Input Parameters

- *dhcpv6_server_ptr*: Pointer to DHCPv6 Server
- *table_index*: Table index to store lease at
- *lease_IP_address*: Pointer to IP address leased to the Client
- *T1*: Client requested renew time
- *T2*: Client requested rebind time
- *valid_lifetime*: Client lease becomes deprecated
- *preferred_lifetime*: Client lease becomes invalid

### Return Values

- **NX_SUCCESS** (0x00) Server successfully started
- **NX_DHCPV6_PARAMETER_ERROR** (0xE93) Invalid IP lease data input
- NX_PTR_ERROR (0x16) Invalid pointer input

### Allowed From

Application code

### Example

```
/* Retrieve the DHCPv6 Server lease data. */
For (I = 0; I < NX_DHCPV6_MAX_LEASES; i++)
{
    /* Get the next lease record. */
    status = nx_dhcpv6_server_startdhcpv6_server_ptr, i, &next_ipv6_address, &T1,
             &T2, &preferred_lifetime, &valid_lifetime);
    /* The host application then saves this record to memory.
}
/* If status is NX_SUCCESS the Server data is successfully downloaded. */
```

## nx_dhcpv6_add_ip_address_lease

Add an IP address lease to the Server table

### Prototype

```
UINT _nx_dhcpv6_add_ip_address_lease(
     NX_DHCPV6_SERVER *dhcpv6_server_ptr,
     UINT table_index,
     NXD_ADDRESS *lease_IP_address,
     ULONG T1,
     ULONG T2, 
     ULONG valid_lifetime,
     ULONG preferred_lifetime)
```

### Description

This service loads IP lease data from a previous DHCPv6 Server session from non volatile memory to the Server lease table. This is not necessary if the Server is running for the first time and has no previous lease data. If this is the case the host application must create an IP address range for assigning IP addresses, using the ***nx_dhcpv6_create_ip_address_range*** service. The data is sufficient to reconstruct a DHCPv6 lease record. The table index need not be specified. If set to 0xFFFFFFFF (infinity) the DHCPv6 Server will find the next available slot to copy the data to.

> **Note:** Uploading IP lease data MUST be done before uploading Client records; both MUST be done before (re)starting the DHCPv6 Server.

The capability of storing and retrieving data between the DHCPv6 Server and non volatile memory is a requirement of the DHCPv6 protocol.

### Input Parameters

- *dhcpv6_server_ptr*: Pointer to DHCPv6 Server
- *table_index*: Table index to store lease at
- *lease_IP_address*: Pointer to IP address leased to the Client
- *T1*: Client requested renew time
- *T2*: Client requested rebind time
- *valid_lifetime*: Client lease becomes deprecated
- *preferred_lifetime*: Client lease becomes invalid

### Return Values

- **NX_SUCCESS** (0x00) Server successfully started
- **NX_DHCPV6_TABLE_FULL** (0xEC4) No room for more lease data**
- **NX_DHCPV6_INVALID_INTERFACE_IP_ADDRESS** (0xE95) Lease data does not appear to be on link with Server DHCPv6 interface
- **NX_DHCPV6_PARAM_ERROR** (0xE93) Invalid IP lease data input
- NX_PTR_ERROR (0x16) Invalid pointer input

### Allowed From

Application code

### Example

```
/* Copy the IP lease data to the Server address table. Note that the table index
is defaulted to 0xFFFFFFFF meaning the DHCPv6 Server will find an empty slot 
for each lease. */

    For(I = 0; I < NX_DHCPV6_MAX_LEASES; i++)
    {
        status = nx_dhcpv6_add_ip_address_lease(dhcpv6_server_ptr, 0xFFFFFFFF,
                 &next_ipv6_address, &T1, &T2, &preferred_lifetime, &valid_lifetime);
        /* Get the next lease address from memory… */
    }
    
    /* If status is NX_SUCCESS the lease data was successfully uploaded. It is opk
    to add the Client records to the Server table now. */
```

## nx_dhcpv6_add_client_record

Add a Client record to the Server table

### Prototype

```
UINT _nx_dhcpv6_add_client_record(
     NX_DHCPV6_SERVER *dhcpv6_server_ptr, 
     UINT table_index,
     ULONG message_xid, 
     NXD_ADDRESS *client_address,
     UINT client_state, 
     ULONG IP_lease_time_accrued,
     ULONG valid_lifetime,
     UINT duid_type,
     UINTduid_hardware, 
     ULONG physical_address_msw,
     ULONG physical_address_lsw,
     ULONG duid_time, 
     ULONG duid_vendor_number,
     UCHAR *duid_vendor_private,
     UINT duid_private_length)
```

### Description

This service copies Client data from non volatile memory to the Server table one record at a time. This is only necessary if the Server is being rebooted and has Client data from a previous session to restore from memory. If a Server has no previous data, the DHCPv6 Server will initialize the Client table to be able for adding Client records.

It is not necessary to specify the table index. If set to 0xFFFFFFFF (infinity) the DHCPv6 Server will locate the next available slot. The DHCPv6 Server can reconstruct a Client record from this data.

> **Note:** The host application MUST upload the IP lease data BEFORE the Client record data. This is so that internally the DHCPv6 Server can cross link the tables so that each Client record is joined with its corresponding IP lease record in their respective tables. See *nx_dhcpv6_add_ip_address_lease* for details on uploading IP lease data from memory.

> **Note:** Depending on DUID type, not all data must be supplied. For example if a Client has a vendor assigned DUID type, it can send in zero for DUID Link Layer parameters (MAC address, hardware type, DUID time).

The capability of storing and retrieving data between the DHCPv6 Server and non volatile memory is a requirement of the DHCPv6 protocol.

### Input Parameters

- *dhcpv6_server_ptr*: Pointer to DHCPv6 Server

### Return Values

- **NX_SUCCESS** (0x00) Server successfully started
- **NX_ INVALID_PARAMETERS** (0x4D) Invalid non pointer input**
- **NX_DHCPV6_TABLE_FULL** (0xEC4) No empty slots left for adding another Client record
- **NX_DHCPV6_ADDRESS_NOT_FOUND** (0xEA8) Client assigned address not found in Server lease table.
- NX_PTR_ERROR (0x16) Invalid pointer input

### Allowed From

Application code

### Example

```
/*Add the IP lease data and Client records back to the server before starting
theServer. */
    /* Copy the 'lease data' to the server table FIRST. */
for (i = 0; i< NX_DHCPV6_MAX_LEASES; i++)
    {

        /* Add the next lease record. Let the server find the next
        available slot. */
        status = nx_dhcpv6_add_ip_address_lease(dhcpv6_server_ptr,
                 0xFFFFFFFF,,&next_ipv6_address, NX_DHCPV6_DEFAULT_T1_TIME, 
                 NX_DHCPV6_DEFAULT_T2_TIME, NX_DHCPV6_DEFAULT_PREFERRED_TIME, 
                 NX_DHCPV6_DEFAULT_VALID_TIME);
if (status != NX_SUCCESS)
return status;
        /* Get the next IP lease record from memory. */
        …
    }
    /* Copy the client records to the Server table NEXT.
    for (i = 0; i< NX_DHCPV6_MAX_LEASES; i++)
    {
        /* Add the next client record. Let the server find the next
        available slot. */
        status = nx_dhcpv6_add_client_record(dhcpv6_server_ptr, 0xFFFFFFFF,
                 message_xid, &client_ipv6_address, NX_DHCPV6_STATE_BOUND,
                 IP_lifetime_time_accrued, valid_lifetime, duid_type, 
                 duid_hardware, physical_address_msw, physical_address_lsw, 
                 duid_time, 0, NX_NULL, 0);
if (status != NX_SUCCESS)
return status;
        /* Get the next Client record from memory. */
        …
    }

/* If status is NX_SUCCESS the Server data was successfully restored and 
it is ok to start the DHCPv6 server now. */
```

## nx_dhcpv6_retrieve_client_record

Retrieve a Client record from the Server table

### Prototype

```
UINT _nx_dhcpv6_retrieve_client_record(
     NX_DHCPV6_SERVER *dhcpv6_server_ptr, 
     UINT table_index,
     ULONG *message_xid, 
     NXD_ADDRESS *client_address,
     UINT *client_state, 
     ULONG IP_lease_time_accrued, 
     ULONG *valid_lifetime,
     UINT *duid_type, 
     UINT *duid_hardware,
     ULONG *physical_address_msw, 
     ULONG *physical_address_lsw,
     ULONG *duid_time, 
     ULONG *duid_vendor_number, 
     UCHAR *duid_vendor_private, 
     UINT *duid_private_length)
```

### Description

This service copies the essential data from the Server's Client record table for storage to non-volatile memory. The Server can reconstruct an adequate Client record from such data in the reverse process (uploading data to the Server table). Regardless of the DUID type, none of the pointers can be NULL pointers; data is initialized to zero for all parameters. For example, if the Client DUID type is Link Layer Plus Time, the vendor number is returned as zero and the private ID is an empty string.

The capability of storing and retrieving data between the DHCPv6 Server and non volatile memory is a requirement of the DHCPv6 protocol. It makes no difference in what order IP lease data and Client record data is saved to nonvolatile memory.

> **Note:** It is not recommended to copy data to or from Server tables without stopping or suspending the DHCPv6 Server first.

### Input Parameters

- *dhcpv6_server_ptr*: Pointer to DHCPv6 Server
- *table_index*: Index into Server's client table
- *message_xid*: Client Server Transaction ID
- *client_address*: IPv6 address leased to Client
- *client_state*: Client DHCPv6 state (e.g. bound)
- *IP_lease_time_accrued*: Time expired on lease already *dhcpv6_server_ptr* Pointer to DHCPv6 Server
- *dhcpv6_server_ptr*: Pointer to DHCPv6 Server

### Return Values

- **NX_SUCCESS** (0x00) Server successfully started
- **NX_DHCPV6_INVALID_DUID** (0xECC) Invalid or inconsistent DUID data
- **NX_PTR_ERROR** (0x16) Invalid pointer input
- NX_INVALID_PARAMETERS (0x4D) Invalid non pointer input

### Allowed From

Application code

### Example

```
/* Retrieve the Client records from the DHCPv6 Server table. */
For (i = 0; i< NX_MAX_DHCPV6_CLIENTS; i++)
{
    status = nx_dhcpv6_retrieve_client_recorddhcpv6_server_ptr, i, &message_xid,
             &client_ipv6_address, &client_state, &IP_lifetime_time_accrued, 
             valid_lifetime, &duid_type, &duid_hardware, &physical_address_msw,
             &physical_address_lsw, &duid_time, &duid_vendor_number, &private_id[0], 
             &length);
    /* The host application can save this data to memory now.
}
/* If status is NX_SUCCESS the Server successfully started. */
```

## nx_dhcpv6_server_interface_set

Set the interface index for Server DHCPv6 interface

### Prototype

```
UINT _nx_dhcpv6_server_interface_set(
     NX_DHCPV6_SERVER *dhcpv6_server_ptr, 
     UINT iface_index,
     UINT ga_address_index)
```

### Description

This service sets the network interface on which the DHCPv6 Server handles DHCPv6 Client requests. Not that for versions of NetX Duo that do not support multihome, the interface value is defaulted to zero. The global address index is necessary to obtain the Server global address on its DHCPv6 interface. This is used by the DHCPv6 logic to ensure that lease addresses and other DHCPv6 data is on link with the DHCPv6 Server.

This must be called before the DHCPv6 server is started, even for applications on single homed devices or without multihome support.

### Input Parameters

- *dhcpv6_server_ptr*: Pointer to DHCPv6 Server
- *iface_index*: Server DHCPv6 Server interface
- *ga_address_index*: Index of Server global address in the Server IP instance address table

### Return Values

- **NX_SUCCESS** (0x00) Server successfully started
- **NX_INVALID_INTERFACE** (0x4C) Interface does not exist
- NX_NO_INTERFACE_ADDRESS (0x50) Global index exceeds the IP instance maximum IPv6 addresses (NX_MAX_IPV6_ADDRESSES)
- NX_PTR_ERROR (0x16) Invalid pointer input

### Allowed From

Application code

### Example

```
/* Set the Server DHCPv6 interface to the primary interface. The global IP 
address is at the index 1 in the IP address table. */
status = nx_dhcpv6_server_interface_set(&dhcp_server_0, 0, 1);

/* If status is NX_SUCCESS the Server interface is successfully set. */
```
## nx_dhcpv6_set_server_duid

Set the Server DUID for DHCPv6 packets

### Prototype

```
UINT _nx_dhcpv6_set_server_duid(
     NX_DHCPV6_SERVER *dhcpv6_server_ptr,
     UINT duid_type,
     UINT hardware_type, 
     ULONG mac_address_msw,
     ULONG mac_address_lsw, 
     ULONG time)
```

### Description

This service sets the Server DUID and must be called before the host application starts the Server. For link layer and link layer time DUID types, the host application must supply the hardware type and MAC address data. For link layer time DUIDs, the time pointer must point to a valid time. The number of seconds since Jan 1, 2000 is a typical seed value. If the Server DUID type is the enterprise, vendor assigned type, the DUID will be created from the user configurable options NX_DHCPV6_SERVER_DUID_VENDOR_PRIVATE_ID and NX_DHCPV6_SERVER_DUID_VENDOR_ASSIGNED_ID, and the time and MAC address values can be set to NULL.

> **Note:** It is the host application's responsibility to save the Server DUID parameters to nonvolatile memory such that it uses the same DUID in messages to Clients between reboots. This is a requirement of the DHCPv6 protocol (RFC 3315).

### Input Parameters

- *dhcpv6_server_ptr*: Pointer to DHCPv6 Server
- *duid_type*: DHCPv6 Server DUID type
- *hardware_type*: Hardware type (e.g. Ethernet)
- *mac_address_msw*: Pointer to DHCPv6 Server
- *mac_address_lsw*: Pointer to DHCPv6 Server
- *time*: Time value for DUID

### Return Values

- **NX_SUCCESS** (0x00) Server successfully suspended
- **NX_DHCPV6_INVALID_SERVER_DUID** (0XE98) Unknown or unsupported DUID type
- **NX_INVALID_PARAMETERS** (0x4D) Invalid non pointer input
- NX_PTR_ERROR (0x16) Invalid pointer input

### Allowed From

Application code

### Example

```
/* Set the DHCPv6 ServerDUID as Link layer plus time, over Ethernet hardware. */
duid_time = SECONDS_SINCE_JAN_1_2000_MOD_32 + rand();

status = nx_dhcpv6_set_server_duid(&dhcp_server_0,1, 0x6,
         physical_address_msw,physical_address_lsw,duid_time);

/* If status is NX_SUCCESS the ServerDUID is successfully set. */
```

## nx_dhcpv6_server_option_request_handler_set

Set the option request handler for DHCPv6 Server instance

### Prototype

```
UINT nx_dhcpv6_server_option_request_handler_set(
     NX_DHCPV6_SERVER *dhcpv6_server_ptr, 
     VOID (*dhcpv6_option_request_handler_extended)(
          struct NX_DHCPV6_SERVER_STRUCT *dhcpv6_server_ptr, 
          UINT option_request,
          UCHAR *buffer_ptr, 
          UINT *index,
          UINT available_payload));
```

### Description

This service sets the DHCPv6 Server extended option request handler.

### Input Parameters

- *dhcpv6_server_ptr*: Pointer to DHCPv6 Server
- *dhcpv6_option_request_handler_extended*: Pointer to extended options request handler

### Return Values

- **NX_SUCCESS** (0x00) Server successfully resumed
- NX_PTR_ERROR (0x16) Invalid pointer input

### Allowed From

Application Code

### Example

```
/* Set the option request handler for DHCPv6 Server. */
status = nx_dhcpv6_server_option_request_handler_set(&dhcp_server_0,
         dhcpv6_option_request_handler_extended);

/* If status is NX_SUCCESS the extended handler successfully set. */
```

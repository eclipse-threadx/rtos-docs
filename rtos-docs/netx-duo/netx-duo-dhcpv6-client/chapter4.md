---
title: Chapter 4 - NetX Duo DHCPv6 Client services
description: "This chapter contains a description of all NetX Duo DHCPv6 Client services (listed below) in alphabetic order."
---

# Chapter 4 - NetX Duo DHCPv6 Client services

This chapter contains a description of all NetX Duo DHCPv6 Client services (listed below) in alphabetic order.

In the "Return Values" section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## nx_dhcpv6_client_create

Create a DHCPv6 client instance

### Prototype

```C
UINT  nx_dhcpv6_client_create(
    NX_DHCPV6 *dhcpv6_ptr, 
    NX_IP *ip_ptr,
    CHAR *name_ptr, 
    NX_PACKET_POOL *packet_pool_ptr, 
    VOID *stack_ptr,
    ULONG stack_size,
    VOID (*dhcpv6_state_change_notify)(
        struct NX_DHCPV6_STRUCT *dhcpv6_ptr, 
        UINT old_state,
        UINT new_state), 
    VOID (*dhcpv6_server_error_handler)(
        struct NX_DHCPV6_STRUCT *dhcpv6_ptr, 
        UINT op_code, UINT status_code, 
        UINT message_type));
```

### Description

This service creates a DHCPv6 client instance including callback functions.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 control block  

- *ip_ptr*: Pointer to Client IP instance  

- *name_ptr*: Pointer to name for DHCPv6 instance

- *packet_pool_ptr*: Pointer to Client packet pool

- *stack_ptr*: Pointer to Client stack memory

- *stack_size*: Size of Client stack memory

- *dhcpv6_state_change_notify*: Pointer to callback function invoked when the Client initiates a new DHCPv6 request to the server

- *dhcpv6_server_error_handler*: Pointer to callback function invoked when the Client receives an error status from the server

### Return Values

- **NX_SUCCESS** (0x00) Successful Client create

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_DHCPV6_PARAM_ERROR (0xE93) Invalid non pointer input

### Allowed From

Threads

### Example

```C
/* Create a DHCPv6 client instance without specifying link local or preferred
   global IP address.  */
status =  nx_dhcpv6_client_create(&dhcp_0, &ip_0, "DHCPv6 Client", &pool_0,
                                  NULL, NULL, pointer, 2048,        
                                  dhcpv6_state_change_notify, 
                                  dhcpv6_server_error_handler);

/* If status is NX_SUCCESS a DHCPv6 client instance was successfully
   created.  */
```

### See Also

- nx_dhcpv6_client_delete

## nx_dhcpv6_client_delete

Delete a DHCPv6 Client instance

### Prototype

```C
UINT nx_dhcpv6_client_delete(NX_DHCPV6 *dhcpv6_ptr);
```

### Description

This service deletes a previously created DHCPv6 client instance.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 client instance

### Return Values

- **NX_SUCCESS** (0x00) Successful DHCPv6 deletion

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_DHCPV6_PARAM_ERROR (0xE93) Invalid non pointer input

### Allowed From

Threads

### Example

```C
/* Delete a DHCPv6 client instance.  */
status =  nx_dhcpv6_client_delete(&my_dhcp);

/* If status is NX_SUCCESS the DHCPv6 client instance was successfully
   deleted.  */
```

### See Also

- nx_dhcpv6_client_create

## nx_dhcpv6_client_set_interface

Sets Client's Network Interface for DHCPv6

### Prototype

```C
UINT    nx_dhcpv6_client_set_interface(
    NX_DHCPV6 *dhcpv6_ptr, 
    UINT *interface_index);
```

### Description

This service sets the Client's network interface for communicating with the DHCPv6 Server(s) to the specified input interface index.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

- *interface_index*: Index indicating network interface

### Return Values

- **NX_SUCCESS** (0x00) Interface successfully set

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_INVALID_INTERFACE (0x4C) Invalid interface index input

### Allowed From

Threads

### Example

```C
/* Set the client interface for DHCPv6 communication with the Server to 
   the secondary interface (1). */

UINT index = 1;
status = nx_dhcpv6_client_set_interface(&dhcp_0, index);

/* If status is NX_SUCCESS, the Client successfully set 
   the DHCPv6 network interface.  */
```

### See Also

- nx_dhcpv6_client _create
- nx_dhcpv6_start

## nx_dhcpv6_client_set_destination_address

Sets the destination address where DHCPv6 message should be sent to

### Prototype

```C
UINT nx_dhcpv6_client_set_destination_address(
    NX_DHCPV6 *dhcpv6_ptr,
    NXD_ADDRESS *destination_address);
```

### Description

This service sets the destination address where DHCPv6 message should be sent to. By default is ALL_DHCP_Relay_Agents_and_Servers(FF02::1:2).

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

- *destination_address*: Destination address

### Return Values

- **NX_SUCCESS** (0x00) Interface successfully set

- NX_PTR_ERROR (0x07) Invalid pointer input

- NX_DHCPV6_PARAM_ERROR (0xE93) Parament error

### Allowed From

Threads

### Example

```C
/* Set the destination address where DHCPv6 message should be sent to. */

NXD_ADDRESS dest_address; /* Set the destination address.  */

status = nx_dhcpv6_client_set_destination_address(&dhcp_0, &dest_address);

/* If status is NX_SUCCESS, the Client successfully set the destination address. */
```

### See Also

- nx_dhcpv6_client _create
- nx_dhcpv6_start

## nx_dhcpv6_create_client_duid

Create Client DUID object

### Prototype

```C
UINT nx_dhcpv6_create_client_duid(
    NX_DHCPV6 *dhcpv6_ptr,
    UINT duid_type, UINT hardware_type,
    ULONG time);
```

### Description

This service creates the Client DUID with the input parameters. If the time input is not supplied and the duid type indicates link layer with time, this function will supply a time which includes a randomizing factor for uniqueness. Vendor assigned (enterprise) duid types are not supported.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

- *duid_type*: Type of DUID (hardware, enterprise etc)

- *hardware_type*: Network hardware e.g. IEEE 802

- *time*: Value used in creating unique identifier

### Return Values

- **NX_SUCCESS** (0x00) Successful Client DUID created

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_DHCPV6_PARAM_ERROR (0xE93) Invalid non pointer input

- NX_DHCPV6_UNSUPPORTED_DUID_TYPE (0xE98) DUID type unknown or not supported 

- NX_DHCPV6_UNSUPPORTED_DUID_HW_TYPE (0xE99) DUID hardware type unknown or not supported

### Allowed From

Threads

### Example

```C
/* Create the Client DUID from the supplied input.
   The time field is left NULL so the DHCPv6 client will provide one.  */
status = nx_dhcpv6_create_client_duid(&dhcp_0, NX_DHCPV6_DUID_TYPE_LINK_TIME,
                                      NX_DHCPV6_HW_TYPE_IEEE_802, 0)

/* If status is NX_SUCCESS the client DUID was successfully created.  */
```

### See Also

- nx_dhcpv6_create_client_ia
- nx_dhcpv6_create_client_iana
- nx_dhcpv6_create_server_duid

## nx_dhcpv6_create_client_ia

Add an Identity Association to the Client

### Prototype

```C
UINT nx_dhcpv6_create_client_ia(
    NX_DHCPV6 *dhcpv6_ptr,
    NXD_ADDRESS *ipv6_address,
    ULONG preferred_lifetime,
    ULONG valid_lifetime);
```

### Description

This service is identical to the *nx_dhcpv6_add_client_ia* service. It adds a Client Identity Association by filling in the Client record with the supplied parameters. To request the maximum preferred and valid lifetimes, set these parameters to infinity. To add more than one IA to a DHCPv6 Client, set the NX_DHCPV6_MAX_IA_ADDRESS to a value higher than the default value of 1.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

- *ipv6_address*: Pointer to NetX Duo IP address block

- *preferred_lifetime*: Length of time before IP address is deprecated

- *valid_lifetime*: Length of time before IP address is expired

### Return Values

- **NX_SUCCESS** (0x00) Successful Client IA added

- **NX_DHCPV6_IA_ADDRESS_ALREADY_EXIST** (0xEAF) Duplicate IA address 

- **NX_DHCPV6_REACHED_MAX_IA_ADDRESS** (0xEAE) IA exceeds the max IAs Client can store

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_DHCPV6_INVALID_IA_ADDRESS (0xEA4) Invalid (e.g. null) IA address in IA

- NX_DHCPV6_PARAM_ERROR (0xE93) Invalid non pointer input


### Allowed From

Threads

### Example

```C
/* Add an Client IA using the supplied input.   */
status = nx_dhcpv6_create_client_ia(&dhcp_0, &ipv6_address, 
NX_DHCPV6_PREFERRED_LIFETIME, NX_DHCPV6_VALID_LIFETIME);

/* If status is NX_SUCCESS the client IA was successfully added.  */
```

### See Also

- nx_dhcpv6_add_client_duid
- nx_dhcpv6_create_server_duid
- nx_dhcpv6_create_client_iana

## nx_dhcpv6_create_client_iana

Create an Identity Association (Non Temporary) for the Client

### Prototype

```C
UINT nx_dhcpv6_create_client_iana(
    NX_DHCPV6 *dhcpv6_ptr, 
    UINT IA_ident,
    ULONG T1, ULONG T2);
```

### Description

This service creates a Client Non Temporary Identity Association (IANA) from the supplied parameters. To set the T1 and T2 times to maximum (infinity) in the DHCPv6 Client requests, set these parameters to NX_DHCPV6_INFINITE_LEASE. 

> **Note:** A Client has only one IANA.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

- *IA_ident*: Identity Association unique identifier

- *T1*: When the Client must start the IPv6 address renewal

- *T2*: When the Client must start theIPv6 address rebinding

### Return Values

- **NX_SUCCESS** (0x00) Successfully created the IANA

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_DHCPV6_PARAM_ERROR (0xE93) Invalid non pointer input

### Allowed From

Threads

### Example

```C
/* Create the Client IANA from the supplied input.  */
status = nx_dhcpv6_create_client_iana(&dhcp_0, DHCPV6_IA_ID, DHCPV6_T1,   
                                      DHCPV6_T2);

/* If status is NX_SUCCESS the client IANA was successfully created.  */
```

### See Also

- nx_dhcpv6_create_client_duid
- nx_dhcpv6_create_server_duid
- nx_dhcpv6_add_client_ia

## nx_dhcpv6_add_client_ia 

Add an Identity Association to the Client

### Prototype

```C
UINT nx_dhcpv6_add_client_ia(
    NX_DHCPV6 *dhcpv6_ptr, 
    NXD_ADDRESS *ipv6_address, 
    ULONG preferred_lifetime, 
    ULONG valid_lifetime);
```

### Description

This service adds a Client Identity Association by filling in the Client record with the supplied parameters. To request the maximum preferred and valid lifetimes, set these parameters to infinity. To add more than one IA to a DHCPv6 Client, set the NX_DHCPV6_MAX_IA_ADDRESS to a value higher than the default value of 1.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

- *ipv6_address*: Pointer to NetX Duo IP address block

- *preferred_lifetime*: Length of time before IP address is deprecated

- *valid_lifetime*: Length of time before IP address is expired 

### Return Values

- **NX_SUCCESS** (0x00) Successful Client IA added

- **NX_DHCPV6_IA_ADDRESS_ALREADY_EXIST** (0xEAF) Duplicate IA address 

- **NX_DHCPV6_REACHED_MAX_IA_ADDRESS** (0xEAE) IA exceeds the max IAs Client can store

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_DHCPV6_INVALID_IA_ADDRESS (0xEA4) Invalid (e.g. null) IA address in IA

- NX_DHCPV6_PARAM_ERROR (0xE93) Invalid non pointer input

 
### Allowed From

Threads

### Example

```C
/* Add an Client IA using the supplied input.   */
status = nx_dhcpv6_add_client_ia(&dhcp_0, &ipv6_address, 
								 NX_DHCPV6_PREFERRED_LIFETIME,
                                 NX_DHCPV6_VALID_LIFETIME);

/* If status is NX_SUCCESS the client IA was successfully added.  */
```

### See Also

- nx_dhcpv6_create_client_duid
- nx_dhcpv6_create_server_duid
- nx_dhcpv6_create_client_iana

## nx_dhcpv6_get_client_duid_time_id

Retrieves time ID from Client DUID

### Prototype

```C
UINT nx_dhcpv6_get_client_duid_time_id(
    NX_DHCPV6 *dhcpv6_ptr,
    ULONG *time_id);
```

### Description

This service retrieves the time ID field from the Client DUID. If the application must first call *nx_dhcpv6_create_client_duid*, to fill in the Client DUID in the DHCPv6 Client instance or it will have a null value for this field. The intent is for the application to save this data and present the same Client DUID to the server, including the time field, across reboots.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

- *time_id*: Pointer to Client DUID time field

### Return Values

- **NX_SUCCESS** (0x00) IP lease data successfully retrieved

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Retrieve the time ID from the Client DUID.  */
status = nx_dhcpv6_get_client_duid_time_id(&dhcp_0, &time_ID);

/* If status is NX_SUCCESS the time ID was retrieved. */
```

### See Also

- nx_dhcpv6_get_IP_address
- nx_dhcpv6_get_time_lease_data
- nx_dhcpv6_get_other_option_data
- nx_dhcpv6_get_time_accrued

## nx_dhcpv6_get_IP_address

Retrieves Client's global IPv6 address

### Prototype

```C
UINT nx_dhcpv6_get_IP_address(
    NX_DHCPV6 *dhcpv6_ptr, 
    NXD_ADDRESS *ip_address);
```

### Description

This service retrieves the Client's global IPv6 address. If the Client does not have a valid address, an error status is returned. If a Client has more than one global IPv6 address, the primary IPv6 address is returned.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

- *ip_address*: Pointer to IPv6 address

### Return Values

- **NX_SUCCESS** (0x00) IPv6 address successfully assigned

- **NX_DHCPV6_IA_ADDRESS_NOT_VALID** (0xEAD) IPv6 address is not valid

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
UINT address_status;
UINT address_index;

/* Retrieve the client's assigned IP address.  */
status = nx_dhcpv6_get_IP_address(&dhcp_0, &ipv6_address);

/* If status is NX_SUCCESS the client IP address was assigned.
   Now register it with NetX Duo on the primary interface (index zero). 
   The address index is returned in the address_index field*/
status = nxd_ipv6_address_set(&ip_0, 0, &ipv6_address, 64, &address_index);
```

### See Also

- nx_dhcpv6_get_lease_time_data
- nx_dhcpv6_get_client_duid_time_id
- nx_dhcpv6_get_other_option_data
- nx_dhcpv6_get_time_accrued

## nx_dhcpv6_get_lease_time_data

Retrieves Client's IA address lease time data

### Prototype

```C
UINT nx_dhcpv6_get_lease_time_data(
    NX_DHCPV6 *dhcpv6_ptr,
    ULONG *T1, 
    ULONG *T2,
    ULONG *preferred_lifetime, 
    ULONG *valid_lifetime);
```

### Description

This service retrieves the Client's global IA address time data. If the Client IA address status is invalid, time data is set to zero and a successful completion status is returned. If a Client has more than one global IPv6 address, the primary IA address data is returned.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

- *T1*: Pointer to IA address renew time

- *T2*: Pointer to IA address rebind time

- *preferred_lifetime*: Pointer to time when IA address is deprecated

- *valid_lifetime*: Pointer to time when IA address is expired

### Return Values

- **NX_SUCCESS** (0x00) IA lease data successfully retrieved

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Retrieve the client's assigned IA lease data.  */
status = nx_dhcpv6_get_lease_time_data(&dhcp_0, &T1, &T2, &preferred_lifetime, 
                                       &valid_lifetime);

/* If status is NX_SUCCESS the client IA address lease data was retrieved.  */
```

### See Also

- nx_dhcpv6_get_IP_address
- nx_dhcpv6_get_client_duid_time_id
- nx_dhcpv6_get_other_option_data
- nx_dhcpv6_get_time_accrued
- nx_dhcpv6_get_iana_lease_time

## nx_dhcpv6_get_iana lease_time

Retrieve the Client's IANA lease time data

### Prototype

```C
UINT nx_dhcpv6_get_iana_lease_time(
    NX_DHCPV6 *dhcpv6_ptr,
    ULONG *T1, 
    ULONG *T2);
```

### Description

This service retrieves the Client's global IA-NA lease time data (T1 and T2). If none of the Client IA-NA addresses have a valid address status, time data is set to zero and a successful completion status is returned. If a Client has more than one global IPv6 address, the primary IA address data is returned.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

- *T1*: Pointer to time for starting lease renewal

- *T2*: Pointer to time for starting lease rebinding

### Return Values

- **NX_SUCCESS** (0x00) IANA lease data successfully retrieved

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Retrieve the client's assigned IANA lease data.  */
status = nx_dhcpv6_get_iana_lease_time(&dhcp_0, &T1, &T2);

/* If status is NX_SUCCESS the client IA address lease data was retrieved.  */
```

### See Also

- nx_dhcpv6_get_IP_address
- nx_dhcpv6_get_client_duid_time_id
- nx_dhcpv6_get_other_option_data
- nx_dhcpv6_get_time_accrued
- nx_dhcpv6_get_lease_time_data

## nx_dhcpv6_get_valid_ip_address_count

Retrieve a count of Client's valid IA addresses

### Prototype

```C
UINT nx_dhcpv6_get_valid_ip_address_count(
    NX_DHCPV6 *dhcpv6_ptr, 
    UINT *address_count);
```

### Description

This service retrieves the count of the Client's valid IPv6 addresses. A valid IPv6 address is bound (assigned) to the Client and registered with the IP instance.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

- *address_count*: Pointer to address count to return

### Return Values

- **NX_SUCCESS** (0x00) IANA lease data successfully retrieved

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
UINT address_count; 

/* Retrieve the count of valid IA-NA addresses.  */
status = nx_dhcpv6_get_valid_ip_address_count(&dhcp_0, &address_count);

/* If status is NX_SUCCESS the client IA address count was retrieved.  */
```

## nx_dhcpv6_get_valid_ip_address_lease_time

Retrieve the Client IA data by address index

### Prototype

```C
UINT nx_dhcpv6_get_valid_ip_address_lease_time(
    NX_DHCPV6 *dhcpv6_ptr, 
    UINT address_index,
    NXD_ADDRESS *ip_address,
    ULONG *preferred_lifetime,
    ULONG *valid_lifetime);
```

### Description

This service retrieves the Client's IA address and lease data by address index. If an invalid index is supplied, or the IPv6 address at that index is not valid, the service returns an NX_DHCPV6_IA_ADDRESS_NOT_VALID error status.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

- *address_index*: Index into DHCPv6 IA table

- *ip_address*: Pointer to IPv6 address to retrieve

- *preferred_lifetime*: Pointer to time when IA address is deprecated

- *valid_lifetime*: Pointer to time when IA address is expired

### Return Values

- **NX_SUCCESS** (0x00) IANA data successfully retrieved

- **NX_DHCPV6_IA_ADDRESS_NOT_VALID** (0xEAD) An invalid index or no valid IPv6 address at the supplied index 

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
UINT address_index = 1; 
NXD_ADDRESS *ip_address;

/* Retrieve the IPv6 address, and valid and preferred lifetime for the IA record
   Saved at index 1 in the DHCPv6 table.  */
status = nx_dhcpv6_get_valid_ip_address_lease_time(&dhcp_0, address_index, 
                                                   &ip_address, 
                                                   &preferred_lifetime,  
                                                   &valid_lifetime);

/* If status is NX_SUCCESS the client IA address and lease data were retrieved.  */
```

### See Also

- nx_dhcpv6_get_IP_address
- nx_dhcpv6_get_iana_lease_time
- nx_dhcpv6_get_lease_time_data

## nx_dhcpv6_get_DNS_server_address

Retrieves DNS Server address 

### Prototype

```C
UINT nx_dhcpv6_get_DNS_server_address(
    NX_DHCPV6 *dhcpv6_ptr,
    UINT index,
    NXD_ADDRESS *server_address);
```

### Description

This service retrieves the DNS server IPv6 address data at the specified index in the Client list. If the list does not contain a server address at the index, an error is returned. The index may not exceed the size of the DNS Server list is specified by the user configurable option NX_DHCPV6_NUM_DNS_SERVERS.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

- *index*: Index into the DNS Server list

- *server_address*: Pointer to Server address buffer

### Return Values

- **NX_SUCCESS** (0x00) Address successfully retrieved

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Retrieve the DNS server at the specified index in the list. */

UINT index = 0;
NXD_ADDRESS server_address;


        status = nx_dhcpv6_get_DNS_server_address(&dhcp_0, index, &server_address);

/* If status == NX_SUCCESS, the DNS server IP address successfully retrieved. */
```

### See Also

- nx_dhcpv6_get_IP_address
- nx_dhcpv6_get_lease_time_data
- nx_dhcpv6_get_time_accrued

## nx_dhcpv6_get_other_option_data

Retrieves DHCPv6 option data 

### Prototype

```C
UINT  nx_dhcpv6_get_other_option_data(
    NX_DHCPV6 *dhcpv6_ptr, 
    UINT option_code,
    UCHAR *buffer);
```

### Description

This service retrieves DHCPv6 option data from a DHCPv6 message for the specified option code.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

- *option*: code Option code for which data to retrieve

- *buffer*: Pointer to buffer to copy data to 

### Return Values

- **NX_SUCCESS** (0x00) Option data successfully retrieved

- **NX_DHCPV6_UNKNOWN_OPTION** (0xEAB) Unknown/unsupported option code

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_DHCPV6_PARAM_ERROR (0xE93) Invalid non pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Retrieve the option data specified by the input option code. */
status = nx_dhcpv6_get_other_option_data(&dhcp_0, option_code, buffer);

/* If status is NX_SUCCESS the option data was retrieved. */
```

### See Also

- nx_dhcpv6_get_IP_address
- nx_dhcpv6_get_lease_time_data
- nx_dhcpv6_get_time_accrued

## nx_dhcpv6_get_time_accrued

Retrieves time accrued on Client's IP address lease

### Prototype

```C
UINT nx_dhcpv6_get_time_accrued(
    NX_DHCPV6 *dhcpv6_ptr,
    ULONG *time_accrued);
```

### Description

This service retrieves the time accrued on the Client's IPv6 address lease. The function checks all the IPv6 addresses assigned to the Client for the first valid address. If no valid addresses are found, a zero value for time accrued is returned.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

- *time_accrued*: Pointer to time accrued in IP lease

### Return Values

- **NX_SUCCESS** (0x00) Accrued time successfully retrieved

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Retrieve time accrued time on the Client address lease. */
status = nx_dhcpv6_get_time_accrued(&dhcp_0, &time_accrued);

/* If status is NX_SUCCESS the time accrued on the client IP address 
   lease was retrieved. */
```

### See Also

- nx_dhcpv6_get_IP_address
- nx_dhcpv6_get_other_option_data
- nx_dhcpv6_get_lease_time_data
- nx_dhcpv6_set_time_accrued

## nx_dhcpv6_get_time_server_address

Retrieves Time Server address 

### Prototype

```C
UINT  nx_dhcpv6_get_time_server_address(
    NX_DHCPV6 *dhcpv6_ptr,
    UINT index, 
    NXD_ADDRESS *server_address);
```

### Description

This service retrieves the Time server IPv6 address data at the specified index in the Client list. If the list does not contain a server address at the index, an error is returned. The index may not exceed the size of the Time Server list is specified by the user configurable option NX_DHCPV6_NUM_TIME_SERVERS.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

- *index*: Index into the Time Server list

- *server_address*: Pointer to Server address buffer

### Return Values

- **NX_SUCCESS** (0x00) Address successfully retrieved

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Retrieve the Time server at the specified index in the list. */

UINT index = 0;
NXD_ADDRESS server_address;


      status = nx_dhcpv6_get_time_server_address(&dhcp_0, index, &server_address);

/* If status == NX_SUCCESS, the Time server IP address successfully retrieved. */
```

### See Also

- nx_dhcpv6_get_IP_address
- nx_dhcpv6_get_lease_time_data
- nx_dhcpv6_get_time_accrued
- nx_dhcpv6_get_DNS_server_address

## nx_dhcpv6_reinitialize

Remove the Client IP address from the IP table

### Prototype

```C
UINT nx_dhcpv6_reinitialize(NX_DHCPV6 *dhcpv6_ptr);
```

### Description

This service reinitializes the Client for restarting the DHCPv6 state machine and re-running the DHCPv6 protocol. This is not necessary if the Client has not previously started the DHPCv6 state machine or been assigned any IPv6 addresses. The addresses saved to the DHCPv6 Client as well as registered with the IP instance are both cleared.

> **Note:** The application must still start the DHCPv6 Client using the *nx_dhcpv6_start service* and begin the request for IPv6 address assignment by calling *nx_dhcpv6_request_solicit*.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

### Return Values

- **NX_SUCCESS** (0x00) Address successfully removed

- **NX_DHCPV6_ALREADY_STARTED** (0xE91) DHCPv6 Client is already running

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Clear the assigned IP address(es) from the Client and the IP instance */
status = nx_dhcpv6_reinitialize(&dhcp_0);

/* If status is NX_SUCCESS the Client IP address was successfully removed. */
```

### See Also

- nx_dhcpv6_stop
- nx_dhcpv6_start

## nx_dhcpv6_request_confirm

Process the Client's CONFIRM state

### Prototype

```C
UINT nx_dhcpv6_request_confirm(NX_DHCPV6 *dhcpv6_ptr);
```

### Description

This service sends a CONFIRM request. If a reply is received from the Server, the DHCPv6 Client updates its lease parameters with the received data.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

### Return Values

- **NX_SUCCESS** (0x00) CONFIRM message successfully sent and processed

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Send a CONFIRM message to the Server. */
status = nx_dhcpv6_request_confirm(&dhcp_0);

/* If status is NX_SUCCESS the Client successfully sent the CONFIRM message. */
```

### See Also

- nx_dhcpv6_request_inform_request
- nx_dhcpv6_request_release
- nx_dhcpv6_request_solicit


## nx_dhcpv6_request_inform_request

Process the Client's INFORM REQUEST state

### Prototype

```C
UINT nx_dhcpv6_request_inform_request(NX_DHCPV6 *dhcpv6_ptr);
```

### Description

This service sends an INFORM REQUEST message. If a reply is received, When one is received, the reply is processed to determine it is valid and the server granted the request. The Client instance is then updated with the server information as needed.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

### Return Values

- **NX_SUCCESS** (0x00) INFORM REQUEST message successfully created and processed

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Send an INFORM REQUEST message to the server. */
status = nx_dhcpv6_request_inform_request(&dhcp_0);

/* If status is NX_SUCCESS the Client successfully sent the INFORM REQUEST 
   message and processed the reply. */
```

### See Also

- nx_dhcpv6_request_confirm

## nx_dhcpv6_request_option_DNS_server

Add DNS Server to DHCPv6 Option request

### Prototype

```C
UINT nx_dhcpv6_request_option_DNS_server(NX_DHCPV6 *dhcpv6_ptr);
```

### Description

This service adds the option for requesting DNS server information to the DHCPv6 option request. If the Server reply includes DNS server data, the Client will store the DNS server if it has room to do so. The number of DNS servers the Client can store is determined by the configurable option NX_DHCPV6_NUM_DNS_SERVERS whose default value is 2.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

### Return Values

- **NX_SUCCESS** (0x00) DNS server option is included

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Set the DNS server option in Client requests. */
nx_dhcpv6_request_option_DNS_server(&dhcp_0, NX_TRUE);
```

### See Also

- nx_dhcpv6_request_option_domain_name
- nx_dhcpv6_request_option_time_server
- nx_dhcpv6_request_option_timezone

## nx_dhcpv6_request_option_FQDN

Add Fully Qualified Domain Name option to Option request list

### Prototype

```C
UINT nx_dhcpv6_request_option_FQDN(
    NX_DHCPV6 *dhcpv6_ptr,
    UCHAR *domain_name, 
    UINT op);
```

### Description

This service adds the option for adding the Client Fully Qualified Domain Name to the DHCPv6 option request. There are three options for the FQDN option:

- NX_DHCPV6_CLIENT_DESIRES_UPDATE_AAAA_RR 0 Update the FQDN-to-IPv6 address mapping for FQDN and address(es) used by the Client.

- NX_DHCPV6_CLIENT_DESIRES_SERVER_DO_DNS_UPDATE 1 Update the FQDN-to-IPv6 address mapping for FQDN and address(es) used by the Client to the server.

- NX_DHCPV6_CLIENT_DESIRES_NO_SERVER_DNS_UPDATE 2 Request the server perform no DNS updates on the Client's behalf.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

- *domain_name*: String holding the domain name

- *op*: Type of FQDN option to apply (see list above)

### Return Values

- **NX_SUCCESS** (0x00) FQDN option is included

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Set the FQDN option in Client DHCPv6 request. */
nx_dhcpv6_request_option_FQDN(&dhcp_0, "DHCPv6_Client", 
							  NX_DHCPV6_CLIENT_DESIRES_NO_SERVER_DNS_UPDATE);
```

### See Also

- nx_dhcpv6_request_option_domain_name
- nx_dhcpv6_request_option_time_server
- nx_dhcpv6_request_option_timezone

## nx_dhcpv6_request_option_domain_name

Add domain name option to DHCPv6 option request

### Prototype

```C
UINT nx_dhcpv6_request_option_domain_name(NX_DHCPV6 *dhcpv6_ptr);
```

### Description

This service adds the domain name option to the option request in Client request messages. If the Server reply includes domain name data, the Client will store the domain name information if the size of the domain name is within the buffer size for holding the domain name. This buffer size is a configurable option (NX_DHCPV6_DOMAIN_NAME_BUFFER_SIZE) with a default value of 30 bytes.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

### Return Values

- **NX_SUCCESS** (0x00) Domain name option set

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Set the domain name option in Client requests. */
nx_dhcpv6_request_option_domain_name(&dhcp_0, NX_TRUE);
```

### See Also

- nx_dhcpv6_request_option_DNS_server
- nx_dhcpv6_request_option_time_server
- nx_dhcpv6_request_option_timezone

## nx_dhcpv6_request_option_time_server

Set time server data as optional request

### Prototype

```C
UINT nx_dhcpv6_request_option_time_server(NX_DHCPV6 *dhcpv6_ptr);
```

### Description

This service adds the option for time server information to the option request of Client request messages. If the Server reply includes tim server data, the Client will store the time server if it has room to do so. The number of time servers the Client can store is determined by the configurable option

NX_DHCPV6_NUM_TIME _SERVERS whose default value is 1.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

### Return Values

- **NX_SUCCESS** (0x00) Time server option added

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Set the time server option in Client request messages. */
nx_dhcpv6_request_option_time_server(&dhcp_0, NX_TRUE);
```

### See Also

- nx_dhcpv6_request_option_DNS_server
- nx_dhcpv6_request_option_domain_name
- nx_dhcpv6_request_option_timezone

## nx_dhcpv6_request_option_timezone

Set time zone data as optional request

### Prototype

```C
UINT nx_dhcpv6_request_option_timezone(NX_DHCPV6 *dhcpv6_ptr);
```

### Description

This service adds the option for requesting time zone information to the Client option request. If the Server reply includes time zone data, the Client will store the time zone information if the size of the time zone is within the buffer size for holding the time zone. This buffer size is a configurable option (NX_DHCPV6_ TIME_ZONE _BUFFER_SIZE) with a default value of 10 bytes.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

### Return Values

- **NX_SUCCESS** (0x00) Time zone option added

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Set time zone option in Client request messages. */
nx_dhcpv6_request_option_timezone(&dhcp_0, NX_TRUE);
```

### See Also

- nx_dhcpv6_request_option_DNS_server
- nx_dhcpv6_request_option_domain_name
- nx_dhcpv6_request_option_time_server

## nx_dhcpv6_request_release

Send a DHCPv6 RELEASE message

### Prototype

```C
UINT nx_dhcpv6_request_release(NX_DHCPV6 *dhcpv6_ptr);
```

### Description

This service sends a RELEASE message on the Client network. If the message is successfully sent, a successful status is returned. A successful completion does not mean the Client received a response or has been granted an IPv6 address yet. The DHCPv6 Client thread task waits for a reply from a DHCPv6 Server. If one is received, it checks the reply is valid and stores the data to the Client record.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

### Return Values

- **NX_SUCCESS** (0x00) RELEASE message successfully sent

- **NX_DHCPV6_NOT_STARTED** (0xE92) DHCPv6 Client task not started

- **NX_DHCPV6_IA_ADDRESS_NOT_VALID** (0xEAD) Address not bound to Client

- **NX_INVALID_INTERFACE** (0x4C) Not found in IP address table

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Send an RELEASE message to the Server. */
status = nx_dhcpv6_request_release(&dhcp_0);

/* If status is NX_SUCCESS the Client successfully sent the RELEASE message. */
```

### See Also

- nx_dhcpv6_request_confirm
- nx_dhcpv6_request_inform_request
- nx_dhcpv6_request_solicit

## nx_dhcpv6_request_solicit

Send a SOLICIT message

### Prototype

```C
UINT nx_dhcpv6_request_solicit(NX_DHCPV6 *dhcpv6_ptr);
```

### Description

This service sends a SOLICIT message out on the network. If the message is successfully sent, a successful status is returned. A successful completion does not mean the Client received a response or has been granted an IPv6 address yet. The DHCPv6 Client thread task waits for a reply (an ADVERTISE message) from a DHCPv6 Server. If one is received, it checks the reply is valid, stores the data to the Client record and promotes the Client to the REQUEST state.

> **Note:** If the Rapid Commit option is set, the DHCPv6 Client will go directly to the Bound state if it receives a valid Server ADVERTISE message. See the service description for *nx_dhcpv6_request_solicit_rapid* for more details.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

### Return Values

- **NX_SUCCESS** (0x00) SOLICIT message successfully sent

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Send an SOLICIT message to the server. */
status = nx_dhcpv6_request_solicit(&dhcp_0);

/* If status is NX_SUCCESS the Client successfully sent the SOLICIT message. */
```

## nx_dhcpv6_request_solicit_rapid

Send a SOLICIT message with the Rapid Commit option

### Prototype

```C
UINT nx_dhcpv6_request_solicit_rapid(NX_DHCPV6 *dhcpv6_ptr);
```

### Description

This service sends a SOLICIT message out on the network with the Rapid Commit option set. If the message is successfully sent, a successful status is returned. A successful completion does not mean the Client received a response or has been granted an IPv6 address yet. The DHCPv6 Client thread task waits for a reply (an ADVERTISE message) from a DHCPv6 Server. If one is received, it checks the reply is valid, stores the data to the Client record and promotes the Client to the BOUND state.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

### Return Values

- **NX_SUCCESS** (0x00) SOLICIT message successfully sent

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Send an SOLICIT message to the server. */
status = nx_dhcpv6_request_solicit_rapid(&dhcp_0);

/* If status is NX_SUCCESS the Client successfully sent the SOLICIT message. */
```

### See Also

- nx_dhcpv6_request_solicit
- nx_dhcpv6_request_confirm
- nx_dhcpv6_request_inform_request
- nx_dhcpv6_request_release

## nx_dhcpv6_resume

Resume DHCPv6 Client task 

### Prototype

```C
UINT nx_dhcpv6_resume(NX_DHCPV6 *dhcpv6_ptr);
```

### Description

This service resumes the DHCPv6 Client thread task. The current DHCPv6 Client state will be processed (e.g. Bound, Solicit)

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

### Return Values

- **NX_SUCCESS** (0x00) Client successfully resumed

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Resume the DHCPv6 Client task. */
status = nx_dhcpv6_resume(&dhcp_0);

/* If status is NX_SUCCESS the Client thread task successfully resumed. */
```

### See Also

- nx_dhcpv6_start
- nx_dhcpv6_stop
- nx_dhcpv6_suspend

## nx_dhcpv6_set_ time_accrued

Sets time accrued on Client's IP address lease

### Prototype

```C
UINT nx_dhcpv6_set_time_accrued(
    NX_DHCPV6 *dhcpv6_ptr,
    ULONG time_accrued);
```

### Description

This service sets the time accrued on the Client's global IP address since it was assigned by the server. This should only be used if a Client is currently bound to an assigned IPv6 address.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

- *time_accrued*: Time accrued in IP lease

### Return Values

- **NX_SUCCESS** (0x00) Time accrued successfully set

- NX_PTR_ERROR (0x16) Invalid pointer input

### Allowed From

Threads

### Example

```C
/* Set time accrued since client's assigned IP address was assigned. */
status = nx_dhcpv6_set_time_accrued(&dhcp_0, time_accrued);

/* If status is NX_SUCCESS the time accrued on the client IP address lease was 
   successfully set. */
```

### See Also

- nx_dhcpv6_get_IP_address
- nx_dhcpv6_get_other_option_data
- nx_dhcpv6_get_lease_time_data
- nx_dhcpv6_get_time_accrued

## nx_dhcpv6_start

Start the DHCPv6 Client task 

### Prototype

```C
UINT nx_dhcpv6_start(NX_DHCPV6 *dhcpv6_ptr);
```

### Description

This service starts the DHCPv6 Client task and prepares the Client for running the DHCPv6 protocol. It verifies the Client instance has sufficient information (such as a Client DUID), creates and binds the UDP socket for sending and receiving DHCPv6 messages and activates timers for keeping track of session time and when the current IPv6 lease expires.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

### Return Values

- **NX_SUCCESS** (0x00) Client successfully started

- **NX_DHCPV6_MISSING_REQUIRED_OPTIONS** (0xEA9) Client missing required options

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Start the DHCPv6 Client task. */
status = nx_dhcpv6_start(&dhcp_0);

/* If status is NX_SUCCESS the Client successfully started. */
```

### See Also

- nx_dhcpv6_resume
- nx_dhcpv6_suspend
- nx_dhcpv6_stop
- nx_dhcpv6_reinitialize

## nx_dhcpv6_stop

Stop the DHCPv6 Client task 

### Prototype

```C
UINT nx_dhcpv6_stop(NX_DHCPV6 *dhcpv6_ptr);
```

### Description

This service stops the DHCPv6 Client task, and clears retransmission counts, maximum retransmission intervals, deactivates the session and lease expiration timers, and unbinds the DHCPv6 Client socket port. To restart the Client, one must first stop and optionally reinitialize the Client before starting another session with any DHCPv6 server. See the Small Example section for more details.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

### Return Values

- **NX_SUCCESS** (0x00) Client successfully stopped

- **NX_DHCPV6_NOT_STARTED** (0xE92) Client thread not started

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread


### Allowed From

Threads

### Example

```C
/* Stop the DHCPv6 Client task. */
status = nx_dhcpv6_start(&dhcp_0);

/* If status is NX_SUCCESS the Client successfully stopped. */
```

### See Also

- nx_dhcpv6_resume
- nx_dhcpv6_suspend
- nx_dhcpv6_reinitialize
- nx_dhcpv6_start

## nx_dhcpv6_suspend

Suspend the DHCPv6 Client task 

### Prototype

```C
UINT nx_dhcpv6_suspend(NX_DHCPV6 *dhcpv6_ptr);
```

### Description

This service suspends the DHCPv6 client task and any request it was in the middle of processing. Timers are deactivated and the Client state is set to non-running.

### Input Parameters

- *dhcpv6_ptr*: Pointer to DHCPv6 Client instance

### Return Values

- **NX_SUCCESS** (0x00) Client successfully suspended

- **NX_DHCPV6_NOT_STARTED** (0XE92) Client not running so cannot be suspended

- NX_PTR_ERROR (0x16) Invalid pointer input

- NX_CALLER_ERROR (0x11) Must be called from thread

### Allowed From

Threads

### Example

```C
/* Suspend the DHCPv6 Client task. */
status = nx_dhcpv6_suspend(&dhcp_0);

/* If status is NX_SUCCESS the Client successfully suspended. */
```

### See Also

- nx_dhcpv6_resume
- nx_dhcpv6_start
- nx_dhcpv6_reinitialize
- nx_dhcpv6_stop

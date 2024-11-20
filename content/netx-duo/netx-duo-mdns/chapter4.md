---
title: Chapter 4 - Description of mDNS services
description: This chapter contains a description of all NetX Duo mDNS services
---


This chapter contains a description of all NetX Duo mDNS services (listed below).

> **Note:** In the **Return Values** section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## nx_mdns_create

Create an mDNS instance

### Prototype

```C
UINT nx_mdns_create(
    NX_MDNS *mdns_ptr,
    NX_IP *ip_ptr,
    NX_PACKET_POOL *packet_pool,
    UINT priority, 
    VOID *stack_ptr,
    UINT stack_size,
    UCHAR *host_name,
    VOID *local_service_cache,
    UINT local_service_cache_size,
    VOID *peer_service_cache,
    UINT peer_service_cache_size,
    VOID (*probing_notify)(
        NX_MDNS *mdns_ptr,
        UCHAR *name,
        UINT probing_state));
```

### Description

This service creates an mDNS instance on the specific IP instance and associated resources. A thread is also created to handle incoming mDNS messages, to respond to queries, and to periodically transmit query messages.

### Input Parameters

- *mdns_ptr*: Pointer to mDNS control block.
- *ip_ptr*: Pointer to the associated IP instance.
- *packet_pool*: Pointer to a valid packet pool.
- *priority*: Priority of the mDNS thread.
- *stack_ptr*: Pointer to the stack area for the mDNS thread
- *stack_size*: Size of the stack area.
- *host_name*: Host name assigned to this node.
- *local_service_cache*: Storage space for local registered services.
- *local_service_cache_size*: Size of the local service cache.
- *peer_service_cache*: Storage space for service information received
- *peer_service_cache_size*: Size of the peer service cache
- *probing_notify*: Optional callback function invoked at the end of the probing operation. It notifies the application whether or not the host name (when enabling mDNS on a local interface), or the service name (after registering a service) is unique.

### Return Values

- **NX_SUCCESS** (0x00) Successfully created mDNS instance.

### Allowed From

Threads

### Example

```C
UCHAR stack_ptr[2048];
UCHAR local_cache_ptr[2048];
UCHAR peer_cache_ptr[8192];

/* Create a mDNS instance. */
status = nx_mdns_create(&my_mdns, &ip_0, &pool_0,
    3, stack_ptr, 2048,
    "NETX-MDNS-HOST",
    local_cache_ptr, 2048,
    peer_cache_ptr, 8192,
    probing_notify);

/* If status is NX_SUCCESS, mDNS instance was created. */
```

## nx_mdns_delete

Delete an mDNS instance

### Prototype

```C
UINT nx_mdns_delete(NX_MDNS *mdns_ptr);
```

### Description

This service deletes the mDNS instance and frees its resources.

### Input Parameters

- *mdns_ptr*: Pointer to the mDNS control block.

### Return Values

- **NX_SUCCESS** (0x00) Successfully deleted the mDNS instance.

### Allowed From

Threads

### Example

```C
/* Delete a previously created mDNS instance. */

status = nx_mdns_delete(&my_mdns);

/* If status is NX_SUCCESS, the mDNS instance has been deleted. */
```

## nx_mdns_enable

Start the mDNS service

### Prototype

```C
UINT nx_mdns_enable(
    NX_MDNS *mdns_ptr,
    UINT interface_index);
```

### Description

This API enables mDNS service on specific physical interface. Once the service is enabled, the mDNS module first probes all its unique service names on the network before responding to queries received on the interface.

### Input Parameters

- *mdns_ptr*: Pointer to the mDNS instance control block.
- *interface_index*: Index to the interface where the service is to be enabled

### Return Values

- **NX_SUCCESS** (0x00) Successfully enabled the service.

### Allowed From

Threads

### Example

```C
/* Enable mDNS service on specific interface. */

status = nx_mdns_enable(&my_mdns, 0);

/* If status is NX_SUCCESS, mDNS service was enabled. */
```

## nx_mdns_disable

Disable the mDNS service

### Prototype

```C
UINT nx_mdns_disable(
    NX_MDNS *mdns_ptr,
    UINT interface_index);
```

### Description

This API disables mDNS service on the specific physical interface. Once the service is disabled, the mDNS sends "goodbye" messages for every local service to the network that is attached to the interface, so the neighboring nodes are notified.

### Input Parameters

- *mdns_ptr*: Pointer to mDNS control block.
- *interface_index*: Index to the interface where the service is to be disabled

### Return Values

- **NX_SUCCESS** (0x00) Successfully disabled the service.

### Allowed From

Threads

### Example

```C
/* Disable mDNS service on specific interface. */

status = nx_mdns_disable(&my_mdns, 0);

/* If status is NX_SUCCESS, mDNS service was disabled. */
```

## nx_mdns_cache_notify_set

Installs the mDNS cache full notify function

### Prototype

```c
UINT nx_mdns_cache_notify_set(
    NX_MDNS *mdns_ptr,
    VOID (*cache_full_notify_cb)(
        NX_MDNS *mdns_ptr,
        UINT state,
        UINT cache_type));
```

### Description

This service installs a user-supplied callback function, which is invoked when either the local service cache or peer service cache becomes full. When the service cache is full, no more mDNS Resource Record can be added. Note that the service cache may become full as a result of internal fragmentation, when services with various string lengths are added and removed. On receiving a cache full notification on peer service cache, the application may use the service "*nx_mdns_service_cache_clear"* to erase all entries in the peer service cache.

### Input Parameters

- *mdns_ptr*: Pointer to the mDNS control block.

### Return Values

- **NX_SUCCESS** (0x00) Successfully installed the mDNS cache notify callback function.

### Allowed From

Threads

### Example

```C
/* Set mDNS cache notify callback. */

status = nx_mdns_cache_notify_set(&my_mdns, cache_full_notify_cb);

/* If status is NX_SUCCESS, mDNS cache notify callback was set. */
```

## nx_mdns_cache_notify_clear

Clear the mDNS service cache full notify function

### Prototype

```C
UINT nx_mdns_cache_notify_clear(NX_MDNS *mdns_ptr);
```

### Description

This service clears a user-supplied service cache notify callback function.

### Input Parameters

- *mdns_ptr*: Pointer to the mDNS control block.

### Return Values

- **NX_SUCCESS** (0x00) Successfully cleared the mDNS service cache notify callback function.

### Allowed From

Threads

### Example

```C
/* Clear mDNS cache notify callback. */

status = nx_mdns_cache_notify_clear(&my_mdns);

/* If status is NX_SUCCESS, mDNS cache notify callback clear. */
```

## nx_mdns_domain_name_set

Sets the domain name

### Prototype

```C
UINT nx_mdns_domain_name_set(
    NX_MDNS *mdns_ptr,
    CHAR *domain_name);
```

### Description

This service sets up the default local domain name. When the mDNS instance is created, the default local domain name is set to ".local". This API allows an application to overwrite the default local domain name.

### Input Parameters

- *mdns_ptr*: Pointer to mDNS control block.
- *domain_name*: The domain name to be used.

### Return Values

- **NX_SUCCESS** (0x00) Successfully configured local domain.

### Allowed From

Threads

### Example

```C
/* Set the domain name. */

status = nx_mdns_domain_name_set(&my_mdns, "home");

/* If status is NX_SUCCESS, the "home" domain name was set. */
```

## nx_mdns_service_announcement_timing_set

Sets the timing parameters for service announcement messages

### Prototype

```C
UINT nx_mdns_service_announcement_timing_set(
    NX_MDNS *mdns_ptr,
    UINT t,
    UINT p,
    UINT k,
    UINT retrans_interval,
    UINT period_interval,
    UINT max_time);
```

### Description

This service reconfigures the timing parameters employed by mDNS when sending the service announcements. Publish period starts from *t* ticks and can be expanded telescopically with 2 to the power of *k* factor. The number of repetitions per advertisement is *p*, the interval between each repeated advertisement is *interval* ticks, and the number of announcement period is max_time. By default, the initial period is set to 1 second, with k = 1 (the period doubles each time), *p = 1* (no repetition), retrans_interval = 0(no time interval), period_interval = 0xFFFFFFFF(max period interval) and max_time = 3(number of advertisement).

### Input Parameters

- *mdns_ptr*: Pointer to mDNS control block.
- *t*: Number of ticks for the initial period. Default is 100 ticks for 1 second.
- *p*: Number of repetitions. Default value is 1.
- *k*: Telescopic factor. Default value is 1.
- *retrans_interval*: Number of ticks to wait before sending out repeated announcement messages. Default value is 0.
- *period_interval*: Number of ticks between two announcement period. Default value is 0xFFFFFFFF.
- *max_time*: Number of announcement period to use for the advertisement. After the *max_time* announcement periods, no more announcement messages are sent. Default value is 3.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sets the timing values.

### Allowed From

Threads

### Example

```C
/* Set the service announcement timing. */

status = nx_mdns_service_announcement_timing_set(&my_mdns, 100,
    1, 1, 0, 0xFFFFFFFF, 3);

/* If status is NX_SUCCESS, the service announcement timing was set. */
```

## nx_mdns_service_add

Add a local service

### Prototype

```C
UINT nx_mdns_service_add(
    NX_MDNS *mdns_ptr,
    CHAR *instance,
    CHAR *service,
    CHAR *subtype,
    UINT ttl,
    USHORT priority,
    USHORT weight,
    USHORT port,
    UCHAR *text,
    UCHAR is_unique,
    UINT interface_index);
```

### Description

This API registers a service offered by the application. If the flag *is_unique* is set, mDNS probes the service name to make sure it is unique on the local network before starting to announce the service on the network. *Instance* is the instance portion of the service name. The *service* is the service portion of the service name. For example "_http._tcp" is a service. To describe a service with subtype, caller must use the *subtype* parameter. For example, if the desired service is "_printer._sub._http._tcp", the service field is "_http._tcp:, and the subtype field is "_printer".

### Input Parameters

- *mdns_ptr*: Pointer to mDNS control block.
- *instance*: Pointer to the instance name of the service.
- *service*: Pointer to the mDNS service type, excluding subtype information.
- *subtype*: Pointer to the subtype portion of the mDNS service, if applicable.
- *priority*: Service priority
- *weight*: Service weight
- *port*: TCP or UDP port number the service uses
- *text*: Additional text information
- *is_unique*: Boolean flag indicating whether the service is shared or unique. For services registered as unique, mDNS must probe the service on the network before starting offering it.
- *interface_index*: Physical interface the service is offered through. For a service that is offered through any of the attached services, the value *NX_MDNS_ALL_INTERFACES* is used.

### Return Values

- **NX_SUCCESS** (0x00) Successfully registered the service.

### Allowed From

Threads

### Example

```C
/* Add local service. */

status = nx_mdns_service_add(&my_mdns, "NETX-SERVICE",
    "_http._tcp", NX_NULL,
    NX_NULL, 0, 0, 0, 80, NX_TRUE, 0);

/* If status is NX_SUCCESS, The service (NetX-SERVICE._http._tcp.local) was added. */
```

## nx_mdns_service_delete

Remove a previous registered service

### Prototype

```C
UINT nx_mdns_service_delete(
    NX_MDNS *mdns_ptr,
    CHAR *instance,
    CHAR *service,
    CHAR *subtype);
```

### Description

This API deletes a previous registered service. As the service is deleted, "goodbye" messages are sent to the local network so the neighboring nodes are notified.

### Input Parameters

- *mdns_ptr*: Pointer to mDNS control block.
- *instance*: Pointer to the instance name of the service.
- *service*: Pointer to the mDNS service type, excluding subtype information.
- *subtype*: Pointer to the subtype portion of the mDNS service, if applicable.

### Return Values

- **NX_SUCCESS** (0x00) Successfully deleted the service.

### Allowed From

Threads

### Example

```C
/* Delete local service. */

status = nx_mdns_service_delete(&my_mdns, "NETX-SERVICE", "_http._tcp", NX_NULL);

/* If status is NX_SUCCESS, The service (NetX-SERVICE._http._tcp.local) was deleted. */
```

## nx_mdns_service_one_shot_query

Initiate one-shot service discovery

### Prototype

```C
UINT nx_mdns_service_one_shot_query(
    NX_MDNS *mdns_ptr,
    UCHAR *instance,
    UCHAR *service,
    UCHAR *subtype,
    NX_MDNS_SERVICE *service_ptr,
    ULONG wait_option);
```

### Description

This service performs a one-shot mDNS query. If the specified service is found in the peer service cache, the first instance is returned. If no services are found in the local peer service cache, the mDNS module issues a query command and wait for response. The service is blocked till either the first answer is received or the query times out.

### Input Parameters

- *mdns_ptr*: Pointer to mDNS control block.
- *instance*: Pointer to the instance name of the service, if applicable.
- *service*: Pointer to the mDNS service type, excluding subtype information. the application must specify the service type.
- *subtype*: Pointer to the subtype portion of the mDNS service, if applicable.
- *service_ptr*: User provided pointer to NX_MDNS_SERVICE structure that stores the query results.
- *wait_option*: The amount of time, in ticks, to wait for a response.

### Return Values

- **NX_SUCCESS** (0x00) Successfully obtained service information.

### Allowed From

Threads

### Example

```C
/* Start service one shot query. */

status = nx_mdns_service_one_shot_query(&my_mdns, "NETX-SERVICE", "_http._tcp",
    NX_NULL, service_ptr, 500);

/* If status is NX_SUCCESS, The query with
    name: NetX-SERVICE._http._tcp.local,
     type: ANY (SRV and TXT) was sent.
    And the answer was stored in service_ptr if success. */
```

## nx_mdns_service_continuous_query

Initiate continuous service discovery

### Prototype

```C
UINT nx_mdns_service_continuous_query(
    NX_MDNS *mdns_ptr,
    CHAR *instance,
    CHAR *service,
    CHAR *subtype);
```

### Description

This service starts a continuous query. Note that the service returns immediately. After issuing a continuous query, the application may retrieve service record by using the API ***nx_mdns_service_lookup***. To stop the continuous query, the application may use the API ***nx_mdns_service_query_stop***

### Input Parameters

- *mdns_ptr*: Pointer to mDNS control block.
- *instance*: Pointer to the instance name of the service, if applicable.
- *service*: Pointer to the mDNS service type, excluding subtype information, if applicable.
- *subtype*: Pointer to the subtype portion of the mDNS service, if applicable.

### Return Values

- **NX_SUCCESS** (0x00) Successfully started continues query.

### Allowed From

Threads

### Example

```C
/* Start service continuous query. */

status = nx_mdns_service_continuous_query(&my_mdns,
    "NETX-SERVICE", "_http._tcp", NX_NULL);

/* If status is NX_SUCCESS, The continuous query with
    name: NetX-SERVICE._http._tcp.local,
    type: ANY (SRV and TXT) was added.
    And the query will be periodically sent. */
```

## nx_mdns_service_query_stop

Cease a previously issued continuous service discovery

### Prototype

```C
UINT nx_mdns_service_query_stop(
    NX_MDNS *mdns_ptr,
    CHAR *instance,
    CHAR *service,
    CHAR *subtype);
```

### Description

This API terminates a previous issued continuous service discovery.

### Input Parameters

- *mdns_ptr*: Pointer to mDNS control block.
- *instance*: Pointer to the instance name of the service.
- *service*: Pointer to the mDNS service type, subtype information.
- *subtype*: Pointer to the subtype portion of the mDNS service, if applicable.

### Return Values

- **NX_SUCCESS** (0x00) Successfully stopped continues query.

### Allowed From

Threads

### Example

```C
/* Stop service continuous query. */

status = nx_mdns_service_query_stop(&my_mdns, "NETX-SERVICE", "_http._tcp", NX_NULL);

/* If status is NX_SUCCESS, The continuous query with
    name: NetX-SERVICE._http._tcp.local,
    type: ANY (SRV and TXT) was stopped. */
```

## nx_mdns_service_lookup

Retrieves the service from the local peer service cache

### Prototype

```C
UINT nx_mdns_service_lookup(
    NXD_MDNS *mdns_ptr,
    CHAR *instance,
    CHAR *service,
    CHAR *subtype,
    UINT instance_index,
    NXD_MDNS_SERVICE *service_ptr);
```

### Description

This service looks up services matching the instance name (if provided) and the type of service in the local peer service cache. Application shall start the service lookup with *instance_index* set to zero for the first service in the cache that matches the description. Application shall keep using this service with increasing *instance_index* value for additional services found in the cache, till the service returns *NX_NO_MORE_ENTRIES*, which indicates the end of the cache.

### Input Parameters

- *mdns_ptr*: Pointer to mDNS control block.
- *instance*: Pointer to the instance name of the service, if applicable.
- *service*: Pointer to the mDNS service type, excluding subtype information, if applicable.
- *subtype*: Pointer to the subtype portion of the mDNS service, if applicable.
- *Instance_index*: Index number to the instance to be returned.
- *service_ptr*: User provided pointer to NX_MDNS_SERVICE structure that stores the lookup results.

### Return Values

- **NX_SUCCESS** (0x00) Successfully retrieved the service
- **NX_NO_MORE_ENTRIES** (0x17) No service entry is found at the specified index number. This error code indicates the end of the search.

### Allowed From

Threads

### Example

```C
/* Lookup the service on specific index. */

status = nx_mdns_service_lookup(&my_mdns, "NETX-SERVICE", "_http._tcp", NX_NULL,
    0, service_ptr);

/* If status is NX_SUCCESS, The service with
    name: NetX-SERVICE._http._tcp.local, was retrieved. */
```

## nx_mdns_service_ignore_set

Configures a service ignore set

### Prototype

```C
UINT nx_mdns_service_ignore_set(
    NX_MDNS *mdns_ptr,
    ULONG service_mask);
```

### Description

This API configures a mask to ignore services specified by the *service_mask* bitmask. User may optionally use the service_mask to select service types it does not wish to be cached. A list of services is defined in the table *nx_mdns_service_types* in *nxd_mdns.c.* The corresponding mask of the first service type in nx_mdns_service_types[] is 0x00000001, the mask of the second service type is 0x00000002, and so on.

### Input Parameters

- *mdns_ptr*: Pointer to mDNS control block.
- *service_mask*: User-defined service types to ignore. The mask is a 32-bit ULONG type. Each bit represents an entry in the user-defined *nx_mdns_service_types* array. If a bit is set, the corresponding service type specified in the *nx_mdns_service_type* array will not store in the peer service cache.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sets the service ignore mask.

### Allowed From

Threads

### Example

```C
/* Set the service mask to ignore the specified service. */

status = nx_mdns_service_ignore_set(&my_mdns, 0x00000003);

/* If status is NX_SUCCESS, The service with
    type "_device-info" and "_http" will be ignored. */
```

## nx_mdns_service_notify_set

Configures a service change notify callback function

### Prototype

```C
UINT nx_mdns_service_notify_set(
    NX_MDNS *mdns_ptr,
    ULONG service_mask,
    VOID (*service_change_notify)(
        NX_MDNS *mdns_ptr,
        NX_MDNS_SERVICE *service_ptr,
        UINT state));
```

### Description

This API configures a service change notify callback function. This callback function is invoked when a service offered by other nodes on the network is added, changed or is no longer available. User may optionally use the service_mask to select service types it wishes to monitor. A list of services being monitored are hard-coded in the table *nx_mdns_service_types* in *nxd_mdns.c.*

The corresponding mask of the first service type in nx_mdns_service_types[] is 0x00000001, the mask of the second service type is 0x00000002, and so on.

### Input Parameters

- *mdns_ptr*: Pointer to mDNS control block.
- *service_mask*: User-defined service types to monitor. The mask is a 32-bit ULONG type. Each bit represents an entry in the *nx_mdns_service_types* array.
- *service_change_notify*: The callback function to be invoked when the specified service is changed. The detailed service information is returned in the memory pointed to by *service_ptr.* Note that the content in the memory is invalid after returning from the notify callback function.

### Return Values

- **NX_SUCCESS** (0x00) Successfully installed the callback function.

### Allowed From

Threads

### Example

```C
/* Set the service mask to notify the specified service. */

status = nx_mdns_service_notify_set(&my_mdns, 0x00000002, service_change_notify);

/* If status is NX_SUCCESS, the callback will be called
    if received the service with type "_http". */
```

## nx_mdns_service_notify_clear

Clear the service change notify callback function

### Prototype

```C
UINT nx_mdns_service_notify_clear(NX_MDNS *mdns_ptr);
```

### Description

This API clears the service change notify callback function and the .

### Input Parameters

- *mdns_ptr*: Pointer to mDNS control block..

### Return Values

- **NX_SUCCESS** (0x00) Successfully cleared the callback function.

### Allowed From

Threads

### Example

```C
/* Clear the service notify. */

status = nx_mdns_service_notify_clear(&my_mdns);

/* If status is NX_SUCCESS, the service notify function clear. */
```

## nx_mdns_host_address_get

Get the host address

### Prototype

```C
UINT nx_mdns_host_address_get(
    NX_MDNS *mdns_ptr,
    UCHAR *host_name,
    ULONG *ipv4_address,
    ULONG *ipv6_address,
    ULONG wait_option);
```

### Description

This service performs a mDNS query on host IPv4 and IPv6 addresses. If the address of specified host name is found in peer service cache, the address is returned. If no address is found in the peer service cache, the mDNS module issues A and AAAA type queries and wait for response. This API blocks until either an answer is received or the query times out.

### Input Parameters

- *mdns_ptr*: Pointer to mDNS control block.
- *host_name*: Pointer to host name.
- *ipv4_address*: Pointer to a 4-byte aligned address for IPv4 address storage space. User shall allocate 4 bytes of space for the IPv4 - address. NX_NULL address can be passed into this API if application does not need to retrieve IPv4 address.
- *ipv6_address*: Pointer to the 4-byte aligned address for IPv6 address storage space. User shall allocate 16 bytes of space for the - IPv6 address. NX_NULL address can be passed into this API if application does not need to retrieve IPv6 address.
- *wait_option*: The amount of time, in ticks, to wait for a response.

### Return Values

- **NX_SUCCESS** (0x00) Successfully obtained host address.

### Allowed From

Threads

### Example

```C
ULONG ipv4_address;
ULONG ipv6_address[4];

/* Get the IP address of specified host. */
status = nx_mdns_host_address_get(&my_mdns, "MDNS-Host", &ipv4_address, ipv6_address, 500);

/* If status is NX_SUCCESS, the IP address of specified host was retrieved. */
```

## nx_mdns_local_cache_clear

Erase all local services

### Prototype

```C
UINT nx_mdns_local_cache_clear(NX_MDNS *mdns_ptr);
```

### Description

This service clears all entries in the local service cache after send the Goodbye message.

### Input Parameters

- *mdns_ptr*: Pointer to mDNS control block.

### Return Values

- **NX_SUCCESS** (0x00) Successfully erased all entries in the cache.

### Allowed From

Threads

### Example

```C
/* Clear the local cache, delete all local service. */

status = nx_mdns_local_cache_clear(&my_mdns);

/* If status is NX_SUCCESS, all services and resource records of local cache were deleted. */
```

## nx_mdns_peer_cache_clear

Erase all the discovered services

### Prototype

```C
UINT nx_mdns_peer_cache_clear(NX_MDNS *mdns_ptr);
```

### Description

This service clears all entries in the peer service cache.

### Input Parameters

- *mdns_ptr*: Pointer to mDNS control block.

### Return Values

- **NX_SUCCESS** (0x00) Successfully erased all entries in the cache.

### Allowed From

Threads

### Example

```C
/* Clear the peer cache, delete all peer service. */

status = nx_mdns_peer_cache_clear(&my_mdns);

/* If status is NX_SUCCESS, all services and resource records of peer cache were deleted. */
```

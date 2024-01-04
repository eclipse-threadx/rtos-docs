---
title: Chapter 4 - Description of NAT services
description: This chapter contains a description of all NetX Duo NAT API services in alphabetical order.
---

# Chapter 4 - Description of NAT services

This chapter contains a description of all NetX Duo NAT API services (listed below) in alphabetical order.

> **Note:** In the **Return Values** section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## nx_nat_create

Create a NAT Server

### Prototype

```C
UINT nx_nat_create(
    NX_NAT_DEVICE *nat_ptr,
    NX_IP *ip_ptr,
    UINT global_interface_index,
    VOID *dynamic_cache_memory,
    UINT dynamic_cache_size);
```

### Description

This service creates an instance of the NAT server.

### Input Parameters

- *nat_ptr*: Pointer to NAT instance to create
- *ip_ptr Pointer*: to IP instance
- *global_interface_index*: Index to the global network interface
- *dynamic_cache_memory*: Pointer memory area to NAT table
- *dynamic_cache_size*: Size of memory area for NAT table

### Return Values

- **NX_SUCCESS** (0x00) NAT server successfully created
- NX_PTR_ERROR (0x07) Invalid input pointer parameter
- NX_NAT_PARAM_ERROR (0xD01) Invalid non pointer input
- NX_NAT_CACHE_ERROR (0xD02) Invalid cache pointer input

### Allowed From

Application code

### Example

```C
#define NX_NAT_ENTRY_CACHE_SIZE 20480

static UCHAR nat_cache[NX_NAT_ENTRY_CACHE_SIZE];
UINT global_interface_index = 0;

/* Create a NAT Server and cache with a global interface index. */
status = nx_nat_create(nat_ptr, ip_ptr, global_interface_index,
    nat_cache, NX_NAT_ENTRY_CACHE_SIZE);

/* If status = NX_SUCCESS, the NAT instance was successfully
    created. */
```

## nx_nat_delete

Delete a NAT Server

### Prototype

```C
UINT nx_nat_delete(NX_NAT_DEVICE *nat_ptr);
```

### Description

This service deletes a previously created NAT Server.

### Input Parameters

- *nat_ptr*: Pointer to NAT instance to delete

### Return Values

- **NX_SUCCESS** (0x00) NAT successfully deleted
- NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From

Application code

### Example

```C
/* Delete the NAT instance. */
status = nx_nat_delete (nat_ptr);

/* If the NAT instance was successfully deleted, status = NX_SUCCESS. */
```

## nx_nat_enable

Enable the IP instance for NAT

### Prototype

```C
UINT nx_nat_enable(NX_NAT_DEVICE *nat_ptr);
```

### Description

This service enables the IP instance for NAT (e.g. forward received packets to the NAT server).

### Input Parameters

- *nat_ptr*: Pointer to NAT instance

### Return Values

- **NX_SUCCESS** (0x00) NAT successfully enabled
- NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From

Application code

### Example

```C
/* Enable the NAT server. */
status = nx_nat_enable (nat_ptr);

/* If status = NX_SUCCESS, the IP instance was successfully enabled for NAT. */
```

## nx_nat_disable

Disable the IP instance for NAT

### Prototype

```C
UINT nx_nat_disable(NX_NAT_DEVICE *nat_ptr);
```

### Description

This service disables NAT on the IP instance.

### Input Parameters

- *nat_ptr*: Pointer to NAT instance

### Return Values

- **NX_SUCCESS** (0x00) NAT successfully disabled
- NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From

Application code

### Example

```C
/* Disable the NAT server. */
status = nx_nat_disable (nat_ptr);

/* If status = NX_SUCCESS the NAT operation successfully disable. */
```

## nx_nat_cache_notify_set

Set a cache full notify callback function

### Prototype

```C
UINT nx_nat_cache_notify_set(
    NX_NAT_DEVICE *nat_ptr,
    VOID (*cache_full_notify_cb)(NX_NAT_DEVICE *nat_ptr)));
```

### Description

This service registers the cache full callback with the input function pointer cache_full_notify_cb which points to a user defined cache full notify function.

### Input Parameters

- *nat_ptr*: Pointer to NAT instance
- *cache_full_notify_cb*: Pointer to cache full notify function

### Return Values

- **NX_SUCCESS** (0x00) Cache full notify function successfully set
- NX_PTR_ERROR (0x07) Invalid input pointer parameter
- NX_NAT_PARAM_ERROR (0xD01) Invalid non pointer input

### Allowed From

Application code

### Example

```C
/* Set the cache full notify callback function to the NAT instance. */
status = nx_nat_cache_notify_set(nat_ptr, cache_full_notify_cb);

/* If status = NX_SUCCESS the callback function was successfully set. */
```

## nx_nat_inbound_entry_create

Create an inbound entry in the NAT translation table

### Prototype

```C
UINT nx_nat_inbound_entry_create(
    NX_NAT_DEVICE *nat_ptr,
    NX_NAT_TRANSLATION_ENTRY *entry_ptr
    ULONG local_ip_address,
    USHORT external_port,
    USHORT local_port,
    UCHAR protocol);
```

### Description

This service creates an inbound entry as static (permanent entry, never expires) and adds it to the NAT translation table. These entries are usually created for local host servers where a connection is initiated from a host on the outside network. The NAT server checks that the external port is not already in use in the translation table or bound by a previously existing NetX Duo socket of the same protocol.

### Input Parameters

- *nat_ptr*: Pointer to NAT instance
- *entry_ptr*: Pointer to translation entry
- *local_ip_address*: Local host IP address
- *external_port*: Destination port on the external network
- *local_port*: Source (local host) port
- *protocol*: Packet protocol (e.g TCP)

### Return Values

- **NX_SUCCESS** (0x00) Entry successfully created
- **NX_NAT_PORT_UNAVAILABLE** (0xD0D) Invalid external port
- NX_PTR_ERROR (0x07) Invalid input pointer parameter
- NX_NAT_PARAM_ERROR (0xD01) Invalid non pointer input

### Allowed From

Application code

### Example

```C
/* Create an entry for an inbound TCP packet. */
status = nx_nat_inbound_entry_create(nat_ptr, entry_ptr,
    IP_ADDRESS(192,168,2,2), 5001, 5001,
    NX_PROTOCOL_TCP);

/* If status = NX_SUCCESS the entry was successfully created. */
```

## nx_nat_inbound_entry_delete

Delete an inbound entry in the NAT translation table

### Prototype

```C
UINT nx_nat_inbound_entry_delete(
    NX_NAT_DEVICE *nat_ptr,
    NX_NAT_TRANSLATION_ENTRY *delete_entry_ptr)
```

### Description

This service deletes the specified inbound entry from the translation table.

### Input Parameters

- *nat_ptr*: Pointer to NAT instance
- *delete_entry_ptr*: Pointer to the NAT translation entry

### Return Values

- **NX_SUCCESS** (0x00) Entry successfully deleted
- **NX_NAT_ENTRY_NOT_FOUND** (0xD04) Entry does not found
- NX_PTR_ERROR (0x07) Invalid input pointer parameter
- NX_NAT_ENTRY_TYPE_ERROR (0xD0C) Invalid translation type

### Allowed From

Application code

### Example

```C
/* Delete the specified static entry from the translation table. */
status = nx_nat_inbound_entry_delete(nat_ptr, delete_entry_ptr);

/* If status = NX_SUCCESS the entry was successfully deleted. */
```

---
title: Chapter 3 - Description of NetX Duo AutoIP services
description: This chapter contains a description of all NetX Duo AutoIP services (listed below) in alphabetic order.
---

# Chapter 3 - Description of NetX Duo AutoIP services

This chapter contains a description of all NetX Duo AutoIP services (listed below) in alphabetic order.

In the "Return Values" section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## nx_auto_ip_create

Create AutoIP instance

### Prototype

```c
UINT nx_auto_ip_create(
    NX_AUTO_IP *auto_ip_ptr,
    CHAR *name,
    NX_IP *ip_ptr,
    VOID *stack_ptr,
    ULONG stack_size,
    UINT priority);
```

### Description

This service creates an AutoIP instance on the specified IP instance.

### Input Parameters

- *auto_ip_ptr*: Pointer to AutoIP control block.
- *name*: Name of AutoIP instance.
- *ip_ptr*: Pointer to IP instance.
- *stack_ptr*: Pointer to AutoIP thread stack area.
- *stack_size*: Size of the AutoIP thread stack area.
- *priority*: Priority of the AutoIP thread.

> **Note:** If DHCP is used, the DHCP thread must have a higher priority than the IP instance thread and the AutoIP thread.

### Return Values

- **NX_SUCCESS**: (0x00) Successful AutoIP create.
- **NX_AUTO_IP_ERROR**: (0xA00) AutoIP create error.
- NX_PTR_ERROR: (0x16) Invalid AutoIP, ip_ptr, or stack pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Initialization, Threads

### Example

```c
/* Create the AutoIP instance "auto_ip_0" on "ip_0". */
status = nx_auto_ip_create(&auto_ip_0, "AutoIP 0", &ip_0, pointer, 4096, 1);

/* If status is NX_SUCCESS an AutoIP instance was successfully created. */
```

### See Also

nx_auto_ip_delete, nx_auto_ip_set_interface, nx_auto_ip_get_address, nx_auto_ip_start, nx_auto_ip_stop

## nx_auto_ip_delete

Delete AutoIP instance

### Prototype

```c
UINT nx_auto_ip_delete(NX_AUTO_IP *auto_ip_ptr);
```

### Description

This service deletes a previously created AutoIP instance on the specified IP instance.

### Input Parameters

- *auto_ip_ptr*: Pointer to AutoIP control block.

### Return Values

- **NX_SUCCESS**: (0x00) Successful AutoIP delete.
- **NX_AUTO_IP_ERROR**: (0xA00) AutoIP delete error.
- NX_PTR_ERROR: (0x16) Invalid AutoIP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c
/* Delete the AutoIP instance "auto_ip_0." */
status = nx_auto_ip_delete(&auto_ip_0);

/* If status is NX_SUCCESS an AutoIP instance was successfully deleted. */
```

### See Also

nx_auto_ip_create, nx_auto_ip_set_interface, nx_auto_ip_get_address, nx_auto_ip_start, nx_auto_ip_stop

## nx_auto_ip_get_address

Get current AutoIP address

### Prototype

```c
UINT nx_auto_ip_get_address(
    NX_AUTO_IP *auto_ip_ptr,
    ULONG *local_ip_address);
```

### Description

This service retrieves the currently setup AutoIP address. If there isn't one, an IP address of 0.0.0.0 is returned.

### Input Parameters

- *auto_ip_ptr*: Pointer to AutoIP control block.
- *local_ip_address*: Destination for return IP address.

### Return Values

- **NX_SUCCESS**: (0x00) Successful AutoIP address get.
- **NX_AUTO_IP_NO_LOCAL**: (0xA01) No valid AutoIP address.
- NX_PTR_ERROR: (0x16) Invalid AutoIP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Initialization, Timers, Threads, ISRs

### Example

```c
ULONG local_address;

/* Get the AutoIP address resolved by the instance "auto_ip_0." */
status = nx_auto_ip_get_address(&auto_ip_0, &local_address);

/* If status is NX_SUCCESS the local IP address is in "local_address." */
```

### See Also

nx_auto_ip_create, nx_auto_ip_set_interface, nx_auto_ip_delete, nx_auto_ip_start, nx_auto_ip_stop

## nx_auto_ip_set_interface

Set network interface for AutoIP

### Prototype

```c
UINT nx_auto_ip_set_interface(
    NX_AUTO_IP *auto_ip_ptr,
    UINT interface_index);
```

### Description

This service sets the index for the network interface AutoIP will probe for a network IP address. The default is zero (the primary network interface). Only applicable for multihomed devices.

### Input Parameters

- *auto_ip_ptr*: Pointer to AutoIP control block.
- *interface_index*: Interface to probe IP address for

### Return Values

- **NX_SUCCESS**: (0x00) Successful AutoIP interface set
- **NX_AUTO_IP_BAD_INTERFACE_INDEX**: (0xA02) Invalid network interface NX_PTR_ERROR (0x16) Invalid AutoIP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Initialization, Timers, Threads, ISRs

### Example

```c
ULONG interface_index;

/* Set the network interface on which AutoIP probes for host address. */
status = nx_auto_ip_set_interface(&auto_ip_0, interface_index);

/* If status is NX_SUCCESS the network interface is valid and set in the AutoIP control block *auto_ip_0*. */
```

### See Also

nx_auto_ip_create, nx_auto_ip_get_address, nx_auto_ip_delete, nx_auto_ip_start, nx_auto_ip_stop

## nx_auto_ip_start

Start AutoIP processing

### Prototype

```c
UINT nx_auto_ip_start(
    NX_AUTO_IP *auto_ip_ptr,
    ULONG starting_local_address);
```

### Description

This service starts the AutoIP protocol on a previously created AutoIP instance.

### Input Parameters

- *auto_ip_ptr*: Pointer to AutoIP control block.
- *starting_local_address*: Optional AutoIP starting address. A value of IP_ADDRESS(0,0,0,0) specifies that a random AutoIP address should be derived. Otherwise, if a valid AutoIP address is specified, NetX Duo AutoIP attempts to assign that address.

### Return Values

- **NX_SUCCESS**: (0x00) Successful AutoIP start.
- **NX_AUTO_IP_ERROR**: (0xA00) AutoIP start error.
- NX_PTR_ERROR: (0x16) Invalid AutoIP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Initialization, Threads

### Example

```c
/* Start the AutoIP instance "auto_ip_0." */
status = nx_auto_ip_start(&auto_ip_0, IP_ADDRESS(0,0,0,0));

/* If status is NX_SUCCESS an AutoIP instance was successfully started. */
```

### See Also

nx_auto_ip_create, nx_auto_ip_set_interface, nx_auto_ip_delete, nx_auto_ip_get_address, nx_auto_ip_stop

## nx_auto_ip_stop

Stop AutoIP processing

### Prototype

```c
UINT nx_auto_ip_stop(NX_AUTO_IP *auto_ip_ptr);
```

### Description

This service stops the AutoIP protocol on a previously created and started AutoIP instance. This service is typically used when the IP address is changed via DHCP or manually to a non-AutoIP address.

### Input Parameters

- *auto_ip_ptr*: Pointer to AutoIP control block.

### Return Values

- **NX_SUCCESS**: (0x00) Successful AutoIP stop.
- **NX_AUTO_IP_ERROR**: (0xA00) AutoIP stop error.
- NX_PTR_ERROR: (0x16) Invalid AutoIP pointer.
- NX_CALLER_ERROR: (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c
/* Stop the AutoIP instance "auto_ip_0." */
status = nx_auto_ip_stop(&auto_ip_0);

/* If status is NX_SUCCESS an AutoIP instance was successfully stopped. */
```

### See Also

nx_auto_ip_create, nx_auto_ip_set_interface, nx_auto_ip_delete, nx_auto_ip_get_address, nx_auto_ip_start
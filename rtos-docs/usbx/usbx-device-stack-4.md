---
title: Chapter 4 - Description of USBX Device Services
description: Learn about the USBX Device Services.
---

# Description of USBX Device Services

This chapter contains a description of USBX device stack services in alphabetic order.

The USBX device stack API functions available to the application are as follows.
- [ux_device_stack_alternate_setting_get](#ux_device_stack_alternate_setting_get)
- [ux_device_stack_alternate_setting_set](#ux_device_stack_alternate_setting_set)
- [ux_device_stack_class_register](#ux_device_stack_class_register)
- [ux_device_stack_class_unregister](#ux_device_stack_class_unregister)
- [ux_device_stack_configuration_get](#ux_device_stack_configuration_get)
- [ux_device_stack_configuration_set](#ux_device_stack_configuration_set)
- [ux_device_stack_descriptor_send](#ux_device_stack_descriptor_send)
- [ux_device_stack_disconnect](#ux_device_stack_disconnect)
- [ux_device_stack_endpoint_stall](#ux_device_stack_endpoint_stall)
- [ux_device_stack_host_wakeup](#ux_device_stack_host_wakeup)
- [ux_device_stack_initialize](#ux_device_stack_initialize)
- [ux_device_stack_interface_delete](#ux_device_stack_interface_delete)
- [ux_device_stack_interface_get](#ux_device_stack_interface_get)
- [ux_device_stack_interface_set](#ux_device_stack_interface_set)
- [ux_device_stack_interface_start](#ux_device_stack_interface_start)
- [ux_device_stack_transfer_request](#ux_device_stack_transfer_request)
- [ux_device_stack_transfer_abort](#ux_device_stack_transfer_abort)
- [ux_device_stack_uninitialize](#ux_device_stack_uninitialize)
- [ux_device_stack_microsoft_extension_register](#ux_device_stack_microsoft_extension_register)

## ux_device_stack_alternate_setting_get

Get current alternate setting for an interface value

### Prototype

```c
UINT ux_device_stack_alternate_setting_get(ULONG interface_value);
```

### Description

This function is used by the USB host to obtain the current alternate setting for a specific interface value. It is called by the controller driver when a **GET_INTERFACE** request is received, to start data stage of the control transfer in background, and return immediately.

### Input Parameter

- *interface_value*: Interface value for which the current alternate setting is queried

### Return Values

- **UX_SUCCESS** (0x00) The data transfer was completed.
- **UX_ERROR** (0xFF) Wrong interface value.

### Example

```c
ULONG interface_value;
UINT status;

/* The following example illustrates this service. */
status = ux_device_stack_alternate_setting_get(interface_value);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_device_stack_alternate_setting_set

Set current alternate setting for an interface value

### Prototype

```c
UINT ux_device_stack_alternate_setting_set(
    ULONG interface_value, 
    ULONG alternate_setting_value);
```

### Description

This function is used by the USB host to set the current alternate setting for a specific interface value. It is called by the controller driver when a **SET_INTERFACE** request is received. When the **SET_INTERFACE** is completed, the values of the alternate settings are applied to the class.

The device stack will issue a **UX_SLAVE_CLASS_COMMAND_CHANGE** to the class that owns this interface to reflect the change of alternate setting.

The function starts status stage of the control transfer in background, and return immediately.

### Parameters

- *interface_value*: Interface value for which the current alternate setting is set.
- *alternate_setting_value*: The new alternate setting value.

### Return Values

- **UX_SUCCESS** (0x00) The data transfer was completed.
- **UX_INTERFACE_HANDLE_UNKNOWN** (0x52) No interface attached.
- **UX_FUNCTION_NOT_SUPPORTED** (0x54) Device is not configured.
- **UX_ERROR** (0xFF) Wrong interface value.

### Example

```c
ULONG interface_value;
ULONG alternate_setting_value;

/* The following example illustrates this service. */
status = ux_device_stack_alternate_setting_set(interface_value, alternate_setting_value);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_device_stack_class_register

Register a new USB device class

### Prototype

```c
UINT ux_device_stack_class_register(
    UCHAR *class_name,
    UINT (*class_entry_function)(struct UX_SLAVE_CLASS_COMMAND_STRUCT *),
    ULONG configuration_number, 
    ULONG interface_number,  
    VOID *parameter);
```

### Description

This function is used by the application to register a new USB device class. This registration starts a class container and not an instance of the class and returns immediately. A class should have an active thread and be attached to a specific interface.

Some classes expect a parameter or parameter list. For instance, the device storage class would expect the geometry of the storage device it is trying to emulate. The parameter field is therefore dependent on the class requirement and can be a value or a pointer to a structure filled with the class values.

> **Note:** The C string of class_name must be NULL-terminated and the length of it (without the NULL-terminator itself) must be no larger than **UX_MAX_CLASS_NAME_LENGTH**.

### Parameters

- *class_name*: Class Name
- *class_entry_function*: The entry function of the class.
- *configuration_number*: The configuration number this class is attached to.
- *interface_number*: The interface number this class is attached to.
- *parameter*: A pointer to a class specific parameter list.

### Return Values

- **UX_SUCCESS** (0x00) The class was registered
- **UX_MEMORY_INSUFFICIENT** (0x12) No entries left in class table.
- **UX_THREAD_ERROR** (0x16) Cannot create a class thread.

### Example

```c
UINT status;

/* The following example illustrates this service. */

/* Initialize the device storage class. The class is connected with interface 1 */
status = ux_device_stack_class_register(_ux_system_slave_class_storage_name ux_device_class_storage_entry, 1, 1, (VOID *)&parameter);
```

## ux_device_stack_class_unregister

Unregister a USB device class

### Prototype

```c
UINT ux_device_stack_class_unregister(
    UCHAR *class_name,
    UINT (*class_entry_function)(struct UX_SLAVE_CLASS_COMMAND_STRUCT*));
```

### Description

This function is used by the application to unregister a USB device class and returns immediately. 

The device stack will issue a **UX_SLAVE_CLASS_COMMAND_UNINITIALIZE** to the class to uninitialized.

> **Note:** The C string of class_name must be NULL-terminated and the
length of it (without the NULL-terminator itself) must be no larger than **UX_MAX_CLASS_NAME_LENGTH**.

### Parameters

- *class_name*: Class Name
- *class_entry_function*: The entry function of the class.

### Return Values

- **UX_SUCCESS** (0x00) The class was unregistered.
- **UX_NO_CLASS_MATCH** (0x57) The class isn't registered.

### Example

```c
/* The following example illustrates this service. */

/* Unitialize the device storage class. */
status = ux_device_stack_class_unregister(_ux_system_slave_class_storage_name, ux_device_class_storage_entry);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_device_stack_configuration_get

Get the current configuration

### Prototype

```c
UINT ux_device_stack_configuration_get(VOID);
```

### Description

This function is used by the host to obtain the current configuration running in the device. It is called by the controller driver when a GET_CONFIGURATION request is received.  It starts data stage of the control transfer in background, and returns immediately.

### Input Parameter

None

### Return Value

- **UX_SUCCESS** (0x00) The data transfer was completed.

### Example

```c
UINT status;

/* The following example illustrates this service. */
status = ux_device_stack_configuration_get();

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_device_stack_configuration_set

Set the current configuration

### Prototype

```c
UINT ux_device_stack_configuration_set(ULONG configuration_value);
```

### Description

This function is used by the host to set the current configuration running in the device. Upon reception of this command, the USB device stack will activate the alternate setting 0 of each interface connected to this configuration.

The function issues protocol error or starts status stage of the control transfer in background, and return immediately.

### Input Parameter

- *configuration_value*: The configuration value selected by the host.

### Return Value

- **UX_SUCCESS** (0x00) The configuration was successfully set.

### Example

```c
ULONG configuration_value;
UINT status;

/* The following example illustrates this service. */
status = ux_device_stack_configuration_set(configuration_value);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_device_stack_descriptor_send

Send a descriptor to the host

### Prototype

```c
UINT ux_device_stack_descriptor_send(
    ULONG descriptor_type, 
    ULONG request_index, 
    ULONG host_length);
```

### Description

This function is used by the device side to return a descriptor to the host. This descriptor can be a device descriptor, a configuration descriptor or a string descriptor.

The function issues protocol error or starts data stage of the control transfer in background, and return immediately.

### Parameters

- *descriptor_type*: The type of the descriptor. Must be one of the following values.
  - **UX_DEVICE_DESCRIPTOR_ITEM**
  - **UX_CONFIGURATION_DESCRIPTOR_ITEM**
  - **UX_STRING_DESCRIPTOR_ITEM**
  - **UX_DEVICE_QUALIFIER_DESCRIPTOR_ITEM**
  - **UX_OTHER_SPEED_DESCRIPTOR_ITEM**
- *request_index*: The index of the descriptor.
- *host_length*: The length required by the host.

### Return Values

- **UX_SUCCESS** (0x00) The data transfer was completed.
- **UX_ERROR** (0xFF) The transfer was not completed.

### Example

```c
ULONG descriptor_type;
ULONG request_index;
ULONG host_length;
UINT status;

/* The following example illustrates this service. */
status = ux_device_stack_descriptor_send(descriptor_type, request_index, host_length);

/* If status equals UX_SUCCESS, the operation was successful. */
```

### ux_device_stack_disconnect

Disconnect device stack

### Prototype

```c
UINT ux_device_stack_disconnect(VOID);
```

### Description

The VBUS manager calls this function when there is a device disconnection. The device stack will inform all classes registered to this device and will thereafter release all the device resources, and return immediately.

### Input Parameter

None

### Return Value

- **UX_SUCCESS** (0x00) The device was disconnected.

### Example

```c
UINT status;

/* The following example illustrates this service. */
status = ux_device_stack_disconnect();

/* If status equals UX_SUCCESS, the operation was successful. */
```

### ux_device_stack_endpoint_stall

Request endpoint Stall condition

### Prototype

```c
UINT ux_device_stack_endpoint_stall(UX_SLAVE_ENDPOINT *endpoint);
```

### Description

This function is called by the USB device class when an endpoint should return a Stall condition to the host.

The function calls device controller driver to stall the endpoint, and return immediately.

### Input Parameter

- *endpoint*: The endpoint on which the Stall condition is requested.

### Return Value

- **UX_SUCCESS** (0x00) This operation was successful.
- **UX_ERROR** (0xFF) The device is in an invalid state.

### Example

```c
UINT status;

/* The following example illustrates this service. */
status = ux_device_stack_endpoint_stall(endpoint);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_device_stack_host_wakeup

Wake up the host

### Prototype

```c
UINT ux_device_stack_host_wakeup(VOID);
```

### Description

This function is called when the device wants to wake up the host. This command is only valid when the device is in suspend mode. It is up to the device application to decide when it wants to wake up the USB host. For instance, a USB modem can wake up a host when it detects a RING signal on the telephone line.

The function calls device controller driver to issue the wakeup signal, and return immediately.

### Input Parameter

None

### Return values

- **UX_SUCCESS** (0x00) The call was successful.
- **UX_FUNCTION_NOT_SUPPORTED** (0x54) The call failed (the device was probably not in the suspended mode).
- **UX_ERROR** (0xFF) The call failed.

### Example

```c
UINT status;

/* The following example illustrates this service. */
status = ux_device_stack_host_wakeup();

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_device_stack_initialize

Initialize USB device stack

### Prototype

```c
UINT ux_device_stack_initialize(
    UCHAR *device_framework_high_speed,
    ULONG device_framework_length_high_speed,
    UCHAR *device_framework_full_speed,
    ULONG device_framework_length_full_speed,
    UCHAR *string_framework,
    ULONG string_framework_length,
    UCHAR *language_id_framework,
    ULONG language_id_framework_length,
    UINT (*ux_system_slave_change_function)(ULONG));
```

### Description

This function is called by the application to initialize the USB device stack. It does not initialize any classes or any controllers. This should be done with separate function calls. This call mainly provides the stack with the device framework for the USB function. It supports both high and full speeds with the possibility to have completely separate device framework for each speed. String framework and multiple languages are supported.

### Parameters

- *device_framework_high_speed*: Pointer to the high speed framework.
- *device_framework_length_high_speed*: Length of the high speed framework.
- *device_framework_full_speed*: Pointer to the full speed framework.
- *device_framework_length_full_speed*: Length of the full speed framework.
- *string_framework*: Pointer to string framework.
- *string_framework_length*: Length of string framework.
- *language_id_framework*: Pointer to string language framework.
- *language_id_framework_length*: Length of the string language framework.
- *ux_system_slave_change_function*: Function to be called when the device state changes.

### Return Values

- **UX_SUCCESS** (0x00) This operation was successful.
- **UX_MEMORY_INSUFFICIENT** (0x12) Not enough memory to initialize the stack.
- **UX_DESCRIPTOR_CORRUPTED** (0x42) The descriptor is invalid.

### Example

```c
/* Example of a device framework */

#define DEVICE_FRAMEWORK_LENGTH_FULL_SPEED 50

UCHAR device_framework_full_speed[] = {
    /* Device descriptor */
    0x12, 0x01, 0x10, 0x01, 0x00, 0x00, 0x00, 0x08,
    0xec, 0x08, 0x10, 0x00, 0x00, 0x00, 0x00, 0x00,
    0x00, 0x01,

    /* Configuration descriptor */
    0x09, 0x02, 0x20, 0x00, 0x01, 0x01, 0x00, 0xc0,
    0x32,

    /* Interface descriptor */
    0x09, 0x04, 0x00, 0x00, 0x02, 0x08, 0x06, 0x50,
    0x00,

    /* Endpoint descriptor (Bulk Out) */
    0x07, 0x05, 0x01, 0x02, 0x40, 0x00, 0x00,

    /* Endpoint descriptor (Bulk In) */
    0x07, 0x05, 0x82, 0x02, 0x40, 0x00, 0x00
};

#define DEVICE_FRAMEWORK_LENGTH_HIGH_SPEED 60

UCHAR device_framework_high_speed[] = {
    /* Device descriptor */
    0x12, 0x01, 0x00, 0x02, 0x00, 0x00, 0x00, 0x40,
    0x0a, 0x07, 0x25, 0x40, 0x01, 0x00, 0x01, 0x02,
    0x03, 0x01,

    /* Device qualifier descriptor */
    0x0a, 0x06, 0x00, 0x02, 0x00, 0x00, 0x00, 0x40,
    0x01, 0x00,

    /* Configuration descriptor */
    0x09, 0x02, 0x20, 0x00, 0x01, 0x01, 0x00, 0xc0,
    0x32,

    /* Interface descriptor */
    0x09, 0x04, 0x00, 0x00, 0x02, 0x08, 0x06, 0x50,
    0x00,

    /* Endpoint descriptor (Bulk Out) */
    0x07, 0x05, 0x01, 0x02, 0x00, 0x02, 0x00,

    /* Endpoint descriptor (Bulk In) */
    0x07, 0x05, 0x82, 0x02, 0x00, 0x02, 0x00
};

/* String Device Framework:
    Byte 0 and 1: Word containing the language ID: 0x0904 for US
    Byte 2 : Byte containing the index of the descriptor
    Byte 3 : Byte containing the length of the descriptor string */

#define STRING_FRAMEWORK_LENGTH 38 UCHAR string_framework[] = {
    /* Manufacturer string descriptor: Index 1 */
    0x09, 0x04, 0x01, 0x0c,
    0x45, 0x78, 0x70, 0x72,0x65, 0x73, 0x20, 0x4c,
    0x6f, 0x67, 0x69, 0x63,

    /* Product string descriptor: Index 2 */
    0x09, 0x04, 0x02, 0x0c,
    0x4D, 0x4C, 0x36, 0x39, 0x36, 0x35, 0x30, 0x30,
    0x20, 0x53, 0x44, 0x4B,

    /* Serial Number string descriptor: Index 3 */
    0x09, 0x04, 0x03, 0x04,
    0x30, 0x30, 0x30, 0x31
};

/* Multiple languages are supported on the device, to add a language besides English, 
  the Unicode language code must be appended to the language_id_framework array 
  and the length adjusted accordingly. */

#define LANGUAGE_ID_FRAMEWORK_LENGTH 2

UCHAR language_id_framework[] = {
    /* English. */
    0x09, 0x04
};
```

The application can request a call back when the controller changes its state. The two main states for the controller are:

- **UX_DEVICE_SUSPENDED**
- **UX_DEVICE_RESUMED**

If the application does not need Suspend/Resume signals, it would supply a UX_NULL function.

```c
UINT status;

/* The code below is required for installing the device portion of USBX. 
    There is no call back for device status change in this example. */
status = ux_device_stack_initialize(&device_framework_high_speed, DEVICE_FRAMEWORK_LENGTH_HIGH_SPEED,
                                    &device_framework_full_speed, DEVICE_FRAMEWORK_LENGTH_FULL_SPEED,
                                    &string_framework, STRING_FRAMEWORK_LENGTH,
                                    &language_id_framework, LANGUAGE_ID_FRAMEWORK_LENGTH,
                                    UX_NULL);

/* If status equals UX_SUCCESS, initialization was successful. */
```

## ux_device_stack_interface_delete

Delete a stack interface

### Prototype

```c
UINT ux_device_stack_interface_delete(UX_SLAVE_INTERFACE *ux_interface);
```

### Description

This function is called when an interface should be removed. An interface is either removed when a device is extracted, or following a bus reset, or when there is a new alternate setting.

### Input Parameter

- *ux_interface*: Pointer to the interface to remove.

### Return Value

- **UX_SUCCESS** (0x00) This operation was successful.

### Example

```c
UINT status;

/* The following example illustrates this service. */
status = ux_device_stack_interface_delete(interface);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_device_stack_interface_get

Get the current interface value

### Prototype

```c
UINT ux_device_stack_interface_get(UINT interface_value);
```

### Description

This function is called when the host queries the current interface. The device returns the current interface value by transfer in background and returns immediately.

> **Note:** This function is deprecated. It is available for legacy software, but new software should use the ***ux_device_stack_alternate_setting_get*** function instead.

### Input Parameter

- *interface_value*: Interface value to return.

### Return Values

- **UX_SUCCESS** (0x00) This operation was successful.
- **UX_ERROR** (0xFF) No interface exists.

### Example

```c
ULONG interface_value;

UINT status;

/* The following example illustrates this service. */
status = ux_device_stack_interface_get(interface_value);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_device_stack_interface_set

Change the alternate setting of the interface

### Prototype

```c
UINT ux_device_stack_interface_set(
    UCHAR *device_framework,
    ULONG device_framework_length, 
    ULONG alternate_setting_value);
```

### Description

This function is called when the host requests a change of the alternate setting for the interface.

This function starts status stage of the control transfer in background, and return immediately.

### Parameters

- *device_framework*: Address of the device framework for this interface.
- *device_framework_length*: Length of the device framework.
- *alternate_setting_value*: Alternate setting value to be used by this interface.

### Return Values

- **UX_SUCCESS** (0x00) This operation was successful.
- **UX_ERROR** (0xFF) No interface exists.

### Example

```c
UCHAR *device_framework
ULONG device_framework_length;
ULONG alternate_setting_value;
UINT  status;

/* The following example illustrates this service. */
status = ux_device_stack_interface_set(device_framework, device_framework_length, alternate_setting_value);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_device_stack_interface_start

Start search for a class to own an interface instance

### Prototype

```c
UINT ux_device_stack_interface_start(UX_SLAVE_INTERFACE *ux_interface);
```

### Description

This function is called when an interface has been selected by the host and the device stack needs to search for a device class to own this interface instance.

The device stack will issue a **UX_SLAVE_CLASS_COMMAND_QUERY** to the class that owns this interface to check if class match.

This function is not blocking and returns immediately.

### Input Parameter

- *ux_interface*: Pointer to the interface created.

### Return Values

- **UX_SUCCESS** (0x00) This operation was successful.
- **UX_NO_CLASS_MATCH** (0x57) No class exists for this interface.

### Example

```c
UINT status;

/* The following example illustrates this service. */
status = ux_device_stack_interface_start(interface);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_device_stack_transfer_request

Request to transfer data to the host

### Prototype

```c
UINT ux_device_stack_transfer_request(
    UX_SLAVE_TRANSFER *transfer_request,
    ULONG slave_length, 
    ULONG host_length);
```

### Description

This function is called when a class or the stack wants to transfer data to the host. The host always polls the device but the device can prepare data in advance.

This function starts transfer in background and return immediately if endpoint is in normal state. If non-control endpoint is halted, when function is invoked, it waits the halt state to be cleared to continue starting transfer.

### Parameters

- *transfer_request*: Pointer to the transfer request.
- *slave_length*: Length the device wants to return.
- *host_length*: Length the host has requested.

### Return Values

- **UX_SUCCESS** (0x00) This operation was successful.
- **UX_TRANSFER_NOT_READY** (0x25) The device is in an invalid state; it must be **ATTACHED**, **CONFIGURED**, or **ADDRESSED**.
- **UX_ERROR** (0xFF) Transport error.

### Example

```c
UINT status;

/* The following example illustrates how to transfer more data than an application requests. */
while(total_length) {
    /* How much can we send in this transfer? */
    if (total_length > UX_SLAVE_CLASS_STORAGE_BUFFER_SIZE)
        transfer_length = UX_SLAVE_CLASS_STORAGE_BUFFER_SIZE;
    else
        transfer_length = total_length;

   /* Copy the Storage Buffer into the transfer request memory. */
   ux_utility_memory_copy(transfer_request ->  ux_slave_transfer_request_data_pointer,
       media_memory, transfer_length);

   /* Send the data payload back to the caller. */
   status = ux_device_transfer_request(transfer_request,
       transfer_length, transfer_length);

   /* If status equals UX_SUCCESS, the operation was successful. */

   /* Update the buffer address.  */
    media_memory += transfer_length;

   /* Update the length to remain. */
    total_length -= transfer_length;
}
```

## ux_device_stack_transfer_abort

Cancel a transfer request

### Prototype

```c
UINT ux_device_stack_transfer_abort(
    UX_SLAVE_TRANSFER *transfer_request, 
    ULONG completion_code);
```

### Description

This function is called when an application needs to cancel a transfer request or when the stack needs to abort a transfer request associated with an endpoint, and return immediately.

### Parameters

- *transfer_request*: Pointer to the transfer request.
- *completion_code*: Error code to be returned to the class waiting for this transfer request to complete.

### Return Value

- **UX_SUCCESS** (0x00) This operation was successful.

### Example

```c
UINT status;

/* The following example illustrates how to abort a transfer when a
    bus reset has been detected on the bus. */
status = ux_device_stack_transfer_abort(transfer_request, UX_TRANSFER_BUS_RESET);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_device_stack_uninitialize

Uninitialize stack

### Prototype

```c
UINT ux_device_stack_uninitialize();
```

### Description

This function is called when an application needs to uninitialize the USBX device stack – all device stack resources are freed, and return immediately. This should be called after all classes have been unregistered via ux_device_stack_class_unregister.

### Parameters

None

### Return Value

**UX_SUCCESS** (0x00) This operation was successful.

## ux_device_stack_microsoft_extension_register

Registers the Microsoft extensions

### Prototype

```c
UINT ux_device_stack_microsoft_extension_register(
    ULONG vendor_request, 
    UINT (*vendor_request_function)(ULONG, ULONG, ULONG, ULONG, UCHAR *, ULONG *))
```

### Description

This function registers the Microsoft extensions to support vendor commands before the device is configured.

### Parameters

- *vendor_request*: Vendor Command. 
- *vendor_request_function*: Vendor Command application Callback.

### Return Value

**UX_SUCCESS** (0x00) This operation was successful.

### Example

```c
#define UX_DEMO_VENDOR_REQUEST 0x54

/* MS extensions.  */
status = _ux_device_stack_microsoft_extension_register(UX_DEMO_VENDOR_REQUEST, pima_device_vendor_request);


UINT pima_device_vendor_request(ULONG request, ULONG request_value, ULONG request_index, ULONG request_length,
                       UCHAR *transfer_request_buffer,
                       ULONG *transfer_request_length)
{
UINT    status;
ULONG   length;

    /* Do some sanity check.  The request must be our vendor request. */
    if (request != UX_TEST_VENDOR_REQUEST)

        /* Do not proceed.  */
        return(UX_ERROR);

    /* Check the wIndex value. Values can be :
        0x0001 : Genre
        0x0004 : Extended compatible ID
        0x0005 : Extended properties */
    switch (request_index)
    {

        case    0x0001 :

            /* Not sure what this is for. Windows does not seem to request this. Drop it.  */
            status = UX_ERROR;
            break;

        case    0x0004 :
        case    0x0005 :

            /* Length to return.  */
            length = UX_MIN(0x28, request_length);

            /* Length check.  */
            UX_ASSERT(*transfer_request_length >= length);

            /* At least length should be returned.  */
            if (length < 4)
            {
                status = UX_ERROR;
                break;
            }
            status = UX_SUCCESS;

            /* Return the length.  */
            *transfer_request_length = length;

            /* Reset returned bytes.  */
            ux_utility_memory_set(transfer_request_buffer, 0, length);

            /* Build the descriptor to be returned.  This is not a composite descriptor. Single MTP.
                First dword is length of the descriptor.  */
            ux_utility_long_put(transfer_request_buffer, 0x0028);
            length -= 4;

            /* Then the version. fixed to 0x0100.  */
            if (length < 2)
                break;
            ux_utility_short_put(transfer_request_buffer + 4, 0x0100);
            length -= 2;

            /* Then the descriptor ID. Fixed to 0x0004.  */
            if (length < 2)
                break;
            ux_utility_short_put(transfer_request_buffer + 6, 0x0004);
            length -= 2;

            /* Then the bcount field. Fixed to 0x0001.  */
            if (length < 1)
                break;
            *(transfer_request_buffer + 8) = 0x01;
            length -= 1;

            /* Reset the next 7 bytes.  */
            if (length < 7)
                break;
            ux_utility_memory_set(transfer_request_buffer + 9, 0x00, 7);
            length -= 7;

            /* Last byte of header is the interface number, here 0.  */
            if (length < 1)
                break;
            *(transfer_request_buffer + 16) = 0x00;
            length -= 1;

            /* First byte of descriptor is set to 1.  */
            if (length < 1)
                break;
            *(transfer_request_buffer + 17) = 0x01;
            length -= 1;

            /* Reset the next 8 + 8 + 6 bytes.  */
            if (length < (8+8+6))
                break;
            ux_utility_memory_set(transfer_request_buffer + 18, 0x00, (8 + 8 + 6));
            length -= 8+8+6;

            /* Set the compatible ID to MTP.  */
            if (length < 3)
                break;
            ux_utility_memory_copy(transfer_request_buffer + 18, "MTP", 3);
            length -= 3;

            /* We are done here.  */
            status = UX_SUCCESS;
            break;

        default :
            status = UX_ERROR;
            break;

    }
    /* Return status to device stack.  */
    return(status);
}

```

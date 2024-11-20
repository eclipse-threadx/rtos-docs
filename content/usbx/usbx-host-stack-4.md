---
title: Chapter 4 - Description of USBX Host Services
description: Learn about the USBX Host Services.
---

## ux_host_stack_initialize

Initialize USBX for host operation.

### Prototype

```c
UINT ux_host_stack_initialize(
    UINT (*system_change_function)
    (ULONG, UX_HOST_CLASS *,VOID *));
```

### Description

This function will initialize the USB host stack and return immediately. The supplied memory area will be setup for USBX internal use. If UX_SUCCESS is returned, USBX is ready for host controller and class registration.

### Input Parameter

- *system_change_function*: Pointer to optional callback routine for notifying application of instance changes.
  The change callback function is as following:
  ```c
  UINT demo_ux_system_host_change_function(ULONG change_code, UX_HOST_CLASS *ux_class, VOID *param);
  ```
  The possible changes notification parameters are:

  |       UX Change Code       |     Pointer to Class     |              Pointer to Instance               |                             Description                             |
  | -------------------------- | ------------------------ | ---------------------------------------------- | ------------------------------------------------------------------- |
  | UX_DEVICE_INSERTION        | Class (`UX_HOST_CLASS*`) | Instance                                       | Registered class created and activated a function instance          |
  | UX_DEVICE_REMOVAL          | Class (`UX_HOST_CLASS*`) | Instance                                       | Function instance is deactivated                                    |
  | UX_HID_CLIENT_INSERTION    | Class (`UX_HOST_CLASS*`) | HID Client (`UX_HOST_CLASS_HID_CLIENT*`)       | HID class client (keyboard, mouse, remote control ...) is activated |
  | UX_HID_CLIENT_REMOVAL      | Class (`UX_HOST_CLASS*`) | HID Client (`UX_HOST_CLASS_HID_CLIENT*`)       | HID class client is deactivated                                     |
  | UX_STORAGE_MEDIA_INSERTION | Class (`UX_HOST_CLASS*`) | Storage Media (`UX_HOST_CLASS_STORAGE_MEDIA*`) | Storage media is ready (only available for No-FileX mode)           |
  | UX_STORAGE_MEDIA_REMOVAL   | Class (`UX_HOST_CLASS*`) | Storage Media (`UX_HOST_CLASS_STORAGE_MEDIA*`) | Storage media is removed (only available for No-FileX mode)         |
  | UX_DEVICE_CONNECTION       | Null (`UX_NULL`)         | Device (`UX_DEVICE*`)                          | Device is connected                                                 |
  | UX_DEVICE_DISCONNECTION    | Null (`UX_NULL`)         | Device (`UX_DEVICE*`)                          | Device is disconnected                                              |

### Return Value

- **UX_SUCCESS** (0x00) Successful initialization.
- **UX_MEMORY_INSUFFICIENT** (0x12) A memory allocation failed.

### Example

```c
UINT status;

/* Initialize USBX for host operation, without notification. */
status = ux_host_stack_initialize(UX_NULL);

/* If status equals UX_SUCCESS, USBX has been successfully initialized for host operation. */
```

## ux_host_stack_endpoint_transfer_abort

Abort all transactions attached to a transfer request for an endpoint.

### Prototype

```c
UINT ux_host_stack_endpoint_transfer_abort(UX_ENDPOINT *endpoint);
```

### Description

This function will cancel all transactions active or pending for a specific transfer request attached to an endpoint and return immediately. If the transfer request has a callback function attached, the callback function will be called with the UX_TRANSACTION_ABORTED status.

### Input Parameter

- *endpoint*: Pointer to an endpoint.

### Return Values

- **UX_SUCCESS** (0x00) No errors.
- **UX_ENDPOINT_HANDLE_UNKNOWN** (0x53) Endpoint handle is not valid.

### Example

```c
UX_HOST_CLASS_PRINTER *printer;
UINT status;

/* Get the instance for this class. */
printer = (UX_HOST_CLASS_PRINTER *) command ->
    ux_host_class_command_instance;

/* The printer is being shut down. */

printer -> printer_state = UX_HOST_CLASS_INSTANCE_SHUTDOWN;

/* We need to abort transactions on the bulk out pipe. */
status = ux_host_stack_endpoint_transfer_abort
    (printer -> printer_bulk_out_endpoint);

/* If status equals UX_SUCCESS, the operation was successful */
```

## ux_host_stack_class_get

Get the pointer to a class container.

### Prototype

```c
UINT ux_host_stack_class_get(
    UCHAR *class_name,
    UX_HOST_CLASS **class);
```

### Description

This function returns a pointer to the class container immediately. A class needs to obtain its container from the USB stack to search for instances when a class or an application wants to open a device.

> **Note:** The C string of class_name must be NULL-terminated and the length of it (without the NULL-terminator itself) must be no larger than UX_MAX_CLASS_NAME_LENGTH.

### Parameters

- *class_name*: Pointer to the class name.
- *class*: A pointer updated by the function call that contains the class container for the name of the class.

### Return Values

- **UX_SUCCESS** (0x00) No errors, on return the class field is filed with the pointer to the class container.
- **UX_HOST_CLASS_UNKNOWN** (0x59) Class is unknown by the stack.

### Example

```c
UX_HOST_CLASS *printer_container;
UINT status;

/* Get the container for this class. */
status = ux_host_stack_class_get("ux_host_class_printer", &printer_container);

/* If status equals UX_SUCCESS, the operation was successful */
```

## ux_host_stack_class_register

Register a USB class to the USB stack.

### Prototype

```c
UINT ux_host_stack_class_register(
    UCHAR *class_name,
    UINT (*class_entry_address) (struct UX_HOST_CLASS_COMMAND_STRUCT *));
```

### Description

This function registers a USB class to the USB stack and returns immediately. The class must
specify an entry point for the USB stack to send commands such as the following.

- **UX_HOST_CLASS_COMMAND_QUERY**
- **UX_HOST_CLASS_COMMAND_ACTIVATE**
- **UX_HOST_CLASS_COMMAND_DESTROY**

> **Note:** The C string of *class_name* must be NULL-terminated and the length of it (without the NULL-terminator itself) must be no larger than **UX_MAX_CLASS_NAME_LENGTH**.

### Parameters

- *class_name*: Pointer to the name of the class, valid entries are found in the file ux_system_initialize.c under the USB Classes of USBX.
- *class_entry_address*: Address of the entry function of the class.

### Return Values

- **UX_SUCCESS** (0x00) Class installed successfully.
- **UX_MEMORY_ARRAY_FULL** (0x1a) No more memory to store this class.
- **UX_HOST_CLASS_ALREADY_INSTALLED** (0x58) Host class already installed.

### Example:

```c
UINT status;

/* Register all the classes for this implementation. */
status = ux_host_stack_class_register("ux_host_class_hub", ux_host_class_hub_entry);

/* If status equals UX_SUCCESS, class was successfully installed. */
```

## ux_host_stack_class_instance_create

Create a new class instance for a class container.

### Prototype

```c
UINT ux_host_stack_class_instance_create(
    UX_HOST_CLASS *class,
    VOID *class_instance);
```

### Description

This function creates a new class instance for a class container and returns immediately. The instance of a class is not contained in the class code to reduce the class complexity. Rather, each class instance is attached to the class container located in the main stack.

### Parameters

- *class*: Pointer to the class container.
- *class_instance*: Pointer to the class instance to be created.

### Return Value

- **UX_SUCCESS** (0x00) The class instance was attached to the class container.

### Example

```c
UINT status;

UX_HOST_CLASS_PRINTER *printer;

/* Obtain memory for this class instance. */

printer = ux_memory_allocate(UX_NO_ALIGN, sizeof(UX_HOST_CLASS_PRINTER));

if (printer == UX_NULL)
    return(UX_MEMORY_INSUFFICIENT);

/* Store the class container into this instance. */
printer -> printer_class = command -> ux_host_class;

/* Create this class instance. */
status = ux_host_stack_class_instance_create(printer -> printer_class, (VOID *)printer);

/* If status equals UX_SUCCESS, the class instance was successfully created and attached to the class container. */
```

## ux_host_stack_class_instance_destroy

Destroy a class instance for a class container.

### Prototype

```c
UINT ux_host_stack_class_instance_destroy(
    UX_HOST_CLASS *class,
    VOID *class_instance);
```

### Description

This function destroys a class instance for a class container and returns immediately.

### Parameters

- *class*: Pointer to the class container.
- *class_instance*: Pointer to the instance to destroy.

### Return Values

- **UX_SUCCESS** (0x00) The class instance was destroyed.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) The class instance is not attached to the class container.

### Example

```c
UINT status;
UX_HOST_CLASS_PRINTER *printer;

/* Get the instance for this class. */
printer = (UX_HOST_CLASS_PRINTER *) command -> ux_host_class_command_instance;

/* The printer is being shut down. */
printer -> printer_state = UX_HOST_CLASS_INSTANCE_SHUTDOWN;

/* Destroy the instance. */
status = ux_host_stack_class_instance_destroy(printer -> printer_class, (VOID *) printer);

/* If status equals UX_SUCCESS, the class instance was successfully destroyed. */
```

## ux_host_stack_class_instance_get

Get a class instance pointer for a specific class.

### Prototype

```c
UINT ux_host_stack_class_instance_get(
    UX_HOST_CLASS *class,
    UINT class_index,
    VOID **class_instance);
```

### Description

This function returns a class instance pointer for a specific class immediately. The instance of a class is not contained in the class code to reduce the class complexity. Rather, each class instance is attached to the class container. This function is used to search for class instances within a class container.

### Parameters

- *class*: Pointer to the class container.
- *class_index*: An index to be used by the function call within the list of attached classes to the container.
- *class_instance*: Pointer to the instance to be returned by the function call.

### Return Values

- **UX_SUCCESS** (0x00) The class instance was found.

- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) There are no more class instances attached to the class container.

### Example

```c
UINT status;

UX_HOST_CLASS_PRINTER *printer;

/* Obtain memory for this class instance. */
printer = ux_memory_allocate(UX_NO_ALIGN, sizeof(UX_HOST_CLASS_PRINTER));

if (printer == UX_NULL) return(UX_MEMORY_INSUFFICIENT);

/* Search for instance index 2. */
status = ux_host_stack_class_instance_get(class, 2, (VOID *) printer);

/* If status equals UX_SUCCESS, the class instance was found. */
```

## ux_host_stack_device_configuration_get

Get a pointer to a configuration container.

### Prototype

```c
UINT ux_host_stack_device_configuration_get(
    UX_DEVICE *device,
    UINT configuration_index,
    UX_CONFIGURATION *configuration);
```

### Description

This function returns a configuration container based on a device handle and a configuration index, immediately.

### Parameters

- *device*: Pointer to the device container that owns the configuration requested.
- *configuration_index*: Index of the configuration to be searched.
- *configuration*: Address of the pointer to the configuration container to be returned.

### Return Values

- **UX_SUCCESS** (0x00) The configuration was found.
- **UX_DEVICE_HANDLE_UNKNOWN** (0x50) The device container does not exist.
- **UX_CONFIGURATION_HANDLE_UNKNOWN** (0x51) The configuration handle for the index does not exist.

### Example

```c
UINT status;

UX_HOST_CLASS_PRINTER *printer;

/* If the device has been configured already, we don't need to do it
again. */

if (printer -> printer_device -> ux_device_state == UX_DEVICE_CONFIGURED)
    return(UX_SUCCESS);

/* A printer normally has one configuration, retrieve 1st configuration only. */

status = ux_host_stack_device_configuration_get(printer -> printer_device,
    0, configuration);

/* If status equals UX_SUCCESS, the configuration was found. */
```

## ux_host_stack_device_configuration_select

Select a specific configuration for a device.

### Prototype

```c
UINT ux_host_stack_device_configuration_select (UX_CONFIGURATION *configuration);
```

### Description

This function selects a specific configuration for a device. When this configuration is set to the device, by default, each device interface and its associated alternate setting 0 is activated on the device. If the device/interface class wishes to change the setting of a particular interface. It issues a **ux_host_stack_configuration_set** service call and wait it end to continue.

### Parameters

- *configuration*: Pointer to the configuration container that is to be enabled for this device.

### Return Values

- **UX_SUCCESS** (0x00) The configuration selection was successful.
- **UX_CONFIGURATION_HANDLE_UNKNOWN** (0x51) The configuration handle does not exist.
- **UX_OVER_CURRENT_CONDITION** (0x43) An over current condition exists on the bus for this configuration.

### Example

```c
UINT status;

UX_HOST_CLASS_PRINTER *printer;

/* If the device has been configured already, we don't need to do it again. */
if (printer -> printer_device -> ux_device_state == UX_DEVICE_CONFIGURED)
    return(UX_SUCCESS);

/* A printer normally has one configuration - retrieve 1st configuration only. */
status = ux_host_stack_device_configuration_get(printer -> printer_device, 0,configuration);

/* If status equals UX_SUCCESS, the configuration selection was successful. */

/* If valid configuration, ask USBX to set this configuration. */
status = ux_host_stack_device_configuration_select(configuration);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_stack_device_get

Get a pointer to a device container.

### Prototype

```c
UINT ux_host_stack_device_get(
    ULONG device_index,
    UX_DEVICE *device);
```

### Description

This function returns a device container based on its index immediately. The device index starts with 0. Note that the index is a ULONG because we could have several controllers and a byte index might not be enough. The device index should not be confused with the device address that is bus specific.

### Parameters

- *device_index*: Index of the device.
- *device*: Address of the pointer for the device container to return.

### Return Values

- **UX_SUCCESS** (0x00) The device container exists and is returned
- **UX_DEVICE_HANDLE_UNKNOWN** (0x50) Device unknown

### Example

```c
UINT status;

/* Locate the first device in USBX. */
status = ux_host_stack_device_get(0, device);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_stack_interface_endpoint_get

Get an endpoint container.

### Prototype

```c
UINT ux_host_stack_interface_endpoint_get(
    UX_INTERFACE *interface,
    UINT endpoint_index,
    UX_ENDPOINT *endpoint);
```

### Description

This function returns an endpoint container based on the interface handle and an endpoint index immediately. It is assumed that the alternate setting for the interface has been selected or the default setting is being used prior to the endpoint(s) being searched.

### Parameters

- *interface*: Pointer to the interface container that contains the endpoint requested.
- *endpoint_index*: Index of the endpoint in this interface.
- *endpoint*: Address of the endpoint container to be returned.

### Return Values

- **UX_SUCCESS** (0x00) The endpoint container exists and is returned.
- **UX_INTERFACE_HANDLE_UNKNOWN** (0x52) Interface specified does not exist.
- **UX_ENDPOINT_HANDLE_UNKNOWN** (0x53) Endpoint index does not exist.

### Example

```c
UINT status;
UX_HOST_CLASS_PRINTER *printer;

for(endpoint_index = 0;
    endpoint_index < printer -> printer_interface ->
        ux_interface_descriptor.bNumEndpoints;
    endpoint_index++)
{
    status = ux_host_stack_interface_endpoint_get (printer -> printer_interface,
        endpoint_index, &endpoint);

    if (status == UX_SUCCESS)
    {
        /* Check if endpoint is bulk and OUT. */
        if (((endpoint -> ux_endpoint_descriptor.bEndpointAddress &
            UX_ENDPOINT_DIRECTION) == UX_ENDPOINT_OUT) &&
            ((endpoint -> ux_endpoint_descriptor.bmAttributes &
            UX_MASK_ENDPOINT_TYPE) == UX_BULK_ENDPOINT))
        return(UX_SUCCESS);
    }
}
```

## ux_host_stack_hcd_register

Register a USB controller to the USB stack.

### Prototype

```c
UINT ux_host_stack_hcd_register(
    UCHAR *hcd_name,
    UINT (*hcd_function)(struct UX_HCD_STRUCT *),
    ULONG hcd_param1, ULONG hcd_param2);
```

### Description

This function registers a USB controller to the USB stack and returns immediately. It mainly allocates the memory used by this controller and passes the initialization command to the controller.

> **Note:** The C string of hcd_name must be NULL-terminated and the length of it (without the NULL-terminator itself) must be no larger than **UX_MAX_HCD_NAME_LENGTH**.

### Parameters

- *hcd_name*: Name of the host controller
- *hcd_function*: The function in the host controller responsible for the initialization.
- *hcd_param1*: The IO or memory resource used by the hcd.
- *hcd_param2*: The IRQ used by the host controller.

### Return Values

- **UX_SUCCESS** (0x00) The controller was initialized properly.
- **UX_MEMORY_INSUFFICIENT** (0x12) Not enough memory for this controller.
- **UX_PORT_RESET_FAILED** (0x31) The reset of the controller failed.
- **UX_CONTROLLER_INIT_FAILED** (0x32) The controller failed to initialize properly.

### Example

```c
UINT status;

/* Initialize a host controller mapped at address 0xd0000 and using IRQ 10. */

status = ux_host_stack_hcd_register("ux_hcd_controller",
    ux_hcd_controller_initialize, 0xd0000, 0x0a);

/* If status equals UX_SUCCESS, the controller was initialized properly. */

/* Note that the application must also setup a call to the
    interrupt handler for the controller.
    The function for the controller is called _ux_hch_controller_interrupt_handler. */
```

## ux_host_stack_configuration_interface_get

Get an interface container pointer.

### Prototype

```c
UINT ux_host_stack_configuration_interface_get (
    UX_CONFIGURATION *configuration,
    UINT interface_index,
    UINT alternate_setting_index,
    UX_INTERFACE **interface);
```

### Description

This function returns an interface container based on a configuration handle, an interface index, and an alternate setting index, immediately.

### Parameters

- *configuration*: Pointer to the configuration container that owns the interface.
- *interface_index*: Interface index to be searched.
- *alternate_setting_index*: Alternate setting within the interface to search.
- *interface*: Address of the interface container pointer to be returned.

### Return Values

- **UX_SUCCESS** (0x00) The interface container for the interface index and the alternate setting was found and returned.
- **UX_CONFIGURATION_HANDLE_UNKNOWN** (0x51) The configuration does not exist.
- **UX_INTERFACE_HANDLE_UNKNOWN** (0x52) The interface does not exist.

### Example

```c
UINT status;

/* Search for the default alternate setting on the first interface for the printer. */
status = ux_host_stack_configuration_interface_get(configuration, 0, 0,
    &printer -> printer_interface);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_stack_interface_setting_select

Select an alternate setting for an interface.

### Prototype

```c
UINT ux_host_stack_interface_setting_select(UX_INTERFACE *interface);
```

### Description

This function selects a specific alternate setting for a given interface belonging to the selected configuration. This function is used to change from the default alternate setting to a new setting or to go back to the default alternate setting. When a new alternate setting is selected, the previous endpoint characteristics are invalid and should be reloaded. It issues a **_ux_host_stack_interface_set** service call, where transfer request is issued an waited before continue.

### Input Parameter

- *interface*: Pointer to the interface container whose alternate setting is to be selected.

### Return Values

- **UX_SUCCESS** (0x00) The alternate setting for this interface has been successfully selected.
- **UX_INTERFACE_HANDLE_UNKNOWN** (0x52) The interface does not exist.

### Example

```c
UINT status;

/* Select a new alternate setting for this interface. */
status = ux_host_stack_interface_setting_select(interface);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_stack_transfer_request_abort

Abort a pending transfer request.

### Prototype

```c
UINT ux_host_stack_transfer_request_abort(UX_TRANSFER REQUEST *transfer request);
```

### Description

This function aborts a pending transfer request that has been previously submitted and returns immediately. This function only cancels a specific transfer request. The call back to the function will have the UX_TRANSFER REQUEST_STATUS_ABORT status.

### Parameters

- *transfer request*: Pointer to the transfer request to be aborted.

### Return Values

- **UX_SUCCESS** (0x00) The USB transfer for this transfer request was
canceled.

### Example

```c
UINT status;

/* The following example illustrates this service. */
status = ux_host_stack_transfer_request_abort(transfer request);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_stack_transfer_request

Request a USB transfer.

### Prototype

```c
UINT ux_host_stack_transfer_request(UX_TRANSFER REQUEST *transfer request);
```

### Description

This function performs a USB transaction. On entry the transfer request gives the endpoint pipe selected for this transaction and the parameters associated with the transfer (data payload, length of transaction). For Control pipe, the transaction is blocking and will only return when the three phases of the control transfer have been completed or if there is a previous error. For other pipes, the USB stack will schedule the transaction on the USB but will not wait for its completion. Each transfer request for non-blocking pipes has to specify a completion routine handler.

When the function call returns, the status of the transfer request should be examined as it contains the result of the transaction.

### Input Parameter

- *transfer_request*: Pointer to the transfer request. The transfer request contains all the necessary information required for the transfer.

### Return Values

- **UX_SUCCESS** (0x00) The USB transfer for this transfer request was scheduled properly. The status code of the transfer request should be examined when the transfer request completes.
- **UX_MEMORY_INSUFFICIENT** (0x12) Not enough memory to allocate the necessary controller resources.
- **UX_TRANSFER_NOT_READY** (0x25) The device was in an invalid state – must be ATTACHED,ADDRESSED, or CONFIGURED.

### Example:

```c
UINT status;

/* Create a transfer request for the SET_CONFIGURATION request. No data for this request. */
transfer_request -> ux_transfer_request_requested_length = 0;
transfer_request -> ux_transfer_request_function = UX_SET_CONFIGURATION;
transfer_request -> ux_transfer_request_type =
    | UX_REQUEST_OUT           |
    | UX_REQUEST_TYPE_STANDARD |
    UX_REQUEST_TARGET_DEVICE;

transfer_request -> ux_transfer_request_value = (USHORT)
    configuration -> ux_configuration_descriptor.bConfigurationValue;
transfer_request -> ux_transfer_request_index = 0;

/* Send request to HCD layer. */
status = ux_host_stack_transfer_request(transfer_request);

/* If status equals UX_SUCCESS, the operation was successful. */
```

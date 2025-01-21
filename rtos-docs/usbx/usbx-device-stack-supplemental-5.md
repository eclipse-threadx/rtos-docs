---
title: Chapter 5 - USBX OTG
description: Learn how USBX supports the OTG functionalities of USB when an OTG compliant USB controller is available in the hardware design.
---
# Chapter 5 - USBX OTG

USBX supports the OTG functionalities of USB when an OTG compliant USB controller is available in the hardware design.

USBX supports OTG in the core USB stack. But for OTG to function, it requires a specific USB controller. USBX OTG controller functions can be found in the usbx_otg directory. The current USBX version only supports the NXP LPC3131 with full OTG capabilities.

The regular controller driver functions (host or device) can still be found in the standard USBX usbx_device_controllers and usbx_host_controllers but the usbx_otg directory contains the specific OTG functions associated with the USB controller.

There are four categories of functions for an OTG controller in addition to the usual host/device functions.

- [VBUS specific functions](#vbus_specific_functions)
- [Start and Stop the controller](#start_and_Stop_the_controller)
- [USB role manager](#usb_role_manager)
- [Interrupt handlers](#interrupt_handlers)

## vbus_specific_functions

Each controller needs to have a VBUS manager to change the state of VBUS based on power management requirements. Usually, this function only performs turning on or off VBUS.

## start_and_Stop_the_controller

Unlike a regular USB implementation, OTG requires the host and/or the device stack to be activated and deactivated when the role changes.

## usb_role_manager

The USB role manager receives commands to change the state of the USB. There are several states that need transitions to and from:

| State                    | Value | Description                                           |
| ------------------------ | ----- | ----------------------------------------------------- |
| UX_OTG_IDLE            | 0     | The device is Idle. Not connected to anything |
| UX_OTG_IDLE_TO_HOST  | 1     | Device is connected with type A connector             |
| UX_OTG_IDLE_TO_SLAVE | 2     | Device is connected with type B connector             |
| UX_OTG_HOST_TO_IDLE  | 3     | Host device got disconnected                          |
| UX_OTG_HOST_TO_SLAVE | 4     | Role swap from Host to Slave                          |
| UX_OTG_SLAVE_TO_IDLE | 5     | Slave device is disconnected                          |
| UX_OTG_SLAVE_TO_HOST | 6     | Role swap from Slave to Host                          |

## interrupt_handlers

Both host and device controller drivers for OTG needs different interrupt handlers to monitor signals beyond traditional USB interrupts, in particular signals due to SRP and VBUS.

How to initialize a USB OTG controller. We use the NXP LPC3131 as an example here.

```C
/* Initialize the LPC3131 OTG controller. */
status = ux_otg_lpc3131_initialize(0x19000000, lpc3131_vbus_function, demo_change_mode_callback);
```

In this example, we initialize the LPC3131 in OTG mode by passing a VBUS function and a callback for mode change (from host to slave or vice versa).

The callback function should simply record the new mode and wake up a pending thread to act up the new state.

```C
void demo_change_mode_callback(ULONG mode) 
{
    /* Simply save the otg mode. */
    otg_mode = mode;

    /* Wake up the thread that is waiting. */
    ux_utility_semaphore_put(&mode_change_semaphore);
}
```

The mode value that is passed can have the following values.

- **UX_OTG_MODE_IDLE**
- **UX_OTG_MODE_SLAVE**
- **UX_OTG_MODE_HOST**

The application can always check what the device is by looking at the variable:

```C
_ux_system_otg -> ux_system_otg_device_type
```

Its values can be any of the following.

- **UX_OTG_DEVICE_A**
- **UX_OTG_DEVICE_B**
- **UX_OTG_DEVICE_IDLE**

A USB OTG host device can always ask for a role swap by issuing the following command.

```C
/* Ask the stack to perform a HNP swap with the device. We relinquish the host role to A device. */
ux_host_stack_role_swap(storage -> ux_host_class_storage_device);
```

For a slave device, there is no command to issue but the slave device can set a state to change the role, which will be picked up by the host when it issues a **GET_STATUS** and the swap will then be initiated.

```C
/* We are a B device, ask for role swap.
   The next GET_STATUS from the host will get the status change and do the HNP. */
_ux_system_otg -> ux_system_otg_slave_role_swap_flag = UX_OTG_HOST_REQUEST_FLAG;
```

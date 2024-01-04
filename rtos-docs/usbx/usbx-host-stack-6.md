---
title: Chapter 6 - USBX CDC-ECM Class Usage
description: USBX contains a CDC-ECM class for the host and device side. This class is designed to be used with NetX Duo, specifically, the USBX CDC-ECM class acts as the driver for NetX Duo. This is why there are no CDC-ECM APIs listed in Chapter 5.
---
# Chapter 6 - USBX CDC-ECM Class Usage

USBX contains a CDC-ECM class for the host and device side. This class is designed to be used with NetX Duo, specifically, the USBX CDC-ECM class acts as the driver for NetX Duo. This is why there are no CDC-ECM APIs listed in Chapter 5.

Once NetX Duo and USBX are initialized and an instance of a CDC-ECM device is found by USBX, the application exclusively uses NetX Duo to communicate with the device. Initialization follows the pattern shown in the example below.

```c
UINT status;

/* The USB controller should be the last component initialized so that
everything is ready when data starts being received. */

/* Initialize USBX. */

ux_system_initialize(memory_pointer, UX_USBX_MEMORY_SIZE, UX_NULL, 0);

/* The code below is required for installing the host portion of USBX */
status = ux_host_stack_initialize(UX_NULL);

/* Register cdc_ecm class. */

status = ux_host_stack_class_register(_ux_system_host_class_cdc_ecm_name,
    ux_host_class_cdc_ecm_entry);

/* Perform the initialization of the network driver. */

_ux_network_driver_init();

/* Initialize NetX Duo. Refer to NetX Duo user guide for details to add initialization code. */

/* Register the platform-specific USB controller. */

status = ux_host_stack_hcd_register("controller_name", controller_entry, param1, param2);

/* Find the CDC-ECM class. */
class_cdc_ecm_get();

/* Now wait for the link to be up. */

while (cdc_ecm -> ux_host_class_cdc_ecm_link_state != UX_HOST_CLASS_CDC_ECM_LINK_STATE_UP)
    tx_thread_sleep(10);

/* At this point, everything has been initialized, and we've found a CDC-ECM device.
    Now NetX Duo can be used to communicate with the device. */
```

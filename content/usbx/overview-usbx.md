---
title: Understand USBX
description: USBX is a high-performance USB host, device, and on-the-go (OTG) embedded stack, USBX is fully integrated with ThreadX and available for all ThreadX–supported processors. 
---

USBX is a high-performance USB host, device, and on-the-go (OTG) embedded stack. USBX is fully integrated with ThreadX and available for all ThreadX–supported processors. Like ThreadX, USBX is designed to have a small footprint and high performance, making it ideal for deeply embedded applications that require an interface with USB devices.

## Host, Device, OTG & Extensive Class Support

USBX Host/Device embedded USB protocol stack is an Industrial Grade embedded USB solution designed specifically for deeply embedded, real-time, and IoT applications. USBX provides host, device, and OTG support, as well as extensive class support. USBX is fully integrated with ThreadX Real-Time Operating System, FileX embedded FAT-compatible file system, and NetX Duo embedded TCP/IP stack. All of this, combined with an extremely small footprint, fast execution and superior ease-of-use, make USBX the ideal choice for the most demanding embedded IoT applications requiring USB connectivity.

### USBX memory footprint

USBX has a remarkably small minimal footprint of 10.5 KB of FLASH and 5.1 KB RAM for USBX Device CDC/ACM support. USBX Host requires a minimum of 18 KB of FLASH and 25 KB of RAM for CDC/ACM support.

An additional 10 KB to 13 KB of instruction area memory is needed for TCP functionality. USBX RAM usage typically ranges from 2.6 KB to 3.6 KB plus the packet pool memory, which is defined by the application.

Like ThreadX, the size of USBX automatically scales based on the services actually used by the application. This virtually eliminates the need for complicated configuration and build parameters, making things easier for the developer.

### USB Interoperability verification

USBX Device Stack has been rigorously tested with the USB IF standard testing tool USBCV to ensure full compliance with the USB specifications and interoperability with different host systems.
In addition, USBX OTG stack has been verified and certified by the independent test lab Allion in Taiwan.

### USB Host controller support

USBX supports major USB standards like OHCI and EHCI. In addition, USBX supports proprietary discrete USB host controllers from Atmel, Microchip, Philips, Renesas, ST, TI, and other vendors. USBX also supports multiple host controllers in the same application.
USB Device controller support
USBX supports popular USB device controllers from Analog Devices, Atmel, Microchip, NXP, Philips, Renesas, ST, TI, and other vendors.

### Extensive Host Class support

USBX Host provides support for most popular classes, including ASIX, AUDIO, CDC/ACM, CDC/ECM, GSER, HID (keyboard, mouse, and remote control), HUB, PIMA (PTP/MTP), PRINTER, PROLIFIC, and STORAGE.

### Extensive USB Device Class support

USBX Device provides support for most popular classes, including CDC/ACM, CDC/ECM, DFU, HID, PIMA (PTP/MTP) (w/MTP), RNDIS, and STORAGE. Support for custom classes is also available.

### Pictbridge support

USBX supports the full Pictbridge implementation both on the host and the device. Pictbridge sits on top of USBX PIMA (PTP/MTP) class on both sides. The PictBridge standard allows the connection of a digital still camera or a smart phone directly to a printer without a PC, enabling direct printing to certain Pictbridge aware printers. When a camera or phone is connected to a printer, the printer is the USB host and the camera is the USB device. However, with Pictbridge, the camera will appear as being the host and commands are driven from the camera. The camera is the storage server, the printer the storage client. The camera is the print client and the printer is of course the print server. Pictbridge uses USB as a transport layer but relies on PTP (Picture Transfer Protocol) for the communication protocol.

### Custom class support

USBX Host and Device support custom classes. An example custom class is provided in the USBX distribution. This simple data pump class is called DPUMP and can be used as a model for custom application classes.
Advanced technology
USBX Host and Device support custom classes. An example custom class is provided in the USBX distribution. USBX is advanced technology that includes:

* Host, Device, and OTG support
* USB low, full, and high-speed support
* Automatic scaling
* Fully integrated with ThreadX, FileX, and NetX Duo
* Optional performance metrics
* TraceX system analysis support

## USBX APIs

### USBX Host API

The USBX Host API is an intuitive and consistent API, following a noun-verb naming convention. All APIs have leading ux_host_* to easily identify as USBX. Any blocking APIs have optional thread timeout.

* ASIX
    - Minimal 0.3 KB FLASH, 4 KB RAM
    - Automatic scalingSystem-level trace via TraceX
    - Intuitive USBX host APIs in this form: *ux_host_class_asix_**
* AUDIO
    - Minimal 1.2 KB FLASH, 4 KB RAM
    - Automatic scaling
    - Intuitive USBX host APIs in this form: *ux_host_class_audio_**
* CDC/ACM
    - Minimal 1.4 KB FLASH, 4 KB RAM
    - Automatic scaling
    - System-level trace via TraceX
    - Intuitive USBX host APIs in this form: *ux_host_class_cdc_acm_**
* HID
    - Minimal 0.3 KB FLASH, 4 KB RAM
    - Keyboard, mouse, and remote support
    - Automatic scaling
    - System-level trace via TraceX
    - Intuitive USBX host APIs in this form: *ux_host_class_hid_** 
* HUB
    - Minimal 1.7 KB FLASH, 2 KB RAM
    - Automatic scaling
    - System-level trace via TraceX
    - Intuitive USBX host APIs in this form: *ux_host_class_hub_**
* PIMA (PTP/MTP)
    - Minimal 0.9 KB FLASH, 8 KB RAM
    - Automatic scaling
    - System-level trace via TraceX
    - Intuitive USBX host APIs in this form: *ux_host_class_pima_**
* PRINTER
    - Minimal 0.8 KB FLASH, 8 KB RAM
    - Automatic scaling
    -  System-level trace via TraceX
    -  Intuitive USBX host APIs in this form: *ux_host_class_printer_**
* PROLIFIC
    - Minimal 1.5 KB FLASH, 4 KB RAM
    - Automatic scaling
    - System-level trace via TraceX
    - Intuitive USBX host APIs in this form: *ux_host_class_prolific_**
* STORAGE
    - Minimal 5.6 KB FLASH, 4 KB RAM
    - Automatic scaling<br> Integrated with FileX
    - System-level trace via TraceX
    - Intuitive USBX host APIs in this form: *ux_host_class_storage_**
* USB Host STACK
    - Supports many host controllers
    - Minimal 18 KB FLASH, 25 KB RAM
    - Automatic scaling
    - Support for multiple host controllers on same platform
    -  USB low, full, and high-speed support
    -  System-level trace via TraceX
    -  Intuitive USBX host APIs in this form: *ux_host_stack_* * 
* OHCI, EHCI, PROPRIETARY Host CONTROLLERS 

### USBX Device API

The USBX Device API is an intuitive and consistent API following a noun-verb naming convention. All APIs have leading ux_device_* to easily identify as USBX. Blocking APIs have optional thread timeout. Please see [USBX Host User Guide](usbx-host-stack-about) for more details.

* CDC/ACM
    - Minimal 0.8 KB FLASH, 2 KB RAM
    - Automatic scaling
    - System-level trace via TraceX
    - Intuitive USBX device APIs in this form: *ux_device_class_cdc_acm_**.
* CDC/ECM
    - Minimal 1.5 KB FLASH, 4 KB to 8 KB RAM
    - Automatic scaling
    - System-level trace via TraceX<br> Intuitive USBX device APIs in this form: *ux_device_class_cdc_ecm_**.
* DFU
    - Minimal 1.1 KB FLASH, 2 KB RAM
    -  Automatic scaling
    -  System-level trace via TraceX
    - Intuitive USBX device APIs in this form: *ux_device_class_dfu_** 
* GSER
    - Minimal 0.6 KB FLASH, 4 KB RAM
    - Automatic scaling
    - System-level trace via TraceX
    - Intuitive USBX device APIs in this form: *ux_device_class_gser_**
* HID
    - Minimal 0.9 KB FLASH, 2 KB RAM
    - Automatic scaling
    - System-level trace via TraceX
    - Intuitive USBX device APIs in this form: *ux_device_class_hid_** 
PIMA (PTP/MTP)
    - Minimal 5.2 KB FLASH, 8 KB RAM
    - Automatic scaling
    - System-level trace via TraceX
    - Intuitive USBX device APIs in this form: *ux_device_class_pima_** 
* STORAGE
    - Minimal 2.3 KB FLASH, 4 KB RAM
    - Automatic scaling
    - System-level trace via TraceX
    - Intuitive USBX device APIs in this form: *ux_device_class_storage_**
* RNDIS
    - Minimal 2.3 KB FLASH, 4 KB to 8 KB RAM
    - Automatic scaling
    - Integrated with NetX Duo
    - System-level trace via TraceX
    - Intuitive USBX device APIs in this form: *ux_device_class_rndls_**
* USBX Device STACK
    - Minimal 2.3 KB FLASH, 4 KB RAM
    - Automatic scaling
    - System-level trace via TraceX
    - Intuitive USBX device APIs in this form: *ux_device_class_storage_**
* PROPRIETARY Host CONTROLLERS

## Next steps

Start working with the USBX Host and Device Stack by following our [Host Stack User Guide](usbx-host-stack-about.md) or [Device Stack User Guide](usbx-device-stack-about).
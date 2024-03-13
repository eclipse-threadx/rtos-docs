---
title: Chapter 2 - USBX Host Stack Installation
description: Learn how to install the USBX host stack.
---
# Chapter 2 - USBX Host Stack Installation

## Host Considerations

### Computer Type

Embedded development is usually performed on Windows PC or Unix host computers. After the application is compiled, linked, and located on the host, it is downloaded to the target hardware for execution.

### Download Interfaces

Usually, the target download is done over an RS-232 serial interface, although parallel interfaces, USB, and Ethernet are becoming more popular. See the development tool documentation for available options.

### Debugging Tools

Debugging is done typically over the same link as the program image download. A variety of debuggers exist, ranging from small monitor programs running on the target through Background Debug Monitor (BDM) and In-Circuit Emulator (ICE) tools. The ICE tool provides the most robust debugging of actual target hardware.

### Required Hard Disk Space

The source code for USBX is delivered in ASCII format and requires approximately 500 KBytes of space on the host computer's hard disk.

## Target Considerations

USBX requires between 24 KBytes and 64 KBytes of Read Only Memory (ROM) on the target in host mode. The amount of memory required is dependent on the type of controller used and the USB classes linked to USBX. Another 32 KBytes of the target's Random Access Memory (RAM) are required for USBX global data structures and memory pool. This memory pool can also be adjusted depending on the expected number of devices on the USB and the type of USB controller. The USBX device side requires roughly 10-12 K of ROM depending on the type of device controller. The RAM memory usage depends on the type of class emulated by the device.

USBX also relies on ThreadX semaphores, mutexes, and threads for multiple thread protection, and I/O suspension and periodic processing for monitoring the USB bus topology.

### Product Distribution

USBX can be obtained from our public source code repository at <https://github.com/eclipse-threadx/usbx/>.

The following is a list of several important files in the repository:

- ***ux_api.h***: This C header file contains all system equates, data structures, and service prototypes.
- ***ux_port.h***: This C header file contains all development-tool-specific data definitions and structures.
- ***ux.lib***: This is the binary version of the USBX C library. It is distributed with the standard package.
- ***demo_usbx.c***: The C file containing a simple USBX demo

All filenames are in lower-case. This naming convention makes it easier to convert the commands to Unix development platforms.

## USBX Installation

USBX is installed by cloning the GitHub repository to your local machine. The following is typical syntax for creating a clone of the USBX repository on your PC:

```powershell
    git clone https://github.com/eclipse-threadx/usbx
```

Alternatively you can download a copy of the repository using the download button on the GitHub main page.

You will also find instructions for building the USBX library on the front page of the online repository.

The following general instructions apply to virtually any installation:

1. Use the same directory in which you previously installed ThreadX on the host hard drive. All USBX names are unique and will not interfere with the previous USBX installation.
2. Add a call to ***ux_system_initialize*** at or near the beginning of ***tx_application_define*.** This is where the USBX resources are initialized.
3. Add a call to ***ux_host_stack_initialize*.**
4. Add one or more calls to initialize the required USBX.
5. Add one or more calls to initialize the host controllers available in the system.
6. It may be required to modify the tx_low_level_initialize.c file to add low-level hardware initialization and interrupt vector routing. This is specific to the hardware platform and will not be discussed here.
7. Compile application source code and link with the USBX and ThreadX run time libraries (FileX and/or Netx Duo may also be required if the USB storage class and/or USB network classes are to be compiled in), ux.a (or ux.lib) and tx.a (or tx.lib). The resulting can be downloaded to the target and executed.

## Configuration Options

There are several configuration options for building the USBX library. All options are located in the ***ux_user.h***.

The list below details each configuration option.


- **UX_PERIODIC_RATE**: This value represents how many ticks per seconds for a specific hardware platform. The default is 1000 indicating one tick per millisecond.
- **UX_MAX_CLASS_DRIVER**: This value is the maximum number of classes that can be loaded by USBX. This value represents the class container and not the number of instances of a class. For instance, if a particular implementation of USBX needs the hub class, the printer class, and the storage class, then the UX_MAX_CLASS_DRIVER value can be set to 3 regardless of the number of devices that belong to these classes.
- **UX_MAX_HCD**: This value represents the number of different host controllers that are available in the system. For USB 1.1 support, this value will mostly be 1. For USB 2.0 support this value can be more than 1. This value represents the number of concurrent host controllers running at the same time. If for instance, there are two instances of OHCI running or one EHCI and one OHCI controllers running, the UX_MAX_HCD should be set to 2. 
- **UX_MAX_DEVICES**: This value represents the maximum number of devices that can be attached to the USB. Normally, the theoretical maximum number on a single USB is 127 devices. This value can be scaled down to conserve memory. It should be noted that this value represents the total number of devices regardless of the number of USB buses in the system.
- **UX_MAX_ED**: This value represents the maximum number of EDs in the controller pool. This number is assigned to one controller only. If multiple instances of controllers are present, this value is used by each individual controller.
- **UX_MAX_TD and UX_MAX_ISO_TD**: This value represents the maximum number of regular and isochronous TDs in the controller pool. This number is assigned to one controller only. If multiple instances of controllers are present, this value is used by each individual controller.
- **UX_THREAD_STACK_SIZE**: This value is the size of the stack in bytes for the USBX threads. It can be typically 1024 bytes or 2048 bytes depending on the processor used and the host controller.
- **UX_HOST_ENUM_THREAD_STACK_SIZE**: This value is the stack size of the USB host enumeration thread. If this symbol I not set, the USBX host enumeration thread stack size is set to **UX_THREAD_STACK_SIZE**. 
- **UX_HOST_HCD_THREAD_STACK_SIZE**: This value is the stack size of the USB host HCD thread. If this symbol I not set, the USBX host HCD thread stack size is set to **UX_THREAD_STACK_SIZE**.
- **UX_THREAD_PRIORITY_ENUM**: This is the ThreadX priority value for the USBX enumeration threads that monitors the bus topology.
- **UX_THREAD_PRIORITY_CLASS**: This is the ThreadX priority value for the standard USBX threads.
- **UX_THREAD_PRIORITY_KEYBOARD**: This is the ThreadX priority value for the USBX HID keyboard class.
- **UX_THREAD_PRIORITY_HCD**: This is the ThreadX priority value for the host controller thread.
- **UX_NO_TIME_SLICE**: This value actually defines the time slice that will be used for threads. For example, if defined to 0, the ThreadX target port does not use time slices.
- **UX_MAX_HOST_LUN**: This value represents the maximum number of SCSI logical units represented in the host storage class driver.
- **UX_HOST_CLASS_STORAGE_INCLUDE_LEGACY_PROTOCOL_SUPPORT**: If defined, this value includes code to handle storage devices that use the CB or CBI protocol (such as floppy disks). It is off by default because these protocols are obsolete, being superseded by the Bulk Only Transport (BOT protocol, which virtually all modern storage devices use.
- **UX_HOST_CLASS_HID_KEYBOARD_EVENTS_KEY_CHANGES_MODE**: If defined, this value causes ux_host_class_hid_keyboard_key_get to only report key changes that is, key presses and key releases. By default, it only reports when a key is down.
- **UX_HOST_CLASS_HID_KEYBOARD_EVENTS_KEY_CHANGES_MODE_REPORT_KEY_DOWN_ONLY**: Only used if **UX_HOST_CLASS_HID_KEYBOARD_EVENTS_KEY_CHANGES_MODE** is defined. If defined, causes ux_host_class_hid_keyboard_key_get to only report key pressed/down changes; key released/up changes are not reported.
- **UX_HOST_CLASS_HID_KEYBOARD_EVENTS_KEY_CHANGES_MODE_REPORT_LOCK_KEYS**: Only used if **UX_HOST_CLASS_HID_KEYBOARD_EVENTS_KEY_CHANGES_MODE** is defined. If defined, causes ux_host_class_hid_keyboard_key_get to report lock key (CapsLock/NumLock/ScrollLock) changes.
- **UX_HOST_CLASS_HID_KEYBOARD_EVENTS_KEY_CHANGES_MODE_REPORT_MODIFIER_KEYS**: Only used if **UX_HOST_CLASS_HID_KEYBOARD_EVENTS_KEY_CHANGES_MODE** is defined. If defined, causes ux_host_class_hid_keyboard_key_get to report modifier key (Ctrl/Alt/Shift/GUI) changes.
- **UX_HOST_CLASS_CDC_ECM_NX_PKPOOL_ENTRIES**: If defined, this value represents the number of packets in the CDC-ECM host class. The default is 16.

## Source Code Tree

The USBX files are provided in several directories.

![Source Code Tree](media/usbx-host-stack/source-code-tree.png)

In order to make the files recognizable by their names, the following convention has been adopted:

| File Suffix Name | File description                          |
| ---------------- | ----------------------------------------- |
| ux_host_stack    | usbx host stack core files                |
| ux_host_class    | usbx host stack classes files             |
| ux_hcd           | usbx host stack controller driver files   |
| ux_device_stack  | usbx device stack core files              |
| ux_device_class  | usbx device stack classes files           |
| ux_dcd           | usbx device stack controller driver files |
| ux_otg           | usbx otg controller driver related files  |
| ux_pictbridge    | usbx pictbridge files                     |
| ux_utility       | usbx utility functions                    |
| demo_usbx        | demonstration files for USBX              |

## Initialization of USBX resources

USBX has its own memory manager. The memory needs to be allocated to USBX before the host or device side of USBX is initialized. USBX memory manager can accommodate systems where memory can be cached.

The following function initializes USBX memory resources with 128K of regular memory and no separate pool for cache safe memory:

```c
/* Initialize USBX Memory */

ux_system_initialize(memory_pointer,(128*1024),UX_NULL,0);
```

The prototype for the ux_system_initialize is as follows.

```c
UINT ux_system_initialize( 
    VOID *regular_memory_pool_start,
    ULONG regular_memory_size,
    VOID *cache_safe_memory_pool_start,
    ULONG cache_safe_memory_size);
```

### Input parameters:

- **regular_memory_pool_start** Beginning of the regular memory pool.
- **regular_memory_size** Size of the regular memory pool.
- **cache_safe_memory_pool_start** Beginning of the cache safe memory pool.
- **cache_safe_memory_size** Size of the cache safe memory pool.    |

Not all systems require the definition of cache safe memory. In such a system, the values passed during the initialization for the memory pointer will be set to UX_NULL and the size of the pool to 0. USBX will then use the regular memory pool in lieu of the cache safe pool.

In a system where the regular memory is not cache safe and a controller requires to perform DMA memory (like OHCI, EHCI controllers amongst others) it is necessary to define a memory pool in a cache safe zone.

## Uninitialization of USBX resources

USBX can be terminated by releasing its resources. Prior to terminating usbx, all classes and controller resources need to be terminated properly. The following function uninitializes USBX memory resources :

```c
/* Unitialize USBX Resources */

ux_system_uninitialize();
```

The prototype for the ux_system_initialize is as follows.

```c
UINT ux_system_uninitialize(VOID);
```

## Definition of USB Host Controllers

It is required to define at least one USB host controller for USBX to operate in host mode. The application initialization file should contain this definition. The following line performs the definition of a generic host controller.

```c
ux_host_stack_hcd_register("ux_hcd_controller",
        ux_hcd_controller_initialize, 0xd0000, 0x0a);
```

The ux_host_stack_hcd_register has the following prototype.

```c
UINT ux_host_stack_hcd_register(
    UCHAR *hcd_name,
    UINT (*hcd_initialize_function)(struct UX_HCD_STRUCT *),
    ULONG hcd_param1, ULONG hcd_param2);
```

The ux_host_stack_hcd_register function has the following parameters.

- **hcd_name**: string of the controller name
- **hcd_initialize_function**: initialization function of the controller
- **hcd_param1**: usually the IO value or Memory used by the controller
- **hcd_param2**: usually the IRQ used by the controller

In our previous example:

- "ux_hcd_controller" is the name of the controller
- "ux_hcd_controller_initialize" is the initialization routine for the host controller
- 0xd0000 is the address at which the host controller registers are visible in memory
- and 0x0a is the IRQ used by the host controller.

Following is an example of the initialization of USBX in host mode with one host controller and several classes.

```c
UINT status;

/* Initialize USBX. */
ux_system_initialize(memory_ptr, (128*1024),0,0);

/* The code below is required for installing the USBX host stack. */
status = ux_host_stack_initialize(UX_NULL);

/* If status equals UX_SUCCESS, host stack has been initialized. */

/* Register all the host classes for this USBX implementation. */
status = ux_host_class_register("ux_host_class_hub", ux_host_class_hub_entry);

/* If status equals UX_SUCCESS, host class has been registered. */
status = ux_host_class_register("ux_host_class_storage", ux_host_class_storage_entry);

/* If status equals UX_SUCCESS, host class has been registered. */
status = ux_host_class_register("ux_host_class_printer", ux_host_class_printer_entry);

/* If status equals UX_SUCCESS, host class has been registered. */
status = ux_host_class_register("ux_host_class_audio", ux_host_class_audio_entry);

/* If status equals UX_SUCCESS, host class has been registered. */

/* Register all the USB host controllers available in this system. */ 
status = ux_host_stack_hcd_register("ux_hcd_controller", ux_hcd_controller_initialize, 0x300000, 0x0a);

/* If status equals UX_SUCCESS, USB host controllers have been registered. */
```

## Definition of Host Classes

It is required to define one or more host classes with USBX. A USB class is required to drive a USB device after the USB stack has configured the USB device. A USB class is specific to the device. One or more classes may be required to drive a USB device depending on the number of interfaces contained in the USB device descriptors.

This is an example of the registration of the HUB class.

```c
status = ux_host_stack_class_register("ux_host_class_hub", ux_host_class_hub_entry);
```

The function ux_host_class_register has the following prototype.

```c
UINT ux_host_stack_class_register(
    UCHAR *class_name, 
    UINT (*class_entry_address) (struct UX_HOST_CLASS_COMMAND_STRUCT *));
```

- **class_name** is the name of the class
- **class_entry_address** is the entry point of the class

In the example of the HUB class initialization:

- "ux_host_class_hub" is the name of the hub class
- ux_host_class_hub_entry is the entry point of the HUB class.

## Access to Host Class Function Instance

After class registration and host controller driver (HCD) registration, the USB devices connected to USB port can be detected and enumerated. During enumeration, registered class checks connected device information, if the right information is found memory is allocated for class function instance and the instance is then activated. The instance is used by application to do class function specific operations.

When USB device is removed from USB port, the activated class function instance is then deactivated.

There are two ways to access the host instances: from registered class and/or through host change callback notification.

### Get instance through registered class

When class function instance is activated, it's linked to the class who owns the instance. So through class the instance can be accessed. E.g., to check if `storage` instance is available:

```c
    /* Find the main storage container (class)  */
    status =  ux_host_stack_class_get(_ux_system_host_class_storage_name, &ux_class);
    if (status != UX_SUCCESS)
        return(status);

    /* Find the storage instance under container (class)  */
    status =  ux_host_stack_class_instance_get(ux_class, 0, (void **) &storage);
    if (status == UX_SUCCESS)
    {
        /* Check if storage state, storage specific resources are ready.  */
        if (storage -> ux_host_class_storage_state == UX_HOST_CLASS_INSTANCE_LIVE &&
            ux_class -> ux_host_class_ext != UX_NULL &&
            ux_class -> ux_host_class_media != UX_NULL)
            return(UX_SUCCESS);
    }
```
[`ux_host_stack_class_get`](usbx-host-stack-4.md#ux_host_stack_class_get) is used to get a registered class, given class name used on registration.

[`ux_host_stack_class_instance_get`](usbx-host-stack-4.md#ux_host_stack_class_instance_get) is used to get function instance from specific registered class.

In example, the state of the instance and some other function related resources is checked, to confirm the instance is ready to use.

### Get instance through host change callback notification

There is an optional host change callback function which is assigned on host stack initialization. If the function is available, it's invoked when function instance is activated and in good state with all required resources available. Inside the callback the instance is passed and inside application callback user can save it for further operations.

For more details, see [**ux_host_stack_initialize**](usbx-host-stack-4.md#ux_host_stack_initialize).

## Troubleshooting

USBX is delivered with a demonstration file and a simulation environment. It is always a good idea to get the demonstration platform running first—either on the target hardware or a specific demonstration platform.

If the demonstration system does not work, try the following things to narrow the problem.

## USBX Version ID

The current version of USBX is available both to the user and the application software during run-time. The programmer can obtain the USBX version from examination of the ***ux_port.h*** file. In addition, this file also contains a version history of the corresponding port. Application software can obtain the USBX version by examining the global string ***_ux_version_id***, which is defined in ***ux_port.h***.

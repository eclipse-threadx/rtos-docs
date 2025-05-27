---
title: Chapter 2 - USBX Device Stack Installation
description: Learn how to install the USBX device stack, as well as the important host considerations you need to think about before you install.
---
# Chapter 2 - USBX Device Stack Installation

## Host Considerations

### Computer Type

Embedded development is usually performed on Windows PC or Unix host computers. After the application is compiled, linked, and located on the host, it is downloaded to the target hardware for execution.

### Download Interfaces

Usually the target download is done over an RS-232 serial interface, although parallel interfaces, USB, and Ethernet are becoming more popular. See the development tool documentation for available options.

### Debugging Tools

Debugging is done typically over the same link as the program image download. A variety of debuggers exist, ranging from small monitor programs running on the target through Background Debug Monitor (BDM) and In-Circuit Emulator (ICE) tools. The ICE tool provides the most robust debugging of actual target hardware.

### Required Hard Disk Space

The source code for USBX is delivered in ASCII format and requires approximately 500 KBytes of space on the host computer's hard disk.

### Target Considerations

USBX requires between 24 KBytes and 64 KBytes of Read Only Memory (ROM) on the target in host mode. The amount of memory required is dependent on the type of controller used and the USB classes linked to USBX. Another 32 KBytes of the target's Random Access Memory (RAM) are required for USBX global data structures and memory pool. This memory pool can also be adjusted depending on the expected number of devices on the USB and the type of USB controller. The USBX device side requires roughly 10-12 K of ROM depending on the type of device controller. The RAM memory usage depends on the type of class emulated by the device.

USBX also relies on ThreadX semaphores, mutexes, and threads for multiple thread protection, and I/O suspension and periodic processing for monitoring the USB bus topology.

### Product Distribution

USBX can be obtained from our public source code repository at <https://github.com/eclipse-threadx/usbx/>.

The following is a list of several important files in the repository.

* ***ux_api.h***: This C header file contains all system equates, data structures, and service prototypes.
* ***ux_port.h***: This C header file contains all development-tool-specific data definitions and structures.
* ***demo_usbx.c***: The C file containing a simple USBX demo

All filenames are in lower-case. This naming convention makes it easier to convert the commands to Unix development platforms.

## USBX Installation

USBX is installed by cloning the GitHub repository to your local machine. The following is typical syntax for creating a clone of the USBX repository on your PC:

```c
    git clone https://github.com/eclipse-threadx/usbx
```

Alternatively you can download a copy of the repository using the download button on the GitHub main page.

You will also find instructions for building the USBX library on the front page of the online repository.

The following general instructions apply to virtually any installation:

with RTOS:

1. Use the same directory in which you previously installed ThreadX on the host hard drive. All USBX names are unique and will not interfere with the previous USBX installation.
2. Add a call to ***ux_system_initialize*** at or near the beginning of ***tx_application_define***. This is where the USBX resources are initialized.
3. Add a call to ***ux_device_stack_initialize*.**
4. Add one or more calls to initialize the required USBX classes (either host and/or devices classes)
5. Add one or more calls to initialize the device controller available in the system.
6. It may be required to modify the tx_low_level_initialize.c file to add low-level hardware initialization and interrupt vector routing. This is specific to the hardware platform and will not be discussed here.|
7. Compile application source code and link with the USBX and ThreadX run time libraries (FileX and/or NetX Duo may also be required if the USB storage class and/or USB network classes are to be compiled in), ux.a (or ux.lib) and tx.a (or tx.lib). The resulting can be downloaded to the target and executed!

with Standalone (without RTOS):
1. All USBX names are unique and will not interfere with the previous USBX installation.
2. Add a call to ***ux_system_initialize*** This is where the USBX resources are initialized.
3. Add a call to ***ux_device_stack_initialize*.**
4. Add one or more calls to initialize the required USBX classes (either host and/or devices classes)
5. Add one or more calls to initialize the device controller available in the system.
6. Add a call to ***ux_system_tasks_run*.** in main function while(1).

## Configuration Options

There are several configuration options for building the USBX library. All options are located in the ***ux_user.h***.

The list below details each configuration option.

|          Configuration Option                   | Description |
| ----------------------------------------------- | ----------- |
| **UX_PERIODIC_RATE**                            | This value represents how many ticks per seconds for a specific hardware platform. The default is 1000 indicating 1 tick per millisecond. |
| **UX_THREAD_STACK_SIZE**                        | This value is the size of the stack in bytes for the USBX threads. It can be typically 1024 bytes or 2048 bytes depending on the processor used and the host controller. |
| **UX_THREAD_PRIORITY_CLASS**                    | This is the ThreadX priority value for the standard USBX threads. |
| **UX_THREAD_PRIORITY_DCD**                      | This is the ThreadX priority value for the device controller thread. |
| **UX_ALIGN_MIN**                                | This value represents minimal allocated memory alignment in number of bytes. The default is UX_ALIGN_8 (0x07) to align allocated memory to 8 bytes. |
| **UX_ENFORCE_SAFE_ALIGNMENT**                   | This value forces the memory allocation scheme to enforce alignment of memory with the UX_SAFE_ALIGN field. |
| **UX_NO_TIME_SLICE**                            | This value actually defines the time slice that will be used for threads. For example, if defined to 0, the ThreadX target port does not use time slices. |
| **UX_MAX_SLAVE_CLASS_DRIVER**                   | This value is the maximum number of classes in the device stack that can be loaded by USBX. |
| **UX_MAX_SLAVE_INTERFACES**                     | This value is the maximum number of interfaces in the device framework. |
| **UX_DEVICE_ENDPOINT_BUFFER_OWNER**             | This value represents the endpoint buffer owner. 0 The default, endpoint buffer is managed by core stack. Each endpoint takes UX_SLAVE_REQUEST_DATA_MAX_LENGTH bytes, 1 Endpoint buffer managed by classes. In this case not all endpoints consume UX_SLAVE_REQUEST_DATA_MAX_LENGTH bytes |
| **UX_SLAVE_REQUEST_CONTROL_MAX_LENGTH**         | This value represents the maximum number of bytes received on a control endpoint in the device stack. The default is 256 bytes but can be reduced in memory constrained environments. |
| **UX_SLAVE_REQUEST_DATA_MAX_LENGTH**            | This value represents the maximum number of bytes that can be received or transmitted on any endpoint. This value cannot be less than the maximum packet size of any endpoint. The default is 4096 bytes but can be reduced in memory constrained environments. For cd-rom support in the storage  class, this value cannot be less than 2048. |
| **UX_DEVICE_BIDIRECTIONAL_ENDPOINT_SUPPORT**    | If defined, the device side enables bi-directional-endpoints support, e.g., endpoints addressed 0x01 and 0x81. Otherwise (the default case), endpoint number must be unique in same configuration. Note the feature must be used with compatible DCD and peripheral. |
| **UX_DEVICE_ALTERNATE_SETTING_SUPPORT_DISABLE** | This macro disables interface alternate setting support. |
| **UX_DEVICE_INITIALIZE_FRAMEWORK_SCAN_DISABLE** | This macro disables device framework scan, where max number of endpoints (except EP0) and max number of interfaces are calculated at runtime, as a base to allocate memory for interfaces and endpoints structures and their buffers. Undefined, the following two macros must be defined to initialize memory structures. |
| **UX_MAX_DEVICE_ENDPOINTS**                     | This value represents max number of endpoints (except EP0) activated at the same time. Works if UX_DEVICE_INITIALIZE_FRAMEWORK_SCAN_DISABLE is defined. |
| **UX_MAX_DEVICE_INTERFACES**                    | This value represents max number of interfaces activated at the same time. Works if UX_DEVICE_INITIALIZE_FRAMEWORK_SCAN_DISABLE is defined. |
| **UX_NAME_REFERENCED_BY_POINTER**               | Defined, the _name in structs are referenced by pointer instead of by contents. By default the _name is an array of string that saves characters, the contents are compared to confirm match. If referenced by pointer the address pointer to const string is saved, the pointers are compared to confirm match. |
| **UX_DEVICE_SIDE_ONLY**                         | This value will only enable the device side of USBX. |
| **UX_STANDALONE**                               | This macro will enable the standalone mode of USBX. |
| **UX_DEBUG_LOG_SIZE**                           | This value represents the size of the log pool. |
| **UX_ENABLE_ASSERT**                            | This enables the assert checks inside USBX. |
| **UX_ENABLE_ERROR_CHECKING**                    | This option enables the basic USBX error checking. This define is typically used when the application is debugging and removed after the application is fully debugged. |

## Source Code Tree

The USBX files are provided in several directories.

![Source Code Tree](./media/usbx-device-stack/source-code-tree.png)

In order to make the files recognizable by their names, the following convention has been adopted:

| File Suffix Name  | File description                          |
| ----------------- | ----------------------------------------- |
| ux_host_stack     | usbx host stack core files                |
| ux_host_class     | usbx host stack classes files             |
| ux_hcd            | usbx host stack controller driver files   |
| ux_device_stack   | usbx device stack core files              |
| ux_device_class   | usbx device stack classes files           |
| ux_dcd            | usbx device stack controller driver files |
| ux_otg            | usbx otg controller driver related files  |
| ux_pictbridge     | usbx pictbridge files                     |
| ux_utility        | usbx utility functions                    |
| demo_usbx         | demonstration files for USBX              |

## Initialization of USBX resources

USBX has its own memory manager. The memory needs to be allocated to USBX before the host or device side of USBX is initialized. USBX memory manager can accommodate systems where memory can be cached.

The following function initializes USBX memory resources with 128 K of regular memory and no separate pool for cache safe memory:

```c
/* Initialize USBX Memory */
ux_system_initialize(memory_pointer, (128*1024), UX_NULL, 0);
```

The prototype for the ux_system_initialize is as follows:

```c
UINT ux_system_initialize(VOID *regular_memory_pool_start,
                          ULONG regular_memory_size,
                          VOID *cache_safe_memory_pool_start,
                          ULONG cache_safe_memory_size);
```

Where:

- *regular_memory_pool_start*: Beginning of the regular memory pool.
- *regular_memory_size*: Size of the regular memory pool.
- *cache_safe_memory_pool_start*: Beginning of the cache safe memory pool.
- *cache_safe_memory_size*: Size of the cache safe memory pool.

Not all systems require the definition of cache safe memory. In such a system, the values passed during the initialization for the memory pointer will be set to UX_NULL and the size of the pool to 0. USBX will then use the regular memory pool in lieu of the cache safe pool.

In a system where the regular memory is not cache safe and a controller requires to perform DMA memory it is necessary to define a memory pool in a cache safe zone.

## Uninitialization of USBX resources

USBX can be terminated by releasing its resources. Prior to terminating usbx, all classes and controller resources need to be terminated properly. The following function uninitializes USBX memory resources:

```c
/* Uninitialize USBX Resources */
ux_system_uninitialize();
```

The prototype for the ux_system_initialize is as follows:

```c
UINT ux_system_uninitialize(VOID);
```

## Task Run of USBX resources

This function runs USB system tasks, including possible tasks for host, device and OTG.

```c

    while(1)
    {
#if defined(UX_DEVICE_STANDALONE)
        ux_system_tasks_run();
#endif
    }

```

The prototype for the ux_system_tasks_run is as follows:

```c
UINT ux_system_tasks_run(VOID);
```

## Definition of USB Device Controller

Only one USB device controller can be defined at any time to operate in device mode. The application initialization file should contain this definition. The following line performs the definition of a generic usb controller:

```c
ux_dcd_controller_initialize(0x7BB00000, 0, 0xB7A00000);
```

The USB device initialization has the following prototype:

```c
UINT ux_dcd_controller_initialize(ULONG dcd_io, ULONG dcd_irq, ULONG dcd_vbus_address);
```

Where:

- *dcd_io*: Address of the controller IO.
- *dcd_irq*: Interrupt used by the controller.
- *dcd_vbus_address*: Address of the VBUS GPIO .

The following example is the initialization of USBX in device mode with the storage device class and a generic controller:

```c
/* Initialize USBX Memory */

ux_system_initialize(memory_pointer,(128*1024), 0, 0);

/* The code below is required for installing the device portion of USBX */
status = ux_device_stack_initialize(&device_framework_high_speed, DEVICE_FRAMEWORK_LENGTH_HIGH_SPEED,
                                    &device_framework_full_speed, DEVICE_FRAMEWORK_LENGTH_FULL_SPEED,
                                    &string_framework, STRING_FRAMEWORK_LENGTH,
                                    &language_id_framework, LANGUAGE_ID_FRAMEWORK_LENGTH,
                                    UX_NULL);

/* If status equals UX_SUCCESS, installation was successful. */

/* Store the number of LUN in this device storage instance: single LUN. */
storage_parameter.ux_slave_class_storage_parameter_number_lun = 1;

/* Initialize the storage class parameters for reading/writing to the Flash Disk. */
storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_last_lba = 0x1e6bfe;
storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_block_length = 512;
storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_type = 0;
storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_removable_flag = 0x80;
storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_read = demo_thread_flash_media_read;
storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_write = demo_thread_flash_media_write;
storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_status = demo_thread_flash_media_status;

/* Initialize the device storage class. The class is connected with interface 0 */
status = ux_device_stack_class_register(ux_system_slave_class_storage_name ux_device_class_storage_entry, ux_device_class_storage_thread, 0, (VOID *)&storage_parameter);

/* Register the device controllers available in this system */
status = ux_dcd_controller_initialize(0x7BB00000, 0, 0xB7A00000);

/* If status equals UX_SUCCESS, registration was successful. */
```

## Troubleshooting

USBX is delivered with a demonstration file and a simulation environment. It is always a good idea to get the demonstration platform running first—either on the target hardware or a specific demonstration platform.

## USBX Version ID

The current version of USBX is available both to the user and the application software during run-time. The programmer can obtain the USBX version from examination of the ***ux_port.h*** file. In addition, this file also contains a version history of the corresponding port. Application software can obtain the USBX version by examining the global string ***_ux_version_id***, which is defined in ***ux_port.h***.

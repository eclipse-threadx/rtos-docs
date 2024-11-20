---
title: Chapter 5 - USBX Device Class Considerations
description: Learn about the USBX Device Class considerations.
---

## Device Class registration

Each device class follows the same principle for registration. A structure containing specific class parameters is passed to the class initialize function.

```c
/* Set the parameters for callback when insertion/extraction of a HID device. */

hid_parameter.ux_slave_class_hid_instance_activate = tx_demo_hid_instance_activate;

hid_parameter.ux_slave_class_hid_instance_deactivate = tx_demo_hid_instance_deactivate;

/* Initialize the hid class parameters for the device. */
hid_parameter.ux_device_class_hid_parameter_report_address = hid_device_report;

hid_parameter.ux_device_class_hid_parameter_report_length = HID_DEVICE_REPORT_LENGTH;

hid_parameter.ux_device_class_hid_parameter_report_id = UX_TRUE;
hid_parameter.ux_device_class_hid_parameter_callback = demo_thread_hid_callback;

/* Initialize the device hid class. The class is connected with interface 0 */

status = ux_device_stack_class_register(_ux_system_slave_class_hid_name,
    ux_device_class_hid_entry,1,0, (VOID *)&hid_parameter);
```

Each class can register, optionally, a callback function when a instance of the class gets activated. The callback is then called by the device stack to inform the application that an instance was created.

The application would have in its body the 2 functions for activation and deactivation, as shown in the following example.

```c
VOID tx_demo_hid_instance_activate(VOID *hid_instance)
{
    /* Save the HID instance. */
    hid_slave = (UX_SLAVE_CLASS_HID *) hid_instance;
}

VOID tx_demo_hid_instance_deactivate(VOID *hid_instance)
{
    /* Reset the HID instance. */
    hid_slave = UX_NULL;
}
```

It is not recommended to do anything within these functions but to memorize the instance of the class and synchronize with the rest of the application.

## General Considerations for Bulk Transfer

As per the USB 2.0 specification, an endpoint must always transmit data payloads with a data field less than or equal to the endpoint's
reported wMaxPacketSize value. The size of a data packet is limited to bMaxPacketSize. Transfer can be completed with the following cases
1. The endpoint has transferred exactly the amount of data expected
2. When a device or host endpoint receives a packet with size less than max packet size(wMaxPacketSize). This short packet indicates that there is no more data packet left and transfer is complete or when the data packets to be transmitted are all equal to wMaxPacketSize, then the end of transfer cannot be determined. In order for transfer completion a Zero Length Packet(ZLP) must be sent
Short packets and Zero Length Packets signify the end of a bulk data transfer. 
The above considerations apply to raw bulk data transfer APIs, e.g., ux_device_class_cdc_acm_read().

## USB Device Storage Class

The USB device storage class allows for a storage device embedded in the system to be made visible to a USB host.

The USB device storage class does not by itself provide a storage solution. It merely accepts and interprets SCSI requests coming from the host. When one of these requests is a read or a write command, it will invoke a pre-defined call back to a real storage device handler, such as an ATA device driver or a Flash device driver.

When initializing the device storage class, a pointer structure is given to the class that contains all the information necessary. An example is given below.

```c
/* Initialize the storage class parameters to customize vendor strings. */

storage_parameter.ux_slave_class_storage_parameter_vendor_id
    = demo_ux_system_slave_class_storage_vendor_id;

storage_parameter.ux_slave_class_storage_parameter_product_id
    = demo_ux_system_slave_class_storage_product_id;

storage_parameter.ux_slave_class_storage_parameter_product_rev
    = demo_ux_system_slave_class_storage_product_rev;

storage_parameter.ux_slave_class_storage_parameter_product_serial
    = demo_ux_system_slave_class_storage_product_serial;

/* Store the number of LUN in this device storage instance: single LUN. */
storage_parameter.ux_slave_class_storage_parameter_number_lun = 1;

/* Initialize the storage class parameters for reading/writing to the Flash Disk. */

storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_last_lba = 0x1e6bfe;

storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_block_length = 512;

storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_type = 0;

storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_removable_flag = 0x80;

storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_read_only_flag
    = UX_FALSE;

storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_read
    = tx_demo_thread_flash_media_read;

storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_write
    = tx_demo_thread_flash_media_write;

storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_status
    = tx_demo_thread_flash_media_status;

/* A simple write caching support.
   If this callback is assigned, host gets notification from caching page
   that write caching is supported, and can send SYNCHRONIZE_CACHE to
   flush cache. But caching options are not allowed to be changed.  */
storage_parameter.ux_slave_class_storage_parameter_lun[0].
ux_slave_class_storage_media_flush  =
   tx_demo_thread_flash_media_flush;

/* Initialize the device storage class. The class is connected with interface 0 */
status = ux_device_stack_class_register(_ux_system_slave_class_storage_name,
    ux_device_class_storage_entry, ux_device_class_storage_thread, 0, (VOID *)&storage_parameter);
```

In this example, the driver storage strings are customized by assigning string pointers to corresponding parameter. If any one of the string pointer is left to UX_NULL, the default ThreadX string is used.

In this example, the drive's last block address or LBA is given as well as the logical sector size. The LBA is the number of sectors available in the media –1. The block length is set to 512 in regular storage media. It can be set to 2048 for optical drives.

The application needs to pass three callback function pointers to allow the storage class to read, write and obtain status for the media.

The prototypes for the read and write functions are shown in the example below.

```c
UINT media_read( 
    VOID *storage,  
    ULONG lun,  
    UCHAR *data_pointer,
    ULONG number_blocks,  
    ULONG lba,  
    ULONG *media_status);

UINT media_write( 
    VOID *storage,  
    ULONG lun,  
    UCHAR *data_pointer,
    ULONG number_blocks,  
    ULONG lba,  
    ULONG *media_status);
```

Where:

- *storage* is the instance of the storage class.
- *lun* is the LUN the command is directed to.
- *data_pointer* is the address of the buffer to be used for reading or writing.
- *number_blocks* is the number of sectors to read/write.
- *lba* is the sector address to read.
- *media_status* should be filled out exactly like the media status callback return value.

The return value can have either the value UX_SUCCESS or UX_ERROR indicating a successful or unsuccessful operation. These operations do not need to return any other error codes. If there is an error in any operation, the storage class will invoke the status call back function.

This function has the following prototype.

```c
ULONG media_status( 
    VOID *storage,  
    ULONG lun,  
    ULONG media_id,  
    ULONG *media_status);
```

The calling parameter media_id is not currently used and should always be 0. In the future it may be used to distinguish multiple storage devices or storage devices with multiple SCSI LUNs. This version of the storage class does not support multiple instances of the storage class or storage devices with multiple SCSI LUNs.

The return value is a SCSI error code that can have the following format.

- **Bits 0-7** Sense_key
- **Bits 8-15** Additional Sense Code
- **Bits 16-23** Additional Sense Code Qualifier

The following table provides the possible Sense/ASC/ASCQ combinations.

| Sense Key | ASC | ASCQ |                    Description                    |
| --------- | --- | ---- | ------------------------------------------------- |
| 00        | 00  | 00   | NO SENSE                                          |
| 01        | 17  | 01   | RECOVERED DATA WITH RETRIES                       |
| 01        | 18  | 00   | RECOVERED DATA WITH ECC                           |
| 02        | 04  | 01   | LOGICAL DRIVE NOT READY - BECOMING READY          |
| 02        | 04  | 02   | LOGICAL DRIVE NOT READY - INITIALIZATION REQUIRED |
| 02        | 04  | 04   | LOGICAL UNIT NOT READY - FORMAT IN PROGRESS       |
| 02        | 04  | FF   | LOGICAL DRIVE NOT READY - DEVICE IS BUSY          |
| 02        | 06  | 00   | NO REFERENCE POSITION FOUND                       |
| 02        | 08  | 00   | LOGICAL UNIT COMMUNICATION FAILURE                |
| 02        | 08  | 01   | LOGICAL UNIT COMMUNICATION TIME-OUT               |
| 02        | 08  | 80   | LOGICAL UNIT COMMUNICATION OVERRUN                |
| 02        | 3A  | 00   | MEDIUM NOT PRESENT                                |
| 02        | 54  | 00   | USB TO HOST SYSTEM INTERFACE FAILURE              |
| 02        | 80  | 00   | INSUFFICIENT RESOURCES                            |
| 02        | FF  | FF   | UNKNOWN ERROR                                     |
| 03        | 02  | 00   | NO SEEK COMPLETE                                  |
| 03        | 03  | 00   | WRITE FAULT                                       |
| 03        | 10  | 00   | ID CRC ERROR                                      |
| 03        | 11  | 00   | UNRECOVERED READ ERROR                            |
| 03        | 12  | 00   | ADDRESS MARK NOT FOUND FOR ID FIELD               |
| 03        | 13  | 00   | ADDRESS MARK NOT FOUND FOR DATA FIELD             |
| 03        | 14  | 00   | RECORDED ENTITY NOT FOUND                         |
| 03        | 30  | 01   | CANNOT READ MEDIUM - UNKNOWN FORMAT               |
| 03        | 31  | 01   | FORMAT COMMAND FAILED                             |
| 04        | 40  | NN   | DIAGNOSTIC FAILURE ON COMPONENT NN (80H-FFH)      |
| 05        | 1A  | 00   | PARAMETER LIST LENGTH ERROR                       |
| 05        | 20  | 00   | INVALID COMMAND OPERATION CODE                    |
| 05        | 21  | 00   | LOGICAL BLOCK ADDRESS OUT OF RANGE                |
| 05        | 24  | 00   | INVALID FIELD IN COMMAND PACKET                   |
| 05        | 25  | 00   | LOGICAL UNIT NOT SUPPORTED                        |
| 05        | 26  | 00   | INVALID FIELD IN PARAMETER LIST                   |
| 05        | 26  | 01   | PARAMETER NOT SUPPORTED                           |
| 05        | 26  | 02   | PARAMETER VALUE INVALID                           |
| 05        | 39  | 00   | SAVING PARAMETERS NOT SUPPORT                     |
| 06        | 28  | 00   | NOT READY TO READY TRANSITION – MEDIA CHANGED     |
| 06        | 29  | 00   | POWER ON RESET OR BUS DEVICE RESET OCCURRED       |
| 06        | 2F  | 00   | COMMANDS CLEARED BY ANOTHER INITIATOR             |
| 07        | 27  | 00   | WRITE PROTECTED MEDIA                             |
| 0B        | 4E  | 00   | OVERLAPPED COMMAND ATTEMPTED                      |

There are two additional, optional callbacks the application may implement; one is for responding to a **GET_STATUS_NOTIFICATION** command and the other is for responding to the **SYNCHRONIZE_CACHE** command.

If the application would like to handle the **GET_STATUS_NOTIFICATION** command from the host, it should implement a callback with the following prototype.

```c
UINT ux_slave_class_storage_media_notification( 
    VOID *storage,  
    ULONG lun,
    ULONG media_id,  
    ULONG notification_class,
    UCHAR **media_notification,  
    ULONG *media_notification_length);
```

Where:

- *storage* is the instance of the storage class.
- *media_id* is not currently used. notification_class specifies the class of notification.
- *media_notification* should be set by the application to the buffer containing the response for the notification.
- *media_notification_length* should be set by the application to contain the length of the response buffer.

The return value indicates whether or not the command succeeded – should be either **UX_SUCCESS** or **UX_ERROR**.

If the application does not implement this callback, then upon receiving the **GET_STATUS_NOTIFICATION** command, USBX will notify the host that the command is not implemented.

The **SYNCHRONIZE_CACHE** command should be handled if the application is utilizing a cache for writes from the host. A host may send this command if it knows the storage device is about to be disconnected, for example, in Windows, if you right click a flash drive icon in the toolbar and select "Eject \[storage device name\]", Windows will issue the **SYNCHRONIZE_CACHE** command to that device.

If the application would like to handle the **SYNCHRONIZE_CACHE** command from the host, it should implement a callback with the following prototype.

```c
UINT ux_slave_class_storage_media_flush(
    VOID *storage, 
    ULONG lun,
    ULONG number_blocks, 
    ULONG lba, 
    ULONG *media_status);
```

Where:

- *storage* is the instance of the storage class.
- *lun* parameter specifies which LUN the command is directed to.
- *number_blocks* specifies the number of blocks to synchronize.
- *lba* is the sector address of the first block to synchronize.
- *media_status* should be filled out exactly like the media status callback return value.

The return value indicates whether or not the command succeeded – should be either **UX_SUCCESS** or **UX_ERROR**.

### Multiple SCSI LUN

The USBX device storage class supports multiple LUNs. It is therefore possible to create a storage device that acts as a CD-ROM and a Flash disk at the same time. In such a case, the initialization would be slightly different. Here is an example for a Flash Disk and CD-ROM:

```c
/* Store the number of LUN in this device storage instance. */
storage_parameter.ux_slave_class_storage_parameter_number_lun = 2;

/* Initialize the storage class parameters for reading/writing to the Flash Disk. */
storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_last_lba = 0x7bbff;

storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_block_length = 512;

storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_type = 0;

storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_removable_flag = 0x80;

storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_read = tx_demo_thread_flash_media_read;

storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_write = tx_demo_thread_flash_media_write;

storage_parameter.ux_slave_class_storage_parameter_lun[0].ux_slave_class_storage_media_status = tx_demo_thread_flash_media_status;

/* Initialize the storage class LUN parameters for reading/writing to the CD-ROM. */

storage_parameter.ux_slave_class_storage_parameter_lun[1].ux_slave_class_storage_media_last_lba = 0x04caaf;

storage_parameter.ux_slave_class_storage_parameter_lun[1].ux_slave_class_storage_media_block_length = 2048;

storage_parameter.ux_slave_class_storage_parameter_lun[1].ux_slave_class_storage_media_type = 5;

storage_parameter.ux_slave_class_storage_parameter_lun[1].ux_slave_class_storage_media_removable_flag = 0x80;

storage_parameter.ux_slave_class_storage_parameter_lun[1].ux_slave_class_storage_media_read = tx_demo_thread_cdrom_media_read;

storage_parameter.ux_slave_class_storage_parameter_lun[1].ux_slave_class_storage_media_write = tx_demo_thread_cdrom_media_write;

storage_parameter.ux_slave_class_storage_parameter_lun[1].ux_slave_class_storage_media_status = tx_demo_thread_cdrom_media_status;

/* Initialize the device storage class for a Flash disk and CD-ROM. The class is connected with interface 0 */ status = ux_device_stack_class_register(_ux_system_slave_class_storage_name,ux_device_class_storage_entry,
    ux_device_class_storage_thread,0, (VOID *) &storage_parameter);
```

### Write Caching SCSI LUN

The USBX device storage class supports write caching on LUNs.

The application needs to pass a callback function pointer to allow the storage class to report write caching enable to host and do flushing on host request.

The flush callback function has the following prototype:

```c
UINT    media_flush(VOID *storage, ULONG lun, ULONG number_blocks,    ULONG lba, ULONG *media_status);
```

The calling parameter *number_blocks* and *lba* specifies the area on LUN that needs flush.

Note that when the callback is not assigned, host is not notified for write caching support, so there is no option for it. When the callback is assigned, host is notified for write caching enabled, but not allowed to change this setting.

## USB Device CDC-ACM Class

The USB device CDC-ACM class allows for a USB host system to communicate with the device as a serial device. This class is based on the USB standard and is a subset of the CDC standard.

A CDC-ACM compliant device framework needs to be declared by the device stack. An example is found here below.

```c
unsigned char device_framework_full_speed[] = {

    /*
    Device descriptor 18 bytes
    0x02 bDeviceClass: CDC class code
    0x00 bDeviceSubclass: CDC class sub code 0x00 bDeviceProtocol: CDC Device protocol
    idVendor & idProduct - https://www.linux-usb.org/usb.ids
    */

    0x12, 0x01, 0x10, 0x01,
    0xEF, 0x02, 0x01, 0x08,
    0x84, 0x84, 0x00, 0x00,
    0x00, 0x01, 0x01, 0x02,
    0x03, 0x01,

    /* Configuration 1 descriptor 9 bytes */
    0x09, 0x02, 0x4b, 0x00, 0x02, 0x01, 0x00,0x40, 0x00,

    /* Interface association descriptor. 8 bytes. */
    0x08, 0x0b, 0x00,
    0x02, 0x02, 0x02, 0x00, 0x00,

    /* Communication Class Interface Descriptor Requirement. 9 bytes. */
    0x09, 0x04, 0x00, 0x00,0x01,0x02, 0x02, 0x01, 0x00,

    /* Header Functional Descriptor 5 bytes */
    0x05, 0x24, 0x00,0x10, 0x01,

    /* ACM Functional Descriptor 4 bytes */
    0x04, 0x24, 0x02,0x0f,

    /* Union Functional Descriptor 5 bytes */
    0x05, 0x24, 0x06, 0x00,

    /* Master interface */
    0x01, /* Slave interface */

    /* Call Management Functional Descriptor 5 bytes */
    0x05, 0x24, 0x01,0x03, 0x01, /* Data interface */

    /* Endpoint 1 descriptor 7 bytes */
    0x07, 0x05, 0x83, 0x03,0x08, 0x00, 0xFF,

    /* Data Class Interface Descriptor Requirement 9 bytes */
    0x09, 0x04, 0x01, 0x00, 0x02,0x0A, 0x00, 0x00, 0x00,

    /* First alternate setting Endpoint 1 descriptor 7 bytes*/
    0x07, 0x05, 0x02,0x02,0x40, 0x00,0x00,

    /* Endpoint 2 descriptor 7 bytes */
    0x07, 0x05, 0x81,0x02,0x40, 0x00, 0x00,

};
```

The CDC-ACM class uses a composite device framework to group interfaces (control and data). As a result care should be taken when defining the device descriptor. __USBX relies on the interface association descriptor (IAD) to know internally how to bind interfaces__. The IAD descriptor should be declared before the interfaces and contain the first interface of the CDC-ACM class and how many interfaces are attached.

The CDC-ACM class also uses a union functional descriptor which performs the same function as the newer IAD descriptor. Although a Union Functional descriptor must be declared for historical reasons and compatibility with the host side, it is not used by USBX.

The initialization of the CDC-ACM class expects the following parameters.

```c
/* Set the parameters for callback when insertion/extraction of a CDC device. */

parameter.ux_slave_class_cdc_acm_instance_activate = tx_demo_cdc_instance_activate;

parameter.ux_slave_class_cdc_acm_instance_deactivate = tx_demo_cdc_instance_deactivate;

parameter.ux_slave_class_cdc_acm_parameter_change = tx_demo_cdc_instance_parameter_change;

/* Initialize the device cdc class. This class owns both interfaces starting with 0. */
status = ux_device_stack_class_register(_ux_system_slave_class_cdc_acm_name,ux_device_class_cdc_acm_entry,
    1,0, &parameter);
```

The 2 parameters defined are callback pointers into the user applications that will be called when the stack activates or deactivate this class.

The third parameter defined is a callback pointer to the user application that will be called when there is line coding or line states parameter change. E.g., when there is request from host to change DTR state to **TRUE**, the callback is invoked, in it user application can check line states through IOCTL function to kow host is ready for communication.

The CDC-ACM is based on a USB-IF standard and is automatically recognized by MACOs and Linux operating systems. On Windows platforms, this class requires a .inf file for Windows version prior to 10. Windows 10 does not require any .inf files. We supply a template for the CDC-ACM class and it can be found in the ***usbx_windows_host_files*** directory. For more recent version of Windows the file CDC_ACM_Template_Win7_64bit.inf should be used (except Win10). This file needs to be modified to reflect the PID/VID used by the device. The PID/VID will be specific to the final customer when the company and the product are registered with the USB-IF. In the inf file, the fields to modify are located here.

```INF
[DeviceList]
%DESCRIPTION%=DriverInstall, USB\VID_8484&PID_0000

[DeviceList.NTamd64]
%DESCRIPTION%=DriverInstall, USB\VID_8484&PID_0000
```

In the device framework of the CDC-ACM device, the PID/VID are stored in the device descriptor (see the device descriptor declared above).

When a USB host systems discovers the USB CDC-ACM device, it will mount a serial class and the device can be used with any serial terminal program. See the host Operating System for reference.

The CDC-ACM class API functions are defined below.

### ux_device_class_cdc_acm_ioctl

Perform IOCTL on the CDC-ACM interface

### Prototype

```c
UINT ux_device_class_cdc_acm_ioctl ( 
    UX_SLAVE_CLASS_CDC_ACM *cdc_acm, 
    ULONG ioctl_function,  
    VOID *parameter);
```

### Description

This function is called when an application needs to perform various ioctl calls to the cdc acm interface

### Parameters

- **cdc_acm**: Pointer to the cdc class instance.
- **ioctl_function**: Ioctl function to be performed.
- **parameter**: Pointer to a parameter specific to the ioctl call.

### Return Value

- **UX_SUCCESS** (0x00) This operation was successful.
- **UX_ERROR** (0xFF) Error from function

### Example

```c
/* Start cdc acm callback transmission. */

status = _ux_device_class_cdc_acm_ioctl(cdc_acm_slave,
    UX_SLAVE_CLASS_CDC_ACM_IOCTL_TRANSMISSION_START, &callback);

if(status != UX_SUCCESS)
    return;
```

### Ioctl functions:

|                    Function                     | Value |
| ----------------------------------------------- | ----- |
| UX_SLAVE_CLASS_CDC_ACM_IOCTL_SET_LINE_CODING    | 1     |
| UX_SLAVE_CLASS_CDC_ACM_IOCTL_GET_LINE_CODING    | 2     |
| UX_SLAVE_CLASS_CDC_ACM_IOCTL_GET_LINE_STATE     | 3     |
| UX_SLAVE_CLASS_CDC_ACM_IOCTL_ABORT_PIPE         | 4     |
| UX_SLAVE_CLASS_CDC_ACM_IOCTL_SET_LINE_STATE     | 5     |
| UX_SLAVE_CLASS_CDC_ACM_IOCTL_TRANSMISSION_START | 6     |
| UX_SLAVE_CLASS_CDC_ACM_IOCTL_TRANSMISSION_STOP  | 7     |

### ux_device_class_cdc_acm_ioctl: UX_SLAVE_CLASS_CDC_ACM_IOCTL_SET_LINE_CODING

Perform IOCTL Set Line Coding on the CDC-ACM interface

### Prototype

```c
UINT ux_device_class_cdc_acm_ioctl ( 
    UX_SLAVE_CLASS_CDC_ACM*cdc_acm,
    ULONG ioctl_function,  
    VOID *parameter);
```

### Description

This function is called when an application needs to Set the Line Coding parameters.

### Parameters

- **cdc_acm**: Pointer to the cdc class instance.
- **ioctl_function**: ux_SLAVE_CLASS_CDC_ACM_IOCTL_SET_LINE_CODING
- **parameter**: Pointer to a line parameter structure:

```c
typedef struct UX_SLAVE_CLASS_CDC_ACM_LINE_CODING_PARAMETER_STRUCT
{
    ULONG ux_slave_class_cdc_acm_parameter_baudrate;
    UCHAR ux_slave_class_cdc_acm_parameter_stop_bit;
    UCHAR ux_slave_class_cdc_acm_parameter_parity;
    UCHAR ux_slave_class_cdc_acm_parameter_data_bit;
} UX_SLAVE_CLASS_CDC_ACM_LINE_CODING_PARAMETER;
```

### Return Value

**UX_SUCCESS** (0x00) This operation was successful.

### Example

```c
/* Change the line coding values. */

line_coding.ux_slave_class_cdc_acm_line_coding_dter = 9600;
line_coding.ux_slave_class_cdc_acm_line_coding_stop_bit =
    UX_HOST_CLASS_CDC_ACM_LINE_CODING_STOP_BIT_15;

line_coding.ux_slave_class_cdc_acm_line_coding_parity =
    UX_HOST_CLASS_CDC_ACM_LINE_CODING_PARITY_EVEN;

line_coding.ux_slave_class_cdc_acm_line_coding_data_bits = 5;

status = _ux_slave_class_cdc_acm_ioctl(cdc_acm,
    UX_SLAVE_CLASS_CDC_ACM_IOCTL_SET_LINE_CODING, &line_coding);

if (status != UX_SUCCESS)
    break;
```

### ux_device_class_cdc_acm_ioctl: UX_SLAVE_CLASS_CDC_ACM_IOCTL_GET_LINE_CODING

Perform IOCTL Get Line Coding on the CDC-ACM interface

### Prototype

```c
device_class_cdc_acm_ioctl ( 
    UX_SLAVE_CLASS_CDC_ACM *cdc_acm,
    ULONG ioctl_function,  
    VOID *parameter);
```

### Description

This function is called when an application needs to Get the Line Coding parameters.

### Parameters

- **cdc_acm**: Pointer to the cdc class instance.
- **ioctl_function**: ux_SLAVE_CLASS_CDC_ACM_IOCTL_GET_ LINE_CODING
- **parameter**: Pointer to a line parameter structure:

```c
typedef struct UX_SLAVE_CLASS_CDC_ACM_LINE_CODING_PARAMETER_STRUCT
{
    ULONG ux_slave_class_cdc_acm_parameter_baudrate;
    UCHAR ux_slave_class_cdc_acm_parameter_stop_bit;
    UCHAR ux_slave_class_cdc_acm_parameter_parity;
    UCHAR ux_slave_class_cdc_acm_parameter_data_bit;
} UX_SLAVE_CLASS_CDC_ACM_LINE_CODING_PARAMETER;
```

### Return Value

- **UX_SUCCESS** (0x00) This operation was successful.

### Example

```c
/* This is to retrieve BAUD rate. */

status = _ux_device_class_cdc_acm_ioctl(cdc_acm_slave,
    UX_SLAVE_CLASS_CDC_ACM_IOCTL_GET_LINE_CODING, &line_coding);

/* Any error ? */
if (status == UX_SUCCESS)
{
    /* Decode BAUD rate. */
    switch (line_coding.ux_slave_class_cdc_acm_parameter_baudrate)
    {
        case 1200 :
            status = tx_demo_thread_slave_cdc_acm_response("1200", 4);
            break;
        case 2400 :
            status = tx_demo_thread_slave_cdc_acm_response("2400", 4);
            break;
        case 4800 :
            status = tx_demo_thread_slave_cdc_acm_response("4800", 4);
            break;
        case 9600 :
            status = tx_demo_thread_slave_cdc_acm_response("9600", 4);
            break;
        case 115200 :
            status = tx_demo_thread_slave_cdc_acm_response("115200", 6);
            break;
    }
}
```

### ux_device_class_cdc_acm_ioctl: UX_SLAVE_CLASS_CDC_ACM_IOCTL_GET_LINE_STATE

Perform IOCTL Get Line State on the CDC-ACM interface

### Prototype

```c
UINT ux_device_class_cdc_acm_ioctl ( 
    UX_SLAVE_CLASS_CDC_ACM*cdc_acm,
    ULONG ioctl_function,  
    VOID *parameter);
```

### Description

This function is called when an application needs to Get the Line State parameters.

### Parameters

- **cdc_acm**: Pointer to the cdc class instance.
- **ioctl_function**: ux_SLAVE_CLASS_CDC_ACM_IOCTL_GET_LINE_STATE
- **parameter**: Pointer to a line parameter structure:

```c
typedef struct UX_SLAVE_CLASS_CDC_ACM_LINE_STATE_PARAMETER_STRUCT
{
    UCHAR ux_slave_class_cdc_acm_parameter_rts;
    UCHAR ux_slave_class_cdc_acm_parameter_dtr;
} UX_SLAVE_CLASS_CDC_ACM_LINE_STATE_PARAMETER;
```

### Return Value

- **UX_SUCCESS** (0x00) This operation was successful.

### Example

```c
/* This is to retrieve RTS state. */
status = _ux_device_class_cdc_acm_ioctl(cdc_acm_slave,
    UX_SLAVE_CLASS_CDC_ACM_IOCTL_GET_LINE_STATE, &line_state);

/* Any error ? */
if (status == UX_SUCCESS)
{
/* Check state. */
    if (line_state.ux_slave_class_cdc_acm_parameter_rts == UX_TRUE)
        /* State is ON. */
        status = tx_demo_thread_slave_cdc_acm_response("RTS ON", 6);
    else
        /* State is OFF. */
        status = tx_demo_thread_slave_cdc_acm_response("RTS OFF", 7);
}
```

### ux_device_class_cdc_acm_ioctl: UX_SLAVE_CLASS_CDC_ACM_IOCTL_SET_LINE_STATE

Perform IOCTL Set Line State on the CDC-ACM interface

### Prototype

```c
UINT ux_device_class_cdc_acm_ioctl ( 
    UX_SLAVE_CLASS_CDC_ACM *cdc_acm,
    ULONG ioctl_function,  
    VOID *parameter);
```

### Description

This function is called when an application needs to Get the Line State parameters

### Parameters

- **cdc_acm**: Pointer to the cdc class instance.
- **ioctl_function**: ux_SLAVE_CLASS_CDC_ACM_IOCTL_SET_LINE_STATE
- **parameter**: Pointer to a line parameter structure:

```c
typedef struct UX_SLAVE_CLASS_CDC_ACM_LINE_STATE_PARAMETER_STRUCT
{
    UCHAR ux_slave_class_cdc_acm_parameter_rts;
    UCHAR ux_slave_class_cdc_acm_parameter_dtr;
} UX_SLAVE_CLASS_CDC_ACM_LINE_STATE_PARAMETER;
```

### Return Value

- **UX_SUCCESS** (0x00) This operation was successful.

### Example

```c
/* This is to set RTS state. */

line_state.ux_slave_class_cdc_acm_parameter_rts = UX_TRUE;
status = _ux_device_class_cdc_acm_ioctl(cdc_acm_slave,
    UX_SLAVE_CLASS_CDC_ACM_IOCTL_SET_LINE_STATE, &line_state);

/* If status is UX_SUCCESS, the operation was successful. */
```

### ux_device_class_cdc_acm_ioctl: UX_SLAVE_CLASS_CDC_ACM_IOCTL_ABORT_PIPE

Perform IOCTL ABORT PIPE on the CDC-ACM interface

### Prototype

```c
UINT ux_device_class_cdc_acm_ioctl ( 
    UX_SLAVE_CLASS_CDC_ACM *cdc_acm,
    ULONG ioctl_function,  
    VOID *parameter);
```

### Description

This function is called when an application needs to abort a pipe. For example, to abort an ongoing write, UX_SLAVE_CLASS_CDC_ACM_ENDPOINT_XMIT should be passed as the parameter.

### Parameters

- **cdc_acm**: Pointer to the cdc class instance.
- **ioctl_function**: ux_SLAVE_CLASS_CDC_ACM_IOCTL_ABORT_PIPE
- **parameter**: The pipe direction:

```c
UX_SLAVE_CLASS_CDC_ACM_ENDPOINT_XMIT 1

UX_SLAVE_CLASS_CDC_ACM_ENDPOINT_RCV 2
```

### Return Value

- **UX_SUCCESS** (0x00) This operation was successful.
- **UX_ENDPOINT_HANDLE_UNKNOWN** (0x53) Invalid pipe direction.

### Example

```c
/* This is to abort the Xmit pipe. */

status = _ux_device_class_cdc_acm_ioctl(cdc_acm_slave,
    UX_SLAVE_CLASS_CDC_ACM_IOCTL_ABORT_PIPE,
    UX_SLAVE_CLASS_CDC_ACM_ENDPOINT_XMIT);

/* If status is UX_SUCCESS, the operation was successful. */
```

### ux_device_class_cdc_acm_ioctl: UX_SLAVE_CLASS_CDC_ACM_IOCTL_TRANSMISSION_START

Perform IOCTL Transmission Start on the CDC-ACM interface

### Prototype

```c
UINT ux_device_class_cdc_acm_ioctl ( 
    UX_SLAVE_CLASS_CDC_ACM *cdc_acm,
    ULONG ioctl_function,  
    VOID *parameter);
```

### Description

This function is called when an application wants to use transmission with callback.

### Parameters

- **cdc_acm**: Pointer to the cdc class instance.
- **ioctl_function**: ux_SLAVE_CLASS_CDC_ACM_IOCTL_TRANSMISSION_START
- **parameter**: Pointer to the Start Transmission parameter structure:

```c
typedef struct UX_SLAVE_CLASS_CDC_ACM_CALLBACK_PARAMETER_STRUCT
{
    UINT (*ux_device_class_cdc_acm_parameter_write_callback)(struct UX_SLAVE_CLASS_CDC_ACM_STRUCT *cdc_acm,
        UINT status, ULONG length);
    UINT (*ux_device_class_cdc_acm_parameter_read_callback)(struct UX_SLAVE_CLASS_CDC_ACM_STRUCT *cdc_acm,
        UINT status, UCHAR *data_pointer, ULONG length);
} UX_SLAVE_CLASS_CDC_ACM_CALLBACK_PARAMETER;
```

### Return Value

- **UX_SUCCESS** (0x00) This operation was successful.
- **UX_ERROR** (0xFF) Transmission already started.
- **UX_MEMORY_INSUFFICIENT** (0x12) A memory allocation failed.

### Example

```c
/* Set the callback parameter. */

callback.ux_device_class_cdc_acm_parameter_write_callback
    = tx_demo_thread_slave_write_callback;

callback.ux_device_class_cdc_acm_parameter_read_callback
    = tx_demo_thread_slave_read_callback;

/* Program the start of transmission. */
status = _ux_device_class_cdc_acm_ioctl(cdc_acm_slave,
    UX_SLAVE_CLASS_CDC_ACM_IOCTL_TRANSMISSION_START, &callback);

/* If status is UX_SUCCESS, the operation was successful. */
```

### ux_device_class_cdc_acm_ioctl: UX_SLAVE_CLASS_CDC_ACM_IOCTL_TRANSMISSION_STOP

Perform IOCTL Transmission Stop on the CDC-ACM interface

### Prototype

```c
UINT ux_device_class_cdc_acm_ioctl( 
    UX_SLAVE_CLASS_CDC_ACM *cdc_acm,
    ULONG ioctl_function,  
    VOID *parameter);
```

### Description

This function is called when an application wants to stop using transmission with callback.

### Parameters

- **cdc_acm**: Pointer to the cdc class instance.
- **ioctl_function**: ux_SLAVE_CLASS_CDC_ACM_IOCTL_TRANSMISSION_STOP
- **parameter**: Not used

### Return Value

- **UX_SUCCESS** (0x00) This operation was successful.
- **UX_ERROR** (0xFF) No ongoing transmission.

### Example

```c
/* Program the stop of transmission. */

status = _ux_device_class_cdc_acm_ioctl(cdc_acm_slave,
    UX_SLAVE_CLASS_CDC_ACM_IOCTL_TRANSMISSION_STOP, UX_NULL);

/* If status is UX_SUCCESS, the operation was successful. */
```

### ux_device_class_cdc_acm_read

Read from CDC-ACM pipe

### Prototype

```c
UINT ux_device_class_cdc_acm_read( 
    UX_SLAVE_CLASS_CDC_ACM *cdc_acm,
    UCHAR *buffer,  
    ULONG requested_length,  
    ULONG *actual_length);
```

### Description

This function is called when an application needs to read from the OUT data pipe (OUT from the host, IN from the device). It is blocking.

> **Note:** This functions reads raw bulk data from host, so it keeps pending until buffer is full or host terminates the transfer by a short packet (including Zero Length Packet). For more details, please refer to section [**General Considerations for Bulk Transfer**](#general-considerations-for-bulk-transfer).
> The function reads bytes from the host packet by packet. If the prepared buffer size is smaller than a packet and the host sends more data than expected (in other words, the prepared buffer size is not a multiple of the USB endpoint's max packet size), then buffer overflow will occur. To avoid this issue, the recommended way to read is to allocate a buffer exactly one packet size (USB endpoint max packet size). This way if there is more data, the next read can get it and no buffer overflow will occur. If there is less data, the current read can get a short packet instead of generating an error.

### Parameters

- **cdc_acm**: Pointer to the cdc class instance.
- **buffer**: Buffer address where data will be stored.
- **requested_length**: The maximum length we expect.
- **actual_length**: The length returned into the buffer.

### Return Value

- **UX_SUCCESS** (0x00) This operation was successful.
- **UX_CONFIGURATION_HANDLE_UNKNOWN** (0x51) Device is no longer in the configured state.
- **UX_TRANSFER_NO_ANSWER** (0x22) No answer from device. The device was probably disconnected while the transfer was pending.

### Example

```c
/* Read from the CDC class. */

status = ux_device_class_cdc_acm_read(cdc, buffer, UX_DEMO_BUFFER_SIZE, &actual_length);

if(status != UX_SUCCESS) return;
```

### ux_device_class_cdc_acm_write

Write to a CDC-ACM pipe

### Prototype

```c
UINT ux_device_class_cdc_acm_write( 
    UX_SLAVE_CLASS_CDC_ACM *cdc_acm,
    UCHAR *buffer,  
    ULONG requested_length,  
    ULONG *actual_length);
```

### Description

This function is called when an application needs to write to the IN data pipe (IN from the host, OUT from the device). It is blocking.

### Parameters

- **cdc_acm**: Pointer to the cdc class instance.
- **buffer**: Buffer address where data is stored.
- **requested_length**: The length of the buffer to write.
- **actual_length**: The length returned into the buffer after write is performed.

### Return Value
- **UX_SUCCESS** (0x00) This operation was successful.
- **UX_CONFIGURATION_HANDLE_UNKNOWN** (0x51) Device is no longer in the configured state.
- **UX_TRANSFER_NO_ANSWER** (0x22) No answer from device. The device was probably disconnected while the transfer was pending.

### Example

```c
/* Write to the CDC class bulk in pipe. */

status = ux_device_class_cdc_acm_write(cdc, buffer, UX_DEMO_BUFFER_SIZE, &actual_length);

if(status != UX_SUCCESS)
    return;
```

### ux_device_class_cdc_acm_write_with_callback

Writing to a CDC-ACM pipe with callback

### Prototype

```c
UINT ux_device_class_cdc_acm_write_with_callback( 
    UX_SLAVE_CLASS_CDC_ACM *cdc_acm,
    UCHAR *buffer,  
    ULONG requested_length);
```

### Description

This function is called when an application needs to write to the IN data pipe (IN from the host, OUT from the device). This function is non-blocking and the completion will be done through a callback set in **UX_SLAVE_CLASS_CDC_ACM_IOCTL_TRANSMISSION_START**.

### Parameters

- **cdc_acm**: Pointer to the cdc class instance.
- **buffer**: Buffer address where data is stored.
- **requested_length**: The length of the buffer to write.
- **actual_length**: The length returned into the buffer after write is performed

### Return Value

- **UX_SUCCESS** (0x00) This operation was successful.
- **UX_CONFIGURATION_HANDLE_UNKNOWN** (0x51) Device is no longer in the configured state.
- **UX_TRANSFER_NO_ANSWER** (0x22) No answer from device. The device was probably disconnected while the transfer was pending.

### Example

```c
/* Write to the CDC class bulk in pipe non blocking mode. */
status = ux_device_class_cdc_acm_write_with_callback(cdc, buffer, UX_DEMO_BUFFER_SIZE);

if(status != UX_SUCCESS)
    return;
```

### USB Device CDC-ECM Class

The USB device CDC-ECM class allows for a USB host system to communicate with the device as a ethernet device. This class is based on the USB standard and is a subset of the CDC standard.

A CDC-ECM compliant device framework needs to be declared by the device stack. An example is found here below.

```c
unsigned char device_framework_full_speed[] = {

    /* Device descriptor 18 bytes
    0x02 bDeviceClass: CDC_ECM class code
    0x06 bDeviceSubclass: CDC_ECM class sub code
    0x00 bDeviceProtocol: CDC_ECM Device protocol
    idVendor & idProduct - https://www.linux-usb.org/usb.ids
    0x3939 idVendor: Eclipse ThreadX test.
    */
    
    0x12, 0x01, 0x10, 0x01,
    0x02, 0x00, 0x00, 0x08,
    0x39, 0x39, 0x08, 0x08, 0x00, 0x01, 0x01, 0x02, 03,0x01,
    
    /* Configuration 1 descriptor 9 bytes. */
    0x09, 0x02, 0x58, 0x00,0x02, 0x01, 0x00,0x40, 0x00,
    
    /* Interface association descriptor. 8 bytes. */
    
    0x08, 0x0b, 0x00, 0x02, 0x02, 0x06, 0x00, 0x00,
    
    /* Communication Class Interface Descriptor Requirement 9 bytes */
    0x09, 0x04, 0x00, 0x00,0x01,0x02, 0x06, 0x00, 0x00,
    
    /* Header Functional Descriptor 5 bytes */
    0x05, 0x24, 0x00, 0x10, 0x01,
    
    /* ECM Functional Descriptor 13 bytes */
    0x0D, 0x24, 0x0F, 0x04,0x00, 0x00, 0x00, 0x00, 0xEA, 0x05, 0x00,
    0x00,0x00,
    
    /* Union Functional Descriptor 5 bytes */
    0x05, 0x24, 0x06, 0x00,0x01,
    
    /* Endpoint descriptor (Interrupt) */
    0x07, 0x05, 0x83, 0x03, 0x08, 0x00, 0x08,
    
    /* Data Class Interface Descriptor Alternate Setting 0, 0 endpoints. 9 bytes */
    0x09, 0x04, 0x01, 0x00, 0x00, 0x0A, 0x00, 0x00, 0x00,
    
    /* Data Class Interface Descriptor Alternate Setting 1, 2 endpoints. 9 bytes */
    0x09, 0x04, 0x01, 0x01, 0x02, 0x0A, 0x00, 0x00,0x00,
    
    /* First alternate setting Endpoint 1 descriptor 7 bytes */
    0x07, 0x05, 0x02, 0x02, 0x40, 0x00, 0x00,
    
    /* Endpoint 2 descriptor 7 bytes */
    0x07, 0x05, 0x81, 0x02, 0x40, 0x00,0x00

};
```

The CDC-ECM class uses a very similar device descriptor approach to the CDC-ACM and also requires an IAD descriptor. See the CDC-ACM class for definition.

In addition to the regular device framework, the CDC-ECM requires special string descriptors. An example is given below.

```c
unsigned char string_framework[] = {
    /* Manufacturer string descriptor: Index 1 - "Eclipse ThreadX" */
    0x09, 0x04, 0x01, 0x0c,
    0x45, 0x78, 0x70, 0x72, 0x65, 0x73, 0x20, 0x4c,
    0x6f, 0x67, 0x69, 0x63,
    
    /* Product string descriptor: Index 2 - "EL CDCECM Device" */
    0x09, 0x04, 0x02, 0x10,
    0x45, 0x4c, 0x20, 0x43, 0x44, 0x43, 0x45, 0x43,
    0x4d, 0x20, 0x44, 0x65, 0x76, 0x69, 0x63, 0x64,
    
    /* Serial Number string descriptor: Index 3 - "0001" */
    0x09, 0x04, 0x03, 0x04,
    0x30, 0x30, 0x30, 0x31,
    
    /* MAC Address string descriptor: Index 4 - "001E5841B879" */
    0x09, 0x04, 0x04, 0x0C,
    0x30, 0x30, 0x31, 0x45, 0x35, 0x38,
    0x34, 0x31, 0x42, 0x38, 0x37, 0x39

};
```

The MAC address string descriptor is used by the CDC-ECM class to reply to the host queries as to what MAC address the device is answering to at the TCP/IP protocol. It can be set to the device choice but must be defined here.

The initialization of the CDC-ECM class is as follows.

```c
/* Set the parameters for callback when insertion/extraction of a CDC device. Set to NULL.*/
cdc_ecm_parameter.ux_slave_class_cdc_ecm_instance_activate = UX_NULL;
cdc_ecm_parameter.ux_slave_class_cdc_ecm_instance_deactivate = UX_NULL;

/* Define a NODE ID. */
cdc_ecm_parameter.ux_slave_class_cdc_ecm_parameter_local_node_id[0] = 0x00;
cdc_ecm_parameter.ux_slave_class_cdc_ecm_parameter_local_node_id[1] = 0x1e;
cdc_ecm_parameter.ux_slave_class_cdc_ecm_parameter_local_node_id[2] = 0x58;
cdc_ecm_parameter.ux_slave_class_cdc_ecm_parameter_local_node_id[3] = 0x41;
cdc_ecm_parameter.ux_slave_class_cdc_ecm_parameter_local_node_id[4] = 0xb8;
cdc_ecm_parameter.ux_slave_class_cdc_ecm_parameter_local_node_id[5] = 0x78;

/* Define a remote NODE ID. */
cdc_ecm_parameter.ux_slave_class_cdc_ecm_parameter_remote_node_id[0] = 0x00;
cdc_ecm_parameter.ux_slave_class_cdc_ecm_parameter_remote_node_id[1] = 0x1e;
cdc_ecm_parameter.ux_slave_class_cdc_ecm_parameter_remote_node_id[2] = 0x58;
cdc_ecm_parameter.ux_slave_class_cdc_ecm_parameter_remote_node_id[3] = 0x41;
cdc_ecm_parameter.ux_slave_class_cdc_ecm_parameter_remote_node_id[4] = 0xb8;
cdc_ecm_parameter.ux_slave_class_cdc_ecm_parameter_remote_node_id[5] = 0x79;

/* Initialize the device cdc_ecm class. */
status = ux_device_stack_class_register(_ux_system_slave_class_cdc_ecm_name,
    ux_device_class_cdc_ecm_entry, 1,0,&cdc_ecm_parameter);
```

The initialization of this class expects the same function callback for activation and deactivation, although here as an exercise they are set to NULL so that no callback is performed.

The next parameters are for the definition of the node IDs. 2 Nodes are necessary for the CDC-ECM, a local node and a remote node. The local node specifies the MAC address of the device, while the remote node specifies the MAC address of the host. The remote node must be the same one as the one declared in the device framework string descriptor.

The CDC-ECM class has built-in APIs for transferring data both ways but they are hidden to the application as the user application will communicate with the USB Ethernet device through NetX.

The USBX CDC-ECM class is closely tied to the NetX Duo Network stack. An application using both NetX Duo and USBX CDC-ECM class will activate the NetX network stack in its usual way but in addition needs to activate the USB network stack as follows.

```c
/* Initialize the NetX Duo system. */
nx_system_initialize();

/* Perform the initialization of the network driver. This will initialize the USBX network layer.*/
ux_network_driver_init();
```

The USB network stack needs to be activated only once and is not specific to CDCECM but is required by any USB class that requires NetX services.

The CDC-ECM class will be recognized by MAC OS and Linux hosts. But there is no driver supplied by Windows to recognize CDC-ECM natively. Some commercial products do exist for Windows platforms and they supply their own .inf file. This file will need to be modified the same way as the CDC-ACM inf template to match the PID/VID of the USB network device.

## USB Device HID Class

The USB device HID class allows for a USB host system to connect to a HID device with specific HID client capabilities.

USBX HID device class is relatively simple compared to the host side. It is closely tied to the behavior of the device and its HID descriptor.

Any HID client requires first to define a HID device framework as the example below.

```c
UCHAR device_framework_full_speed[] = {
    /* Device descriptor */
    0x12, 0x01, 0x10, 0x01, 0x00, 0x00, 0x00, 0x08,
    0x81, 0x0A, 0x01, 0x01, 0x00, 0x00, 0x00, 0x00,
    0x00, 0x01,

    /* Configuration descriptor */
    0x09, 0x02, 0x22, 0x00, 0x01, 0x01, 0x00, 0xc0, 0x32,

    /* Interface descriptor */
    0x09, 0x04, 0x00, 0x00, 0x01, 0x03, 0x00, 0x00, 0x00,

    /* HID descriptor */
    0x09, 0x21, 0x10, 0x01, 0x21, 0x01, 0x22, 0x3f, 0x00,

    /* Endpoint descriptor (Interrupt) */
    0x07, 0x05, 0x81, 0x03, 0x08, 0x00, 0x08

};
```

The HID framework contains an interface descriptor that describes the HID class and the HID device subclass. The HID interface can be a standalone class or part of a composite device. If the HID device supports multiple report, its *report_id* parameter should be set to **UX_TRUE**, if not it should be set to **UX_FALSE**.

The initialization of the HID class is as follow, using a USB keyboard as an example.

```c
/* Initialize the hid class parameters for a keyboard. */
hid_parameter.ux_device_class_hid_parameter_report_address = hid_keyboard_report;
hid_parameter.ux_device_class_hid_parameter_report_length = HID_KEYBOARD_REPORT_LENGTH;
hid_parameter.ux_device_class_hid_parameter_callback = tx_demo_thread_hid_callback;
hid_parameter.ux_device_class_hid_parameter_get_callback = tx_demo_thread_hid_get_callback;
hid_parameter.ux_device_class_hid_parameter_report_id = 0;

/* Initialize the device hid class. The class is connected with interface 0 */

status = ux_device_stack_class_register(_ux_system_slave_class_hid_name,
    ux_device_class_hid_entry, 1,0,(VOID *)&hid_parameter);
if (status!=UX_SUCCESS)
    return;
```

The application needs to pass to the HID class a HID report descriptor and its length. The report descriptor is a collection of items that describe the device. For more information on the HID grammar refer to the HID USB class specification.

In addition to the report descriptor, the application passes a call back when a HID event happens.

The USBX HID class supports the following standard HID commands from the host.

|               Command name               | Value |                                   Description                                   |
| ---------------------------------------- | ----- | ------------------------------------------------------------------------------- |
| UX_DEVICE_CLASS_HID_COMMAND_GET_REPORT   | 0x01  | Get a report from the device, callback is invoked to let application fill event |
| UX_DEVICE_CLASS_HID_COMMAND_GET_IDLE     | 0x02  | Get the idle frequency of the interrupt endpoint                                |
| UX_DEVICE_CLASS_HID_COMMAND_GET_PROTOCOL | 0x03  | Get the protocol running on the device                                          |
| UX_DEVICE_CLASS_HID_COMMAND_SET_REPORT   | 0x09  | Set a report to the device, callback is invoked to notify application           |
| UX_DEVICE_CLASS_HID_COMMAND_SET_IDLE     | 0x0a  | Set the idle frequency of the interrupt endpoint                                |
| UX_DEVICE_CLASS_HID_COMMAND_SET_PROTOCOL | 0x0b  | Get the protocol running on the device                                          |

The Get and Set report are the most commonly used commands by HID to transfer data back and forth between the host and the device. Most commonly the host sends data on the control endpoint but can receive data either on the interrupt endpoint or by issuing a GET_REPORT command to fetch the data on the control endpoint.

The HID class can send data back to the host on the interrupt endpoint by using the ux_device_class_hid_event_set function.

### ux_device_class_hid_event_set

Setting an event to the HID class

### Prototype

```c
UINT ux_device_class_hid_event_set( 
    UX_SLAVE_CLASS_HID *hid,
    UX_SLAVE_CLASS_HID_EVENT *hid_event);
```

### Description

This function is called when an application needs to send a HID event back to the host. The function is not blocking, it merely puts the report into a circular queue and returns to the application.

### Parameters

- **hid**: Pointer to the hid class instance.
- **hid_event**: Pointer to structure of the hid event.

### Return Value

- **UX_SUCCESS** (0x00) This operation was successful.
- **UX_ERROR** (0xFF) Error no space available in circular queue.

### Example

```c
/* Insert a key into the keyboard event. Length is fixed to 8. */
hid_event.ux_device_class_hid_event_length = 8;

/* First byte is a modifier byte. */
hid_event.ux_device_class_hid_event_buffer[0] = 0;

/* Second byte is reserved. */
hid_event.ux_device_class_hid_event_buffer[1] = 0;

/* The 6 next bytes are keys. We only have one key here. */
hid_event.ux_device_class_hid_event_buffer[2] = key;

/* Set the keyboard event. */
ux_device_class_hid_event_set(hid, &hid_event);
```

The callback defined at the initialization of the HID class performs the opposite of sending an event. It gets as input the event sent by the host. The prototype of the callback is as follows.

### hid_callback

Notify that an event is from host through control SET_REPORT request

### Prototype

```c
UINT hid_callback(
    UX_SLAVE_CLASS_HID *hid, 
    UX_SLAVE_CLASS_HID_EVENT *hid_event);
```

### Description

This function is called when the host sends a HID SET_REPORT to the application.

### Parameters

- **hid**: Pointer to the hid class instance.
- **hid_event**: Pointer to structure of the hid event.

### Example

The following example shows how to interpret an event for a HID keyboard:

```c
UINT tx_demo_thread_hid_callback(UX_SLAVE_CLASS_HID *hid, UX_SLAVE_CLASS_HID_EVENT *hid_event
{
    /* There was an event. Analyze it. Is it NUM LOCK ? */
    if (hid_event -> ux_device_class_hid_event_buffer[0] & HID_NUM_LOCK_MASK)
        /* Set the Num lock flag. */
        num_lock_flag = UX_TRUE;
    else
        /* Reset the Num lock flag. */
        num_lock_flag = UX_FALSE;

    /* There was an event. Analyze it. Is it CAPS LOCK ? */
    if (hid_event -> ux_device_class_hid_event_buffer[0] & HID_CAPS_LOCK_MASK)
        /* Set the Caps lock flag. */
        caps_lock_flag = UX_TRUE;
    else
        /* Reset the Caps lock flag. */
        caps_lock_flag = UX_FALSE;
}
```

### hid_get_callback

Notify that a host is requesting event through control GET_REPORT request

### Prototype

```c
UINT hid_get_callback(
    UX_SLAVE_CLASS_HID *hid, 
    UX_SLAVE_CLASS_HID_EVENT *hid_event);
```

### Description

This function is invoked when host is requesting event through control GET_REPORT
request, application must fill event structure in the callback, the data size must
not exceed the event length and the max event buffer size.

### Parameters

- **hid**: Pointer to the hid class instance.
- **hid_event**: Pointer to structure of the hid event.

### Example

The following example shows how to prepare an event:

```c
UINT tx_demo_thread_hid_get_callback(UX_SLAVE_CLASS_HID *hid, UX_SLAVE_CLASS_HID_EVENT *hid_event)
{
    /* Host is asking input for GET_REPORT, prepare event data here.  */

    /* Check if report ID is used,
     * note in application the setting could be consistent so the check can be optimized.
     */
    if (hid -> ux_device_class_hid_report_id)
    {
        /* First byte in buffer must be report ID, if report ID is required.
         * See HID spec. for more details.
         */
        event->ux_device_class_hid_event_report_id = demo_hid_event_report_id;
        event->ux_device_class_hid_event_report_type = demo_hid_event_report_type;
        if (demo_hid_event_length < UX_DEVICE_CLASS_HID_EVENT_BUFFER_LENGTH - 1)
            demo_hid_event_length += 1;
        event->ux_device_class_hid_event_length = demo_hid_event_length;
        *(event->ux_device_class_hid_event_buffer) = (UCHAR)event->ux_device_class_hid_event_report_id;
        _ux_utility_memory_copy(event->ux_device_class_hid_event_buffer + 1,
                                demo_hid_event_buffer,
                                event->ux_device_class_hid_event_length - 1);
    }
    else
        /* A prepared event can be copied here. Note if copying data to event buffer,
         * keep the total size inside UX_DEVICE_CLASS_HID_EVENT_BUFFER_LENGTH.
         */
        _ux_utility_memory_copy(event, my_event, sizeof(UX_SLAVE_CLASS_HID_EVENT));
}
```

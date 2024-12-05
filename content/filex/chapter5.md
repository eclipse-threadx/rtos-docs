---
title: Chapter 5 - I/O Drivers for FileX
description: This chapter contains a description of I/O drivers for FileX and is designed to help developers write application-specific drivers.
---


This chapter contains a description of I/O drivers for FileX and is designed to help developers write application-specific drivers.

## I/O Driver Introduction

FileX supports multiple media devices. The FX_MEDIA structure defines everything required to manage a media device. This structure contains all media information, including the media-specific I/O driver and associated parameters for passing information and status between the driver and FileX. In most systems, there is a unique I/O driver for each FileX media instance.

## I/O Driver Entry

Each FileX I/O driver has a single entry function that is defined by the ***fx_media_open*** service call. The driver entry function has the following format:

```c
void my_driver_entry(FX_MEDIA *media_ptr);
```

FileX calls the I/O driver entry function to request all physical media access, including initialization and boot sector reading. Requests made to the driver are done sequentially; i.e., FileX waits for the current request to complete before another request is sent.

## I/O Driver Requests

Because each I/O driver has a single entry function, FileX makes specific requests through the media control block. Specifically, the  **fx_media_driver_request** member of **FX_MEDIA** is used to specify the exact driver request. The I/O driver communicates the success or failure of the request through the **fx_media_driver_status** member of **FX_MEDIA**. If the driver request was successful, **FX_SUCCESS** is placed in this field before the driver returns. Otherwise, if an error is detected, FX_IO_ERROR is placed in this field.

### Driver Initialization

Although the actual driver initialization processing is application specific, it usually consists of data structure initialization and possibly some preliminary hardware initialization. This request is the first made by FileX and is done from within the fx_media_open service.

If media write protection is detected, the driver fx_media_driver_write_protect member of FX_MEDIA should be set to FX_TRUE.

The following FX_MEDIA members are used for the I/O driver initialization request:

|FX_MEDIA member|Meaning|
|-----------|-----------|
|fx_media_driver_request|FX_DRIVER_INIT|

FileX provides a mechanism to inform the application driver when sectors are no longer being used. This is especially useful for FLASH memory managers that must manage all in-use logical sectors mapped to the FLASH.

If such notification of free sectors is required, the application driver simply sets the *fx_media_driver_free_sector_update* field in the associated **FX_MEDIA** structure to **FX_TRUE**. After set, FileX makes a ***FX_DRIVER_RELEASE_SECTORS*** I/O driver call indicating when one or more consecutive sectors becomes free.

### Boot Sector Read

Instead of using a standard read request, FileX makes a specific request to read the media's boot sector. The following **FX_MEDIA** members are used for the I/O driver boot sector read request:

|FX_MEDIA member|Meaning|
|-----------|-----------|
|fx_media_driver_request| FX_DRIVER_BOOT_READ|
|fx_media_driver_buffer| Address of destination for boot sector.|

### Boot Sector Write

Instead of using a standard write request, FileX makes a specific request to write the media's boot sector. The following **FX_MEDIA** members are used for the I/O driver boot sector write request:

|FX_MEDIA member|Meaning|
|-----------|-----------|
|fx_media_driver_request| FX_DRIVER_BOOT_WRITE|
|fx_media_driver_buffer| Address of source for boot sector.|

### Sector Read

FileX reads one or more sectors into memory by issuing a read request to the I/O driver. The following **FX_MEDIA** members are used for the I/O driver read request:

|FX_MEDIA member|Meaning|
|-----------|-----------|
|fx_media_driver_request| FX_DRIVER_READ|
|fx_media_driver_logical_sector|Logical sector to read|
|fx_media_driver_sectors|Number of sectors to read|
|fx_media_driver_buffer|Destination buffer for sector(s) read|
|fx_media_driver_data_sector_read|Set to FX_TRUE if a file data sector is requested. Otherwise, FX_FALSE if a system sector (FAT or directory sector) is requested.|
|fx_media_driver_sector_type|Defines the explicit type of sector requested, as follows:<br />FX_FAT_SECTOR (2)<br />FX_DIRECTORY_SECTOR (3)<br />FX_DATA_SECTOR (4)|

### Sector Write

FileX writes one or more sectors to the physical media by issuing a write request to the I/O driver. The following FX_MEDIA members are used for the I/O driver write request:

|FX_MEDIA member| Meaning|
|-----------|-----------|
|fx_media_driver_request|FX_DRIVER_WRITE|
|fx_media_driver_logical_sector|Logical sector to write|
|fx_media_driver_sectors|Number of sectors to write|
|fx_media_driver_buffer|Source buffer for sector(s) to write|
|fx_media_driver_system_write| Set to FX_TRUE if a system sector is requested (FAT or directory sector). Otherwise, FX_FALSE if a file data sector is requested.|
|fx_media_driver_sector_type|Defines the explicit type of sector requested, as follows:<br> <br>FX_FAT_SECTOR (2) <br> FX_DIRECTORY_SECTOR (3) <br>FX_DATA_SECTOR (4).|

### Driver Flush

FileX flushes all sectors currently in the driver's sector cache to the physical media by issuing a flush request to the I/O driver. Of course, if the driver is not caching sectors, this request requires no driver processing. The following FX_MEDIA members are used for the I/O driver flush request:

|FX_MEDIA member| Meaning|
|-----------|-----------|
|fx_media_driver_request|FX_DRIVER_FLUSH|

### Driver Abort

FileX informs the driver to abort all further physical I/O activity with the physical media by issuing an abort request to the I/O driver. The driver should not perform any I/O again until it is re-initialized. The following FX_MEDIA members are used for the I/O driver abort request:

|FX_MEDIA member| Meaning|
|-----------|-----------|
|fx_media_driver_request| FX_DRIVER_ABORT|

### Release Sectors

If previously selected by the driver during initialization, FileX informs the driver whenever one or more consecutive sectors become free. If the driver is actually a FLASH manager, this information can be used to tell the FLASH manager that these sectors are no longer needed. The following **FX_MEDIA** members are used for the I/O release sectors request:

|FX_MEDIA member| Meaning|
|-----------|-----------|
|fx_media_driver_request|FX_DRIVER_RELEASE_SECTORS|
|fx_media_driver_logical_sector|Start of free sector|
|fx_media_driver_sectors|Number of free sectors|

### Driver Suspension

Because I/O with physical media may take some time, suspending the calling thread is often desirable. Of course, this assumes completion of the underlying I/O operation is interrupt driven. If so, thread suspension is easily implemented with a ThreadX semaphore. After starting the input or output operation, the I/O driver suspends on its own internal I/O semaphore (created with an initial count of zero during driver initialization). As part of the driver I/O completion interrupt processing, the same I/O semaphore is set, which in turn wakes up the suspended thread.

### Sector Translation

Because FileX views the media as linear logical sectors, I/O requests made to the I/O driver are made with logical sectors. It is the driver's responsibility to translate between logical sectors and the physical geometry of the media, which may include heads, tracks, and physical sectors. For FLASH and RAM disk media, the logical sectors typically map directory to physical sectors. In any case, here are the typical formulas to perform the logical to physical sector
mapping in the I/O driver:

```c
media_ptr -> fx_media_driver_physical_sector =
    (media_ptr -> fx_media_driver_logical_sector % media_ptr -> fx_media_sectors_per_track) + 1;

media_ptr -> fx_media_driver_physical_head =
    (media_ptr -> fx_media_driver_logical_sector/ media_ptr ->
    fx_media_sectors_per_track) % media_ptr -> fx_media_heads;

media_ptr -> fx_media_driver_physical_track =(media_ptr ->
    fx_media_driver_logical_sector/ (media_ptr -> fx_media_sectors_per_track *
    media_ptr -> fx_media_heads)));
```

Note that physical sectors start at one, while logical sectors start at zero.

### Hidden Sectors

Hidden sectors resided prior to the boot record on the media. Because they are really outside the scope of the FAT file system layout, they must be accounted for in each logical sector operation the driver does.

### Media Write Protect

The FileX driver can turn on write protect by setting the fx_media_driver_write_protect field in the media control block. This will cause an error to be returned if any FileX calls are made in an attempt to write to the media.

## Sample RAM Driver

The FileX demonstration system is delivered with a small RAM disk
driver, which is defined in the file fx_ram_driver.c. The driver assumes a 32K memory space and creates a
boot record for 256 128-byte sectors. This file provides a good
example of how to implement application specific FileX I/O
drivers.

```c
/*FUNCTION: _fx_ram_driver
RELEASE: PORTABLE C 5.7
AUTHOR: William E. Lamie, Eclipse Foundation, Inc.
DESCRIPTION: This function is the entry point to the generic
    RAM disk driver that is delivered with all versions of FileX.
    The format of the RAM disk is easily modified by calling fx_media_format prior to opening the media.

    This driver also serves as a template for developing FileX drivers
    for actual devices. Simply replace the read/write sector logic
    with calls to read/write from the appropriate physical device.

FileX RAM/FLASH structures look like the following:
Physical Sector             Contents
    0                         Boot record
    1                         FAT Area Start
    +FAT Sectors             Root Directory Start
    +Directory Sectors         Data Sector Start

INPUT: media_ptr Media control block pointer
OUTPUT: None
CALLS:     _fx_utility_memory_copy Copy sector memory
        _fx_utility_16_unsigned_read Read 16-bit unsigned
CALLED BY: FileX System Functions
RELEASE HISTORY:

    DATE         NAME         DESCRIPTION
    12-12-2005     William E.     Lamie Initial Version 5.0
    07-18-2007     William E.     Lamie Modified comment(s),
                                resulting in version 5.1
    03-01-2009     William E.     Lamie Modified comment(s),
                                resulting in version 5.2
    11-01-2015     William E.     Lamie Modified comment(s),
                                resulting in version 5.3
    04-15-2016     William E.     Lamie Modified comment(s),
                                resulting in version 5.4
    04-03-2017     William E.     Lamie Modified comment(s),
                                fixed compiler warnings,
                                resulting in version 5.5
    12-01-2018     William E.     Lamie Modified comment(s),
                                checked buffer overflow,
                                resulting in version 5.6
    08-15-2019     William E.     Lamie Modified comment(s),
                                resulting in version 5.7
*/

VOID _fx_ram_driver(FX_MEDIA *media_ptr) { UCHAR *source_buffer;
                                           UCHAR *destination_buffer;
                                           UINT bytes_per_sector;

    /* There are several useful/important pieces of information contained
        in the media structure, some of which are supplied by FileX and
        others are for the driver to setup. The following
        is a summary of the necessary FX_MEDIA structure members:
    FX_MEDIA Member                     Meaning

    fx_media_driver_request             FileX request type.
        Valid requests from FileX are as follows:
        FX_DRIVER_READ
        FX_DRIVER_WRITE
        FX_DRIVER_FLUSH
        FX_DRIVER_ABORT
        FX_DRIVER_INIT
        FX_DRIVER_BOOT_READ
        FX_DRIVER_RELEASE_SECTORS
        FX_DRIVER_BOOT_WRITE FX_DRIVER_UNINIT

    fx_media_driver_status              This value is RETURNED by the driver.
        If the operation is successful, this field should be set to FX_SUCCESS
        for before returning. Otherwise, if an error occurred, this field should be set to FX_IO_ERROR.

    fx_media_driver_buffer              Pointer to buffer to read or write sector data. This is supplied by FileX.

    fx_media_driver_logical_sector      Logical sector FileX is requesting.
    fx_media_driver_sectors             Number of sectors FileX is requesting.

    The following is a summary of the optional FX_MEDIA structure members: FX_MEDIA Member Meaning

    fx_media_driver_info                Pointer to any additional information or memory.
        This is optional for the driver use and is setup from the fx_media_open call.
        The RAM disk uses this pointer for the RAM disk memory itself.

    fx_media_driver_write_protect       The DRIVER sets this to FX_TRUE when media is write protected.
        This is typically done in initialization, but can be done anytime.

    fx_media_driver_free_sector_update  The DRIVER sets this to FX_TRUE when it needs
        to know when clusters are released. This is important for FLASH wear-leveling drivers.

    fx_media_driver_system_write        FileX sets this flag to FX_TRUE if the sector
        being written is a system sector, e.g., a boot, FAT, or directory sector.
        The driver may choose to use this to initiate error recovery logic for greater fault
        tolerance. fx_media_driver_data_sector_read FileX sets this flag to FX_TRUE
        if the sector(s) being read are file data sectors, i.e., NOT system sectors.

    fx_media_driver_sector_type         FileX sets this variable to the specific type of
        sector being read or written. The following sector types are identified:
            FX_UNKNOWN_SECTOR
            FX_BOOT_SECTOR
            FX_FAT_SECTOR
            FX_DIRECTORY_SECTOR
            FX_DATA_SECTOR */

    /* Process the driver request specified in the media control block. */

    switch (media_ptr -> fx_media_driver_request){

        case FX_DRIVER_READ: {

            /* Calculate the RAM disk sector offset. Note the RAM disk memory
            is pointed to by the fx_media_driver_info pointer, which is supplied
            by the application in the call to fx_media_open. */

            source_buffer = ((UCHAR *)media_ptr -> fx_media_driver_info) +
                ((media_ptr -> fx_media_driver_logical_sector +
                media_ptr -> fx_media_hidden_sectors) * media_ptr -> fx_media_bytes_per_sector);

            /* Copy the RAM sector into the destination. */

             _fx_utility_memory_copy(source_buffer,
                media_ptr -> fx_media_driver_buffer, media_ptr -> fx_media_driver_sectors *
                media_ptr -> fx_media_bytes_per_sector);

            /* Successful driver request. */

            media_ptr -> fx_media_driver_status = FX_SUCCESS; break; }

        case FX_DRIVER_WRITE: {

            /* Calculate the RAM disk sector offset. Note the RAM disk memory
                is pointed to by the fx_media_driver_info pointer, which is supplied
                by the application in the call to fx_media_open. */

            destination_buffer = ((UCHAR *)media_ptr -> fx_media_driver_info) +
                ((media_ptr -> fx_media_driver_logical_sector +
                media_ptr -> fx_media_hidden_sectors) * media_ptr -> fx_media_bytes_per_sector);

            /* Copy the source to the RAM sector. */

            _fx_utility_memory_copy(media_ptr -> fx_media_driver_buffer,
                destination_buffer, media_ptr -> fx_media_driver_sectors *
                media_ptr -> fx_media_bytes_per_sector);

            /* Successful driver request. */

            media_ptr -> fx_media_driver_status = FX_SUCCESS; break; }

        case FX_DRIVER_FLUSH: {
            /* Return driver success. */
            media_ptr -> fx_media_driver_status = FX_SUCCESS; break; }

        case FX_DRIVER_ABORT: {
            /* Return driver success. */
            media_ptr -> fx_media_driver_status = FX_SUCCESS; break; }

        case FX_DRIVER_INIT: {

            /* FLASH drivers are responsible for setting several fields
                in the media structure, as follows:
                media_ptr -> fx_media_driver_free_sector_update media_ptr ->
                fx_media_driver_write_protect
                The fx_media_driver_free_sector_update flag is used to instruct
                FileX to inform the driver whenever sectors are not being used.
                This is especially useful for FLASH managers so they don't have
                maintain mapping for sectors no longer in use.
                The fx_media_driver_write_protect flag can be set anytime by
                the driver to indicate the media is not writable. Write attempts
                made when this flag is set are returned as errors. */
            /* Perform basic initialization here... since the boot record is going
                to be read subsequently and again for volume name requests. */
            /* Successful driver request. */

            media_ptr -> fx_media_driver_status = FX_SUCCESS; break; }

         case FX_DRIVER_UNINIT: {

            /* There is nothing to do in this case for the RAM driver.
                For actual devices some shutdown processing may be necessary. */

            /* Successful driver request. */
            media_ptr -> fx_media_driver_status = FX_SUCCESS; break; }

        case FX_DRIVER_BOOT_READ: {
            /* Read the boot record and return to the caller. */

            /* Calculate the RAM disk boot sector offset, which is at the
                very beginning of the RAM disk. Note the RAM disk memory is pointed
                to by the fx_media_driver_info pointer, which is supplied by the
                application in the call to fx_media_open. */

            source_buffer = (UCHAR *)media_ptr -> fx_media_driver_info;

            /* For RAM driver, determine if the boot record is valid. */

            if ((source_buffer[0] != (UCHAR)0xEB) ||

            ((source_buffer[1] != (UCHAR)0x34) &&

            (source_buffer[1] != (UCHAR)0x76)) ||

            (source_buffer[2] != (UCHAR)0x90)) {

            /* Invalid boot record, return an error! */
            media_ptr -> fx_media_driver_status = FX_MEDIA_INVALID; return; }

            /* For RAM disk only, pickup the bytes per sector. */

            bytes_per_sector =
                _fx_utility_16_unsigned_read(&source_buffer[FX_BYTES_SECTOR]);

            /* Ensure this is less than the media memory size. */
            if (bytes_per_sector \media_ptr -> fx_media_memory_size) {

            media_ptr -> fx_media_driver_status = FX_BUFFER_ERROR; break; }

            /* Copy the RAM boot sector into the destination. */

            _fx_utility_memory_copy(source_buffer, media_ptr -> fx_media_driver_buffer, bytes_per_sector);

            /* Successful driver request. */

            media_ptr -> fx_media_driver_status = FX_SUCCESS; break; }

        case FX_DRIVER_BOOT_WRITE: {

            /* Write the boot record and return to the caller. */

            /* Calculate the RAM disk boot sector offset, which is at the
                very beginning of the RAM disk. Note the RAM disk memory is
                pointed to by the fx_media_driver_info pointer, which is supplied
                by the application in the call to fx_media_open. */ 
            destination_buffer = (UCHAR *)media_ptr -> fx_media_driver_info;

            /* Copy the RAM boot sector into the destination. */

            _fx_utility_memory_copy(media_ptr -> fx_media_driver_buffer,
                destination_buffer, media_ptr -> fx_media_bytes_per_sector);

            /* Successful driver request. */

            media_ptr -> fx_media_driver_status = FX_SUCCESS; break; }

        default: {
            /* Invalid driver request. */
            media_ptr -> fx_media_driver_status = FX_IO_ERROR; break;}
    }
}
```

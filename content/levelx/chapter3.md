---
title: LevelX NAND support
description: NAND flash memory is commonly utilized within LevelX for large data storage, which is typical of file systems.
---


NAND flash memory is commonly utilized for large data storage, which is typical of file systems. NAND memory consists of *blocks*. Within each NAND block is a series of *pages*. NAND blocks are erasable, which means that all pages within the NAND block are erased (set to all ones). Each NAND block page has a set of *spare bytes* that are utilized by LevelX for bookkeeping, bad block management, and error detection. NAND block pages are available in a variety of sizes. The most common page sizes are: 

| **Page Size** | **Spare Bytes** |
| ------------- | --------------- |
| 256           | 8               |
| 512           | 16              |
| 2048          | 64              |

NAND memory differs from NOR memory in that there is no direct access, i.e., NAND memory cannot be read directly from the processor like NOR memory. NAND memory can only be written to after an erase a limited number of times. Again, this differs from NOR memory that can be written an unlimited number of times providing the write request is clearing set bits. Finally, the spare bytes associated with each page are unique to NAND flash. Typical spare byte configurations are as shown in the table below.

| **Spare Bytes** | **Byte numbers** | **Configuration**     |
| ------------------------- | -------------- | --------------------- |
| 8                         | Bytes 0-2:     | ECC bytes             |
|                           | Bytes 3,4,6,7: | LevelX Metadata       |
|                           | Byte 5:        | Bad block flag        |
| 16                        | Bytes 0-3,6-7: | ECC bytes             |
|                           | Bytes 8-11:    | LevelX Metadata       |
|                           | Bytes 12-15:   | Unused                |
|                           | Byte 5:        | Bad block flag        |
| 64                        | Byte 0:        | Bad block flag        |
|                           | Bytes 2-5:     | LevelX Metadata       |
|                           | Bytes 6-39:    | Unused                |
|                           | Bytes 40-63:   | ECC bytes             |

LevelX Utilizes 4 of the spare bytes of each NAND page for keeping track of the data stored in the each page. These 4 bytes are used to implement a 32-bit unsigned integer with a LevelX proprietary format. The upper 4 bits of the 32-bit field (bit 31 to bit 28) are used to indicate the data type stored in the page. The data type can be user sector data and metadata. If the data type indicate it as user sector data, the bit 28-0 of this field stores the logic sector number. If the data type indicate it as metadata, the bit 7-0 of this field stores the metadata page number. The format of the spare bytes is shown below.

**LevelX Spare Bytes Format**

| Bit(s) | Meaning |
| ------ | ------- |
| 31-29  | Data type. Specify the data type of the data stored in the page. It can be LevelX metadata or user sector data.  |
| 28-0   | Logical sector number, if the data type is user sector data. |
| 7-0    | Metadata page number if data type is metadata. |

LevelX utilizes a few NAND blocks for the metadata. The metadata blocks are used to store the mapping information between the logical block and the physical block and other tables used to keep track of the blocks and pages.

## NAND Bad Block Support

NAND memory is also more likely to have bad blocks than NOR memory. This is largely because NAND manufacturers can increase yield by allowing bad blocks and requiring software to work-around such bad blocks. LevelX handles NAND bad block management by simply mapping around bad blocks.

LevelX also provides APIs for 256-byte Hamming Error Correction Codes (ECC) for the underlying LevelX driver to utilize for calculating new ECC codes or to perform 1-bit error correction on page reading within each 256-byte section of the page.

## NAND Driver Requirements

LevelX requires an underlying NAND flash driver that is specific to the underlying flash part and hardware implementation. The driver is specified to LevelX during initialization via the API ***lx_nand_flash_open*** and ***lx_nand_flash_format***. If **LX_NAND_ENABLE_CONTROL_BLOCK_FOR_DRIVER_INTERFACE** is defined, the driver services functions will have pointer to the NAND control block as the first parameter. The driver may access the user extensions in the control block for driver specific data. The prototype of the LevelX driver is as follows.

```c
INT nand_driver_initialize(LX_NAND_FLASH *instance);
```

The *instance* parameter specifies the LevelX NAND control block. The driver initialization function is responsible for setting up all the other driver-level services for the associated LevelX instance. The services required for each LevelX NAND instance are shown in the list below.

- Read Pages
- Write Pages
- Copy Pages
- Block Erase
- Block Erased Verify
- Page Erased Verify
- Block Status Get
- Block Status Set
- System Error Handler

## Driver Initialization

These services are setup via setting function pointers in the **LX_NAND_FLASH** instance within the driver's initialization function. The driver initialization function also specifies the total number of block, pages per block, bytes per page, spare data offsets and lengths and total spare byte length. There are two spare data stored in each page, spare data 1 and spare data 2. The spare data 1 is used to store data type as described above and must be at least 4 bytes and ECC protected. The spare data 2 is optional and may be used to indicate the start of metadata blocks. The driver initialization function likely also performs additional device and/or implementation-specific initialization duties before returning **LX_SUCCESS**.

## Driver Read Pages

The LevelX NAND driver "read pages" service is responsible for reading one or more pages in a specific block of the NAND flash. All error checking and correcting logic is the responsibility of the driver service. If successful, the LevelX NAND driver returns **LX_SUCCESS**. If not successful, the LevelX NAND driver returns **LX_ERROR**. The prototype of the LevelX NAND driver "read pages" service is given below.

```c
INT nand_driver_read_pages(
    LX_NAND_FLASH *nand_flash,
    ULONG block,
    ULONG page,
    UCHAR* main_buffer,
    UCHAR* spare_buffer,
    ULONG pages);
```

Where *block* and *page* identify the first page to read and *main_buffer* and *spare_buffer* specify where to place the page contents and spare data. And *pages* specify how many pages to read. The *nand_flash* parameter is provided for driver to access the NAND control block if needed.

## Driver Write Pages

The LevelX NAND driver "write pages" service is responsible for writing one or more pages into the specified block of the NAND flash. All error checking and ECC computation is the responsibility of the driver service. If successful, the LevelX NAND driver returns **LX_SUCCESS**. If not successful, the LevelX NAND driver returns **LX_ERROR**. The prototype of the LevelX NAND driver "write pages" service is shown below.

```c
INT nand_driver_write_pages(
    LX_NAND_FLASH *nand_flash,
    ULONG block, 
    ULONG page,
    UCHAR* main_buffer,
    UCHAR* spare_buffer,
    ULONG pages);
```

Where *block* and *page* identify the first page to write and *main_buffer* and *spare_buffer* specify the source of the page contents and spare data. And *pages* specify how many pages to write. The *nand_flash* parameter is provided for driver to access the NAND control block if needed.

> **Note:** LevelX relies on the driver for low-level error detection when writing to the flash page, which typically involves reading back the page and comparing with the write buffer to ensure the write was successful.

## Driver Copy Pages

The LevelX NAND driver "copy pages" service is responsible for copying one or more pages from one block of the NAND flash to another block of the NAND flash. All error checking and ECC computation is the responsibility of the driver service. If successful, the LevelX NAND driver returns **LX_SUCCESS**. If not successful, the LevelX NAND driver returns **LX_ERROR**. The prototype of the LevelX NAND driver "copy pages" service is shown below.

```c
INT nand_driver_copy_pages(
    LX_NAND_FLASH *nand_flash,
    ULONG source_block,  
    ULONG source_page,
    ULONG destination_block,
    ULONG destination_page,
    ULONG pages,
    UCHAR *data_buffer);   
```

Where *source_block* and *source_page* identify the first page to copy and *destination_block* and *destination_page* specifies the first destination block and page. And *pages* specify how many pages to copy. The parameter *data_buffer* is provided for driver to store one page and spare data for copy operation if the driver needs to read the source page before writing to the destination page.

## Driver Block Erase

The LevelX NAND driver "block erase" service is responsible for erasing the specified block of the NAND flash. If successful, the LevelX NAND driver returns **LX_SUCCESS**. If not successful, the LevelX NAND driver returns **LX_ERROR**. The prototype of the LevelX NAND driver "block erase" service is as follows.

```c
INT nand_driver_block_erase(
    LX_NAND_FLASH *nand_flash,
    ULONG block,  
    ULONG erase_count);
```

Where *block* identifies which block to erase. The parameter *erase_count* is provided for diagnostic purposes. For example, the driver may want to alert another portion of the application software when the erase count exceeds a specific threshold. The *nand_flash* parameter is provided for driver to access the NAND control block if needed.

> **Note:** LevelX relies on the driver for low-level error detection when the block is erased, which typically involves ensuring that all pages of the block are all ones.

## Driver Block Erased Verify

The LevelX NAND driver "block erased verify" service is responsible for verifying that the specified block of the NAND flash is erased. If it is erased, the LevelX NAND driver returns **LX_SUCCESS**. If the block is not erased, the LevelX NAND driver returns **LX_ERROR**. The prototype of the LevelX NAND driver "block erased verify" service is:

```c
INT nand_driver_block_erased_verify(
    LX_NAND_FLASH *nand_flash,
    ULONG block);
```

Where *block* specifies which block to verify that it is erased. The *nand_flash* parameter is provided for driver to access the NAND control block if needed.

> **Note:** LevelX relies on the driver to examine all pages and all bytes of each page – including spare and data bytes – to ensure they are erased (contain all ones).

## Driver Page Erased Verify

The LevelX NAND driver "page erased verify" service is responsible for verifying that the specified page of the specified block of the NAND flash is erased. If it is erased, the LevelX NAND driver returns **LX_SUCCESS**. If the page is not erased, the LevelX NAND driver returns **LX_ERROR**. The prototype of the LevelX NAND driver "page erased verify" service is:

```c
INT nand_driver_page_erased_verify(
    LX_NAND_FLASH *nand_flash,
    ULONG block,  
    ULONG page);
```
Where *block* specifies which block and *page* specifies the page to verify that it is erased. The *nand_flash* parameter is provided for driver to access the NAND control block if needed.

> **Note:** LevelX relies on the driver to examine all bytes of the specified page – including spare and data bytes – to ensure they are erased (contain all ones).

## Driver Block Status Get

The LevelX NAND driver "block status get" service is responsible for retrieving the bad block flag of the specified block of the NAND flash. If it is successful, the LevelX NAND driver returns **LX_SUCCESS**. If it is not successful, the LevelX NAND driver returns **LX_ERROR**. The prototype of the LevelX NAND driver "block status get" service is: shown below.

```c
INT nand_driver_block_status_get(
    LX_NAND_FLASH *nand_flash,
    ULONG block,  
    UCHAR *bad_block_byte);
```

Where *block* specifies which block and *bad_block_byte* specifies the destination for the bad block flag. The *nand_flash* parameter is provided for driver to access the NAND control block if needed.

## Driver Block Status Set

The LevelX NAND driver "block status set" service is responsible for setting the bad block flag of the specified block of the NAND flash. If it is successful, the LevelX NAND driver returns **LX_SUCCESS**. If it is not successful, the LevelX NAND driver returns **LX_ERROR**. The prototype of the LevelX NAND driver "block status set" service is:

```c
INT nand_driver_block_status_set(
    LX_NAND_FLASH *nand_flash,
    ULONG block,
    UCHAR bad_block_byte);
```

Where *block* specifies which block and *bad_block_byte* specifies the value of the bad block flag. The *nand_flash* parameter is provided for driver to access the NAND control block if needed.

## Driver System Error

The LevelX NAND driver "system error handler" service is responsible for setting handling system errors detected by LevelX. The processing in this routine is application dependent. If it is successful, the LevelX NAND driver returns **LX_SUCCESS**. If it is not successful, the LevelX NAND driver returns **LX_ERROR**. The prototype of the LevelX NAND driver "system error" service is:

```c
INT nand_driver_system_error(
    LX_NAND_FLASH *nand_flash,
    UINT error_code,  
    ULONG block, 
    ULONG page);
```

Where *block* specifies which block, and *page* specifies the specific page the error represented by *error_code* occurred. The *nand_flash* parameter is provided for driver to access the NAND control block if needed.

## NAND Simulated Driver

LevelX provides a simulated NAND flash driver that simply uses RAM to simulate the operation of a NAND flash part. By default, the NAND simulated driver provides 8 NAND flash blocks with 16 pages per block and 2048 bytes per page.

The simulated NAND flash driver initialization function is ***lx_nand_flash_simulator_initialize*** and is defined in ***lx_nand_flash_simulator.c***. This driver also provides a good template for writing specific NAND flash drivers.

## NAND FileX Integration

As mentioned earlier, LevelX does not rely on FileX for operation. All the LevelX APIs may be called directly by the application software to store/retrieve raw data to the logical sectors provided by LevelX. However, LevelX also supports FileX.

The file ***fx_nand_flash_simulated_driver.c*** contains an example FileX driver for use with the NAND flash simulation. An interesting aspect of this driver is that it combines 512-byte logical sectors typically used by FileX into single logical sector read/write requests to the LevelX simulator using 2048-byte pages. This results in more efficient use of the NAND flash memory. The NAND flash FileX driver for LevelX provides a good starting point for writing custom FileX drivers.  
  
> **Note:** The FileX NAND flash format should be one full block size of sectors less than the NAND flash provides. This will help ensure best performance during the wear level processing. Additional techniques to improve write performance in the LevelX wear leveling algorithm include the following.
> 
> 1. Ensure that all writes are exactly one or more clusters in size and start on exact cluster boundaries.
> 1. Pre-allocate clusters before performing large file write operations via the FileX ***fx_file_allocate*** class of APIs.
> 1. Ensure the FileX driver is enabled to receive release sector information and requests made to the driver to release sectors are handled in the driver by calling ***lx_nor_flash_sector_release***.

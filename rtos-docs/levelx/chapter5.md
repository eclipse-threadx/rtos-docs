---
title: Chapter 5 - LevelX NOR support
description: NOR flash memory is composed of blocks that are typically evenly divisible by 512 bytes. LevelX divides each NOR flash block into 512-byte logical sectors.
---

# Chapter 5 - LevelX NOR support

NOR flash memory is composed of *blocks* that are typically evenly divisible by 512 bytes. There are no concept of a flash *page* in NOR flash memory. Also, there are no *spare* bytes in NOR flash memory, hence LevelX must use the NOR flash memory itself for all management information. Direct read access is possible in NOR flash memory. Write access typically requires a special sequence of operations. NOR flash memory may be written to multiple times, providing that bits are being cleared. Bits in NOR flash memory can only be set once, via the erase block operation.

LevelX divides each NOR flash block into 512-byte logical *sectors*. Furthermore, LevelX uses the first "n" sectors of each NOR flash block to store management information. The format of the LevelX NOR flash memory management information is:

**LevelX NOR Block Format**

| Byte Offset  | Contents                     |
| ------------ | ---------------------------- |
| 0            | [Block Erase Count]          |
| 4            | [Minimum Mapped Sector]      |
| 8            | [Maximum Mapped Sector]      |
| 12           | [Free Sector Bit Map]        |
| m            | [Sector 0 Mapping Entry]     |
|              | …                            |
| m+4*(n-1)    | [Sector "n" Mapping Entry]   |
|              | …                            |
| s            | [Sector 0 Contents]          |
|              | …                            |
| s+512*(n-1) | [Sector "n" Contents]         |

The 32-bit *Block Erase Count* contains the number of times the block has been erased. The main goal of LevelX is to keep the erase count of all blocks relatively close to help prevent any one block from wearing out prematurely. The 32-bit *Minimum Mapped Sector* and *Maximum Mapped Sector* fields are written only when all the logical sectors in the block have been mapped and written to. These fields are useful for optimization of the sector read operation. The *Free Sector Bit Map* entry is a bit map where each set bit corresponds to an unmapped sector in the block. This field is used to make the free sector search more efficient. This is a variable length field that requires one word for every 32 sectors in the block. The *Sector Mapping Entry* array contains mapping information for each sector in the block. Each entry has the following format:

**Sector Mapping Entry**

| Bit(s) | Meaning  |
| ------ | -------- |
| 31     | Valid flag. When set and logical sector not all ones indicates mapping is valid |
| 30     | Obsolete flag. When clear, this mapping is either obsolete or is in the process of becoming obsolete. |
| 29     | Mapping entry write is complete when this bit is 0 |
| 0-28   | Logical sector mapped to this physical sector—when not all ones. |

The upper bit of the 32-bit field (bit 31) is used to indicate the logical sector mapping is valid. If this bit is 0, the information in this entry (and corresponding sector contents) is no longer valid. The next bit - bit 30 - is used to indicate this sector is in the process of becoming obsolete and a new sector is being written. Bit 29 is used to indicate when the mapping entry write is complete. If bit 29 is 0, the mapping entry write is complete. If bit 29 is set, the mapping entry was in the process of being written. Bits 30 and 29 are used in recovering from a potential power loss while updating a new sector mapping. Finally, the lower 29-bits (28-0) contain the logical sector number for the sector. If a sector has not been mapped, all bits will be set, i.e., it will have a value of 0xFFFFFFFF.

## NOR Driver Requirements

LevelX requires an underlying NOR flash driver that is specific to the underlying flash part and hardware implementation. The driver is specified to LevelX during initialization via the API ***lx_nor_flash_open***. If **LX_NOR_ENABLE_CONTROL_BLOCK_FOR_DRIVER_INTERFACE** is defined, the driver services functions will have pointer to the NOR control block as the first parameter. The driver may access the user extensions in the control block for driver specific data. The prototype of the LevelX driver is:

```c
INT nor_driver_initialize(LX_NOR_FLASH *instance);
```

The "*instance*" parameter specifies the LevelX NOR control block. The driver initialization function is responsible for setting up all the other driver-level services for the associated LevelX instance. The services required for each LevelX NOR instance are:

- Read Sector
- Write Sector
- Block Erase
- Block Erased Verify
- System Error Handler

## Driver Initialization

These services are setup via setting function pointers in the **LX_NOR_FLASH** instance within the driver's initialization function. The driver initialization function also is responsible for:

1. Specifying the base address of the flash.
1. Specifying the total number of blocks and the number of words per block.
1. A RAM buffer for reading one sector of flash (512 bytes) and aligned for ULONG access.

The driver initialization function likely also performs additional device and/or implementation-specific initialization duties before returning **LX_SUCCESS**.

## Driver Read Sector

The LevelX NOR driver "read sector" service is responsible for reading a specific sector in a specific block of the NOR flash. All error checking and correcting logic is the responsibility of the driver service. If successful, the LevelX NOR driver returns **LX_SUCCESS**. If not successful, the LevelX NOR driver returns *LX_ERROR*. The prototype of the LevelX NOR driver "read sector" service is:

```c
INT nor_driver_read_sector(
    LX_NOR_FLASH *nor_flash,
    ULONG *flash_address,
    ULONG *destination, 
    ULONG words);
```

Where "*flash_address*" specifies the address of a logical sector within a NOR flash block of memory and "*destination*" and "*words*" specify where to place the sector contents and how many 32-bit words to read. The *nor_flash* parameter is provided for driver to access the NOR control block if needed.

## Driver Write Sector

The LevelX NOR driver "write sector" service is responsible for writing a specific sector into a block of the NOR flash. All error checking is the responsibility of the driver service. If successful, the LevelX NOR driver returns **LX_SUCCESS**. If not successful, the LevelX NOR driver returns **LX_ERROR**. The prototype of the LevelX NOR driver "write sector" service is:

```c
INT nor_driver_write_sector(
    LX_NOR_FLASH *nor_flash,
    ULONG *flash_address,
    ULONG *source, 
    ULONG words);
```

Where "*flash_address*" specifies the address of a logical sector within a NOR flash block of memory and "*source*" and "*words*" specify the source of the write and how many 32-bit words to write. The *nor_flash* parameter is provided for driver to access the NOR control block if needed.

> **Note:** LevelX relies on the driver to verify that the write sector was successful. This is typically done by reading back the programmed value to ensure it matches the requested value to be written.

## Driver Block Erase

The LevelX NOR driver "block erase" service is responsible for erasing the specified block of the NOR flash. If successful, the LevelX NOR driver returns **LX_SUCCESS**. If not successful, the LevelX NOR driver returns **LX_ERROR**. The prototype of the LevelX NOR driver "block erase" service is:

```c
INT nor_driver_block_erase(
    LX_NOR_FLASH *nor_flash,
    ULONG block,  
    ULONG erase_count);
```

Where "*block*" identifies which NOR block to erase. The parameter "*erase_count*" is provided for diagnostic purposes. For example, the driver may want to alert another portion of the application software when the erase count exceeds a specific threshold. The *nor_flash* parameter is provided for driver to access the NOR control block if needed.

> **Note:** LevelX relies on the driver to examine all bytes of the block to ensure they are erased (contain all ones).

## Driver Block Erased Verify

The LevelX NOR driver "block erased verify" service is responsible for verifying that the specified block of the NOR flash is erased. If it is erased, the LevelX NOR driver returns **LX_SUCCESS**. If the block is not erased, the LevelX NOR driver returns **LX_ERROR**. The prototype of the LevelX NOR driver "block erased verify" service is:

```c
INT nor_driver_block_erased_verify(LX_NOR_FLASH *nor_flash, ULONG block);
```

Where "*block*" specifies which block to verify that it is erased. The *nor_flash* parameter is provided for driver to access the NOR control block if needed.

> **Note:** LevelX relies on the driver to examine all bytes of the specified to ensure they are erased (contain all ones).

## Driver System Error

The LevelX NOR driver "system error handler" service is responsible for setting handling system errors detected by LevelX. The processing in this routine is application dependent. If it is successful, the LevelX NOR driver returns **LX_SUCCESS**. If it is not successful, the LevelX NOR driver returns **LX_ERROR**. The prototype of the LevelX NOR driver "system error" service is:

```c
INT nor_driver_system_error(LX_NOR_FLASH *nor_flash, UINT error_code);
```

Where "*error_code*" represents the error that occurred. The *nor_flash* parameter is provided for driver to access the NOR control block if needed.

## NOR Simulated Driver

LevelX provides a simulated NOR flash driver that simply uses RAM to simulate the operation of a NOR flash part. By default, the NOR simulated driver provides 8 NOR flash blocks with 16 512-byte sectors per block.

The simulated NOR flash driver initialization function is ***lx_nor_flash_simulator_initialize*** and is defined in ***lx_nor_flash_simulator.c***. This driver also provides a good template for writing specific NOR flash drivers.

## NOR FileX Integration

As mentioned earlier, LevelX does not rely on FileX for operation. All the LevelX APIs may be called directly by the application software to store/retrieve raw data to the logical sectors provided by LevelX. However, LevelX also supports FileX.

The file ***fx_nor_flash_simulated_driver.c*** contains an example FileX driver for use with the NOR flash simulation. The NOR flash FileX driver for LevelX provides a good starting point for writing custom FileX drivers.

> **Note:** The FileX NOR flash format should be one full block size of sectors less than the NOR flash provides. This will help ensure best performance during the wear level processing. Additional techniques to improve write performance in the LevelX wear leveling algorithm include:
> 1. Ensure that all writes are exactly one or more clusters in size and start on exact cluster boundaries.
> 2. Pre-allocate clusters before performing large file write operations via the FileX ***fx_file_allocate*** class of APIs.
> 3.  Periodic use of ***lx_nor_flash_defragment*** to free up as many NOR blocks as possible and thus improve write performance.
> 4. Ensure the FileX driver is enabled to receive release sector information and requests made to the driver to release sectors are handled in the driver by calling ***lx_nor_flash_sector_release***.

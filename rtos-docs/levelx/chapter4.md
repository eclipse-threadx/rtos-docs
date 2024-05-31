---
title: Chapter 4 - LevelX NAND APIs
description: The LevelX NAND APIs available to the application.
---

# Chapter 4 - LevelX NAND APIs
The LevelX NAND API functions available to the application are as follows.

## NAND_Services
- [lx_nand_flash_close](#lx_nand_flash_close)
- [lx_nand_flash_initialize](#lx_nand_flash_initialize)
- [lx_nand_flash_open](#lx_nand_flash_open)
- [lx_nand_flash_page_ecc_check](#lx_nand_flash_page_ecc_check)
- [lx_nand_flash_page_ecc_compute](#lx_nand_flash_page_ecc_compute)
- [lx_nand_flash_sectors_read](#lx_nand_flash_sectors_read)
- [lx_nand_flash_sectors_release](#lx_nand_flash_sectors_release)
- [lx_nand_flash_sectors_write](#lx_nand_flash_sectors_write)

## lx_nand_flash_close

Close NAND flash instance

### Prototype

```c
UINT lx_nand_flash_close(LX_NAND_FLASH *nand_flash);
```

### Description

This service closes the previously opened NAND flash instance.

### Input Parameters

- *nand_flash*: NAND flash instance pointer.

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_ERROR**: (0x01) Error closing flash instance.

### Allowed From

Threads

### Example

```c
/* Close NAND flash instance "my_nand_flash". */
status = lx_nand_flash_close(&my_nand_flash);  
  
/* If status is LX_SUCCESS the request was successful. */
```

### See Also

- [levelx NAND Services](#NAND_Services)

## lx_nand_flash_defragment

Defragment NAND flash instance

### Prototype

```c
UINT lx_nand_flash_defragment(LX_NAND_FLASH *nand_flash);
```

### Description

This service is deprecated and not supported.

### Input Parameters

- *nand_flash*: NAND flash instance pointer.

### Return Values

- **LX_NOT_SUPPORTED**: (0x0C) Not supported.

### Allowed From

Threads

### Example

```c
/* Defragment NAND flash instance "my_nand_flash". */  
status = lx_nand_flash_defragment(&my_nand_flash);  
  
/* If status is LX_SUCCESS the request was successful. */
```

### See Also

- [levelx NAND Services](#NAND_Services)

## lx_nand_flash_extended_cache_enable

Enable/disable extended NAND cache

### Prototype

```c
UINT lx_nand_flash_extended_cache_enable(
    LX_NAND_FLASH
    *nand_flash,  
    VOID *memory, 
    ULONG size);
```

### Description

This service is deprecated and not supported.

### Input Parameters

- *nand_flash*: NAND flash instance pointer.  
- *memory*: Starting address for cache memory aligned for ULONG access. A value of LX_NULL disables the cache.  
- *size*: The size in bytes of the memory supplied.

### Return Values

- **LX_NOT_SUPPORTED**: (0x0C) Not supported.

### Allowed From

Threads

### Example

```c
/* Enable the NAND flash cache for the instance "my_nand_flash". */
status = lx_nand_flash_extended_cache_enable(&my_nand_flash,  
    &my_memory, sizeof(my_memory));  
  
/* If status is LX_SUCCESS the request was successful. */
```

### See Also

- [levelx NAND Services](#NAND_Services)

## lx_nand_flash_format

Format NAND flash instance

### Prototype

```c
UINT  _lx_nand_flash_format(LX_NAND_FLASH* nand_flash, CHAR* name,
                            UINT(*nand_driver_initialize)(LX_NAND_FLASH*),
                            ULONG* memory_ptr, UINT memory_size);
```

### Description

This service erase the NAND flash and formats the NAND flash instance with the specified name, driver initialization function. Note that the driver initialization function is responsible for installing various function pointers for reading, writing, and erasing blocks/pages of the NAND hardware associated with this NAND flash instance. 

### Input Parameters

- *nand_flash*: NAND flash instance pointer.
- *name*: Name of NAND flash instance.
- *nand_driver_initialize*: Function pointer to NAND flash driver initialization function. Please refer to Chapter 3 of this guide for more details on NAND flash driver responsibilities.
- *memory_ptr*: Pointer to working memory for LevelX.
- *memory_size*: Size of working memory for LevelX. The size must be at least 7 * total block count + 2 * page size.

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_ERROR**: (0x01) Error formatting NAND flash instance.
- **LX_NO_BLOCKS**: (0x0B) Not enough blocks to format NAND flash instance.

### Allowed From

Threads

### Example

```c
/* Format NAND flash instance "my_nand_flash". */
status = lx_nand_flash_format(&my_nand_flash, "my nand flash",  
    my_nand_driver_initialize, &my_memory, sizeof(my_memory));

/* If status is LX_SUCCESS the NAND is erased and initial format is built, it can be opened later by calling lx_nand_flash_open. */
```

### See Also

- [levelx NAND Services](#NAND_Services)

## lx_nand_flash_initialize

Initialize NAND flash support

### Prototype

```c
UINT lx_nand_flash_initialize(void);
```

### Description

This service initializes LevelX NAND flash support. It must be called before any other LevelX NAND APIs.

### Input Parameters

- **None**

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_ERROR**: (0x01) Error initializing NAND flash support.

### Allowed From

Initialization, Threads

### Example

```c
/* Initialize NAND flash support. */
status = lx_nand_flash_initialize();  
  
/* If status is LX_SUCCESS the request was successful. */
```

### See Also

- [levelx NAND Services](#NAND_Services)

## lx_nand_flash_open

Open NAND flash instance

### Prototype

```c
UINT lx_nand_flash_open(
    LX_NAND_FLASH *nand_flash, 
    CHAR *name,  
    UINT (*nand_driver_initialize) (LX_NAND_FLASH *),
    ULONG* memory_ptr, 
    UINT memory_size);
```

### Description

This service opens a NAND flash instance with the specified NAND flash control block and driver initialization function. Note that the driver initialization function is responsible for installing various function pointers for reading, writing, and erasing blocks/pages of the NAND hardware associated with this NAND flash instance.

### Input Parameters

- *nand_flash*: NAND flash instance pointer.
- *name*: Name of NAND flash instance.
- *nand_driver_initialize*: Function pointer to NAND flash driver initialization function. Please refer to Chapter 3 of this guide for more details on NAND flash driver responsibilities.
- *memory_ptr*: Pointer to working memory for LevelX.
- *memory_size*: Size of working memory for LevelX. The size must be at least 7 * total block count + 2 * page size.

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_ERROR**: (0x01) Error opening NAND flash instance.
- **LX_NO_MEMORY**: (0x08) Driver did not provide buffer for reading one page into RAM.

### Allowed From

Threads

### Example

```c
/* Open NAND flash instance "my_nand_flash" with the driver "my_nand_driver_initialize". */ 
status = lx_nand_flash_open(&my_nand_flash,"my nand flash",  
    my_nand_driver_initialize, &my_memory, sizeof(my_memory));  
  
/* If status is LX_SUCCESS the request was successful. */
```

### See Also

- lx_nand_flash_close
- lx_nand_flash_initialize
- lx_nand_flash_page_ecc_check
- lx_nand_flash_page_ecc_compute
- lx_nand_flash_sectors_read
- lx_nand_flash_sectors_release
- lx_nand_flash_sectors_write

## lx_nand_flash_page_ecc_check

Check page for ECC errors with correction

### Prototype

```c
UINT lx_nand_flash_page_ecc_check(
    LX_NAND_FLASH *nand_flash,  
    UCHAR *page_buffer, 
    UCHAR *ecc_buffer);
```

### Description

This service verifies the integrity of the supplied NAND page buffer with the supplied ECC. Page size (defined in the NAND flash instance pointer) is assumed to be a multiple of 256-bytes and the ECC code supplied is capable of correcting a 1 bit error in each 256-byte portion of the page.

### Input Parameters

- *nand_flash*: NAND flash instance pointer.
- *page_buffer*: Pointer to NAND flash page buffer.
- *ecc_buffer*: Pointer to ECC for NAND flash page. Note that there are 3 ECC bytes per 256-byte portion of the page.

### Return Values

- **LX_SUCCESS**: (0x00) NAND page has no errors.
- **LX_NAND_ERROR_CORRECTED**: (0x06) One or more 1-bit errors were corrected in the NAND page—correction(s) are in the page buffer.
- **LX_NAND_ERROR_NOT_CORRECTED**: (0x07) Too many errors to correct on the NAND page.

### Allowed From

LevelX Driver

### Example

```c
/* Check the NAND page pointed to by "page_pointer" of the NAND flash instance "my_nand_flash" . */
status = lx_nand_flash_page_ecc_check(&my_nand_flash, page_pointer, ecc_pointer);  
  
/* If status is LX_SUCCESS the page is valid. */
```

### See Also

- [levelx NAND Services](#NAND_Services)

## lx_nand_flash_page_ecc_compute

Compute ECC for page

### Prototype

```c
UINT lx_nand_flash_page_ecc_compute(
    LX_NAND_FLASH *nand_flash,  
    UCHAR *page_buffer, 
    UCHAR *ecc_buffer);
```

### Description

This service computes the ECC of the supplied NAND page buffer and returns the ECC in the supplied ECC buffer. Page size is assume to be a multiple of 256-bytes (defined in the NAND flash instance pointer). The ECC code is used to verify the integrity of the page when it is read at a later time.

### Input Parameters

- *nand_flash*: NAND flash instance pointer.
- *page_buffer*: Pointer to NAND flash page buffer.
- *ecc_buffer*: Pointer to destination for ECC of the NAND flash page. Note that must be 3 bytes of ECC storage per 256-byte portion of the page. For example, a 2048 byte page would require 24 bytes for the ECC.

### Return Values

- **LX_SUCCESS**: (0x00) ECC successfully calculated.
- **LX_ERROR**: (0x01) Error calculating ECC.

### Allowed From

LevelX Driver

### Example

```c
/* Compute ECC for the NAND page pointed to by "page_pointer" of the NAND flash instance "my_nand_flash". */  
status = lx_nand_flash_page_ecc_compute(&my_nand_flash, page_pointer, ecc_pointer);  
  
/* If status is LX_SUCCESS the ECC was calculated and Can be found in the memory pointed to by "ecc_pointer." */
```

### See Also

- [levelx NAND Services](#NAND_Services)

## lx_nand_flash_partial_defragment

Partial defragment of NAND flash instance

### Prototype

```c
UINT lx_nand_flash_partial_defragment(
    LX_NAND_FLASH *nand_flash,  
    UINT max_blocks);
```

### Description

This service is deprecated and not supported.

### Input Parameters

- *nand_flash*: NAND flash instance pointer.
- *max_blocks*: Maximum number of blocks.

### Return Values

- **LX_NOT_SUPPORTED**: (0x0C) Not supported.

### Allowed From

Threads

### Example

```c
/* Defragment 1 block of NAND flash instance "my_nand_flash". */  
status = lx_nand_flash_partial_defragment(&my_nand_flash, 1);  
  
/* If status is LX_SUCCESS the request was successful. */
```

### See Also

- [levelx NAND Services](#NAND_Services)

## lx_nand_flash_sector_read

Read NAND flash sector

### Prototype

```c
UINT lx_nand_flash_sector_read(
    LX_NAND_FLASH *nand_flash,  
    ULONG logical_sector, 
    VOID *buffer);
```

### Description

This service reads the logical sector from the NAND flash instance and if successful returns the contents in the supplied buffer. Note that NAND sector size is always the page size of the underlying NAND hardware.

### Input Parameters

- *nand_flash*: NAND flash instance pointer.
- *logical_sector*: Logical sector to read.
- *buffer*: Pointer to destination for contents of the logical sector. Note that the buffer is assumed to be the size of the NAND flash page size and aligned for ULONG access.

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_ERROR**: (0x01) Error reading NAND flash sector.

### Allowed From

Threads

### Example

```c
/* Read logical sector 20 of the NAND flash instance "my_nand_flash" and place contents in "buffer". */
status = lx_nand_flash_sector_read(&my_nand_flash, 20, buffer);  
  
/* If status is LX_SUCCESS, "buffer" contains the contents of logical sector 20. */
```

### See Also

- [levelx NAND Services](#NAND_Services)

## lx_nand_flash_sectors_read

Read multiple NAND flash sectors

### Prototype

```c
UINT lx_nand_flash_sectors_read(
    LX_NAND_FLASH *nand_flash,  
    ULONG logical_sector, 
    VOID *buffer, 
    ULONG sector_count);
```

### Description

This service reads the specified number of logical sectors from the NAND flash instance and if successful returns the contents in the supplied buffer. Note that NAND sector size is always the page size of the underlying NAND hardware.

### Input Parameters

- *nand_flash*: NAND flash instance pointer.
- *logical_sector*: Logical sector to read.
- *buffer*: Pointer to destination for contents of the logical sector. Note that the logical sector size is the same as NAND flash page size and the buffer is assumed to be large enough to hold all the sectors to read and aligned for ULONG access.
- *sector_count*: Number of logical sectors to read.

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_ERROR**: (0x01) Error reading NAND flash sector.

### Allowed From

Threads

### Example

```c
/* Read 10 logical sectors starting at logical sector 20 of the NAND flash instance "my_nand_flash" and place contents in "buffer". */
status = lx_nand_flash_sectors_read(&my_nand_flash, 20, buffer, 10);

/* If status is LX_SUCCESS, "buffer" contains the contents of logical sectors 20 through 29. */
```


## lx_nand_flash_sector_release

Release NAND flash sector

### Prototype

```c
UINT lx_nand_flash_sector_release(
    LX_NAND_FLASH *nand_flash,
    ULONG logical_sector);
```

### Description

This service releases the logical sector mapping in the NAND flash instance. Releasing a logical sector when not used makes the LevelX wear leveling more efficient.

### Input Parameters

- *nand_flash*: NAND flash instance pointer.
- *logical_sector*: Logical sector to release.

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_ERROR**: (0x01) Error NAND flash sector write.

### Allowed From

Threads

### Example

```c
/* Release logical sector 20 of the NAND flash instance "my_nand_flash". */  
status = lx_nand_flash_sector_release(&my_nand_flash, 20);  
  
/* If status is LX_SUCCESS, logical sector 20 has been released. */
```

### See Also

- [levelx NAND Services](#NAND_Services)

## lx_nand_flash_sectors_release

Release multiple NAND flash sectors

### Prototype

```c
UINT lx_nand_flash_sectors_release(
    LX_NAND_FLASH *nand_flash,
    ULONG logical_sector,
    ULONG sector_count);
```

### Description

This service releases the specified number of logical sectors in the NAND flash instance. Releasing a logical sector when not used makes the LevelX wear leveling more efficient.

### Input Parameters

- *nand_flash*: NAND flash instance pointer.
- *logical_sector*: Logical sector to release.
- *sector_count*: Number of logical sectors to release.

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_ERROR**: (0x01) Error NAND flash sector write.

### Allowed From

Threads

### Example

```c
/* Release 10 logical sectors starting at logical sector 20 of the NAND flash instance "my_nand_flash". */
status = lx_nand_flash_sectors_release(&my_nand_flash, 20, 10);

/* If status is LX_SUCCESS, logical sectors 20 through 29 have been released. */
```

### See Also

- [levelx NAND Services](#NAND_Services)

## lx_nand_flash_sector_write

Write NAND flash sector

### Prototype

```c
UINT lx_nand_flash_sector_write(
    LX_NAND_FLASH *nand_flash,
    ULONG logical_sector, 
    VOID *buffer);
```

### Description

This service writes the specified logical sector in the NAND flash instance.

### Input Parameters

- *nand_flash*: NAND flash instance pointer.
- *logical_sector*: Logical sector to write.
- *buffer*: Pointer to the contents of the logical sector. Note that the buffer is assumed to be the size of the NAND flash page size and aligned for ULONG access.

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_NO_SECTORS**: (0x02) No more free sectors are available to perform the write.
- **LX_ERROR**: (0x01) Error writing NAND flash sector.

### Allowed From

Threads

### Example

```c
/* Write logical sector 20 of the NAND flash instance "my_nand_flash" with the contents pointed to by "buffer". */  
status = lx_nand_flash_sector_write(&my_nand_flash, 20, buffer);  
  
/* If status is LX_SUCCESS, logical sector 20 has been written with the contents of "buffer". */
```

### See Also

- [levelx NAND Services](#NAND_Services)

## lx_nand_flash_sectors_write

Write multiple NAND flash sectors

### Prototype

```c
UINT lx_nand_flash_sectors_write(
    LX_NAND_FLASH *nand_flash,
    ULONG logical_sector, 
    VOID *buffer, 
    ULONG sector_count);
```

### Description

This service writes the specified number of logical sectors in the NAND flash instance.

### Input Parameters

- *nand_flash*: NAND flash instance pointer.
- *logical_sector*: Logical sector to write.
- *buffer*: Pointer to the contents of the logical sector. Note that the logical sector size is the same as NAND flash page size and the buffer is assumed to be large enough to hold all the sectors to write and aligned for ULONG access.
- *sector_count*: Number of logical sectors to write.

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_NO_SECTORS**: (0x02) No more free sectors are available to perform the write.
- **LX_ERROR**: (0x01) Error writing NAND flash sector.

### Allowed From

Threads

### Example

```c
/* Write 10 logical sectors starting at logical sector 20 of the NAND flash instance "my_nand_flash" with the contents pointed to by "buffer". */
status = lx_nand_flash_sectors_write(&my_nand_flash, 20, buffer, 10);

/* If status is LX_SUCCESS, logical sectors 20 through 29 have been written with the contents of "buffer". */
```

### See Also

- [levelx NAND Services](#NAND_Services)



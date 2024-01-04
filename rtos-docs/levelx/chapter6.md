---
title: Chapter 6 - LevelX NOR APIs
description: The LevelX NOR APIs available to the application.
---

# Chapter 6 - LevelX NOR APIs

The LevelX NOR API functions available to the application are as follows.

- ***lx_nor_flash_close***: *Close NOR flash instance*
- ***lx_nor_flash_defragment***: *Defragment NOR flash instance*
- ***lx_nor_flash_extended_cache_enable***: *Enable/disable extended NOR cache*
- ***lx_nor_flash_initialize***: *Initialize NOR flash support*
- ***lx_nor_flash_open***: *Open NOR flash instance*
- ***lx_nor_flash_partial_defragment***: *Partial defragment of NOR flash instance*
- ***lx_nor_flash_sector_read***: *Read NOR flash sector*
- ***lx_nor_flash_sector_release***: *Release NOR flash sector*
- ***lx_nor_flash_sector_write***: *Write NOR flash sector*

## lx_nor_flash_close

Close NOR flash instance

### Prototype

```c
UINT lx_nor_flash_close(LX_NOR_FLASH *nor_flash);
```

### Description

This service closes the previously opened NOR flash instance.

### Input Parameters

- *nor_flash*: NOR flash instance pointer.

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_ERROR**: (0x01) Error closing flash instance.

### Allowed From

Threads

### Example

```c
/* Close NOR flash instance "my_nor_flash". */  
status = lx_nor_flash_close(&my_nor_flash);  
  
/* If status is LX_SUCCESS the request was successful. */
```

### See Also

- lx_nor_flash_defragment
- lx_nor_flash_extended_cache_enable
- lx_nor_flash_initialize
- lx_nor_flash_open
- lx_nor_flash_partial_defragment
- lx_nor_flash_sector_read
- lx_nor_flash_sector_release
- lx_nor_flash_sector_write

## lx_nor_flash_defragment

Defragment NOR flash instance

### Prototype

```c
UINT lx_nor_flash_defragment(LX_NOR_FLASH *nor_flash);
```

### Description

This service defragments the previously opened NOR flash instance. The defragment process maximizes the number of free sectors and blocks.

### Input Parameters

- *nor_flash*: NOR flash instance pointer.

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_ERROR**: (0x01) Error defragmenting flash instance.

### Allowed From

Threads

### Example

```c
/* Defragment NOR flash instance "my_nor_flash". */  
status = lx_nor_flash_defragment(&my_nor_flash);  
  
/* If status is LX_SUCCESS the request was successful. */
```

### See Also

- lx_nor_flash_close
- lx_nor_flash_extended_cache_enable
- lx_nor_flash_initialize
- lx_nor_flash_open
- lx_nor_flash_partial_defragment
- lx_nor_flash_sector_read
- lx_nor_flash_sector_release
- lx_nor_flash_sector_write

## lx_nor_flash_extended_cache_enable

Enable/disable extended NOR cache

### Prototype

```c
UINT lx_nor_flash_extended_cache_enable(
    LX_NOR_FLASH *nor_flash,  
    VOID *memory, 
    ULONG size);
```

### Description

This service implements a NOR sector cache layer in RAM using the memory supplied by the application. Each 512 bytes of memory supplied translates to one NOR sector that can be cached. The sectors cached are those that contain the block control information, e.g., erase count, free sector map, and sector mapping information. Data sectors are not stored in this cache.

### Input Parameters

- **nor_flash**: NOR flash instance pointer.  
- **memory**: Starting address for cache memory, aligned for ULONG access. A value of LX_NULL disables the cache.  
- **size**: The size in bytes of the memory supplied (should be multiple of 512 bytes).

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_ERROR**: (0x01) Not enough memory for one NOR sector.
- **LX_DISABLED**: (0x09) NOR extended cache disabled by configuration option.

### Allowed From

Threads

### Example

```c
/* Enable the NOR flash cache for the instance "my_nor_flash". */  
status = lx_nor_flash_extended_cache_enable(&my_nor_flash,  
    &my_memory, sizeof(my_memory));  
  
/* If status is LX_SUCCESS the request was successful. */
```

### See Also

- lx_nor_flash_close
- lx_nor_flash_defragment
- lx_nor_flash_initialize
- lx_nor_flash_open
- lx_nor_flash_partial_defragment
- lx_nor_flash_sector_read
- lx_nor_flash_sector_release
- lx_nor_flash_sector_write

## lx_nor_flash_initialize

Initialize NOR flash support

### Prototype

```c
UINT lx_nor_flash_initialize(void);
```

### Description

This service initializes LevelX NOR flash support. It must be called before any other LevelX NOR APIs.

### Input Parameters

- **None**

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_ERROR**: (0x01) Error initializing NOR flash support.

### Allowed From

Initialization, Threads

### Example

```c
/* Initialize NOR flash support. */  
status = lx_nor_flash_initialize();  
  
/* If status is LX_SUCCESS the request was successful. */
```

### See Also

- lx_nor_flash_close
- lx_nor_flash_defragment
- lx_nor_flash_extended_cache_enable
- lx_nor_flash_partial_defragment
- lx_nor_flash_open
- lx_nor_flash_sector_read
- lx_nor_flash_sector_release
- lx_nor_flash_sector_write

## lx_nor_flash_open

Open NOR flash instance

### Prototype

```c
UINT lx_nor_flash_open(
    LX_NOR_FLASH *nor_flash, 
    CHAR *name,  
    UINT (*nor_driver_initialize) (LX_NOR_FLASH *));
```

### Description

This service opens a NOR flash instance with the specified NOR flash control block and driver initialization function. Note that the driver initialization function is responsible for installing various function pointers for reading, writing, and erasing blocks of the NOR hardware associated with this NOR flash instance.

### Input Parameters

- *nor_flash*: NOR flash instance pointer.
- *name*: Name of NOR flash instance.
- *nor_driver_initialize*: Function pointer to NOR flash driver Initialization function. Please refer to Chapter 5 of this guide for more details on NOR flash driver responsibilities.

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_ERROR**: (0x01) Error opening NOR flash instance.
- **LX_NO_MEMORY**:  (0x08) Driver did not provide buffer for reading none sector into RAM.

### Allowed From

Threads

### Example

```c
/* Open NOR flash instance "my_nor_flash" with the driver "my_nor_driver_initialize". */  
status = lx_nor_flash_open(&my_nor_flash,"my NOR flash",  
    my_nor_driver_initialize);  
  
/* If status is LX_SUCCESS the request was successful. */
```

### See Also

- lx_nor_flash_close
- lx_nor_flash_defragment
- lx_nor_flash_extended_cache_enable
- lx_nor_flash_initialize
- lx_nor_flash_partial_defragment
- lx_nor_flash_sector_read
- lx_nor_flash_sector_release
- lx_nor_flash_sector_write

## lx_nor_flash_partial_defragment

Partial defragment of NOR flash instance

### Prototype

```c
UINT lx_nor_flash_partial_defragment(
    LX_NOR_FLASH *nor_flash, 
    UINT max_blocks);
```

### Description

This service defragments the previously opened NOR flash instance up to the maximum number of blocks specified. The defragment process maximizes the number of free sectors and blocks.

### Input Parameters

- *nor_flash*: NOR flash instance pointer.
- *max_blocks*: Maximum number of blocks.

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_ERROR**: (0x01) Error defragmenting flash instance.

### Allowed From

Threads

### Example

```c
/* Defragment of one block in NOR flash instance* *"my_nor_flash". */  
status = lx_nor_flash_partial_defragment(&my_nor_flash, 1);  
  
/* If status is LX_SUCCESS the request was successful. */
```

### See Also

- lx_nor_flash_close
- lx_nor_flash_defragment
- lx_nor_flash_extended_cache_enable
- lx_nor_flash_initialize
- lx_nor_flash_open
- lx_nor_flash_sector_read
- lx_nor_flash_sector_release
- lx_nor_flash_sector_write

## lx_nor_flash_sector_read

Read NOR flash sector

### Prototype

```c
UINT lx_nor_flash_sector_read(
    LX_NOR_FLASH *nor_flash,  
    ULONG logical_sector, 
    VOID *buffer);
```

### Description

This service reads the logical sector from the NOR flash instance and if successful returns the contents in the supplied buffer. Note that NOR sector size is always 512 bytes.

### Input Parameters

- *nor_flash* NOR flash instance pointer.
- *logical_sector*: Logical sector to read.
- *buffer*: Pointer to destination for contents of the logical sector. Note that the buffer is assumed to be 512 bytes and aligned for ULONG access.

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_ERROR**: (0x01) Error reading NOR flash sector.

### Allowed From

Threads

### Example

```c
/* Read logical sector 20 of the NOR flash instance "my_nor_flash" and place contents in "buffer". */ 
status = lx_nor_flash_sector_read(&my_nor_flash, 20, buffer);  
  
/* If status is LX_SUCCESS, "buffer" contains the contents of logical sector 20. */
```

### See Also

- lx_nor_flash_close
- lx_nor_flash_defragment
- lx_nor_flash_extended_cache_enable
- lx_nor_flash_initialize
- lx_nor_flash_open
- lx_nor_flash_partial_defragment
- lx_nor_flash_sector_release
- lx_nor_flash_sector_write

## lx_nor_flash_sector_release

Release NOR flash sector

### Prototype

```c
UINT lx_nor_flash_sector_release(
    LX_NOR_FLASH *nor_flash,
    ULONG logical_sector);
```

### Description

This service releases the logical sector mapping in the NOR flash instance. Releasing a logical sector when not used makes the LevelX wear leveling more efficient.

### Input Parameters

- *nor_flash*: NOR flash instance pointer.
- *logical_sector*: Logical sector to release.

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_ERROR**: (0x01) Error NOR flash sector write.

### Allowed From

Threads

### Example

```c
/* Release logical sector 20 of the NOR flash instance "my_nor_flash". */  
status = lx_nor_flash_sector_release(&my_nor_flash, 20);  
  
/* If status is LX_SUCCESS, logical sector 20 has been released. */
```

### See Also

- lx_nor_flash_close
- lx_nor_flash_defragment
- lx_nor_flash_extended_cache_enable
- lx_nor_flash_initialize
- lx_nor_flash_open
- lx_nor_flash_partial_defragment
- lx_nor_flash_sector_read
- lx_nor_flash_sector_write

## lx_nor_flash_sector_write

Write NOR flash sector

### Prototype

```c
UINT lx_nor_flash_sector_write(
    LX_nor_FLASH *NOR_flash,
    ULONG logical_sector, 
    VOID *buffer);
```

### Description

This service writes the specified logical sector in the NOR flash instance.

### Input Parameters

- *nor_flash*: NOR flash instance pointer.
- *logical_sector*: Logical sector to write.
- *buffer*: Pointer to the contents of the logical sector. Note that the buffer is assumed to be 512 bytes aligned for ULONG access.

### Return Values

- **LX_SUCCESS**: (0x00) Successful request.
- **LX_NO_SECTORS**: (0x02) No more free sectors are available to perform the write.
- **LX_ERROR**: (0x01) Error releasing NOR flash sector.

### Allowed From

Threads

### Example

```c
/* Write logical sector 20 of the NOR flash instance "my_nor_flash" with the contents pointed to by "buffer". */ 
status = lx_nor_flash_sector_write(&my_nor_flash, 20, buffer);  
  
/* If status is LX_SUCCESS, logical sector 20 has been written with the contents of "buffer". */
```

### See Also

- lx_nor_flash_close
- lx_nor_flash_defragment
- lx_nor_flash_extended_cache_enable
- lx_nor_flash_initialize
- lx_nor_flash_open
- lx_nor_flash_partial_defragment
- lx_nor_flash_sector_read
- lx_nor_flash_sector_release

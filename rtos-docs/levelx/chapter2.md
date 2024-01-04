---
title: Chapter 2 - Installation and use of LevelX
description: Installation and use of LevelX is straightforward and described in the following sections of this chapter.
---

# Chapter 2 - Installation and use of LevelX

Installation and use of LevelX is straightforward and described in the following sections of this chapter.

## Distribution

LevelX is distributed in ANSI C where each function is contained in its own separate C file. The files in the LevelX distribution are as follows.
- lx_api.h
- lx_nand_flash_256byte_ecc_check.c
- lx_nand_flash_256byte_ecc_compute.c
- lx_nand_flash_block_allocate.c
- lx_nand_flash_block_data_move.c
- lx_nand_flash_block_find.c
- lx_nand_flash_block_mapping_set.c
- lx_nand_flash_block_status_set.c
- lx_nand_flash_close.c
- lx_nand_flash_data_page_copy.c
- lx_nand_flash_defragment.c
- lx_nand_flash_erase_count_set.c
- lx_nand_flash_extended_cache_enable.c
- lx_nand_flash_format.c
- lx_nand_flash_free_block_list_add.c
- lx_nand_flash_initialize.c
- lx_nand_flash_mapped_block_list_add.c
- lx_nand_flash_mapped_block_list_get.c
- lx_nand_flash_mapped_block_list_remove.c
- lx_nand_flash_memory_initialize.c
- lx_nand_flash_metadata_allocate.c
- lx_nand_flash_metadata_build.c
- lx_nand_flash_metadata_write.c
- lx_nand_flash_open.c
- lx_nand_flash_page_ecc_check.c
- lx_nand_flash_page_ecc_compute.c
- lx_nand_flash_partial_defragment.c
- lx_nand_flash_sectors_read.c
- lx_nand_flash_sectors_release.c
- lx_nand_flash_sectors_write.c
- lx_nand_flash_sector_read.c
- lx_nand_flash_sector_release.c
- lx_nand_flash_sector_write.c
- lx_nand_flash_system_error.c
- lx_nor_flash_block_reclaim.c
- lx_nor_flash_close.c
- lx_nor_flash_defragment.c  
- lx_nor_flash_extended_cache_enable.c
- lx_nor_flash_initialize.c
- lx_nor_flash_logical_sector_find.c
- lx_nor_flash_next_block_to_erase_find.c
- lx_nor_flash_open.c
- lx_nor_flash_partial_defragment.c
- lx_nor_flash_physical_sector_allocate.c
- lx_nor_flash_sector_mapping_cache_invalidate.c
- lx_nor_flash_sector_read.c
- lx_nor_flash_sector_release.c
- lx_nor_flash_sector_write.c
- lx_nor_flash_system_error.c

There are also simulator and FileX driver samples for both LevelX NAND and NOR instances, as follows.

- demo_filex_nand_flash.c  
- fx_nand_flash_simulated_driver.c
- lx_nand_flash_simulator.c
- demo_filex_nor_flash.c  
- fx_nor_flash_simulated_driver.c
- lx_nor_flash_simulator.c

Of course, if only NAND flash is required, only the LevelX NAND flash files (***lx_nand_\*.c***) are needed. Similarly, if only NOR flash is required, only the NOR flash files (***lx_nor_\*.c***) are needed.

## Configuration Options

LevelX can be configured at compile time via the conditional defines described below. Simply add the desired define to the compilation of each LevelX source to use the option.

- **LX_DIRECT_READ**:  Defined, this option bypasses the NOR flash driver read routine in favor or reading the NOR memory directly, resulting in a significant performance increase.
- **LX_FREE_SECTOR_DATA_VERIFY**: Defined, this causes the LevelX NOR instance open logic to verify free NOR sectors are all ones.
- **LX_NAND_SECTOR_MAPPING_CACHE_SIZE**:  By default this value is 16 and defines the logical sector mapping cache size. Large values improve performance, but cost memory. The minimum size is 8 and all values must be a power of 2.
- **LX_NAND_FLASH_DIRECT_MAPPING_CACHE**: Defined, this creates a direct mapping cache, such that there are no cache misses. It also requires that LX_NAND_SECTOR_MAPPING_CACHE_SIZE represents the exact number of total pages in your flash device.
- **LX_NOR_DISABLE_EXTENDED_CACHE**: Defined, this disabled the extended NOR cache.
- **LX_NOR_EXTENDED_CACHE_SIZE**: By default this value is 8, which represents a maximum of 8 sectors that can be cached in a NOR instance.
- **LX_NOR_SECTOR_MAPPING_CACHE_SIZE**: By default this value is 16 and defines the logical sector mapping cache size. Large values improve performance, but cost memory. The minimum size is 8 and all values must be a power of 2.
- **LX_THREAD_SAFE_ENABLE**: Defined, this makes LevelX thread-safe by using a ThreadX mutex object throughout the API.
- **LX_STANDALONE_ENABLE**: Defined, enables LevelX to be used in standalone mode (without Eclipse ThreadX). By default this symbol is not defined.

  > **Important:** When using LevelX in Standalone mode (**LX_STANDALONE_ENABLE** must be defined), ThreadX files/libraries are not required. LevelX thread-safe feature is disabled in this mode.

- **LX_NOR_FLASH_USER_EXTENSION**: Define user extension for NOR flash control block. User extension is placed at the end of flash control block and it is not cleared on opening flash.
- **LX_NAND_FLASH_USER_EXTENSION**: Define user extension for NAND flash control block. User extension is placed at the end of flash control block and it is not cleared on opening flash.
- **LX_NOR_ENABLE_MAPPING_BITMAP**: Determine if logical sector mapping bitmap should be enabled in extended cache. Cache memory will be allocated to sector mapping bitmap first. One bit can be allocated for each physical sector.
- **LX_NOR_ENABLE_OBSOLETE_COUNT_CACHE**: Determine if obsolete count cache should be enabled in extended cache. Cache memory will be allocated to obsolete count cache after the mapping bitmap if enabled, and the rest of the cache memory is allocated to sector cache.
- **LX_NOR_OBSOLETE_COUNT_CACHE_TYPE**: Defines obsolete count cache element size. If number of sectors per block is greater than 256, use USHORT instead of UCHAR.
- **LX_NOR_SECTOR_SIZE**: Define the logical sector size for NOR flash. The sector size is in units of 32-bit words. This sector size should match the sector size used in file system.

## Using LevelX

To use LevelX, either by itself or with FileX, include the file ***lx_api.h*** in the code that references the LevelX API. Also ensure that the LevelX object code is available at link time. Please examine the files ***demo_filex_nand_flash.c*** and ***demo_filex_nor_flash.c*** for examples of how to use LevelX.

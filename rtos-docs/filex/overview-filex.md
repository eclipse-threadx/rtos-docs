---
title: Understand FileX
description: FileX is a high-performance, file allocation table (FAT)-compatible file system that's fully integrated with ThreadX and available for all supported processors. Like ThreadX, FileX is designed to have a small footprint and high performance, making it ideal for today's deeply embedded applications that require file management operations. FileX supports most physical media, including RAM, USBX, SD CARD, and NAND/NOR flash memories via LevelX.
---

# Overview of FileX

FileX embedded file system is Eclipse ThreadX's advanced, industrial grade solution for Eclipse Foundation FAT file formats, designed specifically for deeply embedded, real-time, and IoT applications. FileX supports all of Eclipse Foundation's file formats, including FAT12, FAT16, FAT32, and exFAT. FileX also offers optional fault tolerance and FLASH wear leveling via an add-on product called [LevelX](../levelx/index.md). All of this combined with a small footprint, fast execution, and superior ease-of-use, make FileX the ideal choice for the most demanding embedded IoT applications.

## API protocols

### Media Services

- FAT 12/16/32 and exFAT support
- Minimal 6KB FLASH, 2.5KB RAM
- Complete media access services
- Unlimited number of media instance
- Simple read/write logical sector driver interface
- Multiple partition support
- Logical sector cache
- FAT entry cache
- Optional fault tolerance support
- Deferred Secondary FAT update
- System-level Trace via TraceX
- Intuitive media access APIs, including:
  - fx_media_open
  - fx_media_close
  - fx_media_format
  - fx_media_space_available

### Directory Services

- Up to 256 byte paths
- Long and 8.3 directory names supported
- Directory create & delete
- Directory navigation and traversal
- Directory attributes management
- System-level Trace via TraceX
- Intuitive directory access APIs, including:
  - fx_directory_create
  - fx_directory_delete
  - fx_directory_attributes_set
  - fx_directory_attributes_read
  - fx_directory_first_entry_find
  - fx_directory_next_entry_find

### File Services

- Minimal 3.3KB FLASH
- Unlimited open files
- Read-only files can be opened multiple times
- Long and 8.3 directory names supported
- Contiguous file support
- Fast seek logic
- Pre-allocation of clusters
- File create, delete, and rename
- File read, write, and see
- File attributes management
- System-level Trace via TraceX
- Intuitive file access APIs, including:
  - fx_file_create
  - fx_file_delete
  - fx_file_attributes_set
  - fx_file_attributes_read
  - fx_file_read
  - fx_file_seek
  - fx_file_write

## Advanced technology

FileX is advanced technology, including the following.

- FAT 12/16/32 and exFAT support
- Multiple partition support
- Automatic scaling
- Endian neutral
- Long file name and 8.3 support
- Optional fault tolerance support
- Logical sector cache
- FAT entry cache
- Pre-allocation of clusters
- Contiguous file support
- Optional performance metrics
- TraceX system analysis support

## NOR/NAND Wear Leveling (LevelX)

LevelX is Eclipse Foundation's NOR/NAND FLASH wear leveling product. LevelX can be used in conjunction with FileX or as a stand-alone, direct read/write FLASH sector library for the application.

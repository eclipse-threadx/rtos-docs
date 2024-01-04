---
title: Chapter 1 - Introduction to FileX
description: FileX is a complete FAT format media and file management system for deeply embedded applications.
---

# Chapter 1 - Introduction to FileX

FileX is a complete FAT format media and file management system for deeply embedded applications. This chapter introduces FileX, describing its applications and benefits.

## FileX Unique Features

FileX supports an unlimited number of media devices at the same time, including RAM disks, FLASH managers, and actual physical devices. It supports 12-, 16-, and 32-bit File Allocation Table (FAT) formats, and also supports Extended File Allocation Table (exFAT), contiguous file allocation, and is highly optimized for both size and performance. FileX also includes fault tolerant support, media open/ close, and file write callback functions.

Designed to meet the growing need for FLASH devices, FileX uses the same design and coding methods as ThreadX. Like all Eclipse Foundation products, FileX is distributed with full ANSI C source code, and it has no run-time royalties.

### Product Highlights

- Complete ThreadX processor support
- No royalties
- Complete ANSI C source code
- Real-time performance
- Responsive technical support
- Unlimited FileX objects (media, directories, and files)
- Dynamic FileX object creation/deletion
- Flexible memory usage
- Size scales automatically
- Small footprint (as low as 6 KBytes) instruction area size: 6-30K
- Complete integration with ThreadX
- Endian neutral
- Easy-to-implement FileX I/O drivers
- 12-, 16-, and 32-bit FAT support
- exFAT support
- Long filename support
- Internal FAT entry cache
- Unicode name support
- Contiguous file allocation
- Consecutive sector and cluster read/write
- Internal logical sector cache
- RAM disk demonstration runs out-of-the-box
- Media format capability
- Error detection and recovery
- Fault tolerant options
- Built-in performance statistics
- Standalone support (No Eclipse ThreadX)

## Safety Certifications

### TÜV Certification

FileX has been certified by SGS-TÜV Saar for use in safety-critical systems, according to IEC-61508 SIL 4. The certification confirms that FileX can be used in the development of safety-related software for the highest safety integrity levels of IEC-61508 for the "Functional Safety of electrical, electronic, and programmable electronic safety-related systems." SGS-TUV Saar, formed through a joint venture of Germany's SGS-Group and TUV Saarland, has become the leading accredited, independent company for testing, auditing, verifying, and certifying embedded software for safety-related systems worldwide.

![SGS TUV Saar logo](./media/user-guide/sgs-tuv-saar-logo.png)

- IEC 61508 up to SIL 4

> **Important:** Please contact us for more information on which version(s) of FileX have been certified by TÜV or for the availability of test reports, certificates, and associated documentation.

### UL Certification

FileX has been certified by UL for compliance with UL 60730-1 Annex H, CSA E60730-1 Annex H, IEC 60730-1 Annex H, UL 60335-1 Annex R, IEC 603351 Annex R, and UL 1998 safety standards for software in programmable components. Along with IEC/UL 60730-1, which has requirements for "Controls Using Software" in its Annex H, the IEC 60335-1 standard describes the requirements for "Programmable Electronic Circuits" in its Annex R. IEC 60730 Annex H and IEC 60335-1 Annex R address the safety of MCU hardware and software used in appliances such as washing machines, dishwashers, dryers, refrigerators, freezers, and ovens.

![C RU US 2](./media/user-guide/c-ru-us-logo.png)

_UL/IEC 60730, UL/IEC 60335, UL 1998_

> **Important:** Please contact us for more information on which version(s) of FileX have been certified by UL or for the availability of test reports, certificates, and associated documentation.

## Powerful Services of FileX

### Multiple Media Management

FileX can support an unlimited number of physical media. Each media instance has its own distinct memory area and associated driver specified on the **_fx_media_open_** call. The default distribution of FileX comes with a simple RAM media driver and a demonstration system that uses this RAM disk.

### Logical Sector Cache

By reducing the number of whole sector transfers, both to and from the media, the FileX logical sector cache significantly improves performance. FileX maintains a logical sector cache for each opened media. The depth of the logical sector cache is determined by the amount of memory supplied to FileX with the **_fx_media_open_** API call.

### Contiguous File Support

FileX offers contiguous file support through the API service **_fx_file_allocate_** to improve and make file access time deterministic. This routine takes the amount of memory requested and looks for a series of adjacent clusters to satisfy the request. If such clusters are found, they are pre-allocated by making them part of the file's chain of allocated clusters. On moving physical media, the FileX contiguous file support results in a significant performance improvement and makes the access time deterministic.

### Dynamic Creation

FileX allows you to create system resources dynamically. This is especially important if your application has
multiple or dynamic configuration requirements. In addition, there are no predetermined limits on the number of FileX resources you can use (media or files). Also, the number of system objects does not have any
impact on performance.

## Easy-to-use API

FileX provides the very best deeply embedded file system technology in a manner that is easy to understand and easy to use! The FileX Application Programming Interface (API) makes the services intuitive and consistent. You won't have to decipher "alphabet soup" services that are all too common with other file systems.

For a complete list of the FileX Version 5 Services, see [Appendix A](appendix-a.md).

## exFAT Support

exFAT (extended File Allocation Table) is a file system designed by Eclipse Foundation to allow file size to exceed 2GB, a limit imposed by FAT32 file systems. It is the default file system for SD cards with capacity over 32GB. SD cards or flash drives formatted with FileX exFAT format are compatible with Windows. exFAT supports file size up to one Exabyte (EB), which is approximately one billion GB.

Users wishing to use exFAT must recompile the FileX library with the symbol **_FX_ENABLE_EXFAT_** defined. When opening media, FileX detects the media type. If the media is formatted with exFAT, FileX reads and writes the file system following exFAT standard. To format new media with exFAT, use the service **_fx_media_exFAT_format_**. By default exFAT is not enabled.

## Fault Tolerant Support

The FileX Fault Tolerant Module is designed to prevent file system corruption caused by interruptions during the file or directory update. For example, when appending data to a file, FileX needs to update the content of the file, the directory entry, and possibly the FAT entries. If this sequence of update is interrupted (such as power glitch, or the media is ejected in the middle of the update), the file system is in an inconsistent state, which may affect the integrity of the entire file system, leading towards corruption of other files.

The FileX Fault Tolerant Module works by recording all steps required to update a file or a directory along the way. This log entry is stored on the media in dedicated sectors (blocks) that FileX can find and access. The location of the log data can be accessed even without a proper file system. Therefore, in case the file system is corrupted, FileX is still able to find the log entry and restore the file system back into a good state.

As FileX updates file or directory, log entries are created. After the update operation is successfully completed, the log entries are removed. If the log entries were not properly removed after a successful file update, if the recovery process determines that the content in the log entry matches the file system, nothing needs to be done, and the log entries can be cleaned up.

In case the file system update operation was interrupted, next time the media is mounted by FileX, the Fault Tolerant Module analyzes the log entries. The information in the log entries allows FileX to back out partial changes already applied to the file system (in case the failure happens during the early stage of the file update operation), or if the log entries contain re-do information, FileX is able to apply the changes required to finish the prior operation.

This fault tolerant feature is available to all FAT file systems supported by FileX, including FAT12, FAT16, FAT32, and exFAT. By default fault tolerant is not enabled in FileX. To enable the fault tolerant feature, FileX must be built with the symbol **FX_ENABLE_FAULT_TOLERANT** and **FX_FAULT_TOLERANT** defined. At run time, the application starts fault tolerant service by calling **_fx_fault_tolerant_enable_**.
After the service starts, all file and directory write operations go through the Fault Tolerant Module.

As fault tolerant service starts, it first detects whether or not the media is protected under the Fault Tolerant Module. If it is not, FileX assumes integrity of the file system, and starts protection by allocating free blocks from the file system to be used for logging and caching. If the Fault Tolerant Module logs are found on the file system, it analyzes the log entries. FileX reverts the prior operation or redoes the prior operation, depending on the content of the log entries. The file system becomes available after all the prior log entries are processed. This ensures that FIleX starts from a known good state.

After a media is protected under the FileX Fault Tolerant Module, the media will not be updated with another file system. Doing so would leave the log entries on the file system inconsistent with the contents in the FAT table, the directory entry. If the media is updated by another file system before moving it back to FileX with the Fault Tolerant Module, the result is undefined.

## Callback Functions

The following three callback functions are added to FileX:

- Media Open callback
- Media Close callback
- File Write callback

After registered, these functions will notify the application when such events occur.

## Easy Integration

FileX is easily integrated with virtually any FLASH or media device. Porting FileX is simple. This guide describes the process in detail, and the RAM driver of the demo system makes for a very good place to start!

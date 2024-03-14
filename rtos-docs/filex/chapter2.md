---
title: Chapter 2 - Installation and use of FileX
description: This chapter contains an introduction to FileX and a description of installation conditions, procedures, and use, including the following.
---

# Chapter 2 - Installation and use of FileX

This chapter contains an introduction to FileX and a description of installation conditions, procedures, and use. 

## Host Considerations

### Computer Type

Embedded development is usually performed on Windows or Linux (Unix) host computers. After the application is compiled, linked, and located on the host, it is downloaded to the target hardware for execution.

### Download Interfaces

Usually the target download is done from within the development tool's debugger. After download, the debugger is responsible for providing target execution control (go, halt, breakpoint, etc.) as well as access to memory and processor registers.

### Debugging Tools

Most development tool debuggers communicate with the target hardware via on-chip debug (OCD) connections such as JTAG (IEEE 1149.1) and Background Debug Mode (BDM). Debuggers also communicate with target hardware through In-Circuit Emulation (ICE) connections. Both OCD and ICE connections provide robust solutions with minimal intrusion on the target resident software.

### Required Hard Disk Space

The source code for FileX is delivered in ASCII format and requires approximately 500 KBytes of space on the host computer's hard disk

## Target Considerations

FileX requires between 6 KBytes and 30 KBytes of  Read-Only Memory (ROM) on the target. Another  100 bytes of the target's Random Access Memory (RAM) are required for FileX global data structures. Each opened media also requires 1.5 KBytes of RAM for the control block in addition to RAM for storing data for one sector (typically 512 bytes).

For date/time stamping to function properly, FileX relies on ThreadX timer facilities. This is implemented by creating a FileX-specific timer during FileX initialization. FileX also relies on ThreadX semaphores for multiple thread protection and I/O suspension.

## Product Distribution

FileX can be obtained from our public source code repository at <https://github.com/eclipse-threadx/filex/>.

The following is a list of several important files in the repository:

- ***fx_api.h*** : This C header file contains all system equates, data structures, and service prototypes.
- ***fx_port.h*** : This C header file contains all development-tool-specific data definitions and structures.
- ***demo_filex.c*** : This C file contains a small demo application.
- ***fx.a (or fx.lib)*** : This is the binary version of the FileX C library. It is distributed with the standard package.

> **Important:** *All file names are in lower-case. This naming convention makes it easier to convert the commands to Linux (Unix) development platforms.*

## FileX Installation

FileX is installed by cloning the GitHub repository to your local machine. The following is typical syntax for creating a clone of the FileX repository on your PC:

```c
    git clone https://github.com/eclipse-threadx/filex
```

Alternatively you can download a copy of the repository using the download button on the GitHub main page.

You will also find instructions for building the FileX library on the front page of the online repository.

> **Important:** Application software needs access to the FileX library file (usually called* usually ***fx.a*** or ***fx.lib***) *and the C include files **fx_api.h** and **fx_port.h**. This is accomplished either by setting the appropriate path for the development tools or by copying these files into the application development area.

## Using FileX

Using FileX is easy. Basically, the application code must include ***fx_api.h*** during compilation and link with the FileX run-time library ***fx.a*** (or ***fx.lib***). Of course, the ThreadX files, namely ***tx_api.h*** and ***tx.a*** (or ***tx.lib***)*,* are also required.

> **Important:** When using FileX in Standalone mode (**FX_STANDALONE_ENABLE** must be defined), ThreadX files/libraries are not required.

Assuming you are already using ThreadX, there are four steps required to build a FileX application:

1. Include the ***fx_api.h*** file in all application files that use FileX services or data structures.
1. Initialize the FileX system by calling ***fx_system_initialize*** from the ***tx_application_define*** function or an application thread.

   > **Important:** When using FileX in Standalone mode, ***fx_system_initialize*** should be directly called from application code.

1. Add one or more calls to ***fx_media_open*** to set up the FileX media. This call must be made from the context of an application thread.

   > **Important:** *Remember that the **fx_media_open** call requires enough RAM to store data for one sector.*

1. Compile application source and link with the FileX and ThreadX run-time libraries, ***fx.a*** (or ***fx.lib***) and ***tx.a*** (or ***tx.lib***). The resulting image can be downloaded to the target and executed!

## Troubleshooting

Each FileX port is delivered with a demonstration application. It is always a good idea to get the demonstration system running first—either on the target hardware or a specific demonstration environment.

If the demonstration system does not work, try the following things to narrow the problem:

1. Determine how much of the demonstration is running.
1. Increase stack sizes (this is more important in actual application code than it is for the demonstration).
1. Ensure there is enough RAM for the 32KBytes default RAM disk size. The basic system will operate on much less RAM; however, as more of the RAM disk is used, problems will surface if there is not enough memory.
1. Temporarily bypass any recent changes to see if the problem disappears or changes.

## Configuration Options

There are several configuration options when building the FileX library and the application using FileX. The options below can be defined in the application source, on the command line, or within the ***fx_user.h*** include file.

> **Important:** *Options defined in **fx_user.h** are applied only if the application and ThreadX library are built with ***FX_INCLUDE_USER_DEFINE_FILE*** defined.*
> When using FileX in Standalone mode (**FX_STANDALONE_ENABLE** must be defined), ThreadX files/libraries are not required.

The following list describes each configuration option in detail:

|Define|Meaning|
|----------    |-----------|
|FX_MAX_LAST_NAME_LEN        |This value defines the maximum file name length, which includes full path name. By default, this value is 256.|
|FX_DONT_UPDATE_OPEN_FILES    |Defined, FileX does not update already opened files.|
|FX_MEDIA_DISABLE_SEARCH_CACHE    |Defined, the file search cache optimization is disabled.|
|FX_MEDIA_DISABLE_SEARCH_CACHE    |Defined, the file search cache optimization is disabled.|
|FX_DISABLE_DIRECT_DATA_READ_CACHE_FILL |Defined, the direct read sector update of cache is disabled.|
|FX_MEDIA_STATISTICS_DISABLE |Defined, gathering of media statistics is disabled.|
|FX_SINGLE_OPEN_LEGACY |Defined, legacy single open logic for the same file is enabled.|
|FX_RENAME_PATH_INHERIT    |Defined, renaming inherits path information.|
|FX_DISABLE_ERROR_CHECKING    |Removes the basic FileX error checking API and results in improved performance (as much as 30%) and smaller code size.|
|FX_MAX_LONG_NAME_LEN    |Specifies the maximum file name size for FileX. The default value is 256, but this can be overridden with a command-line define. Legal values range between 13 and 256.|
|FX_MAX_SECTOR_CACHE|Specifies the maximum number of logical sectors that can be cached by FileX. The actual number of sectors that can be cached is lesser of this constant and how many sectors can fit in the amount of memory supplied at fx_media_open. The default value is 256. All values must be a power of 2.|
|FX_FAT_MAP_SIZE    |Specifies the number of sectors that can be represented in the FAT update map. The default value is 256, but this can be overridden with a command-line define. Larger values help reduce unneeded updates of secondary FAT sectors.|
|FX_MAX_FAT_CACHE    |Specifies the number of entries in the internal FAT cache. The default value is 16, but this can be overridden with a command-line define. All values must be a power of 2.|
|FX_FAULT_TOLERANT    |When defined, FileX immediately passes write requests of all system sectors (boot, FAT, and directory sectors) to the media's driver. This potentially decreases performance, but helps limit corruption to lost clusters. Note that enabling this feature does not automatically enable FileX Fault Tolerant Module, which is enabled by defining|
|FX_FAULT_TOLERANT_DATA    |When defined, FileX immediately passes all file data write requests to the media's driver. This potentially decreases performance, but helps limit lost file data. Note that enabling this feature does not automatically enable FileX Fault Tolerant Module, which is enabled by defining ***FX_ENABLE_FAULT_TOLERANT***|
|FX_NO_LOCAL_PATH|Removes local path logic from FileX, resulting in smaller code size.|
|FX_NO_TIMER|Eliminates the ThreadX timer setup to update the FileX system time and date. Doing so causes default time and date to be placed on all file operations.|
|FX_UPDATE_RATE_IN_SECONDS    |Specifies rate at which system time in FileX is adjusted. By default, value is 10, specifying that the FileX system time is updated every 10 seconds.|
|FX_ENABLE_EXFAT| When defined, the logic for handling exFAT file system is enabled in FileX. By default this symbol is not defined.| 
|FX_UPDATE_RATE_IN_TICKS| Specifies the same rate as ***FX_UPDATE_RATE_IN_SECONDS*** (see above), except in terms of the underlying ThreadX timer frequency. The default is 1000, which assumes a 10ms ThreadX timer rate and a 10 second interval.|
|FX_SINGLE_THREAD|Eliminates ThreadX protection logic from the FileX source. It should be used if FileX is being used only from one thread or if FileX is being used without ThreadX.|
|FX_DRIVER_USE_64BIT_LBA|When defined, enables 64-bit sector addresses used in I/O driver. By default this option is not defined.|
|FX_ENABLE_FAULT_TOLERANT| When defined, enables FileX Fault Tolerant Module. Enabling Fault Tolerant automatically defines the symbol ***FX_FAULT_TOLERANT*** and ***FX_FAULT_TOLERANT_DATA***. By default this option is not defined.|
|FX_FAULT_TOLERANT_BOOT_INDEX|Defines byte offset in the boot sector where the cluster for the fault tolerant log is. By default this value is 116. This field takes 4 bytes. Bytes 116 through 119 are chosen because they are marked as reserved by FAT 12/16/32/exFAT specification.|
|FX_FAULT_TOLERANT_MINIMAL_CLUSTER|This symbol is deprecated. It is no longer being used by FileX Fault Tolerant.|
|FX_STANDALONE_ENABLE|Defined, enables FileX to be used in standalone mode (without Eclipse ThreadX). By default this symbol is not defined.|

> **Important:** When **FX_STANDALONE_ENABLE** is defined, Local path logic and ThreadX timer setup are disabled.

## FileX Version ID

The current version of FileX is available both to the user and the application software during run-time. The programmer can obtain the FileX version from examination of the **fx_port.h** file. In addition, this file also contains a version history of the corresponding port. Application software can obtain the FileX version by examining the global string ***_fx_version_id***.

---
title: Chapter 2 - Installation and Use of GUIX
description: This chapter contains a description of various issues related to installation, setup, and use of the high performance user interface product GUIX.
---

# Chapter 2 - Installation and Use of GUIX

This chapter contains a description of various issues related to
installation, setup, and use of the high performance user interface 
product GUIX.  

## Host Considerations

Embedded development is usually performed on Windows or Linux (Unix)
host computers. After the application is compiled, linked, and the
executable is generated on the host, it is downloaded to the target
hardware for execution.

Usually the target download is done from within the development tool's
debugger. After download, the debugger is responsible for providing
target execution control (go, halt, breakpoint, etc.) as well as access
to memory and processor registers.

Most development tool debuggers communicate with the target hardware via
on-chip debug (OCD) connections such as JTAG (IEEE 1149.1) and
Background Debug Mode (BDM). Debuggers also communicate with target
hardware through In-Circuit Emulation (ICE) connections. Both OCD and
ICE connections provide robust solutions with minimal intrusion on the
target resident software.

As for resources used on the host, the source code for GUXI is delivered
in ASCII format and requires approximately 30 Mbytes of space on the
host computer's hard disk.

## Target Considerations

GUIX requires between 5 KBytes and 80 Kbytes of Read-Only Memory (ROM)
on the target. Another 5 to 10KBytes of the target's Random Access Memory (RAM) are required for the GUIX thread stack and other global data structures.

In addition, GUIX requires the use of a ThreadX timer and a ThreadX
mutex object. These facilities are used for periodic processing needs
and thread protection inside GUIX.

## Product Distribution

GUIX can be obtained from our public source code repository at <https://github.com/eclipse-threadx/guix/>.

The following is a list of the important files common to most product distributions:

| Filename&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Description   |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| gx_api.h        | This C header file contains all system equates, data structures, and service prototypes. |
| gx_port.h       | This C header file contains all target-specific and development tool-specific data definitions and structures.                                                                                                                                         |
| gx.a (or gx.lib) | This is the binary version of the GUIX C library. This is normally built by compiling and archiving the provided GUIX library source files, however this library may be provided in pre-built form depending on your hardware target and license type. |

> **Important:** *All files are in lower-case, making it easy to convert the commands to Linux (Unix) development platforms.*

## GUIX Installation

GUIX is installed by cloning the GitHub repository to your local machine. The following is typical syntax for creating a clone of the GUIX repository on your PC:

```c
    git clone https://github.com/eclipse-threadx/guix
```

Alternatively you can download a copy of the repository using the download button on the GitHub main page.

You will also find instructions for building the GUIX library on the front page of the online repository.

**Note:** *Application software needs access to the GUIX library file, usually called **gx.a** (or **gx.lib**), and the C include files **gx_api.h** and **gx_port.h**. This is accomplished either by setting the appropriate path for the development tools or by copying these files into the application development area.*

## Using GUIX

Using GUIX is easy. Basically, the application code must include ***gx_api.h*** during compilation and link with the GUIX library ***gx.a*** (or ***gx.lib**)*.

There are four easy steps required to build a GUIX
application:

| Steps   | Description    |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Step&nbsp;1: | Include the ***gx_api.h*** file in all application files that use GUIX services or data structures.                                                               |
| Step&nbsp;2: | Initialize the GUIX system by calling ***gx_system_initialize*** from the ***tx_application_define*** function or an application thread.                       |
| Step&nbsp;3: | Create a display instance, create a canvas for the display, and create the root window and any other windows or widgets necessary.                                 |
| Step&nbsp;4: | Compile application source and link with the GUIX runtime library ***gx.a*** (or ***gx.lib***). The resulting image can be downloaded to the target and executed! |

## Troubleshooting

Each GUIX port is delivered with a demonstration application that
executes on specific display hardware. The same basic demonstration is
delivered with all versions of GUIX. It is always a good idea to get the
demonstration system running first.

If the demonstration system does not run properly, perform the following
operations to narrow the problem:

1. Determine how much of the demonstration is running.

2. Increase the stack size of the GUIX thread by changing the
    compile-time constant **GX_THREAD_STACK_SIZE** and recompiling
    the GUIX library

3. Recompile the GUIX library with the appropriate debug options listed
    in the configuration option section.

4. Examine the return status from all API calls.

5. Determine if there is an internal system error by setting a
    breakpoint at the function ***_gx_system_error_process***. There
    error code and caller should give clues as to what might be going
    wrong.

6. Temporarily bypass any recent changes to see if the problem
    disappears or changes.

## Configuration Options

There are several configuration options when building the GUIX library and the application using GUIX. These options are used to tune the library size and feature set to best fit your application requirements. For example, if your application will have only one thread utilizing the GUIX API services, the configuration flag **GX_DISABLE_MULTITHREAD_SUPPORT** should be defined to eliminate the overhead associated with protecting critical code sections from pre-emption by multiple threads. The various configuration flags can be defined in the application source, on the command line, or within the ***gx_user.h*** include file.

Whenever the GUIX library configuration flags are modified, it is
required to rebuild both the GUIX library and your application modules
for the configuration changes to take effect.

The complete list of configuration flags is documented in Appendix H:
GUIX Build-Time Configuration Flags.

## GUIX Version ID

The current version of GUIX is available to both the user and the
application software during runtime. The programmer can obtain the GUIX version from examination of the ***gx_port.h*** file. In addition, this file also contains a version history of the corresponding port Application software can obtain the GUIX version by examining the global string ***_gx_version_id*** in ***gx_port.h***.

Application software can also obtain release information from the
constants shown below defined in ***gx_api.h**.* These constants
identify the current product release by name and the product major and
minor version.

```C
#define __PRODUCT_GUIX__

#define __GUIX_MAJOR_VERSION__

#define __GUIX_MINOR_VERSION__
```

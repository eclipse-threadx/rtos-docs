---
title: About the NetX Duo User Guide
description: This guide contains comprehensive information about NetX Duo, the Eclipse Foundation high-performance IPv4/IPv6 dual network stack.
---


This guide contains comprehensive information about NetX Duo, the Eclipse Foundation high-performance IPv4/IPv6 dual network stack. 

It is intended for embedded real-time software developers familiar with basic networking concepts, ThreadX, and the C programming language.

## Organization

- [Chapter 1](chapter1) - Introduces NetX Duo
- [Chapter 2](chapter2) - Gives the basic steps to install and use NetX Duo with your ThreadX application
- [Chapter 3](chapter3) - Provides a functional overview of the NetX Duo system and basic information about the TCP/IP networking standards
- [Chapter 4](chapter4) - Details the application's interface to NetX Duo
- [Chapter 5](chapter5) - Describes network drivers for NetX Duo
- [Appendix A](appendix-a) - NetX Duo Services
- [Appendix B](appendix-b) - NetX Duo Constants
- [Appendix C](appendix-c) - NetX Duo Data Types
- [Appendix D](appendix-d) - BSD-Compatible Socket API
- [Appendix E](appendix-e) - ASCII Chart

## Guide Conventions

Italics - Typeface denotes book titles, emphasizes important words, and indicates variables.

**Boldface** - Typeface denotes file names, key words, and further emphasizes important words and variables.

> **Important:** Information symbols draw attention to important or additional information that could affect performance or function.
 
> **Warning:** Warning symbols draw attention to situations that developers should avoid because they could cause fatal errors.

## NetX Duo Data Types

In addition to the custom NetX Duo control structure data types, there are several special data types that are used in NetX Duo service call interfaces. These special data types map directly to data types of the underlying C compiler. This is done to ensure portability between different C compilers. The exact implementation is inherited from ThreadX and can be found in the ***tx_port.h*** file included in the ThreadX distribution.

The following is a list of NetX Duo service call data types and their associated meanings:

**UINT**: Basic unsigned integer. This type must support 32-bit unsigned data; however, it is mapped to the most convenient unsigned data type.  
**ULONG**: Unsigned long type. This type must support 32-bit unsigned  data.
**VOID**: Almost always equivalent to the compiler's void type.  
**CHAR**: Most often a standard 8-bit character type.  

Additional data types are used within the NetX Duo source. They are located in either the ***tx_port.h*** or ***nx_port.h*** files.

## Troubleshooting

For troubleshooting, be sure to collect the following information:

1. A detailed description of the problem, including frequency of occurrence and whether it can be reliably reproduced.
2. A detailed description of any changes to the application and/or NetX Duo that preceded the problem.
3. The contents of the _tx_version_id and
_nx_version_id strings found in the tx_port.h and nx_port.h files of your distribution. These strings provide valuable information regarding your run-time environment.
4. The contents in RAM of the following ULONG variables:

    **_tx_build_options**

    **_nx_system_build_options1**

    **_nx_system_build_options2**

    **_nx_system_build_options3**

    **_nx_system_build_options4**

    **_nx_system_build_options5**

    These variables give information on how your ThreadX and NetX Duo libraries were built.

5. A trace buffer captured immediately after the problem was detected. This is accomplished by building the ThreadX and NetX Duo libraries with TX_ENABLE_EVENT_TRACE and calling tx_trace_enable with the trace buffer information.

---
title: FileX user guide
description: This guide contains comprehensive information about FileX, the high-performance real-time file system from Eclipse Foundation.
---


This guide contains comprehensive information about FileX, the high-performance, real-time embedded file system from Eclipse Foundation. To gain the most from this guide, you should be familiar with standard real-time operating system functions, FAT file system services, and the C programming language.

## Organization

[Chapter 1](chapter1) - Introduces FileX

[Chapter 2](chapter2) - Gives the basic steps to install and use FileX with your ThreadX application

[Chapter 3](chapter3) - Provides a functional overview of the FileX system and basic information about FAT file system formats

[Chapter 4](chapter4) - Details the application's interface to FileX

[Chapter 5](chapter5) - Describes the supplied FileX RAM driver and how to write your own custom FileX drivers

[Chapter 6](chapter6) - Describes the FileX Fault Tolerant Module

[Appendix A](appendix-a) - FileX Services

[Appendix B](appendix-b) - FileX Constants

[Appendix C](appendix-c) - FileX Data Types

[Appendix D](appendix-d) - ASCII Chart

## Guide Conventions

*Italics* - Typeface denotes book titles, emphasizes important words, and indicates variables.

**Boldface** - Typeface denotes file names,
key words, and further emphasizes important words and variables.

> **Note:** Information symbols draw attention to important or additional information that could affect performance or function.

> **Important:** Warning symbols draw attention to situations that developers should avoid because they could cause fatal errors.

## FileX Data Types

In addition to the custom FileX control structure data types, there is a series of special data types that are used in FileX service call interfaces. These special data types map directly to data types of the underlying C compiler. This is done to ensure portability between different C compilers. The exact implementation is inherited from ThreadX and can be found in the tx_port.h file included in the ThreadX distribution.

The following is a list of FileX service call data types and their associated meanings.

| Type  | Description  |
|---|---|
| **UINT** | Basic unsigned integer. This type must support 8-bit unsigned data; however, it is mapped to the most convenient unsigned data type. |
| **ULONG** | Unsigned long type. This type must support 32-bit unsigned data. |
| **VOID** | Almost always equivalent to the compiler's void type. |
| **CHAR** | Most often a standard 8-bit character type. |
| **ULONG64** | 64-bit unsigned integer data type. |

Additional data types are used within the FileX source. They are located in either the ***tx_port.h*** or ***fx_port.h*** files.

## Troubleshooting

For troubleshooting, be sure to collect the following information.

1. A detailed description of the problem, including frequency of occurrence and whether it can be reliably reproduced.
2. A detailed description of any changes to the application and/or FileX that preceded the problem.
3. The contents of the _tx_version_id and
_fx_version_id strings found in the ***tx_port.h*** and ***fx_port.h*** files of your distribution. These strings will provide valuable information regarding your run-time environment.
4. The contents in RAM of the following **ULONG** variables. These variables will give information on how your ThreadX and FileX libraries were built:

    **_tx_build_options**

    **_fx_system_build_options1**

    **_fx_system_build_options2**

    **_fx_system_build_options3**

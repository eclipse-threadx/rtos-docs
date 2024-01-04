---
title: About the ThreadX Guide
description: This guide provides comprehensive information about ThreadX, the Eclipse Foundation high-performance real-time kernel.
---

# About the ThreadX Guide

This guide provides comprehensive information about ThreadX, the Eclipse Foundation high-performance real-time kernel. 

It is intended for the embedded real-time software developer. The developer should be familiar with standard real-time operating system functions and the C programming language.

## Organization

[Chapter 1](chapter1.md) - Provides a basic overview of ThreadX and its relationship to real-time embedded development

[Chapter 2](chapter2.md) - Gives the basic steps to install and use ThreadX in your application right *out of the box*

[Chapter 3](chapter3.md) - Describes in detail the functional operation of ThreadX, the high performance real-time kernel

[Chapter 4](chapter4.md) - Details the application's interface to ThreadX

[Chapter 5](chapter5.md) - Describes writing I/O drivers for ThreadX applications

[Chapter 6](chapter6.md) - Describes the demonstration application that is supplied with every ThreadX processor support package

[Appendix A](appendix-a.md) - ThreadX API

[Appendix B](appendix-b.md) - ThreadX constants

[Appendix C](appendix-c.md) - ThreadX data types

[Appendix D](appendix-d.md) - ASCII chart

[Appendix E](appendix-e.md) - ThreadX SMP MISRA C compliance

## Guide Conventions

*Italics* - typeface denotes book titles, emphasizes important words, and indicates parameters.

**Boldface** - typeface denotes key words, constants, type names, user interface elements, variable names, and further emphasizes important words.

***Italics and Boldface*** - typeface denotes file names and function names.

> **Important:** Information symbols draw attention to important or additional information that could affect performance or function.

> **Warning:** Warning symbols draw attention to situations in which developers should take care to avoid because they could cause fatal errors.

## ThreadX Data Types

In addition to the custom ThreadX control structure data types, there are a series of special data types that are used in ThreadX service call interfaces. These special data types map directly to data types of the underlying C compiler. This is done to insure portability between different C compilers. The exact implementation can be found in the ***tx_port.h*** file included with the source.

The following is a list of ThreadX service call data types and their associated meanings:

| Data type  | Description |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| **UINT** | Basic unsigned integer. This type must support 8-bit unsigned data; however, it is mapped to the most convenient unsigned data type. |
| **ULONG** | Unsigned long type. This type must support 32-bit unsigned data. |
| **VOID** | Almost always equivalent to the compiler's void type. |
| **CHAR** | Most often a standard 8-bit character type. |

Additional data types are used within the ThreadX source. They are
also located in the ***tx_port.h*** file.

## Troubleshooting

For troubleshooting, be sure to collect the following information:

1. A detailed description of the problem, including frequency of occurrence and whether it can be reliably reproduced.
2. A detailed description of any changes to the application and/or ThreadX that preceded the problem.
3. The contents of the *_tx_version_id* string found in the *tx_port.h* file of your distribution. This string provides valuable information regarding your run-time environment.
4. The contents in RAM of the **_tx_build_options** **ULONG** variable. This variable gives information on how your ThreadX library was built.

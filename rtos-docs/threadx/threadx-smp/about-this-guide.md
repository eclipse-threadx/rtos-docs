---
title: About This Guide
description: This guide provides comprehensive information about ThreadX SMP, the Eclipse Foundation high-performance embedded real-time kernel.
---

# About This Guide

This guide provides comprehensive information about ThreadX SMP, the Eclipse Foundation high-performance embedded real-time kernel.

It is intended for the embedded real-time software developer. The developer should be familiar with standard real-time operating system functions and the C programming language.

## Organization

| Chapter       | Overview                    |
| ------------- | ---------------------------------------------------------------------------------------------------------- |
| **Chapter 1** | Provides a basic overview of ThreadX SMP and its relationship to real-time embedded development.           |
| **Chapter 2** | Gives the basic steps to install and use ThreadX SMP in your application right *out of the box*.           |
| **Chapter 3** | Describes in detail the functional operation of ThreadX SMP, the high-performance real-time SMP kernel.    |
| **Chapter 4** | Details the application's interface to ThreadX SMP.                                                        |
| **Chapter 5** | Describes writing I/O drivers for ThreadX SMP applications.                                                |
| **Chapter 6** | Describes the demonstration application that is supplied with every ThreadX SMP processor support package. |
| **Appendix A** | ThreadX SMP API        |
| **Appendix B** | ThreadX SMP constants  |
| **Appendix C** | ThreadX SMP data types |
| **Appendix D** | ASCII chart            |

## Guide Conventions

- *Italics* - *typeface denotes book titles, emphasizes important words, and indicates variables.*
- **Boldface** - **typeface denotes file names, key words, and further emphasizes important words and variables.**

> **Important:** Information symbols draw attention to important or additional information that could affect performance or function.

> **Warning:** Warning symbols draw attention to situations that developers should take care to avoid because they could cause fatal errors.

## ThreadX SMP Data Types

In addition to the custom ThreadX SMP control structure data types, there are a series of special data types that are used in ThreadX SMP service call interfaces. These special data types map directly to data types of the underlying C compiler. This is done to insure portability between different C compilers. The exact implementation can be found in the ***tx_port.h*** file included on the distribution disk.

The following is a list of ThreadX SMP service call data types and their associated meanings:

| Data Type          | Meaning                                                          |
| --------- | --------------------------------------------------------- |
| **UINT**  | Basic unsigned integer. This type must support 8-bit unsigned data; however, it is mapped to the most convenient unsigned data type. |
| **ULONG** | Unsigned long type. This type must support 32-bit unsigned data.                                                                     |
| **VOID**  | Almost always equivalent to the compiler's void type.                                                                                |
| **CHAR**  | Most often a standard 8-bit character type.                                                                                          |

Additional data types are used within the ThreadX SMP source. They are also located in the ***tx_port.h*** file.

### Troubleshooting

For troubleshooting, be sure to collect the following information:

1. A detailed description of the problem, including frequency of occurrence and whether it can be reliably reproduced.
2. A detailed description of any changes to the application and/or ThreadX SMP that preceded the problem.
3. The contents of the ***_tx_version_id*** string found in the ***tx_port.h*** file of your distribution. This string provides valuable information regarding your run-time environment.
4. The contents in RAM of the ***_tx_build_options*** ULONG variable. This variable gives information on how your ThreadX SMP library was built.


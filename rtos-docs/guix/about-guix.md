---
title: GUIX User Guide
description: This guide contains comprehensive information about GUIX, the high-performance GUI product from Eclipse Foundation.
---
# About GUIX User Guide

This guide contains comprehensive information about GUIX, the high-performance GUI product from Eclipse Foundation. It is intended for embedded real-time software developers familiar with basic GUI concepts, ThreadX, and the C programming language.

## Organization

[Chapter 1 - Introduction to GUIX](chapter-1.md)

[Chapter 2 - Installation and use of GUIX](chapter-2.md)

[Chapter 3 - Functional Overview of GUIX](chapter-3.md)

[Chapter 4 - Description of GUIX Services](chapter-4.md)

[Chapter 5 - GUIX Display Drivers](chapter-5.md)  

[GUIX Example](guix-example.md)

[Appendix A - GUIX Color Definitions](appendix-a.md)

[Appendix B - GUIX Color Formats](appendix-b.md)

[Appendix C - GUIX Widget Styles](appendix-c.md)

[Appendix D - GUIX Brush, Canvas and Gradient Attributes](appendix-d.md)

[Appendix E - GUIX Event Description](appendix-e.md)

[Appendix F - GUIX RTOS Binding Services](appendix-f.md)

[Appendix G - GUIX Font Structure](appendix-g.md)

[Appendix H - GUIX Build-Time Configuration flags](appendix-h.md)

[Appendix I - GUIX Information Structures](appendix-i.md)

[Appendix J - Canvas Partial Frame Buffer Feature](appendix-j.md)

## Guide Conventions

*Italics* - Typeface denotes book titles, emphasizes important words, and indicates variables.

**Boldface** - Typeface denotes file names, key words, and further emphasizes important words and variables.

> **Important:** Information symbols draw attention to important or additional information that could affect performance or function.

## GUIX Data Types

In addition to the custom GUIX control structure data types, there are several special data types that are used in GUIX service call interfaces. These special data types map directly to data types of the underlying C compiler. This is done to ensure portability between different C compilers. The exact implementation is inherited from ThreadX and can be found in the ***tx_port.h*** file included in the ThreadX distribution.

The following is a list of GUIX service call data types and their associated meanings:

| Data type | Description |
| --------------------- | --------------------------------------------------------------------------------------------------------------------- |
| **UINT**             | Basic unsigned integer. This type is mapped to the most convenient unsigned data type.                                |
| **INT**              | Basic signed integer. This type is mapped to the most convenient signed data type.                                    |
| **ULONG**            | Unsigned long type. This type must support 32-bit unsigned data.                                                      |
| **VOID**             | Almost always equivalent to the compiler's void type.                                                                 |
| **GX_CHAR**         | Most often typedefed as the compiler defined char type.                                                               |
| **GX_BYTE**          | 8-bit signed type.                                                                                                    |
| **GX_UBYTE**         | 8-bit unsigned type.                                                                                                  |
| **GX_VALUE**        | 16 or 32 bit signed type. Defined as needed for best performance on the target system.                                |
| **GX_FIXED_VAL**   | Fixed point numeric data type.                                                                                        |
| **GX_RESOURCE_ID** | Unsigned long type.                                                                                                   |
| **GX_COLOR**        | Unsigned long type.                                                                                                   |
| **GX_STRING**       | Structure containing GX_CHAR \*gx_string_ptr and UINT gx_string_length.                                          |
| **GX_POINT**        | Structure containing gx_point_x and gx_point_y.                                                                   |
| **GX_RECTANGLE**    | Structure containing gx_rectangle_left, gx_rectangle_top, gx_rectangle_right, and gx_rectangle_bottom fields. |
| **GX_GLYPH**        | Structure containing glyph metrics.                                                                                   |
| **GX_FONT**         | Structure containing font metrics.                                                                                    |
| **GX_BRUSH**        | Structure containing brush metrics.                                                                               |
**GX_PIXELMAP**       | Structure containing pixelmap metrics.

Additional data types are used within the GUIX source. They are located in either the ***tx_port.h*** or ***gx_port.h*** files.

## Troubleshooting

For troubleshooting, capture the following information:

1. A detailed description of the problem, including frequency of occurrence and whether it can be reliably reproduced.

2. A detailed description of any changes to the application and/or GUIX that preceded the problem.

3. The contents of the _tx_version_id and _gx_version_id strings found in the ***tx_port.h*** and ***gx_port.h*** files of your distribution. These strings will provide valuable Information regarding your run-time environment.

4. The contents in RAM of the following ULONG variables:

    **_tx_build_options**
    **_gx_system_build_options**

    These variables will give information on how your ThreadX and GUIX libraries were built.

5. The contents in RAM of the following ULONG variables:

    **_gx_system_last_error**
    **_gx_system_error_count**

    These variables keep track of internal system errors in GUIX. If the _gx_system_error_count is greater than one, please set a breakpoint on the function return in the _gx_system_error_process function and find the value of _gx_system_last_error at this point. This will yield the first internal GUIX system error.

6. A trace buffer captured immediately after the problem was detected. This is accomplished by building the ThreadX and GUIX libraries with TX_ENABLE_EVENT_TRACE and calling tx_trace_enable with the trace buffer information.

7. The GUIX Studio project you are using, if applicable, or at a minimum a small project sufficient to demonstrate the deficiency.

---
title: TraceX user guide
description: This guide contains comprehensive information about TraceX, the Windows-based system analysis tool from Eclipse Foundation.
---

# About this guide

This guide contains comprehensive information about TraceX, the Microsoft Windows-based system analysis tool for Eclipse ThreadX.

It is intended for the embedded real-time software developer using ThreadX Real-Time Operating System (RTOS) and add-on components. The developer should be familiar with standard ThreadX, FileX, and NetX Duo concepts.

## Organization

- [Chapter 1](chapter1.md) - contains an basic overview of TraceX and describes its relationship to real-time development.
- [Chapter 2](chapter2.md) - gives the basic steps to install and use TraceX to analyze your application right out of the box.
- [Chapter 3](chapter3.md) - describes the main features of TraceX.
- [Chapter 4](chapter4.md) - details performance analysis features of TraceX.
- [Chapter 5](chapter5.md) - describes how to set up ThreadX, FileX, and NetX Duo in order to generate a trace buffer that is viewable by TraceX.
- [Chapter 6](chapter6.md) - describes TraceX events in detail.
- [Chapter 7](chapter7.md) - describes FileX events in detail.
- [Chapter 8](chapter8.md) - describes NetX Duo events in detail.
- [Chapter 9](chapter9.md) - describes USBX events in detail.
- [Chapter 10](chapter10.md) - describes creating custom user events in detail.
- [Chapter 11](chapter11.md) - describes the internal trace buffer in detail.
- [Appendix A](appendix-a.md) - ThreadX port-specific file with its time-stamp source for gathering trace events.
- [Appendix B](appendix-b.md) - ThreadX *tx_trace.h* file that shows implementation details regarding the event trace buffer.
- [Appendix C](appendix-c.md) - Summarizes command line utilities for converting various file formats into proper TraceX binary files.
- [Appendix D](appendix-d.md) - Examples of dumping trace files from various development tools.

## Guide Conventions

*Italics* - Typeface denotes book titles, emphasizes important words, and indicates variables.

**Boldface** - Typeface denotes file names, key words, and further emphasizes important words and variables.

> **Note:** Indicates information of note.

## Troubleshooting

For troubleshooting, be sure to collect the following information:

1. A detailed description of the problem, including frequency of occurrence and whether it can be reliably reproduced.
2. A detailed description of any changes to the application and/or ThreadX that preceded the problem.
3. The contents of the *_tx_version_id* string found in the *tx_port.h* file of your distribution. This string provides valuable information regarding your run-time environment.
4. The contents in RAM of the *_tx_build_options* ULONG variable. This variable gives information on how your ThreadX library was built.

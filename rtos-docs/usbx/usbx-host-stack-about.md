---
title: USBX Host Stack User Guide
description: This guide provides comprehensive information about USBX, the high-performance USB foundation software from Eclipse Foundation.
---
# USBX Host Stack User Guide

This guide provides comprehensive information about USBX, the high-performance USB foundation software from Eclipse Foundation.

It is intended for the embedded real-time software developer. The developer should be familiar with standard real-time operating system functions, the USB specification, and the C programming language.

For technical information related to USB, see the USB specification and USB Class specifications that can be downloaded at [https://www.USB.org/developers](https://www.USB.org/developers)

## Organization

- [**Chapter 1**](usbx-host-stack-1.md) - contains an introduction to USBX

- [**Chapter 2**](usbx-host-stack-2.md) - gives the basic steps to install and use USBX with your ThreadX application

- [**Chapter 3**](usbx-host-stack-3.md) - provides a functional overview of USBX and basic information about USB

- [**Chapter 4**](usbx-host-stack-4.md) - details the application's interface to USBX in host mode

- [**Chapter 5**](usbx-host-stack-5.md) - describes the APIs of the USBX Host classes

- [**Chapter 6**](usbx-host-stack-6.md) - describes the USBX CDC-ECM class

## Troubleshooting

For troubleshooting, be sure to collect the following information:

1. A detailed description of the problem, including frequency of occurrence and whether it can be reliably reproduced.
2. A detailed description of any changes to the application and/or ThreadX that preceded the problem.
3. The contents of the **_tx_version_id** string found in the ***tx_port.h*** file of your distribution. This string provides valuable information regarding your run-time environment.
4. The contents in RAM of the *_tx_build_options* **ULONG** variable. This variable gives information on how your ThreadX library was built.

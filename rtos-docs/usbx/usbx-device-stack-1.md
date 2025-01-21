---
title: Chapter 1 - Introduction to USBX Device Stack
description: USBX is a full-featured USB stack for deeply embedded applications. This chapter introduces USBX, describing its benefits and application.
---
# Chapter 1 - Introduction to USBX Device Stack

USBX is a full-featured USB stack for deeply embedded applications. This chapter introduces USBX, describing its applications and benefits 

## USBX features

USBX supports the three existing USB specifications: 1.1, 2.0 and OTG. It is designed to be scalable and will accommodate simple USB topologies with only one connected device as well as complex topologies with multiple devices and cascading hubs. USBX supports all the data transfer types of the USB protocols: control, bulk, interrupt, and isochronous.

USBX supports both the host side and the device side. Each side is composed of three layers.

- Controller layer
- Stack layer
- Class layer

The relationship between the USB layers is as follows:

![USB layers](media/usbx-device-stack/usb-layers.png)

## Product Highlights

- Complete ThreadX processor support
- No royalties
- Complete ANSI C source code
- Real-time performance
- Responsive technical support
- Multiple class support
- Multiple class instances
- Integration of classes with ThreadX, FileX and NetX Duo
- Support for USB devices with multiple configurations
- Support for USB composite devices
- Support for USB power management
- Support for USB OTG
- Export trace events for TraceX

## Powerful Services of USBX

### Complete USB Device Framework Support

USBX can support the most demanding USB devices, including multiple configurations, multiple interfaces, and multiple alternate settings.

### Easy-To-Use APIs

USBX provides the best deeply embedded USB stack in a manner that is easy to understand and use. The USBX API makes the services intuitive and consistent. By using the provided USBX class APIs, the user application does not need to understand the complexity of the USB protocols.

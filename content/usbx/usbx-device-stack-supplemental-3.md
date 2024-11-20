---
title: Chapter 3 - USBX DPUMP Class Considerations
description: USBX contains a DPUMP class for the host and device side. This class is not a standard class in itself, but rather an example that illustrates how to create a simple device by using two bulk pipes and sending data back and forth on these two pipes
---


USBX contains a **DPUMP** class for the host and device side. This class is not a standard class in itself, but rather an example that illustrates how to create a simple device by using two bulk pipes and sending data back and forth on these two pipes. The **DPUMP** class could be used to start a custom class or for legacy RS232 devices.

USB DPUMP flow chart:

![USB DPUMP Flow Chart](../media/usbx-device-stack-supplemental/usb-dpump-flow-chart.png)

## USBX DPUMP Device Class

The device **DPUMP** class uses a thread, which is started upon connection to the USB host. The thread waits for a packet coming on the Bulk Out endpoint. When a packet is received, it copies the content to the Bulk In endpoint buffer and posts a transaction on this endpoint, waiting for the host to issue a request to read from this endpoint. This provides a loopback mechanism between the Bulk Out and Bulk In endpoints.

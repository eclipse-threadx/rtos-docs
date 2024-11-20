---
title: Chapter 2 - Installing and using NetX Duo Iperf
description: This chapter provides instructions for installing and using the Iperf sample.
---


This chapter contains a description of various issues related to installation, setup, and usage of the NetX Duo Iperf Demonstration.

## Installing the Demonstration

Please follow the platform specific installation instructions supplied in the distribution.

## Installing Iperf

There is a variety of Iperf programs that you may use. However, the examples in this document are based on the Java-based ***Jperf 2.0.2***, which is available from multiple sources on the Internet.

> **Note:** *Jperf requires that Java is installed on the host machine.*

## Setting the IP Address

By default, the IP address the NetX Duo Iperf Demonstration is set to **192.2.2.149**. This is setup in the file ***demo_netx_duo_iperf.c*** as a parameter to the call to ***nx_ip_create***.

## Network Assumptions

This demonstration assumes that the Iperf host machine and the target board running the NetX Duo Iperf Demonstration are connected to a 100Mbps full-duplex Ethernet switch. To achieve best performance, there
should be no other traffic on the test network.

It is also possible to connect the Iperf host and the NetX Duo target board back-to-back with a cross-over Ethernet cable.

## Running the Demonstration

Running the demonstration is easy; simply load, build and execute the NetX Duo Iperf Demonstration project -- typically named ***demo_netx_duo_iperf***.

## Browse to the Demonstration

Browse to the target board via a browser on the Iperf host platform. Assuming the target board IP address of **192.2.2.149**, the following is an example of the NetX Duo Iperf Demonstration initial web page.

![An example of the Iperf initial web page](media/picture1.jpg)

## Running Jperf

Running Jperf is easy, simply double-click on the Windows batch file ***jperf.bat***. This launches the Jperf IDE, as shown below. Once the Jperf IDE is displayed, the **Server Address** field must be set to the IP address of the NetX Duo Iperf Demonstration target board. In this example, the NetX Duo target board IP address is **192.2.2.149**. Also worth noting are the **UDP Bandwidth** and **UDP Packet Size** fields. These need to be set up for optimal UDP receive performance, as shown below.

![Optimizing the UDP performance.](media/picture2.jpg)

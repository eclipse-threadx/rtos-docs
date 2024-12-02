---
title: Chapter 1 - Introduction to NetX Duo
description: This chapter contains an introduction to NetX Duo and a description of its applications and benefits.
---


NetX Duo is a high-performance real time implementation of the TCP/IP standards designed exclusively for embedded ThreadX-based applications. This chapter contains an introduction to NetX Duo and a description of its applications and benefits.

## NetX Duo Unique Features

Unlike other TCP/IP implementations, NetX Duo is designed to be versatile—easily scaling from small micro-controller-based applications to those that use powerful RISC and DSP processors. This is in sharp contrast to public domain or other commercial implementations originally intended for workstation environments but then squeezed into embedded designs.

### Piconet Architecture

Underlying the superior scalability and performance of NetX Duo is <em>Piconet</em>, a software architecture especially designed for embedded systems. Piconet architecture maximizes scalability by implementing NetX Duo services as a C library. In this way, only those services used by the application are brought into the final runtime image. Hence, the actual size of NetX Duo is determined by the application. For most applications, the instruction image requirements of NetX Duo ranges between 5 KBytes and 30 KBytes in size. With IPv6 and ICMPv6 enabled for IPv6 address configuration and neighbor discovery protocols, NetX Duo ranges in size from 30 kbytes to 45 kbytes.

NetX Duo achieves superior network performance by layering internal component function calls only when it is necessary. In addition, much of NetX Duo processing is done directly in-line, resulting in outstanding performance advantages over the workstation network software used in embedded designs in the past.

### Zero-copy Implementation

NetX Duo provides a packet-based, zero-copy implementation of TCP/IP. Zero copy means that data in the application's packet buffer are never copied inside NetX Duo. This greatly improves performance and frees up valuable processor cycles to the application, which is important in embedded applications.

### UDP Fast Path Technology

With <em>UDP Fast Path Technology</em>, NetX Duo provides the fastest possible UDP processing. On the sending side, UDP processing—including the optional UDP checksum—is contained within the <em>**nx_udp_socket_send**</em> service. No additional function calls are made until the packet is ready to be sent via the internal NetX Duo IP send routine. This routine is also flat (that is, its function call nesting is minimal) so the packet is quickly dispatched to the application's network driver. When the UDP packet is received, the NetX Duo packet-receive processing places the packet directly on the appropriate UDP socket's receive queue or gives it to the first thread suspended waiting for a receive packet from the UDP socket's receive queue. No additional ThreadX context switches are necessary.

### ANSI C Source Code

NetX Duo is written completely in ANSI C and is portable immediately to virtually any processor architecture that has an ANSI C compiler and ThreadX support.

### Not A Black Box

Most distributions of NetX Duo include the complete C source code. This eliminates the "black-box" problems that occur with many commercial network stacks. By using NetX Duo, applications developers can see exactly what the network stack is doing—there are no mysteries!

Having the source code also allows for application-specific modifications. Although not recommended, it is beneficial to have the ability to modify the network stack if it is required.

These features are especially comforting to developers accustomed to working with in-house or public domain network stacks. They expect to have source code and the ability to modify it. NetX Duo is the ultimate network software for such developers.

### BSD-Compatible Socket API

For legacy applications, NetX Duo also provides a BSD-compatible socket interface that makes calls to the high-performance NetX Duo API underneath. This helps in migrating existing network application code to NetX Duo.

## RFCs Supported by NetX Duo

NetX Duo support of RFCs describing basic network protocols includes but is not limited to the following network protocols. NetX Duo follows all general recommendations and basic requirements within the constraints of a real-time operating system with small memory footprint and efficient execution.

| **RFC**  | **Description**                                        |
| -------- | ------------------------------------------------------ |
| RFC 1112 | Host Extensions for IP Multicasting (IGMPv1)           |
| RFC 1122 | Requirements for Internet Hosts - Communication Layers |
| RFC 2236 | Internet Group Management Protocol, Version 2          |
| RFC 768  | User Datagram Protocol (UDP)                           |
| RFC 791  | Internet Protocol (IP)                                 |
| RFC 792  | Internet Control Message Protocol (ICMP)               |
| RFC 793  | Transmission Control Protocol (TCP)                    |
| RFC 826  | Ethernet Address Resolution Protocol (ARP)             |
| RFC 903  | Reverse Address Resolution Protocol (RARP)             |
| RFC 5681 | TCP Congestion Control                                 |

Below are the IPv6-related RFCs supported by NetX Duo.

| **RFC**  | **Description**                                                                          |
| -------- | ---------------------------------------------------------------------------------------- |
| RFC 1981 | Path MTU Discovery for Internet Protocol v6 (IPv6)                                       |
| RFC 2460 | Internet Protocol v6 (IPv6) Specification                                                |
| RFC 2464 | Transmission of IPv6 Packets over Ethernet Networks                                      |
| RFC 4291 | Internet Protocol v6 (IPv6) Addressing Architecture                                      |
| RFC 4443 | Internet Control Message Protocol (ICMPv6) for Internet Protocol v6 (IPv6) Specification |
| RFC 4861 | Neighbor Discovery for IP v6                                                             |
| RFC 4862 | IPv6 Stateless Address Auto Configuration                                                |

## Embedded Network Applications

Embedded network applications are applications that need network access and execute on microprocessors hidden inside products such as cellular phones, communication equipment, automotive engines, laser printers, medical devices, and so forth. Such applications almost always have some memory and performance constraints. Another distinction of embedded network applications is that their software and hardware have a dedicated purpose.

Network software that must perform its processing within an exact period of time is called _real-time_ _network_ software, and when time constraints are imposed on network applications, they are classified as real-time applications. Embedded network applications are almost always real time because of their inherent interaction with the external world.

## NetX Duo Benefits

The primary benefits of using NetX Duo for embedded applications are high-speed Internet connectivity and small memory requirements. NetX Duo is also integrated with the high performance, multitasking ThreadX real-time operating system.

### Improved Responsiveness

The high-performance NetX Duo protocol stack enables embedded network applications to respond faster than ever before. This is especially important for embedded applications that either have a significant volume of network traffic or stringent processing requirements on a single packet.

### Software Maintenance

Using NetX Duo allows developers to easily partition the network aspects of their embedded application. This partitioning makes the entire development process easy and significantly enhances future software maintenance.

### Increased Throughput

NetX Duo provides the highest-performance networking available, which is achieved by minimal packet processing overhead. This also enables increased throughput.

### Processor Isolation

NetX Duo provides a robust, processor-independent interface between the application and the underlying processor and network hardware. This allows developers to concentrate on the network aspects of the application rather than spending extra time dealing with hardware issues directly affecting networking.

### Ease of Use

NetX Duo is designed with the application developer in mind. The NetX Duo architecture and service call interface are easy to understand. As a result, NetX Duo developers can quickly use its advanced features.

### Improve Time to Market

The powerful features of NetX Duo accelerate the software development process. NetX Duo abstracts most processor and network hardware issues, thereby removing these concerns from a majority of application network-specific areas. This, coupled with the ease-of-use and advanced feature set, result in a faster time to market!

### Protecting the Software Investment

NetX Duo is written exclusively in ANSI C and is fully integrated with the ThreadX real-time operating system. This means NetX Duo applications are instantly portable to all ThreadX supported processors. Better still, a new processor architecture can be supported with ThreadX in a matter of weeks. As a result, using NetX Duo ensures the application's migration path and protects the original development investment.

## IPv6 Ready Logo Certification

NetX Duo "IPv6 Ready" certification was obtained through the "IPv6 Core Protocol (Phase 2) Self Test" package available from the IPv6 Ready Organization. Refer to the following IPv6-Ready project websites for more information on the test platform and test cases:[**https://www.ipv6ready.org/**](https://www.ipv6ready.org/)

The Phase 2 IPv6 Core Protocol Self Test Suite validates that an IPv6 stack observes the requirements set forth in the following RFCs with extensive testing:  
Section 1: RFC 2460  
Section 2: RFC 4861  
Section 3: RFC 4862  
Section 4: RFC 1981  
Section 5: RFC 4443

## IxANVL Test

NetX Duo is tested with IxANVL from IXIA. IxANVL is the industry standard for automated network and protocol validation. More information about IxANVL can be found at: [**https://www.ixiacom.com/products/ixanvl**](https://www.ipv6ready.org/)

In particular the following NetX Duo modules are tested with IxANVL:

| **Module**      | **Standard**                                                      |
| --------------- | ----------------------------------------------------------------- |
| IP              | RFC791 </br> RFC1122 </br> RFC894                                 |
| ICMP            | RFC792 </br> RFC1122 </br> RFC1812                                |
| UDP             | RFC768 </br> RFC1122                                              |
| TCP-Core        | RFC793 </br> RFC1122 </br> RFC2460                                |
| TCP-Advanced    | RFC1981</br>RFC2001</br>RFC2385</br>RFC2463</br>RFC813</br>RFC896 |
| TCP-Performance | RFC793</br>RFC1323</br>RFC2018                                    |

## Safety Certifications

### TÜV Certification

NetX Duo has been certified by SGS-TÜV Saar for use in safety-critical systems, according to IEC-61508 SIL 4. The certification confirms that NetX Duo can be used in the development of safety-related software for the highest safety integrity levels of IEC-61508 for the "Functional Safety of electrical, electronic, and programmable electronic safety-related systems." SGS-TUV Saar, formed through a joint venture of Germany's SGS-Group and TUV Saarland, has become the leading accredited, independent company for testing, auditing, verifying, and certifying embedded software for safety-related systems worldwide.

{{< figure src="../media/user-guide/sgs-tuv-saar-logo.png" title="Diagram of SGS-TÜV Saar certification logo." imgClass="img-responsive center-block" >}}

- IEC 61508 up to SIL 4

### UL Certification

NetX Duo has been certified by UL for compliance with UL 60730-1 Annex H, CSA E60730-1 Annex H, IEC 60730-1 Annex H, UL 60335-1 Annex R, IEC 60335-1 Annex R, and UL 1998 safety standards for software in programmable components. Along with IEC/UL 60730-1, which has requirements for "Controls Using Software" in its Annex H, the IEC 60335-1 standard describes the requirements for "Programmable Electronic Circuits" in its Annex R. IEC 60730 Annex H and IEC 60335-1 Annex R address the safety of MCU hardware and software used in appliances such as washing machines, dishwashers, dryers, refrigerators, freezers, and ovens.

{{< figure src="../media/user-guide/c-ru-us-logo.png" title="Diagram of UL certification logo." imgClass="img-responsive center-block" >}}

_UL/IEC 60730, UL/IEC 60335, UL 1998_

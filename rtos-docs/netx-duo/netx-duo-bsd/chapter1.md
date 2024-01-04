---
title: Chapter 1 - Introduction to NetX Duo BSD
description: The BSD Socket API Compliancy Wrapper supports some of the basic BSD Socket API calls, with some limitations and utilizes NetX Duo primitives underneath.
---

# Chapter 1 - Introduction to NetX Duo BSD

The BSD Socket API Compliancy Wrapper supports some of the basic BSD Socket API calls, with some limitations and utilizes NetX Duo primitives underneath.

## BSD Socket API Compliancy Wrapper Source

The Wrapper source code is designed for simplicity and is comprised of two files, namely *nxd_bsd.h* and *nxd_bsd.c*. The *nxd_bsd.h* file defines all the necessary BSD Socket API wrapper constants and subroutine prototypes, while *nxd_bsd.c* contains the actual BSD Socket API compatibility source code. These Wrapper source files are common to all NetX Duo support packages.

The package consists of:

- **nxd_bsd.c**: Wrapper source code
- **nxd_bsd.h**: Main header file

Sample demo programs:

- **bsd_demo_udp.c**: *Demo with two UDP peers (IPv4 only)*
- **bsd_demo_tcp.c**: *Demo with a single TCP server and client*
- **bsd_demo_raw.c**: *Demo with two RAW peers*
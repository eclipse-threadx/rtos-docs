---
title: Appendix D - NetX Duo BSD-Compatible Socket API
description: Learn about the NetX Duo BSD-Compatible Socket API for IPv4 and IPv6.
---

# Appendix D - NetX Duo BSD-Compatible Socket API

## BSD-Compatible Socket API 
The BSD-Compatible Socket API supports a subset of the BSD Sockets API calls (with some limitations) by utilizing NetX Duo primitives underneath. Both IPv6 and IPv4 protocols and network addressing are supported. This BSD-Compatible Sockets API layer should perform as fast or slightly faster than typical BSD implementations because this API utilizes internal NetX Duo primitives and bypasses unnecessary NetX Duo error checking.  

Configurable options allow the host application to define the maximum number of sockets, TCP maximum window size, and depth of listen queue.

Due to performance and architecture constraints, this BSD-Compatible Sockets API does not support all BSD Sockets calls. In addition, not all BSD options are available for the BSD services, specifically the following:

  - ***select*** call works with only fd_set \*readfds, other arguments in this call e.g., writefds, exceptfds are not supported.
  - The "int flags" argument is not supported for ***send***, ***recv***, ***sendto,*** and ***recvfrom*** calls. 
  - The BSD-Compatible Socket API supports only limited set of BSD Sockets calls.

The source code is designed for simplicity and is comprised of only two files, ***nxd_bsd.c*** and ***nxd_bsd.h***. Installation requires adding these two files to the build project (not the NetX library) and creating the host application which will use BSD Socket service calls. The ***nxd_bsd.h*** file must also be included in your application source. Sample demo files for both IPv4 and IPv6  based applications are included with the distribution which is freely available with NetX Duo. Further details are available in the help and Readme files bundled with the BSDCompatible Socket API
package.

The BSD-Compatible Sockets API supports the following BSD Sockets API calls:

```c
INT     bsd_initialize (NX_IP *default_ip, NX_PACKET_POOL *default_pool, CHAR
        *bsd_memory_not_used);
```
```c
INT     getpeername( INT sockID, struct sockaddr *remoteAddress, INT
        *addressLength);
```
```c
INT     getsockname( INT sockID, struct sockaddr *localAddress, INT *addressLength);
```
```c
INT     recvfrom(INT sockID, CHAR *buffer, INT buffersize, INT flags,struct sockaddr
        *fromAddr, INT *fromAddrLen);
```
```c        
INT     recv(INT sockID, VOID *rcvBuffer, INT bufferLength, INT flags);
```
```c
INT     sendto(INT sockID, CHAR *msg, INT msgLength, INT flags, struct sockaddr
        *destAddr, INT destAddrLen);
```
```c        
INT     send(INT sockID, const CHAR *msg, INT msgLength, INT flags);
```
```c
INT     accept(INT sockID, struct sockaddr *ClientAddress, INT *addressLength);
```
```c
INT     listen(INT sockID, INT backlog);
```
```c
INT     bind (INT sockID, struct sockaddr *localAddress, INT addressLength);
```
```c
INT     connect(INT sockID, struct sockaddr *remoteAddress, INT addressLength);
```
```c
INT     socket( INT protocolFamily, INT type, INT protocol);
```
```c
INT     soc_close ( INT sockID);
```
```c
INT     select(INT nfds, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct
        timeval *timeout);
```
```c
VOID    FD_SET(INT fd, fd_set *fdset);
```
```c
VOID    FD_CLR(INT fd, fd_set *fdset);
```
```c
INT     FD_ISSET(INT fd, fd_set *fdset);
```
```c
VOID    FD_ZERO(fd_set *fdset);
```
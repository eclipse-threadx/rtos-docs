---
title: Chapter 1 - Introduction to NetX Duo FTP
description: The File Transfer Protocol (FTP) is a protocol designed for file transfers.
---

# Chapter 1 - Introduction to NetX Duo FTP

The File Transfer Protocol (FTP) is a protocol designed for file transfers. FTP utilizes reliable Transmission Control Protocol (TCP) services to perform its file transfer function. Because of this, FTP is a highly reliable file transfer protocol. FTP is also high-performance. The actual FTP file transfer is performed on a dedicated FTP connection. NetX Duo FTP accommodates both IPv4 and IPv6 networks. IPv6 does not directly change the FTP protocol, although some changes in the original NetX Duo FTP API are necessary to accommodate IPv6 and will be described in this document.

## FTP Requirements

In order to function properly, the NetX Duo FTP package requires NetX Duo. The host application must create an IP instance for running NetX Duo services and periodic tasks. If running the FTP host application over an IPv6 network, IPv6, and ICMPv6 must be enabled on the IP task. TCP must be also enabled for either IPv6 or IPv4 networks. The IPv6 host application must set its linklocal and global IPv6 address using the IPv6 API and/or DHCPv6. A demo program in section "Small Example System" in **Chapter 2** demonstrates how this is done.

The FTP Server and Client are also designed to work with the FileX embedded file system. If FileX is not available, the host developer can implement or substitute their own file system along the guidelines suggested in filex_stub.h by defining each of the services listed in that file. This is discussed in later sections of this guide.

The FTP Client portion of the NetX Duo FTP package has no further requirements.

The FTP Server portion of the NetX Duo FTP package has several additional requirements. First, it requires complete access to TCP *well-known port 21* for handling all Client FTP command requests and *well-known port 20* for handling all Client FTP data transfers.

## FTP Constraints

The FTP standard has many options regarding the representation of file data. NetX Duo FTP does not implement switch options e.g. ls –al. NetX Duo FTP Server expects to receive requests and their arguments in a single packet rather than consecutive packets.

Similar to UNIX implementations, NetX Duo FTP assumes the following file format constraints:

- File Type: **Binary**
- File Format: **Nonprint Only**
- File Structure: **File Structure Only**

## FTP File Names

FTP file names should be in the format of the target file system (usually FileX). They should be NULL terminated ASCII strings, with full path information if necessary. There is no specified limit for the size of FTP file names in the NetX Duo FTP implementation. However, the packet pool payload size should be able to accommodate the maximum path and/or file name.

## FTP Client Commands

The FTP has a simple mechanism for opening connections and performing file and directory operations. There is basically a set of standard FTP commands that are issued by the Client after a connection has been successfully established on the TCP *well-known port 21*. The following shows some of the basic FTP commands. Note that the only difference when FTP runs over IPv6 is that the PORT command is replaced with the EPRT command:

- CWD path *Change working directory*
- DELE filename *Delete specified file name*
- EPRT ip_address, port *Provide IPv6 address and Client data port*
- EPSV *Request passive transfer mode for IPv6*
- LIST directory *Get directory listing*
- MKD directory *Make new directory*
- NLST directory *Get directory listing*
- NOOP *No operation, returns success*
- PASS password *Provide password for login*
- PASV *Request passive transfer mode for IPv4*
- PORT ip_address,port *Provide IP address and Client data port*
- PWD path *Pickup current directory path*
- QUIT *Terminate Client connection*
- RETR filename *Read specified file*
- RMD directory *Delete specified directory*
- RNFR oldfilename *Specify file to rename*
- RNTO newfilename *Rename file to supplied file name*
- STOR filename *Write specified file*
- TYPE I *Select binary file image*
- USER username *Provide username for login*

These ASCII commands are used internally by the NetX Duo FTP Client software to perform FTP operations with the FTP Server.

## FTP Server Responses

Once the FTP Server processes the Client request, it returns a 3-digit coded response in ASCII followed by optional ASCII text. The numeric response is used by the FTP Client software to determine whether the operation succeeded or failed. The following list shows various FTP Server responses to Client requests:

**First Numeric Field Meaning**

- 1xx *Positive preliminary status – another reply coming*.
- 2xx *Positive completion status*.
- 3xx *Positive preliminary status – another command must be sent*.
- 4xx *Temporary error condition*.
- 5xx *Error condition.*

**Second Numeric Field Meaning**

- x0x *Syntax error in command*.
- x1x *Informational message*.
- x2x *Connection related*.
- x3x *Authentication related*.
- x4x *Unspecified*.
- x5x *File system related*.

For example, a Client request to disconnect an FTP connection with the QUIT command will typically be responded with a "221" code from the Server – if the disconnect is successful.

## FTP Passive Transfer Mode

By default, the NetX Duo FTP Client uses the active transport mode to exchange data over the data socket with the FTP server. The problem with this arrangement is that it requires the FTP Client to open a TCP server socket for the FTP Server to connect to. This represents a possible security risk and may be blocked by the Client firewall. Passive transfer mode differs from active transport mode by having the FTP server create the TCP server socket on the data connection. This eliminates the security risk (for the FTP Client).

To enable passive data transfer, the application calls *nx_ftp_client_passive_mode_set* on a previously created FTP Client with the second argument set to NX_TRUE. Thereafter, all subsequent NetX Duo FTP Client services for transferring data (NLST, RETR, STOR) are attempted in the passive transport mode.

The FTP Client first sends the passive command (no arguments), PASV command for IPv4 or EPSV command for IPv6. If the FTP server supports this request it will return the 227 "OK" response for PASV, and 229 "OK" response for EPSV. Then the Client sends the request e.g. RETR. If the server refuses passive transfer mode, the NetX Duo FTP Client service returns an error status.

To disable passive transport mode and return to active transport mode, the application calls *nx_ftp_client_passive_mode_set* with the second argument set to NX_FALSE.

## FTP Communication

The FTP Server utilizes the *well-known TCP port 21* to field Client requests. FTP Clients may use any available TCP port. The general sequence of FTP events is as follows:

**FTP Read File Requests**:

1. Client issues TCP connect to Server port 21.
1. Server sends "220" response to signal success.
1. Client sends "USER" message with "username."
1. Server sends "331" response to signal success.
1. Client sends "PASS" message with "password."
1. Server sends "230" response to signal success.
1. Client sends "TYPE I" message for binary transfer.
1. Server sends "200" response to signal success.
1. *IPv4 applications*: Client sends "PORT" message with IP address and port.<br />*IPv6 applications*: Client sends "EPRT" message with IP address and port.
1. Server sends "200" response to signal success.
1. Client sends "RETR" message with file name to read.
1. Server creates data socket and connects with client data port specified in the "PORT" command.
1. Server sends "125" response to signal file read has started.
1. Server sends contents of file through the data connection. This process continues until file is completely transferred.
1. When finished, Server disconnects data connection.
1. Server sends "250" response to signal file read is successful.
1. Client sends "QUIT" to terminate FTP connection.
1. Server sends "221" response to signal disconnect is successful.
1. Server disconnects FTP connection.

As mentioned previously, the only difference between FTP running over IPv4 and IPv6 is the PORT command is replaced with the EPRT command for IPv6.

If the FTP Client makes a read request in the passive transfer mode, the command sequence is as follows (**bolded** lines indicates a different step from active transfer mode):

1. Client issues TCP connect to Server port 21.
1. Server sends "220" response to signal success.
1. Client sends "USER" message with "username."
1. Server sends "331" response to signal success.
1. Client sends "PASS" message with "password."
1. Server sends "230" response to signal success.
1. Client sends "TYPE I" message for binary transfer.
1. Server sends "200" response to signal success.
1. ***IPv4 applications:* Client sends "PASV" message.**<br />***IPv6 applications:* Client sends "EPSV" message.**
1. ***IPv4 applications:* Server sends "227" response, and IP address and port for the Client to connect to, to signal success.**<br />***IPv6 applications:* Server sends "229" response, and IP address and port for the Client to connect to, to signal success.**
1. Client sends "RETR" message with file name to read.
1. **Server creates data server socket and listens for the Client connect request on this socket using the port specified in the response in step 10.**
1. **Server sends "150" response on the control socket to signal file read has started.**
1. Server sends contents of file through the data connection. This process continues until file is completely transferred.
1. When finished, Server disconnects data connection.
1. **Server sends "226" response on the control socket to signal file read is successful.**
1. Client sends "QUIT" to terminate FTP connection.
1. Server sends "221" response to signal disconnect is successful.
1. Server disconnects FTP connection.

**FTP Write Requests**:

1. Client issues TCP connect to Server port 21.
1. Server sends "220" response to signal success.
1. Client sends "USER" message with "username."
1. Server sends "331" response to signal success.
1. Client sends "PASS" message with "password."
1. Server sends "230" response to signal success.
1. Client sends "TYPE I" message for binary transfer.
1. Server sends "200" response to signal success.
1. *IPv4 applications*: Client sends "PORT" message with IP address and port.<br />*IPv6 applications*: Client sends "EPRT" message with IP address and port.
1. Server sends "200" response to signal success.
1. Client sends "STOR" message with file name to write.
1. Server creates data socket and connects with client data port specified in the previous "EPRT" or "PORT" command.
1. Server sends "125" response to signal file write has started.
1. Client sends contents of file through the data connection. This process continues until file is completely transferred.
1. When finished, Client disconnects data connection.
1. Server sends "250" response to signal file write is successful.
1. Client sends "QUIT" to terminate FTP connection.
1. Server sends "221" response to signal disconnect is successful.
1. Server disconnects FTP connection.

If the FTP Client makes a write request in the passive transfer mode, the command sequence is as follows (**bolded** lines indicates a different step from active transfer mode):

1. Client issues TCP connect to Server port 21.
1. Server sends "220" response to signal success.
1. Client sends "USER" message with "username."
1. Server sends "331" response to signal success.
1. Client sends "PASS" message with "password."
1. Server sends "230" response to signal success.
1. Client sends "TYPE I" message for binary transfer.
1. Server sends "200" response to signal success.
1. ***IPv4 applications:* Client sends "PASV" message.**<br />***IPv6 applications:* Client sends "EPSV" message.**
1. ***IPv4 applications:* Server sends "227" response, and IP address and port for the Client to connect to, to signal success.**<br />***IPv6 applications:* Server sends "229" response, and IP address and port for the Client to connect to, to signal success.**
1. Client sends "STOR" message with file name to write.
1. **Server creates data server socket and listens for the Client connect request on this socket using the port specified in the response in step 10.**
1. **Server sends "150" response on the control socket to signal file write has started.**
1. Client sends contents of file through the data connection. This process continues until file is completely transferred.
1. When finished, Client disconnects data connection.
1. **Server sends "226" response on the control socket to signal file write is successful.**
1. Client sends "QUIT" to terminate FTP connection.
1. Server sends "221" response to signal disconnect is successful.
1. Server disconnects FTP connection.

## FTP Authentication

Whenever an FTP connection takes place, the Client must provide the Server with a *username* and *password*. Some FTP sites allow what is called *Anonymous FTP*, which allows FTP access without a specific username and password. For this type of connection, "anonymous" should be supplied for username and the password should be a complete e-mail address.

The user is responsible for supplying NetX Duo FTP with login and logout authentication routines. These are supplied during the ***nxd_ftp_server_create*** and ***nx_ftp_server_create*** services and called from the password processing. The difference between the two is the ***nxd_ftp_server_create*** input function pointers to login and logout authenticate functions expect the NetX Duo address type ***NXD_ADDRESS***. This data type holds both IPv4 or IPv6 address formats, making this function the "duo" service supporting both IPv4 and IPv6 networks. The ***nx_ftp_server_create*** input function pointers to login and logout authenticate functions expect ULONG IP address type. This function is limited to IPv4 networks. The developer is encouraged to use the "duo" service whenever possible.

If the *login* function returns NX_SUCCESS, the connection is authenticated and FTP operations are allowed. Otherwise, if the *login* function returns something other than NX_SUCCESS, the connection attempt is rejected.

## FTP Multi-Thread Support

The NetX Duo FTP Client services can be called from multiple threads simultaneously. However, read or write requests for a particular FTP Client instance should be done in sequence from the same thread.

## FTP RFCs

NetX Duo FTP is compliant with RFC 959, RFC 2428 and related RFCs.

---
title: Chapter 1 - Introduction to NetX Duo TFTP
description: "The Trivial File Transfer Protocol (TFTP) is a lightweight protocol designed for file transfers."
---


The Trivial File Transfer Protocol (TFTP) is a lightweight protocol designed for file transfers. Unlike more robust protocols, TFTP does not perform extensive error checking and can also have limited performance because it is a stop-and-wait protocol. After a TFTP data packet is sent, the sender waits for an ACK to be returned by the recipient. Although this is simple, it does limit the overall TFTP throughput. The TFTP package enables hosts to use the TFTP protocol over IP networks.

## TFTP Requirements

In order to function properly, the TFTP Clients portion of the NetX Duo TFTP package requires that an IP instance has already been created. In addition, UDP must be enabled on that same IP instance. The Client portion of the NetX Duo TFTP package has no further requirements.

The TFTP Server portion of the NetX Duo TFTP package has several additional requirements. First, it requires complete access to the UDP *well known port 69* for handling all client TFTP requests. The TFTP Server is also designed for use with the FileX embedded file system. If FileX is not available, the user may port the portions of FileX used to their own environment. This is discussed in later sections of this guide.

## TFTP File Names 

TFTP file names should be in the format of the target file system. They should be NULL terminated ASCII strings, with full path information if necessary. There is no specified limit in the size of TFTP file names in the NetX Duo TFTP implementation.

## TFTP Messages

The TFTP has a very simple mechanism for opening, reading, writing, and closing files. There are basically 2-4 bytes of TFTP header underneath the UDP header. The definition of the TFTP file open messages has the following format:

**oooof…f0OCTET0**

Where:

- **oooo** 2-byte Opcode field  
0x0001 -> Open for read  
0x0002 -> Open for write

- **f…f** n-byte Filename field

- 0  1-byte NULL termination character

- **OCTET** ASCII "OCTET" to specify binary transfer

- 0  1-byte NULL termination character

The definition of the TFTP write, ACK, and error messages are slightly different and are defined as follows:

**oooobbbbd…d**

Where:

- **oooo** 2-byte Opcode field  
0x0003 -> Data packet  
0x0004 -> ACK for last read  
0x0005 -> Error condition  

- **bbbb** 2-byte Block Number field (1-n)

- **d…d** n-byte Data field


- 0x0001 (read) File Name 0 OCTET 0

- 0x0002 (write) File Name 0 OCTET 0

## TFTP Communication

TFTP Servers utilize the well-known UDP port 69 to listen for Client requests. TFTP Client sockets may bind to any available UDP port. Data packet payload containing the file to upload or download is sent in 512 byte chunks, until the last packet containing < 512 bytes. Therefore a packet containing fewer than 512 bytes signals the end of file. The general sequence of events is as follows:

TFTP Read File Requests:

1.  The Client issues an "Open For Read" request with the file name and waits for a reply from the Server.

2.  The Server sends the first 512 bytes of the file or less if the file size is less than 512 bytes.

3.  The Client receives data, sends an ACK, and waits for the next packet from the Server for files containing more than 512 bytes.

4.  The sequence ends when the Client receives a packet containing fewer than 512 bytes.

TFTP Write Requests:

1.  The Client issues an "Open for Write" request with the file name and waits for an ACK with a block number of 0 from the Server.

2.  When the Server is ready to write the file, it sends an ACK with a block number of zero.

3.  The Client sends the first 512 bytes of the file (or less for files less than 512 bytes) to the Server and waits for an ACK back.

4.  The Server sends an ACK after the bytes are written.

5.  The sequence ends when the Client completes writing a packet containing fewer than 512 bytes.
 

## TFTP Server Session Timer

The TFTP Server has a limited number of client request slots. If a client session appears to be dropped, that slot cannot be available for re-use. However if the NX_TFTP_SERVER_RETRANSMIT_ENABLE option is enabled, the NetX Duo TFTP Server creates an session timer that monitors the timeout on each of its client sessions. When a session timeout expires it is terminated and any open files are closed. Thus the 'slot' becomes available for another TFTP Client request.

To set the timeout, adjust the configuration option NX_TFTP_SERVER_RETRANSMIT_TIMEOUT which by default is 200 timer ticks. The interval between which session timeouts are checked is set by the NX_TFTP_SERVER_TIMEOUT_PERIOD which is 20 timer ticks by default.

When the TFTP Client has uploaded (written) files to the TFTP FileX media, the media needs to be flushed so that data can be written from the TFTP server ram disk to the underlying media (TFTP Client disk memory). This can be done using the fx_media_flush service if the application does not wish to close the TFTP Server.

However, after closing the TFTP server, the application should, if it has no further use for the FileX media, then close the media using the fx_media_close service. This will flush the file data back to the TFTP Client disk, close open files, and update the directory information to the media.

The "Small Example" section in this chapter demonstrates this.

A third option for updating the FileX media is to compile the FileX library with the FX_FAULT_TOLERANT or the FX_FAULT_TOLERANT_DATA options instead of using FileX services explicitly. If defined, FileX automatically passes write requests to the media driver. These options may limit performance but offer greater protection from lost file data or lost clusters. For more information about this and FileX in general, please see the Express Logic FileX User Guide.

## TFTP Multi-Thread Support

The NetX Duo TFTP Client services can be called from multiple threads simultaneously. However, read or write requests for a particular TFTP Client instance should be done in sequence from the same thread.

## TFTP RFCs

NetX Duo TFTP is compliant with RFC1350 and related RFCs.


---
title: Chapter 3 - Description of NetX Duo TFTP services
description: "This chapter contains a description of all NetX Duo TFTP services (listed below) in alphabetic order."
---


This chapter contains a description of all NetX Duo TFTP services (listed below) in alphabetic order. Unless otherwise specified, all services support IPv6 and IPv4 communications.

In the **Return Values** section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

> **Note:** The IPv4 equivalents of all the services listed above are available in NetX Duo TFTP Client and Server e.g. *nx_tftp_server_create* and *nx_tftp_client_file_open*. Only the 'Duo' API descriptions, e.g. services beginning with *nxd_*, are provided in the following pages. Where an NXD_ADDRESS \* input is specified, the IPv4 equivalent API calls for ULONG input. Otherwise there is no difference in using the API.

## nxd_tftp_client_create

Create a TFTP Client instance

### Prototype

```C
UINT nxd_tftp_client_create(
    NX_TFTP_CLIENT *tftp_client_ptr,
    CHAR *tftp_client_name,
    NX_IP *ip_ptr,
    NX_PACKET_POOL *pool_ptr);
```

### Description

This service creates a TFTP Client instance for the previously created IP instance.

> **Important:** The application must make certain the supplied IP and packet pool are already created. In addition, UDP must be enabled for the IP instance prior to calling this service.

### Input Parameters

- *tftp_client_ptr*: Pointer to TFTP Client control block.

- *tftp_client_name*: Name of this TFTP Client instance

- *ip_ptr*: Pointer to previously created IP instance.

- *pool_ptr*: Pointer to packet pool TFTP Client instance.

### Return Values

- **NX_SUCCESS**(0x00) Successful TFTP create.

- **NX_TFTP_INVALID_IP_VERSION** (0x0C) Invalid or unsupported IP version

- **NX_TFTP_INVALID_SERVER_ADDRESS** (0x08) Invalid Server IP address received

- **NX_TFTP_NO_ACK_RECEIVED** (0x09) Server ACK not received

- NX_PTR_ERROR (0x16) Invalid IP, pool, or TFTP pointer.

- NX_INVALID_PARAMETERS (0x4D) Invalid non pointer input

- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Initialization and Threads

### Example

```C
/* Create a TFTP Client instance. */
status =  nxd_tftp_client_create(&my_tftp_client, "My TFTP Client", 
													&my_ip, &pool_ptr);

/* If status is NX_SUCCESS a TFTP Client instance was successfully
   created. */
```

## nxd_tftp_client_delete

Delete a TFTP Client instance

### Prototype

```C
UINT nxd_tftp_client_delete(NX_TFTP_CLIENT *tftp_client_ptr);
```

### Description

This service deletes a previously created TFTP Client instance.

### Input Parameters

- *tftp_client_ptr*: Pointer to previously created TFTP client instance.

### Return Values

- **NX_SUCCESS** (0x00) Successful TFTP Client delete.

- NX_PTR_ERROR (0x16) Invalid pointer input.

- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Delete a TFTP Client instance. */
status =  nxd_tftp_client_delete(&my_tftp_client);

/* If status is NX_SUCCESS the TFTP Client instance was successfully
   deleted. */
```

## nxd_tftp_client_error_info_get

Get client error information

### Prototype

```C
UINT nxd_tftp_client_error_info_get(
    NX_TFTP_CLIENT *tftp_client_ptr,
    UINT *error_code,
    CHAR **error_string);
```

### Description

This service returns the last error code received and sets the pointer to the client's internal error string. In error conditions, the user can view the last error sent by the server. A null error string indicates no error is present.

### Input Parameters

- *tftp_client_ptr*: Pointer to previously created TFTP Client instance.

- *error_code*: Pointer to destination area for error code

- *error_string*: Pointer to destination for error string

### Return Values

- **NX_SUCCESS** (0x00) Successful TFTP error info get.  

- NX_PTR_ERROR (0x16) Invalid TFTP Client pointer.

- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Get error information for client. */
status =  nxd_tftp_client_error_info_get(&my_tftp_client, &error_code,
					&error_string_ptr);

/* If status is NX_SUCCESS the error code and error string are available. */
```

## nxd_tftp_client_file_close

Close client file

### Prototype

```C
UINT nxd_tftp_client_file_close(
    NX_TFTP_CLIENT *tftp_client_ptr,
    UINT ip_type);
```

### Description

This service closes the previously opened file by this TFTP Client instance. A TFTP Client instance is allowed to have only one file open at a time.

### Input Parameters

- *tftp_client_ptr*: Pointer to previously created TFTP Client instance.

- *ip_type*: Indicate which IP protocol to use. Valid options are IPv4 (4) or IPv6 (6).

### Return Values

- **NX_SUCCESS** (0x00) Successful TFTP file close.

- NX_PTR_ERROR (0x16) Invalid pointer input.

- NX_CALLER_ERROR (0x11) Invalid caller of this service.

- NX_INVALID_PARAMETERS (0x4D) Invalid non pointer input.

### Allowed From

Threads

### Example

```C
/* Close the previously opened file associated with "my_client". */
status =  nxd_tftp_client_file_close(&my_tftp_client);

/* If status is NX_SUCCESS the TFTP file is closed. */
```

## nx_tftp_client_file_open

Open TFTP client file

### Prototype

```C
UINT nx_tftp_client_file_open(
    NX_TFTP_CLIENT *tftp_client_ptr, 
    CHAR *file_name,
    NXD_ADDRESS *server_ip_address,
    UINT open_type,
    ULONG wait_option);
```

### Description

This service attempts to open the specified file on the TFTP Server at the specified IP address. The file will be opened for either reading or writing. 

> **Note:** This is limited to IPv4 packets only, and is intended for supporting NetX TFTP applications. Developers are encouraged to port their applications to using equivalent "duo" service *nxd_tftp_client_file_open.*

### Input Parameters

- *tftp_client_ptr*: Pointer to TFTP control block.

- *file_name*: ASCII file name, NULL-terminated and with appropriate path information.

- *server_ip_address*: Server TFTP address.

- *open_type*: Type of open request, either:

   - **NX_TFTP_OPEN_FOR_READ** (0x01)

   - **NX_TFTP_OPEN_FOR_WRITE** (0x02)

- *wait_option*: Defines how long the service will wait for the TFTP Client file open. The wait options are defined as follows:

    - **timeout value** (0x00000001 through 0xFFFFFFFE)

    - **TX_WAIT_FOREVER** (0xFFFFFFFF) 
  
    - Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until a TFTP Server responds to the request.
  
    - Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the TFTP server response.

- *ip_type*: Indicate which IP protocol to use. Valid options are IPv4 (4) or IPv6 (6).

### Return Values

- **NX_SUCCESS** (0x00) Successful Client file open

- **NX_TFTP_NOT_CLOSED** (0xC3) Client already has file open

- **NX_INVALID_TFTP_SERVER_ADDRESS** (0x08) Invalid server address received

- **NX_TFTP_NO_ACK_RECEIVED** (0x09) No ACK received from server

- **NX_TFTP_INVALID_SERVER_ADDRESS** (0x08) Invalid Server IP received

- **NX_TFTP_CODE_ERROR** (0x05) Received error code

- NX_PTR_ERROR (0x16) Invalid pointer input.

- NX_CALLER_ERROR (0x11) Invalid caller of this service

- NX_IP_ADDRESS_ERROR (0x21) Invalid Server IP address

- NX_OPTION_ERROR (0x0A) Invalid open type

### Allowed From

Threads

### Example

```C
/* Define the TFTP server address. */
NXD_ADDRESS server_ip_address;

server_ip_address.nxd_ip_version = NX_IP_VERSION_V6;
server _ip_address.nxd_ip_address.v6[0] = 0x20010db8;
server _ip_address.nxd_ip_address.v6[1] = 0xf101;
server _ip_address.nxd_ip_address.v6[2] = 0;
server _ip_address.nxd_ip_address.v6[3] = 0x101;

/* Open file "test.txt" for reading on the TFTP Server. */
status =  nxd_tftp_client_file_open(&my_tftp_client, "test.txt",
 		& server_ip_address, NX_TFTP_OPEN_FOR_READ, 200);

/* If status is NX_SUCCESS the "test.txt" file is now open for reading. */
```

## nxd_tftp_client_file_open

Open TFTP client file

### Prototype

```C
UINT nxd_tftp_client_file_open(
    NX_TFTP_CLIENT *tftp_client_ptr,
		CHAR *file_name, NXD_ADDRESS *server_ip_address,
    UINT open_type,
    ULONG wait_option,
    UINT ip_type);
```

### Description

This service attempts to open the specified file on the TFTP Server at the specified IPv6 address. The file will be opened for either reading or writing.

### Input Parameters

- *tftp_client_ptr*: Pointer to TFTP control block.

- *file_name*: ASCII file name, NULL-terminated and with appropriate path information.

- *server_ip_address*: Server TFTP address.

- *open_type*: Type of open request, either:

   - **NX_TFTP_OPEN_FOR_READ** (0x01)

   - **NX_TFTP_OPEN_FOR_WRITE** (0x02)

- *wait_option*: Defines how long the service will wait for the TFTP Client file open. The wait options are defined as follows:

   - **timeout value** (0x00000001 through 0xFFFFFFFE)

   - **TX_WAIT_FOREVER** (0xFFFFFFFF)

   - Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until a TFTP Server responds to the request.

   - Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the TFTP server response.

- *ip_type*: Indicate which IP protocol to use. Valid options are IPv4 (4) or IPv6 (6).

### Return Values

- **NX_SUCCESS** (0x00) Successful Client file open

- **NX_TFTP_NOT_CLOSED** (0xC3) Client already has file open

- **NX_INVALID_TFTP_SERVER_ADDRESS** (0x08) Invalid server address received

- **NX_TFTP_NO_ACK_RECEIVED** (0x09) No ACK received from server

- **NX_TFTP_INVALID_IP_VERSION** (0x0C) Invalid IP version

- **NX_TFTP_INVALID_SERVER_ADDRESS** (0x08) Invalid Server IP received

- **NX_TFTP_CODE_ERROR** (0x05) Received error code

- NX_PTR_ERROR (0x16) Invalid pointer input.

- NX_CALLER_ERROR (0x11) Invalid caller of this service

- NX_IP_ADDRESS_ERROR (0x21) Invalid Server IP address

- NX_OPTION_ERROR (0x0A) Invalid open type

- NX_INVALID_PARAMETERS (0x4D) Invalid non pointer input

### Allowed From

Threads

### Example

```C
/* Define the TFTP server address. */
NXD_ADDRESS server_ip_address;

server_ip_address.nxd_ip_version = NX_IP_VERSION_V6;
server _ip_address.nxd_ip_address.v6[0] = 0x20010db8;
server _ip_address.nxd_ip_address.v6[1] = 0xf101;
server _ip_address.nxd_ip_address.v6[2] = 0;
server _ip_address.nxd_ip_address.v6[3] = 0x101;

/* Open file "test.txt" for reading on the TFTP Server. */
status =  nxd_tftp_client_file_open(&my_tftp_client, "test.txt",
				& server_ip_address, NX_TFTP_OPEN_FOR_READ, 200);

/* If status is NX_SUCCESS the "test.txt" file is now open for reading. */
```

## nxd_tftp_client_file_read

Read a block from client file

### Prototype

```C
UINT nxd_tftp_client_file_read(
    NX_TFTP_CLIENT *tftp_client_ptr,
    NX_PACKET **packet_ptr,
    ULONG wait_option,
    UINT ip_type);
```

### Description

This service reads a 512-byte block from the previously opened TFTP Client file. A block containing fewer than 512 bytes signals the end of the file.

### Input Parameters

- *tftp_client_ptr*: Pointer to TFTP Client control block.

- *packet_ptr*: Destination for packet containing the block read from the file.

- *wait_option*: Defines how long the service will wait for the read to complete. The wait options are defined as follows:

   - **timeout value** (0x00000001 through 0xFFFFFFFE)

   - **TX_WAIT_FOREVER** (0xFFFFFFFF)

   - Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the TFTP Server responds to the request.

   - Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the TFTP server to send a block of the file.

- *ip_type*: Indicate which IP protocol to use. Valid options are IPv4 (4) or IPv6 (6).

### Return Values

- **NX_SUCCESS** (0x00) Successful Client block read

- **NX_TFTP_NOT_OPEN** (0xC3) Specified Client file is not open for reading

- **NX_NO_PACKET** (0x01) No Packet received from Server.

- **NX_INVALID_TFTP_SERVER_ADDRESS** (0x08) Invalid server address received

- **NX_TFTP_NO_ACK_RECEIVED** (0x09) No ACK received from Server

- **NX_TFTP_END_OF_FILE** (0xC5) End of file detected (not an error).

- **NX_TFTP_INVALID_IP_VERSION** (0x0C) Invalid IP version

- **NX_TFTP_CODE_ERROR** (0x05) Received error code

- **NX_TFTP_FAILED** (0xC2) Unknown TFTP code received

- **NX_TFTP_INVALID_BLOCK_NUMBER** (0x0A) Invalid block number received

- NX_PTR_ERROR (0x16) Invalid pointer input.

- NX_CALLER_ERROR (0x11) Invalid caller of this service

- NX_INVALID_PARAMETERS (0x4D) Invalid non pointer input

### Allowed From

Threads

### Example

```C
/* Read a block from a previously opened file of "my_client". */
status =  nxd_tftp_client_file_read(&my_tftp_client, &return_packet_ptr, 200);

/* If status is NX_SUCCESS a block of the TFTP file is in the payload of
   "return_packet_ptr". */
```

## nxd_tftp_client_file_write

Write a block to Client file

### Prototype

```C
UINT nxd_tftp_client_file_write(
    NX_TFTP_CLIENT *tftp_client_ptr,
    NX_PACKET *packet_ptr,
    ULONG wait_option, UINT ip_type);
```

### Description

This service writes a 512-byte block to the previously opened TFTP Client file. Specifying a block containing fewer than 512 bytes signals the end of the file.

### Input Parameters

- *tftp_client_ptr*: Pointer to TFTP Client control block.

- *packet_ptr*: Packet containing the block to write to the file.

- *wait_option*: Defines how long the service will wait for the write to complete. The wait options are defined as follows:

   - **timeout value** (0x00000001 through 0xFFFFFFFE)

   - **TX_WAIT_FOREVER** (0xFFFFFFFF)

   - Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the TFTP Server responds to the request.
 
   - Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the TFTP server to send an ACK for the write request.

- *ip_type*: Indicate which IP protocol to use. Valid options are IPv4 (4) or IPv6 (6).

### Return Values

- **NX_SUCCESS** (0x00) Successful Client block write

- **NX_TFTP_NOT_OPEN** (0xC3) Specified Client file is not open for writing

- **NX_TFTP_TIMEOUT** (0xC1) Timeout waiting for Server ACK

- **NX_INVALID_TFTP_SERVER_ADDRESS** (0x08) Invalid server address received

- **NX_TFTP_NO_ACK_RECEIVED** (0x09) No ACK received from server

- **NX_TFTP_INVALID_IP_VERSION** (0x0C) Invalid IP version

- **NX_INVALID_TFTP_SERVER_ADDRESS** (0x08) Invalid server address received

- **NX_TFTP_CODE_ERROR** (0x05) Received error code

- NX_PTR_ERROR (0x16) Invalid pointer input.

- NX_CALLER_ERROR (0x11) Invalid caller of this service

- NX_INVALID_PARAMETERS (0x4D) Invalid non pointer input

### Allowed From

Threads

### Example

```C
/* Write a block to the previously opened file of "my_client". */
status =  nxd_tftp_client_file_write(&my_tftp_client, packet_ptr, 200);

/* If status is NX_SUCCESS the block in the payload of "packet_ptr" was 
   written to the TFTP file opened by "my_client". */
```

## nxd_tftp_client_packet_allocate

Allocate packet for Client file write

### Prototype

```C
UINT nxd_tftp_client_packet_allocate(
    NX_PACKET_POOL *pool_ptr,
    NX_PACKET **packet_ptr,
    ULONG wait_option,
    UINT ip_type);
```

### Description

This service allocates a UDP packet from the specified packet pool and makes room for the 4-byte TFTP header before the packet is returned to the caller. The caller can then build a buffer for writing to a client file.

### Input Parameters

- *pool_ptr*: Pointer to packet pool.

- *packet_ptr*: Destination for pointer to allocated packet.

- *wait_option*: Defines how long the service will wait for the packet allocate to complete. The wait options are defined as follows:

   - **timeout value** (0x00000001 through 0xFFFFFFFE)

   - **TX_WAIT_FOREVER** (0xFFFFFFFF)

   - Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the allocation completes.

   - Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the packet allocation.

- *ip_type*: Indicate which IP protocol to use. Valid options are IPv4 (4) or IPv6 (6).

### Return Values

- **NX_SUCCESS** (0x00) Successful packet allocate

- NX_PTR_ERROR (0x16) Invalid pointer input.

- NX_CALLER_ERROR (0x11) Invalid caller of this service

- NX_INVALID_PARAMETERS (0x4D) Invalid non pointer input

### Allowed From

Threads

### Example

```C
/* Allocate a packet for TFTP file write. */
status =  nxd_tftp_client_packet_allocate(&my_pool, &packet_ptr, 200);

/* If status is NX_SUCCESS "packet_ptr" contains the new packet. */
```

## nxd_tftp_client_set_interface

Set physical interface for TFTP requests

### Prototype

```C
UINT nxd_tftp_client_set_interface(
    NX_TFTP_CLIENT *tftp_client_ptr,
    UINT if_index);
```

### Description

This service uses the input interface index to set the physical interface for the TFTP Client to send and receive TFTP packets. The default value is zero, for the primary interface.

> **Note:** NetX Duo must support multihome addressing (v5.6 or later) to use this service.

### Input Parameters

- *tftp_client_ptr*: Pointer to TFTP Client instance

- *if_index*: Index of physical interface to use

### Return Values

- **NX_SUCCESS** (0x00) Successfully set interface (0x0B) Invalid interface input

- NX_PTR_ERROR (0x16) Invalid pointer input.

- NX_CALLER_ERROR (0x11) Invalid caller of this service

- NX_TFTP_INVALID_INTERFACE (0x0B) Invalid interface input

### Allowed From

Threads

### Example

```C
/* Specify the primary interface for TFTP requests. */
status =  nxd_tftp_client_set_interface(&client, 0);

/* If status is NX_SUCCESS the primary interface will be use for TFTP communications. */
```

## nxd_tftp_server_create

Create TFTP server

### Prototype

```C
UINT nxd_tftp_server_create(
    NX_TFTP_SERVER *tftp_server_ptr,
    CHAR *tftp_server_name,
    NX_IP *ip_ptr,
    FX_MEDIA *media_ptr, 
    VOID *stack_ptr,
    ULONG stack_size,
    NX_PACKET_POOL *pool_ptr);
```

### Description

This service creates a TFTP Server that responds to TFTP Client requests on port 69. The Server must be started by a subsequent call to *nxd_tftp_server_start*.

> **Important:** The application must make certain the supplied IP instance, packet pool, and FileX media instance are already created. In addition, UDP must be enabled for the IP instance prior to calling this service.

### Input Parameters

- *tftp_server_ptr*: Pointer to TFTP Server control block.

- *tftp_server_name*: Name of this TFTP Server instance

- *ip_ptr*: Pointer to previously created IP instance.

- *media_ptr*: Pointer to FileX media instance.

- *stack_ptr*: Pointer to TFTP Server stack area.

- *stack_size*: Number of bytes in the TFTP Server stack.

- *pool_ptr*: Pointer to TFTP packet pool.

> **Note:** The supplied pool must have packet payloads at least 580 bytes in size.<sup>1</sup>

<sup>1</sup> The data portion of a packet is exactly 512 bytes, but the packet payload size must be at least 572 bytes. The remaining bytes are used for the UDP, IPv6, and Ethernet headers and potential trailing bytes required by the driver for alignment.

### Return Values

- **NX_SUCCESS** (0x00) Successful Server create

- **NX_TFTP_POOL_ERROR** (0xC6) Packet pool has packet size of less than 560 bytes

- NX_PTR_ERROR (0x16) Invalid pointer input.

### Allowed From

Initialization, Threads

### Example

```C
/* Create a TFTP Server called "my_server". */
status =  nxd_tftp_server_create(&my_server, "My TFTP Server", &server_ip, 
				&ram_disk, stack_ptr, 2048, pool_ptr);

/* If status is NX_SUCCESS the TFTP Server is created. */
```

## nxd_tftp_server_delete

Delete TFTP Server

### Prototype

```C
UINT nxd_tftp_server_delete(NX_TFTP_SERVER *tftp_server_ptr);
```

### Description

This service deletes a previously created TFTP Server.

### Input Parameters

- *tftp_server_ptr*: Pointer to TFTP Server control block.

### Return Values

- **NX_SUCCESS** (0x00) Successful Server delete

- NX_PTR_ERROR (0x16) Invalid pointer input.

- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```C
/* Delete the TFTP Server called "my_server". */
status =  nxd_tftp_server_delete(&my_server);

/* If status is NX_SUCCESS the TFTP Server is deleted. */
```

## nxd_tftp_server_start

Start TFTP server

### Prototype

```C
UINT nxd_tftp_server_start(NX_TFTP_SERVER *tftp_server_ptr);
```

### Description

This service starts the previously created TFTP Server.

### Input Parameters

- *tftp_server_ptr*: Pointer to TFTP Server control block.

### Return Values

- **NX_SUCCESS** (0x00) Successful Server start

- NX_PTR_ERROR (0x16) Invalid pointer input .
 
### Allowed From

Initialization, threads

### Example

```C
/* Start the TFTP Server called "my_server". */
status =  nxd_tftp_server_start(&my_server);

/* If status is NX_SUCCESS the TFTP Server is started. */
```

## nxd_tftp_server_stop

Stop TFTP Server

### Prototype

```C
UINT nxd_tftp_server_stop(NX_TFTP_SERVER *tftp_server_ptr);
```

### Description

This service stops the previously created TFTP Server.

### Input Parameters

- *tftp_server_ptr*: Pointer to TFTP Server control block.

### Return Values

- **NX_SUCCESS** (0x00) Successful Server stop

- NX_PTR_ERROR (0x16) Invalid pointer input.

- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```C
/* Stop the TFTP Server called "my_server". */
status =  nxd_tftp_server_stop(&my_server);

/* If status is NX_SUCCESS the TFTP Server is stopped. */
```

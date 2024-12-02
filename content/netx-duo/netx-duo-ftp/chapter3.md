---
title: Chapter 3 - Description of FTP services
description: This chapter contains a description of all NetX Duo FTP services (listed below) in alphabetic order.  
---


This chapter contains a description of all NetX Duo FTP services (listed below) in alphabetic order (except where IPv4 and IPv6 equivalents of the same service are paired together).

> **Note:** In the **Return Values** section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## nx_ftp_client_connect

Connect to an FTP Server over IPv4

### Prototype

```C
UINT nx_ftp_client_connect(
    NX_FTP_CLIENT *ftp_client_ptr,
    ULONG server_ip,
    CHAR *username,
    CHAR *password,
    ULONG wait_option);
```

### Description

This service connects the previously created NetX Duo FTP Client instance to the FTP Server at the supplied IP address.

### Input Parameters

- *ftp_client_ptr*: Pointer to FTP Client control block.
- *server_ip*: IP address of FTP Server.
- *username*: Client username for authentication.
- *password*: Client password for authentication.
- *wait_option*: Defines how long the service will wait for the FTP Client connection. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the FTP Server response.
  - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until a FTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP connection.
- **NX_TFTP_EXPECTED_22X_CODE** (0xDB) Did not get a 22X (ok) response
- **NX_FTP_EXPECTED_23X_CODE** (0xDC) Did not get a 23X (ok) response
- **NX_FTP_EXPECTED_33X_CODE** (0xDE) Did not get a 33X (ok) response
- **NX_FTP_NOT_DISCONNECTED** (0xD4) Client is already connected.
- NX_PTR_ERROR (0x07) Invalid pointer input.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.
- NX_IP_ADDRESS_ERROR (0x21) Invalid IP address.

### Allowed From

Threads

### Example

```C
/* Connect the FTP Client instance "my_client" to the FTP Server at

IP address 1.2.3.4. */

status = nx_ftp_client_connect(&my_client, IP_ADDRESS(1,2,3,4), NULL, NULL, 100);

/* If status is NX_SUCCESS an FTP Client instance was successfully  
    connected to the FTP Server. */
```

### See Also

nx_ftp_client_create, nx_ftp_client_delete, nx_ftp_client_directory_create, nx_ftp_client_disconnect, nxd_ftp_client_connect

## nxd_ftp_client_connect

Connect to an FTP Server with IPv6 support

### Prototype

```C
UINT nxd_ftp_client_connect(
    NX_FTP_CLIENT *ftp_client_ptr,
    NXD_ADDRESS *server_ipduo, 
    CHAR *username,
    CHAR *password,
    ULONG wait_option);
```

### Description

This service connects the previously created NetX Duo FTP Client instance to the FTP Server at the supplied IP address. Both IPv4 and IPv6 networks are supported.

### Input Parameters

- *ftp_client_ptr*: Pointer to FTP Client control block.
- *server_ipduo*: IP address of the FTP Server.
- *username*: Client username for authentication.
- *password*: Client password for authentication.
- *wait_option*: Defines how long the service will wait for the FTP Client connection. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the FTP Server response.
  - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until a FTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP connection.
- **NX_TFTP_EXPECTED_22X_CODE** (0xDB) Did not get a 22X (ok) response
- **NX_FTP_EXPECTED_23X_CODE** (0xDC) Did not get a 23X (ok) response
- **NX_FTP_EXPECTED_33X_CODE** (0xDE) Did not get a 33X (ok) response
- **NX_FTP_NOT_DISCONNECTED** (0xD4) Client is already connected
- NX_PTR_ERROR (0x07) Invalid pointer inout.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.
- NX_IP_ADDRESS_ERROR (0x21) Invalid IP address.

### Allowed From

Threads

### Example

```C
/* Connect an FTP Client instance to the FTP Server. */

/* Set up an IPv6 address for the server here.
    Note this could also be an IPv4 address as well*/

server_ip_addr.nxd_ip_address.v6[3] = 0x106;
server_ip_addr.nxd_ip_address.v6[2] = 0x0;
server_ip_addr.nxd_ip_address.v6[1] = 0x0000f101;
server_ip_addr.nxd_ip_address.v6[0] = 0x20010db8;
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V6;

status = nxd_ftp_client_connect(&my_client, server_ip_addr, NULL, NULL, 100);

/* If status is NX_SUCCESS an FTP Client instance was successfully  
    connected to the FTP Server. */
```

### See Also

nx_ftp_client_create, nx_ftp_client_delete, nx_ftp_client_directory_create, nx_ftp_client_disconnect, nx_ftp_client_connect

## nx_ftp_client_create

Create an FTP Client instance

### Prototype

```C
UINT nx_ftp_client_create(
    NX_FTP_CLIENT *ftp_client_ptr,
    CHAR *ftp_client_name,
    NX_IP *ip_ptr,
    ULONG window_size,
    NX_PACKET_POOL *pool_ptr);
```

### Description

This service creates an FTP Client instance.

### Input Parameters

- *ftp_client_ptr*: Pointer to FTP Client control block.
- *ftp_client_name*: Name of FTP Client.
- *ip_ptr*: Pointer to previously created IP instance.
- *window_size*: Advertised window size for TCP sockets of this FTP Client.
- *pool_ptr*: Pointer to the default packet pool for this FTP Client. Note that the minimum packet payload must be large enough to hold  complete path and the file or directory name.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP Client create.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Initialization and Threads

### Example

```C
/* Create the FTP Client instance "my_client." */
status = nx_ftp_client_create(&my_client, "My Client", &client_ip,
    2000, &client_pool);
/* If status is NX_SUCCESS the FTP Client instance was successfully created. */
```

## nx_ftp_client_delete

Delete an FTP Client instance

### Prototype

```C
UINT nx_ftp_client_delete(NX_FTP_CLIENT *ftp_client_ptr);
```

### Description

This service deletes an FTP Client instance.

### Input Parameters

- *ftp_client_ptr*: Pointer to FTP Client control block.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP Client delete.
- **NX_FTP_NOT_DISCONNECTED** (0xD4) FTP client not disconnected
- NX_PTR_ERROR (0x07) Invalid FTP pointer.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Delete the FTP Client instance "my_client." */

status = nx_ftp_client_delete(&my_client);

/* If status is NX_SUCCESS the FTP Client instance was successfully  
    deleted. */
```

## nx_ftp_client_directory_create

Create a directory on FTP Server

### Prototype

```C
UINT nx_ftp_client_directory_create(
    NX_FTP_CLIENT *ftp_client_ptr,
    CHAR *directory_name,
    ULONG wait_option);
```

### Description

This service creates the specified directory on the FTP Server that is connected to the specified FTP Client.

### Input Parameters

- *ftp_client_ptr*: Pointer to FTP Client control block.
- *directory_name*: Name of directory to create.
- *wait_option*: Defines how long the service will wait for the FTP directory create. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the FTP Server response.
  - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until a FTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP directory create.
- **NX_FTP_NOT_CONNECTED** (0xD3) FTP Client is not connected.
- **NX_FTP_EXPECTED_2XX_CODE** (0xDA) Did not get a 2XX (ok) response
- NX_PTR_ERROR (0x07) Invalid FTP pointer.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Create the directory "my_dir" on the FTP Server connected to
    the FTP Client instance "my_client." */

status = nx_ftp_client_directory_create(&my_client, "my_dir", 200);

/* If status is NX_SUCCESS the directory "my_dir" was successfully created. */
```

## nx_ftp_client_directory_default_set

Set default directory on FTP Server

### Prototype

```C
UINT nx_ftp_client_directory_default_set(
    NX_FTP_CLIENT *ftp_client_ptr,
    CHAR *directory_path,
    ULONG wait_option);
```

### Description

This service sets the default directory on the FTP Server that is connected to the specified FTP Client. This default directory applies only to this client's connection.

### Input Parameters

- *ftp_client_ptr*: Pointer to FTP Client control block.
- *directory_path*: Name of directory path to set.
- *wait_option*: Defines how long the service will wait for the FTP default directory set. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the FTP Server response.
  - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until a FTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP default set.
- **NX_FTP_NOT_CONNECTED** (0xD3) FTP Client is not connected.
- **NX_FTP_EXPECTED_2XX_CODE** (0xDA) Did not get a 2XX (ok) response
- NX_PTR_ERROR (0x07) Invalid FTP pointer.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Set the default directory to "my_dir" on the FTP Server connected to
    the FTP Client instance "my_client." */

status = nx_ftp_client_directory_default_set(&my_client, "my_dir", 200);

/* If status is NX_SUCCESS the directory "my_dir" is the default directory. */
```

## nx_ftp_client_directory_delete

Delete directory on FTP Server

### Prototype

```C
UINT nx_ftp_client_directory_delete(
    NX_FTP_CLIENT *ftp_client_ptr,
    CHAR *directory_name,
    ULONG wait_option);
```

### Description

This service deletes the specified directory on the FTP Server that is connected to the specified FTP Client.

### Input Parameters

- *ftp_client_ptr*: Pointer to FTP Client control block.
- *directory_name*: Name of directory to delete.
- *wait_option*: Defines how long the service will wait for the FTP directory delete. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the FTP Server response.
  - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until a FTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP directory delete.
- **NX_FTP_NOT_CONNECTED** (0xD3) FTP Client is not connected.
- **NX_FTP_EXPECTED_2XX_CODE** (0xDA) Did not get a 2XX (ok) response
- NX_PTR_ERROR (0x07) Invalid FTP pointer.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Delete directory "my_dir" on the FTP Server connected to
    the FTP Client instance "my_client." */

status = nx_ftp_client_directory_delete(&my_client, "my_dir", 200);

/* If status is NX_SUCCESS the directory "my_dir" is deleted. */
```

## nx_ftp_client_directory_listing_get

Get directory listing from FTP Server

### Prototype

```C
UINT nx_ftp_client_directory_listing_get(
    NX_FTP_CLIENT *ftp_client_ptr,
    CHAR *directory_name,
    NX_PACKET **packet_ptr,
    ULONG wait_option);
```

### Description

This service gets the contents of the specified directory on the FTP Server that is connected to the specified FTP Client. The supplied packet pointer will contain one or more directory entries. Each entry is separated by a \<cr/lf\> combination. The ***nx_ftp_client_directory_listing_continue*** should be called to complete the directory get operation.

### Input Parameters

- *ftp_client_ptr*: Pointer to FTP Client control block.
- *directory_name*: Name of directory to get contents of.
- *packet_ptr*: Pointer to destination packet pointer. If successful, the packet payload will contain one or more directory entries.
- *wait_option*: Defines how long the service will wait for the FTP directory listing. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the FTP Server response.
  - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until a FTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP directory listing.
- **NX_FTP_NOT_CONNECTED** (0xD3) FTP Client is not connected.
- **NX_NOT_ENABLED** (0x14) Service (IPv6) not enabled
- **NX_FTP_EXPECTED_1XX_CODE** (0xD9) Did not get a 1XX (ok) response
- **NX_FTP_EXPECTED_2XX_CODE** (0xDA) Did not get a 2XX (ok) response
- NX_PTR_ERROR (0x07) Invalid FTP pointer.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Get the contents of directory "my_dir" on the FTP Server connected to
    the FTP Client instance "my_client." */

status = nx_ftp_client_directory_listing_get(&my_client, "my_dir", &my_packet,
    200);

/* If status is NX_SUCCESS, one or more entries of the directory "my_dir"
    can be found in "my_packet". */
```

## nx_ftp_client_directory_listing_continue

Continue directory listing from FTP Server

### Prototype

```C
UINT nx_ftp_client_directory_listing_continue(
    NX_FTP_CLIENT *ftp_client_ptr,
    NX_PACKET **packet_ptr,
    ULONG wait_option);
```

### Description

This service continues getting the contents of the specified directory on the FTP Server that is connected to the specified FTP Client. It should have been immediately preceded by a call to ***nx_ftp_client_directory_listing_get***. If successful, the supplied packet pointer will contain one or more directory entries. This routine should be called until an NX_FTP_END_OF_LISTING status is received.

### Input Parameters

- *ftp_client_ptr*: Pointer to FTP Client control block.
- *packet_ptr*: Pointer to destination packet pointer. If successful, the packet payload will contain one or more directory entries, separated by a <cr/lf>.
- *wait_option*: Defines how long the service will wait for the FTP directory listing. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the FTP Server response.
  - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until a FTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP directory listing.
- **NX_FTP_END_OF_LISTING** (0xD8) No more entries in this directory.
- **NX_FTP_NOT_CONNECTED** (0xD3) FTP Client is not connected.
- **NX_FTP_EXPECTED_2XX_CODE** (0xDA) Did not get a 2XX (ok) response
- NX_PTR_ERROR (0x07) Invalid FTP pointer.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Continue getting the contents of directory "my_dir" on the FTP Server
    connected to the FTP Client instance "my_client." */

status = nx_ftp_client_directory_listing_continue(&my_client, &my_packet,
    200);

/* If status is NX_SUCCESS, one or more entries of the directory "my_dir"
    can be found in "my_packet". */
```

## nx_ftp_client_disconnect

Disconnect from FTP Server

### Prototype

```C
UINT nx_ftp_client_disconnect(
    NX_FTP_CLIENT *ftp_client_ptr,
    ULONG wait_option);
```

### Description

This service disconnects a previously established FTP Server connection with the specified FTP Client.

### Input Parameters

- *ftp_client_ptr*: Pointer to FTP Client control block.
- *wait_option*: Defines how long the service will wait for the FTP Client disconnect. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the FTP Server response.
  - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until a FTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP disconnect.
- **NX_FTP_NOT_CONNECTED** (0xD3) FTP Client is not connected.
- **NX_FTP_EXPECTED_2XX_CODE** (0xDA) Did not get a 2XX (ok) response
- NX_PTR_ERROR (0x07) Invalid FTP pointer.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Disconnect "my_client" from the FTP Server. */

status = nx_ftp_client_disconnect(&my_client, 200);

/* If status is NX_SUCCESS, "my_client" has been disconnected. */
```

## nx_ftp_client_file_close

Close Client file

### Prototype

```C
UINT nx_ftp_client_file_close(
    NX_FTP_CLIENT *ftp_client_ptr,
    ULONG wait_option);
```

### Description

This service closes a previously opened file on the FTP Server.

### Input Parameters

- *ftp_client_ptr*: Pointer to FTP Client control block.
- *wait_option*: Defines how long the service will wait for the FTP Client file close. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the FTP Server response.
  - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until a FTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP file close.
- **NX_FTP_NOT_CONNECTED** (0xD3) FTP Client is not connected.
- **NX_FTP_NOT_OPEN (0xD5)** File not open; cannot close it
- **NX_FTP_EXPECTED_2XX_CODE** (0xDA) Did not get a 2XX (ok) response
- NX_PTR_ERROR (0x07) Invalid FTP pointer.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Close previously opened file of client "my_client" on the FTP Server. */

status = nx_ftp_client_file_close(&my_client, 200);

/* If status is NX_SUCCESS, the file opened previously in the "my_client" FTP
    connection has been closed. */
```

## nx_ftp_client_file_delete

Delete file on FTP Server

### Prototype

```C
UINT nx_ftp_client_file_delete(
    NX_FTP_CLIENT *ftp_client_ptr,
    CHAR *file_name,
    ULONG wait_option);
```

### Description

This service deletes the specified file on the FTP Server.

### Input Parameters

- *ftp_client_ptr*: Pointer to FTP Client control block.
- *file_name*: Name of file to delete.
- *wait_option*: Defines how long the service will wait for the FTP Client file delete. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the FTP Server response.
  - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until a FTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP file delete.
- **NX_FTP_NOT_CONNECTED** (0xD3) FTP Client is not connected.
- **NX_FTP_EXPECTED_2XX_CODE** (0xDA) Did not get a 2XX (ok) response
- NX_PTR_ERROR (0x07) Invalid FTP pointer.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Delete the file "my_file.txt" on the FTP Server using the previously
    connected client "my_client." */

status = nx_ftp_client_file_delete(&my_client, "my_file.txt", 200);

/* If status is NX_SUCCESS, the file "my_file.txt" on the FTP Server is
    deleted. */
```

## nx_ftp_client_file_open

Opens file on FTP Server

### Prototype

```C
UINT nx_ftp_client_file_open(
    NX_FTP_CLIENT *ftp_client_ptr,
    CHAR *file_name,
    UINT open_type,
    ULONG wait_option);
```

### Description

This service opens the specified file – for reading or writing – on the FTP Server previously connected to the specified Client instance.

### Input Parameters

- *ftp_client_ptr*: Pointer to FTP Client control block.
- *file_name*: Name of file to open.
- *open_type*: Either **NX_FTP_OPEN_FOR_READ** or **NX_FTP_OPEN_FOR_WRITE**.
- *wait_option*: Defines how long the service will wait for the FTP Client file open. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the FTP Server response.
  - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until a FTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP file open.
- **NX_OPTION_ERROR** (0x0A) Invalid open type.
- **NX_FTP_NOT_CONNECTED** (0xD3) FTP Client is not connected.
- **NX_FTP_NOT_CLOSED** (0xD6) FTP Client is already opened.
- **NX_NO_FREE_PORTS** (0x45) No TCP ports available to assign
- NX_PTR_ERROR (0x07) Invalid FTP pointer.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Open the file "my_file.txt" for reading on the FTP Server using the previously
    connected client "my_client." */

status = nx_ftp_client_file_open(&my_client, "my_file.txt", NX_FTP_OPEN_FOR_READ,
    200);

/* If status is NX_SUCCESS, the file "my_file.txt" on the FTP Server is
    open for reading. */
```

## nx_ftp_client_file_read

Read from file

### Prototype

```C
UINT nx_ftp_client_file_read(
    NX_FTP_CLIENT *ftp_client_ptr,
    NX_PACKET **packet_ptr,
    ULONG wait_option);
```

### Description

This service reads a packet from a previously opened file. It should be called repetitively until a status of NX_FTP_END_OF_FILE is received.

Note that the caller does not allocate a packet for this service.  It need only supply a pointer to a packet pointer. This service will update that packet pointer to point to a packet retrieved from the socket receive queue.  If a successful status is returned, that means there was a packet available, and it is the caller's responsibility to release the packet when it is done with it.

If a non-zero status (either an error status or NX_FTP_END_OF_FILE) is returned, the caller does not release the packet. Otherwise, an error is generated when if the packet pointer is NULL.

### Input Parameters

- *ftp_client_ptr*: Pointer to FTP Client control block.
- *packet_ptr*: Pointer to destination for the data packet pointer to be stored. If successful, the packet some or all the contains of the file.
- *wait_option*: Defines how long the service will wait for the FTP Client file read. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the FTP Server response.
  - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until a FTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP file read.
- **NX_FTP_NOT_OPEN** (0xD5) FTP Client is not opened.
- **NX_FTP_END_OF_FILE** (0xD7) End of file condition.
- NX_PTR_ERROR (0x07) Invalid FTP pointer.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
NX_PACKET   *my_packet;

/* Read a packet of data from the file "my_file.txt" that was previously opened
    from the client "my_client." */

status = nx_ftp_client_file_read(&my_client, &my_packet, 200);

/* Check status.  */
if (status != NX_SUCCESS)
{
    error_counter++;
}
else
{
    /* Release packet when done with it. */
    nx_packet_release(my_packet);
}

/* If status is NX_SUCCESS, the packet pointer, "my_packet" points to the packet
    that contains the next bytes from the file. If the file is completely
    downloaded, an NX_FTP_END_OF_FILE status is returned (no packet retrieved). */
```

## nx_ftp_client_file_rename

Rename file on FTP Server

### Prototype

```C
UINT nx_ftp_client_file_rename(
    NX_FTP_CLIENT *ftp_ptr,
    CHAR *filename,
    CHAR *new_filename,
    ULONG wait_option);
```

### Description

This service renames a file on the FTP Server.

### Input Parameters

- *ftp_client_ptr*: Pointer to FTP Client control block.
- *filename*: Current name of file.
- *new_filename*: New name for file.
- *wait_option*: Defines how long the service will wait for the FTP Client file rename. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the FTP Server response.
  - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until a FTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP file rename.
- **NX_FTP_NOT_CONNECTED** (0xD3) FTP Client is not connected.
- **NX_FTP_EXPECTED_3XX_CODE** (0XDD) Did not receive 3XX (ok) response
- **NX_FTP_EXPECTED_2XX_CODE** (0xDA) Did not get a 2XX (ok) response
- NX_PTR_ERROR (0x07) Invalid FTP pointer.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Rename file "my_file.txt" to "new_file.txt" on the previously connected
    Client instance "my_client." */

status = nx_ftp_client_file_rename(&my_client, "my_file.txt", "new_file.txt",
    200);

/* If status is NX_SUCCESS, the file has been renamed. */
```

## nx_ftp_client_file_write

Write to file

### Prototype

```C
UINT nx_ftp_client_file_write(
    NX_FTP_CLIENT *ftp_client_ptr,
    NX_PACKET *packet_ptr,
    ULONG wait_option);
```

### Description

This service writes a packet of data to the previously opened file on the FTP Server.

### Input Parameters

- *ftp_client_ptr*: Pointer to FTP Client control block.
- *packet_ptr*: Packet pointer containing data to write.
- *wait_option*: Defines how long the service will wait for the FTP Client file write. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the FTP Server response.
  - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until a FTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP file write.
- **NX_FTP_NOT_OPEN** (0xD5) FTP Client is not opened.
- NX_PTR_ERROR (0x07) Invalid FTP pointer.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Write the data contained in "my_packet" to the previously opened file
    "my_file.txt". */

status = nx_ftp_client_file_write(&my_client, my_packet, 200);

/* If status is NX_SUCCESS, the file has been written to. */
```

## nx_ftp_client_passive_mode_set

Enable or disable passive transfer mode

### Prototype

```C
UINT nx_ftp_client_passive_mode_set(
    NX_FTP_CLIENT *ftp_client_ptr,
    UINT passive_mode_enabled);
```

### Description

This service enables passive transfer mode if the passive_mode_enabled input is set to NX_TRUE on a previously created FTP Client instance such that subsequent calls to read or write files (RETR, STOR) or download a directory listing (NLST) are done in transfer mode. To disable passive mode transfer and return to the default behavior of active transfer mode, call this function with the passive_mode_enabled input set to **NX_FALSE**.

### Input Parameters

- *ftp_client_ptr*: Pointer to FTP Client control block.
- *passive_mode_enabled*: If set to **NX_TRUE**, passive mode is enabled. If set to **NX_FALSE**, passive mode is disabled.

### Return Values

- **NX_SUCCESS** (0x00) Successful passive mode set.
- NX_PTR_ERROR (0x07) Invalid FTP pointer.
- NX_INVALID_PARAMETERS (0x4D) Invalid non pointer input

### Allowed From

Threads

### Example

```C
/* Enable the FTP Client to exchange data with the FTP server in passive mode. */

status = nx_ftp_client_passive_mode_set(&my_client, NX_TRUE);

/* If status is NX_SUCCESS, the FTP client is in passive transfer mode. */
```

## nx_ftp_server_create

Create FTP Server

### Prototype

```C
UINT nx_ftp_server_create(
    NX_FTP_SERVER *ftp_server_ptr,
    CHAR *ftp_server_name,
    NX_IP *ip_ptr,
    FX_MEDIA *media_ptr,
    VOID *stack_ptr,
    ULONG stack_size,
    NX_PACKET_POOL *pool_ptr,
    UINT (*ftp_login)(
        struct NX_FTP_SERVER_STRUCT *ftp_server_ptr,
        ULONG client_ip_address,
        UINT client_port,
        CHAR *name,
        CHAR *password,
        CHAR *extra_info),
    UINT (*ftp_logout)(
        struct NX_FTP_SERVER_STRUCT *ftp_server_ptr,
        ULONG client_ip_address,
        UINT client_port,
        CHAR *name,
        CHAR *password,
        CHAR *extra_info));
```

### Description

This service creates an FTP Server instance on the specified and previously created NetX Duo IP instance. Note the FTP Server needs to be started with a call to ***nx_ftp_server_start*** for it to begin operation.

### Input Parameters

- *ftp_server_ptr*: Pointer to FTP Server control block.
- *ftp_server_name*: Name of FTP Server.
- *ip_ptr*: Pointer to associated NetX Duo IP instance. Note there can only be one FTP Server for an IP instance.
- *media_ptr*: Pointer to associated FileX media instance.
- *stack_ptr*: Pointer to memory for the internal FTP Server thread's stack area.
- *stack_size*: Size of stack area specified by *stack_ptr*.
- *pool_ptr*: Pointer to default NetX Duo packet pool. Note the payload size of packets in the pool must be large enough to accommodate the largest filename/path.
- *ftp_login*: Function pointer to application's login function. This function is supplied the username and password from the Client requesting a connection, and the Client address in the ULONG data type. If this is valid, the application's login function should return **NX_SUCCESS**.
- *ftp_logout*: Function pointer to application's logout function. This function is supplied the username and password from the Client requesting a disconnection, and the Client address in the ULONG data type. If this is valid, the application's login function should return **NX_SUCCESS**.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP Server create.
- NX_PTR_ERROR (0x07) Invalid FTP pointer.

### Allowed From

Initialization and Threads

### Example

```C
/* Create the FTP Server "my_server" on the IP instance "ip_0" using the
    "ram_disk" media. */

status = nx_ftp_server_create(&my_server, "My Server Name", &ip_0, &ram_disk,
    stack_ptr, stack_size, &pool_0,
    my_login, my_logout);

/* If status is NX_SUCCESS, the FTP Server has been created. */
```

## nxd_ftp_server_create

Create FTP Server with IPv4 and IPv6 support

### Prototype

```C
UINT nxd_ftp_server_create(
    NX_FTP_SERVER *ftp_server_ptr,
    CHAR *ftp_server_name,
    NX_IP *ip_ptr,
    FX_MEDIA *media_ptr,
    VOID *stack_ptr,
    ULONG stack_size,
    NX_PACKET_POOL *pool_ptr,
    UINT (*ftp_login_duo)(
        struct NX_FTP_SERVER_STRUCT *ftp_server_ptr,
        NXD_ADDRESS *client_ip_address,
        UINT client_port,
        CHAR *name,
        CHAR *password,
        CHAR *extra_info),
    UINT (*ftp_logout_duo)(
        struct NX_FTP_SERVER_STRUCT *ftp_server_ptr,
        NXD_ADDRESS *client_ip_address,
        UINT client_port,
        CHAR *name,
        CHAR *password,
        CHAR *extra_info));
```

### Description

This service creates an FTP Server instance on the specified and previously created NetX Duo IP instance. Note the FTP Server still needs to be started with a call to ***nx_ftp_server_start*** for it to begin operation after the Server is created.

### Input Parameters

- *ftp_server_ptr*: Pointer to FTP Server control block.
- *ftp_server_name*: Name of FTP Server.
- *ip_ptr*: Pointer to associated NetX Duo IP instance. Note there can only be one FTP Server for an IP instance.
- *media_ptr*: Pointer to associated FileX media instance.
- *stack_ptr*: Pointer to memory for the internal FTP Server thread's stack area.
- *stack_size*: Size of stack area specified by *stack_ptr*.
- *pool_ptr*: Pointer to default NetX Duo packet pool. Note the payload size of packets in the pool must be large enough to accommodate the largest filename/path.
- *ftp_login_duo*: Function pointer to application's login function. This function is supplied the username and password from the Client requesting a connection, and a pointer to the Client address in the NXD_ADDRESS data type. If this is valid, the application's login function should return NX_SUCCESS.
- *ftp_logout_duo*: Function pointer to application's logout function. This function is supplied the username and password from the Client requesting a disconnection, and a pointer to the Client address in the NXD_ADDRESS data type. If this is valid, the application's login function should return NX_SUCCESS.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP Server create.
- NX_PTR_ERROR (0x07) Invalid FTP pointer.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Initialization and Threads

### Example

```C
/* Create the FTP Server "my_server" on the IP instance "ip_0" using the
    "ram_disk" media. */

status = nxd_ftp_server_create(&my_server, "My Server Name", &ip_0, &ram_disk,
    stack_ptr, stack_size, &pool_0,
    my_duo_login, my_duo_logout);

/* If status is NX_SUCCESS, the FTP Server has been created. */
```

## nx_ftp_server_delete

Delete FTP Server

### Prototype

```C
UINT nx_ftp_server_delete(NX_FTP_SERVER *ftp_server_ptr);
```

### Description

This service deletes a previously created FTP Server instance.

### Input Parameters

- *ftp_server_ptr*: Pointer to FTP Server control block.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP Server delete.
- NX_PTR_ERROR (0x07) Invalid FTP pointer.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Delete the FTP Server "my_server". */

status = nx_ftp_server_delete(&my_server);

/* If status is NX_SUCCESS, the FTP Server has been deleted. */
```

## nx_ftp_server_start

Start FTP Server

### Prototype

```C
UINT nx_ftp_server_start(NX_FTP_SERVER *ftp_server_ptr);
```

### Description

This service starts a previously created FTP Server instance.

### Input Parameters

- *ftp_server_ptr*: Pointer to FTP Server control block.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP Server start.
- NX_PTR_ERROR (0x07) Invalid FTP pointer.

### Allowed From

Threads

### Example

```C
/* Start the FTP Server "my_server". */

status = nx_ftp_server_start(&my_server);

/* If status is NX_SUCCESS, the FTP Server has been started. */
```

## nx_ftp_server_stop

Stop FTP Server

### Prototype

```C
UINT nx_ftp_server_stop(NX_FTP_SERVER *ftp_server_ptr);
```

### Description

This service stops a previously created and started FTP Server instance.

### Input Parameters

- *ftp_server_ptr*: Pointer to FTP Server control block.

### Return Values

- **NX_SUCCESS** (0x00) Successful FTP Server stop.
- NX_PTR_ERROR (0x07) Invalid FTP pointer.
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Stop the FTP Server "my_server". */

status = nx_ftp_server_stop(&my_server);

/* If status is NX_SUCCESS, the FTP Server has been stopped. */
```

---
title: Chapter 3 - Description of NetX Duo HTTP Services
description: This chapter contains a description of all NetX Duo HTTP services (listed below) in alphabetical order except for the 'NetX' (IPv4 only) equivalent of the same service are paired together).
date: 2024-07-21
---


This chapter contains a description of all NetX Duo HTTP services (listed below) in alphabetical order except for the 'NetX' (IPv4 only) equivalent of the same service are paired together).

In the **Return Values** section in the following API descriptions, values in BOLD are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## nx_http_client_create

Create a PPPoE Client instance

### Prototype

```c
UINT nx_http_client_create(
    NX_HTTP_CLIENT *client_ptr,
    CHAR *client_name,
    NX_IP *ip_ptr,
    NX_PACKET_POOL *pool_ptr,
    ULONG window_size);
```

### Description

This service creates an HTTP Client instance on the specified IP instance.

### Input Parameters

 - *client_ptr*: Pointer to HTTP Client control block.
 - *client_name*: Name of HTTP Client instance.
 - *ip_ptr*: Pointer to IP instance.
 - *pool_ptr*: Pointer to default packet pool. Note that the packets in this pool must have a payload large enough to handle the complete response header. This is defined by **NX_HTTP_CLIENT_MIN_PACKET_SIZE** in ***nx_http.h***.
 - *window_size*: Size of the Client's TCP socket receive window.

### Return Values

 - **NX_SUCCESS** (0x00) Successful HTTP Client create
 - NX_PTR_ERROR (0x07) Invalid HTTP, ip_ptr, or packet pool pointer
 - NX_HTTP_POOL_ERROR (0xE9) Invalid payload size in packet pool

### Allowed From

Initialization, Threads

### Example

```c
/* Create the HTTP Client instance "my_client" on "ip_0". */
status =  nx_http_client_create(&my_client, "my client", &ip_0, &pool_0, 100);

/* If status is NX_SUCCESS an HTTP Client instance was successfully created. */
```

## nx_http_client_delete

Delete an HTTP Client Instance

### Prototype

```c
UINT nx_http_client_delete(NX_HTTP_CLIENT *client_ptr);
```

### Description

This service deletes a previously created HTTP Client instance.

### Input Parameters

 - *client_ptr*: Pointer to HTTP Client control block.

### Return Values

 - **NX_SUCCESS** (0x00) Successful HTTP Client delete
 - NX_PTR_ERROR (0x07) Invalid HTTP pointer
 - NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```c
/* Delete the HTTP Client instance "my_client."  */
status =  nx_http_client_delete(&my_client);

/* If status is NX_SUCCESS an HTTP Client instance was successfully deleted. */
```

## nx_http_client_get_start

Start an HTTP GET request over IPv4

### Prototype

```c
UINT nx_http_client_get_start(
    NX_HTTP_CLIENT *client_ptr,
    ULONG ip_address,
    CHAR *resource,
    CHAR *input_ptr,
    UINT input_size,
    CHAR *username,
    CHAR *password,
    ULONG wait_option);
```

### Description

This service attempts to create and send a GET request with the resource specified by "resource" pointer on the previously created HTTP Client instance. If this routine returns NX_SUCCESS, the application can then make multiple calls to *nx_http_client_get_packet* to retrieve packets of data corresponding to the requested resource content.

> **Note:** The resource string can refer to a local file e.g. ``` "/index.htm" ``` or it can refer to another URL e.g. ```http://abc.website.com/index.htm``` if the HTTP Server indicates it supports referring GET requests.

This service works only over IPv4 network. For applications that need to connect to IPv6 network, *nxd_http_client_get_start_extended* should be used.

This service is deprecated. Developers are encouraged to migrate to *nx_http_client_get_start_extended* or *nxd_http_client_get_start_extended*.

### Input Parameters

 - *client_ptr*: Pointer to HTTP Client control block.
 - *ip_address*: IP address of the HTTP Server.
 - *resource*: Pointer to URL string for requested resource.
 - *input_ptr*: Pointer to additional data for the GET request. This is optional. If valid, the specified input is placed in the content area of the message and a POST is used instead of a GET operation.
 - *input_size*: Number of bytes in optional additional input pointed to by ```input_ptr```.
 - *username*: Pointer to optional user name for authentication.
 - *password*: Pointer to optional password for authentication.
 - *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  	- **time out value** (0x00000001 through 0xFFFFFFFE)
  	- **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting **TX_WAIT_FOREVER** causes the calling thread to suspend indefinitely until the HTTP Server responds to the request. Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.

### Return Values

 - **NX_SUCCESS** (0x00) Successfully sent HTTP Client. GET start message.
 - **NX_HTTP_ERROR** (0xE0) Internal HTTP Client error
 - **NX_HTTP_NOT_READY** (0xEA) HTTP Client not ready
 - **NX_HTTP_FAILED** (0xE2) HTTP Client error communicating with the HTTP Server.
 - **NX_HTTP_AUTHENTICATION_ERROR** (0xEB) Invalid name and/or password.
 - NX_PTR_ERROR (0x07) Invalid pointer input
 - NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c
/* Start the GET operation on the HTTP Client "my_client."  */
status =  nx_http_client_get_start(&my_client, IP_ADDRESS(1,2,3,5), "/TEST.HTM",
                           NX_NULL, 0, "myname", "mypassword", 1000);

/* If status is NX_SUCCESS, the GET request for TEST.HTM is started and is so
   far successful. The client must now call nx_http_client_get_packet multiple
   times to retrieve the content associated with TEST.HTM. */


#define POST_MESSAGE   "Add this data to the message content"

/* Start the POST operation on the HTTP Client "my_client."  */
status =  nx_http_client_get_start(&my_client, IP_ADDRESS(1,2,3,5), "/TEST.HTM",
                            POST_MESSAGE, sizeof(POST_MESSAGE),
                            "myname", "mypassword", 1000);


/* If status is NX_SUCCESS, the POST_MESSAGE is added to the message in the POST request
   for TEST.HTM and successfully sent. */
```

## nx_http_client_get_start_extended

Start an HTTP GET request over IPv4

### Prototype

```c
UINT nx_http_client_get_start_extended(
    NX_HTTP_CLIENT *client_ptr,
    ULONG ip_address,
    CHAR *resource,
    UINT resource_length,
    CHAR *input_ptr,
    UINT input_size,
    CHAR *username,
    UINT username_length,
    CHAR *password,
    UINT password_length,
    ULONG wait_option);
```

### Description

This service attempts to create and send a GET request with the resource specified by "resource" pointer on the previously created HTTP Client instance. If this routine returns NX_SUCCESS, the application can then make multiple calls to *nx_http_client_get_packet* to retrieve packets of data corresponding to the requested resource content.

> **Note:** The resource string can refer to a local file e.g. ``` "/index.htm" ``` or it can refer to another URL e.g. ```http://abc.website.com/index.htm``` if the HTTP Server indicates it supports referring GET requests.

This service works only over IPv4 network. For applications that need to connect to IPv6 network, *nxd_http_client_get_start_extended* should be used.

This service replaces *nx_http_client_get_start*. It requires caller to specify the length of the resource, username and password.

### Input Parameters

 - *client_ptr*: Pointer to HTTP Client control block.
 - *ip_address*: IP address of the HTTP Server.
 - *resource Pointer*: to URL string for requested resource.
 - *resource_length*: Length of URL string for requested resource.
 - *input_ptr*: Pointer to additional data for the GET request. This is optional. If valid, the specified input is placed in the content area of the message and a POST is used instead of a GET operation.
 - *input_size*: Number of bytes in optional additional input pointed to by ```input_ptr```.
 - *username*: Pointer to optional user name for authentication.
 - *username_length*: Length of optional user name for authentication.
 - *password*: Pointer to optional password for authentication.
 - *password_length*: Length of optional password for authentication.
 - *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
	- **time out value** (0x00000001 through 0xFFFFFFFE)
	- **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting **TX_WAIT_FOREVER** causes the calling thread to suspend indefinitely until the HTTP Server responds to the request. Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.

### Return Values

 - **NX_SUCCESS** (0x00) Successfully sent HTTP Client. GET start message
 - **NX_HTTP_ERROR** (0xE0) Internal HTTP Client error
 - **NX_HTTP_NOT_READY** (0xEA) HTTP Client not ready
 - **NX_HTTP_FAILED** (0xE2) HTTP Client error communicating with the HTTP Server.
 - **NX_HTTP_AUTHENTICATION_ERROR** (0xEB) Invalid name and/or password.
 - NX_PTR_ERROR (0x07) Invalid pointer input
 - NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```c
/* If status is NX_SUCCESS, the GET request for TEST.HTM is started and is so
   far successful. The client must now call nx_http_client_get_packet multiple
   times to retrieve the content associated with TEST.HTM. */


#define POST_MESSAGE   "Add this data to the message content"

/* Start the POST operation on the HTTP Client "my_client."  */
status =  nx_http_client_get_start_extended(&my_client, IP_ADDRESS(1,2,3,5), "/TEST.HTM",
                                     9, POST_MESSAGE, sizeof(POST_MESSAGE),
                                     "myname", 6, "mypassword", 10, 1000);


/* If status is NX_SUCCESS, the POST_MESSAGE is added to the message in the POST request
   for TEST.HTM and successfully sent. */
```

## nxd_http_client_get_start

Send an HTTP GET request (IPv4 or IPv6)

### Prototype

```c
UINT nxd_http_client_get_start(
    NX_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS *server_ip,
    CHAR *resource,
    CHAR *input_ptr,
    UINT input_size,
    CHAR *username,
    CHAR *password,
    ULONG wait_option);
```

### Description

This service attempts to create and send a GET request with the resource specified by *resource* pointer on the previously created HTTP Client instance. It can be used to connect to IPv4 or IPv6 network. If this routine returns **NX_SUCCESS**, the application can then make multiple calls to ***nx_http_client_get_packet*** to retrieve packets of data corresponding to the requested resource content.

> **Note:** The resource string can refer to a local file e.g. ``` "/index.htm" ``` or it can refer to another URL e.g. ```http://abc.website.com/index.htm``` if the HTTP Server indicates it supports referring GET requests.

This service is deprecated. Developers are encouraged to migrate to *nxd_http_client_get_start_extended*.

### Input Parameters

 - *client_ptr*: Pointer to HTTP Client control block.
 - *Server_ip*: IP address of the HTTP Server.
 - *resource*: Pointer to URL string for requested resource.
 - *input_ptr*: Pointer to additional data for the GET request. This is optional. If valid, the specified input is placed in the content area of the message and a POST is used instead of a GET operation.
 - *input_size*: Number of bytes in optional additional input pointed to by ```input_ptr```.
 - *username*: Pointer to optional user name for authentication.
 - *username_length*: Length of optional user name for authentication.
 - *password*: Pointer to optional password for authentication.
 - *password_length*: Length of optional password for authentication.
 - *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
    - **time out value** (0x00000001 through 0xFFFFFFFE)
    - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting **TX_WAIT_FOREVER** causes the calling thread to suspend indefinitely until the HTTP Server responds to the request. Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.

### Return Values

 - **NX_SUCCESS** (0x00) Successfully sent GET request
 - **NX_HTTP_PASSWORD_TOO_LONG** (0xF0) Password exceeds buffer size
 - **NX_HTTP_NOT_READY** (0xEA) HTTP Client not ready
 - **NX_HTTP_FAILED** (0xE2) Invalid packet parameters.
 - **NX_HTTP_AUTHENTICATION_ERROR** (0xEB) Invalid name or password
 - NX_PTR_ERROR (0x07) Invalid pointer input
 - NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```c
NXD_ADDRESS server_ip_address;

/* for an IPv4 address, define as follows: */
server_ip_address.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_address.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,4);

/* for an IPv6 address, define as follows: */
server_ip_address.nxd_ip_version = NX_IP_VERSION_V6;
server_ip_address.nxd_ip_address.v6[0] = 0x20010db8;
server_ip_address.nxd_ip_address.v6[1] = 0x0;
server_ip_address.nxd_ip_address.v6[2] = 0xf101;
server_ip_address.nxd_ip_address.v6[3] = 0x106;


/* Start the GET operation on the HTTP Client "my_client."  */
status =  nxd_http_client_get_start(&my_client, server_ip_address, "/TEST.HTM",
NX_NULL, 0, "myname", "mypassword", 1000);


/* If status is NX_SUCCESS, the GET request for TEST.HTM is started and is so
   far successful. The client must now call nx_http_client_get_packet multiple
   times to retrieve the content associated with TEST.HTM. */
```

## nxd_http_client_get_start_extended

Send an HTTP GET request (IPv4 or IPv6)

### Prototype

```c
UINT nxd_http_client_get_start_extended(
    NX_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS *server_ip,
    CHAR *resource,
    UINT resource_length,
    CHAR *input_ptr,
    UINT input_size,
    CHAR *username,
    UINT username_length,
    CHAR *password,
    UINT password_length,
    ULONG wait_option);
```

### Description

This service attempts to create and send a GET request with the resource specified by "resource" pointer on the previously created HTTP Client instance. It can be used to connect to IPv4 or IPv6 network. If this routine returns NX_SUCCESS, the application can then make multiple calls to *nx_http_client_get_packet* to retrieve packets of data corresponding to the requested resource content.

> **Note:** The resource string can refer to a local file e.g. ``` "/index.htm" ``` or it can refer to another URL e.g. ```http://abc.website.com/index.htm``` if the HTTP Server indicates it supports referring GET requests.

This service replaces *nxd_http_client_get_start*. It requires caller to specify the length of the resource, username and password.

### Input Parameters

 - *client_ptr*: Pointer to HTTP Client control block.
 - *Server_ip*: IP address of the HTTP Server.
 - *resource*: Pointer to URL string for requested resource.
 - *resource_length*: Length of URL string for requested resource.
 - *input_ptr*: Pointer to additional data for the GET request. This is optional. If valid, the specified input is placed in the content area of the message and a POST is used instead of a GET operation.
 - *input_size*: Number of bytes in optional additional input pointed to by ```input_ptr```.
 - *username*: Pointer to optional user name for authentication.
 - *username_length*: Length of optional user name for authentication.
 - *password*: Pointer to optional password for authentication.
 - *password_length*: Length of optional password for authentication.
 - *wait_option*: Defines how long the service will wait internally to process the HTTP Client get start. The wait options are defined as follows:
    - **time out value** (0x00000001 through 0xFFFFFFFE)
    - **TX_WAIT_FOREVER** (0xFFFFFFFF)

    Selecting TX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

    Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.

### Return Values

 - **NX_SUCCESS** (0x00) Successfully sent GET request
 - **NX_HTTP_PASSWORD_TOO_LONG** (0xF0) Password exceeds buffer size
 - **NX_HTTP_NOT_READY** (0xEA) HTTP Client not ready
 - **NX_HTTP_FAILED** (0xE2) Invalid packet parameters.
 - **NX_HTTP_AUTHENTICATION_ERROR** (0xEB) Invalid name or password
 - NX_PTR_ERROR (0x07) Invalid pointer input
 - NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```c
NXD_ADDRESS server_ip_address;

/* for an IPv4 address, define as follows: */
server_ip_address.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_address.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,4);

/* for an IPv6 address, define as follows: */
server_ip_address.nxd_ip_version = NX_IP_VERSION_V6;
server_ip_address.nxd_ip_address.v6[0] = 0x20010db8;
server_ip_address.nxd_ip_address.v6[1] = 0x0;
server_ip_address.nxd_ip_address.v6[2] = 0xf101;
server_ip_address.nxd_ip_address.v6[3] = 0x106;


/* Start the GET operation on the HTTP Client "my_client."  */
status =  nxd_http_client_get_start_extended(&my_client, server_ip_address,
                                             "/TEST.HTM", 9, NX_NULL, 0, "myname",
        6, "mypassword", 10, 1000);


/* If status is NX_SUCCESS, the GET request for TEST.HTM is started and is so
   far successful. The client must now call nx_http_client_get_packet multiple
   times to retrieve the content associated with TEST.HTM. */
```

## nx_http_client_get_packet

Get next resource data packet

### Prototype

```c
UINT nx_http_client_get_packet(
    NX_HTTP_CLIENT *client_ptr,
    NX_PACKET **packet_ptr,
    ULONG wait_option);
```

### Description

This service retrieves the next packet of content of the resource requested by the previous *nx_http_client_get_start* call. Successive calls to this routine should be made until the return status of NX_HTTP_GET_DONE is received.

### Input Parameters

 - *client_ptr*: Pointer to HTTP Client control block.
 - *packet_ptr*: Destination for packet pointer containing partial resource content.
 - *wait_option*: Defines how long the service will wait for the HTTP Client get packet. The wait options are defined as follows:
    - **time out value** (0x00000001 through 0xFFFFFFFE)
    - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting **TX_WAIT_FOREVER** causes the calling thread to suspend indefinitely until the HTTP Server responds to the request. Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.

### Return Values

 - **NX_SUCCESS** (0x00) Successful HTTP Client get packet.
 - **NX_HTTP_GET_DONE** (0xEC) HTTP Client get packet is done
 - **NX_HTTP_NOT_READY** (0xEA) HTTP Client not in get mode.
 - **NX_HTTP_BAD_PACKET_LENGTH** (0xED) Invalid packet length
 - NX_PTR_ERROR (0x07) Invalid pointer input
 - NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```c
/* Get the next packet of resource content on the HTTP Client "my_client."
   Note that the nx_http_client_get_start routine must have been called
   previously. */
status =  nx_http_client_get_packet(&my_client, &next_packet, 1000);


/* If status is NX_SUCCESS, the next packet of content is pointed to
   by "next_packet". */
```

## nx_http_client_put_start

Start an HTTP PUT request over IPv4

### Prototype

```c
UINT nx_http_client_put_start(
    NX_HTTP_CLIENT *client_ptr,
    ULONG ip_address,
    CHAR *resource,
    CHAR *username,
    CHAR *password,
    ULONG total_bytes,
    ULONG wait_option);
```

### Description

This service attempts to send a PUT request with the specified resource to the HTTP Server at the supplied IP address. If this routine is successful, the application code should make successive calls to the ***nx_http_client_put_packet*** routine to actually send the resource contents to the HTTP Server.

> **Note:** The resource string can refer to a local file e.g. ``` "/index.htm" ``` or it can refer to another URL e.g. ```http://abc.website.com/index.htm``` if the HTTP Server indicates it supports referring PUT requests.

This service is deprecated. Developers are encouraged to migrate to ***nxd_http_client_put_start_extended***.

### Input Parameters

 - *client_ptr*: Pointer to HTTP Client control block.
 - *ip_address*: IP address of the HTTP Server.
 - *resource*: Pointer to URL string for requested resource.
 - *username*: Pointer to optional user name for authentication.
 - *password*: Pointer to optional password for authentication.
 - *total_bytes*: Total bytes of resource being sent. Note that the combined length of all packets sent via subsequent calls to *nx_http_client_put_packet* must equal this value.
 - *wait_option*: Defines how long the service will wait for the HTTP Client PUT start. The wait options are defined as follows:
    - **time out value** (0x00000001 through 0xFFFFFFFE)
    - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting **TX_WAIT_FOREVER** causes the calling thread to suspend indefinitely until the HTTP Server responds to the request. Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response

### Return Values

 - **NX_SUCCESS** (0x00) Successfully sent PUT request
 - **NX_HTTP_USERNAME_TOO_LONG** (0xF1) Username too large for buffer
 - **NX_HTTP_NOT_READY** (0xEA) HTTP Client not ready
 - NX_PTR_ERROR (0x07) Invalid pointer input
 - NX_SIZE_ERROR (0x09) Invalid total size of resource
 - NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```c
/* Start an HTTP PUT to place the 20-byte resource "/TEST.HTM" on the HTTP Server
   at IP address 1.2.3.5. */
status =  nx_http_client_put_start(&my_client, IP_ADDRESS(1, 2, 3, 5),
"/TEST.HTM", "myname", "mypassword", 20, NX_WAIT_FOREVER);

/* If status is NX_SUCCESS, the PUT operation for TEST.HTM has successfully been
   started. */
```

## nx_http_client_put_start_extended

Start an HTTP PUT request over IPv4

### Prototype

```c
UINT nx_http_client_put_start_extended(
    NX_HTTP_CLIENT *client_ptr,
    ULONG ip_address,
    CHAR *resource,
    UINT resource_length,
    CHAR *username,
    UINT username_length,
    CHAR *password,
    UINT password_length,
    ULONG total_bytes,
    ULONG wait_option);
```

### Description

This service attempts to send a PUT request with the specified resource to the HTTP Server at the supplied IP address. If this routine is successful, the application code should make successive calls to the *nx_http_client_put_packet* routine to actually send the resource contents to the HTTP Server.

> **Note:** The resource string can refer to a local file e.g. ``` "/index.htm" ``` or it can refer to another URL e.g. ```http://abc.website.com/index.htm``` if the HTTP Server indicates it supports referring PUT requests.

This service replaces *nx_http_client_put_start*. It requires caller to specify the length of the resource, username and password.

### Input Parameters

 - *client_ptr*: Pointer to HTTP Client control block.
 - *ip_address*: IP address of the HTTP Server.
 - *resource*: Pointer to URL string for requested resource.
 - *resource_length*: Length of URL string for resource to send to Server.
 - *username*: Pointer to optional user name for authentication.
 - *username_length*: Length of optional user name for authentication.
 - *password*: Pointer to optional password for authentication.
 - *password_length*: Length of optional password for authentication.
 - *total_bytes*: Total bytes of resource being sent. Note that the combined length of all packets sent via subsequent calls to *nx_http_client_put_packet* must equal this value.
 - *wait_option*: Defines how long the service will wait for the HTTP Client PUT start. The wait options are defined as follows:
    - **time out value** (0x00000001 through 0xFFFFFFFE)
    - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting **TX_WAIT_FOREVER** causes the calling thread to suspend indefinitely until the HTTP Server responds to the request. Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.

### Return Values

 - **NX_SUCCESS*: (0x00) Successfully sent PUT request
 - **NX_HTTP_USERNAME_TOO_LONG*: (0xF1) Username too large for buffer
 - **NX_HTTP_NOT_READY*: (0xEA) HTTP Client not ready
 - NX_PTR_ERROR (0x07) Invalid pointer input
 - NX_SIZE_ERROR (0x09) Invalid total size of resource
 - NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```c
/* Start an HTTP PUT to place the 20-byte resource "/TEST.HTM" on the HTTP Server
   at IP address 1.2.3.5. */
status =  nx_http_client_put_start_extended(&my_client, IP_ADDRESS(1, 2, 3, 5),
"/TEST.HTM", 9, "myname", 6, "mypassword", 10, 20, NX_WAIT_FOREVER);

/* If status is NX_SUCCESS, the PUT operation for TEST.HTM has successfully been
   started. */
```

## nxd_http_client_put_start

Start an HTTP PUT request (IPv4 or IPv6)

### Prototype

```c
UINT nxd_http_client_put_start(
    NX_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS *server_ip,
    CHAR *resource,
    CHAR *username,
    CHAR *password,
    ULONG total_bytes,
    ULONG wait_option);
```

### Description

This service attempts to PUT (send) the specified resource on the HTTP Server at the supplied IP address over IPv6. If this routine is successful, the application code should make successive calls to the *nx_http_client_put_packet* routine to actually send the resource contents to the HTTP Server.

> **Note:** The resource string can refer to a local file e.g. ``` "/index.htm" ``` or it can refer to another URL e.g. ```http://abc.website.com/index.htm``` if the HTTP Server indicates it supports referring PUT requests.

This service is deprecated. Developers are encouraged to migrate to *nxd_http_client_put_start_extended*.

### Input Parameters

 - *client_ptr*: Pointer to HTTP Client control block.
 - *server_ip*: IP address of the HTTP Server.
 - *resource*: Pointer to URL string for resource to send to Server.
 - *username*: Pointer to optional user name for authentication.
 - *password*: Pointer to optional password for authentication.
 - *total_bytes*: Total bytes of resource being sent. Note that the combined length of all packets sent via subsequent calls to *nx_http_client_put_packet* must equal this value.
 - *wait_option*: Defines how long the service will wait for the HTTP Client PUT start. The wait options are defined as follows:
    - **time out value** (0x00000001 through 0xFFFFFFFE)
    - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting **TX_WAIT_FOREVER** causes the calling thread to suspend indefinitely until the HTTP Server responds to the request. Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.

### Return Values

 - **NX_SUCCESS** (0x00) Successfully sent HTTP Client PUT request
 - **NX_HTTP_ERROR** (0xE0) HTTP Client internal error
 - **NX_HTTP_NOT_READY** (0xEA) HTTP Client not ready
 - **NX_HTTP_FAILED** (0xE2) HTTP Client error communicating with the HTTP Server
 - NX_PTR_ERROR (0x07) Invalid pointer input
 - NX_SIZE_ERROR (0x09) Invalid total size of resource
 - NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```c
NXD_ADDRESS server_ip_address;

/* for an IPv4 address, define as follows: */
server_ip_address.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_address.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,4);

/* for an IPv6 address, define as follows: */
server_ip_address.nxd_ip_version = NX_IP_VERSION_V6;
server_ip_address.nxd_ip_address.v6[0] = 0x20010db8;
server_ip_address.nxd_ip_address.v6[1] = 0x0;
server_ip_address.nxd_ip_address.v6[2] = 0xf101;
server_ip_address.nxd_ip_address.v6[3] = 0x106;

/* Start an HTTP PUT to place the 20-byte resource Client_test.HTM" on the HTTPv6  
   Server. */
status =  nxd_http_client_put_start(&my_client, &server_ip_address,
 									"/client_test.htm", "name", "password", 103, 50);

/* If status is NX_SUCCESS, the PUT operation for Client_test.HTM has successfully
   been started. */
```

## nxd_http_client_put_start_extended

Start an HTTP PUT request (IPv4 or IPv6)

### Prototype

```c
UINT nxd_http_client_put_start_extended(
    NX_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS *server_ip,
    CHAR *resource,
    UINT resource_length,
    CHAR *username,
    UINT username_length,
    CHAR *password,
    UINT password_length,
    ULONG total_bytes,
    ULONG wait_option);
```

### Description

This service attempts to PUT (send) the specified resource on the HTTP Server at the supplied IP address over IPv6. If this routine is successful, the application code should make successive calls to the *nx_http_client_put_packet* routine to actually send the resource contents to the HTTP Server.

> **Note:** The resource string can refer to a local file e.g. ``` "/index.htm" ``` or it can refer to another URL e.g. ```http://abc.website.com/index.htm``` if the HTTP Server indicates it supports referring PUT requests.

This service replaces *nxd_http_client_put_start*. It requires caller to specify the length of the resource, username and password.

### Input Parameters

 - *client_ptr*: Pointer to HTTP Client control block.
 - *ip_address*: IP address of the HTTP Server.
 - *resource*: Pointer to URL string for requested resource.
 - *resource_length*: Length of URL string for resource to send to Server.
 - *username*: Pointer to optional user name for authentication.
 - *username_length*: Length of optional user name for authentication.
 - *password*: Pointer to optional password for authentication.
 - *password_length*: Length of optional password for authentication.
 - *total_bytes*: Total bytes of resource being sent. Note that the combined length of all packets sent via subsequent calls to *nx_http_client_put_packet* must equal this value.
 - *wait_option*: Defines how long the service will wait for the HTTP Client PUT start. The wait options are defined as follows:
    - **time out value** (0x00000001 through 0xFFFFFFFE)
    - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting **TX_WAIT_FOREVER** causes the calling thread to suspend indefinitely until the HTTP Server responds to the request. Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.

### Return Values

 - **NX_SUCCESS** (0x00) Successfully sent HTTP Client PUT request
 - **NX_HTTP_ERROR** (0xE0) HTTP Client internal error
 - **NX_HTTP_NOT_READY** (0xEA) HTTP Client not ready
 - NX_HTTP_FAILED (0xE2) HTTP Client error communicating with the HTTP Server
 - NX_PTR_ERROR (0x07) Invalid pointer input
 - NX_SIZE_ERROR (0x09) Invalid total size of resource
 - NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```c
NXD_ADDRESS server_ip_address;

/* for an IPv4 address, define as follows: */
server_ip_address.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_address.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,4);

/* for an IPv6 address, define as follows: */
server_ip_address.nxd_ip_version = NX_IP_VERSION_V6;
server_ip_address.nxd_ip_address.v6[0] = 0x20010db8;
server_ip_address.nxd_ip_address.v6[1] = 0x0;
server_ip_address.nxd_ip_address.v6[2] = 0xf101;
server_ip_address.nxd_ip_address.v6[3] = 0x106;

/* Start an HTTP PUT to place the 20-byte resource Client_test.HTM" on the HTTPv6  
   Server. */
status =  nxd_http_client_put_start_extended(&my_client, &server_ip_address,
											"/client_test.htm", 16, "name", 4, "password", 8, 103, 50);

/* If status is NX_SUCCESS, the PUT operation for Client_test.HTM has successfully
   been started. */
```

## nx_http_client_put_packet

Send next resource data packet

### Prototype

```c
UINT nx_http_client_put_packet(
    NX_HTTP_CLIENT *client_ptr,
    NX_PACKET *packet_ptr,
    ULONG wait_option);
```

### Description

This service attempts to send the next packet of resource content to the HTTP Server.

> **Note:** This routine should be called repetitively until the combined length of the packets sent equals the "total_bytes" specified in the previous *nx_http_client_put_start* call.

### Input Parameters

 - *client_ptr*: Pointer to HTTP Client control block.
 - *packet_ptr*: Pointer to next content of the resource to being sent to the HTTP Server.
 - *wait_option*: Defines how long the service will wait internally to process the HTTP Client PUT packet. The wait options are defined as follows:
    - **time out value** (0x00000001 through 0xFFFFFFFE)
    - **TX_WAIT_FOREVER** (0xFFFFFFFF) Selecting **TX_WAIT_FOREVER** causes the calling thread to suspend indefinitely until the HTTP Server responds to the request. Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.

### Return Values

 - **NX_SUCCESS** (0x00) Successfully sent HTTP Client packet.
 - **NX_HTTP_NOT_READY** (0xEA) HTTP Client not ready
 - **NX_HTTP_REQUEST_UNSUCCESSFUL_CODE** (0xEE) Received Server error code
 - **NX_HTTP_BAD_PACKET_LENGTH** (0xED) Invalid packet length
 - **NX_HTTP_AUTHENTICATION_ERROR** (0xEB) Invalid name and/or Password
 - **NX_HTTP_INCOMPLETE_PUT_ERROR** (0xEF) Server responds before PUT Is complete
 - NX_PTR_ERROR (0x07) Invalid pointer input
 - NX_INVALID_PACKET (0x12) Packet too small for TCP header
 - NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```c
/* Send a 20-byte packet representing the content of the resource
   "/TEST.HTM" to the HTTP Server. */
status =  nx_http_client_put_packet(NX_HTTP_CLIENT *client_ptr, NX_PACKET *packet_ptr, ULONG wait_option);

/* If status is NX_SUCCESS, the 20-byte resource contents of TEST.HTM has
   successfully been sent. */
```

## nx_http_client_set_connect_port

Set the connection port to the Server

### Prototype

```c
UINT nx_http_client_set_connect_port(
    NX_HTTP_CLIENT *client_ptr,
    UINT port);
```

### Description

This service changes the connect port when connecting to the HTTP Server to the specified port at runtime. Otherwise the connect port defaults to 80. This must be called before ***nx_http_client_get_start*** and ***nx_http_client_put_start*** e.g. when the HTTP Client connects with the Server.

### Input Parameters

 - *client_ptr*: Pointer to HTTP Client control block.
 - *port*: Port for connecting to the Server.

### Return Values

 - **NX_SUCCESS** (0x00) Successfully change port
 - NX_INVALID_PORT (0x46) Port exceeds the maximum (0xFFFF) or is zero
 - NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads, Initialization

### Example

```c
NX_HTTP_CLIENT *client_ptr;

/* Change the connect port to 114. */
status =  nx_http_client_set_connect_port(client_ptr, 114);

/* If status is NX_SUCCESS, the connect port is successfully changed. */
```

## nx_http_server_cache_info_callback_set

Set the callback to retrieve URL max age and date

### Prototype

```c
UINT nx_http_server_cache_info_callback_set(
    NX_HTTP_SERVER *server_ptr,
    UINT (*cache_info_get)(
        CHAR *resource,
        UINT *max_age,
        NX_HTTP_SERVER_DATE *date));
```

### Description

This service sets the callback service invoked to obtain the maximum age and last modified date of the specified resource.

### Input Parameters

 - *server_ptr*: Pointer to HTTP Server control block.
 - *cache_info_get*: Pointer to the callback
 - *max_age*: Pointer to maximum age of a resource
 - *data*: Pointer to last modified date returned.

### Return Values

 - **NX_SUCCESS** (0x00) Successfully set the callback
 - NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Initialization

### Example

```c
NX_HTTP_SERVER my_server;

UINT cache_info_get(CHAR *resource, UINT *max_age,
                           NX_HTTP_SERVER_DATE *last_modified);

/* After my_server is created with nx_http_server_create and before the HTTP  
   server is set by nx_http_server_start, set the cache info callback: */

status = nx_http_server_cache_info_callback_set(&my_server, cache_info_get);


/* If status is NX_SUCCESS, the callback was successfully sent. */
```

## nx_http_server_callback_data_send

Send data from callback function

### Prototype

```c
UINT nx_http_server_callback_data_send(
    NX_HTTP_SERVER *server_ptr,
    VOID *data_ptr,
    ULONG data_length);
```

### Description

This service sends the data in the supplied packet from the application's callback routine. This is typically used to send dynamic data associated with GET/POST requests.

> **Note:** If this function is used, the callback routine is responsible for sending the entire response in the proper format. In addition, the callback routine must return the status of NX_HTTP_CALLBACK_COMPLETED.

### Input Parameters

 - *server_ptr*: Pointer to HTTP Server control block.
 - *data_ptr*: Pointer to the data to send.
 - *data_length*: Number of bytes to send.

### Return Values

 - **NX_SUCCESS** (0x00) Successfully sent Server data
 - NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```c
UINT  my_request_notify(NX_HTTP_SERVER *server_ptr, UINT request_type,
  CHAR *resource, NX_PACKET *packet_ptr)
{

    /* Look for the test resource!  */
    if ((request_type == NX_HTTP_SERVER_GET_REQUEST) &&
                (strcmp(resource, "/test.htm") == 0))
    {

        /* Found it, override the GET processing by sending the resource
    contents directly. */

        nx_http_server_callback_data_send(server_ptr,
    "HTTP/1.0 200 \r\nContent-Length:
    103\r\nContent-Type: text/html\r\n\r\n",
    63);

        nx_http_server_callback_data_send(server_ptr, "<HTML>\r\n<HEAD><TITLE>NetX
    HTTP Test </TITLE></HEAD>\r\n
    <BODY>\r\n<H1>NetX Test Page
    </H1>\r\n</BODY>\r\n</HTML>\r\n", 103);

        /* Return completion status. */
        return(NX_HTTP_CALLBACK_COMPLETED);
    }

    return(NX_SUCCESS);
}
```

## nx_http_server_callback_generate_response_header

Create a response header in a callback function

### Prototype

```c
UINT nx_http_server_callback_generate_response_header(
    NX_HTTP_SERVER *server_ptr,
    NX_PACKET **packet_pptr,
    CHAR *status_code,
    UINT content_length,
    CHAR *content_type,
    CHAR* additional_header);
```

### Description

This service calls the internal function *_nx_http_server_generate_response_header* when the HTTP server responds to Client get, put and delete requests. It is intended for use in HTTP server callback functions when the HTTP server application is designing its response to the Client.

This service is deprecated. Developers are encouraged to migrate to *nxd_http_server_callback_generate_response_header_extended*.

### Input Parameters

 - *server_ptr*: Pointer to HTTP Server control block.
 - *packet_pptr*: Pointer a packet pointer allocated for message
 - *status_code*: Indicate status of resource. Examples:
 	- **NX_HTTP_STATUS_OK**
	- **NX_HTTP_STATUS_MODIFIED**
	- **NX_HTTP_STATUS_INTERNAL_ERROR**
 - *content_length*: Size of content in bytes
 - *content_type*: Type of HTTP e.g. "text/plain"
 - *additional_header*: Pointer to additional header text

### Return Values

 - **NX_SUCCESS** (0x00) Successfully created header
 - NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```c
CHAR demotestbuffer[] = "<html>\r\n\r\n<head>\r\n\r\n<title>Main   \
			Window</title>\r\n</head>\r\n\r\n<body>Test message\r\n   \  </body>\r\n</html>\r\n";

     /* my_request_notify is the application request notify callback registered with
      the HTTP server in nx_http_server_create, creates a response to the received  
      Client request. */

      UINT  my_request_notify(NX_HTTP_SERVER *server_ptr, UINT request_type,
 CHAR *resource, NX_PACKET *recv_packet_ptr)
      {

NX_PACKET   *sresp_packet_ptr;
ULONG       string_length;
CHAR        temp_string[30];
ULONG       length = 0;


   length = sizeof(demotestbuffer) - 1;

/* Derive the client request type from the client request. */
   string_length =  (ULONG) nx_http_server_type_retrieve(server_ptr, server_ptr ->
nx_http_server_request_resource, temp_string,
sizeof(temp_string));

       /* Null terminate the string. */
   temp_string[temp] = 0;

       /* Now build a response header with server status is OK and no additional header  
          info. */
   status = nx_http_server_callback_generate_response_header(http_server_ptr,
                  &resp_packet_ptr, NX_HTTP_STATUS_OK,
                  length, temp_string, NX_NULL);

/* If status is NX_SUCCESS, the header was successfully appended. */

/* Now add data to the packet. */
   status = nx_packet_data_append(resp_packet_ptr, &demotestbuffer[0],
                 length,  server_ptr ->
                 nx_http_server_packet_pool_ptr, NX_WAIT_FOREVER);
   if (status != NX_SUCCESS)
   {
       nx_packet_release(resp_packet_ptr);
       return status;
   }



/* Now send the packet! */
           status = nx_tcp_socket_send(&(server_ptr -> nx_http_server_socket),
                                      resp_packet_ptr, NX_HTTP_SERVER_TIMEOUT_SEND);

   if (status != NX_SUCCESS)
   {
      nx_packet_release(resp_packet_ptr);

      return status;
   }

/* Let HTTP server know the response has been sent. */
  return NX_HTTP_CALLBACK_COMPLETED;

     }
```

## nx_http_server_callback_generate_response_header_extended

Create a response header in a callback function

### Prototype

```c
UINT nx_http_server_callback_generate_response_header_extended(
    NX_HTTP_SERVER *server_ptr,
    NX_PACKET **packet_pptr,
    CHAR *status_code,
    UINT status_code_length,
    UINT content_length,
    CHAR *content_type,
    UINT content_type_length,
    CHAR *additional_header,
    UINT additional_header_length);
```

### Description

This service calls the internal function *_nx_http_server_generate_response_header* when the HTTP server responds to Client get, put and delete requests. It is intended for use in HTTP server callback functions when the HTTP server application is designing its response to the Client.

This service replaces *nx_http_server_callback_generate_response_header*. This version supplies additional length information to the callback function.

### Input Parameters

 - *server_ptr*: Pointer to HTTP Server control block.
 - *packet_pptr*: Pointer a packet pointer allocated for message
 - *status_code*: Indicate status of resource. Examples:
 	- **NX_HTTP_STATUS_OK**
 	- **NX_HTTP_STATUS_MODIFIED**
 	- **NX_HTTP_STATUS_INTERNAL_ERROR**
 - *status_code*: Length of status code
 - *content_length*: Size of content in bytes
 - *content_type*: Type of HTTP e.g. "text/plain"
 - *content_type_length*: Length of HTTP type
 - *additional_header*: Pointer to additional header text
 - *additional_header_length*: Length of additional header text

### Return Values

 - **NX_SUCCESS** (0x00) Successfully created header
 - NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```c
CHAR demotestbuffer[] = "<html>\r\n\r\n<head>\r\n\r\n<title>Main   \
	 Window</title>\r\n</head>\r\n\r\n<body>Test message\r\n   \  </body>\r\n</html>\r\n";

     /* my_request_notify is the application request notify callback registered with
      the HTTP server in nx_http_server_create, creates a response to the received  
      Client request. */

      UINT  my_request_notify(NX_HTTP_SERVER *server_ptr, UINT request_type,
 CHAR *resource, NX_PACKET *recv_packet_ptr)
      {

NX_PACKET   *sresp_packet_ptr;
ULONG       string_length;
CHAR        temp_string[30];
ULONG       length = 0;


   length = sizeof(demotestbuffer) - 1;

/* Derive the client request type from the client request. */
   string_length =  (ULONG) nx_http_server_type_retrieve(server_ptr, server_ptr ->
nx_http_server_request_resource, temp_string,
sizeof(temp_string));

       /* Null terminate the string. */
   temp_string[temp] = 0;

       /* Now build a response header with server status is OK and no additional header  
          info. */
   status = nx_http_server_callback_generate_response_header_extended(
                             http_server_ptr, &resp_packet_ptr, NX_HTTP_STATUS_OK,
              sizeof(NX_HTTP_STATUS_OK) - 1, length,
              temp_string, string_length, NX_NULL, 0);

/* If status is NX_SUCCESS, the header was successfully appended. */

/* Now add data to the packet. */
   status = nx_packet_data_append(resp_packet_ptr, &demotestbuffer[0],
                 length,  server_ptr ->
                 nx_http_server_packet_pool_ptr, NX_WAIT_FOREVER);
   if (status != NX_SUCCESS)
   {
       nx_packet_release(resp_packet_ptr);
       return status;
   }



/* Now send the packet! */
           status = nx_tcp_socket_send(&(server_ptr -> nx_http_server_socket),
                                      resp_packet_ptr, NX_HTTP_SERVER_TIMEOUT_SEND);

   if (status != NX_SUCCESS)
   {
      nx_packet_release(resp_packet_ptr);

      return status;
   }

/* Let HTTP server know the response has been sent. */
  return NX_HTTP_CALLBACK_COMPLETED;

     }
```

## nx_http_server_callback_packet_send

Send an HTTP packet from callback function

### Prototype

```c
UINT nx_http_server_callback_packet_send(
    NX_HTTP_SERVER *server_ptr,
    NX_PACKET *packet_ptr);
```

### Description

This service sends a complete HTTP server response from an HTTP callback. HTTP server will send the packet with the NX_HTTP_SERVER _TIMEOUT_SEND. The HTTP header and data must be appended to the packet. If the return status indicates an error, the HTTP application must release the packet.

The callback should return NX_HTTP_CALLBACK_COMPLETED.

See *nx_http_server_callback_generate_response_header* for a more detailed example.

### Input Parameters

 - *server_ptr*: Pointer to HTTP Server control block.
 - *packet_ptr*: Pointer to the packet to send

### Return Values

 - **NX_SUCCESS** (0x00) Successfully sent Server packet
 - NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```c
/* The packet is appended with HTTP header and data and is ready to send to the
   Client directly. */

   status = nx_http_server_callback_response_send(server_ptr, packet_ptr);

   if (status != NX_SUCCESS)
   {

	nx_packet_release(packet_ptr);
   }

    return(NX_HTTP_CALLBACK_COMPLETED);
```

## nx_http_server_callback_response_send

Send response from callback function

### Prototype

```c
UINT nx_http_server_callback_response_send(
    NX_HTTP_SERVER *server_ptr,
    CHAR *header,
    CHAR *information,
    CHAR additional_info);
```

### Description

This service sends the supplied response information from the application's callback routine. This is typically used to send custom responses associated with GET/POST requests.

> **Note:** If this function is used, the callback routine must return the status of NX_HTTP_CALLBACK_COMPLETED.

This service is deprecated. Developers are encouraged to migrate to *nxd_http_server_callback_response_send_extended*.

### Input Parameters

 - *server_ptr*: Pointer to HTTP Server control block.
 - *header*: Pointer to the response header string.
 - *information*: Pointer to the information string.
 - *additional_info*: Pointer to the additional information string.

### Return Values

 - **NX_SUCCESS** (0x00) Successfully sent Server response

### Allowed From

Threads

### Example

```c
UINT  my_request_notify(NX_HTTP_SERVER *server_ptr, UINT request_type,
					    CHAR *resource, NX_PACKET *packet_ptr)
{

    /* Look for the test resource!  */
    if ((request_type == NX_HTTP_SERVER_GET_REQUEST) &&
               (strcmp(resource, "/test.htm") == 0))
    {

        /* In this example, we will complete the GET processing with
           a resource not found response. */
	 nx_http_server_callback_response_send(server_ptr,
                      "HTTP/1.0 404 ",
                      "NetX HTTP Server unable to find  
                       file: ", resource);

        /* Return completion status. */
        return(NX_HTTP_CALLBACK_COMPLETED);
    }

    return(NX_SUCCESS);
}
```

## nx_http_server_callback_response_send_extended

Send response from callback function

### Prototype

```c
UINT nx_http_server_callback_response_send_extended(
    NX_HTTP_SERVER *server_ptr,
    CHAR *header,
    UINT header_length,
    CHAR *information,
    UINT information_length,
    CHAR *additional_info,
    UINT additional_info_length);
```

### Description

This service sends the supplied response information from the application's callback routine. This is typically used to send custom responses associated with GET/POST requests.

> **Note:** If this function is used, the callback routine must return the status of NX_HTTP_CALLBACK_COMPLETED.

This service replaces *nx_http_server_callback_response_send*. This version takes additional length information as arguments.

### Input Parameters

 - *server_ptr*: Pointer to HTTP Server control block.
 - *header*: Pointer to the response header string.
 - *header_length*: Length of the response header string.
 - *information*: Pointer to the information string.
 - *information_length*: Length of the information string.
 - *additional_info*: Pointer to the additional information string.
 - *additional_info_length*: Length of the additional information string.

### Return Values

 - **NX_SUCCESS** (0x00) Successfully sent Server response

### Allowed From

Threads

### Example

```c
UINT  my_request_notify(NX_HTTP_SERVER *server_ptr, UINT request_type,
						CHAR *resource, NX_PACKET *packet_ptr)
{

    /* Look for the test resource!  */
    if ((request_type == NX_HTTP_SERVER_GET_REQUEST) &&
               (strcmp(resource, "/test.htm") == 0))
    {

        /* In this example, we will complete the GET processing with
           a resource not found response. */
	 nx_http_server_callback_response_send_extended(
                                             server_ptr,
                      "HTTP/1.0 404 ", 12,
                      "NetX HTTP Server unable to find  
                       file: ", 38, resource, 9);

        /* Return completion status. */
        return(NX_HTTP_CALLBACK_COMPLETED);
    }

    return(NX_SUCCESS);
}
```

## nx_http_server_content_get

Get content from the request

### Prototype

```c
UINT nx_http_server_content_get(
    NX_HTTP_SERVER *server_ptr,
    NX_PACKET *packet_ptr,
    ULONG byte_offset,
    CHAR *destination_ptr,
    UINT destination_size,
    UINT *actual_size);
```

### Description

This service attempts to retrieve the specified amount of content from the POST or PUT HTTP Client request. It should be called from the application's request notify callback specified during HTTP Server creation (***nx_http_server_create***).

This service is deprecated. Developers are encouraged to migrate to *nx_http_server_content_get_extended*.

### Input Parameters

 - *server_ptr*: Pointer to HTTP Server control block.
 - *packet_ptr*: Pointer to the HTTP Client request packet. Note that this packet must not be released by the request notify callback.
 - *byte_offset*: Number of bytes to offset into the content area.
 - *destination_ptr*: Pointer to the destination area for the content.
 - *destination_size*: Maximum number of bytes available in the destination area.
 - *actual_size*: Pointer to the destination variable that will be set to the actual size of the content copied.

### Return Values

 - **NX_SUCCESS** (0x00) Successful HTTP Server content get
 - **NX_HTTP_ERROR** (0xE0) HTTP Server internal error
 - **NX_HTTP_DATA_END** (0xE7) End of request content
 - **NX_HTTP_TIMEOUT** (0xE1) HTTP Server timeout in getting next packet of content
 - NX_PTR_ERROR (0x07) Invalid pointer input
 - NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```c
/* Assuming we are in the application's request notify callback
   routine, retrieve up to 100 bytes of content starting at offset
   0. */
status =  nx_http_server_content_get(&my_server, packet_ptr,
                      0, my_buffer, 100, &actual_size);

/* If status is NX_SUCCESS, "my_buffer" contains "actual_size" bytes of
   request content. */
```

## nx_http_server_content_get_extended

Get content from the request/supports zero length Content Length

### Prototype

```c
UINT nx_http_server_content_get_extended(
    NX_HTTP_SERVER *server_ptr,
    NX_PACKET *packet_ptr,
    ULONG byte_offset,
    CHAR *destination_ptr,
    UINT destination_size,
    UINT *actual_size);
```

### Description

This service is almost identical to ***nx_http_server_content_get*** it attempts to retrieve the specified amount of content from the POST or PUT HTTP Client request. However it handles requests with Content Length of zero value ('empty request') as a valid request. It should be called from the application's request notify callback specified during HTTP Server creation (***nx_http_server_create***).

This service replaces ***nx_http_server_content_get***. This version requires caller to supply additional length information.

### Input Parameters

 - *server_ptr*: Pointer to HTTP Server control block.
 - *packet_ptr*: Pointer to the HTTP Client request packet. Note that this packet must not be released by the request notify callback.
 - *byte_offset*: Number of bytes to offset into the content area.
 - *destination_ptr*: Pointer to the destination area for the content.
 - *destination_size*: Maximum number of bytes available in the destination area.
 - *actual_size*: Pointer to the destination variable that will be set to the actual size of the content copied.

### Return Values

 - **NX_SUCCESS** (0x00) Successful HTTP content get
 - **NX_HTTP_ERROR** (0xE0) HTTP Server internal error
 - **NX_HTTP_DATA_END** (0xE7) End of request content
 - **NX_HTTP_TIMEOUT** (0xE1) HTTP Server timeout in getting next packet
 - NX_PTR_ERROR (0x07) Invalid  pointer input
 - NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```c
/* Assuming we are in the application's request notify callback
   routine, retrieve up to 100 bytes of content starting at offset
   0. */
status =  nx_http_server_content_get_extended(&my_server, packet_ptr,
                               0, my_buffer, 100, &actual_size);

/* If status is NX_SUCCESS, "my_buffer" contains "actual_size" bytes of
   request content. */
```

## nx_http_server_content_length_get

Get length of content in the request

### Prototype

```c
UINT nx_http_server_content_length_get(NX_PACKET *packet_ptr);
```

### Description

This service attempts to retrieve the HTTP content length in the supplied packet. If there is no HTTP content, this routine returns a value of zero. It should be called from the application's request notify callback specified during HTTP Server creation (*nx_http_server_create*).

This service is deprecated. Developers are encouraged to migrate to *nx_http_server_content_length_get_extended*.

### Input Parameters

 - *packet_ptr*: Pointer to the HTTP Client request packet. Note that this packet must not be released by the request notify callback.

### Return Values

 - **content length** On error, a value of zero is returned  

### Allowed From

Threads

### Example

```c
/* Assuming we are in the application's request notify callback
   routine, get the content length of the HTTP Client request. */
length =  nx_http_server_content_length_get(packet_ptr);

/* The "length" variable now contains the length of the HTTP Client
   request content area. */
```

## nx_http_server_content_length_get_extended

Get length of content in the request/supports Content Length of zero value

### Prototype

```c
UINT nx_http_server_content_length_get_extended(
    NX_PACKET *packet_ptr,
    UINT *content_length);
```

### Description

This service is similar to ***nx_http_server_content_length_get**;* attempts to retrieve the HTTP content length in the supplied packet. However, the return value indicates successful completion status, and the actual length value is returned in the input pointer *content_length*. If there is no HTTP content/Content Length = 0, this routine still returns a successful completion status and the content_length input pointer points to a valid length (zero). It should be called from the application's request notify callback specified during HTTP Server creation (***nx_http_server_create***).

This service replaces ***nx_http_server_content_length_get***.

### Input Parameters

 - *packet_ptr*: Pointer to the HTTP Client request packet. Note that this packet must not be released by the request notify callback.
 - *content_length*: Pointer to value retrieved from Content Length field

### Return Values

 - **NX_SUCCESS** (0x00) Successful Server content get
 - **NX_HTTP_INCOMPLETE_PUT_ERROR** (0xEF) Improper HTTP header format
 - NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```c
/* Assuming we are in the application's request notify callback
   routine, get the content length of the HTTP Client request. */
ULONG content_length;

status =  nx_http_server_content_length_get_extended(packet_ptr, &content_length);

/* If the "status" variable indicates successful completion, the"length" variable
   contains the length of the HTTP Client request content area. */
```

## nx_http_server_create

Create an HTTP Server instance

### Prototype

```c
UINT nx_http_server_create(
    NX_HTTP_SERVER *http_server_ptr,
	 CHAR *http_server_name,
    NX_IP *ip_ptr,
    FX_MEDIA *media_ptr,
    VOID *stack_ptr,
    ULONG stack_size,
    NX_PACKET_POOL *pool_ptr,
    UINT (*authentication_check)(
        NX_HTTP_SERVER *server_ptr,
        UINT request_type,
        CHAR *resource,
        CHAR **name,
        CHAR **password,
        CHAR **realm),
	  UINT (*request_notify)(
        NX_HTTP_SERVER *server_ptr,
        INT request_type,
        CHAR *resource,
        NX_PACKET *packet_ptr));
```

### Description

This service creates an HTTP Server instance, which runs in the context of its own ThreadX thread. The optional *authentication_check* and request_notify application callback routines give the application software control over the basic operations of the HTTP Server.

### Input Parameters

 - *http_server_ptr*: Pointer to HTTP Server control block.
 - *http_server_name*: Pointer to HTTP Server's name.
 - *ip_ptr*: Pointer to previously created IP instance.
 - *media_ptr*: Pointer to previously created FileX media instance.
 - *stack_ptr*: Pointer to HTTP Server thread stack area.
 - *stack_size*: Pointer to HTTP Server thread stack size.
 - *authentication_check*: Function pointer to application's authentication checking routine. If specified, this routine is called for each HTTP Client request. If this parameter is NULL, no authentication will be performed.
 - *request_notify*: Function pointer to application's request notify routine. If specified, this routine is called prior to the HTTP server processing of the request. This allows the resource name to be redirected or fields within a resource to be updated prior to completing the HTTP Client request.

### Return Values

 - **NX_SUCCESS** (0x00) Successful HTTP Server create.
 - NX_PTR_ERROR (0x07) Invalid HTTP Server, IP, media, stack, or packet pool pointer.
 - NX_HTTP_POOL_ERROR (0xE9) Packet payload of pool is not large enough to contain complete HTTP request.

### Allowed From

Initialization, Threads

### Example

```c
/* Create an HTTP Server instance called "my_server."  */
status =  nx_http_server_create(&my_server, "my server", &ip_0, &ram_disk,
			  stack_ptr, stack_size, &pool_0,
			  my_authentication_check, my_request_notify);

/* If status equals NX_SUCCESS, the HTTP Server creation was successful. */
```

## nx_http_server_delete

Delete an HTTP Server instance

### Prototype

```c
UINT nx_http_server_delete(NX_HTTP_SERVER *http_server_ptr);
```

### Description

This service deletes a previously created HTTP Server instance.

### Input Parameters

 - *http_server_ptr*: Pointer to HTTP Server control block.

### Return Values

 - **NX_SUCCESS** (0x00) Successful HTTP Server delete
 - NX_PTR_ERROR (0x07) Invalid HTTP Server pointer
 - NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```c
/* Delete the HTTP Server instance called "my_server."  */
status =  nx_http_server_delete(&my_server);

/* If status equals NX_SUCCESS, the HTTP Server delete was successful. */
```

## nx_http_server_get_entity_content

Retrieve the location and length of entity data

### Prototype

```c
UINT nx_http_server_get_entity_content(
    NX_HTTP_SERVER *server_ptr,
    NX_PACKET **packet_pptr,
    ULONG *available_offset,
    ULONG *available_length);
```

### Description

This service determines the location of the start of data within the current multipart entity in the received Client messages, and the length of data not including the boundary string. Internally HTTP server updates its own offsets so that this function can be called again on the same Client datagram for messages with multiple entities. The packet pointer is updated to the next packet where the Client message is a multi-packet datagram.

> **Note:** **NX_HTTP_MULTIPART_ENABLE** must be enabled to use this service.

See [*nx_http_server_get_entity_header*](#nx_http_server_get_entity_header) for more details.

### Input Parameters

 - *server_ptr*: Pointer to HTTP Server
 - *packet_pptr*: Pointer to location of packet pointer. Note that the application should not release this packet.
 - *available_offset*: Pointer to offset of entity data from the packet prepend pointer
 - *available_length*: Pointer to length of entity data

### Return Values

 - **NX_SUCCESS** (0x00) Successfully retrieved size and location of entity content
 - **NX_HTTP_BOUNDARY_ALREADY_FOUND** (0xF4) Content for the HTTP server  internal multipart markers is already found
 - NX_HTTP_ERROR (0xE0) Internal HTTP error
 - NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```c
NX_HTTP_SERVER my_server;

UINT        offset, length;
NX_PACKET  *packet_ptr;

/* Inside the request notify callback, the HTTP server application first obtains
   the entity header to determine details about the multipart data. If
   successful, it then calls this service to get the location of entity data: */

status =  nx_http_server_get_entity_content(&my_server, &packet_ptr, *offset,
&length);

/* If status equals NX_SUCCESS, offset and location determine the location of the
   entity data. */
```

## nx_http_server_get_entity_header

Retrieve the contents of entity header

### Prototype

```c
UINT nx_http_server_get_entity_header(
    NX_HTTP_SERVER *server_ptr,
    NX_PACKET **packet_pptr,
    UCHAR *entity_header_buffer,
    ULONG buffer_size);
```

### Description

This service retrieves the entity header into the specified buffer. Internally HTTP Server updates its own pointers to locate the next multipart entity in a Client datagram with multiple entity headers. The packet pointer is updated to the next packet where the Client message is a multi-packet datagram.

> **Note:** **NX_HTTP_MULTIPART_ENABLE** must be enabled to use this service.

### Input Parameters

 - *server_ptr*: Pointer to HTTP Server
 - *packet_pptr*: Pointer to location of packet pointer. Note that the application should not release this packet.
 - *entity_header_buffer*: Pointer to location to store entity header
 - *buffer_size*: Size of input buffer

### Return Values

 - **NX_SUCCESS** (0x00) Successfully retrieved entity header
 - **NX_HTTP_NOT_FOUND** **(0xE6)** Entity header field not found
 - **NX_HTTP_TIMEOUT** **(0xE1)** Time expired to receive next packet for multipacket client message
 - NX_HTTP_ERROR (0xE0) Internal HTTP error
 - NX_PTR_ERROR (0x07) Invalid pointer input
 - NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```c
/* my_request_notify is the application request notify callback registered with
      the HTTP server in nx_http_server_create, creates a response to the received  
      Client request. */

      UINT  my_request_notify(NX_HTTP_SERVER *server_ptr, UINT request_type,
 							  CHAR *resource, NX_PACKET *packet_ptr)
      {

		NX_PACKET   *sresp_packet_ptr;
		UINT        offset, length;
		NX_PACKET   *response_pkt;
		UCHAR       buffer[1440];

    	/* Process multipart data. */
    	if(request_type == NX_HTTP_SERVER_POST_REQUEST)
    	{

	   /* Get the content header. */
	   while(nx_http_server_get_entity_header(server_ptr, &packet_ptr, buffer,
                                         sizeof(buffer)) == NX_SUCCESS)
   {

      /* Header obtained successfully. Get the content data location. */
      while(nx_http_server_get_entity_content(server_ptr, &packet_ptr, &offset,
                                              &length) == NX_SUCCESS)
      {
           /* Write content data to buffer. */
           nx_packet_data_extract_offset(packet_ptr, offset, buffer, length,
                                         &length);
           buffer[length] = 0;
      }

    }

    /* Generate HTTP header. */
    status = nx_http_server_callback_generate_response_header(server_ptr,
                         &response_pkt, NX_HTTP_STATUS_OK, 800, "text/html",
                         "Server: NetXDuo HTTP 5.3\r\n");

    if(status == NX_SUCCESS)
    {
        if(nx_http_server_callback_packet_send(server_ptr, response_pkt) !=
  NX_SUCCESS)
 {
                nx_packet_release(response_pkt);
	 }
    }


}
else
{
	/* Indicate we have not processed the response to client yet.*/
	return(NX_SUCCESS);
}

/* Indicate the response to client is transmitted. */
return(NX_HTTP_CALLBACK_COMPLETED);
```

## nx_http_server_gmt_callback_set

Set the callback to obtain GMT date and time

### Prototype

```c
UINT nx_http_server_gmt_callback_set(
    NX_HTTP_SERVER *server_ptr,
    VOID (*gmt_get)(NX_HTTP_SERVER_DATE *date);
```

### Description

This service sets the callback to obtain GMT date and time with a previously created HTTP server. This service is invoked with the HTTP server is creating a header in HTTP server responses to the Client.

### Input Parameters

 - *server_ptr*: Pointer to HTTP Server
 - *gmt_getv*: Pointer to GMT callback
 - *datev*: Pointer to the date retrieved

### Return Values

 - **NX_SUCCESS** (0x00) Successfully set the callback
 - NX_PTR_ERROR (0x07)	Invalid packet or parameter pointer.

### Allowed From

Threads

### Example

```c
NX_HTTP_SERVER my_server;

VOID get_gmt(NX_HTTP_SERVER_DATE *now);

/* After the HTTP server is created by calling nx_http_server_create, and before
   starting HTTP services when nx_http_server_start is called, set the GMT
   retrieve callback: */

status =  nx_http_server_gmt_callback_set(&my_server, gmt_get);

/* If status equals NX_SUCCESS, the gmt_get will be called to set the HTTP server
   response header date. */
```

## nx_http_server_invalid_userpassword_notify_set

Set the callback to to handle invalid user/password

### Prototype

```c
UINT nx_http_server_invalid_userpassword_notify_set(
    NX_HTTP_SERVER *http_server_ptr,
    UINT (*invalid_username_password_callback)
        (CHAR *resource,
        NXD_ADDRESS *client_address,
        UINT request_type));
```

### Description

This service sets the callback invoked when an invalid username and password is received in a Client get, put or delete request, either by digest or basic authentication. The HTTP server must be previously created.

### Input Parameters

 - *server_ptr*: Pointer to HTTP Server
 - *invalid_username_password_callback*: Pointer to invalid user/pass callback
 - *resource*: Pointer to the resource specified by the client
 - *client_address*: Pointer to client address. Can be IPv4 or IPv6
 - *request_type*: Indicates client request type. May be:
	- **NX_HTTP_SERVER_GET_REQUEST**
	- **NX_HTTP_SERVER_POST_REQUEST**
	- **NX_HTTP_SERVER_HEAD_REQUEST**
	- **NX_HTTP_SERVER_PUT_REQUEST**
	- **NX_HTTP_SERVER_DELETE_REQUEST**

### Return Values

 - **NX_SUCCESS** (0x00) Successfully set the callback
 - NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```c
NX_HTTP_SERVER my_server;
VOID invalid_username_password_callback (NX_ CHAR *resource,
										 NXD_ADDRESS *client_address,
										 UINT   request_type);

/* After the HTTP server is created by calling nx_http_server_create, and before
   starting HTTP services when nx_http_server_start is called, set the invalid
   username password callback: */

status =  nx_http_server_gmt_callback_set(&my_server,
										  invalid_username_password_callback);

/* If status equals NX_SUCCESS, the invalid_username_password_callback function
   will be called when the HTTP server receives an invalid username/password. */
```

## nx_http_server_mime_maps_additional_set

Set additional MIME maps for HTML

### Prototype

```c
UINT nx_http_server_mime_maps_additional_set(
    NX_HTTP_SERVER *server_ptr,
    NX_HTTP_SERVER_MIME_MAP *mime_maps,
    UINT mime_maps_num);
```

### Description

This service allows the HTTP application developer to add additional MIME types from the default MIME types supplied by NetX Duo HTTP Server (see *nx_http_server_get_type* for list of defined types).

When a client request is received, e.g. a GET request, HTTP server parses the requested file type from the HTTP header using preferentially the additional MIME map set and if no match if found, it looks for a match in the default MIME map of the HTTP server. If no match is found, the MIME type defaults to "text/plain".

If the request notify function is registered with the HTTP server, the request notify callback can call ***nx_http_server_type_retrieve*** to parse the file type.

### Input Parameters

 - *server_ptr*: Pointer to HTTP Server instance
 - *mime_maps*: Pointer to a MIME map array
 - *mime_map_num*: Number of MIME maps in array

### Return Values

 - **NX_SUCCESS** (0x00) Successful HTTP Server MIME map set
 - NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Initialization, Threads

### Example

```c
/* my_server is an NX_HTTP_SERVER previously created. */

NX_HTTP_SERVER_MIME_MAP my_mime_maps [2];

static NX_HTTP_SERVER_MIME_MAP my_mime_maps[] =
{
    {"abc",      "yourtype/abc"},
    {"xyz",      "mytype/xyz"},
};

status =  nx_http_server_mime_maps_additional_set(&my_server,
												  &my_mime_maps[0], 2);

/* If status equals NX_SUCCESS, two additional MIME types are added to the HTTP  
  server MIME map set." */
```

## nx_http_server_packet_content_find

Extract content length and set pointer to start of data

### Prototype

```c
UINT nx_http_server_packet_content_find(
    NX_HTTP_SERVER *server_ptr,
    NX_PACKET **packet_ptr,
    UINT *content_length);
```

### Description

This service extracts the content length from the HTTP header. It also  updates the supplied packet as follows: the packet prepend pointer (start of location of packet buffer to write to) is set to the HTTP content (data) just passed the HTTP header.

If the beginning of content is not found in the current packet, the function waits for the next packet to be received using the **NX_HTTP_SERVER_TIMEOUT_RECEIVE** wait option.

> **Note:** This should not be called before calling ***nx_http_server_get_entity_header*** because it modifies the prepend pointer past the entity header.

### Input Parameters

 - *server_ptr*: Pointer to HTTP server instance
 - *packet_ptr*: Pointer to packet pointer for returning the packet with updated prepend pointer
 - *content_length*: Pointer to extracted *content_length*

### Return Values

 - **NX_SUCCESS** (0x00) HTTP content length found and packet successfully updated
 - **NX_HTTP_TIMEOUT** (0xE1) Time expired waiting on next packet
 - NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```c
/* The HTTP server pointed to by server_ptr is previously created and started.
The server has received a Client request packet, recv_packet_ptr, and the packet
content find service is called from the request notify callback function
registered with the HTTP server. */

UINT content_length;

status =  nx_http_server_packet_content_find(server_ptr, recv_packet_ptr,
                                             &content_length);

/* If status equals NX_SUCCESS, the content length specifies the content length
   and the packet pointer prepend pointer is set to the HTTP content (data). */
```

## nx_http_server_packet_get

Receive the next HTTP packet

### Prototype

```c
UINT nx_http_server_packet_get(
    NX_HTTP_SERVER *server_ptr,
    NX_PACKET **packet_ptr);
```

### Description

This service returns the next packet received on the HTTP server socket. The wait option to receive a packet is NX_HTTP_SERVER_TIMEOUT_RECEIVE.

### Input Parameters

 - *server_ptr*: Pointer to HTTP server instance
 - *packet_ptr*: Pointer to received packet

### Return Values

 - **NX_SUCCESS** (0x00) Successfully received next packet
 - **NX_HTTP_TIMEOUT** (0xE1) Time expired waiting on next packet
 - NX_PTR_ERROR (0x07)	Invalid pointer input

### Allowed From

Threads

### Example

```c
/* The HTTP server pointed to by server_ptr is previously created and started. */

UINT content_length;
NX_PACKET *recv_packet_ptr;

status =  nx_http_server_packet_get(server_ptr, &recv_packet_ptr);

/* If status equals NX_SUCCESS, a Client packet is obtained. */
```

## nx_http_server_param_get

Get parameter from the request

### Prototype

```c
UINT nx_http_server_param_get(
    NX_PACKET *packet_ptr,
    UINT param_number, CHAR *param_ptr,
    UINT max_param_size);
```

### Description

This service attempts to retrieve the specified HTTP URL parameter in the supplied request packet. If the requested HTTP parameter is not present,  this routine returns a status of **NX_HTTP_NOT_FOUND**. This routine should be called from the application's request notify callback specified during HTTP Server creation (***nx_http_server_create***).

### Input Parameters

 - *packet_ptr*: Pointer to HTTP Client request packet. Note that the application should not release this packet.
 - *param_number*: Logical number of the parameter starting at zero, from left to right in the parameter list.
 - *param_ptr*: Destination area to copy the parameter.
 - *max_param_size*: Maximum size of the parameter destination area.

### Return Values

 - **NX_SUCCESS** (0x00) Successful HTTP Server parameter get
 - **NX_HTTP_NOT_FOUND** (0xE6) Specified parameter not found
 - **NX_HTTP_IMPROPERLY_TERMINATED_PARAM** (0xF3) Request parameter not properly terminated
 - NX_PTR_ERROR (0x07) Invalid pointer input
 - NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```c
/* Assuming we are in the application's request notify callback
   routine, get the first parameter of the HTTP Client request. */

status =  nx_http_server_param_get(request_packet_ptr, 0, param_destination,
			   30);

/* If status equals NX_SUCCESS, the NULL-terminated first parameter can be found
   in "param_destination." */
```

## nx_http_server_query_get

Get query from the request

### Prototype

```c
UINT nx_http_server_query_get(
    NX_PACKET *packet_ptr,
    UINT query_number,
    CHAR *query_ptr,
    UINT max_query_size);
```

### Description

This service attempts to retrieve the specified HTTP URL query in the supplied request packet. If the requested HTTP query is not present,  this routine returns a status of **NX_HTTP_NOT_FOUND**. This routine should be called from the application's request notify callback specified during HTTP Server creation (***nx_http_server_create***).

### Input Parameters

 - *packet_ptr*: Pointer to HTTP Client request packet. Note that the application should not release this packet.
 - *query_number*: Logical number of the parameter starting at zero, from left to right in the query list.
 - *query_ptr*: Destination area to copy the query.
 - *max_query_size*: Maximum size of the query destination area.

### Return Values

 - **NX_SUCCESS** (0x00) Successful HTTP Server query get
 - **NX_HTTP_FAILED** (0xE2) Query size too small.
 - **NX_HTTP_NOT_FOUND** (0xE6) Specified query not found
 - **NX_HTTP_NO_QUERY_PARSED** (0xF2) No query in Client request
 - NX_PTR_ERROR (0x07) Invalid pointer input
 - NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```c
/* Assuming we are in the application's request notify callback
   routine, get the first query of the HTTP Client request. */
status =  nx_http_server_query_get(request_packet_ptr, 0, query_destination,
				30);

/* If status equals NX_SUCCESS, the NULL-terminated first query can be found
   in "query_destination." */
```

## nx_http_server_start

Start the HTTP Server

### Prototype

```c
UINT nx_http_server_start(NX_HTTP_SERVER *http_server_ptr);
```

### Description

This service starts the previously create HTTP Server instance.

### Input Parameters

 - *http_server_ptr*: Pointer to HTTP Server instance.

### Return Values

 - **NX_SUCCESS** (0x00) Successful Server start
 - NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Initialization, Threads

### Example

```c
/* Start the HTTP Server instance "my_server."  */
status =  nx_http_server_start(&my_server);

/* If status equals NX_SUCCESS, the HTTP Server has been started. */
```

## nx_http_server_stop

Stop the HTTP Server

### Prototype

```c
UINT nx_http_server_stop(NX_HTTP_SERVER *http_server_ptr);
```

### Description

This service stops the previously create HTTP Server instance. This routine should be called prior to deleting an HTTP Server instance.

### Input Parameters

 - *http_server_ptr*: Pointer to HTTP Server instance.

### Return Values

 - **NX_SUCCESS** (0x00) Successful Server stop
 - NX_PTR_ERROR (0x07) Invalid pointer input
 - NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```c
/* Stop the HTTP Server instance "my_server."  */
status =  nx_http_server_stop(&my_server);

/* If status equals NX_SUCCESS, the HTTP Server has been stopped. */
```

## nx_http_server_type_get

Extract file type from Client HTTP request

### Prototype

```c
UINT nx_http_server_type_get(
    NX_HTTP_SERVER *http_server_ptr,
    CHAR *name,
    CHAR *http_type_string);
```

### Description

> **Note:** This service is deprecated. Users are encouraged to use the service *nx_http_server_type_retrieve*.

This service extracts the HTTP request type in the buffer http_type_string and its length in the return value from the input buffer name, usually the URL. If no MIME map is found, it defaults to the "text/plain" type. Otherwise it compares the extracted type against the HTTP Server default MIME maps for a match. The default MIME maps in NetX Duo HTTP Server are:

 - **html** text/html
 - **htm** text/html
 - **txt** text/plain
 - **gif** image/gif
 - **jpg** image/jpeg
 - **ico** image/x-icon

If supplied, it will also search a user defined set of additional MIME maps. See *nx_http_server_mime_maps_additional_set* for more details on user defined maps.

This service is deprecated. Developers are encouraged to migrate to *nx_http_server_type_get_extended*.

### Input Parameters

 - *http_server_ptr*: Pointer to HTTP Server instance
 - *name Pointer*: to buffer to search
 - *http_type_string*: (Pointer to extracted HTML type)

### Return Values

 - **Length of string in bytes** Non zero value is success. Zero indicates error.

### Allowed From

Application

### Example

```c
/* my_server is a previously created HTTP server, which starts accepting client
   requests when nx_http_server_start is called*/

CHAR temp_string[20];
UINT string_length;

/* Extract the HTTP type. */
    string_length =  nx_http_server_type_get(&my_server_ptr,
				my_server.nx_http_server_request_resource, temp_string);

/* If string_length is non zero, the HTTP string is extracted. */
```

## nx_http_server_type_get_extended

Extract file type from Client HTTP request

### Prototype

```c
UINT nx_http_server_type_get_extended(
    NX_HTTP_SERVER *http_server_ptr,
    CHAR *name, CHAR *http_type_string,
    UINT http_type_string_max_size);
```

### Description

This service extracts the HTTP request type in the buffer *http_type_string* and its length in the return value from the input buffer *name*, usually the URL. If no MIME map is found, it defaults to the "text/plain" type. Otherwise it compares the extracted type against the HTTP Server default MIME maps for a match. The default MIME maps in NetX Duo HTTP Server are:

 - **html** text/html
 - **htm** text/html
 - **txt** text/plain
 - **gif** image/gif
 - **jpg** image/jpeg
 - **ico** image/x-icon

If supplied, it will also search a user defined set of additional MIME maps. See *nx_http_server_mime_maps_additional_set* for more details on user defined maps.

This service replaces *nx_http_server_type_get*. This version supplies additional length information.

### Input Parameters

 - *http_server_ptr*: Pointer to HTTP Server instance
 - *name*: Pointer to buffer to search
 - *name_length*: Length of buffer to search
 - *http_type_string*: (Pointer to extracted HTML type)
 - *http_type_string_max_size*: Size of the http_type_string buffer

### Return Values

 - **Length of string in bytes** Non zero value is success. Zero indicates error.

### Allowed From

Application

### Example

```c
/* my_server is a previously created HTTP server, which starts accepting client
   requests when nx_http_server_start is called*/

CHAR temp_string[20];
UINT string_length;

/* Get the length of request resource. */
	if (_nx_utility_string_length_check(
					my_server.nx_http_server_request_resource, &resource_length,
                    sizeof(my_server.nx_http_server_request_resource) - 1))
           {
               return;
           }

/* Extract the HTTP type. */
    string_length =  nx_http_server_type_get_extended(&my_server,
			   my_server.nx_http_server_request_resource, resource_length,
			   temp_string, sizeof(temp_string));

/* If string_length is non zero, the HTTP string is extracted. */
```

For a more detailed example, see the description for [*nx_http_server_callback_generate_response_header*](#nx_http_server_callback_generate_response_header).

## nx_http_server_digest_authenticate_notify_set

Set digest authenticate callback function

### Prototype

```c
UINT nx_http_server_digest_authenticate_notify_set(
    NX_HTTP_SERVER *http_server_ptr,
    UINT (*digest_authenticate_callback)(
        NX_HTTP_SERVER *server_ptr,
        CHAR *name_ptr,
        CHAR *realm_ptr,
        CHAR *password_ptr,
        CHAR *method,
        CHAR *authorization_uri,
        CHAR *authorization_nc,
        CHAR *authorization_cnonce));
```

### Description

This service sets the callback invoked when digest authenticate is performed.

### Input Parameters

 - *http_server_ptr*: Pointer to HTTP Server instance
 - *digest_authenticate_callback*: Pointer to digest authenticate callback

### Return Values

 - **NX_SUCCESS** (0x00) Successfully set the callback
 - NX_PTR_ERROR (0x07) Invalid pointer input
 - NX_NOT_SUPPORTED (0x4B) Digest authenticate not enabled

### Allowed From

Application

### Example

```c
UINT digest_authenticate_callback(NX_HTTP_SERVER *server_ptr, CHAR *name_ptr,
                                  CHAR *realm_ptr, CHAR *password_ptr, CHAR *method,
                                  CHAR *authorization_uri, CHAR *authorization_nc,
                                  CHAR *authorization_cnonce)
{
    return(NX_SUCCESS);
}

NX_HTTP_SERVER my_server;

/* After the HTTP server is created by calling nx_http_server_create, and before
   starting HTTP services when nx_http_server_start is called, set the digest authenticate callback: */

status =  nx_http_server_digest_authenticate_notify_set (&my_server,
    digest_authenticate_callback);

/* If status equals NX_SUCCESS, the digest_authenticate_callback function
   will be called when the HTTP server performs digest authenticate. */
```

## nx_http_server_authentication_check_set

Set authentication checking callback function

### Prototype

```c
UINT nx_http_server_authentication_check_set(
    NX_HTTP_SERVER *http_server_ptr,
    UINT (*authentication_check_extended)(
        NX_HTTP_SERVER *server_ptr,
        UINT request_type,
        CHAR *resource,
        CHAR **name,
        UINT *name_length,
        CHAR **password,
        UINT *password_length,
        CHAR **realm,
        UINT *realm_length));
```

### Description

This service sets the callback function of authentication checking.

### Input Parameters

 - *http_server_ptr*: Pointer to HTTP Server instance
 - *authentication_check_extended*: Pointer to application's authentication checking

### Return Values

 - **NX_SUCCESS** (0x00) Successfully set the callback
 - NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Application

### Example

```c
static UINT  authentication_check_extended(NX_HTTP_SERVER *server_ptr,
                                           UINT request_type, CHAR *resource,
                                           CHAR **name, UINT *name_length,
                                           CHAR **password, UINT *password_length,
                                           CHAR **realm, UINT *realm_length)
{

    /* Just use a simple name, password, and realm for all
       requests and resources. */
    *name =     "name";
    *password = "password";
    *realm =    "NetX Duo HTTP demo";
    *name_length = 4;
    *password_length = 8;
    *realm_length = 18;

    /* Request basic authentication. */
    return(NX_HTTP_BASIC_AUTHENTICATE);
}

NX_HTTP_SERVER my_server;

/* After the HTTP server is created by calling nx_http_server_create, and before
   starting HTTP services when nx_http_server_start is called, set the
   authentication checking callback: */

nx_http_server_authentication_check_set (&my_server,
										 authentication_check_extended);
```

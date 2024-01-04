---
title: Chapter 3 - Description of HTTP services
description: This chapter contains a description of all NetX Duo Web HTTP services (listed below) in alphabetical order.
---

# Chapter 3 - Description of HTTP services

This chapter contains a description of all NetX Duo Web HTTP services (listed below) in alphabetical order.

> **Note:** In the "Return Values" section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## HTTP and HTTPS Client API

nx_web_http_client_connect

Open a plaintext socket to an HTTP server for custom requests

### Prototype

```C
UINT nx_web_http_client_connect(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS *server_ip,
    UINT server_port,
    ULONG wait_option);
```

### Description

This method is for **plaintext** HTTP.

This service opens a plaintext TCP socket to an HTTP server but does not send any requests. Requests are created with n*x_web_http_client_request_initialize* and sent using *nx_web_http_client_request_send*. Custom HTTP headers may be added to the request using *nx_web_http_client_request_header_add*.

Use of this service enables an application to add any number of custom headers to the request. **This allows for customized HTTP requests intended for specific applications.**

> **Note:** The nx_web_http_client_*_start methods are provided for convenience (e.g. *nx_web_http_client_get_start*) and handle the request generation and socket connection. You can use those services instead of *nx_web_http_client_connect* and the related routines if you do not need custom HTTP headers in your requests.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *server_ip*: IP address of remote HTTP server.
- *server_port*: Port on remote HTTP server (e.g. 80 for HTTP).
- *wait_option*: Defines how long the service will wait for underlying network operations. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful connection of TCP socket.
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_WEB_HTTP_NOT_READY (0x3000A) Another request is already in progress.

### Allowed From

Threads

### Example

```C
NXD_ADDRESS server_ip_addr;

/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

nx_web_http_client_connect(&my_client, &server_ip_address,
    NX_WEB_HTTP_SERVER_PORT, NX_WAIT_FOREVER);

/* Create a new GET request on the HTTP client instance. */
nx_web_http_client_request_initialize(&my_client,
    NX_WEB_HTTP_METHOD_GET,
    "https://192.168.1.150/test.txt ", "host.com",
    0, /* Used by PUT and POST only */
    NX_FALSE,
    NX_NULL, /* username */
    NX_NULL, /* password */
    NX_WAIT_FOREVER);

/* Add a custom header to the GET request we just created. */
status = nx_web_http_client_request_header_add(&my_client, "Server", 6,
    "NetX Web HTTPS Server", 21, NX_WAIT_FOREVER);

/* Start the GET operation to get a response from the HTTPS server. */
status = nx_web_http_client_request_send(&my_client, 1000);

/* At this point, we need to handle the response from the server by repeatedly
    calling *nx_web_http_client_response_body_get* until the entire response is retrieved. */

get_status = NX_SUCCESS;

while(get_status != NX_WEB_HTTP_GET_DONE)
{
    get_status = nx_web_http_client_response_body_get(&my_client, &receive_packet,
        NX_WAIT_FOREVER);
    /* Process response packets… */
}
```

## nx_web_http_client_create

Create an HTTP Client Instance

### Prototype

```C
UINT nx_web_http_client_create(
    NX_WEB_HTTP_CLIENT *client_ptr,
    CHAR *client_name, NX_IP *ip_ptr, 
    NX_PACKET_POOL *pool_ptr,
    ULONG window_size);
```

### Description

This service creates an HTTP Client instance on the specified IP instance. The Client instance can be used for either HTTP or HTTPS. See the services *nx_web_http_client_connect* and *nx_web_http_client_secure_connect* for more information on starting an HTTP or HTTPS instance. Also refer to the API for *nx_web_http_client_get_**, *nx_web_http_client_put_**, *nx_web_http_client_post_** for simple invocations of GET, PUT, and POST methods.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *client_name*: Name of HTTP Client instance.
- *ip_ptr*: Pointer to IP instance.
- *pool_ptr*: Pointer to default packet pool. Note that the packets in this pool must have a payload large enough to handle the complete response header. This is defined by *NX_WEB_HTTP_CLIENT_MIN_PACKET_SIZE* in *nx_web_http_client.h*.
- *window_size*: Size of the Client's TCP socket receive window.

### Return Values

- **NX_SUCCESS** (0x00) Successful HTTP Client create
- NX_PTR_ERROR (0x16) Invalid HTTP, ip_ptr, or packet pool pointer
- NX_WEB_HTTP_POOL_ERROR (0x30009) Invalid payload size in packet pool

### Allowed From

Initialization, Threads

### Example

```C
/* Create the HTTP Client instance "my_client" on "ip_0". */
status = nx_web_http_client_create(&my_client, "my client", &ip_0, &pool_0, 8192);

/* If status is NX_SUCCESS an HTTP Client instance was successfully
    created. */
```

## nx_web_http_client_delete

Delete an HTTP Client Instance

### Prototype

```C
UINT nx_web_http_client_delete(NX_WEB_HTTP_CLIENT *client_ptr);
```

### Description

This service deletes a previously created HTTP Client instance.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.

### Return Values

- **NX_SUCCESS** (0x00) Successful HTTP Client delete
- NX_PTR_ERROR (0x16) Invalid HTTP pointer
- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```C
/* Delete the HTTP Client instance "my_client." */
status = nx_web_http_client_delete(&my_client);

/* If status is NX_SUCCESS an HTTP Client instance was successfully
    deleted. */
```

## nx_web_http_client_delete_start

Start a plaintext HTTP DELETE request

### Prototype

```C
UINT nx_web_http_client_delete_start(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port, CHAR *resource,
    CHAR *host, CHAR *username,
    CHAR *password,
    ULONG wait_option);
```

### Description

This method is for **plaintext** HTTP.

This service attempts to send a DELETE request for the resource specified by "resource" pointer on the previously created HTTP Client instance. If this routine returns NX_SUCCESS, the application can then call *nx_web_http_client_response_body_get* to retrieve the server's response.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring GET requests.

This API is deprecated. Developers are encouraged to use *nx_web_http_client_delete_start_extended*.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: Port on remote HTTP Server.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *resource*: Pointer to URL string for requested resource.
- *username*: Pointer to optional user name for authentication.
- *password*: Pointer to optional password for authentication.
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent HTTP Client DELETE request message
- **NX_WEB_HTTP_ERROR** (0x30000) Internal HTTP Client error
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- **NX_WEB_HTTP_FAILED** (0x30002) HTTP Client error communicating with the HTTP Server.
- **NX_WEB_HTTP_AUTHENTICATION_ERROR** (0x3000B) Invalid name and/or password.
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

/* Start the DELETE operation on the HTTP Client "my_client." */
status = nx_web_http_client_delete_start(&my_client, &server_ip_address,
    NX_WEB_HTTP_SERVER_PORT, "/TEST.HTM",
    "host.com", "myname", "mypassword",
    1000);

/* If status is NX_SUCCESS, the DELETE request for TEST.HTM is started and is so
    far successful. The client must now call *nx_web_http_client_response_body_get*
    to retrieve the response from the server. */
```

## nx_web_http_client_delete_start_extended

Start a plaintext HTTP DELETE request

### Prototype

```C
UINT nx_web_http_client_delete_start_extended(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port,
    CHAR *resource,
    UINT resource_length,
    CHAR *host,
    UINT host_length,
    CHAR *username,
    UINT username_length,
    CHAR *password,
    UINT password_length,
    ULONG wait_option);
```

### Description

This method is for **plaintext** HTTP.

This service attempts to send a DELETE request for the resource specified by "resource" pointer on the previously created HTTP Client instance. If this routine returns NX_SUCCESS, the application can then call *nx_web_http_client_response_body_get* to retrieve the server's response.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring GET requests.

The strings of resource, host, username and password must be NULL-terminated, and length of each string matches the length specified in the argument list.

This service replaces *nx_web_http_client_delete_start* . This version requires callers to supply length information to the function.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: Port on remote HTTP Server.
- *resource*: Pointer to URL string for requested resource.
- *resource_length*: String length of requested resource.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *host_length*: String length of host.
- *username*: Pointer to optional user name for authentication.
- *username_length*: String length of user name for authentication.
- *password*: Pointer to optional password for authentication.
- *password_length*: String length of password for authentication.
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value*: (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER*: (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent HTTP Client DELETE request message
- **NX_WEB_HTTP_ERROR** (0x30000) Internal HTTP Client error
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- **NX_WEB_HTTP_FAILED** (0x30002) HTTP Client error communicating with the HTTP Server.
- **NX_WEB_HTTP_AUTHENTICATION_ERROR** (0x3000B) Invalid name and/or password.
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

/* Start the DELETE operation on the HTTP Client "my_client." */
status = nx_web_http_client_delete_start_extended(&my_client,
    &server_ip_address,
    NX_WEB_HTTP_SERVER_PORT, "/TEST.HTM",
    sizeof("/TEST.HTM") – 1,
    "host.com", sizeof("host.com") – 1,
    "myname", sizeof("myname") – 1,
    "mypassword", sizeof("mypassword") – 1,
    1000);

/* If status is NX_SUCCESS, the DELETE request for TEST.HTM is started and is so
    far successful. The client must now call *nx_web_http_client_response_body_get*
    to retrieve the response from the server. */
```

## nx_web_http_client_delete_secure_start

Start an encrypted HTTPS DELETE request

### Prototype

```C
UINT nx_web_http_client_delete_secure_start(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port,
    CHAR *resource,
    CHAR *host,
    CHAR *username,
    CHAR *password,
    UINT (*tls_setup)(
        NX_WEB_HTTP_CLIENT *client_ptr,
        NX_SECURE_TLS_SESSION *tls_session),
    ULONG wait_option);
```

### Description

This method is for **TLS-secured** HTTPS.

This service attempts to send a DELETE request for the resource specified by "resource" pointer on the previously created HTTP Client instance. If this routine returns NX_SUCCESS, the application can then call *nx_web_http_client_response_body_get* to retrieve the server's response.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring GET requests.

This service is deprecated. Developers are encouraged to use *nx_web_http_client_delete_secure_start_extended*.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: Port on remote HTTP Server.
- *resource*: Pointer to URL string for requested resource. resource must be NULL-terminated.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *username*: Pointer to optional user name for authentication.
- *password*: Pointer to optional password for authentication.
- *tls_setup*: Callback used to setup TLS configuration. The application defines this callback to initialize TLS cryptography and credentials (e.g. certificates).
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent HTTP Client DELETE request message
- **NX_WEB_HTTP_ERROR** (0x30000) Internal HTTP Client error
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- **NX_WEB_HTTP_FAILED** (0x30002) HTTP Client error communicating with the HTTP Server.
- **NX_WEB_HTTP_AUTHENTICATION_ERROR** (0x3000B) Invalid name and/or password.
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

/* Start the DELETE operation on the HTTP Client "my_client." */
status = nx_web_http_client_delete_secure_start(&my_client, &server_ip_addr,
    NX_WEB_HTTPS_SERVER_PORT, "/TEST.HTM",
    "host.com", "myname", "mypassword", my_tls_setup_function, 1000);

/* If status is NX_SUCCESS, the DELETE request for TEST.HTM is started and is so
    far successful. The client must now call *nx_web_http_client_response_body_get*
    to retrieve the server's response. */
```

## nx_web_http_client_delete_secure_start_extended

Start an encrypted HTTPS DELETE request

### Prototype

```C
UINT nx_web_http_client_delete_secure_start_extended(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port,
    CHAR *resource,
    UINT resource_length,
    CHAR *host,
    UINT host_length,
    CHAR *username,
    UINT username_length,
    CHAR *password,
    UINT password_length,
    UINT (*tls_setup)(
        NX_WEB_HTTP_CLIENT *client_ptr,
        NX_SECURE_TLS_SESSION *tls_session),
    ULONG wait_option);
```

### Description

This method is for **TLS-secured** HTTPS.

This service attempts to send a DELETE request for the resource specified by "resource" pointer on the previously created HTTP Client instance. If this routine returns NX_SUCCESS, the application can then call *nx_web_http_client_response_body_get* to retrieve the server's response.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring GET requests.

The strings of resource, host, username and password must be NULL-terminated and length of each string matches the length specified in the argument list.

This service replaces *nx_web_http_client_delete_secure_start*. This version requires callers to supply length information to the function.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: Port on remote HTTP Server.
- *resource*: Pointer to URL string for requested resource. resource must be NULL-terminated.
- *resource_length*: String length of requested resource.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *host_length*: String length of host.
- *username*: Pointer to optional user name for authentication.
- *username_length*: String length of user name for authentication.
- *password*: Pointer to optional password for authentication.
- *password_length*: String length of password for authentication.
- *tls_setup*: Callback used to setup TLS configuration. The application defines this callback to initialize TLS cryptography and credentials (e.g. certificates).
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent HTTP Client DELETE request message
- **NX_WEB_HTTP_ERROR** (0x30000) Internal HTTP Client error
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- **NX_WEB_HTTP_FAILED** (0x30002) HTTP Client error communicating with the HTTP Server.
- **NX_WEB_HTTP_AUTHENTICATION_ERROR** (0x3000B) Invalid name and/or password.
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service.


### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

/* Start the DELETE operation on the HTTP Client "my_client." */
status = nx_web_http_client_delete_secure_start_extended(&my_client,
    &server_ip_addr,
    NX_WEB_HTTPS_SERVER_PORT, "/TEST.HTM", sizeof("/TEST.HTM") – 1,
    "host.com", sizeof("host.com") – 1,
    "myname", sizeof("myname") – 1,
    "mypassword", sizeof("mypassword") – 1, my_tls_setup_function, 1000);

/* If status is NX_SUCCESS, the DELETE request for TEST.HTM is started and is so
    far successful. The client must now call *nx_web_http_client_response_body_get*
    to retrieve the server's response. */
```

## nx_web_http_client_get_start

Start a plaintext HTTP GET request

### Prototype

```C
UINT nx_web_http_client_get_start(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port,
    CHAR *resource,
    CHAR *host, CHAR *username,
    CHAR *password,
    ULONG wait_option);
```

### Description

This method is for **plaintext** HTTP.

This service attempts to GET the resource specified by "resource" pointer on the previously created HTTP Client instance. If this routine returns NX_SUCCESS, the application can then make multiple calls to *nx_web_http_client_response_body_get* to retrieve packets of data corresponding to the requested resource content.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring GET requests.

This service is deprecated. Developers are encouraged to use *nx_web_http_client_get_start_extended*.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: Port on remote HTTP Server.
- *resource*: Pointer to URL string for requested resource.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *username*: Pointer to optional user name for authentication.
- *password*: Pointer to optional password for authentication.
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent HTTP Client GET start message
- **NX_WEB_HTTP_ERROR** (0x30000) Internal HTTP Client error
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- **NX_WEB_HTTP_FAILED** (0x30002) HTTP Client error communicating with the HTTP Server.
- **NX_WEB_HTTP_AUTHENTICATION_ERROR** (0x3000B) Invalid name and/or password.
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

/* Start the GET operation on the HTTP Client "my_client." */
status = nx_web_http_client_get_start(&my_client, &server_ip_addr,
    NX_WEB_HTTP_SERVER_PORT, "/TEST.HTM",
    "host.com", "myname", "mypassword",
    1000);

/* If status is NX_SUCCESS, the GET request for TEST.HTM is started and is so
    far successful. The client must now call *nx_web_http_client_response_body_get*
    multiple times to retrieve the content associated with TEST.HTM. */
```

## nx_web_http_client_get_start_extended

Start a plaintext HTTP GET request

### Prototype

```C
UINT nx_web_http_client_get_start_extended(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port,
    CHAR *resource,
    UINT resource_length,
    CHAR *host,
    UINT host_length,
    CHAR *username,
    UINT username_length,
    CHAR *password,
    UINT password_length,
    ULONG wait_option);
```

### Description

This method is for **plaintext** HTTP.

This service attempts to GET the resource specified by "resource" pointer on the previously created HTTP Client instance. If this routine returns NX_SUCCESS, the application can then make multiple calls to *nx_web_http_client_response_body_get* to retrieve packets of data corresponding to the requested resource content.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring GET requests.

The strings of resource, host, username and password must be NULL-terminated, and length of each string matches the length specified in the argument list.

This service replaces *nx_web_http_client_get_start*. This version requires callers to supply length information to the function.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: Port on remote HTTP Server.
- *resource*: Pointer to URL string for requested resource.
- *resource_length*: String length of requested resource.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *host_length*: String length of host.
- *username*: Pointer to optional user name for authentication.
- *username_length*: String length of user name for authentication.
- *password*: Pointer to optional password for authentication.
- *password_length*: String length of password for authentication.
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent HTTP Client GET start message
- **NX_WEB_HTTP_ERROR** (0x30000) Internal HTTP Client error
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- **NX_WEB_HTTP_FAILED** (0x30002) HTTP Client error communicating with the HTTP Server.
- **NX_WEB_HTTP_AUTHENTICATION_ERROR** (0x3000B) Invalid name and/or password.
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

/* Start the GET operation on the HTTP Client "my_client." */
status = nx_web_http_client_get_start_extended(&my_client, &server_ip_addr,
    NX_WEB_HTTP_SERVER_PORT, "/TEST.HTM",
    sizeof("/TEST.HTM") – 1,
    "host.com", sizeof("host.com") – 1,
    "myname", sizeof("myname") – 1,
    "mypassword", sizeof("mypassword") – 1,
    1000);

/* If status is NX_SUCCESS, the GET request for TEST.HTM is started and is so
    far successful. The client must now call *nx_web_http_client_response_body_get*
    multiple times to retrieve the content associated with TEST.HTM. */
```

## nx_web_http_client_get_secure_start

Start an encrypted HTTPS GET request

### Prototype

```C
UINT nx_web_http_client_get_secure_start(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port,
    CHAR *resource,
    CHAR *host,
    CHAR *username,
    CHAR *password,
    UINT (*tls_setup)(
        NX_WEB_HTTP_CLIENT *client_ptr,
        NX_SECURE_TLS_SESSION *tls_session),
    ULONG wait_option);
```

### Description

This method is for **TLS-secured** HTTPS.

This service attempts to GET the resource specified by "resource" pointer on the previously created HTTP Client instance. If this routine returns NX_SUCCESS, the application can then make multiple calls to *nx_web_http_client_response_body_get* to retrieve packets of data corresponding to the requested resource content.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring GET requests.

This service is deprecated. Developers are encouraged to use *nx_web_http_client_get_secure_start_extended*.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: Port on remote HTTP Server.
- *resource*: Pointer to URL string for requested resource.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *username*: Pointer to optional user name for authentication.
- *password*: Pointer to optional password for authentication.
- *tls_setup*: Callback used to setup TLS configuration. The application defines this callback to initialize TLS cryptography and credentials (e.g. certificates).
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent HTTP Client GET start message
- **NX_WEB_HTTP_ERROR** (0x30000) Internal HTTP Client error
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- **NX_WEB_HTTP_FAILED** (0x30002) HTTP Client error communicating with the HTTP Server.
- **NX_WEB_HTTP_AUTHENTICATION_ERROR** (0x3000B) Invalid name and/or password.
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

/* Start the GET operation on the HTTP Client "my_client." */
status = nx_web_http_client_get_secure_start(&my_client, &server_ip_addr,
    NX_WEB_HTTPS_SERVER_PORT, "/TEST.HTM",
    "host.com", "myname", "mypassword",
    my_tls_setup_function, 1000);

/* If status is NX_SUCCESS, the GET request for TEST.HTM is started
    and is so far successful. The client must now call
    *nx_web_http_client_response_body_get* multiple times to
    retrieve the content associated with TEST.HTM. */
```

## nx_web_http_client_get_secure_start_extended

Start an encrypted HTTPS GET request

### Prototype

```C
UINT nx_web_http_client_get_secure_start_extended(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port,
    CHAR *resource,
    UINT resource_length,
    CHAR *host,
    UINT host_length,
    CHAR *username,
    UINT username_length,
    CHAR *password,
    UINT password_length,
    UINT (*tls_setup)(
        NX_WEB_HTTP_CLIENT *client_ptr,
        NX_SECURE_TLS_SESSION *tls_session),
    ULONG wait_option);
```

### Description

This method is for **TLS-secured** HTTPS.

This service attempts to GET the resource specified by "resource" pointer on the previously created HTTP Client instance. If this routine returns NX_SUCCESS, the application can then make multiple calls to *nx_web_http_client_response_body_get* to retrieve packets of data corresponding to the requested resource content.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring GET requests.

The strings of resource, host, username and password must be NULL-terminated, and length of each string matches the length specified in the argument list.

This service replaces *nx_web_http_client_secure_start*. This version requires callers to supply length information to the function.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: Port on remote HTTP Server.
- *resource*: Pointer to URL string for requested resource.
- *resource_length*: String length of requested resource.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *host_length*: String length of host.
- *username*: Pointer to optional user name for authentication.
- *username_length*: String length of user name for authentication.
- *password*: Pointer to optional password for authentication.
- *password_length*: String length of password for authentication.
- *tls_setup*: Callback used to setup TLS configuration. The application defines this callback to initialize TLS cryptography and credentials (e.g. certificates).
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent HTTP Client GET start message
- **NX_WEB_HTTP_ERROR** (0x30000) Internal HTTP Client error
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- **NX_WEB_HTTP_FAILED** (0x30002) HTTP Client error communicating with the HTTP Server.
- **NX_WEB_HTTP_AUTHENTICATION_ERROR** (0x3000B) Invalid name and/or password.
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

/* Start the GET operation on the HTTP Client "my_client." */
status = nx_web_http_client_get_secure_start_extended(&my_client,
    &server_ip_addr, NX_WEB_HTTPS_SERVER_PORT,
    "/TEST.HTM", sizeof("/TEST.HTM") – 1,
    "host.com", sizeof("host.com") – 1,
    "myname", sizeof("myname") – 1,
    "mypassword", sizeof("mypassword") – 1,
    my_tls_setup_function, 1000);

/* If status is NX_SUCCESS, the GET request for TEST.HTM
    is started and is so far successful. The client must now call
    *nx_web_http_client_response_body_get* multiple times to retrieve
    the content associated with TEST.HTM. */
```

## nx_web_http_client_head_start

Start a plaintext HTTP HEAD request

### Prototype

```C
UINT nx_web_http_client_head_start(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port,
    CHAR *resource,
    CHAR *host, CHAR *username,
    CHAR *password,
    ULONG wait_option);
```

### Description

This method is for **plaintext** HTTP.

This service attempts to retrieve the HEAD metadata for the resource specified by "resource" pointer on the previously created HTTP Client instance. If this routine returns NX_SUCCESS, the application can then call *nx_web_http_client_response_body_get* to retrieve the response.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring GET requests.

This service is deprecated. Developers are encouraged to use *nx_web_http_client_head_start_extended*.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: Port on remote HTTP Server.
- *resource*: Pointer to URL string for requested resource.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *username*: Pointer to optional user name for authentication.
- *password*: Pointer to optional password for authentication.
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent HTTP Client HEAD request message
- **NX_WEB_HTTP_ERROR** (0x30000) Internal HTTP Client error
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- **NX_WEB_HTTP_FAILED** (0x30002) HTTP Client error communicating with the HTTP Server.
- **NX_WEB_HTTP_AUTHENTICATION_ERROR** (0x3000B) Invalid name and/or password.
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

/* Start the HEAD operation on the HTTP Client "my_client." */
status = nx_web_http_client_head_start(&my_client, &server_ip_addr,
    NX_WEB_HTTP_SERVER_PORT, "/TEST.HTM",
    "host.com", "myname", "mypassword",
    1000);

/* If status is NX_SUCCESS, the HEAD request for TEST.HTM is started and is so
    far successful. The client must now call *nx_web_http_client_response_body_get*
    to retrieve the response from the server. */
```

## nx_web_http_client_head_start_extended

Start a plaintext HTTP HEAD request

### Prototype

```C
UINT nx_web_http_client_head_start_extended(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port, CHAR *resource,
    UINT resource_length,
    CHAR *host,
    UINT host_length,
    CHAR *username,
    UINT username_length,
    CHAR *password,
    UINT password_length,
    ULONG wait_option);
```

### Description

This method is for **plaintext** HTTP.

This service attempts to retrieve the HEAD metadata for the resource specified by "resource" pointer on the previously created HTTP Client instance. If this routine returns NX_SUCCESS, the application can then call *nx_web_http_client_response_body_get* to retrieve the response.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring GET requests.

The strings of resource, host, username and password must be NULL-terminated and length of each string matches the length specified in the argument list.

This service replaces *nx_web_http_client_head_start*. This version requires callers to supply length information to the function.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: Port on remote HTTP Server.
- *resource*: Pointer to URL string for requested resource.
- *resource_length*: String length of requested resource.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *host_length*: String length of host.
- *username*: Pointer to optional user name for authentication.
- *username_length*: String length of user name for authentication.
- *password*: Pointer to optional password for authentication.
- *password_length*: String length of password for authentication.
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent HTTP Client HEAD request message
- **NX_WEB_HTTP_ERROR** (0x30000) Internal HTTP Client error
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- **NX_WEB_HTTP_FAILED** (0x30002) HTTP Client error communicating with the HTTP Server.
- **NX_WEB_HTTP_AUTHENTICATION_ERROR** (0x3000B) Invalid name and/or password.
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

/* Start the HEAD operation on the HTTP Client "my_client." */
status = nx_web_http_client_head_start_extended(&my_client,
    &server_ip_addr, NX_WEB_HTTPS_SERVER_PORT,
    "/TEST.HTM", sizeof("/TEST.HTM") – 1,
    "host.com", sizeof("host.com") – 1,
    "myname", sizeof("myname") – 1,
    "mypassword", sizeof("mypassword") – 1,
    1000);

/* If status is NX_SUCCESS, the HEAD request for TEST.HTM is started and is so
    far successful. The client must now call *nx_web_http_client_response_body_get*
    to retrieve the response from the server. */
```

## nx_web_http_client_head_secure_start

Start an encrypted HTTPS HEAD request

### Prototype

```C
UINT nx_web_http_client_head_secure_start(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port,
    CHAR *resource,
    CHAR *host,
    CHAR *username,
    CHAR *password,
    UINT (*tls_setup)(
        NX_WEB_HTTP_CLIENT *client_ptr,
        NX_SECURE_TLS_SESSION *tls_session),
    ULONG wait_option);
```

### Description

This method is for **TLS-secured** HTTPS.

This service attempts to retrieve the HEAD metadata for the resource specified by "resource" pointer on the previously created HTTP Client instance. If this routine returns NX_SUCCESS, the application can then call *nx_web_http_client_response_body_get* to retrieve the server's response corresponding to the requested resource content.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring GET requests.

This service is deprecated. Developers are encouraged to use *nx_web_http_client_head_secure_start_extended*.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: Port on remote HTTP Server.
- *resource*: Pointer to URL string for requested resource.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *username*: Pointer to optional user name for authentication.
- *password*: Pointer to optional password for authentication.
- *tls_setup*: Callback used to setup TLS configuration. The application defines this callback to initialize TLS cryptography and credentials (e.g. certificates).
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent HTTP Client HEAD request message
- **NX_WEB_HTTP_ERROR** (0x30000) Internal HTTP Client error
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- **NX_WEB_HTTP_FAILED** (0x30002) HTTP Client error communicating with the HTTP Server.
- **NX_WEB_HTTP_AUTHENTICATION_ERROR** (0x3000B) Invalid name and/or password.
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

/* Start the HEAD operation on the HTTP Client "my_client." */
status = nx_web_http_client_head_secure_start(&my_client, &server_ip_addr,
    NX_WEB_HTTPS_SERVER_PORT, "/TEST.HTM",
    "host.com", "myname", "mypassword",
    my_tls_setup_function, 1000);

/* If status is NX_SUCCESS, the HEAD request for TEST.HTM is started and is so
    far successful. The client must now call *nx_web_http_client_response_body_get*
    to retrieve the server's response. */
```

## nx_web_http_client_head_secure_start_extended

Start an encrypted HTTPS HEAD request

### Prototype

```C
UINT nx_web_http_client_head_secure_start(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port,
    CHAR *resource,
    UINT resource_length,
    CHAR *host,
    UINT host_length,
    CHAR *username,
    UINT username_length,
    CHAR *password,
    UINT password_length,
    UINT (*tls_setup)(
        NX_WEB_HTTP_CLIENT *client_ptr,
        NX_SECURE_TLS_SESSION *tls_session),
    ULONG wait_option);
```

### Description

This method is for **TLS-secured** HTTPS.

This service attempts to retrieve the HEAD metadata for the resource specified by "resource" pointer on the previously created HTTP Client instance. If this routine returns NX_SUCCESS, the application can then call *nx_web_http_client_response_body_get* to retrieve the server's response corresponding to the requested resource content.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring GET requests.

The strings of resource, host, username and password must be NULL-terminated and length of each string matches the length specified in the argument list.

This service replaces *nx_web_http_client_head_secure_start*. This version requires callers to supply length information to the function.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: Port on remote HTTP Server.
- *resource*: Pointer to URL string for requested resource.
- *resource_length*: String length of requested resource.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *host_length*: String length of host.
- *username*: Pointer to optional user name for authentication.
- *username_length*: String length of user name for authentication.
- *password*: Pointer to optional password for authentication.
- *password_length*: String length of password for authentication.
- *tls_setup*: Callback used to setup TLS configuration. The application defines this callback to initialize TLS cryptography and credentials (e.g. certificates).
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent HTTP Client HEAD request message
- **NX_WEB_HTTP_ERROR** (0x30000) Internal HTTP Client error
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- **NX_WEB_HTTP_FAILED** (0x30002) HTTP Client error communicating with the HTTP Server.
- **NX_WEB_HTTP_AUTHENTICATION_ERROR** (0x3000B) Invalid name and/or password.
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

/* Start the HEAD operation on the HTTP Client "my_client." */
status = nx_web_http_client_head_secure_start_extended(&my_client,
    &server_ip_addr, NX_WEB_HTTPS_SERVER_PORT,
    "/TEST.HTM", sizeof("/TEST.HTM") – 1,
    "host.com", sizeof("host.com") – 1,
    "myname", sizeof("myname") – 1,
    "mypassword", sizeof("mypassword") – 1,
    my_tls_setup_function, 1000);

/* If status is NX_SUCCESS, the HEAD request for TEST.HTM is started and is so
    far successful. The client must now call *nx_web_http_client_response_body_get*
    to retrieve the server's response. */
```

## nx_web_http_client_request_packet_allocate

Allocate a HTTP(S) packet

### Prototype

```C
UINT nx_web_http_client_request_packet_allocate(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NX_PACKET **packet_ptr,
    ULONG wait_option);
```

### Description

This service attempts to allocates a packet for Client HTTP(S).

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *packet_ptr*: Pointer to allocated packet.
- *wait_option*: Defines the wait time in ticks if there are no packets available in the packet pool. The wait options are defined as follows:
  - **NX_NO_WAIT** (0x00000000)
  - **NX_WAIT_FOREVER** (0xFFFFFFFF)
  - **timeout in ticks** (0x00000001 through 0xFFFFFFFE)

### Return Values

- **NX_SUCCESS** (0x00) Successful packet allocate
- **NX_NO_PACKET** (0x01) No packet available
- **NX_WAIT_ABORTED** (0x1A) Requested suspension was aborted by a call to *tx_thread_wait_abort*.
- **NX_INVALID_PARAMETERS** (0x4D) Packet size cannot support protocol.
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Allocate a packet for HTTP(S) Client and suspend for a maximum of 5 timer
    ticks if the pool is empty. */
status = nx_web_http_client_request_packet_allocate(&my_client, &packet_ptr, 5);

/* If status is NX_SUCCESS, the newly allocated packet pointer is found in the
    variable packet_ptr. */
```

## nx_web_http_client_post_start

Start an HTTP POST request

### Prototype

```C
UINT nx_web_http_client_post_start(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port,
    CHAR *resource,
    CHAR *host,
    CHAR *username,
    CHAR *password,
    ULONG total_bytes,
    ULONG wait_option);
```

### Description

This method is for **plaintext** HTTP.

This service attempts to send a POST request with the specified resource to the HTTP Server at the supplied IP address and port. If this routine is successful, the application code should make successive calls to the *nx_web_http_client_put_packet* routine to send the resource contents to the HTTP Server.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring PUT requests.

This service is deprecated. Developers are encouraged to use *nx_web_http_client_post_start_extended*.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: TCP port on the remote HTTP Server.
- *resource*: Pointer to URL string for resource to send to Server.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *username*: Pointer to optional user name for authentication.
- *password*: Pointer to optional password for authentication.
- *total_bytes*: Total bytes of resource being sent. Note that the combined length of all packets sent via subsequent calls to *nx_web_http_client_put_packet* must equal this value.
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent POST request
- **NX_WEB_HTTP_USERNAME_TOO_LONG** (0x30012) Username too large for buffer
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_SIZE_ERROR (0x09) Invalid total size of resource
- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

/* Start an HTTP POST to post the 20-byte resource "/TEST.HTM" to the HTTP Server
    at IP address 1.2.3.5. */
status = nx_web_http_client_post_start(&my_client, &server_ip_addr,
    NX_WEB_HTTP_SERVER_PORT, "/TEST.HTM",
    "host.com", "myname", "mypassword", 20,
    NX_WAIT_FOREVER);

/* If status is NX_SUCCESS, the POST operation for TEST.HTM has successfully been
    started. */
```

## nx_web_http_client_post_start_extended

Start an HTTP POST request

### Prototype

```C
UINT nx_web_http_client_post_start_extended(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port,
    CHAR *resource,
    UINT resource_length,
    CHAR *host,
    UINT host_length,
    CHAR *username,
    UINT username_length,
    CHAR *password,
    UINT password_length,
    ULONG total_bytes,
    ULONG wait_option);
```

### Description

This method is for **plaintext** HTTP.

This service attempts to send a POST request with the specified resource to the HTTP Server at the supplied IP address and port. If this routine is successful, the application code should make successive calls to the *nx_web_http_client_put_packet* routine to send the resource contents to the HTTP Server.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring PUT requests.

The strings of resource, host, username and password must be NULL-terminated and length of each string matches the length specified in the argument list.

This service replaces *nx_web_http_client_post_start* . This version requires callers to supply length information to the function.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: TCP port on the remote HTTP Server.
- *resource*: Pointer to URL string for requested resource.
- *resource_length*: String length of requested resource.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *host_length*: String length of host.
- *username*: Pointer to optional user name for authentication.
- *username_length*: String length of user name for authentication.
- *password*: Pointer to optional password for authentication.
- *password_length*: String length of password for authentication.
- *total_bytes*: Total bytes of resource being sent. Note that the combined length of all packets sent via subsequent calls to *nx_web_http_client_put_packet* must equal this value.
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent POST request
- **NX_WEB_HTTP_USERNAME_TOO_LONG** (0x30012) Username too large for buffer
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_SIZE_ERROR (0x09) Invalid total size of resource
- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

/* Start an HTTP POST to post the 20-byte resource "/TEST.HTM" to the HTTP Server
    at IP address 1.2.3.5. */
status = nx_web_http_client_post_start_extended(&my_client,
    &server_ip_addr, NX_WEB_HTTPS_SERVER_PORT,
    "/TEST.HTM", sizeof("/TEST.HTM") – 1,
    "host.com", sizeof("host.com") – 1,
    "myname", sizeof("myname") – 1,
    "mypassword", sizeof("mypassword") – 1, 20,
    NX_WAIT_FOREVER);

/* If status is NX_SUCCESS, the POST operation for TEST.HTM has successfully been
    started. */
```

## nx_web_http_client_post_secure_start

Start an encrypted HTTPS POST request

### Prototype

```C
UINT nx_web_http_client_post_secure_start(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port,
    CHAR *resource,
    CHAR *host,
    CHAR *username,
    CHAR *password,
    ULONG total_bytes,
    UINT (*tls_setup)(
        NX_WEB_HTTP_CLIENT *client_ptr,
        NX_SECURE_TLS_SESSION *tls_session),
    ULONG wait_option);
```

### Description

This method is for **TLS-secured** HTTPS.

This service attempts to send a POST request with the specified resource to the HTTPS Server at the supplied IP address and port. If this routine is successful, the application code should make successive calls to the *nx_web_http_client_put_packet* routine to send the resource contents to the HTTP Server.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring PUT requests.

This service is deprecated. Developers are encouraged to use *nx_web_http_client_post_secure_start_extended*.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: TCP port on the remote HTTP Server.
- *resource*: Pointer to URL string for resource to send to Server.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *username*: Pointer to optional user name for authentication.
- *password*: Pointer to optional password for authentication.
- *total_bytes*: Total bytes of resource being sent. Note that the combined length of all packets sent via subsequent calls to *nx_web_http_client_put_packet* must equal this value.
- *tls_setup*: Callback used to setup TLS configuration. The application defines this callback to initialize TLS cryptography and credentials (e.g. certificates).
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent POST request
- **NX_WEB_HTTP_USERNAME_TOO_LONG** (0x30012) Username too large for buffer
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_SIZE_ERROR (0x09) Invalid total size of resource
- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```C
/* Start an HTTP PUT to place the 20-byte resource "/TEST.HTM" on the HTTPS Server
    at IP address 1.2.3.5. */

/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

status = nx_web_http_client_put_secure_start(&my_client, &server_ip_addr,
    NX_WEB_HTTPS_SERVER_PORT, "/TEST.HTM",
    "host.com", "myname", "mypassword", 20,
    tls_setup, NX_WAIT_FOREVER);

/* If status is NX_SUCCESS, the POST operation for TEST.HTM has successfully been
    started. */
```

## nx_web_http_client_post_secure_start_extended

Start an encrypted HTTPS POST request

### Prototype

```C
UINT nx_web_http_client_post_secure_start_extended(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port, CHAR *resource,
    UINT resource_length,
    CHAR *host,
    UINT host_length,
    CHAR *username,
    UINT username_length,
    CHAR *password,
    UINT password_length,
    ULONG total_bytes,
    UINT (*tls_setup)(
        NX_WEB_HTTP_CLIENT *client_ptr,
        NX_SECURE_TLS_SESSION *tls_session),
    ULONG wait_option);
```

### Description

This method is for **TLS-secured** HTTPS.

This service attempts to send a POST request with the specified resource to the HTTPS Server at the supplied IP address and port. If this routine is successful, the application code should make successive calls to the *nx_web_http_client_put_packet* routine to send the resource contents to the HTTP Server.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring PUT requests.

The strings of resource, host, username and password must be NULL-terminated and length of each string matches the length specified in the argument list.

This service replaces *nx_web_http_client_post_secure_start* . This version requires callers to supply length information to the function.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: TCP port on the remote HTTP Server.
- *resource*: Pointer to URL string for requested resource.
- *resource_length*: String length of requested resource.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *host_length*: String length of host.
- *username*: Pointer to optional user name for authentication.
- *username_length*: String length of user name for authentication.
- *password*: Pointer to optional password for authentication.
- *password_length*: String length of password for authentication.
- *total_bytes*: Total bytes of resource being sent. Note that the combined length of all packets sent via subsequent calls to *nx_web_http_client_put_packet* must equal this value.
- *tls_setup*: Callback used to setup TLS configuration. The application defines this callback to initialize TLS cryptography and credentials (e.g. certificates).
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent POST request
- **NX_WEB_HTTP_USERNAME_TOO_LONG** (0x30012) Username too large for buffer
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_SIZE_ERROR (0x09) Invalid total size of resource
- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```C
/* Start an HTTP PUT to place the 20-byte resource "/TEST.HTM" on the HTTPS Server
    at IP address 1.2.3.5. */

/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

status = nx_web_http_client_put_secure_start_extended(&my_client,
    &server_ip_addr, NX_WEB_HTTPS_SERVER_PORT,
    "/TEST.HTM", sizeof("/TEST.HTM") – 1,
    "host.com", sizeof("host.com") – 1,
    "myname", sizeof("myname") – 1,
    "mypassword", sizeof("mypassword") – 1, 20,
    tls_setup, NX_WAIT_FOREVER);

/* If status is NX_SUCCESS, the POST operation for TEST.HTM has successfully been
    started. */
```

## nx_web_http_client_put_start

Start an HTTP PUT request

### Prototype

```C
UINT nx_web_http_client_put_start(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port,
    CHAR *resource,
    CHAR *host,
    CHAR *username,
    CHAR *password,
    ULONG total_bytes,
    ULONG wait_option);
```

### Description

This method is for **plaintext** HTTP.

This service attempts to send a PUT request with the specified resource to the HTTP Server at the supplied IP address and port. If this routine is successful, the application code should make successive calls to the *nx_web_http_client_put_packet* routine to send the resource contents to the HTTP Server.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring PUT requests.

This service is deprecated. Developers are encouraged to use *nx_web_http_client_put_start_extended*.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: TCP port on the remote HTTP Server.
- *resource*: Pointer to URL string for resource to send to Server.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *username*: Pointer to optional user name for authentication.
- *password*: Pointer to optional password for authentication.
- *total_bytes*: Total bytes of resource being sent. Note that the combined length of all packets sent via subsequent calls to *nx_web_http_client_put_packet* must equal this value.
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent PUT request
- **NX_WEB_HTTP_USERNAME_TOO_LONG** (0x30012) Username too large for buffer
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_SIZE_ERROR (0x09) Invalid total size of resource
- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```C
/* Start an HTTP PUT to place the 20-byte resource "/TEST.HTM" on the HTTP Server
    at IP address 1.2.3.5. */

/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

status = nx_web_http_client_put_start(&my_client, &server_ip_addr,
    NX_WEB_HTTP_SERVER_PORT, "/TEST.HTM",
    "host.com", "myname", "mypassword", 20,
    NX_WAIT_FOREVER);

/* If status is NX_SUCCESS, the PUT operation for TEST.HTM has successfully been
    started. */
```

## nx_web_http_client_put_start_extended

Start an HTTP PUT request

### Prototype

```C
UINT nx_web_http_client_put_start(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port,
    CHAR *resource,
    UINT resource_length,
    CHAR *host,
    UINT host_length,
    CHAR *username,
    UINT username_length,
    CHAR *password,
    UINT password_length,
    ULONG total_bytes,
    ULONG wait_option);
```

### Description

This method is for **plaintext** HTTP.

This service attempts to send a PUT request with the specified resource to the HTTP Server at the supplied IP address and port. If this routine is successful, the application code should make successive calls to the *nx_web_http_client_put_packet* routine to send the resource contents to the HTTP Server.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring PUT requests.

The strings of resource, host, username and password must be NULL-terminated and length of each string matches the length specified in the argument list.

This service replaces *nx_web_http_client_put_start* . This version requires callers to supply length information to the function.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: TCP port on the remote HTTP Server.
- *resource*: Pointer to URL string for requested resource.
- *resource_length*: String length of requested resource.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *host_length*: String length of host.
- *username*: Pointer to optional user name for authentication.
- *username_length*: String length of user name for authentication.
- *password*: Pointer to optional password for authentication.
- *password_length*: String length of password for authentication.
- *total_bytes*: Total bytes of resource being sent. Note that the combined length of all packets sent via subsequent calls to *nx_web_http_client_put_packet* must equal this value.
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent PUT request
- **NX_WEB_HTTP_USERNAME_TOO_LONG** (0x30012) Username too large for buffer
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_SIZE_ERROR (0x09) Invalid total size of resource
- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```c
/* Start an HTTP PUT to place the 20-byte resource "/TEST.HTM" on the HTTP Server
    at IP address 1.2.3.5. */

/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

status = nx_web_http_client_put_start_extended(&my_client,
    &server_ip_addr, NX_WEB_HTTPS_SERVER_PORT,
    "/TEST.HTM", sizeof("/TEST.HTM") – 1,
    "host.com", sizeof("host.com") – 1,
    "myname", sizeof("myname") – 1,
    "mypassword", sizeof("mypassword") – 1, 20,
    NX_WAIT_FOREVER);

/* If status is NX_SUCCESS, the PUT operation for TEST.HTM has successfully been
    started. */
```

## nx_web_http_client_put_secure_start

Start an encrypted HTTPS PUT request

### Prototype

```C
UINT nx_web_http_client_put_secure_start(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port,
    CHAR *resource,
    CHAR *host,
    CHAR *username,
    CHAR *password,
    ULONG total_bytes,
    UINT (*tls_setup)(
        NX_WEB_HTTP_CLIENT *client_ptr,
        NX_SECURE_TLS_SESSION *tls_session),
    ULONG wait_option);
```

### Description

This method is for **TLS-secured** HTTPS.

This service attempts to send a PUT request with the specified resource to the HTTPS Server at the supplied IP address and port. If this routine is successful, the application code should make successive calls to the *nx_web_http_client_put_packet* routine to send the resource contents to the HTTP Server.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring PUT requests.

This service is deprecated. Developers are encouraged to use *nx_web_http_client_put_secure_start_extended*.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: TCP port on the remote HTTP Server.
- *resource*: Pointer to URL string for resource to send to Server.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *username*: Pointer to optional user name for authentication.
- *password*: Pointer to optional password for authentication.
- *total_bytes*: Total bytes of resource being sent. Note that the combined length of all packets sent via subsequent calls to *nx_web_http_client_put_packet* must equal this value.
- *tls_setup*: Callback used to setup TLS configuration. The application defines this callback to initialize TLS cryptography and credentials (e.g. certificates).
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent PUT request
- **NX_WEB_HTTP_USERNAME_TOO_LONG** (0x30012) Username too large for buffer
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_SIZE_ERROR (0x09) Invalid total size of resource
- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```C
/* Start an HTTP PUT to place the 20-byte resource "/TEST.HTM" on the HTTPS Server
    at IP address 1.2.3.5. */

/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

status = nx_web_http_client_put_secure_start(&my_client, &server_ip_addr,
    NX_WEB_HTTPS_SERVER_PORT, "/TEST.HTM",
    "host.com", "myname", "mypassword", 20,
    tls_setup, NX_WAIT_FOREVER);

/* If status is NX_SUCCESS, the PUT operation for TEST.HTM has successfully been
    started. */
```

## nx_web_http_client_put_secure_start_extended

Start an encrypted HTTPS PUT request

### Prototype

```C
UINT nx_web_http_client_put_secure_start(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS ip_address,
    UINT server_port,
    CHAR *resource,
    UINT resource_length,
    CHAR *host,
    UINT host_length,
    CHAR *username,
    UINT username_length, 
    CHAR *password,
    UINT password_length,
    ULONG total_bytes,
    UINT (*tls_setup)(
        NX_WEB_HTTP_CLIENT *client_ptr,
        NX_SECURE_TLS_SESSION *tls_session),
    ULONG wait_option);
```

### Description

This method is for **TLS-secured** HTTPS.

This service attempts to send a PUT request with the specified resource to the HTTPS Server at the supplied IP address and port. If this routine is successful, the application code should make successive calls to the *nx_web_http_client_put_packet* routine to send the resource contents to the HTTP Server.

Note that the resource string can refer to a local file e.g. "/index.htm" or it can refer to another URL e.g. `http://abc.website.com/index.htm` if the HTTP Server indicates it supports referring PUT requests.

The strings of resource, host, username and password must be NULL-terminated and length of each string matches the length specified in the argument list.

This service replaces *nx_web_http_client_put_secure_start*. This version requires callers to supply length information to the function.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *ip_address*: IP address of the HTTP Server.
- *server_port*: TCP port on the remote HTTP Server.
- *resource*: Pointer to URL string for requested resource.
- *resource_length*: String length of requested resource.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *host_length*: String length of host.
- *username*: Pointer to optional user name for authentication.
- *username_length*: String length of user name for authentication.
- *password*: Pointer to optional password for authentication.
- *password_length*: String length of password for authentication.
- *total_bytes*: Total bytes of resource being sent. Note that the combined length of all packets sent via subsequent calls to *nx_web_http_client_put_packet* must equal this value.
- *tls_setup*: Callback used to setup TLS configuration. The application defines this callback to initialize TLS cryptography and credentials (e.g. certificates).
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent PUT request
- **NX_WEB_HTTP_USERNAME_TOO_LONG** (0x30012) Username too large for buffer
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_SIZE_ERROR (0x09) Invalid total size of resource
- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```C
/* Start an HTTP PUT to place the 20-byte resource "/TEST.HTM" on the HTTPS Server
    at IP address 1.2.3.5. */

/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

status = nx_web_http_client_put_secure_start_extended(&my_client,
    &server_ip_addr, NX_WEB_HTTPS_SERVER_PORT,
    "/TEST.HTM", sizeof("/TEST.HTM") – 1,
    "host.com", sizeof("host.com") – 1,
    "myname", sizeof("myname") – 1,
    "mypassword", sizeof("mypassword") – 1, 20,
    tls_setup, NX_WAIT_FOREVER);

/* If status is NX_SUCCESS, the PUT operation for TEST.HTM has successfully been
    started. */
```

## nx_web_http_client_put_packet

Send next resource data packet

### Prototype

```C
UINT nx_web_http_client_put_packet(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NX_PACKET *packet_ptr,
    ULONG wait_option);
```

### Description

This service attempts to send the next packet of resource content to the HTTP Server for both PUT and POST operations. Note that this routine should be called repetitively until the combined length of the packets sent equals the "total_bytes" specified in the previous *nx_web_http_client_put_start* or *nx_web_http_client_post_start* call (or their corresponding secure versions).

This service also checks for a response from the server in case there was a problem establishing the HTTP (or HTTPS) connection.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *packet_ptr*: Pointer to next content of the resource to being sent to the HTTP Server.
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent HTTP Client packet.
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not ready
- **NX_WEB_HTTP_REQUEST_UNSUCCESSFUL_CODE** (0x3000E) Received Server error code**
- **NX_WEB_HTTP_BAD_PACKET_LENGTH** (0x3000D) Invalid packet length
- **NX_WEB_HTTP_AUTHENTICATION_ERROR** (0x3000B) Invalid name and/or Password
- **NX_WEB_HTTP_INCOMPLETE_PUT_ERROR** (0x3000F) Server responds before PUT is complete
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_INVALID_PACKET (0x12) Packet too small for TCP header
- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```C
/* Send a 20-byte packet representing the content of the resource
    "/TEST.HTM" to the HTTP Server. */

status = nx_web_http_client_put_packet(&client_ptr, packet_ptr, NX_WAIT_FOREVER);

/* If status is NX_SUCCESS, the 20-byte resource contents of TEST.HTM has
    successfully been sent. */
```

## nx_web_http_client_request_chunked_set

Set chunked transfer for HTTP(S) request

### Prototype

```C
UINT nx_web_http_client_request_chunked_set(
    NX_WEB_HTTP_CLIENT *client_ptr,
    UINT chunk_size,
    NX_PACKET *packet_ptr);
```

### Description

This service uses chunked transfer coding to send a custom HTTP(S) request to the server specified in the *nx_web_http_client_connect* or *nx_web_http_client_secure_connect* call which has previously established the socket connection to the remote host.

> **Note:** If the application uses chunked transfer coding to send a request data packet, it must call this service after calling *nx_web_http_client_request_packet_allocate*, and before call *nx_web_http_client_request_packet_send* .

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *chunk_size*: Size of the chunk-data in octets.
- *packet_ptr*: HTTP(S) request data packet pointer.

### Return Values

- **NX_SUCCESS** (0x00) Successfully set chunked.
- NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTPS server. */

nx_web_http_client_secure_connect(&my_client, IP_ADDRESS(1,2,3,5),
    NX_WEB_HTTPS_SERVER_PORT, tls_setup_callback,
    NX_WAIT_FOREVER);

/* Create a PUT request on the HTTP client instance. */
nx_web_http_client_request_initialize(&my_client,
    NX_WEB_HTTP_METHOD_PUT,
    "https://192.168.1.150/test.txt ", "host.com",
    0, /* Used by PUT and POST only */
    NX_TRUE,
    NX_NULL, /* username */
    NX_NULL, /* password */
    NX_WAIT_FOREVER);

/* Start the PUT operation. */
nx_web_http_client_request_send(&my_client, 1000);

/* Create a new data packet request on the HTTP(S) client instance. */
nx_web_http_client_request_packet_allocate(&my_client,
    &my_packet,
    NX_WAIT_FOREVER);

/* Set the chunked transfer. */
status = nx_web_http_client_request_chunked_set(&my_client, 128, my_packet);

/* At this point, user can fill the data into my_packet. */
nx_packet_data_append(my_packet, data_ptr, data_size,
    packet_pool, NX_WAIT_FOREVER);

/* Send data packet request to server. */
nx_web_http_client_request_packet_send(&my_client, my_packet,
    0, NX_WAIT_FOREVER);
```

## nx_web_http_client_request_header_add

Add a custom header to a custom HTTP request

### Prototype

```C
UINT nx_web_http_client_request_header_add(
    NX_WEB_HTTP_CLIENT *client_ptr,
    CHAR *field_name,
    UINT name_length,
    CHAR *field_value,
    UINT value_length,
    UINT wait_option);
```

### Description

This service adds a custom header (in the form of a field name and value) to a custom HTTP request created with n*x_web_http_client_request_initialize*.

Use of this service enables an application to add any number of custom headers to the request. **This allows for customized HTTP requests intended for specific applications.**

> **Note:** The nx_web_http_client_\*_start methods are provided for convenience. These functions all use this routine internally (along with *nx_web_http_client_request_initialize)* to create and send HTTP requests.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *field_name*: Field name string (e.g. "Content-Type").
- *name_length*: Length of field name string in bytes.
- *field_value*: Field value string (e.g. "application/octet-stream").
- *value_length*: Length of value string in bytes.
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful addition of header to request.
- NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTPS server. */
nx_web_http_client_secure_connect(&my_client, IP_ADDRESS(1,2,3,5),
    NX_WEB_HTTPS_SERVER_PORT, tls_setup_callback,
    NX_WAIT_FOREVER);

/* Create a new GET request on the HTTP client instance. */

nx_web_http_client_request_initialize(&my_client,
    NX_WEB_HTTP_METHOD_GET,
    "https://192.168.1.150/test.txt ", "host.com",
    0, /* Used by PUT and POST only */
    NX_FALSE,
    NX_NULL, /* username */
    NX_NULL, /* password */
    NX_WAIT_FOREVER);

/* Add a custom header to the GET request we just created. */
status = nx_web_http_client_request_header_add(&my_client, "Server", 6,
    "NetX Web HTTPS Server", 21, NX_WAIT_FOREVER);

/* Start the GET operation to get a response from the HTTPS server. */
status = nx_web_http_client_request_send(&my_client, 1000);

/* At this point, we need to handle the response from the server
    by repeatedly calling *nx_web_http_client_response_body_get*
    until the entire response is retrieved. */

get_status = NX_SUCCESS;

while(get_status != NX_WEB_HTTP_GET_DONE)
{
    get_status = nx_web_http_client_response_body_get(&my_client, &receive_packet,
        NX_WAIT_FOREVER);

    /* Process response packets… */
}
```

## nx_web_http_client_request_initialize

Initialize a custom HTTP request

### Prototype

```C
UINT nx_web_http_client_request_initialize(
    NX_WEB_HTTP_CLIENT *client_ptr,
    UINT method, CHAR *resource,
    CHAR *host,
    UINT input_size,
    UINT transfer_encoding_trunked,
    CHAR *username,
    CHAR *password,
    UINT wait_option);
```

### Description

This service creates a custom HTTP request and associates it with the HTTP Client instance. Unlike the simpler *nx_web_http_client_get_start* (along with the methods for PUT, POST, and the associated secure versions of those API), the custom request is not sent until the *nx_web_http_client_request_send* service is called.

Use of this service enables an application to add any number of custom headers to the request using the ***nx_web_http_client_request_header_add*** service. This allows for customized HTTP requests intended for specific applications.

> **Note:** The nx_web_http_client_\*_start methods are provided for convenience. These functions all use this routine internally (along with *nx_web_http_client_request_send)* to create and send HTTP requests.

This service is deprecated. Developers are encouraged to use *nx_web_http_client_request_initialize_extended*.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *method*: The HTTP request method to use. The options are defined as follows:
  - **NX_WEB_HTTP_METHOD_NONE (0x0)**
  - **NX_WEB_HTTP_METHOD_GET (0x1)**
  - **NX_WEB_HTTP_METHOD_PUT (0x2)**
  - **NX_WEB_HTTP_METHOD_POST (0x3)**
  - **NX_WEB_HTTP_METHOD_DELETE (0x4)**
  - **NX_WEB_HTTP_METHOD_HEAD (0x5)**
- *resource*: Name of resource being transferred.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *input_size*: Size of input data for PUT and POST. Pass 0 for other operations.
- *transfer_encoding_trunked*: Reserved parameter for future trunked transfer support.
- *username*: Username for protected resources.
- *password*: Password for protected resources.
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful initialization of request.
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_WEB_HTTP_METHOD_ERROR (0x30014) Some required information was missing (e.g. input_size for PUT or POST).

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTPS server. */
nx_web_http_client_secure_connect(&my_client, IP_ADDRESS(1,2,3,5),
    NX_WEB_HTTPS_SERVER_PORT, tls_setup_callback,
    NX_WAIT_FOREVER);

/* Create a new GET request on the HTTP client instance. */
nx_web_http_client_request_initialize(&my_client,
    NX_WEB_HTTP_METHOD_GET,
    "test.txt ", "host.com",
    0, /* Used by PUT and POST only */
    NX_FALSE,
    NX_NULL, /* username */
    NX_NULL, /* password */
    NX_WAIT_FOREVER);

/* Start the GET operation to get a response from the HTTPS server. */

status = nx_web_http_client_request_send(&my_client, 1000);

/* At this point, we need to handle the response from the server by repeatedly
    calling *nx_web_http_client_response_body_get* until the entire response is retrieved. */
get_status = NX_SUCCESS;

while(get_status != NX_WEB_HTTP_GET_DONE)
{
    get_status = nx_web_http_client_response_body_get(&my_client, &receive_packet,
        NX_WAIT_FOREVER);

    /* Process response packets… */
}
```

## nx_web_http_client_request_initialize_extended

Initialize a custom HTTP request

### Prototype

```C
UINT nx_web_http_client_request_initialize_extended(
    NX_WEB_HTTP_CLIENT *client_ptr,
    UINT method,
    CHAR *resource,
    UINT resource_length,
    CHAR *host,
    UINT host_length,
    UINT input_size,
    UINT transfer_encoding_trunked,
    CHAR *username,
    UINT username_length,
    CHAR *password,
    UINT password_length,
    UINT wait_option);
```

### Description

This service creates a custom HTTP request and associates it with the HTTP Client instance. Unlike the simpler *nx_web_http_client_get_start* (along with the methods for PUT, POST, and the associated secure versions of those API), the custom request is not sent until the *nx_web_http_client_request_send* service is called.

Use of this service enables an application to add any number of custom headers to the request using the ***nx_web_http_client_request_header_add*** service. This allows for customized HTTP requests intended for specific applications.

> **Note:** The nx_web_http_client_\*_start methods are provided for convenience. These functions all use this routine internally (along with *nx_web_http_client_request_send)* to create and send HTTP requests.

The strings of resource, host, username and password must be NULL-terminated and length of each string matches the length specified in the argument list.

This service replaces *nx_web_http_client_request_initialize*. This version requires callers to supply length information to the function.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *method*: The HTTP request method to use. The options are defined as follows:
  - **NX_WEB_HTTP_METHOD_NONE (0x0)**
  - **NX_WEB_HTTP_METHOD_GET (0x1)**
  - **NX_WEB_HTTP_METHOD_PUT (0x2)**
  - **NX_WEB_HTTP_METHOD_POST (0x3)**
  - **NX_WEB_HTTP_METHOD_DELETE (0x4)**
  - **NX_WEB_HTTP_METHOD_HEAD (0x5)**
- *resource*: Name of resource being transferred.
- *resource_length*: String length of requested resource.
- *host*: Null-terminated string of the server's domain name. This string is transmitted in the HTTP Host header field. The host string cannot be NULL.
- *host_length*: String length of host.
- *input_size*: Size of input data for PUT and POST. Pass 0 for other operations.
- *transfer_encoding_trunked*: Reserved parameter for future trunked transfer support.
- *username*: Pointer to optional user name for authentication.
- *username_length*: String length of user name for authentication.
- *password*: Pointer to optional password for authentication.
- *password_length*: String length of password for authentication.
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful initialization of request.
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_WEB_HTTP_METHOD_ERROR (0x30014) Some required information was missing (e.g. input_size for PUT or POST).

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTPS server. */
nx_web_http_client_secure_connect(&my_client, IP_ADDRESS(1,2,3,5),
    NX_WEB_HTTPS_SERVER_PORT, tls_setup_callback,
    NX_WAIT_FOREVER);

/* Create a new GET request on the HTTP client instance. */
nx_web_http_client_request_initialize_extended(&my_client,
    NX_WEB_HTTP_METHOD_GET,
    "test.txt ", sizeof("test.txt ") – 1,
    "host.com", sizeof("host.com") – 1,
    0, /* Used by PUT and POST only */
    NX_FALSE,
    NX_NULL, /* username */
    0,
    NX_NULL, /* password */
    0,
    NX_WAIT_FOREVER);

/* Start the GET operation to get a response from the HTTPS server. */
status = nx_web_http_client_request_send(&my_client, 1000);


/* At this point, we need to handle the response from the server by repeatedly
    calling *nx_web_http_client_response_body_get* until the entire response is retrieved. */
get_status = NX_SUCCESS;
while(get_status != NX_WEB_HTTP_GET_DONE)
{
    get_status = nx_web_http_client_response_body_get(&my_client, &receive_packet,
        NX_WAIT_FOREVER);

    /* Process response packets… */
}
```

## nx_web_http_client_request_packet_send

Send HTTP(S) request data packet to remote server

### Prototype

```C
UINT nx_web_http_client_request_packet_send(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NX_PACKET *packet_ptr,
    UINT more_date,
    ULONG wait_option);
```

### Description

This service sends a custom HTTP(S) request data packet created with *nx_web_http_client_request_packet_allocate*  to the server specified in the *nx_web_http_client_connect* or *nx_web_http_client_secure_connect(*) call which has previously established the socket connection to the remote host.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *packet_ptr*: HTTP(S) request data packet pointer.
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful send of request data packet.
- NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTPS server. */
nx_web_http_client_secure_connect(&my_client, IP_ADDRESS(1,2,3,5),
    NX_WEB_HTTPS_SERVER_PORT, tls_setup_callback,
    NX_WAIT_FOREVER);

/* Create a PUT request on the HTTP client instance. */
nx_web_http_client_request_initialize(&my_client,
    NX_WEB_HTTP_METHOD_PUT,
    "https://192.168.1.150/test.txt ", "host.com",
    128, /* Used by PUT and POST only */
    NX_FALSE,
    NX_NULL, /* username */
    NX_NULL, /* password */
    NX_WAIT_FOREVER);

/* Start the PUT operation. */
nx_web_http_client_request_send(&my_client, 1000);

/* Create a new data packet request on the HTTP(S) client instance. */
nx_web_http_client_request_packet_allocate(&my_client,
    &my_packet,
    NX_WAIT_FOREVER);

/* At this point, user can fill the data into my_packet. */
nx_packet_data_append(my_packet, data_ptr, data_size,
    packet_pool, NX_WAIT_FOREVER);

/* Send data packet request to server. */
status = nx_web_http_client_request_packet_send(&my_client,
    my_packet,
    0,
    NX_WAIT_FOREVER);
```

## nx_web_http_client_request_send

Send a custom HTTP request

### Prototype

```C
UINT nx_web_http_client_request_send(
    NX_WEB_HTTP_CLIENT *client_ptr,
    UINT wait_option);
```

### Description

This service sends a custom HTTP request created with *nx_web_http_client_request_initialize* to the server specified in the *nx_web_http_client_connect* or *nx_web_http_client_secure_connect* both of which have previously established the socket connection to the remote host.

Use of this service enables an application to add any number of custom headers to the request using the ***nx_web_http_client_request_header_add*** service. This allows for customized HTTP requests intended for specific applications.

> **Note:** The nx_web_http_client_\*_start methods are provided for convenience. These functions all use this routine internally (along with *nx_web_http_client_request_initialize)* to create and send HTTP requests.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful send of request.
- NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTPS server. */
nx_web_http_client_secure_connect(&my_client, IP_ADDRESS(1,2,3,5),
    NX_WEB_HTTPS_SERVER_PORT, tls_setup_callback,
    NX_WAIT_FOREVER);

/* Create a new GET request on the HTTP client instance. */
nx_web_http_client_request_initialize(&my_client,
    NX_WEB_HTTP_METHOD_GET,
    "https://192.168.1.150/test.txt ", "host.com",
    0, /* Used by PUT and POST only */
    NX_FALSE,
    NX_NULL, /* username */
    NX_NULL, /* password */
    NX_WAIT_FOREVER);

/* Start the GET operation to get a response from the HTTPS server. */
status = nx_web_http_client_request_send(&my_client, 1000);

/* At this point, we need to handle the response from the server by
    repeatedly calling *nx_web_http_client_response_body_get* until
    the entire response is retrieved. */

get_status = NX_SUCCESS;

while(get_status != NX_WEB_HTTP_GET_DONE)
{
    get_status = nx_web_http_client_response_body_get(&my_client, &receive_packet,
        NX_WAIT_FOREVER);

    /* Process response packets… */
}
```

## nx_web_http_client_response_body_get

Get next resource data packet

### Prototype

```C
UINT nx_web_http_client_response_body_get(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NX_PACKET **packet_ptr,
    ULONG wait_option);
```

### Description

This service retrieves the next packet of content of the resource requested by the previous request. Successive calls to this routine should be made until the return status of NX_WEB_HTTP_GET_DONE is received.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *packet_ptr*: Destination for packet pointer containing partial resource content.
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful get of HTTP Client packet.
- **NX_WEB_HTTP_GET_DONE** (0x3000C) HTTP Client get packet is done
- **NX_WEB_HTTP_NOT_READY** (0x3000A) HTTP Client not in get mode.
- **NX_WEB_HTTP_BAD_PACKET_LENGTH** (0x3000D) Invalid packet length
- **NX_WEB_HTTP_STATUS_CODE_CONTINUE** (0x3001A) HTTP status code 100 Continue
- **NX_WEB_HTTP_STATUS_CODE_SWITCHING_PROTOCOLS** (0x3001B) HTTP status code 101 Switching Protocols
- **NX_WEB_HTTP_STATUS_CODE_CREATED** (0x3001C) HTTP status code 201 Created
- **NX_WEB_HTTP_STATUS_CODE_ACCEPTED** (0x3001D) HTTP status code 202 Accepted
- **NX_WEB_HTTP_STATUS_CODE_NON_AUTH_INFO** (0x3001E) HTTP status code 203 Non-Authoritative Information
- **NX_WEB_HTTP_STATUS_CODE_NO_CONTENT** (0x3001F) HTTP status code 204 No Content
- **NX_WEB_HTTP_STATUS_CODE_RESET_CONTENT** (0x30020) HTTP status code 205 Reset Content
- **NX_WEB_HTTP_STATUS_CODE_PARTIAL_CONTENT** (0x30021) HTTP status code 206 Partial Content
- **NX_WEB_HTTP_STATUS_CODE_MULTIPLE_CHOICES** (0x30022) HTTP status code 300 Multiple Choices
- **NX_WEB_HTTP_STATUS_CODE_MOVED_PERMANENTLY** (0x30023) HTTP status code 301 Moved Permanently
- **NX_WEB_HTTP_STATUS_CODE_FOUND** (0x30024) HTTP status code 302 Found
- **NX_WEB_HTTP_STATUS_CODE_SEE_OTHER** (0x30025) HTTP status code 303 See Other
- **NX_WEB_HTTP_STATUS_CODE_NOT_MODIFIED** (0x30026) HTTP status code 304 Not Modified
- **NX_WEB_HTTP_STATUS_CODE_USE_PROXY** (0x30027) HTTP status code 305 Use Proxy
- **NX_WEB_HTTP_STATUS_CODE_TEMPORARY_REDIRECT** (0x30028) HTTP status code 307 Temporary Redirect
- **NX_WEB_HTTP_STATUS_CODE_BAD_REQUEST** (0x30029) HTTP status code 400 Bad Request
- **NX_WEB_HTTP_STATUS_CODE_UNAUTHORIZED** (0x3002A) HTTP status code 401 Unauthorized
- **NX_WEB_HTTP_STATUS_CODE_PAYMENT_REQUIRED** (0x3002B) HTTP status code 402 Payment Required
- **NX_WEB_HTTP_STATUS_CODE_FORBIDDEN** (0x3002C) HTTP status code 403 Forbidden
- **NX_WEB_HTTP_STATUS_CODE_NOT_FOUND** (0x3002D) HTTP status code 404 Not Found
- **NX_WEB_HTTP_STATUS_CODE_METHOD_NOT_ALLOWED** (0x3002E) HTTP status code 405 Method Not Allowed
- **NX_WEB_HTTP_STATUS_CODE_NOT_ACCEPTABLE** (0x3002F) HTTP status code 406 Not Acceptable
- **NX_WEB_HTTP_STATUS_CODE_PROXY_AUTH_REQUIRED** (0x30030) HTTP status code 407 Proxy Authentication Required
- **NX_WEB_HTTP_STATUS_CODE_REQUEST_TIMEOUT** (0x30031) HTTP status code 408 Request Time-out
- **NX_WEB_HTTP_STATUS_CODE_CONFLICT** (0x30032) HTTP status code 409 Conflict
- **NX_WEB_HTTP_STATUS_CODE_GONE** (0x30033) HTTP status code 410 Gone
- **NX_WEB_HTTP_STATUS_CODE_LENGTH_REQUIRED** (0x30034) HTTP status code 411 Length Required
- **NX_WEB_HTTP_STATUS_CODE_PRECONDITION_FAILED** (0x30035) HTTP status code 412 Precondition Failed
- **NX_WEB_HTTP_STATUS_CODE_ENTITY_TOO_LARGE** (0x30036) HTTP status code 413 Request Entity Too Large
- **NX_WEB_HTTP_STATUS_CODE_URL_TOO_LARGE** (0x30037) HTTP status code 414 Request URL Too Large
- **NX_WEB_HTTP_STATUS_CODE_UNSUPPORTED_MEDIA** (0x30038) HTTP status code 415 Unsupported Media Type
- **NX_WEB_HTTP_STATUS_CODE_RANGE_NOT_SATISFY** (0x30039) HTTP status code 416 Requested range not satisfiable
- **NX_WEB_HTTP_STATUS_CODE_EXPECTATION_FAILED** (0x3003A) HTTP status code 417 Expectation Failed
- **NX_WEB_HTTP_STATUS_CODE_INTERNAL_ERROR** (0x3003B) HTTP status code 500 Internal Server Error
- **NX_WEB_HTTP_STATUS_CODE_NOT_IMPLEMENTED** (0x3003C) HTTP status code 501 Not Implemented
- **NX_WEB_HTTP_STATUS_CODE_BAD_GATEWAY** (0x3003D) HTTP status code 502 Bad Gateway
- **NX_WEB_HTTP_STATUS_CODE_SERVICE_UNAVAILABLE** (0x3003E) HTTP status code 503 Service Unavailable
- **NX_WEB_HTTP_STATUS_CODE_GATEWAY_TIMEOUT** (0x3003F) HTTP status code 504 Gateway Time-out
- **NX_WEB_HTTP_STATUS_CODE_VERSION_ERROR** (0x30040) HTTP status code 505 HTTP Version not supported
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```C
/* Get the next packet of resource content on the HTTP Client "my_client."
    Note that the nx_web_http_client_get_start routine must have been called
    previously. */
status = nx_web_http_client_response_body_get(&my_client, &next_packet, 1000);

/* If status is NX_SUCCESS, the next packet of content is pointed to
    by "next_packet". */
```

## nx_web_http_client_response_header_callback_set

Set callback to invoke when processing HTTP headers

### Prototype

```C
UINT nx_web_http_client_response_header_callback_set(
    NX_WEB_HTTP_CLIENT *client_ptr,
    VOID (*callback_function)(
        NX_WEB_HTTP_CLIENT *client_ptr,
        CHAR *field_name,
        UINT field_name_length,
        CHAR *field_value,
        UINT field_value_length));
```

### Description

This service assigns a callback that will be invoked whenever NetX Duo Web HTTP Client processes an HTTP header in an incoming response from a remote HTTP server. The callback is invoked once for each header in the response as it is processed. The callback allows an HTTP Client application to access each of the headers in the HTTP server response to take any desired actions beyond the basic processing that NetX Duo Web HTTP Client does.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.

- *callback_function*: Callback invoked during response header processing. The callback is invoked with the field name and value as strings (and their lengths). For example, the header "Content-Length: 100" would cause the function to be invoked with "Content-Length" for *field_name* and the string "100" for *field_value*.

### Return Values

- **NX_SUCCESS** (0x00) Successful assignment of callback.
- NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```C
/* Setup a callback to print out header information as it is processed. */
VOID http_response_callback(NX_WEB_HTTP_CLIENT *client_ptr, CHAR *field_name,
    UINT field_name_length, CHAR *field_value,
    UINT field_value_length)
{
    CHAR name[100];
    CHAR value[100];
    memset(name, 0, sizeof(name));

    memset(value, 0, sizeof(value));

    strncpy(name, field_name, field_name_length);
    strncpy(value, field_value, field_value_length);

    printf("Received header: \n");
    printf("\tField name: %s (%d bytes)\n", name, field_name_length);
    printf("\tValue: %s (%d bytes)\n\n", value, field_value_length);
}

/* Assign the callback to the HTTP client instance. */
nx_web_http_client_response_header_callback_set(&my_client,
    http_response_callback);

/* Start a GET operation to get a response from the HTTP server. */
status = nx_web_http_client_get_start(&my_client, IP_ADDRESS(1,2,3,5),
    NX_WEB_HTTP_SERVER_PORT, "/TEST.HTM",
    "myname", "mypassword", 1000);

/* When the HTTP server returns a response to the GET request, NetX Web HTTP
    Client will invoke the http_response_callback function for each header
    processed in the HTTP response. */
```

## nx_web_http_client_secure_connect

Open a TLS session to an HTTPS server for custom requests

### Prototype

```C
UINT nx_web_http_client_secure_connect(
    NX_WEB_HTTP_CLIENT *client_ptr,
    NXD_ADDRESS *server_ip,
    UINT server_port,
    UINT (*tls_setup)(
        NX_WEB_HTTP_CLIENT *client_ptr,
        NX_SECURE_TLS_SESSION *tls),
    ULONG wait_option);
```

### Description

This method is for **TLS-secured** HTTPS.

This service opens a secured TLS session to an HTTPS server but does not send any requests. Requests are created with n*x_web_http_client_request_initialize* and sent using *nx_web_http_client_request_send*. Custom HTTP headers may be added to the request using *nx_web_http_client_request_header_add*.

Use of this service enables an application to add any number of custom headers to the request. **This allows for customized HTTP requests intended for specific applications.**

> **Note:** The nx_web_http_client_\*_start methods are provided for convenience. These functions all use this routine internally (along with *nx_web_http_client_request_initialize)* to create and send HTTP requests.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *server_ip*: IP address of remote HTTPS server.
- *server_port*: Port on remote HTTPS server (e.g. 443 for HTTPS).
- *tls_setup*: Callback used to setup TLS configuration. The application defines this callback to initialize TLS cryptography and credentials (e.g. certificates).
- *wait_option*: Defines how long the service will wait for the HTTP Client get start request. The wait options are defined as follows:
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.

### Return Values

- **NX_SUCCESS** (0x00) Successful connection of TLS session.
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_WEB_HTTP_NOT_READY (0x3000A) Another request is already in progress.

### Allowed From

Threads

### Example

```C
/* Connect to a remote HTTPS server. */
/* Connect to a remote HTTP server. */
server_ip_addr.nxd_ip_version = NX_IP_VERSION_V4;
server_ip_addr.nxd_ip_address.v4 = IP_ADDRESS(1,2,3,5);

nx_web_http_client_secure_connect(&my_client, &server_ip_addr,
    NX_WEB_HTTPS_SERVER_PORT, tls_setup_callback,
    NX_WAIT_FOREVER);

/* Create a new GET request on the HTTP client instance. */
nx_web_http_client_request_initialize(&my_client,
    NX_WEB_HTTP_METHOD_GET,
    "https://192.168.1.150/test.txt ", "host.com",
    0, /* Used by PUT and POST only */
    NX_FALSE,
    NX_NULL, /* username */
    NX_NULL, /* password */
    NX_WAIT_FOREVER);

/* Add a custom header to the GET request we just created. */
status = nx_web_http_client_request_header_add(&my_client, "Server", 6,
    "NetX Web HTTPS Server", 21, NX_WAIT_FOREVER);

/* Start the GET operation to get a response from the HTTPS server. */
status = nx_web_http_client_request_send(&my_client, 1000);

/* At this point, we need to handle the response from the server by repeatedly
    calling *nx_web_http_client_response_body_get* until the entire response is retrieved. */

get_status = NX_SUCCESS;

while(get_status != NX_WEB_HTTP_GET_DONE)
{
    get_status = nx_web_http_client_response_body_get(&my_client, &receive_packet,
        NX_WAIT_FOREVER);
    /* Process response packets… */
}
```

## HTTP and HTTPS Server API

## nx_web_http_server_cache_info_callback_set

Set the callback to retrieve URL max age and date

### Prototype

```C
UINT nx_web_http_server_cache_info_callback_set(
    NX_WEB_HTTP_SERVER *server_ptr,
    UINT (*cache_info_get)(CHAR *resource,
    UINT *max_age,
    NX_WEB_HTTP_SERVER_DATE *date));
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
- **NX_PTR_ERROR** (0x07) Invalid pointer input

### Allowed From

Initialization

### Example

```C
NX_WEB_HTTP_SERVER my_server;

UINT cache_info_get(CHAR *resource, UINT *max_age,
    NX_WEB_HTTP_SERVER_DATE *last_modified);

/* After my_server is created with nx_web_http_server_create and before the HTTP
    server is set by nx_web_http_server_start, set the cache info callback: */
status = nx_web_http_server_cache_info_callback_set(&my_server, cache_info_get);

/* If status is NX_SUCCESS, the callback was successfully sent. */
```

## nx_web_http_server_callback_data_send

Send data from callback function

### Prototype

```C
UINT nx_web_http_server_callback_data_send(
    NX_WEB_HTTP_SERVER *server_ptr,
    VOID *data_ptr, ULONG data_length);
```

### Description

This service sends the data in the supplied packet from the application's callback routine. This is typically used to send dynamic data associated with GET/POST requests. Note that if this function is used, the callback routine is responsible for sending the entire response in the proper format. In addition, the callback routine must return the status of NX_WEB_HTTP_CALLBACK_COMPLETED.

### Input Parameters

- *server_ptr*: Pointer to HTTP Server control block.
- *data_ptr*: Pointer to the data to send.
- *data_length*: Number of bytes to send.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent Server data
- **NX_PTR_ERROR** (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```C
UINT my_request_notify(NX_WEB_HTTP_SERVER *server_ptr, UINT request_type,
    CHAR *resource, NX_PACKET *packet_ptr)
{
    /* Look for the test resource! */
    if ((request_type == NX_WEB_HTTP_SERVER_GET_REQUEST) &&
        (strcmp(resource, "/test.htm") == 0))
    {
        /* Found it, override the GET processing by sending the resource
            contents directly. */
        nx_web_http_server_callback_data_send(server_ptr,
            "HTTP/1.0 200 \r\nContent-Length:" "103\r\nContent-Type: text/html\r\n\r\n",
            63);

        nx_web_http_server_callback_data_send(server_ptr, "<HTML>\r\n<HEAD><TITLE>NetX"
            "HTTP Test </TITLE></HEAD>\r\n"
            :<BODY>\r\n<H1>NetX Test Page"
            "</H1>\r\n</BODY>\r\n</HTML>\r\n", 103);

        /* Return completion status. */
        return(NX_WEB_HTTP_CALLBACK_COMPLETED);
    }

    return(NX_SUCCESS);
}
```

## nx_web_http_server_callback_generate_response_header

Create a response header in a callback function

### Prototype

```C
UINT nx_web_http_server_callback_generate_response_header(
    NX_WEB_HTTP_SERVER *server_ptr,
    NX_PACKET **packet_pptr,
    CHAR *status_code,
    UINT content_length,
    CHAR *content_type,
    CHAR* additional_header);
```

### Description

This service is used in the HTTP(S) server callback routine (defined by the application) to generate an HTTP response header. The server callback routine is invoked when the HTTP server responds to Client GET, PUT and DELETE requests which require an HTTP response. This function takes the response information from the application and generates the appropriate response header. See the service *nx_web_http_server_create* for more information on the server request callback routine.

This API is deprecated. Developers are encouraged to use *nx_web_http_server_callback_generate_response_header_extended*.

### Input Parameters

- *server_ptr*: Pointer to HTTP Server control block.
- *packet_pptr*: Pointer a packet pointer allocated for message
- *status_code*: Indicate status of resource. Examples:
  - **NX_WEB_HTTP_STATUS_OK**
  - **NX_WEB_HTTP_STATUS_MODIFIED**
  - **NX_WEB_HTTP_STATUS_INTERNAL_ERROR**
- *content_length*: Size of content in bytes
- *content_type*: Type of HTTP e.g. "text/plain"
- *additional_header*: Pointer to additional header text

### Return Values

- **NX_SUCCESS** (0x00) Successfully created HTML header
- **NX_PTR_ERROR** (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```C
CHAR demotestbuffer[] = "<html>> r\n\r\n<head>> r\n\r\n<title>Main \
    Window</title>> r\n</head>> r\n\r\n<body>Test message\r\n \ </body>> r\n</html>> r\n";

/* my_request_notify* is the application request notify callback registered
    with the HTTP server in *nx_web_http_server_create*,
    creates a response to the received Client request. */

UINT my_request_notify(NX_WEB_HTTP_SERVER *server_ptr, UINT request_type,
    CHAR *resource, NX_PACKET *recv_packet_ptr)
{
    NX_PACKET *resp_packet_ptr;
    ULONG string_length;
    CHAR temp_string[30];
    ULONG length = 0;
    length = strlen(&demotestbuffer[0]);

    /* Derive the client request type from the client request. */
    nx_web_http_server_type_extract(server_ptr,
        server_ptr -> nx_web_http_server_request_resource,
        temp_string, sizeof(temp_string), &string_length);

    /* Null terminate the string. */
    temp_string[string_length] = 0;

    /* Now build a response header with server status is OK and no additional header info. */
    status = nx_web_http_server_callback_generate_response_header(http_server_ptr,
        &resp_packet_ptr, NX_WEB_HTTP_STATUS_OK,
        length, temp_string, NX_NULL);

    /* If status is NX_SUCCESS, the header was successfully appended. */

    /* Now add data to the packet. */
    status = nx_packet_data_append(resp_packet_ptr, &demotestbuffer[0],
        strlen(&demotestbuffer[0]), server_ptr >>
        nx_web_http_server_packet_pool_ptr, NX_WAIT_FOREVER);

    if (status != NX_SUCCESS)
    {
        nx_packet_release(resp_packet_ptr);
        return status;
    }

    /* Now send the packet! */
    status = nx_web_http_server_callback_packet_send(
        &(server_ptr -> nx_web_http_server_socket),
        resp_packet_ptr);

    if (status != NX_SUCCESS)
    {
        nx_packet_release(resp_packet_ptr);
        return status;
    }

    /* Let HTTP server know the response has been sent. */
    return(NX_WEB_HTTP_CALLBACK_COMPLETED);
}
```

## nx_web_http_server_callback_generate_response_header_extended

Create a response header in a callback function

### Prototype

```C
UINT nx_web_http_server_callback_generate_response_header_extended(
    NX_WEB_HTTP_SERVER *server_ptr,
    NX_PACKET **packet_pptr,
    CHAR *status_code,
    UINT status_code_length,
    UINT content_length,
    CHAR *content_type,
    UINT content_type_length,
    CHAR* additional_header,
    UINT additional_header_length);
```

### Description

This service is used in the HTTP(S) server callback routine (defined by the application) to generate an HTTP response header. The server callback routine is invoked when the HTTP server responds to Client GET, PUT and DELETE requests which require an HTTP response. This function takes the response information from the application and generates the appropriate response header. See the service *nx_web_http_server_create* for more information on the server request callback routine.

### Input Parameters

- *server_ptr*: Pointer to HTTP Server control block.
- *packet_pptr*: Pointer a packet pointer allocated for message
- *status_code*: Indicate status of resource. Examples:
  - **NX_WEB_HTTP_STATUS_OK**
  - **NX_WEB_HTTP_STATUS_MODIFIED**
  - **NX_WEB_HTTP_STATUS_INTERNAL_ERROR**
- *status_code_length*: String length of status code
- *content_length*: Size of content in bytes
- *content_type*: Type of HTTP e.g. "text/plain"
- *content_type_length*: String length of content type
- *additional_header*: Pointer to additional header text
- *additional_header_length*: Length of additional header text

### Return Values

- **NX_SUCCESS** (0x00) Successfully created HTML header
- **NX_PTR_ERROR** (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```C
CHAR demotestbuffer[] = "<html>> r\n\r\n<head>> r\n\r\n<title>Main \
    Window</title>> r\n</head>> r\n\r\n<body>Test message\r\n \ </body>> r\n</html>> r\n";

/* my_request_notify* is the application request notify callback
    registered with the HTTP server in *nx_web_http_server_create*,
    creates a response to the received Client request. */

UINT my_request_notify(NX_WEB_HTTP_SERVER *server_ptr, UINT request_type,
    CHAR *resource, NX_PACKET *recv_packet_ptr)
{
    NX_PACKET *resp_packet_ptr;
    ULONG string_length;
    CHAR temp_string[30];
    ULONG length = 0;
    length = strlen(&demotestbuffer[0]);

    /* Derive the client request type from the client request. */
    nx_web_http_server_type_extract(server_ptr,
        server_ptr -> nx_web_http_server_request_resource,
        temp_string, sizeof(temp_string), &string_length);

    /* Null terminate the string. */
    temp_string[string_length] = 0;

    /* Now build a response header with server status is OK and no additional header info. */
    status = nx_web_http_server_callback_generate_response_header_extended(http_server_ptr,
        &resp_packet_ptr, NX_WEB_HTTP_STATUS_OK,
        sizeof(NX_WEB_HTTP_STATUS_OK) – 1,
        length, temp_string, string_length NX_NULL, 0);

    /* If status is NX_SUCCESS, the header was successfully appended. */

    /* Now add data to the packet. */
    status = nx_packet_data_append(resp_packet_ptr, &demotestbuffer[0],
        strlen(&demotestbuffer[0]), server_ptr >>
        nx_web_http_server_packet_pool_ptr, NX_WAIT_FOREVER);

    if (status != NX_SUCCESS)
    {
        nx_packet_release(resp_packet_ptr);
        return status;
    }

    /* Now send the packet! */
    status = nx_web_http_server_callback_packet_send(
        &(server_ptr -> nx_web_http_server_socket),
        resp_packet_ptr);

    if (status != NX_SUCCESS)
    {
        nx_packet_release(resp_packet_ptr);
        return status;
    }

    /* Let HTTP server know the response has been sent. */
    return(NX_WEB_HTTP_CALLBACK_COMPLETED);

}
```

## nx_web_http_server_callback_packet_send

Send an HTTP packet from callback function

### Prototype

```C
UINT nx_web_http_server_callback_packet_send(
    NX_WEB_HTTP_SERVER *server_ptr,
    NX_PACKET *packet_ptr);
```

### Description

This service sends a complete HTTP server response from an HTTP callback. HTTP server will send the packet with the NX_WEB_HTTP_SERVER _TIMEOUT_SEND. The HTTP header and data must be appended to the packet. If the return status indicates an error, the HTTP application must release the packet.

The callback should return NX_WEB_HTTP_CALLBACK_COMPLETED.

See *nx_web_http_server_callback_generate_response_header* for a more detailed example.

### Input Parameters

- *server_ptr*: Pointer to HTTP Server control block
- *packet_ptr*: Pointer to the packet to send

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent HTTP Server packet
- **NX_PTR_ERROR** (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```C
/* The packet is appended with HTTP header and data
    and is ready to send to the Client directly. */
status = nx_web_http_server_callback_packet_send(server_ptr, packet_ptr);

if (status != NX_SUCCESS)
{
    nx_packet_release(packet_ptr);
}

return(NX_WEB_HTTP_CALLBACK_COMPLETED);
```

## nx_web_http_server_callback_response_send

Send response from callback function

### Prototype

```C
UINT nx_web_http_server_callback_response_send(
    NX_WEB_HTTP_SERVER *server_ptr,
    CHAR *header,
    CHAR *information,
    CHAR additional_info);
```

### Description

This service sends the supplied response information from the application's callback routine. This is typically used to send custom responses associated with GET/POST requests. Note that if this function is used, the callback routine must return the status of NX_WEB_HTTP_CALLBACK_COMPLETED.

This service is deprecated. Developers are encouraged to use *nx_web_http_server_callback_response_send_extended*.

### Input Parameters

- *server_ptr*: Pointer to HTTP Server control block.
- *header*: Pointer to the response header string.
- *information*: Pointer to the information string.
- *additional_info*: Pointer to the additional information string.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent HTTP Server response

### Allowed From

Threads

### Example

```C
UINT my_request_notify(NX_WEB_HTTP_SERVER *server_ptr, UINT request_type,
    CHAR *resource, NX_PACKET *packet_ptr)
{
    /* Look for the test resource! */
    if ((request_type == NX_WEB_HTTP_SERVER_GET_REQUEST) &&
        (strcmp(resource, "/test.htm") == 0))
    {
        /* In this example, we will complete the GET processing with
            a resource not found response. */
        nx_web_http_server_callback_response_send(server_ptr,
            "HTTP/1.0 404 ",
            "NetX HTTP Server unable to find file: ",
            resource);

        /* Return completion status. */
        return(NX_WEB_HTTP_CALLBACK_COMPLETED);
    }

    return(NX_SUCCESS);
}
```

## nx_web_http_server_callback_response_send_extended

Send response from callback function

### Prototype

```C
UINT nx_web_http_server_callback_response_send_extended(
    NX_WEB_HTTP_SERVER *server_ptr,
    CHAR *header, UINT header_length,
    CHAR *information,
    UINT information_length,
    CHAR additional_info,
    UINT additional_info_length);
```

### Description

This service sends the supplied response information from the application's callback routine. This is typically used to send custom responses associated with GET/POST requests. Note that if this function is used, the callback routine must return the status of NX_WEB_HTTP_CALLBACK_COMPLETED.

The strings of header, information and additional_info must be NULL-terminated and length of each string matches the length specified in the argument list.

This service replaces *nx_web_http_server_callback_response_send*. This version requires callers to supply length information to the function.

### Input Parameters

- *server_ptr*: Pointer to HTTP Server control block.
- *header*: Pointer to the response header string.
- *header_length*: Length of the response header string.
- *information*: Pointer to the information string.
- *Information_length*: Length of the information string.
- *additional_info*: Pointer to the additional information string.
- *additional_info_length*: Length of the additional information string.

### Return Values

- **NX_SUCCESS** (0x00) Successfully sent HTTP Server response

### Allowed From

Threads

### Example

```C
UINT my_request_notify(NX_WEB_HTTP_SERVER *server_ptr, UINT request_type,
    CHAR *resource, NX_PACKET *packet_ptr)
{
    /* Look for the test resource! */
    if ((request_type == NX_WEB_HTTP_SERVER_GET_REQUEST) &&
        (strcmp(resource, "/test.htm") == 0))
    {
        /* In this example, we will complete the GET processing with
            a resource not found response. */
        nx_web_http_server_callback_response_send_extended(server_ptr,
            "HTTP/1.0 404 ",
            sizeof("HTTP/1.0 404 ") – 1,
            "NetX HTTP Server unable to find file: ",
            sizeof("NetX HTTP Server unable to find file: ") – 1,
            resource, strlen(resource));

        /* Return completion status. */
        return(NX_WEB_HTTP_CALLBACK_COMPLETED);
    }

    return(NX_SUCCESS);
}
```

## nx_web_http_server_content_get

Get content from the request

### Prototype

```C
UINT nx_web_http_server_content_get(
    NX_WEB_HTTP_SERVER *server_ptr,
    NX_PACKET *packet_ptr,
    ULONG byte_offset,
    CHAR *destination_ptr,
    UINT destination_size,
    UINT *actual_size);
```

### Description

This service attempts to retrieve the specified amount of content from the POST or PUT HTTP Client request. It should be called from the application's request notify callback specified during HTTP Server creation (*nx_web_http_server_create*).

### Input Parameters

- *server_ptr*: Pointer to HTTP Server control block.
- *packet_ptr*: Pointer to the HTTP Client request packet. Note that this packet must not be released by the request notify callback.
- *byte_offset*: Number of bytes to offset into the content area.
- *destination_ptr*: Pointer to the destination area for the content.
- *destination_size*: Maximum number of bytes available in the destination area.
- *actual_size*: Pointer to the destination variable that will be set to the actual size of the content copied.

### Return Values

- **NX_SUCCESS** (0x00) Successful HTTP Server content Get
- **NX_WEB_HTTP_ERROR** (0x30000) HTTP Server internal error
- **NX_WEB_HTTP_DATA_END** (0x30007) End of request content
- **NX_WEB_HTTP_TIMEOUT** (0x30001) HTTP Server timeout in getting next packet of content
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```C
/* Assuming we are in the application's request notify callback
    routine, retrieve up to 100 bytes of content starting at offset
    0. */
status = nx_web_http_server_content_get(&my_server, packet_ptr,
    0, my_buffer, 100, &actual_size);

/* If status is NX_SUCCESS, "my_buffer" contains "actual_size" bytes of
    request content. */
```

## nx_web_http_server_content_get_extended

Get content from the request/supports zero length Content Length

### Prototype

```C
UINT nx_web_http_server_content_get_extended(
    NX_WEB_HTTP_SERVER *server_ptr,
    NX_PACKET *packet_ptr,
    ULONG byte_offset,
    CHAR *destination_ptr,
    UINT destination_size,
    UINT *actual_size);
```

### Description

This service is almost identical to *nx_web_http_server_content_get*; it attempts to retrieve the specified amount of content from the POST or PUT HTTP Client request. However it handles requests with Content Length of zero value ('empty request') as a valid request. It should be called from the application's request notify callback specified during HTTP Server creation (*nx_web_http_server_create*).

This service replaces *nx_web_http_server_content_get*. This version requires callers to supply length information to the function.

### Input Parameters

- *server_ptr*: Pointer to HTTP Server control block.
- *packet_ptr*: Pointer to the HTTP Client request packet. Note that this packet must not be released by the request notify callback.
- *byte_offset*: Number of bytes to offset into the content area.
- *destination_ptr*: Pointer to the destination area for the content.
- *destination_size*: Maximum number of bytes available in the destination area.
- *actual_size*: Pointer to the destination variable that will be set to the actual size of the content copied.

### Return Values

- **NX_SUCCESS** (0x00) Successful HTTP content get
- **NX_WEB_HTTP_ERROR** (0x30000) HTTP Server internal error
- **NX_WEB_HTTP_DATA_END** (0x30007) End of request content
- **NX_WEB_HTTP_TIMEOUT** (0x30001) HTTP Server timeout in getting next packet
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```C
/* Assuming we are in the application's request notify callback
    routine, retrieve up to 100 bytes of content starting at offset
    0. */
status = nx_web_http_server_content_get_extended(&my_server, packet_ptr,
    0, my_buffer, 100, &actual_size);

/* If status is NX_SUCCESS, "my_buffer" contains "actual_size" bytes of
    request content. */
```

## nx_web_http_server_content_length_get

Get length of content in the request/supports Content Length of zero value

### Prototype

```C
UINT nx_web_http_server_content_length_get(
    NX_PACKET *packet_ptr,
    UINT *content_length);
```

### Description

This service attempts to retrieve the HTTP content length in the supplied packet. The return value indicates successful completion status and the actual length value is returned in the input pointer content_length. If there is no HTTP content/Content Length = 0, this routine still returns a successful completion status and the content_length input pointer points to a valid length (zero). It should be called from the application's request notify callback specified during HTTP Server creation (*nx_web_http_server_create*).

### Input Parameters

- *packet_ptr*: Pointer to the HTTP Client request packet. Note that this packet must not be released by the request notify callback.
- *content_length*: Pointer to value retrieved from Content Length field

### Return Values

- **NX_SUCCESS** (0x00) Successful HTTP Server Content Length Get
- **NX_WEB_HTTP_INCOMPLETE_PUT_ERROR** (0x3000F) Improper HTTP header format
- NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```C
/* Assuming we are in the application's request notify callback
    routine, get the content length of the HTTP Client request. */

ULONG content_length;
status = nx_web_http_server_content_length_get(packet_ptr, &content_length);

/* If the "status" variable indicates successful completion, the "length"
    Variable contains the length of the HTTP Client request content area. */
```

## nx_web_http_server_create

Create an HTTP Server instance

### Prototype

```C
UINT nx_web_http_server_create(NX_WEB_HTTP_SERVER *http_server_ptr,
    CHAR *http_server_name, NX_IP *ip_ptr, UINT server_port,
    FX_MEDIA *media_ptr, VOID *stack_ptr, ULONG stack_size,
    NX_PACKET_POOL *pool_ptr,
    UINT (*authentication_check)(NX_WEB_HTTP_SERVER *server_ptr,
        UINT request_type, CHAR *resource, CHAR **name,
        CHAR **password, CHAR **realm),
    UINT (*request_notify)(NX_WEB_HTTP_SERVER *server_ptr,
        UINT request_type, CHAR *resource, NX_PACKET *packet_ptr));
```

### Description

This service creates an HTTP Server instance, which runs in the context of its own ThreadX thread. The optional *authentication_check* and *request_notify* application callback routines give the application software control over the basic operations of the HTTP Server.

This service is used to create both plaintext HTTP servers and TLS-secured HTTPS servers. To enable HTTPS using TLS, see the service *nx_web_http_server_secure_configure*.

### Input Parameters

- *http_server_ptr*: Pointer to HTTP Server control block.
- *http_server_name*: Pointer to HTTP Server's name.
- *ip_ptr*: Pointer to previously created IP instance.
- *server_port*: TCP listening port for server instance.
- *media_ptr*: Pointer to previously created FileX media instance.
- *stack_ptr*: Pointer to HTTP Server thread stack area.
- *stack_size*: Pointer to HTTP Server thread stack size.
- *authentication_check*: Function pointer to application's authentication checking routine. If specified, this routine is called for each HTTP Client request. If this parameter is NULL, no authentication will be performed. This parameter is deprecated. Call *nx_web_http_server_authenticate_check_set* instead.
- *request_notify*: Function pointer to application's request notify routine. If specified, this routine is called prior to the HTTP server processing of the request. This allows the resource name to be redirected or fields within a resource to be updated prior to completing the HTTP Client request.

### Return Values

- **NX_SUCCESS** (0x00) Successful HTTP Server create.
- NX_PTR_ERROR (0x07) Invalid HTTP Server, IP, media, stack, or packet pool pointer.
- NX_WEB_HTTP_POOL_ERROR (0x30009) Packet payload of pool is not large enough to contain complete HTTP request.

### Allowed From

Initialization, Threads

### Example

```C
/* Create an HTTP Server instance called "my_server." */
status = nx_web_http_server_create(&my_server, "my server", &ip_0,
    NX_WEB_HTTPS_SERVER_PORT, &ram_disk,
    stack_ptr, stack_size, &pool_0,
    my_authentication_check, my_request_notify);

/* If status equals NX_SUCCESS, the HTTP Server creation was successful. */
```

## nx_web_http_server_delete

Delete an HTTP Server instance

### Prototype

```C
UINT nx_web_http_server_delete(NX_WEB_HTTP_SERVER *http_server_ptr);
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

```C
/* Delete the HTTP Server instance called "my_server." */
status = nx_web_http_server_delete(&my_server);

/* If status equals NX_SUCCESS, the HTTP Server delete was successful. */
```

## nx_web_http_server_get_entity_content

Retrieve the location and length of entity data

### Prototype

```C
UINT nx_web_http_server_get_entity_content(
    NX_WEB_HTTP_SERVER *server_ptr,
    NX_PACKET **packet_pptr,
    ULONG *available_offset,
    ULONG *available_length);
```

### Description

This service determines the location of the start of data within the current multipart entity in the received Client messages, and the length of data not including the boundary string. Internally, the HTTP server updates its own offsets so that this function can be called again on the same Client datagram for messages with multiple entities. The packet pointer is updated to the next packet where the Client message is a multi-packet datagram.

Note that NX_WEB_HTTP_MULTIPART_ENABLE must be enabled to use this service. Also note that the application should not release the packet pointed to by packet_pptr. This is done internally by the HTTP server.

See *nx_web_http_server_get_entity_header* for more details.

### Input Parameters

- *server_ptr*: Pointer to HTTP Server
- *packet_pptr*: Pointer to location of packet pointer. Note that the application should not release this packet
- *available_offset*: Pointer to offset of entity data from the packet prepend pointer
- *available_length*: Pointer to length of entity data

### Return Values

- **NX_SUCCESS** (0x00) Successfully retrieved size and location of entity content
- **NX_WEB_HTTP_BOUNDARY_ALREADY_FOUND** (0x30016) Content for the HTTP server internal multipart markers is already found
- NX_WEB_HTTP_ERROR (0x30000) HTTP Server internal error
- NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```C
NX_WEB_HTTP_SERVER my_server;
UINT offset, length;
NX_PACKET *packet_ptr;

/* Inside the request notify callback, the HTTP server application first obtains
    the entity header to determine details about the multipart data. If
    successful, it then calls this service to get the location of entity data: */
status = nx_web_http_server_get_entity_content(&my_server, &packet_ptr, *offset,
    &length);

/* If status equals NX_SUCCESS, offset and location determine the location of the
    entity data. */
```

## nx_web_http_server_get_entity_header

Retrieve the contents of entity header

### Prototype

```C
UINT nx_web_http_server_get_entity_header(
    NX_WEB_HTTP_SERVER *server_ptr,
    NX_PACKET **packet_pptr,
    UCHAR *entity_header_buffer,
    ULONG buffer_size);
```

### Description

This service retrieves the entity header into the specified buffer. Internally HTTP Server updates its own pointers to locate the next multipart entity in a Client datagram with multiple entity headers. The packet pointer is updated to the next packet where the Client message is a multi-packet datagram.

Note that NX_WEB_HTTP_MULTIPART_ENABLE must be enabled to use this service. Note also that the application should not release the packet pointed to by packet_pptr.

### Input Parameters

- *server_ptr*: Pointer to HTTP Server
- *packet_pptr*: Pointer to location of packet pointer. Note that the application should not release this packet
- *entity_header_buffer*: Pointer to location to store entity header
- *buffer_size*: Size of input buffer

### Return Values

- **NX_SUCCESS** (0x00) Successfully retrieved entity Header
- **NX_WEB_HTTP_NOT_FOUND** (0x30006) Entity header field not found
- **NX_WEB_HTTP_TIMEOUT** (0x30001) Time expired to receive next packet for multipacket client message
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service
- NX_WEB_HTTP_ERROR (0x30000) Internal HTTP error

### Allowed From

Threads

### Example

```C
/* Buffer to hold data we are extracting from the request. */
UCHAR buffer[1440];

/* *my_request_notify* is the application request notify callback
    registered with the HTTP server in *nx_web_http_server_create*,
    creates a response to the received Client request. */
UINT my_request_notify(NX_WEB_HTTP_SERVER *server_ptr, UINT request_type,
    CHAR *resource, NX_PACKET *packet_ptr)
{
    UINT offset, length;
    NX_PACKET *response_pkt;

    /* Process multipart data. */
    if(request_type == NX_WEB_HTTP_SERVER_POST_REQUEST)
    {
        /* Get the content header. */
        while(nx_web_http_server_get_entity_header(server_ptr, &packet_ptr, buffer,
            sizeof(buffer)) == NX_SUCCESS)
        {
            /* Header obtained successfully. Get the content data location. */
            while(nx_web_http_server_get_entity_content(server_ptr,
                &packet_ptr, &offset, &length) == NX_SUCCESS)
            {
                /* Write content data to buffer. */
                nx_packet_data_extract_offset(packet_ptr, offset, buffer, length,
                    &length);
                buffer[length] = 0;
            }
        }

        /* Generate HTTP header. */
        status = nx_web_http_server_callback_generate_response_header(server_ptr,
            &response_pkt, NX_WEB_HTTP_STATUS_OK, 800, "text/html",
            "Server: NetX WEB HTTP 5.10\r\n");

        if(status == NX_SUCCESS)
        {
            if(nx_web_http_server_callback_packet_send(server_ptr, response_pkt) !=
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
    return(NX_WEB_HTTP_CALLBACK_COMPLETED);
}

```

## nx_web_http_server_gmt_callback_set

Set the callback to obtain GMT date and time

### Prototype

```C
UINT nx_web_http_server_gmt_callback_set(
    NX_WEB_HTTP_SERVER *server_ptr,
    VOID (*gmt_get)(NX_WEB_HTTP_SERVER_DATE *date);
```

### Description

This service sets the callback to obtain GMT date and time with a previously created HTTP server. This service is invoked with the HTTP server is creating a header in HTTP server responses to the Client.

### Input Parameters

- *server_ptr*: Pointer to HTTP Server
- *gmt_get*: Pointer to GMT callback
- *date*: Pointer to the date retrieved

### Return Values

- **NX_SUCCESS** (0x00) Successfully set the callback
- NX_PTR_ERROR (0x07) Invalid packet or parameter pointer.

### Allowed From

Threads

### Example

```C
NX_WEB_HTTP_SERVER my_server;

VOID get_gmt(NX_WEB_HTTP_SERVER_DATE *now);

/* After the HTTP server is created by calling nx_web_http_server_create,
    and before starting HTTP services when nx_web_http_server_start is called,
    set the GMT retrieve callback: */
status = nx_web_http_server_gmt_callback_set(&my_server, gmt_get);

/* If status equals NX_SUCCESS, the gmt_get will be called to set the HTTP server
    response header date. */
```

## nx_web_http_server_invalid_userpassword_notify_set

Set the callback to handle invalid user/password

### Prototype

```C
UINT nx_web_http_server_invalid_userpassword_notify_set(
    NX_WEB_HTTP_SERVER *http_server_ptr,
    UINT (*invalid_username_password_callback)(
        CHAR *resource,
        ULONG client_address,
        UINT request_type));
```

### Description

This service sets the callback invoked when an invalid username and password is received in a Client get, put or delete request, either by digest or basic authentication. The HTTP server must be previously created.

### Input Parameters

- *server_ptr*: Pointer to HTTP Server
- *invalid_username_password_callback*: Pointer to invalid user/pass callback
- *resource*: Pointer to the resource specified by the client
- *client_address*: Client address
- *request_type*: Indicates client request type. May be:
  - *NX_WEB_HTTP_SERVER_GET_REQUEST*
  - *NX_WEB_HTTP_SERVER_POST_REQUEST NX_WEB_HTTP_SERVER_HEAD_REQUEST*
  - *NX_WEB_HTTP_SERVER_PUT_REQUEST NX_WEB_HTTP_SERVER_DELETE_REQUEST*

### Return Values

- **NX_SUCCESS** (0x00) Successfully set the callback
- NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```C
NX_WEB_HTTP_SERVER my_server;

VOID invalid_username_password_callback(NX_CHAR *resource,
    ULONG client_address,
    UINT request_type);

/* After the HTTP server is created by calling nx_web_http_server_create,
    and before starting HTTP services when nx_web_http_server_start is called,
    set the invalid username password callback: */

status = nx_web_http_server_invalid_userpassword_notify_set( (&my_server,
    invalid_username_password_callback);

/* If status equals NX_SUCCESS, the invalid_username_password_callback function
    will be called when the HTTP server receives an invalid username/password. */
```

## nx_web_http_server_mime_maps_additional_set

Set additional MIME maps for HTML

### Prototype

```C
UINT nx_web_http_server_mime_maps_additional_set(
    NX_WEB_HTTP_SERVER *server_ptr,
    NX_WEB_HTTP_SERVER_MIME_MAP *mime_maps,
    UINT mime_maps_num);
```

### Description

This service allows the HTTP application developer to add additional MIME types from the default MIME types supplied by the NetX Duo Web HTTP Server. See *nx_web_http_server_get_type* for list of defined types.

When a client request is received, e.g. a GET request, HTTP server parses the requested file type from the HTTP header using preferentially the additional MIME map set and if no match if found, it looks for a match in the default MIME map of the HTTP server. If no match is found, the MIME type defaults to "text/plain".

If the request notify function is registered with the HTTP server, the request notify callback can call *nx_web_http_server_type_get_extended* to parse the file type.

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

```C
/* my_server is an NX_WEB_HTTP_SERVER previously created. */
static NX_WEB_HTTP_SERVER_MIME_MAP my_mime_maps[] =
{
    {"abc", "yourtype/abc"},
    {"xyz", "mytype/xyz"},
};

status = nx_web_http_server_mime_maps_additional_set(&my_server,
    &my_mime_maps[0], 2);

/* If status equals NX_SUCCESS, two additional MIME types are added to the HTTP
    server MIME map set." */
```

## nx_web_http_server_response_packet_allocate

Allocate an HTTP(S) packet

### Prototype

```C
UINT nx_web_http_server_response_packet_allocate(
    NX_WEB_HTTP_SERVER *server_ptr,
    NX_PACKET **packet_ptr,
    ULONG wait_option);
```

### Description

This service attempts to allocates a packet for the HTTP(S) server.

Note that if a subsequent NetX Duo or HTTP Server API using this packet as input **fails**, such as nx_packet_data_append or **nx_web_http_server_callback_packet_send, the application is responsible for releasing the packet. **

### Input Parameters

- *server_ptr*: Pointer to HTTP Server control block.
- *packet_ptr*: Pointer to allocated packet.
- *wait_option*: Defines the wait time in ticks if there are no packets available in the packet pool. The wait options are defined as follows:
  - **NX_NO_WAIT** (0x00000000) Selecting NX_NO_WAIT causes the calling thread to return immediately if the request cannot be fulfilled.
  - **NX_WAIT_FOREVER** (0xFFFFFFFF) Selecting NX_WAIT_FOREVER causes the calling thread to suspend indefinitely until the HTTP Server responds to the request.
  - **timeout value** (0x00000001 through 0xFFFFFFFE) Selecting a numeric value (0x1-0xFFFFFFFE) specifies the maximum number of timer-ticks to stay suspended while waiting for the HTTP Server response.

### Return Values

- **NX_SUCCESS** (0x00) Successful packet allocate
- **NX_NO_PACKET** (0x01) No packet available
- **NX_WAIT_ABORTED** (0x1A) Requested suspension was aborted by a call to *tx_thread_wait_abort*.
- **NX_INVALID_PARAMETERS** (0x4D) Packet size cannot support protocol.
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service.

### Allowed From

Threads

### Example

```C
/* Allocate a packet for HTTP(S) Server and suspend for a maximum of 5 timer
    ticks if the pool is empty. */
status = nx_web_http_server_response_packet_allocate(&my_client, &packet_ptr, 5);

/* If status is NX_SUCCESS, the newly allocated packet pointer is found in the
    variable packet_ptr. */
```

## nx_web_http_server_packet_content_find

Extract content length and set pointer to start of data

### Prototype

```C
UINT nx_web_http_server_packet_content_find(
    NX_WEB_HTTP_SERVER *server_ptr,
    NX_PACKET **packet_ptr,
    UINT *content_length);
```

### Description

This service extracts the content length from the HTTP header. It also updates the supplied packet as follows: the packet prepend pointer (start of location of packet buffer to write to) is set to the HTTP content (data) just passed the HTTP header.

If the beginning of content is not found in the current packet, the function waits for the next packet to be received using the NX_WEB_HTTP_SERVER_TIMEOUT_RECEIVE wait option.

Note this should not be called before calling *nx_web_http_server_get_entity_header* because it modifies the packet prepend pointer past the entity header.

### Input Parameters

- *server_ptr*: Pointer to HTTP server instance
- *packet_ptr*: Pointer to packet pointer for returning the packet with updated prepend pointer
- *content_length*: Pointer to extracted content_length

### Return Values

- **NX_SUCCESS** (0x00) HTTP content length found and packet successfully updated
- **NX_WEB_HTTP_TIMEOUT** (0x30001) Time expired waiting on next Packet
- NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```c
/* The HTTP server pointed to by server_ptr is previously created and started.
    The server has received a Client request packet, recv_packet_ptr,
    and the packet content find service is called from the request notify callback
    function registered with the HTTP server. */

UINT content_length;

status = nx_web_http_server_packet_content_find(server_ptr, recv_packet_ptr,
    &content_length);

/* If status equals NX_SUCCESS, the content length specifies the content length
    and the packet pointer prepend pointer is set to the HTTP content (data). */
```

## nx_web_http_server_packet_get

Receive the next HTTP packet

### Prototype

```C
UINT nx_web_http_server_packet_get(NX_WEB_HTTP_SERVER *server_ptr,
    NX_PACKET **packet_ptr);
```

### Description

This service returns the next packet received on the HTTP server socket. The wait option to receive a packet is NX_WEB_HTTP_SERVER_TIMEOUT_RECEIVE.

Note that if successful the application is responsible for releasing the packet.

### Input Parameters

- *server_ptr*: Pointer to HTTP server instance
- *packet_ptr*: Pointer to received packet

### Return Values

- **NX_SUCCESS** (0x00) Successfully received next HTTP packet
- **NX_WEB_HTTP_TIMEOUT** (0x30001) Time expired waiting on next Packet
- NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```C
/* The HTTP server pointed to by server_ptr is previously created and started. */
UINT content_length;
NX_PACKET *recv_packet_ptr;

status = nx_web_http_server_packet_get(server_ptr, &recv_packet_ptr);

/* If status equals NX_SUCCESS, a Client packet is obtained. */
```

## nx_web_http_server_param_get

Get parameter from the request

### Prototype

```C
UINT nx_web_http_server_param_get(
    NX_PACKET *packet_ptr,
    UINT param_number,
    CHAR *param_ptr,
    UINT *param_size,
    UINT max_param_size);
```

### Description

This service attempts to retrieve the specified HTTP URL parameter in the supplied request packet. If the requested HTTP parameter is not present, this routine returns a status of NX_WEB_HTTP_NOT_FOUND. This routine should be called from the application's request notify callback specified during HTTP Server creation (*nx_web_http_server_create*).

### Input Parameters

- *packet_ptr*: Pointer to HTTP Client request packet. Note that the application should not release this packet.
- *param_number*: Logical number of the parameter starting at zero, from left to right in the parameter list.
- *param_ptr*: Destination area to copy the parameter.
- *param_size*: Return the total parameter data length (in bytes).
- *max_param_size*: Maximum size of the parameter destination area.

### Return Values

- **NX_SUCCESS** (0x00) Successful HTTP Server parameter get
- **NX_WEB_HTTP_NOT_FOUND** (0x30006) Specified parameter not found
- **NX_WEB_HTTP_IMPROPERLY_TERMINATED_PARAM** (0x30015) Request parameter not properly terminated
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```C
/* Assuming we are in the application's request notify callback
    routine, get the first parameter of the HTTP Client request. */

status = nx_web_http_server_param_get(request_packet_ptr, 0, param_destination,
    &param_size, 30);

/* If status equals NX_SUCCESS, the NULL-terminated first parameter can be found
    in "param_destination" and the size of that string can be found
    in the variable "param_size." */
```

## nx_web_http_server_query_get

Get query from the request

### Prototype

```C
UINT nx_web_http_server_query_get(
    NX_PACKET *packet_ptr,
    UINT query_number,
    CHAR *query_ptr,
    CHAR *query_size,
    UINT max_query_size);
```

### Description

This service attempts to retrieve the specified HTTP URL query in the supplied request packet. If the requested HTTP query is not present, this routine returns a status of NX_WEB_HTTP_NOT_FOUND. This routine should be called from the application's request notify callback specified during HTTP Server creation (*nx_web_http_server_create*).

### Input Parameters

- *packet_ptr*: Pointer to HTTP Client request packet. Note that the application should not release this packet.
- *query_number*: Logical number of the parameter starting at zero, from left to right in the query list.
- *query_ptr*: Destination area to copy the query.
- *query_size*: Return query data size (in bytes).
- *max_query_size*: Maximum size of the query destination

area.

### Return Values

- **NX_SUCCESS** (0x00) Successful HTTP Server query get
- **NX_WEB_HTTP_FAILED** (0x30002) Query size too small.
- **NX_WEB_HTTP_NOT_FOUND** (0x30006) Specified query not found
- **NX_WEB_HTTP_NO_QUERY_PARSED** (0x30013) No query in Client request
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```C
/* Assuming we are in the application's request notify callback
    routine, get the first query of the HTTP Client request. */

status = nx_web_http_server_query_get(request_packet_ptr, 0,
    query_destination, &query_size, 30);

/* If status equals NX_SUCCESS, the NULL-terminated first query can be found
    in "query_destination" and the length of that string can be found in the
    variable "query_size". */
```

## nx_web_http_server_response_chunked_set

Set chunked transfer for HTTP(S) response

### Prototype

```C
UINT nx_web_http_server_response_chunked_set(
    NX_WEB_HTTP_SERVER *server_ptr,
    UINT chunk_size,
    NX_PACKET *packet_ptr);
```

### Description

This service uses chunked transfer coding to send a custom HTTP(S) response data packet created with *nx_web_http_server_response_packet_allocate* to the client.

> **Note:** If the application uses chunked transfer coding to send a response data packet, it must call this service after calling *nx_web_http_server_response_packet_allocate*, and before calling *nx_web_http_server_callback_packet_send*.

### Input Parameters

- *client_ptr*: Pointer to HTTP Client control block.
- *chunk_size*: Size of the chunk-data in octets.
- *packet_ptr*: HTTP(S) request data packet pointer.

### Return Values

- **NX_SUCCESS** (0x00) Successful set chunked.
- NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Threads

### Example

```C
/* Generate HTTP header. */
nx_web_http_server_callback_generate_response_header(server_ptr,
    &response_pkt, NX_WEB_HTTP_STATUS_OK, 0, "text/html",
    "Transfer-Encoding: chunked\r\n");

/* Create a new data packet response on the HTTP(S) Server instance. */
nx_web_http_server_response_packet_allocate(&my_server, &my_packet, NX_WAIT_FOREVER);

/* Set the chunked transfer. */
status = nx_web_http_server_response_chunked_set(&my_server, 128, my_packet)

/* At this point, user can fill the data into my_packet. */
nx_packet_data_append(my_packet, data_ptr, data_size,
    packet_pool, NX_WAIT_FOREVER);

/* Send data packet response to client. */
nx_web_http_server_callback_packet_send(&my_server, my_packet);
```

## nx_web_http_server_secure_configure

Configure an HTTP Server to use TLS for secure HTTPS

### Prototype

```C
UINT nx_web_http_server_secure_configure(
    NX_WEB_HTTP_SERVER *http_server_ptr,
    const NX_SECURE_TLS_CRYPTO *crypto_table,
    VOID *metadata_buffer,
    ULONG metadata_size,
    UCHAR* packet_buffer,
    UINT packet_buffer_size,
    NX_SECURE_X509_CERT *identity_certificate,
    NX_SECURE_X509_CERT *trusted_certificates[],
    UINT trusted_certs_num,
    NX_SECURE_X509_CERT *remote_certificates[],
    UINT remote_certs_num,
    UCHAR *remote_certificate_buffer,
    UINT remote_cert_buffer_size);
```

### Description

This service configures a previously created NetX Duo Web HTTP server instance to use TLS for secure HTTPS communications. The parameters are used to configure all the possible TLS sessions with identical state so that each incoming HTTPS Client experiences consistent behavior. The number of TLS sessions is controlled using the macro NX_WEB_HTTP_SESSION_MAX.

The cryptographic routine table (ciphersuite table) is shared between all TLS sessions as it just contains function pointers.

The metadata and packet reassembly buffers are each divided equally between all TLS sessions. If the buffer size is not evenly divisible by the number of sessions the remainder will be unused.

The passed-in identity certificate is used by all sessions. During TLS operation the server identity certificate is only read from so copies are not needed for each session.

The trusted certificates are added to each TLS session in the HTTPS Server. These are used for Client certificate authentication which is automatically enabled when remote certificate space is provided.

The remote certificate array and buffer is shared by default between all TLS sessions. The remote certificates are used for Client certificate authentication which is automatically enabled when the remote certificate count is nonzero. Due to the buffer being shared some sessions may block during certificate validation.

To disable client certificate authentication, pass NX_NULL for the remote_certificates parameter and a value of 0 for the remote_certs_num parameter.

Return values will include any TLS error codes resulting from issues in the configuration of the TLS sessions.

### Input Parameters

- *http_server_ptr*: Pointer to HTTP Server instance.
- *crypto_table*: Pointer to TLS ciphersuite table.
- *metadata_buffer*: Pointer to cryptographic metadata buffer.
- *metadata_size*: Size of cryptographic metadata buffer.
- *packet_buffer*: TLS packet reassembly buffer.
- *packet_buffer*: Size of TLS packet buffer – should be equal to (<desired TLS buffer size* NX_WEB_HTTP_SESSION_MAX).
- *identity_certificate*: TLS server identity certificate – will be used for all HTTPS server sessions.
- *trusted_certificates*: Pointer to array of NX_SECURE_X509_CERT objects, used to validate incoming client certificates if client certificate authentication is enabled by passing a non-zero value for the *remote_certs_num* parameter.
- *trusted_certs_num*: Number of trusted certificates in the *trusted_certificates* array.
- *remote_certificates*: Pointer to array of NX_SECURE_X509_CERT objects, used for incoming client certificates.
- *remote_certs_num*: Number of remote certificates. Should be the maximum number of expected certificates from clients. Client certificate authentication is enabled automatically when this is non-zero.
- *remote_certificate_buffer*: Buffer to contain incoming remote certificates from clients if client certificate authentication is enabled. remote_cert_buffer_size Size of remote certificates buffer. Should be equal to (<maximum expected certificate size \* remote_certs_num).


### Return Values

- **NX_SUCCESS** (0x00) Successful initialization of the TLS session.
- **NX_NOT_CONNECTED** (0x38) The underlying TCP socket is no longer connected.
- **NX_SECURE_TLS_UNRECOGNIZED_MESSAGE_TYPE** (0x102) A received TLS message type is incorrect.
- **NX_SECURE_TLS_UNSUPPORTED_CIPHER** (0x106) A cipher provided by the remote host is not supported.
- **NX_SECURE_TLS_HANDSHAKE_FAILURE** (0x107) Message processing during the TLS handshake has failed.
- **NX_SECURE_TLS_HASH_MAC_VERIFY_FAILURE** (0x108) An incoming message failed a  hash MAC check.
- **NX_SECURE_TLS_TCP_SEND_FAILED** (0x109) An underlying TCP socket send failed.
- **NX_SECURE_TLS_INCORRECT_MESSAGE_LENGTH** (0x10A) An incoming message had an invalid length field.
- **NX_SECURE_TLS_BAD_CIPHERSPEC** (0x10B) An incoming ChangeCipherSpec message was incorrect.
- **NX_SECURE_TLS_INVALID_SERVER_CER** (0x10C) An incoming TLS certificate is unusable for identifying the remote TLS server.
- **NX_SECURE_TLS_UNSUPPORTED_PUBLIC_CIPHER** (0x10D) The public-key cipher provided by the remote host is unsupported.
- **NX_SECURE_TLS_NO_SUPPORTED_CIPHERS** (0x10E) The remote host has indicated no ciphersuites that are supported by the NetX Duo Secure TLS stack.
- **NX_SECURE_TLS_UNKNOWN_TLS_VERSION** (0x10F) A received TLS message had an unknown TLS version in its header.
- **NX_SECURE_TLS_UNSUPPORTED_TLS_VERSION** (0x110) A received TLS message had a known but unsupported TLS version in its header.
- **NX_SECURE_TLS_ALLOCATE_PACKET_FAILED** (0x111) An internal TLS packet allocation failed.
- **NX_SECURE_TLS_INVALID_CERTIFICATE** (0x112) The remote host provided an invalid certificate.
- **NX_SECURE_TLS_ALERT_RECEIVED** (0x114) The remote host sent an alert indicating an error and ending the TLS session.
- **NX_PTR_ERROR** (0x07) Tried to use an invalid pointer.

### Allowed From

Initialization, Threads

### Example

```C
/* Create the HTTPS Server. */

status = nx_web_http_server_create(&my_server, "My HTTP Server",
    &ip_0, &ram_disk, &server_stack, sizeof(server_stack),
    &pool_0, authentication_check, server_request_callback);

/* Initialize device certificate (used for all sessions in HTTPS server). */
nx_secure_x509_certificate_initialize(&certificate, device_cert_der,
    device_cert_der_len, NX_NULL, 0,
    device_cert_key_der, device_cert_key_der_len,
    NX_SECURE_X509_KEY_TYPE_RSA_PKCS1_DER);

/* Setup TLS session for the HTTPS server.
    Note that since the remote_certs_num parameter is 0,
    no trusted certificates are needed, and Client certificate authentication is disabled. */
status = nx_web_http_server_secure_configure(&my_server, &nx_crypto_tls_ciphers,
    crypto_metadata,
    sizeof(crypto_metadata),
    tls_packet_buffer, sizeof(tls_packet_buffer),
    &certificate, NX_NULL, 0, NX_NULL, 0, NX_NULL, 0);

/* Start an HTTPS Server with TLS. */
status = nx_web_http_server_start(&my_server);

/* If status equals NX_SUCCESS, the HTTP Server has been started. */
```

## nx_web_http_server_start

Start the HTTP Server

### Prototype

```C
UINT nx_web_http_server_start(NX_WEB_HTTP_SERVER *http_server_ptr);
```

### Description

This service starts a previously created HTTP or HTTPS Server instance.

HTTPS servers share the same API as HTTP. To enable HTTPS using TLS on an HTTP server, see the service *nx_web_http_server_secure_configure.*

### Input Parameters

- *http_server_ptr*: Pointer to HTTP Server instance.

### Return Values

- **NX_SUCCESS** (0x00) Successful HTTP Server Start
- NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Initialization, Threads

### Example

```C
/* Start the HTTP Server instance "my_server." */
status = nx_web_http_server_start(&my_server);

/* If status equals NX_SUCCESS, the HTTP Server has been started. */
```

## nx_web_http_server_stop

Stop the HTTP Server

### Prototype

```C
UINT nx_web_http_server_stop(NX_WEB_HTTP_SERVER *http_server_ptr);
```

### Description

This service stops the previously create HTTP Server instance. This routine should be called prior to deleting an HTTP Server instance.

### Input Parameters

- *http_server_ptr*: Pointer to HTTP Server instance.

### Return Values

- **NX_SUCCESS** (0x00) Successful HTTP Server Stop
- NX_PTR_ERROR (0x07) Invalid pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```C
/* Stop the HTTP Server instance "my_server." */
status = nx_web_http_server_stop(&my_server);

/* If status equals NX_SUCCESS, the HTTP Server has been stopped. */
```

## nx_web_http_server_type_get

Extract file type from Client HTTP request

### Prototype

```C
UINT nx_web_http_server_type_get(
    NX_WEB_HTTP_SERVER *http_server_ptr,
    CHAR *name, CHAR *http_type_string,
    UINT *string_size);
```

### Description

> **Note:** This service is deprecated. Users are encouraged to use the service *nx_web_http_server_type_get_extended*.

This service extracts the HTTP request type in the buffer *http_type_string* and its length in *string_size* from the input buffer *name*, usually the URL. If no MIME map is found, it defaults to the "text/plain" type. Otherwise it compares the extracted type against the HTTP Server default MIME maps for a match. The default MIME maps in NetX Duo Web HTTP Server are:

- html text/html
- htm text/html
- txt text/plain
- gif image/gif
- jpg image/jpeg
- ico image/x-icon

If supplied, it will also search a user defined set of additional MIME maps. See *nx_web_http_server_mime_maps_additional_set* for more details on user defined maps.

### Input Parameters

- *http_server_ptr*: Pointer to HTTP Server instance
- *name*: Pointer to buffer to search
- *http_type_string*: Pointer to extracted HTML type string
- *string_size*: Pointer to return extracted HTML type string length.

### Return Values

- **NX_SUCCESS** (0x00) Successful extraction of type
- NX_PTR_ERROR (0x07) Invalid pointer input
- **NX_WEB_HTTP_EXTENSION_MIME_DEFAULT** (0x30019) Default "text/plain" returned.

### Allowed From

Application

### Example

```C
/* my_server is a previously created HTTP server, which starts accepting client
    requests when *nx_web_http_server_start* is called */

CHAR temp_string[20];
UINT string_length;

/* Extract the HTTP type. */
string_length = nx_web_http_server_type_get(&my_server_ptr,
    my_server.nx_web_http_server_request_resource, temp_string);

/* If string_length is non zero, the HTTP string is extracted. */
    For a more detailed example, see the description for
    *nx_web_http_server_callback_generate_response_header.*
```

## nx_web_http_server_type_get_extended

Extract file type from Client HTTP request

### Prototype

```C
UINT nx_web_http_server_type_get_extended(
    NX_WEB_HTTP_SERVER *http_server_ptr,
    CHAR *name, UINT name_length,
    CHAR *http_type_string,
    UINT http_type_string_max_size,
    UINT *string_size);
```

### Description

This service extracts the HTTP request type in the buffer *http_type_string* and its length in *string_size* from the input buffer *name*, usually the URL. If no MIME map is found, it defaults to the "text/plain" type. Otherwise it compares the extracted type against the HTTP Server default MIME maps for a match. The default MIME maps in NetX Duo Web HTTP Server are:

- html text/html
- htm text/html
- txt text/plain
- gif image/gif
- jpg image/jpeg
- ico image/x-icon

If supplied, it will also search a user defined set of additional MIME maps. See *nx_web_http_server_mime_maps_additional_set* for more details on user defined maps.

This service replaces *nx_web_http_server_type_get*. This version requires callers to supply length information to the function.

### Input Parameters

- *http_server_ptr*: Pointer to HTTP Server instance
- *name*: Pointer to buffer to search
- *name_length*: Length of name
- *http_type_string*: Pointer to extracted HTML type string
- *http_type_string_max_size*: Size of the http_type_string buffer size
- *string_size*: Pointer to return extracted HTML type string

length.

### Return Values

- **NX_SUCCESS** (0x00) Successful extraction of type
- NX_PTR_ERROR (0x07) Invalid pointer input
- **NX_WEB_HTTP_EXTENSION_MIME_DEFAULT** (0x30019) Default "text/plain" returned.

### Allowed From

Application

### Example

```C
/* my_server is a previously created HTTP server, which starts accepting client
    requests when *nx_web_http_server_start* is called */

CHAR temp_string[20];
UINT string_length;
UINT ret;

/* Extract the HTTP type. */
ret = nx_web_http_server_type_get_extended(&my_server_ptr,
    my_server.nx_web_http_server_request_resource,
    strlen(my_server.nx_web_http_server_request_resource),
    temp_string,sizeof(temp_string), &string_length);

/* If string_length is non zero, the HTTP string is extracted. */
    For a more detailed example, see the description for
    *nx_web_http_server_callback_generate_response_header.*
```

## nx_web_http_server_digest_authenticate_notify_set

Set digest authenticate callback function

### Prototype

```C
UINT nx_web_http_server_digest_authenticate_notify_set(
    NX_WEB_HTTP_SERVER *http_server_ptr,
    UINT (*digest_authenticate_callback)(
        NX_WEB_HTTP_SERVER *server_ptr,
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

```C
UINT digest_authenticate_callback(NX_WEB_HTTP_SERVER *server_ptr, CHAR *name_ptr,
    CHAR *realm_ptr, CHAR *password_ptr, CHAR *method,
    CHAR *authorization_uri, CHAR *authorization_nc,
    CHAR *authorization_cnonce)
{
    return(NX_SUCCESS);
}

NX_WEB_HTTP_SERVER my_server;

/* After the HTTP server is created by calling nx_web_http_server_create, and
    before starting HTTP services when nx_web_http_server_start is called, set the digest authenticate callback: */
status = nx_web_http_server_digest_authenticate_notify_set(&my_server,
    digest_authenticate_callback);

/* If status equals NX_SUCCESS, the digest_authenticate_callback function
    will be called when the HTTP server performs digest authenticate. */
```

## nx_web_http_server_authenticate_check_set

Set digest authenticate callback function

### Prototype

```C
UINT nx_web_http_server_digest_authenticate_notify_set(
    NX_WEB_HTTP_SERVER *http_server_ptr,
    UINT (*authentication_check_extended)(
        NX_WEB_HTTP_SERVER *server_ptr,
        UINT request_type,
        CHAR *resource,
        CHAR **name,
        UINT *name_length,
        CHAR **password,
        UINT password_length,
        CHAR **realm,
        UINT *realm_length));
```

### Description

This service sets the callback invoked when authenticate check is performed.

### Input Parameters

- *http_server_ptr*: Pointer to HTTP Server instance
- *authentication_check_extended*: Pointer to authenticate check callback

### Return Values

- **NX_SUCCESS** (0x00) Successfully set the callback
- NX_PTR_ERROR (0x07) Invalid pointer input

### Allowed From

Application

### Example

```C
UINT authenticate_check_callback(NX_WEB_HTTP_SERVER *server_ptr,
    UINT request_type,
    CHAR *name_ptr, UCHAR *resource, UCHAR **name,
    UINT *name_length, UCHAR **password,
    UINT *password_length, UCHAR **realm,
    UINT *realm_length)
{
    *name = "name";
    *name_length = 4;
    *password = "password";
    *password_length = 8;
    *realm = "realm";
    *realm_length = 5;
    return(NX_SUCCESS);
}

NX_WEB_HTTP_SERVER my_server;

/* After the HTTP server is created by calling nx_web_http_server_create, and
    before starting HTTP services when nx_web_http_server_start is called, set the authenticate check callback: */

status = nx_web_http_digest_authenticate_check_set (&my_server,
    authenticate_check_callback);

/* If status equals NX_SUCCESS, the authenticate_check_callback function
    will be called when the HTTP server performs authenticate check. */
```

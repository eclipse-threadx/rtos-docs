---
title: Chapter 1 - Introduction to HTTP and HTTPS
description: This chapter introduces the NetX Duo HTTP/HTTPS for Web module.
---

# Chapter 1 - Introduction to HTTP and HTTPS

The Hypertext Transfer Protocol (HTTP) is a protocol designed for transferring content on the Web. HTTP is a simple protocol that utilizes reliable Transmission Control Protocol (TCP) services to perform its content transfer function. Because of this, HTTP is a highly reliable content transfer protocol. HTTP is one of the most used application protocols. All operations on the Web utilize the HTTP protocol.

HTTPS is the secure version of the HTTP protocol, which implements HTTP using Transport Layer Security (TLS) to secure the underlying TCP connection. Other than the additional configuration required to set up TLS, HTTPS is basically identical to HTTP in use.

## General HTTP Requirements

In order to function properly, the NetX Duo Web HTTP package requires that NetX Duo (version 5.10 or later) is installed. In addition, an IP instance must be created, and TCP must be enabled on that same IP instance. For HTTPS support, NetX Duo Secure TLS (version 5.11 or later) must also be installed (see next section). The demo file in section "Small Example System" in **Chapter 2** demonstrates how this is done.

The HTTP Client portion of the NetX Duo Web HTTP package has no further requirements.

The HTTP Server portion of the NetX Duo Web HTTP package has several additional requirements. First, it requires complete access to TCP *well-known port 80* for handling all Client HTTP requests (this can be changed by the application to any other valid TCP port). The HTTP Server is also designed for use with the FileX embedded file system. If FileX is not available, the user may port the portions of FileX used to their own environment. This is discussed in later sections of this guide.

## HTTPS Requirements

For HTTPS to function properly, the NetX Duo Web HTTP package requires that NetX Duo (version 5.10 or later) and NetX Duo Secure TLS (version 5.11 or later) are both installed. In addition, an IP instance must be created, and TCP must be enabled on that same IP instance for use with TLS. The TLS session will need to be initialized with appropriate cryptographic routines, a trusted CA certificate, and space must be allocated for certificates that will be provided by remote server hosts during the TLS handshake. The demo file in section "Small Example HTTPS System" in **Chapter 2** will demonstrate how this is done.

The HTTPS Client portion of the NetX Duo Web HTTP package has no further requirements.

The HTTPS Server portion of the NetX Duo Web HTTP package has several additional requirements. First, it requires complete access to TCP *well-known port 443* for handling all Client HTTPS requests (as with plaintext HTTP, this port can be changed by the application). Second, the TLS session will need to be initialized with proper cryptographic routines and a server identity certificate (or Pre-Shared Key). The HTTPS Server is also designed for use with the FileX embedded file system. If FileX is not available, the user may port the portions of FileX used to their own environment. The use of FileX is discussed in later sections of this guide.

Refer to the NetX Duo Secure documentation for more information on configuration options for TLS.

Unless noted, all HTTP functionality described in this document also applies to HTTPS.

## HTTP and HTTPS Constraints

NetX Duo Web HTTP implements the HTTP 1.1 standard. However, here are following constraints:

1. Request pipelining is not supported
1. The HTTP Server supports both basic and MD5 digest authentication, but not MD5-sess. At present, the HTTP Client supports only basic authentication. When using TLS for HTTPS, HTTP authentication may still be used.
1. No content compression is supported.
1. TRACE, OPTIONS, and CONNECT requests are not supported.
1. The packet pool associated with the HTTP Server or Client must be large enough to hold the complete HTTP header.
1. HTTP Client services are for content transfer only—there are no display utilities provided in this package.

## HTTP URL (Resource Names)

The HTTP protocol is designed to transfer content on Web. The requested content is specified by the Universal Resource Locator (URL). This is the primary component of every HTTP request. URLs always start with a "/" character and typically correspond to files on the HTTP Server. Common HTTP file extensions are shown below:

- **.htm** (or **.html**) Hypertext Markup Language (HTML)
- **.txt** Plain ASCII text
- **.gif** Binary GIF image
- **.xbm** Binary Xbitmap image

## HTTP Client Requests

The HTTP has a simple mechanism for requesting Web content. There is a set of standard HTTP commands that are issued by the Client after a connection has been successfully established on the TCP *well-known port 80 (port 443 for HTTPS)*. The following shows some of the basic HTTP commands:

- **GET *resource* HTTP/1.1** Get the specified resource
- **POST *resource* HTTP/1.1** Get the specified resource and pass attached input to the HTTP Server
- **HEAD *resource* HTTP/1.1** Treated like a GET but not content is returned by the HTTP Server
- **PUT *resource* HTTP/1.1** Place resource on HTTP Server
- **DELETE *resource* HTTP/1.1** Delete resource on the Server

These ASCII commands are generated internally by Web browsers and the NetX Duo Web HTTP Client services to perform HTTP operations with an HTTP Server.

Note that the HTTP Client application should use port 80, or port 443 if HTTPS is used. Both the Client and Server HTTP APIs take the port as a parameter – the macros NX_WEB_HTTP_SERVER_PORT (port 80) and NX_WEB_HTTPS_SERVER_PORT (port 443) are defined for convenience. The HTTP Server port can also be changed at runtime using the *nx_web_http_client_set_connect_port()* service. See Chapter 4 for more details on this service.

## HTTP Server Responses

The HTTP Server utilizes the same *well-known TCP port 80 (443 for HTTPS)* to send Client command responses. Once the HTTP Server processes the Client command, it returns an ASCII response string that includes a 3-digit numeric status code. The numeric response is used by the HTTP Client software to determine whether the operation succeeded or failed. Following is a list of various HTTP Server responses to Client commands:

- **200** Request was successful
- **400** Request was not formed properly
- **401** Unauthorized request, client needs to send authentication
- **404** Specified resource in request was not found
- **500** Internal HTTP Server error
- **501** Request not implemented by HTTP Server
- **502** Service is not available

For example, a successful Client request to PUT the file "test.htm" is responded with the message "HTTP/1.1 200 OK."

## HTTP Communication

As mentioned previously, the HTTP Server utilizes the *well-known TCP port 80 (443 for HTTPS)* to field Client requests. HTTP Clients may use any available TCP port for outgoing connections. The general sequence of HTTP events is as follows:

**HTTP GET Request**:

1. Client issues TCP connect to Server port 80 (or 443 for HTTPS).
1. If HTTPS is being used, the TCP connection is followed by a TLS handshake to authenticate the server and establish a secure channel.
1. Client sends "**GET *resource* HTTP/1.1**" request (along with other header information).
1. Server builds an "**HTTP/1.1 200 OK**" message with additional information followed immediately by the resource content (if any).
1. Server disconnects from the client (TLS is shut down if HTTPS is being used).
1. Client disconnects from the socket (TLS is shut down following the disconnection alert from the server).

**HTTP PUT Request**:

1. Client issues TCP connect to Server port 80 (or 443).
1. If HTTPS is being used, the TCP connection is followed by a TLS handshake to authenticate the server and establish a secure channel.
1. Client sends "PUT resource HTTP/1.1" request, along with other header information, and followed by the resource content.
1. Server builds an "HTTP/1.1 200 OK" message with additional information followed immediately by the resource content.
1. Server performs a disconnection.
1. Client performs a disconnection.

> **Note:** As mentioned previously, the HTTP Server can change the default connect port (80 or 443) at runtime another port using the *nx_web_http_client_set_connect_port()* for web servers that use alternate ports to connect to clients.

## HTTP Authentication

HTTP authentication is optional and is not required for all Web requests. There are two flavors of authentication, namely *basic* and *digest*. Basic authentication is equivalent to the *name* and *password* authentication found in many protocols. In HTTP basic authentication, the name and passwords are concatenated and encoded in the base64 format. The main disadvantage of basic authentication is the name and password are transmitted openly in the request. This makes it somewhat easy for the name and password to be stolen. Digest authentication addresses this problem by never transmitting the name and password in the request. Instead, an algorithm is used to derive a 128-bit digest from the name, password, and other information. The NetX Duo Web HTTP Server supports the standard MD5 digest algorithm.

When is authentication required? The HTTP Server decides if a requested resource requires authentication. If authentication is required and the Client request did not include the proper authentication, a "HTTP/1.1 401 Unauthorized" response with the type of authentication required is sent to the Client. The Client is then expected to form a new request with the proper authentication.

When HTTPS is used, the HTTPS Server can still utilize HTTP authentication. In this case, TLS is used to encrypt all HTTP traffic so using *basic* HTTP authentication does not pose a security risk. *Digest* authentication is also permitted but provides no significant security improvement over basic authentication over TLS.

## HTTP Authentication Callback

As mentioned before, HTTP authentication is optional and isn't required on all Web transfers. In addition, authentication is typically resource dependent. Access of some resources on the Server require authentication, while others do not. The NetX Duo Web HTTP Server package allows the application to specify (via the ***nx_web_http_server_create*** call) an authentication callback routine that is called at the beginning of handling each HTTP Client request.

The callback routine provides the NetX Duo Web HTTP Server with the username, password, and realm strings associated with the resource and return the type of authentication necessary. If no authentication is necessary for the resource, the authentication callback should return the value of **NX_WEB_HTTP_DONT_AUTHENTICATE**. Otherwise, if basic authentication is required for the specified resource, the routine should return **NX_WEB_HTTP_BASIC_AUTHENTICATE**. And finally, if MD5 digest authentication is required, the callback routine should return **NX_WEB_HTTP_DIGEST_AUTHENTICATE**. If no authentication is required for any resource provided by the HTTP Server, the callback is not needed, and a NULL pointer can be provided to the HTTP Server create call.

The format of the application authenticate callback routine is very simple and is defined below:

```C
UINT nx_web_http_server_authentication_check(NX_WEB_HTTP_SERVER *server_ptr,
    UINT request_type, CHAR *resource,
    CHAR **name, CHAR **password,
    CHAR **realm);
```

The input parameters are defined as follows:

- **request_type** Specifies the HTTP Client request, valid requests are defined as:
  - **NX_WEB_HTTP_SERVER_GET_REQUEST**
  - **NX_WEB_HTTP_SERVER_POST_REQUEST**
  - **NX_WEB_HTTP_SERVER_HEAD_REQUEST**
  - **NX_WEB_HTTP_SERVER_PUT_REQUEST**
  - **NX_WEB_HTTP_SERVER_DELETE_REQUEST**
- **resource** Specific resource requested.
- **name** Destination for the pointer to the required username.
- **password** Destination for the pointer to the required password.
- **realm** Destination for the pointer to the realm for this authentication.

The return value of the authentication routine specifies if authentication is required. name, password, and realm pointers are not used if **NX_WEB_HTTP_DONT_AUTHENTICATE** is returned by the authentication callback routine. Otherwise the HTTP server developer must ensure that **NX_WEB_HTTP_MAX_USERNAME** and **NX_WEB_HTTP_MAX_PASSWORD** defined in *nx_web_http_server.h* are large enough for the username and password specified in the authentication callback. These both have a default size of 20 characters.

## HTTP Invalid Username/Password Callback

The optional invalid username/password callback in the NetX Duo Web HTTP Server is invoked if the HTTP server receives an invalid username and password combination in a Client request. If the HTTP server application registers a callback with HTTP server it will be invoked if either basic or digest authentication fails *in nx_web_http_server_get_process()*, in *nx_web_http_server_put_process(),* or *in nx_web_http_server_delete_process().*

To register a callback with the HTTP server, the following service is defined for the NetX Duo Web HTTP Server.

```C
UINT nx_web_http_server_invalid_userpassword_notify_set(
    NX_WEB_HTTP_SERVER *http_server_ptr,
    UINT (*invalid_username_password_callback)
        (CHAR *resource, ULONG *client_nx_address,
        UINT request_type));
```

The request types are defined as follows:

- **NX_WEB_HTTP_SERVER_GET_REQUEST**
- **NX_WEB_HTTP_SERVER_POST_REQUEST**
- **NX_WEB_HTTP_SERVER_HEAD_REQUEST**
- **NX_WEB_HTTP_SERVER_PUT_REQUEST**
- **NX_WEB_HTTP_SERVER_DELETE_REQUEST**

## HTTP Insert GMT Date Header Callback

There is an optional callback in the NetX Duo Web HTTP Server to insert a date header in its response messages. This callback is invoked when the HTTP Server is responding to a put or get request

To register a GMT date callback with the HTTP Server, the following service is defined.

```C
UINT nx_web_http_server_gmt_callback_set(
    NX_WEB_HTTP_SERVER *server_ptr,
    VOID (*gmt_get)(NX_WEB_HTTP_SERVER_DATE *date);
```

The NX_WEB_HTTP_SERVER_DATE data type is defined as follows:

```C
typedef struct NX_WEB_HTTP_SERVER_DATE_STRUCT
{
    USHORT nx_web_http_server_year; /* Year */
    UCHAR nx_web_http_server_month; /* Month */
    UCHAR nx_web_http_server_day; /* Day */
    UCHAR nx_web_http_server_hour; /* Hour */
    UCHAR nx_web_http_server_minute; /* Minute */
    UCHAR nx_web_http_server_second; /* Second */
    UCHAR nx_web_http_server_weekday; /* Weekday */
} NX_WEB_HTTP_SERVER_DATE;
```

## HTTP Cache Info Get Callback

The HTTP Server has a callback to request the maximum age and date from the HTTP application for a specific resource. This information is used to determine if the HTTP server sends an entire page in response to a Client Get request. If the "if modified since" in the Client request is not found or does not match the "last modified" date returned by the get cache callback, the entire page is sent.

To register the callback with the HTTP server the following service is defined:

```C
UINT nx_web_http_server_cache_info_callback_set(
    NX_WEB_HTTP_SERVER *server_ptr,
    UINT (*cache_info_get)
    (CHAR *, UINT *, NX_WEB_HTTP_SERVER_DATE *));
```

## HTTP Chunked Transfer Coding Support

When the total length of HTTP message cannot be determined before sending it, the Chunked Transfer Coding feature can be used to send messages as series of chunks without the "Content-Length" header field. This feature is supported in all HTTP request and response messages. As a receiver, this feature is supported, and the chunk header is processed transparently by internal logic. As a sender, the API *nx_web_http_client_request_chunked_set* and *nx_web_http_server_response_chunked_set* must be invoked by client and server respectively.

```C
UINT nx_web_http_client_request_chunked_set(NX_WEB_HTTP_CLIENT *client_ptr,
    UINT chunk_size,
    NX_PACKET *packet_ptr);

UINT nx_web_http_server_response_chunked_set(NX_WEB_HTTP_SERVER *server_ptr,
    UINT chunk_size,
    NX_PACKET *packet_ptr);
```

For more details on these services, see their descriptions in Chapter 3 "Description of HTTP Services".

## HTTP Multipart Support

Multipurpose Internet Mail Extensions (MIME) was originally intended for the SMTP protocol, but its use has spread to HTTP. MIME allows messages to contain mixed message types (e.g. image/jpg and text/plain) within the same message. The NetX Duo Web HTTP Server has services to determine content type in HTTP messages containing MIME from the Client. To enable HTTP multipart support and use these services, the configuration option **NX_WEB_HTTP_MULTIPART_ENABLE** must be defined.

```C
UINT nx_web_http_server_get_entity_header(NX_WEB_HTTP_SERVER *server_ptr,
    NX_PACKET **packet_pptr,
    UCHAR *entity_header_buffer,
    ULONG buffer_size);

UINT nx_web_http_server_get_entity_content(NX_WEB_HTTP_SERVER *server_ptr,
    NX_PACKET **packet_pptr,
    ULONG *available_offset,
    ULONG *available_length);
```

For more details on the use of these services, see their description in Chapter 3 "Description of HTTP Services".

## HTTP Multi-Thread Support

The NetX Duo Web HTTP Client services can be called from multiple threads simultaneously. However, read or write requests for a particular HTTP Client instance should be done in sequence from the same thread.

If using HTTPS, NetX Duo Web HTTP Client services may be called from multiple threads but due to the added complexity of the underlying TLS functionality each thread should have a single, independent HTTP Client instance (NX_WEB_HTTP_CLIENT control structure).

## HTTP RFCs

NetX Duo Web HTTP is compliant with RFC1945 "Hypertext Transfer Protocol/1.0", RFC 2616 "Hypertext Transfer Protocol – HTTP/1.1", RFC 2581 "TCP Congestion Control", RFC 1122 "Requirements for Internet Hosts", and related RFCs.

For HTTPS, NetX Duo Web HTTP is compliant with RFC 2818 "HTTP over TLS".

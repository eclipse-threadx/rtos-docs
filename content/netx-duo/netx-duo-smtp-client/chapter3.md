---
title: Chapter 3 - Client description of SMTP Client services
description: This chapter contains a description of all NetX Duo SMTP Client services (listed below) in order of usage in a typical SMTP Client application.
---


This chapter contains a description of all NetX Duo SMTP Client services (listed below) in order of usage in a typical SMTP Client application.

> **Note:** In the **Return Values** section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## nxd_smtp_client_create

Create an SMTP Client Instance

### Prototype

```C
UINT nxd_smtp_client_create(NX_SMTP_CLIENT *client_ptr,
    NX_IP *ip_ptr, NX_PACKET_POOL
    *client_packet_pool_ptr,
    CHAR *username, CHAR *password,
    CHAR *from_address,
    CHAR *client_domain,
    UINT authentication_type, NXD_ADDRESS *server_address,
    UINT port);
```

### Description

This service creates an SMTP Client instance on the specified IP instance.

### Input Parameters

- *client_ptr*: Pointer to SMTP Client control block;
- *ip_ptr*: Pointer to IP instance;
- *packet_pool_ptr*: Pointer to Client packet pool;
- *username*: NULL-terminated Username for authentication
- *password*: NULL-terminated password for authentication
- *from_address*: NULL-terminated sender's address
- *client_domain*: NULL-terminated domain name
- *authentication_type*: Client authentication type. Supported types are:
  - NX_SMTP_CLIENT_AUTH_LOGIN
  - NX_SMTP_CLIENT_AUTH_PLAIN
  - NX_SMTP_CLIENT_AUTH_NONE
- *server_address*: Pointer to SMTP Server IP address
- *server_port*: SMTP Server TCP port

### Return Values

- **NX_SUCCESS** (0x00) SMTP Client successfully created. TCP socket creation status
- NX_SMTP_INVALID_PARAM (0xA5) Invalid non pointer input
- NX_IP_ADDRESS_ERROR (0x21) Invalid IP address type
- NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From

Application Code

### Example

```C
/* Create the SMTP Client instance. */
NX_PACKET_POOL client_packet_pool;
NX_IP client_ip;
NX_SMTP_CLIENT demo_client;

#define USERNAME "myusername"
#define PASSWORD "mypassword"
#define FROM_ADDRESS "<myname@mycompany.com>"
#define LOCAL_DOMAIN "mycompany.com"
#define SERVER_PORT 25

/* Define client authentication type as LOGIN. 
    If not specified or unknown the SMTP Client will set it to PLAIN. */
#define CLIENT_AUTHENTICATION_TYPE NX_SMTP_CLIENT_AUTH_LOGIN

NXD_ADDRESS server_ip_address;

#ifdef USE_IPV6
    /* Set up the Server IPv6 address. */
    server_ip_address.nxd_ip_version = NX_IP_VERSION_V6;
    server_ip_address.nxd_ip_address.v6[0] = 0x20010db8;
    server_ip_address.nxd_ip_address.v6[1] = 0xf101;
    server_ip_address.nxd_ip_address.v6[2] = 0;
    server_ip_address.nxd_ip_address.v6[3] = 0x106;
#else
    server_ip_address.nxd_ip_version = NX_IP_VERSION_V4;
    server_ip_address.nxd_ip_address.v4 = SERVER_IP_ADDRESS;
#endif

status = nxd_smtp_client_create(&demo_client, &client_ip, &client_packet_pool,
    USERNAME, PASSWORD, FROM_ADDRESS,
    LOCAL_DOMAIN, CLIENT_AUTHENTICATION_TYPE,
    &server_ip_address, SERVER_PORT);

/* If an SMTP Client instance was successfully created, status = NX_SUCCESS. */
```

## nx_smtp_client_delete

Delete an SMTP Client Instance

### Prototype

```C
UINT nx_smtp_client_delete(NX_SMTP_CLIENT *client_ptr);
```

### Description

This service deletes a previously created SMTP Client instance.

### Input Parameters

- *client_ptr*: Pointer to SMTP Client instance.

### Return Values

- **NX_SUCCESS** (0x00) Client successfully deleted
- NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From

Threads

### Example

```C
/* Delete the SMTP Client instance "my_client." */

NX_SMTP_CLIENT demo_client;

status = nx_smtp_client_delete(&demo_client);

/* If an SMTP Client instance was successfully deleted, status = NX_SUCCESS. */
```

## nx_smtp_mail_send

Create and send an SMTP mail item

### Prototype

```C
UINT nx_smtp_mail_send(NX_SMTP_CLIENT *client_ptr,
    CHAR *recipient_address,
    UINT priority, CHAR *subject,
    CHAR *mail_body,
    UINT mail_body_length);
```

### Description

This service creates and sends an SMTP mail item. The SMTP Client establishes a TCP connection with the SMTP Server and sends a series of SMTP commands. If no errors are encountered, it will transmit the mail message to the Server. Regardless if the mail is sent successfully it will terminate the TCP connection and return a status indicating outcome of the mail transmission. The application may call this service for as many mail messages as it needs to send without limit.

### Input Parameters

- *client_ptr*: Pointer to SMTP Client
- *recipient_address*: NULL-terminated recipient address.
- *subject*: NULL-terminated subject line text;.
- *priority*: Priority level at which mail is delivered
- *mail_body*: Pointer to mail message
- *mail_body_length*: Size of mail message

### Return Values

- **NX_SUCCESS** (0x00) Mail successfully sent
- **NX_SMTP_CLIENT_NOT_INITIALIZED** (0xB2) SMTP Client instance not initialized for SMTP session status Outcome of SMTP session
- NX_PTR_ERROR (0x07) Invalid pointer parameter
- NX_SMTP_INVALID_PARAM (0xA5) Invalid non pointer input
- NX_CALLER_ERROR (0x11) Invalid caller of this service

### Allowed From

Threads

### Example

```C
/* Create and send a Client mail item. */

#define RECIPIENT_ADDRESS "<your@yourcompany.com>"
#define SUBJECT "NetX Duo SMTP Client Demo"
#define MAIL_BODY "NetX Duo SMTP client is an SMTP client " \
    "implementation for embedded devices \r\n" \
    "to send email to SMTP servers.\r\n"

status = nx_smtp_mail_send(&demo_client, RECIPIENT_ADDRESS,
    NX_SMTP_MAIL_PRIORITY_NORMAL,
    SUBJECT_LINE, MAIL_BODY,
    sizeof(MAIL_BODY) - 1);

/* Return status being NX_SUCCESS indicates the mail has been
    successfully sent. */
```

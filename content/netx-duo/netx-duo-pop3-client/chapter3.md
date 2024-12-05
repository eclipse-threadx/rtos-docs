---
title: Chapter 3 - Description of POP3 Client services
description: This chapter contains a description of all NetX Duo POP3 Client services (listed below) in alphabetical order.
---


This chapter contains a description of all NetX Duo POP3 Client services (listed below) in alphabetical order.

> **Note:** In the **Return Values** section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## nx_pop3_client_create

Create a POP3 Client instance for IPv4

### Prototype

```C
UINT nx_pop3_client_create(
    NX_POP3_CLIENT *client_ptr,
    UINT APOP_authentication,
    NX_IP *ip_ptr,
    NX_PACKET_POOL *packet_pool_ptr,
    ULONG server_ip_address,
    ULONG server_port,
    CHAR *client_name,
    CHAR *client_password);
```

### Description

This service creates an instance of the POP3 Client. It supports only IPv4 POP3 server addresses.

Note that the device application must first create an IP instance and a packet pool for the POP3 Client to transmit packets. This packet pool created for use exclusively by the POP3 Client task or the same packet pool used in the IP instance creation. The packet pool may also be shared with the Ethernet driver packet pool but this has the disadvantage of using large packet pools whose payload is intended for receiving potentially large packets payload for the POP3 Client to send relatively small POP3 message packets to the server.

### Input Parameters

- *client_ptr*: Pointer to Client to create
- *APOP_authentication*: Enable APOP authentication
- *ip_ptr Pointer*: to IP instance
- *packet_pool_ptr*: Pointer to Client packet pool
- *server_ip_address*: POP3 server IPv4 address
- *server_port*: POP3 server port
- *client_name*: Pointer to Client name
- *client_password*: Pointer to Client password

### Return Values

- **NX_SUCCESS** (0x00) Client successfully created
- **status** Status completion of NetX Duo and ThreadX service calls
- NX_PTR_ERROR (0x07) Invalid input pointer parameter
- NX_POP3_PARAM_ERROR (0xB1) Invalid non pointer input

### Allowed From

Application code

### Example

```C
/* Create the POP3 Client. Note that the Client uses its password for its APOP shared
    secret. */

/* Set up user defined callback services. */
/* Create username and password for our POP3 Client mail drop. */
#define LOCALHOST "recipient@expresslogic.com"
#define LOCALHOST_PASSWORD "pass"
#define POP3_SERVER_ADDRESS IP_ADDRESS(192,2,2,92)
#define POP3_SERVER_PORT 110

/* Create a NetX POP3 Client instance. */
status = nx_pop3_client_create(&demo_client,
    NX_FALSE /* disable APOP authentication */,
    &client_ip, &client_packet_pool,
    POP3_SERVER_ADDRESS, POP3_SERVER_PORT,
    LOCALHOST, LOCALHOST_PASSWORD);

/* If the Client was successfully created, status = NX_SUCCESS. */
```

## nxd_pop3_client_create

Create a POP3 Client instance for IPv4 or IPv6

### Prototype

```C
UINT nxd_pop3_client_create(
    NX_POP3_CLIENT *client_ptr,
    UINT APOP_authentication,
    NX_IP *ip_ptr,
    NX_PACKET_POOL *packet_pool_ptr,
    NXD_ADDRESS *server_ip_address,
    ULONG server_port,
    CHAR *client_name,
    CHAR *client_password);
```

### Description

This service creates an instance of the POP3 Client. It supports both IPv4 and IPv6 POP3 server addresses. See the previously described *nx_pop3_client_create* service for more details on POP3 Client create process.

### Input Parameters

- *client_ptr*: Pointer to Client to create
- *APOP_authentication*: Enable APOP authentication
- *ip_ptr*: Pointer to IP instance
- *packet_pool_ptr*: Pointer to Client packet pool
- *server_ip_address*: POP3 server IPv6 or IPv4 address
- *server_port*: POP3 server port
- *client_name*:  Pointer to Client name
- *client_password*: Pointer to Client password

### Return Values

- **NX_SUCCESS** (0x00) Client successfully created
- **status** Status completion of NetX Duo and ThreadX service calls
- NX_PTR_ERROR (0x07) Invalid input pointer parameter
- NX_POP3_PARAM_ERROR (0xB1) Invalid non pointer input

### Allowed From

Application code

### Example

```C
/* Create the POP3 Client. */

/* Create username and password for our POP3 Client mail drop. */
#define LOCALHOST "recipient@expresslogic.com"
#define LOCALHOST_PASSWORD "pass"
#define POP3_SERVER_PORT 110

/* Create a NetX POP3 Client instance. Also note the details of IPv6 address validation
    and enabling IPv6 and ICMPv6 services on the IP task are not shown here. See the
    NetX Duo User Guide for more details on this process.
*/
NXD_ADDRESS server_ip_address;

/* Set client IP interface address. */
server_ip_address.nxd_ip_version = NX_IP_VERSION_V6;
server_ip_address.nxd_ip_address.v6[0] = 0x20010db8;
server_ip_address.nxd_ip_address.v6[1] = 0xf101;
server_ip_address.nxd_ip_address.v6[2] = 0;
server_ip_address.nxd_ip_address.v6[3] = 0x101;
status = nxd_pop3_client_create(&demo_client,
    NX_FALSE /* disable APOP authentication */,
    &client_ip, &client_packet_pool,
    &server_ip_address, POP3_SERVER_PORT,
    LOCALHOST, LOCALHOST_PASSWORD);

/* If the Client was successfully created, status = NX_SUCCESS. */
```

## nx_pop3_client_delete

Delete a POP3 Client instance

### Prototype

```C
UINT nx_pop3_client_delete(NX_POP3_CLIENT *client_ptr);
```

### Description

This service deletes a previously created POP3 Client. Not that this service does not delete the POP3 Client packet pool. The device application must delete this resource separately if it no longer has use for the packet pool.

### Input Parameters

- *client_ptr*: Pointer to Client to delete

### Return Values

- **NX_SUCCESS** (0x00) Client successfully deleted
- NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From

Application code

### Example

```C
/* Delete the POP3 Client. */
status = nx_pop3_client_delete (&demo_client);

/* If the Client was successfully deleted, status = NX_SUCCESS. */
```

## nx_pop3_client_mail_item_delete

Delete a specified mail item from the Client maildrop

### Prototype

```C
UINT nx_pop3_client_mail_items_delete(
    NX_POP3_CLIENT *client_ptr,
    UINT mail_index);
```

### Description

This service deletes the specified mail item from the Client maildrop. It is intended for after downloading the mail item, although some POP3 servers may automatically delete mail items after being requested by the Client.

### Input Parameters

- *client_ptr*: Pointer to Client instance
- *mail_index*: Index into Client maildrop

### Return Values

- **NX_SUCCESS** (0x00) Delete request successful
- **NX_POP3_INVALID_MAIL_ITEM**(0xB2) Invalid mail item index
- **NX_POP3_INSUFFICIENT_PACKET_PAYLOAD**(0xB6) Client packet payload too small for POP3 request.
- **NX_POP3_SERVER_ERROR_STATUS**(0xB4) Server replies with error status
- NX_POP3_CLIENT_INVALID_INDEX(0xB8) Invalid mail index input
- NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From

Application code

### Example

```C
ULONG item_index;

/* Delete the POP3 Client mail item. */
status = nx_pop3_client_mail_item_delete(&demo_client, item_index);

/* If the server accepts the DELE request (and deletes the mail item), status =
    NX_SUCCESS. */
```

## nx_pop3_client_mail_item_get

Retrieve a specified mail item

### Prototype

```C
UINT nx_pop3_client_mail_item_get(
    NX_POP3_CLIENT *client_ptr,
    UINT mail_item,
    ULONG *item_size);
```

### Description

This service makes a RETR request to retrieve a mail item from the Client maildrop specified by the index mail_item. After making a RETR request, and receiving a positive response from the Server, the Client can start downloading the mail message using the *nx_pop3_client_mail_item_message_get* service. Note that the service also supplies the size of the requested mail item extracted from the Server reply.

### Input Parameters

- *client_ptr*: Pointer to Client instance
- *mail_item*: Index into Client maildrop
- *item_size*: Pointer to size of mail message

### Return Values

- **NX_SUCCESS** (0x00) Mail item successfully retrieved
- **NX_POP3_INVALID_MAIL_ITEM** (0xB2) Invalid mail item index
- **NX_POP3_INSUFFICIENT_PACKET_PAYLOAD** (0xB6) Client packet payload too small for POP3 request.
- **NX_POP3_SERVER_ERROR_STATUS** (0xB4) Server replies with error status
- NX_POP3_CLIENT_INVALID_INDEX (0xB8) Invalid mail index input
- NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From

Application code

### Example

```C
ULONG item_size;

/* Retrieve the POP3 Client mail item. */
status = nx_pop3_client_mail_item_get (&demo_client, 1, &item_size);

/* If the mail item was successfully obtained, status = NX_SUCCESS. */
```

## nx_pop3_client_mail_items_get

Retrieve the number of mail items in maildrop

### Prototype

```C
UINT nx_pop3_client_mail_items_get(
    NX_POP3_CLIENT *client_ptr,
    UINT *number_mail_items,
    ULONG *maildrop_total_size);
```

### Description

This service makes a STAT request to retrieve the number of mail items and total size of mail message data from the Client maildrop.

### Input Parameters

- *client_ptr*: Pointer to Client instance
- *number_mail_item*: Number of mail in Client maildrop
- *maildrop_total_size*: Pointer to size of all mail message

### Return Values

- **NX_SUCCESS** (0x00) Mail item successfully retrieved
- **NX_POP3_INVALID_MAIL_ITEM** (0xB2) Invalid mail item index
- **NX_POP3_INSUFFICIENT_PACKET_PAYLOAD** (0xB6) Client packet payload too small for POP3 request.
- **NX_POP3_SERVER_ERROR_STATUS** (0xB4) Server replies with error status
- NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From

Application code

### Example

```C
UINT number_mail_items;

ULONG maildrop_total_size;

/* Retrieve the size and number of items in POP3 Client maildrop. */

status = nx_pop3_client_mail_item_get (&demo_client, 1, &number_mail_items,
    &maildrop_total_size);

/* If the maildrop data was successfully obtained, status = NX_SUCCESS. */
```

## nx_pop3_client_mail_item_message_get

Retrieve the specified mail item message

### Prototype

```C
UINT nx_pop3_client_mail_item_message_get(
    NX_POP3_CLIENT *client_ptr,
    NX_PACKET **recv_packet_ptr,
    ULONG *bytes_retrieved,
    UINT *final_packet);
```

### Description

This service retrieves the mail item message, size of the mail message, and if it is the last packet in the mail message. If final_packet is NX_TRUE the packet pointed to by recv_packet_ptr is the final packet in the mail item message.

### Input Parameters

- *client_ptr*: Pointer to Client instance
- *recv_packet_ptr*: Received packet of message data
- *number_mail_item*: Number of mail in Client maildrop
- *maildrop_total_size*: Pointer to size of all mail message

### Return Values

- **NX_SUCCESS** (0x00) Mail item successfully retrieved
- **NX_POP3_CLIENT_INVALID_STATE** (0xB7) Client packet payload too small for POP3 request.
- NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From

Application code

### Example

```C
NX_PACKET *recv_packet_ptr;

ULONG bytes_retrieved;

UINT final_packet;

/* Retrieve the size and number of items in POP3 Client maildrop. */

status = nx_pop3_client_mail_item_message_get (&demo_client, &recv_packet_ptr,
    bytes_retrieved, final_packet);

/* If the maildrop message packet was successfully obtained, status = NX_SUCCESS. */
```

## nx_pop3_client_mail_item_size_get

Retrieve the size of the specified mail item

### Prototype

```C
UINT nx_pop3_client_mail_item_size_get(
    NX_POP3_CLIENT *client_ptr,
    UINT mail_item, ULONG *size);
```

### Description

This service makes a LIST request to obtain the size of the specified mail item.

### Input Parameters

- *client_ptr*: Pointer to Client instance
- *mail_item*: Index into Client maildrop
- *size*: Pointer to size of mail message

### Return Values

- **NX_SUCCESS** (0x00) Mail item successfully retrieved
- **NX_POP3_INVALID_MAIL_ITEM** (0xB2) Invalid mail item index
- **NX_POP3_INSUFFICIENT_PACKET_PAYLOAD** (0xB6) Client packet payload too small for POP3 request.
- **NX_POP3_SERVER_ERROR_STATUS** (0xB4) Server replies with error status
- NX_POP3_CLIENT_INVALID_INDEX (0xB8) Invalid mail index input
- NX_PTR_ERROR (0x07) Invalid input pointer parameter

### Allowed From

Application code

### Example

```C
ULONG size;

UINT mail_item;

/* Retrieve the size of the specified mail item in POP3 Client maildrop. */

status = nx_pop3_client_mail_item_size_get (&demo_client, mail_item, &size);

/* If the maildrop message packet was successfully obtained, status = NX_SUCCESS. */
```

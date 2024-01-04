---
title: Chapter 1 - Introduction to NetX Duo POP3
description: The NetX Duo POP3 Client API is designed to provide a mail transport system for small workstations to access Client maildrops on POP3 Servers for retrieving Client mail. 
---

# Chapter 1 - Introduction to NetX Duo POP3

The Post Office Protocol Version 3 (POP3) is a protocol designed to provide a mail transport system for small workstations to access Client maildrops on POP3 Servers for retrieving Client mail. POP3 utilizes Transmission Control Protocol (TCP) services to perform mail transfer. Because of this, POP3 is a highly reliable content transfer protocol.

However, POP3 is does not provide extensive operations on mail handling. Typically, mail is downloaded by the Client and then deleted from the Server's maildrop.

## NetX Duo POP3 Client Requirements

### Client Requirements

The NetX POP3 Client API requires a previously created NetX Duo IP instance using *nx_ip_create* and a previously created NetX packet pool using *nx_packet_pool_create*. Because the NetX Duo POP3 Client utilizes TCP services, TCP must be enabled with the *nx_tcp_enable* call prior to using the NetX Duo POP3 Client API on that same IP instance. The POP3 Client uses a TCP socket to connect to a POP3 Server on the Server's POP3 port. This is typically set at the *well-known port 110, though neither POP3 Client nor Server are required to use this port.*

The size of the packet pool used in creating the POP3 Client is user configurable in terms of packet payload and number of packets available. If the packet is used only in the POP3 Client create service, the packet payload need not be more than 100-120 bytes depending on the length of username and password, or APOP digest. The USER command with the local host's user name is probably the largest message sent by the POP3 Client. It is possible to share the same packet pool in the nx_ip_create (IP default packet pool) since the IP internal operations do not require very large packet payload for sending and receiving TCP control data.

However, it is not generally advantageous for the Ethernet driver to use the same packet pool as the POP3 Client packet pool. Generally, the payload of the receive packet pool payload is set the IP instance MTU (typically 1500 bytes) of the network interface which is much larger than POP3 Client messages. Incoming POP3 messages would usually be much larger data size then outgoing POP3 Client messages

## NetX Duo POP3 Client Creation

There are two services for creating the POP3 Client. The recommended service is *nxd_pop3_client_create* which takes an NXD_ADDRESS address data type that accepts IPv4 or IPv6 addresses for the POP3 server. The other POP3 Client create service, *nx_pop3_client_create*, only accepts IPv4 addresses for the POP3 server. Both services bind the TCP socket port and connect with the POP3 server.

After connecting with the POP3 server, the POP3 Client application can call *nx_pop3_client _mail_items_get* to obtain the number of mail items sitting in its maildrop box:

```C
UINT nx_pop3_client_mail_items_get(NX_POP3_CLIENT *client_ptr,
    UINT *number_mail_items, ULONG *maildrop_total_size);
```

If one or more items are in the Client maildrop, the application can obtain the size of a specific mail item, using the *nx_pop3_client_get_mail_item* service:

```C
UINT nx_pop3_client_mail_item_get(NX_POP3_CLIENT *client_ptr,
    UINT mail_item, ULONG *item_size);
```

The first mail item in the maildrop is at index 1.

To get the actual mail message, the application can call the *nx_pop3_client_mail_item_get_message_data* service to retrieve the mail message packets until the service indicates the last packet is received by the final_packet input argument:

```C
UINT nx_pop3_client_mail_item_message_get(
    NX_POP3_CLIENT *client_ptr,
    NX_PACKET **recv_packet_ptr,
    ULONG *bytes_retrieved,
    UINT *final_packet);
```

To delete a specific mail item, the application calls *nx_pop3_client_mail_item_delete* with the same index as used in the preceding *nx_pop3_client_get_mail_item* call.

The Client can be deleted using the *nx_pop3_client_delete* service. Note it is up to the application to delete the POP3 Client packet pool using the *nx_packet_pool_delete* service there is no longer has any use for it.

## NetX Duo POP3 Client Constraints

There are some constraints in the NetX Duo POP3 Client implementation:

1. The NetX Duo POP3 Client does not support the AUTH command although it does implement APOP authentication using DIGEST-MD5 for the Client Server authentication exchange.
2. NetX Duo POP3 Client does not implement all POP3 commands (e.g. the TOP or UIDL commands). Below is a list of commands it does support:
   - NOOP
   - RSET

## NetX Duo POP3 Client Login

A NetX Duo POP3 Client must authenticate itself (login) to a POP3 Server to access a maildrop. It can do so either by using the USER/PASS commands and providing a username and password known to the POP3 Server, or by using the APOP command and MD5 digest described below.

The username is typically a fully qualified domain name (contains a local-part and a domain name, separated by an '@' character). When using the POP3 commands USER and PASS, the Client is sending its username and password unencrypted over the Internet.

To avoid the security risk of clear texting username and password, the NetX Duo POP3 Client can be configured to use APOP authentication by setting the *APOP_authentication* parameter in the *nxd_pop3_client_create* service:

```C
UINT nxd_pop3_client_create(NX_POP3_CLIENT *client_ptr,
    UINT APOP_authentication,
    NX_IP *ip_ptr,
    NX_PACKET_POOL *packet_pool_ptr,
    NXD_ADDRESS *server_ip_address, ULONG server_port,
    CHAR *client_name,
    CHAR *client_password);
```

Or for IPv4 only applications, the *nx_pop3_client_create* service:

```C
UINT  nx_pop3_client_create(NX_POP3_CLIENT *client_ptr,
    UINT APOP_authentication, NX_IP *ip_ptr,
    NX_PACKET_POOL *packet_pool_ptr,
    ULONG server_ip_address,
    ULONG server_port, CHAR *client_name,
    CHAR *client_password);
```

When the Client sends the APOP command, it takes as its only argument an MD5 digest containing the server domain, local time and process ID extracted from the Server greeting, plus the Client password. The POP3 Server will create an MD5 digest containing the same information and if its MD5 digest matches the Client's MD5 digest, the Client is authenticated.

If APOP authentication fails, NetX Duo POP3 Client will attempt USER/PASS authentication.

## The POP3 Client Maildrop

Client mail is stored on a POP3 Server in a mailbox or "maildrop." A Client maildrop on a POP3 Server is represented as a 1 based list of mail items. That is, each mail is referred to by its index in the maildrop list with the first mail item at index 1 (not zero). POP3 commands refer to specific mail items by their index in this list.

## The POP3 Protocol State Machine

The POP3 protocol requires that both Client and Server maintain the state of the POP3 session. First, the Client attempts to connect to the POP3 Server. If successful it enters into the POP3 protocol which has three distinct states defined by RFC 1939. The initial state is the Authorization state in which it must identify itself to the Server. In the Authorization state, the POP3 Client can only issue the USER and the PASS commands, and in that order, or the APOP command.

Once the POP3 Client is authenticated, the Client session enters the Transaction state. In this state, the Client can download and request mail deletion. The commands allowed in the Transaction state are LIST, STAT, RETR, DELE, RSET and QUIT. Typically the POP3 Client sends a STAT command followed by a series of RETR commands (one for each mail item in its maildrop).

Once the Client issues the QUIT command, the POP3 session enters the Update state in which it initiates the TCP disconnect from the Server. To download mail at another time, the POP3 Client application can call nx_pop3_client_mail_items_get to check for new mail in the maildrop.

### POP3 Server Reply Codes

- +OK The Server uses this reply to accept a Client command. The Server can include additional information after the '+OK' but cannot assume the Client will process this information, except in the case of downloading mail message data or the LIST or DELE commands. In the latter case, the 'argument' after the command references the index of the mail item in the Client maildrop.
- -ERR The Server uses this reply to reject a Client command. The Server may send additional information following the '-ERR' but cannot assume the Client will process this information.

### Sample POP3 Client - Server Session

**Basic POP3 example using USER/PASS:**

```
S: <wait for connection on TCP port 110>
C: <open connection>
S: +OK POP3 server ready <1896.697170952@dbc.mtview.ca.us>
C: USER mrose
S: +OK mrose is valid
C: PASS mvan99
S: +OK mrose is logged in
C: STAT
S: +OK 2 320
C: RETR 1
S: +OK 120 octets
S: <the POP3 server sends message 1>
S: .
C: DELE 1
S: +OK message 1 deleted
C: RETR 2
S: +OK 200 octets
S: <the POP3 server sends message 2>
S: .
C: DELE 2
S: +OK message 2 deleted
C: QUIT
S: +OK POP3 server signing off (maildrop empty)
C: <close connection>
S: <wait for next connection>
```

**Basic POP3 example using APOP (and LIST instead of STAT):**

```
S: <wait for connection on TCP port 110>
C: <open connection>
S: +OK POP3 server ready <1896.697170952@dbc.mtview.ca.us>
C: APOP mrose c4c9334bac560ecc979e58001b3e22fb
S: +OK mrose's maildrop has 2 messages (320 octets)
C: LIST
S: +OK 2 messages (320 octets)
S: 1 120
S: 2 200
S: .
C: RETR 1
S: +OK 120 octets
S: <the POP3 server sends message 1>
S: .
C: DELE 1
S: +OK message 1 deleted
C: RETR 2
S: +OK 200 octets
S: <the POP3 server sends message 2>
S: .
C: DELE 2
S: +OK message 2 deleted
C: QUIT
S: +OK dewey POP3 server signing off (maildrop empty)
C: <close connection>
S: <wait for next connection>
```

## RFCs Supported by NetX Duo POP3 Client

NetX Duo Client POP3 is compliant with RFC 1939.

---
title: Chapter 1 - Introduction to NetX Duo SMTP client
description: The Simple Mail Transfer Protocol (SMTP) is a protocol for transferring mail across networks and the Internet.
---


The Simple Mail Transfer Protocol (SMTP) is a protocol for transferring mail across networks and the Internet. It utilizes the reliable Transmission Control Protocol (TCP) services to perform its content transfer function.

## NetX Duo SMTP Client Requirements

The NetX Duo SMTP Client requires creation of a NetX Duo IP instance and NetX Duo packet pool. The SMTP Client uses a TCP socket to connect to an SMTP Server on the *well-known port 25. Therefore*, TCP must first be enabled by calling the *nx_tcp_enable* service on a previously created IP instance.

The SMTP Client create call (nxd_smtp_client_create) requires a previously created packet pool for transmitting SMTP commands to the Server as well as for sending the actual mail message. Packet payload depends on the anticipated size of the mail contents and must allow for TCP, IP header, and MAC header. (Note that the IPv6 header is 40 bytes while the IPv4 header is 20 bytes.)

If the entire mail message cannot fit in one packet, the SMTP Client allocates additional packets to contain the rest of the message.

## NetX Duo SMTP Client Constraints

While the NetX Duo SMTP protocol implements the RFC 2821 and 2554 standards, there are some constraints:

1. The NetX Duo SMTP Client supports only LOGIN and PLAIN authentication, but not CRAM-MD5 digest authentication.
2. The NetX Duo SMTP Client messages are limited to one recipient per mail item, and only one mail message per TCP connection with the SMTP server.
3. VRFY, SEND, SOML, EXPN, SAML, ETRN, TURN and SIZE SMTP options are not supported.
4. The SMTP Client is not mail browser ("mail user agent") which is typically used for creating the mail message. It is a "mail transfer agent" only. It will provide the necessary processing of the mail message body for SMTP transport as specified in RFC 2821. It does not check the contents for correct syntax e.g. the recipient and reverse pathway. There is no restriction what is in the mail buffer e.g. MIME data or clear text messages. Mail message format, specified in RFC 2822 for including headers and message body is beyond the scope of the SMTP Client API.

## Commands Supported by NetX Duo SMTP Client

The NetX Duo SMTP Client uses the following commands during a mail session with an SMTP Server.

- **EHLO** The Client would like to initiate a session that includes some or all extension protocol SMTP services available from the SMTP Server. This is the default.
- **HELO** The Client would like to initiate a session limited to basic SMTP services.
- **MAIL** The Client would like the Server to receive Client mail.
- **AUTH** The Client would like to initiate authentication by the Server.
- **RCPT** The Client would like to submit a mailbox of another host it would like the mail to be delivered to.
- **DATA** The Client would like to initiate sending mail message data to the Server.
- **QUIT** The Client would like to terminate the session.

## Getting Started

The SMTP Client application creates an IP instance and an enables TCP on that IP instance. It then creates the SMTP Client using the following service:

```C
UINT nxd_smtp_client_create(NX_SMTP_CLIENT *client_ptr,
    NX_IP *ip_ptr, NX_PACKET_POOL
    *client_packet_pool_ptr,
    CHAR *username, CHAR *password,
    CHAR *from_address,
    CHAR *client_domain,
    UINT authentication_type,
    NXD_ADDRESS *server_address, UINT port);
```

The *client_packet_pool_ptr* is a pointer to a previously created packet pool the SMTP Client will use to send messages to the SMTP Server.

Note that an application must provide a *from_address* for the local device and a server IP address. All addresses must be fully qualified domain names. A fully qualified domain name contains a local-part and a domain name, separated by an '@' character. Note that the SMTP Client does not check the syntax of the *from_address* or the *recipient_address* in the nx_smtp_mail_send service below.

After the SMTP Client is created, the SMTP Client application creates a mail item with a properly formatted SMTP mail message, and makes the mail item send request to the SMTP Client using the following API:

```C
UINT nx_smtp_mail_send(NX_SMTP_CLIENT *client_ptr,
    CHAR *recipient_address, UINT priority,
    CHAR *subject, CHAR *mail_body,
    UINT mail_body_length);
```

There is essentially no difference in running SMTP Client over IPv4 or IPv6 from user perspective. Differences between the two IP protocols are handled in the underlying NetX Duo layer.

Note that an application wishing to send mail must provide a recipient address in the *nx_smtp_client_mail* call.

For authentication, usernames can either be fully qualified domain names, or display user names. This depends on how the Server performs authentication.

The demo in the Small Example section later in this User Guide shows how the message should be formatted. The status if the mail item was successfully sent will be NX_SUCCESS. If an error occurs, whether it is an internal error, a broken TCP connection or receiving a Server reply error code, *nx_smtp_mail_send* will return a non-zero error status.

When sending a mail item, NetX Duo SMTP Client creates a new TCP connection with the SMTP server and begins an SMTP session. In this session, the Client sends a series of commands to the SMTP Server as part of the SMTP protocol, culminating in sending out the actual mail message. The TCP connection is then terminated, regardless of the outcome of the SMTP session.

After mail transmission, regardless of success or failure, the SMTP Client is returned to the 'initial' state, and can be used for another mail transfer session.

## NetX Duo SMTP Authentication

Authentication is a way for SMTP Clients to prove their identity to the SMTP Server and have their mail delivered as trusted users. Most commercial SMTP Servers require that Clients be authenticated.

Typically, authentication data consists of the sender's username and password. During an authentication challenge, the Server prompts for this information and the Client responds by sending the requested data in encoded format. The Server decodes the data and attempts to find a match in its user database. If found, the Server indicates the authentication is successful. SMTP authentication is defined in [RFC 2554](http://www.ietf.org/rfc/rfc2554.txt).

There are two flavors of authentication, namely *basic* and *digest*. Digest is not supported in the current NetX Duo SMTP Client, and will not be discussed here. Basic authentication is equivalent to the *name* and *password* authentication described above. In SMTP basic authentication, the name and passwords are base64 encoded. The advantage of basic authentication is its ease of implementation and widespread use. The main disadvantage of basic authentication is name and password data is transmitted openly in the request.

### Plain Authentication

The NetX Duo SMTP Client sends an AUTH command with the PLAIN parameter. If the NetX Duo SMTP Server supports this type of authentication, it will reply with a 334 reply code. The Client replies with a single base64 encoded username and password message to the Server. If the Server determines the Client authentication is successful, it responds with the 235 success code.

### Login Authentication

The NetX Duo SMTP Client sends an AUTH command with the LOGIN parameter. If the NetX Duo SMTP Server supports this type of authentication, it will reply with a 334 reply code as the start of the authentication 'challenge'. It sends a base64 encoded prompt back to the Client which is typically "Username". The Client decodes the prompt, and replies with a base64 encoded username. If the Server accepts the Client username, it sends out a base64 encoded prompt for the Client password. The Client responds with a base64 encoded password. If the Server determines the Client authentication is successful, it responds with the 235 success code.

### No Authentication

Some SMTP Servers are configured without authentication. If so, their 250 response to the Client EHLO message will not list any authentication types. However, no authentication types listed does not necessarily mean the Server does not require or support authentication. If the Client is configured for PLAIN or LOGIN authentication in this situation, the NetX Duo Client thread task will default to PLAIN. If the Client is configured for NONE, the authentication step is skipped and the SMTP state advances to the MAIL state.

Note that if the Client is configured for no authentication and the SMTP Server does support authentication, the Client authentication type is switched to PLAIN.

## RFCs Supported by NetX Duo SMTP Client

NetX Duo SMTP Client API is compliant with RFC2821 "Simple Mail Transfer Protocol" and RFC 2554 "SMTP Service Extension for Authentication."

---
title: Chapter 1 - Introduction to NetX Duo Telnet
description: NetX Duo Telnet is a protocol designed for transferring commands and responses between two nodes on the internet.
---


NetX Duo Telnet is a protocol designed for transferring commands and responses between two nodes on the internet. Telnet is a simple protocol that utilizes reliable Transmission Control Protocol (TCP) services to perform its transfer function. Because of this, Telnet is a highly reliable transfer protocol. Telnet is also one of the most used application protocols.

## Telnet Requirements

In order to function properly, the NetX Duo Telnet package requires that a NetX IP instance has already been created. In addition, TCP must be enabled on that same IP instance. The Telnet Client portion of the NetX Duo Telnet package has no further requirements.

The Telnet Server portion of the NetX Duo Telnet package has one additional requirement. It requires complete access to TCP *well-known port 23* for handling all Client Telnet requests.

## Telnet Constraints 

The NetX Duo Telnet protocol implements the Telnet standard. However,the interpretation and response of Telnet commands, indicated by a byte with the value of 255, is the responsibility of the application. The various Telnet commands and command parameters are defined in the *nxd_telnet_client.h and nxd_telnet_server.h* files.

## Telnet Communication

As mentioned previously, the Telnet Server utilizes the *well-known TCP port 23* to field Client requests. Telnet Clients may use any available TCP port.

## Telnet Authentication

Telnet authentication is the responsibility of the application's Telnet Server callback function. The application's Telnet Server "new connection" callback would typically prompt the Client for name and/or password. The Client would then be responsible for providing the information. The Server would then process the information in the "receive data" callback. This is where the application Server code would have to authenticate the information and decide whether or not it is valid.

## Telnet New Connection Callback

The NetX Duo Telnet Server calls the application specified callback function whenever a new Telnet Client request is received. The application specifies the callback function when the Telnet Server is created via the ***nx_telnet_server_create*** function. Typical actions of the "new connection" callback include sending a banner or prompt to the Client. This could very well include a prompt for login information.

### Prototype

```c
void  telnet_new_connection(NX_TELNET_SERVER *server_ptr, 
                            UINT logical_connection);
```

### Input Parameters

- **server_ptr**: Pointer to the calling Telnet Server.
- **logical_connection**: The internal logical connection for the Telnet Server. This can be used by the application as an index into buffers and/or data structures specific for each Client connection. Its value ranges from 0 through NX_TELNET_MAX_CLIENTS - 1.

## Telnet Receive Data Callback

The NetX Duo Telnet Server calls the application specified callback function whenever a new Telnet Client data is received. The application specifies the callback function when the Telnet Server is created via the ***nx_telnet_server_create*** function. Typical actions of the "new connection" callback include echoing the data back and/or parsing the data and providing data as a result of interpreting a command from the client.

Note that this callback routine must also release the supplied packet.

### Prototype

```c
void  telnet_receive_data(NX_TELNET_SERVER *server_ptr, 
                          UINT logical_connection, NX_PACKET *packet_ptr);
```
### Input Parameters

- **server_ptr**: Pointer to the calling Telnet Server.
- **logical_connection**: The internal logical connection for the Telnet Server. This can be used by the application as an index into buffers and/or data structures specific for each Client connection. Its value ranges from 0 through NX_TELNET_MAX_CLIENTS - 1.
- **packet_ptr**: Pointer to packet containing the data from the Client.

## Telnet End Connection Callback

The NetX Duo Telnet Server calls the application specified callback function whenever a Telnet Client ends the connection. The application specifies the callback function when the Telnet Server is created via the ***nx_telnet_server_create*** function. Typical actions of the "end connection" callback include cleaning up any Client specific data structures associated with the logical connection.

### Prototype
```c
void  telnet_end_connection(NX_TELNET_SERVER *server_ptr, 
                            UINT logical_connection);
```

### Input Parameters

- **server_ptr** Pointer to the calling Telnet Server.
- **logical_connection** The internal logical connection for the Telnet Server. This can be used by the application as an index into buffers and/or data structures specific for each Client connection. Its value ranges from 0 through NX_TELNET_MAX_CLIENTS - 1.

## Telnet Option Negotiation

The NetX Duo Telnet Server supports a limited set of Telnet options, Echo and Suppress Go Ahead.

To enable this feature the NX_TELNET_SERVER_OPTION_DISABLE must not be defined. By default it is not defined. The Telnet Server creates a packet pool in the *nx_telnet_server_create* service from which it allocates packets for sending telnet options requests to the Client. See "Configuration Options" for setting the packet payload (NX_TELNET_SERVER_PACKET_PAYLOAD) and packet pool size (NX_TELNET_SERVER_PACKET_POOL_SIZE) for this packet pool. It will delete this packet pool when the *nx_telnet_server_delete* service is called.

Upon making a connection with the Telnet Client, it will send out this set of telnet options to the Client if it has not received option requests from the Client:

- will echo
- dont echo
- will sga

When it receives Telnet data from the Client, the Telnet Server checks if the first byte is the "IAC" code. If so, it will process all the options in the Client packet. Options not in the list above are ignored.

By default, the Telnet Server creates its own internal packet pool if NX_TELNET_SERVER_OPTION_DISABLE is not defined and it needs to transmit Telnet option commands. The Telnet Server packet pool is defined by NX_TELNET_SERVER_PACKET_PAYLOAD and NX_TELNET_SERVER_PACKET_POOLSIZE. If, however, NX_TELNET_SERVER_USER_CREATE_PACKET_POOL is defined, the application must create the Telnet Server packet pool and set it as the Telnet Server packet pool by calling *_nx_telnet_server_packet_pool_set*. See Chapter 3 "Description of Telnet Services" for more details about this function.

Unlike the NetX Duo Telnet Server, the NetX Duo Telnet Client task thread does not automatically send and respond to received options from the Telnet Server. This must be done by the Telnet Client application.

## Telnet Multi-Thread Support

The NetX Duo Telnet Client services can be called from multiple threads simultaneously. However, read or write requests for a particular Telnet Client instance should be done in sequence from the same thread.

## Telnet RFCs

NetX Duo Telnet is compliant with RFC854 and related RFCs.

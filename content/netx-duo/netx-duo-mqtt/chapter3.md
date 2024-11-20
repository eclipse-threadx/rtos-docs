---
title: Chapter 3 - Description of NetX Duo MQTT Client Services
description: This chapter contains a description of all NetX Duo MQTT Client services (listed below) in alphabetical order.
---


This chapter contains a description of all NetX Duo MQTT client services (listed below) in alphabetical order.

In the "Return Values" section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## nxd_mqtt_client_create

Create MQTT Client Instance

### Prototype

```c
UINT nxd_mqtt_client_create(
    NXD_MQTT_CLIENT *client_ptr,
    CHAR *client_name,
    CHAR *client_id,
    UINT client_id_length,
    NX_IP *ip_ptr,
    NX_PACKET_POOL
    *pool_ptr,
    VOID *stack_ptr,
    ULONG stack_size, UINT
    mqtt_thread_priority,
    VOID *memory_ptr,
    ULONG memory_size);
```

### Description

This service creates an MQTT Client instance on the specified IP instance. The *client_id* string is passed to the server during MQTT connection phase as the *Client Identifier (ClientId)*. It also creates the necessary ThreadX resources (MQTT Client task thread, mutex, event flag group, and TCP socket).

### Input Parameters

- *client_ptr*: Pointer to MQTT Client control block.
- *client_name*: Client name string.
- *client_id*: Client ID string used during connection phase. MQTT broker uses this client_id to uniquely identify a client.
- *client_id_length*: Length of the client ID string, in bytes.
- *ip_ptr*: Pointer to IP instance.
- *pool_ptr*: Pointer to a packet pool MQTT client uses for its operation.
- *stack_ptr*: Stack area for the MQTT Client thread.
- *stack_size*: Size of the stack area, in bytes.
- *mqtt_thread_priority*: The priority of the MQTT Thread.
- *memory_ptr*: Deprecated. Not used anymore.
- *memory_size*: Deprecated. Not used anymore.

### Return Values

- **NXD_MQTT_SUCCESS** (0x00) Successfully created MQTT client.
- **NXD_MQTT_INTERNAL_ERROR** (0x10004) Internal logic error
- NX_PTR_ERROR (0x07) Invalid MQTT control block, ip_ptr, or packet pool pointer.
- NXD_MQTT_INVALID_PARAMETER (0x10009) Invalid will topic string, will_retrain_flag, or will_QoS value.

### Allowed From

Threads

### Example

```c
#define CLIENT_ID_STRING "My Test Client"

#define MQTT_THREAD_PRIORITY 2

NXD_MQTT_CLIENT my_client;
NX_IP ip_0; /* Assume ip_0 is created prior to MQTT client creation. */
NX_PACKET_POOL pool_0;/* Assume pool_0 is created prior to MQTT client creation. */
UCHAR mqtt_thread_stack[STACK_SIZE];

/* Create the MQTT Client instance on "ip_0". */
status = **nxd_mqtt_client_create**(&my_client, "my client",
    CLIENT_ID_STRING, stlren(CLIENT_ID_STRING),
    &ip_0, &pool_0, (VOID*)mqtt_thread_stack, STACK_SIZE,
    MQTT_THREAD_PRIORITY, NX_NULL, 0);

/* If status is NXD_MQTT_SUCCESS an MQTT Client instance was successfully created. */
```

## nxd_mqtt_client_will_message_set

Sets the Will message

### Prototype

```c
UINT nxd_mqtt_client_will_message_set(
    NXD_MQTT_CLIENT *client_ptr,
    Const UCHAR *will_topic,
    UINT will_topic_length
    Const UCHAR *will_message,
    UINT will_message_length,
    UINT will_retain_flag,
    UINT will_QoS);
```

### Description

This service sets the optional will topic and will message before the client connects to the server. Will topic must be UTF-8 encoded string.

The will message, if set, is transmitted to the broker as part of the CONNECT message. Therefore application wishing to use will message must use this service before the MQTT connection is make.

### Input Parameters

- *client_ptr*: Pointer to MQTT Client control block.
- *will_topic*: UTF-8 encoded will topic string. Will topic must be present. Caller must keep the will_topic string valid till the *nx_mqtt_client_connect* call is made.
- *will_topic_length*: Number of bytes in the will topic string
- *will_message*: Application defined will message. If will message is not required, application can set this field to *NX_NULL.*
- *will_message_length*: Number of bytes in the will message string. If will_message is set to NULL, will_message_length must be set to 0.
- *will_retain_flag*: Whether the server publishes the will message as a retained message. Valid values are *NX_TRUE* or *NX_FALSE.*
- *will_QoS*: QoS value used by the server when sending will message. Valid values are 0 or 1.  

### Return Values

- **NXD_MQTT_SUCCESS** (0x00) Successfully sets the will message.
- **NXD_MQTT_QOS2_NOT_SUPPORTED** (0x1000C) QoS level 2 messages are not supported.
- NX_PTR_ERROR (0x07) Invalid MQTT control block.
- NXD_MQTT_INVALID_PARAMETER (0x10009) Invalid will topic string, will_retrain_flag, or will_QoS value.

### Allowed From

Threads

### Example

```c
#define WILL_TOPIC "my_will_topic"

#define WILL_MESSAGE "my will message"

/* Create the MQTT Client instance "my_client" on "ip_0". */
status = nxd_mqtt_client_will_message_set(&my_client,
    WILL_TOPIC, STRLEN(WILL_TOPIC),
    WILL_MESSAGE, STRLEN(WILL_MESSAGE),
    NX_TRUE, 0);

/* If status is NXD_MQTT_SUCCESS the will message is properly
configured for the session. It will be transmitted to the server
during MQTT connection. */
```

## nxd_mqtt_client_login_set

Sets MQTT client login username and password

### Prototype

```c
UINT nxd_mqtt_client_login_set(
    NXD_MQTT_CLIENT *client_ptr,
    Const UCHAR *username,
    UINT username_length
    Const UCHAR *password,
    UINT password_length);
```

### Description

This service sets the username and password, which is used during MQTT connection phase for log in authentication purpose.

The MQTT client login with username and password is optional. In situations where the server requires a user name and password, the user name and password must be set before the connection is established.

### Input Parameters

- *client_ptr*: Pointer to MQTT Client control block.
- *username*: UTF-8 encoded user name string. Caller must keep the username string valid till the *nx_mqtt_client_connect* call is made.
- *username_length*: Number of bytes in the username string
- *password*: Password string. If password is not required, this field may be set to NX_NULL.

### Return Values

- **NXD_MQTT_SUCCESS** (0x00) Successfully sets the will message.
- NX_PTR_ERROR (0x07) Invalid MQTT control block.
- NXD_MQTT_INVALID_PARAMETER (0x10009) Invalid username string or the password string.

### Allowed From

Threads

### Example

```c
#define USERNAME "MY_NAME"

#define PASSWORD "MY_LOGIN_SECRET"

/* Create the MQTT Client instance "my_client" on "ip_0". */
status = nxd_mqtt_client_login_set(&my_client,
    USERNAME, STRLEN(USERNAME),
    PASSWORD, STRLEN(PASSWORD));

/* If status is NXD_MQTT_SUCCESS the username and the password 
are set for the session. This information will be 
transmitted to the server during MQTT connection. */
```

## nxd_mqtt_client_connect

Connect MQTT Client to the broker

### Prototype

```c
UINT nxd_mqtt_client_connect(
    NXD_MQTT_CLIENT *client_ptr,
    NXD_ADDRESS *server_ip,
    UINT server_port,
    UINT keepalive, UINT clean_session,
    ULONG wait_option));
```

### Description

This service initiates a connection to the broker. First it binds a TCP socket, then makes a TCP connection. Assuming that succeeds, it creates a timer if the MQTT keep alive feature is enabled. Then it connects with the MQTT server (broker).

Note that this service creates an MQTT connection with no TLS protection. To create a secure MQTT connection, the application shall use the service ***nxd_mqtt_client_secure_connect.***

Upon the connection, if the client sets the *clean_session* to NX_FALSE, the client will retransmit any messages stored that have not been acknowledged yet.

### Input Parameters

- *client_ptr*: Pointer to MQTT Client control block.
- *server_ip*: Broker IP address.
- *server_port*: Broker port number. The default port for MQTT is defined as **NXD_MQTT_PORT** (1883).
- *keep_alive*: The keep alive value, in seconds, to be used during the session. The value indicates the maximum time between two MQTT control messages being sent to the broker before the broker times out this client. The value 0 turns off the keep-alive feature.
- *clean_session*: Whether the server shall start this session clean. Valid options are **NX_TRUE** or **NX_FALSE.**
- *wait_option*: Connection wait time.

### Return Values

- **NXD_MQTT_SUCCESS** (0x00) Successful MQTT connection
- **NXD_MQTT_ALREADY_CONNECTED** (0x10001) The client is already connected to the broker.
- **NXD_MQTT_MUTEX_FAILURE** (0x10003) Failed to obtain MQTT mutex
- **NXD_MQTT_INTERNAL_ERROR** (0x10004) Internal logic error
- **NXD_MQTT_CONNECT_FAILURE** (0x10005) Failed to connect to the broker.
- **NXD_MQTT_COMMUNICATION_FAILURE** (0x10007) Unable to send messages to the broker.
- **NXD_MQTT_SERVER_MESSAGE_FAILURE** (0x10008) Server responded with error
- **NXD_MQTT_ERROR_UNACCEPTABLE_PROTOCOL** (0x10081) Server response code
- **NXD_MQTT_ERROR_IDENTIFIER_REJECTED** (0x10082) Server response code
- **NXD_MQTT_ERROR_SERVER_UNAVAILABLE** (0x10083) Server response code
- **NXD_MQTT_ERROR_BAD_USERNAME_PASSWORD** (0x10084) Server response code
- **NXD_MQTT_ERROR_NOT_AUTHORIZED** (0x10085) Server response code
- NX_PTR_ERROR (0x07) Invalid MQTT control block, ip_ptr, or packet pool pointer

### Allowed From

Threads

### Example

```c
NXD_ADDRESS broker_address;

/* Set up broker IP address */
broker_address.nxd_ip_version = 4;
broker_address.nxd_ip_address.v4 = MQTT_BROKER_ADDRESS;

/* Create the MQTT Client instance "my_client" on "ip_0". */
status = nxd_mqtt_client_connect(&my_client, &broker_address,
    NXD_MQTT_PORT,
    0, /* Turn off keepalive */
    NX_TRUE, /* Clean session flag set */
    NX_WAIT_FOREVER);

/* If status is NXD_MQTT_SUCCESS a connection to the broker is successfully established. */
```

## nxd_mqtt_client_secure_connect

Connect MQTT client to the broker with TLS security

### Prototype

```c
UINT nxd_mqtt_client_secure_connect(
    NXD_MQTT_CLIENT *client_ptr,
    NXD_ADDRESS *server_ip,
    UINT server_port,
    UINT (*tls_setup)(
        NXD_MQTT_CLIENT *,
        NX_SECURE_TLS_SESSION *,
        NX_SECURE_TLS_CERTIFICATE *,
        NX_SECURE_TLS_CERTIFICATE *),
    UINT keepalive,
    UINT connection_flag,
    UINT clean_session,
    ULONG wait_option));
```

### Description

This service is identical to ***nxd_mqtt_client_connect*** except that the connection goes through TLS layer instead of TCP. Therefore, communication between the client and the broker is secured.

The user-defined *tls_setup* is a callback function that the MQTT client uses prior to making a MQTT client connection. The application shall initialize NetX Duo Secure TLS, configure security parameters, and load relevant certificates to be used during TLS handshake. The actual TLS handshake happens after a TCP connection is established on the broker's MQTT TLS port (default TCP port 8883). Once the TLS handshake is successful, the MQTT CONNECT control packet is sent via TLS.

To make secure connections, the NetX Duo Secure TLS library must be available, and the NetX Duo MQTT client must be built with **NX_SECURE_ENABLE** defined.

### Input Parameters

- *client_ptr*: Pointer to MQTT Client control block.
- *server_ip*: Broker IP address.
- *server_port*: Broker port number. The default port for MQTT is defined as **NXD_MQTT_TLS_PORT**: (8883).
- *tls_setup*: User-provided TLS Setup callback function. This callback function is invoked to set up TLS client connection parameters.
- *keep_alive*: The keep-alive value to be used during the session. The value 0 turns off the keep-alive feature.
- *clean_session*: Whether or not the server shall start this session clean. Valid options are **NX_TRUE** or **NX_FALSE.**
- *wait_option*: Connection wait time.

### Return Values

- **NXD_MQTT_SUCCESS** (0x00) Successful MQTT client connection established via TLS.
- **NXD_MQTT_ALREADY_CONNECTED** (0x10001) The client is already connected to the broker.
- **NXD_MQTT_MUTEX_FAILURE** (0x10003) Failed to obtain MQTT mutex 
- **NXD_MQTT_INTERNAL_ERROR** (0x10004) Internal logic error
- **NXD_MQTT_CONNECT_FAILURE** (0x10005) Failed to connect to the broker.
- **NXD_MQTT_COMMUNICATION_FAILURE** (0x10007) Unable to send messages to the broker.
- **NXD_MQTT_SERVER_MESSAGE_FAILURE** (0x10008) Server responded with error message.
- **NXD_MQTT_ERROR_UNACCEPTABLE_PROTOCOL** (0x10081) Server response code
- **NXD_MQTT_ERROR_IDENTIFIER_REJECTED** (0x10082) Server response code
- **NXD_MQTT_ERROR_SERVER_UNAVAILABLE** (0x10083) Server response code
- **NXD_MQTT_ERROR_BAD_USERNAME_PASSWORD** (0x10084) Server response code
- **NXD_MQTT_ERROR_NOT_AUTHORIZED** (0x10085) Server response code
- NX_PTR_ERROR (0x07) Invalid MQTT control block or sever address structure.
- NX_INVALID_PORT (0x46) Server port cannot be 0.
- NXD_MQTT_INVALID_PARAMETER (0x10009) Input parameter error
- NXD_MQTT_CLIENT_NOT_RUNNING (0x1000E) MQTT Thread has not started running yet.

### Allowed From

Threads

### Example

```c
/* TLS setup routine. This function is responsible for setting up TLS parameters.*/
UINT tls_setup_callback(NXD_MQTT_CLIENT *client_ptr,
    NX_SECURE_TLS_SESSION *session_ptr,
    NX_SECURE_TLS_CERTIFICATE *certificate_ptr,
    NX_SECURE_TLS_CERTIFICATE *trusted_certificate,
    UINT timeout)
{
/* Note this routine is simplified to highlight the
necessary steps to setup a TLS session. Each
application may employ different procedures suitable for
its TLS settings, such as cipher suite, certificates. */

/* Create a TLS session for the MQTT connection, and pass
in various crypto methods this session can use for the
initial TLS handshake. */

/* Load appropriate certificates, or set up session key for Pre-share key operation. */

/* Start the TLS session */

/* Return NX_SUCCESS if the TLS session is established. */
    return(NX_SUCCESS);
}

NXD_ADDRESS broker_address;

/* Set up broker IP address */
broker_address.nxd_ip_version = 4;
broker_address.nxd_ip_address.v4 = MQTT_BROKER_ADDRESS;

/* Create the MQTT Client instance "my_client" on "ip_0". */
status = nxd_mqtt_client_secure_connect(&my_client,
    &server_address, NXD_MQTT_TLS_PORT, tls_setup_callback,
    0, /* Turn off keepalive */
    NX_TRUE, /* Clean session set */
    NX_WAIT_FOREVER);

/* If status is NXD_MQTT_SUCCESS the MQTT Client was successfully connected to the broker via TLS. */
```

## nxd_mqtt_client_publish

Publish a message through the broker

### Prototype

```c
UINT nxd_mqtt_client_publish(
    NXD_MQTT_CLIENT *client_ptr,
    CHAR *topic_name,
    UINT topic_name_length,
    CHAR *message, UINT
    message_length,
    UINT retain,
    UINT QoS,
    ULONG timeout);
```

### Description

This service publishes a message through the broker. Publishing QoS level 2 messages is not supported yet.

### Input Parameters

- *client_ptr*: Pointer to MQTT Client control block.
- *topic_name*: Topic to publish to.
- *topic_name_length*: Length of the topic, in bytes.
- *message*: Pointer to the message buffer.
- *message_length*: Size of the message, in bytes
- *retain*: Determines if the broker shall retain the message.
- *QoS*: The desired QoS value: 0 or 1.
- *timeout*: Timeout value

### Return Values

- **NXD_MQTT_SUCCESS** (0x00) Successful MQTT Client create
- **NXD_MQTT_INTERNAL_ERROR** (0x10004) Internal logic error.
- **NXD_MQTT_PACKET_POOL_FAILURE** (0x10006) Failed to obtain packet from the packet pool.
- **NXD_MQTT_COMMUNICATION_FAILURE** (0x10007) Failed to communication with the broker.
- **NXD_MQTT_QOS2_NOT_SUPPORTED** (0x1000C) QoS level 2 messages are not supported.
- NX_PTR_ERROR (0x07) Invalid MQTT control block, ip_ptr, or packet pool pointer
- NXD_MQTT_INVALID_PARAMETER (0x10009) Input parameter error

### Allowed From

Threads

### Example

```c
CHAR *topic = "temperature";
CHAR *message = "100";

/* Publish the temperature value. */
status = nxd_mqtt_client_publish(&my_client,
    topic, STRLEN(topic),
    message, STRLEN(message),
    NX_TRUE, /* Server retains message. */);
    0, /* QOS */
    NX_WAIT_FOREVER);

/* If status is NXD_MQTT_SUCCESS the message has been 
successfully sent to the broker. */
```

## nxd_mqtt_client_subscribe

Subscribe to a topic

### Prototype

```c
UINT nxd_mqtt_client_subscribe(
    NXD_MQTT_CLIENT *mqtt_client_ptr,
    CHAR *topic_name,
    UINT topic_name_length,
    UINT QoS);
```

### Description

This service subscribes to a specific topic. Subscribing to QoS level 2 messages is not supported yet.

### Input Parameters

- *client_ptr*: Pointer to MQTT Client control block.
- *topic_name*: Topic to publish to.
- *topic_name_length*: Length of the topic, in bytes.
- *QoS The desired QoS level:*: 0 or 1.

### Return Values

- **NXD_MQTT_SUCCESS** (0x00) Successfully subscribed to the topic.
- **NXD_MQTT_NOT_CONNECTED** (0x10002) The client is not connected to the broker.
- **NXD_MQTT_MUTEX_FAILURE** (0x10003) Failed to obtain MQTT mutex
- **NXD_MQTT_INTERNAL_ERROR** (0x10004) Internal logic error
- **NXD_MQTT_COMMUNICATION_FAILURE** (0x10007) Unable to send messages to the broker.
- **NXD_MQTT_QOS2_NOT_SUPPORTED** (0x1000C) QoS level 2messages are not supported.
- NX_PTR_ERROR (0x07) Invalid MQTT control block, ip_ptr, or packet pool pointer
- NXD_MQTT_INVALID_PARAMETER (0x10009) topic_name is not set, or topic_name_length is zero, or QoS is value is invalid.

### Allowed From

Threads

### Example

```c
/* Subscribe to the topic "temperature" with QoS level 0 */
CHAR *topic = "temperature";

status = nxd_mqtt_client_subscribe(&my_client, topic,
    STRLEN(topic), 0);

/* If status is NXD_MQTT_SUCCESS, the client successfully
subscribes to the topic "temperate". At this point the client
is ready for receiving messages from the broker. */
```

## nxd_mqtt_client_unsubscribe

Unsubscribe from a topic

### Prototype

```c
UINT nxd_mqtt_client_unsubscribe(
    NXD_MQTT_CLIENT *mqtt_client_pr,
    CHAR *topic_name,
    UINT topic_name_length);
```

### Description

This service unsubscribes from a topic.

### Input Parameters

- *client_ptr*: Pointer to MQTT Client control block.
- *topic_name*: Topic to unsubscribe from.
- *topic_name_length*: Length of the topic, in bytes.

### Return Values

- **NXD_MQTT_SUCCESS** (0x00) Successfully unsubscribed from the topic.
- **NXD_MQTT_NOT_CONNECTED** (0x10002) The client is not connected to the broker.
- **NXD_MQTT_MUTEX_FAILURE** (0x10003) Failed to obtain MQTT mutex.
- **NXD_MQTT_INTERNAL_ERROR** (0x10004) Internal logic error
- **NXD_MQTT_COMMUNICATION_FAILURE** (0x10007) Unable to send messages to the broker.
- NX_PTR_ERROR (0x07) Invalid MQTT control block pointer
- NXD_MQTT_INVALID_PARAMETER (0x10009) topic_name is not set, or topic_name_length is zero.

### Allowed From

Threads

### Example

```c
/* Subscribe to the topic "temperature" with QoS level 0 */
CHAR *topic = "temperature";

status = nxd_mqtt_client_unsubscribe(&my_client, topic,
    STRLEN(topic));

/* If status is NXD_MQTT_SUCCESS, the client successfully
unsubscribes the topic "temperate". */
```

## nxd_mqtt_client_receive_notify_set

Set MQTT message receive notify callback function

### Prototype

```c
UINT nxd_mqtt_client_receive_notify_set(
    NXD_MQTT_CLIENT *client_ptr,
    VOID(*receive_notify)(
        NXD_MQTT_CLIENT* client_ptr,
        UINT message_count));
```

### Description

This service registers a callback function with the MQTT client. Upon receiving a message published by the broker, MQTT client stores the message in the receive queue. If the callback function is set, the callback function is invoked to notify the application that a message is ready to be retrieved. The receive notify function takes a pointer to the MQTT client control block, and a *message_count* indicating the number of messages available in the receive queue. Note that the number may change between the receive notification and when the application retrieves these messages, as new messages may have arrived in the interval.

### Input Parameters

- *client_ptr*: Pointer to MQTT Client control block.
- *receive_notify*: User supplied callback function to be invoked on receiving a message.

### Return Values

- **NXD_MQTT_SUCCESS** (0x00) Successfully set the receive notify function.
- NX_PTR_ERROR (0x07) Invalid MQTT control block.

### Allowed From

Threads

### Example

```c
/* Sample MQTT receive notify function. */

VOID my_notify_func(NXD_MQTT_CLIENT* client_ptr,
    UINT message_count)
{

/* On receiving a message, set an event flag to wake up the
application thread. The message will be received and
processed in the application thread. */
tx_event_flags_set(&mqtt_app_flag,
    MESSAGE_RECEIVED_EVENT, TX_OR);

/* All done. Return to the caller. */
    return;
}

/* Set the receive callback function. */
status = **nxd_mqtt_client_receive_notify_set**(&my_client,
    my_notify_func);

/* If status is NXD_MQTT_SUCCESS the notify function is properly set. */
```

## nxd_mqtt_client_message_get

Retrieve a message from the broker

### Prototype

```c
UINT nxd_mqtt_client_message_get(
    NXD_MQTT_CLIENT *client_ptr,
    UCHAR *topic_buffer,
    UINT topic_buffer_size,
    UINT *actual_topic_length,
    UCHAR *message_buffer,
    UINT message_buffer_size,
    UINT *actual_message_length);
```

### Description

This service retrieves a message published by the broker. All incoming messages are stored in the receive queue. The application uses this service to retrieve these messages. This call is non-blocking. If the receive queue is empty, this service returns **NXD_MQTT_NO_MESSAGE**. An application wishing to be notified of incoming message can call the service ***nxd_mqtt_client_receive_notify_set*** to register a receive callback function.

The caller needs to provide memory space for the topic string and the message body. The sizes of these two buffers are passed in using *topic_buffer_size* and *message_buffer_size*. The actual number of bytes in the topic string and the message body are returned in *actual_topic_length* and *actual_message_length*. If topic length or massage length is greater than the buffer space provided, this service returns error code *NXD_MQTT_INSUFFICIENT_BUFFER_SIZE*.

The application shall allocate a bigger buffer and try again.

### Input Parameters

- *client_ptr*: Pointer to MQTT Client control block.
- *topic_buffer*: Pointer to the memory location where the topic string
is copied into.
- *topic_buffer_size*: Size of the topic buffer.
- *actual_topic_length*: Pointer to the memory location where the actual topic length is returned.
- *message_buffer*: Pointer to the memory location where the message string is copied into.
- *message_buffer_size*: Size of the message buffer.
- *actual_message_length*: Pointer to the memory location where the message length is returned.

### Return Values

- **NXD_MQTT_SUCCESS** (0x00) Successfully retrieved message.
- **NXD_MQTT_INTERNAL_ERROR** (0x10004) Internal logic error
- **NXD_MQTT_NO_MESSAGE** (0x1000A) The receive queue is empty.
- **NXD_MQTT_INSUFFICIENT_BUFFER_SIZE** (0x1000D) Topic buffer or message buffer is too small for the topic or the message.
- NX_PTR_ERROR (0x07) Invalid MQTT control block, ip_ptr, or packet pool pointer
- NXD_MQTT_INVALID_PARAMETER (0x10009) message_buffer or topic_buffer pointer is NULL

### Allowed From

Threads

### Example

```c
UCHAR topic[MAX_TOPIC_SIZE];
UCHAR message[MAX_TOPIC_SIZE];
UINT topic_length;
UINT message_length;

/* Retrieve a message from MQTT client receive queue. */
status = nxd_mqtt_client_message_get(&my_client, topic,
    sizeof(topic), &topic_length, message, sizeof(message),
    &message_length);

/* Check the return value. */
if(status == NXD_MQTT_SUCCESS)
{
/* A message is received. All done. */
}
else if (status == NXD_MQTT_NO_MESSAGE)
{
/* No more messages in the receive queue. All done. */
}
else
{
/* Receive error. */
}
```

## nxd_mqtt_client_disconnect_notify_set

Set MQTT message disconnect notify callback function

### Prototype

```c
UINT nxd_mqtt_client_disconnect_notify_set(
    NXD_MQTT_CLIENT *client_ptr,
    VOID(*disconnect_notify)(NXD_MQTT_CLIENT* client_ptr));
```

### Description

This service registers a callback function with the MQTT client. When MQTT detects the connection to the broker is lost, it calls this notify function to alert the application. Therefore, the application can use this callback function to detect a lost connection, and to be able to re-establish connection to the broker.

```c
VOID callback_func(NXD_MQTT_CLIENT *client_ptr);
```

### Input Parameters

- *client_ptr*: Pointer to MQTT Client control block.
- *disconnect_notify*: User supplied callback function to be invoked when MQTT detects the connection to the broker is lost.

### Return Values

- **NXD_MQTT_SUCCESS** (0x00) Successfully set the disconnect notify function.
- NX_PTR_ERROR (0x07) Invalid MQTT control block.

### Allowed From

Threads

### Example

```c
VOID disconnect_notify(NXD_MQTT_CLIENT *client_ptr)
{
/* MQTT client is disconnected from the broker. Notify the application so it can re-connect to the broker. */
}

status = nxd_mqtt_client_disconnect_notify_set(client_ptr,
    disconnect_notify);
```

## nxd_mqtt_client_disconnect

Disconnect MQTT client from the broker

### Prototype

```c
UINT nxd_mqtt_client_disconnect(NXD_MQTT_CLIENT *client_ptr);
```

### Description

This service disconnects the client from the broker. Note that messages on the receive queue are released. Messages with QoS 1 in the transmit queue are not released. After the client reconnects to the server, QoS 1 messages can be processed, unless the client reconnects to the server with *clean_session* flag set to **NX_TRUE**.

If the connection was made with TLS security protection, this service will close the TLS session before disconnecting the TCP connection.

The actual TCP socket disconnect call has a wait option defined by NXD_MQTT_SOCKET_TIMEOUT (timer ticks). The default value is NX_WAIT_FOREVER. To avoid indefinite suspension in the event that the network connection is lost or the server is not responding, set this option to a finite value.

### Input Parameters

- *client_ptr*: Pointer to MQTT Client control block.

### Return Values

- **NXD_MQTT_SUCCESS** (0x00) Successfully disconnected from broker
- **NXD_MQTT_MUTEX_FAILURE** (0x10003) Failed to obtain MQTT mutex.
- NX_PTR_ERROR (0x07) Invalid MQTT control block

### Allowed From

Threads

### Example

```c
/* Disconnect from the broker. */
status = nxd_mqtt_client_disconnect(&my_client);

/* If status is NXD_MQTT_SUCCESS the client is successfully
disconnected from the broker. */
```

## nxd_mqtt_client_delete

Delete the MQTT client instance

### Prototype

```c
UINT nxd_mqtt_client_delete(NXD_MQTT_CLIENT *client_ptr);
```

### Description

This service deletes the MQTT client instance and releases internal resources. This service automatically disconnects the client from the broker if it is still connected. Messages not yet transmitted or not been acknowledged are released. Messages received but not retrieved by the application are also released.

If the connection was made with TLS security protection, this service closes the TLS session before disconnecting the TCP connection.

After the client is deleted, an application wishing to use MQTT service needs to create a new instance.

### Input Parameters

- *client_ptr*: Pointer to MQTT Client control block.

### Return Values

- **NXD_MQTT_SUCCESS** (0x00) Successfully deleted MQTT client.
- NX_PTR_ERROR (0x07) Invalid MQTT control block

### Allowed From

Threads

### Example

```c
/* Delete the MQTT client instance. */
status = nxd_mqtt_client_delete(&my_client);

/* If status is NXD_MQTT_SUCCESS the client is successfully
deleted from the system. */
```

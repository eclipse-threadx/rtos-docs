---
title: Chapter 1 - Introduction to NetX Duo MQTT
description: The NetX Duo MQTT client package requires that NetX Duo (version 5.10 or later) be installed, properly configured, and the IP instance has been created.
---


## NetX Duo MQTT Requirements

The NetX Duo MQTT client package requires that NetX Duo (version 5.10 or later) be installed, properly configured, and the IP instance has been created. The TCP module must be enabled in the system. In addition, if TLS security is required, the NetX Duo Secure TLS module needs to be configured according to the security parameter required by the broker.

## NetX Duo MQTT Specification

NetX Duo MQTT client implement is compliant with OASIS MQTT Version 3.1.1 Oct 29{{< sup >}}th{{</ sup >}} 2014. The specification can be found at:

- [http://mqtt.org/](http://mqtt.org/)

## NetX Duo MQTT Basic Operation

MQTT (Message Queue Telemetry Transport) is based on publisher-subscriber model. A client can publish information to other clients through a broker. A client, if interested in a topic, can subscribe to the topic through the broker. A broker is responsible for delivering published messages to its clients who subscribe to the topic. In this publisher-subscriber model, multiple clients may publish data with the same topic. A client will receive a message it publishes if the client subscribes to the same topic.

Depending on the use case, a client may choose one of the 3 QoS levels when publishing a message:

- **QoS 0**: The message is delivered at most once. Messages sent with QoS 0 may be lost.
- **QoS 1**: The message is delivered at least once. Messages sent with QoS 1 may be delivered more than once.
- **QoS 2**: The message is delivered exactly once. Messages sent with QoS 2 is guaranteed to be delivered, with no duplication.

> **Note:** This implementation of MQTT client does not support QoS level 2 messages.

Since QoS 1 and QoS 2 are guaranteed to be delivered, the broker keeps track the state of QoS 1 and QoS 2 messages sent to each client. This is particularly important for clients that expect QoS1 or QoS 2 messages. The client may be disconnected from the broker (for example when the client reboots, or the communication link is temporarily lost). The broker must store QoS 1 and QoS 2 messages so the messages can be delivered later once the client is reconnected to the broker. However, the client may choose not to receive any stale messages from the broker after reconnection. The client can do so by initiating the connection with the *clean_session* flag set to ***NX_TRUE***. In this case, upon receiving the MQTT CONNECT message, the broker shall discard any session information associated with this client, including undelivered or unconfirmed QoS 1 or QoS 2 messages. If the *clean_session* flag is to ***NX_FALSE***, the server shall resend the QoS 1 and QoS 2 messages. The MQTT Client also resends any un-acknowledged messages if *clean_session* is set to ***NX_TRUE*.** This acknowledgment is different from the TCP layer ACK, although that happens as well. The MQTT client sends an acknowledgment to the broker.

An application creates an MQTT client instance by calling ***nxd_mqtt_client_create()***. Once the client is created, the application can connect to the broker by calling ***nxd_mqtt_client_connect()***. After connecting to the broker, the client can subscribe to a topic by calling ***nxd_mqtt_client_subscribe()***, or publish a topic by calling ***nxd_mqtt_client_publish()***.

Incoming MQTT messages are stored in the receive queue in the MQTT client instance. Application retrieves these message by calling ***nxd_mqtt_client_message_get()***. If there are messages in the receive queue, the first message (e.g. the oldest) from the queue is returned to the caller. The topic string from the message is also returned.

> **Note:** The function ***nxd_mqtt_client_message_get()*** does not block if the MQTT client receive queue is empty. The function returns immediately with the return code ***NXD_MQTT_NO_MESSAGE***. The application shall treat this return value as an indication that the receive queue is empty, not an error.

To avoid polling the receive queue for incoming messages, the application can register a callback function with the MQTT client by calling ***nxd_mqtt_client_receive_notify_set()***. The callback function is declared as:

```c
VOID (*receive_notify_callback)(NXD_MQTT_CLIENT *client_ptr, 
    UINT message_count);
```

As the MQTT client receives messages from the broker, it invokes the callback function if the function is set. The callback function passes the pointer to the client control block and a message count value. The message count value indicates the number of MQTT messages in the receive queue. Note that this callback function executes in the MQTT client thread context. Therefore, the callback function should not execute any procedures that may block the MQTT client thread. The callback function should trigger the application thread to call ***nxd_mqtt_client_message_get()*** to retrieve the messages.

To disconnect and terminate the MQTT client service, the application shall use the service ***nxd_mqtt_client_disconnect()*** and ***nxd_mqtt_client_delete().*** Calling ***nxd_mqtt_client_disconnect()*** simply disconnects the TCP connection to the broker. It releases messages already received and stored in the receive queue. However, it does not release QoS level 1 messages in the transmit queue. QoS level 1 messages are retransmitted upon connection, assuming the ***clean_session*** flag is set to ***NX_FALSE.***

The broker may also disconnect from the client. When the TCP connection between the client and the broker is terminated, the application can be notified by the disconnect notify function. To use the notification mechanism, application installs the disconnect notify function by calling ***nxd_mqtt_client_disconnect_notify_set*.** Once a TCP disconnect is observed and the MQTT session has been created, the notification function is invoked.

Calling ***nxd_mqtt_client_delete()*** releases all message blocks in the transmit queue and the receive queue. Unacknowledged QoS level 1 messages are also deleted.

## Secure MQTT Connection

The MQTT client makes a secure connection to the broker using the NetX Duo Secure TLS module. The default port number for MQTT with TLS security is 8883, defined in ***NXD_MQTT_TLS_PORT***.

To create a secure MQTT connection to the broker, a TLS session needs to be negotiated after a TCP connection is established, before MQTT CONNECT messages can be sent to the broker. The TLS session set up is accomplished by calling ***nxd_mqtt_client_secure_connect()*** and passing in a user-defined TLS setup callback function. During the MQTT connection phase, once the TCP connection is established, the client invokes the TLS setup callback function to start a proper TLS handshake process. After the TLS session is established, the client continues the MQTT CONNECT message over the secure channel.

The user defined callback function takes five input values and is declared as:

```c
UINT tls_Setup_callback(NXD_MQTT_CLIENT *client_ptr,
    NX_SECURE_TLS_SESSION *session_ptr,
    NX_SECURE_TLS_CERTIFICATE *certificate_ptr,
    NX_SECURE_TLS_CERTIFICATE *trusted_certificate);
```

Below is a description of the input parameters:

- **client_ptr**: Pointer to the MQTT client control block.
- **session_ptr**: Pointer to the TLS session control block.
- **certificate_ptr**: Pointer to the certificate control block. The setup function configures this certificate before sending it to the broker.
- **trusted_certificate_ptr**: Pointer to the trusted certificate. TLS setup function configures the trusted certificate to authenticate the server.

In the TLS setup function, the application is responsible for creating a TLS session, and configuring the session with a proper certificate. The following pseudo code outlines a typical TLS session start up procedure. The reader is referred to the NetX Duo Secure TLS User Guide for details on using TLS APIs.

Below is an example TLS setup callback:

```c
UINT tls_setup_callback(NXD_MQTT_CLIENT *client_pt
    NX_SECURE_TLS_SESSION *session_ptr,
    NX_SECURE_TLS_CERTIFICATE *certificate_ptr,
    NX_SECURE_TLS_CERTIFICATE *trusted_certificate_ptr)
{
    /* Initialize TLS module */
    nx_secure_tls_initialize();

    /* Create a TLS session */
    nx_secure_tls_session_create(session_ptr, …);

    /* Need to allocate space for the certificate coming in from the broker. */
    memset(certificate_ptr), 0, sizeof(NX_SECURE_TLS_CERTIFICATE));

    nx_secure_tls_remote_certificate_allocate(session_ptr, certificate_ptr);

    /* Add a CA Certificate to our trusted store for verifying incoming server certificates. */
    nx_secure_tls_certificate_initialize(
        trusted_certificate_ptr,
        ca_cert_der,
        ca_cert_der_len, NULL, 0);
    nx_secure_tls_trusted_certificate_add(session_ptr,
        trusted_certificate));
}
```

## Known Limitations of the NetX Duo MQTT Client

- NetX Duo MQTT does not support sending or receiving QoS level 2 messages.
- NetX Duo MQTT does not support chained-packets.

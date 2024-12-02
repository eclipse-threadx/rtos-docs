---
title: Chapter 2 - Installation and use of NetX Duo MQTT client
description: This chapter contains a description of various issues related to installation, setup, and usage of the NetX Duo MQTT Client component.
---


This chapter contains a description of various issues related to installation, setup, and usage of the NetX Duo MQTT client component.

## Product Distribution

MQTT Client for NetX Duo is available at [https://github.com/eclipse-threadx/netxduo](https://github.com/eclipse-threadx/netxduo). The package includes two source files, one include file, and a file that contains this document, as follows:

- **nxd_mqtt_client.h** Header file for MQTT Client for NetX Duo
- **nxd_mqtt_client.c** C Source file for MQTT Client for NetX Duo
- **nxd_mqtt_client.pdf** Description of MQTT Client for NetX Duo
- **demo_mqtt_client.c** NetX Duo MQTT demonstration

## MQTT Client Installation

In order to use MQTT Client for NetX Duo, the entire distribution mentioned previously should be copied to the same directory where NetX Duo is installed. For example, if NetX Duo is installed in the directory "*\threadx\arm7\green*" then the *nxd_mqtt_client.h* and *nxd_mqtt_client.c* for NetX Duo MQTT Client need to be copied into this directory.

## Using MQTT Client

Using MQTT Client for NetX Duo is easy. Basically, the application code must include *nxd_mqtt_client.h* after it includes *tx_api.h* and *nx_api.h*, in order to use ThreadX, and NetX Duo, respectively. Once the MQTT Client header files are included, the application code is then able to use the MQTT services described later in this guide. The application must also include *nxd_mqtt_client.c* in the build process. These files must be compiled in the same manner as other application files and its object form must be linked along with the files of the application. This is all that is required to use NetX Duo MQTT Client.

## Using MQTT Client with NetX Duo Secure TLS

To use MQTT client with NetX Duo Secure TLS module, application must have NetX Duo Secure TLS module installed, and include *nx_secure_tls_api.h* and *nx_crypto.h*. The MQTT library must be built with the symbol ***NX_SECURE_ENABLE*** defined.

## Configuration Options

There are several configuration options for building MQTT client for NetX Duo. Following is a list of all options, where each is described in detail. The default values are listed, but can be redefined prior to inclusion of *nxd_mqtt_client.h.*

- **NX_DISABLE_ERROR_CHECKING**: Defined, this option removes the
basic MQTT client error checking. It is typically used after the
application has been debugged.
- **NX_SECURE_ENABLE**: Defined, MQTT Client is built with TLS support.
Defining this symbol requires NetX Duo Secure TLS module to be installed.
*NX_SECURE_ENABLE* is not enabled by default.**
- **NXD_MQTT_REQUIRE_TLS**: Defined, application must use TLS to
connect to MQTT broker. This feature requires *NX_SECURE_ENABLE*
defined. By default, this symbol is not defined.
- **NXD_MQTT_MAXIMUM_TRANSMIT_QUEUE_DEPTH**: Defined, MQTT transmit queue depth is enabled. It must be positive integer.
- **NXD_MQTT_MAX_TOPIC_NAME_LENGTH**: Deprecated.
- **NXD_MQTT_MAX_MESSAGE_LENGTH**: Deprecated.
- **NXD_MQTT_KEEPALIVE_TIMER_RATE**: Defines the MQTT timer rate, in ThreadX timer ticks. This timer is used to keep track of the time since last MQTT control message was sent, and sends out an MQTT PINGREQ message before the keep-alive time expires. This timer is activated if the client connects to the broker with a keep-alive timer value set. The default value is TX_TIMER_TICKS_PER_SECOND, which is a one-second timer.
- **NXD_MQTT_PING_TIMEOUT_DELAY**: Defines the time the MQTT client waits for PINGRESP from the broker after it sends out MQTT PINGREQ. If no PINGRESP is received after this timeout delay, the client treats the broker as non-responsive and disconnects itself from the broker. The default PING timeout delay is TX_TIMER_TICKS_PER_SECOND, which is one second.
- **NXD_MQTT_SOCKET_TIMEOUT**: Defines the time out in the TCP socket disconnect call when disconnecting from the MQTT server in timer ticks. The default value is NX_WAIT_FOREVER.

## Sample MQTT program

The following program illustrates a simple MQTT application. For simplicity, the return codes are assumed to be successful, therefore no further error checking is done.

```c
#define LOCAL_SERVER_ADDRESS (IP_ADDRESS(192, 168, 1, 81))

/*******************************************************/
/* IOT MQTT Client Example                             */
/*******************************************************/
#define DEMO_STACK_SIZE 2048
#define CLIENT_ID_STRING "mytestclient"
#define MQTT_CLIENT_STACK_SIZE 4096
#define STRLEN(p) (sizeof(p) - 1)

/* Declare the MQTT thread stack space. */
static ULONG mqtt_client_stack[MQTT_CLIENT_STACK_SIZE / sizeof(ULONG)];

/* Declare the MQTT client control block. */
static NXD_MQTT_CLIENT mqtt_client;

/* Define the symbol for signaling a received message. */

/* Define the test threads. */

#define TOPIC_NAME "topic"

#define MESSAGE_STRING "This is a message. "

/* Define the priority of the MQTT internal thread. */
#define MQTT_THREAD_PRIORITY 2

/* Define the MQTT keep alive timer for 5 minutes */
#define MQTT_KEEP_ALIVE_TIMER 300
#define QOS0 0
#define QOS1 1

/* Declare event flag, which is used in this demo. */
TX_EVENT_FLAGS_GROUP mqtt_app_flag;
#define DEMO_MESSAGE_EVENT 1
#define DEMO_ALL_EVENTS 3

/* Declare buffers to hold message and topic. */
static UCHAR message_buffer[NXD_MQTT_MAX_MESSAGE_LENGTH];
static UCHAR topic_buffer[NXD_MQTT_MAX_TOPIC_NAME_LENGTH];

/* Declare the disconnect notify function. */
static VOID my_disconnect_func(NXD_MQTT_CLIENT *client_ptr)
{
    printf("client disconnected from server\n");
}

static VOID my_notify_func(NXD_MQTT_CLIENT* client_ptr, UINT number_of_messages)
{
    tx_event_flags_set(&mqtt_app_flag, DEMO_MESSAGE_EVENT, TX_OR);
    return;
}

static ULONG error_counter;
void demo_mqtt_client_local(NX_IP *ip_ptr, NX_PACKET_POOL *pool_ptr)
{
    UINT status;
    NXD_ADDRESS server_ip;
    ULONG events;
    UINT topic_length, message_length;

    /* Create MQTT client instance. */
    nxd_mqtt_client_create(&mqtt_client, "my_client",
        CLIENT_ID_STRING, STRLEN(CLIENT_ID_STRING), ip_ptr, pool_ptr,
        (VOID*)mqtt_client_stack, sizeof(mqtt_client_stack),
        MQTT_THREAD_PRIORITY, NX_NULL, 0);

    /* Register the disconnect notification function. */
    nxd_mqtt_client_disconnect_notify_set(&mqtt_client, my_disconnect_func);

    /* Create an event flag for this demo. */
    status = tx_event_flags_create(&mqtt_app_flag, "my app event");
    server_ip.nxd_ip_version = 4;
    server_ip.nxd_ip_address.v4 = LOCAL_SERVER_ADDRESS;

    /* Start the connection to the server. */
    nxd_mqtt_client_connect(&mqtt_client, &server_ip, NXD_MQTT_PORT, 
        MQTT_KEEP_ALIVE_TIMER, 0, NX_WAIT_FOREVER);

    /* Subscribe to the topic with QoS level 0. */
    nxd_mqtt_client_subscribe(&mqtt_client, TOPIC_NAME, STRLEN(TOPIC_NAME),
        QOS0);

    /* Set the receive notify function. */
    nxd_mqtt_client_receive_notify_set(&mqtt_client, my_notify_func);

    /* Publish a message with QoS Level 1. */
    nxd_mqtt_client_publish(&mqtt_client, TOPIC_NAME,
        STRLEN(TOPIC_NAME), (CHAR*)MESSAGE_STRING, 
        STRLEN(MESSAGE_STRING), 0, QOS1, NX_WAIT_FOREVER);

    /* Now wait for the broker to publish the message. */
    tx_event_flags_get(&mqtt_app_flag, DEMO_ALL_EVENTS,
        TX_OR_CLEAR, &events, TX_WAIT_FOREVER);

    if(events & DEMO_MESSAGE_EVENT)
    {
        nxd_mqtt_client_message_get(&mqtt_client, topic_buffer,
            sizeof(topic_buffer), &topic_length, message_buffer,
            sizeof(message_buffer), &message_length);

        topic_buffer[topic_length] = 0;

        message_buffer[message_length] = 0;

        printf("topic = %s, message = %s\n", topic_buffer, message_buffer);
    }

    /* Now unsubscribe the topic. */
    nxd_mqtt_client_unsubscribe(&mqtt_client, TOPIC_NAME,
        STRLEN(TOPIC_NAME));

    /* Disconnect from the broker. */
    nxd_mqtt_client_disconnect(&mqtt_client);

    /* Delete the client instance, release all the resources. */
    nxd_mqtt_client_delete(&mqtt_client);
    return;
}
```

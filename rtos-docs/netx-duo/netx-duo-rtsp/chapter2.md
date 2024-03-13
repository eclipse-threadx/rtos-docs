---
title: Chapter 2 - Installation and use of NetX Duo RTSP Server
description: This chapter contains a description of various issues related to installation, set up, and usage of the NetX Duo RTSP Server services.
---

# Chapter 2 - Installation and use of NetX Duo RTSP Server

This chapter contains a description of various issues related to installation, setup, and usage of the NetX Duo RTSP Server services.

## Product Distribution

NetX Duo RTSP Server is available at [https://github.comeclipse-threadx/netxduo](https://github.comeclipse-threadx/netxduo). The package includes one source file and one header file, as follows:

- **nx_rtsp_server.h** Header file for NetX Duo RTSP Server
- **nx_rtsp_server.c** C Source file for NetX Duo RTSP Server

## NetX Duo RTSP Server Installation

In order to use the NetX Duo RTSP Server API, the entire distribution mentioned previously should be copied to the same directory where NetX Duo is installed. For example, if NetX Duo is installed in the directory "*\threadx\arm7\green*" then the *nx_rtsp_server.h* and *nx_rtsp_server.c* files should be copied into this directory for RTSP Server applications.

## Using NetX Duo RTSP Server

Using the NetX Duo RTSP Server is easy. Basically, the application code must include *nx_rtsp_server.h* for after it includes *tx_api.h*, and *nx_api.h*, in order to use ThreadX and NetX Duo respectively. The build project must include the RTSP source code *nx_rtsp_server.c* and the host application file, and of course the ThreadX and NetX library files. This is all that is required to use NetX Duo RTSP Server.

> **Note:** Since RTSP utilizes NetX Duo TCP services, TCP must be enabled with the *nx_tcp_enable* call prior to using RTSP.

## Small Example System of NetX Duo RTSP Server

An example of how to use NetX Duo RTSP Server is described below.

> **Caution:** This is provided for demonstration purposes only and is not guaranteed to compile and run as is.
>
> Please refer to the NetX Duo RTSP Server release code distribution for  demo source code file(s) that will properly build in the native Eclipse ThreadX environment.  Also be aware that these demos are intentionally kept very simple as they are intended to introduce NetX Duo RTSP Server application to new users.

In this example, the RTSP server is created and started in the *sample_entry*. The RTSP server is created with the *nx_rtsp_server_create* API, and the *rtsp_disconnect_callback* is registered in this API. The RTSP server is started with the *nx_rtsp_server_start* API. The callback functions are registered with the *nx_rtsp_server_describe_callback_set*, *nx_rtsp_server_setup_callback_set*, *nx_rtsp_server_play_callback_set*, *nx_rtsp_server_teardown_callback_set*, *nx_rtsp_server_pause_callback_set*, and *nx_rtsp_server_set_parameter_callback_set* APIs.

In the *rtsp_describe_callback*, the *nx_rtsp_server_sdp_set* is called to set SDP string to the DESCRIBE response, then the RTSP server sends the DESCRIBE response to the RTSP client.

In the *rtsp_setup_callback*, the client transport information is passed to create the corresponding media session, and the server transport information is set back to the transport_ptr parameter, then the RTSP server sends the SETUP response to the RTSP client. If using NetX Duo RTP service, there are commented reference codes to create the RTP sessions.

In the *rtsp_play_callback*, the RTP information is set to the PLAY response, and the play event is triggered to start the stream transmission, then the RTSP server sends the PALY response to the RTSP client. If using NetX Duo RTP service, there are commented reference codes to get the RTP information.

In the *rtsp_pause_callback*, the pause event is triggered to pause the stream transmission, then the RTSP server sends the PAUSE response to the RTSP client.

In the *rtsp_set_parameter_callback*, the parameters are checked and updated, then the RTSP server sends the SET_PARAMETER response to the RTSP client.

In the *rtsp_teardown_callback*, the teardown event is triggered to teardown the stream transmission and release the resources, then the RTSP server sends the TEARDOWN response to the RTSP client.

In the *rtsp_disconnect_callback*, the teardown event is triggered to teardown the stream transmission and release the resources.

```C
/* This is the test control routine the NetX RTSP module.  All tests are dispatched from this routine.  */

#include "tx_api.h"
#include "nx_api.h"
#include "nx_rtsp_server.h"


/* Define the stack size for demo tasks. */
#define DEMO_TEST_STACK_SIZE              2048
#define DEMO_RTSP_SERVER_STACK_SIZE       2048
static UCHAR test_thread_stack[DEMO_TEST_STACK_SIZE];
static UCHAR rtsp_server_stack[DEMO_RTSP_SERVER_STACK_SIZE];

/* Define events for the test task */
#define DEMO_ALL_EVENTS                   ((ULONG)0xFFFFFFFF)
#define DEMO_PLAY_EVENT                   ((ULONG)0x00000001)
#define DEMO_PAUSE_EVENT                  ((ULONG)0x00000002)
#define DEMO_TEARDOWN_EVENT               ((ULONG)0x00000003)

/* The RTSP server listening port.  */
#ifndef DEMO_RTSP_SERVER_PORT
#define DEMO_RTSP_SERVER_PORT             554
#endif

/* The RTSP server thread priority.  */
#ifndef DEMO_RTSP_SERVER_PRIORITY
#define DEMO_RTSP_SERVER_PRIORITY         3
#endif

/* File name shown in rtsp SETUP request */
#ifndef DEMO_RTSP_VIDEO_FILE_NAME
#define DEMO_RTSP_VIDEO_FILE_NAME         "trackID=0"
#endif

#ifndef DEMO_RTSP_AUDIO_FILE_NAME
#define DEMO_RTSP_AUDIO_FILE_NAME         "trackID=1"
#endif

/* Declare the prototypes for the test entry points. */
TX_THREAD            test_thread;
NX_RTSP_SERVER       rtsp_0;

/* Declare events to use in tasks. */
TX_EVENT_FLAGS_GROUP demo_test_events;

/* Define an error counter.  */
ULONG                error_counter;

static VOID test_server_entry(ULONG thread_input);
static UINT rtsp_describe_callback(NX_RTSP_CLIENT *rtsp_client_ptr, UCHAR *uri, UINT uri_length);
static UINT rtsp_setup_callback(NX_RTSP_CLIENT *rtsp_client_ptr, UCHAR *uri, UINT uri_length, NX_RTSP_TRANSPORT *transport_ptr);
static UINT rtsp_play_callback(NX_RTSP_CLIENT *rtsp_client_ptr, UCHAR *uri, UINT uri_length, UCHAR *range_ptr, UINT range_length);
static UINT rtsp_teardown_callback(NX_RTSP_CLIENT *rtsp_client_ptr, UCHAR *uri, UINT uri_length);
static UINT rtsp_pause_callback(NX_RTSP_CLIENT *rtsp_client_ptr, UCHAR *uri, UINT uri_length, UCHAR *range_ptr, UINT range_length);
static UINT rtsp_set_parameter_callback(NX_RTSP_CLIENT *rtsp_client_ptr, UCHAR *uri, UINT uri_length, UCHAR *parameter_ptr, ULONG parameter_length);
static UINT rtsp_disconnect_callback(NX_RTSP_CLIENT *rtsp_client_ptr);


/* Define what the initial system looks like.  */
void sample_entry(NX_IP *ip_ptr, NX_PACKET_POOL *pool_ptr, VOID *dns_ptr, UINT (*unix_time_callback)(ULONG *unix_time))
{

UINT   status;

    /* Create RTSP server. */
    status = nx_rtsp_server_create(&rtsp_0, "RTSP Server", sizeof("RTSP Server") - 1, ip_ptr, pool_ptr,
                                   rtsp_server_stack, DEMO_RTSP_SERVER_STACK_SIZE, DEMO_RTSP_SERVER_PRIORITY, DEMO_RTSP_SERVER_PORT, rtsp_disconnect_callback);
    if (status)
        error_counter++;

    /* Set callback functions. */
    nx_rtsp_server_describe_callback_set(&rtsp_0, rtsp_describe_callback);
    nx_rtsp_server_setup_callback_set(&rtsp_0, rtsp_setup_callback);
    nx_rtsp_server_play_callback_set(&rtsp_0, rtsp_play_callback);
    nx_rtsp_server_teardown_callback_set(&rtsp_0, rtsp_teardown_callback);
    nx_rtsp_server_pause_callback_set(&rtsp_0, rtsp_pause_callback);
    nx_rtsp_server_set_parameter_callback_set(&rtsp_0, rtsp_set_parameter_callback);

    /* Start RTSP server. */
    status = nx_rtsp_server_start(&rtsp_0);
    if (status)
        error_counter++;

    printf("RTSP server started!\r\n");

    /* Create a helper thread for the server. */
    status = tx_thread_create(&test_thread, "Test thread", test_server_entry, 0,
                              test_thread_stack, DEMO_TEST_STACK_SIZE,
                              4, 4, TX_NO_TIME_SLICE, TX_AUTO_START);
    if (status)
        error_counter++;

    /* Create event for the play thread */
    status = tx_event_flags_create(&demo_test_events, "Demo events");
    if (status)
        error_counter++;
}

static VOID test_server_entry(ULONG thread_input)
{
ULONG events = 0;

    while (1)
    {

        tx_event_flags_get(&demo_test_events, DEMO_ALL_EVENTS, TX_OR_CLEAR, &events, TX_WAIT_FOREVER);

        if (events & DEMO_PLAY_EVENT)
        {
            /* Start the stream transmission here.  */
        }

        if (events & DEMO_PAUSE_EVENT)
        {
            /* Pause the stream transmission here.  */
        }

        if (events & DEMO_TEARDOWN_EVENT)
        {
            /* Stop stream transmission and release the resource here.  */
        }
    }
}

/* SDP string options */
static CHAR *sdp =
"v=0\r\ns=H264 video with AAC audio, streamed by the NetX RTSP Server\r\n\
m=video 0 RTP/AVP 96\r\n\
a=rtpmap:96 H264/90000\r\n\
a=fmtp:96 profile-level-id=42A01E; packetization-mode=1\r\n\
a=control:trackID=0\r\n\
m=audio 0 RTP/AVP 97\r\n\
a=rtpmap:97 mpeg4-generic/44100/1\r\n\
a=fmtp:97 mode=AAC-hbr; SizeLength=13\r\n\
a=control:trackID=1\r\n";


static UINT rtsp_describe_callback(NX_RTSP_CLIENT *rtsp_client_ptr, UCHAR *uri, UINT uri_length)
{
UINT status;

    /* Set the SDP string.  */
    status = nx_rtsp_server_sdp_set(rtsp_client_ptr, sdp, strlen(sdp));
    return(status);
}

static UINT rtsp_setup_callback(NX_RTSP_CLIENT *rtsp_client_ptr, UCHAR *uri, UINT uri_length, NX_RTSP_TRANSPORT *transport_ptr)
{
UINT status;
UINT rtp_port, rtcp_port, rtp_ssrc;

    /* Get the server RTP and RTCP ports.  */
    /* User implementation here to set value to rtp_port and rtcp_port.  */
    /* If using NetX Duo RTP service, RTP API (see details from the RTP user guide) is called as follows.  */
    /* status = nx_rtp_sender_port_get(&rtp_0, &rtp_port, &rtcp_port);
      if (status)
      {
        return(status);
      }  */

    if (strstr(uri, DEMO_RTSP_VIDEO_FILE_NAME))
    {

        /* Setup the video session.  */
        /* User implementation here to use the client transport info to create the video session.  */
        /* If using NetX Duo RTP service, RTP API (see details from the RTP user guide) is called as follows.  */
        /* status = nx_rtp_sender_session_create(&rtp_0, &rtp_session_video, DEMO_RTP_PAYLOAD_TYPE_VIDEO,
                                                 &(transport_ptr -> client_ip_address), transport_ptr -> client_rtp_port, transport_ptr -> client_rtcp_port);
           if (status)
           {
               return(status);
           }  */

        /* Obtain generated SSRC.  */
        /* User implementation here to set value to rtp_ssrc.  */
        /* If using NetX Duo RTP service, RTP API (see details from the RTP user guide) is called as follows.  */
        /* status = nx_rtp_sender_session_ssrc_get(&rtp_session_video, &rtp_ssrc);
           if (status)
           {
               return(status);
           }  */
    }
    else if (strstr(uri, DEMO_RTSP_AUDIO_FILE_NAME))
    {

        /* Setup the audio session.  */
        /* User implementation here to use the client transport info to create the audio session.  */
        /* If using NetX Duo RTP service, RTP API (see details from the RTP user guide) is called as follows.  */
        /* status = nx_rtp_sender_session_create(&rtp_0, &rtp_session_audio, DEMO_RTP_PAYLOAD_TYPE_AUDIO,
                                                 &(transport_ptr -> client_ip_address), transport_ptr -> client_rtp_port, transport_ptr -> client_rtcp_port);
           if (status)
           {
               return(status);
           }  */

        /* Obtain generated SSRC.  */
        /* User implementation here to set value to rtp_ssrc.  */
        /* If using NetX Duo RTP service, RTP API (see details from the RTP user guide) is called as follows.  */
        /* status = nx_rtp_sender_session_ssrc_get(&rtp_session_audio, &rtp_ssrc);
           if (status)
           {
               return(status);
           }  */
    }
    else
    {

        return(NX_NOT_SUCCESSFUL);
    }

    /* Set the server RTP and RTCP ports, and RTP SSRC back.  */
    transport_ptr -> server_rtp_port = rtp_port;
    transport_ptr -> server_rtcp_port = rtcp_port;
    transport_ptr -> rtp_ssrc = rtp_ssrc;

    return(NX_SUCCESS);
}

static UINT rtsp_play_callback(NX_RTSP_CLIENT *rtsp_client_ptr, UCHAR *uri, UINT uri_length, UCHAR *range_ptr, UINT range_length)
{
UINT status;
UINT video_seq, audio_seq, video_rtptime, audio_rtptime;

    /* Retrieve the video sequence number.  */
    /* User implementation here to set value to video_seq.  */
    /* If using NetX Duo RTP service, RTP API (see details from the RTP user guide) is called as follows.  */
    /* nx_rtp_sender_session_sequence_number_get(&rtp_session_video, &video_seq);  */

    /* Assign recorded timestamps.  */
    /* User implementation here to set value to video_rtptime.  */

    /* Set RTP information into RTSP client */
    status = nx_rtsp_server_rtp_info_set(rtsp_client_ptr, DEMO_RTSP_VIDEO_FILE_NAME, sizeof(DEMO_RTSP_VIDEO_FILE_NAME) - 1, video_seq, video_rtptime);
    if (status)
    {
        return(status);
    }

    /* Retrieve the sequence number through rtp sender functions */
    /* User implementation here to set value to audio_seq.  */
    /* If using NetX Duo RTP service, RTP API (see details from the RTP user guide) is called as follows.  */
    /* nx_rtp_sender_session_sequence_number_get(&rtp_session_audio, &audio_seq);  */

    /* Assign recorded timestamps */
    /* User implementation here to set value to audio_rtptime.  */

    /* Set RTP information into RTSP client */
    status = nx_rtsp_server_rtp_info_set(rtsp_client_ptr, DEMO_RTSP_AUDIO_FILE_NAME, sizeof(DEMO_RTSP_VIDEO_FILE_NAME) - 1, audio_seq, audio_rtptime);
    if (status)
    {
        return(status);
    }

    /* Trigger the play event */
    tx_event_flags_set(&demo_test_events, DEMO_PLAY_EVENT, TX_OR);

    return(NX_SUCCESS);
}

static UINT rtsp_teardown_callback(NX_RTSP_CLIENT *rtsp_client_ptr, UCHAR *uri, UINT uri_length)
{

    /* Trigger the tear down event */
    tx_event_flags_set(&demo_test_events, DEMO_TEARDOWN_EVENT, TX_OR);
    return(NX_SUCCESS);
}

static UINT rtsp_pause_callback(NX_RTSP_CLIENT *rtsp_client_ptr, UCHAR *uri, UINT uri_length, UCHAR *range_ptr, UINT range_length)
{

    /* Trigger the pause event */
    tx_event_flags_set(&demo_test_events, DEMO_PAUSE_EVENT, TX_OR);
    return(NX_SUCCESS);
}

static UINT rtsp_set_parameter_callback(NX_RTSP_CLIENT *rtsp_client_ptr, UCHAR *uri, UINT uri_length, UCHAR *parameter_ptr, ULONG parameter_length)
{

    /* User implementation here to check and update parameters.  */
    return(NX_SUCCESS);
}

static UINT rtsp_disconnect_callback(NX_RTSP_CLIENT *rtsp_client_ptr)
{

    /* Trigger the tear down event */
    tx_event_flags_set(&demo_test_events, DEMO_TEARDOWN_EVENT, TX_OR);
    return(NX_SUCCESS);
}

```

## Configuration Options

There are several configuration options for building NetX Duo RTSP server. The default values are listed but can be redefined prior to inclusion of *nx_rtsp_server.h*. The following list describes each in detail:

- **NX_RTSP_SERVER_MAX_CLIENTS** The max number of concurrent clients the server supports. By default, this value is defined as 2.
- **NX_RTSP_SERVER_TIME_SLICE** RTSP Server time slice. By default, this value is defined as *TX_NO_TIME_SLICE*.
- **NX_RTSP_SERVER_PACKET_TIMEOUT** The timeout for the packet allocation and data appending. By default, this value is defined as 1 second ( 1 \* *NX_IP_PERIODIC_RATE*).
- **NX_RTSP_SERVER_ACCEPT_TIMEOUT** The timeout for the RTSP Server socket accepting. By default, this value is defined as 10 seconds ( 10 \* *NX_IP_PERIODIC_RATE*).
- **NX_RTSP_SERVER_SEND_TIMEOUT** The timeout for the packet sending. By default, this value is defined as 1 second ( 1 \* *NX_IP_PERIODIC_RATE*).
- **NX_RTSP_SERVER_ACTIVITY_TIMEOUT** The in seconds for RTSP client activity. By default, this value is defined as 60 seconds.
- **NX_RTSP_SERVER_TYPE_OF_SERVICE** The type of service for RTSP TCP requests. By default, this value is defined as *NX_IP_NORMAL*.
- **NX_RTSP_SERVER_FRAGMENT_OPTION** The fragment option for RTSP TCP requests. By default, this value is defined as *NX_FRAGMENT_OKAY*.
- **NX_RTSP_SERVER_TIME_TO_LIVE** The TTL for RTSP TCP requests. By default, this value is defined as *NX_IP_TIME_TO_LIVE*.
- **NX_RTSP_SERVER_WINDOW_SIZE** The window size for RTSP TCP requests. By default, this value is defined as 8192.

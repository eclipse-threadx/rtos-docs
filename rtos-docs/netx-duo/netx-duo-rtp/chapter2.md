---
title: Chapter 2 - Installation and use of NetX Duo RTP Sender
description: This chapter contains a description of various issues related to installation, set up, and usage of the NetX Duo RTP Sender services.
---

# Chapter 2 - Installation and use of NetX Duo RTP Sender

This chapter contains a description of various issues related to installation, setup, and usage of the NetX Duo RTP Sender services.

## Product Distribution

NetX Duo RTP Sender is available at [https://github.com/azure-rtos/netxduo](https://github.com/azure-rtos/netxduo). The package includes one source file and one header file, as follows:

- **nx_rtp_sender.h** Header file for NetX Duo RTP Sender
- **nx_rtp_sender.c** C Source file for NetX Duo RTP Sender
- **demo_rtsp_over_rtp.h** Header file for NetX Duo RTP Sender demo with RTSP server
- **demo_rtsp_over_rtp.c** C Source file for NetX Duo RTP Sender demo with RTSP server

## NetX Duo RTP Sender Installation

In order to use the NetX Duo RTP Sender API, the entire distribution mentioned previously should be copied to the same directory where NetX Duo is installed. For example, if NetX Duo is installed in the directory "*\threadx\arm7\green*" then the *nx_rtp_sender.h* and *nx_rtp_sender.c* files should be copied into this directory for RTP Sender applications.

## Using NetX Duo RTP Sender

Using the NetX Duo RTP Sender is easy. Basically, the application code must include *nx_rtp_sender.h* after it includes *tx_api.h*, and *nx_api.h*, in order to use ThreadX and NetX Duo respectively. The build project must include the RTP source code *nx_rtp_sender.c* and the host application file, and of course the ThreadX and NetX library files. This is all that is required to use NetX Duo RTP Sender.

> **Note:** Since RTP utilizes NetX Duo UDP services, UDP must be enabled with the *nx_udp_enable* call prior to using RTP.

## Small Example System of NetX Duo RTP Sender

An example of how to use NetX Duo RTP Sender is described in the header file and the source file below.  For simplicity, the return codes are assumed to be successful, therefore no further error checking is done.

> **Caution:** This is provided for demonstration purposes only and is not guaranteed to compile and run as is.
>
> Please refer to the NetX Duo RTP Sender release code distribution for demo source code file(s) that will properly build in the native Eclipse ThreadX environment. Also be aware that these demos are intentionally kept very simple as they are intended to introduce NetX Duo RTP Sender application to new users.

```C
/* This is a small demo of NetX Duo RTP Sender on the high-performance NetX TCP/IP stack. This demo relies on ThreadX and NetX Duo to show a simple rtp audio (AAC format) and video (H.264) data transfer to the client.
*/

#include "tx_api.h"
#include "nx_api.h"
#include "nx_rtp_sender.h"

/* Define demo macros.  */

#define DEMO_RTP_SERVER_ADDRESS         IP_ADDRESS(1,2,3,4)
#define DEMO_RTP_CLIENT_ADDRESS         IP_ADDRESS(1,2,3,5)
#define DEMO_RTP_CLIENT_RTP_PORT        6002
#define DEMO_RTP_CLIENT_RTCP_PORT       6003
#define DEMO_RTP_PAYLOAD_TYPE_VIDEO     96
#define DEMO_RTP_PAYLOAD_TYPE_AUDIO     97
#define DEMO_CNAME                      "someone@example.com"

/* Define demo data. */

#define DEMO_RTP_TIMESTAMP              1234
#define DEMO_NTP_MSW                    123
#define DEMO_NTP_LSW                    456

static UCHAR demo_rtp_audio_packet_data[] = "Demo rtp audio packet data";
static UCHAR demo_rtp_video_packet_data[] = "Demo rtp video packet data";

/* Define the ThreadX object control blocks...  */

static TX_THREAD                   thread_0;
static NX_PACKET_POOL              pool_0;
static NX_IP                       ip_0;

/* Define the rtp sender control block.  */
static NX_RTP_SENDER               rtp_0;
static NX_RTP_SESSION              rtp_session_0;
static NX_RTP_SESSION              rtp_session_1;

/* Define error counter. */
static ULONG                       error_counter;

/* Define user registered rtcp callback functions.  */
static UINT demo_rtcp_receiver_report_callback(NX_RTP_SESSION *session, NX_RTCP_RECEIVER_REPORT *report)
{
    /*
        TODO: Add user implementation code..

        Note!: since this callback is invoked from the IP thread, the application should not block in this callback.

        Tip: in this callback, we can obtain and record below information:
            1) report -> receiver_ssrc: the ssrc of the receiver who sends the rr report
            2) report -> fraction_loss: the fraction lost of the receiver
            3) report -> packet_loss: the cumulative number of packets lost of the receiver
            4) report -> extended_max: the extended highest sequence number received of the receiver
            5) report -> jitter: the inter-arrival jitter of the receiver
            6) report -> last_sr: the last SR timestamp of the receiver
            7) report -> delay: the delay since last SR timestamp of the receiver.
    */

    return(NX_SUCCESS);
}

static UINT demo_rtcp_sdes_callback(NX_RTCP_SDES_INFO *sdes_info)
{
    /*
        TODO: Add user implementation code..

        Note!: since this callback is invoked from the IP thread, the application should not block in this callback.

        Tip: in this callback, we can obtain and record below information:
            1) sdes_info -> ssrc: the ssrc of the receiver who sends the sdes packet
            2) sdes_info -> cname_length: the length of the cname field
            3) sdes_info -> cname: the cname field
    */

    return(NX_SUCCESS);
}

/* Define what the initial system looks like.  */
void sample_entry(NX_IP *ip_ptr, NX_PACKET_POOL *pool_ptr, VOID *dns_ptr,
                  UINT (*unix_time_callback)(ULONG *unix_time))
{

UINT        status;
NXD_ADDRESS client_ip_address;
NX_PACKET  *send_packet;


    /* Create RTP sender.  */
    status = nx_rtp_sender_create(&rtp_0, ip_ptr, pool_ptr, DEMO_CNAME, sizeof(DEMO_CNAME) - 1);
    if (status)
        error_count++;

    /* Register RR report callback function. */
    status = nx_rtp_sender_rtcp_receiver_report_callback_set(&rtp_0, demo_rtcp_receiver_report_callback);
    if (status)
        error_count++;

    /* Register SDES report callback function. */
    status = nx_rtp_sender_rtcp_sdes_callback_set(&rtp_0, demo_rtcp_sdes_callback);
    if (status)
        error_count++;

    /* Setup rtp sender session for video data send.  */
    client_ip_address.nxd_ip_version = NX_IP_VERSION_V4;
    client_ip_address.nxd_ip_address.v4 = RTP_CLIENT_ADDRESS;
    status = nx_rtp_sender_session_create(&rtp_0, &rtp_session_0, RTP_PAYLOAD_TYPE_VIDEO,
                                          0, &client_ip_address,
                                          RTP_CLIENT_RTP_PORT, RTP_CLIENT_RTCP_PORT);
    if (status)
        error_count++;

    /* Setup rtp sender session for audio data send.  */
    status = nx_rtp_sender_session_create(&rtp_0, &rtp_session_1, RTP_PAYLOAD_TYPE_AUDIO,
                                          0, &client_ip_address,
                                          RTP_CLIENT_RTP_PORT, RTP_CLIENT_RTCP_PORT);
    if (status)
        error_count++;

    /* Use h264 and aac api to send video and audio data */
    status = nx_rtp_sender_session_h264_send(&rtp_session_0, (void*)demo_rtp_video_packet_data,
                                             sizeof(demo_rtp_video_packet_data),
                                             DEMO_RTP_TIMESTAMP, DEMO_NTP_MSW, DEMO_NTP_LSW, NX_TRUE);
    if (status)
        error_count++;

    status = nx_rtp_sender_session_aac_send(&rtp_session_1, (void*)demo_rtp_audio_packet_data,
                                            sizeof(demo_rtp_audio_packet_data),
                                            DEMO_RTP_TIMESTAMP, DEMO_NTP_MSW, DEMO_NTP_LSW, NX_TRUE);
    if (status)
        error_count++;

    /* Note!: for PCM audio payload, below typical procedure could be applied to send an RTP packet. */
    //
    // /* Allocate a packet. */
    // status = nx_rtp_sender_session_packet_allocate(&rtp_session_0, &send_packet, 5 * NX_IP_PERIODIC_RATE);
    // if (status)
    //     error_count++;
    //
    // /* Copy payload data into the packet. */
    // status = nx_packet_data_append(send_packet, (void*)demo_rtp_audio_packet_data,
    //                                sizeof(demo_rtp_audio_packet_data),
    //                                rtp_0.nx_rtp_sender_ip_ptr -> nx_ip_default_packet_pool,
    //                                5 * NX_IP_PERIODIC_RATE);
    // if (status)
    //     error_count++;
    //
    // /* Send RTP packet data. */
    // status = nx_rtp_sender_session_packet_send(&rtp_session_0, send_packet, DEMO_RTP_TIMESTAMP,
    //                                            DEMO_NTP_MSW, DEMO_NTP_LSW, NX_TRUE);
    // if (status)
    //     error_count++;
}
```

## Demo System of NetX Duo RTSP Server over RTP Sender

The above example is simple. More detailed information about how to use RTP sender to send RTP packets to the client can be found inside NetX Duo rtsp folder, with a demo module called ***demo_rtsp_over_rtp***. The demo system is described below.

> **Caution:** This is provided for demonstration purposes only and is not guaranteed to compile and run as is.
>
> Please refer to the NetX Duo RTP Sender release code distribution for demo source code file(s) that will properly build in the native Eclipse ThreadX environment.  Also be aware that these demos are intentionally kept very simple as they are intended to introduce NetX Duo RTP Sender application to new users.

In this demo, the RTP sender is created and started in the *sample_entry*. The RTP sender is created with the *nx_rtp_sender_create* API. Then, the *test_rtcp_receiver_report_callback* and *test_rtcp_sdes_callback* are registered by calling API functions *nx_rtp_sender_rtcp_receiver_report_callback_set* and *nx_rtp_sender_rtcp_sdes_callback_set* separately.

In the *rtsp_setup_callback*, the transport information between RTSP server and client is set back to the *transport_ptr* parameter. Therefore, firstly, the *nx_rtp_sender_port_get* is called to get bound RTP/RTCP port pair in the specific RTP sender, and the bound port pair are recorded into *transport_ptr*. An RTP session for transferring specific payload type data requested by the RTSP client is created by calling *nx_rtp_sender_session_create*. After the session created, the session ssrc can be read by calling *nx_rtp_sender_session_ssrc_get* and recorded into *transport_ptr* as well. All recorded RTP information are set to the SETUP response to the RTSP client. If sample-based encoding payload format such as PCM is applied, the sample factor can be configured by calling *nx_rtp_sender_session_sample_factor_set*. The sample factor is computed by the audio sample size (***DEMO_AUDIO_SAMPLE_SIZE***) and audio channel number (***DEMO_AUDIO_CHANNEL_NUM***), with example as follows:

```C
  1) sample bits:  8, channel number: 1, factor = 1 * (8/8) = 1
  2) sample bits: 16, channel number: 1, factor = 1 * (16/8) = 2
  3) sample bits: 16, channel number: 2, factor = 2 * (16/8) = 4
```

In the *rtsp_play_callback*, the current sequence number in RTP session is read by calling *nx_rtp_sender_session_sequence_number_get* and set into RTSP response to the RTSP client.

In the *rtsp_teardown_callback*, since there is no need to maintain the RTP session(s) with the specific receiver when this callback is invokes, it is fine to delete the session by calling *nx_rtp_sender_session_delete*.

RTP payload data sending is mainly implemented in the *test_server_entry*. Both unicast and multicast RTP payload data sending are supported. By default, unicast is applied and multicast can replace unicast by defining the macro ***DEMO_MULTICAST_ENABLED***. This demo shows 2 ways how RTP payload data sending is triggered. By default, users can trigger audio data transfer or video data transfer by calling *tx_event_flags_set(&demo_test_events, DEMO_AUDIO_DATA_READY_EVENT, TX_OR)* or *tx_event_flags_set(&demo_test_events, DEMO_VIDEO_DATA_READY_EVENT, TX_OR)* separately. Users can also define the macro ***DEMO_PLAY_BY_TIMER*** to enable software timer triggered RTP payload data sending. When this macro is defined, a software timer is automatically enabled with default 10-millisecond period defined by the macro ***DEMO_PLAY_TIMER_INTERVAL***. Users can define the value of macros ***DEMO_AUDIO_FRAME_PER_SECOND*** and ***DEMO_VIDEO_FRAME_PER_SECOND*** to control the audio and video data sending rate. The default value of these 2 macros are 30 and 43, respectively. The sending rate shall be the same as the real audio or video FPS to guarantee media playing normally. This demo also shows how different RTP payload data to send, with 2 video types and 2 audio types. By default, the demo shows H.264 and AAC data sending. Users can replace video type with MJPEG by re-defining the macro **DEMO_AUDIO_FORMAT** with **DEMO_VIDEO_FORMAT_MJPEG**; and replace audio type with PCM by re-defining the macro **DEMO_AUDIO_FORMAT** with **DEMO_AUDIO_FORMAT_PCM**.

No matter which configuration combination is applied as described above, users need to implement the following callback function(s) to provide payload data and also define following macro(s):

```C
/* Define this callback function if users want to execute any code after RTSP PLAY command is received; otherwise, left this macro to be its default value. For example, in this function, it is suitable for users to execute camera initialization or audio initialization corresponding codes. */

#define DEMO_MEDIA_DATA_INIT  demo_media_data_init_callback

VOID (*demo_media_data_init_callback)(VOID)
{
    /* User implementation here to initialize media data.  */
}
```

```C
/* Define this callback function if users want to run the demo code for RTP audio data transfer; otherwise, left this macro to be its default value.  */
#define DEMO_AUDIO_DATA_READ  demo_audio_data_read_callback

UINT (*demo_audio_data_read_callback)(UCHAR **data_ptr, ULONG *data_size)
{
    /* User implementation here to read audio data.  */
}
```

```C
/* Define this callback function if users want to run the demo code for RTP video data transfer; otherwise, left this macro to be its default value.  */
#define DEMO_VIDEO_DATA_READ  demo_video_data_read_callback

UINT (*demo_video_data_read_callback)(UCHAR **data_ptr, ULONG *data_size)
{
    /* User implementation here to read video data.  */
}
```

## Configuration Options

There are several configuration options for building NetX Duo RTP sender. The default values are listed but can be redefined prior to inclusion of *nx_rtp_sender.h*. The following list describes each in detail:

- **NX_RTP_SENDER_TYPE_OF_SERVICE** The type of service for RTP UDP requests. By default, this value is defined as *NX_IP_NORMAL*.
- **NX_RTP_SENDER_FRAGMENT_OPTION** The fragment option for RTP UDP requests. By default, this value is defined as *NX_FRAGMENT_OKAY*.
- **NX_RTP_SENDER_TIME_TO_LIVE** The TTL for RTP UDP requests. By default, this value is defined as *0x80*.
- **NX_RTP_SENDER_QUEUE_DEPTH** The maximum depth of receive queue for RTSP UDP requests. By default, this value is defined as *5*.
- **NX_RTP_SENDER_PACKET_TIMEOUT** The suspension option for packet data operations. By default, this value is defined as *NX_IP_PERIODIC_RATE*.
- **NX_RTCP_INTERVAL** The period for automatic RTCP packet sending. By default, this value is defined as *5*.

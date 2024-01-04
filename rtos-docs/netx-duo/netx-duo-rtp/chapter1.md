---
title: Chapter 1 - Introduction to the NetX Duo RTP Sender
description: The real-time transport protocol (RTP) is a protocol designed for providing end-to-end real-time data delivery service.
---

# Chapter 1 - Introduction to the NetX Duo RTP Sender

RTP Sender utilizes user datagram protocol (UDP) services to perform its data packets (RTP) and control packets (RTCP) transfer function. The real-time transport protocol (RTP) is a protocol designed for providing end-to-end real-time data delivery service, commonly for audio and video content. RTP relies on the transport protocol functionality provided by UDP, and contributes more transport protocol functionality for real-time transfer. The RTP control protocol (RTCP) works with RTP, provides periodic statistics to other session participants and parses received periodic statistics from other session participants in RTP session. Data distribution quality provided by RTCP helps the application to adjust the data distribution rate and quality. Moreover, the automatic sent RTCP sender report by RTP Sender includes the link information between RTP timestamp and NTP timestamp, which could be used by the application to synchronize RTP data packets in different sessions (e.g. video and audio synchronization). 

RTP Sender typically works with RTSP server to provide the RTP/RTCP services for the RTSP server. The RTSP server is responsible for the RTSP session management, and the RTP Sender is responsible for the RTP/RTCP session management. The RTSP server shall create the RTP Sender instance, and then create the RTP session instance on the RTP Sender. The RTSP server shall also call the RTP Sender APIs to send the RTP/RTCP packets.

The RTP payload format is determined by the application. The RTP Sender supports the following payload formats: MJPEG and H.264 for video, PCM and AAC for audio. Users can also implement other payload formats by using the existing RTP Sender APIs.

## NetX Duo RTP Sender Requirements 

The NetX Duo RTP Sender requires that an IP instance has already been created. In addition, UDP must be enabled on that same IP instance.

## NetX Duo RTP Sender Specification

The NetX Duo RTP Sender implement is compliant with [RFC 3550](https://www.rfc-editor.org/rfc/rfc3550.txt).

Below table shows supported RTP payload format implementation details in RTP Sender:
Format | Type | Specification | Corresponding RTP Sender API Function(s) to send
--- | --- | --- | ---
PCM | Audio | [RFC 3551](https://www.rfc-editor.org/rfc/rfc3551.txt) | nx_rtp_sender_session_packet_send(), nx_rtp_sender_session_sample_factor_set()
AAC | Audio | [RFC 3640](https://www.rfc-editor.org/rfc/rfc3640.txt) and [RFC 6416](https://www.rfc-editor.org/rfc/rfc6416.txt) | nx_rtp_sender_session_aac_send()
MJPEG | Video | [RFC 2435](https://www.rfc-editor.org/rfc/rfc2435.txt) | nx_rtp_sender_session_jpeg_send()
H.264 | Video | [RFC 6184](https://www.rfc-editor.org/rfc/rfc6184.txt) | nx_rtp_sender_session_h264_send()


## NetX Duo RTP Sender Constraints

***RTP:***
- *CSRC*: contributing source feature is not supported since RTP sender is not targeted for RTP mixer application. Thus, the CSRC count is always zero in RTP header (i.e. no CSRC list).

***RTP Payload Format:***
- Complete frame data is required (i.e. marker bit shall always be *NX_TRUE*) to send RTP payload format data including JPEG/MJPEG, H.264 and AAC. 
- *JPEG/MJPEG*: **8-bit** sample precision only, **YUV420 or YUV422** type only, encoded with **standard huffman** tables only.
- *H.264*: **non-interleaved mode** only (i.e. indicate packetization-mode=1 in SDP).
- *AAC*: **aac-hbr mode** only, **13-bit** frame length only.

***RTCP:***
- Packet chain is not supported.
- *SDES*: only **CNAME** field is parsed for both sent and received RTCP SDES packet.
- *BYE*: the goodbye RTCP packet is ignored and not known by the user.

## NetX Duo RTP Sender Basic Operation

An application creates an RTP sender by calling ***nx_rtp_sender_create()***. Once the RTP sender is created, the application can get the bound RTP/RTCP port pair by calling ***nx_rtp_sender_port_get()***. Within the RTP sender, the application can then create an RTP session by calling ***nx_rtp_sender_session_create()***. In RTP layer, there is no limitation for the number of sessions created within an RTP sender.

An RTP session is responsible for sending one specific type of RTP packets to one specific receiver (the receiver could be a multicast group). Once the RTP session is created, many configurations or parameters are fixed such as the payload type transferred in the session, the ip interface used to transfer RTP packets, the receiver's ip address and RTP/RTCP port pair, the initial random SSRC and sequence number for the session. The application can get the session SSRC by calling ***nx_rtp_sender_session_ssrc_get()***, and get the current sequence number in the session by calling ***nx_rtp_sender_session_sequence_number_get()***.

The application chooses different RTP session send function(s) in RTP sender depending on the payload type. So far, the application can call ***nx_rtp_sender_session_jpeg_send()*** to send JPEG payload, and call ***nx_rtp_sender_session_h264_send()*** to send H.264 payload, and call ***nx_rtp_sender_session_aac_send()*** to send AAC payload. The application can also call ***nx_rtp_sender_session_packet_send()*** to send other payload types or send own implemented RTP sender not supported features for the existing payload types. The procedure for the application to use the function nx_rtp_sender_session_packet_send:
- Step 1: allocate an RTP packet by calling ***nx_rtp_sender_session_packet_allocate()***.
- Step 2: fill the RTP packet with the payload data by calling ***nx_packet_data_append()***.
- Step 3: send the RTP packet by calling ***nx_rtp_sender_session_packet_send()***.

> **Warning:** Unless an error is returned, the application should not release the allocated RTP packet by calling ***nx_packet_release()*** after calling the function ***nx_packet_data_append()*** or ***nx_rtp_sender_session_packet_send()***. Doing so will cause unpredictable results because the network driver will also try to release the packet after transmission.

Transferring **sample-based encoding** payload such as PCM audio payload is special, due to the way how RTP timestamp increases. The application shall call enable sample-based encoding payload transfer for the session by calling ***nx_rtp_sender_session_sample_factor_set()*** and set a none-zero value. The sample factor is computed and provided by the application. With the sample factor configured, when fragmentation happens, the function nx_rtp_sender_session_packet_send is able to handle the automatic increase RTP timestamp as required for each fragmented packet. The application can then send sample-based encoding payload through the procedure described above.

Received **RTCP RR reports** (receiver report) are parsed by RTP sender automatically. The application can choose to get RR reports through registering the callback function by calling ***nx_rtp_sender_rtcp_receiver_report_callback_set()***. The callback function is declared as:

```C
UINT nx_rtp_sender_rtcp_receiver_report_callback_set(NX_RTP_SENDER *rtp_sender,
    UINT (*rtcp_rr_cb)(NX_RTP_SESSION *, NX_RTCP_RECEIVER_REPORT *));
```

As the RTP sender receives a RTCP RR packet inside a specific session, it invokes the callback function if the function is set. The callback function passes the pointer to the session and the pointer to the receiver report structure. The application can then parse the receiver report structure to get the information it needs.

Received **RTCP SDES packets** are parsed by RTP sender automatically as well. In SDES packet, only the CNAME field is parsed and extracted. The application can choose to get CNAME through registering the callback function by calling ***nx_rtp_sender_rtcp_sdes_callback_set()***. The callback function is declared as:

```C
UINT nx_rtp_sender_rtcp_sdes_callback_set(NX_RTP_SENDER *rtp_sender,
    UINT (*rtcp_sdes_cb)(NX_RTCP_SDES_INFO *));
```

As the RTP sender receives a RTCP SDES packet, it invokes the callback function if the function is set. The callback function passes the pointer to the SDES information structure. The application can then parse the SDES information structure to get the CNAME information.

When the connection with the receiver ends, the application can unlink the session(s) corresponding to the receiver from the RTP sender by calling ***nx_rtp_sender_session_delete()***. The application can also release all the resources of the RTP sender by calling ***nx_rtp_sender_delete()***.

> **Note:** The application can successfully delete an RTP sender only when all linked sessions having been deleted.

---
title: Chapter 1 - Introduction to the NetX Duo RTSP Server
description: The Real-Time Streaming Protocol (RTSP) is a protocol designed for multimedia network remote control.
---


The Real-Time Streaming Protocol (RTSP) is a protocol designed for multimedia network remote control. NetX Duo RTSP Server utilizes transmission control protocol (TCP) services to perform its packets transfer function. Therefore, RTSP provides a highly reliable control service.

RTSP Server normally relies on RTP to transmit continuous multimedia streams. RTSP server is responsible for the RTSP session management and it also creates the RTP Sender instance, and then creates the RTP session instance on the RTP Sender, and calls the RTP Sender APIs to send the RTP/RTCP packets.

## RTSP Server Requirements

In order to function properly, the NetX Duo RTSP Server package requires that a NetX Duo is installed. In addition, an IP instance must already be created, and TCP must be enabled on that same IP instance. The RTSP Server package has no further requirements.

## RTSP RFCs

RTSP Server is compliant with the following RFCs:

[RFC 2326 Real Time Streaming Protocol (RTSP)](https://www.rfc-editor.org/rfc/rfc2326).

## RTSP Concepts

***Client-Server Model***: RTSP operates on a client-server model where the client initiates requests to the server to control the streaming session.

***Control and Data Channels***: RTSP uses two channels: the control channel for sending control commands (play, pause, etc.) and the data channel for streaming the actual media content.

***Transport Method***:

- Unicast: the media stream is sent from the RTSP server to the client which raises the RTSP request, with the port number specified in the SETUP request.

- Multicast (server choose address): the RTSP server chooses the multicast address and port number, and sends them to the client in the DESCRIBE response. This is the typical way for live media transmission.

- Multicast (client choose address): not supported. The RTSP server is not supported to join in an existing multicast conference through the client's request.


## RTSP Server Communication/Method

RTSP Server methods referred in RFC 2326:

- **OPTIONS**: supported by default. This method requests the methods supported by the server.
```C
Example:
    RTSP Client -> RTSP Server: OPTIONS * RTSP/1.0
                                CSeq: 1
    RTSP Server -> RTSP Client: RTSP/1.0 200 OK
                                CSeq: 1
                                Public: OPTIONS, DESCRIBE, SETUP, TEARDOWN, PLAY, PAUSE, SET_PARAMETER
```

- **DESCRIBE**: supported. This method requests information about the media stream. The application can choose to enable this method by registering the callback function ***nx_rtsp_server_describe_callback_set()***.
```C
Example:
    RTSP Client -> RTSP Server: DESCRIBE rtsp://10.0.0.55:554 RTSP/1.0
                                CSeq: 2
                                Accept: application/sdp
    RTSP Server -> RTSP Client: RTSP/1.0 200 OK
                                CSeq: 2
                                Content-Type: application/sdp
                                Content-Length: 149

                                v=0
                                s=PCM audio, streamed by the NetX Duo RTSP Server
                                m=audio 0 RTP/AVP 11
                                a=rtpmap:11 L16/44100/1
                                a=fmtp:11 emphasis=50-15
                                a=ptime:5
                                a=control:trackID=0
```

- **SETUP**: supported. This method prepares the server for streaming. The application **MUST** enable this method by registering the callback function ***nx_rtsp_server_setup_callback_set()***.
```C
Example:
    RTSP Client -> RTSP Server: SETUP rtsp://10.0.0.55:554/trackID=0 RTSP/1.0
                                CSeq: 3
                                Transport: RTP/AVP;unicast;client_port=55612-55613
    RTSP Server -> RTSP Client: RTSP/1.0 200 OK
                                CSeq: 3
                                Server: RTSP Server
                                Session: 15724;timeout=60
                                Transport: RTP/AVP;unicast;client_port=55612-55613;server_port=5004-5005;ssrc=6334
```

- **PLAY**: supported. This method starts playing the media stream. The application **MUST** enable this method by registering the callback function ***nx_rtsp_server_play_callback_set()***.
```C
Example:
    RTSP Client -> RTSP Server: PLAY rtsp://10.0.0.55:554 RTSP/1.0
                                CSeq: 4
                                Session: 15724
                                Range: npt=0.000-
    RTSP Server -> RTSP Client: RTSP/1.0 200 OK
                                CSeq: 4
                                Server: RTSP Server
                                RTP-Info: url=rtsp://10.0.0.55:554/trackID=0;seq=26500;rtptime=19169
                                Range: npt=0.000-
                                Session: 15724;timeout=60
```

- **PAUSE**: supported. This method pauses the media stream. The application can choose to enable this method by registering the callback function ***nx_rtsp_server_pause_callback_set()***.
```C
Example:
    RTSP Client -> RTSP Server: PAUSE rtsp://10.0.0.55:554 RTSP/1.0
                                CSeq: 5
                                Session: 15724
    RTSP Server -> RTSP Client: RTSP/1.0 200 OK
                                CSeq: 5
                                Server: RTSP Server
                                Range: npt=0.000-
                                Session: 15724;timeout=60
```

- **SET_PARAMETER**: supported. This method sets the value of a parameter. The application can choose to enable this method by registering the callback function ***nx_rtsp_server_set_parameter_callback_set()***.
```C
Example:
    RTSP Client -> RTSP Server: SET_PARAMETER rtsp://10.0.0.55:554 RTSP/1.0
                                CSeq: 6
                                Content-Type: text/parameters
                                Content-Length: 15

                                volume: 0.5
    RTSP Server -> RTSP Client: RTSP/1.0 200 OK
                                CSeq: 6
                                Server: RTSP Server
                                Session: 15724;timeout=60
```

- **TEARDOWN**: supported. This method ends the session and stops streaming. The application **MUST** enable this method by registering the callback function ***nx_rtsp_server_teardown_callback_set()***.
```C
Example:
    RTSP Client -> RTSP Server: TEARDOWN rtsp://10.0.0.55:554 RTSP/1.0
                                CSeq: 7
                                Session: 15724
    RTSP Server -> RTSP Client: RTSP/1.0 200 OK
                                CSeq: 7
                                Server: RTSP Server
```

- *ANNOUNCE*: not supported. The application can use *DESCRIBE* method to present the description of RTSP Server.
- *GET_PARAMETERS*: not supported.
- *REDIRECT*: not supported.
- *RECORD*: not supported.
- *Embedded (Interleaved) Binary Data*: not supported.

## RTSP Server Basic Operation

An application can create an RTSP server by calling ***nx_rtsp_server_create()***. After that, all functional needed RTSP method callback functions shall be registered before starting the server. Mandatory registered RTSP methods **MUST** be registered before staring the RTSP server; otherwise, starting the RTSP server will fail.

*Mandatory registered RTSP methods*:
- *SETUP*: registered by calling ***nx_rtsp_server_setup_callback_set()***
- *PLAY*: registered by calling ***nx_rtsp_server_play_callback_set()***
- *TEARDOWN*: registered by calling ***nx_rtsp_server_teardown_callback_set()***

*Optional registered RTSP methods*:
- *DESCRIBE*: registered by calling ***nx_rtsp_server_describe_callback_set()***
- *PAUSE*: registered by calling ***nx_rtsp_server_pause_callback_set()***
- *SET_PARAMETER*: registered by calling ***nx_rtsp_server_set_parameter_callback_set()***

When all mandatory RTSP methods are successfully registered, the application can start the RTSP server by calling ***nx_rtsp_server_start()***. The RTSP server is then ready to accept incoming RTSP requests. The application can temporarily stop the RTSP server by calling ***nx_rtsp_server_stop()*** and restart the RTSP server by calling ***nx_rtsp_server_start()***. The application can also release the resources through deleting the RTSP server by calling ***nx_rtsp_server_delete()***.

> **Note:** A software timer used to check RTSP session timeout is created with the RTSP server. The application can change the timeout value by modifying the macro ***NX_RTSP_SERVER_ACTIVITY_TIMEOUT***. The default timeout value is 60 seconds. The software timer is automatically refreshed when an RTSP request is received from the client; therefore, some RTSP client(s) usually send a *OPTION* request to keep the RTSP session alive. Besides, if RTP is applied as the real-time data sending mechanism, the periodical received RTCP report from the receiver also refreshes the software timer. Thus, the application shall call the function ***nx_rtsp_server_keepalive_update()*** in the callback function of RTCP RR report receiving. Reference the user guide in the RTP module for more details about RTCP RR report.

As the RTSP server receives a *DESCRIBE* request, it invokes the callback function if the function is set. The callback function passes the pointer to the RTSP client and the pointer to the URI with URI length. In the callback function, since **SDP** (Session Description Protocol) is applied as the standard to present media initialization information for RTSP server resources, the application **SHOULD** call ***nx_rtsp_server_sdp_set()*** which passes the SDP string to RTSP server. The SDP string contains the information about the supported multimedia format and corresponding details of the RTSP server. These information is parsed and attached in the response of the *DESCRIBE* request to the RTSP client. Besides, these information probably contains the mechanism utilized by RTSP to send real-time data stream(s). RTP is normally a typical protocol to use. The SDP string shall be compliant with the standard if describing RTP. Except above operations, this callback function may also execute any other application needed code. The callback function is declared as:

```C
UINT (*nx_rtsp_server_method_describe_callback)(NX_RTSP_CLIENT *rtsp_client_ptr, UCHAR *uri, UINT uri_length);
```

> **Note:** A standard format SDP string which can describe current RTSP server multimedia resources shall be prepared for the *DESCRIBE* request callback function.

As the RTSP server receives a *SETUP* request, it invokes the callback function which must have been set. The callback function passes the pointer to the RTSP client, the pointer to the URI with URI length and the pointer to the transport structure. The application shall parse the URI string first, and if RTP is applied as the real-time data sending mechanism, the application can setup specific RTP sessions based on the parsed setup requirement from the client, and may also execute any other application needed code. The callback function is declared as:

```C
UINT (*nx_rtsp_server_method_setup_callback)(NX_RTSP_CLIENT *rtsp_client_ptr, UCHAR *uri, UINT uri_length,
                                             NX_RTSP_TRANSPORT *transport_ptr);
```

As the RTSP server receives a *PLAY* request, it invokes the callback function which must have been set. The callback function passes the pointer to the RTSP client, the pointer to the URI with URI length and the pointer to the string with the playing time range information. The application shall parse the range string and if RTP is applied as the real-time data sending mechanism, read and set RTP sequence number with the play range into the response of *PLAY* request to the client by calling ***nx_rtsp_server_rtp_info_set()***, and may execute any other application needed code. The callback function is declared as:

```C
UINT (*nx_rtsp_server_method_play_callback)(NX_RTSP_CLIENT *rtsp_client_ptr, UCHAR *uri, UINT uri_length,
                                            UCHAR *range_ptr, UINT range_length);
```

As the RTSP server receives a *PAUSE* request, it invokes the callback function if the function is set. The callback function passes the pointer to the RTSP client and the pointer to the URI with URI length and the pointer to the string with the playing time range information. In the callback function, the application could execute own code to temporarily pause current data sending. The callback function is declared as:

```C
UINT (*nx_rtsp_server_method_pause_callback)(NX_RTSP_CLIENT *rtsp_client_ptr, UCHAR *uri, UINT uri_length,
                                             UCHAR *range_ptr, UINT range_length);
```

> **Note:** In the callback function of *PLAY* and *PAUSE* request, through parsing the received range string, the application can execute nothing with the *Range* field if the requested NPT start and end time in *Range* field initially looks like "*Range: npt=0.000-*". It is also possible that none zero time *Range* field is requested by the receiver, then the application can set a significant NPT start and end time in *Range* field inside the response of *PLAY* or *PAUSE* request by calling ***nx_rtsp_server_range_npt_set()***. The application set time range may be different with the parsed range requested by the receiver, the value can vary depending on the real application.

As the RTSP server receives a *SET_PARAMETER* request, it invokes the callback function if the function is set. The callback function passes the pointer to the RTSP client and the pointer to the URI with URI length and the pointer to the string with the parameter information. In the callback function, the application could execute own code to set the value of the parameter(s) parsed by the request from the client. The callback function is declared as:

```C
UINT (*nx_rtsp_server_method_set_parameter_callback)(NX_RTSP_CLIENT *rtsp_client_ptr, UCHAR *uri, UINT uri_length,
                                                     UCHAR *parameter_ptr, ULONG parameter_length);
```

As the RTSP server receives a *TEARDOWN* request, it invokes the callback function which must have been set. The callback function passes the pointer to the RTSP client, the pointer to the URI with URI length. If RTP is applied as the real-time data sending mechanism, the application shall delete the related RTP session(s) with the receiver, and execute any other application needed code. The callback function is declared as:

```C
UINT (*nx_rtsp_server_method_teardown_callback)(NX_RTSP_CLIENT *rtsp_client_ptr, UCHAR *uri, UINT uri_length);
```

As the RTSP server receives a disconnection from the receiver and so, invokes the application callback function if the function is set. The callback function passes the pointer to the RTSP client. In the callback function, the application could execute own needed code. The callback function is declared as:

```C
UINT (*nx_rtsp_server_disconnect_callback)(NX_RTSP_CLIENT *rtsp_client_ptr);
```

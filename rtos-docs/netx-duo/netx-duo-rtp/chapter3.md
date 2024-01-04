---
title: Chapter 3 - Description of NetX Duo RTP Sender Services
description: This chapter contains a description of all NetX Duo RTP sender services (listed below) in alphabetic order.
---

# Chapter 3 - Description of NetX Duo RTP sender Services

This chapter contains a description of all NetX Duo RTP sender services (listed below) in alphabetic order.

> **Note:** In the **Return Values** section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## nx_rtp_sender_create

Create an RTP sender instance

### Prototype

```C
UINT nx_rtp_sender_create(
    NX_RTP_SENDER *rtp_sender,
    NX_IP *ip_ptr,
    NX_PACKET_POOL *pool_ptr,
    CHAR *cname,
    UCHAR cname_length)
```

### Description

This service creates an RTP sender on the specified IP.

### Input Parameters

- *rtp_sender*: Pointer to RTP sender.
- *ip_ptr*: Pointer to the created IP instance.
- *pool_ptr*: Pointer to RTP packet pool.
- *cname*: Pointer to the name string shown in RTCP SDES report.
- *cname_length*: Length of the name string.

### Return Values

- **NX_SUCCESS** (0x00) Successful RTP sender creation.
- **NX_NO_FREE_PORTS** (0x45) No free ports available.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Create the RTP Sender instance "rtp_0". */

status = nx_rtp_sender_create(&rtp_0, ip_ptr, pool_ptr,
                              "someone@example.com",
                              sizeof("someone@example.com") - 1);

/* If status is NX_SUCCESS an RTP sender instance was successfully created. */
```

## nx_rtp_sender_delete

Delete an RTP sender instance

### Prototype

```C
UINT nx_rtp_sender_delete(NX_RTP_SENDER *rtp_sender);
```

### Description

This service deletes a previously created RTP sender. The RTP sender can only be deleted
when all sessions have been deleted.

### Input Parameters

- *rtp_sender*: Pointer to RTP sender.

### Return Values

- **NX_SUCCESS** (0x00) Successful RTP sender deletion.
- **NX_DELETE_ERROR** (0x10) Error since there is at least one session not deleted.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Delete the RTP sender instance "rtp_0". */

status = nx_rtp_sender_delete(&rtp_0);

/* If status is NX_SUCCESS the RTP sender instance was successfully deleted. */
```

## nx_rtp_sender_port_get

Get bound RTP and RTCP ports for specific RTP sender

### Prototype

```C
UINT nx_rtp_sender_port_get(
    NX_RTP_SENDER *rtp_sender,
    UINT *rtp_port,
    UINT *rtcp_port);
```

### Description

This service returns the bound RTP and RTCP ports for specified RTP sender.
Function nx_rtp_sender_create shall be called before calling this service.

### Input Parameters

- *rtp_sender*: Pointer to RTP client.
- *rtp_port*: Pointer to returned bound RTP port.
- *rtcp_port*: Pointer to returned bound RTCP port.

### Return Values

- **NX_SUCCESS** (0x00) Successful ports get.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Get bound RTP and RTCP ports for "rtp_0". */

status = nx_rtp_sender_port_get(&rtp_0, &rtp_port, &rtcp_port);

/* If status is NX_SUCCESS the bound RTP and RTCP ports were successfully obtained. */
```

## nx_rtp_sender_session_create

Create an RTP session instance on specific RTP sender

### Prototype

```C
UINT nx_rtp_sender_session_create(
    NX_RTP_SENDER *rtp_sender,
    NX_RTP_SESSION *session,
    ULONG payload_type,
    UINT interface_index,
    NXD_ADDRESS *receiver_ip_address,
    UINT receiver_rtp_port_number,
    UINT receiver_rtcp_port_number)
```

### Description

This service creates an RTP session on the specified RTP sender.

### Input Parameters

- *rtp_sender*: Pointer to RTP sender.
- *session*: Pointer to the RTP session instance to be created.
- *payload_type*: Format of RTP payload transferred in this session.
- *interface_index*: IP interface instance.
- *receiver_ip_address*: The receiver's IP address.
- *receiver_rtp_port_number*: The receiver's RTP port.
- *receiver_rtcp_port_number*: The receiver's RTCP port.

### Return Values

- **NX_SUCCESS** (0x00) Successful RTP session creation.
- **NX_IP_ADDRESS_ERROR** (0x21) Invalid IP address.
- **NX_INVALID_PARAMETERS** (0x4D) Invalid input argument.
- NX_PTR_ERROR (0x07) Invalid pointer input.
- NX_INVALID_INTERFACE (0x4C) Invalid interface index.

### Allowed From

Threads

### Example

```C
/* Create the RTP session instance "rtp_session_0" with "rtp_0". */

status = nx_rtp_sender_session_create(&rtp_0, &rtp_session_0, 96, 0,
                                      IP_ADDRESS(10, 1, 0, 55), 5004, 5005);

/* If status is NX_SUCCESS an RTP session instance was successfully created. */
```

## nx_rtp_session_delete

Delete an RTP session instance

### Prototype

```C
UINT nx_rtp_sender_session_delete(NX_RTP_SESSION *session);
```

### Description

This service deletes a previously created RTP session.

### Input Parameters

- *session*: Pointer to RTP session.

### Return Values

- **NX_SUCCESS** (0x00) Successful RTP session deletion.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Delete the RTP session instance "rtp_session_0". */

status = nx_rtp_sender_session_delete(&rtp_session_0);

/* If status is NX_SUCCESS the RTP session instance was successfully deleted. */
```

## nx_rtp_sender_session_sample_factor_set

Set the sample factor value for sample-based payload in RTP session

### Prototype

```C
UINT nx_rtp_sender_session_sample_factor_set(
    NX_RTP_SESSION *session,
    UINT factor);
```

### Description

This service sets the sample factor value for sample-based payload in RTP session.
The sample factor determines the timestamp increasing rate for each RTP packet in the function
_nx_rtp_sender_session_packet_send when the fragmentation feature triggered in sample-based
mode since timestamp shall be increased in a pace for each fragmentation packet.

The default sample factor value 0, representing frame-based mode applied. User can use this
function to set a non-zero sample factor, with automatically triggering sample-based mode.
    Examples about how the sample factor is computed for audio payload:
- sample bits:  8, channel number: 1, factor = 1 * (8/8) = 1
- sample bits: 16, channel number: 1, factor = 1 * (16/8) = 2
- sample bits: 16, channel number: 2, factor = 2 * (16/8) = 4

### Input Parameters

- *session*: Pointer to RTP session.
- *factor*: The sampling factor.

### Return Values

- **NX_SUCCESS** (0x00) Successful sampling factor set.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Set the sampling factor value 2 for "rtp_session_0". */

status = nx_rtp_sender_session_sample_factor_set(&rtp_session_0, 2);

/* If status is NX_SUCCESS the sampling factor was successfully set. */
```

## nx_rtp_sender_session_packet_allocate

Return allocated RTP packet from the packet pool given by rtp_sender_create

### Prototype

```C
UINT nx_rtp_sender_session_packet_allocate(
    NX_RTP_SESSION *session,
    NX_PACKET **packet_ptr,
    ULONG wait_option);
```

### Description

This service allocates an RTP packet from the packet pool given by rtp_sender_create,
and returns this packet to the user.

### Input Parameters

- *session*: Pointer to RTP session.
- *packet_ptr*: Pointer to allocated packet.
- *wait_option*: Suspension option.

### Return Values

- **NX_SUCCESS** (0x00) Successful RTP packet get.
- NX_PTR_ERROR (0x07) Invalid pointer input.
- NX_UNDERFLOW (0x02) Invalid packet prepend pointer.

### Allowed From

Threads

### Example

```C
/* Allocate and get an RTP packet for "rtp_session_0". */

status = nx_rtp_sender_session_packet_allocate(&rtp_session_0, &packet_ptr, NX_WAIT_FOREVER);

/* If status is NX_SUCCESS the RTP packet was successfully allocated and returned. */
```

## nx_rtp_sender_session_packet_send

Send the packet data in RTP format on specific session

### Prototype

```C
UINT nx_rtp_sender_session_packet_send(
    NX_RTP_SESSION *session,
    NX_PACKET *packet_ptr,
    ULONG timestamp,
    ULONG ntp_msw,
    ULONG ntp_lsw,
    UINT marker);
```

### Description

This service sends passed packet data in RTP format, and calls RTP sender rtcp send function as the entry to send RTCP report.

In the function prototype, it is apparent that each packet is linked to one RTP timestamp which represents the time of the first sample in the packet. The initial value of the timestamp **SHOULD** be random and the timestamp shall be increased by the application monotonically and linearly in time. The timestamp is used to control the packet stream playing rate at the receiver. For example, for frame-based encoding payload, if the RTP sender is sending a 90000Hz video stream with 30 frames per second, the timestamp should be increased by 90000/30=**3000 for each frame** of the video. If this function is called more than 1 time for sending a single frame, the timestamp could be the same value for each call.

Besides, for sample-based encoding payload such as PCM format audio stream, the timestamp should be increased by **the number of sampling bytes** for each function call. For example, if the RTP sender is sending an 8000Hz audio stream with 16 bits per sample and with 2 audio channels, for each function call, the timestamp should be increased by: ***packet_ptr -> nx_packet_length / ((16 / 8) * 2)***. With the increasing timestamp, the receiver is able to play the packet stream at the target rate.

The 64-bit network time are composed of two 32-bit arguments ntp_msw and ntp_lsw. The network time is filled into the RTCP sender report header *NTP timestamp* field. Each RTCP sender report header includes a network time with a RTP timestamp, which provides the link between them. Except indicating the wallclock time, the network time helps to **synchronize different RTP streams** in the RTP sender, since the application shall apply the same network time increasing mechanism for different RTP streams.

### Input Parameters

- *session*: Pointer to RTP session.
- *packet_ptr*: Pointer to packet data to send.
- *timestamp*: RTP timestamp for current data.
- *ntp_msw*: Most significant word of network time.
- *ntp_lsw*: Least significant word of network time.
- *marker*: Marker bit for significant event such as frame boundary.

### Return Values

- **NX_SUCCESS** (0x00) Successful packet data sending.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Send packet in RTP format on "rtp_session_0". */

status = nx_rtp_sender_session_packet_send(&rtp_session_0, &send_packet,
                                           rtp_timestamp, ntp_msw, ntp_lsw, NX_TRUE);

/* If status is NX_SUCCESS the packet was successfully sent. */
```

## nx_rtp_sender_session_sequence_number_get

Get the current RTP sequence number on specific session

### Prototype

```C
UINT nx_rtp_sender_session_sequence_number_get(
    NX_RTP_SESSION *session,
    UINT *sequence_number);
```

### Description

This service returns the current RTP sequence number. The application such as RTSP may
get sequence number by calling this function and pass it to the receiver in order to
provide the initial RTP sequence number for the receiver.

### Input Parameters

- *session*: Pointer to RTP session.
- *sequence_number*: Pointer to returned sequence number.

### Return Values

- **NX_SUCCESS** (0x00) Successful sequence number get.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Get current sequence number for "rtp_session_0". */

status = nx_rtp_sender_session_sequence_number_get(&rtp_session_0, &seq_number);

/* If status is NX_SUCCESS the sequence number was successfully obtained. */
```

## nx_rtp_sender_session_ssrc_get

Get the current SSRC on specific session

### Prototype

```C
UINT nx_rtp_sender_session_ssrc_get(
    NX_RTP_SESSION *session,
    ULONG *ssrc);
```

### Description

This service returns the current SSRC. The application such as RTSP may get SSRC by calling
this function and and pass it to the receiver as the identifier for each session.

### Input Parameters

- *session*: Pointer to RTP session.
- *ssrc*: Pointer to returned SSRC.

### Return Values

- **NX_SUCCESS** (0x00) Successful SSRC get.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Get current SSRC for "rtp_session_0". */

status = nx_rtp_sender_session_ssrc_get(&rtp_session_0, &ssrc);

/* If status is NX_SUCCESS the SSRC was successfully obtained. */
```

## nx_rtp_sender_session_jpeg_send

Send a complete JPEG format frame in RTP/JPEG format on specific session

### Prototype

```C
UINT nx_rtp_sender_session_jpeg_send(
    NX_RTP_SESSION *session,
    UCHAR *frame_data,
    ULONG frame_data_size,
    ULONG timestamp,
    ULONG ntp_msw,
    ULONG ntp_lsw,
    UINT marker);
```

### Description

This service parses and makes the passed data in RTP/JPEG format, and then
calls RTP session packet send function to send these data in RTP packet.
    The function references RFC 2435 as the standard with following notes:
- A complete jpeg scan file inside frame data buffer is required.
- Use dynamic quantization table mapping.
- The provided jpeg scan file shall be 8-bit sample precision, YUV420 or YUV422 type, and encoded with standard huffman tables.
- Restart marker is not supported.

**Reference the description of the function nx_rtp_sender_session_packet_send for more details about how timestamp, ntp_msw and ntp_lsw are increased for each function call.**

### Input Parameters

- *session*: Pointer to RTP session.
- *frame_data*: Pointer to JPEG frame data array to send.
- *frame_data_size*: Size of JPEG frame data to send.
- *timestamp*: RTP timestamp for current data.
- *ntp_msw*: Most significant word of network time.
- *ntp_lsw*: Least significant word of network time.
- *marker*: Marker bit for significant event such as frame boundary.

### Return Values

- **NX_SUCCESS** (0x00) Successful data sending.
- **NX_NOT_SUPPORTED** (0x4B) Unsupported input format or mode.
- **NX_NOT_SUCCESSFUL** (0x43) Invalid JPEG scan file.
- **NX_SIZE_ERROR** (0x09) Invalid JPEG scan file size.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Send JPEG frame in RTP/JPEG format on specific session. */

status = nx_rtp_sender_session_jpeg_send(&rtp_session_0, frame_data, frame_data_size,
                                         rtp_timestamp, ntp_msw, ntp_lsw, NX_TRUE);

/* If status is NX_SUCCESS the data was successfully sent. */
```

## nx_rtp_sender_session_h264_send

Send a complete H.264 format frame in RTP format on specific session

### Prototype

```C
UINT nx_rtp_sender_session_h264_send(
    NX_RTP_SESSION *session,
    UCHAR *frame_data,
    ULONG frame_data_size,
    ULONG timestamp,
    ULONG ntp_msw,
    ULONG ntp_lsw,
    UINT marker);
```

### Description

This service parses and separates the passed data into h264 frames or slices, and
processes each frame/slice from VCL format to NAL format, and finally calls RTP
session send function to send these frame/slice(s) in RTP packet.
    The function references RFC 6184 as the standard with below notes:
- A complete h264 data frame shall be inside the frame data buffer.
- Special frame(s) such as SEI, SPS and PPS can be inside the frame data buffer.
- Each H264 frame/slice inside the frame data buffer shall be in VCL (video coding layer) format.
- SDP shall indicate that non-interleaved mode is applied (i.e. packetization-mode=1), which supports the use of single NAL unit packet and FU-A packets.

**Reference the description of the function nx_rtp_sender_session_packet_send for more details about how timestamp, ntp_msw and ntp_lsw are increased for each function call.**

### Input Parameters

- *session*: Pointer to RTP session.
- *frame_data*: Pointer to H.264 frame data array to send.
- *frame_data_size*: Size of H.264 frame data to send.
- *timestamp*: RTP timestamp for current data.
- *ntp_msw*: Most significant word of network time.
- *ntp_lsw*: Least significant word of network time.
- *marker*: Marker bit for significant event such as frame boundary.

### Return Values

- **NX_SUCCESS** (0x00) Successful data sending.
- **NX_NOT_SUPPORTED** (0x4B) Unsupported input format or mode.
- **NX_NOT_SUCCESSFUL** (0x43) Invalid H.264 frame data.
- **NX_SIZE_ERROR** (0x09) Invalid H.264 frame size.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Send H.264 frame in RTP format on specific session. */

status = nx_rtp_sender_session_h264_send(&rtp_session_0, frame_data, frame_data_size,
                                         rtp_timestamp, ntp_msw, ntp_lsw, NX_TRUE);

/* If status is NX_SUCCESS the data was successfully sent. */
```

## nx_rtp_sender_session_aac_send

Send a complete AAC format frame in RTP format on specific session

### Prototype

```C
UINT nx_rtp_sender_session_aac_send(
    NX_RTP_SESSION *session,
    UCHAR *frame_data,
    ULONG frame_data_size,
    ULONG timestamp,
    ULONG ntp_msw,
    ULONG ntp_lsw,
    UINT marker);
```

### Description

This service parses and makes the passed data in RTP/AAC format, and then calls
RTP session send function to send these data in RTP packet, with AAC-HBR mode.
    The function references RFC 3640 as the standard with below notes:
- A complete aac frame data shall be inside frame data buffer
- SDP shall indicate that aac-hbr mode is applied, with SizeLength field to be 13 since 13-bit frame length is applied for computing the length in AU header.

**Reference the description of the function nx_rtp_sender_session_packet_send for more details about how timestamp, ntp_msw and ntp_lsw are increased for each function call.**

### Input Parameters

- *session*: Pointer to RTP session.
- *frame_data*: Pointer to AAC frame data array to send.
- *frame_data_size*: Size of AAC frame data to send.
- *timestamp*: RTP timestamp for current data.
- *ntp_msw*: Most significant word of network time.
- *ntp_lsw*: Least significant word of network time.
- *marker*: Marker bit for significant event such as frame boundary.

### Return Values

- **NX_SUCCESS** (0x00) Successful data sending.
- **NX_NOT_SUPPORTED** (0x4B) Unsupported input format or mode.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Send AAC frame in RTP format on specific session. */

status = nx_rtp_sender_session_aac_send(&rtp_session_0, frame_data, frame_data_size,
                                        rtp_timestamp, ntp_msw, ntp_lsw, NX_TRUE);

/* If status is NX_SUCCESS the data was successfully sent. */
```

## nx_rtp_sender_rtcp_receiver_report_callback_set

Set the callback function for receiver report parsed by RTCP

### Prototype

```C
UINT nx_rtp_sender_rtcp_receiver_report_callback_set(
    NX_RTP_SENDER *rtp_sender,
    UINT (*rtcp_rr_cb)(NX_RTP_SESSION *, NX_RTCP_RECEIVER_REPORT *));
```

### Description

This service sets a callback routine for RTCP RR packet receive notification. If a NULL
pointer is supplied the receive notify function is disabled. Note that this callback
function is invoked from the IP thread, Application shall not block the thread.

### Input Parameters

- *rtp_sender*: Pointer to RTP sender.
- *rtcp_rr_cb*: Pointer to application specified callback function. The parameters of this callback function are:
    - *session*: Pointer to the RTP session instance.
    - *rr*: Pointer to the receiver report structure.

### Return Values

- **NX_SUCCESS** (0x00) Successful receive report callback function set.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Set RTCP receiver report callback function for RTP sender instance "rtp_0". */

status = nx_rtp_sender_rtcp_receiver_report_callback_set(&rtp_0, test_rtcp_receiver_report_callback);

/* If status is NX_SUCCESS the receiver report callback function was successfully set. */
```

## nx_rtp_sender_rtcp_sdes_callback_set

Set the callback function for SDES packet parsed by RTCP

### Prototype

```C
UINT nx_rtp_sender_rtcp_sdes_callback_set(
    NX_RTP_SENDER *rtp_sender,
    UINT (*rtcp_sdes_cb)(NX_RTCP_SDES_INFO *));
```

### Description

This service sets a callback routine for RTCP SDES packet receive notification. If a NULL
pointer is supplied the receive notify function is disabled. Note that this callback
function is invoked from the IP thread, Application shall not block the thread.

### Input Parameters

- *rtp_sender*: Pointer to RTP sender.
- *rtcp_sdes_cb*: Pointer to application specified callback function. The parameter of this callback function is:
    - *sdes_info*: Pointer to the SDES information structure.

### Return Values

- **NX_SUCCESS** (0x00) Successful SDES packet callback function set.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Set RTCP SDES packet callback function for RTP sender instance "rtp_0". */

status = nx_rtp_sender_rtcp_sdes_callback_set(&rtp_0, test_rtcp_sdes_callback);

/* If status is NX_SUCCESS the SDES packet callback function was successfully set. */
```

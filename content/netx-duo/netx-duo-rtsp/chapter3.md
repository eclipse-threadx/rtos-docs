---
title: Chapter 3 - Description of NetX Duo RTSP Server Services
description: This chapter contains a description of all NetX Duo RTSP server services (listed below) in alphabetic order.  
---


This chapter contains a description of all NetX Duo RTSP server services (listed below) in alphabetic order.

> **Note:** In the **Return Values** section in the following API descriptions, values in **BOLD** are not affected by the **NX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.

## nx_rtsp_server_create

Create an RTSP server instance

### Prototype

```C
UINT nx_rtsp_server_create(
    NX_RTSP_SERVER *rtsp_server_ptr,
    CHAR *server_name,
    UINT server_name_length,
    NX_IP *ip_ptr,
    NX_PACKET_POOL *rtsp_packet_pool,
    VOID *stack_ptr,
    ULONG stack_size,
    UINT priority,
    UINT server_port,
    UINT (*disconnect_callback)(NX_RTSP_CLIENT *rtsp_client_ptr))
```

### Description

This service creates an RTSP server on the specified IP and port.

### Input Parameters

- *rtsp_server_ptr*: Pointer to RTSP server.
- *server_name*: Name of RTSP server.
- *server_name_length*: Length of RTSP server name.
- *ip_ptr*: Pointer to the created IP instance.
- *rtsp_packet_pool*: Pointer to RTSP packet pool.
- *stack_ptr*: RTSP server thread's stack pointer.
- *stack_size*: RTSP server thread's stack size.
- *priority*: Priority of RTSP server thread.
- *server_port*: RTSP server's listening port.
- *disconnect_callback*: Pointer to disconnect callback function. This function is called when an RTSP client disconnects from the server.

### Return Values

- **NX_SUCCESS** (0x00) Successful RTSP server creation.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Create the RTSP Server instance "rtsp_0". */

status = nx_rtsp_server_create(&rtsp_0, "RTSP Server",
    sizeof("RTSP Server") - 1, ip_ptr, pool_ptr,
    rtsp_server_stack, DEMO_RTSP_SERVER_STACK_SIZE,
    3, 554, rtsp_disconnect_callback);


/* If status is NX_SUCCESS an RTSP server instance was successfully created. */
```

## nx_rtsp_server_delete

Delete an RTSP server instance

### Prototype

```C
UINT nx_rtsp_server_delete(NX_RTSP_SERVER *rtsp_server_ptr);
```

### Description

This service deletes a previously created RTSP server.

### Input Parameters

- *rtsp_server_ptr*: Pointer to RTSP server.

### Return Values

- **NX_SUCCESS** (0x00) Successful RTSP server deletion.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Delete the RTSP server instance "rtsp_0". */

status = nx_rtsp_server_delete(&rtsp_0);

/* If status is NX_SUCCESS the RTSP server instance was successfully  
    deleted. */
```

## nx_rtsp_server_describe_callback_set

Set the callback function for DESCRIBE request

### Prototype

```C
UINT nx_rtsp_server_describe_callback_set(
    NX_RTSP_SERVER *rtsp_server_ptr,
    UINT (*callback)(
        NX_RTSP_CLIENT *rtsp_client_ptr,
        UCHAR *uri,
        UINT uri_length));
```

### Description

This service sets callback function for DESCRIBE request. This callback function is called when an RTSP client sends a DESCRIBE request to the server. [nx_rtsp_server_sdp_set](#nx_rtsp_server_sdp_set) should be called in the callback function to set the SDP information for the DESCRIBE response.

### Input Parameters

- *rtsp_server_ptr*: Pointer to RTSP server.
- *callback*: Pointer to DESCRIBE request callback function. If the URI is valid and the SDP string is set successfully, this function should return **NX_SUCCESS**. Otherwise, this function should return another status. In addition, [nx_rtsp_server_error_response_send](#nx_rtsp_server_error_response_send) can be called to send the specified error status code. The parameters of this callback function are:
    - *rtsp_client_ptr*: Pointer to RTSP client.
    - *uri*: Pointer to the URI of DESCRIBE request.
    - *uri_length*: Length of the URI of DESCRIBE request.

### Return Values

- **NX_SUCCESS** (0x00) Successful DESCRIBE request callback function set.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Set DESCRIBE request callback function for RTSP server instance "rtsp_0". */

status = nx_rtsp_server_describe_callback_set(&rtsp_0, rtsp_describe_callback);

/* If status is NX_SUCCESS the DESCRIBE request callback function was successfully set. */
```

## nx_rtsp_server_error_response_send

Send the error response packet

### Prototype

```C
UINT nx_rtsp_server_error_response_send(
    NX_RTSP_CLIENT *rtsp_client_ptr,
    UINT status_code);
```

### Description

This service sends the error response packet with specified status code.

### Input Parameters

- *rtsp_client_ptr*: Pointer to RTSP client.
- *status_code*: Status code of the error response.

### Return Values

- **NX_SUCCESS** (0x00) Successful error response sending.
- **NX_RTSP_SERVER_NO_PACKET** (0x7003) No response packet available.
- **NX_RTSP_SERVER_INVALID_PARAMETER** (0x7007) Invalid status code.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Send error response to the connected RTSP client. */

status = nx_rtsp_server_error_response_send(rtsp_client_ptr, NX_RTSP_STATUS_CODE_UNSUPPORTED_MEDIA_TYPE);

/* If status is NX_SUCCESS the error response was successfully sent. */
```

## nx_rtsp_server_keepalive_update

Update the keep-alive timer

### Prototype

```C
UINT nx_rtsp_server_keepalive_update(
    NX_RTSP_CLIENT *rtsp_client_ptr);
```

### Description

This service updates the keep-alive timer. If RTP is used for media transport, the keep-alive timer is updated when an RTCP packet is received.

### Input Parameters

- *rtsp_client_ptr*: Pointer to RTSP client.

### Return Values

- **NX_SUCCESS** (0x00) Successful keep-alive.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Update keep-alive timer for the connected RTSP client. */

status = nx_rtsp_server_keepalive_update(rtsp_client_ptr);

/* If status is NX_SUCCESS the keep-alive timer was successfully updated. */
```

## nx_rtsp_server_pause_callback_set

Set the callback function for PAUSE request

### Prototype

```C
UINT nx_rtsp_server_pause_callback_set(
    NX_RTSP_SERVER *rtsp_server_ptr,
    UINT (*callback)(
        NX_RTSP_CLIENT *rtsp_client_ptr,
        UCHAR *uri,
        UINT uri_length,
        UCHAR *range_ptr,
        UINT range_length));
```

### Description

This service sets callback function for PAUSE request. This callback function is called when an RTSP client sends a PAUSE request to the server. [nx_rtsp_server_range_npt_set](#nx_rtsp_server_range_npt_set) can be called in the callback function to set the NPT range for the PAUSE response.

### Input Parameters

- *rtsp_server_ptr*: Pointer to RTSP server.
- *callback*: Pointer to PAUSE request callback function. If the URI is valid and the stream delivery can be paused, this function should return **NX_SUCCESS**. Otherwise, this function should return another status. In addition, [nx_rtsp_server_error_response_send](#nx_rtsp_server_error_response_send) can be called to send the specified error status code. The parameters of this callback function are:
    - *rtsp_client_ptr*: Pointer to RTSP client.
    - *uri*: Pointer to the URI of PAUSE request.
    - *uri_length*: Length of the URI of PAUSE request.
    - *range_ptr*: Pointer to the Range field of PAUSE request.
    - *range_length*: Length of the Range field of PAUSE request.

### Return Values

- **NX_SUCCESS** (0x00) Successful PAUSE request callback function set.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Set PAUSE request callback function for RTSP server instance "rtsp_0". */

status = nx_rtsp_server_pause_callback_set(&rtsp_0, rtsp_pause_callback);

/* If status is NX_SUCCESS the PAUSE request callback function was successfully set. */
```

## nx_rtsp_server_play_callback_set

Set the callback function for PLAY request

### Prototype

```C
UINT nx_rtsp_server_play_callback_set(
    NX_RTSP_SERVER *rtsp_server_ptr,
    UINT (*callback)(
        NX_RTSP_CLIENT *rtsp_client_ptr,
        UCHAR *uri,
        UINT uri_length,
        UCHAR *range_ptr,
        UINT range_length));
```

### Description

This service sets callback function for PLAY request. This callback function is called when an RTSP client sends a PLAY request to the server. [nx_rtsp_server_range_npt_set](#nx_rtsp_server_range_npt_set) can be called in the callback function to set the NPT range for the PLAY response. [nx_rtsp_server_rtp_info_set](#nx_rtsp_server_rtp_info_set) must be called in the callback function to set the RTP-Info header for the PLAY response.

### Input Parameters

- *rtsp_server_ptr*: Pointer to RTSP server.
- *callback*: Pointer to PLAY request callback function. If the URI is valid and the stream delivery can be started, this function should return **NX_SUCCESS**. Otherwise, this function should return another status. In addition, [nx_rtsp_server_error_response_send](#nx_rtsp_server_error_response_send) can be called to send the specified error status code. The parameters of this callback function are:
    - *rtsp_client_ptr*: Pointer to RTSP client.
    - *uri*: Pointer to the URI of PLAY request.
    - *uri_length*: Length of the URI of PLAY request.
    - *range_ptr*: Pointer to the Range field of PLAY request.
    - *range_length*: Length of the Range field of PLAY request.

### Return Values

- **NX_SUCCESS** (0x00) Successful PLAY request callback function set.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Set PLAY request callback function for RTSP server instance "rtsp_0". */

status = nx_rtsp_server_play_callback_set(&rtsp_0, rtsp_play_callback);

/* If status is NX_SUCCESS the PLAY request callback function was successfully set. */
```

## nx_rtsp_server_range_npt_set

Set the NPT start and end time

### Prototype

```C
UINT nx_rtsp_server_range_npt_set(
    NX_RTSP_CLIENT *rtsp_client_ptr
    UINT npt_start,
    UINT npt_end);
```

### Description

This service sets the NPT start and end time in Range field. This function can only be called in PLAY and PAUSE request callback functions.

### Input Parameters

- *rtsp_client_ptr*: Pointer to RTSP client.
- *npt_start*: The NPT start time in milliseconds.
- *npt_end*: The NPT end time in milliseconds.

### Return Values

- **NX_SUCCESS** (0x00) Successful NPT start and end time set.
- **NX_RTSP_SERVER_INVALID_REQUEST** (0x7006) Called by invalid request callback.
- **NX_RTSP_SERVER_INVALID_PARAMETER** (0x7007) Invalid parameter that npt_end is less than npt_start.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Set the NPT start and end time. */

status = nx_rtsp_server_range_npt_set(rtsp_client_ptr, 0, 30000);

/* If status is NX_SUCCESS the NPT time was successfully set. */
```

## nx_rtsp_server_rtp_info_set

Set the RTP-Info to the response packet

### Prototype

```C
UINT nx_rtsp_server_rtp_info_set(
    NX_RTSP_CLIENT *rtsp_client_ptr
    UCHAR *track_id,
    UINT track_id_len,
    UINT rtp_seq,
    UINT rtp_time);
```

### Description

This service sets the RTP-Info to the response packet. This function can only be called in PLAY callback function.

### Input Parameters

- *rtsp_client_ptr*: Pointer to RTSP client.
- *track_id*: Pointer to track ID, which is the ID of the resource.
- *track_id_len*: The length of track ID.
- *rtp_seq*: The RTP sequence number.
- *rtp_time*: The RTP timestamp.

### Return Values

- **NX_SUCCESS** (0x00) Successful RTP-Info set.
- **NX_RTSP_SERVER_NO_PACKET** (0x7003) No response packet available.
- **NX_RTSP_SERVER_INVALID_REQUEST** (0x7006) Called by invalid request callback.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Set the RTP-Info for video. */

status = nx_rtsp_server_rtp_info_set(rtsp_client_ptr, DEMO_RTSP_VIDEO_FILE_NAME, sizeof(DEMO_RTSP_VIDEO_FILE_NAME) - 1, video_seq, video_rtptime);

/* If status is NX_SUCCESS the RTP-Info was successfully set. */
```

## nx_rtsp_server_sdp_set

Set the SDP string to the response packet

### Prototype

```C
UINT nx_rtsp_server_sdp_set(
    NX_RTSP_CLIENT *rtsp_client_ptr
    UCHAR *sdp_string,
    UINT sdp_length);
```

### Description

This service sets the SDP string to the response packet. This function can only be called in DESCRIBE callback function.

### Input Parameters

- *rtsp_client_ptr*: Pointer to RTSP client.
- *sdp_string*: Pointer to SDP string.
- *sdp_length*: The length of SDP string.

### Return Values

- **NX_SUCCESS** (0x00) Successful SDP string set.
- **NX_RTSP_SERVER_NO_PACKET** (0x7003) No response packet available.
- **NX_RTSP_SERVER_INVALID_REQUEST** (0x7006) Called by invalid request callback.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Set the SDP string. */

status = nx_rtsp_server_sdp_set(rtsp_client_ptr, sdp, strlen(sdp));

/* If status is NX_SUCCESS the SDP was successfully set. */
```

## nx_rtsp_server_setup_callback_set

Set the callback function for SETUP request

### Prototype

```C
UINT nx_rtsp_server_setup_callback_set(
    NX_RTSP_SERVER *rtsp_server_ptr,
    UINT (*callback)(
        NX_RTSP_CLIENT *rtsp_client_ptr,
        UCHAR *uri,
        UINT uri_length,
        NX_RTSP_TRANSPORT *transport_ptr));
```

### Description

This service sets callback function for SETUP request. This callback function is called when an RTSP client sends a SETUP request to the server. The client transport information is passed to the callback function. And the callback function must set the server transport information back.

### Input Parameters

- *rtsp_server_ptr*: Pointer to RTSP server.
- *callback*: Pointer to SETUP request callback function. If the URI is valid and the transport information can be resolved and set successfully, this function should return **NX_SUCCESS**. Otherwise, this function should return another status. In addition, [nx_rtsp_server_error_response_send](#nx_rtsp_server_error_response_send) can be called to send the specified error status code. The parameters of this callback function are:
  - *rtsp_client_ptr*: Pointer to RTSP client.
  - *uri*: Pointer to URI of SETUP request.
  - *uri_length*: The length of URI of SETUP request.
  - *transport_ptr*: Pointer to transport information. This parameter is an input and output parameter. The client transport information is passed through this parameter, and the server transport information must be set back to this parameter. The client transport information includes the transport protocol (UDP/TCP), transport mode (unicast/multicast), client IP address, and the client RTP/RTCP port pair. The server transport information includes the RTP SSRC, server IP address, and the server RTP/RTCP port pair. If using multicast mode with server choosing the multicast address, the client multicast address, ports, and TTL must also be set back to this parameter.

### Return Values

- **NX_SUCCESS** (0x00) Successful SETUP request callback function set.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Set SETUP request callback function for RTSP server instance "rtsp_0". */

status = nx_rtsp_server_setup_callback_set(&rtsp_0, rtsp_setup_callback);;

/* If status is NX_SUCCESS the SETUP request callback function was successfully set. */
```

## nx_rtsp_server_set_parameter_callback_set

Set the callback function for SET_PARAMETER request

### Prototype

```C
UINT nx_rtsp_server_set_parameter_callback_set(
    NX_RTSP_SERVER *rtsp_server_ptr,
    UINT (*callback)(
        NX_RTSP_CLIENT *rtsp_client_ptr,
        UCHAR *uri,
        UINT uri_length,
        UCHAR *parameter_ptr,
        ULONG parameter_length));
```

### Description

This service sets callback function for SET_PARAMETER request. This callback function is called when an RTSP client sends a SET_PARAMETER request to the server. The parameters which the client requests to update are passed to the callback function.

### Input Parameters

- *rtsp_server_ptr*: Pointer to RTSP server.
- *callback*: Pointer to SET_PARAMETER request callback function. If the URI is valid and the parameters can be updated successfully, this function should return **NX_SUCCESS**. Otherwise, this function should return another status. In addition, [nx_rtsp_server_error_response_send](#nx_rtsp_server_error_response_send) can be called to send the specified error status code. The parameters of this callback function are:
  - *rtsp_client_ptr*: Pointer to RTSP client.
  - *uri*: Pointer to URI of SET_PARAMETER request.
  - *uri_length*: The length of URI of SET_PARAMETER request.
  - *parameter_ptr*: Pointer to parameter of SET_PARAMETER request.
  - *parameter_length*: The length of parameter of SET_PARAMETER request.

### Return Values

- **NX_SUCCESS** (0x00) Successful SET_PARAMETER request callback function set.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Set SET_PARAMETER request callback function for RTSP server instance "rtsp_0". */

status = nx_rtsp_server_set_parameter_callback_set(&rtsp_0, rtsp_set_parameter_callback);

/* If status is NX_SUCCESS the SET_PARAMETER request callback function was successfully set. */
```

## nx_rtsp_server_start

Start the RTSP server

### Prototype

```C
UINT nx_rtsp_server_start(
    NX_RTSP_SERVER *rtsp_server_ptr);
```

### Description

This service starts the RTSP server. This function must be called after the RTSP server is created.

### Input Parameters

- *rtsp_server_ptr*: Pointer to RTSP server.

### Return Values

- **NX_SUCCESS** (0x00) Successful RTSP server start.
- **NX_RTSP_SERVER_ALREADY_STARTED** (0x7003) RTSP server already started.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Start the RTSP server instance "rtsp_0". */

status = nx_rtsp_server_start(&rtsp_0);

/* If status is NX_SUCCESS the RTSP server was successfully started. */
```

## nx_rtsp_server_stop

Stop the RTSP server

### Prototype

```C
UINT nx_rtsp_server_stop(
    NX_RTSP_SERVER *rtsp_server_ptr);
```

### Description

This service stops a previously started RTSP server. 

### Input Parameters

- *rtsp_server_ptr*: Pointer to RTSP server.

### Return Values

- **NX_SUCCESS** (0x00) Successful RTSP Server stop.
- **NX_RTSP_SERVER_NOT_STARTED** (0x7003) RTSP server not started.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Stop the RTSP server instance "rtsp_0". */

status = nx_rtsp_server_stop(&rtsp_0);

/* If status is NX_SUCCESS the RTSP server was successfully stopped. */
```

## nx_rtsp_server_teardown_callback_set

Set the callback function for TEARDOWN request

### Prototype

```C
UINT nx_rtsp_server_teardown_callback_set(
    NX_RTSP_SERVER *rtsp_server_ptr,
    UINT (*callback)(
        NX_RTSP_CLIENT *rtsp_client_ptr,
        UCHAR *uri,
        UINT uri_length));
```

### Description

This service sets callback function for TEARDOWN request. This callback function is called when an RTSP client sends a TEARDOWN request to the server. The transport sessions should be closed in this callback function.

### Input Parameters

- *rtsp_server_ptr*: Pointer to RTSP server.
- *callback*: Pointer to TEARDOWN request callback function. If the URI is valid and the transport session can be closed successfully, this function should return **NX_SUCCESS**. Otherwise, this function should return another status. In addition, [nx_rtsp_server_error_response_send](#nx_rtsp_server_error_response_send) can be called to send the specified error status code. The parameters of this callback function are:
  - *rtsp_client_ptr*: Pointer to RTSP client.
  - *uri*: Pointer to URI of TEARDOWN request.
  - *uri_length*: The length of URI of TEARDOWN request.

### Return Values

- **NX_SUCCESS** (0x00) Successful TEARDOWN request callback function set.
- NX_PTR_ERROR (0x07) Invalid pointer input.

### Allowed From

Threads

### Example

```C
/* Set TEARDOWN request callback function for RTSP server instance "rtsp_0". */

status = nx_rtsp_server_teardown_callback_set(&rtsp_0, rtsp_teardown_callback);;

/* If status is NX_SUCCESS the TEARDOWN request callback function was successfully set. */
```
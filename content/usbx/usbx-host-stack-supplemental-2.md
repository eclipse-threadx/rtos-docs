---
title: USBX Host Classes API
description: This chapter covers all the exposed APIs of the USBX host classes. 
---

This chapter covers all the exposed APIs of the USBX host classes. The following APIs for each class are described in detail.

- Printer class
- Audio class
- Asix class
- Pima/PTP class
- Prolific class
- Generic Serial class

## ux_host_class_printer_read

Read from the printer interface.

### Prototype

```C
UINT ux_host_class_printer_read(
    UX_HOST_CLASS_PRINTER *printer,
    UCHAR *data_pointer,
    ULONG requested_length,
    ULONG *actual_length)
```

### Description

This function reads from the printer interface. The call is blocking and only returns when there is either an error or when the transfer is complete. A read is allowed only on bi-directional printers.

### Parameters

- **printer**: Pointer to the printer class instance.
- **data_pointer**: Pointer to the buffer address of the data payload.
- **requested_length**: Length to be received.
- **actual_length**: Length actually received.

### Return Value

- **UX_SUCCESS**:  (0x00) The data transfer was completed.
- **UX_FUNCTION_NOT_SUPPORTED**: (0x54) Function not supported because the printer is not bi-directional.
- **UX_TRANSFER_TIMEOUT**: (0x5c) Transfer timeout, reading incomplete.

### Example

```C
UINT status;

/* The following example illustrates this service. */
status = ux_host_class_printer_read(printer, data_pointer, requested_length, &actual_length);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_printer_write

Write to the printer interface.

### Prototype

```C
UINT ux_host_class_printer_write(
    UX_HOST_CLASS_PRINTER *printer,
    UCHAR *data_pointer,  
    ULONG requested_length,
    ULONG *actual_length)
```

### Description

This function writes to the printer interface. The call is blocking and only returns when there is either an error or when the transfer is complete.

### Parameters

- **printer**: Pointer to the printer class instance.
- **data_pointer**: Pointer to the buffer address of the data payload.
- **requested_length**: Length to be sent.
- **actual_length**: Length actually sent.

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed.
- **UX_TRANSFER_TIMEOUT**: (0x5c) Transfer timeout, writing incomplete.

### Example

```C
UINT status;

/* The following example illustrates this service. */
status = ux_host_class_printer_write(printer, data_pointer, requested_length, &actual_length);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_printer_soft_reset

Perform a soft reset to the printer.

### Prototype

```C
UINT ux_host_class_printer_soft_reset(UX_HOST_CLASS_PRINTER *printer)
```

### Description

This function performs a soft reset to the printer.

### Input Parameter

- **printer**: Pointer to the printer class instance.

### Return Value

- **UX_SUCCESS**: (0x00)The reset was completed.
- **UX_TRANSFER_TIMEOUT**: (0x5c) Transfer timeout, reset not completed.

### Example

```C
UINT status;

/* The following example illustrates this service. */
status = ux_host_class_printer_soft_reset(printer);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_printer_status_get

Get the printer status

### Prototype

```C
UINT ux_host_class_printer_status_get( 
    UX_HOST_CLASS_PRINTER *printer,
    ULONG *printer_status)
```

### Description

This function obtains the printer status. The printer status is similar to the LPT status (1284 standard).

### Parameters

- **printer**: Pointer to the printer class instance.
- **printer_status**: Address of the status to be returned.

### Return Value

- **UX_SUCCESS** (0x00): The reset was completed.
- **UX_MEMORY_INSUFFICIENT**: (0x12) Not enough memory to perform the operation.
- **UX_TRANSFER_TIMEOUT**: (0x5c) Transfer timeout, reset not completed

### Example

```C
UINT status;

/* The following example illustrates this service. */
status = ux_host_class_printer_status_get(printer, printer_status);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_printer_device_id_get

Get the printer device id.

### Prototype

```C
UINT ux_host_class_printer_device_id_get( 
    UX_HOST_CLASS_PRINTER *printer,
    UCHAR *descriptor_buffer, 
    ULONG length)
```

### Description

This function obtains the printer IEEE 1284 device ID string (including length in the first two bytes in big endian format).

### Parameters

- **printer**: Pointer to the printer class instance.
- **descriptor_buffer**: Pointer to a buffer to fill IEEE 1284 device ID string (including length in the first two bytes in BE format) 
- **length**: Length of buffer in bytes.

### Return Value

- **UX_SUCCESS** (0x00): The operation was successful.
- **UX_MEMORY_INSUFFICIENT**: (0x12) Not enough memory to perform the operation.
- **UX_TRANSFER_TIMEOUT**: (0x5c) Transfer timeout, request not completed
- **UX_TRANSFER_NOT_READY**: (0x25) The device was in an invalid state – must be ATTACHED,ADDRESSED, or CONFIGURED.
- **UX_TRANSFER_STALL**: (0x21) Transfer stalled.
- **TX_WAIT_ABORTED** (0x1A) Suspension was aborted by another thread, timer, or ISR.
- **TX_SEMAPHORE_ERROR** (0x0C) Invalid counting semaphore pointer.
- **TX_WAIT_ERROR** (0x04) A wait option other than TX_NO_WAIT was specified on a call from a non-thread.

### Example

```C
UINT status;

/* The following example illustrates this service. */
status = ux_host_class_printer_device_id_get(printer, descriptor_buffer, length);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_audio_read

Read from the audio interface.

### Prototype

```C
UINT ux_host_class_audio_read(
    UX_HOST_CLASS_AUDIO *audio,
    UX_HOST_CLASS_AUDIO_TRANSFER_REQUEST
    *audio_transfer_request)
```

### Description

This function reads from the audio interface. The call is non-blocking. The application must ensure that the appropriate alternate setting has been selected for the audio streaming interface.

### Parameters

- **audio**: Pointer to the audio class instance.
- **audio_transfer_request**: Pointer to the audio transfer structure.

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed
- **UX_FUNCTION_NOT_SUPPORTED**" (0x54) Function not supported

### Example

```C
/* The following example reads from the audio interface. */

audio_transfer_request.ux_host_class_audio_transfer_request_completion_function = tx_audio_transfer_completion_function;
audio_transfer_request.ux_host_class_audio_transfer_request_class_instance = audio;
audio_transfer_request.ux_host_class_audio_transfer_request_next_audio_audio_transfer_request = UX_NULL;
audio_transfer_request.ux_host_class_audio_transfer_request_data_pointer = audio_buffer;
audio_transfer_request.ux_host_class_audio_transfer_request_requested_length = requested_length;
audio_transfer_request.ux_host_class_audio_transfer_request_packet_length = AUDIO_FRAME_LENGTH;

status = ux_host_class_audio_read(audio, audio_transfer_request);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_audio_write

Write to the audio interface.

### Prototype

```C
UINT ux_host_class_audio_write(
    UX_HOST_CLASS_AUDIO *audio,
    UX_HOST_CLASS_AUDIO_TRANSFER_REQUEST *audio_transfer_request)
```

### Description

This function writes to the audio interface. The call is non-blocking. The application must ensure that the appropriate alternate setting has been selected for the audio streaming interface.

### Parameters

- **audio**: Pointer to the audio class instance
- **audio_transfer_request**: Pointer to the audio transfer structure

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed.
- **UX_FUNCTION_NOT_SUPPORTED**: (0x54) Function not supported.
- **ux_host_CLASS_AUDIO_WRONG_INTERFACE**: (0x81) Interface incorrect.

### Example

```C
UINT status;

/* The following example writes to the audio interface */

audio_transfer_request.ux_host_class_audio_transfer_request_completion_function = tx_audio_transfer_completion_function;
audio_transfer_request.ux_host_class_audio_transfer_request_class_instance = audio;
audio_transfer_request.ux_host_class_audio_transfer_request_next_audio_audio_transfer_request = UX_NULL;
audio_transfer_request.ux_host_class_audio_transfer_request_data_pointer = audio_buffer;
audio_transfer_request.ux_host_class_audio_transfer_request_requested_length = requested_length;
audio_transfer_request.ux_host_class_audio_transfer_request_packet_length = AUDIO_FRAME_LENGTH;
status = ux_host_class_audio_write(audio, audio_transfer_request);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_audio_control_get

Get a specific control from the audio control interface.

### Prototype

```C
UINT ux_host_class_audio_control_get(
    UX_HOST_CLASS_AUDIO *audio,
    UX_HOST_CLASS_AUDIO_CONTROL *audio_control)
```

### Description

This function reads a specific control from the audio control interface.

### Parameters

- **audio**: Pointer to the audio class instance
- **audio_control**: Pointer to the audio control structure

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed
- **UX_FUNCTION_NOT_SUPPORTED**: (0x54) Function not supported
- **UX_HOST_CLASS_AUDIO_WRONG_INTERFACE**: (0x81) Interface incorrect

### Example

```C
UINT status;

/* The following example reads the volume control from a stereo USB speaker. */

UX_HOST_CLASS_AUDIO_CONTROL audio_control;

audio_control.ux_host_class_audio_control_channel = 1;
audio_control.ux_host_class_audio_control = UX_HOST_CLASS_AUDIO_VOLUME_CONTROL;

status = ux_host_class_audio_control_get(audio, &audio_control);

/* If status equals UX_SUCCESS, the operation was successful. */

audio_control.ux_host_class_audio_control_channel = 2;
audio_control.ux_host_class_audio_control = UX_HOST_CLASS_AUDIO_VOLUME_CONTROL;

status = ux_host_class_audio_control_get(audio, &audio_control);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_audio_control_value_set

Set a specific control to the audio control interface.

### Prototype

```C
UINT ux_host_class_audio_control_value_set(
    UX_HOST_CLASS_AUDIO *audio,
    UX_HOST_CLASS_AUDIO_CONTROL *audio_control)
```

**Description **

This function sets a specific control to the audio control interface.

### Parameters

- **audio**: Pointer to the audio class instance
- **audio_control**: Pointer to the audio control structure

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed
- **UX_FUNCTION_NOT_SUPPORTED**: (0x54) Function not supported
- **UX_HOST_CLASS_AUDIO_WRONG_INTERFACE**: (0x81) Interface incorrect

### Example

```C
/* The following example sets the volume control of a stereo USB speaker. */

UX_HOST_CLASS_AUDIO_CONTROL audio_control;

UINT status;

audio_control.ux_host_class_audio_control_channel = 1;
audio_control.ux_host_class_audio_control = UX_HOST_CLASS_AUDIO_VOLUME_CONTROL;
audio_control.ux_host_class_audio_control_cur = 0xf000;

status = ux_host_class_audio_control_value_set(audio, &audio_control);
/* If status equals UX_SUCCESS, the operation was successful. */

current_volume = audio_control.audio_control_cur;
audio_control.ux_host_class_audio_control_channel = 2;
audio_control.ux_host_class_audio_control = UX_HOST_CLASS_AUDIO_VOLUME_CONTROL;
audio_control.ux_host_class_audio_control_cur = 0xf000;

status = ux_host_class_audio_control_value_set(audio, &audio_control);
/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_audio_streaming_sampling_set

Set an alternate setting interface of the audio streaming interface.

### Prototype

```C
UINT ux_host_class_audio_streaming_sampling_set
    (UX_HOST_CLASS_AUDIO *audio,
    UX_HOST_CLASS_AUDIO_SAMPLING *audio_sampling)
```

### Description

This function sets the appropriate alternate setting interface of the audio streaming interface according to a specific sampling structure.

### Parameters

- **audio**: Pointer to the audio class instance.
- **audio_sampling**: Pointer to the audio sampling structure.

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed
- **UX_FUNCTION_NOT_SUPPORTED**: (0x54) Function not supported
- **UX_HOST_CLASS_AUDIO_WRONG_INTERFACE**: (0x81) Interface incorrect
- **UX_NO_ALTERNATE_SETTING**: (0x5e) No alternate setting for the sampling values

### Example

```C
/* The following example sets the alternate setting interface of a stereo USB speaker. */

UX_HOST_CLASS_AUDIO_SAMPLING audio_sampling;

UINT status;

sampling.ux_host_class_audio_sampling_channels = 2;
sampling.ux_host_class_audio_sampling_frequency = AUDIO_FREQUENCY;
sampling. ux_host_class_audio_sampling_resolution = 16;

status = ux_host_class_audio_streaming_sampling_set(audio, &sampling);
/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_audio_streaming_sampling_get

Get possible sampling settings of audio streaming interface.

### Prototype

```C
UINT ux_host_class_audio_streaming_sampling_get(
    UX_HOST_CLASS_AUDIO *audio,
    UX_HOST_CLASS_AUDIO_SAMPLING_CHARACTERISTICS *audio_sampling)
```

### Description

This function gets, one by one, all the possible sampling settings available in each of the alternate settings of the audio streaming interface. The first time the function is used, all the fields in the calling structure pointer must be reset. The function will return a specific set of streaming values upon return unless the end of the alternate settings has been reached. When this function is reused, the previous sampling values will be used to find the next sampling values.

### Parameters

- **audio**: Pointer to the audio class instance.
- **audio_sampling**: Pointer to the audio sampling structure.

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed
- **UX_FUNCTION_NOT_SUPPORTED**: (0x54) Function not supported
- **UX_HOST_CLASS_AUDIO_WRONG_INTERFACE**: (0x81) Interface incorrect
- **UX_NO_ALTERNATE_SETTING**: (0x5e) No alternate setting for the sampling values

### Example

```C
/* The following example gets the sampling values for the first alternate setting interface of a stereo USB speaker. */

UX_HOST_CLASS_AUDIO_SAMPLING_CHARACTERISTICS audio_sampling;

UINT status;

sampling.ux_host_class_audio_sampling_channels=0;
sampling.ux_host_class_audio_sampling_frequency_low=0;
sampling.ux_host_class_audio_sampling_frequency_high=0;
sampling.ux_host_class_audio_sampling_resolution=0;

status = ux_host_class_audio_streaming_sampling_get(audio, &sampling);

/* If status equals UX_SUCCESS, the operation was successful and information could be displayed as follows:

printf("Number of channels %d, Resolution %d bits, frequency range %d-%d\n",
    sampling.audio_channels, sampling.audio_resolution,
    sampling.audio_frequency_low, sampling.audio_frequency_high);

*/
```

## ux_host_class_asix_read

Read from the asix interface.

### Prototype

```C
UINT ux_host_class_asix_read(
    UX_HOST_CLASS_ASIX *asix,
    UCHAR *data_pointer,
    ULONG requested_length,
    ULONG *actual_length)
```

### Description

This function reads from the asix interface. The call is blocking and only returns when there is either an error or when the transfer is complete.

### Parameters

- **asix**: Pointer to the asix class instance.
- **data_pointer**: Pointer to the buffer address of the data payload.
- **requested_length**: Length to be received.
- **actual_length**: Length actually received.

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed.
- **UX_TRANSFER_TIMEOUT**: (0x5c) Transfer timeout, reading incomplete.

### Example

```C
UINT status;

/* The following example illustrates this service. */

status = ux_host_class_asix_read(asix, data_pointer, requested_length, &actual_length);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_asix_write

Write to the asix interface.

### Prototype

```C
UINT ux_host_class_asix_write(
    VOID *asix_class,
    NX_PACKET *packet)
```

### Description

This function writes to the asix interface. The call is non blocking.

### Parameters

- **asix**: Pointer to the asix class instance.
- **packet**: NetX Duo data packet

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed.
- **UX_ERROR**: (0xFF) Transfer could not be requested.

### Example

```C
UINT status;

/* The following example illustrates this service. */

status = ux_host_class_asix_write(asix, packet);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_pima_session_open

Open a session between Initiator and Responder.

### Prototype

```C
UINT ux_host_class_pima_session_open(
    UX_HOST_CLASS_PIMA *pima,
    UX_HOST_CLASS_PIMA_SESSION *pima_session)
```

### Description

This function opens a session between a PIMA Initiator and a PIMA Responder. Once a session is successfully opened, most PIMA commands can be executed.

### Parameters

- **pima**: Pointer to the pima class instance.
- **pima_session**: Pointer to PIMA session.

### Return Value

- **UX_SUCCESS**: (0x00) Session successfully opened
- **UX_HOST_CLASS_PIMA_RC_SESSION_ALREADY_OPENED**: (0x201E) Session already opened

### Example

```C
/* Open a pima session. */

status = ux_host_class_pima_session_open(pima, pima_session);

if (status != UX_SUCCESS)
    return(UX_PICTBRIDGE_ERROR_SESSION_NOT_OPEN);
```

## ux_host_class_pima_session_close

Close a session between Initiator and Responder.

### Prototype

```C
UINT ux_host_class_pima_session_close(
    UX_HOST_CLASS_PIMA *pima,
    UX_HOST_CLASS_PIMA_SESSION *pima_session)
```

### Description

This function closes a session that was previously opened between a PIMA Initiator and a PIMA Responder. Once a session is closed, most PIMA commands can no longer be executed.

### Parameters

- **pima**: Pointer to the pima class instance.
- **pima_session**: Pointer to PIMA session.

### Return Value

- **UX_SUCCESS**: (0x00) The session was closed.
- **UX_HOST_CLASS_PIMA_RC_SESSION_NOT_OPEN**: (0x2003) Session not opened.

### Example

```C
/* Close the pima session. */

status = ux_host_class_pima_session_close(pima, pima_session);
```

## ux_host_class_pima_storage_ids_get

Obtain the storage ID array from Responder.

### Prototype

```C
UINT ux_host_class_pima_storage_ids_get(
    UX_HOST_CLASS_PIMA *pima,
    UX_HOST_CLASS_PIMA_SESSION *pima_session,
    ULONG *storage_ids_array,
    ULONG storage_id_length)
```

### Description

This function obtains the storage ID array from the responder.

### Parameters

- **pima**: Pointer to the pima class instance.
- **pima_session**: Pointer to PIMA session
- **storage_ids_array**: Array where storage IDs will be returned
- **storage_id_length**: Length of the storage array

### Return Value

- **UX_SUCCESS**: (0x00) The storage ID array has been populated
- **UX_HOST_CLASS_PIMA_RC_SESSION_NOT_OPEN**: (0x2003) Session not opened
- **UX_MEMORY_INSUFFICIENT**: (0x12) Not enough memory to create PIMA command.

### Example

```C
/* Get the number of storage IDs. */
status = ux_host_class_pima_storage_ids_get(pima, pima_session,
    pictbridge ->ux_pictbridge_storage_ids, 64);

if (status != UX_SUCCESS)
{
    /* Close the pima session. */
    status = ux_host_class_pima_session_close(pima, pima_session);

    return(UX_PICTBRIDGE_ERROR_STORE_NOT_AVAILABLE);
}
```

## ux_host_class_pima_storage_info_get

Obtain the storage information from Responder.

### Prototype

```C
UINT ux_host_class_pima_storage_info_get(
    UX_HOST_CLASS_PIMA *pima,
    UX_HOST_CLASS_PIMA_SESSION *pima_session,
    ULONG storage_id,
    UX_HOST_CLASS_PIMA_STORAGE *storage)
```

### Description

This function obtains the storage information for a storage container of value *storage_id*.

### Parameters

- **pima**: Pointer to the pima class instance.
- **pima_session**: Pointer to PIMA session
- **storage_id**: ID of the storage container
- **storage**: Pointer to storage information container

### Return Value

- **UX_SUCCESS**: (0x00) The storage information was retrieved
- **UX_HOST_CLASS_PIMA_RC_SESSION_NOT_OPEN**: (0x2003) Session not opened
- **UX_MEMORY_INSUFFICIENT** (0x12) Not enough memory to create PIMA command.

### Example

```C
/* Get the first storage ID info container. */
status = ux_host_class_pima_storage_info_get(pima, pima_session,
    pictbridge ->ux_pictbridge_storage_ids[0],
    (UX_HOST_CLASS_PIMA_STORAGE *)pictbridge ->ux_pictbridge_storage);

if (status != UX_SUCCESS)
{
    /* Close the pima session. */
    status = ux_host_class_pima_session_close(pictbridge ->
        ux_pictbridge_pima, pima_session);
    return(UX_PICTBRIDGE_ERROR_STORE_NOT_AVAILABLE);
}
```

## ux_host_class_pima_num_objects_get

Obtain the number of objects on a storage container from Responder.

### Prototype

```C
UINT ux_host_class_pima_num_objects_get(
    UX_HOST_CLASS_PIMA *pima,
    UX_HOST_CLASS_PIMA_SESSION *pima_session,
    ULONG storage_id,
    ULONG object_format_code)
```

### Description

This function obtains the number of objects stored on a specific storage container of value storage_id matching a specific format code. The number of objects is returned in the field: ux_host_class_pima_session_nb_objects of the pima_session structure.

### Parameters

- **pima**: Pointer to the pima class instance.
- **pima_session**: Pointer to PIMA session
- **storage_id**: ID of the storage container
- **object_format_code**: Objects format code filter.

The Object Format Codes can have one of the following values.

| Object Format Code               | Description   |     USBX code                          |
|----------------------------------|---------------|------------------------------------------|
| 0x3000                           | Undefined Undefined non-image object | UX_HOST_CLASS_PIMA_OFC_UNDEFINED   |
| 0x3001                           | Association Association (e.g. folder) | UX_HOST_CLASS_PIMA_OFC_ASSOCIATION |
| 0x3002                           | Script Device-model specific script | UX_HOST_CLASS_PIMA_OFC_SCRIPT      |
| 0x3003                           | Executable Device model-specific binary executable | UX_HOST_CLASS_PIMA_OFC_EXECUTABLE  |
| 0x3004                           | Text Text file  |   UX_HOST_CLASS_PIMA_OFC_TEXT        |
| 0x3005                           | HTML HyperText Markup Language file (text) | UX_HOST_CLASS_PIMA_OFC_HTML        |
| 0x3006                           | DPOF Digital Print Order Format file (text) | UX_HOST_CLASS_PIMA_OFC_DPOF        |
| 0x3007                           | AIFF Audio clip  |  UX_HOST_CLASS_PIMA_OFC_AIFF        |
| 0x3008                           | WAV Audio clip   |  UX_HOST_CLASS_PIMA_OFC_WAV         |
| 0x3009                           | MP3 Audio clip   |  UX_HOST_CLASS_PIMA_OFC_MP3         |
| 0x300A                           | AVI Video clip   |  UX_HOST_CLASS_PIMA_OFC_AVI         |
| 0x300B                           | MPEG Video clip  |  UX_HOST_CLASS_PIMA_OFC_MPEG        |
| 0x300C                           | ASF Microsoft Advanced Streaming Format (video) | UX_HOST_CLASS_PIMA_OFC_ASF         |
| 0x3800                           | Undefined Unknown image object | UX_HOST_CLASS_PIMA_OFC_QT          |
| 0x3801                           | EXIF/JPEG Exchangeable File Format, JEIDA standard | UX_HOST_CLASS_PIMA_OFC_EXIF_JPEG   |
| 0x3802                           | TIFF/EP Tag Image File Format for Electronic Photography | UX_HOST_CLASS_PIMA_OFC_TIFF_EP     |
| 0x3803                           | FlashPix Structured Storage Image Format | UX_HOST_CLASS_PIMA_OFC_FLASHPIX    |
| 0x3804                           | BMP Microsoft Windows Bitmap file | UX_HOST_CLASS_PIMA_OFC_BMP         |
| 0x3805                           | CIFF Canon Camera Image File Format | UX_HOST_CLASS_PIMA_OFC_CIFF        |
| 0x3806                           | Undefined Reserved |  |
| 0x3807                           | GIF Graphics Interchange Format | UX_HOST_CLASS_PIMA_OFC_GIF         |
| 0x3808                           | JFIF JPEG File Interchange Format | UX_HOST_CLASS_PIMA_OFC_JFIF        |
| 0x3809                           | PCD PhotoCD Image Pac | UX_HOST_CLASS_PIMA_OFC_PCD         |
| 0x380A                           | PICT Quickdraw Image Format | UX_HOST_CLASS_PIMA_OFC_PICT        |
| 0x380B                           | PNG Portable Network Graphics | UX_HOST_CLASS_PIMA_OFC_PNG         |
| 0x380C                           | Undefined Reserved |   |
| 0x380D                           | TIFF Tag Image File Format | UX_HOST_CLASS_PIMA_OFC_TIFF        |
| 0x380E                           | TIFF/IT Tag Image File Format for Information Technology (graphic arts) | UX_HOST_CLASS_PIMA_OFC_TIFF_IT     |
| 0x380F                           | JP2 JPEG2000 Baseline File Format | UX_HOST_CLASS_PIMA_OFC_JP2         |
| 0x3810                           | JPX JPEG2000 Extended File Format | UX_HOST_CLASS_PIMA_OFC_JPX         |
| All other codes with MSN of 0011 | Any Undefined Reserved for future use |                                    |
| All other codes with MSN of 1011 | Any Vendor-Defined Vendor-Defined type: Image |                                    |

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed.
- **UX_HOST_CLASS_PIMA_RC_SESSION_NOT_OPEN**: (0x2003) Session not opened
- **UX_MEMORY_INSUFFICIENT**: (0x12) Not enough memory to create PIMA command.

### Example

```C
/* Get the number of objects on all containers matching a SCRIPT object. */
status = ux_host_class_pima_num_objects_get(pima, pima_session,
    UX_PICTBRIDGE_ALL_CONTAINERS, UX_PICTBRIDGE_OBJECT_SCRIPT);

if (status != UX_SUCCESS)
{
    /* Close the pima session. */
    status = ux_**host_class_pima_session_close(pima, pima_session);

    return(UX_PICTBRIDGE_ERROR_STORE_NOT_AVAILABLE);
} else
    /* The number of objects is returned in the field: pima_session -> ux_host_class_pima_session_nb_objects */
```

## ux_host_class_pima_object_handles_get

Obtain object handles from Responder.

### Prototype

```C
UINT ux_host_class_pima_object_handles_get(
    UX_HOST_CLASS_PIMA *pima,
    UX_HOST_CLASS_PIMA_SESSION *pima_session,
    ULONG *object_handles_array,
    ULONG object_handles_length,
    ULONG storage_id,
    ULONG object_format_code,
    ULONG object_handle_association)
```

### Description

Returns an array of Object Handles present in the storage container indicated by the storage_id parameter. If an aggregated list across all stores is desired, this value shall be set to 0xFFFFFFFF.

### Parameters

- **pima**: Pointer to the pima class instance.
- **pima_session**: Pointer to PIMA session
- **object_handles_array**: Array where handles are returned
- **object_handles_length**: Length of the array
- **storage_id**: ID of the storage container
- **object_format_code**: Format code for object (see table for function ux_host_class_pima_num_objects_get)
- **object_handle_association**: Optional object association value

The object handle association can be one of the value from the table below:

| AssociationCode                        | AssociationType      | Interpretation       |
|----------------------------------------|----------------------|----------------------|
| 0x0000                                 | Undefined            | Undefined            |
| 0x0001                                 | GenericFolder        | Unused               |
| 0x0002                                 | Album                | Reserved             |
| 0x0003                                 | TimeSequence         | DefaultPlaybackDelta |
| 0x0004                                 | HorizontalPanoramic  | Unused               |
| 0x0005                                 | VerticalPanoramic    | Unused               |
| 0x0006                                 | 2DPanoramic          | ImagesPerRow         |
| 0x0007                                 | AncillaryData        | Undefined            |
| All other values with bit 15 set to 0  | Reserved             | Undefined            |
| All values with bit 15 set to 1        | Vendor-Defined       | Vendor-Defined       |

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed.
- **UX_HOST_CLASS_PIMA_RC_SESSION_NOT_OPEN**: (0x2003) Session not opened
- **UX_MEMORY_INSUFFICIENT**: (0x12) Not enough memory to create PIMA command.

### Example

```C
/* Get the array of objects handles on the container. */
status = ux_**host_class_pima_object_handles_get(pima, pima_session,
    pictbridge ->ux_pictbridge_object_handles_array,
    4 * pima_session ->ux_host_class_pima_session_nb_objects,
    UX_PICTBRIDGE_ALL_CONTAINERS,
    UX_PICTBRIDGE_OBJECT_SCRIPT, 0);

if (status != UX_SUCCESS)
{
    /* Close the pima session. */
    status = ux_host_class_pima_session_close(pima, pima_session);
    return(UX_PICTBRIDGE_ERROR_STORE_NOT_AVAILABLE);
}
```

## ux_host_class_pima_object_info_get

Obtain the object information from Responder.

### Prototype

```C
UINT ux_host_class_pima_object_info_get(
    UX_HOST_CLASS_PIMA *pima,
    UX_HOST_CLASS_PIMA_SESSION *pima_session,
    ULONG object_handle,
    UX_HOST_CLASS_PIMA_OBJECT *object)
```

### Description

This function obtains the object information for an object handle.

### Parameters

- **pima**: Pointer to the pima class instance.
- **pima_session**: Pointer to PIMA session
- **object_handle**: Handle of the object
- **object**: Pointer to object information container

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed.
- **UX_HOST_CLASS_PIMA_RC_SESSION_NOT_OPEN**: (0x2003) Session not opened
- **UX_MEMORY_INSUFFICIENT**: (0x12) Not enough memory to create PIMA command.

### Example

```C
/* We search for an object that is a picture or a script. */
object_index = 0;

while (object_index < pima_session -> ux_host_class_pima_session_nb_objects)
{
    /* Get the object info structure. */
    status = ux_**host_class_pima_object_info_get(pima, pima_session,
        pictbridge -> ux_pictbridge_object_handles_array[object_index], 
        pima_object);

    if (status != UX_SUCCESS)
    {
        /* Close the pima session. */
        status = ux_host_class_pima_session_close(pima, pima_session);

        return(UX_PICTBRIDGE_ERROR_INVALID_OBJECT_HANDLE );
    }
}
```

## ux_host_class_pima_object_info_send

Send the object information to Responder.

### Prototype

```C
UINT ux_host_class_pima_object_info_send(
    UX_HOST_CLASS_PIMA *pima,
    UX_HOST_CLASS_PIMA_SESSION *pima_session,
    ULONG storage_id,
    ULONG parent_object_id,
    UX_HOST_CLASS_PIMA_OBJECT *object)
```

### Description

This function sends the storage information for a storage container of value storage_id. The Initiator should use this command before sending an object to the responder.

### Parameters

- **pima**: Pointer to the pima class instance.
- **pima_session**: Pointer to PIMA session.
- **storage_id**: Destination storage ID.
- **parent_object_id**: Parent ObjectHandle on Responder where object should be placed.
- **object**: Pointer to object information container.

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed.
- **UX_HOST_CLASS_PIMA_RC_SESSION_NOT_OPEN**: (0x2003) Session not opened
- **UX_MEMORY_INSUFFICIENT**: (0x12) Not enough memory to create PIMA command.

### Example

```C
/* Send a script info. */
status = ux_host_class_pima_object_info_send(pima, pima_session,
    0, 0, pima_object);

if (status != UX_SUCCESS)
{
    /* Close the pima session. */
    status = ux_host_class_pima_session_close(pima, pima_session);

    return(UX_ERROR);
}
```

## ux_host_class_pima_object_open

Open an object stored in the Responder.

### Prototype

```C
UINT ux_host_class_pima_object_open(
    UX_HOST_CLASS_PIMA *pima,
    UX_HOST_CLASS_PIMA_SESSION *pima_session,
    ULONG object_handle,
    UX_HOST_CLASS_PIMA_OBJECT *object)
```

### Description

This function opens an object on the responder before reading or writing.

### Parameters

- **pima**: Pointer to the pima class instance.
- **pima_session**: Pointer to PIMA session.
- **object_handle**: handle of the object.
- **object**: Pointer to object information container.

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed.
- **UX_HOST_CLASS_PIMA_RC_SESSION_NOT_OPEN**: (0x2003) Session not opened
- **UX_HOST_CLASS_PIMA_RC_OBJECT_ALREADY_OPENED**: (0x2021) Object already opened.
- **UX_MEMORY_INSUFFICIENT**: (0x12) Not enough memory to create PIMA command.

### Example

```C
/* Open the object. */

status = ux_host_class_pima_object_open(pima, pima_session,
    object_handle, pima_object);

/* Check status. */
if (status != UX_SUCCESS)
    return(status);
```

## ux_host_class_pima_object_get

Get an object stored in the Responder.

### Prototype

```C
UINT ux_host_class_pima_object_get(
    UX_HOST_CLASS_PIMA *pima,
    UX_HOST_CLASS_PIMA_SESSION *pima_session,
    ULONG object_handle,
    UX_HOST_CLASS_PIMA_OBJECT *object,
    UCHAR *object_buffer,
    ULONG object_buffer_length,
    ULONG *object_actual_length)
```

### Description

This function gets an object on the responder.

### Parameters

- **pima**: Pointer to the pima class instance.
- **pima_session**: Pointer to PIMA session
- **object_handle**: handle of the object
- **object**: Pointer to object information container
- **object_buffer**: Address of object data
- **object_buffer_length**: Requested length of object
- **object_actual_length**: Length of object returned

### Return Value

- **UX_SUCCESS**: (0x00) The object was transferred
- **UX_HOST_CLASS_PIMA_RC_SESSION_NOT_OPEN**: (0x2003) Session not opened
- **UX_HOST_CLASS_PIMA_RC_OBJECT_NOT_OPENED**: (0x2023) Object not opened.
- **UX_HOST_CLASS_PIMA_RC_ACCESS_DENIED**: (0x200f) Access to object denied
- **UX_HOST_CLASS_PIMA_RC_INCOMPLETE_TRANSFER**: (0x2007) Transfer is incomplete
- **UX_MEMORY_INSUFFICIENT**: (0x12) Not enough memory to create PIMA command.
- **UX_TRANSFER_ERROR**: (0x23) Transfer error while reading object

### Example

```C
/* Open the object. */

status = ux_host_class_pima_object_open(pima, pima_session,
    object_handle, pima_object);

/* Check status. */
if (status != UX_SUCCESS)
    return(status);

/* Set the object buffer pointer. */
object_buffer = pima_object ->ux_host_class_pima_object_buffer;

/* Obtain all the object data. */
while(object_length != 0)
{
    /* Calculate what length to request. */
    if (object_length > UX_PICTBRIDGE_MAX_PIMA_OBJECT_BUFFER)
        /* Request maximum length. */
        requested_length = UX_PICTBRIDGE_MAX_PIMA_OBJECT_BUFFER;
    else
        /* Request remaining length. */
        requested_length = object_length;

    /* Get the object data. */
    status = ux_host_class_pima_object_get(pima, pima_session,
        object_handle, pima_object, object_buffer,
        requested_length, &actual_length);

    if (status != UX_SUCCESS)
    {
        /* We had a problem, abort the transfer. */
        ux_host_class_pima_object_transfer_abort(pima, pima_session,
            object_handle, pima_object);

        /* And close the object. */
        ux_host_class_pima_object_close(pima, pima_session,
            object_handle, pima_object, object);

        return(status);

    }

    /* We have received some data, update the length remaining. */
    object_length -= actual_length;

    /* Update the buffer address. */
    object_buffer += actual_length;

}

/* Close the object. */
status = ux_host_class_pima_object_close(pima, pima_session,
    object_handle, pima_object, object);
```

## ux_host_class_pima_object_send

Send an object stored in the Responder.

### Prototype

```C
UINT ux_host_class_pima_object_send(
    UX_HOST_CLASS_PIMA *pima,
    UX_HOST_CLASS_PIMA_SESSION *pima_session,
    UX_HOST_CLASS_PIMA_OBJECT *object,
    UCHAR *object_buffer, ULONG object_buffer_length)
```

### Description

This function sends an object to the responder.

### Parameters

- **pima**: Pointer to the pima class instance.
- **pima_session**: Pointer to PIMA session
- **object_handle**: handle of the object
- **object**: Pointer to object information container
- **object_buffer**: Address of object data
- **object_buffer_length**: Requested length of object

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed.
- **UX_HOST_CLASS_PIMA_RC_SESSION_NOT_OPEN**: (0x2003) Session not opened
- **UX_HOST_CLASS_PIMA_RC_OBJECT_NOT_OPENED**: (0x2023) Object not opened.
- **UX_HOST_CLASS_PIMA_RC_ACCESS_DENIED**: (0x200f) Access to object denied
- **UX_HOST_CLASS_PIMA_RC_INCOMPLETE_TRANSFER**: (0x2007) Transfer is incomplete
- **UX_MEMORY_INSUFFICIENT**: (0x12) Not enough memory to create PIMA command.
- **UX_TRANSFER_ERROR**: (0x23) Transfer error while writing object

### Example

```C
/* Open the object. */
status = ux_host_class_pima_object_open(pima, pima_session,
    object_handle, pima_object);

/* Get the object length. */
object_length = pima_object ->ux_host_class_pima_object_compressed_size;

/* Recall the object buffer address. */
pima_object_buffer = pima_object ->ux_host_class_pima_object_buffer;

/* Send all the object data. */
while(object_length != 0)
{
    /* Calculate what length to request. */
    if (object_length > UX_PICTBRIDGE_MAX_PIMA_OBJECT_BUFFER)
        /* Request maximum length. */
        requested_length = UX_PICTBRIDGE_MAX_PIMA_OBJECT_BUFFER;
    else
        /* Request remaining length. */
        requested_length = object_length;

    /* Send the object data. */
    status = ux_host_class_pima_object_send(pima,
        pima_session, pima_object,
        pima_object_buffer, requested_length);

    if (status != UX_SUCCESS)
    {
        /* Abort the transfer. */
        ux_host_class_pima_object_transfer_abort(pima, pima_session,
            object_handle, pima_object);

        /* Return status. */
        return(status);
    }

    /* We have sent some data, update the length remaining. */
    object_length -= requested_length;
}

/* Close the object. */
status = ux_host_class_pima_object_close(pima, pima_session, object_handle,
    pima_object, object);
```

## ux_host_class_pima_thumb_get

Get a thumb object stored in the Responder.

### Prototype

```C
UINT ux_host_class_pima_thumb_get(
    UX_HOST_CLASS_PIMA *pima,
    UX_HOST_CLASS_PIMA_SESSION *pima_session,
    ULONG object_handle,
    UX_HOST_CLASS_PIMA_OBJECT *object,
    UCHAR *thumb_buffer, ULONG thumb_buffer_length,
    ULONG *thumb_actual_length)
```

### Description

This function gets a thumb object on the responder.

### Parameters

- **pima**: Pointer to the pima class instance.
- **pima_session**: Pointer to PIMA session.
- **object_handle**: handle of the object.
- **object**: Pointer to object information container.
- **thumb_buffer**: Address of thumb object data.
- **thumb_buffer_length**: Requested length of thumb object.
- **thumb_actual_length**: Length of thumb object returned.

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed.
- **UX_HOST_CLASS_PIMA_RC_SESSION_NOT_OPEN**: (0x2003) Session not opened.
- **UX_HOST_CLASS_PIMA_RC_OBJECT_NOT_OPENED**: (0x2023) Object not opened.
- **UX_HOST_CLASS_PIMA_RC_ACCESS_DENIED**: (0x200f) Access to object denied.
- **UX_HOST_CLASS_PIMA_RC_INCOMPLETE_TRANSFER**: (0x2007) Transfer is incomplete.
- **UX_MEMORY_INSUFFICIENT**: (0x12) Not enough memory to create PIMA command.
- **UX_TRANSFER_ERROR**: (0x23) Transfer error while reading object.

### Example

```C
/* Get the thumb object data. */

status = ux_host_class_pima_thumb_get(pima, pima_session,
    object_handle, pima_object, object_buffer,
    requested_length, &actual_length);

if (status != UX_SUCCESS)
{
    /* And close the object. */
    ux_host_class_pima_object_close(pima, pima_session, object_handle, pima_object, object);

    return(status);
}
```

## ux_host_class_pima_object_delete

Delete an object stored in the Responder.

### Prototype

```C
UINT ux_host_class_pima_object_delete(
    UX_HOST_CLASS_PIMA *pima,
    UX_HOST_CLASS_PIMA_SESSION *pima_session,
    ULONG object_handle)
```

### Description

This function deletes an object on the responder

### Parameters

- **pima**: Pointer to the pima class instance.
- **pima_session**: Pointer to PIMA session
- **object_handle**: handle of the object

### Return Value

- **UX_SUCCESS**: (0x00) The object was deleted.
- **UX_HOST_CLASS_PIMA_RC_SESSION_NOT_OPEN**: (0x2003) Session not opened.
- **UX_HOST_CLASS_PIMA_RC_ACCESS_DENIED**: (0x200f) Cannot delete object.
- **UX_MEMORY_INSUFFICIENT**: (0x12) Not enough memory to create PIMA command.

### Example

```C
/* Delete the object. */
status = ux_host_class_pima_object_delete(pima, pima_session, object_handle, pima_object);

/* Check status. */
if (status != UX_SUCCESS)
    return(status);
```

## ux_host_class_pima_object_close

Close an object stored in the Responder

### Prototype

```C
UINT ux_host_class_pima_object_close(
    UX_HOST_CLASS_PIMA *pima,
    UX_HOST_CLASS_PIMA_SESSION *pima_session,
    ULONG object_handle, UX_HOST_CLASS_PIMA_OBJECT *object)
```

### Description

This function closes an object on the responder.

### Parameters

- **pima**: Pointer to the pima class instance.
- **pima_session**: Pointer to PIMA session.
- **object_handle**: Handle of the object.
- **object**: Pointer to object.

### Return Value

- **UX_SUCCESS**: (0x00) The object was closed.
- **UX_HOST_CLASS_PIMA_RC_SESSION_NOT_OPEN**: (0x2003) Session not opened.
- **UX_HOST_CLASS_PIMA_RC_OBJECT_NOT_OPENED**: (0x2023) Object not opened.
- **UX_MEMORY_INSUFFICIENT**: (0x12) Not enough memory to create PIMA command.

### Example

```C
/* Close the object. */
status = ux_host_class_pima_object_close(pima, pima_session, object_handle, object);
```

## ux_host_class_gser_read

Read from the generic serial interface.

### Prototype

```C
UINT ux_host_class_gser_read(
    UX_HOST_CLASS_GSER *gser,
    ULONG interface_index,
    UCHAR *data_pointer,
    ULONG requested_length,
    ULONG *actual_length)
```

### Description

This function reads from the generic serial interface. The call is blocking and only returns when there is either an error or when the transfer is complete.

### Parameters

- **gser**: Pointer to the gser class instance.
- **interface_index**: Interface index to read from.
- **data_pointer**: Pointer to the buffer address of the data payload.
- **requested_length**: Length to be received.
- **actual_length**: Length actually received.

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed.
- **UX_TRANSFER_TIMEOUT**: (0x5c) Transfer timeout, reading incomplete.

### Example

```C
UINT status;

/* The following example illustrates this service. */
status = ux_host_class_gser_read(cdc_acm, interface_index,data_pointer, requested_length, &actual_length);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_gser_write

Write to the generic serial interface.

### Prototype

```C
UINT ux_host_class_gser_write(
    UX_HOST_CLASS_GSER *gser,
    ULONG interface_index,
    UCHAR *data_pointer,
    ULONG requested_length,
    ULONG *actual_length)
```

### Description

This function writes to the generic serial interface. The call is blocking and only returns when there is either an error or when the transfer is complete.

### Parameters

- **gser**: Pointer to the gser class instance.
- **interface_index**: Interface to which to write.
- **data_pointer**: Pointer to the buffer address of the data payload.
- **requested_length**: Length to be sent.
- **actual_length**: Length actually sent.

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed.
- **UX_TRANSFER_TIMEOUT**: (0x5c) Transfer timeout, writing incomplete.

### Example

```C
UINT status;

/* The following example illustrates this service. */
status = ux_host_class_cdc_acm_write(gser, data_pointer, requested_length, &actual_length);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_gser_ioctl

Perform an IOCTL function to the generic serial interface.

### Prototype

```C
UINT ux_host_class_gser_ioctl(
    UX_HOST_CLASS_GSER *gser,
    ULONG ioctl_function,
    VOID *parameter)
```

### Description

This function performs a specific ioctl function to the gser interface. The call is blocking and only returns when there is either an error or when the command is completed.

### Parameters

- **gser**: Pointer to the gser class instance.
- **ioctl_function**: ioctl function to be performed. See table below for one of the allowed ioctl functions.
- **parameter**: Pointerto a parameter specific to the ioctl

### Return Value

- **UX_SUCCESS**: (0x00) The data transfer was completed.
- **UX_MEMORY_INSUFFICIENT**: (0x12) Not enough memory.
- **UX_HOST_CLASS_UNKNOWN**: (0x59) Wrong class instance
- **UX_FUNCTION_NOT_SUPPORTED**: (0x54) Unknown IOCTL function.

### IOCTL functions

- UX_HOST_CLASS_GSER_IOCTL_SET_LINE_CODING
- UX_HOST_CLASS_GSER_IOCTL_GET_LINE_CODING
- UX_HOST_CLASS_GSER_IOCTL_SET_LINE_STATE
- UX_HOST_CLASS_GSER_IOCTL_SEND_BREAK
- UX_HOST_CLASS_GSER_IOCTL_ABORT_IN_PIPE
- UX_HOST_CLASS_GSER_IOCTL_ABORT_OUT_PIPE
- UX_HOST_CLASS_GSER_IOCTL_NOTIFICATION_CALLBACK
- UX_HOST_CLASS_GSER_IOCTL_GET_DEVICE_STATUS

### Example

```C
UINT status;

/* The following example illustrates this service. */

status = ux_host_class_gser_ioctl(gser,
    UX_HOST_CLASS_GSER_IOCTL_GET_LINE_CODING,
    (VOID *)&line_coding);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_gser_reception_start

Start reception on the generic serial interface

### Prototype

```C
UINT ux_host_class_gser_reception_start(
    UX_HOST_CLASS_GSER *gser,
    UX_HOST_CLASS_GSER_RECEPTION *gser_reception)
```

### Description

This function starts the reception on the generic serial class interface. This function allows for non-blocking reception. When a buffer is received, a callback in invoked into the application.

### Parameters

- **gser** Pointer to the gser class instance.
- **gser_reception** Structure containing the reception parameters

### Return Value

- **UX_SUCCESS** (0x00) The data transfer was completed.
- **UX_HOST_CLASS_UNKNOWN** (0x59) Wrong class instance
- **UX_ERROR** (0x01) Error

### Example

```C
/* Start the reception for gser. AT commands are on interface 2. */
gser_reception.ux_host_class_gser_reception_interface_index =
    UX_DEMO_GSER_AT_INTERFACE;
gser_reception.ux_host_class_gser_reception_block_size =
    UX_DEMO_RECEPTION_BLOCK_SIZE;
gser_reception.ux_host_class_gser_reception_data_buffer =
    gser_reception_buffer;
gser_reception.ux_host_class_gser_reception_data_buffer_size =
    UX_DEMO_RECEPTION_BUFFER_SIZE;
gser_reception.ux_host_class_gser_reception_callback =
    tx_demo_thread_callback;

ux_host_class_gser_reception_start(gser, &gser_reception);
```

## ux_host_class_gser_reception_stop

Stop reception on the generic serial interface

### Prototype

```C
UINT ux_host_class_gser_reception_stop(
    UX_HOST_CLASS_GSER *gser,
    UX_HOST_CLASS_GSER_RECEPTION *gser_reception)
```

### Description

This function stops the reception on the generic serial class interface.

### Parameters

- **gser** Pointer to the gser class instance.
- **gser_reception** Structure containing the reception parameters

### Return Value

- **UX_SUCCESS** (0x00) The data transfer was completed.
- **UX_HOST_CLASS_UNKNOWN** (0x59) Wrong class instance
- **UX_ERROR** (0x01) Error

### Example

```C
/* Stops the reception for gser. */
ux_host_class_gser_reception_stop(gser, &gser_reception);
```

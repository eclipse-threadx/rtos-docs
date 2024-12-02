---
title: Chapter 5 - USBX Host Classes API
description: Learn about the USBX Host Classes API.
---

This chapter covers all the exposed APIs of the USBX host classes. The
following APIs for each class are described in detail.

- HID class
- CDC-ACM class
- CDC-ECM class
- Storage class

## ux_host_class_hid_client_register

Register a HID client to the HID class.

### Prototype

```c
UINT ux_host_class_hid_client_register(
    UCHAR *hid_client_name,
    UINT (*hid_client_handler)
    (struct UX_HOST_CLASS_HID_CLIENT_COMMAND_STRUCT *));
```

### Description

This function is used to register a HID client to the HID class and returns immediately. The HID class needs to find a match between a HID device and HID client before requesting data from this device.

> **Note:** The C string of hid_client_name must be NULL-terminated and the length of it (without the NULL-terminator itself) must be no larger than **UX_HOST_CLASS_HID_MAX_CLIENT_NAME_LENGTH**.

### Parameters

- **hid_client_name** Pointer to the HID client name.
- **hid_client_handler** Pointer to the HID client handler.

### Return Values

- **UX_SUCCESS** (0x00) The data transfer was completed
- **UX_MEMORY_INSUFFICIENT** (0x12) Memory allocation for client failed.
- **UX_MEMORY_ARRAY_FULL** (0x1a) Max clients already registered.
- **UX_HOST_CLASS_ALREADY_INSTALLED** (0x58) This class already exists

### Example

```c
UINT status;

/* The following example illustrates how to register a HID client, in
this case a USB mouse, to the HID class. */

status = ux_host_class_hid_client_register("ux_host_class_hid_client_mouse",
    ux_host_class_hid_mouse_entry);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_hid_report_callback_register

Register a callback from the HID class.

### Prototype

```c
UINT ux_host_class_hid_report_callback_register(
    UX_HOST_CLASS_HID *hid,
    UX_HOST_CLASS_HID_REPORT_CALLBACK *call_back);
```

### Description

This function is used to register a callback from the HID class to the HID client when a report is received and returns immediately.

### Parameters

- **hid** Pointer to the HID class instance
- **call_back** Pointer to the call_back structure

### Return values

- **UX_SUCCESS** (0x00) The data transfer was completed
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) Invalid HID instance.
- **UX_HOST_CLASS_HID_REPORT_ERROR** (0x79) Error in the report callback registration.

### Example

```c
UINT status;

/* This example illustrates how to register a HID client, in this case a USB mouse, to the HID class. In this case, the HID client is asking the HID class to call the client for each usage received in the HID report. */

call_back.ux_host_class_hid_report_callback_id = 0;
call_back.ux_host_class_hid_report_callback_function = ux_host_class_hid_mouse_callback;

call_back.ux_host_class_hid_report_callback_buffer = UX_NULL;
call_back.ux_host_class_hid_report_callback_flags = UX_HOST_CLASS_HID_REPORT_INDIVIDUAL_USAGE;

call_back.ux_host_class_hid_report_callback_length = 0;

status = ux_host_class_hid_report_callback_register(hid, &call_back);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_hid_periodic_report_start

Start the periodic endpoint for a HID class instance.

### Prototype

```c
UINT ux_host_class_hid_periodic_report_start(UX_HOST_CLASS_HID *hid);
```

### Description

This function is used to start the periodic (interrupt) endpoint for the instance of the HID class that is bound to this HID client. The HID class cannot start the periodic endpoint until the HID client is activated and therefore it is left to the HID client to start this endpoint to receive reports.

To protect states for reentry, it waits the semaphore of HID instance before modifying periodic report polling state.

### Input Parameter

- **hid** Pointer to the HID class instance.

### Return Values

- **UX_SUCCESS** (0x00) Periodic reporting successfully started.
- **ux_host_class_hid_PERIODIC_REPORT_ERROR** (0x7A) Error in the periodic report.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) HID class instance does not exist.

### Example

```c
UINT status;

/* The following example illustrates how to start the periodic
endpoint. */

status = ux_host_class_hid_periodic_report_start(hid);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_hid_periodic_report_stop

Stop the periodic endpoint for a HID class instance.

### Prototype

```c
UINT ux_host_class_hid_periodic_report_stop(UX_HOST_CLASS_HID *hid);
```

### Description

This function is used to stop the periodic (interrupt) endpoint for the instance of the HID class that is bound to this HID client. The HID class cannot stop the periodic endpoint until the HID client is deactivated, all its resources freed and therefore it is left to the HID client to stop this endpoint.

To protect states for reentry, it waits the semaphore of HID instance before modifying periodic report polling state.

### Input Parameter

- **hid** Pointer to the HID class instance.

### Return Values

- **UX_SUCCESS** (0x00) Periodic reporting successfully stopped.
- **ux_host_class_hid_PERIODIC_REPORT_ERROR** (0x7A) Error in the periodic report.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) HID class instance does not exist

### Example

```c
UINT status;

/* The following example illustrates how to stop the periodic endpoint. */

status = ux_host_class_hid_periodic_report_stop(hid);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_hid_idle_get

Get a report idle rate from a HID class instance

### Prototype

```c
UINT  ux_host_class_hid_idle_get(UX_HOST_CLASS_HID *hid,
                                USHORT *idle_time, USHORT report_id)
```

### Description

This function is used to get idle rate time period used by device to keep sending reports when there is no data.

To protect states for reentry, it waits the semaphore of HID instance and the semaphore of device default control endpoint before issuing the transfer request.

### Parameters

- **hid** Pointer to the HID class instance.
- **idle_time** Pointer to the buffer to hold idle rate time period between reports.
- **report_id** Report ID.

### Return Values

- **UX_SUCCESS** (0x00) The data transfer was completed.
- **UX_FUNCTION_NOT_SUPPORTED** (0x54) Function not supported.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) HID class instance does not exist.
- **UX_TRANSFER_STALLED** (0x21) Request is not accepted by device, there is no idle rate control and reports are only sent when there is data.

### Example

```c
USHORT idle_time;
UINT   status;

/* The following example illustrates how to get idle rate. */

status = ux_host_class_hid_idle_get(hid, &idle_time, 0);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_hid_idle_set

Send idle rate

### Prototype

```c
UINT  ux_host_class_hid_idle_set(UX_HOST_CLASS_HID *hid,
                                USHORT idle_time, USHORT report_id)
```

### Description

This function is used to set idle rate time period to the device.

To protect states for reentry, it waits the semaphore of HID instance and the semaphore of device default control endpoint before issuing the transfer request.

### Parameters

- **hid** Pointer to the HID class instance.
- **idle_time** Idle rate time period to set.
- **report_id** Report ID.

### Return Values

- **UX_SUCCESS** (0x00) The data transfer was completed.
- **UX_FUNCTION_NOT_SUPPORTED** (0x54) Function not supported.
- **UX_HOST_CLASS_HID_REPORT_ERROR** (0x70) Error in the periodic report.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) HID class instance does not exist.
- **UX_TRANSFER_STALLED** (0x21) Request is not accepted by device, there is no idle rate control and reports are only sent when there is data.

### Example

```c
/* The following example illustrates how to set idle rate. */

status = ux_host_class_hid_idle_set(hid, 50, 0);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_hid_report_get

Get a report from a HID class instance.

### Prototype

```c
UINT ux_host_class_hid_report_get(
    UX_HOST_CLASS_HID *hid,
    UX_HOST_CLASS_HID_CLIENT_REPORT *client_report);
```

### Description

This function is used to receive a report directly from the device without relying on the periodic endpoint. This report is coming from the control endpoint but its treatment is the same as though it were coming on the periodic endpoint.

To protect states for reentry, it waits the semaphore of HID instance and the semaphore of device default control endpoint before issuing the transfer request.

### Parameters

- **hid** Pointer to the HID class instance.
- **client_report** Pointer to the HID client report.

### Return Values

- **UX_SUCCESS** (0x00) The report was successfully received.
- **UX_HOST_CLASS_HID_REPORT_ERROR** (0x70) Either client report was invalid or error during transfer.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) HID class instance does not exist.
- **UX_BUFFER_OVERFLOW** (0x5d) The buffer supplied is not big enough to accommodate the uncompressed report.

### Example

```c
UX_HOST_CLASS_HID_CLIENT_REPORT input_report;

UINT status;

/* The following example illustrates how to get a report. */

input_report.ux_host_class_hid_client_report = hid_report;
input_report.ux_host_class_hid_client_report_buffer = buffer;
input_report.ux_host_class_hid_client_report_length = length;
input_report.ux_host_class_hid_client_flags = UX_HOST_CLASS_HID_REPORT_INDIVIDUAL_USAGE;

status = ux_host_class_hid_report_get(hid, &input_report);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_hid_report_set

Send a report

### Prototype

```c
UINT ux_host_class_hid_report_set(
    UX_HOST_CLASS_HID *hid,
    UX_HOST_CLASS_HID_CLIENT_REPORT *client_report);
```

### Description

This function is used to send a report directly to the device.

To protect states for reentry, it waits the semaphore of HID instance and the semaphore of device default control endpoint before issuing the transfer request.

### Parameters

- **hid** Pointer to the HID class instance.
- **client_report** Pointer to the HID client report.

### Return Values

- **UX_SUCCESS** (0x00) The report was successfully sent.
- **UX_HOST_CLASS_HID_REPORT_ERROR** (0x70) Either client report was invalid or error during transfer.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) HID class instance does not exist.
- **UX_HOST_CLASS_HID_REPORT_OVERFLOW** (0x5d) The buffer supplied is not big enough to accommodate the uncompressed report.

### Example

```c
/* The following example illustrates how to send a report. */

UX_HOST_CLASS_HID_CLIENT_REPORT input_report;

input_report.ux_host_class_hid_client_report = hid_report;
input_report.ux_host_class_hid_client_report_buffer = buffer;
input_report.ux_host_class_hid_client_report_length = length;
input_report.ux_host_class_hid_client_report_flags = UX_HOST_CLASS_HID_REPORT_INDIVIDUAL_USAGE;

status = ux_host_class_hid_report_set(hid, &input_report);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_hid_mouse_buttons_get

Get mouse buttons.

### Prototype

```c
UINT ux_host_class_hid_mouse_buttons_get(
    UX_HOST_CLASS_HID_MOUSE *mouse_instance,
    ULONG *mouse_buttons);
```

### Description

This function is used to get the mouse buttons.

Note the polling of button states are in background. The last updated buttons states are returned immediately.

### Parameters

- **mouse_instance** Pointer to the HID mouse instance.
- **mouse_buttons** Pointer to the return buttons.

### Return Values

- **UX_SUCCESS** (0x00) Mouse button successfully retrieved.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) HID class instance does not exist.

### Example

```c
/* The following example illustrates how to obtain mouse buttons. */

UX_HOST_CLASS_HID_MOUSE *mouse_instance;

ULONG mouse_buttons;

status = ux_host_class_hid_mouse_button_get(mouse_instance, &mouse_buttons);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_hid_mouse_position_get

Get mouse position.

### Prototype

```c
UINT ux_host_class_hid_mouse_position_get(
    UX_HOST_CLASS_HID_MOUSE *mouse_instance,
    SLONG *mouse_x_position, 
    SLONG *mouse_y_position);
```

### Description

This function is used to get the mouse position in x & y coordinates.

Note the polling of position states are in background. The last updated position states are returned immediately.

### Parameters

- **mouse_instance** Pointer to the HID mouse instance.
- **mouse_x_position** Pointer to the x coordinate.
- **mouse_y_position** Pointer to the y coordinate.

### Return Values

- **UX_SUCCESS** (0x00) X &amp; Y coordinates successfully retrieved.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) HID class instance does not exist.

### Example

```c
/* The following example illustrates how to obtain mouse coordinates. */

UX_HOST_CLASS_HID_MOUSE *mouse_instance;

SLONG mouse_x_position;
SLONG mouse_y_position;

status = ux_host_class_hid_mouse_position_get(mouse_instance,
    &mouse_x_position, &mouse_y_position);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_hid_keyboard_key_get

Get keyboard key and state.

### Prototype

```c
UINT ux_host_class_hid_keyboard_key_get(
    UX_HOST_CLASS_HID_KEYBOARD *keyboard_instance,
    ULONG *keyboard_key, 
    ULONG *keyboard_state);
```

### Description

This function is used to get the keyboard key and state.

Note the polling of key states are in background. The last updated keys states are returned immediately.

### Parameters

- **keyboard_instance** Pointer to the HID keyboard instance.
- **keyboard_key** Pointer to keyboard key container.
- **keyboard_state** Pointer to the keyboard state container.

### Return Values

- **UX_SUCCESS** (0x00) Key and state successfully retrieved.
- **UX_ERROR** (0xff) Nothing to report.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) HID class instance does not exist.

The keyboard state can have the following values.

- **UX_HID_KEYBOARD_STATE_KEY_UP** 0x10000
- **UX_HID_KEYBOARD_STATE_NUM_LOCK** 0x0001
- **UX_HID_KEYBOARD_STATE_CAPS_LOCK** 0x0002
- **UX_HID_KEYBOARD_STATE_SCROLL_LOCK** 0x0004
- **UX_HID_KEYBOARD_STATE_MASK_LOCK** 0x0007
- **UX_HID_KEYBOARD_STATE_LEFT_SHIFT** 0x0100
- **UX_HID_KEYBOARD_STATE_RIGHT_SHIFT** 0x0200
- **UX_HID_KEYBOARD_STATE_SHIFT** 0x0300
- **UX_HID_KEYBOARD_STATE_LEFT_ALT** 0x0400
- **UX_HID_KEYBOARD_STATE_RIGHT_ALT** 0x0800
- **UX_HID_KEYBOARD_STATE_ALT** 0x0a00
- **UX_HID_KEYBOARD_STATE_LEFT_CTRL** 0x1000
- **UX_HID_KEYBOARD_STATE_RIGHT_CTRL** 0x2000
- **UX_HID_KEYBOARD_STATE_CTRL** 0x3000
- **UX_HID_KEYBOARD_STATE_LEFT_GUI** 0x4000
- **UX_HID_KEYBOARD_STATE_RIGHT_GUI** 0x8000
- **UX_HID_KEYBOARD_STATE_GUI** 0xa000

### Example

```c
while (1)
{

    /* Get a key/state from the keyboard. */
    status = ux_host_class_hid_keyboard_key_get(keyboard,
        &keyboard_char, &keyboard_state);

    /* Check if there is something. */
    if (status == UX_SUCCESS)
    {

        #ifdef UX_HOST_CLASS_HID_KEYBOARD_EVENTS_KEY_CHANGES_MODE
            if (keyboard_state & UX_HID_KEYBOARD_STATE_KEY_UP)
            {
                /* The key was released. */
            } else
            {
                /* The key was pressed. */
            }
        #endif

        /* We have a character in the queue. */
        keyboard_queue[keyboard_queue_index] = (UCHAR) keyboard_char;

        /* Can we accept more ? */
        if(keyboard_queue_index < 1024)
            keyboard_queue_index++;
    }

    tx_thread_sleep(10);

}
```

## ux_host_class_hid_keyboard_ioctl

Perform an IOCTL function to the HID keyboard.

### Prototype

```c
UINT ux_host_class_hid_keyboard_ioctl(
    UX_HOST_CLASS_HID_KEYBOARD *keyboard_instance,
    ULONG ioctl_function, VOID *parameter);
```

### Description

This function performs a specific ioctl function to the HID keyboard. The call is blocking and only returns when there is either an error or when the command is completed.

### Parameters

- **keyboard_instance** Pointer to the HID keyboard instance.
- **ioctl_function** ioctl function to be performed. See table below for one of the allowed ioctl functions.
- **parameter** Pointer to a parameter specific to the ioctl.

### Return Values

- **UX_SUCCESS** (0x00) The ioctl function completed successfully.
- **UX_FUNCTION_NOT_SUPPORTED** (0x54) Unknown IOCTL function

### IOCTL functions

- UX_HID_KEYBOARD_IOCTL_SET_LAYOUT
- UX_HID_KEYBOARD_IOCTL_KEY_DECODING_ENABLE
- UX_HID_KEYBOARD_IOCTL_KEY_DECODING_DISABLE

### Example – change keyboard layout

```c
UINT status;

/* This example shows usage of the SET_LAYOUT IOCTL function.
    USBX receives raw key values from the device (these raw values
    are defined in the HID usage table specification) and optionally
    decodes them for application usage. The decoding is performed
    based on a set of arrays that act as maps – which array is used
    depends on the raw key value (i.e. keypad and non-keypad) and
    the current state of the keyboard (i.e. shift, caps lock, etc.). */

/* When the shift condition is not present and the raw key value
    is not within the keypad value range, this array will be used to decode the raw key value. */

static UCHAR keyboard_layout_raw_to_unshifted_map[] =
{
    0,0,0,0,
    'a','b','c','d','e','f','g',
    'h','i','j','k','l','m','n',
    'o','p','q','r','s','t',
    'u','v','w','x','y','z',
    '1','2','3','4','5','6','7','8','9','0',
    0x0d,0x1b,0x08,0x07,0x20,'-','=','[',']',
    '\\','#',';',0x27,'`',',','.','/',0xf0,
    0xbb,0xbc,0xbd,0xbe,0xbf,0xc0,0xc1,0xc2,0xc3,0xc4,0xc5,0xc6,
    0x00,0xf1,0x00,0xd2,0xc7,0xc9,0xd3,0xcf,0xd1,0xcd,0xcd,0xd0,0xc8,0xf2,
    '/','*','-','+',
    0x0d,'1','2','3','4','5','6','7','8','9','0','.','\\',0x00,0x00,'=',
    0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,
};

/* When the shift condition is present and the raw key value
    is not within the keypad value range, this array will be used to decode the raw key value. */

static UCHAR keyboard_layout_raw_to_shifted_map[] =
{
    0,0,0,0,
    'A','B','C','D','E','F','G',
    'H','I','J','K','L','M','N',
    'O','P','Q','R','S','T',
    'U','V','W','X','Y','Z',
    '!','@','#','$','%','^','&','*','(',')',
    0x0d,0x1b,0x08,0x07,0x20,'_','+','{','}',
    '|','~',':','"','~','<','>','?',0xf0,
    0xbb,0xbc,0xbd,0xbe,0xbf,0xc0,0xc1,0xc2,0xc3,0xc4,0xc5,0xc6,
    0x00,0xf1,0x00,0xd2,0xc7,0xc9,0xd3,0xcf,0xd1,0xcd,0xcd,0xd0,0xc8,0xf2,
    '/','*','-','+',
    0x0d,'1','2','3','4','5','6','7','8','9','0','.','\\',0x00,0x00,'=',
    0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,
};

/* When numlock is on and the raw key value is within the keypad
    value range, this array will be used to decode the raw key value. */

static UCHAR keyboard_layout_raw_to_numlock_on_map[] =
{
    '/','*','-','+',
    0x0d,'1','2','3','4','5','6','7','8','9','0','.','\\',0x00,0x00,'=',
};

/* When numlock is off and the raw key value is within the keypad value
    range, this array will be used to decode the raw key value. */
static UCHAR keyboard_layout_raw_to_numlock_off_map[] =
{
    '/','*','-','+',
    0x0d,0xcf,0xd0,0xd1,0xcb,'5',0xcd,0xc7,0xc8,0xc9,0xd2,0xd3,'\\',0x00,0x
    00,'=',
};

/* Specify the keyboard layout for USBX usage. */
static UX_HOST_CLASS_HID_KEYBOARD_LAYOUT keyboard_layout =
{
    keyboard_layout_raw_to_shifted_map,
    keyboard_layout_raw_to_unshifted_map,
    keyboard_layout_raw_to_numlock_on_map,
    keyboard_layout_raw_to_numlock_off_map,
    /* The maximum raw key value. Values larger than this are discarded. */
    UX_HID_KEYBOARD_KEYS_UPPER_RANGE,
    /* The raw key value for the letter 'a'. */
    UX_HID_KEYBOARD_KEY_LETTER_A,
    /* The raw key value for the letter 'z'. */
    UX_HID_KEYBOARD_KEY_LETTER_Z,
    /* The lower range raw key value for keypad keys - inclusive. */
    UX_HID_KEYBOARD_KEYS_KEYPAD_LOWER_RANGE,
    /* The upper range raw key value for keypad keys. */
    UX_HID_KEYBOARD_KEYS_KEYPAD_UPPER_RANGE
};

/* Call the IOCTL function to change the keyboard layout. */
status = ux_host_class_hid_keyboard_ioctl(keyboard,
    UX_HID_KEYBOARD_IOCTL_SET_LAYOUT, (VOID *)&keyboard_layout);

/* If status equals UX_SUCCESS, the operation was successful. */
```

### Example – disable keyboard key decode

```c
UINT status;

/* The following example illustrates IOCTL function of Disable key decode from keyboard layout. */
status = ux_host_class_hid_keyboard_ioctl(keyboard,
    UX_HID_KEYBOARD_IOCTL_DISABLE_KEYS_DECODE, UX_NULL);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_hid_remote_control_usage_get

Get remote control usage

### Prototype

```c
UINT ux_host_class_hid_remote_control_usage_get(
    UX_HOST_CLASS_HID_REMOTE_CONTROL *remote_control_instance,
    LONG *usage, 
    ULONG *value);
```

### Description

This function is used to get the remote control usages.

Note the polling of remote control usages are in background and the usages are put in a circular queue, the function just returns the very first usage received and saved in the queue, immediately. When there is nothing in the queue status error is returned.

### Parameters

- **remote_control_instance** Pointer to the HID remote control instance.
- **usage** Pointer to the usage.
- **value** Pointer to the value for the usage.

### Return Values

- **UX_SUCCESS** (0x00) The data transfer was completed.
- **UX_ERROR** (0xff) Nothing to report.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) HID class instance does not exist.

The list of all possible usages is too long to fit in this user guide. For a full description, the ux_host_class_hid.h has the entire set of possible values.

### Example

```c
/* Read usages and values as the user changes the vol/bass/treble buttons on the speaker */

while (remote_control != UX_NULL)
{
    status = ux_host_class_hid_remote_control_usage_get(remote_control, &usage, &value);
    if (status == UX_SUCCESS)
    {
        /* We have something coming from the HID remote control,
        we filter the usage here and only allow the
        volume usage which can be VOLUME, VOLUME_INCREMENT or VOLUME_DECREMENT */
        switch(usage)
        {
            case UX_HOST_CLASS_HID_CONSUMER_VOLUME :
            case UX_HOST_CLASS_HID_CONSUMER_VOLUME_INCREMENT :
            case UX_HOST_CLASS_HID_CONSUMER_VOLUME_DECREMENT :

            if (value<0x80)
            {
                if (current_volume + audio_control.ux_host_class_audio_control_res < 0xffff)
                    current_volume = current_volume + audio_control.ux_host_class_audio_control_res;
                } else {
                if (current_volume > audio_control.ux_host_class_audio_control_res)
                    current_volume = current_volumeaudio_control.ux_host_class_audio_control_res;
            }

            audio_control.ux_host_class_audio_control_channel = 1;
            audio_control.ux_host_class_audio_control = UX_HOST_CLASS_AUDIO_VOLUME_CONTROL;
            audio_control.ux_host_class_audio_control_cur = current_volume;
            status = ux_host_class_audio_control_value_set(audio, &audio_control);
            audio_control.ux_host_class_audio_control_channel = 2;
            audio_control.ux_host_class_audio_control = UX_HOST_CLASS_AUDIO_VOLUME_CONTROL;
            audio_control.ux_host_class_audio_control_cur = current_volume;
            status = ux_host_class_audio_control_value_set(audio, &audio_control);
            break;

        }
    }
    tx_thread_sleep(10);

}
```

### ux_host_class_cdc_acm_read

Read from the cdc_acm interface.

### Prototype

```c
UINT ux_host_class_cdc_acm_read(
    UX_HOST_CLASS_CDC_ACM *cdc_acm,
    UCHAR *data_pointer,
    ULONG requested_length,
    ULONG *actual_length);
```

### Description

This function reads from the cdc_acm interface. The call is blocking and only returns when there is either an error or when the transfer is complete.

> **Note:** This functions reads raw bulk data from device, so it keeps pending until buffer is full or device terminates the transfer by a short packet (including Zero Length Packet). For more details, please refer to [**General Considerations for Bulk Transfer**](../usbx-device-stack-5.md#general-considerations-for-bulk-transfer).
> The function reads bytes from the device packet by packet. If the prepared buffer size is smaller than a packet and the device sends more data than expected (in other words, the prepared buffer size is not a multiple of the USB endpoint's max packet size), then buffer overflow will occur. To avoid this issue, the recommended way to read is to allocate a buffer exactly one packet size (USB endpoint max packet size). This way if there is more data, the next read can get it and no buffer overflow will occur. If there is less data, the current read can get a short packet instead of generating an error.

### Parameters

- **cdc_acm** Pointer to the cdc_acm class instance.
- **data_pointer** Pointer to the buffer address of the data payload.
- **requested_length** Length to be received.
- **actual_length** Length actually received.

### Return Values

- **UX_SUCCESS** (0x00) The data transfer was completed.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) The cdc_acm instance is invalid.
- **UX_TRANSFER_TIMEOUT** (0x5c) Transfer timeout, reading incomplete.
- **UX_TRANSFER_BUFFER_OVERFLOW** (0x27) Transfer buffer overflow, inside a USB packet, host sending more bytes than available buffer.

### Example

```c
UINT status;

/* The following example illustrates this service. */

status = ux_host_class_cdc_acm_read(cdc_acm, data_pointer,
    requested_length, &actual_length);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_cdc_acm_write

Write to the cdc_acm interface

### Prototype

```c
UINT ux_host_class_cdc_acm_write(
    UX_HOST_CLASS_CDC_ACM *cdc_acm,
    UCHAR *data_pointer, 
    ULONG requested_length, 
    ULONG *actual_length);
```

### Description

This function writes to the cdc_acm interface. The call is blocking and only returns when there is either an error or when the transfer is complete.

### Parameters

- **cdc_acm** Pointer to the cdc_acm class instance.
- **data_pointer** Pointer to the buffer address of the data payload.
- **requested_length** Length to be sent.
- **actual_length** Length actually sent.

### Return Values

- **UX_SUCCESS** (0x00) The data transfer was completed.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) The cdc_acm instance is invalid.
- **UX_TRANSFER_TIMEOUT** (0x5c) Transfer timeout, writing incomplete.

### Example

```c
UINT status;

/* The following example illustrates this service. */

status = ux_host_class_cdc_acm_write(cdc_acm, data_pointer,
    requested_length, &actual_length);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_cdc_acm_ioctl

Perform an IOCTL function to the cdc_acm interface.

### Prototype

```c
UINT ux_host_class_cdc_acm_ioctl(
    UX_HOST_CLASS_CDC_ACM *cdc_acm,
    ULONG ioctl_function, 
    VOID *parameter);
```

### Description

This function performs a specific ioctl function to the cdc_acm interface. The call is blocking and only returns when there is either an error or when the command is completed.

### Parameters

- **cdc_acm** Pointer to the cdc_acm class instance.
- **ioctl_function** ioctl function to be performed. See table below for one of the allowed ioctl functions.
- **parameter** Pointer to a parameter specific to the ioctl

### Return Value

- **UX_SUCCESS** (0x00) The data transfer was completed.
- **UX_MEMORY_INSUFFICIENT** (0x12) Not enough memory.
- **UX_HOST_CLASS_INSTANCE_UNKNOWN** (0x5b) CDC-ACM instance is in an invalid state.
- **UX_FUNCTION_NOT_SUPPORTED** (0x54) Unknown IOCTL function.

### IOCTL functions:

- UX_HOST_CLASS_CDC_ACM_IOCTL_SET_LINE_CODING
- UX_HOST_CLASS_CDC_ACM_IOCTL_GET_LINE_CODING
- UX_HOST_CLASS_CDC_ACM_IOCTL_SET_LINE_STATE
- UX_HOST_CLASS_CDC_ACM_IOCTL_SEND_BREAK
- UX_HOST_CLASS_CDC_ACM_IOCTL_ABORT_IN_PIPE
- UX_HOST_CLASS_CDC_ACM_IOCTL_ABORT_OUT_PIPE
- UX_HOST_CLASS_CDC_ACM_IOCTL_NOTIFICATION_CALLBACK
- UX_HOST_CLASS_CDC_ACM_IOCTL_GET_DEVICE_STATUS

```c
UINT status;

/* The following example illustrates this service. */

status = ux_host_class_cdc_acm_ioctl(cdc_acm,
    UX_HOST_CLASS_CDC_ACM_IOCTL_GET_LINE_CODING, (VOID *)&line_coding);

/* If status equals UX_SUCCESS, the operation was successful. */
```

## ux_host_class_cdc_acm_reception_start

Begins background reception of data from the device.

### Prototype

```c
UINT ux_host_class_cdc_acm_reception_start(
    UX_HOST_CLASS_CDC_ACM *cdc_acm,
    UX_HOST_CLASS_CDC_ACM_RECEPTION *cdc_acm_reception);
```

### Description

This function causes USBX to continuously read data from the device in the background. Upon completion of each transaction, the callback specified in **cdc_acm_reception** is invoked so the application may perform further processing of the transaction's data.

> **Note:** **ux_host_class_cdc_acm_read** must not be used while background reception is in use.

> **Note:** **cdc_acm_reception** must have its callback function assigned, where *ux_host_class_cdc_acm_reception_data_tail* must be moved after data processed. The example of this callback implement follows:
> ```c
> void demo_host_cdc_acm_reception_callback(UX_HOST_CLASS_CDC_ACM *cdc_acm, UINT status, UCHAR *reception_buffer, ULONG reception_size)
> {
> UX_HOST_CLASS_CDC_ACM_RECEPTION *reception = cdc_acm -> ux_host_class_cdc_acm_reception;
> 
>   /* Perform processing of the transaction's data at reception_buffer.  */
> 
>   /* Move reception data tail to next buffer.  */
>   if (reception -> ux_host_class_cdc_acm_reception_data_tail + reception -> ux_host_class_cdc_acm_reception_block_size >= reception -> ux_host_class_cdc_acm_reception_data_buffer + reception -> ux_host_class_cdc_acm_reception_data_buffer_size)
> 
>       /* We are at the end of the buffer. Move back to the beginning.  */
>       reception -> ux_host_class_cdc_acm_reception_data_tail = reception -> ux_host_class_cdc_acm_reception_data_buffer;
>   else
> 
>       /* Program the tail to be after the current buffer.  */
>       reception -> ux_host_class_cdc_acm_reception_data_tail += reception -> ux_host_class_cdc_acm_reception_block_size;
> }
> ```

### Parameters

- **cdc_acm** Pointer to the cdc_acm class instance.
- **cdc_acm_reception** Pointer to parameter that contains values defining behavior of background reception. The layout of this parameter follows:

```c
typedef struct UX_HOST_CLASS_CDC_ACM_RECEPTION_STRUCT
{
    ULONG ux_host_class_cdc_acm_reception_state;
    ULONG ux_host_class_cdc_acm_reception_block_size;
    UCHAR *ux_host_class_cdc_acm_reception_data_buffer;
    ULONG ux_host_class_cdc_acm_reception_data_buffer_size;
    UCHAR *ux_host_class_cdc_acm_reception_data_head;
    UCHAR *ux_host_class_cdc_acm_reception_data_tail;
    VOID (*ux_host_class_cdc_acm_reception_callback)(struct UX_HOST_CLASS_CDC_ACM_STRUCT *cdc_acm,
        UINT status, UCHAR *reception_buffer, ULONG reception_size);
} UX_HOST_CLASS_CDC_ACM_RECEPTION;
```

### Return Value

- **UX_SUCCESS** (0x00) Background reception successfully started.
- **UX_HOST_CLASS_INSTANCE _UNKNOWN** (0x5b) Wrong class instance.

```c
UINT status;
UX_HOST_CLASS_CDC_ACM_RECEPTION cdc_acm_reception;

/* Setup the background reception parameter. */

/* Set the desired max read size for each transaction.
    For example, if this value is 64, then the maximum amount of
    data received from the device in a single transaction is 64.
    If the amount of data received from the device is less than this value,
    the callback will still be invoked with the actual amount of data received. */
cdc_acm_reception.ux_host_class_cdc_acm_reception_block_size = block_size;

/* Set the buffer where the data from the device is read to. */
cdc_acm_reception.ux_host_class_cdc_acm_reception_data_buffer = cdc_acm_reception_buffer;

/* Set the size of the data reception buffer.
    Note that this should be at least as large as ux_host_class_cdc_acm_reception_block_size. */
cdc_acm_reception.ux_host_class_cdc_acm_reception_data_buffer_size = cdc_acm_reception_buffer_size;

/* Set the callback that is to be invoked upon each reception transfer completion. */
cdc_acm_reception.ux_host_class_cdc_acm_reception_callback = reception_callback;

/* Start background reception using the values we defined in the reception parameter. */
status = ux_host_class_cdc_acm_reception_start(cdc_acm_host_data, &cdc_acm_reception);

/* If status equals UX_SUCCESS, background reception has successfully started. */
```

## ux_host_class_cdc_acm_reception_stop

Stops background reception of packets.

### Prototype

```c
UINT ux_host_class_cdc_acm_reception_stop(
    UX_HOST_CLASS_CDC_ACM *cdc_acm,
    UX_HOST_CLASS_CDC_ACM_RECEPTION *cdc_acm_reception);
```

### Description

This function causes USBX to stop background reception previously
started by **ux_host_class_cdc_acm_reception_start**.

### Parameters

- **cdc_acm** Pointer to the cdc_acm class instance.
- **cdc_acm_reception** Pointer to the same parameter that was used to start background reception. The layout of this parameter follows:

```c
typedef struct UX_HOST_CLASS_CDC_ACM_RECEPTION_STRUCT
{
    ULONG ux_host_class_cdc_acm_reception_state;
    ULONG ux_host_class_cdc_acm_reception_block_size;
    UCHAR *ux_host_class_cdc_acm_reception_data_buffer;
    ULONG ux_host_class_cdc_acm_reception_data_buffer_size;
    UCHAR *ux_host_class_cdc_acm_reception_data_head;
    UCHAR *ux_host_class_cdc_acm_reception_data_tail;
    VOID (*ux_host_class_cdc_acm_reception_callback)(
            struct UX_HOST_CLASS_CDC_ACM_STRUCT *cdc_acm, UINT status,
            UCHAR *reception_buffer, ULONG reception_size);
} UX_HOST_CLASS_CDC_ACM_RECEPTION;
```

### Return Value

- **UX_SUCCESS** (0x00) Background reception successfully stopped.
- **UX_HOST_CLASS_INSTANCE _UNKNOWN** (0x5b) Wrong class instance.

```c
UINT status;
UX_HOST_CLASS_CDC_ACM_RECEPTION cdc_acm_reception;

/* Stop background reception. The reception parameter should be the same
    that was passed to ux_host_class_cdc_acm_reception_start. */
status = ux_host_class_cdc_acm_reception_stop(cdc_acm, &cdc_acm_reception);

/* If status equals UX_SUCCESS, background reception has successfully stopped. */
```

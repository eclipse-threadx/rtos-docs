---
title: Chapter 5 - Generating trace buffers
description: This chapter contains a description about how to build a TraceX event buffer and also describes the underlying format of the buffer. 
---


This chapter contains a description about how to build a TraceX event buffer and also describes the underlying format of the buffer.

## ThreadX Event Trace Support

ThreadX provides built-in event trace support for all ThreadX services, thread state changes, and user-defined events. The ThreadX event-trace capability is primarily designed as a post-mortem tool to analyze the last "n" activities in the application. From this information, the developer may spot problems and/or potential targets of optimization.

TraceX graphically displays the event trace buffer built by ThreadX. The following describes how to build the buffer and describes the underlying format of the buffer.

## Enabling Event Trace

To enable event trace, define the time-stamp constants, build the ThreadX library with **TX_ENABLE_EVENT_TRACE** defined, and enable tracing by calling the **tx_trace_enable** function.

## Defining Time-Stamp Constants

The time-stamp constants are designed to provide the developer control over the time-stamp used in the event trace entries. The two time-stamp constants and their default values are as follows:

```c
#ifndef TX_TRACE_TIME_SOURCE
#define TX_TRACE_TIME_SOURCE ++_tx_trace_simulated_time
#endif
#ifndef TX_TRACE_TIME_MASK
#define TX_TRACE_TIME_MASK   0xFFFFFFFFUL
#endif
```

The above constants are defined in **tx_port.h** and create a "fake" time-stamp that simply increments by one on each event. The following is an example of an actual timestamp definition:

```c
#ifndef TX_TRACE_TIME_SOURCE
#define TX_TRACE_TIME_SOURCE ((ULONG) 0x0x13000004)
#endif
#ifndef TX_TRACE_TIME_MASK
#define TX_TRACE_TIME_MASK 0xFFFFFFFFUL
#endif
```

The above constants specify a 32-bit timer that is obtained by reading the address 0x13000004. Most application specific time-stamps should be setup in a similar fashion.

## Exporting the Trace Buffer

TraceX needs the trace buffer in a binary, Intel HEX, or Motorola S-Record file format on the host. The easiest way to accomplish this is to stop the target and instruct your debugger to dump the memory area you supplied to ***tx_trace_enable*** function into a file on the host.

> **Warning:** ***Be careful not to stop the target within a trace gathering code itself. Doing so can cause invalid trace information. If the program is halted within ThreadX, it is best to step over any trace insert macro before dumping the trace buffer.***

> **Important:** *Appendix D shows how to dump the trace buffer from within a variety of development tools.*

## Extended Event Trace API

When ThreadX is built with **TX_ENABLE_EVENT_TRACE** defined, the following new event trace APIs are available to the application:

- tx_trace_enable: *Enable event tracing*
- tx_trace_event_filter: *Filter specified event(s)*
- tx_trace_event_unfilter: *Unfilter specified event(s)*
- tx_trace_disable: *Disable event tracing*
- tx_trace_isr_enter_insert: *Insert ISR enter trace event*
- tx_trace_isr_exit_insert: *Insert ISR exit trace event*
- tx_trace_buffer_full_notify: *Register trace buffer full application callback*
- tx_trace_user_event_insert: *Insert user event*

### tx_trace_enable

Enable event tracing

#### Prototype

```c
UINT tx_trace_enable (VOID *trace_buffer_start,
     ULONG trace_buffer_size, ULONG registry_entries);
```

#### Description
This service enables event tracing inside ThreadX. The trace buffer and the maximum number of ThreadX objects are supplied by the
application.

> **Important:** The ThreadX library and application must be built with **TX_ENABLE_EVENT_TRACE** defined in order to use event tracing.

#### Input Parameters

- **trace_buffer_start**: Pointer to the start of the user-supplied trace buffer.
- **trace_buffer_size**: Total number of bytes in the memory for the trace buffer. The larger the trace buffer, the more entries it is able to store.
- **registry_entries**: Number of application ThreadX objects to keep in the trace registry. The registry is used to correlate object addresses with object names. This is highly useful for GUI trace analysis tools.

#### Return Values

- **TX_SUCCESS** (0x00) Successful event trace enable.
- **TX_SIZE_ERROR** (0x05) Specified trace buffer size is too small. It must be large enough for the trace header, the object registry, and at least one trace entry.
- **TX_NOT_DONE** (0x20) Event tracing was already enabled.
- **TX_FEATURE_NOT_ENABLED** (0xFF) System was not compiled with trace enabled.

#### Allowed From

Initialization and threads

#### Example

```c
UCHAR my_trace_buffer[64000];

/* Enable event tracing using the global "my_trace_buffer" memory and supporting a maximum of 30 ThreadX objects in the registry. */
status = tx_trace_enable (&my_trace_buffer, 64000, 30);

/* If status is TX_SUCCESS the event tracing is enabled. */
```

#### See Also

tx_trace_event_filter, tx_trace_event_unfilter,
tx_trace_disable, tx_trace_isr_enter_insert,
tx_trace_isr_exit_insert, tx_trace_buffer_full_notify,
tx_trace_user_event_insert

### tx_trace_event_filter

Filter specified events

#### Prototype

```c
UINT tx_trace_event_filter (ULONG  vent_filter_bits);
```

#### Description

This service filters the specified event(s) from being inserted into the active trace buffer. Note that by default no events are filtered after *tx_trace_enable* is called.

> **Important:** The ThreadX library and application must be built with **TX_ENABLE_EVENT_TRACE** defined in order to use event tracing.

#### Input Parameters

- **event_filter_bits**: Bits that correspond to events to filter. Multiple events may be filtered by simply oring together the
appropriate constants. Valid constants for this variable are defined as follows:

```c
TX_TRACE_ALL_EVENTS                   0x000007FF
TX_TRACE_INTERNAL_EVENTS              0x00000001
TX_TRACE_BLOCK_POOL_EVENTS            0x00000002
TX_TRACE_BYTE_POOL_EVENTS             0x00000004
TX_TRACE_EVENT_FLAGS_EVENTS           0x00000008
TX_TRACE_INTERRUPT_CONTROL_EVENT      0x00000010
TX_TRACE_MUTEX_EVENTS                 0x00000020
TX_TRACE_QUEUE_EVENTS                 0x00000040
TX_TRACE_SEMAPHORE_EVENTS             0x00000080
TX_TRACE_THREAD_EVENTS                0x00000100
TX_TRACE_TIME_EVENTS                  0x00000200
TX_TRACE_TIMER_EVENTS                 0x00000400
FX_TRACE_ALL_EVENTS                   0x00007800
FX_TRACE_INTERNAL_EVENTS              0x00000800
FX_TRACE_MEDIA_EVENTS                 0x00001000
FX_TRACE_DIRECTORY_EVENTS             0x00002000
FX_TRACE_FILE_EVENTS                  0x00004000
NX_TRACE_ALL_EVENTS                   0x00FF8000
NX_TRACE_INTERNAL_EVENTS              0x00008000
NX_TRACE_ARP_EVENTS                   0x00010000
NX_TRACE_ICMP_EVENTS                  0x00020000
NX_TRACE_IGMP_EVENTS                  0x00040000
NX_TRACE_IP_EVENTS                    0x00080000
NX_TRACE_PACKET_EVENTS                0x00100000
NX_TRACE_RARP_EVENTS                  0x00200000
NX_TRACE_TCP_EVENTS                   0x00400000
NX_TRACE_UDP_EVENTS                   0x00800000
UX_TRACE_ALL_EVENTS                   0x7F000000
UX_TRACE_ERRORS                       0x01000000
UX_TRACE_HOST_STACK_EVENTS            0x02000000
UX_TRACE_DEVICE_STACK_EVENTS          0x04000000
UX_TRACE_HOST_CONTROLLER_EVENTS       0x08000000
UX_TRACE_DEVICE_CONTROLLER_EVENTS     0x10000000
UX_TRACE_HOST_CLASS_EVENTS            0x20000000
UX_TRACE_DEVICE_CLASS_EVENTS          0x40000000
```

#### Return Values

- **TX_SUCCESS** (0x00) Successful event filter.
- **TX_FEATURE_NOT_ENABLED** (0xFF) System was not compiled with trace enabled.

#### Allowed From

Initialization and threads

#### Example

```c
/* Filter queue and byte pool events from trace buffer. */

status = tx_trace_event_filter (TX_TRACE_QUEUE_EVENTS | TX_TRACE_BYTE_POOL_EVENTS);

/* If status is TX_SUCCESS all queue and byte pool events are filtered. */
```

#### See Also

tx_trace_enable, tx_trace_event_unfilter, tx_trace_disable, tx_trace_isr_enter_insert, tx_trace_isr_exit_insert,
tx_trace_buffer_full_notify, tx_trace_user_event_insert

### tx_trace_event_unfilter

Unfilter specified events

#### Prototype

```c
UINT tx_trace_event_unfilter (ULONG event_unfilter_bits);
```

#### Description

This service unfilters the specified event(s) such that they will be inserted into the active trace buffer.

> **Important:** The ThreadX library and application must be built with **TX_ENABLE_EVENT_TRACE** defined in order to use event tracing.

#### Input Parameters

- **event_unfilter_bits**: Bits that correspond to events to unfilter. Multiple events may be unfiltered by simply or-ing together the appropriate constants. Valid constants for this variable are defined as follows:

```c
TX_TRACE_ALL_EVENTS                  0x000007FF
TX_TRACE_INTERNAL_EVENTS             0x00000001
TX_TRACE_BLOCK_POOL_EVENTS           0x00000002
TX_TRACE_BYTE_POOL_EVENTS            0x00000004
TX_TRACE_EVENT_FLAGS_EVENTS          0x00000008
TX_TRACE_INTERRUPT_CONTROL_EVENT     0x00000010
TX_TRACE_MUTEX_EVENTS                0x00000020
TX_TRACE_QUEUE_EVENTS                0x00000040
TX_TRACE_SEMAPHORE_EVENTS            0x00000080
TX_TRACE_THREAD_EVENTS               0x00000100
TX_TRACE_TIME_EVENTS                 0x00000200
TX_TRACE_TIMER_EVENTS                0x00000400
FX_TRACE_ALL_EVENTS                  0x00007800
FX_TRACE_INTERNAL_EVENTS             0x00000800
FX_TRACE_MEDIA_EVENTS                0x00001000
FX_TRACE_DIRECTORY_EVENTS            0x00002000
FX_TRACE_FILE_EVENTS                 0x00004000
NX_TRACE_ALL_EVENTS                  0x00FF8000
NX_TRACE_INTERNAL_EVENTS             0x00008000
NX_TRACE_ARP_EVENTS                  0x00010000
NX_TRACE_ICMP_EVENTS                 0x00020000
NX_TRACE_IGMP_EVENTS                 0x00040000
NX_TRACE_IP_EVENTS                   0x00080000
NX_TRACE_PACKET_EVENTS               0x00100000
NX_TRACE_RARP_EVENTS                 0x00200000
NX_TRACE_TCP_EVENTS                  0x00400000
NX_TRACE_UDP_EVENTS                  0x00800000
UX_TRACE_ALL_EVENTS                  0x7F000000
UX_TRACE_ERRORS                      0x01000000
UX_TRACE_HOST_STACK_EVENTS           0x02000000
UX_TRACE_DEVICE_STACK_EVENTS         0x04000000
UX_TRACE_HOST_CONTROLLER_EVENTS      0x08000000
UX_TRACE_DEVICE_CONTROLLER_EVENTS    0x10000000
UX_TRACE_HOST_CLASS_EVENTS           0x20000000
UX_TRACE_DEVICE_CLASS_EVENTS         0x40000000
```

#### Return Values

- **TX_SUCCESS** (0x00) Successful event unfilter.
- **TX_FEATURE_NOT_ENABLED** (0xFF) System was not compiled with trace enabled.

#### Allowed From

Initialization and threads

#### Example

```c
/* Un-filter queue and byte pool events from trace buffer. */ 
status = 
  tx_trace_event_unfilter (TX_TRACE_QUEUE_EVENTS | TX_TRACE_BYTE_POOL_EVENTS);

/* If status is TX_SUCCESS all queue and byte pool events are un-filtered. */
```

#### See Also

tx_trace_enable, tx_trace_event_filter, tx_trace_disable, tx_trace_isr_enter_insert, tx_trace_isr_exit_insert, tx_trace_buffer_full_notify, tx_trace_user_event_insert

### tx_trace_disable

#### Disable event tracing

#### Prototype

```c
UINT tx_trace_disable (VOID);
```

#### Description

This service disables event tracing inside ThreadX. This can be useful if the application wants to freeze the current event trace buffer and possibly transport it externally during run-time. Once disabled, the **tx_trace_enable** can be called to start tracing again.

> **Important:** The ThreadX library and application must be built with **TX_ENABLE_EVENT_TRACE** defined in order to use event tracing.

#### Input Parameters

None.

#### Return Values

- **TX_SUCCESS** (0x00) Successful event trace disable.
- **TX_NOT_DONE** (0x20) Event tracing was not enabled.
- **TX_FEATURE_NOT_ENABLED** (0xFF) System was not compiled with trace enabled.

#### Allowed From

Initialization and threads

#### Example 

```c
/* Disable event tracing. */ 
status = tx_trace_disable ();

/* If status is TX_SUCCESS the event tracing is disabled. */
```

#### See Also

tx_trace_enable, tx_trace_event_filter,
tx_trace_event_unfilter, tx_trace_isr_enter_insert,
tx_trace_isr_exit_insert, tx_trace_buffer_full_notify,
tx_trace_user_event_insert

### tx_trace_isr_enter_insert

#### Insert ISR enter event

#### Prototype

```c
VOID tx_trace_isr_enter_insert (ULONG isr_id);
```

#### Description

This service inserts the ISR enter event into the event trace buffer. It should be called by the application at the beginning of ISR processing. The supplied parameter should identify the specific ISR to the application.

> **Important:** The ThreadX library and application must be built with **TX_ENABLE_EVENT_TRACE** defined in order to use event tracing.

#### Input Parameters 
- **isr_id**: Application specific value to identify the ISR.

#### Return Values

**None**

#### Allowed From 

ISRs

#### Example

```c
/* Insert trace event to identify the application's ISR with an ID of 3. */

status = tx_trace_isr_enter_insert (3);

/* If status is TX_SUCCESS the ISR entry event was inserted. */
```

#### See Also

tx_trace_enable, tx_trace_event_filter,
tx_trace_event_unfilter, tx_trace_disable,
tx_trace_isr_exit_insert, tx_trace_buffer_full_notify,
tx_trace_user_event_insert

### tx_trace_isr_exit_insert

#### Insert ISR exit event

#### Prototype

```c
VOID tx_trace_isr_exit_insert (ULONG isr_id);
```

#### Description

This service inserts the ISR entry event into the event trace buffer. It should be called by the application at the beginning of ISR processing. The supplied parameter should identify the ISR to the application.

> **Important:** The ThreadX library and application must be built with **TX_ENABLE_EVENT_TRACE** defined in order to use event tracing.

#### Input Parameters 

- **isr_id**: Application specific value to identify the ISR.

#### Return Values

**None**

#### Allowed From

ISRs

#### Example

```c
/* Insert trace event to identify the application's ISR with an ID of 3. */ 

status = tx_trace_isr_exit_insert (3);

/* If status is TX_SUCCESS the ISR exit event was inserted. */
```

#### See Also

tx_trace_enable, tx_trace_event_filter, tx_trace_event_unfilter, tx_trace_disable, tx_trace_isr_enter_insert, tx_trace_buffer_full_notify,
tx_trace_user_event_insert

### tx_trace_buffer_full_notify

#### Register trace buffer full application callback

#### Prototype

```c
VOID tx_trace_buffer_full_notify (VOID (*full_buffer_callback)(VOID *));
```

#### Description

This service registers an application callback function that is called by ThreadX when the trace buffer becomes full. The application can then choose to disable tracing and/or possibly setup a new trace buffer.

> **Important:** The ThreadX library and application must be built with **TX_ENABLE_EVENT_TRACE** defined in order to use event tracing.

#### Input Parameters

- **full_buffer_callback**: Application function to call when the trace buffer is full. A value of NULL disables the notification callback.

#### Return Values

**None**

#### Allowed From

ISRs

#### Example

```c
y_trace_is_full(void *trace_buffer_start)

{

     /* Application specific processing goes here! */

}

/* Register the "my_trace_is_full" function to be called whenever the trace buffer fills. */

status = tx_trace_buffer_full_notify (my_trace_is_full);

/* If status is TX_SUCCESS the "my_trace_is_full" function is registered. */
```

#### See Also

tx_trace_enable, tx_trace_event_filter, tx_trace_event_unfilter, tx_trace_disable, tx_trace_isr_enter_insert, tx_trace_isr_exit_insert,
tx_trace_user_event_insert

### tx_trace_user_event_insert

#### Insert user event

#### Prototype

```c
UINT tx_trace_user_event_insert (ULONG event_id, 
   ULONG info_field_1, ULONG info_field_2,
   ULONG info_field_3, ULONG info_field_4);
```

#### Description

This service inserts the user event into the trace buffer. User event IDs must be greater than the constant **TX_TRACE_USER_EVENT_START**, which is defined to be 4096. The maximum user event is defined by the constant **TX_TRACE_USER_EVENT_END**, which is defined to be 65535. All events within this range are available to the application. The information fields are application specific.

> **Important:** The ThreadX library and application must be built with **TX_ENABLE_EVENT_TRACE** defined in order to use event tracing.

#### Input Parameters

- **event_id**: Application-specific event identification and must start be greater than **TX_TRACE_USER_EVENT_START** and less than or equal to **TX_TRACE_USER_EVENT_END**.
- **info_field_1**: Application-specific information field.
- **info_field_2**: Application-specific information field.
- **info_field_3**: Application-specific information field.
- **info_field_4**: Application-specific information field.

#### Return Values
- **TX_SUCCESS** (0x00) Successful user event insert.
- **TX_NOT_DONE** (0x20) Event tracing is not enabled.
- **TX_FEATURE_NOT_ENABLED** (0xFF) The system was not compiled with trace enabled.

#### Allowed From

Initialization and threads

#### Example

```c
/* Insert user event 3000, with info fields of 1, 2, 3, 4. */ 

status = tx_trace_user_event_insert (3000, 1, 2, 3, 4);

/* If status is TX_SUCCESS the user event was inserted. */
```

#### See Also

tx_trace_enable, tx_trace_event_filter,
tx_trace_event_unfilter, tx_trace_disable,
tx_trace_isr_enter_insert, tx_trace_isr_exit_insert,
tx_trace_buffer_full_notify
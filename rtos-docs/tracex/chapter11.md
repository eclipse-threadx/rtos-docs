---
title: Chapter 11 - Format of event trace buffer
description: ThreadX provides built-in event trace support for all ThreadX services, thread state changes, and user-defined events.
---

# Chapter 11 - Format of event trace buffer

ThreadX provides built-in event trace support for all ThreadX services, thread state changes, and user-defined events. To use event trace, simply build the ThreadX, NetX Duo, and FileX libraries with **TX_ENABLE_EVENT_TRACE** defined and enable tracing by calling the ***tx_trace_enable*** function. This chapter describes that process.

## Event Trace Format

The format of the ThreadX event trace buffer is divided into three sections, namely the control header, object registry, and the trace entries. The following describes the general layout of the ThreadX event trace buffer:

**Control Header**

**Object Registry Entry 0**

**…**

**Object Register Entry "n"**

**Event Trace Entry 0**

**…**

**Event Trace Entry "n"**

### Event Trace Control Header

The control header defines the exact layout of the event trace buffer. This includes how many ThreadX objects can be registered as well as how many events can be recorded. In addition, the control header defines where each of the elements of the trace buffer resides. The following data structure defines the control header:

```c
typedef struct TX_TRACE_CONTROL_HEADER_STRUCT
{
    ULONG    tx_trace_control_header_id;
    ULONG    tx_trace_control_header_timer_valid_mask;
    ULONG    tx_trace_control_header_trace_base_address;
    ULONG    tx_trace_control_header_object_registry_start_pointer;
    USHORT   tx_trace_control_header_reserved1;
    USHORT   tx_trace_control_header_object_registry_name_size;
    ULONG    tx_trace_control_header_object_registry_end_pointer;
    ULONG    tx_trace_control_header_buffer_start_pointer;
    ULONG    tx_trace_control_header_buffer_end_pointer;
    ULONG    tx_trace_control_header_buffer_current_pointer;
    ULONG    tx_trace_control_header_reserved2;
    ULONG    tx_trace_control_header_reserved3;
    ULONG    tx_trace_control_header_reserved4;
} TX_TRACE_CONTROL_HEADER;
```

### Control Header ID

The control header ID consists of the 32-bit HEX value of 0x54585442, which corresponds to the ASCII characters ***TXTB***. Since this value is written as a 32-bit unsigned variable, it can also be used to detect the endianness of the event trace buffer. For example, if the value in the first four byes of memory is 0x54, 0x58, 0x54, 0x42, the event trace buffer was written in big endian format. Otherwise, the event trace buffer was written in little endian format.

### Timer Valid Mask

The timer valid mask defines how many bits of the timestamp in the actual event trace entries are valid. For example, if the time-stamp source has 16-bits, the value in this field should be 0xFFFF. A 32-bit time-stamp source would have a value of 0xFFFFFFFF. This value is defined by the ***TX_TRACE_TIME_MASK*** constant in ***tx_port.h***.

### Trace Base Address

The trace buffer base address is the address the application specified as the start of the trace buffer in the ***tx_trace_enable*** call. This address is maintained for the sole use of the analysis tool to derive bufferrelative offsets for the various elements in the buffer. For example, the buffer relative offset of the current event in the trace buffer is calculated by simple subtraction of the base address from the current event address.

### Registry Start and End Pointers

The registry start pointer points to the address of the first object registry entry, while the registry end pointer points to the address im../mediately following the last register entry. These values are setup during the *tx_trace_enable* processing and are not changed throughout the duration of tracing.

### Registry Name Size

The registry name size defines maximum size in bytes for each object name in the registry entry and is defined by the symbol
***TX_TRACE_OBJECT_REGISTRY_NAME***. The default value is 32 and is defined in ***tx_trace.h***. The object name corresponds to the name given by the application when the object was created. For example, the object registry name for a thread is the name supplied by the application to the ***tx_thread_create***call.

### Buffer Start and End Pointers

The event trace buffer start pointer points to the address of the first trace entry, while the registry end pointer points to the address im../mediately following the last trace entry. These values are setup during the ***tx_trace_enable</*** processing and are not changed throughout the duration of tracing.

### Current Buffer Pointer

The event trace buffer current pointer points to the address of the oldest trace entry. Since the trace entries are maintained in a circular list, the current buffer pointer is also represents the next trace entry to be written. |

## Event Trace Object Registry

The event trace object registry contains ***n*** object registry entries that correspond to the objects created by the application. The main purpose of the object registry is for external analysis tools to correlate actual object names with the object addresses of the trace buffer entries. The number of registry entries is specified by the application in the ***tx_trace_enable*** call.

Each object register entry contains information about a specific ThreadX object previously created by the application. The following data structure defines each object registry entry:

```c
typedef struct TX_TRACE_OBJECT_REGISTRY_ENTRY_STRUCT
{
    UCHAR    tx_trace_object_registry_entry_object_available**;
    UCHAR    tx_trace_object_registry_entry_object_type**;
    UCHAR    tx_trace_object_registry_entry_object_reserved1;
    UCHAR    tx_trace_object_registry_entry_object_reserved2;
    ULONG    tx_trace_thread_registry_entry_object_pointer;
    ULONG    tx_trace_object_registry_entry_object_parameter_1;
    ULONG    tx_trace_object_registry_entry_object_parameter_2;
    UCHAR    tx_trace_thread_registry_entry_object_name[TX_TRACE_OBJECT_REGISTRY_NAME];

} TX_TRACE_OBJECT_REGISTRY_ENTRY;
```

### Object Available Flag

The object available flag is set to 1 if the object registry entry is available. Otherwise, if the value is
not 1, the object registry entry is not available. Note that the entry could still contain valid information even though it is available.

### Object Entry Type

The object entry type identifies the type of object in this entry. The following is a list of the valid object types:

| **Value**	| **Object Type** |
|---------- | --------------- |
| 0         | Not Valid       |
| 1         | Thread          |
| 2         | Timer |
| 3         | Queue |
| 4         | Semaphore |
| 5         | Mutex |
| 6         | Event Flags Group |
| 7         | Block Pool |
| 8         | Byte Pool |
| 9         | ../media |
| 10        | File |
| 11        | IP |
| 12        | Packet Pool |
| 13        | TCP Socket |
| 14        | UDP Socket |
| 15-20     | Reserved |  
| 21        | USB Host Stack Device |
| 22        | USB Host Stack Interface |
| 23        | USB Host Endpoint |
| 24        | USB Host Class |
| 25        | USB Device |
| 26        | USB Device Interface |
| 27        | USB Device Endpoint |
| 28        | USB Device Class |

### Object Pointer

The object pointer specifies the object address that is used for accessing the object using the ThreadX API.

### Object Reserved Fields

For all objects other than threads, these reserved fields should be 0. For threads, the priority of the thread at the time it is entered into the registry is placed in these two reserved fields.

### Object Parameters

The object parameters contain supplemental information about the object. The following describes the supplemental information for each ThreadX object:

| **Object Type**        | **Parameter 1**  | **Parameter 2** |
|----------------------- | ---------------- | --------------- |
| Thread                 | Stack Start      | Stack Size |
| Timer                  | Initial Ticks | Reschedule Ticks |
| Queue                  | Queue Size | Message Size |
| Semaphore              | Initial Instances | - |
| Mutex                  | Inheritance Flag | - |
| Event Flags Group      | - | - |
| Block Pool             | Total Blocks | Block Size |
| Byte Pool              | Total Bytes | - |
| ../media                  | Fat Cache Size | Sector Cache Size |
| File                   | - | - |
| IP                     | Stack Start | Stack Size |
| Packet Pool            | Packet Size | Number of Packets |
| TCP Socket             | IP address | Window Size |
| UDP Socket             | IP address | RX Queue Max |

### Object Name

The object name contains the name of the ThreadX object. The name is the name provided to ThreadX at the time the object was created. By default, the object name has a maximum of 32 characters. Actual names greater than 32 characters are truncated.

## Event Trace Entries

The event trace entries are found in the bottom portion of the event trace buffer. The entries are maintained in a circular list, with the current entry pointer pointing to the oldest entry. The number of entries in the list is calculated by the ***tx_trace_enable*** call.

Each object register entry contains information about a specific ThreadX trace event. The following data structure defines each trace event entry:

```c
typedef struct TX_TRACE_BUFFER_ENTRY_STRUCT
{
    ULONG     tx_trace_buffer_entry_thread_pointer;
    ULONG     tx_trace_buffer_entry_thread_priority;
    ULONG     tx_trace_buffer_entry_event_id;
    ULONG     tx_trace_buffer_entry_time_stamp;
    ULONG     tx_trace_buffer_entry_information_field_1;
    ULONG     tx_trace_buffer_entry_information_field_2;
    ULONG     tx_trace_buffer_entry_information_field_3;
    ULONG     tx_trace_buffer_entry_information_field_4;
} TX_TRACE_BUFFER_ENTRY;
```

### Thread Pointer

The thread pointer contains the address of the thread running at the time of this event. If the event occurred during
initialization (no thread running), the value of this pointer is 0xF0F0F0F0. If the event occurred during an Interrupt Service Routine (ISR), the value of this pointer is 0xFFFFFFFF. If the entry has not yet been used, the value of this pointer is 0.

### Thread Priority

The thread priority field contains the thread priority and preemption-threshold of the thread that was running at the time of this event. If an interrupt context is present (thread pointer is 0xFFFFFFFF), the value of this field is not the priority but instead the value of ***_tx_thread_current_ptr*** at the time of the event. Otherwise, the value of this field is 0.

### Event ID

The event ID specifies the event that took place. Valid ThreadX trace event IDs range from 1 through 1024. Values starting at 1025 and above are reserved for user-specific events. Please refer to the ***tx_trace.h*** file for the complete definition of ThreadX event IDs.</td>

### Information Fields (1-4)

The information fields contain additional information about the specific event. Please refer to the ***tx_trace.h*** file for the complete description of the information fields for each of the defined ThreadX event IDs.
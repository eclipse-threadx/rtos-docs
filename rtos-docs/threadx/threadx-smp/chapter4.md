---
title: Chapter 4 - Description of ThreadX SMP Services
description: This chapter contains a description of all ThreadX SMP services in alphabetic order.
---

# Chapter 4 - Description of ThreadX SMP Services

This chapter contains a description of all ThreadX SMP services in alphabetic order. Their names are designed so all similar services are grouped together. In the "Return Values" section in the following descriptions, values in **BOLD** are not affected by the **TX_DISABLE_ERROR_CHECKING** define used to disable API error checking; while values shown in nonbold are completely disabled. In addition, a "**Yes**" listed under the "**Preemption Possible**" heading indicates that calling the service may resume a higher-priority thread, thus preempting the calling thread.

The Threadx SMP API functions available to the application are as follows.
## Thread_SMP_Services
- [tx_thread_smp_core_exclude](#tx_thread_smp_core_exclude)
- [tx_thread_smp_core_exclude_get](#tx_thread_smp_core_exclude_get)
- [tx_thread_smp_core_get](#tx_thread_smp_core_get)

## Timer_SMP_Services
- [tx_timer_smp_core_exclude](#tx_timer_smp_core_exclude)
- [tx_timer_smp_core_exclude_get](#tx_timer_smp_core_exclude_get)

## tx_thread_smp_core_exclude

Exclude thread execution on a set of cores

### Prototype

```C
UINT  tx_thread_smp_core_exclude(TX_THREAD *thread_ptr,
                          ULONG exclusion_map);
```
### Description

This function excludes the specified thread from executing on the core(s) specified in the bit map called "*exclusion_map*." Each bit in "*exclusion_map*" represents a core (bit 0 represents core 0, etc.). If the bit is set, the corresponding core is excluded from executing the specified thread.

> **Important:** Use of processor exclusion may cause additional processing in the thread to core mapping logic in order to find the optimal match. This processing is bounded by the number of ready threads.

### Parameters 

- **thread_ptr**: Pointer to thread to change the core exclusion.
- **exclusion_map**: Bit map where a sit bit indicates that that core is excluded. Supplying a 0 value enables the thread to execute on any core (default).

### Return Values

- **TX_SUCCESS**: (0x00) Successful core exclusion.
- **TX_THREAD_ERROR**: (0x0E) Invalid thread pointer.

### Allowed From

Initialization, ISRs, threads, and timers

### Example

```C
/* Exclude core 0 for "Thread 0". */
tx_thread_smp_core_exclude(&thread_0, 0x01);
```

### See Also

- [Thread SMP Services](#Thread_SMP_Services)

## tx_thread_smp_core_exclude_get

Gets the thread's current core exclusion

### Prototype

```C
UINT tx_thread_smp_core_exclude_get(TX_THREAD *thread_ptr,
                         ULONG *exclusion_map_ptr);
```
### Description

This function returns the current core exclusion list.

### Parameters

- **thread_ptr**: Pointer to thread from which to retrieve the core exclusion.
- **exclusion_map_ptr**: Destination for current core exclusion bit map.

### Return Values

- **TX_SUCCESS**: (0x00) Successful retrieval of thread's core exclusion.
- **TX_THREAD_ERROR**: (0x0E) Invalid thread pointer.
- **TX_PTR_ERROR**: (0x03) Invalid exclusion destination pointer.

### Allowed From

Initialization, ISRs, threads, and timers

### Example

```C
ULONG excluded_cores;
/* Retrieve the core exclusion for "Thread 0". */
tx_thread_smp_core_exclude_get(&thread_0, &excluded_cores);
```

### See Also

- [Thread SMP Services](#Thread_SMP_Services)

## tx_thread_smp_core_get

Retrieve currently executing core of caller

### Prototype

```C
UINT  tx_thread_smp_core_get(void);
```
### Description

This function returns the core ID of the core executing this service.

### Parameters

None

### Return Values

- core_id: ID of currently executing core, (0 through TX_THREAD_SMP_MAX_CORES-1)

### Allowed From

Initialization, ISRs, threads, and timers

### Example

```C
UINT core;
/* Pickup the currently executing core. */
core = tx_thread_smp_core_get();

/* At this point, "core" contains the executing core ID. */
```
### See Also

- [Thread SMP Services](#Thread_SMP_Services)

## tx_timer_smp_core_exclude

Exclude timer execution on a set of cores

### Prototype

```C
UINT  tx_timer_smp_core_exclude(TX_TIMER *timer_ptr, ULONG exclusion_map);
```
### Description

This function excludes the specified timer from executing on the core(s) specified in the bit map called "*exclusion_map*." Each bit in "*exclusion_map*" represents a core (bit 0 represents core 0, etc.). If the bit is set, the corresponding core is excluded from executing the specified timer.

> **Important:** Use of processor exclusion may cause additional processing in the thread to core mapping logic in order to find the optimal match. This processing is bounded by the number of ready threads.

### Parameters

- **timer_ptr**: Pointer to timer to change the core exclusion.
- **exclusion_map**: Bit map where a sit bit indicates that that core is excluded. Supplying a 0 value enables the timer to execute on any core (default).

### Return Values

- **TX_SUCCESS** (0x00) Successful core exclusion.
- **TX_TIMER_ERROR** (0x0E) Invalid timer pointer.

### Allowed From

Initialization, ISRs, threads, and timers

### Example

```C
/* Exclude core 0 for "Timer 0". */
tx_timer_smp_core_exclude(&timer_0, 0x01);
```

### See Also

- [Timer SMP Services](#Timer_SMP_Services)

## tx_timer_smp_core_exclude_get

Gets the timer's current core exclusion

### Prototype

```C
UINT tx_timer_smp_core_exclude_get(TX_TIMER *timer_ptr,
                         ULONG *exclusion_map_ptr);
```
### Description

This function returns the current core exclusion list.

### Parameters

- **timer_ptr**: Pointer to timer from which to retrieve the core exclusion.
- **exclusion_map_ptr**: Destination for current core exclusion bit map.

### Return Values

- **TX_SUCCESS**: (0x00) Successful retrieval of timer's core exclusion.
- **TX_TIMER_ERROR**: (0x0E) Invalid timer pointer.
- **TX_PTR_ERROR**: (0x03) Invalid exclusion destination pointer.

### Allowed From

Initialization, ISRs, threads, and timers

### Example

```C
ULONG excluded_cores;

/* Retrieve the core exclusion for "Timer 0". */
tx_timer_smp_core_exclude_get(&timer_0, &excluded_cores);
```
### See Also

- [Timer SMP Services](#Timer_SMP_Services)
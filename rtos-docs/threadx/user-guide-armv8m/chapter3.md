---
title: Chapter 3 - ThreadX APIs for ARMv8-M
description: This chapter is a description of the ARMv8-M-specific ThreadX services.
---

# Chapter 3  ThreadX APIs for ARMv8-M

This chapter contains a description of the ARMv8-M-specific ThreadX services in alphabetic order. Their names are designed so all similar services are grouped together. In the "Return Values" section in the following descriptions, values in **BOLD** are not affected by the **TX_DISABLE_ERROR_CHECKING** define used to disable API error checking; while values shown in non-bold are completely disabled.

- [tx_thread_secure_stack_allocate](#tx_thread_secure_stack_allocate) Allocate a thread stack in secure memory.
- [tx_thread_secure_stack_free](#tx_thread_secure_stack_free) Free thread stack in secure memory

## tx_thread_secure_stack_allocate

Allocate a thread stack in secure memory.

### Prototype

```c
UINT tx_thread_secure_stack_allocate(
    TX_THREAD *thread_ptr, 
    ULONG stack_size);
```

### Description

This service allocates a stack of size stack_size bytes in secure memory. This function should be called for every thread that calls secure functions.

### Input Parameters

- **thread_ptr** Pointer to previously created thread.

- **stack_size** Size of secure stack.

### Return Values

- **TX_SUCCESS** (0x00) Successful request.

- **TX_SIZE_ERROR** (0x05) Stack size out of range.

- **TX_THREAD_ERROR** (0x0E) Invalid thread pointer.

- **TX_NO_MEMORY** (0x10) Unable to allocate memory.

- **TX_CALLER_ERROR** (0x13) Invalid caller of this service.

- **TX_FEATURE_NOT_ENABLED** (0xFF) The system was compiled to run in secure mode.

### Allowed From

Initialization, threads

### Example

```c
/* Create thread.  */
tx_thread_create(&thread_0, "thread 0", thread_0_entry, 0, pointer, DEMO_STACK_SIZE, 1, 1, TX_NO_TIME_SLICE, TX_AUTO_START);

/* Allocate secure stack so this thread can call secure functions.  */
status = tx_thread_secure_stack_allocate(&thread_0, 256);

/* If status is TX_SUCCESS the request was successful.  */
```

### See Also

tx_thread_secure_stack_free

##  tx_thread_secure_stack_free

Free a thread stack in secure memory. 

### Prototype

```c
UINT tx_thread_secure_stack_free(TX_THREAD *thread_ptr);
```

### Description

This service frees a thread's secure stack in secure memory. This function should be called if a thread has a secure stack and when the thread no longer needs to call secure functions or the thread is deleted.

### Input Parameters

- **thread_ptr** Pointer to previously created thread.

### Return Values

- **TX_SUCCESS** (0x00) Successful request.

- **TX_THREAD_ERROR** (0x0E) Invalid thread pointer.

- **TX_CALLER_ERROR** (0x13) Invalid caller of this service.

- **TX_FEATURE_NOT_ENABLED** (0xFF) The system was compiled to run in secure mode.

### Allowed From

Initialization, threads

### Example

```c
/* Free thread's secure stack.  */
status = tx_thread_secure_stack_free(&thread_0);

/* If status is TX_SUCCESS the request was successful.  */

/* Delete thread.  */
tx_thread_delete(&thread_0);
```

## See Also

tx_thread_secure_stack_allocate

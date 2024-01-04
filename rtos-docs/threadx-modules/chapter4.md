---
title: Chapter 4 - Module APIs
description: This article is a summary of the additional APIs available to a module.
---

# Chapter 4 - Module APIs

## Summary of Module APIs

There are several additional API functions available to a module, as follows:

- ***txm_module_application_request*** - *Application-specific request to resident code*
- ***txm_module_object_allocate*** - *Allocate memory outside of module for object*
- ***txm_module_object_deallocate*** - *Deallocate previously allocated object memory*
- ***txm_module_object_pointer_get*** - *Find system object and retrieve object pointer*
- ***txm_module_object_pointer_get_extended*** - *Find system object and retrieve object pointer, name length safety*

## Return values

Additional error codes are returned for some ThreadX APIs. These additional error codes are defined as follows:

- **TXM_MODULE_INVALID_PROPERTIES** (0xF3): Indicates the module does not have the correct properties to make an API call. For example, calling trace APIs in user mode.
- **TXM_MODULE_INVALID_MEMORY** (0xF4): Indicates the memory supplied by the module is invalid or is in an invalid location. For example, in memory protected modules, object control blocks are not allowed to be located in memory the module can access.
- **TXM_MODULE_INVALID_CALLBACK** (0xF5): Callback specified in the API is outside the range of the module's code and is therefore invalid.

---

## txm_module_application_request

Application-specific request to resident code.

### Prototype

```c
UINT txm_module_application_request(
    ULONG request, 
    ULONG param_1,
    ULONG param_2,
    ULONG param_3);
```

### Description

This service makes the specified request to the resident portion of the application. It is assumed that the request structure is prepared prior to the call. The actual processing of the request takes place in the resident code in the function ***_txm_module_manager_application_request***. By default, this function is left empty and is designed for the resident application developer to modify.

### Input parameters

- **request** Request ID (application defined)
- **param_1** First parameter
- **param_2** Second parameter
- **param_3** Third parameter

### Return values

- **TX_SUCCESS** (0x00) Successful request.
- **TX_NOT_AVAILABLE** (0x1D) Request not supported by resident code.

### Allowed from

Module threads

### Example

```c
/* Call application resident code with ID=77 and the
   parameters set to 1, 2, 3. */
status = txm_module_application_request(77, 1, 2, 3);

/* If status is TX_SUCCESS the request was successful. */
```

---

## txm_module_object_allocate

Allocate memory in the object pool (created by the resident application) for a module object control block.

### Prototype

```c
UINT txm_module_object_allocate(
   VOID **object_ptr, 
   ULONG object_size);
```

### Description

This service allocates memory for a module object from memory outside of the module, which helps prevent corruption of the object control block by the module's code. In memory protected systems, all object control blocks must be allocated with this API before they can be created.

### Input parameters

- **object_ptr** Destination of object pointer on successful allocation.
- **object_size** Size in bytes of the object to be allocated.

### Return values

- **TX_SUCCESS** (0x00) Successful object allocate.
- **TX_NO_MEMORY** (0x10) Not enough memory.
- **TX_NOT_AVAILABLE** (0x1D) Module manager has not created an object pool to allocate from

### Allowed from

Module threads

### Example

```c
TX_QUEUE *queue_pointer;

/* Allocate a control block for a module message queue. */
status = txm_module_object_allocate(&queue_pointer, sizeof(TX_QUEUE));

/* If status is TX_SUCCESS the queue_pointer points to
   memory allocated outside of the module and can be supplied
   to tx_queue_create to create a queue for the module. */
```

### See also

- txm_module_object_deallocate
- txm_module_object_pointer_get

---

## txm_module_object_deallocate

Deallocate previously allocated object memory

### Prototype

```c
UINT txm_module_object_deallocate(VOID *object_ptr);
```

### Description

***This service has been deprecated because it is no longer needed***.

The memory that was previously allocated via ***txm_module_object_allocate*** is deallocated in the ***tx_\*_delete*** service.

### Input parameters

- **object_ptr** Object pointer to deallocate.

### Return values

- **TX_SUCCESS** (0x00) Successful object allocate.

### Allowed from

Module threads

### Example

```c
TX_QUEUE *queue_pointer;

/* Deallocate control block for a module message queue. */
status = txm_module_object_deallocate(queue_pointer);

/* If status is TX_SUCCESS the object memory associated
   with queue_pointer is deallocated. */
```

### See also

- txm_module_object_allocate
- txm_module_object_pointer_get

---

## txm_module_object_pointer_get

Find system object and retrieve object pointer

### Prototype

```c
UINT txm_module_object_pointer_get(
   UINT object_type, CHAR *name, 
   VOID **object_ptr);
```

### Description

This service retrieves the object pointer of a particular type with a particular name. If the object is not found, an error is returned. Otherwise, if the object is found, the address of that object is placed in "object_ptr." This pointer can then be used to make system service calls, to interact with the resident code, and/or other loaded modules in the system.

### Input parameters

- **object_type** Type of ThreadX object requested. Valid types are as follows:
  - TXM_BLOCK_POOL_OBJECT
  - TXM_BYTE_POOL_OBJECT
  - TXM_EVENT_FLAGS_OBJECT
  - TXM_MUTEX_OBJECT
  - TXM_QUEUE_OBJECT
  - TXM_SEMAPHORE_OBJECT
  - TXM_THREAD_OBJECT
  - TXM_TIMER_OBJECT
  - TXM_IP_OBJECT
  - TXM_PACKET_POOL_OBJECT
  - TXM_UDP_SOCKET_OBJECT
  - TXM_TCP_SOCKET_OBJECT
- **name** Application-specific object name as defined when the object was created.
- **object_ptr** Destination for object pointer.

### Return values

- **TX_SUCCESS** (0x00) Successful object get.
- **TX_OPTION_ERROR** (0x08) Invalid object type.
- **TX_PTR_ERROR** (0x03) Invalid destination.
- **TX_SIZE_ERROR** (0x05) Invalid size.
- **TX_NO_INSTANCE** (0x0D) Object not found.

### Allowed from

Module threads

### Example

```c
TX_QUEUE *queue_pointer;

/* Find the pointer for "fft_queue" in the resident part
   of the application. */
status = txm_module_object_pointer_get(TXM_QUEUE_OBJECT,
         "fft_queue", &queue_pointer);

/* If status is TX_SUCCESS the found queue pointer is in
   "queue_pointer". This queue pointer can then be used to
   send messages to the "fft_queue." */
```

### See also

- txm_module_manager_object_pointer_get_extended

---

## txm_module_object_pointer_get_extended

Find system object and retrieve object pointer

### Prototype

```c
UINT txm_module_object_pointer_get_extended(UINT object_type,
                                            CHAR *name,
                                            UINT name_length,
                                            VOID **object_ptr);
```

### Description

This service retrieves the object pointer of a particular type with a particular name. If the object is not found, an error is returned. Otherwise, if the object is found, the address of that object is placed in "object_ptr." This pointer can then be used to make system service calls, to interact with the resident code, and/or other loaded modules in the system.

### Input parameters

- **object_type** Type of ThreadX object requested. Valid types are as follows:
  - TXM_BLOCK_POOL_OBJECT
  - TXM_BYTE_POOL_OBJECT
  - TXM_EVENT_FLAGS_OBJECT
  - TXM_MUTEX_OBJECT
  - TXM_QUEUE_OBJECT
  - TXM_SEMAPHORE_OBJECT
  - TXM_THREAD_OBJECT
  - TXM_TIMER_OBJECT
  - TXM_IP_OBJECT
  - TXM_PACKET_POOL_OBJECT
  - TXM_UDP_SOCKET_OBJECT
  - TXM_TCP_SOCKET_OBJECT
- **name** Application-specific object name as defined when the object was created.
- **name_length** Length of name.
- **object_ptr** Destination for object pointer.

### Return values

- **TX_SUCCESS** (0x00) Successful object get.
- **TX_OPTION_ERROR** (0x08) Invalid object type.
- **TX_PTR_ERROR** (0x03) Invalid destination.
- **TX_SIZE_ERROR** (0x05) Invalid size.
- **TX_NO_INSTANCE** (0x0D) Object not found.

### Allowed from

Module threads

### Example

```c
TX_QUEUE *queue_pointer;

/* Find the pointer for "fft_queue" in the resident part
   of the application. */
   status = txm_module_object_pointer_get_extended(TXM_QUEUE_OBJECT,
            "fft_queue", 9, &queue_pointer);

/* If status is TX_SUCCESS the found queue pointer is in
   "queue_pointer". This queue pointer can then be used to
   send messages to the "fft_queue." */
```

### See also

- txm_module_manager_object_pointer_get

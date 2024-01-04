---
title: Chapter 5 - Module Manager APIs
description: This article is a summary of the Module Manager APIs available to the resident portion of the application.
---

# Chapter 5 - Module Manager APIs

## Summary of Module Manager APIs

There are several additional APIs available to the resident portion of the application, as follows.

- ***txm_module_manager_absolute_load*** - *Module's code and data are at absolute (fixed) addresses*
- ***txm_module_manager_external_memory_enable*** - *Enable module access to a shared memory space*
- ***txm_module_manager_file_load*** - *Load module from file via FileX*
- ***txm_module_manager_in_place_load*** - *Load module data, execute in place*
- ***txm_module_manager_initialize*** - *Initialize the module manager*
- ***txm_module_manager_mm_initialize*** - *Initialize the memory management hardware*
- ***txm_module_manager_maximum_module_priority_set*** - *Set the maximum thread priority allowed in a module*
- ***txm_module_manager_memory_fault_notify*** - *Register an application callback on memory fault*
- ***txm_module_manager_memory_load*** - *Load the module from memory*
- ***txm_module_manager_object_pool_create*** - *Create an object pool for modules*
- ***txm_module_manager_properties_get*** - *Get module properties*
- ***txm_module_manager_start*** - *Start execution of the specified module*
- ***txm_module_manager_stop*** - *Stop execution of the specified module*
- ***txm_module_manager_unload*** - *Unload the module*

---

## txm_module_manager_absolute_load

Module's code and data are at absolute (fixed) addresses.

### Prototype

```c
UINT txm_module_manager_absolute_load(
    TXM_MODULE_INSTANCE *module_instance,
    CHAR *module_name,
    VOID *module_location);
```

### Description

This service initializes the module contained in the specified location and prepares it for execution. There is no code or data relocation, thus position-independence is not necessary.

### Input parameters

- **module_instance** Pointer to the instance of the module.
- **module_name** Name of the module.
- **module_location** Pointer to module's code area, preamble first.

### Return values

- **TX_SUCCESS** (0x00) Successful module load.
- **TX_CALLER_ERROR** (0x13) Invalid caller.
- **TX_NOT_AVAILABLE** (0x1D) Manager not initialized.
- **TX_NO_MEMORY** (0x10) Not enough memory to load module.
- **TX_PTR_ERROR** (0x03) Invalid pointer, module instance, or module preamble.
- **TXM_MODULE_ALIGNMENT_ERROR** (0xF0) Invalid alignment.
- **TXM_MODULE_ALREADY_LOADED** (0xF1) Module already loaded.
- **TXM_MODULE_INVALID** (0xF2) Invalid module preamble.
- **TXM_MODULE_INVALID_PROPERTIES** (0xF3) Incompatible properties.

### Allowed from

Initialization and threads

### Example

```c
TXM_MODULE_INSTANCE my_module;

/* Initialize the module manager. */
status = txm_module_manager_initialize((VOID*)0x64010000,0x10000);

/* Load the module that has its code area at address 0x080F0000. */
txm_module_manager_absolute_load(&my_module, "my module", (VOID *) 0x080F0000);

/* Start the module. */
status = txm_module_manager_start(&my_module);
```

### See also

- txm_module_manager_file_load
- txm_module_manager_in_place_load
- txm_module_manager_memory_load
- txm_module_manager_unload

---

## txm_module_manager_external_memory_enable

Enable module to access a shared memory space.

### Prototype

```c
UINT txm_module_manager_external_memory_enable(
     TXM_MODULE_INSTANCE *module_instance,
     VOID *start_address,
     ULONG length,
     UINT attributes);
```

### Description

This service creates an entry in the memory management hardware table for a shared memory region that the module can access.

### Input parameters

- **module_instance** Pointer to the instance of the module.
- **start_address** Starting address of shared memory region.
- **length** Length of shared memory region.
- **attributes** Attributes of memory region (cache, read, write, etc.). Attributes are port-specific; see [appendix](appendix.md) for attributes format.

### Return values

- **TX_SUCCESS** (0x00) Memory entry created successfully.
- **TX_NOT_AVAILABLE** (0x1D) Manager not initialized or feature not available.
- **TX_PTR_ERROR** (0x03) Invalid module instance.
- **TX_START_ERROR** (0x10) Module not in loaded state.
- **TXM_MODULE_ALIGNMENT_ERROR** (0xF0) Invalid start address alignment.
- **TXM_MODULE_INVALID_PROPERTIES** (0xF3) Incompatible properties.

### Allowed from

Initialization and threads

### Example

```c
TXM_MODULE_INSTANCE my_module;

/* Initialize the module manager with 64KB of RAM starting at address 0x64010000. */
txm_module_manager_initialize((VOID *) 0x64010000, 0x10000);

/* Load the module that has its code area at address 0x080F0000. */
txm_module_manager_in_place_load(&my_module, "my module", (VOID *) 0x080F0000);

/* Create a shared memory space 256 bytes long at address 0x64005000
   with read & write, no execute, outer & inner write back cache
   attributes. Note that these attributes are port-specific. */
txm_module_manager_external_memory_enable(&my_module, (VOID*)0x64005000, 256, 0x3F);
```

---

## txm_module_manager_file_load

Load module from file via FileX.

### Prototype

```c
UINT txm_module_manager_file_load(
    TXM_MODULE_INSTANCE *module_instance,
    CHAR *module_name,
    FX_MEDIA *media_ptr,
    CHAR *file_name);
```

### Description

This service loads the binary image of the module contained in the specified file into the module memory area and prepares it for execution. It is assumed that the supplied media is already opened.

> **Note:** The FileX system is utilized to load the file. In order to enable FileX access, the module, module library, Module Manager and the ThreadX library (with the Module Manager sources) must be built with **FX_FILEX_PRESENT** defined in the projects.

### Input parameters

- **module_instance** Pointer to the instance of the module.
- **module_name** Name of the module.
- **media_ptr** Pointer to already opened FileX media.
- **file_name** Name of module's binary file.

### Return values

- **TX_SUCCESS** (0x00) Successful module load.
- **TX_CALLER_ERROR** (0x13) Invalid caller.
- **TX_NOT_AVAILABLE** (0x1D) Manager not initialized.
- **TX_NO_MEMORY** (0x10) Not enough memory to load module.
- **TX_NOT_DONE** (0x20) Media not open, file not found or file is invalid.
- **TX_PTR_ERROR** (0x03) Invalid module pointer.
- **TXM_MODULE_ALIGNMENT_ERROR** (0xF0) Invalid alignment.
- **TXM_MODULE_ALREADY_LOADED** (0xF1) Module already loaded.
- **TXM_MODULE_INVALID** (0xF2) | Invalid module preamble.
- **TXM_MODULE_INVALID_PROPERTIES** (0xF3) Incompatible properties.

### Allowed from

Threads

### Example

```c
TXM_MODULE_INSTANCE my_module;

/* Initialize the module manager. */
status = txm_module_manager_initialize((VOID*)0x64010000,0x10000);

/* Load the module from a binary file. */
status = txm_module_manager_file_load(&my_module, "my module",
                                      &sdio_disk, "demo_thread_module.bin");

/* Start the module. */
status = txm_module_manager_start(&my_module);
```

### See also

- txm_module_manager_absolute_load
- txm_module_manager_in_place_load
- txm_module_manager_memory_load
- txm_module_manager_unload

---

## txm_module_manager_in_place_load

Load module data only, execute module in existing location.

### Prototype

```c
UINT txm_module_manager_in_place_load(
    TXM_MODULE_INSTANCE *module_instance,
    CHAR *module_name,
    VOID *location);
```

### Description

This service loads the module's data area only into the module memory area and prepares it for execution. Module code execution will be in-place, that is, from the address offset specified by the module preamble at the supplied location.

### Input parameters

- **module_instance** Pointer to the instance of the module.
- **module_name** Name of the module.
- **location** Pointer to module's code area, preamble first.

### Return values

- **TX_SUCCESS** (0x00) Successful module load.
- **TX_CALLER_ERROR** (0x13) Invalid caller.
- **TX_NOT_AVAILABLE** (0x1D) Manager not initialized.
- **TX_NO_MEMORY** (0x10) Not enough memory to load module.
- **TX_PTR_ERROR** (0x03) Invalid pointer, module instance, or module preamble.
- **TXM_MODULE_ALIGNMENT_ERROR** (0xF0) Invalid alignment.
- **TXM_MODULE_ALREADY_LOADED** (0xF1) Module already loaded.
- **TXM_MODULE_INVALID** (0xF2) Invalid module preamble.
- **TXM_MODULE_INVALID_PROPERTIES** (0xF3) Incompatible properties.

### Allowed from

Initialization and threads

### Example

```c
TXM_MODULE_INSTANCE my_module;

/* Initialize the module manager with 64KB of RAM starting at address 0x64010000. */
txm_module_manager_initialize((VOID *) 0x64010000, 0x10000);

/* Load the module that has its code area at address 0x080F0000. */
txm_module_manager_in_place_load(&my_module, "my module", (VOID *) 0x080F0000);

/* Start the module. */
txm_module_manager_start(&my_module);
```

### See also

- txm_module_manager_absolute_load
- txm_module_manager_file_load
- txm_module_manager_memory_load
- txm_module_manager_unload

---

## txm_module_manager_initialize

Initialize the module manager.

### Prototype

```c
UINT txm_module_manager_initialize(
    VOID *module_memory_start,
    ULONG module_memory_size);
```

### Description

This service initializes the Module Manager's internal resources, including the memory area used for loading modules.

### Input parameters

- **module_memory_start** Pointer to the start of module memory.
- **module_memory_size** Size in bytes of the module memory.

### Return values

- **TX_SUCCESS** (0x00) Successful initialization.
- **TX_CALLER_ERROR** (0x13) Invalid caller.

### Allowed from

Initialization and threads

### Example

```c
/* Initialize the module manager with 64KB of RAM starting at
   address 0x64010000. */
txm_module_manager_initialize((VOID *) 0x64010000, 0x10000);
```

### See also

- txm_module_manager_object_pool_create

---

## txm_module_manager_initialize_mmu

Initialize the memory management hardware.

### Prototype

```c
UINT txm_module_manager_initialize_mmu(VOID);
```

### Description

This service initializes the MMU.

### Input parameters

none

### Return values

- **TX_SUCCESS** (0x00) Successful initialization.
- **TX_CALLER_ERROR** (0x13) Invalid caller.

### Allowed from

Initialization and Threads

### Example

```c
/* Initialize the module manager with 64KB of RAM starting at address 0x64010000. */
txm_module_manager_initialize((VOID *) 0x64010000, 0x10000);

/* Initialize the MMU. */
txm_module_manager_initialize_mmu();
```

---

## txm_module_manager_maximum_module_priority_set

Set the maximum thread priority allowed in a module.

### Prototype

```c
UINT txm_module_manager_maximum_module_priority_set(
         TXM_MODULE_INSTANCE *module_instance,
         UINT priority);
```

### Description

This service sets the maximum thread priority allowed in a module.

### Input parameters

- **module_instance** Pointer to the instance of the module.
- **priority** Maximum thread priority.

### Return values

- **TX_SUCCESS** (0x00) Successful initialization.
- **TX_NOT_AVAILABLE** (0x1D) Manager not initialized.
- **TX_PTR_ERROR** (0x03) Invalid module instance.
- **TX_START_ERROR** (0x10) Module not in loaded state.

### Allowed from

Initialization and threads

### Example

```c
/* Load the module that has its code area at address 0x080F0000. */
txm_module_manager_in_place_load(&my_module, "my module", (VOID *) 0x080F0000);

/* Set the maximum thread priority in my_module to 5. */
txm_module_manager_maximum_module_priority_set(&my_module, 5);
```

---

## txm_module_manager_memory_fault_notify

Register an application callback on memory fault.

### Prototype

```c
UINT txm_module_manager_memory_fault_notify(
     VOID (*notify_function)(TX_THREAD *, MODULE_INSTANCE *));
```

### Description

This service registers the specified application memory fault notification callback function with the Module Manager. If a memory fault occurs, this function is called with a pointer to the offending thread and the module instance corresponding to the offending thread. The Module Manager processing automatically terminates the offending thread, but leaves any other threads in the module untouched. It is up to the application to decide what to do with the module associated
with the memory fault.

Please see the internal **_txm_module_manager_memory_fault_info** struct for specific information on the memory fault itself.

> **Note:** The memory fault notification callback function is executed directly from the memory fault exception, so only ThreadX APIs Allowed from interrupt service routines can be called. Thus, in order to stop and unload the offending module, the application notification callback must send a signal to an application task so that the module can be stopped and unloaded.

### Input parameters

- **notify_function** Function pointer to the application's memory fault notification callback. Supplying a NULL value disables the memory fault notification.

### Return values

- **TX_SUCCESS** (0x00) Successful notification function registration.

### Allowed from

Initialization and threads

### Example

```c
/* Register a memory fault callback. */
txm_module_manager_memory_fault_notify(my_memory_fault_handler);
```

---

## txm_module_manager_memory_load

Load module from memory.

### Prototype

```c
UINT txm_module_manager_memory_load (
    TXM_MODULE_INSTANCE *module_instance,
    CHAR *module_name,
    VOID *location);
```

### Description

This service loads the module's code and data area into the module memory area set up by ***txm_module_manager_initialize*** and prepares it for execution.

### Input parameters

- **module_instance** Pointer to the instance of the module.
- **module_name** Name of the module.
- **location** Pointer to module's code area, preamble first.

### Return values

- **TX_SUCCESS** (0x00) Successful module load.
- **TX_CALLER_ERROR** (0x13) Invalid caller.
- **TX_NOT_AVAILABLE** (0x1D) Manager not initialized.
- **TX_NO_MEMORY** (0x10) Not enough memory to load module.
- **TX_PTR_ERROR** (0x03) Invalid pointer, module instance, or module preamble.
- **TXM_MODULE_ALIGNMENT_ERROR** (0xF0) Invalid alignment.
- **TXM_MODULE_ALREADY_LOADED** (0xF1) Module already loaded.
- **TXM_MODULE_INVALID** (0xF2) Invalid module preamble.
- **TXM_MODULE_INVALID_PROPERTIES** (0xF3) Incompatible properties.

### Allowed from

Initialization and threads

### Example

```c
TXM_MODULE_INSTANCE     my_module;

/* Initialize the module manager with 64KB of RAM starting at address 0x64010000. */
txm_module_manager_initialize((VOID *) 0x64010000, 0x10000);

/* Load the module that has its code area at address 0x080F0000. */
 txm_module_manager_memory_load(&my_module, "my module", (VOID *) 0x080F0000);
```

### See also

- txm_module_manager_file_load
- txm_module_manager_in_place_load
- txm_module_manager_unload

---

## txm_module_manager_object_pool_create

Create an object pool for modules.

### Prototype

```c
UINT txm_module_manager_object_pool_create(
    VOID *pool_memory_start,
    ULONG pool_memory_size);
```

### Description

This service creates a Module Manager object memory pool that the modules can allocate ThreadX/NetX Duo objects from, thereby keeping the system object out of the module's memory area.

### Input parameters

- **pool_memory_start** Pointer to the start of object memory.
- **pool_memory_size** Size in bytes of the object memory pool.

### Return values

- **TX_SUCCESS** (0x00) Successful initialization.
- **TX_CALLER_ERROR** (0x13) Invalid caller.

### Allowed from

Initialization and threads

### Example

```c
TXM_MODULE_INSTANCE my_module;

/* Initialize the module manager with 64KB of RAM starting at address 0x64010000. */
txm_module_manager_initialize((VOID *) 0x64010000, 0x10000);

/* Create an object memory pool in the next 64KB of memory. */
txm_module_manager_object_pool_create((VOID *) 0x64020000, 0x10000);
```

### See also

- txm_module_manager_initialize

---

## txm_module_manager_properties_get

Get module properties.

### Prototype

```c
UINT txm_module_manager_properties_get(
    TXM_MODULE_INSTANCE *module_instance,
    ULONG *module_properties_ptr);
```

### Description

This service returns the properties (specified in the preamble) of a module.

### Input parameters

- **module_instance** Pointer to the module instance.
- **module_properties_ptr** Pointer to destination for module's properties.

### Return values

- **TX_SUCCESS** (0x00) Successful initialization.
- **TX_PTR_ERROR** (0x03) Invalid pointer.
- **TX_CALLER_ERROR** (0x13) Invalid caller.

### Allowed from

Threads

### Example

```c
TXM_MODULE_INSTANCE     my_module;
ULONG                   module_properties;

/* Initialize the module manager with 64KB of RAM starting at address 0x64010000. */
txm_module_manager_initialize((VOID *) 0x64010000, 0x10000);

/* Create an object memory pool in the next 64KB of memory. */
txm_module_manager_object_pool_create((VOID *) 0x64020000, 0x10000);

/* Load the module that has its code area at address 0x080F0000. */
txm_module_manager_in_place_load(&my_module, "my module", (VOID *) 0x080F0000);

/* Get module properties. */
txm_module_manager_properties_get(&my_module, &module_properties);
```

---

## txm_module_manager_start

Start execution of the module.

### Prototype

```c
UINT txm_module_manager_start(TXM_MODULE_INSTANCE *module_instance);
```

### Description

This service starts execution of the specified, already loaded module.

### Input parameters

- **module_instance** Pointer to previously loaded module instance.

### Return values

- **TX_SUCCESS** (0x00) Successful module start.
- **TX_CALLER_ERROR** (0x13) Invalid caller.
- **TX_NOT_AVAILABLE** (0x1D) Manager not initialized.
- **TX_PTR_ERROR** (0x03) Invalid pointer or module instance.
- **TX_START_ERROR** (0x10) Module already started.

### Allowed from

Initialization and threads

### Example

```c
/* Start the module. */
txm_module_manager_start(&my_module);

/* Let the module run for a while. */
tx_thread_sleep(2000);

/* Stop the module. */
txm_module_manager_stop(&my_module);

/* Unload the module. */
txm_module_manager_unload(&my_module);
```

### See also

- txm_module_manager_stop

---

## txm_module_manager_stop

Stop execution of the module.

### Prototype

```c
UINT txm_module_manager_stop(TXM_MODULE_INSTANCE *module_instance);
```

### Description

This service stops a module that was previously loaded and started. Stopping a module includes executing the module's optional stop thread, terminating all threads, and deleting all resources associated with the module.

### Input parameters

- **module_instance** Pointer to module instance.

### Return values

- **TX_SUCCESS** (0x00) Successful module stop.
- **TX_CALLER_ERROR** (0x13) Invalid caller.
- **TX_NOT_AVAILABLE** (0x1D) Manager not initialized.
- **TX_PTR_ERROR** (0x03) Invalid pointer or module instance.
- **TX_START_ERROR** (0x10) Module not started.

### Allowed from

Threads

### Example

```c
/* Start the module. */
txm_module_manager_start(&my_module);

/* Let the module run for a while. */
tx_thread_sleep(20000);

/* Stop the module. */
txm_module_manager_stop(&my_module);

/* Unload the module. */
txm_module_manager_unload(&my_module);
```

### See also

- txm_module_manager_start

---

## txm_module_manager_unload

Unload the module.

### Prototype

```c
UINT txm_module_manager_unload(TXM_MODULE_INSTANCE *module_instance);
```

### Description

This service unloads the previously loaded and stopped module, freeing all the associated module memory resources.

### Input parameters

- **module_instance** Pointer to the instance of the module.

### Return values

- **TX_SUCCESS** 0x00) Successful module unload.
- **TX_CALLER_ERROR** (0x13) Invalid caller.
- **TX_NOT_AVAILABLE** (0x1D) Manager not initialized.
- **TX_NOT_DONE** (0x20) Invalid module or module not stopped.
- **TX_PTR_ERROR** (0x03) Invalid pointer or module instance.

### Allowed from

Initialization and threads

### Example

```c
/* Stop the module. */
txm_module_manager_stop(&my_module);

/* Unload the module. */
status = txm_module_manager_unload(&my_module);
```

### See also

- txm_module_manager_absolute_load
- txm_module_manager_file_load
- txm_module_manager_in_place_load
- txm_module_manager_memory_load
- txm_module_manager_stop

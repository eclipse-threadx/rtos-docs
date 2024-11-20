---
title: Chapter 3 - Module Manager requirements
description: This article is a description of the steps required for building the ThreadX Module Manager.

---


The ThreadX Module Manager resides in the resident portion of the application along with the ThreadX RTOS. It is responsible for starting the module as well as fielding and dispatching all module requests for ThreadX API services.

> **Note:** The ThreadX Module **Manager** source files (C and assembly) should be added to the ThreadX library project "**tx**".

The following steps are required for building the ThreadX Module Manager (each step is described in greater detail below).

1. The **TX_THREAD** control block must be extended to include module information. The easiest way to accomplish this is to replace the definition of **TX_THREAD_EXTENSION_2** in the ***tx_port.h*** file with the **TX_THREAD_EXTENSION_2** found in ***txm_module_port.h***. See [appendix](appendix) for port-specific extensions.

   Example extension:

   ```c
   #define TX_THREAD_EXTENSION_2                     \
       VOID      tx_thread_module_instance_ptr;      \
       VOID      tx_thread_module_entry_info_ptr;    \
       ULONG     tx_thread_module_current_user_mode; \
       ULONG     tx_thread_module_user_mode;         \
       VOID     *tx_thread_module_kernel_stack_start;\
       VOID     *tx_thread_module_kernel_stack_end;  \
       ULONG     tx_thread_module_kernel_stack_size; \
       VOID     *tx_thread_module_stack_ptr;         \
       VOID     *tx_thread_module_stack_start;       \
       VOID     *tx_thread_module_stack_end;         \
       ULONG     tx_thread_module_stack_size;        \
       VOID     *tx_thread_module_reserved;
   ```

   The following extensions must be defined in ***tx_port.h***.

   ```c
   #define TX_EVENT_FLAGS_GROUP_EXTENSION  VOID    *tx_event_flags_group_module_instance; \
        VOID   (*tx_event_flags_group_set_module_notify)(struct TX_EVENT_FLAGS_GROUP_STRUCT *group_ptr);
   
   #define TX_QUEUE_EXTENSION              VOID    *tx_queue_module_instance; \
        VOID   (*tx_queue_send_module_notify)(struct TX_QUEUE_STRUCT *queue_ptr);
   
   #define TX_SEMAPHORE_EXTENSION          VOID    *tx_semaphore_module_instance; \
        VOID   (*tx_semaphore_put_module_notify)(struct TX_SEMAPHORE_STRUCT *semaphore_ptr);
   
   #define TX_TIMER_EXTENSION              VOID    *tx_timer_module_instance; \
        VOID   (*tx_timer_module_expiration_function)(ULONG id);
   ```

2. Add all the ***txm_module_manager_\**** C and assembly files to the ThreadX library project ***tx***.
3. Rebuild all libraries and executable projects. If NetX Duo is required, all Module and Module Manager C code should be built with **TXM_MODULE_ENABLE_NETX_DUO** defined.

## Module Manager sources

The ThreadX Module Manager has a set of source files that are designed to be linked and located directly with the resident ThreadX code. These files provide the ability to launch a module and field subsequent ThreadX API requests from the module. The module manager files are as follows.

| **File Name** |  **Contents** |
|-------------- | ------------- |
| ***txm_module.h*** | Include file that defines module information (also included in the module source code). |
| ***txm_module_manager_dispatch.h*** | Include file that defines dispatch helper functions.|
| ***txm_module_manager_util.h*** | Include file that defines internal utility helper macros & functions. |
| ***txm_module_port.h*** | Include file that defines port-specific module information (also included in the module source code). |
| ***tx_initialize_low_level.\[s,S,68\]*** | Replaces existing ThreadX library file. Updated vector table and additional vector handlers for module manager and memory exceptions. *This file is only in Cortex-A7/ARM, Cortex-M7/ARM, Cortex-R4/ARM, Cortex-R4/IAR, MCF544xx/GHS, RX63/IAR, RX65N/IAR.*|
| ***tx_thread_context_restore.s*** | Replaces existing ThreadX library file. Restore thread context after interrupt processing. *This file is only in Cortex-A7/ARM, Cortex-R4/ARM, Cortex-R4/IAR.*|
| ***tx_thread_schedule.\[s,S,68\]*** | Replaces existing ThreadX library file. Extended scheduler code, which in this case is used to update memory management registers. |
| ***tx_thread_stack_build.s*** | Replaces existing ThreadX library file. Builds the stack frame of a thread. *This file is only in Cortex-A7/ARM, Cortex-R4/ARM, Cortex-R4/IAR.*|
| ***txm_module_manager_thread_stack_build.\[s,S,68\]*** | Builds all module initial stacks, includes setup for position-independent data access. |
| ***txm_module_manager_user_mode_entry.\[s,S\]*** | Allows the module to enter kernel mode. *This file is only in Cortex-A7/ARM, Cortex-R4/ARM, Cortex-R4/IAR.*|
| ***txm_module_manager_alignment_adjust.c*** | Handles port-specific alignment requirements.|
| ***txm_module_manager_application_request.c*** | Handles the application-specific requests to the resident code. |
| ***txm_module_manager_callback_request.c*** | Sends a callback request to a module. |
| ***txm_module_manager_event_flags_notify_trampoline.c*** | Processes the event flags set notification call from ThreadX. |
| ***txm_module_manager_external_memory_enable.c*** | Creates an entry in the memory management table for a shared memory space the module can access. |
| ***txm_module_manager_file_load.c*** | Allocates and loads a binary module file into the module memory area and prepares it for execution. |
| ***txm_module_manager_in_place_load.c*** | Allocates the module data area and prepares for module execution from the supplied code address. |
| ***txm_module_manager_initialize.c*** | Initializes the Module Manager, including specification of the module memory area available for loading and running modules. |
| ***txm_module_manager_initialize_mmu.c*** | Initialize MMU. Users can edit this file according to their memory map. *This file is only in Cortex-A7/ARM* |
| ***txm_module_manager_mm_initialize.c*** | Initialize MPU/MMU. Users can edit this file according to their memory map. *This file is only in Cortex-A7/ARM* |
| ***txm_module_manager_kernel_dispatch.c*** | Handles the module API requests, based on the request ID. |
| ***txm_module_manager_maximum_module_priority_set.c*** | Sets the maximum thread priority allowed in a module. |
| ***txm_module_manager_memory_fault_handler.c*** | Handles memory faults detected in an executing module. |
| ***txm_module_manager_memory_fault_notify.c*** | Registers an application notification callback whenever a memory fault occurs. |
| ***txm_module_manager_memory_load.c*** | Allocates and loads a module's code and data and prepares the module for execution. |
| ***txm_module_manager_mm_register_setup.c*** | Sets up MPU/MMU registers for the module based on where the code and data are loaded. |
| ***txm_module_manager_object_allocate.c*** | Allocates memory for a module object. |
| ***txm_module_manager_object_deallocate.c*** | Deallocates memory for a module object. |
| ***txm_module_manager_object_pointer_get.c*** | Searches for the supplied object type and name, and if found, returns the object pointer. |
| ***txm_module_manager_object_pointer_get_extended.c*** | Searches for the supplied object type and name, and if found, returns the object pointer. Name length specified for safety. |
| ***txm_module_manager_object_pool_create.c***  | Creates a pool of objects outside the module's data area that module applications can allocate from. |
| ***txm_module_manager_properties_get.c*** | Gets the properties of the specified module. |
| ***txm_module_manager_queue_notify_trampoline.c*** | Processes the queue notification call from ThreadX. |
| ***txm_module_manager_semaphore_notify_trampoline.c*** | Processes the semaphore put notification call from ThreadX.|
| ***txm_module_manager_start.c*** | Starts execution of a module. |
| ***txm_module_manager_stop.c*** | Stops execution of a module. |
| ***txm_module_manager_thread_create.c*** | Creates all module threads. |
| ***txm_module_manager_thread_notify_trampoline.c*** | Processes the thread entry/exit notification call from ThreadX. |
| ***txm_module_manager_thread_reset.c*** | Reset a module thread. |
| ***txm_module_manager_timer_notify_trampoline.c*** | Processes timer expirations from ThreadX. |
| ***txm_module_manager_unload.c*** | Unloads the module from the module memory area. |
| ***txm_module_manager_util.c*** | Internal helper functions for manager. |

## Module Manager initialization

The resident portion of the application is responsible for calling the Module Manager initialization function ***txm_module_manager_initialize***. This function sets up the internal structures for loading and unloading modules, including setting up the memory area used for allocating module memory.

## Module Manager loading

The Module Manager can load modules dynamically into the module memory from binary module files or from a module code section that is already present in the resident code area. In addition, the module manager can execute code in place, that is, only the module data is allocated in the module memory and the code execution is done in place. The following Module Manager load API functions are available.

* ***txm_module_manager_file_load***

* ***txm_module_manager_in_place_load***

* ***txm_module_manager_memory_load***

The memory protected version of the Module Manager also makes sure that the module is loaded with the proper alignment and the memory management registers are set up properly for each module. When memory protection is enabled via the module preamble options, module memory access is restricted to the module code and data areas.

## Module Manager starting

The Module Manager initiates execution of a previously-loaded module via the ***txm_module_manager_start*** API function. To initiate module execution, this function creates a thread that enters the module at the starting location specified in the module preamble. The priority and stack size of this thread is also specified in the module preamble.

## Module Manager stopping

The Module Manager terminates execution of a previously-loaded and executing module via the ***txm_module_manager_stop*** function. This API function first terminates and deletes the initial starting thread. If the module preamble specifies a stop thread, this thread is created and executed. The Module Manager waits for a fixed period of time for the stop thread to complete. Once complete, all system resources created by the module are deleted and the module is placed in a dormant state, from which it can be either restarted or unloaded.

## Module Manager unloading

The Module Manager unloads a previously-loaded but not executing module via the ***txm_module_manager_unload*** function. This API releases all memory associated with the module, freeing it for use with another module in the future.

## Module Manager requests

Requests made by modules to the Module Manager are done via macros in ***txm_module.h*** that map all ThreadX calls to call the Module Manager dispatch function via a function pointer supplied to the module by the Module Manager.

Additional application-specific services made via the module calling ***txm_module_application_request*** are handled by the same macro mechanism used for the ThreadX API. By default, this handling function in the Module Manager is empty and designed such that the application adds the necessary code to process the application-specific requests.

If the request is not implemented by the Module Manager, a value of **TX_NOT_AVAILABLE** error status is returned by the Module Manager. This error code is also returned if the module requests an operation that is outside the scope of the module's access. For example, a module is not allowed to create a timer with the timer control block or callback address outside of the module's code area.

## Module Manager example

The following is an example of Module Manager code that launches the example module previously defined in Chapter 2. It is assumed that the module is already loaded, presumably by the debugger, at ROM address 0x00800000.

```c
#include "tx_api.h"
#include "txm_module.h"

#define DEMO_STACK_SIZE 1024

/* Define the ThreadX object control blocks. */
TX_THREAD   module_manager;

/* Define thread prototype. */
void        module_manager_entry(ULONG thread_input);

/* Define the module object pool area. */
UCHAR       object_memory[8192];

/* Define the module data pool area. */
#define MODULE_DATA_SIZE 65536
UCHAR       module_data_area[MODULE_DATA_SIZE];

/* Define module instances. */
TXM_MODULE_INSTANCE     my_module1;
TXM_MODULE_INSTANCE     my_module2;

/* Define the count of memory faults. */
ULONG memory_faults;

/* Define fault handler. */
VOID module_fault_handler(TX_THREAD *thread, TXM_MODULE_INSTANCE *module)
{
    /* Just increment the fault counter. */
    memory_faults++;
}

/* Define main entry point. */
int main()
{
    /* Enter the ThreadX kernel. */
    tx_kernel_enter();
}

/* Define what the initial system looks like. */
void tx_application_define(void *first_unused_memory)
{
    /* Create the module manager thread. */
    tx_thread_create(&module_manager, "Module Manager Thread", module_manager_entry, 0,
                    first_unused_memory, DEMO_STACK_SIZE,
                    1, 1, TX_NO_TIME_SLICE, TX_AUTO_START);
}

/* Define the test threads. */
void module_manager_entry(ULONG thread_input)
{
    /* Initialize the module manager. */
    txm_module_manager_initialize((VOID *) module_data_area, MODULE_DATA_SIZE);

    /* Create a pool for module objects. */
    txm_module_manager_object_pool_create(object_memory, sizeof(object_memory));

    /* Register a fault handler. */
    txm_module_manager_memory_fault_notify(module_fault_handler);

    /* Load the module that is already there,
        in this example it is placed at 0x00800000. */
    txm_module_manager_in_place_load(&my_module1, "my module1", (VOID *) 0x00800000);

    /* Load a second instance of the module. */
    txm_module_manager_in_place_load(&my_module2, "my module2", (VOID *) 0x00800000);

    /* Enable shared memory region for module2. */
    txm_module_manager_external_memory_enable(&my_module2, (void*)0x20600000, 0x010000, 0x3F);

    /* Start the modules. */
    txm_module_manager_start(&my_module1);
    txm_module_manager_start(&my_module2);

    /* Sleep for a while and let the modules run... */
    tx_thread_sleep(300);

    /* Stop the modules. */
    txm_module_manager_stop(&my_module1);
    txm_module_manager_stop(&my_module2);

    /* Unload the modules. */
    txm_module_manager_unload(&my_module1);
    txm_module_manager_unload(&my_module2);

    /* Reload the modules. */
    txm_module_manager_in_place_load(&my_module2, "my module2", (VOID *) 0x00800000);
    txm_module_manager_in_place_load(&my_module1, "my module1", (VOID *) 0x00800000);

    /* Give both modules shared memory. */
    txm_module_manager_external_memory_enable(&my_module2, (void*)0x20600000, 0x010000, 0x3F);
    txm_module_manager_external_memory_enable(&my_module1, (void*)0x20600000, 0x010000, 0x3F);

    /* Set maximum module1 priority to 5. */
    txm_module_manager_maximum_module_priority_set(&my_module1, 5);

    /* Start the modules again. */
    txm_module_manager_start(&my_module2);
    txm_module_manager_start(&my_module1);

    /* Now just spin... */
    while(1)
    {
        tx_thread_sleep(100);

        /* Threads 0 and 5 in module1 are not created because they violate the maximum priority. */
    }
}
```

## Module Manager building

The ***txm_module_manager_\**** source files must be added to the ThreadX library.

A ThreadX Module Manager application is effectively the same as a standard ThreadX application, which is one or more application files linked together with the ThreadX library ***tx.a***. Building a module manager application is dependent on the tool chain being used. See [appendix](appendix) for port-specific examples.

---
title: Chapter 2 - Installation & Use of ThreadX SMP
description: This chapter contains a description of various issues related to installation, setup, and usage of the high-performance ThreadX SMP kernel.
---

# Chapter 2 - Installation & Use of ThreadX SMP

This chapter contains a description of various issues related to installation, setup, and usage of the high-performance ThreadX SMP kernel.

## Host Considerations

Embedded software is usually developed on Windows or Linux (Unix) host computers. After the application is compiled, linked, and located on the host, it is downloaded to the target hardware for execution.

Usually the target download is done from within the development tool debugger. After download, the debugger is responsible for providing target execution control (go, halt, breakpoint, etc.) as well as access to memory and processor registers.

Most development tool debuggers communicate with the target hardware via on-chip debug (OCD) connections such as JTAG (IEEE 1149.1) and Background Debug Mode (BDM). Debuggers also communicate with target hardware through In-Circuit Emulation (ICE) connections. Both OCD and ICE connections provide robust solutions with minimal intrusion on the target resident software.

As for resources used on the host, the source code for ThreadX SMP is delivered in ASCII format and requires approximately 1 MBytes of space on the host computer's hard disk.

> **Important:** Please review the supplied **readme_threadx.txt** file for additional host system considerations and options.

## Target Considerations

ThreadX SMP requires between 2 KBytes and 20 KBytes of Read Only Memory (ROM) on the target. Another 1 to 2 KBytes of the target's Random Access Memory (RAM) are required for the ThreadX SMP system stack and other global data structures.

For timer-related functions like service call time-outs, time-slicing, and application timers to function, the underlying target hardware must provide a periodic interrupt source. If the processor has this capability, it is utilized by ThreadX SMP. Otherwise, if the target processor does not have the ability to generate a periodic interrupt, the user's hardware must provide it. Setup and configuration of the timer interrupt is typically located in the ***tx_initialize_low_level*** assembly file in the ThreadX SMP distribution.

> **Important:** ThreadX SMP is still functional even if no periodic timer interrupt source is available. However, none of the timer-related services are functional. Please review the supplied **readme_threadx.txt** file for any additional host system considerations and/or options.

## Product Distribution

The exact content of the distribution disk depends on the target processor, development tools, and the ThreadX SMP package purchased. However, the following is a list of several important files that are common to most product distributions:

### ThreadX_Express_Startup.pdf

This PDF provides a simple, four-step procedure to get ThreadX SMP running on a specific target processor/board and specific development tools.

### readme_threadx.txt

Text file containing specific information about the ThreadX SMP port, including information about the target processor and the development tools.

| Tool | Description |
| -------------- | ------------------------------------------------------------------------------------------------- |
| **tx_api.h**  | C header file containing all system equates, data structures, and service prototypes.             |
| **tx_port.h** | C header file containing all development-tool and target specific data definitions and structures. |
|**demo_threadx.c**| C file containing a small demo application.|
|**tx.a (or tx.lib)**| Binary version of the ThreadX SMP C library that is distributed with the *standard* package.|

> **Important:** All file names are in lower-case. This naming convention makes it easier to convert the commands to Linux (Unix) development platforms.

## ThreadX SMP Installation

Installation of ThreadX SMP is straightforward. Refer to the ***ThreadX_Express_Startup.pdf*** file and the ***readme_threadx.txt*** file for specific information on installing ThreadX SMP for your specific environment.

> **Important:** Be sure to back up the ThreadX SMP distribution disk and store it in a safe location.

> **Important:** Application software needs access to the ThreadX SMP library file (usually **tx.a** or **tx.lib**) and the C include files **tx_api.h** and **tx_port.h**. This is accomplished either by setting the appropriate path for the development tools or by copying these files into the application development area.

## Using ThreadX SMP

Using ThreadX SMP is easy. Basically, the application code must include ***tx_api.h*** during compilation and link with the ThreadX SMP run-time library ***tx.a*** (or ***tx.lib)***.

There are four steps required to build a ThreadX SMP application:

Include the ***tx_api.h*** file in all application files that use ThreadX SMP services or data structures.

Create the standard C ***main*** function. This function must eventually call ***tx_kernel_enter*** to start ThreadX SMP. Application-specific initialization that does not involve ThreadX SMP may be added prior to entering the kernel.

> **Important:** The ThreadX SMP entry function **tx_kernel_enter** does not return. So be sure not to place any processing or function calls after it.

Create the ***tx_application_define*** function. This is where the initial system resources are created. Examples of system resources include threads, queues, memory pools, event flags groups, mutexes, and semaphores.

Compile application source and link with the ThreadX SMP run-time library ***tx.lib***. The resulting image can be downloaded to the target and executed!

## Small Example System

The small example system in Figure 1 on page 28 shows the creation of a single thread with a priority of 3. The thread executes, increments a counter, then sleeps for one clock tick. This process continues forever.

```c
#include              "tx_api.h"

unsigned long         my_thread_counter = 0;
TX_THREAD             my_thread;

main( )
{
      /* Enter the ThreadX SMP kernel. */
      tx_kernel_enter( );
}

void tx_application_define(void *first_unused_memory)
{

      /* Create my_thread! */
      tx_thread_create(&my_thread, "My Thread",
          my_thread_entry, 0x1234, first_unused_memory, 1024,
             3, 3, TX_NO_TIME_SLICE, TX_AUTO_START);
}

void my_thread_entry(ULONG thread_input)
{
      /* Enter into a forever loop. */
      while(1)
      {

            /* Increment thread counter. */
            my_thread_counter++;

            /* Sleep for 1 tick. */
            tx_thread_sleep(1);
      }
}
```
**FIGURE 1. Template for Application Development**

Although this is a simple example, it provides a good template for real application development. Once again, please see the ***readme_threadx.txt*** file for additional details.

## Troubleshooting

Each ThreadX SMP port is delivered with a demonstration application. It is always a good idea to first get the demonstration system running—either on actual target hardware or simulated environment.

> **Important:** See the **readme_threadx.txt** file supplied with the distribution for more specific details regarding the demonstration system.

If the demonstration system does not execute properly, the following are some troubleshooting tips:

  - Determine how much of the demonstration is running.

  - Increase stack sizes (this is more important in actual application code than it is for the demonstration).

  - Rebuild the ThreadX SMP library with TX_ENABLE_STACK_CHECKING defined. This will enable the built-in ThreadX SMP stack checking.

  - Temporarily bypass any recent changes to see if the problem disappears or changes.

## Configuration Options

There are several configuration options when building the ThreadX SMP library and the application using ThreadX SMP. The options below can be defined in the application source, on the command line, or within the ***tx_user.h*** include file.

> **Important:** Options defined in **tx_user.h** are applied only if the application and ThreadX SMP library are built with **TX_INCLUDE_USER_DEFINE_FILE** defined.

### Smallest Configuration
For the smallest code size, the following ThreadX SMP configuration options should be considered (in absence of all other options):

- TX_DISABLE_ERROR_CHECKING
- TX_DISABLE_PREEMPTION_THRESHOLD
- TX_DISABLE_NOTIFY_CALLBACKS 
- TX_DISABLE_REDUNDANT_CLEARING 
- TX_DISABLE_STACK_FILLING 
- TX_NOT_INTERRUPTABLE 
- TX_TIMER_PROCESS_IN_ISR

### Fastest Configuration 
For the fastest execution, the same configuration options used for the Smallest Configuration previously, but with this option also considered:

- TX_REACTIVATE_INLINE

Review the ***readme_threadx.txt*** file for additional options for your specific version of ThreadX SMP. Detailed configuration options are described beginning on page 28.

### Global Time Source  
For other Eclipse ThreadX products (FileX, NetX Duo, GUIX, USBX, etc.), ThreadX SMP defines the number of ThreadX SMP timer ticks that represents one second. Others derive their time requirements based on this constant. By default, the value is 100, assuming a 10ms periodic interrupt. The user may override this value by defining TX_TIMER_TICKS_PER_SECOND with the desired value in ***tx_port.h*** or within the IDE or command line.

### Detailed Configuration Options

- **TX_BLOCK_POOL_ENABLE_PERFORMANCE_INFO** : When defined, enables the gathering of performance information on block pools. By default, this option is not defined.
- **TX_BYTE_POOL_ENABLE_PERFORMANCE_INFO** : When defined, enables the gathering of performance information on byte pools. By default, this option is not defined.
- **TX_DISABLE_ERROR_CHECKING**: Bypasses basic service call error checking. When defined in the application source, all basic parameter error checking is disabled. This may improve performance by as much as 30% and may also reduce the image size.

> **Note:** *It is only safe to disable error checking if the application can absolutely guarantee all input parameters are always valid under all circumstances, including input parameters derived from external input. If invalid input is supplied to the API with error checking disabled, the resulting behavior is undefined and could result in memory corruption or system crash.*

> **Note:** *ThreadX SMP API return values not affected by disabling error checking are listed in bold in the "Return Values" section of each API description in Chapter 4. The nonbold return values are void if error checking is disabled by using the TX_DISABLE_ERROR_CHECKING option.*
- **TX_DISABLE_NOTIFY_CALLBACKS** : When defined, disables the notify callbacks for various ThreadX SMP objects. Using this option slightly reduces code size and improves performance. By default, this option is not defined.
- **TX_DISABLE_PREEMPTION_THRESHOLD** : When defined, disables the preemption threshold feature and slightly reduces code size and improves performance. Of course, the preemption threshold capabilities are no longer available. By default, this option is not defined.
- **TX_DISABLE_REDUNDANT_CLEARING** : When defined, removes the logic for initializing ThreadX SMP global C data structures to zero. This should only be used if the compiler's initialization code sets all un-initialized C global data to zero. Using this option slightly reduces code size and improves performance during initialization. By default, this option is not defined.
- **TX_DISABLE_STACK_FILLING** : When defined, disables placing the 0xEF value in each byte of each thread's stack when created. By default, this option is not defined.
- **TX_ENABLE_EVENT_TRACE** : When defined, ThreadX SMP enables the event gathering code for creating a TraceX trace buffer. See the TraceX User Guide for more details.
- **TX_ENABLE_STACK_CHECKING** : When defined, enables ThreadX SMP run-time stack checking, which includes analysis of how much stack has been used and examination of data pattern "fences" before and after the stack area. If a stack error is detected, the registered application stack error handler is called. This option does result in slightly increased overhead and code size. Review the ***tx_- thread_stack_error_notify*** API for more information. By default, this option is not defined.
- **TX_EVENT_FLAGS_ENABLE_PERFORMANCE_INFO** : When defined, enables the gathering of performance information on event flags groups. By default, this option is not defined.
- **TX_INLINE_THREAD_RESUME_SUSPEND** : When defined, ThreadX SMP improves the ***tx_thread_resume*** and ***tx_thread_suspend*** API calls via in-line code. This increases code size but enhances performance of these two API calls.
- **TX_MAX_PRIORITIES** : Defines the priority levels for ThreadX SMP. Legal values range from 32 through 1024 (inclusive) and must be evenly divisible by 32. Increasing the number of priority levels supported increases the RAM usage by 128 bytes for every group of 32 priorities. However, there is only a negligible effect on performance. By default, this value is set to 32 priority levels.
- **TX_MINIMUM_STACK** : Defines the minimum stack size (in bytes). It is used for error checking when threads are created. The default value is port-specific and is found in ***tx_port.h.***
- **TX_MISRA_ENABLE** : When defined, ThreadX SMP utilizes MISRA C compliant conventions. Refer to ***[ThreadX SMP MISRA C compliance](appendix-e.md)*** for details.
- **TX_MUTEX_ENABLE_PERFORMANCE_INFO** : When defined, enables the gathering of performance information on mutexes. By default, this option is not defined.
- **TX_NO_TIMER** : When defined, the ThreadX SMP timer logic is completely disabled. This is useful in cases where the ThreadX SMP timer features (thread sleep, API timeouts, time-slicing, and application timers) are not utilized.
- **TX_NOT_INTERRUPTABLE** : When defined, ThreadX SMP does not attempt to minimize interrupt lockout time. This results in faster execution but does slightly increase interrupt lockout time.
- **TX_QUEUE_ENABLE_PERFORMANCE_INFO** : When defined, enables the gathering of performance information on queues. By default, this option is not defined.
- **TX_REACTIVATE_INLINE** : When defined, performs reactivation of ThreadX SMP timers in-line instead of using a function call. This improves performance but slightly increases code size. By default, this option is not defined.
- **TX_SEMAPHORE_ENABLE_PERFORMANCE_INFO** : When defined, enables the gathering of performance information on semaphores. By default, this option is not defined.
- **TX_THREAD_ENABLE_PERFORMANCE_INFO** : When defined, enables the gathering of performance information on threads. By default, this option is not defined.
- **TX_THREAD_SMP_CORE_MASK** : Defines bit map mask for CORE exclusion. For example, a 4-core environment has a value of 0xF for this define.
- **TX_THREAD_SMP_DEBUG_ENABLE** : When defined, ThreadX SMP debug information is saved in a circular buffer.
- **TX_THREAD_SMP_DYNAMIC_CORE_MAX** : When defined, enables the dynamic maximum number of cores that can be adjusted at run-time.
- **TX_THREAD_SMP_EQUAL_PRIORITY** : When defined, ThreadX SMP only schedules equal priority threads in parallel. This should be defined prior to building the ThreadX SMP library.
- **TX_THREAD_SMP_INTER_CORE_INTERRUPT** : When defined, ThreadX SMP generates inter-core interrupts.
- **TX_THREAD_SMP_MAX_CORES** : Defines the maximum number of cores.
- **TX_THREAD_SMP_ONLY_CORE_0_DEFAULT** : When defined, ThreadX SMP defaults all threads and timers to execute only on core 0 by default. The application may override this by calling the core exclude APIs. This should be defined prior to building the ThreadX SMP library.
- **TX_THREAD_SMP_WAKEUP_LOGIC** : When defined, application macro to wakeup core "i" is invoked. This should be defined prior to inclusion of ***tx_port.h.***
- **TX_THREAD_SMP_WAKEUP(i)** : Defines an application macro to wakeup core "i". This should be defined prior to inclusion of ***tx_port.h.***
- **TX_TIMER_ENABLE_PERFORMANCE_INFO** : When defined, enables the gathering of performance information on timers. By default, this option is not defined.
- **TX_TIMER_PROCESS_IN_ISR** : When defined, eliminates the internal system timer thread for ThreadX SMP. This results in improved performance on timer events and smaller RAM requirements because the timer stack and control block are no longer needed. However, using this option moves all the timer expiration processing to the timer ISR level. By default, this option is not defined.

  > **Note:** That services allowed from timers may not be allowed from ISRs and thus might not be allowed when using this option.

- **TX_TIMER_THREAD_PRIORITY** : Defines the priority of the internal ThreadX SMP system timer thread. The default value is priority 0—the highest priority in ThreadX SMP. The default value is defined in ***tx_port.h.***
- **TX_TIMER_THREAD_STACK_SIZE** : Defines the stack size (in bytes) of the internal ThreadX SMP system timer thread. This thread processes all thread sleep requests as well as all service call timeouts. In addition, all application timer callback routines are invoked from this context. The default value is port-specific and is found in ***tx_port.h.***

## ThreadX SMP Version ID

The ThreadX SMP version ID can be found in the ***readme_threadx.txt*** file. This file also contains a version history of the corresponding port. Application software can obtain the ThreadX SMP version by examining the global string ***_tx_version_id.***

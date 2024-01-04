---
title: Appendix F - GUIX RTOS Binding Services
description: Learn about GUIX RTOS binding services.
---

# Appendix F - GUIX RTOS Binding Services

GUIX requires thread or tasking services, mutex, event queue, and timing services providing by the underlying RTOS. By default GUIX is configured to utilize the ThreadX real time operating system to provide these services. To port GUIX to another operating system, the developer should # define the pre-processor directive GX_DISABLE_THREADX_BINDING and rebuild the GUIX library to remove the ThreadX dependencies. In addition, the developer will need to provide the following macro
definitions and supporting functions. Examples of these macro definitions and supporting functions can be found in the files gx_system_rtos_bind.h and gx_system_rtos_bind.c, which provide an
example generic rtos integration.

System Integration macros:

**GX_RTOS_BINDING_INITIALIZE**

This macro is invoked during system initialization. The macro should be defined to call any function needed to prepare your rtos system services or rtos resources prior to use. This is the binding's opportunity to prepare the rtos resources that GUIX will use.

**GX_SYSTEM_THREAD_START**

This macro is invoked when the GUIX task or thread should start executing. This macro should be defined to call a function which will start the GUIX thread running. The entry point to the GUIX thread is passed to the called function. The signature of the called function must be

**UINT *function_name*(VOID (thread_entry_point)(VOID));**

This function should return GX_SUCCESS if the thread is successfully started, or GX_FAILURE.

**GX_EVENT_PUSH**

This macro is invoked to push an event into the FIFO event queue used by GUIX. When porting to a new rtos, it is your responsibility to implement this event queue in a thread-safe manner. GX_EVENT structures must be copied into this queue and copied out of this queue, i.e. a queue of GX_EVENT pointers will not work, since GUIX events can be automatic variables from the view of the event producer. The signature of the function called by this macro must be:

**UINT *function_name* (GX_EVENT *event_ptr);**

This function should return GX_SUCCESS if the event is pushed into the event queue, otherwise it should return GX_FAILURE.

**GX_EVENT_POP**

This macro is invoked to remove the head (oldest) event from the GUIX event queue and copy it into the requested location. This function must be able to optionally block or wait for an event if no events are currently in the event queue. The signature of the function invoked by this macro must be

UINT function_name(GX_EVENT *put_event, GX_BOOL wait)

If the wait parameter == GX_TRUE, the function should not return until an event is provided. If the wait parameter is GX_FALSE, the function should return immediately with or without an event.

If an event is retrieved from the queue, it should copied into the put_event location and the return status is GX_SUCCESS. Otherwise the return status should be GX_FAILURE.

**GX_EVENT_FOLD**

This macro is invoked by GUIX to fold an event into the FIFO event queue. Folding an event means that if an event of the same type already exists in the queue, that entry is update to contain the payload of the new event. If an existing event of the same type is not found in the queue, a new event is pushed into the queue. 

For bindings that cannot implement the event fold feature, it is acceptable to simply invoke the GX_EVENT_PUSH.

**GX_TIMER_START**

This macro is invoked when GUIX needs to receive periodic timer input. This macro should invoke a service that starts the low-level RTOS periodic timer service. If the RTOS timer service cannot be easily stopped and started, it is acceptable but less efficient to leave this service running at all times.

When the low-level RTOS timer service periodically expires, the binding must call the GUIX system function _gx_system_timer_expiration(0); Calling this function periodically is what drives the high-level GUIX timer widget timer services.

**GX_TIMER_STOP**

This macro is invoked when GUIX no longer needs a periodic timer (i.e. there are no active GUIX timers running). If the RTOS timer service cannot be easily stopped and started, it is acceptable but less efficient to leave this service running at all times and define this
macro to do nothing.

**GX_SYSTEM_MUTEX_LOCK**

This macro is invoked by GUIX during critical code sections to prevent another task from  pre-empting and modifying common data structures, potentially causing corruption. This macro should call a function that implements the suitable RTOS resource locking service.

If you never utilize any GUIX API services outside of the GUIX thread, you can define this macro to do nothing.

**GX_SYSTEM_MUTEX_UNLOCK**

This macro is invoked at the end of critical code sections, and should unlock the GUIX resource using the suitable underlying RTOS service. If you never utilize any GUIX API services outside of the GUIX thread, you can define this macro to do nothing.

**GX_SYSTEM_TIME_GET**

This macro should call a function that returns the current system time is "system ticks", which is usually the number of low-level timer interrupts that have occurred since system startup. This service is used to calculate touch event pen speed for touch input gestures. The signature of the function invoked by this macro must be:

**ULONG *function_name*(VOID);**

**GX_CURRENT_THREAD**

This macro is invoked to identify the currently executing thread. The service called by this macro must return a void *, meaning that the data type used by your operating system to identify the current execution thread must be cast to a void * to be returned to GUIX.

A complete example of a generic RTOS binding is implemented in the files gx_system_rtos_bind.h and gx_system_rtos_bind.c
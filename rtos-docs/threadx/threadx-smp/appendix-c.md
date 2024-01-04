---
title: Appendix C - ThreadX SMP Data Types
description: ThreadX SMP Data Types
---

# Appendix C - ThreadX SMP Data Types

## TX_BLOCK_POOL

```C
typedef struct TX_BLOCK_POOL_STRUCT
{
    ULONG tx_block_pool_id;
    CHAR *tx_block_pool_name;
    ULONG tx_block_pool_available;
    ULONG tx_block_pool_total;
    UCHAR *tx_block_pool_available_list;
    UCHAR *tx_block_pool_start;
    ULONG tx_block_pool_size;
    ULONG tx_block_pool_block_size;
    struct TX_THREAD_STRUCT

*tx_block_pool_suspension_list;
    ULONG tx_block_pool_suspended_count;
    struct TX_BLOCK_POOL_STRUCT

*tx_block_pool_created_next,

*tx_block_pool_created_previous;

#ifdef TX_BLOCK_POOL_ENABLE_PERFORMANCE_INFO
    ULONG
tx_block_pool_performance_allocate_count;
    ULONG
tx_block_pool_performance_release_count;
    ULONG
tx_block_pool_performance_suspension_count;
    ULONG
tx_block_pool_performance_timeout_count;
#endif

    TX_BLOCK_POOL_EXTENSION /* Port defined */

} TX_BLOCK_POOL;
```
## TX_BYTE_POOL

```C
typedef struct TX_BYTE_POOL_STRUCT
{
    ULONG tx_byte_pool_id;
    CHAR *tx_byte_pool_name;
    ULONG tx_byte_pool_available;
    ULONG tx_byte_pool_fragments;
    UCHAR *tx_byte_pool_list;
    UCHAR *tx_byte_pool_search;
    UCHAR *tx_byte_pool_start;
    ULONG tx_byte_pool_size;
    struct TX_THREAD_STRUCT
                      *tx_byte_pool_owner;
    struct TX_THREAD_STRUCT

*tx_byte_pool_suspension_list;
    ULONG
tx_byte_pool_suspended_count;
    struct TX_BYTE_POOL_STRUCT

*tx_byte_pool_created_next,

*tx_byte_pool_created_previous;

#ifdef TX_BYTE_POOL_ENABLE_PERFORMANCE_INFO
    ULONG
tx_byte_pool_performance_allocate_count;
    ULONG
tx_byte_pool_performance_release_count;
    ULONG tx_byte_pool_performance_merge_count;
    ULONG tx_byte_pool_performance_split_count;
    ULONG tx_byte_pool_performance_search_count;
    ULONG
tx_byte_pool_performance_suspension_count;
    ULONG
tx_byte_pool_performance_timeout_count;
#endif

    TX_BYTE_POOL_EXTENSION /* Port defined */
} TX_BYTE_POOL;
```
## TX_EVENT_FLAGS_GROUP

```C
typedef struct TX_EVENT_FLAGS_GROUP_STRUCT
{
    ULONG tx_event_flags_group_id;
    CHAR *tx_event_flags_group_name;
    ULONG tx_event_flags_group_current;
    UINT tx_event_flags_group_reset_search;
    struct TX_THREAD_STRUCT

*tx_event_flags_group_suspension_list;
    ULONG
tx_event_flags_group_suspended_count;
    struct TX_EVENT_FLAGS_GROUP_STRUCT
*tx_event_flags_group_created_next,

*tx_event_flags_group_created_previous;
    ULONG
tx_event_flags_group_delayed_clear;

#ifdef TX_EVENT_FLAGS_ENABLE_PERFORMANCE_INFO
    ULONG
tx_event_flags_group_performance_set_count;
    ULONG
tx_event_flags_group__performance_get_count;
    ULONG
tx_event_flags_group___performance_suspension_count;
    ULONG
tx_event_flags_group____performance_timeout_count;
#endif

#ifndef TX_DISABLE_NOTIFY_CALLBACKS

    VOID
(*tx_event_flags_group_set_notify)(struct
TX_EVENT_FLAGS_GROUP_STRUCT *);
#endif

    TX_EVENT_FLAGS_GROUP_EXTENSION /* Port
defined  */
} TX_EVENT_FLAGS_GROUP;
```
## TX_MUTEX

```C
typedef struct TX_MUTEX_STRUCT
{
    ULONG tx_mutex_id;
    CHAR *tx_mutex_name;
    ULONG tx_mutex_ownership_count;
    TX_THREAD *tx_mutex_owner;
    UINT tx_mutex_inherit;
    UINT tx_mutex_original_priority;
    struct TX_THREAD_STRUCT
                      *tx_mutex_suspension_list;
    ULONG tx_mutex_suspended_count;
    struct TX_MUTEX_STRUCT
                      *tx_mutex_created_next,

*tx_mutex_created_previous;
    ULONG tx_mutex_highest_priority_waiting;
    struct TX_MUTEX_STRUCT
                      *tx_mutex_owned_next,
                      *tx_mutex_owned_previous;

#ifdef TX_MUTEX_ENABLE_PERFORMANCE_INFO
    ULONG tx_mutex_performance_put_count;
    ULONG tx_mutex_performance_get_count;
    ULONG tx_mutex_performance_suspension_count;
    ULONG tx_mutex_performance_timeout_count;
    ULONG
tx_mutex_performance_priority_inversion_count;
    ULONG
tx_mutex_performance__priority_inheritance_count
;
#endif

    TX_MUTEX_EXTENSION /* Port defined */

} TX_MUTEX;
```

## TX_QUEUE

```C
typedef struct TX_QUEUE_STRUCT
{
    ULONG tx_queue_id;
    CHAR *tx_queue_name;
    UINT tx_queue_message_size;
    ULONG tx_queue_capacity;
    ULONG tx_queue_enqueued;
    ULONG tx_queue_available_storage;
    ULONG *tx_queue_start;
    ULONG *tx_queue_end;
    ULONG *tx_queue_read;
    ULONG *tx_queue_write;
    struct TX_THREAD_STRUCT
                      *tx_queue_suspension_list;
    ULONG tx_queue_suspended_count;
    struct TX_QUEUE_STRUCT
                      *tx_queue_created_next,

*tx_queue_created_previous;

#ifdef TX_QUEUE_ENABLE_PERFORMANCE_INFO
    ULONG
tx_queue_performance_messages_sent_count;
    ULONG
tx_queue_performance_messages_received_count;
    ULONG
tx_queue_performance_empty_suspension_count;
    ULONG
tx_queue_performance_full_suspension_count;
    ULONG tx_queue_performance_full_error_count;
    ULONG tx_queue_performance_timeout_count;
#endif

#ifndef TX_DISABLE_NOTIFY_CALLBACKS
    VOID *tx_queue_send_notify)(struct
TX_QUEUE_STRUCT *);
#endif

    TX_QUEUE_EXTENSION /* Port defined  */

} TX_QUEUE;
```
## TX_SEMAPHORE

```C
typedef struct TX_SEMAPHORE_STRUCT
{
    ULONG tx_semaphore_id;
    CHAR *tx_semaphore_name;
    ULONG tx_semaphore_count;
    struct TX_THREAD_STRUCT

*tx_semaphore_suspension_list;
    ULONG tx_semaphore_suspended_count;
    struct TX_SEMAPHORE_STRUCT

*tx_semaphore_created_next,

*tx_semaphore_created_previous;

#ifdef TX_SEMAPHORE_ENABLE_PERFORMANCE_INFO
    ULONG tx_semaphore_performance_put_count;
    ULONG tx_semaphore_performance_get_count;
    ULONG
tx_semaphore_performance_suspension_count;
    ULONG
tx_semaphore_performance_timeout_count;
#endif

#ifndef TX_DISABLE_NOTIFY_CALLBACKS
    VOID (*tx_semaphore_put_notify)(struct
TX_SEMAPHORE_STRUCT *);
#endif

    TX_SEMAPHORE_EXTENSION /* Port defined  */

} TX_SEMAPHORE;
```
## TX_THREAD

```C
typedef struct TX_THREAD_STRUCT
{
    ULONG tx_thread_id;
    ULONG tx_thread_run_count;
    VOID *tx_thread_stack_ptr;
    VOID *tx_thread_stack_start;
    VOID *tx_thread_stack_end;
    ULONG tx_thread_stack_size;
    ULONG tx_thread_time_slice;
    ULONG tx_thread_new_time_slice;
    struct TX_THREAD_STRUCT
                      *tx_thread_ready_next,
                      *tx_thread_ready_previous;

    TX_THREAD_EXTENSION_0  /* Port defined  */

    CHAR *tx_thread_name;
    UINT tx_thread_priority;
    UINT tx_thread_state;
    UINT tx_thread_delayed_suspend;
    UINT tx_thread_suspending;
    UINT tx_thread_preempt_threshold;
    VOID (*tx_thread_schedule_hook)(struct
TX_THREAD_STRUCT *, ULONG);
    VOID (*tx_thread_entry)(ULONG);
    ULONG tx_thread_entry_parameter;
    TX_TIMER_INTERNAL tx_thread_timer;
    VOID (*tx_thread_suspend_cleanup)(struct
TX_THREAD_STRUCT *);
    VOID *tx_thread_suspend_control_block;
    struct TX_THREAD_STRUCT
                      *tx_thread_suspended_next,

*tx_thread_suspended_previous;
    ULONG tx_thread_suspend_info;
    VOID *tx_thread_additional_suspend_info;
    UINT tx_thread_suspend_option;
    UINT tx_thread_suspend_status;

    TX_THREAD_EXTENSION_1 /* Port defined  */

    struct TX_THREAD_STRUCT
                      *tx_thread_created_next,

*tx_thread_created_previous;
    UINT tx_thread_smp_core_mapped;
    ULONG tx_thread_smp_core_control;
    UINT tx_thread_smp_core_executing;

    TX_THREAD_EXTENSION_2 /* Port defined  */

    ULONG tx_thread_smp_cores_excluded;
    ULONG tx_thread_smp_cores_allowed;

    VOID *tx_thread_filex_ptr;

    UINT tx_thread_user_priority;
    UINT tx_thread_user_preempt_threshold;
    UINT tx_thread_inherit_priority;
    ULONG tx_thread_owned_mutex_count;
    struct TX_MUTEX_STRUCT
*tx_thread_owned_mutex_list;

#ifdef TX_THREAD_ENABLE_PERFORMANCE_INFO
    ULONG tx_thread_performance_resume_count;
    ULONG tx_thread_performance_suspend_count;
    ULONG
tx_thread_performance_solicited_preemption_count
;
    ULONG
tx_thread_performance_interrupt_preemption_count
;
    ULONG
tx_thread_performance_priority_inversion_count;
    struct TX_THREAD_STRUCT

*tx_thread_performance_last_preempting_thread;
    ULONG
tx_thread_performance_time_slice_count;
    ULONG
tx_thread_performance_relinquish_count;
    ULONG tx_thread_performance_timeout_count;
    ULONG
tx_thread_performance_wait_abort_count;
#endif
    VOID *tx_thread_stack_highest_ptr;
#ifndef TX_DISABLE_NOTIFY_CALLBACKS
    VOID (*tx_thread_entry_exit_notify)
                      (struct TX_THREAD_STRUCT
*, UINT);
#endif

    TX_THREAD_EXTENSION_3 /* Port defined  */
    ULONG tx_thread_suspension_sequence;

    TX_THREAD_USER_EXTENSION

} TX_THREAD;
```
## TX_TIMER

```C
typedef struct TX_TIMER_STRUCT
{
    ULONG tx_timer_id;
    CHAR *tx_timer_name;
    TX_TIMER_INTERNAL tx_timer_internal;
    struct TX_TIMER_STRUCT
                      *tx_timer_created_next,

*tx_timer_created_previous;

    TX_TIMER_EXTENSION  /* Port defined  */

#ifdef TX_TIMER_ENABLE_PERFORMANCE_INFO
    ULONG tx_timer_performance_activate_count;
    ULONG tx_timer_performance_reactivate_count;
    ULONG tx_timer_performance_deactivate_count;
    ULONG tx_timer_performance_expiration_count;
    ULONG
tx_timer_performance__expiration_adjust_count;
#endif

} TX_TIMER;
```

## TX_TIMER_INTERNAL

```C
typedef struct TX_TIMER_INTERNAL_STRUCT
{
    ULONG tx_timer_internal_remaining_ticks;
    ULONG tx_timer_internal_re_initialize_ticks;
    VOID
(*tx_timer_internal_timeout_function)(ULONG);
    ULONG tx_timer_internal_timeout_param;
    struct TX_TIMER_INTERNAL_STRUCT
    *tx_timer_internal_active_next,

*tx_timer_internal_active_previous;
    struct TX_TIMER_INTERNAL_STRUCT

*tx_timer_internal_list_head;
    ULONG tx_timer_internal_smp_cores_excluded
    TX_TIMER_INTERNAL_EXTENSION /* Port defined
*/

} TX_TIMER_INTERNAL;
```
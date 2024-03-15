---
title: Appendix A - FileX services
description: Learn about the FileX Services.
---

# Appendix A - FileX services

## System Services

```c
UINT    fx_system_date_get(UINT *year, UINT *month, UINT *day);
UINT    fx_system_date_set(UINT year, UINT month, UINT day);
UINT    fx_system_time_get(UINT *hour, UINT *minute, UINT *second);
UINT    fx_system_time_set(UINT hour, UINT minute, UINT second);
VOID    fx_system_initialize(VOID);
```

## Media Services

```c
UINT    fx_media_abort(FX_MEDIA *media_ptr); 
UINT    fx_media_cache_invalidate(FX_MEDIA *media_ptr);
UINT    fx_media_check(FX_MEDIA *media_ptr, UCHAR *scratch_memory_ptr, 
                       ULONG scratch_memory_size, ULONG error_correction_option,
                       ULONG *errors_detected_ptr);

UINT    fx_media_close(FX_MEDIA *media_ptr);
UINT    fx_media_close_notify_set(FX_MEDIA *media_ptr, VOID (*media_close_notify)(FX_MEDIA *media));
UINT    fx_media_extended_space_available(FX_MEDIA *media_ptr, ULONG64 *available_bytes_ptr);
UINT    fx_media_flush(FX_MEDIA *media_ptr);
UINT    fx_media_format(FX_MEDIA *media_ptr, VOID (*driver)(FX_MEDIA *media), 
                        VOID *driver_info_ptr, UCHAR *memory_ptr, 
                        UINT memory_size, CHAR *volume_name,
                        UINT number_of_fats, UINT directory_entries, 
                        UINT hidden_sectors, ULONG total_sectors,
                        UINT bytes_per_sector, UINT sectors_per_cluster,
                        UINT heads, UINT sectors_per_track);

UINT    fx_media_open(FX_MEDIA *media_ptr, CHAR *media_name,
                      VOID (*media_driver)(FX_MEDIA *), 
                      VOID *driver_info_ptr, VOID *memory_ptr,
                      ULONG memory_size);

UINT    fx_media_open_notify_set(FX_MEDIA *media_ptr, VOID (*media_open_notify)(FX_MEDIA *media));

UINT    fx_media_read(FX_MEDIA *media_ptr, ULONG logical_sector, VOID *buffer_ptr);

UINT    fx_media_space_available(FX_MEDIA *media_ptr, ULONG *available_bytes_ptr);
UINT    fx_media_volume_get(FX_MEDIA *media_ptr, CHAR *volume_name, UINT volume_source);

UINT    fx_media_volume_get_extended(FX_MEDIA *media_ptr, CHAR *volume_name,
                                     UINT volume_name_buffer_length, UINT volume_source);

UINT    fx_media_volume_set(FX_MEDIA *media_ptr, CHAR *volume_name);
UINT    fx_media_write(FX_MEDIA *media_ptr, ULONG logical_sector, VOID *buffer_ptr);

```

## Directory Services

```c 
UINT    fx_directory_attributes_read(FX_MEDIA *media_ptr, CHAR *directory_name,
                                     UINT *attributes_ptr);

UINT    fx_directory_attributes_set(FX_MEDIA *media_ptr, CHAR *directory_name,
                                    UINT attributes);

UINT    fx_directory_create(FX_MEDIA *media_ptr, CHAR *directory_name);
UINT    fx_directory_default_get(FX_MEDIA *media_ptr, CHAR **return_path_name);
UINT    fx_directory_default_set(FX_MEDIA *media_ptr, CHAR *new_path_name);
UINT    fx_directory_delete(FX_MEDIA *media_ptr, CHAR *directory_name);
UINT    fx_directory_first_entry_find(FX_MEDIA *media_ptr, CHAR *directory_name);
UINT    fx_directory_first_full_entry_find(FX_MEDIA *media_ptr, CHAR *directory_name,
                                           UINT *attributes, ULONG *size, 
                                           UINT *year, UINT *month, UINT *day, 
                                           UINT *hour, UINT *minute, UINT *second);

UINT    fx_directory_information_get(FX_MEDIA *media_ptr, CHAR *directory_name, 
                                      UINT *attributes, ULONG *size,
                                      UINT *year, UINT *month, UINT *day,
                                      UINT *hour, UINT *minute, UINT *second);
UINT    fx_directory_local_path_clear(FX_MEDIA *media_ptr);
UINT    fx_directory_local_path_get(FX_MEDIA *media_ptr, CHAR **return_path_name);
UINT    fx_directory_local_path_restore(FX_MEDIA *media_ptr, FX_LOCAL_PATH *local_path_ptr);
UINT    fx_directory_local_path_set(FX_MEDIA *media_ptr, FX_LOCAL_PATH *local_path_ptr,
                                    CHAR *new_path_name);

UINT    fx_directory_long_name_get(FX_MEDIA *media_ptr, CHAR *short_name, CHAR *long_name);

UINT    fx_directory_long_name_get_extended( FX_MEDIA *media_ptr, CHAR *short_name, 
                                             CHAR *long_name, UINT long_file_name_buffer_length);

UINT    fx_directory_name_test(FX_MEDIA *media_ptr, CHAR *directory_name);
UINT    fx_directory_next_entry_find(FX_MEDIA *media_ptr, CHAR *directory_name);
UINT    fx_directory_next_full_entry_find(FX_MEDIA *media_ptr, CHAR *directory_name, 
                                          UINT *attributes, ULONG *size,
                                          UINT *year,UINT *month, UINT *day, 
                                          UINT *hour, UINT *minute, UINT *second);

UINT    fx_directory_rename(FX_MEDIA *media_ptr, CHAR *old_directory_name,
                            CHAR *new_directory_name);

UINT    fx_directory_short_name_get(FX_MEDIA *media_ptr, CHAR *long_name, 
                                    CHAR *short_name);

UINT    fx_directory_short_name_get_extended(FX_MEDIA *media_ptr, CHAR *long_name, 
                                             CHAR *short_name, UINT short_file_name_length);
```

## File Services

```c
UINT    fx_fault_tolerant_enable(FX_MEDIA *media_ptr, VOID *memory_buffer, UINT memory_size);
UINT    fx_file_allocate(FX_FILE *file_ptr, ULONG size);
UINT    fx_file_attributes_read(FX_MEDIA *media_ptr, CHAR *file_name, UINT *attributes_ptr);
UINT    fx_file_attributes_set(FX_MEDIA *media_ptr, CHAR *file_name, UINT attributes);
UINT    fx_file_best_effort_allocate(FX_FILE *file_ptr, ULONG size, ULONG *actual_size_allocated);
UINT    fx_file_close(FX_FILE *file_ptr);
UINT    fx_file_create(FX_MEDIA *media_ptr, CHAR *file_name);
UINT    fx_file_date_time_set(FX_MEDIA *media_ptr, CHAR *file_name, 
                              UINT year, UINT month, UINT day,
                              UINT hour, UINT minute, UINT second);
UINT    fx_file_delete(FX_MEDIA *media_ptr, CHAR *file_name);
UINT    fx_file_extended_allocate(FX_FILE *file_ptr, ULONG64 size);
UINT    fx_file_extended_best_effort_allocate(FX_FILE *file_ptr, ULONG64 size, 
                                              ULONG64 *actual_size_allocated);
UINT    fx_file_extended_relative_seek(FX_FILE *file_ptr, ULONG64 byte_offset,
                                       UINT seek_from);
UINT    fx_file_extended_seek(FX_FILE *file_ptr, ULONG64 byte_offset);
UINT    fx_file_extended_truncate(FX_FILE *file_ptr, ULONG64 size);
UINT    fx_file_extended_truncate_release(FX_FILE *file_ptr, ULONG64 size);
UINT    fx_file_open(FX_MEDIA *media_ptr, FX_FILE *file_ptr,
                     CHAR *file_name, UINT open_type);
UINT    fx_file_read(FX_FILE *file_ptr, VOID *buffer_ptr,
                     ULONG request_size, ULONG *actual_size);
UINT    fx_file_relative_seek(FX_FILE *file_ptr, ULONG byte_offset, UINT seek_from);
UINT    fx_file_rename(FX_MEDIA *media_ptr, CHAR *old_file_name,
                       CHAR *new_file_name);
UINT    fx_file_seek(FX_FILE *file_ptr, ULONG byte_offset);
UINT    fx_file_truncate(FX_FILE *file_ptr, ULONG size);
UINT    fx_file_truncate_release(FX_FILE *file_ptr, ULONG size);
UINT    fx_file_write(FX_FILE *file_ptr, VOID *buffer_ptr, ULONG size);

UINT    fx_file_write_notify_set(FX_FILE *file_ptr, VOID (*file_write_notify)(FX_FILE *file));
```

## Unicode Services

```c
UINT    fx_unicode_directory_create(FX_MEDIA *media_ptr, UCHAR *source_unicode_name,
                                    ULONG source_unicode_length, CHAR *short_name);

UINT    fx_unicode_directory_rename(FX_MEDIA *media_ptr, UCHAR *old_unicode_name,
                                    ULONG old_unicode_length, UCHAR *new_unicode_name,
                                    ULONG new_unicode_length, CHAR *new_short_name);

UINT    fx_unicode_file_create(FX_MEDIA *media_ptr, UCHAR *source_unicode_name,
                               ULONG source_unicode_length, CHAR *short_name);

UINT    fx_unicode_file_rename(FX_MEDIA *media_ptr, UCHAR *old_unicode_name,
                               ULONG old_unicode_length, UCHAR *new_unicode_name,
                               ULONG new_unicode_length, CHAR *new_short_name);

ULONG    fx_unicode_length_get(UCHAR *unicode_name);


UINT    fx_unicode_length_get_extended(UCHAR *unicode_name, UINT buffer_length);
UINT    fx_unicode_name_get(FX_MEDIA *media_ptr, CHAR *source_short_name,
                            UCHAR *destination_unicode_name, ULONG *destination_unicode_length);

UINT    fx_unicode_name_get_extended(FX_MEDIA *media_ptr, CHAR *source_short_name,
                                     UCHAR *destination_unicode_name, ULONG *destination_unicode_length,
                                     ULONG unicode_name_buffer_length);

UINT    fx_unicode_short_name_get(FX_MEDIA *media_ptr, UCHAR *source_unicode_name,
                                  ULONG source_unicode_length, CHAR *destination_short_name);

UINT    fx_unicode_short_name_get_extended(FX_MEDIA *media_ptr, UCHAR *source_unicode_name,
                                           ULONG source_unicode_length, CHAR *destination_short_name,
                                           ULONG short_name_buffer_length);
```

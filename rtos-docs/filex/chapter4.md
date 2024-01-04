---
title: Chapter 4 - Description of FileX services
description: This chapter contains a description of all FileX services in alphabetic order.
---

# Chapter 4- Description of FileX services

This chapter contains a description of all FileX services in alphabetic order. Service names are designed so all similar services are grouped together.

## fx_directory_attributes_read

Reads a specified directory's attributes.

### Prototype

```c
UINT fx_directory_attributes_read ( 
    FX_MEDIA *media_ptr,
    CHAR *directory_name,
    UINT *attributes_ptr);
```

### Description

This service reads the directory's attributes from the specified media.

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *directory_name*: Pointer to the name of the requested directory (directory path is optional).
- *attributes_ptr*: Pointer to the destination for the directory's attributes to be placed. The directory attributes are returned in a bit-map format with the following possible settings.
  - **FX_READ_ONLY** (0x01)
  - **FX_HIDDEN** (0x02)
  - **FX_SYSTEM** (0x04)
  - **FX_VOLUME** (0x08)
  - **FX_DIRECTORY** (0x10)
  - **FX_ARCHIVE** (0x20)

### Return Values

- **FX_SUCCESS** (0x00) Successful directory attributes read
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open
- **FX _NOT FOUND** (0x04) Specified directory was not found in the media
- **FX_NOT_DIRECTORY** (0x0E) Entry is not a directory
- **FX_IO_ERROR** (0x90) Driver I/O error
- **FX_FILE_CORRUPT** 0x08) File is corrupted
- **FX_SECTOR_INVALID** (0x89) Invalid sector
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_MEDIA_INVALID** (0x02) Invalid media
- **FX_PTR_ERROR** (0x18) Invalid media pointer
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA     my_media;
UINT     status;
/* Retrieve the attributes of "mydir" from the specified media.*/
status = fx_directory_attributes_read(&my_media, "mydir", &attributes);
/* If status equals FX_SUCCESS, "attributes" contains the directory attributes of "mydir". */
```

### See Also

- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_attributes_set

Sets a specified directory's attributes.

### Prototype

```c
UINT fx_directory_attributes_set(
    FX_MEDIA *media_ptr,
    CHAR *directory_name,
    UINT *attributes);
```

### Description

This service sets the directory's attributes to those specified by the caller.

> **Warning:** *This application is only allowed to modify a subset of the directory's attributes with this service. Any attempt to set additional attributes will result in an error.*

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *directory_name*: Pointer to the name of the requested directory (directory path is optional).
- *attributes*: The new attributes of  this directory. The valid directory attributes are defined as follows.
  - **FX_READ_ONLY** (0x01)
  - **FX_HIDDEN** (0x02)
  - **FX_SYSTEM** (0x04)
  - **FX_ARCHIVE** (0x20)

### Return Values

- **FX_SUCCESS** (0x00) Successful directory attribute set
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open
- **FX_NOT_FOUND** (0x04) Specified directory was not found in the media
- **FX_NOT_DIRECTORY** (0x0E) Entry is not a directory
- **FX_IO_ERROR** (0x90) Driver I/O error
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected
- **FX_FILE_CORRUPT** (0x08) File is corrupted
- **FX_SECTOR_INVALID** (0x89) Invalid sector
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_MEDIA_INVALID** (0x02) Invalid media
- **FX_NO_MORE_ENTRIES** (0x0F) No more entries in this directory
- **FX_PTR_ERROR** (0x18) Invalid media pointer
- **FX_INVALID_ATTR** (0x19) Invalid attributes selected.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA             my_media;
UINT                 status;
status = fx_directory_attributes_set(&my_media, "mydir", FX_READ_ONLY);
/*Set the attributes of "mydir" to read-only. */
/* If status equals FX_SUCCESS, the directory "mydir" is read-only. */
```

### See Also

- fx_directory_attributes_read
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_create

Creates a subdirectory

### Prototype

```c
UINT fx_directory_create(
    FX_MEDIA *media_ptr,
    CHAR *directory_name);
```

### Description

This service creates a subdirectory in the current default directory or in the path supplied in the directory name. Unlike the root directory, subdirectories do not have a limit on the number of files they can hold. The root directory can only hold the number of entries determined by the boot record.

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *directory_name*: Pointer to the name of the directory to create (directory path is optional).

### Return Values

- **FX_SUCCESS** (0x00) Successful directory create.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open
- **FX_NOT_FOUND** (0x04) Specified directory was not found in the media
- **FX_NOT_DIRECTORY** (0x0E) Entry is not a directory
- **FX_IO_ERROR** (0x90) Driver I/O error
- **FX_FILE _CORRUPT** (0x08) File is corrupted
- **FX_SECTOR_INVALID** (0x89) Invalid sector
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_MEDIA_INVALID** (0x02) Invalid media
- **FX_NO_MORE_ENTRIES** (0x0F) No more entries in this directory
- **FX_PTR_ERROR** (0x18) Invalid media pointer
- **FX_INVALID_ATTR** (0x19) Invalid attributes selected.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA             my_media;
UINT                 status;
/* Create a subdirectory called "temp" in the current default directory. */

status = fx_directory_create(&my_media, "temp");

/* If status equals FX_SUCCESS, the new subdirectory "temp" has been created. */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_default_get

Gets the last default directory

### Prototype

```c
UINT fx_directory_default_get(
    FX_MEDIA *media_ptr,
    CHAR **return_path_name);
```

### Description

This service returns the pointer to the path last set by ***fx_directory_default_set***. If the default directory has not been set or if the current default directory is the root directory, a value of **FX_NULL** is returned.

> **Important:** *The default size of the internal path string is 256 characters; it can be changed by modifying **FX_MAXIMUM_PATH** in **fx_api.h** and rebuilding the entire FileX library. The character string path is maintained for the application and is not used internally by FileX.*

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *return_path_name*: Pointer to the destination for the last default directory string. A value of **FX_NULL** is returned if the current setting of the default directory is the root. When the media is opened, root is the default.

### Return Values

- **FX_SUCCESS** (0x00) Successful default directory get
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open
- **FX_PTR_ERROR** (0x18) Invalid media or destination pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA my_media;
CHAR *current_default_dir;
UINT status;
/* Retrieve the current default directory. */
status = fx_directory_default_get(&my_media, &current_default_dir);
/* If status equals FX_SUCCESS, "current_default_dir" 
    contains a pointer to the current default directory).*/
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_default_set

Sets the default directory

### Prototype

```c

UINT fx_directory_default_set(
    FX_MEDIA *media_ptr,
    CHAR *new_path_name);
```

### Description

This service sets the default directory of the media. If a value of **FX_NULL** is supplied, the default directory is set to the media's root directory. All subsequent file operations that do not explicitly specify a path will default to this directory.

> **Important:** *The default size of the internal path string is 256 characters; it can be changed by modifying **FX_MAXIMUM_PATH** in **fx_api.h** and rebuilding the entire FileX library. The character string path is maintained for the application and is not used internally by FileX.*

> **Important:** *For names supplied by the application, FileX supports both backslash (\\) and forward slash (/) characters to separate directories, subdirectories, and file names. However, FileX only uses the backslash character in paths returned to the application.*

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *new_path_name*: Pointer to new default directory name. If a value of **FX_NULL** is supplied, the default directory of the media is set to the media's root directory.

### Return Values

- **FX_SUCCESS** (0x00)  Successful default directory set
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open
- **FX_INVALID_PATH** (0x0D) New directory could not be found
- **FX_PTR_ERROR** (0x18) Invalid media pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA     my_media;
UINT status;
/* Set the default directory to \abc\def\ghi. */
status = fx_directory_default_set(&my_media, "\\abc\\def\\ghi");
/* If status equals FX_SUCCESS, the default directory for this media is \abc\def\ghi. All subsequent file operations that do not explicitly specify a path will default to this directory. Note that the character "\" serves as an escape character in a string. To represent the character "\", use the construct "\\". This is done because of the C language- only one "\" is really present in the string. */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_delete

Deletes the specified subdirectory.

### Prototype

```c
UINT fx_directory_delete(
    FX_MEDIA *media_ptr,
    CHAR *directory_name);
```

### Description

This service deletes the specified directory. Note that the directory must be empty to delete it.

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *directory_name*: Pointer to name of directory to delete (directory path is optional).

### Return Values

- **FX_SUCCESS** (0x00) Successful directory delete
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open
- **FX_NOT_FOUND** (0x04) Specified directory was not found
- **FX_DIR_NOT_EMPTY** (0x10) Specified directory is not empty
- **FX_IO_ERROR** (0x90) Driver I/O error
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected
- **FX_FILE_CORRUPT** (0x08) File is corrupted
- **FX_SECTOR_INVALID** (0x89) Invalid sector
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_MEDIA_INVALID** (0x02) Invalid media
- **FX_NO_MORE_ENTRIES** (0x0F) No more entries in this directory
- **FX_NOT_DIRECTORY** (0x0E) Not a directory entry
- **FX_PTR_ERROR** (0x18) Invalid media pointer
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA     my_media;
UINT status;
/* Set the default directory to \abc\def\ghi. */
status = fx_directory_delete(&my_media, "abc");
/* Delete the subdirectory "abc." */
/* If status equals FX_SUCCESS, the subdirectory "abc" was deleted. */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_first_entry_find

Gets the first directory entry.

### Prototype

```c
UINT fx_directory_first_entry_find(
    FX_MEDIA *media_ptr,
    CHAR *return_entry_name);
```

### Description

This service retrieves the first entry name in the default directory and copies it to the specified destination.

> **Warning:** *The specified destination must be large enough to hold the maximum sized FileX name, as defined by **FX_MAX_LONG_NAME_LEN.***

> **Warning:** *If using a non-local path, it is important to prevent (with a ThreadX semaphore, mutex, or priority level change) other application threads from changing this directory while a directory traversal is taking place. Otherwise, invalid results may be obtained.*

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *return_entry_name*: Pointer to destination for the first entry name in the default directory.

### Return Values

- **FX_SUCCESS** (0x00) Successful first directory entry find
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open
- **FX_NO_MORE_ENTRIES** (0x0F) No more entries in this directory
- **FX_IO_ERROR** (0x90) Driver I/O error
- **FX_FILE_CORRUPT** (0x08) File is corrupted
- **FX_SECTOR_INVALID** (0x89) Invalid sector
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry
- **FX_PTR_ERROR** (0x18) Invalid media or destination pointer
- **FX_CALLER_ERROR** (0x20) Caller is not a thread

### Allowed From

Threads

### Example

```c
FX_MEDIA         my_media;
UINT             status;
CHAR             entry[FX_MAX_LONG_NAME_LEN];
/* Retrieve the first directory entry in the current directory. */
status = fx_directory_first_entry_find(&my_media, entry);
/* If status equals FX_SUCCESS, the entry in the directory is the "entry" string. */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_first_full_entry_find

Gets the first directory entry with full information.

### Prototype

```c
UINT fx_directory_first_full_entry_find(
    FX_MEDIA *media_ptr,
    CHAR *directory_name,
    UINT *attributes,
    ULONG *size,
    UINT *year, UINT *month, UINT *day,
    UINT *hour, UINT *minute, UINT *second);
```

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *directory_name*: Pointer to the destination for the name of a directory entry. Must be at least as big as **FX_MAX_LONG_NAME_LEN**.
- *attributes*: If non-null, pointer to the destination for the entry's attributes to be placed. The attributes are returned in a bit-map format with the following possible settings.
  - **FX_READ_ONLY** (0x01)
  - **FX_HIDDEN** (0x02)
  - **FX_SYSTEM** (0x04)
  - **FX_VOLUME** (0x08)
  - **FX_DIRECTORY** (0x10)
  - **FX_ARCHIVE** (0x20)
- *size*: If non-null, a pointer to the destination for the entry's size in bytes.
- *year*: If non-null, a pointer to the destination for the entry's year of modification.
- *month*: If non-null, a pointer to the destination for the entry's month of modification.
- *day*: If non-null, a pointer to the destination for the entry's day of modification.
- *hour*: If non-null, a pointer to the destination for the entry's hour of modification.
- *minute*: If non-null, a pointer to the destination for the entry's minute of modification.
- *second*: If non-null, a pointer to the destination for the entry's second of modification.

### Return Values

- **FX_SUCCESS** (0x00) Successful first directory entry find
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open
- **FX_NO_MORE_ENTRIES** (0x0F) No more entries in this directory
- **FX_IO_ERROR** (0x90) Driver I/O error
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected
- **FX_FILE _CORRUPT** (0x08) File is corrupted
- **FX_SECTOR_INVALID** (0x89) Invalid sector
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_MEDIA_INVALID** (0x02) Invalid media
- **FX_PTR_ERROR** (0x18) Invalid media or destination pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA     my_media;
UINT         status;
CHAR         entry_name[FX_MAX_LONG_NAME_LEN];
UINT         attributes;
ULONG        size;
UINT         year;
UINT         month;
UINT         day;
UINT         hour;
UINT         minute;
UINT         second;
/* Get the first directory entry in the default directory with full information. */
status = fx_directory_first_full_entry_find(&my_media, entry_name,
    &attributes, &size, &year, &month, &day, &hour, &minute, &second);

/* If status equals FX_SUCCESS, the entry's information is in the local variables. */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_information_get

Gets a directory's  entry information

### Prototype

```c
UINT fx_directory_first_full_entry_find(
    FX_MEDIA *media_ptr,
    CHAR *directory_name,
    UINT *attributes,
    ULONG *size,
    UINT *year, UINT *month, UINT *day,
    UINT *hour, UINT *minute, UINT *second);
```

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *directory_name*: Pointer to name of the directory entry.
- *attributes*: Pointer to the destination for the attributes.
- *size*: Pointer to the destination for the size.
- *year*: Pointer to the destination for the year.
- *month*: Pointer to the destination for the month.
- *day*: Pointer to the destination for the day.
- *hour*: Pointer to the destination for the hour.
- *minute*: Pointer to the destination for the minute.
- *second*: Pointer to the destination for the second.

### Return Values

- **FX_SUCCESS** (0x00) Successful first directory entry find
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open
- **FX_NOT_FOUND** (0x04) Specified directory was not found in the media
- **FX_IO_ERROR** (0x90) Driver I/O error
- **FX_MEDIA_INVALID** (0x02) Invalid media
- **FX_FILE _CORRUPT** (0x08) File is corrupted
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_SECTOR_INVALID** (0x89) Invalid sector
- **FX_PTR_ERROR** (0x18) Invalid media or destination pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA     my_media;
UINT         status; attributes; year; month; day;
CHAR         entry_name[FX_MAX_LONG_NAME_LEN];
ULONG        size;
UINT         hour; minute; second;
/* Retrieve information about the directory entry "myfile.txt".*/
status = fx_directory_information_get(&my_media, "myfile.txt", &attributes, &size,
                                      &year, &month, &day,
                                      &hour, &minute, &second);
/* If status equals FX_SUCCESS, the directory entry information is available in the local variables. */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_local_path_clear

Clears the default local path

### Prototype

```c
UINT fx_directory_local_path_clear(FX_MEDIA *media_ptr);
```

### Description

This service clears the previous local path set up for the calling thread.

### Input Parameters

- *media_ptr*: Pointer to a previously opened media.

### Return Values

- **FX_SUCCESS** (0x00) Successful local path clear.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not currently open
- **FX_NOT_IMPLEMENTED** (0x22) FX_NO_LCOAL_PATH is defined
- **FX_PTR_ERROR** (0x18) Invalid media pointer

### Allowed From

Threads

### Example

```c
FX_MEDIA             my_media;
UINT                 status;
/* Clear the previously setup local path for this media. */
status = fx_directory_local_path_clear(&my_media);
/* If status equals FX_SUCCESS the local path is cleared. */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_local_path_get

Gets the current local path string.

### Prototype

```c
UINT fx_directory_local_path_clear(
    FX_MEDIA *media_ptr,
    CHAR **return_path_name);
```

### Description

This service returns the local path pointer of the specified media. If there is no local path set, a **NULL** is returned to the caller.

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *return_path_name*: Pointer to the destination string pointer for the local path string to be stored.

### Return Values

- **FX_SUCCESS** (0x00) Successful local path get.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not currently open
- **FX_NOT_IMPLEMENTED** (0x22) NX_NO_LCOAL_PATH
- **FX_PTR_ERROR** (0x18) Invalid media pointer

### Allowed From

Threads

### Example

```c
FX_MEDIA         my_media;
CHAR             *my_path;
UINT             status;
/* Retrieve the current local path string. */
status = fx_directory_local_path_get(&my_media, &my_path);
/* If status equals FX_SUCCESS, "my_path" points to the local path string. */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_local_path_restore

Restores the previous local path.

### Prototype

```c
UINT fx_directory_local_path_restore(
    FX_MEDIA *media_ptr,
    FX_LOCAL_PATH *local_path_ptr);
```

### Description

This service restores a previously set local path. The directory search position made on this local path is also restored, which makes this routine useful in recursive directory traversals by the application.

> **Important:** *Each local path contains a local path string of **FX_MAXIMUM_PATH** in size, which by default is 256 characters. This internal path string is not used by FileX and is provided only for the application's use. If **FX_LOCAL_PATH** is going to be declared as a local variable, users should beware of the stack growing by the size of this structure. Users are welcome to reduce the size of **FX_MAXIMUM_PATH** and rebuild the FileX library source.*

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *local_path_ptr*: Pointer to the previously set local path. It's very important to ensure that this pointer does indeed point to a previously used and still intact local path.

### Return Values

- **FX_SUCCESS** (0x00) Successful local path restore.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not currently open.
- **FX_NOT_IMPLEMENTED** (0x22) FX_NO_LCOAL_PATH is defined.
- **FX_PTR_ERROR** (0x18) Invalid media or local path pointer.

### Allowed From

Threads

### Example

```c
FX_MEDIA                  my_media;
FX_LOCAL_PATH             my_previous_local_path;
UINT                      status;
/* Restore the previous local path. */
status = fx_directory_local_path_restore(&my_media, &my_previous_local_path);
/* If status equals FX_SUCCESS, the previous local path has been restored. */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_local_path_set

Sets up a thread-specific local path

### Prototype

```c
UINT fx_directory_local_path_set(
    FX_MEDIA *media_ptr,
    FX_LOCAL_PATH *local_path_ptr,
    CHAR *new_path_name);
```

### Description

This service sets up a thread-specific path as specified by the *new_path_string*. After successful completion of this routine, the local path information stored in *local_path_ptr* will take precedence over the global media path for all file and directory operations made by this thread. This will have no impact on any other thread in the system.

> **Important:** *The default size of the local path string is 256 characters; it can be changed by modifying **FX_MAXIMUM_PATH** in ***fx_api.h*** and rebuilding the entire FileX library. The character string path is maintained for the application and is not used internally by FileX.*

> **Important:** *For names supplied by the application, FileX supports both backslash (\\) and forward slash (/) characters to separate directories, subdirectories, and file names. However, FileX only uses the backslash character in paths returned to the application.*

### Input Parameters

- *media_ptr*: Pointer to the previously opened media.
- *local_path_ptr*: Destination for holding the thread-specific local path information. The address of this structure may be supplied to the local path restore function in the future.
- *new_path_name*: Specifies the local path to setup.

### Return Values

- **FX_SUCCESS** (0x00) Successful default directory set.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_NOT_IMPLEMENTED** (0x22) **FX_NO_LOCAL_PATH**
- **FX_INVALID_PATH** (0x0D) New directory could not be found.
- **FX_NOT_IMPLEMENTED** (0x22)- **FX_NO_LOCAL_PATH** is defined.
- **FX_PTR_ERROR** (0x18) Invalid media or local path pointer.

### Allowed From

Threads

### Example

```c
FX_MEDIA         my_media;
UINT             status;
FX_LOCAL_PATH     my_previous_local_path;
/* Set the local path to \abc\def\ghi. */
status = fx_directory_local_path_set (&my_media,&local_path,"\\abc\\def\\ghi");

/* If status equals FX_SUCCESS, the default directory for this thread
is \abc\def\ghi. All subsequent file operations that do not explicitly
specify a path will default to this directory. Note that the character
"\" serves as an escape character in a string. To represent the
character "\", use the construct "\\".*/
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_long_name_get

Gets the long name of a directory from its short name.

### Prototype

```c
UINT fx_directory_long_name_get(
    FX_MEDIA *media_ptr,
    CHAR *short_name,
    CHAR *long_name);
```

### Description

This service retrieves the long name (if any) associated with the supplied short (8.3 format) name. The short name can be either a file name or a directory name.

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *short_name*: Pointer to the source short name (8.3 format).
- *long_name*: Pointer to the destination for the long name. If there is no long name, the short name is returned. Note that the destination for the long name must be large enough to hold **FX_MAX_LONG_NAME_LEN** characters.

### Return Values

- **FX_SUCCESS** (0x00) Successful long name get
- **FX_NOT_FOUND** (0x04) Short name was not found
- **FX_IO_ERROR** (0x90) Driver I/O error
- **FX_MEDIA_INVALID** (0x02) Invalid media
- **FX_FILE_CORRUPT** (0x08) File is corrupted
- **FX_SECTOR_INVALID** (0x89) Invalid sector
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_PTR_ERROR** (0x18) Invalid media or name pointer
- **FX_CALLER_ERROR** (0x20) Caller is not a thread

### Allowed From

Threads

### Example

```c
FX_MEDIA            my_media;
UCHAR               my_long_name[FX_MAX_LONG_NAME_LEN];
/* Retrieve the long name associated with "TEXT~01.TXT". */
status = fx_directory_long_name_get(&my_media, "TEXT~01.TXT", my_long_name);
/* If status is FX_SUCCESS the long name was successfully retrieved. */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_long_name_get_extended

Gets the long name and extended information from short name.

### Prototype

```c
UINT fx_directory_long_name_get_extended(
    FX_MEDIA *media_ptr,
    CHAR *short_name,
    CHAR *long_name,
    UINT long_file_name_buffer_length);
```

### Description

This service retrieves the long name (if any) associated with the supplied short (8.3 format) name. The short name can be either a file name or a directory name.

### Input Parameters

- *media_ptr*: Pointer to media control block.
- *short_name*: Pointer to source short name (8.3 format).
- *long_name*: Pointer to destination for the long name. If there is no long name, the short name is returned. Note: Destination for the long name must be large enough to hold **FX_MAX_LONG_NAME_LEN** characters.
- *long_file_name_buffer_length*: Length of the long name buffer.

### Return Values

- **FX_SUCCESS** (0x00) Successful long name get.
- **FX_NOT_FOUND** (0x04) Short name was not found.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_MEDIA_INVALID** (0x02) Invalid media.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation.
- **FX_PTR_ERROR** (0x18) Invalid media or name pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA        my_media;
UCHAR            my_long_name[FX_MAX_LONG_NAME_LEN];
/* Retrieve the long name associated with "TEXT~01.TXT". */

status = fx_directory_long_name_get_extended(&my_media,
    "TEXT~01.TXT", my_long_name, sizeof(my_long_name));

/* If status is FX_SUCCESS the long name was successfully retrieved. */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_name_test

Tests for the specified directory.

### Prototype

```c
UINT fx_directory_name_test(
    FX_MEDIA *media_ptr,
    CHAR *directory_name);
```

### Description

This service tests whether or not the supplied name is a directory. If so, a **FX_SUCCESS** is returned.

### Input Parameters

- *media_ptr*: Pointer to the media control block.
- *directory_name*: Pointer to the name of the directory entry.

### Return Values

- **FX_SUCCESS** (0x00) Supplied name is a directory.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open
- **FX_NOT_FOUND** (0x04) Directory entry could not be found.
- **FX_NOT_DIRECTORY** (0x0E)  Entry is not a directory
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_MEDIA_INVALID** (0x02) Invalid media.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation.
- **FX_PTR_ERROR** (0x18) Invalid media or name pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA        my_media;
UNIT             status;
/* Check to see if the name "abc" is directory */

status = fx_directory_name_test(&my_media, "abc");

/* If status equals FX_SUCCESS "abc" is a directory. */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create

## fx_directory_next_entry_find

Finds the next directory entry.

### Prototype

```c
UINT fx_directory_next_entry_find(
    FX_MEDIA *media_ptr,
    CHAR *return_entry_name);
```

### Description

This service returns the next entry name in the current default directory.

> **Warning:** *If using a non-local path, it is also important to prevent (with a ThreadX semaphore or thread priority level) other application threads from changing this directory while a directory traversal is taking place. Otherwise, invalid results may be obtained.*

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *return_entry_name*: Pointer to destination for the next entry name in the default directory. The buffer this pointer points to must be large enough to hold the maximum size of FileX name, defined by **FX_MAX_LONG_NAME_LEN**.

### Return Values

- **FX_SUCCESS** (0x00) Successful next entry find
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_NO_MORE_ENTRIES** (0x0F) No more entries in this directory.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_PTR_ERROR** (0x18) Invalid media pointer.
- **FX_CALLER_ERROR** (0x20)     Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA        my_media;
CHAR            next_name[FX_MAX_LONG_NAME_LEN];
UINT            status;

/* Retrieve the next entry in the default directory. */

status = fx_directory_next_entry_find(&my_media, next_name);

/* If status equals TX_SUCCESS, the name of the next directory entry is in "next_name". */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_next_full_entry_find

Gets the next directory entry with its full information.

### Prototype

```c
UINT fx_directory_next_full_entry_find(
    FX_MEDIA *media_ptr,
    CHAR *directory_name,
    UINT *attributes, 
    ULONG *size,
    UINT *year, 
    UINT *month, 
    UINT *day,
    UINT *hour, 
    UINT *minute, 
    UINT *second);
```

### Description

This service retrieves the next entry name in the default directory and copies it to the specified destination. It also returns full information about the entry as specified by the additional input parameters.

> **Warning:** *The specified destination must be large enough to hold the maximum sized FileX name, as defined by **FX_MAX_LONG_NAME_LEN***

> **Warning:** *If using a non-local path, it is important to prevent (with a ThreadX semaphore, mutex, or priority level change) other application threads from changing this directory while a directory traversal is taking place. Otherwise, invalid results may be obtained.*

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *directory_name*: Pointer to the destination for the name of a directory entry. Must be at least as big as **FX_MAX_LONG_NAME_LEN**.
- *attributes*: If non-null, pointer to the destination for the entry's attributes to be placed.The attributes are returned in a bit-map format with the following possible settings.
  - **FX_READ_ONLY** (0x01)
  - **FX_HIDDEN** (0x02)
  - **FX_SYSTEM** (0x04)
  - **FX_VOLUME** (0x08)
  - **FX_DIRECTORY** (0x10)
  - **FX_ARCHIVE** (0x20)
- *size*: If non-null, pointer to the destination for the entry's size in bytes.
- *month*: If non-null, pointer to the destination for the entry's month of modification.
- *year*: If non-null, pointer to the destination for the entry's year of modification.
- *day*: If non-null, pointer to the destination for the entry's day of modification.
- *hour*: If non-null, pointer to the destination for the entry's hour of modification.
- *minute*: If non-null, pointer to the destination for the entry's minute of modification.
- *second*: If non-null, pointer to the destination for the entry's second of modification.

### Return Values

- **FX_SUCCESS** (0x00) Successful directory next entry find.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_NO_MORE_ENTRIES** (0x0F) No more entries in this directory.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation.
- **FX_MEDIA_INVALID** (0x02) Invalid media.
- **FX_PTR_ERROR** (0x18) Invalid media pointer or all input parameters are NULL.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA    my_media;
UINT        status;
CHAR        entry_name[FX_MAX_LONG_NAME_LEN];
UINT        attributes;
ULONG       size;
UINT        year;
UINT        month;
UINT        day;
UINT        hour;
UINT        minute;
UINT        second;

/* Get the next directory entry in the default directory with full information. */
status = fx_directory_next_full_entry_find(&my_media, entry_name, &attributes, &size,
                                           &year, &month, &day,
                                           &hour, &minute, &second);

/* If status equals FX_SUCCESS, the entry's information is in the local variables. */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_rename

Renames the specified directory.

### Prototype

```c
UINT fx_directory_rename(
    FX_MEDIA *media_ptr,
    CHAR *old_directory_name,
    CHAR *new_directory_name);
```

### Description

This service changes the directory name to the specified new directory name. Renaming is also done relative to the specified path or the default path. If a path is specified in the new directory name, the renamed directory is effectively moved to the specified path. If no path is specified, the renamed directory is placed in the current default path.

### Input Parameters

- *media_ptr*: Pointer to media control block.
- *old_directory_name*: Pointer to current directory name.
- *new_directory_name*: Pointer to new directory name.

### Return Values

- **FX_SUCCESS** (0x00) Successful directory rename.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_NOT_FOUND** (0x04) Directory entry could not be found.
- **FX_NOT_DIRECTORY** (0x0E) Entry is not a directory.
- **FX_INVALID_NAME** (0x0C) New directory name is invalid.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation.
- **FX_MEDIA_INVALID** (0x02) Invalid media.
- **FX_NO_MORE_ENTRIES** (0x0F) No more entries in this directory.
- **FX_INVALID_PATH** (0x0D) Invalid path supplied with directory name.
- **FX_ALREADY_CREATED** (0x0B) Specified directory was already created.
- **FX_PTR_ERROR** (0x18) Invalid media pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA         my_media;
UINT             status;

/* Change the directory "abc" to "def". */
status = fx_directory_rename(&my_media, "abc", "def");

/* If status equals FX_SUCCESS, the directory was changed to "def". */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_short_name_get
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_short_name_get

Gets the specified directory's short name from its long name.

### Prototype

```c
UINT fx_directory_short_name_get(
    FX_MEDIA *media_ptr,
    CHAR *long_name, 
    CHAR *short_name);
```

### Description

This service retrieves the short (8.3 format) name associated with the supplied long name. The long name can be either a file name or a directory name.

### Input Parameters

- *media_ptr*: Pointer to media control block.
- *long_name*: Pointer to source long name.
- *short_name*: Pointer to destination short name (8.3 format). Note that the destination for the short name must be large enough to hold 14 characters.

### Return Values

- **FX_SUCCESS** (0x00) Successful short name get.
- **FX_NOT_FOUND** (0x04) Long name was not found.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_MEDIA_INVALID** (0x02) Invalid media.
- **FX_PTR_ERROR** (0x18) Invalid media or name pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA         my_media;
UCHAR             my_short_name[14];

/* Retrieve the short name associated with "my_really_long_name". */

status = fx_directory_short_name_get(&my_media,
    "my_really_long_name", my_short_name);

/* If status is FX_SUCCESS the short name was successfully retrieved. */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_directory_short_name_get_extended

Gets the specified directory's short name from its long name.

### Prototype

```csharp
UINT fx_directory_short_name_get_extended(
    FX_MEDIA *media_ptr,
    CHAR *long_name,
    CHAR *short_name,
    UINT short_file_name_length);
```

### Description

This service retrieves the short (8.3 format) name associated with the supplied long name. The long name can be either a file name or a directory name.

### Input Parameters

- *media_ptr*: Pointer to media control block.
- *long_name*: Pointer to source long name.
- *short_name*: Pointer to destination short name (8.3 format). Note: Destination for the short name must be large enough to hold 14 characters.
- *short_file_name_length*: Length of short name buffer.

### Return Values

- **FX_SUCCESS** (0x00) Successful short name get.
- **FX_NOT_FOUND** (0x04) Long name was not found.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_MEDIA_INVALID** (0x02) Invalid media.
- **FX_PTR_ERROR** (0x18) Invalid media or name pointer.
- **FX_CALLER_ERROR** (0x20)    Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA        my_media;
UCHAR            my_short_name[14];

/* Retrieve the short name associated with "my_really_long_name". */

status = fx_directory_short_name_get_extended(&my_media,
    "my_really_long_name", my_short_name, sizeof(my_short_name));

/* If status is FX_SUCCESS the short name was successfully retrieved. */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_unicode_directory_create
- fx_unicode_directory_rename

## fx_fault_tolerant_enable

Enables the fault tolerant service.

### Prototype

```csharp
UINT fx_fault_tolerant_enable(
    FX_MEDIA *media_ptr,
    VOID *memory_buffer,
    UINT memory_size);
```

### Description

This service enables the fault tolerant module. Upon starting, the fault tolerant module detects whether or not the file system is under FileX fault tolerant protection. If it is not, the service finds available sectors on the file system to store logs on file system transactions. If the file system is under FileX fault tolerant protection, it applies the logs to the file system to maintain its integrity.

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *memory_ptr*: Pointer to a block of memory used by the fault tolerant module as scratch memory.
- *memory_size*: The size of the scratch memory. In order for fault tolerant to work properly, the scratch memory size shall be at least 3072 bytes,- and must be multiple of sector size.

### Return Values

- **FX_SUCCESS** (0x00) Successfully enabled fault tolerant.
- **FX_NOT_ENOUGH_MEMORY** (0x91)    memory size too small.
- **FX_BOOT_ERROR** (0x01) Boot sector error.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_NO_MORE_ENTRIES** (0x0F) No more free cluster available.
- **FX_NO_MORE_SPACE** (0x0A) Media associated with this file does not have enough available clusters.
- **FX_SECTOR_INVALID** (0x89) Sector is invalid
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_PTR_ERROR** (0x18) Invalid media pointer.
- **FX_CALLER_ERROR** (0x20)    Caller is not a thread.

### Allowed From

Initialization, threads

### Example

```c

/* Declare memory space used for fault tolerant. */

ULONG fault tolerant_memory[3072 / sizeof(ULONG)];

/* Enable fault tolerant. */

fx_fault_tolerant_enable(media_ptr, fault_tolerant_memory, sizeof(fault_tolerant_memory));

```

### See Also

- fx_system_initialize
- fx_media_abort
- fx_media_cache_invalidate
- fx_media_check
- fx_media_close
- fx_media_close_notify_set
- fx_media_exFAT_format
- fx_media_extended_space_available
- fx_media_flush
- fx_media_format
- fx_media_open
- fx_media_open_notify_set
- fx_media_read
- fx_media_space_available
- fx_media_volume_get
- fx_media_volume_set
- fx_media_write

## fx_file_allocate

Allocates space for a file

### Prototype

```csharp
UINT fx_file_allocate(
    FX_FILE *file_ptr, 
    ULONG size);
```

### Description

This service allocates and links one or more contiguous clusters to the end of the specified file. FileX determines the number of clusters required by dividing the requested size by the number of bytes per cluster. The result is then rounded up to the next whole cluster.

To allocate space beyond 4GB, application shall use the service ***fx_file_extended_allocate***.

### Input Parameters

- *file_ptr*: Pointer to a previously opened file.
- *size*: Number of bytes to allocate for the file.

### Return Values

- **FX_SUCCESS** (0x00) Successful file allocation.
- **FX_ACCESS_ERROR** (0x06) Specified file is not open for writing.
- **FX_FAT_READ_ERROR** (0x03) Failed to read FAT entry.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_NOT_OPEN** (0x07) Specified file is not currently open.
- **FX_NO_MORE_ENTRIES** (0x0F) No more free cluster available.
- **FX_NO_MORE_SPACE** (0x0A) Media associated with this file does not have enough available clusters.
- **FX_SECTOR_INVALID** (0x89) Sector is invalid
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_PTR_ERROR** (0x18) Invalid file pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c

FX_FILE            my_file;
UINT            status;

/* Allocate 1024 bytes to the end of my_file. */

status = fx_file_allocate(&my_file, 1024);

/* If status equals FX_SUCCESS the file now has one or more
    contiguous cluster(s) that can accommodate at least 1024 bytes of user data. */
```

### See Also

- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open- fx_file_read
- fx_file_relative_seek
- fx_file_rename- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_attributes_read

Reads a file's attributes.

### Prototype

```c
    UINT fx_file_attributes_read(
    FX_MEDIA *media_ptr,
    CHAR *file_name,
    UINT *attributes_ptr);
```

### Description

This service reads the file's attributes from the specified media.

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *file_name*: Pointer to the name of the requested file (directory path is optional).
- *attributes_ptr*: Pointer to the destination for the file's attributes to be placed. The file attributes are returned in a bit-map format with the following possible settings.
  - **FX_READ_ONLY** (0x01)
  - **FX_HIDDEN** (0x02)
  - **FX_SYSTEM** (0x04)
  - **FX_VOLUME** (0x08)
  - **FX_DIRECTORY** (0x10)
  - **FX_ARCHIVE** (0x20)

### Return Values

- **FX_SUCCESS** (0x00) Successful attribute read.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_NOT_FOUND** (0x04) Specified file was not found in the media.
- **FX_NOT_A_FILE** (0x05) Specified file is a directory.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_NO_MORE_ENTRIES** (0x0F) No more FAT entries.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_PTR_ERROR** (0x18) Invalid media or attributes pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c

FX_MEDIA         my_media;
UINT             status;
UINT             attributes;

/* Retrieve the attributes of "myfile.txt" from the specified media. */

status = fx_file_attributes_read(&my_media, "myfile.txt", &attributes);

/* If status equals FX_SUCCESS, "attributes"
    contains the file attributes for "myfile.txt". */

```

### See Also

- fx_file_allocate
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_attributes_set

Sets the specified file's attributes.

### Prototype

```c
UINT fx_file_attributes_set(
    FX_MEDIA *media_ptr,
    CHAR *file_name,
    UINT attributes);
```

### Description

This service sets the file's attributes to those specified by the caller.

> **Warning:** *The application is only allowed to modify a subset of the file's attributes with this service. Any attempt to set additional attributes will result in an error.*

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *file_name*: Pointer to the name of the requested file** (directory path is optional).
- *attributes*: The new attributes for the file. The valid file attributes are defined as follows:
  - **FX_READ_ONLY** (0x01)
  - **FX_HIDDEN** (0x02)
  - **FX_SYSTEM** (0x04)
  - **FX_ARCHIVE** (0x20)

### Return Values

- **FX_SUCCESS** (0x00) Successful attribute set.
- **FX_ACCESS_ERROR** (0x06) File is open and cannot have its attributes set.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_NO_MORE_ENTRIES** (0x0F) No more entries in the FAT table or exFAT cluster map.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation.
- **FX_NOT_FOUND** (0x04) Specified file was not found in the media.
- **FX_NOT_A_FILE** (0x05) Specified file is a directory.
- **FX_SECTOR_INVALID** (0x89) Sector is invalid
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_MEDIA_INVALID** (0x02) Invalid media.
- **FX_PTR_ERROR** (0x18) Invalid media pointer.
- **FX_INVALID_ATTR** (0x19) Invalid attributes selected.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c

FX_MEDIA         my_media;
UINT             status;

/* Set the attributes of "myfile.txt" to read-only. */

status = fx_file_attributes_set(&my_media, "myfile.txt", FX_READ_ONLY);

/* If status equals FX_SUCCESS, the file is now read-only. */

```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_best_effort_allocate

Makes its best effort to allocate space for a file.

### Prototype

```c
UINT fx_file_best_effort_allocate(
    FX_FILE *file_ptr,
    ULONG size,
    ULONG *actual_size_allocated);
```

### Description

This service allocates and links one or more contiguous clusters to the end of the specified file. FileX determines the number of clusters required by dividing the requested size by the number of bytes per cluster. The result is then rounded up to the next whole cluster. If there are not enough consecutive clusters available in the media, this service links the largest available block of consecutive clusters to the file. The amount of space actually allocated to the file is returned to the caller.

To allocate space beyond 4GB, application shall use the service *fx_file_extended_best_effort_allocate*.

### Input Parameters

- *file_ptr*: Pointer to a previously opened file.
- *size*: Number of bytes to allocate for the file.

### Return Values

- **FX_SUCCESS** (0x00) Successful best-effort file allocation.
- **FX_ACCESS_ERROR** (0x06) Specified file is not open for writing.
- **FX_NOT_OPEN** (0x07) Specified file is not currently open.
- **FX_NO_MORE_SPACE** (0x0A) Media associated with this file does not have enough available clusters.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_NO_MORE_ENTRIES** (0x0F) No more FAT entries.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_PTR_ERROR** (0x18) Invalid file pointer or destination.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c

FX_FILE         my_file;
UINT             status;
ULONG             actual_allocation;

/* Attempt to allocate 1024 bytes to the end of my_file. */

status = fx_file_best_effort_allocate(&my_file, 1024, &actual_allocation);

/* If status equals FX_SUCCESS, the number of bytes
    allocated to the file is found in actual_allocation. */

```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_close

Closes a file.

### Prototype

```c
UINT fx_file_close(FX_FILE *file_ptr);
```

### Description

This service closes the specified file. If the file was open for writing and if it was modified, this service completes the file modification process by updating its directory entry with the new size and the current system time and date.

### Input Parameters

- *file_ptr*: Pointer to a previously opened file.

### Return Values

- **FX_SUCCESS** (0x00) Successful file close.
- **FX_NOT_OPEN** (0x07) Specified file is not open.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_PTR_ERROR** (0x18) Invalid media or attributes pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c

FX_FILE        my_file;
UINT        status;

/* Close the previously opened file "my_file". */
status = fx_file_close(&my_file);

/* If status equals FX_SUCCESS, the file was closed successfully. */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_create

Creates a file

### Prototype

```c
UINT fx_file_create(
    FX_MEDIA *media_ptr,
    CHAR *file_name);
```

### Description

This service creates the specified file in the default directory or in the directory path supplied with the file name.

> **Warning:** *This service creates a file of zero length, i.e., no clusters allocated. Allocation will automatically take place on subsequent file writes or can be done in advance with the fx_file_allocate service or fx_file_extended_allocate for space beyond 4GB) service.*

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *file_name*: Pointer to the name of the file to create (directory path is optional).

### Return Values

- **FX_SUCCESS** (0x00) Successful file create.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_ALREADY_CREATED** (0x0B) Specified file was already created.
- **FX_NO_MORE_SPACE** (0x0A)    Either there are no more entries in the root directory or there are no more clusters available.
- **FX_INVALID_PATH** (0x0D) Invalid path supplied with file name.
- **FX_INVALID_NAME** (0x0C) File name is invalid.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_NO_MORE_ENTRIES** (0x0F) No more FAT entries.
- **FX_NO_MORE_SPACE** (0x0A)    No more space to complete the operation
- **FX_MEDIA_INVALID** (0x02)Invalid media.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Underlying media is write protected.
- **FX_PTR_ERROR** (0x18) Invalid media or file name pointer.
- **FX_CALLER_ERROR** (0x20)    Caller is not a thread.

### Allowed From

Threads

### Example

```c

FX_MEDIA         my_media;
UINT             status;

/* Create a file called "myfile.txt" in the
    root or the default directory of the media. */

status = fx_file_create(&my_media, "myfile.txt");

/* If status equals FX_SUCCESS, a zero sized file named "myfile.txt". */
```

### See Also
- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_date_time_set

Sets a file's date and time.

### Prototype

```c
UINT fx_file_date_time_set(
    FX_MEDIA *media_ptr, 
    CHAR *file_name,
    UINT year, 
    UINT month, 
    UINT day,
    UINT hour, 
    UINT minute, 
    UINT second);
```

### Description

This service sets the date and time of the specified file.

```c

FX_MEDIA         my_media;
/* Set the date/time of "my_file". */
status = fx_file_date_time_set(&my_media, "my_file", 1999, 12, 31, 23, 59, 59);

/* If status is FX_SUCCESS the file's date/time was successfully set. /*
```

### Input Parameters

- *media_ptr*: Pointer to media control block.
- *file_name*: Pointer to name of the file.
- *year*: Value of year (1980-2107 inclusive).
- *month*: Value of month (1-12 inclusive).
- *day*: Value of day (1-31 inclusive).
- *hour*: Value of hour (0-23 inclusive).
- *minute*: Value of minute (0-59 inclusive).
- *second*: Value of second (0-59 inclusive).

### Return Values

- **FX_SUCCESS** (0x00) Successful date/time set.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_NOT_FOUND** (0x04)    File was not found.
- **FX_FILE_CORRUPT** (0x08)    File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_NO_MORE_ENTRIES** (0x0F) No more FAT entries.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_PTR_ERROR** (0x18) Invalid media or name pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.
- **FX_INVALID_YEAR** (0x12) Year is invalid.
- **FX_INVALID_MONTH** (0x13) Month is invalid.
- **FX_INVALID_DAY** (0x14) Day is invalid.
- **FX_INVALID_HOUR** (0x15)    Hour is invalid.
- **FX_INVALID_MINUTE** (0x16) Minute is invalid.
- **FX_INVALID_SECOND** (0x17) Second is invalid.

### Allowed From

Threads

### Example

```c

FX_MEDIA         my_media;
/* Set the date/time of "my_file". */
status = fx_file_date_time_set(&my_media, "my_file", 1999, 12, 31, 23, 59, 59);

/* If status is FX_SUCCESS the file's date/time was successfully set. /*
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_delete

Deletes a file.

### Prototype

```c
UINT fx_file_delete(
    FX_MEDIA *media_ptr, 
    CHAR *file_name);
```

### Description

This service deletes the specified file.

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *file_name*: Pointer to the name of the file to delete (directory path is optional).

### Return Values

- **FX_SUCCESS** (0x00) Successful file delete.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_NOT_FOUND** (0x04) Specified file was not found.
- **FX_NOT_A_FILE** (0x05) Specified file name was a directory or volume.
- **FX_ACCESS_ERROR** (0x06) Specified file is currently open.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_NO_MORE_ENTRIES** (0x0F) No more FAT entries.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_MEDIA_INVALID** (0x02) Invalid media.
- **FX_PTR_ERROR** (0x18) Invalid media pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c

FX_MEDIA            my_media;
UINT                 status;
/* Delete the file "myfile.txt". */

status = fx_file_delete(&my_media, "myfile.txt");

/* If status equals FX_SUCCESS, "myfile.txt" has been deleted. */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_extended_allocate

Allocates space for a file.

### Prototype

```c
UINT fx_file_extended_allocate(
    FX_FILE *file_ptr, 
    ULONG64 size);
```

### Description

This service allocates and links one or more contiguous clusters to the end of the specified file. FileX determines the number of clusters required by dividing the requested size by the number of bytes per cluster. The result is then rounded up to the next whole cluster.

This service is designed for exFAT. The *size* parameter takes a 64-bit integer value, which allows the caller to pre-allocate space beyond 4GB range.

### Input Parameters

- *file_ptr*: Pointer to a previously opened file.
- *size*: Number of bytes to allocate for the file.

### Return Values

- **FX_SUCCESS** (0x00) Successful file allocation.
- **FX_ACCESS_ERROR** (0x06) Specified file is not open for writing.
- **FX_NOT_OPEN** (0x07) Specified file is not currently open.
- **FX_NO_MORE_SPACE** (0x0A) Media associated with this file does not have enough available clusters.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_NO_MORE_ENTRIES** (0x0F) No more FAT entries.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_PTR_ERROR** (0x18) Invalid file pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c

FX_FILE            my_file;
UINT            status;
/* Allocate 0x100000000 bytes to the end of my_file. */

status = fx_file_extended_allocate(&my_file, 0x100000000);

/* If status equals FX_SUCCESS the file now has
    one or more contiguous cluster(s) that can accommodate at least
    1024 bytes of user data. */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_extended_best_effort_allocate

Makes the best effort to allocate space for a file.

### Prototype

```c
UINT fx_file_extended best_effort_allocate(
    FX_FILE *file_ptr,
    ULONG64 size,
    ULONG64 *actual_size_allocated);
```

### Description

This service allocates and links one or more contiguous clusters to the end of the specified file. FileX determines the number of clusters required by dividing the requested size by the number of bytes per cluster. The result is then rounded up to the next whole cluster. If there are not enough consecutive clusters available in the media, this service links the largest available block of consecutive clusters to the file. The amount of space actually allocated to the file is returned to the caller.

This service is designed for exFAT. The *size* parameter takes a 64-bit integer value, which allows the caller to pre-allocate space beyond 4GB range.

### Input Parameters

- *file_ptr*: Pointer to a previously opened file.
- *size*: Number of bytes to allocate for the file.

### Return Values

- **FX_SUCCESS** (0x00) Successful file allocation.
- **FX_ACCESS_ERROR** (0x06) Specified file is not open for writing.
- **FX_NOT_OPEN** (0x07) Specified file is not currently open.
- **FX_NO_MORE_SPACE** (0x0A) Media associated with this file does not have enough available clusters.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_NO_MORE_ENTRIES** (0x0F) No more FAT entries.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_PTR_ERROR** (0x18) Invalid file pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c

FX_FILE my_file;
UINT             status;
ULONG64         actual_allocation;

/* Attempt to allocate 0x100000000 bytes to the end of my_file. */

status = fx_file_extended_best_effort_allocate(&my_file,
    0x100000000, &actual_allocation);

/* If status equals FX_SUCCESS, the number of bytes
    allocated to the file is found in actual_allocation. */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_extended_relative_seek

Positions the internal read/write file pointer to a relative byte offset.

### Prototype

```c
UINT fx_file_extended_relative_seek(
    FX_FILE *file_ptr,
    ULONG64 byte_offset,
    UINT seek_from);
```

### Description

This service positions the internal file read/write pointer to the specified relative byte offset. Any subsequent file read or write request will begin at this location in the file.

This service is designed for exFAT. The *byte_offset* parameter takes a 64bit integer value, which allows the caller to reposition the read/write pointer beyond 4GB range.

If **FX_SEEK_BEGIN** is specified for the *seek_from parameter*, the seek operation is performed from the beginning of the file. If **FX_SEEK_END** is specified the seek operation is performed backward from the end of the file. If **FX_SEEK_FORWARD** is specified, the seek operation is performed forward from the current file position. If **FX_SEEK_BACK** is specified, the seek operation is performed backward from the current file position.

> **Important:** *If the seek operation attempts to seek past the end of the file, the file's read/write pointer is positioned to the end of the file. Conversely, if the seek operation attempts to position past the beginning of the file, the file's read/write pointer is positioned to the beginning of the file.*

### Input Parameters

- *file_ptr*: Pointer to a previously opened file.
- *byte_offset*: Desired relative byte offset in file.
- *seek_from*: The direction and location of where to perform the relative seek from. Valid seek options are defined as follows:
  - **FX_SEEK_BEGIN** (0x00)
  - **FX_SEEK_END** (0x01)
  - **FX_SEEK_FORWARD** (0x02)
  - **FX_SEEK_BACK** (0x03)

If FX_SEEK_BEGIN is specified, the seek operation is performed from the beginning of the file. If FX_SEEK_END is specified the seek operation is performed backward from the end of the file. If FX_SEEK_FORWARD is specified, the seek operation is performed forward from the current file position. If FX_SEEK_BACK is specified, the seek operation is performed backward from the current file position.

### Return Values

- **FX_SUCCESS** (0x00) Successful file relative seek.
- **FX_NOT_OPEN** (0x07) Specified file is not currently open.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_PTR_ERROR** (0x18) Invalid file pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_FILE     my_file;
UINT         status;

/* Attempt to seek forward 0x100000000 bytes in "my_file". */

status = fx_file_extended_relative_seek(&my_file, 0x100000000, FX_SEEK_FORWARD);

/* If status equals FX_SUCCESS, the file read/write
    pointers are positioned 0x100000000 bytes forward. */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_extended_seek

Positions the internal read/write pointer to a byte offset.

### Prototype

```c
UINT fx_file_extended_seek(
    FX_FILE *file_ptr, 
    ULONG64 byte_offset);
```

### Description

This service positions the internal file read/write pointer to the specified byte offset. Any subsequent file read or write request will begin at this location in the file.

This service is designed for exFAT. The *byte_offset* parameter takes a 64bit integer value, which allows the caller to reposition the read/write pointer beyond 4GB range.

### Input Parameters

- *file_ptr*: Pointer to the file control block.
- *byte_offset*: Desired byte offset in file. A value of zero will position the read/write pointer at the beginning of the file, while a value greater than the file's size will position the read/write pointer at the end of the file.

### Return Values

- **FX_SUCCESS** (0x00) Successful file seek.
- **FX_NOT_OPEN** (0x07) Specified file is not open.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_PTR_ERROR** (0x18) Invalid file pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_FILE         my_file;
UINT             status;
/* Seek to position 0x100000000 of "my_file." */

status = fx_file_extended_seek(&my_file, 0x100000000);

/* If status equals FX_SUCCESS, the file read/write pointer
    is now positioned 0x100000000 bytes from the beginning of the file. */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_extended_truncate

Truncates a file.

### Prototype

```c
UINT fx_file_truncate(
    FX_FILE *file_ptr,
    ULONG64 size);
```

### Description

This service truncates the size of the file to the specified size. If the supplied size is greater than the actual file size, this service doesn't do anything. None of the media clusters associated with the file are released.

> **Warning:** *Use caution truncating files that may also be simultaneously open for reading. Truncating a file also opened for reading can result in reading invalid data.*

This service is designed for exFAT. The *size* parameter takes a 64-bit integer value, which allows the caller to operate beyond 4GB range.

### Input Parameters

- *file_ptr*: Pointer to the file control block.
- *size*: New file size. Bytes past this new file size are discarded.

### Return Values

- **FX_SUCCESS** (0x00) Successful file truncate.
- **FX_NOT_OPEN** (0x07) Specified file is not open.
- **FX_ACCESS_ERROR** (0x06) Specified file is not open for writing.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_NO_MORE_ENTRIES** (0x0F) No more FAT entries.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Underlying media is write protected.
- **FX_PTR_ERROR** (0x18) Invalid file pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_FILE            my_file;
UINT            status;
/* Truncate "my_file" to 0x100000000 bytes. */

status = fx_file_extended_truncate(&my_file, 0x100000000);

/* If status equals FX_SUCCESS, "my_file" contains 0x100000000 or fewer bytes. */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_extended_truncate_release

Truncates a file and its releases cluster(s).

### Prototype

```c
UINT fx_file_extended_truncate_release(
    FX_FILE *file_ptr, 
    ULONG64 size);
```

### Description

This service truncates the size of the file to the specified size. If the supplied size is greater than the actual file size, this service does not do anything. Unlike the ***fx_file_extended_truncate*** service, this service does release any unused clusters.

> **Warning:** *Use caution truncating files that may also be simultaneously open for reading. Truncating a file also opened for reading can result in reading invalid data.*

This service is designed for exFAT. The *size* parameter takes a 64-bit integer value, which allows the caller to operate beyond 4GB range.

### Input Parameters

- *file_ptr*: Pointer to a previously opened file.
- *size*: New file size. Bytes past this new file size are discarded.

### Return Values

- **FX_SUCCESS** (0x00) Successful file truncate.
- **FX_ACCESS_ERROR** (0x06) Specified file is not open for writing.
- **FX_NOT_OPEN** (0x07) Specified file is not currently open.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_NO_MORE_ENTRIES** (0x0F) No more FAT entries.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_PTR_ERROR** (0x18) Invalid file pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_FILE            my_file;
UINT            status;
/* Attempt to truncate everything after the first 0x100000000 bytes of "my_file". */

status = fx_file_extended_truncate_release(&my_file, 0x100000000);

/* If status equals FX_SUCCESS, the file is now 0x100000000
    bytes or fewer and all unused clusters have been released. */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_open

Opens a file.

### Prototype

```c
UINT fx_file_open(
    FX_MEDIA *media_ptr,
    FX_FILE *file_ptr,
    CHAR *file_name,
    UINT open_type);
```

### Description

This service opens the specified file for either reading or writing. A file may be opened for reading multiple times, while a file can only be opened for writing once until the writer closes the file.

> **Important:** *Care must be taken if a file is concurrently open for reading and writing. File writing performed when a file is simultaneously opened for reading may not be seen by the reader, unless the reader closes and reopens the file for reading. Similarly, the file writer should be careful when using file truncate services. If a file is truncated by the writer, readers of the same file could return invalid data.*

Opening files with **FX_OPEN_FOR_READ** and **FX_OPEN_FOR_READ_FAST** is similar, but not the same. **FX_OPEN_FOR_READ** includes verification that the linked list of the clusters that comprise the file are intact, while **FX_OPEN_FOR_READ_FAST** does not perform this verification.

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *file_ptr*: Pointer to the file control block.
- *file_name*: Pointer to the name of the file to open (directory path is optional).
- *open_type*: Type of file open. Valid open type options are the following.
  - **FX_OPEN_FOR_READ** (0x00)
  - **FX_OPEN_FOR_WRITE** (0x01)
  - **FX_OPEN_FOR_READ_FAST** (0x02)

### Return Values

- **FX_SUCCESS** (0x00) Successful file open.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_NOT_FOUND** (0x04) Specified file was not found.
- **FX_NOT_A_FILE** (0x05) Specified file name was a directory or volume.
- **FX_FILE_CORRUPT** (0x08) Specified file is corrupt and the open failed.
- **FX_ACCESS_ERROR** (0x06) Specified file is already open or open type is invalid.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_MEDIA_INVALID** (0x02) Invalid media.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Underlying media is write protected.
- **FX_PTR_ERROR** (0x18) Invalid media or file pointer.
- **FX_CALLER_ERROR** (0x20)    Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA     my_media;
FX_FILE     my_file;
UINT         status;

/* Open the file "myfile.txt" for reading. */

status = fx_file_open(&my_media, &my_file, "myfile.txt", FX_OPEN_FOR_READ);

/* If status equals FX_SUCCESS, file "myfile.txt" is now
    open and may be accessed now with the my_file pointer. */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_read

Reads bytes from the specified file.

### Prototype

```c
UINT fx_file_read(
    FX_FILE *file_ptr, 
    VOID *buffer_ptr,
    ULONG request_size, 
    ULONG *actual_size);
```

### Description

This service reads bytes from the file and stores them in the supplied buffer. After the read is complete, the file's internal read pointer is adjusted to point at the next byte in the file. If there are fewer bytes remaining in the request, only the bytes remaining are stored in the buffer. In any case, the total number of bytes placed in the buffer is returned to the caller.

> **Warning:** *The application must ensure that the buffer supplied is able to store the specified number of requested bytes.*

> **Warning:** *Faster performance is achieved if the destination buffer is on a long-word boundary and the requested size is evenly divisible by sizeof(**ULONG**).*

### Input Parameters

- *file_ptr*: Pointer to the file control block.
- *buffer_ptr*: Pointer to the destination buffer for the read.
- *request_size*: Maximum number of bytes to read.
- *actual_size*: Pointer to the variable to hold the actual number of bytes read into the supplied buffer.

### Return Values

- **FX_SUCCESS** (0x00) Successful file read.
- **FX_NOT_OPEN** (0x07) Specified file is not open.
- **FX_FILE_CORRUPT** (0x08) Specified file is corrupt and the read failed.
- **FX_END_OF_FILE** (0x09) End of file has been reached.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_PTR_ERROR** (0x18) Invalid file or buffer pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_FILE                 my_file;
unsigned char           my_buffer[1024];
ULONG                   actual_bytes;
UINT                    status;

/* Read up to 1024 bytes into "my_buffer." */
status = fx_file_read(&my_file, my_buffer, 1024, &actual_bytes);

/* If status equals FX_SUCCESS, "my_buffer" contains the bytes
    read from the file. The total number of bytes read is in "actual_bytes." */
```

### See Also

- fx_file_allocate,
- fx_file_attributes_read,
- fx_file_attributes_set,
- fx_file_best_effort_allocate,
- fx_file_close,
- fx_file_create,
- fx_file_date_time_set,
- fx_file_delete,
- fx_file_extended_allocate,
- fx_file_extended_best_effort_allocate,
- fx_file_extended_relative_seek,
- fx_file_extended_seek,
- fx_file_extended_truncate,
- fx_file_extended_truncate_release,
- fx_file_open,
- fx_file_relative_seek,
- fx_file_rename,
- fx_file_seek,
- fx_file_truncate,
- fx_file_truncate_release,
- fx_file_write,
- fx_file_write_notify_set,
- fx_unicode_file_create,
- fx_unicode_file_rename,
- fx_unicode_name_get,
- fx_unicode_short_name_get

## fx_file_relative_seek

Positions the internal read/write pointer to a relative byte offset.

### Prototype

```c
UINT fx_file_relative_seek(
    FX_FILE *file_ptr,
    ULONG byte_offset,
    UINT seek_from);
```

### Description

This service positions the internal file read/write pointer to the specified relative byte offset. Any subsequent file read or write request will begin at this location in the file.

> **Important:** *If the seek operation attempts to seek past the end of the file, the file's read/write pointer is positioned to the end of the file. Conversely, if the seek operation attempts to position past the beginning of the file, the file's read/write pointer is positioned to the beginning of the file.*

To seek with an offset value beyond 4GB, application shall use the service *fx_file_extended_relative_seek*.

If **FX_SEEK_BEGIN** is specified in the *seek-from* parameter, the seek operation is performed from the beginning of the file. If **FX_SEEK_END** is specified the seek operation is performed backward from the end of the file. If **FX_SEEK_FORWARD** is specified, the seek operation is performed forward from the current file position. If **FX_SEEK_BACK** is specified, the seek operation is performed backward from the current file position.

### Input Parameters

- *file_ptr*: Pointer to a previously opened file.
- *byte_offset*: Desired relative byte offset in file.
- *seek_from*: The direction and location of where to perform the relative seek from. Valid seek options are defined as follows:
  - **FX_SEEK_BEGIN** (0x00)
  - **FX_SEEK_END** (0x01)
  - **FX_SEEK_FORWARD** (0x02)
  - **FX_SEEK_BACK** (0x03)

### Return Values

- **FX_SUCCESS** (0x00) Successful file relative seek.
- **FX_NOT_OPEN** (0x07) Specified file is not currently open.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_NO_MORE_ENTRIES** (0x0F) No more FAT entries.
- **FX_PTR_ERROR** (0x18) Invalid file pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_FILE     my_file;
UINT         status;

/* Attempt to move 10 bytes forward in "my_file". */

status = fx_file_relative_seek(&my_file, 10, FX_SEEK_FORWARD);

/* If status equals FX_SUCCESS, the file read/write pointers
    are positioned 10 bytes forward. */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_rename

Renames a file.

### Prototype

```c
UINT fx_file_rename(
    FX_MEDIA *media_ptr,
    CHAR *old_file_name,
    CHAR *new_file_name);
```

### Description

This service changes the name of the file specified by *old_file_name*. Renaming is also done relative to the specified path or the default path. If a path is specified in the new file name, the renamed file is effectively moved to the specified path. If no path is specified, the renamed file is placed in the current default path.

### Input Parameters

- *media_ptr*: Pointer to a media control block.
- *old_file_name*: Pointer to a character string containing the name of the file to rename (directory path is optional).
- *new_file_name*: Pointer to the new file name. The directory path is not allowed.

### Return Values

- **FX_SUCCESS** (0x00) Successful file rename.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_NOT_FOUND** (0x04)    Specified file was not found.
- **FX_NOT_A_FILE** (0x05) Specified file is a directory.
- **FX_ACCESS_ERROR** (0x06) Specified file is already open.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23)    Specified media is write protected.
- **FX_INVALID_NAME** (0x0C) Specified new file name is not a valid file name.
- **FX_INVALID_PATH** (0x0D)    Path is invalid.
- **FX_ALREADY_CREATED** (0x0B) The new file name is used.
- **FX_MEDIA_INVALID** (0x02)    Media is invalid.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_NO_MORE_ENTRIES** (0x0F) No more FAT entries.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT table.
- **FX_PTR_ERROR** (0x18) Invalid media pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA         my_media;
UINT             status;

/* Rename "myfile1.txt" to "myfile2.txt" in the default directory of the media. */

status = fx_file_rename(&my_media, "myfile1.txt", "myfile2.txt");

/* If status equals FX_SUCCESS, the file was successfully renamed. */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_seek

Positions the internal read/write pointer to a byte offset.

### Prototype

```c
UINT fx_file_seek(
    FX_FILE *file_ptr,
    ULONG byte_offset);
```

### Description

This service positions the internal file read/write pointer to the specified byte offset. Any subsequent file read or write request will begin at this location in the file.

To seek with an offset value beyond 4GB, application shall use the service ***fx_file_extended_seek***.

### Input Parameters

- *file_ptr*: Pointer to the file control block.
- *byte_offset*: Desired byte offset in file. A value of zero will position the read/write pointer at the beginning of the file, while a value greater than the file's size will position the read/write pointer at the end of the file.

### Return Values

- **FX_SUCCESS** (0x00) Successful file seek.
- **FX_NOT_OPEN** (0x07) Specified file is not open.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_PTR_ERROR** (0x18) Invalid file pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_FILE            my_file;
UINT            status;
/* Seek to the beginning of "my_file." */
status = fx_file_seek(&my_file, 0);
/* If status equals FX_SUCCESS, the file read/write pointer
    is now positioned to the beginning of the file. */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_truncate

Truncates a file to a specified number of bytes.

### Prototype

```c
UINT fx_file_truncate(
    FX_FILE *file_ptr,
    ULONG size);
```

### Description

This service truncates the size of the file to the specified size. If the supplied size is greater than the actual file size, this service doesn't do anything. None of the media clusters associated with the file are released.

> **Warning:** *Use caution truncating files that may also be simultaneously open for reading. Truncating a file also opened for reading can result in reading invalid data.*

To operate beyond 4GB, application shall use the service ***fx_file_extended_truncate***.

### Input Parameters

- *file_ptr*: Pointer to the file control block.
- *size*: New file size. Bytes past this new file size are discarded.

### Return Values

- **FX_SUCCESS** (0x00) Successful file truncate.
- **FX_NOT_OPEN** (0x07) Specified file is not open.
- **FX_ACCESS_ERROR** (0x06) Specified file is not open for writing.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_NO_MORE_ENTRIES** (0x0F) No more FAT entries.
- **FX_NO_MORE_SPACE** (0x0A) No more space to complete the operation
- **FX_PTR_ERROR** (0x18) Invalid file pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_FILE                my_file;
UINT                status;
/* Truncate "my_file" to 100 bytes. */

status = fx_file_truncate(&my_file, 100);

/* If status equals FX_SUCCESS, "my_file" contains 100 or fewer bytes. */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_truncate_release

Truncates a file and releases its cluster(s).

### Prototype

```c
UINT fx_file_truncate(
    FX_FILE *file_ptr,
    ULONG size);
```

### Description

This service truncates the size of the file to the specified size. If the supplied size is greater than the actual file size, this service does not do anything. Unlike the ***fx_file_truncate*** service, this service does release any unused clusters.

> **Warning:** *Use caution truncating files that may also be simultaneously open for reading. Truncating a file also opened for reading can result in reading invalid data.*

To operate beyond 4GB, application shall use the service ***fx_file_extended_truncate_release***.

### Input Parameters

- *file_ptr*: Pointer to a previously opened file.
- *size*: New file size. Bytes past this new file size are discarded.

### Return Values

- **FX_SUCCESS** (0x00) Successful file truncate.
- **FX_ACCESS_ERROR** (0x06) Specified file is not open for writing.
- **FX_NOT_OPEN** (0x07) Specified file is not currently open.
- **FX_IO_ERROR** (0x90)    Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Underlying media is write protected.
- **FX_FILE_CORRUPT** (0x08)    File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_NO_MORE_ENTRIES** (0x0F) No more FAT entries.
- **FX_NO_MORE_SPACE** (0x0A)    No more space to complete the operation.
- **FX_PTR_ERROR** (0x18) Invalid file pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_FILE         my_file;
UINT             status;

/* Attempt to truncate everything after the first 100 bytes of "my_file". */

status = fx_file_truncate_release(&my_file, 100);

/* If status equals FX_SUCCESS, the file is now 100 bytes
    or fewer and all unused clusters have been released. */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_file_write

Writes bytes to a file.

### Prototype

```c
UINT fx_file_write(
    FX_FILE *file_ptr,
    VOID *buffer_ptr,
    ULONG size);
```

### Description

This service writes bytes from the specified buffer starting at the file's current position. After the write is complete, the file's internal read pointer is adjusted to point at the next byte in the file.

> **Warning:** *Faster performance is achieved if the source buffer is on a long-word boundary and the requested size is evenly divisible by sizeof(**ULONG**).*

### Input Parameters

- *file_ptr*: Pointer to the file control block.
- *buffer_ptr*: Pointer to the source buffer for the write.
- *size*: Number of bytes to write.

### Return Values

- **FX_SUCCESS** (0x00) Successful file write.
- **FX_NOT_OPEN** (0x07) Specified file is not open.
- **FX_ACCESS_ERROR** (0x06) Specified file is not open for writing.
- **FX_NO_MORE_SPACE** (0x0A) There is no more room available in the media to perform this write.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT entry.
- **FX_NO_MORE_ENTRIES** (0x0F) No more FAT entries.
- **FX_PTR_ERROR** (0x18) Invalid file or buffer pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_FILE            my_file;
UINT            status;
/* Write a 10 character buffer to "my_file." */

status = fx_file_write(&my_file, "1234567890", 10);

/* If status equals FX_SUCCESS, the small text string was written out to the file. */
```

### See Also

- fx_file_allocate,
- fx_file_attributes_read,
- fx_file_attributes_set,
- fx_file_best_effort_allocate,
- fx_file_close,
- fx_file_create,
- fx_file_date_time_set,
- fx_file_delete,
- fx_file_extended_allocate,
- fx_file_extended_best_effort_allocate,
- fx_file_extended_relative_seek,
- fx_file_extended_seek,
- fx_file_extended_truncate,
- fx_file_extended_truncate_release,
- fx_file_open,
- fx_file_read,
- fx_file_relative_seek,
- fx_file_rename,
- fx_file_seek,
- fx_file_truncate,
- fx_file_truncate_release,
- fx_file_write_notify_set,
- fx_unicode_file_create,
- fx_unicode_file_rename,
- fx_unicode_name_get,
- fx_unicode_short_name_get

## fx_file_write_notify_set

Sets the file write notify callback function.

### Prototype

```c
UINT fx_file_write_notify_set(
    FX_FILE *file_ptr,
    VOID (*file_write_notify)(FX_FILE*));
```

### Description

This service installs callback function that is invoked after a successful file write operation.

### Input Parameters

- *file_ptr*: Pointer to the file control block.
- *file_write_notify*: File write callback function to be installed. Set the callback function to **NULL** disables the callback function.

### Return Values

- **FX_SUCCESS** (0x00) Successfully installed the callback function.
- **FX_PTR_ERROR** (0x18) file_ptr is **NULL**.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
fx_file_write_notify_set(file_ptr, my_file_close_callback);

```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_media_abort

Aborts all current activities on the specified media

### Prototype

```c
UINT fx_media_abort(FX_MEDIA *media_ptr);
```

### Description

This service aborts all current activities associated with the media, including closing all open files, sending an abort request to the associated driver, and placing the media in an aborted state. This service is typically called when I/O errors are detected.

> **Warning:** *The media must be re-opened to use it again after an abort operation is performed.*

### Input Parameters

- *media_ptr*: Pointer to media control block.

### Return Values

- **FX_SUCCESS** (0x00) Successful media abort.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_PTR_ERROR** (0x18) Invalid media pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA    my_media;
UINT        status;
/* Abort all activity associated with "my_media". */

status = fx_media_abort(&my_media);

/* If status equals FX_SUCCESS, all activity
    associated with the media has been aborted. */

```

### See Also

- fx_fault_tolerant_enable
- fx_media_cache_invalidate
- fx_media_check
- fx_media_close
- fx_media_close_notify_set
- fx_media_exFAT_format
- fx_media_extended_space_available
- fx_media_flush
- fx_media_format
- fx_media_open
- fx_media_open_notify_set
- fx_media_read
- fx_media_space_available
- fx_media_volume_get
- fx_media_volume_set
- fx_media_write
- fx_system_initialize

## fx_media_cache_invalidate

Invalidates a logical sector cache.

### Prototype

```c
UINT fx_media_cache_invalidate(FX_MEDIA *media_ptr);
```

### Description

This service flushes all dirty sectors in the cache and then invalidates the entire logical sector cache.

### Input Parameters

- *media_ptr*: Pointer to media control block

### Return Values

- **FX_SUCCESS** (0x00) Successful media cache invalidate.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_PTR_ERROR** (0x18) Invalid media or scratch pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA     my_media;

/* Invalidate the cache of the media. */
status = fx_media_cache_invalidate(&my_media);

/* If status is FX_SUCCESS the cache in the media
    was successfully flushed and invalidated. */

```

### See Also

- fx_fault_tolerant_enable
- fx_media_abort
- fx_media_check
- fx_media_close
- fx_media_close_notify_set
- fx_media_exFAT_format
- fx_media_extended_space_available
- fx_media_flush
- fx_media_format
- fx_media_open
- fx_media_open_notify_set
- fx_media_read
- fx_media_space_available
- fx_media_volume_get
- fx_media_volume_set
- fx_media_write
- fx_system_initialize

## fx_media_check

Checks the specified media for errors.

### Prototype

```c
UINT fx_media_check(
    FX_MEDIA *media_ptr,
    UCHAR *scratch_memory_ptr,
    ULONG scratch_memory_size,
    ULONG error_correction_option,
    ULONG *errors_detected_ptr);
```

### Description

This service checks the specified media for basic structural errors, including file/directory cross-linking, invalid FAT chains, and lost clusters. This service also provides the capability to correct detected errors.

The fx_media_check service requires scratch memory for its depth-first analysis of directories and files in the media. Specifically, the scratch memory supplied to the media check service must be large enough to hold several directory entries, a data structure to "stack" the current directory entry position before entering into subdirectories, and finally the logical FAT bit map. The scratch memory should be at least 512-1024 bytes plus memory for the logical FAT bit map, which requires as many bits as there are clusters in the media. For example, a device with 8000 clusters would require 1000 bytes to represent and thus require a total scratch area on the order of 2048 bytes.

> **Warning:** *This service should only be called immediately after fx_media_open and without any other file system activity.*

### Input Parameters

- *media_ptr*: Pointer to media control block.
- *scratch_memory_ptr*: Pointer to the start of scratch memory.
- *scratch_memory_size*: Size of scratch memory in bytes.
- *error_correction_option*: Error correction option bits, when the bit is set, error correction is performed. The error correction option bits are defined as follows:
  - **FX_FAT_CHAIN_ERROR** (0x01)
  - **FX_DIRECTORY_ERROR** (0x02)
  - **FX_LOST_CLUSTER_ERROR** (0x04)
    Simply OR together the required error correction options. If no error correction is required, a value of 0 should be supplied.
- *errors_detected_ptr*: Destination for error detection bits, as defined below:
  - **FX_FAT_CHAIN_ERROR** (0x01)
  - **FX_DIRECTORY_ERROR** (0x02)
  - **FX_LOST_CLUSTER_ERROR** (0x04)
  - **FX_FILE_SIZE_ERROR** (0x08)

### Return Values

- **FX_SUCCESS** (0x00) Successful media check, view the errors detected destination for details.
- **FX_ACCESS_ERROR** (0x06) Unable to perform check with open files.
- **FX_FILE_CORRUPT** (0x08) File is corrupted.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_NO_MORE_SPACE** (0x0A) No more space on the media.
- **FX_NOT_ENOUGH_MEMORY** (0x91) Supplied scratch memory is not large enough.
- **FX_ERROR_NOT_FIXED** (0x93) Corruption of FAT32 root directory that could not be fixed.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_SECTOR_INVALID** (0x89) Sector is invalid.
- **FX_PTR_ERROR** (0x18) Invalid media or scratch pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.


### Allowed From

Threads

### Example

```c
FX_MEDIA         my_media;
ULONG             detected_errors;
UCHAR             sratch_memory[4096];

/* Check the media and correct all errors. */

status = fx_media_check(&my_media, sratch_memory, 4096,
                        FX_FAT_CHAIN_ERROR |
                        FX_DIRECTORY_ERROR |
                        FX_LOST_CLUSTER_ERROR, &detected_errors);

/* If status is FX_SUCCESS and detected_errors is 0,
    the media was successfully checked and found to be error free. */

```

### See Also

- fx_fault_tolerant_enable
- fx_media_abort
- fx_media_cache_invalidate
- fx_media_close
- fx_media_close_notify_set
- fx_media_exFAT_format
- fx_media_extended_space_available
- fx_media_flush
- fx_media_format
- fx_media_open
- fx_media_open_notify_set
- fx_media_read
- fx_media_space_available
- fx_media_volume_get
- fx_media_volume_set
- fx_media_write
- fx_system_initialize

## fx_media_close

Closes the specified media.

### Prototype

```c
UINT fx_media_close(FX_MEDIA *media_ptr);
```

### Description

This service closes the specified media. In the process of closing the media, all open files are closed and any remaining buffers are flushed to the physical media.

### Input Parameters

- *media_ptr*: Pointer to media control block.

### Return Values

- **FX_SUCCESS** (0x00) Successful media close.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_PTR_ERROR** (0x18) Invalid media pointer.
- **FX_CALLER_ERROR**    (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA    my_media;
UINT        status;
/* Close "my_media". */

status = fx_media_close(&my_media);

/* If status equals FX_SUCCESS, "my_media" is closed. */

```

### See Also

- fx_fault_tolerant_enable
- fx_media_abort
- fx_media_cache_invalidate
- fx_media_check
- fx_media_close_notify_set
- fx_media_exFAT_format
- fx_media_extended_space_available
- fx_media_flush
- fx_media_format
- fx_media_open
- fx_media_open_notify_set
- fx_media_read
- fx_media_space_available
- fx_media_volume_get
- fx_media_volume_set
- fx_media_write
- fx_system_initialize

## fx_media_close_notify_set

Sets the media close-notification callback function.

### Prototype

```c
UINT fx_media_close_notify_set(
    FX_MEDIA *media_ptr,
    VOID (*media_close_notify)(FX_MEDIA*));
```

### Description

This service sets a notify callback function which will be invoked after a media is successfully closed.

### Input Parameters

- *media_ptr*: Pointer to media control block.
- *media_close_notify*: Media close notify callback function to be installed. Passing NULL as the callback function disables the media close callback.

### Return Values

- **FX_SUCCESS** (0x00) Successfully installed the callback function.
- **FX_PTR_ERROR** (0x18) media_ptr is NULL.
- **FX_CALLER_ERROR**    (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
fx_media_close_notify_set(media_ptr, my_media_close_callback);

```

### See Also

- fx_fault_tolerant_enable
- fx_media_abort
- fx_media_cache_invalidate
- fx_media_check
- fx_media_close
- fx_media_exFAT_format
- fx_media_extended_space_available
- fx_media_flush
- fx_media_format
- fx_media_open
- fx_media_open_notify_set
- fx_media_read
- fx_media_space_available
- fx_media_volume_get
- fx_media_volume_set
- fx_media_write
- fx_system_initialize

## fx_media_exFAT_format

Formats the specified media as an exFAT file system.

### Prototype

```c
UINT fx_media_exFAT_format(
    FX_MEDIA *media_ptr,
    VOID (*driver)(FX_MEDIA *media),
    VOID *driver_info_ptr, 
    UCHAR *memory_ptr,
    UINT memory_size, 
    CHAR *volume_name,
    UINT number_of_fats,
    ULONG64 hidden_sectors, 
    ULONG64 total_sectors,
    UINT bytes_per_sector, 
    UINT sectors_per_cluster,
    UINT volume_serial_number, 
    UINT boundary_unit);
```

### Description

This service formats the supplied media in an exFAT compatible manner based on the supplied parameters. This service must be called prior to opening the media.

> **Warning:** *Formatting an already formatted media effectively erases all files and directories on the media.*

> **Important:** *The exFAT volume size should match the size of the partition (if there is an MBR or GPT layout), or the size of the whole device if there is no partition layout (no MBR or GPT). There is a limitation on Windows that exFAT Disk will not be recognized if formatted with some values of total sectors that are less than available sectors*

> **Important:** *With reference to the specification, the bytes per sector may take on only the following values: 512, 1024, 2048 or 4096.*

### Input Parameters

- *media_ptr*: Pointer to the media control block. This is used only to provide some basic information necessary for the driver to operate.
- *driver*: Pointer to the I/O driver for this media. This will typically be the same driver supplied to the subsequent fx_media_open call.
- *driver_info_ptr*: Pointer to the optional information that the I/O driver may utilize.
- *memory_ptr*: Pointer to the working memory for the media. memory_size Specifies the size of the working media memory. The size must be at least as large as the media's sector size.
- *volume_name*: Pointer to the volume name string, which is a maximum of 11 characters.
- *number_of_fats*: Number of FATs on the media. Current implementation supports one FAT on the media.
- *hidden_sectors*: Number of sectors hidden before this media's boot sector. This is typical when multiple partitions are present.
- *total_sectors*: Total number of sectors in the media.
- *bytes_per_sector*: Number of bytes per sector, which is typically 512. FileX requires this to be a multiple of 32.
- *sectors_per_cluster*: Number of sectors in each cluster. The cluster is the minimum allocation unit in a FAT file system.
- *volume_serial_number*: Serial number to be used for this volume.
- *boundary_unit*: Physical data area alignment size, in number of sectors.

### Return Values

- **FX_SUCCESS** (0x00) Successful media format.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_PTR_ERROR** (0x18) Invalid media, driver, or memory pointer.
- **FX_CALLER_ERROR**    (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA             sd_card;
UCHAR                 media_memory[512];

/* Format a 64GB SD card with exFAT file system. The media has
    been properly partitioned, with the partition starts from sector 32768.
    For 64GB, there are total of 120913920 sectors, each sector 512 bytes. */

status = fx_media_exFAT_format(&sd_card, _fx_sd_driver,
                                driver_information, media_memory,
                                sizeof(media_memory),
                                "exFAT_DISK" /* Volume Name */,
                                1 /* Number of FATs */,
                                32768 /* Hidden sectors */,
                                120913920 /* Total sectors */,
                                512 /* Sector size */,
                                256 /* Sectors per cluster */,
                                12345 /* Volume ID */,
                                8192 /* Boundary unit */);

/* If status is FX_SUCCESS, the media was successfully formatted. */

```

### See Also

- fx_fault_tolerant_enable
- fx_media_abort
- fx_media_cache_invalidate
- fx_media_check
- fx_media_close
- fx_media_close_notify_set
- fx_media_extended_space_available
- fx_media_flush
- fx_media_format
- fx_media_open
- fx_media_open_notify_set
- fx_media_read
- fx_media_space_available
- fx_media_volume_get
- fx_media_volume_set
- fx_media_write
- fx_system_initialize

## fx_media_extended_space_available

Returns the available media space.

### Prototype

```c
UINT fx_media_extended_space_available(
    FX_MEDIA *media_ptr,
    ULONG64 *available_bytes_ptr);
```

### Description

This service returns the number of bytes available in the media.

This service is designed for exFAT. The pointer to *available_bytes* parameter takes a 64-bit integer value, which allows the caller to work with media beyond 4GB range.

### Input Parameters

- *media_ptr*: Pointer to a previously opened media.
- *available_bytes_ptr*: Available bytes left in the media.

### Return Values

- **FX_SUCCESS** (0x00) Successfully retrieved space available on the media.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_PTR_ERROR** (0x18) Invalid media pointer or available bytes pointer is NULL.
- **FX_CALLER_ERROR**    (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA        my_media;
ULONG64            available_bytes;
UINT            status;
/* Retrieve the available bytes in the media. */

status = fx_media_extended_space_available(&my_media, &available_bytes);

/* If status equals FX_SUCCESS, the number of available bytes is in "available_bytes." */

```

### See Also

- fx_fault_tolerant_enable
- fx_media_abort
- fx_media_cache_invalidate
- fx_media_check
- fx_media_close
- fx_media_close_notify_set
- fx_media_exFAT_format
- fx_media_flush
- fx_media_format
- fx_media_open
- fx_media_open_notify_set
- fx_media_read
- fx_media_space_available
- fx_media_volume_get
- fx_media_volume_set
- fx_media_write
- fx_system_initialize

## fx_media_flush

Flushes all cached data to the physical media

### Prototype

```c
UINT fx_media_flush(FX_MEDIA *media_ptr);
```

### Description

This service flushes all cached sectors and directory entries of any modified files to the physical media.

> **Warning:** *This routine may be called periodically by the application to reduce the risk of file corruption and/or data loss in the event of a sudden loss of power on the target.*

### Input Parameters

- *media_ptr*: Pointer to media control block.

### Return Values

- **FX_SUCCESS** (0x00) Successful media flush.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_FILE_CORRUPT**    (0x08) File is corrupted.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_PTR_ERROR** (0x18) Invalid media pointer.
- **FX_CALLER_ERROR**    (0x20)    Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA         my_media;
UINT             status;

/* Flush all cached sectors and modified file entries to the physical media. */

status = fx_media_flush(&my_media);

/* If status equals FX_SUCCESS, the physical media is completely up-to-date. */

```

### See Also

- fx_fault_tolerant_enable
- fx_media_abort
- fx_media_cache_invalidate
- fx_media_check
- fx_media_close
- fx_media_close_notify_set
- fx_media_exFAT_format
- fx_media_extended_space_available
- fx_media_format
- fx_media_open
- fx_media_open_notify_set
- fx_media_read
- fx_media_space_available
- fx_media_volume_get
- fx_media_volume_set
- fx_media_write
- fx_system_initialize

## fx_media_format

Formats the specified media.

### Prototype

```c
UINT fx_media_format(
    FX_MEDIA *media_ptr,
    VOID (*driver)(FX_MEDIA *media),
    VOID *driver_info_ptr,
    UCHAR *memory_ptr, 
    UINT memory_size,
    CHAR *volume_name, 
    UINT number_of_fats,
    UINT directory_entries, 
    UINT hidden_sectors,
    ULONG total_sectors, 
    UINT bytes_per_sector,
    UINT sectors_per_cluster,
    UINT heads,
    UINT sectors_per_track);
```

### Description

This service formats the supplied media in a FAT 12/16/32 compatible manner based on the supplied parameters. This service must be called prior to opening the media.

> **Warning:** *Formatting an already formatted media effectively erases all files and directories on the media.*

### Input Parameters

- *media_ptr*: Pointer to media control block. This is used only to provide some basic information necessary for the driver to operate.
- *driver*: Pointer to the I/O driver for this media. This will typically be the same driver supplied to the subsequent fx_media_open call.
- *driver_info_ptr*: Pointer to optional information that the I/O driver may utilize.
- *memory_ptr*: Pointer to the working memory for the media.
- *memory_size*: Specifies the size of the working media memory. The size must be at least as large as the media's sector size.
- *volume_name*: Pointer to the volume name string, which is a maximum of 11 characters.
- *number_of_fats*: Number of FATs in the media. The minimal value is 1 for the primary FAT. Values greater than 1 result in additional FAT copies being maintained at run-time.
- *directory_entries*: Number of directory entries in the root directory.
- *hidden_sectors*: Number of sectors hidden before this media's boot sector. This is typical when multiple partitions are present.
- *total_sectors*: Total number of sectors in the media.
- *bytes_per_sector**: Number of bytes per sector, which is typically 512. FileX requires this to be a multiple of 32.
> **Important:** *With reference to the specification, the bytes per sector may take on only the following values: 512, 1024, 2048 or 4096.*

### Input Parameters

- *media_ptr*: Pointer to the media control block. This is used only to provide some basic information necessary for the driver to operate.
- *driver*: Pointer to the I/O driver for this media. This will typically be the same driver supplied to the subsequent fx_media_open call.
- *driver_info_ptr*: Pointer to the optional information that the I/O driver may utilize.
- *memory_ptr*: Pointer to the working memory for the media.
- *memory_size*: Specifies the size of the working media memory. The size must be at least as large as the media's sector size.
- *volume_name*: Pointer to the volume name string, which is a maximum of 11 characters.
- *number_of_fats*: Number of FATs in the media. The minimal value is 1 for the primary FAT. Values greater than 1 result in additional FAT copies being maintained at run-time.
- *directory_entries*: Number of directory entries in the root directory.
- *hidden_sectors*: Number of sectors hidden before this media's boot sector. This is typical when multiple partitions are present.
- *total_sectors*: Total number of sectors in the media.
- *bytes_per_sector*: Number of bytes per sector, which is typically 512. FileX requires this to be a multiple of 32.
- *sectors_per_cluster*: Number of sectors in each cluster. The cluster is the minimum allocation unit in a FAT file system.
- *heads*: Number of physical heads.
- *sectors_per_track*: Number of sectors per track.

### Return Values

- **FX_SUCCESS** (0x00) Successful media format.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_PTR_ERROR** (0x18) Invalid media, driver, or memory pointer.
- **FX_CALLER_ERROR**    (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA         ram_disk;
UCHAR             media_memory[512];
UCHAR             ram_disk_memory[32768];

/* Format a RAM disk with 32768 bytes and 512 bytes per sector. */

status = fx_media_format(&ram_disk, _fx_ram_driver,
                         ram_disk_memory, media_memory,
                         sizeof(media_memory),
                         "MY_RAM_DISK" /* Volume Name */,
                         1 /* Number of FATs */,
                         32 /* Directory Entries */,
                         0 /* Hidden sectors */,
                         64 /* Total sectors */,
                         512 /* Sector size */,
                         1 /* Sectors per cluster */,
                         1 /* Heads */,
                         1 /* Sectors per track */);

/* If status is FX_SUCCESS, the media was successfully formatted
    and can now be opened with the following call: */

```

### See Also

- fx_fault_tolerant_enable
- fx_media_abort
- fx_media_cache_invalidate
- fx_media_check
- fx_media_close
- fx_media_close_notify_set
- fx_media_exFAT_format
- fx_media_extended_space_available
- fx_media_flush
- fx_media_open
- fx_media_open_notify_set
- fx_media_read
- fx_media_space_available
- fx_media_volume_get
- fx_media_volume_set
- fx_media_write
- fx_system_initialize

## fx_media_open

Opens the specified media for file access.

### Prototype

```c
UINT fx_media_open(
    FX_MEDIA *media_ptr, 
    CHAR *media_name,
    VOID(*media_driver)(FX_MEDIA *),
    VOID *driver_info_ptr,
    VOID *memory_ptr, 
    ULONG memory_size);
```

### Description

This service opens a media for file access using the supplied I/O driver.

> **Warning:** *The memory supplied to this service is used to implement an internal logical sector cache, hence, the more memory supplied the more physical I/O is reduced. FileX requires a cache of at least one logical sector (bytes per sector of the media).*

### Input Parameters

- *media_ptr*: Pointer to media control block.
- *media_name*: Pointer to media's name.
- *media_driver*: Pointer to I/O driver for this media. The I/O driver must conform to FileX driver requirements defined in Chapter 5.
- *driver_info_ptr*: Pointer to optional information that the supplied I/O driver may utilize.
- *memory_ptr*: Pointer to the working memory for the media.
- *memory_size*: Specifies the size of the working media memory. The size must be as large as the media's sector size (typically 512 bytes).

### Return Values

- **FX_SUCCESS** (0x00) Successful media open.
- **FX_BOOT_ERROR** (0x01) Error reading the media's boot sector.
- **FX_MEDIA_INVALID** (0x02) Specified media's boot sector is corrupt or invalid. In addition, this return code is used to indicate that either the logical sector cache size or the FAT entry size is not a power of 2.
- **FX_FAT_READ_ERROR** (0x03) Error reading the media FAT.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_PTR_ERROR** (0x18) One or more pointers are NULL.
- **FX_CALLER_ERROR** (0x20)    Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA         ram_disk,
UINT             status;
UCHAR             buffer[128];
/* Open a 32KByte RAM disk starting at the fixed address of 0x800000.
    Note that the total 32KByte media size and 128-byte sector size is defined inside of the driver. */

status = fx_media_open(&ram_disk, "RAM DISK", fx_ram_driver, 0, &buffer[0], sizeof(buffer));

/* If status equals FX_SUCCESS, the RAM disk has been successfully setup and is ready for file access! */

```

### See Also

- fx_fault_tolerant_enable
- fx_media_abort
- fx_media_cache_invalidate
- fx_media_check
- fx_media_close
- fx_media_close_notify_set
- fx_media_exFAT_format
- fx_media_extended_space_available
- fx_media_flush
- fx_media_format
- fx_media_open_notify_set
- fx_media_read
- fx_media_space_available
- fx_media_volume_get
- fx_media_volume_set
- fx_media_write
- fx_system_initialize

## fx_media_open_notify_set

Sets the media open-notification callback function.

### Prototype

```c
UINT fx_media_open_notify_set(
    FX_MEDIA *media_ptr,
    VOID (*media_open_notify)(FX_MEDIA*));
```

### Description

This service sets a notification callback function which will be invoked after a media is successfully opened.

### Input Parameters

- *media_ptr*: Pointer to the media control block.
- *media_open_notify*: Media open-notification callback function to be installed. Passing **NULL** as the callback function disables the media open callback.

### Return Values


- **FX_SUCCESS** (0x00) Successfully installed the callback function.
- **FX_PTR_ERROR** (0x18) media_ptr is **NULL**.
- **FX_CALLER_ERROR**    (0x20)    Caller is not a thread.

### Allowed From

Threads

### Example

```c
fx_media_open_notify_set(media_ptr, my_media_open_callback);

```

### See Also

- fx_fault_tolerant_enable
- fx_media_abort
- fx_media_cache_invalidate
- fx_media_check
- fx_media_close
- fx_media_close_notify_set
- fx_media_exFAT_format
- fx_media_extended_space_available
- fx_media_flush
- fx_media_format
- fx_media_open
- fx_media_open_notify_set
- fx_media_space_available
- fx_media_volume_get
- fx_media_volume_set
- fx_media_write
- fx_system_initialize

## fx_media_read

Reads a logical sector from the media.

### Prototype

```c
UINT fx_media_read(
    FX_MEDIA *media_ptr, 
    ULONG logical_sector, 
    VOID *buffer_ptr);
```

### Description

This service reads a logical sector from the media and places it into the supplied buffer.

### Input Parameters

- *media_ptr*: Pointer to a previously-opened media.
- *logical_sector*: Logical sector to read.
- *buffer_ptr*: Pointer to the destination for the logical sector read.

### Return Values

- **FX_SUCCESS** (0x00) Successful media read.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_PTR_ERROR** (0x18) Invalid media or buffer pointer.
- **FX_CALLER_ERROR** (0x20)    Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA         my_media;
UCHAR             my_buffer[128];
UINT            STATUS;
/* Read logical sector 22 into "my_buffer" assuming the
    physical media has a sector size of 128. */
status = fx_media_read(&my_media, 22, my_buffer);
/* If status equals FX_SUCCESS, the contents of logical sector 22 are in "my_buffer". */
```

### See Also

- fx_fault_tolerant_enable
- fx_media_abort
- fx_media_cache_invalidate
- fx_media_check
- fx_media_close
- fx_media_close_notify_set
- fx_media_exFAT_format
- fx_media_extended_space_available
- fx_media_flush
- fx_media_format
- fx_media_open
- fx_media_open_notify_set
- fx_media_space_available
- fx_media_volume_get
- fx_media_volume_set
- fx_media_write
- fx_system_initialize

## fx_media_space_available

Returns the available media space in bytes.

### Prototype

```c
UINT fx_media_space_available(
    FX_MEDIA *media_ptr,
    ULONG *available_bytes_ptr);
```

### Description

This service returns the number of bytes available in the media.

To work with the media larger than 4GB, application shall use the service ***fx_media_extended_space_available***.

### Input Parameters

- *media_ptr*: Pointer to a previously-opened media.
- *available_bytes_ptr*: Available bytes left in the media.

### Return Values

- **FX_SUCCESS** (0x00) Successfully returned available space on media.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_PTR_ERROR** (0x18) Invalid media pointer or available bytes pointer is NULL.
- **FX_CALLER_ERROR**    (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA        my_media;
ULONG            available_bytes;
UINT            status;
/* Retrieve the available bytes in the media. */

status = fx_media_space_available(&my_media, &available_bytes);

/* If status equals FX_SUCCESS, the number of available bytes is in "available_bytes." */

```

### See Also

- fx_fault_tolerant_enable
- fx_media_abort
- fx_media_cache_invalidate
- fx_media_check
- fx_media_close
- fx_media_close_notify_set
- fx_media_exFAT_format
- fx_media_extended_space_available
- fx_media_flush
- fx_media_format
- fx_media_open
- fx_media_open_notify_set
- fx_media_read
- fx_media_volume_get
- fx_media_volume_set
- fx_media_write
- fx_system_initialize

## fx_media_volume_get

Gets the media's volume name.

### Prototype

```c
UINT fx_media_volume_get(
    FX_MEDIA *media_ptr,
    CHAR *volume_name,
    UINT volume_source);
```

### Description

This service retrieves the volume name of the previously-opened media.

### Input Parameters

- *media_ptr*: Pointer to the media control block.
- *volume_name*: Pointer to destination for volume name. Note that the destination must be at least large enough to hold 12 characters.
- *volume_source*: Designates where to retrieve the name, either from the boot sector or the root directory. Valid values for this parameter are:
  - **FX_BOOT_SECTOR**
  - **FX_DIRECTORY_SECTOR**

### Return Values

- **FX_SUCCESS** (0x00) Successful media volume get.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_NOT_FOUND** (0x04) Volume not found.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_PTR_ERROR** (0x18) Invalid media or volume destination pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA        ram_disk;
UCHAR             volume_name[12];
/* Retrieve the volume name of the RAM disk, from the boot sector. */

status = fx_media_volume_get_extended(&ram_disk, volume_name,
                                      sizeof(volume_name), FX_BOOT_SECTOR);

/* If status is FX_SUCCESS, the volume name was successfully retrieved. */

```

### See Also

- fx_fault_tolerant_enable
- fx_media_abort
- fx_media_cache_invalidate
- fx_media_check
- fx_media_close
- fx_media_close_notify_set
- fx_media_exFAT_format
- fx_media_extended_space_available
- fx_media_flush
- fx_media_format
- fx_media_open
- fx_media_open_notify_set
- fx_media_read
- fx_media_space_available
- fx_media_volume_set
- fx_media_write
- fx_system_initialize

## fx_media_volume_get_extended

Gets the volume name of previously-opened media.

### Prototype

```c
UINT fx_media_volume_get_extended(
    FX_MEDIA *media_ptr, 
    CHAR *volume_name,
    UINT volume_name_buffer_length,
    UINT volume_source);
```

### Description

This service retrieves the volume name of the previously opened media.

> **Important:** This service is identical to ***fx_media_volume_get*** except the caller passes in the size of the *volume_name* buffer.

### Input Parameters

- *media_ptr*: Pointer to the media control block.
- *volume_name*: Pointer to the destination for volume name. The destination must be at least large enough to hold 12 characters.
- *volume_name_buffer_length*: Size of volume_name buffer.
- *volume_source*: Designates where to retrieve the name, either from the boot sector or the root directory. Valid values for this parameter are:
  - **FX_BOOT_SECTOR**
  - **FX_DIRECTORY_SECTOR**

### Return Values

- **FX_SUCCESS** (0x00) Successful media volume get.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_NOT_FOUND** (0x04) Volume not found.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_PTR_ERROR** (0x18) Invalid media or volume destination pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA         ram_disk;
UCHAR             volume_name[12];

/* Retrieve the volume name of the RAM disk, from the boot sector. */

status = fx_media_volume_get_extended(&ram_disk, volume_name,
                                      sizeof(volume_name),
                                      FX_BOOT_SECTOR);

/* If status is FX_SUCCESS, the volume name was successfully retrieved. */
```

### See Also

- fx_fault_tolerant_enable
- fx_media_abort
- fx_media_cache_invalidate
- fx_media_check
- fx_media_close
- fx_media_close_notify_set
- fx_media_exFAT_format
- fx_media_extended_space_available
- fx_media_flush
- fx_media_format
- fx_media_open
- fx_media_open_notify_set
- fx_media_read
- fx_media_space_available
- fx_media_volume_set
- fx_media_write
- fx_system_initialize

## fx_media_volume_set

Sets the media's volume name

### Prototype

```c
UINT fx_media_volume_set(
    FX_MEDIA *media_ptr, 
    CHAR *volume_name);
```

### Description

This service sets the volume name of the previously-opened media.

### Input Parameters

- *media_ptr*: Pointer to the media control block.
- *volume_name*: Pointer to the volume name.

### Return Values

- **FX_SUCCESS** (0x00) Successful media volume set.
- **FX_INVALID_NAME** (0x0C) Volume_name is invalid.
- **FX_MEDIA_INVALID** (0x02) Unable to set volume name.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_PTR_ERROR** (0x18) Invalid media or volume name pointer.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA ram_disk;

/* Set the volume name to "MY_VOLUME". */

status = fx_media_volume_set(&ram_disk, "MY_VOLUME");

/* If status is FX_SUCCESS, the volume name was successfully set. */
```

### See Also

- fx_fault_tolerant_enable
- fx_media_abort
- fx_media_cache_invalidate
- fx_media_check
- fx_media_close
- fx_media_close_notify_set
- fx_media_exFAT_format
- fx_media_extended_space_available
- fx_media_flush
- fx_media_format
- fx_media_open
- fx_media_open_notify_set
- fx_media_read
- fx_media_space_available
- fx_media_volume_get
- fx_media_write
- fx_system_initialize

## fx_media_write

Writes a logical sector to the media.

### Prototype

```c
UINT fx_media_write(
    FX_MEDIA *media_ptr, 
    ULONG logical_sector,
    VOID *buffer_ptr);
```

### Description

This service writes the supplied buffer to the specified logical sector.

### Input Parameters

- *media_ptr*: Pointer to a previously-opened media.
- *logical_sector*: Logical sector to write.
- *buffer_ptr*: Pointer to the source for the logical sector write.

### Return Values

- **FX_SUCCESS** (0x00) Successful media write.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_PTR_ERROR** (0x18) Invalid media pointer.
- **FX_CALLER_ERROR**    (0x20)    Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA             my_media;
UCHAR                 my_buffer[128];
UINT                 status;

/* Write logical sector 22 from "my_buffer" assuming
    the physical media has a sector size of 128. */

status = fx_media_write(&my_media, 22, my_buffer);

/* If status equals FX_SUCCESS, the contents of logical
    sector 22 are now the same as "my_buffer." */
```

### See Also

- fx_fault_tolerant_enable
- fx_media_abort
- fx_media_cache_invalidate
- fx_media_check
- fx_media_close
- fx_media_close_notify_set
- fx_media_exFAT_format
- fx_media_extended_space_available
- fx_media_flush
- fx_media_format
- fx_media_open
- fx_media_open_notify_set
- fx_media_read
- fx_media_space_available
- fx_media_volume_get
- fx_media_volume_set
- fx_system_initialize

## fx_system_date_get

Gets the file system date.

### Prototype

```c
UINT fx_system_date_get(
    UINT *year, 
    UINT *month, 
    UINT *day);
```

### Description

This service returns the current system date.

### Input Parameters

- *year*: Pointer to destination for year.
- *month*: Pointer to destination for month.
- *day*: Pointer to destination for day.

### Return Values

- **FX_SUCCESS** (0x00) Successful date retrieval.
- **FX_PTR_ERROR** (0x18) One or more of the input parameters are NULL.

### Allowed From

Threads

### Example

```c
UINT            status;
UINT            year;
UINT            month;
UINT            day;
/* Retrieve the current system date. */

status = fx_system_date_get(&year, &month, &day);

/* If status equals FX_SUCCESS, the year, month,
    and day parameters now have meaningful information. */
```

### See Also

- fx_system_date_set
- fx_system_initialize
- fx_system_time_get
- fx_system_time_set

## fx_system_date_set

Sets the system date.

### Prototype

```c
UINT fx_system_date_set(
    UINT year, 
    UINT month, 
    UINT day);
```

### Description

This service sets the system date as specified.

> **Warning:** *This service should be called shortly after invoking the ***fx_system_initialize*** function to set the initial system date. By default, the system date is that of the last generic FileX release.*

### Input Parameters

- *year*: New year. The valid range is from 1980 through the year 2107.
- *month*: New month. The valid range is from 1 through 12.
- *day*: New day. The valid range is from 1 through 31, depending on month and leap year conditions.

### Return Values

- **FX_SUCCESS** (0x00) Successful date setting.
- **FX_INVALID_YEAR** (0x12) Invalid year specified.
- **FX_INVALID_MONTH** (0x13) Invalid month specified.
- **FX_INVALID_DAY** (0x14) Invalid day specified.

### Allowed From

Initialization, threads

### Example

```c
 UINT             status;

/* Set the system date to December 12, 2005. */

status = fx_system_date_set(2005, 12, 12);

/* If status equals FX_SUCCESS, the file system date is now
    12-12-2005. */
```

### See Also

- fx_system_date_get
- fx_system_initialize
- fx_system_time_get
- fx_system_time_set

## fx_system_initialize

Initializes the entire FileX system.

### Prototype

```c
VOID fx_system_initialize(void);
```

### Description

This service initializes all the major FileX data structures. It should be called either in ***tx_application_define*** or possibly from an initialization thread and must be called prior to using any other FileX service.

> **Warning:** *Once initialized by this call, the application should call ***fx_system_date_set*** and **fx_system_time_set** to start with an accurate system date and time.*

### Input Parameters

None

### Return Values

None

### Allowed From

Initialization, threads

### Example

```c
void tx_application_define(VOID *free_memory)
{
    UINT status;
    /* Initialize the FileX system. */
    fx_system_initialize();
    /* Set the file system date. */
    fx_system_date_set(my_year, my_month, my_day);

    /* Set the file system time. */
    fx_system_time_set(my_hour, my_minute, my_second);

    /* Now perform all other initialization and possibly FileX media
        open calls if the corresponding driver does not block on the boot sector read. */

    ...

}
```

### See Also

- fx_system_date_get
- fx_system_date_set
- fx_system_time_get
- fx_system_time_set

## fx_system_time_get

Gets the current system time.

### Prototype

```c
UINT fx_system_time_get(
    UINT *hour,
    UINT *minute,
    UINT *second);
```

### Description

This service retrieves the current system time.

### Input Parameters

- *hour*: Pointer to destination for hour.
- *minute*: Pointer to destination for minute.
- *second*: Pointer to destination for second.

### Return Values

- **FX_SUCCESS** (0x00) Successful system time retrieval.
- **FX_PTR_ERROR** (0x18) One or more of the input parameters

### Allowed From

Threads

### Example

```c
UINT             status;
UINT             hour;
UINT             minute;
UINT             second;

/* Retrieve the current system time. */

status = fx_system_time_get(&hour, &minute, &second);

/* If status equals FX_SUCCESS, the current system time
    is in the hour, minute, and second variables. */
```

### See Also

- fx_system_date_get
- fx_system_date_set
- fx_system_initialize
- fx_system_time_set

## fx_system_time_set

Sets the current system time.

### Prototype

```c
UINT fx_system_time_set(
    UINT hour, 
    UINT minute, 
    UINT second);
```

### Description

This service sets the current system time to that specified by the input parameters.

> **Warning:** *This service should be called shortly after invoking the ***fx_system_initialize*** function to set the initial system time. By default, the system time is 0:0:0.*

### Input Parameters

- *hour*: New hour (0-23).
- *minute*: New minute (0-59).
- *second*: New second (0-59).

### Return Values

- **FX_SUCCESS** (0x00) Successful system time retrieval.
- **FX_INVALID_HOUR**    (0x15) New hour is invalid.
- **FX_INVALID_MINUTE** (0x16) New minute is invalid.
- **FX_INVALID_SECOND** (0x17) New second is invalid.

### Allowed From

Threads

### Example

```c
 UINT status;

/* Set the current system time to hour 23, minute 21, and second 20. */

status = fx_system_time_set(23, 21, 20);

/* If status is FX_SUCCESS, the current system time has been set. */
```

### See Also

- fx_system_date_get
- fx_system_date_set
- fx_system_initialize
- fx_system_time_get

## fx_unicode_directory_create

Creates a Unicode-named directory.

### Prototype

```c
UINT fx_unicode_directory_create(
    FX_MEDIA *media_ptr, 
    UCHAR *source_unicode_name,
    ULONG source_unicode_length, 
    CHAR *short_name);
```

### Description

This service creates a Unicode-named subdirectory in the current default directory—no path information is allowed in the Unicode source name parameter. If successful, the short name (8.3 format) of the newly created Unicode subdirectory is returned by the service.

> **Warning:** *All operations on the Unicode subdirectory (making it the default path, deleting, etc.) should be done by supplying the returned short name (8.3 format) to the standard FileX directory services.*

> **Important:** *This service is not supported on exFAT media.*

### Input Parameters

- *media_ptr*: Pointer to media control block.
- *source_unicode_name*: Pointer to the Unicode name for the new subdirectory.
- *source_unicode_length*: Length of Unicode name.
- *short_name*: Pointer to destination for short name (8.3 format) for the new Unicode subdirectory.

### Return Values

- **FX_SUCCESS** (0x00) Successful Unicode directory create.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_ALREADY_CREATED** (0x0B) Specified directory already exists.
- **FX_NO_MORE_SPACE** (0x0A) No more clusters available in the media for the new directory entry.
- **FX_NOT_IMPLEMENTED** (0x22) Service not implemented for exFAT file system.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_PTR_ERROR** (0x18) Invalid media or name pointers.
- **FX_CALLER_ERROR** (0x20)    Caller is not a thread.
- **FX_IO_ERROR**    (0x90) Driver I/O error.

### Allowed From

Threads

### Example

```c
FX_MEDIA             ram_disk;
UCHAR                 my_short_name[13];
UCHAR                 my_unicode_name[] = {0x38,0xC1, 0x88,0xBC, 0xF8,0xC9, 0x20,0x00,
                                         0x54,0xD6, 0x7C,0xC7, 0x20,0x00, 0x74,0xC7,
                                         0x84,0xB9, 0x20,0x00, 0x85,0xC7, 0xC8,0xB2,
                                         0xE4,0xB2, 0x2E,0x00, 0x64,0x00, 0x6F,0x00,
                                         0x63,0x00, 0x00,0x00};

/* Create a Unicode subdirectory with the name contained in "my_unicode_name". */

length = fx_unicode_directory_create(&ram_disk, my_unicode_name, 17, my_short_name);

/* If successful, the Unicode subdirectory is created and "my_short_name"
    contains the 8.3 format name that can be used with other FileX services. */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_rename

## fx_unicode_directory_rename

Renames directory using a Unicode string.

### Prototype

```c
UINT fx_unicode_directory_rename(
    FX_MEDIA *media_ptr, 
    UCHAR *old_unicode_name,
    ULONG old_unicode_length, 
    UCHAR *new_unicode_name,
    ULONG new_unicode_length,
    CHAR *new_short_name);
```

### Description

This service changes a Unicode-named subdirectory to specified new Unicode name in current working directory. The Unicode name parameters must not have path information.

> **Important:** *This service is not supported on exFAT media.*

### Input Parameters

- *media_ptr*: Pointer to media control block.
- *old_unicode_name*: Pointer to the Unicode name for the current file.
- *old_unicode_name_length*: Length of current Unicode name.
- *new_unicode_name*: Pointer to the new Unicode file name.
- *old_unicode_name_length*: Length of new Unicode name.
- *new_short_name*: Pointer to destination for short name (8.3 format) for the renamed Unicode file. Rename Directory with Unicode

### Return Values

- **FX_SUCCESS** (0x00) Successful media open.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_ALREADY_CREATED** (0x0B) Specified directory name already exists.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_PTR_ERROR** (0x18) One or more pointers are NULL.
- **FX_CALLER_ERROR** (0x20)    Caller is not a thread.
- **FX_WRITE_PROTECT** (0x23) Specified media is write-protected.

### Allowed From

Threads

### Example

```c
FX_MEDIA             my_media;
UINT                 status;
UCHAR                 my_short_name[13];
UCHAR                 my_old_unicode_name[] = {'a', '\0', 'b', '\0', 'c', '\0', '\0', '\0'};
UCHAR                 my_new_unicode_name[] = {'d', '\0', 'e', '\0', 'f', '\0', '\0', '\0'};

/* Change the Unicode-named file "abc" to "def". */

status = fx_unicode_directory_rename(&my_media, my_old_unicode_name,
    3, my_new_unicode_name, 3, my_short_name);

/* If status equals FX_SUCCESS, the directory was changed to "def" and
    "my_short_name" contains the 8.3 format name that can be used with other FileX services. */
```

### See Also

- fx_directory_attributes_read
- fx_directory_attributes_set
- fx_directory_create
- fx_directory_default_get
- fx_directory_default_set
- fx_directory_delete
- fx_directory_first_entry_find
- fx_directory_first_full_entry_find
- fx_directory_information_get
- fx_directory_local_path_clear
- fx_directory_local_path_get
- fx_directory_local_path_restore
- fx_directory_local_path_set
- fx_directory_long_name_get
- fx_directory_name_test
- fx_directory_next_entry_find
- fx_directory_next_full_entry_find
- fx_directory_rename
- fx_directory_short_name_get
- fx_unicode_directory_create

## fx_unicode_file_create

Creates a Unicode-named file.

### Prototype

```c
UINT fx_unicode_file_create(
    FX_MEDIA *media_ptr, 
    UCHAR *source_unicode_name,
    ULONG source_unicode_length, 
    CHAR *short_name);
```

### Description

This service creates a Unicode-named file in the current default directory—no path information is allowed in the Unicode source name parameter. If successful, the short name (8.3 format) of the newly created Unicode file is returned by the service.

> **Warning:** *All operations on the Unicode file (opening, writing, reading, closing, etc.) should be done by supplying the returned short name (8.3 format) to the standard FileX file services.*

### Input Parameters

- *media_ptr*: Pointer to media control block.
- *source_unicode_name*: Pointer to the Unicode name for the new file.
- *source_unicode_length*: Length of Unicode name.
- *short_name*: Pointer to destination for short name (8.3 format) for the new Unicode file.

### Return Values

- **FX_SUCCESS** (0x00) Successful file create.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_ALREADY_CREATED** (0x0B) Specified file already exists.
- **FX_NO_MORE_SPACE** (0x0A) No more clusters available in the media for the new file entry.
- **FX_NOT_IMPLEMENTED** (0x22) Service not implemented for exFAT file system.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_WRITE_PROTECT** (0x23) Specified media is write protected.
- **FX_PTR_ERROR** (0x18) Invalid media or name pointers.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA         ram_disk;
UCHAR             my_short_name[13];
UCHAR             my_unicode_name[] = {0x38,0xC1, 0x88,0xBC, 0xF8,0xC9, 0x20,0x00,
                                     0x54,0xD6, 0x7C,0xC7, 0x20,0x00, 0x74,0xC7,
                                     0x84,0xB9, 0x20,0x00, 0x85,0xC7, 0xC8,0xB2,
                                     0xE4,0xB2, 0x2E,0x00, 0x64,0x00, 0x6F,0x00,
                                     0x63,0x00, 0x00,0x00};

/* Create a Unicode file with the name contained in "my_unicode_name". */

length = fx_unicode_file_create(&ram_disk, my_unicode_name, 17, my_short_name);

/* If successful, the Unicode file is created and "my_short_name"
    contains the 8.3 format name that can be used with other FileX services. */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_unicode_file_rename

Renames a file using a unicode string.

### Prototype

```c
UINT fx_unicode_file_rename(
    FX_MEDIA *media_ptr, 
    UCHAR *old_unicode_name,
    ULONG old_unicode_length, 
    UCHAR *new_unicode_name,
    ULONG new_unicode_length, 
    CHAR *new_short_name);
```

### Description

This service changes a Unicode-named file name to specified new Unicode name in current default directory. The Unicode name parameters must not have path information.

> **Important:** *This service is not supported on exFAT media.*

### Input Parameters

- *media_ptr*: Pointer to media control block.
- *old_unicode_name*: Pointer to the Unicode name for the current file.
- *old_unicode_name_length**: Length of current Unicode name.
- *new_unicode_name*: Pointer to the new Unicode file name.
- *new_unicode_name_length*: Length of new Unicode name.
- *new_short_name*: Pointer to destination for short name (8.3 format) for the renamed Unicode file.

### Return Values

- **FX_SUCCESS** (0x00) Successful media open.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_ALREADY_CREATED** (0x0B) Specified file name already exists.
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_PTR_ERROR** (0x18) One or more pointers are NULL.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.
- **FX_WRITE_PROTECT** (0x23) Specified media is write-protected.

### Allowed From

Threads

### Example

```c
FX_MEDIA             my_media;
UINT                 status;
UCHAR                 my_short_name[13];
UCHAR                 my_old_unicode_name[] = {'a', '\0', 'b', '\0', 'c', '\0', '\0', '\0'};
UCHAR                 my_new_unicode_name[] = {'d', '\0', 'e', '\0', 'f', '\0', '\0', '\0'};

/* Change the Unicode-named file "abc" to "def". */

status = fx_unicode_file_rename(&my_media, my_old_unicode_name,
    3, my_new_unicode_name, 3, my_short_name);

/* If status equals FX_SUCCESS, the file name was changed to "def" and
    "my_short_name" contains the 8.3 format name that can be used with other FileX services. */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_unicode_length_get

Gets the length of a Unicode name.

### Prototype

```c
ULONG fx_unicode_length_get(UCHAR *unicode_name);
```

### Description

This service determines the length of the supplied Unicode name. A Unicode character is represented by two bytes. A Unicode name is a series of two byte Unicode characters terminated by two **NULL** bytes (two bytes of 0 value).

### Input Parameters

*unicode_name*: Pointer to the Unicode name.

### Return Values

**length**: Length of Unicode name (number of Unicode characters in the name).

### Allowed From

Threads

### Example

```c
UCHAR     my_unicode_name[] = {0x38,0xC1, 0x88,0xBC, 0xF8,0xC9, 0x20,0x00,
                           0x54,0xD6, 0x7C,0xC7, 0x20,0x00, 0x74,0xC7,
                           0x84,0xB9, 0x20,0x00, 0x85,0xC7, 0xC8,0xB2,
                           0xE4,0xB2, 0x2E,0x00, 0x64,0x00, 0x6F,0x00,
                           0x63,0x00, 0x00,0x00};

UINT     length;

/* Get the length of "my_unicode_name". */

length = fx_unicode_length_get(my_unicode_name);

/* A value of 17 will be returned for the length of the "my_unicode_name". */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_unicode_length_get_extended

Gets length of a Unicode name from a buffer of a specified length.

### Prototype

```c
UINT fx_unicode_length_get_extended(
    UCHAR *unicode_name,
    UINT buffer_length);
```

### Description

This service gets the length of the supplied Unicode name. A Unicode character is represented by two bytes. A Unicode name is a series of two-byte Unicode characters terminated by two **NULL** bytes (two bytes of 0 value).

> **Important:** *This service is identical to ***fx_unicode_length_get*** except the caller passes in the size of the *unicode_name* buffer, including the two **NULL** characters.*

### Input Parameters

- *unicode_name*: Pointer to the Unicode name.
- *buffer_length*: Size of the Unicode name buffer.

### Return Values

**length**: Length of Unicode name (number of Unicode characters in the name).

### Allowed From

Threads

### Example

```c
UCHAR         my_unicode_name[] = {0x38,0xC1, 0x88,0xBC, 0xF8,0xC9, 0x20,0x00,
                                 0x54,0xD6, 0x7C,0xC7, 0x20,0x00, 0x74,0xC7,
                                 0x84,0xB9, 0x20,0x00, 0x85,0xC7, 0xC8,0xB2,
                                 0xE4,0xB2, 0x2E,0x00, 0x64,0x00, 0x6F,0x00,
                                 0x63,0x00, 0x00,0x00};

UINT         length;

/* Get the length of "my_unicode_name". */

length = fx_unicode_length_get_extended(my_unicode_name, sizeof(my_unicode_name));

/* A value of 17 will be returned for the length of the "my_unicode_name". */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

## fx_unicode_name_get

Gets the Unicode name from a short name.

### Prototype

```c
UINT fx_unicode_name_get(
    FX_MEDIA *media_ptr, 
    CHAR *source_short_name,
    UCHAR *destination_unicode_name, 
    ULONG *destination_unicode_length);
```

### Description

This service retrieves the Unicode name associated with the supplied short name (8.3 format) within the current default directory—no path information is allowed in the short name parameter. If successful, the Unicode name associated with the short name is returned by the service.

> **Important:** *This service can be used to get Unicode names for both files and subdirectories.*

### Input Parameters

- *media_ptr*: Pointer to media control block.
- *short_name* Pointer to short name (8.3 format).
- *destination_unicode_name*: Pointer to the destination for the Unicode name associated with the supplied short name.
- *destination_unicode_length*: Pointer to returned Unicode name length.

### Return Values

- **FX_SUCCESS** (0x00) Successful Unicode name retrieval.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT table.
- **FX_FILE_CORRUPT** (0x08) File is corrupted
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_NOT_FOUND** (0x04) Short name was not found or the Unicode destination size is too small.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_PTR_ERROR** (0x18) Invalid media or name pointers.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA             ram_disk;
UCHAR                 my_unicode_name[256*2];
ULONG                 unicode_name_length;

/* Get the Unicode name associated with the short name "ABC0~111.TXT".
    Note that the Unicode destination must have 2 times the
    number of maximum characters in the name. */

length = fx_unicode_name_get(&ram_disk, "ABC0~111.TXT", my_unicode_name, unicode_name_length);

/* If successful, the Unicode name is returned in "my_unicode_name". */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_short_name_get

## fx_unicode_name_get_extended

Gets the Unicode name from a short name in a buffer of a specified length.

### Prototype

```c
UINT fx_unicode_name_get_extended(
    FX_MEDIA *media_ptr,
    CHAR *source_short_name,
    UCHAR *destination_unicode_name,
    ULONG *destination_unicode_length,
    ULONG unicode_name_buffer_length);
```

### Description

This service retrieves the Unicode-name associated with the supplied short name (8.3 format) within the current default directory—no path information is allowed in the short name parameter. If successful, the Unicode name associated with the short name is returned by the service.

> **Important:** *This service is identical to ***fx_unicode_name_get***, except the caller supplies the size of the destination Unicode buffer as an input argument. This allows the service to guarantee that it will not overwrite the destination Unicode buffer*

> **Important:** *This service can be used to get Unicode names for both files and subdirectories.*

### Input Parameters

- *media_ptr*: Pointer to media control block.
- *short_name*: Pointer to short name (8.3 format).
- *destination_unicode_name*: Pointer to the destination for the Unicode name associated with the supplied short name.
- *destination_unicode_length*: Pointer to returned Unicode name length.
- *unicode_name_buffer_length*: Size of the Unicode name buffer. Note: A NULL terminator is required, which makes an extra byte.

### Return Values

- **FX_SUCCESS** (0x00) Successful Unicode name retrieval.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT table.
- **FX_FILE_CORRUPT** (0x08) File is corrupted
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_NOT_FOUND** (0x04) Short name was not found or the Unicode destination size is too small.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_PTR_ERROR** (0x18) Invalid media or name pointers.
- **FX_CALLER_ERROR** (0x20) Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA         ram_disk;
UCHAR             my_unicode_name[256*2];
ULONG             unicode_name_length;

/* Get the Unicode name associated with the short name "ABC0~111.TXT".
    Note that the Unicode destination must have 2 times the number of maximum characters in the name. */

length = fx_unicode_name_get_extended(&ram_disk, "ABC0~111.TXT",
    my_unicode_name,&unicode_name_length, sizeof(ny_unicode_name));

/* If successful, the Unicode name is returned in "my_unicode_name". */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_short_name_get

## fx_unicode_short_name_get

Gets the short name from a Unicode name.

### Prototype

```c
UINT fx_unicode_short_name_get(
    FX_MEDIA *media_ptr,
    UCHAR *source_unicode_name,
    ULONG source_unicode_length,
    CHAR *destination_short_name);
```

This service retrieves the short name (8.3 format) associated with the Unicode-name within the current default directory—no path information is allowed in the Unicode name parameter. If successful, the short name associated with the Unicode name is returned by the service.

> **Important:** *This service can be used to get short names for both files and subdirectories.*

### Input Parameters

- *media_ptr*: Pointer to media control block.
- *source_unicode_name*: Pointer to Unicode name.
- *source_unicode_length*: Length of Unicode name.
- *destination_short_name*: Pointer to destination for the short name (8.3 format). This must be at least 13 bytes in size.

### Return Values

- **FX_SUCCESS** (0x00) Successful short name retrieval.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT table.
- **FX_FILE_CORRUPT** (0x08) File is corrupted
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_NOT_FOUND** (0x04)    Unicode name was not found.
- **FX_NOT_IMPLEMENTED** (0x22) Service not implemented for exFAT file system.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_PTR_ERROR** (0x18) Invalid media or name pointers.
- **FX_CALLER_ERROR** (0x20)    Caller is not a thread.

### Allowed From

Threads

### Example

```c
FX_MEDIA         ram_disk;
UCHAR             my_short_name[13];
UCHAR             my_unicode_name[] = {0x38,0xC1, 0x88,0xBC, 0xF8,0xC9, 0x20,0x00,
                                     0x54,0xD6, 0x7C,0xC7, 0x20,0x00, 0x74,0xC7,
                                     0x84,0xB9, 0x20,0x00, 0x85,0xC7, 0xC8,0xB2,
                                     0xE4,0xB2, 0x2E,0x00, 0x64,0x00, 0x6F,0x00,
                                     0x63,0x00, 0x00,0x00};

/* Get the short name associated with the Unicode name contained in the array "my_unicode_name". */

length = fx_unicode_short_name_get(&ram_disk, my_unicode_name, 17, my_short_name);

/* If successful, the short name is returned in "my_short_name". */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get

## fx_unicode_short_name_get_extended

Gets the short name from Unicode name in a buffer of a specified length.

### Prototype

```c
UINT fx_unicode_short_name_get_extended(
    FX_MEDIA *media_ptr,
    UCHAR *source_unicode_name,
    ULONG source_unicode_length, 
    CHAR *destination_short_name,
    ULONG short_name_buffer_length);
```

### Description

This service retrieves the short name (8.3 format) associated with the Unicode-name within the current default directory—no path information is allowed in the Unicode name parameter. If successful, the short name associated with the Unicode name is returned by the service.

> **Important:** *This service is identical to ***fx_unicode_short_name_get***, except the caller supplies the size of the destination buffer as an input argument. This allows the service to guarantee the short name does not exceed the destination buffer.*

> **Note:** *This service can be used to get short names for both files and subdirectories*

### Input Parameters

- *media_ptr*: Pointer to media control block.
- *source_unicode_name*: Pointer to Unicode name.
- *source_unicode_length*: Length of Unicode name.
- *destination_short_name*: Pointer to destination for the short name (8.3 format). This must be at least 13 bytes in size.
- *short_name_buffer_length*: Size of the destination buffer. The buffer size must be at least 14 bytes.

### Return Values

- **FX_SUCCESS** (0x00) Successful short name retrieval.
- **FX_FAT_READ_ERROR** (0x03) Unable to read FAT table.
- **FX_FILE_CORRUPT** (0x08)    File is corrupted
- **FX_IO_ERROR** (0x90) Driver I/O error.
- **FX_MEDIA_NOT_OPEN** (0x11) Specified media is not open.
- **FX_NOT_FOUND** (0x04) Unicode name was not found.
- **FX_NOT_IMPLEMENTED** (0x22) Service not implemented for exFAT file system.
- **FX_SECTOR_INVALID** (0x89) Invalid sector.
- **FX_PTR_ERROR** (0x18) Invalid media or name pointers.
- **FX_CALLER_ERROR** (0x20)    Caller is not a thread.

### Allowed From

Threads

### Example

```c
#define         SHORT_NAME_BUFFER_SIZE 13
FX_MEDIA         ram_disk;
UCHAR             my_short_name[SHORT_NAME_BUFFER_SIZE];
UCHAR             my_unicode_name[] = {0x38,0xC1, 0x88,0xBC, 0xF8,0xC9, 0x20,0x00,
                                     0x54,0xD6, 0x7C,0xC7, 0x20,0x00, 0x74,0xC7,
                                     0x84,0xB9, 0x20,0x00, 0x85,0xC7, 0xC8,0xB2,
                                     0xE4,0xB2, 0x2E,0x00, 0x64,0x00, 0x6F,0x00,
                                     0x63,0x00, 0x00,0x00};

/* Get the short name associated with the Unicode name contained in the array "my_unicode_name". */

length = fx_unicode_short_name_get_extended(&ram_disk,
    my_unicode_name, 17, my_short_name,SHORT_NAME_BUFFER_SIZE);

/* If successful, the short name is returned in "my_short_name". */
```

### See Also

- fx_file_allocate
- fx_file_attributes_read
- fx_file_attributes_set
- fx_file_best_effort_allocate
- fx_file_close
- fx_file_create
- fx_file_date_time_set
- fx_file_delete
- fx_file_extended_allocate
- fx_file_extended_best_effort_allocate
- fx_file_extended_relative_seek
- fx_file_extended_seek
- fx_file_extended_truncate
- fx_file_extended_truncate_release
- fx_file_open
- fx_file_read
- fx_file_relative_seek
- fx_file_rename
- fx_file_seek
- fx_file_truncate
- fx_file_truncate_release
- fx_file_write
- fx_file_write_notify_set
- fx_unicode_file_create
- fx_unicode_file_rename
- fx_unicode_name_get
- fx_unicode_short_name_get

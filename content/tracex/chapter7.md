---
title: Chapter 7 - FileX trace events
description: This chapter contains a description of the FileX events. 
---


This chapter contains a description of the FileX events. 

## List of Events and Icons

**The following is a list of FileX events displayed by TraceX.**

The following describes each event:

| **Icon**                         | **Meaning**                               |
| -------------------------------- | ----------------------------------------- |
| ![Internal Logical Sector Cache Miss icon](../media/user-guide/fx-events/image1.png)    | Internal Logical Sector Cache Miss |
| ![Internal Directory Cache Miss icon](../media/user-guide/fx-events/image2.png)    | Internal Directory Cache Miss |
| ![Internal Media Flush icon](../media/user-guide/fx-events/image3.png)    | Internal Media Flush |
| ![Internal Directory Entry Read icon](../media/user-guide/fx-events/image4.png)    | Internal Directory Entry Read |
| ![Internal Directory Entry Write icon](../media/user-guide/fx-events/image5.png)    | Internal Directory Entry Write |
| ![Internal I / O Driver Read icon](../media/user-guide/fx-events/image6.png)    | Internal I/O Driver Read |
| ![Internal I / O Driver Write icon](../media/user-guide/fx-events/image7.png)    | Internal I/O Driver Write |
| ![Internal I / O Driver Flush icon](../media/user-guide/fx-events/image8.png)    | Internal I/O Driver Flush |
| ![Internal I / O Driver Abort icon](../media/user-guide/fx-events/image9.png)    | Internal I/O Driver Abort |
| ![Internal I / O Driver Initialize icon](../media/user-guide/fx-events/image10.png)    | Internal I/O Driver Initialize |
| ![Internal I / O Driver Boot Read icon](../media/user-guide/fx-events/image11.png)    | Internal I/O Driver Boot Read |
| ![Internal I / O Driver Release Sectors icon](../media/user-guide/fx-events/image12.png)    | Internal I/O Driver Release Sectors |
| ![Internal I / O Driver Boot Write icon](../media/user-guide/fx-events/image13.png)    | Internal I/O Driver Boot Write |
| ![Internal I / O Driver Driver Un-initialize icon](../media/user-guide/fx-events/image14.png)    | Internal I / O Driver Driver Un-initialize |
| ![Directory Attributes Read icon](../media/user-guide/fx-events/image15.png)    | **Directory Attributes Read** (*fx_directory_attributes_read*) |
| ![Directory Attributes Set icon](../media/user-guide/fx-events/image16.png)    | **Directory Attributes Set** (*fx_directory_attributes_set*) |
| ![Directory Create icon](../media/user-guide/fx-events/image17.png)    | **Directory Create** (*fx_directory_create*) |
| ![Directory Default Get icon](../media/user-guide/fx-events/image18.png)    | **Directory Default Get** (*fx_directory_default_get*) |
| ![Directory Default Set icon](../media/user-guide/fx-events/image19.png)    | **Directory Default Set** (*fx_directory_default_set*) |
| ![Directory Delete icon](../media/user-guide/fx-events/image20.png)    | **Directory Delete** (*fx_directory_delete*) |
| ![Directory First Entry Find icon](../media/user-guide/fx-events/image21.png)    | **Directory First Entry Find** (*fx_directory_first_entry_find*) |
| ![Directory First Full Entry Find icon](../media/user-guide/fx-events/image22.png)    | **Directory First Full Entry Find** (*fx_directory_first_full_entry_find*) |
| ![Directory Information Get icon](../media/user-guide/fx-events/image23.png)    | **Directory Information Get** (*fx_directory_information_get*) |
| ![Directory Local Path Clear icon](../media/user-guide/fx-events/image24.png)    | **Directory Local Path Clear** (*fx_directory_local_path_clear*) |
| ![Directory Local Path Get icon](../media/user-guide/fx-events/image25.png)    | **Directory Local Path Get** (*fx_directory_local_path_get*) |
| ![Directory Local Path Restore icon](../media/user-guide/fx-events/image26.png)    | **Directory Local Path Restore** (*fx_directory_local_path_restore*) |
| ![Directory Local Path Set icon](../media/user-guide/fx-events/image27.png)    | **Directory Local Path Set** (*fx_directory_local_path_set*) |
| ![Directory Long Name Get icon](../media/user-guide/fx-events/image28.png)    | **Directory Long Name Get** (*fx_directory_long_name_get*) |
| ![Directory Name Test icon](../media/user-guide/fx-events/image29.png)    | **Directory Name Test** (*fx_directory_name_test*) |
| ![Directory Next Entry Find icon](../media/user-guide/fx-events/image30.png)    | **Directory Next Entry Find** (*fx_directory_next_entry_find*) |
| ![Directory Next Full Entry Find icon](../media/user-guide/fx-events/image31.png)    | **Directory Next Full Entry Find** (*fx_directory_next_full_entry_find*) |
| ![Directory Rename icon](../media/user-guide/fx-events/image32.png)    | **Directory Rename** (*fx_directory_rename*) |
| ![Directory Short Name Get icon](../media/user-guide/fx-events/image33.png)    | **Directory Short Name Get** (*fx_directory_short_name_get*) |
| ![File Allocate icon](../media/user-guide/fx-events/image34.png)    | **File Allocate** (*fx_file_allocate*) |
| ![File Attributes Read icon](../media/user-guide/fx-events/image35.png)    | **File Attributes Read** (*fx_file_attributes_read*) |
| ![File Attributes Set icon](../media/user-guide/fx-events/image36.png)    | **File Attributes Set** (*fx_file_attributes_set*) |
| ![File Best Effort Allocate icon](../media/user-guide/fx-events/image37.png)    | **File Best Effort Allocate** (*fx_file_best_effort_allocate*) |
| ![File Close icon](../media/user-guide/fx-events/image38.png)    | **File Close** (*fx_file_close*) |
| ![File Create icon](../media/user-guide/fx-events/image39.png)    | **File Create** (*fx_file_create*) |
| ![File Date Time Set icon](../media/user-guide/fx-events/image40.png)    | **File Date Time Set** (*fx_file_date_time_set*) |
| ![File Delete icon](../media/user-guide/fx-events/image41.png)    | **File Delete** (*fx_file_delete*) |
| ![File Open icon](../media/user-guide/fx-events/image42.png)    | **File Open** (*fx_file_open*) |
| ![File Read icon](../media/user-guide/fx-events/image43.png)    | **File Read** (*fx_file_read*) |
| ![File Relative Seek icon](../media/user-guide/fx-events/image44.png)    | **File Relative Seek** (*fx_file_relative_seek*) |
| ![File Rename icon](../media/user-guide/fx-events/image45.png)    | **File Rename** (*fx_file_rename*) |
| ![File Seek icon](../media/user-guide/fx-events/image46.png)    | **File Seek** (*fx_file_seek*) |
| ![File Truncate icon](../media/user-guide/fx-events/image47.png)    | **File Truncate** (*fx_file_truncate*) |
| ![File Truncate Release icon](../media/user-guide/fx-events/image48.png)    | **File Truncate Release** (*fx_file_truncate_release*) |
| ![File Write icon](../media/user-guide/fx-events/image49.png)    | **File Write** (*fx_file_write*) |
| ![Media Abort icon](../media/user-guide/fx-events/image50.png)    | **Media Abort** (*fx_media_abort*) |
| ![Media Cache Invalidate icon](../media/user-guide/fx-events/image51.png)    | **Media Cache Invalidate** (*fx_media_cache_invalidate*) |
| ![Media Check icon](../media/user-guide/fx-events/image52.png)    | **Media Check** (*fx_media_check*) |
| ![*Media Close icon](../media/user-guide/fx-events/image53.png)    | **Media Close** (*fx_media_close*) |
| ![Media Flush icon](../media/user-guide/fx-events/image54.png)    | **Media Flush** (*fx_media_flush*) |
| ![Media Format icon](../media/user-guide/fx-events/image55.png)    | **Media Format** (*fx_media_format*) |
| ![Media Open icon](../media/user-guide/fx-events/image56.png)    | **Media Open** (*fx_media_open*) |
| ![Media Read icon](../media/user-guide/fx-events/image57.png)    | **Media Read** (*fx_media_read*) |
| ![Media Space Available icon](../media/user-guide/fx-events/image58.png)    | **Media Space Available** (*fx_media_space_available*) |
| ![Media Volume Get icon](../media/user-guide/fx-events/image59.png)    | **Media Volume Get** (*fx_media_volume_get*) |
| ![Media Volume Set icon](../media/user-guide/fx-events/image60.png)    | **Media Volume Set** (*fx_media_volume_set*) |
| ![Media Write icon](../media/user-guide/fx-events/image61.png)    | **Media Write** (*fx_media_write*) |
| ![System Date Get icon](../media/user-guide/fx-events/image62.png)    | **System Date Get** (*fx_system_date_get*) |
| ![System Date Set icon](../media/user-guide/fx-events/image63.png)    | **System Date Set** (*fx_system_date_set*) |
| ![System Initialize icon](../media/user-guide/fx-events/image64.png)    | **System Initialize** (*fx_system_initialize*) |
| ![System Time Get icon](../media/user-guide/fx-events/image65.png)    | **System Time Get** (*fx_system_time_get*) |
| ![System Time Set icon](../media/user-guide/fx-events/image66.png)    | **System Time Set** (*fx_system_time_set*) |
| ![Unicode Directory Create icon](../media/user-guide/fx-events/image67.png)    | **Unicode Directory Create** (*fx_unicode_directory_create*) |
| ![Unicode Directory Rename icon](../media/user-guide/fx-events/image68.png)    | **Unicode Directory Rename** (*fx_unicode_directory_rename*) |
| ![Unicode File Create icon](../media/user-guide/fx-events/image69.png)    | **Unicode File Create** (*fx_unicode_file_create*) |
| ![Unicode File Rename icon](../media/user-guide/fx-events/image70.png)    | **Unicode File Rename** (*fx_unicode_file_rename*) |
| ![Unicode Length Get icon](../media/user-guide/fx-events/image71.png)    | **Unicode Length Get** (*fx_unicode_length_get*) |
| ![Unicode Name Get icon](../media/user-guide/fx-events/image72.png)    | **Unicode Name Get** (*fx_unicode_name_get*) |
| ![Unicode Short Name Get icon](../media/user-guide/fx-events/image73.png)    | **Unicode Short Name Get** (*fx_unicode_short_name_get*) |

## Event Descriptions

The following describes each individual event.

### Internal Logical Sector Cache Miss 

#### Internal logical sector cache miss

**Icon** ![Internal logical sector cache miss icon](../media/user-guide/fx-events/image1.png)

**Description**

This event represents an internal FileX logical sector cache miss.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Sector.
- Info Field 3: Total misses.
- Info Field 4: Cache size.

### Internal Directory Cache Miss

#### Internal directory cache miss

**Icon** ![Internal directory cache miss icon](../media/user-guide/fx-events/image2.png)

**Description** 

This event represents an internal FileX directory cache miss.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Total misses.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal Media Flush 

#### Internal media flush

**Icon** ![Internal media flush icon](../media/user-guide/fx-events/image3.png)

**Description**

This event represents an internal FileX media flush.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Number of dirty sectors.
- Info Field 3: Not used.
- Info Field 4: Not used.


### Internal Directory Entry Read 

#### Internal directory entry read

**Icon** ![Internal directory entry read icon](../media/user-guide/fx-events/image4.png)

**Description**

This event represents an internal FileX directory entry read event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal Directory Entry Write

#### Internal directory entry write

**Icon** ![Internal directory entry write icon](../media/user-guide/fx-events/image5.png)

**Description**

This event represents an internal FileX directory entry write event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal I/O Driver Read 

#### Internal I/O driver read 

**Icon** ![Internal I / O driver read icon](../media/user-guide/fx-events/image6.png)

**Description** 

This event represents an internal FileX I/O driver read event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Sector.
- Info Field 3: Number of sectors.
- Info Field 4: Buffer pointer.

### Internal I/O Driver Write 

#### Internal I/O driver write 

**Icon** ![Internal I / O driver write icon](../media/user-guide/fx-events/image7.png)

**Description** 

This event represents an internal FileX I/O driver write event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Sector.
- Info Field 3: Number of sectors.
- Info Field 4: Buffer pointer.

### Internal I/O Driver Flush 

#### Internal I/O driver flush 

**Icon** ![Internal I / O driver flush icon](../media/user-guide/fx-events/image8.png)

**Description** 

This event represents an internal FileX I/O driver flush event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal I/O Driver Abort 

#### Internal I/O driver abort

**Icon** ![Internal I / O driver abort icon](../media/user-guide/fx-events/image9.png)

**Description**

This event represents an internal FileX I/O driver abort event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal I/O Driver Initialize 

#### Internal I/O driver initialize 

**Icon** ![Internal I / O driver initialize icon](../media/user-guide/fx-events/image10.png)

**Description** 

This event represents an internal FileX I/O driver initialize event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal I/O Driver Boot Sector Read 

#### Internal I/O driver boot sector read 

**Icon** ![Internal I / O driver boot sector read icon](../media/user-guide/fx-events/image11.png)

**Description** 

This event represents an internal FileX I/O driver boot sector read event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Buffer pointer.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal I/O Driver Release Sectors 

#### Internal I/O driver release sectors 

**Icon** ![Internal I / O driver release sectors icon](../media/user-guide/fx-events/image12.png)

**Description** 

This event represents an internal FileX I/O driver release sectors event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Sector.
- Info Field 3: Number of sectors.
- Info Field 4: Not used.

### Internal I/O Driver Boot Sector Write

#### Internal I/O driver boot sector write 

**Icon** ![Internal I / O driver boot sector write icon](../media/user-guide/fx-events/image13.png)

**Description** 

This event represents an internal FileX I/O driver boot sector write event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Buffer pointer.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal I/O Driver Un-initialize 

#### Internal I/O driver un-initialize 

**Icon** ![Internal I / O driver un-initialize icon](../media/user-guide/fx-events/image14.png)

**Description** 

This event represents an internal FileX I/O driver un-initialize event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.
</blockquote></td>

### Directory Attributes Read 

#### fx_directory_attributes_read 

**Icon** ![Directory attributes read icon](../media/user-guide/fx-events/image15.png)

**Description** 

This event represents a directory attributes read event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to directory name.
- Info Field 3: Attributes bit map:

  | Attribute                         | Value        |
  |---------------------------------- | --------|
  |  Read Only                        | (0x01)  |
  |  Hidden                           | (0x02)  |
  |  System                           | (0x04)  |
  |  Volume                           | (0x08)  |
  |  Directory                        | (0x10)  |
  |  Archive                          | (0x20)  |
- Info Field 4: Not used.

### Directory Attributes Set 

#### fx_directory_attributes_set 

**Icon** ![Attributes set icon](../media/user-guide/fx-events/image16.png)

**Description** 

This event represents a directory a directory attributes set event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to directory name.
- Info Field 3: Attributes bit map:

  |  Attribute                        | Value        |
  |---------------------------------- | --------|
  |  Read Only                        | (0x01)  |
  |  Hidden                           | (0x02)  |
  |  System                           | (0x04)  |
  |  Archive                          | (0x20)  |
- Info Field 4: Not used.

### Directory Create 

#### fx_directory_create

**Icon** ![Directory create icon](../media/user-guide/fx-events/image17.png)

**Description** 

This event represents a directory create event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to directory name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Default Get 

#### fx_directory_default_get 

**Icon** ![Directory default get icon](../media/user-guide/fx-events/image18.png)

**Description** 

This event represents a directory default set event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to return path name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Default Set 

#### fx_directory_default_set 

**Icon** ![Directory default set icon](../media/user-guide/fx-events/image19.png)

**Description** 

This event represents a directory default set event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to new default path name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Delete 

#### fx_directory_delete

**Icon** ![Directory delete icon](../media/user-guide/fx-events/image20.png)

**Description** 

This event represents a directory delete event.

**Information Fields**
- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to directory name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory First Entry Find 

#### fx_directory_first_entry_find

**Icon** ![Directory first entry find icon](../media/user-guide/fx-events/image21.png)

**Description** 

This event represents a directory first entry find event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to directory name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory First Full Entry Find 

#### fx_directory_first_full_entry_find

**Icon** ![Directory first full entry find icon](../media/user-guide/fx-events/image22.png)

**Description** 

This event represents a directory first full entry find event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to directory name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Information Get 

#### fx_directory_information_get

**Icon** ![Directory information get icon](../media/user-guide/fx-events/image23.png)

**Description** 

This event represents a directory information get event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to directory name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Local Path Clear 

#### fx_directory_local_path_clear

**Icon** ![Directory local path clear icon](../media/user-guide/fx-events/image24.png)

**Description** 

This event represents a directory local path clear event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Local Path Get 

#### fx_directory_local_path_get

**Icon** ![Directory local path get icon](../media/user-guide/fx-events/image25.png)

**Description** 

This event represents a directory local path get event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to return path name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Local Path Restore 

#### fx_directory_local_path_restore

**Icon** ![Directory local path restore icon](../media/user-guide/fx-events/image26.png)

**Description** 

This event represents a directory local path restore event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to local path structure.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Local Path Set 

#### fx_directory_local_path_set

**Icon** ![Directory local path set icon](../media/user-guide/fx-events/image27.png)

**Description** 

This event represents a directory local path set event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to local path structure.
- Info Field 3: Pointer to new path name.
- Info Field 4: Not used.

### Directory Long Name Get 

#### fx_directory_long_name_get

**Icon** ![Directory long name get icon](../media/user-guide/fx-events/image28.png)

**Description** 

This event represents a directory long name get event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to short file name.
- Info Field 3: Pointer to long file name.
- Info Field 4: Not used.

### Directory Name Test 

#### fx_directory_name_test

**Icon** ![Directory name test icon](../media/user-guide/fx-events/image29.png)

**Description** 

This event represents a directory name test event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to directory name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Next Entry Find 

#### fx_directory_next_entry_find

**Icon** ![Directory next entry find icon](../media/user-guide/fx-events/image30.png)

**Description** 

This event represents a directory next entry find event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to directory name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Next Full Entry Find 

#### fx_directory_next_full_entry_find

**Icon** ![Directory next full entry find icon](../media/user-guide/fx-events/image31.png)

**Description**

This event represents a directory next full entry find event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to directory name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Rename 

#### fx_directory_rename event

**Icon** ![Directory rename icon](../media/user-guide/fx-events/image32.png)

**Description**

This event represents a directory rename event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to old directory name.
- Info Field 3: Pointer to new directory name.
- Info Field 4: Not used.

### Directory Short Name Get 

#### fx_directory_short_name_get

**Icon** ![Directory short name get icon](../media/user-guide/fx-events/image33.png)

**Description**

This event represents a directory short name get event.

**Information Fields** 

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to long file name.
- Info Field 3: Pointer to short file name.
- Info Field 4: Not used.

### File Allocate 

#### fx_file_allocate

**Icon** ![File allocate icon](../media/user-guide/fx-events/image34.png)

**Description**

This event represents a file allocate event.

**Information Fields**

- Info Field 1: Pointer to the file.
- Info Field 2: Requested size.
- Info Field 3: Current size.
- Info Field 4: New size.

### File Attributes Read 

#### fx_file_attributes_read

**Icon** ![File attributes read icon](../media/user-guide/fx-events/image35.png)

**Description**

This event represents a file attributes read event.

**Information Fields**

- Info Field 1: Pointer to the media. 
- Info Field 2: Attributes bit map:

  |  Attribute                         | Value        |
  |---------------------------------- | --------|
  |  Read Only                        | (0x01)  |
  |  Hidden                           | (0x02)  |
  |  System                           | (0x04)  |
  |  Volume                           | (0x08)  |
  |  Directory                        | (0x10)  |
  |  Archive                          | (0x20)  |
- Info Field 3: Not used.
- Info Field 4: Not used.

### File Attributes Set 

#### fx_file_attributes_set

**Icon** ![File attributes set icon](../media/user-guide/fx-events/image36.png)

**Description** 

This event represents a file attributes set event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to file name. 
- Info Field 3: Attributes bit map:

  | Attribute                         | Value        |
  |---------------------------------- | --------|
  |  Read Only                        | (0x01)  |
  |  Hidden                           | (0x02)  |
  |  System                           | (0x04)  |
  |  Archive                          | (0x20)  |
- Info Field 4: Not used.

### File Best Effort Allocate 

#### fx_file_best_effort_allocate

**Icon** ![File best effort allocate icon](../media/user-guide/fx-events/image37.png)

**Description** 

This event represents a file best effort allocate event.

**Information Fields**

- Info Field 1: Pointer to the file.
- Info Field 2: Requested size.
- Info Field 3: Actual size allocated.
- Info Field 4: Not used.

### File Close 

#### fx_file_close

**Icon** ![File close icon](../media/user-guide/fx-events/image38.png)

**Description** 

This event represents a file close event.

**Information Fields**

- Info Field 1: Pointer to the file.
- Info Field 2: File size.
- Info Field 3: Not used.
- Info Field 4: Not used.

### File Create

#### fx_file_create

**Icon** ![File create icon](../media/user-guide/fx-events/image39.png)

**Description**

This event represents a file create event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to file name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### File Date Time Set 

#### fx_file_date_time_set

**Icon** ![File date time set icon](../media/user-guide/fx-events/image40.png)

**Description**

This event represents a file date/time set event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to file name.
- Info Field 3: Year.
- Info Field 4: Month.

### File Delete 

#### fx_file_delete

**Icon** ![File delete icon](../media/user-guide/fx-events/image41.png)

**Description**

This event represents a file delete event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to file name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### File Open 

#### fx_file_open

**Icon** ![File open icon](../media/user-guide/fx-events/image42.png)

**Description**

This event represents a file open event.

**Information Fields** 

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to the file control block.
- Info Field 3: Pointer to file name.
- Info Field 4: Open type:

  |  Open Type                        | Value        |
  |---------------------------------- | --------|
  |  Open for Read                    | (0x00)  |
  |  Open for Write                   | (0x01)  |
  |  Fast Open for Read               | (0x02)  |

### File Read 

#### fx_file_read

**Icon** ![File read icon](../media/user-guide/fx-events/image43.png)

**Description**

This event represents a file read event.

**Information Fields**

- Info Field 1: Pointer to the file.
- Info Field 2: Buffer pointer.
- Info Field 3: Request size.
- Info Field 4: Actual size read.

### File Relative Seek 

#### fx_file_relative_seek

**Icon** ![File relative seek icon](../media/user-guide/fx-events/image44.png)

**Description**

This event represents a file relative seek event.

**Information Fields**

- Info Field 1: Pointer to the file.
- Info Field 2: Byte offset.
- Info Field 3: Seek from:

  | Event                             | Value        |
  |---------------------------------- | --------|
  |  From Beginning                   | (0x00)  |
  |  From End                         | (0x01)  | 
  |  Forward                          | (0x02)  |
  |  Backward                         | (0x03)  |
- Info Field 4: Previous offset.

### File Rename 

#### fx_file_rename

**Icon** ![File rename icon](../media/user-guide/fx-events/image45.png)

**Description**

This event represents a file rename event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to old file name.
- Info Field 3: Pointer to new file name.
- Info Field 4: Not used.

### File Seek 

#### fx_file_seek

**Icon** ![File seek icon](../media/user-guide/fx-events/image46.png)

**Description**

This event represents a file seek event.

**Information Fields** 

- Info Field 1: Pointer to the file.
- Info Field 2: Byte offset.
- Info Field 3: Previous offset.
- Info Field 4: Not used.

### File Truncate 

#### fx_file_truncate

**Icon** ![File truncate icon](../media/user-guide/fx-events/image47.png)

**Description**

This event represents a file truncate event.

**Information Fields**

- Info Field 1: Pointer to the file.
- Info Field 2: Requested size.
- Info Field 3: Previous size.
- Info Field 4: New size.

### File Truncate Release 

#### fx_file_truncate_release 

**Icon** ![File truncate release icon](../media/user-guide/fx-events/image48.png)

**Description**

This event represents a file truncate release event.

**Information Fields**

- Info Field 1: Pointer to the file.
- Info Field 2: Requested size.
- Info Field 3: Previous size.
- Info Field 4: New size.

### File Write 

#### fx_file_write 

**Icon** ![File write icon](../media/user-guide/fx-events/image49.png)

**Description**

This event represents a file write event.

**Information Fields**

- Info Field 1: Pointer to the file.
- Info Field 2: Buffer pointer.
- Info Field 3: Request size.
- Info Field 4: Actual size written.

### Media Abort 

#### fx_media_abort 

**Icon** ![Media abort icon](../media/user-guide/fx-events/image50.png)

**Description**

This event represents a media abort event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Media Cache Invalidate

#### fx_media_cache_invalidate

**Icon** ![Media cache invalidate icon](../media/user-guide/fx-events/image51.png)

**Description**

This event represents a media cache invalidate event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Media Check 

#### fx_media_check

**Icon** ![Media check icon](../media/user-guide/fx-events/image52.png)

**Description**

This event represents a media check event.

**Information Fields** 

- Info Field 1: Pointer to the media.
- Info Field 2: Scratch memory pointer.
- Info Field 3: Scratch memory size.
- Info Field 4: Errors bit map:

  | Error type                        | Value        |
  |---------------------------------- | --------|
  |  FAT Chain Error                  | (0x01)  |
  |  Directory Error                  | (0x02)  |
  |  Lost Cluster Error               | (0x04)  |
  |  File Size Error                  | (0x08)  |

### Media Close 

#### fx_media_close

**Icon** ![Media Close icon](../media/user-guide/fx-events/image53.png)

**Description**

This event represents a media close event.

**Information Fields** 

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Media Flush 

#### fx_media_flush

**Icon** ![Media flush icon](../media/user-guide/fx-events/image54.png)

**Description**

This event represents a media flush event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Media Format 

#### fx_media_format

**Icon** ![Media format icon](../media/user-guide/fx-events/image55.png)

**Description**

This event represents a media format event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Number of root entries.
- Info Field 3: Sectors.
- Info Field 4: Sectors per cluster.

### Media Open 

#### fx_media_open

**Icon** ![Media open icon](../media/user-guide/fx-events/image56.png)

**Description**

This event represents a media open event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to media driver entry.
- Info Field 3: Memory pointer.
- Info Field 4: Memory size.

### Media Read Media Read 

#### fx_media_read

**Icon** ![Media read icon](../media/user-guide/fx-events/image57.png)

**Description**

This event represents a media read event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Logical sector.
- Info Field 3: Buffer pointer.
- Info Field 4: Bytes read.

### Media Space Available 

#### fx_media_space_available

**Icon** ![Media space available icon](../media/user-guide/fx-events/image58.png)

**Description**

This event represents a media space available event.

**Information Fields** 

- Info Field 1: Pointer to the media.
- Info Field 2: Available bytes pointer.
- Info Field 3: Number of free clusters.
- Info Field 4: Not used.

### Media Volume Get 

#### fx_media_volume_get

**Icon** ![Media volume get icon](../media/user-guide/fx-events/image59.png)

**Description**

This event represents a media volume get event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to volume name.
- Info Field 3: Volume source.
- Info Field 4: Not used.

### Media Volume Set 

#### fx_media_volume_set

**Icon** ![Media volume set icon](../media/user-guide/fx-events/image60.png)

**Description**

This event represents a media volume set event.

**Information Fields** 
- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to volume name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Media Write 

#### fx_media_write

**Icon** ![Media write icon](../media/user-guide/fx-events/image61.png)

**Description**

This event represents a media write event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Logical sector.
- Info Field 3: Buffer pointer.
- Info Field 4: Bytes written.

### System Date Get 

#### fx_system_date_get 

**Icon** ![System date get icon](../media/user-guide/fx-events/image62.png)

**Description**

This event represents a system date get event.

**Information Fields**

- Info Field 1: Year.
- Info Field 2: Month.
- Info Field 3: Day.
- Info Field 4: Not used.

### System Date Set 

#### fx_system_date_set 

**Icon** ![System date set icon](../media/user-guide/fx-events/image63.png)

**Description**

This event represents a system date set event.

**Information Fields**

- Info Field 1: Year.
- Info Field 2: Month.
- Info Field 3: Day.
- Info Field 4: Not used.

### System Initialize 

#### fx_system_initialize 

**Icon** ![System initialize icon](../media/user-guide/fx-events/image64.png)

**Description**

This event represents a system initialize event.

**Information Fields**

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### System Time Get 

#### fx_system_time_get 

**Icon** ![System time get icon](../media/user-guide/fx-events/image65.png)

**Description**

This event represents a system time get event.

**Information Fields** 

- Info Field 1: Hour.
- Info Field 2: Minute.
- Info Field 3: Second.
- Info Field 4: Not used.

### System Time Set 

#### fx_system_time_set 

**Icon** ![System time set icon](../media/user-guide/fx-events/image66.png)

**Description**

This event represents a system time set event.

**Information Fields**

- Info Field 1: Hour.
- Info Field 2: Minute.
- Info Field 3: Second.
- Info Field 4: Not used.

### Unicode Directory Create 

#### fx_unicode_directory_create 

**Icon** ![Unicode directory create icon](../media/user-guide/fx-events/image67.png)

**Description**

This event represents a Unicode directory create event.

**Information Fields** 

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to Unicode name.
- Info Field 3: Size of Unicode name.
- Info Field 4: Pointer to short name.

### Unicode Directory Rename 

#### fx_unicode_directory_rename

**Icon** ![Unicode directory rename icon](../media/user-guide/fx-events/image68.png)

**Description**

This event represents a Unicode directory rename event.

**Information Fields** 

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to Unicode name.
- Info Field 3: Size of Unicode name.
- Info Field 4: Pointer to short name.

### Unicode File Create 

#### fx_unicode_file_create

**Icon** ![Unicode file create icon](../media/user-guide/fx-events/image69.png)

**Description**

This event represents a Unicode file create event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to the Unicode name.
- Info Field 3: Size of Unicode name.
- Info Field 4: Pointer to short name.

### Unicode File Rename 

#### fx_unicode_file_rename

**Icon** ![Unicode file rename icon](../media/user-guide/fx-events/image70.png)

**Description**

This event represents a Unicode file rename event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to Unicode name.
- Info Field 3: Size of Unicode name.
- Info Field 4: Pointer to short name.

### Unicode Length Get 

#### fx_unicode_length_get

**Icon** ![Unicode length get icon](../media/user-guide/fx-events/image71.png)

**Description**

This event represents a Unicode length get event.

**Information Fields**

- Info Field 1: Pointer to the Unicode name.
- Info Field 2: Length.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Unicode Name Get 

#### fx_unicode_name_get

**Icon** ![Unicode name get icon](../media/user-guide/fx-events/image72.png)

**Description**

This event represents a Unicode name get event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Source short name.
- Info Field 3: Destination Unicode name pointer.
- Info Field 4: Destination Unicode name length.

### Unicode Short Name Get 

#### fx_unicode_short_name_get

**Icon** ![Unicode short name get icon](../media/user-guide/fx-events/image73.png)

**Description**

This event represents a Unicode short name get event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to source Unicode name.
- Info Field 3: Length of Unicode name.
- Info Field 4: Pointer to short name.
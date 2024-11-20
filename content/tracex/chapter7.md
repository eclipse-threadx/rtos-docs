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
| {{< figure src="../media/user-guide/fx-events/image1.png" title="Internal Logical Sector Cache Miss icon" imgClass="img-responsive center-block" >}}    | Internal Logical Sector Cache Miss |
| {{< figure src="../media/user-guide/fx-events/image2.png" title="Internal Directory Cache Miss icon" imgClass="img-responsive center-block" >}}    | Internal Directory Cache Miss |
| {{< figure src="../media/user-guide/fx-events/image3.png" title="Internal Media Flush icon" imgClass="img-responsive center-block" >}}    | Internal Media Flush |
| {{< figure src="../media/user-guide/fx-events/image4.png" title="Internal Directory Entry Read icon" imgClass="img-responsive center-block" >}}    | Internal Directory Entry Read |
| {{< figure src="../media/user-guide/fx-events/image5.png" title="Internal Directory Entry Write icon" imgClass="img-responsive center-block" >}}    | Internal Directory Entry Write |
| {{< figure src="../media/user-guide/fx-events/image6.png" title="Internal I / O Driver Read icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Read |
| {{< figure src="../media/user-guide/fx-events/image7.png" title="Internal I / O Driver Write icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Write |
| {{< figure src="../media/user-guide/fx-events/image8.png" title="Internal I / O Driver Flush icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Flush |
| {{< figure src="../media/user-guide/fx-events/image9.png" title="Internal I / O Driver Abort icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Abort |
| {{< figure src="../media/user-guide/fx-events/image10.png" title="Internal I / O Driver Initialize icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Initialize |
| {{< figure src="../media/user-guide/fx-events/image11.png" title="Internal I / O Driver Boot Read icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Boot Read |
| {{< figure src="../media/user-guide/fx-events/image12.png" title="Internal I / O Driver Release Sectors icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Release Sectors |
| {{< figure src="../media/user-guide/fx-events/image13.png" title="Internal I / O Driver Boot Write icon" imgClass="img-responsive center-block" >}}    | Internal I/O Driver Boot Write |
| {{< figure src="../media/user-guide/fx-events/image14.png" title="Internal I / O Driver Driver Un-initialize icon" imgClass="img-responsive center-block" >}}    | Internal I / O Driver Driver Un-initialize |
| {{< figure src="../media/user-guide/fx-events/image15.png" title="Directory Attributes Read icon" imgClass="img-responsive center-block" >}}    | **Directory Attributes Read** (*fx_directory_attributes_read*) |
| {{< figure src="../media/user-guide/fx-events/image16.png" title="Directory Attributes Set icon" imgClass="img-responsive center-block" >}}    | **Directory Attributes Set** (*fx_directory_attributes_set*) |
| {{< figure src="../media/user-guide/fx-events/image17.png" title="Directory Create icon" imgClass="img-responsive center-block" >}}    | **Directory Create** (*fx_directory_create*) |
| {{< figure src="../media/user-guide/fx-events/image18.png" title="Directory Default Get icon" imgClass="img-responsive center-block" >}}    | **Directory Default Get** (*fx_directory_default_get*) |
| {{< figure src="../media/user-guide/fx-events/image19.png" title="Directory Default Set icon" imgClass="img-responsive center-block" >}}    | **Directory Default Set** (*fx_directory_default_set*) |
| {{< figure src="../media/user-guide/fx-events/image20.png" title="Directory Delete icon" imgClass="img-responsive center-block" >}}    | **Directory Delete** (*fx_directory_delete*) |
| {{< figure src="../media/user-guide/fx-events/image21.png" title="Directory First Entry Find icon" imgClass="img-responsive center-block" >}}    | **Directory First Entry Find** (*fx_directory_first_entry_find*) |
| {{< figure src="../media/user-guide/fx-events/image22.png" title="Directory First Full Entry Find icon" imgClass="img-responsive center-block" >}}    | **Directory First Full Entry Find** (*fx_directory_first_full_entry_find*) |
| {{< figure src="../media/user-guide/fx-events/image23.png" title="Directory Information Get icon" imgClass="img-responsive center-block" >}}    | **Directory Information Get** (*fx_directory_information_get*) |
| {{< figure src="../media/user-guide/fx-events/image24.png" title="Directory Local Path Clear icon" imgClass="img-responsive center-block" >}}    | **Directory Local Path Clear** (*fx_directory_local_path_clear*) |
| {{< figure src="../media/user-guide/fx-events/image25.png" title="Directory Local Path Get icon" imgClass="img-responsive center-block" >}}    | **Directory Local Path Get** (*fx_directory_local_path_get*) |
| {{< figure src="../media/user-guide/fx-events/image26.png" title="Directory Local Path Restore icon" imgClass="img-responsive center-block" >}}    | **Directory Local Path Restore** (*fx_directory_local_path_restore*) |
| {{< figure src="../media/user-guide/fx-events/image27.png" title="Directory Local Path Set icon" imgClass="img-responsive center-block" >}}    | **Directory Local Path Set** (*fx_directory_local_path_set*) |
| {{< figure src="../media/user-guide/fx-events/image28.png" title="Directory Long Name Get icon" imgClass="img-responsive center-block" >}}    | **Directory Long Name Get** (*fx_directory_long_name_get*) |
| {{< figure src="../media/user-guide/fx-events/image29.png" title="Directory Name Test icon" imgClass="img-responsive center-block" >}}    | **Directory Name Test** (*fx_directory_name_test*) |
| {{< figure src="../media/user-guide/fx-events/image30.png" title="Directory Next Entry Find icon" imgClass="img-responsive center-block" >}}    | **Directory Next Entry Find** (*fx_directory_next_entry_find*) |
| {{< figure src="../media/user-guide/fx-events/image31.png" title="Directory Next Full Entry Find icon" imgClass="img-responsive center-block" >}}    | **Directory Next Full Entry Find** (*fx_directory_next_full_entry_find*) |
| {{< figure src="../media/user-guide/fx-events/image32.png" title="Directory Rename icon" imgClass="img-responsive center-block" >}}    | **Directory Rename** (*fx_directory_rename*) |
| {{< figure src="../media/user-guide/fx-events/image33.png" title="Directory Short Name Get icon" imgClass="img-responsive center-block" >}}    | **Directory Short Name Get** (*fx_directory_short_name_get*) |
| {{< figure src="../media/user-guide/fx-events/image34.png" title="File Allocate icon" imgClass="img-responsive center-block" >}}    | **File Allocate** (*fx_file_allocate*) |
| {{< figure src="../media/user-guide/fx-events/image35.png" title="File Attributes Read icon" imgClass="img-responsive center-block" >}}    | **File Attributes Read** (*fx_file_attributes_read*) |
| {{< figure src="../media/user-guide/fx-events/image36.png" title="File Attributes Set icon" imgClass="img-responsive center-block" >}}    | **File Attributes Set** (*fx_file_attributes_set*) |
| {{< figure src="../media/user-guide/fx-events/image37.png" title="File Best Effort Allocate icon" imgClass="img-responsive center-block" >}}    | **File Best Effort Allocate** (*fx_file_best_effort_allocate*) |
| {{< figure src="../media/user-guide/fx-events/image38.png" title="File Close icon" imgClass="img-responsive center-block" >}}    | **File Close** (*fx_file_close*) |
| {{< figure src="../media/user-guide/fx-events/image39.png" title="File Create icon" imgClass="img-responsive center-block" >}}    | **File Create** (*fx_file_create*) |
| {{< figure src="../media/user-guide/fx-events/image40.png" title="File Date Time Set icon" imgClass="img-responsive center-block" >}}    | **File Date Time Set** (*fx_file_date_time_set*) |
| {{< figure src="../media/user-guide/fx-events/image41.png" title="File Delete icon" imgClass="img-responsive center-block" >}}    | **File Delete** (*fx_file_delete*) |
| {{< figure src="../media/user-guide/fx-events/image42.png" title="File Open icon" imgClass="img-responsive center-block" >}}    | **File Open** (*fx_file_open*) |
| {{< figure src="../media/user-guide/fx-events/image43.png" title="File Read icon" imgClass="img-responsive center-block" >}}    | **File Read** (*fx_file_read*) |
| {{< figure src="../media/user-guide/fx-events/image44.png" title="File Relative Seek icon" imgClass="img-responsive center-block" >}}    | **File Relative Seek** (*fx_file_relative_seek*) |
| {{< figure src="../media/user-guide/fx-events/image45.png" title="File Rename icon" imgClass="img-responsive center-block" >}}    | **File Rename** (*fx_file_rename*) |
| {{< figure src="../media/user-guide/fx-events/image46.png" title="File Seek icon" imgClass="img-responsive center-block" >}}    | **File Seek** (*fx_file_seek*) |
| {{< figure src="../media/user-guide/fx-events/image47.png" title="File Truncate icon" imgClass="img-responsive center-block" >}}    | **File Truncate** (*fx_file_truncate*) |
| {{< figure src="../media/user-guide/fx-events/image48.png" title="File Truncate Release icon" imgClass="img-responsive center-block" >}}    | **File Truncate Release** (*fx_file_truncate_release*) |
| {{< figure src="../media/user-guide/fx-events/image49.png" title="File Write icon" imgClass="img-responsive center-block" >}}    | **File Write** (*fx_file_write*) |
| {{< figure src="../media/user-guide/fx-events/image50.png" title="Media Abort icon" imgClass="img-responsive center-block" >}}    | **Media Abort** (*fx_media_abort*) |
| {{< figure src="../media/user-guide/fx-events/image51.png" title="Media Cache Invalidate icon" imgClass="img-responsive center-block" >}}    | **Media Cache Invalidate** (*fx_media_cache_invalidate*) |
| {{< figure src="../media/user-guide/fx-events/image52.png" title="Media Check icon" imgClass="img-responsive center-block" >}}    | **Media Check** (*fx_media_check*) |
| {{< figure src="../media/user-guide/fx-events/image53.png" title="*Media Close icon" imgClass="img-responsive center-block" >}}    | **Media Close** (*fx_media_close*) |
| {{< figure src="../media/user-guide/fx-events/image54.png" title="Media Flush icon" imgClass="img-responsive center-block" >}}    | **Media Flush** (*fx_media_flush*) |
| {{< figure src="../media/user-guide/fx-events/image55.png" title="Media Format icon" imgClass="img-responsive center-block" >}}    | **Media Format** (*fx_media_format*) |
| {{< figure src="../media/user-guide/fx-events/image56.png" title="Media Open icon" imgClass="img-responsive center-block" >}}    | **Media Open** (*fx_media_open*) |
| {{< figure src="../media/user-guide/fx-events/image57.png" title="Media Read icon" imgClass="img-responsive center-block" >}}    | **Media Read** (*fx_media_read*) |
| {{< figure src="../media/user-guide/fx-events/image58.png" title="Media Space Available icon" imgClass="img-responsive center-block" >}}    | **Media Space Available** (*fx_media_space_available*) |
| {{< figure src="../media/user-guide/fx-events/image59.png" title="Media Volume Get icon" imgClass="img-responsive center-block" >}}    | **Media Volume Get** (*fx_media_volume_get*) |
| {{< figure src="../media/user-guide/fx-events/image60.png" title="Media Volume Set icon" imgClass="img-responsive center-block" >}}    | **Media Volume Set** (*fx_media_volume_set*) |
| {{< figure src="../media/user-guide/fx-events/image61.png" title="Media Write icon" imgClass="img-responsive center-block" >}}    | **Media Write** (*fx_media_write*) |
| {{< figure src="../media/user-guide/fx-events/image62.png" title="System Date Get icon" imgClass="img-responsive center-block" >}}    | **System Date Get** (*fx_system_date_get*) |
| {{< figure src="../media/user-guide/fx-events/image63.png" title="System Date Set icon" imgClass="img-responsive center-block" >}}    | **System Date Set** (*fx_system_date_set*) |
| {{< figure src="../media/user-guide/fx-events/image64.png" title="System Initialize icon" imgClass="img-responsive center-block" >}}    | **System Initialize** (*fx_system_initialize*) |
| {{< figure src="../media/user-guide/fx-events/image65.png" title="System Time Get icon" imgClass="img-responsive center-block" >}}    | **System Time Get** (*fx_system_time_get*) |
| {{< figure src="../media/user-guide/fx-events/image66.png" title="System Time Set icon" imgClass="img-responsive center-block" >}}    | **System Time Set** (*fx_system_time_set*) |
| {{< figure src="../media/user-guide/fx-events/image67.png" title="Unicode Directory Create icon" imgClass="img-responsive center-block" >}}    | **Unicode Directory Create** (*fx_unicode_directory_create*) |
| {{< figure src="../media/user-guide/fx-events/image68.png" title="Unicode Directory Rename icon" imgClass="img-responsive center-block" >}}    | **Unicode Directory Rename** (*fx_unicode_directory_rename*) |
| {{< figure src="../media/user-guide/fx-events/image69.png" title="Unicode File Create icon" imgClass="img-responsive center-block" >}}    | **Unicode File Create** (*fx_unicode_file_create*) |
| {{< figure src="../media/user-guide/fx-events/image70.png" title="Unicode File Rename icon" imgClass="img-responsive center-block" >}}    | **Unicode File Rename** (*fx_unicode_file_rename*) |
| {{< figure src="../media/user-guide/fx-events/image71.png" title="Unicode Length Get icon" imgClass="img-responsive center-block" >}}    | **Unicode Length Get** (*fx_unicode_length_get*) |
| {{< figure src="../media/user-guide/fx-events/image72.png" title="Unicode Name Get icon" imgClass="img-responsive center-block" >}}    | **Unicode Name Get** (*fx_unicode_name_get*) |
| {{< figure src="../media/user-guide/fx-events/image73.png" title="Unicode Short Name Get icon" imgClass="img-responsive center-block" >}}    | **Unicode Short Name Get** (*fx_unicode_short_name_get*) |

## Event Descriptions

The following describes each individual event.

### Internal Logical Sector Cache Miss 

#### Internal logical sector cache miss

**Icon** {{< figure src="../media/user-guide/fx-events/image1.png" title="Internal logical sector cache miss icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal FileX logical sector cache miss.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Sector.
- Info Field 3: Total misses.
- Info Field 4: Cache size.

### Internal Directory Cache Miss

#### Internal directory cache miss

**Icon** {{< figure src="../media/user-guide/fx-events/image2.png" title="Internal directory cache miss icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents an internal FileX directory cache miss.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Total misses.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal Media Flush 

#### Internal media flush

**Icon** {{< figure src="../media/user-guide/fx-events/image3.png" title="Internal media flush icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal FileX media flush.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Number of dirty sectors.
- Info Field 3: Not used.
- Info Field 4: Not used.


### Internal Directory Entry Read 

#### Internal directory entry read

**Icon** {{< figure src="../media/user-guide/fx-events/image4.png" title="Internal directory entry read icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal FileX directory entry read event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal Directory Entry Write

#### Internal directory entry write

**Icon** {{< figure src="../media/user-guide/fx-events/image5.png" title="Internal directory entry write icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal FileX directory entry write event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal I/O Driver Read 

#### Internal I/O driver read 

**Icon** {{< figure src="../media/user-guide/fx-events/image6.png" title="Internal I / O driver read icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents an internal FileX I/O driver read event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Sector.
- Info Field 3: Number of sectors.
- Info Field 4: Buffer pointer.

### Internal I/O Driver Write 

#### Internal I/O driver write 

**Icon** {{< figure src="../media/user-guide/fx-events/image7.png" title="Internal I / O driver write icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents an internal FileX I/O driver write event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Sector.
- Info Field 3: Number of sectors.
- Info Field 4: Buffer pointer.

### Internal I/O Driver Flush 

#### Internal I/O driver flush 

**Icon** {{< figure src="../media/user-guide/fx-events/image8.png" title="Internal I / O driver flush icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents an internal FileX I/O driver flush event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal I/O Driver Abort 

#### Internal I/O driver abort

**Icon** {{< figure src="../media/user-guide/fx-events/image9.png" title="Internal I / O driver abort icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal FileX I/O driver abort event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal I/O Driver Initialize 

#### Internal I/O driver initialize 

**Icon** {{< figure src="../media/user-guide/fx-events/image10.png" title="Internal I / O driver initialize icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents an internal FileX I/O driver initialize event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal I/O Driver Boot Sector Read 

#### Internal I/O driver boot sector read 

**Icon** {{< figure src="../media/user-guide/fx-events/image11.png" title="Internal I / O driver boot sector read icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents an internal FileX I/O driver boot sector read event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Buffer pointer.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal I/O Driver Release Sectors 

#### Internal I/O driver release sectors 

**Icon** {{< figure src="../media/user-guide/fx-events/image12.png" title="Internal I / O driver release sectors icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents an internal FileX I/O driver release sectors event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Sector.
- Info Field 3: Number of sectors.
- Info Field 4: Not used.

### Internal I/O Driver Boot Sector Write

#### Internal I/O driver boot sector write 

**Icon** {{< figure src="../media/user-guide/fx-events/image13.png" title="Internal I / O driver boot sector write icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents an internal FileX I/O driver boot sector write event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Buffer pointer.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Internal I/O Driver Un-initialize 

#### Internal I/O driver un-initialize 

**Icon** {{< figure src="../media/user-guide/fx-events/image14.png" title="Internal I / O driver un-initialize icon" imgClass="img-responsive center-block" >}}

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

**Icon** {{< figure src="../media/user-guide/fx-events/image15.png" title="Directory attributes read icon" imgClass="img-responsive center-block" >}}

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

**Icon** {{< figure src="../media/user-guide/fx-events/image16.png" title="Attributes set icon" imgClass="img-responsive center-block" >}}

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

**Icon** {{< figure src="../media/user-guide/fx-events/image17.png" title="Directory create icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents a directory create event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to directory name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Default Get 

#### fx_directory_default_get 

**Icon** {{< figure src="../media/user-guide/fx-events/image18.png" title="Directory default get icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents a directory default set event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to return path name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Default Set 

#### fx_directory_default_set 

**Icon** {{< figure src="../media/user-guide/fx-events/image19.png" title="Directory default set icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents a directory default set event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to new default path name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Delete 

#### fx_directory_delete

**Icon** {{< figure src="../media/user-guide/fx-events/image20.png" title="Directory delete icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents a directory delete event.

**Information Fields**
- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to directory name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory First Entry Find 

#### fx_directory_first_entry_find

**Icon** {{< figure src="../media/user-guide/fx-events/image21.png" title="Directory first entry find icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents a directory first entry find event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to directory name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory First Full Entry Find 

#### fx_directory_first_full_entry_find

**Icon** {{< figure src="../media/user-guide/fx-events/image22.png" title="Directory first full entry find icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents a directory first full entry find event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to directory name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Information Get 

#### fx_directory_information_get

**Icon** {{< figure src="../media/user-guide/fx-events/image23.png" title="Directory information get icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents a directory information get event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to directory name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Local Path Clear 

#### fx_directory_local_path_clear

**Icon** {{< figure src="../media/user-guide/fx-events/image24.png" title="Directory local path clear icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents a directory local path clear event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Local Path Get 

#### fx_directory_local_path_get

**Icon** {{< figure src="../media/user-guide/fx-events/image25.png" title="Directory local path get icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents a directory local path get event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to return path name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Local Path Restore 

#### fx_directory_local_path_restore

**Icon** {{< figure src="../media/user-guide/fx-events/image26.png" title="Directory local path restore icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents a directory local path restore event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to local path structure.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Local Path Set 

#### fx_directory_local_path_set

**Icon** {{< figure src="../media/user-guide/fx-events/image27.png" title="Directory local path set icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents a directory local path set event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to local path structure.
- Info Field 3: Pointer to new path name.
- Info Field 4: Not used.

### Directory Long Name Get 

#### fx_directory_long_name_get

**Icon** {{< figure src="../media/user-guide/fx-events/image28.png" title="Directory long name get icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents a directory long name get event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to short file name.
- Info Field 3: Pointer to long file name.
- Info Field 4: Not used.

### Directory Name Test 

#### fx_directory_name_test

**Icon** {{< figure src="../media/user-guide/fx-events/image29.png" title="Directory name test icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents a directory name test event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to directory name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Next Entry Find 

#### fx_directory_next_entry_find

**Icon** {{< figure src="../media/user-guide/fx-events/image30.png" title="Directory next entry find icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents a directory next entry find event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to directory name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Next Full Entry Find 

#### fx_directory_next_full_entry_find

**Icon** {{< figure src="../media/user-guide/fx-events/image31.png" title="Directory next full entry find icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a directory next full entry find event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to directory name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Directory Rename 

#### fx_directory_rename event

**Icon** {{< figure src="../media/user-guide/fx-events/image32.png" title="Directory rename icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a directory rename event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to old directory name.
- Info Field 3: Pointer to new directory name.
- Info Field 4: Not used.

### Directory Short Name Get 

#### fx_directory_short_name_get

**Icon** {{< figure src="../media/user-guide/fx-events/image33.png" title="Directory short name get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a directory short name get event.

**Information Fields** 

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to long file name.
- Info Field 3: Pointer to short file name.
- Info Field 4: Not used.

### File Allocate 

#### fx_file_allocate

**Icon** {{< figure src="../media/user-guide/fx-events/image34.png" title="File allocate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a file allocate event.

**Information Fields**

- Info Field 1: Pointer to the file.
- Info Field 2: Requested size.
- Info Field 3: Current size.
- Info Field 4: New size.

### File Attributes Read 

#### fx_file_attributes_read

**Icon** {{< figure src="../media/user-guide/fx-events/image35.png" title="File attributes read icon" imgClass="img-responsive center-block" >}}

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

**Icon** {{< figure src="../media/user-guide/fx-events/image36.png" title="File attributes set icon" imgClass="img-responsive center-block" >}}

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

**Icon** {{< figure src="../media/user-guide/fx-events/image37.png" title="File best effort allocate icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents a file best effort allocate event.

**Information Fields**

- Info Field 1: Pointer to the file.
- Info Field 2: Requested size.
- Info Field 3: Actual size allocated.
- Info Field 4: Not used.

### File Close 

#### fx_file_close

**Icon** {{< figure src="../media/user-guide/fx-events/image38.png" title="File close icon" imgClass="img-responsive center-block" >}}

**Description** 

This event represents a file close event.

**Information Fields**

- Info Field 1: Pointer to the file.
- Info Field 2: File size.
- Info Field 3: Not used.
- Info Field 4: Not used.

### File Create

#### fx_file_create

**Icon** {{< figure src="../media/user-guide/fx-events/image39.png" title="File create icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a file create event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to file name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### File Date Time Set 

#### fx_file_date_time_set

**Icon** {{< figure src="../media/user-guide/fx-events/image40.png" title="File date time set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a file date/time set event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to file name.
- Info Field 3: Year.
- Info Field 4: Month.

### File Delete 

#### fx_file_delete

**Icon** {{< figure src="../media/user-guide/fx-events/image41.png" title="File delete icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a file delete event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to file name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### File Open 

#### fx_file_open

**Icon** {{< figure src="../media/user-guide/fx-events/image42.png" title="File open icon" imgClass="img-responsive center-block" >}}

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

**Icon** {{< figure src="../media/user-guide/fx-events/image43.png" title="File read icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a file read event.

**Information Fields**

- Info Field 1: Pointer to the file.
- Info Field 2: Buffer pointer.
- Info Field 3: Request size.
- Info Field 4: Actual size read.

### File Relative Seek 

#### fx_file_relative_seek

**Icon** {{< figure src="../media/user-guide/fx-events/image44.png" title="File relative seek icon" imgClass="img-responsive center-block" >}}

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

**Icon** {{< figure src="../media/user-guide/fx-events/image45.png" title="File rename icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a file rename event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to old file name.
- Info Field 3: Pointer to new file name.
- Info Field 4: Not used.

### File Seek 

#### fx_file_seek

**Icon** {{< figure src="../media/user-guide/fx-events/image46.png" title="File seek icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a file seek event.

**Information Fields** 

- Info Field 1: Pointer to the file.
- Info Field 2: Byte offset.
- Info Field 3: Previous offset.
- Info Field 4: Not used.

### File Truncate 

#### fx_file_truncate

**Icon** {{< figure src="../media/user-guide/fx-events/image47.png" title="File truncate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a file truncate event.

**Information Fields**

- Info Field 1: Pointer to the file.
- Info Field 2: Requested size.
- Info Field 3: Previous size.
- Info Field 4: New size.

### File Truncate Release 

#### fx_file_truncate_release 

**Icon** {{< figure src="../media/user-guide/fx-events/image48.png" title="File truncate release icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a file truncate release event.

**Information Fields**

- Info Field 1: Pointer to the file.
- Info Field 2: Requested size.
- Info Field 3: Previous size.
- Info Field 4: New size.

### File Write 

#### fx_file_write 

**Icon** {{< figure src="../media/user-guide/fx-events/image49.png" title="File write icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a file write event.

**Information Fields**

- Info Field 1: Pointer to the file.
- Info Field 2: Buffer pointer.
- Info Field 3: Request size.
- Info Field 4: Actual size written.

### Media Abort 

#### fx_media_abort 

**Icon** {{< figure src="../media/user-guide/fx-events/image50.png" title="Media abort icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a media abort event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Media Cache Invalidate

#### fx_media_cache_invalidate

**Icon** {{< figure src="../media/user-guide/fx-events/image51.png" title="Media cache invalidate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a media cache invalidate event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Media Check 

#### fx_media_check

**Icon** {{< figure src="../media/user-guide/fx-events/image52.png" title="Media check icon" imgClass="img-responsive center-block" >}}

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

**Icon** {{< figure src="../media/user-guide/fx-events/image53.png" title="Media Close icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a media close event.

**Information Fields** 

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Media Flush 

#### fx_media_flush

**Icon** {{< figure src="../media/user-guide/fx-events/image54.png" title="Media flush icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a media flush event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Media Format 

#### fx_media_format

**Icon** {{< figure src="../media/user-guide/fx-events/image55.png" title="Media format icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a media format event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Number of root entries.
- Info Field 3: Sectors.
- Info Field 4: Sectors per cluster.

### Media Open 

#### fx_media_open

**Icon** {{< figure src="../media/user-guide/fx-events/image56.png" title="Media open icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a media open event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to media driver entry.
- Info Field 3: Memory pointer.
- Info Field 4: Memory size.

### Media Read Media Read 

#### fx_media_read

**Icon** {{< figure src="../media/user-guide/fx-events/image57.png" title="Media read icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a media read event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Logical sector.
- Info Field 3: Buffer pointer.
- Info Field 4: Bytes read.

### Media Space Available 

#### fx_media_space_available

**Icon** {{< figure src="../media/user-guide/fx-events/image58.png" title="Media space available icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a media space available event.

**Information Fields** 

- Info Field 1: Pointer to the media.
- Info Field 2: Available bytes pointer.
- Info Field 3: Number of free clusters.
- Info Field 4: Not used.

### Media Volume Get 

#### fx_media_volume_get

**Icon** {{< figure src="../media/user-guide/fx-events/image59.png" title="Media volume get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a media volume get event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to volume name.
- Info Field 3: Volume source.
- Info Field 4: Not used.

### Media Volume Set 

#### fx_media_volume_set

**Icon** {{< figure src="../media/user-guide/fx-events/image60.png" title="Media volume set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a media volume set event.

**Information Fields** 
- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to volume name.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Media Write 

#### fx_media_write

**Icon** {{< figure src="../media/user-guide/fx-events/image61.png" title="Media write icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a media write event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Logical sector.
- Info Field 3: Buffer pointer.
- Info Field 4: Bytes written.

### System Date Get 

#### fx_system_date_get 

**Icon** {{< figure src="../media/user-guide/fx-events/image62.png" title="System date get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a system date get event.

**Information Fields**

- Info Field 1: Year.
- Info Field 2: Month.
- Info Field 3: Day.
- Info Field 4: Not used.

### System Date Set 

#### fx_system_date_set 

**Icon** {{< figure src="../media/user-guide/fx-events/image63.png" title="System date set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a system date set event.

**Information Fields**

- Info Field 1: Year.
- Info Field 2: Month.
- Info Field 3: Day.
- Info Field 4: Not used.

### System Initialize 

#### fx_system_initialize 

**Icon** {{< figure src="../media/user-guide/fx-events/image64.png" title="System initialize icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a system initialize event.

**Information Fields**

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### System Time Get 

#### fx_system_time_get 

**Icon** {{< figure src="../media/user-guide/fx-events/image65.png" title="System time get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a system time get event.

**Information Fields** 

- Info Field 1: Hour.
- Info Field 2: Minute.
- Info Field 3: Second.
- Info Field 4: Not used.

### System Time Set 

#### fx_system_time_set 

**Icon** {{< figure src="../media/user-guide/fx-events/image66.png" title="System time set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a system time set event.

**Information Fields**

- Info Field 1: Hour.
- Info Field 2: Minute.
- Info Field 3: Second.
- Info Field 4: Not used.

### Unicode Directory Create 

#### fx_unicode_directory_create 

**Icon** {{< figure src="../media/user-guide/fx-events/image67.png" title="Unicode directory create icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a Unicode directory create event.

**Information Fields** 

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to Unicode name.
- Info Field 3: Size of Unicode name.
- Info Field 4: Pointer to short name.

### Unicode Directory Rename 

#### fx_unicode_directory_rename

**Icon** {{< figure src="../media/user-guide/fx-events/image68.png" title="Unicode directory rename icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a Unicode directory rename event.

**Information Fields** 

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to Unicode name.
- Info Field 3: Size of Unicode name.
- Info Field 4: Pointer to short name.

### Unicode File Create 

#### fx_unicode_file_create

**Icon** {{< figure src="../media/user-guide/fx-events/image69.png" title="Unicode file create icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a Unicode file create event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to the Unicode name.
- Info Field 3: Size of Unicode name.
- Info Field 4: Pointer to short name.

### Unicode File Rename 

#### fx_unicode_file_rename

**Icon** {{< figure src="../media/user-guide/fx-events/image70.png" title="Unicode file rename icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a Unicode file rename event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to Unicode name.
- Info Field 3: Size of Unicode name.
- Info Field 4: Pointer to short name.

### Unicode Length Get 

#### fx_unicode_length_get

**Icon** {{< figure src="../media/user-guide/fx-events/image71.png" title="Unicode length get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a Unicode length get event.

**Information Fields**

- Info Field 1: Pointer to the Unicode name.
- Info Field 2: Length.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Unicode Name Get 

#### fx_unicode_name_get

**Icon** {{< figure src="../media/user-guide/fx-events/image72.png" title="Unicode name get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a Unicode name get event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Source short name.
- Info Field 3: Destination Unicode name pointer.
- Info Field 4: Destination Unicode name length.

### Unicode Short Name Get 

#### fx_unicode_short_name_get

**Icon** {{< figure src="../media/user-guide/fx-events/image73.png" title="Unicode short name get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a Unicode short name get event.

**Information Fields**

- Info Field 1: Pointer to the media.
- Info Field 2: Pointer to source Unicode name.
- Info Field 3: Length of Unicode name.
- Info Field 4: Pointer to short name.
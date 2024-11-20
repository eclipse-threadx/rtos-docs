---
title: Appendix B - FileX constants
description: Learn about the FileX constants.
---


## Alphabetic Listings 

| Constant (by alphabetic)             | Value          |
|--------------------------------------|----------------|
|FX_12_BIT_FAT_SIZE                    |4086|
|FX_12BIT_SIZE                         |3|
|FX_16_BIT_FAT_SIZE                    |65525|
|FX_ACCESS_ERROR                       |0x06|
|FX_ALREADY_CREATED                    |0x0B|
|FX_ARCHIVE                            |0x20|
|FX_BAD_CLUSTER                        |0xFFF7|
|FX_BAD_CLUSTER_32                     |0x0FFFFFF7|
|FX_BASE_YEAR                          |1980|
|FX_BIGDOS                             |0x06|
|FX_BOOT_ERROR                         |0x01|
|FX_BOOT_SECTOR                        |1|
|FX_BOOT_SECTOR_SIZE                   |512|
|FX_BOOT_SIG                           |0x026|
|FX_BUFFER_ERROR                       |0x21|
|FX_BYTES_SECTOR                       |0x00B|
|FX_CALLER_ERROR                       |0x20|
|FX_DATA_SECTOR                        |4|
|FX_DAY_MASK                           |0x1F|
|FX_DIR_ENTRY_DONE                     |0x00|
|FX_DIR_ENTRY_FREE                     |0xE5|
|FX_DIR_ENTRY_SIZE                     |32|
|FX_DIR_EXT_SIZE                       |3|
|FX_DIR_NAME_SIZE                      |8|
|FX_DIR_NOT_EMPTY                      |0x10|
|FX_DIR_RESERVED                       |8|
|FX_DIRECTORY                          |0x10|
|FX_DIRECTORY_ERROR                    |0x02|
|FX_DIRECTORY_SECTOR                   |3|
|FX_DRIVE_NUMBER                       |0x024|
|FX_DRIVER_ABORT                       |3|
|FX_DRIVER_BOOT_READ                   |5|
|FX_DRIVER_BOOT_WRITE                  |7|
|FX_DRIVER_FLUSH                       |2|
|FX_DRIVER_INIT                        |4|
|FX_DRIVER_READ                        |0|
|FX_DRIVER_RELEASE_SECTORS             |6|
|FX_DRIVER_UNINIT                      |8|
|FX_DRIVER_WRITE                       |1|
|FX_EF_BOOT_CODE                       |120|
|FX_EF_BYTE_PER_SECTOR_SHIFT           |108|
|FX_EF_CLUSTER_COUNT                   |92|
|FX_EF_CLUSTER_HEAP_OFFSET             |88|
|FX_EF_DRIVE_SELECT                    |111|
|FX_EF_FAT_LENGTH                      |84|
|FX_EF_FAT_OFFSET                      |80|
|FX_EF_FILE_SYSTEM_REVISION            |104|
|FX_EF_FIRST_CLUSTER_OF_ROOT_DIR       |96|
|FX_EF_MUST_BE_ZERO                    |11|
|FX_EF_NUMBER_OF_FATS                  |110|
|FX_EF_PARTITION_OFFSET                |64|
|FX_EF_PERCENT_IN_USE                  |112|
|FX_EF_RESERVED                        |113|
|FX_EF_SECTOR_PER_CLUSTER_SHIFT        |109|
|FX_EF_VOLUME_FLAGS                    |106|
|FX_EF_VOLUME_LENGTH                   |72|
|FX_EF_VOLUME_SERIAL_NUMBER            |100|
|FX_END_OF_FILE                        |0x09|
|FX_ERROR_FIXED                        |0x92|
|FX_ERROR_NOT_FIXED                    |0x93|
|FX_FALSE                              |0|
|FX_FAT_CACHE_DEPTH                    |4|
|FX_FAT_CACHE_HASH_MASK                |0x3|
|FX_FAT_CHAIN_ERROR                    |0x01|
|FX_FAT_ENTRY_START                    |2|
|FX_FAT_MAP_SIZE                       |128|
|FX_FAT_READ_ERROR                     |0x03|
|FX_FAT_SECTOR                         |2|
|FX_FAT12                              |0x01|
|FX_FAT16                              |0x04|
|FX_FAT32                              |0x0B|
|FX_FAULT_TOLERANT_CACHE_SIZE          |1024|
|FX_FILE_ABORTED_ID                    |0x46494C41UL|
|FX_FILE_CLOSED_ID                     |0x46494C43UL|
|FX_FILE_CORRUPT                       |0x08|
|FX_FILE_ID                            |0x46494C45UL|
|FX_FILE_SIZE_ERROR                    |0x08|
|FX_FILE_SYSTEM_TYPE                   |0x036|
|FX_FREE_CLUSTER                       |0x0000|
|FX_HEADS                              |0x01A|
|FX_HIDDEN                             |0x02|
|FX_HIDDEN_SECTORS                     |0x01C|
|FX_HOUR_MASK                          |0x1F|
|FX_HOUR_SHIFT                         |11|
|FX_HUGE_SECTORS                       |0x020|
|FX_INITIAL_DATE                       |0x4761|
|FX_INITIAL_TIME                       |0x0000|
|FX_INVALID_ATTR                       |0x19|
|FX_INVALID_CHECKSUM                   |0x95|
|FX_INVALID_DAY                        |0x14|
|FX_INVALID_HOUR                       |0x15|
|FX_INVALID_MINUTE                     |0x16|
|FX_INVALID_MONTH                      |0x13|
|FX_INVALID_NAME                       |0x0C|
|FX_INVALID_OPTION                     |0x24|
|FX_INVALID_PATH                       |0x0D|
|FX_INVALID_SECOND                     |0x17|
|FX_INVALID_STATE                      |0x97|
|FX_INVALID_YEAR                       |0x12|
|FX_IO_ERROR                           |0x90|
|FX_JUMP_INSTR                         |0x000|
|FX_LAST_CLUSTER_1                     |0xFFF8|
|FX_LAST_CLUSTER_1_32                  |0x0FFFFFF8|
|FX_LAST_CLUSTER_2                     |0xFFFF|
|FX_LAST_CLUSTER_2_32                  |0x0FFFFFFF|
|FX_LONG_NAME                          |0xF|
|FX_LONG_NAME_ENTRY_LEN                |13|
|FX_LOST_CLUSTER_ERROR                 |0x04|
|FX_MAX_12BIT_CLUST                    |0x0FF0|
|FX_MAX_EX_FAT_NAME_LEN                |255|
|FX_MAX_FAT_CACHE                      |256|
|FX_MAX_LAST_NAME_LEN                  |256|
|FX_MAX_LONG_NAME_LEN                  |256|
|FX_MAX_SECTOR_CACHE                   |256|
|FX_MAX_SHORT_NAME_LEN                 |13|
|FX_MAXIMUM_HOUR                       |23|
|FX_MAXIMUM_MINUTE                     |59|
|FX_MAXIMUM_MONTH                      |12|
|FX_MAXIMUM_PATH                       |256|
|FX_MAXIMUM_SECOND                     |59|
|FX_MAXIMUM_YEAR                       |2107|
|FX_MEDIA_ABORTED_ID                   |0x4D454441UL|
|FX_MEDIA_CLOSED_ID                    |0x4D454443UL|
|FX_MEDIA_ID                           |0x4D454449UL|
|FX_MEDIA_INVALID                      |0x02|
|FX_MEDIA_NOT_OPEN                     |0x11|
|FX_MEDIA_TYPE                         |0x015|
|FX_MINUTE_MASK                        |0x3F|
|FX_MINUTE_SHIFT                       |5|
|FX_MONTH_MASK                         |0x0F|
|FX_MONTH_SHIFT                        |5|
|FX_NO_FAT                             |0xFF|
|FX_NO_MORE_ENTRIES                    |0x0F|
|FX_NO_MORE_SPACE                      |0x0A|
|FX_NOT_A_FILE                         |0x05|
|FX_NOT_AVAILABLE                      |0x94|
|FX_NOT_DIRECTORY                      |0x0E|
|FX_NOT_ENOUGH_MEMORY                  |0x91|
|FX_NOT_FOUND                          |0x04|
|FX_NOT_IMPLEMENTED                    |0x22|
|FX_NOT_OPEN                           |0x07|
|FX_NOT_USED                           |0x0001|
|FX_NULL                               |0|
|FX_NUMBER_OF_FATS                     |0x010|
|FX_OEM_NAME                           |0x003|
|FX_OPEN_FOR_READ                      |0|
|FX_OPEN_FOR_READ_FAST                 |2|
|FX_OPEN_FOR_WRITE                     |1|
|FX_PTR_ERROR                          |0x18|
|FX_READ_CONTINU                       |0x96|
|FX_READ_ONLY                          |0x01|
|FX_RESERVED                           |0x025|
|FX_RESERVED_1                         |0xFFF0|
|FX_RESERVED_1_32                      |0x0FFFFFF0|
|FX_RESERVED_2                         |0xFFF6|
|FX_RESERVED_2_32                      |0x0FFFFFF6|
|FX_RESERVED_SECTOR                    |0x00E|
|FX_ROOT_CLUSTER_32                    |0x02C|
|FX_ROOT_DIR_ENTRIES                   |0x011|
|FX_SECOND_MASK                        |0x1F|
|FX_SECTOR_CACHE_DEPTH                 |4|
|FX_SECTOR_CACHE_HASH_ENABLE           |16|
|FX_SECTOR_CACHE_HASH_MASK             |0x3|
|FX_SECTOR_INVALID                     |0x89|
|FX_SECTORS                            |0x013|
|FX_SECTORS_CLUSTER                    |0x00D|
|FX_SECTORS_PER_FAT                    |0x016|
|FX_SECTORS_PER_FAT_32                 |0x024|
|FX_SECTORS_PER_TRK                    |0x018|
|FX_SEEK_BACK                          |3|
|FX_SEEK_BEGIN                         |0|
|FX_SEEK_END                           |1|
|FX_SEEK_FORWARD                       |2|
|FX_SIG_BYTE_1                         |0x55|
|FX_SIG_BYTE_2                         |0xAA|
|FX_SIG_OFFSET                         |0x1FE|
|FX_SIGN_EXTEND                        |0xF000|
|FX_SUCCESS                            |0x00|
|FX_SYSTEM                             |0x04|
|FX_TRUE                               |1|
|FX_UNKNOWN_SECTOR                     |0|
|FX_VOLUME                             |0x08|
|FX_VOLUME_ID                          |0x027|
|FX_VOLUME_LABEL                       |0x02B|
|FX_WRITE_PROTECT                      |0x23|
|FX_YEAR_MASK                          |0x7F|
|FX_YEAR_SHIFT                         |9|

## Listings by Value

| Constant (by value)   |  Value         |
|-----------|-----------|
|FX_DIR_ENTRY_DONE|0x00|
|FX_DRIVER_READ|0|
|FX_FALSE|0|
|FX_FREE_CLUSTER|0x0000|
|FX_INITIAL_TIME|0x0000|
|FX_JUMP_INSTR|0x000|
|FX_NULL|0|
|FX_OPEN_FOR_READ|0|
|FX_SEEK_BEGIN|0|
|FX_SUCCESS|0x00|
|FX_UNKNOWN_SECTOR|0|
|FX_BOOT_ERROR|0x01|
|FX_BOOT_SECTOR|1|
|FX_DRIVER_WRITE|1|
|FX_FAT_CHAIN_ERROR|0x01|
|FX_NOT_USED|0x0001|
|FX_OPEN_FOR_WRITE|1|
|FX_READ_ONLY|0x01|
|FX_FAT12|0x01|
|FX_SEEK_END|1|
|FX_TRUE|1|
|FX_DIRECTORY_ERROR|0x02|
|FX_HIDDEN|0x02|
|FX_MEDIA_INVALID|0x02|
|FX_DRIVER_FLUSH|2|
|FX_FAT_ENTRY_START|2|
|FX_FAT_SECTOR|2|
|FX_OPEN_FOR_READ_FAST|2|
|FX_SEEK_FORWARD|2|
|FX_12BIT_SIZE|3|
|FX_DIR_EXT_SIZE|3|
|FX_DIRECTORY_SECTOR|3|
|FX_DRIVER_ABORT|3|
|FX_FAT_CACHE_HASH_MASK|0x3|
|FX_FAT_READ_ERROR|0x03|
|FX_OEM_NAME|0x003|
|FX_SECTOR_CACHE_HASH_MASK|0x3|
|FX_SEEK_BACK|3|
|FX_DATA_SECTOR|4|
|FX_DRIVER_INIT|4|
|FX_FAT_CACHE_DEPTH|4|
|FX_FAT16|0x04|
|FX_LOST_CLUSTER_ERROR|0x04|
|FX_NOT_FOUND|0x04|
|FX_SECTOR_CACHE_DEPTH|4|
|FX_SYSTEM|0x04|
|FX_DRIVER_BOOT_READ|5|
|FX_MINUTE_SHIFT|5|
|FX_MONTH_SHIFT|5|
|FX_NOT_A_FILE|0x05|
|FX_ACCESS_ERROR|0x06|
|FX_BIGDOS|0x06|
|FX_DRIVER_RELEASE_SECTORS|6|
|FX_DRIVER_BOOT_WRITE|7|
|FX_NOT_OPEN|0x07|
|FX_DIR_NAME_SIZE|8|
|FX_DIR_RESERVED|8|
|FX_DRIVER_UNINIT|8|
|FX_FILE_CORRUPT|0x08|
|FX_FILE_SIZE_ERROR|0x08|
|FX_VOLUME|0x08|
|FX_END_OF_FILE|0x09|
|FX_YEAR_SHIFT|9|
|FX_NO_MORE_SPACE|0x0A|
|FX_EF_MUST_BE_ZERO|11|
|FX_ALREADY_CREATED|0x0B|
|FX_FAT32|0x0B|
|FX_BYTES_SECTOR|0x00B|
|FX_HOUR_SHIFT|11|
|FX_INVALID_NAME|0x0C|
|FX_MAXIMUM_MONTH|12|
|FX_INVALID_PATH|0x0D|
|FX_SECTORS_CLUSTER|0x00D|
|FX_LONG_NAME_ENTRY_LEN|13|
|FX_MAX_SHORT_NAME_LEN|13|
|FX_NOT_DIRECTORY|0x0E|
|FX_RESERVED_SECTORS|0x00E|
|FX_LONG_NAME|0xF|
|FX_MONTH_MASK|0x0F|
|FX_NO_MORE_ENTRIES|0x0F|
|FX_DIR_NOT_EMPTY|0x10|
|FX_DIRECTORY|0x10|
|FX_MAX_FAT_CACHE|16|
|FX_MAX_SECTOR_CACHE|16|
|FX_NUMBER_OF_FATS|0x010|
|FX_SECTOR_CACHE_HASH_ENABLE|16|
|FX_MEDIA_NOT_OPEN|0x11|
|FX_ROOT_DIR_ENTRIES|0x011|
|FX_INVALID_YEAR|0x12|
|FX_INVALID_MONTH|0x13|
|FX_SECTORS|0x013|
|FX_INVALID_DAY|0x14|
|FX_INVALID_HOUR|0x15|
|FX_MEDIA_TYPE|0x015|
|FX_INVALID_MINUTE|0x16|
|FX_SECTORS_PER_FAT|0x016|
|FX_INVALID_SECOND|0x17|
|FX_MAXIMUM_HOUR|23|
|FX_PTR_ERROR|0x18|
|FX_SECTORS_PER_TRK|0x018|
|FX_INVALID_ATTR|0x19|
|FX_HEADS|0x01A|
|FX_HIDDEN_SECTORS|0x01C|
|FX_DAY_MASK|0x1F|
|FX_HOUR_MASK|0x1F|
|FX_SECOND_MASK|0x1F|
|FX_ARCHIVE|0x20|
|FX_CALLER_ERROR|0x20|
|FX_DIR_ENTRY_SIZE|32|
|FX_HUGE_SECTORS|0x020|
|FX_BUFFER_ERROR|0x21|
|FX_MAX_LONG_NAME_LEN|33|
|FX_NOT_IMPLEMENTED|0x22|
|FX_WRITE_PROTECT|0x23|
|FX_DRIVE_NUMBER|0x024|
|FX_INVALID_OPTION|0x24|
|FX_SECTORS_PER_FAT_32|0x024|
|FX_RESERVED|0x025|
|FX_BOOT_SIG|0x026|
|FX_VOLUME_ID|0x027|
|FX_VOLUME_LABEL|0x02B|
|FX_ROOT_CLUSTER_32|0x02C|
|FX_FILE_SYSTEM_TYPE|0x036|
|FX_MAXIMUM_MINUTE|59|
|FX_MAXIMUM_SECOND|59|
|FX_MINUTE_MASK|0x3F|
|FX_EF_PARTITION_OFFSET|64|
|FX_EF_VOLUME_LENGTH|72|
|FX_EF_FAT_OFFSET|80|
|FX_EF_FAT_LENGTH|84|
|FX_SIG_BYTE_1|0x55|
|FX_EF_CLUSTER_HEAP_OFFSET|88|
|FX_EF_CLUSTER_COUNT|92|
|FX_EF_FIRST_CLUSTER_OF_ROOT_DIR|96|
|FX_EF_VOLUME_SERIAL_NUMBER|100|
|FX_EF_FILE_SYSTEM_REVISION|104|
|FX_EF_VOLUME_FLAGS|106|
|FX_EF_BYTE_PER_SECTOR_SHIFT|108|
|FX_EF_SECTOR_PER_CLUSTER_SHIFT|109|
|FX_EF_NUMBER_OF_FATS|110|
|FX_EF_DRIVE_SELECT|11|
|FX_EF_PERCENT_IN_USE|112|
|FX_EF_RESERVED|113|
|FX_EF_BOOT_CODE|120|
|FX_YEAR_MASK|0x7F|
|FX_FAT_MAP_SIZE|128|
|FX_SECTOR_INVALID|0x89|
|FX_IO_ERROR|0x90|
|FX_NOT_ENOUGH_MEMORY|0x91|
|FX_ERROR_FIXED|0x92|
|FX_ERROR_NOT_FIXED|0x93|
|FX_NOT_AVAILABLE|0x94|
|FX_INVALID_CHECKSUM|0x95|
|FX_READ_CONTINUE|0x96|
|FX_INVALID_STATE|0x97|
|FX_SIG_BYTE_2|0xAA|
|FX_DIR_ENTRY_FREE|0xE5|
|FX_NO_FAT|0xFF|
|FX_MAX_EX_FAT_NAME_LEN|255|
|FX_MAXIMUM_PATH|256|
|FX_SIG_OFFSET|0x1FE|
|FX_BOOT_SECTOR_SIZE|512|
|FX_FAULT_TOLERANT_CACHE_SIZE|1024|
|FX_BASE_YEAR|1980|
|FX_MAXIMUM_YEAR|2107|
|FX_MAX_12BIT_CLUST|0x0FF0|
|FX_12_BIT_FAT_SIZE|4086|
|FX_INITIAL_DATE|0x4761|
|FX_SIGN_EXTEND|0xF000|
|FX_RESERVED_1|0xFFF0|
|FX_16_BIT_FAT_SIZE|65525|
|FX_RESERVED_2|0xFFF6|
|FX_BAD_CLUSTER|0xFFF7|
|FX_LAST_CLUSTER_1|0xFFF8|
|FX_LAST_CLUSTER_2|0xFFFF|
|FX_RESERVED_1_32|0x0FFFFFF0|
|FX_RESERVED_2_32|0x0FFFFFF6|
|FX_BAD_CLUSTER_32|0x0FFFFFF7|
|FX_LAST_CLUSTER_1_32|0x0FFFFFF8|
|FX_LAST_CLUSTER_2_32|0x0FFFFFFF|
|FX_FILE_ABORTED_ID|0x46494C41UL|
|FX_FILE_CLOSED_ID|0x46494C43UL|
|FX_FILE_ID|0x46494C45UL|
|FX_MEDIA_ABORTED_ID|0x4D454441UL|
|FX_MEDIA_CLOSED_ID|0x4D454443UL|
|FX_MEDIA_ID|0x4D454449UL|
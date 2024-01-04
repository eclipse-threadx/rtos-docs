---
title: Chapter 1 - Overview of LevelX
description: LevelX provides NAND and NOR flash wear leveling facilities to embedded applications.
---

# Chapter 1 - Overview of LevelX

LevelX provides NAND and NOR flash wear leveling facilities to embedded applications. Since both NAND and NOR flash memory can only be erased a finite number of times, it's critical to distribute the flash memory use evenly. This is typically called "wear leveling" and is the purpose behind LevelX.

The algorithm that chooses which flash block to reuse is primarily based on the erase count, but not entirely. The block with the lowest erase count might not be chosen if there is another block that has an erase count within an acceptable delta from the minimal erase count and that has a greater number of obsolete mappings. In such cases, the block with the greatest number of obsolete mappings will be erased and reused, thus saving overhead in moving valid mapping entries.

LevelX supports multiple instances of NAND and/or NOR parts, i.e., the application can utilize separate instances of LevelX within the same application. Each instance requires its own control block provided by the application as well as its own flash driver.

LevelX presents to the user an array of logical sectors that are mapped to physical flash memory inside of LevelX. To enhance performance, LevelX also provides a cache of the most recent logical sector mappings. The size of this cache is defined by the programmer. Applications may use LevelX in conjunction with FileX or may read/write logical sectors directly. LevelX has no dependency on FileX and very little dependency on ThreadX (only primitive ThreadX data types are used).

LevelX is designed for fault tolerance. Flash updates are performed in a multiple-step process that can be interrupted in each step. LevelX automatically recovers to the optimal state during the next operation.

LevelX requires a flash driver for physical access to the underlying flash memory. Example NAND and NOR simulated drivers are provided and can be used as a good starting point for implementing actual LevelX drivers. In addition, driver requirements are detailed later in this documentation.

The following chapters describe the functional operation for the NAND and NOR LevelX support.

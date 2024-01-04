---
title: Appendix H - GUIX Build-Time Configuration flags
description: Learn about GUIX build-time configuration flags.
---

# Appendix H - GUIX Build-Time Configuration flags

GUIX support several conditional compilation options and configuration values. The default setting for these conditionals and configuration values can be overridden by pre-defining the value, either in your gx_user.h header file or on your compiler command line.

**GX_DISABLE_THREADX_BINDING**
- Default: Undefined
- Description: This conditional can be used to disable the default ThreadX RTOS binding. If you want to run GUIX with an RTOS other than ThreadX, you should #define GX_DISABLE_THREADX_BINDING and provide your own RTOS binding services.

**GX_SYSTEM_TIMER_MS**
- Default: 20
- Description: This value defines the desired GUIX timer interval or precision.

**TX_TIMER_TICKS_PER_SECOND**
- Default: 100
- Description: This value defines the number of TX timer interrupt frequencies. Since the default ThreadX interval timer is 10 ms, this value defaults to a 100-Hz frequency.

**GX_DISABLE_MULTITHREAD_SUPPORT**
- Default: Not defined
- Description: This compile-time conditional can be used to disable the GUIX API support for multiple threads invoking the GUIX API concurrently. If only one application thread will ever utilize the GUIX API, you should define this flag to reduce the system overhead associated with protecting critical code sections.

**GX_DISABLE_UTF8_SUPPORT**
- Default: Not Defined.
- Description: This compile-time conditional can be used to remove the GUIX internal support for UTF8 format string encoding. If you are using only character values M- 0xff in your application, turning on this #define will reduce the code size and overhead associated with supporting UTF8 format string encoding.

**GX_DISABLE_ARC_DRAWING_SUPPORT**
- Default: Not defined.
- Description: This conditional can be used to reduce the GUIX library code size and GX_DISPLAY structure size by removing support for the arc-drawing functions circle, arc, pie, and ellipse. These functions are not required by the default GUIX widget set.

**GX_DISABLE_SOFTWARE_DECODER_SUPPORT**
- Default: Not defined.
- Description: This conditional can be defined to remove the GUIX library runtime jpeg and png software decoder support. If your application does not require runtime decode of jpg or png files meaning your application does not use RAW format pixelmaps produced by Studio and does not read image files from an external filesystem, you can turn on this #define to reduce the GUIX library footprint.

**GX_DISABLE_BINARY_RESOURCE_SUPPORT**
- Default: Not defined
- Description: This conditional can be used to remove the GUIX library support for loading binary resource data. Binary resources can be used to do runtime binding of resource data with your GUIX application. If you are using only C source code format resource files, you can define this conditional to reduce your GUIX library footprint.

**GX_DISABLE_BRUSH_ALPHA_SUPPORT**
- Default: Not defined.
- Description: When running at 16 bpp and higher color depths, GUIX optionally supports drawing non-arc graphics, pixelmaps, and fonts with an alpha value defined by the drawing context brush. Supporting this drawing mode introduces a small runtime overhead and library footprint increase, which can be eliminated by defining this flag if you do not require alpha-blending drawing support. Note that pixelmaps with alpha channel, anti-aliased fonts, and other anti-aliasing drawing modes are still supported regardless of this conditional setting.

**GX_DISABLE_THREADX_TIMER_SOURCE**
- Default: Not defined.
- Description: This conditional can be used to disable the ThreadX timer source. You should define GX_DISABLE_THREADX_TIMER_SOURCE if you want to use a different timer source.

**GX_ENABLE_ARM_HELIUM**
- Default: Not defined.
- Description: This conditional can be used to enable ARM Helium instructions for JPEG decoding, resulting in enhanced performance.

**GX_ENABLE_CANVAS_PARTIAL_FRAME_BUFFER**
- Default: Not defined.
- Description: This conditional can be used to enable partial frame buffer support for canvas. When this conditional is defined, you are able to define a canvas buffer that is smaller than the canvas size. This is useful when the system has limited memory.

**GX_CANVAS_REFRESH_DIRECTION_HORIZONTAL**
- Default: Not defined.
- Description: This conditional is used when canvas partial frame buffer feature is enabled. This conditional can be used to enable horizontal refresh direction for canvas, specifically from left to right.

**GX_CANVAS_REFRESH_DIRECTION_VERTICAL**
- Default: Not defined.
- Description: This conditional is used when canvas partial frame buffer feature is enabled. This conditional can be used to enable vertical refresh direction for canvas, specifically from top to bottom. 

**GX_REPEAT_BUTTON_INITIAL_TICS**
- Default: 10.
- Description: If a button has style GX_STYLE_BUTTON_REPEAT, this value defines how long the button
waits before beginning to send repeated GX_EVENT_CLICKED events.

**GX_MAX_QUEUE_EVENTS**
- Default: 48.
- Description: Defines the size of the GUIX event queue in units of event structure entries. If the event queue overflows, events being pushed into the queue are discarded and GX_SYSTEM_ERROR is returned by the gx_system_event_send() function.

**GX_MAX_DIRTY_AREAS**
- Default: 64.
- Description: Defines the maximum number of unique dirty list entries that can be maintained by one canvas. When the dirty list overflows, GUIX will default to marking the canvas root window as dirty, which is less efficient than drawing individual child widgets.

**GX_MAX_CONTEXT_NESTING**
- Default: 8.
- Description: Defines the maximum nesting of the drawing context stack. This is equivalent to the maximum nesting of parent/child/child/child widgets within the UI definition.

**GX_MAX_INPUT_CAPTURE_NESTING**
- Default: 4.
- Description: Defines the size of the stack used to maintain the list of widgets that have captures the user input (mouse and keyboard).

**GX_SYSTEM_THREAD_PRIORITY**
- Default: 16.
- Description: Defines the priority of the GUIX thread created during gx_system_initialize().

**GX_SYSTEM_THREAD_TIMESLICE**
- Default: 10.
- Description: Defines the GUIX thread timeslice in terms of RTOS timer ticks. If other threads are defined with the same priority as the GUIX thread, this value determines how often those competing threads are granted CPU control.

**GX_CURSOR_BLINK_INTERVAL**
- Default: 20.
- Description: Defines the rate at which the input cursor blinks for text input widgets. This value is in terms of GUIX timer ticks, which by default is defines as 50 ms, so a value of 20 indicates that the input cursor blinks once per second.

**GX_MULTI_LINE_INDEX_CACHE_SIZE**
- Default: 32.
- Description: Defines the size of the list-start index cache maintained by the multi-line text view and multi-line text input widgets. This cache is used to accomplish fast vertical scrolling of multi line text widgets. For best performance, the cache size should be set greater than the number of visible rows of the largest multi line text widget defined by the application. For example, if the most visible rows for any text widget are 20 rows, the application might define a cache size
of 32 (the default), which allows GUIX to scroll vertically without recalculating all line start indexes.

**GX_MULTI_LINE_TEXT_BUTTON_MAX_LINES**
- Default: 4.
- Description: The multi-line text button control block maintains a pointer to each line of text to be displayed by the button. This value determines the number of text pointers needed by the worst case multi-line text button.

**GX_POLYGON_MAX_EDGE_NUM**
- Default: 10.
- Description: This value determines the most complex polygon that can be drawn by GUIX. The polygon drawing algorithm determines the lines needed to define the polygon edges, and this definition defines the maximum number of edges that can be supported.

**GX_NUMERIC_SCROLL_WHEEL_STRING_BUFFER_SIZE**
- Default: 16.
- Description: For a number scroll wheel, the scroll wheel widget converts integer values to ascii strings. This value determines the maximum length of the string required to display the assigned integer values.

**GX_DEFAULT_CIRCULAR_GAUGE_ANIMATION_DELAY**
- Default: 5.
- Description: Defines the number of GUIX timer ticks (50 ms) between updates of a circular gauge configured to animate the needle movement between last and current angular position.

**GX_NUMERIC_PROMPT_BUFFER_SIZE**
- Default: 16.
- Description: A numeric prompt allocates a buffer to convert an integer value assigned to the prompt to an ascii string. This definition defines the size of this character buffer.

**GX_ANIMATION_POOL_SIZE**
- Default: 6.
- Description: GUIX defines an animation pool from which animation information structures can be dynamically allocated and returned, using gx_system_animation_get and gx_system_animation_free()
APIs. This definition defines the size of this animation control block pool.

**GX_MOUSE_SUPPORT**
- Default: Not defined.
- Description: This definition enables support for mouse input. Software mouse requires that the display driver draw and track the mouse cursor, which adds extra overhead to the display driver. This definition should only be defined when a mouse (not a touch screen) must be supported.

**GX_HARDWARE_MOUSE_SUPPORT**
- Default: Not defined.
- Description: When this definition is defined, the GUIX display driver utilizes hardware mouse cursor drawing support. This reduces the memory required to capture the canvas memory beneath the mouse cursor and improves system performance for those hardware targets support a mouse overlay graphics layer.

**GX_FONT_KERNING_SUPPORT**
- Default: Not defined.
- Description: This definition can be defined to enable font kerning support. Font kerning improves glyph spacing for certain glyph combinations. This support adds a small amount of overhead to the
runtime string drawing functions, and also adds a small amount of size to the font data structures.

**GX_WIDGET_USER_DATA**
- Default: Not defined.
- Description: If defined, this adds a user-defined data field to the GX_WIDGET control block. This data field can be assigned using the properties view within GUIX Studio. This data field is ignored by GUIX internally, but can be used by application software for many purposes.

**GUIX_5_4_0_COMPATIBILITY**
- Default: Not defined.
- Description: Certain GUI APIs were modified after release 5.4.0 to add support for disabled text colors and to improve the accuracy of certain math functions by using fixed-point match parameters. These changes make GUIX library releases after 5.4.0 incompatible with previous releases. However, by turning on this #define, the library can be built such that the APIs fully compatible with releases <= 5.4.0, meaning that no changes are needed in existing applications to compile with the latest GUIX library release.

**GX_MAX_STRING_LENGTH**
- Default: 102400
- Description: Defines the maximum length of a string, which is used to test invalid strings. If the input string is exceeding the maximum string length, it will be regard as invalid.
---
title: Chapter 4 - Description of GUIX Services
description: This chapter contains a description of all GUIX services (listed below) in alphabetic order.
---
# Chapter 4 - Description of GUIX Services

This chapter contains a description of all GUIX services (listed below) in alphabetic order.  

In the "Return Values" section in the following API descriptions, values in **BOLD** are not affected by the **GX_DISABLE_ERROR_CHECKING** define that is used to disable API error checking, while non-bold values are completely disabled.


## gx_accordion_menu_create

Create an accordion menu

### Prototype

```C
UINT gx_accordion_menu_create(
    GX_ACCORDION_MENU *accordion,
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent,
    ULONG style,
    USHORT accordion_menu_id,
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates an accordion menu as specified and attaches the accordion menu to the supplied parent widget. An accordion menu is an expanding/collapsing menu display widget. It accepts all types of widget as its child menu items. Accordion menus can be nested, meaning several levels of menu depth can be created.

To insert a child item into a menu item widget, it's recommended to use GX_MENU type widget as a parent menu item.

Tips for creating a single level accordion menu:

1.  Create an accordion menu.

2.  Attach GX_MENU type widgets to the accordion menu.

3.  Attach child widgets to the GX_MENU type parent. The child item type can be any GUIX widget type.

Tips for creating multi level accordion menu:

1.  Create an accordion menu.

2.  Attach GX_MENU type widgets to the accordion menu.

3.  Attach GX_ACCORDION_MENU type widget to the GX_MENU type parent.

4.  Attach menu items to the GX_ACCORDION_MENU type parent as described in the single level accordion menu creation.

### Parameters

- *accordion*: Pointer to the accordion menu control block.
- *name*: Name of the accordion menu.
- *parent*: Pointer to parent widget.
- *style*: Style of the widget. **Appendix D** contains pre-defined general styles for all widgets as well as widget specific styles.
- *accordion_menu_id*: Application-defined ID of the accordion menu.
- *size*: Size of the accordion menu.

### Return Values

- **GX_SUCCESS** (0x00) Successful accordion menu creation.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.

### Allowed From

Initialization and threads

### Example

```C
GX_ACCORDION_MENU my_accordion;
GX_MENU menu_1;
GX_MENU item_1;
GX_RECTANGLE size;

gx_utility_rectangle_define(&size, 100, 100, 300, 150);

status = gx_accordion_menu_create(&my_accordion, "my_accordion",
                                  parent, GX_STYLE_ENABLED,
                                  MY_ACCORDION_ID, &size);

gx_menu_create(&menu_1, "menu_1", &my_accordion,
               GX_STRING_ID_MENU_1, GX_ID_NONE,
               GX_STYLE_ENABLED | GX_TYLE_BORDER_THIN, 0, &size);

gx_menu_create(&item_1, "item_1", &my_accordion,
               GX_STRING_ID_ITEM_1, GX_ID_NONE,
               GX_STYLE_ENABLED | GX_STYLE_BORDER_THIN, 0, &size);

gx_text_offset_set(&item_1, 30, 0);

gx_menu_insert(&menu_1, &item_1);

/* If status is GX_SUCCESS the accordion menu was successfully created. */

```

The demo application demo_guix_widget_types, provided as part of the GUIX Studio installation, provides a complete example of using the accordion menu widget.

### See Also

- gx_accordion_menu_draw
- gx_accordion_menu_event_process
- gx_accordion_menu_position
- gx_menu_create
- gx_menu_draw
- gx_menu_insert
- gx_menu_remove
- gx_menu_text_draw
- gx_menu_text_offset_set

## gx_accordion_menu_draw

Draw accordion menu

### Prototype

```C
VOID gx_accordion_menu_draw(GX_ACCORDION_MENU *accordion);
```

### Description

This service draws the specified accordion menu. This service is normally called internally by the GUIX canvas refresh mechanism, but is exposed to the application to assist with implementing custom drawing functions for custom accordion menu widgets.

### Parameters

- *accordion*: Pointer to the accordion menu control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Define a custom accordion menu draw function. */

VOID my_accordion_menu_draw(GX_ACCORDION_MENU *accordion)
{
    /* Call default accordion menu draw. */
    gx_accordion_menu_draw(accordion);

    /* Add custom drawing here. */
}

```

### See Also

- gx_accordion_menu_create
- gx_accordion_menu_event_process
- gx_accordion_menu_position
- gx_menu_create
- gx_menu_draw
- gx_menu_insert
- gx_menu_remove
- gx_menu_text_draw
- gx_menu_text_offset_set

## gx_accordion_menu_event_process

Process an accordion menu event

### Prototype

```C
UINT gx_accordion_menu_event_process(
    GX_ACCORDION_MENU *accordion, 
    GX_EVENT *event_ptr);
```

### Description

This service processes an event for the specified accordion menu. This service should be called as the default event handler by any custom accordion menu event processing functions.

This service handles GX_EVENT_PEN_DOWN and **GX_EVENT_PEN_UP** events to expand/collapse a menu item.

### Parameters

- *accordion*: Pointer to accordion menu control block.
- *event_ptr*: Pointer to the event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful accordion menu event process.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Threads

### Example

```C
/* Call generic accordion menu event processing as part of custom event processing function. */

UINT custom_accordion_event_process(GX_ACCORDION_MENU *accordion, GX_EVENT *event)
{
    UINT status = GX_SUCCESS;

    switch(event->gx_event_type)
    {
    case xyz:
        /* Insert custom event handling here. */
        break;

    default:
        /* Pass all other events to the default accordion menu event processing. */
        status = gx_accordion_menu_event_process(accordion, event);
        break;
    }
    return status;
}

```

### See Also

- gx_accordion_menu_create
- gx_accordion_menu_draw
- gx_accordion_menu_position
- gx_menu_create
- gx_menu_draw
- gx_menu_insert
- gx_menu_remove
- gx_menu_text_draw
- gx_menu_text_offset_set

## gx_accordion_menu_position

Position menu items

### Prototype

```C
UINT gx_accordion_menu_position(GX_ACCORDION_MENU *accordion);
```

### Description

This service positions the menu items for the accordion menu. This function is normally called internally when the accordion menu is becoming visible. If you want to insert/remove items to/from an accordion menu, or change the expand styles of the child item, this function should be called to reposition the child items.

### Parameters

- *accordion*: Pointer to the accordion menu control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful accordion menu position.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Position menu items in the accordion menu "my_accordion". */
status = gx_accordion_menu_position (&my_accordion);

/* If status is GX_SUCCESS the children in the accordion menu
"my_accordion" are positioned. */

```

### See Also

- gx_accordion_menu_create
- gx_accordion_menu_draw
- gx_accordion_menu_event_process
- gx_menu_create
- gx_menu_draw
- gx_menu_insert
- gx_menu_remove
- gx_menu_text_draw
- gx_menu_text_offset_set

## gx_animation_canvas_define

Provide canvas memory to an animation controller

### Prototype

```C
UINT gx_animation_canvas_define(
    GX_ANIMATION *animation, 
    GX_CANVAS *canvas);
```

### Description

This service provides a memory canvas to an animation controller used to implement the animation sequence. This provided canvas should be large enough to hold the animation target widget.

When an animation canvas is defined, the target widget is drawn once to this animation canvas, and the screen slide or fade effect is accomplished by modifying the canvas offset and the canvas alpha value. When hardware support for multiple graphics layers is provided, defining an animation canvas that is bound to a hardware graphics overlay layer can *greatly* improve the performance of slide and fade animations.

The animation manager does require an animation canvas to execute fade-in and fade-out animation types if running at color depth less than 16 bpp.

### Parameters

- *animation*: Pointer to the animation control block.
- *canvas*: Memory canvas used to implement the translation animation.

### Return Values

- **GX_SUCCESS** (0x00) Successfully defined animation canvas.
- **GX_INVALID_STATUS** (0x10) Invalid animation status.
- GX_INVALID_MEMORY_SIZE (0x29) The provided memory block is not large enough to create the canvas.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_ANIMATION animation;
GX_CANVAS animation_canvas;
GX_ROOT_WINDOW animation_root;
/* Create animation canvas. */
status = gx_canvas_create(
        &animation_canvas, /* Canvas control block. */
        "animation_canvas", /* Name of canvas. */
        display, /* Display control block. */
        GX_ANIMATION_MANAGED, /* Type of canvas. */
        width, /* Width of canvas. */
        height, /* Height of canvas. */
        memory_area, /* Memory area of canvas. */
        memory_size /* Size of canvas memory. */
        );
if (status == GX_SUCCESS)
{
    /* Create the root window for the canvas. */
    status = gx_window_root_create(
        &animation_root, /* Root window control block. */
        "animation_root", /* Name of root window. */
        &animation_canvas, /* Canvas of the root window. */ GX_STYLE_NONE, /* Style of the window. */
        GX_ID_NONE, /* Root window ID. */
        &root_size /* Window size. */
        );
}
if (status == GX_SUCCESS)
{
    /* Define canvas for the animation. */
    status = gx_animation_canvas_define(&animation,
                &animation_canvas);
}
/* If status is GX_SUCCESS the new canvas was successfully set to the animation manager. */

```

### See Also

- gx_animation_create
- gx_animation_delete
- gx_animation_drag_disable
- gx_animation_drag_enable
- gx_animation_landing_speed_set
- gx_animation_start
- gx_animation_stop

## gx_animation_create

Create an animation controller

### Prototype

```C
UINT gx_animation_create(GX_ANIMATION *animation);
```

### Description

This service creates an animation controller. The controller is initialized to the idle state. One cannot start an animation unless it is in the IDLE state. The GX_ANIMATION control block pointer may be obtained using gx_system_animation_get(), or it may be a statically defined control block.

### Parameters

- *animation*: Pointer to animation control block.


### Return Values

- **GX_SUCCESS** (0x00) Successfully created animation controller.
- GX_ALREADY_CREATED (0x13) Control block already initialized.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_ANIMATION *animation;

/* Allocate an animation control from system pool. */
gx_system_animation_get(&animation);

/* Initialize the control block. */

if (animation)
{
    status = gx_animation_create(animation);
}

/* If status is GX_SUCCESS the new animation controller was successfully created and initialized. */

```

### See Also

- gx_animation_canvas_define
- gx_animation_delete
- gx_animation_drag_disable
- gx_animation_drag_enable
- gx_animation_start
- gx_animation_landing_speed_set
- gx_animation_stop
- gx_system_animation_get
- gx_system_animation_free

## gx_animation_delete

Delete one or multiple animation controllers

### Prototype

```C
UINT gx_animation_delete(GX_ANIMATION *animation, GX_WIDGET *parent);
```

### Description

This service deletes an animation sequence if the input animation pointer is set, otherwise, deletes all the animations belong to the specified parent widget.

### Parameters

- *animation*: Pointer to the animation control block.
- *parent*: Pointer to the parent widget.

### Return Values

- **GX_SUCCESS** (0x00) Successfully deleted animation controller(s).
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

- Delete One Animation

```C
GX_ANIMATION *animation;

/* Allocate an animation control from system pool. */
gx_system_animation_get(&animation);

if (animation)
{
    /* Create an animation. */
    gx_animation_create(animation);

    /* Delete an animation. */
    status = gx_animation_delete(animation, GX_NULL);
}

/* If status is GX_SUCCESS the animation controller was successfully deleted and returned back to system animation pool. */

```

- Delete Multiple Animations
```C

status = gx_animation_delete(GX_NULL, parent);

/* If status is GX_SUCCESS all the animations belong to the parent were successfully deleted. */

```

### See Also

- gx_animation_canvas_define
- gx_animation_create
- gx_animation_drag_disable
- gx_animation_drag_enable
- gx_animation_start
- gx_animation_landing_speed_set
- gx_animation_stop
- gx_system_animation_get
- gx_system_animation_free

## gx_animation_drag_disable

Disable the screen drag animation hook

### Prototype

```C
UINT gx_animation_drag_disable(
    GX_ANIMATION *animation, 
    GX_WIDGET *widget);
```

### Description

This service removes the screen drag animation hook procedure from the widget's default event process function and stops the animation sequence. The screen drag animation hook procedure handles events for a screen drag animation.

### Parameters

- *animation*: Pointer to the animation control block.
- *widget*: Pointer to the widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_ANIMATION (0x32) Drag animation is not enabled.

### Allowed From

Initialization and threads

### Example

```C
GX_ANIMATION animation;
GX_WIDGET *animation_parent;

/* Disable screen drag animation of "animation_parent". */
status = gx_animation_drag_disable(&animation, animation_parent);

/* If status is GX_SUCCESS the screen drag hook has been removed
    from the event process of "animation_parent". */

```

### See Also

- gx_animation_canvas_define
- gx_animation_create
- gx_animation_delete
- gx_animation_drag_enable
- gx_animation_landing_speed_set
- gx_animation_start
- gx_animation_stop
- gx_system_animation_get
- gx_system_animation_free

## gx_animation_drag_enable

Enable the screen drag animation hook

### Prototype

```C
UINT gx_animation_drag_enable(
    GX_ANIMATION *animation,
    GX_WIDGET *widget, 
    GX_ANIMATION_INFO *info);
```

### Description

This service sets the internally defined screen drag animation event process function as a hook procedure of a widget's default event process function. The screen drag animation event process function handles events for a screen drag animation.

The screen drag hook procedure becomes the default handler for pen input events sent to the target widget. The original widget event processing function is called in a daisy-chain fashion after checking for screen drag input event types.

### Parameters

- *animation*: Pointer to the animation control block.
- *widget*: Pointer to the widget control block.
- *info*: Animation information.

### Return Values

- **GX_SUCCESS** (0x00) Successful.
- **GX_INVALID_STATUS** (0x26) Invalid animation status.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_ANIMATION (0x32) Drag animation is already enabled.
- GX_INVALID_VALUE (0x22) Invalid value.
- GX_INVALID_WIDGET (0x12) Slide screen list is not provided.

### Allowed From

Initialization and threads

### Example

```C
GX_ANIMATION animation;
GX_ANIMATION_INFO info;
GX_WIDGET *animation_parent;
GX_WIDGET *screen_list[] = {
    screen_1,
    screen_2,
    screen_3,
    GX_NULL
}

memset(&info, 0, sizeof(GX_ANIMATION_INFO);
info.gx_animation_parent = animation_parent;

/* If GX_STYLE_ANIMATION_WRAP is set, the screen list will wrap itself. */
info.gx_animation_style = GX_ANIMATION_SCREEN_DRAG |
                          GX_ANIMATION_HORIZONTAL |
                          GX_STYLE_ANIMATION_WRAP;
info.gx_animation_frame_interval = 1;
info.gx_animation_slide_screen_list = screen_list;

status = gx_animation_drag_enable(&animation, animation_parent,
                                  info);

/* If status is GX_SUCCESS the screen slide animation event
    process function has been set as a hook procedure of
    "animation_parent". */
```

### See Also

- gx_animation_canvas_define
- gx_animation_create
- gx_animation_delete
- gx_animation_drag_disable
- gx_animation_landing_speed_set
- gx_animation_start
- gx_animation_stop
- gx_system_animation_get
- gx_system_animation_free

## gx_animation_landing_speed_set

Set landing speed for the screen drag animation

### Prototype

```C
UINT gx_animation_landing_speed_set(
    GX_ANIMATION *animation, 
    USHORT shift_per_step);
```

### Description

This service sets the landing speed for the screen drag animation.

### Parameters

- *animation*: Pointer to the animation control block.
- *shift_per_step*: Shift distance for each step.

### Return Values

- **GX_SUCCESS** (0x00) Successful.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_VALUE (0x22) Invalid parameter.

### Allowed From

Initialization and threads

### Example

```C
/* Set landing speed of "my_animation" to 20. */
status = gx_animation_landing_peed_set(&my_animation, 20);

/* If status is GX_SUCCESS the landing speed is successfully set to 20. */
```

### See Also

- gx_animation_canvas_define
- gx_animation_create
- gx_animation_delete
- gx_animation_slide_disable
- gx_animation_slide_enable
- gx_animation_start
- gx_animation_stop
- gx_system_animation_get
- gx_system_animation_free

## gx_animation_start

Start a timer-driven animation

### Prototype

```C
UINT gx_animation_start(
    GX_ANIMATION *animation, 
    GX_ANIMATION_INFO *params);
```

### Description

This service initiates an animation sequence using a previously created animation instance and a new set of animation parameters. This function makes a local copy of the parameters, meaning the parameter structure does not need to be statically defined.

The **GX_ANIMATION** control structure can be statically defined by the application, or it can be obtained using the ***gx_system_animation_get*** API function.

The **GX_ANIMATION_INFO** structure defines the parameters of the animation to be executed. For a complete description of this structure and the meaning of each field, refer to the GUIX Animation Component section in Chapter 3 of this manual.

### Parameters

- *animation*: Pointer to the animation control block.
- *params*: Pointer to the parameter structure.

### Return Values

- **GX_SUCCESS** (0x00) Successful.
- **GX_INVALID_VALUE** (0x22) Invalid parameter.
- **GX_FAILURE** (0x10) Animation root window not found.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Invalid animation target.
- GX_INVALID_STATUS (0x26) Invalid animation status.

### Allowed From

Initialization and threads

### Example

```C
GX_ANIMATION_INFO params;
GX_ANIMATION *animation;

/* Obtain an animation control block pointer. */
gx_system_animation_get(&animation);
if (animation)
{
    /* Define a slide down and to the right. */
    params.gx_animation_start_position.gx_point_x = 0;
    params.gx_animation_start_position.gx_point_y = 0;
    params.gx_animation_end_position.gx_point_x = 100;
    params.gx_animation_end_position.gx_point_y = 200;
    params.gx_animation_style= GX_ANIMATION_TRANSLATE;
    params.gx_animation_target = &my_window;
    params.gx_animation_parent = root_window;
    params.gx_animation_start_alpha = 255;
    params.gx_animation_end_alpha = 255;

    params.gx_animation_delay_before = 0;
    params.gx_animation_steps = 10;
    params.gx_animation_tick_rate = 2;

    status = gx_animation_start(&animation, &params);
}

/* If status is GX_SUCCESS the animation is initiated. */
```

### See Also

- gx_animation_canvas_define
- gx_animation_create
- gx_animation_slide_disable
- gx_animation_slide_enable
- gx_animation_landing_speed_set
- gx_animation_stop
- gx_system_animation_get
- gx_system_animation_free

## gx_animation_stop

Stop an active timer-driven animation

### Prototype

```C
UINT gx_animation_stop(GX_ANIMATION *animation);
```

### Description

Stop a previously started animation. If the animation control block pointer was allocated using gx_system_animation_get, the application can re-use the control block or return it to the system pool using gx_system_animation_free().

### Parameters

- *animation*: Pointer to the animation control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful GX_PTR_ERROR (0x07) Invalid pointer.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_STATUS (0x26) Invalid controller status.

### Allowed From

Initialization and threads

### Example

```C
GX_ANIMATION animation;

status = gx_animation_stop(&animation);

/* If status is GX_SUCCESS the animation is stopped. */

```

### See Also

- gx_animation_canvas_define
- gx_animation_create
- gx_animation_delete
- gx_animation_drag_disable
- gx_animation_drag_enable
- gx_animation_start
- gx_system_animation_get
- gx_system_animation_free

## gx_binres_font_load

Load font from the standalone binary data to the specified buffer.

### Prototype

```C
UINT gx_binres_font_load(GX_UBYTE *root_address, UINT font_index, GX_UBYTE *buffer, ULONG *buffer_size);
```

### Description

This service loads a font from standalone binary data to the specified buffer. The font page structures, as well as glyph structures, shall be allocated from the user-supplied buffer. Instead of copying the actual glyph data to the buffer, the function uses pointers to the glyph data in the binary data to compose the font. Therefore, it is crucial to ensure that the supplied buffer is sufficient to hold the page and glyph structures of the font.

The standalone binary data differs from the original version of the GUIX binary data, which typically holds a complete theme table or language table information. In contrast, the standalone binary data comprises one or more standalone resources. To learn more about how to generate the standalone binary file, refer to the GUIX Studio documentation.

### Parameters

- *root_address*: The root address of the binary resource data.
- *font_index*: The zero-based index used to identify the font to be loaded.
- *buffer*: The buffer used for loading the font.
- *buffer_size*: The size of the buffer provided. If the size of the provided buffer is insufficient, it is overwritten with the required size.

### Return Values

- **GX_SUCCESS** (0x00) Successful
- **GX_INVALID_FORMAT** (0x24) Invalid binary resource
- **GX_PTR_ERROR** (0x07) Invalid pointer
- **GX_NOT_FOUND** (0x09) Invalid font index
- **GX_INVALID_MEMORY_SIZE** (0x29) The input buffer size is insufficient

### Allowed From

Initialization and threads

### Example

```C
#define FONT_BUFFER_SIZE 1024

/* Specify address that binary resource data is located. */
GX_UBYTE *root_address = 0x60000000;
GX_CHAR   buffer[FONT_BUFFER_SIZE];
ULONG     buffer_size = FONT_BUFFER_SIZE;

status = gx_binres_font_load(root_address, 0, buffer, &buffer_size);

/* If status is GX_SUCCESS, the first font in the binary data was successfully loaded. */

```

### See Also

- gx_binres_pixelmap_load

## gx_binres_language_count_get

Returns the number of languages present in binary resource data

### Prototype

```C
UINT gx_binres_language_count_get(
    GX_UBYTE *root_address, 
    GX_VALUE *returned_count);
```

### Description

This service parses a binary resource data header to return the number of languages contained within the binary data. This is useful for applications that must display a selection list to the user allowing the user to select from a choice of languages.

### Parameters

- *root_address*: Address of the binary resource data in memory.
- *return_count*: Location to store the returned language count.

### Return Values

- **GX_SUCCESS** (0x00) Successful.
- **GX_INVALID_FORMAT** (0x24) Invalid binary resource.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_VALUE language_count = 0;

/* Specify address that binary resource data is located. */
GX_UBYTE *root_address = 0x60000000;

status = gx_binres_language_count_get(root_address, &language_count);

/* If status is GX_SUCCESS, the language table was successfully loaded. */

```

### See Also

- gx_binres_language_info_load

## gx_binres_language_info_load

Load the language table information

### Prototype

```C
UINT gx_binres_language_info_load(
    GX_UBYTE *root_address, 
    GX_LANGUAGE_HEADER *put_info);
```

### Description

This service parses a binary resource data blob to populate an array of **GX_LANGUAGE_HEADER** structures, informing the application of the language names and string table size of each language contained within the binary data. The application should first call gx_binres_language_count_get() to determine the number of languages within the binary data, and ensure that the put_info pointer passed to this function points to an array of language_count **GX_LANGUAGE_HEADER** structures.

This service is used by application to determine at runtime the content of a binary resource data chunk.

The **GX_LANGUAGE_HEADER** structure is defined as:

```c
typedef struct GX_LANGUAGE_HEADER_STRUCT{
    USHORT gx_language_header_magic_number;
    USHORT gx_language_header_index;
    UCHAR gx_language_header_name[GX_LANGUAGE_HEADER_NAME_SIZE];
    ULONG  gx_language_header_data_size;
} GX_LANGUAGE_HEADER;
```

- The *magic_number* field is used for internal validation of the binary resource data format.
- The *header_index* field indicates the order in which the languages are defined in the binary data.
- The *header_name* field contains the language name.
- The *header_data_size* field contains the data size of the language string table.

### Parameters

- *root_address*: Address of the binary resource data in memory.
- *put_info*: Pointer to the array of **GX_LANGUAGE_HEADER** structures.

### Return Values

- **GX_SUCCESS** (0x00) Successful.
- **GX_INVALID_FORMAT** (0x24) Invalid binary resource.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_LANGUAGE_HEADER language_info[4];

/* Specify address that binary resource data is located. */
GX_UBYTE    *root_address = 0x60000000;

status = gx_binres_language_info_load(root_address,
    language_info);
/* If status is GX_SUCCESS, the language table was successfully loaded. */

```

### See Also

- gx_binres_language_count_get

## gx_binres_language_table_load

Loads the language table resource (deprecated)

### Prototype

```C
UINT gx_binres_language_table_load(
    GX_UBYTE *root_address, 
    GX_UBYTE **returned_language_table);
```

### Description

This deprecated API allows applications to load string table data from older (prior to release 5.6) binary resource data files.

New applications should use gx_binres_language_table_load_ext().

This service builds up a language table structure containing pointers to table resources, the generated data structures point to the resource data "in place", it does not copy the resource data. The resource data must be placed in a general access memory location, and the base address of this memory location is passed to this API.

This service requires a runtime allocated memory block sufficient in size to hold the language table structure, and therefore the gx_system_memory_allocator_set API must be invoked once before this service is requested.

The returned language table defines one or more string table(s), each string table containing pointers to string resources in resource data memory.

### Parameters

- *root_address*: Address of the binary resource data in memory.
- *returned _language_table*: Pointer to the loaded language table.

### Return Values

- **GX_SUCCESS** (0x00) Successful.
- **GX_INVALID_FORMAT** (0x24) Invalid binary resource.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_SYSTEM_MEMPRY_ERROR (0x30) Memory allocator or free function is not defined.

### Allowed From

Initialization and threads

### Example

```C
GX_UBYTE ***language_table = GX_NULL;

/* Specify address that binary resource data is located. */
GX_UBYTE *root_address = 0x60000000;

status = gx_binres_language_table_load(root_address, &language_table);

/* If status is GX_SUCCESS, the language table was successfully loaded. */

```

### See Also

- gx_binres_language_table_load_ext

## gx_binres_language_table_load_ext

Load language table resource

### Prototype

```C
UINT gx_binres_language_table_load_ext(
    GX_UBYTE *root_address, 
    GX_STRING **returned_language_table);
```

### Description

This service builds up a language table structure containing pointers to table resources, the generated data structures point to the resource data "in place", it does not copy the resource data. The resource data must be placed in a general access memory location, and the base address of this memory location is passed to this API.

This service requires a runtime allocated memory block sufficient in size to hold the language table structure, and therefore the gx_system_memory_allocator_set API must be invoked once before this service is requested.

The returned language table defines one or more string table(s), each string table containing pointers to string resources in resource data memory.

### Parameters

- *root_address*: Address of the binary resource data in memory.
- *returned _language_table*: Pointer to the loaded language table.

### Return Values

- **GX_SUCCESS** (0x00) Successful.
- **GX_INVALID_FORMAT** (0x24) Invalid binary resource.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_SYSTEM_MEMPRY_ERROR (0x30) Memory allocator or free function is not defined.

### Allowed From

Initialization and threads

### Example

```C
GX_STRING **language_table = GX_NULL;

/* Specify address that binary resource data is located. */
GX_UBYTE *root_address = 0x60000000;

status = gx_binres_language_table_load_ext(root_address, &language_table);

/* If status is GX_SUCCESS, the language table was successfully loaded. */

```

### See Also

- gx_binres_theme_load

## gx_binres_pixelmap_load

Load pixelmap from the standalone binary data to the specified buffer.

### Prototype

```C
UINT gx_binres_pixelmap_load(GX_UBYTE *root_address, UINT map_index, GX_PIXELMAP *pixelmap);
```

### Description

This service loads a pixelmap from standalone binary data to the specified pixelmap structure.
It reads the pixelmap properties directly from the binary data and utilizes pointers to the pixelmap data within the binary to construct the pixelmap.

The standalone binary data differs from the original version of the GUIX binary data, which typically holds a complete theme table or language table information. In contrast, the standalone binary data comprises one or more standalone resources. To learn more about how to generate the standalone binary file, refer to the GUIX Studio documentation.

### Parameters

- *root_address*: The root address of the binary resource data.
- *map_index*: The 0-based index used to identify the pixelmap to be loaded.
- *pixelmap*: The pixelmap memory for loading the pixelmap.


### Return Values

- **GX_SUCCESS** (0x00) Successful
- **GX_INVALID_FORMAT** (0x24) Invalid binary resource
- **GX_PTR_ERROR** (0x07) Invalid pointer
- **GX_NOT_FOUND** (0x09) Invalid pixelmap index

### Allowed From

Initialization and threads

### Example

```C
#define FONT_BUFFER_SIZE 1024

/* Specify address that binary resource data is located. */
GX_UBYTE *root_address = 0x60000000;
GX_PIXELMAP pixelmap;

status = gx_binres_pixelmap_load(root_address, 0, &pixelmap);

/* If status is GX_SUCCESS, the first pixelmap in the binary data was successfully loaded. */

```

### See Also

- gx_binres_font_load

## gx_binres_theme_load

Load the theme resource

### Prototype

```C
UINT gx_binres_theme_load(
    GX_UBYTE *root_address, 
    INT theme_id, GX_THEME **returned_theme);
```

### Description

This service builds up a GX_THEME structure containing pointers to the resource tables for the requested theme. The generated data structures point to the resource data "in place"; it does not copy the resource data. So the resource data must be placed in a general access memory location, and the base address of this memory location is passed to this API.

This service requires a runtime allocated memory block sufficient in size to hold the theme table structure, and therefore the gx_system_memory_allocator_set API must be invoked once before this service is requested.

### Parameters

- *root_address*: Address of the binary resource data in memory.
- *theme_id*: The identifier of the theme.
- *returned_theme*: Pointer to the loaded theme.

### Return Values

- **GX_SUCCESS** (0x00) Successful.
- **GX_INVALID_FORMAT** (0x24) Invalid binary resource.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_VALUE (0x22) Invalid theme ID.
- GX_SYSTEM_MEMORY_ERROR (0x30) Memory allocator or free function is not defined.

### Allowed From

Initialization and threads

### Example

```C
GX_CHAR *theme = GX_NULL;
GX_UBYTE *root_address = 0x60000000;
INT theme_id = 0;

status = gx_binres_theme_load(root_address, theme_id, &theme);

/* If status is GX_SUCCESS, the theme was successfully loaded. */
```

### See Also

- gx_binres_language_table_read

## gx_brush_default
Set the default brush

### Prototype

```C
UINT gx_brush_default(GX_BRUSH *brush);
```

### Description

This service sets the brush for the current context to the system default value.

### Parameters
- *brush*: Pointer to the brush control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful brush definition.
- GX_PTR_ERROR (0x07) Invalid brush pointer.

### Allowed From

Initialization and threads

### Example

```C
/*Reset the brush to its default value. */
status = gx_brush_default(&my_brush);

/* If status is GX_SUCCESS the brush was successfully reset to its default value. */

```

### See Also

- gx_brush_define


## gx_brush_define

Define the brush

### Prototype

```C
UINT gx_brush_define(
    GX_BRUSH *brush, 
    GX_COLOR line_color, 
    GX_COLOR fill_color, 
    UINT style);
```

### Description

This service defines a brush with the specified line color, fill color and style.

### Parameters

- *brush*: Pointer to the brush control block.
- *line_color*: Color of the brush line. **Appendix A** contains pre-defined colors. Note that the application may add custom colors as well.
- *fill_color*: Color of the brush fill. **Appendix A** contains pre-defined colors. Note that the application may add custom colors as well.
- *style*: Brush style. **Appendix D** describes the supported brush styles. Brush styles can be combined into one variable using bitwise OR operation.

### Return Values

- **GX_SUCCESS** (0x00) Successful brush definition.
- GX_PTR_ERROR (0x07) Invalid brush pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Define a brush. */
status = gx_brush_define(&my_brush, GX_COLOR_BLACK, GX_COLOR_BLACK,
                         GX_STYLE_BORDER_NONE);

/* If status is GX_SUCCESS the brush was successfully created. */
```

### See Also

- gx_brush_default

## gx_button_background_draw

 Draw the button background

### Prototype

```C
VOID gx_button_background_draw(GX_BUTTON *button);
```

### Description

This service draws the button background. This function is normally called internally by the gx_button_draw function, but is exposed to the application to assist in writing custom drawing functions.

### Parameters

- *button*: Pointer to the button control block.

### Return Values

None

### Allowed From

Threads

### Example

```C
VOID custom_button_draw(GX_BUTTON *button)
{
    /* Draw button background. */
    gx_button_background_draw(button);

    /* Add custom drawing here. */

    /* Draw child widgets. */
    gx_widget_children_draw((GX_WIDGET *)button);
}
```

### See Also

- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_icon_button_create
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_radio_button_create
- gx_radio_button_draw
- gx_text_button_create
- gx_text_button_color_set
- gx_text_button_draw

## gx_button_create

Create a button

### Prototype

```C
UINT gx_button_create(
    GX_BUTTON *button, 
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent, 
    ULONG style, 
    USHORT button_id, 
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a button as specified and associates the button with the supplied parent widget.

### Parameters

- *button*: Pointer to the button control block.
- *name*: Logical name of the button.
- *parent*: Pointer to the parent widget of the button.
- *style*: Button style. **Appendix D** contains pre-defined general styles for all widgets as well as widget specific styles.
- *button_id*: Application-defined ID of the button.
- *size*: Size of the button.

### Return Values

- **GX_SUCCESS** (0x00) Successful button creation.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.

### Allowed From

Initialization and threads

### Example

```C
GX_BUTTON my_top_button;

/* Create a stop button. */
status = gx_button_create(&my_stop_button, "my stop button",
                            &my_parent_window,
                            GX_STYLE_BUTTON_TOGGLE,
                            MY_STOP_BUTTON_ID, &size);

/* If status is GX_SUCCESS the stop button was successfully created. */
```

### See Also

- gx_button_background_draw
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_radio_button_create
- gx_radio_button_draw
- gx_icon_button_create
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_text_button_create
- gx_text_button_color_set
- gx_text_button_draw

## gx_button_deselect


Deselect a button

### Prototype

```C
UINT gx_button_deselect(
    GX_BUTTON *button, 
    GX_BOOL gen_event);
```

### Description

This service deselects the specified button and generate a signal event depending on button styles.

| Button Style              | Signal                     |
|---------------------------|----------------------------|
| None                      | GX_EVENT_CLICKED         |
| GX_STYLE_BUTTON_RADIO  | GX_EVENT_RADIO_DESELECT |
| GX_STYLE_BUTTON_TOGGLE | GX_EVENT_TOGGLE_OFF     |

### Parameters

- *button*: Pointer to button control block.
- *gen_event*: If **GX_TRUE**, the button will generate a **GX_EVENT_CLICKED**, **GX_EVENT_DESELECT**, or **GX_EVENT_TOGGLE_OFFSET** event depending on the button style. If **GX_FALSE**, the button will not generate any higher level event even if it would normally do so.

### Return Values

- **GX_SUCCESS** (0x00) Successful button deselect.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Deselect button. */
status = gx_button_deselect(&my_stop_button, GX_TRUE);

/* If status is GX_SUCCESS the stop button was successfully deselected. */
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_radio_button_create
- gx_radio_button_draw
- gx_icon_button_create
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_text_button_create
- gx_text_button_color_set
- gx_text_button_draw

## gx_button_draw

Draw a button

### Prototype

```C
VOID gx_button_draw(GX_BUTTON *button);
```

### Description

This service draws the specified button. This function is normally called internally by the GUIX canvas refresh mechanism, but is exposed to the application to assist with implementing custom drawing functions for custom button widgets.

### Parameters

- *button*: Pointer to the button control block.

### Return Values

None

### Allowed From

Threads

### Example

```C
/* Write a custom button draw function. */
VOID custom_button_draw(GX_BUTTON *button)
{
    /* Call default button draw. */
    gx_button_draw(button);

    /* Add custom drawing here. */
}

```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_event_process
- gx_button_select
- gx_radio_button_create
- gx_radio_button_draw
- gx_icon_button_create
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_text_button_create
- gx_text_button_color_set
- gx_text_button_draw

## gx_button_event_process

Process a button event

### Prototype

```C
UINT gx_button_event_process(
    GX_BUTTON *button, 
    GX_EVENT *event);
```

### Description

This service processes an event for the specified button.

### Parameters

- *button*: Pointer to the button control block.
- *event_ptr*: Pointer to the event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful button event process.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Threads

### Example

```C
/* Call generic button event processing as part of custom event processing function. */

UINT custom_button_event_process(GX_BUTTON *button,
                                 GX_EVENT *event)
{
    UINT status = GX_SUCCESS;

    switch(event->gx_event_type)
    {
    case xyz:
        /* Insert custom event handling here. */
        break;
    default:
        /* Pass all other events to the default button event processing. */
        status = gx_button_event_process(button, event);
        break;
    }
    return status;
}
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_select
- gx_radio_button_create
- gx_radio_button_draw
- gx_icon_button_create
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_text_button_create
- gx_text_button_color_set
- gx_text_button_draw

## gx_button_select

Select a button

### Prototype

```C
UINT gx_button_select(GX_BUTTON *button);
```

### Description

This service selects the specified button and generates a signal event depending on button styles.

Deselects the siblings for a radio button group.

| Button Style                       | Signal                   |
|------------------------------------|--------------------------|
| GX_STYLE_BUTTON_RADIO           | GX_EVENT_RADIO_SELECT |
| GX_STYLE_BUTTON_EVENT_ON_PUSH | GX_EVENT_CLICKED       |
| GX_STYLE_BUTTON_TOGGLE          | GX_EVENT_TOGGLE_ON    |


### Parameters

- *button*: Pointer to the button control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful button select.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Select button. */
status = gx_button_select(&my_stop_button);

/* If status is GX_SUCCESS the stop button was successfully selected. */
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_radio_button_create
- gx_radio_button_draw
- gx_icon_button_create
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_text_button_create
- gx_text_button_color_set
- gx_text_button_draw

## gx_canvas_aligned_text_draw

Draw text with specified alignment style

### Prototype

```C
UINT  _gx_canvas_aligned_text_draw(
    GX_CONST GX_STRING *string,
    GX_RECTANGLE *rectangle,
    ULONG alignment);
```

### Description

This service draws text on the canvas with specified alignment style.

### Parameters

- *string*: Pointer to string to draw.
- *rectangle*: Drawing area.
- *alignment*: Alignment style. It can be one of the following values:
    - GX_STYLE_TEXT_LEFT
    - GX_STYLE_TEXT_RIGHT
    - GX_STYLE_TEXT_CENTER

### Return Values

- **GX_SUCCESS** (0x00) Successful text draw.
- **GX_FAILURE** (0x1E) Failed text draw.
- **GX_INVALID_FONT** (0x16) Brush font is not set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_CONTEXT (0x06) No open drawing context.
- GX_INVALID_STRING_LENGTH (0x34) Invalid string length.

### Allowed From

Initialization and threads

### Example

```C
GX_RECTANGLE rectangle;
GX_STRING string;

/* Define text drawing area. */
gx_utility_rectangle_define(&rectangle, 0, 0, 100, 100);

/* Define string. */
string.gx_string_ptr = "example";
string.gx_string_length = sizeof("example") - 1;

/* Draw string with center alignment. */
status = gx_canvas_aligned_text_draw(&string, &rectangle, GX_STYLE_TEXT_CENTER);

/* If status is GX_SUCCESS the string has been drawn to the center of the specified rectangle. */
```

### See Also

- gx_canvas_arc_draw
- gx_canvas_block_move
- gx_canvas_circle_draw
- gx_canvas_ellipse_draw
- gx_canvas_line_draw
- gx_canvas_pie_draw
- gx_canvas_pixelmap_draw
- gx_canvas_pixelmap_tile
- gx_canvas_polygon_draw
- gx_canvas_rectangle_draw
- gx_canvas_rotated_text_draw
- gx_canvas_rotated_text_draw_ext
- gx_canvas_text_draw
- gx_canvas_text_draw_ext

## gx_canvas_alpha_set

Set the alpha-blend value for the canvas

### Prototype

```C
UINT gx_canvas_alpha_set(
    GX_CANVAS *canvas, 
    GX_UBYTE alpha);
```

### Description

This service sets the alpha-blend value for the specified canvas. Canvas alpha values can range from 0 (transparent) to 255 (fully opaque).

Blending overlay canvases requires either hardware graphics layer support, or software support via the creation of a composite canvas.

Hardware support for canvas blending is enabled by invoking the gx_canvas_hardware_layer_bind() API prior to setting the canvas alpha value. When a canvas is bound to a hardware graphics layer, calling the gx_canvas_alpha_set() API directly invokes the hardware graphics layer blending services.

To utilize software support for canvas blending, the application must create a canvas with GX_CANVAS_COMPOSITE style, into which all other managed canvases are composited prior to final display. Software support for canvas blending is only provided when running with a display driver of 16-bpp or higher color depth.

### Parameters

- *canvas*: Pointer to the canvas control block.
- *alpha*: Alpha-blend value. Must be in the range of  0 (transparent) to 255 (opaque) inclusive.

### Return Values

- **GX_SUCCESS** (0x00) Successful alpha-blend value set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_CANVAS (0x20) Invalid canvas.

### Allowed From

Initialization and threads

### Example

```C
/* Set the alpha-blend value of "my_canvas". */
status = gx_canvas_alpha_set(&my_canvas, GX_ALPHA_VALUE_OPAQUE);

/* If status is GX_SUCCESS the alpha-blend value was successfully set. */
```

### See Also

- gx_canvas_create
- gx_canvas_drawing_complete
- gx_canvas_drawing_initiate
- gx_canvas_offset_set
- gx_canvas_shift
- gx_canvas_hardware_layer_bind
- gx_canvas_show
- gx_canvas_hide

## gx_canvas_arc_draw

Draw an arc

### Prototype

```C
UINT gx_canvas_arc_draw(
    INT xcenter, 
    INT ycenter, 
    UINT r,
    INT start_angle, 
    INT end_angle);
```

### Description

This service draws a circle arc on the canvas using the current brush. The circle arc is clipped to the canvas invalid region. This service requires GX_ARC_DRAWING_SUPPORT to be defined.

### Parameters

- *xcenter*: x-position of center of the circle arc.
- *ycenter*: y-position of center of the circle arc.
- *r*: Radius of the circle arc.
- *start_angle*: Starting angle of the circle arc.
- *end_angle*: Ending angle of the circle arc.

### Return Values

- **GX_SUCCESS** (0x00) Successful arc draw.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_INVALID_VALUE (0x22) Invalid value.
- GX_INVALID_CONTEXT (0x06) No open drawing context.

### Allowed From

Threads

### Example

```C
/* Draw a circle arc from 0 degree to 90 degree in clockwise. */
status = gx_canvas_arc_draw(100, 100, 50, 0, 90);

/* If status is GX_SUCCESS the arc has been drawn on "my_canvas". */
```

### See Also

- gx_canvas_block_move
- gx_canvas_circle_draw
- gx_display_create
- gx_canvas_ellipse_draw
- gx_canvas_line_draw
- gx_canvas_pie_draw
- gx_canvas_pixelmap_draw
- gx_canvas_pixelmap_tile
- gx_canvas_polygon_draw
- gx_canvas_rectangle_draw
- gx_canvas_text_draw

## gx_canvas_block_move

Move block of canvas pixels

### Prototype

```C
UINT gx_canvas_block_move(
    GX_RECTANGLE *block,
    GX_VALUE x_shift, 
    GX_VALUE y_shift, 
    GX_RECTANGLE *dirty);
```

### Description

This service moves a block of canvas pixel data in the direction specified. This service is used internally by GUIX to accomplish fast scrolling, but may also be used by the application software.

### Parameters

- *block*: Coordinates of area to move.
- *x_shift*: Number of pixels to shift on the x-axis.
- *y_shift*: Number of pixels to shift on the y-axis.
- *dirty*: If the block move is successful, this function returns the portion of the source rectangle that is still dirty to the caller in this parameter.

### Return Values

- **GX_SUCCESS** (0x00) Successful block move.
- **GX_FAILURE** (0x10) Failed block move.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Threads

### Example

```C
GX_RECTANGLE invalid;
GX_RECTANGLE move;

/* Define 100 x 100 pixel rectangle. */
gx_utility_rectangle_define(&move, 0, 0, 99, 99);

/* Move this rectangle 10 pixels to the right. */
status = gx_canvas_block_move(&move, 10, 0, &invalid);

/* If status is GX_SUCCESS, then 'invalid' marks the area that needs to be redrawn after the block move. */

```

### See Also

- gx_canvas_arc_draw
- gx_canvas_circle_draw
- gx_display_create
- gx_canvas_ellipse_draw
- gx_canvas_line_draw
- gx_canvas_pie_draw
- gx_canvas_pixelmap_draw
- gx_canvas_pixelmap_tile
- gx_canvas_polygon_draw
- gx_canvas_rectangle_draw
- gx_canvas_text_draw

## gx_canvas_circle_draw

Draws a circle

### Prototype

```C
UINT gx_canvas_circle_draw(
    INT xcenter, 
    INT ycenter, 
    UINT r);
```

### Description

This service draws a circle on the canvas using the current brush. The circle is clipped to the canvas invalid region. This service requires GX_ARC_DRAWING_SUPPORT to be defined.

### Parameters

- *xcenter*: x-coord of center of the circle.
- *ycenter*: y-coord of center of the circle.
- *r*: Radius of the circle.

### Return Values

- **GX_SUCCESS** (0x00) Successful circle draw.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_INVALID_VALUE (0x22) Invalid circle radius.
- GX_INVALID_CONTEXT (0x06) No open drawing context.

### Allowed From

Threads

### Example

```C

/* Draw a circle of radius 10 centered at (100, 100). */
status = gx_canvas_circle_draw(100, 100, 50);

/* If status is GX_SUCCESS the circle has been drawn on "my_canvas". */

```

### See Also

- gx_canvas_arc_draw
- gx_canvas_block_move
- gx_display_create
- gx_canvas_ellipse_draw
- gx_canvas_line_draw
- gx_canvas_pie_draw
- gx_canvas_pixelmap_draw
- gx_canvas_pixelmap_tile
- gx_canvas_polygon_draw
- gx_canvas_rectangle_draw
- gx_canvas_text_draw


## gx_canvas_create

Create a canvas

### Prototype

```C
UINT gx_canvas_create(
    GX_CANVAS *canvas, 
    GX_CONST GX_CHAR *name,
    GX_DISPLAY *display, 
    UINT type, 
    UINT width, 
    UINT height,
    GX_COLOR *memory_area, 
    ULONG memory_size);
```

### Description

This service creates the canvas with the specified properties and associated memory.

### Parameters

- *canvas*: Pointer to the canvas control block.
- *name*: Logical name for the canvas.
- *display*: Pointer to the previously-created display.
- *type*: Type of canvas. The canvas types include:
  - **GX_CANVAS_SIMPLE**: A memory canvas which is used to off-screen drawing.
  - **GX_CANVAS_MANAGED**: A canvas which automatically flushed to the active display, either as part of the composite building process or as part of the buffer toggle operation for single-canvas architectures.
  - **GX_CANVAS_VISIBLE**: This flag can be used to turn on and off a canvas, without losing the canvas drawing contents.
  - **GX_CANVAS_MODIFIED**: Reserved for future use.
  - **GX_CANVAS_COMPOSITE**: This flag is used by the application when configuring a multiple-canvas system which will composite multiple managed canvases into the composite canvas, and the composite is the driven to the hardware frame buffer.
- *width*: Width in pixels.
- *height*: Height in pixels.
- *memory_area*: Memory area for canvas. This value can by **GX_NULL** at the time of canvas creation and later initialized using the ***gx_canvas_memory_define*** function.
- *memory_size*: Size of memory area in bytes, or 0 if the canvas memory will be defined after the canvas is created.

### Return Values

- **GX_SUCCESS** (0x00) Successful canvas create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_CANVAS_SIZE (0x1C) Invalid canvas control block size.
- GX_INVALID_SIZE (0x19) Insufficient memory size.
- GX_INVALID_TYPE (0x1B) Invalid canvas type.

### Allowed From

Initialization and threads

### Example

```C
/* Define global canvas memory area. */
GX_COLOR my_canvas_memory[272*480];

…

/* Create "my_canvas". */
status = gx_canvas_create(&my_canvas, "my canvas", &my_display,
                    (GX_CANVAS_MANAGED | GX_CANVAS_VISIBLE),
                    272, 480,
                    my_canvas_memory,
                    sizeof(default_canvas_memory));

/* If status is GX_SUCCESS the 272 x 480 canvas was successfully created. */
```

### See Also

- gx_canvas_delete
- gx_canvas_hardware_layer_bind
- gx_canvas_memory_define

## gx_canvas_delete

Delete canvas

### Prototype

```C
UINT gx_canvas_delete(GX_CANVAS *canvas);
```

### Description

This service deletes the canvas. The canvas is removed from the internal linked list of canvas maintained by GUIX.

### Parameters

- *canvas*: Pointer to the canvas control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful canvas create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_CANVAS (0x20) Invalid canvas.

### Allowed From

Initialization and threads

### Example

```C
/* Delete "my_canvas". */
status = gx_canvas_delete (&my_canvas);

/* If status is GX_SUCCESS my_canvas was deleted. */
```

### See Also

- gx_canvas_alpha_set
- gx_canvas_drawing_complete
- gx_canvas_create
- gx_canvas_drawing_initiate
- gx_canvas_offset_set
- gx_canvas_shift

## gx_canvas_drawing_complete

Complete canvas drawing

### Prototype

```C
UINT gx_canvas_drawing_complete(
    GX_CANVAS *canvas, 
    GX_BOOL flush);
```

### Description

This service lets GUIX know the application's drawing on the specified canvas is complete.

The application can use this service to force immediate drawing to a canvas. This flushes the canvas to the visible frame buffer and/or triggers a bugger toggle operation, depending on system memory architecture.

This service should only be called by the application to close a drawing sequence begun with gx_canvas_drawing_initiate().

### Parameters

- *canvas*: Pointer to canvas control block.
- *flush*: If **GX_TRUE**, canvas changes are flushed to the display.

### Return Values

- **GX_SUCCESS** (0x00) Successful drawing completion.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Complete drawing on "my_canvas" and flush to display. */
status = gx_canvas_drawing_complete(&my_canvas, GX_TRUE);

/* If status is GX_SUCCESS the canvas drawing was successfully completed. */
```

### See Also

- gx_canvas_alpha_set
- gx_canvas_create
- gx_canvas_drawing_initiate
- gx_canvas_offset_set
- gx_canvas_shift

## gx_canvas_drawing_initiate

Initiate canvas drawing

### Prototype

```C
UINT gx_canvas_drawing_initiate(
    GX_CANVAS *canvas,
    GX_WIDGET *who, 
    GX_RECTANGLE *dirty_area);
```

### Description

This service initiates drawing on the specified canvas. This service is called internally as part of the deferred drawing operation performed automatically by GUIX when a canvas needs to be update. However, the application is allowed bypass the GUIX deferred drawing algorithm and perform immediate and direct drawing on a canvas by first calling gx_canvas_drawing_initiate, then calling the desired drawing functions, then calling gx_canvas_drawing_complete().

### Parameters

- *canvas*: Pointer to canvas control block.
- *who*: Pointer to widget control block of the caller. This parameter is used to initialize the drawing clipping and view parameters for subsequent drawing operations.
- *dirty_area*: Area to draw within. This parameter is passed by the caller to indicate the area to which the caller wants all drawing operations clipped. This is usually the area previously marked as dirty, but the caller is free to expand or contract the clipping area.

### Return Values

- **GX_SUCCESS** (0x00) Successful drawing initiation.
- **GX_DRAW_NESTING_EXCEEDED** (0x05) Exceed maximum nesting count.
- **GX_NO_VIEW** (0x03) No viewports for the caller.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_CANVAS (0x20) Invalid canvas.

### Allowed From

Initialization and threads

### Example

```C
/* Initiate drawing on "my_canvas", my_widget.gx_widget_size
specify the area the application wants GUIX to redraw. */
status = gx_canvas_drawing_initiate(&my_canvas, &my_widget, &my_widget.gx_widget_size);

/* If status is GX_SUCCESS the canvas drawing was successfully initiated. */
```

### See Also

- gx_canvas_alpha_set
- gx_canvas_create
- gx_canvas_drawing_complete
- gx_canvas_offset_set
- gx_canvas_shift

## gx_canvas_ellipse_draw

Draw ellipse

### Prototype

```C
UINT gx_canvas_ellipse_draw(
    INT xcenter, 
    INT ycenter, 
    INT a, 
    dINT b);
```

### Description

This service draws an ellipse on the canvas using the current brush. The ellipse is clipped to the canvas invalid region. This service requires GX_ARC_DRAWING_SUPPORT to be defined.

### Parameters

- *xcenter*: x-coord of the center of the ellipse.
- *ycenter*: y-coord of the center of the ellipse.
- *a*: Length of the semi-major axis.
- *b*: Length of the semi-minor axis.

### Return Values

- **GX_SUCCESS** (0x00) Successful circle draw.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_INVALID_VALUE (0x22) Invalid value.
- GX_INVALID_CONTEXT (0x06) No open drawing context.

### Allowed From

Threads

### Example

```C
/* Draw an ellipse of semi-major radius 100, semi-minor radius 50
   and centered at (200, 200). */
status = gx_canvas_ellipse_draw(200, 200, 100, 50);

/* If status is GX_SUCCESS the ellipse has been drawn on "my_canvas". */
```

### See Also

- gx_canvas_arc_draw
- gx_canvas_block_move
- gx_canvas_circle_draw
- gx_display_create
- gx_canvas_line_draw
- gx_canvas_pie_draw
- gx_canvas_pixelmap_draw
- gx_canvas_pixelmap_tile
- gx_canvas_polygon_draw
- gx_canvas_rectangle_draw
- gx_canvas_text_draw

## gx_canvas_hardware_layer_bind

Bind the canvas to the hardware graphics layer

### Prototype

```C
UINT gx_canvas_hardware_layer_bind(
    GX_CANVAS *canvas, 
    INT layer);
```

### Description

This service binds a GUIX drawing canvas to a hardware graphics layer. This service is only required for hardware devices supporting multiple hardware graphics layers.

Binding a canvas to a hardware graphics layer results in the gx_canvas_show(), gx_canvas_hide(), gx_canvas_alpha_set(), and gx_canvas_offset_set() APIs being implemented directly by hardware display driver services.

If the hardware display driver does not support multiple graphics layers, this service will fail returning GX_INVALID_DISPLAY.

### Parameters

- *canvas*: canvas to be implement in hardware.
- *layer*: hardware graphics layer.

### Return Values

- **GX_SUCCESS** (0x00) Successful binding.
- **GX_INVALID_DISPLAY** (0x1D) Display layer service is not defined.
- GX_PTR_ERROR (0x17) Invalid pointers.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_INVALID_CANVAS (0x20) Invalid canvas.
- GX_NOT_SUPPORTED (0x28) Not supported.

### Allowed From

Initialization and threads

### Example

```C
/* Binds the canvas to the hardware graphics layer 1. */
status = gx_canvas_hardware_layer_bind(&my_canvas, 1);

/* If status is GX_SUCCESS, the drawing canvas is bound to the hardware graphics. */
```

### See Also

- gx_canvas_create
- gx_canvas_memory_define

## gx_canvas_hide

Hide a canvas, making it invisible

### Prototype

```C
UINT gx_canvas_hide(GX_CANVAS *canvas);
```

### Description

This service hides a GUIX canvas. If the canvas has been bound to a hardware graphics layer using gx_canvas_hardware_layer_bind(), this service is implemented using hardware support.

### Parameters

- *canvas*: Canvas to be hidden.

### Return Values

- **GX_SUCCESS** (0x00) Successful hide.
- GX_INVALID_CANVAS (0x20) Invalid canvas.
- GX_PTR_ERROR (0x17) Invalid pointers.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.

### Allowed From

Initialization and threads

### Example

```C
/* Make my_canvas invisible. */
status = gx_canvas_hide(&my_canvas);

/* If status is GX_SUCCESS, the canvas has been hidden. */

```

### See Also

- gx_canvas_create
- gx_canvas_drawing_complete
- gx_canvas_drawing_initiate
- gx_canvas_offset_set
- gx_canvas_shift
- gx_canvas_hardware_layer_bind
- gx_canvas_show
- gx_canvas_hide

## gx_canvas_line_draw

Draws a line

### Prototype

```C
UINT gx_canvas_line_draw(
    GX_VALUE x_start, 
    GX_VALUE y_start,
    GX_VALUE x_end, 
    GX_VALUE y_end);
```

### Description

This service draws a line on the canvas using the current brush. The line is clipped to the canvas invalid region.

### Parameters

- *x_start*: Starting x-position of the line.
- *y_end*: Starting y-position of the line.
- *x_start*: Ending x-position of the line.
- *y_end*: Ending y-position of the line.

### Return Values

- **GX_SUCCESS** (0x00) Successful line draw.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_CONTEXT (0x06) No open drawing context.

### Allowed From

Threads

### Example

```C
/* Draw line on canvas. */
status = gx_canvas_line_draw(0, 1, 320, 480);

/* If status is GX_SUCCESS, the line has been drawn to canvas. */
```

### See Also

- gx_canvas_arc_draw
- gx_canvas_block_move
- gx_canvas_circle_draw
- gx_display_create
- gx_canvas_ellipse_draw
- gx_canvas_pie_draw
- gx_canvas_pixelmap_draw
- gx_canvas_pixelmap_tile
- gx_canvas_polygon_draw
- gx_canvas_rectangle_draw
- gx_canvas_text_draw

## gx_canvas_memory_define

Define the canvas memory

### Prototype

```C
UINT gx_canvas_memory_define(
    GX_CANVAS *canvas, 
    GX_COLOR *memory,
    ULONG memsize);
```

### Description

This service can be used to assign the canvas memory address after the canvas has been created.

### Parameters

- *canvas*: Pointer to previously created canvas.
- *memory*: Canvas memory address.
- *memsize*: Size of the canvas memory block in bytes.

### Return Values

- **GX_SUCCESS** (0x00) Successful assignment.
- GX_INVALID_CANVAS (0x20) Invalid control block.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Threads

### Example

```C
/* Assign canvas memory block. */
status = gx_canvas_memory_define(canvas,
    (GX_COLOR *) DRAM_MEMORY,
    (640 * 480 * 2));

/* If status is GX_SUCCESS, the canvas memory pointer has be reassigned. */

```

### See Also

- gx_canvas_create
- gx_canvas_hardware_layer_bind

## gx_canvas_mouse_define

Define the mouse cursor image

### Prototype

```C
UINT gx_canvas_mouse_define(GX_CANVAS *canvas,
    GX_MOUSE_CURSOR_INFO *info);
```

### Description

This service defines mouse information for the specified canvas. This service requires GX_MOUSE_SUPPORT to be defined.

### Parameters

- *canvas*: Pointer to canvas control block.
- *info*: Pointer to mouse cursor information. **Appendix I** contains definition to GX_MOUSE_CURSOR_INFO structure.

### Return Values

- **GX_SUCCESS** (0x00) Successful mouse info set.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.

### Allowed From

Initialization and threads

### Example

```C
/* Set mouse cursor info. */
GX_MOUSE_CURSOR_INFO mouse_cursor;
mouse_cursor.gx_mouse_cursor_image_id = GX_PIXELMAP_ID_MOUSE;
mouse_cursor.gx_mouse_cursor_hotspot_x = 0;
mouse_cursor.gx_mouse_cursor_hotspot_y = 0;

status = gx_canvas_mouse_define(&my_canvas, &mouse_cursor);

/* If status is GX_SUCCESS the mouse info of "my_canvas" has been
set successfully. */

```

### See Also

- gx_canvas_mouse_show
- gx_canvas_mouse_hide

## gx_canvas_mouse_hide

Turn off the mouse cursor

### Prototype

```C
UINT gx_canvas_mouse_hide(GX_CANVAS *canvas);
```

### Description

This service makes the mouse cursor hidden from the specified canvas. This service requires GX_MOUSE_SUPPORT to be defined.

### Parameters

- *canvas*: Pointer to the canvas control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful mouse cursor hide.
- **GX_FAILURE** (0X10) Failed mouse cursor hide.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.

### Allowed From

Initialization and threads

### Example

```C
/* Hide the mouse cursor. */
status = gx_canvas_mouse_hide(&my_canvas);

/* If status is GX_SUCCESS the mouse cursor of "my_canvas" has been
hidden successfully. */

```

### See Also

- gx_canvas_mouse_show
- gx_canvas_mouse_define

## gx_canvas_mouse_show

Turn on the mouse cursor

### Prototype

```C
UINT gx_canvas_mouse_show(GX_CANVAS *canvas);
```

### Description

This service makes the mouse cursor visible for the specified canvas. This service requires GX_MOUSE_SUPPORT to be defined. The ***gx_canvas_mouse_define*** API should be invoked to define the mouse cursor image before this service is requested.

### Parameters

- *canvas*: Pointer to canvas control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful mouse info set.
- **GX_FAILURE** (0X10) Failed mouse cursor show.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.

### Allowed From

Initialization and threads

### Example

```C
/* Make mouse cursor hidden ". */
status = gx_canvas_mouse_show(&my_canvas);

/* If status is GX_SUCCESS the mouse of "my_canvas" has been
hidden successfully. */
```

### See Also

- gx_canvas_mouse_show
- gx_canvas_mouse_define

## gx_canvas_offset_set


Assign canvas x,y display offset

### Prototype

```C
UINT gx_canvas_offset_set(
    GX_CANVAS *canvas,
    GX_VALUE x, 
    GX_VALUE y);
```

### Description

This service assigns an x,y display offset for the specified canvas. This controls the position at which the canvas is composited into the visible frame buffer, and is often used when the canvas is smaller than the physical display.

If the canvas has been bound to a hardware graphics layer using the gx_canvas_hardware_layer_bind() API, the gx_canvas_offset_set service is implemented directly using hardware support.

### Parameters

- *canvas*: Pointer to canvas control block.
- *x*: X coordinate of offset.
- *y*: Y coordinate of offset.

### Return Values

- **GX_SUCCESS** (0x00) Successful assignment of offset.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_CANVAS (0x20) Invalid canvas.

### Allowed From

Initialization and threads

### Example

```C
/* Set display offset for "my_canvas". */
status = gx_canvas_offset_set(&my_canvas, 20, 30);

/* If status is GX_SUCCESS the canvas drawing is now offset from
position 20,30. */
```

### See Also

- gx_canvas_alpha_set
- gx_canvas_create
- gx_canvas_drawing_complete
- gx_canvas_initiate
- gx_canvas_shift
- gx_canvas_show
- gx_canvas_hide
- gx_canvas_hardware_layer_bind

## gx_canvas_pie_draw


Draw pie

### Prototype

```C
UINT gx_canvas_pie_draw(
    INT xcenter, 
    INT ycenter,
    UINT r, 
    INT start_angle, 
    INT end_angle);
```

### Description

This service draws a pie into the canvas using the current drawing context brush. The pie is clipped to the canvas invalid region. This service requires the configuration option GX_ARC_DRAWING_SUPPORT to be defined.

### Parameters

- *xcenter*: x-position of center of the pie.
- *ycenter*: y-position of center of the pie.
- *r*: Radius of the pie.
- *start_angle*: Starting angle of the pie.
- *end_angle*: Ending angle of the pie.

### Return Values

- **GX_SUCCESS** (0x00) Successful arc draw.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_INVALID_VALUE (0x22) Invalid value.
- GX_INVALID_CONTEXT (0x06) No open drawing context.

### Allowed From

Initialization and threads

### Example

```C
/* Draw a pie from 0 degree to 90 degree in clockwise. */
status = gx_canvas_pie_draw(100, 100, 50, 0, 90);

/* If status is GX_SUCCESS the pie has been drawn to canvas. */
```

### See Also

- gx_canvas_arc_draw
- gx_canvas_block_move
- gx_canvas_circle_draw
- gx_display_create
- gx_canvas_ellipse_draw
- gx_canvas_line_draw
- gx_canvas_pixelmap_draw
- gx_canvas_pixelmap_tile
- gx_canvas_polygon_draw
- gx_canvas_rectangle_draw
- gx_canvas_text_draw

## gx_canvas_pixel_draw


Draw pixel

### Prototype

```C
UINT gx_canvas_pixel_draw(GX_POINT position);
```

### Description

This service draws a pixel on the canvas using the line color of the current drawing context brush. If configuration option **GX_BRUSH_ALPHA_SUPPORT** is defined, blend the pixel with the otherwise, draw the pixel as fully opaque.

### Parameters
- *position*: x,y position of pixel to draw.

### Return Values

- **GX_SUCCESS** (0x00) Successful pixelmap draw.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_INVALID_CONTEXT (0x06) No open drawing context.

### Allowed From

Initialization and threads

### Example

```C
GX_POINT point; /* The x,y position you want to draw to. */
GX_RECTANGLE drawto; /* The rectangle bounding your drawing. */
GX_CANVAS *mycanvas; /* The canvas you want to draw to. */

/* Calculate 1x1 pixel drawing area. */
gx_utility_rectangle_define(&drawto,
    point.gx_point_x, point.gx_point_y,
    point.gx_point_x, point.gx_point_y);

/* Get my canvas. */
gx_widget_canvas_get(win, &mycanvas);

/* Open my canvas for drawing. */
gx_canvas_drawing_initiate(mycanvas, win, &drawto);

/* Setup my brush colors. Use any color ID in your resources. */
gx_context_line_color_set(GX_COLOR_ID_WINDOW_BORDER);

/* Draw a pixel. */
status = gx_canvas_pixel_draw(point);

/* Close the canvas. */
gx_canvas_drawing_complete(mycanvas, GX_TRUE);

/* If status is GX_SUCCESS, the pixel was successfully drawn to mycanvas. */
```

### See Also

- gx_canvas_block_move
- gx_canvas_pixelmap_tile
- gx_canvas_pixelmap_blend

## gx_canvas_pixelmap_blend

Blend pixelmap

### Prototype

```C
UINT gx_canvas_pixelmap_blend(
    GX_VALUE x_position,
    GX_VALUE y_position, 
    GX_PIXELMAP *pixelmap, 
    GX_UBYTE alpha);
```

### Description

This service blends a pixelmap with the canvas background. The blending ratio is specified by the caller. The alpha value can range from 0 (fully transparent) to 255 (fully opaque). The pixelmap may also include an internal alpha channel, which is combined with the incoming blending value. This service is only supported by display drivers running at 16-bpp color depth and higher.

### Parameters

- *x_start*: Starting x-position of the pixelmap.
- *y_end*: Starting y-position of the pixelmap.
- *pixelmap*: Pointer to pixelmap.

### Return Values

- **GX_SUCCESS** (0x00) Successful pixelmap draw.
- **GX_NOT_SUPPORTED** (0x28) Not supported.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_CONTEXT (0x06) No open drawing context.

### Allowed From

Initialization and threads

### Example

```C
/* Draw pixelmap on active canvas. */

GX_PIXELMAP *map;
gx_system_pixelmap_get(ID_MY_PIXELMAP, &map);

status = gx_canvas_pixelmap_blend(10, 20, map, 128);

/* If status is GX_SUCCESS the pixelmap has been blended onto the current canvas. */
```

### See Also

- gx_canvas_block_move
- gx_canvas_pixelmap_get
- gx_canvas_pixelmap_tile
- gx_canvas_pixelmap_draw

## gx_canvas_pixelmap_draw

Draw pixelmap

### Prototype

```C
UINT gx_canvas_pixelmap_draw(
    GX_VALUE x_position, 
    GX_VALUE y_position,
    GX_PIXELMAP *pixelmap);
```

### Description

This service draws a pixelmap on the canvas.

### Parameters

- *x_position*: Starting x-position of the pixelmap.
- *y_position*: Starting y-position of the pixelmap.
- *pixelmap*: Pointer to pixelmap.

### Return Values

- **GX_SUCCESS** (0x00) Successful pixelmap draw.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_CONTEXT (0x06) No open drawing context.

### Allowed From

Initialization and threads

### Example

```C
/* Draw pixelmap on canvas. */
status = gx_canvas_pixelmap_draw(10, 20, &my_pixelmap);

/* If status is GX_SUCCESS the pixelmap "my_pixelmap" has been drawn. */
```

### See Also

- gx_canvas_block_move
- gx_canvas_pixelmap_get
- gx_canvas_pixelmap_tile
- gx_canvas_pixelmap_blend

## gx_canvas_pixelmap_get

Get canvas pixelmap

### Prototype

```C
UINT gx_canvas_pixelmap_get(GX_PIXELMAP *pixelmap);
```

### Description

This service returns a GX_PIXELMAP structure pointing to the canvas data. The pixelmap format is set to the current display color format.

### Parameters

- *pixelmap*: Returned pixelmap.

### Return Values

- **GX_SUCCESS** (0x00) Successful pixelmap get.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Draw pixelmap on active canvas. */

GX_PIXELMAP *map;

status = gx_canvas_pixelmap_get(map);

/* If status is GX_SUCCESS the pixelmap has been retrieved. */
```

### See Also

- gx_canvas_pixelmap_blend
- gx_canvas_pixelmap_tile
- gx_canvas_pixelmap_draw

## gx_canvas_pixelmap_rotate


Draw rotated pixelmap

### Prototype

```C
UINT gx_canvas_pixelmap_rotate(
    GX_VALUE x_position, 
    GX_VALUE y_position,
    GX_PIXELMAP *pixelmap, 
    INT angle,
    INT rot_cx, 
    INT rot_cy);
```

### Description

This service rotates a pixelmap at the specified angle and renders the pixelmap to the canvas directly as the rotation is performed. This service differs from gx_utility_pixelmap_rotate in that the output of the rotation is directly rendered to the canvas memory, and the rotated pixelmap is not returned to the caller.

The advantage of this service over gx_utility_pixelmap_rotate is that no additional memory is required to hold the rotated pixelmap. The disadvantage is that the rotation code must be executed each time the pixelmap is drawn.

Clipping and viewport validation are enforced during rendering of the rotated pixelmap.

### Parameters

- *x_position*: Starting x-position of the pixelmap.
- *y_position*: Starting y-position of the pixelmap.
- *pixelmap*: Pointer to pixelmap.
- *angle*: Angle to rotate.
- *rot_cx*: X-coord of center of rotation. If this value is set to -1, the center of the image is used as the rotation center.
- *rot_cy*: Y-coord of center of rotation. If this value is set to -1, the center of the image is used as the center of rotation.

### Return Values

- **GX_SUCCESS** (0x00) Successful pixelmap draw.
- **GX_NOT_SUPPORTED** (0x28) Source pixelmap is compressed format, which is not supported.
- **GX_FAILURE** (0x10) Pixelmap draw or rotate driver is not provided.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_CONTEXT (0x06) No open drawing context.

### Allowed From

Initialization and threads

### Example

```C
/* Rotate "src_pixelmap" by 30 degree in clockwise direction and draw in on canvas. */
status = gx_canvas_pixelmap_rotate(10, 20, &my_pixelmap, 30, -1, -1);

/* If status is GX_SUCCESS the rotated pixelmap "my_pixelmap" has been drawn. */
```

### See Also

- gx_canvas_block_move
- gx_canvas_pixelmap_get
- gx_canvas_pixelmap_tile
- gx_canvas_pixelmap_blend

## gx_canvas_pixelmap_tile

Tile pixelmap

### Prototype

```C
UINT gx_canvas_pixelmap_tile(
    GX_RECTANGLE *fill,
    GX_PIXELMAP *pixelmap);
```

### Description

This service fills a rectangle within a canvas with the requested pixelmap.

### Parameters

- *fill*: Area to tile with pixelmap.
- *pixelmap*: Pointer to pixelmap.

### Return Values

- **GX_SUCCESS** (0x00) Successful pixelmap tile.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_CONTEXT (0x06) No open drawing context.
- GX_INVALID_VALUE (0x22) Invalid fill size.

### Allowed From

Initialization and threads

### Example

```C
/* Tile pixelmap on canvas. */
status = gx_canvas_pixelmap_tile(&tile_area, &my_pixelmap);

/* If status is GX_SUCCESS the pixelmap "my_pixelmap" has been tiled on canvas. */
```

### See Also

- gx_canvas_block_move
- gx_canvas_pixelmap_get
- gx_canvas_pixelmap_blend
- gx_canvas_pixelmap_draw

## gx_canvas_polygon_draw


Draw polygon

### Prototype

```C
UINT gx_canvas_polygon_draw(
    GX_POINT *point_array,
    INT number_of_points);
```

### Description

This service draws a polygon on the canvas using the current drawing context brush.

### Parameters

- *point_array*: Array of points of the polygon.
- *number_of_points*: Number of points of polygon.

### Return Values

- **GX_SUCCESS** (0x00) Successful polygon draw.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_CONTEXT (0x06) No open drawing context.

### Allowed From

Initialization and threads

### Example

```C
GX_POINT my_polygon[4] =
{
    { 208, 63 },
    { 274, 63 },
    { 274, 163 },
    { 208, 163 }
};

/* Draw polygon "my_polygon" on canvas. */
status = gx_canvas_polygon_draw(&my_polygon, 4);

/* If status is GX_SUCCESS the polygon "my_polygon" has been drawn. */
```

### See Also

- gx_canvas_arc_draw
- gx_canvas_block_move
- gx_canvas_circle_draw
- gx_display_create
- gx_canvas_ellipse_draw
- gx_canvas_line_draw
- gx_canvas_pie_draw
- gx_canvas_pixelmap_draw
- gx_canvas_pixelmap_tile
- gx_canvas_rectangle_draw
- gx_canvas_text_draw

## gx_canvas_rectangle_draw


Draw rectangle

### Prototype

```C
UINT gx_canvas_rectangle_draw(GX_RECTANGLE *rectangle);
```

### Description

This service draws a rectangle on the canvas.

### Parameters

- *rectangle*: Rectangle to draw.

### Return Values

- **GX_SUCCESS** (0x00) Successful rectangle draw.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_CONTEXT (0x06) No open drawing context.

### Allowed From

Initialization and threads

### Example

```C
/* Draw rectangle "my_rectangle" on canvas. */
status = gx_canvas_rectangle_draw(&my_rectangle);

/* If status is GX_SUCCESS the rectangle "my_rectangle" has been drawn. */
```

### See Also

- gx_canvas_arc_draw
- gx_canvas_block_move
- gx_canvas_circle_draw
- gx_display_create
- gx_canvas_ellipse_draw
- gx_canvas_line_draw
- gx_canvas_pie_draw
- gx_canvas_pixelmap_draw
- gx_canvas_pixelmap_tile
- gx_canvas_polygon_draw
- gx_canvas_text_draw

## gx_canvas_rotated_text_draw


Draw text rotated about a center point (deprecated)

### Prototype

```C
UINT gx_canvas_rotated_text_draw(
    const GX_CHAR *text,
    GX_VALUE xCenter, 
    GX_VALUE yCenter, 
    INT angle);
```

### Description

This API has been deprecated in favor or gx_canvas_rotated_text_draw_ext(). While still supported, new applications should not use this API and should instead use gx_canvas_rotated_text_draw_ext().

This service draws text to the canvas. The text is drawn rotated about the requested center point. The current drawing context font and drawing context line color is used to render the text.

This service uses the function gx_utility_string_to_alphamap to render the text string to a temporary 8bpp pixelmap containing only alpha value. The service then rotates the alphamap using the function gx_utility_pixelmap_rotate. After the final alphamap is rendered to the canvas, this service frees the temporary alphamap and associated memory.

Since a temporary alphamap is required to render rotated text, the application must configure the gx_system_memory_allocator by the calling gx_system_memory_allocator_set() API before attempting to draw rotated text.

This service should only be used to render rotated text "one time". If the same text string will be drawn multiple times at different locations or different rotation angles, it is more efficient to use the utility function gx_utility_string_to_alphamap() to create the text alphamap once, then use gx_utility_pixelmap_rotate multiple times to rotate the resulting alphamap repeatedly.

### Parameters

- *text*: Text string to be drawn.
- *xCenter*: Center position around which text will be rotated.
- *yCenter*: Center position around which text will be rotated.
- *angle*: The desired text rotation angle, in degrees.

### Return Values

- **GX_SUCCESS** (0x00) Successful text rendering.
- **GX_INVALID_STRING_LENGTH** (0x34) Invalid string length.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_SYSTEM_MEMORY_ERROR (0x30) Insufficient memory available or gx_system_memory_allocator has not been assigned.

### Allowed From

Initialization and threads

### Example

```C
void my_window_draw(GX_WINDOW *window)
{

    GX_VALUE xpos = 100;
    GX_VALUE ypos = 100;
    INT dynamic_count = 1234567;
    GX_CHAR dynamic_text[10];

    /* Call default window draw routine. */
    gx_window_draw(window);

    /* Set font. */
    gx_context_font_set(GX_FONT_ID_SMALL_BOLD);

    /* Convert int value to string. */
    gx_utility_ltoa(dynamic_count, dynamic_text, 20);

    /* Draw rotate text. */
    gx_canvas_rotated_text_draw(dynamic_text, xpos, ypos, 45);
}
```

### See Also

- gx_canvas_aligned_text_draw
- gx_canvas_alpha_set
- gx_canvas_drawing_complete
- gx_canvas_create
- gx_canvas_drawing_initiate
- gx_canvas_offset_set
- gx_canvas_shift
- gx_canvas_rotated_text_draw_ext
- gx_canvas_text_draw
- gx_canvas_text_draw_ext

## gx_canvas_rotated_text_draw_ext


Draw text rotated about a center point

### Prototype

```C
UINT gx_canvas_rotated_text_draw_ext(
    GX_CONST GX_STRING *text,
    GX_VALUE xCenter, 
    GX_VALUE yCenter, 
    INT angle);
```

### Description

This service draws text to the canvas. The text is drawn rotated about the requested center point. The current drawing context font and drawing context line color is used to render the text.

This service uses the function gx_utility_string_to_alphamap to render the text string to a temporary 8bpp pixelmap containing only alpha value. The service then rotates the alphamap using the function gx_utility_pixelmap_rotate. After the final alphamap is rendered to the canvas, this service frees the temporary alphamap and associated memory.

Since a temporary alphamap is required to render rotated text, the application must configure the gx_system_memory_allocator by the calling ***gx_system_memory_allocator_set*** API before attempting to draw rotated text.

This service should only be used to render rotated text "one time". If the same text string will be drawn multiple times at different locations or different rotation angles, it is more efficient to use the utility function gx_utility_string_to_alphamap() to create the text alphamap once, then use gx_utility_pixelmap_rotate multiple times to rotate the resulting alphamap repeatedly.

### Parameters

- *text*: Text string to be drawn.
- *xCenter*: Center position around which text will be rotated.
- *yCenter*: Center position around which text will be rotated.
- *angle*: The desired text rotation angle, in degrees.

### Return Values

- **GX_SUCCESS** (0x00) Successful text rendering.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_INVALID_CONTEXT (0x06) Invalid draw context.
- GX_SYSTEM_MEMORY_ERROR (0x30) Insufficient memory available or gx_system_memory_allocator has not been assigned.
- GX_INVALID_STRING_LENGTH (0x34) Invalid string length.

### Allowed From

Initialization and threads

### Example

```C
void my_window_draw(GX_WINDOW *window)
{

    GX_VALUE xpos = 100;
    GX_VALUE ypos = 100;
    INT dynamic_count = 1234567;
    GX_CHAR dynamic_text[10];
    GX_STRING string;

    /* Call default window draw routine. */
    gx_window_draw(window);

    /* Set font. */
    gx_context_font_set(GX_FONT_ID_SMALL_BOLD);

    /* Convert int value to string. */
    gx_utility_ltoa(dynamic_count, dynamic_text, 20);

    string.gx_string_ptr = dynamic_text;
    string.gx_string_length = strlen(dynamic_text);

    /* Draw rotate text. */
    gx_canvas_rotated_text_draw_ext(&string, xpos, ypos, 45);
}
```

### See Also

- gx_canvas_aligned_text_draw
- gx_canvas_alpha_set
- gx_canvas_drawing_complete
- gx_canvas_create
- gx_canvas_drawing_initiate
- gx_canvas_offset_set
- gx_canvas_shift
- gx_canvas_rotated_text_draw
- gx_canvas_rotated_text_draw_ext
- gx_canvas_text_draw
- gx_canvas_text_draw_ext

## gx_canvas_shift


Shift canvas by x,y

### Prototype

```C
UINT gx_canvas_shift(
    GX_CANVAS *canvas, 
    GX_VALUE x, GX_VALUE y);
```

### Description

This service shifts the specified canvas offset by the specified amount. This affects the position at which the canvas is rendered within the visible frame buffer.

### Parameters

- *canvas*: Pointer to canvas control block.
- *x*: Pixels to shift on the X axis.
- *y*: Pixels to shift on the Y axis.

### Return Values

- **GX_SUCCESS** (0x00) Successful canvas shift.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_CANVAS (0x20) Invalid canvas.

### Allowed From

Initialization and threads

### Example

```C
/* Shift canvas "my_canvas". */
status = gx_canvas_shift(&my_canvas, 10, 15);

/* If status is GX_SUCCESS the canvas has been shifted by 10 pixels
    on the X axis and 15 on the Y axis. */
```

### See Also

- gx_canvas_drawing_complete
- gx_canvas_initiate
- gx_canvas_alpha_set
- gx_canvas_create
- gx_canvas_offset_set

## gx_canvas_show


Make a canvas visible

### Prototype

```C
UINT gx_canvas_show(GX_CANVAS *canvas);
```

### Description

This service makes a canvas visible. If the canvas has been previously bound to a hardware graphics layer using the gx_canvas_hardware_layer_bind() API, the gx_canvas_show() service is implemented directly using hardware support.

### Parameters

- *canvas*: Pointer to canvas control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful assignment of offset.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_CANVAS (0x20) Invalid canvas.

### Allowed From

Initialization and threads

### Example

```C
/* Make this canvas visible. */
status = gx_canvas_show(&my_canvas);

/* If status is GX_SUCCESS the canvas drawing is now visible. */
```

### See Also

- gx_canvas_alpha_set
- gx_canvas_create
- gx_canvas_drawing_complete
- gx_canvas_initiate
- gx_canvas_shift
- gx_canvas_hide
- gx_canvas_hardware_layer_bind

## gx_canvas_text_draw

Draw text (deprecated)

### Prototype

```C
UINT gx_canvas_text_draw(
    GX_VALUE x_start, 
    GX_VALUE y_start,
    GX_CONST GX_CHAR *string, 
    INT length);
```

### Description

This service draws text on the canvas. This API, while still supported, is deprecated and new applications should instead use gx_canvas_text_draw_ext().

### Parameters

- *x_start*: Starting x-coordinate for text.
- *y_start*: Starting y-coordinate for text.
- *string*: Pointer to string to draw.
- *length*: If length >= 0, limits the number of characters drawn to length. If length < 0, the entire string until NULL terminator is drawn.

### Return Values

- **GX_SUCCESS** (0x00) Successful text draw.
- **GX_FAILURE** (0x1E) Failed text draw.
- **GX_INVALID_STRING_LENGTH** (0x34) Invalid string length.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_CONTEXT (0x06) No open drawing context.

### Allowed From

Initialization and threads

### Example

```C
/* Draw text "example" on current canvas". */
status = gx_canvas_text_draw(10, 20, "example", 7);

/* Draw all of a string of unknown length on the current canvas. */
status = gx_canvas_text_draw(10, 40, string_ptr, -1);

/* If status is GX_SUCCESS the text "example" has been drawn. */
```

### See Also

- gx_canvas_aligned_text_draw
- gx_canvas_arc_draw
- gx_canvas_block_move
- gx_canvas_circle_draw
- gx_canvas_ellipse_draw
- gx_canvas_line_draw
- gx_canvas_pie_draw
- gx_canvas_pixelmap_draw
- gx_canvas_pixelmap_tile
- gx_canvas_polygon_draw
- gx_canvas_rectangle_draw
- gx_canvas_rotated_text_draw
- gx_canvas_rotated_text_draw_ext
- gx_canvas_text_draw_ext

## gx_canvas_text_draw_ext


Draw text

### Prototype

```C
UINT gx_canvas_text_draw_ext(
    GX_VALUE x_start, 
    GX_VALUE y_start,
    GX_CONST GX_STRING *string);
```

### Description

This service draws text on the canvas.

### Parameters

- *x_start*: Starting x-coordinate for text.
- *y_start*: Starting y-coordinate for text.
- *string*: Pointer to string to draw.

### Return Values

- **GX_SUCCESS** (0x00) Successful text draw.
- **GX_FAILURE** (0x1E) Failed text draw.
- **GX_INVALID_FONT** (0x16) Invalid font.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_CONTEXT (0x06) No open drawing context.
- GX_INVALID_STRING_LENGTH (0x34) Invalid string length.

### Allowed From

Initialization and threads

### Example

```C
GX_STRING string;
string.gx_string_ptr = "example";
string.gx_string_length = 7;

/* Draw text "example" on current canvas". */
status = gx_canvas_text_draw_ext(10, 20, &string);

/* If status is GX_SUCCESS the text "example" has been drawn. */
```

### See Also

- gx_canvas_aligned_text_draw
- gx_canvas_arc_draw
- gx_canvas_block_move
- gx_canvas_circle_draw
- gx_canvas_ellipse_draw
- gx_canvas_line_draw
- gx_canvas_pie_draw
- gx_canvas_pixelmap_draw
- gx_canvas_pixelmap_tile
- gx_canvas_polygon_draw
- gx_canvas_rectangle_draw
- gx_canvas_rotated_text_draw
- gx_canvas_rotated_text_draw_ext
- gx_canvas_text_draw

## gx_checkbox_create


Create checkbox

### Prototype

```C
UINT gx_checkbox_create(
    GX_CHECKBOX *checkbox, 
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent, 
    GX_RESOURCE_ID text_id,
    ULONG style, 
    USHORT checkbox_id, 
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a checkbox widget with the specified properties. GX_CHECKBOX is derived from GX_TEXT_BUTTON, and all gx_text_button services may be used with GX_CHECKBOX widgets.

### Parameters

- *checkbox*: Pointer to checkbox control block name Logical name of checkbox widget.
- *parent*: Pointer to the parent widget.
- *text_id*: Resource ID of checkbox text.
- *style*: Style of checkbox. **Appendix D** contains pre-defined general styles for all widgets as well as widget specific styles.
- *checkbox_id*: Application-defined ID of checkbox.
- *size*: Dimensions of checkbox.

### Return Values

- **GX_SUCCESS** (0x00) Successful checkbox create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid size.

### Allowed From

Initialization and threads

### Example

```C
/* Create "my_checkbox". */
status = gx_checkbox_create(&my_checkbox, "my_checkbox",
    &my_parent,
    MY_CHECKBOX_TEXT_RESOURCE_ID, GX_STYLE_BORDER_RAISED,
    MY_CHECKBOX_ID,
    &size);

/* If status is GX_SUCCESS the checkbox "my_checkbox" has been created. */
```
### See Also

- gx_checkbox_draw
- gx_checkbox_event_process
- gx_checkbox_select

## gx_checkbox_draw

Draw checkbox

### Prototype

```C
VOID gx_checkbox_draw(GX_CHECKBOX *checkbox);
```

### Description

This service draws the specified checkbox. This function is normally called internally by the GUIX canvas refresh mechanism, but is exposed to the application to assist with implementing custom drawing functions for custom checkbox widgets.

### Parameters

- *checkbox*: Pointer to checkbox control block.

### Return Values

None

### Allowed From

Threads

### Example

```C
/* Write a custom checkbox draw function. */
VOID custom_checkbox_draw(GX_CHECKBOX *checkbox)
{
    /* Call default checkbox draw. */
    gx_checkbox_draw(checkbox);

    /* Add custom drawing here. */
}
```

### See Also

- gx_checkbox_create
- gx_checkbox_event_process
- gx_checkbox_select

## gx_checkbox_event_process


Process checkbox event

### Prototype

```C
UINT gx_checkbox_event_process(
    GX_CHECKBOX *checkbox, 
    GX_EVENT *event_ptr);
```

### Description

This service processes an event for the specified checkbox. This service should be called as the default event handler by any custom checkbox event processing functions.

### Parameters

- *checkbox*: Pointer to checkbox control block.
- *event_ptr*: Pointer to the event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful checkbox event process.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Call generic checkbox event processing as part of custom event processing function. */
UINT custom_checkbox_event_process(GX_CHECKBOX *checkbox, GX_EVENT *event)
{
    UINT status = GX_SUCCESS;

    switch(event->gx_event_type)
    {
    case xyz:
        /* Insert custom event handling here. */
        break;

    default:
        /* Pass all other events to the default checkbox event processing. */
        status = gx_checkbox_event_process(checkbox, event);
        break;
    }
    return status;
}
```

### See Also

- gx_checkbox_create
- gx_checkbox_draw
- gx_checkbox_select

## gx_checkbox_pixelmap_set


Set pixelmap for checkbox

### Prototype

```C
UINT gx_checkbox_pixelmap_set(
    GX_CHECKBOX *checkbox,
    GX_RESOURCE_ID unchecked_id, 
    GX_RESOURCE_ID checked_id,
    GX_RESOURCE_ID unchecked_disabled_id, 
    GX_RESOURCE_ID checked_disabled_id);
```

### Description

This service assigns the pixelmaps to be displayed by the specified checkbox for each checkbox state. The resource IDs can be duplicated.

### Parameters

- *checkbox*: Pointer to checkbox control block.
- *unchecked_id*: Pixelmap used for unchecked state.
- *checked_id*: Pixelmap used for checked state.
- *unchecked_disabled_id*: Pixelmap used for a disabled and unchecked checkbox.
- *checked_disabled_id*: Pixelmap used for a disabled and checked checkbox.

### Return Values

- **GX_SUCCESS** (0x00) Successful checkbox select.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Select "my_checkbox". */
status = gx_checkbox_pixelmap_set(&my_checkbox,
    PIXELMAP_UNCHECKED_ID,
    PIXELMAP_CHECKED_ID, PIXELMAP_UNCHECKED_DISABLED_ID,
    PIXELMAP_CHECKED_DISABLED_ID));

/* If status is GX_SUCCESS the pixelmaps are assigned to the checkbox "my_checkbox". */
```

### See Also

- gx_checkbox_create
- gx_checkbox_draw
- gx_checkbox_event_process

## gx_checkbox_select


Select checkbox

### Prototype

```C
UINT gx_checkbox_select(GX_CHECKBOX *checkbox);
```

### Description

This service forces a checkbox to the selected state.

### Parameters

- *checkbox*: Pointer to checkbox control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful checkbox select.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Select "my_checkbox". */
status = gx_checkbox_select(&my_checkbox);

/* If status is GX_SUCCESS the checkbox "my_checkbox" has been toggled. */
```

### See Also

- gx_checkbox_create
- gx_checkbox_draw
- gx_checkbox_event_process

## gx_circular_gauge_angle_get


Get current angle

### Prototype

```C
UINT gx_circular_gauge_angle_get(
    GX_CIRCULAR_GAUGE *gauge, 
    INT *angle);
```

### Description

This service retrieves the current needle angle of circular gauge widget.

### Parameters

- *gauge*: Pointer to circular gauge control block.
- *angle*: Current needle angle to be retrieved.

### Return Values

- **GX_SUCCESS** (0x00) Successful circular gauge angle get.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
INT current_angle;

/* Retrieve the current needle angle of "my_gauge". */
status = gx_circular_gauge_angle_get(&my_gauge, &current_angle);

/* If status is GX_SUCCESS the current needle angle of "my_gauge" has been retrieved. */
```

### See Also

- gx_circular_gauge_angle_set
- gx_circular_gauge_animation_set
- gx_circular_gauge_background_draw
- gx_circular_gauge_create
- gx_circular_gauge_draw
- gx_circular_gauge_event_process

## gx_circular_gauge_angle_set


Set target angle

### Prototype

```C
UINT gx_circular_gauge_angle_set(
    GX_CIRCULAR_GAUGE *gauge,
    INT angle);
```

### Description

This service sets the target angle of a circular gauge widget.

### Parameters

- *gauge*: Pointer to circular gauge control block.
- *angle*: Target needle angle.

### Return Values

- **GX_SUCCESS** (0x00) Successful angle set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set target angle of "my_gauge" to 180. */
status = gx_circular_gauge_angle_set(&my_gauge, 180);

/* If status is GX_SUCCESS the circular gauge of "my_gauge" has been set. */
```

### See Also

- gx_circular_gauge_angle_get
- gx_circular_gauge_animation_set
- gx_circular_gauge_background_draw
- gx_circular_gauge_create
- gx_circular_gauge_draw
- gx_circular_gauge_event_process

## gx_circular_gauge_animation_set


Set animation parameters

### Prototype

```C
UINT gx_circular_gauge_animation_set(
    GX_CIRCULAR_GAUGE *gauge,
    INT steps, 
    INT delay);
```

### Description

This service sets animation steps and delay time for a circular gauge widget.

### Parameters

- *gauge*: Pointer to circular gauge control block.
- *steps*: Total steps for one rotation.
- *delay*: Delay time for every step.

### Return Values

- **GX_SUCCESS** (0x00) Successful checkbox select.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_VALUE (0x22) Invalid value.

### Allowed From

Initialization and threads

### Example

```C
/* Set animation steps and delay time of circular gauge "my_gauge"
    to 30 and 1, the needle of "my_gauge" will rotate from
    current angle to target angle by 30 steps with 1 tick delay between every step. */
status = gx_circular_gauge_animation_set(&my_gauge, 30, 1);

/* If status is GX_SUCCESS the steps and delay time of "my_gauge" has been set. */
```

### See Also

- gx_circular_gauge_angle_get
- gx_circular_gauge_angle_set
- gx_circular_gauge_background_draw
- gx_circular_gauge_create
- gx_circular_gauge_draw
- gx_circular_gauge_event_process

## gx_circular_gauge_background_draw


Draw circular gauge background

### Prototype

```C
VOID gx_circular_gauge_background_draw(GX_CIRCULAR_GAUGE *gauge);
```

### Description

This service draws background of the specified circular gauge. This service is normally called internally by the gx_circular_gauge_draw function, but is exposed to the application to assist in writing custom drawing functions.

### Parameters

- *gauge*: Pointer to circular gauge control block.

### Return Values

None

### Allowed From

Threads

### Example

```C
/* Draw circular gauge background. */
gx_circular_gauge_background_draw(&my_circular_gauge);
```

### See Also

- gx_circular_gauge_angle_get
- gx_circular_gauge_angle_set
- gx_circular_gauge_animation_set
- gx_circular_gauge_create
- gx_circular_gauge_draw
- gx_circular_gauge_event_process

## gx_circular_gauge_create


Create circular gauge

### Prototype

```C
UINT gx_circular_gauge_create(
    GX_CIRCULAR_GAUGE *gauge,
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent,
    GX_CIRCULAR_GAUGE_INFO *info,
    GX_RESOURCE_ID background_id,
    ULONG style,
    USHORT circular_gauge_id,
    GX_VALUE xpos,
    GX_VALUE ypos);
```

### Description

This service creates a circular gauge widget with the specified properties.

### Parameters

- *gauge*: Pointer to circular gauge control block.
- *name*: Logical name of circular gauge widget.
- *parent*: Pointer to the parent widget.
- *info*: Pointer to the gauge information structure. **Appendix I** contains definition to GX_CIRCULAR_GAUGE_INFO structure.
- *background_id*: Resource ID of circular gauge background pixelmap.
- *style*: Style of circular gauge. **Appendix D** contains pre-defined general styles for all widgets as well as widget specific styles.
- *circular_gauge_id*: Application-defined ID of circular gauge.
- *xpos*: Gauge x-coordinate position.
- *ypos*: Gauge y-coordinate position.

### Return Values

- **GX_SUCCESS** (0x00) Successful checkbox select.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_SIZE (0x19) Invalid control block size.
- GX_ALREADY_CREATED (0x13) Widget already created.

### Allowed From

Initialization and threads

### Example

```C
GX_CIRCULAR_GAUGE gauge_info;

gauge_info.gx_circular_gauge_info_animation_steps = 30;
gauge_info.gx_circular_gauge_info_animation_delay = 1;
gauge_info.gx_circular_gauge_info_needle_xpos = 140;
gauge_info.gx_circular_gauge_info_needle_ypos = 140;
gauge_info.gx_circular_gauge_info_needle_xcor = 20;
gauge_info.gx_circular_gauge_info_needle_ycor = 88;
gauge_info.gx_circular_gauge_info_needle_pixelmap = GX_PIXELMAP_ID_NEEDLE;

/* Create "my_gauge". */
status = gx_circular_gauge_create(&my_gauge, "my_gauge",
            &my_parent,
            &gauge_info, MY_PIXELMAP_RESOURCE_ID, GX_NULL,
            MY_ICON_ID, 5, 30);

/* If status is GX_SUCCESS the circular gauge "my_gauge" has been created. */
```

### See Also

- gx_circular_gauge_angle_get
- gx_circular_gauge_angle_set
- gx_circular_gauge_animation_set
- gx_circular_gauge_background_draw
- gx_circular_gauge_draw
- gx_circular_gauge_event_process

## gx_circular_gauge_draw

Draw circular gauge

### Prototype

```C
VOID gx_circular_gauge_draw(GX_CIRCULAR_GAUGE *gauge);
```

### Description

This service draws the specified circular gauge. This function is normally called internally by the GUIX canvas refresh mechanism, but is exposed to the application to assist with implementing custom drawing functions for custom gauge widgets.

### Parameters

- *gauge*: Pointer to circular gauge control block.

### Return Values

None

### Allowed From

Threads

### Example

```C
/* Write a custom circular gauge draw function. */
VOID custom_gauge_draw(GX_CIRCULAR_GAUGE *gauge)
{
    /* Call default circular gauge draw. */
    gx_circular_gauge_draw(gauge);

    /* Add custom drawing here. */
}
```

### See Also

- gx_circular_gauge_angle_get
- gx_circular_gauge_angle_set
- gx_circular_gauge_animation_set
- gx_circular_gauge_background_draw
- gx_circular_gauge_create
- gx_circular_gauge_event_process

## gx_circular_gauge_event_process


Process circular gauge event

### Prototype

```C
UINT gx_circular_gauge_event_process(
    GX_CIRCULAR_GAUGE *gauge,
    GX_EVENT *event);
```

### Description

This service processes an event for the specified circular gauge.

### Parameters

- *gauge*: Pointer to gauge control block.
- *event_ptr*: Pointer to event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful gauge event process.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Threads

### Example

```C
/* Call generic circular gauge event processing as part of custom event processing function. */
UINT custom_gauge_event_process(GX_CIRCULAR_GAUGE *gauge,
    GX_EVENT *event)
{
    UINT status = GX_SUCCESS;

    switch(event->gx_event_type)
    {
    case xyz:
        /* Insert custom event handling here. */
        break;

    default:
        /* Pass all other events to the default circular gauge event processing. */
        status = gx_circular_gauge_event_process(gauge, event);
        break;
}
return status;
```

### See Also

- gx_circular_gauge_angle_get
- gx_circular_gauge_angle_set
- gx_circular_gauge_animation_set
- gx_circular_gauge_background_draw
- gx_circular_gauge_create
- gx_circular_gauge_draw

## gx_context_brush_default


Set brush of current drawing context

### Prototype

```C
UINT gx_context_brush_default(GX_DRAW_CONTEXT *context);
```

### Description

This service sets the brush of the specified drawing context to default.

### Parameters

- *context*: Pointer to context control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful creation.
- GX_PTR_ERROR (0x07) Invalid context pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set the brush of "my_context" to default. */
status = gx_context_brush_default(&my_context);

/* If status is GX_SUCCESS the brush of "my_context" has been set to default. */

```

### See Also

- gx_context_brush_define
- gx_context_brush_get
- gx_context_brush_set
- gx_context_brush_style_set
- gx_context_brush_pattern_set
- gx_context_brush_width_set
- gx_context_fill_color_set
- gx_context_font_set
- gx_context_line_color_set
- gx_context_pixelmap_set
- gx_context_raw_brush_define
- gx_context_raw_fill_color_set
- gx_context_raw_line_color_set

## gx_context_brush_define


Define brush of current drawing context

### Prototype

```C
UINT gx_context_brush_define(
    GX_RESOURCE_ID line_color_id,
    GX_RESOURCE_ID fill_color_id, 
    UINT style);
```

### Description

This service defines the brush of the current drawing context.

### Parameters

- *line_color_id*: Resource ID of line color. **Appendix B** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.
- *fill_color_id*: Resource ID of fill color. **Appendix B** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.
- *style*: Style of brush. **Appendix D** describes the supported brush styles. Brush styles can be combined into one variable using bitwise OR operation.

### Return Values

- **GX_SUCCESS** (0x00) Successful context brush define.
- **GX_INVALID_RESOURCE_ID** (0x33) Invalid resource ID.
- GX_INVALID_CONTEXT (0x06) No active drawing context define.

### Allowed From

Initialization and threads

### Example

```C
/* Define the brush of the current context. */
status = gx_context_brush_define(GX_COLOR_BLACK_ID,
    GX_COLOR_BLACK_ID,
    GX_STYLE_BORDER_NONE);

/* If status is GX_SUCCESS the brush of the current context has been defined. */
```

### See Also

- gx_context_brush_default
- gx_context_brush_get
- gx_context_brush_set
- gx_context_brush_pattern_set
- gx_context_brush_style_set
- gx_context_brush_width_set
- gx_context_fill_color_set
- gx_context_font_set
- gx_context_line_color_set
- gx_context_pixelmap_set
- gx_context_raw_brush_define
- gx_context_raw_fill_color_set
- gx_context_raw_line_color_set

## gx_context_brush_get


Get brush of current drawing context

### Prototype

```C
UINT gx_context_brush_get(GX_BRUSH **return_brush);
```

### Description

This service returns a pointer to the active brush in the current drawing context. If there is no active drawing context, the service fails and returns a NULL pointer.

### Parameters

- *return_brush*: Pointer to destination for brush.

### Return Values

- **GX_SUCCESS** (0x00) Successfully retrieved context brush.
- GX_INVALID_CONTEXT (0x06) No active drawing context.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_BRUSH *my_brush;

/* Get the brush of the current context. */
status = gx_context_brush_get(&my_brush);

/* If status is GX_SUCCESS the brush of the current context has been retrieved. */
```

### See Also

- gx_context_brush_set
- gx_context_brush_style_set
- gx_context_brush_default
- gx_context_brush_define
- gx_context_brush_pattern_set
- gx_context_brush_width_set
- gx_context_fill_color_set
- gx_context_font_set
- gx_context_line_color_set
- gx_context_pixelmap_set
- gx_context_raw_brush_define
- gx_context_raw_fill_color_set
- gx_context_raw_line_color_set

## gx_context_brush_pattern_set


Set brush pattern of current drawing context

### Prototype

```C
UINT gx_context_brush_pattern_set(ULONG pattern);
```

### Description

This service sets the brush pattern of the current drawing context.

The brush pattern is used for drawing dashed horizontal and dashed vertical lines. When the gx_canvas_line_draw() is called, and the line is horizontal or vertical, and the brush.gx_brush_line_pattern field is non-zero, a pattern line is drawn.

The brush pattern mask is currently only supported for horizontal and vertical lines.

### Parameters

- *pattern*: Pattern to be used for the brush. This is a simple hexadecimal on/off pattern to be used for pattern line drawing.

### Return Values

- **GX_SUCCESS** (0x00) Successful context brush set.
- GX_INVALID_CONTEXT (0x06) Invalid drawing context.

### Allowed From

Initialization and threads

### Example

```C
/* Set the brush pattern for the current context. */
status = gx_context_brush_pattern_set(0x80808080);

/* If status is GX_SUCCESS the brush pattern of the current context
has been set to the specified pattern. */
```

### See Also

- gx_context_brush_default
- gx_context_brush_define
- gx_context_brush_get
- gx_context_brush_style_set
- gx_context_brush_width_set
- gx_context_fill_color_set
- gx_context_font_set
- gx_context_line_color_set
- gx_context_pixelmap_set
- gx_context_raw_brush_define
- gx_context_raw_fill_color_set
- gx_context_raw_line_color_set

## gx_context_brush_set


Set brush of current drawing context

### Prototype

```C
UINT gx_context_brush_set(GX_BRUSH *brush);
```

### Description

This service sets the brush of the current drawing context.

### Parameters

- *brush*: Pointer to brush to use for current context.

### Return Values

- **GX_SUCCESS** (0x00) Successful context brush set.
- GX_INVALID_CONTEXT (0x06) No active drawing context.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_BRUSH my_brush;
GX_FONT *font;
GX_COLOR fill_color;
GX_COLOR line_color;

/* Retrieve the font that associated with the specified font ID. */
gx_context_font_get(MY_FONT_ID, &font);

/* Retrieve the color that associated with the specified color ID. */
gx_context_color_get(MY_FILL_COLOR_ID, &fill_color);
gx_context_color_get(MY_LINE_COLOR, &line_color);

my_brush.gx_brush_pixelmap = MY_PIXELMAP_ID;
my_brush.gx_brush_font = font;
my_brush.gx_brush_line_pattern = 0x80808080;
my_brush.gx_brush_pattern_mask = 0x80000000;
my_brush.gx_brush_fill_color = fill_color;
my_brush.gx_brush_line_color = line_color;
my_brush.gx_brush_style = GX_BRUSH_SOLID_FILL | GX_BRUSH_ALIAS |
                          GX_BRUSH_PIXELMAP_FILL | GX_BRUSH_ROUND
my_brush.gx_brush_width = 2;
my_brush.gx_brush_alpha = 255;

/* Set the brush of the current context. */
status = gx_context_brush_set(my_brush);

/* If status is GX_SUCCESS the brush of the current context has been set. */
```

### See Also

- gx_context_brush_default
- gx_context_brush_define
- gx_context_brush_get
- gx_context_brush_pattern_set
- gx_context_brush_style_set
- gx_context_brush_width_set
- gx_context_fill_color_set
- gx_context_font_set
- gx_context_line_color_set
- gx_context_pixelmap_set
- gx_context_raw_brush_define
- gx_context_raw_fill_color_set
- gx_context_raw_line_color_set

## gx_context_brush_style_set


Set brush style of current drawing context

### Prototype

```C
UINT gx_context_brush_style_set(UINT style);
```

### Description

This service sets the brush style of the current drawing context.

### Parameters

- *style*: Brush style of current context. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.

### Return Values

- **GX_SUCCESS** (0x00) Successful context brush style set.
- GX_INVALID_CONTEXT (0x06) No active drawing context.

### Allowed From

Initialization and threads

### Example

```C
/* Set the brush style of the current context. */
status = gx_context_brush_style_set(GX_BRUSH_ALIAS);

/* If status is GX_SUCCESS the brush style of the current context has been set. */
```

### See Also

- gx_context_brush_default
- gx_context_brush_define
- gx_context_brush_get
- gx_context_brush_set
- gx_context_brush_pattern_set
- gx_context_brush_width_set
- gx_context_fill_color_set
- gx_context_font_set
- gx_context_line_color_set
- gx_context_pixelmap_set
- gx_context_raw_brush_define
- gx_context_raw_fill_color_set
- gx_context_raw_line_color_set

## gx_context_brush_width_set


Set brush width of current drawing context

### Prototype

```C
UINT gx_context_brush_width_set(UINT width);
```

### Description

This service sets the width of the active brush in the current drawing context.

### Parameters

*width*: Brush width in pixels of current context

### Return Values

**GX_SUCCESS** (0x00) Successful context brush width set

GX_INVALID_CONTEXT (0x06) No active drawing context

### Allowed From

Initialization and threads

### Example

```C
/* Set the brush width of the current context to 10 pixels. */
status = gx_context_brush_width_set(10); /*

If status is GX_SUCCESS the brush width of the current context has been set to 10. */
```

### See Also

- gx_context_brush_default
- gx_context_brush_define
- gx_context_brush_get
- gx_context_brush_set
- gx_context_brush_pattern_set
- gx_context_brush_style_set
- gx_context_fill_color_set
- gx_context_font_set
- gx_context_line_color_set
- gx_context_pixelmap_set
- gx_context_raw_brush_define
- gx_context_raw_fill_color_set
- gx_context_raw_line_color_set

## gx_context_color_get


Get color value associated with color ID in current draw context

### Prototype

```C
UINT gx_context_color_get(
    GX_RESOURCE_ID color_id,
    GX_COLOR *return_color);
```

### Description

This service retrieves the color value associated with the indicated color ID. The color value is returned in the color format of the active context display. This service should only be called from within an active drawing operation.

### Parameters

*color_id*: Resource ID of color requested.
*return_color*: Address of variable to hold returned color value.

### Return Values

**GX_SUCCESS** (0x00) Successful color value get
**GX_INVALID_RESOURCE_ID** (0x33) Invalid resource ID
GX_INVALID_CONTEXT (0x06) No active drawing context
GX_PTR_ERROR (0x07) Invalid pointer

### Allowed From

Initialization and threads

### Example

```C
GX_COLOR color_value;

/* Get the color value. */
status = gx_context_color_get(MY_BLACK_COLOR_ID, &color_value);
```

### See Also

- gx_context_brush_default
- gx_context_brush_define
- gx_context_brush_get
- gx_context_brush_set
- gx_context_brush_pattern_set
- gx_context_brush_style_set
- gx_context_brush_width_set
- gx_context_font_set
- gx_context_line_color_set
- gx_context_pixelmap_set
- gx_context_raw_brush_define
- gx_context_raw_fill_color_set
- gx_context_raw_line_color_set

## gx_context_fill_color_set

Set fill color of current drawing context

### Prototype

```C
UINT gx_context_fill_color_set(GX_RESOURCE_ID fill_color_id);
```

### Description

This service sets the fill color of the active brush in the current drawing context.

### Parameters

- *fill_color_id*: Resource ID of fill color of current context. **Appendix B** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.

### Return Values

- **GX_SUCCESS** (0x00) Successful context fill color set.
- **GX_INVALID_RESOURCE_ID** (0x33) Invalid resource ID.
- GX_INVALID_CONTEXT (0x06) No active drawing context.

### Allowed From

Initialization and threads

### Example

```C
/* Set the fill color of the current context to black. */
status = gx_context_fill_color_set(MY_BLACK_COLOR_ID);

/* If status is GX_SUCCESS the fill color of the current context has been set to black. */
```

### See Also

- gx_context_brush_default
- gx_context_brush_define
- gx_context_brush_get
- gx_context_brush_set
- gx_context_brush_pattern_set
- gx_context_brush_style_set
- gx_context_brush_width_set
- gx_context_font_set
- gx_context_line_color_set
- gx_context_pixelmap_set
- gx_context_raw_brush_define
- gx_context_raw_fill_color_set
- gx_context_raw_line_color_set

## gx_context_font_get

Get font associated with font ID in current draw context

### Prototype

```C
UINT gx_context_font_get(
    GX_RESOURCE_ID font_id,
    GX_FONT **return_font);
```

### Description

This service retrieves the font pointer associated with the indicated font ID. This service should only be called from within an active drawing operation.

### Parameters

- *font_id*: Resource ID of font requested.
- *return_font*: Address of variable to hold returned font pointer.

### Return Values

- **GX_SUCCESS** (0x00) Successfully retrieved font.
- **GX_INVALID_RESOURCE_ID** (0x33) Invalid resource ID.
- GX_INVALID_CONTEXT (0x06) No active drawing context.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_FONT *my_font;

/* Get the font pointer. */
status = gx_context_font_get(MY_MIDSIZE_FONT, &my_font);

/* If status is GX_SUCCESS, the font that indicated with MY_MIDSIZE_FONT has been successfully retrieved. */
```

### See Also

- gx_context_brush_get
- gx_context_brush_set
- gx_context_brush_default
- gx_context_brush_define
- gx_context_brush_pattern_set
- gx_context_brush_style_set
- gx_context_brush_width_set
- gx_context_font_set
- gx_context_line_color_set
- gx_context_pixelmap_set
- gx_context_raw_brush_define
- gx_context_raw_fill_color_set
- gx_context_raw_line_color_set

## gx_context_font_set

Set font of current drawing context

### Prototype

```C
UINT gx_context_font_set(GX_RESOURCE_ID font_id);
```

### Description

This service sets the font in the active brush of the current drawing context.

### Parameters

- *font_id*: Font resource ID of current context.

### Return Values

- **GX_SUCCESS** (0x00) Successful context font set.
- **GX_INVALID_RESOURCE_ID** (0x33) Invalid resource ID.
- GX_INVALID_CONTEXT (0x06) No active drawing context.

### Allowed From

Initialization and threads

### Example

```C
/* Set the font of the current context to MY_FONT_ID. */
status = gx_context_font_set(MY_FONT_ID);

/* If status is GX_SUCCESS the font of the current context has been set. */
```

### See Also

- gx_context_brush_default
- gx_context_brush_define
- gx_context_brush_get
- gx_context_brush_set
- gx_context_brush_pattern_set
- gx_context_brush_style_set
- gx_context_brush_width_set
- gx_context_fill_color_set
- gx_context_line_color_set
- gx_context_pixelmap_set
- gx_context_raw_brush_define
- gx_context_raw_fill_color_set
- gx_context_raw_line_color_set

## gx_context_line_color_set

Set line color of current drawing context

### Prototype

```C
UINT gx_context_line_color_set(GX_RESOURCE_ID line_color_id);
```

### Description

This service sets the line color of the active brush in the current drawing context.

### Parameters

- *line_color_id*: Line color of current context. **Appendix B** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.

### Return Values

- **GX_SUCCESS** (0x00) Successful context line color set.
- **GX_INVALID_RESOURCE_ID** (0x33) Invalid resource ID.
- GX_INVALID_CONTEXT (0x06) No active drawing context.

### Allowed From

Initialization and threads

### Example

```C
/* Set the line color of the current context to black. */
status = gx_context_line_color_set(GX_COLOR_BLACK_ID);

/* If status is GX_SUCCESS the line color of the current context has been set to black. */
```

### See Also

- gx_context_brush_default
- gx_context_brush_define
- gx_context_brush_get
- gx_context_brush_set
- gx_context_brush_pattern_set
- gx_context_brush_style_set
- gx_context_brush_width_set
- gx_context_fill_color_set
- gx_context_font_set
- gx_context_pixelmap_set
- gx_context_raw_brush_define
- gx_context_raw_fill_color_set
- gx_context_raw_line_color_set

## gx_context_pixelmap_get

Get pixelmap associated with pixelmap ID in current draw context

### Prototype

```C
UINT gx_context_pixelmap_get(
    GX_RESOURCE_ID pixelmap_id,
    GX_PIXELMAP **return_map);
```

### Description

This service retrieves the pixelmap pointer associated with the indicated pixelmap ID.

### Parameters

- *pixelmap_id*: Resource ID of pixelmap requested.
- *return_map*: Address of variable to hold returned pixelmap address.

### Return Values

- **GX_SUCCESS** (0x00) Successfully retrieved pixelmap.
- **GX_INVALID_RESOURCE_ID** (0x33) Invalid resource ID.
- GX_INVALID_CONTEXT (0x06) No active drawing context.
- GX_PTR_ERROR (0x07) Invalid pixelmap pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_PIXELMAP *map;

/* Get the pixelmap pointer. */
status = gx_context_pixelmap_get(MY_PIXELMAP_ID, &map);

/* If status is GX_SUCCESS, the pixelmap was successfully retrieved. */
```

### See Also

- gx_context_brush_default
- gx_context_brush_define
- gx_context_brush_get
- gx_context_brush_set
- gx_context_brush_pattern_set
- gx_context_brush_style_set
- gx_context_brush_width_set
- gx_context_font_set
- gx_context_line_color_set
- gx_context_pixelmap_set
- gx_context_raw_brush_define
- gx_context_raw_fill_color_set
- gx_context_raw_line_color_set

## gx_context_pixelmap_set

Set pixelmap of current draw context

### Prototype

```C
UINT gx_context_pixelmap_set(GX_RESOURCE_ID pixelmap_id);
```

### Description

This service assigns the pixelmap of the active brush in the current drawing context.

### Parameters

- *pixelmap_id*: Pixelmap resource ID to use for current context.

### Return Values

- **GX_SUCCESS** (0x00) Successful context pixelmap set.
- **GX_INVALID_RESOURCE_ID** (0x33) Invalid resource ID.
- GX_INVALID_CONTEXT (0x06) Invalid context.

### Allowed From

Initialization and threads

### Example

```C
/* Set pixelmap of the current context to MY_PIXELMAP_ID. */
status = gx_context_pixelmap_set(MY_PIXELMAP_ID);

/* If status is GX_SUCCESS the pixelmap of the current context has been set. */
```

### See Also

- gx_context_brush_default
- gx_context_brush_define
- gx_context_brush_get
- gx_context_brush_set
- gx_context_brush_pattern_set
- gx_context_brush_style_set
- gx_context_brush_width_set
- gx_context_fill_color_set
- gx_context_font_set
- gx_context_line_color_set
- gx_context_raw_brush_define
- gx_context_raw_fill_color_set
- gx_context_raw_line_color_set

## gx_context_raw_brush_define

Define raw brush of current draw context

### Prototype

```C
UINT gx_context_raw_brush_define(
    GX_COLOR line_color,
    GX_COLOR fill_color, 
    UINT style);
```

### Description

This service defines the raw brush of the current screen context. Raw definitions are used when 32-bit ARGB color values are to be passed into the brush rather than color IDs. Raw color definitions are useful when the desired color is not present in the current system color table or when the RGB color value is computed at runtime.

### Parameters

- *line_color*: Color of line in 32-bit raw ARGB color format. **Appendix A** contains pre-defined colors. Note that the application may add custom colors as well.
- *fill_color*: Color of fill in 32-bit raw ARGB color format. **Appendix A** contains pre-defined colors. Note that the application may add custom colors as well.
- *style*: Style of brush. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.

### Return Values

- **GX_SUCCESS** (0x00) Successful context raw brush define.
- GX_INVALID_CONTEXT (0x06) No active drawing context.

### Allowed From

Initialization and threads

### Example

```C
/* Define the raw brush of the current context. */
status = gx_context_raw_brush_define(GX_COLOR_BLACK, GX_COLOR_BLACK, GX_STYLE_BORDER_NONE);

/* If status is GX_SUCCESS the raw brush of the current context has been defined. */
```

### See Also

- gx_context_brush_default
- gx_context_brush_define
- gx_context_brush_get
- gx_context_brush_set
- gx_context_brush_pattern_set
- gx_context_brush_style_set
- gx_context_brush_width_set
- gx_context_fill_color_set
- gx_context_font_set
- gx_context_line_color_set
- gx_context_pixelmap_set
- gx_context_raw_fill_color_set
- gx_context_raw_line_color_set

## gx_context_raw_fill_color_set

Set raw fill color of current drawing context

### Prototype

```C
UINT gx_context_raw_fill_color_set(GX_COLOR line_color);
```

### Description

This service sets the raw fill color of the current screen context. The line_color parameter is a 32-bit ARGB format raw color value, rather than a color ID value. Raw color definitions are useful when the desired color is not present in the current system color table or when the RGB color value is computed at runtime.

### Parameters

- *line_color*: Color of line. **Appendix A** contains pre-defined colors. Note that the application may add custom colors as.
well.

### Return Values

- **GX_SUCCESS** (0x00) Successful context raw fill color set.
- GX_INVALID_CONTEXT (0x06) No active drawing context.

### Allowed From

Initialization and threads

### Example

```C
/* Set the raw fill color of the current context. */
status = gx_context_raw_fill_color_set(GX_COLOR_BLACK);

/* If status is GX_SUCCESS the raw fill color of the current context has been set. */
```

### See Also

- gx_context_brush_default
- gx_context_brush_define
- gx_context_brush_get
- gx_context_brush_set
- gx_context_brush_pattern_set
- gx_context_brush_style_set
- gx_context_brush_width_set
- gx_context_fill_color_set
- gx_context_font_set
- gx_context_line_color_set
- gx_context_pixelmap_set
- gx_context_raw_brush_define
- gx_context_raw_line_color_set

## gx_context_raw_line_color_set

Set raw line color of current drawing context

### Prototype

```C
UINT gx_context_raw_line_color_set(GX_COLOR line_color);
```

### Description

This service sets the line color of the active brush in the current drawing context. The line_color parameter is a 32-bit ARGB format raw color value, rather than a color ID value. Raw color definitions are useful when the desired color is not present in the current system color table or when the RGB color value is computed at runtime.

### Parameters

- *line_color*: Color of line value. **Appendix A** contains pre-defined colors. Note that the application may add custom colors as well.

### Return Values

- **GX_SUCCESS** (0x00) Successful context raw line color set.
- **GX_INVALID_CONTEXT** (0X06) No active drawing context.

### Allowed From

Initialization and threads

```C
/* Set the raw line color of the current context. */
status = gx_context_raw_line_color_set(GX_COLOR_BLACK);

/* If status is GX_SUCCESS the raw line color of the current context has been set. */
```

### See Also

- gx_context_brush_default
- gx_context_brush_define
- gx_context_brush_get
- gx_context_brush_set
- gx_context_brush_pattern_set
- gx_context_brush_style_set
- gx_context_brush_width_set
- gx_context_fill_color_set
- gx_context_font_set
- gx_context_line_color_set
- gx_context_pixelmap_set
- gx_context_raw_brush_define
- gx_context_raw_fill_color_set

## gx_context_string_get

Retrieve string associated with String ID (deprecated)

### Prototype

```C
UINT gx_context_string_get(GX_RESOURCE_ID string_id, GX_CONST GX_CHAR **return_string);
```

### Description

This deprecated API returns the string associated with the given string ID. New applications should use gx_context_string_get_ext().

### Parameters

- *string_id*: String ID generated by the GUIX Studio application.
- *return_string*: Address of variable to return string pointer.

### Return Values

- **GX_SUCCESS** (0x00) Successful context raw line color set.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_INVALID_CONTEXT (0X06) No active drawing context.

### Allowed From

Initialization and threads

### Example

```C
GX_CHAR *text;

status = gx_context_string_get(GX_ID_ERROR, &text);

/* If status is GX_SUCCESS the string pointer has been returned. */
```

### See Also

- gx_context_string_get_ext

## gx_context_string_get_ext

Retrieve string associated with given string ID.

### Prototype

```C
UINT gx_context_string_get_ext(GX_RESOURCE_ID string_id, GX_STRING *return_string);
```

### Description

This service returns the string associated with a given string ID. This service can only be invoked when there is an active drawing context, i.e. from within the drawing function of a widget. This service identifies the active canvas and display and retrieves the string from the located display instance.

### Parameters

- *string_id*: String ID used to identify the string, as generated by GUIX Studio in the application resource header file.
- *return_string*: Address of GX_STRING variable in which the string pointer and string length will be returned.

### Return Values

- **GX_SUCCESS** (0x00) Successful context raw line color set.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_INVALID_CONTEXT (0X06) No active drawing context.

### Allowed From

Initialization and threads

### Example


```C
GX_STRING string;

/* Set the raw line color of the current context. */
status = gx_context_string_get_ext(ID_ERROR, &string);

/* If status is GX_SUCCESS the string.gx_string_ptr and
string.gx_string_length values have been returned. */
```

### See Also

- gx_context_brush_default
- gx_context_brush_define
- gx_context_brush_get
- gx_context_brush_set
- gx_context_brush_pattern_set
- gx_context_brush_style_set
- gx_context_brush_width_set
- gx_context_fill_color_set
- gx_context_font_set
- gx_context_line_color_set
- gx_context_pixelmap_set
- gx_context_raw_brush_define
- gx_context_raw_fill_color_set

## gx_display_active_language_set

Assign the display active language

### Prototype

```C
UINT gx_display_active_language_set(
    GX_DISPLAY *display, 
    GX_UBYTE language);
```

### Description

This service assigns the currently active language for the indicated display. The language index corresponds to the languages defined in the display language table, and is not an ANSI language identifier.

Different displays in a multi display system can each run different active languages. The display language table should be assigned before this API is used. When a display is initialized using gx_studio_display_configure, the language table is automatically installed and the application passes in the active language index.

### Parameters

- *display*: Pointer to display control block.
- *language*: Active language index.

### Return Values

- **GX_SUCCESS** (0x00) Successful language assign.
- GX_PTR_ERROR (0x07) Invalid display pointer.
- GX_INVALID_VALUE (0x22) Invalid language index.

### Allowed From

Initialization and threads

### Example

```C
/* Change value of color MY_COLOR_ID. */
status = gx_display_active_language_set(&my_display, LANGUAGE_ENGLISH);

/* If status is GX_SUCCESS the active language has been assigned. */
```

### See Also

- gx_display_language_table_set
- gx_studio_display_configure

## gx_display_color_set

Re-assign one color value

### Prototype

```C
UINT gx_display_color_set(
    GX_DISPLAY *display,
    GX_RESOURCE_ID color_id, 
    GX_COLOR new_color);
```

### Description

This service re-assigns the color value associated with the specified color ID. This can be used to modify the color table of a display without providing an entirely new color table. The color value provided must be in the native format supported by the display.

### Parameters

- *display*: Pointer to display control block.
- *color_id*: Color ID to reassign.
- *new_color*: Color value to assign to this color_id slot.

### Return Values

- **GX_SUCCESS** (0x00) Successful color reassign.
- GX_INVALID_RESOURCE_ID (0x33) Invalid color ID.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_INVALID_DISPLAY (0x1D) Invalid display.

### Allowed From

Initialization and threads

### Example

```C
/* Change value of color MY_COLOR_ID. */
status = gx_display_color_set(&my_display, MY_COLOR_ID, 0x5454);

/* If status is GX_SUCCESS the color has been reassigned. */
```

### See Also

- gx_display_color_table_set

## gx_display_color_table_set

Assign display color table

### Prototype

```C
UINT gx_display_color_table_set(
    GX_DISPLAY *display,
    GX_COLOR *color_table, 
    INT color_count);
```

### Description

This service re-assigns the color table to be used by the display. This service is normally invoked by the GUIX Studio generated display configuration function, but can also be called by the application software.

### Parameters

- *display*: Pointer to display control block.
- *color_table*: Array of color values in display native format.
- *color_count*: Indicates number of entries in color table.

### Return Values

- **GX_SUCCESS** (0x00) Successful color table set.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_INVALID_DISPLAY (0x1D) Invalid display.

### Allowed From

Initialization and threads

### Example

```C
GX_COLOR default_table[32] = { … };

/* Change the color table. */
status = gx_display_color_table_set(&my_display, default_table, 32);

/* If status is GX_SUCCESS the color table has been reassigned. */
```

### See Also

- gx_display_color_set

## gx_display_create

Create display

### Prototype

```C
UINT gx_display_create(
    GX_DISPLAY *display, 
    GX_CONST CHAR *name,
    UINT (*display_driver_setup)(GX_DISPLAY *),
    GX_VALUE width, 
    GX_VALUE height);
```

### Description

This service creates a display and calls the display driver setup function. GUIX takes this display and adds it to its internal list of displays.

### Parameters

- *display*: Pointer to display control block.
- *name*: Name of the display.
- *display_driver_setup*: Pointer to display driver setup function.
- *optional_driver_info*: Pointer to optional driver information.
- *color_format*: Color format, as defined in **Appendix C**.
- *width*: Number of pixels on the x-axis.
- *height*: Number of pixels on the y-axis.

### Return Values

- **GX_SUCCESS** (0x00) Successful display create.
- **GX_SYSTEM_ERROR** (0xFE) Fail to setup display.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_SIZE (0x23) Invalid display control block size.

### Allowed From

Initialization and threads

### Example

```C
GX_DISPLAY my_display;

UINT my_driver_setup_callback(GX_DISPLAY *display)
{
    …
}

/* Create screen "my_display". */
status = gx_display_create(&my_display, "my display",
                           my_driver_setup_callback, 320, 480);

/* If status is GX_SUCCESS, the screen "my_display" has been created. */
```

### See Also

- gx_display_delete

## gx_display_delete


Destroy display

### Prototype

```C
UINT gx_display_delete(
    GX_DISPLAY *display,
    VOID (*display_driver_cleanup)(GX_DISPLAY *));
```

### Description

This service shuts down a display, and cleans up allocated resources.

### Parameters

- *display*: Pointer to display control block.
- *display_driver_cleanup*: Pointer to display driver cleanup function.

### Return Values

- **GX_SUCCESS** (0x00) Successful display delete.
- **GX_FAILURE** (0x10) Created display list is NULL.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
VOID driver_cleanup_callback(GX_DISPLAY *display)
{
    …
}

/* Delete a display "my_display". */
status = gx_display_delete(&my_display, driver_cleanup_callback);

/* If status is GX_SUCCESS the screen "my_display" has been destroyed. */
```

### See Also

- gx_canvas_block_move
- gx_canvas_line_draw
- gx_canvas_pixelmap_draw
- gx_canvas_pixelmap_tile
- gx_canvas_polygon_draw
- gx_canvas_rectangle_draw
- gx_canvas_text_draw
- gx_display_create

## gx_display_font_table_set

Assign display font table

### Prototype

```C
UINT gx_display_font_table_set(
    GX_DISPLAY *display,
    GX_FONT **font_table, 
    INT table_size);
```

### Description

This service re-assigns the font table to be used by the display. This service is normally invoked by the GUIX Studio generated display configuration function, but can also be called by the application software.

### Parameters

- *display*: Pointer to display control block.
- *font_table*: Array of GX_FONT pointers.
- *table_size*: Number of fonts in table.

### Return Values

- *GX_SUCCESS* (0x00) Successful font table set.
- *GX_CALLER_ERROR* (0x11) Invalid caller of this function.
- *GX_PTR_ERROR* (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_FONT *font_table[32] = { … };

/* Assign font table. */
status = gx_display_font_table_set(&my_display, font_table, 32);

/* If status is GX_SUCCESS, the font table has been reassigned. */
```

### See Also

- gx_display_color_set
- gx_display_color_table_set
- gx_display_pixelmap_table_set

## gx_display_language_direction_table_set

Set language direction table

### Prototype

```C
UINT  _gx_display_language_direction_table_set(
    GX_DISPLAY *display,
    GX_CONST GX_UBYTE *language_direction_table,
    GX_UBYTE num_languages);
```

### Description

This service is used for setting a base direction for each language utilized within the system. The base direction is used for determining the reading direction of the text, which can be either left-to-right or right-to-left. This service only valid when configured with **GX_ENABLE_BIDI_SUPPORT**.

Configuring the base direction is especially crucial when working with a mixture of left-to-right and right-to-left strings. When the base direction varies, the rendering results for identical strings containing both types of characters also differ.

Typically, GUIX Studio automatically generates a language direction table, associating a specific direction with each language. For example, languages like Hebrew and Arabic, which read from right to left, are assigned *GX_LANGUAGE_DIRECTION_RTL*, while languages like English, which read from left to right, are assigned *GX_LANGUAGE_DIRECTION_LTR*. If the default base direction isn't match your requirement, you can utilize this API to set a customized language direction table.

### Parameters

- *display*: Pointer to display control block.
- *language_direction_table*: The language direction table to be sets. The supported direction values are:
    - *GX_LANGUAGE_DIRECTION_LTR*: Left to right language direction.
    - *GX_LANGUAGE_DIRECTION_RTL*: Right to left language direction.
- *num_languages*: Number of languages in the language direction table.

### Return Values

- **GX_SUCCESS** (0x00) Successful font table set.
- **GX_INVALID_VALUE** (0x22) Invalid table size.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_CONST GX_UBYTE language_direction_table[2] = 
{
    GX_LANGUAGE_DIRECTION_RTL,
    GX_LANGUAGE_DIRECTION_LTR
};

/* Set language direction table. */
status = gx_display_language_direction_table_set(&my_display, language_direction_table, 2);

/* If status is GX_SUCCESS, the language direction table has been set. */
```

### See Also

- gx_display_language_table_set
- gx_display_active_language_set
- gx_display_string_get
- gx_display_language_table_get_ext

## gx_display_language_table_get

Retrieve display language table (deprecated)

### Prototype

```C
UINT gx_display_language_table_get(
    GX_DISPLAY *display,
    GX_CHAR **table, 
    GX_UBYTE *language_count,
    UINT *string_table_size);
```

### Description

This service retrieves the language table from the indicated display. This service can be used by an application to modify the display language table, at runtime, using dynamically defined strings.

This API is deprecated and supported only for applications using the old style language table (i.e. the Studio generated resource file is generated for library version prior to version 5.6). New applications should use gx_display_language_table_get_ext().

### Parameters

- *display*: Pointer to display control block.
- *table*: Address to receive table pointer.
- *language_count*: Address to receive language count.
- *string_table_size*: Address to receive string table size.

### Return Values

- **GX_SUCCESS** (0x00) Successful font table set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_CHAR **language_table;
GX_UBYTE language_count;
UINT string_count;

/* Retrieve language table. */
status = gx_display_language_table_get(&my_display,
        &language_table, &language_count, &string_count);

/* If status is GX_SUCCESS, the language table has been retrieved. */
```

### See Also

- gx_display_language_direction_table_set
- gx_display_language_table_set
- gx_display_active_langauge_set
- gx_display_string_get
- gx_display_language_table_get_ext

## gx_display_language_table_get_ext

Retrieve display language table

### Prototype

```C
UINT gx_display_language_table_get_ext(
    GX_DISPLAY *display,
    GX_STRING **table, 
    GX_UBYTE *language_count, 
    UINT *string_table_size);
```

### Description

This service retrieves the language table from the indicated display. This service can be used by an application to modify the display language table, at runtime, using dynamically defined strings.

### Parameters

- *display*: Pointer to display control block.
- *table*: Address to receive table pointer.
- *language_count*: Address to receive language count.
- *string_table_size*: Address to receive string table size.

### Return Values

- **GX_SUCCESS** (0x00) Successful font table set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_STRING **language_table;
GX_UBYTE language_count;
UINT string_count;

/* Retrieve language table. */
status = gx_display_language_table_get_ext(&my_display, &language_table, &language_count, &string_count);

/* If status is GX_SUCCESS, the language table has been retrieved. */
```

### See Also

- gx_display_language_table_set_ext
- gx_display_active_langauge_set
- gx_display_string_get_ext

## gx_display_language_table_set

Assign display language table (deprecated)

### Prototype

```C
UINT gx_display_language_table_set(
    GX_DISPLAY *display,
    GX_CHAR **table, 
    GX_UBYTE number_of_languages, 
    UINT number_of_strings);
```

### Description

This service is deprecated and new applications should use gx_display_language_table_set_ext(). This service is supported only for compatibility with Studio generated resources files targeting library versions prior to version 5.6.

This service assigns the language table to be used by the display. This service is normally invoked by the GUIX Studio generated function gx_studio_display_configure, but can also be called by the application software.

### Parameters

- *display*: Pointer to display control block.
- *table*: Language table.
- *number_of_languages*: Number of columns in the provided table.
- *number_of_strings*: Number of strings in each table column.

### Return Values

- **GX_SUCCESS** (0x00) Successful font table set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_CHAR ***language_table= my_app_language_table;

/* Assign language table. */
status = gx_display_language_table_set(&my_display, language_table, 5, 232);

/* If status is GX_SUCCESS, the language table has been reassigned. */
```

### See Also

- gx_display_active_language_set
- gx_display_string_get

## gx_display_language_table_set_ext

Assign display language table

### Prototype

```C
UINT gx_display_language_table_set_ext(
    GX_DISPLAY *display,
    GX_STRING **table, 
    GX_UBYTE number_of_languages,
    UINT number_of_strings);
```

### Description

This service assigns the language table to be used by the display. This service is normally invoked by the GUIX Studio generated function gx_studio_display_configure, but can also be called by the application software.

Runtime language table assignment is usually done when languages are loaded from a binary resource file using gx_binres_language_table_load().

### Parameters

- *display*: Pointer to display control block.
- *table*: Language table.
- *number_of_languages*: Number of columns in the provided table.
- *number_of_strings*: Number of strings in each table column.

### Return Values

- **GX_SUCCESS** (0x00) Successful font table set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_STRING_LENGTH (0x34) Invalid string length.

### Allowed From

Initialization and threads

### Example

```C
GX_STRING **language_table = my_app_language_table;

/* Assign the language table. */
status = gx_display_language_table_set_ext(&my_display, language_table, 5, 132);

/* If status is GX_SUCCESS, the language table has been reassigned. */
```

### See Also

- gx_display_active_language_set
- gx_display_string_get

## gx_display_pixelmap_table_set

Assign display font table

### Prototype

```C
UINT gx_display_pixelmap_table_set(
    GX_DISPLAY *display,
    GX_PIXELMAP **pixelmap_table, 
    INT table_size);
```

### Description

This service re-assigns the pixelmap table to be used by the display. This service is normally invoked by the Studio generated display configuration function, but can also be called by the application software.

### Parameters

- *display*: Pointer to display control block.
- *pixelmap_table*: Array of GX_PIXELMAP pointers.
- *table_size*: Number of pixelmaps in table.

### Return Values

- **GX_SUCCESS** (0x00) Successful set pixelmap table.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_PIXELMAP *pixelmap_table[32] = { … };

/* Assign pixelmap table. */
status = gx_display_pixelmap_table_set(&my_display, pixelmap_table, 32);

/* If status is GX_SUCCESS the pixelmap table has been reassigned. */
```

### See Also

- gx_display_color_set
- gx_display_color_table_set
- gx_display_font_table_set

## gx_display_string_get

Retrieve a string from the active string table (deprecated)

### Prototype

```C
UINT gx_display_string_get(
    GX_DISPLAY *display,
    GX_RESOURCE_ID string_id, 
    GX_CONST GX_CHAR **string);
```

### Description

This service is deprecated in favor of gx_display_string_get_ext().

This service retrieves a string from the active string table for the indicated display. The active language is used to select the string from the language table assigned to the display.

String IDs are generated by GUIX Studio and are found in the application resources.h header file.

### Parameters

- *display*: Pointer to display control block.
- *string_id*: String ID, generated by GUIX Studio.
- *string*: Address of string pointer variable.

### Return Values

- **GX_SUCCESS** (0x00) Successful string retrieval.
- **GX_INVALID_RESOURCE_ID** (0X33) Invalid string ID.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid display pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_CHAR *string;

/* Retrieve string value. */
status = gx_display_string_get(&my_display, GX_STRING_ID_MONDAY, &string);

/* If status is GX_SUCCESS, the string has been retrieved. */
```


### See Also

- gx_display_active_language_set
- gx_display_language_table_set

## gx_display_string_get_ext

Retrieve a string from the active string table

### Prototype

```C
UINT gx_display_string_get_ext(
    GX_DISPLAY *display,
    GX_RESOURCE_ID string_id,
    GX_STRING *string);
```

### Description

This service retrieves a string from the active string table for the indicated display. The active language is used to select the string from the language table assigned to the display.

String IDs are generated by GUIX Studio and are found in the application resources.h header file.

### Parameters

- *display*: Pointer to display control block.
- *string_id*: String ID, generated by GUIX Studio.
- *string*: Address of GX_STRING variable in which *string.gx_string_ptr* and *string.gx_string_length* will be returned.

### Return Values

- **GX_SUCCESS** (0x00) Successful string retrieval.
- **GX_INVALID_RESOURCE_ID** (0X33) Invalid string ID.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid display pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_STRING string;

/* Retrieve string value. */
status = gx_display_string_get_ext(&my_display, GX_STRING_ID_MONDAY, &string);

/* If status is GX_SUCCESS, the string has been retrieved. */
```

### See Also

- gx_display_active_language_set
- gx_display_language_table_set


## gx_display_string_table_get


Retrieve the active string table (deprecated)

### Prototype

```C
UINT gx_display_string_table_get(
    GX_DISPLAY *display,
    GX_UBYTE language, 
    GX_CHAR ***table,
    UINT *table_size);
```

### Description

This service is deprecated and replaced by gx_display_string_table_get_ext().

This service retrieves the string table associated with the active language. This service is not frequently used, but is provided for completeness for those applications that might need to make runtime modifications to the string table.

### Parameters

- *display*: Pointer to display control block.
- *language*: Table column to retrieve.
- *table*: Address of variable to return pointer.
- *table_size*: Address of variable to return table size.

### Return Values

- **GX_SUCCESS** (0x00) Successful font table set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_NOT_FOUND (0x09) Invalid language index.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_CHAR **string_table;
UINT table_size;

/* Retrieve string table. */
status = gx_display_string_table_get(&my_display, LANGUAGE_ENGLISH,
    &string_table, &table_size);

/* If status is GX_SUCCESS, the string table has been retrieved. */
```

### See Also

- gx_display_color_set
- gx_display_color_table_set
- gx_display_pixelmap_table_set

## gx_display_string_table_get_ext


Retrieve the active string table

### Prototype

```C
UINT gx_display_string_table_get(
    GX_DISPLAY *display,
    GX_UBYTE language, 
    GX_STRING **table, 
    UINT *table_size);
```

### Description

This service retrieves the string table associated with the active language. This service is not frequently used, but is provided for completeness for those applications that might need to make runtime modifications to the string table.

### Parameters

- *display*: Pointer to display control block.
- *language*: Table column to retrieve.
- *table*: Address of variable to return pointer.
- *table_size*: Address of variable to return table size.

### Return Values

- **GX_SUCCESS** (0x00) Successful font table set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_NOT_FOUND (0x09) Invalid language index.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_STRING *string_table;
UINT table_size;

/* Retrieve string table. */
status = gx_display_string_table_get_ext(&my_display,
    LANGUAGE_ENGLISH, &string_table, &table_size);

/* If status is GX_SUCCESS, the string table has been retrieved. */
```

### See Also

- gx_display_color_set
- gx_display_color_table_set
- gx_display_pixelmap_table_set

## gx_display_theme_install


Install themes to the specified display

### Prototype

```C
UINT gx_display_theme_install(
    GX_DISPLAY *display, 
    GX_THEME *theme_table);
```

### Description

This service installs themes to the specified display. This service is normally invoked by the Studio generated display configuration function, but can also be called by the application software.

### Parameters

- *display*: Pointer to display control block.
- *theme_table*: Theme table to be installed.

### Return Values

- **GX_SUCCESS** (0x00) Successfully installed theme table.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid theme table pointer.
- GX_INVALID_DISPLAY (0x1D) Invalid display.

### Allowed From

Initialization and threads

### Example

```C
GX_THEME theme_1;

&theme_table[1] = {
    &theme_1,
    …
}

/* Define resource tables. */
GX_COLOR color_table[32] = {…};
GX_FONT *font_table[32] = {…};
GX_PIXELMAP *pixelmap_table[32] = { … };

/* Define scroll appearance. */
GX_SCROLLBAR_APPEARANCE scroll_appearance;

memset(&scroll_appearance, 0, sizeof(GX_SCROLLBAR_APPEARANCE));

scroll_appearance.gx_scroll_width = 20;
scroll_appearance.gx_scroll_thumb_width = 18;
scroll_appearance.gx_scroll_thumb_color =
    GX_COLOR_ID_SCROLL_BUTTON;
scroll_appearance.gx_scroll_thumb_border_color =
    GX_COLOR_ID_SCROLL_BUTTON;
scroll_appearance.gx_scroll_button_color =
    GX_COLOR_ID_SCROLL_BUTTON;
scroll_appearance.gx_scroll_thumb_travel_min = 20;
scroll_appearance.gx_scroll_thumb_travel_max = 20;
scroll appearance.gx_scroll_thumb_border_style =
    GX_STYLE_BORDER_THIN;

theme_1.theme_color_table = color_table;
theme_1.theme_font_table = font_table;
theme_1.theme_pixlemap_table = pixelmap_table;
theme_1.theme_palette = GX_NULL;
theme_1.theme_vertical_scrollbar_appearance = scroll_appearance;
theme_1.theme_horizontal_scrollbar_appearance = scroll_appearance;
theme_1.theme_vertical_scroll_style = GX_SCROLLBAR_RELATIVE_THUMB;
theme_1.theme_horizontal_scroll_style =
    GX_SCROLLBAR_RELATIVE_THUMB;
theme_1.theme_color_table_size = 32;
theme_1.theme_font_table_size = 32;
theme_1.theme_pixelmap_table_size = 32;
theme_1.theme_palette_size = 0;

/* Install theme table. */
status = gx_display_theme_install(&my_display, theme_table);

/* If status is GX_SUCCESS the theme table has been installed. */
```

### See Also

- gx_display_color_set
- gx_display_color_table_set
- gx_display_font_table_set

## gx_drop_list_close


Close a drop list

### Prototype

```C
UINT gx_drop_list_close(GX_DROP_LIST *drop_list);
```

### Description

This service closes the popup list of the specified drop list.

### Parameters

- *drop_list*: Pointer to the drop list control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful closed the drop list.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Close a drop list. */
status = gx_drop_list_close(&drop_list);

/* If status is GX_SUCCESS, the drop list is closed. */
```

### See Also

- gx_drop_list_create
- gx_drop_list_event_process
- gx_drop_list_open

gx_drop_list_pixelmap_set, gx_drop_list_popup_get

## gx_drop_list_create


Create a drop list

### Prototype

```C
UINT gx_drop_list_create(
    GX_DROP_LIST *drop_list, 
    GX_CONST
    GX_CHAR *name, 
    GX_WIDGET *parent,
    INT total_rows, 
    INT open_height,
    VOID (*callback)(GX_VERTICAL_LIST *, 
    GX_WIDGET *, INT),
    ULONG style, 
    USHORT drop_list_id,
    GX_CONST GX_RECTANGLE *size)
```

### Description

This service creates a drop list. A drop list is a combination of the drop list widget, and a popup vertical list that is displayed when the drop-list is opened. The popup vertical list is created automatically when the drop-list widget is created, and displayed or hidden when the drop-list widget is opened or closed, respectively.

The drop list widget supports two associated pixelmaps. The first, described as "List Wallpaper" in the Studio properties view, is the optional wallpaper pixelmap that is displayed as the background of the vertical list that is displayed when the drop-list widget is opened. The second pixelmap, described as the "Background Image" in the Studio properties view, is an optional image displayed as the background of the drop-list itself.

A drop-list widget can have (but is not required to have) a child widget that is used to open and close the drop list. This is customarily an icon or button widget, but even a custom widget could be used as the open/close toggle for the parent drop list. The key setting which makes this child widget operate the drop list is that this child widget must have the pre-defined widget ID ID_DROP_LIST_BUTTON.

To define a child widget which will open and close the drop-list, first add a child widget to the drop list, and position this child within the drop list as desired:

![Screenshot of the drop list.](./media/guix/image6.jpg)

Then use the Studio properties view to assign this child widget the ID value ID_DROP_LIST_BUTTON, which is an internally defined ID value recognized by the parent drop list:

![Screenshot of the Studio properties view.](./media/guix/image7.jpg)

### Parameters
- *drop_list*: Pointer to the drop list control block.
- *name*: Name of the drop list.
- *parent*: Pointer to the parent widget.
- *total_rows*: Total number of rows in the drop list.
- *open_height*: The height of the vertical list displayed when the drop list is opened.
- *callback*: Function called by the vertical list when the list is scrolled. Refer to the documentation of GX_VERTICAL_LIST for more information.
- *style*: The drop-down list border style.
- *drop_list_id*: Application-defined ID of the drop list.
- *size*: Dimensions of the drop list.

### Return Values

- **GX_SUCCESS** (0x00) Successful drop list create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.

### Allowed From

Initialization and threads

### Example

```C
GX_DROP_LIST my_drop_list;
VOID list_row_create(GX_VERTICAL_LIST *list,
                     GX_WIDGET *row, INT index);
{
    …
}

GX_RECTANGLE size;

gx_utility_rectangle_define(&size, 10, 10, 80, 40);

status = gx_drop_list_create(&my_drop_list,
    "my drop list", GX_NULL,
    10, 100, list_row_create,
    GX_STYLE_BORDER_RECESSED | GX_STYLE_ENABLED,
    ID_DROP_LIST, &size)

/* Is status is GX_SUCCESS, the drop list was successfully created. */
```

### See Also:

- gx_drop_list_close
- gx_drop_list_event_process
- gx_drop_list_open
- gx_drop_list_pixelmap_set
- gx_drop_list_popup_get

## gx_drop_list_event_process


Process drop list event

### Prototype

```C
UINT gx_drop_list_event_process(
    GX_DROP_LIST *list, 
    GX_EVENT *event);
```

### Description

This service processes an event for the drop list.

### Parameters

- *drop_list*: Drop list widget control block.
- *event*: Pointer to event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successfully processed drop list event.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Threads

### Example

```C
/* Call generic drop list event processing as part of custom event processing function. */

UINT custom_drop_list_event_process(GX_DROP_LIST *drop_list,
    GX_EVENT *event)
{
    UINT status = GX_SUCCESS;

    switch(event->gx_event_type)
    {
    case xyz:
        /* Insert custom event handling here. */
        break;
    default:
        /* Pass all other events to the default drop list event processing. */
        status = gx_drop_list_event_process(drop_list, event);
        break;
    }
    return status;
}
```

### See Also

- gx_drop_list_close
- gx_drop_list_create
- gx_drop_list_open
- gx_drop_list_pixelmap_set
- gx_drop_list_popup_get

## gx_drop_list_open


Open a drop list

### Prototype

```C
UINT gx_drop_list_open(GX_DROP_LIST *drop_list);
```

### Description

This service opens a previously created drop list.

### Parameters

- *drop_list*: Pointer to the drop list control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful drop list create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_DROP_LIST mylist;

/* Open the popup list of "mylist". */
status = gx_drop_list_open(&mylist);

/* If status == GX_SUCCESS, the popup list of "mylist" has been displayed. */
```

### See Also

- gx_drop_list_close
- gx_drop_list_create
- gx_drop_list_event_process
- gx_drop_list_pixelmap_set
- gx_drop_list_popup_get

## gx_drop_list_pixelmap_set


Assign a background image to the drop list

### Prototype

```C
UINT gx_drop_list_pixelmap_set(
    GX_DROP_LIST *drop_list,
    GX_RESOURCE_ID id);
```

### Description

Assign a background image to the drop list. This pixelmap is used as the background for the drop list widget itself, and not for the popup vertical list that is displayed when the list is opened. To assign a pixelmap to the drop-list popup, you would need to first call gx_drop_list_popup_get to retrieve a pointer to the popup list, and then use gx_window_wallpaper_set() to assign a pixelmap to this popup list.

### Parameters

- *drop_list*: Pointer to the drop list control block.
- *id*: Resource ID to the pixelmap.

### Return Values

- **GX_SUCCESS** (0x00) Successful drop list pixelmap set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_DROP_LIST mylist;

/* Create the drop list here. */

/* Assign a pixelmap id, which will turn on the list button. */
status = gx_drop_list_pixelmap_set(&mylist,
    GX_PIXELMAP_ID_DROP_LIST_BACKGROND);

/* If status is GX_SUCCESS, the pixelmap was successfully set. */
```

### See Also:

- gx_drop_list_close
- gx_drop_list_create
- gx_drop_list_event_process
- gx_drop_list_open
- gx_drop_list_popup_get

## gx_drop_list_popup_get


Retrieve pointer to popup vertical list

### Prototype

```C
UINT gx_drop_list_popup_get(
    GX_DROP_LIST *drop_list,
    GX_VERTICAL_LIST **return_list);
```

### Description

A drop-list widget is composed of the drop-list widget itself, and a popup vertical list that is shown when the drop-list widget is opened. This service retrieves a pointer to the vertical list component of the drop list, allowing the application to invoke API services directly on this vertical list.

### Parameters

- *drop_list*: Pointer to the drop list control block.
- *return_list*: Pointer to the vertical list stored in the drop list.

### Return Values

- **GX_SUCCESS** (0x00) Successfully retrieved popup vertical list.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_DROP_LIST drop_list;
GX_VERTICAL_LIST *vertical_list;

/* Retrieve the popup list of "drop_list". */
status = gx_drop_list_popup_get(&drop_list, &vertical_list)

/* If status is GX_SUCCESS, the popup list was successfully retrieved. */
```

### See Also:

- gx_drop_list_close
- gx_drop_list_create
- gx_drop_list_open
- gx_drop_list_pixelmap_set

## gx_generic_scroll_wheel_children_position

Position children for the generic scroll wheel

### Prototype

```C
UINT gx_generic_scroll_wheel_children_position(
    GX_GENERIC_SCROLL_WHEEL *wheel)
```

### Description

This function positions the children for the generic scroll wheel according to the generic scroll wheel row height. This function is called by default when the generic scroll wheel is shown.

### Parameters

- *wheel*: Pointer to the generic scroll wheel control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully positioned the children for the generic scroll wheel.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Position children in the generic scroll wheel. */
status = gx_generic_scroll_wheel_children_position (&wheel);

/* If status is GX_SUCCESS the children in the generic scroll wheel are positioned. */
```

### See Also

- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_generic_scroll_wheel_create
- gx_generic_scroll_wheel_draw
- gx_generic_scroll_wheel_event_process
- gx_generic_scroll_wheel_row_height_set
- gx_generic_scroll_wheel_total_rows_set

## gx_generic_scroll_wheel_create


Create generic scroll wheel

### Prototype

```C
UINT gx_generic_scroll_wheel_create(
    GX_GENERIC_SCROLL_WHEEL *wheel,
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent,
    INT total_rows,
    VOID (*callback)(GX_GENERIC_SCROLL_WHEEL *, GX_WIDGET *, INT),
    ULONG style,
    USHORT id,
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a generic scroll wheel widget.

A generic scroll wheel is a type of scroll wheel widget that is consisted of child widgets. Other types of scroll wheel widgets are also available. For more information about the scroll wheel widget hierarchy, widget types, and widget derivation, see the gx_scroll_wheel_create() API.

GX_GENERIC_SCROLL_WHEEL is derived from GX_SCROLL_WHEEL and supports all gx_scroll_wheel services.

All scroll wheel types generate GX_EVENT_LIST_SELECT events to their parent when the scroll wheel is scrolled.

### Parameters

- *wheel*: Pointer to generic scroll wheel control block.
- *name*: Logical name of generic scroll wheel widget.
- *parent*: Pointer to the parent widget.
- *total_rows*: Total rows of the scroll wheel.
- *callback*: Callback function to create a widget row. It can be GX_NULL if the number of total rows matches the child count. It should be provided for widget reuse when the child count is less than the number of total rows or GX_STYLE_WRAP style is set. And in this case make sure the child count is 1 more than the number of visible rows.
- *style*: Style of generic scroll wheel. **Appendix D** contains pre-defined general styles for all widgets and widget-specific styles.
- *id*: Application-defined ID of generic scroll wheel.
- *size*: Dimensions of generic scroll wheel widget.

### Return Values

- **GX_SUCCESS** (0x00) Successfully created generic scroll wheel.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.
- GX_INVALID_VALUE (0x22) Invalid number of rows.
- GX_INVALID_WIDGET (0x12) Invalid parent widget.

### Allowed From

Initialization and threads

### Example

```C

/* Define visible rows. */
#define VISIBLE_ROWS 5

/* Define row memory. */
GX_NUMERIC_PROMPT row_memory[VISIBLE_ROWS + 1];

/* Define callback function. */
VOID row_create(GX_GENERIC_SCROLL_WHEEL *wheel, GX_WIDGET *widget, INT index)
{
GX_NUMERIC_PROMPT *row = (GX_PROMPT *)widget;
GX_BOOL result;
GX_RECTANGLE size;

    gx_widget_created_test(widget, &result);

    if(!result)
    {
        gx_numeric_prompt_create(row, "", wheel, 0, GX_STYLE_ENABLED, 0, &size);
    }

    gx_numeric_prompt_value_set(row, index);
}

/* Create "generic_wheel" with 20 rows. */
status = gx_generic_scroll_wheel_create(&generic_wheel, "generic_wheel", &parent, 20, row_create,
                                       GX_STYLE_ENABLED, WHEEL_ID, &size);

/* If status is GX_SUCCESS the generic scroll wheel "generic_wheel"" has been created. */

/* Create children for generic scroll wheel. */
for(index = 0; index <= VISIBLE_ROWS; index++)
{
    row_create(generic_wheel, (GX_WIDGET *)&row_memory[index], index);
}
```

### See Also

- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_generic_scroll_wheel_children_position
- gx_generic_scroll_wheel_draw
- gx_generic_scroll_wheel_event_process
- gx_generic_scroll_wheel_row_height_set
- gx_generic_scroll_wheel_total_rows_set

## gx_generic_scroll_wheel_draw

Draw window

### Prototype

```C
VOID gx_generic_scroll_wheel_draw(GX_GENERIC_SCROLL_WHEEL *wheel);
```

### Description

This service draws a generic scroll wheel. This service is normally called
internally during canvas refresh, but can also be called from custom
generic scroll wheel drawing functions.

### Parameters

- *wheel*: Pointer to generic scroll wheel control block.

### Return Values

None

### Allowed From

Threads

### Example

```C
/* Write a custom generic scroll wheel draw function. */

VOID my_custom_generic_scroll_wheel_draw(GX_GENERIC_SCROLL_WHEEL *wheel)
{
    /* Call default generic scroll wheel draw. */
    gx_generic_scroll_wheel_draw(wheel);

    /* Add your own drawing here. */
}
```

### See Also

- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_generic_scroll_wheel_children_position
- gx_generic_scroll_wheel_create
- gx_generic_scroll_wheel_event_process
- gx_generic_scroll_wheel_row_height_set
- gx_generic_scroll_wheel_total_rows_set

## gx_generic_scroll_wheel_event_process

Process generic scroll wheel event

### Prototype

```C
UINT gx_generic_scroll_wheel_event_process(
    GX_GENERIC_SCROLL_WHEEL *wheel, 
    GX_EVENT *event);
```

### Description

This service processes an event for this window.

### Parameters

- *wheel*: Pointer to generic scroll wheel control block.
- *event*: Pointer to event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful generic scroll wheel event processing.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Call generic generic scroll wheel event processing as part of custom event processing function. */

UINT custom_generic_scroll_wheel_event_process(GX_GENERIC_SCROLL_WHEEL *wheel,
                                               GX_EVENT *event)
{
    UINT status = GX_SUCCESS;

    switch(event->gx_event_type)
    {
        case xyz:

            /* Insert custom event handling here. */
            break;

        default:

            /* Pass all other events to the default generic scroll wheel event processing. */
            status = gx_generic_scroll_wheel_event_process(wheel, event);
            break;
        }
        return status;
}
```

### See Also

- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_generic_scroll_wheel_children_position
- gx_generic_scroll_wheel_create
- gx_generic_scroll_wheel_draw
- gx_generic_scroll_wheel_row_height_set
- gx_generic_scroll_wheel_total_rows_set

## gx_generic_scroll_wheel_row_height_set


Assign the row height for each wheel row

### Prototype

```C
UINT gx_generic_scroll_wheel_row_height_set(
    GX_GENERIC_SCROLL_WHEEL *wheel,
    GX_VALUE row_height);
```

### Description

This service assigns the row height for each row of the scroll wheel.

### Parameters

- *wheel*: Pointer to the generic scroll wheel control block.
- *row_height*: Row height value, in pixels.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set scroll wheel height.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_VALUE (0x22) Invalid row height.

### Allowed From

Initialization and threads

### Example

```C
status = gx_generic_scroll_wheel_row_height_set(&wheel, 40);

/* If status == GX_SUCCESS the wheel row height has been set to 40 pixels. */
```

### See Also

- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_generic_scroll_wheel_children_position
- gx_generic_scroll_wheel_create
- gx_generic_scroll_wheel_draw
- gx_generic_scroll_wheel_event_process
- gx_generic_scroll_wheel_total_rows_set

## gx_generic_scroll_wheel_total_rows_set


Assign the total scroll wheel rows available

### Prototype

```C
UINT gx_generic_scroll_wheel_total_rows_set(
    GX_GENERIC_SCROLL_WHEEL *wheel,
    INT total_rows);
```

### Description

This service assigns or changes the total number of generic scroll wheel rows.

### Parameters

- *wheel*: Pointer to generic scroll wheel control block.
- *total_rows*: Total number of wheel rows to present to the user.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set generic scroll wheel total row.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_VALUE (0x22) Invalid number of total rows.

### Allowed From

Initialization and threads

### Example

```C
status = gx_generic_scroll_wheel_total_rows_set(&wheel, 20);

/* If status == GX_SUCCESS the scroll wheel has been changed to display 20 total rows. */
```
### See Also

- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_generic_scroll_wheel_children_position
- gx_generic_scroll_wheel_create
- gx_generic_scroll_wheel_draw
- gx_generic_scroll_wheel_event_process
- gx_generic_scroll_wheel_row_height_set

## gx_horizontal_list_children_position


Position children for the horizontal list

### Prototype

```C
UINT gx_horizontal_list_children_position(GX_HORIZONTAL_LIST *horizontal_list);
```

### Description

This function positions the children for the horizontal list. This function is called automatically when the list receives the GX_EVENT_SHOW event, but should be called directly if the list is modified after it has been made visible.

### Parameters

- *horizontal_list*: Pointer to the horizontal list control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful positioned the children for the horizontal list.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Position children in the horizontal list. */
status = gx_horizontal_list_children_position (&horizontal_list);

/* If status is GX_SUCCESS the children in the horizontal list are positioned. */
```

### See Also

- gx_horizontal_list_create
- gx_horizontal_list_event_process
- gx_horizontal_list_page_index_set
- gx_horizontal_list_selected_index_get
- gx_horinzontal_list_selected_widget_get
- gx_horizontal_list_selected_set
- gx_horizontal_list_total_columns_set

## gx_horizontal_list_create


Create horizontal list

### Prototype

```C
UINT gx_horizontal_list_create(
    GX_HORIZONTAL_LIST *horizontal_list,
    GX_CONST GX_CHAR *name, 
    GX_WIDGET *parent,
    INT total_columns,
    VOID (*callback)(GX_HORIZONTAL_LIST *, 
    GX_WIDGET *, INT),
    ULONG style, 
    USHORT horizontal_list_id,
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a horizontal list.

### Parameters

- *horizontal_list*: Horizontal list widget control block.
- *name*: Name of horizontal list.
- *parent*: Pointer to parent widget.
- *total_columns*: Total number of columns in horizontal list.
- *callback*: This is a pointer to a callback function provided by the application. The callback function is invoked when the horizontal list is scrolled, to create the newly visible list items. In this way the horizontal list can display any user-defined widget type as the list items.
- *style*: Style of scrollbar widget. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *horizontal_list_id*: Application-defined ID of horizontal list.
- *size*: Dimensions of the horitzonal list.

### Return Values

- **GX_SUCCESS** (0x00) Successfully created the horizontal list.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.
- **GX_INVALID_VALUE** (0x22) Number of columns not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Create horizontal list "my_list" with 5 columns. */
status = gx_horizontal_list_create(&my_list, "my_list", &my_parent,
    5, callback, GX_STYLE_WRAP, MY_LIST_ID, &size);

/* If status is GX_SUCCESS the horizontal list "my_list" has been created. */
```

### See Also

- gx_horizontal_list_children_position
- gx_horizontal_list_event_process
- gx_horizontal_list_page_index_set
- gx_horizontal_list_selected_index_get
- gx_horinzontal_list_selected_widget_get
- gx_horizontal_list_selected_set
- gx_horizontal_list_total_columns_set

## gx_horizontal_list_event_process


Process horizontal list event

### Prototype

```C
UINT gx_horizontal_list_event_process(
    GX_HORIZONTAL_LIST *list,
    GX_EVENT *event);
```

### Description

This service processes an event for the horizontal list.

### Parameters

- *list*: Horizontal list widget control block.
- *event*: Pointer to event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successfully processed horizontal list event.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Call generic horizontal list event processing as part of custom event processing function. */

UINT custom_list_event_process(
    GX_HORIZONTAL_LIST *list,
    GX_EVENT *event)
{
    UINT status = GX_SUCCESS;

    switch(event->gx_event_type)
    {
    case xyz:
        /* Insert custom event handling here. */
        break;
    default:
        /* Pass all other events to the default horizontal
        list event processing. */
        status =
        gx_horizontal_list_event_process(list, event);
        break;
    }
    return status;
}
```

### See Also

- gx_horizontal_list_children_position
- gx_horizontal_list_create
- gx_horizontal_list_page_index_set
- gx_horizontal_list_selected_index_get
- gx_horinzontal_list_selected_widget_get
- gx_horizontal_list_selected_set
- gx_horizontal_list_total_columns_set

## gx_horizontal_list_page_index_set


Set starting page index

### Prototype

```C
UINT gx_horizontal_list_page_index_set(
    GX_HORIZONTAL_LIST *list,
    INT *index);
```

### Description

This service sets the starting index for the horizontal list.

### Parameters

- *list*: Horizontal list widget control block.
- *index*: The new top index.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set starting page index for the horizontal list.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- **GX_INVALID_VALUE** (0x22) Invalid value.

### Allowed From

Initialization and threads

### Example

```C
/* Sets the starting page index of horizontal list "my_list" as 1. */
status = gx_horizontal_list_page_index_set(&my_list, 1);

/* If status is GX_SUCCESS the starting page index of "my_list" has been set. */
```

### See Also

- gx_horizontal_list_children_position
- gx_horizontal_list_create
- gx_horizontal_list_event_process
- gx_horizontal_list_selected_index_get
- gx_horinzontal_list_selected_widget_get
- gx_horizontal_list_selected_set
- gx_horizontal_list_total_columns_set

## gx_horizontal_list_selected_index_get


Get selected entry index from horizontal list

### Prototype

```C
UINT gx_horizontal_list_selected_index_get(
    GX_horizobntal_LIST *horizontal_list,
    INT *return_index);
```

### Description

This service returns the selected list entry index of the horizontal list.

### Parameters

- *horizontal_list*: Horizontal list widget control block.
- *return_index*: Destination for return list index.

### Return Values

- **GX_SUCCESS** (0x00) Successful obtained the horizontal list entry.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
INT current_index_entry;

/* Get the list entry at the current index of horizontal list "my_list". */
status = gx_horizontal_list_selected_index_get(&my_list,
    &current_index_entry);

/* If status is GX_SUCCESS, "current_index_entry" contains the current list selection index. */
```

### See Also

- gx_horizontal_list_children_position
- gx_horizontal_list_create
- gx_horizontal_list_event_process
- gx_horizontal_list_page_index_set
- gx_horinzontal_list_selected_widget_get
- gx_horizontal_list_selected_set
- gx_horizontal_list_total_columns_set

## gx_horizontal_list_selected_set


Assign the selected entry in a horizontal list

### Prototype

```C
UINT gx_horizontal_list_selected_set(
    GX_HORIZONATAL_LIST *horizontal_list,
    INT index);
```

### Description

This service assigns the selected entry in a horizontal list. If required, the horizontal list will scroll to make the selected entry visible.

### Parameters

- *horizontal_list*: Horizontal list widget control block.
- *index*: Index based position of new list entry.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set the horizontal list entry.
- **GX_FAILURE** (0x10) Index is not in invalid range.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set the list entry of "my_list" to the child in line 12. */
status = gx_horizontal_list_selected_set(&my_list, 12);

/* If status is GX_SUCCESS, the list entry of "my_list" has been successfully set to 12. */
```

### See Also

- gx_horizontal_list_children_position
- gx_horizontal_list_create
- gx_horizontal_list_event_process
- gx_horizontal_list_page_index_set
- gx_horizontal_list_selected_index_get
- gx_horinzontal_list_selected_widget_get
- gx_horizontal_list_total_columns_set

## gx_horizontal_list_selected_widget_get


Get selected entry from horizontal list

### Prototype

```C
UINT gx_horizontal_list_selected_widget_get(
    GX_horizobntal_LIST *horizontal_list,
    GX_WIDGET **return_list_entry);
```

### Description

This service returns the selected list entry of the horizontal list. Note that if the horizontal list has more rows than child widgets, and the selected entry has been scrolled from view, this API will return GX_NULL because the child widgets are re-used as the list content is scrolled. The gx_horizontal_list_selected_index_get function will reliably return the index of the selected item, even if that item has been scrolled from view.

### Parameters

- *horizontal_list*: Horizontal list widget control block.
- *return_list_entry*: Destination for return list entry widget.

### Return Values

- **GX_SUCCESS** (0x00) Successful obtained the horizontal list entry.
- **GX_FAILURE** (0x10) Selected widget has been scrolled from view in a list with more rows than client children.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Get the list entry at the current index of horizontal list "my_list". */
status = gx_horizontal_list_selected_widget_get(&my_list, &current_list_entry);

/* If status is GX_SUCCESS, "current_list_entry" contains a pointer to the currently selected widget. */

    
```

### See Also

- gx_horizontal_list_children_position
- gx_horizontal_list_create
- gx_horizontal_list_event_process
- gx_horizontal_list_page_index_set
- gx_horizontal_list_selected_index_get
- gx_horizontal_list_selected_set
- gx_horizontal_list_total_columns_set

## gx_horizontal_list_total_columns_set


Assign the total number of list columns

### Prototype

```C
UINT gx_horizontal_list_total_columns_set(
    GX_HORIZONTAL_LIST *horizontal_list,
    INT count);
```

### Description

This service assigns the total number of columns to be displayed by the horizontal list.

### Parameters

- *horizontal_list*: Horizontal list widget control block.
- *count*: Number of columns to display.

### Return Values

- **GX_SUCCESS** (0x00) Successful assigned column count.
- GX_CALLER_ERROR (0x10) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_VALUE (0x22) Invalid count value.

### Allowed From

Initialization and threads

### Example

```C
/* Tell my list to display 20 total columns. */
status = gx_horizontal_list_total_columns_set(&my_list, 20);

/* If status is GX_SUCCESS, the total colums of "my_list" was successfully set to 20. */
```

### See Also

- gx_horizontal_list_children_position
- gx_horizontal_list_create
- gx_horizontal_list_event_process
- gx_horizontal_list_page_index_set
- gx_horizontal_list_selected_index_get
- gx_horinzontal_list_selected_widget_get
- gx_horizontal_list_selected_set

## gx_horizontal_scrollbar_create


Create horizontal scrollbar

### Prototype

```C
UINT gx_horizontal_scrollbar_create(
    GX_SCROLLBAR *scrollbar,
    GX_CONST GX_CHAR *name,
    GX_WINDOW *parent,
    GX_SCROLLBAR_APPEARANCE *appearance ULONG style);
```

### Description

This service creates a horizontal scrollbar. The ID for a horizontal scrollbar is pre-defined (because a window has to know how to catch events from it), and the size is automatic (because it has to fill the parent window's client width). If we decide to allow client area scrollbars, we will need to add another create function with the ID and size parameters.

### Parameters

- *scrollbar*: Scrollbar widget control block.
- *name*: Name of scrollbar.
- *parent*: Pointer to parent widget.
- *appearance*: The appearance structure defines the appearance of the scroll bar. If this value is GX_NULL, the scrollbar will use the default scrollbar appearance defined by gx_system_scroll_appearance_get. Refer to **Appendix I** for the definition of the GX_SCROLLBAR_APPEARANCE structure.
- *Style*: Style of scrollbar widget. **Appendix D** contains pre- defined general styles for all widgets as well as widget-specific styles.

### Return Values

- **GX_SUCCESS** (0x00) Successful horizontal scrollbar create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.

### Allowed From

Initialization and threads

### Example

```C
/* Create horizontal scrollbar "my_scrollbar". */
status = gx_horizontal_scrollbar_create(&my_scrollbar,
    "my_horizontal_scrollbar", &my_parent, GX_NULL,
    GX_STYLE_SCROLLBAR_END_BUTTONS);

/* If status is GX_SUCCESS the horizontal scrollbar "my_scrollbar" has been created. */
```

### See Also

- gx_scrollbar_draw
- gx_scrollbar_event_process
- gx_scrollbar_limit_check
- gx_scrollbar_reset
- gx_vertical_scrollbar_create

## gx_icon_button_create


Create icon button

### Prototype

```C
UINT gx_icon_button_create(
    GX_ICON_BUTTON *button,
    GX_CONST GX_CHAR *name, 
    GX_WIDGET *parent,
    GX_RESOURCE_ID icon_id,
    ULONG style,
    USHORT icon_button_id,
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates the specified icon button widget.

GX_ICON_BUTTON is derived from GX_BUTTON and supports all gx_button API services.

### Parameters

- *button*: Pointer to icon button control block.
- *name*: Logical name of icon button widget.
- *parent*: Pointer to the parent widget.
- *icon_id*: Resource ID of icon.
- *style*: Style of icon. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *icon_button_id*: Application-defined ID of icon button.
- *size*: Dimensions of icon button.

### Return Values

- **GX_SUCCESS** (0x00) Successfully created icon button.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.

### Allowed From

Initialization and threads

### Example

```C
/* Create "my_icon_button". */
status = gx_icon_button_create(&my_icon_button, "my_icon_button",
    &my_parent,
    MY_ICON_RESOURCE_ID, GX_STYLE_BORDER_RAISED, MY_ICON_BUTTON_ID,
    &size);

/* If status is GX_SUCCESS the icon button "my_icon_button" has been created. */
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_icon_create
- gx_icon_draw
- gx_icon_pixelmap_set
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_radio_button_create
- gx_radio_button_draw gx_text_button_create
- gx_text_button_color_set
- gx_text_button_draw

## gx_icon_button_draw


Draw an icon button

### Prototype

```C
VOID gx_icon_button_draw(GX_ICON_BUTTON *button);
```

### Description

This service draws the icon button. This function is normally called internally by GUIX as part of a canvas refresh operation, but it also exposed to the application that might want to provide a custom drawing function and invoke the default icon button drawing as custom drawing base.

### Parameters

- *button*: Pointer to icon button control block.

### Return Values

None

### Allowed From

Threads

### Example

```C
/* Write a custom icon draw function. */
void MyIconButtonDraw(GX_ICON_BUTTON *button)
{
    /* Do the normal drawing first. */
    gx_icon_button_draw(button);

    /* Add custom drawing here. */
}
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_icon_create
- gx_icon_draw
- gx_icon_pixelmap_set
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_radio_button_create
- gx_radio_button_draw gx_text_button_create
- gx_text_button_color_set
- gx_text_button_draw

## gx_icon_button_pixelmap_set


Set pixelmap to the icon button widget

### Prototype

```C
UINT gx_icon_button_pixelmap_set(
    GX_ICON_BUTTON *button,
    GX_RESOURCE_ID icon_id);
```

### Description

This service assigns a new pixelmap to the icon button widget.

### Parameters

- *button*: Pointer to icon button control block.
- *icon_id*: Resource ID of pixelmap.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set icon button pixelmap.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set pixelmap for "my_icon_button". */
status = gx_icon_button_pixelmap_set(&my_icon_button,
    GX_PIXELMAP_ID_ICON);

/* If status is GX_SUCCESS, the pixelmap of the icon button "my_icon_button" has been set. */
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_icon_create
- gx_icon_draw
- gx_icon_pixelmap_set
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_radio_button_create
- gx_radio_button_draw gx_text_button_create
- gx_text_button_color_set
- gx_text_button_draw

## gx_icon_background_draw


Draw icon background

### Prototype

```C
VOID gx_icon_background_draw(GX_ICON *icon);
```

### Description

This service draws background of the specified icon widget. This service is normally called internally by the gx_icon_button_draw function, but is exposed to the application to assist in writing custom drawing functions.

### Parameters

- *icon*: Pointer to icon widget control block.

### Return Values

None

### Allowed From

Initialization and threads

### Example

```C
/* Write a custom icon draw function. */
void MyIconButtonDraw(GX_ICON *icon)
{
    /* Call icon background draw. */
    gx_icon_background_draw(icon);

    /* Add custom drawing here. */

    /* Draw child widgets. */
    gx_widget_children_draw(icon);
}
```

### See Also

- gx_icon_create
- gx_icon_draw
- gx_icon_event_process
- gx_icon_pixelmap_set

## gx_icon_create


Create icon

### Prototype

```C
UINT gx_icon_create(
    GX_ICON *icon, 
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent,
    GX_RESOURCE_ID pixelmap_id, 
    ULONG style,
    USHORT icon_id, 
    GX_VALUE x, 
    GX_VALUE y);
```

### Description

This service creates the specified icon widget.

### Parameters

- *icon*: Pointer to icon control block.
- *name*: Logical name of icon widget.
- *parent*: Pointer to the parent widget.
- *pixelmap_id*: Resource ID of pixelmap.
- *style*: Style of icon.
- *icon_id*: Application-defined ID of icon.
- *x*: Starting x-coordinate position.
- *y*: Starting y-coordinate position.

### Return Values

- **GX_SUCCESS** (0x00) Successful icon create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.

### Allowed From

Initialization and threads

### Example

```C
/* Create "my_icon". */
status = gx_icon_create(&my_icon, "my_icon", &my_parent,
    MY_PIXELMAP_RESOURCE_ID, GX_STYLE_BORDER_RAISED, MY_ICON_ID,
    5, 30);

/* If status is GX_SUCCESS the icon "my_icon" has been created. */
```

### See Also

- gx_icon_button_create
- gx_icon_draw
- gx_icon_pixelmap_set

## gx_icon_draw


Draw icon

### Prototype

```C
VOID gx_icon_draw(GX_ICON *icon);
```

### Description

This service draws the specified icon widget. This service is normally called internally by GUIX as part of a canvas refresh operation, but it also exposed to the application that might want to provide a custom drawing function and invoke the default icon drawing as custom drawing base.

### Parameters

- *icon*: Pointer to icon widget control block.

### Return Values

None

### Allowed From

Threads

### Example

```C
/* Write a custom icon drawing function. */

VOID my_icon_draw(GX_MENU *menu)
{
    /* Call default icon draw. */
    gx_icon_draw(menu);

    /* Add your own drawing here. */
}
```

### See Also

- gx_icon_button_create
- gx_icon_create
- gx_icon_pixelmap_set

## gx_icon_event_process

Icon widget event processing

### Prototype

```C
UINT gx_icon_event_process(
    GX_ICON *icon, 
    GX_EVENT *event_ptr);
```

### Description

This service handles events sent to a GX_ICON widget.

### Parameters

- *icon*: Pointer to icon widget control block.
- *event_ptr*: Pointer to GX_EVENT structure.

### Return Values

- **GX_SUCCESS** (0x00) Successful processed icon event.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Threads

### Example

```C
switch(event_ptr->gx_event_type)
{
case GX_EVENT_SHOW:
    /* Do default handling. */
    status = gx_icon_event_process(icon, event_ptr);

    /* Add your own handling here. */
    break;
}
```

### See Also

- gx_icon_button_create
- gx_icon_create
- gx_icon_pixelmap_set

## gx_icon_pixelmap_set


Set pixelmap for icon

### Prototype

```C
UINT gx_icon_pixelmap_set(
    GX_ICON *icon, 
    GX_RESOURCE_ID normal_id,
    GX_RESOURCE_ID selected_id);
```

### Description

This service sets the pixelmap for the specified icon widget.

### Parameters

- *icon*: Pointer to icon widget control block.
- *normal_id*: Normal state Resource ID.
- *selected_id*: Selected state Resource ID.

### Return Values

- **GX_SUCCESS** (0x00) Successful icon pixelmap set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set pixelmap for "my_icon". */
status = gx_icon_pixelmap_set(&my_icon,
    MY_NOT_SELECTED_RESOURCE_ID,
    MY_SELECTED_ID);

/* If status is GX_SUCCESS, the icon "my_icon" has a pixelmap set. */
```

### See Also

- gx_icon_button_create
- gx_icon_create
- gx_icon_draw

## gx_image_reader_create


Create image reader module instance

### Prototype

```C
UINT gx_image_reader_create(
    GX_IMAGE_READER *image_reader,
    GX_CONST GX_UBYTE *data, 
    INT data_size,
    GX_UBYTE color_format, 
    GX_UBYTE mode);
```

### Description

This function creates a runtime raw image reader / decoder. Currently only jpeg and png raw image types are supported. This service requires GX_SOFTWARE_DECODER_SUPPORT to be defined.

### Parameters

- *image_reader*: Image reader control block.
- *data*: Pointer to raw input data.
- *data_size*: Size of raw input data.
- *color_format*: The requested output color format.
- *mode*: Compress, dither and alpha modes flags.

### Return Values

- **GX_SUCCESS** (0x00) Successful image reader create.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_VALUE (0x22) Invalid data size.

### Allowed From

Initialization and Threads

### Example

```C
GX_IMAGE_READER my_image_reader;

"color format" could be set to:
    GX_COLOR_FORMAT_32ARGB
    GX_COLOR_FORMAT_24RGB
    GX_COLOR_FORMAT_565RGB
    GX_COLOR_FORMAT_8BIT_PALETTE

/* Create image reader "my_image_reader". */
status = gx_image_reader_create(&my_image_reader, decoder_data,
    decoder_data_size,
    GX_COLOR_FORMAT_32ARGB,
    GX_IMAGE_READER_MODE_COMPRESS)

/* If status is GX_SUCCESS, "my_image_reader" has been successfully created. */
```

### See Also

- gx_image_reader_start
- gx_image_reader_palette_set

## gx_image_reader_palette_set


Define image reader palette

### Prototype

```C
UINT gx_image_reader_palette_set(
    GX_IMAGE_READER *image_reader,
    GX_COLOR *pal,
    UINT palsize);
```

### Description

This service sets palette for image reader control block. This service requires GX_SOFTWARE_DECODER_SUPPORT to be defined.

### Parameters

- *image_reader*: Image reader control block.
- *pal*: Pointer to palette.
- *palsize*: The size of the palette.

### Return Values

- **GX_SUCCESS** (0x00) Successful image reader palette set.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_VALUE (0x22) Invalid palette size.

### Allowed From

Initialization and Threads

### Example

```C
/* Set "my_palette" as the palette of "my_image_reader". */
status = gx_image_reader_palette_set(&my_image_reader, my_palette,
    my_palette_size);

/* If status is GX_SUCCESS the palette of "my_image_reader" has been set to "my_palette". */
```

### See Also

- gx_image_reader_create
- gx_image_reader_start

## gx_image_reader_start

Start the decompress and conversion process

### Prototype

```C
UINT gx_image_reader_start(
    GX_IMAGE_READER *image_reader, 
    GX_PIXELMAP *outmap);
```

### Description

This service decodes a raw image to a specified color format. Currently only jpeg and png raw image types are supported. This requires GX_SOFTWARE_DECODER_SUPPORT to be defined.

### Parameters

- *image_reader*: Image reader control block.
- *pixelmap*: Output pixelmap.

### Return Values

- **GX_SUCCESS** (0x00) Successful image decoding.
- **GX_SYSTEM_MEMORY_ERROR** (0x30) Memory allocator is not defined or memory allocation failed.
- **GX_NOT_SUPPORTED** (0x28) Input image type or output color format is not supported.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and Threads

### Example

```C
GX_IMAGE_READER my_image_reader;
GX_PIXELMAP output_map;

/* Create my_image_reader here. */

/* Decoding a raw image according to the settings of "my_image_reader". */
status = gx_image_reader_start(&my_image_reader, output_map)

/* If status is GX_SUCCESS "output_map" have been loaded with the requested pixelmap. */
```

### See Also

- gx_image_reader_create
- gx_image_reader_palette_set

## gx_line_chart_axis_draw


Draw line chart x,y axis

### Prototype

```C
VOID gx_line_chart_axis_draw(GX_LINE_CHART *chart);
```

### Description

This service draws the x,y axis of a line chart. The axis colors and line width parameters are retrieved from the line chart information structure.

This service is normally called internally by the gx_line_chart_draw function, but is exposed to the application to assist in writing custom drawing functions.

### Parameters

- *chart*: Line chart control block.

### Return Values

None

### Allowed From

Threads

### Example

```C
/* Write a custom chart drawing function. */

VOID my_chart_draw(GX_LINE_CHART *chart)
{
    /* Call default window draw. */
    gx_window_draw((GX_WINDOW *)chart);

    /* Draw the line chart axis. */
    gx_line_chart_axis_draw(&chart->gx_line_chart_info);

    /* Draw the data line. */
    gx_line_chart_data_draw(&chart->gx_line_chart_info);

    /* Add your own drawing here. */
}
```

### See Also

- gx_line_chart_create
- gx_line_chart_data_draw
- gx_line_chart_draw
- gx_line_chart_update
- gx_line_chart_y_scale_calculate

## gx_line_chart_create


Create GX_LINE_CHART widget

### Prototype

```C
UINT gx_line_chart_create(
    GX_LINE_CHART *chart,
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent,
    GX_LINE_CHART_INFO *info,
    ULONG style,
    USHORT chart_id, GX_RECTANGLE *size)
```

### Description

This service creates a line chart window. The chart drawing parameters and chart data are passed in via the GX_LINE_CHART_INFO structure.

GX_LINE_CHART is based on GX_WINDOW, and supports all of the GX_WINDOW APIs.

### Parameters

- *chart*: Pointer to the GX_LINE_CHART control block.
- *name*: Optional line chart name.
- *parent*: Parent widget, or GX_NULL.
- *info*: Structure defining line chart drawing parameters. **Appendix I** contains definition to GX_LINE_CHART_INFO structure.
- *style*: Widget style flags.
- *chart_id*: Chart logical ID value.
- *size*: Chart window bounding rectangle.

### Return Values

- **GX_SUCCESS** (0x00) Successful line chart create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.
- GX_ALREADY_CREATED (0x13) Widget already created.

### Allowed From

Initialization and threads

### Example

```C
/* Create a line chart. */
GX_LINE_CHART chart;
GX_RECTANGLE chart_size;
GX_LINE_CHART_INFO gx_chart_info;
GX_WINDOW_ROOT *root_window;

chart_size = root_window->gx_widget_size;

/* Initialize params for the GUIX base chart. */
gx_chart_info.gx_line_chart_min_val = DATA_MIN_VAL;
gx_chart_info.gx_line_chart_max_val = DATA_MAX_VAL;
gx_chart_info.gx_line_chart_max_data_count = MAX_DATA_COUNT;
gx_chart_info.gx_line_chart_active_data_count = 0;
gx_chart_info.gx_line_chart_axis_line_width = AXIS_LINE_WIDTH;
gx_chart_info.gx_line_chart_data_line_width = DATA_LINE_WIDTH;
gx_chart_info.gx_line_chart_data = chart_data;
gx_chart_info.gx_line_chart_line_color = GX_COLOR_ID_DATA_LINE;
gx_chart_info.gx_line_chart_axis_color = GX_COLOR_ID_AXIS_LINE;

/* Leave room for labels on bottom and right. */
gx_chart_info.gx_line_chart_left_margin = 0;
gx_chart_info.gx_line_chart_top_margin = 0;
gx_chart_info.gx_line_chart_right_margin = 80;
gx_chart_info.gx_line_chart_bottom_margin = 32;

status = gx_line_chart_create(&chart, "Line Chart", root_window,
    &gx_chart_info, GX_STYLE_NONE, &chart_size);

/* If status is GX_SUCCESS, the "chart" has been successfully created. */
```

### See Also

- gx_line_chart_create
- gx_line_chart_data_draw
- gx_line_chart_draw
- gx_line_chart_update
- gx_line_chart_y_scale_calculate

## gx_line_chart_data_draw


Draw line chart data line

### Prototype

```C
VOID gx_line_chart_data_draw(GX_LINE_CHART *chart);
```

### Description

This service draws the line chart data line. The line colors and line width parameters are retrieved from the line chart information structure.

This service is normally called internally by the gx_line_chart_draw function, but is exposed to the application to assist in writing custom drawing functions.

### Parameters

- *chart*: Line chart control block.

### Return Values

None

### Allowed From

Threads

### Example

```C
/* Write a custom chart drawing function. */

VOID my_chart_draw(GX_LINE_CHART *chart)
{
    /* Call default window draw. */
    gx_window_draw((GX_WINDOW *)chart);

    /* Draw the line chart axis. */
    gx_line_chart_axis_draw(chart);

    /* Draw the data line. */
    gx_line_chart_data_draw(chart);

    /* Add your own drawing here. */
}
```

### See Also

- gx_line_chart_create
- gx_line_chart_draw
- gx_line_chart_update
- gx_line_chart_y_scale_calculate

## gx_line_chart_draw


Draw the line chart

### Prototype

```C
UINT gx_line_chart_draw(GX_LINE_CHART *chart);
```

### Description

This is the default line chart drawing function, which draws the chart axis and data line. Applications usually provide a custom drawing function to replace the default drawing to add things such as tickmarks, scale, or other information to the chart axis and data line drawn by the base line chart widget.

### Parameters

- *chart*: Pointer to the line chart control block.

### Return Values

None

### Allowed From

Threads

### Example

```C
/* Write a custom chart drawing function. */

VOID my_chart_draw(GX_LINE_CHART *chart)
{
    /* Call default line chart draw. */
    gx_line_chart_draw(chart);

    /* Add your own drawing here. */
}
```

### See Also

- gx_line_chart_create
- gx_line_chart_draw
- gx_line_chart_update
- gx_line_chart_y_scale_calculate

## gx_line_chart_update


Update line chart data line

### Prototype

```C
UINT gx_line_chart_update(
    GX_LINE_CHART *chart, 
    INT *data,
    INT data_count)
```

### Description

This service updates the data array plotted by the line chart window, and forces the window to redraw.

### Parameters

- *chart*: Line chart control block.
- *data*: Data array to be plotted.
- *data_count*: Size of the data array.

### Return Values

- **GX_SUCCESS** (0x00) Successful text button create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
INT chart_data[100];
GX_LINE_CHART chart;

/* Update the data array associated with "chart". */
status = gx_line_chart_update(&chart, chart_data, 100);

/* If status is GX_SUCCESS, the line chart data has been updated. */
```

### See Also

- gx_line_chart_create
- gx_line_chart_data_draw
- gx_line_chart_draw
- gx_line_chart_y_scale_calculate

## gx_line_chart_y_scale_calculate


Calculate fixed-point y axis scaling value

### Prototype

```C
UINT gx_line_chart_y_scale_calculate(
    GX_LINE_CHART *chart,
    INT *return_val);
```

### Description

This service calculates the fixed-point scaling value used to plot data values on the chart Y axis. The chart_info parameters and chart bounding rectangle are used to calculate this scaling value.

### Parameters

- *chart*: Line chart control block.
- *return_val*: Address of value to hold fixed-point return value.

### Return Values

- **GX_SUCCESS** (0x00) Successful y scale value calculate.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_LINE_CHART chart;
INT y_scale;

/* Caluclate y scale value of "chart". */
status = gx_line_chart_y_scale_calculate(&chart, &y_scale);

/* If status is GX_SUCCESS, y scale value of "chart" has been calculated. */
```

### See Also

- gx_line_chart_create
- gx_line_chart_data_draw
- gx_line_chart_draw
- gx_line_chart_update

## gx_menu_create


Create a menu

### Prototype

```C
UINT gx_menu_create(
    GX_MENU *menu,GX_CONST GX_CHAR *name,
    GX_WIDGET *parent, 
    GX_RESOURCE_ID text_id,
    GX_RESOURCE_ID fill_id, 
    ULONG style, 
    USHORT menu_id, 
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a menu as specified and associates the menu with the supplied parent widget. It accepts all types of widget as child menu item. To insert a widget as a child menu item, call ***gx_menu_insert***.

GX_MENU is derived from GX_PIXELMAP_PROMPT and supports all gx_pixelmap_prompt API services.

### Parameters

- *menu*: Pointer to menu control block.
- *name*: Name of the menu.
- *parent*: Pointer to parent widget.
- *text_id*: Resource ID of text.
- *fill_id*: Resource ID of fill.
- *style*: Style of the widget. **Appendix D** contains pre-defined general styles for all widgets as well as widget specific styles.
- *menu_id*: Application-defined ID of the menu.
- *size*: Size of the menu.

### Return Values

- **GX_SUCCESS** (0x00) Successful menu creation.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.

### Allowed From

Initialization and threads

### Example

```C
GX_MENU my_menu;

/* Create "my_menu". */
status = gx_menu_create(&my_menu, "my_menu", parent, MY_TEXT_ID,
    MY_FILL_ID, GX_STYLE_ENABLED, MY_MENU_ID,
    &size);

/* If status is GX_SUCCESS, the menu was successfully created. */
```

### See Also

- gx_accordion_meu_create
- gx_accordion_menu_draw
- gx_accordion_menu_event_process
- gx_accordion_menu_position
- gx_menu_draw
- gx_menu_event_process
- gx_menu_insert
- gx_menu_remove
- gx_menu_text_draw
- gx_menu_text_offset_set

## gx_menu_draw


Draw menu

### Prototype

```C
VOID gx_menu_draw(GX_MENU *menu);
```

### Description

This service draws the specified menu. This function is normally called internally by the GUIX canvas refresh mechanism, but is exposed to the application to assist with implementing custom drawing functions for custom menu widgets.

### Parameters

- *menu*: Pointer to menu control block.

### Return Values

None

### Allowed From

Threads

### Example

```C
/* Write a custom menu drawing function. */

VOID my_menu_draw(GX_MENU *menu)
{
    /* Call default menu draw. */
    gx_menu_draw(menu);

    /* Add your own drawing here. */
}
```

### See Also

- gx_accordion_menu_create
- gx_accordion_menu_draw
- gx_accordion_menu_event_process
- gx_accordion_menu_position
- gx_menu_create
- gx_menu_event_process
- gx_menu_insert
- gx_menu_remove
- gx_menu_text_draw
- gx_menu_text_offset_set

## gx_menu_event_process

Process menu event

### Prototype

```C
UINT gx_menu_event_process(GX_MENU *menu, GX_EVENT *event_ptr);
```

### Description

This service processes an event for the specified menu. This service should be called as the default event handler by any custom menu event processing functions.

### Parameters

- *menu*: Pointer to menu control block.
- *event_ptr*: Pointer to the event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful menu event process.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x23) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Call generic menu event processing as part of custom event processing function. */

UINT custom_menu_event_process(GX_MENU *menu, GX_EVENT *event)
{
    UINT status = GX_SUCCESS;

    switch(event->gx_event_type)
    {
    case xyz:
        /* Insert custom event handling here. */

        /* Call default event process if this is a system event such as GX_EVENT_SHOW. */
        status = gx_menu_event_process(menu, event);
        break;

    default:
        /* Pass all other events to the default menu
           event processing. */
        status = gx_menu_event_process(menu, event);
        break;
    }
    return status;
}

```

### See Also

- gx_menu_create
- gx_menu_draw
- gx_menu_insert
- gx_menu_remove
- gx_menu_text_draw
- gx_menu_text_offset_set

## gx_menu_insert


Insert a new item

### Prototype

```C
UINT gx_menu_insert(
    GX_MENU *menu, 
    GX_WIDGET *insert);
```

### Description

This service inserts a new item to the menu.

### Parameters

- *menu*: Pointer to menu control block.
- *widget*: Pointer to the widget to insert.

### Return Values

- **GX_SUCCESS** (0x00) Successfully inserted new item into menu.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Insert new item "my_widget" to the menu "my_menu". */
status = gx_menu_insert(&my_menu, &my_widget);

/* If status is GX_SUCCESS the new item "my_widget" has been inserted to the menu "my_menu". */
```

### See Also

- gx_accordion_menu_create
- gx_accordion_menu_draw
- gx_accordion_menu_event_process
- gx_accordion_menu_position
- gx_menu_create
- gx_menu_draw
- gx_menu_event_process
- gx_menu_remove
- gx_menu_text_draw
- gx_menu_text_offset_set

## gx_menu_remove


Remove an item

### Prototype

```C
UINT gx_menu_remvoe(
    GX_MENU *menu, 
    GX_WIDGET *widget);
```

### Description

This service removes an item from the menu.

### Parameters

- *menu*: Pointer to menu control block.
- *widget*: Pointer to widget to remove.

### Return Values

- **GX_SUCCESS** (0x00) Successfully removed menu item.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Remove item "my_widget" from menu "my_menu". */
status = gx_menu_remove(&my_menu, &my_widget);

/* If status is GX_SUCCESS the item "my_widget" has been removed from menu "my_menu". */
```

### See Also

- gx_accordion_menu_create
- gx_accordion_menu_draw
- gx_accordion_menu_event_process
- gx_accordion_menu_position
- gx_menu_create
- gx_menu_draw
- gx_menu_event_process
- gx_menu_insert
- gx_menu_text_draw
- gx_menu_text_offset_set

## gx_menu_text_draw


Draw menu text

### Prototype

```C
VOID gx_menu_text_draw(GX_MENU *menu);
```

### Description

This service draws the text of a menu. This function is normally called internally by the GUIX canvas refresh mechanism, but is exposed to the application to assist with implementing custom drawing functions for custom menu widgets.

### Parameters

- *menu*: Pointer to menu control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom menu drawing function. */

VOID my_menu_draw(GX_MENU *menu)
{
    /* Call default menu background draw. */
    gx_pixelmap_prompt_background_draw(
                            (GX_PIXELMAP_PROMPT *)menu);

    /* Draw menu text. */
    gx_menu_text_draw(menu);

    /* Add your own drawing here. */
}
```

### See Also

- gx_accordion_menu_create
- gx_accordion_menu_draw
- gx_accordion_menu_event_process
- gx_accordion_menu_position
- gx_menu_create
- gx_menu_draw
- gx_menu_event_process
- gx_menu_insert
- gx_menu_remove
- gx_menu_text_offset_set

## gx_menu_text_offset_set


Set menu text draw offset

### Prototype

```C
UINT gx_menu_text_offset_set(
    GX_MENU *menu, 
    GX_VALUE x_offset,
    GX_VALUE y_offset);
```

### Description

This service sets x, y display offset for menu text.

### Parameters

- *menu*: Pointer to menu control block.
- *x_offset*: X coordinate of offset.
- *y_offset*: Y coordinate of offset.

### Return Values

- **GX_SUCCESS** (0x00) Successful set menu text draw offset.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set text draw offset of menu "my_menu" to (20, 10). */
status = gx_menu_text_offset_set(&my_menu, 20, 10);

/* If status is GX_SUCCESS the text draw offset of menu "my_menu" has been set to (20, 10). */
```

### See Also

- gx_accordion_menu_create
- gx_accordion_menu_draw
- gx_accordion_menu_event_process
- gx_accordion_menu_position
- gx_menu_create
- gx_menu_draw
- gx_menu_event_process
- gx_menu_insert
- gx_menu_remove
- gx_menu_text_draw

## gx_multi_line_text_button_create


Create multi line text button

### Prototype

```C
UINT gx_multi_line_text_button_create(
    GX_MULTI_LINE_TEXT_BUTTON *text_button,
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent,
    GX_RESOURCE_ID text_id,
    ULONG style,
    USHORT text_button_id, 
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a multi-line text button widget. A multi-line text button displays the button text over 1-n lines. The maximum number of lines is defined by the constant GX_MULTI_LINE_TEXT_BUTTON_MAX_LINES, which defaults to 4. The line breaks are set by carriage return and/or carriage return + line feed pairs within the text string assigned to the multi-line text button.

GX_MULTI_LINE_TEXT_BUTTON is derived from GX_TEXT_BUTTON and supports all gx_text_button API services.

### Parameters

- *text_button*: Pointer to text button control block.
- *name*: Logical name of text button.
- *parent*: Pointer to parent widget of the button.
- *text_id*: Resource ID of text.
- *style*: Text button style. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *text_button_id*: Application-defined ID of the text button.
- *size*: Size of the button.

### Return Values

- **GX_SUCCESS** (0x00) Successful multi line text button create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.

### Allowed From

Initialization and threads

### Example

```C
/* Create multi-line text button "my_text_button". */
status = gx_multi_line_text_button_create(&my_text_button, "my text button",
    &my_parent_window, MY_TEXT_RESOURCE_ID,
    GX_STYLE_BUTTON_TOGGLE, MY_TEXT_BUTTON_ID,
    &size);

/* If status is GX_SUCCESS, the multi-line text button "my_text_button" was created. */
```

### See Also

- gx_text_button_create
- gx_button_create
- gx_multi_line_text_button_draw
- gx_multi_line_text_button_event_process
- gx_multi_line_text_button_text_set
- gx_multi_line_text_button_text_id_set

## gx_multi_line_text_button_draw


Draw multi-line text button

### Prototype

```C
VOID gx_multi_line_text_button_draw(GX_MULTI_LINE_TEXT_BUTTON *button);
```

### Description

This service draws the multi-line text button. This function is normally called internally by GUIX as part of a canvas refresh operation, but it also exposed to the application that might want to provide a custom drawing function and invoke the default multi-line text button drawing as custom drawing base.

### Parameters

- *button*: Pointer to text button control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Draw the text button "my_text_button". */
void MyButtonDraw(GX_MULTI_LINE_TEXT_BUTTON *button)
{
    /* Do the normal drawing first. */
    gx_multi_line_text_button_draw(&my_text_button);

    /* Add custom drawing here. */
}
```

### See Also

- gx_text_button_create
- gx_button_create
- gx_multi_line_text_button_draw
- gx_multi_line_text_button_event_process
- gx_multi_line_text_button_text_set
- gx_multi_line_text_button_text_id_set

## gx_multi_line_text_button_event_process


Default event handling for multi-line text button

### Prototype

```C
UINT gx_multi_line_text_button_event_process(
    GX_MULTI_LINE_TEXT_BUTTON *button,
    GX_EVENT *event_ptr);
```

### Description

This service is the default event handling function for the multi line text button widget. This function is made accessible to applications that want to provide custom event handling for a text button widget.

### Parameters

- *button*: Pointer to text button control block.
- *event_ptr*: Event to be processed.

### Return Values

- **GX_SUCCESS** (0x00) Successfully handled event.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
UINT MyEventHandler(GX_MULTI_LINE_TEXT_BUTTON *button,
    GX_EVENT *event_ptr)
{
    switch(event->gx_event_type)
    {
    case GX_EVENT_SHOW:
        gx_multi_line_text_button_event_process(button, event_ptr);
        /* Add custom actions here. */
        break;
    default:
        gx_multi_line_text_button_event_process(button, event_ptr);
        break;
    }
    return GX_SUCCESS;
}
```

### See Also

- gx_text_button_create
- gx_button_create
- gx_multi_line_text_button_draw
- gx_multi_line_text_button_event_process
- gx_multi_line_text_button_text_set
- gx_multi_line_text_button_text_id_set

## gx_multi_line_text_button_text_draw


Drawing support function

### Prototype

```C
VOID gx_multi_line_text_button_text_draw(GX_MULTI_LINE_TEXT_BUTTON *text_button);
```

### Description

This support function draws the text portion of a multi-line text button. This function is called internally by gx_multi_line_text_button_draw(), and is provided as a separate API as a convenience for applications that define a custom multi-line text button drawing function. Applications that want to customize the button background drawing can provide their custom drawing function, and invoke the multi_line_text_button_text_draw service as part of their custom drawing to draw the button text over the background.

### Parameters

- *text_button*: Pointer to text button control block.

### Return Values

- None

### Allowed From

Initialization and threads

### Example

```C
/* Define a custom drawing function. */
VOID my_button_draw(GX_MULTI_LINE_TEXT_BUTTON *button)
{
    /* Insert code here to draw button background. */

    /* Call support function to do text drawing. */
    gx_multi_line_text_button_text_draw();

    /* Draw child widgets. */
    gx_widget_children_draw((GX_WIDGET *) button);
}
```

### See Also

- gx_text_button_create
- gx_button_create
- gx_multi_line_text_button_draw
- gx_multi_line_text_button_event_process
- gx_multi_line_text_button_text_set
- gx_multi_line_text_button_text_id_set


## gx_multi_line_text_button_text_id_set


Set text resource ID to the text button

### Prototype

```C
UINT gx_multi_line_text_button_text_id_set(
    GX_MULTI_LINE_TEXT_BUTTON *text_button,
    RESOURCE_ID string_id)
```

### Description

This service sets the specified string resource ID to the text button. The string may contain newline characters which act to display the text on multiple lines within the button area.

### Parameters

- *text_button*: Pointer to text button control block.
- *string_id*: Resource ID of the string.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set the string resource ID to the text button.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set the string ID "MY_STRING_ID" to the text button "my_text_button". */
status = gx_multi_line_text_button_text_id_set(
    &my_text_button, MY_STRING_ID);

/* If status is GX_SUCCESS, the string ID MY_STRING_ID was set to "my_text_button". */
```

### See Also

- gx_text_button_create
- gx_button_create
- gx_multi_line_text_button_draw
- gx_multi_line_text_button_event_process
- gx_multi_line_text_button_text_set
- gx_multi_line_text_button_text_id_set

## gx_multi_line_text_button_text_set


Assign text to the text button (deprecated)

### Prototype

```C
UINT gx_mult_line_text_button_text_set(
    GX_MULTI_LINE_TEXT_BUTTON *text_button,
    GX_CHAR *text);
```

### Description

This service is deprecated in favor of gx_multi_line_text_button_text_set_ext().

This service assigns the specified string to the text button. If the text_button widget was created with style GX_STYLE_TEXT_COPY, the widget creates a private copy of the text string assigned, and therefore the gx_system_memmory_allocate_set API must be invoked once before this service is requested. If GX_STYLE_TEXT_COPY is not active, the widget does not make a private copy of the incoming string, and therefore the string must be statically or globally allocated, i.e. it may not be an automatic or temporary variable.

### Parameters

- *text_button*: Pointer to text button control block.
- *text*: pointer to the NULL-terminated string.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set the text to the button.
- **GX_SYSTEM_MEMORY_ERROR** (0x30) Memory allocator is not defined.
- **GX_INVALID_STRING_LENGTH** (0x34) Invalid string length.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
static GX_CHAR text[] = "myrstring";

/* Set text to the text button "my_text_button". */
status = gx_multi_line_text_button_text_set(&my_text_button, text);

/* If status is GX_SUCCESS, the text of "my_text_button" was set. */
```

### See Also

- gx_text_button_create
- gx_button_create
- gx_multi_line_text_button_draw
- gx_multi_line_text_button_event_process
- gx_multi_line_text_button_text_set
- gx_multi_line_text_button_text_id_set

## gx_multi_line_text_button_text_set_ext


Assign text to the text button

### Prototype

```C
UINT gx_mult_line_text_button_text_set_ext(
    GX_MULTI_LINE_TEXT_BUTTON *text_button,
    GX_STRING *string)
```

### Description

This service assigns the specified string to the text button. If the text_button widget was created with style GX_STYLE_TEXT_COPY, the widget creates a private copy of the text string assigned, and therefore the gx_system_memmory_allocate_set API must be invoked once before this service is requested. If GX_STYLE_TEXT_COPY is not active, the widget does not make a private copy of the incoming string, and therefore the string must be statically or globally allocated, i.e. it may not be an automatic or temporary variable.

### Parameters

- *text_button*: Pointer to text button control block.
- *string*: pointer to GX_STRING variable.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set the text to the button.
- **GX_SYSTEM_MEMORY_ERROR** (0x30) Memory allocator is not defined.
- **GX_INVALID_STRING_LENGTH** (0x34) Invalid string length.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
static GX_CHAR text[] = "myrstring";
GX_STRING string;

string.gx_string_ptr = text;
string.gx_string_length = strlen(text);

/* Set text to the text button "my_text_button". */
status = gx_multi_line_text_button_text_set_ext(&my_text_button, string);

/* If status is GX_SUCCESS, the text of "my_text_button" was set. */
```

### See Also

- gx_multi_line_text_button_draw
- gx_multi_line_text_button_event_process
- gx_multi_line_text_button_text_set
- gx_multi_line_text_button_text_id_set

## gx_multi_line_text_input_backspace


Delete a character before multi line text input cursor position

### Prototype

```C
UINT gx_multi_line_text_input_backspace(
    GX_MULTI_LINE_TEXT_INPUT *text_input);
```

### Description

This service deletes the character before multi line text input cursor position. This service is called internally when a backspace key down event is received, but can also be invoked by the application.

### Parameters

- *text_input*: Multi-line text input widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful multi-line text input backspace.
- **GX_FAILURE** (0x10) Invalid font.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x23) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Delete a character before the cursor of "my_text_input". */
status = gx_multi_line_text_input_backspace(&my_text_input);

/* If status is GX_SUCCESS the character before the cursor has been deleted. */
```

### See Also

- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_buffer_clear


Deletes all characters from the text input buffer

### Prototype

```C
UINT gx_multi_line_text_input_buffer_clear(GX_MULTI_LINE_TEXT_INPUT *text_input);
```

### Description

This service deletes all characters from the text input buffer.

### Parameters

- *text_input*: Multi-line text input widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful multi-line text input buffer clear.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Clear input buffer of "my_text_input". */
status = gx_multi_line_text_input_clear(&my_text_input);

/* If status is GX_SUCCESS the text input widget has emptied its input buffer. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_buffer_get


Retrieves buffer information of text input widget

### Prototype

```C
UINT gx_multi_line_text_input_buffer_get(
    GX_MULTI_LINE_TEXT_INPUT *text_input, 
    GX_CHAR **buffer_address,
    UINT *content_size, 
    UINT *buffer_size);
```

### Description

This service retrieves buffer information of a multi-line text input widget.

### Parameters

- *text_input*: Multi-line text input widget control block.
- *buffer_address*: The address of the input buffer.
- *content_size*: The byte count of the input data.
- *buffer_size*: The size of the input buffer.

### Return Values

- **GX_SUCCESS** (0x00) Successful multi-line text get.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_CHAR *buffer_address;
UINT context_size;
UINT buffer_size;

/* Retrieves buffer information of "my_text_input" widget. */
status = gx_multi_line_text_input_buffer_get(&my_text_input,
    &buffer_address, &string_size,
    &buffer_size);

/* If status is GX_SUCCESS the value of buffer_address, string_size and buffer_size has been retrieved. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_char_insert


Insert a character string at current multi line text input cursor position (deprecated)

### Prototype

```C
UINT gx_multi_line_text_input_char_insert(
    GX_MULTI_LINE_TEXT_INPUT *text_input,
    GX_UBYTE *insert_str,
    UINT insert_size);
```

### Description

This API is deprecated and replaced by gx_multi_line_text_input_char_insert_ext().

This service inserts a character string into the multi line text input string buffer at the current cursor position. This service is called internally when specific key down event is received, but can also be invoked by the application.

### Parameters

- *text_input*: Multi-line text input widget control block.
- *insert_str*: UTF-8 format character string to be inserted.
- *insert_size*: Byte count to be inserted.

### Return Values

- **GX_SUCCESS** (0x00) Successfully inserted the character string.
- **GX_FAILURE** (0x10) Invalid font or out of buffer size.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_VALUE (0x22) Invalid string size.

### Allowed From

Initialization and threads

### Example

```C
/* Insert characters at current cursor position. */
GX_CHAR insert_text[10] = "insert";
status = gx_multi_line_text_input_char_insert(&my_text_input,
    insert_text,
    GX_STRLEN(insert_text));

/* If status is GX_SUCCESS the multi line text input widget has successfully insert the string. */
```

### See Also

- gx_multi_line_text_input_char_insert_ext

## gx_multi_line_text_input_char_insert_ext


Insert a character string at current multi line text input cursor position (deprecated)

### Prototype

```C
UINT gx_multi_line_text_input_char_insert_ext(
    GX_MULTI_LINE_TEXT_INPUT *text_input,
    GX_CONST GX_STRING *string);
```

### Description

This service inserts a character string into the multi line text input string buffer at the current cursor position. This service is called internally when specific key down events are received, but can also be invoked by the application.

### Parameters

- *text_input*: Multi-line text input widget control block.
- *string*: UTF-8 encoded character string to be inserted.

### Return Values

- **GX_SUCCESS** (0x00) Successfully inserted the character string.
- **GX_FAILURE** (0x10) Invalid font or out of buffer size.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_STRING_LENGTH (0x34) Invalid string length.

### Allowed From

Initialization and threads

### Example

```C
/* Insert characters at current cursor position. */
GX_CHAR insert_text[10] = "insert";
GX_STRING string;

string.gx_string_ptr = insert_text;
string.gx_string_length = strlen(insert_text);

status = gx_multi_line_text_input_char_insert_ext(&my_text_input, &string);

/* If status is GX_SUCCESS the multi line text input widget has successfully inserted the string. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_create


Create multi-line text input

### Prototype

```C
UINT gx_multi_line_text_input_create(
    GX_MULTI_LINE_TEXT_INPUT *text_input,
    GX_CONST GX_CHAR *name, GX_WINDOW *parent,
    GX_CHAR *input_buffer, UINT buffer_size,
    ULONG style, USHORT text_input_id, 
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a multi-line text input widget.

GX_MULTI_LINE_TEXT_INPUT is derived from GX_MULTI_LINE_TEXT_VIEW and supports all gx_multi_line_text_view services.

### Parameters

- *text_input*: Multi-line text input widget control block.
- *name*: Name of text input widget.
- *parent*: Pointer to parent widget.
- *input_buffer*: Pointer to text input buffer.
- *buffer_size*: Size of text input buffer in bytes.
- *style*: Style of text input widget. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *text_input_id*: Application-defined ID of text input.
- *size*: Dimensions of text input widget.

### Return Values

- **GX_SUCCESS** (0x00) Successful multi-line text input create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_WIDGET (0x12) Parent widget not valid.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.

### Allowed From

Initialization and threads

### Example

```C
GX_MULTI_LINE_TEXT_INPUT my_text_input;
GX_CHAR my_buffer[100];
GX_RECTANGLE size;

/* Define widget size. */
gx_utility_rectangle_define(&size, 10, 10, 100, 200);

/* Create multi-line text input widget "my_text_input". */
status = gx_multi_line_text_input_create(&my_text_input,
    "my_text_input", &my_parent,
    my_buffer, 100, GX_STYLE_BORDER_RAISED,
    MY_TEXT_INPUT_ID, &size);

/* If status is GX_SUCCESS, the text input "my_text_input" has been created. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_cursor_pos_get


Retrieve multi line text input cursor position

### Prototype

```C
UINT gx_multi_line_text_input_cursor_pos_get(
    GX_MULTI_LINE_TEXT_INPUT *text_input,
    GX_POINT cursor_pos);
```

### Description

This service retrieves the mult-line text input cursor position.

### Parameters

- *text_input*: Multi-line text input widget control block.
- *cursor_pos*: Retrieved cursor position.

### Return Values

- **GX_SUCCESS** (0x00) Successfully retrieved cursor position.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Retrieve the cursor position of "my_text_input". */
GX_POINT cursor_pos;
status = gx_multi_line_text_input_cursor_pos_get(&my_text_input,
    &cursor_pos);

/* If status is GX_SUCCESS the cursor position has been retrieved. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_delete


Delete the character at the multi line text input cursor position

### Prototype

```C
UINT gx_multi_line_text_input_delete(GX_MULTI_LINE_TEXT_INPUT *text_input);
```

### Description

This service deletes the character after the multi line text input cursor position. This service is called internally when a delete key down event is received, but can also be invoked by the application.

### Parameters

- *text_input*: Multi-line text input widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully delete a character after the cursor.
- **GX_FAILURE** (0x10) Invalid font.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Delete the character after the cursor of "my_text_input". */
status = gx_multi_line_text_input_delete(&my_text_input);

/* If status is GX_SUCCESS the character after the cursor has been deleted. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_down_arrow


Move the multi line text input cursor to the next line

### Prototype

```C
UINT gx_multi_line_text_input_down_arrow(GX_MULTI_LINE_TEXT_INPUT *text_input);
```

### Description

This service positions the multi line text input widget cursor to the next line. This service is called internally when a down arrow key down event is received, but can also be invoked by the application.

### Parameters

- *text_input*: Multi-line text input widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully moved text input cursor to the next line.
- **GX_FAILURE** (0x10) Invalid font or line height.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Move input cursor to the next line. */
status = gx_multi_line_text_input_down_arrow(&my_text_input);

/* If status is GX_SUCCESS, text text input cursor has been moved to the next line. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_end


Move the multi line text input cursor to the end of the current line

### Prototype

```C
UINT gx_multi_line_text_input_end(GX_MULTI_LINE_TEXT_INPUT *text_input);
```

### Description

This service positions the multi line text input widget cursor to the end of the current string line. This service is called internally when an end key down event is received, but can also be invoked by the application.

### Parameters

- *text_input*: Multi-line text input widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully moved text input cursor to end of the current line.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Move input cursor to the end of current line. */
status = gx_multi_line_text_input_end(&my_text_input);

/* If status is GX_SUCCESS, the multi line text input cursor has been moved to the end of the current line. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_event_process


Default event handling for multi-line text input

### Prototype

```C
UINT gx_multi_line_text_input_event_process(
    GX_MULTI_LINE_TEXT_INPUT *input,
    GX_EVENT *event_ptr);
```

### Description

This service is the default event handling function for the multi line text input widget. This function is made accessible to applications that want to provide custom event handling for a multi line text input widget.

### Parameters

- *button*: Pointer to multi line text input control block.
- *event_ptr*: Event to be processed.

### Return Values

- **GX_SUCCESS** (0x00) Successfully handled event.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Parent widget not valid.

### Allowed From

Initialization and threads

### Example

```C
UINT MyEventHandler(GX_MULTI_LINE_TEXT_INPUT *input,
    GX_EVENT *event_ptr)
{
    switch(event->gx_event_type)
    {
    case xyz:
        /* Insert custom event handling here. */
        break;
    default:
        /* Pass all other events to the generic multi line text input event processing. */
        gx_multi_line_text_input_event_process(input, event_ptr);
        break;
    }
    return GX_SUCCESS;
}
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_fill_color_set


Set multi line text input background color

### Prototype

```C
UINT gx_multi_line_text_input_fill_color_set(
    GX_MULTI_LINE_TEXT_INPUT *text_input,
    GX_RESOURCE_ID normal_fill_color_id,
    GX_RESOURCE_ID selected_fill_color_id,
    GX_RESOURCE_ID disabled_fill_color_id,
    GX_RESOURCE_ID readonly_fill_color_id);
```

### Description

This service assigns fill colors for the multi-line text input widget.

### Parameters

- *text_input*: Multi-line text input widget control block.
- *normal_fill_color_id*: Resource ID of the normal fill color that used in normal state.
- *selected_fill_color_id*: Resource ID of the selected fill color that used when the widget gain focus.
- *disabled_fill_color_id*: Resource ID of the disabled fill color that used when GX_STYLE_ENABLED is not active.
- *readonly_fill_color_id*: Resource ID of the read only fill color that used when both GX_STYLE_ENABLED and GX_STYLE_INPUT_READONLY are active.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set colors for the multi-line text input.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set fill colors for the multi-line text input widget "my_text_input". */

status = gx_multi_line_text_input_fill_color_set(&my_text_input,
    GX_COLOR_ID_NORMAL_FILL,
    GX_COLOR_ID_SELECTED_FILL,
    GX_COLOR_ID_DISABLED_FILL,
    GX_COLOR_ID_READONLY_FILL);

/* If status is GX_SUCCESS, the fill color of "my_text_input" has been successfully set. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_home


Move the text input cursor to the start of the current line

### Prototype

```C
UINT gx_multi_line_text_input_home(GX_MULTI_LINE_TEXT_INPUT *text_input);
```

### Description

This service moves the text input cursor position to the start of the current line. This service is called internally when a home key down event is received, but can also be invoked by the application.

### Parameters

- *text_input*: Multi-line text input widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully moved cursor to start of the current line.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Move cursor to the start of the current line. */
status = gx_multi_line_text_input_home(&my_text_input);

/* If status is GX_SUCCESS the cursor has been moved to the start of the current line. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_left_arrow


Move multi line text input cursor one character to the left

### Prototype

```C
UINT gx_multi_line_text_input_left_arrow(GX_MULTI_LINE_TEXT_INPUT *text_input);
```

### Description

This service moves the multi line text input cursor one character to the left. This service is called internally when a left key down event is received, but can also be invoked by the application.

### Parameters

- *text_input*: Single-line text input widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully moved cursor to the left.
- **GX_FAILURE** (0x10) Invalid font.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Move the cursor one character to the left. */
status = gx_multi_line_text_input_left_arrow(&my_text_input);

/* If status is GX_SUCCESS the multi line text input cursor has been moved one character to the left. */

```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_right_arrow


Move multi line text input cursor one character to the right

### Prototype

```C
UINT gx_multi_line_text_input_right_arrow(GX_MULTI_LINE_TEXT_INPUT *text_input);
```

### Description

This service moves the multi line text input cursor one character to the right. This service is called internally when a right key down event is received, but can also be invoked by the application.

### Parameters

- *text_input*: Multi-line text input widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully moved cursor to the right.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Move cursor one character to the right. */
status = gx_multi_line_text_input_right_arrow(&my_text_input);

/* If status is GX_SUCCESS the text input cursor has been moved one character to the right. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_style_add


Add multi line text input styles

### Prototype

```C
UINT gx_multi_line_text_input_style_add(
    GX_MULTI_LINE_TEXT_INPUT *text_input,
    ULONG style);
```

### Description

This service adds styles to a multi-line text input widget.

### Parameters

- *text_input*: Multi-line text input widget control block.
- *style*: Styles to add. **Appendix D** contains pre-defined general styles for all widgets.

### Return Values

- **GX_SUCCESS** (0x00) Successful multi-line text input style add.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Add style GX_STYTLE_CURSOR_ALWAYS to multi-line text input widget "my_text_input". */
status = gx_multi_line_text_input_style_add(&my_text_input,
    GX_STYLE_CURSOR_ALWAYS);

/* If status is GX_SUCCESS the style GX_STYLE_CURSOR_ALWAYS has been successfully added. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_style_remove


Remove styles

### Prototype

```C
UINT gx_multi_line_text_input_remove(
    GX_MULTI_LINE_TEXT_INPUT *text_input,
    ULONG style);
```

### Description

This service removes the specified styles from the multi-line text input widget.

### Parameters

- *text_input*: Multi-line text input widget control block.
- *style*: Styles to remove. **Appendix D** contains pre-defined general styles for all widgets.

### Return Values

- **GX_SUCCESS** (0x00) Successful multi-line text input create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Remove style GX_STYLE_CURSOR_ALWAYS from text input widget "my_text_input". */
status = gx_multi_line_text_input_style_remove(&my_text_input,
    GX_STYLE_CURSOR_ALWAYS);

/* If status is GX_SUCCESS, the style GX_STYLE_CURSOR_ALWAYS has been successfully removed. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_style_set

Set multi line text input styles

### Prototype

```C
UINT gx_multi_line_text_input_style_set(
    GX_MULTI_LINE_TEXT_INPUT *text_input, 
    ULONG style);
```

### Description

This service sets styles for a multi-line text input widget.

### Parameters

- *text_input*: Multi-line text input widget control block.
- *style*: Styles to set. **Appendix D** contains pre-defined general styles for all widgets.

### Return Values

- **GX_SUCCESS** (0x00) Successful multi-line text input style set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set style GX_STYLE_CURSOR_ALWAYS for text input widget "my_text_input". */
status = gx_multi_line_text_input_style_set(&my_text_input,
    GX_STYLE_CURSOR_ALWAYS);

/* If status is GX_SUCCESS the text input style has been set. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_text_color_set


Set multi line text input text color

### Prototype

```C
UINT gx_multi_line_text_input_text_color_set(
    GX_MULTI_LINE_TEXT_INPUT *text_input,
    GX_RESOURCE_ID normal_text_color_id,
    GX_RESOURCE_ID selected_text_color_id,
    GX_RESOURCE_ID disabled_text_color_id,
    GX_RESOURCE_ID readonly_text_color_id);
```

### Description

This service assigns text colors for the multi-line text input widget.

### Parameters

- *text_input*: Multi-line text input widget control block.
- *normal_fill_color_id*: Resource ID of the normal text color that used in normal state.
- *selected_text_color_id*: Resource ID of the selected text color that used when the widget gain focus.
- *disabled_text_color_id*: Resource ID of the disabled text color that used when GX_STYLE_ENABLED is not active.
- *readonly_text_color_id*: Resource ID of the read only text color that used when both GX_STYLE_ENABLED and GX_STYLE_TEXT_INPUT_READONLY are active.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set colors for the multi-line text input.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set text colors for the multi-line text input widget "my_text_input". */
status = gx_multi_line_text_input_text_color_set(&my_text_input,
    GX_COLOR_ID_NORMAL_TEXT,
    GX_COLOR_ID_SELECTED_TEXT,
    GX_COLOR_ID_DISABLED_TEXT,
    GX_COLOR_ID_READONLY_TEXT);

/* If status is GX_SUCCESS, the fill colors of "my_text_view" has been successfully set. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_text_select


Select text

### Prototype

```C
UINT gx_multi_line_text_input_text_select(
    GX_MULTI_LINE_TEXT_INPUT *text_input,
    UINT start_index, UINT end_index);
```

### Description

This service selects multi line text input text with specified start mark and end mark index and highlights the selected text with the selected fill and text colors.

### Parameters

- *text_input*: Pointer to multi line text input control block.
- *start_index*: Index of the first selected character.
- *end_index*: Index of the last selected character.

### Return Values

- **GX_SUCCESS** (0x00) Successful multi line text input text selection.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_VALUE (0x22) Index value not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Select text between index [0, 9]. */
status = gx_multi_line_text_input_text_select(&my_text_input,
    0,9);

/* If status is GX_SUCCESS, the text between 
    index [0, 9] "my_text_input" was selected. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_text_set


Assign text to the text input (deprecated)

### Prototype

```C
UINT gx_mult_line_text_input_text_set(
    GX_MULTI_LINE_TEXT_INPUT *text_input,
    GX_CHAR *text);
```

### Description

This API is deprecated and replace by gx_multi_line_text_input_text_set_ext().

This service assigns the specified string to the multi line text input. If the multi_line_text_input widget's input buffer size is smaller than string length, the string will be truncated.

### Parameters

- *text_input*: Pointer to multi line text input control block.
- *text*: pointer to the NULL-terminated string.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set the text to the multi line text input.
- **GX_INVALID_STRING_LENGTH** (0x34) Invalid string length.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set the string "my string" to the text button "my_text_input". */
status = gx_multi_line_text_input_text_set(&my_text_input,
    "myrstring");

/* If status is GX_SUCCESS, the content of "my_text_input" bas been reset. */
```

### See Also

- gx_multi_line_text_input_text_set_ext

## gx_multi_line_text_input_text_set_ext


Assign text to the text input

### Prototype

```C
UINT gx_mult_line_text_input_text_set(
    GX_MULTI_LINE_TEXT_INPUT *text_input,
    GX_CONST GX_STRING *string);
```

### Description

This service assigns the specified string to the multi line text input. If the multi_line_text_input widget's input buffer size is smaller than string length, the string will be truncated.

### Parameters

- *text_input*: Pointer to multi line text input control block.
- *string*: pointer to GX_STRING to assign.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set the text to the multi line text input.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_STRING_LENGTH (0x34) Invalid string length.

### Allowed From

Initialization and threads

### Example

```C
/* Set the string "my string" to the text button "my_text_input". */
GX_STRING string;
string.gx_string_ptr = "myrstring";
string.gx_string_length = strlen(string.gx_string_ptr);

status = gx_multi_line_text_input_text_set_ext(&my_text_input, &string);

/* If status is GX_SUCCESS, the content of "my_text_input" bas been reset. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_input_up_arrow


Move multi line text input cursor to the previous line

### Prototype

```C
UINT gx_multi_line_text_input_up_arrow(GX_MULTI_LINE_TEXT_INPUT *text_input);
```

### Description

This service moves the multi line text input cursor to the previous line of text. This service is called internally when an up arrow key down event is received, but can also be invoked by the application.

### Parameters

- *text_input*: Single-line text input widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully moved cursor to the previous line.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Move the cursor to the previous line. */
status = gx_multi_line_text_input_up_arrow(&my_text_input);

/* If status is GX_SUCCESS the multi line text input cursor has been moved to the previous line. */

```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_view_create


Create multi-line text view

### Prototype

```C
UINT gx_multi_line_text_view_create(
    GX_MULTI_LINE_TEXT_VIEW *text_view,
    GX_CONST GX_CHAR *name, 
    GX_WINDOW *parent, 
    GX_RESOURCE_ID text_id,
    ULONG style, 
    USHORT text_view_id, 
    GX_CONST GX_RECTANGLE *size);

```

### Description

This service creates a GX_MULTI_LINE_TEXT_VIEW widget. This widget type is derived from GX_WINDOW, and therefore all gx_window API services may also be utilized with this widget type.

### Parameters

- *text_view*: Multi-line text view widget control block.
- *name*: Name of the text view widget.
- *parent*: Pointer to parent widget.
- *text_id*: Resource ID of the text string.
- *style*: Style of text view widget. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *text_view_id*: Application-defined ID of text view.
- *size*: Dimensions of text view widget.

### Return Values

- **GX_SUCCESS** (0x00) Successfully created multi-line text view widget.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.

### Allowed From

Initialization and threads

### Example

```C
/* Create multi-line text view widget "my_text_view". */
status = gx_multi_line_text_view_create(&my_text_view,
    "my_text_view", &my_parent,
    TEXT_STRING_ID, GX_STYLE_BORDER_RAISED,
    MY_TEXT_VIEW_ID, &size);

/* If status is GX_SUCCESS the text view "my_text_view" has been successfully created. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_view_draw


Draw a multi line text view widget

### Prototype

```C
VOID gx_multi_line_text_view_draw(GX_MULTI_LINE_TEXT_VIEW * text_view);
```

### Description

This service draws a multi line text view widget. This service is normally called internally during canvas refresh, but can also be called from custom multi line text view drawing functions.

### Parameters

- *text_view*: Multi-line text view widget control block.

### Return Values

- None

### Allowed From

Initialization and threads

### Example

```C
/* Write a custom multi line text view drawing function. */

VOID my_multi_line_text_view_draw(GX_MULTI_LINE_TEXT_VIEW *view)
{
    /* Call default multi line text view draw. */
    gx_multi_line_text_view_draw(view);

    /* Add your own drawing here. */
}
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_view_event_process


Process multi-line text view event

### Prototype

```C
UINT gx_multi_line_text_view_event_process(
    GX_MULTI_LINE_TEXT_VIEW *text_view, 
    GX_EVENT *event);
```

### Description

This service processes an event for a multi-line text view widget.

### Parameters

- *text_view*: Multi-line text view widget control block.
- *event*: Pointer to event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful multi-line text view event process.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Write a custom event handler. */
UINT my_event_handler(GX_MULTI_LINE_TEXT_VIEW *view, GX_EVENT *event_ptr)
{
    switch(event->gx_event_type)
    {
    case GX_EVENT_SHOW:
        gx_multi_line_text_view_event_process(view, event_ptr);
        /* Add custom actions here. */
        break;
    default:
        gx_multi_line_text_view_event_process (view, event_ptr);
        break;
    }
    return GX_SUCCESS;
}
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_view_font_set


Set font used in multi-line text view

### Prototype

```C
UINT gx_multi_line_text_view_text_id_set(
    GX_MULTI_LINE_TEXT_VIEW *text_view, 
    GX_RESOURCE_ID font_id);
```

### Description

This service sets the font of a multi-line text view widget.

### Parameters

- *text_view*: Multi-line text view widget control block.
- *font_id*: Resource ID for the font.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set font for the multi-line text view.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set font ID FONT_ID to the multi-line text view widget "my_text_view". */
status = gx_multi_line_text_view_font_set(&my_text_view, FONT_ID);

/* If status is GX_SUCCESS, the text view "my_text_view" will use the specified font to display its text. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_view_line_space_set


Set multi-line text view line space

### Prototype

```C
UINT gx_multi_line_text_view_line_space_set(
    GX_MULTI_LINE_TEXT_VIEW *text_view, 
    GX_BYTE line_space);
```

### Description

This service sets the spacing between lines of text for the multi-line text view widget.

### Parameters

- *view*: Multi-line text view widget control block.
- *line_space*: Value to set.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set line space value for the multi-line text view.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

```C
/* Set line space of "my_text_view" to 2. */
status = gx_multi_line_text_view_line_space_set(&my_text_view, 2);

/* If status is GX_SUCCESS, the line space of "my_text_view" has been successfully set to 2. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_view_scroll_info_get

Get multi-line text view scroll info


### Prototype

```C
UINT gx_multi_line_text_view_scroll_info_get(
    GX_MULTI_LINE_TEXT_VIEW *text_view, ULONG style,
    GX_SCROLL_INFO *info);
```

### Description

This service gets the multi-line text view scroll information.

### Parameters

- *text_view*: Multi-line text view widget control block.
- *style*: GX_SCROLLBAR_HORIZONTAL or GX_SCROLLBAR_VERTICAL.
- *info*: Pointer to destination for scroll info. **Appendix I** contains definition to GX_SCROLL_INFO structure.

### Return Values

- **GX_SUCCESS** (0x00) Successfully retrieved text view scroll info.
- **GX_FAILURE** (0x10) Widget is not visible or text view font ID is not valid.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_SCROLL_INFO scroll_info;

/* Get scroll information for multi-line text view "my_text_view". */
status = gx_multi_line_text_view_scroll_info_get(&my_text_view,
    &scroll_info);

/* If status is GX_SUCCESS the "scroll_info" contains the scroll information for multi-line text view "my_text_view". */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_view_text_color_set


Set the text color for the multi line text view

### Prototype

```C
UINT gx_multi_line_text_view_text_color_set(
    GX_MULTI_LINE_TEXT_VIEW *text_view,
    GX_RESOURCE_ID normal_text_color_id,
    GX_RESOURCE_ID selected_text_color_id,
    GX_RESOURCe_ID disabled_text_color_id);
```

### Description

This service assigns text color to the multi-line text view widget.

### Parameters

- *text_view*: Multi-line text view widget control block.
- *normal_text_color_id*: Resource ID of the normal text color that used in normal state.
- *selected_text_color_id*: Resource ID of the selected text color that used when the widget gain focus.
- *disabled_text_color_id*: Resource ID of the disabled text color that used GX_STYLE_ENABLED is not active.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set colors for the multi-line text view.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set text colors for the multi-line text view widget "my_text_view". */
status = gx_multi_line_text_view_text_color_set(&my_text_view,
    GX_COLOR_ID_NORMAL_TEXT,
    GX_COLOR_ID_SELECTED_TEXT,
    GX_COLOR_ID_DISABLED_TEXT);

/* If status is GX_SUCCESS the text color of "my_text_view" has been successfully set. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_view_text_id_set


Set system text string in multi line text view

### Prototype

```C
UINT gx_multi_line_text_view_text_id_set(
    GX_MULTI_LINE_TEXT_VIEW *text_view, 
    GX_RESOURCE_ID text_id);
```

### Description

This service sets the resource ID of a string to the multi-line text view widget.

### Parameters

- *text_view*: Multi-line text view widget control block.
- *text_id*: Resource ID for the text string.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set string ID for the multi-line text view.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set string ID STRING_ID to the multi-line text view widget "my_text_view". */
status = gx_multi_line_text_view_text_id_set(&my_text_view, STRING_ID);

/* If status is GX_SUCCESS the text id of "my_text_view" has been successfully set. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_view_text_set

Set user-defined string in multi line text view (Deprecated)

### Prototype

```C
UINT gx_multi_line_text_view_text_set(
    GX_MULTI_LINE_TEXT_VIEW *text_view, 
    GX_CONST GX_CHAR *text);
```

### Description

This service has been deprecated in favor of gx_multi_line_text_view_text_set_ext().

This service assigns a text string to the multi-line text view widget. If the text_view widget was created with style GX_STYLE_TEXT_COPY, the widget creates a private copy of the text string assigned, and therefore the gx_system_memory_allocate_set API must be invoked once before this service is requested. If GX_STYLE_TEXT_COPY is not active, the widget does not make a private copy of the incoming string, and therefore the assigned string must be statically or globally allocated, i.e. it may not be an automatic or temporary variable.

### Parameters

- *text_view*: Multi-line text view widget control block.
- *text*: NULL-terminated text string.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set string for the multi-line text view.
- **GX_SYSTEM_MEMORY_ERROR** (0x30) Memory allocator is not defined or memory allocation failed.
- **GX_INVALID_STRING_LENGTH** (0x34) Invalid string length.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set string "my string" to the multi-line text view widget "my_text_view". */
status = gx_multi_line_text_view_text_set(&my_text_view, "my string");

/* If status is GX_SUCCESS the text of "my_text_view" has been successfully set. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set_ext
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_view_text_set_ext

Set user-defined string in multi line text view

### Prototype

```C
UINT gx_multi_line_text_view_text_set_ext(
    GX_MULTI_LINE_TEXT_VIEW *text_view,
    GX_CONST GX_STRING *text);
```

### Description

This service assigns a text string to the multi-line text view widget. If the text_view widget was created with style GX_STYLE_TEXT_COPY, the widget creates a private copy of the text string assigned, and therefore the gx_system_memory_allocate_set API must be invoked once before this service is requested. If GX_STYLE_TEXT_COPY is not active, the widget does not make a private copy of the incoming string, and therefore the assigned string must be statically or globally allocated, i.e. it may not be an automatic or temporary variable.

### Parameters

- *text_view*: Multi-line text view widget control block.
- *text*: Pointer to a string structure.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set string for the multi-line text view.
- **GX_SYSTEM_MEMORY_ERROR** (0x30) Memory allocator is not defined or memory allocation failed.
- GX_INVALID_STRING_LENGTH (0x34) Invalid string length.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_STRING string;

string.gx_string_ptr = "my string";
string.gx_string_length = sizeof("my string") - 1;

/* Set string "my string" to the multi-line text view widget "my_text_view". */
status = gx_multi_line_text_view_text_set(&my_text_view, &string);

/* If status is GX_SUCCESS the text of "my_text_view" has been successfully set. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_whitespace_set

## gx_multi_line_text_view_whitespace_set


Set multi-line text view whitespace

### Prototype

```C
UINT gx_multi_line_text_view_whitespace_set(
    GX_MULTI_LINE_TEXT_VIEW *text_view, 
    GX_UBYTE whitespace);
```

### Description

This service sets spacing between widget outlines and client area for a multi-line text view widget.

### Parameters

- *text_view*: Multi-line text view widget control block.
- *whitespace*: Width of margin between text_view widget and the displayed text, in pixels.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set whitespace for the multi-line text view.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set whitespace of "my_text_view" to 2. */
status = gx_multi_line_text_view_whitespace_set(&my_text_view, 2);

/* If status is GX_SUCCESS the whitespace of "my_text_view" has been successfully set to 2. */
```

### See Also

- gx_multi_line_text_input_backspace
- gx_multi_line_text_input_buffer_clear
- gx_multi_line_text_input_buffer_get
- gx_multi_line_text_input_char_insert
- gx_multi_line_text_input_create
- gx_multi_line_text_input_cursor_pos_get
- gx_multi_line_text_input_delete
- gx_multi_line_text_input_down_arrow
- gx_multi_line_text_input_end
- gx_multi_line_text_input_event_process
- gx_multi_line_text_input_fill_color_set
- gx_multi_line_text_input_home
- gx_multi_line_text_input_left_arrow
- gx_multi_line_text_input_right_arrow
- gx_mutli_line_text_input_style_add
- gx_multi_line_text_input_style_remove
- gx_multi_line_text_input_style_set
- gx_multi_line_text_input_text_color_set
- gx_multi_line_text_input_text_select
- gx_multi_line_text_input_text_set
- gx_multi_line_text_input_up_arrow
- gx_multi_line_text_view_create
- gx_multi_line_text_view_draw
- gx_multi_line_text_view_event_process
- gx_multi_line_text_view_font_set
- gx_multi_line_text_view_line_space_set
- gx_multi_line_text_view_scroll_info_get
- gx_multi_line_text_view_text_color_set
- gx_multi_line_text_view_text_id_set
- gx_multi_line_text_view_text_set

## gx_numeric_pixelmap_prompt_create


Create numeric pixelmap prompt

### Prototype

```C
UINT gx_numeric_pixelmap_prompt_create(
    GX_NUMERIC_PIXELMAP_PROMPT *prompt,
    GX_CONST GX_CHAR name, GX_WIDGET *parent,
    GX_RESOURCE_ID text_id, GX_RESOURCE_ID fill_id,
    ULONG style, USHORT pixelmap_prompt_id,
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a numeric pixelmap prompt widget. A numeric_pixelmap_prompt is just a pixelmap_prompt that keeps its own buffer and provides a gx_numeric_pixelmap_prompt_value_set(INT) API, the buffer size is defined by the constant GX_NUMERIC_PROMPT_BUFFER_SIZE, which defaults to 16.

GX_NUMERIC_PIXELMAP_PROMPT is derived from GX_PIXELMAP_PROMPT and supports all gx_pixelmap_prompt API services.

### Parameters

- *prompt*: Numeric pixelmap prompt control block.
- *name*: Name of prompt.
- *parent*: Parent widget control block.
- *text_id*: Resource string ID.
- *fill_id*: Pixelmap ID for fill area.
- *style*: Style of numeric pixelmap prompt, **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *pixelmap_prompt_id*: Application-defined ID of prompt.
- *size*: Dimensions of numeric pixelmap prompt.

### Return Values

- **GX_SUCCESS** (0x00) Successfully create numeric pixelmap prompt.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.

### Allowed From

Initialization and threads

### Example

```C
/* Create "my_numeric_pix_prompt". */
status = gx_numeric_pixelmap_prompt_create(&my_numeric_pix_prompt,
    "my_numeric_pix_prompt", &my_parent,
    MY_TEXT_RESOURCE_ID, MY_FEEL_RESOURCE_ID,
    GX_STYLE_BORDER_RAISED, MY_NUMERIC_PIXELMAP_PROMPT_ID,
    &size);

/* If status is GX_SUCCESS the numeric pixelmap prompt "my_numeric_pix_prompt" has been created. */
```

### See Also

- gx_numeric_pixelmap_format_function_set
- gx_numeric_pixelmap_prompt_value_set

## gx_numeric_pixelmap_prompt_format_function_set


Override format function of numeric pixelmap prompt

### Prototype

```C
UINT gx_numeric_pixelmap_format_function_set(
    GX_NUMERIC_PIXELMAP_PROMPT *prompt,
    OID (*format_func)(GX_NUMERIC_PIXELMAP_PROMPT *, INT));
```

### Description

This service overrides the default format function of the numeric pixelmap prompt widget. The default format function converts the numeric pixelmap prompt value to a string and stores it in the widget's private buffer. This service allows the application to define its own format function to format and store the numeric pixelmap prompt value in the widget's private buffer.

### Parameters

- *prompt*: Numeric pixelmap prompt control block.
- *format_func*: Format function to be set.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set numeric pixelmap prompt format function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Define my numeric pixelmap format function. */
VOID my_format_function(GX_NUMERIC_PIXELMAP_PROMPT *prompt,
    INT value)
{
    /* If the value is "1234", the new format will be "12.34". */

    INT length;
    gx_utility_ltoa(value / 100,
        prompt->gx_numeric_pixelmap_prompt_buffer,
        GX_NUMERIC_PROMPT_ BUFFER_SIZE);
    Length = GX_STRLEN(prompt->gx_numeric_pixelmap_prompt_buffer);
    prompt->gx_numeric_pixelmap_prompt_buffer[length++] = '.';
    gx_utility_ltoa(value % 100,
        prompt->gx_numeric_pixelmap_prompt_buffer + length,
        GX_NUMERIC_PROMPT_BUFFER_SIZE - length);
}

/* Override default format function of "my_numeric_pix_prompt". */
status = gx_numeric_pixelmap_prompt_format_function_set(
    &my_numeric_pix_prompt,
    my_format_function);

/* If status is GX_SUCCESS the format function of "my_numeric_pix_prompt" has been override. */
```

### See Also

- gx_numeric_pixelmap_prompt_create
- gx_numeric_pixelmap_prompt_value_set

## gx_numeric_pixelmap_prompt_value_set


Set numeric pixelmap prompt value

### Prototype

```C
UINT gx_numeric_pixelmap_prompt_value_set(
    GX_NUMERIC_PIXELMAP_PROMPT *prompt,
    INT value);
```

### Description

This service an integer value to a numeric pixelmap prompt.

### Parameters

- *prompt*: Numeric pixelmap prompt control block.
- *value*: Integer value to be set.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set numeric pixelmap prompt value.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set a value to "my_numeric_pix_prompt". */
status = gx_numeric_pixelmap_prompt_value_set(&my_numeric_pix_prompt, 1000);

/* If status is GX_SUCCESS the value of the numeric pixelmap prompt "my_numeric_pix_prompt" has been set. */
```

### See Also

- gx_numeric_pixelmap_prompt_create
- gx_numeric_pixelmap_format_function_set

## gx_numeric_prompt_create


Create numeric prompt

### Prototype

```C
UINT gx_numeric_prompt_create( 
    GX_NUMERIC_PROMPT *prompt,
    GX_CONST GX_CHAR name, GX_WIDGET *parent,
    GX_RESOURCE_ID text_id,
    ULONG style, USHORT prompt_id,
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a numeric prompt widget. A numeric_ prompt is just a prompt that keeps its own buffer and provides a gx_numeric_ prompt_value_set(INT) API, the buffer size is defined by the constant GX_NUMERIC_PROMPT_BUFFER_SIZE, which defaults to 16.

GX_NUMERIC_PROMPT is derived from GX_PROMPT and supports all gx_prompt API services.

### Parameters

- *prompt*: Numeric prompt control block.
- *name*: Name of prompt.
- *parent*: Parent widget control block.
- *text_id*: Resource string ID.
- *style*: Style of numeric prompt, **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *prompt_id*: Application-defined ID of prompt.
- *size*: Dimensions of numeric prompt.

### Return Values

- **GX_SUCCESS** (0x00) Successfully creat numeric prompt.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.

### Allowed From

Initialization and threads

### Example

```C
/* Create "my_numeric _prompt". */
status = gx_numeric_prompt_create(&my_numeric_prompt,
    "my_numeric_prompt", &my_parent,
    MY_TEXT_RESOURCE_ID, GX_STYLE_BORDER_RAISED, MY_NUMERIC_PROMPT_ID,
    &size);

/* If status is GX_SUCCESS the numeric prompt "my_numeric_prompt" has been created. */
```

### See Also

- gx_numeric_format_function_set
- gx_numeric_prompt_value_set

## gx_numeric_prompt_format_function_set


Override format function of numeric prompt

### Prototype

```C
UINT gx_numeric_format_function_set(
    GX_NUMERIC_PROMPT *prompt,
    VOID (*format_func)(GX_NUMERIC_PROMPT *, INT));
```

### Description

This service overrides the default format function of a numeric prompt widget. The default format function converts the numeric prompt value to a string and stores it in the widget's private buffer. This service allows the application to define its own format function to format and store the numeric prompt value in the widget's private buffer.

### Parameters

- *prompt*: Numeric prompt control block.
- *format_func*: Format function to be set.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set numeric prompt format function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Define my numeric format function. */
VOID my_format_function(GX_NUMERIC_PROMPT *prompt, INT value)
{
    /* If the value is "1234", the new format will be "12.34". */

    INT length;
    gx_utility_ltoa(value / 100, prompt->gx_numeric_prompt_buffer,
        GX_NUMERIC_PROMPT_ BUFFER_SIZE);
    Length = GX_STRLEN(prompt->gx_numeric_prompt_buffer);
    prompt->gx_numeric_prompt_buffer[length++] = '.';
    gx_utility_ltoa(value % 100,
        prompt->gx_numeric_prompt_buffer + length,
        GX_NUMERIC_PROMPT_BUFFER_SIZE - length);
}

/* Override the default format function of "my_numeric_prompt". */
status = gx_numeric_prompt_format_function_set(&my_numeric_prompt,
    my_format_function);

/* If status is GX_SUCCESS, the format function of "my_numeric_prompt" has been override. */
```

### See Also

- gx_numeric_prompt_create
- gx_numeric_prompt_value_set

## gx_numeric_prompt_value_set


Set numeric prompt value

### Prototype

```C
UINT gx_numeric_prompt_value_set(
    GX_NUMERIC_PROMPT *prompt, 
    INT value);
```

### Description

This service sets an integer value to a numeric prompt.

### Parameters

- *prompt*: Numeric prompt control block.
- *value*: Integer value to be set.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set numeric prompt value.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set a value to "my_numeric_prompt". */
status = gx_numeric_prompt_value_set(&my_numeric_prompt, 1000);

/* If status is GX_SUCCESS the value of the numeric prompt "my_numeric_prompt" has been set. */
```

### See Also

- gx_numeric_prompt_create
- gx_numeric_format_function_set

## gx_numeric_scroll_wheel_create


Create numeric scroll wheel

### Prototype

```C
UINT gx_numeric_scroll_wheel_create(
    GX_NUMERIC_SCROLL_WHEEL *wheel,
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent,
    INT start_val,
    INT end_val,
    ULONG style,
    USHORT wheel_id,
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a numeric scroll wheel widget.

A numeric scroll wheel is a type of scroll wheel widget that is specifically used for displaying a range of numbers. Other types of scroll wheel widgets are also available. Refer to the gx_scroll_wheel_create() API for more information about the scroll wheel widget hierarchy, widget types, and widget derivation.

GX_NUMERIC_SCROLL_WHEEL is derived from GX_TEXT_SCROLL_WHEEL and supports all gx_text_scroll_wheel and gx_scroll_wheel services.

All scroll wheel types generate GX_EVENT_LIST_SELECT events to their parent when the scroll wheel is scrolled.

A numeric scroll wheel will default to having "abs(end_val – start_val) + 1" rows. In other words, the scroll wheel will display every value between start_val and end_val, incrementing or decrementing by 1 with each row. Note that start_val can be greater or less than end_val, depending on which way the application wants the range to appear.

If the application wants to change the row increment, it can do this by calling gx_scroll_wheel_total_rows_set() after creating the numeric scroll wheel. For example, an application wanting to create a scroll wheel that displays the values years 1980 to 2020, incrementing by 5, might do this:

```C
gx_numeric_scroll_wheel_create(&wheel, GX_NULL, parent, 1980,
    2020, style, id, &size);

/* the years 1980 through 2020, inclusive, incrementing by 5 years, yields 9 total rows */

gx_scroll_wheel_total_rows_set(&wheel, 9);
```

### Parameters

- *wheel*: Pointer to numeric scroll wheel control block.
- *name*: Logical name of pixelmap button widget.
- *parent*: Pointer to the parent widget.
- *start_val*: Starting numeric value.
- *end_val*: Ending numeric value.
- *style*: Style of checkbox. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *wheel_id*: Application-defined ID of scroll wheel.
- *size*: Dimensions of scroll wheel widget.

### Return Values

- **GX_SUCCESS** (0x00) Successfully created numeric scroll wheel.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.

### Allowed From

Initialization and threads

### Example

```C
/* Create "year_wheel". */
status = gx_numeric_scroll_wheel_create(&year_wheel,
    "year_selector"", &parent,
    1980, 2040,
    GX_STYLE_ENABLED | GX_STYLE_TEXT_CENTER |
    GX_STYLE_TRANSPARENT | GX_STYLE_WRAP |
    GX_STYLE_TEXT_SCROLL_WHEEL_ROUND,
    YEAR_WHEEL_ID,
    &size);

/* If status is GX_SUCCESS the scroll wheel "year_wheel" has been created. */
```

### See Also

- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_total_rows_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set
- gx_string_scroll_wheel_create
- gx_string_scroll_wheel_text_get

## gx_numeric_scroll_wheel_range_set


Assign value range of numeric scroll wheel

### Prototype

```C
UINT
gx_numeric_scroll_wheel_range_set(
    GX_NUMERIC_SCROLL_WHEEL *wheel,
    INT start_val, INT end_val);
```

### Description

This service modifies the range of values allowed and displayed by a numeric scroll wheel widget.

A numeric scroll wheel is a type of scroll wheel widget that is specifically used for displaying a range of numbers. Other types of scroll wheel widgets are also available. Refer to the gx_scroll_wheel_create() API for more information about the scroll wheel widget hierarchy, widget types, and widget derivation.

Invoking this API resets the scroll wheel total rows to abs(end_val – start_val) + 1, meaning the scroll wheel will increment by 1 for each row. To change this, the application can call gx_scroll_wheel_total_rows_set() to change the total number of rows, effectively changing the value increment between rows.

### Parameters

- *wheel*: Pointer to numeric scroll wheel control block.
- *start_val*: Starting numeric value.
- *end_val*: Ending numeric value.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set numeric scroll wheel range.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.

### Allowed From

Initialization and threads 

### Example

```C
/* Change range of "rate" scroll wheel. */

status = gx_numeric_scroll_wheel_range_set(&year_wheel, 0, 200);

/* If status is GX_SUCCESS the scroll wheel range has been modified. */
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_total_rows_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set
- gx_string_scroll_wheel_create
- gx_string_scroll_wheel_event_process
- gx_string_scroll_wheel_text_get

## gx_pixelmap_button_create


Create pixelmap button

### Prototype

```C
UINT gx_pixelmap_button_create(
    GX_PIXELMAP_BUTTON *button,
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent,
    GX_RESOURCE_ID normal_id,
    GX_RESOURCE_ID selected_id,
    GX_RESOURCE_ID disabled_id,
    ULONG style,
    USHORT pixelmap_button_id,
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a pixelmap button widget.

GX_PIXELMAP_BUTTON is derived from GX_BUTTON and supports all gx_button services.

### Parameters

- *button*: Pointer to pixelmap button control block.
- *name*: Logical name of pixelmap button widget.
- *parent*: Pointer to the parent widget.
- *normal_id*: Normal state Resource ID.
- *selected_id*: Selected state Resource ID.
- *disabled_id*: Disabled state Resource ID.
- *style*: Style of checkbox. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *pixelmap_button_id*: Application-defined ID of pixelmap button.
- *size*: Dimensions of pixelmap button.

### Return Values

- **GX_SUCCESS** (0x00) Successfully created pixelmap button.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.

### Allowed From

Initialization and threads

### Example

```C
/* Create "my_pixelmap_button". */
status = gx_pixelmap_button_create(&my_pixelmap_button,
    "my_pixelmap_button", &my_parent,
    MY_NORMAL_RESOURCE_ID, MY_SELECTED_RESOURCE_ID,
    MY_DESELECTED_RESOURCE_ID, GX_STYLE_BORDER_RAISED,
    MY_PIXELMAP_BUTTON_ID,
    &size);

/* If status is GX_SUCCESS the pixelmap button "my_pixelmap_button" has been created. */
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_pixelmap_button_draw
- gx_pixelmap_button_pxielmap_set
- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_radio_button_create
- gx_radio_button_draw
- gx_icon_button_create
- gx_text_button_create
- gx_text_button_color_set
- gx_text_button_draw

## gx_pixelmap_button_draw


Draw pixelmap button

### Prototype

```C
VOID gx_pixelmap_button_draw(GX_PIXELMAP_BUTTON *button);
```

### Description

This service draws a pixelmap button widget. This function is normally called internally by the GUIX canvas refresh mechanism, but is exposed to the application to assist with implementing custom drawing functions for custom pixelmap button widgets.

### Parameters

- *button*: Pointer to pixelmap button control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom pixelmap button drawing function. */

VOID my_pixelmap_button_draw(GX_PIXELMAP_BUTTON *button)
{
    /* Call default pixelmap button draw. */
    gx_pixelmap_button_draw(button);

    /* Add your own drawing here. */
}
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_pixelmap_button_create
- gx_pixelmap_button_pxielmap_set
- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_radio_button_create
- gx_radio_button_draw
- gx_icon_button_create
- gx_text_button_create
- gx_text_button_color_set
- gx_text_button_draw

## gx_pixelmap_button_event_process


Pixelmap button event processing

### Prototype

```C
UINT gx_pixelmap_button_event_process(
    GX_PIXELMAP_BUTTON *button,
    GX_EVENT *event_ptr);
```

### Description

This service provides default event handling for the pixelmap button widget type.

### Parameters

- *button*: Pointer to pixelmap button control block.
- *event_ptr*: Pointer to GX_EVENT structure.

### Return Values

- **GX_SUCCESS** (0x00) Successful pixelmap button draw.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
switch(event_ptr->gx_event_type)
{
    case GX_EVENT_SHOW:
        /* Do default handling. */
        status = gx_pixelmap_button_event_process(icon, event_ptr);
    
        /* Add my own handling here. */
        break;
}
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_pixelmap_button_create
- gx_pixelmap_button_pxielmap_set
- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_radio_button_create
- gx_radio_button_draw
- gx_icon_button_create
- gx_text_button_create
- gx_text_button_color_set
- gx_text_button_draw

## gx_pixelmap_button_pixelmap_set


Assign pixelmaps to button

### Prototype

```C
UINT gx_pixelmap_button_pixelmap_set(
    GX_PIXELMAP_BUTTON *button, 
    GX_RESOURCE_ID normal_id,
    GX_RESOURCE_ID selected_id, 
    GX_RESOURCE_ID disabled_id);
```

### Description

This service sets pixelmaps to the pixelmap button.

### Parameters

- *button*: Pointer to pixelmap button control block.
- *normal_id*: Resource ID of the pixelmap to be used as normal state.
- *selected_id*: Resource ID of the pixelmap to be used when the button is selected.
- *disabled_id*: Resource ID of the pixelmap to be used when the button is disabled.

### Return Values

- **GX_SUCCESS** (0x00) Successful sets the pixelmap to the button.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Draw "my_pixelmap_button". */
status = gx_pixelmap_button_pixelmap_set (&my_pixelmap_button,
    NORMAL_ID, SELECTED_ID,
    DISABLED_ID);

/* If status is GX_SUCCESS the pixelmap button is properly configured with the specified pixelmaps. */
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_radio_button_create
- gx_radio_button_draw
- gx_icon_button_create
- gx_text_button_create
- gx_text_button_color_set
- gx_text_button_draw

## gx_pixelmap_prompt_create


Create pixelmap prompt

### Prototype

```C
UINT gx_pixelmap_prompt_create(
    GX_PIXELMAP_PROMPT *prompt,
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent,
    GX_RESOURCE_ID text_id,
    GX_RESOURCE_ID fill_pixelmap_id,
    ULONG style,
    USHORT pixelmap_prompt_id,
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a pixelmap prompt widget. A pixelmap prompt differs from a standard GX_PROMPT in that it paints the background of the prompt using pixelmaps. The create function accepts one pixelmap ID, the normal state fill pixelmap. Up to six pixelmaps may be assigned to the pixelmap prompt.

### Parameters

- *prompt*: Pointer to pixelmap prompt control block.
- *name*: Logical name of pixelmap prompt widget.
- *parent*: Pointer to the parent widget.
- *text_id*: Resource ID of text.
- *fill_id*: Resource ID of fill.
- *style*: Style of checkbox. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *pixelmap_prompt_id*: Application-defined ID of pixelmap prompt.
- *size*: Dimensions of pixelmap prompt.

### Return Values

- **GX_SUCCESS** (0x00) Successful pixelmap prompt create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.
- GX_ALREADY_CREATED (0x13) Widget already created.

### Allowed From

Initialization and threads

### Example

```C
/* Create "my_pixelmap_prompt". */
status = gx_pixelmap_prompt_create(&my_pixelmap_prompt,
    "my_pixelmap_prompt", &my_parent,
    MY_TEXT_RESOURCE_ID, MY_LEFT_RESOURCE_ID,
    MY_FILL_RESOURCE_ID, MY_RIGHT_RESOURCE_ID,
    GX_STYLE_BORDER_RAISED, MY_PIXELMAP_PROMPT_ID,
    &size);

/* If status is GX_SUCCESS the pixelmap prompt "my_pixelmap_prompt" has been created. */
```

### See Also

- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_pixelmap_button_pixelmap_set
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_prompt_create
- gx_prompt_draw
- gx_prompt_font_set
- gx_prompt_text_color_set
- gx_prompt_text_get
- gx_prompt_text_set

## gx_pixelmap_prompt_draw


Draw pixelmap prompt

### Prototype

```C
VOID gx_pixelmap_prompt_draw(GX_PIXELMAP_PROMPT *prompt);
```

### Description

This service draws a pixelmap prompt widget. This function is normally called internally by the GUIX canvas refresh mechanism, but is exposed to the application to assist with implementing custom drawing functions for custom pixelmap prompt widgets.

### Parameters

- *prompt*: Pointer to pixelmap prompt control block.

### Return Values

- None

### Allowed From

Initialization and threads

### Example

```C
/* Write a custom pixelmap prompt drawing function. */

VOID my_pixelmap_button_draw(GX_PIXELMAP_PROMPT *prompt)
{
    /* Call default pixelmap prompt draw. */
    gx_pixelmap_prompt_draw(prompt);

    /* Add your own drawing here. */
}
```

### See Also

- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_pixelmap_button_pixelmap_set
- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_pixelmap_set
- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_prompt_create
- gx_prompt_draw
- gx_prompt_font_set
- gx_prompt_text_color_set
- gx_prompt_text_get
- gx_prompt_text_set

## gx_pixelmap_prompt_pixelmap_set


Assign pixelmaps to prompt

### Prototype

```C
UINT gx_pixelmap_prompt_pixelmap_set(
    GX_PIXELMAP_PROMPT *prompt,
    GX_RESOURCE_ID normal_left_pixelmap,
    GX_RESOURCE_ID normal_fill_pixelmap,
    GX_RESOURCE_ID normal_right_pixelmap,
    GX_RESOURCE_ID selected_left_pixelmap,
    GX_RESOURCE_ID selected_fill_pixelmap,
    GX_RESOURCE_ID selected_right_pixelmap);
```

### Description

This service assigns pixelmap IDs to the pixelmap prompt. The left, fill, and right pixelmap IDs are used to allow the application to use one set of pixelmaps for prompts of various widths but a common height to save on storage requirements. If the left and right IDs are not used, they should be set to 0. If the prompt should draw itself differently when it gains input focus, the selected pixelmap IDs are used for that purpose. If the selected IDs are not used or are the same as the normal IDs, set them to 0.

### Parameters

- *prompt*: Pointer to pixelmap prompt control block.
- *normal_left_id*: Resource ID of the pixelmap to be used on the left side in the normal state.
- *normal_fill_id*: Resource ID of the pixelmap to be used as a tiled fill in the normal state.
- *normal_right_id*: Resource ID of the pixelmap to be used on the right side in the normal state.
- *selected_left_id*: Resource ID of the pixelmap to be used on the left side in the selected state.
- *selected_fill_id*: Resource ID of the pixelmap to be used as a tiled fill in the selected state.
- *selected_right_id*: Resource ID of the pixelmap to be used on the right side in the selected state.

### Return Values

- **GX_SUCCESS** (0x00) Successful sets the pixelmap to the prompt.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Assign pixelmap IDs to "my_prompt". Only the normal state pixelmaps are used in this case. */
status = gx_pixelmap_prompt_pixelmap_set (&my_prompt,
    normal_left_id, normal_fill_id,
    normal_right_id, 0, 0, 0);

/* If status is GX_SUCCESS the pixelmap prompt is properly configured with pixelmaps. */
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_pixelmap_button_pixelmap_set
- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_radio_button_create
- gx_radio_button_draw
- gx_icon_button_create
- gx_text_button_create
- gx_text_button_color_set
- gx_text_button_draw

## gx_pixelmap_slider_create


Create pixelmap slider

### Prototype

```C
UINT gx_pixelmap_slider_create(
    GX_PIXELMAP_SLIDER *slider,
    GX_CONST GX_CHAR *name, GX_WIDGET *parent,
    GX_SLIDER_INFO *info,
    GX_PIXELMAP_SLIDER_INFO *pixelmap_info, ULONG style,
    USHORT pixelmap_slider_id,
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a pixelmap slider widget.

### Parameters

- *slider*: Pointer to pixelmap slider control block.
- *name*: Logical name of pixelmap slider widget.
- *parent*: Pointer to the parent widget.
- *info*: Pointer to a GX_SLIDER_INFO structure which contains values defining the slider minimum value, maximum value, current value, and needle limits. **Appendix I** contains definition for GX_SLIDER_INFO structure.
- *pixelmap_info*: Pointer to a GX_PIXELMAP_SLIDER_INFO structure which defines the pixelmaps used to draw the slider background and needle. **Appendix I** contains definition for GX_PIXELMAP_SLIDER_INFO structure. The slider background can use one or two pixelmaps. If one, the background does not change as the needle moves. If two backgrounds are defined, the background before the needle uses the first background pixelmap, and the background after the needle uses the second background pixelmap.
- *style*: Style of slider. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *pixelmap_slider_id*: Application-defined ID of pixelmap slider.
- *size*: Dimensions of pixelmap prompt.

### Return Values

- **GX_SUCCESS** (0x00) Successfully created pixelmap slider.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.

### Allowed From

Initialization and threads

### Example

```C
GX_SLIDER_INFO info;
GX_PIXELMAP_SLIDER_INFO pixelmap_info;

/* Initiate slider information structure. */
info.gx_slider_info_min_val = 0;
info.gx_slider_info_max_val = 100;
info.gx_slider_info_current_val = 50;
info.gx_slider_info_min_travel = 10;
info.gx_slider_info_max_tralvel = 10;
info.gx_slider_info_needle_width = 5;
info.gx_slider_info_needle_height = 10;
info.gx_slider_info_needle_inset = 5;
info.gx_slider_info_needle_hotspot_offset;

/* Initiate pixelmap slider information structure. */
pixelmap_info.gx_pixelmap_slider_info_lower_background_pixelmap =
                                        GX_PIXELMAP_ID_ORANGE;
pixelmap_info.gx_pixelmap_slider_info_upper_background_pixelmap =
                                        GX_PIXELMAP_ID_EMPTY;
pixelmap_info.gx_pixelmap_slider_info_needle_pixelmap =
                                        GX_PIXELMAP_ID_NEEDLE;

/* Create "my_pixelmap_slider". */
status = gx_pixelmap_slider_create(&my_pixelmap_slider,
                            "my_pixelmap_slider", &my_parent,
                            &info, &pixelmap_info,
                            GX_STYLE_BORDER_RAISED,
                            MY_PIXELMAP_SLIDER_ID, &size);

/* If status is GX_SUCCESS the pixelmap slider "my_pixelmap_slider" has been created. */
```

### See Also

- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_pixelmap_button_pixelmap_set
- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_slider_create
- gx_slider_draw
- gx_slider_event_process
- gx_slider_needle_draw
- gx_slider_needle_position_get
- gx_slider_needle_position_get
- gx_slider_tickmarks_draw
- gx_slider_travel_get
- gx_slider_value_calculate
- gx_slider_value_set

## gx_pixelmap_slider_draw


Draw pixelmap slider

### Prototype

```C
VOID gx_pixelmap_slider_draw(GX_PIXELMAP_SLIDER *slider);
```

### Description

This service draws a pixelmap slider widget. This function is normally called internally by the GUIX canvas refresh mechanism, but is exposed to the application to assist with implementing custom drawing functions for custom pixelmap slider widgets.

### Parameters

- *slider*: Pointer to pixelmap slider control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom pixelmap slider drawing function. */

VOID my_pixelmap_slider_draw(GX_PIXELMAP_SLIDER *pixelmap_slider)
{
    /* Call default pixelmap slider draw. */
    gx_pixelmap_slider_draw(pixelmap_slider);

    /* Add your own drawing here. */
}
```

### See Also

- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_pixelmap_button_pixelmap_set
- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_pixelmap_slider_create
- gx_pixelmap_slider_event_process
- gx_pixelmap_slider_pixelmap_set
- gx_slider_create
- gx_slider_draw
- gx_slider_event_process
- gx_slider_needle_draw
- gx_slider_needle_position_get
- gx_slider_needle_position_get
- gx_slider_tickmarks_draw
- gx_slider_travel_get
- gx_slider_value_calculate
- gx_slider_value_set

## gx_pixelmap_slider_event_process


Process pixelmap slider event

### Prototype

```C
UINT gx_pixelmap_slider_event_process(
    GX_PIXELMAP_SLIDER *slider,
    GX_EVENT *event);
```

### Description

This service processes an event for the specified pixelmap slider widget.

### Parameters

- *slider*: Pointer to pixelmap.
- *slider*: control block event Pointer to event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful pixelmap slider event process.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Write a custom event processing function. */
UINT my_event_hanlder(GX_PIXELMAP_SLIDER *pixelmap_slider, GX_EVENT *event_ptr)
{
    switch(event_ptr->gx_event_type)
    {
    case GX_EVENT_SHOW:
        /* Do default handling. */
        status = gx_pixelmap_slider_event_process(pixelmap_slider,
                                                    event_ptr);

        /* Add my own handling here. */
        break;
    default:
        status = gx_pixelmap_slider_event_process(pixelmap_slider,
                                                  event_ptr);
        break;
    }
    return status;
}
```

### See Also

- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_pixelmap_button_pixelmap_set
- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_pixelmap_set
- gx_slider_create
- gx_slider_draw
- gx_slider_event_process
- gx_slider_needle_draw
- gx_slider_needle_position_get
- gx_slider_needle_position_get
- gx_slider_tickmarks_draw
- gx_slider_travel_get
- gx_slider_value_calculate
- gx_slider_value_set

## gx_pixelmap_slider_pixelmap_set


Assign pixelmaps to slider

### Prototype

```C
UINT gx_pixelmap_slider_pixelmap_set(
    GX_PIXELMAP_SLIDER *slider,
    GX_PIXELMAP_SLIDER_INFO *pixinfo);
```

### Description

This service sets pixelmaps to the pixelmap slider.

### Parameters

- *slider*: Pointer to pixelmap slider control block.
- *pixinfo*: Pointer to a GX_PIXELMAP_SLIDER_INFO structure which defines the pixelmaps used to draw the slider background and needle. **Appendix I** contains definition for GX_PIXELMAP_SLIDER_INFO structure. The slider background can use one or two pixelmaps. If one, the background does not change as the needle moves. If two backgrounds are defined, the background before the needle uses the first background pixelmap, and the background after the needle uses the second background pixelmap.

### Return Values

- **GX_SUCCESS** (0x00) Successful sets the pixelmap to the slider.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_PIXELMAP_SLIDER_INFO pixelmap_info;

/* Initiate pixelmap slider information structure. */
pixelmap_info.gx_pixelmap_slider_info_lower_background_pixelmap =
                                        GX_PIXELMAP_ID_ORANGE;
pixelmap_info.gx_pixelmap_slider_info_upper_background_pixelmap =
                                        GX_PIXELMAP_ID_EMPTY;
pixelmap_info.gx_pixelmap_slider_info_needle_pixelmap =
                                        GX_PIXELMAP_ID_NEEDLE;

/* Draw "my_pixelmap_button". */
status = gx_pixelmap_slider _pixelmap_set (&my_pixelmap_slider,
                                &pixelmap_info);

/* If status is GX_SUCCESS the pixelmap slider is properly configured with "pixelmap_info". */
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_radio_button_create
- gx_radio_button_draw
- gx_icon_button_create
- gx_text_button_create
- gx_text_button_color_set
- gx_text_button_draw

## gx_progress_bar_background_draw


Draw progress bar background

### Prototype

```C
VOID gx_progress_bar_background_draw(GX_PROGRESS_BAR *progress_bar)
```

### Description

This service draws the background of the specified progress bar. This function is called internally as part of the gx_progress_bar_draw(), but is exposed to the application to support those cases where the application defines a custom progress bar drawing function.

### Parameters

- *progress bar*: Pointer to progress bar control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom progress bar drawing function. */

VOID my_progress_bar_draw(GX_PROGRESS_BAR *progress_bar)
{
    /* Call default progress bar background draw. */
    gx_progress_bar_background_draw(progress_bar);

    /* Call default progress bar text draw. */
    gx_progress_bar_text_draw(progress_bar);

    /* Add your own drawing here. */
}
```

### See Also

- gx_progress_bar_create
- gx_progress_bar_draw
- gx_progress_bar_event_process
- gx_progress_bar_font_set
- gx_progress_bar_info_set
- gx_progress_bar_pixelmap_set
- gx_progress_bar_range_set
- gx_progress_bar_text_color_set
- gx_progress_bar_value_set

## gx_progress_bar_create


Create a progress bar

### Prototype

```C
UINT gx_progress_bar_create(
    GX_PROGRESS_BAR *progress_bar,
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent,
    GX_PROGRESS_BAR_INFO *progress_bar_info,
    ULONG style, 
    USHORT progress_bar_id, 
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a progress bar widget.

### Parameters

- *progress_bar*: Progress bar control block.
- *name*: Logical name.
- *parent*: Pointer to the parent widget.
- *progress_bar_info*: Pointer to a GX_PROGRESS_BAR_INFO structure. **Appendix I** contains definition for GX_PROGRESS_BAR_INFO structure.
- *style*: Style of progress bar. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *progress_bar_id*: Application-defined ID of progress bar.
- *size*: Dimensions of progress bar.

### Return Values

- **GX_SUCCESS** (0x00) Successful progress bar create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.
- GX_INVALID_WIDGET (0x12) Parent widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_PROGRESS_BAR_INFO info;
GX_RECTANGLE size;

info.gx_progress_bar_info_min_val = 0;
info.gx_progress_bar_info_max_val = 100;
info.gx_progress_bar_info_current_val = 0;
info.gx_progress_bar_font_id = GX_FONT_ID_SYSTEM_FONT;
info.gx_progress_bar_normal_text_color = GX_COLOR_ID_WHITE;
info.gx_progress_bar_selected_text_color = GX_COLOR_ID_BLUE;
info.gx_progress_bar_fill_pixelmap = 0;

size.gx_rectangle_left = 10;
size.gx_rectangle_top = 10;
size.gx_rectangle_right = 110;
size.gx_rectangle_bottom = 140;

/* Create a progress bar with the specified information. */
status = gx_progress_bar_create(&my_progress_bar, GX_NULL, GX_NULL,
                        &info, GX_STYLE_BORDER_THIN,
                        0, &size);

/* If status is GX_SUCSESS the progress bar "my_progress_bar" has been successfully created. */
```

### See Also

- gx_progress_bar_draw
- gx_progress_bar_event_process
- gx_progress_bar_font_set
- gx_progress_bar_info_set
- gx_progress_bar_pixelmap_set
- gx_progress_bar_range_set
- gx_progress_bar_text_color_set
- gx_progress_bar_text_draw
- gx_progress_bar_value_set

## gx_progress_bar_draw


Draw a progress bar

### Prototype

```C
VOID gx_progress_bar_draw(GX_PROGRESS_BAR *progress_bar);
```

### Description

This service draws a progress bar widget. This function is normally called internally by the GUIX canvas refresh mechanism, but is exposed to the application to assist with implementing custom drawing functions for custom progress bar widgets.

### Parameters

- *progress_bar*: Progress bar control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom progress bar drawing function. */

VOID my_progress_bar_draw(GX_PROGRESS_BAR *progress_bar)
{
    /* Call default progress bar draw. */
    gx_progress_bar_draw(progress_bar);

    /* Add your own drawing here. */
}
```

### See Also

- gx_progress_bar_create
- gx_progress_bar_event_process
- gx_progress_bar_font_set
- gx_progress_bar_info_set
- gx_progress_bar_pixelmap_set
- gx_progress_bar_range_set
- gx_progress_bar_text_color_set
- gx_progress_bar_text_draw
- gx_progress_bar_value_set

## gx_progress_bar_event_process


Progress a progress bar event

### Prototype

```C
UINT gx_progress_bar_event_process(
    GX_PROGRESS_BAR *progress_bar,
    GX_EVENT *event_ptr);
```

### Description

This service processes a progress bar event.

### Parameters

- *progress_bar*: Progress bar control block.
- *event_ptr*: Pointer to GX_EVENT structure.

### Return Values

- **GX_SUCCESS** (0x00) Successful prompt create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Initialization and threads

### Example

```C
/* Write a custom event processing function. */

UINT my_event_process (GX_PROGRESS_BAR *progress_bar, GX_EVENT *event_ptr)
{
    switch(event_ptr->gx_event_type)
    {
    case GX_EVENT_SHOW:
        /* Do default handling. */
        status = gx_progress_bar_event_process(progress_bar,
                                                event_ptr);
        /* Add my own handling here. */
        break;
    default:
        status = gx_progress_bar_event_process(progress_bar,
                                                event_ptr);
        break;
    }
    return status;
}
```

### See Also

- gx_progress_bar_create
- gx_progress_bar_event_draw
- gx_progress_bar_font_set
- gx_progress_bar_info_set
- gx_progress_bar_pixelmap_set
- gx_progress_bar_range_set
- gx_progress_bar_text_color_set
- gx_progress_bar_text_draw
- gx_progress_bar_value_set

## gx_progress_bar_font_set


Set font of progress bar text

### Prototype

```C
UINT gx_progress_bar_font_set(
    GX_PROGRESS_BAR *progress_bar,
    GX_RESOURCE_ID font_id);
```

### Description

This service sets the font of a progress bar widget.

### Parameters

- *progress_bar*: Progress bar control block.
- *font_id*: Font resource ID.

### Return Values

- **GX_SUCCESS** (0x00) Successful progress bar font set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
UINT status = gx_progress_bar_font_set(&progress_bar,
                                        GX_FONT_ID_MEDIUM);

/* If status is GX_SUCCESS, the font was successfully assigned. */
```

### See Also

- gx_progress_bar_create
- gx_progress_bar_draw
- gx_progress_bar_event_process
- gx_progress_bar_info_set
- gx_progress_bar_pixelmap_set
- gx_progress_bar_range_set
- gx_progress_bar_text_color_set
- gx_progress_bar_text_draw
- gx_progress_bar_value_set

## gx_progress_bar_info_set


Set progress bar information structure

### Prototype

```C
UINT gx_progress_bar_info_set(
    GX_PROGRESS_BAR *progress_bar,
    GX_PROGRESS_BAR_INFO *info);
```

### Description

This service resets the information structure of a progress bar widget.

### Parameters

- *progress_bar*: Progress bar control block.
- *info*: Pointer to a GX_PROGRESS_BAR_INFO structure. **Appendix I** contains definition for GX_PROGRESS_BAR_INFO structure.

### Return Values

- **GX_SUCCESS** (0x00) Successfully reset progress bar info.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_PROGRESS_BAR_INFO info;

info.gx_progress_bar_info_min_val = 0;
info.gx_progress_bar_info_max_val = 100;
info.gx_progress_bar_info_current_val = 0;
info.gx_progress_bar_font_id = GX_FONT_ID_SYSTEM_FONT;
info.gx_progress_bar_normal_text_color = GX_COLOR_ID_WHITE;
info.gx_progress_bar_selected_text_color = GX_COLOR_ID_BLUE;
info.gx_progress_bar_fill_pixelmap = 0;

status = gx_progress_bar_info_set(&progress_bar, &info);

/* If status == GX_SUCCESS the progress bar info was re-assigned. */
```

### See Also

- gx_progress_bar_info_create
- gx_progress_bar_draw
- gx_progress_bar_event_process
- gx_progress_bar_font_set
- gx_progress_bar_pixelmap_set
- gx_progress_bar_range_set
- gx_progress_bar_text_color_set
- gx_progress_bar_text_draw
- gx_progress_bar_value_set

## gx_progress_bar_pixelmap_set


Set pixelmap used to draw progress bar

### Prototype

```C
UINT gx_progress_bar_pixelmap_set(
    GX_PROGRESS_BAR *progress_bar,
    GX_RESOURCE_ID pixelmap_id);
```

### Description

This service sets the pixelmap used to fill the progress bar background.

### Parameters

- *progress_bar*: Progress bar control block.
- *pixelmap_id*: Pixelmap resource ID.

### Return Values

- **GX_SUCCESS** (0x00) Successful progress bar pixelmap set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
UINT status = gx_progress_bar_pixelmap_set(&progress_bar,
                                GX_PIXELMAP_ID_PROGRESS_FILL);

/* If status is GX_SUCCESS, the pixelmap was successfully assigned. */
```

### See Also

- gx_progress_bar_pixelmap_create
- gx_progress_bar_draw
- gx_progress_bar_event_process
- gx_progress_bar_font_set
- gx_progress_bar_info_set
- gx_progress_bar_range_set
- gx_progress_bar_text_color_set
- gx_progress_bar_text_draw
- gx_progress_bar_value_set

## gx_progress_bar_range_set


Set value range of a progress bar

### Prototype

```C
UINT gx_progress_bar_range_set(
    GX_PROGRESS_BAR *progress_bar,
    INT min_value, 
    INT max_value);
```

### Description

This service sets the progress bar value range.

### Parameters

- *progress_bar*: Progress bar control block.
- *min_value*: Progress bar minimum value.
- *max_value*: Progress bar maximum value.

### Return Values

- **GX_SUCCESS** (0x00) Successful progress bar range set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
UINT status = gx_progress_bar_range_set(progress_bar, 0, 100);

/* If status is GX_SUCCESS, the progress bar range was successfully assigned. */
```

### See Also

- gx_progress_bar_range_create
- gx_progress_bar_draw
- gx_progress_bar_event_process
- gx_progress_bar_font_set
- gx_progress_bar_info_set
- gx_progress_bar_pixelmap_set
- gx_progress_bar_text_color_set
- gx_progress_bar_text_draw
- gx_progress_bar_value_set

## gx_progress_bar_text_color_set


Set the text color of a progress bar

### Prototype

```C
UINT gx_progress_bar_text_color_set(
    GX_PROGRESS_BAR *progress_bar,
    GX_RESOURCE_ID normal_text_color,
    GX_RESOURCE_ID selected_text_color,
    GX_RESOURCE_ID disabled_text_color);
```

### Description

This service sets the text color of a progress bar widget.

### Parameters

- *progress_bar*: Progress bar control block.
- *normal_text_color*: Resource ID of normal text color that used in normal state.
- *selected_text_color*: Resource ID of selected text color that used when the widget gain focus.
- *disabled_text_color*: Resource ID of disabled text color that used when GX_STYLE_ENABLED is not active.

### Return Values

- **GX_SUCCESS** (0x00) Successful progress bar text color set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set text colors for the progress bar "my_progress_bar"*/
UINT status = gx_progress_bar_text_color_set(&my_progress_bar,
                        GX_COLOR_ID_NORMAL_TEXT,
                        GX_COLOR_ID_SELECTED_TEXT,
                        GX_COLOR_ID_DISABLED_TEXT);

/* If status is GX_SUCCESS, the progress bar text colors were successfully assigned. */
```

### See Also

- gx_progress_bar_create
- gx_progress_bar_draw
- gx_progress_bar_event_process
- gx_progress_bar_font_set
- gx_progress_bar_info_set
- gx_progress_bar_pixelmap_set
- gx_progress_bar_range_set
- gx_progress_bar_text_draw
- gx_progress_bar_value_set

## gx_progress_bar_text_draw


Draw progress bar text

### Prototype

```C
VOID gx_progress_bar_text_draw(GX_PROGRESS_BAR *progress_bar)
```

### Description

This service draws the text of specified progress bar. This function is called internally as part of the gx_progress_bar_draw(), but is exposed to the application to support those cases where the application defines a custom progress bar drawing function.

### Parameters

- *progress bar*: Pointer to progress bar control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom progress bar drawing function. */

VOID my_progress_bar_draw(GX_PROGRESS_BAR *progress_bar)
{
    /* Call default progress bar background draw. */
    gx_progress_bar_background_draw(progress_bar);

    /* Call default progress bar text draw. */
    gx_progress_bar_text_draw(progress_bar);

    /* Add your own drawing here. */
}
```

### See Also

- gx_progress_bar_create
- gx_progress_bar_draw
- gx_progress_bar_event_process
- gx_progress_bar_font_set
- gx_progress_bar_info_set
- gx_progress_bar_pixelmap_set
- gx_progress_bar_range_set
- gx_progress_bar_text_color_set
- gx_progress_bar_value_set

## gx_progress_bar_value_set


Set current value of a progress bar

### Prototype

```C
UINT gx_progress_bar_value_set(
    GX_PROGRESS_BAR *progress_bar,
    INT value);
```

### Description

This service assigns the progress bar current value. The progress bar widget will automatically invalidate and redraw itself when the progress bar value is changed.

### Parameters

- *progress_bar*: Progress bar control block.
- *value*: Progress bar current value.

### Return Values

- **GX_SUCCESS** (0x00) Successful set the value of the progress bar.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
UINT status = gx_progress_bar_value_set(progress_bar, 50);

/* If status == GX_SUCCESS the progress bar value was successfully assigned. */
```

### See Also

- gx_progress_bar_value_create
- gx_progress_bar_draw
- gx_progress_bar_event_process
- gx_progress_bar_font_set
- gx_progress_bar_info_set
- gx_progress_bar_pixelmap_set
- gx_progress_bar_range_set
- gx_progress_bar_text_color_set
- gx_progress_bar_text_draw

## gx_prompt_create


Create prompt

### Prototype

```C
UINT gx_prompt_create(
    GX_PROMPT *prompt, 
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent, 
    GX_RESOURCE_ID text_id,
    ULONG style, 
    USHORT prompt_id,
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a prompt widget.

GX_PROMPT is derived from GX_WIDGET and supports all gx_widget services.

### Parameters

- *Pointer*: to the parent widget.
- *text_id*: Resource ID of prompt text.
- *style*: Style of prompt. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *prompt_id*: Application-defined ID of prompt.
- *size*: Dimensions of prompt.

### Return Values

- **GX_SUCCESS** (0x00) Successful prompt create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.

### Allowed From

Initialization and threads

### Example

```C
/* Create "my_prompt". */
status = gx_prompt_create(&my_prompt, "my_promPt", &my_parent,
                    MY_PROMPT_TEXT_RESOURCE_ID,
                    GX_STYLE_BORDER_RAISED, MY_PROPMT_ID, &size);

/* If status is GX_SUCCESS the prompt "my_prompt" has been created. */
```

### See Also

- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_prompt_draw
- gx_prompt_event_process
- gx_prompt_font_set
- gx_prompt_text_color_set
- gx_prompt_text_get
- gx_prompt_text_id_set
- gx_prompt_text_set

## gx_prompt_draw


Draw prompt

### Prototype

```C
VOID gx_prompt_draw(GX_PROMPT *prompt);
```

### Description

This service draws a prompt widget. This service is called internally by GUIX during canvas refresh, but can also be called by custom drawing functions.

### Parameters

- *prompt*: Pointer to prompt widget control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom prompt drawing function. */

VOID my_prompt_draw(GX_PROMPT *prompt)
{
    /* Call default prompt draw. */
    gx_prompt_draw(prompt);

    /* Add your own drawing here. */
}
```

### See Also

- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_prompt_create
- gx_prompt_event_process
- gx_prompt_font_set
- gx_prompt_text_color_set
- gx_prompt_text_get
- gx_prompt_text_id_set
- gx_prompt_text_set

## gx_prompt_event_process


Process prompt event

### Prototype

```C
UINT gx_prompt_event_process(GX_PROMPT *prompt, GX_EVENT *event_ptr);
```

### Description

This service processes an event for the specified prompt. This service should be called as the default event handler by any custom prompt event processing functions.

### Parameters

- *prompt*: Pointer to prompt control block.
- *event_ptr*: Pointer to the event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful prompt event process.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Call generic prompt event processing as part of custom event processing function. */

UINT custom_prompt_event_process(GX_PROMPT *prompt, GX_EVENT *event)
{
    UINT status = GX_SUCCESS;

    switch(event->gx_event_type)
    {
    case xyz:
        /* Insert custom event handling here. */
        break;

    default:
        /* Pass all other events to the default prompt event processing. */
        status = gx_prompt_event_process(prompt, event);
        break;
    }
    return status;
}
```

### See Also

- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_prompt_create
- gx_prompt_draw
- gx_prompt_font_set
- gx_prompt_text_color_set
- gx_prompt_text_get
- gx_prompt_text_id_set
- gx_prompt_text_set

## gx_prompt_font_set


Set prompt font

### Prototype

```C
UINT gx_prompt_font_set(
    GX_PROMPT *prompt, 
    GX_RESOURCE_ID font_id);
```

### Description

This service sets the font of a prompt widget.

### Parameters

- *prompt*: Pointer to prompt widget control block.
- *font_id*: Resource ID of font.

### Return Values

- **GX_SUCCESS** (0x00) Successful prompt font set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set the font of "my_prompt". */
status = gx_prompt_font_set(&my_prompt, MY_PROMPT_FONT_ID);

/* If status is GX_SUCCESS the font for prompt "my_prompt" has been set. */
```

### See Also

- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_prompt_create
- gx_prompt_draw
- gx_prompt_event_process
- gx_prompt_text_color_set
- gx_prompt_text_get
- gx_prompt_text_id_set
- gx_prompt_text_set

## gx_prompt_text_color_set


Set prompt text color

### Prototype

```C
UINT gx_prompt_text_color_set(
    GX_PROMPT *prompt,
    GX_RESOURCE_ID normal_color,
    GX_RESOURCE_ID selected_color,
    GX_RESOURCE_ID disabled_color);
```

### Description

This service sets the text color of a prompt widget.

### Parameters

- *prompt*: Pointer to prompt widget control block.
- *normal_color*: Resource ID of color for normal text. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.
- *selected_color*: Resource ID of color for selected text, used when the widget gain focus. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.
- *disabled_color*: Resource ID of color for disabled text, used when GX_STYLE_ENABLED is not active. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.

### Return Values

- **GX_SUCCESS** (0x00) Successful prompt text color set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Initialization and threads

### Example

```C
/* Set the text color of "my_prompt". */
status = gx_prompt_text_color_set(&my_prompt,
                    GX_COLOR_ID_BLACK,
                    GX_COLOR_ID_LIGHTGRAY,
                    GX_COLOR_ID_DISABLED_TEXT);

/* If status is GX_SUCCESS the text color for prompt "my_prompt" has been set. */
```

### See Also

- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_prompt_create
- gx_prompt_draw
- gx_prompt_event_process
- gx_prompt_font_set
- gx_prompt_text_get
- gx_prompt_text_id_set
- gx_prompt_text_set

## gx_prompt_text_draw


Drawing support function

### Prototype

```C
VOID gx_prompt_text_draw(GX_PROMPT *prompt);
```

### Description

This support function draws the text portion of a prompt. This function is called internally by gx_prompt_draw(), and is provided as a separate API as a convenience for applications that define a custom prompt drawing function. Applications that want to customize the prompt background drawing can provide their custom drawing function, and invoke the gx_prompt_text_draw service as part of their custom drawing to draw the prompt text over the background.

### Parameters

- *prompt*: Pointer to the prompt control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Define a custom drawing function. */
VOID my_prompt_draw(GX_PROMPT *prompt)
{
    /* Insert code here to draw prompt background. */

    /* Call support function to do text drawing. */
    gx_prompt_text_draw();

    /* Draw child widgets. */
    gx_widget_children_draw((GX_WIDGET *) prompt);
}
```

### See Also

- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_prompt_create
- gx_prompt_draw
- gx_prompt_event_process
- gx_prompt_font_set
- gx_prompt_text_color_set
- gx_prompt_text_id_set
- gx_prompt_text_set

## gx_prompt_text_get


Get prompt text (deprecated)

### Prototype

```C
UINT gx_prompt_text_get(
    GX_PROMPT *prompt, 
    GX_CHAR **return_text);
```

### Description

This service is deprecated in favor of gx_prompt_text_get_ext().

This service gets the text of a prompt widget.

### Parameters

- *prompt*: Pointer to prompt widget control block.
- *return_text*: Pointer to destination for text.

### Return Values

- **GX_SUCCESS** (0x00) Successful prompt text get.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_PROMPT my_prompt;
GX_CHAR *my_prompt_text;

/* Get the text of "my_prompt". */
status = gx_prompt_text_get(&my_prompt, &my_prompt_text);

/* If status is GX_SUCCESS the pointer "my_prompt_text" points to the text displayed by "my_prompt". */
```

### See Also

- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_prompt_create
- gx_prompt_draw
- gx_prompt_event_process
- gx_prompt_font_set
- gx_prompt_text_color_set
- gx_prompt_text_id_set
- gx_prompt_text_set

## gx_prompt_text_get_ext


Get prompt text

### Prototype

```C
UINT gx_prompt_text_get(
    GX_PROMPT *prompt, 
    GX_STRING *return_string);
```

### Description

This service gets the string of a prompt widget.

### Parameters

- *prompt*: Pointer to prompt widget control block.
- *return_string*: Pointer to destination for string.

### Return Values

- **GX_SUCCESS** (0x00) Successful prompt text get.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_PROMPT my_prompt;
GX_STRING my_prompt_string;

/* Get the text of "my_prompt". */
status = gx_prompt_text_get_ext(&my_prompt, &my_prompt_string);

/* If status is GX_SUCCESS then my_prompt_string has been initialize to hold a copy of the prompt string. */
```

### See Also

- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_prompt_create
- gx_prompt_draw
- gx_prompt_event_process
- gx_prompt_font_set
- gx_prompt_text_color_set
- gx_prompt_text_id_set
- gx_prompt_text_set

## gx_prompt_text_id_set


Set prompt text ID

### Prototype

```C
UINT gx_prompt_text_id_set(
    GX_PROMPT *prompt, 
    GX_RESOURCE_ID string_id);
```

### Description

This service sets the string ID for the text prompt widget.

### Parameters

- *prompt*: Pointer to prompt widget control block.
- *string_id*: Resource ID of the string.

### Return Values

- **GX_SUCCESS** (0x00) Successful prompt text ID set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_SYSTEM_MEMORY_ERROR (0x30) Memory free function is not defined.

### Allowed From

Initialization and threads

### Example

```C
/* Set the string ID of "my_prompt". */
status = gx_prompt_text_id_set(&my_prompt, MY_STRING_ID);

/* If status is GX_SUCCESS the text ID for prompt "my_prompt" has been set. */
```

### See Also

- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_prompt_create
- gx_prompt_draw
- gx_prompt_event_process
- gx_prompt_font_set
- gx_prompt_text_get
- gx_prompt_text_set

## gx_prompt_text_set


Set prompt text (deprecated)

### Prototype

```C
UINT gx_prompt_text_set(
    GX_PROMPT *prompt, 
    GX_CHAR *text);
```

### Description

This service has been deprecated in favor of gx_prompt_text_set_ext().

This service sets the text of a prompt widget. If the prompt widget was created with style GX_STYLE_TEXT_COPY, the widget creates a private copy of the text string assigned. If GX_STYLE_TEXT_COPY is not active, the widget does not make a private copy of the incoming string, and therefore the string must be statically or globally allocated, i.e. it may not be an automatic or temporary variable.

GX_PROMPT is derived from GX_WIDGET, and therefore all gx_widget API services may be used with GX_PROMPT.

### Parameters

- *prompt*: Pointer to prompt widget control block.
- *text*: Pointer to text.

### Return Values

- **GX_SUCCESS** (0x00) Successful prompt text set.
- **GX_SYSTEM_MEMORY_ERROR** (0x30) Memory allocate function is not defined.
- **GX_INVALID_STRING_LENGTH** (0x34) Invalid string length.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Set the text of "my_prompt" to "my_text". */
status = gx_prompt_text_set(&my_prompt, "my_text");

/* If status is GX_SUCCESS the text for "my_prompt" has been set. */
```

### See Also

- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_prompt_create
- gx_prompt_draw
- gx_prompt_event_process
- gx_prompt_font_set
- gx_prompt_text_color_set
- gx_prompt_text_id_set
- gx_prompt_text_get

## gx_prompt_text_set_ext

Set prompt text

### Prototype

```C
UINT gx_prompt_text_set_ext(
    GX_PROMPT *prompt,
    GX_STRING *string);
```

### Description

This service sets the text of a prompt widget. If the prompt widget was created with style GX_STYLE_TEXT_COPY, the widget creates a private copy of the text string assigned. If GX_STYLE_TEXT_COPY is not active, the widget does not make a private copy of the incoming string, and therefore the string must be statically or globally allocated, i.e. it may not be an automatic or temporary variable.

GX_PROMPT is derived from GX_WIDGET, and therefore all gx_widget API services may be used with GX_PROMPT.

### Parameters

- *prompt*: Pointer to prompt widget control block.
- *text*: Pointer to text.

### Return Values

- **GX_SUCCESS** (0x00) Successful prompt text set.
- **GX_SYSTEM_MEMORY_ERROR** (0x30) Memory allocate function is not defined.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_STRING_LENGTH (0x34) Invalid string length.

### Allowed From

Initialization and threads

### Example

```C
GX_STRING new_string;
new_string.gx_string_ptr = "my_text";
new_string.gx_string_length = strlen(new_string.gx_string_ptr);

/* Set the text of "my_prompt" to "new_string". */
status = gx_prompt_text_set(&my_prompt, &new_string);

/* If status is GX_SUCCESS the text for "my_prompt" has been set. */
```

### See Also

- gx_pixelmap_prompt_create
- gx_pixelmap_prompt_draw
- gx_pixelmap_prompt_pixelmap_set
- gx_prompt_create
- gx_prompt_draw
- gx_prompt_event_process
- gx_prompt_font_set
- gx_prompt_text_color_set
- gx_prompt_text_id_set
- gx_prompt_text_get

## gx_radial_progress_bar_anchor_set


Set starting angle

### Prototype

```C
UINT gx_radial_progress_bar_anchor_set(
    GX_RADIAL_PROGRESS_BAR *progress_bar, 
    GX_VALUE angle);
```

### Description

This service sets the starting angle for radial progress bar.

### Parameters

- *progress*: bar Pointer to radial progress bar control block.
- *angle*: Starting angle of the circular arc.

### Return Values

- **GX_SUCCESS** (0x00) Successful radial progress bar anchor set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Initialization and threads

### Example

```C
GX_VALUE start_angle = 90;

/* Set the start angle of "my_progress_bar" to 90 degree. */
status = gx_radial_progress_bar_anchor_set(&my_progress_bar, start_angle);

/* If status is GX_SUCCESS the anchor value of "my_progress_bar" has been set. */
```

### See Also

- gx_radial_progress_bar_background_draw
- gx_radial_progress_bar_create
- gx_radial_progress_bar_draw
- gx_radial_progress_bar_event_process
- gx_radial_progress_bar_font_set
- gx_radial_progress_bar_info_set
- gx_radial_progress_bar_text_color_set
- gx_radial_progress_bar_text_draw
- gx_radial_progress_bar_value_set

## gx_radial_progress_bar_background_draw


Draw background

### Prototype

```C
VOID gx_radial_progress_bar_background_draw(GX_RADIAL_PROGRESS_BAR *progress_bar);
```

### Description

This service draws a radial progress bar background. This service is internally referenced by the gx_radial_progress_bar_draw function, but is exposed for use by the application in those cases where the application defines a custom radial progress bar drawing function

### Parameters

- *progress bar*: Pointer to radial progress bar control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom radial progress bar drawing function. */

VOID my_radial_progress_bar_draw(GX_RADIAL_PROGRESS_BAR *radial_progress)
{
    /* Call default radial progress bar background draw. */
    gx_radial_progress_bar_background_draw(radial_progress);

    /* Add your own drawing here. */

    /* Draw child widgets. */
    gx_widget_children_draw((GX_WIDGET *)radial_progress);
}
```
### See Also

- gx_radial_progress_bar_anchor_set
- gx_radial_progress_bar_create
- gx_radial_progress_bar_draw
- gx_radial_progress_bar_event_process
- gx_radial_progress_bar_font_set
- gx_radial_progress_bar_info_set
- gx_radial_progress_bar_text_color_set
- gx_radial_progress_bar_text_draw
- gx_radial_progress_bar_value_set

## gx_radial_progress_bar_create


Create radial progress bar

### Prototype

```C
UINT gx_radial_progress_bar_create(
    GX_RADIAL_PROGRESS_BAR *progress_bar,
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent,
    GX_RADIAL_PROGRESS_BAR_INFO *info,
    ULONG style
    USHORT id);
```

### Description

This service creates a radial progress bar.

If the widget style GX_STYLE_ENABLED is applied to the progress bar, the progress bar will accept pen_down, pen_drag, and pen_up input to modify the progress bar current value.

The widget style GX_STYLE_PROGRESS_TEXT_DRAW can be used to enable drawing the progress bar value as text within the progress bar area. If this style is used in combination with the style GX_STYLE_PROGRESS_PERCENT, the progress bar value is displayed as a percentage. Otherwise the progress bar value is displayed as the current angular value.

### Parameters

- *progress*: bar Pointer to radial progress bar control block.
- *name*: Name of radial progress bar.
- *parent*: Pointer to parent widget.
- *info*: Pointer to a GX_RADIAL_PROGRESS_BAR structure. **Appendix I** contains definition for GX_RADIAL_PROGRESS_BAR structure.
- *style*: Style of radial progress bar.
- *id*: Application-defined ID of progress bar.

### Return Values

- **GX_SUCCESS** (0x00) Successful radial progress bar create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_WIDGET (0x12) Invalid parent widget.

### Allowed From

Initialization and threads

### Example

```C
GX_RAIDAL_PROGRESS_BAR_INFO info;

info.gx_radial_progress_bar_info_xcenter = 200;
info.gx_radial_progress_bar_info_ycenter = 200;
info.gx_radial_progress_bar_info_radius = 100;
info.gx_radial_progress_bar_info_current_angle = 180;
info.gx_radial_progress_bar_info_anchor_val = -180;
info.gx_radial_progress_bar_info_font_id = GX_FONT_ID_SYSTEM;
info.gx_raidal_progress_bar_info_normal_text_color =
                                        GX_COLOR_ID_TEXT;
info.gx_radial_progress_bar_info_selected_text_color =
                                        GX_COLOR_ID_TEXT;
info.gx_radial_progress_bar_info_disabled_text_color =
                                        GX_COLOR_ID_DISABLED_TEXT;
info.gx_radial_progress_bar_info_normal_brush_width = 20;
info.gx_raidal_progress_bar_info_selected_brush_width = 16;
info.gx_radial_progress_bar_info_normal_brush_color =
                                        GX_COLOR_ID_WIDGET_FILL;
info.gx_radial_progress_bar_info_selected_brush_color =
                                        GX_COLOR_ID_SELECTED_FILL;

/* Create a radial progress bar "my_progress_bar". */
status = gx_radial_progress_bar_create(&my_progress_bar,
                    "my_progress_bar", parent, &info,
                    GX_STYLE_ENABLED | GX_STYLE_TRANSPARENT |
                    GX_STYLE_PROGRESS_TEXT_DRAW,
                    ID_MY_RADIAL_PROGRESS);

/* If status is GX_SUCCESS the radial progress bar "my_progress_bar" has been created. */
```

### See Also

- gx_radial_progress_bar_anchor_set
- gx_radial_progress_bar_background_draw
- gx_radial_progress_bar_draw
- gx_radial_progress_bar_event_process
- gx_radial_progress_bar_font_set
- gx_radial_progress_bar_info_set
- gx_radial_progress_bar_text_color_set
- gx_radial_progress_bar_text_draw
- gx_radial_progress_bar_value_set

## gx_radial_progress_bar_draw


Draw a radial progress bar

### Prototype

```C
VOID gx_radial_progress_bar_draw(GX_RADIAL_PROGRESS_BAR *progress_bar);
```

### Description

This service draws a radial progress bar. This service is used internally referenced by the gx_radial_progress_bar_create function, but is exposed for use by the application in those cases where the application defines a custom radial progress bar drawing function.

### Parameters

- *progress bar*: Pointer to radial progress bar control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom radial progress bar drawing function. */

VOID my_radial_progress_bar_draw(GX_RADIAL_PROGRESS_BAR *radial_progress)
{
    /* Call default radial progress bar draw. */
    gx_radial_progress_bar_draw(radial_progress);

    /* Add your own drawing here. */

    /* Draw child widgets. */
    gx_widget_children_draw((GX_WIDGET *)radial_progress);
}
```

### See Also

- gx_radial_progress_bar_anchor_set
- gx_radial_progress_bar_background_draw
- gx_radial_progress_bar_create
- gx_radial_progress_bar_event_process
- gx_radial_progress_bar_font_set
- gx_radial_progress_bar_info_set
- gx_radial_progress_bar_size_calculate
- gx_radial_progress_bar_text_color_set
- gx_radial_progress_bar_text_draw
- gx_radial_progress_bar_value_set

## gx_radial_progress_bar_event_process


Process radial progress bar event

### Prototype

```C
UINT gx_radial_progress_bar_event_process(
    GX_RADIAL_PROGRESS_BAR *progress_bar,
    GX_EVENT *event_ptr);
```

### Description

This service processes a radial progress bar event. This function is internally referenced by the gx_radial_progress_bar_create function, but is exposed for use by the application in those cases where the application defines a custom radial progress event processing function.

### Parameters

- *progress*: bar Pointer to radial progress bar control block.
- *event_ptr*: Pointer to event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful radial progress bar event process.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Initialization and threads

### Example

```C
/* Write a custom event processing function. */

UINT my_event_process (GX_RADIAL_PROGRESS_BAR *radial_progress, GX_EVENT *event_ptr)
{
    switch(event_ptr->gx_event_type)
    {
        case GX_EVENT_SHOW:
        /* Do default handling. */
        status = gx_radial_progress_bar_event_process(
                            radial_progress, event_ptr);
        /* Add my own handling here. */
        break;
    default:
        status = gx_radial_progress_bar_event_process(
                            radial_progress, event_ptr);
        break;
    }
    return status;
}
```

### See Also

- gx_radial_progress_bar_anchor_set
- gx_radial_progress_bar_background_draw
- gx_radial_progress_bar_create
- gx_radial_progress_bar_draw
- gx_radial_progress_bar_font_set
- gx_radial_progress_bar_info_set
- gx_radial_progress_bar_text_color_set
- gx_radial_progress_bar_text_draw
- gx_radial_progress_bar_value_set

## gx_radial_progress_bar_font_set


Set radial progress bar font

### Prototype

```C
UINT gx_radial_progress_bar_font_set(
    GX_RADIAL_PROGRESS_BAR *progress_bar,
    GX_RESOURCE_ID font_id);
```

### Description

This service sets the font of a radial progress bar widget. This parameter has no effect if the widget style GX_STYLE_PROGRESS_TEXT_DRAW is not set.

### Parameters

- *progress*: bar Pointer to radial progress bar control block.
- *font_id*: Resource ID of font.

### Return Values

- **GX_SUCCESS** (0x00) Successful radial progress bar font set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Initialization and threads

### Example

```C
/* Set font for radial progress bar "my_progress_bar". */
status = gx_radial_progress_bar_font_set(&my_progress_bar, font);

/* If status is GX_SUCCESS the font of "my_progress_bar" has been set. */
```

### See Also

- gx_radial_progress_bar_anchor_set
- gx_radial_progress_bar_background_draw
- gx_radial_progress_bar_create
- gx_radial_progress_bar_draw
- gx_radial_progress_bar_event_process
- gx_radial_progress_bar_info_set
- gx_radial_progress_bar_text_color_set
- gx_radial_progress_bar_text_draw
- gx_radial_progress_bar_value_set

## gx_radial_progress_bar_info_set


Set radial progress bar information

### Prototype

```C
UINT gx_radial_progress_bar_info_set(
    GX_RADIAL_PROGRESS_BAR *progress_bar,
    GX_RADIAL_PROGRESS_BAR_INFO *info);
```

### Description

This service resets the information parameters assigned to the radial progress bar.

### Parameters

- *progress*: bar Pointer to radial progress bar control block.
- *info*: Pointer to radial progress bar information structure. **Appendix I** contains definition for GX_RADIAL_PROGRESS_BAR_INFO structure.

### Return Values

- **GX_SUCCESS** (0x00) Successful radial progress bar info set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Initialization and threads

### Example

```C
GX_RAIDAL_PROGRESS_BAR_INFO info;

info.gx_radial_progress_bar_info_xcenter = 200;
info.gx_radial_progress_bar_info_ycenter = 200;
info.gx_radial_progress_bar_info_radius = 100;
info.gx_radial_progress_bar_info_current_angle = 180;
info.gx_radial_progress_bar_info_anchor_val = -180;
info.gx_radial_progress_bar_info_font_id = GX_FONT_ID_SYSTEM;
info.gx_raidal_progress_bar_info_normal_text_color =
                                GX_COLOR_ID_TEXT;
info.gx_radial_progress_bar_info_selected_text_color =
                                GX_COLOR_ID_TEXT;
info.gx_radial_progress_bar_info_disabled_text_color =
                                GX_COLOR_ID_DISABLED_TEXT;
info.gx_radial_progress_bar_info_normal_brush_width = 20;
info.gx_raidal_progress_bar_info_selected_brush_width = 16;
info.gx_radial_progress_bar_info_normal_brush_color =
                                GX_COLOR_ID_WIDGET_FILL;
info.gx_radial_progress_bar_info_selected_brush_color =
                                GX_COLOR_ID_SELECTED_FILL;

/* Set appearance information for radial progress bar "my_progress_bar". */
status = gx_radial_progress_bar_info_set(&my_progress_bar, &info);

/* If status is GX_SUCCESS the appearance information of "my_progress_bar" has been set. */
```

### See Also

- gx_radial_progress_bar_anchor_set
- gx_radial_progress_bar_background_draw
- gx_radial_progress_bar_create
- gx_radial_progress_bar_draw
- gx_radial_progress_bar_event_process
- gx_radial_progress_bar_font_set
- gx_radial_progress_bar_text_color_set
- gx_radial_progress_bar_text_draw
- gx_radial_progress_bar_value_set

## gx_radial_progress_bar_text_color_set


Set radial progress bar text color

### Prototype

```C
UINT gx_radial_progress_bar_text_color_set(
    GX_RADIAL_PROGRESS_BAR *progress_bar,
    GX_RESOURCE_ID normal_text_color,
    GX_RESOURCE_ID selected_text_color,
    GX_RESOURCE_ID disabled_text_color);
```

### Description

This service sets the text color of radial progress bar. This value is only used if the style GX_STYLE_PROGRESS_TEXT_DRAW is set.

### Parameters

- *progress*: bar Pointer to radial progress bar control block.
- *normal_color*: Resource ID of text color in normal state. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.
- *selected_color*: Resource ID of text color when the widget gain focus. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.
- *disabled_color*: Resource ID of text color when the style GX_STYLE_ENABLED is not set. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.

### Return Values

- **GX_SUCCESS** (0x00) Successful radial progress bar text color set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget check.

### Allowed From

Initialization and threads

### Example

```C
/* Set text color for radial progress bar "my_progress_bar". */
status = gx_radial_progress_bar_text_color_set(&my_progress_bar,
                            GX_COLOR_ID_NORMAL_TEXT,
                            GX_COLOR_ID_SELECTED_TEXT,
                            GX_COLOR_ID_DISABLED_TEXT);

/* If status is GX_SUCCESS the text color of "my_progress_bar" has been set. */
```

### See Also

- gx_radial_progress_bar_anchor_set
- gx_radial_progress_bar_background_draw
- gx_radial_progress_bar_create
- gx_radial_progress_bar_draw
- gx_radial_progress_bar_event_process
- gx_radial_progress_bar_font_set
- gx_radial_progress_bar_info_set
- gx_radial_progress_bar_text_draw
- gx_radial_progress_bar_value_set

## gx_radial_progress_bar_text_draw


Draw radial progress bar text

### Prototype

```C
VOID gx_radial_progress_bar_text_draw(GX_RADIAL_PROGRESS_BAR *progress_bar);
```

### Description

This service draws the text of specified radial progress bar. This function is called internally as part of the gx_radial_progress_bar_draw(), but is exposed to the application to support those cases where the application defines a custom progress bar drawing function.

### Parameters

- *progress bar*: Pointer to radial progress bar control block.

### Return Values

- None

### Allowed From

Initialization and Threads

### Example

```C
/* Draw text for radial progress bar "my_progress_bar". */
status = gx_radial_progress_bar_text_draw (&my_progress_bar);

/* If status is GX_SUCCESS the text of "my_progress_bar" has been drawn. */

/* Write a custom radial progress bar drawing function. */
VOID my_radial_progress_bar_draw(GX_RADIAL_PROGRESS_BAR *radial_progress)
{
    /* Add your own background draw here. */

    /* Call default radial progress bar text draw. */
    gx_radial_progress_bar_text_draw(radial_progress);

    /* Draw child widgets. */
    gx_widget_children_draw((GX_WIDGET *)radial_progress);
}
```

### See Also

- gx_radial_progress_bar_anchor_set
- gx_radial_progress_bar_background_draw
- gx_radial_progress_bar_create
- gx_radial_progress_bar_draw
- gx_radial_progress_bar_event_process
- gx_radial_progress_bar_font_set
- gx_radial_progress_bar_info_set
- gx_radial_progress_bar_text_color_set
- gx_radial_progress_bar_value_set

## gx_radial_progress_bar_value_set


Set radial progress bar value

### Prototype

```C
UINT gx_radial_progress_bar_value_set(
    GX_RADIAL_PROGRESS_BAR *progress_bar, 
    GX_VALUE value);
```

### Description

This service sets radial progress bar value. The assigned value is limited to the range [-360, 360], defining the possible range of angular values for the progress bar current location. The application must scale the real-world value being indicated to assign an angular value to the progress bar widget.

The progress bar is drawn such that the current value indicates the angular delta between the anchor position and the end point of the upper arc. Negative values cause the arc to be drawn in a clockwise direction starting at the anchor position. Positive current value causes the arc to be drawn in a counter-clockwise direction starting at the anchor position.

For example, to draw an arc starting at the top of the arc (12 o'clock position) and ending at the right (three o'clock position), assign an anchor value of 90 degrees and a current value of -90 degrees.

### Parameters

- *progress*: bar Pointer to radial progress bar control block.
- *value*: New progress bar value.

### Return Values

- **GX_SUCCESS** (0x00) Successful radial progress bar value set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointers.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Initialization and threads

### Example

```C
GX_VALUE new_value = 180;

/* Set value for radial progress bar "my_progress_bar". */
status = gx_radial_progress_bar_value_set(&my_progress_bar,
                                        new_value);

/* If status is GX_SUCCESS the value of "my_progress_bar" has been set. */
```

### See Also

- gx_radial_progress_bar_anchor_set
- gx_radial_progress_bar_background_draw
- gx_radial_progress_bar_create
- gx_radial_progress_bar_draw
- gx_radial_progress_bar_event_process
- gx_radial_progress_bar_font_set
- gx_radial_progress_bar_info_set
- gx_radial_progress_bar_text_color_set
- gx_radial_progress_bar_text_draw

## gx_radio_button_create


Create radio button

### Prototype

```C
UINT gx_radio_button_create(
    GX_RADIO_BUTTON *button,
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent,
    GX_RESOURCE_ID text_id, ULONG style,
    USHORT radio_button_id,
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a radio button widget. GX_RADIO_BUTTON is derived from GX_TEXT_BUTTON, and therefore all gx_text_button services are also supported by this widget type.

### Parameters

- *button*: Pointer to radio button control block.
- *name*: Logical name of radio button widget.
- *parent*: Pointer to the parent widget.
- *text_id*: Resource ID of radio button.
- *style*: Style of radio button. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *radio_button_id*: Application-defined ID of radio button.
- *size*: Dimensions of radio button.

### Return Values

- **GX_SUCCESS** (0x00) Successful radio button create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Initialization and threads

### Example

```C
/* Create "my_radio_button". */
status = gx_radio_button_create(&my_radio_button, "my_radio_button", &my_parent,
                    MY_RADIO_BUTTON_TEXT_RESOURCE_ID,
                    GX_STYLE_BORDER_RAISED, MY_RADIO_BUTTON_ID, &size);

/* If status is GX_SUCCESS the radio button "my_radio_button" has been created. */
```
### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_icon_button_create
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_text_button_create
- gx_text_button_color_set
- gx_text_button_draw
- gx_radio_button_draw

## gx_radio_button_draw


Draw radio button

### Prototype

```C
VOID gx_radio_button_draw(GX_RADIO_BUTTON *button);
```

### Description

This service draws a radio button widget. This service is called internally by the GUIX canvas refresh, but can also be called by overridden drawing functions.

### Parameters

- *button*: Pointer to radio button widget control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom radio button drawing function. */

VOID my_radio_button_draw(GX_RADIO_BUTTON *radio_button)
{
    /* Call default radio button draw. */
    gx_radio_button_draw(radio_button);

    /* Add your own drawing here. */

    /* Draw child widgets. */
    gx_widget_children_draw((GX_WIDGET *)radio_button);
}
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_icon_button_create
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_text_button_create
- gx_text_button_color_set
- gx_text_button_draw
- gx_radio_button_create

## gx_radio_button_pixelmap_set


Set pixelmaps for radio button

### Prototype

```C
UINT gx_radio_button_pixelmap_set(
    GX_RADIO_BUTTON *button,
    GX_RESOURCE_ID off_id,
    GX_RESOURCE_ID on_id,
    GX_RESOURCE_ID off_disabled_id,
    GX_RESOURCE_ID on_disabled_id);
```

### Description

This service assigns the pixelmaps to be displayed by the specified radio button for each button state. The resource IDs can be duplicated.

### Parameters

- *off_id*: Pixelmap used for radio button off state.
- *on_id*: Pixelmap used for radio button on state.
- *off_disabled_id*: Pixelmap used for radio button disabled and off state.
- *on_disabled_id*: Pixelmap used for radio button disabled and on state.

### Return Values

- **GX_SUCCESS** (0x00) Successful radio button pixelmaps set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Initialization and threads

### Example

```C
/* Set pixelmap for "my_radio_button". */
status = gx_radio_button_pixelmap_set(&my_radio_button,
                                MY_OFF_PIXELMAP,
                                MY_ON_PIXELMAP,
                                MY_OFF_DISABLED_PIXELMAP,
                                MY_ON_DISABLED_PIXELMAP);

/* If status is GX_SUCCESS the pixelmaps for radio button "my_radio_button" has been set. */
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_icon_button_create
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_text_button_create
- gx_text_button_color_set
- gx_text_button_draw
- gx_radio_button_create

## gx_radial_slider_anchor_angles_set


Set radial slider anchor list

### Prototype

```C
UINT gx_radial_slider_anchor_angles_set(
    GX_RADIAL_SLIDER *slider,
    GX_VALUE *anchor_angles, 
    USHORT anchor_count);
```

### Description

This service sets anchor angles for radial slider. If anchor angle list is set, the radial slider angle will be one of defined anchor angles.

### Parameters

- *slider*: Radial slider control block.
- *anchor_angles*: Angle list to set.
- *anchor_count*: Count of the anchor angles.

### Return Values

- **GX_SUCCESS** (0x00) Successful anchor angles set.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.
- GX_INVALID_VALUE (0x22) Invalid anchor list.

### Allowed From

Initialization and threads

### Example

```C
GX_VALUE anchor_angles[]={0, 30, 60, 90, 120, 150, 180};
USHORT anchor_count = 7;

/* Set anchor angles for radial slider. */
status = gx_radial_slider_anchor_angles_set(&my_radial_slider,
                                anchor_angles, anchor_count);

/* If status is GX_SUCCESS the anchor angles have been set for "my_radial_slider". */

/* Set anchor angles for radial slider. */
status = gx_radial_slider_anchor_angles_set(&my_radial_slider,
                                GX_NULL, 0);

/* If status is GX_SUCCESS the anchor angles have been removed from "my_radial_slider". */
```

### See Also

- gx_radial_slider_angle_set
- gx_radial_slider_animation_set
- gx_radial_slider_animation_start
- gx_radial_slider_create
- gx_radial_slider_draw
- gx_radial_slider_event_process
- gx_radial_slider_info_get
- gx_radial_slider_info_set
- gx_radial_slider_pixelmap_set

## gx_radial_slider_angle_set

Set radial slider angle

### Prototype

```C
UINT gx_radial_slider_angle_set(
    GX_RADIAL_SLIDER *slider,
    GX_VALUE new_angle);
```

### Description

This service sets new angle value for radial slider.

### Parameters

- *slider*: Pointer to radial slider control block.
- *new_angle*: New angle value to be set.

### Return Values

- **GX_SUCCESS** (0x00) Successful radial slider angle set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Initialization and threads

### Example

```C
/* Set "my_radial_slider" angle to 0 degree(3 o'clock position). */
status = gx_radial_slider_angle_set(&my_radial_slider, 0);

/* If status is GX_SUCCESS the value of "my_radial_slider" has been set to 0 degree. */
```

### See Also

- gx_radial_slider_anchor_angles_set
- gx_radial_slider_animation_set
- gx_radial_slider_animation_start
- gx_radial_slider_create
- gx_radial_slider_draw
- gx_radial_slider_event_process
- gx_radial_slider_info_get
- gx_radial_slider_info_set
- gx_radial_slider_pixelmap_set

## gx_radial_slider_animation_set


Create radial slider animation info

### Prototype

```C
UINT gx_radial_slider_animation_set(
    GX_RADIAL_SLIDER *slider,
    USHORT steps, USHORT delay, 
    USHORT animation_style,
    VOID (*animation_update_callback)(GX_RADIAL_SLIDER *slider));
```

### Description

This service sets animation steps, delay time and animation styles for radial slider needle animation.

### Parameters

- *slider*: Pointer to radial slider control block.
- *steps*: Total steps for one animation.
- *delay*: Delay time for each animation step.
- *animation_style*: Easing function type, includes:.
  - GX_ANIMATION_BACK_EASE_IN
  - GX_ANIMATION_BACK_EASE_OUT
  - GX_ANIMATION_BACK_EASE_IN_OUT
  - GX_ANIMATION_BOUNCE_EASE_IN
  - GX_ANIMATION_BOUNCE_EASE_OUT
  - GX_ANIMATION_BOUNCE_EASE_IN_OUT
  - GX_ANIMATION_CIRC_EASE_IN
  - GX_ANIMATION_CIRC_EASE_OUT
  - GX_ANIMATION_CIRC_EASE_IN_OUT
  - GX_ANIMATION_CUBIC_EASE_IN
  - GX_ANIMATION_CUBIC_EASE_OUT
  - GX_ANIMATION_CUBIC_EASE_IN_OUT
  - GX_ANIMATION_ELASTIC_EASE_IN
  - GX_ANIMATION_ELASTIC_EASE_OUT
  - GX_ANIMATION_ELASTIC_EASE_IN_OUT
  - GX_ANIMATION_EXPO_EASE_IN
  - GX_ANIMATION_EXPO_EASE_OUT
  - GX_ANIMATION_EXPO_EASE_IN_OUT
  - GX_ANIMATION_QUAD_EASE_IN
  - GX_ANIMATION_QUAD_EASE_OUT
  - GX_ANIMATION_QUAD_EASE_IN_OUT
  - GX_ANIMATION_QUART_EASE_IN
  - GX_ANIMATION_QUART_EASE_OUT
  - GX_ANIMATION_QUART_EASE_IN_OUT
  - GX_ANIMATION_QUINT_EASE_IN
  - GX_ANIMATION_QUINT_EASE_OUT
  - GX_ANIMATION_QUINT_EASE_IN_OUT
  - GX_ANIMATION_SINE_EASE_IN
  - GX_ANIMATION_SINE_EASE_OUT
  - GX_ANIMATION_SINE_EASE_IN_OUT
- *animation_update_callback*: User-defined callback function that will be called after each animation step.

### Return Values

- **GX_SUCCESS** (0x00) Successful radial slider animation set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Initialization and threads

### Example

```C
VOID animation_callback(GX_RADIAL_SLIDER *slider)
{
    /* This function will be called after each animation step,
    add custom code here. */
}

/* Set animation info for "my_radial_slider". */
status = gx_radial_slider_animation_set(&my_radial_slider, 15, 2,
                                GX_ANIMATION_CIRC_EASE_IN_OUT,
                                animation_callback);

/* If status is GX_SUCCESS the radial slider needle will move with specified animation. */

/* Disable animation for "my_radial_slider". */
status = gx_radial_slider_animation_set(&my_radial_slider, 0, 0,
                                        0, 0);

/* If status is GX_SUCCESS the radial slider needle will move without animation. */
```

### See Also

- gx_radial_slider_anchor_angles_set
- gx_radial_slider_angle_set
- gx_radial_slider_animation_start
- gx_radial_slider_create
- gx_radial_slider_draw
- gx_radial_slider_event_process
- gx_radial_slider_info_get
- gx_radial_slider_info_set
- gx_radial_slider_pixelmap_set

## gx_radial_slider_animation_start


Set new radial slider value with animation

### Prototype

```C
UINT gx_radial_slider_animation_start(
    GX_RADIAL_SLIDER *slider,
    GX_VALUE target_angle);
```

### Description

This service starts an animation to move the slider needle from current position to the specified position.

### Parameters

- *slider*: Pointer to radial slider control block.
- *target_angle*: Target angle value.

### Return Values

- **GX_SUCCESS** (0x00) Successful radial slider animation start.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Initialization and threads

### Example

```C
/* Start an animation to move radial slider needle from
current position to 90 degree position. */
status = gx_radial_slider_animation_start(&my_radial_slider, 90);

/* If status is GX_SUCCESS the radial slider needle animation has been started. */
```

### See Also

- gx_radial_slider_anchor_angles_set
- gx_radial_slider_angle_set
- gx_radial_slider_animation_set
- gx_radial_slider_create
- gx_radial_slider_draw
- gx_radial_slider_event_process
- gx_radial_slider_info_get
- gx_radial_slider_info_set
- gx_radial_slider_pixelmap_set

## gx_radial_slider_create


Create radial slider

### Prototype

```C
UINT gx_radial_slider_create(
    GX_RADIAL_SLIDER *slider,
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent,
    GX_RADIAL_SLIDER_INFO *info,
    ULONG style,
    USHORT slider_id, 
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a radial slider widget.

### Parameters

- *slider*: Pointer to radial slider control block.
- *name*: Logical name of radial slider widget.
- *parent*: Pointer to the parent widget.
- *info*: Radial slider appearance definition, **Appendix I** contains definition to GX_RADIAL_SLIDER_INFO.
- *style*: Style of radio button. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *radio_button_id*: Application-defined ID of radial slider.
- *size*: Dimensions of radial slider.

### Return Values

- **GX_SUCCESS** (0x00) Successful radial slider create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.
- GX_INVALID_WIDGET (0x12) Invalid parent widget.

### Allowed From

Initialization and threads

### Example

```C
GX_RADIAL_SLIDER_INFO info;
GX_RECTANGLE size;

/* Distance from left side of widget to rotating center. */
info.gx_radial_slider_info_xcenter = 100;

/* Distance from top size of widget to rotating center. */
info.gx_radial_slider_info_ycenter = 100;

/* Radius of rotating circle. */
info.gx_radial_slider_info_radius = 100;

/* Widget of rotating track. */
info.gx_radial_slider_info_track_width = 40;

/* Current angle value. */
info.gx_radial_slider_info_current_angle = 0;

/* Minimum angle value. */
info.gx_radial_slider_min_anlge = -60;

/* Maximum angle value. */
info.gx_radial_slider_max_angle = 240;

/* Anchor value list. */
info.gx_radial_slider_angle_list = GX_NULL;

/* Anchor value count. */
info.gx_radial_slider_list_count = 0;

/* Resource ID of background pixelmap. */
info.gx_radial_slider_background_pixelmap = GX_PIXELMAP_ID_BKGRD;

/* Resource ID of needle pixelmap. */
info.gx_radial_slider_needle_pixelmap = GX_PIXELMAP_ID_NEEDLE;

/* Define widget size. */
gx_utility_rectangle_define(&size, 0, 0, 200, 200);

/* Create "my_radial_slider". */
status = gx_radial_slider_create(&my_radial_slider,
                                "my_radial_slider", &my_parent,
                                &info, GX_STYLE_ENABLED,
                                MY_RADIAL_SLIDER_ID, &size);

/* If status is GX_SUCCESS the radial slider "my_radial_slider" has been created. */
```

### See Also

- gx_radial_slider_anchor_angles_set
- gx_radial_slider_angle_set
- gx_radial_slider_animation_set
- gx_radial_slider_animation_start
- gx_radial_slider_draw
- gx_radial_slider_event_process
- gx_radial_slider_info_get
- gx_radial_slider_info_set
- gx_radial_slider_pixelmap_set

## gx_radial_slider_draw


Draw radial slider

### Prototype

```C
VOID gx_radial_slider_draw(GX_RADIAL_SLIDER *slider);
```

### Description

This service draws a radial slider. This service is called internally by the GUIX canvas refresh, but can also be called by overridden drawing functions.

### Parameters

- *slider*: Pointer to radial slider control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom radio button drawing function. */

VOID my_radial_slider_draw(GX_RADIAL_SLIDER *radial_slider)
{
    /* Call default radial slider draw. */
    gx_radio_slider_draw(radial_slider);

    /* Add your own drawing here. */
}
```

### See Also

- gx_radial_slider_anchor_angles_set
- gx_radial_slider_angle_set
- gx_radial_slider_animation_set
- gx_radial_slider_animation_start
- gx_radial_slider_create
- gx_radial_slider_draw
- gx_radial_slider_event_process
- gx_radial_slider_info_get
- gx_radial_slider_info_set
- gx_radial_slider_pixelmap_set

## gx_radial_slider_event_process


Process radial slider event

### Prototype

```C
UINT gx_radial_slider_event_process(
    GX_RADIAL_SLIDER *slider,
    GX_EVENT *event_ptr);
```

### Description

This service processes a radial slider event. This service should be called as the default event handler by any custom radial slider event processing functions.

### Parameters

- *slider*: Pointer to radial slider control block.
- *event_ptr*: Pointer to event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful radial slider event process.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Initialization and threads

### Example

```C
/* Write a custom event processing function. */

UINT my_event_process(GX_RADIAL_SLIDER *slider,
                    GX_EVENT *event_ptr)
{
    switch(event_ptr->gx_event_type)
    {
    case GX_EVENT_SHOW:
        /* Do default handling. */
        status = gx_radial_slider_event_process(slider, event_ptr);

        /* Add my own handling here. */
        break;
    default:
        status = gx_radial_slider_event_process(slider, event_ptr);
        break;
    }
    return status;
}
```

### See Also

- gx_radial_slider_anchor_angles_set
- gx_radial_slider_angle_set
- gx_radial_slider_animation_set
- gx_radial_slider_animation_start
- gx_radial_slider_create
- gx_radial_slider_draw
- gx_radial_slider_info_get
- gx_radial_slider_info_set
- gx_radial_slider_pixelmap_set

## gx_radial_slider_info_get


Retrieve radial slider info

### Prototype

```C
UINT gx_radial_slider_info_get(
    GX_RADIAL_SLIDER *slider,
    GX_RADIAL_SLIDER_INFO **info);
```

### Description

This service retrieves radial slider information pointer.

### Parameters

- *slider*: Pointer to radial slider control block.
- *info*: Retrieved radial slider information pointer.

### Return Values

- **GX_SUCCESS** (0x00) Successful radial slider info.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Initialization and threads

### Example

```C
GX_RADIAL_SLIDER_INFO *info;

/* Retrive radial slider information structure. */
status = gx_radial_slider_info_get(&my_radial_slider,
                                    "my_radial_slider", &info);

/* If status is GX_SUCCESS the radial slider information pointer of "my_radial_slider" has been retrieved. */
```

### See Also

- gx_radial_slider_anchor_angles_set
- gx_radial_slider_angle_set
- gx_radial_slider_animation_set
- gx_radial_slider_animation_start
- gx_radial_slider_create
- gx_radial_slider_draw
- gx_radial_slider_event_process
- gx_radial_slider_info_set
- gx_radial_slider_pixelmap_set

## gx_radial_slider_info_set


Set radial slider info

### Prototype

```C
UINT gx_radial_slider_info_set(
    GX_RADIAL_SLIDER *slider,
    GX_RADIAL_SLIDER_INFO *info);
```

### Description

This service sets radial slider information.

### Parameters

- *slider*: Pointer to radial slider control block.
- *info*: Radial slider information to set.

### Return Values

- **GX_SUCCESS** (0x00) Successful radial slider info set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Initialization and threads

### Example

```C
GX_RADIAL_SLIDER_INFO info;

/* Distance from left side of widget to rotating center. */
info.gx_radial_slider_info_xcenter = 100;

/* Distance from top size of widget to rotating center. */
info.gx_radial_slider_info_ycenter = 100;

/* Radius of rotating circle. */
info.gx_radial_slider_info_radius = 100;

/* Widget of rotating track. */
info.gx_radial_slider_info_track_width = 40;

/* Current angle value. */
info.gx_radial_slider_info_current_angle = 0;

/* Minimum angle value. */
info.gx_radial_slider_min_anlge = -60;

/* Maximum angle value. */
info.gx_radial_slider_max_angle = 240;

/* Anchor value list. */
info.gx_radial_slider_angle_list = GX_NULL;

/* Anchor value count. */
info.gx_radial_slider_list_count = 0;

/* Resource ID of background pixelmap. */
info.gx_radial_slider_background_pixelmap = GX_PIXELMAP_ID_BKGRD;

/* Resource ID of needle pixelmap. */
info.gx_radial_slider_needle_pixelmap = GX_PIXELMAP_ID_NEEDLE;

/* Reset radial slider info for "my_radial_slider". */
status = gx_radial_slider_info_set(&my_radial_slider, &info);

/* If status is GX_SUCCESS the radial slider info of "my_radial_slider" has been reset. */
```

### See Also

- gx_radial_slider_anchor_angles_set
- gx_radial_slider_angle_set
- gx_radial_slider_animation_set
- gx_radial_slider_animation_start
- gx_radial_slider_create
- gx_radial_slider_draw
- gx_radial_slider_event_process
- gx_radial_slider_info_get
- gx_radial_slider_pixelmap_set

## gx_radial_slider_pixelmap_set


Set radial slider pixelmaps

### Prototype

```C
UINT gx_radial_slider_pixelmap_set(
    GX_RADIAL_SLIDER *slider,
    GX_RESOURCE_ID background_pixemap,
    GX_REOUSRCE_ID needle_pixelmap);
```

### Description

This service sets radial slider background and needle pixelmaps.

### Parameters

- *slider*: Pointer to radial slider control block.
- *background_pixelmap*: Resource ID of background pixelmap.
- *needle_pixelmap*: Resource ID of needle pixelmap.

### Return Values

- **GX_SUCCESS** (0x00) Successful radial slider pixelmap set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Initialization and threads

### Example

```C
/* Create "my_radio_button". */
status = gx_radial_slider_pixelmap_set(&my_radial_slider, GX_PIXELMAP_ID_BG, GX_PIXELMAP_ID_NEEDLE);

/* If status is GX_SUCCESS the background and needle pixelmap of "my_radial_slider" has been reset. */
```

### See Also

- gx_radial_slider_anchor_angles_set
- gx_radial_slider_angle_set
- gx_radial_slider_animation_set
- gx_radial_slider_animation_start
- gx_radial_slider_create
- gx_radial_slider_draw
- gx_radial_slider_event_process
- gx_radial_slider_info_get
- gx_radial_slider_info_set

## gx_rich_text_view_create

Create a rich text view

### Prototype

```C
UINT gx_rich_text_view_create(
    GX_RICH_TEXT_VIEW *text_view,
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent,
    GX_RESOURCE_ID text_id,
    GX_RICH_TEXT_FONTS *fonts,
    USHORT id,
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a rich text view as specified.


**Formatting tags are used to display special types for text:**

- **\<b>\</b>** Set text font with user specified bold font ID
- **\<i>\</i>** Set text font with user specified italic font ID
- **\<u>\</u>** Enable text underline
- **\<f GX_FONT_ID>\</f>** Set text font with specified font ID
- **\<c GX_COLOR_ID>\</c>** Set text font with specified color ID
- **\<hc GX_COLOR_ID>\</hc>** Set text highlight color specified color ID
- **\<align left/right/center>\</align>** Set text alignment

**Examples of formatting tag usage:**

- \<b>This is bold text<\b>
- \<i>This is italic text<\i>
- \<u>This is text with underline<\u>
- \<f 0>This text font ID is set to 0<\f>
- \<c 1>This text color ID is set to 1<\c>
- \<hc 2>This text highlight color ID is set to 2<\hc>
- \<align left> This text is left aligned<\align>

### Parameters

- *text_view*: Pointer to rich text view control block.
- *name*: Name of the rich text view.
- *parent*: Pointer to parent widget.
- *text_id*: Resource ID of the text string.
- *fonts*: Pointer to the rich text view font information. **Appendix I** contains definition to GX_RICH_TEXT_FONTS structure.
- *style*: Style of the widget. **Appendix D** contains pre-defined general styles for all widgets as well as widget specific styles.
- *id*: Application-defined ID of the rich text view.
- *size*: Size of the rich text view.

### Return Values

- **GX_SUCCESS** (0x00) Successful rich text view creation.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.

### Allowed From

Initialization and threads

### Example

```C
GX_RICH_TEXT_VIEW rich_view;
GX_RCIH_TEXT_FONTS fonts;
GX_RECTANGLE size;

/* Defined widget size. */
gx_utility_rectangle_define(&size, 100, 100, 300, 150);

/* Define font information. */
fonts.gx_rich_text_fonts_normal_id = GX_FONT_ID_SYSTEM;
fonts.gx_rich_text_fonts_bold_id = GX_FONT_ID_SYSTEM;
fonts.gx_rich_text_fonts_italic_id = GX_FONT_ID_SYSTEM;
fonts.gx_rich_text_fonts_bold_italic_id = GX_FONT_ID_SYSTEM;

/* Create rich text view widget. */
status = gx_rich_text_view_create(&rich_view, "my_rich_view",
                                  parent, GX_STRING_ID_TEST_STRING,
                                  &fonts,
                                  GX_STYLE_ENABLED,
                                  MY_RICH_VIEW_ID, &size);

/* If status is GX_SUCCESS the rich text view was successfully created. */

```

The demo application demo_guix_widget_types, provided as part of the GUIX Studio installation, provides a complete example of using the rich text view widget.

### See Also

- gx_rich_text_view_draw
- gx_rich_text_view_fonts_set
- gx_rich_text_view_text_draw

## gx_rich_text_view_draw


Draw rich text view

### Prototype

```C
VOID gx_rich_text_view_draw(GX_RICH_TEXT_VIEW *text_view);
```

### Description

This service draws the specified rich text view widget. This service is normally called internally by GUIX as part of a canvas refresh operation, but it also exposed to the application that might want to provide a custom drawing function and invoke the default rich text view drawing as custom drawing base.

### Parameters

- *text_view*: Pointer to rich text view widget control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom rich text view drawing function. */

VOID my_rich_text_view_draw(GX_RICH_TEXT_VIEW *text_view)
{
    /* Call default rich text view draw. */
    gx_rich_text_view_draw(text_view);

    /* Add your own drawing here. */
}
```

### See Also

- gx_rich_text_view_create
- gx_rich_text_view_fonts_set
- gx_rich_text_view_text_draw

## gx_rich_text_view_fonts_set

Set rich text view fonts

### Prototype

```C
UINT gx_rich_text_view_fonts_set(GX_RICH_TEXT_VIEW *text_view, GX_RICH_TEXT_FONTS *fonts);
```

### Description

This service sets the fonts of a rich text view widget.

### Parameters

- *text_view*: Pointer to rich text view widget control block.
- *fonts*: Pointer to rich text view font information. **Appendix I** contains definitions to GX_RICH_TEXT_FONTS structure.

### Return Values

- **GX_SUCCESS** (0x00) Successful rich text view fonts set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_RICH_TEXT_FONTS fonts;

/* Replace the font ids as needed. */
fonts.gx_rich_text_fonts_normal_id = GX_FONT_ID_SYSTEM;
fonts.gx_rich_text_fonts_bold_id = GX_FONT_ID_SYSTEM;
fonts.gx_rich_text_fonts_italic_id = GX_FONT_ID_SYSTEM;
fonts.gx_rich_text_fonts_bold_italic_id = GX_FONT_ID_SYSTEM;

/* Set the font of "my_rich_view". */
status = gx_rich_text_view_fonts_set(&my_rich_view, &fonts);

/* If status is GX_SUCCESS the font for rich text view "my_rich_view" has been set. */
```

### See Also

- gx_rich_text_view_create
- gx_rich_text_view_draw
- gx_rich_text_view_text_draw

## gx_rich_text_view_text_draw


Draw rich text view text

### Prototype

```C
VOID gx_rich_text_view_text_draw(GX_RICH_TEXT_VIEW *text_view);
```

### Description

This support function draws the text portion of a rich text view. This function is called internally by gx_rich_text_view_draw(), and is provided as a separate API as a convenience for applications that define a custom rich text view drawing function. Applications that want to customize the rich text view background drawing can provide their custom drawing function, and invoke the gx_rich_text_view_text_draw service as part of their custom drawing to draw the rich text view text over the background.

### Parameters

- *text_view*: Pointer to rich text view widget control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom rich text view drawing function. */

VOID my_rich_text_view_draw(GX_RICH_TEXT_VIEW *text_view)
{
    /* Insert code here to draw rich text view background. */

    /* Call support function to do text drawing. */
    gx_rich_text_view_text_draw();

    /* Draw child widgets. */
    gx_widget_children_draw((GX_WIDGET *) rich_text_view);
}
```

### See Also

- gx_rich_text_view_create
- gx_rich_text_view_draw
- gx_rich_text_view_fonts_set

## gx_screen_stack_create


Initialize a screen stack

### Prototype

```C
UINT gx_screen_stack_create(
    GX_SCREEN_STACK_CONTROL *control,
    GX_WIDGET **memory_buffer, 
    INT buffer_size);
```

### Description

This service initializes a screen stack. The application must define the memory block and buffer size used to implement the screen stack feature.

> **Note:** *This API is obsoleted, and is replaced with gx_system_screen_stack_create(). This version is provided only for backwards compatibility with previous library releases..*

### Parameters

- *control*: Screen stack control block.
- *memory_buffer*: Pointer to a memory buffer that used as a screen stack.
- *buffer_size*: Memory size in bytes.

### Return Values

- **GX_SUCCESS** (0x00) Successful screen stack create.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_VALUE (0x22) Invalid buffer size.

### Allowed From

Initialization and threads

### Example

```C
#define SCREEN_STACK_SIZE 10

GX_SCREEN_STACK_CONTROL my_stack_control;
GX_WIDGET *screen_stack[SCREEN_STACK_SIZE];

/* Initialize "my_stack_control". */
status = gx_screen_stack_create(&my_stack_control, screen_stack,
                        SCREEN_STACK_SIZE * sizeof(GX_WIDGET *));

/* If status is GX_SUCCESS the screen control block "my_stack control" has been initialized. */
```

### See Also

- gx_screen_stack_push
- gx_screen_stack_pop
- gx_screen_stack_reset

## gx_screen_stack_pop


Remove the topmost entry from the screen stack

### Prototype

```C
UINT gx_screen_stack_pop(GX_SCREEN_STACK_CONTROL *control);
```

### Description

This service removes the topmost entry from the screen stack, and attaches the popped screen to its previous parent. This API also detaches any existing children from the
parent.

> **Note:** *This API is obsoleted, and is replaced with gx_system_screen_stack_pop(). This version is provided only for backwards compatibility with previous library releases..*

### Parameters

- *control*: Screen stack control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful screen stack pop.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Threads

### Example

```C
/* Remove the topmost entry from the screen stack. */
status = gx_screen_stack_pop(&my_stack_control);

/* If status is GX_SUCCESS the topmost entry has been removed from the screen stack, 
    and the popped screen has been attached to its parent. */
```

### See Also

- gx_screen_stack_create
- gx_screen_stack_push
- gx_screen_stack_reset

## gx_screen_stack_push


Push screen and its parents to stack

### Prototype

```C
UINT gx_screen_stack_push(
    GX_SCREEN_STACK_CONTROL *control,
    GX_WIDGET *screen,
    GX_WIDGET *new_screen);
```

### Description

This service detaches screen from its parent, and pushes the screen pointer and the parent pointer onto the screen stack. The new screen pointer is then attached to the parent.


> **Note:** *This API is obsoleted, and is replaced with gx_system_screen_stack_pop(). This version is provided only for backwards compatibility with previous library releases..*

### Parameters

- *control*: Screen stack control block.
- *screen*: Screen pointer to push.
- *new_screen*: Pointer of the new screen.

### Return Values

- **GX_SUCCESS** (0x00) Successful screen stack push.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Threads

### Example

```C
/* Push "screen" and its parent to screen stack, Dettach "screen" from its parent, and attach "new screen" to the parent. */
status = gx_screen_stack_push(&my_stack_control,
                                screen, new_screen);

/* If status is GX_SUCCESS the widget "screen" and its parent have been pushed to screen stack, "screen" has been detached from its parent, "new_screen" has been attached to the parent. */
```

### See Also

- gx_screen_stack_create
- gx_screen_stack_push
- gx_screen_stack_reset

## gx_screen_stack_reset


Removes all entries from the screen stack

### Prototype

```C
UINT gx_screen_stack_reset(GX_SCREEN_STACK_CONTROL *control);
```

### Description

This service removes all entries from the screen stack.

> **Note:** *This API is obsoleted, and is replaced with gx_system_screen_stack_pop(). This version is provided only for backwards compatibility with previous library releases..*

### Parameters

- *control*: Screen stack control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful scroll thumb create.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Threads

### Example

```C
/* Remove all enteries from the screen stack. */
status = gx_screen_stack_reset(&my_stack_control);

/* If status is GX_SUCCESS all entries of screen stack has been removed. */
```

### See Also

- gx_screen_stack_create
- gx_screen_stack_push
- gx_screen_stack_pop

## gx_scroll_thumb_create


Create scroll thumb

### Prototype

```C
UINT gx_scroll_thumb_create(
    GX_SCROLL_THUMB *scroll_thumb,
    GX_SCROLLBAR *parent, ULONG style);
```

### Description

This service creates a scroll thumbwheel. This service is normally called internally when a GX_SCROLLBAR is created, but is made public in order to allow custom scrollbar implementations.

### Parameters

- *scroll_thumb*: Scroll thumb widget control block.
- *parent*: Pointer to parent scrollbar.
- *style*: Style of scrollbar widget. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.

### Return Values

- **GX_SUCCESS** (0x00) Successful scroll thumb create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.
- GX_INVALID_WIDGET (0x12) Parent widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_SCROLL_THUMB my_scroll_thumb;

/* Create scroll thumb "my_scroll_thumb". */
status = gx_scroll_thumb_create(&my_scroll_thumb, &my_scrollbar,
                                GX_STYLE_NONE);

/* If status is GX_SUCCESS the scroll thumb "my_scroll_thumb" has been created. */
```

### See Also

- gx_scroll_thumb_draw
- gx_scroll_thumb_event_process

## gx_scroll_thumb_draw


Draw scroll thumb

### Prototype

```C
VOID gx_scroll_thumb_draw(GX_SCROLL_THUMB *scroll_thumb);
```

### Description

This service draws a scroll thumbwheel. This service is called internally by the GUIX canvas refresh, but can also be called by overridden drawing functions.

### Parameters

- *scroll_thumb*: Scroll thumb widget control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom scroll thumb drawing function. */

VOID my_scroll_thumb_draw(GX_SCROLL_THUMB *thumb)
{
    /* Call default scroll thumb draw. */
    gx_scroll_thumb_draw(thumb);

    /* Add your own drawing here. */
}
```

### See Also

- gx_scroll_thumb_create
- gx_scroll_thumb_event_process

## gx_scroll_thumb_event_process


Process scroll thumb event

### Prototype

```C
UINT gx_scroll_thumb_event_process(
    GX_SCROLL_THUMB *scroll_thumb,
    GX_EVENT *event);
```

### Description

This service handles events sent to a scrollbar thumbwheel. This service is normally used internally by GUIX, but is made public to assist with implementing custom scrollbar behaviors.

### Parameters

- *scroll_thumb*: Scroll thumb widget control block.
- *event*: Pointer to event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful scroll thumb event process.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Write a custom event processing function. */

UINT my_event_process (GX_SCROLL_THUMB *thumb, GX_EVENT *event_ptr)
{
    switch(event_ptr->gx_event_type)
    {
    case GX_EVENT_SHOW:
        /* Do default handling. */
        status = gx_scroll_thumb_event_process(thumb, event_ptr);

        /* Add my own handling here. */
        break;
    default:
        status = gx_scroll_thumb_event_process(thumb, event_ptr);
        break;
    }
    return status;
}
```

### See Also

- gx_scroll_thumb_create
- gx_scroll_thumb_draw

## gx_scroll_wheel_create


Create a base scroll wheel widget

### Prototype

```C
UINT gx_scroll_wheel_create( 
    GX_SCROLL_WHEEL *wheel, 
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent,
    INT total_rows, 
    ULONG style,
    USHORT id, 
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a base scroll wheel widget.

A base scroll wheel is the base widget for all scroll wheel widget types, including the **gx_generic_scroll_wheel** and **gx_text_scroll_wheel** which is the base for **gx_numeric_scroll_wheel** and **gx_string_ scroll_wheel** widgets. The base scroll wheel widget provides event handling, scrolling animation, and selected row calculation for all scroll wheel widget types.

Applications would not normally create an instance of a generic scroll wheel widget, since this widget type provides no drawing function. However access to this API is provided to assist applications which need to create a custom scroll wheel widget type.

GX_SCROLL_WHEEL is based on GX_WINDOW, and therefore all GX_WINDOW APIs may be used with GX_SCROLL_WHEEL and widgets derived from GX_SCROLL_WHEEL.

### Parameters

- *wheel*: Pointer to generic scroll wheel control block.
- *name*: Application assigned widget name.
- *parent*: Parent widget, or GX_NULL.
- *total_rows*: Total available rows.
- *style*: Widget style flags.
- *id*: Application assigned widget ID.
- *size*: Rectangle defining initial widget size.

### Return Values

- **GX_SUCCESS** (0x00) Successfully created scroll wheel.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_SIZE (0x19) Invalid control block size.
- GX_ALREADY_CREATED (0x13) Widget created.
- GX_INVALID_WIDGET (0x12) Parent widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Call generic create function during custom widget createl. */

UINT custom_scroll_wheel_create(CUSTOM_SCROLL_WHEEL *wheel,
                    GX_CONST GX_CHAR *name,
                    GX_WIDGET *parent,
                    INT total_rows, ULONG style,
                    USHORT id,
                    GX_CONST GX_RECTANGLE *size)
{
    /* Create base widget as part of custom create. */
    status = gx_scroll_wheel_create(wheel, name, parent,
                            total_rows, style, id, size);

    /* If status is GX_SUCCESS the base scroll wheel has been created. */
}
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_speed_set
- gx_scroll_wheel_total_rows_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set
- gx_string_scroll_wheel_create
- gx_string_scroll_wheel_event_process
- gx_string_scroll_wheel_text_get

## gx_scroll_wheel_event_process


Event processing function for generic scroll wheel widget

### Prototype

```C
UINT gx_scroll_wheel_event_process(
    GX_SCROLL_WHEEL *wheel,
    GX_EVENT *event);
```

### Description

This service provides the basic input event handling for all scroll wheel widget types.

This function is exposed to the application software to assist with applications which need to create a custom scroll wheel widget type. Applications would often provide their own event processing function, but invoke the generic event processing for wheel widgets for events that they do not need to customize.

### Parameters

- *wheel*: Pointer to generic scroll wheel control block.
- *event*: GX_EVENT pointer.

### Return Values

- **GX_SUCCESS** (0x00) Successfully scroll wheel event process.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Call generic scroll wheel event processing 
    as part of custom event processing function. */

UINT custom_scroll_wheel_event_process(CUSTOM_SCROLL_WHEEL *wheel,
                    GX_EVENT *event)
{
    switch(event->gx_event_type)
    {
    case xyz:
        /* Insert custom event handling here. */
        break;

    default:
        /* Pass all other events to the generic scroll wheel event processing. */
        gx_scroll_wheel_event_process(wheel, event);
        break;
    }
}
```


### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_create
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_speed_set
- gx_scroll_wheel_total_rows_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set
- gx_string_scroll_wheel_create
- gx_string_scroll_wheel_event_process
- gx_string_scroll_wheel_text_get

## gx_scroll_wheel_gradient_alpha_set


Assign gradient alpha values for optional overlay gradient

### Prototype

```C
UINT gx_scroll_wheel_gradient_alpha_set(
    GX_SCROLL_WHEEL *wheel,
    GX_UBYTE start_alpha,
    GX_UBYTE end_alpha);
```

### Description

This service defines the starting and ending alpha values for an optional gradient overlay of the scroll wheel widget.

All scroll wheel widgets support a "fade" effect of the scroll wheel rows as the rows near the top and bottom edge of the scroll wheel widget. This fade effect is accomplished by drawing a gradient pixelmap over the scroll wheel rows, which make the rows appear to fade out as the rows are drawn near the top and bottom of the scroll wheel widget.

This API service allows the application to modify the fading effect intensity, or disable this effect entirely by setting the start and end alpha values to 0.

The gradient pixelmap is created at runtime when the scroll wheel initially becomes visible. This requires that a runtime memory allocation service has been defined using _gx_system_memory_allocator_set(). If no memory allocator function has been defined, the gradient image will not be created and no fade effect will be available.

### Parameters

- *wheel*: Pointer to generic scroll wheel control block.
- *start_alpha*: The overlay gradient starting alpha value.
- *end_alpha*: The overlay gradient ending alpha value.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set the scroll wheel gradient alpha.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
status = gx_scroll_wheel_gradient_alpha_set(&wheel, 240, 0);
/* If status == GX_SUCCESS the wheel gradient alpha values were
Successfully assigned. */
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_speed_set
- gx_scroll_wheel_total_rows_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set
- gx_string_scroll_wheel_create
- gx_string_scroll_wheel_event_process
- gx_string_scroll_wheel_text_get

## gx_scroll_wheel_row_height_set


Assign the row height for each wheel row

### Prototype

```C
UINT gx_scroll_wheel_row_height_set(
    GX_SCROLL_WHEEL *wheel,
    GX_VALUE row_height);
```

### Description

This service assigns the row height for each row of the scroll wheel. Note that if the scroll wheel has style GX_STYLE_TEXT_SCROLL_WHEEL_ROUND, the row height passes through a transform which effectively reduces the row height as the row nears the top or bottom edge of the wheel.

### Parameters

- *wheel*: Pointer to generic scroll wheel control block.
- *row_height*: Row height value, in pixels.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set scroll wheel height.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
status = gx_scroll_wheel_row_height_set(&wheel, 40);

/* If status == GX_SUCCESS the wheel row height has been set to 40
pixels. */
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_gradient_alpha_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_speed_set
- gx_scroll_wheel_total_rows_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set
- gx_string_scroll_wheel_create
- gx_string_scroll_wheel_event_process
- gx_string_scroll_wheel_text_get

## gx_scroll_wheel_selected_background_set


Assign background image for wheel selected row

### Prototype

```C
UINT gx_scroll_wheel_selected_background_set(
    GX_SCROLL_WHEEL*wheel, 
    GX_RESOURCE_ID image_id);
```

### Description

This service assigns an optional pixelmap ID that is drawn behind the selected row of the scroll wheel. This can be used to highlight the selected row so that the user can easily distinguish which row of the scroll wheel is selected.

### Parameters

- *wheel*: Pointer to generic scroll wheel control block.
- *image_id*: Pixelmap ID to use as the selected row background image.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set scroll wheel background.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
status = gx_scroll_wheel_selected_background_set(&wheel,
GX_PIXELMAP_ID_SELECTED_ROW);

/* If status == GX_SUCCESS the background image has been
assigned. */
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_speed_set
- gx_scroll_wheel_total_rows_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set
- gx_string_scroll_wheel_create
- gx_string_scroll_wheel_event_process
- gx_string_scroll_wheel_text_get

## gx_scroll_wheel_selected_get


Retrieve the currently selected wheel row

### Prototype

```C
UINT gx_scroll_wheel_selected_get(
    GX_SCROLL_WHEEL *wheel,
    INT *row);
```

### Description

This service queries the scroll wheel to retrieve the currently selected row. The caller must pass the location to return the selected row index as the second parameter to this function.

### Parameters

- *wheel*: Pointer to generic scroll wheel control block.
- *row*: Location in which selected row value will be returned.

### Return Values

- **GX_SUCCESS** (0x00) Successfully retrieved selected wheel row.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x19) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
INT row;
status = gx_scroll_wheel_selected_get(&wheel, &row);

/* If status == GX_SUCCESS the selected row has been returned in
the row variable. */
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_speed_set
- gx_scroll_wheel_total_rows_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set
- gx_string_scroll_wheel_create
- gx_string_scroll_wheel_event_process
- gx_string_scroll_wheel_text_get

## gx_scroll_wheel_selected_set


Assign selected scroll wheel row

### Prototype

```C
UINT gx_scroll_wheel_selected_set(
    GX_SCROLL_WHEEL *wheel,
    INT row);
```

### Description

This service assigns the currently selected scroll wheel row.

### Parameters

- *wheel*: Pointer to generic scroll wheel control block.
- *row*: Row of the scroll wheel to be selected.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set the selected wheel row.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
status = gx_scroll_wheel_selected_set(&wheel, 20);

/* If status == GX_SUCCESS the scroll wheel has been set to select row 20. */
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_speed_set
- gx_scroll_wheel_total_rows_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set
- gx_string_scroll_wheel_create
- gx_string_scroll_wheel_event_process
- gx_string_scroll_wheel_text_get

## gx_scroll_wheel_speed_set


Assign scrolling speed

### Prototype

```C
UINT gx_scroll_wheel_speed_set(
    GX_SCROLL_WHEEL *wheel,
    GX_FIXED_VAL start_speed_rate,
    GX_FIXED_VAL end_speed_rate,
    GX_VALUE max_steps,
    GX_VALUE delay);
```

### Description

This service assigns the scrolling speed for the scroll wheel widget.

### Parameters

- *wheel*: Pointer to generic scroll wheel control block.
- *start_speed_rate*: The rate of scrolling start speed to flick speed.
- *end_speed_rate*: The rate of scrolling end speed to flick speed.
- *max_steps*: Max steps for scrolling.
- *delay*: Delay time of each step.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set wheel speed.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_VALUE (0x22) Invalid value.

### Allowed From

Initialization and threads

### Example

```C
status = gx_scroll_wheel_speed_set(&wheel, GX_FIXED_VAL_MAKE(2),
                                GX_FIXED_VAL_MAKE(1) / 2, 10, 2);

/* If status == GX_SUCCESS the scroll wheel speed has been
successfully set. */
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_total_rows_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set
- gx_string_scroll_wheel_create
- gx_string_scroll_wheel_event_process
- gx_string_scroll_wheel_text_get

## gx_scroll_wheel_total_rows_set


Assign the total scroll wheel rows available

### Prototype

```C
UINT gx_scroll_wheel_total_rows_set(
    GX_SCROLL_WHEEL *wheel,
    INT total_rows);
```

### Description

This service assigns the number of rows available in the indicated scroll wheel. The scroll wheel widget usually receives the row content from the application in the form of an array of strings or user supplied string data. This API informs the scroll wheel of the total number of rows that should be presented to the user.

### Parameters

- *wheel*: Pointer to generic scroll wheel control block.
- *total_rows*: Total number of wheel rows to present to the user.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set scroll wheel total row.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_VALUE (0x22) Invalid value.

### Allowed From

Initialization and threads

### Example

```C
status = gx_scroll_wheel_total_rows_set(&wheel, 100);

/* If status == GX_SUCCESS the scroll wheel has been changed to display 100 total rows. */
```
### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_speed_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set
- gx_string_scroll_wheel_create
- gx_string_scroll_wheel_event_process
- gx_string_scroll_wheel_text_get

## gx_scrollbar_draw


Draw scrollbar

### Prototype

```C
VOID gx_scrollbar_draw(GX_SCROLLBAR *scrollbar);
```

### Description

This service draws a scrollbar. A common drawing function is used for both vertical and horizontal scrollbar widgets. This service is called internally by the GUIX canvas refresh, but can also be called by overridden drawing functions.

### Parameters

- *scrollbar*: Scrollbar widget to draw.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom scrollbar drawing function. */

VOID my_scrollbar_draw(GX_SCROLLBAR *scrollbar)
{
    /* Call default scrollbar draw. */
    gx_scrollbar_draw(thumb);

    /* Add your own drawing here. */
}
```

### See Also

- gx_horizontal_scrollbar_create
- gx_scrollbar_event_process
- gx_scrollbar_limit_check
- gx_scrollbar_reset
- gx_vertical_scrollbar_create

## gx_scrollbar_event_process


Process scrollbar event

### Prototype

```C
UINT gx_scrollbar_event_process(
    GX_SCROLLBAR *scrollbar,
    GX_EVENT *event);
```

### Description

This service processes a scrollbar event. A common event handling function used for both vertical and horizontal scrollbar widgets.

### Parameters

- *scrollbar*: Scrollbar widget control block.
- *event*: Pointer to event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful scrollbar event process.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
Call generic scrollbar event processing as part of custom event processing function. */

UINT custom_scrollbar_event_process(GX_SCROLLBAR *scrollbar,
                                    GX_EVENT *event)
{
    switch(event->gx_event_type)
    {
    case xyz:
        /* Insert custom event handling here. */
        break;
    default:
        /* Pass all other events to the generic scrollbar event processing. */
        gx_scrollbar_event_process(scrollbar, event);
        break;
    }
}
```

### See Also

- gx_horizontal_scrollbar_create
- gx_scrollbar_draw
- gx_scrollbar_limit_check
- gx_scrollbar_reset
- gx_vertical_scrollbar_create

## gx_scrollbar_limit_check


Check scrollbar limit

### Prototype

```C
UINT gx_scrollbar_limit_check(GX_SCROLLBAR *scrollbar);
```

### Description

This service checks the limit of the scrollbar and prevents the scrollbar thumbwheel from traveling beyond the predefined limits.

### Parameters

- *scrollbar*: Scrollbar widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful scrollbar limit check.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Check scrollbar limit of "my_scrollbar". */
status = gx_scrollbar_limit_check(&my_scrollbar);

/* If status is GX_SUCCESS the limit of scrollbar "my_scrollbar" has been checked. */
```

### See Also

- gx_horizontal_scrollbar_create
- gx_scrollbar_draw
- gx_scrollbar_event_process
- gx_scrollbar_reset
- gx_vertical_scrollbar_create

## gx_scrollbar_reset


Reset scrollbar

### Prototype

```C
UINT gx_scrollbar_reset(
    GX_SCROLLBAR *scrollbar,
    GX_SCROLL_INFO *info);
```

### Description

This service resets the scrollbar.

### Parameters

- *scrollbar*: Scrollbar widget control block.
- *info*: Pointer to GX_SCROLL_INFO structure that defines the scrollbar limits, current value, and step or increment. **Appendix I** contains definition to GX_SCROLL_INFO structure.

### Return Values

- **GX_SUCCESS** (0x00) Successful scrollbar reset.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_VALUE (0x22) Scroll info not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Reset scrollbar "my_scrollbar". */

GX_SCROLL_INFO my_info;

my_info.gx_scroll_value = 0;
my_info.gx_scroll_minimum = 0;
my_info.gx_scroll_maximum = 100;
my_info.gx_scroll_visible = 10;
my_info.gx_scroll_increment = 1;

status = gx_scrollbar_reset(&my_scrollbar, &my_info);

/* If status is GX_SUCCESS the scrollbar "my_scrollbar" has been reset. */
```

### See Also

- gx_horizontal_scrollbar_create
- gx_scrollbar_draw
- gx_scrollbar_event_process
- gx_scrollbar_limit_check
- gx_vertical_scrollbar_create

## gx_scrollbar_value_set


Assign scrollbar value

### Prototype

```C
UINT gx_scrollbar_value_set(
    GX_SCROLLBAR *scrollbar,
    INT value);
```

### Description

This service assigns the current scrollbar value. A GX_EVENT_VERTICAL_SCROLL or GX_EVENT_HORIZONTAL_SCROLL event will be generated to the parent window.

### Parameters

- *scrollbar*: Scrollbar widget control block.
- *value*: New scrollbar value.

### Return Values

- **GX_SUCCESS** (0x00) Successful scrollbar event process.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Assign new value for scrollbar "my_scrollbar". */

status = gx_scrollbar_value_set(&my_scrollbar, 0);

/* If status is GX_SUCCESS the current position of scrollbar "my_scrollbar" has been set. */

```

### See Also

- gx_horizontal_scrollbar_create
- gx_scrollbar_draw
- gx_scrollbar_limit_check
- gx_scrollbar_reset
- gx_vertical_scrollbar_create

## gx_single_line_text_input_backspace


Process a backspace character in text input widget

### Prototype

```C
UINT gx_single_line_text_input_backspace(GX_SINGLE_LINE_TEXT_INPUT *text_input);
```

### Description

This service deletes the character before text input cursor position. This service is called internally when a backspace key down event is received, but can also be invoked by the application.

### Parameters

- *text_input*: Single-line text input widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successful single-line text input create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x23) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Delete a character before the cursor of "my_text_input". */
status = gx_single_line_text_input_backspace(&my_text_input);

/* If status is GX_SUCCESS the character before the cursor has been deleted. */
```

### See Also

- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_input_draw_position_get
- gx_single_line_text_input_end
- gx_single_line_text_input_event_process
- gx_single_line_text_input_fill_color_set
- gx_single_line_text_input_home
- gx_single_line_text_input_left_arrow
- gx_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_color_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_buffer_clear


Deletes all characters from the text input buffer

### Prototype

```C
UINT gx_single_line_text_input_buffer_clear(
    GX_SINGLE_LINE_TEXT_INPUT *text_input);
```

### Description

This service deletes all characters from the text input buffer.

### Parameters

- *text_input*: Single-line text input widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully cleared single-line text input buffer.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Clear input buffer of "my_text_input". */
status = gx_single_line_text_input_clear(&my_text_input);

/* If status is GX_SUCCESS the text input widget has emptied its input buffer. */
```

### See Also

- gx_single_line_text_input_buffer_backspace
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_input_draw_position_get
- gx_single_line_text_input_end
- gx_single_line_text_input_event_process
- gx_single_line_text_input_fill_color_set
- gx_single_line_text_input_home
- gx_single_line_text_input_left_arrow
- gx_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_color_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_buffer_get


Retrieves buffer information of text input widget

### Prototype

```C
UINT gx_single_line_text_input_buffer_get(
    GX_SINGLE_LINE_TEXT_INPUT *text_input,
    GX_CHAR **buffer_address, 
    UINT *content_size, 
    UINT *buffer_size);
```

### Description

This service retrieves buffer information of the text input widget.

### Parameters

- *text_input*: Single-line text input widget control block.
- *buffer_address*: The address of the input buffer.
- *content_size*: The byte count of the input data.
- *buffer_size*: The size of the input buffer.

### Return Values

- **GX_SUCCESS** (0x00) Successfully retrieved buffer information.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_CHAR *buffer_address;
UINT buffer_size;
UINT content_size;

/* Retrieve buffer information of "my_text_input" widget. */
status = gx_single_line_text_input_buffer_get(&my_text_input,
                    &buffer_address, &string_size, &buffer_size);

/* If status is GX_SUCCESS the value of buffer_address, string_size and buffer_size has been retrieved. */
```

### See Also

- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_input_end
- gx_single_line_text_input_event_process
- gx_single_line_text_input_fill_color_set
- gx_single_line_text_input_home
- gx_single_line_text_input_left_arrow
- gx_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_color_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_character_delete


Delete the character at the current cursor position

### Prototype

```C
UINT gx_single_line_text_input_character_delete(
    GX_SINGLE_LINE_TEXT_INPUT *text_input);
```

### Description

This service deletes the character after the text input cursor position. This service is called internally when a delete key down event is received, but can also be invoked by the application.

### Parameters

- *text_input*: Single-line text input widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully delete a character after the cursor.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Delete the character after the cursor of "my_text_input". */
status = gx_single_line_text_input_character_delete(&my_text_input);

/* If status is GX_SUCCESS the character after the cursor has been deleted. */
```

### See Also

- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_input_draw_position_get
- gx_single_line_text_input_end
- gx_single_line_text_input_event_process
- gx_single_line_text_input_fill_color_set
- gx_single_line_text_input_home
- gx_single_line_text_input_left_arrow
- gx_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_multi_line_text_input_create
- gx_single_line_text_input_text_color_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_character_insert


Insert a character string at current cursor position

### Prototype

```C
UINT gx_single_line_text_input_character_insert(
    GX_SINGLE_LINE_TEXT_INPUT *text_input,
    GX_UBYTE *insert_str,
    UINT insert_size);
```

### Description

This service inserts a character string into the text input string buffer at the current cursor position.

### Parameters

- *text_input*: Single-line text input widget control block.
- *insert_str*: Character string to be inserted.
- *insert_size*: Byte count to be inserted.

### Return Values

- **GX_SUCCESS** (0x00) Successfully inserted the character.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Insert characters at current cursor position. */

GX_CHAR insert_text[10] = "insert";
status = gx_single_line_text_input_character_insert(&my_text_input,
                                            insert_text,
                                            GX_STRLEN(insert_text));

/* If status is GX_SUCCESS the text input widget has successfully insert the string. */
```

### See Also

- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_input_draw_position_get
- gx_single_line_text_input_end
- gx_single_line_text_input_event_process
- gx_single_line_text_input_fill_color_set
- gx_single_line_text_input_home
- gx_single_line_text_input_left_arrow
- gx_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_color_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_create


Create a text input widget

### Prototype

```C
UINT gx_single_line_text_input_create(
    GX_SINGLE_LINE_TEXT_INPUT *text_input,
    GX_CONST GX_CHAR *name, GX_WIDGET *parent,
    GX_CHAR *input_buffer, UINT buffer_size,
    UINT style, USHORT text_input_id, GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a text input widget. The caller must provide storage for the input string and indicate the maximum length of the string.

GX_SINGLE_LINE_TEXT_INPUT is derived from GX_PROMPT and therefore all gx_prompt services may be used with GX_SINGLE_LINE_TEXT_INPUT widgets.

### Parameters

- *text_input*: Single-line text input widget control block.
- *name*: Optional widget logical name.
- *parent*: Optional parent widget.
- *input_buffer*: Storage for input string.
- *buffer_size*: Size of input string storage area, in bytes.
- *style*: Text input style flags.
- *text_input_id*: Optional ID of the input widget.
- *size*: Rectangle defining initial widget size.

### Return Values

- **GX_SUCCESS** (0x00) Successful single-line text input create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_SINGLE_LINE_TEXT_INPUT my_text_input;
static GX_CHAR			 text_input_buffer[100];
GX_RECTANGLE 			 size;

/* Define widget size. */
gx_utility_rectangle_define(&size, 10, 10, 110, 40);

/* Create single-line text input widget "my_text_input". */
status = gx_single_line_text_input_create(&my_text_input,
                        "text_input", GX_NULL, text_input_buffer, 100,
                        GX_STYLE_ENABLED, 0, &size);

/* If status is GX_SUCCESS, the text input widget has been created. */
```

### See Also

- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_draw
- gx_single_line_text_draw_position_get
- gx_single_line_text_input_end
- gx_single_line_text_input_event_process
- gx_single_line_text_input_fill_color_set
- gx_single_line_text_input_home
- gx_single_line_text_input_left_arrow
- gx_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_color_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_draw


Draw a text input widget

### Prototype

```C
VOID gx_single_line_text_input_draw(GX_SINGLE_LINE_TEXT_INPUT *text_input);
```

### Description

This service draws a text input widget. This service is normally called internally during canvas refresh, but can also be called from custom text input drawing functions.

### Parameters

- *text_input*: Single-line text input widget control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom single line text input draw function. */

VOID my_sl_text_input_draw(GX_SINGLE_LINE_TEXT_INPUT *input)
{
    /* Call default single line text input draw. */
    gx_single_line_text_input_draw(input);

    /* Add your own drawing here. */
}
```

### See Also

- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_draw_position_get
- gx_single_line_text_input_end
- gx_single_line_text_input_event_process
- gx_single_line_text_input_fill_color_set
- gx_single_line_text_input_home
- gx_single_line_text_input_left_arrow
- gx_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_color_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_draw_position_get


Retrieve text draw start position

### Prototype

```C
UINT gx_single_line_text_input_draw_position_get(
    GX_SINGLE_LINE_TEXT_INPUT *text_input,
    GX_VALUE *xpos, GX_VALUE *ypos);
```

### Description

This service retrieves the draw start position of text input text.

### Parameters

- *text_input*: Single-line text input widget control block.
- *xpos*: Retrieved draw start position in x coordinate.
- *ypos*: Retrieved draw start position in y coordinate.

### Return Values

- **GX_SUCCESS** (0x00) Successfully move text input cursor to end.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Write a custom single line text input draw function. */

VOID my_sl_text_input_draw(GX_SINGLE_LINE_TEXT_INPUT *input)
{
GX_VALUE xpos;
GX_VALUE ypos;

    /* Draw background. */
    gx_widget_border_draw(input, border_color, upper_fill_color,
                          lower_fill_color, GX_TRUE);

    /* Retrieve text draw start position. */
    gx_single_line_text_input_draw_position_get(input, xpos,
                                                ypos);

    /* Add your text drawing here. */
}
```

### See Also

- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_input_event_process
- gx_single_line_text_input_fill_color_set
- gx_single_line_text_input_home
- gx_single_line_text_input_left_arrow
- gx_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_color_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_end


Move the text input cursor to the string end

### Prototype

```C
UINT gx_single_line_text_input_end(GX_SINGLE_LINE_TEXT_INPUT *text_input);
```

### Description

This service positions the text input widget cursor at the end of the input string. This service is called internally when an end key down event is received, but can also be invoked by the application.

### Parameters

- *text_input*: Single-line text input widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully move text input cursor to end.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Move input cursor to end. */
status = gx_single_line_text_input_end(&my_text_input);

/* If status is GX_SUCCESS, text text input cursor has been moved to end. */
```

### See Also

- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_draw_position_get
- gx_single_line_text_input_event_process
- gx_single_line_text_input_fill_color_set
- gx_single_line_text_input_home
- gx_single_line_text_input_left_arrow
- gx_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_color_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_event_process


Text input widget event processing function

### Prototype

```C
UINT gx_single_line_text_input_event_process(
    GX_SINGLE_LINE_TEXT_INPUT *text_input, 
    GX_EVENT *event_ptr);
```

### Description

This service processes a single line text input event. This function is internally referenced by the gx_single_line_text_input_create function, but is exposed for use by the application in those cases where the application defines a custom single line text input event processing function.

### Parameters

- *text_input*: Single-line text input widget control block.
- *event_ptr*: Pointer to GX_EVENT structure.

### Return Values

- **GX_SUCCESS** (0x00) Successfully processed text input event.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Call generic single line text input event processing as part of custom event processing function. */

UINT custom_sl_text_input_event_process(
                                GX_SINGLE_LINE_TEXT_INPUT *input,
                                GX_EVENT *event)
{
    UINT status = GX_SUCCESS;

    switch(event->gx_event_type)
    {
    case xyz:
        /* Insert custom event handling here. */
        break;
    default:
        /* Pass all other events to the default single line text input event processing. */
        status = gx_single_line_text_input_event_process(input, event);
        break;
    }
    return status;
}
```

### See Also

- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_draw_position_get
- gx_single_line_text_input_end
- gx_single_line_text_input_fill_color_set
- gx_single_line_text_input_home
- gx_single_line_text_input_left_arrow
- gx_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_color_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_fill_color_set


Set single line text input background color

### Prototype

```C
UINT gx_single_line_text_input_fill_color_set(
    GX_SINGLE_LINE_TEXT_INPUT *text_input,
    GX_RESOURCE_ID normal_fill_color_id,
    GX_RESOURCE_ID selected_fill_color_id,
    GX_RESOURCE_ID disabled_fill_color_id,
    GX_RESOURCE_ID readonly_fill_color_id);
```

### Description

This service sets the fill color of the single line text input.

### Parameters

- *text_input*: Pointer to single line text input control block.
- *normal_fill_color_id*: Resource ID of the widget fill color in normal state. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.
- *selected_fill_color_id*: Resource ID of the widget fill color when the widget gain focus. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.
- *disabled_fill_color_id*: Resource ID of the widget fill color when the style GX_STYLE_ENABLED is not set. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.
- *readonly_fill_color_id*: Resource ID of the widget fill color when both style GX_STYLE_ENABLED and GX_STYLE_TEXT_INPUT_READYONLY are set. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.

### Return Values

- **GX_SUCCESS** (0x00) Successful single line text input fill color set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set fill colors for single line text input "my_text_input". */
status = gx_single_line_text_input_fill_color_set(&my_text_input,
                    GX_COLOR_ID_NORMAL_FILL,
                    GX_COLOR_ID_SELECTED_FILL,
                    GX_COLOR_ID_DISABLED_FILL,
                    GX_COLOR_ID_READONLY_FILL);

/* If status is GX_SUCCESS, the fill color of "my_text_input" was
set. */
```

### See Also

- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_draw_position_get
- gx_single_line_text_input_end
- gx_single_line_text_input_event_process
- gx_single_line_text_input_home
- gx_single_line_text_input_left_arrow
- gx_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_color_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_home


Move the text input cursor to the home position

### Prototype

```C
UINT gx_single_line_text_input_home(GX_SINGLE_LINE_TEXT_INPUT *text_input);
```

### Description

This service moves the text input cursor position to the start of the input string. This service is called internally when a home key down event is received, but can also be invoked by the application.

### Parameters

- *text_input*: Single-line text input widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully moved cursor to the home position.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Move cursor to the start of the input text. */
status = gx_single_line_text_input_home(&my_text_input);

/* If status is GX_SUCCESS the cursor has been moved to the home position. */
```

### See Also
- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_draw_position_get
- gx_single_line_text_input_end
- gx_single_line_text_input_event_process
- gx_single_line_text_input_fill_color_set
- gx_single_line_text_input_left_arrow
- gx_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_color_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_left_arrow


Move input cursor one character to the left

### Prototype

```C
UINT gx_single_line_text_input_left_arrow(GX_SINGLE_LINE_TEXT_INPUT *text_input);
```

### Description

This service moves the text input cursor one character to the left. This service is called internally when a left key down event is received, but can also be invoked by the application.

### Parameters

- *text_input*: Single-line text input widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully moved cursor to the left.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Move the cursor one character to the left. */
status = gx_single_line_text_input_left_arrow(&my_text_input);

/* If status is GX_SUCCESS the text input cursor has been moved one character to the left. */
```

### See Also

- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_draw_position_get
- gx_single_line_text_input_end
- gx_single_line_text_input_event_process
- gx_single_line_text_input_fill_color_set
- gx_single_line_text_input_home
- gx_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_color_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_position_get

Move cursor to pixel position

### Prototype

```C
UINT gx_single_line_text_input_position_get(
    GX_SINGLE_LINE_TEXT_INPUT *text_input,
    INT pixel_position);
```

### Description

This service positions the text input cursor based on the requested pixel position. The text input cursor index will be calculated based on the x value of the pixel position. This service is called internally when a pen down event is received, but can also be invoked by the application.

### Parameters

- *text_input*: Single-line text input widget control block.
- *pixel_position*: X value of pixel position.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set the cursor to requested position.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Initialization and threads

### Example

```C
/* Set cursor to requested position. */
status = gx_single_line_text_input_position_get(&my_text_input,
                                                100);

/* If status is GX_SUCCESS the text input widget cursor has been positioned. */
```

### See Also

- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_draw_position_get
- gx_single_line_text_input_end
- gx_single_line_text_input_event_process
- gx_single_line_text_input_fill_color_set
- gx_single_line_text_input_home
- gx_single_line_text_input_left_arrow
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_color_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_right_arrow


Move input cursor one character to the right

### Prototype

```C
UINT gx_single_line_text_input_right_arrow(
    GX_SINGLE_LINE_TEXT_INPUT *text_input);
```

### Description

This service moves the text input cursor one character to the right. This service is called internally when a right key down event is received, but can also be invoked by the application.

### Parameters

- *text_input*: Single-line text input widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully moved cursor to the right.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Move cursor one character to the right. */
status = gx_single_line_text_input_right_arrow(&my_text_input);

/* If status is GX_SUCCESS the text input cursor has been moved one character to the right. */
```

### See Also

- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_draw_position_get
- gx_single_line_text_input_end
- gx_single_line_text_input_event_process
- gx_single_line_text_input_fill_color_set
- gx_single_line_text_input_home
- gx_single_line_text_input_position_get
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_color_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_style_add


Add styles

### Prototype

```C
UINT gx_single_line_text_input_style_add(
    GX_SINGLE_LINE_TEXT_INPUT *text_input, 
    ULONG style);
```

### Description

This service adds the specified style(s) to the single line text input widget.

### Parameters

- *text_input*: Single-line text input widget control block.
- *style*: New style to add. **Appendix D** contains pre-defined general styles for all widgets.

### Return Values

- **GX_SUCCESS** (0x00) Successfully added style to widget.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Add a new style to "my_text_input". */
status = gx_single_line_text_input_style_add(&my_text_input,
GX_STYLE_CURSOR_AWAYS);

/* If status is GX_SUCCESS the GX_STYLE_CUSROSR_SHOR have been successfully added. */
```

### See Also

- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_draw_position_get
- gx_single_line_text_input_end
- gx_single_line_text_input_event_process
- gx_single_line_text_input_fill_color_set
- gx_single_line_text_input_home
- gax_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_color_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_style_remove

Remove styles

### Prototype

```C
UINT gx_single_line_text_input_style_remove(
    GX_SINGLE_LINE_TEXT_INPUT *text_input, 
    ULONG style);
```

### Description

This service removes the specified style(s) from the single line text input widget.

### Parameters

- *text_input*: Single-line text input widget control block.
- *style*: Style(s) to remove. **Appendix D** contains pre-defined general styles for all widgets.

### Return Values

- **GX_SUCCESS** (0x00) Successfully removed style(s) from widget.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Remove cursor blink style from "my_text_input". */
status = gx_single_line_text_input_style_remove(&my_text_input,
GX_STYLE_CURSOR_BLINK);

/* If status is GX_SUCCESS the GX_STYLE_CURSOR_BLINK style has been successfully removed. */
```

### See Also

- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_draw_position_get
- gx_single_line_text_input_end
- gx_single_line_text_input_event_process
- gx_single_line_text_input_home
- gx_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_color_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_style_set


Set text input styles

### Prototype

```C
UINT gx_single_line_text_input_style_set(
    GX_SINGLE_LINE_TEXT_INPUT *text_input, 
    ULONG style);
```

### Description

This service sets the specified style(s) to the single line text input widget.

### Parameters

- *text_input*: Single-line text input widget control block.
- *style*: style flags to assign.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set the text input style.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set style for "my_text_input". */
status = gx_single_line_text_input_style_set(&my_text_input,
                    GX_STYLE_ENABLED | GX_STYLE_CURSOR_BLINK);

/* If status is GX_SUCCESS the text input style has been successfully set to the specified styles. */
```

### See Also

- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_draw_position_get
- gx_single_line_text_input_end
- gx_signle_line_text_input_event_process
- gx_single_line_text_input_home
- gx_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_text_color_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_text_color_set


Set single line text input text color

### Prototype

```C
UINT gx_single_line_text_input_text_color_set(
    GX_SINGLE_LINE_TEXT_INPUT *text_input,
    GX_RESOURCE_ID normal_text_color_id,
    GX_RESOURCE_ID selected_text_color_id,
    GX_RESOURCE_ID disabled_text_color_id,
    GX_RESOURCE_ID readonly_text_color_id);
```

### Description

This service sets the text color of the single line text input.

### Parameters

- *text_input*: Pointer to single line text input control block.
- *normal_text_color_id*: Resource ID of the text color in normal state. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.
- *selected_text_color_id*: Resource ID of the text color when the widget gain focus. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.
- *disabled_text_color_id*: Resource ID of the text color when the style GX_STYLE_ENABLED is not set. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.
- *readonly_text_color_id*: Resource ID of the text color when both style GX_STYLE_ENABLED and GX_STYLE_TEXT_INPUT_READONLY are set. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.

### Return Values

- **GX_SUCCESS** (0x00) Successful single line text input text color set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set text colors for single line text input "my_text_input". */
status = gx_single_line_text_input_text_color_set(&my_text_input,
                                        GX_COLOR_ID_NORMAL_TEXT,
                                        GX_COLOR_ID_SELECTED_TEXT,
                                        GX_COLOR_ID_DISABLED_TEXT,
                                        GX_COLOR_ID_READONLY_TEXT);

/* If status is GX_SUCCESS, the text color "my_text_input" was set. */
```

### See Also

- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_draw_position_get
- gx_single_line_text_input_end
- gx_single_line_text_input_event_process
- gx_single_line_text_input_fill_color_set
- gx_single_line_text_input_home
- gx_single_line_text_input_left_arrow
- gx_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_select
- gx_single_line_text_input_text_set

## gx_single_line_text_input_text_select

Select text

### Prototype

```C
UINT gx_single_line_text_input_text_color_set(
    GX_SINGLE_LINE_TEXT_INPUT *text_input,
    UINT start_index, UINT end_index);
```

### Description

This service selects text with specified start mark and end mark index and highlights the selected text with the selected fill and text colors.

### Parameters

- *text_input*: Pointer to single line text input control block.
- *start_index*: Index of the first selected character.
- *end_index*: Index of the last selected character.

### Return Values

- **GX_SUCCESS** (0x00) Successful single line text input text selection.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Select text between index [0, 9]. */
status = gx_single_line_text_input_text_select(&my_text_input,
                                                0,9);

/* If status is GX_SUCCESS, the text between index [0, 9] "my_text_input" was selected. */
```

### See Also

- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_draw_position_get
- gx_single_line_text_input_end
- gx_single_line_text_input_event_process
- gx_single_line_text_input_fill_color_set
- gx_single_line_text_input_home
- gx_single_line_text_input_left_arrow
- gx_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_set

## gx_single_line_text_input_text_set


Set single line text input text (deprecated)

### Prototype

```C
UINT gx_single_line_text_input_text_set(
    GX_SINGLE_LINE_TEXT_INPUT *text_input,
    GX_CONST GX_CHAR *text);
```

### Description

This service has been deprecated in favor of gx_single_line_text_input_text_set_ext()

This service sets the text of the single line text input.

### Parameters

- *text_input*: Pointer to single line text input control block.
- *text*: NULL-terminated text string.

### Return Values

- **GX_SUCCESS** (0x00) Successful single line text input text color set.
- **GX_INVALID_STRING_LENGTH** (0x34) Invalid string length.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_CONST GX_CHAR new_text = "Set Single Line Text Input Text";

/* Set text for single line text input "my_text_input". */
status = gx_single_line_text_input_text_set(&my_text_input,
                                        new_text);

/* If status is GX_SUCCESS, the text of "my_text_input" was set. */
```

### See Also

- gx_single_line_text_input_text_set_ext

## gx_single_line_text_input_text_set_ext


Set single line text input text

### Prototype

```C
UINT gx_single_line_text_input_text_set_ext(
    GX_SINGLE_LINE_TEXT_INPUT *text_input,
    GX_CONST GX_STRING *string);
```

### Description

This service sets the text of the single line text input.

### Parameters

- *text_input*: Pointer to single line text input control block.
- *string*: GX_STRING variable.

### Return Values

- **GX_SUCCESS** (0x00) Successful single line text input text color set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_STRING_LENGTH (0x34) Invalid string length.

### Allowed From

Initialization and threads

### Example

```C
GX_STRING new_string;
new_string.gx_string_ptr = "Set Single Line Text Input Text";
new_string.gx_string_length = strlen(new_string.gx_string_ptr);

/* Set text for single line text input "my_text_input". */
status = gx_single_line_text_input_text_set_ext(&my_text_input, &new_string);

/* If status is GX_SUCCESS, the string has been assigned to my_text_input. */
```

### See Also

- gx_single_line_text_input_backspace
- gx_single_line_text_input_buffer_clear
- gx_single_line_text_input_buffer_get
- gx_single_line_text_input_character_delete
- gx_single_line_text_input_character_insert
- gx_single_line_text_input_create
- gx_single_line_text_input_draw
- gx_single_line_text_draw_position_get
- gx_single_line_text_input_end
- gx_single_line_text_input_event_process
- gx_single_line_text_input_fill_color_set
- gx_single_line_text_input_home
- gx_single_line_text_input_left_arrow
- gx_single_line_text_input_position_get
- gx_single_line_text_input_right_arrow
- gx_single_line_text_input_style_add
- gx_single_line_text_input_style_remove
- gx_single_line_text_input_style_set
- gx_single_line_text_input_text_set_ext


## gx_slider_create

Create slider

### Prototype

```C
UINT gx_slider_create(
    GX_SLIDER *slider, 
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent, INT tick_count,
    GX_SLIDER_INFO *slider_info,
    ULONG style, USHORT slider_id,
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a slider widget.

GX_SLIDER is derived from GX_WIDGET, and therefore all gx_widget API services may be used with GX_SLIDER type widgets.

### Parameters

- *slider*: Slider widget control block.
- *name*: Name of slider.
- *parent*: Pointer to parent widget.
- *tick_count*: Number of slider ticks.
- *slider_info*: Pointer to slider info which is a structure used to pass the slider value limits, slider needle size and position, and other slider parameters. **Appendix I** contains definition to GX_SLIDER_INFO structure.
- *style*: Style of slider. **Appendix D** contains predefined general styles for all widgets as well as widget-specific styles.
- *slider_id*: Application-defined ID of slider.
- *size*: Dimensions of slider.

### Return Values

- **GX_SUCCESS** (0x00) Successful slider create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.
- GX_INVALID_WIDGET (0x12) Parent widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Create slider "my_slider". */

GX_SLIDER my_slider;
GX_SLIDER_INFO info;

info.gx_slider_info_min_val = 0;
info.gx_slider_info_max_val = 100;
info.gx_slider_info_current_val = 50;
info.gx_slider_info_increment = 1;
info.gx_slider_info_min_travel = 20;
info.gx_slider_info_max_travel = 20;
info.gx_slider_info_needle_width = 10;
info.gx_slider_info_needle_height = 10;
info.gx_slider_info_needle_inset = 5;
info.gx_slider_info_needle_hotspot_offset = 5;

status = gx_slider_create(&my_slider, "my_slider",
    &my_parent, 10, info, GX_STYLE_ENABLED, ID_MY_SLIDER, &size);

/* If status is GX_SUCCESS the slider "my_slider" has been created. */
```

### See Also

- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_pixelmap_slider_pixelmap_set
- gx_slider_draw
- gx_slider_event_process
- gx_slider_needle_draw
- gx_slider_needle_position_get
- gx_slider_needle_position_get
- gx_slider_tickmarks_draw
- gx_slider_travel_get
- gx_slider_value_calculate
- gx_slider_value_set

## gx_slider_draw

Draw slider

### Prototype

```C
VOID gx_slider_draw(GX_SLIDER *slider);
```

### Description

This service draws a slider. This service is used internally by the gx_slider_create function, but is also exposed for use by the application in those instances when a custom slider drawing function is defined.

### Parameters

- *slider*: Slider widget control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom slider draw function. */
VOID my_slider_draw(GX_SLIDER *slider)
{
    /* Call default slider draw. */
    gx_slider_draw(slider);

    /* Add your own drawing here. */
}
```

### See Also

- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_pixelmap_slider_pixelmap_set
- gx_slider_create
- gx_slider_event_process
- gx_slider_needle_draw
- gx_slider_needle_position_get
- gx_slider_needle_position_get
- gx_slider_tickmarks_draw
- gx_slider_travel_get
- gx_slider_value_calculate
- gx_slider_value_set

## gx_slider_event_process

Process slider event

### Prototype

```C
UINT gx_slider_event_process(
    GX_SLIDER *slider, 
    GX_EVENT *event);
```

### Description

This service processes a slider event. This function is internally referenced by the gx_slider_create function, but is exposed for use by the application in those cases where the application defines a custom slider event processing function.

### Parameters

- *slider*: Slider widget control block.
- *event*: Pointer to event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful slider event process.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Call generic slider event processing as part of custom event processing function. */

UINT custom_slider_event_process(GX_SLIDER *slider,
    GX_EVENT *event)
{
    UINT status = GX_SUCCESS;
    switch(event->gx_event_type)
    {
        case xyz:
            /* Insert custom event handling here. */
            break;
        default:
            /* Pass all other events to the default slider event processing. */
            status = gx_slider_event_process(slider, event);
            break;
    }
    return status;
}
```

### See Also

- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_pixelmap_slider_pixelmap_set
- gx_slider_create
- gx_slider_draw
- gx_slider_needle_draw
- gx_slider_needle_position_get
- gx_slider_needle_position_get
- gx_slider_tickmarks_draw
- gx_slider_travel_get
- gx_slider_value_calculate
- gx_slider_value_set

## gx_slider_info_set

Set slider information block

### Prototype

```C
UINT gx_slider_info_set(
    GX_SLIDER *slider, 
    GX_SLIDER_INFO *info);
```

### Description

This service assigns the specified slider information such as slider minimum, slider maximum, and slider current value to the indicated slider. The slider will update the needle position and redraw based on the new slider information.

### Parameters

- *slider*: Slider widget control block.
- *info*: Pointer to the slider information structure. **Appendix I** contains definition to GX_SLIDER_INFO structure.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set slider information.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_SLIDER_INFO my_slider_info;
my_slider_info.gx_slider_info_min_val = 0;
my_slider_info.gx_slider_info_max_val = 100;
my_slider_info.gx_slider_info_current_val = 50;
my_slider_info.gx_slider_info_increment = 1;
my_slider_info.gx_slider_info_min_travel = 20;
my_slider_info.gx_slider_info_max_travel = 20;
my_slider_info.gx_slider_info_needle_width = 10;
my_slider_info.gx_slider_info_needle_height = 10;
my_slider_info.gx_slider_info_needle_inset = 5;

my_slider_info.gx_slider_info_needle_hotspot_offset = 5;

/* Set slider information for slider "my_slider". */
status = gx_slider_info_set (&my_slider, &my_slider_info);

/* If status is GX_SUCCESS the "my_slider" is configured with my_slider_info. */
```

### See Also

- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_pixelmap_slider_pixelmap_set
- gx_slider_create
- gx_slider_draw
- gx_slider_needle_draw
- gx_slider_needle_position_get
- gx_slider_needle_position_get
- gx_slider_tickmarks_draw
- gx_slider_travel_get
- gx_slider_value_calculate
- gx_slider_value_set

## gx_slider_needle_draw

Draw slider needle

### Prototype

```C
VOID gx_slider_needle_draw(GX_SLIDER *slider);
```

### Description

This service draws a slider needle. This service is automatically called by the gx_slider_draw function, but may also be invoked by the application as part of a customized slider drawing function.

### Parameters

- *slider*: Slider widget control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom slider draw function. */
VOID my_slider_draw(GX_SLIDER *slider)
{
    /* Add your own background draw here. */

    /* Call default tickmarks draw. */
    gx_slider_tickmarks_draw(slider);

    /* Call default slider needle draw. */
    gx_slider_needle_draw(slider);
}
```

### See Also

- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_pixelmap_slider_pixelmap_set
- gx_slider_create
- gx_slider_draw
- gx_slider_event_process
- gx_slider_needle_position_get
- gx_slider_needle_position_get
- gx_slider_tickmarks_draw
- gx_slider_travel_get
- gx_slider_value_calculate
- gx_slider_value_set

## gx_slider_needle_position_get

Get slider needle position

### Prototype

```C
UINT gx_slider_needle_position_get(
    GX_SLIDER *slider,
    GX_SLIDER_INFO *slider_info,
    GX_RECTANGLE *return_position);
```

### Description

This service computes the slider needle position based on the current slider value.

### Parameters

- *slider*: Slider widget control block.
- *slider_info*: Pointer to slider information structure defining the slider limits, needle size and offset, and other slider parameters. **Appendix I** contains definition to GX_SLIDER_INFO structure.
- *return_position*: Pointer to destination for needle position.

### Return Values

- **GX_SUCCESS** (0x00) Successful slider needle position get.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_RECTANGLE needle_position;

/* Get the needle position for slider "my_slider". */
status = gx_slider_needle_posistion_get(&my_slider, &slider_info, &needle_position);

/* If status is GX_SUCCESS the needle position for slider "my_slider" has been retrieved. */
```

### See Also

- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_pixelmap_slider_pixelmap_set
- gx_slider_create
- gx_slider_draw
- gx_slider_event_process
- gx_slider_needle_draw
- gx_slider_needle_position_get
- gx_slider_tickmarks_draw
- gx_slider_travel_get
- gx_slider_value_calculate
- gx_slider_value_set

## gx_slider_tickmarks_draw

Draw slider tickmarks

### Prototype

```C
VOID gx_slider_tickmarks_draw(GX_SLIDER *slider);
```

### Description

This service draws the slider tickmarks. This function is called internally by the gx_slider_draw function, but is exposed for use by applications that might implement a custom slider drawing function.

### Parameters

- *slider*: Slider widget control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom slider draw function. */
VOID my_slider_draw(GX_SLIDER *slider)
{
    /* Add your own background draw here. */

    /* Call default tickmarks draw. */
    gx_slider_tickmarks_draw(slider);

    /* Call default slider needle draw. */
    gx_slider_needle_draw(slider);
}
```

### See Also

- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_pixelmap_slider_pixelmap_set
- gx_slider_create
- gx_slider_draw
- gx_slider_event_process
- gx_slider_needle_draw
- gx_slider_needle_position_get
- gx_slider_needle_position_get
- gx_slider_travel_get
- gx_slider_value_calculate
- gx_slider_value_set

## gx_slider_travel_get

Get slider travel

### Prototype

```C
UINT gx_slider_travel_get(
    GX_SLIDER *widget,
    GX_SLIDER_INFO *info,
    INT *return_min_travel, 
    INT *return_max_travel);
```

### Description

This service gets the slider travel.

### Parameters

- *slider*: Slider widget control block.
- *info*: Pointer to slider info structure. **Appendix I** contains definition to GX_SLIDER_INFO structure.
- *return_min_travel*: Pointer to destination for minimum travel value.
- *return_max_travell*: Pointer to destination for maximum travel value.

### Return Values

- **GX_SUCCESS** (0x00) Successful slider travel get.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_VALUE (0x22) Slider info not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Get travel information for for slider "my_slider". */
status = gx_slider_travel_get(&my_slider, &info,
    &my_min_travel, &my_max_travel);

/* If status is GX_SUCCESS the travel max/min values for slider "my_slider" have been retrieved. */
```

### See Also

- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_pixelmap_slider_pixelmap_set
- gx_slider_create
- gx_slider_draw
- gx_slider_event_process
- gx_slider_needle_draw
- gx_slider_needle_position_get
- gx_slider_needle_position_get
- gx_slider_tickmarks_draw
- gx_slider_value_calculate
- gx_slider_value_set

## gx_slider_value_calculate

Calculate slider value

### Prototype

```C
UINT gx_slider_value_calculate(
    GX_SLIDER *slider, 
    GX_SLIDER_INFO *info,
    INT new_position);
```

### Description

This service calculates the slider value based on the slider needle position. This function is called internally by GUIX when the user moves the slider needle, but can also be invoked by the application when implementing a custom slider widget.

### Parameters

- *slider*: Slider widget control block.
- *info*: Pointer to slider info. **Appendix I** contains definition to GX_LISDER_INFO structure.
- *new_position*: New slider position.

### Return Values

- **GX_SUCCESS** (0x00) Successful slider value calculate.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_VALUE (0x22) Slider info not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Calculate new value for slider "my_slider". */
status = gx_slider_value_calculate(&my_slider,
    &my_slider.gx_slider_info, new_slider_position);

/* If status is GX_SUCCESS the slider value of "my_slider" has been calculated. */

```

### See Also

- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_pixelmap_slider_pixelmap_set
- gx_slider_create
- gx_slider_draw
- gx_slider_event_process
- gx_slider_needle_draw
- gx_slider_needle_position_get
- gx_slider_needle_position_get
- gx_slider_tickmarks_draw
- gx_slider_travel_get
- gx_slider_value_set

## gx_slider_value_set

Set slider value

### Prototype

```C
UINT gx_slider_value_set(
    GX_SLIDER *slider,
    GX_SLIDER_INFO *info, 
    INT new_value);
```

### Description

This service sets the slider value. This API can be called by the application to move a slider needle under program control, bypassing the need for user input to drag the slider needle.

### Parameters

- *slider*: Slider widget control block.
- *info*: Pointer to slider info structure. **Appendix I** contains definition to GX_SLIDER_INFO structure.
- *new_value*: New slider value.

### Return Values

- **GX_SUCCESS** (0x00) Successful slider value set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set new value for slider "my_slider". */
status = gx_slider_value_set(&my_slider,
    &my_slider.gx_slider_info, new_value);

/* If status is GX_SUCCESS the new value has been set for slider "my_slider". */
```

### See Also

- gx_pixelmap_slider_create
- gx_pixelmap_slider_draw
- gx_pixelmap_slider_event_process
- gx_pixelmap_slider_pixelmap_set
- gx_slider_create
- gx_slider_draw
- gx_slider_event_process
- gx_slider_needle_draw
- gx_slider_needle_position_get
- gx_slider_needle_position_get
- gx_slider_tickmarks_draw
- gx_slider_travel_get
- gx_slider_value_calculate

## gx_sprite_create

Create a sprite widget

### Prototype

```C
UINT gx_sprite_create(
    GX_SPRITE *sprite, GX_CONST GX_CHAR *name,
    GX_WIDGET *parent,
    GX_SPRITE_FRAME *frame_list,
    USHORT frame_count,
    ULONG style, USHORT sprite_id, 
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a GX_SPRITE widget. A sprite is used to display a sequence of pixelmaps as in an animation, or can be used as a multi-state pixelmap display widget.

GX_SPRITE is derived from GX_WIDGET and supports all gx_widget API services.

The GX_SPRITE widget requires an array of GX_SPRITE_FRAME structures to define the sprite animation. **Appendix I** contains definition to GX_PRITE_FRAME structure.

### Parameters

- *sprite*: Sprite widget control block.
- *name*: Optional sprite name.
- *parent*: Pointer to parent widget.
- *frame_list*: An array of GX_SPRITE_FRAME structures.
- *frame_count*: specifies the number of entries in the frame list array.
- *style*: Style of sprite. **Appendix D** contains predefined general styles for all widgets as well as widget-specific styles.
- *sprite_id*: Application-defined ID of sprite.
- *size*: Dimensions of sprite.

### Return Values

- **GX_SUCCESS** (0x00) Successful sprite create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_WIDGET (0x12) Parent widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Create sprite "my_sprite". */

status = gx_sprite_create(&my_sprite, "my_sprite",
    &my_parent,
    sprite_frame_list, frame_count,
    GX_STYLE_SPRITE_AUTO|GX_STYLE_SPRITE_LOOP,
    ID_MY_SPRITE, &size);

/* If status is GX_SUCCESS the sprite "my_sprite" has been created. */
```

### See Also

- gx_sprite_start
- gx_sprite_stop
- gx_sprite_current_frame_set
- gx_sprite_frame_list_set

## gx_sprite_current_frame_set

Assign sprite frame

### Prototype

```C
UINT gx_sprite_current_frame_set(
    GX_SPRITE *sprite, 
    USHORT frame);
```

### Description

This service assigns the current sprite frame. If a GX_SPRITE widget is not auto-running, it can be used as a program controlled state light, displaying the commanded frame pixelmap.

### Parameters

- *sprite*: Sprite widget control block.
- *frame*: Sprite frame to display.

### Return Values

- **GX_SUCCESS** (0x00) Successful.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Assign frame number 3 as the current sprite frame. */
status = gx_sprite_current_frame_set(&my_sprite, 3);

/* If status is GX_SUCCESS the sprite "my_sprite" will display frame index 3. */
```

### See Also

- gx_sprite_start
- gx_sprite_stop
- gx_sprite_create
- gx_sprite_frame_list_set

## gx_sprite_frame_list_set

Assign or alter a sprite frame list

### Prototype

```C
UINT gx_sprite_frame_list_set(
    GX_SPRITE *sprite,
    GX_SPRITE_FRAME *frame_list,
    USHORT frame_count);
```

### Description

This service can be used to assign or re-assign the frame list used by a sprite widget after the sprite widget has been created. For information about the contents of a sprite frame list, refer to the gx_sprite_create API documentation.

### Parameters

- *sprite*: Sprite widget control block.
- *frame_list*: Array of GX_SPRITE_FRAME structures or GX_NULL if no frame list.
- *frame_count*: Number of frames in frame list array.

### Return Values

- **GX_SUCCESS** (0x00) Successful sprite frame list set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Assign framelist_1, which has 10 frames, to my_sprite. */
status = gx_sprite_frame_list_set(&my_sprite, framelist_1, 10);

/* If status is GX_SUCCESS the new frame list is now associated with this sprite. */
```

### See Also

- gx_sprite_current_frame_set
- gx_sprite_stop
- gx_sprite_create
- gx_sprite_create

## gx_sprite_start

Start a sprite run sequence

### Prototype

```C
UINT gx_sprite_start(
    GX_SPRITE *sprite, 
    USHORT frame);
```

### Description

This service starts a sprite auto-run sequence. The sprite widget will cycle through the sprite frames until the last frame is reached, or will run continuously if the GX_SPRITE_LOOP style is set.

### Parameters

- *sprite*: Sprite widget control block.
- *frame*: Initial sprite frame to display, usually frame 0.

### Return Values

- **GX_SUCCESS** (0x00) Successfully started sprite run.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Start the sprite "my_sprite". */
status = gx_sprite_start(&my_sprite, 0);

/* If status is GX_SUCCESS the sprite "my_sprite" will start running. */
```

### See Also

- gx_sprite_current_frame_set
- gx_sprite_stop
- gx_sprite_create
- gx_sprite_frame_list_set

## gx_sprite_stop

Stop a sprite run sequence

### Prototype

```C
UINT gx_sprite_stop(GX_SPRITE *sprite);
```

### Description

This service stops a sprite auto-run sequence.

### Parameters

- *sprite*: Sprite widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully stopped sprite run.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Stop the sprite sequence. */
status = gx_sprite_stop(&my_sprite);

/* If status is GX_SUCCESS the sprite "my_sprite" is stopped. */
```

### See Also

- gx_sprite_current_frame_set
- gx_sprite_start
- gx_sprite_create
- gx_sprite_frame_list_set

## gx_string_scroll_wheel_create

Create a string type scroll wheel (Deprecated)

### Prototype

```C
UINT gx_string_scroll_wheel_create(
    GX_STRING_SCROLL_WHEEL *wheel,
    GX_CONST GX_CHAR *name, 
    GX_WIDGET *parent, 
    INT total_rows,
    GX_CONST GX_CHAR **string_list, 
    ULONG style,
    USHORT Id, 
    GX_CONST GX_RECTANGLE *size)
```

### Description

This service is deprecated in favor of gx_string_scroll_wheel_create_ext().

This service creates a string type scroll wheel.

GX_STRING_SCROLL_WHEEL is derived from GX_TEXT_SCROLL_WHEEL, and therefore all gx_text_scroll_wheel API functions maybe be used with GX_STRING_SCROLL_WHEEL widgets.

The application can pass in a simple string array to the create function which defines the strings that will be displayed by the scroll wheel, or the application can pass GX_NULL as the string_list parameter and call the `gx_string_scroll_wheel_string_id_list_set()` API to provide an array of String IDs. If the latter method is used the string scroll wheel will automatically switch the displayed strings if the active application language is modified.

As an alternative, if the strings to be displayed are not statically defined or not know at the time the scroll wheel is created, the application can pass GX_NULL as the string list parameter, and call the API function gx_text_scroll_wheel_callback_set() to define a callback function which will provide the strings to be displayed in a real-time as-needed basis..

### Parameters

- *wheel*: String scroll wheel control block address.
- *name*: Application defined widget name.
- *parent*: Wheel parent or GX_NULL.
- *total_rows*: Total rows to be presented to user.
- *string_list*: Statically defined string array, or GX_NULL.
- *style*: Desired style flags.
- *Id*: Application defined wheel style flags.
- *size*: Initial scroll wheel size.

### Return Values

- **GX_SUCCESS** (0x00) Successfully created string scroll wheel.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_CONST GX_CHAR *days[] = {
    "Sunday",
    "Monday",
    "Tuesday",
    "Wednesday",
    "Thursday",
    "Friday",
    "Saturday"
};

GX_STRING_SCROLL_WHEEL wheel;

/* Create the string scroll wheel. */

status = gx_string_scroll_wheel_create(&wheel, "Day Wheel",
    root, 7, days,
    GX_STYLE_ENABLED|GX_STYLE_TEXT_CENTER|GX_STYLE_TRANSPARENT|
    GX_STYLE_WRAP|GX_STYLE_TEXT_SCROLL_WHEEL_ROUND,
    ID_SCROLL_WHEEL_DAY, &size);

/* If status is GX_SUCCESS the string scroll wheel has been created. */
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_total_rows_set
- gx_string_scroll_wheel_event_process
- gx_string_scroll_wheel_string_id_list_set
- gx_string_scroll_wheel_string_list_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set

## gx_string_scroll_wheel_create_ext

Create a string type scroll wheel

### Prototype

```C
UINT gx_string_scroll_wheel_create_ext(
    GX_STRING_SCROLL_WHEEL *wheel,
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent,
    INT total_rows,
    GX_CONST GX_STRING *string_list,
    ULONG style,
    USHORT Id,
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a string type scroll wheel.

GX_STRING_SCROLL_WHEEL is derived from GX_TEXT_SCROLL_WHEEL, and therefore all gx_text_scroll_wheel API functions maybe be used with GX_STRING_SCROLL_WHEEL widgets.

The application can pass in a simple string array to the create function which defines the strings that will be displayed by the scroll wheel, or the application can pass GX_NULL as the string_list parameter and call the `gx_string_scroll_wheel_string_id_list_set()` API to provide an array of String IDs. If the latter method is used the string scroll wheel will automatically switch the displayed strings if the active application language is modified.

As an alternative, if the strings to be displayed are not statically defined or not know at the time the scroll wheel is created, the application can pass GX_NULL as the string list parameter, and call the API function gx_text_scroll_wheel_callback_set() to define a callback function which will provide the strings to be displayed in a real-time as-needed basis..

### Parameters

- *wheel*: String scroll wheel control block address.
- *name*: Application defined widget name.
- *parent*: Wheel parent or GX_NULL.
- *total_rows*: Total rows to be presented to user.
- *string_list*: Statically defined string array, or GX_NULL.
- *style*: Desired style flags.
- *Id*: Application defined wheel style flags.
- *size*: Initial scroll wheel size.

### Return Values

- **GX_SUCCESS** (0x00) Successfully created string scroll wheel.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_WIDGET (0x12) Parent widget not valid.
- GX_INVALID_SIZE (0x19) Invalid control block size.
- GX_INVALID_STRING_LENGTH (0x34) Invalid string length.

### Allowed From

Initialization and threads

### Example

```C
GX_CONST GX_STRING days[] = {
    {"Sunday", sizeof("Sunday") - 1},
    {"Monday", sizeof("Monday") - 1},
    {"Tuesday", sizeof("Tuesday") - 1},
    {"Wednesday", sizeof("Wednesday") - 1},
    {"Thursday", sizeof("Thursday") - 1},
    {"Friday", sizeof("Friday") - 1},
    {"Saturday", sizeof("Saturday") - 1}
};

GX_STRING_SCROLL_WHEEL wheel;

/* Create the string scroll wheel. */

status = gx_string_scroll_wheel_create(&wheel, "Day Wheel",
                                       root, 7, days,
                                       GX_STYLE_ENABLED|GX_STYLE_TEXT_CENTER|GX_STYLE_TRANSPARENT|
                                       GX_STYLE_WRAP|GX_STYLE_TEXT_SCROLL_WHEEL_ROUND,
                                       ID_SCROLL_WHEEL_DAY, &size);

/* If status is GX_SUCCESS the string scroll wheel has been created. */
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_total_rows_set
- gx_string_scroll_wheel_event_process
- gx_string_scroll_wheel_string_id_list_set
- gx_string_scroll_wheel_string_list_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set

## gx_string_scroll_wheel_event_process


Process string scroll wheel event

### Prototype

```C
UINT gx_string_scroll_wheel_string_id_list_set(
    GX_STRING_SCROLL_WHEEL *wheel,
    GX_CONST GX_RESOURCE_ID *string_id_list, 
    INT id_count);
```

### Description

This service processes an event for the specified string scroll wheel. This service should be called as the default event handler by any custom string scroll wheel event processing functions.

### Parameters

- *wheel*: Pointer to string scroll wheel control block.
- *event_ptr*: Pointer to the event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful string scroll wheel event process.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Call generic string scroll wheel event processing as part of custom event processing function. */

UINT custom_string_scroll_wheel_event_process(GX_STRING_SCROLL_WHEEL *wheel, GX_EVENT *event)
{
    UINT status = GX_SUCCESS;

    switch(event->gx_event_type)
    {
    case xyz:
        /* Insert custom event handling here. */
        break;

    default:
        /* Pass all other events to the default string scroll wheel event processing. */
        status = gx_string_scroll_wheel_event_process(wheel, event);
        break;
    }
    return status;
}
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_total_rows_set
- gx_string_scroll_wheel_event_process
- gx_string_scroll_wheel_string_id_list_set
- gx_string_scroll_wheel_string_list_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set

## gx_string_scroll_wheel_string_id_list_set

Assign array of string IDs

### Prototype

```C
UINT gx_string_scroll_wheel_string_id_list_set(
    GX_STRING_SCROLL_WHEEL *wheel,
    GX_CONST GX_RESOURCE_ID *string_id_list, INT id_count);
```

### Description

This service assigns an array of string IDs to a string scroll wheel widget. This method of assigning strings to a string scroll wheel is recommended if the strings are statically defined and the widget must operate in multiple languages. If this API is to be used, the scroll wheel widget should first be created with a GX_NULL string list.

### Parameters

- *wheel*: String scroll wheel control block address.
- *string_id_list*: Array of String IDs.
- *id_count*: Size of the ID list.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set string ID array.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_VALUE (0x22) Invalid ID list size.

### Allowed From

Initialization and threads

### Example

```C
GX_CONST RESOURCE_ID wheel_ids[] = {
    GX_STRING_ID_SUNDAY,
    GX_STRING_ID_MONDAY,
    GX_STRING_ID_TUESDAY,
    GX_STRING_ID_WEDNESDAY,
    GX_STRING_ID_THURSDAY,
    GX_STRING_ID_FRIDAY,
    GX_STRING_ID_SATURDAY
};

GX_STRING_SCROLL_WHEEL wheel;

/* Stop the sprite sequence. */

status = gx_string_scroll_wheel_string_id_list_set(&wheel, wheel_ids, 7);

/* If status is GX_SUCCESS the ID list has been assigned. */
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_total_rows_set
- gx_string_scroll_wheel_create
- gx_string_scroll_wheel_event_process
- gx_string_scroll_wheel_string_list_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set

## gx_string_scroll_wheel_string_list_set

Assign array of strings (Deprecated)

### Prototype

```C
UINT gx_string_scroll_wheel_string_list_set(
    GX_STRING_SCROLL_WHEEL *wheel,
    GX_CONST GX_CHAR *string_list, 
    INT string_count);
```

### Description

This service is deprecated in favor of gx_string_scroll_wheel_string_list_set_ext().

This assigns an array of strings to a string scroll wheel widget. This can be used to modify the strings displayed after the widget has initially been created.

Note that string_scroll_wheel does not support GX_STYLE_TEXT_COPY, and therefore the array of strings passed into this function should be statically defined by the application.

### Parameters

- *wheel*: String scroll wheel control block address.
- *string_list*: Array of string pointers.
- *string_count*: Size of the string array.

### Return Values

- **GX_SUCCESS** (0x00) Successfully changed strings for scroll wheel.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_VALUE (0x22) Invalid string list size.

### Allowed From

Initialization and threads

### Example

```C
GX_CONST GX_CHAR *days[] = {
    "Sunday",
    "Monday",
    "Tuesday",
    "Wednesday",
    "Thursday",
    "Friday",
    "Saturday"
};

GX_STRING_SCROLL_WHEEL wheel;

/* Set the array of strings to the scroll wheel. */

status = gx_string_scroll_wheel_string_list_set(&wheel, days, 7);

/* If status is GX_SUCCESS the string array has been assigned. */
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_total_rows_set
- gx_string_scroll_wheel_create
- gx_string_scroll_wheel_event_process
- gx_string_scroll_wheel_string_list_set_ext
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set

## gx_string_scroll_wheel_string_list_set_ext

Assign array of strings

### Prototype

```C
UINT gx_string_scroll_wheel_string_list_set_ext(
    GX_STRING_SCROLL_WHEEL *wheel,
    GX_CONST GX_STRING *string_list,
    INT string_count);
```

### Description

This assigns an array of strings to a string scroll wheel widget. This can be used to modify the strings displayed after the widget has initially been created.

Note that string_scroll_wheel does not support GX_STYLE_TEXT_COPY, and therefore the array of strings passed into this function should be statically defined by the application.

### Parameters

- *wheel*: String scroll wheel control block address.
- *string_list*: Array of string pointers.
- *string_count*: Size of the string array.

### Return Values

- **GX_SUCCESS** (0x00) Successfully changed strings for scroll wheel.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_VALUE (0x22) Invalid string list size.
- GX_INVALID_STRING_LENGTH (0x34) Invalid string length.

### Allowed From

Initialization and threads

### Example

```C
GX_CONST GX_STRING days[] = {
    {"Sunday", sizeof("Sunday") - 1},
    {"Monday", sizeof("Monday") - 1},
    {"Tuesday", sizeof("Tuesday") - 1},
    {"Wednesday", sizeof("Wednesday") - 1},
    {"Thursday", sizeof("Thursday") - 1},
    {"Friday", sizeof("Friday") - 1},
    {"Saturday", sizeof("Saturday") - 1}
};

GX_STRING_SCROLL_WHEEL wheel;

/* Set the array of strings to the scroll wheel. */

status = gx_string_scroll_wheel_string_list_set_ext(&wheel, days, 7);

/* If status is GX_SUCCESS the string array has been assigned. */
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_total_rows_set
- gx_string_scroll_wheel_create
- gx_string_scroll_wheel_event_process
- gx_string_scroll_wheel_string_list_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set

## gx_studio_widget_create

Create widget defined in Studio generated specifications file

### Prototype

```C
GX_WIDGET *gx_studio_widget_create(
    GX_BYTE *control,
    GX_CONST GX_STUDIO_WIDGET *definition, 
    GX_WIDGET *parent);
```

### Description

This service creates a widget and the widget's children using a widget specification defined within the GUIX Studio generated specifications file. This function avoids the "by name" lookup of the similar function `gx_studio_named_widget_create()`.

The GX_STUDIO_WIDGET structure is defined in the application specifications header file generated by GUIX Studio.

For statically allocated widgets, the widget control block is defined in the generated specifications.c file, and given the widget name defined within GUIX Studio. For dynamically allocated widgets, the application should pass GX_NULL as the widget control block address and the function will attempt to dynamically allocate the widget control block using the `gx_system_memory_allocate()` function, which is also defined by and provided by the application.

For an application to directly reference the GUIX Studio widget definition within the generated specifications file, it is necessary to follow the naming convention utilized by the GUI Studio code generator. The GX_STUDIO_WIDGET structure generated within the specifications.c file is always named according to this convention: <widget_name>_define, where the <widget_name> field may be repeated multiple times if the widget is child of a child widget.

### Parameters

- *control*: Pointer to widget control block, or GX_NULL if dynamically allocated.
- *definition*: Studio generated widget definition structure.
- *parent*: pointer to the widget parent, if any.

### Return Values

Pointer to the created widget control block, or GX_NULL if the creation was not successful.

### Allowed From

Initialization and threads

### Example

```C
/* Create the widget "playlist_screen", which is statically allocated. */

widget = gx_studio_widget_create(&playlist_screen,
    &playlist_screen_define,
    root_window);

/* If widget != GX_NULL the widget was created. */

/* Create the widget "songs_screen", which is dynamically allocated. */

widget = gx_studio_widget_create(GX_NULL,
    &songs_screen_define, root_window);
```

### See Also

- gx_studio_named_widget_create

## gx_studio_named_widget_create

Create widget defined in Studio generated specifications file

### Prototype

```C
UINT *gx_studio_named_widget_create(
    char *name, 
    GX_WIDGET *parent,
    GX_WIDGET **new_widget);
```

### Description

This service creates a widget and the widget's children using a widget specification defined within the GUIX Studio generated specifications file.

This API function is used to create top-level screens using the screen name specified within the GUIX Studio application as the widget definition identifier.

### Parameters

- *name*: Screen name as defined within GUIX Studio application.
- *parent*: pointer to the widget parent, if any.
- *new_widget*: location to return created widget pointer.

### Return Values

- **GX_SUCCESS** (0x00) Successful.
- **GX_FAILURE** (0x11) Named widget could not be found.

### Allowed From

Initialization and threads

### Example

```C
/* Create the widget named "child_popup",
   which is a child of the top-level screen "main_screen". */

GX_WIDGET *menu_screen;

status = gx_studio_named_widget_create("main_menu",
    &root_window, &menu_screen);

/* If status == GX_SUCCESS the screen was created
   and linked to the root window. */
```

### See Also

- gx_studio_widget_create

## gx_studio_display_configure

Configure display defined in GUIX Studio project

### Prototype

```C
UINT *gx_studio_display_configure(
    USHORT display,
    UINT (*driver)(GX_DISPLAY *),
    USHORT language, 
    USHORT theme,
    GX_WINDOW_ROOT **return_root);
```

### Description

This service initializes a GX_DISPLAY so that it is ready for use. This function consolidates the functions to initialize a GX_DISPLAY control block, create a canvas to fit the display, and create a root window for the canvas. This function also installs the language and resource theme requested after the display has been initialized.

This function consolidates the programming effort most commonly required to prepare a display for use. The function invokes the gx_display_create(), gx_display_color_table_set, gx_display_font_table_set, gx_display_pixelmap_table_set, gx_system_language_table_set, gx_system_active_language_set, gx_system_scroll_appearance_set, gx_canvas_create, and gx_window_root_create functions, all or some of which would otherwise be required by the application program.

### Parameters

- *display*: Index into the display table, which corresponds to the display definitions in the Studio project file.
- *driver*: pointer to display driver initialization function. This function is invoked to initialize the indirect function pointers of the GX_DISPLAY control block, as well as perform any required hardware setup.
- *language*: initial language table index.
- *language*: initial theme index.
- *root*: pointer to variable in which to return root window address, or GX_NULL.

### Return Values

- **GX_SUCCESS** (0x00) Successful.
- **GX_FAILURE** (0x11) Display could not be initialized.

### Allowed From

Initialization and threads

### Example

```C
/* Create the widget named "child_popup", which is a child of the
   top-level screen "main_screen". */

GX_WIDGET *menu_screen;

status = gx_studio_display_configure(MAIN_DISPLAY,
    my_driver_setup, LANGUAGE_ENGLISH, DEFAULT_THEME, GX_NULL);

/* If status == GX_SUCCESS the display was initialized, a canvas was
created for the display, a root window was created for the canvas, and
the requested language and theme have been installed.
*/
```

### See Also

- gx_display_create
- gx_display_color_table_set
- gx_display_font_table_set
- gx_display_pixelmap_table_set
- gx_system_language_table_set
- gx_system_active_language_set
- gx_system_scroll_appearance_set
- gx_canvas_create
- gx_window_root_create

## gx_system_active_language_set

Set active language

### Prototype

```C
UINT gx_system_active_language_set(GX_UBYTE language);
```

### Description

This service set the current language. The language index must be less than the number of columns in the application string table. This function has been deprecated in favor of gx_display_active_language_set. All new applications should use gx_display_active_langauge_set.

### Parameters

- *language*: Language index, defined in resource header file.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set active language.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_VALUE (0x22) Invalid language index.

### Allowed From

Initialization and threads

### Example

```C
/* Set active language and mark widget canvas as dirty. */
status = gx_system_active_language_set(ID_LANGUAGE_ENGLISH);

/* If status is GX_SUCCESS the active language has been assigned. */
```

### See Also

- gx_display_language_table_set
- gx_display_active_langauge_set
- gx_display_string_get

## gx_system_animation_get

Obtain animation control block from system pool

### Prototype

```C
UINT gx_system_animation_get(GX_ANIMATION **animation);
```

### Description

This service can be used to obtain an animation control block from a pool of such control blocks maintained by the gx_system component. The animation control block pool and related API services are only provided if the constant GX_ANIMATION_POOL_SIZE is defined with a value 0. The default setting for this value is 6, meaning that the system animation control block pool contains size GX_ANIMATION control block.

An animation control block allocated using this API is automatically returned to the free pool if the animation runs to completion. If the animation is stopped using gx_animation_stop, or fails to be started due to some returned error, the animation control block should be returned to the free pool by the application using
gx_system_animation_free.

### Parameters

- *animation*: Address of pointer to receive GX_ANIMATION pointer.

### Return Values

- **GX_SUCCESS** (0x00) Successfully obtained animation control block.
- **GX_OUT_OF_ANIMATIONS** (0x31) System animation pool exhausted.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid animation pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_ANIMATION *animation;

UINT status = gx_system_animation_get(&animation);

if (status == GX_SUCCESS)
{
    gx_animation_start(animation, animation_info);
}
```

### See Also

- gx_animation_create
- gx_animation_start
- gx_animation_stop
- gx_system_animation_free

## gx_system_animation_free

Return an animation control block to the system pool

### Prototype

```C
UINT gx_system_animation_free(GX_ANIMATION *animation);
```

### Description

This service can be used to return an animation control block to the system pool. The animation control block pool and related API services are only provided if the constant GX_ANIMATION_POOL_SIZE is defined with a value 0. The default setting for this value is 6, meaning that the system animation control block pool contains size GX_ANIMATION control block.

An animation control block allocated using gx_system_animation_get() is automatically returned to the free pool if the animation runs to completion. Attempting to return an animation control block to the free pool that has already been returned to the free pool has no effect.

If the animation is stopped using gx_animation_stop, or fails to be started due to some returned error, the animation control block that has been obtained using gx_system_animation_get() should be returned to the free pool by the application using gx_system_animation_free().

An animation must be in IDLE state before it can be returned to the free pool. An animation is in the IDLE state when it has not been started, when it is stopped, or when it runs to completion.

### Parameters

- *animation*: Pointer to the GX_ANIMATION control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully released animation block.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid animation pointer.
- GX_INVALID_ANIMATION (0x32) The animation is not IDLE, or it has not been allocated from the system pool.

### Allowed From

Initialization and threads

### Example

```C
GX_ANIMATION *animation;

UINT status = gx_system_animation_get(&animation);

if (status == GX_SUCCESS)
{
    status = gx_animation_start(animation, animation_info);

    if (status != GX_SUCCESS)
    {
        /* Animation did not start, return it to the free pool. */
        gx_system_animation_free(animation);
    }
}
```

### See Also

- gx_animation_create
- gx_animation_start
- gx_animation_stop
- gx_system_animation_get

## gx_system_bidi_text_disable

Disable dynamic bi-directional text support

### Prototype

```C
UINT gx_system_bidi_text_disable(VOID);
```

### Description

This service disables dynamic bi-directional text support. This service requires GX_DYNAMIC_BIDI_TEXT_SUPPORT to be defined when building the GUIX library, and is only required if runtime reordering of BiDi string data is needed. Most applications utilize GUIX Studio to produce correctly reordered BiDi text strings.

### Parameters

None

### Return Values

- **GX_SUCCESS** (0x00) Successfully disabled BiDi text support.

### Allowed From

Initialization and threads

### Example

```C
/* GX_DYNAMIC_BIDI_TEXT_SUPPORT is defined. */

/* Diable bidi text support. */
status = gx_system_bidi_text_disable();

/* If status is GX_SUCCESS, bidi text support was disabled. */
```

### See Also

- gx_system_bidi_text_enable

## gx_system_bidi_text_enable

Enable dynamic bidi text support

### Prototype

```C
UINT gx_system_bidi_text_enable(VOID);
```

### Description

This service enables dynamic bi-directional text support. This service requires GX_DYNAMIC_BIDI_TEXT_SUPPORT to be defined when building the GUIX library, and is only required if runtime reordering of BiDi string data is needed. Most applications utilize GUIX Studio to produce correctly reordered BiDi text strings.

### Parameters

None

### Return Values

- **GX_SUCCESS** (0x00) Successfully enabled bidi text support.

### Allowed From

Initialization and threads

### Example

```C
/* GX_DYNAMIC_BIDI_TEXT_SUPPORT is defined. */

/* Enable bidi text support. */
status = gx_system_bidi_text_enable();

/* If status is GX_SUCCESS, bidi text support was enabled. */
```

### See Also

- gx_system_bidi_text_disable

## gx_system_canvas_refresh

Refresh all dirty canvases

### Prototype

```C
UINT gx_system_canvas_refresh(VOID);
```

### Description

This service forces an immediate redrawing of all dirty widgets and canvases. This service is normally invoked internally by the GUIX system component, but can be called by the application to force an immediate system redrawing operation.

### Parameters

None

### Return Values

- **GX_SUCCESS** (0x00) Successfully released animation block.
- **GX_INVALID_CANVAS** (0x20) No canvas created.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.

### Allowed From

Initialization and threads

### Example

```C
/* Force immediate redraw operation. */
status = gx_system_canvas_refresh();

/* If status is GX_SUCCESS, canvas has been redraw. */
```

### See Also

- gx_system_active_language_set
- gx_system_dirty_mark
- gx_system_dirty_partial_add
- gx_system_draw_context_get
- gx_system_event_fold
- gx_system_event_send
- gx_system_focus_claim
- gx_system_initialize
- gx_system_initialize
- gx_system_language_table_get
- gx_system_language_table_set
- gx_system_memory_allocator_set
- gx_system_scroll_appearance_get
- gx_system_scroll_appearance_set
- gx_system_start
- gx_system_string_get
- gx_system_string_table_get
- gx_system_string_width_get
- gx_system_timer_start
- gx_system_timer_stop
- gx_system_pen_configure
- gx_system_version_string_get
- gx_system_widget_find

## gx_system_dirty_mark

Mark area dirty

### Prototype

```C
UINT gx_system_dirty_mark(GX_WIDGET *widget);
```

### Description

This service marks the area of this widget as dirty. This effectively queues the widget for redrawing when the system event processing has been completed.

### Parameters

- *widget*: Pointer to widget control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully marked widget dirty.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Mark widget "my_widget" as dirty. */
status = gx_system_dirty_mark(&my_widget);

/* If status is GX_SUCCESS the area associated
   with "my_widget" has been marked as dirty. */
```

### See Also

- gx_system_active_language_set
- gx_system_canvas_refresh
- gx_system_dirty_partial_add
- gx_system_draw_context_get
- gx_system_event_fold
- gx_system_event_send
- gx_system_focus_claim
- gx_system_initialize
- gx_system_initialize
- gx_system_language_table_get
- gx_system_language_table_set
- gx_system_memory_allocator_set
- gx_system_scroll_appearance_get
- gx_system_scroll_appearance_set
- gx_system_start
- gx_system_string_get
- gx_system_string_table_get
- gx_system_string_width_get
- gx_system_timer_start
- gx_system_timer_stop
- gx_system_pen_configure
- gx_system_version_string_get
- gx_system_widget_find

## gx_system_dirty_partial_add

Mark partial area dirty

### Prototype

```C
UINT gx_system_dirty_partial_add(
    GX_WIDGET *widget,
    GX_RECTANGLE *dirty_area);
```

### Description

This service marks the partial area of this widget as dirty. This queues the widget for redrawing by the GUIX canvas refresh operation when the system event processing has been completed.

### Parameters

- *widget*: Pointer to widget control block.
- *dirty_area*: Dirty area of widget to mark dirty.

### Return Values

- **GX_SUCCESS** (0x00) Successful partial dirty area mark.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_SIZE (0x19) Invalid size of dirty area.

### Allowed From

Initialization and threads

### Example

```C
/* Mark widget "my_widget" partial area as dirty. */

status = gx_system_dirty_partial_add(&my_widget,
    &partial_area);

/* If status is GX_SUCCESS the partial area "partial_area"
associated with "my_widget" has been marked as dirty. */
```

### See Also

- gx_system_active_language_set
- gx_system_canvas_refresh
- gx_system_dirty_mark
- gx_system_draw_context_get
- gx_system_event_fold
- gx_system_event_send
- gx_system_focus_claim
- gx_system_initialize
- gx_system_initialize
- gx_system_language_table_get
- gx_system_language_table_set
- gx_system_memory_allocator_set
- gx_system_scroll_appearance_get
- gx_system_scroll_appearance_set
- gx_system_start
- gx_system_string_get
- gx_system_string_table_get
- gx_system_string_width_get
- gx_system_timer_start
- gx_system_timer_stop
- gx_system_pen_configure
- gx_system_version_string_get
- gx_system_widget_find

## gx_system_draw_context_get

Get drawing context

### Prototype

```C
UINT gx_system_draw_context_get(GX_DRAW_CONTEXT **current_context);
```

### Description

This service returns a pointer to the current drawing context.

### Parameters

- *current_context*: Pointer to destination for current drawing context pointer.

### Return Values

- **GX_SUCCESS** (0x00) Successful current context get.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_DRAW_CONTEXT *current_context;

/* Get current drawing context. */

status = gx_system_draw_context_get(&current_context);

/* If status is GX_SUCCESS the current drawing context
   is contained in "current_context". */
```

### See Also

- gx_system_active_language_set
- gx_system_canvas_refresh
- gx_system_dirty_mark
- gx_system_dirty_partial_add
- gx_system_event_fold
- gx_system_event_send
- gx_system_focus_claim
- gx_system_initialize
- gx_system_initialize
- gx_system_language_table_get
- gx_system_language_table_set
- gx_system_memory_allocator_set
- gx_system_scroll_appearance_get
- gx_system_scroll_appearance_set
- gx_system_start
- gx_system_string_get
- gx_system_string_table_get
- gx_system_string_width_get
- gx_system_timer_start
- gx_system_timer_stop
- gx_system_pen_configure
- gx_system_version_string_get
- gx_system_widget_find

## gx_system_event_fold

Send event

### Prototype

```C
UINT gx_system_event_fold(GX_EVENT *event);
```

### Description

This service searches the GUIX event queue for an event of the same type. If an event of the same type exists, the event payload is updated to match the new event. If no matching event is found, the gx_system_event_send function is called to add the new event to the end of the event queue.

This function is commonly used by fast touch input drivers to prevent filling the event queue with multiple PEN_DRAG events. This function can also be called by the application to prevent multiple events of the same type from being added to the GUIX event queue.

### Parameters

- *event*: Pointer to event.

### Return Values

- **GX_SUCCESS** (0x00) Successful event send.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_EVENT my_event;

memset(&my_event, 0, sizeof(GX_EVENT));

my_event.gx_event_type = GX_EVENT_PEN_DOWN;

my_event.gx_event_payload.gx_event_pointdata.gx_point_x = 100;
my_event.gx_event_payload.gx_event_pointdata.gx_point_y = 200;

/* Send "my_event" for processing. */
status = gx_system_event_fold(&my_event);

/* If status is GX_SUCCESS the event has been sent for processing. */
```

### See Also

- gx_system_active_language_set
- gx_system_canvas_refresh
- gx_system_dirty_mark
- gx_system_dirty_partial_add
- gx_system_draw_context_get
- gx_system_event_send
- gx_system_focus_claim
- gx_system_initialize
- gx_system_initialize
- gx_system_language_table_get
- gx_system_language_table_set
- gx_system_memory_allocator_set
- gx_system_scroll_appearance_get
- gx_system_scroll_appearance_set
- gx_system_start
- gx_system_string_get
- gx_system_string_table_get
- gx_system_string_width_get
- gx_system_timer_start
- gx_system_timer_stop
- gx_system_pen_configure
- gx_system_version_string_get
- gx_system_widget_find

## gx_system_event_send

Send event

### Prototype

```C
UINT gx_system_event_send(GX_EVENT *event);
```

### Description

This service sends the specified event into the GUIX system event queue. The new event is placed at the end of the queue.

### Parameters

- *event*: Pointer to event.

### Return Values

- **GX_SUCCESS** (0x00) Successful event send.
- **GX_SYSTEM_ERROR** (0xFE) Failed event send, which could be due to the queue being full.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Send "new_event" for processing. */

GX_EVENT new_event;

new_event.gx_event_target = widget ->gx_widget_parent;
new_event.gx_event_type = MY_EVENT_TYPE;

/* Set optional param. */
new_event.gx_event_payload.xxxx = yyyy
new_event.gx_event_sender = widget->gx_widget_id;

/* Push the event to event pool. */
status = gx_system_event_send(&new_event);

/* If status is GX_SUCCESS the event has been sent for processing. */
```

### See Also

- gx_system_active_language_set
- gx_system_canvas_refresh
- gx_system_dirty_mark
- gx_system_dirty_partial_add
- gx_system_draw_context_get
- gx_system_event_fold
- gx_system_focus_claim
- gx_system_initialize
- gx_system_initialize
- gx_system_language_table_get
- gx_system_language_table_set
- gx_system_memory_allocator_set
- gx_system_scroll_appearance_get
- gx_system_scroll_appearance_set
- gx_system_start
- gx_system_string_get
- gx_system_string_table_get
- gx_system_string_width_get
- gx_system_timer_start
- gx_system_timer_stop
- gx_system_pen_configure
- gx_system_version_string_get
- gx_system_widget_find

## gx_system_focus_claim

Claim focus

### Prototype

```C
UINT gx_system_focus_claim(GX_WIDGET *widget);
```

### Description

This service claims the focus for the specified widget. If the widget did no previously have focus, it will receive a GX_EVENT_FOCUS_GAINED event.

### Parameters

- *widget*: Pointer to widget control block to claim focus.

### Return Values

- **GX_SUCCESS** (0x00) Successful focus claim.
- **GX_NO_CHANGE** (0x08) Widget already owns focus.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_INVALID_WIDGET (0x12 Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Claim focus for widget "my_widget". */
status = gx_system_claim_focus(&my_widget);

/* If status is GX_SUCCESS the focus has been claimed for
   "my_widget". */
```

### See Also

- gx_system_active_language_set
- gx_system_canvas_refresh
- gx_system_dirty_mark
- gx_system_dirty_partial_add
- gx_system_draw_context_get
- gx_system_event_fold
- gx_system_event_send
- gx_system_initialize
- gx_system_initialize
- gx_system_language_table_get
- gx_system_language_table_set
- gx_system_memory_allocator_set
- gx_system_scroll_appearance_get
- gx_system_scroll_appearance_set
- gx_system_start
- gx_system_string_get
- gx_system_string_table_get
- gx_system_string_width_get
- gx_system_timer_start
- gx_system_timer_stop
- gx_system_pen_configure
- gx_system_version_string_get
- gx_system_widget_find

## gx_system_initialize

Initialize GUIX

### Prototype

```C
UINT gx_system_initialize(VOID);
```

### Description

This service initializes GUIX. This service must be invoked before any other GUIX API service, and should only be invoked once at system startup.

### Parameters

- None

### Return Values

- **GX_SUCCESS** (0x00) Successful system initialize.
- **GX_SYSTEM_ERROR** (0xFE) Invalid GX_EVENT control block size or event queue/mutex/thread create failed.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.

### Allowed From

Initialization and threads

### Example

```C
/* Initialize GUIX. */
status = gx_system_initialize();

/* If status is GX_SUCCESS, GUIX has been initialized. */
```

### See Also

- gx_system_active_language_set
- gx_system_canvas_refresh
- gx_system_dirty_mark
- gx_system_dirty_partial_add
- gx_system_draw_context_get
- gx_system_event_fold
- gx_system_event_send
- gx_system_focus_claim
- gx_system_initialize
- gx_system_language_table_get
- gx_system_language_table_set
- gx_system_memory_allocator_set
- gx_system_scroll_appearance_get
- gx_system_scroll_appearance_set
- gx_system_start
- gx_system_string_get
- gx_system_string_table_get
- gx_system_string_width_get
- gx_system_timer_start
- gx_system_timer_stop
- gx_system_pen_configure
- gx_system_version_string_get
- gx_system_widget_find

## gx_system_language_table_get

Retrieve active language table

### Prototype

```C
UINT gx_system_language_table_get( 
    GX_CHAR ****language_table,
    GX_UBYTE *languages_count, 
    UINT *string_count);
```

### Description

This service retrieves the active language table. This function is deprecated in favor of gx_display_language_table_get. All new applications should use gx_display_language_table_get.

### Parameters

- *language_table*: Address of pointer to return language table.
- *languages_count*: Address of variable to return table columns.
- *string_count*: Address of pointer to return table rows.

### Return Values

- **GX_SUCCESS** (0x00) Successfully retrieved active language table.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_CHAR ***language_table;

GX_UBYTE language_count;

UINT string_count;

/* Retrieve the language table. */

status = gx_system_language_table_get(&language_table, &language_count, &string_count);
```

### See Also

- gx_display_language_table_get
- gx_display_language_table_set

## gx_system_language_table_set

Assign active language table

### Prototype

```C
UINT gx_system_language_table_set(
    GX_CHAR ***language_table,
    GX_UBYTE languages_count, 
    UINT string_count);
```

### Description

This service installs the active language table. This function has been deprecated in favor of gx_display_language_table_set. All new applications should use gx_display_language_table_set.

### Parameters

- *language_table*: Pointer to language table.
- *languages_count*: Number of languages in table.
- *string_count*: Number of string table rows.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set language table.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Retrieve the language table. */

status = gx_system_language_table_set(language_table,
    language_count, string_count);
```

### See Also

- gx_display_language_table_set
- gx_display_language_table_get
- gx_display_active_language_set

## gx_system_memory_allocator_set

Assign functions for memory allocation, de-allocation

### Prototype

```C
UINT gx_system_memory_allocator_set(VOID *(allocate)(ULONG size),
    VOID(*release)(VOID *));
```

### Description

This service assigns the application supplied callback function for dynamic memory allocation and de-allocation.

If no GUIX service that uses dynamic memory allocation is needed by the application, this service does not need to be called.

If used, this service should be called after gx_system_initialize() which clears the GUIX service pointers, and before any GUIX service that requires use of dynamical memory allocation.

GUIX services which require a runtime memory allocation and de-allocation service include:

- Loading binary resources from external storage into the GUIX runtime environment.
- The software runtime jpeg image decoder.
- The software runtime png image decoder.
- Using text widgets with GX_STYLE_TEXT_COPY.
- Runtime pixelmap resize and rotation utility functions.
- Runtime screen and widget control block allocation.

### Parameters

- *allocator*: Memory allocator function.
- *release*: Memory free function.

### Return Values

- **GX_SUCCESS** (0x00) Successfully assigned memory allocate function.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

The following example utilizes a ThreadX byte pool to implement a threadsafe dynamic memory allocation and memory de-allocation service.

```C
TX_BYTE_POOL memory_pool;

#define SCRATCHPAD_SIZE (1024 * 4)

ULONG scratchpad[SCRATCHPAD_SIZE];

/* Define memory allocation service. */

VOID *memory_allocate(ULONG size)
{
    VOID *memptr;

    if (tx_byte_allocate(&memory_pool, &memptr, size, TX_NO_WAIT) == TX_SUCCESS)
    {
        return memptr;
    }
    return NULL;
}

/* Define memory de-allocation service. */
void memory_free(VOID *mem)
{
    tx_byte_release(mem);
}

/* Create byte pool and install our dynamic memory services with GUIX
*/

VOID tx_application_define(void *first_unused_memory)
{
    /* Create byte pool for GUIX to use. */
    tx_byte_pool_create(&memory_pool, "scratchpad", scratchpad,
        SCRATCHPAD_SIZE * sizeof(ULONG));

    guix_setup();

    /* Install our memory allocator and de-allocator. */
    gx_system_memory_allocator_set(memory_allocate, memory_free);
}
```

### See Also

- gx_system_active_language_set
- gx_system_canvas_refresh
- gx_system_dirty_mark
- gx_system_dirty_partial_add
- gx_system_draw_context_get
- gx_system_event_fold
- gx_system_event_send
- gx_system_focus_claim
- gx_system_initialize
- gx_system_initialize
- gx_system_language_table_get
- gx_system_language_table_set
- gx_system_scroll_appearance_get
- gx_system_scroll_appearance_set
- gx_system_start
- gx_system_string_get
- gx_system_string_table_get
- gx_system_string_width_get
- gx_system_timer_start
- gx_system_timer_stop
- gx_system_pen_configure
- gx_system_version_string_get
- gx_system_widget_find

## gx_system_pen_configure

Set pen configuration

### Prototype

```C
UINT gx_system_pen_configure(GX_PEN_CONFIGURATION *pen_configuration);
```

### Description

This service sets pen configuration to control the pen speed and distance parameters used to trigger the generation of GX_EVENT_FLICK event types.

The gx_pen_configuration_min_drag_dist member of GX_PEN_CONFIGURATION is a fixed point data type, and you should use GX_FIXED_VAL_MAKE(value) to convert from INT to GX_FIXED_VAL. For example, if you want to set minimum drag distance to 0.5 pixel per tick, you have to set the
gx_pen_configuration_min_drag_dist to `GX_FIXED_VAL_MAKE(1) / 2`.

In GUIX releases 5.4.0 and older, the gx_pen_configuration_min_drag_dist member of GX_PEN_CONFIGURATION was of (INT << 8) type rather than GX_FIXED_VAL type. If your project with 5.4.0 version GUIX library is using this API, you will need to modify the min_drag_dist parameter or #define GUIX_5_4_0_COMPATIBILITY when building the GUIX library.

### Parameters

- *pen_configuration*: Pointer to pen configuration structure. **Appendix I** contains definition to GX_PEN_CONFIGURATION structure.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set pen configuration.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.

### Allowed From

Initialization and threads

### Example

```C
/* Define pen configuration, set minimum drag distance to 0.5 pixel
per tick, maximum pen speed to 10 ticks. That means GUIX will trigger
a vertical or horizontal flick event if the drag time is smaller than
10 ticks and the drag distance is bigger than 0.5 * drag_ticks. */

GX_PEN_CONFIGURATION pen_configuration;

#if defined(GUIX_5_4_0_COMPATIBILITY)
Pen_configuration.gx_pen_configuration_min_drag_dist = (1 << 8)/ 2;
#else
pen_configuration.gx_pen_configuration_min_drag_dist = GX_FIXED_VAL_MAKE(1) / 2;
#endif

pen_configuration.gx_pen_configuration_max_pen_speed_ticks = 10;

/* Set the pen configuration. */
status = gx_system_pen_configure(&pen_configuration);

/* If status is GX_SUCCESS the touch configure has been set. */
```

## gx_system_screen_stack_create

Create and initialize the system screen stack

### Prototype

```C
UINT gx_system_screen_stack_create(
    GX_WIDGET **memory, 
    INT size);
```

### Description

This service defines a memory pool to be used for the system screen stack. The system screen stack is an optional feature that can be used by the application to manage application screen flow appearance.

If the application intends to utilize the screen stack services, the gx_system_screen_stack_create function must first be called to setup the screen stack memory region.

### Parameters

- *memory*: Pointer to the reserved memory block.
- *size*: Size, in bytes, of the reserved memory block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully creation.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
#define SCREEN_STACK_DEPTH 8
GX_WIDGET *screen_stack[SCREEN_STACK_DEPTH * 2];

UINT status;

/* Get the scrollbar appearance. */

status = gx_system_screen_stack_create(screen_stack,
    sizeof(GX_WIDGET *) * SCREEN_STACK_DEPTH * 2);

/* If status is GX_SUCCESS the system screen stack is initialized
and ready for use. */
```

### See Also

- gx_system_screen_stack_get
- gx_system_screen_stack_pop
- gx_system_screen_stack_push
- gx_system_screen_stack_reset

## gx_system_screen_stack_get

Pop the topmost screen stack pointers

### Prototype

```C
UINT gx_system_screen_stack_get(
    GX_WIDGET **popped_parent,
    GX_WIDGET **popped_screen);
```

### Description

This function pops the topmost screen stack pointers and returns those pointers to the caller. This function differs from gx_system_screen_stack_pop() in that the popped screen is not automatically reattached to the previous parent. Instead, the pointers are popped from the stack and returned to the caller, allowing the caller to attach or discard the returned screen as desired.

### Parameters

- *popped_parent*: Location to store the parent widget pointer.
- *popped_screen*: Location to store the popped screen pointer.

### Return Values

- **GX_SUCCESS** (0x00) Successfully retrieval of screen stack pointers.
- **GX_FAILURE** (0x10) Invalid or empty screen stack.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
UINT status;

GX_WIDGET *parent_screen;

GX_WIDGET *popped_screen;

/* Pop a screen stack entry. */
status = gx_system_screen_stack_get(&parent_screen, &popped_screen);

/* If status is GX_SUCCESS, parent_screen and
popped_screen hold the topmost screen stack pointers. */
```

### See Also

- gx_system_screen_stack_create
- gx_system_screen_stack_pop
- gx_system_screen_stack_push
- gx_system_screen_stack_reset

## gx_system_screen_stack_pop

Pop the topmost entry from the system screen stack

### Prototype

```C
UINT gx_system_screen_stack_pop();
```

### Description

This function pops the topmost entry fo the screen stack and automatically attaches the popped screen to the popped parent widget.

### Parameters

None

### Return Values

- **GX_SUCCESS** (0x00) Successfully retrieval of screen stack pointers.
- **GX_FAILURE** (0x10) Invalid or empty screen stack.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
UINT status;

/* Pop a screen stack entry. */
status = gx_system_screen_stack_pop();

/* If status is GX_SUCCESS, the topmost screen stack entry has been
popped from the stack and re-attached to the previous parent. */
```

### See Also

- gx_system_screen_stack_get
- gx_system_screen_stack_create
- gx_system_screen_stack_push
- gx_system_screen_stack_reset

## gx_system_screen_stack_push

Push a widget and parent pointer to the screen stack

### Prototype

```C
UINT gx_system_screen_stack_push(GX_WIDGET *screen)
```

### Description

This service places a pointer to the indicated widget, which is usually a top-level screen, onto the screen stack. If the widget has a parent it is detached from the parent. The parent widget pointer is also pushed to the screen stack. The parent widget may be NULL, meaning a screen that is not visible or attached to any parent may be pushed onto the screen stack. If a widget with no parent is pushed to the screen stack, the screen_stack_get() API should be used to retrieve the pushed screen pointer, rather than using the screen_stack_pop() API, which attempts to reattached the popped widget to its previous parent.

### Parameters

- *screen*: Pointer to the widget to be pushed to the screen stack.

### Return Values

- **GX_SUCCESS** (0x00) Successfully get scrollbar appearance.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Get the scrollbar appearance. */
status = gx_system_screen_stack_push(window);

/* If status is GX_SUCCESS, the widget pointed to by "window" has
been pushed to the screen stack, along with the widget's parent
ponter. */
```

### See Also

- gx_system_screen_stack_get
- gx_system_screen_stack_pop
- gx_system_screen_stack_create
- gx_system_screen_stack_reset

## gx_system_screen_stack_reset

Reset the system screen stack

### Prototype

```C
UINT gx_system_screen_stack_reset();
```

### Description

This function removes all entries from the system screen stack. If the screens popped from the stack have dynamically allocated control blocks allocated by GUIX Studio, the memory for those control blocks is freed.

### Parameters

None

### Return Values

- **GX_SUCCESS** (0x00) Successfully get scrollbar appearance.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Get the scrollbar appearance. */
status = gx_system_screen_stack_reset();

/* If status is GX_SUCCESS the system screen stack has been cleared
of entries. */
```

### See Also

- gx_system_screen_stack_get
- gx_system_screen_stack_pop
- gx_system_screen_stack_push
- gx_system_screen_stack_create

## gx_system_scroll_appearance_get

Get scroll appearance

### Prototype

```C
UINT gx_system_scroll_appearance_get(
    ULONG style,
    GX_SCROLLBAR_APPEARANCE *return_appearance);
```

### Description

This service gets the scrollbar appearance.

### Parameters

- *style*: Scrollbar style: `GX_SCROLLBAR_HORIZONTAL` or `GX_SCROLLBAR_VERTICAL`.
- *return_appearance*: Pointer to destination for appearance. **Appendix I** contains definition to GX_SCROLLBAR_APPERANCE.

### Return Values

- **GX_SUCCESS** (0x00) Successfully get scrollbar appearance.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_SCROLLBAR_APPEARANCE my_appearance;

/* Get the scrollbar appearance. */

status = gx_system_scroll_appearance_get(style, &my_appearance);

/* If status is GX_SUCCESS "my_appearance" now contains the scroll
appearance. */
```

### See Also

- gx_system_active_language_set
- gx_system_canvas_refresh
- gx_system_dirty_mark
- gx_system_dirty_partial_add
- gx_system_draw_context_get
- gx_system_event_fold
- gx_system_event_send
- gx_system_focus_claim
- gx_system_initialize
- gx_system_initialize
- gx_system_language_table_get
- gx_system_language_table_set
- gx_system_memory_allocator_set
- gx_system_scroll_appearance_set
- gx_system_start
- gx_system_string_get
- gx_system_string_table_get
- gx_system_string_width_get
- gx_system_timer_start
- gx_system_timer_stop
- gx_system_pen_configure
- gx_system_version_string_get
- gx_system_widget_find

## gx_system_scroll_appearance_set

Set scroll appearance

### Prototype

```C
UINT gx_system_scroll_appearance_set(
    ULONG style,
    GX_SCROLLBAR_APPEARANCE *appearance);
```

### Description

This service sets the default scroll appearance. When a scroll is created, this appearance structure is used unless the application provides a custom version.

### Parameters

- *style*: Scroll style `GX_SCROLLBAR_HORIZONTAL` or `GX_SCROLLBAR_VERTICAL`.
- *appearance*: Pointer to appearance structure initialized with various scrollbar appearance attributes. Refer to **Appendix I** for the definition of the GX_SCROLLBAR_APPEARANCE structure.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set scroll appearance set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_SCROLLBAR_APPEARANCE my_appearance;

memset(&my_appearance, 0, sizeof(GX_SCROLLBAR_APPEARANCE));

my_appearance.gx_scroll_width = 20;
my_appearance.gx_scroll_thumb_width = 18;

my_appearance.gx_scroll_thumb_color = GX_COLOR_ID_SCROLL_BUTTON;
my_appearance.gx_scroll_thumb_border_color = GX_COLOR_ID_SCROLL_BUTTON;
my_appearance.gx_scroll_button_color = GX_COLOR_ID_SCROLL_BUTTON;
my_appearance.gx_scroll_thumb_travel_min = 20;
my_appearance.gx_scroll_thumb_travel_max = 20;
my_appearance.gx_scroll_thumb_border_style = GX_STYLE_BORDER_THIN;

/* Set the scroll appearance. */

status = gx_system_scroll_appearance_set(GX_SCROLLBAR_VERTICAL, &my_appearance);

/* If status is GX_SUCCESS the scroll appearance has been set. */
```

### See Also

- gx_system_active_language_set
- gx_system_canvas_refresh
- gx_system_dirty_mark
- gx_system_dirty_partial_add
- gx_system_draw_context_get
- gx_system_event_fold
- gx_system_event_send
- gx_system_focus_claim
- gx_system_initialize
- gx_system_initialize
- gx_system_language_table_get
- gx_system_language_table_set
- gx_system_scroll_appearance_get
- gx_system_start
- gx_system_string_get
- gx_system_string_table_get
- gx_system_string_width_get
- gx_system_timer_start
- gx_system_timer_stop
- gx_system_pen_configure
- gx_system_version_string_get
- gx_system_widget_find

## gx_system_start

Start GUIX

### Prototype

```C
UINT gx_system_start(VOID);
```

### Description

This service starts GUIX processing. Under normal circumstances this function never returns, but instead begins processing the GUIX event queue. When the GUIX event queue is empty, this service suspends the calling thread until new events arrive in the GUIX event queue.

### Parameters

- None

### Return Values

- **GX_SUCCESS** (0x00) Successful system start.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.

### Allowed From

Initialization and threads

### Example

```C
/* Start GUIX. */
status = gx_system_start();

/* If status is GX_SUCCESS . GUIX has been started. */
```

### See Also

- gx_system_active_language_set
- gx_system_canvas_refresh
- gx_system_dirty_mark
- gx_system_dirty_partial_add
- gx_system_draw_context_get
- gx_system_event_fold
- gx_system_event_send
- gx_system_focus_claim
- gx_system_initialize
- gx_system_initialize
- gx_system_language_table_get
- gx_system_language_table_set
- gx_system_memory_allocator_set
- gx_system_scroll_appearance_get
- gx_system_scroll_appearance_set
- gx_system_string_get
- gx_system_string_table_get
- gx_system_string_width_get
- gx_system_timer_start
- gx_system_timer_stop
- gx_system_pen_configure
- gx_system_version_string_get
- gx_system_widget_find

## gx_system_string_get

Get string

### Prototype

```C
UINT gx_system_string_get(
    GX_RESOURCE_ID string_id,
    GX_CHAR **return_string);
```

### Description

This service gets the string for the specified resource ID, using the first defined display and the currently active language. This function has been deprecated in favor of gx_display_string_get. All new applications should use gx_display_string_get.

### Parameters

- *string_id*: String resource ID.
- *return_string*: Pointer to string destination pointer.

### Return Values

- **GX_SUCCESS** (0x00) Successful string get.
- **GX_INVALID_RESOURCE_ID** (0x33) Invalid resource ID.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07): Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Get the string associated with MY_STRING_ID. */

status = gx_system_string_get(MY_STRING_RESOURCE_ID, &my_string);

/* If status is GX_SUCCESS the string is contained in "my_string".
*/
```

### See Also

- gx_display_string_get
- gx_display_string_table_get
- gx_display_language_table_set

## gx_system_string_table_get

Retrieves the string table

### Prototype

```C
UINT gx_system_string_table_get(
    GX_UBYTE language,
    GX_CHAR ***string_table,
    UINT *get_size);
```

### Description

This service retrieves the string table for the requested language from the first display. This function has been deprecated in favor of gx_display_string_table_get. All new applications should use gx_display_string_table_get.

### Parameters

- *language*: Language index.
- *string_table*: Pointer to storage space of the string table pointer, or NULL if the caller does not need to get the pointer to the string table.
- *get_size*: Pointer to the storage for the number of strings in string table, or NULL if the caller does not need to get the number of strings in the string table.

### Return Values

- **GX_SUCCESS** (0x00) Successful string table get.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.

### Allowed From

Initialization and threads

### Example

```C
/* Get the string table. */

CHAR **my_string_table; UINT table_size;

status = gx_system_string_table_get(LANGUAGE_ID_ENGLISH,
    &my_string_table, &table_size);

/* If status is GX_SUCCESS . the pointer to the string table has
been obtained. */
```

### See Also

- gx_display_string_table_get
- gx_display_string_get
- gx_display_active_language_set
- gx_display_language_table_set

## gx_system_string_width_get

Get string width (deprecated)

### Prototype

```C
UINT gx_system_string_width_get(
    GX_FONT *font, GX_CHAR *string,
    INT string_length,
    GX_VALUE *return_width);
```

### Description

This service is deprecated in favor of gx_system_string_width_get_ext().

This service computes the display width of the supplied string in pixels using the specified font. If the string_length parameter is >= 0, only the request count of characters are included in the calculation. If the string_length parameter is -1, the entire string up to the NULL terminator is used in the calculation.

### Parameters

- *font*: Pointer to string's font.
- *string*: Pointer to string.
- *string_length*: Length of string.
- *return_width*: Destination for width of string.

### Return Values

- **GX_SUCCESS** (0x00) Successful string width get.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_INVALID_FONT (0x16) Invalid font.
- GX_INVALID_STRING_LENGTH (0x34) Invalid string length.

### Allowed From

Initialization and threads

### Example

```C
/* Get the string width of "my_string". */

status = gx_system_string_width_get(&my_font, &my_string,
    strlen(my_string), &my_width);

/* If status is GX_SUCCESS . "my_width" contains the string width.
*/
```

### See Also

- gx_system_string_width_get_ext

## gx_system_string_width_get_ext

Get string width

### Prototype

```C
UINT gx_system_string_width_get_ext(
    GX_FONT *font,
    GX_STRING *string,
    GX_VALUE *return_width);
```

### Description

This service computes the display width of the supplied string in pixels using the specified font.

### Parameters

- *font*: Pointer to string's font.
- *string*: Pointer to string.
- *return_width*: Destination for width of string.

### Return Values

- **GX_SUCCESS** (0x00) Successful string width get.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_INVALID_FONT (0x16) Invalid font.
- GX_INVALID_STRING_LENGTH (0x34) Invalid string length.

### Allowed From

Initialization and threads

### Example

```C
GX_STRING my_string; my_string.gx_string_ptr = "Monday";

my_string.gx_string_length = strlen(my_string.gx_string_ptr);

/* Get the string width of "my_string". */

status = gx_system_string_width_get_ext(&my_font,
    &my_string, &my_width);

/* If status is GX_SUCCESS . "my_width" contains the string width.
*/
```

### See Also

- gx_system_event_fold
- gx_system_focus_claim
- gx_system_initialize
- gx_system_language_table_get
- gx_system_language_table_set
- gx_system_memory_allocator_set
- gx_system_scroll_appearance_get
- gx_system_scroll_appearance_set
- gx_system_start
- gx_system_string_get
- gx_system_string_table_get
- gx_system_timer_start
- gx_system_timer_stop
- gx_system_pen_configure
- gx_system_version_string_get
- gx_system_widget_find

## gx_system_text_render_style_set

Set text rendering style

### Prototype

```C
UINT  gx_system_text_render_style_set(GX_UBYTE style)
```

### Description

This service sets style for rendering text. Currently this service is used for Thai glyph shaping. The service is availlabel only when configured with **GX_THAI_GLYPH_SHAPING_SUPPORT**.

When rendering Thai string, it needs to adjusts Thai glyph position by substitution technique, involving extra glyph sets that defined in Private Use Area.
GUIX Studio supports generate adjusted Thai strings automatically when "Generate adjusted Thai string" is configured through language setting dialog. If you want to shape the Thai string dynamically, you need to call this service to set GX_TEXT_RENDER_THAI_GLYPH_SHAPING style. Then, the text rendering engine will execute the necessary shaping prior to rendering the string.

### Parameters

- *style*: Style for rendering text. It can be one of the following values:
    - GX_TEXT_RENDER_THAI_GLYPH_SHAPING

### Return Values

- **GX_SUCCESS** (0x00) Successfully set text render style.

### Allowed From

Initialization and threads

### Example

```C
/* Set rendering style GX_TEXT_RENDER_THAI_GLYPH_SHAPING. */
status = gx_system_text_render_style_set(GX_TEXT_RENDER_THAI_GLYPH_SHAPING);

/* If status is GX_SUCCESS. The style for text rendering has been set. */
```

### See Also

- gx_system_event_fold
- gx_system_focus_claim
- gx_system_initialize
- gx_system_language_table_get
- gx_system_language_table_set
- gx_system_memory_allocator_set
- gx_system_scroll_appearance_get
- gx_system_scroll_appearance_set
- gx_system_start
- gx_system_string_get
- gx_system_string_table_get
- gx_system_string_width_get
- gx_system_timer_stop
- gx_system_pen_configure
- gx_system_version_string_get
- gx_system_widget_find

## gx_system_timer_start

Start timer

### Prototype

```C
UINT gx_system_timer_start(
    GX_WIDGET *owner, 
    UINT timer_id,
    UINT initial_ticks,
    UINT reschedule_ticks);
```

### Description

This service starts a timer for the specified widget. The constant GX_MAX_ACTIVE_TIMERS defined the maximum active timers supported. The default setting for this value is 32.

### Parameters

- *owner*: Pointer to widget control block.
- *timer_id*: ID of timer.
- *initial_ticks*: initial expiration ticks.
- *reschedule_ticks*: Periodic expiration ticks.

### Return Values

- **GX_SUCCESS** (0x00) Successful timer start.
- GX_OUT_OF_TIMERS (0x04) No more timers.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_VALUE (0x22) Timer value(s) not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Start a periodic timer for the widget "my_widget". */
status = gx_system_timer_start(&my_widget, MY_TIMER_ID, 10, 20);

/* If status is GX_SUCCESS . the timer for "my_widget" has been
started. */
```

### See Also

- gx_system_event_fold
- gx_system_focus_claim
- gx_system_initialize
- gx_system_language_table_get
- gx_system_language_table_set
- gx_system_memory_allocator_set
- gx_system_scroll_appearance_get
- gx_system_scroll_appearance_set
- gx_system_start
- gx_system_string_get
- gx_system_string_table_get
- gx_system_string_width_get
- gx_system_timer_stop
- gx_system_pen_configure
- gx_system_version_string_get
- gx_system_widget_find

## gx_system_timer_stop

Stop timer

### Prototype

```C
UINT gx_system_timer_stop(
    GW_WIDGET *owner, 
    UINT timer_id);
```

### Description

This service stops the timer with the specified timer_id associated with the calling widget. To stop all timers linked to a particular widget, the application can pass the timer_id value of 0.

### Parameters

- *owner*: Pointer to widget control block.
- *timer_id*: ID of timer, or 0 for all timers.

### Return Values

- **GX_SUCCESS** (0x00) Successful timer stop.
- GX_NOT_FOUND (0x09) Timer ID not found.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Stop the periodic timer for the widget "my_widget". */
status = gx_system_timer_stop(&my_widget, MY_TIMER_ID);

/* If status is GX_SUCCESS . the timer for "my_widget" has been
stopped. */
```

### See Also

- gx_system_event_fold
- gx_system_focus_claim
- gx_system_initialize
- gx_system_language_table_get
- gx_system_language_table_set
- gx_system_memory_allocator_set
- gx_system_scroll_appearance_get
- gx_system_scroll_appearance_set
- gx_system_start
- gx_system_string_get
- gx_system_string_table_get
- gx_system_string_width_get
- gx_system_timer_start
- gx_system_pen_configure
- gx_system_version_string_get
- gx_system_widget_find

## gx_system_version_string_get

Retrieve GUIX library version string (deprecated)

### Prototype

```C
UINT gx_system_version_string_get(GX_CHAR **version);
```

### Description

This service is deprecated in favor of gx_system_version_string_get_ext().

This service retrieves the GUIX library version string.

### Parameters

- *version*: Pointer to return string value.

### Return Values

- **GX_SUCCESS** (0x00) Successfully retrieved version string.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_CHAR *version;

/* Get the library version string. */

status = gx_system_verrsion_string_get(&version);
```

### See Also

- gx_system_version_string_get_ext()

## gx_system_version_string_get_ext

Retrieve GUIX library version string

### Prototype

```C
UINT gx_system_version_string_get_ext(GX_STRING *version);
```

### Description

This service retrieves the GUIX library version string.

### Parameters

- *version*: Pointer to return string value.

### Return Values

- **GX_SUCCESS** (0x00) Successfully retrieved version string.
- **GX_INVALID_STRING_LENGTH** (0x34) Invalid string length.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_STRING version;

/* Get the library version string. */

status = gx_system_version_string_get_ext(&version);
```

### See Also

- gx_system_active_language_set
- gx_system_canvas_refresh
- gx_system_dirty_mark
- gx_system_dirty_partial_add
- gx_system_draw_context_get
- gx_system_event_fold
- gx_system_event_send
- gx_system_focus_claim
- gx_system_initialize
- gx_system_initialize
- gx_system_language_table_get
- gx_system_language_table_set
- gx_system_memory_allocator_set
- gx_system_scroll_appearance_get
- gx_system_scroll_appearance_set
- gx_system_start
- gx_system_string_get
- gx_system_string_table_get
- gx_system_string_width_get
- gx_system_timer_start
- gx_system_timer_stop
- gx_system_pen_configure
- gx_system_widget_find

## gx_system_widget_find

Find widget

### Prototype

```C
UINT gx_system_widget_find(
    USHORT widget_id,
    INT search_level, 
    GX_WIDGET **return_search_result);
```

### Description

This service searches for the specified widget ID. Unlike gx_widget_find(), this function searches the children of all root windows defined in the system, meaning this is an exhaustive search of all visible widgets. If you know the parent of the widget you are searching for, use gx_widget_find() instead.

### Parameters

- *widget_id*: Widget ID to search for.
- *search_level*: Defines the recursive nesting level into which child widgets are searched. If this value is 0, only immediate children of each root window are searched. If this value is GX_SEARCH_DEPTH_INFINITE, the function nests down into all children searching for the requested widget ID. For any other value > 0, the search level defines how deeply nested this function will go searching for the requested widget ID.
- *return_search_result*: Pointer to destination for widget found.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget search.
- **GX_NOT_FOUND** (0x09) Widget ID not found.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
/* Search recursively from the top level widget for the widget with
ID of MY_WIDGET_ID. */

status = gx_system_widget_find(MY_WIDGET_ID,
    GX_SEARCH_DEPTH_INFINITE,
    &my_widget);

/* If status is GX_SUCCESS . the search was successful and
"my_widget" contains the pointer to the widget. */
```

### See Also

- gx_system_active_language_set
- gx_system_canvas_refresh
- gx_system_dirty_mark
- gx_system_dirty_partial_add
- gx_system_draw_context_get
- gx_system_event_fold
- gx_system_event_send
- gx_system_focus_claim
- gx_system_initialize
- gx_system_initialize
- gx_system_language_table_get
- gx_system_language_table_set
- gx_system_memory_allocator_set
- gx_system_scroll_appearance_get
- gx_system_scroll_appearance_set
- gx_system_start
- gx_system_string_get
- gx_system_string_table_get
- gx_system_string_width_get
- gx_system_timer_start
- gx_system_timer_stop
- gx_system_pen_configure
- gx_system_version_string_get

## gx_text_button_create

Create text button

### Prototype

```C
UINT gx_text_button_create(
    GX_TEXT_BUTTON *text_button,
    GX_CONST GX_CHAR *name, 
    GX_WIDGET *parent,
    GX_RESOURCE_ID text_id, 
    ULONG style,
    USHORT text_button_id, 
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a text button widget.

GX_TEXT_BUTTON is derived from GX_BUTTON and supports all gx_button API services.

### Parameters

- *text_button*: Pointer to text button control block.
- *name*: Logical name of text button.
- *parent*: Pointer to parent widget of the button.
- *text_id*: Resource ID of text.
- *style*: Text button style. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *text_button_id*: Application-defined ID of the text button.
- *size*: Size of the button.

### Return Values

- **GX_SUCCESS** (0x00) Successful text button create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.
- GX_INVALID_WIDGET (0x12) Parent widget not valid.


### Allowed From

Initialization and threads

### Example

```C
GX_TEXT_BUTTON my_text_button;

GX_RECTANGLE size;

/* Define widget size. */

gx_utility_rectangle_define(&size, 0, 0, 100, 100);

/* Create text button "my_text_button". */

status = gx_text_button_create(&my_text_button, "my text button",
    &my_parent_window, MY_TEXT_RESOURCE_ID,
    GX_STYLE_BUTTON_TOGGLE, MY_TEXT_BUTTON_ID, &size);

/* If status is GX_SUCCESS, the text button "my_text_button" was
created. */
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_icon_button_create
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_text_button_color_set
- gx_text_button_draw
- gx_text_button_event_process
- gx_text_button_font_set
- gx_text_button_text_get
- gx_text_button_text_set
- gx_text_button_text_id_set

## gx_text_button_draw

Draw text button

### Prototype

```C
VOID gx_text_button_draw(GX_TEXT_BUTTON *button);
```

### Description

This service draws the text button. This service is normally called internally during canvas refresh, but can also be called from custom text button drawing functions.

### Parameters

- *button*: Pointer to text button control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom text button draw function. */

VOID my_text_button_draw(GX_TEXT_BUTTON *text_button)
{
    /* Call default text button draw. */
    gx_text_button_draw(text_button);

    /* Add your own drawing here. */
}
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_icon_button_create
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_text_button_create
- gx_text_button_event_process
- gx_text_button_color_set
- gx_text_button_font_set
- gx_text_button_text_get
- gx_text_button_text_set
- gx_text_button_text_id_set

## gx_text_button_event_process


Process text button event

### Prototype

```C
UINT gx_text_button_event_process(GX_TEXT_BUTTON *text_button, GX_EVENT *event_ptr);
```

### Description

This service processes an event for the specified text button. This service should be called as the default event handler by any custom text button event processing functions.

### Parameters

- *text_button*: Pointer to text button control block.
- *event_ptr*: Pointer to the event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful text button event process.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Call generic text button event processing as part of custom event processing function. */

UINT custom_text_button_event_process(GX_TEXT_BUTTON *text_button, GX_EVENT *event)
{
    UINT status = GX_SUCCESS;

    switch(event->gx_event_type)
    {
    case xyz:
        /* Insert custom event handling here. */
        break;

    default:
        /* Pass all other events to the default text button event processing. */
        status = gx_text_button_event_process(wheel, event);
        break;
    }
    return status;
}
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_icon_button_create
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_text_button_create
- gx_text_button_draw
- gx_text_button_color_set
- gx_text_button_font_set
- gx_text_button_text_get
- gx_text_button_text_set
- gx_text_button_text_id_set

## gx_text_button_font_set

Set the font to text button

### Prototype

```C
UINT gx_text_button_font_set(
    GX_TEXT_BUTTON *button,
    GX_RESOURCE_ID font_id);
```

### Description

This service assigns a font to the specified button.

### Parameters

- *button*: Pointer to text button control block.
- *font_id*: Resource ID fo the font.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set the font.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set the text button with the font ID MY_FONT. */
status = gx_text_button_font_set(&my_text_button, MY_FONT);

/* If status is GX_SUCCESS, the font of the text button
"my_text_button" was set to MY_FONT. */
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_icon_button_create
- gx_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_text_button_create
- gx_text_button_draw
- gx_text_button_event_process
- gx_text_button_color_set
- gx_text_button_text_get
- gx_text_button_text_set
- gx_text_button_text_id_set

## gx_text_button_text_color_set

Set text button color

### Prototype

```C
UINT gx_text_button_text_color_set(
    GX_TEXT_BUTTON *text_button,
    GX_RESOURCE_ID normal_text_color_id,
    GX_RESOURCE_ID selected_text_color_id,
    GX_RESOURCE_ID disabled_text_color_id);
```

### Description

This service sets the color of the text button.

### Parameters

- *text_button*: Pointer to text button control block.
- *normal_text_color_id*: Resource ID of normal text. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.
- *selected_text_color_id*: Resource ID of selected text. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.
- *disabled_text_color_id*: Resource ID of color for disabled text. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.

### Return Values

- **GX_SUCCESS** (0x00) Successful text button color set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set the color of the text button "my_text_button". */
status = gx_text_button_text_color_set(&my_text_button,
    GX_COLOR_ID_NORMAL_TEXT,
    GX_COLOR_ID_SELECTED_TEXT,
    GX_COLOR_ID_DISABLED_TEXT);

/* If status is GX_SUCCESS, the text color of "my_text_button" was
set. */
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_icon_button_create, x_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_text_button_create
- gx_text_button_draw
- gx_text_button_event_process
- gx_text_button_font_set
- gx_text_button_text_get
- gx_text_button_text_set
- gx_text_button_text_id_set

## gx_text_button_text_draw

Support function to draw button text

### Prototype

```C
VOID gx_text_button_text_draw(GX_TEXT_BUTTON *text_button);
```

### Description

This support function draws the text portion of a text button. This function is called internally by gx_text_button_draw, and is provided as a separate API as a convenience for applications that define a custom button drawing function. Applications that want to customize the button background drawing can provide their custom drawing function, and invoke the gx_text_button_text_draw service as part of their custom drawing to draw the button text over the background.

### Parameters

- *text_button*: Pointer to text button control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Define a custom drawing function. */

VOID my_button_draw(GX_TEXT_BUTTON *button)
{
    /* Insert code here to draw button background. */

    /* Call support function to do text drawing. */
    gx_text_button_text_draw(button);

    /* Draw child widgets. */
    gx_widget_children_draw((GX_WIDGET *) button);
}
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_icon_button_create, x_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_text_button_create
- gx_text_button_draw
- gx_text_button_event_process
- gx_text_button_font_set
- gx_text_button_text_color_set
- gx_text_button_text_set
- gx_text_button_text_id_set

## gx_text_button_text_get

Get text from the text button (deprecated)

### Prototype

```C
UINT gx_text_button_text_get(
    GX_TEXT_BUTTON *text_button,
    GX_CHAR **return_text);
```

### Description

This service is deprecated in favor of gx_text_button_text_get_ext().

This service retrieves the specified string from the text button.

### Parameters

- *text_button*: Pointer to text button control block.
- *return_text*: Pointer to the string retrieved from the text button.

### Return Values

- **GX_SUCCESS** (0x00) Successfully get the text from the button.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_CHAR *string;

/* Get the string from the text button "my_text_button". */
status = gx_text_button_text_get(&my_text_button, &string);

/* If status is GX_SUCCESS, the string pointer from
"my_text_button" is retrieved and stored in string. */
```

### See Also

- gx_text_button_text_get_ext

## gx_text_button_text_get_ext

Get text from the text button

### Prototype

```C
UINT gx_text_button_text_get_ext(
    GX_TEXT_BUTTON *text_button,
    GX_STRING *return_string);
```

### Description

This service retrieves the specified string from the text button.

### Parameters

- *text_button*: Pointer to text button control block.
- *return_string*: Pointer to the string retrieved from the text button.

### Return Values

- **GX_SUCCESS** (0x00) Successfully get the text from the button.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_STRING string;

/* Get the string from the text button "my_text_button". */
status = gx_text_button_text_get_ext(&my_text_button, &string);

/* If status is GX_SUCCESS, the string pointer and length from
"my_text_button" is retrieved and stored in string. */
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_icon_button_create, x_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_text_button_create
- gx_text_button_draw
- gx_text_button_event_process
- gx_text_button_font_set
- gx_text_button_text_color_set
- gx_text_button_text_set
- gx_text_button_text_id_set

## gx_text_button_text_id_set

Set text resource ID to the text button

### Prototype

```C
UINT gx_text_button_text_id_set(
    GX_TEXT_BUTTON *text_button,
    RESOURCE_ID string_id);
```

### Description

This service sets the specified string resource ID to the text button.

### Parameters

- *text_button*: Pointer to text button control block.
- *string_id*: Resource ID of the string.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set the string resource ID to the text button.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set the string ID "MY_STRING_ID" to the text button
"my_text_button". */

status = gx_text_button_text_id_set(&my_text_button,
    MY_STRING_ID);

/* If status is GX_SUCCESS, the string ID MY_STRING_ID was set to
"my_text_button". */
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_icon_button_create, x_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_text_button_create
- gx_text_button_draw
- gx_text_button_event_process
- gx_text_button_font_set
- gx_text_button_text_color_set
- gx_text_button_text_get

## gx_text_button_text_set

Assign text to the text button (deprecated)

### Prototype

```C
UINT gx_text_button_text_set(
    GX_TEXT_BUTTON *text_button,
    GX_CHAR *text);
```

### Description

This service is deprecated in favor of gx_text_button_text_set_ext().

This service assigns the specified string to the text button. If the text_button widget was created with style  GX_STYLE_TEXT_COPY, the widget creates a private copy of the text string assigned. If GX_STYLE_TEXT_COPY is not active, the widget does not make a private copy of the incoming string, and therefore the string must be statically or globally allocated, i.e. it may not be an automatic or temporary variable.

### Parameters

- *text_button*: Pointer to text button control block.
- *text*: pointer to the NULL-terminated string.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set the text to the button.
- **GX_SYSTEM_MEMORY_ERROR** (0x30) Memory allocator not defined or memory allocation failed.
- **GX_INVALID_STRING_LENGTH** (0x34) Invalid string length.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set the string "my string" to the text button "my_text_button".
*/

status = gx_text_button_text_set(&my_text_button, "my string");

/* If status is GX_SUCCESS, the string "my_text_button" was set.
*/
```

### See Also

- gx_text_button_event_process
- gx_text_button_text_set_ext
- gx_text_button_text_id_set

## gx_text_button_text_set_ext

Assign text to the text button

### Prototype

```C
UINT gx_text_button_text_set_ext(
    GX_TEXT_BUTTON *text_button,
    GX_STRING *string);
```

### Description

This service assigns the specified string to the text button. If the text_button widget was created with style GX_STYLE_TEXT_COPY, the widget creates a private copy of the text string assigned. If GX_STYLE_TEXT_COPY is not active, the widget does not make a private copy of the incoming string, and therefore the string must be statically or globally allocated, i.e. it may not be an automatic or temporary variable.

### Parameters

- *text_button*: Pointer to text button control block.
- *string*: pointer to the GX_STRING variable.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set the text to the button.
- **GX_SYSTEM_MEMORY_ERROR** (0x30) Memory allocator not defined or memory allocation failed.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_STRING_LENGTH (0x34) Invalid string length.

### Allowed From

Initialization and threads

### Example

```C
GX_STRING new_string; new_string.gx_string_ptr = "Monday";

new_string.gx_string_length = strlen(new_string.gx_string_ptr);

/* Assign the string "new_string" to the text button
"my_text_button". */

status = gx_text_button_text_set_ext(&my_text_button, &new_string);

/* If status is GX_SUCCESS, the string "my_text_button" was set.
*/
```

### See Also

- gx_button_background_draw
- gx_button_create
- gx_button_deselect
- gx_button_draw
- gx_button_event_process
- gx_button_select
- gx_icon_button_create, x_pixelmap_button_create
- gx_pixelmap_button_draw
- gx_text_button_create
- gx_text_button_draw
- gx_text_button_event_process
- gx_text_button_font_set
- gx_text_button_text_color_set
- gx_text_button_text_get
- gx_text_button_text_id_set

## gx_text_input_cursor_blink_interval_set

Set cursor blink interval

### Prototype

```C
UINT gx_text_input_cursor_blink_interval_set(
    GX_TEXT_INPUT_CURSOR *cursor_input,
    GX_UBYTE blink_interval);
```

### Description

This service sets blink interval value of the cursor.

### Parameters

- *cursor_input*: Cursor control block.
- *blink_interval*: Value to be set.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set the cursor blink interval.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_VALUE (0x22) Blink interval value not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_TEXT_INPUT_CURSOR *input_cursor;

/* Pointer the input cursor to the cursor instance of single/multi
line text input widget. */

input_cursor = &sl_input.gx_single_line_text_input_cursor_instance;

/* Set the blink interval value of "input_cursor" to 2. */
status = gx_text_input_cursor_blink_interval_set(input_cursor, 2);

/* If status is GX_SUCCESS, the blink interval value of
"input_cursor" has been successfully set to 2. */
```

### See Also

- gx_text_input_cursor_height_set
- gx_text_input_cursor_width_set

## gx_text_input_cursor_height_set

Set cursor height

### Prototype

```C
UINT gx_text_input_cursor_height_set(
    GX_TEXT_INPUT_CURSOR *cursor_input,
    GX_UBYTE height);
```

### Description

This service sets height of the cursor.

### Parameters

- *cursor_input*: Cursor control block.
- *height*: Value to be set.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set cursor height.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_VALUE (0x22) Height value not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_TEXT_INPUT_CURSOR *input_cursor;

/* Pointer the input cursor to the cursor instance of single/multi
line text input widget. */

input_cursor = &sl_input.gx_single_line_text_input_cursor_instance;

/* Set height value of "input_cursor". */
status = gx_text_input_cursor_height_set(&input_cursor, 15);

/* If status is GX_SUCCESS, the height value of "input_curosr" has
been successfully set to 15. */
```

### See Also

- gx_text_input_cursor_blink_interval_set
- gx_text_input_cursor_width_set

## gx_text_input_cursor_width_set

Set cursor width

### Prototype

```C
UINT gx_text_input_cursor_blink_width_set(
    GX_TEXT_INPUT_CURSOR *cursor_input,
    GX_UBYTE *width);
```

### Description

This service sets width of the cursor.

### Parameters

- *cursor_input*: Cursor control block.
- *width*: Value to be set.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set the cursor width.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_VALUE (0x22) Width value not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_TEXT_INPUT_CURSOR *input_cursor;

/* Pointer the input cursor to the cursor instance of single/multi
line text input widget. */

input_cursor = &sl_input.gx_single_line_text_input_cursor_instance;

/* Set width of "input_curosr" to 2. */

status = gx_text_input_cursor_blink_width_set(&input_cursor, 2);

/* If status is GX_SUCCESS, the width of "input_cursor" has been
successfully set to 2. */
```

### See Also

- gx_text_input_cursor_blink_interval_set
- gx_text_input_cursor_height_set

## gx_text_scroll_wheel_callback_set

Assign the callback function of text type scroll wheel (deprecated)

### Prototype

```C
UINT gx_text_scroll_wheel_callback_set(
    GX_TEXT_SCROLL_WHEEL *wheel,
    GX_CONST GX_CHAR *(*callback)(GX_TEXT_SCROLL_WHEEL *, int));
```

### Description

This service is deprecated in favor of gx_text_scroll_wheel_callback_set_ext().

This service assigns the callback function which a text type scroll wheel will invoke to determine the text string to be displayed at each row of the scroll wheel.

For GX_NUMERIC_SCROLL_WHEEL and GX_STRING_SCROLL_WHEEL, default callback functions are provided and the application does not need to make any changes to use these default implementations.

This API is provided to allow the application to customize the formatting or other parameters of the string that is displayed on each row of the scroll wheel widget.

The callback function will receive as input a pointer to the scroll wheel control block and the row number that is being displayed. The function should return a pointer to a text string.

### Parameters

- *wheel*: String scroll wheel control block address.
- *callback*: Pointer to callback function.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set callback.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_TEXT_SCROLL_WHEEL wheel;

GX_CHAR string_buffer[20];

GX_CHAR *my_wheel_callback(GX_TEXT_SCROLL_WHEEL *wheel, int row)
{
    /* Just for an example, return row number as string for rows
    >= 0, and return text "Invalid" otherwise. */
    if (row >= 0)
    {
        gx_utility_ltoa(row, string_buffer, 20);
    }
    else
    {
        return("Invalid");
    }
}

gx_text_scroll_wheel_create(&wheel, "my wheel", root, 10,
    GX_STYLE_ENABLED|GX_STYLE_TEXT_CENTER|GX_STYLE_TRANSPARENT|
    GX_STYLE_WRAP|ID_MY_WHEEL, &size);

status = gx_text_scroll_wheel_callback_set(&wheel,
    my_wheel_callback);

/* If status is GX_SUCCESS, the scroll whell callback function has
been set. */
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_total_rows_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_event_process
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set

## gx_text_scroll_wheel_callback_set_ext

Assign the callback function of text type scroll wheel

### Prototype

```C
UINT gx_text_scroll_wheel_callback_set_ext(
    GX_TEXT_SCROLL_WHEEL *wheel,
    UINT *(*callback)(GX_TEXT_SCROLL_WHEEL *, int, GX_STRING *));
```

### Description

This service assigns the callback function which a text type scroll wheel will invoke to determine the text string to be displayed at each row of the scroll wheel.

For GX_NUMERIC_SCROLL_WHEEL and GX_STRING_SCROLL_WHEEL, default callback functions are provided and the application does not need to make any changes to use these default implementations.

This API is provided to allow the application to customize the formatting or other parameters of the string that is displayed on each row of the scroll wheel widget.

The callback function will receive as input a pointer to the scroll wheel control block and the row number that is being displayed. The function should return a pointer to a text string.

### Parameters

- *wheel*: String scroll wheel control block address.
- *callback*: Pointer to callback function.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set callback.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_TEXT_SCROLL_WHEEL wheel;

GX_CHAR string_buffer[20];

UINT *my_wheel_callback(GX_TEXT_SCROLL_WHEEL *wheel, int row,
    GX_STRING *return_string)
{
    /* Just for an example, return row number as string for rows
    >= 0, and return text "Invalid" otherwise. */
    if (row >= 0)
    {
        gx_utility_ltoa(row, string_buffer, 20);
        return_string->gx_string_ptr = string_buffer;
        return_string->gx_string_length = strlen(string_buffer);
    }
    else
    {
        return_string->gx_string_ptr = "Invalid";
        return_string->gx_string_length = strlen("Invalid");
    }

    return GX_SUCCESS;
}

gx_text_scroll_wheel_create(&wheel, "my wheel", root, 10,
    GX_STYLE_ENABLED|GX_STYLE_TEXT_CENTER|GX_STYLE_TRANSPARENT|
    GX_STYLE_WRAP|ID_MY_WHEEL, &size);

status = gx_text_scroll_wheel_callback_set_ext(&wheel,
    my_wheel_callback);

/* If status is GX_SUCCESS, the scroll whell callback function has
been set. */
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_total_rows_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_event_process
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set

## gx_text_scroll_wheel_create

Create a text scroll wheel

### Prototype

```C
UINT gx_text_scroll_wheel_create(
    GX_TEXT_SCROLL_WHEEL *wheel,
    GX_CONST GX_CHAR *name, 
    GX_WIDGET *parent, 
    INT total_rows,
    ULONG style, 
    USHORT Id, 
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a text scroll wheel. The text scroll wheel is a base widget for the GX_STRING_SCROLL_WHEEL and GX_NUMERIC_SCROLL_WHEEL type widgets. This function is called internally by gx_string_scroll_wheel_create and gx_numeric_scroll_wheel_create, and is provided as a separate API as a convenience for applications that define a custom scroll wheel widget.

### Parameters

- *wheel*: Text scroll wheel control block address.
- *name*: Application defined widget name.
- *parent*: Wheel parent or GX_NULL.
- *total_rows*: Total rows to be presented to user.
- *style*: Desired style flags.
- *Id*: Application defined wheel style flags.
- *size*: Initial scroll wheel size.

### Return Values

- **GX_SUCCESS** (0x00) Successfully created text scroll wheel.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_WIDGET (0x12) Widget not valid.


### Allowed From

Initialization and threads

### Example

```C
/* Define a custom scroll wheel widget. */
typedef MY_SCROLL_WHEEL_STRUCT
{
    GX_TEXT_SCROLL_WHEEL text_scroll_wheel;

    /* Add custom members here. */

} MY_SCROLL_WHEEL;

MY_SCROLL_WHEEL my_scroll_wheel;

UINT my_scroll_wheel_create(MY_SCROLL_WHEEL *wheel,
    GX_CONST GX_CHAR *name, GX_WIDGET *parent,
    INT total_rows, ULONG style, USHORT Id,
    GX_CONST GX_RECTANGLE *size)
{
    /* Call base creation. */
    status = gx_text_scroll_wheel_create(
        &wheel.text_scroll_wheel,
        "my_text_scroll_wheel", GX_NULL, 7,
        GX_STYLE_ENABLED, ID_MY_SCROLL_WHEEL, &size);

    if (status == GX_SUCCESS)
    {
        /* Add custom initialization here. */
        if(parent)
        {
            gx_widget_link(parent, (GX_WIDGET *)wheel);
        }
    }
}
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_total_rows_set
- gx_string_scroll_wheel_string_id_list_set
- gx_string_scroll_wheel_string_list_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_event_process
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set

## gx_text_scroll_wheel_draw

Draw a text scroll wheel

### Prototype

```C
VOID gx_text_scroll_wheel_draw(GX_TEXT_SCROLL_WHEEL *wheel);
```

### Description

This is the default drawing function for all wheel types based on GX_TEXT_SCROLL_WHEEL. This function can be overridden by applications that require customization of the text scroll wheel drawing appearance.

GX_STRING_SCROLL_WHEEL and GX_NUMERIC_SCROLL_WHEEL are both based on or derived from GX_TEXT_SCROLL_WHEEL.

### Parameters

- *wheel*: String scroll wheel control block address.

### Return Values

- None

### Allowed From

Initialization and threads

### Example

```C
/* Write a custom wheel draw function. */
UINT my_wheel_draw(GX_TEXT_SCROLL_WHEEL *wheel)
{
    /* Perform default drawing. */
    gx_text_scroll_wheel_draw(wheel);

    /* Add custom drawing here. */
}
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_total_rows_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_event_process
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set

## gx_text_scroll_wheel_event_process


Process text scroll wheel event

### Prototype

```C
UINT gx_text_scroll_wheel_event_process(GX_TEXT_SCROLL_WHEEL *wheel, GX_EVENT *event_ptr);
```

### Description

This service processes an event for the specified text scroll wheel. This service should be called as the default event handler by any custom text scroll wheel event processing functions.

### Parameters

- *wheel*: Pointer to text scroll wheel control block.
- *event_ptr*: Pointer to the event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful text scroll wheel event process.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Call generic text scroll wheel event processing as part of custom event processing function. */

UINT custom_text_scroll_wheel_event_process(GX_TEXT_SCROLL_WHEEL *wheel, GX_EVENT *event)
{
    UINT status = GX_SUCCESS;

    switch(event->gx_event_type)
    {
    case xyz:
        /* Insert custom event handling here. */
        break;

    default:
        /* Pass all other events to the default text scroll wheel event processing. */
        status = gx_text_scroll_wheel_event_process(wheel, event);
        break;
    }
    return status;
}
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_total_rows_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_font_set
- gx_text_scroll_wheel_text_color_set

## gx_text_scroll_wheel_font_set

Assign fonts used to draw scroll wheel rows

### Prototype

```C
UINT gx_text_scroll_font_set(
    GX_TEXT_SCROLL_WHEEL *wheel,
    GX_RESOURCE_ID normal_font, 
    GX_RESOURCE_ID selected_font);
```

### Description

Assign the fonts use to draw the text of a text scroll wheel based widget.

### Parameters

- *wheel*: String scroll wheel control block address.

### Return Values

- **GX_SUCCESS** (0x00) Successfully assigned wheel font.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_NUMERIC_SCROLL_WHEEL wheel;

status = gx_text_scroll_wheel_font_set(&wheel,
    GX_FONT_ID_WHEEL_NORMAL,
    GX_FONT_ID_WHEEL_SELECTED);

/* If status is GX_SUCCESS, the scroll wheel fonts have been assigned. */
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_total_rows_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_event_process
- gx_text_scroll_wheel_text_color_set

## gx_text_scroll_wheel_text_color_set

Assign colors used to draw scroll wheel rows

### Prototype

```C
UINT gx_text_scroll_wheel_text_color_set(
    GX_TEXT_SCROLL_WHEEL *wheel,
    GX_RESOURCE_ID normal_text_color,
    GX_RESOURCE_ID selected_text_color,
    GX_RESOURCe_ID disabled_text_color);
```

### Description

This function assigns the text colors used to draw a text based scroll wheel rows.

### Parameters

- *wheel*: String scroll wheel control block address.
- *normal_text_color*: Color used to draw non-selected rows.
- *selected_text_color*: Color used to draw selected row.
- *disabled_text_color*: Color used to draw text for disabled widget.

### Return Values

- **GX_SUCCESS** (0x00) Successfully assigned scroll wheel text color.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_STRING_SCROLL_WHEEL wheel;

UINT status = gx_text_scroll_wheel_text_color_set(&wheel,
    GX_COLOR_ID_NORMAL_TEXT,
    GX_COLOR_ID_SELECTED_TEXT,
    GX_COLOR_ID_DISABLED_TEXT);

/* If status is GX_SUCCESS, the colors used to draw the wheel text have been assigned. */
```

### See Also

- gx_numeric_scroll_wheel_create
- gx_numeric_scroll_wheel_range_set
- gx_scroll_wheel_create
- gx_scroll_wheel_event_process
- gx_scroll_wheel_gradient_alpha_set
- gx_scroll_wheel_row_height_set
- gx_scroll_wheel_selected_background_set
- gx_scroll_wheel_selected_get
- gx_scroll_wheel_selected_set
- gx_scroll_wheel_total_rows_set
- gx_text_scroll_wheel_callback_set
- gx_text_scroll_wheel_create
- gx_text_scroll_wheel_draw
- gx_text_scroll_wheel_event_process
- gx_text_scroll_wheel_font_set

## gx_tree_view_create

Create a tree view

### Prototype

```C
UINT gx_tree_view_create(
    GX_TREE_VIEW *tree,
    GX_CONST GX_CHAR *name, 
    GX_WIDGET *parent,
    ULONG style, 
    USHORT tree_view_id,
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a tree view as specified and associates the tree view with the supplied parent widget. It accepts all types of widget as child menu item. It's recommended to use GX_MENU type widget as its child menu item.

GX_TREE_VIEW is derived from GX_WINDOW and supports all gx_window API services.

### Parameters

- *tree*: Pointer to tree view control block.
- *name*: Name of the tree view.
- *parent*: Pointer to parent widget.
- *style*: Style of the widget. **Appendix D** contains pre-defined general styles for all widgets as well as widget specific styles.
- *menu_id*: Application-defined ID of the tree view.
- *size*: Size of the tree view.

### Return Values

- **GX_SUCCESS** (0x00) Successful tree view creation.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
status = gx_tree_view_create(&my_tree_view,
    "my_tree_view", parent,
    GX_STYLE_ENABLED, MY_TREE_VIEW_ID,
    &size);

/* If status is GX_SUCCESS the tree view was successfully created. */
```

### See Also

- gx_menu_draw
- gx_menu_insert
- gx_menu_remove
- gx_menu_text_draw
- gx_menu_text_offset_set
- gx_tree_view_draw
- gx_tree_view_event_process
- gx_tree_view_indentation_set
- gx_tree_view_position
- gx_tree_view_root_line_color_set
- gx_tree_view_root_pixelmap_set
- gx_tree_view_selected_get
- gx_tree_view_selected_set

## gx_tree_view_draw

Draw tree view

### Prototype

```C
VOID gx_tree_view_draw(GX_TREE_VIEW *tree);
```

### Description

This service draws the specified tree view. This function is normally called internally by the GUIX canvas refresh mechanism, but is exposed to the application to assist with implementing custom drawing functions for custom tree view widgets.

### Parameters

- *tree*: Pointer to tree view control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom tree view draw function. */
UINT my_tree_view_draw(GX_TREE_VIEW *tree_view)
{
    /* Perform default drawing. */
    gx_tree_view_draw(tree_view);

    /* Add custom drawing here. */
}
```

### See Also

- gx_menu_draw
- gx_menu_insert
- gx_menu_remove
- gx_menu_text_draw
- gx_menu_text_offset_set
- gx_tree_view_create
- gx_tree_view_event_process
- gx_tree_view_indentation_set
- gx_tree_view_position
- gx_tree_view_root_line_color_set
- gx_tree_view_root_pixelmap_set
- gx_tree_view_selected_get
- gx_tree_view_selected_set

## gx_tree_view_event_process

Process tree view event

### Prototype

```C
UINT gx_tree_view_event_process(
    GX_TREE_VIEW *tree, 
    GX_EVENT event_ptr);
```

### Description

This service processes an event for the specified tree view. This service should be called as the default event handler by any custom tree view event processing functions.

### Parameters

- *tree*: Pointer to tree view control block.
- *event_ptr*: Pointer to the event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful process tree view event.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Call generic tree view event processing as part of custom event
    processing function. */

UINT custom_tree_view_event_process(GX_TREE_VIEW *tree_view,
    GX_EVENT *event)
{
    UINT status = GX_SUCCESS;

    switch(event->gx_event_type)
    {
        case xyz:
            /* Insert custom event handling here. */
            break;
        default:
            /* Pass all other events to the default tree view event processing. */
            status = gx_tree_view_event_process(tree_view, event);
            break;
    }
}
return status;
```

### See Also

- gx_menu_draw
- gx_menu_insert
- gx_menu_remove
- gx_menu_text_draw
- gx_menu_text_offset_set
- gx_tree_view_create
- gx_tree_view_draw
- gx_tree_view_indentation_set
- gx_tree_view_position
- gx_tree_view_root_line_color_set
- gx_tree_view_root_pixelmap_set
- gx_tree_view_selected_get
- gx_tree_view_selected_set

## gx_tree_view_indentation_set

Set tree view indentation

### Prototype

```C
UINT gx_tree_view_indentation_set(
    GX_TREE_VIEW *tree,
    GX_VALUE indentation);
```

### Description

This service sets indentation for the tree view.

### Parameters

- *tree*: Pointer to tree view control block.
- *indentation*: Indentation to set.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set tree view indentation.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set tree view "my_tree" indentation to 10. */
status = gx_tree_view_indentation_set(&my_tree, 10);

/* If status is GX_SUCCESS the indentation of tree view "my_tree"
has been set to 10. */
```

### See Also

- gx_menu_draw
- gx_menu_insert
- gx_menu_remove
- gx_menu_text_draw
- gx_menu_text_offset_set
- gx_tree_view_create
- gx_tree_view_draw
- gx_tree_view_event_process
- tree_view_position
- gx_tree_view_root_line_color_set
- gx_tree_view_root_pixemlap_set
- gx_tree_view_selected_get
- gx_tree_view_selected_set

## gx_tree_view_position

Position tree view items

### Prototype

```C
UINT gx_tree_view_position(GX_TREE_VIEW *tree);
```

### Description

This service positions tree view items.

### Parameters

- *tree*: Pointer to tree view control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully positioned tree view items.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Position tree view "my_tree" items. */
status = gx_tree_view_position(&my_tree);

/* If status is GX_SUCCESS the items of tree view "my_tree" has
been positioned. */
```

### See Also

- gx_menu_draw
- gx_menu_insert
- gx_menu_remove
- gx_menu_text_draw
- gx_menu_text_offset_set
- gx_tree_view_create
- gx_tree_view_draw
- gx_tree_view_event_process
- gx_tree_view_indentation_set
- gx_tree_view_root_line_color_set
- gx_tree_view_root_pixelmap_set
- gx_tree_view_selected_get
- gx_tree_view_selected_set

## gx_tree_view_root_line_color_set

Set tree view root line color

### Prototype

```C
UINT gx_tree_view_root_line_color_set(
    GX_TREE_VIEW *tree,
    GX_RESOURCE_ID color_id);
```

### Description

This service assigns root line color for the tree view.

### Parameters

- *tree*: Pointer to tree view control block.
- *color_id*: Resource ID of root line color.

### Return Values

- **GX_SUCCESS** (0x00) Successful set root line color.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set root line color for tree view "my_tree". */

status = gx_tree_view_root_line_color_set(&my_tree,
    MY_ROOT LINE_COLOR_ID);

/* If status is GX_SUCCESS the root line color of the tree view
"my_tree" has been set. */
```

### See Also

- gx_menu_draw
- gx_menu_insert
- gx_menu_remove
- gx_menu_text_draw
- gx_menu_text_offset_set
- gx_tree_view_create
- gx_tree_view_draw
- gx_tree_view_event_process
- gx_tree_view_indentation_set
- gx_tree_view_position
- gx_tree_view_root_pixelmap_set
- gx_tree_view_selected_get
- gx_tree_view_selected_set

## gx_tree_view_root_pixelmap_set

Set tree view root pixelmap

### Prototype

```C
UINT gx_tree_view_root_pixelmap_set(
    GX_TREE_VIEW *tree,
    GX_RESOURCE_ID expand_map_id,
    GX_RESOURCE_ID collapse_map_id);
```

### Description

This service assigns expand and collapse pixelmap for the tree view.

### Parameters

- *tree*: Pointer to tree view control block.
- *expand_map_id*: Resource ID of expand pixelmap.
- *collapse_map_id*: Resource ID of collapse pixelmap.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set root pixelmap.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set root pixelmaps for tree view "my_tree". */

status = gx_tree_view_root_pixelmap_set(&my_tree,
    MY_EXPAND_MAP_ID,
    MY_COLLAPSE_MAP_ID);

/* If status is GX_SUCCESS the root pixelmaps of tree view
"my_tree" has been set. */
```

### See Also

- gx_menu_draw
- gx_menu_insert
- gx_menu_remove
- gx_menu_text_draw
- gx_menu_text_offset_set
- gx_tree_view_create
- gx_tree_view_draw
- gx_tree_view_event_process
- gx_tree_view_indentation_set
- gx_tree_view_position
- gx_tree_view_selected_get
- gx_tree_view_selected_set

## gx_tree_view_selected_get

Get selected item

### Prototype

```C
UINT gx_tree_view_selected_get(
    GX_TREE_VIEW *tree,
    GX_WIDGET **selected);
```

### Description

This service retrieves current selected item of the tree view.

### Parameters

- *tree*: Pointer to tree view control block.
- *selected*: Pointer to selected widget pointer.

### Return Values

- **GX_SUCCESS** (0x00) Successfully retrieved selected item.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Retrieve selected item of tree view "my_tree". */

GX_WIDGET *selected;

status = gx_tree_view_selected_get(&my_tree, &selected);

/* If status is GX_SUCCESS the selected item of tree view "my_tree"
has been retrieved. */
```

### See Also

- gx_menu_draw
- gx_menu_insert
- gx_menu_remove
- gx_menu_text_draw
- gx_menu_text_offset_set
- gx_tree_view_create
- gx_tree_view_draw
- gx_tree_view_event_process
- gx_tree_view_indentation_set
- gx_tree_view_position
- gx_tree_view_root_line_color_set
- gx_tree_view_root_pixelmap_set
- gx_tree_view_selected_set

## gx_tree_view_selected_set

Set selected item

### Prototype

```C
UINT gx_tree_view_selected_set(
    GX_TREE_VIEW *tree,
    GX_WIDGET *selected);
```

### Description

This service sets selected item for the tree view.

### Parameters

- *tree*: Pointer to tree view control block.
- *selected*: Pointer to the new selected item.

### Return Values

- **GX_SUCCESS** (0x00) Successful draw menu.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set selected item of tree view "my_tree" to "tree_view_item'. */
status = gx_tree_view_selected_set(&my_tree, &tree_view_item);

/* If status is GX_SUCCESS selected item of tree view "my_menu" has
been set to "tree_view_item". */
```

### See Also

- gx_menu_draw
- gx_menu_insert
- gx_menu_remove
- gx_menu_text_draw
- gx_menu_text_offset_set
- gx_tree_view_create
- gx_tree_view_draw
- gx_tree_view_event_process
- gx_tree_view_indentation_set
- gx_tree_view_position
- gx_tree_view_root_line_color_set
- gx_tree_view_root_pixelmap_set
- gx_tree_view_selected_get

## gx_utility_canvas_to_bmp

Convert canvas memory to bitmap

### Prototype

```C
UINT gx_utility_canvas_to_bmp(
    GX_CANVAS *canvas, 
    GX_RECTANGLE *rect,
    UINT (*write_data)(GX_UBYTE *byte_data, UINT data_count));
```

### Description

This service converts canvas memory to bitmap file.

### Parameters

- *canvas*: Canvas control block pointer.
- *rect*: Rectangle to convert.
- *write_data*: Callback function pointer to write data to.

### Return Values

- **GX_SUCCESS** (0x00) Successfully converted integer value to string.
- GX_PTR_ERROR (0x07) Invalid return buffer pointer.
- GX_INVALID_SIZE (0x19) Invalid return buffer size.

### Allowed From

Initialization and threads

### Example

```C
FILE *fp = GX_NULL;

/* Define call back function of how to write the data read from
canvas memory. */

UINT write_data_callback(GX_UBYTE *byte_data, UINT data_count)
{
    if (fp)
    {
        fwrite(byte_data, 1, data_count, fp);
    }
    return GX_SUCCESS;
}

VOID scroll_wheel_screen_draw(GX_WINDOW *window)
{
    UINT status;

    GX_RECTANGLE size = {31,31,610,450};
    gx_window_draw(window);
    if (screenshot)
    {
        fp = fopen("../screenshot.bmp", "wb");
        /* Convert canvas memory to bitmap format.
           Status GX_SUCCESS means operation succeed. */
        status = gx_utility_canvas_to_bmp(root->gx_window_root_canvas,
            &size, write_data_callback);

        fclose(fp);
    }
}
```

### See Also

- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift

## gx_utility_circle_point_get

Calculate point on a circle

### Prototype

```C
INT gx_utility_circle_point_get(
    INT xCenter, 
    INT yCenter,
    UINT radius, 
    INT angle, 
    GX_POINT *point);
```

### Description

This service calculates point on a circle given the circle center, radius, and angle.

### Parameters

- *xCenter*: x coordinate of circle center.
- *yCenter*: y coordinate of circle center.
- *radius*: Circle radius.
- *angle*: Angle at which to calculate circle perimeter point, in degrees.
- *point*: Address of GX_POINT variable at which to store calculated x,y coordinate.
- *end_alpha*: End alpha value.

### Return Values

- **GX_SUCCESS** (0x00) Gradient was created.
- GX_PTR_ERROR (0x07) Invalid return point address.
- GX_INVALID_VALUE (0x22) Invalid radius parameter.

### Allowed From

Initialization and threads

### Example

```C
GX_POINT point;
    UINT status;

        status = gx_utility_circle_point_get(100, 100,
20, 45,
&point);

/* If status is GX_SUCCESS the value of point is the x,y coordinate of a point on the circle perimeter at an angle of 45 degrees, where the circle is centered at (100, 100), has a radius of 20 pixels. */

```

### See Also

- gx_utility_ltoa
- gx_utility_math_asin
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift

## gx_utility_gradient_create

Create a gradient pixelmap

### Prototype

```C
INT gx_utility_gradient_create(
    GX_GRADIENT *gradient,
    GX_VALUE width, 
    GX_VALUE height,
    UCHAR type, 
    GX_UBYTE start_alpha, 
    GX_UBYTE end_alpha);
```

### Description

This service creates a gradient pixelmap at runtime. A gradient image can be used to accomplish fade effects and other interesting visual changes.

The width and height of the requested gradient can be no less than 2x2 pixels.

GUIX internally maintains a list of created gradients, and this function will first search the gradient list to find a matching gradient pixelmap before creating a new pixelmap. In other words,  the same gradient pixelmap is needed multiple times, only one pixelmap is actually created, and each gradient that requires this pixelmap shares the created pixelmap.

This API requires the gx_system_memory_allocator function be defined to allow runtime memory allocation.

The gradient type flags include GX_GRADIENT_TYPE_ALPHA and GX_GRADIENT_TYPE_MIRROR. Only  GX_GRADIENT_TYPE_ALPHA type gradients are currently supported (i.e. this type flag must be set). The  GX_GRADIENT_TYPE_MORROR flag is optional, and when set  instructs the gradient creation logic to create a gradient that changes from start_alpha to end_alpha and back to start_alpha. Otherwise a linear gradient is created.

### Parameters

- *gradient*: Pointer to gradient control block structure.
- *width*: Requested pixelmap width.
- *height*: Requested pixelmap height.
- *type*: Requested gradient type.
- *start_alpha*: Starting alpha value.
- *end_alpha*: End alpha value.

### Return Values

- **GX_SUCCESS** (0x00) Gradient was created.
- **GX_INVALID_SIZE** (0x19) Gradient is not at least 2x2 pixels.
- **GX_INVALID_VALUE** (0x22) Width and height value not valid.
- **GX_INVALID_TYPE** (0x1B) Gradient type not valid.
- **GX_NOT_SUPPORTED** (0x28) Gradient is not type GX_GRADIENT_TYPE_ALPHA.
- **GX_FAILURE** (0x10) Memory allocator is not defined or memory allocation is failed.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Gradient pointer not valid<.

### Allowed From

Initialization and threads

### Example

```C
GX_GRADIENT gradient;

UINT status;

status = gx_utiity_gradient_create(&gradient, 3, 40,
    GX_GRADIENT_TYPE_ALPHA, 240, 0);

/* If status == GX_SUCCESS the gradient pixelmap has been created. */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_asin
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift

## gx_utility_gradient_delete

Delete a previously created gradient

### Prototype

```C
INT gx_utility_gradient_delete(GX_GRADIENT *gradient);
```

### Description

This service deletes a previously created gradient. If the pixelmap associated with this gradient is not in use by any other gradients, the pixelmap data will also be deleted.

### Parameters

- *gradient*: Pointer to gradient control block.

### Return Values

- **GX_SUCCESS** (0x00) Gradient was deleted.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Gradient pointer is not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_GRADIENT gradient;

UINT status;

/* Delete previously created gradient. */
status = gx_utility_gradient_delete(&gradient);

/* If status == GX_SUCCESS, the gradient has been deleted. */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_asin
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift

## gx_utility_ltoa

Convert long integer to ASCII

### Prototype

```C
UINT gx_utility_itoa(
    LONG value, 
    GX_CHAR *return_buffer,
    UINT seturn_buffer_size);
```

### Description

This service converts a long integer value into an ASCII string.

### Parameters

- *value*: Long integer value to convert.
- *return_buffer*: Destination buffer for ASCII string.
- *return_buffer_size*: Size of destination buffer.

### Return Values

- **GX_SUCCESS** (0x00) Successfully converted integer value to string.
- GX_PTR_ERROR (0x07) Invalid return buffer pointer.
- GX_INVALID_SIZE (0x19) Invalid return buffer size.

### Allowed From

All

### Example

```C
INT my_value = 200;

GX_CHAR string_buffer[10];

UINT status;

/* Convert "my_value" into an ASCII string. */
status = gx_utility_ltoa(my_value, string_buffer, 10);

/* If status is GX_SUCCESS, "string_buffer" contains the ASCII
representation of "my_value". */
```

### See Also

- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift

## gx_utility_math_acos

Compute arc cosine

### Prototype

```C
INT gx_utility_math_acos(GX_FIXED_VAL x);
```

### Description

This service computes the angle value of the arc cosine x.

The input value is a fixed point data type, call GX_FIXED_VAL_MAKE to convert from INT to GX_FIXED_VAL type. For example, if you want to calculate the arc cosine of 0.5, make the input as GX_FIXED_VAL_MAKE(1) / 2.

In 5.4.0 or lesser version GUIX, the input value type of this function is INT, and the value is limited to the range [-256, 256]. The application must scale the value from range [-1, 1] to range [-256, 256] before invoke this service. If your project with GUIX version equal or lesser than 5.4.0 has reference to this API, and you want to upgrade your project with the latest guix library. You have two options.

1. Fix the input value of this API call to use GX_FIXED_VAL data type value.
1. Define GUIX_5_4_0_COMPATIBILITY.

### Parameters

- *x*: Value whose arc cosine is computed.

### Return Values

- **angle**: Angle value of arc cosine x.

### Allowed From

All

### Example

```C
/* Compute the angle value of arc cosine of "0.5". */

#if defined(GUIX_5_4_0_COMPATIBILITY)
    x = 256 / 2;
#else
    x = GX_FIXED_VAL_MAKE(1) / 2;
#endif

angle = gx_utility_math_acos(x);

/* "angle" contains the angle value of arc cosine "x". */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_asin
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift

## gx_utility_math_asin

Compute arc sine

### Prototype

```C
INT gx_utility_math_asin(GX_FIXED_VAL x);
```

### Description

This service computes the angle value of the arc sine x.

The input value is a fixed point data type, call  GX_FIXED_VAL_MAKE to convert from INT to GX_FIXED_VAL  type. For example, if you want to calculate the arc sin of 0.5, make the input as GX_FIXED_VAL_MAKE(1) / 2.

In 5.4.0 or lesser version GUIX, the input value type of this function is INT, and the value is limited to the range [-256, 256]. The application must scale the value from range [-1, 1] to range [-256, 256] before invoke this service. If your project with GUIX version equal or lesser than 5.4.0, and you want to upgrade your project with the latest guix library. You have two options.

1. Fix the input value of this API call to use GX_FIXED_VAL data type value.
1. Define GUIX_5_4_0_COMPATIBILITY.

### Parameters

- *x*: Value whose arc sine is computed.

### Return Values

- **angle**: Angle value of arc sine x.

### Allowed From

All

### Example

```C
/* Compute the angle value of arc sine of "x". */

#if defined GUIX_5_4_0_COMPATIBILITY
    x = 256 / 2;
#else
    X = GX_FIXED_VAL_MAKE(1) / 2;
#endif

angle = gx_utility_math_asin(x);

/* "angle" contains the angle value of arc sine "x". */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_acos
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift

## gx_utility_math_cos

Compute cosine

### Prototype

```C
GX_FIXED_VAL gx_utility_math_cos(GX_FIXED_VAL angle);
```

### Description

This service computes the cosine of the supplied angle.

The input value is a fixed point data type, call  GX_FIXED_VAL_MAKE to convert from INT to GX_FIXED_VAL. For example, if you want to calculate the cosine of 90 degrees, make input as GX_FIXED_VAL_MAKE(90).

The return value is a fixed point data type, call  GX_FIXED_VAL_TO_INT to convert from GX_FIXED_VAL to INT.

In 5.4.0 or lower version GUIX version, the input value and return value type of this service is INT, the input value and return value are enlarged by 256. And therefore, the application must scale the angle value by 256 before invoke this service. If your project with GUIX version equal or lesser than 5.4.0, and you want to upgrade your project with the latest guix library, you have two options.

1. Fix the input value and the handling to the return value of this API call to use GX_FIXED_VAL date type value.
1. Define GUIX_5_4_0_COMPATIBILITY.

### Parameters

- *angle*: Angle to compute cosine of.

### Return Values

- **cosine**: Cosine of supplied angle.

### Allowed From

All

### Example

```C
/* Compute cosine of 90 degree. */

INT angle = 90;

#if defined (GUIX_5_4_0_COMPATIBILITY)
    INT scaled_angle = angle << 8;
#else
    GX_FIXED_VAL scaled_angle = GX_FIXED_VAL_MAKE(angle);
#endif

my_angle_cosine = gx_utility_math_cos(scaled_angle);

/* "my_angle_cosine" contains the cosine of "my_angle". */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_acos
- gx_utility_math_asin
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift

## gx_utility_math_sin

Compute sine

### Prototype

```C
GX_FIXED_VAL gx_utility_math_sin(GX_FIXED_VAL angle);
```

### Description

This service computes the sine of the supplied angle.

The input value is a fixed point data type, call GX_FIXED_VAL_MAKE to convert from INT to GX_FIXED_VAL. For example, if you want to calculate the sine of 90 degrees, make input as GX_FIXED_VAL_MAKE(90). 

The return value is a fixed point data type, call GX_FIXED_VAL_TO_INT to convert from GX_FIXED_VAL to INT.

In 5.4.0 or lesser version GUIX, the input value and return value type is INT, the input value and return value are enlarged by 256. And therefore, the application must scale the angle value by 256 before invoke this service. If your project with GUIX version equal or lesser than 5.4.0, and you want to upgrade your project with the latest guix library, you have two options.

1. Fix the input value and the handing to the return value of this API call to use GX_FIXED_VAL data type value.
1. Define GUIX_5_4_0_COMPATIBILITY.

### Parameters

- *angle*: Angle to compute sine of.

### Return Values

- **sine**: Sine of supplied angle.

### Allowed From

All

### Example

```C
INT my_angle = 80;

/* Compute sine of "my_angle". */

#if defined(GUIX_5_4_0_COMPATIBILITY)
    INT scaled_angle = my_angle << 8;
#else
    GX_FIXED_VAL = GX_FIXED_VAL_MAKE(my_angle);
#endif

my_angle_sine = gx_utility_math_sin(scaled_angle);

/* "my_angle_sine" contains the sine of "my_angle". */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_acos
- gx_utility_asin
- gx_utility_math_cos
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift

## gx_utility_math_sqrt

Compute square root

### Prototype

```C
UINT gx_utility_math_sqrt(UINT value);
```

### Description

This service computes the square root of the supplied value.

### Parameters

- *value*: Value to compute square root of.

### Return Values

- **square root**: Square root of supplied value.

### Allowed From

All

### Example

```C
/* Compute square root of "my_value". */
my_square_root = gx_utility_math_sqrt(my_value);

/* "my_square_root" contains the square root of "my_value". */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift

## gx_utility_bidi_paragraph_reorder

Reorder BiDi text into its display order (Deprecated)

### Prototype

```C
UINT gx_utility_bidi_paragraph_reorder(GX_BIDI_TEXT_INFO *input_info, 
                                       GX_BIDI_RESOLVED_TEXT_INFO **resolved_info_head)
```

### Description

This service is deprecated. Please use gx_utility_bidi_text_reorder_ext instead, as it provides additional functionality of setting base direction.

This service reorders BiDi text into its display order. If text draw font and display width are provided,
line breaking will be applied first, reordering process will be applied based on each line. If the provided text contains more than one paragraph, the service will break the text into paragraphs, the line breaking and reordering result of each paragraph will be linked as a list.

### Parameters

- *input_info*: Pointer to the information for line breaking and text reordering.
- *resolved_info_head*: Pointer to the linked list that contains line breaking and reordering results of each paragraph of the provided text.

### Return Values

- **GX_SUCCESS** (0x00) Successful BiDi text reorder.
- **GX_SYSTEM_MEMORY_ERROR** (0x30) Memory allocator is not defined or memory allocation is failed.
- GX_PTR_ERROR (0x07) Invalid input pointers.

### Allowed From

All

### Example
#### Draw single line BiDi text to canvas
```C
VOID custom_widget_draw(GX_WIDGET *widget)
{
    GX_BIDI_TEXT_INFO input_info;
    GX_BIDI_RESOLVED_TEXT_INFO *resolved_info;
    GX_CHAR bidi_text[] = "Single Line String";
    
    input_info.gx_bidi_text_info_text.gx_string_ptr = bidi_text;
    input_info.gx_bidi_text_info_text.gx_string_length = sizeof(bidi_text) - 1;
    input_info.gx_bidi_text_info_font = GX_NULL;
    input_info.gx_bidi_text_info_display_width = -1;
    
    /* Reordering BiDi text. */
    status = gx_utility_bidi_paragraph_reorder(&input_info, &resolved_info);
    
    if(status == GX_SUCCESS)
    {
        /* Draw reordered BiDi text to canvas. */
        gx_canvas_text_draw_ext(widget->gx_widget_size.gx_rectangle_left,
                                widget->gx_widget_size.gx_rectangle_top,
                                resolved_info.gx_bidi_resolved_text_info_text);

        /* Delete resolved information list. */
        gx_utility_bidi_resolved_text_info_delete(&resolved_info);
    }
}
```
#### Draw multi line bidi text to canvas
```C
VOID custom_widget_draw(GX_WIDGET *widget)
{
    GX_BIDI_TEXT_INFO input_info;
    GX_BIDI_RESOLVED_TEXT_INFO *resolved_info;
    GX_CHAR bidi_text[] = "Multi Line BiDi Text Display Example";
    GX_FONT *font;
    GX_RECTANGLE client;
    GX_VALUE display_width;
    INT index;
    GX_VALUE xpos;
    GX_VALUE ypos;
    
    /* Pickup the font. */
    gx_context_font_get(font_id, &font);
    gx_widget_client_get(widget, 2, &client);
    
    display_width = (GX_VALUE)(client.gx_rectangle_right - client.gx_rectangle_left + 1);
    
    input_info.gx_bidi_text_info_text.gx_string_ptr = bidi_text;
    input_info.gx_bidi_text_info_text.gx_string_length = sizeof(bidi_text) - 1;
    input_info.gx_bidi_text_info_font = font;
    input_info.gx_bidi_text_info_display_width = display_width;
    
    /* Reordering BiDi text. */
    status = gx_utility_bidi_paragraph_reorder(&input_info, &resolved_info);
    
    if(status == GX_SUCCESS)
    {
        xpos = client.gx_rectangle_left;
        ypos = client.gx_rectangle_top;
        
        /* Draw reordered bidi text to canvas. */
        for(index = 0; index < resolved_info.gx_bidi_resolved_text_info_total_lines; index++)
        {
            gx_canvas_text_draw_ext(xpos, ypos, &resolved_info.gx_bidi_resolved_text_info_text[index]);
        
            ypos += font->gx_font_line_height;
        }
        
        /* Delete resolved information list. */
        gx_utility_bidi_resolved_text_info_delete(&resolved_info);
    }
}
```

#### Draw multi paragraph bidi text to canvas
```C
VOID custom_widget_draw(GX_WIDGET *widget)
{
    GX_BIDI_TEXT_INFO input_info;
    GX_BIDI_RESOLVED_TEXT_INFO *resolved_info;
    GX_BIDI_RESOLVED_TEXT_INFO *next;
    GX_CHAR bidi_text[] = "Multi Paragraph\r BiDi Text Display Example";
    GX_FONT *font;
    GX_RECTANGLE client;
    GX_VALUE display_width;
    INT index;
    GX_VALUE xpos;
    GX_VALUE ypos;
    
    /* Pickup the font. */
    gx_context_font_get(font_id, &font);
    gx_widget_client_get(widget, 2, &client);
    
    display_width = (GX_VALUE)(client.gx_rectangle_right - client.gx_rectangle_left + 1);
    
    
    input_info.gx_bidi_text_info_text.gx_string_ptr = bidi_text;
    input_info.gx_bidi_text_info_text.gx_string_length = sizeof(bidi_text) - 1;
    input_info.gx_bidi_text_info_font = font;
    input_info.gx_bidi_text_info_display_width = display_width;
    
    /* Reordering BiDi text. */
    status = gx_utility_bidi_paragraph_reorder(&input_info, &resolved_info);
    
    if(status == GX_SUCCESS)
    {
        xpos = client.gx_rectangle_left;
        ypos = client.gx_rectangle_top;
        
        next = resolved_info;
        
        /* Draw reordered bidi text to canvas. */
        while(next)
        {
            for(index = 0; index < next.gx_bidi_resolved_text_info_total_lines; index++)
            {
                gx_canvas_text_draw_ext(xpos, ypos, &next.gx_bidi_resolved_text_info_text[index]);
            
                ypos += font->gx_font_line_height;
            }
            
            next = next->gx_bidi_resolved_text_info_next;
        }
        
        /* Delete resolved information list. */
        gx_utility_bidi_resolved_text_info_delete(&resolved_info);
    }
}
```

### See Also

- gx_utility_bidi_resolved_text_info_delete

## gx_utility_bidi_paragraph_reorder_ext

Reorder BiDi text into its display order

### Prototype

```C
UINT gx_utility_bidi_paragraph_reorder_ext(
    GX_BIDI_TEXT_INFO *input_info, 
    GX_BIDI_RESOLVED_TEXT_INFO **resolved_info_head)
```

### Description

This service reorders BiDi text into its display order. If text draw font and display width are provided,
line breaking will be applied first, reordering process will be applied based on each line. If the provided text contains more than one paragraph, the service will break the text into paragraphs, the line breaking and reordering result of each paragraph will be linked as a list.

This service has the same functionality of the deprecated API `gx_utility_bidi_paragraph_reorder`, but it offers the additional capability of configuring base directions for the BiDi text. In cases where the base direction is not explicitly specified, it will be determined based on the first strong character within the paragraph. A strong character is defined as a character possessing distinct and explicit directionality.

The available base direction options are:
- GX_LANGUAGE_DIRECTION_RTL
- GX_LANGUAGE_DIRECTION_LTR


### Parameters

- *input_info*: Pointer to the information for line breaking and text reordering.
- *resolved_info_head*: Pointer to the linked list that contains line breaking and reordering results of each paragraph of the provided text.

### Return Values

- **GX_SUCCESS** (0x00) Successful BiDi text reorder.
- **GX_SYSTEM_MEMORY_ERROR** (0x30) Memory allocator is not defined or memory allocation is failed.
- GX_PTR_ERROR (0x07) Invalid input pointers.

### Allowed From

All

### Example
#### Draw single line BiDi text to canvas
```C
VOID custom_widget_draw(GX_WIDGET *widget)
{
    GX_BIDI_TEXT_INFO input_info;
    GX_BIDI_RESOLVED_TEXT_INFO *resolved_info;
    GX_CHAR bidi_text[] = "Single Line String";
    
    /* Set text for reordering. */
    input_info.gx_bidi_text_info_text.gx_string_ptr = bidi_text;
    input_info.gx_bidi_text_info_text.gx_string_length = sizeof(bidi_text) - 1;

    /* Set font when line breaking is required, otherwise set it to GX_NULL. */
    input_info.gx_bidi_text_info_font = GX_NULL;

    /* Set display width when line breaking is required, otherwise set it to -1. */
    input_info.gx_bidi_text_info_display_width = -1;

    /* Set language direction. */
    input_info.gx_bidi_text_info_direction = GX_LANGUAGE_DIRECTION_RTL;
    
    /* Reordering BiDi text. */
    status = gx_utility_bidi_paragraph_reorder(&input_info, &resolved_info);
    
    if(status == GX_SUCCESS)
    {
        /* Draw reordered BiDi text to canvas. */
        gx_canvas_text_draw_ext(widget->gx_widget_size.gx_rectangle_left,
                                widget->gx_widget_size.gx_rectangle_top,
                                resolved_info.gx_bidi_resolved_text_info_text);

        /* Delete resolved information list. */
        gx_utility_bidi_resolved_text_info_delete(&resolved_info);
    }
}
```
#### Draw multi line bidi text to canvas
```C
VOID custom_widget_draw(GX_WIDGET *widget)
{
    GX_BIDI_TEXT_INFO input_info;
    GX_BIDI_RESOLVED_TEXT_INFO *resolved_info;
    GX_CHAR bidi_text[] = "Multi Line BiDi Text Display Example";
    GX_FONT *font;
    GX_RECTANGLE client;
    GX_VALUE display_width;
    INT index;
    GX_VALUE xpos;
    GX_VALUE ypos;
    
    /* Pickup the font. */
    gx_context_font_get(font_id, &font);
    gx_widget_client_get(widget, 2, &client);
    
    display_width = (GX_VALUE)(client.gx_rectangle_right - client.gx_rectangle_left + 1);
    
    /* Set text for reordering. */
    input_info.gx_bidi_text_info_text.gx_string_ptr = bidi_text;
    input_info.gx_bidi_text_info_text.gx_string_length = sizeof(bidi_text) - 1;

    /* Set font for calculating character width. */
    input_info.gx_bidi_text_info_font = font;

    /* Set display width for line breaking. */
    input_info.gx_bidi_text_info_display_width = display_width;

    /* Set base direction. */
    input_info.gx_bidi_text_info_direction = GX_LANGUAGE_DIRECTION_RTL;
    
    /* Reordering BiDi text. */
    status = gx_utility_bidi_paragraph_reorder(&input_info, &resolved_info);
    
    if(status == GX_SUCCESS)
    {
        xpos = client.gx_rectangle_left;
        ypos = client.gx_rectangle_top;
        
        /* Draw reordered bidi text to canvas. */
        for(index = 0; index < resolved_info.gx_bidi_resolved_text_info_total_lines; index++)
        {
            gx_canvas_text_draw_ext(xpos, ypos, &resolved_info.gx_bidi_resolved_text_info_text[index]);
        
            ypos += font->gx_font_line_height;
        }
        
        /* Delete resolved information list. */
        gx_utility_bidi_resolved_text_info_delete(&resolved_info);
    }
}
```

#### Draw multi paragraph bidi text to canvas
```C
VOID custom_widget_draw(GX_WIDGET *widget)
{
    GX_BIDI_TEXT_INFO input_info;
    GX_BIDI_RESOLVED_TEXT_INFO *resolved_info;
    GX_BIDI_RESOLVED_TEXT_INFO *next;
    GX_CHAR bidi_text[] = "Multi Paragraph\r BiDi Text Display Example";
    GX_FONT *font;
    GX_RECTANGLE client;
    GX_VALUE display_width;
    INT index;
    GX_VALUE xpos;
    GX_VALUE ypos;
    
    /* Pickup the font. */
    gx_context_font_get(font_id, &font);
    gx_widget_client_get(widget, 2, &client);
    
    display_width = (GX_VALUE)(client.gx_rectangle_right - client.gx_rectangle_left + 1);
    
    /* Set text for reordering. */
    input_info.gx_bidi_text_info_text.gx_string_ptr = bidi_text;
    input_info.gx_bidi_text_info_text.gx_string_length = sizeof(bidi_text) - 1;

    /* Set font for calculating character width. */
    input_info.gx_bidi_text_info_font = font;

    /* Set display width for line breaking. */
    input_info.gx_bidi_text_info_display_width = display_width;

    /* Set base direction. */
    input_info.gx_bidi_text_info_direction = GX_LANGUAGE_DIRECTION_RTL;
    
    /* Reordering BiDi text. */
    status = gx_utility_bidi_paragraph_reorder(&input_info, &resolved_info);
    
    if(status == GX_SUCCESS)
    {
        xpos = client.gx_rectangle_left;
        ypos = client.gx_rectangle_top;
        
        next = resolved_info;
        
        /* Draw reordered bidi text to canvas. */
        while(next)
        {
            for(index = 0; index < next.gx_bidi_resolved_text_info_total_lines; index++)
            {
                gx_canvas_text_draw_ext(xpos, ypos, &next.gx_bidi_resolved_text_info_text[index]);
            
                ypos += font->gx_font_line_height;
            }
            
            /* Move to next paragraph. */
            next = next->gx_bidi_resolved_text_info_next;
        }
        
        /* Delete resolved information list. */
        gx_utility_bidi_resolved_text_info_delete(&resolved_info);
    }
}
```

### See Also

- gx_utility_bidi_resolved_text_info_delete

## gx_utility_bidi_resolved_text_info_delete

Delete resolved BiDi text info list

### Prototype

```C
UINT gx_utility_bidi_resolved_text_info_delete(GX_BIDI_RESOLVED_TEXT_INFO **resolved_info_head)
```

### Description

This service deletes resolved information list that returned by gx_utility_bidi_paragraph_reorder.

### Parameters

- *resolved_info_head*: Pointer to the resolved BiDi text information list.

### Return Values

- **GX_SUCCESS** (0x00) Successful resolved text delete.
- **GX_SYSTEM_MEMORY_ERROR** (0x30) Memory allocator is not defined or memory allocation is failed.
- GX_PTR_ERROR (0x07) Invalid input pointers.

### Allowed From

All

### Example
```C
VOID custom_widget_draw(GX_WIDGET *widget)
{
    GX_BIDI_TEXT_INFO input_info;
    GX_BIDI_RESOLVED_TEXT_INFO *resolved_info;
    GX_CHAR bidi_text[] = "Single Line String";
    
    input_info.gx_bidi_text_info_text.gx_string_ptr = bidi_text;
    input_info.gx_bidi_text_info_text.gx_string_length = sizeof(bidi_text) - 1;
    input_info.gx_bidi_text_info_font = GX_NULL;
    input_info.gx_bidi_text_info_display_width = -1;
    
    /* Reordering BiDi text. */
    status = gx_utility_bidi_paragraph_reorder(&input_info, &resolved_info);
    
    if(status == GX_SUCCESS)
    {
        /* Draw reordered bidi text to canvas. */
        gx_canvas_text_draw_ext(widget->gx_widget_size.gx_rectangle_left,
                                widget->gx_widget_size.gx_rectangle_top,
                                resolved_info.gx_bidi_resolved_text_info_text);

        /* Delete resolved information list. */
        gx_utility_bidi_resolved_text_info_delete(&resolved_info);
    }
}
```

### See Also

- gx_utility_bidi_paragraph_reorder

## gx_utility_pixelmap_resize

Resize pixelmap

### Prototype

```C
UINT gx_utility_pixelmap_resize(
    GX_PIXELMAP *src,
    GX_PIXELMAP *destination, 
    INT width, 
    INT height);
```

### Description

This service resizes a pixelmap and returns a pointer to a new pixelmap, which is the result of the pixelmap resize.

This service requires the prior use of gx_system_memory_allocator_set, to allow allocation of memory to hold the resized pixelmap data.

### Parameters

- *src*: Pointer to the pixelmap to resize.
- *destination*: Destination buffer for the resulting pixelmap.
- *width*: Width of the resulting pixelmap, in pixels.
- *height*: Height of the resulting pixelmap, in pixels.

### Return Values

- **GX_SUCCESS** (0x00) Successful pixelmap resize.
- GX_PTR_ERROR (0x07) Invalid source or destination pixelmap pointer.
- GX_INVALID_VALUE (0x22) Width or height value not valid.
- GX_NOT_SUPPORTED (0x28) Source pixelmap is compressed format.
- GX_SYSTEM_MEMORY_ERROR (0x30) Memory allocator is not defined or memory allocation is failed.

### Allowed From

All

### Example

```C
GX_PIXLEMAP *des_pixelmap;

/* Resize "src_pixelmap" with specifiy width and height. */
status = gx_utility_pixelmap_resize(src_pixelmap,
    &des_pixelmap, 100, 200);

/* If status is GX_SUCCESS. "des_pixelmap" successfully load the
resulting pixelmap of resize. */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift
- gx_canvas_pixelmap_rotate

## gx_utility_pixelmap_rotate

Rotate pixelmap

### Prototype

```C
UINT gx_utility_pixelmap_rotate(
    GX_PIXELMAP *src, INT angle,
    GX_PIXELMAP *destination,
    UINT *rot_cx, 
    UINT *rot_cy);
```

### Description

This service rotates a pixelmap and returns a pointer to a new pixelmap, which is the result of the pixelmap rotation. To rotate a pixelmap directly to the canvas, use gx_canvas_pixelmap_rotate().

This service requires the prior use of gx_system_memory_allocator_set, to allow allocation of memory to hold the rotated pixelmap data.

### Parameters

- *src*: The pixelmap to rotate.
- *angle*: Angle of rotation in degrees.
- *destination*: Destination buffer for the resulting pixelmap.
- *rot_cx*: Retrieved x coordinate of rotation center with respect to destination pixelmap. Should be initiated with the x coordinate of rotation center with respect to source pixelmap. If rot_cx is GX_NULL, value will not be retrieved.
- *rot_cy*: Retrieved y coordinate of rotation center with respect to destination pixelmap. Should be initiated with the y coordinate of rotation center with respect to source pixelmap. If rot_cy is GX_NULL, value will not be retrieved.

### Return Values

- **GX_SUCCESS** (0x00) Successful pixelmap rotate.
- **GX_INVALID_VALUE** (0x22) Angle value is 0.
- **GX_INVALID_FORMAT** (0x24) Source pixelmap is compressed format, which is not supported.
- **GX_SYSTEM_MEMORY_ERROR** (0x30) Memory allocator is not defined or memory allocation is failed.
- **GX_INVALID_WIDTH** (0x1E) Invalid pixelmap width.
- **GX_INVALID_HEIGHT** (0x1F) Invalid pixelmap height.
- GX_PTR_ERROR (0x07) Invalid source or destination pixelmap pointer.

### Allowed From

All

### Example

```C
rot_cx = source_rotate_center_x;
rot_cy = source_rotate_center_y;

/* Rotate "src_pixelmap" by 30 degree in clockwise direction. */
status = gx_utility_pixelmap_rotate(src_pixelmap, 30,
    &des_pixelmap, &rot_cx, &rot_cy);

/* If status is GX_SUCCESS. "des_pixelmap" successfully load the
resulting pixelmap of rotation. */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift
- gx_canvas_pixelmap_rotate

## gx_utility_pixelmap_simple_rotate

Rotate pixelmap

### Prototype

```C
UINT gx_utility_pixelmap_simple_rotate(
    GX_PIXELMAP *src,
    INT angle,
    GX_PIXELMAP *destination,
    UINT *rot_cx,
    UINT *rot_cy);
```

### Description

This service rotates a pixelmap by 90 degrees, 180 degrees or 270 degrees.

### Parameters

- *src*: The pixelmap to rotate.
- *angle*: Angle of rotation in degrees.
- *destination*: Destination buffer for the resulting pixelmap.
- *rot_cx*: Retrieved x coordinate of rotation center with respect to destination pixelmap. Should be initiated with the x coordinate of rotation center with respect to source pixelmap. If rot_cx is GX_NULL, value will not be retrieved.
*rot_cy*: Retrieved y coordinate of rotation center with respect to destination pixelmap. Should be initiated with the y coordinate of rotation center with respect to source pixelmap. If rot_cy is GX_NULL, value will not be retrieved.

### Return Values

- **GX_SUCCESS** (0x00) Successful pixelmap rotate.
- **GX_INVALID_VALUE** (0x22) Angle value is 0 or not a simple angle like 90, 180, 270.
- **GX_INVALID_FORMAT** (0x28) Source pixelmap is compressed format, which is not supported.
- **GX_SYSTEM_MEMORY_ERROR** (0x30) Memory allocator is not defined or memory allocation is failed.
- GX_PTR_ERROR (0x07) Invalid source or destination pixelmap pointer.

### Allowed From

All

### Example

```C
rot_cx = source_rotate_center_x;
rot_cy = source_rotate_center_y;

/* Rotate "src_pixelmap" by 90 degree in clockwise direction. */
status = gx_utility_pixelmap_simple_rotate(src_pixelmap, 90,
    &des_pixelmap,
    &rot_cx, &rot_cy);

/* If status is GX_SUCCESS. "des_pixelmap" successfully load the
resulting pixelmap of rotation. */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift

## gx_utility_rectangle_center

Center rectangle within another rectangle

### Prototype

```C
UINT gx_utility_rectangle_center(
    GX_RECTANGLE *rectangle,
    GX_RECTANGLE *within_rectangle);
```

### Description

This service centers the rectangle within another rectangle.

### Parameters

- *rectangle*: Rectangle to center.
- *within_rectangle*: Rectangle to center within.

### Return Values

- **GX_SUCCESS** (0x00) Successfully centered the rectangle.
- GX_PTR_ERROR (0x07) Invalid input rectangle pointer.
- GX_INVALID_SIZE (0x19) Invalid rectangle size.

### Allowed From

All

### Example

```C
UINT status;

/* Center "my_inner_rectangle" inside of "my_outer_rectangle".*/
status = gx_utility_rectangle_center(&my_inner_rectangle,
    &my_outer_rectangle);

/* Is status is GX_SUCCESS, "my_inner_rectangle" is centered
within "my_other_rectangle". */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift

## gx_utility_rectangle_center_find

Find center of rectangle

### Prototype

```C
UINT gx_utility_rectangle_center_find(
    GX_RECTANGLE *rectangle,
    GX_POINT *return_center);
```

### Description

This service finds the center of the rectangle.

### Parameters

- *rectangle*: Rectangle.
- *return_center*: Pointer to center point.

### Return Values

- **GX_SUCCESS** (0x00) Successfully found the center of the rectangle.
- GX_PTR_ERROR (0x07) Invalid input pointer.
- GX_INVALID_SIZE (0x19) Invalid rectangle size.

### Allowed From

Initialization and threads

### Example

```C
UINT status;

GX_RECTANGLE my_rectangle;

GX_POINT my_center_point;

gx_utility_define(&my_rectangle, 0, 0, 100, 100);

/* Find center of "my_rectangle"". */

status = gx_utility_rectangle_center_find(&my_rectangle,
    &my_center_point);

/* If status is GX_SUCCESS, "my_center_point" is the center point
of "my_rectangle" (50, 50). */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift

## gx_utility_rectangle_combine

Combine two rectangles into first

### Prototype

```C
UINT gx_utility_rectangle_combine(
    GX_RECTANGLE *first_rectangle,
    GX_RECTANGLE *second_rectangle);
```

### Description

This service combines the first and second rectangle into the first rectangle. The first rectangle is expanded to include the second.

### Parameters

- *first_rectangle*: First rectangle and combined rectangle.
- *second_rectangle*: Second rectangle.

### Return Values

- **GX_SUCCESS** (0x00) Successfully combined two rectangles.
- GX_PTR_ERROR (0x07) Invalid input pointer.

### Allowed From

Initialization and threads

### Example

```C
UINT status;

GX_RECTANGLE rect_a;

GX_RECTANGLE rect_b;

gx_utility_rectangle_define(&rect_a, 0, 0, 100, 100);
gx_utility_rectangle_define(&rect_b, 50, 50, 200, 200);

/* Combine "my_rectangle_a" to "my_rectangle_b". */
status = gx_utility_rectangle_combine(&rect_a, &rect_b);

/* If status is GX_SUCCESS, "rect_a" is (0, 0, 200, 200) the merger
of the original "rect_a" and "rect_b". */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift

## gx_utility_rectangle_compare

Compare two rectangles

### Prototype

```C
GX_BOOL gx_utility_rectangle_compare( 
    GX_RECTANGLE *first_rectangle,
    GX_RECTANGLE *second_rectangle);
```

### Description

This service compares the first and second rectangle. If they are equal, a value of GX_TRUE is returned.

### Parameters

- *first_rectangle*: First rectangle.
- *second_rectangle*: Second rectangle.

### Return Values

- **result**: GX_TRUE if rectangles are equal, otherwise GX_FALSE is returned.

### Allowed From

Initialization and threads

### Example

```C
/* Compare "my_rectangle_a" to "my_rectangle_b". */
result = gx_utility_rectangle_compare(&my_rectangle_a,
    &my_rectangle_b);

/* If result is GX_TRUE, the two rectangles are equal. */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift

## gx_utility_rectangle_define

Define a rectangle

### Prototype

```C
UINT gx_utility_rectangle_define(
    GX_RECTANGLE *rectangle,
    GX_VALUE left,
    GX_VALUE top, 
    GX_VALUE right, 
    GX_VALUE bottom);
```

### Description

This service defines a rectangle as specified.

### Parameters

- *rectangle*: Rectangle control block.
- *left*: Left most coordinate.
- *top*: Top most coordinate.
- *right*: Right most coordinate.
- *bottom*: Bottom most coordinate.

### Return Values

- **GX_SUCCESS** (0x00) Successfully defined a rectangle.
- GX_PTR_ERROR (0x07) Invalid rectangle pointer.

### Allowed From

All

### Example

```C
UINT status;

GX_RECTANGLE my_rect;

/* Define "my_rect". */

status = gx_utility_rectangle_define(&my_rect, 10, 5, 200, 100);

/* If status is GX_SUCCESS, "my_rect" is defined. */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift

## gx_utility_rectangle_overlap_detect

Detect overlap of rectangles

### Prototype

```C
GX_BOOL gx_utility_rectangle_overlap_detect(
    GX_RECTANGLE *first_rectangle,
    GX_RECTANGLE *second_rectangle,
    GX_RECTANGLE *return_overlap_area);
```

### Description

This service detects any overlap of the supplied rectangles. If overlap is found, the service returns GX_TRUE and the overlapping rectangle.

### Parameters

- *first_rectangle*: First rectangle.
- *second_rectangle*: Second rectangle.
- *return_overlap_area*: Overlapping rectangle area.

### Return Values

- **result**: GX_TRUE if rectangles overlap, otherwise GX_FALSE.

### Allowed From

All

### Example

```C
/* Detect overlap of "my_rectangle_a" and "my_rectangle_b". */
result = gx_utility_rectangle_overlap_detect(&my_rectangle_a,
    &my_rectangle_b,
    &my_overlap_area);

/* If result is GX_TRUE, "my_overlap_area" specifies the area the
rectangles overlap. */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift

## gx_utility_rectangle_point_detect

Detect if point resides in rectangle

### Prototype

```C
GX_BOOL gx_utility_rectangle_point_detect(
    GX_RECTANGLE *rectangle,
    GX_POINT point);
```

### Description

This service detects if the specified point resides in the rectangle. If the point does reside in the rectangle, the service returns  GX_TRUE.

### Parameters

- *rectangle*: Rectangle.
- *point*: Point.

### Return Values

- **result**: GX_TRUE if point resides in rectangle, otherwise GX_FALSE.

### Allowed From

All

### Example

```c
GX_RECTANGLE my_rectangle;

GX_POINT my_point;

gx_utility_rectangle_define(&my_rectangle, 0, 0, 100, 100);

my_point.gx_point_x = 20; my_point.gx_point_y = 20;

/* Detect if point "my_point" is within "my_rectangle"". */
result = gx_utility_rectangle_point_detect(&my_rectangle,
    &my_point);

/* If result is GX_TRUE, "my_point" resides in the rectangle. */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_shift

## gx_utility_rectangle_resize

Grow rectangle

### Prototype

```C
UINT gx_utility_rectangle_resize(
    GX_RECTANGLE *rectangle,
    GX_VALUE adjust);
```

### Description

This service increases the size of the rectangle as specified.

### Parameters

- *rectangle*: Pointer to rectangle.
- *adjust*: Amount to adjust the rectangle.

### Return Values

- **GX_SUCCESS** (0x00) Successfully resized the rectangle.
- GX_PTR_ERROR (0x07) Invalid input rectangle pointer.

### Allowed From

All

### Example

```C
UINT status;

/* Adjust "my_rectangle" by increasing 20 pixels on four sides. */
status = gx_utility_rectangle_resize(&my_rectangle, 20);

/* If status is GX_SUCCESS, "my_rectangle" is 20 pixels larger. */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect
- gx_utility_rectangle_shift

## gx_utility_rectangle_shift

Shift rectangle

### Prototype

```C
UINT gx_utility_rectangle_shift(
    GX_RECTANGLE *rectangle,
    GX_VALUE x_shift,
    GX_VALUE y_shift);
```

### Description

This service shifts the rectangle by the specified values.

### Parameters

- *rectangle*: Rectangle to shift.
- *x_shif*: Number of pixels to shift on the x-axis.
- *y_shift*: Number of pixels to shift on the y-axis.

### Return Values

- **GX_SUCCESS** (0x00) Successfully shifted the rectangle.
- GX_PTR_ERROR (0x07) Invalid input rectangle pointer.

### Allowed From

All

### Example

```C
UINT status;

/* Shift "my_rectangle". */

status = gx_utility_rectangle_shift(&my_rectangle, 10, 20);

/* If status is GX_SUCCESS, "my_rectangle" has been shifted. */
```

### See Also

- gx_utility_ltoa
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect

## gx_utility_string_to_alphamap

Render string to an 8bpp alphamap type pixelmap (deprecated)

### Prototype

```C
UINT gx_utility_string_to_alphamap(
    const GX_CHAR *text, const
    GX_FONT *font, 
    GX_PIXELMAP *return_map);
```

### Description

This service has been deprecated in favor of gx_utility_string_to_alphamap_ext().

This service renders a text string to an alphamap, which is a special form of 8bpp pixelmap containing only alpha values. This service is typically used along with gx_utility_pixelmap_rotate and gx_canvas_pixelmap_draw to draw rotated text to the canvas.

This service calculates the memory size needed for the resulting alphamap, and invokes the gx_system_memory_allocator() function defined by the application to dynamically allocate memory. The application must call gx_system_memory_allocator_set() at some point, usually during program startup, prior to using this service.

If a text string is to be rotated and drawn to the canvas just once, the service gx_canvas_rotated_text_draw() is provided as an alternate. gx_canvas_rotated_text_draw() will call gx_utility_string_to_alphamap(), gx_utility_pixelmap_rotate(), and gx_canvas_pixelmap_draw() to render the rotated text in one operation. However if the same text will be drawn multiple times rotated at various angles, it is more efficient to create the alphamap once using the gx_utility_string_to_alphmap API, then rotate the resulting alphamap multiple times as needed.

### Parameters

- *text*: Text string to render to alphamap.
- *font*: The font to be to render the text.
- *return_map*: Pointer to the GX_PIXELMAP to be returned to the caller.

### Return Values

- **GX_SUCCESS** (0x00) Successfully rendered a text string to an alphamap.
- **GX_INVALID_STRING_LENGTH** (0x34) Invalid string length.
- GX_PTR_ERROR (0x07) Invalid input pointer.
- GX_SYSTEM_MEMORY_ERROR (0x30) Memory allocation/free function is not defined.

### Allowed From

Initialization and threads

### Example

```C
GX_PIXELMAP alphamap;

GX_PIXELMAP rotated_text;

INT xpos;

INT ypos;

gx_widget_font_get(widget, GX_FONT_ID_SCREEN_LABEL, &font);

/* Render string to alphamap once. */

gx_utility_string_to_alphamap("Hello World", font, &alphamap);

/* Rotate and render the alphmap at multiple angles. */

gx_utility_pixelmap_rotate(&alphamap, 45, &rotated_text,
    &xpos, &ypos);

gx_canvas_pixelmap_draw(10, 10, &rotated_text);

gx_utility_pixelmap_rotate(&alphamap, 135, &rotated_text,
    &xpos, &ypos);

gx_canvas_pixelmap_draw(100, 100, &rotated_text);

gx_utility_pixelmap_rotate(&alphamap, 300, &rotated_text,
    &xpos, &ypos);

gx_canvas_pixelmap_draw(200, 200, &rotated_text);
```

### See Also

- gx_utility_string_to_alphamap_ext

## gx_utility_string_to_alphamap_ext

Render string to an 8bpp alphamap type pixelmap

### Prototype

```C
UINT gx_utility_string_to_alphamap_ext(
    GX_CONST GX_STRING *string,
    GX_CONST GX_FONT *font, 
    GX_PIXELMAP *return_map);
```

### Description

This service renders a text string to an alphamap, which is a special form of 8bpp pixelmap containing only alpha values. This service is typically used along with gx_utility_pixelmap_rotate and gx_canvas_pixelmap_draw to draw rotated text to the canvas.

This service calculates the memory size needed for the resulting alphamap, and invokes the gx_system_memory_allocator() function defined by the application to dynamically allocate memory. The application must call gx_system_memory_allocator_set() at some point, usually during program startup, prior to using this service.

If a text string is to be rotated and drawn to the canvas just once, the service gx_canvas_rotated_text_draw() is provided as an alternate. gx_canvas_rotated_text_draw() will call gx_utility_string_to_alphamap(), gx_utility_pixelmap_rotate(), and gx_canvas_pixelmap_draw() to render the rotated text in one operation. However if the same text will be drawn multiple times rotated at various angles, it is more efficient to create the alphamap once using the gx_utility_string_to_alphmap API, then rotate the resulting alphamap multiple times as needed.

### Parameters

- *string*: Text string to render to alphamap.
- *font*: The font to be to render the text.
- *return_map*: Pointer to the GX_PIXELMAP to be returned to the caller.

### Return Values

- **GX_SUCCESS** (0x00) Successfully rendered a text string to an alphamap.
- GX_PTR_ERROR (0x07) Invalid input pointer.
- GX_SYSTEM_MEMORY_ERROR (0x30) Memory allocation/free function is not defined.
- GX_INVALID_STRING_LENGTH (0x34) Invalid string length.

### Allowed From

Initialization and threads

### Example

```C
GX_STRING string;

GX_PIXELMAP alphamap;

GX_PIXELMAP rotated_text;

INT xpos;

INT ypos;

gx_widget_font_get(widget, GX_FONT_ID_SCREEN_LABEL, &font);

string.gx_string_ptr = "Hello World";

string.gx_string_length = strlen(string.gx_string_ptr);

/* Render string to alphamap once. */
gx_utility_string_to_alphamap_ext(&string, font, &alphamap);

/* Rotate and render the alphmap at multiple angles. */
gx_utility_pixelmap_rotate(&alphamap, 45, &rotated_text,
    &xpos, &ypos);

gx_canvas_pixelmap_draw(10, 10, &rotated_text);

gx_utility_pixelmap_rotate(&alphamap, 135, &rotated_text,
    &xpos, &ypos);

gx_canvas_pixelmap_draw(100, 100, &rotated_text);

gx_utility_pixelmap_rotate(&alphamap, 300, &rotated_text,
    &xpos, &ypos);

gx_canvas_pixelmap_draw(200, 200, &rotated_text);
```

### See Also

- gx_utility_ltoa
- gx_utility_math_cos
- gx_utility_math_sin
- gx_utility_math_sqrt
- gx_utility_pixelmap_rotate
- gx_utility_pixelmap_simple_rotate
- gx_utility_rectangle_center
- gx_utility_rectangle_center_find
- gx_utility_rectangle_combine
- gx_utility_rectangle_compare
- gx_utility_rectangle_define
- gx_utility_rectangle_grow
- gx_utility_rectangle_overlap_detect
- gx_utility_rectangle_point_detect

## gx_vertical_list_children_position

Position children for the vertical list

### Prototype

```C
UINT gx_vertical_list_children_position(
    GX_VERTICAL_LIST *vertical_list)
```

### Description

This function positions the children for the vertical list.

### Parameters

- *vertical_list*: Pointer to the vertical list control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully positioned the children for the vertical list.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Position children in the vertical list. */
status = gx_vertical_list_children_position (&vertical_list);

/* If status is GX_SUCCESS the children in the vertical list are positioned.. */
```

### See Also

- gx_vertical_list_create
- gx_vertical_list_event_process
- gx_vertical_list_page_index_set
- gx_vertical_list_selected_index_get
- gx_vertical_list_selecgted_widget_get
- gx_vertical_list_selected_widget_get
- gx_vertical_list_selected_set
- gx_vertical_list_total_rows_set

## gx_vertical_list_create

Create vertical list

### Prototype

```C
UINT gx_vertical_list_create(
    GX_VERTICAL_LIST *vertical_list,
    GX_CONST GX_CHAR *name, 
    GX_WIDGET *parent, 
    INT total_rows,
    VOID (*callback)(GX_VERTICAL_LIST *, GX_WIDGET *, INT),
    ULONG style, USHORT vertical_list_id,
    GX_CONST GX_RECTANGLE *size);
```
### Description

This service creates a vertical list.

GX_VERTICAL_LIST is derived from GX_WINDOW and supports all gx_window API services.

### Parameters

- *vertical_list*: Vertical list widget control block.
- *name*: Name of vertical list.
- *parent*: Pointer to parent widget.
- *total_rows*: Total number of rows in vertical list.
- *callback*: A function that will be called by the vertical list when the list is scrolled. The caller should initially create enough GX_WIDGET based children to fill the visible list rows. As the list is scrolled, this function is called to re-create the list children corresponding to the supplied list index.
- *style*: Style of scrollbar widget. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *vertical_list_id*: Application-defined ID of vertical list.
- *size*: Dimensions of vertical list.

### Return Values

- **GX_SUCCESS** (0x00) Successfully created the vertical list.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.
- GX_INVALID_VALUE (0x22) Number of rows not valid.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Create vertical list "my_list" with 20 rows. */
status = gx_vertical_list_create(&my_list, "my_list", &my_parent,
                    20, callback, GX_STYLE_WRAP, MY_LIST_ID,
                    &size);

/* If status is GX_SUCCESS the vertical list "my_list" has been created. */
```

### See Also

- gx_vertical_list_children_position
- gx_vertical_list_event_process
- gx_vertical_list_page_index_set
- gx_vertical_list_selected_index_get
- gx_vertical_list_selected_widget_get
- gx_vertical_list_selected_set
- gx_vertical_list_total_rows_set

## gx_vertical_list_event_process

Process vertical list event

### Prototype

```C
UINT gx_vertical_list_event_process(GX_VERTICAL_LIST *list,
                                                      GX_EVENT *event);
```

### Description

This service processes an event for the vertical list.

### Parameters

- *list*: Vertical list widget control block.
- *event*: Pointer to event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successfully processed the vertical list event.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Process "my_event" for vertical list "my_list". */
status = gx_vertical_list_event_process(&my_list, &my_event);

/* If status is GX_SUCCESS the event for vertical list "my_list" has been processed. */
```

### See Also

- gx_vertical_list_children_position
- gx_vertical_list_create
- gx_vertical_list_page_index_set
- gx_vertical_list_selected_index_get
- gx_vertical_list_selected_widget_get
- gx_vertical_list_selected_set
- gx_vertical_list_selected_set

## gx_vertical_list_page_index_set

Set starting page index

### Prototype

```C
UINT gx_vertical_list_page_index_set(
    GX_VERTICAL_LIST *list,
    INT index);
```
### Description

This service sets the starting index for the vertical list.

### Parameters

- *list*: Vertical list widget control block.
- *index*: The new top index.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set starting page index for the vertical list.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid widget pointer.
- GX_INVALID_VALUE (0x22) Invalid index value.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set the starting page index of vertical list "my_list" to 4. */
status = gx_vertical_list_page_index_set(&my_list, 4);

/* If status is GX_SUCCESS the starting page index of "my_list" has
been set to 4. */
```
### See Also

- gx_vertical_list_children_position
- gx_vertical_list_create
- gx_vertical_list_event_process
- gx_vertical_list_selected_index_get
- gx_vertical_list_selected_widget_get
- gx_vertical_list_selected_set
- gx_vertical_list_total_rows_set

## gx_vertical_list_selected_index_get

Get selected index from vertical list

### Prototype

```C
UINT gx_vertical_list_selected_index_get(
    GX_VERTICAL_LIST *vertical_list,
    INT *return_index);
```
### Description

This service returns the selected index of the vertical list

### Parameters

- *vertical_list*: Vertical list widget control block.
- *return_index*: Destination for return of selected index.

### Return Values

- **GX_SUCCESS** (0x00) Successfully get the vertical list entry.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
INT current_selected_index;

/* Get the list entry at the current index of vertical list "my_list". */
status = gx_vertical_list_selected_index_get(&my_list, &current_selected_index);

/* If status is GX_SUCCESS, "current_list_index" contains the index of the selected list item. */
```
### See Also

- gx_vertical_list_children_position
- gx_vertical_list_create
- gx_vertical_list_event_process
- gx_vertical_list_page_index_set
- gx_vertical_list_selected_widget_get
- gx_vertical_list_selected_set
- gx_vertical_list_total_rows_set

## gx_vertical_list_selected_set

Assign the selected entry in a vertical list

### Prototype

```C
UINT gx_vertical_list_selected_set(
    GX_VERTICAL_LIST *vertical_list,
    INT index);
```

### Description

This service assigns the selected entry in a vertical list. If necessary, the vertical list will scroll to make the selected entry visible.

### Parameters

- *vertical_list*: Vertical list widget control block.
- *index*: Index based position of new list entry.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set the vertical list entry.
- **GX_FAILURE** (0x10) Input index not found in list.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Vertical list or list entry widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set the list entry of "my_list" to the child in line 12. */
status = gx_vertical_list_selected_set(&my_list, 12);

/* If status is GX_SUCCESS, the list entry of "my_list" has been successfully set to 12. */
```
### See Also

- gx_vertical_list_children_position
- gx_vertical_list_create
- gx_vertical_list_event_process
- gx_vertical_list_page_index_get
- gx_vertical_list_selected_index_get
- gx_vertical_list_selected_widget_get
- gx_vertical_list_total_rows_set

## gx_vertical_list_selected_widget_get

Get selected widget from vertical list

### Prototype

```C
UINT gx_vertical_list_selected_widget_get(
    GX_VERTICAL_LIST *vertical_list,
    GX_WIDGET **return_list_entry);
```
### Description

This service returns the selected widget of the vertical list. Note that if the list contains more rows than child widgets, and the selected child widget has been scrolled from view, this function will return GX_NULL as the GX_WIDGET pointer, since the widget has been reused to display a new list entry.

### Parameters

- *vertical_list*: Vertical list widget control block.
- *return_list_entry*: Destination for return list entry widget.

### Return Values

- **GX_SUCCESS** (0x00) Successfully get the vertical list entry.
- **GX_FAILURE** (0x10) The selected widget has been scrolled from view.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_WIDGET *current_selected_widget;

/* Get the list entry at the current index of vertical list "my_list". */
status = gx_vertical_list_selected_widget_get(&my_list, &current_selected_widget);

/* If status is GX_SUCCESS, "current_list_entry" contains a pointer to the currently selected widget. */
```

### See Also

- gx_vertical_list_children_position
- gx_vertical_list_create
- gx_vertical_list_event_process
- gx_vertical_list_page_index_set
- gx_vertical_list_selected_index_get
- gx_vertical_list_selected_set
- gx_vertical_list_total_rows_set

## gx_vertical_list_total_rows_set

Set total number of vertical list rows

### Prototype

```C
UINT gx_vertical_list_total_rows_set(
    GX_VERTICAL_LIST *vertical_list,
    INT count);
```
### Description

This service assigns or changes the total number of list rows.

### Parameters

- *vertical_list*: Vertical list widget control block.
- *count*: New list row count.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set the vertical list row count.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_VALUE (0x22) Row count value not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set the list row count to 20 items. */
status = gx_vertical_list_total_rows_set(&my_list, 20);

/* If status is GX_SUCCESS, the total rows of "my_list" has been set to 20. */
```
### See Also

- gx_vertical_list_children_position
- gx_vertical_list_create
- gx_vertical_list_event_process
- gx_vertical_list_page_index_set
- gx_vertical_list_selected_index_get
- gx_vertical_list_selected_widget_get
- gx_vertical_list_selected_set

## gx_vertical_scrollbar_create

Create vertical scrollbar

### Prototype

```C
UINT gx_vertical_scrollbar_create(
    GX_SCROLLBAR *scrollbar,
    GX_CONST GX_CHAR *name, 
    GX_WINDOW *parent,
    GX_SCROLLBAR_APPEARANCE *appearance,
    ULONG style);
```
### Description

This service creates a vertical scrollbar.

### Parameters

- *scrollbar*: Scrollbar widget control block.
- *name*: Name of scrollbar.
- *parent*: Pointer to parent widget.
- *appearance*: Appearance of vertical scrollbar widget.
- *style*: Style of the scrollbar.

### Return Values

- **GX_SUCCESS** (0x00) Successful vertical scrollbar create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.
- GX_INVALID_WIDGET (0x12) Parent widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Create vertical scrollbar "my_scrollbar". */
status = gx_vertical_scrollbar_create(&my_scrollbar,
                          "my_vertical_scrollbar",
                          &my_parent, &scrollbar_appearance,
                          GX_STYLE_ENABLED);

/* If status is GX_SUCCESS the vertical scrollbar "my_scrollbar" has been created. */
```
### See Also

- gx_horizontal_scrollbar_create
- gx_scrollbar_draw
- gx_scrollbar_event_process
- gx_scrollbar_limit_check
- gx_scrollbar_reset

## gx_widget_allocate

Allocate a widget control block

### Prototype

```C
UINT gx_widget_allocate(
    GX_WIDGET **control_block,
    ULONG memsize);
```
### Description

This service dynamically allocates a widget control block, by calling the application defined memory allocation function. This service is primarily used by the functions generated by GUIX Studio to dynamically allocate control block when the "Dynamic Allocation" property is selected in the GUIX Studio properties view.

### Parameters

- *control_block*: Pointer to returned control block pointer.
- *memsize*: Control block size, in bytes.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget allocate.
- GX_SYSTEM_MEMORY_ERROR (0x30) Memory allocator is not defined or memory allocation failed.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_MEMORY_SIZE (0x29) Memory size not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_TEXT_BUTTON *button;

/* Attach "my_widget" to "my_parent". */
status = gx_widget_allocate(&button, sizeof(GX_TEXT_BUTTON));

/* If status is GX_SUCCESS the button widget control block is allocated. */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_create
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_attach

Attach widget to its parent

### Prototype

```C
UINT gx_widget_attach(
    GX_WIDGET *parent, 
    GX_WIDGET *widget);
```
### Description

This service attaches the widget to the specified parent. If the widget is already attached to another parent, it is first detached. If the widget is already attached to the same parent, the function does nothing.

The widget becomes the front-most child of its parent in terms of z ordering. If sibling widgets overlap, this widget is drawn on top of siblings. To put the new widget in the back of the z-order, use gx_widget_back_attach or gx_widget_back_move.

### Parameters

- *parent*: Pointer to parent widget.
- *widget*: Pointer to child widget.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget attach.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Parent or widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Attach "my_widget" to "my_parent". */
status = gx_widget_attach(&my_parent, &my_widget);

/* If status is GX_SUCCESS the widget "my_widget" is attached to "my_parent". */
```

### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_create
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_background_draw

Draw a widget background

### Prototype

```C
VOID gx_widget_background_draw(GX_WIDGET *widget);
```
### Description

This service performs a solid color fill of a widget background. This service is automatically called by the gx_widget_draw function, but may also be invoked by the application as part of a customized widget drawing function.

### Parameters

- *widget*: Pointer to widget to be drawn.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom widget draw function. */

VOID my_widget_draw(GX_WIDGET * widget)
{
    /* Call default widget background draw. */
    gx_widget_background_draw(widget);

    /* Add your own drawing here. */

    /* Draw child widgets. */
    gx_widget_children_draw(widget);
}
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_create
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_back_attach

Attach widget to its parent

### Prototype

```C
UINT gx_widget_back_attach(
    GX_WIDGET *parent, 
    GX_WIDGET *widget);
```
### Description

This service attaches the widget to the specified parent. If the widget is already attached to another parent, it is first detached. If the widget is already attached to the same parent, the function does nothing.

The widget becomes the back-most child of its parent in terms of z ordering. If sibling widgets overlap, this widget is drawn behind those siblings. To put the new widget in the front of the z-order, use gx_widget_attach or gx_widget_front_move.

### Parameters

- *parent*: Pointer to parent widget.
- *widget*: Pointer to child widget.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget attach.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Parent or widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Attach "my_widget" to "my_parent". */
status = gx_widget_back_attach(&my_parent, &my_widget);

/* If status is GX_SUCCESS the widget "my_widget" is attached to "my_parent". */
```
### See Also

- gx_widget_back_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_create
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_back_move

Move widget to back

### Prototype

```C
UINT gx_widget_back_move(
    GX_WIDGET *widget,
    GX_BOOL *return_widget_moved);
```
### Description

This service moves the widget to the back in the parent's Z-order of child widgets.

### Parameters

- *parent*: Pointer to parent widget.
- *return_widget_moved*: Pointer to destination for flag indicating the widget was moved.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget move to the back.
- **GX_NO_CHANGE** (0x08) No changes are applied.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Move "my_widget" to the back. */
status = gx_widget_back_move(&my_widget, &moved_flag);

/* If status is GX_SUCCESS and "moved_flag" is GX_TRUE, the widget "my_widget" was moved to the back. */
```
### See Also

- gx_widget_attach
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_create
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_block_move

Move a rectangular block of pixels

### Prototype

```C
UINT gx_widget_block_move(
    GX_WIDGET *widget,
    GX_RECTANGLE *block,
    INT xshift, INT yshift);
```

### Description

This service moves a rectangular block of pixels. This service is most often used to implement fast scrolling.

### Parameters

- *widget*: Pointer to widget requesting block move.
- *block*: Rectangle bounding block to move.
- *xshift*: The x shift amount in pixels.
- *yshift*: The y shift amount in pixels.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget move to the back.
- **GX_INVALID_CANVAS** (0x20) Widget canvas not found.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_VALUE (0x22) Invalid rectangle size.

### Allowed From

Initialization and threads

### Example

```C
/* Move a block of pixels 20 pixels to the right. */
status = gx_widget_block_move(&my_widget, &size, 20, 0);

/* If status is GX_SUCCESS the block of pixels was moved. */
```

### See Also

- gx_widget_attach
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_create
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_border_draw

Draw widget border

### Prototype

```C
VOID gx_widget_border_draw(
    GX_WIDGET *widget,
    GX_COLOR border_color,
    GX_COLOR upper_fill, 
    GX_COLOR lower_fill, 
    GX_BOOL fill);
```

### Description

This service draws the widget border. This service is normally invoked as part of a widget drawing function. This service interprets the widget border style flags to draw no border, a thin border, a raised border, a recessed border, or a thick border.

### Parameters

- *widget*: Pointer to widget.
- *border_color*: Color of border. **Appendix A** contains pre-defined colors. Note that the application may add custom colors as well.
- *upper_fill*: Color of upper fill. **Appendix A** contains pre-defined colors. Note that the application may add custom colors as well.
- *lower_fill*: Color of lower fill. **Appendix A** contains pre-defined colors. Note that the application may add custom colors as well.
- *fill*: This boolean flag indicates whether or not the widget area should be filled with the supplied fill colors. If this value is GX_FALSE, only the widget border is drawn.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom widget draw function. */

VOID my_widget_draw(GX_WIDGET * widget)
{
      /* Call widget border draw. */
      gx_widget_border_draw(widget, GX_COLOR_BLACK,
                            GX_COLOR_GREEN, GX_COLOR_BLUE,
                            GX_TRUE);

      /* Add your own drawing here. */

      /* Draw child widgets. */
      Gx_widget_children_draw(widget);
}
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_create
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_border_style_set

Set widget border style

### Prototype

```C
UINT gx_widget_border_style_set(
    GX_WIDGET *widget, 
    ULONG style);
```

### Description

This service sets the widget border style.

### Parameters

- *widget*: Pointer to widget.
- *style*: Style of border. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget border style set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set border style of "my_widget". */
status = gx_widget_border_style_set(&my_widget,
                                    GX_STYLE_BORDER_RAISED);

/* If status is GX_SUCCESS the widget "my_widget" border style has been set. */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_create
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_border_width_get

Get widget border width

### Prototype

```C
UINT gx_widget_border_width_get(
    GX_WIDGET *widget,
    GX_VALUE *return_width);
```

### Description

This service gets the widget border width.

### Parameters

- *widget*: Pointer to widget.
- *return_width*: Pointer to destination for widget border width.

### Return Values

- **GX_SUCCESS** (0x00) Successfully retrieved border width.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_VALUE my_width;

/* Get border width of "my_widget". */
status = gx_widget_border_width_get(&my_widget, &my_width);

/* If status is GX_SUCCESS, "my_width" contains the border width of the widget "my_widget". */
```

### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_create
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_canvas_get

Get widget canvas

### Prototype

```C
UINT gx_widget_canvas_get(
    GX_WIDGET *widget,
    GX_CANVAS **return_canvas)
```
### Description

This service returns a pointer to the canvas onto which this widget is rendered.

### Parameters

- *widget*: Pointer to widget.
- *return_canvas*: Pointer to destination for widget's canvas.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget canvas get.
- **GX_FAILURE** (0x10) Widget canvas not found.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Get canvas associated with "my_widget". */
status = gx_widget_canvas_get(&my_widget, &my_canvas);

/* If status is GX_SUCCESS, "my_canvas" contains the canvas of the widget "my_widget". */
```

### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_create
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_child_detect

Detect widget child

### Prototype

```C
UINT gx_widget_child_detect(
    GX_WIDGET *parent, 
    GX_WIDGET *child,
    GX_BOOL *return_detect);
```

### Description

This service detects if the widget is a child of the parent widget. This service nests to search children of children, and returns TRUE if the parent widget is at any level an ancestor of the child widget.

### Parameters

- *parent*: Pointer to parent widget.
- *child*: Pointer to child widget.
- *return_detect*: Pointer to destination for detection.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget child detection.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Parent or child widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_BOOL detected;

/* Determine if "my_child" is a child of "my_widget". */
status = gx_widget_child_detect(&my_widget, &my_child, &detected);

/* If status is GX_SUCCESS and "detected" is GX_TRUE, "my_child" is a child of widget "my_widget". */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_create
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_children_draw

Draw widget children

### Prototype

```C
VOID gx_widget_children_draw(GX_WIDGET *widget);
```

### Description

This service draws all children of the parent widget. This service is normally invoked by all standard widget drawing functions to draw any existing child widgets, and should be invoked by any custom drawing functions to allow child widgets to be attached to your custom parent widget type.

### Parameters

- *widget*: Pointer to widget.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom widget draw function. */

VOID my_widget_draw(GX_WIDGET * widget)
{
        /* Call default widget background draw. */
        gx_widget_background_draw(widget);

        /* Add your own drawing here. */

        /* Draw child widgets. */
        gx_widget_children_draw(widget);
}
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_client_get
- gx_widget_create
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_client_get

Get widget client area

### Prototype

```C
UINT gx_widget_client_get(
    GX_WIDGET *widget,
    GX_VALUE border_width,
    GX_RECTANGLE *return_client_area);
```
### Description

This service computes the client area of widget by subtracting the widget border width from the overall widget size.

### Parameters

- *widget*: Pointer to widget.
- *border_width*: Width of widget border.
- *return_client_area*: Destination for returning client area.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget client area get.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_VALUE (0x22) Widget border not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_RECTANGLE client_area
/* Get client area of widget "my_widget". */
status = gx_widget_client_get(&my_widget, my_widget_width,
                              &client_area);

/* If status is GX_SUCCESS, the "client_area" is the client area of "my_widget". */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_create
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_color_get

Get color

### Prototype

```C
UINT gx_widget_color_get(
    GX_WIDGET *widget,
    GX_RESOURCE_ID resource_id,
    GX_COLOR *return_color);
```

### Description

This service gets the color associated with the supplied resource ID. This service should only be called by visible widgets.

### Parameters

- *widget*: Pointer to widget control block.
- *resource_id*: Resource ID of color. **Appendix B** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.
- *return_color*: Pointer to destination for color. **Appendix A** contains pre-defined colors. Note that the application may add custom colors as well.

### Return Values

- **GX_SUCCESS** (0x00) Successful color get.
- **GX_INVALID_RESOURCE_ID** (0x33) Invalid resource ID.
- **GX_INVALID_CANVAS** (0x20) Widget canvas not valid or widget is invisible.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_COLOR actual_color;

/* Get color for resource ID MY_FIRST_COLOR_ID. */
status = gx_widget_color_get(my_widget, MY_FIRST_COLOR_RESOURCE_ID,
                            &actual_color);

/* If status is GX_SUCCESS the actual color is contained in "actual_color". */
```
### See Also

- gx_widget_font_get
- gx_widget_pixelmap_get

## gx_widget_create

Create widget

### Prototype

```C
UINT gx_widget_create(
    GX_WIDGET *widget, 
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent,
    ULONG style, 
    USHORT widget_id,
    GX_CONST GX_RECTANGLE *size);
```
### Description

This service creates a widget.

### Parameters

- *widget*: Pointer to widget.
- *name*: Logical name of widget.
- *parent*: Pointer to parent widget.
- *style*: Style. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *widget_id*: Application-defined ID of the widget.
- *size*: Size of the widget.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.
- GX_INVALID_WIDGET (0x12) Parent widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_WIDGET my_widget;
GX_RECTANGLE size;

gx_utility_rectangle_define(&size, 0, 0, 100, 100);

/* Get client area of widget "my_widget". */
status = gx_widget_create(&my_widget, "my widget",
                          &my_parent_window, GX_STYLE_BORDER_RAISED, MY_WIDGET_ID, &size);

/* If status is GX_SUCCESS, the widget "my_widget" has been created. */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_created_test

Test if widget created

### Prototype

```C
UINT gx_widget_created_test(
    GX_WIDGET *widget,
    GX_BOOL *return_test);
```
### Description

This service tests to determine if the widget has previously been created. If no errors are encountered, this function return GX_SUCCESS, regardless if the widget is created yet or not. The result of the test is in the return_test pointer.

### Parameters

- *widget*: Pointer to widget.
- *return_test*: Destination for test result.

### Return Values

- **GX_SUCCESS** (0x00) Successful test completion.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_BOOL was_created;

/* Test to see if widget "my_widget" is created. */
status = gx_widget_created_test(&my_widget, &was_created);

/* If status is GX_SUCCESS, no error occurred. If "was_created" is
GX_TRUE, the widget "my_widget" has been created. */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_delete

Delete widget

### Prototype

```C
UINT gx_widget_delete(GX_WIDGET *widget);
```

### Description

This service deletes the widget. If the widget control block is dynamically allocated, the gx_system_memory_free service is invoked to free dynamically allocated storage.

### Parameters

- *widget*: Pointer to widget.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget delete GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_SYSTEM_MEMORY_ERROR (0x30) Memory free function is not defined.

### Allowed From

Initialization and threads

### Example

```C
/* Delete widget "my_widget". */
status = gx_widget_delete(&my_widget);

/* If status is GX_SUCCESS the widget "my_widget" has been deleted. */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_detach

Detach widget from parent

### Prototype

```C
UINT gx_widget_detach(GX_WIDGET *widget);
```

### Description

This service detaches the widget from its parent.

### Parameters

- *widget*: Pointer to widget.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget detach GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Detach widget "my_widget" from its parent. */
status = gx_widget_detach(&my_widget);

/* If status is GX_SUCCESS the widget "my_widget" has been detached. */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_draw

Draw widget

### Prototype

```C
VOID gx_widget_draw(GX_WIDGET *widget);
```

### Description

This service draws the widget. This function is normally called internally by the GUIX canvas refresh mechanism, but is exposed to the application to assist with implementing custom drawing functions.

### Parameters

- *widget*: Pointer to widget.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom widget draw function. */

VOID my_custom_widget_draw(GX_WIDGET *widget)
{
      /* Call default widget draw. */
      gx_widget_draw(widget);

      /* Add your own drawing here. */
}
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_draw_set

Assign the widget drawing function

### Prototype

```C
UINT gx_widget_draw_set(
    GX_WIDGET *widget,
    VOID (*drawing_function) (GX_WIDGET *));
```

### Description

This service overrides the default drawing function of the widget.

### Parameters

- *widget*: Pointer to widget.
- *drawing_function*: Pointer to drawing function.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget drawing function override.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Define a custom drawing function. */
VOID my_drawing_function(GX_WIDGET *widget)
{
      /* Add your own drawing here. */
}

/* Set the drawing function of widget "my_widget" to "my_drawing_function". */
status = gx_widget_draw_set(&my_widget, my_drawing_function);

/* If status is GX_SUCCESS the widget "my_widget" has the drawning function "my_drawing_function". */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_event_generate

Generate widget event

### Prototype

```C
UINT gx_widget_event_generate(
    GX_WIDGET *widget, 
    USHORT event_type, LONG value);
```

### Description

This service generates a GX_SIGNAL type of event, which is a particular type or class of GX_EVENT. gx_widget_event_generate() encodes the 16 bit widget ID, along with the passed in event_type, into a single 32 bit GX_EVENT.gx_event_type value. The value parameter is encoded into the generated gx_event. gx_event_payload.gx_event_longdata field.

The generated event.gx_event_target field is always loaded with the calling widget's parent, meaning the generated event is always sent first to the parent of the generating widget.

Note that gx_widget_event_generate should only be used to send GX_SIGNAL range event types. For all  other event types, including user defined event types, use the gx_system_event_send() API, which grants full control over every field of the event pushed in the GUIX event queue.

### Parameters

- *widget*: Pointer to widget.
- *event_type*: Type of event. **Appendix E** contains pre-defined GUIX events. Additional events may be added by the application.
- *value*: Additional event information.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget event generation.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Generate a redraw event for widget "my_widget". */
status = gx_widget_event_generate(&my_widget, GX_EVENT_REDRAW, 0);

/* If status is GX_SUCCESS the redraw event for widget "my_widget" has been generated. */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get
- gx_system_event_send

## gx_widget_event_process

Process widget event

### Prototype

```C
UINT gx_widget_event_process(
    GX_WIDGET *widget, 
    GX_EVENT *event);
```

### Description

This is the default event processing function for all widgets. When a custom event processing function is written, the default action for any event type should always be to pass the event to the widget type upon which a widget is based. Widgets that are based on the most basic GX_WIDGET type pass use gx_widget_event_process as their default event processing function.

### Parameters

- *widget*: Pointer to widget.
- *event*: Pointer to event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget event processing.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Process event "my_event" for widget "my_widget". */
status = gx_widget_event_process(&my_widget, &my_event);

/* If status is GX_SUCCESS the event "my_event" for widget "my_widget" has been processed. */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_event_process_set

Set event processing function of widget

### Prototype

```C
UINT gx_widget_event_process_set(
    GX_WIDGET *widget,
    UINT (*event_processing) (GX_WIDGET *, GX_EVENT *));
```

### Description

This service overrides the event processing function of the widget.

### Parameters

- *widget*: Pointer to widget.
- *event_processing*: Pointer to new event processing function.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget event processing override.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
UINT my_event_process(GX_TREE_VIEW *tree_view,
                      GX_EVENT *event)
{
      UINT status = GX_SUCCESS;

      switch(event->gx_event_type)
      {
      case xyz:
            /* Insert custom event handling here. */
            break;

      default:
            /* Pass all other events to the default tree view event processing. */
            status = gx_tree_view_event_process(tree_view, event);
            break;
      }
      return status;
}

/* Use "my_event_process" to process events for widget "my_tree_view". */
status = gx_widget_event_process_set((GX_WIDGET *)&my_tree_view,
                    (VOID (*)(GX_WIDGET *))my_event_process);

/* If status is GX_SUCCESS all event processing for widget "my_tree_view" is handled by "my_event_process". */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_event_to_parent

Send event to widget's parent

### Prototype

```C
UINT gx_widget_event_to_parent(
    GX_WIDGET *widget,
    GX_EVENT *event);
```
### Description

This service sends an event to the widget's parent.

### Parameters

- *widget*: Pointer to widget.
- *event*: Pointer to the event.

### Return Values

- **GX_SUCCESS** (0x00) Successfully sent event to widget's parent.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Threads

### Example

```C
/* Send my_event to the widget's parent. */
status = gx_widget_event_to_parent(&my_widget, my_event);

/* If status is GX_SUCCESS the event has been delivered to the parent of my_widget. */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_fill_color_set

Set widget background color

### Prototype

```C
UINT gx_widget_fill_color_set(
    GX_WIDGET *widget,
    GX_RESOURCE_ID normal_color_id,
    GX_RESOURCE_ID selected_color_id,
    GX_RESOURCE_ID disabled_color_id);
```
### Description

This service sets the widget background colors.

### Parameters

- *widget*: Pointer to widget.
- *normal_color_id*: Resource ID of the fill color in normal state. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.
- *selected_color_id*: Resource ID of the fill color when the widget gain focus. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.
- *disabled_color_id*: Resource ID of the fill color when the style GX_STYLE_ENABLED is not set. **Appendix A** contains pre-defined color Resource IDs. Note that the application may add custom color Resource IDs as well.

### Return Values

- **GX_SUCCESS** (0x00) Successfully set widget fill color.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set background of "my_widget". */
status = gx_widget_fill_color_set(&my_widget,
                    GX_COLOR_ID_NORMAL_FILL,
                    GX_COLOR_ID_SELECTED_FILL,
                    GX_COLOR_ID_DISABLED_FILL);

/* If status is GX_SUCCESS the widget "my_widget" background has been set. */
```

### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_create
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_find

Find child widget of parent widget

### Prototype

```C
UINT gx_widget_find(
    GX_WIDGET *parent, 
    USHORT widget_id,
    INT search_depth, 
    GX_WIDGET **return_widget);
```
### Description

This service searches through the children of the specified parent looking for a widget with the  requested ID value.

### Parameters

- *parent*: Pointer to parent widget from which search is started.
- *widget_id*: Widget ID to search for.
- *search_depth*: Defines the recursive nesting level into which the function will search child widgets. If this value is <= 0, only immediate children of the parent widget are searched. If this value is GX_SEARCH_DEPTH_INFINITE, all children of all child widgets are exhaustively searched. For any other value > 0, this value limits how deeply nested this function will search through child widgets looked for the requested widget ID.
- *return_widget*: Pointer to destination for found widget.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget find.
- **GX_NOT_FOUND** (0x09) Widget not fount.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_WIDGET *widget_found;

/* Find widget "my_widget". */
status = gx_widget_find(&my_widget, GX_SEARCH_DEPTH_INFINITE
                        MY_WIDGET_ID, &widget_found);

/* If status is GX_SUCCESS, the pointer "widget_found" contains the pointer to the widget found. */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_first_child_get

Return pointer to first child widget

### Prototype

```C
UINT gx_widget_first_child_get(
    GX_WIDGET *parent,
    GX_WIDGET **widget_return);
```
### Description

GUIX maintains a tree structured list of parent and child widgets. This service returns a pointer to the first child widget of the parent.

### Parameters

- *parent*: Pointer to parent widget.
- *widget_return*: Pointer to return widget pointer.

### Return Values

- **GX_SUCCESS** (0x00) pointer returned.
- GX_PTR_ERROR (0x07) Invalid widget pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Threads

### Example

```C
/* Retrieve child widget pointer. */

GX_WIDGET *get_child_widget(GX_WIDGET *parent)
{
      GX_WIDGET *child;
      UINT status;

      status = gx_widget_first_child_get(parent, &child);
      if (status == GX_SUCCESS)
      {
            return child;
      }
      return GX_NULL;
}
```
### See Also

- gx_widget_last_child_get
- gx_widget_next_sibling_get
- gx_widget_parent_get
- gx_widget_previous_sibling_get
- gx_widget_top_visible_child_find

## gx_widget_focus_next

Move focus to next widget in navigation order

### Prototype

```C
UINT gx_widget_focus_next(GX_WIDGET *widget);
```

### Description

This service moves focus to the next sibling widget in the linked list of widgets that accept focus.

### Parameters

- *widget*: Pointer to widget control block.

### Return Values

- **GX_SUCCESS** (0x00) focus was moved.
- **GX_FAILURE** (0x00) focus was not moved.
- GX_PTR_ERROR (0x07) Invalid widget pointer.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Initialization and threads

### Example

```C
/* Move focus to next widget in navigation order. */
status = gx_widget_focus_next(&my_widget);

/* If status is GX_SUCCESS the focus has been moved to the next widget in the navigation order. */
```
### See Also

- gx_widget_focus_previous

## gx_widget_focus_previous

Move focus to previous widget in navigation order

### Prototype

```C
UINT gx_widget_focus_previous(GX_WIDGET *widget);
```

### Description

This service moves focus to the previous widget in the navigation order.

### Parameters

- *widget*: Pointer to widget that current has input focus.

### Return Values

- **GX_SUCCESS** (0x00) focus was moved.
- **GX_FAILURE** (0x00) focus was not moved.

### Allowed From

Initialization and threads

### Example

```C
/* Move focus to previuos widget in navigation order. */ status =
gx_widget_focus_previous(&my_widget);

/* If status is GX_SUCCESS the input focus has been moved to the
previous widget. */
```
### See Also

- gx_widget_focus_next

## gx_widget_font_get

Get font for specified resource ID

### Prototype

```C
UINT gx_widget_font_get(
    GX_WIDGETG *widget,
    GX_RESOURCE_ID resource_id,
    GX_FONT **return_font);
```
### Description

This service retrieves the font associated with the specified resource ID from the font table of the display on which this widget is visible. This function should only be called by a visible widget.

### Parameters

- *widget*: Pointer to widget control block.
- *resource_id*: Resource ID of font.
- *return_font*: Pointer to destination for font pointer.

### Return Values

- **GX_SUCCESS** (0x00) Successfully retrieved font.
- **GX_INVALID_RESOURCE_ID** (0x33) Invalid resource ID.
- **GX_INVALID_CANVAS** (0x20) Widget canvas not valid or widget is invisible.
- GX_PTR_ERROR (0x07) Invalid widget pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_FONT *my_font;
/* Get font for MY_FONT_ID. */
status = gx_widget_font_get(widget, MY_FONT_RESOURCE_ID, &my_font);
/* If status is GX_SUCCESS the font pointer has been retrieved in "my_font". */
```
### See Also

- gx_widget_color_get
- gx_widget_pixelmap_get

## gx_widget_free

Release memory associated with a widget

### Prototype

```C
UINT gx_widget_free(GX_WIDGETG *widget);
```

### Description

This service releases the memory associated with a widget control block.

### Parameters

- *widget*: Pointer to widget control block.
- *resource_id*: Resource ID of font.
- *return_font*: Pointer to destination for font pointer.

### Return Values

- **GX_SUCCESS** (0x00) Successfully freed widget.
- **GX_SYSTEM_MEMPRY_ERROR** (0x30) Memory free function is not defined.
- GX_PTR_ERROR (0x07) Invalid widget pointer.

### Allowed From

Initialization and threads

### Example

```C
GX_WIDGET widget;
UINT status;

status = gx_widget_allocate(&widget, sizeof(GX_WIDGET))

/* Free a runtime allocated widget. */
if (status == GX_SUCCESS)
{
      status = gx_widget_free(widget);
}

/* If status is GX_SUCCESS the memory that allocated to the widget has been released. */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_front_move

Move widget to front

### Prototype

```C
UINT gx_widget_front_move(
    GX_WIDGET *widget, 
    GX_BOOL *return_moved);
```

### Description

This service moves the widget to the front in the parent Z-order list of child widgets.

### Parameters

- *widget*: Pointer to widget to move.
- *return_moved*: Pointer to destination for indication widget was moved.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget move to front.
- **GX_NO_CHANGE** (0x08) Widget already in front.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_BOOL widget_moved;

/* Move widget "my_widget" to the front. */
status = gx_widget_front_move(&my_widget, &widget_moved);

/* If status is GX_SUCCESS and "widget_moved" is GX_TRUE, the
widget "my_widget" was moved to the front . */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_height_get

Get widget height

### Prototype

```C
UINT gx_widget_height_get(
    GX_WIDGET *widget,
    UINT *return_height);
```

### Description

This service gets the widget height.

### Parameters

- *widget*: Pointer to widget.
- *return_height*: Pointer to destination for widget height.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget height get.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_VALUE widget_height;

/* Get height for widget "my_widget". */
status = gx_widget_height_get(&my_widget, &widget_height);

/* If status is GX_SUCCESS the height of the widget is contained in
"widget_height" . */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_hide

Hide widget

### Prototype

```C
UINT gx_widget_hide(GX_WIDGET *widget);
```

### Description

This service hides the widget. This widget is still attached to it's parent, but it is not allowed to draw on the canvas.

### Parameters

- *widget*: Pointer to widget.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget hide.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```c
/* Hide widget "my_widget". */
status = gx_widget_hide(&my_widget);

/* If status is GX_SUCCESS the widget "my_widget" is hidden. */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_style_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_last_child_get

Return pointer to last child widget

### Prototype

```C
UINT gx_widget_last_child_get(
    GX_WIDGET *parent,
    GX_WIDGET **widget_return);
```
### Description

GUIX maintains a tree structured list of parent and child widgets. This service returns a pointer to the last child widget of the parent.

### Parameters

- *parent*: Pointer to parent widget.
- *widget_return*: Pointer to return widget pointer.

### Return Values

- **GX_SUCCESS** (0x00) pointer returned.
- GX_PTR_ERROR (0x07) Invalid widget pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Threads

### Example

```C
/* Retrieve child widget pointer. */

GX_WIDGET *get_last_child_widget(GX_WIDGET *parent)
{

      GX_WIDGET *child;
      UINT status;

      status = gx_widget_last_child_get(parent, &child);
      if (status == GX_SUCCESS)
      {
              return child;
      }
      return GX_NULL;
}
```
### See Also

- gx_widget_first_child_get
- gx_widget_next_sibling_get
- gx_widget_parent_get
- gx_widget_previous_sibling_get
- gx_widget_top_visible_child_find

## gx_widget_next_sibling_get

Return pointer to next sibling of current widget

### Prototype

```C
UINT gx_widget_next_sibling_get(
    GX_WIDGET *current,
    GX_WIDGET **widget_return);
```
### Description

GUIX maintains a tree structured list of parent and child widgets. This service returns a pointer to the next sibling of the current widget.

### Parameters

- *current*: Pointer to current widget.
- *widget_return*: Pointer to return widget pointer.

### Return Values

- **GX_SUCCESS** (0x00) pointer returned.
- GX_PTR_ERROR (0x07) Invalid widget pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Threads

### Example

```C
/* Retrieve next sibling widget pointer. */

GX_WIDGET *get_next(GX_WIDGET *current)
{
      GX_WIDGET *sibling;
      UINT status;

      status = gx_widget_next_sibling_get(current, &sibling);
      if (status == GX_SUCCESS)
      {
            return sibling;
      }
      return GX_NULL;
}
```
### See Also

- gx_widget_first_child_get
- gx_widget_last_child_get
- gx_widget_parent_get
- gx_widget_previous_sibling_get
- gx_widget_top_visible_child_find

## gx_widget_parent_get

Return pointer to parent of current widget

### Prototype

```C
UINT gx_widget_parent_get(
    GX_WIDGET *current,
    GX_WIDGET **widget_return);
```
### Description

GUIX maintains a tree structured list of parent and child widgets. This service returns a pointer to the parent of the current widget.

### Parameters

- *current*: Pointer to current widget.
- *widget_return*: Pointer to return widget pointer.

### Return Values

- **GX_SUCCESS** (0x00) pointer returned.
- GX_PTR_ERROR (0x07) Invalid widget pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Threads

### Example

```C
/* Retrieve parent widget. */

GX_WIDGET *get_parent(GX_WIDGET *current)
{
      GX_WIDGET *parent;
      UINT status;

      status = gx_widget_parent_get(current, &parent);
      if (status == GX_SUCCESS)
      {
            return parent;
      }
      return GX_NULL;
}
```
### See Also

- gx_widget_first_child_get
- gx_widget_last_child_get
- gx_widget_next_sibling_get
- gx_widget_previous_sibling_get
- gx_widget_top_visible_child_find

## gx_widget_pixelmap_get

Get pixelmap

### Prototype

```C
UINT gx_widget_pixelmap_get(
    GX_WIDGET *widget,
    GX_RESOURCE_ID resource_id,
    GX_PIXELMAP **return_pixelmap);
```

### Description

This service gets the pixelmap associated with the supplied resource ID. This service should only be called for visible widgets.

### Parameters

- *widget*: Pointer to widget control block.
- *pixelmap_id*: Pixelmap resource ID.
- *return_pixelmap*: Pointer to pixelmap destination pointer.

### Return Values

- **GX_SUCCESS** (0x00) Successful pixelmap get.
- **GX_INVALID_RESOURCE_ID** (0x33) Invalid resource ID.
- **GX_INVALID_CANVAS** (0x20) Widget canvas not valid or widget is invisible.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Initialization and threads

### Example

```c
GX_PIXELMAP *my_pixelmap;

/* Get the pixelmap associated with MY_PIXELMAP_ID. */
status = gx_widget_pixelmap_get(widget, MY_PIXELMAP_RESOURCE_ID,
                                &my_pixelmap);

/* If status is GX_SUCCESS, "my_pixelmap" contains the pixemap pointer. */
```
### See Also

- gx_widget_color_get
- gx_widget_font_get

## gx_widget_previous_sibling_get

Return pointer to previous sibling of the current widget

### Prototype

```C
UINT gx_widget_previous_sibling_get(
    GX_WIDGET *current,
    GX_WIDGET **widget_return);
```

### Description

GUIX maintains a tree structured list of parent and child widgets. This service returns a pointer to the previous sibling of the current widget.

### Parameters

- *current*: Pointer to current widget.
- *widget_return*: Pointer to return widget pointer.

### Return Values

- **GX_SUCCESS** (0x00) pointer returned.
- GX_PTR_ERROR (0x07) Invalid widget pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Threads

### Example

```C
/* Retrieve previous sibling widget. */

GX_WIDGET *get_previous(GX_WIDGET *current)
{
      GX_WIDGET *sibling;
      UINT status;

      status = gx_widget_previous_sibling_get(current, &sibling);
      if (status == GX_SUCCESS)
      {
          return sibling;
      }
      return GX_NULL;
}
```
### See Also

- gx_widget_first_child_get
- gx_widget_last_child_get
- gx_widget_next_sibling_get
- gx_widget_parent_get
- gx_widget_top_visible_child_find

## gx_widget_resize

Resize widget

### Prototype

```C
UINT gx_widget_resize(
    GX_WIDGET *widget, 
    GX_RECTANGLE *new_size);
```
### Description

This service resizes the widget. If the widget is visible, it is automatically invalidated and queued for redrawing.

### Parameters

- *widget*: Pointer to widget.
- *new_size*: New widget size.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget resize.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_RECTANGLE new_size;

gx_utility_rectangle_define(&new_size, 0, 0, 100, 100);

/* Resize widget "my_widget". */
status = gx_widget_resize(&my_widget, &new_size);

/* If status is GX_SUCCESS the widget "my_widget" has been resized. */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_shift

Shift widget

### Prototype

```C
UINT gx_widget_shift(
    GX_WIDGET *widget, 
    GX_VALUE x_shift,
    GX_VALUE y_shift, 
    GX_BOOL mark_dirty);
```

### Description

This service shifts the widget and optionally marks it as dirty.

### Parameters

- *widget*: Pointer to widget.
- *x_shift*: Number of pixels to shift on x-axis.
- *y_shift*: Number of pixels to shift on y-axis.
- *mark_dirty*: GX_TRUE to indicate dirty, otherwise GX_FALSE.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget shift GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Shift widget "my_widget". */
status = gx_widget_shift(&my_widget, 10, 20, GX_FALSE);

/* If status is GX_SUCCESS the widget "my_widget" has been shifted. */
```

### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_show

Show widget

### Prototype

```C
UINT gx_widget_show(GX_WIDGET *widget);
```

### Description

This service shows the widget. The widget will become visible only if it is attached to a parent and the parent widget is also visible.

### Parameters

- *widget*: Pointer to widget.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget show.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Show widget "my_widget". */
status = gx_widget_show(&my_widget);

/* If status is GX_SUCCESS the widget "my_widget" has been shown. */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_status_add

Add widget status

### Prototype

```C
UINT gx_widget_status_add(
    GX_WIDGET *widget, 
    ULONG status)
```

### Description

This service adds any combination of status flags to the specified widget.

### Parameters

- *widget*: Pointer to widget.
- *status*: Status to add.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget status add.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Add status to widget "my_widget". */
status = gx_widget_status_add(&my_widget, status_to_add);

/* If status is GX_SUCCESS the widget "my_widget" status was. */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_status_get

Get widget status

### Prototype

```C
UINT gx_widget_status_get(
    GX_WIDGET *widget,
    ULONG *return_status)
```

### Description

This service retrieves status flags from the widget.

### Parameters

- *widget*: Pointer to widget.
- *return_status*: Pointer to the status being returned.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget status get.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
ULONT get_status;

/* Retrieve status flag from widget "my_widget". */ status =
gx_widget_status_get(&my_widget, &get_status);

/* If status is GX_SUCCESS the status from widget "my_widget" is
saved to "get_status". */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_status_remove

Remove widget status

### Prototype

```C
UINT gx_widget_status_remove(
    GX_WIDGET *widget, 
    ULONG status)
```

### Description

This service removes the specified status flags from the widgets internal status variable.

### Parameters

- *widget*: Pointer to widget.
- *status*: Status to remove.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget status removal.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Remove status of widget "my_widget". */
status = gx_widget_status_remove(&my_widget, status_to_remove);

/* If status is GX_SUCCESS, the status flags are removed from the
widget "my_widget". */
```
### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_status_test

Test widget status

### Prototype

```C
UINT gx_widget_status_test(
    GX_WIDGET *widget, 
    ULONG status,
    GX_BOOL *return_test);
```
### Description

This service tests the status flags of the specified widget and stores the result in the memory pointed by "return_test".

### Parameters

- *widget*: Pointer to widget.
- *status*: Status to test.
- *return_status*: Pointer to destination for result of test.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget status test.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_BOOL test_result;

/* Test status of widget "my_widget". */
status = gx_widget_status_test(&my_widget, status_to_test,
                              &test_result);

/* If status is GX_SUCCESS the widget "my_widget" status was tested
and the result in "test_result". */
```
### See Also

- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set

## gx_widget_string_get

Retrieve string associated with a visible widget and string ID (deprecated)

### Prototype

```C
UINT gx_widget_string_get(
    GX_WIDGET *widget,
    GX_RESOURCE_ID string_id,
    GX_CONST GX_CHAR **string);
```

### Description

This service is deprecated in favor of gx_widget_string_get_ext().

This service returns the string table entry for the given string ID value. This service is similar to gx_display_string_get, except the active display is determined automatically rather than being passed in by the caller. This service can only be used for widgets which are visible, i.e. the display associated with this widget is known.

### Parameters

- *widget*: Pointer to widget.
- *string_id*: String ID value from resources header.
- *string*: Address of variable to return string.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget status test.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_CONST GX_CHAR *string;

/* Test status of widget "my_widget". */
status = gx_widget_string_get(&my_widget, GX_STRING_ID_SHUTDOWN,
      &string);

/* If status is GX_SUCCESS the string has been retrieved. */
```
### See Also

- gx_display_string_get
- gx_display_active_langauge_set

## gx_widget_string_get_ext

Retrieve string associated with a visible widget and string ID

### Prototype

```C
UINT gx_widget_string_get(
    GX_WIDGET *widget,
    GX_RESOURCE_ID string_id,
    GX_CONST GX_STRING *string);
```

### Description

This service returns the string table entry for the given string ID value. This service is similar to gx_display_string_get, except the active display is determined automatically rather than being passed in by the caller. This service can only be used for widgets which are visible, i.e. the display associated with this widget is known.

### Parameters

- *widget*: Pointer to widget.
- *string_id*: String ID value from resources header.
- *string*: Address of variable to return string.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget status test.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_STRING string;

/* Test status of widget "my_widget". */

status = gx_widget_string_get_ext(&my_widget,
                          GX_STRING_ID_SHUTDOWN, &string);

/* If status is GX_SUCCESS the string has been retrieved. */
```

### See Also

- gx_display_string_get
- gx_display_active_langauge_set

## gx_widget_style_add

Add widget style

### Prototype

```C
UINT gx_widget_style_add(
    GX_WIDGET *widget, 
    ULONG style)
```

### Description

This service adds a style to the widget. In addition, the following
actions are taken.

If the added style is GX_STYLE_TRANSPARENT, status
GX_STATUS_TRANSPARENT will be added.

If the added style is GX_STYLE_ENABLED, status
GX_STATUS_SELECTABLE will be added.

If the widget is visible, it is automatically invalidated and queued
for redrawing.

### Parameters

- *widget*: Pointer to widget.
- *style*: New style to add. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget style add.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Add style to widget "my_widget". */
status = gx_widget_style_add(&my_widget, GX_STYLE_BORDER_RAISED);

/* If status is GX_SUCCESS, the style was successfully applied to the widget "my_widget". */
```

### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_style_get

Get widget style

### Prototype

```C
UINT gx_widget_style_get(
    GX_WIDGET *widget, 
    ULONG *return_style)
```

### Description

This service retrieves style flag from the widget.

### Parameters

- *widget*: Pointer to widget.
- *return_style*: Pointer to the style being returned.

### Return Values

- **GX_SUCCESS** (0x00) Successfully retrieved widget style.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads


### Example

```C
/* Retrieve style from widget into "style". */
status = gx_widget_style_get(&my_widget, &style);

/* If status is GX_SUCCESS the style flag from widget is saved in "style". */
```

### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_remove
- gx_widget_style_add
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_style_remove

Remove widget style

### Prototype

```C
UINT gx_widget_style_remove(
    GX_WIDGET *widget, 
    ULONG style)
```

### Description

This service removes a style from the widget. In addition, the
following actions are taken.

If the removed style is GX_STYLE_TRANSPARENT, status
GX_STATUS_TRANSPARENT will be removed.

If the removed style is GX_STYLE_ENABLED, status
GX_STATUS_SELECTABLE will be removed.

If the widget is visible, it is automatically invalidated and queued
for redrawing.

### Parameters

- *widget*: Pointer to widget.
- *style*: Style to remove. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget style remove.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Remove style from widget "my_widget". */
status = gx_widget_style_remove(&my_widget,
                                GX_STYLE_BORDER_RAISED);

/* If status is GX_SUCCESS the GX_STYLE_BORDER_RAISED style was removed from the widget "my_widget".*/
```

### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_style_set

Set widget style

### Prototype

```C
UINT gx_widget_style_set(
    GX_WIDGET *widget, 
    ULONG style)
```

### Description

This service sets a style to the widget.

If the set style includes GX_STYLE_TRANSPARENT, status
GX_STATUS_TRANSPARENT will be added, otherwise the status will be
removed.

If the set style includes GX_STYLE_ENABLED, status
GX_STATUS_SELECTABLE will be added, otherwise the status will be
removed.

If the widget is visible, it is automatically invalidated and queued
for redrawing.

### Parameters

- *widget*: Pointer to widget.
- *style*: Style to set. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget style set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set style GX_STYLE_TRANSPARENT to the widget "my_widget". */
status = gx_widget_style_set(&my_widget, GX_STYLE_TRANSPARENT);

/* If status is GX_SUCCESS the widget "my_widget" style is set to GX_STYLE_TRANSPARENT. */
```

### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_set
- gx_widget_width_get

## gx_widget_text_blend

Blend text assigned to widget (deprecated)

### Prototype

```C
UINT gx_widget_text_blend(
    GX_WIDGET *widget, 
    UINT *tColor,
    UINT font_id, 
    GX_CHAR *string,
    INT x_offset, 
    INT y_offset, 
    UCHAR alpha)
```

### Description

This service is deprecated in favor of gx_widget_text_blend_ext().

This service blends the specified text over a widget using current
brush and text alignment.

### Parameters

- *widget*: Pointer to widget.
- *tColor*: Text color.
- *font_id*: Font ID.
- *string*: Drawing string.
- *x_offset*: Drawing position adjustment.
- *y_offset*: Drawing position adjustment.
- *alpha*: Blending value 0-255.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget width get.
- **GX_INVALID_STRING_LENGTH** (0x34) Invalid string length.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Blend "my_string" over "my_widget" given alpha value 120. */
status = gx_widget_text_blend(&my_widget, my_text_color, my_font_id,
                              &my_string, xoffset, yoffset, 120);

/* If status is GX_SUCCESS "my_string" has been blend to "my_widget". */
```

### See Also

- gx_widget_text_blend_ext

## gx_widget_text_blend_ext

Blend text assigned to widget

### Prototype

```C
UINT gx_widget_text_blend_ext(
    GX_WIDGET *widget, 
    UINT *tColor,
    UINT font_id, 
    GX_CONST GX_STRING *string,
    INT x_offset, 
    INT y_offset, 
    UCHAR alpha)
```

### Description

This service is deprecated in favor of gx_widget_text_blend_ext().

This service renders a string over the specified widget using the
current brush and text alignment and specified color, font, and x,y
offset.

### Parameters

- *widget*: Pointer to widget.
- *tColor*: Text color.
- *font_id*: Font ID.
- *string*: Drawing string.
- *x_offset*: Drawing position adjustment.
- *y_offset*: Drawing position adjustment.
- *alpha*: Blending value 0-255.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget width get.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_STRING_LENGTH (0x34) Invalid string length.

### Allowed From

Initialization and threads

### Example

```C
gx_string render_string;
render_string.gx_string_ptr = "Hello";
render_string.gx_string_length =
    strlen(render_string.gx_string_ptr);

/* Blend "my_string" over "my_widget" given alpha value 120. */
status = gx_widget_text_blend_ext(&my_widget,
        my_text_color,
        my_font_id,
        &render_string, xoffset, yoffset, 120);

/* If status is GX_SUCCESS "my_string" has been blend to "my_widget". */
```

### See Also

- gx_widget_text_draw_ext

## gx_widget_text_draw

Draw text assigned to widget (deprecated)

### Prototype

```C
VOID gx_widget_text_draw(
    GX_WIDGET *widget, 
    UINT *tColor,
    UINT font_id, 
    GX_CHAR *string,
    INT x_offset, 
    INT y_offset)
```

### Description

This service is deprecated in favor of gx_widget_text_draw_ext().

This service draws the specified text over a widget using current
brush and text alignment.

### Parameters

- *widget*: Pointer to widget.
- *tColor*: Text color.
- *font_id*: Font ID.
- *string*: Drawing string.
- *x_offset*: Drawing position adjustment.
- *y_offset*: Drawing position adjustment.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom widget draw function. */

VOID my_custom_widget_draw(GX_WIDGET *widget)
{
    /* Add your own background raw here. */

    /* Call widget text draw. */
    gx_widget_text_draw(widget, my_text_color, my_font_id
                        &my_string, xoffset, yoffset);
}
```

### See Also

- gx_widget_text_blend
- gx_widget_text_id_draw

## gx_widget_text_draw_ext

Draw text assigned to widget

### Prototype

```C
VOID gx_widget_text_draw(
    GX_WIDGET *widget,
    UINT *tColor,
    UINT font_id, 
    GX_CONST GX_STRING *string,
    INT x_offset,
    INT y_offset)
```

### Description

This service draws the specified text over a widget using current
brush and text alignment.

### Parameters

- *widget*: Pointer to widget.
- *tColor*: Text color.
- *font_id*: Font ID.
- *text_id*: Text ID.
- *x_offset*: Drawing position adjustment.
- *y_offset*: Drawing position adjustment.

### Return Values

- None

### Allowed From

Threads

### Example

```C
gx_string render_string;
render_string.gx_string_ptr = "Hello";
render_string.gx_string_length =
    strlen(render_string.gx_string_ptr);

/* Write a custom widget draw function. */
VOID my_custom_widget_draw(GX_WIDGET *widget)
{
    /* Add your own background draw here. */

    /* Call widget text draw. */
    gx_widget_text_draw_ext(widget, my_text_color, my_font_id
                        &render_string, xoffset, yoffset);
}
```

### See Also

- gx_widget_text_blend
- gx_widget_text_id_draw

## gx_widget_text_id_draw

Draw text assigned to widget

### Prototype

```C
VOID gx_widget_text_id_draw(
    GX_WIDGET *widget, 
    UINT *tColor,
    UINT font_id, 
    UINT text_id,
    INT x_offset, 
    INT y_offse)
```

### Description

This service draws text over a widget given a text ID.

### Parameters

- *widget*: Pointer to widget.
- *tColor*: Text color.
- *font_id*: Font ID.
- *text_id*: Text ID.
- *x_offset*: Drawing position adjustment.
- *y_offset*: Drawing position adjustment.

### Return Values

- None

### Allowed From

Initialization and threads

### Example

```C
/* Write a custom widget draw function. */

VOID my_custom_widget_draw(GX_WIDGET *widget)
{
    /* Add your own background raw here. */

    /* Call widget text id draw. */
    gx_widget_text_id_draw(widget, my_text_color, my_font_id
                            &my_string_id, xoffset, yoffset);
}
```

### See Also

- gx_widget_text_blend
- gx_widget_text_draw

## gx_widget_top_visible_child_find

Return pointer to visible child that is top of Z order

### Prototype

```C
UINT gx_widget_top_visible_child_find(
    GX_WIDGET *current,
    GX_WIDGET **widget_return);
```

### Description

GUIX maintains a tree structured list of parent and child widgets.
This service returns a pointer to the topmost visible child of the
current widget.

### Parameters

- *current*: Pointer to current widget.
- *widget_return*: Pointer to return widget pointer.

### Return Values

- **GX_SUCCESS** (0x00) pointer returned.
- GX_PTR_ERROR (0x07) Invalid widget pointer.
- GX_INVALID_WIDGET (0x12) Invalid widget.

### Allowed From

Threads

### Example

```C
/* Retrieve topmost visible widget on the display. */

GX_WIDGET *get_top_window(GX_WINDOW_ROOT *root)
{
    GX_WIDGET *top_window;
    UINT status;

    status = gx_widget_top_visible_child_find(root, &top_window);
    if (status == GX_SUCCESS)
    {
        return top_window;
    }
    return GX_NULL;
}
```

### See Also

- gx_widget_first_child_get
- gx_widget_last_child_get
- gx_widget_next_sibling_get
- gx_widget_parent_get
- gx_widget_previous_sibling_get

## gx_widget_type_find

Search for a widget of the requested type

### Prototype

```C
UINT gx_widget_type_find(
    GX_WIDGET *parent, 
    USHORT widget_id,
    GX_WIDGET **return_widget)
```

### Description

This service searches for a widget of the requested type.

### Parameters

- *widget*: Pointer to widget.
- *return_width*: Pointer to destination for widget width.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget width get.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_WIDGET *retrieved_widget;

/* Find horizontal scrollbar under "parent_widget". */
status = gx_widget_type_find(&parent_widget,
                GX_TYPE_HORIZONTAL_SCROLL_BAR, &retrieved_widget);

/* If status is GX_SUCCESS, the horizontal scrollbar widget is retrieved. */
```

### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set

## gx_widget_width_get

Get widget width

### Prototype

```C
UINT gx_widget_width_get(
    GX_WIDGET *widget,
    GX_VALUE *return_width)
```

### Description

This service gets the width of the widget.

### Parameters

- *widget*: Pointer to widget.
- *return_width*: Pointer to destination for widget width.

### Return Values

- **GX_SUCCESS** (0x00) Successful widget width get.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_VALUE my_widget_width;

/* Get width of widget "my_widget". */
status = gx_widget_width_get(&my_widget, &my_widget_width);

/* If status is GX_SUCCESS the width of widget "my_widget" is in "my_widget_width". */
```

### See Also

- gx_widget_attach
- gx_widget_back_move
- gx_widget_background_set
- gx_widget_border_draw
- gx_widget_border_style_set
- gx_widget_border_width_get
- gx_widget_canvas_get
- gx_widget_child_detect
- gx_widget_children_draw
- gx_widget_client_get
- gx_widget_created
- gx_widget_created_test
- gx_widget_delete
- gx_widget_detach
- gx_widget_draw
- gx_widget_draw_set
- gx_widget_event_generate
- gx_widget_event_process
- gx_widget_event_process_set
- gx_widget_event_to_parent
- gx_widget_find
- gx_widget_front_move
- gx_widget_height_get
- gx_widget_hide
- gx_widget_resize
- gx_widget_shift
- gx_widget_show
- gx_widget_status_add
- gx_widget_status_get
- gx_widget_status_remove
- gx_widget_status_test
- gx_widget_style_add
- gx_widget_style_get
- gx_widget_style_remove
- gx_widget_style_set

## gx_window_background_draw

Draw a window background

### Prototype

```C
VOID gx_window_background_draw(GX_WINDOW *window);
```
### Description

This service draws the background of the specified window.
This service is automatically called by the gx_window_draw function, but may also be invoked by the application as part of a customized window drawing function.

### Parameters

- *window*: Pointer to window to be drawn.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom window draw function. */

VOID my_window_draw(GX_WINDOW *window)
{
    /* Call default window background draw. */
    gx_window_background_draw(widget);

    /* Add your own drawing here. */

    /* Draw child widgets. */
    gx_widget_children_draw(widget);
}
```
### See Also

- gx_window_draw

## gx_window_client_height_get

Get window client height

### Prototype

```C
UINT gx_window_client_height_get(
    GX_WINDOW *window,
    GX_VALUE *return_height);
```

### Description

This service gets the client height of the window.

### Parameters

- *window*: Pointer to window.
- *return_height*: Pointer to destination for client height.

### Return Values

- **GX_SUCCESS** (0x00) Successful window client height get.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_RECTANGLE my_client_height;

/* Get client height of "my_window". */
status = gx_window_client_height_get(&my_window,
                                     &my_client_height);

/* If status is GX_SUCCESS the window "my_window" client height is contained in "my_client_height". */
```

### See Also

- gx_window_canvas_set
- gx_window_client_scroll
- gx_window_client_width_get
- gx_window_create
- gx_window_draw
- gx_window_event_process
- gx_window_root_create
- gx_window_root_delete
- gx_window_root_event_process
- gx_window_root_find
- gx_window_scroll_info_get
- gx_window_scrollbar_find
- x_window_wallpaper_get
- gx_window_wallpaper_set

## gx_window_client_scroll

Scroll window clients

### Prototype

```C
UINT gx_window_client_scroll(
    GX_WINDOW *window, 
    GX_VALUE x_scroll,
    GX_VALUE y_scroll);
```

### Description

This service scrolls the window clients by the specified amount.

### Parameters

- *window*: Pointer to window.
- *x_scroll*: Amount to scroll on the x-axis.
- *y_scroll*: Amount to scroll on the y-axis.

### Return Values

- **GX_SUCCESS** (0x00) Successful window client scroll.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Scroll clients of "my_window". */
status = gx_window_client_scroll(&my_window, 10, 0);

/* If status is GX_SUCCESS the clients of window "my_window" have been scrolled. */
```

### See Also

- gx_window_canvas_set
- gx_window_client_height_get
- gx_window_client_width_get
- gx_window_create
- gx_window_draw
- gx_window_event_process
- gx_window_root_create
- gx_window_root_delete
- gx_window_root_event_process
- gx_window_root_find gx_window_scroll_info_get
- gx_window_scrollbar_find
- gx_window_wallpaper_get
- gx_window_wallpaper_set

## gx_window_client_width_get

Get window client width

### Prototype

```C
UINT gx_window_client_width_get(
    GX_WINDOW *window,
    GX_VALUE *return_width);
```

### Description

This service gets the client width of the specified window.

### Parameters

- *window*: Pointer to window.
- *return_height*: Pointer to destination for client width.

### Return Values

- **GX_SUCCESS** (0x00) Successful window client width get.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_VALUE my_client_widthl

/* Get client width of "my_window". */
status = gx_window_client_width_get(&my_window, &my_client_width);

/* If status is GX_SUCCESS "my_client_width" contains the client width of window "my_window". */
```

### See Also

- gx_window_canvas_set
- gx_window_client_height_get
- gx_window_client_scroll
- gx_window_create
- gx_window_draw
- gx_window_event_process
- gx_window_root_create
- gx_window_root_delete
- gx_window_root_event_process
- gx_window_root_find
- gx_window_scroll_info_get
- gx_window_scrollbar_find gx_window_wallpaper_get
- gx_window_wallpaper_set

## gx_window_close

Close modal window

### Prototype

```C
UINT gx_window_close(GX_WINDOW *window);
```

### Description

This service forces a modal window to detach from its parent and
return from the modal execution loop.

### Parameters

- *window*: Pointer to window control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully closed window.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Close window "my_window". */
status = gx_window_close(&my_window);

/* If status is GX_SUCCESS window "my_window" has been closed. */
```

### See Also

- gx_window_canvas_set
- gx_window_client_height_get
- gx_window_client_scroll
- gx_window_client_width_get
- gx_window_draw
- gx_window_event_process
- gx_window_root_create
- gx_window_root_delete
- gx_window_root_event_process
- gx_window_root_find
- gx_window_scroll_info_get
- gx_window_scrollbar_find
- gx_window_wallpaper_get
- gx_window_wallpaper_set

## gx_window_create

Create window

### Prototype

```C
UINT gx_window_create(
    GX_WINDOW *window, 
    GX_CONST GX_CHAR *name,
    GX_WIDGET *parent, 
    ULONG style,
    USHORT window_id, 
    GX_CONST GX_RECTANGLE *size);
```

### Description

This service creates a window.

GX_WINDOW is derived from GX_WIDGET and supports all gx_widget API
services.

### Parameters

- *window*: Pointer to window control block.
- *name*: Logical name of window.
- *parent*: Pointer to parent widget.
- *style*: Window style. **Appendix D** contains pre-defined general styles for all widgets as well as widget-specific styles.
- *window_id*: Application-defined ID of the window.
- *size*: Size of the window.

### Return Values

- **GX_SUCCESS** (0x00) Successful window create.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_ALREADY_CREATED (0x13) Widget already created.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_WINDOW my_window;

/* Create window "my_window". */
status = gx_window_create(&my_window, "my window",
                          &my_parent_window, GX_STYLE_BORDER_RAISED,
                          MY_WINDOW_ID, &size);

/* If status is GX_SUCCESS window "my_window" has been created. */
```

### See Also

- gx_window_canvas_set
- gx_window_client_height_get
- gx_window_client_scroll
- gx_window_client_width_get
- gx_window_draw
- gx_window_event_process
- gx_window_root_create
- gx_window_root_delete
- gx_window_root_event_process
- gx_window_root_find
- gx_window_scroll_info_get
- gx_window_scrollbar_find
- gx_window_wallpaper_get
- gx_window_wallpaper_set

## gx_window_draw

Draw window

### Prototype

```C
VOID gx_window_draw(GX_WINDOW *window);
```

### Description

This service draws a window. This service is normally called
internally during canvas refresh, but can also be called from custom
window drawing functions.

### Parameters

- *window*: Pointer to window control block.

### Return Values

- None

### Allowed From

Threads

### Example

```C
/* Write a custom window draw function. */

VOID my_custom_window_draw(GX_WINDOW *window)
{
    /* Call default window draw. */
    gx_window_draw(window);

    /* Add your own drawing here. */
}
```

### See Also

- gx_window_canvas_set
- gx_window_client_height_get
- gx_window_client_scroll
- gx_window_client_width_get
- gx_window_create
- gx_window_event_process
- gx_window_root_create
- gx_window_root_delete
- gx_window_root_event_process
- gx_window_root_find
- gx_window_scroll_info_get
- gx_window_scrollbar_find
- gx_window_wallpaper_get
- gx_window_wallpaper_set

## gx_window_event_process

Process window event

### Prototype

```C
UINT gx_window_event_process(
    GX_WINDOW *window, 
    GX_EVENT *event);
```

### Description

This service processes an event for this window.

### Parameters

- *window*: Pointer to window control block.
- *event*: Pointer to event to process.

### Return Values

- **GX_SUCCESS** (0x00) Successful window event processing.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Threads

### Example

```C
/* Call generic window event processing as part of custom event processing function. */

UINT custom_window_event_process(GX_WINDOW *window,
                                 GX_EVENT *event)
{
    UINT status = GX_SUCCESS;

    switch(event->gx_event_type)
    {
        case xyz:
            /* Insert custom event handling here. */
            break;
        default:
            /* Pass all other events to the default window event processing. */
            status = gx_window_event_process(window, event);
            break;
        }
        return status;
}
```

### See Also

- gx_window_canvas_set
- gx_window_client_height_get
- gx_window_client_scroll
- gx_window_client_width_get
- gx_window_create
- gx_window_draw
- gx_window_root_create
- gx_window_root_delete
- gx_window_root_event_process
- gx_window_root_find
- gx_window_scroll_info_get
- gx_window_scrollbar_find
- gx_window_wallpaper_get
- gx_window_wallpaper_set

## gx_window_execute

Modally execute a window

### Prototype

```C
UINT gx_window_execute(
    GX_WINDOW *window,
    ULONG *return_ptr)
```

### Description

This service modally executes a window. Any user input (pen events,
etc.) outside of the window client area will be ignored. Note that this
function enters a continuous blocking execution loop, and does not
return to the caller until the model execution is terminated.

Modal execution terminates when the event processing for any received
event returns a non-zero status value, or when the gx_window_close
API function is invoked. The non-zero event processing return code is
returned to the caller through the return_ptr passed into this API

### Parameters

- *window*: Pointer to window control block.
- *return_ptr*: Location to save modal execution exit status. May be GX_NULL.

### Return Values

- **GX_SUCCESS** (0x00) Successful execution.
- **GX_SYSTEM_EVENT_RECEIVE_ERROR** (0x05) Pickup event from event queue failed.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Execute a modal window. */
status = gx_window_execute(&my_window, &return_code);

/* If status is GX_SUCCESS the window was executed, and return_code holds the execution return code. */
```

### See Also

- gx_window_canvas_set
- gx_window_client_height_get
- gx_window_client_scroll
- gx_window_client_width_get
- gx_window_create
- gx_window_draw
- gx_window_event_process
- gx_window_root_delete
- gx_window_root_event_process
- gx_window_root_find
- gx_window_scroll_info_get
- gx_window_scrollbar_find
- gx_window_wallpaper_get
- gx_window_wallpaper_set

## gx_window_root_create

Create a root window

### Prototype

```C
UINT gx_window_root_create(
    GX_WINDOW_ROOT *root_window,
    GX_CONST GX_CHAR *name,
    GX_CANVAS *canvas, 
    ULONG style,
    USHORT id,
    GX_CONST GX_RECTANGLE *size)
```

### Description

This service creates a root window.

### Parameters

- *root_window*: Pointer to root window control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully created root window.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_SIZE (0x19) Invalid widget control block size.
- GX_ALREADY_CREATED (0x13) Widget already created.

### Allowed From

Initialization and threads

### Example

```C
GX_ROOT_WINDOW root_window;
GX_CANVAS canvas;

/* Create canvas here. */

/* Create a root window. */
status = gx_window_root_create(&root_window, "root", &canvas,
GX_STYLE_BORDER_NONE, GX_NULL, &size);

/* If status is GX_SUCCESS, the "root_window" is successfully created. */
```

### See Also

- gx_window_root_delete
- gx_window_root_event_process
- gx_window_root_find

## gx_window_root_delete

Destroy a root window

### Prototype

```C
UINT gx_window_root_delete(GX_WINDOW_ROOT *root_window)
```

### Description

This service deletes a root window.

### Parameters

- *root_window*: Pointer to root window control block.

### Return Values

- **GX_SUCCESS** (0x00) Successfully deleted root window.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_SYSTEM_MEMORY_ERROR (0x30) Memory free function is not defined.

### Allowed From

Initialization and threads

### Example

```C
/* Delete a root window. */
status = gx_window_root_delete(&root_window);

/* If status is GX_SUCCESS the "root_window" is destroyed. */
```

### See Also

- gx_window_canvas_set
- gx_window_client_height_get
- gx_window_client_scroll
- gx_window_client_width_get
- gx_window_create
- gx_window_draw
- gx_window_event_process
- gx_window_root_create
- gx_window_root_event_process
- gx_window_root_find
- gx_window_scroll_info_get
- gx_window_scrollbar_find
- gx_window_wallpaper_get
- gx_window_wallpaper_set

## gx_window_root_event_process

Process event for the root window

### Prototype

```C
UINT gx_window_root_create(
    GX_WINDOW_ROOT *root_window,
    GX_EVENT *event)
```

### Description

This service processes events for the specified root window.

### Parameters

- *root_window*: Pointer to root window control block.
- *event*: Pointer to the event to be processed.

### Return Values

- **GX_SUCCESS** (0x00) Successfully processed root window event.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.

### Allowed From

Threads

### Example

```C
/* Call generic root window event processing as part of custom event processing function. */

UINT custom_root_window_event_process(GX_ROOT_WINDOW *root,
                                      GX_EVENT *event)
{
    UINT status = GX_SUCCESS;

    switch(event->gx_event_type)
    {
    case xyz:
        /* Insert custom event handling here. */
        break;
    default:
        /* Pass all other events to the default root window event processing. */
        status = gx_window_root_event_process(root, event);
        break;
    }
    return status;
}
```

### See Also

- gx_window_canvas_set
- gx_window_client_height_get
- gx_window_client_scroll
- gx_window_client_width_get
- gx_window_create
- gx_window_draw
- gx_window_event_process
- gx_window_root_create
- gx_window_root_delete
- gx_window_root_find
- gx_window_scroll_info_get
- gx_window_scrollbar_find
- gx_window_wallpaper_get
- gx_window_wallpaper_set

## gx_window_root_find

Find root window

### Prototype

```C
UINT gx_window_root_find(
    GX_WIDGET *widget,
    GX_WINDOW_ROOT **return_root_window);
```

### Description

This service finds the root window for the specified widget.

### Parameters

- *widget*: Pointer to widget control block.
- *return_root_window*: Pointer to destination for found root window.

### Return Values

- **GX_SUCCESS** (0x00) Successful root window find.
- **GX_FAILURE** (0x00) Root window not exist.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Find root window associated with window "my_window". */
status = gx_window_root_find(&my_window, &root_window);

/* If status is GX_SUCCESS the "root_window" contains the root window for window "my_window". */
```

### See Also

- gx_window_canvas_set
- gx_window_client_height_get
- gx_window_client_scroll
- gx_window_client_width_get
- gx_window_create
- gx_window_draw
- gx_window_event_process
- gx_window_root_create
- gx_window_root_delete
- gx_window_root_event_process
- gx_window_scroll_info_get
- gx_window_scrollbar_find
- gx_window_wallpaper_get
- gx_window_wallpaper_set

## gx_window_scroll_info_get

Get window scroll info

### Prototype

```C
UINT gx_window_scroll_info_get(
    GX_WINDOW *window, 
    ULONG style,
    GX_SCROLL_INFO *return_scroll_info);
```

### Description

This service gets the window scroll information.

### Parameters

- *window*: Pointer to window.
- *style*: GX_SCROLLBAR_HORIZONTAL or GX_SCROLLBAR_VERTICAL.
- *return_scroll_info*: Pointer to destination for scroll info. The parent window initializes this structure to inform the scrollbar of the parent window total size, viewable area, and scrolling increment and limits. The default implementation uses the windows client area as the viewable area and scrolls by pixels, but customized window implementation can utilize the scroll parameters. **Appendix I** contains the definition of the GX_SCROLL_INFO structure.

### Return Values

- **GX_SUCCESS** (0x00) Successful window scroll info get.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_TYPE (0x1B) Invalid type.

### Allowed From

Initialization and threads

### Example

```C
GX_SCROLL_INFO scroll_info;

/* Get scroll information for window "my_window". */
status = gx_window_scroll_info_get(&my_window,
                    GX_SCROLLBAR_HORIZONTAL, &scroll_info);

/* If status is GX_SUCCESS the "scroll_info" contains the scroll information for window "my_window". */
```
### See Also

- gx_window_canvas_set
- gx_window_client_height_get
- gx_window_client_scroll
- gx_window_client_width_get
- gx_window_create
- gx_window_draw
- gx_window_event_process
- gx_window_root_create
- gx_window_root_delete
- gx_window_root_event_process
- gx_window_root_find
- gx_window_scrollbar_find
- gx_window_wallpaper_get
- gx_window_wallpaper_set

## gx_window_scrollbar_find

Find window scrollbar

### Prototype

```C
UINT gx_window_scrollbar_find(
    GX_WINDOW *window, 
    USHORT type,
    GX_SCROLLBAR **return_scrollbar);
```

### Description

This service finds the scrollbar for the specified window.

### Parameters

- *window*: Pointer to window.
- *type*: GX_TYPE_VERTICAL_SCROLL or GX_TYPE_HORIZONTAL_SCROLL.
- *return_scrollbar*: Pointer to destination for scrollbar.

### Return Values

- **GX_SUCCESS** (0x00) Successful window scrollbar find.
- **GX_NOT_FOUND** (0x09) Scrollbar not found.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.
- GX_INVALID_TYPE (0x1B) Invalid type.

### Allowed From

Initialization and threads

### Example

```C
/* Find horizontal scrollbar for window "my_window". */
status = gx_window_scrollbar_find(&my_window,
                 GX_SCROLLBAR_HORIZONTAL, &my_scrollbar);

/* If status is GX_SUCCESS the "my_scrollbar" contains the horizontal scrollbar for window "my_window". */
```

### See Also

- gx_window_canvas_set
- gx_window_client_height_get
- gx_window_client_scroll
- gx_window_client_width_get
- gx_window_create
- gx_window_draw
- gx_window_event_process
- gx_window_root_create
- gx_window_root_delete
- gx_window_root_event_process
- gx_window_root_find
- gx_window_scroll_info_get
- gx_window_wallpaper_get
- gx_window_wallpaper_set

## gx_window_wallpaper_get

Get window wallpaper

### Prototype

```C
UINT gx_window_wallpaper_get(
    GX_WINDOW *window,
    GX_RESOURCE_ID *return_wallpaper_id);
```

### Description

This service gets the wallpaper for the specified window.

### Parameters

- *window*: Pointer to window.
- *return_wallpaper_id*: Pointer to destination for resource ID of wallpaper.

### Return Values

- **GX_SUCCESS** (0x00) Successful window wallpaper get.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
GX_RESOURCE_ID my_window_wallpaper;

/* Get wallpaper for window "my_window". */
status = gx_window_wallpaper_get(&my_window, &my_window_wallpaper);

/* If status is GX_SUCCESS the "my_window_wallpaper" contains the wallpaper resource ID for window "my_window". */
```

### See Also

- gx_window_canvas_set
- gx_window_client_height_get
- gx_window_client_scroll
- gx_window_client_width_get
- gx_window_create
- gx_window_draw
- gx_window_event_process
- gx_window_root_create
- gx_window_root_delete
- gx_window_root_event_process
- gx_window_root_find
- gx_window_scroll_info_get
- gx_window_scrollbar_find
- gx_window_wallpaper_set

## gx_window_wallpaper_set

Set window wallpaper

### Prototype

```C
UINT gx_window_wallpaper_set(
    GX_WINDOW *window,
    GX_RESOURCE_ID wallpaper_id,
    GX_BOOL tile);
```

### Description

This service sets the wallpaper for the specified window.

### Parameters

- *window*: Pointer to window.
- *wallpaper_id*: Resource ID of wallpaper to use.
- *tile*: Wallpaper is tiled if GX_TRUE, otherwise wallpaper is not tiled.

### Return Values

- **GX_SUCCESS** (0x00) Successful window wallpaper set.
- GX_CALLER_ERROR (0x11) Invalid caller of this function.
- GX_PTR_ERROR (0x07) Invalid pointer.
- GX_INVALID_WIDGET (0x12) Widget not valid.

### Allowed From

Initialization and threads

### Example

```C
/* Set wallpaper for window "my_window". */
status = gx_window_wallpaper_set(&my_window,
                                 MY_WALLPAPER_RESOURCE_ID,
                                 GX_TRUE);

/* If status is GX_SUCCESS the wallpaper for window "my_window" is set. */
```

### See Also

- gx_window_canvas_set
- gx_window_client_height_get
- gx_window_client_scroll
- gx_window_client_width_get
- gx_window_create
- gx_window_draw
- gx_window_event_process
- gx_window_root_create
- gx_window_root_delete
- gx_window_root_event_process
- gx_window_root_find
-  gx_window_scroll_info_get
- gx_window_scrollbar_find
- gx_window_wallpaper_get

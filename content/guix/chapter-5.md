---
title: Chapter 5 - GUIX Display Drivers
description: GUIX Display drivers define the software interface between the abstract drawing canvas and the physical display hardware.
---

GUIX Display drivers define the software interface between the abstract drawing canvas and the physical display hardware. The GUIX display driver implements the lowest-level drawing functions that actually change pixel color information in the canvas memory and transfer the canvas memory to the physical display frame buffer in double-buffered systems.

GUIX Display drivers are defined by a structure containing the physical display parameters and a set of function pointers to the low-level driver functions. By using these indirect function pointers, the abstract canvas and widget drawing functions are made completely independent of the hardware details.

GUIX provides a complete, fully functional, default set of drawing functions for each supported  color depth and color format. When implementing a display driver with no specific hardware acceleration capability or other hardware specific considerations, these default drawing functions are normally sufficient for the final driver implementation. For these simplest of drivers, the only function that normally needs to be implemented in the driver software is a function to configure the hardware device. This often involves initializing various hardware registers to define the LCD display clock, display dimensions etc. For all other functions, the driver implementation simply initialize the GX_DISPLAY function pointers to the default function implementations for the desired color depth and format.

When implementing a custom display driver, the best practice is to first initialize your display driver drawing function pointers with the default software implementation for the color depth you want to support, then replace those function pointers where desired to call your custom function implementations (if any). To assist with this, there is a default setup function available for each supported color depth and format. For example, if you are writing a 16 bit 5:6:5 format RGB display driver, the first thing your custom driver would normally do is invoke the generic setup routine for this color depth:

UINT my_custom_565_display_driver(GX_DISPLAY *display)

```c
{
      /* Perform standard function pointer setup. */
      _gx_display_driver_565rgb_setup(display, GX_NULL, my_buffertoggle);

}
```

The parameter my_buffer_toggle above is a pointer to your display driver buffer toggle function (which may be GX_NULL if your driver is single-buffered and drawing directly to the hardware frame buffer).

If you are writing a custom display driver, you will need to include the gx_display.h header file in your custom driver source, which is an internal use header file not available to application level software.

The GUIX display level drawing functions receive as input a pointer to a **GX_DRAW_CONTEXT** structure. The **GX_DRAW_CONTEXT** structure defines the clipping coordinates for the current drawing operation along with the brush and colors being used. Each drawing function receives as input additional parameters specific to the function requirements.

The signature of the **GX_DISPLAY** driver entry point is defined as

```c
UINT <device>_graphics_driver_<format>(GX_DISPLAY *display)
```

While the name of this function is completely up to the implementor, the convention for the drivers provided with GUIX is to use a hardware specific device name in the \<device\> field and color format for \<format\> field above.

This function must initialize the **GX_DISPLAY** structure provided as input and perform any hardware setup that is required. The **GX_DISPLAY** structure contains the following fields.

| Field | Description |
| --- | --- |
| ULONG gx_display_id | This is a field for use by the application, in cases where more than one instance of a particular driver is created. |
| CHAR *gx_display_name | An optional name used to identify the driver. |
| GX_DISPLAY *gx_display_created_next | This field is initialized by GUIX, and is used to create and maintain a list of all GX_DISPLAY instances. |
| GX_DISPLAY *gx_display_created_previous | This field is initialized by GUIX, and is used to create and maintain a list of all GX_DISPLAY instances. |
| GX_VALUE gx_display_color_format | This field should reflect the graphics data format supported by this driver. The color format types are defined in the gx_api.h header file. |
| GX_VALUE gx_display_width | This field should be initialized to hold the physical display width, in pixels. |
| GX_VALUE gx_display_height | This field should be initialized to hold the physical display height, in pixels. |
| GX_COLOR *gx_display_color_table | This is a pointer to a table used to convert color ID values to color format specific color values. |
| GX_PIXELMAP *gx_display_pixelmap_table | This is a pointer to the active pixelmap table for this display. |
| GX_FONT *gx_display_font_table | This is a pointer to the active font table for this display. |
| GX_COLOR *gx_display_palette | For palette mode drivers, this is a pointer to the active color palette. For drivers that do not use a color palette, this pointer is GX_NULL. |
| UINT gx_display_pixelmap_table_size | Number of entries in the active pixelmap table. |
| UINT gx_display_font_table_size | Number of entries in the active font table. |
| UINT gx_display_palette_size | Number of entries in color palette (if any). |
| ULONG gx_display_handle | A unique identifier, or *handle*, that specifies the display.
| UINT gx_display_driver_ready | This field is use to signal to GUIX when the driver is ready for operation. In some cases, the driver may require several levels of initialization and configuration, during which time GUIX must not attempt to utilize the driver. This flag should be set to 1 when the driver is ready to service drawing requests. |
| VOID *gx_display_driver_data | This field is for use by the driver implementation. If the driver needs to create and reference additional information not available in the GX_DISPLAY structure, the driver should allocate space for and point to this additional data using this structure field. An example of driver-specific extra data might include the DMA channel being used by the driver or the SPI channel to which the display frame buffer is connected. |
| VOID (*gx_display_driver_drawing_initiate)(struct GX_DISPLAY_STRUCT *display, struct GX_CANVAS_STRUCT *canvas) | This is a function pointer that, if not NULL, is invoked by the gx_canvas_drawing_initiate function. For display drivers that utilize a graphics accelerator or hardware graphics display list, this function might be used to begin a new display list. This function pointer can be NULL. |
| VOID (*gx_display_driver_palette_set)(struct GX_DISPLAY_STRUCT *display, GX_COLOR *palette, INT count) | This is a pointer to a function to install a color palette. This function is NULL unless the driver operates in palette (also called color lookup table or CLUT) mode. |
| VOID (*gx_display_driver_simple_line_draw)(GX_DRAW_CONTEXT *context, INT x1, INTy1, INT x2, INT y2) | This is a pointer to a function to implement generic line drawing, no anti-aliasing. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_simple_wide_line_draw)(GX_DRAW_CONTEXT *context, INT x1, INTy1, INT x2, INT y2) | This is a pointer to a function to implement generic wide line drawing, no anti-aliasing. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_anti_aliased_line_draw)(GX_DRAW_CONTEXT *context, INT x1, INTy1, INT x2, INT y2) | This is a pointer to a function to implement generic anti-aliased line drawing. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_anti_aliased_wide_line_draw)(GX_DRAW_CONTEXT *context, INT x1, INTy1, INT x2, INT y2) | This is a pointer to a function to implement generic anti-aliased wide line drawing. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_horizontal_line_draw)(GX_DRAW_CONTEXT *context, INT x1, INT x2, INT y) | This is a pointer to a function to implement the special case of horizontal line drawing. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_horizontal_pixelmap_line_draw)(GX_DRAW_CONTEXT *context, INT x1, INT x2, INT y, GX_PIXELMAP *map) | This is a pointer to a function to implement drawing a single pixelmap row. This function is used internally for pattern filling shapes. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_vertical_line_draw)(GX_DRAW_CONTEXT *context, INT y1, INT y2, INT x) | This is a pointer to a function to implement the special case of vertical line drawing. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_horizontal_pattern_line_draw)(GX_DRAW_CONTEXT *context, INT x1, INT x2, INT y) | This is a pointer to a function to implement horizontal pattern line drawing. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_vertical_pattern_line_draw)(GX_DRAW_CONTEXT *context, INT y1, INT y2, INT x) | This is a pointer to a function to implement vertical pattern line drawing. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_canvas_copy)(struct GX_CANVAS_STRUCT *source, struct GX_CANVAS_STRUCT *dest) | This is a pointer to a function to copy canvas data from one canvas to another. The source canvas invalid rectangle is used to define the copy area. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_canvas_blend)(struct GX_CANVAS_STRUCT *source, struct GX_CANVAS_STRUCT *dest) | This is a pointer to a function to alpha-blend canvas data from the source canvas with the existing data in the destination canvas. The source canvas invalid rectangle is used to define the blend area. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_pixelmap_blend)(GX_DRAW_CONTEXT *context, INT xpos, INT ypos, GX_PIXELMAP *pmp, GX_UBYTE alpha) | This is a pointer to a function to blend a pixelmap on the background canvas defined by the draw context. The supplied alpha value may be in addition to an alpha channel contained in the pixelmap data. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_pixelmap_draw)(GX_DRAW_CONTEXT *context, INT xpos, INT ypos, GX_PIXELMAP *pmp) | This is a pointer to a function to draw a pixelmap into the canvas defined by the draw context. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_jpeg_draw)(GX_DRAW_CONTEXT *context, INT xpos, INT ypos, GX_PIXELMAP *pmp) | This is a pointer to a function to decode a jpg image and render it directly to the canvas. This function is only provided if GX_SOFTWARE_DECODER_SUPPORT is defined. This function pointer can be NULL. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_png_draw)(GX_DRAW_CONTEXT *context, INT xpos, INT ypos, GX_PIXELMAP *pmp) | This is a pointer to a function to decode a png image and render it directly to the canvas. This function is only provided if GX_SOFTWARE_DECODER_SUPPORT is defined. This function pointer can be NULL. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_pixelmap_rotate)(GX_DRAW_CONTEXT *context, INT xpos, INT ypos, GX_PIXELMAP *pmp INT angle, INT rot_cx, INT rot_cy) | This is a pointer to a function to rotate a pixelmap and render the result directly to the canvas. This function is invoked by the gx_canvas_pixelmap_rotate API Default implementations of this function are provided for each supported color depth and color format. |
| VOID *gx_display_driver_pixel_write)(GX_DRAW_CONTEXT *context, INT x, INT y, GX_COLOR color) | This is a pointer to a function to write one pixel into the canvas memory. Default implementations of this function are provided for each supported color depth and color format. |
| VOID *gx_display_driver_block_move)(GX_DRAW_CONTEXT *context, GX_RECTANGLE *block, INT xshift, INT yshift) | This is a pointer to a function to move or shift a block of pixels within a canvas. This function is primarily used for rapidly scrolling a window contents. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_pixel_blend)(GX_DRAW_CONTEXT *context, INT x, INT y, GX_COLOR color, GX_UBYTE alpha) | This function is used to alpha-blend the incoming pixel color value with the existing color value in the canvas memory at position x,y. Default implementations of this function are provided for each supported color depth and color format. |
| GX_COLOR (*gx_display_driver_native_color_get)(GX_COLOR rawcolor) | This function converts a color from the 32-bit A:R:G:B color format used internally by GUIX to the native color format of the canvas and display. Some loss of color information is expected for display drivers running at lower color depths. Default implementations of this function are provided for each supported color depth and color format. |
| USHORT (*gx_display_driver_row_pitch_get)(USHORT width) | Returns the byte count or stride of one row of graphics data given the requested canvas width. This function is used to calculate the size of the memory area needed to create a canvas. The row pitch and width are not always the same due to hardware scan line alignment constraints. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_buffer_toggle)(struct GX_CANVAS_STRUCT *canvas, GX_RECTANGLE *dirty_area) | This is a pointer to a function to toggle between the working and visible frame buffers for double-buffered memory systems. This function must first instruct the hardware to begin using the new frame buffer, then copy the modified portion of the new visible buffer to the companion buffer, to insure the two buffers stay in synch. | 
| VOID (*gx_display_driver_polygon_draw)(GX_DRAW_CONTEXT *context, INT num_points, GX_POINT *vertices | Pointer to a function to draw a polygon. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_polygon_fill)(GX_DRAW_CONTEXT *context, INT num_points, GX_POINT *vertices | Pointer to a function to draw a filled polygon. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_circle_draw)(GX_DRAW_CONTEXT *context, INT xcenter, INT ycenter, UINT r) | Pointer to a function to draw a circle. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_anti_aliased_circle_draw) (GX_DRAW_CONTEXT *context, INT xcenter, INT ycenter, UINT r) | Pointer to a function to draw an anti-aliased circle. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_wide_circle_draw)(GX_DRAW_CONTEXT *context, INT xcenter, INT ycenter, UINT r) | Pointer to a function to draw a circle with a wide outline. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_wide_anti_aliased_circle_draw) (GX_DRAW_CONTEXT *context, INT xcenter, INT ycenter, UINT r) | Pointer to a function to draw an anti-aliased circle with a wide outline. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_circle_fill)(GX_DRAW_CONTEXT *context, INT xcenter, INT ycenter, UINT r) | Pointer to a function to draw a filled circle. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_arc_draw)(GX_DRAW_CONTEXT *context, INT xcenter, INT ycenter, UINT r, INT start_angle, INT end_angle) | Pointer to a function to draw an arc. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_anti_aliased_arc_draw)(GX_DRAW_CONTEXT *context, INT xcenter, INT ycenter, UINT r, INT start_angle, INTend_angle) | Pointer to a function to draw an anti-aliased arc. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_wide_arc_draw)(GX_DRAW_CONTEXT *context, INT xcenter, INT ycenter, UINT r, INT start_angle, INT end_angle) | Pointer to a function to draw an arc with a wide outline. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_anti_aliased_wide_arc_draw)(GX_DRAW_CONTEXT *context, INT xcenter, INT ycenter, UINT r, INT start_angle, INTend_angle) | Pointer to a function to draw an anti-aliased arc. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_arc_fill)(GX_DRAW_CONTEXT *context, INT xcenter, INT ycenter, UINT r, INT start_angle, INT end_angle) | Pointer to a function to draw a filled arc. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_pie_fill)(GX_DRAW_CONTEXT *context, INT xcenter, INT ycenter, UINT r, INT start_angle, INT end_angle) | Pointer to a function to draw a filled pie. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_ellipse_draw)(GX_DRAW_CONTEXT *context, INT xcenter, INT ycenter, INT a, INT b) | Pointer to a function to draw an ellipse. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_anti_aliased_ellipse_draw)(GX_DRAW_CONTEXT *context, INT xcenter, INT ycenter, INT a, INT b) | Pointer to a function to draw an ellipse. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_wide_ellipse_draw)(GX_DRAW_CONTEXT *context, INT xcenter, INT ycenter, INT a, INT b) | Pointer to a function to draw an ellipse with a wide outline. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_anti_aliased_wide_ellipse_draw)(GX_DRAW_CONTEXT *context, INT xcenter, INT ycenter, INT a, INT b) | Pointer to a function to draw an ellipse with a wide outline. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_ellipse_fill)(GX_DRAW_CONTEXT *context, INT xcenter, INT ycenter, INT a, INT b) | Pointer to a function to draw a filled ellipse. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_8bit_glyph_draw)(GX_DRAW_CONTEXT *context, GX_RECTANGLE *draw_area, GX_POINT *map_offset, constGX_GLYPH *glyph) | Pointer to function to draw one 8-bit aliased text glyph to the canvas using the brush of the current drawing context. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_4bit_glyph_draw)(GX_DRAW_CONTEXT *context, GX_RECTANGLE *draw_area, GX_POINT *map_offset, const GX_GLYPH *glyph) | Pointer to function to draw one 4-bit aliased text glyph to the canvas using the brush of the current drawing context. Default implementations of this function are provided for each supported color depth and color format. |
| VOID (*gx_display_driver_1bit_glyph_draw)(GX_DRAW_CONTEXT *context, GX_RECTANGLE *draw_area, GX_POINT *map_offset, const GX_GLYPH *glyph) | Pointer to function to draw one 1-bit monochrome text glyph to the canvas using the brush of the current drawing context. Default implementations of this function are provided for each supported color depth and color format. |



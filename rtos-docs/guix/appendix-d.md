---
title: Appendix D - GUIX Brush, Canvas and Gradient Attributes
description: Learn about the GUIX brush, canvas and gradient attributes.
---

# Appendix D - GUIX Brush, Canvas and Gradient Attributes

__**Brush Styles:**__

**GX_BRUSH_OUTLINE**
- Value: 0x0000
- Description: This brush style applies to shape drawing functions such as gx_canvas_rectangle_draw or gx_canvas_polygon_draw. This style indicates the shape should be outlined, in addition to optionally being filled. If the GX_BRUSH_OUTLINE style is set and the GX_BRUSH_SOLID_FILL is cleared, the shape is only outlined.

**GX_BRUSH_SOLID_FILL**
- Value: 0x0001
- Description: This brush style applies to shape drawing functions, and indicates that the requested shape should be filled with a solid color using the current brush fill color.

**GX_BRUSH_PIXELMAP_FILL**
- Value: 0x0002
- Description: This brush style applies to shape drawing functions, and indicates that the requested shape should be pattern filled with the current brush pixelmap.

**GX_BRUSH_ALIAS**
- Value: 0x0004
- Description: This brush style applies to all line drawing and shape outlines. If this flag is set, lines and outlines are drawing with the more accurate but also more time consuming anti-aliased drawing algorithms. This style flag is only used for 16-bpp color depths and higher.

**GX_BRUSH_UNDERLINE**
- Value: 0x0008
- Description: This flag applies to text drawing, and indicates that subsequent text drawn should be underlined.

**GX_BRUSH_ROUND**
- Value: 0x0010
- Description: This flag applies to line drawing, and indicates that line ends are drawn with a round or circular shape, rather than the default square shape.

__**Canvas Flags:**__

**GX_CANVAS_SIMPLE**
- Value: 0x01
- Description: A memory canvas which is used to off-screen drawing.

**GX_CANVAS_MANAGED**
- Value: 0x02
- Description: A canvas which automatically flushed to the active display, either as part of the composite building process or as part of the buffer toggle operation for single-canvas architectures.

**GX_CANVAS_VISIBLE**
- Value: 0x04
- Description: This flag can be used to turn on and off a canvas, without losing the canvas drawing contents.

**GX_CANVAS_MODIFIED**
- Value: 0x08
- Description: Reserved for future use.

**GX_CANVAS_COMPOSITE**
- Value: 0x20
- Description: This flag is used by the application when configuring a multiple-canvas system which will composite multiple managed canvases into the composite canvas, and the composite is the driven to the hardware frame buffer.

__**Gradient Types:**__

**GX_GRADIENT_TYPE_VERTICAL**
- Value: 0x01
- Description: Creates a vertical alphamap gradient.

**GX_GRADIENT_TYPE_ALPHA**
- Value: 0x02
- Description: Creates an alpha-map style gradient. This is currently the only gradient style supported.

**GX_GRADIENT_TYPE_MIRROR**
- Value: 0x04
- Description: This flag indicates that the gradient should peak at the center of the width/height range, and return to the starting value as it reaches the right/bottom edge. Without this style flag, the gradient will be a linear gradient from top-to-bottom or left-to-right, depending on the GX_GRADIENT_TYPE_VERTICAL flag.
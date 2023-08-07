# Introduce to the gui_win widget

The widget provides a virtual area for the developer to place application-required widgets. The developer can depend on the requirement to create the space relative to the screen. For example, Figure 1 creates an area the same as the screen dimension, and the developer can create a space with different sizes, as in Figure 2.

<center>
<div align="center">
<img src=https://drive.google.com/uc?id=1zQxEx2PWgukON2fomfEJNtLTHkBoE82J&export=download/>
</div>
</center>
<p class="text-center">
Figure 1: Same Window and Screen size
</p>

<center>
<div align="center">
<img src=https://drive.google.com/uc?id=1geGAfaFP8fuXyGIkX4VcDCDI_xKAMoh9&export=download/>
</div>
</center>
<p class="text-center">
Figure 2: Different Window and Screen sizes
</p>

And the following widgets will take the Window's widget left-top corner as the initial coordinates in Figure 3.

<center>
<div align="center">
<img src=https://drive.google.com/uc?id=1atjNxkUz0QH8HmKk6Odv8qXkPR6-_cX7&export=download/>
</div>
</center>
<p class="text-center">
Figure 3: Relative coordination to the Window widget
</p>

---
## gui_win Widget public APIs
### gui_win_create
Create the gui_win widget.
```javascript=
/**
 * @brief create a window widget.
 * @param parent the father widget the window nested in.
 * @param filename the window widget name.
 * @param x the X-axis coordinate.
 * @param x the Y-axis coordinate.
 * @param w the width.
 * @param h the hight.
 * @return return the widget object pointer
 *
 */
 gui_win_t *gui_win_create(void *parent,
                           const char *filename,
                           int16_t x,
                           int16_t y,
                           int16_t w,
                           int16_t h);
```
### set_animate
      The developer can use this function member to add the required update callback function. Whenever the GUI engine starts to refresh the display content, it will dereference the callback function to update the necessary information.

```javascript=
/**
 * @brief Configure the Windows animation callback function
 * @param struct gui_win *win: Window widget
 * @param uint32_t dur: The animation will last for a certain duration
 * @param int repeatCount: The animation will repeat times
 * @param void *callback: Animate callback function
 * @param void *p: Window update data
 * @return NULL
 *
 */
 void (* set_animate)(struct gui_win *win,
                      uint32_t dur,
                      int repeatCount,
                      void *callback,
                      void *p);
```

## gui_win widget Examples
### Create a gui_win widget example 1
This example shows that the gui_win widget size and coordination are the same as the display device, creating a red rectangle within this area. As shown in Figure 4.
      
```javascript=
 static uint16_t screen_width  = 454;
 static uint16_t screen_height = 454;
 static uint16_t windows_coordinat_x = 0;
 static uint16_t windows_coordinat_y = 0;
 uint16_t window_width = screen_height - windows_coordinat_x;
 uint16_t window_height = screen_height - windows_coordinat_y;

 static uint32_t rect_color_rgba = 0xFF0000FF;

 gui_win_t *win_inst = gui_win_create(&(app->screen),
                                      "win",
                                      windows_coordinat_x,
                                      windows_coordinat_y,
                                      window_width,
                                      window_height);

 gui_canvas_t *canvas = gui_canvas_create((void*)win_inst,
                                          "canvas",
                                          0,
                                          0,
                                          window_width,
                                          window_height,
                                          rect_color_rgba);
 canvas->draw = draw_sample_canvas;

 static void draw_sample_canvas(gui_canvas_t *canvas)
 {
    canvas_rectangle_t r = {0};
    static canvas_fill_t color;

    memcpy(&(r.fill), &canvas->base.color, sizeof(canvas->base.color));
    r.height = canvas->base.base.base.h;
    r.width = canvas->base.base.base.w;
    r.rx = 0;
    r.x = canvas->base.base.base.x;
    r.y = canvas->base.base.base.y;
    gui_canvas_api.rectangle((void*)canvas, &r);
 }
```

<center>
<div align="center">
<img src=https://drive.google.com/uc?id=1KzklB8BEwVOUq3cMzfe6PrvY0lCuJbAo&export=download/>
</div>
</center>
<p class="text-center">
Figure 4: Window widget with a red rectangle
</p>


### Create a gui_win widget example 2
This example shows that the gui_win and the offset 50 pixels of the coordination of the display device. As shown in Figure 2 5, we are creating a green rectangle within this area.

```javascript=
 static uint16_t screen_width  = 454;
 static uint16_t screen_height = 454;
 static uint16_t windows_coordinat_x = 50;
 static uint16_t windows_coordinat_y = 50;
 uint16_t window_width = screen_height - windows_coordinat_x;
 uint16_t window_height = screen_height - windows_coordinat_y;
 static uint32_t rect_color_rgba = 0x00FF00FF;

 gui_win_t *win_inst = gui_win_create(&(app->screen),
                                      "win",
                                      windows_coordinat_x,
                                      windows_coordinat_y,
                                      window_width,
                                      window_height);

 gui_canvas_t *canvas = gui_canvas_create((void*)win_inst,
                                          "canvas",
                                          0,
                                          0,
                                          window_width,
                                          window_height,
                                          rect_color_rgba);
```

<center>
<div align="center">
<img src=https://drive.google.com/uc?id=1lxy0xogofWX-7Wpr_gCviLqXtRZIdpzB&export=download/>
</div>
</center>
<p class="text-center">
Figure 5: Window widget with a different orientation
</p>

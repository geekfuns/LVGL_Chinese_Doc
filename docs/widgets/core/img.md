```eval_rst
.. include:: /header.rst 
:github_url: |github_link_base|/widgets/img.md
```
# 图像部件 Image (lv_img)


## 概览 Overview

图像是显示来自闪存（作为数组）或来自文件的图像的基本对象。 图像也可以显示符号（`LV_SYMBOL_...`）。

使用 [图像解码器接口](/overview/image.html#image-decoder) [Image decoder interface](/overview/image.html#image-decoder) 也可以支持自定义图像格式。

## 部件块与样式（Parts and Styles）

- `LV_PART_MAIN` 即显示图像的区域

## 用法（Usage）

### 图像源（Image source）

为了提供最大的灵活性，图像的来源可以是：

- 代码中的变量（带有像素的 C 数组）。
- 外部存储的文件（例如在 SD 卡上）。
- 带有 [Symbols](/overview/font) 的文本。

要设置图像的来源，请使用`lv_img_set_src(img, src)`


要从 PNG、JPG 或 BMP 图像生成像素数组，请使用 [在线图像转换工具](https://lvgl.io/tools/imageconverter) 并使用指针设置转换后的图像：`lv_img_set_src(img1, &converted_img_var );`

要使该变量在 C 文件中可见，您需要使用 `LV_IMG_DECLARE(converted_img_var)` 声明它。

要使用外部文件，需要使用在线转换器工具转换图像文件并选择二进制输出格式。

您还需要使用 LVGL 的文件系统模块，并为基本文件操作注册一个具有一些功能的驱动程序。 转到 [文件系统](/overview/file-system) 了解更多信息。

要设置来自文件的图像，请使用`lv_img_set_src(img, "S:folder1/my_img.bin")`。

您还可以设置类似于 [标签](/widgets/core/label) 的符号。 在这种情况下，图像将根据样式中指定的 *font* 呈现为文本。 它可以使用轻量级的单色“字母”代替真实图像。 你可以设置像`lv_img_set_src(img1, LV_SYMBOL_OK)`这样的符号。

### Label as an image
Images and labels are sometimes used to convey the same thing. For example, to describe what a button does. 
Therefore, images and labels are somewhat interchangeable, that is the images can display texts by using `LV_SYMBOL_DUMMY` as the prefix of the text. For example, `lv_img_set_src(img, LV_SYMBOL_DUMMY "Some text")`.


### Transparency
The internal (variable) and external images support 2 transparency handling methods:

- **Chroma-keying** - Pixels with `LV_COLOR_CHROMA_KEY` (*lv_conf.h*) color will be transparent.
- **Alpha byte** - An alpha byte is added to every pixel that contains the pixel's opacity

### Palette and Alpha index
Besides the *True color* (RGB) color format, the following formats are supported:
- **Indexed** - Image has a palette.
- **Alpha indexed** - Only alpha values are stored.

These options can be selected in the image converter. To learn more about the color formats, read the [Images](/overview/image) section.

### Recolor
A color can be mixed with every pixel of an image with a given intensity.
This can be useful to show different states (checked, inactive, pressed, etc.) of an image without storing more versions of the same image.
This feature can be enabled in the style by setting `img_recolor_opa` between `LV_OPA_TRANSP` (no recolor, value: 0) and `LV_OPA_COVER` (full recolor, value: 255).
The default value is `LV_OPA_TRANSP` so this feature is disabled.

The color to mix is set by `img_recolor`.

### Auto-size
If the width or height of the image object is set to `LV_SIZE_CONTENT` the object's size will be set according to the size of the image source in the respective direction.

### Mosaic
If the object's size is greater than the image size in any directions, then the image will be repeated like a mosaic.
This allows creation a large image from only a very narrow source.
For example, you can have a *300 x 5* image with a special gradient and set it as a wallpaper using the mosaic feature.

### Offset
With `lv_img_set_offset_x(img, x_ofs)` and `lv_img_set_offset_y(img, y_ofs)`, you can add some offset to the displayed image.
Useful if the object size is smaller than the image source size.
Using the offset parameter a [Texture atlas](https://en.wikipedia.org/wiki/Texture_atlas) or a "running image" effect can be created by [Animating](/overview/animation) the x or y offset.

## Transformations

Using the `lv_img_set_zoom(img, factor)` the images will be zoomed. Set `factor` to `256` or `LV_IMG_ZOOM_NONE` to disable zooming. 
A larger value enlarges the images (e.g. `512` double size), a smaller value shrinks it (e.g. `128` half size).
Fractional scale works as well. E.g. `281` for 10% enlargement.

To rotate the image use `lv_img_set_angle(img, angle)`. Angle has 0.1 degree precision, so for 45.8° set 458.

The `transform_zoom` and `transform_angle` style properties are also used to determine the final zoom and angle.

By default, the pivot point of the rotation is the center of the image. It can be changed with `lv_img_set_pivot(img, pivot_x, pivot_y)`. `0;0` is the top left corner.

The quality of the transformation can be adjusted with `lv_img_set_antialias(img, true/false)`. With enabled anti-aliasing the transformations are higher quality but slower.

The transformations require the whole image to be available. Therefore indexed images (`LV_IMG_CF_INDEXED_...`), alpha only images (`LV_IMG_CF_ALPHA_...`) or images from files can not be transformed. 
In other words transformations work only on true color images stored as C array, or if a custom [Image decoder](/overview/images#image-edecoder) returns the whole image.

Note that the real coordinates of image objects won't change during transformation. That is `lv_obj_get_width/height/x/y()` will return the original, non-zoomed coordinates.

### Size mode

By default, when the image is zoomed or rotated the real coordinates of the image object are not changed. 
The larger content simply overflows the object's boundaries. 
It also means the layouts are not affected the by the transformations. 

If you need the object size to be updated to the transformed size set `lv_img_set_size_mode(img, LV_IMG_SIZE_MODE_REAL)`. (The previous mode is the default and called `LV_IMG_SIZE_MODE_VIRTUAL`).
In this case if the width/height of the object is set to `LV_SIZE_CONTENT` the object's size will be set to the zoomed and rotated size.
If an explicit size is set then the overflowing content will be cropped. 

## Events
No special events are sent by image objects.

See the events of the [Base object](/widgets/obj) too.

Learn more about [Events](/overview/event).

## Keys
No *Keys* are processed by the object type.

Learn more about [Keys](/overview/indev).

## Example

```eval_rst

.. include:: ../../../examples/widgets/img/index.rst

```

## API

```eval_rst

.. doxygenfile:: lv_img.h
  :project: lvgl

```

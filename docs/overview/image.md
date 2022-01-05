```eval_rst
.. include:: /header.rst 
:github_url: |github_link_base|/overview/image.md
```
# 图像（Images）

图像在系统中是存储位图数据和元数据的文件或变量

## 储存图像（Store images）

图像可以存储在两个地方
- 内部存储器（RAM 或 ROM）中的变量里
- 文件

### 变量（Variables）

存储在变量中的图像主要由具有以下字段的 `lv_img_dsc_t` 结构体组成：

- **header**
  - *cf* 颜色格式（Color format）. See [below](#color-format)
  - *w* 宽度（像素值）width in pixels (<= 2048)
  - *h* 高度（像素值）height in pixels (<= 2048)
  - *always zero* 3 bits 0 值 
  - *reserved* 系统保留字段
- **data** 指向存储图像本身的数组的指针pointer to an array where the image itself is stored
- **data_size** `data` 字节长度 （bytes）

这些通常放在 C 文件中并存储在项目里。 它们像其他常量数据一样链接到生成的可执行文件中。

### 文件（Files）

要处理文件，您需要向 LVGL 添加一个存储 *Drive*。 简而言之，*Drive* 是在 LVGL 中注册以进行文件操作的函数（*open*、*read*、*close* 等）的集合。在任何情况下，*Drive* 都只是读取和/或将数据写入内存的抽象。 

您可以向标准文件系统（SD 卡上的 FAT32）添加接口，或者创建简单的文件系统以从 SPI 闪存读取数据，在任何情况下，*Drive* 都只是读取和/或将数据写入内存的抽象。

具体请看 [File system](/overview/file-system) 

存储为文件的图像不会链接到生成的可执行文件中，必须在显示之前读入 RAM。 因此，它们不像在编译时链接的图像资源那样友好。 但是，它们更容易替换而无需重建主程序。


## 颜色格式（Color formats）
支持各种内置颜色格式：
- **LV_IMG_CF_TRUE_COLOR** 简单地存储 RGB 颜色（以 LVGL 配置的任何颜色深度）。
- **LV_IMG_CF_TRUE_COLOR_ALPHA** 类似于`LV_IMG_CF_TRUE_COLOR`，但它还为每个像素添加了一个 alpha（透明度）字节。
- **LV_IMG_CF_TRUE_COLOR_CHROMA_KEYED** 类似于`LV_IMG_CF_TRUE_COLOR`，但如果像素具有`LV_COLOR_TRANSP` 颜色（在*lv_conf.h* 中设置），它将是透明的。
- **LV_IMG_CF_INDEXED_1/2/4/8BIT** 使用具有 2、4、16 或 256 种颜色的调色板，并以 1、2、4 或 8 位存储每个像素。
- **LV_IMG_CF_ALPHA_1/2/4/8BIT**  **仅存储 1、2、4 或 8 位的 Alpha 值** 像素采用`style.img_recolor` 的颜色和设置的不透明度。 源图像必须是 Alpha 通道。 这非常适用于类似于字体的位图，其中整个图像是一种可以更改的颜色。

`LV_IMG_CF_TRUE_COLOR` 图像的字节按以下顺序存储

对于 32 位色深：
- Byte 0: Blue
- Byte 1: Green
- Byte 2: Red
- Byte 3: Alpha

对于 16 位色深：
- Byte 0: Green 3 lower bit, Blue 5 bit
- Byte 1: Red 5 bit, Green 3 higher bit
- Byte 2: Alpha byte (only with LV_IMG_CF_TRUE_COLOR_ALPHA)

对于 8 位色深：
- Byte 0: Red 3 bit, Green 3 bit, Blue 2 bit
- Byte 2: Alpha byte (only with LV_IMG_CF_TRUE_COLOR_ALPHA)


您可以以 *Raw* 格式存储图像，以表明它没有使用其中一种内置颜色格式进行编码，并且需要使用外部 [图像解码器](#image-decoder) 来解码图像。

- **LV_IMG_CF_RAW** Indicates a basic raw image (e.g. a PNG or JPG image).
- **LV_IMG_CF_RAW_ALPHA** Indicates that an image has alpha and an alpha byte is added for every pixel.
- **LV_IMG_CF_RAW_CHROMA_KEYED** Indicates that an image is chroma-keyed as described in `LV_IMG_CF_TRUE_COLOR_CHROMA_KEYED` above.


## 添加与使用图像（Add and use images）

您可以通过两种方式将图像添加到 LVGL：
- 使用在线转换器
- 手动创建图像

### 在线转换（Online converter）
在线图像转换器：https://lvgl.io/tools/imageconverter

通过在线转换器将图像添加到 LVGL 很容易。

1. 准备 *BMP*, *PNG* or *JPG* 格式的图像.
2. 为图像指定在 LVGL 中的名称。
3. 选择 [颜色格式](#color-formats) [Color format](#color-formats).
4. 选择要生成的图像类型。 选择二进制文件将生成一个 `.bin` 文件，该文件必须单独存储并使用 [文件支持](#files) 读取。 选择一个变量将生成一个可以链接到您的项目的标准 C 文件。
5. 点击*转换*按钮。 转换完成后，您的浏览器将自动下载生成的文件。

在生成的 C 数组（变量）中，所有色深（1、8、16 或 32）的位图都包含在 C 文件中，但实际上只有与 *lv_conf.h* 中的 `LV_COLOR_DEPTH` 匹配的色深才会链接到生成的可执行文件中。

对于二进制文件，您需要指定所需的颜色格式：
- RGB332 for 8-bit color depth
- RGB565 for 16-bit color depth
- RGB565 Swap for 16-bit color depth (two bytes are swapped)
- RGB888 for 32-bit color depth

### 手动创建（Manually create an image）

如果您在运行时生成图像，您可以制作一个图像变量以使用 LVGL 显示它。 例如：

```c
uint8_t my_img_data[] = {0x00, 0x01, 0x02, ...};

static lv_img_dsc_t my_img_dsc = {
    .header.always_zero = 0,
    .header.w = 80,
    .header.h = 60,
    .data_size = 80 * 60 * LV_COLOR_DEPTH / 8,
    .header.cf = LV_IMG_CF_TRUE_COLOR,          /*Set the color format*/
    .data = my_img_data,
};

```

如果颜色格式是`LV_IMG_CF_TRUE_COLOR_ALPHA`，你可以将`data_size`设置为`80 * 60 * LV_IMG_PX_SIZE_ALPHA_BYTE`。

在运行时创建和显示图像的另一个（可能更简单）方法是使用 [Canvas](/widgets/core/canvas) 对象。

### 使用图像（Use images）

在 LVGL 中使用图像的最简单方法是使用 [lv_img](/widgets/core/img) 对象显示它：

```c
lv_obj_t * icon = lv_img_create(lv_scr_act(), NULL);

/*From variable*/
lv_img_set_src(icon, &my_icon_dsc);

/*From file*/
lv_img_set_src(icon, "S:my_icon.bin");
```
如果图像是使用在线转换器转换的，则应在调用图像的文件使用函数 `LV_IMG_DECLARE(my_icon_dsc)` 来声明图像。



## 图像解码器（Image decoder）
As you can see in the [Color formats](#color-formats) section, LVGL supports several built-in image formats. In many cases, these will be all you need. LVGL doesn't directly support, however, generic image formats like PNG or JPG.

To handle non-built-in image formats, you need to use external libraries and attach them to LVGL via the *Image decoder* interface.

An image decoder consists of 4 callbacks:
- **info** get some basic info about the image (width, height and color format).
- **open** open an image: either store a decoded image or set it to `NULL` to indicate the image can be read line-by-line.
- **read** if *open* didn't fully open an image this function should give some decoded data (max 1 line) from a given position.
- **close** close an opened image, free the allocated resources.

You can add any number of image decoders. When an image needs to be drawn, the library will try all the registered image decoders until it finds one which can open the image, i.e. one which knows that format.

The `LV_IMG_CF_TRUE_COLOR_...`, `LV_IMG_INDEXED_...` and `LV_IMG_ALPHA_...` formats (essentially, all non-`RAW` formats) are understood by the built-in decoder.

### 自定义图像格式（Custom image formats）

The easiest way to create a custom image is to use the online image converter and select `Raw`, `Raw with alpha` or `Raw with chroma-keyed` format. It will just take every byte of the binary file you uploaded and write it as an image "bitmap". You then need to attach an image decoder that will parse that bitmap and generate the real, renderable bitmap.

`header.cf` will be `LV_IMG_CF_RAW`, `LV_IMG_CF_RAW_ALPHA` or `LV_IMG_CF_RAW_CHROMA_KEYED` accordingly. You should choose the correct format according to your needs: a fully opaque image, using an alpha channel or using a chroma key.

After decoding, the *raw* formats are considered *True color* by the library. In other words, the image decoder must decode the *Raw* images to *True color* according to the format described in the [Color formats](#color-formats) section.

If you want to create a custom image, you should use `LV_IMG_CF_USER_ENCODED_0..7` color formats. However, the library can draw images only in *True color* format (or *Raw* but ultimately it will be in *True color* format).
The `LV_IMG_CF_USER_ENCODED_...` formats are not known by the library and therefore they should be decoded to one of the known formats from the [Color formats](#color-formats) section.
It's possible to decode an image to a non-true color format first (for example: `LV_IMG_INDEXED_4BITS`) and then call the built-in decoder functions to convert it to *True color*.

With *User encoded* formats, the color format in the open function (`dsc->header.cf`) should be changed according to the new format.


### 注册图像解码器（Register an image decoder）

Here's an example of getting LVGL to work with PNG images.

First, you need to create a new image decoder and set some functions to open/close the PNG files. It should look like this:

```c
/*Create a new decoder and register functions */
lv_img_decoder_t * dec = lv_img_decoder_create();
lv_img_decoder_set_info_cb(dec, decoder_info);
lv_img_decoder_set_open_cb(dec, decoder_open);
lv_img_decoder_set_close_cb(dec, decoder_close);


/**
 * Get info about a PNG image
 * @param decoder pointer to the decoder where this function belongs
 * @param src can be file name or pointer to a C array
 * @param header store the info here
 * @return LV_RES_OK: no error; LV_RES_INV: can't get the info
 */
static lv_res_t decoder_info(lv_img_decoder_t * decoder, const void * src, lv_img_header_t * header)
{
  /*Check whether the type `src` is known by the decoder*/
  if(is_png(src) == false) return LV_RES_INV;

  /* Read the PNG header and find `width` and `height` */
  ...

  header->cf = LV_IMG_CF_RAW_ALPHA;
  header->w = width;
  header->h = height;
}

/**
 * Open a PNG image and return the decided image
 * @param decoder pointer to the decoder where this function belongs
 * @param dsc pointer to a descriptor which describes this decoding session
 * @return LV_RES_OK: no error; LV_RES_INV: can't get the info
 */
static lv_res_t decoder_open(lv_img_decoder_t * decoder, lv_img_decoder_dsc_t * dsc)
{

  /*Check whether the type `src` is known by the decoder*/
  if(is_png(src) == false) return LV_RES_INV;

  /*Decode and store the image. If `dsc->img_data` is `NULL`, the `read_line` function will be called to get the image data line-by-line*/
  dsc->img_data = my_png_decoder(src);

  /*Change the color format if required. For PNG usually 'Raw' is fine*/
  dsc->header.cf = LV_IMG_CF_...

  /*Call a built in decoder function if required. It's not required if`my_png_decoder` opened the image in true color format.*/
  lv_res_t res = lv_img_decoder_built_in_open(decoder, dsc);

  return res;
}

/**
 * Decode `len` pixels starting from the given `x`, `y` coordinates and store them in `buf`.
 * Required only if the "open" function can't open the whole decoded pixel array. (dsc->img_data == NULL)
 * @param decoder pointer to the decoder the function associated with
 * @param dsc pointer to decoder descriptor
 * @param x start x coordinate
 * @param y start y coordinate
 * @param len number of pixels to decode
 * @param buf a buffer to store the decoded pixels
 * @return LV_RES_OK: ok; LV_RES_INV: failed
 */
lv_res_t decoder_built_in_read_line(lv_img_decoder_t * decoder, lv_img_decoder_dsc_t * dsc, lv_coord_t x,
                                                  lv_coord_t y, lv_coord_t len, uint8_t * buf)
{
   /*With PNG it's usually not required*/

   /*Copy `len` pixels from `x` and `y` coordinates in True color format to `buf` */

}

/**
 * Free the allocated resources
 * @param decoder pointer to the decoder where this function belongs
 * @param dsc pointer to a descriptor which describes this decoding session
 */
static void decoder_close(lv_img_decoder_t * decoder, lv_img_decoder_dsc_t * dsc)
{
  /*Free all allocated data*/

  /*Call the built-in close function if the built-in open/read_line was used*/
  lv_img_decoder_built_in_close(decoder, dsc);

}

```

So in summary:
- In `decoder_info`, you should collect some basic information about the image and store it in `header`.
- In `decoder_open`, you should try to open the image source pointed by `dsc->src`. Its type is already in `dsc->src_type == LV_IMG_SRC_FILE/VARIABLE`.
If this format/type is not supported by the decoder, return `LV_RES_INV`.
However, if you can open the image, a pointer to the decoded *True color* image should be set in `dsc->img_data`.
If the format is known, but you don't want to decode the entire image (e.g. no memory for it), set `dsc->img_data = NULL` and use `read_line` to get the pixel data.
- In `decoder_close` you should free all allocated resources.
- `decoder_read` is optional. Decoding the whole image requires extra memory and some computational overhead.
However, it can decode one line of the image without decoding the whole image, you can save memory and time.
To indicate that the *line read* function should be used, set `dsc->img_data = NULL` in the open function.


### 手动使用解码器（Manually use an image decoder）

LVGL will use registered image decoders automatically if you try and draw a raw image (i.e. using the `lv_img` object) but you can use them manually too. Create an `lv_img_decoder_dsc_t` variable to describe the decoding session and call `lv_img_decoder_open()`.

The `color` parameter is used only with `LV_IMG_CF_ALPHA_1/2/4/8BIT` images to tell color of the image. 
`frame_id` can be used if the image to open is an animation. 


```c

lv_res_t res;
lv_img_decoder_dsc_t dsc;
res = lv_img_decoder_open(&dsc, &my_img_dsc, color, frame_id);

if(res == LV_RES_OK) {
  /*Do something with `dsc->img_data`*/
  lv_img_decoder_close(&dsc);
}

```


## 图像缓存（Image caching）

有时打开图像需要很多时间。
连续解码 PNG 图像或从缓慢的外部存储器加载图像将是低效的，并且用户体验不好。

因此，LVGL 可以预先缓存给定数量的图像。 缓存意味着一些图像将保持打开状态，因此 LVGL 可以从通过 `dsc->img_data` 快速访问，减少加载时间。


Of course, caching images is resource intensive as it uses more RAM to store the decoded image. LVGL tries to optimize the process as much as possible (see below), but you will still need to evaluate if this would be beneficial for your platform or not. Image caching may not be worth it if you have a deeply embedded target which decodes small images from a relatively fast storage medium.

### Cache size
The number of cache entries can be defined with `LV_IMG_CACHE_DEF_SIZE` in *lv_conf.h*. The default value is 1 so only the most recently used image will be left open.

The size of the cache can be changed at run-time with `lv_img_cache_set_size(entry_num)`.

### Value of images
When you use more images than cache entries, LVGL can't cache all of the images. Instead, the library will close one of the cached images to free space.

To decide which image to close, LVGL uses a measurement it previously made of how long it took to open the image. Cache entries that hold slower-to-open images are considered more valuable and are kept in the cache as long as possible.

If you want or need to override LVGL's measurement, you can manually set the *time to open* value in the decoder open function in `dsc->time_to_open = time_ms` to give a higher or lower value. (Leave it unchanged to let LVGL control it.)

Every cache entry has a *"life"* value. Every time an image is opened through the cache, the *life* value of all entries is decreased to make them older.
When a cached image is used, its *life* value is increased by the *time to open* value to make it more alive.

If there is no more space in the cache, the entry with the lowest life value will be closed.

### Memory usage
Note that a cached image might continuously consume memory. For example, if three PNG images are cached, they will consume memory while they are open.

Therefore, it's the user's responsibility to be sure there is enough RAM to cache even the largest images at the same time.

### Clean the cache
Let's say you have loaded a PNG image into a `lv_img_dsc_t my_png` variable and use it in an `lv_img` object. If the image is already cached and you then change the underlying PNG file, you need to notify LVGL to cache the image again. Otherwise, there is no easy way of detecting that the underlying file changed and LVGL will still draw the old image from cache.

To do this, use `lv_img_cache_invalidate_src(&my_png)`. If `NULL` is passed as a parameter, the whole cache will be cleaned.


## API


### Image buffer

```eval_rst

.. doxygenfile:: lv_img_buf.h
  :project: lvgl

```

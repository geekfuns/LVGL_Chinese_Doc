```eval_rst
.. include:: /header.rst 
:github_url: |github_link_base|/overview/font.md
```
# 字体（Fonts）


在 LVGL 中，字体是渲染单个字母（字形）图像所需的位图和其他信息的集合，字体存储在 `lv_font_t` 变量中，可以在样式的 *text_font* 字段中设置。 例如：
```c
lv_style_set_text_font(&my_style, &lv_font_montserrat_28);  /*Set a larger font*/
```
字体具有 **bpp（像素位数）** 属性，它显示了使用多少位来描述字体中的像素，像素值决定像素的不透明度。

这样，使用更高的 *bpp*，字母的边缘可以更平滑。 可能的 *bpp* 值为 1、2、4 和 8（值越高表示质量越好）。

*bpp* 属性还会影响存储字体所需的内存量。 例如，*bpp = 4* 使字体比 *bpp = 1* 大近四倍。
  

## Unicode编码支持（Unicode support）

LVGL 支持 **UTF-8** 编码的 Unicode 字符。
编辑器通常需要配置为将代码/文本保存为 UTF-8（通常是默认值）格式, `LV_TXT_ENC` 在 *lv_conf.h* 中设置为 `LV_TXT_ENC_UTF8`。 （默认值）

测试：
```c
lv_obj_t * label1 = lv_label_create(lv_scr_act(), NULL);
lv_label_set_text(label1, LV_SYMBOL_OK);
```

如果一切正常，应显示 ✓ 字符。

## 内嵌字体（Built-in fonts）

有几种不同大小的内置字体，可以在 `lv_conf.h` 中使用 *LV_FONT_...* 定义启用。

### 普通字体（Normal fonts）

包含所有 ASCII 字符、度数符号 (U+00B0)、子弹符号 (U+2022) 和内置符号（见下文）。
- `LV_FONT_MONTSERRAT_12` 12 px font
- `LV_FONT_MONTSERRAT_14` 14 px font
- `LV_FONT_MONTSERRAT_16` 16 px font
- `LV_FONT_MONTSERRAT_18` 18 px font
- `LV_FONT_MONTSERRAT_20` 20 px font
- `LV_FONT_MONTSERRAT_22` 22 px font
- `LV_FONT_MONTSERRAT_24` 24 px font
- `LV_FONT_MONTSERRAT_26` 26 px font
- `LV_FONT_MONTSERRAT_28` 28 px font
- `LV_FONT_MONTSERRAT_30` 30 px font
- `LV_FONT_MONTSERRAT_32` 32 px font
- `LV_FONT_MONTSERRAT_34` 34 px font
- `LV_FONT_MONTSERRAT_36` 36 px font
- `LV_FONT_MONTSERRAT_38` 38 px font
- `LV_FONT_MONTSERRAT_40` 40 px font
- `LV_FONT_MONTSERRAT_42` 42 px font
- `LV_FONT_MONTSERRAT_44` 44 px font
- `LV_FONT_MONTSERRAT_46` 46 px font
- `LV_FONT_MONTSERRAT_48` 48 px font

### 特殊字体（Special fonts）
- `LV_FONT_MONTSERRAT_12_SUBPX` 与普通 12 px 字体相同，但具有 [子像素渲染] [subpixel rendering](#subpixel-rendering) 
- `LV_FONT_MONTSERRAT_28_COMPRESSED` 与普通 28 px 字体相同，但存储为3 bpp字体 [compressed font](#compress-fonts) 
- `LV_FONT_DEJAVU_16_PERSIAN_HEBREW` 16 px 字体，正常范围 + 希伯来语、阿拉伯语、波斯语字母及其所有形式
- `LV_FONT_SIMSUN_16_CJK` 具有正常范围的 16 px 字体加上 1000 个最常见的 CJK 部首
- `LV_FONT_UNSCII_8` 8 px 像素完美字体，只有 ASCII 字符
- `LV_FONT_UNSCII_16` 16 px 像素完美字体，只有 ASCII 字符

内置字体是**全局变量**，名称如下
`lv_font_montserrat_16` 对于 16 px 高度的字体。 要在样式中使用它们，只需添加一个指向字体变量的指针，即字体名称。

*bpp = 4* 的内置字体包含 ASCII 字符并使用 [Montserrat](https://fonts.google.com/specimen/Montserrat) 字体。

除了 ASCII 范围外，以下符号还添加到 [FontAwesome](https://fontawesome.com/) 字体的内置字体中。

![](/misc/symbols.png "Built-in Symbols in LVGL")

这些符号可以单独使用：
```c
lv_label_set_text(my_label, LV_SYMBOL_OK);
```

或与字符串一起使用（编译时字符串连接）：
```c
lv_label_set_text(my_label, LV_SYMBOL_OK "Apply");
```

或多个符号组合在一起：
```c
lv_label_set_text(my_label, LV_SYMBOL_OK LV_SYMBOL_WIFI LV_SYMBOL_PLAY);
```

## 特性（Special features）

### 双向书写支持（Bidirectional support）
大多数语言使用从左到右（简称 LTR）书写方向，但是某些语言（例如希伯来语、波斯语或阿拉伯语）使用从右到左（简称 RTL）方向。

LVGL 不仅支持 RTL 文本，还支持混合（又名双向，BiDi）文本渲染。 一些例子：

![](/misc/bidi.png "Bidirectional text examples")


BiDi 支持由 *lv_conf.h* 中的 `LV_USE_BIDI` 启用

所有文本都有一个基本方向（LTR 或 RTL），它决定了一些渲染规则和文本的默认对齐方式（左或右）。
然而，在 LVGL 中，基本方向不仅适用于标签，同样可以为每个对象设置的通用属性。
如果未设置，则它将从父级继承，这意味着设置屏幕的基本方向就足够了，每个对象都会继承它。

屏幕的默认基本方向可以通过 *lv_conf.h* 中的 `LV_BIDI_BASE_DIR_DEF` 设置，其他对象从其父对象继承方向。

要设置对象的基本方向，请使用 `lv_obj_set_base_dir(obj, base_dir)`。 可能的基本方向是：
- `LV_BIDI_DIR_LTR`: Left to Right base direction
- `LV_BIDI_DIR_RTL`: Right to Left base direction
- `LV_BIDI_DIR_AUTO`: Auto detect base direction
- `LV_BIDI_DIR_INHERIT`: Inherit base direction from the parent (or a default value for non-screen objects)

此列表总结了 RTL 基本方向对对象的影响：
- Create objects by default on the right
- `lv_tabview`: Displays tabs from right to left
- `lv_checkbox`: Shows the box on the right
- `lv_btnmatrix`: Shows buttons from right to left
- `lv_list`: Shows icons on the right
- `lv_dropdown`: Aligns options to the right
- The texts in `lv_table`, `lv_btnmatrix`, `lv_keyboard`, `lv_tabview`, `lv_dropdown`, `lv_roller` are "BiDi processed" to be displayed correctly

### 阿拉伯语和波斯语支持（Arabic and Persian support）
There are some special rules to display Arabic and Persian characters: the *form* of a character depends on its position in the text. 
A different form of the same letter needs to be used if is isolated, at start, middle or end positions. Besides these, some conjunction rules should also be taken into account.

LVGL supports these rules if `LV_USE_ARABIC_PERSIAN_CHARS` is enabled.

However, there some limitations:
- Only displaying text is supported (e.g. on labels), text inputs (e.g. text area) don't support this feature.
- Static text (i.e. const) is not processed. E.g. texts set by `lv_label_set_text()` will be "Arabic processed" but `lv_lable_set_text_static()` won't.
- Text get functions (e.g. `lv_label_get_text()`) will return the processed text.

### 亚像素渲染（Subpixel rendering）

抗锯齿相关，若有需要请自行翻译

Subpixel rendering allows for tripling the horizontal resolution by rendering anti-aliased edges on Red, Green and Blue channels instead of at pixel level granularity. This takes advantage of the position of physical color channels of each pixel, resulting in higher quality letter anti-aliasing. Learn more [here](https://en.wikipedia.org/wiki/Subpixel_rendering).

For subpixel rendering, the fonts need to be generated with special settings: 
- In the online converter tick the `Subpixel` box
- In the command line tool use `--lcd` flag. Note that the generated font needs about three times more memory.

Subpixel rendering works only if the color channels of the pixels have a horizontal layout. That is the R, G, B channels are next each other and not above each other. 
The order of color channels also needs to match with the library settings. By default, LVGL assumes `RGB` order, however this can be swapped by setting `LV_SUBPX_BGR  1` in *lv_conf.h*.

### 压缩字体（Compressed fonts）
The bitmaps of fonts can be compressed by 
- ticking the `Compressed` check box in the online converter
- not passing the `--no-compress` flag to the offline converter (compression is applied by default) 

Compression is more effective with larger fonts and higher bpp. However, it's about 30% slower to render compressed fonts.
Therefore it's recommended to compress only the largest fonts of a user interface, because
- they need the most memory 
- they can be compressed better
- and probably they are used less frequently then the medium-sized fonts, so the performance cost is smaller.

## 增加新字体（Add a new font）

有几种方法可以将新字体添加到项目中：
1. 最简单的方法是使用 [Online font converter](https://lvgl.io/tools/fontconverter). 只需设置参数，单击 *Convert* 按钮，将字体复制到项目中即可。 **请务必仔细阅读该站点上提供的步骤，否则在转换时会出错。**
2. 使用 [Offline font converter](https://github.com/lvgl/lv_font_conv). (需要安装 Node.js)
3. 如果您想创建类似与内置字体（Montserrat 字体和符号），但大小和/或范围不同的内容，您可以使用 `lvgl/scripts/built_in_font` 文件夹中的 `built_in_font_gen.py` 脚本。
（需要安装 Python 和 `lv_font_conv`）

要在文件中声明字体，请使用 `LV_FONT_DECLARE(my_font_name)`.

要使字体全局可用（如内置字体），请将它们添加到 *lv_conf.h* 中的`LV_FONT_CUSTOM_DECLARE`。

## 添加新符号（Add new symbols）

内置符号是从 [FontAwesome](https://fontawesome.com/) 字体创建的。

1. 在 [https://fontawesome.com](https://fontawesome.com) 上搜索符号。 例如 [USB 符号](https://fontawesome.com/icons/usb?style=brands)。 复制其 Unicode ID，在本例中为`0xf287`。
2. 打开 [Online font converter](https://lvgl.io/tools/fontconverter). 添加 [FontAwesome.woff](https://lvgl.io/assets/others/FontAwesome5-Solid+Brands+Regular.woff). 
3. 设置名称、大小、BPP 等参数。 将使用此名称在项目中声明和使用字体
4. 将符号的 Unicode ID 添加到范围字段。 例如，USB 符号的` 0xf287`。 更多的符号可以用`,`枚举。
5. 转换字体并将生成的源代码复制到您的项目中，需要确保编译了字体的 `.c` 文件。
6. 使用语句 `extern lv_font_t my_font_name;` 声明代码，或者使用函数 `LV_FONT_DECLARE(my_font_name);`.

**使用符号（Using the symbol）**
1. 将 Unicode 值转换为 UTF8，例如在 [站点](http://www.ltg.ed.ac.uk/~richard/utf-8.cgi?input=f287&mode=hex) 上。 对于`0xf287`，*Hex UTF-8 字节* 是`EF 8A 87`。
2. 从 UTF8 值创建一个 `define` 字符串：`#define MY_USB_SYMBOL "\xEF\x8A\x87"`
3. 创建一个标签并设置文本。 例如 `lv_label_set_text(label, MY_USB_SYMBOL)`

注意 - `lv_label_set_text(label, MY_USB_SYMBOL)` 在 `style.text.font` 属性中定义的字体中搜索此符号。 要使用该符号，您可能需要对其进行更改。 例如` style.text.font = my_font_name`

Note - `lv_label_set_text(label, MY_USB_SYMBOL)` searches for this symbol in the font defined in `style.text.font` properties. To use the symbol you may need to change it. Eg ` style.text.font = my_font_name` 

## 在运行时加载字体（Load a font at run-time）

机翻：
`lv_font_load` 可用于从文件加载字体。 字体需要具有特殊的二进制格式。 （不是 TTF 或 WOFF）。
使用 [lv_font_conv](https://github.com/lvgl/lv_font_conv/) 和 `--format bin` 选项来生成 LVGL 兼容字体文件。

请注意，要加载字体 [LVGL 的文件系统](/overview/file-system) 需要启用，并且必须添加驱动程序。

`lv_font_load` can be used to load a font from a file. The font needs to have a special binary format. (Not TTF or WOFF). 
Use [lv_font_conv](https://github.com/lvgl/lv_font_conv/) with the `--format bin` option to generate an LVGL compatible font file.

Note that to load a font [LVGL's filesystem](/overview/file-system) needs to be enabled and a driver must be added.

Example
```c
lv_font_t * my_font;
my_font = lv_font_load(X/path/to/my_font.bin);

/*Use the font*/

/*Free the font if not required anymore*/
lv_font_free(my_font);
```


## 添加新的字体引擎（Add a new font engine）

LVGL 的字体界面设计得非常灵活，但即便如此，同样也可以添加自己的字体引擎来代替 LVGL 的内部字体引擎。

例如，您可以使用 [FreeType](https://www.freetype.org/) 实时渲染来自 TTF 字体的字形或使用外部闪存来存储字体的位图并在库需要时读取它们。

可以在 [lv_freetype](https://github.com/lvgl/lv_lib_freetype) 存储库中找到可以使用的 FreeType。

为此，需要创建一个自定义的 `lv_font_t` 变量：
```c
/*Describe the properties of a font*/
lv_font_t my_font;
my_font.get_glyph_dsc = my_get_glyph_dsc_cb;        /*Set a callback to get info about gylphs*/
my_font.get_glyph_bitmap = my_get_glyph_bitmap_cb;  /*Set a callback to get bitmap of a glyp*/
my_font.line_height = height;                       /*The real line height where any text fits*/
my_font.base_line = base_line;                      /*Base line measured from the top of line_height*/
my_font.dsc = something_required;                   /*Store any implementation specific data here*/
my_font.user_data = user_data;                      /*Optionally some extra user data*/

...

/* Get info about glyph of `unicode_letter` in `font` font.
 * Store the result in `dsc_out`.
 * The next letter (`unicode_letter_next`) might be used to calculate the width required by this glyph (kerning)
 */
bool my_get_glyph_dsc_cb(const lv_font_t * font, lv_font_glyph_dsc_t * dsc_out, uint32_t unicode_letter, uint32_t unicode_letter_next)
{
    /*Your code here*/

    /* Store the result.
     * For example ...
     */
    dsc_out->adv_w = 12;        /*Horizontal space required by the glyph in [px]*/
    dsc_out->box_h = 8;         /*Height of the bitmap in [px]*/
    dsc_out->box_w = 6;         /*Width of the bitmap in [px]*/
    dsc_out->ofs_x = 0;         /*X offset of the bitmap in [pf]*/
    dsc_out->ofs_y = 3;         /*Y offset of the bitmap measured from the as line*/
    dsc_out->bpp   = 2;         /*Bits per pixel: 1/2/4/8*/

    return true;                /*true: glyph found; false: glyph was not found*/
}


/* Get the bitmap of `unicode_letter` from `font`. */
const uint8_t * my_get_glyph_bitmap_cb(const lv_font_t * font, uint32_t unicode_letter)
{
    /* Your code here */

    /* The bitmap should be a continuous bitstream where
     * each pixel is represented by `bpp` bits */

    return bitmap;    /*Or NULL if not found*/
}
```

## 字体退回（Use font fallback）

您可以在 `lv_font_t` 中指定 `fallback` 以提供字体的回退。 当字体找不到字母的字形时，它会尝试让字体从`fallback` 来处理。

`fallback` can be chained, so it will try to solve until there is no `fallback` set.

```c
/* Roboto font doesn't have support for CJK glyphs */
lv_font_t *roboto = my_font_load_function();
/* Droid Sans Fallback has more glyphs but its typeface doesn't look good as Roboto */
lv_font_t *droid_sans_fallback = my_font_load_function();
/* So now we can display Roboto for supported characters while having wider characters set support */
roboto->fallback = droid_sans_fallback;
```

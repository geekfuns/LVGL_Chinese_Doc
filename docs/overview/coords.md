```eval_rst
.. include:: /header.rst 
:github_url: |github_link_base|/overview/coords.md
```
# 位置、大小与布局（Positions, sizes, and layouts）

## 概览（Overview）

与 LVGL 的许多其他部分类似，设置坐标的概念受到 CSS 的启发。

简单来说：
- 坐标显式地存储在样式（大小、位置、布局等）中
- 支持最小宽度、最大宽度、最小高度、最大高度
- 有像素、百分比和“内容”单位
- x=0; y=0 坐标表示父对象的左上角加左/上边距加上边框宽度
- 宽度/高度表示完整尺寸，“内容区域”较小于填充和边框宽度
- 支持部分flexbox 和grid功能
 
###  单位（Units）
- 像素pixel: 像素坐标，只能为整数. E.g. `lv_obj_set_x(btn, 10)`
- 百分比percentage: 对象或其父对象的大小百分比（取决于属性）。 `lv_pct(value)` 将值转换为百分比。 E.g. `lv_obj_set_width(btn, lv_pct(50))`
- `LV_SIZE_CONTENT`: 设置对象的宽度/高度以涉及所有子项的特殊值。 就像CSS中的 `auto` . E.g. `lv_obj_set_width(btn, LV_SIZE_CONTENT)`.

### 盒子模型（Boxing model）
与CSS中的模型相似 [border-box](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing) .

对象的“box”由以下部分构成：
- 边界框（bounding box）：元素的宽度/高度。
- 边框宽度（border width）：边框的宽度。
- 填充（padding）：对象两侧与其子对象之间的空间。
- 内容（content）: 内容区域，即边界框的大小减去边框宽度和内边距。

![The box models of LVGL: The content area is smaller than the bounding box with the padding and border width](/misc/boxmodel.png)

边框绘制在边界框内，放置对象的子对象时，在边界内 LVGL 会保留“填充边距”。

轮廓绘制在边界框外。

### 重要提示（Important notes）
本节描述了 LVGL 可能会出现的特殊情况。

#### 非立即计算坐标（Postponed coordinate calculation）

LVGL为了提高性能，不会立即重新计算坐标变化，变化的对象被标记为“dirty”，在重绘屏幕之前 LVGL会检查是否有“dirty”对象，如果有，则刷新它们的位置、大小和布局。

换句话说，如果你需要获取一个物体的坐标，而坐标刚刚改变，则需要强制LVGL重新计算坐标。

具体的，使用函数 `lv_obj_update_layout(obj)`.

对象的大小和位置可能取决于父级或布局，调用`lv_obj_update_layout`函数会重新计算`obj`屏幕上所有对象的坐标。

#### 移除样式（Removing styles）

正如 使用样式 [Using styles](#using-styles) 部分所述，坐标也可以通过样式属性设置。 
更准确地说，每个与样式坐标相关的属性都存储为样式属性。 如果你使用 `lv_obj_set_x(obj, 20)` 函数，LVGL 会将 `x=20` 存储在对象的样式中.

这是一个内部机制，与使用LVGL没有多大关系。但是，有一种情况需要了解。 如果对象的样式被删除
```c
lv_obj_remove_style_all(obj)
```

或者
```c
lv_obj_remove_style(obj, NULL, LV_PART_MAIN);
```
先前设置的坐标也将被删除。

例如：
```c
/*obj1的大小最终将被设置为默认值*/
lv_obj_set_size(obj1, 200, 100);  /*Now obj1 has 200;100 size*/
lv_obj_remove_style_all(obj1);    /*It removes the set sizes*/


/*obj2 会被设置为尺寸 200，100  */
lv_obj_remove_style_all(obj2);
lv_obj_set_size(obj2, 200, 100);
```

## 坐标（Position）

### 简易方式（Simple way）
要设置对象的x和y坐标，请使用：
```c
lv_obj_set_x(obj, 10);        //分开设置
lv_obj_set_y(obj, 20);
lv_obj_set_pos(obj, 10, 20); 	//同时设置
```

默认情况下，x和y坐标是从父级内容区域的左上角开始测量的。

比如，如果父对象设置了每边5个像素的填充，上述代码实际上会在 (15, 25)的位置放置  `obj` 。

百分比值则是根据父级的内容区域大小计算的。

```c
lv_obj_set_x(btn, lv_pct(10)); //x = 10 % of parent content area width
```

### 对齐（Align）
在某些情况下，可以从默认左上角更改定位原点。例如更改为右下角，（0,0）位置表示：与右下角对齐。

使用函数：
```c
lv_obj_set_align(obj, align);
``` 

如果要更改对齐方式并设置新坐标，请执行以下操作：
```c
lv_obj_align(obj, align, x, y);
```

可以使用以下对齐选项：
- `LV_ALIGN_TOP_LEFT`
- `LV_ALIGN_TOP_MID`
- `LV_ALIGN_TOP_RIGHT`
- `LV_ALIGN_BOTTOM_LEFT`
- `LV_ALIGN_BOTTOM_MID`
- `LV_ALIGN_BOTTOM_RIGHT`
- `LV_ALIGN_LEFT_MID`
- `LV_ALIGN_RIGHT_MID`
- `LV_ALIGN_CENTER`

由于将子对象与其父对象的中心对齐是常用方式，因此存在一个专用函数：

```c
lv_obj_center(obj);

//同样效果
lv_obj_align(obj, LV_ALIGN_CENTER, 0, 0);
```

如果父对象的大小更改，则子对象的设置对齐方式和位置将自动更新。

上面介绍的函数将对象与其父对象对齐。但也可以将对象与任意引用对象对齐。

```c
lv_obj_align_to(obj_to_align, reference_obj, align, x, y);
```

除了上面的对齐选项外，还可以使用以下选项对齐外部的对象：

- `LV_ALIGN_OUT_TOP_LEFT`
- `LV_ALIGN_OUT_TOP_MID`
- `LV_ALIGN_OUT_TOP_RIGHT`
- `LV_ALIGN_OUT_BOTTOM_LEFT`
- `LV_ALIGN_OUT_BOTTOM_MID`
- `LV_ALIGN_OUT_BOTTOM_RIGHT`
- `LV_ALIGN_OUT_LEFT_TOP`
- `LV_ALIGN_OUT_LEFT_MID`
- `LV_ALIGN_OUT_LEFT_BOTTOM`
- `LV_ALIGN_OUT_RIGHT_TOP`
- `LV_ALIGN_OUT_RIGHT_MID`
- `LV_ALIGN_OUT_RIGHT_BOTTOM`

例如，要对齐按钮上方的标签并使标签水平居中，请执行以下操作：
```c
lv_obj_align_to(label, btn, LV_ALIGN_OUT_TOP_MID, 0, -10);
```

请注意，与`lv_obj_align（）`不同，`lv_obj_align_to（）`不能在对象坐标或参考对象坐标发生变化时重新对齐对象。

## 尺寸（Size）

### 简易方式（Simple way）

同样的，可以通过如下方式设置尺寸
```c
lv_obj_set_width(obj, 200);       //Separate...
lv_obj_set_height(obj, 100);
lv_obj_set_size(obj, 200, 100); 	//Or in one function
```

百分比值是基于父级内容区域大小计算的。例如，要将对象的高度设置为屏幕高度：
```c
lv_obj_set_height(obj, lv_pct(100));
```

尺寸大小设置支持一个特殊值: `LV_SIZE_CONTENT`. 这意味着对象在各方向上的大小将设置为其子对象的大小。

请注意，此时只会考虑右侧和底部的子项，而顶部和左侧的子项仍会被裁剪，同时，带有 `LV_OBJ_FLAG_HIDDEN` 或 `LV_OBJ_FLAG_FLOATING`的对象将被“LV_SIZE_CONTENT”计算忽略。


上述函数设置对象边界框的大小，同样的我们也可以设置内容区域的大小。 这意味着对象的边界框会因为填充的增加而扩大。
```c
lv_obj_set_content_width(obj, 50); //实际宽度：padding left + 50 + padding right

lv_obj_set_content_height(obj, 30); //实际宽度：padding top + 30 + padding bottom
```

可以使用以下函数检索边界框和内容区域的大小：
```c
lv_coord_t w = lv_obj_get_width(obj);
lv_coord_t h = lv_obj_get_height(obj);
lv_coord_t content_w = lv_obj_get_content_width(obj);
lv_coord_t content_h = lv_obj_get_content_height(obj);
```

## 使用样式（Using styles）

在底层中，位置、大小和对齐特性实际上是样式特性。
为了简单起见，上述“简单方式”没有使用与样式相关的代码，反而是在对象的本地样式中设置位置、大小和对齐特性。

但是，使用样式设置坐标有一定优势：
- 它可以方便地同时设置多个对象的宽度/高度等。例如，使所有滑块的大小为100x10像素。
- 它可以在一个位置修改值。
- 这些值可以被其他样式部分覆盖。例如，`style_btn`默认设置对象为“100x50”，但添加`style_full_width`只覆盖对象的宽度。
- 根据状态，对象可以具有不同的位置或大小,如在`LV_STATE_DEFAULT`状态下宽度为100 px ，在 `LV_STATE_PRESSED`状态下宽度为 120 px
- 样式转换使坐标更改更加平滑。 


具体的：
```c
static lv_style_t style;
lv_style_init(&style);
lv_style_set_width(&style, 100);

lv_obj_t * btn = lv_btn_create(lv_scr_act());
lv_obj_add_style(btn, &style, LV_PART_MAIN);
```

正如您将在下面看到的，LVGL还有其他一些关于大小和位置设置的强大功能。
但是，为了保持LVGL API的精简，最常见是使用简单方式的坐标设置功能，更复杂的功能可以通过样式方法来使用。

## 位置变换（Translation）

假设有3个按钮彼此相邻。其位置设置如上边的代码所述，如何实现，当按下按钮时，按钮向上移动一点？

实现这一点的一种方法是为**按下状态**设置新的Y坐标：
```c
static lv_style_t style_normal;
lv_style_init(&style_normal);
lv_style_set_y(&style_normal, 100);

static lv_style_t style_pressed;
lv_style_init(&style_pressed);
lv_style_set_y(&style_pressed, 80);

lv_obj_add_style(btn1, &style_normal, LV_STATE_DEFAULT);
lv_obj_add_style(btn1, &style_pressed, LV_STATE_PRESSED);

lv_obj_add_style(btn2, &style_normal, LV_STATE_DEFAULT);
lv_obj_add_style(btn2, &style_pressed, LV_STATE_PRESSED);

lv_obj_add_style(btn3, &style_normal, LV_STATE_DEFAULT);
lv_obj_add_style(btn3, &style_pressed, LV_STATE_PRESSED);
```

这是可行的，但它不够灵活，因为按下的坐标是硬编码的。
如果按钮不在y=100， `style_pressed` 无法正常工作，位置变换可以解决这个问题。
```c
static lv_style_t style_normal;
lv_style_init(&style_normal);
lv_style_set_y(&style_normal, 100);

static lv_style_t style_pressed;
lv_style_init(&style_pressed);
lv_style_set_translate_y(&style_pressed, -20); //Translation

lv_obj_add_style(btn1, &style_normal, LV_STATE_DEFAULT);
lv_obj_add_style(btn1, &style_pressed, LV_STATE_PRESSED);

lv_obj_add_style(btn2, &style_normal, LV_STATE_DEFAULT);
lv_obj_add_style(btn2, &style_pressed, LV_STATE_PRESSED);

lv_obj_add_style(btn3, &style_normal, LV_STATE_DEFAULT);
lv_obj_add_style(btn3, &style_pressed, LV_STATE_PRESSED);
```

位置变换是从对象的当前位置开始的，所以和初始坐标无关

百分比值也可以用于位置变换。百分比是相对于对象本身的大小而言（而不是相对于父对象的大小）。 如 `lv_pct(50)` 将以对象本身的宽度/高度的一半移动对象。

由于是在计算布局之后进行变换的，因此，可以平移已有对象的位置。

变换实际上移动了对象，这意味着它会使滚动条和`LV_SIZE_CONTENT`大小的对象对位置变化做出反应。


## 变形（Transformation）

与位置类似，对象的大小也可以相对于当前大小进行更改。
转换后的宽度和高度将添加到对象的两侧。 这意味着 10 px 变换宽度会使对象宽 2x10 像素。

与位置平移不同的是，尺寸变换不会使对象“真正”变大。 换句话说，滚动条、布局`LV_SIZE_CONTENT` 不会对转换后的大小做出反应。

因此，变换“仅”是一种视觉效果。

此代码在按下时放大按钮：
```c
static lv_style_t style_pressed;
lv_style_init(&style_pressed);
lv_style_set_transform_width(&style_pressed, 10);
lv_style_set_transform_height(&style_pressed, 10);

lv_obj_add_style(btn, &style_pressed, LV_STATE_PRESSED);
```

### 最大最小值（Min and Max size）
与 CSS 类似，LVGL 也支持 `min-width`、`max-width`、`min-height` 和 `max-height`。 这是为了防止对象大小变得比这些值更小/更大。

如果大小是按百分比或“LV_SIZE_CONTENT”设置的话，这个设置会更加有用。
```c
static lv_style_t style_max_height;
lv_style_init(&style_max_height);
lv_style_set_y(&style_max_height, 200);

lv_obj_set_height(obj, lv_pct(100));
lv_obj_add_style(obj, &style_max_height, LV_STATE_DEFAULT); //Limit the  height to 200 px
```

也可以使用相对于父内容区域大小的百分比值。
```c
static lv_style_t style_max_height;
lv_style_init(&style_max_height);
lv_style_set_y(&style_max_height, lv_pct(50));

lv_obj_set_height(obj, lv_pct(100));
lv_obj_add_style(obj, &style_max_height, LV_STATE_DEFAULT); //Limit the height to half parent height
```

## 布局（Layout）

### 概览

布局可以更新对象子项的位置和大小。 它们可用于自动将子项排列成一行或一列，或者以更复杂的形式排列。 

布局设置的位置和大小会覆盖“正常”的 x、y、宽度和高度设置。

每个布局只有一个相同的功能： `lv_obj_set_layout(obj, <LAYOUT_NAME>)` 在对象上设置布局。

有关父级和子级的更多设置，请参阅给定布局的文档。

### 内置布局（Built-in layout）
LVGL 带有两个非常强大的布局：
- Flexbox
- Grid

### 标志（Flags）
有一些标志可用于对象以影响它们在布局中的行为：
- `LV_OBJ_FLAG_HIDDEN` 在布局计算中忽略隐藏的对象。
- `LV_OBJ_FLAG_IGNORE_LAYOUT` 该对象会被布局简单地忽略。 它的坐标可以照常设置。
- `LV_OBJ_FLAG_FLOATING` 与 `LV_OBJ_FLAG_IGNORE_LAYOUT` 相同，但在 `LV_SIZE_CONTENT` 计算中将忽略带有 `LV_OBJ_FLAG_FLOATING` 的对象。

这些标志可以添加/删除 `lv_obj_add/clear_flag(obj, FLAG);`

### 添加新布局（Adding new layouts）

LVGL可以自定义布局自由扩展：
```c
uint32_t MY_LAYOUT;

...

MY_LAYOUT = lv_layout_register(my_layout_update, &user_data);

...

void my_layout_update(lv_obj_t * obj, void * user_data)
{
	/*Will be called automatically if it's required to reposition/resize the children of "obj" */	
}
```
可以添加自定义样式属性，这些属性可以在更新回调中检索和使用。 例如：

```c
uint32_t MY_PROP;
...

LV_STYLE_MY_PROP = lv_style_register_prop();

...
static inline void lv_style_set_my_prop(lv_style_t * style, uint32_t value)
{
    lv_style_value_t v = {
        .num = (int32_t)value
    };
    lv_style_set_prop(style, LV_STYLE_MY_PROP, v);
}

```

## Examples

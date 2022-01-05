```eval_rst
.. include:: /header.rst 
:github_url: |github_link_base|/overview/scroll.md
```
# 滚动（Scroll）

## 概览（Overview）

在 LVGL 中，滚动的工作非常直观：如果一个对象在其父内容区域（没有填充的大小）之外，则父对象变为可滚动并且会出现滚动条。

任何对象都可以滚动，包括 `lv_obj_t`, `lv_img`, `lv_btn`, `lv_meter`, 等等。

对象可以一次水平或垂直滚动，但是不能对角。

### 滚动条（Scrollbar）
 
#### 模式（Mode）
滚动条根据配置的“模式”显示。 存在以下“模式”：
- `LV_SCROLLBAR_MODE_OFF`  不显示滚动条
- `LV_SCROLLBAR_MODE_ON`  显示滚动条
- `LV_SCROLLBAR_MODE_ACTIVE` 在滚动时显示滚动条
- `LV_SCROLLBAR_MODE_AUTO`  当内容足够大可以滚动时显示滚动条

可以通过函数 `lv_obj_set_scrollbar_mode(obj, LV_SCROLLBAR_MODE_...)` 在对象上设置滚动条模式。


#### 样式（Styling）

滚动条有自己的专用的部件块`LV_PART_SCROLLBAR`。 例如，滚动条可以像这样变成红色：
```c
static lv_style_t style_red;
lv_style_init(&style_red);
lv_style_set_bg_color(&style_red, lv_color_red());

...

lv_obj_add_style(obj, &style_red, LV_PART_SCROLLBAR);
```
对象在滚动时进入`LV_STATE_SCROLLED`状态。 这允许在滚动时向滚动条或对象本身添加不同的样式。

例如，当对象滚动时，此代码使滚动条变为蓝色：
```c
static lv_style_t style_blue;
lv_style_init(&style_blue);
lv_style_set_bg_color(&style_blue, lv_color_blue());

...

lv_obj_add_style(obj, &style_blue, LV_STATE_SCROLLED | LV_PART_SCROLLBAR);
```

如果`LV_PART_SCROLLBAR` 的基本方向是RTL (`LV_BASE_DIR_RTL`)（向右滚动），则垂直滚动条将放置在左侧。
请注意，`base_dir` 样式属性是继承的。 因此，它可以直接设置在对象的`LV_PART_SCROLLBAR`部分，或在对象或任何父级的主要部分上使滚动条继承基本方向。


### 时间（Events）
以下事件与滚动相关：
- `LV_EVENT_SCROLL_BEGIN` 开始滚动
- `LV_EVENT_SCROLL_END` 结束滚动
- `LV_EVENT_SCROLL` 正在滚动，每次位置变化时触发。

## Basic example
TODO

## 滚动的特点（Features of scrolling）

滚动有许多有用的附加功能。

### 可滚动性（Scrollable）

可以通过函数 `lv_obj_clear_flag(obj, LV_OBJ_FLAG_SCROLLABLE)`使对象不可滚动。

不可滚动的对象仍然可以将滚动（链）传播到它们的父对象。

滚动发生的方向可以通过函数 `lv_obj_set_scroll_dir(obj, LV_DIR_...)` 控制。

方向可能有以下值：
- `LV_DIR_TOP` 只向上
- `LV_DIR_LEFT` 只向左
- `LV_DIR_BOTTOM` 只向下
- `LV_DIR_RIGHT` 只向右
- `LV_DIR_HOR` 水平滚动
- `LV_DIR_TOP` 垂直滚动
- `LV_DIR_ALL` 随意滚动

同样可以使用或表达式： `LV_DIR_TOP | LV_DIR_LEFT`。


### 滚动链（Scroll chain）

如果对象无法进一步滚动（例如其内容已到达最底部的位置），则额外的滚动会传播到其父对象。 如果父级可以在那个方向滚动，那么它将被滚动，同样的，它也继续传播给祖父母级和祖祖父母级。

滚动传播称为“滚动链接”，可以使用 `LV_OBJ_FLAG_SCROLL_CHAIN` 标志来启用/禁用。

如果禁用滚动链接，则传播将停止在对象上，并且不会传递到父对象上。

### 滚动动量（Scroll momentum）

当用户滚动对象并释放它时，LVGL可以模拟滚动的惯性动量，类似于对象被抛出并且滚动缓慢减慢的效果。

可以使用 `LV_OBJ_FLAG_SCROLL_MOMENTUM` 标志启用/禁用滚动动量。


### 弹性滚动（Elastic scroll）

通常，对象不能滚动超过其内容的末端。 即内容的顶部不能低于对象的顶部。

但是，使用 `LV_OBJ_FLAG_SCROLL_ELASTIC` 标志会在用户“滚动”内容时添加一种奇特的效果。 滚动变慢，内容可以在对象内部滚动，当对象被释放时，滚动到其中的内容将通过动画的形式回到有效位置。

### 捕捉（Snapping）

滚动结束时，可以根据特定规则捕捉对象的子项。可以使用 `LV_OBJ_FLAG_SNAPPABLE` 标志将子对象单独设置为可捕捉。 

对象可以通过四种方式捕捉子对象：
- `LV_SCROLL_SNAP_NONE` 禁用捕捉（默认）
- `LV_SCROLL_SNAP_START` 将子对象与滚动对象的左侧/顶部对齐
- `LV_SCROLL_SNAP_END` 将子对象与滚动对象的右侧/底部对齐
- `LV_SCROLL_SNAP_CENTER` 将子对象与滚动对象的中心对齐
  
捕捉对齐方式通过函数 `lv_obj_set_scroll_snap_x/y(obj, LV_SCROLL_SNAP_...)` 来设置:
 
 
滚动在底层中具体的实现过程如下：
1. 用户滚动对象并释放屏幕
2. LVGL 考虑滚动动量计算滚动结束的位置
3. LVGL 寻找最近的滚动点
4. LVGL 滚动到带动画的捕捉点

### 单次滚动（Scroll one）

单次滚动即LVGL只允许在滚动中捕捉一次子对象，这需要使子对象可捕捉并设置与`LV_SCROLL_SNAP_NONE`不同的滚动对齐对齐方式。

此功能可以通过 `LV_OBJ_FLAG_SCROLL_ONE` 标志启用。

### Scroll on focus
Imagine that there a lot of objects in a group that are on a scrollable object. Pressing the "Tab" button focuses the next object but it might be outside the visible area of the scrollable object. 
If the "scroll on focus" feature is enabled LVGL will automatically scroll objects to bring their children into view. 
The scrolling happens recursively therefore even nested scrollable objects are handled properly. 
The object will be scrolled into view even if it's on a different page of a tabview. 

## Scroll manually
The following API functions allow manual scrolling of objects:
- `lv_obj_scroll_by(obj, x, y, LV_ANIM_ON/OFF)` scroll by `x` and `y` values
- `lv_obj_scroll_to(obj, x, y, LV_ANIM_ON/OFF)` scroll to bring the given coordinate to the top left corner
- `lv_obj_scroll_to_x(obj, x, LV_ANIM_ON/OFF)` scroll to bring the given coordinate to the left side
- `lv_obj_scroll_to_y(obj, y, LV_ANIM_ON/OFF)` scroll to bring the given coordinate to the top side

## Self size

Self size is a property of an object. Normally, the user shouldn't use this parameter but if a custom widget is created it might be useful.

In short, self size establishes the size of an object's content. To understand it better take the example of a table. 
Let's say it has 10 rows each with 50 px height. So the total height of the content is 500 px. In other words the "self height" is 500 px. 
If the user sets only 200 px height for the table LVGL will see that the self size is larger and make the table scrollable.

This means not only the children can make an object scrollable but a larger self size will too.

LVGL uses the `LV_EVENT_GET_SELF_SIZE` event to get the self size of an object. Here is an example to see how to handle the event:
```c
if(event_code == LV_EVENT_GET_SELF_SIZE) {
	lv_point_t * p = lv_event_get_param(e);
  
  //If x or y < 0 then it doesn't neesd to be calculated now
  if(p->x >= 0) {
    p->x = 200;	//Set or calculate the self width
  }
  
  if(p->y >= 0) {
    p->y = 50;	//Set or calculate the self height
  }
}
```

## Examples

```eval_rst

.. include:: ../../examples/scroll/index.rst

```

```eval_rst
.. include:: /header.rst 
:github_url: |github_link_base|/overview/style.md
```
# 样式（Styles）

*Styles* 用于设置对象的外观。 lvgl 中的样式很大程度上受到 CSS 的启发。 简而言之，其概念如下：
- 样式是一个 `lv_style_t` 变量，它可以保存边框宽度、文本颜色等属性。 它类似于 CSS 中的“类”。
- 可以将样式分配给对象以更改其外观。 在赋值时，可以指定目标部分（CSS 中的*pseudo-element*）和目标状态（*pseudo class*）。例如，当滑块处于按下状态时，可以将“style_blue”样式添加到滑块的旋钮。
- 任何数量的对象都可以使用相同的样式。
- 样式可以级联，这意味着可以将多个样式分配给同一个对象，并且每个样式可以具有不同的属性。
因此，并非所有属性都必须在样式中指定。 LVGL 将搜索一个属性，直到一个样式定义它，或者如果它没有被任何样式指定，则使用默认值。
例如，`style_btn` 可以导致默认的灰色按钮，而`style_btn_red` 只能添加一个`background-color=red` 来覆盖背景颜色。

- 最近添加的样式具有更高的优先级。 这意味着如果一个属性以两种样式指定，则将使用对象中的最新样式。
- 如果未在对象中指定，某些属性（例如文本颜色）可以从父级继承。
- 对象也可以有比“正常”样式更高优先级的本地样式。
- 与 CSS（伪类描述不同的状态，例如`:focus`）不同，在 LVGL 中，一个属性被分配给给定的状态。
- 当对象改变状态时可以使用转换或者变换。


## 状态（States）
对象可以处于以下状态的组合：
- `LV_STATE_DEFAULT` (0x0000) Normal, released state
- `LV_STATE_CHECKED` (0x0001) Toggled or checked state
- `LV_STATE_FOCUSED` (0x0002) Focused via keypad or encoder or clicked via touchpad/mouse 
- `LV_STATE_FOCUS_KEY` (0x0004) Focused via keypad or encoder but not via touchpad/mouse 
- `LV_STATE_EDITED` (0x0008) Edit by an encoder
- `LV_STATE_HOVERED` (0x0010) Hovered by mouse (not supported now)
- `LV_STATE_PRESSED` (0x0020) Being pressed
- `LV_STATE_SCROLLED` (0x0040) Being scrolled
- `LV_STATE_DISABLED` (0x0080) Disabled state
- `LV_STATE_USER_1` (0x1000) Custom state
- `LV_STATE_USER_2` (0x2000) Custom state
- `LV_STATE_USER_3` (0x4000) Custom state
- `LV_STATE_USER_4` (0x8000) Custom state
  
一个对象可以同时处于聚焦和按下等状态的组合。 这表示为 `LV_STATE_FOCUSED | LV_STATE_PRESSED`. 

可以将样式添加到任何状态或状态组合。
例如，为默认状态和按下状态设置不同的背景颜色。
如果属性未在状态中定义，则将使用最佳匹配状态的属性。 通常这意味着使用属性`LV_STATE_DEFAULT`。˛
如果即使为默认状态也未设置该属性，则将使用默认值。 （见后）

但**最佳匹配属性**到底意味着什么？
状态有一个由其值显示的优先级（请参见上面的列表）。值越大，优先级越高。
为了确定要使用哪个状态的属性，让我们举一个例子。假设背景色的定义如下：
- `LV_STATE_DEFAULT`: white
- `LV_STATE_PRESSED`: gray
- `LV_STATE_FOCUSED`: red

1. 最初，对象处于默认状态，因此情况很简单：属性在对象的当前状态中完全定义为白色。
2. 按下对象时，有两个相关属性：默认为白色（默认为与每个状态相关）和按下为灰色。按下状态具有0x0020优先级，高于默认状态的0x0000优先级，因此将使用灰色。
3. 当对象被聚焦时，与按下状态下发生的情况相同，将使用红色。（选中状态的优先级高于默认状态）。
4. 当对象聚焦并按下时，灰色和红色都可以工作，但按下状态的优先级高于聚焦状态，因此将使用灰色。
5. 但也可以为设置为玫瑰色 `LV_STATE_PRESSED | LV_STATE_FOCUSED`. 在这种情况下，此组合状态的优先级为0x0020+0x0002=0x0022，高于按下状态的优先级，因此将使用玫瑰色（红加灰）。
6. 当对象处于选中状态时，没有属性可设置此状态的背景色。因此，由于没有更好的选项，对象在默认状态的属性中保持为白色。

一些实用说明：
- 状态的优先级（值）非常直观，这是用户期望的。例如，如果一个对象被聚焦，用户仍然希望看到它是否被按下，因此按下状态具有更高的优先级。如果聚焦状态具有更高的优先级，它将覆盖按下的颜色。
- 如果要为所有状态设置属性（例如，红色背景色），只需将其设置为默认状态。如果对象找不到其当前状态的属性，它将返回默认状态的属性。
- 使用OR运算符描述复杂情况的属性。（例如，按下+选中+聚焦）
- 为不同的状态使用不同的样式元素是个好习惯。例如，想要找到按下、选中+按下、聚焦、聚焦+按下、聚焦+按下、聚焦+按下+选中等状态时的背景色相当困难。相反，例如，对按下和选中的状态使用背景色，并用不同的边框颜色指示聚焦状态可能会更好。
- 
## 层叠样式（Cascading styles）
不需要在一种样式中设置所有属性。可以向对象添加更多样式，并使后来添加的样式修改或扩展外观。
例如，创建常规灰色按钮样式，并为仅设置了新背景色的红色按钮创建新样式。

这与CSS中列出的类非常相似 `<div class=".btn .btn-red">`.

以后添加的样式优先于以前设置的样式。因此，在上面的灰色/红色按钮示例中，应首先添加普通按钮样式，然后添加红色样式。
但是，仍然需要考虑到状态的优先级，比如：
- 基本按钮样式为默认状态定义深灰色，为按下状态定义浅灰色
- 红色按钮样式在默认状态下将背景颜色定义为红色

在这种情况下，当按钮被释放（处于默认状态）时，它将是红色的，因为在最近添加的样式（红色）中找到了完美匹配。 
当按钮被按下时，浅灰色是更好的匹配，因为它完美地描述了当前状态，所以按钮将是浅灰色。 

## 继承（Inheritance） 
某些属性（通常与文本相关的属性）可以从父对象的样式继承。
仅当未在对象的样式中设置给定属性时（即使在默认状态下），才应用继承。

在这种情况下，如果该属性是可继承的，则将在父对象中搜索该属性的值，直到某个对象为该属性指定了一个值。 父对象将使用自己的状态来确定值。因此，如果按下按钮，并且文本颜色来自此处，则将使用按下的文本颜色。


## 部件块（Parts）

对象由**部件块**组成，每个部件块都有自己的样式。 

LVGL中存在以下预定义的部件块：

- `LV_PART_MAIN` 像矩形的背景
- `LV_PART_SCROLLBAR`  滚动条
- `LV_PART_INDICATOR` 指示器，例如用于滑块、条、开关或复选框的勾选框
- `LV_PART_KNOB` 旋钮
- `LV_PART_SELECTED` 指示当前选择的选项或部分
- `LV_PART_ITEMS` 多个相似元素 (如图表中的单元格)
- `LV_PART_TICKS` 刻度，例如在仪表上
- `LV_PART_CURSOR` 光标，标记特定区域
- `LV_PART_CUSTOM_FIRST` 自定义


如一个滑块 [Slider](/widgets/core/slider) 有以下几个部件块:
- 背景
- 指示器
- 旋钮

这意味着滑块的三个部分都可以有自己的样式，请参见下文如何向对象和部件块添加样式。

## 初始化样式和设置/读取属性（Initialize styles and set/get properties）


样式存储在 `lv_style_t` 变量中。样式变量应为静态`static`、全局`global`或动态分配`dynamically allocated`。

换句话说，它们不能是函数中的局部变量，因为函数退出时局部变量会被销毁。

在使用样式之前，应使用`lv_style_init(&my_style)`对其进行初始化。初始化样式后，可以添加或更改特性。


使用函数 `lv_style_set_<property_name>(&style, <value>);` 来设置样式，例如：
```c
static lv_style_t style_btn; //定义样式变量
lv_style_init(&style_btn);  //初始化样式变量
lv_style_set_bg_color(&style_btn, lv_color_hex(0x115588)); //设置颜色属性
lv_style_set_bg_opa(&style_btn, LV_OPA_50);
lv_style_set_border_width(&style_btn, 2);
lv_style_set_border_color(&style_btn, lv_color_black());

static lv_style_t style_btn_red;
lv_style_init(&style_btn_red);
lv_style_set_bg_color(&style_btn_red, lv_plaette_main(LV_PALETTE_RED));
lv_style_set_bg_opa(&style_btn_red, LV_OPA_COVER);
```

如果想要删除属性可以使用如下的方式：

```c
lv_style_remove_prop(&style, LV_STYLE_BG_COLOR);
```

如果想要读取属性可以使用如下的方式：
```c
lv_style_value_t v;
lv_res_t res = lv_style_get_prop(&style, LV_STYLE_BG_COLOR, &v);
if(res == LV_RES_OK) {	/*Found*/
	do_something(v.color);
}
```

`lv_style_value_t` 有3个字段:
- `num` 是整数、布尔值和不透明度属性
- `color` 是颜色属性
- `ptr` 是指针属性

要重置样式（清除其所有数据），请使用：
```c
lv_style_reset(&style);
```

样式也可以构建为`const`以保存在RAM中：
```c
const lv_style_const_prop_t style1_props[] = {
   LV_STYLE_CONST_WIDTH(50),
   LV_STYLE_CONST_HEIGHT(50),
   LV_STYLE_PROP_INV,
};
     
LV_STYLE_CONST_INIT(style1, style1_props);
```

`const`定义的样式可以像其他样式一样使用，但（显然）不能添加新属性。

## 向部件添加和删除样式（Add and remove styles to a widget）


如果仅仅是定义一种样式并没有什么用，我们需要把样式指定给部件（widget）才能发挥它的效果。

### 添加样式（Add styles）
使用函数 `lv_obj_add_style(obj, &style, <selector>)`向一个对象（object）添加样式. `<selector>` 是一个可以通过或运算定义的参数，它代表向某个部件块的某个状态添加样式，例如：

- `LV_PART_MAIN | LV_STATE_DEFAULT`
- `LV_STATE_PRESSED`: 代表主部件被按下时 `LV_PART_MAIN` 可以省略
- `LV_PART_SCROLLBAR`: 代表默认状态时的滚动条
- `LV_STATE_DEFAULT` 可以省略
- `LV_PART_SCROLLBAR | LV_STATE_SCROLLED`: 代表对象滚动时的滚动条部分
- `0` 和 `LV_PART_MAIN | LV_STATE_DEFAULT` 的含义一样 
- `LV_PART_INDICATOR | LV_STATE_PRESSED | LV_STATE_CHECKED` 代表指示器被按下并被选中时

使用样式 `lv_obj_add_style`: 
```c
lv_obj_add_style(btn, &style_btn, 0);      				  /*按钮默认样式Default button style*/
lv_obj_add_style(btn, &btn_red, LV_STATE_PRESSED);  /*按钮按下时变为红色Overwrite only some colors to red when pressed*/
```

### 删除样式（Remove styles）
使用函数 `lv_obj_remove_style_all(obj)` 来删除对象的所有样式。

使用函数 `lv_obj_remove_style(obj, style, selector)` 来删除指定部件块、状态的样式。
此函数只会在匹配上在执行函数 `lv_obj_add_style` 时设置的 `selector` 参数之后才执行删除操作。

参数`style` 可以为 `NULL` 来达到删除 `selector` 里的所有样式


参数 `selector` 可以使用  `LV_STATE_ANY` 和 `LV_PART_ANY` 两个值来删除任何状态或者任何部件块的样式



### 报告样式变更（Report style changes）
如果已分配给对象的样式发生更改（即添加或更改属性），则应通知使用该样式的对象。 有 3 个选项可以执行此操作：
1. 如果更改的属性可以通过简单的重绘（例如更改颜色或不透明度）来更新，只需调用函数 `lv_obj_invalidate(obj)` 或 `lv_obj_invalidate(lv_scr_act())`。
2. 如果更改或添加了更复杂的样式属性, 并且知道哪些对象调用了该样式，则调用函数 `lv_obj_refresh_style(obj, part, property)`。要刷新所有部件块和属性，请使用 `lv_obj_refresh_style(obj, LV_PART_ANY, LV_STYLE_PROP_ANY)`。
3. 如果想要检查某个对象的样式是否被更改并同时刷新它们，可以调用函数`lv_obj_report_style_change(&style)`来实现。 如果 `style` 为 `NULL` 则所有对象都会收到样式被更改的报告

### 获取某个对象的属性值（Get a property's value on an object）

由于对象的样式存在级联、继承、局部样式和变换，某些时候我们可以需要获得属性的最终值，可以通过函数 `lv_obj_get_style_<property_name>(obj, <part>)` 来实现。

函数会读取对象的当前状态，如果当前状态的属性值未被更改，则返回默认值。

例如：
```c
lv_color_t color = lv_obj_get_style_bg_color(btn, LV_PART_MAIN);
```

## 局部样式（Local styles）
In addition to "normal" styles, objects can also store local styles. This concept is similar to inline styles in CSS (e.g. `<div style="color:red">`) with some modification. 

Local styles are like normal styles, but they can't be shared among other objects. If used, local styles are allocated automatically, and freed when the object is deleted.
They are useful to add local customization to an object.

Unlike in CSS, LVGL local styles can be assigned to states (*pseudo-classes*) and parts (*pseudo-elements*).

To set a local property use functions like `lv_obj_set_style_<property_name>(obj, <value>, <selector>);`  
For example:
```c
lv_obj_set_style_bg_color(slider, lv_color_red(), LV_PART_INDICATOR | LV_STATE_FOCUSED);
```
## Properties
For the full list of style properties click [here](/overview/style-props).

### Typical background properties
In the documentation of the widgets you will see sentences like "The widget uses the typical background properties". These "typical background properties" are the ones related to:
- Background
- Border
- Outline
- Shadow
- Padding
- Width and height transformation
- X and Y translation


## Transitions
By default, when an object changes state (e.g. it's pressed) the new properties from the new state are set immediately. However, with transitions it's possible to play an animation on state change.
For example, on pressing a button its background color can be animated to the pressed color over 300 ms.

The parameters of the transitions are stored in the styles. It's possible to set 
- the time of the transition
- the delay before starting the transition 
- the animation path (also known as the timing or easing function)
- the properties to animate 

The transition properties can be defined for each state. For example, setting a 500 ms transition time in the default state means that when the object goes to the default state a 500 ms transition time is applied. 
Setting a 100 ms transition time in the pressed state causes a 100 ms transition when going to the pressed state.
This example configuration results in going to the pressed state quickly and then going back to default slowly. 

To describe a transition an `lv_transition_dsc_t` variable needs to be initialized and added to a style:
```c
/*Only its pointer is saved so must static, global or dynamically allocated */
static const lv_style_prop_t trans_props[] = {
											LV_STYLE_BG_OPA, LV_STYLE_BG_COLOR,
											0, /*End marker*/
};

static lv_style_transition_dsc_t trans1;
lv_style_transition_dsc_init(&trans1, trans_props, lv_anim_path_ease_out, duration_ms, delay_ms);

lv_style_set_transition(&style1, &trans1);
```

## Color filter
TODO


## Themes
Themes are a collection of styles. If there is an active theme LVGL applies it on every created widget.
This will give a default appearance to the UI which can then be modified by adding further styles.

Every display can have a different theme. For example, you could have a colorful theme on a TFT and monochrome theme on a secondary monochrome display.

To set a theme for a display, two steps are required:
1. Initialize a theme
2. Assign the initialized theme to a display.

Theme initialization functions can have different prototypes. This example shows how to set the "default" theme:
```c
lv_theme_t * th = lv_theme_default_init(display,  /*Use the DPI, size, etc from this display*/ 
                                        LV_COLOR_PALETTE_BLUE, LV_COLOR_PALETTE_CYAN,   /*Primary and secondary palette*/
                                        false,    /*Light or dark mode*/ 
                                        &lv_font_montserrat_10, &lv_font_montserrat_14, &lv_font_montserrat_18); /*Small, normal, large fonts*/
                                        
lv_disp_set_theme(display, th); /*Assign the theme to the display*/
```


The included themes are enabled in `lv_conf.h`. If the default theme is enabled by `LV_USE_THEME_DEFAULT 1` LVGL automatically initializes and sets it when a display is created.

### Extending themes

Built-in themes can be extended. 
If a custom theme is created, a parent theme can be selected. The parent theme's styles will be added before the custom theme's styles. 
Any number of themes can be chained this way. E.g. default theme -> custom theme -> dark theme.

`lv_theme_set_parent(new_theme, base_theme)` extends the `base_theme` with the `new_theme`.

There is an example for it below.

## Examples

```eval_rst

.. include:: ../../examples/styles/index.rst

```

## API
```eval_rst

.. doxygenfile:: lv_style.h
  :project: lvgl

.. doxygenfile:: lv_theme.h
  :project: lvgl

.. doxygenfile:: lv_obj_style_gen.h
  :project: lvgl
  
.. doxygenfile:: lv_style_gen.h
  :project: lvgl
  

```

```eval_rst
.. include:: /header.rst 
:github_url: |github_link_base|/overview/object.md
```
# 对象（Objects）

在LVGL中，用户界面构建的**基本单元** 就是对象, 或者叫做 *部件（Widgets）*。

例如按钮 [Button](/widgets/core/btn), 标签 [Label](/widgets/core/label), 图片 [Image](/widgets/core/img), 列表 [List](/widgets/extra/list), 图标 [Chart](/widgets/extra/chart) 或者 文本区域 [Text area](/widgets/core/textarea)。

你可以在 对象类型 [Object types](/widgets/index) 中查看。

所有对象都使用“**lv_obj_t**”指针作为句柄引用。 此指针稍后可用于设置或获取对象的属性。

## 属性（Attributes）

### 基本属性（Basic attributes）

所有对象都有如下的基本属性:
- Position 位置
- Size 尺寸
- Parent 形状
- Styles 样式
- Event handlers 事件处理
- Etc 等等

你可以通过函数 `lv_obj_set_...` 和 `lv_obj_get_...` 来设置/获取属性. 例如:

```c
/*Set basic object attributes*/
/*设置基本属性*/
lv_obj_set_size(btn1, 100, 50);	  /*Set a button's size 设置按钮尺寸*/
lv_obj_set_pos(btn1, 20,30);      /*Set a button's position 设置按钮位置*/
```

要查看所有可用功能，请访问基础对象文档 [Base object's documentation](/widgets/obj).

### 具体属性（Specific attributes）

对象类型也有特殊的属性。 例如，一个滑块有
- 最大最小值 （Minimum and maximum values ）
- 当前值 （Current value）

对于这些特殊的属性，每个对象类型可能都有唯一的 API 函数。 例如对于滑块：

```c
/*Set slider specific attributes*/
lv_slider_set_range(slider1, 0, 100);	   				/*Set the min. and max. values 设置最大最小值*/
lv_slider_set_value(slider1, 40, LV_ANIM_ON);		/*Set the current value (position) 设置滑块当前位置（值）*/
```

 部件的具体API请查看文档 [Documentation](/widgets/index) 或者查看他们的头文件 (如 *widgets/lv_slider.h*)

## 工作机制（Working mechanisms）

### 父子结构（Parent-child structure）

父对象可以被视为其子对象的容器。 每个对象只有一个父对象（屏幕除外，它是顶层对象），但一个父对象可以有任意数量的子对象。

父对象的类型没有限制，但有些对象通常是父对象（例如：按钮）或子对象（例如：标签）。

### 同时移动（Moving together）

如果父对象的位置发生变化，子对象将随之移动。因此，所有位置都相对于父对象而言。

![](/misc/par_child1.png "Objects are moving together 1")

```c
lv_obj_t * parent = lv_obj_create(lv_scr_act());   /*Create a parent object on the current screen 将当前屏幕作为父对象*/
lv_obj_set_size(parent, 100, 80);	                 /*Set the size of the parent 设置父对象尺寸*/

lv_obj_t * obj1 = lv_obj_create(parent);	         /*Create an object on the previously created parent object 为之前的父对象创建一个子对象*/
lv_obj_set_pos(obj1, 10, 10);	                     /*Set the position of the new object 设置子对象位置*/
```

完成父子对象创建后，修改父对象位置：

![](/misc/par_child2.png "Graphical objects are moving together 2")  

```c
lv_obj_set_pos(parent, 50, 50);	/*Move the parent. The child will move with it. 子对象同父对象一起移动*/
```

（为简单起见，示例中未显示对象颜色的调整。）

### 子对象的可见性（Visibility only on the parent）

如果子对象部分或完全位于其父对象之外，则外部部分将不可见。

![](/misc/par_child3.png "A graphical object is visible on its parent")  

```c
lv_obj_set_x(obj1, -30);	/*Move the child a little bit off the parent 将子对象移动一部分出去*/
```

### 创建/删除对象（Create and delete objects）

在 LVGL 中，可以在运行时动态创建和删除对象。 这意味着只有当前创建的（现有）对象消耗 RAM。

这允许仅在单击按钮打开屏幕时创建屏幕，并在加载新屏幕时删除屏幕。

可以根据设备的当前环境创建 UI。 例如，可以根据当前连接的传感器创建仪表、图表、条形图和滑块。
 
每个部件（wedgets）的**创建（create）** 函数都有如下的原型：

```c
lv_obj_t * lv_<widget>_create(lv_obj_t * parent, <other parameters if any>);
```

通常，创建函数只有一个 **parent** 参数，告诉它们在哪个对象上创建新部件。 

creat函数的返回值是一个指向创建对象的指针，类型为 `lv_obj_t *`。

所有对象类型都有一个通用的 **delete** 函数。 它删除对象及其所有子对象。

```c
void lv_obj_del(lv_obj_t * obj);
```

- `lv_obj_del` 立即删除对象。
- `lv_obj_del_async(obj)` 异步删除，系统将在下一次调用`lv_timer_handler()`时删除对象


例如，如果要在子对象`LV_EVENT_DELETE`处理程序中删除此对象的父对象时，可以使用 `lv_obj_clean(obj)` 删除对象的所有子项（但不是对象本身），一段时间后，您可以使用 `lv_obj_del_delayed(obj, 1000)` 来删除对象。 延迟以毫秒表示。


## 屏幕（Screens）

### 创建屏幕（Create screens）

屏幕是没有父对象的特殊对象， 它们可以像这样创建：
```c
lv_obj_t * scr1 = lv_obj_create(NULL);
```

可以使用任何对象类型创建屏幕对象。 具体请看 [Base object](/widgets/obj)

### 设置活动屏幕（Get the active screen）

每个显示器上至少需要一个活动屏幕。 默认情况下，库创建并加载一个**基础对象**作为屏幕。

要获取当前活动的屏幕，请使用 `lv_scr_act()` 函数。

### 加载屏幕（Load screens）

要加载新屏幕，请使用 `lv_scr_load(scr1)`。

### 层（Layers）
系统会自动生成两个层：
- 顶层 top layer
- 系统层 system layer

它们独立于屏幕，并且存在于每个屏幕上。**顶层**在屏幕上的每个对象之上，**系统层**则在**顶层**之上。
你可以随意将窗口添加到**顶层**上。 但是，**系统层**仅限于系统级的东西 (如鼠标光标设置 `lv_indev_set_cursor()`).

`lv_layer_top()` 和 `lv_layer_sys()` 函数分别返回指向顶层和系统层的指针。

具体请查看 [Layer overview](/overview/layer)。

#### 用动画加载屏幕（Load screen with animation）

如果想用动画的方式加载新屏幕，可以使用函数 `lv_scr_load_anim(scr, transition_type, time, delay, auto_del)`. 

共有如下几种方式：
- `LV_SCR_LOAD_ANIM_NONE` 在 `delay` 毫秒后立即切换
- `LV_SCR_LOAD_ANIM_OVER_LEFT/RIGHT/TOP/BOTTOM` 将新屏幕按指定的方向移动
- `LV_SCR_LOAD_ANIM_MOVE_LEFT/RIGHT/TOP/BOTTOM` 新屏幕和当前屏幕都按指定的方向移动
- `LV_SCR_LOAD_ANIM_FADE_ON` 将新屏幕淡入旧屏幕

如果将`auto_del`设置为`true`，系统将在动画完成时自动删除旧屏幕。

在动画开始`delay`时间后，新屏幕将变为活动状态（由`lv_scr_act（）`返回）。

### 多显示器处理（Handling multiple display）

系统在 **当前显示屏（default display）** 上创建显示， **当前显示屏（default display）** 由函数 `lv_disp_drv_register`注册。 可以通过函数 `lv_disp_set_default(disp)`显式的创建新的 **当前显示屏（default display）** 。

`lv_scr_act()`, `lv_scr_load()` 和 `lv_scr_load_anim()` 都是在 **当前显示屏（default display）** 上进行的

具体请看 [Multi-display support](/overview/display) 

## 部件块（Parts）

部件由多个部件块组成。 如 [Base object](/widgets/obj) 使用主控件（main）和滚动条（scrollbar）控件， [Slider](/widgets/core/slider) 使用主控件（main）、指示器（indicator）和旋钮（knob）部件。 


部件块（Parts）就像CSS中的 *pseudo-elements* 


LVGL中存在以下预定义的部件块（Parts）：

- `LV_PART_MAIN` 像矩形的背景
- `LV_PART_SCROLLBAR`  滚动条
- `LV_PART_INDICATOR` 指示器，例如用于滑块、条、开关或复选框的勾选框
- `LV_PART_KNOB` 旋钮
- `LV_PART_SELECTED` 指示当前选择的选项或部分
- `LV_PART_ITEMS` 多个相似元素 (如图表中的单元格)
- `LV_PART_TICKS` 刻度，例如在仪表上
- `LV_PART_CURSOR` 光标，标记特定区域
- `LV_PART_CUSTOM_FIRST` 自定义

部件块的主要目的是允许为部件的“组件”设置样式，具体请看 [Style overview](/overview/style) .

## 状态（States）

对象可以有以下一种或多种状态：

- `LV_STATE_DEFAULT` 正常、释放状态
- `LV_STATE_CHECKED` 切换或选中状态
- `LV_STATE_FOCUSED` 被键盘、编码器或触摸板/鼠标选中
- `LV_STATE_FOCUS_KEY` 通过键盘或编码器选中，但不通过触摸板/鼠标选中
- `LV_STATE_EDITED` 由编码器编辑
- `LV_STATE_HOVERED` 鼠标悬停（暂不支持）
- `LV_STATE_PRESSED` 点击中
- `LV_STATE_SCROLLED` 滚动中
- `LV_STATE_DISABLED` 去使能状态
- `LV_STATE_USER_1` 用户定义
- `LV_STATE_USER_2` 用户定义
- `LV_STATE_USER_3` 用户定义
- `LV_STATE_USER_4` 用户定义

当用户与对象交互（按下、释放、选中等）时，LVGL通常会自动更改状态，当然也可以手动更改。

设置或清除给定状态（但保持其他状态不变）使用`lv_obj_add/clear_state(obj, LV_STATE_...)`

可以使用或运算符同时赋值两个状态 `lv_obj_add_state(obj, part, LV_STATE_PRESSED | LV_PRESSED_CHECKED)`.

具体请参阅 [Style overview](/overview/style).

## 截屏（Snapshot）
可以为对象及其子对象生成截屏图像。 具体请参阅[Snapshot](/others/snapshot).

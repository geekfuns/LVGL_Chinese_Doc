```eval_rst
.. include:: /header.rst 
:github_url: |github_link_base|/overview/event.md
```
# 事件（Events）

当对象被操作时，LVGL 中会触发事件，例如 当一个对象
- 被点击
- 滚动
- 改变值
- 重绘等

## 给对象添加事件（Add events to the object）

用户可以为对象分配回调函数以查看其事件，比如：
```c
lv_obj_t * btn = lv_btn_create(lv_scr_act());
lv_obj_add_event_cb(btn, my_event_cb, LV_EVENT_CLICKED, NULL);   /*注册回调函数Assign an event callback*/

...

static void my_event_cb(lv_event_t * event)
{
    printf("Clicked\n");
}
```


在这个例子中， `LV_EVENT_CLICKED` 表示只有在对象被点击时才会触发回调函数`my_event_cb`. 具体请看 [list of event codes](#event-codes)
`LV_EVENT_ALL` 则表示接受所有事件。

`lv_obj_add_event_cb` 的最后一个参数是指向事件中可用的回调函数的指针，稍后将更详细地描述。


可以向一个对象添加多个事件，如下所示：

```c
lv_obj_add_event_cb(obj, my_event_cb_1, LV_EVENT_CLICKED, NULL);
lv_obj_add_event_cb(obj, my_event_cb_2, LV_EVENT_PRESSED, NULL);
lv_obj_add_event_cb(obj, my_event_cb_3, LV_EVENT_ALL, NULL);		/*No filtering, receive all events*/
```

即使是相同事件的处理也可以用于具有不同 `user_data` 的回调函数。 例如：
```c
lv_obj_add_event_cb(obj, increment_on_click, LV_EVENT_CLICKED, &num1);
lv_obj_add_event_cb(obj, increment_on_click, LV_EVENT_CLICKED, &num2);
```

这些事件将按照添加的顺序被调用，其他对象可以使用相同的*回调函数*。



## 从对象中删除事件（Remove event(s) from an object）

使用函数 `lv_obj_remove_event_cb(obj, event_cb)` 或者 `lv_obj_remove_event_dsc(obj, event_dsc)` 来删除对象中的事件。 `event_dsc` 是在执行函数 `lv_obj_add_event_cb` 时所用的参数。

## 事件代码（Event codes）

事件代码可以分为以下几类：
- 输入设备事件 Input device events
- 绘图事件 Drawing events
- 其他事件 Other events
- 特殊事件 Special events
- 自定义事件 Custom events

所有对象（例如按钮/标签/滑块等），无论其类型如何，都会接收 *Input device*、*Drawing* 和 *Other* 事件。

*特殊事件* 只存在与某些部件类型， 具体请看 [widgets' documentation](/widgets/index) 

*自定义事件* 由用户添加，LVGL不会产生此事件。

LVGL定义了如下事件代码：

### Input device events
- `LV_EVENT_PRESSED`      对象被按下
- `LV_EVENT_PRESSING`     正在按下一个对象（按下时连续调用）
- `LV_EVENT_PRESS_LOST`   一个对象仍在被按下，但光标/手指已离开该对象（松手）
- `LV_EVENT_SHORT_CLICKED`  一个对象被压了一小段时间，然后被释放。如果是在滚动则不会调用。
- `LV_EVENT_LONG_PRESSED` 至少在输入设备驱动程序中指定的 `long_press_time` 时间内按下了一个对象。 如果是在滚动则不会调用。
- `LV_EVENT_LONG_PRESSED_REPEAT`  在每个 `long_press_repeat_time` 毫秒的 `long_press_time` 之后调用。 如果是在滚动则不会调用。
- `LV_EVENT_CLICKED`      如果对象没有滚动，则在释放时调用（无论是否长按）
- `LV_EVENT_RELEASED`     当一个对象被释放时，在任何情况下都调用
- `LV_EVENT_SCROLL_BEGIN` 滚动开始。 事件参数是 `NULL` 或带有滚动动画描述符的 `lv_anim_t *`，如果需要可以修改。
- `LV_EVENT_SCROLL_END`   滚动结束。
- `LV_EVENT_SCROLL`       对象滚动过了
- `LV_EVENT_GESTURE`      检测到手势。 通过函数获取手势 `lv_indev_get_gesture_dir(lv_indev_get_act());`
- `LV_EVENT_KEY`          A key is sent to an object. Get the key with `lv_indev_get_key(lv_indev_get_act());`
- `LV_EVENT_FOCUSED`      一个对象被聚焦
- `LV_EVENT_DEFOCUSED`    对象未聚焦
- `LV_EVENT_LEAVE`        对象未聚焦但仍被选中
- `LV_EVENT_HIT_TEST`     执行高级命中测试。 使用 `lv_hit_test_info_t * a = lv_event_get_hit_test_info(e)` 并且检查 `a->point` 是否可以点击对象， 如果没有设置则 `a->res = false` 


### 绘图事件（Drawing events）
- `LV_EVENT_COVER_CHECK` 检查物体是否完全覆盖了一个区域。 事件参数是 `lv_cover_check_info_t *`.
- `LV_EVENT_REFR_EXT_DRAW_SIZE`  Get the required extra draw area around an object (e.g. for a shadow). The event parameter is `lv_coord_t *` to store the size. Only overwrite it with a larger value.
- `LV_EVENT_DRAW_MAIN_BEGIN` Starting the main drawing phase.
- `LV_EVENT_DRAW_MAIN`   Perform the main drawing
- `LV_EVENT_DRAW_MAIN_END`   Finishing the main drawing phase
- `LV_EVENT_DRAW_POST_BEGIN` Starting the post draw phase (when all children are drawn)
- `LV_EVENT_DRAW_POST`   Perform the post draw phase (when all children are drawn)
- `LV_EVENT_DRAW_POST_END`   Finishing the post draw phase (when all children are drawn)
- `LV_EVENT_DRAW_PART_BEGIN` Starting to draw a part. The event parameter is `lv_obj_draw_dsc_t *`. Learn more [here](/overview/drawing).
- `LV_EVENT_DRAW_PART_END`   Finishing to draw a part. The event parameter is `lv_obj_draw_dsc_t *`. Learn more [here](/overview/drawing).

### 其他事件（Other events）
- `LV_EVENT_DELETE`       删除对象
- `LV_EVENT_CHILD_CHANGED`    添加/删除子对象
- `LV_EVENT_CHILD_CREATED`    创建了新子对象，并通知所有父对象
- `LV_EVENT_CHILD_DELETED`    删除了新子对象，并通知所有父对象
- `LV_EVENT_SIZE_CHANGED`    对象坐标/大小已更改
- `LV_EVENT_STYLE_CHANGED`    对象的样式已更改
- `LV_EVENT_BASE_DIR_CHANGED` 基本目录已更改
- `LV_EVENT_GET_SELF_SIZE`   获取部件块的内部大小
- `LV_EVENT_SCREEN_UNLOAD_START` 屏幕开始删除，当 lv_scr_load/lv_scr_load_anim 被调用时立即触发
- `LV_EVENT_SCREEN_LOAD_START` 屏幕加载开始，当屏幕更改延迟到期时触发
- `LV_EVENT_SCREEN_LOADED`    屏幕已加载，在所有动画完成时调用
- `LV_EVENT_SCREEN_UNLOADED`  屏幕已删除，在所有动画完成时调用

### 特殊事件（Special events）
- `LV_EVENT_VALUE_CHANGED`    对象的值已更改（如滑块移动）
- `LV_EVENT_INSERT`      文本被插入到对象中。 事件数据是插入的`char *`。
- `LV_EVENT_REFRESH`      通知对象刷新其上的某些内容（对于用户）
- `LV_EVENT_READY`        一个进程已经完成
- `LV_EVENT_CANCEL`       一个进程被取消

### 自定义事件（Custom events）
Any custom event codes can be registered by `uint32_t MY_EVENT_1 = lv_event_register_id();` 

They can be sent to any object with `lv_event_send(obj, MY_EVENT_1, &some_data)`

## 手动发送事件（Sending events）


要手动向对象发送事件，请使用 `lv_event_send(obj, <EVENT_CODE> &some_data)`。

例如，这可用于通过模拟按钮按下来手动关闭消息框（尽管有更简单的方法可以做到这一点）：
```c
/*Simulate the press of the first button (indexes start from zero)*/
uint32_t btn_id = 0;
lv_event_send(mbox, LV_EVENT_VALUE_CHANGED, &btn_id);
```

### 刷新事件（Refresh event）

`LV_EVENT_REFRESH` 是一个特殊事件，因为它旨在让用户通知对象刷新自身。 如：
- 通知标签根据一个或多个变量（例如当前时间）刷新其文本
- 当语言改变时刷新标签
- 如果满足某些条件（例如输入正确的 PIN），则启用按钮
- 如果超出限制，则向/从对象添加/删除样式等

## Fields of lv_event_t

`lv_event_t` is the only parameter passed to the event callback and it contains all data about the event. The following values can be gotten from it:
- `lv_event_get_code(e)` get the event code
- `lv_event_get_current_target(e)` get the object to which an event was sent. I.e. the object whose event handler is being called.
- `lv_event_get_target(e)` get the object that originally triggered the event (different from `lv_event_get_target` if [event bubbling](#event-bubbling) is enabled)
- `lv_event_get_user_data(e)` get the pointer passed as the last parameter of `lv_obj_add_event_cb`.
- `lv_event_get_param(e)` get the parameter passed as the last parameter of `lv_event_send` 


## Event bubbling

If `lv_obj_add_flag(obj, LV_OBJ_FLAG_EVENT_BUBBLE)` is enabled all events will be sent to an object's parent too. If the parent also has `LV_OBJ_FLAG_EVENT_BUBBLE` enabled the event will be sent to its parent and so on. 

The *target* parameter of the event is always the current target object, not the original object. To get the original target call `lv_event_get_original_target(e)` in the event handler.  



## Examples

```eval_rst

.. include:: ../../examples/event/index.rst

```

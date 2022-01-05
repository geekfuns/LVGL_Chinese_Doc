```eval_rst
.. include:: /header.rst 
:github_url: |github_link_base|/overview/layer.md
```

# 层（Layers）

## 创建顺序（Order of creation）

默认情况下，LVGL在旧对象之上绘制新对象。

例如，假设我们将一个按钮添加到名为button1的父对象，然后再添加另一个名为button2的按钮。然后button1（及其子对象）将位于背景中，并且可以被button2及其子对象覆盖。


![](/misc/layers.png "Creating graphical objects in LVGL")  

```c
/*Create a screen*/
lv_obj_t * scr = lv_obj_create(NULL, NULL);
lv_scr_load(scr);          /*Load the screen*/

/*Create 2 buttons*/
lv_obj_t * btn1 = lv_btn_create(scr, NULL);         /*Create a button on the screen*/
lv_btn_set_fit(btn1, true, true);                   /*Enable automatically setting the size according to content*/
lv_obj_set_pos(btn1, 60, 40);              	   /*Set the position of the button*/

lv_obj_t * btn2 = lv_btn_create(scr, btn1);         /*Copy the first button*/
lv_obj_set_pos(btn2, 180, 80);                    /*Set the position of the button*/

/*Add labels to the buttons*/
lv_obj_t * label1 = lv_label_create(btn1, NULL);	/*Create a label on the first button*/
lv_label_set_text(label1, "Button 1");          	/*Set the text of the label*/

lv_obj_t * label2 = lv_label_create(btn2, NULL);  	/*Create a label on the second button*/
lv_label_set_text(label2, "Button 2");            	/*Set the text of the label*/

/*Delete the second label*/
lv_obj_del(label2);
```

## 将对象置于上层（Bring to the foreground）

有四种显式方法可以改变对象所在的层：
- 函数 `lv_obj_move_foreground(obj)` 将对象置于最上层，同样的，使用函数 `lv_obj_move_background(obj)` 将对象置于最下层。
- 函数 `lv_obj_move_up(obj)` 将对象向上移动一层，同样的，使用函数 `lv_obj_move_down(obj)` 将对象向下移动一层。
- 函数 `lv_obj_swap(obj1, obj2)` 将两个对象的层交换。
- 函数 `lv_obj_set_parent(obj, new_parent)` 将对象 `obj` 置于 `new_parent` 之上。


## 顶层与系统层（Top and sys layers）

LVGL 使用名为“layer_top”和“layer_sys”的两个特殊层。两者在显示器的所有屏幕上都是可见的和通用的。 **但是，它们不会在多个物理显示器之间共享。** 

`layer_top` 始终位于默认屏幕（`lv_scr_act()`）的上一层，而 `layer_sys` 位于 `layer_top` 的上一层。

用户可以使用 `layer_top` 来创建一些可见的内容。 例如，一个菜单栏、一个弹出窗口等。如果启用了`click` 属性，那么`layer_top` 将吸收所有用户点击并充当模态。

```c
lv_obj_add_flag(lv_layer_top(), LV_OBJ_FLAG_CLICKABLE);
```

`layer_sys` 在 LVGL 中也用于类似的目的。 例如，它将鼠标光标放在所有图层上方以确保它始终可见。

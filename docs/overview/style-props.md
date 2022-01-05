# 样式属性（Style properties）

## 大小与位置（Size and position）

Properties related to size, position, alignment and layout of the objects.
与对象的大小、位置、对齐方式和布局相关的属性。

### 宽度（width）

可以使用像素、百分比和`LV_SIZE_CONTENT`来设置对象的宽度。百分比值与父对象的内容区域的宽度相关。


<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>默认值（Default）</strong> 与部件相关（Widget dependent）</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 最小宽度（min_width）
设置最小宽度。可以使用像素值和百分比值。百分比值与父对象的内容区域的宽度相关。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 最大宽度（max_width）
设置最大宽度。可以使用像素值和百分比值。百分比值与父对象的内容区域的宽度相关。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> LV_COORD_MAX</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 高度（height）
可以使用像素、百分比和`LV_SIZE_CONTENT`来设置对象的高度。百分比值与父对象的内容区域的宽度相关。

<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> Widget dependent</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 最小高度（min_height）
设置最小高度。可以使用像素值和百分比值。百分比值与父对象的内容区域的宽度相关。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 最大高度（max_height）
设置最大高度。可以使用像素值和百分比值。百分比值与父对象的内容区域的宽度相关。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> LV_COORD_MAX</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### x
设置对象的x坐标，并需要考虑对齐  `align` 的方式. 可以使用像素值和百分比值。百分比值与父对象的内容区域的宽度相关。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### y
设置对象的y坐标，并需要考虑对齐  `align` 的方式. 可以使用像素值和百分比值。百分比值与父对象的内容区域的宽度相关。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 对齐方式（align）
设置对齐方式，该对齐方式告诉应从父节点的哪个位置来作为 X 和 Y 坐标的原点。 共有以下几种选项: `LV_ALIGN_DEFAULT`, `LV_ALIGN_TOP_LEFT/MID/RIGHT`, `LV_ALIGN_BOTTOM_LEFT/MID/RIGHT`, `LV_ALIGN_LEFT/RIGHT_MID`, `LV_ALIGN_CENTER`。

`LV_ALIGN_DEFAULT` 代表 `LV_ALIGN_TOP_LEFT` ，即以左上角作为原点，`LV_ALIGN_TOP_RIGHT` 则是右上角作为原点。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_ALIGN_DEFAULT`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 变换宽度（transform_width）
此值会使对象变得更宽。 可以使用像素和百分比（`lv_pct(x)`）值。 百分比值相对于对象的宽度而言。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> Yes</li>
</ul>

### 变换高度（transform_height）
此值会使对象在两侧变得更高。 可以使用像素和百分比（`lv_pct(x)`）值。 百分比值相对于对象的高度而言。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> Yes</li>
</ul>

### 变换x坐标（translate_x）
沿x方向移动对象，在完成布局、对齐和其他定位之后使用。 可以使用像素和百分比（使用 `lv_pct(x)`）值。 百分比值相对于对象的宽度而言。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 变换y坐标（translate_y）
沿y方向移动对象，在完成布局、对齐和其他定位之后使用。 可以使用像素和百分比（使用 `lv_pct(x)`）值。 百分比值相对于对象的高度而言。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 缩放（transform_zoom）
与图片缩放类似， 与对象的属性进行相乘。256(或者 `LV_IMG_ZOOM_NONE`)是普通尺寸，128则缩放到原来的一半，512则是放大一倍，以此类推。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> Yes</li>
</ul>

### 变换角度（transform_angle）
与旋转图片类似类似地旋转对象，1代表0.1度，则 45 度 = 450 
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> Yes</li>
</ul>

## 填充（Padding）
用于描述父级与子级之间以及子级之间间距的属性。 非常类似于 HTML 中的填充属性。

### 顶部填充（pad_top）
在顶部设置填充。 它使这个方向的内容区域变得更小。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 底部填充（pad_bottom）
在底部设置填充。 它使这个方向的内容区域变得更小。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 左部填充（pad_left）
在左部设置填充。 它使这个方向的内容区域变得更小。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 右部填充（pad_right）
在右部设置填充。 它使这个方向的内容区域变得更小。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 行填充（pad_row）
设置行之间的填充。 在布局时使用。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 列填充（pad_column）
设置列之间的填充。 在布局时使用
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

## 背景（Background）
用于描述对象的背景颜色和图像的属性。

### 背景颜色（bg_color）
Set the background color of the object.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `0xffffff`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### bg_opa
设置背景的不透明度。`0` , `LV_OPA_0` 或 `LV_OPA_TRANSP` 意味着完全透明, `256` , `LV_OPA_100` 或 `LV_OPA_COVER` 意味着不透明, 其他值或 `LV_OPA_10`、`LV_OPA_20` 等表示半透明。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_OPA_TRANSP` </li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 渐变色（bg_grad_color）
设置背景的渐变色. 只有在 `grad_dir` 不是 `LV_GRAD_DIR_NONE` 时生效。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `0x000000`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 渐变方向（bg_grad_dir）
设置背景渐变的方向. 有三个值 `LV_GRAD_DIR_NONE/HOR/VER`.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_GRAD_DIR_NONE`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 背景色起点（bg_main_stop）
设置背景色的起点。 0 表示顶部/左侧，255 表示底部/右侧，128 表示中心，依此类推
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 背景渐变色起点（bg_grad_stop）
设置背景渐变颜色的起点。 0 表示顶部/左侧，255 表示底部/右侧，128 表示中心，依此类推
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 255</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 背景图片（bg_img_src）
设置背景图像。 可以是指向 `lv_img_dsc_t` 的指针、文件路径或`LV_SYMBOL_...`
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `NULL`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> Yes</li>
</ul>

### 背景图片透明度（bg_img_opa）
设置背景图片的不透明度。`0` , `LV_OPA_0` 或 `LV_OPA_TRANSP` 意味着完全透明, `256` , `LV_OPA_100` 或 `LV_OPA_COVER` 意味着不透明, 其他值或 `LV_OPA_10`、`LV_OPA_20` 等表示半透明。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_OPA_COVER`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 背景图片重新着色（bg_img_recolor）
设置一种颜色以混合到背景图像。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `0x000000`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 背景图片重新着色强度（bg_img_recolor_opa）
设置背景图像重新着色的强度。 值 0、`LV_OPA_0` 或 `LV_OPA_TRANSP` 表示不混合，256、`LV_OPA_100` 或 `LV_OPA_COVER` 表示完全重新着色，其他值或 LV_OPA_10、LV_OPA_20 等按比例解释。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_OPA_TRANSP`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 背景图片平铺（bg_img_tiled）
如果启用，背景图像将被平铺。 可能的值为“true”或“false”。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

## 边框（Border）
描述边狂的属性

### 边框颜色（border_color）
Set the color of the border
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `0x000000`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 边框不透明度（border_opa）
设置边框的不透明度。 `0` , `LV_OPA_0` 或 `LV_OPA_TRANSP` 意味着完全透明, `256` , `LV_OPA_100` 或 `LV_OPA_COVER` 意味着不透明, 其他值或 `LV_OPA_10`、`LV_OPA_20` 等表示半透明。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_OPA_COVER`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### 边框宽度（border_width）
设置边框的宽度。 只能使用像素值。
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### border_side
Set ony which side(s) the border should be drawn. The possible values are `LV_BORDER_SIDE_NONE/TOP/BOTTOM/LEFT/RIGHT/INTERNAL`. OR-ed calues an be used as well, e.g. `LV_BORDER_SIDE_TOP | LV_BORDER_SIDE_LEFT`.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_BORDER_SIDE_NONE`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### border_post
Sets whether the the border should be drawn before or after the children ar drawn. `true`: after children, `false`: before children
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

## Outline
Properties to describe the outline. It's like a border but drawn outside of the rectangles.

### outline_width
Set the width of the outline in pixels. 
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> Yes</li>
</ul>

### outline_color
Set the color of the outline.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `0x000000`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### outline_opa
Set the opacity of the outline. Value 0, `LV_OPA_0` or `LV_OPA_TRANSP` means fully transparent, 256, `LV_OPA_100` or `LV_OPA_COVER` means fully covering, other values or LV_OPA_10, LV_OPA_20, etc means semi transparency.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_OPA_COVER`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> Yes</li>
</ul>

### outline_pad
Set the padding of the outline, i.e. the gap between object and the outline.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> Yes</li>
</ul>

## Shadow
Properties to describe the shadow drawn under the rectangles.

### shadow_width
Set the width of the shadow in pixels. The value should be >= 0.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> Yes</li>
</ul>

### shadow_ofs_x
Set an offset on the shadow in pixels in X direction. 
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> Yes</li>
</ul>

### shadow_ofs_y
Set an offset on the shadow in pixels in Y direction. 
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> Yes</li>
</ul>

### shadow_spread
Make the shadow calcuation to use a larger or smaller rectangle as base. The value can be in pixel to make the area larger/smaller
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> Yes</li>
</ul>

### shadow_color
Set the color of the shadow
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `0x000000`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### shadow_opa
Set the opacity of the shadow. Value 0, `LV_OPA_0` or `LV_OPA_TRANSP` means fully transparent, 256, `LV_OPA_100` or `LV_OPA_COVER` means fully covering, other values or LV_OPA_10, LV_OPA_20, etc means semi transparency.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_OPA_COVER`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> Yes</li>
</ul>

## Image
Properties to describe the images

### img_opa
Set the opacity of an image. Value 0, `LV_OPA_0` or `LV_OPA_TRANSP` means fully transparent, 256, `LV_OPA_100` or `LV_OPA_COVER` means fully covering, other values or LV_OPA_10, LV_OPA_20, etc means semi transparency.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_OPA_COVER`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### img_recolor
Set color to mixt to the image.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `0x000000`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### img_recolor_opa
Set the intensity of the color mixing. Value 0, `LV_OPA_0` or `LV_OPA_TRANSP` means fully transparent, 256, `LV_OPA_100` or `LV_OPA_COVER` means fully covering, other values or LV_OPA_10, LV_OPA_20, etc means semi transparency.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

## Line
Properties to describe line-like objects

### line_width
Set the width of the lines in pixel.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> Yes</li>
</ul>

### line_dash_width
Set the width of dashes in pixel. Note that dash works only on horizontal and vertical lines
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### line_dash_gap
Set the gap between dashes in pixel. Note that dash works only on horizontal and vertical lines
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### line_rounded
Make the end points of the lines rounded. `true`: rounded, `false`: perpendicular line ending 
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### line_color
Set the color fo the lines.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `0x000000`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### line_opa
Set the opacity of the lines.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_OPA_COVER`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

## Arc
TODO

### arc_width
Set the width (ticjkness) of the arcs in pixel.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> Yes</li>
</ul>

### arc_rounded
Make the end points of the arcs rounded. `true`: rounded, `false`: perpendicular line ending 
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### arc_color
Set the color of the arc.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `0x000000`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### arc_opa
Set the opacity of the arcs.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_OPA_COVER`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### arc_img_src
Set an image from which the arc will be masked out. It's useful to display complex effects on the arcs. Can be a pointer to `lv_img_dsc_t` or a path to a file
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `NULL`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

## Text
Properties to describe the propeties of text. All these properties are inherited.

### text_color
Sets the color of the text.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `0x000000`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### text_opa
Set the opacity of the text. Value 0, `LV_OPA_0` or `LV_OPA_TRANSP` means fully transparent, 256, `LV_OPA_100` or `LV_OPA_COVER` means fully covering, other values or LV_OPA_10, LV_OPA_20, etc means semi transparency.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_OPA_COVER`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### text_font
Set the font of the text (a pointer `lv_font_t *`). 
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_FONT_DEFAULT`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### text_letter_space
Set the letter space in pixels
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### text_line_space
Set the line space in pixels.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### text_decor
Set decoration for the text. The possible values are `LV_TEXT_DECOR_NONE/UNDERLINE/STRIKETHROUGH`. OR-ed values can be used as well.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_TEXT_DECOR_NONE`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### text_align
Set how to align the lines of the text. Note that it doesn't align the object itself, only the lines inside the object. The possible values are `LV_TEXT_ALIGN_LEFT/CENTER/RIGHT/AUTO`. `LV_TEXT_ALIGN_AUTO` detect the text base direction and uses left or right alignment accordingly
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_TEXT_ALIGN_AUTO`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

## Miscellaneous
Mixed proprites for various purposes.

### radius
Set the radius on every corner. The value is interpreted in pixel (>= 0) or `LV_RADIUS_CIRCLE` for max. radius
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### clip_corner
Enable to clip the overflowed content on the rounded corner. Can be `true` or `false`.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### opa
Scale down all opacity values of the object by this factor. Value 0, `LV_OPA_0` or `LV_OPA_TRANSP` means fully transparent, 256, `LV_OPA_100` or `LV_OPA_COVER` means fully covering, other values or LV_OPA_10, LV_OPA_20, etc means semi transparency.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_OPA_COVER`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### color_filter_dsc
Mix a color to all colors of the object.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `NULL`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### color_filter_opa
The intensity of mixing of color filter.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_OPA_TRANSP`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### anim_time
The animation time in milliseconds. It's meaning is widget specific. E.g. blink time of the cursor on the text area or scroll time of a roller. See the widgets' documentation to learn more.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### anim_speed
The animation speed in pixel/sec. It's meaning is widget specific. E.g. scroll speed of label. See the widgets' documentation to learn more.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### transition
An initialized `lv_style_transition_dsc_t` to describe a transition.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `NULL`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### blend_mode
Describes how to blend the colors to the background. The possibel values are `LV_BLEND_MODE_NORMAL/ADDITIVE/SUBTRACTIVE/MULTIPLY`
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_BLEND_MODE_NORMAL`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### layout
Set the layout if the object. The children will be repositioned and resized according to the policies set for the layout. For the possible values see the documentation of the layouts.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> 0</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> No</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

### base_dir
Set the base direction of the obejct. The possible values are `LV_BIDI_DIR_LTR/RTL/AUTO`.
<ul>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Default</strong> `LV_BASE_DIR_AUTO`</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Inherited</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Layout</strong> Yes</li>
<li style='display:inline; margin-right: 20px; margin-left: 0px'><strong>Ext. draw</strong> No</li>
</ul>

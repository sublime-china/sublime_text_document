# [SUBLIME TEXT中文文档之](index)模板详解

Sublime Text界面的外观由模板控制。术语*模板*严格指UI的外观 - 按钮，选择列表，侧边栏，标签等。源代码，标记和散文的突出显示由[颜色方案](color_schemes)控制 。

Sublime Text的模板引擎基于光栅图形。PNG用于防止纹理退化并提供完整的alpha控制。UI中的每个元素最多可以应用四层纹理或填充，并具有控制不透明度和填充的属性。可以根据用户交互和设置有条件地更改在每个元素上设置的属性。

Sublime Text模板通过.sublime-theme格式实现。它是一种JSON格式，用于指定匹配元素和修改其外观的规则。

*   [示例](themes#example)
*   [术语](themes#terminology)
*   [一般信息](themes#general_information)
*   [特性](themes#attributes)
*   [设置](themes#settings)
*   [属性](themes#properties)
*   [分子](themes#elements)
*   [弃用](themes#deprecated)
*   [过时的](themes#obsolete)
*   [定制](themes#customization)

## 示例

*以下是.sublime-theme文件格式的示例 。完整的模板将包含更多规则，以涵盖UI中使用的所有元素。*

~~~js
{
    // Set up the textures for a button
    {
        "class": "button_control",
        "layer0.tint": [0, 0, 0],
        "layer0.opacity": 1.0,
        "layer1.texture": "Theme - Example/textures/button_background.png",
        "layer1.inner_margin": 4,
        "layer1.opacity": 1.0,
        "layer2.texture": "Theme - Example/textures/button_highlight.png",
        "layer2.inner_margin": 4,
        "layer2.opacity": 0.0,
        "content_margin": [4, 8, 4, 8]
    },
    // Show the highlight texture when the button is hovered
    {
        "class": "button_control",
        "attributes": ["hover"],
        "layer2.opacity": 1.0
    },
    // Basic text label style
    {
        "class": "label_control",
        "fg": [240, 240, 240],
        "font.bold": true
    },
    // Brighten labels contained in a button on hover
    {
        "class": "label_control",
        "parents": [{"class": "button_control", "attributes": ["hover"]}],
        "fg": [255, 255, 255]
    }
}
~~~

## 术语

模板是JSON对象数组，称为*规则*。每个规则对象都包含`class`用于匹配元素的键。除`class`，匹配可以进一步通过指定的限制`attributes`，`settings`，`parents`和`platforms`密钥。*属性*会影响元素的外观或行为。

大多数元素都有一个类名，尽管少数元素有多个元素允许通用和特定样式。例如，`popup_control`该类可用于为自动完成和HTML弹出窗口设置样式，但`popup_control auto_complete_popup`可用于仅定位自动完成弹出窗口。多个`class`值由空格分隔。当规则指定多个类名时，所有元素必须存在于要应用的规则的元素上。

`attributes`由Sublime Text设置，并指示用户交互的状态或有关元素性质的其他信息。该值是一个字符串数组。实例包括`"hover"`，`"pressed"`和`"dirty"`。

`settings`是用户控制的值，可以在运行时更改。该值是一个字符串数组，它是从`.sublime-settings`文件中提取的布尔设置的名称 。要检查`false`值，请在设置名称前加上`!`模板，也可以创建自己的设置以允许用户更改样式。例子包括`"bold_folder_labels"`和`"!always_show_minimap_viewport"`。

该`parents`键是指定对象数组`class`和`attributes`必须在一个父元素相匹配。

该`platforms`键是一个字符串指定什么操作系统，应用规则的数组。有效的选项包括`"osx"`，`"windows"`，和`"linux"`。

*属性*引用JSON对象中的所有其他键。某些属性可用于所有元素，而其他属性特定于单个元素。

## 一般信息

以下部分讨论有关图像的信息以及如何指定样式。

### 特性

与CSS不同，Sublime Text模板在将规则应用于元素时不进行特性匹配。所有规则都按顺序针对每个元素进行测试。匹配的后续规则将覆盖先前规则的属性。

### 纹理图像

使用PNG图像指定模板中的所有纹理。每个纹理应保存在“正常”DPI，其中文件中的每个像素将映射到一个设备像素。模板定义中的所有文件路径都应引用正常的DPI版本。

每个纹理的第二个版本也应该包含在DPI的两倍，并`@2x`在扩展名之前添加到文件名。Sublime Text将`@2x`在高DPI屏幕上显示时自动使用该 版本。

虽然目前尚未实施，但显示器的分辨率越来越高，可能会`@3x`在未来的某些时候支持变体。

*目前不支持SVG图像。*

### 外形尺寸

引用维度的模板中的整数单元始终在与设备无关的像素（DIP）中指定。Sublime Text根据屏幕密度自动处理缩放UI元素。

### 填充和边距

填充和边距可以通过以下三种方式之一指定：

*   单个整数值 - 相同的值应用于左侧，顶部，右侧和底部
*   一个包含两个整数的数组 - 第一个值应用于左侧和顶部，而第二个值应用于右侧和底部
*   一个包含四个整数的数组 - 这些值按顺序应用于左侧，顶部，右侧和底部

### 颜色值

颜色可以以多种不同的格式指定：

#### RGB

在RGB颜色空间中的颜色通过的3个或4个数字的阵列指定，前三个是整数范围从`0`到`255`表示红色，绿色和蓝色的组件。可选的第四数目是范围从浮子`0.0`至`1.0`控制该颜色的不透明度。

*颜色为白色，完全不透明*

~~~js
[255, 255, 255]
~~~

*颜色为蓝色，不透明度为50％*

~~~js
[0, 0, 255, 0.5]
~~~

#### HSL

也可以使用HSL颜色空间通过创建4个元素的数组来指定颜色，第一个是字符串`"hsl"`。第二个元素是从整数`0`到`360`指定色调。第三是从整数`0`到`100`指定饱和度，和第四个是由一个整数`0`，以`100`指定的亮度。

*深洋红色，完全不透明*

~~~js
["hsl", 325, 100, 30]
~~~

可以添加 来自`0.0`to 的float`1.0`作为第五个元素来控制不透明度。

*明亮的蓝绿色，50％不透明度*

~~~js
["hsl", 180, 100, 75, 0.5]
~~~

#### 派生的颜色

也可以从当前的全局颜色方案中导出颜色。使用具有特定格式的数组指定此格式的颜色。在所有情况下，第一个元素是基色，可以是`"foreground"`，`"background"`或`"accent"`。

##### 改变基色的不透明度

要改变一个基色的不透明度，指定2个元素的阵列，第一个基体颜色名，第二个是从浮`0.0`到`1.0`。不透明度将设置为浮点值。

*配色方案前景，不透明度为90％*

~~~js
["foreground", 0.9]
~~~

##### 去饱和基色

要使基色去饱和，请指定包含3个元素的数组。第一个是基色的名称，第二个是字符串`"grayscale"`，第三个是来自的整数`0`，`100`它指定应保留现有颜色的饱和度（在HSL颜色空间中）的百分比。值`100`意味着没有变化，而值`0`会导致颜色完全去饱和。

*颜色方案前景，饱和度调整为原始值的1/4。*

~~~js
["foreground", "grayscale", 25]
~~~

##### 色调基色

5和6元素导出的颜色允许将颜色混合到基色中。5种颜色使用RGBA颜色，而6种颜色使用HSLA颜色。在这两种情况下，最后一个元素（通常表示不透明度）控制将多少二次色混合到基底中。

*配色方案背景，用白色照亮*

~~~js
["background", 255, 255, 255, 0.1]
~~~

*配色方案，深红色*

~~~js
["accent", "hsl", 0, 100, 30, 0.2]
~~~

从颜色方案派生的颜色将始终基于全局颜色方案，并且不会反映特定于视图的颜色方案。UI中的某些特定于视图的控件具有着色属性，允许使用特定于视图的颜色方案颜色。

## 特性

特性指定为字符串数组。每个字符串都是一个特性名称。要检查是否缺少特性，请`!`在名称前加上a 。

以下特性对所有元素都是通用的：

徘徊

每当用户的鼠标悬停在元素上时设置

### 亮度

尽管并非所有元素都可用，但许多元素都具有基于当前颜色方案的近似亮度设置的特性。大多数元素都具有基于全局颜色方案设置的特性。但是，选项卡和选项卡背景具有基于特定于所选视图的颜色方案的特性。

`V`当表示为[HSV](https://en.wikipedia.org/wiki/HSL_and_HSV)颜色时，基于背景颜色 的值 分配特性。

file\_light

`V`从`0.60-1.00`

file\_medium

`V`从`0.30-0.59`

file\_medium\_dark

`V`从`0.10-0.29`

file\_dark

`V`从`0.00-0.09`

## 设置

某些Sublime Text设置旨在影响UI。模板应尊重这些设置并根据它们更改元素。

overlay\_scroll\_bars

这应该会影响滚动条的样式 - 通常它们应该是半透明的`overlay`，并且`scroll_area_control`应该将其属性设置为`true`

always\_show\_minimap\_viewport

如果当前视口区域应在小地图上突出显示，即使用户没有悬停在小地图上。

bold\_folder\_labels

如果侧栏中的文件夹名称应将`font.bold`属性设置为`true`。

mouse\_wheel\_switches\_tabs

这用于控制Linux上标签的鼠标滚轮行为。它应与检查相结合`!enable_tab_scrolling`来改变`mouse_wheel_switch`的关键`tabset_control`来`false`。

highlight\_modified\_tabs

如果应突出显示已修改文件的选项卡。除`dirty`属性外，还应检查此设置 。

show\_tab\_close\_buttons

如果标签应该有关闭按钮

## 属性

`.sublime-theme`文件是描述UI元素应当如何称呼对象的JSON阵列。UI中的每个元素都支持以下键：

层0。*\**

元素 的最底部纹理[图层](themes#layer_properties)

layer1的。*\**

元素 的第二个纹理[图层](themes#layer_properties)

二层。*\**

元素 的第三个纹理[图层](themes#layer_properties)

三层。*\**

元素 的第四个纹理[图层](themes#layer_properties)

hit\_test\_level

一个浮点值，设置一个像素所需的不透明度，以便点击一下“点击”

### 图层属性

UI中的每个元素都支持最多四个纹理图层，用于显示填充颜色和光栅图形。每个层都有格式的虚线子键 。有效的子键包括：`layer*#*.*sub-key*`

*层＃。*不透明度

从一个浮点值`0.0`，以`1.0`控制该层的主不透明度。

例：`0.9`

*层＃。*着色

要应用于图层的填充颜色 的[颜色值](themes#color_values)。

例：`[255, 0, 0, 127]`

*层＃。*质地

相对于`Packages/`文件夹的PNG图像文件路径的字符串。

例：`"Theme - Default/arrow_right.png"`

*层＃。*inner\_margin

通过使用四条线切割成9的网格来拉伸纹理图像以适合元素。请参阅[填充和边距](themes#padding_margin)以获取有效格式，以指定用于制作切片的边距。

例：`[5, 2, 5, 2]`

*层＃。*draw\_center

一个布尔值，控制是否`*layer#.*inner_margin`应绘制创建的9网格的中心矩形。这是一个允许跳过未使用的纹理部分的优化。

例：`false`

*层＃。*重复

一个布尔值，控制是否应重复纹理而不是拉伸。

例：`false`

#### 价值动画

浮动指定的属性可能会随着时间的推移而动画化。不是提供单个数值，而是使用包含动画细节的对象指定动画。*值动画主要用于随时间改变不透明度。*对象键是：

目标

从一个浮点值`0.0`，以`1.0`控制该目标值

例：`1.0`

速度

浮点值`1.0`或更大值，用于控制动画所需的相对时间长度

例：`1.5`

插值

一个可选字符串，允许指定`smoothstep`函数的使用 而不是默认的线性函数。

默认值：`"linear"`  
示例：`"smoothstep"`

*由于规则是按顺序应用的，因此可以尝试设置基本不透明度，然后使用后续规则指定属性或父尝试应用动画。这通常会导致值重置为基本量，然后动画似乎无法正常工作。*

#### 纹理动画

所述`*layer#.*texture`子密钥可以是一个对象来指定基于两个或两个以上的PNG图像的动画。对象键是：

关键帧

按顺序排列PNG图像路径的字符串数组

例：`["Theme - Default/spinner.png", "Theme - Default/spinner1.png"]`

循环

一个可选的布尔值，控制动画是否应该重复

默认值：`false`  
示例：`true`

frame\_time

一个可选的浮点数，指定每帧应显示多长时间。`1.0`代表1秒。

默认值：`0.0333`（30 fps）  
示例：`0.0166`（60 fps）

### 纹理着色属性

某些元素具有由当前颜色方案的背景设置的可用色调值。可以修改色调并将其应用于 图像。`layer*#*.texture`

tint\_index

控制应用色调的图层。必须从一个整数`0`到`3`。

tint\_modifier

在该范围的四个整数的数组`0`来`255`。前三个从色调颜色混合到RGB值中，第四个值指定要应用多少RGB修改器值。

### 字体属性

某些文本元素允许设置以下字体属性：

font.face

字体的名称

字体大小

整数点大小

font.bold

如果字体应为粗体，则为布尔值

font.italic

如果字体应为斜体，则为布尔值

### 阴影属性

某些文本元素允许设置以下属性：

shadow\_color

用于文本阴影 的[颜色值](themes#color_values)

shadow\_offset

包含阴影的X和Y偏移的2元素数组

### 过滤标签属性

快速面板中使用的标签具有基于选择和匹配的颜色控制

FG

未选择的，不匹配的文本 的[颜色值](themes#color_values)

match\_fg

未选择的匹配文本 的[颜色值](themes#color_values)

BG

一个[颜色值](themes#color_values)对未被选择的行的背景

selected\_fg

选定的不匹配文本 的[颜色值](themes#color_values)

selected\_match\_fg

选定匹配文本 的[颜色值](themes#color_values)

BG

一个[颜色值](themes#color_values)针对所选择的行的背景

font.face

字体的名称

字体大小

整数点大小

### 数据表属性

基于行的数据表提供以下属性：

dark\_content

如果背景很暗 - 用于设置`dark`滚动条的属性

row\_padding

padding以[padding和margin中](themes#padding_margin)描述的格式之一添加到每一行[](themes#padding_margin)

## 分子

以下是构成Sublime Text UI的元素的详尽列表，以及支持的属性和属性。

*   [视窗](themes#elements-windows)
*   [边栏](themes#elements-side_bar)
*   [标签](themes#elements-tabs)
*   [快速面板](themes#elements-quick_panel)
*   [查看](themes#elements-views)
*   [面板](themes#elements-panels)
*   [状态栏](themes#elements-status_bar)
*   [对话框](themes#elements-dialogs)
*   [滚动条](themes#elements-scroll_bars)
*   [输入](themes#elements-inputs)
*   [按钮](themes#elements-buttons)
*   [标签](themes#elements-labels)
*   [工具提示](themes#elements-tool_tips)

### 视窗

标题栏

*仅在OS X 10.10+上受支持。*

#### 属性

[光度属性](themes#luminosity_attributes)

#### 属性

FG

用于窗口标题文本 的[颜色值](themes#color_values)

BG

用于标题栏背景 的[颜色值](themes#color_values)

窗口

此元素不能直接设置样式，但可以在`parents`说明符中使用。发光度属性基于全局颜色方案设置。

#### 属性

[光度属性](themes#luminosity_attributes)

#### 属性

*没有*

edit\_window

此元素包含主编辑器窗口，旨在用于`parents`说明符。

#### 属性

*没有*

switch\_project\_window

此元素包含“切换项目”窗口，旨在用于`parents`说明符。

#### 属性

*没有*

### 边栏

sidebar\_container

处理滚动的主要侧边栏容器

#### 属性

content\_margin

该[保证金](themes#padding_margin)周围的`sidebar_tree`

sidebar\_tree

包含多个`tree_row`s的 树控件

#### 属性

[数据表属性](themes#data_table_properties)

缩进

一个整数量，用于缩进树结构的每个级别

indent\_offset

为了定位`disclosure_button_control`和定义 ，应用于每一行的附加缩进`close_button`

indent\_top\_level

如果树中的顶级行应缩进，则为布尔值

spacer\_rows

一个布尔值控制是否应该在侧边栏的“*打开文件*和*文件夹”*部分之间添加一个空行，当两者都可见时。

tree\_row

行可以包含标题，打开文件，文件夹或文件

#### 属性

可选

当一行可选时

选

选择可选行时

扩张

当一行可扩展时

扩大

扩展可扩展行时

sidebar\_heading

侧栏中的“打开文件”，“组＃”或“文件夹”标题之一

#### 属性

[字体属性](themes#font_properties)[阴影属性](themes#shadow_properties)

FG

用于文本 的[颜色值](themes#color_values)

sidebar\_label

打开文件，文件夹名称和文件名的名称

#### 属性

[字体属性](themes#font_properties)[阴影属性](themes#shadow_properties)

FG

用于文本 的[颜色值](themes#color_values)

close\_button

“*打开文件”*部分 中每个文件左侧的按钮

#### 属性

content\_margin

对于按钮，[边距](themes#padding_margin)指定尺寸

disclosure\_button\_control

可以展开的所有`tree_row`s中 的展开/折叠图标

#### 属性

content\_margin

对于按钮，[边距](themes#padding_margin)指定尺寸

icon\_folder

内容完全枚举后用于文件夹

#### 属性

content\_margin

对于图标，[边距](themes#padding_margin)指定尺寸

icon\_folder\_loading

在枚举内容时用于文件夹

#### 属性

content\_margin

对于图标，[边距](themes#padding_margin)指定尺寸

icon\_folder\_dup

用于先前在侧栏中扫描过的文件夹。*由于递归符号链接，这对于防止可能无限的文件列表是必要的。*

#### 属性

content\_margin

对于图标，[边距](themes#padding_margin)指定尺寸

icon\_file\_type

文件的图标。在`layer0.texture`因为它是基于动态确定不应该设置`icon`提供设置*.tmPreferences*文件。

#### 属性

content\_margin

对于图标，[边距](themes#padding_margin)指定尺寸

### 标签

tabset\_control

#### 属性

[光度属性](themes#luminosity_attributes)

#### 属性

content\_margin

该[保证金](themes#padding_margin)周围的`tab_control`小号

tab\_overlap

选项卡应重叠多少DIP

tab\_width

空间可用时的默认选项卡宽度

tab\_min\_width

标签滚动前的最小标签宽度

tab\_height

DIP中标签的高度

mouse\_wheel\_switch

如果鼠标滚轮应切换标签 - 只应`true`设置`enable_tab_scrolling`为假 设置

tab\_control

#### 属性

[光度属性](themes#luminosity_attributes)

脏

当关联的视图未保存更改时

选

当关联视图是其组中的活动视图时

短暂的

当关联视图是预览而未完全打开时

#### 属性

content\_margin

该[保证金](themes#padding_margin)周围的`tab_label`

max\_margin\_trim

`content_margin`当标签空间非常有限时，可以删除 多少左右

accent\_tint\_index

控制应用重音色调的图层。必须从一个整数`0`到`3`。强调颜色由颜色方案指定。

accent\_tint\_modifier

在该范围的四个整数的数组`0`来`255`。前三个从重音色调颜色混合到RGB值，第四个值指定要应用的RGB修改器值的多少。

tab\_label

#### 属性

短暂的

当关联视图是预览而未完全打开时

#### 属性

[字体属性](themes#font_properties)[阴影属性](themes#shadow_properties)

FG

用于文本 的[颜色值](themes#color_values)

tab\_close\_button

#### 属性

content\_margin

对于按钮，[边距](themes#padding_margin)指定尺寸

accent\_tint\_index

控制应用重音色调的图层。必须从一个整数`0`到`3`。强调颜色由颜色方案指定。

accent\_tint\_modifier

在该范围的四个整数的数组`0`来`255`。前三个从重音色调颜色混合到RGB值，第四个值指定要应用的RGB修改器值的多少。

scroll\_tabs\_left\_button

#### 属性

content\_margin

对于按钮，[边距](themes#padding_margin)指定尺寸

scroll\_tabs\_right\_button

#### 属性

content\_margin

对于按钮，[边距](themes#padding_margin)指定尺寸

show\_tabs\_dropdown\_button

#### 属性

content\_margin

对于按钮，[边距](themes#padding_margin)指定尺寸

### 快速面板

快速面板用于各种Goto功能，命令调色板，可供插件使用。

overlay\_control

快速面板的容器，包括输入和数据表

#### 属性

content\_margin

该[保证金](themes#padding_margin)周围的`quick_panel`

quick\_panel

数据表显示在输入下方。通常，高度是动态的，因此图层将不可见，但“切换项目”窗口将使用图层作为已过滤选项下方的空白区域。

#### 属性

[数据表属性](themes#data_table_properties)

mini\_quick\_panel\_row

非文件行`quick_panel`。包含`quick_panel_label`行中每行文本的一行。

#### 属性

选

选择行时

quick\_panel\_row

一个Goto Anything文件行`quick_panel`。也用于Switch Project窗口。

包含`quick_panel_label`文件名和`quick_panel_path_label`文件路径。

#### 属性

选

选择行时

quick\_panel\_label

文件名`quick_panel_row`和中的所有文本`mini_quick_panel_row`

#### 属性

[过滤标签属性](themes#filter_label_properties)

quick\_panel\_path\_label

文件路径`quick_panel_row`

#### 属性

[过滤标签属性](themes#filter_label_properties)

### 查看

text\_area\_control

此元素无法直接设置样式，因为它由颜色方案控制，但它可以在`parents`说明符中使用。

#### 属性

[光度属性](themes#luminosity_attributes)

#### 属性

*没有*

grid\_layout\_control

当多个组可见时，在视图之间显示边框

#### 属性

*没有层支持*

边框颜色

用于边框 的[颜色值](themes#color_values)

border\_size

DIP中边框大小的整数

minimap\_control

控制小地图上视口投影的显示

#### 属性

*没有层支持*

viewport\_color

用于填充视口投影 的[颜色值](themes#color_values)

viewport\_opacity

浮点数`0.0`到`1.0`指定视口投影的不透明度

fold\_button\_control

天沟中的代码折叠按钮

#### 属性

扩大

当一段代码展开时

#### 属性

content\_margin

对于按钮，[边距](themes#padding_margin)指定尺寸

popup\_control auto\_complete\_popup

自动完成弹出窗口的主要容器

popup\_control html\_popup

*显示定义*和第三方包 使用的HTML弹出窗口的主要容器 。滚动条的色调将设置为HTML文档的背景颜色。

auto\_complete

完成数据的数据表。基于应用于显示弹出窗口的视图的颜色方案的背景颜色来设置色调。

#### 属性

[数据表属性](themes#data_table_properties)[纹理着色属性](themes#texture_tinting_properties)

table\_row

一排`auto_complete`

#### 属性

选

当用户突出显示完成时

auto\_complete\_label

文字在`table_row`

#### 属性

[过滤标签属性](themes#filter_label_properties)

fg\_blend

一个布尔控制，如果`fg`，`match_fg`，`selected_fg`，和`selected_match_fg`的值应该从当前视图的颜色方案共混到前景颜色

### 面板

panel\_control find\_panel

查找和增量查找面板的容器。

#### 属性

content\_margin

该[保证金](themes#padding_margin)周围的面板内容

panel\_control replace\_panel

“替换”面板的容器。

#### 属性

content\_margin

该[保证金](themes#padding_margin)周围的面板内容

panel\_control find\_in\_files\_panel

“在文件中查找”面板的容器。

#### 属性

content\_margin

该[保证金](themes#padding_margin)周围的面板内容

panel\_control input\_panel

输入面板的容器，可通过API获得，用于文件重命名等操作。

#### 属性

content\_margin

该[保证金](themes#padding_margin)周围的面板内容

panel\_control console\_panel

控制台的容器。

#### 属性

content\_margin

该[保证金](themes#padding_margin)周围的面板内容

panel\_control output\_panel

输出面板的容器，可通过API获得并用于构建结果。

#### 属性

content\_margin

该[保证金](themes#padding_margin)周围的面板内容

panel\_control switch\_project\_panel

Switch Project窗口中输入的容器。

#### 属性

content\_margin

该[保证金](themes#padding_margin)周围的面板内容

panel\_grid\_control

布局网格用于在各种面板上定位输入。

#### 属性

*没有层支持*

inside\_spacing

整数填充，放置在网格的每个单元格之间

outside\_vspacing

在网格上方和下方放置的整数填充

outside\_hspacing

一个整数填充，放置在网格的左侧和右侧

panel\_close\_button

用于关闭打开面板的按钮

#### 属性

content\_margin

对于按钮，[边距](themes#padding_margin)指定尺寸

### 状态栏

状态栏

#### 属性

panel\_visible

当面板显示在状态栏上方时

#### 属性

content\_margin

该[保证金](themes#padding_margin)周围`panel_button_control`，`status_container`和`status_buttons`小号

panel\_button\_control

状态栏左侧的面板切换器按钮

#### 属性

content\_margin

对于按钮，[边距](themes#padding_margin)指定尺寸

status\_container

包含当前状态消息的区域

#### 属性

content\_margin

该[保证金](themes#padding_margin)周围的状态消息

status\_button

显示并允许更改缩进，语法，编码和行结尾的状态按钮

#### 属性

content\_margin

对于按钮，[边距](themes#padding_margin)指定尺寸

MIN\_SIZE

在DIP中指定按钮最小宽度和高度的两个整数数组

### 对话框

对话

“索引器状态”和“更新”窗口都将此类用于窗口背景

progress\_bar\_control

进度条容器。进度条显示在用于OS X和Windows更新的“更新”窗口中。

progress\_gauge\_control

表示迄今为止完成的进度的栏

#### 属性

content\_margin

的[余量](themes#padding_margin)指定栏的高度

### 滚动条

scroll\_area\_control

滚动区域包含滚动的元素，以及条形，轨道和圆盘。

#### 属性

content\_margin

在滚动的内容周围添加 的[边距](themes#padding_margin)

覆盖

设置要在内容顶部呈现的滚动条

scroll\_bar\_control

滚动条包含滚动轨道。基于正在滚动的元素的背景颜色设置色调。

#### 属性

黑暗

当滚动区域内容较暗时，需要一个轻滚动条

横

当滚动条应该是水平而不是垂直

#### 属性

[纹理着色属性](themes#texture_tinting_properties)

content\_margin

在滚动轨道周围添加 的[边距](themes#padding_margin)

scroll\_track\_control

冰球运行的轨道。基于正在滚动的元素的背景颜色设置色调。

#### 属性

黑暗

当滚动区域内容较暗时，需要一个轻滚动条

横

当滚动条应该是水平而不是垂直

#### 属性

[纹理着色属性](themes#texture_tinting_properties)

scroll\_corner\_control

puck\_control

滚动冰球或手柄。基于正在滚动的元素的背景颜色设置色调。

#### 属性

黑暗

当滚动区域内容较暗时，需要一个轻滚动条

横

当滚动条应该是水平而不是垂直

#### 属性

[纹理着色属性](themes#texture_tinting_properties)

### 输入

text\_line\_control

快速面板，查找，替换，在文件和输入面板中查找使用的文本输入。

#### 属性

content\_margin

该[保证金](themes#padding_margin)周围的文本

color\_scheme\_tint

一个[颜色值](themes#color_values)用来着色的颜色方案的背景

color\_scheme\_tint\_2

一个[颜色值](themes#color_values)以使用到二次色调添加到颜色方案的背景

dropdown\_button\_control

用于关闭打开面板的按钮

#### 属性

content\_margin

对于按钮，[边距](themes#padding_margin)指定尺寸

### 按钮

button\_control

文字按钮

#### 属性

压制

按下按钮时设置

#### 属性

MIN\_SIZE

在DIP中指定按钮最小宽度和高度的两个整数数组

icon\_button\_group

控制相关图标按钮间距的网格

#### 属性

*没有层支持*

间距

组中每个按钮之间的整数个像素

icon\_button\_control

“查找”，“在文件中查找”和“替换”面板中的基于图标的小按钮

#### 属性

选

切换图标按钮时

剩下

当按钮是组中最左侧的按钮时

对

当按钮是组中最右侧的按钮时

icon\_regex

用于在“查找”，“在文件中查找”和“替换”面板中启用正则表达式模式的按钮

#### 属性

content\_margin

对于图标，[边距](themes#padding_margin)指定尺寸

icon\_case

用于在“查找”，“在文件中查找”和“替换”面板中启用区分大小写模式的按钮

#### 属性

content\_margin

对于图标，[边距](themes#padding_margin)指定尺寸

icon\_whole\_word

用于在“查找”，“在文件中查找”和“替换”面板中启用全字模式的按钮

#### 属性

content\_margin

对于图标，[边距](themes#padding_margin)指定尺寸

icon\_wrap

使用“查找和替换”面板时启用搜索换行的按钮

#### 属性

content\_margin

对于图标，[边距](themes#padding_margin)指定尺寸

icon\_in\_selection

使用“查找和替换”面板时，仅在选择中搜索的按钮

#### 属性

content\_margin

对于图标，[边距](themes#padding_margin)指定尺寸

icon\_highlight

用于在“查找和替换”面板中突出显示所有匹配项的按钮

#### 属性

content\_margin

对于图标，[边距](themes#padding_margin)指定尺寸

icon\_preserve\_case

使用“替换”面板时启用保留大小写模式的按钮

#### 属性

content\_margin

对于图标，[边距](themes#padding_margin)指定尺寸

icon\_context

使用“在文件中查找”面板时显示匹配上下文的按钮

#### 属性

content\_margin

对于图标，[边距](themes#padding_margin)指定尺寸

icon\_use\_buffer

使用“在文件中查找”面板时，显示的按钮会导致缓冲区而不是输出面板

#### 属性

content\_margin

对于图标，[边距](themes#padding_margin)指定尺寸

### 标签

label\_control

标签显示在查找，替换，在文件和输入面板中查找。此外，标签在“更新”窗口，文本按钮和文本中使用`status_container`。

*可以使用`parents`密钥来完成定位特定标签。*

#### 属性

[字体属性](themes#font_properties)[阴影属性](themes#shadow_properties)

颜色

用于文本 的[颜色值](themes#color_values)

title\_label\_control

标题标签用于“关于”窗口。

#### 属性

[字体属性](themes#font_properties)[阴影属性](themes#shadow_properties)

颜色

用于文本 的[颜色值](themes#color_values)

### 工具提示

tool\_tip\_control

将鼠标悬停在标签和按钮上时显示的工具提示

#### 属性

content\_margin

工具提示文本周围 的[边距](themes#padding_margin)

tool\_tip\_label\_control

工具提示中显示的文本

#### 属性

[字体属性](themes#font_properties)[阴影属性](themes#shadow_properties)

颜色

用于文本 的[颜色值](themes#color_values)

## 弃用

### 颜色值

在构建3127之前，指定颜色不透明度的唯一方法是使用包含来自`0`to的所有整数的4元素数组`255`。第四个元素控制不透明度，使其`0`完全透明并且`255`完全不透明。的优选格式是现在使用的浮子从`0.0`到`1.0`。

## 过时的

随着Sublime Text的UI随着时间的推移而适应，某些元素和属性不再适用或支持。

### 分子

`sheet_container_control`在Sublime Text 3的最新版本中， 该元素永远不会被用户看到。

命名`icon_reverse`用于存在于查找面板中的元素，用于控制搜索是在视图中向前还是向后移动。现在由*Find*和*Find Prev*按钮控制。

名为的元素`quick_panel_score_label`不再出现在Goto Anything快速面板中。

### 属性

`blur`过去支持 该属性来模糊元素后面的像素数据，但由于实现原因，目前不支持该属性。

## 定制

用户可以通过创建具有将附加到原始模板定义的新规则的文件来自定义模板。

要创建模板的用户特定自定义，请创建与模板具有相同文件名的新文件，但将其保存在Packages/User/目录中。

例如，要自定义默认模板，请创建名为Packages/User/Default.sublime-theme的文件。将以下规则添加到该文件将增加侧栏中文本的大小。

~~~js
[
    {
        "class": "sidebar_heading",
        "font.size": 15,
    },
    {
        "class": "sidebar_label",
        "font.size": 14
    }
]
~~~
# [SUBLIME TEXT中文文档之](index)配色方案

Sublime Text中源代码和文本的高亮显示由配色方案控制。*配色方案*按照语法来控制颜色和文本样式。用户界面的其他部分由[模板](themes)来控制。模板控制按钮、选择列表、侧边栏和选项卡等元素。

Sublime Text配色方案使用.sublime-color-scheme文件实现，其语法为JSON。 Sublime Text还支持使用TextMate[.tmTheme格式](color_schemes_tmtheme)。

*   [示例](color_schemes#example)
*   [颜色](color_schemes#colors)
*   [变量](color_schemes#variables)
*   [全局设置](color_schemes#global_settings)
    *   [强调提示](color_schemes#global_settings-accents)
    *   [CSS](color_schemes#global_settings-css)
    *   [插槽](color_schemes#global_settings-gutter)
    *   [选择](color_schemes#global_settings-selection)
    *   [查找](color_schemes#global_settings-find)
    *   [指南](color_schemes#global_settings-guides)
    *   [括号](color_schemes#global_settings-brackets)
    *   [标签](color_schemes#global_settings-tags)
    *   [阴影](color_schemes#global_settings-shadows)
*   [范围规则](color_schemes#scope_rules)
    *   [匹配](color_schemes#matching)
    *   [命名](color_schemes#naming)
    *   [风格规则](color_schemes#style_rules)
    *   [哈希语法高亮](color_schemes#hashed_syntax_highlighting)
    *   [例子](color_schemes#examples)
*   [定制](color_schemes#customization)
*   [附录：CSS颜色](color_schemes#css_colors)

## 例

*以下是.sublime-color-scheme文件格式的示例 。完整的配色方案将有更多规则来涵盖标准范围名称。*

~~~js
{
    "name": "Example Color Scheme",
    "globals":
    {
        "background": "rgb(34, 34, 34)",
        "foreground": "#EEEEEE",
        "caret": "white"
    },
    "rules":
    [
        {
            "name": "Comment",
            "scope": "comment",
            "foreground": "#888888"
        },
        {
            "name": "String",
            "scope": "string",
            "foreground": "hsla(50, 100%, 50%, 1)",
        },
        {
            "name": "Number",
            "scope": "constant.numeric",
            "foreground": "#7F00FF",
            "font_style": "italic",
        }
    ]
}
~~~

## 颜色

可以使用以下七种格式之一指定配色方案中的颜色：

*   **十六进制RGB**：`#`后跟六个十六进制字符，前两个指定红色通道，第二个指定绿色通道，最后两个指定蓝色通道。红色写成`#FF0000`。当三对中的每一对对两个字符使用相同的值时，可以使用缩写形式。红色写成`#F00`。
*   **十六进制RGBA**：与十六**进制RGBA**相同，但在末尾添加一对额外的十六进制字符以指定alpha通道。红色与67％的不透明度写成`#FF0000AA`。缩写形式将是`#F00A`。
*   **RGB功能表示法**：一个名为的函数`rgb`，它接受0到255范围内的三个整数。第一个整数指定红色通道，第二个整数指定绿色通道，第三个整数指定蓝色通道。红色写成`rgb(255, 0, 0)`。
*   **RGBA函数表示法**：相同于RGB格式的功能，除了功能的名称是`rgba`与第四参数被添加从接受的值`0.0`，以`1.0`指定alpha通道。红色与50％不透明度写成`rgba(255, 0, 0, 0.5)`。
*   **HSL功能表示法**：一个名为的函数`hsl`，它接受三个值。第一个是指定色调的0到360范围内的整数。第二个是指定饱和度的百分比。第三个是指定亮度的百分比。红色写成`hsl(0, 100%, 100%)`。
*   **HSLA函数表示法**：相同于HSL功能格式，除了功能的名称是`hsla`与第四参数被添加从接受的值`0.0`，以`1.0`指定alpha通道。红色与50％不透明度写成`hsla(0, 100%, 100%, 0.5)`。
*   **命名**：[CSS颜色名称](color_schemes#css_colors)。*请注意，虽然某些与.tmTheme文件中使用的X11命名颜色共享名称 ，但实际颜色往往不同。*

另外，颜色可以指定为[变量](color_schemes#variables)，然后通过语法引用`var(example_var_name)`。当与CSS颜色模块级别4组合变量的引用是特别有用的[彩色-MOD函数](https://drafts.csswg.org/css-color-4/#modifying-colors)和所支持的`blend()`，`blenda()`和`alpha()`调节器。

*   **blend（）调节器**：将颜色混合到底座中。在RGB空间中混合等于灰色的部分和通过变量引用的基色：`color(var(base_green) blend(#888 50%))`。如果颜色应在HSL空间中混合，请使用以下形式：`color(var(base_green) blend(#888 50% hsl))`。生成的alpha值始终是基色的alpha通道。
*   **blenda（）调节器**：功能与**调节器的**功能相同`blend()`，但混合了两种颜色的alpha通道，而不是仅使用基座的alpha通道。将部分透明灰色混合为绿色的示例：`color(var(base_green) blenda(#8888 50% hsl))`
*   **α（）调节**：改变基色指定，从值的alpha通道`0.0`到`1.0`。α通道设置为90％：`color(var(base_green) alpha(0.9))`。*此调整器也可以使用**简写名称`a()`。*

## 变量

可以在`variables`密钥中创建可重用的颜色定义。名称可能是利用字符的任意字符串`a-z`，`A-Z`，`0-9`，`_`和`-`。值可以是任何有效的颜色格式。

可以通过语法在全局设置和规则中引用变量`var(example_var_name)`。以下示例显示了基本变量用法：

~~~js
{
    "name": "Example Color Scheme",
    "variables":
    {
        "green": "hsla(153, 80%, 40%, 1)",
        "black": "#111",
        "white": "rgb(242, 242, 242)"
    },
    "globals":
    {
        "background": "var(black)",
        "foreground": "var(white)",
        "caret": "color(var(white) alpha(0.8))"
    },
    "rules":
    [
        {
            "name": "Comment",
            "scope": "comment",
            "foreground": "color(var(black) blend(#fff 50%))"
        },
        {
            "name": "String",
            "scope": "string",
            "foreground": "var(green)",
        },
        {
            "name": "Number",
            "scope": "constant.numeric",
            "foreground": "#7F00FF",
            "font_style": "italic",
        }
    ]
}
~~~

## 全局设置

以下全局设置使用`globals`键进入对象 。

背景

默认背景颜色

前景

文本的默认颜色

插入符号

插入符号的颜色

line\_highlight

包含插入符号的行的背景颜色。*仅在`highlight_line`启用设置时使用。*

### 强调提示

拼写错误

用于在拼写错误的单词下绘制的波浪下划线的颜色。

fold\_marker

用于指示内容的标记的颜色已折叠。

minimap\_border

`draw_minimap_border`启用 此设置时，在小地图的视口区域周围绘制的边框颜色 。*请注意，视口通常仅在悬停时可见，除非`always_show_minimap_viewport`启用该设置。*

强调提示

可供模板使用的颜色。*默认模板使用此选项可在`highlight_modified_tabs`启用设置时高亮显示已修改的选项卡 。*

### CSS

CSS适用于通过API公开的弹出窗口和[幻像](minihtml)功能创建的[minihtml](minihtml)内容。[minihtml CSS参考](minihtml#css)中讨论了支持的CSS属性 。

建议使用minihtml的插件`id`在`<body>`生成的HTML标记上设置唯一属性，以允许配色方案覆盖默认插件样式。

popup\_css

CSS传递给弹出窗口。

phantom\_css

CSS传递给幽灵。*如果未指定，则使用`popup_css`。*

### 插槽

插槽

插槽的背景颜色

gutter\_foreground

插槽中的行号颜色

### 选择

选择

所选文本的背景颜色

selection\_foreground

一种颜色，它将覆盖选区的基于范围的文本颜色

selection\_border

选择边框的颜色

selection\_border\_width

选择边框的宽度，从`0`到`4`。

inactive\_selection

当前未聚焦的视图中选择的背景颜色

inactive\_selection\_foreground

一种颜色，它将覆盖当前未聚焦的视图中所选内容的基于范围的文本颜色

selection\_corner\_style

选择时使用的角落样式。选项包括:(`round`默认），`cut`或`square`。

selection\_corner\_radius

当使用半径`selection_corner_style`是`round`或`cut`。

### 查找

高亮

在“查找”面板中选中 “*高亮显示匹配”*选项时“其他”匹配的边框颜色 。还用于高亮显示在文件中查找结果中的匹配项。

find\_highlight

“查找”面板匹配的文本背景颜色

find\_highlight\_foreground

一种颜色，它将覆盖“查找”面板匹配的文本的基于范围的文本颜色

### 指南

指南由`draw_indent_guides`设置全局控制。

指南

用于绘制缩进指南的颜色。*仅`"draw_normal"`在设置中存在该选项时使用`indent_guide_options`。*

active\_guide

用于绘制包含插入符号的缩进级别的缩进指南的颜色。*仅`"draw_active"`在设置中存在该选项时使用`indent_guide_options`。*

stack\_guide

用于绘制包含插入符号的缩进级别的父缩进级别的缩进指南的颜色。*仅`"draw_active"`在设置中存在该选项时使用`indent_guide_options`。*

### 括号

括号匹配由`match_brackets`设置全局控制 。

brackets\_options

当插入符号紧邻时，括号如何高亮显示。接受以下以空格分隔的列表：

*   `underline`
*   `stippled_underline`
*   `squiggly_underline`
*   `foreground`
*   `bold`
*   `italic`

brackets\_foreground

绘制指定样式时使用的颜色`brackets_options`。

bracket\_contents\_options

当插入符号位于一对括号之间时，如何高亮显示括号。接受以下以空格分隔的列表：

*   `underline`
*   `stippled_underline`
*   `squiggly_underline`
*   `foreground`

bracket\_contents\_foreground

绘制指定样式时使用的颜色`brackets_contents_options`。

### 标签

标签匹配由`match_tags`设置全局控制 。

tags\_options

当插入符号位于其中时，标记如何高亮显示。接受以下以空格分隔的列表：

*   `underline`
*   `stippled_underline`
*   `squiggly_underline`
*   `foreground`

tags\_foreground

绘制指定样式时使用的颜色`tags_options`。

### 阴影

阴影

用于显示文本区域可以水平滚动的阴影的颜色

shadow\_width

与设备无关的像素中阴影的宽度

## 范围规则

配色方案通过范围与文件中的文本进行交互。范围通过*语法*设置为代码或散文令牌。范围是虚线字符串，从最少到最具体指定。例如，`if`PHP中的关键字可以通过范围名称指定`keyword.control.php`。

### 匹配

配色方案通过匹配点缀标签（从第一个开始）将颜色和字体样式应用于范围。前缀匹配是将配色方案应用于多种语法的标准方法。而不是匹配`keyword.control.php`，大多数配色方案将改为分配颜色`keyword`。匹配范围中的前一个或两个标签是最常见的。除非需要特定于语法的覆盖，否则包括最终标签（语法名称）并不常见。

### 命名

语法的作者可以将他们想要的任何范围分配给给定的标记。这与事实相结合，有数百个社区维护的语法意味着很难知道要定位的范围。建立[官方范围命名准则](http://www.sublimetext.com/docs/3/scope_naming)是为了帮助语法和配色方案作者使用通用集合，以实现更好的互操作性。“[配色方案中](http://www.sublimetext.com/docs/3/scope_naming#color_schemes)的[用法”](http://www.sublimetext.com/docs/3/scope_naming#color_schemes)部分提供了配色方案作者应该努力处理的基准范围集。

### 风格规则

每个范围样式规则都包含一个包含`scope`键的对象，以及一个或多个以下可选键：

*   `name`\- 范围规则的（任意）名称
*   `foreground`\- 文字颜色
*   `background`\- 背景颜色
*   `selection_foreground`\- 选中时的文本颜色
*   `font_style`\-零个或更多的`bold`，`italic`由空格分隔

### 哈希语法高亮显示

该`foreground`键支持一种名为Hashed Syntax Highlighting的特殊模式，其中与指定范围匹配的每个标记将从一个或多个渐变中获得唯一的颜色。一些编辑将这种高亮显示方式称为“语义高亮显示”。

要使用“散列语法高亮显示”，`foreground`键必须具有两个或更多颜色列表的值。Sublime Text将创建256种不同的颜色，这些颜色是所提供颜色之间的线性插值（lerp）。插值在HSL空间中完成。

当Sublime Text高亮显示文件中的标记时，它将创建标记的散列值，并使用它来选择256个线性插值中的一个。给定标记的每个实例都将使用相同的颜色。例如，每个实例`first_name`都具有相同的颜色，但每个实例`name`都具有不同的颜色。

对于散列语法高亮显示最明显，起点和终点之间的色调差异应尽可能远。以下是使用变量名称的蓝色，紫色和粉红色的示例：

~~~js
{
    "scope": "source - punctuation - keyword",
    "foreground": ["hsl(200, 60%, 70%)", "hsl(330, 60%, 70%)"]
}
~~~

### 例子

以下范围样式规则将所有字符串着色为绿色：

~~~js
{
    "name": "Strings",
    "scope": "string",
    "foreground": "#00FF00"
}
~~~

要将所有数字设置为粗体，斜体红色，请使用：

~~~js
{
    "name": "Numbers",
    "scope": "constant.numeric",
    "foreground": "#FF0000",
    "font_style": "bold italic"
}
~~~

## 定制

基于.sublime-color-scheme格式的配色方案仅由filename指定，而不是基于包的文件路径。这允许用户通过覆盖变量或全局变量以及添加规则来自定义配色方案。

要创建配色方案的用户特定自定义，请创建与配色方案具有相同文件名的新文件，但将其保存在Packages / User /目录中。

例如，要自定义默认的Monokai配色方案，请创建名为Packages / User / Monokai.sublime-color-scheme的文件。以下设置会将背景颜色更改为完全去饱和的灰色，黄色更加生动，并且将添加新规则更改Python文档字符串，使其与字符串相同。

~~~js
{
    "variables":
    {
        "yellow": "hsl(54, 100%, 50%)",
    },
    "globals":
    {
        "background": "hsl(70, 0%, 15%)",
    },
    "rules":
    [
        {
            "name": "Python docstrings",
            "scope": "comment.block.documentation.python",
            "foreground": "var(yellow)"
        },
    ]
}
~~~

合并`variables`和`globals`对象 的内容，用户的副本覆盖具有相同名称的键。对于`rules`列表，附加用户的规则。

## [附录：CSS颜色](https://www.sublimetext.com/docs/color_schemes#css_colors)

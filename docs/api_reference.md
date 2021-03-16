# 插件扩展API参考

### 一般信息

*   [示例插件](api_reference#example_plugins)
*   [插件生命周期](api_reference#plugin_lifecycle)
*   [多线程](api_reference#threading)
*   [单位和坐标](api_reference#units_coordinates)
*   [类型](api_reference#types)

### 核心组件

*   [`sublime`模块](api_reference#sublime)
*   [`Sheet`类](api_reference#sublime.Sheet)
*   [`View`类](api_reference#sublime.View)
*   [`Selection`类](api_reference#sublime.Selection)
*   [`Region`类](api_reference#sublime.Region)
*   [`Phantom`类](api_reference#sublime.Phantom)
*   [`PhantomSet`类](api_reference#sublime.PhantomSet)
*   [`Edit`类](api_reference#sublime.Edit)
*   [`Window`类](api_reference#sublime.Window)
*   [`Settings`类](api_reference#sublime.Settings)

### 插件扩展

*   [`sublime_plugin`模块](api_reference#sublime_plugin)
*   [`EventListener`类](api_reference#sublime_plugin.EventListener)
*   [`ViewEventListener`类](api_reference#sublime_plugin.ViewEventListener)
*   [`ApplicationCommand`类](api_reference#sublime_plugin.ApplicationCommand)
*   [`WindowCommand`类](api_reference#sublime_plugin.WindowCommand)
*   [`TextCommand`类](api_reference#sublime_plugin.TextCommand)
*   [`TextInputHandler`类](api_reference#sublime_plugin.TextInputHandler)
*   [`ListInputHandler`类](api_reference#sublime_plugin.ListInputHandler)

## 一般信息

### 示例插件

在Sublime Text 2中附带一些默认插件，可以在Default包中找到它们：

*   Packages/Default/delete\_word.py删除光标左侧或右侧的单词
*   Packages/Default/duplicate\_line.py复制当前行
*   Packages/Default/exec.py使用幻像显示内联错误
*   Packages/Default/font.py显示如何使用设置
*   Packages/Default/goto\_line.py提示用户输入，然后更新选择
*   Packages/Default/mark.py使用add\_regions()将图标添加到装订线
*   Packages/Default/show\_scope\_name.py使用弹出窗口显示插入符号的范围名称
*   Packages/Default/trim\_trailing\_whitespace.py在保存之前修改缓冲区
*   Packages/Default/arithmetic.py通过命令选项板运行时接受来自用户的输入

### 插件生命周期

在导入时，插件可能不会调用任何API函数，但sublime.version()，sublime.platform()，sublime.architecture()和sublime.channel()除外。

如果插件定义了模块级函数plugin\_loaded()，那么当API准备好使用时将调用它。插件也可以定义plugin\_unloaded()，以便在插件卸载之前得到通知。

### 多线程

所有API函数都是线程安全的，但请记住，从在备用线程中运行的代码的角度来看，应用程序状态将在代码运行时发生变化。

### 单位和坐标

接受或返回坐标或尺寸的API函数使用与设备无关的像素(倾角)值来实现。虽然在某些情况下这些将等同于设备像素，但通常情况并非如此。根据CSS规范，[minihtml](minihtml)将px单元视为与设备无关。

### 类型

本文档通常仅涉及Python数据类型。某些类型名称是此处记录的类，但是也有一些自定义类型名称引用具有特定语义的构造：

*   **location**：元组(str，str，(int，int)，包含有关符号位置的信息。第一个字符串是绝对文件路径，第二个字符串是相对于项目的文件路径，第三个元素是行和列的双元素元组。
*   **point**：一个int，表示从编辑器缓冲区开始的偏移量。的view方法text\_point()和rowcol()允许转换和从这种格式。
*   **value**：任何Python数据类型bool，int，float，str，list或dict。
*   **dip**：表示与设备无关的像素的浮点数。
*   **vector**：表示x和y坐标的(dip，dip)元组。
*   **CommandInputHandler**：[TextInputHandler](api_reference#sublime_plugin.TextInputHandler)或[ListInputHandler](api_reference#sublime_plugin.ListInputHandler)的子类。

## `sublime`模块

| 方法 | 返回值 | 描述 |
| --- | --- | --- |
| set\_timeout(callback, delay) | None | 在给定的delay(以毫秒为单位)后 运行主线程中的callback。具有相等延迟的回调将按照添加的顺序运行。 |
| set\_timeout\_async(callback, delay) | None | 在给定的delay(以毫秒为单位)后在备用线程上 运行callback。 |
| error\_message(string) | None | 向用户显示错误对话框。 |
| message\_dialog(string) | None | 向用户显示消息对话框。 |
| ok\_cancel\_dialog(string, ) | bool | 向用户显示确定/取消问题对话框。如果提供了ok\_title，则可以将其用作ok按钮上的文本。如果用户按下ok按钮，则返回True。 |
| yes\_no\_cancel\_dialog(string, , ) | int | 向用户显示是/否/取消问题对话框。如果提供yes\_title和/或no\_title，它们将用作某些平台上相应按钮上的文本。返回sublime.DIALOG\_YES，sublime.DIALOG\_NO或sublime.DIALOG\_CANCEL。 |
| load\_resource(name) | str | 加载给定的资源。该name应该是格式Packages/Default/Main.sublime-menu。 |
| load\_binary\_resource(name) | bytes | 加载给定的资源。该name应该是格式Packages/Default/Main.sublime-menu。 |
| find\_resources(pattern) | \[str\] | 查找文件名与给定pattern匹配的资源。 |
| encode\_value(value, ) | str | 将JSON兼容value编码为字符串表示形式。如果pretty设置为True，则字符串将包含换行符和缩进。 |
| decode\_value(string) | [value](api_reference#type-value) | 将JSON字符串解码为对象。如果string无效，则抛出ValueError。 |
| expand\_variables(value, variables) | [value](api_reference#type-value) | 使用字典变量中定义的变量 扩展字符串value中的所有variables。value也可以是list或dict，在这种情况下，结构将被递归扩展。字符串应使用片段语法，例如：`expand_variables("Hello, ${name}", {"name": "Foo"})` |
| load\_settings(base\_name) | [Settings](api_reference#sublime.Settings) | 加载命名设置。名称应包含文件名和扩展名，但不包括路径。将搜索包以查找与base\_name匹配的文件，并将结果整理到设置对象中。随后调用load\_settings()与BASE\_NAME将返回相同的对象，而不是再次从磁盘加载设置。 |
| save\_settings(base\_name) | None | 将指定设置对象的任何内存中更改刷新到磁盘。 |
| windows() | \[[Window](api_reference#sublime.Window)\] | 返回所有打开窗口的列表。 |
| active\_window() | [Window](api_reference#sublime.Window) | 返回最近使用的窗口。 |
| packages\_path() | str | 返回所有用户的松散包所在的路径。 |
| installed\_packages\_path() | str | 返回所有用户的.sublime-package文件所在的路径。 |
| cache\_path() | str | 返回Sublime Text存储缓存文件的路径。 |
| get\_clipboard() | str | 返回剪贴板的内容。size\_limit用于防止不必要的大数据，默认为16,777,216个字符 |
| set\_clipboard(string) | None | 设置剪贴板的内容。 |
| score\_selector(scope, selector) | int | 将selector与给定scope匹配，返回分数。得分为0表示不匹配，高于0表示匹配。可以将不同的选择器与相同的范围进行比较：较高的分数意味着选择器是范围的更好匹配。 |
| run\_command(string, ) | None | 使用(可选)给定的args运行命名的[ApplicationCommand](api_reference#sublime_plugin.ApplicationCommand)。 |
| get\_macro() | \[dict\] | 返回危害当前录制的宏的命令和参数列表。每个dict都包含keys命令和args。 |
| log\_commands(flag) | None | 控制命令记录。如果启用，则所有命令都从键绑定运行，菜单将记录到控制台。 |
| log\_input(flag) | None | 控制输入​​记录。如果启用，则所有按键都将记录到控制台。 |
| log\_result\_regex(flag) | None | 控制结果正则表达式日志记录。这对于调试构建系统中使用的正则表达式很有用。 |
| version() | str | 返回版本号 |
| platform() | str | 返回平台，可能是"osx"，"linux"或"windows" |
| arch() | str | 返回CPU架构，可能是"x32"或"x64" |

## `sublime.Sheet`类

表示窗口内的内容容器，即选项卡。表格可能包含[视图](api_reference#sublime.View)或图像预览。

[窗口](api_reference#sublime.Window)*或*无[查看](api_reference#sublime.View)*或*无

| 方法 | 返回值 | 描述 |
| --- | --- | --- |
| id() | int | 返回唯一标识此工作表的数字。 |
| window() |  | 返回包含工作表的窗口。如果工作表已关闭，则可以为“无”。 |
| view() |  | 返回工作表中包含的视图。如果工作表是图像预览，或者视图已关闭，则可以为“无”。 |

## `sublime.View`类

表示文本缓冲区的视图。请注意，多个视图可能引用相同的缓冲区，但它们有自己唯一的选择和几何。

| 方法 | 返回值 | 描述 |
| --- | --- | --- |
| id() | int | 返回唯一标识此视图的数字。 |
| buffer\_id() | int | 返回唯一标识此视图下的缓冲区的数字。 |
| is\_primary() | bool | 如果视图是文件的主视图。如果用户已在文件中打开多个视图，则仅为False。 |
| file\_name() | str | 全名文件是与缓冲区关联的文件，如果磁盘上不存在则为None。 |
| name() | str | 分配给缓冲区的名称(如果有) |
| set\_name(name) | None | 为缓冲区指定名称 |
| is\_loading() | bool | 如果缓冲区仍在从磁盘加载，并且尚未准备好使用，则 返回True。 |
| is\_dirty() | bool | 如果对缓冲区有任何未保存的修改，则 返回True。 |
| is\_read\_only() | bool | 如果可能未修改缓冲区，则 返回True。 |
| set\_read\_only(value) | None | 设置缓冲区的只读属性。 |
| is\_scratch() | bool | 如果缓冲区是临时缓冲区，则 返回True。划痕缓冲区从不报告为脏。 |
| set\_scratch(value) | None | 设置缓冲区的scratch属性。 |
| settings() | [Settings](api_reference#sublime.Settings) | 返回对视图的设置对象的引用。对此设置对象的任何更改都将对此视图是私有的。 |
| window() | [Window](api_reference#sublime.Window) | 返回对包含视图的窗口的引用。 |
| run\_command(string, ) | None | 使用(可选)给定的args运行命名的[TextCommand](api_reference#sublime.TextCommand)。 |
| size() | int | 返回文件中的字符数。 |
| substr(region) | str | 以字符串形式 返回region的内容。 |
| substr(point) | str | 返回该point右侧的字符。 |
| insert(edit, point, string) | int | 将给定string插入指定point的缓冲区中。返回插入的字符数：如果将制表符转换为当前缓冲区中的空格，则可能会有所不同。 |
| erase(edit, region) | None | 从缓冲区中 删除该region的内容。 |
| replace(edit, region, string) | None | 用给定的string替换region的内容。 |
| sel() | [Selection](api_reference#sublime.Selection) | 返回对选择的引用。 |
| line(point) | [Region](api_reference#sublime.Region) | 返回包含该point的行。 |
| line(region) | [Region](api_reference#sublime.Region) | 返回region的修改副本，使其从一行的开头开始，到一行的结尾。请注意，它可能跨越几行。 |
| full\_line(point) | [Region](api_reference#sublime.Region) | 作为line()，但该区域包含尾随换行符，如果有的话。 |
| full\_line(region) | [Region](api_reference#sublime.Region) | 作为line()，但该区域包含尾随换行符，如果有的话。 |
| lines(region) | \[[Region](api_reference#sublime.Region)\] | 返回与region相交的行列表(按排序顺序)。 |
| split\_by\_newlines(region) | \[[Region](api_reference#sublime.Region)\] | 将region拆分为使得返回的每个区域仅存在于一条线上。 |
| word(point) | [Region](api_reference#sublime.Region) | 返回包含该point的单词。 |
| word(region) | [Region](api_reference#sublime.Region) | 返回region的修改副本，使其从单词的开头开始，到单词结尾处结束。请注意，它可能会跨越几个单词。 |
| classify(point) | int | 
对point进行分类，返回零或多个这些标志的按位OR：

*   sublime.CLASS\_WORD\_START
*   sublime.CLASS\_WORD\_END
*   sublime.CLASS\_PUNCTUATION\_START
*   sublime.CLASS\_PUNCTUATION\_END
*   sublime.CLASS\_SUB\_WORD\_START
*   sublime.CLASS\_SUB\_WORD\_END
*   sublime.CLASS\_LINE\_START
*   sublime.CLASS\_LINE\_END
*   sublime.CLASS\_EMPTY\_LINE

 |
| find\_by\_class(point, forward, classes, ) | [Region](api_reference#sublime.Region) | 查找与给定class匹配的point之后的下一个位置。如果forward为False，则向后搜索而不是向前搜索。classes是sublime.CLASS\_XXX标志的按位OR 。可以传入separators，以定义应该考虑哪些字符来分隔单词。 |
| expand\_by\_class(point, classes, ) | [Region](api_reference#sublime.Region) | 将point向左和向右扩展，直到每一侧都落在与class匹配的位置。classes是sublime.CLASS\_XXX标志的按位OR 。可以传入分隔符，以定义应该考虑哪些字符来分隔单词。 |
| expand\_by\_class(region, classes, ) | [Region](api_reference#sublime.Region) | 向左和向右 扩展region，直到每一侧都落在与class匹配的位置。classes是sublime.CLASS\_XXX标志的按位OR 。可以传入separators，以定义应该考虑哪些字符来分隔单词。 |
| find(pattern, start\_point, ) | [Region](api_reference#sublime.Region) | 返回匹配正则表达式模式的第一个区域，从start\_point开始，如果找不到则返回None。可选的flags参数可以是sublime.LITERAL，sublime.IGNORECASE，也可以是两个ORed在一起。 |
| find\_all(pattern, , , ) | \[[Region](api_reference#sublime.Region)\] | 返回与正则表达式模式匹配的所有(非重叠)区域。可选的flags参数可以是sublime.LITERAL，sublime.IGNORECASE，也可以是两个ORed在一起。如果给出了格式字符串，则所有匹配项将使用格式化字符串进行格式化并放入提取列表中。 |
| rowcol(point) | (int, int) | 计算的基于0的行号和列号point。 |
| text\_point(row, col) | int | 计算给定的基于0的row和col的字符偏移量。请注意，col被解释为超过行开头的字符数。 |
| set\_syntax\_file(syntax\_file) | None | 更改视图使用的语法。syntax\_file应该是Packages/Python/Python.tmLanguage的名称。要检索当前语法，请使用`view.settings().get('syntax')`。 |
| extract\_scope(point) | [Region](api_reference#sublime.Region) | 返回在给定point分配给字符的语法范围名称的范围。 |
| scope\_name(point) | str | 返回在给定point分配给字符的语法范围名称。 |
| match\_selector(point, selector) | bool | 检查selector是否在给定point的范围内，如果匹配则返回bool。 |
| score\_selector(point, selector) | int | 将selector与给定point的范围匹配，返回分数。得分为0表示不匹配，高于0表示匹配。可以将不同的选择器与相同的范围进行比较：较高的分数意味着选择器是范围的更好匹配。 |
| find\_by\_selector(selector) | \[[Region](api_reference#sublime.Region)\] | 查找与给定selector匹配的文件中的所有区域，并将它们作为列表返回。 |
| show(location, ) | None | 滚动视图以显示给定location，该location可以是[point](api_reference#type-point)，[Region](api_reference#sublime.Region)或[Selection](api_reference#sublime.Selection)。 |
| show\_at\_center(location) | None | 将视图滚动到中心location，该location可以是[point](api_reference#type-point)或[Selection](api_reference#sublime.Region)。 |
| visible\_region() | [Region](api_reference#sublime.Region) | 返回视图的当前可见区域。 |
| viewport\_position() | [vector](api_reference#type-vector) | 返回视口在布局坐标中的偏移量。 |
| set\_viewport\_position(vector, <animate<) | None | 将视口滚动到给定的布局位置。 |
| viewport\_extent() | [vector](api_reference#type-vector) | 返回视口的宽度和高度。 |
| layout\_extent() | [vector](api_reference#type-vector) | 返回布局的宽度和高度。 |
| text\_to\_layout(point) | [vector](api_reference#type-vector) | 将文本点转换为布局位置 |
| text\_to\_window(point) | [vector](api_reference#type-vector) | 将文本点转换为窗口位置 |
| layout\_to\_text(vector) | [point](api_reference#type-point) | 将布局位置转换为文本点 |
| layout\_to\_window(vector) | [vector](api_reference#type-point) | 将布局位置转换为窗口位置 |
| window\_to\_layout(vector) | [vector](api_reference#type-vector) | 将窗口位置转换为布局位置 |
| window\_to\_text(vector) | [point](api_reference#type-point) | 将窗口位置转换为文本点 |
| line\_height() | [dip](api_reference#type-dip) | 返回布局中使用的灯光高度 |
| em\_width() | [dip](api_reference#type-dip) | 返回布局中使用的典型字符宽度 |
| add\_regions(key, \[regions\], , , ) | None | 

向视图 添加一组region。如果已存在具有给定key的一组region，则它们将被覆盖。的scope被用于源颜色来绘制region中，它应该是一个范围的名称，如"comment"或"string"。如果scope为空，则不会绘制region。

可选的图标名称(如果给定)将在每个区域旁边的装订线中绘制命名图标。该icon将使用与示波器关联的颜色进行着色。有效的图标名称是point，circle和bookmark。图标名称也可以是完整的包相对路径，例如Packages/Theme - Default/dot.png。

可选的flags参数是按位组合：

*   sublime.DRAW\_EMPTY：使用垂直条绘制空白区域。默认情况下，它们根本不会被绘制。
*   sublime.HIDE\_ON\_MINIMAP：不显示小地图上的区域。
*   sublime.DRAW\_EMPTY\_AS\_OVERWRITE：使用水平条而不是垂直条绘制空区域。
*   sublime.DRAW\_NO\_FILL：禁用填充区域，只留下轮廓。
*   sublime.DRAW\_NO\_OUTLINE：禁用绘制区域的轮廓。
*   sublime.DRAW\_SOLID\_UNDERLINE：在区域下方绘制一个实线下划线。
*   sublime.DRAW\_STIPPLED\_UNDERLINE：在区域下方绘制一个点状下划线。
*   sublime.DRAW\_SQUIGGLY\_UNDERLINE：在区域下方绘制一条波浪形下划线。
*   sublime.PERSISTENT：保存会话中的区域。
*   sublime.HIDDEN：不要绘制区域。

下划线样式是独占的，无论是零还是其中一个都应该给出。如果使用下划线，通常应传入sublime.DRAW\_NO\_FILL和sublime.DRAW\_NO\_OUTLINE。

 |
| get\_regions(key) | \[[Region](api_reference#sublime.Region)\] | 返回与给定key关联的区域(如果有) |
| erase\_regions(key) | None | 删除了指定的区域 |
| set\_status(key, value) | None | 将状态key添加到视图。该value将显示在状态栏中，以逗号分隔的所有状态值列表，按键排序。将value设置为空字符串将清除状态。 |
| get\_status(key) | str | 返回与key关联的先前分配的值(如果有)。 |
| erase\_status(key) | None | 清除指定的状态。 |
| command\_history(index, ) | (str, dict, int) | 

返回存储在undo/redo堆栈中的给定历史记录条目的命令名称，命令参数和重复计数。

索引0对应于最近的命令，\-1对应于此之前的命令，依此类推。索引的正值表示在重做堆栈中查找命令。如果撤消/重做历史记录扩展得不够远，则返回(None，None，0)。

将modification\_only设置为True(默认值为False)将仅返回修改缓冲区的条目。

 |
| change\_count() | int | 返回当前更改计数。每次修改缓冲区时，更改计数都会递增。更改计数可用于确定缓冲区自上次检查后是否已更改。 |
| fold(\[regions\]) | bool | 折叠给定region，如果它们已经折叠则 返回False |
| fold(region) | bool | 折叠给定region，如果已经折叠则 返回False |
| unfold(region) | \[[Region](api_reference#sublime.Region)\] | 展现在所有文本region，返回展开区域 |
| unfold(\[regions\]) | \[[Region](api_reference#sublime.Region)\] | 展现在所有文本region，返回展开区域 |
| encoding() | str | 返回当前与文件关联的编码 |
| set\_encoding(encoding) | None | 对文件应用新编码。下次保存文件时将使用此编码。 |
| line\_endings() | str | 返回当前文件使用的行结尾。 |
| set\_line\_endings(line\_endings) | None | 设置下次保存时将应用的行结尾。 |
| overwrite\_status() | bool | 返回覆盖状态，用户通常通过插入键切换。 |
| set\_overwrite\_status(enabled) | None | 设置覆盖状态。 |
| symbols() | \[([Region](api_reference#sublime.Region), str)\] | 提取缓冲区中定义的所有符号。 |
| show\_popup\_menu(items, on\_done, ) | None | 

在插入符号处显示弹出菜单，以选择列表中的项目。on\_done将使用所选项的索引调用一次。如果取消弹出菜单，将使用参数\-1调用on\_done。

items是一个字符串列表。

flags它当前未使用。

 |
| show\_popup(content, , , , , , ) | None | 

显示显示HTML内容的弹出窗口。

flags是以下的按位组合：

*   sublime.COOPERATE\_WITH\_AUTO\_COMPLETE。使弹出窗口显示在自动完成菜单旁边
*   sublime.HIDE\_ON\_MOUSE\_MOVE。移动，单击或滚动鼠标时弹出窗口隐藏
*   sublime.HIDE\_ON\_MOUSE\_MOVE\_AWAY。移动鼠标时弹出窗口(除非弹出窗口)或单击或滚动时弹出窗口

默认location的\-1将显示在光标处弹出，否则文本点应该传递。

max\_width和max\_height设置弹出窗口的最大尺寸，之后将显示滚动条。

on\_navigate是一个回调函数，它应该接受用户单击的链接上的href属性的字符串内容。

隐藏弹出窗口时调用on\_hide。

 |
| update\_popup(content) | None | 更新当前可见弹出窗口的内容。 |
| is\_popup\_visible() | bool | 如果当前显示弹出窗口，则返回。 |
| hide\_popup() | None | 隐藏弹出窗口。 |
| is\_auto\_complete\_visible() | bool | 如果自动完成菜单当前可见，则返回。 |
| style() | dict | 返回视图的全局样式设置的dict。*所有颜色都标准化为带有前导散列的六字符十六进制形式，例如＃ff0000。* |
| style\_for\_scope(scope\_name) | dict | 接受字符串作用域名称并返回样式信息的dict，包括键foreground，bold，italic，source\_line，source\_column和source\_file。如果范围具有背景颜色集，则将显示关键background。*前景色和背景色被标准化为具有前导散列的六字符十六进制形式，例如＃ff0000。* |

## `sublime.Selection`类

维护一组区域，确保没有重叠。区域按排序顺序保存。

| 方法 | 返回值 | 描述 |
| --- | --- | --- |
| clear() | None | 删除所有区域。 |
| add(region) | None | 添加给定region。它将与集合中已包含的任何相交区域合并。 |
| add\_all(regions) | None | 添加给定list或元组中的所有区域。 |
| subtract(region) | None | 从集合中的所有区域中 减去该region。 |
| contains(region) | bool | 如果给定region是子集，则 返回True。 |

## `sublime.Region`类

表示缓冲区的一个区域。空区域，哪里`a == b`有效。

创建具有初始值a和b的Region。

| 构造函数 | 描述 |
| --- | --- |
| Region(a, b) |  |

| 属性 | 类型 | 描述 |
| --- | --- | --- |
| a | int | 该地区的第一个结束。 |
| b | int | 该地区的第二个目的地。可能少于a，在这种情况下该区域是反向的。 |
| xpos | int | 区域的目标水平位置，如果未定义，则为\-1。按向上或向下键时的效果行为。 |

| 方法 | 返回值 | 描述 |
| --- | --- | --- |
| begin() | int | 返回a和b的最小值。 |
| end() | int | 返回a和b的最大值。 |
| size() | int | 返回该区域跨越的字符数。始终> = 0。 |
| empty() | bool | 返回Trueiffbegin()\==end()。 |
| cover(region) | [Region](api_reference#sublime.Region) | 返回跨越此区域和给定区域的区域。 |
| intersection(region) | [Region](api_reference#sublime.Region) | 返回两个区域的集合交集。 |
| intersects(region) | bool | 返回True如果self ==region或两者都包含一个或多个共同的位置。 |
| contains(region) | bool | 如果给定region是子集，则 返回True。 |
| contains(point) | bool | 返回Trueiffbegin()<=point<=end()。 |

## `sublime.Phantom`类

表示基于HTML的装饰，用于显示散布在视图中的不可编辑内容。与PhantomSet一起使用，可以将幻像实际添加到View中。构建Phantom并将其添加到View后，对属性的更改将不起作用。

创建附加到region的幻像。的content是HTML由被处理[minihtml](minihtml)。

layout必须是以下之一：

*   sublime.LAYOUT\_INLINE：显示region和后续点之间的幻像。
*   sublime.LAYOUT\_BELOW：显示当前行下方空间中的幻像，与该region左对齐。
*   sublime.LAYOUT\_BLOCK：显示当前行下方空间中的幻像，与行的开头左对齐。

on\_navigate是一个可选的回调函数，它应该接受单个字符串参数，即单击链接的href属性。

| 构造函数 | 描述 |
| --- | --- |
| Phantom(region, content, layout, ) |  |

## `sublime.PhantomSet`类

一个管理虚位的集合，以及添加它们，更新它们并从视图中删除它们的过程。

创建附加到view的PhantomSet 。key是一个将Phantom组合在一起的字符串。

| 构造函数 | 描述 |
| --- | --- |
| PhantomSet(view, ) |  |

| 方法 | 返回值 | 描述 |
| --- | --- | --- |
| update(phantoms) | None | 
phantoms应该是一个虚位列表。

将更新集合中每个现有虚位的.region属性。将在视图中添加新的幻像，并且将删除不在phantoms列表中的幻像。

 |

## `sublime.Edit`类

编辑对象没有任何功能，它们存在于组缓冲区修改中。

编辑对象将传递给[TextCommand](api_reference#sublime.TextCommand)，并且无法由用户创建。使用无效的Edit对象或来自不同View的Edit对象将导致需要它们的函数失败。

| 方法 | 返回值 | 描述 |
| --- | --- | --- |
| (no methods) |  |  |

## `sublime.Window`类

[查看](api_reference#sublime.View)*或*无

| 方法 | 返回值 | 描述 |
| --- | --- | --- |
| id() | int | 返回唯一标识此窗口的数字。 |
| new\_file() | [View](api_reference#sublime.View) | 创建一个新文件。返回的视图将为空，其is\_loaded()方法将返回True。 |
| open\_file(file\_name, ) | [View](api_reference#sublime.View) | 
打开指定的文件，并返回相应的视图。如果文件已经打开，它将被带到前面。注意，文件的加载是异步的，在返回的视图操作将是不可能的，直到它的is\_loading()方法返回假。

可选的flags参数是按位组合：

*   sublime.ENCODED\_POSITION：表示应搜索file\_name：row或：row：col后缀
*   sublime.TRANSIENT：仅作为预览打开文件：在修改之前，它不会分配标签

 |
| find\_open\_file(file\_name) | [View](api_reference#sublime.View) | 在打开文件列表中查找指定文件，并返回相应的视图，如果没有打开此类文件则返回None。 |
| active\_sheet() | [Sheet](api_reference#sublime.Sheet) | 返回当前关注的工作表。 |
| active\_view() | [View](api_reference#sublime.View) | 返回当前编辑的视图。 |
| active\_sheet\_in\_group(group) | [Sheet](api_reference#sublime.Sheet) | 返回给定group中当前聚焦的工作表。 |
| active\_view\_in\_group(group) | [View](api_reference#sublime.View) | 返回给定group中当前编辑的视图。 |
| sheets() | \[[Sheet](api_reference#sublime.Sheet)\] | 返回窗口中所有打开的工作表。 |
| sheets\_in\_group(group) | \[[Sheet](api_reference#sublime.Sheet)\] | 返回给定group中的所有打开的工作表。 |
| views() | \[[View](api_reference#sublime.View)\] | 返回窗口中的所有打开视图。 |
| views\_in\_group(group) | \[[View](api_reference#sublime.View)\] | 返回给定group中的所有打开视图。 |
| num\_groups() | int | 返回窗口中的视图组数。 |
| active\_group() | int | 返回当前所选组的索引。 |
| focus\_group(group) | None | 使给定group处于活动状态。 |
| focus\_sheet(sheet) | None | 切换到给定的工作sheet。 |
| focus\_view(view) | None | 切换到给定view。 |
| get\_sheet\_index(sheet) | (int, int) | 返回该组的内的组，和索引sheet。如果未找到，则返回\-1。 |
| set\_sheet\_index(sheet, group, index) | None | 将工作sheet移动到给定的group和index。 |
| get\_view\_index(view) | (int, int) | 返回view组内的组和索引。如果未找到，则返回\-1。 |
| set\_view\_index(view, group, index) | None | 将view移动到给定的group和索引。 |
| status\_message(string) | None | 在状态栏中显示消息。 |
| is\_menu\_visible() | bool | 如果菜单可见，则 返回True。 |
| set\_menu\_visible(flag) | None | 控制菜单是否可见。 |
| is\_sidebar\_visible() | bool | 如果内容可用时将显示侧栏，则 返回True。 |
| set\_sidebar\_visible(flag) | None | 设置内容可用时要显示或隐藏的侧边栏。 |
| get\_tabs\_visible() | bool | 如果将显示打开文件的选项卡，则 返回True。 |
| set\_tabs\_visible(flag) | None | 控制是否显示打开文件的选项卡。 |
| is\_minimap\_visible() | bool | 如果启用了小地图，则 返回True。 |
| set\_minimap\_visible(flag) | None | 控制小地图的可见性。 |
| is\_status\_bar\_visible() | bool | 如果将显示状态栏，则 返回True。 |
| set\_status\_bar\_visible(flag) | None | 控制状态栏的可见性。 |
| folders() | \[str\] | 返回当前打开的文件夹列表。 |
| project\_file\_name() | str | 返回当前打开的项目文件的名称(如果有)。 |
| project\_data() | dict | 返回与当前窗口关联的项目数据。数据的格式与.sublime-project文件的内容相同。 |
| set\_project\_data(data) | None | 更新与当前窗口关联的项目数据。如果窗口与.sublime-project文件关联，则项目文件将在磁盘上更新，否则窗口将在内部存储数据。 |
| run\_command(string, ) | None | 使用(可选)给定的args运行命名的[WindowCommand](api_reference#sublime.WindowCommand)。此方法能够运行任何类型的命令，通过输入焦点调度命令。 |
| show\_quick\_panel(items, on\_done, , , ) | None | 

显示快速面板，以选择列表中的项目。on\_done将使用所选项的索引调用一次。如果快速面板被取消，将使用参数\-1调用on\_done。

items可以是字符串列表，也可以是字符串列表列表。在后一种情况下，快捷面板中的每个条目都将显示多行。

flags是sublime.MONOSPACE\_FONT和sublime.KEEP\_OPEN\_ON\_FOCUS\_LOST的按位OR。

on\_highlighted，如果给定，将在每次更改快速面板中突出显示的项目时调用。

 |
| show\_input\_panel(caption, initial\_text, on\_done, on\_change, on\_cancel) | [View](api_reference#sublime.View) | 显示输入面板，以从用户收集一行输入。on\_done和on\_change，如果不是None，都应该是期望单个字符串参数的函数。on\_cancel应该是一个不需要参数的函数。返回用于输入窗口小部件的视图。 |
| create\_output\_panel(name, ) | [View](api_reference#sublime.View) | 

返回与命名输出面板关联的视图，并根据需要创建它。可以通过运行show\_panel窗口命令来显示输出面板，并将panel参数设置为带有"output"的名称。字首。

可选的未列出参数是一个布尔值，用于控制是否应在面板切换器中列出输出面板。

 |
| find\_output\_panel(name) |  | 返回与命名输出面板关联的视图，如果输出面板不存在，则返回None。 |
| destroy\_output\_panel(name) | None | 销毁命名的输出面板，如果当前打开则将其隐藏。 |
| active\_panel() | str*or*None | 返回当前打开的面板的名称，如果没有打开面板，则返回None。除输出面板外，还将返回内置面板名称(例如"console"，"find"等)。 |
| panels() | \[str\] | 返回尚未标记为未列出的所有面板的名称列表。除输出面板外，还包括某些内置面板。 |
| lookup\_symbol\_in\_index(symbol) | \[[location](api_reference#type-location)\] | 返回在当前项目中的文件之间定义符号的所有位置。 |
| lookup\_symbol\_in\_open\_files(symbol) | \[[location](api_reference#type-location)\] | 返回跨打开文件定义符号的所有位置。 |
| extract\_variables() | dict | 

返回使用上下文键填充的字符串字典：

包，平台，文件，file\_path，file\_name，file\_base\_name，file\_extension，文件夹，项目，project\_path，project\_name，project\_base\_name，project\_extension。此dict适合传递给sublime.expand\_variables()。

 |

## `sublime.Settings`类

| 方法 | 返回值 | 描述 |
| --- | --- | --- |
| get(name, ) | [value](api_reference#type-value) | 返回命名设置，如果未定义，则返回默认值。如果未通过，则默认值为None。 |
| set(name, value) | None | 设置命名设置。只接受原始类型，列表和dicts。 |
| erase(name) | None | 删除命名设置。不会从任何父设置中删除它。 |
| has(name) | bool | 如果此设置集或其父项之一中存在命名选项，则 返回True。 |
| add\_on\_change(key, on\_change) | None | 只要更改此对象中的设置，就注册要运行的回调。 |
| clear\_on\_change(key) | None | 删除使用给定密钥注册的所有回调。 |

## `sublime_plugin`模块

| 方法 | 返回值 | 描述 |
| --- | --- | --- |
| (no methods) |  |  |

## `sublime_plugin.EventListener`类

请注意，其中许多事件都是由视图底层的缓冲区触发的，因此该方法只调用一次，第一个视图作为参数。

| 方法 | 返回值 | 描述 |
| --- | --- | --- |
| on\_new(view) | None | 在创建新缓冲区时调用。 |
| on\_new\_async(view) | None | 在创建新缓冲区时调用。在单独的线程中运行，不会阻止应用程序。 |
| on\_clone(view) | None | 从现有视图克隆视图时调用。 |
| on\_clone\_async(view) | None | 从现有视图克隆视图时调用。在单独的线程中运行，不会阻止应用程序。 |
| on\_load(view) | None | 文件加载完成后调用。 |
| on\_load\_async(view) | None | 文件加载完成后调用。在单独的线程中运行，不会阻止应用程序。 |
| on\_pre\_close(view) | None | 在视图即将关闭时调用。此时视图仍将在窗口中。 |
| on\_close(view) | None | 视图关闭时调用(注意，可能仍有其他视图进入同一缓冲区)。 |
| on\_pre\_save(view) | None | 在视图保存之前调用。 |
| on\_pre\_save\_async(view) | None | 在视图保存之前调用。在单独的线程中运行，不会阻止应用程序。 |
| on\_post\_save(view) | None | 视图保存后调用。 |
| on\_post\_save\_async(view) | None | 视图保存后调用。在单独的线程中运行，不会阻止应用程序。 |
| on\_modified(view) | None | 在对视图进行更改后调用。 |
| on\_modified\_async(view) | None | 在对视图进行更改后调用。在单独的线程中运行，不会阻止应用程序。 |
| on\_selection\_modified(view) | None | 在视图中修改选择后调用。 |
| on\_selection\_modified\_async(view) | None | 在视图中修改选择后调用。在单独的线程中运行，不会阻止应用程序。 |
| on\_activated(view) | None | 当视图获得输入焦点时调用。 |
| on\_activated\_async(view) | None | 当视图获得输入焦点时调用。在单独的线程中运行，不会阻止应用程序。 |
| on\_deactivated(view) | None | 视图丢失输入焦点时调用。 |
| on\_deactivated\_async(view) | None | 视图丢失输入焦点时调用。在单独的线程中运行，不会阻止应用程序。 |
| on\_hover(view, point, hover\_zone) | None | 
当用户的鼠标悬停在视图上一小段时间时调用。

point是鼠标位置视图中的最近点。根据hover\_zone的值，鼠标实际上可能并不相邻：

*   sublime.HOVER\_TEXT：当鼠标悬停在文本上时。
*   sublime.HOVER\_GUTTER：当鼠标悬停在排水沟上方时。
*   sublime.HOVER\_MARGIN：当鼠标悬停在一行右侧的空白处时。

 |
| on\_query\_context(view, key, operator, operand, match\_all) | bool*or*None | 

在确定触发与给定上下文密钥的密钥绑定时调用。如果插件知道如何响应上下文，它应返回TrueofFalse。如果上下文未知，则应返回None。

运营商是以下之一：

*   sublime.OP\_EQUAL：上下文的值是否等于操作数？
*   sublime.OP\_NOT\_EQUAL：上下文的值是否不等于操作数？
*   sublime.OP\_REGEX\_MATCH：上下文的值是否与操作数中给出的正则表达式匹配？
*   sublime.OP\_NOT\_REGEX\_MATCH：上下文的值是否与操作数中给出的正则表达式不匹配？
*   sublime.OP\_REGEX\_CONTAINS：上下文的值是否包含与操作数中给出的正则表达式匹配的子字符串？
*   sublime.OP\_NOT\_REGEX\_CONTAINS：上下文的值是否包含与操作数中给出的正则表达式匹配的子字符串？

如果上下文与选择相关，则应使用match\_all：每个选择是否必须匹配(match\_all == True)，或者至少是一个匹配(match\_all == False)？

 |
| on\_query\_completions(view, prefix, locations) | list*,*tuple*or*None | 

每当完成将被呈现给用户时调用。该前缀是文本完成的unicode字符串。

location是一个[点](api_reference#type-point)列表。由于无论语法如何`view.match_selector(point, relevant_scope)`都要为每个视图中的所有完成调用此方法，因此应调用此方法以确定该点是否相关。

返回值必须是以下格式之一：

*   无：未提供完成
    
    ~~~python
    return None
    ~~~
    
*   2元素列表/元组的列表。第一个元素是完成触发器的unicode字符串，第二个元素是unicode替换文本。
    
    ~~~python
    return [["me1", "method1()"], ["me2", "method2()"]]
    ~~~
    
    触发器可能包含一个制表符(\\t)，后跟一个提示，显示在完成框的右侧。
    
    ~~~python
    return [
        ["me1\tmethod", "method1()"],
        ["me2\tmethod", "method2()"]
    ]
    ~~~
    
    替换文本可能包含美元数字字段，例如片段，例如$ 0，$ 1。
    
    ~~~python
    return [
        ["fn", "def ${1:name}($2) { $0 }"],
        ["for", "for ($1; $2; $3) { $0 }"]
    ]
    ~~~
    
*   一个2元素元组，第一个元素是上面列出的列表格式，第二个元素是以下列表中的位标志：
    
    *   sublime.INHIBIT\_WORD\_COMPLETIONS：防止Sublime Text根据视图内容显示完成情况
    *   sublime.INHIBIT\_EXPLICIT\_COMPLETIONS：防止Sublime Text显示基于.sublime-completions文件的完成
    
    ~~~python
    return (
        [
            ["me1", "method1()"],
            ["me2", "method2()"]
        ],
        sublime.INHIBIT_WORD_COMPLETIONS | sublime.INHIBIT_EXPLICIT_COMPLETIONS
    )
    ~~~
    

 |
| on\_text\_command(view, command\_name, args) | (str, dict) | 发出文本命令时调用。监听器可以返回一个(command, arguments)元组来重写命令，或者返回None来运行未修改的命令。 |
| on\_window\_command(window, command\_name, args) | (str, dict) | 发出窗口命令时调用。监听器可以返回一个(command, arguments)元组来重写命令，或者返回None来运行未修改的命令。 |
| on\_post\_text\_command(view, command\_name, args) | None | 在执行文本命令后调用。 |
| on\_post\_window\_command(window, command\_name, args) | None | 在执行窗口命令后调用。 |

## `sublime_plugin.ViewEventListener`类

一个类，它为[EventListener](api_reference#sublime.EventListener)提供类似的事件处理，但绑定到特定视图。提供基于类方法的过滤，以控制为其创建的视图对象。

视图作为单个参数传递给构造函数。默认实现通过self.view使视图可用。

| 分类方法 | 返回值 | 描述 |
| --- | --- | --- |
| is\_applicable(settings) | bool | 一个@classmethod接收一个[设置](api_reference#sublime.Settings)对象和应返回布尔指示此类适用于使用这些设置的图 |
| applies\_to\_primary\_view\_only() | bool | 一个@classmethod应该返回一个布尔值，表示如果该类只适用于一个文件中的主视图。如果视图是文件的唯一视图或第一视图，则视图被视为主视图。 |

| 方法 | 返回值 | 描述 |
| --- | --- | --- |
| on\_load() | None | 文件加载完成后调用。 |
| on\_load\_async() | None | 文件加载完成后调用。在单独的线程中运行，不会阻止应用程序。 |
| on\_pre\_close() | None | 在视图即将关闭时调用。此时视图仍将在窗口中。 |
| on\_close() | None | 视图关闭时调用(注意，可能仍有其他视图进入同一缓冲区)。 |
| on\_pre\_save() | None | 在视图保存之前调用。 |
| on\_pre\_save\_async() | None | 在视图保存之前调用。在单独的线程中运行，不会阻止应用程序。 |
| on\_post\_save() | None | 视图保存后调用。 |
| on\_post\_save\_async() | None | 视图保存后调用。在单独的线程中运行，不会阻止应用程序。 |
| on\_modified() | None | 在对视图进行更改后调用。 |
| on\_modified\_async() | None | 在对视图进行更改后调用。在单独的线程中运行，不会阻止应用程序。 |
| on\_selection\_modified() | None | 在视图中修改选择后调用。 |
| on\_selection\_modified\_async() | None | 在视图中修改选择后调用。在单独的线程中运行，不会阻止应用程序。 |
| on\_activated() | None | 当视图获得输入焦点时调用。 |
| on\_activated\_async() | None | 当视图获得输入焦点时调用。在单独的线程中运行，不会阻止应用程序。 |
| on\_deactivated() | None | 视图失去输入焦点时调用。 |
| on\_deactivated\_async() | None | 视图失去输入焦点时调用。在单独的线程中运行，不会阻止应用程序。 |
| on\_hover(point, hover\_zone) | None | 
当用户的鼠标在视图上盘旋一小段时间时调用。

point是鼠标位置视图中的最近点。根据hover\_zone的值，鼠标实际上可能并不相邻：

*   sublime.HOVER\_TEXT：当鼠标悬停在文本上时。
*   sublime.HOVER\_GUTTER：当鼠标悬停在排水沟上方时。
*   sublime.HOVER\_MARGIN：当鼠标悬停在一行右侧的空白处时。

 |
| on\_query\_context(key, operator, operand, match\_all) | bool*or*None | 

在确定触发与给定上下文密钥的密钥绑定时调用。如果插件知道如何响应上下文，它应返回TrueofFalse。如果上下文未知，则应返回None。

运营商是以下之一：

*   sublime.OP\_EQUAL：上下文的值是否等于操作数？
*   sublime.OP\_NOT\_EQUAL：上下文的值是否不等于操作数？
*   sublime.OP\_REGEX\_MATCH：上下文的值是否与操作数中给出的正则表达式匹配？
*   sublime.OP\_NOT\_REGEX\_MATCH：上下文的值是否与操作数中给出的正则表达式不匹配？
*   sublime.OP\_REGEX\_CONTAINS：上下文的值是否包含与操作数中给出的正则表达式匹配的子字符串？
*   sublime.OP\_NOT\_REGEX\_CONTAINS：上下文的值是否包含与操作数中给出的正则表达式匹配的子字符串？

如果上下文与选择相关，则应使用match\_all：每个选择是否必须匹配(match\_all == True)，或者至少是一个匹配(match\_all == False)？

 |
| on\_query\_completions(prefix, locations) | list*,*tuple*or*None | 

每当完成将被呈现给用户时调用。该前缀是文本完成的unicode字符串。

location是一个[点](api_reference#type-point)列表。由于无论语法如何`self.view.match_selector(point, relevant_scope)`都要为所有完成调用此方法，因此应调用此方法以确定该点是否相关。

返回值必须是以下格式之一：

*   无：未提供完成
    
    ~~~python
    return None
    ~~~
    
*   2元素列表/元组的列表。第一个元素是完成触发器的unicode字符串，第二个元素是unicode替换文本。
    
    ~~~python
    return [["me1", "method1()"], ["me2", "method2()"]]
    ~~~
    
    触发器可能包含一个制表符(\\ t)，后跟一个提示，显示在完成框的右侧。
    
    ~~~python
    return [
        ["me1\tmethod", "method1()"],
        ["me2\tmethod", "method2()"]
    ]
    ~~~
    
    替换文本可能包含美元数字字段，例如片段，例如$ 0，$ 1。
    
    ~~~python
    return [
        ["fn", "def ${1:name}($2) { $0 }"],
        ["for", "for ($1; $2; $3) { $0 }"]
    ]
    ~~~
    
*   一个2元素元组，第一个元素是上面列出的列表格式，第二个元素是以下列表中的位标志：
    
    *   sublime.INHIBIT\_WORD\_COMPLETIONS：防止Sublime Text根据视图内容显示完成情况
    *   sublime.INHIBIT\_EXPLICIT\_COMPLETIONS：防止Sublime Text显示基于.sublime-completions文件的完成
    
    ~~~python
    return (
        [
            ["me1", "method1()"],
            ["me2", "method2()"]
        ],
        sublime.INHIBIT_WORD_COMPLETIONS | sublime.INHIBIT_EXPLICIT_COMPLETIONS
    )
    ~~~
    

 |
| on\_text\_command(command\_name, args) | (str, dict) | 发出文本命令时调用。监听器可以返回一个(command\_name, args)元组来重写命令，或者返回None来运行未修改的命令。 |
| on\_post\_text\_command(command\_name, args) | None | 在执行文本命令后调用。 |

## `sublime_plugin.ApplicationCommand`类

[CommandInputHandler](api_reference#type-CommandInputHandler)*或*None

| 方法 | 返回值 | 描述 |
| --- | --- | --- |
| run() | None | 运行命令时调用。 |
| is\_enabled() | bool | 如果此时能够运行该命令，则 返回True。默认实现总是返回True。 |
| is\_visible() | bool | 如果此时命令应显示在菜单中，则 返回True。默认实现始终返回True。 |
| is\_checked() | bool | 如果菜单项旁边应显示一个复选框，则 返回True。该.sublime菜单文件必须设置为复选框属性真正被用于此目的。 |
| description() | str | 返回具有给定参数的命令的描述。如果没有提供标题，则在菜单中使用。返回None以获取默认描述。 |
| input(args) |  | 如果返回None以外的其他内容，则会在命令选项板中运行命令之前提示用户输入。 |

## `sublime_plugin.WindowCommand`类

WindowCommands每个窗口实例化一次。可以通过self.window检索Window对象

[CommandInputHandler](api_reference#type-CommandInputHandler)*或*None

| 方法 | 返回值 | 描述 |
| --- | --- | --- |
| run() | None | 运行命令时调用。 |
| is\_enabled() | bool | 如果此时能够运行该命令，则 返回True。默认实现总是返回True。 |
| is\_visible() | bool | 如果此时命令应显示在菜单中，则 返回True。默认实现始终返回True。 |
| description() | str | 返回具有给定参数的命令的描述。如果没有提供标题，则在菜单中使用。返回None以获取默认描述。 |
| input(args) |  | 如果返回None以外的其他内容，则会在命令选项板中运行命令之前提示用户输入。 |

## `sublime_plugin.TextCommand`类

每个视图都会实例化一次TextCommands。可以通过self.view检索View对象

[CommandInputHandler](api_reference#type-CommandInputHandler)*或*None

| 方法 | 返回值 | 描述 |
| --- | --- | --- |
| run(edit, ) | None | 运行命令时调用。 |
| is\_enabled() | bool | 如果此时能够运行该命令，则 返回True。默认实现总是返回True。 |
| is\_visible() | bool | 如果此时命令应显示在菜单中，则 返回True。默认实现始终返回True。 |
| description() | str | 返回具有给定参数的命令的描述。用于菜单和撤消/重做描述。返回None以获取默认描述。 |
| want\_event() | bool | 返回True以在鼠标操作触发命令时接收事件参数。事件信息允许命令确定单击视图的哪个部分。默认实现返回False。 |
| input(args) |  | 如果返回None以外的其他内容，则会在命令选项板中运行命令之前提示用户输入。 |

## `sublime_plugin.TextInputHandler`类

TextInputHandlers可用于接受命令选项板中的文本输入。从命令的input()方法返回this的子类。

[CommandInputHandler](api_reference#type-CommandInputHandler)*或*None

| 方法 | 返回值 | 描述 |
| --- | --- | --- |
| name() | str | 此输入处理程序正在编辑的命令参数名称。对于名为*FooBarInputHandler*的输入处理程序，默认为*foo\_bar* |
| placeholder() | str | 在用户输入任何内容之前，占位符文本显示在文本输入框中。默认为空。 |
| initial\_text() | str | 文本输入框中显示的初始文本。默认为空。 |
| preview(text) | str*or*sublime(str) | 每当用户更改输入框中的文本时调用。返回值(纯文本或HTML)将显示在命令选项板的预览区域中。 |
| validate(text) | bool | 每当用户在文本输入框中按Enter键时调用。返回False以禁止当前值。 |
| cancel() | None | 当取消输入处理程序时，用户按退格键或退出时调用。 |
| confirm(text) | None | 在接受输入时调用，在用户按下回车并且文本已经过验证之后。 |
| next\_input(args) |  | 用户完成此输入后返回下一个输入。可以返回None表示不再需要输入，或者返回sublime\_plugin.BackInputHandler()以指示输入处理程序应该从堆栈中取出。 |
| description(text) | str | 当此输入处理程序不在输入处理程序堆栈顶部时，在Command Palette中显示的文本。默认为用户输入的文本。 |

## `sublime_plugin.ListInputHandler`类

ListInputHandlers可用于接受命令选项板中列表项的选择输入。从命令的input()方法返回this的子类。

| 方法 | 返回值 | 描述 |
| --- | --- | --- |
| name() | str | 此输入处理程序正在编辑的命令参数名称。对于名为*FooBarInputHandler*的输入处理程序，默认为*foo\_bar* |
| list\_items() | \[str\]*or*\[(str,value)\] | 要在列表中显示的项目。如果返回(str，value)元组的列表，则str将显示给用户，而该值将用作命令参数。
(可选)返回(list\_items，selected\_item\_index)元组以指示初始选择。

 |
| placeholder() | str | 在用户输入任何内容之前，占位符文本显示在文本输入框中。默认为空。 |
| initial\_text() | str | 过滤器框中显示的初始文本。默认为空。 |
| preview(value) | str*or*sublime(str) | 每当用户更改所选项目时调用。返回值(纯文本或HTML)将显示在命令选项板的预览区域中。 |
| validate(value) | bool | 每当用户在文本输入框中按Enter键时调用。返回False以禁止当前值。 |
| cancel() | None | 当取消输入处理程序时，用户按退格键或退出时调用。 |
| confirm(value) | None | 在接受输入时调用，在用户按下enter并且项目已经过验证后调用。 |
| next\_input(args) | [CommandInputHandler](api_reference#type-CommandInputHandler)*or*None | 用户完成此输入后返回下一个输入。可以返回None表示不再需要输入，或者返回sublime\_plugin.BackInputHandler()以指示输入处理程序应该从堆栈中取出。 |
| description(value, text) | str | 当此输入处理程序不在输入处理程序堆栈顶部时，在Command Palette中显示的文本。默认为用户选择的列表项的文本。 |
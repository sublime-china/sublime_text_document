# 菜单

Sublime Text中的菜单由以.sublime-菜单结尾的文件定义。菜单使用JSON，顶级结构为数组。每个绑定都是一个JSON对象，其中包含用于定义菜单条目的文本及其应执行的操作的信息。

## Example

* 以下是.sublime-menu 文件格式的示例。*

~~~
[
    {
        "caption": "File",
        "mnemonic": "F",
        "id": "file",
        "children":
        [
            { "command": "new_file", "caption": "New File", "mnemonic": "N" },

            { "command": "prompt_open_file", "caption": "Open File…", "mnemonic": "O", "platform": "!OSX" },
            { "command": "prompt_open_folder", "caption": "Open Folder…", "platform": "!OSX" },
            { "command": "prompt_open", "caption": "Open…", "platform": "OSX" }
        ]
    }
]
~~~

## 条目

每个菜单项都是具有一个或多个键的JSON对象。支持的键列表包括:

"caption"

菜单项的文本

"mnemonic"

用作激活条目的键的字符。仅适用于Windows和Linux。必须与 "caption" 中字符的大小写匹配。

"command"

激活条目时要执行的命令的字符串

"args"

要发送到命令的args的JSON对象

"children"

用于创建子菜单的JSON条目数组

"id"

菜单项的唯一字符串。用于带有 "children" 的菜单项，以允许添加其他子级条目。

"platform"

字符串之一: `"OSX"`，`"!OSX"`，`"Windows"`，`"!Windows"`，`"Linux"` 或 `"!Linux"`。控制条目显示在哪个平台上。

每个菜单条目至少需要非功能条目的键 `"caption"`，或执行操作的条目的 `"command"`。

## 可用菜单

Sublime Text有七个可以自定义的菜单:

Main.sublime-menu

>应用程序的主菜单

Side Bar Mount Point.sublime-menu

>侧栏中顶级文件夹的上下文菜单

Side Bar.sublime-menu

>侧栏中文件和文件夹的上下文菜单。具有用于将文件和文件夹名称传递给命令的 “魔术” 参数。

>将为文件启用带有arg `"files": []` 的条目，并通过arg`files` 将文件名传递给命令。将为文件夹启用带有arg`"dirs": []`的条目，并通过arg`dirs` 将文件名传递给命令。将为文件和文件夹启用带有arg`"paths": []`的条目，并通过arg`paths`将文件和文件夹名称传递给命令。 

Tab Context.sublime-menu

>文件选项卡的上下文菜单

Context.sublime-menu

>文本区域的上下文菜单

Find in Files.sublime-menu

>单击 `在文件中查找` 面板中的 `...` 按钮时显示的菜单

Widget Context.sublime-menu

> 各种面板中文本输入的上下文菜单。* 从技术上讲，可以通过Widget.sublime-settings内部的上下文菜单设置来更改此文件名。

## 添加自菜单

使用条目的 "id" 键，可以将条目添加到子菜单。添加子菜单条目时，仅指定父条目的 "id" 和 "children" 键，并将 "children" 的值设置为要附加到子菜单的条目数组。

例如，向视图添加新布局![▶](images/right.svg)布局菜单, 创建条目，例如:

~~~
[
    {
        "id": "layout",
        "children":
        [
            {
                "caption": "Grid: 9",
                "command": "set_layout",
                "args":
                {
                    "cols": [0.0, 0.33, 0.66, 1.0],
                    "rows": [0.0, 0.33, 0.66, 1.0],
                    "cells":
                    [
                        [0, 0, 1, 1], [1, 0, 2, 1], [2, 0, 3, 1],
                        [0, 1, 1, 2], [1, 1, 2, 2], [2, 1, 3, 2],
                        [0, 2, 1, 3], [1, 2, 2, 3], [2, 2, 3, 3]
                    ]
                }
            },
        ]
    }
]
~~~

要在Main.sublime-菜单中查找条目的 "id"，请使用命令选项板中的视图包文件管理器，然后选择 default/Main.sublime-menu。

## 定制化

用户可以通过在包/用户/目录中创建一个适当命名的文件来自定义可用菜单。

例如，要自定义侧栏中文件和文件夹的上下文菜单，请创建名为package/User/side bar.sublime-菜单的文件。添加以下内容将创建一个条目，该条目将执行将路径复制到剪贴板的 (假设) 命令。

~~~
[
    {
        "caption": "Copy Full Path",
        "mnemonic": "y",
        "command": "copy_path",
        "args": {"paths": []}
    }
]
~~~
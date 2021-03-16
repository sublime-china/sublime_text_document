# 按键绑定

Sublime Text中的键绑定由以.sublime-keymap结尾的文件定义。键绑定使用JSON，顶级结构为数组。每个绑定都是一个JSON对象。

## Example

*以下是.sublime-keymap文件格式的示例。*

~~~
[
    {
        "keys": ["super+ctrl+m"],
        "command": "convert_syntax"
    },
    {
        "keys": ["super+shift+9"],
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
    {
        "keys": ["super+alt+up"],
        "command": "noop",
        "context":
        [
            {"key": "panel", "operand": "find"},
            {"key": "panel_has_focus"},
        ]
    }
]
~~~

## 绑定

每个键绑定需要两个键，"keys" 和 "command" 。要将args传递给命令，应指定 "args" 键。要将键绑定限制为特定情况，必须包括 "context" 键。

### "keys"KEY

"keys" 值必须是一个字符串数组，其中每个字符串包含一个 *按键*，由一个键和任何修饰符组成。当数组中存在多个按键时，只有按顺序执行该命令时，才会调用该命令。

*绑定**Escape**键*

~~~
{
    "keys": ["escape"],
    "command": "noop"
}
~~~

*绑定**A**修饰键**Ctrl***

~~~
{
    "keys": ["ctrl+a"],
    "command": "noop"
}
~~~

#### 修饰键

以下修饰符可以与每个 *按键* 的按键名称组合在一起。

*   `ctrl`
*   `control`
*   `alt`
*   `option`\- Mac
*   `command`\- Mac
*   `super`\- the**Windows**key on Windows and Linux, or**⌘**on Mac
*   `primary`\-**Ctrl**on Windows and Linux, or**⌘**on Mac

#### KEY NAMES

键名称由印在键上的 (非移位) 字符或键名称指定:

| - | - | - |           -            |     -     |     -      |  -  |
|:-:|:-:|:-:|:----------------------:|:---------:|:----------:|:---:|
| a | n | 0 |           ,            |    up     |  keypad0   | f1  |
| b | o | 1 |           .            |   down    |  keypad1   | f2  |
| c | p | 2 |           \\           |   left    |  keypad2   | f3  |
| d | q | 3 |           /            |   right   |  keypad3   | f4  |
| e | r | 4 |           ;            |  insert   |  keypad4   | f5  |
| f | s | 5 |           '            |   home    |  keypad5   | f6  |
| g | t | 6 | ` | end | keypad6 | f7 |           |            |     |
| h | u | 7 |           +            |  pageup   |  keypad7   | f8  |
| i | v | 8 |           \=           | pagedown  |  keypad8   | f9  |
| j | w | 9 |           \[           | backspace |  keypad9   | f10 |
| k | x | 0 |           \]           |  delete   | keypad\_pe | f11 |
| l | y |   |                        |    tab    | keypad\_di | f12 |
| m | z |   |                        |   enter   | keypad\_mu | f13 |
|   |   |   |                        |   pause   | keypad\_mi | f14 |
|   |   |   |                        |  escape   | keypad\_pl | f15 |
|   |   |   |                        |   space   | keypad\_en | f16 |
|   |   |   |                        |           |   clear    | f17 |
|   |   |   |                        |           |            | f18 |
|   |   |   |                        |           |            | f19 |
|   |   |   |                        |           |            | f20 |

### "command"KEY

"command" 键指定检测到按键时要执行的命令的名称。该命令可以是内置命令，也可以是由插件实现的命令。

~~~
{
    "keys": ["ctrl+a"],
    "command": "select_all"
}
~~~

*目前没有所有内置命令的编译列表。通过查看 Default ({PLATFORM\_name}).sublime-keymap 文件可以找到许多命令的名称。*


### "args"KEY

发送到 "command" 键的参数可以由JSON对象在 "args" 键下指定。

~~~
{
    "keys": ["primary+shift+b"],
    "command": "build",
    "args": {"select": true}
}
~~~

### "context"KEY

为了允许根据情况做出不同反应的键绑定，"context" 键允许指定一个或多个条件，这些条件必须计算为true才能使键绑定处于活动状态。

"context" 值是对象的数组。每个对象必须包含具有字符串值的 "key" 键。键是可以使用 “运算符” 和 “操作数” 进行比较的预定义值列表之一。

如果 "key" 支持 *equailty* 运算符，则 "运算符" 必须是以下之一: "equal" 或 "not\_equal"。对于 *matching*，它必须是以下之一: "regex\_match"， "not\_regex\_match"，"regex\_contains" 或 "not\_regex\_contains"。默认的 "operator" 为 "equal"，默认的 "operand" 为 `true`。

对于处理选择的 "key" 值，支持附加键 "match\_all"。这默认为 `false`，这意味着条件只需要对单个选择求值为true。如果 "match\_all" 为 `true`，则所有选择的条件必须计算为true。

以下是有效上下文 "key" 值的列表:

| Key                        | Operand | Operators                                                      | Match All | Description                                       |
|----------------------------|---------|----------------------------------------------------------------|-----------|---------------------------------------------------|
| "auto\_complete\_visible"  | boolean | equality                                                       | no        | 是否自动完成下拉列表可见                          |
| "eol\_selector"            | string  | equality                                                       | yes       | 一个用于匹配当前行中命名范围的[选择器](selectors) |
| "following\_text"          | string  | matching                                                       | yes       | 选择后的文本                                      |
| "has\_next\_field"         | boolean | equality                                                       | no        | 是否 选择是片段中存在后续字段的字段               |
| "has\_prev\_field"         | boolean | equality                                                       | no        | 是否选择是片段中存在先前字段的字段                |
| "is\_recording\_macro"     | boolean | equality                                                       | no        | 是否用户当前正在记录宏                            |
| "last\_command"            | string  | equality                                                       | no        | 最后运行的命令的名称                              |
| "last\_modifying\_command" | string  | equality                                                       | no        | 上次运行修改缓冲区的命令的名称                    |
| "num\_selections"          | integer | equality                                                       | no        | 当前缓冲区中的选择数量                            |
| "popup\_visible"           | boolean | equality                                                       | no        | 是否弹出窗口当前正在显示                          |
|                            |         |                                                                |           |                                                   |
| "preceding\_text"          | string  |                                                                |           |                                                   |
| matching                   | yes     | The text before the selection                                  |           |                                                   |
| "read\_only"               | boolean | equality                                                       | no        | 是否缓冲区被标记为只读                            |
| "selection\_empty"         | boolean |                                                                |           |                                                   |
| equality                   | yes     | 是否当前选择不包含任何字符                                     |           |                                                   |
| "selector"                 | string  |                                                                |           |                                                   |
| equality                   | yes     | A[selector](selectors)to match the scope name of the selection |           |                                                   |
| "text"                     | string  |                                                                |           |                                                   |
| matching                   | yes     | The text of the selection                                      |           |                                                   |
| "panel"                    | string  | equality                                                       | no        | 当前面板的名称                                    |
| "panel\_visible"           | boolean | equality                                                       | no        | 是否面板可见                                      |
| "panel\_has\_focus"        | boolean | equality                                                       | no        | 是否具有焦点的面板可见并                          |
| "overlay\_visible"         | boolean | equality                                                       | no        | 快速面板是否可见                                  |

## 用户绑定

用户可以通过在 包/用户/目录 中创建一个名为default.sublime-keymap的文件来自定义其键绑定。

例如，以下将创建键绑定显示未保存的更改，如果存在，通过 **Ctrl***+***Shift***+***\`**。

~~~
[
    {
        "keys": ["ctrl+shift+`"],
        "command": "diff_changes"
    }
]
~~~
# 

[DOCUMENTATION](index)[TOC](vintage#toc)[TOP](vintage#)

Vintage Mode 复古模式

Vintage是一个用于Sublime Text的vi模式编辑包。它允许您将vi的命令模式与Sublime Text的功能相结合，包括多种选择。

复古模式是公开开发的，补丁是非常受欢迎的。如果你想贡献参见[GitHub repo](https://github.com/sublimehq/Vintage).

*   [Enabling Vintage](vintage#enabling_vintage)
*   [What’s Included](vintage#included)
*   [What’s Not](vintage#not_included)
*   [Under the Hood](vintage#under_the_hood)
*   [Mac](vintage#mac)
*   [Ctrl Keys](vintage#ctrl_keys)
*   [Ex Mode](vintage#ex_mode)

## 启用
默认情况下，通过 ignored\_packages 设置禁用Vintage。如果您从被忽略的包列表中删除 `"Vintage"`，您将能够使用vi键进行编辑:

1.  选择首选项 ![▶](images/right.svg)设置菜单项
2.  编辑 ignored\_packages setting, 改变:`"ignored_packages": ["Vintage"]` 到:`"ignored_packages": []` 设置 然后保存文件.
3.  复古模式启用了 – 你会看到  "INSERT MODE" 列在状态栏上

默认情况下，Vintage以插入模式开始。可以通过在用户设置中添加以下设置来更改此设置:

~~~
"vintage_start_in_command_mode": true
~~~

## 包含什么

Vintage包括最基本的动作: d (删除) 、y (复制) 、c (更改) 、gu (小写) 、gU (大写) 、g ~ (交换大小写) 、g？(第13页)，(缩进)。

它还包括许多动作，包括l、h、j、k、W、w、e、E、b、B、alt + w (子词移动)，alt + W (通过子词向后移动)，$，^，%，0，G，gg，f，F，t，T，^ f，^ b，H，M，和l。

支持文本对象，包括单词、引号、括号和标签。

重复 (.) 在那里，就像指定命令和动作的计数一样。支持寄存器，宏和书签也是如此。也支持许多其他杂项命令，例如 \ * 、/、n、N、s、Sand更多。

## 不做什么

插入模式是常规的 Sublime Text 编辑，具有通常的 Sublime Text 键绑定: vi插入模式键绑定不被模拟。
除了通过命令选项板工作的: 魔杖: e之外，没有实现Ex命令。


## Under the Hood

复古模式完全通过键绑定和插件API实现 -- 请随意浏览复古包，看看它是如何组合在一起的。例如，如果要绑定 jj 到退出插入模式，可以添加此键绑定:

~~~
{
    "keys": ["j", "j"],
    "command": "exit_insert_mode",
    "context":
    [
        { "key": "setting.command_mode", "operand": false },
        { "key": "setting.is_widget", "operand": false }
    ]
}

~~~

## Mac

默认情况下，在Mac上，按住一个键不会重复，而是会显示一个弹出菜单来在字符变化之间进行选择。这在命令模式下不能很好地工作，因此您可能需要禁用它。这可以通过在 Terminal.app 中执行以下操作来完成:

~~~
defaults write com.sublimetext.2 ApplePressAndHoldEnabled -bool false
~~~

## Ctrl Keys

Vintage 支持以下**Ctrl**key 绑定:

*   **Ctrl***+***\[**: Escape
*   **Ctrl***+***R**: Redo
*   **Ctrl***+***Y**: Scroll down one line
*   **Ctrl***+***E**: Scroll up one line
*   **Ctrl***+***F**: Page Down
*   **Ctrl***+***B**: Page Up

然而，由于和Sublime Text 快捷键绑定冲突，它们默认在Windows和Linux上禁用的。它们可以通过 vintage\_ctrl\_key 设置:

~~~
"vintage_ctrl_keys": true
~~~
启用。

## Ex 模式

看一下[VintageEx](https://github.com/SublimeText/VintageEx)来了解 VintageEx 的ex模式。
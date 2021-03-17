# 缩进设置

缩进设置确定制表位（Tab)的大小，并控制制表（Tab)键是否应插入制表符或空格。除自动检测外，还可以全局，按文件类型或按文件进行自定义。

## 设置

tab_size整数。一个Tab等于的空格的数量translate_tabs_to_spaces布尔值，如果为true，则在按下Tab键时，将会插入tab_size数量的空格detect_indentation布尔值，如果为true（默认值），则在加载文件时将自动计算 tab_size和translate_tabs_to_spacesuse_tab_stops布尔值，如果translate_tabs_to_spaces为true，则use_tab_stops将使tab和退格插入/删除到下一个制表位

### 设置文件

按以下顺序查询设置文件：

1.  Packages/Default/Preferences.sublime-settings
2.  Packages/Default/Preferences ().sublime-settings
3.  **Packages/User/Preferences.sublime-settings**
4.  Packages//.sublime-settings
5.  **Packages/User/.sublime-settings**

通常，您应将设置放在Packages/User/Preferences.sublime-settings中。如果要指定某种文件类型的设置（例如，Python），则应将它们放在Packages/User/Python.sublime-settings中。

### 单语法设置

可以基于每个语法指定设置。您可以使用Preferences![▶](images/right.svg)Settings – Syntax Specific菜单编辑当前语法的设置。

## 缩进检测

当一个文件被加载时，Sublime Text会根据tab\_size和translate\_tabs\_to\_spaces配置对文件进行格式化，在格式化会告知用户。可以使用detect\_indentation设置来禁用此功能。

缩进检测可以通过View![▶](images/right.svg)Indentation![▶](images/right.svg)Guess Settings From Buffer菜单手动运行，该菜单运行detect\_indentation命令。

## Tabs和空格互转

View![▶](images/right.svg)Indentation菜单包含用于在制表符和空格之间转换当前文件中的前导空格的命令。这些菜单项运行expand\_tabs和unexpand\_tabs命令。

## 自动缩进

当您按Enter键时，自动缩进会猜出要在每行上插入的前导空格的数量。它由以下设置控制：

auto_indent布尔值，默认启用。启用自动缩进smart_indent布尔值，默认启用。使自动缩进更聪明一些，例如，通过在C语句中的if语句后缩进下一行。trim_automatic_white_space布尔值，默认启用。在将插入符号移离线条时，修剪auto_indent添加的空白区域。indent_to_bracket布尔值，默认情况下禁用。在缩进时将空格添加到第一个开括号。在缩进时使用如下：use_indent_to_bracket(to_indent,
                      like_this);
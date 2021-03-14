# [SUBLIME TEXT中文文档之](index)免打扰模式（Distraction Free Mode）

免打扰模式全屏显示您的文件，显示器中央只显示文字。所有UI都是隐藏的，但是可以访问。免打扰模式可以通过View![▶](http://www.sublimetext.cn/images/right.svg)Enter Distraction Free Mode菜单项输入。

在免打扰模式中，将隐藏所有UI（侧边栏，小地图，状态栏等）。您可以通过View菜单有选择地启用部分UI - 下次进入免打扰模式时，您的设置将被记住。

## 定制

在免打扰模式中将应用某些设置。默认设置（位于Packages/Default/Distraction Free.sublime-settings)中）是：

~~~js
{
    "line_numbers": false,
    "gutter": false,
    "draw_centered": true,
    "wrap_width": 80,
    "word_wrap": true,
    "scroll_past_end": true
}

~~~

您可以通过编辑Packages/User/Distraction Free.sublime-settings来自定义这些，可以通过Preferences![▶](http://www.sublimetext.cn/images/right.svg)Settings – Distraction Free菜单访问。

值得注意的是上面的wrap\_width设置。值80导致包装发生在80个字符。您可能希望将其设置为更高的值，或者通过将其设置为0来换行到窗口宽度。
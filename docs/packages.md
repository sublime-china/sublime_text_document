# 包管理

包是Sublime Text使用的资源文件的集合：插件，语法突出显示定义，菜单，代码片段等。Sublime Text附带了几个包，可以使用更多用户创建的包。

包存储在.sublime-package文件中，这些文件是具有不同扩展名的zip文件。软件包也可以解压缩存储在目录中，或两者混合：软件包目录中的任何松散文件都将覆盖存储在.sublime-package文件中的文件。

## 位置

压缩包可以存储在：

*    /封装/
*    /已安装的包/

松散的包裹可能存储在：

*    /封装/

例如，Python包存储在 /Packages/Python.sublime-package中， / Packages / Python /目录中的任何文件都将覆盖存储在.sublime-package文件中的文件。

通常， / Packages /适用于Sublime Text附带的软件包， / Installed Packages /适用于用户安装的软件包。

## 特别套餐

有两个特殊包：默认和用户。始终首先订购默认值，并始终最后订购用户。在程序包之间合并文件时，程序包顺序生效，例如Main.sublime-menu。任何包都可能包含一个名为Main.sublime-menu的文件，但是这不会覆盖主菜单，而是根据包的顺序合并文件。

默认和用户以外的包按字母顺序排序。

## 创建一个新包

要创建新包，只需在 / Installed Packages /下创建一个新目录。您可以从“首选项![▶](images/right.svg)浏览包”菜单访问此目录。

## 从压缩包中覆盖文件

要覆盖现有包中的文件，只需在Packages /  /目录下创建一个具有相同名称的文件。

例如，要覆盖Sublime Text附带的Python.sublime-package包中的文件function.sublime-snippet，在 / Packages /目录下创建一个名为Python的目录，并将function.sublime-snippet文件放在那里。
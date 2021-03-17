# 恢复初始状态

通过删除数据文件夹，可以将Sublime Text还原为恢复初始状态。根据您的操作系统，此文件夹位于：

*   OS X：〜/ Library / Application Support / Sublime Text 3
*   Windows：％APPDATA％\\ Sublime Text 3
*   Linux：〜/ .config / sublime-text-3

要恢复到恢复初始状态，您可以：

1.  退出Sublime文本
2.  将数据文件夹移动到备份位置
3.  启动Sublime文本

重新启动时，将创建一个新的数据文件夹，就像您第一次运行Sublime Text一样。请记住，这也会删除所有设置和包。数据文件夹的备份副本可用于检索无法重最初的安装的配置或自定义程序包。

### OS X.

在OS X上，默认情况下隐藏〜/ Library文件夹。要在那里导航，请在Finder中选择Go![▶](images/right.svg)Go to Folder菜单项，然后输入〜/ Library。

### 视窗

在Windows中，缓存文件存储在单独的位置％LOCALAPPDATA％\\ Sublime Text 3中，以提高漫游配置文件的性能。
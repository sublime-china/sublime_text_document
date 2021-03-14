# 插件移植指南

Sublime Text 3包含与Sublime Text 2在插件方面的一些重要区别，并且大多数插件至少需要少量移植才能工作。变化是：

*   Python 3.3
*   进程外插件
*   异步事件
*   限制`begin_edit()`和`end_edit()`
*   压缩包
*   导入模块

## Python 3.3

Sublime Text 3使用Python 3.3，而Sublime Text 2使用Python 2.6。此外，在OS X上，不再使用Python的系统构建，而是将Sublime Text与其自己的版本捆绑在一起。Windows和Linux也捆绑了他们自己的版本，就像以前一样。

## 进程外插件

插件现在在一个单独的进程中运行`plugin_host`。从插件作者的角度来看，应该没有可观察到的差异，除了插件主机中的崩溃将不再降低主应用程序。

## 异步事件

在Sublime Text 2中，只有该`set_timeout()`方法是线程安全的。在Sublime Text 3中，每个API方法都是线程安全的。此外，现在有异步事件处理程序，可以更轻松地编写非阻塞代码：

*   `on_modified_async()`
*   `on_selection_modified_async()`
*   `on_pre_save_async()`
*   `on_post_save_async()`
*   `on_activated_async()`
*   `on_deactivated_async()`
*   `on_new_async()`
*   `on_load_async()`
*   `on_clone_async()`
*   `set_timeout_async()`

编写线程代码时，请记住，当函数运行时，缓冲区将在您下面发生变化。

## 限制`begin_edit()`和`end_edit()`

`begin_end()`并且`end_edit()`不再可以直接访问，除非在某些特殊情况下。获取[Edit](api_reference#sublime.Edit)对象的有效实例的唯一方法是将代码放在[TextCommand](porting_guide#sublime_plugin.TextCommand)子类中。通常，大多数代码都可以通过将代码放在[TextCommand](porting_guide#sublime_plugin.TextCommand)之间`begin_edit()`和[之后](porting_guide#sublime_plugin.TextCommand)，然后运行命令来重构。`end_edit()`[](porting_guide#sublime_plugin.TextCommand)`run_command()`

这种方法消除了悬空[Edit](api_reference#sublime.Edit)对象的问题，并确保repeat命令和宏可以正常工作。

## 压缩包

Sublime Text 3中的包可以直接从.sublime-package（即重命名的.zip文件）文件运行，而Sublime Text 2则在运行之前解压缩它们。

虽然在大多数更改中这应该没有区别，但是如果要访问包中的文件，请务必记住这一点。

## 导入模块

在Sublime Text 3中导入其他插件更简单，更健壮，可以使用常规import语句完成，例如，`import Default.comment`将导入Packages / Default / Comment.py。

## 启动时受限制的API使用情况

由于`plugin_host`异步加载，在导入时无法使用API​​。这意味着模块中的所有顶级语句都不能调用[sublime](api_reference#sublime)模块中的任何函数。在启动期间，API处于休眠状态，并将静默忽略所做的任何请求。
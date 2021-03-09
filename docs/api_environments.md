# 

[DOCUMENTATION](index)[TOC](api_environments#toc)[TOP](api_environments#)

API 环境变量

Version:  
[Dev](api_environments#ver-dev)[3.2](api_environments#ver-3.2)[3.1](api_environments#ver-3.1)[3.0](api_environments#ver-3.0)

 Sublime Text 中的插件是由 在[package](包)根下的python文件。以下文档描述了运行插件的Python环境。

*   [Overview](api_environments#overview)
*   [Python Version](api_environments#python_version)
    *   [Selecting the Python Version](api_environments#selecting_python_version)4050
*   [Modules](api_environments#modules)
*   [Pre-Installed Packages](api_environments#preinstalled_packages)4050

## Overview

Sublime Text在主编辑器UI的单独进程中运行插件。此进程运行名为 plugin\_host 的可执行文件。

在单独的进程中运行插件可确保整个编辑器不会因插件编写不当而崩溃。如果插件确实导致 plugin\_host 崩溃，用户仍然可以在重新启动Sublime Text之前保存他们的工作。

所有插件都在单个 plugin\_host 进程中运行，并共享单个Python环境。每个插件都作为以包命名的模块的子模块加载。例如，在MyPackage/my\_plugin.py 中的插件将作为Python模块 `MyPackage.my_plugin`加载。

plugin_host 进程包含Python编程语言的嵌入式版本，并暴露 [API](api_reference)给插件。即使最终用户在其计算机上安装了Python，该文件始终使用其自己的嵌入式版本的Python。

## Python Version

默认情况下，所有插件均使用Python 3.3.6运行。* Sublime Text的Python 3.3.6版本包括从Python 3.4返回的一些补丁，用于修复64位版本的Windows上unicode路径和 `ctypes`模块崩溃的问题。*

从内部版本4050开始，插件也可以使用Python 3.8运行。Python 3.8对语言进行了许多改进，具有更好的性能，并从Python软件基金会获得了持续的支持和错误修复。4050

### 选择 PYTHON 版本4050

为了提供向下兼容性，Sublime Text 4050将继续在 Python 3.3 上运行所有插件。

任何希望使用Python 3.8的包都必须在包的根目录中创建一个名为.python-version的文件。此文件应包含文本 `3.3` 或 `3.8` 以选择要使用的Python版本。* 如果文件名为。python-versionis不存在，或者它包含除 `3.8` 以外的任何值，则将使用Python 3.3。*

包中的所有插件都将使用相同版本的Python。项目完结，系统自动填充内容包含在旧版本的Sublime Text中加载的 `3.8` 的python-versionfile将尝试使用Python 3.3运行插件。*

## 模块

处于plugin\_host 的 Python environment 包含了所有的[Python 标准库](https://docs.python.org/3.3/library/), 除了:

*   audioop
*   cryptLINUX
*   curses
*   cProfileLINUX
*   fpectl
*   readline

*   lzma
*   msilib
*   nis
*   ossaudiodev
*   resource
*   spwd

*   syslog
*   test
*   tkinter
*   turtle
*   wave

## 预装的包4050

以下包在Python 3.3 和 3.8 环境变量里都预装了：

*   [certifi](https://pypi.org/project/certifi/): urllib使用的一组SSL root 证书
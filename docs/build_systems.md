# 构建系统

Sublime Text提供了构建系统，允许用户运行外部程序。构建系统的常见用途示例包括：编译，转换，linting和执行测试。

构建系统通过JSON指定并保存在扩展名为.sublime-build的文件中。可以通过Tools![▶](http://www.sublimetext.cn/images/right.svg)Build System![▶](http://www.sublimetext.cn/images/right.svg)New Build System ...菜单项或Build: New Build System命令选项板条目创建新的构建系统。

构建系统有各种方式可以将自己与文件和项目相关联。使用此信息，Sublime Text可以智能地仅向用户显示可行的构建系统。内置`exec`目标提供了快速启动和运行的常用选项。对于更复杂的需求，构建系统可以定位用Python编写的自定义Sublime Text命令。

*   [基本例子](build_systems#basic_example)
*   [用法](build_systems#usage)
*   [选项](build_systems#options)
*   [`exec`目标选项](build_systems#exec_options)
*   [自定义选项](build_systems#custom_options)
*   [变量](build_systems#variables)
*   [高级示例](build_systems#advanced_example)

## 基本例子

以下是构建系统的基本示例。此构建系统将执行当前打开的Python文件。

~~~js
{
    "cmd": ["python", "$file"],
    "selector": "source.python",
    "file_regex": "^\\s*File \"(...*?)\", line ([0-9]*)"
}
~~~

的[用法](build_systems#usage)和[选项](build_systems#options)部分将讨论如何使用和定制构建系统。

## 用法

构建系统包括以下功能：

*   根据文件类型自动选择构建系统
*   记住上次使用的构建系统
*   构建系统结果的导航
*   能够取消构建

### 运行构建

可以通过以下方法之一运行构建：

| 键盘 | 菜单 |
| --- | --- |
| 在Windows / Linux的 | 苹果电脑 | 所有 | 工具![▶](http://www.sublimetext.cn/images/right.svg)构建 |
| **Ctrl***+***B.** | **⌘***+***B.** | **F7** |

输出将显示在Sublime Text窗口底部显示的输出面板中。

### 选择构建系统

默认情况下，Sublime Text使用自动选择构建系统。当用户调用构建时，将使用当前文件的语法和文件名来选择适当的构建系统。

如果多个构建系统与当前文件类型匹配，系统将提示用户选择他们希望使用的构建系统。一旦选择了构建系统，Sublime Text将记住它，直到用户更改其选择。

要手动选择构建系统，请使用：

| 菜单 |
| --- |
| 工具![▶](http://www.sublimetext.cn/images/right.svg)构建系统 |

要在可行选项中更改构建系统，请使用以下方法之一：

| 键盘 | 菜单 | 命令调色板 |
| --- | --- | --- |
| 在Windows / Linux的 | 苹果电脑 | 工具![▶](http://www.sublimetext.cn/images/right.svg)构建...... | Build With: |
| **Ctrl***+***Shift***+***B.** | **⇧***+***⌘***+***B.** |

### 导航结果

构建系统允许导航构建输出中指定的文件。通常，这用于跳转到错误的位置。可通过以下方式执行导航：

| 命令 | 键盘 | 菜单 |
| --- | --- | --- |
| 下一个结果 | **F4** | 工具![▶](http://www.sublimetext.cn/images/right.svg)构建结果![▶](http://www.sublimetext.cn/images/right.svg)下一步结果 |
| 上一个结果 | **Shift***+***F4** | 工具![▶](http://www.sublimetext.cn/images/right.svg)构建结果![▶](http://www.sublimetext.cn/images/right.svg)上一个结果 |

### 取消构建

可以通过以下方式取消进程内构建：

| 键盘 | 菜单 | 命令调色板 |
| --- | --- | --- |
| 在Windows / Linux的 | 苹果电脑 | 工具![▶](http://www.sublimetext.cn/images/right.svg)取消构建 | Build: Cancel |
| **Ctrl***+***Break** | **Ctrl***+***C.** |

## 选项

所有构建系统都可以在.sublime-build文件中使用以下顶级键。

选择

应为此构建系统启用的语法的基本作用域名称。  
*示例：`"source.python"`。*

FILE\_PATTERNS

应为其启用构建系统的文件名模式列表。  
*示例：`["*.py"]`。*

密钥文件

文件名列表（如果存在于其中一个打开的文件夹中）将导致构建系统启用。  
*示例：`["Makefile"]`。*

变种

将从顶级构建系统继承选项的辅助构建系统列表。每个变体都需要指定一个`name`键，并可以覆盖或添加顶级构建系统的选项。  
*例：  

~~~
[
    {
        "name": "Debug Symbols",
        "cmd": ["my_command", "-D", "$file"]
    }
]

~~~* 

取消

字符串命令名称或字符串选项列表。如果指定了字符串，则指定的命令将用于取消构建。如果是字符串列表，`target`则会调用主要字符串，并添加这些选项。这只需要在使用自定义时指定`target`。  
*示例：`"cancel_my_build"`或`{"kill": true}`。*

目标

调用构建系统时运行的命令。exec的默认值允许使用[exec Target Options中](build_systems#exec_options)指定的其他[选项](build_systems#exec_options)。如果`exec`指定的值不是指定的，则[exec Target Options中的](build_systems#exec_options)任何[选项](build_systems#exec_options)都不会执行任何操作。有关完整示例，请参阅[高级](build_systems#advanced_example)示例。  
*例：`"my_build"`*

视窗

在Windows计算机上执行构建系统时使用的选项对象。  
*例：  

~~~
{
    "cmd": ["my_command.exe", "/D", "$file"]
}

~~~* 

OSX

在Mac计算机上执行构建系统时使用的选项对象。  
*例：  

~~~
{
    "cmd": ["/Applications/MyProgram.app/Contents/MacOS/my_command", "-d", "$file"]
}

~~~* 

Linux的

在Linux机器上执行构建系统时使用的选项对象。  
*例：  

~~~
{
    "cmd": ["/usr/local/bin/my_command", "-d", "$file"]
}

~~~* 

## `exec`目标选项

默认`target`的`exec`使用受到了广大构建系统。它提供以下选项来控制要执行的程序以及如何显示结果。

CMD

指定要运行的可执行文件的字符串列表，以及传递给它的任何参数。不支持Shell构造，例如管道和重定向 - 请参阅[shell\_cmd](build_systems#exec_option-shell_cmd)。可以使用[变量](build_systems#variables)。  
*例：`["my_command", "-d", "$file"]`*

shell\_cmd

一个字符串，指定要执行的shell命令。与[cmd](build_systems#exec_option-cmd)选项不同 ，它允许管道和重定向。将`bash`在Mac和Linux机器以及`cmd.exe`Windows上使用。可以使用[变量](build_systems#variables)。  
*例：`"my_command \"$file\" | other_command"`*

working\_dir

一个字符串，指定要在其中执行[cmd](build_systems#exec_option-cmd)或[shell\_cmd](build_systems#exec_option-shell_cmd)的目录 。可以使用[变量](build_systems#variables)。  
*例：`"$file_path"`*

file\_regex

包含要在构建输出上运行以匹配文件信息的正则表达式的字符串。匹配的文件信息用于启用结果导航。正则表达式应该捕获2,3或4组。  
  
捕获组应该是：

1.  文件名
2.  电话号码
3.  列号
4.  信息

*例：`"^\s*(\\S[^:]*)\\((\\d+):(\\d+)\\): ([^\\n]+)"`*

line\_regex

包含要在构建输出上运行以匹配行信息的正则表达式的字符串。匹配的文件信息用于启用结果导航。正则表达式应该捕获1,2或3组。  
  
小组应该抓住：

1.  电话号码
2.  列号
3.  错误信息

只有当某些结果严格包含行号，行号和列号或带有消息的行号和列号时，才需要使用此正则表达式。进行这样的匹配时， 将使用[file\_regex](build_systems#exec_option-file_regex)选项向后搜索以查找适当的文件名。  
  
*例：`"^\s*line (\\d+) col (\\d+): ([^\\n]+)"`*

编码

一个字符串，指定构建系统输出的编码。使用[Python编解码器名称](https://docs.python.org/3.3/library/codecs#id3)。默认为`"utf-8"`。  
*例：`"iso-8859-1"`*

ENV

包含运行[cmd](build_systems#exec_option-cmd)或[shell\_cmd](build_systems#exec_option-shell_cmd)时要使用的环境变量值的对象。  
*例：

~~~
{
    “PYTHONIOENCODING”：“utf-8”
}

~~~* 

安静

一个布尔值，可减少有关构建系统调用的输出量。  
*例：`true`*

word\_wrap

一个布尔值，用于打开构建系统输出面板中的自动换行。  
*例：`true`*

句法

一个字符串，指定用于突出显示构建系统输出面板的语法文件。  
*例：`"Packages/JavaScript/JSON.sublime-syntax"`*

## 自定义选项

当实现命令以充当构建系统目标时，命令的关键字参数可通过.sublime-build文件中的选项获得。但是，某些参数名称不起作用，因为它们与内置的构建系统功能冲突。

以下名称不会作为参数传递给命令。这也适用于其他情况，如在指定的选项`cancel`，`linux`，`osx`和`windows`选项。

*   `cancel`
*   `file_patterns`
*   `keyfile`
*   `keyfiles`
*   `linux`
*   `osx`
*   `save_untitled_files`
*   `selector`
*   `target`
*   `variants`
*   `windows`

## 变量

以下变量将在指定的任何字符串内膨胀`"cmd"`，`"shell_cmd"`或`"working_dir"`选项。

如果`$`需要在其中一个选项中指定文字，则必须使用a进行转义`\`。由于JSON也使用反斜杠进行转义，`$`因此需要将其写为`\\$`。

请注意，任何替换都将发生`target`。如果使用自定义目标，则可以通过使用`sublime.expand_variables()`结果来 实现其他选项的变量扩展`self.window.extract_variables()`。

$包

Packages /文件夹 的路径

$平台

包含平台Sublime Text的字符串正在：`windows`，`osx`或运行`linux`。

$文件

活动视图中文件的完整路径，包括文件夹。

$ FILE\_PATH

包含活动视图中文件的文件夹的路径。

$ FILE\_NAME

活动视图中文件的文件名（无文件夹路径）。

$ file\_base\_name

活动视图中文件的文件名（不包括扩展名）。

$ FILE\_EXTENSION

活动视图中文件的文件名扩展名。

$文件夹

第一个文件夹的完整路径在侧栏中打开。

$项目

当前项目文件的完整路径。

$ project\_path

包含当前项目文件的文件夹的路径。

$ PROJECT\_NAME

当前项目文件的文件名（sans文件夹路径）。

$ project\_base\_name

当前项目文件的文件名，不包括扩展名。

$ project\_extension

当前项目文件的扩展名。

## 高级示例

以下示例显示了一个自定义`target`命令，可以取消构建并导航结果。

一个`target`用于构建系统应该是一个[`sublime.WindowCommand`](api_reference#sublime_plugin.WindowCommand)。这将提供实例变量，`self.window`以允许与当前项目，窗口和活动视图进行交互。

*请注意，以下示例在其实现中有些简单，并且它不会处理许多常见的边缘情况。*

以下Python可以保存到名为Package / User / my\_example\_build.py的文件中 ：

~~~python
import sublime
import sublime_plugin

import subprocess
import threading
import os


class MyExampleBuildCommand(sublime_plugin.WindowCommand):

    encoding = 'utf-8'
    killed = False
    proc = None
    panel = None
    panel_lock = threading.Lock()

    def is_enabled(self, lint=False, integration=False, kill=False):
        # The Cancel build option should only be available
        # when the process is still running
        if kill:
            return self.proc is not None and self.proc.poll() is None
        return True

    def run(self, lint=False, integration=False, kill=False):
        if kill:
            if self.proc:
                self.killed = True
                self.proc.terminate()
            return

        vars = self.window.extract_variables()
        working_dir = vars['file_path']

        # A lock is used to ensure only one thread is
        # touching the output panel at a time
        with self.panel_lock:
            # Creating the panel implicitly clears any previous contents
            self.panel = self.window.create_output_panel('exec')

            # Enable result navigation. The result_file_regex does
            # the primary matching, but result_line_regex is used
            # when build output includes some entries that only
            # contain line/column info beneath a previous line
            # listing the file info. The result_base_dir sets the
            # path to resolve relative file names against.
            settings = self.panel.settings()
            settings.set(
                'result_file_regex',
                r'^File "([^"]+)" line (\d+) col (\d+)'
            )
            settings.set(
                'result_line_regex',
                r'^\s+line (\d+) col (\d+)'
            )
            settings.set('result_base_dir', working_dir)

            self.window.run_command('show_panel', {'panel': 'output.exec'})

        if self.proc is not None:
            self.proc.terminate()
            self.proc = None

        args = ['my_cli']
        if lint:
            args.append('-l')
        elif integration:
            args.append('-i')
        args.append(vars['file_name'])
        self.proc = subprocess.Popen(
            args,
            stdout=subprocess.PIPE,
            stderr=subprocess.STDOUT,
            cwd=working_dir
        )
        self.killed = False

        threading.Thread(
            target=self.read_handle,
            args=(self.proc.stdout,)
        ).start()

    def read_handle(self, handle):
        chunk_size = 2 ** 13
        out = b''
        while True:
            try:
                data = os.read(handle.fileno(), chunk_size)
                # If exactly the requested number of bytes was
                # read, there may be more data, and the current
                # data may contain part of a multibyte char
                out += data
                if len(data) == chunk_size:
                    continue
                if data == b'' and out == b'':
                    raise IOError('EOF')
                # We pass out to a function to ensure the
                # timeout gets the value of out right now,
                # rather than a future (mutated) version
                self.queue_write(out.decode(self.encoding))
                if data == b'':
                    raise IOError('EOF')
                out = b''
            except (UnicodeDecodeError) as e:
                msg = 'Error decoding output using %s - %s'
                self.queue_write(msg  % (self.encoding, str(e)))
                break
            except (IOError):
                if self.killed:
                    msg = 'Cancelled'
                else:
                    msg = 'Finished'
                self.queue_write('\n[%s]' % msg)
                break

    def queue_write(self, text):
        sublime.set_timeout(lambda: self.do_write(text), 1)

    def do_write(self, text):
        with self.panel_lock:
            self.panel.run_command('append', {'characters': text})

~~~

`MyExampleBuildCommand`可以将 自定义配置为构建系统，使用以下JSON保存到名为Packages / User / My Example Build.sublime-build的文件中：

~~~js
{
    "target": "my_example_build",
    "selector": "source.mylang",
    "cancel": {"kill": true},
    "variants": [
        {
            "name": "Lint",
            "lint": true
        },
        {
            "name": "Integration Tests",
            "integration": true
        }
    ]
}
~~~
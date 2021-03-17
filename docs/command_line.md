# 命令行

Sublime Text包含一个命令行工具 `subl`，用于处理命令行中的文件。这可以用来以崇高的文本打开文件和项目，也可以用作unix工具的 `EDITOR`，例如git和subversion。

## Setup

一些操作系统和安装方法将需要进行配置更改，以使 `subl` 在 `PATH` 上可用。

### WINDOWS

在Windows上，命令行帮助程序为 `subl.exe`。要在 *命令提示符* 或 *Powershell* 中使用此命令，需要将Sublime Text安装文件夹添加到 `Path` 环境变量中:

#### WINDOWS 10

*Show instructions for:[Windows 8](command_line#windows-8),[Windows 7](command_line#windows-7)*

*   打开*Start Menu*and type environ
*   Select the item*Edit the system environment variables*
*   Click the button*Environment Variables*at the bottom of the*System Properties*dialog
*   Select, or create, the`Path`environment variable in the appropriate section:
    *   For the current user, select`Path`in the*User variables for {username}*section
    *   For all users, select`Path`in the*System variables*section
*   Click the*New*button and add an entry with the Sublime Text installation directory
    *   64bit installs are typically in C:\\Program Files\\Sublime Text\\
    *   32bit installs on a 64bit version of Windows will be inC:\\Program Files (x86)\\Sublime Text\\
    *   32bit installs on a 32bit version of Windows will be inC:\\Program Files\\Sublime Text\\

### MAC

要使用subl，需要将Sublime Text bin文件夹添加到path中。对于Sublime Text的典型安装，它将位于/apps/Sublime Text.app/content/SharedSupport/bin。


#### BASH

如果使用Bash (macOS 10.15之前的默认值)，则以下命令会将bin文件夹添加到 `PATH` 环境变量中:


~~~
echo 'export PATH="/Applications/Sublime Text.app/Contents/SharedSupport/bin:$PATH"' >> ~/.bash_profile

~~~

#### ZSH

如果使用Zsh (默认从macOS 10.15开始)，则以下命令会将bin文件夹添加到 `PATH` 环境变量中:
~~~
echo 'export PATH="/Applications/Sublime Text.app/Contents/SharedSupport/bin:$PATH"' >> ~/.zprofile

~~~

### LINUX

如果 Sublime Text 安装通过一个[Linux Package Manager Repositories](linux_repositories) 或一个包, a subl 软链将自动安装到 /usr/bin/ 目录。

如果从tarball安装，sublime\_text可执行文件应符号可以手动链接到subl，命令如下:

~~~
sudo ln -s /opt/sublime_text/sublime_text /usr/local/bin/subl

~~~

symlink命令的确切详细信息将取决于安装位置。大多数默认`PATH`环境变量值应包含/usr/local/bin，因此不需要其他命令。

## 用法
查看可用参数 运行subl --help。*每个操作系统的可用标志都会有所不同-以下示例来自Mac。*

~~~
Sublime Text build 3211

Usage: %s [arguments] [files]         Edit the given files
   or: %s [arguments] [directories]   Open the given directories
   or: %s [arguments] -               Edit stdin

Arguments:
  --project <project>: Load the given project
  --command <command>: Run the given command
  -n or --new-window:  Open a new window
  -a or --add:         Add folders to the current window
  -w or --wait:        Wait for the files to be closed before returning
  -b or --background:  Don't activate the application

  -s or --stay:        Keep the application activated after closing the file
  -h or --help:        Show help (this message) and exit
  -v or --version:     Show version and exit

--wait is implied if reading from stdin. Use --stay to not switch back
to the terminal when a file is closed (only relevant if waiting for a file).

Filenames may be given a :line or :line:column suffix to open at a specific location.

~~~

### 配置为编辑器

要使用Sublime Text作为提示输入的许多命令的编辑器，请设置您的`EDITOR`环境变量：

~~~
export EDITOR='subl -w'

~~~

指定`-w`将导致`subl`命令在文件关闭之前不退出。
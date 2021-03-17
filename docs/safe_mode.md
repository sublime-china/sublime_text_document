# 安全模式

Added in:4.0

Sublime Text包含一个名为 *安全模式* 的执行模式，该模式禁用所有自定义。这可以在尝试诊断问题时使用，因为它允许在原始状态下轻松运行编辑器，同时保持所有首选项和第三方包不变。



*从版本4.0开始提供安全模式。要在早期版本中实现类似的结果，请参阅[Reverting to a Freshly Installed State](revert).*

*   [Behavior](safe_mode#behavior)
*   [Starting](safe_mode#starting)

## Behavior

在安全模式下启动Sublime Text时，应用程序使用备用数据目录。数据目录是存储所有首选项、自定义键绑定、会话数据和第三方包的位置。为确保安全模式不受以前会话的影响，启动时会完全删除安全模式数据目录。*请勿在安全模式数据目录中存储任何重要文件或自定义设置。*

只有在应用程序已完全关闭的情况下才能进入安全模式。如果任何windows保持打开状态 (或在Mac上应用程序本身)，则安全模式将不起作用。

*   **Windows:**%AppData%\\Sublime Text (Safe Mode)\\
*   **Mac:**~/Library/Application Support/Sublime Text (Safe Mode)/
*   **Linux:**~/.config/sublime-text-safe-mode/

为了防止每次Sublime Text启动时都必须重新安装任何许可证密钥，安全模式将从正常数据目录复制许可证密钥。

## Starting

Sublime Text可以启动为安全模式下通过[命令行界面](command_line):

~~~
subl --safe-mode

~~~

此外，在Windows和Mac上，在启动应用程序时按住修饰键将在安全模式下打开它:

*   **Windows:****Shift***+***Alt**
*   **Mac:****Option**
# 

[DOCUMENTATION](index)[TOC](indexing#toc)[TOP](indexing#)

Indexing

Version:  
[Dev](indexing#ver-dev)[3.2](indexing#ver-3.2)[3.1](indexing#ver-3.1)[3.0](indexing#ver-3.0)

Sublime Text包括一个索引引擎，该引擎扫描窗口/项目中的所有文件和文件夹，并使用该信息来提供跳转到定义的功能，并提供上下文感知的完成4.0。

*   [Features](indexing#features)
    *   [Goto Definition](indexing#goto_definition)
    *   [Context-Aware Completions](indexing#context-aware_completions)4.0
*   [Status](indexing#status)
*   [Settings](indexing#settings)

## Features

### GOTO DEFINITION

扫描项目中的文件时，索引引擎会记录每个符号及其位置的列表。每种语法都能够定义分类为符号的内容，但通常对函数、方法、类和其他数据类型进行索引。除了记录定义的位置之外，索引器还记录引用-已知符号的调用或调用。

符号索引可以通过以下方式访问:


*   将鼠标悬停在一个单词上以显示*Goto Definition Popup*
*   调用 *在项目中转到符号* 以模糊搜索符号
    *   **Windows/Linux:****Ctrl***+***Shift***+***R**
    *   **Mac:****⌘***+***Shift***+***R**
*   对插入符号下的单词执行 *跳转定义*
    *   **All OSes:****F12**
*   对插入符号下的单词执行 *跳转引用 *
    *   **All OSes:****Shift***+***F12**

所有 *Goto* 命令也可以通过Goto菜单调用。

### CONTEXT-AWARE COMPLETIONS 4.0

除了提供有关符号的信息外，索引还用于提供上下文感知的完成。索引器列出项目中存在的所有单词，以及有关单词序列和任何尾随标点的信息。

当显示完成时，查询索引以提供智能建议。如果没有索引，Sublime Text 将只建议当前文件中的匹配单词。使用索引，它提供所有文件的完成，使用前面的单词来帮助建议更好的匹配，并在适当的时候建议尾随标点符号。

## Status

索引引擎的当前状态和活动可以通过 帮助![▶](images/right.svg)索引状态菜单项。 这将显示一个窗口，其中包含索引消息的当前状态、进度条和日志。

当索引引擎处于活动状态时，状态栏将包含带有百分比的文本标签。 该百分比表示索引是活动的，以及它在流程中的进展程度。单击百分比将打开 *索引状态* 窗口。

## Settings

索引引擎使用低优先级后台进程来加载和分析项目中的文件。根据机器和可用资源，可能需要修改配置以确保进程不会干扰机器的其他使用。

index\_files boolean

是否索引引擎已启用

Default:`true`

index\_workers integer

要使用的后台进程的数量。值 `0` 会导致Sublime Text根据CPU内核的数量自动选择响应的数量。

Default:`0`

index\_exclude\_gitignore 4.0 boolean

是否文件通过忽略。gitignore被排除在索引之外

Default:`true`

index\_skip\_unknown\_extensions 4.0 boolean

是否扩展名未知的文件被排除在索引之外

Default:`true`

index\_exclude\_patterns array of strings

[文件模式](file_patterns)用于从索引中排除文件

Default:`["*.log"]`

show\_definitions boolean

将鼠标悬停在已编制索引的单词上时，是否出现 *跳转定义* 弹出窗口

Default:`true`

auto\_complete\_use\_index 4.0 boolean

是否完成应使用来自索引的信息

Default:`true`
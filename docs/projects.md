# 项目

Sublime Text中的项目由两个文件组成：

*   .sublime-project文件，其中包含项目定义。
*   .sublime-workspace文件，其中包含用户特定数据，例如打开文件和对每个文件的修改。

一般情况下，.sublime-project文件将被加入版本控制，而.sublime-workspace文件则不会。

## 项目格式

.sublime-project文件是JSON格式，支持三个一级选项：

*   文件夹（folders）：包含文件夹
*   设置（settings）：文件设置
*   构建系统（build\_systems）：用于项目特定的构建系统

一个例子：

~~~js
{
    "folders":
    [
        {
            "path": "src",
            "folder_exclude_patterns": ["backup"],
            "follow_symlinks": true
        },
        {
            "path": "docs",
            "name": "Documentation",
            "file_exclude_patterns": ["*.css"]
        }
    ],
    "settings":
    {
        "tab_size": 8
    },
    "build_systems":
    [
        {
            "name": "List",
            "shell_cmd": "ls -l"
        }
    ]
}

~~~

### 文件夹（FOLDERS）

每个文件夹必须具有path键，该键可以与项目目录相关，也可以是完全限定的路径。其他可选键包括：

*   name\- 用于代替侧栏中文件夹名称的字符串。
*   file\_include\_patterns\- 要包含在文件夹中的文件名的字符串列表。任何不符合这些模式的内容都将被排除在外。在file\_exclude\_patterns之前检查。
*   file\_exclude\_patterns\- 要从文件夹中排除的文件名的字符串列表。这将添加到同名的全局设置中。在file\_include\_patterns之后检查。
*   folder\_include\_patterns\- 要包含在文件夹中的子文件夹路径的字符串列表。任何不符合这些模式的内容都将被排除在外。在folder\_exclude\_patterns之前检查。
*   folder\_exclude\_patterns\- 要从文件夹中排除的子文件夹路径的字符串列表。这将添加到同名的全局设置中。在folder\_include\_patterns之后检查。
*   binary\_file\_patterns\- 要视为二进制文件的文件名字符串列表，因此在Goto Anything或在文件中查找。
*   index\_include\_patterns\- 文件夹中索引的完整文件路径的字符串列表。这将添加到同名的全局设置中。任何与这些模式不匹配的内容都将从索引中排除。在index\_exclude\_patterns之前检查。
*   index\_exclude\_patterns\- 文件夹中索引的文件完整路径的字符串列表。这将添加到同名的全局设置中。在index\_include\_patterns之后检查。
*   follow\_symlinks\- 如果在构建文件夹树时应遵循符号链接。

早期版本中的转换项目可能在文件夹下具有mount\_points条目。 如果您希望使用排除模式，则需要更改为上述格式。

### 设置（SETTINGS）

可以使用设置键在此处指定[设置](http://www.sublimetext.cn/docs/3/settings.html)，并覆盖常规用户设置。 但是，它们不会覆盖特定于语法的设置。

*请注意，只有[编辑器设置](http://www.sublimetext.cn/docs/3/settings.html#categories)类别中的设置可能由项目控制。*

### 构建系统（BUILD SYSTEMS）

build\_systems指定内联[构建系统（Build Systems）](http://docs.sublimetext.info/en/latest/file_processing/build_systems.html)的列表定义。 除常规构建系统设置外，还必须为每个设置指定name，通过Tools![▶](http://www.sublimetext.cn/images/right.svg)Build Systems菜单，查看构建系统列表。
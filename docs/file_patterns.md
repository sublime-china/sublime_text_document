# 文件模式

Sublime文本中的各种功能使用文件模式。这些模式用于匹配文件/目录名称和路径。它们在功能上与shell全局模式相似，但具有一些独特的行为。

## Basic Syntax

文件模式允许两个匹配运算符:

*   **`*`:**匹配零个或多个字符，除了`/`
*   **`?`:**完全匹配一个字符，除了`/`

*不支持来自Bash的字符类 [abc] 或globstar运算符 \*\*。*

### EXAMPLES

*   模式`abc`将匹配`abc`不匹配`abcd`
*   模式`a?c`将匹配`abc`不匹配`ac`
*   模式`a*c`将匹配`abc`,`ac`和`abdc`

## Path Rules

当模式中不存在 `/`* 时，仅将其与文件或目录名称进行比较，并且仅应用基本语法。当模式中包含 `/`* 时，它将行为更改为:

*   模式与整个文件或目录路径匹配
*   在 `*/` 前缀或 `/*` 后缀中，`*` 将匹配 `/` 字符
*   `*/` 如果模式不以a`/`or`*` 开头，则将隐式前缀
*   如果模式不以 `/`或`*`开头，`*`将是隐式后缀 <4.0
*   如果模式以/结尾，它将被视为目录模式，并将匹配具有该名称的目录和 4.0任何包含的文件或子目录
*   如果模式以单 `/` 开头，则将其作为绝对路径进行比较
*   如果pattern以 '//' 开头，则将其作为来自项目根的相对路径进行比较 4.0。

### EXAMPLES

*   模式及`mydir/one`将匹配`/parent/mydir/one`,`/mydir/one`和`/mydir/one/sub`
*   模式及`mydir/two`将匹配`/parent/mydir/two`和`/parent/mydir/two_sub`
*   模式及`/mydir/three`将匹配`/mydir/three` 但不匹配`/nested/mydir/three`
*   模式及`mydir/three/`将匹配`/parent/mydir/three/sub`但不匹配`/parent/mydir/three`
*   模式及`//mydir/five`将匹配`/project1/mydir/five`和`/project2/mydir/five`但不匹配`/project1/nested/mydir/five`4.0

## Uses

文件模式用于:

*   *Find in Files*面板*Where*input
*   各种设置
    *   folder\_exclude\_patterns:*[设置](settings)*和*[项目目录](projects#folders)*
    *   folder\_include\_patterns:*[项目目录](projects#folders)*
    *   file\_exclude\_patterns:*[设置](settings)*和*[项目目录](projects#folders)*
    *   file\_include\_patterns:*[项目目录](projects#folders)*
    *   binary\_file\_patterns:*[设置](settings)*和*[项目目录](projects#folders)*
    *   index\_exclude\_patterns:*[设置](settings)*和*[项目目录](projects#folders)*
    *   index\_include\_patterns:*[设置](settings)*和*[项目目录](projects#folders)*
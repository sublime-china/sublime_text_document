# 语法定义

Sublime Text可以使用.sublime-syntax和.tmLanguage文件进行语法高亮显示。本文档描述了.sublime-syntax文件。

## 概况

Sublime Syntax文件是带有标题的[YAML](http://yaml.org/)文件，后跟一个上下文列表。每个上下文都有一个模式列表，描述如何在该上下文中高亮显示文本，以及如何更改当前文本。

这是一个用于高亮显示C的语法文件的小示例。

~~~yaml
%YAML 1.2
---
name: C
file_extensions: [c, h]
scope: source.c

contexts:
  main:
    - match: \b(if|else|for|while)\b
      scope: keyword.control.c

~~~

语法定义的核心是将范围（例如，keyword.control.c）分配给文本的区域。颜色方案使用这些范围来高亮显示文本。

此语法文件包含一个上下文main，它匹配单词\[if，else，for，while\]，并为它们指定范围keyword.control.c。上下文名称mainspecial：每个语法都必须定义一个主上下文，因为它将在文件的开头使用。

match键是一个正则表达式，支持[Oniguruma regex engine](https://raw.githubusercontent.com/kkos/oniguruma/5.9.6/doc/RE)的特性。在上面的示例中，\\b用于确保只匹配单词边界，以确保elsewhere单词不被视为关键字。

请注意，由于YAML语法，.sublime-syntax文件中不允许使用制表符。

## 头部定义

标题区域中允许的键有：

*   **name**。这定义了菜单中语法显示的名称。它是可选的，如果不使用，将从文件名派生。
*   **file\_extensions**。字符串列表，定义此语法应使用的文件扩展名
*   **first\_line\_match**。在没有可识别扩展名的情况下打开文件时，将针对此正则表达式测试文件内容的第一行，以查看是否应该应用语法。
*   **scope**。分配给文件中所有文本的默认范围
*   **hidden**。隐藏的语法定义不会显示在菜单中，但仍可以通过插件分配，或者由其他语法定义包含。

## 上下文

对于大多数语言，您需要多个上下文。例如，在C，我们不想for一个字符串的中间字加以强调的关键词。以下是如何处理此问题的示例：

~~~yaml
%YAML 1.2
---
name: C
file_extensions: [c, h]
scope: source.c

contexts:
  main:
    - match: \b(if|else|for|while)\b
      scope: keyword.control.c
    - match: '"'
      push: string

  string:
    - meta_scope: string.quoted.double.c
    - match: \\.
      scope: constant.character.escape.c
    - match: '"'
      pop: true

~~~

第二个模式已添加到与双引号字符匹配的主上下文中（请注意，'"'用于此，因为独立引用将是YAML语法错误），并将新上下文string推送到上下文这意味着文件的其余部分将使用字符串上下文进行处理，而不是主上下文，直到字符串上下文从堆栈中弹出。

字符串上下文引入了一种新模式：meta\_scope。这将在字符串上下文位于堆栈上时将string.quoted.double.c范围分配给所有文本。

在Sublime Text编辑时，您可以通过按Ctrl+shift+p（OSX）或ctrl+alt+shift+p（Windows和Linux）来检查插入符号下文本的范围。

字符串上下文有两种模式：第一种匹配反斜杠字符后跟任何其他字符，第二种匹配引号字符。请注意，最后一个模式指定了一个操作：当遇到未转义的引用时，字符串上下文将从上下文堆栈中弹出，返回使用主上下文分配范围。

当上下文具有多个模式时，将找到最左边的模式。当多个模式在同一位置匹配时，将选择第一个定义的模式。

### 元模式

*   **meta\_scope**。这会将给定范围分配给此上下文中的所有文本，包括将上下文推入堆栈并将其弹出的模式。
*   **meta\_content\_scope**。如上所述，但不适用于触发上下文的文本（例如，在上面的字符串示例中，内容范围不会应用于引号字符）。
*   **meta\_include\_prototype**。用于停止当前上下文自动包含prototype上下文。
*   **clear\_scopes**。此设置允许从当前堆栈中删除范围名称。它可以是整数，或者值为true以删除所有范围名称。它在meta\_scope和meta\_content\_scope之前应用。这通常仅在一种语法嵌入另一种语法时使用。

在任何匹配或包含模式之前，元模式必须首先列在上下文中。

### 匹配模式

一个match模式可以包括以下键：

*   **match**。在[正则表达式](https://raw.githubusercontent.com/kkos/oniguruma/5.9.6/doc/RE)用来匹配的文本。YAML允许在没有引号的情况下编写许多字符串，这有助于使正则表达式更清晰，但重要的是要了解何时需要引用正则表达式。如果你的正则表达式包含字符＃，：，\-，{，\[或\>那么你可能需要引用它。正则表达式一次只针对一行文本运行。
*   **scope**。分配给匹配文本的范围。
*   **captures**。数字到范围的映射，将范围分配给匹配正则表达式的捕获部分。请参阅下面的示例。
*   **push**。推入堆栈的上下文。这可以是单个上下文名称，上下文名称列表，也可以是内联的匿名上下文。
*   **pop**。弹出堆栈中的当前上下文。此键唯一可接受的值是true。
*   **set**。接受与push相同的参数，但首先关闭此上下文，然后将给定的上下文推送到堆栈。
*   **embed**。接受要推入的单个上下文的名称。虽然类似于push，但只要找到escape模式，它就会弹出任意数量的嵌套上下文。这使其成为将一种语法嵌入另一种语法的理想工具。
    *   **escape**。如果使用了embed则此键是必需的，并且是用于退出嵌入式上下文的正则表达式。此模式中的任何反向引用都将引用match正则表达式中的捕获组。
    *   **embed\_scope**。分配给所有文字作用域后匹配的match和前escape。与meta\_content\_scope类似。
    *   **escape\_captures**。对于escape模式，捕获组到作用域名称的映射。使用捕获组0将范围应用于整个转义匹配。

请注意，操作：push，pop，set和embed是独占的，并且在单个匹配模式中只能使用其中一个。

在此示例中，正则表达式包括两个捕获，捕获键用于为每个捕获键分配不同的范围：

~~~yaml
- match: "^\\s*(#)\\s*\\b(include)\\b"
  captures:
    1: meta.preprocessor.c++
    2: keyword.control.include.c++

~~~

### 包括模式

通常将一个上下文的内容包含在另一个上下文中很方便。例如，您可以定义几种不同的上下文来解析C语言，几乎所有上下文都可以包含注释。您可以将它们包括在内，而不是将相关匹配模式复制到每个上下文中：

~~~yaml
expr:
  - include: comments
  - match: \b[0-9]+\b
    scope: constant.numeric.c
  ...

~~~

这里，将引入注释上下文中定义的所有匹配模式和包含模式。它们将被插入到包含模式的位置，因此您仍然可以控制模式顺序。注释上下文中定义的任何元模式都将被忽略。

对于诸如注释之类的元素，包含它们是如此常见，以至于使它们在每个上下文中自动包含它们更简单，而只是列出异常。您可以通过创建名为prototype的上下文来完成此操作，它将自动包含在每个其他上下文的顶部，除非上下文使用meta\_include\_prototype元模式。例如：

~~~yaml
prototype:
  - include: comments

string:
  - meta_include_prototype: false
  ...

~~~

在C中，字符串内的/\*不会启动注释，因此字符串上下文指示不应包含原型。

## 包括其他文件

Sublime Syntax文件支持嵌入另一个语法定义的一个语法定义的概念。例如，HTML可以包含嵌入式JavaScript。以下是HTML的基本语法定义示例：

~~~yaml
scope: text

contexts:
  main:
    - match: <script>
      push: Packages/JavaScript/JavaScript.sublime-syntax
      with_prototype:
        - match: (?=</script>)
          pop: true
    - match: "<"
      scope: punctuation.definition.tag.begin
    - match: ">"
      scope: punctuation.definition.tag.end

~~~

请注意上面的第一条规则。它表示当我们遇到标记时，JavaScript.sublime-syntax中的主要上下文应该被推送到上下文堆栈。它还定义了另一个键，with\_prototype。这包含将插入到JavaScript.sublime-syntax中定义的每个上下文中的模式列表。请注意，with\_prototype在概念上与原型上下文类似，但是它将始终插入到每个引用的上下文中，而不管它们的meta\_include\_prototype设置如何。

在这种情况下，插入的模式将弹出当前上下文，而下一个文本是标记。请注意，它实际上并不匹配标记，它只是使用前瞻断言，它在这里扮演两个关键角色：它都允许HTML规则与结束标记匹配，按照正常情况高亮显示它，以及它将确保弹出所有JavaScript上下文。例如，上下文堆栈可能位于JavaScript字符串的中间，但是当遇到时，JavaScript字符串和主要上下文都将被弹出。

请注意，虽然Sublime Text支持.sublime-syntax和.tmLanguage文件，但是不可能在.sublime-syntax文件中包含.tmLanguage文件。

另一种常见的场景是包含HTML的模板语言。这是一个例子，这次是[Jinja的](http://jinja.pocoo.org/)一个子集：

~~~yaml
scope: text.jinja
contexts:
  main:
    - match: ""
      push: "Packages/HTML/HTML.sublime-syntax"
      with_prototype:
        - match: "{{"
          push: expr

  expr:
    - match: "}}"
      pop: true
    - match: \b(if|else)\b
      scope: keyword.control

~~~

这与HTML嵌入式JavaScript示例完全不同，因为模板语言倾向于从内到外操作：默认情况下，它需要充当HTML，仅在某些表达式上转义为底层模板语言。

在上面的示例中，我们可以看到默认情况下它以HTML模式运行：主要上下文包含一个始终匹配的模式，不使用文本，只包含HTML语法。

在包含HTML语法的地方，Jinja语法指令（{{...}}）通过with\_prototype键包含在内，因此可以注入HTML语法中的每个上下文（以及JavaScript，通过传递性）。

## 变量

几个正则表达式的共同点并不少见。为避免重复输入，您可以使用变量：

~~~yaml
variables:
  ident: '[A-Za-z_][A-Za-z_0-9]*'
contexts:
  main:
    - match: '\b{{ident}}\b'
      scope: keyword.control

~~~

变量必须在.sublime-syntax文件的顶层定义，并通过{{varname}}在regxes中引用。变量本身可能包含其他变量。请注意，任何与{{\[A-Za-z0-9 \_\] +}}不匹配的文本都不会被视为变量，因此正则表达式仍然可以包含文字{{characers，例如。

## 选定的例子

### 支架平衡

此示例高亮显示没有相应开括号的结束括号：

~~~yaml
name: C
scope: source.c

contexts:
  main:
    - match: \(
      push: brackets
    - match: \)
      scope: invalid.illegal.stray-bracket-end

  brackets:
    - match: \)
      pop: true
    - include: main

~~~

### 顺序上下文

此示例将高亮显示包含太多分号的C语句：

~~~yaml
for_stmt:
  - match: \(
    set: for_stmt_expr1
for_stmt_expr1:
  - match: ";"
    set: for_stmt_expr2
  - match: \)
    pop: true
  - include: expr
for_stmt_expr2:
  - match: ";"
    set: for_stmt_expr3
  - match: \)
    pop: true
  - include: expr
for_stmt_expr3:
  - match: \)
    pop: true
  - match: ";"
    scope: invalid.illegal.stray-semi-colon
  - include: expr

~~~

### 高级堆栈使用

在C中，符号通常使用typedef关键字定义。因此，*Goto定义*可以选择它们，符号应该附加entity.name.type范围。

这样做可能有点棘手，因为虽然typedef有时很简单，但它们可能会非常复杂：

~~~clike
typedef int coordinate_t;

typedef struct
{
    int x;
    int y;
} point_t;

~~~

要识别这些，在匹配typedef关键字后，两个上下文将被压入堆栈：第一个将识别一个类型名，然后弹出，而第二个将识别该类型的介绍名称：

~~~yaml
main:
  - match: \btypedef\b
    scope: keyword.control.c
    set: [typedef_after_typename, typename]

typename:
  - match: \bstruct\b
    set:
      - match: "{"
        set:
          - match: "}"
            pop: true
  - match: \b[A-Za-z_][A-Za-z_0-9]*\b
    pop: true

typedef_after_typename:
  - match: \b[A-Za-z_][A-Za-z_0-9]*\b
    scope: entity.name.type
    pop: true

~~~

在上面的示例中，typename是一个可重用的上下文，它将读取一个typename，并在完成后从堆栈中弹出。它可以在需要使用类型的任何上下文中使用，例如在typedef中，或作为函数参数。

在主要方面使用了推栈上两个上下文，在列表中的最右边的背景下成为堆栈上的最顶层上下文匹配模式。一旦typename上下文自动弹出，typedef\_after\_typename上下文将位于堆栈的顶部。

另请注意，在typename上下文中使用匿名上下文是为了简洁起见。

### PHP HEREDOCS

此示例显示如何在PHP 中与[Heredocs](http://php.net/language.types.string#language.types.string.syntax.heredoc)匹配。主上下文中的匹配模式捕获heredoc标识符，并且heredoc上下文中对应的pop模式引用带有\\ 1符号的捕获文本：

~~~yaml
name: PHP
scope: source.php

contexts:
  main:
    - match: <<<([A-Za-z][A-Za-z0-9_]*)
      push: heredoc

  heredoc:
    - meta_scope: string.unquoted.heredoc
    - match: ^\1;
        pop: true

~~~

## 测试

构建语法定义时，不是使用show\_scope\_name命令手动检查作用域，而是可以定义一个语法测试文件来执行检查：

~~~clike
// SYNTAX TEST "Packages/C/C.sublime-syntax"
#pragma once
// <- source.c meta.preprocessor.c++
 // <- keyword.control.import

// foo
// ^ source.c comment.line
// <- punctuation.definition.comment

/* foo */
// ^ source.c comment.block
// <- punctuation.definition.comment.begin
//     ^ punctuation.definition.comment.end

#include "stdio.h"
// <- meta.preprocessor.include.c++
//       ^ meta string punctuation.definition.string.begin
//               ^ meta string punctuation.definition.string.end
int square(int x)
// <- storage.type
//  ^ meta.function entity.name.function
//         ^ storage.type
{
    return x * x;
//  ^^^^^^ keyword.control
}

"Hello, World! // not a comment";
// ^ string.quoted.double
//                  ^ string.quoted.double - comment

~~~

要制作一个，请遵循这些规则

1.  确保文件名以syntax\_test\_开头。
2.  确保文件保存在Packages目录中的某个位置：相应的.sublime-syntax文件旁边是一个不错的选择。
3.  确保文件的第一行以： SYNTAX TEST“”开头。请注意，语法文件可以是.sublime-syntax或.tmLanguage文件。

满足上述条件后，运行带有语法测试或语法定义文件的build命令将运行所有语法测试，并在输出面板中显示结果。*下一个结果*（F4）可用于导航到第一个失败测试。

语法测试文件中的每个测试必须首先启动注释标记（在第一行建立，根据语法实际上不必是注释），然后是^或< -标记。

两种类型的测试是：

*   Caret：^这将针对最近的非测试行上的范围测试以下选择器。它将在^所在的同一列测试它。连续的^将针对选择器测试每一列。
*   箭头：< -这将针对最近的非测试行上的范围测试以下选择器。它将在注释字符所在的同一列中测试它。
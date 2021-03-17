# 范围命名

Sublime Text中的语法定义和颜色方案通过使用范围名称进行交互。范围是虚线字符串，从最少到最具体指定。例如，`if`PHP中的关键字可以通过范围名称指定`keyword.control.php`。

Sublime Text支持TextMate语言语法，并从各种开源包继承其默认语法。该[TextMate的语言的语法文件](https://manual.macromates.com/en/language_grammars#naming_conventions)提供了一组基本已经由社区在慢慢扩大和改变的范围名称。

这是一个活文档，试图记录在语法定义和颜色方案中使用范围名称的最佳实践。所有Sublime Text默认包都努力遵循这些建议。

## 语法定义中的用法

下面记录的范围是在创建语法定义时要使用的建议的范围名称基础集。

在本文档中，从虚线范围名称的末尾省略了语法名称。编写语法时，除非另有说明，否则语法名称应为虚线名称的最后一段。例如，Ruby中的控制关键字是`keyword.control.ruby`，而在Python中则是`keyword.control.python`。

这是一个持续的过程，用于改进和扩展Sublime Text附带的默认语法。截至2016年中期，最近重新编写了以下语法，可以作为参考：

*   [CSS](https://github.com/sublimehq/Packages/blob/master/CSS/CSS.sublime-syntax)
*   [Go](https://github.com/sublimehq/Packages/blob/master/Go/Go.sublime-syntax)
*   [HTML](https://github.com/sublimehq/Packages/blob/master/HTML/HTML.sublime-syntax)
*   [PHP](https://github.com/sublimehq/Packages/blob/master/PHP/PHP.sublime-syntax)
*   [Rust](https://github.com/sublimehq/Packages/blob/master/Rust/Rust.sublime-syntax)

### 顶级范围索引

以下顶级范围列表按字母顺序排序。建议在编写或修改语法之前至少读取整个列表一次。

*   [`comment.`](scope_naming#comment)
*   [`constant.`](scope_naming#constant)
*   [`entity.`](scope_naming#entity)
*   [`invalid.`](scope_naming#invalid)
*   [`keyword.`](scope_naming#keyword)
*   [`markup.`](scope_naming#markup)
*   [`meta.`](scope_naming#meta)

*   [`punctuation.`](scope_naming#punctuation)
*   [`source.`](scope_naming#source)
*   [`storage.`](scope_naming#storage)
*   [`string.`](scope_naming#string)
*   [`support.`](scope_naming#support)
*   [`text.`](scope_naming#text)
*   [`variable.`](scope_naming#variable)

### `COMMENT.`

单行和多行注释应分别使用：

*   `comment.line`
*   `comment.block`

用作文档的多行注释（例如Javadoc或PhpDoc）应使用：

*   `comment.block.documentation`

描述评论的符号，例如`//`或`/*`应另外使用：

*   `punctuation.definition.comment`

具有表示代码部分的特殊语法的注释应仅在文本上使用以下范围。这将使其显示在符号列表中。

*   `meta.toc-list`

### `CONSTANT.`

数字文字，包括整数，浮点数等，应使用以下之一：

*   `constant.numeric`
*   `constant.numeric.integer`
*   `constant.numeric.float`
*   `constant.numeric.hex`
*   `constant.numeric.octal`

语言中内置的常量（如布尔值和空值）应使用：

*   `constant.language`

字符串中的字符转义，例如`\n`和`\x20`，应该使用：

*   `constant.character.escape`

格式化占位符，例如用于`sprintf()`，例如`%s`，应使用：

*   `constant.other.placeholder`

其他特定于语言的常量值（例如Ruby中的符号）应使用：

*   `constant.other`

### `ENTITY.`

实体范围通常分配给代码和标记中的数据结构，类型和其他唯一可识别构造的名称。值得注意的例外是`entity.name.tag`和`entity.other.attribute-name`，它们在HTML和XML标记中使用。

数据结构的名称将使用以下范围之一或新的子范围`entity.name`\- 此列表并非详尽无遗。要提供丰富的语义信息，请使用给定语言构造的特定术语。

*避免`entity.name.type.class`和`entity.name.type.struct`不必要地嵌套范围标签`type`。*

*   `entity.name.class`
*   `entity.name.struct`
*   `entity.name.enum`
*   `entity.name.union`
*   `entity.name.trait`
*   `entity.name.interface`
*   `entity.name.type`

`forward-decl`上述变体用于诸如C和C ++之类的语言。此类范围可用于从符号列表和索引中排除标识符。

*   `entity.name.class.forward-decl`

列为继承类或实现的接口/特征的类，接口和特征名称应使用：

*   `entity.other.inherited-class`

函数名称接收以下范围之一。这些包含在符号列表和索引中。

*   `entity.name.function`
*   `entity.name.function.constructor`
*   `entity.name.function.destructor`

命名空间，包和模块使用以下范围。在一种语言中通常没有多种类型的此类构造，因此这个范围就足够了。

*   `entity.name.namespace`

常量应使用以下范围，或者`variable.other.constant`取决于语言语义。此范围通常包含在符号列表和索引中。

*   `entity.name.constant`

goto结构的标签应该使用：

*   `entity.name.label`

标记语言中的标题名称（例如Markdown和Textile）应使用：

*   `entity.name.section`

HTML和XML标记应使用以下范围。这是`entity.name`应用于重复构造的唯一范围。

*   `entity.name.tag`

HTML，CSS和XML使用以下标记属性名称：

*   `entity.other.attribute-name`

### `INVALID.`

在特定上下文中非法的元素应使用以下范围。过度使用这可能会导致用户在编辑代码时出现令人不快的突出显示。

*   `invalid.illegal`

不推荐使用的元素应使用以下范围确定范围。这应该很少使用，因为用户可能正在使用旧版本的语言。

*   `invalid.deprecated`

### `KEYWORD.`

控制关键字的例子包括`if`，`try`，`end`和`while`。一些语法更喜欢标记`if`和`else`使用`conditional`变体。该`import`变体通常用于适当的情况。

*   `keyword.control`
*   `keyword.control.conditional`
*   `keyword.control.import`

包含标点符号的关键字（例如`@`CSS中的符号）会将以下范围添加到符号中：

*   `punctuation.definition.keyword`

所有剩余的非运营商关键字属于`other`变体：

*   `keyword.other`

运算符通常是符号，因此该术语`keyword`看起来有点矛盾。有时根据运算符的类型引用特定变体。

*   `keyword.operator`
*   `keyword.operator.assignment`
*   `keyword.operator.arithmetic`
*   `keyword.operator.bitwise`
*   `keyword.operator.logical`

当运算符是单词时，例如`and`，`or`或`not`，使用以下变体：

*   `keyword.operator.word`

### `MARKUP.`

标记范围用于内容，而不是代码。这包括Markdown和Textile等语法。

章节标题应使用：

*   `markup.heading`

列表应使用以下之一：

*   `markup.list.unnumbered`
*   `markup.list.numbered`

基本文本样式应使用以下之一：

*   `markup.bold`
*   `markup.italic`
*   `markup.underline`

插入和删除的内容（例如`diff`输出）应使用：

*   `markup.inserted`
*   `markup.deleted`

链接应使用：

*   `markup.underline.link`

Blockquotes和其他引用样式应该使用：

*   `markup.quote`

通常用于代码的内联和块文字引用应该使用：

*   `markup.raw.inline`
*   `markup.raw.block`

其他标记，包括脚注和表格等构造，应使用：

*   `markup.other`

### `META.`

元范围用于范围较大的代码或标记部分，通常包含多个更具体的范围。这些不是由颜色方案设计，而是由首选项和插件使用。

应使用以下范围之一确定数据结构的完整内容。`entity.name`与之类似，它们应根据语言进行定制，以提供丰富的语义信息。它们应包括所有元素，例如名称，继承详细信息和正文。

*   `meta.class`
*   `meta.struct`
*   `meta.enum`
*   `meta.union`
*   `meta.trait`
*   `meta.interface`
*   `meta.type`

函数的整个范围应由以下范围之一涵盖。每个变体应该应用于特定部分，而不是堆叠。例如，`meta.function.php meta.function.parameters.php`永远不应该发生，而是范围应该在`meta.function.php`当时`meta.function.parameters.php`和之间交替`meta.function.php`。

*   `meta.function`
*   `meta.function.parameters`
*   `meta.function.return-type`

命名空间，模块或包的整体应使用：

*   `meta.namespace`

C语言中的预处理程序语句应该使用：

*   `meta.preprocessor`

完整标识符（包括命名空间名称）应使用以下范围。这些标识符是变量，函数和类名的完全限定形式。例如，在C ++中，路径可能看起来像`myns::myclass`，而在PHP中，它看起来像`\MyNS\MyClass`。

*   `meta.path`

函数名称（包括完整路径）和所有参数应接收以下范围。`variable.function`除非函数作用域，否则函数或方法的名称应该是`support.function`。

*   `meta.function-call`

由大括号描述的代码段应`meta`基于适当的语义使用以下范围之一。在`{`和`}`字符应该另外使用`punctuation`范围。

*   `meta.block`
*   `punctuation.section.block.begin`
*   `punctuation.section.block.end`
*   `meta.braces`
*   `punctuation.section.braces.begin`
*   `punctuation.section.braces.end`

由括号描述的代码段应`meta`基于适当的语义使用以下范围之一。在`(`和`)`字符应该另外使用`punctuation`范围。

*   `meta.group`
*   `punctuation.section.group.begin`
*   `punctuation.section.group.end`
*   `meta.parens`
*   `punctuation.section.parens.begin`
*   `punctuation.section.parens.end`

由方括号描述的代码段应使用以下范围。在`[`和`]`字符应该另外使用`punctuation`范围。

*   `meta.brackets`
*   `punctuation.section.brackets.begin`
*   `punctuation.section.brackets.end`

通用数据类型构造应使用以下范围。任何表示开头和结尾的符号，例如`<`和`>`，都应该另外使用`punctuation`范围。

*   `meta.generic`
*   `punctuation.definition.generic.begin`
*   `punctuation.definition.generic.end`

HTML和XML标记（包括标点符号，名称和属性）应使用以下内容：

*   `meta.tag`

标记语言中的段落使用：

*   `meta.paragraph`

### `PUNCTUATION.`

以下范围是未嵌入其他范围的标点范围。例如，该[`string.`](scope_naming#string)部分包含有关字符串标点符号范围的文档。

逗号和冒号等分隔符应使用：

*   `punctuation.separator`

分号或其他语句终止符应使用：

*   `punctuation.terminator`

行连续字符（例如Python和R）应该使用：

*   `punctuation.separator.continuation`

成员访问权限，范围解析或类似构造应使用以下范围。对于Python或JavaScript，这将是`.`。在PHP中，这将适用于`->`和`::`。在C ++中，这将适用于所有三个。

*   `punctuation.accessor`

### `SOURCE.`

以下范围的特定于语言的变体通常应用于整个源代码文件：

*   `source`

### `STORAGE.`

类型和定义/声明关键字应使用以下范围。例子包括`int`，`bool`，`char`，`func`，`function`，`class`和`def`。根据语言和语义，`const`可能是这个或`storage.modifier`。

*   `storage.type`

影响变量，函数或数据结构存储的关键字应使用以下范围。实例包括`static`，`inline`，`const`，`public`和`private`。

*   `storage.modifier`

### `STRING.`

基本字符串使用以下范围之一，基于所使用的引号类型：

*   `string.quoted.single`
*   `string.quoted.double`
*   `string.quoted.triple`

使用非常规报价的字符串，例如`<`和`>`C导入，应使用：

*   `string.quoted.other`

字符串开头和结尾的标点符号应使用：

*   `punctuation.definition.string.begin`
*   `punctuation.definition.string.end`

不带引号的字符串，例如Shell和Batch File，应该使用：

*   `string.unquoted`

正则表达式文字应该使用：

*   `string.regexp`

### `SUPPORT.`

基本框架提供的元素应使用以下范围之一。示例包括Objective-C中的Cocoa或JavaScript中的浏览器/节点。

*   `support.constant`
*   `support.function`
*   `support.module`

虽然也用于基础框架，但许多语法将这些应用于范围无法识别的类和类型，从而有效地确定了所有用户构造的范围。

*   `support.type`
*   `support.class`

### `TEXT.`

编程语言[`source.`](scope_naming#source)用作基本范围，而内容使用`text.`。最大的区别之一是在`text.`范围内禁用了许多插件和其他动态功能。[`markup.`](scope_naming#markup)范围通常在文本中使用。

HTML应使用以下范围。此范围的变体与其他范围不同，因为变体总是在``诸如`text.basic`或之后添加`text.markdown`。

*   `text`

XML应该使用：

*   `text.xml`

### `VARIABLE.`

通用变量应使用以下范围。有些语言使用`readwrite`变体与`constant`下面讨论的变体进行对比。

*   `variable.other`
*   `variable.other.readwrite`

作为变量名称一部分的符号应另外应用于以下范围。例如，`$`在PHP和Shell中。

*   `punctuation.definition.variable`

通常通过`const`修饰符的不可变变量应该具有以下范围。根据语言和语义，`entity.name.constant`可能是更好的选择。

*   `variable.other.constant`

由指定的语言保留的变量名，如`this`，`self`，`super`，等应使用：

*   `variable.language`

一个或多个函数的参数应使用以下范围。这也可以用于其他类似参数的变量，例如Go中的接收器或命名返回值。

*   `variable.parameter`

类或其他数据结构的字段，属性，成员和属性应使用：

*   `variable.other.member`

函数和方法名称应使用以下内容作用域，但仅限于调用它们时。定义时，他们应该使用`entity.name.function`。

*   `variable.function`

## 在配色方案中的用法

通常，在颜色方案中将颜色和样式应用于范围时，应首先设置选择器的最常规形式。利用上一节中概述的范围的高质量语法应该为最终用户带来良好的用户体验。

### 最小范围覆盖范围

以下是要强调的建议的最小范围集。添加额外可能会导致略微改善的体验，但是过于具体会导致颜色方案通常只对一个或两个语法看起来很好。

*   `entity.name`
*   `entity.other.inherited-class`
*   `entity.name.section`
*   `entity.name.tag`
*   `entity.other.attribute-name`
*   `variable`
*   `variable.language`
*   `variable.parameter`
*   `variable.function`
*   `constant`
*   `constant.numeric`
*   `constant.language`
*   `constant.character.escape`
*   `storage.type`
*   `storage.modifier`
*   `support`
*   `keyword`
*   `keyword.control`
*   `keyword.operator`
*   `string`
*   `comment`
*   `invalid`
*   `invalid.deprecated`

### `META.`颜色

在样式范围内，抵制直接设置`meta`范围的冲动。它们主要用于为首选项和插件提供上下文信息。

### `ENTITY.NAME.`颜色

历史上，许多配色方案已经提供了一种颜色`entity.name.function`和`entity.name.type`，经常用不同的颜色`entity.name.tag`。这使得新`entity.name.*`范围不突出显示。

颜色方案应该指定一种颜色`entity.name`，它将应用于类，类型，结构，接口和许多其他数据结构。可以为两个范围覆盖此颜色，`entity.name.tag`并将`entity.name.section`其用于不同类型的构造。
# 完成
Sublime文本包括一些通过完成单词或插入样板来节省打字和时间的方法。完成包括以下来源:

*   Words from the current file
*   Context-aware suggestions4.0
*   Completion files
*   Snippet files
*   Plugins

存在各种设置来自定义完成行为。用户可以编写自己的片段或完成文件，并且存在许多第三方包来提供完成。

## Usage

默认情况下，当用户编辑源代码或标记时，Sublime Text将自动显示补全弹出窗口，但不在注释、字符串或标记中的散文中。


按 **Esc** 键将隐藏完成弹出窗口。手动显示完成弹出，按 **Ctrl** + **Space**。 *如果没有完成可用，message`No available completions` 将显示在状态栏。*

## 上下文感知建议 4.0

Sublime Text中的完成引擎使用后台进程扫描项目中的所有文件以构建完成索引。该索引用于根据现有代码中的模式向用户提供建议的完成。

示例:

*   如果经常将属性设置为布尔值，则建议将包括`true`or`false`
*   如果标识符通常是 “被调用的”，则该名称将被建议附加 `()`

所提供的精确完成是基于各种启发式算法，并从项目中的现有代码中导出。*由于完成是基于对现有代码的分析，因此不会建议项目中未使用的单词。*

## 完成元数据 4.0

除文本内容外，补全还可能为用户提供其他详细信息。这些包括完成所代表的元素的 *种类* 、帮助选择完成的简短 *注释* 以及可能包含指向其他资源的链接的 *详细信息*。

&nbsp;Kind info f apply() s application m absolute() s acls abstract class&nbsp; Annotation c agent Struct&nbsp;2 Definitions&nbsp;Details

### KIND INFO

完成可能会提供与完成触发器一起提供的信息。这包括一个高级类别、一个识别字母和一个名称。以下是一些最常见的种类:

| Icon | Name       |
|------|------------|
| k    | Keyword    |
| t    | Type       |
| f    | Function   |
| a    | Namespace  |
| n    | Navigation |
| m    | Markup     |
| v    | Variable   |
| s    | Snippet    |

*在本文档和Sublime Text中，将鼠标悬停在一个字母上会显示一个带有全名的工具提示。种类元数据的颜色由主题决定，可能与上面显示的不匹配。*

.sublime-completions文件和插件可以使用上面列出的任何类别的组合，以及自定义演示的任何Unicode字符和名称。

### 注释

注释显示在完成弹出窗口的右侧边缘，并且可能包含作者认为有用的任何信息。通常，注释将是一两个单词。

### DETAILS

完成的 `details` 字段可能包含带有链接的富文本描述。选择完成时，完成的详细信息显示在完成弹出窗口的底部。

## 定制化

有许多方法可以通过新的完成来增强引擎:

### 完成文件


向Sublime Text添加完成的最基本形式是创建一个.sublime-completions 文件。Completions文件使用JSON格式，并包含具有键 "scope" 和 "completions" 的对象。

"scope" 键的值是一个字符串，其中包含应用于completions的语法的 [选择器] (selectors)。"completions" 值是完成的数组。数组中的每个条目表示单个完成，并且可以是字符串或对象。

#### 简单格式

当完成是一个字符串时，该字符串同时代表触发器文本和完成内容。

*一个basic.sublime-completions file:*

~~~
{
    "scope": "source.python",
    "completions": [
        "def",
        "class",
        "None",
        "True",
        "False"
    ]
}
~~~

#### 复格式

当完成是一个对象时，有效键包括:

trigger string REQUIRED

>用户必须输入的文本以匹配完成

content sstring REQUIRED

>将插入到文件中的内容。支持片段 [fields] (completions#snippet_fields) 和 [variables] (completions#snippet_variables)。 

annotation 4.0 string

>要显示以完成的注释。

kind 4.0 string, 3-element array of strings

>完成的元数据种类。

如果值是字符串，则它必须是以下之一:

*   `"ambiguous"`
*   `"function"`
*   `"keyword"`
*   `"markup"`
*   `"namespace"`
*   `"navigation"`
*   `"snippet"`
*   `"type"`
*   `"variable"`

举例:`"kind":"function"`

如果值是字符串的3元素数组，则它们必须为:

1.  上面列表中的一个字符串，主题使用它来选择元数据类型的颜色
2.  要显示在触发器左侧的单个Unicode字符
3.  种类的描述，可在种类字母工具提示和详细信息窗格中查看 (当可见时)

举例:`"kind": ["function","m","Method"]`

details 4.0 string

完成的单行描述。可能包含以下用于基本格式设置的HTML标记:

*   `<ahref="">`–[protocols](minihtml#protocols)
*   `<b>`
*   `<strong>`
*   `<i>`
*   `<em>`
*   `<u>`
*   `<tt>`
*   `<code>`

*除了上面列出的属性或标签外，不支持其他属性或标签。*

举例 :`"details":"Wraps selection in a <code>&lt;b&gt;</code> tag"`

*A.sublime-completions file with examples of each field:*

~~~
{
    "scope": "source.python",
    "completions": [
        {
            "trigger": "def",
            "contents": "def",
            "kind": "keyword"
        },
        {
            "trigger": "fun",
            "annotation": "basic function",
            "contents": "def ${1:name}($2):\n    $0\n",
            "kind": "snippet",
            "details": "A simple, non-<code>async</code> function definition"
        }
    ]
}
~~~

### 片段

片段通常用于样板类型的内容，由于跨越多行，因此不容易使用.sublime-completions 格式进行创作。

片段是扩展名为.sublime-snippet的XML文件。它们具有顶级标签 `<snippet>`，其中包含以下标签:

scope

> 片段应启用的[选择器](selectors)语法

tabTrigger

> 用于匹配完成弹出窗口中的片段的文本

contents

> 应用片段时插入文档的文本。支持 [fields](completions#snippet_fields) 和 [variables] (completions#snippet_variables)。

> 通常，此标签的内容被包装在 `<![CDATA['and']]>`中，因此不需要对内容进行XML转义。

description

> 片段的可选描述，显示在选项板中

*An example.sublime-snippetfile:*

~~~
<snippet>
    <scope>source.python</scope>
    <tabTrigger>fun</tabTrigger>
    <content><![CDATA[def ${1:name}($2):
    ${0:pass}]]></content>
    <description>function, non-async</description>
</snippet>

~~~

#### FIELDS

片段支持 *字段*，用户在插入片段后可以通过片段中的位置tab切换。字段可以是简单的位置，但也可以提供默认内容。

简单字段为 `$`，后跟整数。字段 `$0` 是片段完成后将放置选择的位置。字段 `$1` 到 `$n` 在移动到 `$0` 之前都已填写。

~~~
Name: $1
Email: $2
Description: $0

~~~

具有默认内容的字段使用格式 `${1:default text}`。默认内容可以是文字文本，也可以包含[变量](completions#snippet_variables)。

~~~
Name: ${1:first} ${2:last}
Email: ${3:user}@${4:example.com}
~~~

如果片段不包含字段 `$0`，则在末尾隐式添加该片段。

#### 变量

可以将以下变量添加到代码段中，以包括要插入代码段的文件中的信息:

| Variable            | Description                                         |
|---------------------|-----------------------------------------------------|
| `$SELECTION`        | The current selection                               |
| `$TM_SELECTED_TEXT` | The current selection                               |
| `$TM_LINE_INDEX`    | The 0-based line number of the current line         |
| `$TM_LINE_NUMBER`   | The 1-based line number of the current line         |
| `$TM_DIRECTORY`     | The path to the directory containing the file       |
| `$TM_FILEPATH`      | The path to the file                                |
| `$TM_FILENAME`      | The file name of the file                           |
| `$TM_CURRENT_WORD`  | The contents of the current word                    |
| `$TM_CURRENT_LINE`  | The contents of the current line                    |
| `$TM_TAB_SIZE`      | The number of spaces per tab                        |
| `$TM_SOFT_TABS`     | `YES`or`NO`– if tabs should be translated to spaces |
| `$TM_SCOPE`         | The base scope name of the file’s syntax            |

除了上面命名的变量之外，字段还可以用作变量，允许用户输入单个值并在多个位置重复该值。

~~~
Name: ${1:first} ${2:last}
Email: ${3:$1}@${4:example.com}
~~~

#### 变量替换

变量可以直接引用，也可以使用正则表达式对其进行修改。 带有替换的变量以以下格式编写`${*name*/*regex*/*replace*/*flags*}`.

*正则表达式* 段支持[regular expressions](https://www.boost.org/doc/libs/1_64_0/libs/regex/doc/html/boost_regex/syntax/perl_syntax). *replace* 段支持相应的[替换格式](https://www.boost.org/doc/libs/1_64_0/libs/regex/doc/html/boost_regex/format/boost_format_syntax). *标志* 段将包含来自以下内容的零个字母:

*   `g`– 所有事件，而不仅仅是第一次，都应该被替换
*   `i`– 不区分大小写匹配
*   `m`– 多行模式，其中 '^' 匹配每行的开头

通常情况下，变量替换与引用字段的数值变量相结合。

*在删除第一个空格和之后的所有内容之后，以下内容使用第一个字段作为第二个字段的默认内容:*

~~~
Name: ${1:name}
Email: ${2:${1/\s.*//}}@${3:example.com}
~~~

#### ESCAPING

由于片段可以包含以 `$` 开头的变量，因此必须将文字 `$` 字符写成 `\$`。

执行变量替换时，必须将 `/` 字符写成 `\/`。

### 插件

添加完成的最强大的工具是Python插件。

编写插件以提供完成包括在扩展的类上实现方法 on\_query\_completions()[事件监听](api_reference#sublime_plugin.EventListener)或[ViewEventListener](api_reference#sublime_plugin.ViewEventListener).

~~~
import sublime
import sublime_plugin


class MyCompletions(sublime_plugin.EventListener):
    def on_query_completions(self, view, prefix, locations):
        if not view.match_selector(locations[0], "source.python"):
            return []
        
        available_completions = [
            "def",
            "class",
            "None",
            "True",
            "False"
        ]
        
        prefix = prefix.lower()

        out = []
        for comp in available_completions:
            if comp.lower().startswith(prefix):
                out.append(comp)
        
        return out


~~~

异步完成可以通过on\_query\_completions() 返回 一个[CompletionList](api_reference#sublime.CompletionList)对象.4050

完成细节，包括种类元数据，由从on\_query\_completions()返回[CompletionItem](api_reference#sublime.CompletionItem)对象提供。4050

## 设置

tab\_completion boolean

> 启用后，按 **Tab** 将插入最佳匹配完成。禁用时，**Tab**只会触发片段或插入制表符。**Shift***+***Tab**可使用明确选项卡标签。

> *禁用此设置不会隐式禁用自动完成。*

auto\_complete boolean

> 键入时自动显示完成弹出窗口。

> *此行为不受设置选项卡 tab\_completion 的影响。*

auto\_complete\_size\_limit integer

> 如果当前文件的文件大小 (以字节为单位) 大于此大小，则不会自动显示完成弹出窗口。

auto\_complete\_delay integer

> 自动显示完成弹出窗口之前要等待的毫秒数。

auto\_complete\_selector string

> 一个 [选择器](selectors)，用于限制何时自动显示完成弹出窗口。

> 示例:`"meta.tag, source - comment - string.quoted.double.block - string.quoted.single.block - string.unquoted.heredoc"`

> *`auto\_complete\_triggers` 设置可用于在特定情况下重新启用自动完成弹出窗口。*

auto\_complete\_triggers array of objects

> 为何时自动显示完成弹出窗口提供显式触发器。

> 每个对象必须包含键 `"selector"` ，其字符串值包含 [选择器](selectors)，以匹配插入符号位置，和带有字符串值的 “字符” 键，指定插入符号左侧必须存在哪些字符。

>Example:

~~~
[
    {
        "selector": "text",
        "characters": "<"
    }
]
~~~

> *触发器将覆盖设置 auto\_complete\_selector 。*

auto\_complete\_commit\_on\_tab boolean

> 默认情况下，自动完成将在**Enter**上提交当前完成。此设置可用于在**Tab** 上完成。

> 在**Tab** 上完成通常是一个更好的选择，因为它消除了提交完成和插入换行符之间的歧义。

auto\_complete\_with\_fields boolean

> 控制片段字段处于活动状态时是否自动显示完成弹出窗口。仅在启用了 auto\_complete\_commit\_on\_tab 选项卡上的 “提交” 时才相关。

auto\_complete\_cycle boolean

> 控制按**⬆**时会发生什么 当选择完成弹出窗口中的第一项时: 如果为 `false`，则隐藏弹出窗口，否则选择弹出窗口中的最后一个完成。

> 也使第一完成选择当 **⬇**在最后一次完成时按下。

auto\_complete\_use\_history boolean

> 是否应该自动选择先前选择的完成

auto\_complete\_use\_index 4.0 boolean

> 启用后，完成弹出窗口将显示基于项目中其他文件的上下文感知建议。

auto\_complete\_preserve\_order 4.0 string

> 控制在键入时如何重新排序自动完成结果:

*   `"none"`– 根据完成与键入的文本的匹配程度对结果进行完全重新排序
*   `"some"`– 部分重新排序结果，考虑到完成与类型匹配的程度以及完成的可能性
*   `"strict"`– 永远不对结果重新排序

auto\_complete\_trailing\_symbols 4.0 boolean

> 是否完成引擎认为尾随符号可能足够，则添加尾随符号 (例如`.`,`()`)

auto\_complete\_trailing\_spaces 4.0 boolean

> 是否完成引擎认为它们可能足够，则在完成后添加一个空格

auto\_complete\_include\_snippets 4.0 boolean

> 控制片段是否不会包含在完成弹出窗口中。

> *禁用后，仍然可以通过输入选项卡触发器并在未显示完成弹出窗口时按**Tab**来触发片段。*
auto\_complete\_include\_snippets\_when\_typing 4.0 boolean

> 当它被设置为 `false`动触发时，片段不会出现在完成弹出窗口中。如果手动触发，将显示它们。

ignored\_snippets4.0 array of strings

> [文件模式](file_patterns) 指定要忽略的片段文件。

> *例如，忽略所有默认的 C++ snippets:*

~~~
[
    "C++/*"
]
~~~
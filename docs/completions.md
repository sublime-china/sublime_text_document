# Completions

Version:  
[Dev](completions#ver-dev)[3.2](completions#ver-3.2)[3.1](completions#ver-3.1)[3.0](completions#ver-3.0)

Sublime Text includes a few methods to save typing and time by finishing words or inserting boilerplate. Completions include the following sources:

*   Words from the current file
*   Context-aware suggestions4.0
*   Completion files
*   Snippet files
*   Plugins

Various settings exist to customize the behavior of completions. Users can write their own snippets or completions files, and many third-party packages exist to provide completions.

*   [Usage](completions#usage)
*   [Context-Aware Suggestions](completions#context-aware_suggestions)4.0
*   [Completion Metadata](completions#completion_metadata)4.0
*   [Customization](completions#customization)
    *   [Completion Files](completions#completion_files)
    *   [Snippets](completions#snippets)
        *   [Fields](completions#snippet_fields)
        *   [Variables](completions#snippet_variables)
        *   [Variable Substitution](completions#variable_substitution)
        *   [Escaping](completions#snippet_escaping)
    *   [Plugins](completions#plugins)
*   [Settings](completions#settings)

## Usage

By default, Sublime Text will automatically show the completions popup when a user is editing source code or markup, but not within prose in comments, strings or markups.

Pressing the**Esc**key will hide the completions popup. To manually show the completions popup, press**Ctrl***+***Space**.*If no completions are available, the message`No available completions`will be displayed in the status bar.*

## Context-Aware Suggestions4.0

The completion engine in Sublime Text uses background processes to scan all of the files in a project to build a completion index. This index is used to provide suggested completions to the user, based on patterns in existing code.

Some examples:

*   if a property is frequently set to a boolean value, the suggestions will include`true`or`false`
*   if an identifier is typically "called", the name will be suggested with`()`appended

The exact completions offered are based on various heuristics, and are derived from existing code in a project.*Since completions are based on analysing existing code, words not used in a project will not be suggested.*

## Completion Metadata4.0

In addition to their textual contents, completions may also provide additional details to users. These include the*kind*of element the completion represents, a short*annotation*to help in picking a completion, and*details*that may contain links to additional resources.

&nbsp;Kind infofapply()sapplicationmabsolute()saclsabstract class&nbsp;AnnotationcagentStruct&nbsp;2 Definitions&nbsp;Details

### KIND INFO

Completions may provide*kind*info to be presented with the completion trigger. This includes a high-level category, an identifying letter, and a name. The following are some of the most common kinds:

| Icon | Name |
| --- | --- |
| k | Keyword |
| t | Type |
| f | Function |
| a | Namespace |
| n | Navigation |
| m | Markup |
| v | Variable |
| s | Snippet |

*Both in this documentation and in Sublime Text, hovering the mouse over a kind letter will show a tooltip with the full name. The color of kind metadata is determined by the theme, and may not match what is shown above.*

.sublime-completionsfiles and plugins can use combinations of any category listed above, along with any Unicode character and name for a custom presentation.

### ANNOTATIONS

Annotations are displayed on the right-hand edge of the completions popup, and may contain any information the author deems useful. Typically the annotations will be a word or two.

### DETAILS

The`details`field of a completion may contain a rich text description with links. The details for a completion is shown at the bottom of the completions popup when the completion is selected.

## Customization

There exist a number of ways in which the engine can be augmented with new completions:

*   [Completion Files](completions#completion_files)
*   [Snippets](completions#snippets)
*   [Plugins](completions#plugins)

### COMPLETION FILES

The most basic form of adding completions to Sublime Text is by creating a.sublime-completionsfile. Completions files use the JSON format, and contain an object with the keys"scope"and"completions".

The"scope"key’s value is a string containing a[selector](selectors)of the syntax the completions apply to. The"completions"value is an array of completions. Each entry in the array represents a single completion, and may be a string or an object.

#### SIMPLE FORMAT

When a completion is a string, the string reprents both the trigger text and the completion contents.

*A basic.sublime-completionsfile:*

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

#### RICH FORMAT

When a completion is an object, the valid keys include:

triggerstringREQUIRED

The text the user must enter to match the completion

contentsstringREQUIRED

The contents that will be inserted into the file. Supports snippet[fields](completions#snippet_fields)and[variables](completions#snippet_variables).

annotation4.0string

The annotation to display for the completion.

kind4.0string, 3-element array of strings

The kind metadata for the completion.

If the value is a string, it must be one of:

*   `"ambiguous"`
*   `"function"`
*   `"keyword"`
*   `"markup"`
*   `"namespace"`
*   `"navigation"`
*   `"snippet"`
*   `"type"`
*   `"variable"`

Example:`"kind":"function"`

If the value is a 3-element array of strings, they must be:

1.  A string from the list above, which is used by the theme to select the color of the kind metadata
2.  A single Unicode character to be shown to the left of the trigger
3.  A description of the kind, viewable in the kind letter tool tip, and the detail pane (when visible)

Example:`"kind": ["function","m","Method"]`

details4.0string

A single line description of the completion. May contain the following HTML tags for basic formatting:

*   `<ahref="">`–[protocols](minihtml#protocols)
*   `<b>`
*   `<strong>`
*   `<i>`
*   `<em>`
*   `<u>`
*   `<tt>`
*   `<code>`

*No other attributes or tags are supported other than those listed above.*

Example:`"details":"Wraps selection in a <code>&lt;b&gt;</code> tag"`

*A.sublime-completionsfile with examples of each field:*

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

### SNIPPETS

Snippets are typically used for boilerplate-type content that isn’t easily authored using the.sublime-completionsformat due to spanning multiple lines.

Snippets are XML files with the extension.sublime-snippet. They have a top-level tag`<snippet>`, containing the following tags:

scope

The[selector](selectors)of the syntax the snippet should be enabled for

tabTrigger

The text used to match the snippet in the completions popup

contents

The text to insert into the document when the snippet it applied. Supports[fields](completions#snippet_fields)and[variables](completions#snippet_variables).

Typically the contents of this tag are wrapped in`<![CDATA[`and`]]>`so that the contents do not need to be XML-escaped.

description

An optional description of the snippet, which is shown in theCommand Palette

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

Snippets support*fields*, locations within the snippet that the user may**Tab**through after inserting the snippet. Fields may be simple positions, but may also provide default content.

Simple fields are a`$`followed by an integer. Field`$0`is where the selection will be placed once the snippet is completed. Fields`$1`through`$n`are all filled in before moving to`$0`.

~~~
Name: $1
Email: $2
Description: $0

~~~

Fields with default content use the format`${1:default text}`. Default content may be literal text, or it may contain[variables](completions#snippet_variables).

~~~
Name: ${1:first} ${2:last}
Email: ${3:user}@${4:example.com}
~~~

If a snippet does not contain field`$0`, it is implicitly added at the end.

#### VARIABLES

The following variables may be added to a snippet to include information from the file the snippet is being inserted into:

| Variable | Description |
| --- | --- |
| `$SELECTION` | The current selection |
| `$TM_SELECTED_TEXT` | The current selection |
| `$TM_LINE_INDEX` | The 0-based line number of the current line |
| `$TM_LINE_NUMBER` | The 1-based line number of the current line |
| `$TM_DIRECTORY` | The path to the directory containing the file |
| `$TM_FILEPATH` | The path to the file |
| `$TM_FILENAME` | The file name of the file |
| `$TM_CURRENT_WORD` | The contents of the current word |
| `$TM_CURRENT_LINE` | The contents of the current line |
| `$TM_TAB_SIZE` | The number of spaces per tab |
| `$TM_SOFT_TABS` | `YES`or`NO`– if tabs should be translated to spaces |
| `$TM_SCOPE` | The base scope name of the file’s syntax |

In addition to the named variables above, fields may also be used as variables, allowing a user to enter a single value and have it repeated in multiple places.

~~~
Name: ${1:first} ${2:last}
Email: ${3:$1}@${4:example.com}
~~~

#### VARIABLE SUBSTITUTION

Variables can be directly referenced, or they may be modified using a regular expression. Variables with substitutions are written in the format`${*name*/*regex*/*replace*/*flags*}`.

The*regex*segment supports[regular expressions](https://www.boost.org/doc/libs/1_64_0/libs/regex/doc/html/boost_regex/syntax/perl_syntax). The*replace*segment supports a corresponding[replace format](https://www.boost.org/doc/libs/1_64_0/libs/regex/doc/html/boost_regex/format/boost_format_syntax). The*flags*segment will contain zero letters from:

*   `g`– all occurences, rather than just the first, should be replaced
*   `i`– case insensitive matching
*   `m`– multiline mode, where`^`matches the beginning of each line

Often times variable substitutions are combined with numeric variables referencing fields.

*The following uses the first field as the default content of the second field, after removing the first whitespace and everything after:*

~~~
Name: ${1:name}
Email: ${2:${1/\s.*//}}@${3:example.com}
~~~

#### ESCAPING

Since snippets can contain variables, which start with a`$`, literal`$`characters must be written as`\$`.

When performing variable substitution, literal`/`characters must be written as`\/`.

### PLUGINS

The most powerful tool for adding completions are Python plugins.

Writing a plugin to provide completions involves implementing the methodon\_query\_completions()on a class that extends[EventListener](api_reference#sublime_plugin.EventListener)or[ViewEventListener](api_reference#sublime_plugin.ViewEventListener).

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

Asynchronous completions can be provided byon\_query\_completions()returning a[CompletionList](api_reference#sublime.CompletionList)object.4050

Completion details, including kind metadata, are provided by returning[CompletionItem](api_reference#sublime.CompletionItem)objects fromon\_query\_completions().4050

## Settings

tab\_completionboolean

When enabled, pressing**Tab**will insert the best matching completion. When disabled,**Tab**will only trigger snippets or insert a tab character.**Shift***+***Tab**can be used to insert an explicit tab whentab\_completionis enabled.

*Disabling this setting will not implicitly disableauto\_complete.*

auto\_completeboolean

Automatically show the completions popup when typing.

*This behavior is not influenced by the settingtab\_completion.*

auto\_complete\_size\_limitinteger

If the filesize in bytes of the current file is larger than this, the completions popup will not be automatically shown.

auto\_complete\_delayinteger

The number of milliseconds to wait before showing the completions popup automatically.

auto\_complete\_selectorstring

A[selector](selectors)to limit when the completions popup will be automatically shown.

Example:`"meta.tag, source - comment - string.quoted.double.block - string.quoted.single.block - string.unquoted.heredoc"`

*Theauto\_complete\_triggers"setting may be used to re-enable the automatic completions popup in specific situations.*

auto\_complete\_triggersarray of objects

Provides explicit triggers for when to automatically show the completions popup.

Each object must contain the keys`"selector"`with a string value containing a[selector](selectors)to match the caret position against, and a`"characters"`key with a string value specifying what characters must be present to the left of the caret.

Example:

~~~
[
    {
        "selector": "text",
        "characters": "<"
    }
]
~~~

*Triggers will override the settingauto\_complete\_selector.*

auto\_complete\_commit\_on\_tabboolean

By default, auto complete will commit the current completion on**Enter**. This setting can be used to make it complete on**Tab**instead.

Completing on**Tab**is generally a superior option, as it removes ambiguity between committing the completion and inserting a newline.

auto\_complete\_with\_fieldsboolean

Controls if the completions popup is automatically shown when snippet fields are active. Only relevant ifauto\_complete\_commit\_on\_tabis enabled.

auto\_complete\_cycleboolean

Controls what happens when pressing**⬆**while the first item in the completions popup is selected: if`false`, the popup is hidden, otherwise the last completion in the popup is selected.

Also causes the first completion to be selected when**⬇**is pressed on the last completion.

auto\_complete\_use\_historyboolean

If previously-selected completions should be automatically selected

auto\_complete\_use\_index4.0boolean

When enabled, the completions popup will show context-aware suggestions based on other files in the project

auto\_complete\_preserve\_order4.0string

Controls how the auto complete results are reordered when typing:

*   `"none"`– fully reorder the results according to how well the completion matches the typed text
*   `"some"`– partially reorder the results, taking into account how well the completion matches whats typed, and likelihood of the completion
*   `"strict"`– never reorder the results

auto\_complete\_trailing\_symbols4.0boolean

Add trailing symbols (e.g.,`.`,`()`) if the completion engine thinks they‘re likely enough

auto\_complete\_trailing\_spaces4.0boolean

Add a space after completions if the completion engine thinks they‘re likely enough

auto\_complete\_include\_snippets4.0boolean

Controls if snippets will not be included in the completions popup.

*When disabled, snippets can still be triggered by typing their tab trigger in, and pressing**Tab**when the completion popup is not showing.*

auto\_complete\_include\_snippets\_when\_typing4.0boolean

When this is set to`false`, snippets won‘t be present in the completions popup when it is automatically triggered. They will be shown if it is manually triggered.

ignored\_snippets4.0array of strings

[File patterns](file_patterns)specifying which snippet files to ignore.

*For example, to ignore all the default C++ snippets:*

~~~
[
    "C++/*"
]
~~~
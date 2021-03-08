# 

[DOCUMENTATION](https://www.sublimetext.com/docs/index.html)[TOC](https://www.sublimetext.com/docs/key_bindings.html#toc)[TOP](https://www.sublimetext.com/docs/key_bindings.html#)

Key Bindings

Key bindings in Sublime Text are defined by files ending in.sublime-keymap. Key bindings use JSON, with the top-level structure being an array. Each binding is a JSON object.

*   [Example](https://www.sublimetext.com/docs/key_bindings.html#example)
*   [Bindings](https://www.sublimetext.com/docs/key_bindings.html#bindings)
    *   ["keys"Key](https://www.sublimetext.com/docs/key_bindings.html#keys)
    *   ["command"Key](https://www.sublimetext.com/docs/key_bindings.html#command)
    *   ["args"Key](https://www.sublimetext.com/docs/key_bindings.html#args)
    *   ["context"Key](https://www.sublimetext.com/docs/key_bindings.html#context)
*   [User Bindings](https://www.sublimetext.com/docs/key_bindings.html#user_bindings)

## Example

*The following is an example of the format of a.sublime-keymapfile.*

~~~
[
    {
        "keys": ["super+ctrl+m"],
        "command": "convert_syntax"
    },
    {
        "keys": ["super+shift+9"],
        "command": "set_layout",
        "args":
        {
            "cols": [0.0, 0.33, 0.66, 1.0],
            "rows": [0.0, 0.33, 0.66, 1.0],
            "cells":
            [
                [0, 0, 1, 1], [1, 0, 2, 1], [2, 0, 3, 1],
                [0, 1, 1, 2], [1, 1, 2, 2], [2, 1, 3, 2],
                [0, 2, 1, 3], [1, 2, 2, 3], [2, 2, 3, 3]
            ]
        }
    },
    {
        "keys": ["super+alt+up"],
        "command": "noop",
        "context":
        [
            {"key": "panel", "operand": "find"},
            {"key": "panel_has_focus"},
        ]
    }
]
~~~

## Bindings

Each key binding requires two keys,"keys"and"command". To pass args to a command, the"args"key should be specified. To restrict a key binding to a specific situation, the"context"key must be included.

### "keys"KEY

The"keys"value must be an array of strings, where each string contains a*key press*, comprised of a key and any modifiers. When multiple key presses are present in the array, the command will only be invoked if the presses are performed in sequence.

*A key binding for the**Escape**key*

~~~
{
    "keys": ["escape"],
    "command": "noop"
}
~~~

*A key binding for the key**A**with the modifier**Ctrl***

~~~
{
    "keys": ["ctrl+a"],
    "command": "noop"
}
~~~

#### MODIFIERS

The following modifiers may be combined with key names for each*key press*.

*   `ctrl`
*   `control`
*   `alt`
*   `option`\- Mac
*   `command`\- Mac
*   `super`\- the**Windows**key on Windows and Linux, or**⌘**on Mac
*   `primary`\-**Ctrl**on Windows and Linux, or**⌘**on Mac

#### KEY NAMES

Key names are specified either by the (non-shifted) character printed on the key, or a key name:

`a`  
`b`  
`c`  
`d`  
`e`  
`f`  
`g`  
`h`  
`i`  
`j`  
`k`  
`l`  
`m`

`n`  
`o`  
`p`  
`q`  
`r`  
`s`  
`t`  
`u`  
`v`  
`w`  
`x`  
`y`  
`z`

`0`  
`1`  
`2`  
`3`  
`4`  
`5`  
`6`  
`7`  
`8`  
`9`

`,`  
`.`  
`\`  
`/`  
`;`  
`'`  
```  
`+`  
`-`  
`=`  
`[`  
`]`  

`up`  
`down`  
`left`  
`right`  
`insert`  
`home`  
`end`  
`pageup`  
`pagedown`  
`backspace`  
`delete`  
`tab`  
`enter`  
`pause`  
`escape`  
`space`

`keypad0`  
`keypad1`  
`keypad2`  
`keypad3`  
`keypad4`  
`keypad5`  
`keypad6`  
`keypad7`  
`keypad8`  
`keypad9`  
`keypad_period`  
`keypad_divide`  
`keypad_multiply`  
`keypad_minus`  
`keypad_plus`  
`keypad_enter`  
`clear`

`f1`  
`f2`  
`f3`  
`f4`  
`f5`  
`f6`  
`f7`  
`f8`  
`f9`  
`f10`  
`f11`  
`f12`  
`f13`  
`f14`  
`f15`  
`f16`  
`f17`  
`f18`  
`f19`  
`f20`

### "command"KEY

The"command"key specifies the name of the command to execute when the key press(es) are detected. The command may be a built-in command, or a command implemented by a plugin.

~~~
{
    "keys": ["ctrl+a"],
    "command": "select_all"
}
~~~

*Currently there is no compiled list of all built-in commands. The names of many commands can be found by looking at theDefault ({PLATFORM\_NAME}).sublime-keymapfiles in theDefault/package.*

### "args"KEY

The arguments to send to the"command"key may be specified by a JSON object under the"args"key.

~~~
{
    "keys": ["primary+shift+b"],
    "command": "build",
    "args": {"select": true}
}
~~~

### "context"KEY

To allow for key bindings that react differently based on the situation, the"context"key allows specifying one or more conditions that must evaluate to true for the key binding to be active.

The"context"value is an array of objects. Each object must contain a"key"key that has a string value. A key is one of a predefined list of values that may be compared using an"operator"and"operand".

If a"key"support*equailty*operators, the"operator"must be one of:"equal"or"not\_equal". For*matching*it must be one of:"regex\_match","not\_regex\_match","regex\_contains"or"not\_regex\_contains". The default"operator"is"equal"and the default"operand"is`true`.

For"key"values that deal with selections, an additional key"match\_all"is supported. This defaults to`false`, which means the condition only needs to evaluate to true for a single selection. If"match\_all"is`true`, then the condition must evaluate to true for all selections.

The following is a list of valid context"key"values:

| Key | Operand | Operators | Match All | Description |
| --- | --- | --- | --- | --- |
| "auto\_complete\_visible" | boolean | equality | no | If the auto complete dropdown is visible |
| "eol\_selector" | string | equality | yes | A[selector](https://www.sublimetext.com/docs/selectors.html)to match the scope name at the end of the current line |
| "following\_text" | string | matching | yes | The text after the selection |
| "has\_next\_field" | boolean | equality | no | If the selection is a field in a snippet where a subsequent field exists |
| "has\_prev\_field" | boolean | equality | no | If the selection is a field in a snippet where a previous field exists |
| "is\_recording\_macro" | boolean | equality | no | If the user is currently recording a macro |
| "last\_command" | string | equality | no | The name of the last command that was run |
| "last\_modifying\_command" | string | equality | no | The name of the last command run that modified a buffer |
| "num\_selections" | integer | equality | no | The number of selection in the current buffer |
| "popup\_visible" | boolean | equality | no | If a popup is currently being displayed |
| "preceding\_text" | string  
 | matching | yes | The text before the selection |
| "read\_only" | boolean | equality | no | If the buffer is marked read only |
| "selection\_empty" | boolean  
 | equality | yes | If the current selection does not contain any characters |
| "selector" | string  
 | equality | yes | A[selector](https://www.sublimetext.com/docs/selectors.html)to match the scope name of the selection |
| "text" | string  
 | matching | yes | The text of the selection |
| "panel" | string | equality | no | The name of the current panel |
| "panel\_visible" | boolean | equality | no | If a panel is visible |
| "panel\_has\_focus" | boolean | equality | no | If a panel is visible and has focus |
| "overlay\_visible" | boolean | equality | no | If the quick panel is visible |

## User Bindings

Users can customize their key bindings by creating a file namedDefault.sublime-keymapin theirPackages/User/directory.

For example, the following will create a key binding to show unsaved changes, if any exist, via**Ctrl***+***Shift***+***`**.

~~~
[
    {
        "keys": ["ctrl+shift+`"],
        "command": "diff_changes"
    }
]
~~~
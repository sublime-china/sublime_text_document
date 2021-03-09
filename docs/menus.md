# 

[DOCUMENTATION](index)[TOC](menus#toc)[TOP](menus#)

Menus

Menus in Sublime Text are defined by files ending in.sublime-menu. Menus use JSON, with the top-level structure being an array. Each binding is a JSON object containing information to define the text of the menu entry and what action it should perform.

*   [Example](menus#example)
*   [Entries](menus#entries)
*   [Available Menus](menus#available_menus)
*   [Adding to Submenus](menus#adding_to_submenus)
*   [Customization](menus#customization)

## Example

*The following is an example of the format of a.sublime-menufile.*

~~~
[
    {
        "caption": "File",
        "mnemonic": "F",
        "id": "file",
        "children":
        [
            { "command": "new_file", "caption": "New File", "mnemonic": "N" },

            { "command": "prompt_open_file", "caption": "Open File…", "mnemonic": "O", "platform": "!OSX" },
            { "command": "prompt_open_folder", "caption": "Open Folder…", "platform": "!OSX" },
            { "command": "prompt_open", "caption": "Open…", "platform": "OSX" }
        ]
    }
]
~~~

## Entries

Each menu entry is a JSON object with one or more keys. The list of supported keys includes:

"caption"

The text of the menu entry

"mnemonic"

The character to use as the key to press to activate the entry. Only applies to Windows and Linux. Must match the case of the character in the"caption".

"command"

A string of the command to execute when the entry is activated

"args"

A JSON object of args to send to the command

"children"

A JSON array of entries to create a submenu from

"id"

A unique string for the menu entry. Used for menu entries with"children"to allow adding additional child entries.

"platform"

One of the strings:`"OSX"`,`"!OSX"`,`"Windows"`,`"!Windows"`,`"Linux"`or`"!Linux"`. Controls what platforms the entry is shown on.

Each menu entry requires at minimum, the key`"caption"`for a non-functionality entry, or`"command"`for an entry that performs an action.

## Available Menus

Sublime Text has seven menus that may be customized:

Main.sublime-menu

Primary menu for the application

Side Bar Mount Point.sublime-menu

Context menu for top-level folders in the side bar

Side Bar.sublime-menu

Context menu for files and folders in the side bar. Has "magic" args for passing file and folder names to commands.

Entries with an arg`"files": []`will be enabled for files and will pass file names to the command via the arg`files`. Entries with an arg`"dirs": []`will be enabled for folders and will pass file names to the command via the arg`dirs`. Entries with an arg`"paths": []`will be enabled for files and folders and will pass file and folder names to the command via the arg`paths`.

Tab Context.sublime-menu

Context menu for file tabs

Context.sublime-menu

Context menu for text areas

Find in Files.sublime-menu

Menu shown when clicking the`...`button in Find in Files panel

Widget Context.sublime-menu

Context menu for text inputs in various panels.*Technically this file name can be changed via thecontext\_menusetting inside*ofWidget.sublime-settings.

## Adding to Submenus

Using the"id"key of an entry, it is possible to add entries to submenus. When adding submenu entries, specify only the"id"and"children"keys of the parent entry, and set the value of"children"to an array of entries to append to the submenu.

For example, to add a new layout to theView![▶](https://www.sublimetext.com/images/right.svg)Layoutmenu, create an entry such as:

~~~
[
    {
        "id": "layout",
        "children":
        [
            {
                "caption": "Grid: 9",
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
        ]
    }
]
~~~

To find the"id"of entries in theMain.sublime-menu, use theView Package Filecommand from the command palette and selectDefault/Main.sublime-menu.

## Customization

Users can customize the available menus by creating an appropriately-named file in theirPackages/User/directory.

For example, to customize the context menu for files and folders in the side bar, create a file namedPackages/User/Side Bar.sublime-menu. Adding the following would create an entry that would execute a (hypothetical) command that would copy the path to the clipboard.

~~~
[
    {
        "caption": "Copy Full Path",
        "mnemonic": "y",
        "command": "copy_path",
        "args": {"paths": []}
    }
]
~~~
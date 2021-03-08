# 

[DOCUMENTATION](index)[TOC](indentation#toc)[TOP](indentation#)

Indentation Settings

Indentation settings determine the size of the tab stops, and control whether the**Tab**key should insert tabs or spaces. In addition to the automatic detection, they can be customized globally, per-syntax type, or per-file.

*   [Basic Settings](indentation#basic_settings)
*   [Indentation Detection](indentation#indentation_detection)
*   [Converting Between Tabs and Spaces](indentation#converting_tabs_spaces)
*   [Automatic Indentation Settings](indentation#automatic_indentation_settings)

## Basic Settings

tab\_sizeinteger

The number of spaces a tab is considered equal to

translate\_tabs\_to\_spacesboolean

If`true`, spaces will be inserted up to the next tab stop when**Tab**is pressed, rather than inserting a tab character

detect\_indentationboolean

If`true`, the default,tab\_sizeandtranslate\_tabs\_to\_spaceswill be calculated automatically when loading a file

use\_tab\_stopsboolean

Iftranslate\_tabs\_to\_spacesis`true`,use\_tab\_stopswill make**Tab**insert, and**Backspace**delete, up to the next tab stop

*See[Settings](settings)for information about how to set global and syntax-specific settings.*

## Indentation Detection

When a file is loaded, its contents are examined, and thetab\_sizeandtranslate\_tabs\_to\_spacessettings are set for that file. The status area will report when this happens. While this generally works well, you may want to disable it. You can do that with thedetect\_indentationsetting.

Indentation detection can be run manually via theView![▶](images/right.svg)Indentation![▶](images/right.svg)Guess Settings From Buffermenu, which runs thedetect\_indentationcommand.

## Converting Between Tabs and Spaces

TheView![▶](images/right.svg)Indentationmenu has commands to convert leading whitespace in the current file between tabs and spaces. These menu items run theexpand\_tabsandunexpand\_tabscommands.

## Automatic Indentation Settings

Automatic indentation guesses the amount of leading whitespace to insert on each line when you press enter. It's controlled with these settings:

auto\_indentboolean

Enables auto indent

Default:`true`

smart\_indentboolean

Makes auto indent a little smarter, e.g., by indenting the next line after an if statement in C.

Default:`true`

trim\_automatic\_white\_spaceboolean

Trims white space added byauto\_indentwhen moving the caret off the line.

Default:`true`

indent\_to\_bracketboolean

Adds whitespace up to the first open bracket when indenting. Use when indenting like this:

~~~
use_indent_to_bracket(to_indent,
                      like_this);

~~~

Default:`false`
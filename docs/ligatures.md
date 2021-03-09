[DOCUMENTATION](index.html)

Ligatures

Sublime Text supports ligatures for symbols since version 3.1. Thefont\_optionssetting can be used to customize ligature behavior.

## Usage

In the default configuration, Sublime Text will use ligatures for sequences of symbols within the ASCII range. The ligatures must be part of the`clig`,`liga`or`calt`OpenType tables of the font for them to be used. Additionally, the sequence of characters that compromise the ligature must all be part of the same token, as defined by the syntax being used to display the file.

To use ligatures from the`dlig`OpenType table, thefont\_optionssetting must have the string`"dlig"`added. Thefont\_optionssetting also allows for disabling ligatures from the`clig`,`liga`or`calt`tables by adding the respective`"no_clig"`,`"no_liga"`or`"no_calt"`strings.

## Troubleshooting

If ligatures are not displaying, please check the following:

*   Check to see what unicode characters make up the ligature. Sublime Text currently only supports ligatures comprised from the following characters:`!`,`"`,`#`,`$`,`%`,`&`,`'`,`(`,`)`,`*`,`+`,`,`,`-`,`.`,`/`,`:`,`;`,`<`,`=`,`>`,`?`,`@`,`[`,`\`,`]`,`^`,`_`,```,`{`,`|`,`}`,`~`.
*   Make sure Sublime Text build 3146 or newer is installed. If trying to usefont\_optionsto control ligatures, ensure build 3158 or newer is installed.
*   See if the ligatures appear when using the "Plain Text" syntax for the file. If so, the syntax is likely breaking the symbols into distinct tokens, preventing a ligature from being used.
*   If using Sublime Text on Linux, check and see what version of Pango is installed. Pango 1.38, released in 2015, is required to specify thefont\_optionsto control liagures.
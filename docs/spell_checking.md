# 

[DOCUMENTATION](index)[TOC](spell_checking#toc)[TOP](spell_checking#)

Spell Checking

Sublime Text uses[Hunspell](http://hunspell.sourceforge.net/)for its spell checking support. Additional dictionaries can be obtained from the[OpenOffice.org Extension List](http://extensions.services.openoffice.org/en/dictionaries).

Dictionaries in a format ready to be used by Sublime Text are available at[https://github.com/titoBouzout/Dictionaries](https://github.com/titoBouzout/Dictionaries).

*   [Dictionaries](spell_checking#dictionaries)
*   [Settings](spell_checking#settings)
*   [Commands](spell_checking#commands)

## Dictionaries

Sublime Text currently only supports UTF-8 encoded dictionaries. Most dictionaries are not encoded in UTF-8, instead using a more compact encoding for the language in question. To use a dictionary with Sublime Text, it'll first need to be converted into UTF-8.

Once you have a UTF-8 encoded dictionary, it can be installed by placing it in a package, for example,Packages/User/, which you can access from thePreferences![▶](images/right.svg)Browse Packagesmenu item. Once the file is in place, you can select the dictionary from theView![▶](images/right.svg)Dictionarymenu.

## Settings

There are two[settings](settings)that effect the spell checking:spell\_check, which controls if spell checking is enabled, anddictionary, which gives the path to the dictionary file to use. For example:

spell\_checkboolean

If spell checking is enabled

Default:`false`

dictionarystring

The package-relative path to the dictionary file

Default:`"Packages/Language - English/en_US.dic"`

added\_wordsarray of strings

An array of properly spelled words to augment the dictionary with

Example:`["unobscurable"]`

ignored\_wordsarray of strings

An array of words to ignore when checking spelling

Example:`["revelationary"]`

## Commands

*   next\_misspelling: Select the next misspelling
*   prev\_misspelling: Select the previous misspelling
*   add\_word: Adds the word given by thewordargument to the add list
*   ignore\_word: Adds the word given by thewordargument to the ignore list
# 

[DOCUMENTATION](index)[TOC](indexing#toc)[TOP](indexing#)

Indexing

Version:  
[Dev](indexing#ver-dev)[3.2](indexing#ver-3.2)[3.1](indexing#ver-3.1)[3.0](indexing#ver-3.0)

Sublime Text includes an indexing engine that scans all of the files and folders in a window/project and uses that information to provide the ability to jump to definitionsand provide context-aware completions4.0.

*   [Features](indexing#features)
    *   [Goto Definition](indexing#goto_definition)
    *   [Context-Aware Completions](indexing#context-aware_completions)4.0
*   [Status](indexing#status)
*   [Settings](indexing#settings)

## Features

### GOTO DEFINITION

When scanning the files in a project, the indexing engine records a list of every symbol and its location. Each syntax has the ability to define what is classified as a symbol, but typically functions, methods, classes and other data types are indexed. In addition to recording the location of definitions, the indexer records references – calls or invocations of known symbols.

The index of symbols can be accessed via:

*   Hovering over a word to show the*Goto Definition Popup*
*   Invoking*Goto Symbol in Project*to fuzzy-search through symbols
    *   **Windows/Linux:****Ctrl***+***Shift***+***R**
    *   **Mac:****⌘***+***Shift***+***R**
*   Executing*Goto Definition*for the word under the caret
    *   **All OSes:****F12**
*   Executing*Goto Reference*for the word under the caret
    *   **All OSes:****Shift***+***F12**

All of the*Goto*commands can also be invoked via theGotomenu.

### CONTEXT-AWARE COMPLETIONS4.0

In addition to providing information about symbols, the index is used to provide context-aware completions. The indexer makes a list of all words present in the project, along with information about sequences of words and any trailing punctuation.

When completions are shown, the index is queried to provide intelligent suggestions. Without the index, Sublime Text will only suggest matching words from the current file. With the index, it provides completions from all files, uses the preceeeding words to help suggest better matches and will suggest trailing punctuation when appropriate.

## Status

The current status and activity of the indexing engine can be seen via theHelp![▶](images/right.svg)Indexing Status…menu entry. This will show a window with the current status, progress bar and log of indexing messages.

When the indexing engine is active, the status bar will contain a text label with a percentage. This percentage indicates indexing is active, and how far along in the process it is. Clicking the percentage will open the*Indexing Status*window.

## Settings

The indexing engine uses low-priority background processes to load and analyse the files in the project. Depending on the machine and available resources, it may be desirable to modify the configuration to ensure the processes don't interfere with other usage of the machine.

index\_filesboolean

If the indexing engine is enabled

Default:`true`

index\_workersinteger

The number of background processes to use. A value of`0`causes Sublime Text to automatically pick the number of porcesses based on the number of CPU cores.

Default:`0`

index\_exclude\_gitignore4.0boolean

If files ignored via.gitignoreare excluded from indexing

Default:`true`

index\_skip\_unknown\_extensions4.0boolean

If files with unknown extensions are excluded from indexing

Default:`true`

index\_exclude\_patternsarray of strings

[File patterns](file_patterns)used to exclude files from indexing

Default:`["*.log"]`

show\_definitionsboolean

If the*Goto Definition*popup will appear when hovering the mouse over a word that has been indexed

Default:`true`

auto\_complete\_use\_index4.0boolean

If completions should utilize information from the index

Default:`true`
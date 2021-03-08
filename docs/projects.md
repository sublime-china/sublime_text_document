# 

[DOCUMENTATION](index)[TOC](projects#toc)[TOP](projects#)

Projects

Version:  
[Dev](projects#ver-dev)[3.2](projects#ver-3.2)[3.1](projects#ver-3.1)[3.0](projects#ver-3.0)

Projects in Sublime Text are made up of two files: the.sublime-projectfile, which contains the project definition, and the.sublime-workspacefile, which contains user specific data, such as the open files and the modifications to each.

As a general rule, the.sublime-projectfile would be checked into version control, while the.sublime-workspacefile would not.

*   [Project Format](projects#project_format)
*   ["folders"Key](projects#folders)
*   ["settings"Key](projects#settings)
*   ["build\_systems"Key](projects#build_systems)

## Project Format

.sublime-projectfiles are JSON, and support three top level sections:"folders", for the included folders,"settings", for file-setting overrides, and"build\_systems", for project specific build systems. An example:

~~~
{
    "folders":
    [
        {
            "path": "src",
            "folder_exclude_patterns": ["backup"],
            "follow_symlinks": true
        },
        {
            "path": "docs",
            "name": "Documentation",
            "file_exclude_patterns": ["*.css"]
        }
    ],
    "settings":
    {
        "tab_size": 8
    },
    "build_systems":
    [
        {
            "name": "List",
            "shell_cmd": "ls -l"
        }
    ]
}

~~~

## "folders"Key

The"folders"key contains an array of objects. Each object must have a"path"key, which may be relative to the project directory, or a fully qualified path.

Additional optional keys include:

"name"string

A name used in place of the folder name in the side bar

"file\_include\_patterns"array of strings

[Patterns](file_patterns)of files to include from the folder. Anything not matching these patterns will be excluded. This is checked before"file\_exclude\_patterns".

"file\_exclude\_patterns"array of strings

[Patterns](file_patterns)of files to exclude from the folder. This is added to the global setting of the same name. This is checked after"file\_include\_patterns".

"folder\_include\_patterns"array of strings

[Patterns](file_patterns)of folders to include from the folder. Anything not matching these patterns will be excluded. This is checked before"folder\_exclude\_patterns".

"folder\_exclude\_patterns"array of strings

[Patterns](file_patterns)of folders to exclude from the folder. This is added to the global setting of the same name. This is checked after"folder\_include\_patterns".

"binary\_file\_patterns"3.1array of strings

[Patterns](file_patterns)of files to treat as binary files, and thus ignored inGoto AnythingandFind in Files.

"index\_include\_patterns"3.1array of strings

[Patterns](file_patterns)of files to index in the folder. This is added to the global setting of the same name. Anything not matching these patterns will be excluded from the index. This is checked before"index\_exclude\_patterns".

"index\_exclude\_patterns"3.1

[Patterns](file_patterns)of files to excluding from indexing in the folder. This is added to the global setting of the same name. This is checked after"index\_include\_patterns".

"follow\_symlinks"boolean

If symlinks should be followed when building the folder tree.

Converted projects from earlier versions may have a"mount\_points"entry under"folders". If you wish to use the exclude patterns, you'll need to change to the above format.

## "settings"Key

[Settings](settings)may be specified here using the"settings"key, and will override regular user settings. They will not, however, override syntax-specific settings.

*Note that only settings in the category[Editor Settings](settings#categories)may be controlled by a project.*

## "build\_systems"Key

"build\_systems"specifies a list of inline[Build Systems](build_systems)definitions. In addition to the regular build system settings, a"name"must be specified for each one. Build systems listed here will be available via the regularTools![▶](images/right.svg)Build Systemsmenu.
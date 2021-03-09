# 

[DOCUMENTATION](index)[TOC](file_patterns#toc)[TOP](file_patterns#)

File Patterns

Version:  
[Dev](file_patterns#ver-dev)[3.2](file_patterns#ver-3.2)[3.1](file_patterns#ver-3.1)[3.0](file_patterns#ver-3.0)

Various features in Sublime Text use file patterns. These patterns are used to match against file/directory names and paths. They are similar in functionality to shell glob patterns, but have some unique behavior.

*   [Basic Syntax](file_patterns#syntax)
*   [Path Rules](file_patterns#path_rules)
*   [Uses](file_patterns#uses)

## Basic Syntax

File patterns allow two matching operators:

*   **`*`:**matches zero or more characters, except`/`
*   **`?`:**matches exactly one character, except`/`

*Neither character classes,`[abc]`, nor the globstar operator,`**`, from Bash are supported.*

### EXAMPLES

*   The pattern`abc`will match`abc`but not`abcd`
*   The pattern`a?c`will match`abc`but not`ac`
*   The pattern`a*c`will match`abc`,`ac`and`abdc`

## Path Rules

When`/`*is not present*in a pattern, it is only compared against the file or directory name and only the basic syntax applies. When a`/`*is included*in a pattern, it changes the behavior to:

*   The pattern is matched against the entire file or directory path
*   In a`*/`prefix or`/*`suffix, the`*`will match`/`characters
*   `*/`will be implicitly prefixed if the pattern does not start with a`/`or`*`
*   `*`will be implicitly suffixed if the pattern does not start with a`/`or`*`<4.0
*   If the pattern ends in`/`it will be treated as a directory pattern, and will matchboth a directory with that name and4.0any contained files or subdirectories
*   If a pattern begins with a single`/`, it will be compared as an absolute path
*   If pattern begins with`//`, it will be compared as a relative path from the project root4.0

### EXAMPLES

*   The pattern`mydir/one`will match`/parent/mydir/one`,`/mydir/one`and`/mydir/one/sub`
*   The pattern`mydir/two`will match`/parent/mydir/two`and`/parent/mydir/two_sub`
*   The pattern`/mydir/three`will match`/mydir/three`but not`/nested/mydir/three`
*   The pattern`mydir/three/`will match`/parent/mydir/three/sub`but not`/parent/mydir/three`
*   The pattern`//mydir/five`will match`/project1/mydir/five`and`/project2/mydir/five`but not`/project1/nested/mydir/five`4.0

## Uses

File patterns are used in:

*   *Find in Files*panel*Where*input
*   Various settings
    *   folder\_exclude\_patterns:*[Settings](settings)*and*[Project Folders](projects#folders)*
    *   folder\_include\_patterns:*[Project Folders](projects#folders)*
    *   file\_exclude\_patterns:*[Settings](settings)*and*[Project Folders](projects#folders)*
    *   file\_include\_patterns:*[Project Folders](projects#folders)*
    *   binary\_file\_patterns:*[Settings](settings)*and*[Project Folders](projects#folders)*
    *   index\_exclude\_patterns:*[Settings](settings)*and*[Project Folders](projects#folders)*
    *   index\_include\_patterns:*[Settings](settings)*and*[Project Folders](projects#folders)*
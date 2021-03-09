# 

[DOCUMENTATION](docs/index)[TOC](docs/selectors#toc)[TOP](docs/selectors#)

Selectors

Syntax definitions in Sublime Text use of scope names to provide metadata about tokens. Scopes are dotted strings, specified from least-to-most specific. For example, the`if`keyword in PHP could be specified via the scope name`keyword.control.php`. Tokens may have one or more scope names associated with them. Multiple scope names are associated with a token in an ordered manner.

This document covers selectors, which are the means to match scope names. Color schemes, key bindings, the API and even some settings all deal with selectors in one way or another. For information about standardized scope names, please see the[Scope Naming documentation](docs/scope_naming).

*   [Basic Matching](docs/selectors#basic_matching)
*   [Logical Operators](docs/selectors#logical_operators)

## Basic Matching

A basic selector specifies one or more scope names, and is matched against a token‘s scope names starting with the left-most scope. For a selector to match a token‘s scope name, all of its labels must be present in the same order.

| Scope Name | Selector | Matches |
| --- | --- | --- |
| keyword.control.php | keyword | yes |
| keyword.control.php | keyword.control | yes |
| keyword.control.php | control | no,`control`!=`keyword` |
| keyword.control.php | keyword.cont | no,`cont`!=`control` |
| keyword.control.php | keyword.control.php.embedded | no,`embedded`could not be matched |

When a selector has multiple scope names, each must match one of the token‘s scope names, in order.

| Scope Name | Selector | Matches |
| --- | --- | --- |
| source.php meta.block.php keyword.control.php | keyword | yes |
| source.php meta.block.php keyword.control.php | meta keyword | yes |
| source.php meta.block.php keyword.control.php | keyword meta | no,`meta`could not be matched after`keyword` |

## Logical Operators

In addition to matching scope names based of label prefix matches, selectors may also specify logical operators.

### LOGICAL OR

The logical OR operator is`|`or`,`. If either the selector to the right or left of the operator is matched, the expression will be a match.

| Scope Name | Selector | Matches |
| --- | --- | --- |
| source.php meta.block.php | text | meta | yes |
| source.php | text, meta | no |

### LOGICAL AND

The logical AND operator is`&`. It will require the selector to the right and left of the operator are both matched for the expression to be a match. This is different than a space between selectors, since that denoted hierarchy.

| Scope Name | Selector | Matches |
| --- | --- | --- |
| source.php meta.block.php keyword.control.php | keyword & meta | yes |
| source.php meta.block.php | keyword & meta | no |

### LOGICAL NOT

The logical NOT operator is`-`. It will require the selector to the right to not match for the expression to be a match.

| Scope Name | Selector | Matches |
| --- | --- | --- |
| source.php meta.block.php | source - keyword | yes |
| source.php meta.block.php keyword.control.php | source - keyword | no |

### GROUPING

When working with logical operators, parentheses may be used to group selectors.

| Scope Name | Selector | Matches |
| --- | --- | --- |
| source.php meta.block.php | source - (keyword | storage) | yes |
| source.php meta.block.php | (source - source.php) | text | no |
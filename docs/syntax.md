# 

[DOCUMENTATION](index)

Syntax Definitions

Version:  
[Dev](syntax#ver-dev)[3.2](syntax#ver-3.2)[3.1](syntax#ver-3.1)[3.0](syntax#ver-3.0)

Sublime Text can use both.sublime-syntaxand.tmLanguagefiles for syntax highlighting. This document describes.sublime-syntaxfiles.

*   [Overview](syntax#overview)
*   [Header](syntax#header)
*   [Contexts](syntax#contexts)
    *   [Meta Patterns](syntax#meta_patterns)
    *   [Match Patterns](syntax#match_patterns)
    *   [Include Patterns](syntax#include_patterns)
    *   [Prototype Context](syntax#prototype_context)
*   [Including Other Files](syntax#include-syntax)
*   [Variables](syntax#variables)
*   [Inheritance](syntax#inheritance)4075
*   [Selected Examples](syntax#examples)
*   [Testing](syntax#testing)
*   [Compatibility](syntax#compatibility)4075

## Overview

Sublime Syntax files are[YAML](http://yaml.org/)files with a small header, followed by a list of contexts. Each context has a list of patterns that describe how to highlight text in that context, and how to change the current text.

Here's a small example of a syntax file designed to highlight C.

~~~
%YAML 1.2
---
name: C
file_extensions: [c, h]
scope: source.c

contexts:
  main:
    - match: \b(if|else|for|while)\b
      scope: keyword.control.c

~~~

At its core, a syntax definition assigns scopes (e.g.,`keyword.control.c`) to areas of the text. These scopes are used by color schemes to highlight the text.

This syntax file contains one context,`main`, that matches the words`[if, else, for, while]`, and assigns them the scope`keyword.control.c`. The context name`main`special: every syntax must define a main context, as it will be used at the start of the file.

Thematchkey is a regex, supporting features from the[Oniguruma regex engine](https://raw.githubusercontent.com/kkos/oniguruma/v6.9.1/doc/RE). In the above example,`\b`is used to ensure only word boundaries are matched, to ensure that words such as`elsewhere`are not considered keywords.

Note that due to the YAML syntax, tab characters are not allowed within.sublime-syntaxfiles.

## Header

The allowed keys in the header area are:

name

This defines the name shown for the syntax in the menu. It's optional, and will be derived from the file name if not used.

file\_extensions

A list of strings, defining file extensions this syntax should be used for.*Extensions listed here will be shown in file dialog dropdowns on some operating systems.*

If a file does not have a basename, e.g..gitignore, the entirety of the filename including the leading`.`should be specified.

hidden\_file\_extensions4075

A list of strings, also defining file extensions this syntax should be used for. These extensions are not listed in file dialogs.

first\_line\_match

When a file is opened without a recognized extension, the first line of the file contents will be tested against this regex, to see if the syntax should be applied.

scope

The default scope assigned to all text in the file

version4075

An integer, either`1`or`2`, that controls[backwards compatibility](syntax#compatibility).*New syntaxes should target`2`, as it fixes some inconsistencies in how scopes are applied.*

extends4075

A string of a base syntax this syntax should inherit from. The base syntax must be specified using its package path, e.g.Packages/JavaScript/JavaScript.sublime-syntax. See[Inheritance](syntax#inheritance)for an overview of syntax inheritance.

hidden

Hidden syntax definitions won't be shown in the menu, but can still be assigned by plugins, or included by other syntax definitions.

## Contexts

For most languages, you'll need more than one context. For example, in C, we don't want a`for`word in the middle of a string to be highlighted as a keyword. Here's an example of how to handle this:

~~~
%YAML 1.2
---
name: C
file_extensions: [c, h]
scope: source.c

contexts:
  main:
    - match: \b(if|else|for|while)\b
      scope: keyword.control.c
    - match: '"'
      push: string

  string:
    - meta_scope: string.quoted.double.c
    - match: \\.
      scope: constant.character.escape.c
    - match: '"'
      pop: true

~~~

A second pattern has been added to the main context that matches a double quote character (note that`'"'`is used for this, as a standalone quote would be a YAML syntax error), and pushes a new context,`string`, onto the context stack. This means the rest of the file will be processing using the string context, and not the main context, until the string context is popped off the stack.

The string context introduces a new key:meta\_scope. This will assign the`string.quoted.double.c`scope to all text while the`string`context is on the stack.

While editing in Sublime Text, you can check what scopes have been applied to the text under the caret by pressing**control***+***shift***+***p**(Mac) or**ctrl***+***alt***+***shift***+***p**(Windows/Linux).

The`string`context has two patterns: the first matches a backslash character followed by any other, and the second matches a quote character. Note that the last pattern specifies an action when an unescaped quote is encountered, the string context will be popped off the context stack, returning to assigning scopes using the main context.

When a context has multiple patterns, the leftmost one will be found. When multiple patterns match at the same position, the first defined pattern will be selected.

### META PATTERNS

meta\_scope

This assigns the given scope to all text within this context, including the patterns that push the context onto the stack and pop it off.

meta\_content\_scope

As above, but does not apply to the text that triggers the context (e.g., in the above string example, the content scope would not get applied to the quote characters).

meta\_include\_prototype

Used to stop the current context from automatically including the`prototype`context.

clear\_scopes

This setting allows removing scope names from the current stack. It can be an integer, or the value`true`to remove all scope names. It is applied beforemeta\_scopeandmeta\_content\_scope. This is typically only used when one syntax is embedding another.

meta\_prepend4075

A boolean, controlling context name conflict resolution during[inheritance](syntax#inheritance). If this is specified, the rules in this context will be inserted before any existing rules from a context with the same name in an ancestor syntax definition.

meta\_append4075

A boolean, controlling context name conflict resolution during[inheritance](syntax#inheritance). If this is specified, the rules in this context will be inserted after to any existing rules from a context with the same name in an ancestor syntax definition.

Meta patterns must be listed first in the context, before any match or include patterns.

### MATCH PATTERNS

A*match*pattern can include the following keys:

match

The[regex](https://raw.githubusercontent.com/kkos/oniguruma/5.9.6/doc/RE)used to match against the text. YAML allows many strings to be written without quotes, which can help make the regex clearer, but it's important to understand when you need to quote the regex. If your regex includes the characters`#`,`:`,`-`,`{`,`[`or`>`then you likely need to quote it. Regexes are only ever run against a single line of text at a time.

scope

The scope assigned to the matched text.

captures

A mapping of numbers to scope, assigning scopes to captured portions of the match regex. See below for an example.

push

The contexts to push onto the stack. This may be either a single context name, a list of context names, or an inline, anonymous context.

pop

Pops contexts off the stack. The value`true`will pop a single context.

An integer greater than zero will pop the corresponding number of contexts.4050

Thepopkey can be combined withpush,set,embedandbranch. When combined, the specified number of contexts will be popped off of the stack before the other action is performed. Forpush,embedandbranchactions, the pop treats the match as if it were a lookahead, which means the match will not receive themeta\_scopeof the contexts that are popped.4075

set

Accepts the same arguments as push, but will first pop this context off, and then push the given context(s) onto the stack.

Any match will receive themeta\_scopeof the context being popped*and*the context being pushed.

embed3153

Accepts the name of a single context to push into. While similar topush, it pops out of any number of nested contexts as soon as theescapepattern is found. This makes it an ideal tool for embedding one syntax within another.

escape

This key is required ifembedis used, and is a regex used to exit from the embedded context. Any backreferences in this pattern will refer to capture groups in thematchregex.

embed\_scope

A scope assigned to all text matched after thematchand before theescape. Similar in concept tometa\_content\_scope.

escape\_captures

A mapping of capture groups to scope names, for theescapepattern. Use capture group`0`to apply a scope to the entire escape match.

branch4050

Accepts the names of two or more contexts, which are attempted in order. If afailaction is encountered, the highlighting of the file will be restarted at the character where thebranchoccured, and the next context will be attempted.

branch\_point

This is the unique identifier for thebranchand is specified when a match uses thefailaction.

Thebranchaction allows for handling syntax constructs that are ambiguous, and also allows handling constructs that span multiple lines.

For ideal performance, the contexts should be listed in the order of how likely they are to be accepted.*Note: because highlighting with branches requires reprocessing an entire branch upon each change to the document, the highlighting engine will not rewind more than 128 lines when afailoccurs.*

fail4050

Accepts the name of abranch\_pointto rewind to and retry the next context of. If afailaction specifies abranch\_pointthat was never pushed on the stack, or has already been popped off of the stack, it will have no effect.

The following keys control behavior that is exclusive, and only one can be specified per match pattern:

*   push
*   pop
*   set
*   embed3153
*   branch4050
*   fail4050

#### MATCH EXAMPLES

A basic match assigning a single scope to the entire match:

~~~
- match: \w+
  scope: variable.parameter.c++

~~~

Assigning different scopes to the regex capture groups:

~~~
- match: ^\\s*(#)\\s*\\b(include)\\b
  captures:
    1: meta.preprocessor.c++
    2: keyword.control.include.c++

~~~

Pushing into another context named`function-parameters`:

~~~
- match: \b\w+(?=\()
  scope: entity.name.function.c++
  push: function-parameters

~~~

Popping out of a context:

~~~
- match: \)
  scope: punctuation.section.parens.end.c++
  pop: true

~~~

Popping out of the current context and pushing into another:

~~~
- match: \}
  scope: punctuation.section.block.end.c++
  set: file-global

~~~

Embedding another syntax3153:

~~~
- match: (```)(js|javascript)
  captures:
    1: punctuation.section.code.begin.markdown
    2: constant.other.markdown
  embed: scope:source.js
  embed_scope: meta.embedded.js.markdown
  escape: ^```
  escape_captures:
    0: punctuation.section.code.end.markdown

~~~

Usingbranchto attempt one highlighting, with the ability to fallback to another.4050:

~~~
expression:
  - match: (?=\()
    branch_point: open_parens
    branch:
      - paren_group
      - arrow_function

paren_group:
  - match: \(
    scope: punctuation.section.parens.begin.js
    push:
      - include: expressions
      - match: \)
        scope: punctuation.section.parens.begin.js
        set:
          - match: =>
            fail: open_parens
          - match: (?=\S)
            pop: 2

arrow_function:
  - match: \(
    scope: punctuation.section.parens.begin.js
    push:
      - match: \w+
        scope: variable.parameter.js
      - match: ','
        scope: punctuation.separator.comma.js
      - match: \)
        scope: punctuation.section.parens.begin.js
        set:
          - match: =>
            scope: storage.type.function.arrow.js
            push: arrow_function_body


~~~

Usingpopwith another action.4075:

~~~
paragraph:
  - match: '(```)(py|python)'
    captures:
      1: punctuation.definition.code.begin.md
      2: constant.other.language-name.md
    pop: 1
    embed: scope:source.python
    embed_scope: source.python.embedded
    escape: ^```
    escape_captures:
      0: punctuation.definition.code.end.md

~~~

### INCLUDE PATTERNS

Frequently it's convenient to include the contents of one context within another. For example, you may define several different contexts for parsing the C language, and almost all of them can include comments. Rather than copying the relevant match patterns into each of these contexts, you can include them:

~~~
expr:
  - include: comments
  - match: \b[0-9]+\b
    scope: constant.numeric.c
  ...

~~~

Here, all the match patterns and include patterns defined in the comments context will be pulled in. They'll be inserted at the position of the include pattern, so you can still control the pattern order. Any meta patterns defined in the comments context will be ignored.

#### INCLUDING AN EXTERNAL PROTOTYPE4075

When including a context from another syntax, it may be desirable to also include any applicable prototype from that syntax. By default, an include pattern does*not*include such a prototype. If the key/value pair`apply_prototype:true`is added to the include pattern, the context does not specify`meta_include_prototype:false`and the other syntax has a`prototype`context, then those patterns will also be included.

~~~
tags:
  - include: scope:source.basic
    apply_prototype: true

~~~

### PROTOTYPE CONTEXT

With elements such as comments, it's so common to include them that it's simpler to make them included automatically in every context, and just list the exceptions instead. You can do this by creating a context named`prototype`, it will be included automatically at the top of every other context, unless the context contains themeta\_include\_prototypekey. For example:

~~~
prototype:
  - include: comments

string:
  - meta_include_prototype: false
  ...

~~~

In C, a`/*`inside a string does not start a comment, so the string context indicates that the prototype should not be included.

## Including Other Files

Sublime Syntax files support the notion of one syntax definition including another. For example, HTML can contain embedded JavaScript. Here's an example of a basic syntax defintion for HTML that does so:

~~~
scope: text

contexts:
  main:
    - match: <script>
      push: Packages/JavaScript/JavaScript.sublime-syntax
      with_prototype:
        - match: (?=</script>)
          pop: true
    - match: "<"
      scope: punctuation.definition.tag.begin
    - match: ">"
      scope: punctuation.definition.tag.end

~~~

Note the first rule above. It indicates that when we encounter a`<script>`tag, the main context withinJavaScript.sublime-syntaxshould be pushed onto the context stack. It also defines another key,with\_prototype. This contains a list of patterns that will be inserted into every context defined withinJavaScript.sublime-syntax. Note thatwith\_prototypeis conceptually similar to the`prototype`context, however it will be always be inserted into every referenced context irrespective of theirmeta\_include\_prototypekey.

In this case, the pattern that's inserted will pop off the current context while the next text is a`</script>`tag. Note that it doesn't actually match the`</script>`tag, it's just using a lookahead assertion, which plays two key roles here: It both allows the HTML rules to match against the end tag, highlighting it as-per normal, and it will ensure that all the JavaScript contexts will get popped off. The context stack may be in the middle of a JavaScript string, for example, but when the`</script>`is encountered, both the JavaScript string and main contexts will get popped off.

Note that while Sublime Text supports both.sublime-syntaxand.tmLanguagefiles, it's not possible to include a.tmLanguagefile within a.sublime-syntaxone.

Another common scenario is a templating language including HTML. Here's an example of that, this time with a subset of[Jinja](http://jinja.pocoo.org/):

~~~
scope: text.jinja
contexts:
  main:
    - match: ""
      push: "Packages/HTML/HTML.sublime-syntax"
      with_prototype:
        - match: "{{"
          push: expr

  expr:
    - match: "}}"
      pop: true
    - match: \b(if|else)\b
      scope: keyword.control

~~~

This is quite different from the HTML-embedding-JavaScript example, because templating languages tend to operate from the inside out: by default, it needs to act as HTML, only escaping to the underlying templating language on certain expressions.

In the example above, we can see it operates in HTML mode by default: the main context includes a single pattern that always matches, consuming no text, just including the HTML syntax.

Where the HTML syntax is included, the Jinja syntax directives (`{{ ... }}`) are included via thewith\_prototypekey, and thus get injected into every context in the HTML syntax (and JavaScript, by transitivity).

## Variables

It's not uncommon for several regexes to have parts in common. To avoid repetitious typing, you can use variables:

~~~
variables:
  ident: '[A-Za-z_][A-Za-z_0-9]*'
contexts:
  main:
    - match: '\b{{ident}}\b'
      scope: keyword.control

~~~

Variables must be defined at the top level of the.sublime-syntaxfile, and are referenced within regxes via`{{varname}}`. Variables may themselves include other variables. Note that any text that doesn't match`{{[A-Za-z0-9_]+}}`won't be considered as a variable, so regexes can still include literal`{{`characers, for example.

## Inheritance4075

In situations where a syntax is a slight variant of another, with some additions or changes, inheritance is a useful tool.

When inheriting a syntax, the keyextendsis used with a value containing the*packages path*to the parent syntax. The*packages path*will start withPackages/and will contain the package name and syntax filename. For example:

~~~
%YAML 1.2
---
name: C++
file_extensions: [cc, cpp]
scope: source.c++
extends: Packages/C++/C.sublime-syntax

~~~

A syntax using inheritance will inherit thevariablesandcontextsvalues from its parent syntax. All other top-level keys, such asfile\_extensionsandscopewill*not*be inherited.

### VARIABLES

When extending a syntax, thevariableskey is merged with the parent syntax. Variables with the same name will override previous values.

Variable substitution is performed after all variable values have been realized. Thus, an extending syntax may change a variable from a parent syntax, and all usage of the variable in the parent contexts will use the overridden value.

### CONTEXTS

The contexts in an extending syntax will be a combination of the contexts from the parent syntax, and all those defined under thecontextskey.

Contexts with the same name will override contexts from the parent syntax. To change the behavior when a context name is duplicated, two options are available. These meta key must be specified in the extending syntax:

*   `-meta_prepend:true`  
    all of the patterns in the extending syntax will be inserted before those in the parent syntax.
*   `-meta_append:true`  
    all of the patterns in the extending syntax will be inserted after those in the parent syntax.

### MULTIPLE INHERITANCE4086

When a syntax is derived from a combination of two other syntaxes, multiple inheritance may be used. This allows the keyextendsto be a list of packages paths to two or more parent syntaxes. The parent syntaxes will be processed in order, from top to bottom, and must be derived from the same base.

Two examples of multiple inheritance in the default syntaxes are:

*   **Objective-C++**: extends*C++*and*Objective-C*, both which extend*C*
*   **TSX**: extends*JSX*and*TypeScript*, both which extend*JavaScript*

### LIMITATIONS

A syntax may extend a syntax that itself extends another syntax. There are no enforced limits on extending, other than that all syntaxes must share the same[version](syntax#compatibility).

## Selected Examples

### BRACKET BALANCING

This example highlights closing brackets without a corresponding open bracket:

~~~
name: C
scope: source.c

contexts:
  main:
    - match: \(
      push: brackets
    - match: \)
      scope: invalid.illegal.stray-bracket-end

  brackets:
    - match: \)
      pop: true
    - include: main

~~~

### SEQUENTIAL CONTEXTS

This example will highlight a C style for statement containing too many semicolons:

~~~
for_stmt:
  - match: \(
    set: for_stmt_expr1
for_stmt_expr1:
  - match: ";"
    set: for_stmt_expr2
  - match: \)
    pop: true
  - include: expr
for_stmt_expr2:
  - match: ";"
    set: for_stmt_expr3
  - match: \)
    pop: true
  - include: expr
for_stmt_expr3:
  - match: \)
    pop: true
  - match: ";"
    scope: invalid.illegal.stray-semi-colon
  - include: expr

~~~

### ADVANCED STACK USAGE

In C, symbols are often defined with the`typedef`keyword. So that*Goto Definition*can pick these up, the symbols should have the`entity.name.type`scope attached to them.

Doing this can be a little tricky, as while typedefs are sometimes simple, they can get quite complex:

~~~
typedef int coordinate_t;

typedef struct
{
    int x;
    int y;
} point_t;

~~~

To recognise these, after matching the typedef keyword, two contexts will be pushed onto the stack: the first will recognise a typename, and then pop off, while the second will recognise the introduced name for the type:

~~~
main:
  - match: \btypedef\b
    scope: keyword.control.c
    set: [typedef_after_typename, typename]

typename:
  - match: \bstruct\b
    set:
      - match: "{"
        set:
          - match: "}"
            pop: true
  - match: \b[A-Za-z_][A-Za-z_0-9]*\b
    pop: true

typedef_after_typename:
  - match: \b[A-Za-z_][A-Za-z_0-9]*\b
    scope: entity.name.type
    pop: true

~~~

In the above example,`typename`is a reusable context, that will read in a typename and pop itself off the stack when it's done. It can be used in any context where a type needs to be consumed, such as within a typedef, or as a function argument.

The`main`context uses a match pattern that pushes two contexts on the stack, with the rightmost context in the list becoming the topmost context on the stack. Once the`typename`context has popped itself off, the`typedef_after_typename`context will be at the top of the stack.

Also note above the use of anonymous contexts for brevity within the`typename`context.

### PHP HEREDOCS

This example shows how to match against[Heredocs](http://php.net/language.types.string#language.types.string.syntax.heredoc)in PHP. The match pattern in the main context captures the heredoc identifier, and the corresponding pop pattern in the heredoc context refers to this captured text with the`\1`symbol:

~~~
name: PHP
scope: source.php

contexts:
  main:
    - match: <<<([A-Za-z][A-Za-z0-9_]*)
      push: heredoc

  heredoc:
    - meta_scope: string.unquoted.heredoc
    - match: ^\1;
        pop: true

~~~

## Testing

When building a syntax definition, rather than manually checking scopes with theshow\_scope\_namecommand, you can define a syntax test file that will do the checking for you:

~~~
// SYNTAX TEST "Packages/C/C.sublime-syntax"
#pragma once
// <- source.c meta.preprocessor.c++
 // <- keyword.control.import

// foo
// ^ source.c comment.line
// <- punctuation.definition.comment

/* foo */
// ^ source.c comment.block
// <- punctuation.definition.comment.begin
//     ^ punctuation.definition.comment.end

#include "stdio.h"
// <- meta.preprocessor.include.c++
//       ^ meta string punctuation.definition.string.begin
//               ^ meta string punctuation.definition.string.end
int square(int x)
// <- storage.type
//  ^ meta.function entity.name.function
//         ^ storage.type
{
    return x * x;
//  ^^^^^^ keyword.control
}

"Hello, World! // not a comment";
// ^ string.quoted.double
//                  ^ string.quoted.double - comment

~~~

To make one, follow these rules

1.  Ensure the file name starts withsyntax\_test\_.
2.  Ensure the file is saved somewhere within the Packages directory: next to the corresponding .sublime-syntax file is a good choice.
3.  Ensure the first line of the file starts with:`<comment_token> SYNTAX TEST "<syntax_file>"`. Note that the syntax file can either be a.sublime-syntaxor.tmLanguagefile.

Once the above conditions are met, running thebuildcommand with a syntax test or syntax definition file selected will run all the Syntax Tests, and show the results in an output panel.*Next Result*(F4) can be used to navigate to the first failing test.

Each test in the syntax test file must first start the comment token (established on the first line, it doesn't actually have to be a comment according to the syntax), and then either a`^`or`<-`token.

The two types of tests are:

*   Caret:`^`this will test the following selector against the scope on the most recent non-test line. It will test it at the same column the`^`is in. Consecutive`^`s will test each column against the selector.
*   Arrow:`<-`this will test the following selector against the scope on the most recent non-test line. It will test it at the same column as the comment character is in.

## Compatibility

When the syntax highlighting engine of Sublime Text requires changes that will break existing syntaxes, these modifications or bug fixes are gated behind theversionkey.

Currently there exist two versions:**1**and**2**. The absense of theversionkey indicates version 1.

### VERSION 1

The following is a list of bugs and behavior preserved in version 1 that have been fixed or changed in version 2. This list is primarily useful when understanding what to look for when updating the version of syntax.

*   **`embed_scope`Stacks with`scope`of Other Syntax**  
    
    *Description*  
    When embedding a the`main`context from another syntax, theembed\_scopewill be combined with thescopeof the other syntax. In version 2 syntaxes, thescopeof the other syntax will only be included ifembed\_scopeis not specified.
    
    *Syntax 1*
    
    ~~~
    scope: source.lang
    contexts:
      paragraph:
        - match: \(
          scope: punctuation.section.group.begin
          embed: scope:source.other
          embed_scope: source.other.embedded
          escape: \)
          escape_captures:
            0: punctuation.section.group.end
    
    ~~~
    
    *Syntax 2*
    
    ~~~
    scope: source.other
    contexts:
      main:
        - match: '[a-zA-Z0-9_]+'
          scope: identifier
    
    ~~~
    
    *Text*
    
    ~~~
    'abc'
    ~~~
    
    *Result*  
    The text`abc`will get the scope`source.other.embedded source.other identifier`in version 1 syntaxes. In version 2 syntaxes, it will get`source.other.embedded identifier`.
    
*   **Match Pattern with`set`and`meta_content_scope`**  
    
    *Description*  
    When performing asetaction on a match, the matched text will get themeta\_content\_scopeof the context being popped, even thoughpopactions don’t, and asetis the equivalent of apopthenpush.
    
    *Syntax*
    
    ~~~
    scope: source.lang
    contexts:
      function:
        - meta_content_scope: meta.function
        - match: '[a-zA-Z0-9_]+'
          scope: variable.function
        - match: \(
          scope: punctuation.section.group.begin
          set: function-params
    
      function-params:
        - meta_scope: meta.function.params
        - match: \)
          scope: punctuation.section.group.end
          pop: true
    
    ~~~
    
    *Text*
    
    ~~~
    abc()
    ~~~
    
    *Result*  
    The text`(`should get the scope`meta.function.params punctuation.section.group.begin`. Instead it gets the incorrect scope`meta.function meta.function.params punctuation.section.group.begin`.
    
*   **Match Pattern with`set`and Target with`clear_scopes`**  
    
    *Description*  
    If asetaction has a target with aclear\_scopesvalue, scopes will not be cleared properly.
    
    *Syntax*
    
    ~~~
    scope: source.lang
    contexts:
      main:
        - match: \bdef\b
          scope: keyword
          push:
            - function
            - function-name
    
      function:
        - meta_scope: meta.function
    
      function-name:
        - match: '[a-zA-Z0-9_]+'
          scope: variable.function
        - match: \(
          scope: punctuation.section.group.begin
          set: function-params
    
      function-params:
        - meta_scope: meta.function.params
        - clear_scopes: 1
        - match: \)
          scope: punctuation.section.group.end
          pop: 2
    
    ~~~
    
    *Text*
    
    ~~~
    def abc()
    ~~~
    
    *Result*  
    The text`(`should get the scope`meta.function.params punctuation.section.group.begin`. Instead it gets the incorrect scope`meta.function meta.function.params punctuation.section.group.begin`.
    
*   **Embed Escape Match and Meta Scopes**  
    
    *Description*  
    The text matched by theescapepattern of anembedaction will not get themeta\_scopeormeta\_content\_scopeof the context that contains it.
    
    *Syntax*
    
    ~~~
    scope: source.lang
    contexts:
      context1:
        - meta_scope: meta.group
        - meta_content_scope: meta.content
        - match: \'
          scope: punctuation.begin
          embed: embed
          escape: \'
          escape_captures:
            0: punctuation.end
    
      embed:
        - match: '[a-z]+'
          scope: word
    
    ~~~
    
    *Text*
    
    ~~~
    'abc'
    ~~~
    
    *Result*  
    The second`'`should get the scope`meta.group meta.content punctuation.end`. Instead it gets the incorrect scope`punctuation.end`.
    
*   **Multiple Target Push Actions with`clear_scopes`**  
    
    *Description*  
    If multiple contexts are pushed at once, and more than one context specifiesclear\_scopeswith a value greater than`1`, the resulting scopes are incorrect.
    
    *Syntax*
    
    ~~~
    scope: source.lang
    contexts:
      main:
        - meta_content_scope: meta.main
        - match: '[a-zA-Z0-9]+\b'
          scope: identifier
          push:
            - context2
            - context3
    
      context2:
        - meta_scope: meta.ctx2
        - clear_scopes: 1
    
      context3:
        - meta_scope: meta.ctx3
        - clear_scopes: 1
        - match: \n
          pop: true
    
    ~~~
    
    *Text*
    
    ~~~
    abc 1
    ~~~
    
    *Result*  
    Theclear\_scopesvalues of all target contexts are added up and applied before applying themeta\_scopeandmeta\_content\_scopeof any targets. Thus, the text`abc`will be scoped`meta.ctx2 meta.ctx3 identifier`, instead of the correct scope of`source.lang meta.ctx3 identifier`.
    
*   **Regex Capture Group Order**  
    
    *Description*  
    If an lower-numbered capture group matches text that occurs after text matched by a higher-numbered capture group, the lower-numbered capture group will not have its capture scope applied.
    
    *Syntax*
    
    ~~~
    scope: source.lang
    contexts:
      main:
        - match: '(?:(x)|(y))+'
          captures:
            1: identifier.x
            2: identifier.y
    
    ~~~
    
    *Text*
    
    ~~~
    yx
    ~~~
    
    *Result*  
    The text`y`is matched by capture group 2, and the text`x`is matched by capture group 1.`x`will not get scoped`indentifier.x`since it occurs after the match from capture group 2.
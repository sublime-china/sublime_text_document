# 

[DOCUMENTATION](index)[TOC](build_systems#toc)[TOP](build_systems#)

Build Systems

Sublime Text providesbuild systemsto allow users to run external programs. Examples of common uses for build systems include: compiling, transpiling, linting, and executing tests.

Build systems are specified via JSON and saved in a file with the extension.sublime-build. A new build system can be created by theTools![▶](images/right.svg)Build System![▶](images/right.svg)New Build System…menu item or theBuild: New Build Systemcommand palette entry.

Build systems have various ways they can associate themselves with files and projects. Using this information, Sublime Text can intelligently display only viable build systems to the user. The built-in`exec`target provides common options to get up and running quickly. For more complex requirements, build systems can target custom Sublime Text commands written in Python.

*   [Basic Example](build_systems#basic_example)
*   [Usage](build_systems#usage)
*   [Options](build_systems#options)
*   [`exec`Target Options](build_systems#exec_options)
*   [Custom Options](build_systems#custom_options)
*   [Variables](build_systems#variables)
*   [Advanced Example](build_systems#advanced_example)

## Basic Example

The following is a basic example of a build system. This build system will execute the currently-open Python file.

~~~
{
    "cmd": ["python", "$file"],
    "selector": "source.python",
    "file_regex": "^\\s*File \"(...*?)\", line ([0-9]*)"
}
~~~

The[Usage](build_systems#usage)and[Options](build_systems#options)sections will discuss how to use and customize a build system.

## Usage

Build systems include the following functionality:

*   Automatic selection of a build system based on file type
*   Remembering the last used build system
*   Navigation of build system results
*   Ability to cancel a build

### RUNNING A BUILD

A build can be run by one of the following methods:

| Keyboard | Menu |
| --- | --- |
| Windows/Linux | Mac | All | Tools![▶](images/right.svg)Build |
| **Ctrl***+***B** | **⌘***+***B** | **F7** |

Output will be shown in an output panel displayed at the bottom of the Sublime Text window.

### SELECTING A BUILD SYSTEM

By default, Sublime Text uses automatic selection of build systems. When a user invokes a build, the current file's syntax and filename will be used to pick the appropriate build system.

If more than one build system matches the current file type, the user will be prompted to pick the build system they wish to use. Once a build system has been selected, Sublime Text will remember it until the user changes their selection.

To manually choose a build system, use:

| Menu |
| --- |
| Tools![▶](images/right.svg)Build System |

To change the build system, within the viable options, use one of the following methods:

| Keyboard | Menu | Command Palette |
| --- | --- | --- |
| Windows/Linux | Mac | Tools![▶](images/right.svg)Build With… | Build With: |
| **Ctrl***+***Shift***+***B** | **⇧***+***⌘***+***B** |

### NAVIGATING RESULTS

Build systems allow navigation of files specified in the build output. Typically this is used to jump to the location of errors. Navigation can be performed via:

| Command | Keyboard | Menu |
| --- | --- | --- |
| Next Result | **F4** | Tools![▶](images/right.svg)Build Results![▶](images/right.svg)Next Result |
| Previous Result | **Shift***+***F4** | Tools![▶](images/right.svg)Build Results![▶](images/right.svg)Previous Result |

### CANCELLING A BUILD

An in-process build can be cancelled via:

| Keyboard | Menu | Command Palette |
| --- | --- | --- |
| Windows/Linux | Mac | Tools![▶](images/right.svg)Cancel Build | Build: Cancel |
| **Ctrl***+***Break** | **Ctrl***+***C** |

## Options

All build systems may use the following top-level keys in the.sublime-buildfile:

"selector"string

A[selector](selectors)to match the syntax that this build system should be enabled for.

Example:`"source.python"`

"file\_patterns"array of strings

[Patterns](file_patterns)of file names the build system should be enabled for.

Example:`["*.py"]`

"keyfiles"array of strings

File names, if present in one of the opened folders, that will cause the build system to be enabled.

Example:`["Makefile"]`

"variants"array of objects

Subsidiary build systems that will inherit the options from the top-level build system. Each variant needs to specify a"name"key, and may override or add options to the top-level build system.

Example:

~~~
[
    {
        "name": "Debug Symbols",
        "cmd": ["my_command", "-D", "$file"]
    }
]

~~~

"cancel"string, array of strings

A string command name, or an array of string options.

If a string is specified, the command specified will be used to cancel the build.

If an array of strings, the primary"target"will be called, with these options added on. This only needs to be specified when using a custom"target".

Examples:`"cancel_my_build"`or`{"kill":true}`

"target"string

The command to run when the build system is invoked. The default value ofexecallows use of the additional options specified in[exec Target Options](build_systems#exec_options).

If a value other than`"exec"`is specified, none of the options in[exec Target Options](build_systems#exec_options)will do anything.

See the[Advanced Example](build_systems#advanced_example)for a complete example.

Example:`"my_build"`

"windows"object

Options to use when the build system is being executed on a Windows machine.

Example:

~~~
{
    "cmd": ["my_command.exe", "/D", "$file"]
}

~~~

"osx"object

Options to use when the build system is being executed on a Mac machine.

Example:

~~~
{
    "cmd": ["/Applications/MyProgram.app/Contents/MacOS/my_command", "-d", "$file"]
}

~~~

"linux"object

Options to use when the build system is being executed on a Linux machine.

Example:

~~~
{
    "cmd": ["/usr/local/bin/my_command", "-d", "$file"]
}

~~~

## `exec`Target Options

The default`target`of`exec`is used by the majority of build systems. It provides the following options to control what program to execute, and how to display the results.

"cmd"array of strings

The executable to run, plus any arguments to pass to it. Shell constructs such as piping and redirection are not supported – see[shell\_cmd](build_systems#exec_option-shell_cmd).

May use[variables](build_systems#variables).

Example:`["my_command","-d","$file"]`

"shell\_cmd"string

A shell command to execute. Unlike the[cmd](build_systems#exec_option-cmd)option, this does allow piping and redirection. Will use`bash`on Mac and Linux machine, and`cmd.exe`on Windows.

May use[variables](build_systems#variables).

Example:`"my_command\"$file\"| other_command"`

"working\_dir"string

The directory to execute the[cmd](build_systems#exec_option-cmd)or[shell\_cmd](build_systems#exec_option-shell_cmd)within.

May use[variables](build_systems#variables).

Example:`"$file_path"`

"file\_regex"string

A regular expression to run on the build output to match file information. The matched file information is used to enable result navigation. The regex should capture 2, 3 or 4 groups.

The capture groups should be:

1.  filename
2.  line number
3.  column number
4.  message

Example:`"^\s*(\\S[^:]*)\\((\\d+):(\\d+)\\): ([^\\n]+)"`

"line\_regex"string

A regular expression to run on the build output to match line information. The matched file information is used to enable result navigation. The regex should capture 1, 2 or 3 groups.

The groups should capture:

1.  line number
2.  column number
3.  error message

This regular expression is only necessary when some results contain strictly a line number, line and column numbers, or line and column numbers with a message. When such a match is made, the[file\_regex](build_systems#exec_option-file_regex)option will be used to search backwards to find the appropriate file name.

Example:`"^\s*line (\\d+) col (\\d+): ([^\\n]+)"`

"encoding"string

The encoding of the build system output. Uses[Python codec names](https://docs.python.org/3.3/library/codecs#id3). Defaults to`"utf-8"`.

Example:`"iso-8859-1"`

"env"object

Environment variable values to use when running the[cmd](build_systems#exec_option-cmd)or[shell\_cmd](build_systems#exec_option-shell_cmd).

Example:

~~~
{
    "PYTHONIOENCODING": "utf-8"
}
~~~

"quiet"boolean

Reduces the amount of output about the build system invocation.

Example:`true`

"word\_wrap"boolean

Turns on word wrapping in the build system output panel.

Example:`true`

"syntax"string

The syntax file to use to highlight the build system output panel.

Example:`"Packages/JavaScript/JSON.sublime-syntax"`

## Custom Options

When implementing a command to act as a build system target, the command's keyword arguments are available via options in the.sublime-buildfile. However, certain parameter names will not work since they conflict with built-in build system functionality.

The following names will not be passed as arguments to commands. This also applies to other situations, such as options specified in the"cancel","linux","osx"and"windows"options.

*   "cancel"
*   "file\_patterns"
*   "keyfile"
*   "keyfiles"
*   "linux"
*   "osx"
*   "save\_untitled\_files"
*   "selector"
*   "target"
*   "variants"
*   "windows"

## Variables

The following variables will be expanded within any string specified in the"cmd","shell\_cmd"or"working\_dir"options.

If a literal`$`needs to be specified in one of these options, it must be escaped with a`\`. Since JSON uses backslashes for escaping also,`$`will need to be written as`\\$`.

Please note that this substitution will occur for any"target". If a custom target is used, it may implement variable expansion for additional options by using`sublime.expand_variables()`with the result from`self.window.extract_variables()`.

| Variable | Description |
| --- | --- |
| $packages | The path to thePackages/folder. |
| $platform | The platform Sublime Text is running on:`"windows"`,`"osx"`or`"linux"`. |
| $file | The full path, including folder, to the file in the active view. |
| $file\_path | The path to the folder that contains the file in the active view. |
| $file\_name | The file name (sans folder path) of the file in the active view. |
| $file\_base\_name | The file name, exluding the extension, of the file in the active view. |
| $file\_extension | The extension of the file name of the file in the active view. |
| $folder | The full path to the first folder open in the side bar. |
| $project | The full path to the current project file. |
| $project\_path | The path to the folder containing the current project file. |
| $project\_name | The file name (sans folder path) of the current project file. |
| $project\_base\_name | The file name, excluding the extension, of the current project file. |
| $project\_extension | The extension of the current project file. |

## Advanced Example

The following example shows a custom`target`command, with the ability to cancel a build and navigate results.

A`target`for a build system should be a[`sublime.WindowCommand`](api_reference#sublime_plugin.WindowCommand). This will provide the instance variable of`self.window`to allow interaction with the current project, window and active view.

*Please note that the following example is somewhat simplistic in its implementation, and it won't handle many common edge cases.*

The following Python can be saved to a file namedPackage/User/my\_example\_build.py:

~~~
import sublime
import sublime_plugin

import subprocess
import threading
import os


class MyExampleBuildCommand(sublime_plugin.WindowCommand):

    encoding = 'utf-8'
    killed = False
    proc = None
    panel = None
    panel_lock = threading.Lock()

    def is_enabled(self, lint=False, integration=False, kill=False):
        # The Cancel build option should only be available
        # when the process is still running
        if kill:
            return self.proc is not None and self.proc.poll() is None
        return True

    def run(self, lint=False, integration=False, kill=False):
        if kill:
            if self.proc:
                self.killed = True
                self.proc.terminate()
            return

        vars = self.window.extract_variables()
        working_dir = vars['file_path']

        # A lock is used to ensure only one thread is
        # touching the output panel at a time
        with self.panel_lock:
            # Creating the panel implicitly clears any previous contents
            self.panel = self.window.create_output_panel('exec')

            # Enable result navigation. The result_file_regex does
            # the primary matching, but result_line_regex is used
            # when build output includes some entries that only
            # contain line/column info beneath a previous line
            # listing the file info. The result_base_dir sets the
            # path to resolve relative file names against.
            settings = self.panel.settings()
            settings.set(
                'result_file_regex',
                r'^File "([^"]+)" line (\d+) col (\d+)'
            )
            settings.set(
                'result_line_regex',
                r'^\s+line (\d+) col (\d+)'
            )
            settings.set('result_base_dir', working_dir)

            self.window.run_command('show_panel', {'panel': 'output.exec'})

        if self.proc is not None:
            self.proc.terminate()
            self.proc = None

        args = ['my_cli']
        if lint:
            args.append('-l')
        elif integration:
            args.append('-i')
        args.append(vars['file_name'])
        self.proc = subprocess.Popen(
            args,
            stdout=subprocess.PIPE,
            stderr=subprocess.STDOUT,
            cwd=working_dir
        )
        self.killed = False

        threading.Thread(
            target=self.read_handle,
            args=(self.proc.stdout,)
        ).start()

    def read_handle(self, handle):
        chunk_size = 2 ** 13
        out = b''
        while True:
            try:
                data = os.read(handle.fileno(), chunk_size)
                # If exactly the requested number of bytes was
                # read, there may be more data, and the current
                # data may contain part of a multibyte char
                out += data
                if len(data) == chunk_size:
                    continue
                if data == b'' and out == b'':
                    raise IOError('EOF')
                # We pass out to a function to ensure the
                # timeout gets the value of out right now,
                # rather than a future (mutated) version
                self.queue_write(out.decode(self.encoding))
                if data == b'':
                    raise IOError('EOF')
                out = b''
            except (UnicodeDecodeError) as e:
                msg = 'Error decoding output using %s - %s'
                self.queue_write(msg  % (self.encoding, str(e)))
                break
            except (IOError):
                if self.killed:
                    msg = 'Cancelled'
                else:
                    msg = 'Finished'
                self.queue_write('\n[%s]' % msg)
                break

    def queue_write(self, text):
        sublime.set_timeout(lambda: self.do_write(text), 1)

    def do_write(self, text):
        with self.panel_lock:
            self.panel.run_command('append', {'characters': text})

~~~

The custom`MyExampleBuildCommand`can be configured as a build system using the following JSON saved to a file namedPackages/User/My Example Build.sublime-build:

~~~
{
    "target": "my_example_build",
    "selector": "source.mylang",
    "cancel": {"kill": true},
    "variants": [
        {
            "name": "Lint",
            "lint": true
        },
        {
            "name": "Integration Tests",
            "integration": true
        }
    ]
}
~~~
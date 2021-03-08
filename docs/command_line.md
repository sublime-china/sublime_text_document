# Command Line Interface

Version:  
[Dev](command_line#ver-dev)[3.2](command_line#ver-3.2)[3.1](command_line#ver-3.1)[3.0](command_line#ver-3.0)

Sublime Text includes a command line tool,`subl`, to work with files on the command line. This can be used to open files and projects in Sublime Text, as well working as an`EDITOR`for unix tools, such as git and subversion.

*   [Setup](command_line#setup)
    *   [Windows](command_line#windows)
    *   [Mac](command_line#mac)
    *   [Linux](command_line#linux)
*   [Usage](command_line#usage)
    *   [Configuring as`EDITOR`](command_line#editor)

## Setup

Some operating systems and installation methods will require a configuration change to make`subl`available on the`PATH`.

### WINDOWS

On Windows, the command line helper is`subl.exe`. To use this from the*Command Prompt*or*Powershell*, the Sublime Text installation folder needs to be added to the`Path`environment variable:

#### WINDOWS 10

*Show instructions for:[Windows 8](command_line#windows-8),[Windows 7](command_line#windows-7)*

*   Open the*Start Menu*and typeenviron
*   Select the item*Edit the system environment variables*
*   Click the button*Environment Variables*at the bottom of the*System Properties*dialog
*   Select, or create, the`Path`environment variable in the appropriate section:
    *   For the current user, select`Path`in the*User variables for {username}*section
    *   For all users, select`Path`in the*System variables*section
*   Click the*New*button and add an entry with the Sublime Text installation directory
    *   64bit installs are typically inC:\\Program Files\\Sublime Text\\
    *   32bit installs on a 64bit version of Windows will be inC:\\Program Files (x86)\\Sublime Text\\
    *   32bit installs on a 32bit version of Windows will be inC:\\Program Files\\Sublime Text\\

### MAC

To usesubl, the Sublime Textbinfolder needs to be added to the path. For a typical installation of Sublime Text, this will be located at/Applications/Sublime Text.app/Contents/SharedSupport/bin.

#### BASH

If using Bash, the default before macOS 10.15, the following command will add thebinfolder to the`PATH`environment variable:

~~~
echo 'export PATH="/Applications/Sublime Text.app/Contents/SharedSupport/bin:$PATH"' >> ~/.bash_profile

~~~

#### ZSH

If using Zsh, the default starting with macOS 10.15, the following command will add thebinfolder to the`PATH`environment variable:

~~~
echo 'export PATH="/Applications/Sublime Text.app/Contents/SharedSupport/bin:$PATH"' >> ~/.zprofile

~~~

### LINUX

If Sublime Text is installed via one of the[Linux Package Manager Repositories](linux_repositories)or a package, asublsymlink will automatically be installed into the/usr/bin/directory.

If installing from a tarball, thesublime\_textexecutable should be symlinked tosubl, with a command such as:

~~~
sudo ln -s /opt/sublime_text/sublime_text /usr/local/bin/subl

~~~

The exact details of the symlink command will depend on the installation location. Most default`PATH`environment variable values should contain/usr/local/bin, so no further commands should be necessary.

## Usage

To see the available flags, run`subl --help`.*The available flags will vary per operating system – the following example is from a Mac.*

~~~
Sublime Text build 3211

Usage: %s [arguments] [files]         Edit the given files
   or: %s [arguments] [directories]   Open the given directories
   or: %s [arguments] -               Edit stdin

Arguments:
  --project <project>: Load the given project
  --command <command>: Run the given command
  -n or --new-window:  Open a new window
  -a or --add:         Add folders to the current window
  -w or --wait:        Wait for the files to be closed before returning
  -b or --background:  Don't activate the application

  -s or --stay:        Keep the application activated after closing the file
  -h or --help:        Show help (this message) and exit
  -v or --version:     Show version and exit

--wait is implied if reading from stdin. Use --stay to not switch back
to the terminal when a file is closed (only relevant if waiting for a file).

Filenames may be given a :line or :line:column suffix to open at a specific location.

~~~

### CONFIGURING AS EDITOR

To use Sublime Text as the editor for many commands that prompt for input, set your`EDITOR`environment variable:

~~~
export EDITOR='subl -w'

~~~

Specifying`-w`will cause the`subl`command to not exit until the file is closed.
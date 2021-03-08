# 

[DOCUMENTATION](index)[TOC](packages#toc)[TOP](packages#)

Packages

Packages are a collection of resource files used by Sublime Text: plugins, syntax highlighting definitions, menus, snippets and more. Sublime Text ships with several packages, and more user created ones are available.

Packages are stored in.sublime-packagefiles, which are zip files with a different extension. Packages may also be stored unzipped within a directory, or a mix of the two: any loose files in the package directory will override files stored in the.sublime-packagefile.

*   [Locations](packages#locations)
*   [Special Packages](packages#special_packages)
*   [Creating a New Package](packages#creating_a_new_package)
*   [Overriding Files From a Zipped Package](packages#overriding_files_from_a_zipped_package)

## Locations

Zipped packages may be stored in:

*   **/Packages/
*   **/Installed Packages/

Loose packages may be stored in:

*   **/Packages/

For example, the packagePythonis stored in**/Packages/Python.sublime-package, and any files in the**/Packages/Python/directory will override those stored in the.sublime-packagefile.

In general,**/Packages/is for packages that ship with Sublime Text, and**/Installed Packages/is for packages installed by the user.

## Special Packages

There are two special packages:DefaultandUser.Defaultis always ordered first, andUseris always ordered last. Package ordering comes into effect when merging files between packages, for exampleMain.sublime-menu. Any package may contain a file calledMain.sublime-menu, however this won't override the main menu, instead the files will be merged according to the order of the packages.

Packages other thanDefaultandUserare ordered alphabetically.

## Creating a New Package

To create a new package, simply create a new directory under**/Installed Packages/. You can access this directory from thePreferences![▶](images/right.svg)Browse Packagesmenu.

## Overriding Files From a Zipped Package

To override a file in an existing package, just create a file with the same name under thePackages/**/directory.

For example to override the filefunction.sublime-snippetin thePython.sublime-packagepackage that ships with Sublime Text, create a directory calledPythonunder the**/Packages/directory, and place yourfunction.sublime-snippetfile there.
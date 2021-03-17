# 拼写检查

Sublime Text 使用[Hunspell](http://hunspell.sourceforge.net/)作为它的拼写检查支持。 在[OpenOffice.org Extension List](http://extensions.services.openoffice.org/en/dictionaries)可以找到额外词典。

可以被Sublime Text使用的格式的词典可在以下网址获得[https://github.com/titoBouzout/Dictionaries](https://github.com/titoBouzout/Dictionaries)。

*   [Dictionaries](spell_checking#dictionaries)
*   [Settings](spell_checking#settings)
*   [Commands](spell_checking#commands)

## Dictionaries

Sublime Text目前仅支持UTF-8编码字典。大多数字典不是用UTF-8编码的，而是对所讨论的语言使用更紧凑的编码。要使用具有崇高文本的字典，首先需要将其转换为UTF-8。

一旦您有了UTF-8编码的字典，就可以通过将其放入包 (例如，Packages/User/) 来安装，您可以从首选项访问该包
![▶](images/right.svg)浏览程序包菜单项。文件就位后，您可以从视图中选择字典![▶](images/right.svg)Dictionarymenu.

## Settings

有两个[设置](settings) 影响拼写检查:spell\_check, 控制着拼写检查的开关, 然后是 dictionary, 提供使用的字典文件路径。 示例:

spell\_check boolean

是否启用拼写检查

Default:`false`

dictionary string

指定包相对路径到字典文件

Default:`"Packages/Language - English/en_US.dic"`

added\_words array of strings

一组拼写正确的单词，用于增加字典

Example:`["unobscurable"]`

ignored\_words array of strings

检查拼写时要忽略的单词数组

Example:`["revelationary"]`

## Commands

*   next\_misspelling: 选择下一个拼写错误
*   prev\_misspelling: 选择之前的拼写错误
*   add\_word: 将单词参数给出的单词添加到添加列表中
*   ignore\_word: 将单词参数给出的单词添加到忽略列表中
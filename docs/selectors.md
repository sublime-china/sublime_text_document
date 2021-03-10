# 

[DOCUMENTATION](docs/index)[TOC](docs/selectors#toc)[TOP](docs/selectors#)

Selectors 选择器

Sublime Text中的语法定义使用范围名称提供有关令牌的元数据。范围是虚线字符串，从最小到最具体地指定。例如，可以通过范围名称 `keyword.control.php` 指定PHP中的 `if` 关键字。令牌可能有一个或多个与之关联的范围名称。多个范围名称以有序的方式与令牌关联。

本文档涵盖选择器，这是匹配范围名称的方法。配色方案、键绑定、API甚至一些设置都以这样或那样的方式处理选择器。有关标准化范围名称的信息，请参见[Scope Naming documentation](docs/scope_naming)。

*   [Basic Matching](docs/selectors#basic_matching)
*   [Logical Operators](docs/selectors#logical_operators)

## 基本匹配

基本选择器指定一个或多个范围名称，并与以最左边范围开头的令牌的范围名称匹配。为了使选择器与令牌的范围名称匹配，其所有标签必须以相同的顺序存在。

| Scope Name | Selector | Matches |
| --- | --- | --- |
| keyword.control.php | keyword | yes |
| keyword.control.php | keyword.control | yes |
| keyword.control.php | control | no,`control`!=`keyword` |
| keyword.control.php | keyword.cont | no,`cont`!=`control` |
| keyword.control.php | keyword.control.php.embedded | no,`embedded`could not be matched |

当选择器具有多个范围名称时，每个名称必须按顺序与令牌的范围名称之一匹配。

| Scope Name | Selector | Matches |
| --- | --- | --- |
| source.php meta.block.php keyword.control.php | keyword | yes |
| source.php meta.block.php keyword.control.php | meta keyword | yes |
| source.php meta.block.php keyword.control.php | keyword meta | no,`meta`could not be matched after`keyword` |

## 逻辑运算符

除了根据标签前缀匹配匹配范围名称之外，选择器还可以指定逻辑运算符。

### 逻辑 OR

逻辑OR运算符为 `|`or`,` 。如果匹配运算符右侧或左侧的选择器，则表达式将是匹配项。

| Scope Name | Selector | Matches |
| --- | --- | --- |
| source.php meta.block.php | text | meta | yes |
| source.php | text, meta | no |

### 逻辑 AND

逻辑 AND 运算符是 `&`。这将要求运算符左右两侧的选择器都匹配，以使表达式匹配。这不同于选择器之间的空格，因为它表示层次结构。

| Scope Name | Selector | Matches |
| --- | --- | --- |
| source.php meta.block.php keyword.control.php | keyword & meta | yes |
| source.php meta.block.php | keyword & meta | no |

### 逻辑 NOT

逻辑 NOT 运算符为 `-`。它将要求右侧的选择器不匹配，以使表达式匹配。

| Scope Name | Selector | Matches |
| --- | --- | --- |
| source.php meta.block.php | source - keyword | yes |
| source.php meta.block.php keyword.control.php | source - keyword | no |

### GROUPING

使用逻辑运算符时，可以使用括号对选择器进行分组。

| Scope Name | Selector | Matches |
| --- | --- | --- |
| source.php meta.block.php | source - (keyword | storage) | yes |
| source.php meta.block.php | (source - source.php) | text | no |
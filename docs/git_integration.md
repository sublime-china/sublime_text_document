# Git集成包括以下组件：

*   [侧栏](git_integration#side_bar)
*   [状态栏](git_integration#status_bar)
*   [差异标记](git_integration#diff_markers)
*   [Sublime Merge 整合](git_integration#sublime_merge)
*   [设定值](git_integration#settings)

*请注意：以下文档讨论了Git集成的实现，如Sublime Text随附的Default和Adaptive主题所示。通过主题引擎，第三方主题可以更改信息的视觉表示，在这种情况下，以下文档可能不准确。*

## 侧栏

修改后，显示在侧栏中的文件和文件夹将在右侧边缘包含状态标记。这包括侧边栏“*文件夹”*部分中的文件和文件*夹*，以及“*打开文件”*部分中的*文件*。通过减少名称的不透明度，可在侧栏中消除对忽略的文件和文件夹的强调。

当鼠标悬停在状态标记上时，将显示工具提示，指示文件的状态，如果是文件夹，则指示包含的文件和文件夹的状态。

### 状态徽章键

下表列出了每个徽章的含义。*请注意，徽章的颜色会略有不同，因为它们会适应活动配色方案中最接近的色调。*

*   未追踪
*   改性
*   失踪
*   分阶段加法
*   分阶段修改
*   分阶段删除
*   未合并

当文件夹包含具有多种状态的文件时，最靠近上述列表结尾的标志将覆盖所有其他标志。

## 状态栏

当焦点文件包含在Git仓库的工作目录中时，状态栏将包含当前分支的名称以及未跟踪，修改，暂存或未合并的文件数。状态栏元素将如下所示：

master3

## 差异标记

Sublime Text的[增量差异](incremental_diff)功能与Git集成联系在一起。默认情况下，增量差异功能会跟踪自上次保存文件以来的更改，但也可以针对HEAD进行差异。

这是使用Mariana配色方案的diff标记实际作用的示例：

2728添加了一行2930修改后的行31然后是另一条修改的行3233删除之前的那一行34

将设置git\_diff\_target更改为`"head"`将会修改diff标记，以显示diff与Git仓库HEAD上文件的版本相比，而不是工作目录中文件的版本。

有关更多信息和示例，请参阅[增量差异文档](incremental_diff)，包括有关查看内嵌差异，在块之间导航和还原更改的说明。

## Sublime Merge 整合

Sublime Text中可用的Git功能源自我们其他产品[Sublime Merge中的工作](https://www.sublimemerge.com/)。Sublime Merge是基于Sublime Text技术的功能齐全，功能强大的Git客户端。

由于编辑源代码和散文与管理Git仓库相比需要不同的工具和工作流程，因此我们选择将最合适的Git功能集成到Sublime Text中，但在Sublime Merge中保留了更高级的功能。以下集成点使您可以轻松进入相应的Git上下文：

#### 编辑器上下文菜单

*   打开Git仓库...
*   文件历史记录…
*   线路历史记录…
*   责备档案…

#### 侧栏文件夹上下文菜单

*   打开Git仓库...
*   文件夹历史记录…

#### 侧栏文件上下文菜单

*   打开Git仓库...
*   文件历史记录…
*   文件夹历史记录…
*   责备档案…

#### 命令面板

*   Sublime合并：打开仓库
*   Sublime Merge ：文件夹历史记录
*   Sublime Merge ：文件历史记录
*   Sublime Merge ：责备文件

## 设定值

Git集成可以通过show\_git\_status设置来控制。默认值`true`启用Git集成，而`false`禁用它。

可以通过git\_diff\_target设置控制Git仓库中文件的增量diff行为。有效值包括：

*   `"index"`–对比Git索引，默认
*   `"head"`–与HEAD上的文件进行比较

默认 `index`
---
title: git-clang-format 在 Windows 环境中的使用
layout: post
---

# git-clang-format 在 Windows 环境中的使用

## 背景

协作的项目是在 ARM gcc 中，开发环境中没有 `clang` 

## 环境配置

基础环境要求在 PATH 中有：

* Python 3
* git
* clang-format

### 安装 Python

下载并安装 [Python for Windows](https://www.python.org/downloads/windows) ，注意选择 Latest Python 3 Release。这里的版本是 Python 3.6.5 Windows x86-64 executable installer 。

如果无法使用 Python 3 ，则请安装 Python 2.7 以上的版本。

在安装过程中，请注意勾选 `Add Python 3.6 to PATH` 选项。

安装完成后，在命令行中应当能够使用 `python` 工具。

```
> python --version
Python 3.6.5

```

### 安装 git

下载并安装 [git for windows](https://git-scm.com/download/win) 即可。这里在安装过程中的 PATH 配置可以选择默认。

安装完成后，在命令行中应当能够使用 `git` 工具。

```
> git --version
git version 2.17.0.windows.1

```

### 安装 clang-format

在 [LLVM Download Page](http://llvm.org/builds/) 中下载 Windows 版本并安装即可。这里下载的是 Clang for Windows 32-bit 。

安装过程中需要注意选择 `Add LLVM to the system PATH for current user` 或 `Add LLVM to the system PATH for all users` 其中的一项。

安装完成后，在命令行中应当能够使用 `clang-format` 工具。

```
> clang-format -version
clang-format version 6.0.0 (tags/RELEASE_600/final)

```

clang-format 的基础功能为：

* 格式化单个文件
```
> clang-format -style=file -i /user/main.c
```

## 集成到 git 工作（可选）

目前考虑的 git 工具有：

* [git for windows](https://git-scm.com/download/win)
* [Github Desktop](https://desktop.github.com)

首先试验一下是否能用：

```
> git clang-format -h
usage: git clang-format [OPTIONS] [<commit>] [<commit>] [--] [<file>...]

If zero or one commits are given, run clang-format on all lines that differ
between the working directory and <commit>, which defaults to HEAD.  Changes are
only applied to the working directory.

If two commits are given (requires --diff), run clang-format on all lines in the
second <commit> that differ from the first <commit>.

The following git-config settings set the default of the corresponding option:
  clangFormat.binary
  clangFormat.commit
  clangFormat.extension
  clangFormat.style

positional arguments:
  <commit>              revision from which to compute the diff
  <file>...             if specified, only consider differences in these files

optional arguments:
  -h, --help            show this help message and exit
  --binary BINARY       path to clang-format
  --commit COMMIT       default commit to use if none is specified
  --diff                print a diff instead of applying the changes
  --extensions EXTENSIONS
                        comma-separated list of file extensions to format,
                        excluding the period and case-insensitive
  -f, --force           allow changes to unstaged files
  -p, --patch           select hunks interactively
  -q, --quiet           print less information
  --style STYLE         passed to clang-format
  -v, --verbose         print extra information

```

在 git 工具中的主要用法是：

* 格式化所有已 stage 的文件
```
> git clang-format
```
* 格式化整个 repo （会创建一个 commit）
```
> git clang-format --commit `git hash-object -t tree /dev/null`
```

## 集成到 IDE

目前考虑的 IDE 有：

* Visual Studio Code
* Source Insight

### 集成到 Visual Studio Code

在扩展中搜索并安装 [Clang-Format](https://marketplace.visualstudio.com/items?itemName=xaver.clang-format) 即可（LLVM 官方的扩展是另外一个 [ClangFormat](https://marketplace.visualstudio.com/items?itemName=LLVMExtensions.ClangFormat#review-details) 但是评价没有这个高）。默认的快捷键是 `Ctrl-Shift-F` 。

### 集成到 Source Insight

以 Source Insight 4.0 为例：

1. 打开 `Options` -> `Custom Commands`
2. 点击 `Add...` 按钮，具体内容为：
  * `Command` 值为 `clang-format for current file`
  * `Run` 值为 `clang-format --style=file -i %f`
  * `Dir` 留空（即项目目录，这个目录里需要有 `.clang-format` 文件）
  * 勾选 `Save Files First`
  * 不要勾选 `Pause When Done`
  * 勾选 `Wait Until Done`
3. 点击 `Menu...` 按钮，在合适的菜单中加入刚刚定义的命令(可选)
4. 点击 `Keys...` 按钮，为刚刚定义的命令分配合适的快捷键(可选)

## 格式配置

在项目的根目录，加入 `.clang-format` 文件，以定义该项目的格式。

根据目前的 FreeRTOS 的特点， `.clang-format` 文件的内容为：

```
Language: Cpp  
AccessModifierOffset: -4  
AlignEscapedNewlines: Left
AlignTrailingComments: true  
AllowAllParametersOfDeclarationOnNextLine: true  
AllowShortBlocksOnASingleLine: true  
AllowShortFunctionsOnASingleLine: Empty  
AllowShortIfStatementsOnASingleLine: true  
AllowShortLoopsOnASingleLine: false  
AlwaysBreakBeforeMultilineStrings: true  
AlwaysBreakTemplateDeclarations: true  
BinPackParameters: false  
BreakBeforeBinaryOperators: All  
BreakBeforeBraces: Allman  
BreakBeforeTernaryOperators: true  
BreakConstructorInitializers: BeforeComma
ColumnLimit: 120  
CommentPragmas: ''  
ConstructorInitializerAllOnOneLineOrOnePerLine: true  
ConstructorInitializerIndentWidth: 4  
ContinuationIndentWidth: 4  
DerivePointerAlignment: false  
DisableFormat: false  
ExperimentalAutoDetectBinPacking: false  
IndentCaseLabels: false  
IndentWidth: 4  
IndentWrappedFunctionNames: false  
IndentFunctionDeclarationAfterType: false  
MaxEmptyLinesToKeep: 1  
KeepEmptyLinesAtTheStartOfBlocks: false  
NamespaceIndentation: None
MacroBlockBegin: "^#if"
MacroBlockEnd: "^#endif"
PenaltyBreakBeforeFirstCallParameter: 1  
PenaltyBreakComment: 300  
PenaltyBreakString: 1000  
PenaltyBreakFirstLessLess: 120  
PenaltyExcessCharacter: 1  
PenaltyReturnTypeOnItsOwnLine: 1000  
PointerAlignment: Left  
SpaceAfterCStyleCast: true
SpaceBeforeAssignmentOperators: true  
SpaceBeforeParens: ControlStatements  
SpaceInEmptyParentheses: false  
SpacesBeforeTrailingComments: 1  
SpacesInAngles: false  
SpacesInCStyleCastParentheses: true  
SpacesInContainerLiterals: true  
SpacesInParentheses: true  
Cpp11BracedListStyle: true  
Standard: Cpp11  
TabWidth: 4  
UseTab: Never 

```

目前这个配置并不能完美的应用到 FreeRTOS 上，因此还有待继续调整。

可以使用 [clang-format configurator](https://zed0.co.uk/clang-format-configurator/) 来进行具体的修改。

## 参考

* [resibots/format_code](https://github.com/resibots/format_code)
* [clang-format on LLVM.org](http://clang.llvm.org/docs/ClangFormat.html)
* [clang-format configurator](https://zed0.co.uk/clang-format-configurator/)

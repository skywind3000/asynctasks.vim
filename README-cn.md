![](https://github.com/skywind3000/images/raw/master/p/asynctasks/icon-3.png)

# asynctasks.vim - 现代化的构建任务系统

为 Vim 引入类似 vscode 的 tasks 任务系统，用统一的方式系统化解决各类：编译/运行/测试/部署任务。

[![GitHub license](https://img.shields.io/github/license/Naereen/StrapDown.js.svg)](https://github.com/Naereen/StrapDown.js/blob/master/LICENSE) [![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/Naereen/StrapDown.js/graphs/commit-activity) [![Join the chat at https://gitter.im/skywind3000/asynctasks.vim](https://badges.gitter.im/skywind3000/asynctasks.vim.svg)](https://gitter.im/skywind3000/asynctasks.vim?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)


<!-- TOC -->

- [特性说明](#%E7%89%B9%E6%80%A7%E8%AF%B4%E6%98%8E)
- [快速上手](#%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8B)
    - [安装](#%E5%AE%89%E8%A3%85)
    - [单个文件的编译运行](#%E5%8D%95%E4%B8%AA%E6%96%87%E4%BB%B6%E7%9A%84%E7%BC%96%E8%AF%91%E8%BF%90%E8%A1%8C)
    - [整个项目的编译运行](#%E6%95%B4%E4%B8%AA%E9%A1%B9%E7%9B%AE%E7%9A%84%E7%BC%96%E8%AF%91%E8%BF%90%E8%A1%8C)
    - [配置优先级](#%E9%85%8D%E7%BD%AE%E4%BC%98%E5%85%88%E7%BA%A7)
    - [可用任务查询](#%E5%8F%AF%E7%94%A8%E4%BB%BB%E5%8A%A1%E6%9F%A5%E8%AF%A2)
    - [宏变量展开](#%E5%AE%8F%E5%8F%98%E9%87%8F%E5%B1%95%E5%BC%80)
    - [多种运行模式](#%E5%A4%9A%E7%A7%8D%E8%BF%90%E8%A1%8C%E6%A8%A1%E5%BC%8F)
    - [外部终端](#%E5%A4%96%E9%83%A8%E7%BB%88%E7%AB%AF)
- [高级话题](#%E9%AB%98%E7%BA%A7%E8%AF%9D%E9%A2%98)
    - [交互式任务](#%E4%BA%A4%E4%BA%92%E5%BC%8F%E4%BB%BB%E5%8A%A1)
    - [不同 profile 的任务](#%E4%B8%8D%E5%90%8C-profile-%E7%9A%84%E4%BB%BB%E5%8A%A1)
    - [命令对操作系统的适配](#%E5%91%BD%E4%BB%A4%E5%AF%B9%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%E7%9A%84%E9%80%82%E9%85%8D)
    - [内部变量](#%E5%86%85%E9%83%A8%E5%8F%98%E9%87%8F)
    - [任务数据源](#%E4%BB%BB%E5%8A%A1%E6%95%B0%E6%8D%AE%E6%BA%90)
    - [自定义运行方式](#%E8%87%AA%E5%AE%9A%E4%B9%89%E8%BF%90%E8%A1%8C%E6%96%B9%E5%BC%8F)
    - [插件设置](#%E6%8F%92%E4%BB%B6%E8%AE%BE%E7%BD%AE)
- [使用案例](#%E4%BD%BF%E7%94%A8%E6%A1%88%E4%BE%8B)
- [命令行工具](#%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%B7%A5%E5%85%B7)
- [常见问题](#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)
- [致谢](#%E8%87%B4%E8%B0%A2)

<!-- /TOC -->


<!--&nbps;-->


## 特性说明

`Vim`/`NeoVim` 近年来发展迅速，各种：异步补全/LSP/查错，DAP 等项目相继出现，就连 vimspector 这样以前只能奢望 emacs 的项目如今都出现了。

然而 Vim 任然缺少一套优雅的通用的任务系统来加速你的内部开发循环（编辑，编译，测试）。很多人在处理这些 编译/测试/部署 类任务时，任然还在使用一些比较原始的方法，所以我创建了这个插件，将 vscode 的任务系统引入 Vim。

vscode 为每个项目的根目录下新建了一个 `.vscode` 目录，里面保存了一个 `tasks.json` 来定义针对该项目的任务。而 asynctasks.vim 采用类似机制，在每个项目的根文件夹下面放一个 `.tasks` 来描述针对该项目的局部任务，同时维护一份 `~/.vim/tasks.ini` 的全局任务配置，适配一些通用性很强的项目，避免每个项目重复写 `.tasks` 配置。

说起来好像很简单？其实这是概念简单，很多好的设计从概念上来讲往往非常简单，但是用起来却十分灵活强大，这不是我设计的好，而是 vscode 的 tasks 系统设计的好，我只是大自然的搬运工，这应该是目前 Vim 下最强的构建工具，下面就试用一下：


## 快速上手

### 安装

使用 [vim-plug](https://github.com/junegunn/vim-plug) 进行安装：
```VimL
Plug 'skywind3000/asynctasks.vim'
Plug 'skywind3000/asyncrun.vim'
```

项目依赖 [asyncrun.vim](https://github.com/skywind3000/asyncrun.vim) 项目 `2.4.0` 及以上版本。记得设置：

```VimL
let g:asyncrun_open = 6
```

告诉 asyncrun 运行时自动打开高度为 6 的 quickfix 窗口，不然你看不到任何输出，除非你自己手动用 `:copen` 打开它。


### 单个文件的编译运行

我经常写一些小程序，验证一些小想法，那么在不用创建一个庞大工程的情况下，直接编译和运行单个文件就显得很有用，我们运行 `:AsyncTaskEdit` 命令，就能编辑当前项目或者当前目录的 `.tasks` 配置文件：

```ini
# 定义一个新任务
[file-build]
# 定义任务需要执行的命令，以 `$(...)` 形式出现的宏会在执行时被具体替换
command=gcc -O2 "$(VIM_FILEPATH)" -o "$(VIM_FILEDIR)/$(VIM_FILENOEXT)"
# 定义命令运行的目录
cwd=$(VIM_FILEDIR)

[file-run]
command="$(VIM_FILEDIR)/$(VIM_FILENOEXT)"
cwd=$(VIM_FILEDIR)
# 定义输出方式，在终端内运行
output=terminal
```

这里定义了两个任务：`file-build` 和 `file-run` 在包含这个 `.tasks` 配置文件的目录及其子目录下面任意一个文件，都可以用：

```VimL
:AsyncTask file-build
:AsyncTask file-run
```

两条命令来分别编译和运行他：

![](https://github.com/skywind3000/images/raw/master/p/asynctasks/demo-1.png)

上图是运行 `:AsyncTask file-build` 的效果，默认模式下（output=quickfix），命令输出会实时显示在下方的 `quickfix` 窗口中，编译错误会和 `errorformat` 匹配并显示为高亮，方便你按回车跳转到具体错误，或者用 `:cnext`/`:cprev` 命令快速跳转错误位置。

任务中有丰富的以 `$(..)` 形式出现的宏，在实际执行时会被替换成具体值。能够流畅无阻碍的执行：“编辑/编译/测试” 循环，是提高你编程效率最有效的方法，所以我们把上面两个任务绑定到 F5 和 F9：

```VimL
noremap <silent><f5> :AsyncTask file-run<cr>
noremap <silent><f9> :AsyncTask file-build<cr>
```

在你的 vimrc 中加入上面两句，就能按 F9 编译当前文件，F5 运行它了， 到这里你可能会说，这是 C/C++ 啊，如果我想运行 Python 代码怎么办呢？重新写个任务？不用那么麻烦，`command` 字段支持文件类型过滤：

```ini
[file-run]
command="$(VIM_FILEPATH)"
command:c,cpp="$(VIM_PATHNOEXT)"
command:go="$(VIM_PATHNOEXT)"
command:python=python "$(VIM_FILENAME)"
command:javascript=node "$(VIM_FILENAME)"
command:sh=sh "$(VIM_FILENAME)"
command:lua=lua "$(VIM_FILENAME)"
command:perl=perl "$(VIM_FILENAME)"
command:ruby=ruby "$(VIM_FILENAME)"
output=terminal
cwd=$(VIM_FILEDIR)
```

只需要在 `command` 字段后面加冒号，写明匹配的文件类型就行， 匹配不到的话就会使用最上面的默认命令来执行，注意文件名可能包含空格，所以要双引号，最后加了个 `-save=2` 可以在运行前保存所有改动的文件。

这样简单配置一下，你就能统一的用 F5 运行所有类型的文件了，这下你可以立马把 quickrun 这样的插件卸载掉了，它做的事情还没有上面这几行做的漂亮。接下来我们继续配置 F9 ，根据文件类型调用编译器：

```ini
[file-build]
command:c,cpp=gcc -O2 -Wall "$(VIM_FILEPATH)" -o "$(VIM_PATHNOEXT)" -lstdc++ -lm -msse3
command:go=go build -o "$(VIM_PATHNOEXT)" "$(VIM_FILEPATH)"
command:make=make -f "$(VIM_FILEPATH)"
output=quickfix
cwd=$(VIM_FILEDIR)
save=2
```

这适配了三种类型的文件，C/C++，Go，以及 Makefile，按下 F9 就可以根据当前文件类型执行对应的构建命令，并且把输出显示到 quickfix 窗口中，进行错误匹配。

上面的配置你既可以放在某个目录下，作用于所有下级目录也可以放到全局配置中，整个系统起作用。比你配置什么 `makeprg` 或者 vimscript 写一大堆乱七八糟的 if else 文件类型判断，和 `asyncrun`/`neomake` 调用优雅很多。

这里我们看到编译类项目一般配置 `output=quickfix` （默认值，不写也一样）这样可以将编译输出显示到 quickfix 窗口进行匹配，而运行类项目一般设置 `output=terminal` 选择终端模式，终端模式下有很多不同的运行方式，比如：内置终端，外置终端，quickfix模拟终端，经典 `!` 指令，tmux 分屏等，后面会说怎么指定 `output=terminal` 时的运行方式。

### 整个项目的编译运行

仅有单个文件的编译运行是不够的，大部分时候我们是工作在一个个项目中，很多 vim 插件解决单个文件编译运行还行，但是项目级别的编译运行就相形见拙了。而 `asynctasks.vim` 在这个问题上应该是同类插件中做的最好的。

解决项目编译运行首先需要定位项目目录，在 Vim 中，众多插件也早就采用了一套叫做 `rootmark` 的机制， 从当前文件所在目录一直往上递归到根目录，直到发现某一级父目录中包含下列项目标识：

```VimL
let g:asyncrun_rootmarks = ['.git', '.svn', '.root', '.project', '.hg']
```

则认为该目录是当前项目的根目录，如向上搜索到根目录都没找到任何标识，则将当前文件所在目录当作项目根目录。

如果你的项目在版本管理系统里，那么仓库的顶层文件夹就会被自动识别成项目的根目录，而如果你有一个项目既不在 `git` 中，又不在 `svn` 中怎么办？或者你的 `git`/`svn` 的单个仓库下面有很多项目，你并不想让最上层作为项目根目录的话，你只要在你想要的地方新建一个空的 `.root` 文件就行了。

最后一个边界情况，如果你没有打开文件（未命名新文件窗口），或者当前 buffer 是一个非文件（比如工具窗口），怎么办呢？此时会使用 vim 的**当前文件夹**（即 `:pwd` 返回的值）作为项目目录。

这基本是一套多年下来行之有效的约定了，众多插件都采用这个方法确定项目位置，比如大家熟知的：`YCM`，`AsyncRun`，`CtrlP`，`LeaderF`，`ccls` 和 `Gutentags` 等等。vscode 也采用类似的方法在项目顶层放置一个隐藏的 .vscode 文件夹，来标记项目根目录。

有了项目位置信息后我们就可以在任务中用 `$(VIM_ROOT)` 或者它的别名 `<root>` 来代替项目位置了：

```ini
[project-build]
command=make
# 设置在当前项目的根目录处运行 make
cwd=$(VIM_ROOT)

[project-run]
command=make run
# <root> 是 $(VIM_ROOT) 的别名，写起来容易些
cwd=<root>
output=terminal
```

我们把这两个任务分别绑定到 F6 和 F7 上面：

```VimL
noremap <silent><f6> :AsyncTask project-run<cr>
noremap <silent><f7> :AsyncTask project-build<cr>
```

那么我们就能轻松的使用 F7 来编译当前项目，而 F6 来运行当前项目了。那么也许你会问，上面定义的都是用 make 工具的来编译运行啊，我的项目不用 make 构建怎么办？项目又不能根上面单个文件那样通过单个文件类型来区分 command，难道我要把不同构建类型的项目定义很多个不同的 task，搞一大堆类似 `project-build-cmake` 和 `project-make-ninjia` ，然后在 F1-F12 上绑定满它们吗？

### 配置优先级

并不需要，最简单的做法是你可以把上面两个任务（`project-build` 和 `project-run`）配置成公共任务，放到 `~/.vim/tasks.ini` 这个公共配置里，然后对于所有一般的 make 类型项目，你就不用配置了。

而对于其他类型的项目，比如某个项目中，我还在用 `msbuild` 来构建，我就单独给这个项目的 `.tasks` 局部配置中，再定义两个名字一模一样的局部任务，比如项目 `A` 中：

```ini
[project-build]
command=vcvars32 > nul && msbuild build/StreamNet.vcxproj /property:Configuration=Debug /nologo /verbosity:quiet
cwd=<root>
errorformat=%f(%l):%m

[project-run]
command=build/Debug/StreamNet.exe
cwd=<root>
output=terminal
```

再 `asynctasks.vim` 中，局部配置的优先级高于全局配置，下层目录的配置高于上层目录的配置（`.tasks` 可以嵌套存在）。因此，在 `A` 项目中，老朋友 `project-build` 和 `project-run` 两个任务被我们替换成了针对 `A` 项目的 msbuild 的方法。

先调用 `vcvars32.bat` 初始化 `Visual C++` 环境，然后用 `&&` 符号连接 `msbuild` 命令行，并且将 `errorformat` 设置成 `%f(%l):%m` 来适配 VC++ 输出的错误信息。

这样在 `A` 这个项目中，我任然可以使用 F7 来编译项目，然后 F6 来运行整个项目，不会因为项目切换而导致我的操作发生改变，我可以用统一一致的操作，处理各种不同类型的项目，这就是本地任务和全局任务协同所能产生的奇迹。

PS：可以用 `:AsyncTaskEdit` 来编辑本地任务，`:AsyncTaskEdit!` 来编辑全局任务。

### 可用任务查询

那么当前项目下，到底有些什么可用任务呢？他们到底是局部还是全局的？一个任务到底最终是被什么配置文件给 override 掉了？我们用 `:AsyncTaskList` 命令可以查看：

![](https://github.com/skywind3000/images/raw/master/p/asynctasks/demo-list.png)

该命令能显示可用的 task 名称，具体命令，以及来自哪个配置文件。

PS：以点 `.` 开头的任务名在查询时会被隐藏，使用 `:AsyncTaskList!` 查看所有任务。

### 宏变量展开

前面任务配置里，用到了几个形状如同 `$(VIM_xxx)` 的宏，具体在运行时会具体替换成对应的值，常用的宏有：

```bash
$(VIM_FILEPATH)    # 当前 buffer 的文件名全路径
$(VIM_FILENAME)    # 当前 buffer 的文件名（没有前面的路径）
$(VIM_FILEDIR)     # 当前 buffer 的文件所在路径
$(VIM_FILEEXT)     # 当前 buffer 的扩展名
$(VIM_FILENOEXT)   # 当前 buffer 的主文件名（没有前面路径和后面扩展名）
$(VIM_PATHNOEXT)   # 带路径的主文件名（$VIM_FILEPATH 去掉扩展名）
$(VIM_CWD)         # 当前 Vim 目录（:pwd 命令返回的）
$(VIM_RELDIR)      # 相对于当前路径的文件名
$(VIM_RELNAME)     # 相对于当前路径的文件路径
$(VIM_ROOT)        # 当前 buffer 的项目根目录
$(VIM_CWORD)       # 光标下的单词
$(VIM_CFILE)       # 光标下的文件名
$(VIM_CLINE)       # 光标停留在当前文件的多少行（行号）
$(VIM_GUI)         # 是否在 GUI 下面运行？
$(VIM_VERSION)     # Vim 版本号
$(VIM_COLUMNS)     # 当前屏幕宽度
$(VIM_LINES)       # 当前屏幕高度
$(VIM_SVRNAME)     # v:servername 的值
$(VIM_DIRNAME)     # 当前文件夹目录名，比如 vim 在 ~/github/prj1/src，那就是 src
$(VIM_PRONAME)     # 当前项目目录名，比如项目根目录在 ~/github/prj1，那就是 prj1
$(VIM_INIFILE)     # 当前任务的 ini 文件名
$(VIM_INIHOME)     # 当前任务的 ini 文件的目录（方便调用一些和配置文件位置相关的脚本）
```

上面这些宏基本够你日常使用了，除了替换 `command` 和 `cwd` 配置外，同名的环境变量也被设置成同样的值，例如你某个任务命令太复杂了，你倾向于写道一个 shell 脚本中，那么 `command` 配置就可以简单的调用一下脚本文件：

```ini
[project-build]
command=build/my-build-task.sh
cwd=<root>
```

根本不用传参，这个 `my-build-task.sh` 脚本本内部直接用 `$VIM_FILENAME` 这个环境变量就能取出文件名来，这样通过环境变量传递当前项目/文件信息的方法，结合外部脚本，能让我们定义各种相对复杂的任务，比直接裸写几行 vimscript 的 keymap 强大灵活多了。

那么当前这些宏到底会被展开成什么呢？我们可以通过 `:AsyncTaskMacro` 命令查看：

![](https://github.com/skywind3000/images/raw/master/p/asynctasks/demo-macro-3.png)

左边是宏名称，中间是说明，右边是具体展开值。这条命令很有用，当你写 task 配置忘记宏名称了，用它随时查看，不用翻文档。

### 多种运行模式

配置任务时，output 字段可以设置如何运行任务，它有下面两个值：

- `quickfix`： 默认值，实时显示输出到 quickfix 窗口，并匹配 errorformat。
- `terminal`：在终端内运行任务。

第一个自然没啥好说，当设置为第二个 `terminal` 时，还可以通过一个全局变量：

```VimL
let g:asynctasks_term_pos = 'xxx'
```

来具体设置终端的工作位置和工作模式，它有几个可选值：

| 名称 | 类型 | 说明 |
|:-:|:-:|-|
| `quickfix` | 伪终端 | 默认值，使用 quickfix 窗口模拟终端，输出不匹配 `errorformat`。|
| `vim` | - | 传统 vim 的 `!` 命令运行任务，有些人就是迷恋这种方式。 |
| `tab` | 内置终端 | 在一个新的 tab 上打开内置终端，运行程序。 |
| `TAB` | 内置终端 | 同 `tab` 但是是在左边打开，关闭后方便回到上一个 tab |
| `top` | 内置终端 | 在上方打开可复用内部终端。 |
| `bottom` | 内置终端 | 在下方打开可复用内部终端。 |
| `left` | 内置终端 | 在左边打开可复用内置终端。|
| `right` | 内置终端 | 在右边打开可复用内置终端。 |
| `external` | 外部终端 | 启动一个新的操作系统的外置终端窗口，运行程序。  |

另外在任务配置文件中，也可以用 `pos=?` 来强制指定该任务需要何种方式运行。

基本上 Vim 中常见的运行模式都包含了，选择一个你喜欢的模式即可，见到那演示一下：

当 `output=terminal` 时，设置：

```VimL
let g:asynctasks_term_pos = 'bottom'
```

那么运行 :AsyncTask file-run 时，就能在下方的内置终端运行任务了：

![](https://github.com/skywind3000/images/raw/master/p/asynctasks/demo-2.png)

终端窗口会复用，如果上一个任务结束了，再次运行时不会新建终端窗口，会先尝试复用老的已结束的终端窗口，找不到才会新建。当使设置为 top/bottom/left/right 时，可以用下面两个配置确定终端窗口大小：

```VimL
let g:asynctasks_term_rows = 10    " 设置纵向切割时，高度为 10
let g:asynctasks_term_cols = 80    " 设置横向切割时，宽度为 80
```

有人说分屏的内置终端太小了，没关系，你可以设置成 `tab`：

```VimL
let g:asynctasks_term_pos = 'tab'
```

这样基本就能使用整个 vim 全屏大小的区域了：

![](https://github.com/skywind3000/images/raw/master/p/asynctasks/demo-3.png)

整个 tab 都用于运行你的任务，应该足够大了吧？这是我比较喜欢的方式。

默认的 `quickfix` 方式当然可以运行你的任务，但是它不能处理用户输入，当你的程序需要和用户交互时，你可能会需要一个真实的终端。

**Bonus**：

- tab 模式的终端也可以复用，将 `g:asynctasks_term_reuse` 设置成 `1` 即可。
- 如果你想在打开新分屏终端的时候保持你的焦点不改变，可以将 `g:asynctasks_term_focus` 设置成 `0` 即可。

（PS：内置终端有时候需要调教一下才会比较顺手，这里鼓励大家使用 `ALT+HJKL` 来进行窗口切换，淘汰老旧的 `CTRL+HJKL`，再使用 `ALT+q` 来返回终端 NORMAL 模式，这几个 keymap 我用到现在都非常顺手。）

### 外部终端

在 Windows 下经常使用 Visual Studio 的同学们一般会喜欢像 VS 一样，打开一个新的 cmd 窗口来运行程序，我们设置：

```VimL
let g:asynctasks_term_pos = 'external'
```

那么对于所有 `output=terminal` 的任务，就能使用外部系统终端了：

![](https://github.com/skywind3000/images/raw/master/p/asynctasks/demo-4.png)

是不是有点 VS 的感觉了？基本可能的运行方式都有了。

本插件基本上提供了所有 Vim 中可能的运行程序的方式了，选个你喜欢的即可。

### Runner

得益于 AsyncRun 的 [customizable runners](https://github.com/skywind3000/asyncrun.vim/wiki/Customize-Runner) 机制，任务可以按你想要的任何方式执行，插件发布包含了一批默认 runner：

| Runner | 描 述 | 需 求 | 链 接 |
|-|-|-|-|
| `gnome` | 在新的 Gnome 终端里运行 | GNOME | [gnome.vim](https://github.com/skywind3000/asyncrun.vim/blob/master/autoload/asyncrun/runner/gnome.vim) |
| `gnome_tab` | 在另一个 Gnome 终端的 Tab 里运行 | GNOME | [gnome_tab.vim](https://github.com/skywind3000/asyncrun.vim/blob/master/autoload/asyncrun/runner/gnome_tab.vim) |
| `xterm` | 在新的 xterm 窗口内运行 | xterm | [xterm.vim](https://github.com/skywind3000/asyncrun.vim/blob/master/autoload/asyncrun/runner/xterm.vim) |
| `tmux` | 在一个新的 tmux 的 pane 里运行 | [Vimux](https://github.com/preservim/vimux) | [tmux.vim](https://github.com/skywind3000/asyncrun.vim/blob/master/autoload/asyncrun/runner/tmux.vim) |
| `floaterm` | 在 floaterm 的新窗口里运行 | [floaterm](https://github.com/voldikss/vim-floaterm) | [floaterm.vim](https://github.com/skywind3000/asyncrun.vim/blob/master/autoload/asyncrun/runner/floaterm.vim) |
| `floaterm_reuse` | 再一个可复用的 floaterm 窗口内运行 | [floaterm](https://github.com/voldikss/vim-floaterm) | [floaterm_reuse.vim](https://github.com/skywind3000/asyncrun.vim/blob/master/autoload/asyncrun/runner/floaterm.vim) |
| `quickui` | 在 quickui 的浮窗里运行 | [vim-quickui](https://github.com/skywind3000/vim-quickui) | [quickui.vim](https://github.com/skywind3000/asyncrun.vim/blob/master/autoload/asyncrun/runner/quickui.vim) |
| `toggleterm` | 使用 toggleterm 窗口运行 | [toggleterm.nvim](https://github.com/akinsho/toggleterm.nvim) | [toggleterm.vim](https://github.com/skywind3000/asyncrun.vim/blob/master/autoload/asyncrun/runner/toggleterm.vim) |
| `termhelp` |在 terminal-help 的终端里运行 | [vim-terminal-help](https://github.com/skywind3000/vim-terminal-help) | [termhelp.vim](https://github.com/skywind3000/asyncrun.vim/blob/master/autoload/asyncrun/runner/termhelp.vim) |
| `xfce` | 在 xfce 终端中运行 | xfce4-terminal | [xfce.vim](https://github.com/skywind3000/asyncrun.vim/blob/master/autoload/asyncrun/runner/xfce.vim) |
| `konsole` | 在 KDE 的自带终端里运行 | KDE | [konsole.vim](https://github.com/skywind3000/asyncrun.vim/blob/master/autoload/asyncrun/runner/konsole.vim) |
| `macos` | 在 macOS 的系统终端内运行 | macos | [macos.vim](https://github.com/skywind3000/asyncrun.vim/blob/master/autoload/asyncrun/runner/macos.vim) |
| `iterm` | 在 iTerm2 的 tab 中运行 | macos + iTerm2 | [iterm.vim](https://github.com/skywind3000/asyncrun.vim/blob/master/autoload/asyncrun/runner/iterm.vim) |

当为 AsyncRun 定义了一个 runner，可以在本插件的任务配置里用 `pos` 配置来指定：

```ini
[file-run]
command=python "$(VIM_FILEPATH)"
cwd=$(VIM_FILEDIR)
output=terminal
pos=gnome
```

当你使用:

```VimL
:AsyncTask file-run
```

这个任务将会在 `gnome-terminal` 的 runner 里执行:

![](https://github.com/skywind3000/images/raw/master/p/asynctasks/runner-gnome.png)

在 gnome 下用 gvim 时，在新弹出的终端窗口里运行程序，和 IDE 里开发的体验完全一致。

如果你想避免为大部分任务设置 `pos` 配置，设置全局配置会方便很多：

```VimL
let g:asynctasks_term_pos = 'gnome'
```

全局配置生效后，任何 `output=terminal` 的任务如果没有包含 `pos` 字段，都将默认用 `gnome-terminal` 来运行任务。

注意，任务配置里的 `option` 字段必须为 `terminal`，同时任务配置里的 `pos` 会比全局配置 `g:asynctasks_term_pos` 拥有更高的优先级。

如果你想自定义一个 runner，可以参考 asyncrun 的文档：[customize-runner](https://github.com/skywind3000/asynctasks.vim/wiki/Customize-Runner)。


## 高级话题

本插件 `asynctasks.vim` 还有很多高级的玩法，我们继续：

### 交互式任务

有一些任务需要用户输入点什么东西，比如你配置一个全局搜索字符串的任务，运行时如果希望用户输入关键字的话，你就会用到这项功能。

任务的 `command` 字段可以接受形如 `$(-...)` 的宏，在运行 `:AsyncTask xxx` 时，如果 `command` 里包含这些宏，则会在 Vim 里提示你输入内容：

```ini
[task1]
command=echo hello $(-name), you are a $(-gender).
output=terminal
```

在你使用 `:AsyncTask task1` 运行任务时，该任务会在 Vim 中要求你输入参数：

![](https://github.com/skywind3000/images/raw/master/p/asynctasks/input-ask2.png)

命令行里有两个参数需要输入，问完第一个会问第二个，按 ESC 放弃，回车确认，完成后将会把输入的值替换到上面的命令中，然后开始执行：

![](https://github.com/skywind3000/images/raw/master/p/asynctasks/input-display.png)

如上图所示，该任务正确的显示了用户输入的内容。

你也可以在 `:AsyncTask {任务名}` 命令的后面用 `-varname=xxx` 的形式显示提供值：

```VimL
:AsyncTask task1 -name=Batman -gender=boy
```

当你在命令行里提供这些值了，AsyncTask 就不会再问你要输入了。

_提示：使用 `$(-prompt:default)` 可以提供一个默认值，同时 `$(-prompt:)` 会记住上次的输入。使用 `$(-gender:&male,&female)` 来给用户提供备选。_

真实案例（我自己用的）：

```ini
[grep]
command=rg -n --no-heading --color never "$(-word)" "<root>" -tcpp -tc -tpy -tvim -tgo -tasm
cwd=$(VIM_ROOT)
errorformat=%f:%l:%m
```

这是我的全局 `grep` 任务，只要运行 `:AsyncTask grep` 就会提示我输入要查找的关键字，输入后就能在当前项目中搜索符合条件的代码了。

当然，这个 `$(-word)` 的值你也可以用命令参数提供：

```VimL
:AsyncTask grep -word=hello
```

如果在另一个项目中我需要指明搜索更多类型的文件，我可以专门为该项目定义一个局部的 `grep` 任务，并用另外的参数去执行 `rg`。

当然，大部分时候，这个全局的 `grep` 任务已经足够我用了，对于其他项目，`rg` 除了支持 `.gitignore` 外，还能在项目内放一个额外的 `.ignore` 文件，来指定需要跳过什么（比如一大堆测试文件我不想搜索），或者还要搜索什么。


### 内部变量

内部变量有很多种不同的用途，比如可用来管理不同的 building target。可以在配置文件的 `[+]` 区域定义内部变量：

```ini
[+]
build_target=build_x86
test_target=test_x86

[project-build]
command=make $(+build_target)
cwd=<root>

[project-test]
command=make $(+test_target)
cwd=<root>
```

在 `command` 配置项中，任何符合 `$(+var_name)` 的文本都会被替换成星号区域定义的内容，所以，上面 test 任务的命令最终会变成： 

    make build_x86

想要切换 `project-build` 任务的 target 的话，直接打开配置修改 `[+]` 区域内的变量值就行，不用每次去修改 `command` 配置项。

同样，内部变量的值也可以通过命令行参数，以 `+varname=value` 的形式传递覆盖：

```VimL
:AsyncTask project-test  +test_target=mytest
```

Default values can be defined as $(+varname:default) form, it will be used if variables are absent in both [+] section and :AsyncTask xxx arguments.

变量可以有默认值，默认值定义为 `$(+变量名:默认值)` ：

```ini
[project-test]
command=make $(+test_target:testall)
cwd=<root>
```

如果该变量即没有在 `[+]` 区域里定义，也没有在 `:AsyncTask xxx` 的命令行后提供，那么默认值就会生效。

内部变量还有第三种定义方法，可以在 `g:asynctasks_environ` 中定义，方便 vimscript 操作：

    let g:asynctasks_environ = {'foo': '100', 'bar': '200' }

由于同样的变量可以在多处定义，那么他们的优先级是：

- 低优先级: 全局配置的 `[+]` 区间。
- 中优先级：本地 `.tasks` 配置的 `[+]` 区间。
- 高优先级：vimscript 的字典变量 `g:asynctasks_environ`。
- 最高优先级: 位于 `:AsyncTask 任务名` 命令后面的 `+varname=value` 参数。

高优先级定义的值会覆盖低优先级的内容，利用这个特性很多类似的任务可以只定义一遍。

比如我们在全局配置中定义了两个任务：

```ini
[file-build]
command=gcc -O2 -Wall "$(VIM_FILEPATH)" -o "$(VIM_PATHNOEXT)" $(+cflags:) 
cwd=$(VIM_FILEDIR)

[project-find]
command=rg -n --no-heading --color never "$(-word)" "<root>" $(+findargs:)
cwd=$(VIM_ROOT)
errorformat=%f:%l:%m
```

他们都各自引入了一个默认值为空字符的变量（cflags, findargs），如果本地项目里我们有两个同名但是参数略微不同的任务，我们不需要复制粘贴再定义一次，只需要在本地 `.tasks` 配置中：

```ini
[+]
clags=-g -gprof
findargs=-tcpp
```

这样定义一下就能获得不同的命令效果了，这个可以很方便的简化很多类似任务的定义。

### 不同 profile 的任务

单个任务允许具有多个不同的 `profile`：

```ini
[task1:release]
command=gcc -O2 "$(VIM_FILEPATH)" -o "$(VIM_PATHNOEXT)"
cwd=$(VIM_FILEDIR)

[task1:debug]
command=gcc -g "$(VIM_FILEPATH)" -o "$(VIM_PATHNOEXT)"
cwd=$(VIM_FILEDIR)
```

这里定义了 `task1` 的两个不同 profile：`release` 和 `debug`。默认的 profile 是 `debug`，可以用下面命令改为 `release`：

```VimL
:AsyncTaskProfile release
```

或者：

```VimL
let g:asynctasks_profile = 'release'
```

接着，`:AsyncTask task1` 就能用 `release` 的方式运行 `task1` 了。

附：当 `AsyncTaskProfile` 命令后跟随多个参数时:

```VimL
:AsyncTaskProfile debug release
```

会弹出一个对话框，让你选择到底是用 `debug` 还是用 `release`。

### 命令对操作系统的适配

本插件支持为不同的操作系统定义不同的命令：

```ini
[task1]
command=echo default
command/win32=echo win32 default
command/linux=echo linux default
command:c,cpp/win32=echo c/c++ for win32
command:c,cpp/linux=echo c/c++ for linux
```

命令不当可以用前面提到的文件类型来过滤，还能用操作系统来过滤，如果无法匹配那么默认命令（第一个）就会被使用。

本插件仅仅会自动检测 windows 和 linux，你可以强制设置系统类型：

```VimL
let g:asynctasks_system = 'macos'
```

这样就会匹配所有以 `/macos` 结尾的命令了。

### 任务数据源

当任务很多时，你可能需要各种 UI 插件给你提供任务选择，你可以用下面接口：

```VimL
let current_tasks = asynctasks#list("")
```

来取得所有任务信息，它会返回一个列表，每个 item 是一个任务，方便你同各种 fuzzy finder 集成。

### 自定义运行方式

如果你还想在 tmux 的 split 里或者 gnome-terminal 的 window/tab 里运行任务，以及自定义更多的运行模式，见 [customize runners](https://github.com/skywind3000/asynctasks.vim/wiki/Customize-Runner).

### 插件设置

有很多设置可以具体控制本插件的行为：

##### The `g:asynctasks_config_name` option

修改默认 `.tasks` 配置文件的名称，不喜欢的话可以随便改成：

```VimL
let g:asynctasks_config_name = '.asynctask'
let g:asynctasks_config_name = '.git/tasks.ini'
```

如果你多个本地配置文件，可以用逗号分隔不同配置名字，或者直接用列表：

```VimL
let g:asynctasks_config_name = '.tasks,.git/tasks.ini,.svn/tasks.ini'
let g:asynctasks_config_name = ['.tasks', '.git/tasks.ini', '.svn/tasks.ini']
```

##### The `g:asynctasks_rtp_config` option

修改 `~/.vim` 下面的全局配置文件 `tasks.ini` 的名称：

```VimL
let g:asynctasks_rtp_config = "asynctasks.ini"
```

##### The `g:asynctasks_extra_config` option

额外全局配置，除了 `~/.vim/tasks.ini` 外，你还可以指定更多全局配置：

```VimL
let g:asynctasks_extra_config = [
    \ '~/github/my_dotfiles/my_tasks.ini',
    \ '~/.config/tasks/local_tasks.ini',
    \ ]
```

他们会在加载完 `~/.vim/tasks.ini` 后马上加载。

##### The `g:asynctasks_term_pos` option

你想要何种命令运行 `output=terminal` 的任务，具体见 [多种运行模式](#多种运行模式).

##### The `g:asynctasks_term_cols` option

内置终端的宽度（使用水平分割时）。

##### The `g:asynctasks_term_rows` option

内置终端的高度（使用垂直分割时）。

##### The `g:asynctasks_term_focus` option

设置成 `0` 可以在使用分屏内置终端的时候，避免焦点切换。

##### The `g:asynctasks_term_reuse` option

设置成 `1` 可以复用 tab 类型的内置终端。

##### The `g:asynctasks_term_hidden` option

设置成 `1` 的话，所有内置终端的 buffer 会将 `bufhidden` 初始化成 `hide`。那么不管你全局有没有设置 `hidden`，该终端窗口都变成可以隐藏的。

##### The `g:asynctasks_template` option

设置成 `0` 的话，新建配置文件时就不使用模板了。

##### The `g:asynctasks_confirm` option

设置成 `0` 的话，使用 `:AsyncTaskEdit` 时就不需要你确认文件名了。

##### The `g:asynctasks_filetype` option

任务配置文件的 filetype，默认值是 "taskini".

## 使用案例

这里有很多实际使用案例：

- [Task Examples](https://github.com/skywind3000/asynctasks.vim/wiki/Task-Examples)

## 命令行工具

本插件提供一个名为 `asynctask.py` 的脚本 (在 `bin` 文件夹内)，可帮你在 shell 中运行任务:

```bash

# 在你项目的任意一个子目录中运行任务
# 不需要 cd 回到项目根目录，因为任务中有过 '-cwd=<root>' 的配置
$ asynctask project-build

# 编译文件
$ asynctask file-build hello.c

# 运行文件
$ asynctask file-run hello.c
```

使用 `fzf` 来选择任务:

![](https://github.com/skywind3000/images/raw/master/p/asynctasks/commandline.gif)

更多内容，请访问:

- [Command Line Tool](https://github.com/skywind3000/asynctasks.vim/wiki/Command-Line-Tool).

## 常见问题

具体见：

- [FAQ](https://github.com/skywind3000/asynctasks.vim/wiki/FAQ)

## 致谢

如果你喜欢本插件，希望能给他留下一颗星 [GitHub](https://github.com/skywind3000/asynctasks.vim)，十分感谢。欢迎关注 skywind3000 的 [Twitter](https://twitter.com/skywind3000) 和 [GitHub](https://github.com/skywind3000)。


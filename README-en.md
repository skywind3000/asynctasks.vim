![](images/icon-1.jpg)

# asynctasks.vim - a modern build/task system

An efficient way to handle building/running tasks by imitating vscode's task system.

[![GitHub license](https://img.shields.io/github/license/Naereen/StrapDown.js.svg)](https://github.com/Naereen/StrapDown.js/blob/master/LICENSE) [![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/Naereen/StrapDown.js/graphs/commit-activity) [![Join the chat at https://gitter.im/skywind3000/asynctasks.vim](https://badges.gitter.im/skywind3000/asynctasks.vim.svg)](https://gitter.im/skywind3000/asynctasks.vim?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

<!-- TOC -->

- [Introduction](#introduction)
- [Get started](#get-started)
- [Build and run a single file](#build-and-run-a-single-file)
- [Build and run a project](#build-and-run-a-project)
- [Task priority](#task-priority)
- [Query available tasks](#query-available-tasks)

<!-- /TOC -->

## Introduction

As vim 8.0 released in 2017, we have got many wonderful plugins like: LSP, DAP and  asynchronous linters. Even things like [vimspector](https://github.com/puremourning/vimspector) which could only been imagined in emacs now exist in vim's community.

But vim is still lack of an elegent system to build/run your project. A lot of people are still dealing with building/running tasks in such a primitive and flaky way. Therefor, I decide to create this plugin by introducing vscode's task like machanisms to vim. 

Vscode creates a `.vscode` folder in your project root directory and use a `.vscode/tasks.json` file to define project specific tasks. Similar, `asynctasks.vim` uses a `.tasks` file in your project folders for local tasks and use `~/.vim/tasks.ini` to define global tasks for generic projects.

This is very simple, but most good designs always start from a very simple concept. You will benefit a lot from the productivity and possibility of this task system.

## Get started

Install with `vim-plug`:

```VimL
Plug 'skywind3000/asynctasks.vim'
Plug 'skywind3000/asyncrun.vim'
```

Don't forget to initialize:

```VimL
let g:asyncrun_open = 6
```

And quickfix window can be opened automatically, otherwise you can't see the task output unless use `:copen` manually.

## Build and run a single file

It's convenient for me to build and run a single file directly without creating a new project for that if I want to try some small and new ideas. In this circumstance, we can use `:AsyncTaskEdit` command to edit the `.tasks` configuration file in your current project root directory:

```ini
[file-build]
# macros in the "$(...)" form will be expanded, 
# shell command, use quotation for filenames containing spaces
command=gcc -O2 "$(VIM_FILEPATH)" -o "$(VIM_FILEDIR)/$(VIM_FILENOEXT)"
# working directory
cwd=$(VIM_FILEDIR)

[file-run]
command="$(VIM_FILEDIR)/$(VIM_FILENOEXT)"
cwd=$(VIM_FILEDIR)
# output mode: run in a terminal
output=terminal
```

There are two tasks `file-build` and `file-run` defined in this `.tasks` file. Then from the directory where this `.tasks` reside and its child directories, you can use:

```VimL
:AsyncTask file-build
:AsyncTask file-run
```

To build and run the current file:

![](images/demo-1.png)

This is the result of `:AsyncTask file-build`, the command output displays in the quickfix window and errors are matched with `errorformat`. You can navigate the command output in the quickfix window or use `cnext`/`cprev` to jump between errors.

There are many macros can be used in the command field and will be expanded and replaced when task starts. Having a fast, low-friction Edit/Build/Test cycle is one of the best and easiest ways to increase developer productivity, so we will map them to F5 and F9:

```VimL
noremap <silent><f5> :AsyncTask file-run<cr>
noremap <silent><f9> :AsyncTask file-build<cr>
```

Put the code above in your `vimrc` and you can have F9 to compile current file and F5 to run it. And you may ask, this is for C/C++ only, what if you want to run a python script, should you create a new task `file-run-python` ? Totally unnecessary, commands can match with file types:

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
save=2
```

The `command` followed by a colon accepts file type list separated by comma. If the current file type cannot be matched, the default command will be used. The `-save=2` represents to save all modified buffers before running the task.

At this point, you can have your `F5` to run all type of files. And plugins like quickrun can be obsoleted immediately, they can't do better than this. Then we continue to polish `file-build` to support more file types:

```ini
command:c,cpp=gcc -O2 -Wall "$(VIM_FILEPATH)" -o "$(VIM_PATHNOEXT)" -lstdc++ -lm -msse3
command:go=go build -o "$(VIM_PATHNOEXT)" "$(VIM_FILEPATH)"
command:make=make -f "$(VIM_FILEPATH)"
output=quickfix
cwd=$(VIM_FILEDIR)
save=2
```

Again, F9 can be used to compile many file types, same keybind, different command. This two tasks can be defined in local `.tasks` and work for the project scope or in the `~/.vim/tasks.ini` and work for all project. Much more elegant than using the old `&makeprg` or calling `asyncrun`/`neomake` with a lot `if`/`else` in your `vimrc`.

Tasks for running compilers or grep may set `output=quickfix` (default), because the output can use errorformat to match errors in the quickfix window, while tasks for running your file/project may set `output=terminal`.

When you set `output` to `terminal`, you can further indicate what type of terminal do you want to use exactly, like: a simulated terminal in quickfix window (without matching the errorformat)? the triditional `!` command in vim? the internal terminal ? an external terminal window ? or in a tmux split window ?? The detail will be discussed later.

## Build and run a project

If you want to do something with a project, you must figure out where the project locates. `asynctasks.vim` and its backend `asyncrun.vim` choose a widely used method called `root markers` to indentify the project root directory. The project root is one of the nearest parent directory containing one of these markers:

```VimL
let g:asyncrun_rootmarks = ['.git', '.svn', '.root', '.project', '.hg']
```

If none of the parent directories contains these root markers, the directory of the current file is used as the project root. 

There is a corner case: if current buffer is not a normal file buffer (eg. a tool window) or is an unnamed new buffer, vim's current working directory (which `:pwd` returns) will be used as the project root.

Once we got the project location, the macro `$(VIM_ROOT)`, or its alias `<root>`, can be used to represent the project root:

What if your current project is not in any `git`/`subversion` repository ? How to find out where is my project root ? The solution is very simple, just put an empty `.root` file in your project root, it has been defined in `g:asyncrun_rootmarks` before.

Tasks related to projects can be defined by using this:

```ini
[project-build]
command=make
# set the working directory to the project root.
cwd=$(VIM_ROOT)

[project-run]
command=make run
# <root> is an alias to `$(VIM_ROOT)`, a little easier to type.
cwd=<root>
output=terminal
```

We assign F6 and F7 for them:

```VimL
noremap <silent><f6> :AsyncTask project-run<cr>
noremap <silent><f7> :AsyncTask project-build<cr>
```

Now, F7 can be used to build your project and F6 can be used run your project. You may ask again, this is for `gnu-make` only, but there are a lot of build tools like cmake, ninja and bazel, should you define new tasks as `project-build-cmake` or  `project-build-ninja` and assign different keymaps for them ?


## Task priority

No, you don't have to. The easiest way is to put previous `project-build` and `project-run` in your `~/.vim/tasks.ini` as the default and global tasks, you can use them directly for generic projects using `make`.

For other type of projects, for example, I am using `msbuild` in my project `A`. And I can define a new `project-build` task in the local `.tasks` file residing in project `A`:

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

The `.tasks` configuration file are read top to bottom and the most recent tasks found take precedence. and local tasks always have higher priority than the global tasks. 

Task defined in `.tasks` will always override the task with the same name in `~/.vim/tasks.ini`. So, in project `A`, our two old friends `project-build` and `project-run` have been replaced with the local methods.

Firstly, the new `project-build` task will call `vcvars32.bat` to setup environment variables, then, use a `&&` to concatenate `msbuild` command. `errorformat` is initiated to `%f(%l):%m` for matching `Visual C++` errors in this task.

We can still use `F7` to build this project and `F6` to run it. We don't have to change our habit if we are working in a different type of project. Unified workflow can be used in different type of projects. This is the power of local/global tasks combination.

## Query available tasks


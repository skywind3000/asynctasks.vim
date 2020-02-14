![](images/icon-1.jpg)

# asynctasks.vim - a modern build/task system

An efficient way to handle building/running tasks by imitating vscode's task system.

[![GitHub license](https://img.shields.io/github/license/Naereen/StrapDown.js.svg)](https://github.com/Naereen/StrapDown.js/blob/master/LICENSE) [![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/Naereen/StrapDown.js/graphs/commit-activity) [![Join the chat at https://gitter.im/skywind3000/asynctasks.vim](https://badges.gitter.im/skywind3000/asynctasks.vim.svg)](https://gitter.im/skywind3000/asynctasks.vim?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

<!-- TOC -->

- [Introduction](#introduction)
- [Get started](#get-started)
- [Build and run a single file](#build-and-run-a-single-file)

<!-- /TOC -->

## Introduction

As vim 8.0 released in 2017, we have got many wonderful plugins like: LSP, DAP and  asynchronous linters. Even things like [vimspector](https://github.com/puremourning/vimspector) which could only been imagined in emacs now exist in vim's community.

But vim is still lack of an elegent system to build/run your project. A lot of people are still dealing with building/running tasks in such a primitive way. Therefor, I decide to create this plugin by introducing vscode's task like machanisms to vim. 

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


---
comments: true
title:  "Check your writing in Neovim"
date:   2019-10-08
tags:
    - Development
    - Writing
hide:
    - navigation
excerpt: In my go-to text editor for terminal.
---

Neovim is a text editor designed to be able to use either a command-line interface or a graphical user interface.
I use it to write nearly everything (mainly Python, markdown and LaTeX files).
Many plugins are available to add handy features to Neovim (including the ones designed for Vim).
Basically, it is an enhanced Vim, see the [Neovim about page](https://neovim.io/charter/) for more details.
After writing, automatic checks are useful and help you to save some time.
In this post I share my Neovim configuration and usage.

## Spell-Checking

To do simple spell-checking, you have to add in your `~/.config/nvim/init.vim` file the following lines:

```
" spell languages
set spelllang=en
nnoremap <silent> <C-s> :set spell!<cr>
inoremap <silent> <C-s> <C-O>:set spell!<cr>
```

### Usage

In Neovim press Ctrl+S to highlight the misspelled words, then repress Ctrl+S to hide the highlighted misspelled words.
To select a word suggestion in Insert mode, press Ctrl+X then S to select a suggestion.
In Normal mode press `z=` to see the word suggestions.

!!! info "Note"
    I know that my configuration inhibits the Ctrl+S user signal and it is my point to avoid unwanted behavior while typing.

## Grammar Checking

To check my writing I use [LanguageTool](https://www.languagetool.org/) suite.
It is available for free, have an offline mode using its open-source software (it is the main reason I'm using it).
Here I will help you to install it and how to use it with Neovim.

### LanguageTool installation

LanguageTool requires java runtime.
Here I will install OpenJDK and curl:

#### On Fedora

```bash
sudo dnf -y install java curl
```

#### On Ubuntu

```bash
sudo apt -y install default-jre curl
```

#### Then, on both distributions

The following commands download LanguageTool and install it in `/usr/local`.

```bash
curl https://languagetool.org/download/LanguageTool-stable.zip > /tmp/LanguageTool-stable.zip
sudo unzip /tmp/LanguageTool-stable.zip -d /usr/local/LanguageTool
sudo mv /usr/local/LanguageTool/LanguageTool-*/* /usr/local/LanguageTool/
rm /tmp/LanguageTool-stable.zip
```

### Neovim configuration

I use [vim-plug](https://github.com/junegunn/vim-plug) plugin manager in Neovim.
It is why I use it to install the [LanguageTool plugin from vigoux](https://github.com/vigoux/LanguageTool.nvim) (but other choices are possible).
Before I continue, I recommend you to have a recent version of Neovim (&ge; 0.4.2) to use this plugin (as it use recent changes in Neovim).

In your `~/.config/nvim/init.vim` between the `call plug#begin(<plugging_folderpath>)` and `call plug#end()` you have to add the following lines:

```
" Language tool integration
Plug 'vigoux/LanguageTool.nvim'
```

Then, outside the vim-plug calls you have to add:

```
" grammar checking
autocmd Filetype markdown LanguageToolSetUp
autocmd Filetype tex LanguageToolSetUp
let g:languagetool_server_jar='/usr/local/LanguageTool/languagetool-server.jar'
```

### Usage

To use this plugin you first have to call `:LanguageToolSetUp` in Normal mode to start the LanguageTool server (which is automatically launched for tex files and markdown files).

Then, the plugin proposes the following useful commands:

* `:LanguageToolCheck`: to highlight writing mistakes, you have to recall it when you change your code/text.
* `:LanguageToolSummary`: to show details on highlighted errors in a split.
* `:LanguageToolClear`: to clear LanguageTool displays.

To see more options look at the [plugin page](https://github.com/vigoux/LanguageTool.nvim).

!!! info "Note"
    I frequently alternated between using LanguageTool and [Antidote](https://www.antidote.info/en).
    Ultimately, I chose LanguageTool because it integrates seamlessly with my coding environment and workflows.

Hope it helps some of you.
If you have a better setup or want to improve mine feel free to contribute and contact me.

See you again, Vincent.

<!-- insert
---
title: "telescope-repo.nvim"
date: 2021-08-21T10:02:37
description: "🦘 Jump into the repositories of your filesystem, without any setup"
repo_url: "https://github.com/cljoly/telescope-repo.nvim"
aliases:
- /telescope-repo.nvim
---
{{< github_badge >}}
end_insert -->
<!-- remove -->
# 🦘 telescope-repo.nvim: jump around the repositories in your filesystem, without any setup
<!-- end_remove -->

![Neovim version](https://img.shields.io/badge/Neovim-0.5-57A143?style=flat&logo=neovim)

`telescope-repo` is an extension for [telescope.nvim][] that searches the filesystem for git (or other SCM[^1]) repositories. It does not require any setup: the list of repositories is built on the fly over your whole `$HOME`, you don’t need to manually add projects or open some folders to populate this list, as opposed to [telescope-project.nvim](https://github.com/nvim-telescope/telescope-project.nvim) or [project.nvim](https://github.com/ahmedkhalf/project.nvim).

<!-- remove -->
[![Finding the repositories with “telescope” in their name, with the README in the panel on the top](https://asciinema.org/a/431528.svg)](https://asciinema.org/a/431528)
<!-- end_remove -->
<!-- insert
Finding the repositories with “telescope” in their name, with the README in the panel on the top:

{{< asciicast src="/telescope-repo-nvim/telescope.json" poster="npt:0:04" autoplay="true" loop="true" >}}
end_insert -->

Use cases include:
* If you don’t start vim from the shell (from a GUI or as the start command of a terminal), you are most likely in your `$HOME` directory. You then want to jump into your code as quickly as possible and this plugin can help!
* Sometimes, you have the definition of a function and use of it in different repositories  (e.g. a library you wrote and a program using this library). This plugin helps to open the two, for instance in two splits.
* Some similar extensions rely on strong conventions to find repositories, like “directories containing a `.git` file that is also a directory, all inside directory `X`”. This precludes support of less popular SCMs like [Pijul][], which has a different folder names, or support of [`git worktree`s][worktree], which has a `.git` *file*.

[telescope.nvim]: https://github.com/nvim-telescope/telescope.nvim
[Pijul]: https://pijul.org/
[worktree]: https://git-scm.com/docs/git-worktree
[^1]: SCM: Source-Control Management

`telescope-repo.nvim` is based on [telescope-ghq.nvim](https://github.com/nvim-telescope/telescope-ghq.nvim)

## Installation

You need to add these in your plugin management system:
```lua
'nvim-lua/plenary.nvim'
'nvim-telescope/telescope.nvim'
'cljoly/telescope-repo.nvim'
```
And optionally, to load the extension:
```lua
require'telescope'.load_extension'repo'
```

### Packer

For instance, with [Packer.nvim](https://github.com/wbthomason/packer.nvim):
```lua
use 'cljoly/telescope-repo.nvim'
use {
  'nvim-telescope/telescope.nvim',
  requires = { {'nvim-lua/plenary.nvim'} }
}
```

## External Dependencies

### Required

* [`fd`][] to find the repositories on the filesystem

[`fd`]: https://github.com/sharkdp/fd

### Optional

* [`glow`][] to preview markdown files, will fall back to [`bat`][] if not present (and uses `cat` if neither are present)

[`glow`]: https://github.com/charmbracelet/glow
[`bat`]: https://github.com/sharkdp/bat

## Usage

Run:
```
:Telescope repo list
```

### list

`:Telescope repo list`

Running `repo list` and list repositories' paths.

| key              | action               |
|------------------|----------------------|
| `<CR>` (edit)    | `builtin.git_files`  |

#### options

#### `bin`

Filepath for the binary `fd`.

```vim
" path can be expanded
:Telescope repo list bin=~/fd
```

#### `pattern`

Pattern of the SCM database folder.

Default value: `[[^\.git$]]`

#### `cwd`

Transform the result paths into relative ones with this value as the base dir.

Default value: `vim.fn.getcwd()`

#### `tail_path`

Show only basename of the path.

Default value: `false`

#### `shorten_path`

Call `pathshorten()` for each path. This will for instance transform `/home/me/code/project` to `/h/m/c/project`.

Default value: `false`

## FAQ

### Getting the repository list is slow

You can use your `.fdignore` to exclude some folders from your filesystem. If there is enough interest, [#1](https://github.com/cljoly/telescope-repo.nvim/issues/1) could further enhance this.

### How to use this plugin with Mercurial (hg), Pijul, Fossil…

Set the `pattern` option respectively to `[[^\.hg$]]`, `[[^\.pijul$]]`, `[[^\.fslckout$]]`…

```
lua require'telescope'.extensions.repo.list{pattern=[[^\.hg$]]}
```

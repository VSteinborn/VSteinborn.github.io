+++
title="Your Personal Library - How to Use Multiple Wikis With VimWiki in NeoVim"
date=2025-12-15

[taxonomies] 
tags=["HowTo"]
+++

> _Wisdom is not a product of schooling but of the lifelong attempt to acquire it._
> 
> -- **_Albert Einstein_**

# Motivation 

Every now and then in life we learn some piece of information we would like to remember.
For example, a friend might mention a fun website they recently learned about or we 
came across an interesting paper we would like to remember for a thesis.
It could be great to have some sort of **wiki** that keeps track of this useful information for us 
so it is easier to find later.
This is exactly what [VimWiki](https://github.com/vimwiki/vimwiki) attempts to do!

With VimWiki you can make your own wiki on topics that interest you.

Even more importantly:
- It's free
- There is no vendor lock-in (everything can be a [markdown](https://en.wikipedia.org/wiki/Markdown) file, which is a plain-text file)
- It's open source (people can audit the code)
- Your data remains local (important for data privacy)
- You can interact with your wiki using [vim](https://en.wikipedia.org/wiki/Vim_(text_editor)) controls (fast, but requires basic vim knowledge)
- You can have multiple wikis in different files on your computer and have access between them

The last point is exciting because you can have a personal private wiki you keep on 
your personal computer, and a public 'wiki' about topics that interest you, such as a blog or 
a way for editing documentation for work.

{{ character(name="pineapple", body="
This blog was written with VimWiki!") }}


# Prerequisites

- You need to have [NeoVim](https://neovim.io/) installed on your computer
- Some basic Vim/NeoVim knowledge is required. It is recommended to run `:Tutor<enter>` after first installing NeoVim and following the tutorial

# Installing

Note for Vim users: The VimWiki project page has a Vim installation guide.

I recommend first installing [lazy.nvim](https://lazy.folke.io/installation), which is a plugin manager for NeoVim.

Next, to install the VimWiki plugin I recommend following [this guide](https://namoku.dev/blog/how-do-i-setup-lazyvim/).
This is the best guide I could find and what ultimately helped me install VimWiki.

At the end, your `~/.config/nvim/` directory should look like this:

```
.
├── init.lua 
├── lazy-lock.json <- This file is created when lazy.nvim gets loaded
└── lua
    ├── config
    │   └── lazy.lua
    └── plugins
        └── vimwiki.lua
```

# Short Introduction to VimWiki

You can start VimWiki within Vim or NeoVim by pressing `<leader>ww`, where `<leader>` is a key you can 
set in the configuration files (via `vim.g.mapleader` in NeoVim).
I believe after going through the NeoVim `:Tutor` tutorial `vim.g.mapleader=' '`, so by default the `<leader>` key is the space bar in NeoVim.

Once in VimWiki you can link pages by writing ``[[Cool Websites from Friends]]``, for example. 
To follow the link hover over the link in normal mode and press the `enter` key.
This will take you to the `Cool Websites from Friends` page, which is just another 
file in your file system.
(The extension of the file depends on your settings. It can be a `.md` markdown file if you like.)
You can write text in the file and save the file to create it if it does not exist yet.
To go back to the first page, you can do so by pressing the `backspace` key.

{{ character(name="pineapple", body="You can also use markdown links, 
such as `[Cool Websites](./cool-websites.md)`, and 
follow the links by pressing `enter` over them in normal mode") }}

This is the most basic functionality of VimWiki, but it is quite powerful when 
coupled with Vim controls.
You can quickly travel between sites, edit them and move information between them without touching 
your mouse.
From my experience, this makes for a comfortable and efficient experience when writing notes 
in meetings for example.

# Having Multiple Wikis 

Let's say we want to have two separate Wikis:
- A personal private wiki
- A work-related wiki that will be shared with colleagues (e.g. via Dropbox)

We would like the two wikis to be separated and to live in different files.
For example, the personal wiki could be in our local `\Documents` folder and the work wiki 
can be in a shared Dropbox (which can be accessed as a folder on the computer).

To still make use of the speed of VimWiki we can configure VimWiki with two wikis.
To do this, modify the `vimwiki.lua` file:

```lua
return {
  "vimwiki/vimwiki",
  event = "BufEnter *.md",
  keys = { "<leader>ww", "<leader>wt" },
  init = function()
    vim.g.vimwiki_list = {
      {
        -- [[ Personal Wiki ]]
        name = "Personal Wiki",
        path = "/path/to/personal/wiki1/",
        syntax = "markdown",
        ext = ".md",
        index = "README"
      },
      {
        -- [[ Work Wiki ]]
        name = "Work Wiki",
        path = "/path/to/work/wiki2/",
        syntax = "markdown",
        ext = ".md",
        index = "SUMMARY"
       },
    }
    -- The following is needed to force vimwiki use markdown files
    vim.g.vimwiki_ext2syntax = {['.md'] = 'markdown', ['.markdown'] = 'markdown'} 
  end,
}
```

Now when you press `<leader>ww`, you are taken to the first wiki 
in `vimwiki_list`, which is the personal wiki.
To access other wikis press `<leader>ws`, and a menu opens with the available wikis.
With the menu open, type `2` and then press `enter` to go to the work wiki.

It is also possible to have inter-wiki links by typing `[[wn.<Wiki Name>:<Page Name In Wiki>]]`.
For example, to link to the `SUMMARY.md` index page in the `Work Wiki` from the personal wiki, just 
type ``[[wn.Work Wiki:SUMMARY]]`` on any page in the personal wiki.

Now you can quickly switch between the personal and work wikis with a few button presses all without touching the mouse!


# Learning more

There are also many other features of VimWiki that make it useful, such as searching for text in a Wiki.

If you would like to learn more about VimWiki, I can recommend the well-written documentation, which you can access via the command `:h vimwiki`.
Interestingly, using this setup I can only access the VimWiki documentation after I started 
VimWiki in NeoVim, so keep this in mind if you are having issues.

I hope this guide was helpful to you. Enjoy using VimWiki!

If you decide to cite this post, please include the following information.
```bibtex
@misc{2025-vsteinborn,
    author = {Victor Steinborn},
    title = {Your Personal Library - How to Use Multiple Wikis With VimWiki in NeoVim},
    year = {2025},
    url = {https://vsteinborn.github.io/posts/personal-wiki/}
}
```

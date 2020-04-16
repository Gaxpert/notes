---
title: Vim
date: 2020-03-29 02:48:57+01:00
author: Gaxpert
---

# Vim

List all keybinds mapped

`:map`

Which binds to which mode

`:help map-modes`


a --> Enter ins mode after current char
b --> backword
c --> Delete and insert mode
e --> end of word
f --> find char in current line
s --> substitute single char and ins mode
t --> same as f but moves to before found
u --> undo
\<Ctr> + r --> Redo
w --> move forward one word

H --> Jump to top of screen
M -> move to middle of screen
L -> move to bottom
[Control][b] - Move back one full screen
[Control][f] - Move forward one full screen
[Control][d] - Move forward 1/2 screen
[Control][u] - Move back (up) 1/2 screen
* --> Search for ocurrence

substitute --> :%s/content/copy/g

\<Ctr> + a --> adds N to number
\<Ctr> + x --> substracts N to number



\<leader>+a --> previous buffer
\<leader>+s --> next buffer
\<leader>+d --> close buffer (if not changed)

    autocmd
    
Pattern: autocmd {events} {filetype} {command}


    Javascript
Must add tern file to root of project

.tern-project
ex:

{
  "libs": [
    "browser",
    "jquery"
  ],
  "loadEagerly": [
    "importantfile.js"
  ],
  "plugins": {
    "requirejs": {
      "baseURL": "./",
      "paths": {}
    }
  }
}


    Configs
set nrformats=decimal  (so it treats numbers as decimals)
set hls   Syntax Highlight



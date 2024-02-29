---
title: "Miscellaneous LaTeX hacks/tips"
category: Math
toc: true
tags: 
    - latex
    - neovim
    - vimtex
excerpt: ""
---
For both public consumption and my own use, here are some (heavily opinionated) opinions/options for using LaTeX.

# PdfLaTeX is much faster than XeLaTeX and LuaLaTeX
```
function make_format() {
    pdflatex --ini  \&pdflatex  mylatex.ltx *.tex
}
```

# Use parentheses instead of dollar signs
```
function dollar_switch () {
    perl -077pi.bak -e 's/(\s)\$\$(.+?)\$\$/\1\\[\2\\]/sg' ${1} && perl -077pi.bak -e 's/(\s)\$(.+?)\$/\1\\(\2\\)/sg' ${1}
}
```
# Getting math in section titles without compiler complaints

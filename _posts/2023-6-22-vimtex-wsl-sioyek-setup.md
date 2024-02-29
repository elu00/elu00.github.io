---
title: "VimTex + Sioyek setup for forward and reverse search in WSL"
category: Math
toc: true
tags: 
    - latex
    - neovim
    - sioyek
    - vimtex
excerpt: ""
---
# About
[Forward and reverse search](https://www.overleaf.com/blog/forward-and-reverse-search-in-sharelatex-2014-04-10) are two useful features that allow you to jump directly between a line of source code in a LaTeX file to it's corresponding position in the output PDF (as well as vice versa). 
Having been standard features in online/dedicated editors like TeXStudio or Overleaf, forward/reverse search have also made their way to (neo)vim via the excellent plugin [VimTex](https://github.com/lervag/vimtex).
Unfortunately for WSL users like myself, however, this integration seems to interact poorly with the Linux/Windows exectuable bridge.
After trying several PDF viewers, the only one I could find that worked with my setup was sioyek, though some extra effort (including submitting a patch!) was required in setting it up.
Here's a quick tldr of the configuration that I'm currently using.

# Requirements
It's unlikely that any of these version numbers are hard requirements, but I'm listing them here in case they're useful for troubleshooting.
- [Neovim](https://neovim.io/) installed under WSL, version ≥ 0.6.1
- [VimTex](https://github.com/lervag/vimtex) installed under the neovim version above, version ≥ 2.12
- [Sioyek](https://sioyek.info/) installed on windows, built after Feb 19 2023 (see below for a link to prebuilt binaries)


# Setup
## init.vim
Add the following three lines to your `init.vim`
```vim
let g:vimtex_view_method = 'sioyek'
let g:vimtex_view_sioyek_exe='sioyek.exe'
let g:vimtex_callback_progpath = 'wsl nvim'
```
## Sioyek config
- Download a build of sioyek newer than Feb 2023; as of this post, this does not include any of the prebuilt binaries from the main repo, so you'll need to grab a copy of `sioyek-release-windows.zip` from the [experimental builds](https://github.com/hexomancer/sioyek/releases)
- Add the executable to your path [(see here for a tutorial)](https://linuxhint.com/add-directory-to-path-environment-variables-windows/)
- Add the following line to `prefs.config`
```
vimtex_wsl_fix 1
```


and that's it!

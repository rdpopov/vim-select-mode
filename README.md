# vim-select-mode

A simple-ish wrapper around sed to approximate select mode from
helix/kakoune
## Only works in neovim!!!

## How it works
The plugin provides 2 main functions.
``` vim
    <Plug>VsmHighlightInMotion 
    <Plug>VsmInteractiveReplace
```

They can be used separately from each other. While in hx/kak selection must be
followed by a replace, I find that VsmHighlight is useful by itself. Also, the
selection can be repeated with a `.` since it was an operator. There is a
completion menu with the pattern from the last search to match it whole words or
as a sub string. Both very useful

Similarly, VsmInteractiveReplace is also useful on its own. The region it uses
is the last visual selection, thus it can be expanded manually beyond the strict
boundaries of a textobject. The pattern is the last searched pattern. So a
replace can happen multiple time with the same pattern. But what the pattern is
replaced with should be entered every time. Since it uses sed, sed-fu is also
allowed

I have added completion to both prompts. Highlight has some common patterns, the
contents of register `/` and the contents of `/` with `<\` and `\>` around it.
Replace part deletes if canceled with `<Esc>` or confirmed on empty. If canceled
it cancels the whole replace.
## Demo
![](.demo.gif)

## Some caveats
While achieving my goals for editing they are still powered by sed, so all of
its limitations follow, and how it behaves could depend on your configuration.

Also it is inefficient in it's implementation, the indented use case is small
files and small changes. Otherwise I think it will cause too many updates. Still
haven't had any problems.

By default it uses mark `z` and register `z` during normal operation.

## My example uses
### VsmHighlight
- highlight ls the occurrences of a pattern in a textobject, and also jump
  between them. This can be function, block, sentence, end of the line, etc.

### VsmInteractiveReplace
- Split a line on a certain symbol/s. This is kinda tricky to do with sed
  sometimes, especially if on the same line and with visual mode patterns.
- Add/remove/prepend/append to pattern.
- Refactoring a variable.
- Execute a :g norm command on lines with the match. - If the replace pattern
  begins with @, it will be interpreted as the arguments to a :g norm command

## Setup
There isn't much for setup. Install with plugin manager of choice.

And these are the keymaps I use.

Vimscript:
``` vim
    nnoremap <silent> s <Plug>VsmHighlightInMotion
    nnoremap <silent> <leader>r <Plug>VsmInteractiveReplace
```

Lua:
``` lua
    vim.api.nvim_set_keymap('n','s', '<Plug>VsmHighlightInMotion', {
        noremap = true,
        silent = true,
        desc="Highlight in motion"})
    vim.api.nvim_set_keymap('n','<Leader>r', '<Plug>VsmInteractiveReplace',{
            noremap = true,
            silent = true,
            desc="Replace in visuial selection"})
```

## Things to be added
- [ ] Something to mimic the behaviour of `gcn` to be able to use more
  commands(maybe)
- [x] Add gif a demo for both commands


## Why this is a thing?
Tried kak and helix, liked this mode. And I would consider switching to them
just for this. This kinda gives me the main thing that their modes do. It
doesn't have the transformations that can be done with kak, don't need them.

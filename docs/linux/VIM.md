Vim is a highly configurable text editor built to make creating and changing any kind of text very efficient.
One of the most useful features Vim has is called Vim motions, it allows you to move extremely fast through files and make very precise edits.

For those needing a quick reference: https://vim.rtorr.com/

## A little intro to Vim
Unlike most graphical text editors Vim employs different _Modes_ to acomplish all of it's goals

You can also run vimtutor for a noice CLI introduction to VIM

**Command Mode** -- This is the default mode you open files with, it allows you to type commands by pressing ":" and typing whatever you need

**Insert Mode** -- You will be using this mode to make edits to files, you will most commonly enter it by pressing <Insert> it by pressing `i` although there are other ways

**Visual Mode** -- This mode is used to select stuff, as you can't use a cursor in most terminals, you can use this mode if you need to select content for any reason

## Legend 
For all of these commands you will need to be in _Command_ mode!

ie. `<Ctrl-v>` switches you indo Visual block mode from Command mode

ie `gg` places your cursor at the beginning of the file

## Funs
General movement

- `gg` places your cursor at the beginning of the file
  
- `<Shift-g>` places your cursor at the end of the file
  
- `e` places your cursor at the end of a word
  
- `b` places your cursor at the beginning of a word
  
- `w` places your cursor at the beginning of the next word

Saving files

- `:q` closes a file, WITHOUT SAVING!
  
- `:w` writes to a file, without closing
  
- `:wq` writes and closes a file, you can also use `:x` to do the same thing
  
- `<Shift-zz>` writes and closes a file
  
- `<Shift-zq>` closes a file, WITHOUT SAVING!

Text manipulation

- `dw` deletes the word your cursor is currently placed on
  
-- With love, Dougie <3

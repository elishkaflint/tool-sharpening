# Vim

## Motions

- `0` move to the beginning of a line
- `ctl + G` see where you are in a file
- `gg` go to start of file
- `G` go to end of file
- `<linenumber>G` go to a specific line
- `ctrl + o` go back to where the cursor was before
- `ctrl + i` as above but go forwards
- `%` move to matching parenthesis or bracket

- `H` jump to top of window
- `M` jump to middle of window
- `L` jump to bottom of window

- `f` find eg. `f{`

Use `hjkl` with motions
Eg delete 6 lines `d7j` - delete 7 lines down

## Deletions

`dw` delete a word
`dd` delete a line
`2dd` delete 2 lines at once

`p` paste a `dd`-ed line (under line where the cursor is, doesn't have to be at the start of the line)

## Undo / Redo

- `u` undo a single change
- `U` undo all the changes on a whole line
- `ctrl + R` redo

## Change / Replace

- `r` replace, to change an `a` to a `b` hover over `a` and type `rb`
- `R` - replace mode, type over a word, then hit escape to come out of mode
- `ce` change to the end of a word
- `ea` append to end of word
- `cc` change whole line

## Substitutions

- `:s/thee/the` change thee for the, just once
- `:s/thee/the/g` change thee for the, anywhere in the line
- `:%s/thee/the/g` change thee for the, in the whole file
- `:%s/thee/the/gs` change thee for the, in the whole file, with prompt
- `:#,#s/thee/the/g` change thee for the, in given range (between # and #)

`:!` to execute any command line command eg. `:!ls` (NOTE: All : commands must be finished by hitting <ENTER>)

## Reading from / Writing to Files

`v` visual selection (highlight a whole block)
then press `:w TEST` eg to write selection to a new file called TEST in current directory
type `:r TEST` to add the text from that file into the current file
type `:r !ls` to see the output of the ls command in your file

## Copy Paste
- `y` "yank" or copy, also an operator therefore `yw` copies one word
- `p` "put" or paste

## Search

- `/<word>` search in forwards direction
- `?<word>` search in backwards direction

Typing `:set xxx` sets the option "xxx".  Some options are:
-      'ic' 'ignorecase'       ignore upper/lower case when searching
-      'is' 'incsearch'        show partial matches for a search phrase
-      'hls' 'hlsearch'        highlight all matching phrases

You can either use the long or the short option name.

Prepend "no" to switch an option off:   `:set noic`

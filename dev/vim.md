# [Vim](https://github.com/square/maximum-awesome)

## move
`hjkl`: left, down, up, right

* **word**
  * `w`: beginning of next
  * `b`: beginning
  * `e`: end

* **line**
    * `O or ^`: beginning (`I` to insert)
    * `$`: end (`A` to insert)
    * `:n` or `nG`: jump to line `n`

* **document**
    * `gg`: beginning
    * `G`: end

*  **page**
    * `CTRL-F`: forward
    * `CTRL-B`: backward

## delete
* `x`: character (`nx` to delete `n` characters)
* `dw`: word
* `dd`: line (`ndd` to delete `n` lines from current position)

* **delete `n` line(s)**
    * `dnk`: above
    * `dnj`: below

* **delete current line to**
    * `D` or `d$`: end
    * `d0` or `d^`: beginning

* **delete from document to**
    * `dgg`: start
    * `dG`: end
    * `dnG`: line number `n`

* **[vim-surround](https://github.com/tpope/vim-surround)**
    * `csw"` or `viwS"`: surround word with quotes (repeat on next word: `W.`)
    * `csw<strong>` or `vS<strong>`: add html tags around word
    * `cs'"`: change surrounding quote to single quote
    * `ds"`: delete surrounding quotation
    * `dst`: delete surrounding tag

* **other**
    * `di"`: delete inside double quotation marks
    * `d/c`: delete until next character `c` on any line

## search
* `/pattern`: forward for pattern
* `?pattern`: backward for pattern
* `n`: move to next occurrence of search pattern
* `N`: move to previous occurrence of search pattern
* `/\<word\>`: matches whole word (not pattern)
* `*`: if already on word on screen, hit the `*` key to search for next occurrence of whole word
* `pattern1\|pattern2\|pattern3`: find any of the patterns
* after searching for word, use `:%s//new/g` to change all occurrences to 'new', or `:g/` to list all lines containing the word.
* `:set hlsearch` and `:noh`: enable highlighting of all searches and turn off highlighting

* **search & replace**
    * `:s/, /\r&/g`: replace all `, ` on current line with a new line
    * `:%s/ \+/,/g`: replace all whitespace with single comma
    * `:%s/cat/dog/ig`: replace everything, case insensitive
    * `:%s/$/foo/g`: add `foo` to the end of each line
    * `:5,12s/foo/bar/g`: replace `foo` with `bar` globally between lines 5 and 12
    * `:%s/\s*\w\+\s*$//`: delete last word of each line

* **silver searcher with [ag.vim](https://github.com/rking/ag.vim)**
    * `,a [options] {pattern} [{directory}]`: search code across files in directory
    * `,a flask -l -i -G py`: list matching filenames for `flask`, case insensitive, in filenames that contain `py`

## jumping
* `fc`: search forward on current line to character `c` (`dfc`: delete)
* `Fc`: search backward on current line to character `c` (`dFc`: delete)
* `tc`: search forward on current line to character before character `c` (`dtc`: delete)
* `Tc`: search backward on current line to character after character `c` (`dTc`: delete)

## visual mode
* `v` - select by characters
* `V` - select by lines

* **prepred multiple lines**
    * `CTRL-v`, highlight lines, `Shift-I` to insert at beginning of block, press `Esc`

* **visual select**
    * `>>`: indent the text right by shiftwidth
    * `<<`: indent the text left by shiftwidth
    * `y`: "yank" the text into the paste buffer

* **visual insert**
    * `CTRL V` (select column), `Shift i` (type text to insert), `Esc`: insert text in multi-line selection

## new line
* `o`: start a new line below
* `O`: start a new line above

## clipboard
```
# in .vimrc.local (http://evertpot.com/osx-tmux-vim-copy-paste-clipboard/)
set clipboard=unnamed
```
* `y`: copy/yank
* `p`: paste

or
* `:w !pbcopy`: copy (after visual selection)
* `:r !pbpaste`: paste

## yank
* `yy`: yank (cut) the current line and put it in buffer
* `Nyy`: yank (cut) N lines and put it in buffer

## paste
* `p`: paste at the current position the yanked line or lines from the buffer

## undo/redo
* `u`: undo the previous operation
* `CTRL-r`: redo changes which were undone (undo the undos)

## commenting
* `\\\` toggles current line comment
* `\\` toggles visual selection comment lines

## tabs
* `gt`: go to next tab
* `gT`: go to previous tab

## add vertical line at 80 characters
* `:set colorcolumn=80`

## windows
* `CTRL-w R`: rotate windows up/left
* `CTRL-w r`: rotate windows down/right
* `CTRL-w L`: move current window to the far right
* `CTRL-w H`: move current window to the far left
* `CTRL-w J`: move current window to the very bottom
* `CTRL-w K`: move current window to the very top

## multiple files
* `:sp` open new file in horizontal split
* `:vsp` open new file in vertical split
* `vim -p *`: open multiple files in tabs
* `vim -o *`: open multiple files stacked
* `vim -O *`: open multiple files side by side

* **move between multiple windows**
    * `CTRL-w` + arrow / `CTRL-w` + `w`: move between windows
    * `CTRL-w t` + `CTRL-w K`: change vertically split windows to horizonally split
    * `CTRL-w t` + `CTRL-w H`: horizonally split to vertically split

## filesystem
* **Explore filesystem with [NERDTree](https://github.com/scrooloose/nerdtree)**
    * `,d`: toggle the drawer
    * `,f`: open NERDTree to the current file
    * `i`: open selected file in a horizontal split window
    * `s`: open selected file in a vertical split window
    * `t`: open selected file in a new tab

* **full path fuzzy file, buffer finder with [ctrlp.vim](https://github.com/kien/ctrlp.vim)**
    * `,t` brings up a project file filter for easily opening specific files
    * `,b` restricts `ctrlp.vim` to open buffers

## other maximum-awesome
* `vii`/`vai` visually select *in* or *around* the cursor's indent
* `Vp`/`vp` replaces visual selection with default register *without* yanking selected text (works with any visual selection)
* `,[space]` strips trailing whitespace
* `<C-]>` jump to definition using ctags
* `,l` begins aligning lines on a string, usually used as `,l=` to align assignments
* `<C-hjkl>` move between windows, shorthand for `<C-w> hjkl`

## Vundle
install plugins by adding following to vimrc.bundle.local
```
Plugin '<user>/<plugin>'
```
and run
```
:PluginInstall
```

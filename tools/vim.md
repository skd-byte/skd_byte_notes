# Vim Cheat Sheet

## Table of Contents

- [Vim Cheat Sheet](#vim-cheat-sheet)
  - [Getting Started](#getting-started)
  - [Modes](#modes)
  - [Navigation](#navigation)
  - [Editing](#editing)
  - [Visual Mode](#visual-mode)
  - [Registers](#registers)
  - [Search and Replace](#search-and-replace)
  - [Marks and Jumps](#marks-and-jumps)
  - [Buffers](#buffers)
  - [Windows and Splits](#windows-and-splits)
  - [Clipboard](#clipboard)
  - [Configuration](#configuration)
  - [Commands and Tricks](#commands-and-tricks)
  - [Diff Mode](#diff-mode)
  - [Sample vimrc](#sample-vimrc)

---

## Getting Started

- `vim --version`: check Vim version
- `vim moby.txt`: open a file for editing
- `vim hello.txt +8`: open file and jump to line 8 (`+` accepts any Vim command)
- `vim skd.txt +/emailid`: open file and jump to first match of "emailid"
- `vim +1,2d +wq conway.txt`: open file, delete lines 1-2, write and quit
- `vim` (no argument): start Vim with an empty buffer
- `~` lines indicate lines not inserted (beyond end of file)
- `Ctrl+g`: show the name of the file currently being edited

## Modes

Vim operates in several modes:

| Mode | Description | How to Enter |
|------|-------------|--------------|
| Normal | Default mode for navigating and manipulating text | `Esc` or `Ctrl+c` |
| Insert | For typing/inserting text | `i`, `I`, `a`, `A`, `o`, `O` |
| Visual | For selecting text | `v`, `V`, `Ctrl+v` |
| Command-line | For running Ex commands | `:` |

### Entering Insert Mode

| Key | Action |
|-----|--------|
| `i` | Insert before the cursor |
| `I` | Insert at the start of the line |
| `a` | Insert after the cursor |
| `A` | Insert at the end of the line |
| `o` | Open a new line below and enter insert mode |
| `O` | Open a new line above and enter insert mode |

## Navigation

### Character and Word Movement

| Key | Action |
|-----|--------|
| `h`, `j`, `k`, `l` | Left, down, up, right |
| `10l` | Move 10 characters to the right |
| `w` | Jump to the beginning of the next word |
| `e` | Jump to the end of the current/next word |
| `4w` | Move forward 4 words |

### Line Movement

| Key | Action |
|-----|--------|
| `0` | Jump to the beginning of the line |
| `$` | Jump to the end of the line |

### Sentence and Paragraph Movement

| Key | Action |
|-----|--------|
| `(` / `)` | Move between sentences (sentences end with `.`, `?`, or `!` followed by whitespace) |
| `{` / `}` | Move between paragraphs |

### File Movement

| Key | Action |
|-----|--------|
| `gg` | Jump to the beginning of the file |
| `G` | Jump to the end of the file |
| `8gg` or `:8` | Jump to line 8 |
| `Ctrl+f` | Scroll forward one page (keeps the last line from the previous page) |
| `Ctrl+b` | Scroll backward one page |

### Search Movement

| Key | Action |
|-----|--------|
| `/pattern` | Search forward for "pattern" |
| `?pattern` | Search backward for "pattern" |
| `n` | Jump to the next search match |
| `N` | Jump to the previous search match |
| `*` | Search forward for the word under the cursor |
| `#` | Search backward for the word under the cursor |

## Editing

### Delete

| Key | Action |
|-----|--------|
| `x` | Delete the character under the cursor (stored in register) |
| `dw` | Delete from cursor to the start of the next word |
| `d)` | Delete to the end of the sentence |
| `dd` | Delete the entire line |
| `d/genius` | Delete everything from cursor up to "genius" |

### Yank (Copy)

| Key | Action |
|-----|--------|
| `yw` | Yank (copy) from cursor to the start of the next word |
| `yy` | Yank the entire line |

### Paste

| Key | Action |
|-----|--------|
| `p` | Paste after the cursor |
| `P` | Paste before the cursor |
| `gP` | Paste before the cursor and leave cursor after the pasted text |
| `xp` | Transpose two characters (delete char, then paste after — a common trick) |

### Change

| Key | Action |
|-----|--------|
| `cw` | Delete the word and enter insert mode |
| `c/h` | Delete everything up to "h" and enter insert mode |

### Repeat

| Key | Action |
|-----|--------|
| `.` | Repeat the last change |

### File Operations

| Command | Action |
|---------|--------|
| `:w` | Write (save) the current file |
| `:w skd.txt` | Write to a specific filename |
| `:e skd.txt` | Open/switch to a file for editing (use `Tab` to autocomplete) |
| `:e .` | Open the directory browser |
| `:e!` | Reload the original file from disk (discard changes) |
| `:qa!` | Quit all buffers forcefully without saving |

## Visual Mode

| Key | Action |
|-----|--------|
| `v` | Character-wise visual selection |
| `V` | Line-wise visual selection (press `j`/`k` to extend) |
| `Ctrl+v` | Block-wise (column) visual selection |

In visual mode, select text and then apply an operation (e.g., `d` to delete, `y` to yank).

## Registers

Vim has 52 named registers (`a`-`z`, `A`-`Z`) plus special registers.

| Command | Action |
|---------|--------|
| `"ayy` | Yank the current line into register `a` |
| `"ap` | Paste from register `a` |

Example workflow: `"ayy` (copy line to register a) → `j` (move down) → `"ap` (paste from register a)

## Search and Replace

| Command | Action |
|---------|--------|
| `:s/you/they` | Replace the first occurrence of "you" with "they" on the current line |
| `:s/you/they/g` | Replace all occurrences on the current line |
| `:%s/you/they/g` | Replace all occurrences in the entire file |
| `:%s/you/they/gc` | Replace all with confirmation before each change |

## Marks and Jumps

### Marks

| Command | Action |
|---------|--------|
| `ma` | Set mark `a` at the current position (lowercase = local to buffer) |
| `'a` | Jump to the line of mark `a` |

### Jump List

| Command | Action |
|---------|--------|
| `Ctrl+o` | Jump to the previous position in the jump list |
| `Ctrl+i` | Jump to the next position in the jump list |
| `:jumps` | Show the jump list |
| `'.` | Jump to the line of the last change |

## Buffers

A buffer is the in-memory content of a file. A window is a view into a buffer.

| Command | Action |
|---------|--------|
| `:ls` | List all open buffers |
| `:bn` | Go to the next buffer |
| `:bp` | Go to the previous buffer |
| `:b <number>` | Go to a specific buffer by number |
| `:b ` + `Tab` | Autocomplete buffer names |
| `:bd` | Delete (close) the current buffer |
| `:new filename` | Open a new file in a horizontal split |

## Windows and Splits

| Command | Action |
|---------|--------|
| `:split` or `:sp` | Horizontal split of the current file |
| `:vsplit` or `:vs` | Vertical split of the current file |
| `:vnew` | Vertical split with a new empty buffer |
| `Ctrl+w w` | Cycle between open splits |
| `Ctrl+w c` | Close the current split |

## Clipboard

| Register | Description |
|----------|-------------|
| `"` | Default Vim clipboard (unnamed register) |
| `+` | System clipboard |
| `*` | Selection clipboard (X11 primary selection) |

Use `:set clipboard=unnamedplus` to make Vim use the system clipboard by default.

## Configuration

### Runtime Configuration

| Command | Action |
|---------|--------|
| `:set nu` | Enable line numbers |
| `:set nonu` | Disable line numbers |
| `:set ic` | Case-insensitive search |
| `:set hls` | Enable search highlighting |
| `:noh` | Clear current search highlighting |
| `:set incsearch` | Enable incremental search |
| `:set clipboard=unnamedplus` | Use system clipboard |
| `:syntax on` | Enable syntax highlighting |
| `:colorscheme` | List/set color schemes |
| `:set` | Show all settings that differ from defaults |

### Persistent Configuration

- Config file: `~/.vimrc`
- Comments in vimrc start with `"`

### Key Mapping

- `:noremap <SPACE> <C-F>`: map `Space` to page-down (`Ctrl+f`)

### Abbreviations

- `:abb _ys *youngstars*`: when you type `_ys` and press space, it expands to `*youngstars*`
- `Ctrl+v` then `Space`: insert a space without triggering abbreviation expansion

### Custom Commands

- `:com! Py ! python %`: define a command `:Py` that runs the current file with Python
- `%` refers to the current filename

## Commands and Tricks

| Command | Action |
|---------|--------|
| `:! ls` | Run an external shell command from within Vim |
| `:r filename` | Read and insert the contents of a file below the cursor |
| `:-1r filename` | Read and insert file contents above the current line |
| `:r !ls` | Read the output of a shell command into the buffer |
| `:%! jq` | Pipe the entire buffer through `jq` (useful for formatting JSON) |
| `gf` | Go to the file whose name is under the cursor |
| `vim weather.zip` | Vim can browse the contents of zip files |

## Diff Mode

- `vim -d file1.txt file2.txt`: open two files in diff mode side by side
- `Ctrl+w w`: switch between diff panes
- `do`: diff obtain — pull changes from the other file into the current one
- `dp`: diff put — push changes from the current file to the other one
- `:vert diffsplit file2.txt`: open a vertical diff split with another file from within Vim
- `:set diffopt+=vertical`: force vertical layout for diffs

## Sample vimrc

```vim
" Search & display
set ignorecase
set incsearch
set nohls
set number
set showmatch
set smartcase

set modeline

" Tab settings
set tabstop=8
set softtabstop=4
set shiftwidth=4

" Use system clipboard
set clipboard=unnamedplus

" Silence bells
set visualbell t_vb=
set noerrorbells

" Vertical diff
set diffopt=filler,vertical

```

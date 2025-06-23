" my vimrc --> in dev

" ======================== Basic Settings ======================== {{{1
set nocompatible            " Disable vi compatibility
syntax enable               " Enable syntax highlighting
filetype plugin indent on   " Enable filetype detection/plugins
set encoding=utf-8          " Set encoding
" }}}1

" ======================== Colors ======================== {{{1
if has('termguicolors')
    set termguicolors
endif
colorscheme molokai         " Install with :PlugInstall 'tomasr/molokai'
" }}}1

" ======================== UI Configuration ======================== {{{1
set number                  " Show line numbers
set relativenumber          " Relative line numbers
set cursorline              " Highlight current line
set wildmenu                " Better command-line completion
set showcmd                 " Show partial commands
set laststatus=2            " Always show status line
set scrolloff=8             " Keep 8 lines visible around cursor
set mouse=a                 " Enable mouse support
set splitbelow              " Horizontal splits open below
set splitright              " Vertical splits open to the right
" }}}1

" ======================== Indentation ======================== {{{1
set autoindent              " Auto-indent new lines
set smartindent             " Smart auto-indenting
set expandtab               " Use spaces instead of tabs
set tabstop=4               " Spaces per Tab
set softtabstop=4           " Spaces in insert mode
set shiftwidth=4            " Spaces for autoindent
" }}}1

" ======================== Search ======================== {{{1
set ignorecase              " Case-insensitive search
set smartcase               " Case-sensitive if uppercase
set incsearch               " Incremental search
set hlsearch                " Highlight matches
" }}}1

" ======================== Custom Functions ======================== {{{1
function! CompileJava()
    " Save the file first
    silent! write

    " Check if the file imports JavaFX classes
    let has_javafx = search('import javafx\.', 'nw')

    " Get file and class name (without extension)
    let file = shellescape(expand('%'))
    let class = shellescape(expand('%:r'))

    " Build compile/run command
    if has_javafx
        let javac_cmd = 'javac --module-path $PATH_TO_FX --add-modules javafx.controls ' . file
        let java_cmd = 'java --module-path $PATH_TO_FX --add-modules javafx.controls ' . class
    else
        let javac_cmd = 'javac ' . file
        let java_cmd = 'java ' . class
    endif

    " Run the commands
    execute '!' . javac_cmd . ' && ' . java_cmd
endfunction
" }}}1

" ======================== File Management ======================== {{{1
set hidden                  " Hide buffers instead of closing
set backup                  " Enable backups
set undofile                " Persistent undo
set backupdir=~/.vim/backup " Backup directory
set directory=~/.vim/swap   " Swap directory
set undodir=~/.vim/undodir  " Undo directory
" }}}1

" ======================== Key Mappings ======================== {{{1
let mapleader = " "         " Set leader to Space

" Clear search highlights
nnoremap <leader><space> :nohlsearch<CR>

" Quick save/quit
nnoremap <leader>w :w<CR>
nnoremap <leader>q :q<CR>

" Window navigation
nnoremap <C-h> <C-w>h
nnoremap <C-j> <C-w>j
nnoremap <C-k> <C-w>k
nnoremap <C-l> <C-w>l

" fuzzy search
nnoremap <C-p> :Files<Cr>
nnoremap <C-t> :Buffer<Cr>

" commenting
xmap <C-k> gc

"yank all
set clipboard^=unnamed,unnamedplus
nnoremap <leader>a gg"+yG 

" Quick escape
inoremap jj <Esc>
" }}}1

" ======================== File Execution ======================== {{{1
" Detect filetypes and set execution shortcuts
autocmd FileType python nnoremap <buffer> <leader>r :w!<bar>:!python3 %<CR>
autocmd FileType java nnoremap <buffer> <leader>r :!javac % && java %:r<CR>
autocmd FileType javascript nnoremap <buffer> <leader>r :!node %<CR>
autocmd FileType sh nnoremap <buffer> <leader>r :!bash %<CR>
" }}}1

" ======================== Vim-Plug Setup ======================== {{{1
" Install vim-plug if not installed
if empty(glob('~/.vim/autoload/plug.vim'))
  silent !curl -fLo ~/.vim/autoload/plug.vim --create-dirs
    \ https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
  autocmd VimEnter * PlugInstall --sync | source $MYVIMRC
endif

call plug#begin('~/.vim/plugged')

" ======================== Folding Plugins ======================== {{{2
Plug 'tmhedberg/SimpylFold'          " Better Python folding
Plug 'Konfekt/FastFold'              " Performance optimizations
Plug 'pseewald/vim-anyfold'          " Language-agnostic folding

" Productivity
Plug 'tpope/vim-commentary'
Plug 'tpope/vim-surround'

" Navigation
Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
Plug 'junegunn/fzf.vim'
Plug 'preservim/nerdtree'

" UI
Plug 'itchyny/lightline.vim'

" UltiSnips + friendly-snippets
Plug 'SirVer/ultisnips'
Plug 'honza/vim-snippets'

" Python
Plug 'davidhalter/jedi-vim'

" Java
Plug 'uiiaoo/java-syntax.vim'

" C++
Plug 'rhysd/vim-clang-format'

" }}}2

call plug#end()
" }}}1

" ======================== Plugin Configurations ======================== {{{1

" === NERDTree === {{{2
" Basic Settings
let g:NERDTreeShowHidden = 1                   " Show hidden files
let g:NERDTreeWinSize = 35                     " Panel width
let g:NERDTreeMinimalUI = 1                    " Disable help text
let g:NERDTreeAutoDeleteBuffer = 1             " Delete buffer when deleting file
let g:NERDTreeQuitOnOpen = 0                   " Keep open after file open
let g:NERDTreeIgnore = ['\.pyc$', '\.class$']  " Ignore file patterns

" Key Mappings
nnoremap <silent> <C-n> :NERDTreeToggle<CR>    " Toggle with Ctrl+n
nnoremap <leader>nf :NERDTreeFind<CR>          " Reveal current file
nnoremap <leader>nr :NERDTreeRefreshRoot<CR>   " Refresh root

" Window Management
augroup nerdtree_settings
  autocmd!
  " Close Vim if NERDTree is last window
  autocmd BufEnter * if tabpagenr('$') == 1 && winnr('$') == 1 && exists('b:NERDTree') && b:NERDTree.isTabTree() | quit | endif
  
  " Auto-open NERDTree when opening directory
  autocmd StdinReadPre * let s:std_in=1
  autocmd VimEnter * if argc() == 1 && isdirectory(argv()[0]) && !exists('s:std_in') | execute 'NERDTree' argv()[0] | wincmd p | enew | execute 'cd '.argv()[0] | endif
augroup END

" Visual Customization
let g:NERDTreeHighlightCursorline = 1          " Highlight current line
let g:NERDTreeFileExtensionHighlightFullName = 1 " Colorful icons
let g:NERDTreeExactMatchHighlightFullName = 1
let g:NERDTreePatternMatchHighlightFullName = 1

" File Operations
let g:NERDTreeCreateInPrefixIndicator = '📁'    " Custom create indicator
let g:NERDTreeDirArrowExpandable = '▸'
let g:NERDTreeDirArrowCollapsible = '▾'

" Sync with Current File
autocmd BufEnter * if &modifiable | silent! NERDTreeFind | wincmd p | endif
" }}}2

" }}}1

"======================== Folding Settings ======================== {{{1
" Enable folding for this .vimrc file
augroup vimrc_folding
    autocmd!
    autocmd BufRead,BufNewFile .vimrc setlocal foldmethod=marker
    autocmd BufRead,BufNewFile .vimrc setlocal foldlevel=0
augroup END
" }}}1

" vim: foldmethod=marker foldlevel=0




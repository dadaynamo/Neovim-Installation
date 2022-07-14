# Neovim-Installation
Guida veloce per installazione di neovim come un IDE per web develop
	Configurazione Neovim come un IDE - Versione Italiana.
--------------------------------------------------------------

Questa è una guida completa per avere nel giro di 10 minuti un piccolo ide esplandibile
con alla base il text-editor Neovim.
Questa è una guida molto pratica appunto per creare tutto ciò in modo rapido quindi ci saranno molto spesso poche descrizioni ma molti comandi
da terminale per l'installazioni delle parti e la creazione del IDE finale proprio perchè questo blog nasce appunto per creare il setup per programmare velocemente.
Iniziamo subito!!!


Iniziamo con le installazioni preliminari

Inizialmente bisogna aver già installato per sicurezza già nodejs, git, npm alle ultime versioni
aggiornate (non indico le versioni poichè non posso sapere a che versione state nel momento in cui leggete questa pagina).

Ora quindi iniziamo a installare neovim con il classico comando da terminale 
 
			sudo apt-get install neovim

Se avete versioni o distribuzioni particolari o sistemi operativi differenti da linux consultare il git di neovim ufficiale
https://github.com/neovim/neovim/wiki/Installing-Neovim


una volta fatto ciò potremmo modificare i nostri file di testo con neovim base.
basta da terminale andare nella cartella contenente il file di testo e scrivere:

			$ nvim [ nome_file.estensione ] 

 
Ora non ci resta che aggiungere a neovim i vari plugin fino a renderlo un vero IDE pronto
per la programmazione di qualsiasi LdP.


Setup delle cartelle e installazione dei plugin 
Per permettere l'installazione in neovim di plugin aggiuntivi bisogna usare un qualsiasi plugin manager.
Quello che uso io in questo tutorial è vim-plug   (Ulteriori info consultare https://github.com/junegunn/vim-plug)

Runnare da terminale il seguente comando
sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'



Creazione del file di configurazione 
creare all'interno della cartella .config/ una nuova directory chiamata nvim/.
all'interno creiamo un file di testo chiamato init.vim

modificarlo con nvim scrivendo 
		$ nvim init.vim

e cliccando invio.


ora non resta che copiare all'interno di questo file il seguende script

" The default plugin directory will be as follows:
"   - Vim (Linux/macOS): '~/.vim/plugged'
"   - Vim (Windows): '~/vimfiles/plugged'
"   - Neovim (Linux/macOS/Windows): stdpath('data') . '/plugged'
" You can specify a custom plugin directory by passing it as the argument
"   - e.g. `call plug#begin('~/.vim/plugged')`
"   - Avoid using standard Vim directory names like 'plugin'

:set number
:set relativenumber
:set autoindent
:set tabstop=4
:set shiftwidth=4
:set smarttab
:set softtabstop=4
:set mouse=a

call plug#begin()

Plug 'http://github.com/tpope/vim-surround' " Surrounding ysw)
Plug 'https://github.com/preservim/nerdtree' " NerdTree
Plug 'https://github.com/tpope/vim-commentary' " For Commenting gcc & gc
Plug 'https://github.com/vim-airline/vim-airline' " Status bar
Plug 'https://github.com/lifepillar/pgsql.vim' " PSQL Pluging needs :SQLSetType pgsql.vim
Plug 'https://github.com/ap/vim-css-color' " CSS Color Preview
Plug 'https://github.com/rafi/awesome-vim-colorschemes' " Retro Scheme
Plug 'https://github.com/neoclide/coc.nvim'  " Auto Completion
Plug 'https://github.com/ryanoasis/vim-devicons' " Developer Icons
Plug 'https://github.com/tc50cal/vim-terminal' " Vim Terminal
Plug 'https://github.com/preservim/tagbar' " Tagbar for code navigation
Plug 'https://github.com/terryma/vim-multiple-cursors' " CTRL + N for multiple cursors

set encoding=UTF-8

call plug#end()

nnoremap <C-f> :NERDTreeFocus<CR>
nnoremap <C-n> :NERDTree<CR>
nnoremap <C-t> :NERDTreeToggle<CR>
nnoremap <C-l> :call CocActionAsync('jumpDefinition')<CR>

nmap <F8> :TagbarToggle<CR>

:set completeopt-=preview " For No Previews

:colorscheme jellybeans

let g:NERDTreeDirArrowExpandable="+"
let g:NERDTreeDirArrowCollapsible="~"

" --- Just Some Notes ---
" :PlugClean :PlugInstall :UpdateRemotePlugins
"
" :CocInstall coc-python
" :CocInstall coc-clangd
" :CocInstall coc-snippets
" :CocCommand snippets.edit... FOR EACH FILE TYPE

" air-line
let g:airline_powerline_fonts = 1

if !exists('g:airline_symbols')
    let g:airline_symbols = {}
endif

" airline symbols
let g:airline_left_sep = ''
let g:airline_left_alt_sep = ''
let g:airline_right_sep = ''
let g:airline_right_alt_sep = ''
let g:airline_symbols.branch = ''
let g:airline_symbols.readonly = ''
let g:airline_symbols.linenr = ''

inoremap <expr> <Tab> pumvisible() ? coc#_select_confirm() : "<Tab>"



Una volta fatto ciò assicurarsi di stare in modalità normal di nvim e scrivere :wq per salvare e uscire da nvim.

rientrare nuovamente in init.vim e scrivere :PlugInstall per installare i vari plugin scritti all'interno dello script (quelli racchiusi tra plug#start e plug#end )

Ora bisogna solo risolvere piccoli problemi di dipendenze poichè alcuni plug le richiedono.

Qui per risolvere basta seguire i messaggi di errore in rosso che compariranno in neovim.

Soprattutto per il plugin di autocompletamento bisogna inanzitutto installare exuberant-ctags

sudo apt-get install exuberant-ctags

Poi bisogna installare le dipendenze con yarn install.
bisogna entrare nella cartella contenente i vari plugin e scrivere
$ sudo npm install -g yarn

$ yarn install

$ yarn build

ed ora avremo da installare il plugin che per esempio autocompleta il javascript.
Entrare su nvim e scrivere :CocInstall coc-tsserver e una volta fatto ciò i nostri programmi in js si creeranno da soli !! :)


Piccole istruzioni basilari per usare i plugin installati
-Mentre scriveremo programmi in js usare le freccette per selezionare e con invio per autocompletare il blocco di codice

-in neovim scrivere in modalità normal  :NERDTreeFocus per vedere l'albero del filesystem dwl nostro progetto
- premere f8 per aprire il tab che mostra le varie funzioni e variabili del nostro progetto
-scrivere :terminal bash per creare un nuovo tab con il terminale di linux in neovim

:wq per salvare e uscire da una finestra vim
:q! forzare l'uscita da una finestra vim
- cliccare il tasto i per entrare in modalità scrittura (celeste)
- cliccare esc per tornare in modalità normale (gialla)
-con il mouse cliccare e trascinare per selezionare parte di testo e cliccare d per tagliare , y per copiare. successivamente si può printare con il tasto p la parte di testo copiata o tagliata.
- entrare in modalità visuale cliccare v (partendo da mod normale) e usare le freccette per selezionare il testo.

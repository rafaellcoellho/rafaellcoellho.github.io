---
title: "[aprendizado vim] dividir painel, ctags e fzf"
date: 2022-06-10 00:13:14
layout: post
---

### Dividir tela

Adicionei algumas melhorias no comportamento de divisão de painel do vim
colocando isso no `.vimrc`:

```
set splitbelow
set splitright

nnoremap <C-h> <C-w>h
nnoremap <C-j> <C-w>j
nnoremap <C-k> <C-w>k
nnoremap <C-l> <C-w>l

noremap <silent> <C-Left> :vertical resize +3<CR>
noremap <silent> <C-Right> :vertical resize -3<CR>
noremap <silent> <C-Up> :resize +3<CR>
noremap <silent> <C-Down> :resize -3<CR>
```

#### comandos

- `:vs` -> divisão vertical de painel;
- `:sp` -> divisão horizontal de painel.

Também posso abrir um arquivo específico usando `:vs <caminho_arquivo>`.
Como configurado nos remapeamento de teclas:

- `C-h` -> move para painel da esquerda;
- `C-j` -> move para painel de cima;
- `C-k` -> move para painel de baixo;
- `C-l` -> move para painel da direita;
- `C-Seta` -> ajusta tamanho dos painéis.

### Ctags

No linux existe um programa bem interessante chamado `ctags`. Com ele
é possível navegar entre "entidades" num código-fonte no vim, o
equivalente a ctrl click numa IDE como pycharm. Para checar se está
instalado fazemos:

```
$ ctags --version
Exuberant Ctags 5.8
```

Podemos rodar no código-fonte que estamos editando com:

```
$ ctags -R .
```

O git não vai ficar feliz com esse arquivo novo. Para evitar fazer o commit
por acidente, posso dar um:

```
$ echo tags >> .git/info/exclude
```

#### comandos

- `C-]` -> ir até definição da tag;
- `g C-]` -> ir até definição caso seja ambíguo;
- `C-t` -> voltar de onde veio;

### fzf

Primeiramente preciso instalar os seguintes programas:

```
$ sudo dnf install fzf the_silver_searcher ripgrep fd-finder
```

Adicionar essas linhas no `.zshrc`:

```
export FZF_DEFAULT_COMMAND="fd --type f --color=never --hidden"
export FZF_DEFAULT_OPTS="--no-height"
```

O fzf não se limita a apenas um plugin do vim, vou anotar aqui
como utilizar como um programa qualquer. A ideia é que se
eu passar texto para a entrada dele usando pipe:

```
$ history | fzf
```

Nesse momento vou estar pesquisando todo o conteúdo do meu histórico
de comandos do zsh. Vamos supor que eu queira saber como rodar um
comando de build do docker, é só digitar `docker build` que o fzf vai
me mostrar a lista desses comandos.

Mas qual a diferença de só usar um `grep`? A busca fuzzy usa um algoritmo
mais inteligente que uma pesquisa normal.

Caso eu queria navegar entre as opções que o fzf me mostrou, é só usar
as teclas de seta ou segurar ctrl e usar `jk` como no vim.

Mas quando selecionarmos a nossa opção o fzf vai apenas jogar o resultado
desse texto no `stdout`, ou seja, printar no terminal. Agora vou mostrar
aplicações mais úteis.

#### trocar ctrl r do zsh para fzf

Tendo instalado o oh-my-zsh basta adicionar no `.zshrc`:

```
plugins=([..] fzf)
```

Agora só dar `<C-r>` e ser feliz.

#### vim

É possível abrir o fzf dentro do neovim e abrir arquivos em um novo buffer.
Primeiro adiciono os plugins no `.vimrc`:

```
Plug 'junegunn/fzf'
Plug 'junegunn/fzf.vim'
Plug 'airblade/vim-rooter'
```

A partir desse momento podemos usar os comandos:

- `:Files` -> pesquisar arquivos na raiz do projeto;
-	`:GFiles?` -> pesquisar arquivos alterados no git e ver diff;
-	`:Buffers` -> pesquisar arquivos abertos nos buffers do vim;
-	`:Ag` -> pesquisar conteúdo de arquivos no projeto usando the silver searcher;
-	`:Rg` -> pesquisar conteúdo de arquivos no projeto usando ripgrep (respeita conteúdo do git etc);
-	`:Lines` -> pesquisar conteúdo de arquivos nos buffers abertos;
-	`:BLines` -> pesquisar conteúdo do arquivo no buffer ativo no momento;
-	`:Commits` -> pesquisar commits apenas envolvendo o buffer ativo no momento;

### Referências

+ [artigo sobre dividir painéis no vim]
+ [distrotube tutorial sobre splits]
+ [DevInsideYou tutorial sobre fzf]
+ [plugin do fzf especifico para vim]

[artigo sobre dividir painéis no vim]: https://thoughtbot.com/blog/vim-splits-move-faster-and-more-naturally
[distrotube tutorial sobre splits]: https://www.youtube.com/watch?v=Zir28KFCSQw
[DevInsideYou tutorial sobre fzf]: https://www.youtube.com/watch?v=tB-AgxzBmH8
[plugin do fzf especifico para vim]: https://github.com/junegunn/fzf.vim

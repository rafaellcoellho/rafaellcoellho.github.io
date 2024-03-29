---
title: "[aprendizado vim] arquivo de configuração, plugins e find"
date: 2022-05-09 21:37:07
layout: post
---

Até agora eu evitei utilizar qualquer configuração permanente
pois, já tentei utilizar anteriormente outras configurações prontas como
[the ultimate vim configuration] e sempre me senti sobrecarregado com tanta
informação.

Mas agora preciso configurar algumas alterações permanentes então chegou a
hora de aprender mais. Vou adicionado configurações conforme a necessidade
e tentando evitar algo muito complexo.

### Arquivos de configuração

Como indicado por essa [man page do neovim] encontramos o caminho do arquivo
de configuração:

```
	The config file is located at:
	Unix			~/.config/nvim/init.vim		(or init.lua)
	Windows			~/AppData/Local/nvim/init.vim	(or init.lua)
	|$XDG_CONFIG_HOME|	$XDG_CONFIG_HOME/nvim/init.vim	(or init.lua)
```

No vim o mesmo arquivo é encontrado em `~/.vimrc`.

### Plugins

Estarei utilizando o gestor de plugins chamado [vim-plug]. Segui normalmente os
comandos de instalação. E adiciono no início do arquivo de configuração:

```
call plug#begin()
call plug#end()
[...]
```

Após adicionar as linhas é preciso usar o seguinte comando para instalar:

```
:PlugInstall
```

Por padrão os plugins são instalados em `~/.local/share/nvim/plugged`. Para 
desinstalar basta:

```
:PlugClean
```

Ou de maneira manual:

```
$ cd ~/.local/share/nvim/plugged
$ rm -rf <pasta-do-plugin>
```

Plugins que usarei inicialmente:

- [lightline] -> aquela barrinha estilosa de status;
- [onedark] -> tema clássico do atom.

Obs: resolvendo problema de modo desaparecer quando nome de arquivo for muito
grande e mudando o tema para onedark no lightline:

```
" Configuração do lightline
let g:lightline = {
      \ 'colorscheme': 'onedark',
      \ 'active': {
      \   'left': [
      \     ['mode', 'paste'],
      \     ['filename', 'readonly', 'modified']],
      \   'right': [
      \     ['lineinfo'],
      \     ['percent'],
      \     ['filetype', 'fileencoding', 'fileformat']] },
      \ 'component': {
      \   'filename': '%<%{LightLineFilename()}' } }
function! LightLineFilename()
  let l:fname = expand('%:t')
  let l:fpath = expand('%')
  return &filetype ==# 'dirvish' ?
        \   (l:fpath ==# getcwd() . '/' ? fnamemodify(l:fpath, ':~') :
        \   fnamemodify(l:fpath, ':~:.')) :
        \ &filetype ==# 'fzf' ? 'fzf' :
        \ '' !=# l:fname ? fnamemodify(l:fpath, ':~:.') : '[No Name]'
endfunction
```

### Comando find

No post anterior quando falei de buffer eu utilizei o comando `:e`
(abreviação de `:edit`) para abrir outros arquivos. Mas uma opção melhor
é utilizar o `:find`.

```
:find <caminho>
```

A grande diferença é que o `:find` utiliza a variável `path` para completar
o nome do arquivo que está sendo digitado. Então esse comando se torna
extremamente útil quando adicionamos no nosso `init.vim` a seguinte linha:

```
:set path+=**
```

Essa linha significa que adicionamos no `path` para ser pesquisado todos
os diretórios e subdiretórios da pasta atual em que o vim foi invocado.

A partir de agora ao invés de digitar o caminho todo até o arquivo usando
`:edit`, apenas precisamos digitar `:find` e alguma string que esteja no
nome do arquivo que queremos abrir e usar `<Tab>` para navegar nas opções
disponíveis.

### Referências

+ [the ultimate vim configuration]
+ [man page do neovim]
+ [vim-plug]
+ [vim-airline]
+ [solução do problema do lightline]

[the ultimate vim configuration]: https://github.com/amix/vimrc
[man page do neovim]: https://wiki.archlinux.org/title/Neovim
[vim-plug]: https://github.com/junegunn/vim-plug
[vim-airline]: https://github.com/vim-airline/vim-airline
[lightline]: https://github.com/itchyny/lightline.vim
[onedark]: joshdick/onedark.vim
[solução do problema do lightline]: https://github.com/itchyny/lightline.vim/issues/237

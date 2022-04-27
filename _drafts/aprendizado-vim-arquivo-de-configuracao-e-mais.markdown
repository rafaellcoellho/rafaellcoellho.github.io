---
title: "[aprendizado vim] arquivo de configuração, find"
layout: post
---

Até agora eu evitei utilizar qualquer tipo de configuração permanente
pois já tentei utilizar anteriormente outras configurações prontas como 
[the ultimate vim configuration] e sempre me senti sobrecarrecado com tanta
informação.

Mas agora preciso configurar algumas alterações permanentes então chegou a
hora de aprender mais.

### Arquivos de configuração

Como indicado por essa [man page do neovim]:

```
	The config file is located at:
	Unix			~/.config/nvim/init.vim		(or init.lua)
	Windows			~/AppData/Local/nvim/init.vim	(or init.lua)
	|$XDG_CONFIG_HOME|	$XDG_CONFIG_HOME/nvim/init.vim	(or init.lua)
```

No vim o mesmo arquivo é encontrado em `~/.vimrc`.

### Plugins

Estarei utilizando o gestor de plugins chamado [vim-plug]. Como indicado para
instalar usei o comando:

```
sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
```

E adiciono no inicio `~/.config/nvim/init.vim`:

```
call plug#begin(system('echo -n "${XDG_CONFIG_HOME:-$HOME/.config}/nvim/plugged"'))
[...]
call plug#end()
[...]
```

### Comando find

No post anterior quando falamos de buffer eu utilizei o comando `:e`
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
nome do arquivo que queremos abrir e usar `<Tab>` para navegar nos opções
disponíveis.

### Referências

+ [the ultimate vim configuration]
+ [man page do neovim]
+ [vim-plug]
+ [vim-airline]

[the ultimate vim configuration]: https://github.com/amix/vimrc
[man page do neovim]: https://wiki.archlinux.org/title/Neovim
[vim-plug]: https://github.com/junegunn/vim-plug
[vim-airline]: https://github.com/vim-airline/vim-airline

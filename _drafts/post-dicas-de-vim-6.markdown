---
title: "[aprendizado vim]"
#date: 2022-06-10 00:13:14
layout: post
---

Apesar de estar estudando/praticando vim já faz alguns meses e ter melhorado 
muito desde então, ainda não foi possível utilizar como meu editor principal
para editar código fonte. Para escrever esses artigos do blog e pequenas
alterações em arquivos de configuração a usabilidade do neovim com poucos
plugins é muito mais que suficiente. 

Mas o objetivo final é usar o neovim como principal ambiente de desenvolvimento
a maioria do tempo. Para isso é preciso que suas funcionalidades me ajudem a ser
produtivo tanto quanto o que vem por padrão em no vscode ou pycharm.

A linguagem mais utilizada no meu dia a dia é o python. Espero com esse post
fazer uma configuração descente para ter:

- syntax highlight descente;
- autocomplete;
- pular entre tags no código (sem ter que rodar o ctags toda hora).

Mas antes de começar a adicionar tudo isso, tenho outras coisas para tirar do
caminho.

### Atualizando neovim

No dia que estou escrevendo esse post ainda estou utilizando o fedora 33, e nos
repositórios dele só é possível obter o neovim `0.4.4`. Então para obter uma
versão mais recente fui na [página de release do neovim no github], baixei o
arquivo `nvim-linux64.tar.gz` da versão `0.7.2` e executei os comandos:

```
$ tar xzvf nvim-linux64.tar.gz
$ cd nvim-linux64
$ sudo cp -r ./bin/nvim /usr/local/bin
$ sudo cp -r ./lib/nvim /usr/local/lib
$ sudo cp -r ./share/nvim  /usr/local/share
```

Por algum motivo tive de instalar novamente o [Plug] e dar o `:PlugInstall`
para voltar ao estado anterior. Mas aparentemente tudo continua no mesmo local
¯\_(ツ)_/¯, vai entender.



### Referências

+ [artigo sobre dividir painéis no vim]

[artigo sobre dividir painéis no vim]: https://thoughtbot.com/blog/vim-splits-move-faster-and-more-naturally
[página de release do neovim no github]: https://github.com/neovim/neovim/releases
[Plug]: https://github.com/junegunn/vim-plug

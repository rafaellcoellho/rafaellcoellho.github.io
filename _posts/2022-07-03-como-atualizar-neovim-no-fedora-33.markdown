---
title: "Como atualizar neovim no fedora 33"
date: 2022-07-03 21:42:23
layout: post
---

**Problema**: No dia que estou escrevendo esse post ainda estou utilizando o
fedora 33, e nos repositórios dele só é possível obter o neovim `0.4.4`.

Então para obter uma versão mais recente fui na [página de release do neovim no
github], baixei o arquivo `nvim-linux64.tar.gz` da versão `0.7.2` e executei
os comandos:

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

Aparentemente outra maneira de instalar o neovim seria jogando `nvim-linux64`
em `/opt` e adiciona o caminho de `bin/nvim` no `$PATH`.

### Referências

+ [bom video sobre sistema de pastas do linux do fireship]

[página de release do neovim no github]: https://github.com/neovim/neovim.github.io/ 
[Plug]: https://github.com/junegunn/vim-plug
[bom video sobre sistema de pastas do linux do fireship]: https://www.youtube.com/watch?v=42iQKuQodW4

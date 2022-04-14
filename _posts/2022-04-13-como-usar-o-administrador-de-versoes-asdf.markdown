---
title: "Como usar o adminisrador de versões asdf"
date: 2022-04-13 22:52:12
layout: post
---

O [asdf] é um administrador de versões de ferramentas. Como desenvolvedor eu 
preciso ter mais de uma versão de certos programas instalada no meu linux e o
asdf é a melhor solução que encontrei. Quando em outros tempos seria preciso 
usar nvm, rvm etc agora só é preciso usar uma ferramenta.

Para instalar é só ir na [documentação do asdf]. O próximo passo 
é instalar os plugins para cada linguagem:

+ [ruby]
+ [node]
+ [golang]
+ [erlang]
+ [elixir]
+ [kotlin]
+ [rust]

A partir dai é possível usar o comando `asdf plugin-list` pra saber quais estão 
instalados no momento. Para listar todas as versões de uma linguagem especifica uso 
o comando `asdf list-all linguagem`. Exemplo:

```
$ asdf list-all ruby
1.8.5-p52
1.8.5-p113
1.8.5-p114
1.8.5-p115
1.8.5-p231
1.8.6
1.8.6-p36
[...]
```

Normalmente as versões principais não tem esses complementos juntos ao numero 
da versão. Então se eu quiser instalar a versão 1.8.6 do ruby eu uso o comando 
`asdf install ruby 1.8.6`. 

Para usar a versão no linux inteiro uso o comando `asdf global ruby 1.8.6`. 
Caso seja preciso usar uma versão do ruby apenas para um projeto, posso dar `cd`
até lá e rodar `asdf local ruby 1.8.6`.

Por ultimo, mas não menos importante, é sempre bom lembrar de dar update nos plugins
com `asdf plugin-update --all` e dar update no próprio asdf com `asdf update`. 

 
## Referências 
+ [asdf]
+ [episódio do akitando](https://www.youtube.com/watch?v=epiyExCyb2s)

[asdf]: https://asdf-vm.com/guide/introduction.html
[ruby]: https://github.com/asdf-vm/asdf-ruby
[node]: https://github.com/asdf-vm/asdf-nodejs
[golang]: https://github.com/kennyp/asdf-golang
[erlang]: https://github.com/asdf-vm/asdf-erlang
[elixir]: https://github.com/asdf-vm/asdf-elixir
[kotlin]: https://github.com/asdf-community/asdf-kotlin
[rust]: https://github.com/code-lever/asdf-rust
[documentação do asdf]: https://asdf-vm.com/guide/getting-started.html#_3-install-asdf
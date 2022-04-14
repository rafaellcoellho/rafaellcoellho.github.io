---
title: "Como usar asdf"
layout: post
---

O [asdf] é um administrador de versões de ferramentas. Como desenvolvedor eu 
preciso ter mais de uma versão de certos programas instalada no meu linux e o
asdf é a melhor solução que encontrei. Quando antes seria preciso usar nvm, 
rvm etc, agora só é preciso usar uma ferramenta.

Para instalar tem alguns comandos bobos que não quero perder tempo neles aqui,
então é só ir na documentação de [como instalar o asdf]. A partir desse momento
é preciso instalar os plugins para cada linguagem:

+ [ruby]
+ [node]
+ [golang]
+ [erlang]
+ [elixir]
+ [kotlin]
+ [rust]

A partir dai é possível usar o comando `asdf plugin-list` pra saber quais estão 
instalador no momento. Para instalar uma versão da linguagem especifica uso 
o comando `asdf list-all linguagem`.

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

Normalmente as versões principais não tem esses complementos juntos com o numero 
da versão. Então se eu quiser instalar a versão 1.8.6 do ruby eu uso o comando 
`asdf install ruby 1.8.6`. 

Para usar a versão no nosso linux inteiro uso o comando `asdf global ruby 1.8.6`. 
Caso seja preciso usar uma versão do ruby apenas para um projeto, posso dar `cd`
até lá e rodar `asdf local ruby 1.8.6`.

Por ultimo, mas não menos importante é sempre bom lembrar de dar update nos plugins
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

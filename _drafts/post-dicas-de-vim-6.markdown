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

### LSP

Pesquisando um pouco sobre LSP encontrei o seguinte:

> LSP is defined as an open set of rules that describes how language clients
> and language servers should communicate. All services are handled
> by the server and exposed to the client via the protocol. The client is 
> either a standard text editor or the integrated development environments 
> (IDEs) used by a developer, while the server is the process containing all 
> of the smarts about a given language.
>
> Microsoft developed the protocol in 2015 to help separate language tools and
> editors, allowing programming language support to be implemented and
> distributed independently of any given editor or IDE.

Infelizmente a versão do neovim que estava usando no meu fedora 33 (a `0.4`)
ainda não tinha implementado suporte a LSP, tive que atualizar para a versão 
`0.7.4`.

### Referências

+ [artigo sobre lsp]

[artigo sobre lsp]: https://thoughtbot.com/blog/vim-splits-move-faster-and-more-naturally

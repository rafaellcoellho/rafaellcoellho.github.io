---
title: "[aprendizado vim] corretor orgráfico, macros"
layout: post
---

Eu pretendia dizer no post anterior sobre vim que isso seria uma série
mas sempre que eu faço isso nunca posto mais sobre o assunto. Agora que 
estou escrevendo o segundo post é que tenho coragem de afirmar que vai ser 
uma "série" chamada "aprendizado vim".

A ideia é utilizar o [neovim] sem nenhum plugin. Utilizar o máximo possível
das configurações padrão e caso precise de alterações sempre fazer
brevemente quando necessário usando `:` (dois pontos.

### Corretor ortográfico

Esse [artigo sobre corretor ortográfico o vim] me ajudou com o necessário.
Posso ativar usando o comando:

```
:set spell spelllang=en_us
```

Para desabilitar:

```
:set nospell
```

Atalhos:

- **Ir para próxima/anterior**: `]s`/`[s`;
- **Ver sugestões de correção**: `z=`;
- **Adicionar palavra ao dicionário**: `zg`;
- **Remove palavra errada**: `zw`;

### Macros

É possível salvar uma sequência de comandos e executar novamente eles.
Para criar fazemos um seguinte:

```
q<letra><comandos>q
```

Explicando: 

1. iniciamos criação de macro digitando `q`;
2. digitamos em qual letra vamos salvar a macro;
3. executamos os comandos;
4. encerramos a gravação de comando digitando `q` novamente.

Para executar podemos fazer:

```
@<letra>
```

Ou seja, podemos usar `@` para executar quantas vezes quiser os comandos salvos
nas macros.

### Pesquisar

Para ignorar a diferença de maisculas e minúsculas:

```
:set ignorecase
```

Para voltar a considerar é só usar:

```
:set noignorecase
```

Atalhos:

- **Palavra que esta no cursor**: `*`;

### Referências

+ [neovim]
+ [artigo sobre corretor ortográfico o vim]
+ [video no youtube sobre macros](https://www.youtube.com/watch?v=Hd33Q0ZjZuk)
+ [wiki sobre macros no vim](https://vim.fandom.com/wiki/Macros)
+ [artigo sobre pesquisa no vim](https://linuxize.com/post/vim-search/)

[neovim]: https://neovim.io/
[artigo sobre corretor ortográfico o vim]: https://www.linux.com/training-tutorials/using-spell-checking-vim/

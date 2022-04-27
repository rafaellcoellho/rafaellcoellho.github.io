---
title: "[aprendizado vim] corretor, macros, pesquisa e buffer de arquivo"
layout: post
---

Eu pretendia dizer no post anterior sobre vim que isso seria uma série
mas sempre que eu faço isso nunca posto mais sobre o assunto. Agora que 
estou escrevendo o segundo post é que tenho coragem de afirmar que vai ser 
uma "série" chamada "aprendizado vim".

A ideia é utilizar o [neovim] sem nenhum plugin. Utilizar o máximo possível
das configurações padrão e caso precise de alterações sempre fazer
brevemente quando necessário usando `:` (dois pontos).

### Corretor ortográfico

O corretor padrão do vim apenas checa por palavras erradas e não tem dicionário 
para português do Brasil, mas é melhor que nada. Posso ativar usando o comando:

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
Para criar fazemos o seguinte:

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

Para ignorar a diferença de maiúsculas e minúsculas:

```
:set ignorecase
```

Para voltar a considerar é só usar:

```
:set noignorecase
```

Atalhos:

- **Pesquisar palavra que esta no cursor**: `*`.

### Editar múltiplos arquivos (buffers)

Abrindo um novo buffer de arquivo:

```
:e <caminho>
```

Para saber quais buffers existem e seus respectivos números:

```
:ls
```

Agora basta navegar entre os buffers usando:

```
:b<numero do buffer>
```

E deletar com **b**uffer **d**elete:

```
:bd<numero do buffer>
```

Atalhos:

- **Trocar para o último buffer acessado**: `<C-6>`.

### Referências

+ [neovim]
+ [artigo sobre corretor ortográfico o vim]
+ [video no youtube sobre macros](https://www.youtube.com/watch?v=Hd33Q0ZjZuk)
+ [wiki sobre macros no vim](https://vim.fandom.com/wiki/Macros)
+ [artigo sobre pesquisa no vim](https://linuxize.com/post/vim-search/)

[neovim]: https://neovim.io/
[artigo sobre corretor ortográfico o vim]: https://www.linux.com/training-tutorials/using-spell-checking-vim/

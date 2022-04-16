---
title: "Anotações de uso sobre editor vim"
layout: post
---

Venho utilizado o editor de texto vim para editar minhas mensagem de commit
e arquivos simples já faz alguns meses ou mais. Sempre tive vontade 
de aprender mais sobre ele mas sempre acabo desistindo. Esse post é uma 
maneira de me motivar a aprender.

Talvez as anotações não façam muito sentido para outras pessoas. Mas os 
pontos a seguir foram aprendizados e as vezes é importante poder voltar a aqui e 
relembrar.

### Notação

Existe uma documentação extensa sobre as notações que o vim usa para
comandos do teclado mas não é minha intenção falar sobre isso aqui. Vou
utilizar os mais convenientes:

```
[...]
<S-...>		shift-key			*shift* *<S-*

<C-...>		control-key			*control* *ctrl* *<C-*

<M-...>		alt-key or meta-key		*meta* *alt* *<M-*

<A-...>		same as <M-...>			*<A-*

<D-...>		command-key (Macintosh only)	*<D-*
[...]
```

### Movimentação

- **Descer/Subir uma página**: `<C-d>/<C-u>` (**d** de down e **u** de up);
- **Centralizar a linha atual na tela**: `zz`;
- **Por linha atual no inicio/fim da tela**: `zt/zb` (**t** de top e **b**);
- **Movimentar entre parágrafos**: `{` para subir e `}` para descer;
- **Ir para o próximo `(`,`[` ou `{`**: `%`
- **Voltar para onde estava depois de dar um `gg`/`G`**: `''` (sim, duas vezes o aspas simples).

### Deletar

- **Deletar até o fim da linha**: `D`;
- **Deletar ao redor/entre palavra**: `daw/diw` (**d**elete **a**round/**i**n **w**ord);
- **Deletar ao redor/entre parêntese**: `da(/di(` (**d**elete **a**round/**i**n **(**);
- etc.

### Pesquisar

- **Pesquisar palavra**: `/`; 
- **Próxima ocorrência da palavra**: `n`;
- **Ocorrência anterior da palavra**: `N`.

### Modo de seleção de texto (VISUAL MODE)

- **Selecionar uma linha inteira**: `V`;
- **Selecionar em bloco**: `<C-v>`; 
- **Selecionar ao redor/entre palavra**: `vaw/viw`; 
- etc.

### Misc

- **Sair e salvar**: `ZZ`;
- **Sair e descartar alterações**: `ZQ`;
- **Desfazer/refazer**: `u\<C-r>`;
- **Desfazer ações na linha enteira**: `U`;
- **Executar novamente a ultima sequência de comandos**: `.` (sim, ponto);
- **Acrescentar diretamente no inicio/fim da linha**: `I/A`.

Essas são as anotações por hoje. Espero juntar anotações para outras
postagens dessas. 

## Referências 
+ [let's play do luke smith do vim tutor](https://www.youtube.com/watch?v=d8XtNXutVto)
+ [documentação sobre key annotation no vim](http://vimdoc.sourceforge.net/htmldoc/intro.html#key-notation)

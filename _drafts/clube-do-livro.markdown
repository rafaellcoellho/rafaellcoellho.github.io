---
title: "[clube do livro]"
date: 2022-12-26 00:00:00
layout: post
---

Começando aqui mais uma série de posts. Agora a proposta é ler capítulos 
de livros técnicos e fazer um post de resumo dos aprendizados e caso exista 
exercícios, também suas resoluções.

### Preparações

Antes de começar o conteúdo, preciso instalar o SGBD e uma ferramenta
para interagir com o banco usando interface gráfica.  Para criar localmente o ambiente
o docker é a ferramenta de preferência e eu já escrevi como fazer  isso [nesse post].
Para interagir gráficamente com o SGBD vou utilizar o incrível DBeaver. Também fiz um 
[post explicando como instalar e atualizar] no fedora.

### Conceitos básicos

O primeiro livro a ser lido é [guia mangá de banco de dados]. O primeiro capítulo é apenas
uma introdução boba sobre o que é um banco de dados. Já no segundo capítulo temos alguns
conceitos iniciais interessantes.

### Rascunho

1. Uma unidade de dados é chamada de registro (record)
2. Cada item no registro é chamado de campo (field)
3. Cada registro contém campos do mesmo tipo

4. No banco de dados um campo no qual os valores não se repetem é chamado de único (unique)
5. A ausencia de um valor é chamado de nulo (null)

6. Um banco de dados pode usar um modelo de dados hierárquico, modelo de dados em rede ou um modelo de dados relacional
7. O modelo de dados relacional tem como base uma tabela bidimencional
8. No modelo de dados relacional:
  - uma tabela é também chamada de relação
  - uma unidade de dados ou registro é chamada de linha
  - cada item de dados ou campo é chamado de coluna

11. Alguns campos tem papel importante no banco de dados, como o campo chamado de chave (key)
12. O campo de código de produto é chamado de chave primaria (primary key)

13. O modelo de dados relacional é projetado de forma que se possa processas os dados com operações matemáticas
14. Uma operação para extrair uma coluna é chamado de projeção
15. São oito operações matemáticas possíveis
16. Outra vantagem do moeldo relacional é que se pode processar os dados combinando as operações


### Referências

+ [guia mangá de bancos de dados]
+ [site do DBeaver]

[guia mangá de bancos de dados]: https://www.amazon.com/-/pt/dp/8575221639
[nesse post]: https://rafaellcoellho.github.io/2022/07/27/rodando-postgresql-14-usando-docker-no-linux.html
[site do DBeaver]: https://dbeaver.io/
[post explicando como instalar e atualizar]: https://rafaellcoellho.github.io/2022/12/22/instalando-e-atualizando-dbeaver-no-fedora.html
---
title: "[projeto query+] epicos, histórias de usuário e atividades"
layout: post
---

### Conseitos

Antes de se preocupar com o projeto vou deixar aqui explicado os conseitos
básicos de metodologia ágil.

#### épico

```
Um épico é uma quantidade de trabalho que pode ser dividida em tarefas específicas (chamadas
de histórias do usuário) com base nas necessidades/solicitações dos clientes ou usuários
finais.
```

A ideia é que o épico seja um objetivo sólido geral que pode ser atingido
através da realização de atividades especificas, as histórias de usuário.

#### história de usuário

```
Uma história do usuário é uma explicação informal e geral sobre um recurso de software
escrita a partir da perspectiva do usuário final. Seu objetivo é articular como um recurso
de software pode gerar valor para o cliente.
```

Toda a ideia é escrever a descrição da atividade mais especifica que o épico,
e com o foco no usuário final. A linguagem precisa ser clara e objetiva e não
deve conter conteúdo muito técnico.

#### atividade

As atividades servem para ser mais técnico e específico na hora de realizar a
história de usuário. Eu gosto da ideia d fazer quase um checklist e pontuar cada
atividade de acordo com a quantidade de trabalho necessária para concluir.
No final é só somar tudo, estimar a data de entrega de cada atividade e ao
da sprint temos um burndown chart bem bacana do progresso.

### Projeto

O épico do projeto é bem simples:

```
Resumo: Implementar prova de conceito

Descrição: A entrega da prova de conceito para a disciplina da faculdade, onde será
necessário fazer uma demonstração das funcionalidades básicas do sistema como adicionar
perguntas, responder, criar disciplinas, pesquisar perguntas, etc.
```

Agora vamos para as histórias de usuário:

```
1. Professor cadastra perfil

Como professor
Eu quero cadastrar o meu perfil 
Para que possa criar turmas e responder perguntas.

2. Professor cadastra turmas

Como professor
Eu quero cadastra turma
Para que seja possível agrupar respostas de uma disciplina ministrada em um período.

3. Aluno cadastra perfil

Como aluno
Eu quero cadastrar o meu perfil 
Para que possa responder perguntas e seguir turmas.

4. Aluno enviar pergunta para turma

Como aluno
Eu quero enviar uma pergunta para uma turma cadastrada previamente
Para que quando a pergunta for respondida pelo professor responsável pela turma seja possível consultar futuramente.

5. Professor responde pergunta cadastrada na turma

Como professor
Eu quero responder pergunta cadastrada nas minhas turmas
Para que alunos possam pesquisar as respostas.

6. Aluno pesquisa por pergunta usando frase

Como aluno
Eu quero procurar por uma resposta usando uma frase relacionada com minha dúvida
Para que seja possível encontrar respostas já postadas anteriormente sobre o assunto pesquisado.

7. Professor reprova resposta submetida por aluno

Como professor
Eu quero poder reprovar respostas repetidas ou inválidas
Para que as respostas cadastradas não sejam repetidas ou inválidas.

8. Professor cria palavras-chave

Como professor
Eu quero criar palavras-chave
Para que seja possível vincular respostas.

9. Professor adiciona palavras-chave as respostas

Como professor
Eu quero relacionar respostas cadastradas por mim com palavras-chave
Para que seja mais fácil encontrar na pesquisa.

10. Sistema de classificação automático atribui palavras-chave

Como administrador do sistema
Eu quero utilizar sistema de classificação automático
Para que seja atribuído palavras-chave conforme o texto da pergunta.
```

As atividades eu prefiro ir criando conforme vou trabalhando, fazendo as
considerações técnicas o mais próximo possível do que é preciso no momento.

### Selecionando histórias de usuário

Agora vem uma das horas mais importantes, o que priorizar? Como um bom
estudante que sou, deixei para fazer toda a prova de conseito no ultimo
final de semana antes da entrega ;).

Eu criei todas as histórias de usuário para uma versão inicial do projeto,
mas mesmo essas poucas que criei não poderão ser concluidas em 2 dias.

As histórias eu que adicionarei ao épico da prova de conseito são, na ordem 
implementação:

1. Aluno enviar pergunta para turma;
2. Professor responde pergunta cadastrada na turma;
3. Professor reprova resposta submetida por aluno;
4. Aluno pesquisa por pergunta usando frase.

Parece estranho implementar a resposta do aluno antes de implementar o cadastro
dele. Mas a proposta aqui é ter usuários "mockados" no sistema, sem nenhuma
implementação séria e partir para o comportamento core, que vai ser demonstrado.

Sabendo que autenticação é complicado e que não valeria a pena mostrar apenas o
cadastro de usuário na demonstração do projeto, por que perder tempo com ela?
Vejo a possibilidade de ter problemas para implementar ela e acabar perdendo o
final de semana inteiro com isso.

Estou considerando também o tempo de fazer o setup dos projetos de front-end e
backend, criação de arquivos docker, deploy para ambiente de teste etc. Coisas que
não podemos adicionar nas histórias de usuário pois não são pertinentes aos
usuários finais, mas irão consumir um tempo considerável.

Essas atividades mais técnicas como deploy e setup de ambiente vão aparecer
dentro das histórias de usuário naturalmente, então não precisamos nos preocupar
com muita descrição delas.

O próximo passo é começar a implementação, [ai vamos nós]...
### Referencias 

+ [artigo sobre histórias de usuário]
+ [artigo sobre épicos]
+ [ai vamos nós]

[artigo sobre histórias de usuário]: https://www.atlassian.com/br/agile/project-management/user-stories
[artigo sobre épicos]: https://www.atlassian.com/br/agile/project-management/epics
[ai vamos nós]: https://www.youtube.com/watch?v=Kwrfc4HghBA

---
title: "Básico sobre SQLAlchemy"
layout: post
---

Ando precisando estudar um pouco de SQLAlchemy para o trabalho. Para isso vou
reviver os tempos de "Linguagem de Programação I" na faculdade e escrever uma
pequena aplicação de linha de comando para gerenciar uma agenda.

Vou utilizar o PostgreSQL que configurei no [post anterior]. O objetivo é ter
as seguintes funcionalidades:

- `agd`: mostra numero de contatos cadastrados;
- `agd mostrar`: mostrar todos os contatos da agenda;
- `agd mostrar --pagina <int> --tamanho <int>`: mostrar contatos
de forma paginada;
- `agd adicionar`: inicia input para adicionar um novo contato;
- `agd deletar <id_do_contato>`: deleta um contato a partir do id;
- `agd editar <id_do_contato>`: deleta um contato a partir do id;

Por sorte eu não preciso adicionar nenhum pacote para criar linhas de comando,
pois o python já tem um na biblioteca padrão chamada **argparse**. Após dar uma lida na
[documentação do argparse] e no [tutorial básico sobre argparse do anthonywritescode]
escrevi um [pequeno esqueleto da aplicação]esse não é o objetivo desse post.

### Referências

+ [documentação do SQLAlchemy]
+ [documentação do argparse]
+ [tutorial básico sobre argparse do anthonywritescode]
+ [pequeno esqueleto da aplicação]

[documentação do SQLAlchemy]: https://www.sqlalchemy.org/
[documentação do argparse]: https://docs.python.org/3/howto/argparse.html#
[tutorial básico sobre argparse do anthonywritescode]: https://www.youtube.com/watch?v=-Sgw-6a1HjU
[pequeno esqueleto da aplicação]: https://github.com/rafaellcoellho/blog-basico-sqlalchemy/commit/bd8ca069f39d6ef5a589651a3c49a2cec9d4af5b

[post anterior]: https://rafaellcoellho.github.io/2022/07/27/rodando-postgresql-14-usando-docker-no-linux.html

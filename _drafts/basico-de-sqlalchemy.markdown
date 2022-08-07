---
title: "[projeto agenda] cli com argparse e migrations com alembic"
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
documentação do argparse e no tutorial do anthonywritescode escrevi um 
[pequeno esqueleto da aplicação].

### Migrations

Antes de começar a implementar algo preciso garantir que a estrutura do banco de dados
tenha sido criada.

```sql
CREATE DATABASE agenda
WITH
   OWNER =  postgres
   ENCODING = 'UTF8'
   TABLESPACE = pg_default;
```

Agora vou instalar o **alembic** e o **psycopg2** no projeto:

```
poetry add alembic psycopg2
```

Talvez seja preciso instalar algumas dependencias no linux:

```
sudo dnf install python-devel libpq-devel
```

Iniciando o alembic:

```
alembic init alembic
```

Foi criado o arquivo `alembic.ini` e a pasta `alembic` na raiz no projeto. No arquivo `alembic.ini`
é preciso editar uma variável para passar a URL do banco criado:

```
[...]

sqlalchemy.url = postgresql://postgres:root@localhost:5432/agenda

[...]
```

Para gerar a primeira migration:

```
alembic revision -m "criar_banco_agenda"
```

Agora podemos editar o arquivo `alembic/versions/b33cd3331c9a_criar_banco_agenda.py`:

```python
[...]

from alembic import op
import sqlalchemy as sa

[...]

revision = 'b33cd3331c9a'
down_revision = None

[...]

def upgrade() -> None:
    op.create_table(
        "contato",
        sa.Column("id", sa.Integer, primary_key=True),
        sa.Column("nome", sa.String(50), nullable=False),
        sa.Column("numero", sa.String(9), nullable=False),
    )


def downgrade() -> None:
    op.drop_table("contato")
```

- `revision`: a identificação da migration atual, também é utilizado no nome do arquivo;
- `down_revision`: essa variável o alembic usa para descobrir a ordem correta de rodar as
migrations, ou seja, é a referência para a migration anterior;
- `upgrade()`: função que vai ser ativamente executada;
- `downgrade()`: função que vai ser executada caso seja preciso desfazer a migration, não é
obrigatória.

Podemos utilizar a flag `--sql` no alembic para descobrir qual sql de fato vai ser executado:

```sql
$ alembic upgrade head --sql
[...]
CREATE TABLE contato (
    id SERIAL NOT NULL,
    nome VARCHAR(50) NOT NULL,
    numero VARCHAR(9) NOT NULL,
    PRIMARY KEY (id)
);
```

A sintáxe básica do `CREATE TABLE` é essa:

```sql
CREATE TABLE [IF NOT EXISTS] nome_da_tabela (
  nome_da_coluna_1 tipo(tamanho) constraint,
  nome_da_coluna_2 tipo(tamanho) constraint,
  contraints_da_tabela
);
```

Conhecendo a sintaxe, o comando passa a ser muito simples de entender.

Por ultimo é só rodar sem a flag `--sql` que o alembic vai rodar os scripts contra o banco
informado na sql.

### Referências

+ [documentação do SQLAlchemy]
+ [documentação do argparse]
+ [tutorial básico sobre argparse do anthonywritescode]
+ [tutoriais de postgresql]

[documentação do SQLAlchemy]: https://www.sqlalchemy.org/
[documentação do argparse]: https://docs.python.org/3/howto/argparse.html#
[tutorial básico sobre argparse do anthonywritescode]: https://www.youtube.com/watch?v=-Sgw-6a1HjU
[tutoriais de postgresql]: https://www.postgresqltutorial.com/

[pequeno esqueleto da aplicação]: https://github.com/rafaellcoellho/blog-basico-sqlalchemy/commit/bd8ca069f39d6ef5a589651a3c49a2cec9d4af5b
[post anterior]: https://rafaellcoellho.github.io/2022/07/27/rodando-postgresql-14-usando-docker-no-linux.html

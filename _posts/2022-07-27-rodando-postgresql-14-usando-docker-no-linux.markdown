---
title: "Rodando PostgresSQL 14 usando docker no linux"
date: 2022-07-27 21:15:12
layout: post
---

### Instalando

Até já tenho outro [post sobre como rodar MySQL 8] usando docker. Lá eu comecei
baixando a imagem com a versão desejada, e vou fazer o mesmo aqui:

```
docker pull postgres:14.4
```

Mas resolvi fazer uma coisa diferente: ao invés de usar **bind mounts** vou usar um
volume. Depois de uma lida na [documentação oficial do docker sobre volumes] descobri
como criar um:

```
docker volume create postgres14-blogpost
```

Agora é só rodar o contêiner:

```
docker run -d \
  --name postgres-14.4 \
  -e POSTGRES_PASSWORD=root \
  --mount type=volume,source=postgres14-blogpost,target=/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:14.4
```

Os argumentos já foram explicados no [post sobre como rodar MySQL 8], mas houve a troca
do argumento `-v` para o `--mount`.

+ **--mount**: *type* indica que usaremos um volume para guardas informações externas ao contêiner,
*source* é o nome do volume e *target* é a pasta padrão que o postgres usa para armazenar os dados.

### Conectando usando psql

Agora é hora de testar se está tudo funcionando. Como eu não instalei o pacote antes
tive que fazer:

```
sudo dnf install postgresql
```

Podemos conectar com o comando:

```
psql -h localhost -p 5432 -U postgres -W
```

Outra maneira é utilizando um URI:

```
psql postgresql://postgres:root@localhost:5432
```

Agora é só digitar a senha definida pela variável `POSTGRES_PASSWORD`.

### Comandos do psql

- `\c <nome_do_banco>` -> conectar a um banco;
- `\l` -> listar todos os bancos;
- `\dt` -> listar todas as tabelas do banco;
- `\d <nome_da_tabela>` -> descreve uma tabela no banco;
- `\dn` -> listar todos os schemas do banco;
- `\df` -> listar todas as funções no banco;
- `\dv` -> listar todas as views no banco;
- `\du` -> listar todos os usuários e suas respectivas `roles`;
- `\g` -> executar novamente o comando anterior;
- `\s` -> mostrar o histórico de comandos;
- `\i <nome_de_arquivo>` -> executar comandos vindos de um arquivo;
- `\e` -> editar um comando no editor de texto definido pela variável de ambiente `EDITOR`;
- `\ef <nome_da_funcao>` -> editar uma função do banco de dados usando editor de texto
definido pela variável `EDITOR`
- `\timing` -> habilitar a opção de mostrar tempo que cada query leva;
- `\?` -> ajuda com os comandos do psql;
- `\q` -> sair.

### Referências

+ [imagem oficial do postgres]
+ [documentação oficial do docker sobre bind mounts]
+ [documentação oficial do docker sobre volumes]
+ [README.md da imagem oficial do postgres]
+ [post sobre como rodar MySQL 8]

[imagem oficial do postgres]: https://hub.docker.com/_/postgres
[documentação oficial do docker sobre bind mounts]: https://docs.docker.com/storage/bind-mounts/
[documentação oficial do docker sobre volumes]: https://docs.docker.com/storage/volumes/
[README.md da imagem oficial do postgres]: https://github.com/docker-library/docs/blob/master/postgres/README.md
[post sobre como rodar MySQL 8]: https://rafaellcoellho.github.io/2020/11/08/instalar-mysql-8-usando-docker-no-linux.html

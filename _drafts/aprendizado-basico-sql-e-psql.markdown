---
title: "Rodando PostgresSQL 14 usando docker no linux"
date: 2022-07-27 21:15:12
layout: post
---

### Instalando

Até já tenho um outro [post sobre como rodar MySQL 8] usando docker. Lá eu comecei
baixando a imagem com a versão desejada, e vou fazer o mesmo aqui:

```
docker pull postgres:14.4
```

Mas resolvi fazer uma coisa diferente: ao invés de usar **bind mounts** vou usar um
volume. Depois de uma lida na [documentação oficial do docker sobre volumes] descobri
como criar um volume:

```
docker volume create postgres14-blogpost
```

Agora é só rodar o container:

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

+ **--mount**: *type* indica que usaremos um volume para guardas informações externas ao container,
*source* é o nome do volume e *target* é a pasta padrão que o postgres usa para armazenar os dados.

### Conectando usando psql

Agora é hora de testar se está tudo funcionando. Como eu não tinha instalado o pacote antes
tive que fazer:

```
sudo dnf install postgresql
```

Podemos conectar com o comando:

```
psql -h localhost -p 5432 -U postgres -W
```

Outra maneira é utilizando uma URI:

```
psql postgresql://postgres:root@localhost:5432
```

Agora é só digitar a senha que foi definida pela variável `POSTGRES_PASSWORD`.

### Comandos do psql

- `\l` -> listar todos os bancos;
- `\c <nome_do_banco>` -> conectar a um banco;
- `\dt` -> listar todas as tabelas do banco;
- `\d <nome_da_tabela>` -> inspecionar o schema da tabela;
- `\timing` -> habilitar a opção de mostrar tempo que cada query leva;
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

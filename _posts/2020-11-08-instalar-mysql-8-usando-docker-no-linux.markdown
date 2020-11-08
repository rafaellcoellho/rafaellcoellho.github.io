---
title: Rodando MySQL 8 usando docker no linux
date: 2020-11-08 00:20:00
layout: post
---

Vou utilizar a imagem da versão mais recente do MySQL disponível no dockerhub no momento, ou seja, a `8.0.22`. Baixando a imagem:

```bash
$ docker pull mysql:8.0.22
```

Vamos criar a pasta onde o conteúdo do banco de dados fica armazenado no nosso disco:

```bash
$ mkdir ~/External/Docker/mysql-8
```

Agora vamos instanciar um container do MySQL para usar "globalmente":

```bash
$ docker run --name mysql-8 -e MYSQL_ROOT_PASSWORD=root -d -p 3306:3306 -v $HOME/External/Docker/mysql-8:/var/lib/mysql mysql:8.0.22
```

Explicando todos os argumento:

+ **--name**: A string identificadora do container. Como vou usar essa instância como "global", eu chamei ela só de "mysql-8", mas se estiver usando para um projeto específico, poderia ser algo como "nomeDoProjeto-mysql";
+ **-e**: Expõe a variável de ambiente `MYSQL_ROOT_PASSWORD` com o valor da senha do root do banco. Mais informações sobre outras variáveis de ambiente é só olhar na descrição da imagem no DockerHub lá em referências;
+ **-d**: Sobe o container no background e printa id do container;
+ **-p**: Expõe a porta 3306 do container para a porta 3306 do seu computador.
+ **-v**: Monta o volume interno do container na pasta que eu criei anteriormente. Isso impede que os arquivos salvos no banco desapareceçam quando paramos o container.

Por último temos qual imagem vamos utilizar, nesse caso escolhemos a que acamos de fazer pull alguns passos atrás `mysql:8.0.22`.

Depois de executar o comando teremos uma instância do MySQL 8 rodando.
É comum instalar uma interface gráfica para interagir com o banco. 
Eu pessoalmente gosto muito do [DBeaver](https://dbeaver.io/), mas hoje pretendo instalar o [MySQL Workbench](https://www.mysql.com/products/workbench/).

Infelizmente é necessário ter uma conta e fazer login [aqui](https://login.oracle.com/mysso/signon.jsp) antes de baixar a versão community do Workbench.
Após fazer o login é só ir na [página de download](https://dev.mysql.com/downloads/workbench/) e escolher o pacote para sua distro.

No meu caso eu baxei o RPM para o instalar no meu fedora.
Não sei qual o motivo, mas antes de instalar o RPM tive que instalar uma dependência, o `libzip`.

```bash
$ sudo dnf install libzip
```

Ai sim eu pude instalar corretamente o workbench:

```bash
$ sudo rpm -Uvh mysql-workbench-community-8.0.22-1.fc31.x86_64.rpm
```

Ao abrir já aparece a nossa instância:

![print workbench](/images/2020-11-08-instalar-mysql-8-usando-docker-no-linux/01.png)

Clicando ele vai pedir a senha, que definimos que seria `root`.

### Referências:

+ [documentação na imagem do MySQL no dockerhub](https://hub.docker.com/_/mysql)
+ [tutorial de como instalar o postgres usando docker](https://hackernoon.com/dont-install-postgres-docker-pull-postgres-bee20e200198)
+ [documentação do docker run](https://docs.docker.com/engine/reference/commandline/run/)


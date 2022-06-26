---
title: "[projeto query+] setup do back-end"
layout: post
---

Como um bom projeto de software, não foi possível cumprir no prazo estipulado.
O aprendizado aqui é: tentar fazer do jeito "certo" de primeira é sempre
uma cilada. Esse artigo já corresponde a segunda tentativa de implementar esse
sistema. Então vamos lá.

#### primeiros passos

Primeiro criei o repositório e clonei na minha máquina. Escolhi a licença,
escrevi README.md etc. Vou usar o poetry como gerenciador de versão do projeto.
Iniciando:


```
$ poetry init --name="query-plus" --python="~3.9"
```

Usei o simbolo `~` antes da versão do python para indicar que o poetry pode
instalar qualquer patch que houver (3.9.1, 3.9.2 etc), mas não subir para a
versão 3.10 do python. Básico do poetry:

- `poetry install`: instala todas as dependencias e cria um `virtualenv` se não
houver;
- `poetry shell`: ativa o `virtualenv` do projeto;
- `poetry run <comando>`: roda o `<comando>>` dentro do `virtualenv` do projeto.

O resultado:

```
[tool.poetry]
name = "query-plus"
version = "0.1.0"
description = ""
authors = ["rafaellcoellho <rafaellcoellho@gmail.com>"]
license = "GPLv3"

[tool.poetry.dependencies]
python = "~3.9"

[tool.poetry.dev-dependencies]

[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"
```

#### primeiro teste

Vou seguir a regra do TDD aqui, primeiro escrevendo o teste para em seguida ir
implementando de acordo com a necessidade do teste. Objetivos:

- criar teste de um endpoint de exemplo; 
- criar endpoint exemplo.

### Referencias

+ [artigo sobre setup de um projeto em python]
+ [explicação sobre notação de versões]
+ [teste com FastAPI]

[artigo sobre setup de um projeto em python]: https://cjolowicz.github.io/posts/hypermodern-python-01-setup/
[explicação sobre notação de versões]: https://python-poetry.org/docs/dependency-specification/
[teste com FastAPI]: https://fastapi.tiangolo.com/tutorial/testing/


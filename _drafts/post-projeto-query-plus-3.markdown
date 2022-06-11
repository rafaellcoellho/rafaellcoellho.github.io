---
title: "[projeto query+] setup do back-end"
layout: post
---

Primeiro criei o repositório e clonei na minha máquina. Escolhi a licença,
escrevi README.md etc. Vou usar o poetry como gerenciador de versão do projeto.
Iniciando:

```
$ poetry init --name="query-plus" --python="~3.9"
```

Usei o simbolo `~` antes da versão do python para indicar que o poetry pode
instalar qualquer patch que houver (3.9.1, 3.9.2 etc), mas não subir para a
versão 3.9 do python. Básico do poetry:

- `poetry install`: instala todas as dependencias e cria um `virtualenv` se não
houver;
- `poetry shell`: ativa o `virtualenv` do projeto;
- `poetry run <comando>`: roda o `<comando>>` dentro do `virtualenv` do projeto.

Estou seguindo a estrutura de pastas apresentada no livro Architecture
Patterns with Python.

### Referencias

+ [artigo sobre setup de um projeto em python]
+ [explicação sobre notação de versões]
+ [livro Architecture Patterns with Python]
+ [estrutura de pastas aprensentado no livro]

[artigo sobre setup de um projeto em python]: https://cjolowicz.github.io/posts/hypermodern-python-01-setup/
[explicação sobre notação de versões]: https://python-poetry.org/docs/dependency-specification/
[livro Architecture Patterns with Python]: https://www.amazon.com.br/Architecture-Patterns-Python-Harry-Percival/dp/1492052205
[estrutura de pastas aprensentado no livro]: https://www.cosmicpython.com/book/appendix_project_structure.html


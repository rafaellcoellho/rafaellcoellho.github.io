---
title: "[eos] Pydantic"
layout: post
---

**Projeto escolhido**: Pydantic

**Repositório**: https://github.com/samuelcolvin/pydantic

1. Lendo https://pydantic-docs.helpmanual.io/contributing/
2. Seguindo instruções do link.
3. Não vou dar fork no repo agora, vou apenas clonar direto e depois adiciona mais um remote pro meu fork.

```bash
$ git clone git@github.com:samuelcolvin/pydantic.git
```

5. criando virtualenv com python 3.8 com virtualenvwrapper.

```bash
$ mkvirtualenv --python=python3.8 pydantic
```

6. usar pycharm para editar o código.
7. continuar seguindo comandos

```bash
$ make install
```

8. rodar testes

```bash
$ make
```

obs:
 - tudo muito simples;
 - gostei do make como executor de comandos, gostaria de fazer um artigo sobre isso no blog;

9. ler documentação básica do pydantic em https://pydantic-docs.helpmanual.io/

obs: li só Overview, Install e algumas partes de Usage/Models

10. começar a ler issues com help wanted no github https://github.com/samuelcolvin/pydantic/issues?q=is%3Aopen+is%3Aissue+label%3A%22help+wanted%22

- [x] [#3500](https://github.com/samuelcolvin/pydantic/issues/3500) (não, é relacionado com github actions)
- [x] [#2429](https://github.com/samuelcolvin/pydantic/issues/2429) (complicado, não entendi muita coisa)

obs: talvez eu devesse fazer uma anotação sobre cada coisa que aprendo investigando as issues.
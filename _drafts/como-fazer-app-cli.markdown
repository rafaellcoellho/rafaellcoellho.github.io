---
title: "Usando argparse, tox e setuptools para fazer aplicativo de cli em python"
date: 2022-10-16 14:45:00
layout: post
---

Vou escever um aplicativo "hello world" para aprender como escrever, testar e
deployar um aplicativo de linha de comando. 

### Criando aplicação

O python já vem com uma biblioteca para facilitar a criação de aplicações de linha
de comando chamada argparse. Vou começar criando o modulo principal:

```
└── cli
    ├── main.py
    └── __init__.py
```

O arquivo `main.py` vai conter todo o código:

```python
import argparse


def main(argv=None):
    parser = argparse.ArgumentParser(prog="greet")
    parser.add_argument("name")
    args = parser.parse_args(argv)

    print(f"hello {args.name}")

    return 0


if __name__ == "__main__":
    exit(main())
```

Vou testar se o módulo está funcionando usando a linha de comando:

```
$ python -m cli.main rafael
hello rafael
```

Sucesso! Mas caso a minha aplicação comece a ficar mais complexa não vou querer
testar manualmente todas as possibilidades. Vou criar um teste para garantir que
essa lógica esteja sempre correta.

### Escrevendo testes

Criando o múdulo de testes:

```
├── cli
│   ├── __init__.py
│   ├── main.py
└── tests
    ├── __init__.py
    └── test_cli.py
```

Utilizei a própria função `main` para passar os argumentos e o pytest também
ajuda a checar o `stdout` utilizando o argumento `capsys`:

```python
from cli.main import main


def test_hello_world_cli(capsys):
    main(["test"])
    result = capsys.readouterr()
    assert result.out == "hello test\n"
```

Usando pytest:

```
$ pytest
============================= test session starts ==============================
platform linux -- Python 3.10.4, pytest-7.1.3, pluggy-1.0.0
[...]
collected 1 item                                                               

tests/test_cli.py .                                                      [100%]

============================== 1 passed in 0.01s ===============================
```

Mas ter que rodar o pytest intalado globalmente não é tão bom assim. Prefiro utilizar
a ferramenta tox, que vai automatizar esses testes serem executados em vários ambientes
diferentes. Mas antes disso é preciso empacotar a nossa aplicação utilizando o
`setuptoopls`.

### Empacotando usando setuptools

O `setuptools` precisa de alguns arquivos de configuração para empacotar
o aplicativo, são eles: `setup.py`, `setup.cfg` e `pyproject.toml`. Após criar eles
na raiz do projeto, é preciso preencher o `setup.py` e `pyproject.toml` com valores
padrão:

```python
from setuptools import setup

setup()
```

```ini
[build-system]
build-backend = "setuptools.build_meta"
requires = ["setuptools", "wheel"]
```

Agora vou criar o arquivo principal, `setup.cfg`:

```ini
[metadata]
name = greet
version = 0.0.1

[options]
packages = cli

[options.entry_points]
console_scripts =
    greet = cli.main:main
```

Agora basta usar o comando build:

```
$ python -m build
* Creating virtualenv isolated environment...
* Installing packages in isolated environment... (setuptools, wheel)
* Getting dependencies for sdist...
[...]
Successfully built greet-0.0.1.tar.gz and greet-0.0.1-py3-none-any.whl
```

Temos o nosso pacote na pasta `dist/`. Para instalar basta usar pip:

```
$ pip install dist/greet-0.0.1-py3-none-any.whl 
Processing ./dist/greet-0.0.1-py3-none-any.whl
Installing collected packages: greet
Successfully installed greet-0.0.1
```

Agora a aplicação foi instalada, basta usar:

```
$ greet rafael
hello rafael
```

Finalmente posso usar o tox para rodar os testes.

### Rodando testes com tox

Adicionando o arquivo `tox.ini` na raiz do projeto com uma configuração bem básica:

```ini
[tox]
envlist = py310

[testenv]
deps = pytest
commands =
    pytest {posargs:tests}
```

Para rodar os testes em todos os ambientes disponíveis na sua máquina:

```
$ tox --skip-missing-interpreters
```

Caso eu queira rodar um teste especifico em um ambiente especifico:

```
$ tox -e py310 -- tests/test_cli.py::test_hello_world_cli
```

### Black, mypy e pre-commit

Também vou adicionar algumas ferramentas para ajudar no desenvolvimento. Mais
um arquivo de configuração na raiz do projeto chamado `.pre-commit-config.yaml`:

```yaml
repos:
-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v3.2.0
    hooks:
    -   id: trailing-whitespace
    -   id: end-of-file-fixer
    -   id: check-yaml
-   repo: https://github.com/psf/black
    rev: 22.10.0
    hooks:
    -   id: black
-   repo: https://github.com/pre-commit/mirrors-mypy
    rev: v0.982
    hooks:
    -   id: mypy
        additional_dependencies: [types-all]
        exclude: tests
```

A ideia é usar o `pre-commit` para sempre utilizar o `mypy` e `black` antes de
cada commit. Para iniciar no projeto basta:

```
$ pre-commit install
pre-commit installed at .git/hooks/pre-commit
```

Caso queira executar sem fazer fazer commit basta usar:

```
$ pre-commit run --all-files
```

### Conclusão

Esse foi o esqueleto básico de uma aplicação de linha de comando usando python.
O código fonte mostrado nesse post se encontra no
[meu github](https://github.com/rafaellcoellho/exemplo-cli-python).

### Referências

+ [tutorial do argparse]
+ [testar stdout usando pytest]
+ [documentação do tox]
+ [tutorial explicando como empacotar aplicativos python]
+ [página da documentação do build]
+ [documentação do pre-commit]


[tutorial do argparse]: https://docs.python.org/3/library/argparse.html
[testar stdout usando pytest]: https://docs.pytest.org/en/7.1.x/how-to/capture-stdout-stderr.html
[documentação do tox]: https://tox.wiki/en/latest/
[tutorial explicando como empacotar aplicativos python]: https://pybit.es/articles/how-to-package-and-deploy-cli-apps/
[página da documentação do build]: https://pypa-build.readthedocs.io/en/stable/
[documentação do pre-commit]: https://pre-commit.com/
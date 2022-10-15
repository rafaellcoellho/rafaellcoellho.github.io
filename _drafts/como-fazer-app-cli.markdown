---
title: "Como criar cli em python"
layout: post
---

Vou escever um aplicativo "hello world" para aprender como escrever, testar e
deployar um aplicativo de linha de comando. 

### Criando aplicação

O python já vem com uma biblioteca para facilitar a criação de aplicações de linha
de comando chamada argparse. Criando o modulo principal:

```
└── cli
    ├── cli.py
    └── __init__.py
```

O arquivo `cli.py` vai conter todo o código:

```python
import argparse


def main(argv=None):
    parser = argparse.ArgumentParser(prog='hello')
    parser.add_argument('name')
    args = parser.parse_args(argv)

    print(f"hello {args.name}")

    return 0


if __name__ == '__main__':
    exit(main())
```

Vou testar se o módulo está funcionando usando a linha de comando:

```
$ python -m cli.main rafael
hello rafael
```

Sucesso! Mas caso a minha aplicação comece a ficar mais complexa não vou querer
testar manualmente todas as possibilidades. Vou criar um teste unitário para
garantir que essa lógica esteja sempre correta.

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

Utilizei a própria função main para passar os argumentos e o pytest também
ajuda a checar o `stdout`:

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
collected 1 item                                                               

tests/test_cli.py .                                                      [100%]

============================== 1 passed in 0.01s ===============================
```

Mas ter que rodar o pytest intalado globalmente não é tão bom assim. Prefiro utilizar
a ferramenta tox, que vai automatizar esses testes serem executados em vários ambientes
diferentes.

### Adicionando tox

TODO

### Referências

+ [tutorial do argparse]
+ [testar stdout usando pytest]

[tutorial do argparse]: https://docs.python.org/3/library/argparse.html
[testar stdout usando pytest]: https://docs.pytest.org/en/7.1.x/how-to/capture-stdout-stderr.html
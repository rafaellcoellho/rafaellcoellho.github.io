---
title: "Makefile como executor de comandos"
date: 2022-02-26 23:34:29
layout: post
---

O make é uma ferramenta de automação de compilação de acordo com a [wikipedia]. 
Mas ela pode ser útil mesmo quando não é preciso compilar nada. É possível 
usar como um executor de comandos.

A vantagem de usar o make para fazer isso é por ser comunmente encontrada em 
istribuições linux. Além de fácil de ler, escrever e manter um makefile para 
esse objetivo.

Um exemplo desse tipo de utilização pode ser encontrado no projeto [pydantic]. 
Vou usar como exemplo o [makefile deles na versão 1.9.0]:

```
.DEFAULT_GOAL := all
isort = isort pydantic tests
black = black -S -l 120 --target-version py38 pydantic tests

[...]

.PHONY: lint
lint:
	flake8 pydantic/ tests/
	$(isort) --check-only --df
	$(black) --check --diff

[...]

.PHONY: mypy
mypy:
	mypy pydantic

[...]

.PHONY: test
test:
	pytest --cov=pydantic

.PHONY: testcov
testcov: test
	@echo "building coverage html"
	@coverage html

[...]

.PHONY: all
all: lint mypy testcov

[...]
```

## Como o make executa uma regra

Considerando que estou na pasta do projeto do pydantic e executo o comando:

```
$ make
```

O make procura a primeira regra no arquivo para começar a executar. Isso é 
chamado de **default goal**.

No makefile do pydantic, ele usa a variável especial chamada `.DEFAULT_GOAL` 
para apontar para qual regra o make deve executar primeiro. Mesmo que as 
regras de lint, mypy e testcov venham antes da regra all elas serão executadas 
depois.

## Como executar uma regra especifica

Caso eu queira executar só a regra de testcov:

```
$ make testcov
```

Nesse caso o make vai executar apenas a regra **testcov** e seu target
com dependencia **test**.

## Targets .PHONY

A sintaxe do `.PHONY` é utilizada para sinalizar que o target não 
é um nome do arquivo. Isso evita um possível conflito com o nome de um 
arquivo real e otimiza performace. A sintaxe:

```
.PHONY nome_da_acao
nome_da_acao: dependecias
  comandos
  ...
```

## Imprimir comando da saída

O comportamento comum do make é que [todo comando executado é enviado para saída padrão]. 
Uma maneira de evitar isso é usar a flag `-s` ou `--silent`:

```
$ make --silent
```

Caso queira que apenas exiba e não execute os comandos, existe as flags 
`-n` or `--just-print`:

```
$ make --just-print
```

## Comando help

Já saindo do exemplo do makefile do pydantic e entrando em outras dicas.

Fuçando na web existem vários exemplos de maneiras de adicionar um comando de 
help automáticamente num makefile. Um dos mais interessantes que encontrei foi 
o [esse](https://gist.github.com/klmr/575726c7e05d8780505a):

```
.PHONY: help
help:
	@echo "$$(tput bold)Comandos:$$(tput sgr0)"
	@echo
	@sed -n -e "/^## / { \
		h; \
		s/.*//; \
		:doc" \
		-e "H; \
		n; \
		s/^## //; \
		t doc" \
		-e "s/:.*//; \
		G; \
		s/\\n## /---/; \
		s/\\n/ /g; \
		p; \
	}" ${MAKEFILE_LIST} \
	| LC_ALL='C' sort --ignore-case \
	| awk -F '---' \
		-v ncol=$$(tput cols) \
		-v indent=19 \
		-v col_on="$$(tput setaf 6)" \
		-v col_off="$$(tput sgr0)" \
	'{ \
		printf "%s%*s%s ", col_on, -indent, $$1, col_off; \
		n = split($$2, words, " "); \
		line_length = ncol - indent; \
		for (i = 1; i <= n; i++) { \
			line_length -= length(words[i]) + 1; \
			if (line_length <= 0) { \
				line_length = ncol - indent - length(words[i]) - 1; \
				printf "\n%*s ", -indent, " "; \
			} \
			printf "%s ", words[i]; \
		} \
		printf "\n"; \
	}' \
	| more $(shell test $(shell uname) == Darwin && echo '--no-init --raw-control-chars')
```

Assim podemos popular o comando de help da seguinte maneira: 

```
.PHONY: oi-mundo
## Mostra mensagem de oi mundo no terminal
oi-mundo:
	@echo "oi mundo"


.PHONY: nao-aparecer-no-help
# Essa não vai aparecer no help
nao-aparecer-no-help:
	@echo "Não mostra no help"
```

Temos então o resultado:

```
$ make help
Comandos:

oi-mundo            Mostra mensagem de oi mundo no terminal
```

## Dividindo comandos em arquivos diferentes

As vezes precisamos dividir os comandos entre arquivos diferentes. O make tem 
uma opção onde é possível passar qual arquivo vai ser lido pelo comando:

```
$ make -f NomeDoArquivo.mk
```

Por convenção usamos a extensão `.mk` para outros arquivos de makefile no mesmo projeto.

Nesse [projeto de exemplo] resolvi separar o executor de comandos do makefile 
que compila os arquivos de código fonte. Considerando a estrutura de pastas:

```
├── src
│   └── main.c
├── Makefile
└── MakeGb.mk
```

No makefile principal, criei o comando:

```
gb:
	@ make -f MakeGb.mk all
```

Então ao dar um `make gb`, o make apenas compila os arquivos de código de fonte.

## Referências 
+ [docs da ferramenta make](https://www.gnu.org/software/make/manual/)
+ [pydantic]
+ [como make processa um makefile](https://www.gnu.org/software/make/manual/make.html#How-Make-Works)
+ [todo comando executado é enviado para saída padrão]

[wikipedia]: https://en.wikipedia.org/wiki/Make_(software)
[makefile deles na versão 1.9.0]: https://github.com/samuelcolvin/pydantic/blob/v1.9.0/Makefile
[pydantic]: https://github.com/samuelcolvin/pydantic
[todo comando executado é enviado para saída padrão]: https://www.gnu.org/software/make/manual/make.html#Echoing
[projeto de exemplo]: https://github.com/rafaellcoellho/template-c-gameboy
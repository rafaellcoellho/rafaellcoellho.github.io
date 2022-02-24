---
title: "Makefile para jogo de GameBoy usando GBDK"
layout: post
---

Inspirado pelo Modern Vintage Gamer nesse q
[vídeo](https://www.youtube.com/watch?v=FzPTK91EJY8) resolvi escrever um 
makefile simples para compilar jogos de GameBoy escritos em C usando o GBDK-2020.

Criei um [repositório](https://github.com/rafaellcoellho/template-c-gameboy) 
com um projeto exemplo para usar como template para projetos futuros.

Esse post também vai servir como um tutorial básico de como funciona a 
sintaxe básica de Makefile.

## Variáveis 

Nas primeiras linhas do makefile normalmente são definidas todas as variáveis necessárias 
para escrever as regras para compilar nosso jogo. A sintaxe é a seguinte:

```
NOME_DA_VARIAVEL=valor
```

O nome das variáveis é case sensitive, mas por convensão são sempre nomeadas 
em caixa alta. O valor é uma string. Para utilizar o valor das variáveis é só usar 
`$(NOME_DA_VARIAVEL)`.

Exemplos:

```
NOME_DO_JOGO=foo
ARQUIVO_GB=build/$(NOME_DO_JOGO).gb

DIRETORIO_SOURCES=src
DIRETORIO_OBJ=build
[...]
COMPILADOR=gbdk/bin/lcc

FLAGS_DO_COMPILADOR=-Wa-l -Wl-m -Wf--debug -Wl-y -Wl-w -DUSE_SFR_FOR_REG
```

Dar nomes significativos para variáveis é uma boa ideia.

## Funções builtin

Agora vamos começar a entender algumas funções *builtin* dos makefiles. 
A primeira que aparece é o **wildcard**:

```
ARQUIVOS_C=$(wildcard $(DIRETORIO_SOURCES)/*.c)
```

Como [demonstrado na documentação](https://www.gnu.org/software/make/manual/make.html#Wildcard-Function) 
a sintaxe é `$(wildcard pattern…)`. Essa função vai expandir para um valor 
com nomes dos arquivos que coincidem com o **pattern**, separado por espaço. 

Considerando que a estrutura de pastas seja:

```
├── build
├── src
│   ├── main.c
│   ├── oi_mundo.c
│   └── oi_mundo.h
└── Makefile
```

A variável vai conter o valor:

```
ARQUIVOS_C = src/main.c src/oi_mundo.c
```

Temos a segunda função *buildin* que é o 
[**patsubst**](https://www.gnu.org/software/make/manual/make.html#index-patsubst-1). 
A sintaxe é:

```
$(patsubst pattern,replacement,text)
```

Essa função encontra palavras separadas por espaço em **text** que coincidem 
com o **pattern** e substitui pelo formato do **replacement**. Meio confuso 
mas podemos entender melhor com o exemplo a seguir:

```
ARQUIVOS_OBJ=$(patsubst $(DIRETORIO_SOURCES)/%.c, $(DIRETORIO_OBJ)/%.o, $(ARQUIVOS_C))
```

O objetivo é transformar todos os arquivos de código fonte (.c) em arquivos 
objeto (.o). Então no **pattern** passamos `$(DIRETORIO_SOURCES)/%.c` para 
coincidir com todos os arquivos no **text**, que é o conteúdo da variável 
*ARQUIVOS_C* que vimos anteriormente. O **replacemnt** é o formato que os 
arquivos .o devem ter na pasta build. O resultado seria:

```
ARQUIVOS_OBJ = build/main.o build/oi_mundo.o
```

## Regras

Agora vamos para as **regra** de compilação. Como indicado no 
[manual](https://www.gnu.org/software/make/manual/make.html#Rule-Example) 
temos a sintaxe:

```
targets : prerequisites
  recipe
  …
```

+ **targets**: Normalmente é o nome do arquivo que é resultado dessa regra. 
Mas também pode ser uma ação;
+ **prerequisites**: Arquivos de entrada ou ser outro 
**target**;
+ **recipe**: Comando para ser interpretados pelo shell. Por padrão o 
make usa o `/bin/sh` para executalos.

A primeira regra a ser executada é:

```
all: criar_diretorio_build $(ARQUIVO_GB)
```

O primeiro pré-requisito é `criar_diretorio_build`, logo em seguida temos o 
arquivo do jogo. nesse exemplo *ARQUIVO_GB* é `build/foo.gb`.

Pra criar o *ARQUIVO_GB* na primeira regra, ele procura 
outra regra para gerar esse arquivo e encontra:

```
$(ARQUIVO_GB): $(ARQUIVOS_OBJ)
	$(COMPILADOR) $(FLAGS_DO_COMPILADOR) -o $(ARQUIVO_GB) $(ARQUIVOS_OBJ)
```

Podemos ler essa regra assim: para gerar o *ARQUIVO_GB*, passamos como 
entrada *ARQUIVOS_OBJ* e executamos o comando 
`$(COMPILADOR) $(FLAGS_DO_COMPILADOR) -o $(ARQUIVO_GB) $(ARQUIVOS_OBJ)`.

Para facilitar o entendimento, vamos traduzir todas as variáveis:

```
foo.gb: build/main.o build/oi_mundo.o
  gbdk/bin/lcc -Wa-l -Wl-m -Wf--debug -Wl-y -Wl-w -DUSE_SFR_FOR_REG -o build/foo.gb  build/main.o  build/oi_mundo.o
```

## Variáveis automáticas em regras

Então antes de continuar para a próxima regra, vamos entender 
primeiro o significado de
[**variáveis automáticas**](https://www.gnu.org/software/make/manual/make.html#Automatic-Variables).

No exemplo a seguir vemos o uso de **$@** e **$<**:

```
objeto.o: requisito.c
  $(COMPILADOR) $(FLAGS_DO_COMPILADOR) -o $@ $<
``
Sendo o seu significado:

+ **$@**: O valor do target (objeto.o);
+ **$<**: O valor do pré-requisito (requisito.c).

## Regras Implícitas

Se fossemos executar os comandos de compilação manualmente, nesse ponto 
iriamos executar os seguintes comandos:

```
$ gbdk/bin/lcc -Wa-l -Wl-m -Wf--debug -Wl-y -Wl-w -DUSE_SFR_FOR_REG -c -o build/main.o src/main.c
$ gbdk/bin/lcc -Wa-l -Wl-m -Wf--debug -Wl-y -Wl-w -DUSE_SFR_FOR_REG -c -o build/oi_mundo.o src/oi_mundo.c
```

Então o objetivo aqui é gerar uma comando para cada arquivo objeto sendo 
construido. 

Vamos partir de regras mais simples para atingir esse objetivo:

```
build/main.o: src/main.c
  gbdk/bin/lcc -Wa-l -Wl-m -Wf--debug -Wl-y -Wl-w -DUSE_SFR_FOR_REG -c -o $@ $<

build/oi_mundo.o: src/oi_mundo.c
  gbdk/bin/lcc -Wa-l -Wl-m -Wf--debug -Wl-y -Wl-w -DUSE_SFR_FOR_REG -c -o $@ $<
```

Sabendo disso agora temos que pensar que a regra apresentada anteriormente 
é uma maneira reduzida de escrever essas duas regras mais simples. Ou seja, 
estamos usando a inteligência da sintaxe do makefile para poder construir
qualquer quantidade arbritária de arquivos que criamos no nosso projeto. 
Com a vantagem do make não executar regras novamente se os pré-requisitos 
não mudaram desde a ultima vez, deixando esse processo muito mais eficiente.

Mas como isso exatamente acontece? Através do conseito de 
[**regras implícitas**](https://www.gnu.org/software/make/manual/html_node/Implicit-Rules.html). 
Um bom tutorial de como isso funciona está [aqui](https://rebelsky.cs.grinnell.edu/musings/cnix-make-implicit-rules).

Basicamente na nossa regra mais complexa anterior, substituindo as variáveis:

```
build/%.o: src/%.c
  gbdk/bin/lcc -Wa-l -Wl-m -Wf--debug -Wl-y -Wl-w -DUSE_SFR_FOR_REG -c -o  $@ $<
```

Estamos dizendo que para cada arquivo na pasta build que coincide com o 
pattern de `%.o`, vamos ter como pré-requisito um outro arquivo na pasta src 
que coincide com o pattern `%.c` e vamos executar o comando 
`gbdk/bin/lcc -Wa-l -Wl-m -Wf--debug -Wl-y -Wl-w -DUSE_SFR_FOR_REG -c -o  $@ $<` 
substituindo o target (`$@`) e pré-requisito (`$<`).

```
$(DIRETORIO_OBJ)/%.o: $(DIRETORIO_SOURCES)/%.c
	$(COMPILADOR) $(FLAGS_DO_COMPILADOR) -c -o $@ $<
```

## Regra como executor de comandos

Finalmente temos as duas ultimas regras:

```
criar_diretorio_build:
	@ mkdir -p build

clean:
	@ rm -rf $(DIRETORIO_OBJ)/*
```

Por padrão o makefile [imprime na saída](https://www.gnu.org/software/make/manual/make.html#Echoing) 
todo comando executado em uma regra, mas podemos usar `@` no começo de qualquer 
comando para que evitar isso.

A regra criar_diretorio_build já foi explicada anteriormente. Já a regra **clean** 
é uma regra bastante comum em makefiles, que por convenção apaga e limpa o ambiente.
Nessa situação isso significa apagar todos os arquivos na pasta de build.

## Makefile completo

```
NOME_DO_JOGO=foo
ARQUIVO_GB=build/$(NOME_DO_JOGO).gb

DIRETORIO_SOURCES=src
DIRETORIO_OBJ=build

ARQUIVOS_C=$(wildcard $(DIRETORIO_SOURCES)/*.c)
ARQUIVOS_OBJ=$(patsubst $(DIRETORIO_SOURCES)/%.c, $(DIRETORIO_OBJ)/%.o, $(ARQUIVOS_C))

COMPILADOR=gbdk/bin/lcc

FLAGS_DO_COMPILADOR=-Wa-l -Wl-m -Wf--debug -Wl-y -Wl-w -DUSE_SFR_FOR_REG

all: criar_diretorio_build $(ARQUIVO_GB)

$(ARQUIVO_GB): $(ARQUIVOS_OBJ)
	$(COMPILADOR) $(FLAGS_DO_COMPILADOR) -o $(ARQUIVO_GB) $(ARQUIVOS_OBJ)

$(DIRETORIO_OBJ)/%.o: $(DIRETORIO_SOURCES)/%.c
	$(COMPILADOR) $(FLAGS_DO_COMPILADOR) -c -o $@ $<

criar_diretorio_build:
	@ mkdir -p build

clean:
	@ rm -rf $(DIRETORIO_OBJ)/*
```

## Usando

## Referências 
+ [video do MVG sobre homebrew para GameBoy](https://www.youtube.com/watch?v=FzPTK91EJY8)
+ [docs do gbdk-2020](https://gbdk-2020.github.io/gbdk-2020/docs/api/)
+ [docs da ferramenta make](https://www.gnu.org/software/make/manual/)
+ [tutorial de como funciona regra implícita no make](https://rebelsky.cs.grinnell.edu/musings/cnix-make-implicit-rules)
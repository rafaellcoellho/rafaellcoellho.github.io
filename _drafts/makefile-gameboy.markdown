---
title: "Aprendendo o básico sobre makefiles"
layout: post
---

Ao decorrer dos anos eu já precisei usar/escrever makefiles várias vezes. 
Mas eu nunca entendi bem a sintáxe e nunca parei para estudar.
Decidi escrever esse post para aprender melhor.

Nada melhor para aprender do que encontrar uma motivação em um projeto pessoal.
Inspirado pelo Modern Vintage Gamer nesse 
[vídeo](https://www.youtube.com/watch?v=FzPTK91EJY8) resolvi escrever um 
makefile simples para compilar jogos de GameBoy escritos em C usando o GBDK-2020.

Criei um [repositório](https://github.com/rafaellcoellho/template-c-gameboy) 
com um projeto exemplo para usar como template para outros projetos.

## Variáveis 

Nas primeiras linhas do makefile é definido as variáveis necessárias 
para configurar as regras de compilação e evitar reescrever a mesma 
coisa várias vezes.

A sintaxe de uma variável é a seguinte:

```
NOME_DA_VARIAVEL = valor
```

O nome das variáveis é case sensitive, mas por convensão são sempre nomeadas 
em caixa alta. O valor é uma string. Para utilizar o valor das variáveis é só usar 
`$(NOME_DA_VARIAVEL)`.

Comecei criando algumas variáveis de configuração. Os nomes são auto-explicativos:

```
NOME_DO_JOGO=foo
ARQUIVO_GB=build/$(NOME_DO_JOGO).gb

DIRETORIO_SOURCES=src
DIRETORIO_OBJ=build
[...]
COMPILADOR=gbdk/bin/lcc

FLAGS_DO_COMPILADOR=-Wa-l -Wl-m -Wf--debug -Wl-y -Wl-w -DUSE_SFR_FOR_REG
```

## Funções builtin

Em seguida é preciso definir variáveis que leiam quais arquivos 
existe no projeto. Para isso utilizarei funções *builtin* dos makefiles. 
A primeira é o **wildcard**:

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

A segunda função *buildin* que utilizei foi a 
[**patsubst**](https://www.gnu.org/software/make/manual/make.html#index-patsubst-1). 
A sintaxe é:

```
$(patsubst pattern,replacement,text)
```

Essa função encontra palavras separadas por espaço em **text** que coincidem 
com o **pattern** e substitui pelo formato do **replacement**. Meio confuso 
mas é possível entender melhor com o exemplo a seguir:

```
ARQUIVOS_OBJ=$(patsubst $(DIRETORIO_SOURCES)/%.c, $(DIRETORIO_OBJ)/%.o, $(ARQUIVOS_C))
```

O objetivo é transformar todos os arquivos de código fonte (.c) em arquivos 
objeto (.o). Então no **pattern** passamos `$(DIRETORIO_SOURCES)/%.c` para 
coincidir com todos os arquivos no **text**, que é o conteúdo da variável 
*ARQUIVOS_C* que criei anteriormente. O **replacemnt** é o formato que os 
arquivos .o devem ter na pasta build. O resultado seria:

```
ARQUIVOS_OBJ = build/main.o build/oi_mundo.o
```

## Regras

Agora vou começar a escrever as **regra de compilação**. Como indicado no 
[manual](https://www.gnu.org/software/make/manual/make.html#Rule-Example) 
cada regra segue a sintaxe:

```
targets : prerequisites
  recipe
  …
```

+ **targets**: Normalmente é o nome do arquivo que é resultado dessa regra. 
Mas também pode ser uma ação;
+ **prerequisites**: Arquivos de entrada ou outro **target**;
+ **recipe**: Comando para ser interpretados pelo shell. Por padrão o 
make usa o `/bin/sh` ao executar.

A primeira regra a ser é a principal:

```
all: criar_diretorio_build $(ARQUIVO_GB)
```

O primeiro pré-requisito é `criar_diretorio_build`, logo em seguida temos o 
arquivo do jogo.

Pra criar o *ARQUIVO_GB* na primeira regra, o makefile precisa saber como 
gerar esse arquivo. Então escrevi a regra:

```
$(ARQUIVO_GB): $(ARQUIVOS_OBJ)
	$(COMPILADOR) $(FLAGS_DO_COMPILADOR) -o $(ARQUIVO_GB) $(ARQUIVOS_OBJ)
```

Essa regra pode ser lida assim: para gerar o *ARQUIVO_GB*, passo como 
entrada *ARQUIVOS_OBJ* e executa o comando 
`$(COMPILADOR) $(FLAGS_DO_COMPILADOR) -o $(ARQUIVO_GB) $(ARQUIVOS_OBJ)`.

Para facilitar o entendimento, a regra anterior seria o equivalante a escrever isso:

```
foo.gb: build/main.o build/oi_mundo.o
  gbdk/bin/lcc -Wa-l -Wl-m -Wf--debug -Wl-y -Wl-w -DUSE_SFR_FOR_REG -o build/foo.gb  build/main.o  build/oi_mundo.o
```

## Variáveis automáticas em regras

Antes de continuar para a escrever a próxima regra, preciso 
primeiro apresentar duas 
[**variáveis automáticas**](https://www.gnu.org/software/make/manual/make.html#Automatic-Variables): 
**$@** e **$<**.

No exemplo a seguir

```
objeto.o: requisito.c
  $(COMPILADOR) $(FLAGS_DO_COMPILADOR) -o $@ $<
```

O seus significados seria:

+ **$@**: O valor do target (objeto.o);
+ **$<**: O valor do pré-requisito (requisito.c).

## Regras Implícitas

Partindo da ideias de que eu fosse executar os comandos de compilação manualmente, 
ficaria assim:

```
$ gbdk/bin/lcc -Wa-l -Wl-m -Wf--debug -Wl-y -Wl-w -DUSE_SFR_FOR_REG -c -o build/main.o src/main.c
$ gbdk/bin/lcc -Wa-l -Wl-m -Wf--debug -Wl-y -Wl-w -DUSE_SFR_FOR_REG -c -o build/oi_mundo.o src/oi_mundo.c
```

Então o objetivo aqui é criar uma regra para gerar um comando para cada 
arquivo objeto sendo construido. 

Para ser mais didático, vou partir dessas regras mais simples:

```
build/main.o: src/main.c
  gbdk/bin/lcc -Wa-l -Wl-m -Wf--debug -Wl-y -Wl-w -DUSE_SFR_FOR_REG -c -o $@ $<

build/oi_mundo.o: src/oi_mundo.c
  gbdk/bin/lcc -Wa-l -Wl-m -Wf--debug -Wl-y -Wl-w -DUSE_SFR_FOR_REG -c -o $@ $<
```

O problema dessa abordagem é que ao adicionar um novo arquivo terei que 
escrever mais um regra. Para evitar isso posso usar a sintaxe do makefile 
para compilar qualquer quantidade arbritária de arquivos. Com a vantagem 
do make não executar regras novamente se os pré-requisitos não mudaram 
desde a ultima vez, deixando esse processo muito mais eficiente.

Mas como fazer isso? Através do conseito de 
[**regras implícitas**](https://www.gnu.org/software/make/manual/html_node/Implicit-Rules.html). 
Um bom tutorial de como isso funciona está [aqui](https://rebelsky.cs.grinnell.edu/musings/cnix-make-implicit-rules).

Basicamente posso substituir pelos patterns e variáveis automáticas:

```
build/%.o: src/%.c
  gbdk/bin/lcc -Wa-l -Wl-m -Wf--debug -Wl-y -Wl-w -DUSE_SFR_FOR_REG -c -o  $@ $<
```

Essa regra esta definindo que para cada arquivo na pasta build que coincide 
com o pattern de `%.o`, vamos ter como pré-requisito um outro arquivo na 
pasta src que coincide com o pattern `%.c` e vamos executar o comando 
`gbdk/bin/lcc -Wa-l -Wl-m -Wf--debug -Wl-y -Wl-w -DUSE_SFR_FOR_REG -c -o  $@ $<` 
substituindo o target (`$@`) e pré-requisito (`$<`).

Por fim substituindo pelas variáveis já criadas no inicio:

```
$(DIRETORIO_OBJ)/%.o: $(DIRETORIO_SOURCES)/%.c
	$(COMPILADOR) $(FLAGS_DO_COMPILADOR) -c -o $@ $<
```

## Regra como executor de comandos

Mas ainda falta algumas regras extras:

```
criar_diretorio_build:
	@ mkdir -p build

clean:
	@ rm -rf $(DIRETORIO_OBJ)/*
```

Por padrão o makefile [imprime na saída](https://www.gnu.org/software/make/manual/make.html#Echoing) 
todo comando executado em uma regra, mas usei o `@` no começo do 
comando para evitar isso.

A regra `criar_diretorio_build` é auto-explicativa. Já a regra **clean** 
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

## Referências 
+ [video do MVG sobre homebrew para GameBoy](https://www.youtube.com/watch?v=FzPTK91EJY8)
+ [docs do gbdk-2020](https://gbdk-2020.github.io/gbdk-2020/docs/api/)
+ [docs da ferramenta make](https://www.gnu.org/software/make/manual/)
+ [tutorial de como funciona regra implícita no make](https://rebelsky.cs.grinnell.edu/musings/cnix-make-implicit-rules)
---
title: "Instalando e atualizando DBeaver no fedora 36"
date: 2022-12-22 21:00:00
layout: post
---

Primeiramente vou baixar o pacote RPM da última versão na [página de download
do DBeaver]. Com o arquivo já no computador, para instalar fazemos:

```
$ sudo rpm -Uv Downloads/dbeaver-ce-22.3.0-stable.x86_64.rpm
```

Onde os argumentos significam:

+ **-U**: faz a atualização do pacote, mas se não existir ainda, instala;
+ **-v**: ativa o modo *verbose* da saída dos comandos do rpm;

Quando sair uma nova versão, basta ir ao site do DBeaver e usar o mesmo comando
com o novo pacote.


### Referências

- [página de download do DBeaver]

[página de download do DBeaver]: https://dbeaver.io/download/
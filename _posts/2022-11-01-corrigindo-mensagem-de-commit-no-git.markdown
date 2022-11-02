---
title: "Corrigindo mensagem de commit no git"
date: 2022-11-01 23:52:15
layout: post
---

Eu tenho os seguintes commits na minha branch:

```
$ git log --oneline
28b5e09 separa erros e mensagens em arquivos separados do main
987ad20 remove import não utilizado em testes
0fd492b adiciona classe de erro, erro de descrição vazia e try catch no main
936cfd1 remove função de pasear comandos, corrige if da função de comandos
e63336d corrige imports do arquivo de erros
```

O que eu preciso fazer é corrigir o erro no commit `936cfd1`, mudar a
palavra "pasear" para "parsear". Se esse tivessse sido o ultimo commit
seria possível apenas usar o seguinte comando:

```
$ git commit --amend
```

Onde o git abriria um editor com o texto antigo, eu corrigiria e salvaria.
Mas esse não é o caso, vou precisar de uma ferramenta mais poderosa de edição
do histórico:

```
$ git rebase -i HEAD~2^
```

- **rebase**: é o comando que muda a "base" da nossa branch;
- **-i**: vem de `interactive`, essa opção faz com que seja possível instruir o git a parar
em cada commit no rebase para realizar alguma ação específica;
- **HEAD~2^**: essa é uma notação para selecionar uma série de commits, nesse caso
os ultimos 4 commits;

O comando inteiro quer dizer o seguinte: `selecione os ultimos 4 commits da minha branch
para um rebase interativo`. Após executar o git vai abrir o editor para descrevermos o que
queremos fazer com esses commits:

```
pick 936cfd1 remove função de pasear comandos, corrige if da função de comandos
pick 0fd492b adiciona classe de erro, erro de descrição vazia e try catch no main
pick 987ad20 remove import não utilizado em testes
pick 28b5e09 separa erros e mensagens em arquivos separados do main

# Rebase e63336d..28b5e09 onto e63336d (4 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
[...]
```

Aqui eles aparecem na ordem que serão executados, por isso o primeiro da lista
é o commit mais antigo. Vou usar o comando `edit` no commit `936cfd1`. Após
editar e salvar o git me mostra as opções:

```
Stopped at 936cfd1...  remove função de pasear comandos, corrige if da função de comandos
You can amend the commit now, with

  git commit --amend 

Once you are satisfied with your changes, run

  git rebase --continue
```

Vou dar o `git commit --amend`, mudar a o texto, e em seguida executar
`git rebase --continue` como indicado. Ao verificarmos os commits temos:

```
$ git log --oneline
337d10b separa erros e mensagens em arquivos separados do main
3025858 remove import não utilizado em testes
f733909 adiciona classe de erro, erro de descrição vazia e try catch no main
a4ecd76 remove função de parsear comandos, corrige if da função de comandos
```

Sucesso! Importante reparar que o hash dos commits mudou, pois o git destruiu os
commits e recriou eles. Com essa ferramenta não é possível só corrigir a mensagem
de commit, mas também o conteúdo do commit. Aprender os outros commandos do `rebase -i`
pode trazer muitas ferramentas úteis.

Caso já tenha dado push nos commits antes de fazer essa correção, será preciso usar
`git push -f origin branch`. Então é muito importante ter cuidado com esse tipo
de alteração caso esteja trabalhando com outras pessoas na mesma branch, o ideal
seria fazer esse rebase antes de dar push.

### Referências

+ [capítulo do pro git sobre reescrever histórico de commits]
+ [capítulo do pro git sobre como selecionar commits]
+ [capítulo do pro git sobre rebase]

[capítulo do pro git sobre reescrever histórico de commits]: https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History
[capítulo do pro git sobre como selecionar commits]: https://git-scm.com/book/en/v2/Git-Tools-Revision-Selection
[capítulo do pro git sobre rebase]: https://git-scm.com/book/en/v2/Git-Branching-Rebasing
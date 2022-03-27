---
title: "Liberando espaço no meu linux"
date: 2022-03-27 18:42:00
layout: post
---

Fui checar quanto meu ssd ainda tem disponível:

```
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
[...]
/dev/nvme0n1p4  220G  190G   20G  91% /
[...]
/dev/sda1       916G  213G  657G  25% /home/rafaellcoellho/External
```

Só 20GB sobrando? Preciso melhorar isso.

Esse post vai ser sobre o processo de liberar espaço no meu linux, para 
quando no futuro acontecer a mesma coisa poderei partir do mesmo lugar.

Achei uma [resposta no StackOverflow] sobre como ir gradualmente olhando o 
tamanho das pastas até encontra o culpado.

## Investigando

Executando primeiramente:

```
$ sudo du -cha --max-depth=1 / | grep -E "M|G"
[...]
282G	/home
[...]
406G	/
406G	total
```

Meu ssd está usando 190GB, como que no total tem 406GB? 

Deve ser em decorrencia do meu hd externo estar montado numa pasta 
dentro do `\home`. Olhando no [manual do du] encontrei a opção `-x`:

```
-x, --one-file-system
      skip directories on different file systems
```

Perfeito, agora é só adicionar a flag:

```
$ sudo du -chax --max-depth=1 / | grep -E "M|G"
5,0G	/opt
2,5M	/@System.solv
70G	/home
42M	/etc
100G	/var
15G	/usr
738M	/root
190G	/
190G	total
```

Agora sim! 190GB é exatamente o valor utilizado. Então a pasta mais suspeita é 
o `\var`. Intessante, vou continuar rodando o comando até encontrar alguma coisa.

```
$ sudo du -chax --max-depth=1 /var/lib | grep -E "M|G"
3,2M	/var/lib/gdm
301M	/var/lib/mongo
34M	/var/lib/sss
1,1M	/var/lib/PackageKit
280K	/var/lib/NetworkManager
1,5G	/var/lib/snapd
195M	/var/lib/rpm
27G	/var/lib/flatpak
11M	/var/lib/dnf
68G	/var/lib/docker
13M	/var/lib/texmf
28M	/var/lib/selinux
205M	/var/lib/mlocate
97G	/var/lib
97G	total
```

Parece que encontramos um bom candidato a ser o culpado. O docker sozinho está 
ocupando 60G. Se for possível diminuir 80% desse valor já seria o suficiente 
para fazer meu ssd respirar por mais alguns meses. 

A primeira coisa que pensei foi rodar `docker system prune`. Mas fiquei preocupado 
em apagar alguma coisa que eu uso no meu dia a dia de trabalho. Preciso garantir 
que ele não apague os containers e volumes que eu realmente uso. 

```
$ docker system df
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          24        5         13.69GB   12.23GB (89%)
Containers      6         0         3.793GB   3.793GB (100%)
Local Volumes   221       5         50.62GB   50.57GB (99%)
Build Cache     0         0         0B        0B
```

Então vamos limpar esses volumes e imagens não utilizados:

```
$ docker volume prune                                                                             
WARNING! This will remove all local volumes not used by at least one container.
[...]

Total reclaimed space: 50.57GB

$ docker image prune
WARNING! This will remove all dangling images.
[...]

Total reclaimed space: 6.702GB
```

Yay. Acho que ja é mais do que o suficiente pela limpeza de hoje.

## Referências 
+ [resposta no StackOverflow]
+ [docker image prune]
+ [docker volume prune]
+ [manual do du]

[resposta no StackOverflow]: https://askubuntu.com/questions/911865/no-more-disk-space-how-can-i-find-what-is-taking-up-the-space
[docker image prune]: https://docs.docker.com/engine/reference/commandline/image_prune/
[docker volume prune]: https://docs.docker.com/engine/reference/commandline/volume_prune/
[manual do du]: https://man7.org/linux/man-pages/man1/du.1.html
---
title: "Anotações da atualização do fedora 33 para o 36"
layout: post
---

Sempre a mesma história, resolvo formatar o computador e sempre
esqueço como fazer as atividades mais básicas de configuração do
linux. O objetivo desse post é anotar as atividades mais comuns
para que em uma próxima vez seja um processo mais rápido e eficaz.

### Antes de formatar

- Anotar programas com interface gráfica usados frequentemente;
- Ir até a pasta `~/.ssh` e salvar todos os arquivos lá
presentes (salvar no drive ou algo assim);
- Atualizar dotfiles do vim/neovim, tmux, zsh etc;

### Formatando

Vou deixar aqui o link para os links que normalmente uso:

- [dual boot]
- [apenas linux, mas com /home em outro hd]

### Após formatar

#### configurar chaves SSH

Criar pasta `.ssh` em `$HOME`:

```
mkdir ~/.ssh
```

Mover chaves para `~/.ssh`:

```
mv chaves/* ~/.ssh
```

Iniciar `ssh-agent` no sistema

```
eval "$(ssh-agent -s)"
```

Adicionar chave privada ao `ssh-agent`

```
ssh-add ~/.ssh/id_ed25519
```

#### configurar icones e tema

Instala os pacotes de icones e tema:

```
sudo dnf install papirus-icon-theme materia-gtk-theme
```

Após isso usando o GNOME Tweaks é só mudar os temas.

#### configurar zsh e oh-my-zsh

Instala os pacotes do shell:

```
sudo dnf install zsh
```

Muda o shell do sistema para o zsh:

```
chsh -s $(which zsh)
```

Após abrir outro terminal, temos que instalar

### Referências

+ [tutorial sobre ssh do github]

[dual boot]: https://www.youtube.com/watch?v=6D6L9Wml1oY
[apenas linux, mas com /home em outro hd]: https://www.youtube.com/watch?v=DpwANhOJ1Ug 
[tutorial sobre ssh do github]: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#adding-your-ssh-key-to-the-ssh-agent

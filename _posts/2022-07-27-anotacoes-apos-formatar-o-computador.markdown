---
title: "Anotações após formatar o computador"
date: 2022-07-26 23:59:14
layout: post
---

Sempre a mesma história, resolvo formatar o computador e sempre
esqueço quais atividades preciso executar na configuração do novo linux.
O objetivo desse post é listar as atividades mais comuns para que
na próxima vez seja um processo mais rápido e eficaz.

### Antes de formatar

- Anotar programas com interface gráfica usados frequentemente;
- Ir até à pasta `~/.ssh` e salvar todos os arquivos 
presentes (salvar no drive ou algo assim);
- Atualizar repo pessoal dos dotfiles do vim/neovim, tmux, zsh etc;
- Exportar favoritos dos navegadores;
- Checar se não existe nenhum código não salvo nos repositórios locais.

### Formatando

Vou deixar aqui o link para os links de tutoriais que normalmente uso:

- [dual boot]
- [apenas linux, mas com /home em outro hd]

### Após formatar

- [configurar chaves ssh];
- customizar pacote de ícones para [papirus];
- customizar tema das janelas para [um que usa material design];
- instalar e configurar [tmux], vim/neovim, zsh e [oh-my-zsh];
- mudar tema do oh-my-zsh para [starship];
- instalar [fonte hack do nerdfonts] e usar no emulador de terminal;
- configurar email, usuário e editor de commits no git;
- [instalar docker e docker-compose];
- instalar asdf e configurar pelo menos duas versões das linguagens de
node, python, rust, ruby e golang.

Acredito ser isso. Em futuros formatações posso escrever mais um post como esse
para atualizar esse processo.

[dual boot]: https://www.youtube.com/watch?v=6D6L9Wml1oY
[apenas linux, mas com /home em outro hd]: https://www.youtube.com/watch?v=DpwANhOJ1Ug 
[configurar chaves ssh]: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#adding-your-ssh-key-to-the-ssh-agent
[papirus]: https://github.com/PapirusDevelopmentTeam/papirus-icon-theme
[um que usa material design]: https://github.com/nana-4/materia-theme
[tmux]: https://rafaellcoellho.github.io/2022/06/08/aprendizado-vim-tmux.html
[oh-my-zsh]: https://github.com/ohmyzsh/ohmyzsh
[starship]: https://github.com/spaceship-prompt/spaceship-prompt#oh-my-zsh
[fonte hack do nerdfonts]: https://www.nerdfonts.com/font-downloads
[instalar docker e docker-compose]: https://docs.docker.com/engine/install/fedora/

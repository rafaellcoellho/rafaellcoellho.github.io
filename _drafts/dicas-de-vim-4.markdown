---
title: "[aprendizado vim] tmux"
layout: post
---

Na minha caminhada em aprender a usar o vim é comum ver as pessoas
recomendando a utilização de outro programa chamado tmux. Ele é um
multiplexador de terminais, ou seja, posso abrir vários terminais
dentro dele. A ideia é que enquanto eu estiver em um terminal
editando código em outro posso abrir o shell e rodar comandos.

Uma vantagem importante de usar o tmux e não o
gnome terminal/terminator/tilix é que ele guarda a sessão de terminal
que estamos usando. Então caso eu precise fechar a janela, eu posso
simplesmente conectar na mesma sessão novamente e tudo vai estar como
deixei. Outra vantagem é que se precisar editar algo em um servidor
remoto via ssh, eu terei a vantagem de poder abrir vários terminais
facilmente igual eu teria se estivesse usando o terminator ou tilix.

### Conceitos

- **servidor tmux**: armazena todo o estádo os terminais na memória;
- **cliente tmux**: conecta no servidor para mostrar os terminais para
usuário;
- **sessão**: agrupa uma ou mais janelas;
- **janela**: agrupa uma ou mais paineis;
- **painel**: contém um terminal e um programa executando, aparece e uma
janela.

### Arquivo de configuração

Vamos criar o arquivo na `home` do usuário:

```
$ touch ~/.tmux.conf
```

Podemos recarrecar as configurações em uma sessão já iniciada usando
`:source ~/.tmux.conf`.

Seguir as instruções na página do github do [tpm] para instalar o
gerenciador de plugins. A seguir o arquivo completo:

```
# plugins

set -g @plugin 'odedlaz/tmux-onedark-theme'
set -g @plugin 'tmux-plugins/tmux-resurrect'

# mapeia <C-b> para <C-a>

set -g prefix C-a
unbind C-b
bind C-a send-prefix

# consertar delay irritante ao mudar de modo no vim

set -sg escape-time 0

# ativa o teclado dentro do tmux

set-option -g mouse on

# manda o buffer de copia do tmux para clipboard

bind-key -T copy-mode-vi Enter send-keys -X copy-pipe-and-cancel 'xclip -se c -i'

# mode-keys usando bindings do vim

setw -g mode-keys vi

# teclas de direção do vim para mudar painel

bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# tpm
run '~/.tmux/plugins/tpm/tpm'
```

### Usando tmux

Começamos criando uma nova sessão:

```
$ tmux new -s <nome_da_sessao>
```

O argumento `-s` cria a sessão com o nome de blog. Para conectar numa
sessão já existente utilizamos:

```
$ tmux attach -t <nome_da_sessao>
```

Se não sabe o nome das sessões disponiveis no servidor tmux é só usar
o comando:

```
$ tmux ls
```

### Comandos úteis iniciais

Para executar qualquer comando para o tmux em si é preciso usar uma tecla
especial, o padrão é o `<C-b>`. Mas vamos mudar para `<C-a>` porque é mais
fácil de digitar, ver sessão de arquivo de configuração no final.

#### gerais

- `d` -> desconectar da sessão;
- `s` -> mudar de sessão;
- `:kill-session`: encerrar a sessão;
- `:kill-server`: encerrar o servidor.

#### janelas

- `c` -> cria uma nova janela;
- `w` -> ir para janela na lista;
- `n` -> ir para próxima janela;
- `l` -> ir para janela anterior; 
- `&` -> fecha janela atual.

#### paineis

- `%` -> dividir o painel atual horizontalmente;
- `"` -> dividir o painel atual verticalmente;
- `o` -> ir para o próximo painel;
- `;` -> alternar entre o painel atual e o anterior;
- `A-Seta` -> muda tamanho do painel;
- `z` -> ver o painel atual na janela inteira, se precionar novamente
volta ao normal;
- `{` -> move painel atual para esquerda;
- `}` -> move painel atual para direita;
- `C-o` -> gira em sentido anti-horário todos os paineis;
- `x` -> fechar o painel atual.

#### modo para copiar (modo vi)

- `[` -> entrar no modo de copiar;
- `v` -> ativa modo em bloco;
- `Space` -> inicia modo visual de seleção;
- `Enter` -> manda conteúdo copiado para buffer e sai do modo copiar;
- `]` -> cola o que foi salvo no ultimo buffer no local.

#### tmux plugin manager

- `I` -> instala os plugins que foram adicionados na configuração;
- `u` -> atualiza plugins;
- `alt u` -> desinstala os plugins que foram apagados na configuração.

#### tmux ressurect

- `C-s`: salvar sessão;
- `C-r`: carregar sessão salva.

### Referências

+ [documentação oficial do tmux]
+ [post sobre como iniciar a usar tmux]
+ [tpm]
+ [modo copy do tmux]
+ [tmux-ressurect]

[documentação oficial do tmux]: https://github.com/tmux/tmux/wiki
[post sobre como iniciar a usar tmux]: https://linuxize.com/post/getting-started-with-tmux/
[tpm]: https://github.com/tmux-plugins/tpm
[modo copy do tmux]: https://dev.to/iggredible/the-easy-way-to-copy-text-in-tmux-319g
[tmux-ressurect]: https://github.com/tmux-plugins/tmux-resurrect

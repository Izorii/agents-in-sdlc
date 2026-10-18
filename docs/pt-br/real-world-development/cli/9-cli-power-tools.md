---
title: "Lição 9 - Comandos slash no GitHub Copilot CLI"
description: "Explore comandos slash para gerenciar contexto, selecionar modelos, compartilhar sessões e, opcionalmente, delegar tarefas à nuvem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como qualquer boa ferramenta CLI, o GitHub Copilot CLI inclui vários comandos slash. Eles dão acesso a funcionalidades avançadas, informações internas e opções de configuração adicionais. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Vamos explorar alguns outros comandos úteis.

Nesta lição, você vai:

- usar `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa;
- usar `/model` para explorar os modelos disponíveis;
- aprender como `/share` pode exportar ou compartilhar uma sessão;
- explorar comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

## Cenário

Você concluiu o fluxo de trabalho principal do CLI. Agora, vamos explorar alguns recursos adicionais — gerenciamento de contexto, troca de modelos, compartilhamento de sessões e delegação opcional ao [agente de nuvem do Copilot][about-cloud-agent].

## Explore o contexto do Copilot CLI

Ao trabalhar em tarefas maiores ou mais complexas, você pode se aproximar do limite da janela de contexto do modelo. O Copilot CLI compacta a conversa automaticamente quando necessário, mas você também pode inspecionar ou compactar o contexto com comandos slash.

1. Volte ao Codespace e inicie o Copilot CLI na raiz do repositório, caso ainda não esteja aberto.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e como o contexto está dividido entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare o resultado. Talvez não haja uma grande mudança se a conversa já for pequena.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente à medida que a janela fica cheia. Use `/compact` quando quiser escolher o momento. Use `/clear` ou `/new` para iniciar uma conversa limpa ao mudar para uma tarefa sem relação com a atual.

## Escolha o modelo

Modelos diferentes têm pontos fortes diferentes, e cada pessoa desenvolvedora pode ter suas próprias preferências. O Copilot CLI permite listar e selecionar o modelo que você deseja usar.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhe uma sessão

Trabalhar em equipe e compartilhar aprendizados ajuda todos a melhorar o uso de ferramentas de IA. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar um GitHub gist.

1. Digite `/help` e revise as opções de `/share` disponíveis na versão instalada.
2. Se quiser compartilhar esta sessão, escolha o destino adequado — por exemplo, `/share file` para exportar um arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo a alguém ou publicá-lo. As exportações de sessões podem incluir prompts, respostas e detalhes do projeto.

Publicar um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que a equipe não queira compartilhar.

## Opcional: amplie o trabalho ou delegue

O fluxo principal do workshop está concluído. O Copilot CLI também oferece comandos para tarefas maiores:

- `/fleet` pode dividir subtarefas independentes entre subagentes e executá-las em paralelo;
- `/worktree` pode criar um worktree Git isolado para uma tarefa separada;
- `/delegate` pode enviar uma tarefa ao agente de nuvem do Copilot, que trabalha de forma assíncrona e pode abrir uma pull request.

Esses comandos são opcionais, pois podem criar worktrees adicionais ou trabalho remoto. Antes de experimentá-los, inicie uma tarefa nova e com escopo bem definido e revise o resultado pelo fluxo de trabalho normal. Para saber mais sobre trabalho assíncrono com agentes, continue com o [workshop do agente de nuvem][cloud-workshop].

## Resumo e próximos passos

Os comandos slash do Copilot CLI permitem configurá-lo, compartilhar sessões e observar o que acontece nos bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa;
- usou `/model` para explorar os modelos disponíveis;
- aprendeu como `/share` pode exportar ou compartilhar uma sessão;
- explorou comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

Há mais comandos slash e recursos do Copilot CLI para explorar. Vamos encerrar nossa jornada [revisando o que aprendemos][next-lesson] e os próximos passos para continuar aprendendo.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente de nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/

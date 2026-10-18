---
title: "Lição 2 - Adicione avaliações com estrelas: uma melhoria rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cartões de jogos, revise-a em um navegador encaminhado e faça merge da sua primeira pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira alteração no projeto. Vamos manter a tarefa pequena: os jogos já têm uma avaliação por estrelas nos dados, mas os cartões da página inicial ainda não a exibem. Você pedirá ao agente que mostre a avaliação, revisará a alteração e fará merge da sua primeira pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em uma branch de funcionalidade;
- pedir ao agente que faça uma pequena alteração no projeto;
- revisar a alteração com `/diff`;
- executar o aplicativo e confirmar a alteração em um navegador encaminhado;
- abrir e fazer merge da sua primeira pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. No entanto, os cartões de jogos da página inicial mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, você pedirá ao agente que exiba a avaliação existente em cada cartão — uma alteração pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é o espaço em que você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal do CLI usa o repositório e a branch atualmente selecionados no terminal, em vez de criar um worktree dedicado. As conversas salvas permitem retomar a mesma discussão depois, enquanto os arquivos e a branch continuam sendo o estado normal do Git no disco.

Em uma conversa, você verá três elementos: seus prompts e as respostas do agente, a atividade das ferramentas usadas pelo agente ao explorar e editar arquivos e as alterações que você pode inspecionar com `/diff`.

## Inicie uma conversa e solicite a alteração

Vamos iniciar uma nova conversa para implementar o recurso.

1. Volte ao Codespace.
2. Se o terminal usado anteriormente não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver aberto, inicie-o com o comando:

   ```bash
   copilot --yolo
   ```

4. Confirme que uma nova sessão foi iniciada usando o comando slash `/new` e pressionando <kbd>Enter</kbd>.
5. Use o prompt a seguir para solicitar a alteração:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explorará o projeto, localizará os arquivos usados para exibir os detalhes dos jogos e criará o código necessário. Você acabou de adicionar um recurso com o Copilot CLI.

## Revise o diff

Todas as alterações geradas por IA precisam ser revisadas antes do merge, mesmo as pequenas. Vamos examinar as alterações diretamente no Copilot CLI.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme que o cartão do jogo exibe o valor numérico da avaliação quando disponível e `No rating yet` quando `starRating` for `null`.
3. Confirme que os testes abrangem os dois casos.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija eventuais falhas.
5. Quando concluir a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como o Copilot, assim como todas as ferramentas de IA generativa, é probabilístico e não determinístico, o código exato pode variar. Revise o comportamento em vez de esperar uma implementação idêntica.

## Verifique as alterações

Não basta ler o código e presumir que ele funciona. Vamos pedir ao Copilot que inicie o site para que possamos examinar a interface atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cartões dos jogos exibem as avaliações em uma escala de cinco estrelas.
4. Volte ao Copilot e peça que interrompa o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abra e faça merge da sua primeira pull request

Você criou o recurso. Agora, crie uma pull request (PR) para fazer merge do novo código no projeto.

1. Peça ao agente padrão que crie um commit da alteração:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. Ao criar a PR, o Copilot CLI envia o commit existente e mostra a URL da PR.
3. Abra a PR mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando a URL mostrada pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize a cópia local de `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você publicou sua primeira alteração com o GitHub Copilot CLI. Em particular, você:

- iniciou uma conversa focada do Copilot em uma branch de funcionalidade;
- orientou o agente a fazer uma pequena alteração nos cartões dos jogos;
- revisou a alteração com `/diff`;
- executou o aplicativo e confirmou a avaliação por estrelas em um navegador encaminhado;
- abriu e fez merge da sua primeira pull request.

Em seguida, você começará pela issue de filtragem e usará os modos Plan e Autopilot para [criar um recurso maior][next-lesson].

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference

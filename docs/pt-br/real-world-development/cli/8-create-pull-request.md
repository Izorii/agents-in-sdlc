---
title: "Lição 8 - Crie e faça merge da PR do recurso"
description: "Revise a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; em seguida, crie uma PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Chegou a hora de revisá-los juntos e abrir uma pull request. Você fez o merge manualmente da pull request (PR) de avaliação por estrelas; desta vez, deixará o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Normalmente, separaríamos o recurso, a atualização das instruções, a atualização da skill e o agente de QA em algumas PRs. Para simplificar o workshop, mantivemos todo o fluxo de trabalho de filtragem e qualidade em uma única conversa e branch, e todas essas alterações serão incluídas nesta PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge;
- inspecionar todas as alterações do recurso e as evidências de verificação;
- criar a PR de filtragem;
- habilitar o Agent Merge somente depois da revisão e confirmar que a PR foi mesclada.

## Cenário

Ao longo do fluxo de trabalho da filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o restante do trabalho da PR, mantendo a autorização de merge sob o controle de quem desenvolve.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante para integrar uma pull request. Quando habilitado, o Copilot resolve os impedimentos da PR — corrige verificações de integração contínua (CI) com falha, responde a comentários de revisão e faz rebase quando necessário — e depois habilita o merge automático do GitHub, se o repositório permitir.

Até agora, você mesmo selecionou **Merge pull request**. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que está pronto antes de habilitar o Agent Merge.

## Use o Agent Merge para gerenciar a PR

Com todo o código criado, vamos revisá-lo em conjunto, criar a PR e deixar que o Agent Merge gerencie o restante do processo.

1. Volte ao Codespace.
2. Abra o diálogo de agentes digitando `/agent`.
3. Selecione **Default** na lista de opções e pressione <kbd>Enter</kbd>.
4. Crie uma nova PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua da PR. Quando tudo for aprovado, ele fará o merge.
7. Confirme que aparece uma mensagem do Copilot semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório nem permissões ausentes. Se houver um impedimento, leia o motivo informado e conclua manualmente o merge revisado, quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias partes do processo de desenvolvimento, incluindo a geração do código, os testes e a validação e, agora, o processo da pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge;
- inspecionou todas as alterações do recurso e as evidências de verificação;
- criou a PR de filtragem;
- habilitou o Agent Merge somente depois da revisão e confirmou que a PR foi mesclada.

Em seguida, você vai [explorar outros comandos slash úteis do Copilot CLI][next-lesson] para gerenciar contexto, modelos e compartilhamento, além da delegação opcional à nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference

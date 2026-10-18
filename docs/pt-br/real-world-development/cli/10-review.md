---
title: "Lição 10 - Recapitulação e próximos passos"
description: "Recapitule o fluxo de trabalho do Copilot CLI, as duas pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI ao longo de um fluxo de trabalho contínuo na Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem incluída no backlog;
- adicionou avaliações por estrelas, revisou o resultado em um navegador encaminhado e fez merge manualmente da primeira pull request (PR);
- começou pela issue de filtragem, definiu a abordagem no modo Plan, implementou-a no modo Autopilot e revisou-a no modo Interactive;
- orientou o agente com instruções personalizadas, personalizou a skill `quality-checks` existente e a usou para executar testes unitários, lint e verificações de tipos;
- adicionou o servidor Model Context Protocol (MCP) do Playwright e o usou para explorar a filtragem em um navegador real;
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador;
- revisou todas as alterações de filtragem e autorizou o Agent Merge para a PR da filtragem;
- explorou comandos slash para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria branch baseada na versão atualizada de `main`:

1. **Avaliações por estrelas:** exibir o `starRating` existente e um estado explícito para jogos sem avaliação nos cartões.
2. **Filtragem e fluxo de qualidade:** implementar a filtragem, atualizar as instruções e aplicá-las ao recurso, personalizar o relatório `quality-checks`, criar um perfil de QA e incluir os testes correspondentes.

Do planejamento da filtragem até a abertura da PR, você usou a mesma conversa e a mesma branch. Reunimos esse trabalho em uma única PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: testes automatizados, inspeção manual no navegador e exploração do navegador pelo Copilot usando MCP. A skill `quality-checks` executou testes unitários, lint e verificações de tipos e relatou os resultados no novo formato. O QA reuniu esses resultados com uma revisão dos requisitos e da cobertura de testes antes da PR.

Os testes adicionados devem resolver lacunas reais; uma execução de QA que não precise de novos testes também pode estar correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas após as alterações.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot influenciam o trabalho. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações entre conversas e ajuste-as conforme as necessidades da equipe mudarem. As instruções definem padrões, as skills descrevem tarefas repetíveis, os agentes personalizados definem funções especializadas e os servidores MCP conectam ferramentas externas. Revise as alterações reais e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** de acordo com a tarefa. Use **Plan** para analisar uma abordagem antes de implementá-la, **Interactive** para acompanhar mudanças específicas e **Autopilot** para tarefas com escopo bem definido. Escolha um modelo mais rápido para edições rotineiras e um modelo mais capaz para tarefas complexas.

O contexto continua sendo tão importante quanto a infraestrutura. Descrever com clareza *o que* você quer desenvolver, *por que* e *como* influencia significativamente o resultado.

## Mais recursos para explorar

Você concluiu o fluxo de trabalho principal. Há outros recursos do Copilot CLI que vale a pena conhecer:

- `/review` para pedir ao agente de revisão de código que analise as alterações;
- `/rubber-duck` para discutir um problema e obter outra perspectiva;
- `/fleet` para coordenar subtarefas independentes em paralelo;
- `/worktree` para isolar uma tarefa separada;
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximos passos

A melhor maneira de melhorar no uso de qualquer ferramenta é continuar usando-a. Use o Copilot em código de produção, projetos pessoais e naquele pequeno aplicativo que você pensa em criar há anos, mas ainda não começou. Compartilhe seus aprendizados com a equipe e aprenda com os dela. E, como sempre, explore a documentação.

Se quiser ampliar a Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests

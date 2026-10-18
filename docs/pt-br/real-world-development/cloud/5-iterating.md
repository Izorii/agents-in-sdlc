---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência dos usuários e adicionar funcionalidades. Você pediu ao Copilot que adicionasse documentação ao código, criasse um recurso de jogos relacionados para a equipe de design iterar e implementasse recursos de acessibilidade, incluindo opções para alternar entre os modos de alto contraste e claro. Vamos explorar as alterações no código e, se necessário, enviar comentários ao Copilot para aprimorar o trabalho.

### Cenário

Como já foi destacado várias vezes, os fundamentos do design de software e do DevOps não mudam com a chegada da IA generativa. Sempre é importante revisar o código gerado e seguir o processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para criar a documentação, o recurso de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, há algumas restrições de segurança para garantir que tudo permaneça protegido:

- O Copilot tem acesso somente de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions, onde cria um ambiente separado e efêmero para trabalhar.
- Qualquer fluxo de trabalho do GitHub Actions precisa de aprovação humana antes de ser executado.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), inclusive para servidores MCP.

## Revise a documentação gerada

Vamos começar explorando a primeira pull request (PR) criada pelo agente de nuvem do GitHub Copilot: a PR que adiciona documentação ao código. Você usará a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir a PR, talvez você veja um aviso informando que o Copilot está bloqueado por um firewall. Isso é **esperado**, pois o acesso do Copilot a recursos externos é limitado por padrão, inclusive chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra a pull request com um título semelhante a **Add missing documentation** ou outro mais completo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, a pull request terá a indicação **[WIP]**. Nesse caso, aguarde até que o Copilot conclua o trabalho. Isso pode levar alguns minutos; aproveite para fazer uma pausa ou refletir sobre o que aprendeu.

4. Quando a pull request estiver pronta, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, incluindo os novos comentários TSDoc e outros elementos de documentação. As alterações exatas podem variar.

    Ao examinar as alterações, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Eles são resultado dos arquivos de instruções personalizadas que você revisou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
7. Você deverá ver um indicador de que alguns fluxos de trabalho estão aguardando aprovação.
8. Se houver fluxos aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão ser adicionados à seção de verificações da pull request. Se tudo correr bem, as verificações do projeto serão aprovadas para o aplicativo Astro. Isso pode levar alguns minutos.

## Solicite alterações ao GitHub Copilot

O trabalho com o Copilot em uma pull request não é uma via de mão única. Você também pode marcar o Copilot em comentários da pull request ou em comentários associados a linhas específicas do código, como faria com outras pessoas da equipe. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados não são determinísticos, não há um texto prescritivo para solicitar alterações. Algumas ideias do que você pode pedir ao Copilot:

- Adicione cabeçalhos de comentário no início de cada arquivo de código com uma breve descrição do que ele faz.
- Adicione comentários TSDoc a arquivos TypeScript e Astro.
- Crie um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração à documentação gerada e marque **@copilot**, como faria com qualquer outra pessoa. Use uma das ideias acima ou faça outra solicitação de documentação que gostaria de ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Você pode selecionar **Back to pull request** para voltar à pull request.

    ![Voltar à pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot terminar as alterações, um novo commit deverá aparecer na pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Quando estiver pronto, você pode marcar a PR de rascunho como pronta para revisão e fazer merge para a branch principal.

![Marcar a PR como pronta para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Vamos voltar à PR que o Copilot criou para resolver a issue sobre exibir jogos relacionados na página de detalhes do jogo.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione a PR com um título semelhante a **Show related games on the game details page** ou outro mais completo.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
6. Você deverá ver um indicador de que alguns fluxos de trabalho estão aguardando aprovação.
7. Se houver fluxos aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Os fluxos de trabalho deverão ser adicionados à seção de verificações da pull request. Se tudo correr bem, as verificações do projeto serão aprovadas para o aplicativo Astro. Isso pode levar alguns minutos.
9. **Opcional:** você também pode alternar para essa branch no Codespace e testar manualmente o recurso de jogos relacionados. Acesse seu Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Como antes, você pode iterar com o agente de nuvem para solicitar atualizações. Por exemplo, peça ao Copilot que altere a quantidade de jogos relacionados exibidos ou que adicione cabeçalhos de comentário e comentários TSDoc (lembre-se: esta tarefa foi atribuída **antes** de você atualizar as instruções personalizadas). Como antes, basta adicionar um comentário na guia **Conversation**; o Copilot o verá e iniciará outra sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. Esta PR deve incluir o modo de alto contraste que você solicitou no Exercício 3 e o modo claro solicitado no centro de controle no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione a PR com um título semelhante a **Add high contrast mode to website** ou outro mais completo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, a pull request terá a indicação **[WIP]**. Nesse caso, aguarde até que ele conclua o trabalho. Isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - componentes de interface para alternar entre os modos;
   - uso do armazenamento local para manter as preferências do usuário;
   - alterações de CSS ou estilo para os modos de alto contraste e claro;
   - atributos de acessibilidade (rótulos ARIA, navegação pelo teclado etc.);
   - código JavaScript/TypeScript que gerencia a alternância de modos.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
7. Você deverá ver um indicador de que alguns fluxos de trabalho estão aguardando aprovação.
8. Se houver fluxos aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão ser adicionados à seção de verificações da pull request. Se tudo correr bem, as verificações do projeto serão aprovadas para o aplicativo Astro. Isso pode levar alguns minutos.
10. **Opcional:** alterne para essa branch no Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os modos de alto contraste e claro no navegador para confirmar que funcionam como esperado e persistem depois de recarregar a página.

Observe como o agente personalizado de acessibilidade ajudou a orientar o Copilot para implementar esses recursos de acordo com as práticas recomendadas de acessibilidade. Se encontrar problemas ou oportunidades de melhoria de acessibilidade, marque **@copilot** em um comentário e solicite atualizações, como fez com as PRs anteriores.

## Exercício opcional — continue delegando

O agente de nuvem é mais útil quando você pode atribuir itens reais do backlog e se dedicar a outras tarefas. Para desenvolver esse hábito, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- Crie um formulário de interesse de apoiadores na página de detalhes do jogo.
- Implemente a paginação na página de lista de jogos.
- Adicione validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo destas lições, você:

- **Examinou os arquivos de instruções personalizadas incluídos no repositório** para observar seus efeitos posteriores na saída do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando opções de modo de alto contraste e modo claro.
- **Usou a página de agentes do Copilot como centro de controle** para acompanhar e orientar a sessão de acessibilidade em andamento.
- **Revisou e iterou sobre as pull requests do agente de nuvem**, marcando `@copilot` para solicitar alterações e aprovando fluxos de trabalho.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Se quiser continuar explorando, os outros percursos complementam o que você praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso do CLI](../../cli/)** — entregue duas alterações revisadas no terminal usando os modos Plan e Autopilot, skills de agente, um agente QA personalizado, Playwright MCP, Agent Merge e uma integração opcional `/delegate` com o agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- Atribua ao agente de nuvem uma issue de refatoração ou de cobertura de testes.
- Crie um agente personalizado para outro domínio.
- Configure `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/

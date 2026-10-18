---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É difícil encontrar organizações que não enfrentem problemas com dívida técnica. Pode ser o caso de problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidades que ficaram no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi criado para executar tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando conclui o trabalho, ele gera uma pull request de rascunho para revisão de uma pessoa desenvolvedora. Assim, tarefas repetitivas podem ser delegadas, o desenvolvimento pode acelerar e as pessoas desenvolvedoras podem se concentrar em questões mais amplas.

Com o agente de nuvem do Copilot, você vai explorar:

- a personalização do ambiente para geração de código;
- como garantir que as operações sejam realizadas com segurança;
- a importância de issues com escopo claro;
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívidas técnicas que gostaria de resolver. Os prestadores de serviço contratados inicialmente para criar a primeira versão do site deixaram a documentação em um estado ruim — na verdade, você verá que ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo mostrasse outros títulos da mesma categoria para que as pessoas continuem navegando. Ainda não é necessário ter uma implementação refinada; a equipe quer apenas algo que possa usar para testes de aceitação da experiência do usuário. Embora isso seja atualmente um impedimento, há outras issues de prioridade mais alta.

Esses dois exemplos representam tarefas que podem ser facilmente adiadas e que são boas candidatas para o agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona, enquanto a pessoa desenvolvedora se dedica a outras tarefas e, depois, revisa o trabalho para confirmar que está correto.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano, de modo semelhante a uma pessoa desenvolvedora. Assim como acontece ao trabalhar com uma pessoa da equipe, há várias maneiras de atribuir tarefas, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria uma pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão enquanto ela está em andamento ou depois de concluída. Quando a solução estiver pronta para revisão, o Copilot marcará você na pull request.

## A importância de instruções com escopo bem definido

O GitHub Copilot não faz mágica: ainda é preciso pensar. Até operações aparentemente simples podem se tornar complexas quando analisadas em detalhes. Por isso, [defina cuidadosamente o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um par de programação com IA: trabalhe em etapas, aprenda, experimente e adapte a abordagem. Os fundamentos do desenvolvimento de software não mudam com a chegada da IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já fez um exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente de nuvem do Copilot, leia os arquivos de instruções já incluídos neste repositório, sem alterá-los, para que você possa observar seus efeitos mais adiante.

Abra os arquivos a seguir na interface web do GitHub ou em um codespace, caso já tenha iniciado um:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as expectativas para convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que associa estas instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem no próximo trecho, observe a pull request resultante em busca de comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Esses elementos vêm dos arquivos de instruções. Voltaremos a falar deles ao revisar pull requests em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem do Copilot

A criação de código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja em boas condições, independentemente de quem fará o trabalho. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a uma pessoa da equipe de engenharia de software.

O agente de nuvem usa o [GitHub Actions][github-actions] para configurar o ambiente em que trabalha. Você pode personalizá-lo criando um [fluxo de trabalho de configuração especial][setup-workflow], configurado em `.github/workflows/copilot-setup-steps.yml`, para ser executado antes do início do trabalho. Isso disponibiliza as ferramentas e dependências de desenvolvimento necessárias. Esse fluxo de trabalho já foi configurado para facilitar o laboratório e a experiência de aprendizagem. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos binários do navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e populado do aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá seu próprio token para as operações dele.
    permissions:
      # Para clonar o repositório durante as etapas de configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nas etapas de configuração, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo - Node.js (agora o aplicativo inteiro usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e popula o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro fluxo de trabalho do GitHub, mas contém alguns detalhes importantes:

- Há um único job chamado `copilot-setup-steps`. Ele é executado no GitHub Actions antes de o Copilot começar a trabalhar na pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o fluxo manualmente na guia **Actions** do repositório. Isso é útil para testar se o fluxo de trabalho é executado corretamente, em vez de esperar que o Copilot o execute.

## Adicione documentação

Embora todos reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação alguma. Esse tipo de dívida técnica costuma ficar sem solução, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novas pessoas à equipe. Felizmente, o Copilot é muito útil para criar documentação, e esta é uma issue adequada para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Você voltará para revisar o trabalho em um exercício futuro.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir o diálogo de nova issue.
4. Selecione **Blank issue**.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir o diálogo de atribuição.

    ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

    ![Detalhes da atribuição do Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra a pull request (PR) recém-criada, cujo título será semelhante a `[WIP]: Code lacks documentation`. Se uma nova PR ainda não aparecer na lista, aguarde alguns instantes e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar alguns minutos para aparecer na PR. O Copilot está configurando o ambiente (executando o fluxo de trabalho mencionado anteriormente), analisando o projeto e determinando a melhor abordagem para resolver a issue.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo da pull request para baixo. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

    ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que é possível acompanhar a sessão em tempo real e ver como o Copilot resolve o problema. Isso inclui explorar o código e entender o estado, fazer pausas para pensar e decidir qual plano seguir e também criar código.

O processo provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é executar tarefas de forma assíncrona para liberar você para outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente de nuvem e, em seguida, voltando nossa atenção à criação de código para adicionar funcionalidades ao aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes do jogo

Uma das grandes vantagens do agente de nuvem do GitHub Copilot é a possibilidade de dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele trabalha em outro. Mesmo que adicionar uma seção de jogos relacionados para a equipe de design não demore muito, esse tempo poderia ser usado para outras tarefas. Vamos atribuir a tarefa ao agente de nuvem do Copilot.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir o diálogo de nova issue.
4. Selecione **Blank issue** para usar o template em branco.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir o diálogo de atribuição.

    ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá um conjunto de 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação. O agente de nuvem trabalha de maneira semelhante a uma pessoa da equipe de engenharia de software, então não é necessário monitorá-lo ativamente: basta revisar o resultado quando ele terminar. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Nesta lição, você conheceu o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, é possível atribuir issues ao Copilot para execução assíncrona. Você pode usá-lo para resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código de uma estrutura para outra.

Você explorou estes conceitos:

- personalização do ambiente para geração de código;
- operações realizadas com segurança;
- importância de issues com escopo claro;
- atribuição de issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, vamos nos concentrar na criação e no uso de agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso às instruções personalizadas apresentadas nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions

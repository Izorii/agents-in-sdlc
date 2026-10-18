---
title: "Exercício 3 - Testar seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir uma pull request, confirme se ele realmente funciona no navegador. Em vez de navegar pelo aplicativo manualmente, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador real para testar o recurso — depois, publique sua branch e abra a PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas disponíveis ao Copilot;
- adicionar o servidor MCP do Playwright ao workspace;
- pedir ao Copilot que o use para testar manualmente o recurso de filtragem em um navegador;
- publicar sua branch e abrir uma pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente se torna muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) é a forma como o Copilot faz isso: é um padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama mostrando o funcionamento interno do modo de agente e como ele interage com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) oferece aos agentes de IA uma forma de se comunicar com ferramentas e serviços externos em tempo real. Isso permite que eles acessem informações atualizadas (usando recursos) e executem ações em seu nome (usando ferramentas).

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e os serviços externos. O servidor MCP é responsável por gerenciar a comunicação com as ferramentas externas, como APIs existentes ou ferramentas locais, por exemplo, pacotes npm. Cada servidor MCP fornece ao agente de IA um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP populares:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: fornece acesso a um conjunto de APIs para gerenciar repositórios do GitHub. O agente de IA pode executar ações como criar repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: fornece automação do navegador usando o Playwright. O agente de IA pode executar ações como navegar por páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP que dão acesso a diferentes ferramentas e recursos. O GitHub mantém um [registro MCP](https://github.com/mcp) para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Do ponto de vista da segurança, trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor MCP, examine cuidadosamente o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores MCP em que você confia e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você deverá ver um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada `github` acompanha o template do projeto e permitiu ao Copilot ler o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é necessário instalar nada localmente: o VS Code se conecta por HTTP e você se autentica no GitHub na primeira vez que o Copilot usa uma das ferramentas.

## Adicione o servidor MCP do Playwright

Agora, você adicionará um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar — exatamente o que você precisa para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, de modo que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Ao contrário do servidor `github` remoto, `playwright` é um servidor **local**: o VS Code o inicia executando `npx @playwright/mcp@latest` na sua máquina. O sinalizador `--headless` faz o Playwright executar o navegador sem uma janela visível, o que é necessário dentro de um codespace, onde não há uma área de trabalho para exibi-la.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, então o navegador necessário normalmente já está instalado. Se o Copilot informar que falta um navegador, peça a ele que execute `npx playwright install chromium` e tente novamente.

## Inicie e confie no servidor

O VS Code inicia os servidores MCP **sob demanda** — na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e solicita que você confirme se confia nele.

1. Em `.vscode/mcp.json`, uma ação **Start** aparecerá acima da entrada `playwright`. Selecione-a para iniciar o servidor e confirmar que a configuração está correta.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e escolha confiar nele para que possa ser iniciado.

Iniciá-lo manualmente é opcional. Se você ignorar essa etapa, o VS Code iniciará o servidor (e pedirá que você confirme se confia nele) na primeira vez que o Copilot precisar das ferramentas no próximo passo.

## Teste o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador real para controlar. Em vez de navegar pelo aplicativo para verificar o trabalho, o agente pode abrir uma página, navegar, aplicar filtros e ler os resultados para você — e depois resumir o que observou. É uma forma rápida de confirmar se um recurso se comporta como esperado, sem sair da conversa.

Internamente, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, e não capturas de tela. Assim, o agente trabalha com elementos estruturados e rotulados (botões, links e itens de lista), como a tecnologia assistiva, e um teste funcional rápido também oferece uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste o recurso de filtragem que você acabou de criar:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot iniciará um navegador pelo servidor MCP do Playwright, executará cada etapa e relatará o que encontrou. Compare o resumo com os critérios de aceitação da issue. Se algo parecer incorreto, faça perguntas adicionais ou peça que ele corrija o código antes de abrir uma pull request.

> [!NOTE]
> Para este teste, o aplicativo precisa estar em execução em `http://localhost:4321`. Se você tiver parado o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que usar o servidor MCP do Playwright, o Copilot talvez precise baixar um navegador. Se informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique a branch e crie uma pull request

Agora que confirmou que o recurso funciona, você pode abrir uma pull request (PR) para que a equipe o revise. Primeiro, publique a branch `filtering-vscode`.

1. Acesse o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control mostrando as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Crie a pull request

Há várias maneiras de criar uma pull request, inclusive pelo github.com e pela interface de linha de comando (CLI) do GitHub. Como você já está usando o GitHub Copilot, vamos pedir a ele que crie a PR. Ele pode encontrar a issue correspondente e associá-la à pull request.

1. Acesse o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme que **Agent** está selecionado na lista suspensa de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie uma PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Quando necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute operações. Na primeira vez que usar uma ferramenta do GitHub, talvez seja necessário autenticar-se no GitHub; siga as instruções.
5. Observe como o Copilot pesquisa as issues, encontra a correta e cria a PR.
6. Selecione o link fornecido pelo Copilot para revisar a pull request, mas **não faça merge ainda**.

## Resumo e próximos passos

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas disponíveis ao Copilot;
- adicionou o servidor MCP do Playwright ao workspace;
- usou o servidor para testar manualmente o recurso de filtragem em um navegador antes de publicá-lo;
- publicou sua branch e abriu uma pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso — mas essa é apenas uma das maneiras de ampliar o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration

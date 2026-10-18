---
title: "Lição 6 - Valide a funcionalidade com o Playwright MCP"
description: "Inspecione ou configure o Playwright MCP no Copilot CLI e use-o para explorar o recurso de filtragem no navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código envolve mais do que apenas programar. Precisamos trabalhar com dados e serviços externos e até disponibilizar automações adicionais ao Copilot. É para isso que servem os servidores MCP: eles permitem que o Copilot vá além das funcionalidades integradas ao CLI, oferecendo mais ferramentas e serviços.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa;
- adicionar o servidor MCP do Playwright, caso ele ainda não esteja disponível;
- pedir ao agente que controle um navegador e explore o recurso de filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas a validação de atualizações da interface exige interagir com ela. Você quer permitir que o Copilot use o site como uma pessoa usuária para automatizar ainda mais as alterações e aumentar a confiança de que elas funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Isso possibilita acessar informações atualizadas e executar ações em seu nome.

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e os serviços externos. Cada servidor MCP fornece ao agente de IA um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP populares:

- **[Servidor MCP do GitHub][github-mcp]**: fornece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: oferece recursos de automação do navegador com o Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro MCP][mcp-registry] para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, examine o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicione o servidor MCP do Playwright

Se o servidor MCP do Playwright ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, ignore estas etapas de configuração e continue na próxima seção.

1. Volte ao Codespace.
2. No Copilot CLI, abra o diálogo para adicionar um servidor MCP digitando o comando:

   ```plaintext
   /mcp add
   ```

3. Para o nome, digite `playwright` e pressione <kbd>Tab</kbd>.
4. Confirme que o tipo de servidor **STDIO** está selecionado pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o texto a seguir no diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair do diálogo MCP.

## Peça ao Copilot que explore o recurso com o Playwright

Anteriormente, você confirmou manualmente que a funcionalidade funciona. Agora, vamos usar o servidor MCP do Playwright disponível no Copilot para fazer o mesmo.

1. Use o prompt a seguir para pedir ao Copilot que valide a funcionalidade com o servidor MCP do Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Normalmente, você não precisa pedir ao Copilot que use o servidor MCP, pois ele costuma identificar qual ferramenta usar. No entanto, como você sabe qual ferramenta deve ser usada, pode indicá-la para ajudar a obter resultados mais consistentes e economizar alguns tokens.

2. Observe o Copilot executar as várias etapas no navegador para confirmar se a funcionalidade funciona.
3. Leia o relatório e confirme que tudo se comporta como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, interromperá o servidor e apresentará um relatório.

## Resumo e próximos passos

Parabéns, você usou o servidor MCP do Playwright para explorar seu recurso em um navegador real pelo Copilot CLI. Recapitulando, você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa;
- adicionou o servidor MCP do Playwright, caso ele ainda não estivesse disponível;
- pediu ao agente que controlasse um navegador e explorasse o recurso de filtragem.

Em seguida, você vai [criar um agente personalizado de QA][next-lesson] que combina a skill e as ferramentas do navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers

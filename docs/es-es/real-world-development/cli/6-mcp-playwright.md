---
title: "Lección 6: valida la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar la función de filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos destacado, programar no consiste solo en escribir código. Hay que trabajar con datos y servicios externos y proporcionar a Copilot más opciones de automatización. Para eso sirven los servidores MCP: amplían las funciones integradas en la CLI y proporcionan a Copilot más herramientas y servicios.

En esta lección aprenderás a:

- entender qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- añadir el servidor MCP de Playwright si todavía no está disponible.
- pedir al agente que use un navegador para explorar la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero validar cambios en la interfaz requiere interactuar con ella. Quieres que Copilot use el sitio web como lo haría una persona para automatizar aún más la validación y tener más confianza en que los cambios funcionen como se espera.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos. Así pueden acceder a información actualizada y realizar acciones en tu nombre.

Se accede a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente.

Algunos servidores MCP conocidos son:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a las API para gestionar repositorios de GitHub, incidencias y pull requests.
- **[Playwright MCP Server][playwright-mcp-server]**: proporciona funciones de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP. GitHub aloja un [registro MCP][mcp-registry] para facilitar el descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y evalúa las implicaciones de seguridad.

## Añade el servidor MCP de Playwright

Si todavía no está disponible, añade el servidor MCP de Playwright para que Copilot pueda interactuar con el sitio web como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la siguiente sección.

1. Vuelve a tu Codespace.
2. En Copilot CLI, escribe el siguiente comando para abrir el cuadro de diálogo de adición de servidores MCP:

   ```plaintext
   /mcp add
   ```

3. En el campo del nombre, escribe `playwright` y pulsa <kbd>Tab</kbd>.
4. Pulsa <kbd>Enter</kbd> para confirmar el tipo de servidor **STDIO** y, después, pulsa <kbd>Tab</kbd>.
5. En el cuadro de diálogo **Command**, pega lo siguiente:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Ya has comprobado manualmente que la funcionalidad funciona. Ahora vamos a pedirle al servidor MCP de Playwright disponible en Copilot que haga lo mismo.

1. Envía este prompt para pedir a Copilot que use el servidor MCP de Playwright y valide la funcionalidad:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Normalmente no hace falta indicar a Copilot que use el servidor MCP, porque suele detectarlo por su cuenta. Aun así, puedes orientarlo para que los resultados sean más coherentes y reducir el uso de tokens.

2. Observa los pasos que realiza Copilot en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y confirma que todo se comporta como se espera.

Copilot iniciará el servidor, usará Playwright para interactuar con el sitio web, detendrá el servidor y te entregará un informe.

## Resumen y siguientes pasos

¡Enhorabuena! Has usado el servidor MCP de Playwright para explorar la funcionalidad en un navegador real desde Copilot CLI. En resumen:

- has aprendido qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- has añadido el servidor MCP de Playwright si aún no estaba disponible.
- has pedido al agente que use un navegador para explorar la función de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la skill y las herramientas del navegador en un rol especializado.

## Recursos

- [¿Qué es MCP y por qué se habla tanto de él?][mcp-blog-post]
- [Servidor MCP de Playwright de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers

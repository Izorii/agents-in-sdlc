---
title: "Ejercicio 3: prueba la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo agente. Antes de abrir un pull request, comprueba que funciona en el navegador. En lugar de probar la aplicación manualmente, conectarás el **servidor MCP de Playwright** y pedirás a Copilot que use un navegador real para probar la funcionalidad. Después, publicarás la rama y abrirás el PR.

En este ejercicio aprenderás a:

- descubrir qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas disponibles para Copilot.
- añadir el servidor MCP de Playwright al espacio de trabajo.
- pedir a Copilot que lo use para probar manualmente la funcionalidad de filtrado en un navegador.
- publicar la rama y abrir un pull request para los cambios del filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente resulta mucho más potente cuando puede acceder a herramientas externas. Model Context Protocol (MCP) permite que Copilot lo haga: es un estándar para que el agente se comunique con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo agente y de cómo interactúa con el contexto, el LLM y las herramientas, incluidas las proporcionadas por servidores MCP y extensiones de VS Code.](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos. Mediante MCP pueden consultar información actualizada (con recursos) y realizar acciones en tu nombre (con herramientas).

Se accede a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. El servidor MCP gestiona la comunicación con las herramientas externas, como las API existentes o herramientas locales (por ejemplo, paquetes de NPM). Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente.

Algunos servidores MCP conocidos son:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a un conjunto de API para gestionar repositorios de GitHub. El agente de IA puede crear repositorios, actualizar los existentes y gestionar incidencias y pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: ofrece funciones de automatización del navegador con Playwright. El agente de IA puede navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que dan acceso a herramientas y recursos. GitHub aloja un [registro MCP](https://github.com/mcp) para facilitar el descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Desde el punto de vista de la seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa detenidamente su código fuente, verifica quién lo publica y evalúa las implicaciones de seguridad. Usa solo servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el Codespace.
2. Verás que ya hay un servidor `github` configurado:

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

La plantilla del proyecto incluye la entrada `github`. Por eso Copilot pudo leer la lista de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server]; no hace falta instalarlo localmente. VS Code se conecta por HTTP y te autenticas con GitHub la primera vez que Copilot usa una de sus herramientas.

## Añade el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`, de modo que el archivo quede así:

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

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu equipo ejecutando `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin mostrar una ventana. Es necesario en un Codespace, que no tiene un escritorio donde mostrarlo.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, así que normalmente el navegador necesario ya está instalado. Si Copilot indica que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia el servidor y confirma que es de confianza

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot requiere sus herramientas, VS Code inicia el servidor y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, aparecerá una acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma la confianza para que pueda iniciarse.

Iniciarlo manualmente es opcional. Si lo omites, VS Code lo iniciará y te pedirá que confirmes que confías en él la primera vez que Copilot necesite sus herramientas en el paso siguiente.

## Prueba la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real. En vez de navegar por la aplicación para comprobar el trabajo, el agente puede abrir una página, aplicar filtros y leer el resultado para resumir lo que observa. Así puedes comprobar rápidamente que una funcionalidad se comporta como esperas sin salir de la conversación.

El servidor MCP de Playwright usa el [árbol de accesibilidad][playwright-mcp-server] de la página, no capturas de pantalla. Así, el agente trabaja con elementos estructurados y etiquetados (botones, enlaces y elementos de listas), como las tecnologías de asistencia. Por eso, una comprobación funcional rápida también sirve como prueba básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la funcionalidad que acabas de crear:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot iniciará un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te dirá qué ha observado. Compara su resumen con los criterios de aceptación de la incidencia. Si algo no funciona como esperabas, haz preguntas de seguimiento o pídele que corrija el código antes de abrir un pull request.

> [!NOTE]
> La aplicación debe estar ejecutándose en `http://localhost:4321` para realizar esta prueba. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot use el servidor MCP de Playwright, quizá tenga que descargar un navegador. Si indica que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea un pull request

Ya has confirmado que la funcionalidad funciona y puedes abrir un pull request (PR) para que el equipo la revise. Primero, publica la rama `filtering-vscode`.

1. Abre el panel **Source Control** del Codespace y revisa los cambios que ha realizado Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Panel Source Control con los cambios realizados.](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea el pull request

Hay varias formas de crear un pull request, como usar github.com o la interfaz de línea de comandos (CLI) de GitHub. Como ya estás trabajando con GitHub Copilot, pídele que lo cree: puede encontrar la incidencia correspondiente y relacionarla con el PR.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** está seleccionado en el selector de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree un PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si es necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones. La primera vez que use una herramienta de GitHub, quizá tengas que autenticarte; sigue las indicaciones.
5. Observa cómo Copilot busca las incidencias, encuentra la adecuada y crea el PR.
6. Selecciona el enlace que genere Copilot para revisar el pull request, pero **no lo fusiones todavía**.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has:

- aprendido qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- añadido el servidor MCP de Playwright al espacio de trabajo.
- usado el servidor para probar manualmente la funcionalidad en un navegador antes de publicarla.
- publicado la rama y abierto un pull request para los cambios del filtrado.

Has usado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué se habla tanto de él?][mcp-blog-post]
- [Servidor MCP de Playwright de Microsoft][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration

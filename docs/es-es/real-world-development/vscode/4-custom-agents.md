---
title: "Ejercicio 4: agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados y adaptados a tareas o dominios concretos del flujo de trabajo de desarrollo. Puedes definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio y proporcionarles instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos que les permitan realizar determinados tipos de trabajo con mayor eficacia. Los equipos pueden codificar sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que compruebe el cumplimiento de [WCAG][wcag], un agente de seguridad que aplique prácticas de programación segura o uno de pruebas que mantenga patrones de prueba coherentes.

Los agentes personalizados se definen en archivos Markdown de la carpeta `.github/agents` del proyecto o de forma global en `~/.copilot/agents`. Cada archivo incluye metadatos YAML con al menos los campos `name` y `description`, seguidos de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Comparación entre agentes personalizados y skills de agente

Los agentes personalizados y las [skills de agente][agent-skills-concept] se solapan en cierta medida: ambos suelen definirse mediante archivos Markdown y enseñan a una IA a realizar tareas. La forma más sencilla de distinguirlos es pensar que un **agente personalizado** es quien realiza el trabajo y las **skills** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y se encargan de coordinar skills e incluso otros agentes. En este laboratorio, el agente de accesibilidad revisa y actualiza el sitio conforme a las directrices de accesibilidad. Para ello, podría invocar skills como una skill para gestionar pull requests o ejecutar y controlar pruebas.

> [!NOTE]
> No existe una única forma correcta de crear un agente personalizado. Como con cualquier otra herramienta de IA, prueba distintas opciones e itera hasta encontrar las que funcionen mejor en tu entorno y para tus casos de uso.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que cualquier persona pueda usar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes indican que a algunas personas les cuesta leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es prioritaria, quieres implementarlo cuanto antes. Para generar la funcionalidad, usarás un agente personalizado.

En este ejercicio aprenderás a:

- revisar un agente personalizado de accesibilidad ya creado.
- usar el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para trabajar con la accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve al Codespace, abre un terminal y crea una rama nueva desde `main` para los cambios de accesibilidad. Así mantendrás separado el PR del filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en los metadatos YAML `name` y `description`.

> [!CAUTION]
> Los metadatos `name` y `description` son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores habituales que conviene evitar.

## Usa el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el selector de agentes, situado en la parte inferior de la vista Copilot Chat. Puedes seleccionar uno para aplicar sus instrucciones y herramientas a una sesión de chat.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; se indicarán cuando corresponda.

1. Selecciona **Agent** en el selector de agentes de la vista Chat si todavía no está seleccionado.

   ![Selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Abre el selector de agentes situado en la parte inferior de la vista Chat. Muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía este prompt al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y un diff para que puedas revisarla.
6. A medida que se apliquen las ediciones, el indicador **Files changed** se actualizará para mostrar los archivos modificados por el agente.

> [!NOTE]
> Es probable que este proceso tarde unos minutos. Copilot está realizando cambios reales en el repositorio y puede modificar componentes Astro, CSS y pruebas relacionadas.

En el siguiente ejercicio revisarás y orientarás el trabajo mientras está en curso.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados en tareas y dominios concretos. Los agentes permiten codificar la experiencia y los estándares del equipo en agentes reutilizables que orientan a Copilot para que realice determinados tipos de trabajo con mayor eficacia.

Has aprendido a:

- definir agentes personalizados.
- usar un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson]: revisarás los cambios mientras se realizan y añadirás un control para el modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Prueba la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisión y gestión de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents

---
title: "Ejercicio 2: GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad pendientes, código heredado que hay que actualizar o solicitudes de funcionalidad que llevan tiempo en la lista de tareas pendientes porque no ha habido tiempo para implementarlas. GitHub Copilot cloud agent está diseñado para tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea un borrador de PR para que un desarrollador lo revise. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones de mayor alcance.

Con Copilot cloud agent explorarás cómo:

- personalizar el entorno para generar código.
- asegurarte de que las operaciones se realizan de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado deficiente: no hay ninguna. Como primer paso, quieren añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la forma de descubrir juegos. La página de detalles de cada juego debería mostrar otros juegos relacionados, es decir, títulos de la misma categoría, para que las personas puedan seguir explorando. Todavía no necesitan una implementación pulida; quieren algo que permita probar y aceptar la experiencia de usuario (UX). Es un impedimento actual, pero hay otras incidencias de mayor prioridad.

Estas tareas a menudo se quedan relegadas y son buenas candidatas para Copilot cloud agent. Puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y vuelve más tarde para revisar el resultado.

## Presentación de GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano, de forma parecida a un desarrollador. Al igual que con una persona, puedes colaborar con él de varias formas; por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Tras la asignación, Copilot crea un borrador de pull request para hacer un seguimiento de su trabajo, configura un entorno y empieza la tarea. Puedes explorar la sesión mientras trabaja o cuando haya terminado. Cuando la solución esté lista para que la revises, te mencionará en el pull request.

## La importancia de definir bien el alcance de las instrucciones

GitHub Copilot no hace magia: no puedes dejar de pensar. Hasta las tareas aparentemente sencillas pueden resultar complejas cuando se analizan en detalle. Por eso, [define cuidadosamente el alcance de las tareas para Copilot cloud agent][cloud-agent-best-practices]. Trátalo como un programador en pareja con IA: trabaja por etapas, aprende, experimenta y adapta el proceso. Los fundamentos del desarrollo de software no cambian por incorporar IA generativa.

## Instrucciones personalizadas del repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya has completado un ejercicio práctico sobre ellas, habrás visto `.github/copilot-instructions.md` en acción. Si no, es buen momento para echar un vistazo. Antes de asignar trabajo a Copilot cloud agent, revisa los archivos de instrucciones incluidos en este repositorio, sin modificarlos, para que puedas identificar sus efectos más adelante.

Abre los siguientes archivos en la interfaz web de GitHub o en un Codespace activo:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el campo `applyTo` de los metadatos, que aplica estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes a cloud agent la incidencia `Code lacks documentation` en la siguiente sección, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los comentarios de cabecera del pull request resultante. Los archivos de instrucciones orientan estos cambios. Lo volveremos a destacar al revisar los PR en un ejercicio posterior.

## Configura el entorno de desarrollo de Copilot cloud agent

Para crear código suele ser necesario preparar un entorno y ejecutar scripts de configuración para disponer de las herramientas y dependencias necesarias. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

Cloud agent usa [GitHub Actions][github-actions] para su entorno. Puedes personalizarlo con un [flujo de trabajo especial de configuración][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de empezar el trabajo. Así, el agente puede acceder a las herramientas y dependencias de desarrollo necesarias. Este flujo ya está configurado para agilizar el laboratorio y facilitar el aprendizaje. Proporciona a Copilot Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración en la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Asigna los permisos mínimos necesarios para *tus pasos*. Copilot recibirá un token propio para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración (por ejemplo, para instalar dependencias), necesitarás el permiso `contents: read`. Si no clonas el repositorio en los pasos de configuración, Copilot lo hará automáticamente cuando estos terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (ahora toda la aplicación usa Astro + Drizzle/libSQL).
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

      # Migra y precarga la base de datos SQLite local para que las compilaciones y pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en el pull request.
- Observa el desencadenador `workflow_dispatch`, que permite ejecutar el flujo manualmente desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

Todo el mundo reconoce la importancia de la documentación, pero muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica puede ralentizar la productividad y dificultar el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot es especialmente útil para crear documentación, así que esta es una buena incidencia para asignarle. Trabajará en segundo plano y generará la documentación necesaria. En un ejercicio posterior volverás para revisar su trabajo.

1. Abre una pestaña nueva del navegador y ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nuevas incidencias.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Code lacks documentation`.
6. En **Description**, escribe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

    ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre el pull request (PR) recién creado, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si no aparece un PR nuevo, espera uno o dos minutos y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en el PR. Copilot está preparando el entorno (ejecutando el flujo de trabajo anterior), analizando el proyecto y determinando cómo abordar el problema.

13. Revisa la lista y las tareas que Copilot va a realizar.
14. Desplázate por la cronología del pull request hasta encontrar una actualización que indique que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Quizá tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en curso y cómo Copilot resuelve el problema: explora el código y entiende su estado, se detiene a pensar y decidir el plan adecuado y crea código.

El proceso puede tardar varios minutos. Uno de los objetivos principales de Copilot cloud agent es realizar tareas asíncronas para que podamos centrarnos en otras. Aprovecharemos esta función asignándole otra tarea y, mientras trabaja, escribiremos código para añadir funcionalidad a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las grandes ventajas de GitHub Copilot cloud agent es que permite dividir el trabajo: tú puedes centrarte en unas tareas mientras el agente trabaja en otras. Quizá añadir una sección de juegos relacionados no lleve mucho tiempo, pero podrías dedicar ese tiempo a otras tareas. Vamos a asignársela a Copilot cloud agent.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nuevas incidencias.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Show related games on the game details page`.
6. En **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, verás varios 👀 en el primer comentario de la incidencia, que indican que Copilot está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. Como cloud agent trabaja de forma parecida a un ingeniero de software, no hace falta supervisarlo continuamente; puedes revisar el resultado cuando termine. Ahora veremos cómo crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado [GitHub Copilot cloud agent][copilot-agents]. Puedes asignar incidencias a cloud agent para que las resuelva de forma asíncrona. Copilot puede ayudar a resolver deuda técnica, crear funcionalidades o migrar código de un framework a otro.

Has explorado cómo:

- personalizar el entorno para generar código.
- asegurarte de que las operaciones se realizan de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras cloud agent trabaja en segundo plano, puedes centrarte en crear y usar agentes personalizados. [Copilot cloud agent también puede usar servidores MCP][cloud-agent-mcp] y tiene acceso a las instrucciones personalizadas que exploramos en módulos anteriores.

## Recursos

- [Acerca de Copilot cloud agent][copilot-agents]
- [Asignación de incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración de Copilot cloud agent][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions

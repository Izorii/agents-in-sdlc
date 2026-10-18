---
title: "Lección 2: añadir valoraciones con estrellas, una mejora rápida"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revisarlo en un navegador reenviado y fusionarlo como tu primer pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y probado una conversación. Ahora harás tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración con estrellas, pero las tarjetas de la página principal todavía no la muestran. Pedirás al agente que la añada, revisarás el cambio y lo fusionarás como tu primer pull request.

En esta lección aprenderás a:

- iniciar una conversación específica de Copilot en una rama de funcionalidad.
- pedir al agente que haga un pequeño cambio en el proyecto.
- revisar el cambio con `/diff`.
- ejecutar la aplicación y comprobarla en un navegador reenviado.
- abrir y fusionar tu primer pull request.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, la editorial y la descripción. Como ejercicio inicial, pedirás al agente que muestre en cada tarjeta la valoración existente: un cambio pequeño y autónomo, ideal para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI usa el repositorio y la rama de Git que estén activos en el terminal, en vez de crear un worktree específico. Las conversaciones guardadas te permiten retomar el mismo diálogo más adelante, mientras que los archivos y la rama siguen siendo el estado habitual de Git en el disco.

En una conversación verás tres elementos: tus prompts y las respuestas del agente; la actividad de sus herramientas mientras explora y edita archivos; y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y solicita el cambio

Inicia una conversación nueva para empezar a implementar la funcionalidad.

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Si Copilot aún no está abierto, inicia la aplicación con este comando:

   ```bash
   copilot --yolo
   ```

4. Inicia una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Envía este prompt para solicitar el cambio:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explora el proyecto, localiza los archivos que muestran los detalles de los juegos y crea el código necesario. ¡Ya has añadido una funcionalidad con Copilot CLI!

## Revisa el diff

Antes de fusionar cualquier cambio generado por IA, también hay que revisarlo. Vamos a inspeccionar los cambios directamente en Copilot CLI.

1. Escribe `/diff` y revisa todos los archivos modificados.
2. Confirma que la tarjeta muestra la valoración numérica cuando está disponible y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones que ha ejecutado Copilot y pídele que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la pantalla de diff.

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, ofrece resultados probabilísticos, no deterministas. El código concreto puede variar. Revisa el comportamiento esperado en vez de buscar una implementación idéntica.

## Comprueba los cambios

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que inicie el sitio web para que puedas examinar la interfaz de usuario (UI) actualizada en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas de juegos muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abre y fusiona tu primer pull request

Ya has creado la funcionalidad. Ahora abrirás un pull request (PR) para fusionar el código en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Copilot CLI enviará el commit existente al crear el PR y mostrará su URL.
3. Abre el PR manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) mientras seleccionas la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la fusión.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- iniciaste una conversación específica de Copilot en una rama de funcionalidad.
- pediste al agente que hiciera un pequeño cambio en las tarjetas de juegos.
- revisaste el cambio con `/diff`.
- ejecutaste la aplicación y comprobaste la valoración en un navegador reenviado.
- abriste y fusionaste tu primer pull request.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference

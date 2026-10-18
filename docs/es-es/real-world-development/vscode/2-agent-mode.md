---
title: "Ejercicio 2: añade funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Hasta las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. El flujo de trabajo típico de un desarrollador consiste en localizar todos los archivos necesarios, realizar los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta y volver a actualizarlo. Y así sucesivamente.

El modo agente de Copilot te ayuda con todo esto.

El modo agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de forma parecida a un desarrollador: explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Veamos cómo usarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y cloud agent ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos de comentarios rápidos e interacción directa. Cloud agent trabaja de forma asíncrona como un compañero de equipo y se comunica contigo mediante incidencias y pull requests.

En este ejercicio aprenderás a:

- usar el modo agente de Copilot para explorar el proyecto e identificar los archivos pertinentes y realizar cambios coordinados.
- usar el modo agente de GitHub Copilot para implementar funcionalidades en la interfaz de usuario y la capa de datos.
- revisar los cambios y las pruebas que genere el modo agente antes de fusionarlos en el código.

## Escenario

El catálogo de juegos va creciendo y quieres que los usuarios puedan filtrar por categoría y editorial. En el ejercicio anterior ya creaste un helper de editoriales; ahora completarás con el modo agente de Copilot el trabajo restante en la capa de datos, la interfaz y las pruebas.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con una temática de desarrolladores. Permite listar juegos y mostrar sus detalles. Se trata de una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan una base de datos SQLite local directamente desde su frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se implementa con un pequeño elemento `<script>` de Astro que usa las API DOM estándar.

### Inicia el sitio web

Para facilitar la ejecución del sitio, hay disponible una tarea `npm run dev` que inicia el único servidor de desarrollo Astro. Puedes ejecutarla en GitHub Codespaces:

1. Vuelve a tu Codespace. Tras el ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el Codespace con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web con este comando:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, aparecerá un mensaje con una URL similar a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Para abrir el sitio web, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un Codespace, al seleccionar un enlace a la URL local en el terminal, se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al Codespace, donde se ejecuta el servidor web.

### Explora el sitio web

Una vez iniciado el sitio web, puedes explorar sus funciones principales:

- **Página principal**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora la lista de tareas pendientes con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; se indicarán cuando corresponda.

La implementación inicial del sitio funciona, pero queremos añadir nuevas capacidades. Empecemos por revisar la lista de tareas pendientes. Al configurar el proyecto desde la plantilla, se crearon automáticamente varias incidencias; pide a GitHub Copilot que te las muestre.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal e invoca herramientas.

   ![Selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo en particular. Usamos Claude Sonnet 4.5 al crear este taller y lo indicamos para que las sugerencias de código sean relativamente coherentes y la experiencia resulte adecuada. Sin embargo, como los modelos de lenguaje grandes (LLM) son probabilísticos, las sugerencias pueden diferir de las que se muestran aquí. Es normal.

> [!NOTE]
> Debido al carácter probabilístico de los LLM, Copilot puede usar otro comando MCP, pero debería poder completar la tarea.

3. Envía este prompt a Copilot para consultar la lista de tareas pendientes:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que muestra todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot ha priorizado las tareas según las que considera más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que usarás para proporcionar contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo abordar la incorporación de funcionalidad al sitio.

1. En el Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funcionalidad. Incluyen:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones específicos para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Esto facilita la reutilización, permite dividir directrices extensas en archivos más pequeños y reutilizables y ayuda a proporcionar ejemplos y plantillas a Copilot.

## Implementa la funcionalidad de filtrado

Para completar el filtrado hay que realizar al menos tres cambios distintos en la aplicación:

- añadir o perfeccionar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de los filtros.
- actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse correctamente antes de fusionar los cambios. El modo agente de Copilot puede realizar estas tareas. Vamos a añadir la funcionalidad.

1. Continúa la conversación actual con Copilot o selecciona **New Chat** para empezar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de la interfaz.](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia correspondiente de la lista de tareas pendientes:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes para la funcionalidad. Debería encontrar la capa de datos, la interfaz y las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Copilot explora los archivos del proyecto.](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos, según la complejidad de la tarea y del código. Mientras tanto, es posible que aparezcan los botones **Keep** y **Undo** en el editor. Cuando Copilot termine, habrá un botón **Keep** o **Undo** para cada cambio; no hace falta seleccionarlos mientras esté trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas.](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso fallen algunas pruebas. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta que completa la tarea y no detecta errores.

   ![Sesión de Copilot Agent Mode completada.](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Revisa el código generado para detectar posibles problemas.

> [!CAUTION]
> Es importante revisar siempre el código que genera Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio web y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has usado el modo agente de GitHub Copilot para añadir capacidades al sitio web Tailspin Toys. Has aprendido a:

- usar el modo agente de GitHub Copilot para implementar funcionalidades en la interfaz y la capa de datos.
- usar el modo agente de Copilot para explorar el proyecto, identificar archivos pertinentes y realizar cambios coordinados.
- revisar los cambios y las pruebas generadas por el modo agente antes de fusionarlos en el código.

Ahora [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás un pull request.

### Ejercicio de exploración adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Usa las habilidades adquiridas en este ejercicio y pide a Copilot que actualice el sitio para implementarla. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas, incluidos los archivos de instrucciones existentes.
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá seleccionar el tamaño de página o si será un valor fijo.
- proporciona en el prompt las indicaciones necesarias para que Copilot genere la implementación que necesitas.
- es posible que tengas que iterar con GitHub Copilot y pedir cambios o proporcionar más contexto. Es lo habitual al trabajar con Copilot.

## Recursos

- [Copilot: modos ask, edit y agent; qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode

---
title: "Ejercicio 6: iteración sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← Lección anterior: Supervisión y gestión de agentes][previous-lesson] |
| :-- |

## Revisión del trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario. Has usado el modo agente para añadir filtros en el cliente y el servidor, el servidor MCP de Playwright para probar manualmente los cambios en un navegador y, después, un agente personalizado para implementar funciones de accesibilidad —controles de alto contraste y modo claro—. También has intervenido en la sesión para ampliar el trabajo. Ahora publicarás los cambios locales y los revisarás como lo haría el equipo.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian por incorporar IA generativa. Conviene revisar de verdad todo lo que produce Copilot. Por eso, enviaremos los cambios de accesibilidad desde el Codespace, abriremos un pull request y revisaremos el diff antes de incorporar al resto del equipo.

## Publica las funciones de accesibilidad

Los controles de alto contraste y modo claro que implementaste con el agente personalizado de accesibilidad en el [Ejercicio 4][exercise-4] y el [Ejercicio 5][exercise-5] están guardados en el Codespace como cambios confirmados. Vamos a enviarlos a una rama y abrir un pull request para que el resto del equipo pueda revisarlos.

1. Vuelve al Codespace.
2. Abre la vista **Source Control** en VS Code.
3. Confirma que los cambios de accesibilidad están confirmados. Si tienes cambios sin confirmar del Ejercicio 5, prepáralos y confírmalos ahora con un mensaje descriptivo, por ejemplo, `Add high-contrast and light-mode toggles`.
4. Publica la rama seleccionando **Publish Branch** (o selecciona el menú **...** → **Push**).
5. VS Code te ofrecerá abrir la nueva rama en github.com. Acepta la solicitud o ve al repositorio manualmente y selecciona **Compare & pull request** en el aviso de la rama.
6. Escribe un título claro (por ejemplo, `Add high-contrast and light-mode toggles`) y una descripción breve que resuma qué se ha hecho y por qué.
7. Selecciona **Create pull request**.
8. Cuando se abra el PR, selecciona la pestaña **Files changed** y revisa todo el trabajo. Presta especial atención a:
   - los componentes de interfaz para alternar entre modos.
   - el uso del almacenamiento local para guardar las preferencias.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - el código JavaScript o TypeScript que gestiona el cambio de modo.

9. Vuelve a la pestaña **Conversation**.
10. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo con Approve and run workflows](../../../_images/shared-approve-workflows.png)
11. Espera a que terminen los flujos de trabajo. Si todo va bien, deberían completarse correctamente.

> [!TIP]
> ¿Quieres una segunda opinión sobre las funciones de accesibilidad? Menciona a `@copilot` en un comentario del PR con una solicitud como «revisa este PR para detectar otros problemas de WCAG» o «sugiere mejoras para la navegación con teclado». Copilot iniciará una sesión nueva para responder al comentario.

## Ejercicio opcional: sigue explorando en local

Trabajar de forma iterativa con un agente en el IDE es una habilidad que solo se desarrolla con la práctica. Algunas ideas para sesiones de seguimiento en VS Code:

- crear un formulario para registrar el interés de quienes respaldan los juegos en la página de detalles.
- implementar la paginación en la página de listado de juegos.
- añadir validación de entrada y gestión de errores a los helpers de acceso a datos en `src/lib/`.
- ampliar el alcance del agente de accesibilidad, por ejemplo, para revisar el orden del foco del teclado en todo el sitio.

## Resumen

¡Enhorabuena! Has completado el itinerario de VS Code. En este laboratorio:

- **Has usado Playwright MCP para probar la funcionalidad manualmente.** Has añadido el servidor MCP de Playwright y has pedido a Copilot que use un navegador para verificar el filtrado antes de abrir un pull request.
- **Has usado el modo agente para coordinar cambios en distintas capas.** Has añadido una funcionalidad de filtrado que afecta al cliente, al servidor y a las pruebas en una sola sesión.
- **Has usado un agente personalizado.** Has seleccionado el agente personalizado de accesibilidad y has observado cómo implementaba el modo de alto contraste en el repositorio.
- **Has gestionado y orientado una sesión de agente.** Has revisado los cambios propuestos en el propio editor, aceptado los que querías y ampliado la sesión con una solicitud de seguimiento para añadir el modo claro.
- **Has cerrado el ciclo con un pull request.** Has publicado el trabajo local y lo has revisado de principio a fin, como haría el equipo.

## Revisión y siguientes pasos

Aquí termina el itinerario obligatorio de VS Code. Puedes parar aquí y dar el taller por completado.

Si quieres ampliar tu perspectiva sobre las capacidades de los agentes de Copilot, los demás itinerarios abordan situaciones relacionadas en otras interfaces:

- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, skills, un agente personalizado de QA, Playwright MCP, Agent Merge y transferencias con contexto.
- ☁️ **[Itinerario de cloud agent](../../cloud/)**: asigna incidencias a cloud agent, supervisa sesiones desde la página de agentes e itera sobre los pull requests de forma asíncrona.

También puedes seguir trabajando a partir de lo que has creado. [awesome-copilot][awesome-copilot] es un buen recurso para encontrar archivos de instrucciones, agentes personalizados y skills que puedes adaptar a tus proyectos.

Como ampliación opcional, [Opcional: incorpora Foundry][exercise-7] usa VS Code y Microsoft Foundry Toolkit para preparar un modelo, desplegar un Backer Concierge y conectarlo al sitio.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Supervisión y gestión de agentes][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot

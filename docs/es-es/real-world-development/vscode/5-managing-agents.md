---
title: "Ejercicio 5: supervisa y gestiona agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Cuando usas el modo agente de GitHub Copilot, este trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tiempo real en tu equipo, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición. Puedes revisar los diffs integrados en el chat, aceptar o rechazar cambios individuales y orientar la conversación con prompts de seguimiento para refinar o ampliar el trabajo sin salir del editor.

En este ejercicio aprenderás a:

- supervisar en tiempo real el trabajo del agente de accesibilidad.
- revisar y aceptar las ediciones de archivos propuestas.
- orientar al agente durante la sesión para añadir un control del modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras el trabajo está en curso, el equipo de diseño solicita también un control para el modo claro, para quienes prefieren una interfaz menos oscura que el tema predeterminado. En vez de empezar otra conversación, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y los cambios que propone. Cada edición aparece en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras el agente trabaja, desplázate por el chat para observar las llamadas a herramientas. Verás lecturas y ediciones de archivos y los comandos de terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, el diff aparecerá integrado en el chat:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta necesita autorización (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Autoriza solo los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final con un resumen de los cambios.

## Pide al agente que añada el modo claro

Ahora que el modo de alto contraste está implementado, ampliarás la misma conversación para añadir un control del modo claro. Al continuar en la misma sesión, el agente conserva el contexto del trabajo anterior, incluidos los componentes que ha modificado, el enfoque de estilos y el patrón de almacenamiento local.

1. Envía este prompt de seguimiento en el campo de Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará donde se había detenido, explorará los cambios realizados y propondrá cómo añadir el modo claro.
3. Revisa cada cambio nuevo cuando aparezca, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** en cada diff.
4. Si el agente toma una dirección que no quieres (por ejemplo, intenta refactorizar la interfaz de los controles de un modo que no te convence), puedes detenerlo con el botón **Stop** junto al campo de chat y enviar un mensaje para aclarar lo que necesitas.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Envía prompts de seguimiento para pedir ajustes («simplifica el CSS; usa variables CSS en vez de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también atajos de teclado para ambos controles»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa brevemente todos los archivos modificados por el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos de los archivos modificados y revisa los diffs. Confirma que se aplican los principios de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando te parezca correcto, prepara y confirma los cambios desde el panel Source Control. Los publicarás en la [siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has usado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has aprendido a:

- supervisar el trabajo de un agente en tiempo real.
- aceptar o rechazar ediciones individuales.
- orientar una sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en un pull request y revisarás los cambios que ha producido el agente personalizado local durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode

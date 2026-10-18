---
title: "Ejercicio 4: supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido a Copilot cloud agent que trabaje en tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidad. Aunque cloud agent está diseñado para trabajar de forma asíncrona y autónoma, también es importante supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas a cloud agent, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes consultar todas las tareas de agentes que tienen pull requests abiertos, explorar las operaciones realizadas y orientar una sesión en curso.

En esta lección aprenderás a:

- explorar la página de agentes para supervisar las tareas de cloud agent.
- orientar una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación del modo de alto contraste, el equipo se dio cuenta de que sería buen momento para añadir también un modo claro. Como ya se estaba actualizando el estilo del sitio e incorporando controles de alternancia, parecía lógico incluir esta función. Orientarás al agente para que añada el modo claro además del alto contraste.

## Revisa las tareas de Copilot cloud agent

Veamos el estado actual de las tareas asignadas a Copilot cloud agent.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas asignadas a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa las operaciones que ha realizado Copilot. Entre otras, puede:
   - obtener el código del repositorio.
   - crear el entorno de trabajo.
   - configurar servidores MCP.
   - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos variarán según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel del pull request que aparece a la derecha. Permite consultar el PR y los archivos modificados para supervisar el trabajo.

## Orienta cloud agent

Ya has visto las tareas activas. Ahora pedirás a Copilot que añada el control del modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en el panel de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Verás mensajes parecidos a los ejemplos siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea de cloud agent en la página de agentes, con el cuadro de diálogo para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora al trabajo.

## Deja que Copilot trabaje

Como antes, Copilot trabajará en la tarea actualizada. Incorporará la nueva solicitud al flujo de trabajo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

El proceso tardará varios minutos, así que puedes hacer una pausa y repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas de GitHub Copilot cloud agent. Puedes realizar un seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir tareas en curso para ajustar su alcance o proporcionar indicaciones adicionales.

Has aprendido a:

- explorar el panel de control y la página de agentes para supervisar las tareas de cloud agent.
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, nos ocuparemos de [iterar sobre los pull requests creados por Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents

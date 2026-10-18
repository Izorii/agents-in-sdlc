---
title: "Ejercicio 5: iteración sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisión y gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que añada documentación al código, cree una función de juegos relacionados sobre la que el equipo de diseño pueda iterar e implemente funciones de accesibilidad con controles para el modo de alto contraste y el modo claro. Vamos a revisar los cambios y, si hace falta, enviaremos comentarios a Copilot para que los mejore.

### Escenario

Como ya hemos destacado varias veces, los fundamentos del diseño de software y DevOps no cambian por incorporar IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Con esto en mente, revisaremos las sugerencias de GitHub Copilot para crear la documentación, la función de juegos relacionados y las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y GitHub Copilot cloud agent

Como Copilot cloud agent realiza tareas de forma asíncrona y sin supervisión directa, se han establecido restricciones de seguridad para mantener el entorno protegido:

- Copilot solo tiene acceso de lectura al repositorio y de escritura **únicamente** a la rama que usará para su código.
- Cloud agent se ejecuta en GitHub Actions, que crea un entorno independiente y efímero para trabajar.
- Para ejecutar cualquier flujo de trabajo de GitHub Actions, una persona debe aprobarlo.
- De forma predeterminada, el [acceso a recursos externos está limitado](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por el primer pull request (PR) que ha creado GitHub Copilot cloud agent: el que añade documentación al código. Revisaremos el PR mediante la interfaz habitual de GitHub.com.

> [!NOTE]
> Al explorar el PR, quizá veas un aviso de que un firewall está bloqueando GitHub Copilot. Es **normal**: Copilot tiene acceso limitado a los recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall de Copilot cloud agent][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de pull requests.
3. Abre el pull request cuyo título se parezca a **Add missing documentation**.

> [!NOTE]
> Si Copilot todavía está trabajando, el pull request tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine; puede tardar unos minutos. Aprovecha para descansar o repasar lo que has aprendido.

4. Cuando el pull request esté listo, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye nuevos comentarios TSDoc y otra documentación. Los cambios exactos pueden variar.

    Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y comentarios de cabecera. Se deben a los archivos de instrucciones personalizadas que revisaste al principio del Ejercicio 2.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. Los flujos deberían aparecer en cola en la sección de comprobaciones del pull request. Si todo va bien, las comprobaciones del proyecto deberían terminar correctamente para la aplicación Astro. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot en un pull request no es un proceso unidireccional. También puedes mencionarlo en comentarios del pull request o comentarios insertados en el código, como harías con cualquier miembro del equipo. Copilot los verá e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos especificar exactamente qué pedirle. Puedes solicitar, por ejemplo:

- que añada a la cabecera de cada archivo de código un comentario breve sobre su función.
- que añada comentarios TSDoc a los archivos TypeScript y Astro.
- que cree un README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier persona. Usa alguna de las ideas anteriores o pide otra mejora para la documentación del código.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para aplicar las actualizaciones.
3. Puedes seleccionar **Back to pull request** para volver al pull request.

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, debería aparecer un commit nuevo en el pull request.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Sigue iterando hasta que estés conforme. Después, puedes marcar el PR en borrador como listo para revisión y fusionarlo en la rama principal.

![Marcar el PR como listo para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Volvamos al PR que ha creado Copilot para resolver la incidencia de mostrar juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona el PR cuyo título se parezca a **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
7. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. Los flujos deberían aparecer en cola en la sección de comprobaciones del pull request. Si todo va bien, las comprobaciones del proyecto deberían terminar correctamente para la aplicación Astro. El proceso puede tardar unos minutos.
9. **Opcional:** También puedes cambiar a esta rama en tu Codespace para probar manualmente la función de juegos relacionados. Ve al Codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama que haya creado Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes iterar con Copilot y pedirle actualizaciones. Por ejemplo, puedes solicitar que cambie el número de juegos relacionados o que añada comentarios de cabecera y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Para pedir cambios, añade un comentario nuevo en la pestaña **Conversation**. Copilot lo verá e iniciará otra sesión.

## Revisa las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad que implementó el agente personalizado. El PR debería incluir tanto el modo de alto contraste que asignaste en el Ejercicio 3 como el modo claro que solicitaste desde el panel de control en el Ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona el PR cuyo título se parezca a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot todavía está trabajando, el pull request tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine; puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los componentes de interfaz de usuario para cambiar de modo.
   - el uso del almacenamiento local para guardar las preferencias.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - el código JavaScript o TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. Los flujos deberían aparecer en cola en la sección de comprobaciones del pull request. Si todo va bien, las comprobaciones del proyecto deberían terminar correctamente para la aplicación Astro. El proceso puede tardar unos minutos.
10. **Opcional:** Cambia a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Ve al Codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama que haya creado Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Comprueba que funcionan y que la configuración persiste al recargar la página.

Observa cómo el agente personalizado de accesibilidad ha orientado a Copilot para que implemente las funciones siguiendo las prácticas recomendadas de accesibilidad. Si detectas algún problema o mejora posible, puedes mencionar a **@copilot** en un comentario y pedirle que actualice el código, como hiciste con los PR anteriores.

## Ejercicio opcional: sigue delegando

Cloud agent resulta más útil cuando puede encargarse de tareas reales de la lista de pendientes mientras tú haces otro trabajo. Para practicar, crea varias incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario para registrar el interés de quienes respaldan el proyecto en la página de detalles del juego.
- implementar paginación en la lista de juegos.
- añadir validación de entrada y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el itinerario de cloud agent. En estas lecciones:

- **Revisaste los archivos de instrucciones personalizadas incluidos en el repositorio** para ver cómo influyen en el resultado de cloud agent.
- **Asignaste incidencias a Copilot cloud agent** y observaste cómo configura el entorno, planifica y ejecuta las tareas de forma asíncrona.
- **Creaste y usaste un agente personalizado** para implementar controles de modo de alto contraste y modo claro con atención a la accesibilidad.
- **Usaste la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad mientras estaba en curso.
- **Revisaste los pull requests de cloud agent e iteraste sobre ellos**, mencionando a `@copilot` para pedir cambios y aprobar flujos de trabajo.

## Repasa y continúa

Has completado el itinerario de cloud agent. Los demás itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: implementa y revisa dos cambios desde el terminal con los modos Plan y Autopilot, skills de agente, un agente de QA personalizado, MCP de Playwright, Agent Merge y un puente opcional de `/delegate` a cloud agent.

Prueba también estas ideas en tu propio repositorio:

- asigna a cloud agent una incidencia de refactorización o de cobertura de pruebas.
- crea un agente personalizado para otro dominio.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignación de incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración de Copilot cloud agent][cloud-agent-best-practices]
- [Configuración del firewall de Copilot cloud agent][agent-firewall]

---

| [← Lección anterior: Supervisión y gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/

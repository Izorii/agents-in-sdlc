---
title: "Lección 3: modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: usa Plan para acordar un enfoque, Autopilot para crear el filtrado desde una incidencia e Interactive para revisar y validar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-05
---

Empezamos añadiendo una pequeña funcionalidad al proyecto, pero los cambios de mayor alcance requieren un proceso más sólido. La aplicación GitHub Copilot está diseñada para integrarse en el flujo de trabajo existente de la organización y ayudarnos a crear lo adecuado de la forma correcta. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como se espera y, finalmente, la integrarás correctamente en el proyecto.

> [!NOTE]
> Usarás la misma sesión durante todo el flujo de trabajo de esta funcionalidad. Normalmente usarías sesiones o PR distintos para los diferentes tipos de archivo, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección aprenderás a:

- iniciar una sesión de agente desde una incidencia de GitHub.
- definir requisitos en el modo **Plan**.
- implementar la funcionalidad con el modo **Autopilot**.
- revisar el código.
- validar manualmente la funcionalidad en un lienzo de navegador.

Para completar la funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la skill `quality-checks` existente, añadirás validación con MCP, crearás un agente de QA y abrirás el PR.

## Escenario

El catálogo de Tailspin Toys está creciendo y quienes lo visitan necesitan filtrar los juegos por categoría y editorial. La incidencia pendiente describe la funcionalidad, pero hay que acordar detalles como la combinación de categorías antes de programar. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los fundamentos; al contrario, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Abrir una incidencia que detalle lo que debe hacerse.
2. Crear un plan de lo que se debe desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear un pull request (PR).
7. Fusionar el código una vez revisado y cuando el proceso de integración continua haya finalizado correctamente.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos siguen una variante de los pasos anteriores.

Seguir este enfoque estándar ayuda a garantizar que el código generado por IA cumpla los requisitos establecidos y pase por el mismo proceso de revisión que el código escrito a mano.

## Modos de sesión

El **modo de sesión** determina cuánta autonomía tiene el agente. Puedes elegirlo en el menú desplegable situado debajo del campo de prompt y cambiarlo en cualquier momento:

- **Interactive**: trabajas con el agente, que propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan. Revisas y apruebas el plan antes de que el agente lo ejecute.
- **Autopilot**: el agente trabaja de forma totalmente autónoma: escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Inicia una sesión desde la incidencia

Antes de empezar, confirma que el PR de las valoraciones con estrellas se ha fusionado y que tu `main` local está actualizado.

1. Selecciona **Issues** y abre **Allow users to filter games by category and publisher**.
2. Selecciona **New session** y elige un **new working tree** basado en `main` actualizado.

    ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modo.

## Planifica la funcionalidad de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene la solicitud de la funcionalidad como contexto. Envía:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye el filtrado por categoría y editorial, controles accesibles, cambios de acceso a datos y pruebas. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando ningún juego coincide.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobación de tipos con las herramientas existentes del proyecto. Céntralo en implementar y probar el filtrado; crearás el PR después de completar el flujo de calidad. Pide cambios en el plan antes de aprobarlo y conserva la URL de la incidencia y las aclaraciones acordadas para validarlas más adelante.

## Aprueba Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement with autopilot** o la opción equivalente de tu versión. Confirma que el indicador de modo muestra **Autopilot**.

Copilot comenzará a implementar la funcionalidad. Verás cómo sigue el plan establecido, genera código e incluso ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación inmediatamente, así que revisa el plan primero. Si Copilot informa de dependencias ausentes o de un conflicto de puerto, resuelve el problema de configuración antes de dar las comprobaciones por completadas. Detén solo los servidores que hayas iniciado.

## Revisa y valida la implementación

Una vez generado el código, hay que revisarlo antes de fusionarlo, igual que cualquier otro código. Revisemos los cambios y ejecutemos el sitio para comprobar que todo funciona correctamente.

1. Abre **Changes** y examina la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de varias categorías y editoriales. Comprueba que los cambios siguen las instrucciones existentes del repositorio.
3. Examina la salida de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los fallos y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa y puede reutilizar un servidor local. Asegúrate de que el servidor probado pertenece a este worktree y no a una lección anterior.

## Prueba la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Iniciemos la aplicación como antes y abramos el sitio en un lienzo de navegador.

1. Utiliza este prompt para pedir a Copilot que inicie la aplicación y abra la página en el lienzo de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. En unos instantes, la aplicación se iniciará y se abrirá una ventana de navegador dentro de la aplicación Copilot.
3. Confirma que las tarjetas de juegos valorados muestran su puntuación sobre cinco.
4. Cuando termines, pide a Copilot que detenga el servidor de desarrollo iniciado en esta sesión:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Resumen y siguientes pasos

Has usado distintos modos de agente para desarrollar y revisar una funcionalidad. En esta lección:

- iniciaste una sesión de agente desde una incidencia de GitHub.
- definiste requisitos en el modo **Plan**.
- implementaste la funcionalidad con el modo **Autopilot**.
- revisaste el código.
- validaste manualmente la funcionalidad en un lienzo de navegador.

A continuación, profundizarás en la generación de código y te asegurarás de que siga las prácticas documentadas mediante las [instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions

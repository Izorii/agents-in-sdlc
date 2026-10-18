---
title: "Lección 3: modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear filtros desde una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto, pero los cambios de mayor alcance requieren un proceso más sólido. GitHub Copilot CLI está diseñado para integrarse en los flujos de trabajo que ya usa tu organización y ayudarte a crear lo adecuado de la manera correcta. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que funciona como se espera y, por último, lo fusionarás en el proyecto.

> [!NOTE]
> Seguirás trabajando en la misma conversación y rama durante el flujo de trabajo de esta funcionalidad. Normalmente se usarían ramas o PR distintos para los diferentes tipos de archivo, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección aprenderás a:

- iniciar una conversación nueva de Copilot desde una incidencia de GitHub.
- definir requisitos en el modo Plan.
- implementar la funcionalidad con el modo Autopilot.
- revisar el código.
- validar manualmente la funcionalidad en un navegador reenviado.

Para completar esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la skill existente `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás el PR.

## Escenario

El catálogo de Tailspin Toys está creciendo y quienes lo visitan necesitan filtrar los juegos por categoría y editorial. La incidencia pendiente describe la funcionalidad, pero antes de empezar a programar hay que acordar algunos detalles, como la combinación de categorías. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los fundamentos; al contrario, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia que describa el trabajo.
2. Crear un plan de implementación.
3. Programar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear un pull request (PR).
7. Fusionar el código cuando se haya revisado y el proceso de integración continua haya terminado correctamente.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos siguen una variante de los pasos anteriores.

Seguir este enfoque estándar ayuda a garantizar que el código generado por IA cumple los requisitos y pasa por el mismo proceso de revisión que el código escrito a mano.

## Modos de conversación

El **modo de conversación** determina cuánta autonomía tiene el agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente, que propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas y hace iteraciones hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empieza por la incidencia

Antes de empezar a trabajar en los filtros, vuelve a tu Codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha fusionado el PR de la valoración con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza por `#7` (o un número parecido). El símbolo `#` permite añadir al contexto una incidencia o un pull request (PR) de GitHub.

## Planifica la funcionalidad de filtrado

Planificar permite definir el enfoque antes de pedir a Copilot que implemente una funcionalidad o realice una tarea. Conviene dedicar tiempo a planificar cualquier trabajo complejo. Cambia al modo Plan y pide a Copilot que prepare el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador situado debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, escribe este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque para preparar el plan.

3. Es posible que Copilot te pregunte cómo deberían funcionar los filtros. Responde según tus preferencias; no hay una única respuesta correcta.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que revise cualquier parte que no refleje tus decisiones.

## Aprueba Autopilot

Ya tienes un plan revisado; ahora toca implementarlo. Deja que Copilot trabaje en modo Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta considerar que ha terminado.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción con un nombre parecido en la versión instalada.
2. Confirma que el indicador situado debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente. Revísalo antes de aprobarlo. Si Copilot indica que faltan dependencias o que hay un conflicto de puertos, resuelve el problema de configuración antes de dar por terminadas las comprobaciones.

## Revisa y valida la implementación

Una vez generado el código, hay que revisarlo antes de fusionarlo, igual que cualquier otro código. Revisa los cambios y ejecuta el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` y revisa la implementación de los filtros y sus pruebas.
3. Compara el resultado con la incidencia y con las decisiones que tomaste al planificar.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista diff.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los errores.

## Prueba la nueva funcionalidad

El código parece correcto, pero ¿funciona? Como antes, inicia la aplicación y abre el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editorial, así como las combinaciones que acordaste en el plan.
4. Confirma que el comportamiento al restablecer los filtros y cuando no hay resultados coincide con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- iniciaste una conversación nueva de Copilot desde una incidencia de GitHub.
- definiste requisitos en el modo Plan.
- implementaste la funcionalidad con el modo Autopilot.
- revisaste el código.
- validaste manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizarás en la generación de código y en cómo seguir las prácticas documentadas mediante las [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/

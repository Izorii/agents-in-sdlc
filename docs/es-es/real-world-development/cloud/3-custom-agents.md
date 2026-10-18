---
title: "Ejercicio 3: agentes personalizados"
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

Explorarás los siguientes temas relacionados con los agentes personalizados:

- cómo se definen.
- cómo se asigna una tarea a uno.

## Escenario

Tailspin Toys se compromete a que cualquier persona pueda usar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes indican que a algunas personas les cuesta leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es prioritaria, quieres implementarlo cuanto antes. Para generar la funcionalidad, usarás un agente personalizado.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para trabajar con la accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve al Codespace y revisa el archivo del agente personalizado de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en los metadatos YAML `name` y `description`.

> [!CAUTION]
> Los metadatos `name` y `description` son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores habituales que conviene evitar.

## Crea y asigna una incidencia

El panel de agentes es el centro de control de todos los agentes de tu entorno. Permite asignar tareas a Copilot cloud agent, supervisarlas y redirigir el agente o proporcionarle indicaciones adicionales. Vamos a asignar a Copilot la implementación del modo de alto contraste.

1. Ve a tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nuevas incidencias.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Add high contrast mode to website`.
6. En **Description**, escribe:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Asignación de cloud agent con el agente personalizado y Accessibility resaltados](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados en tareas y dominios concretos. Los agentes permiten codificar la experiencia y los estándares del equipo en agentes reutilizables que orientan a Copilot para que realice determinados tipos de trabajo con mayor eficacia.

Has explorado cómo:

- se definen los agentes personalizados.
- se asigna una tarea a un agente personalizado.

Mientras Copilot implementa el modo de alto contraste, nos ocuparemos de [supervisar y orientar la sesión del agente][next-lesson] desde el panel de control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparación para usar agentes personalizados en la organización][org-custom-agents]
- [Preparación para usar agentes personalizados en la empresa][enterprise-custom-agents]

---

| [← Lección anterior: GitHub Copilot cloud agent][previous-lesson] | [Siguiente lección: Supervisión y gestión de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents

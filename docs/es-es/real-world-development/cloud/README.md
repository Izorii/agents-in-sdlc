---
slug: es-es/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent) permite que GitHub Copilot trabaje de forma asíncrona en la nube. Asignas el trabajo en GitHub y cloud agent lo realiza en segundo plano: explora el repositorio, hace cambios y abre un pull request mientras tú te ocupas de otras tareas.

En estos ejercicios añadirás instrucciones personalizadas que seguirá cloud agent y, después, le asignarás una incidencia de GitHub para que implemente el trabajo. Revisarás los resultados y usarás agentes personalizados para orientar su enfoque, supervisarás y dirigirás sesiones desde el panel de agentes y terminarás revisando los pull requests e iterando sobre los resultados.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea el repositorio y el Codespace |
| [1. Instrucciones personalizadas][ex1] | Contexto | Añade instrucciones personalizadas que seguirá cloud agent |
| [2. Cloud agent][ex2] | Agente asíncrono | Asigna incidencias a Copilot cloud agent |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revisa y usa agentes personalizados |
| [4. Supervisión y gestión de agentes][ex4] | Supervisión | Supervisa y dirige sesiones de agentes |
| [5. Iteración][ex5] | Revisión | Revisa pull requests, itera sobre el trabajo de Copilot y decide los siguientes pasos |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Copilot cloud agent habilitado para tu cuenta u organización

> [!TIP]
> ¿No tienes un plan de pago? El alumnado que cumpla los requisitos puede obtener GitHub Copilot gratis a través de [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que se usan en este taller; te permitirá completar todos los itinerarios.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent está disponible en todos los planes excepto Copilot Free: **Copilot Student, Pro, Pro+, Business y Enterprise**. En Copilot Business o Enterprise, un administrador debe habilitarlo para la organización.

> [!NOTE]
> Este itinerario no cubre MCP. Para explorar el uso de servidores MCP con Copilot, consulta los [ejercicios de CLI](../cli/) o de [VS Code](../vscode/).

## Empieza

**[Empieza por el Ejercicio 0: requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/

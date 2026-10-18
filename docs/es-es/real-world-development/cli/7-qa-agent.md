---
title: "Lección 7: crea y usa un agente de QA"
description: "Crea un agente personalizado de QA que combine los requisitos de la incidencia, la skill quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la skill `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar la experiencia de filtrado en un navegador. Ahora combinarás estas capacidades en un agente personalizado con un proceso de QA bien definido.

En esta lección aprenderás a:

- explorar cómo un agente personalizado trabaja con instrucciones, skills y herramientas MCP.
- crear e inspeccionar un perfil reutilizable de control de calidad.
- seleccionar el agente de QA y revisar sus conclusiones según la incidencia de filtrado.

## Escenario

Antes de abrir un pull request (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar el proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot, definida mediante un perfil Markdown. El perfil describe su propósito, sus instrucciones y las herramientas disponibles. En este taller, definirás un rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has usado tienen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La skill `quality-checks` agrupa comprobaciones repetibles. Playwright MCP proporciona herramientas para el navegador. El perfil de QA indica a Copilot cómo usar esas capacidades para evaluar los requisitos y comunicar los resultados. No sustituye estos elementos ni requiere iniciar otra conversación.

## Crea el perfil de QA

Antes de abrir el PR de la funcionalidad, pide a Copilot que cree un perfil de QA reutilizable. El perfil definirá las comprobaciones y los límites que debe respetar el agente.

1. Vuelve al Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el nuevo agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecciona el perfil

Antes de usar el nuevo agente, revisa el perfil para confirmar que Copilot ha recogido el flujo de trabajo y los límites previstos. Así evitarás que un agente incompleto o con un alcance demasiado amplio cambie la funcionalidad cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee los metadatos. `description` es obligatorio; `name` es opcional, pero proporciona al agente un nombre claro en la interfaz.
3. Lee las instrucciones del perfil y confirma que QA parte de los requisitos, sigue las instrucciones del repositorio, ejecuta la skill `quality-checks` y usa Playwright MCP.
4. Confirma que QA presenta pruebas de apoyo, pide permiso antes de cambiar el código de implementación y no confirma cambios ni abre pull requests.
5. Si al perfil generado le falta alguna responsabilidad o límite, pide al agente predeterminado que lo revise antes de continuar.

## Ejecuta QA según la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Después de crear el perfil, retoma la conversación de filtrado y selecciona QA para que pueda usar la incidencia y las decisiones de planificación que ya están en el contexto.

1. Activa el agente con este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, quizá no aparezca en la lista de agentes disponibles. Aun así, el comando anterior lo activará.

2. Envía este prompt para pedir al agente de QA que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. El agente de QA empezará a trabajar.
4. Cuando termine, lee el informe.

## Resumen y siguientes pasos

Has añadido un rol especializado y reutilizable al flujo de trabajo y has revisado su resultado. En esta lección:

- has explorado cómo un agente personalizado trabaja con instrucciones, skills y herramientas MCP.
- has creado e inspeccionado un perfil reutilizable de control de calidad.
- has seleccionado el agente de QA y revisado sus conclusiones según la incidencia de filtrado.

Ya tienes lista la implementación, la actualización de la skill, el perfil de QA, las pruebas y el informe de verificación. A continuación, [integrarás estos elementos en un PR de funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration

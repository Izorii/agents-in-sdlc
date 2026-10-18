---
title: "Lección 10: resumen y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, los dos pull requests, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI durante un flujo continuo de trabajo en Tailspin Toys. En concreto:

- preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y encontraste la incidencia de filtrado precargada.
- añadiste valoraciones con estrellas, revisaste el resultado en un navegador reenviado y fusionaste manualmente tu primer pull request (PR).
- partiste de la incidencia de filtrado, definiste el enfoque en el modo Plan, lo implementaste en el modo Autopilot y lo revisaste en el modo Interactive.
- orientaste al agente con instrucciones personalizadas, personalizaste la skill `quality-checks` existente y la usaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadiste el servidor Model Context Protocol (MCP) de Playwright y lo usaste para explorar los filtros en un navegador real.
- creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la skill y las pruebas en el navegador.
- revisaste todos los cambios del filtrado y autorizaste Agent Merge para el PR del filtrado.
- exploraste comandos de barra para gestionar el contexto y los modelos, compartir sesiones y delegar tareas en la nube de forma opcional.

## Qué has publicado

El taller incluye dos hitos de PR, cada uno en su propia rama basada en `main` actualizada:

1. **Valoraciones con estrellas:** mostrar en las tarjetas de juegos el valor existente de `starRating` y un estado explícito para los juegos sin valoración.
2. **Filtrado y flujo de trabajo de calidad:** implementar filtros, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe de `quality-checks`, crear un perfil de QA e incluir las pruebas relacionadas.

Desde la planificación del filtrado hasta la apertura del PR, has usado la misma conversación y la misma rama. Hemos agrupado ese trabajo en un solo PR para simplificar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, una inspección del sitio en el navegador y la exploración del navegador por parte de Copilot mediante MCP. La skill `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y presentó los resultados en el nuevo formato. QA combinó esos resultados con una revisión de los requisitos y la cobertura de pruebas antes del PR.

Las pruebas añadidas deben cubrir carencias reales; una ejecución de QA que no necesite pruebas nuevas puede ser correcta. Revisa el código y las pruebas antes de autorizar una fusión, y actualiza las pruebas afectadas después de realizar cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller has actualizado instrucciones, personalizado una skill, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y ajústalas a las necesidades cambiantes del equipo. Las instrucciones fijan estándares, las skills describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados reales de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para la tarea. Usa **Plan** para analizar el enfoque antes de implementarlo, **Interactive** para intervenir mientras se realizan cambios específicos y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para cambios rutinarios y otro más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye notablemente en el resultado.

## Más opciones para explorar

Ya has completado el flujo de trabajo principal. También puedes probar otras funciones de la CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para analizar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea a Copilot cloud agent.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Úsala con código de producción, en proyectos personales o en esa pequeña aplicación que llevas años queriendo crear. Comparte lo aprendido con tu equipo y aprende de sus experiencias. Y, como siempre, consulta la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller de Copilot cloud agent][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar pull requests con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests

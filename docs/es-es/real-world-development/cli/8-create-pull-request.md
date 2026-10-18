---
title: "Lección 8: crea y fusiona el PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la actualización de la skill, el perfil de QA y las pruebas; después, crea un PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la skill, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Es hora de revisarlos en conjunto y abrir un pull request. Tú mismo has fusionado el pull request (PR) de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente dividiríamos la funcionalidad, las actualizaciones de instrucciones, la actualización de la skill y el agente de QA en varios PR. Para simplificar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una conversación y una rama, y lo incluirás en un solo PR.

En esta lección aprenderás a:

- descubrir qué es Agent Merge y cómo automatiza el ciclo de vida de una fusión.
- inspeccionar todos los cambios de la funcionalidad y las pruebas de verificación.
- crear el PR del filtrado.
- habilitar Agent Merge solo después de la revisión y confirmar que el PR se ha fusionado.

## Escenario

Durante el flujo de trabajo de filtrado has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante del PR, pero mantener bajo el control del desarrollador la autorización de la fusión.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para completar un pull request. Al habilitarlo, Copilot resuelve los obstáculos del PR (corrige comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a basar la rama cuando hace falta) y, después, habilita la fusión automática de GitHub si el repositorio lo permite.

Hasta ahora, has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esta responsabilidad. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Usa Agent Merge para gestionar el PR

Ya has creado todo el código. Vamos a revisarlo en conjunto, crear el PR y dejar que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea un PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua del PR. Cuando todo termine correctamente, lo fusionará.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni la falta de permisos. Si el proceso se bloquea, lee el motivo indicado y completa manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varios aspectos del proceso de desarrollo: la generación de código, las pruebas y la validación del código y, ahora, el proceso del pull request. En esta lección:

- has descubierto qué es Agent Merge y cómo automatiza el ciclo de vida de una fusión.
- has inspeccionado todos los cambios de la funcionalidad y las pruebas de verificación.
- has creado el PR del filtrado.
- has habilitado Agent Merge solo después de la revisión y confirmado que el PR se ha fusionado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto y los modelos, compartir sesiones y delegar trabajo en la nube de forma opcional.

## Recursos

- [Gestionar pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference

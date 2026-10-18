---
title: "Lección 9: comandos de barra de GitHub Copilot CLI"
description: "Descubre comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar trabajo en la nube de forma opcional."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como toda buena herramienta de CLI, GitHub Copilot CLI incluye muchos comandos de barra para interactuar con ella. Estos comandos dan acceso a funciones avanzadas, información interna y opciones de configuración adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Veamos otros que pueden resultar útiles.

En esta lección aprenderás a:

- usar `/context` y `/compact` para ver cómo gestiona Copilot el contexto de la conversación.
- usar `/model` para explorar los modelos disponibles.
- descubrir cómo `/share` puede exportar o compartir una sesión.
- explorar comandos opcionales para trabajar en paralelo, usar worktrees y delegar tareas al agente en la nube.

## Escenario

Ya has terminado el flujo de trabajo principal de CLI. Ahora explorarás otras capacidades: gestionar el contexto, cambiar de modelo, compartir sesiones y, de forma opcional, delegar trabajo a [Copilot cloud agent][about-cloud-agent].

## Explora el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, es posible que te acerques al límite de la ventana de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando hace falta y te permite inspeccionar o compactar el contexto con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio, si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre instrucciones del sistema, herramientas, mensajes y espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya era breve, quizá no haya grandes diferencias.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente cuando la ventana se va llenando. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` al cambiar a una tarea no relacionada y querer empezar una conversación nueva.

## Elige el modelo

Cada modelo tiene sus puntos fuertes y cada desarrollador sus preferencias. Copilot CLI permite consultar y seleccionar el modelo que quieras usar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Colaborar y compartir lo aprendido ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un GitHub gist.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en la versión instalada.
2. Si quieres compartir la sesión, elige el destino que mejor se adapte a tus necesidades, como `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio ni de la conversación que el equipo no quiera compartir.

## Opcional: amplía el trabajo o delégalo

Has terminado el taller principal. Copilot CLI también ofrece comandos para tareas de mayor alcance:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un worktree de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea a Copilot cloud agent, que trabaja de forma asíncrona y puede abrir un pull request.

Estos comandos son opcionales, ya que pueden crear worktrees adicionales o iniciar trabajo remoto. Antes de probarlos, empieza una tarea nueva y bien delimitada, y revisa el resultado con tu flujo de trabajo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller de cloud agent][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI te permiten configurarlo, compartir sesiones y consultar lo que ocurre internamente. En esta lección:

- usaste `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usaste `/model` para explorar los modelos disponibles.
- aprendiste cómo `/share` puede exportar una sesión o compartirla.
- exploraste comandos opcionales para trabajar en paralelo, usar worktrees y delegar tareas al agente en la nube.

Copilot CLI ofrece muchos más comandos de barra que puedes explorar. Para terminar el recorrido, [repasaremos lo aprendido][next-lesson] y veremos cómo seguir aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca de Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/

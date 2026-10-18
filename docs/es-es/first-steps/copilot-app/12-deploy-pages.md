---
title: "Lección 12: publicar el sitio con GitHub Pages"
description: "Usa la aplicación de GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su pull request y comprobar que Space Quiz está disponible."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica Space Quiz con GitHub Pages para que todo el mundo pueda acceder. Copilot puede preparar el flujo de trabajo y guiarte durante la publicación, pero tú sigues siendo responsable de revisar los cambios y confirmar que el sitio está disponible.

En esta lección aprenderás a:

- pedir a Copilot que configure una implementación en GitHub Pages.
- revisar el flujo de trabajo y el pull request antes de fusionarlo.
- seguir la implementación y comprobar el sitio publicado.

## Escenario

Space Quiz ya está en un repositorio de GitHub, pero todavía hace falta una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages lo hará accesible desde una URL pública, sin añadir un servidor ni dependencias de ejecución.

## Pide a Copilot que publique el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de tres etapas: un pull request de space-quiz fusionado; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz disponible en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pide al agente que cree y fusione un pull request y cambie la configuración de Pages del repositorio. Revisa el diff, los permisos de implementación y las solicitudes de aprobación de GitHub antes de continuar.

## Revisa el flujo de trabajo y comprueba la implementación

1. Confirma que el archivo README solo se añade si el repositorio aún no tiene uno.
2. Revisa los cambios del pull request, incluidos los pasos de compilación e implementación del flujo de trabajo.
3. Fusiona el pull request solo cuando se superen las comprobaciones obligatorias.
4. Sigue la ejecución de implementación de GitHub Actions hasta que termine correctamente.
5. Abre la URL indicada y confirma que Space Quiz se carga.

Si el agente no puede habilitar Pages o fusionar el pull request, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que la implementación finalice correctamente y el sitio esté disponible.

## Resumen y siguientes pasos

Space Quiz ya está disponible como sitio web y has comprobado la implementación en lugar de fiarte únicamente del informe del agente. Continúa con la [Lección 13: revisión y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/

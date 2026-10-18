---
title: "Lección 5: personaliza y usa una skill de comprobaciones de calidad"
description: "Explora la skill quality-checks existente, personaliza el formato del informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escribir código implica mucho más que limitarse a escribirlo. Ya hemos podido comprobar manualmente que funciona y hemos usado archivos de instrucciones para asegurarnos de que sigue nuestros estándares. Pero ¿qué ocurre con las pruebas, lint y el resto de las tareas de integración continua (CI)?

Para este tipo de tareas, las **skills de agente** son la mejor opción. Ayudan a Copilot a entender cómo ejecutar correctamente operaciones como estas.

En esta lección aprenderás a:

- explorar la skill `quality-checks` existente.
- personalizar el formato de sus resultados.
- ejecutar la skill y revisar su salida.

## Escenario

Tailspin Toys usa la skill `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las skills de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos de apoyo que un agente carga cuando los necesita. En esencia, son una carpeta con el nombre de la skill que incluye un archivo Markdown llamado `SKILL.md`. El Markdown contiene metadatos con un nombre y una descripción que definen la skill, información general sobre lo que hace e indicaciones sobre cuándo debe invocarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos que la skill puede usar.

> [!NOTE]
> ¡Una skill no necesita carpetas ni archivos adicionales! En este ejemplo, nuestra skill ejecutará comandos `npm` para ejecutar las pruebas y los linters. Por eso, no necesita archivos de apoyo adicionales.

Las skills pueden residir en la carpeta `.github/skills` del proyecto para compartirlas y reutilizarlas en el repositorio, o en la carpeta de skills del usuario `~/.copilot/skills`.

## Explora la skill

Explora la skill que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos: `quality-checks`.

1. Si todavía no tienes abierto un lienzo **Files**, selecciona **+** en el panel de revisión y, después, **File**.
2. Busca `.github/skills/quality-checks/SKILL.md`.
3. Lee los campos `name` y `description` de la parte superior. La descripción ayuda a Copilot a saber cuándo debe invocar la skill.
4. Lee las instrucciones y observa cómo orientan a Copilot en el proceso de pruebas y lint.

## Ejecuta la skill antes de cambiarla

Puedes invocar las skills directamente con un comando de barra o con lenguaje natural. Pidamos a Copilot que ejecute las tres comprobaciones de la skill.

1. Selecciona el modo **Interactive** en el menú desplegable.
2. Envía este prompt para invocar la skill:

    ```plaintext
    Run the quality-checks skill for unit tests, lint, and type checks.
    ```

3. Observa el informe final.

## Personaliza el informe

Queremos un informe mejor que indique qué se ha ejecutado, si ha tenido éxito y qué han comunicado realmente las herramientas. Vamos a actualizar la skill para generarlo.

1. Vuelve al lienzo **Files**.
2. Si aún no está abierto, abre `.github/skills/quality-checks/SKILL.md`.
3. Añade esta sección al final del archivo:

    ```markdown
    ## Results output formatting

    Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
    ```

El archivo se guarda automáticamente.

## Ejecuta la skill actualizada

Ya has hecho el cambio; vamos a ver cómo funciona. Usaremos exactamente el mismo prompt que antes.

1. Selecciona el modo **Interactive** en el menú desplegable.
2. Envía este prompt para invocar la skill:

    ```plaintext
    Run the quality-checks skill for unit tests, lint, and type checks.
    ```

3. Observa el informe final.

## Resumen y siguientes pasos

Has personalizado y usado una skill de agente existente. En esta lección:

- has explorado la skill `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has personalizado el formato de sus resultados.
- has ejecutado la skill y revisado su salida.

Este cambio acompañará al filtrado en el PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de skills

Estos ejemplos de la comunidad son referencias, no tareas adicionales. Revisa sus requisitos previos y su comportamiento antes de adoptarlos:

- [Especificación de Agent Skills][skill-spec].
- [Flujo de contribución: `make-repo-contribution`][contribution-example].
- [Documentos de requisitos: `prd`][prd-example].
- [Diagramas y un script de exportación incluido: `drawio`][drawio-example].
- [Pruebas de navegador: `webapp-testing`][browser-example].

El ejemplo de contribución original se llama `make-repo-contribution`; las plantillas antiguas de Tailspin usaban otro nombre, `make-contribution`. Este taller no depende de ninguna de esas skills de contribución.

[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing

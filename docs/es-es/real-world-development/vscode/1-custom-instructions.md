---
title: "Ejercicio 1: instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe hacerse de una forma determinada, o Copilot necesita información de fondo, ese contexto debe estar disponible. Hay varias formas de compartirlo; una de las principales son los [archivos de instrucciones][instruction-files], que proporcionan indicaciones para generar código.

En este ejercicio aprenderás a:

- explorar cómo el contexto específico del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante instrucciones personalizadas del repositorio y archivos de instrucciones específicos para determinadas rutas.
- generar la primera parte de los datos para el filtrado (un helper de editoriales) con las instrucciones actuales.
- añadir un estándar general para el repositorio a `.github/copilot-instructions.md`.
- volver a enviar el mismo prompt y observar cómo el código generado adopta el nuevo estándar.
- confirmar los cambios de instrucciones y del filtrado y enviarlos a `main` para que Copilot use las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir algunos de los estándares indicados. Copilot es probabilístico. El objetivo de este ejercicio es observar la tendencia del cambio de comportamiento al actualizar las instrucciones, no obtener exactamente el mismo código carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de programación:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su función.

Usarás archivos de instrucciones para que Copilot disponga de la información necesaria y pueda realizar las tareas conforme a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor el estilo y los requisitos de programación. Permiten orientar sus sugerencias y fragmentos de código. Puedes especificar las convenciones de programación, las bibliotecas y los comentarios que prefieres. Las instrucciones pueden aplicarse a todo el repositorio o a tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto que sea pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción del proyecto, las prácticas recomendadas y directrices generales.
- Los archivos `.github/instructions/*.instructions.md` sirven para tareas o tipos de archivo concretos. Puedes usarlos para definir directrices para lenguajes como TypeScript o Astro, o para tareas como crear componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat; no se aplican al autocompletado ni a las sugerencias de edición siguiente.
>
> Copilot Chat, Copilot CLI y Copilot cloud agent usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con metadatos `applyTo`.
>
> Copilot también [admite otros formatos de archivos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Recomendaciones para gestionar archivos de instrucciones

Una conversación completa sobre la creación de archivos de instrucciones queda fuera del alcance del taller. A grandes rasgos, los ejemplos del proyecto muestran un enfoque representativo:

- Limita `copilot-instructions.md` a directrices generales del proyecto, como su descripción, estructura y estándares globales de programación.
- Usa archivos `*.instructions.md` para tipos de archivo (pruebas unitarias, componentes Astro o capa de datos) o tareas específicas.
- Escribe las directrices en lenguaje natural, de forma clara y con ejemplos de código correcto e incorrecto.

No existe una única forma de crear archivos de instrucciones ni de usar la IA. La experimentación te ayudará a descubrir qué funciona mejor en tu proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener un conjunto sólido de archivos de instrucciones. Al explorar los del proyecto, verás que cubren muchos tipos de tareas, como las [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada interfaz ofrece esta opción de forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). La lección correspondiente a cada interfaz lo indicará cuando sea pertinente.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Verás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros aspectos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define un patrón glob, relativo a la raíz del repositorio, que determina a qué archivos se aplican las instrucciones. En este caso, coincide con los archivos de prueba TypeScript, por ejemplo, los que siguen el patrón `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentar el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los comentarios de cabecera de los archivos.

## Crea una rama para los cambios

Sigamos algunas prácticas recomendadas y creemos una rama para los cambios.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; se indicarán cuando corresponda.

Para ver el efecto de las instrucciones personalizadas, primero envía un prompt con el archivo de instrucciones actual. Después lo actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas de editor abiertas de ejercicios anteriores para que Copilot use solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el helper.
3. Selecciona **Agent** en el selector de agentes de la vista Chat para que Copilot pueda aplicar cambios a los archivos.

   ![Selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará actualizaciones de código, a menudo en el archivo del helper y sus pruebas.
6. Observa que el helper propuesto es una función tipada que acepta un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el comentario de cabecera del archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que incluya comentarios aunque no se los hayas pedido. No pasa nada: el objetivo es observar la mejora de la coherencia al actualizar las instrucciones.

## Añade un estándar general del repositorio

Como ya se ha indicado, `.github/copilot-instructions.md` contiene información general del proyecto para Copilot. Vamos a documentar los estándares de programación del repositorio para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Documenta los estándares del proyecto, pero todavía no incluye ninguna regla sobre documentación en el código; por eso el helper generado no tenía comentarios.
3. Añade estas líneas Markdown justo debajo de los estándares existentes para pedir a Copilot comentarios de cabecera y documentación TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, las instrucciones pueden estar en el repositorio (`.github/copilot-instructions.md`) para ofrecer directrices generales, o en archivos `*.instructions.md` para determinados lenguajes, tipos de archivo o tareas. El archivo del repositorio es el lugar adecuado para estándares generales como la regla de comentarios de documentación que acabas de añadir.

## Vuelve a enviar el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el búfer.
2. Haz clic en `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía **el mismo prompt**:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que ahora el archivo propuesto empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar de proyecto y lo aplique a código real que se usará en el siguiente ejercicio.

## Confirma las instrucciones y envía la rama

Los archivos de instrucciones son elementos del repositorio y se gestionan con el mismo control de versiones que los demás. Vamos a confirmar y enviar los cambios de la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba desde el terminal que están presentes las actualizaciones de instrucciones y del helper con este comando:

   ```bash
   git status
   ```

3. Añade y confirma los cambios de instrucciones y todos los archivos que Copilot haya actualizado para crear la base del helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y fusiona un pull request

Con la rama enviada, crea un pull request y relaciónalo con la incidencia de estándares de documentación de la lista de tareas pendientes. Copilot ya tiene conectadas las herramientas de GitHub necesarias, así que puedes pedirle que busque la incidencia, cree un PR asociado y lo fusione.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actuará en GitHub por ti. La plantilla del proyecto ya configura las herramientas de GitHub en el espacio de trabajo. No tienes que configurar nada; es posible que VS Code te pida iniciar sesión en GitHub la primera vez que Copilot use una de ellas. Sigue las indicaciones para autorizarlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en el Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia sobre la actualización de los archivos de instrucciones y que cree un PR desde la rama actual, describiendo tanto las actualizaciones de instrucciones como la nueva base del helper de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Cuando Copilot te pida **Allow** para realizar acciones en GitHub, revisa el comando y selecciona **Allow** si procede.
5. Cuando se cree el PR, pide a Copilot que lo fusione y vuelva a `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Cuando Copilot te pida **Allow** para realizar acciones en GitHub y ejecutar comandos de shell, revisa el comando y autorízalo si procede.

¡Ya has creado y fusionado un pull request con GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar qué produce con las instrucciones *existentes*.
- añadir un estándar general al repositorio en `.github/copilot-instructions.md`.
- volver a enviar el mismo prompt y ver cómo el código propuesto adopta el nuevo estándar.
- confirmar los cambios de instrucciones y la base del helper en `main` para que el siguiente ejercicio pueda usarlos.

A continuación, pondrás en práctica esas instrucciones con el [modo agente][next-lesson], mientras Copilot añade una funcionalidad en varias partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot

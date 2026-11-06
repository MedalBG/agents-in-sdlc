---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene proporcionar a Copilot ese contexto. Los [archivos de instrucciones][instruction-files] indican cómo generar código y son una de las formas principales de comunicarle directrices.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de código y los estándares de documentación mediante las instrucciones del repositorio y archivos específicos de rutas.
- generarás la primera parte de la capa de datos para el filtrado (una función auxiliar de editores) con las instrucciones actuales.
- añadirás un estándar general al archivo `.github/copilot-instructions.md`.
- repetirás el mismo prompt y observarás cómo el código generado sigue el estándar nuevo.
- confirmarás los cambios de instrucciones y de filtrado en `main` para que Copilot pueda utilizarlos en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares. Copilot es probabilístico; el objetivo es observar la tendencia después de actualizar las instrucciones, no obtener siempre el mismo resultado exacto.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices para sus prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe utilizar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que explique su función.

Utilizarás archivos de instrucciones para proporcionar a Copilot la información necesaria para trabajar de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot. Puedes especificar convenciones, bibliotecas y comentarios para orientar las sugerencias de código. Hay dos tipos:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio.
- `.github/instructions/*.instructions.md`, archivos para determinados lenguajes, tipos de archivo o tareas.

> [!NOTE]
> En el IDE, los archivos de instrucciones se utilizan para generar código en Copilot Chat, pero no para completar código ni para sugerir la siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan los archivos `*.instructions.md` con frontmatter `applyTo`.
>
> Copilot admite también otros formatos de instrucciones, como AGENTS.md y CLAUDE.md; consulta la [referencia de compatibilidad][custom-instructions-support].

### Prácticas recomendadas para gestionar archivos de instrucciones

- Mantén `copilot-instructions.md` centrado en directrices generales del proyecto, como su descripción, estructura y estándares globales.
- Utiliza `*.instructions.md` para tipos de archivo (pruebas unitarias, componentes de Astro y capa de datos) o tareas concretas.
- Escribe en lenguaje natural y presenta las directrices con claridad, incluidos ejemplos de lo que se debe y no se debe hacer.

No hay un único modo correcto de crear archivos de instrucciones; experimenta para descubrir qué funciona mejor en tu proyecto.

> [!TIP]
> Los proyectos con GitHub Copilot deben tener una colección sólida de instrucciones. En este proyecto encontrarás archivos para [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede generar instrucciones. Por ejemplo, VS Code ofrece **Configure Chat → Generate Agent Instructions**, y Copilot CLI, `/init`. Explora [awesome-copilot][awesome-copilot] para encontrar plantillas y ejemplos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Abre los archivos de instrucciones del repositorio en el editor o en la interfaz web de GitHub:

1. Abre `.github/copilot-instructions.md`.
2. Observa la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**, incluida la sección anidada **GitHub Actions Workflows**.
3. Explora `.github/instructions`. Hay instrucciones para Astro, la capa de datos Drizzle, pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo`, un patrón relativo a la raíz del repositorio que coincide, por ejemplo, con `**/*.test.ts`.
5. Examina las instrucciones para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa sus enlaces a otros archivos de instrucciones y a ejemplos del proyecto. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares del proyecto, pero todavía no exige documentación en el código. A continuación añadirás esa regla y las cabeceras de comentarios.

## Crea una rama para los cambios

1. Vuelve al Codespace.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Utiliza Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; las instrucciones de cada paso indican lo necesario.

Primero enviarás un prompt con las instrucciones actuales. Más adelante, lo repetirás después de actualizarlas para comparar los resultados.

1. Cierra las pestañas de editor anteriores para que Copilot reciba solo el contexto necesario.
2. Abre `src/lib/publishers.ts` para indicar dónde debe ir la función auxiliar.
3. En el selector de agentes de Chat, selecciona **Agent** para que Copilot pueda modificar archivos.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios, normalmente en el archivo auxiliar y en sus pruebas.
6. Observa que la función auxiliar propuesta recibe un cliente `db` como primer argumento y devuelve una matriz de editores tipada. Esto se debe a las convenciones de la capa de datos en `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni una cabecera de comentario para el archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios de documentación aunque no se lo pidas. No pasa nada: lo importante es observar si las instrucciones nuevas mejoran la coherencia.

## Añade un estándar para todo el repositorio

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, cerca de la línea 27. Todavía no contiene una regla para documentar el código.
3. Añade estas líneas debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda el archivo.

> [!TIP]
> Las instrucciones generales van en `.github/copilot-instructions.md`; las reglas específicas de lenguajes, tipos de archivo o tareas se guardan en archivos `*.instructions.md`.

## Repite el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia.
2. Selecciona `src/lib/publishers.ts` para centrar el contexto.
3. Envía el mismo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un comentario parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has indicado a Copilot un nuevo estándar y lo has aplicado a código que se utilizará en el siguiente ejercicio.

## Confirma las instrucciones y publica la rama

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba los cambios:

   ```bash
   git status
   ```

3. Añade las instrucciones y el código auxiliar, y crea un commit:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados, como pruebas, añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Con la rama publicada, crea una PR asociada a la incidencia del backlog sobre la actualización de las instrucciones.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot realiza acciones en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub al espacio de trabajo, así que no tienes que configurar nada. La primera vez que Copilot utilice una herramienta, VS Code puede pedirte que inicies sesión en GitHub. Sigue las instrucciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante el protocolo MCP.

1. Abre Copilot Chat en el Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que está seleccionado **Agent**.
3. Pide a Copilot que encuentre la incidencia y cree una PR desde la rama actual:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot empezará a buscar la incidencia y a crear la PR.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub en tu nombre, revisa la operación y selecciona **Allow** si corresponde.
6. Cuando se haya creado la PR, pídele que la combine en `main` y que vuelva a la rama local `main` para descargar los últimos cambios:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub y ejecutar comandos del terminal en tu nombre, revisa la operación y selecciona **Allow** si corresponde.

¡Has creado y combinado una solicitud de incorporación de cambios con GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones y has utilizado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones existentes.
- añadir un estándar general a `.github/copilot-instructions.md`.
- repetir el prompt y observar cómo el código adopta el estándar nuevo.
- confirmar las instrucciones y la función auxiliar en `main` para el siguiente ejercicio.

A continuación, utilizarás estas instrucciones en [modo agente][next-lesson] para añadir una funcionalidad en varias partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas de Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

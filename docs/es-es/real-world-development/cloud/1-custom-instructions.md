---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de Copilot en la nube →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita información adicional, conviene proporcionarle ese contexto. Los [archivos de instrucciones][instruction-files] ofrecen directrices para que Copilot entienda no solo *qué* quieres, sino también *cómo* quieres que lo haga.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de código y los estándares de documentación mediante instrucciones del repositorio y archivos específicos de rutas.
- añadirás un nuevo estándar general al archivo `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no compararás prompts antes y después. El agente de Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás la influencia de las instrucciones más adelante, al revisar las solicitudes de incorporación de cambios que genere.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe utilizar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que explique su función.

Utilizarás archivos de instrucciones para proporcionar a Copilot la información necesaria para trabajar de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot. Puedes especificar convenciones, bibliotecas y comentarios para orientar las sugerencias de código. Las instrucciones pueden aplicarse a todo el repositorio o a determinados tipos de archivos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto y directrices relevantes para la mayoría de las solicitudes.
- `.github/instructions/*.instructions.md`, archivos que definen directrices para determinados lenguajes, tipos de archivos o tareas.

> [!NOTE]
> En el IDE, los archivos de instrucciones se utilizan para generar código en Copilot Chat, pero no para completar código ni para sugerir la siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo`.
>
> Copilot también admite otros formatos de instrucciones, como AGENTS.md y CLAUDE.md; consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support].

### Prácticas recomendadas para gestionar archivos de instrucciones

El proyecto de ejemplo muestra un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en directrices generales, como la descripción y estructura del proyecto y los estándares de código.
- Utiliza `*.instructions.md` para los tipos de archivo (pruebas unitarias, componentes de Astro, capa de datos) o tareas concretas.
- Escribe en lenguaje natural y presenta las directrices con claridad, incluidos ejemplos de lo que se debe y no se debe hacer.

No hay un único modo correcto de crear archivos de instrucciones. Descubre con la práctica qué funciona mejor en tu proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener una colección sólida de archivos de instrucciones. En este proyecto encontrarás archivos para tareas como [actualizar la interfaz][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada entorno ofrece esta opción de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI.
>
> Para encontrar plantillas y ejemplos, explora [awesome-copilot][awesome-copilot], que reúne archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Este repositorio incluye un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para tareas distintas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Examina la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las directrices anidadas de **GitHub Actions Workflows**.
3. Explora `.github/instructions`. Hay archivos para Astro, la capa de datos Drizzle, pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo` del frontmatter, que establece un patrón relativo a la raíz del repositorio. Por ejemplo, coincide con archivos de prueba TypeScript como `**/*.test.ts`.
5. Examina las instrucciones para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa sus enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares del proyecto, pero todavía no exige documentación en el código. En los pasos siguientes añadirás esa regla y los comentarios de cabecera para los archivos.

## Añade un estándar para todo el repositorio

Añadirás una regla general para que el agente documente el código con comentarios TSDoc y una cabecera de archivo. El agente en la nube la tendrá en cuenta cuando trabaje en incidencias que le asignes más adelante.

Antes de editar, crea una rama. Aunque el agente leerá las instrucciones de la rama a la que se dirija la incidencia, confirmarás los cambios de forma ordenada:

1. En el terminal del Codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`copilot-instructions.md` proporciona información general del proyecto. Añadirás los estándares que faltan:

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, cerca de la línea 27. Todavía no incluye reglas para documentar el código.
3. Añade estas líneas de Markdown debajo de los estándares actuales:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Puedes guardar directrices generales en `.github/copilot-instructions.md` y reglas específicas de lenguajes, tipos de archivo o tareas en `*.instructions.md`. El archivo del repositorio es el lugar adecuado para estándares generales, como los comentarios de documentación que acabas de añadir.

## Confirma, publica y combina las instrucciones

El agente en la nube lee las instrucciones de la rama a la que se dirige la incidencia. En el siguiente ejercicio, partirá de `main`; por eso, los cambios deben llegar a `main`.

1. Prepara los cambios y crea un commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. Así el agente podrá leer estas instrucciones cuando le asignes una incidencia en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main`, puedes omitir los pasos de la rama y confirmar los cambios directamente en `main`. Usar una rama permite practicar un flujo de trabajo habitual.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica en las solicitudes de incorporación de cambios que genere el agente en la nube.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas de Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

---
title: "Ejercicio 2 - Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. El flujo de trabajo de un desarrollador suele consistir en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta y volver a actualizarlo. La lista continúa.

Aquí es donde entra en juego el modo agente de Copilot.

El modo agente de Copilot está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Veamos cómo utilizarlo para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite interacciones rápidas; el agente en la nube trabaja de forma asíncrona con incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y realizar cambios coordinados.
- GitHub Copilot Agent Mode puede implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas que genera Copilot Agent Mode antes de combinar los cambios en el código base.

## Escenario

El catálogo de juegos crece y quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior añadiste una función auxiliar para consultar editores; ahora completarás la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web y su funcionalidad actual. Tailspin Toys es una plataforma de financiación colectiva de juegos de mesa con temática de programación. El sitio es una única aplicación Astro que genera páginas HTML estáticas. Las páginas consultan una base de datos SQLite local mediante Drizzle ORM; no hay una API independiente ni un framework de interfaz en el cliente. Las funciones auxiliares reutilizables están en `src/lib/` y la interactividad se implementa con scripts `<script>` de Astro y las API DOM estándar.

### Inicia el sitio web

La tarea `npm run dev` inicia el servidor de desarrollo de Astro. Para ejecutarlo en GitHub Codespaces:

1. Vuelve al Codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio con el comando siguiente:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, verás un mensaje similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost se redirige a una dirección `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado al servidor del Codespace.

### Explora el sitio web

La página principal muestra una lista de juegos con títulos, imágenes y descripciones. Al seleccionar un juego, se abre su página de detalles, que contiene más información, como el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. Las instrucciones de cada paso indican el modo y el modelo que debes seleccionar.

La plantilla del proyecto ha creado un backlog de incidencias. Pide a Copilot que lo muestre:

1. En el selector de agentes de Chat, selecciona **Agent**, que puede planificar e implementar cambios en varios archivos, ejecutar comandos y utilizar herramientas.
2. Selecciona **Claude Sonnet 4.5** entre los modelos disponibles.

> [!CAUTION]
> Se indica ese modelo porque se utilizó al crear el taller, no porque exista una preferencia por él. Las respuestas pueden variar porque los modelos son probabilísticos.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede utilizar un comando MCP distinto, pero debería poder completar la tarea.

3. Envía este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que se enumeren las incidencias.
5. Revisa la lista y la priorización.

## Revisa los archivos de instrucciones

Antes de poner en marcha el agente para que genere código, revisa el archivo de instrucciones que le proporcionará contexto. Utilizarás el archivo de [instrucciones de interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo abordar la incorporación de funcionalidades al sitio.

1. Abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funcionalidades. Incluyen:
   - Una descripción general de la arquitectura.
   - Principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - Enlaces a instrucciones específicas para distintos tipos de archivo, entre ellas:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Esto permite reutilizar contenido, dividirlo en archivos más pequeños y añadir ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado tendrás que realizar al menos tres actualizaciones independientes:

- Añadir o perfeccionar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- Añadir o actualizar las pruebas del comportamiento de filtrado.
- Actualizar la página de listado de juegos para incorporar la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios en el código base. Copilot Agent Mode puede realizar estas tareas.

1. Continúa la conversación actual o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui**.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que está seleccionado **Agent**.
4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Envía este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad: debería encontrar tanto las funciones auxiliares de la capa de datos como la interfaz y las pruebas. Después, empezará a modificar los archivos y a ejecutar las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos, según la complejidad de la tarea y del código base. Mientras trabaja, pueden aparecer las opciones **Keep** y **Undo** en el editor. Cuando termine, tendrás una opción **Keep** o **Undo** para cada cambio; no es necesario seleccionarlas mientras siga trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo en el panel Copilot Chat que pide confirmar si se desea ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión completa de Copilot Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Es importante revisar siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a utilizar GitHub Copilot Agent Mode para añadir funcionalidades a Tailspin Toys. Has aprendido cómo:

- GitHub Copilot Agent Mode puede implementar funcionalidades en la interfaz y la capa de datos.
- Copilot Agent Mode puede explorar el proyecto, identificar los archivos pertinentes y realizar cambios coordinados.
- revisar los cambios y las pruebas generados por Copilot Agent Mode antes de combinar los cambios en el código base.

A continuación, [probarás la funcionalidad con Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

Cuando crezca la lista de juegos, será necesario habilitar la paginación. Con lo que has aprendido en este ejercicio, pide a Copilot que actualice el sitio. Ten en cuenta lo siguiente:

- Sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones.
- Decide cómo quieres implementar la paginación y si quieres permitir que el usuario elija el tamaño de página o prefieres establecerlo de forma fija.
- Al redactar el prompt, proporciona a Copilot las instrucciones necesarias para crear la implementación que necesitas.
- Puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Este es el flujo normal de trabajo con Copilot.

## Recursos

- [Copilot: modos ask, edit y agent][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode

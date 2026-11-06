---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muchas organizaciones tengan dificultades con la deuda técnica: incidencias de seguridad pendientes, código heredado que hay que actualizar o funcionalidades que siguen en el backlog por falta de tiempo. El agente de GitHub Copilot en la nube puede realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas y centrarte en asuntos más importantes.

Explorarás cómo utilizar el agente en la nube para:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- comprender la importancia de describir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica pendiente. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: de hecho, no hay documentación. El primer paso es añadir comentarios TSDoc a todas las funciones exportadas.

Además, el equipo de diseño quiere mejorar cómo se descubren los juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para poder probar la experiencia. No necesita una implementación refinada, pero esta tarea está bloqueada y otras incidencias tienen mayor prioridad.

Son ejemplos de tareas que pueden quedar relegadas y que puedes asignar al agente de Copilot en la nube. El agente trabaja de forma asíncrona mientras tú te ocupas de otras tareas; después podrás revisar su trabajo.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se la asignas, Copilot crea una PR en borrador para registrar el progreso, configura el entorno y empieza a trabajar. Puedes consultar la sesión mientras está en marcha o cuando termine. Cuando la solución esté lista para revisarse, Copilot te mencionará en la PR.

## La importancia de unas instrucciones bien delimitadas

GitHub Copilot no hace magia: no puedes saltarte el trabajo de reflexión. Incluso una tarea que parece sencilla puede tener aspectos complejos, así que [define cuidadosamente las tareas para el agente de Copilot en la nube][cloud-agent-best-practices]. Trabaja por etapas, aprende, experimenta y adapta el enfoque como harías con un compañero de programación. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has aprendido cómo las instrucciones personalizadas guían a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`; si no, aprovecha para echarle un vistazo. Antes de asignar trabajo al agente de Copilot en la nube, revisa los archivos de instrucciones del repositorio sin modificarlos:

- `.github/copilot-instructions.md`: revisa **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Code lacks documentation`, fíjate en los comentarios TSDoc, las convenciones TypeScript y las cabeceras de comentarios de la PR. Estos elementos proceden de los archivos de instrucciones y volverás a examinarlos en una lección posterior.

## Configura el entorno de desarrollo del agente de Copilot en la nube

La creación de código suele requerir un entorno específico y algunos pasos de configuración. Lo mismo sucede cuando se asignan tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente utiliza [GitHub Actions][github-actions] como entorno. Puedes personalizarlo mediante un [flujo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Este archivo viene preparado para instalar Node.js y las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot tendrá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de la configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en los pasos de configuración, Copilot lo hará automáticamente al finalizar.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación: Node.js (ahora toda la aplicación utiliza Astro y Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El desencadenador `workflow_dispatch` permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Así puedes probar que funciona correctamente en lugar de esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esa deuda técnica puede ralentizar el trabajo y dificultar el mantenimiento o la incorporación de nuevos desarrolladores. Copilot puede ayudar a crear documentación, así que asignarás esta tarea al agente en la nube.

1. Abre tu repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe esta descripción:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) que se acaba de crear. Tendrá un título similar a `[WIP]: Code lacks documentation`. Si todavía no aparece en la lista, espera un momento y actualiza la página.
12. Espera unos minutos a que Copilot cree una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer mientras Copilot prepara el entorno (ejecuta el flujo de trabajo descrito anteriormente), analiza el proyecto y determina la mejor forma de abordar el problema.

13. Revisa la lista y las tareas que Copilot va a completar.
14. Desplázate por la cronología de la PR hasta que veas una actualización que indique que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo para seguir cómo Copilot resuelve el problema: explora el código y comprende su estado, se detiene a pensar y decidir el plan adecuado, y crea código.

El proceso puede tardar varios minutos. Uno de los objetivos principales del agente en la nube es trabajar de forma asíncrona para que puedas ocuparte de otras tareas. Aprovecha esta función asignándole otra tarea y, después, céntrate en escribir código para añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente en la nube es que permite dividir el trabajo. Mientras Copilot añade una sección de juegos relacionados para el equipo de diseño, puedes ocuparte de otras tareas.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. Escribe esta descripción:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después verás el emoji 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot está trabajando en tu segunda solicitud. Como el agente en la nube trabaja de forma similar a un ingeniero de software, no necesitas supervisarlo activamente: podrás revisar su trabajo cuando termine. Ahora puedes centrarte en aprender a crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

Has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona y te ayude con deuda técnica, nuevas funcionalidades o migraciones entre frameworks.

En este ejercicio:

- personalizaste el entorno para generar código.
- aprendiste a tener en cuenta la seguridad de las operaciones.
- exploraste la importancia de delimitar las tareas.
- asignaste incidencias a Copilot.

Mientras el agente trabaja, puedes pasar a crear y utilizar agentes personalizados. El [agente de Copilot en la nube también puede utilizar servidores MCP][cloud-agent-mcp], y cuenta con las instrucciones personalizadas que exploraste en ejercicios anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions

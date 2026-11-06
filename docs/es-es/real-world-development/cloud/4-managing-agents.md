---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores pediste al agente de Copilot en la nube que realizara tres tareas distintas para mejorar la experiencia y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Puedes gestionar las tareas asignadas al agente desde la [página de agentes][agents-page] de GitHub.com. Allí verás las tareas con solicitudes de incorporación de cambios abiertas, las operaciones realizadas y las sesiones en curso, a las que puedes orientar.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- guiarás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar el modo de alto contraste, el equipo decidió que también sería conveniente ofrecer un modo claro. Como ya se estaba trabajando en los estilos y en una opción de alternancia, pedirás al agente que añada también esa funcionalidad.

## Revisa las tareas del agente de Copilot en la nube

Consulta el estado de las tareas asignadas al agente:

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa las tareas que aparecen en los paneles principal e izquierdo. Deberías ver las tareas que has asignado, entre ellas:
   - añadir documentación al código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa las operaciones realizadas por Copilot, que pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo.
   - configurar servidores MCP.
   - realizar pasos para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque utilizado.

4. Revisa también el panel de solicitudes de incorporación de cambios de la derecha, donde puedes consultar la PR y los archivos modificados.

## Guía al agente en la nube

Pedirás a Copilot que añada un modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título puede variar según el nombre que haya elegido Copilot y el estado del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos, hasta que la sesión indique que Copilot ha terminado la configuración y ha empezado a trabajar.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes con el cuadro para guiar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo incorpora al trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva solicitud al flujo cuando termine el paso que estaba realizando. Como antes, puede tardar varios minutos. Mientras tanto, puedes repasar lo aprendido.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, un centro para supervisar y guiar las tareas del agente en la nube. Puedes seguir tareas activas y completadas, revisar el trabajo realizado y orientar sesiones en curso.

En esta lección:

- exploraste el centro de control y la página de agentes para supervisar las tareas.
- redirigiste una sesión en curso para pedir una funcionalidad adicional.

A continuación, [iterarás sobre las solicitudes de incorporación de cambios creadas por Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents

---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para determinadas tareas o áreas del flujo de desarrollo. Al definir agentes en archivos Markdown en `.github/agents`, puedes proporcionar instrucciones, prácticas recomendadas, patrones de código y conocimientos específicos del dominio. Los equipos pueden convertir sus conocimientos en agentes reutilizables: uno de accesibilidad que aplique las directrices de [WCAG][wcag], uno de seguridad que siga prácticas de programación seguras o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown en `.github/agents` o globalmente en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML que contiene, como mínimo, `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se parecen en algunos aspectos: ambos se definen principalmente mediante archivos Markdown e indican a la IA cómo realizar tareas. Una forma sencilla de diferenciarlos es que el **agente personalizado** es quien realiza el trabajo y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este taller, el agente de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad; para ello, podría utilizar habilidades de flujo de trabajo de PR o de ejecución de pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba distintas opciones y adáptalas a tu entorno y tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio aprenderás:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades visuales o preferencias. Algunos usuarios han indicado que el tema oscuro actual es difícil de leer porque hay poco contraste entre el texto y el fondo. El equipo de diseño solicita un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, así que utilizarás un agente personalizado para implementar esta funcionalidad rápidamente.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para accesibilidad. Revisa su contenido para entender cómo guiará a Copilot.

Vuelve al Codespace y abre el archivo del agente personalizado de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de HTML, CSS y JavaScript.
   - errores habituales.

## Crea y asigna una incidencia

La página de agentes es el centro de control del entorno. Desde allí puedes asignar tareas al agente de Copilot en la nube, supervisar su trabajo y orientarlo. Asignarás una tarea para implementar el modo de alto contraste:

1. Abre tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Add high contrast mode to website` en el campo **Title**.
6. Escribe esta descripción:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Captura del cuadro de asignación del agente en la nube, con el agente personalizado y Accessibility resaltados](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados en tareas y ámbitos concretos. Permiten codificar la experiencia y los estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

En este ejercicio:

- aprendiste cómo se definen los agentes personalizados.
- asignaste una tarea a un agente personalizado.

Mientras Copilot implementa el modo de alto contraste, aprenderás a [supervisar y guiar la sesión del agente][next-lesson] desde el centro de control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar la organización para utilizar agentes personalizados][org-custom-agents]
- [Preparar la empresa para utilizar agentes personalizados][enterprise-custom-agents]

---

| [← Lección anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents

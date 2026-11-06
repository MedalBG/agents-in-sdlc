---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia y añadir funcionalidades. Le pediste que documentara el código, implementara la sección de juegos relacionados y añadiera funciones de accesibilidad, como los modos de alto contraste y claro. Revisarás los cambios y, si es necesario, pedirás a Copilot que los mejore.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian con la IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual. Revisarás las propuestas de Copilot para documentar el código, añadir juegos relacionados e implementar las funciones de accesibilidad antes de solicitar la revisión del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de Copilot en la nube trabaja de forma asíncrona y sin supervisión, se aplican algunas restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para su trabajo.
- El agente se ejecuta en GitHub Actions, en un entorno efímero independiente.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empezarás por la primera solicitud de incorporación de cambios (PR) de Copilot, que añade documentación al código. La revisarás desde la interfaz estándar de GitHub.com.

> [!NOTE]
> Es posible que la PR muestre un aviso de que Copilot está bloqueado por un firewall. Es lo esperado: por defecto, Copilot tiene acceso limitado a recursos externos, incluidos los servidores MCP. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests**.
3. Abre la solicitud con un título parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Examina el código actualizado, que incluye los nuevos comentarios TSDoc. Los cambios concretos pueden variar. Comprueba también las convenciones de TypeScript y las cabeceras de comentarios indicadas en los archivos de instrucciones que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo se ponen en cola en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. Esto puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot no es un proceso unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios en línea del código, como harías con otras personas del equipo. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como los resultados no son deterministas, no hay una petición concreta que debas utilizar. Por ejemplo, puedes pedirle que:

- añada una cabecera breve al principio de cada archivo de código.
- añada comentarios TSDoc a archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario y menciona a **@copilot** para pedir un cambio en la documentación generada. Puedes elegir una de las ideas anteriores.
2. Selecciona **View Session** para observar cómo Copilot inicia una nueva sesión y realiza los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, verás un commit nuevo en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, marca la PR como lista para revisión y combínala.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Ahora revisarás la PR creada por Copilot para mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas terminado, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Comprueba que las comprobaciones del proyecto para la aplicación Astro se superen; puede tardar unos minutos.
9. **Opcional:** cambia a la rama en tu Codespace para probar manualmente la funcionalidad. Sustituye `<branch-name>` por la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la sección de juegos relacionados. Puedes pedirle cambios con un comentario en la pestaña **Conversation**, como ajustar el número de juegos o añadir cabeceras y comentarios TSDoc. Recuerda que esta tarea se asignó antes de actualizar las instrucciones personalizadas.

## Revisa las funciones de accesibilidad

Por último, revisarás las funciones de accesibilidad implementadas con el agente personalizado. La PR debería incluir el modo de alto contraste que pediste en el ejercicio 3 y el modo claro que solicitaste desde el centro de control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot todavía está trabajando, la PR tendrá la etiqueta **[WIP]**. Espera unos minutos a que termine.

4. Selecciona **Files changed** y revisa el código generado.
5. Examina la implementación y presta atención a:
   - los componentes de interfaz para alternar los modos.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad (etiquetas ARIA, navegación por teclado, etc.).
   - el código JavaScript/TypeScript que gestiona los modos.

6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que las comprobaciones del proyecto para la aplicación Astro se superen.
10. **Opcional:** cambia a la rama en Codespaces para probar manualmente las funciones de accesibilidad:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Confirma que funcionan y conservan la selección al recargar la página.

El agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones siguiendo prácticas de accesibilidad. Si encuentras algún problema o mejora, puedes mencionarlo en un comentario con `@copilot`.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil cuando puedes asignarle trabajo real del backlog y ocuparte de otra cosa. Para practicarlo, crea algunas incidencias más y asígnaselas a Copilot. Por ejemplo:

- crear un formulario de interés para patrocinadores en la página de detalles de un juego.
- implementar la paginación en la página de la lista de juegos.
- añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Inspeccionaste los archivos de instrucciones personalizados del repositorio** para reconocer su influencia en el trabajo del agente.
- **Asignaste incidencias al agente de Copilot en la nube** y observaste cómo preparaba el entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y utilizaste un agente personalizado** para implementar modos de alto contraste y claro.
- **Utilizaste la página de agentes de Copilot como centro de control** para supervisar y guiar la sesión de accesibilidad.
- **Revisaste e iteraste sobre las solicitudes de incorporación de cambios**, mencionando a `@copilot` para pedir cambios y aprobando los flujos de trabajo.

## Revisión y siguientes pasos

Has completado el itinerario del agente en la nube. Si quieres seguir aprendiendo, los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración MCP desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con Plan y Autopilot, habilidades de agente, un agente QA personalizado, Playwright MCP, Agent Merge y una integración opcional con `/delegate`.

También puedes probar estas ideas en tu repositorio:

- asignar al agente una incidencia de refactorización o cobertura de pruebas.
- crear un agente personalizado para otro ámbito.
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/

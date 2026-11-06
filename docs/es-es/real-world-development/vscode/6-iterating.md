---
title: "Ejercicio 6 - Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
| :-- |

## Revisar el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario. Utilizaste el modo agente para añadir filtros en el cliente y el servidor, Playwright MCP para probarlos manualmente en un navegador y un agente personalizado para implementar los modos de alto contraste y claro. También guiaste la sesión para ampliar el trabajo. Ahora publicarás esos cambios locales y los revisarás como lo haría tu equipo.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian con la IA generativa. Hay que revisar el trabajo de Copilot como cualquier otro cambio. En esta lección enviarás los cambios de accesibilidad desde el Codespace, abrirás una solicitud de incorporación de cambios y revisarás sus diferencias antes de pedir la revisión del equipo.

## Publica las funciones de accesibilidad

Los modos de alto contraste y claro que implementaste con el agente de accesibilidad en los [ejercicios 4][exercise-4] y [5][exercise-5] están confirmados mediante commits en el Codespace. Enviarás la rama y abrirás una PR:

1. Vuelve al Codespace.
2. Abre la vista **Source Control** en VS Code.
3. Confirma que los cambios de accesibilidad están confirmados mediante commits. Si quedan cambios sin confirmar del ejercicio 5, prepáralos y crea un commit descriptivo, como `Add high-contrast and light-mode toggles`.
4. Publica la rama con **Publish Branch** o con **...** → **Push**.
5. Si VS Code te ofrece abrir la rama en github.com, acepta. También puedes abrir el repositorio y seleccionar **Compare & pull request** en el aviso de la rama.
6. Escribe un título claro, como `Add high-contrast and light-mode toggles`, y una descripción breve.
7. Selecciona **Create pull request**.
8. Cuando se abra la PR, selecciona **Files changed** y revisa todos los cambios. Presta atención a:
   - los controles de la interfaz para cambiar de modo.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de estilo para los modos de alto contraste y claro.
   - los atributos de accesibilidad (etiquetas ARIA, navegación por teclado, etc.).
   - el código JavaScript/TypeScript que gestiona los modos.
9. Vuelve a la pestaña **Conversation**.
10. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

11. Espera a que terminen los flujos de trabajo y comprueba que se superen.

> [!TIP]
> Para pedir otra opinión sobre la accesibilidad, menciona a `@copilot` en un comentario de la PR; por ejemplo, puedes pedirle que revise otros problemas de WCAG o sugiera mejoras en la navegación por teclado.

## Ejercicio opcional: sigue explorando localmente

Trabajar de forma iterativa con un agente en el IDE es una habilidad que se desarrolla con la práctica. Puedes probar estas tareas en VS Code:

- añadir un formulario de interés para patrocinadores en la página de detalles del juego.
- implementar la paginación en la página de la lista de juegos.
- añadir validación de entradas y gestión de errores a las funciones de acceso a datos de `src/lib/`.
- ampliar el alcance del agente de accesibilidad, por ejemplo, para revisar el orden del foco del teclado en todo el sitio.

## Resumen

¡Enhorabuena! Has completado el itinerario de VS Code. En este taller:

- **Utilizaste Playwright MCP para probar manualmente la funcionalidad.** Añadiste el servidor y dejaste que Copilot manejara un navegador para verificar el filtrado antes de abrir una PR.
- **Utilizaste el modo agente para coordinar cambios en varias capas.** Añadiste una funcionalidad que afectó al cliente, al servidor y a las pruebas en una sesión.
- **Utilizaste un agente personalizado.** Seleccionaste el agente de accesibilidad y observaste cómo implementaba el modo de alto contraste.
- **Gestionaste y guiaste una sesión de agente.** Revisaste los cambios propuestos y ampliaste el trabajo con una petición para añadir el modo claro.
- **Cerraste el ciclo con una solicitud de incorporación de cambios.** Publicaste el trabajo local y lo revisaste de principio a fin.

## Revisión y siguientes pasos

Has completado el itinerario obligatorio de VS Code. Puedes detenerte aquí.

Para explorar otras experiencias de agentes, consulta:

- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con Plan y Autopilot, habilidades, un agente QA, Playwright MCP, Agent Merge y transferencias con contexto.
- ☁️ **[Itinerario del agente en la nube](../../cloud/)**: asigna incidencias al agente en la nube, supervisa sesiones desde la página de agentes e itera de forma asíncrona sobre las solicitudes de incorporación de cambios.

También puedes utilizar los archivos de instrucciones, agentes personalizados y habilidades de [awesome-copilot][awesome-copilot] como punto de partida para tus proyectos.

Como ampliación opcional, [Opcional: Incorporar Foundry][exercise-7] utiliza VS Code y Microsoft Foundry Toolkit para preparar un modelo, desplegar Backer Concierge y conectarlo al sitio.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot

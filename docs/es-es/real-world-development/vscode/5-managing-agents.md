---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

En modo agente, GitHub Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo y en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición. Puedes revisar las diferencias, aceptar o rechazar cambios y orientar la conversación con prompts adicionales.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos.
- guiarás al agente durante la sesión para que añada un modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño pide además un modo claro para las personas a quienes les resulta demasiado oscuro el tema predeterminado. Guiarás la sesión actual para ampliar el trabajo en lugar de empezar una conversación nueva.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: archivos que explora, comandos que ejecuta y cambios que propone. Cada edición aparece en línea con una diferencia y los controles **Keep** y **Undo**.

1. Vuelve a Copilot Chat desde el ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas, las lecturas y ediciones de archivos y los comandos del terminal.
3. Cuando proponga una edición:
   - selecciona **Keep** para aceptar el cambio.
   - selecciona **Undo** para revertirlo y pedir al agente que lo intente de nuevo.
4. Abre el indicador **Files changed** situado en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta necesita permiso (por ejemplo, para ejecutar un comando del terminal), el agente se detendrá y mostrará **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que termine el modo de alto contraste. El agente mostrará un resumen de los cambios.

## Guía al agente para añadir el modo claro

Ahora ampliarás la misma conversación para añadir un control del modo claro. El agente conservará el contexto del trabajo anterior, incluidos los componentes modificados, los estilos y el uso del almacenamiento local.

1. En el campo de Copilot Chat, envía este prompt:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente revisará los cambios y propondrá nuevas modificaciones para el modo claro.
3. Revisa cada cambio a medida que aparezca. Utiliza **Keep** o **Undo**.
4. Si el agente toma una dirección que no quieres —por ejemplo, reorganiza los controles—, detenlo con **Stop** y envía una aclaración.

> [!TIP]
> Guiar una sesión activa es más rápido que empezar de nuevo. Puedes pedir ajustes («simplify the CSS — use CSS variables instead of duplicating selectors»), corregir el enfoque («don't add a third toggle, integrate light mode into the existing settings panel») o ampliar el alcance («also add keyboard shortcuts for both toggles»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa todos los archivos modificados:

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista de archivos; debería incluir componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se aplican las prácticas del agente de accesibilidad: atributos ARIA, navegación por teclado, HTML semántico y persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios en el panel Source Control. Los publicarás en la [siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has supervisado y guiado una sesión activa de Copilot Chat: revisaste las ediciones, aceptaste o rechazaste cambios y enviaste prompts de seguimiento para ampliar el trabajo sin perder contexto.

En este ejercicio:

- supervisaste el trabajo del agente en tiempo real.
- aceptaste y rechazaste ediciones individuales.
- guiaste una sesión activa con prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás los cambios de accesibilidad como una solicitud de incorporación de cambios y revisarás el trabajo del agente personalizado.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode

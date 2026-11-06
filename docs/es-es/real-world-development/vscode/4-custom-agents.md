---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para determinadas tareas o áreas del flujo de desarrollo. Al definir agentes en archivos Markdown en `.github/agents`, puedes proporcionar instrucciones, prácticas recomendadas, patrones de código y conocimientos específicos. Los equipos pueden convertir sus conocimientos en agentes reutilizables: uno de accesibilidad que aplique las directrices de [WCAG][wcag], uno de seguridad que siga prácticas seguras o uno de pruebas que mantenga patrones coherentes.

Los agentes se definen en archivos Markdown en `.github/agents` o globalmente en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML que contiene `name` y `description`, seguido de un prompt Markdown que define el comportamiento e instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se parecen: ambos se definen mediante archivos Markdown e indican a la IA cómo realizar tareas. Una forma sencilla de diferenciarlos es que el **agente personalizado** es quien realiza el trabajo y las **habilidades** son las herramientas.

Los agentes tienen su propia ventana de contexto y pueden coordinar habilidades y otros agentes. En este taller, el agente de accesibilidad revisará el sitio según las directrices de accesibilidad y podría utilizar habilidades de flujo de trabajo de PR o de ejecución de pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Prueba e itera para encontrar lo que funciona en tu entorno.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas. Algunos usuarios indican que el tema oscuro es difícil de leer por el bajo contraste entre texto y fondo. El equipo de diseño solicita un modo de alto contraste que pueda activarse y desactivarse.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- utilizarás el agente de accesibilidad en Copilot Chat para implementar el modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para accesibilidad:

1. Vuelve al Codespace, abre un terminal y crea una rama nueva a partir de `main` para mantener separada la PR de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abre `.github/agents/accessibility.md`.
3. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

4. Revisa las secciones siguientes, que describen las responsabilidades, las prácticas recomendadas y ejemplos de accesibilidad, así como los errores habituales.

## Utiliza el agente personalizado en Copilot Chat

VS Code muestra los agentes personalizados de `.github/agents` en el selector situado en la parte inferior de Copilot Chat. Puedes seleccionar uno para que la conversación utilice sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. Las instrucciones de cada paso indican el modo y el modelo que debes seleccionar.

1. Si no está seleccionado, elige **Agent** en el selector de agentes.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Abre el selector de agentes, que muestra el agente activo (por defecto, **default**).
3. Selecciona **Accessibility agent**.
4. Envía este prompt:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta y una diferencia que podrás revisar.
6. El indicador **Files changed** se actualizará a medida que el agente modifique archivos.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot modificará archivos reales del repositorio, como componentes Astro, estilos y pruebas relacionadas.

Revisarás y guiarás el trabajo en curso en el siguiente ejercicio.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot y has utilizado el modo agente de Copilot Chat con un agente personalizado.

A continuación, [supervisarás y guiarás el trabajo del agente][next-lesson], revisarás los cambios a medida que se realicen y pedirás que añada un modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados de awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents

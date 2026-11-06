---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear el filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. En vez de probarlo manualmente, conectarás el **servidor Playwright MCP** y dejarás que Copilot controle un navegador real. Después publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo amplía las herramientas de Copilot.
- añadirás el servidor Playwright MCP al espacio de trabajo.
- pedirás a Copilot que lo utilice para probar el filtrado en un navegador.
- publicarás la rama y abrirás una solicitud de incorporación de cambios.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente resulta más potente cuando puede acceder a herramientas externas. Model Context Protocol (MCP) es un estándar que permite a Copilot comunicarse con esas herramientas y servicios.

![Diagrama del funcionamiento interno del modo agente y su interacción con el contexto, el LLM y las herramientas, incluidas las proporcionadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real para obtener información actualizada y realizar acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos mediante un servidor MCP, que actúa como puente hacia las API existentes o las herramientas locales, como los paquetes de NPM. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos son:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona API para gestionar repositorios de GitHub, crear repositorios y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona automatización del navegador, como navegar por páginas web, rellenar formularios y seleccionar botones.

GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y contribuir al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia. Antes de utilizar uno, revisa su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad. Utiliza solo servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Verás que ya está configurado un servidor `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla incluye esta entrada y por eso Copilot pudo leer el backlog. Utiliza el [servidor GitHub MCP remoto][remote-github-mcp-server], así que no hay que instalarlo localmente. VS Code se conecta por HTTP y te autentica con GitHub la primera vez que Copilot utiliza una herramienta.

## Añade el servidor Playwright MCP

Añadirás otro servidor. [Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador controlable para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

`playwright` es un servidor **local**: VS Code lo inicia ejecutando `npx @playwright/mcp@latest`. La opción `--headless` ejecuta el navegador sin ventana, algo necesario en un Codespace sin escritorio.

> [!NOTE]
> Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, así que normalmente el navegador ya está instalado. Si Copilot indica que falta, pídele que ejecute `npx playwright install chromium`.

## Inicia y autoriza el servidor

VS Code inicia los servidores MCP **cuando se necesitan**. La primera vez que Copilot necesita las herramientas, VS Code inicia el servidor y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, selecciona la acción **Start** que aparece sobre la entrada `playwright`.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y autorízalo.

Este paso es opcional; si lo omites, VS Code iniciará el servidor y te pedirá autorización la próxima vez que Copilot necesite sus herramientas.

## Prueba la funcionalidad de filtrado

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe el filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

El servidor Playwright MCP abrirá un navegador, realizará los pasos y te devolverá un informe. Compáralo con los criterios de aceptación de la incidencia. Si hay algún problema, pide más detalles o solicita que Copilot corrija el código antes de crear la PR.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321`. Si detuviste el servidor de desarrollo, vuelve a iniciarlo. La primera vez que Copilot utilice Playwright MCP puede descargar un navegador; si falta, pídele que ejecute `npx playwright install chromium`.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Ya has comprobado que la funcionalidad funciona. Ahora publicarás la rama `filtering-vscode`.

1. Abre el panel **Source Control** de Codespaces y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de commit con el botón **Sparkle**.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Copilot puede localizar la incidencia correspondiente y crear una PR vinculada a ella.

1. En Copilot Chat, selecciona **New Chat**.
2. Confirma que está seleccionado **Agent** para que Copilot pueda utilizar las herramientas de GitHub.
3. Pídele que cree la PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir las operaciones necesarias. La primera vez que Copilot utilice una herramienta de GitHub, puede pedirte que te autentiques.
5. Observa cómo Copilot busca la incidencia y crea la PR.
6. Selecciona el enlace para revisar la PR, pero **no la combines todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- aprendiste qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- añadiste Playwright MCP al espacio de trabajo.
- lo utilizaste para probar manualmente la funcionalidad en un navegador antes de publicarla.
- publicaste la rama y abriste una solicitud de incorporación de cambios.

Has utilizado MCP para probar una funcionalidad, pero es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para facilitar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Microsoft Playwright MCP][playwright-mcp-server]
- [Servidor GitHub MCP][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration

---
title: "Visual Studio Code"
slug: es-es/real-world-development/vscode
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** en VS Code incorpora GitHub Copilot al editor que ya utilizas. En Visual Studio Code y GitHub Codespaces, dirigirás Copilot Chat en modo agente, conectarás herramientas externas mediante MCP y utilizarás agentes personalizados, todo ello sin salir del IDE, donde Copilot puede acceder a tus archivos, al terminal y a los problemas.

Empezarás por añadir instrucciones personalizadas y observar cómo las sigue Copilot. Después, utilizarás el modo agente para desarrollar una funcionalidad de filtrado en la interfaz, la capa de datos y las pruebas. A continuación, conectarás el servidor Playwright MCP y dejarás que Copilot maneje un navegador para probar el filtrado antes de abrir una solicitud de incorporación de cambios. Por último, utilizarás un agente personalizado de accesibilidad y supervisarás, guiarás e iterarás sobre los cambios de Copilot sin salir del editor.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea tu repositorio y Codespace |
| [1. Instrucciones personalizadas][ex1] | Contexto | Añade y verifica instrucciones personalizadas en VS Code |
| [2. Modo agente][ex2] | Generación de código | Desarrolla una funcionalidad de filtrado con el modo agente |
| [3. Pruebas con Playwright MCP][ex3] | Herramientas externas | Prueba la funcionalidad en un navegador con Playwright MCP |
| [4. Agentes personalizados][ex4] | Agentes especializados | Revisa y utiliza agentes personalizados |
| [5. Supervisar y gestionar agentes][ex5] | Supervisión | Supervisa y guía las sesiones del agente |
| [6. Iterar][ex6] | Revisión | Revisa localmente el trabajo de Copilot y elige los siguientes pasos |
| [Opcional: Incorporar Foundry][foundry-toolkit] | Agentes de IA | Prepara un modelo, despliega un agente y conéctalo al sitio en tres módulos con VS Code y Foundry Toolkit |

## Requisitos previos

Antes de asistir a este taller, asegúrate de disponer de:

- [ ] Una cuenta de GitHub con un plan activo de **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Acceso a GitHub Codespaces

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis mediante [GitHub Education][student-plan]. El plan **Copilot Student** incluye el agente, MCP, la revisión de código y Copilot CLI que se utilizan en este taller, por lo que permite completar todos los itinerarios.

## Empezar

**[Empieza por los requisitos previos][ex0]**

[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/

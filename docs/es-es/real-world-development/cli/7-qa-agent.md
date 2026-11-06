---
title: "Lección 7 - Crear y utilizar un agente QA"
description: "Crea un agente QA personalizado que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has utilizado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar el filtrado en un navegador. Ahora reunirás esas capacidades en un agente personalizado con un proceso de QA bien definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- crearás y revisarás un perfil de control de calidad reutilizable.
- seleccionarás el agente QA y revisarás sus conclusiones sobre la incidencia de filtrado.

## Escenario

Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador antes de abrir una solicitud de incorporación de cambios (PR). Un agente personalizado puede coordinar el proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe su propósito, instrucciones y herramientas disponibles. En este taller definirás el rol QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has utilizado tienen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` reúne comprobaciones repetibles. Playwright MCP proporciona herramientas para el navegador. El perfil QA indica a Copilot cómo utilizar esas capacidades para evaluar los requisitos e informar de los resultados. No sustituye a las demás ni requiere otra conversación.

## Crea el perfil QA

Antes de abrir la PR de la funcionalidad, pedirás a Copilot que cree un perfil QA reutilizable. El perfil definirá tanto las comprobaciones como sus límites.

1. Vuelve al Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecciona el perfil

Antes de utilizar el nuevo agente, revisa su perfil para confirmar que Copilot ha recogido el proceso y los límites previstos. Así evitarás que realice cambios demasiado amplios cuando solo quieres verificar la funcionalidad.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, aunque proporciona un nombre claro al agente.
3. Lee las instrucciones del perfil y confirma que QA empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y utiliza Playwright MCP.
4. Confirma que QA aporta pruebas que respaldan el informe, pide permiso antes de cambiar el código de implementación y no crea commits ni solicitudes de incorporación de cambios.
5. Si faltan responsabilidades o límites, pide al agente predeterminado que revise el perfil antes de continuar.

## Ejecuta QA sobre la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Retoma la misma conversación de filtrado después de crear el perfil y selecciona QA para que pueda utilizar el contexto de la incidencia y las decisiones de planificación.

1. Activa el agente con este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, es posible que no aparezca en la lista de agentes disponibles. El comando anterior lo activará igualmente.

2. Pide al agente QA que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. Deja que el agente QA trabaje.
4. Lee el informe cuando termine.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable al flujo de trabajo y has revisado su resultado. En esta lección:

- exploraste cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- creaste e inspeccionaste un perfil QA reutilizable.
- seleccionaste el agente QA y revisaste sus conclusiones sobre la incidencia de filtrado.

La implementación, la actualización de la habilidad, el perfil QA, las pruebas y el informe de verificación están listos para revisarse. A continuación, [los reunirás en una PR de funcionalidad y utilizarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration

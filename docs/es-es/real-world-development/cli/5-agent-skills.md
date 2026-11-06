---
title: "Lección 5 - Personalizar y utilizar una habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste solo en escribir código. Ya has comprobado manualmente que funciona y has utilizado archivos de instrucciones para garantizar que cumple los estándares. ¿Qué ocurre con las pruebas, lint y otras partes de la integración continua (CI)?

Las **habilidades de agente** son adecuadas para estas tareas. Ayudan a Copilot a entender cómo realizar operaciones como estas.

En esta lección:

- explorarás la habilidad `quality-checks` existente.
- personalizarás el formato de los resultados.
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys utiliza la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que sea más fácil de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente reúnen instrucciones reutilizables, scripts ejecutables y recursos complementarios que un agente carga cuando los necesita. Una habilidad se guarda en una carpeta con su nombre y un archivo Markdown `SKILL.md`. El archivo tiene un frontmatter con el nombre y la descripción de la habilidad, una explicación de su función e indicaciones sobre cuándo utilizarla. La carpeta también puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> Las carpetas y los archivos adicionales son opcionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las habilidades pueden estar en `.github/skills` para que formen parte del repositorio y las utilice el equipo, o en la carpeta global de habilidades del usuario, `~/.copilot/skills`.

## Explora la habilidad

Examinarás la habilidad `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve al Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo utilizar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y la ejecución de lint.
4. Comprueba que la habilidad todavía no incluye la sección **Results output formatting**.

## Ejecuta la habilidad antes de cambiarla

Puedes invocar las habilidades directamente con Copilot CLI o utilizar lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones:

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final.

## Personaliza el informe

Mejorarás el informe para que indique qué se ejecutó, si se completó correctamente y qué resultados devolvieron las herramientas.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade al final esta sección:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la habilidad actualizada

Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía el mismo prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y utilizado una habilidad de agente existente. En esta lección:

- exploraste `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- personalizaste el formato de los resultados.
- volviste a cargar la habilidad y la ejecutaste.

Este cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante Playwright MCP][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven de referencia; no tienes que completarlos:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación: `drawio`][drawio-example]
- [Pruebas de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing

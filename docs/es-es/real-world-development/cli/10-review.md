---
title: "Lección 10 - Repaso y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI en un flujo continuo de Tailspin Toys. En concreto:

- preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y localizaste la incidencia de filtrado.
- añadiste valoraciones por estrellas, revisaste el resultado en el navegador y combinaste manualmente tu primera solicitud de incorporación de cambios (PR).
- partiste de la incidencia de filtrado, definiste el enfoque en modo Plan, lo implementaste en modo Autopilot y lo revisaste en modo Interactive.
- guiaste al agente con instrucciones personalizadas, personalizaste la habilidad `quality-checks` y la utilizaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadiste el servidor Model Context Protocol (MCP) de Playwright y lo utilizaste para explorar el filtrado en un navegador real.
- creaste y seleccionaste un agente QA personalizado para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas del navegador.
- revisaste el cambio completo de filtrado y autorizaste Agent Merge para la PR.
- exploraste comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Lo que has entregado

El taller tiene dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizado:

1. **Valoraciones por estrellas:** muestra `starRating` y un estado explícito para los juegos sin valorar.
2. **Flujo de filtrado y calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, has utilizado la misma conversación y rama. Hemos reunido el trabajo en una sola PR para simplificar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias formas: pruebas automatizadas, inspección propia en el navegador y exploración del navegador por parte de Copilot mediante MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y registró los resultados con el nuevo formato. QA combinó esos resultados con una revisión de los requisitos y la cobertura antes de abrir la PR.

Las pruebas nuevas deben cubrir carencias reales; si QA no necesita añadir pruebas, el resultado también puede ser correcto. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller actualizaste instrucciones, personalizaste una habilidad, creaste un perfil QA y configuraste un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y adáptalas a las necesidades del equipo. Las instrucciones definen estándares, las habilidades describen tareas repetibles, los agentes personalizados establecen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Utiliza **Plan** para definir un enfoque antes de empezar, **Interactive** para participar en cambios concretos y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para ediciones rutinarias y uno más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más cosas que explorar

Has recorrido el flujo principal. También puedes probar algunas funciones más de CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y recibir otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola: para código de producción, proyectos personales o esa pequeña aplicación que llevas años pensando en crear. Comparte lo aprendido con el equipo y aprende de sus experiencias. Como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la [serie opcional Backer Concierge con Foundry][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests

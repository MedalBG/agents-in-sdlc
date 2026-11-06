---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad, el perfil QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas están guardados en una rama. Ahora los revisarás juntos y abrirás una solicitud de incorporación de cambios. Combinaste manualmente la PR de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las instrucciones, la actualización de la habilidad y el agente QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una conversación y una rama.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación.
- inspeccionarás el cambio completo de la funcionalidad y las pruebas de verificación.
- crearás la PR de filtrado.
- activarás Agent Merge solo después de revisar el cambio y confirmarás la combinación.

## Escenario

Durante el flujo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR sin quitar al desarrollador el control de la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Cuando lo activas, Copilot resuelve los bloqueos de la PR —corrige comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y actualiza la rama base cuando sea necesario— y, después, activa la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado manualmente **Merge pull request**. Agent Merge puede encargarse de ese proceso. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usa Agent Merge para gestionar la PR

Con el código terminado, revísalo, crea la PR y deja que Agent Merge gestione el resto.

1. Vuelve al Codespace.
2. Abre el diálogo de agentes escribiendo `/agent`.
3. Selecciona **Default** y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará las comprobaciones de integración continua de la PR y la combinará cuando se superen.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generar, probar y validar código, y gestionar una solicitud de incorporación de cambios. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de combinación.
- inspeccionaste el cambio completo de la funcionalidad y las pruebas de verificación.
- creaste la PR de filtrado.
- activaste Agent Merge solo después de revisar el cambio y confirmaste que se combinó.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference

---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Combinaste personalmente la PR de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las instrucciones, la actualización de la habilidad y el agente QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación.
- inspeccionarás la PR completa de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha combinado.

## Escenario

A lo largo del flujo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR y mantener bajo el control del desarrollador la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Cuando lo habilitas, la sesión de la aplicación lee la PR y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y reorganiza la base cuando es necesario. Después la combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, continúa después de reiniciar la aplicación y se desactiva cuando se combina la PR.

Hasta ahora has seleccionado manualmente **Merge pull request**. Agent Merge puede encargarse de ese proceso, pero la edición del código y la combinación requieren tu autorización explícita. Revisa las acciones que puede realizar y su trabajo antes de conceder el permiso para combinar.

## Utiliza Agent Merge para gestionar la PR

Con el código creado y revisado, deja que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable situado junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Se iniciará el proceso de Agent Merge:

- Creará la solicitud de incorporación de cambios con un título y una descripción.
- Si iniciaste la sesión desde una incidencia, la incluirá en el cuerpo de la descripción.
- Reorganizará la base o resolverá posibles conflictos con la rama de destino.
- Supervisará CI para confirmar que se superan todas las comprobaciones.
- Supervisará la PR para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot y realizará los cambios necesarios para resolverlos.
- Opcionalmente, combinará la PR cuando todo termine correctamente.

Ahora permitirás que Agent Merge combine la PR cuando se superen todas las comprobaciones:

5. Revisa la diferencia y las pruebas de verificación antes de conceder el permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Confirma que hay una marca junto a **Merge pull request**.
8. Pide explícitamente al agente que combine la PR:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Supervisa el progreso y resuelve los bloqueos que indique. Antes de continuar, confirma que el estado de la PR es **Merged**; habilitar el permiso o ver un agente trabajando no confirma que la combinación haya terminado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generación, pruebas y validación de código, y ahora también el proceso de la solicitud de incorporación de cambios. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de combinación.
- inspeccionaste la PR completa de la funcionalidad y las pruebas de verificación.
- autorizaste Agent Merge solo después de revisar el trabajo y confirmaste que la PR se había combinado.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app

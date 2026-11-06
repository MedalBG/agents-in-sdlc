---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como otras herramientas de CLI, GitHub Copilot CLI incluye numerosos comandos de barra. Estos comandos ofrecen funciones avanzadas, información interna y opciones de configuración adicionales. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Ahora explorarás otros comandos útiles.

En esta lección:

- utilizarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- utilizarás `/model` para explorar los modelos disponibles.
- aprenderás cómo `/share` puede exportar o compartir una sesión.
- explorarás comandos opcionales para trabajar en paralelo, usar worktrees y delegar tareas al agente en la nube.

## Escenario

Has terminado el flujo de trabajo principal de CLI. Ahora explorarás otras capacidades: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar tareas opcionalmente al [agente de Copilot en la nube][about-cloud-agent].

## Explora el contexto de Copilot CLI

Al trabajar en tareas extensas o complejas, puede que alcances la ventana de contexto máxima del modelo. Copilot CLI compacta la conversación automáticamente cuando es necesario. También puedes inspeccionarla o compactarla con comandos de barra.

1. Vuelve al Codespace e inicia Copilot CLI desde la raíz del repositorio si todavía no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y cómo se distribuye el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Escribe `/context` otra vez y compara el resultado. Si la conversación ya es pequeña, puede que el cambio no sea grande.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando la ventana se llena. Utiliza `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` cuando cambies a una tarea que no esté relacionada y quieras empezar una conversación nueva.

## Elige el modelo

Los modelos tienen distintas fortalezas y cada desarrollador puede tener sus preferencias. Copilot CLI permite consultar y seleccionar el modelo que quieras utilizar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Compartir conocimientos y trabajar en equipo ayuda a todo el mundo a aprovechar mejor las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartirla o publicar un GitHub gist.

1. Escribe `/help` y revisa las opciones de `/share` de la versión instalada.
2. Si quieres compartir la sesión, elige el destino adecuado; por ejemplo, `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de una conversación que el equipo no tenga previsto compartir.

## Opcional: ampliar o delegar el trabajo

Has completado el núcleo del taller. Copilot CLI también ofrece comandos para tareas mayores:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un worktree de Git aislado para una tarea distinta.
- `/delegate` puede enviar una tarea al agente de Copilot en la nube, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear worktrees adicionales o iniciar trabajo remoto. Antes de probarlos, define una tarea nueva y acotada, y revisa el resultado con tu proceso habitual. Para profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI permiten configurar la herramienta, compartir sesiones y consultar su funcionamiento interno. En esta lección:

- utilizaste `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- utilizaste `/model` para explorar los modelos disponibles.
- aprendiste cómo `/share` puede exportar o compartir una sesión.
- exploraste comandos opcionales para trabajar en paralelo, usar worktrees y delegar tareas al agente en la nube.

Hay muchos más comandos de barra que puedes explorar. Para terminar el recorrido, [repasarás lo aprendido][next-lesson] y descubrirás los siguientes pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/

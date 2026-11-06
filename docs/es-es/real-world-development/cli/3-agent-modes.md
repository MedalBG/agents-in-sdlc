---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para implementar el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto, pero los cambios mayores requieren un proceso más sólido. GitHub Copilot CLI está diseñado para trabajar con el flujo de tu organización y ayudarte a crear las cosas adecuadas de la forma correcta. En esta lección, la primera de varias dedicadas a un proceso de desarrollo habitual con agentes, partirás de una incidencia para crear una funcionalidad, comprobar que el código es válido y que se comporta como se espera, y finalmente combinarlo en el proyecto.

> [!NOTE]
> Seguirás usando la misma conversación y rama durante el flujo de trabajo de la funcionalidad. Normalmente se utilizarían ramas o PR distintas para los diferentes tipos de archivos, pero simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot a partir de una incidencia de GitHub.
- definirás los requisitos en modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás manualmente la funcionalidad en un navegador reenviado.

Mientras sigues desarrollando esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación MCP, crearás un agente QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan filtrar los juegos por categoría y editor. La incidencia del backlog describe la funcionalidad, pero es necesario acordar algunos detalles, como la combinación de categorías, antes de escribir código. Utilizarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia sus fundamentos; al contrario, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia registrada que describa el trabajo.
2. Planificar qué hay que desarrollar.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código cuando se haya revisado y se hayan superado las comprobaciones de integración continua.

> [!NOTE]
> Los detalles concretos dependen del equipo y de la organización, pero la mayoría de los procesos son variaciones de este flujo.

Seguir este enfoque ayuda a garantizar que el código generado por IA cumple los requisitos y pasa por el mismo proceso de validación que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente. Propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Parte de la incidencia

Antes de empezar a trabajar en el filtrado, vuelve al Codespace y comprueba que el repositorio y el terminal estén listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo.
2. Confirma que la PR de las valoraciones por estrellas se ha combinado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa dos veces <kbd>Tab</kbd> para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt empieza por `#7` (o un número parecido). El carácter `#` permite añadir al contexto una incidencia o solicitud de incorporación de cambios de GitHub.

## Planifica la funcionalidad de filtrado

La planificación permite definir el enfoque para implementar una funcionalidad o realizar una tarea antes de delegarla en Copilot. Es conveniente dedicar tiempo a planificar las tareas complejas. Cambia al modo Plan y pide a Copilot que cree el plan:

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador situado bajo el prompt muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque.

3. Puede que Copilot te pregunte cómo debería funcionar el filtrado. Responde según tus preferencias; no hay una única respuesta correcta.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que revise cualquier parte que no refleje tus decisiones.

## Aprueba Autopilot

Una vez redactado y revisado el plan, es el momento de implementarlo. Autopilot permite que Copilot itere hasta considerar que ha completado la tarea.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador de modo situado bajo el prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o conflictos de puertos, resuelve el problema de configuración antes de dar por terminadas las comprobaciones.

## Revisa y verifica la implementación

Como con cualquier otro código, debes revisar el código generado antes de combinarlo. Revisaremos el código y ejecutaremos el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en modo Interactive. Confirma que el indicador no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones de planificación.
4. Cuando termines de revisar el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los fallos.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editor, así como las combinaciones que acordaste en el plan.
4. Confirma que el restablecimiento y los resultados vacíos se comportan como se acordó.
5. Vuelve al Codespace y pide a Copilot que detenga el servidor de desarrollo.

## Resumen y siguientes pasos

Has utilizado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- iniciaste una conversación de Copilot a partir de una incidencia de GitHub.
- definiste los requisitos en modo Plan.
- implementaste la funcionalidad con Autopilot.
- revisaste el código.
- validaste manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizarás en cómo generar código de acuerdo con las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/

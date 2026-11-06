---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara el entorno. Crearás una copia propia del repositorio Tailspin Toys, pondrás en marcha un [Codespace][codespaces] y confirmarás que GitHub Copilot Chat funciona en el editor.

## Configura el repositorio del taller

Crearás una instancia a partir de la [plantilla][template-repository], que contiene todos los archivos necesarios para el taller.

1. En una nueva ventana del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. Selecciona **Use this template** en la página del repositorio y, después, **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si asistes a un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.

    ![Configuración del repositorio creado a partir de una plantilla](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio (**organization-or-user-name/repository-name**), porque la necesitarás más adelante.

> [!NOTE]
> **El backlog está preparado**
>
> Al crear el repositorio a partir de la plantilla, se generará automáticamente un backlog de incidencias de GitHub. Utilizarás estas incidencias durante el taller; no tienes que crear ninguna.

## Crea un Codespace

A continuación, utilizarás un Codespace para completar los ejercicios. [GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código en el navegador. Ofrece un IDE completo con compatibilidad para varios lenguajes, extensiones y herramientas.

1. Abre el repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

    ![Selecciona el botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un Codespace.

    ![Crear un Codespace](../../../_images/ex0-create-codespace.png)

La creación del Codespace tardará unos minutos, aunque será más rápida que instalar los servicios manualmente. Mientras tanto, puedes explorar otras funciones de GitHub Copilot.

> [!CAUTION]
> Volverás al Codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está diseñado para ejecutarse en Codespaces o en un [contenedor de desarrollo][dev-containers] local. Ambos entornos incluyen los requisitos necesarios. Si prefieres trabajar localmente, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite. VS Code creará el mismo contenedor de desarrollo que utiliza Codespaces.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Utiliza GitHub Copilot Chat y el modo agente

Para acceder al modo agente de GitHub Copilot Chat, necesitas tener instalada en el IDE la extensión GitHub Copilot Chat. Debería estar instalada si utilizas un Codespace.

> [!TIP]
> Si no tienes la extensión, puedes [instalarla desde Visual Studio Code Marketplace][copilot-chat-extension]. También puedes abrir la vista Extensions de Visual Studio Code, buscar **GitHub Copilot Chat** y seleccionar **Install**.

Es posible que tengas que autenticarte con tu cuenta de GitHub para habilitarla.

1. Vuelve al Codespace.
2. Si Copilot Chat no aparece a la derecha del editor, selecciona el icono **Copilot Chat** en la parte superior del Codespace.
3. Escribe un mensaje como «Hello world» en la ventana de Copilot Chat y pulsa <kbd>Enter</kbd>.
4. Si se te solicita, inicia sesión en tu cuenta de GitHub y sigue los pasos de autenticación.

    ![Ejemplo de una solicitud de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Después de autenticarte, verás la ventana de Copilot Chat.

## Resumen

¡Enhorabuena! Has creado una copia del repositorio del taller e iniciado la creación del Codespace que utilizarás para programar.

## Siguiente paso

Empieza a trabajar con Copilot en el [Ejercicio 1: Instrucciones personalizadas][next-lesson], donde aprenderás las convenciones del proyecto.

## Recursos

- [Información general sobre GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Introducción a Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/

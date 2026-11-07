---
title: "Lección 1: Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, familiarízate con la herramienta y busca la incidencia de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en el terminal. Te permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas desde la línea de comandos. Puedes delegar tareas, solicitar cambios y mantener la concentración. El primer paso, como imaginarás, es instalar la herramienta. Por suerte, puedes hacerlo con herramientas que ya conoces.

En esta lección:

- instalarás GitHub Copilot CLI con npm.
- te autenticarás con tu cuenta de GitHub.
- confiarás en el repositorio del taller y probarás una conversación breve.
- buscarás la incidencia de filtrado con el servidor MCP de GitHub integrado.

## Escenario

Tu equipo empieza a utilizar agentes de IA para abordar un backlog cada vez mayor. Copilot CLI lleva esa capacidad al terminal, donde ya trabajan muchos desarrolladores. En esta lección instalarás y autenticarás la herramienta para poder usarla durante el resto del taller.

## Instalar Copilot CLI

Puedes instalar Copilot CLI con [npm][install-cli], WinGet o Homebrew. GitHub Codespaces ya incluye Node.js, así que utilizarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js está instalado y cumple el requisito de versión:

   ```bash
   node --version
   ```

   Debe aparecer la versión 24 o una posterior.

3. Instala Copilot CLI de forma global:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Debe aparecer el número de versión.

## Autenticarse con GitHub

La primera vez que se inicia Copilot CLI, te pide que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones del código de dispositivo para autenticarte y autorizar Copilot CLI.
3. Copilot CLI muestra este mensaje:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Comprueba que la ruta corresponde a tu repositorio de Tailspin Toys y responde que sí seleccionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> Es posible que ya estés autenticado mediante tu sesión de GitHub en Codespaces. Si Copilot CLI se inicia sin pedirte que te autentiques, ya está listo.

## Familiarizarse con la herramienta

Los comandos escritos en el prompt habitual del terminal se ejecutan directamente en tu Codespace. Cuando se inicia Copilot CLI, el lenguaje natural se envía al agente y los comandos de barra controlan la conversación.

1. Escribe `/model`, utiliza las teclas de flecha para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos disponibles en la versión instalada y, después, pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla a Copilot para comprobar que todo funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Escribe `/mcp list` y confirma que está disponible el servidor MCP de GitHub integrado.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. La utilizarás después de realizar un primer cambio breve.

> [!TIP]
> Una sesión normal de Copilot CLI utiliza la rama que esté seleccionada en el terminal; no crea automáticamente un árbol de trabajo. Crearás una rama de funcionalidad antes de cada cambio.

## Usar el atajo del taller

Copilot CLI suele pedir permiso antes de utilizar herramientas que no tengan permisos establecidos. En este taller, lo volverás a iniciar con `--yolo`, un atajo aprobado por ti que elimina esas solicitudes de permiso dentro del Codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` activa todos los permisos automáticos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un Codespace o una máquina virtual; nunca lo configures como alias predeterminado para el desarrollo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para obtener más información.

En este taller, `--enable-all-github-mcp-tools` activa las herramientas MCP de lectura y escritura de GitHub que se utilizarán en lecciones posteriores para trabajar con incidencias y solicitudes de incorporación de cambios. El Codespace limita el acceso a tu equipo local, pero los recursos de GitHub autenticados siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarlo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta breve sobre el proyecto para confirmar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, cuando cambies las instrucciones o añadas un agente, utilizarás `copilot --resume` para volver a la misma conversación y rama de funcionalidad.

## Resumen y pasos siguientes

¡Enhorabuena! En esta lección:

- has instalado GitHub Copilot CLI con npm.
- te has autenticado con tu cuenta de GitHub.
- has confiado en el repositorio del taller y has probado una conversación breve.
- has encontrado la incidencia de filtrado con el servidor MCP de GitHub integrado.

A continuación, [realizarás tu primer cambio concreto][next-lesson] y utilizarás Copilot CLI para mostrar una valoración por estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use

---
title: "Ejercicio 2: Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones, como ejecutar pruebas. En el flujo habitual de desarrollo, localizas todos los archivos necesarios, realizas los cambios, ejecutas las pruebas, depuras, descubres qué archivo falta, haces otra actualización y así sucesivamente.

El modo agente de Copilot ayuda con este proceso.

El modo agente está diseñado para trabajar de forma más autónoma en el IDE. Se comporta como un desarrollador: explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los problemas que encuentra. Veamos cómo utilizarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite recibir comentarios rápidamente e interactuar con él. El agente en la nube está diseñado para trabajar de forma asíncrona como un miembro del equipo, mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios entre ellos.
- el modo agente de GitHub Copilot puede implementar funciones en la interfaz y la capa de datos.
- revisar los cambios y las pruebas que genera el modo agente de Copilot antes de combinarlos en el código base.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior ya creaste una función auxiliar para los editores. Ahora completarás el trabajo restante en la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y consultar sus detalles. Toda la aplicación está desarrollada con Astro y genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde el frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz del lado cliente. Las funciones reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se añade mediante un pequeño `<script>` de Astro con las API estándar del DOM.

### Iniciar el sitio web

El comando `npm run dev` inicia el único servidor de desarrollo de Astro. Para ejecutarlo en GitHub Codespaces:

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el Codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y selecciona una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir una ventana del navegador con `http://localhost:4321`, ciérralo seleccionando la **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost desde el terminal, se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al Codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Cuando el sitio esté en ejecución, puedes explorar sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como su título, descripción, editor y categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

El sitio ya funciona, pero queremos mejorarlo con nuevas capacidades. Empecemos revisando el backlog. Al crear el proyecto a partir de la plantilla se creó un backlog de incidencias automáticamente. Pídele a GitHub Copilot que lo muestre.

1. En el selector de agentes de la vista Chat, selecciona **Agent**. El agente puede planificar e implementar cambios en varios archivos de forma autónoma, ejecutar comandos del terminal y utilizar herramientas.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no recomiendan un modelo concreto. Al preparar el taller utilizamos Claude Sonnet 4.5 para que las sugerencias de código fueran relativamente coherentes y ofrecer una experiencia uniforme. Sin embargo, los modelos de lenguaje extensos son probabilísticos y es posible que las sugerencias difieran de las instrucciones del taller. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje, Copilot podría utilizar un comando MCP distinto, pero debería poder completar la tarea.

3. Envía este prompt a Copilot para preguntarle por las incidencias del backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que el comando enumere las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según lo que considera más útil para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedirle al agente que genere el código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En el Codespace, abre `.github/instructions/ui.instructions.md`.
2. Fíjate en las pautas generales para añadir funcionalidad, entre ellas:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones específicos para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a los archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar pautas, dividir instrucciones complejas en archivos más manejables y proporcionar ejemplos y plantillas.

## Implementar el filtrado

Para completar el filtrado, la aplicación necesita al menos tres cambios:

- añadir o ajustar la lógica de filtrado de la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar y superar las pruebas antes de combinar los cambios. El modo agente de Copilot puede realizar estas tareas. Añadamos la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o seleccionar **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de cómo seleccionar el archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la función descrita en la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la función, como las funciones auxiliares de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. Puede tardar unos minutos según la complejidad de la tarea y del código base. Durante el proceso quizá aparezcan los botones **Keep** y **Undo** en el editor. Al terminar, podrás conservar o deshacer todos los cambios. No es necesario seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de Copilot Chat para confirmar que quieres ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas y que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.

   ![Sesión completa de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y pasos siguientes

¡Enhorabuena! En este ejercicio has aprendido a utilizar el modo agente de GitHub Copilot para añadir funcionalidad al sitio de Tailspin Toys. Has visto cómo:

- el modo agente de GitHub Copilot implementa funciones en la interfaz y la capa de datos.
- el modo agente explora el proyecto, identifica los archivos pertinentes y coordina cambios entre ellos.
- revisar los cambios y las pruebas generados por el modo agente antes de combinarlos en el código base.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que aumenta la lista de juegos, será necesario habilitar la paginación. Utiliza lo aprendido en este ejercicio para pedir a Copilot que actualice el sitio. Al redactar el prompt:

- sigue las prácticas recomendadas y utiliza los archivos de instrucciones existentes.
- decide cómo debe funcionar la paginación, por ejemplo, si el usuario puede seleccionar el tamaño de página o si debe ser fijo.
- proporciona a Copilot las pautas necesarias para implementar el comportamiento que quieres.
- itera con GitHub Copilot, solicita cambios y aporta contexto cuando sea necesario. Es lo habitual al trabajar con Copilot.

## Recursos

- [Copilot: modos ask, edit y agent; qué hacen y cuándo se utilizan][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
